## Game > Gamebase > iOS SDK 사용 가이드 > 결제

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

1. **Gamebase > Purchase(IAP) > 앱**에서 이용할 스토어를 등록합니다.
    * 스토어: **App Store**를 선택합니다.
2. **Gamebase > Purchase(IAP) > 아이템**에서 아이템을 등록합니다.
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

아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.6.2.png)

1. 게임 클라이언트에서는 Gamebase SDK의 **requestPurchaseWithItemSeq:viewController:completion:**을 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 **requestItemListOfNotConsumedWithCompletion:**을 호출하여 미소비 결제 내역을 확인합니다.
3. 반환된 미소비 결제 내역 목록에 값이 있으면 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
	* UserID, itemSeq, paymentSeq, purchaseToken 을 전달합니다.
4. 게임 서버는 게임 DB 에 이미 동일한 paymentSeq, purchaseToken 으로 아이템을 지급한 이력이 있는지 확인합니다.
	* 4-1. 아직 아이템을 지급하지 않았다면 UserID 에 itemSeq 에 해당하는 아이템을 지급합니다.
    * 4-2. 아이템 지급 후 게임 DB 에 UserID, itemSeq, paymentSeq, purchaseToken 을 저장하여 이후에 중복 지급을 확인할 수 있도록 합니다.
5. 게임 서버는 Gamebase 서버에 API를 통해 consume(소비) API를 요청합니다.
	* [API 가이드](./api-guide/#consume)

<br/>

* 스토어 결제에는 성공했으나 오류가 발생해 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 미소비 결제 내역을 확인하시기 바랍니다. <br/>
	* 로그인에 성공하면 **requestItemListOfNotConsumedWithCompletion:**을 호출해 미소비 결제 내역을 확인합니다.
	* 반환된 미소비 결제 내역 목록에 값이 있다면 게임 클라이언트가 게임 서버에 consume(소비)을 요청해 아이템을 지급합니다.

### Purchase Item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매를 요청합니다.

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


### Get a List of Non-Consumed Items

아이템을 구매했지만, 정상적으로 아이템이 소비(배송, 지급)되지 않은 미소비 결제 내역을 요청합니다.<br/>
미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다..

* 다음 두 가지 상황에서 호출해 주세요.
    1. 결제 성공 후 아이템 소비(consume) 처리 전 최종 확인을 위하여 호출
    2. 로그인 성공 후 소비(consume)하지 못한 아이템이 남아 있지는 않은지 확인하기 위하여 호출


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

### Get a List of Activated Subscriptions

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
사용자 ID가 같다면 Android와 iOS에서 구매한 구독 상품이 모두 조회됩니다.


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

사용자의 App Store 계정으로 구매한 내역을 기준으로 구매 내역을 복원하여 콘솔에 반영합니다.
구매한 구독 상품이 조회되지 않거나 활성화되지 않을 경우 사용합니다.
만료된 결제 건을 포함해 복원된 결제 건이 결과로 반환됩니다.
자동 갱신형 소비성 구독 상품의 경우 반영되지 않은 구매 내역이 있다면 복원 후 미소비 구매 내역에서 조회할 수 있습니다.


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

### App Store Promotion IAP

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


App Store 앱 내에서 아이템을 구매할 수 있는 기능을 제공합니다.
아이템 구매 성공 후, 아래의 등록해놓은 핸들러를 통하여, 아이템지급을 진행할 수 있습니다.

프로모션 IAP는 App Store Connect 에서 별도의 설정이 되어야 노출이 가능합니다.


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


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | GamebaseAdapter가 포함되지 않았습니다.<br/>Error 객체의 도메인이 "TCGB.Gamebase.TCGBPurchase" 인 경우, PurchaseAdapter의 존재 유무를 확인해주시길 바랍니다. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | Gamebase PurchaseAdapter가 초기화되지 않았습니다.   |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | 구매가 취소되었습니다.                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 이전 구매가 완료되지 않았습니다.                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다.             |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 지원하지 않는 스토어입니다. iOS의 지원가능한 스토어는 "AS" 입니다. |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | IAP 라이브러리 에러입니다.<br>error.message 를 확인하세요. |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | 정의되지 않은 구매 에러입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 IAP 모듈에서 발생한 오류입니다.
* 오류 코드는 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [TOAST > TOAST SDK 사용 가이드 > TOAST IAP > iOS > 에러 코드](/TOAST/ko/toast-sdk/iap-ios/#_15)

