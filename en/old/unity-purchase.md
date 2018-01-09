## Game > Gamebase > Unity Developer's Guide > Purchase

## Purchase

This page describes how to set an in-app purchase (IAP).

Gamebase provides an integrated purchase API to easily integrate in-app purchase of many stores in a game.


### Settings

For Android and iOS IAP setting, refer to the below documents. <br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)


###  Purchase Flow

Purchase of items should be implemented in the following order.<br/>

![purchase flow](purchase_flow.png)

1. Call **RequestPurchase** of Gamebase SDK in a game client to try to purchase.
2. After a successful purchase, call **RequestItemListOfNotConsumed** to check list of non-consumed purchases.
3. If the value is on the returned list, the game client sends a request to the game server to consume the purchased item.
4. The game server request for Consume API to the Gamebase server via API.
   [API Guide](http://docs.cloud.toast.com/ko/Game/Gamebase/ko/Server%20Developer%60s%20Guide/#wrapping-api)
5. If the IAP server has successfully called for a Consume API, the game server provides the item to the game client.


A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to implement a reprocessing logic. <br/>

1. Delivery Request of Unprocessed Items
  * When a login is successful, call **RequestItemListOfNotConsumed** to check list of non-consumed purchases.
  * If the value is on the returned list, the game client sends a request to the game server to consume, and the item is provided.
2. Reprocess Error in Purchase
    * When a login is successful, call **RequestRetryTransaction** to try to automatically reprocess the unprocessed.

    * If there is a value on the returned successList, the game client sends a request to the game server to consume, and the item is provided.

    * If there is a value on the returned failList, send the value to the game server or Log & Crash to secure data. Also contact [Customer Center](https://cloud.toast.com/support/faq) for the cause of reprocessing failure.

### Purchase Item


Call following API of an item to purchase, using itemSeq to send a purchase request. <br/>
When a game user cancels purchasing, the **PURCHASE_USER_CANCELED** error will be returned.

**API**

![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

To retrieve the list of items, call the following API. <br/>
 In the array of callback return, information of each item is included.

**API**

![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase. <br/>
In case of non-purchased items, ask the game server (item server) to process delivery (supply) of the items.

**API**

![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)


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

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST Cloud IAP server, try to reprocess by using API. <br/>
Based on the latest success of purchase, API call is required for item delivery (supply).


**API**

![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)


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

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | The purchase module has not been initialized.<br> Check if the gamebase-adapter-purchase-IAP module has been added to project. |
| PURCHASE_USER_CANCELED                   | 4002       | Game user has canceled purchasing an item.                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic has not been completed.     |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | Cannot purchase due to shortage of cash of the store.              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | The store is not supported. <br> You can choose either GG (Google), TS (ONE Store), or TEST. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | Error in IAP library.<br> Check DetailCode.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | The purchase error is undefined.<br> Please upload the entire logs to the Customer Center and we'll respond ASAP. |

* Refer to the following document for the entire error code.
  - [Entire Error Codes](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurred at an IAP module.
* For IAP error codes, refer to the document below.
  * [IAP > Error Code Guide > Client API Error Type](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)
