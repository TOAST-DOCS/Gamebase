## Game > Gamebase > Android Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

#### 1. Store Console

* Refer to the IAP guide as below, to register an app to each store and get an Appkey.
	* [Game > Gamebase > Store Console Guide  > Google Console Guide](./console-google-guide)
	* [Game > Gamebase > Store Console Guide > ONEStore Console Guide](./console-onestore-guide)

#### 2. Register as Store's Tester

* Register a tester to each store for purchase testing.
    * Google
        * [Android > Setting test purchase](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > In-app purchase test](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * For testing, be sure to register device phone number you want a sandbox for, with In-app Information-Test button.
        * A tester device requires USIM, with registered phone number (MDN).
        * Needs **ONE store** application installed.

#### 3. Item Registration

* See the guide to register items.
    * [Game > Gamebase > Console User Guide > Purchase > Register](./oper-purchase/#register_1)

#### 4. Setting SDK

* Add the gamebase-adapter-purchase module of the market to gradle dependency.

```groovy
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'
    
    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
}
```

#### 5. AndroidManifest.xml(ONE store only)

* Add the following setting to use ONE store.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development / release -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Store code must be specified to initialize Gamebase. 
* Select **STORE_CODE** among the following:  
    * GG: Google
    * ONESTORE: OneStore

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, callback);
```

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.  
You may execute an item purchase in the following order: 

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. When a previous purchase has not been closed, purchase fails unless reprocessing runs. Therefore, call **requestItemListOfNotConsumed** before payment so as to run reprocessing, and execute Consume Flow if there's any unsupplied item. 
2. For the game client, call **requestPurchase** of Gamebase SDK to make a purchase. 
3. When a purchase has been successful, call **requestItemListOfNotConsumed** and check history of non-consumable purchases; and if there's any item to supply, execute Consume Flow. 

### Consume Flow

If there's a value on the list of non-consumable purchases, execute Consume Flow in the following order:

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.10.0.png)

1. The game client requests for Consume of a purchase item on the game server. 
    * Deliver UserID, itemSeq, paymentSeq, and purchaseToken.
2. The game server tracks down the history of item supplies with same paymentSeq, or purchaseToken within game database.  
    * 2-1 If not supplied yet, supply the item for itemSeq to UserID.   
    * 2-2 After item is provided, save UserID, itemSeq, paymentSeq, and purchaseToken to game database so as to check redundancy.  
3. The game server calls Consume API to the Gamebase server to complete with item supply.
    * [API Guide > Purchase (IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

* Sometimes, a successful purchase at store ends up with failed closure, due to error. 
* Call **requestItemListOfNotConsumed** to run reprocessing and execute Consume Flow for any non-supplied items.
* It is recommended to call reprocessing in time for the following: 
    * After login is completed 
    * Before payment 
    * Entering store (or lobby) of a game 
    * Checking user profile or mailbox 

### Purchase Items

With gamebaseProductId of an item to purchase, call the following API to request for purchase.<br/>
The gamebaseProductId is generally same as the ID of item registered at store, but it could be changed on Gamebase console. 
Additional information for the payload field remains at the **PurchasableReceipt.payload** field after a successful payment, and therefore, can be applied to many purposes.<br/>
When a game user cancels purchase, the **GamebaseError.PURCHASE_USER_CANCELED** error is returned.
Please process cancellation.

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final String payload,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
// Legacy API
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          final long itemSeq,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
String userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, userPayload, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt receipt, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else if(exception.getCode() == GamebaseError.PURCHASE_USER_CANCELED) {
            // User canceled.
        } else {
            // To Purchase Item Failed cause of the error
        }
    }
});
```

### List Purchasable Items

To retrieve the list of items, call the following API. Information of each item is included in the array of callback return.

**API**

```java
+ (void)Gamebase.Purchase.requestItemListPurchasable(Activity activity, GamebaseDataCallback<List<PurchasableItem>> callback);
```

**Example**

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### List Non-Consumed Items

* List non-consumed consumables and consumable subscriptions (CONSUMABLE_AUTO_RENEWABLE).
* In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).
* When a purchase is not properly completed, reprocessing is also required; please call in time for the following: 
    * See if there's any unsupplied items for a game user 
    	* After login is completed 
    	* Entering store (or lobby) of a game 
    	* Checking user profile or mailbox
    * See if there's any item in need of reprocessing 
    	* Before purchase 
    	*  After failed purchase 

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
Gamebase.Purchase.requestItemListOfNotConsumed(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### List Activated Subscriptions

List activated subscriptions for a current user ID. 
Paid subscriptions (auto-renewable subscription, or auto-renewed consumable subscription) can be queried before they're expired. 
Under a same user ID, both subscriptions for Android and iOS can be listed. 

> <font color="red">[Caution]</font><br/>
>
> Current subscriptions for Android are supported by Google Play Store only.
>

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
Gamebase.Purchase.requestActivatedPurchases(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request subscription list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Promotional Events

When a promotional purchase is completed, get an event from GamebaseEventHandler to be processed.  
See the guide on how to process a promotional purchase event via GamebaseEventHandler.
[Game > Gamebase > User Guide for Android SDK  > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | The purchase module is not initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to the project. |
| PURCHASE_USER_CANCELED                   | 4002       | Purchase is cancelled.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic is not completed.     |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | Cannot purchase due to shortage of cash of the store.             |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | Product is not activated.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | The store is not supported.<br>You can choose either GG (Google), ONESTORE, GALAXY. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | Error in IAP library.<br>Check detail codes.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | Unknown error in purchase.<br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we will respond ASAP. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurs at an TOAST IAP SDK.
* Check the error code as below.

```java
Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Purchase successful");
            ...
        } else {
            Log.e(TAG, "Purchase failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PURCHASE_EXTERNAL_LIBRARY_ERROR) {
                // IAP Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* For TOAST IAP SDK error codes, refer to the document below.
    * [TOAST > TOAST SDK User Guide > TOAST IAP > Android > Error Codes](/TOAST/en/toast-sdk/iap-android/#_24)

