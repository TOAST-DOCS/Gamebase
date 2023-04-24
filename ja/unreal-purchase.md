## Game > Gamebase > Unreal SDK使用ガイド > 決済

ここではUnrealでアプリ内決済機能を使用するために必要な設定方法を説明します。
Gamebaseは、1つの統合された決済APIを提供し、ゲームから簡単に多くのストアのアプリ内決済を連携できるようにサポートします。

### Settings

AndroidまたはiOSでアプリ内決済機能を設定する方法は、次の文書を参照してください。<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### Unreal Plugin設定

> <font color="red">[注意]</font><br/>
>
> 外部プラグインで決済関連処理がある場合、 Gamebase決済機能が正常に動作しない可能性があります。

* Unrealでデフォルトで有効になっているOnline SubSystemプラグインを無効化またはストア機能を利用できないように変更する必要があります。
    * Online SubSystem GooglePlayプラグイン使用時 /Config/Android/AndroidEngine.iniファイルを編集します。
            
            [OnlineSubsystemGooglePlay.Store]
            bSupportsInAppPurchasing=False
            bUseGooglePlayBillingApiV2=False
            
    * Online SubSystem iOSプラグイン使用時 /Config/IOS/IOSEngine.iniファイルを編集します。
            
            [OnlineSubsystemIOS.Store]
            bSupportsInAppPurchasing=False
            

#### Android決済設定(エンジンバージョン4.24以下)

* Epic Games Launcherを通じて4.24以下のバージョンをインストールした場合、
    **Engine\Build\Android\Java\src\com\android\vending\billing\IInAppBillingService.aidl**を削除すると正常にビルドできます。
    * [IInAppBillingService.aidl](https://developer.android.com/google/play/billing/api)ファイルはGamebaseで提供しており、衝突が発生するので除去する必要があります。
    * エンジン4.25以上のバージョンや、githubからエンジンをダウンロードした場合は、別途除去する必要がありません。

###  Purchase Flow


アイテムの購入は大きく分けて決済フロー、消費フロー、再処理フローの3つがあります。
決済フローは、次のような順序で実装してください。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 以前の決済が正常に終了せず、再処理が動作しない場合、決済が失敗します。そのため決済前に**RequestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合はConsume Flowを進行します。
2. ゲームクライアントではGamebase SDKの**RequestPurchase**を呼び出して決済を試行します。
3. 決済が成功すると**RequestItemListOfNotConsumed**を呼び出して未消費決済履歴を確認した後、支給するアイテムが存在場合、Consume Flowを進行します。

### Consume Flow

未消費決済履歴リストに値がある場合、次のような順序でConsume Flowを進行してください。

> <font color="red">[注意]</font><br/>
>
> アイテムの重複支給が発生しないように、ゲームサーバーで必ず重複支給の有無をチェックしてください。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID, gamebaseProductId, paymentSeq, purchaseTokenを伝達します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1. まだアイテムを支給していなければGamebaseサーバーのPayment Transaction APIを呼び出ししてpaymentSeq、purchaseToken値が有効か検証します。
        * [Game > Gamebase > APIガイド > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. purchaseTokenが正常な値の場合はUserIDにgamebaseProductIdに該当するアイテムを支給します。
    * 2-3. アイテム支給後、ゲームDBにUserID、gamebaseProductId、paymentSeq、purchaseTokenを保存して重複支給防止または再支給ができるようにします。
3. アイテム支給有無に関係なく、ゲームサーバーはGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
    * [Game > Gamebase > APIガイド > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* ストア決済には成功したがエラーが発生して正常に終了しなかった場合があります。
* **RequestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合、Consume Flowを進行してください。
* 再処理は次の時点で呼び出すことを推奨します。
    * ログイン完了後
    * 決済前
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時

### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。
ゲームユーザーが購入をキャンセルする場合、**PURCHASE_USER_CANCELED**エラーが返されます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestPurchase(const FString& gamebaseProductId, const FGamebasePurchasableReceiptDelegate& onCallback);
void RequestPurchase(const FString& gamebaseProductId, const FString& payload, const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestPurchase(const FString& gamebaseProductId)
{
    IGamebase::Get().GetPurchase().RequestPurchase(gamebaseProductId, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *purchasableReceipt->gamebaseProductId, purchasableReceipt->price, *purchasableReceipt->currency,
                *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the error code and handle the error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }

        }
    }));
}

void Sample::RequestPurchaseWithPayload(const FString& gamebaseProductId)
{
    FString userPayload = TEXT("{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}");
    
    IGamebase::Get().GetPurchase().RequestPurchase(gamebaseProductId, userPayload, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *purchasableReceipt->gamebaseProductId, purchasableReceipt->price, *purchasableReceipt->currency,
                *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);

            FString payload = purchasableReceipt->payload;
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the error code and handle the error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }
        }
    }));
}
```

**VO**

```cpp
USTRUCT()
struct FGamebasePurchasableReceipt
{
    GENERATED_USTRUCT_BODY()
    
    // 購入したアイテムの商品IDです。
    UPROPERTY()
    FString gamebaseProductId;

    // itemSeqで商品を購入するLegacy API用の識別子です。
    UPROPERTY()
    int64 itemSeq;

    // 購入した商品の価格です。
    UPROPERTY()
    float price;

    // 通貨コードです。
    UPROPERTY()
    FString currency;

    // 決済識別子です。
    // purchaseTokenと一緒に「Consume」サーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIはゲームサーバーで呼び出してください！
    UPROPERTY()
    FString paymentSeq;

    // 決済識別子です。
    // paymentSeqと一緒に「Consume」サーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume APIでは「accessToken」という名前のパラメータで伝達する必要があります。
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIはゲームサーバーで呼び出してください！
    UPROPERTY()
    FString purchaseToken;

    // Google、Appleなどのストアコンソールに登録された商品IDです。
    UPROPERTY()
    FString marketItemId;

    // 次のような商品タイプがあります。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTO_RENEWABLE：サブスクリプション型の商品。
    // * CONSUMABLE_AUTO_RENEWABLE：サブスクリプション型の商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使用される「消費が可能なサブスクリプション商品」。
    UPROPERTY()
    FString productType;

    // 商品を購入したUser ID
    // 商品を購入していないUser IDでログインした場合、購入したアイテムを獲得できません。
    UPROPERTY()
    FString userId;

    // ストアの決済識別子です。
    UPROPERTY()
    FString paymentId;

    // サブスクリプション商品は更新するごとにpaymentIdが変更されます。
    // このフィールドは、初めてサブスクリプション商品を決済した時のpaymentIdを伝えます。
    // ストアや、決済サーバーの状態によって値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    UPROPERTY()
    FString originalPaymentId;
    
    // 商品を購入した時刻です。(epoch time)
    UPROPERTY()
    int64 purchaseTime;
    
    // 購読が終了する時刻です。(epoch time)
    UPROPERTY()
    int64 expiryTime;

    // 決済したストアコードです。
    // GamebaseStoreCodeクラスでストアコードリストを確認できます。
    UPROPERTY()
    FString storeCode;
    
    // RequestPurchase API呼び出し時、payloadに渡された値です。
    // ストアサーバー状態によって情報が流出する場合があるため、使用を推奨しません。
    UPROPERTY()
    FString payload;
    // プロモーション決済かどうか
    // - (Android) Gamebase決済サーバーで一時的に検証ロジックを切る場合は、常にfalseと出力されるため、有効な値が保障されません。
    UPROPERTY()
    bool isPromotion;
    // テスト決済かどうか
    // - (Android) Gamebase決済サーバーで一時的に検証ロジックを切る場合は、常にfalseと出力されるため、有効な値が保障されません。
    UPROPERTY()
    bool isTestPurchase;    
};
```


### List Purchasable Items

アイテムリストを照会するには、次のAPIを呼び出します。 
コールバックで返されるリスト内には、各アイテムの情報が含まれています。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestItemListPurchasable(const FGamebasePurchasableItemListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestItemListPurchasable()
{
    IGamebase::Get().GetPurchase().RequestItemListPurchasable(FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* purchasableItemList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *purchasableItem.gamebaseProductId, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable failed. (error: %d)"), error->code);
        }
    }));
}
```

**VO**

```cpp
USTRUCT()
struct FGamebasePurchasableItem
{
    GENERATED_USTRUCT_BODY()
    
    // Gamebaseコンソールに登録された商品IDです。
    // Gamebase.Purchase.requestPurchase APIで商品を購入する時に使用されます。
    UPROPERTY()
    FString gamebaseProductId;

    // itemSeqで商品を購入するLegacy API用の識別子です。
    UPROPERTY()
    int64 itemSeq;

    // 商品の価格です。
    UPROPERTY()
    float price;

    // 通貨コードです。
    UPROPERTY()
    FString currency;

    // Gamebaseコンソールに登録された商品名です。
    UPROPERTY()
    FString itemName;

    // Google、Appleなどのストアコンソールに登録された商品IDです。
    UPROPERTY()
    FString marketItemId;

    // 次のような商品タイプがあります。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTORENEWABLE：サブスクリプション型の商品。
    // * CONSUMABLE_AUTO_RENEWABLE：サブスクリプション型の商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使用される「消費が可能なサブスクリプション商品」。
    UPROPERTY()
    FString productType;
    
    // 通貨記号が含まれたローカライズされた価格情報です。
    UPROPERTY()
    FString localizedPrice;
    
    // ストアコンソールに登録されたローカライズされた商品名です。
    UPROPERTY()
    FString localizedTitle;

    // ストアコンソールに登録されたローカライズされた商品説明です。
    UPROPERTY()
    FString localizedDescription;
    
    // Gamebaseコンソールで該当商品の「使用状態」を表します。
    UPROPERTY()
    bool isActive;
};
```


### Get a List of Non-Consumed Items

アイテムを購入したが、正常にアイテムが消費(配送、支給)されていない未消費決済内訳をリクエストします。
未決済内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストして、アイテムを配送(支給)するように処理する必要があります。

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | 同じUserIDにて他のストアで購入した未消費履歴も返します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestItemListOfNotConsumed(bool allStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.allStores = allStores;

    IGamebase::Get().GetPurchase().RequestItemListOfNotConsumed(Configuration, FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* purchasableItemList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed failed. (error: %d)"), error->code);
        }
    }));
}
```

### Get the List of Actived Subscriptions

現在のユーザーID基準で有効になっている購読リストを照会します。
決済が完了した購読商品(自動更新型購読、自動更新型消費性購読商品)は、有効期限が切れる前まで照会できます。
ユーザーIDが同じ場合、AndroidとiOSで購入した購読商品が照会されます。

> <font color="red">[注意]</font><br/>
>
> Androidでは現在、Google Playストアでのみサブスクリプション商品をサポートしています。

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | 同じUserIDにて他のストアで購入した未消費履歴も返します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestActivatedPurchases(bool allStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.allStores = allStores;

    IGamebase::Get().GetPurchase().RequestActivatedPurchases(Configuration, FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableReceipt>* purchasableReceiptList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases failed. (error: %d)"), error->code);
        }
    }));
}
```

### List Subscriptions Status

現在ユーザーID基準でサブスクリプション商品の状態を照会します。
コールバックで返されるリスト内にはサブスクリプション商品の情報が含まれています。

> <font color="red">[注意]</font><br/>
>
> * 以下のガイドに従って購読イベントを設定すると購読ステータスコードが正常に返されます。
>     * [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド > Googleシステム内リアルタイム購読情報イベント配信設定](./console-google-guide/#google_1)
>     * イベント設定を行っていない状態で購入したサブスクリプション商品のステータスコードは常に0(PURCHASED)が返されます。
> * 現在サブスクリプション商品はGoogle Playストアのみサポートします。

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                 |
| ------------------------------- | -------------------------- | ----------------------------------------------------------- |
|  includeExpiredSubscriptions    | O                          | 期限切れのサブスクリプション商品まで含めて照会します。<br/>デフォルト値は**false**です。   |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestSubscriptionsStatus(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableSubscriptionStatusDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestSubscriptionsStatus(bool includeExpiredSubscriptions)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.allStores = allStores;
    IGamebase::Get().GetPurchase().RequestSubscriptionsStatus(Configuration, FGamebasePurchasableSubscriptionStatusDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableSubscriptionStatus>* purchasableReceiptList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestSubscriptionsStatus succeeded."));
            for (const FGamebasePurchasableSubscriptionStatus& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestSubscriptionsStatus failed. (error: %d)"), error->code);
        }
    }));
}
```

**VO**
```cpp
USTRUCT()
struct GAMEBASE_API FGamebasePurchasableSubscriptionStatus
{
    GENERATED_USTRUCT_BODY()
    // アプリがインストールされたストアについてGamebaseで内部的に定義したコードです。
    UPROPERTY()
    FString storeCode;
    
    // ストアの決済識別子です。
    UPROPERTY()
    FString paymentId;
    // サブスクリプション商品は更新さえる度にpaymentIdが変更されます。
    // このフィールドはサブスクリプション商品を最初に決済した時のpaymentIdを示します。
    // ストアおよび決済サーバーの状態によって値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    UPROPERTY()
    FString originalPaymentId;
    // 決済識別子です。
    // purchaseTokenと一緒にConsumeサーバーAPIを呼び出すために使用する重要な情報です。
    //    
    // 注意：Consume APIはゲームサーバーで呼び出してください！ (https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap)
    UPROPERTY()
    FString paymentSeq;
    // 購入商品の商品IDです。
    UPROPERTY()
    FString marketItemId;
    
    // IAP Webコンソールの項目固有識別子
    UPROPERTY()
    int64 itemSeq;
    // 次のいずれかの値を持ちます。
    // * UNKNOWN：不明なタイプです。Gamebase SDKをアップデートするか、Gamebaseサポートにお問い合わせください。
    // * CONSUMABLE：消耗品です。
    // * AUTO_RENEWABLE：サブスクリプション商品です。
    UPROPERTY()
    FString productType;
    // 商品を購入したユーザーIDです。
    // 商品購入に使用していないユーザーIDでログインすると、購入した商品を受け取ることができません。
    UPROPERTY()
    FString userId;
    
    // 商品の価格です。
    UPROPERTY()
    float price;
    // 通貨情報です。
    UPROPERTY()
    FString currency;
    // Payment識別子。
    // paymentSeqで'Consume'サーバーAPIを呼び出すために使われる重要な情報です。
    // Consume APIで引数名を'accessToken'に指定する必要があります。
    // 参考: https://docs.toast.com/ko/Game/Gamebase/ko/api-guide/#purchase-iap
    UPROPERTY()
    FString purchaseToken;
    // この値はGoogleで購入する時に使用され、次の値を持つことができます。
    // ただし、GoogleサーバーのエラーによりGamebase決済サーバーで一時的に認証ロジックが無効になっている場合、
    // nullのみを返すため、常に有効な値を保証できない場合があります。
    // * null：正常決済
    // * テスト：テスト決済
    // * プロモーション：プロモーション決済
    UPROPERTY()
    FString purchaseType;
    // 商品を購入した時間。(epoch time)
    UPROPERTY()
    int64 purchaseTime;
    
    // 購読の期限が切れる時間。(epoch time)
    UPROPERTY()
    int64 expiryTime;
    
    // RequestPurchase APIの呼び出し時にペイロードに渡される値です。
    // ストアサーバー状態によって情報が流出する場合があるため、使用を推奨しません。
    UPROPERTY()
    FString payload;
    
    // 購読状態
    // 全体ステータスコードは次の文書を参照してください。
    // - https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatus
    UPROPERTY()
    int32 statusCode;
    
    // 購読状態の説明です。
    UPROPERTY()
    FString statusDescription;
    
    // Gamebaseコンソールに登録された商品IDです。
    // RequestPurchase APIで商品を購入する時に使用されます。
    UPROPERTY()
    FString gamebaseProductId;
};
```


### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchaseモジュールが初期化されませんでした。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したかを確認してください。 |
| PURCHASE_USER_CANCELED                    | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックがまだ完了していない状態でAPIが呼び出されました。 |
| PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 該当ストアのキャッシュが不足しているため決済できません。              |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 該当商品が有効な状態ではありません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 月購入限度を超過しました。             |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | サポートしないストアです。<br>選択できるストアはAS(App Store)、GG(Google)、ONESTORE、GALAXYです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAPライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。
* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud IAPライブラリでエラーが発生した時に返されます。
* NHN Cloud IAPライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* NHN Cloud IAPのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Unity > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
