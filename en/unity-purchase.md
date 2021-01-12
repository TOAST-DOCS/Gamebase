## Game > Gamebase > Unity Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).
Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

For Android and iOS IAP setting, refer to the below documents.<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

To make payments at Unity Standalone, IapAdapter and WebViewAdapter must be added. 
![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
You may execute an item purchase in the following order: 

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. When a previous purchase has not been closed, purchase fails unless reprocessing runs. Therefore, call **RequestItemListOfNotConsumed** before payment so as to run reprocessing, and execute Consume Flow if there's any unsupplied item. 
2. For the game client, call **RequestPurchase** of Gamebase SDK to make a purchase. 
3. When a purchase has been successful, call **RequestItemListOfNotConsumed** and check history of non-consumable purchases; and if there's any item to supply, execute Consume Flow. 

### Consume Flow

If there's a value on the list of non-consumable purchases, execute Consume Flow in the following order:

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.10.0.png)

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
                message.AppendLine(string.Format("itemSeq:{0}", purchasableReceipt.itemSeq));
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
    * [TOAST > User Guide for TOAST SDK > TOAST IAP > Unity > Error Codes](/TOAST/en/toast-sdk/iap-unity/#_17)





