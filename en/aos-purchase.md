## Game > Gamebase > Android Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

#### 1. Store Console

* Refer to the IAP guide as below, to register an app to each store and get an Appkey.
* [IAP > Store interlocking information](/Mobile%20Service/IAP/en/Store%20interlocking%20information/)

#### 2. Register as Store's Tester

* Register a tester to each store for purchase testing.
    * Google
        * [Android > Setting test purchase](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > In-app purchase test](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * For testing, be sure to register device phone number you want a sandbox for, with In-app Information-Test button.
        * A tester device requires USIM, with registered phone number (MDN).
        * Needs **ONE store** application installed.

#### 3. Using TOAST IAP Service

* Refer to the IAP guide to set and register IAP.
    * [IAP > Getting Started](/Mobile%20Service/IAP/en/console-guide/)

#### 4. Download

* Add the **gamebase-adapter-purchase-iap** folder from downloaded SDK to your project.
    * If ONE store purchase is not required, you may delete the **iap-onestore-x.x.x.aar** file.
    * If you need ONE store purchase, the jar file above should be included to the project to build.

#### 5. AndroidManifest.xml(ONE store only)

* Add the following setting to use ONE store.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!-- If the Version is 16.XX.XX, android:value should be "4". https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development / release -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Call **setStoreCode()** of configuration to initialize Gamebase.
* Select a **STORE_CODE** among the following values.
    * GG: Google
    * ONESTORE: ONE store
    * TEST: For IAP testing


```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setStoreCode(STORE_CODE)	// Must declare a store code.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Purchase Flow

Item purchases should be implemented in the following order.<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. Call **requestPurchase** of Gamebase SDK to purchase in a game client.
2. After a successful purchase, call **requestItemListOfNotConsumed** to check the list of non-consumed purchases.
3. If there is a value on the returned list, the game client sends a request to the game server to consume purchased items.
4. The game server requests for Consume API to the Gamebase server via API.
   [API Guide](/Game/Gamebase/en/api-guide/#wrapping-api)
5. If the IAP server has successfully called Consume API, the game server provides the items to the game client.

A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to initialize a reprocessing logic. <br/>

1. Request list of items that are not consumed
    * When a login is successful, call **requestItemListOfNotConsumed** to check list of non-consumed purchases.
    * If the value is on the returned list, the game client sends a request to the game server to consume, so that items can be provided.

2. Request to retry transaction
    * When a login is successful, call **requestRetryTransaction** to try to automatically reprocess the unprocessed.
    * If there is a value on the returned successList, the game client sends a request to the game server to consume, so that items can be provided.
    * If there is a value on the returned failList, send the value to the game server or Log & Crash to collect logs. Also send inquiry to **[TOAST > Customer Center](https://toast.com/support/inquiry)** for the cause of reprocessing failure.

### Purchase Item

Call following API of an item to purchase by using itemSeq to send a purchase request. <br/>
When a game user cancels purchasing, the **GamebaseError.PURCHASE_USER_CANCELED** error will be returned. Please proceed with cancellation.

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(Activity activity, long itemSeq, GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
long itemSeq; // The itemSeq value can be got through the requestItemListPurchasable API.

Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
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

### Get a List of Purchasable Items

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

### Get a List of Non-Consumed Items

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase.<br/>
In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).

* Make a call in the following two cases.
    1. To confirm before an item is consumed after a successful purchase
    2. To check if there is any non-consumed item left after a login is successful

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

### Reprocess Failed Purchase Transaction

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST Cloud IAP server, try to reprocess by using API. <br/>
Based on the latest success of purchase, reprocessing is required by calling an API for item delivery (supply).

**API**

```java
+ (void)Gamebase.Purchase.requestRetryTransaction(Activity activity, GamebaseDataCallback<PurchasableRetryTransactionResult> callback);
```

**Example**

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request retry transaction failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | The purchase module is not initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to the project. |
| PURCHASE_USER_CANCELED                   | 4002       | Purchase is cancelled.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic is not completed.     |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | Cannot purchase due to shortage of cash of the store.             |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | The store is not supported.<br>You can choose either GG (Google), TS (ONE Store), or TEST. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | Error in IAP library.<br>Check detail codes.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | Unknown error in purchase.<br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we will respond ASAP. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurs at an IAP module.
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

* For IAP error codes, refer to the document below.
    * [IAP > Error Code Guide > Client API Error Type](/Mobile%20Service/IAP/en/error-code/#client-api#client-api-errors)

`Last Update: 2019.05.28`