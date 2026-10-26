<!-- machine_translated: true -->

<!-- pre-align:aligned sig=bacf30302c7a -->

<a id="game-gamebase-android-developers-guide-purchase"></a>
## Game > Gamebase > Android Developer's Guide > Purchase { #game-gamebase-android-developers-guide-purchase }

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

<a id="initialization"></a>
### Initialization { #initialization }

> <font color="red">[Caution]</font><br/>
>
> ONE Store v17, v19, and v21 are supported.
> For ONE Store, you can use only one of the following versions: v17, v19, v21, or external, but not at the same time.

* Store code must be specified to initialize Gamebase. 
* Select **STORE_CODE** among the following:  
    * GG: Google Play
    * ONESTORE: ONE Store
    * GALAXY: Galaxy Store
    * HUAWEI: Huawei AppGallery
    * MYCARD: MyCard

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, callback);
```

<a id="purchase-flow"></a>
### Purchase Flow { #purchase-flow }

Purchase of an item can be divided into **Purchase Flow**, **[Consume Flow](#consume-flow)**, and **[Reprocess Flow](#retry-transaction-flow)**.
It is recommended to implement the **Purchase Flow** in the following order:

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous purchase was not completed normally, the purchase will fail if the reprocessing is not performed. Therefore, call **requestItemListOfNotConsumed** before the purchase to run reprocessing, so that the Consume Flow is executed if there are any unprovided items.
2. The game client attempts to make a purchase by calling **requestPurchase** of the Gamebase SDK.
3. If the purchase is successful, call **requestItemListOfNotConsumed** to check the unconsumed purchase details, and if there is an item to provide, proceed with the Consume Flow.

<a id="consume-flow"></a>
### Consume Flow { #consume-flow }

If there's a value on the list of unconsumed purchases, proceed with the **Consume Flow** in the following order:

> <font color="red">[Caution]</font><br/>
>
> To prevent duplicate provision of an item, always check whether the item is being provided in duplicate in the game server.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. The game client makes a request to the game server to consume the purchase item.
    * Passes UserID, paymentSeq, and purchaseToken.
2. The game server checks the game DB to see if there is a history of already providing an item with the same paymentSeq.
    * 2-1. If the item has not been provided yet, call the Gamebase server's Payment Transaction API to verify that the paymentSeq value is matched with the response field and purchaseToken value is valid.
        * [Game > Gamebase > API Guide > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseToken corresponds to **accessToken** in the server API guide document 
    * 2-2. gamebaseProductId can be found in the response field of the server's Payment Transaction API 
        * gamebaseProductId also exists in the client's used payment history list, but the value may not be available at the time of reprocessing, so please use the gamebaseProductId value obtained from the server's Payment Transaction API 
    * 2-3. If the Payment Transaction API call succeeds and the purchaseToken is confirmed to be valid, issues the item corresponding to the gamebaseProductId to the UserID 
    * 2-4. After issuing the item, saves the UserID, gamebaseProductId, paymentSeq, and purchaseToken in the game DB to prevent duplicate payments or re-payments 
3. Regardless of whether the item has been provided, the game server completes the item provision by calling the Gamebase server's consume API so that the unspent payment list is no longer returned.
    * [Game > Gamebase > API Guide > Purchase(IAP) > Consume](./api-guide/#consume)

<a id="retry-transaction-flow"></a>
### Retry Transaction Flow { #retry-transaction-flow }

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* There are cases where the store purchase has been made successfully but the process was not properly completed due to errors.
* Call **requestItemListOfNotConsumed** to run reprocessing and proceed with the [Consume Flow](#consume-flow) for any unprovided items.
* It is recommended to call reprocessing at the following times:
    * After login is completed
    * Before making a purchase
    * When entering the store (or lobby) in a game
    * When checking the user profile or mailbox

<a id="purchase-items"></a>
### Purchase Items { #purchase-items }

Request purchase by calling the following API using the gamebaseProductId of the item to purchase.<br/>
The gamebaseProductId is generally the same as the ID of item registered at the store, but it can be changed in the Gamebase console. 

When a game user cancels purchase, the **GamebaseError.PURCHASE_USER_CANCELED** error is returned.
Please process cancellation.

If a situation occurs where payment completion must be awaited, such as slow payments or those requiring parental consent, the **GamebaseError.PURCHASE_PENDING** error is returned.
Once the payment is successfully completed, you can receive the payment completion event via the GamebaseEventHandler.
[Game > Gamebase > User Guide for Android SDK > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

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
    // Consume API : https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    @NonNull
    String paymentSeq;
    
    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    //
    // Consume API : https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
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

<a id="list-purchasable-items"></a>
### List Purchasable Items { #list-purchasable-items }

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

<a id="list-non-consumed-items"></a>
### List Non-Consumed Items { #list-non-consumed-items }

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

<a id="list-activated-subscriptions"></a>
### List Activated Subscriptions { #list-activated-subscriptions }

List activated subscriptions for a current user ID. 
Paid subscriptions (auto-renewable subscription, or auto-renewed consumable subscription) can be queried before they're expired. 
For subscription life cycle, refer to the following.
[Google Play Store Subscription (Regular Payment) Feature > Subscription Lifecycle Handling](#subscription-lifecycle-handling)

> <font color="red">[Caution]</font><br/>
>
> Subscription products currently only support the Google Play Store for Android.
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


<a id="list-status-of-subscriptions"></a>
### List Status of Subscriptions { #list-status-of-subscriptions }

You can view the status of purchased subscription products based on your current user ID.
Subscription products that have been paid for (auto-renewable subscriptions, auto-renewable consumable subscription products) can be viewed until they expire.
You can retrieve the status of expired subscription products with the **PurchasableConfiguration.setIncludeExpiredSubscriptions(true)** API.
For subscription status codes, see the following.
[Subscription Status Code](#subscription-status-code)

> <font color="red">[Caution]</font><br/>
>
> * The subscription status code is only returned correctly if you follow the guide below to set up the subscription event.
>     * Go to [Game > Gamebase > Store Console Guide > Google Console Guide and set up event propagation of real-time subscription information within Google's system](./console-google-guide/#set-up-google-notifications-to-receive-real-time-subscription-status)
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
    // Subscription status code: https://docs.nhncloud.com/en/Game/Gamebase/en/aos-purchase/#subscription-status-code
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
    // Consume API: https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call the Consume API from game server!
    @NonNull
    String paymentSeq;
    
    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    //
    // Consume API: https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
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
    // * Promo: Promotion payment
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

<a id="subscription-status-code"></a>
#### Subscription Status Codes

* This is the code representing the subscription status returned by **PurchasableSubscriptionStatus.statusCode**.

| Name | Code | Status | Description |
| ---- | ---- | ------ | ----------- |
| ACTIVE | 0 | Active | The subscription is active. |
| CANCELED | 3 | Canceled | The subscription has been canceled. |
| ON_HOLD | 5 | Account Hold | The recurring payment has entered an account hold state (if enabled). |
| IN_GRACE_PERIOD | 6 | Grace Period | The recurring payment has transitioned to a grace period state (if enabled). |
| PAUSED | 10 | Paused | The subscription has been paused. |
| REVOKED | 12 | Revoked | The user canceled the recurring payment before the expiration time. |
| EXPIRED | 13 | Expired | The recurring payment has expired. |
| UNKNOWN | 9999 | Undefined | This is an undefined state. |

<a id="google-play-store-subscription"></a>
### Google Play Store Subscription (Recurring Payment) Features { #google-play-store-subscription }

This section describes how to handle lifecycle events such as renewal and expiration of subscription payments on the Google Play Store.
For more information, see [Added Features per Subscription](https://developer.android.com/google/play/billing/billing_subscriptions).

<a id="subscription-lifecycle-handling"></a>
#### Handle Subscription Lifecycle

Google Play Store subscriptions go through various status changes throughout their lifecycle, and apps must respond to each status accordingly.

* **Active**: Subscription content is accessible and auto-renewal is in progress.
* **Cancelled**: Subscription content is accessible, but the user has cancelled the subscription, so auto-renewal is suspended.
* **Grace Period (In grace period)**: The subscription payment failed due to a payment method issue, but subscription content remains accessible (the system is waiting for the user to update their payment method).
* **On Hold**: The subscription payment failed due to a payment method issue and the subscription is on hold (if a grace period is enabled, this occurs when the user does not update their payment method during the grace period, causing the payment to be placed on hold).
* **Pause**: The subscription has been temporarily paused.
* **Expired**: The subscription has expired.

| Status | Unconsumed Purchase Query<br>(NhnCloudIap.queryConsumablePurchases) | Active Subscription Query<br>(NhnCloudIap.queryActivatedPurchases) | Expiration Time | Auto-Renewal |
| --- | ------------------------------------------------- | ------------------------------------------------- | ----- | -------- |
| Active | Yes | Yes | Future time | Yes |
| Cancelled | Yes | Yes | Future time | No |
| Grace Period (In grace period) | No | Yes | Future time | Yes |
| On Hold | No | No | Past time | Yes |
| Pause | No | No | Past time | Yes |
| Expired | No | No | Past time | No |

<a id="subscription-grace-period"></a>
#### Grace Period

If grace period is enabled and there is an issue with the payment method at the end of a billing cycle, the subscription transitions to a grace period.<br/>
<span style="color:#e11d21">Users must be able to access subscription content during the grace period.</span>
For more information, see [Grace Period](https://developer.android.com/google/play/billing/subs#grace).

> <font color="red">[Caution]</font><br/>
>
> * If the subscription is restored during the grace period (for example, by updating the payment method), automatic renewal resumes.
> * NHN IAP notifies the payment result of renewed payments via the payment update listener (IapService.PurchaseUpdatedListener).
> * Games and apps must ensure that the payment update listener does not expose unnecessary pop-ups to users during critical operations.

##### Standard Subscription Products (AUTO_RENEWABLE)

* During the grace period, users must be able to access subscription content for standard subscription products.
* During the grace period, you can query these products using `NhnCloudIap.queryActivatedPurchases()`.

##### Consumable Subscription Products (CONSUMABLE_AUTO_RENEWABLE)

* When the grace period begins, Google issues a new receipt, but if the payment method is not updated, the account will enter a hold state or the subscription will be canceled.
* Consumable subscription products are not returned by `NhnCloudIap.queryConsumablePurchases()` during the grace period to prevent products from being consumed.

<a id="subscription-account-hold"></a>
#### Account Hold

Account hold refers to the user state when renewal fails due to a payment method issue.
If payment fails, it is retried during the grace period. If payment still fails during the grace period, the subscription status becomes account hold.
Users in account hold status cannot access subscription content.
The account hold period lasts up to 30 days.
If the payment method is not updated before the account hold period ends, the subscription is canceled.
For more information, see [Account Hold](https://developer.android.com/google/play/billing/subs#account-hold).

> <font color="red">[Caution]</font><br/>
>
> * If restored during the account hold period by updating the payment method or similar action, auto-renewal resumes.
> * NHN IAP notifies the payment result of renewed purchases through the purchase update listener (IapService.PurchaseUpdatedListener).
> * Games and apps must ensure that the purchase update listener does not expose unnecessary pop-ups to users during critical operations.

##### General Subscription Products (AUTO_RENEWABLE)

* During the account hold period, general subscription products cannot access subscription content.
* During the account hold period, these are not retrieved with `NhnCloudIap.queryActivatedPurchases()`.

##### Consumable Subscription Products (CONSUMABLE_AUTO_RENEWABLE)

* During the account hold period, consumable subscription products do not create new purchases.
* During the account hold period, new purchases are not retrieved with `NhnCloudIap.queryConsumablePurchases()`.

<a id="subscription-pause"></a>
#### Pause

When the pause feature is enabled, users can pause their recurring payments for a period between 1 week and 3 months.
The pause of recurring payments takes effect after the current subscription period ends.
When the pause period ends, recurring payments are automatically resumed.
For more information, see [Pause](https://developer.android.com/google/play/billing/subs#pause).

> <font color="red">[Caution]</font><br/>
>
> * When the pause period ends, auto-renewal resumes.
> * NHN IAP notifies the payment results of renewed payments via the payment update listener (IapService.PurchaseUpdatedListener).
> * Games or apps must ensure that the payment update listener does not display unnecessary popups to users during critical operations.

##### Standard Subscription Products (AUTO_RENEWABLE)

* During the pause period, standard subscription products cannot access subscription content.
* During the pause period, they are not retrieved by `NhnCloudIap.queryActivatedPurchases()`.

##### Consumable Subscription Products (CONSUMABLE_AUTO_RENEWABLE)

* During the pause period, consumable subscription products do not generate new purchases.
* During the pause period, new purchases are not retrieved by `NhnCloudIap.queryConsumablePurchases()`.

<a id="subscription-resubscribe"></a>
#### Resubscription

When the resubscription feature is enabled, users can resubscribe to a subscription that they canceled within 12 months of the subscription expiration date.
Resubscription creates a new subscription and purchase token.
After a subscription expires, users can repurchase the same product for up to one year after expiration through the Google Play subscription center.
For more information, see [Resubscription](https://developer.android.com/google/play/billing/subs#resubscribe).

> <font color="red">[Caution]</font><br/>
>
> Since the purchase does not proceed through the in-app or in-game screen, user data (`IapPurchase.getDeveloperPayload()`) cannot be used.
> <font color="red">[Caution]</font><br/>
>
> * When a subscription product is purchased through resubscription from the Google Play Store, NHN IAP notifies the purchase result through the purchase update listener (IapService.PurchaseUpdatedListener).
> * Games and apps must ensure that the purchase update listener does not expose unnecessary pop-ups to users during important operations.

<a id="event-by-purchase"></a>
### Event by Purchase { #event-by-purchase }

You can receive and handle events via the GamebaseEventHandler when an item is acquired through a promotion code or a pending payment is completed.
Refer to the guide below for instructions on handling promotion and pending payment events using the GamebaseEventHandler.
[Game > Gamebase > User Guide for Android SDK > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

<a id="error-handling"></a>
### Error Handling { #error-handling }

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | The purchase module has not been initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to the project. |
| PURCHASE_USER_CANCELED                    | 4002       | Purchase has been cancelled.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic is not completed.     |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | Product is not activated.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | You have exceeded your monthly purchase limit.             |
| PURCHASE_PENDING                          | 4008       | Additional verification is required to complete your payment. |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | The store is not supported.<br>You can choose either GG (Google), ONESTORE, GALAXY, HUAWEI, or MYCARD. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | Error in NHN IAP library.<br>Check the error details.  |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | Unknown error in purchase.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN IAP library.
* The information on the error in NHN IAP library is included in the error details, and you can find detailed error code and message as follows.

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

* For NHN IAP SDK error codes, refer to the following:
    * [NHN IAP SDK Error Codes](#nhn-iap-sdk-error-codes)

<a id="nhn-iap-sdk-error-codes"></a>
#### NHN IAP SDK Error Codes

* These are NHN IAP SDK error codes returned as the detailed error (`exception.getDetailCode()`) of **PURCHASE_EXTERNAL_LIBRARY_ERROR**.

##### Common Error Codes

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| FEATURE_NOT_SUPPORTED | -2 | Requested feature is not supported. |
| SERVICE_DISCONNECTED | -1 | Store service is not connected. |
| OK | 0 | Success. |
| USER_CANCELED | 1 | User canceled. |
| SERVICE_UNAVAILABLE | 2 | Network connection is down. |
| BILLING_UNAVAILABLE | 3 | API version is not supported for the type requested. |
| PRODUCT_UNAVAILABLE | 4 | Requested product is not available. |
| DEVELOPER_ERROR | 5 | Developer error. |
| ERROR | 6 | Fatal error during the API action. |
| PRODUCT_ALREADY_OWNED | 7 | Failure to purchase since item is already owned. |
| PRODUCT_NOT_OWNED | 8 | Failure to consume since item is not owned. |
| USER_ID_NOT_REGISTERED | 9 | User ID is not registered. |
| NETWORK_ERROR | 12 | A network error occurred during the operation. |
| UNDEFINED_ERROR | 9999 | Undefined error. |

##### Server Error Codes

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| INACTIVATED_APP | 101 | App is not active. |
| VERIFY_PURCHASE_FAILED | 103 | Failure to verify purchase. |
| PURCHASE_ALREADY_CONSUMED | 104 | Purchase already consumed. |
| PURCHASE_ALREADY_REFUNDED | 105 | Purchase already refunded. |
| PURCHASE_LIMIT_EXCEEDED | 106 | Purchase limit exceeded. |

##### ONE store Error Codes

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| ONESTORE_NEED_LOGIN | 301 | ONE store service is not logged in. |
| ONESTORE_NEED_UPDATE | 302 | ONE store service is not updated or installed. |
| ONESTORE_SECURITY_ERROR | 303 | Abnormal purchase request. |
| ONESTORE_PURCHASE_FAILED | 304 | Purchase request failed. |

##### Galaxy store Error Codes

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| GALAXY_NOT_LOGGED_IN | 501 | Galaxy service is not logged in. |
| GALAXY_NOT_UPDATED | 502 | Galaxy service is not updated or installed. |
| GALAXY_PURCHASE_FAILED | 503 | Galaxy purchase failed. |
| GALAXY_SERVICE_DENIED | 504 | Galaxy service denied. |

