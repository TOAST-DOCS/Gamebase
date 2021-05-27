## Game > Gamebase > Unity Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).
Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

For Android and iOS IAP setting, refer to the below documents.<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

To make payments at Unity Standalone, IapAdapter and WebViewAdapter must be added. 
![GamebaseUnitySDKSettins Inspector](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
You may execute an item purchase in the following order: 

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. When a previous purchase has not been closed, purchase fails unless reprocessing runs. Therefore, call **RequestItemListOfNotConsumed** before payment so as to run reprocessing, and execute Consume Flow if there's any unsupplied item. 
2. For the game client, call **RequestPurchase** of Gamebase SDK to make a purchase. 
3. When a purchase has been successful, call **RequestItemListOfNotConsumed** and check history of non-consumable purchases; and if there's any item to supply, execute Consume Flow. 

### Consume Flow

If there's a value on the list of non-consumable purchases, execute Consume Flow in the following order:

> <font color="red">[Caution]</font><br/>
>
To prevent the multiple issuance of the same purchased item, always check the game server for issuance history of items.
>

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. The game client requests for Consume of a purchase item on the game server. 
    * Deliver UserID, itemSeq, paymentSeq, purchaseToken.
2. The game server tracks down the history of item supplies with same paymentSeq, or purchaseToken within game database.  
    * 2-1. If not supplied yet, supply the item for itemSeq to UserID.  
    * 2-2. After item is provided, save UserID, itemSeq, paymentSeq, and purchaseToken to game database so as to check redundancy.  
3. The game server calls Consume API to the Gamebase server to complete with item supply.
    * [API Guide > Purchase (IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

* Sometimes, a successful purchase at store ends up with failed closure, due to error. 
* Call **RequestItemListOfNotConsumed** to run reprocessing and execute Consume Flow for any non-supplied items. 
* It is recommended to call reprocessing in time for the following:
    * After login is completed
    * Before payment
    * Entering store (or lobby) of a game
    * Checking user profile or mailbox

### Purchase Item

Call following API of an item to purchase by using itemSeq to send a purchase request.
When a game user cancels purchasing, the **PURCHASE_USER_CANCELED** error will be returned.

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

To retrieve the list of items, call the following API. 
Information of each item is included in the array of callback return.

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

Request for the list of non-consumed purchases, in which items were not properly consumed (delivered or supplied) after purchase.  
In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).
When a purchase is not properly completed, reprocessing is also required; please call in time for the following: 

* See if there's any unsupplied items for a game user 
    * After login is completed
    * Entering store (or lobby) of a game
    * Checking user profile or mailbox
* See if there's any item in need of reprocessing 
    * Before purchase
    * After failed purchase 

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

List activated subscriptions for the current user ID. 
Subscriptions that are paid up (e.g. auto-renewable subscription, auto-renewed consumable subscription) can be listed before they are expired.
With a same user ID, all purchased subscriptions from Android and iOS can be listed. 

> <font color="red">[Caution]</font><br/>
>
> Current subscriptions for Android are supported by Google Play Store only. 

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

When a promotional purchase is completed, get an event from GamebaseEventHandler to be processed. 
See the guide on how to process a promotional purchase event via GamebaseEventHandler.
[Game > Gamebase > User Guide for Unity SDK > ETC > Gamebase Event Handler](./unity-etc/#purchase-updated)

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

> <font color="red">[Caution]</font><br/>
>
> To execute an iOS promotion purchase, make sure to follow the guide for a setup.
> [Game > Gamebase > User Guide for iOS SDK > Purchase > Event by Promotion](./ios-purchase/#event-by-promotion)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED | 4001 | The purchase module is not initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to project. |
| PURCHASE_USER_CANCELED | 4002 | Purchase is cancelled. |
| PURCHASE_NOT_FINISHED\_PREVIOUS\_PURCHASING | 4003 | API has been called when a purchase logic is not completed. |
| PURCHASE_NOT_ENOUGH_CASH | 4004 | Cannot purchase due to shortage of cash of the store. |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | Product is not activated.   |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_NOT_SUPPORTED_MARKET | 4010 | The store is not supported.<br>You can choose either GG (Google), TS (ONE Store), or TEST. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | Error in IAP library.<br>Check DetailCode. |
| PURCHASE_UNKNOWN_ERROR | 4999 | Unknown error in purchase.<br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we'll respond ASAP. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)


**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurs at an IAP module.
* Check the error code as below:

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

* For IAP error codes, refer to the document below.
    * [NHN Cloud > User Guide for NHN Cloud SDK > NHN Cloud IAP > Unity > Error Codes](/TOAST/en/toast-sdk/iap-unity/#_17)