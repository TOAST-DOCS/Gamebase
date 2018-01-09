## Game > Gamebase > iOS Developer's Guide > Purchase

## Purchase

This page describes how to set in-app purchase (IAP).

Gamebase provides an integrated purchase API to easily integrate in-app purchase of many stores in a game.

### Settings

#### apple iTunes-Connect
1. Upload a tester app-build
2. Register and approve IAP
3. Register a Sandbox Tester account
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Register TOAST Cloud Console
Below is to be set in the TOAST Cloud Console.

1. Register a store to use at **Gamebase > Purchase(IAP) > App**.
  * Store: Select **App Store**.
2. Register an item at **Gamebase > Purchase(IAP) > Item**.
  * Store: Select **App Store**.
  * Store Item ID: Enter a Product registered at ID iTunes-Connect.
3. When item setting is completed, press **Save**.

#### Set Xcode Project
1. Set **ON** for **Targets > Capabilities > In-App Purchase**.
2. Set appropriate values for Bundle Identifier, Version, and Build at **Targets > General > Identity**.

#### Import Header File

Import the following header to ViewController to implement purchase API.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

Purchase of items should be implemented in the following order. <br/>


![purchase flow](purchase_flow.png)

1. Call **requestPurchaseWithItemSeq:viewController:completion:** of Gamebase SDK in a game client to try to purchase.
2. After a successful purchase, call **requestItemListOfNotConsumedWithCompletion:** to check list of non-consumed purchases.
3. If the value is on the returned list, the game client sends a request to the game server to consume the purchased item.
4. The game server request for Consume API to the Gamebase server via API.
   [API Guide](http://docs.cloud.toast.com/ko/Game/Gamebase/ko/Server%20Developer%60s%20Guide/#wrapping-api)
5. If the IAP server has successfully called for Consume API, the game server provides the item to the game client.

A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to implement a reprocessing logic. <br/>

1. Delivery Request of Unprocessed Items
  * When a login is successful, call **requestItemListOfNotConsumedWithCompletion:** to check list of non-consumed purchases.
  * If the value is on the returned list, the game client sends a request to the game server to consume, and the item is provided.
2. Reprocess Error in Purchase
    * When a login is successful, call **requestRetryTransactionWithCompletion:**to try to automatically reprocess the unprocessed.
    * If there is a value on the returned successList, the game client sends a request to the game server to consume, and the item is provided.
    * If there is a value on the returned failList, send the value to the game server or Log & Crash to secure data. Also contact [Customer Center](https://cloud.toast.com/support/faq) for the cause of reprocessing failure.

### Purchase Item

Call following API of an item to purchase, using itemSeq to send a purchase request.

```objectivec
- (void)purchasingItem:(long)itemSeq {
    [TCGBPurchase requestPurchaseWithItemSeq:itemSeq viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
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



### Get a List of Purchasable Items

To retrieve the list of items, call the following API. In the array of callback return, information of each item is included.

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


### Get a List of Non-Consumed Items

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase. <br/>
In case of non-purchased items, ask the game server (item server) to process delivery (supply) of the items.


* Make a call in the following two cases.
1. To confirm before an item is consumed after a successful purchase
2. To check if there is any non-consumed item left after a login is successful

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



### Reprocess Failed Purchase Transaction

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST Cloud IAP server, try to reprocess by using API. <br/>
Based on the latest success of purchase, API call is required for item delivery (supply).


```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestRetryTransactionWithCompletion:^(TCGBPurchasableRetryTransactionResult *transactionResult, TCGBError *error) {
        if (error != nil) {
            // To Retry Failed Purchasing Transaction Failed cause of the error
            return;
        }

        // Should Deal With This Retry Transaction Result.
        // You may send result to your gameserver and add item to user.
    }];
}
```



### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | GamebaseAdapter has not been included. <br/> If the domain of error object is "TCGB.Gamebase.TCGBPurchase", check if PurchaseAdapter exists. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | Gamebase PurchaseAdapter has not been initialized.   |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | Purchase has been candelled.                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase has not been completed.                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | Cannot purchase due to shortage of case of the store.             |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | The store is not supported. iOS supports "AS". |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | Error in IAP library. Check <br>error.message. |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | The purchase error is undefined. <br> Please upload the entire logs to the Customer Center and we'll respond ASAP. |

* Refer to the following document for the entire error code.
  - [Entire Error Codes](./error-codes#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurred at an IAP module.
* The error code is as below.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* For IAP error codes, refer to the document below.
  - [IAP > Error Code Guide > Client API Error Type](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)
