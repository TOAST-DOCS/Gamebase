## Game > Gamebase > iOS Developer's Guide> Purchase

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
You may execute an item purchase in the following order:

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. When a previous purchase has not been closed, purchase fails unless reprocessing runs. Therefore, call **requestItemListOfNotConsumedWithCompletion:** before payment so as to run reprocessing, and execute Consume Flow if there's any unsupplied item. 
2. For the game client, call **requestPurchaseWithItemSeq:viewController:completion:** of Gamebase SDK to make a purchase.
3. When a purchase has been successful, call **requestItemListOfNotConsumedWithCompletion:** and check history of non-consumable purchases; and if there's any item to supply, execute Consume Flow. 

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
* Call **requestItemListOfNotConsumedWithCompletion:** to run reprocessing and execute Consume Flow for any non-supplied items. 
* It is recommended to call reprocessing in time for the following: 
    * After login is completed
    * Before payment 
    * Entering store (or lobby) of a game
    * Checking user profile or mailbox

### Purchase Item

With gamebaseProductId of an item to purchase, call the following API to request for purchase.   <br/>The gamebaseProductId is generally same as the ID of item registered at store, but it could be changed on Gamebase console. 
Additional information for the payload field remains at the **TCGBPurchasableReceipt.payload** field after a successful payment, and therefore, can be applied to many purposes. <br/>
When a game user cancels purchase, the **TCGB_ERROR_PURCHASE_USER_CANCELED** is returned. Please process cancellation. 

**API**

```objectivec
+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId viewController:(UIViewController *)viewController completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId payload:(NSString *)payload viewController:(UIViewController *)viewController completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

// Legacy API
+ (void)requestPurchaseWithItemSeq:(long)itemSeq viewController:(UIViewController *)viewController completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;
```

**Example**

```objectivec

- (void)purchasingItem:(NSString *)gamebaseProductId {
    NSString *userPayload = @"USER_PAYLOAD";

    [TCGBPurchase requestPurchaseWithGamebaseProductId:gamebaseProductId viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
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


### List Non-Consumed Items

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase.<br/>
In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).

* When a purchase is not properly completed, reprocessing is also required; please call in time for the following: 
    * See if there's any unsupplied items for a game user
        * After login is completed 
        * Entering store (or lobby) of a game 
        * Checking user profile or mailbox
    * See if there's any item in need of reprocessing
        * Before purchase
        * After failed purchase

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListOfNotConsumedWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
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

Subscriptions that are paid up (e.g. auto-renewable subscription, auto-renewed consumable subscription) can be listed before they are expired. 
With a same user ID, all purchased subscriptions from Android and iOS can be listed.

### Reprocess Failed Purchase Transaction

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of NHN Cloud IAP server, try to reprocess by using API. <br/>
Based on the latest success of purchase, reprocessing is required by calling an API for item delivery (supply).

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestActivatedPurchasesWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
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
In the case of auto-renewed consumable subscriptions, any missing purchases can be queried from the non-consumable purchase history after restoration is done. 


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
> It must be built with Xcode 9.0 or later.
> It is supported by Gamebase 1.13.0 or later(It is applicable to NHN Cloud IAP SDK 1.6.0 or later).


> `Caution`
> It can be called only after a successful login.
> After a successful login, it must be executed ahead of any other payment APIs.

#### 사용시 주의 사항#### Caution for Usage
If In-App Purchase (for App Store) is included to SDK, like Facebook SDK or Google AdMob SDK, and advance payment begins even before login to Gamebase, a payment popup may not show up. 

* Solution 
  * Facebook
    * Facebook Console > Settings > Default Setting > Disable `Automatically log in-app events` 
    * When Facebook authentication is not in use: `Exclude GamebaseAuthFacebookAdapter.framework file` and build


#### Overview
* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f


It provides a function to purchase in-app items from App Store apps.
After a successful purchase of items, the items can be delivered using the handler listed below.

The promotion IAP is displayed only when an additional setting is done in App Store Connect.


```objectivec
- (void)didSuccessLogin {
	[TCGBPurchase setPromotionIAPHandler:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
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

#### Process Promotional Event with GamebaseEventHandler

Promotional purchase events can also be handled via GamebaseEventHandler. 
See the guide on how to process a promotional purchase event via GamebaseEventHandler. 
[Game > Gamebase > User Guide for iOS SDK > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)

### Error Handling

| Error                                                      | Error Code | Description                                                  |
| ---------------------------------------------------------- | ---------- | ------------------------------------------------------------ |
| TCGB_ERROR_NOT_SUPPORTED                                   | 10         | GamebaseAdapter is not included. <br/>If domain of the error object is "TCGB.Gamebase.TCGBPurchase", see if PurchaseAdapter exists. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED                    | 4001       | Gamebase PurchaseAdapter has not been initialized.           |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED                      | 4002       | Purchase has been cancelled.                                 |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase has not been completed.                    |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH                   | 4004       | Unable to pay due to shortage of cash for the store.         |
| TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID               | 4005       | Product is not activated.                                    |
| TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID             | 4006       | Requested for purchase with invalid GamebaseProductID.       |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET              | 4010       | Unsupported store. <br />iOS supports "AS".                  |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR            | 4201       | Error of IAP library.<br>Please check error.message.         |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR                      | 4999       | Undefined error of purchase. <br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we'll return at the earliest possible moment. 

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurs at an IAP module.
* The error code is as below.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* See the guide for the IAP error codes.  
    * [NHN Cloud > User Guide for NHN Cloud SDK > NHN Cloud IAP > iOS > Error Codes](/TOAST/ko/toast-sdk/iap-ios/#_15)

