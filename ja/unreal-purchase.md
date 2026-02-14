## Game > Gamebase > Unreal SDK使用ガイド > 決済

ここではUnrealでアプリ内決済機能を使用するために必要な設定方法を説明します。
Gamebaseは、1つの統合された決済APIを提供し、ゲームから簡単に多くのストアのアプリ内決済を連携できるようにサポートします。

### Settings

AndroidまたはiOSでアプリ内決済機能を設定する方法は、次の文書を参照してください。<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)
* [Windows Purchase Settings](unreal-started/#windows-settings)

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

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID、paymentSeq、purchaseTokenを渡します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1. まだアイテムを支給していなければGamebaseサーバーのPayment Transaction APIを呼び出してpurchaseTokenが有効か、レスポンスフィールドのpaymentSeqと一致するか検証します。
        * [Game > Gamebase > APIガイド > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseTokenがサーバーAPIガイド文書の**accessToken**に該当します。
    * 2-2. GamebaseProductIdはサーバーのPayment Transaction APIのレスポンスフィールドで確認できます。
        * クライアントの未消費決済履歴リストにもGamebaseProductIdが存在しますが、再処理時にはその値がない場合もありますので、サーバーのPayment Transaction APIから取得したGamebaseProductIdの値を使用してください。
    * 2-3. Payment Transaction APIの呼び出しが成功し、purchaseTokenが正常であることが確認されると、UserIDにGamebaseProductIdに該当するアイテムを支給します。
    * 2-4. アイテム支給後、ゲームDBにUserID、GamebaseProductId、paymentSeq、purchaseTokenを保存して重複支給防止または再支給ができるようにします。
3. アイテム支給有無に関係なく、ゲームサーバーは未消費履歴が返されないようにGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
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

느린 결제나 부모 동의와 같이 결제 완료를 기다려야 하는 상황이 발생하는 경우에는 **PURCHASE_PENDING** 오류가 반환됩니다.
이후에 결제가 정상적으로 완료되는 경우, GamebaseEventHandler에서 결제 완료 이벤트를 수신할 수 있습니다.
[Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Gamebase Event Handler](./unreal-etc/#purchase-updated)

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestPurchase(const FString& GamebaseProductId, const FGamebasePurchasableReceiptDelegate& Callback);
void RequestPurchase(const FString& GamebaseProductId, const FString& payload, const FGamebasePurchasableReceiptDelegate& Callback);
```

**Example**
```cpp
void USample::RequestPurchase(const FString& GamebaseProductId)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestPurchase(GamebaseProductId, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* PurchasableReceipt, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->Price, *PurchasableReceipt->Currency,
                *PurchasableReceipt->PaymentSeq, *PurchasableReceipt->PurchaseToken);
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(LogTemp, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(LogTemp, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
            }

        }
    }));
}

void USample::RequestPurchaseWithPayload(const FString& GamebaseProductId)
{
    FString UserPayload = TEXT("{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}");
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestPurchase(GamebaseProductId, UserPayload, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* PurchasableReceipt, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {

            UE_LOG(LogTemp, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->price, *PurchasableReceipt->Currency,
                *PurchasableReceipt->PaymentSeq, *PurchasableReceipt->PurchaseToken);

            FString payload = PurchasableReceipt->payload;
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(LogTemp, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(LogTemp, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
            }
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableReceipt
{   
    // 購入したアイテムの商品IDです。
    FString GamebaseProductId;

    // itemSeqで商品を購入するLegacy API用の識別子です。
    int64 ItemSeq;

    // 購入した商品の価格です。
    float Price;

    // 通貨コードです。
    FString Currency;

    // 決済識別子です。
    // purchaseTokenと一緒に「Consume」サーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIはゲームサーバーで呼び出してください！
    FString PaymentSeq;

    // 決済識別子です。
    // paymentSeqと一緒に「Consume」サーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume APIでは「accessToken」という名前のパラメータで伝達する必要があります。
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIはゲームサーバーで呼び出してください！
    FString PurchaseToken;

    // Google、Appleなどのストアコンソールに登録された商品IDです。
    FString MarketItemId;

    // 次のような商品タイプがあります。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTO_RENEWABLE：サブスクリプション型の商品。
    // * CONSUMABLE_AUTO_RENEWABLE：サブスクリプション型の商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使用される「消費が可能なサブスクリプション商品」。
    FString ProductType;

    // 商品を購入したUser ID
    // 商品を購入していないUser IDでログインした場合、購入したアイテムを獲得できません。
    FString UserId;

    // ストアの決済識別子です。
    FString PaymentId;

    // サブスクリプション商品は更新するごとにpaymentIdが変更されます。
    // このフィールドは、初めてサブスクリプション商品を決済した時のpaymentIdを伝えます。
    // ストアや、決済サーバーの状態によって値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    FString OriginalPaymentId;
    
    // 商品を購入した時刻です。(epoch time)
    int64 PurchaseTime;
    
    // 購読が終了する時刻です。(epoch time)
    int64 ExpiryTime;

    // 決済したストアコードです。
    // GamebaseStoreCodeクラスでストアコードリストを確認できます。
    FString StoreCode;
    
    // RequestPurchase API呼び出し時、payloadに渡された値です。
    // ストアサーバー状態によって情報が流出する場合があるため、使用を推奨しません。
    FString Payload;
    // プロモーション決済かどうか
    // - (Android) Gamebase決済サーバーで一時的に検証ロジックを切る場合は、常にfalseと出力されるため、有効な値が保障されません。
    bool bIsPromotion;
    // テスト決済かどうか
    // - (Android) Gamebase決済サーバーで一時的に検証ロジックを切る場合は、常にfalseと出力されるため、有効な値が保障されません。
    bool bIsTestPurchase;    
};
```


### List Purchasable Items

アイテムリストを照会するには、次のAPIを呼び出します。 
コールバックで返されるリスト内には、各アイテムの情報が含まれています。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestItemListPurchasable(const FGamebasePurchasableItemListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestItemListPurchasable()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestItemListPurchasable(FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* PurchasableItemList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& PurchasableItem : *PurchasableItemList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *PurchasableItem.GamebaseProductId, PurchasableItem.Price, *PurchasableItem.Currency, *PurchasableItem.ItemName, *PurchasableItem.MarketItemId);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListPurchasable failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableItem
{
    // Gamebaseコンソールに登録された商品IDです。
    // Gamebase.Purchase.requestPurchase APIで商品を購入する時に使用されます。
    FString GamebaseProductId;

    // itemSeqで商品を購入するLegacy API用の識別子です。
    int64 ItemSeq;

    // 商品の価格です。
    float Price;

    // 通貨コードです。
    FString Currency;

    // Gamebaseコンソールに登録された商品名です。
    FString ItemName;

    // Google、Appleなどのストアコンソールに登録された商品IDです。
    FString MarketItemId;

    // 次のような商品タイプがあります。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTORENEWABLE：サブスクリプション型の商品。
    // * CONSUMABLE_AUTO_RENEWABLE：サブスクリプション型の商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使用される「消費が可能なサブスクリプション商品」。
    FString ProductType;
    
    // 通貨記号が含まれたローカライズされた価格情報です。
    FString LocalizedPrice;
    
    // ストアコンソールに登録されたローカライズされた商品名です。
    FString LocalizedTitle;

    // ストアコンソールに登録されたローカライズされた商品説明です。
    FString LocalizedDescription;
    
    // Gamebaseコンソールで該当商品の「使用状態」を表します。
    bool bIsActive;
};
```


### Get a List of Non-Consumed Items

アイテムを購入したが、正常にアイテムが消費(配送、支給)されていない未消費決済内訳をリクエストします。
未決済内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストして、アイテムを配送(支給)するように処理する必要があります。

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| bAllStores                       | O                          | 同じUserIDにて他のストアで購入した未消費履歴も返します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestItemListOfNotConsumed(bool bAllStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestItemListOfNotConsumed(Configuration, FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* PurchasableItemList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(LogTemp, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableReceipt& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListOfNotConsumed failed. (Error: %d)"), Error->Code);
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
| bAllStores                       | O                          | 同じUserIDにて他のストアで購入した未消費履歴も返します。<br/>デフォルト値は**false**です。 |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestActivatedPurchases(bool bAllStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestActivatedPurchases(Configuration, FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableReceipt>* purchasableReceiptList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestActivatedPurchases failed. (Error: %d)"), Error->Code);
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
|  bIncludeExpiredSubscriptions    | O                          | 期限切れのサブスクリプション商品まで含めて照会します。<br/>デフォルト値は**false**です。   |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestSubscriptionsStatus(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableSubscriptionStatusDelegate& Callback);
```

**Example**
```cpp
void USample::RequestSubscriptionsStatus(bool bIncludeExpiredSubscriptions)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestSubscriptionsStatus(Configuration, FGamebasePurchasableSubscriptionStatusDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableSubscriptionStatus>* purchasableReceiptList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestSubscriptionsStatus succeeded."));

            for (const FGamebasePurchasableSubscriptionStatus& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestSubscriptionsStatus failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

**VO**
```cpp
struct FGamebasePurchasableSubscriptionStatus
{
    // アプリがインストールされたストアについてGamebaseで内部的に定義したコードです。
    FString StoreCode;
    
    // ストアの決済識別子です。
    FString PaymentId;
    // サブスクリプション商品は更新さえる度にpaymentIdが変更されます。
    // このフィールドはサブスクリプション商品を最初に決済した時のpaymentIdを示します。
    // ストアおよび決済サーバーの状態によって値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    FString OriginalPaymentId;
    // 決済識別子です。
    // purchaseTokenと一緒にConsumeサーバーAPIを呼び出すために使用する重要な情報です。
    //    
    // 注意：Consume APIはゲームサーバーで呼び出してください！ (https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap)
    FString PaymentSeq;
    // 購入商品の商品IDです。
    FString MarketItemId;
    
    // IAP Webコンソールの項目固有識別子
    int64 ItemSeq;
    // 次のいずれかの値を持ちます。
    // * UNKNOWN：不明なタイプです。Gamebase SDKをアップデートするか、Gamebaseサポートにお問い合わせください。
    // * CONSUMABLE：消耗品です。
    // * AUTO_RENEWABLE：サブスクリプション商品です。
    FString ProductType;
    // 商品を購入したユーザーIDです。
    // 商品購入に使用していないユーザーIDでログインすると、購入した商品を受け取ることができません。
    FString UserId;
    
    // 商品の価格です。
    float Price;
    // 通貨情報です。
    FString Currency;
    // Payment識別子。
    // paymentSeqで'Consume'サーバーAPIを呼び出すために使われる重要な情報です。
    // Consume APIで引数名を'accessToken'に指定する必要があります。
    // 参考: https://docs.toast.com/ko/Game/Gamebase/ko/api-guide/#purchase-iap
    FString PurchaseToken;
    // 商品を購入した時間。(epoch time)
    int64 PurchaseTime;
    
    // 購読の期限が切れる時間。(epoch time)
    int64 ExpiryTime;
    
    // RequestPurchase APIの呼び出し時にペイロードに渡される値です。
    // ストアサーバー状態によって情報が流出する場合があるため、使用を推奨しません。
    FString Payload;
    
    // 購読状態
    // 全体ステータスコードは次の文書を参照してください。
    // - https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatus
    int32 StatusCode;
    
    // 購読状態の説明です。
    FString StatusDescription;
    
    // Gamebaseコンソールに登録された商品IDです。
    // RequestPurchase APIで商品を購入する時に使用されます。
    FString GamebaseProductId;

    // この値はGoogleで購入する際に使用され、次の値を持つことができます。
    // ただし、GoogleサーバーのエラーによりGamebase決済サーバーで一時的に認証ロジックが無効になった場合
    // nullのみを返すため、常に有効な値を保証するわけではありません。
    // * null:正常決済
    // * テスト:テスト決済
    // * プロモーション:プロモーション決済
    FString PurchaseType;
};
```

### Event by Purchase

Promotion 코드 입력을 통해 상품을 획득한 경우 또는 Pending 결제(느린 결제, 부모 동의 등)가 완료되었을 때 GamebaseEventHandler를 통해 이벤트를 받아 처리할 수 있습니다.
GamebaseEventHandler로 프로모션 결제 및 지연 결제 이벤트를 처리하는 방법은 아래 가이드를 확인하세요.
[Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Gamebase Event Handler](./unreal-etc/#purchase-updated)

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

> <font color="red">[주의]</font><br/>
>
> iOS 프로모션 결제를 위해서는 반드시 아래 가이드를 따라 설정하세요.
> [Game > Gamebase > iOS SDK 사용 가이드 > 결제 > Event by Purchase](./ios-purchase/#event-by-purchase)

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
| PURCHASE_PENDING                          | 4008       | 결제를 완료하려면 추가 확인이 필요합니다. |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | サポートしないストアです。<br>選択できるストアはAS(App Store)、GG(Google)、ONESTORE、GALAXYです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAPライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。
* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud IAPライブラリでエラーが発生した時に返されます。
* NHN Cloud IAPライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(LogTemp, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Message);

    GamebaseInnerError* moduleError = gamebaseError.Error; // GamebaseError.Error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(LogTemp, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleerror->Code, *moduleerror->Message);
    }
}
```

* NHN Cloud IAPのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Unity > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
