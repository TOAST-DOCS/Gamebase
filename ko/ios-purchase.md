## Game > Gamebase > iOS SDK 사용 가이드 > 결제

> <font color="red">[주의]</font><br/>
>
> Unreal, Unity 등 3rd party 결제 플러그인 또는 모듈을 사용할 경우, Gamebase 결제 기능에 영향을 줄 수 있습니다.
>

여기에서는 앱에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Settings

#### Apple iTunes-Connect
1. 테스트용 앱 빌드 업로드
2. In-App Purchases 아이템 등록 및 승인
3. Sandbox Tester 계정 등록
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Gamebase Console 등록
다음은 Gamebase Console에서 설정해야 하는 내용입니다.

1. **Gamebase > Purchase(IAP) > 스토어**에서 이용할 스토어를 등록합니다.
    * 스토어: **App Store**를 선택합니다.
2. **Gamebase > Purchase(IAP) > 상품**에서 아이템을 등록합니다.
    * 상품 ID: 결제 요청 시 사용할 상품 ID를 입력합니다.
    * 상품 이름: 결제 시 표시할 상품 이름을 입력합니다.
    * 사용 여부: 아이템의 사용여부를 결정합니다.
    * 스토어: **App Store**를 선택합니다.
    * 스토어 아이템 ID: iTunes-Connect에 등록한 Product ID를 입력합니다.
3. 아이템을 설정했다면, **저장**을 누릅니다.

#### Xcode Project 설정
1. **Targets > Capabilities > In-App Purchase**를 **ON**으로 설정합니다.
2. **Targets > General > Identity**의 Bundle Identifier, Version, Build의 값을 알맞게 설정합니다.

#### Import Header File

구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

아이템 구매는 크게 결제 Flow 와 Consume Flow, 재처리 Flow 로 나누어 볼 수 있습니다.
결제 Flow는 다음과 같은 순서로 구현하시기 바랍니다.

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 이전 결제가 정상적으로 종료되지 못한 경우 재처리가 동작하지 않으면 결제가 실패합니다. 그러므로 결제 전에 **requestItemListOfNotConsumedWithCompletion:**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 Consume Flow 를 진행합니다.
2. 게임 클라이언트에서는 Gamebase SDK의 **requestPurchaseWithGamebaseProductId:viewController:completion:**를 호출하여 결제를 시도합니다.
3. 결제가 성공하였다면 **requestItemListOfNotConsumedWithCompletion:**를 호출하여 미소비 결제 내역을 확인한 후 지급할 아이템이 존재한다면 Consume Flow 를 진행합니다.

### Consume Flow

미소비 결제 내역 목록에 값이 있으면 다음과 같은 순서로 Consume Flow 를 진행하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 아이템이 중복 지급되는 일이 발생하지 않도록, 게임 서버에서 반드시 중복 지급 여부를 체크하시기 바랍니다.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
    * UserID, gamebaseProductId, paymentSeq, purchaseToken 을 전달합니다.
2. 게임 서버는 게임 DB 에 이미 동일한 paymentSeq 로 아이템을 지급한 이력이 있는지 확인합니다.
    * 2-1. 아직 아이템을 지급하지 않았다면 Gamebase 서버의 Payment Transaction API를 호출하여 paymentSeq, purchaseToken 값이 유효한지 검증합니다.
        * [Game > Gamebase > API 가이드 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. purchaseToken이 정상적인 값이라면 UserID에 gamebaseProductId에 해당하는 아이템을 지급합니다.
    * 2-3. 아이템 지급 후 게임 DB에 UserID, gamebaseProductId, paymentSeq, purchaseToken을 저장하여 중복 지급 방지 또는 재지급을 할 수 있도록 합니다.
3. 아이템 지급 여부와 무관하게 게임 서버는 Gamebase 서버의 consume(소비) API를 호출하여 아이템 지급을 완료합니다.
    * [Game > Gamebase > API 가이드 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 스토어 결제에는 성공했으나 오류가 발생해 정상 종료되지 못하는 경우가 있습니다.
* **requestItemListOfNotConsumedWithCompletion:**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 [Consume Flow](./ios-purchase/#consume-flow) 를 진행하세요.
* 재처리는 다음과 같은 시점에 호출할 것을 권장합니다.
    * 로그인 완료 후.
    * 결제 전.
    * 게임 내 상점(또는 로비) 진입시.
    * 유저 프로필 또는 우편함 확인시.

### Purchase Item

구매하고자 하는 아이템의 gamebaseProductId를 이용해 다음의 API를 호출해 구매를 요청합니다. <br/>
gamebaseProductId는 일반적으로는 스토어에 등록한 아이템의 ID와 동일하지만, Gamebase 콘솔에서도 변경할 수도 있습니다. payload 필드에 입력한 추가 정보는 결제 성공 후 **TCGBPurchasableReceipt.payload** 필드에 유지되므로 여러가지 용도로 활용할 수 있습니다. <br/>
게임 유저가 구매를 취소하는 경우 **TCGB_ERROR_PURCHASE_USER_CANCELED** 오류가 반환됩니다. 취소 처리를 해 주시기 바랍니다.

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

// 구매한 아이템의 상품 ID
@property (nonatomic, strong) NSString *gamebaseProductId;

// 구매한 상품의 가격
@property (assign)            float price;

// 통화 코드
@property (nonatomic, strong) NSString *currency;

// 결제 식별자
// purchaseToken 과 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의: Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *paymentSeq;

// 결제 식별자
// paymentSeq 와 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의: Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *purchaseToken;

// Apple 스토어 콘솔에 등록된 상품 ID
@property (nonatomic, strong) NSString *marketItemId;

// 상품 타입
// UNKNOWN: 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객 센터로 문의하세요.
// CONSUMABLE: 소비성 상품
// AUTO_RENEWABLE: 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE: 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 상품을 구매한 User ID
// 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
@property (nonatomic, strong) NSString *userId;

// 스토어의 결제 식별자
@property (nonatomic, strong, nullable) NSString *paymentId;

// 구독이 종료되는 시각 (epoch time)
@property (nonatomic, assign) long expiryTime;

// 상품 구매 시간 (epoch time)
@property (nonatomic, assign) long purchaseTime;

// requestPurchase API 호출 시 payload 로 전달했던 값
// 이 필드는 예를 들어 동일한 User ID 로 구매 했음에도 게임 채널, 캐릭터 등에 따라 상품 구매 및 지급을 구분하고자 하는 경우 등
// 게임에서 필요로 하는 다양한 추가 정보를 담기 위한 목적으로 활용할 수 있습니다.
@property (nonatomic, strong, nullable) NSString *payload;

// 구독 상품은 갱실 될때마다 paymentId가 변경됩니다.
// 이 필드는 맨 처음 구독 상품을 결제 했을 때의 paymentId 를 알려줍니다.
// 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로 항상 유요한 값을 보장하지는 않습니다.
@property (nonatomic, strong, nullable) NSString *originalPaymentId;

// itemSeq로 상품을 구매하는 Legacy API용 식별자
@property (assign)            long itemSeq;

// sandbox 결제 여부
@property (nonatomic, assign) BOOL sandboxPayment;

// 프로모션 결제 여부
@property (nonatomic, assign) BOOL promotionPayment;

// 스토어 코드 (ex. "AS")
@property (nonatomic, strong) NSString *storeCode;

@end
```



### List Purchasable Items

아이템 목록을 조회하려면 다음 API를 호출합니다. 콜백으로 반환되는 배열(array) 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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
// requestPurchase API로 상품을 구매할 때 사용
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
// UNKNOWN: 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객 센터로 문의하세요.
// CONSUMABLE: 소비성 상품
// AUTO_RENEWABLE: 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE: 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 통화 기호가 포함된 현지화 된 가격 정보
@property (nonatomic, strong) NSString *localizedPrice;

// 스토어 콘솔에 등록된 현지화된 상품 이름
@property (nonatomic, strong) NSString *localizedTitle;

// 스토어 콘솔에 등록된 현지화된 상품 설명
@property (nonatomic, strong) NSString *localizedDescription;

// Gamebase 콘솔에서 해당 상품의 '사용 여부'
@property (nonatomic, assign, getter=isActive) BOOL active;

// itemSeq로 상품을 구매하는 Legacy API용 아이템 식별자
@property (assign) long itemSeq;

@end
```

### List Non-Consumed Items

아이템을 구매했지만, 정상적으로 아이템이 소비(배송, 지급)되지 않은 미소비 결제 내역을 요청합니다.<br/>
미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다..

* 정상적으로 결제가 완료되지 못한 경우 재처리의 역할도 하므로 다음 상황에서 호출해 주세요.
    * 게임 유저에게 지급되지 못한 아이템이 남아 있는지 확인
        * 로그인 완료 후
        * 게임 내 상점(또는 로비) 진입 시
        * 유저 프로필 또는 우편함 확인 시
    * 재처리가 필요한 아이템이 있는지 확인
        * 결제 전
        * 결제 실패 후

**API**

```objectivec
+ (void)requestItemListOfNotConsumedWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                           completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
```

#### Required 파라미터

* configuration: TCGBPurchasableConfiguration으로 미소비 결제 내역 조회에 대한 설정을 변경할 수 있습니다.
* completion: 미소비 결제 내역 조회 결과를 사용자에게 콜백으로 알려줍니다.

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

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.

**API**

```objectivec
+ (void)requestActivatedPurchasesWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                        completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
```

#### Required 파라미터

* configuration: TCGBPurchasableConfiguration으로 활성화된 구독 목록 조회에 대한 설정을 변경할 수 있습니다.
* completion: 활성화된 구독 목록 조회 결과를 사용자에게 콜백으로 알려줍니다.

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

사용자의 App Store 계정으로 구매한 내역을 기준으로 구매 내역을 복원하여 콘솔에 반영합니다.
구매한 구독 상품이 조회되지 않거나 활성화되지 않을 경우 사용합니다.
만료된 결제 건을 포함해 복원된 결제 건이 결과로 반환됩니다.
자동 갱신형 소비성 구독 상품의 경우 반영되지 않은 구매 내역이 있다면 복원 후 미소비 구매 내역에서 조회할 수 있습니다.

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

> `주의`
> iOS 11 이상에서만 사용할 수 있습니다.
> Gamebase 1.13.0 이상에서 지원합니다. (NHN Cloud IAP SDK 1.6.0 이상적용)

프로모션 결제 이벤트는 GamebaseEventHandler를 이용해 처리할 수 있습니다.
GamebaseEventHandler로 프로모션 결제 이벤트를 처리하는 방법은 아래 가이드를 확인하세요.
[Game > Gamebase > iOS SDK 사용 가이드 > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)


#### 사용시 주의 사항
Facebook SDK, Google AdMob SDK 와 같이 SDK 내에 In App Purchase (AppStore 결제) 기능이 있는 경우에는 Gamebase Login 을 하기 전에 사전결제를 시작할 경우에는 정상적으로 결제창이 나타나지 않을 수 있습니다.

* 해결 방법
  * Facebook
    * Facebook Console > 설정 > 기본 설정 > **앱 내 이벤트를 자동으로 로깅(권장)** 기능을 비활성화
    * Facebook 인증 기능을 사용하지 않을 경우: **GamebaseAuthFacebookAdapter.xcframework 파일을 제외**시킨 후 빌드


#### Overview
* Apple Developer Overview: [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference: [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


App Store 앱 내에서 아이템을 구매할 수 있는 기능을 제공합니다.
아이템 구매 성공 후, 아래의 등록해놓은 핸들러를 통하여, 아이템지급을 진행할 수 있습니다.

프로모션 IAP는 App Store Connect 에서 별도의 설정이 되어야 노출이 가능합니다.


#### How to Test App Store Promotion IAP

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
| | bundleId | 앱의 bundeld identifier |
| | productIdentifier | 구매 아이템의 product identifier |

예제) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### TCGBPurchasableConfiguration

| Parameter     | Values            | Description        |
| ------------- | ----------------- | ------------------ |
| allStores     | Bool | 동일 UserID 기준으로 API가 현재 스토어 또는 모든 스토어를 대상으로 동작하도록 설정<br>- 모든 스토어: YES<br>- 현재 스토어: NO<br>**default**: NO    |

### Error Handling

| Error                                                | Error Code | Description                              |
| ---------------------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                             | 10         | GamebaseAdapter가 포함되지 않았습니다.<br/>Error 객체의 도메인이 "TCGB.Gamebase.TCGBPurchase" 인 경우, PurchaseAdapter의 존재 유무를 확인해주시길 바랍니다. |
| TCGB_ERROR_PURCHASE_NOT_INITIALIZED                  | 4001       | Gamebase PurchaseAdapter가 초기화되지 않았습니다.   |
| TCGB_ERROR_PURCHASE_USER_CANCELED                    | 4002       | 구매가 취소되었습니다.                             |
| TCGB_ERROR_PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 이전 구매가 완료되지 않았습니다.                       |
| TCGB_ERROR_PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다.             |
| TCGB_ERROR_PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 해당 상품이 활성화 상태가 아닙니다.             |
| TCGB_ERROR_PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 존재하지 않은 GamebaseProductID로 결제를 요청하였습니다.             |
| TCGB_ERROR_PURCHASE_LIMIT_EXCEEDED                   | 4007       | 월 구매 한도를 초과했습니다.             |
| TCGB_ERROR_PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | 지원하지 않는 스토어입니다. iOS의 지원가능한 스토어는 "AS"입니다. |
| TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAP 라이브러리 오류입니다.<br/>상세 오류를 확인하십시오. |
| TCGB_ERROR_PURCHASE_UNKNOWN_ERROR                    | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud IAP 라이브러리에서 오류가 발생했을 때 반환됩니다.
* NHN Cloud IAP 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 


```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* NHN Cloud IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [NHN Cloud > NHN Cloud SDK 사용 가이드 > NHN Cloud IAP > iOS > 오류 코드](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-ios/#error-codes)

