## Game > Gamebase > iOS Developer's Guide> Purchase

> <font color="red">[Caution]</font><br/>
>
> If you use 3rd party payment plugins or modules such as those for Unreal or Unity, it might affect the Gamebase purchase functionality.
>

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

#### Apple iTunes-Connect
1. Upload a tester app-build
2. Register and approve IAP
3. Register a Sandbox Tester account
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Register at Gamebase Console
See the following for the setup on Gamebase Console.

1. Go to **Gamebase > Purchase (IAP) > Store** and register a store. 
    * Store: Select **App Store**.
2. On **Gamebase > Purchase(IAP) > Product**, register an item. 
    * Product ID: Enter product ID to request for purchase.
    * Product Name: Enter product name to show for purchase.
    * Use: Decide whether to use item.
    * Store: Select **App Store**.
    * Store Item ID: Enter Product ID registered for iTunes-Connect.
3. Press **Save**.

#### Set Xcode Project
1. Set **ON** for **Targets > Capabilities > In-App Purchase**.
2. Set appropriate values for Bundle Identifier, Version, and Build at **Targets > General > Identity**.

#### Import Header File

Import the following header to ViewController to implement purchase API.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
It is recommended to implement the Purchase Flow in the following order:

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous purchase was not completed normally, the purchase will fail if the reprocessing is not performed. Therefore, call **requestItemListOfNotConsumedWithCompletion:** before the purchase to run reprocessing, so that the Consume Flow is executed if there are any unprovided items.
2. The game client attempts to make a purchase by calling **requestPurchaseWithGamebaseProductId:viewController:completion:** of the Gamebase SDK.
3. If the purchase is successful, call **requestItemListOfNotConsumedWithCompletion:** to check the unconsumed purchase details, and if there is an item to provide, proceed with the Consume Flow.

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
* Call **requestItemListOfNotConsumedWithCompletion:** to run reprocessing and proceed with the [Consume Flow](./ios-purchase/#consume-flow) for any unprovided items.
* It is recommended to call reprocessing at the following times:
    * After login is completed
    * Before making a purchase
    * When entering the store (or lobby) in a game
    * When checking the user profile or mailbox

### Purchase Item

With gamebaseProductId of an item to purchase, call the following API to request for purchase.  <br/>
The gamebaseProductId is generally same as the ID of item registered at store, but it could be changed on Gamebase console.

When a game user cancels purchase, the **TCGB_ERROR_PURCHASE_USER_CANCELED** is returned. Please process cancellation. 

**API**

```objectivec
+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                              viewController:(UIViewController *)viewController
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                                     payload:(NSString *)payload 
                              viewController:(UIViewController *)viewController 
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;
```

**Example**

```objectivec
- (void)purchasingItem:(NSString *)gamebaseProductId {
    NSString *userPayload = @"USER_PAYLOAD";

    [TCGBPurchase requestPurchaseWithGamebaseProductId:gamebaseProductId payload:userPayload viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
        NSString *receivedPayload = purchasableReceipt.payload;
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Purchase Item Succeeded
        } else if (error.code == TCGB_ERROR_PURCHASE_USER_CANCELED) {
            // User Canceled Purchasing Item
        } else if (error) {
            // To Purchase Item Failed cause of the error
        }
    }];
}
```

**VO**

```objectivec
@interface TCGBPurchasableReceipt : NSObject

// The product ID of a purchased item.
@property (nonatomic, strong) NSString *gamebaseProductId;

// Price of purchased product
@property (assign)            float price;

// Currency code
@property (nonatomic, strong) NSString *currency;

// Payment identifier
// This is an important piece of information used to call 'Consume' server API with purchaseToken
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// Caution : Call Consume API from game server!
@property (nonatomic, strong) NSString *paymentSeq;

// Payment identifier
// Used to call 'Consume' server API with paymentSeq
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// Caution : Call Consume API from game server!
@property (nonatomic, strong) NSString *purchaseToken;

// The product ID registered with Apple Store console
@property (nonatomic, strong) NSString *marketItemId;

// Product type
// UNKNOWN : An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
// CONSUMABLE : A consumable product.
// AUTO_RENEWABLE : A subscription product
// CONSUMABLE_AUTO_RENEWABLE : This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
@property (nonatomic, strong) NSString *productType;

// This is a user ID with which a product is purchased
// If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
@property (nonatomic, strong) NSString *userId;

// The payment identifier of a store
@property (nonatomic, strong, nullable) NSString *paymentId;

// The time when the subscription expires (epoch time)
@property (nonatomic, assign) long expiryTime;

// The time when the product was purchased (epoch time)
@property (nonatomic, assign) long purchaseTime;

// It is the value passed to payload when calling the requestPurchase API
// This field can be used to hold a variety of additional information. For example, this field can be used to separately handle purchase
// of the products purchased using the same user ID and sort them by game channel or character.
@property (nonatomic, strong, nullable) NSString *payload;

// paymentId is changed whenever product subscription is renewed.
// This field shows the paymentId that was used when a subscription product was first purchased.
// This value does not guarantee to be always valid, as it can have no value depending on the store the user made a purchase and the status of the payment server.
@property (nonatomic, strong, nullable) NSString *originalPaymentId;

// An identifier for Legacy API that purchases products with itemSeq
@property (assign)            long itemSeq;

// Whether it is sandbox payment
@property (nonatomic, assign) BOOL sandboxPayment;

// Whether it is promotion payment
@property (nonatomic, assign) BOOL promotionPayment;

// Store code (ex. "AS")
@property (nonatomic, strong) NSString *storeCode;

@end
```



### List Purchasable Items

To retrieve the list of items, call the following API. Information of each item is included in the array of callback return.

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListPurchasableWithCompletion:^(NSArray *purchasableItemArray, TCGBError *error) {
        if (error != nil) {
            // To Request Item List Failed cause of the error
            return;
        }

        NSMutableArray *itemArrayMutable = [[NSMutableArray alloc] init];
        [purchasableItemArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            TCGBPurchasableItem *item = (TCGBPurchasableItem *)obj;

            [itemArrayMutable addObject:item];
        }];
    }];
}
```


**VO**

```objectivec
@interface TCGBPurchasableItem : NSObject

// The product ID that is registered with the Gamebase console
// Used when purchasing a product with requestPurchase API
@property (nonatomic, strong) NSString *gamebaseProductId;

// Product price
@property (assign) float price;

// Currency code
@property (nonatomic, strong) NSString *currency;

// The product name registered with the Gamebase console
@property (nonatomic, strong) NSString *itemName;

// Store code ("AS")
@property (nonatomic, strong) NSString *marketId;

// The product ID registered with Apple Store console
@property (nonatomic, strong) NSString *marketItemId;

// Product type
// UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
// CONSUMABLE: A consumable product.
// AUTO_RENEWABLE: A subscription product
// CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.

//  Localized price information with currency symbol
@property (nonatomic, strong) NSString *localizedPrice;

// The name of a localized product registered with the store console
@property (nonatomic, strong) NSString *localizedTitle;

// The description of a localized product registered with the store console
@property (nonatomic, strong) NSString *localizedDescription;

// Determines whether the product is used in the Gamebase console or not
@property (nonatomic, assign, getter=isActive) BOOL active;

// An item identifier for Legacy API that purchases products with itemSeq
@property (assign) long itemSeq;

@end
```

### List Non-Consumed Items

Request a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase.<br/>
* In case there is any non-purchased item, request the game server (item server) to proceed with item delivery (provision).

* If the purchase is not completed normally, this API also serves the reprocessing function, so call it in the following situations:
    * Check if there's any unprovided items for a game user
    	* After login is completed
    	* When entering the store (or lobby) of a game
    	* When checking the user profile or mailbox
    * Check if there's any item in need of reprocessing
    	* Before making a purchase
    	* After a purchase fails


**API**

```objectivec
+ (void)requestItemListOfNotConsumedWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                           completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
```

#### Required Parameters

* configuration: With TCGBPurchasableConfiguration, you can change the settings to query unconsumed lists.
* completion: Notifies the user of the query result of unconsumed lists through a callback.

**Example**

```objectivec
- (void)viewDidLoad {
     TCGBPurchasableConfiguration *configuration = [[TCGBPurchasableConfiguration alloc] init];

     [TCGBPurchase requestItemListOfNotConsumedWithConfiguration:configuration completion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Non-consumed Item List Failed cause of the error
            return;
        }

        // Should Deal With This Non-consumed Items.
        // Send this item list to the game(item) server for consuming item
    }];
}
```

### List Activated Subscriptions

List activated subscriptions based on the current user ID.
Subscriptions that are paid up (e.g. auto-renewable subscription, auto-renewed consumable subscription) can be listed before they are expired. 

**API**

```objectivec
+ (void)requestActivatedPurchasesWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                        completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
```

#### Required Parameters

* configuration: With TCGBPurchasableConfiguration, you can change the settings to query activated subscriptions.
* completion: Notifiy the user of the query result of activated subscriptions through a callback.

**Example**

```objectivec
- (void)viewDidLoad {
    TCGBPurchasableConfiguration *configuration = [[TCGBPurchasableConfiguration alloc] init];

    [TCGBPurchase requestActivatedPurchasesWithConfiguration:configuration completion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Activated Item List Failed cause of the error
            return;
        }

        // Should Deal With This Activated Items.
    }];
}
```

### Restore Purchase

List of purchases made by user's App Store account can be restored and applied to console. 
This feature is useful when a purchased subscription cannot be queried nor activated. 
Restored payment, including expired payment, is returned as result.
In the case of auto-renewed consumable subscriptions, any missing purchases can be queried from the unconsumed purchase history after restoration is done. 

**API**

```objectivec
+ (void)requestRestoreWithCompletion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
```

**Example**

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestRestoreWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Restore Failed cause of the error
            return;
        }

        // Should Deal With This Restored Items.
    }];
}
```

### Event by Promotion

> `Caution`
> It is available for iOS version 11 or later.
> Supported in Gamebase 1.13.0 or later. (NHN Cloud IAP SDK 1.6.0 or later applied)

The promotion payment can be processed through GamebaseEventHandler.
Please refer to the guide below for how to process the promotion payment with GamebaseEventHandler.
[Game > Gamebase > iOS SDK user guide > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)


#### Caution for Usage
If In-App Purchase (for App Store) is included to SDK, like Facebook SDK or Google AdMob SDK, and advance payment begins even before login to Gamebase, a payment popup may not show up. 

* Solution 
  * Facebook
    * Facebook Console > Setting > Default Setting > Disable the **Automatically Log In-App Events (Recommended)** feature
    * When the Facebook authentication feature is not used: **Exclude the GamebaseAuthFacebookAdapter.xcframework file** and build


#### Overview
* Apple Developer Overview : [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference : [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


It provides a function to purchase in-app items from App Store apps.
After a successful purchase of items, the items can be delivered using the handler listed below.

The promotion IAP is displayed only when an additional setting is done in App Store Connect.


#### How to Test App Store Promotion IAP

> `Caution`
> You can test your app after uploading it to the App Store Connect and installing the app with TestFlight.
>

1. Install the app with TestFlight.
2. Call the following URL scheme (scheme) to proceed the test.

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | Fixed value |
| host &amp; path | None | None |
| queries | action | purchaseIntent |
| | bundleId | bundled identifier of the app |
| | productIdentifier | product identifier of the purchased item |

E.g.) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### TCGBPurchasableConfiguration

| Parameter     | Values            | Description        |
| ------------- | ----------------- | ------------------ |
| allStores     | Bool | Set to make sure the API works for the current or all stores based on the same UserID<br>- All Stores: YES<br>- Current Store: NO<br>**default**: NO    |

### Error Handling

| Error                                                      | Error Code | Description                                                  |
| ---------------------------------------------------------- | ---------- | ------------------------------------------------------------ |
| TCGB_ERROR_NOT_SUPPORTED                                   | 10         | GamebaseAdapter is not included. <br/>If domain of the error object is "TCGB.Gamebase.TCGBPurchase", see if PurchaseAdapter exists. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED                    | 4001       | Gamebase PurchaseAdapter has not been initialized.           |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED                      | 4002       | Purchase has been cancelled.                                 |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase has not been completed.                    |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH                   | 4004       | Unable to purchase due to shortage of cash for the store.         |
| TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID               | 4005       | Product is not activated.                                    |
| TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID             | 4006       | Requested for purchase with invalid GamebaseProductID.       |
| TCGB_ERROR_PURCHASE_LIMIT_EXCEEDED                         | 4007       | You have exceeded your monthly purchase limit.               |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET              | 4010       | The store is not supported.<br />iOS supports "AS".                  |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR            | 4201       | Error in NHN Cloud IAP library.<br>Please check the error details.         |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR                      | 4999       | Unknown error in purchase.<br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we'll return at the earliest possible moment. 

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* This error occurs when an error occurs in NHN Cloud IAP library.
* The information on the error in NHN Cloud IAP library is included in the error details, and you can find detailed error code and message as follows.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


* See the guide for the NHN Cloud IAP error codes.  
    * [NHN Cloud > User Guide for NHN CLoud SDK > NHN Cloud IAP > iOS > Error Codes](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-ios/#error-codes)
