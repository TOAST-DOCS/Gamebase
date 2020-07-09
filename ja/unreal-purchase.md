## Game > Gamebase > Unreal SDK使用ガイド > 決済

ここではUnrealでアプリ内決済機能を使用するために必要な設定方法を説明します。
Gamebaseは、1つの統合された決済APIを提供し、ゲームから簡単に多くのストアのアプリ内決済を連携できるようにサポートします。

### Settings

AndroidまたはiOSでアプリ内決済機能を設定する方法は、次の文書を参照してください。<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### Android決済設定(エンジンバージョン4.24以下)

* Epic Games Launcherを通して4.24バージョンをインストールした場合、
    **Engine\Build\Android\Java\src\com\android\vending\billing\IInAppBillingService.aidl**を削除すると正常にビルドできます。
    * [IInAppBillingService.aidl](https://developer.android.com/google/play/billing/api)ファイルはGamebaseで提供しており、衝突が発生するので除去する必要があります。
    * エンジン4.25以上のバージョンや、githubからエンジンをダウンロードした場合は、別途除去する必要がありません。

###  Purchase Flow

アイテムの購入は、次の順序で実装してください。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.6.2.png)


1. ゲームクライアントではGamebase SDKの **RequestPurchase**を呼び出して決済を試行します。
2. 決済が成功すると**RequestItemListOfNotConsumed**を呼び出して未消費決済内訳を確認します。
3. 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
	* UserID、itemSeq、paymentSeq、purchaseTokenを伝達します。
4. ゲームサーバーは、ゲームDBにすでに同じpaymentSeq、purchaseTokenへアイテムを支給した履歴があるかを確認します。
	* 4-1. まだアイテムを支給していない場合、UserIDにitemSeqに該当するアイテムを支給します。
    * 4-2. アイテム支給後、ゲームDBにUserID、itemSeq、paymentSeq、purchaseTokenを保存して、後で重複支給を確認できるようにします。
5. ゲームサーバーは、APIを通してGamebaseサーバーにconsume(消費) APIをリクエストします。
	* [APIガイド](./api-guide/#consume)

<br/>

* ストア決済には成功したがエラーが発生して正常終了できない場合があります。ログイン完了後、未消費決済内訳を確認してください。
	* ログインに成功すると、**RequestItemListOfNotConsumed**を呼び出して未消費決済内訳を確認します。
	* 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを支給します。

### Purchase Item

購入するアイテムのitemSeqを利用して、次のAPIを呼び出して購入をリクエストします。
ゲームユーザーが購入をキャンセルする場合 **PURCHASE_USER_CANCELED**エラーが返されます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestPurchase(int64 itemSeq, const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestPurchaseSample(int64 itemSeq)
{
    IGamebase::Get().GetPurchase().RequestPurchase(itemSeq, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                purchasableReceipt->itemSeq, purchasableReceipt->price, *purchasableReceipt->currency, *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }
            
        }
    }));
}
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    purchasableItem.itemSeq, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable failed. (error: %d)"), error->code);
        }
    }));
}
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    purchasableItem.itemSeq, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    purchasableReceipt.itemSeq, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases failed. (error: %d)"), error->code);
        }
    }));
}
```

### App Store Promotion IAP

App Storeアプリ内でアイテムを購入できる機能を提供します。
アイテム購入成功後、下記の登録しておいたハンドラを通して、アイテムを支給できます。

プロモーションIAPは、App Store Connectで別途の設定が完了していると表示できます。

> <font color="red">[注意]</font><br/>
>
> iOS 11以上でのみ使用できます。
> Xcode 9.0以上でビルドする必要があります。
> Gamebase 1.13.0以上でサポートします。 (TOAST IAP SDK 1.6.0以上適用)


> <font color="red">[注意]</font><br/>
>
> ログイン成功後にのみ呼び出せます。
> ログイン成功後、他の決済APIより先に実行される必要があります。

> <font color="red">[注意]</font><br/>
>
> Facebook SDK使用時、App Dashboard > Settings > Basicページで`Log In-App Events Automatically (Recommended)`設定を有効にすると、該当機能が正常に作動しないので注意してください。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void SetPromotionIAPHandler(const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::SetPromotionIAPHandler()
{
    IGamebase::Get().GetPurchase().SetPromotionIAPHandler(FGamebasePurchasableReceiptDelegate::CreateLambda([](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("SetPromotionIAPHandler succeeded. (itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                purchasableReceipt->itemSeq, purchasableReceipt->price, *purchasableReceipt->currency, *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("SetPromotionIAPHandler failed. (error: %d)"), error->code);
            }
        }
    }));
}
```
**Overview**

* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f

**How to Test AppStore Promotion IAP**

> `注意`
> App Store Connectにアプリをアップロードした後、TestFlightを通してアプリをインストールして、テストを進行できます。
> 

1. TestFlightでAppをインストールします。
2. 下記のようにURL Schemeを呼び出して、テストを進行します。

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 固定値 |
| host &amp; path | なし | なし |
| queries | action | purchaseIntent |
|		  | bundleId | アプリのbundeld identifier |
|		  | productIdentifier | 購入アイテムのproduct identifier |

例) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されませんでした。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したかを確認してください。 |
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003      | 購入ロジックがまだ完了していない状態で APIが呼び出されました。 |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 該当ストアのキャッシュが不足しているため決済できません。              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | サポートしないストアです。<br>選択可能なストアはGG(Google)、ONESTOREです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリエラーです。<br>DetailCodeを確認してください。   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* エラーコードを確認する方法は次のとおりです。

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

* IAPエラーコードは、次の文書を参照してください。
    * [TOAST > TOAST SDK使用ガイド > TOAST IAP > Unity > エラーコード](/TOAST/ko/toast-sdk/iap-unity/#_17)
