## Game > Gamebase > Unreal SDK使用ガイド > 決済

ここではUnrealでアプリ内決済機能を使用するために必要な設定方法を説明します。
Gamebaseは、1つの統合された決済APIを提供し、ゲームから簡単に多くのストアのアプリ内決済を連携できるようにサポートします。

### Settings

> <font color="red">[注意]</font><br/>
>
> Android ONE Storeはv17のみをサポートします。
> Android ONE Store v19はサポートを検討中で、現在はサポートしていません。

AndroidまたはiOSでアプリ内決済機能を設定する方法は、次の文書を参照してください。<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### Android決済設定(エンジンバージョン4.24以下)

* Epic Games Launcherを通して4.24バージョンをインストールした場合、
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

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID、itemSeq、paymentSeq、purchaseTokenを伝達します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeq、purchaseTokenでアイテムを支給した履歴があるかを確認します。
    * 2-1まだアイテムを支給していない場合、UserIDにitemSeqに該当するアイテムを支給します。
    * 2-2アイテム支給後、ゲームDBにUserID、itemSeq、paymentSeq、purchaseTokenを保存し、重複支給の有無を確認できるようにします。
3. ゲームサーバーはGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
    * [APIガイド > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

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

// Legacy API
void RequestPurchase(int64 itemSeq, const FGamebasePurchasableReceiptDelegate& onCallback);
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
    // Gamebase.Purchase.requestPurchase APIの呼び出し時にpayloadに伝達した値です。
    //
    // このフィールドは、例えば同じUser IDで購入したがゲームチャンネル、キャラクターなどに応じて
    // 商品購入および支給を区分したい場合など
    // ゲームで必要とするさまざまな追加情報を入れる目的で活用できます。
    UPROPERTY()
    FString payload;
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

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestItemListOfNotConsumed()
{
    IGamebase::Get().GetPurchase().RequestItemListOfNotConsumed(FGamebasePurchasableItemListDelegate::CreateLambda(
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
> 現在の購読商品は、Androidの場合Google Playストアのみサポートします。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestActivatedPurchases()
{
    IGamebase::Get().GetPurchase().RequestActivatedPurchases(FGamebasePurchasableReceiptListDelegate::CreateLambda(
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
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | IAPライブラリエラーです。<br>DetailCodeを確認してください。   |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。
* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* エラーコードを確認する方法は次のとおりです。

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

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Unity > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
