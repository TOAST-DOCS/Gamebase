<link href="./css/gamebase.css" rel="stylesheet">

## Upcoming Products > Gamebase > iOS Developer's Guide

## Configuration

### Getting Started

#### Environments

> **[ 최소사양 ]**
> iOS8 이상
> arm7, arm7s, arm64, i386, x86_64 지원
> Xcode7 이상




#### Setting Xcode Project to use Gamebase
Gamebase는 아래와 같은 방법으로 설정이 가능합니다.

##### 1. 수동 설정
###### **1. 다운로드**
Gamebase는 [http://docs.cloud.toast.com/ko/Download/](http://docs.cloud.toast.com/ko/Download/)에서 다운로드 받습니다.
Gamebase.framework.zip 및 필요한 adapter 들을 다운로드 받습니다.
또한 각 IDP의 인증을 하기위한 SDK파일들을 다운로드 받아야합니다. 해당 IDP의 로그인을 사용할 때만 포함하면 됩니다.
다운로드 받은 뒤, 해당 SDK파일을 프로젝트의 target에 포함시켜야 합니다.

[외부 SDK 다운로드]

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | External SDK Download Link |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle, SocketRocket.framework |  |  |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | [Go to Download](https://developers.facebook.com/docs/ios/downloads) |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | [Go to Download](https://developers.payco.com/guide/sdk/download) |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework |
| Gamebase Push | GamebasePushAdapter.framework |  |

> **주의**
>
> Gamebase Framework 파일 중 이름에 **Adapter**가 포함되어 있는 파일들은 선택적으로 프로젝트 내에서 사용여부를 결정할 수 있으며, 해당 Adapter Framework를 사용하기 위해서는 위의 표에 명시된 외부 SDK들이 필요할 수 있습니다.

> **주의**
> 
>각 IDP에서 제공하는 외부 SDK에 대한 설정은 각 IDP의 가이드 문서를 참고하시길 바랍니다.

###### **2. 압축풀기**
압축을 풀면, 다음과 같이 Gamebase.framework 등의 SDK를 볼 수 있습니다.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)

###### 3. 프로젝트 설정
   1. Framework 파일을 Project의 Project Navigator로 끌어와서 import 합니다. 이 때 추가된 Framework 파일들은 프로젝트 target에 추가되어야 합니다. 
   2. **Gamebase.bundle** 파일도 **Copy Bundle Resources** 에 추가하도록 합니다.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
   3. 패키지에 포함된 **SocketRocket.framework**파일은 Gamebase에서 사용하는 WebSocket 모듈로 **Target > General > Embedded Binaries**에 추가되어야 합니다.
![SocketRocket Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-004_1.0.0.png)
   4. Gamebase를 사용하기 위해서는 Gamebase의 framework외에, Gamebase에서 사용하고 있는 외부 SDK들의 기능을 포함하기 위하여, 여러 framework와 library 파일을 linker에서 참조할 수 있도록 추가해야합니다. 아래 항목들을 추가해야합니다.
      * libz.tbd
      * libsqlite3.tbd
      * libstdc++.tbd
      * AdSupport.framework
      * ImageIO.framework
      * GameKit.framework
      * StoreKit.framework
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
   5. **Target > Build Settings > Linking > Other Linker Flags**에 **-ObjC**를 추가해야 합니다.
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)

> **Information**
>
> Linker에 **-ObjC**옵션 설정은 Static Library에 있는 모든 Objective-C class와 category를 로드하도록 합니다.
> 따라서 이 옵션을 설정하지 않았을 때에 **selector not recognized**와 같은 오류가 발생할 수 있습니다.




### Initialization

#### 1. 앱델리게이트에 필수 헤더파일 불러오기
먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.
AppDelegate.h 에서 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```


#### 2. 초기화 메소드 호출
**application:didFinishLaunchingWithOptions:** 메소드에서, 다음과 같이 초기화를 진행합니다.

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration setShowBlockingPopup:YES];

    [TCGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```

### Lifecycle 관리를 위한 이벤트 처리
iOS의 App Event를 관리하기 위하여 아래에 명기된 **UIApplicationDelegate** protocol을 구현해야합니다.

#### 1. URL Resource를 받기 위한 Event
**application:openURL:sourceApplication:annotation:** 메소드를 호출하여, Switching App을 사용한 인증 시, 각 IDP들의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

>**주의**
>
>UIApplicationDelegate의 **application:openURL:options:**를 이미 Overriding 했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[UIApplication sharedApplication] setApplicationIconBadgeNumber:0];
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

#### 2. 앱 활성화 Event
**applicationDidBecomeActive:** 메소드를 호출하여, App이 활성화 되었는지 여부를 각 IDP의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

#### 3. 앱의 Background로 전환 Event
**applicationDidEnterBackground** 메소드를 호출하여, App이 Background로 전환되었는지 알려주어야 합니다.

```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

#### 4. 앱의 Foreground로 전환 Event
**applicationWillEnterForeground** 메소드를 호출하여, App이 Foreground로 전환된다는 것을 알려주어야 합니다.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```







### Login
Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
AuthAdapter 및 3rd-Party SDK에 대한 설정은 위에 있는 '외부 SDK 다운로드' 링크를 참고하시길 바랍니다.


로그인을 시도하려는 IDP별로, additionalInfo 파라미터를 입력해주어야 하는 경우가 있습니다.
AdditionalInfo에 대한 설명은 하단의 'Gamebase에서 지원 중인 IDP' 항목을 참고합니다.

#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
로그인을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### 2. 최종 로그인 API 호출
특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.
가장 최근에 로그인한 IDP로의 로그인을 시도합니다. 해당 로그인에 대한 토큰이 만료되었거나,
토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다. 이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.

```objectivec
- (void)automaticLogin {
    // Last Logged In Provider Name
    NSString *lastLoggedInProvider = [TCGamebase lastLoggedInProvider];

    [TCGamebase loginForLastLoggedInProviderWithCompletion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithCompletion: or Notify to user -\n\terror[%@]", [error description]);
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");

                [TCGamebase loginWithType:lastLoggedInProvider viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                    if ([TCGamebase isSuccessWithError:error] == YES) {
                        NSLog(@"Login is succeeded.");
                    }
                    else {
                        NSLog(@"Login is failed.");
                    }
                }];
            }
        }
    }];
}
```

#### 3. 특정 IDP 로그인 API 호출
특정 IDP 로그인 호출을 위해서 **[TCGamebase loginWithType:viewController:completion:]** 메소드를 호출해줍니다. 로그인 결과로 **(TCGBError *)error** 객체를 이용해 성공 여부를 판단할 수 있습니다. 또한 **TCGBAuthToken** 객체를 이용하여 userId 등의 사용자 정보 및 토큰 정보를 얻을 수 있습니다.

몇몇 IDP의 로그인시에는 필수적으로 들어가야하는 정보가 있습니다. 예를 들어, facebook 로그인을 구현하기 위해서는 scope 등을 설정해주어야합니다. 이러한 필수 정보들을 설정해주기 위해서, **[TCGamebase loginWithType:additionalInfo:viewController:completion:]** API를 제공합니다.
파라미터 additionalInfo에 필수 정보들을 Dictionary 형태로 입력하시면 됩니다.
(파라미터 값이 nil일 때는, TOAST Cloud Console에 등록한 additionalInfo 값으로 채워집니다. 파라미터 값이 있을 때는 Console에 등록해놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.)

> **Information**
> iOS에서 지원하는 IDP는 **TCGBConstants.h**의 TCGBAuthIDPs 영역의 **kTCGBAuthXXXXXX**로 정의되어 있습니다.

```objectivec
- (void)loginFacebookButtonClick {
    [TCGamebase loginWithType:kTCGBAuthPayco viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember.userId];
        } else {
            // To Login Failed
        }
    }];
}
```

##### Gamebase에서 지원 중인 IDP

###### 1. Guest
###### 2. Facebook
1. AdditionalInfo의 설정이 필요합니다.
	* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Facebook의 경우, OAuth 인증 시도 시, Facebook으로 부터 요청할 정보의 종류를 설정해야 합니다. 
	* 예제
	```json
	{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
	```
2. Facebook SDK를 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다.
[](http://)

###### 3. Payco
1. AdditionalInfo의 설정이 필요합니다.
	* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Payco의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**의 설정이 필요합니다.
	* 예제
	```json
    { "service_code": "HANGAME", "service_code": "Your Service Name" }
    ```

###### 4. GameCenter









### Logout

#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
로그아웃을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### 2. 로그아웃 API 호출
로그아웃 버튼을 클릭하였을 때, 다음의 로그아웃 API를 구현합니다.

```objectivec
[TCGamebase logoutWithCompletion:^(TCGBError *error) {
    if ([TCGamebase isSuccessWithError:error] == YES) {
        // To Logout Succeeded
    } else {
        // To Logout Failed
    }
}];
```









### Withdraw

#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
탈퇴를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### 2. 탈퇴 API 호출
탈퇴 버튼을 클릭하였을 때, 다음의 탈퇴 API를 구현합니다.

```objectivec
[TCGamebase withdrawWithCompletion:^(TCGBError *error) {
    if ([TCGamebase isSuccessWithError:error] == YES) {
        // To Withdrawal Succeeded
    } else {
        // To Withdrawal Failed
    }
}];
```

### Mapping
Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때,
각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

Mapping 에는 Mapping 추가/해제 API 2개가 있습니다.


#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
Mapping을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```



#### 2 Mapping 추가 API 호출
특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면,
**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER** 에러를 리턴합니

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다. 즉, Mapping은 단순히 IDP를 연동만 해줍니다.

아래의 예시에서는 facebook에 대해서 Mapping을 시도하고 있습니다.

```objectivec
[TCGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
    if ([TCGamebase isSuccessWithError:error] == YES) {
        // To Add Mapping Succeeded
    } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
        // User Already Mapped Facebook to other account.
    } else {
        // To Add Mapping Failed cause of the error
    }
}
}];
```

#### 3. Mapping 해제 API 호출
특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다. 연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

```objectivec
[TCGamebase removeMappingWithType:@"facebook" completion:^(TCGBError *error) {
    if ([TCGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}
}];
```









### Purchase

#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### 2. 아이템 구매 API 호출
구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.

```objectivec
- (void)purchasingItem:(long)itemSeq {
    [TCGBPurchase requestPurchaseWithItemSeq:itemSeq viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
        if ([TCGamebase isSuccessWithError:error] == YES) {
            // To Purchase Item Succeeded
        } else if (error.code == TCGB_ERROR_PURCHASE_USER_CANCELED) {
            // User Canceled Purchasing Item
        } else if (error) {
            // To Purchase Item Failed cause of the error
        }
    }];
}
```

#### 3. 아이템 목록 조회 API 호출
아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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


#### 4. 미소비 결제내역 조회 API 호출
아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 **미소비 결제내역**을 요청합니다. 해당 내역을 받은 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

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

#### 5. 결제 실패건 재처리 API 호출
스토어 결제는 정상적으로 이루어졌지만, ToastCloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에,
해당 API를 이용하여 재처리를 시도합니다. 최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.
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







### Push

#### 1. 뷰컨트롤러에 필수 헤더파일 불러오기
구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### 2. Push 등록 API 호출
다음의 API를 호출하여, ToastCloud Push에 해당 사용자를 등록합니다.
Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을 사용자로부터
받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];

}
```

#### 3. Push 설정 조회
사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다.
콜백으로 오는 TCGBPushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryPushWithCompletion:^(TCGBPushConfiguration *configuration, TCGBError *error) {
        if ([TCGamebase isSuccessWithError:error] == NO) {
            // To Request Push Configuration Failed.
        }

        BOOL enablePush = configuration.pushEnabled;
        BOOL enableAdPush = configuration.ADAgreement;
        BOOL enableAdNightPush = configuration.ADAgreementNight;

        // You can handle these variables.
    }];
}
```






### UI (향후 지원 예정)






## Error code

| Category | Sub Category | Error | Error Code | Notes |
| --- | --- | --- | --- | --- |
|Common| | TCGB_ERROR_NOT_INITIALIZED | 1 | |
|      | | TCGB_ERROR_NOT_LOGGED_IN | 2 | |
|      | | TCGB_ERROR_INVALID_PARAMETER | 3 | |
|      | | TCGB_ERROR_INVALID_JSON_FORMAT | 4 | |
|      | | TCGB_ERROR_USER_PERMISSION | 5 | |
|      | | TCGB_ERROR_NOT_SUPPORTED | 10 | |
|      | | TCGB_ERROR_NOT_SUPPORTED_ANDROID | 11 | |
|      | | TCGB_ERROR_NOT_SUPPORTED_IOS | 12 | |
| Network | Socket | TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT | 101 | |
|         | | TCGB_ERROR_SOCKET_ERROR | 110 | |
|         | | TCGB_ERROR_UNKNOWN_ERROR | 999 | |
| Launching | | TCGB_ERROR_LAUNCHING_SERVER_ERROR | 2001 | |
| Auth | Common | TCGB_ERROR_AUTH_USER_CANCELED | 3001 | |
|      |        | TCGB_ERROR_AUTH_NOT_SUPPORTED_PROVIDER | 3002 | |
|      |        | TCGB_ERROR_AUTH_NOT_EXIST_MEMBER | 3003 | |
|      |        | TCGB_ERROR_AUTH_INVALID_MEMBER | 3004 | |
|      |        | TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | |
|      | Gamebase Login | TCGB_ERROR_AUTH_TAP_LOGIN_FAILED | 3101 | |
|      |          | TCGB_ERROR_AUTH_TAP_LOGIN_INVALID_TOKEN_INFO | 3102 | |
|      |          | TCGB_ERROR_AUTH_TAP_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | |
|      | IDP Login | TCGB_ERROR_AUTH_IDP_LOGIN_FAILED | 3201 | |
|      |           | TCGB_ERROR_AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | |
|      | Add Mapping | TCGB_ERROR_AUTH_ADD_MAPPING_FAILED | 3301 | |
|      |            | TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | |
|      |            | TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | |
|      |            | TCGB_ERROR_AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | |
|      | Remove Mapping | TCGB_ERROR_AUTH_REMOVE_MAPPING_FAILED | 3401 | |
|      |               | TCGB_ERROR_AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | |
|      |               | TCGB_ERROR_AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | |
|      | Logout | TCGB_ERROR_AUTH_LOGOUT_FAILED | 3501 | |
|      | Withdrawal | TCGB_ERROR_AUTH_WITHDRAW_FAILED | 3601 | |
|      | Not Playable | TCGB_ERROR_AUTH_NOT_PLAYABLE | 3701 | |
|      | Unknown | TCGB_ERROR_AUTH_UNKNOWN_ERROR | 3999 | |
| Purchase | | TCGB_ERROR_PURCHASE_NOT_INITIALIZED | 4001 | |
|          | | TCGB_ERROR_PURCHASE_USER_CANCELED | 4002 | |
|          | | TCGB_ERROR_PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | |
|          | | TCGB_ERROR_PURCHASE_NOT_SUPPORTED_MARKET | 4010 | |
|          | | TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | |
|          | | TCGB_ERROR_PURCHASE_UNKNOWN_ERROR | 4999 | |
| Push | | TCGB_ERROR_PUSH_NOT_REGISTERED | 5001 | |
|      | | TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | |
|      | | TCGB_ERROR_PUSH_UNKNOWN_ERROR | 5999 | |
| UI | | TCGB_ERROR_UI_UNKNOWN_ERROR | 6999 | |
| Server | | TCGB_ERROR_SERVER_INTERNAL_ERROR | 8001 | |
|        | | TCGB_ERROR_SERVER_REMOTE_SYSTEM_ERROR | 8002 | |
|        | | TCGB_ERROR_SERVER_UNKNOWN_ERROR | 8999 | |
| Platform Reserved | | TCGB_ERROR_INVALID_INTERNAL_STATE | 11001 | |
|                   | | TCGB_ERROR_NOT_CALLABLE_STATE | 11002 | |


## API Reference
SDK 내에 포함되어 있습니다.