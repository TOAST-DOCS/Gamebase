## Game > Gamebase > iOS Developer's Guide> Purchase

This page describes how to set In-App Purchase (IAP).

Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

#### Apple iTunes-Connect
1. Upload a tester app-build
2. Register and approve IAP
3. Register a Sandbox Tester account
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Register TOAST Console
Set TOAST Gamebase Console as follows.

1. Register a store to use at **Gamebase > Purchase (IAP) > App**.
    * Store: Select **App Store**.
2. Register an item at **Gamebase &gt; Purchase (IAP) &gt; Item**.
    * Store: Select **App Store**.
    * Store Item ID: Enter a Product ID registered at iTunes-Connect.
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

Item purchases should be implemented in the following order.<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. Call **requestPurchaseWithItemSeq:viewController:completion:** of Gamebase SDK to purchase in a game client.
2. After a successful purchase, call **requestItemListOfNotConsumedWithCompletion:** to check list of non-consumed purchases.
3. If there is a value on the returned list, the game client sends a request to the game server to consume purchased items.
4. The game server requests for Consume API to the Gamebase server via API.
    [API Guide](/Game/Gamebase/en/api-guide/#wrapping-api)
5. If the IAP server has successfully called Consume API, the game server provides the items to the game client.


A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to initialize a reprocessing logic. <br/>

1. Request list of items that are not consumed
    * When a login is successful, call **requestItemListOfNotConsumedWithCompletion:** to check list of non-consumed purchases.
    * If the value is on the returned list, the game client sends a request to the game server to consume, so that items can be provided.
2. Request to retry transaction
    * When a login is successful, call **requestRetryTransactionWithCompletion:** to try to automatically reprocess the unprocessed.
    * If there is a value on the returned successList, the game client sends a request to the game server to consume, so that items can be provided.
    * If there is a value on the returned failList, send the value to the game server or Log & Crash to collect logs. Also send inquiry to  [**TOAST > Customer Center**](https://toast.com/support/inquiry)for the cause of reprocessing failure.


### Purchase Item

Call following API of an item to purchase by using itemSeq to send a purchase request.

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


### Get a List of Non-Consumed Items

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase.<br/>
In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).

* Make a call in the following two cases.
    1. To confirm before an item is consumed after a successful purchase.
    2. To check if there is any non-consumed item left after a login is successful.


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

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST IAP server, try to reprocess by using API. <br/>
Based on the latest success of purchase, reprocessing is required by calling an API for item delivery (supply).

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



### AppStore Promotion IAP

> `주의`
> iOS 11 이상에서만 사용할 수 있습니다.
> Xcode 9.0 이상에서 빌드가 필요합니다.
> Gamebase 1.13.0 이상에서 지원합니다. (TOAST IAP SDK 1.6.0 이상적용)


> `주의`
> 로그인 성공 이후에만 호출 할 수 있습니다.
> 로그인 성공 후, 다른 결제 API보다 먼저 실행되어야 합니다.


#### Overview
* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f


AppStore 앱 내에서 아이템을 구매할 수 있는 기능을 제공합니다.
아이템 구매 성공 후, 아래의 등록해놓은 핸들러를 통하여, 아이템지급을 진행할 수 있습니다.

프로모션 IAP는 AppStore Connect 에서 별도의 설정이 되어야 노출이 가능합니다.


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


#### How to Test AppStore Promotion IAP

> `주의`
> App Store Connect에 앱을 업로드한 다음 TestFlight를 통하여 앱을 설치 후, 테스트를 진행할 수 있습니다.
> 

1. TestFlight로 App을 설치합니다.
2. 아래와 같은 URL Scheme을 호출하여, 테스트를 진행합니다.
| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 고정값 |
| host &amp; path | 없음 | 없음 |
| queries | action | purchaseIntent |
|		  | bundleId | 앱의 bundeld identifier |
|		  | productIdentifier | 구매 아이템의 product identifier |

예제) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | GamebaseAdapter is not included.<br/>If the domain of error object is "TCGB.Gamebase.TCGBPurchase", check if PurchaseAdapter exists. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | Gamebase PurchaseAdapter is not initialized.   |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | Purchase is cancelled.                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | Previous purchase is not completed.                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | Cannot purchase due to shortage of cash of the store.             |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | The store is not supported. iOS supports "AS". |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | Error in IAP library.<br/>Check error.message. |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | Unknown error in purchase.<br/>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry and we'll respond ASAP. |

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

* For IAP error codes, refer to the document below.
    * [Mobile Service > IAP > Error Code > Client API Error Type](/Mobile%20Service/IAP/en/error-code/#client-api)

