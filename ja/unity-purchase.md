## Game > Gamebase > Unity SDK ご利用ガイド > 決済

ここではUnityでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。
Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携することができるようサポートします。

### Settings

AndroidやiOSでアプリ内決済機能を設定する方法は、次のドキュメントをご参考ください。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

###  Purchase Flow

アイテムの購入は次のような手順で設計してください。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)


1. ゲームクライアントでは、Gamebase SDKの**RequestPurchase**を呼び出して決済を試みます。
2. 決済が成功した場合、**RequestItemListOfNotConsumed**を呼び出して未消費決済の内訳を確認します。
3. 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーに決済アイテムに対するconsume(消費)をリクエストします。
4. ゲームサーバーは、GamebaseのサーバーにAPI経由でconsume(消費)APIをリクエストします。[APIガイド](/Game/Gamebase/ja/api-guide/#wrapping-api)
5. IAPサーバーからconsume(消費)APIの呼び出しに成功すると、ゲームサーバーがゲームクライアントにアイテムを配布します。

ストア決済には成功したものの、エラーが発生して正常に終了することができない場合があります。ログイン完了後に次の二つのAPIをそれぞれ呼び出し、再処理ロジックを設計してください。<br/>

1. 未処理アイテムの送信リクエスト
    * ログインに成功した後、**RequestItemListOfNotConsumed**を呼び出して未消費決済の内訳を確認します。
    * 返された未消費決済内訳のリストに値が存在する場合、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを配布します。
2. 決済エラー再処理リクエスト
    * ログインに成功した後、**RequestRetryTransaction**を呼び出し、未処理内訳に対し自動で再処理を試みます。
    * 返されたsuccessListに値が存在する場合、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを配布します。
    * 返されたfailListに値が存在する場合、該当する値をゲームサーバーやLog & Crashなどを通し送信してデータを確保し、[カスタマーセンター](https://toast.com/support/inquiry)に再処理失敗の原因をお問い合わせください。

### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。
ゲームユーザーが購入をキャンセルする場合、**PURCHASE_USER_CANCELED**エラーが返されます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void RequestPurchase(long itemSeq)
{
    Gamebase.Purchase.RequestPurchase(itemSeq, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}
```

### Get a List of Purchasable Items

アイテムリストを照会したい場合、次のAPIを呼び出します。
コールバックで返されるリストの中にはそれぞれ各アイテムの情報が含まれています。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**Example**
```cs
public void RequestItemListPurchasable()
{
    Gamebase.Purchase.RequestItemListPurchasable((purchasableItemList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```



### Get a List of Non-Consumed Items

アイテムを購入したものの、正常にアイテムが消費(送信、配布)されていない未消費決済の内訳をリクエストします。
未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestItemListOfNotConsumed()
{
    Gamebase.Purchase.RequestItemListOfNotConsumed((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");

            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

### Reprocess Failed Purchase Transaction

ストアでは決済が正常に行われたものの、TOAST IAPサーバーの検証失敗などにより決済が正常に行われなかった場合、APIを利用して再処理を試みます。
最後に決済が成功した内訳を基に、アイテム送信(配布)などのAPIを呼び出して処理する必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestRetryTransaction(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableRetryTransactionResult> callback)
```

**Example**
```cs
public void RequestRetryTransaction()
{
    Gamebase.Purchase.RequestRetryTransaction((purchasableRetryTransactionResult, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RequestRetryTransaction succeeded.");

            // Should Deal With This Retry Transaction Result.
            // You may send result to your gameserver and add item to user.
        }
        else
        {
            Debug.Log(string.Format("RequestRetryTransaction failed. error is {0}", error));
        }
    });
}
```

### AppStore Promotion IAP

AppStore 앱 내에서 아이템을 구매할 수 있는 기능을 제공합니다.
아이템 구매 성공 후, 아래의 등록해놓은 핸들러를 통하여, 아이템지급을 진행할 수 있습니다.

프로모션 IAP는 AppStore Connect 에서 별도의 설정이 되어야 노출이 가능합니다.

> <font color="red">[주의]</font><br/>
> iOS 11 이상에서만 사용할 수 있습니다.
> Xcode 9.0 이상에서 빌드가 필요합니다.
> Gamebase 1.13.0 이상에서 지원합니다. (TOAST IAP SDK 1.6.0 이상적용)


> <font color="red">[주의]</font><br/>
> 로그인 성공 이후에만 호출 할 수 있습니다.
> 로그인 성공 후, 다른 결제 API보다 먼저 실행되어야 합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetPromotionIAPHandler(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void SetPromotionIAPHandler()
{
    Gamebase.Purchase.SetPromotionIAPHandler((purchasableReceipt, error) => 
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}
```
**Overview**

* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f

**How to Test AppStore Promotion IAP**

> `주의`
> App Store Connect에 앱을 업로드한 다음 TestFlight를 통하여 앱을 설치 후, 테스트를 진행할 수 있습니다.
> 

1. TestFlight로 App을 설치합니다.
2. 아래와 같은 URL Scheme을 호출하여, 테스트를 진행합니다.
| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 고정값 |
| host &amp; path | 없음 | 없음 |
| queries | action | purchaseIntent |
|		  | bundleId | 앱의 bundeld identifier |
|		  | productIdentifier | 구매 아이템의 product identifier |

예제) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 購入ロジックが完了していない状態でAPIが呼び出されました。|
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 該当するストアのcashが足りないため決済することができません。            |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | このストアには対応しておりません。<br>選択可能なストアは、GG(Google)、TS(ONE store)、TESTです。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリーエラーです。<br>DetailCodeを確認してください。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* 오류 코드는 다음과 같이 확인하실 수 있습니다.

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    Error moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [Mobile Service > IAP > エラーコード > Client APIエラータイプ](/Mobile%20Service/IAP/ja/error-code/#client-api)





