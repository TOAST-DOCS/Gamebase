## Game > Gamebase > Android Developer's Guide  > Purchase

## Purchase

This page describes how to set an in-app purchase (IAP).  

Gamebase provides an integrated purchase API to easily integrate in-app purchase of many stores in a game. 

### Settings

#### 1. Store Console

* Register an app to each store and get an appkey, in reference of the IAP guide as below.  
* [IAP > Store interlocking information](http://docs.cloud.toast.com/ko/Common/IAP/ko/Store%20interlocking%20information/)

#### 2. Register as Store's Tester

* Register a tester for each store to test purchases.
  * Google
    * [Android > Setting test purchase](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
  * ONE store
    * [ONE store > In-app purchase test](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
    * For testing, be sure to register phone number of a device you want a sandbox for, with in-app information - Test button.
    * A tester device requires USIM, with registered phone number (MDN).
    * Needs **ONE store** application installed. 

#### 3. Use TOAST Cloud IAP 

* Refer to the IAP guide to set IAP and register a product. 
  * [IAP > Getting Started](http://docs.cloud.toast.com/ko/Common/IAP/ko/Web%20Console/)

#### 4. Download

* Add the **gamebase-adapter-purchase-iap** folder of downloaded SDK to the project. 
  * If ONE store purchase is not required, you may delete the **iap-tstore-x.x.x.jar**, **iap_tstore_plugin_vxx.xx.xx.jar** file.
  * If you need ONE store purchase, the jar file in the above should be included to the project for a build. 

#### 5. AndroidManifest.xml(ONE store only)

* Add the following setting to use ONE store.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!-- For version 16.XX.XX, enter 4. https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development:development mode/ release:operation -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Call **setStoreCode()** of configuration to initialize Gamebase. 
* Select a **STORE_CODE** among the following values.
  * GG: Google
  * TS: ONE store
  * TEST: For IAP testing 

```java
String STORE_CODE = "GG";	// Google

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
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

Purchase of items should be implemented in the following order. <br/>

![purchase flow](purchase_flow.png)

1. Call **requestPurchase** of Gamebase SDK in a game client to try to purchase. 
2. After a successful purchase, call **requestItemListOfNotConsumed** to check list of non-consumed purchases. 
3. If the value is on the returned list, the game client sends a request to the game server to consume the purchased item. 
4. The game server request for Consume API to the Gamebase server via API. 
   [API Guide](http://docs.cloud.toast.com/ko/Game/Gamebase/ko/Server%20Developer%60s%20Guide/#wrapping-api)
5. If the IAP server has successfully called for a Consume API, the game server provides the item to the game client. 

A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to implement a reprocessing logic. <br/>

1. Delivery Request of Unprocessed Items  
  * When a login is successful, call **requestItemListOfNotConsumed** to check list of non-consumed purchases. 
  * If the value is on the returned list, the game client sends a request to the game server to consume, and the item is provided. 

2. Reprocess Error in Purchase 
  * When a login is successful, call **requestRetryTransaction** to try to automatically reprocess the unprocessed.  
  * If there is a value on the returned successList, the game client sends a request to the game server to consume, and the item is provided.
  * If there is a value on the returned failList, send the value to the game server or Log & Crash to secure data. Also contact [Customer Center](https://cloud.toast.com/support/faq) for the cause of reprocessing failure.

### Purchase Item

Call following API of an item to purchase, using itemSeq to send a purchase request. <br/>
When a game user cancels purchasing, the **GamebaseError.PURCHASE_USER_CANCELED** error will be returned. Please process a cancellation. 

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

To retrieve the list of items, call the following API. In the array of callback return, information of each item is included. 

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

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase. <br/>
In case of non-purchased items, ask the game server (item server) to process delivery (supply) of the items. 

* Make a call in the following two cases. 
    1. To confirm before an item is consumed after a successful purchase 
    2. To check if there is any non-consumed item left after a login is successful  

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
Based on the latest success of purchase, API call is required for item delivery (supply). 

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
| PURCHASE_NOT_INITIALIZED                 | 4001       | The purchase module has not been initialized. <br>Check  if the gamebase-adapter-purchase-IAP module has been added to the project. |
| PURCHASE_USER_CANCELED                   | 4002       | Game user has canceled purchasing an item. |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | API has been called when a purchase logic has not been completed. |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | Cannot purchase due to shortage of cash of the store. |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | The store is not supported. <br>You can choose either GG (Google), TS (ONE Store), or TEST. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | Error in IAP library. <br>Check detail codes. |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | The purchase error is undefined. <br>Please upload the entire logs to the [Customer Center](https://cloud.toast.com/support/faq) and we'll respond ASAP. |

* Refer to the following document for the entire error code. 
  - [Entire Error Codes](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurred at an IAP module.
* Need to check IAP error codes via exception.getDetailCode().
* For IAP error codes, refer to the document below.  
  - [IAP > Error Code Guide > Client API Error Type](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)

