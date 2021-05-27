## Game > Gamebase > Unity SDK ご利用ガイド > 決済

ここではUnityでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。
Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携することができるようサポートします。

### Settings

AndroidやiOSでアプリ内決済機能を設定する方法は、次のドキュメントをご参考ください。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)


Unity Standaloneで決済を行うには、IapAdapterとWebViewAdapterを追加する必要があります。
![GamebaseUnitySDKSettins Inspector](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)


### Purchase Flow

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
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(string gamebaseProductId, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
static void RequestPurchase(string gamebaseProductId, string payload, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)

// Legacy API
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void RequestPurchase(string gamebaseProductId)
{
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, (purchasableReceipt, error) =>
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


public void RequestPurchase(string gamebaseProductId)
{
    string userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, userPayload, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
            // userPayload value entered when calling API
            string payload = purchasableReceipt.payload
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

**VO**
```cs
public class PurchasableReceipt
{
    /// <summary>
    /// 구매한 아이템의 상품 ID 입니다.
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// itemSeq 로 상품을 구매하는 Legacy API용 식별자 입니다.
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 구매한 상품의 가격 입니다.
    /// </summary>
    public float price;

    /// <summary>
    /// 통화 코드 입니다.
    /// </summary>
    public string currency;

    /// <summary>
    /// 결제 식별자입니다.
    /// purchaseToken 과 함께 'Consume' 서버 API 를 호출하는데 사용하는 중요한 정보입니다.
    ///    
    /// 주의 : Consume API 는 게임 서버에서 호출하세요!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string paymentSeq;

    /// <summary>
    /// 결제 식별자입니다.
    /// paymentSeq 와 함께 'Consume' 서버 API 를 호출하는데 사용하는 중요한 정보입니다.
    /// Consume API 에서는 'accessToken' 라는 이름의 파라메터로 전달해야 합니다.
    ///    
    /// 주의 : Consume API 는 게임 서버에서 호출하세요!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string purchaseToken;

    /// <summary>
    /// Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID 입니다.
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// 상품 타입으로, 다음 값들이 올 수 있습니다.
    /// * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객센터로 문의하세요.
    /// * CONSUMABLE : 소비성 상품.
    /// * AUTO_RENEWABLE : 구독형 상품.
    /// * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// 상품을 구매했던 User ID.
    /// 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
    /// </summary>
    public string userId;

    /// <summary>
    /// 스토어의 결제 식별자 입니다.
    /// </summary>
    public string paymentId;

    /// <summary>
    /// 구독 상품은 갱신 될때마다 paymentId 가 변경됩니다.
    /// 이 필드는 맨 처음 구독 상품을 결제 했을때의 paymentId 를 알려줍니다.
    /// 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    /// 항상 유효한 값을 보장하지는 않습니다.
    /// </summary>
    public string originalPaymentId;

    /// <summary>
    /// 상품을 구매했던 시각입니다.(epoch time)
    /// </summary>
    public long purchaseTime;

    /// <summary>
    /// 구독이 종료되는 시각입니다.(epoch time)
    /// </summary>
    public long expiryTime;

    /// <summary>
    /// Gamebase.Purchase.requestPurchase API 호출시 payload 로 전달했던 값입니다.
    ///
    /// 이 필드는 예를 들어 동일한 User ID 로 구매 했음에도 게임 채널, 캐릭터 등에 따라
    /// 상품 구매 및 지급을 구분하고자 하는 경우 등
    /// 게임에서 필요로 하는 다양한 추가 정보를 담기 위한 목적으로 활용할 수 있습니다.
    /// </summary>
    public string payload;
}
```

### List Purchasable Items

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

**VO**
```cs
public class PurchasableItem
{
    /// <summary>
    /// Gamebase 콘솔에 등록된 상품 ID 입니다.
    /// Gamebase.Purchase.requestPurchase API 로 상품을 구매할때 사용됩니다.
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// itemSeq 로 상품을 구매하는 Legacy API용 식별자 입니다.
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 상품의 가격 입니다.
    /// </summary>
    public float price;

    /// <summary>
    /// 통화 코드 입니다.
    /// </summary>
    public string currency;

    /// <summary>
    /// Gamebase 콘솔에 등록된 상품 이름입니다.
    /// </summary>
    public string itemName;

    /// <summary>
    /// Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID 입니다.
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// 상품 타입으로, 다음 값들이 올 수 있습니다.
    /// * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객센터로 문의하세요.
    /// * CONSUMABLE : 소비성 상품.
    /// * AUTORENEWABLE : 구독형 상품.
    /// * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// 통화 기호가 포함된 현지화 된 가격 정보입니다.
    /// </summary>
    public string localizedPrice;

    /// <summary>
    /// 스토어 콘솔에 등록된 현지화된 상품 이름 입니다.
    /// </summary>
    public string localizedTitle;

    /// <summary>
    /// 스토어 콘솔에 등록된 현지화된 상품 설명 입니다.
    /// </summary>
    public string localizedDescription;

    /// <summary>
    /// Gamebase 콘솔에서 해당 상품의 '사용 여부'를 나타냅니다.
    /// </summary>
    public bool isActive;
}
```

### List Non-Consumed Items

アイテムを購入したが、正常にアイテムが消費(配送、支給)されなかった未消費決済履歴をリクエストします。
未消費決済履歴があある場合はゲームサーバー(アイテムサーバー)にリクエストして、アイテムを配送(支給)するように処理する必要があります。
正常に決済が完了しなかった場合、再処理の役割も担うため、次の状況で呼び出してください。

* ゲームユーザーに支給されなかったアイテムが残っているかを確認
    * ログイン完了後
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時
* 再処理が必要なアイテムがあるかを確認
    * 決済前
    * 決済失敗後

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

### List Actived Subscriptions

現在のユーザーIDで有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、期間が終了するまで照会できます。 
ユーザーIDが同じならAndroidとiOSで購入した定期購入商品が全て照会されます。

> <font color="red">[注意]</font><br/>
>
> 現在、定期購入商品は、Androidの場合Google Playストアのみサポートします。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestActivatedPurchasesSample()
{
    Gamebase.Purchase.RequestActivatedPurchases((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RequestItemListPurchasable succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableReceipt purchasableReceipt in purchasableReceiptList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("gamebaseProductId:{0}", purchasableReceipt.gamebaseProductId));
                message.AppendLine(string.Format("price:{0}", purchasableReceipt.price));
                message.AppendLine(string.Format("currency:{0}", purchasableReceipt.currency));
                
                // You will need paymentSeq and purchaseToken when calling the Consume API.
                // Refer to the following document for the Consume API.
                // https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchaseiap
                message.AppendLine(string.Format("paymentSeq:{0}", purchasableReceipt.paymentSeq));
                message.AppendLine(string.Format("purchaseToken:{0}", purchasableReceipt.purchaseToken));
                message.AppendLine(string.Format("marketItemId:{0}", purchasableReceipt.marketItemId));
                Debug.Log(message);
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RequestItemListPurchasable failed. error is {0}", error));
        }
    });
}
```

### Event by Promotion

プロモーション決済が完了すると、GamebaseEventHandlerを通してイベントを取得して処理できます。
GamebaseEventHandlerでプロモーション決済イベントを処理する方法は、下記のガイドを参照してください。
[Game > Gamebase > Unity SDK使用ガイド > ETC > Gamebase Event Handler](./unity-etc/#purchase-updated)

Supported Platforms
<span style="color:#1D76DB; font-size：10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size：10pt">■</span> UNITY_ANDROID

> <font color="red">[注意]</font><br/>
>
> iOSプロモーション決済を行うには、必ず下記のガイドに沿って設定してください。
> [Game > Gamebase > iOS SDK使用ガイド > 決済 > Event by Promotion](./ios-purchase/#event-by-promotion)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 購入ロジックが完了していない状態でAPIが呼び出されました。|
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 該当するストアのcashが足りないため決済することができません。            |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | 該当商品が有効な状態ではありません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | このストアには対応しておりません。<br>選択可能なストアは、GG(Google)、TS(ONE store)、TESTです。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリーエラーです。<br>DetailCodeを確認してください。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|


* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* エラーコードは次のように確認できます。

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Unity > エラーコード](/TOAST/en/toast-sdk/iap-unity/#_17)
