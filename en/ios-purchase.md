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

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous payment has not properly been completed, the payment will fail when the reprocessing starts. Therefore, you should call **requestItemListOfNotConsumedWithCompletion:** to trigger reprocessing before payment, so as to perform Consume Flow if there are items not issued yet.
2. In the game client, call **requestPurchaseWithGamebaseProductId:viewController:completion:** of Gamebase SDK to attempt payment.
3. If the payment has successfully been made, call **requestItemListOfNotConsumedWithCompletion:** to check the consumable purchases history. If there are items to be provided, perform Consume Flow.

### Consume Flow

If there's a value on the list of non-consumable purchases, execute Consume Flow in the following order:

> <font color="red">[Caution]</font><br/>
>
> To prevent the multiple issuance of the same purchased item, always check the game server for issuance history of items.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. The game client requests the game server to consume the purchased items.
    * UserID, gamebaseProductId, paymentSeq, and purchaseToken are passed.
2. Game server checks the game DB to see if there is any history of the items being issued with the identical paymentSeq.
    * 2-1. If the item has not been issued yet, issue the item belonging to the gamebaseProductId to the UserID.
    * 2-2. After issuing the item, save the UserID, gamebaseProductId, paymentSeq, and purchaseToken in the game DB to prevent provisioning or to allow reprovisioning.
3. Regardless of the item issuance, the game server should call Consume API from the Gamebase server to complete the item issuance.
    * [Game > Gamebase > API Guide > Purchase (IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* There are cases where the store purchase has successfully been made but the process was not properly completed due to errors.
*If there are items not issued yet, call **requestItemListOfNotConsumedWithCompletion:** to trigger reprocessing to finish [Consume Flow](./ios-purchase/#consume-flow).
* It is recommended to call reprocessing:
    * after login.
    * before payment.
    * when entering the in-game store (or lobby).
    * when checking the user profile or mailbox.

### Purchase Item

With gamebaseProductId of an item to purchase, call the following API to request for purchase.   <br/>The gamebaseProductId is generally same as the ID of item registered at store, but it could be changed on Gamebase console. 
Additional information for the payload field remains at the **TCGBPurchasableReceipt.payload** field after a successful payment, and therefore, can be applied to many purposes. <br/>
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

// Legacy API
+ (void)requestPurchaseWithItemSeq:(long)itemSeq 
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

// 구매한 아이템의 상품 ID
@property (nonatomic, strong) NSString *gamebaseProductId;

// 구매한 상품의 가격
@property (assign)            float price;

// 통화 코드
@property (nonatomic, strong) NSString *currency;

// 결제 식별자
// purchaseToken 과 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의 : Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *paymentSeq;

// 결제 식별자
// paymentSeq 와 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의 : Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *purchaseToken;

// Apple 스토어 콘솔에 등록된 상품 ID
@property (nonatomic, strong) NSString *marketItemId;

// 상품 타입
// UNKNOWN : 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객센터로 문의하세요.
// CONSUMABLE : 소비성 상품
// AUTO_RENEWABLE : 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 상품을 구매한 User ID
// 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
@property (nonatomic, strong) NSString *userId;

// 스토어의 결제 식별자
@property (nonatomic, strong) NSString *paymentId;

// 구독이 종료되는 시각 (epoch time)
@property (nonatomic, assign) long expiryTime;

// 상품 구매 시간 (epoch time)
@property (nonatomic, assign) long purchaseTime;

// requestPurchase API 호출 시 payload 로 전달했던 값
// 이 필드는 예를 들어 동일한 User ID 로 구매 했음에도 게임 채널, 캐릭터 등에 따라 상품 구매 및 지급을 구분하고자 하는 경우 등
// 게임에서 필요로 하는 다양한 추가 정보를 담기 위한 목적으로 활용할 수 있습니다.
@property (nonatomic, strong) NSString *payload;

// 구독 상품은 갱실 될때마다 paymentId 가 변경됩니다.
// 이 필드는 맨 처음 구독 상품을 결제 했을 때의 paymentId 를 알려줍니다.
// 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로 항상 유요한 값을 보장하지는 않습니다.
@property (nonatomic, strong) NSString *originalPaymentId;

// itemSeq 로 상품을 구매하는 Lecacy API 용 식별자
@property (assign)            long itemSeq;

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

// Gamebase 콘솔에 등록된 상품 ID
// requestPurchase API 로 상품을 구매할 때 사용
@property (nonatomic, strong) NSString *gamebaseProductId;

// 상품 가격
@property (assign) float price;

// 통화 코드
@property (nonatomic, strong) NSString *currency;

// Gamebase 콘솔에 등록된 상품 이름
@property (nonatomic, strong) NSString *itemName;

// 스토어 코드 ("AS")
@property (nonatomic, strong) NSString *marketId;

// Apple 스토어 콘솔에 등록된 상품 ID
@property (nonatomic, strong) NSString *marketItemId;

// 상품 타입
// UNKNOWN : 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객센터로 문의하세요.
// CONSUMABLE : 소비성 상품
// AUTO_RENEWABLE : 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 통화 기호가 포함된 현지화 된 가격 정보
@property (nonatomic, strong) NSString *localizedPrice;

// 스토어 콘솔에 등록된 현지화된 상품 이름
@property (nonatomic, strong) NSString *localizedTitle;

// 스토어 콘솔에 등록된 현지화된 상품 설명
@property (nonatomic, strong) NSString *localizedDescription;

// Gamebase 콘솔에서 해당 상품의 '사용 여부'
@property (nonatomic, assign, getter=isActive) BOOL active;

// itemSeq로 상품을 구매하는 Lecacy API 용 아이템 식별자
@property (assign) long itemSeq;

@end
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

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST IAP server, try to reprocess by using API. <br/>
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
> Supported in Gamebase 1.13.0 or later. (NHN Cloud IAP SDK 1.6.0 or later applied)


> `Caution`
> It can be called only after a successful login.
> After a successful login, it must be executed ahead of any other payment APIs.

#### Caution for Usage
If In-App Purchase (for App Store) is included to SDK, like Facebook SDK or Google AdMob SDK, and advance payment begins even before login to Gamebase, a payment popup may not show up. 

* Solution 
  * Facebook
    * Facebook Console > 설정 > 기본 설정 > **앱 내 이벤트를 자동으로 로깅(권장)** 기능을 비활성화
    * Facebook 인증 기능을 사용하지 않을 경우 : **GamebaseAuthFacebookAdapter.framework 파일을 제외** 시킨 후 빌드


#### Overview
* Apple Developer Overview : [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference : [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


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
    * [NHN Cloud > User Guide for NHN CLoud SDK > NHN Cloud IAP > iOS > Error Codes](/TOAST/ko/toast-sdk/iap-ios/#_15)

