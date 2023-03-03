## Game > Gamebase > Android Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Initialization

> <font color="red">[Caution]</font><br/>
>
> Only ONE Store v17 and v19 are supported.
> ONE Store v21 is under review and currently not supported.

* Store code must be specified to initialize Gamebase. 
* Select **STORE_CODE** among the following:  
    * GG: Google Play
    * ONESTORE: ONE Store
    * GALAXY: Galaxy Store
    * AMAZON: Amazon Appstore
    * HUAWEI: Huawei AppGallery

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, callback);
```

### Purchase Flow

Purchase of an item can be divided into **Purchase Flow**, **[Consume Flow](./aos-purchase/#consume-flow)**, and **[Reprocess Flow](./aos-purchase/#retry-transaction-flow)**.
It is recommended to implement the **Purchase Flow** in the following order:

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous purchase was not completed normally, the purchase will fail if the reprocessing is not performed. Therefore, call **requestItemListOfNotConsumed** before the purchase to run reprocessing, so that the Consume Flow is executed if there are any unprovided items.
2. The game client attempts to make a purchase by calling **requestPurchase** of the Gamebase SDK.
3. If the purchase is successful, call **requestItemListOfNotConsumed** to check the unconsumed purchase details, and if there is an item to provide, proceed with the Consume Flow.

### Consume Flow

If there's a value on the list of unconsumed purchases, proceed with the **Consume Flow** in the following order:

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
* Call **requestItemListOfNotConsumed** to run reprocessing and proceed with the [Consume Flow](./aos-purchase/#consume-flow) for any unprovided items.
* It is recommended to call reprocessing at the following times:
    * After login is completed
    * Before making a purchase
    * When entering the store (or lobby) in a game
    * When checking the user profile or mailbox

### Purchase Items

Request purchase by calling the following API using the gamebaseProductId of the item to purchase.<br/>
The gamebaseProductId is generally the same as the ID of item registered at the store, but it can be changed in the Gamebase console. 

When a game user cancels purchase, the **GamebaseError.PURCHASE_USER_CANCELED** error is returned.
Please process cancellation.

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, new GamebaseDataCallback<PurchasableReceipt>() {
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

**VO**

```java
class PurchasableReceipt {
    // The product ID of a purchased item.
    @Nullable
    String gamebaseProductId;
    
    // It is the value passed to payload when calling Gamebase.Purchase.requestPurchase API.
    // Not recommended to use the value due to the possible loss of information depending on the store server status.
    @Nullable
    String payload;
    
    // Price of purchased product.
    float price;
    
    // Currency code.
    @NonNull
    String currency;
    
    // Payment identifier.
    // This is an important piece of information used to call 'Consume' Server API with purchaseToken.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    @NonNull
    String paymentSeq;
    
    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    @Nullable
    String purchaseToken;
    
    // This product ID is registered to the console of stores such as Google and Apple.
    @NonNull
    String marketItemId;
    
    // This is a product type that can have the following values:
    // * UNKNOWN : An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE : A consumable product.
    // * AUTO_RENEWABLE : A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE : This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    @NonNull
    String productType;
    
    // This is a user ID with which a product is purchased.
    // If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
    @NonNull
    String userId;
    
    // The payment identifier of a store.
    @Nullable
    String paymentId;
    
    // The time when the product was purchased.(epoch time)
    long purchaseTime;
    
    // The time when the subscription expires.(epoch time)
    long expiryTime;
    
    // This value is used when making a purchase on Google, which can have the following values.
    // However, if the verification logic is temporarily disabled by Gamebase payment server due to error on Google server,
    // it returns only null, so please remember that it does not guarantee a valid return value at all times.
    // * null : Normal payment
    // * Test : Test payment
    // * Promo : Promotion payment
    @Nullable
    String purchaseType;
    
    // paymentId is changed whenever product subscription is renewed.
    // This field shows the paymentId that was used when a subscription product was first purchased.
    // This value does not guarantee to be always valid, as it can have no value
    // depending on the store from which the user made a purchase and the status of the payment server.
    @Nullable
    String originalPaymentId;
    
    // An identifier for Legacy API that purchases products with itemSeq.
    long itemSeq;

    // Store Code that purcahsed products.
    @NonNull
    public String storeCode;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "paymentSeq": "2021032510000001",
    "purchaseToken": "5U_NVCLKSDFKLJJ...",
    "marketItemId": "my_product_001",
    "productType": "CONSUMABLE",
    "userId": "AS@123456ABCDEFGHIJ",
    "paymentId": "GPA.1111-2222-3333-44444",
    "purchaseTime": 1616649225531,
    "expiryTime": 0,
    "itemSeq": 1000001
}
```
```json
{
    "gamebaseProductId": "my_subcription_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "paymentSeq": "2021032510000001",
    "purchaseToken": "5U_NVCLKKLJLSDG...",
    "marketItemId": "my_subcription_product_001",
    "productType": "CONSUMABLE_AUTO_RENEWABLE",
    "userId": "AS@123456ABCDEFGHIJ",
    "paymentId": "GPA.1111-2222-3333-56789",
    "purchaseTime": 1617069916128,
    "expiryTime": 1617070323784,
    "purchaseType": "Test",
    "originalPaymentId": "GPA.1111-2222-3333-56789",
    "itemSeq": 1000002
}
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

**VO**

```java
class PurchasableItem {
    // The product ID that is registered with the Gamebase console.
    // This is used when a product is purchased using Gamebase.Purchase.requestPurchase API.
    @Nullable
    String gamebaseProductId;
    
    // Product price.
    float price;
    
    // Currency code.
    @Nullable
    String currency;
    
    // The name of a product registered with the Gamebase console.
    @Nullable
    String itemName;
    
    // This product ID is registered to the console of stores such as Google and Apple.
    @NonNull
    String marketItemId;
    
    // This is a product type that can have the following values:
    // * UNKNOWN : An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE : A consumable product.
    // * AUTORENEWABLE : A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE : This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    @NonNull
    String productType;
    
    // Localized price information with currency symbol.
    @Nullable
    String localizedPrice;
    
    // The name of a localized product registered with the store console.
    @Nullable
    String localizedTitle;
    
    // The description of a localized product registered with the store console.
    @Nullable
    String localizedDescription;
    
    // Shows whether the product is 'used or not' in the Gamebase console.
    boolean isActive;
    
    // An identifier for Legacy API that purchases products with itemSeq.
    long itemSeq;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "itemName": "Consumable product for test",
    "marketItemId": "my_product_001",
    "productType": "CONSUMABLE",
    "localizedPrice": "₩1,000",
    "localizedTitle": "TEST PRODUCT 001",
    "localizedDescription": "Product for test 001",
    "isActive": true,
    "itemSeq": 1000001
}
```

### List Non-Consumed Items

* List non-consumed consumables (CONSUMABLE) and consumable subscriptions (CONSUMABLE_AUTO_RENEWABLE).
* In case there is any non-purchased item, request the game server (item server) to proceed with item delivery (provision).
* If the purchase is not completed normally, this API also serves the reprocessing function, so call it in the following situations:
    * Check if there's any unprovided items for a game user
    	* After login is completed
    	* When entering the store (or lobby) of a game
    	* When checking the user profile or mailbox
    * Check if there's any item in need of reprocessing
    	* Before making a purchase
    	* After a purchase fails

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| newBuilder()                    | **M**                      | Create Builder for Configuration object creation.                                |
| build()                         | **M**                      | Convert the Builder that has been set up into a Configuration object.                                |
| setAllStores(boolean allStores) | O                          | Return unconsumed lists purchased from a different store with the same UserID.<br/>Default value is **false**. |

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(@NonNull final Activity activity,
                                                       @NonNull final PurchasableConfiguration configuration,
                                                       @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder().build();
Gamebase.Purchase.requestItemListOfNotConsumed(activity, configuration, new GamebaseDataCallback<List<PurchasableReceipt>>() {
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
For subscription life cycle, refer to the following document.
[NHN Cloud > SDK User Guide > IAP > Android > Google Play Store Subscription (Regular payment) feature > Subscription Lifecycle Handing](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#subscription-lifecycle-handling)
> <font color="red">[Caution]</font><br/>
>
> Current subscriptions for Android are supported by Google Play Store only.
>

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                               |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------- |
| newBuilder()                    | **M**                      | Create Builder for Configuration object creation.                            |
| build()                         | **M**                      | Convert the Builder that has been set up into a Configuration object.                          |
| setAllStores(boolean allStores) | O                          | Return the subscriptions purchased from a different store with the same UserID.<br/>Default value is **false**.  |

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(@NonNull final Activity activity,
                                                    @NonNull final PurchasableConfiguration configuration,
                                                    @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder()
        .setAllStores(true)
        .build();
Gamebase.Purchase.requestActivatedPurchases(activity, configuration, new GamebaseDataCallback<List<PurchasableReceipt>>() {
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


### List Status of Subscriptions

You can view the status of purchased subscription products based on your current user ID.
Subscription products that have been paid for (auto-renewable subscriptions, auto-renewable consumable subscription products) can be viewed until they expire.
You can retrieve the status of expired subscription products with the **PurchasableConfiguration.setIncludeExpiredSubscriptions(true)** API.
For subscription status codes, see the following document.
 [NHN Cloud > SDK User Guide > IAP > Android > NHN Cloud IAP Class Reference > IapSubscriptionStatus.StatusCode](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode).

> <font color="red">[Caution]</font><br/>
>
> * The subscription status code is only returned correctly if you follow the guide below to set up the subscription event.
>     * Go to [Game > Gamebase > Store Console Guide > Google Console Guide and set up event propagation of real-time subscription information within Google's system](./console-google-guide/#google_1)
>     * The status code for a subscription product purchased without setting up events always returns 0 (PURCHASED).
> * Subscription products currently only supports Google Play Store.

**PurchasableConfiguration**

| API                                             | Mandatory(M) / Optional(O) | Description                              |
|-------------------------------------------------|----------------------------|------------------------------------------|
| newBuilder()                                    | **M**                      | Creates a Builder to create Configuration objects.  |
| build()                                         | **M**                      | Converts the configured builder into a Configuration object. |
| setIncludeExpiredSubscriptions(boolean include) | O                          | Includes expired subscription products<br/>Default value is **false**. |

**API**

```java
+ (void)Gamebase.Purchase.requestSubscriptionsStatus(@NonNull final Activity activity,
                                                     @NonNull final PurchasableConfiguration configuration,
                                                     @NonNull final GamebaseDataCallback<List<PurchasableSubscriptionStatus>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder()
        .setIncludeExpiredSubscriptions(true)
        .build();
Gamebase.Purchase.requestSubscriptionsStatus(activity, configuration, new GamebaseDataCallback<List<PurchasableSubscriptionStatus>>() {
    @Override
    public void onCallback(List<PurchasableSubscriptionStatus> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request status of subscription list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage()
                    + "errorDetail: " + exception.toString());
        }
    }
});
```

**VO**

```java
class PurchasableSubscriptionStatus {
    // Product ID of purchased item.
    @Nullable
    String gamebaseProductId;
    
    // Subscription status code.
    //
    // IapSubscriptionStatus.StatusCode : https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode
    public int statusCode;
    
    // Description for subscription status code.
    @NonNull
    public String statusDescription;
    
    // Price of purchased product.
    float price;
    
    // Currency code.
    @NonNull
    String currency;
    
    // Payment identifier
    // This is an important piece of information used to call 'Consume' Server API with purchaseToken.
    //
    // Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call the Consume API from game server!
    @NonNull
    String paymentSeq;
    
    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    //
    // Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    @NonNull
    String purchaseToken;
    
    // This is a product ID registered in store consoles such as Google, Apple.
    @NonNull
    String marketItemId;
    
    // Product types are as follows.
    // * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE: A consumable product.
    // * AUTO_RENEWABLE: A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    @NonNull
    String productType;
    
    // This is a user ID that purchased a product.
    // If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
    @NonNull
    String userId;
    
    // Store Code that purchased the product..
    @NonNull
    public String storeCode;
    
    // Payment identifier for store.
    @Nullable
    String paymentId;
    
    // Time when the product was purchased.(epoch time)
    long purchaseTime;
    
    // Time when subsciription ended.(epoch time)
    long expiryTime;
    
    // This value is used when making a purchase on Google, which can have the following values.
    // However, if the verification logic is temporarily disabled by Gamebase payment server due to error on Google server,
    // it returns only null, so please remember that it does not guarantee a valid return value at all times.
    // * null: Normal payment
    // * Test: Test payment
    // * Promotion: Promotion payment
    @Nullable
    String purchaseType;
    
    // PaymentId is changed whenever the subscription product is renewed.
    // This field shows the paymentId used when the subscription product was first paid for.
    // This value does not guarantee to be always valid, as it can have no value
    // depending on the store from which the user made a purchase and the status of the payment server.
    @Nullable
    String originalPaymentId;
    
    // Identifier for Legacy API for purchasing with itemSeq.
    long itemSeq;
    
    // Value sent to payload when calling the Gamebase.Purchase.requestPurchase API.
    // Depending on the status of your store's server, information may be lost, so using it is not recommended.
    @Nullable
    String payload;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_subcription_product_002",
    "statusCode": 13,
    "statusDescription": "EXPIRED",
    "userId": "AS@123456ABCDEFGHIJ",
    "storeCode": "GG",
    "currency": "KRW",
    "expiryTime": 1675012345678,
    "itemSeq": 1000003,
    "marketItemId": "my_subcription_product_002",
    "originalPaymentId": "GPA.1111-2222-3333-56789",
    "paymentId": "GPA.1111-2222-3333-56789",
    "paymentSeq": "2021032510000002",
    "price": 1000.0,
    "productType": "CONSUMABLE_AUTO_RENEWABLE",
    "purchaseTime": 1675001234567,
    "purchaseToken": "kfetTfGk4...",
    "purchaseType": "Test"
}
```


### Promotional Events

When a promotional purchase is completed, get an event from GamebaseEventHandler to be processed.  
See the guide on how to process a promotional purchase event via GamebaseEventHandler.
[Game > Gamebase > User Guide for Android SDK  > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | The purchase module has not been initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to the project. |
| PURCHASE_USER_CANCELED                    | 4002       | Purchase has been cancelled.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic is not completed.     |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | Product is not activated.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | You have exceeded your monthly purchase limit.             |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | The store is not supported.<br>You can choose either GG (Google), ONESTORE, GALAXY, AMAZON, or HUAWEI. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | Error in NHN Cloud IAP library.<br>Check the error details.  |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | Unknown error in purchase.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN Cloud IAP library.
* The information on the error in NHN Cloud IAP library is included in the error details, and you can find detailed error code and message as follows.

```java
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, new GamebaseDataCallback<PurchasableReceipt>() {
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

* For NHN Cloud IAP SDK error codes, refer to the document below.
    * [NHN Cloud > NHN Cloud SDK User Guide > NHN Cloud IAP > Android > Error Codes](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-android/#error-codes)

