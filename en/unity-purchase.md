## Game > Gamebase > Unity Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).
Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

> <font color="red">[Caution]</font><br/>
>
> If there is payment-related processing in an external package, the Gamebase payment function may not work properly.

### Settings

For Android and iOS IAP setting, refer to the below documents.<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
It is recommended to implement the Purchase Flow in the following order:

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous purchase was not completed normally, the purchase will fail if the reprocessing is not performed. Therefore, call **RequestItemListOfNotConsumed** before the purchase to run reprocessing, so that the Consume Flow is executed if there are any unprovided items.
2. The game client attempts to make a purchase by calling **RequestPurchase** of the Gamebase SDK.
3. If the purchase is successful, call **RequestItemListOfNotConsumed** to check the unconsumed purchase details, and if there is an item to provide, proceed with the Consume Flow.

### Consume Flow

If there's a value on the list of unconsumed purchases, proceed with the Consume Flow in the following order:

> <font color="red">[Caution]</font><br/>
>
> To prevent duplicate provision of an item, always check whether the item is being provided in duplicate in the game server.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. The game client makes a request to the game server to consume the purchase item.
    * Passes UserID, gamebaseProductId, paymentSeq, and purchaseToken.
2. The game server checks the game DB to see if there is a history of already providing an item with the same paymentSeq.
    * 2-1. If the item has not been provided yet, call the Gamebase server's Payment Transaction API to verify that the paymentSeq and purchaseToken values are valid.
        * [Game > Gamebase > API Guide > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. If purchaseToken is a normal value, provide the item corresponding to gamebaseProductId to UserID.
    * 2-3. After providing the item, store UserID, gamebaseProductId, paymentSeq, and purchaseToken in the game DB to prevent duplicate provision or allow for re-provision.
3. Regardless of whether the item has been provided, the game server completes the item provision by calling the Gamebase server's consume API.
    * [Game > Gamebase > API Guide > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* There are cases where the store purchase has been made successfully but the process was not properly completed due to errors.
* Call **RequestItemListOfNotConsumed** to run reprocessing and proceed with the Consume Flow for any unprovided items.
* It is recommended to call reprocessing at the following times:
    * After login is completed
    * Before making a purchase
    * When entering the store (or lobby) in a game
    * When checking the user profile or mailbox

### Purchase Item

Request a purchase by using the gamebaseProductId of the item to purchase.<br/>
The gamebaseProductId is generally the same as the ID of item registered at the store, but it can be changed in the Gamebase console. 
Additional information entered in the payload field is maintained at the **PurchasableReceipt.payload** field after a successful payment, so it can be used for many purposes.<br/>

> <font color="red">[Caution]</font><br/>
>
> The AMAZON store does not support the **payload** field.
>

When a game user cancels purchase, the **PURCHASE_USER_CANCELED** error is returned.
Please process cancellation.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(string gamebaseProductId, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
static void RequestPurchase(string gamebaseProductId, string payload, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
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
    /// The product ID of a purchased item.
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// An identifier for Legacy API that purchases products with itemSeq.
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// The price of purchased product.
    /// </summary>
    public float price;

    /// <summary>
    /// Currency code.
    /// </summary>
    public string currency;

    /// <summary>
    /// Payment identifier.
    /// This is an important piece of information used to call 'Consume' server API with purchaseToken.
    ///    
    /// Caution: Call Consume API from game server!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string paymentSeq;

    /// <summary>
    /// Payment identifier.
    /// This is an important piece of information used to call 'Consume' server API with paymentSeq.
    /// In Consume API, the parameter must be named 'accessToken' to be passed.
    ///    
    /// Caution: Call Consume API from game server!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string purchaseToken;

    /// <summary>
    /// The product ID that is registered to store console such as Google or Apple.
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// The product type which can have the following values:
    /// * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    /// * CONSUMABLE: A consumable product.
    /// * AUTO_RENEWABLE: A subscription product.
    /// * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// This is a user ID with which a product is purchased.
    /// If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
    /// </summary>
    public string userId;

    /// <summary>
    /// The payment identifier of a store.
    /// </summary>
    public string paymentId;

    /// <summary>
    /// paymentId is changed whenever product subscription is renewed.
    /// This field shows the paymentId that was used when a subscription product was first purchased.
    /// This value does not guarantee to be always valid, as it can have no value depending on the store
    /// the user made a purchase and the status of the payment server.
    /// </summary>
    public string originalPaymentId;

    /// <summary>
    /// The time when the product was purchased.(epoch time)
    /// </summary>
    public long purchaseTime;

    /// <summary>
    /// The time when the subscription expires.(epoch time)
    /// </summary>
    public long expiryTime;


     /// <summary>
    /// The store code where the purchase is made.
    /// You can check the store code list from GamebaseStoreCode class.
    /// </summary>
    public string storeCode;

    /// <summary>
    /// It is the value passed to payload when calling Gamebase.Purchase.requestPurchase API.
    /// Not recommended to use due to the possible loss of information depending on the store server status.
    /// </summary>
    public string payload;

    /// <summary>
    /// Whether it is promotion payment
    /// - (Android) If the validation logic is temporarily turned off in the Gamebase payment server, the value is output as false only, so it is not always guaranteed to be a valid value.
    /// </summary>
    public bool isPromotion;
    
    /// <summary>
    /// Whether it is test purchase
    /// - (Android) If the validation logic is temporarily turned off in the Gamebase payment server, the value is output as false only, so it is not always guaranteed to be a valid value.
    /// </summary>
    public bool isTestPurchase;
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
    /// The product ID that is registered with the Gamebase console.
    /// Used when a product is purchased using Gamebase.Purchase.requestPurchase API.
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// An identifier for Legacy API that purchases products with itemSeq.
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// The price of a product.
    /// </summary>
    public float price;

    /// <summary>
    /// Currency code.
    /// </summary>
    public string currency;

    /// <summary>
    /// The name of a product registered in the Gamebase console.
    /// </summary>
    public string itemName;

    /// <summary>
    /// The product ID that is registered to store console such as Google or Apple.
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// The product type which can have the following values:
    /// * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    /// * CONSUMABLE: A consumable product.
    /// * AUTORENEWABLE: A subscription product.
    /// * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// Localized price information with currency symbol.
    /// </summary>
    public string localizedPrice;

    /// <summary>
    /// The name of a localized product registered with the store console.
    /// </summary>
    public string localizedTitle;

    /// <summary>
    /// The description of a localized product registered with the store console.
    /// </summary>
    public string localizedDescription;

    /// <summary>
    /// Shows whether the product is 'used or not' in the Gamebase console.
    /// </summary>
    public bool isActive;
}
```

### List Non-Consumed Items

Request for the list of non-consumed purchases, in which items were not properly consumed (delivered or provided) after purchase.  
In case there is any non-purchased item, request the game server (item server) to proceed with item delivery (provision).
If the purchase was not completed normally, this API also serves the reprocessing function, so call it in the following situations:

* Check if there's any unprovided items for a game user
    * After login is completed
    * When entering the store (or lobby) of a game
    * When checking the user profile or mailbox
* Check if there's any item in need of reprocessing
    * Before making a purchase
    * After a purchase fails


**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | Return the unconsumed list purchased from a different store with the same UserID.<br/>Default value is **false**. |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestItemListOfNotConsumedSample(bool allStores)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        allStores = allStores
    };
    Gamebase.Purchase.RequestItemListOfNotConsumed(configuration, (purchasableReceiptList, error) =>
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

### List Activated Subscriptions

List activated subscriptions for the current user ID. 
Subscriptions that are paid up (e.g. auto-renewable subscription, auto-renewed consumable subscription) can be listed before they are expired.

> <font color="red">[Caution]</font><br/>
>
> For Android, subscriptions are currently supported only on the Google Play Store.


**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | Return the unconsumed list purchased from a different store with the same UserID.<br/>Default value is **false**. |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestActivatedPurchasesSample(bool allStores)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        allStores = allStores
    };
    
    Gamebase.Purchase.RequestActivatedPurchases(configuration, (purchasableReceiptList, error) =>
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

| Error                                       | Error Code | Description                              |
| ------------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                    | 4001       | The purchase module has not been initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to project. |
| PURCHASE_USER_CANCELED                      | 4002       | Purchase has been cancelled. |
| PURCHASE_NOT_FINISHED\_PREVIOUS\_PURCHASING | 4003       | API has been called when a purchase logic is not completed. |
| PURCHASE_NOT_ENOUGH_CASH                    | 4004       | Cannot purchase due to shortage of cash of the store. |
| PURCHASE_INACTIVE_PRODUCT_ID                | 4005       | Product is not activated.   |
| PURCHASE_NOT_EXIST_PRODUCT_ID               | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_LIMIT_EXCEEDED                     | 4007       | You have exceeded your monthly purchase limit.              |
| PURCHASE_NOT_SUPPORTED_MARKET               | 4010       | The store is not supported.<br>The stores you can select are AS (App Store), GG (Google), ONESTORE, GALAXY, AMAZON, and HUAWEI. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR             | 4201       | Error in IAP library.<br>Check the code details. |
| PURCHASE_UNKNOWN_ERROR                      | 4999       | Unknown error in purchase.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)


**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN Cloud IAP library.
* The information on the error in NHN Cloud IAP library is included in the error details, and you can find detailed error code and message as follows.

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

* For NHN Cloud IAP error codes, refer to the document below.
    * [NHN Cloud > User Guide for NHN Cloud SDK > NHN Cloud IAP > Unity > Error Codes](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
