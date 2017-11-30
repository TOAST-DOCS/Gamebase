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

##### 1. Configuration
###### **1. Download**
Gamebase는 [http://docs.cloud.toast.com/ko/Download/](http://docs.cloud.toast.com/ko/Download/)에서 다운로드 받습니다.<br/>
Gamebase.framework.zip 및 필요한 adapter 들을 다운로드 받습니다.<br/>
또한 각 IDP의 인증을 하기위한 SDK파일들을 다운로드 받아야합니다. 해당 IDP의 로그인을 사용할 때만 포함하면 됩니다.<br/>
다운로드 받은 뒤, 해당 SDK파일을 프로젝트의 target에 포함시켜야 합니다.

[3rd Party SDK Download]

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | External SDK Download Link |
| --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle |  |  |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | [Go to Download](https://developers.facebook.com/docs/ios/downloads) |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | [Go to Download](https://developers.payco.com/guide/sdk/download) |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework |  |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | Gamebase내에 포함 |
| Gamebase Push | GamebasePushAdapter.framework |  | Gamebase내에 포함 |

> **주의**
>
> Gamebase Framework 파일 중 이름에 **Adapter**가 포함되어 있는 파일들은 선택적으로 프로젝트 내에서 사용여부를 결정할 수 있으며, 해당 Adapter Framework를 사용하기 위해서는 위의 표에 명시된 외부 SDK들이 필요할 수 있습니다.

> **주의**
> 
>각 IDP에서 제공하는 외부 SDK에 대한 설정은 각 IDP의 가이드 문서를 참고하시길 바랍니다.

###### **2. Decompression **
압축을 풀면, 다음과 같이 Gamebase.framework 등의 SDK를 볼 수 있습니다.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)

###### 3. Project Configuration
1. Framework 파일을 Project의 Project Navigator로 끌어와서 import 합니다. 이 때 추가된 Framework 파일들은 프로젝트 target에 추가되어야 합니다. 
2. **Gamebase.bundle** 파일도 **Copy Bundle Resources** 에 추가하도록 합니다.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
3. Gamebase를 사용하기 위해서는 Gamebase의 framework외에, Gamebase에서 사용하고 있는 외부 SDK들의 기능을 포함하기 위하여, 여러 framework와 library 파일을 linker에서 참조할 수 있도록 추가해야합니다. 아래 항목들을 추가해야합니다.
    * libicucore.tbd (Gamebase SDK v1.1.5 이상에서 추가)
    * libz.tbd
    * libsqlite3.tbd
    * libstdc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
4. **Target > Build Settings > Linking > Other Linker Flags**에 **-ObjC**를 추가해야 합니다.
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
5. **Target > Build Settings > Enable Bitcode**를 **No**로 설정합니다.
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> **Information**
>
> Linker에 **-ObjC**옵션 설정은 Static Library에 있는 모든 Objective-C class와 category를 로드하도록 합니다.
> 따라서 이 옵션을 설정하지 않았을 때에 **selector not recognized**와 같은 오류가 발생할 수 있습니다.




## Initialization

### 1. Import Header file into AppDelegate
먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.<br/>
AppDelegate.h 에서 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```



### 2. Initializing Method
**application:didFinishLaunchingWithOptions:** 메소드에서, 다음과 같이 초기화를 진행합니다.

> **주의**
> 
> **initializeWithConfiguration:launchOptions:completion:** 메서드는
> 초기화가 **application:didFinishLaunchingWithOptions:** 외에서도 호출이 가능합니다.

<br/>

> **주의**
>
> **initializeWithConfiguration:launchOptions:completion:** 메서드는 
> 호출되지 않은 상태에서의 다른 Gamebase API 호출에 대해서는 정상작동을 보장하지 않습니다.

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration enableLaunchingStatusPopup:YES];

    [TCGBGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```

### 3. Launching Status
Gamebase initialize 호출 결과로 런칭 상태를 확인 할 수 있습니다.<br/>
런칭 상태는 Gamebase 초기화 이후에 호출되어야합니다.

```objectivec
- (void)myMethodAfterGamebaseInitialized {
    TCGBLaunchingStatus launchingStatus = [TCGBLaunching launchingStatus];

    // You can check whether if Gamebase was initialized or not using this launchingStatus
    if (launchingStatus == 0) {
        NSLog(@"Service is not initialized.");
    }

    // After Initialize Complete
    if (launchingStatus == INSPECTING_SERVICE) {
        NSLog(@"Service in Maintenance");
    } else if (launchingStatus == IN_SERVICE) {
        NSLog(@"Service in Service");
    } else {
        ...
    }
}

```

#### Launching Status Code
| Status | Code | Description |
| --- | --- | --- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업데이트 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | 점검 중이지만 QA 유저 서비스 가능 |
| REQUIRE_UPDATE | 300 | 업데이트 필수 |
| BLOCKED_USER | 301 | 접속 차단 유저 |
| TERMINATED_SERVICE | 302 | 서비스 종료 |
| INSPECTING_SERVICE | 303 | 서비스 점검 중 |
| INSPECTING_ALL_SERVICES | 304 | 전체 서비스 점검 중 |
| INTERNAL_SERVER_ERROR | 500 | 내부 서버 에러 |


## Lifecycle Event
iOS의 App Event를 관리하기 위하여 아래에 명기된 **UIApplicationDelegate** protocol을 구현해야합니다.

### 1. OpenURL Event
**application:openURL:sourceApplication:annotation:** 메소드를 호출하여 Switching App을 사용한 인증 시, 각 IDP들의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

> **주의**
>
> UIApplicationDelegate의 **application:openURL:options:**를 이미 Overriding 했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[UIApplication sharedApplication] setApplicationIconBadgeNumber:0];
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### 2. DidBecomeActive Event
**applicationDidBecomeActive:** 메소드를 호출하여, App이 활성화 되었는지 여부를 각 IDP의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### 3. DidEnterBackground Event
**applicationDidEnterBackground** 메소드를 호출하여, App이 Background로 전환되었는지 알려주어야 합니다.

```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### 4. WillEnterForeground Event
**applicationWillEnterForeground** 메소드를 호출하여, App이 Foreground로 전환된다는 것을 알려주어야 합니다.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```







## Login
Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.<br/>
guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.<br/>
AuthAdapter 및 3rd-Party SDK에 대한 설정은 위에 있는 '외부 SDK 다운로드' 링크를 참고하시길 바랍니다.


로그인을 시도하려는 IDP별로, additionalInfo 파라미터를 입력해주어야 하는 경우가 있습니다.
AdditionalInfo에 대한 설명은 하단의 'Gamebase에서 지원 중인 IDP' 항목을 참고합니다.

### 1. Import Header file into View Controller
로그인을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### 2. Latest Login API
특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.<br/>
가장 최근에 로그인한 IDP로의 로그인을 시도합니다. 해당 로그인에 대한 토큰이 만료되었거나,
토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다.<br/>
이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.

```objectivec
- (void)automaticLogin {
    // Last Logged In Provider Name
    NSString *lastLoggedInProvider = [TCGBGamebase lastLoggedInProvider];

    [TCGBGamebase loginForLastLoggedInProviderWithCompletion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithCompletion: or Notify to user -\n\terror[%@]", [error description]);
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");

                [TCGBGamebase loginWithType:lastLoggedInProvider viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                    if ([TCGBGamebase isSuccessWithError:error] == YES) {
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

### 3. IDP Login API
특정 IDP 로그인 호출을 위해서 **[TCGBGamebase loginWithType:viewController:completion:]** 메소드를 호출해줍니다.<br/>
로그인 결과로 **(TCGBError *)error** 객체를 이용해 성공 여부를 판단할 수 있습니다. <br/>
또한 **TCGBAuthToken** 객체를 이용하여 userId 등의 사용자 정보 및 토큰 정보를 얻을 수 있습니다.<br/>

<br/><br/>
몇몇 IDP의 로그인시에는 필수적으로 들어가야하는 정보가 있습니다.<br/>
예를 들어, facebook 로그인을 구현하기 위해서는 scope 등을 설정해주어야합니다.<br/>
이러한 필수 정보들을 설정해주기 위해서, **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API를 제공합니다.<br/>
파라미터 additionalInfo에 필수 정보들을 Dictionary 형태로 입력하시면 됩니다.<br/>
(파라미터 값이 nil일 때는, TOAST Cloud Console에 등록한 additionalInfo 값으로 채워집니다. 파라미터 값이 있을 때는 Console에 등록해놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.)

> **Information!**
> 
> iOS에서 지원하는 IDP는 **TCGBConstants.h**의 TCGBAuthIDPs 영역의 **kTCGBAuthXXXXXX**로 정의되어 있습니다.

```objectivec
- (void)loginFacebookButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthPayco viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember.userId];
        } else {
            // To Login Failed
        }
    }];
}
```

#### Gamebase에서 지원 중인 IDP

##### 1. Guest
##### 2. Facebook
1. AdditionalInfo의 설정이 필요합니다.
    * **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
    * Facebook의 경우, OAuth 인증 시도 시, Facebook으로 부터 요청할 정보의 종류를 설정해야 합니다. 
    * 예제

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```
2. Facebook SDK를 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다.
[Facebook Developer Guide](https://developers.facebook.com/docs/ios/getting-started)

##### 3. Payco
1. AdditionalInfo의 설정이 필요합니다.
    * **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
    * Payco의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**의 설정이 필요합니다.
    * 예제

```json
{ "service_code": "HANGAME", "service_code": "Your Service Name" }
```

##### 4. GameCenter
TOAST Cloud Console에서의 설정 외에 추가 설정은 없습니다.



<br/>

### 4. Login with access token of external IDP
게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증을 하고 발급받은 AccessToken등을 이용하여, Gamebase 로그인을 할 수 있는 인터페이스 입니다.

* Credential 파라미터의 설정방법
    * NSDictionary 타입으로 설정합니다.
    * **kTCGBAuthLoginWithCredentialProviderNameKeyname** 키에는 idp종류를 설정합니다. (faceboo, payco, iosgamecenter)
    * **kTCGBAuthLoginWithCredentialAccessTokenKeyname** 키에는 외부 SDK로부터 받은 인증정보(AccessToken)를 입력합니다.

> **Tip!** 
> 
> 게임 내에서 외부 서비스(Facebook 등)의 고유기능의 사용이 필요할 때 사용될 수 있습니다.

<br/>

> **주의!**
> 
> 외부 SDK에서 요구하는 개발사항은 Gamebase에서는 지원이 불가능합니다.

```objectivec
#import "TCGBConstants.h"

- (void)auth_login_with_credential {
    [TCGBGamebase loginWithCredential:@{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"여기에 facebook SDK에서 발급받은 Access Token을 입력하세요" } viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```


### 5. Gets Authentication Information for external IDP
외부 인증 SDK에서 AccessToken, UserId, Profile 등의 인증 정보를 가져올 수 있습니다.

```objectivec
// Example for obtaining ID Provider's Authentication Information

// Obtaining Facebook UserID
NSString *userID = [TCGBGamebase authProviderUserIDWithIDPCode:@"facebook"];

// Obtaining Facebook AccessToken
NSString *accessTokenOfIDP = [TCGBGamebase authProviderAccessTokenWithIDPCode:@"facebook"];

// Obtaining Facebook Profile
TCGBAuthProviderProfile *providerProfile = [TCGBGamebase authProviderProfileWithIDPCode:@"facebook"];
```



## Logout

### 1. Import Header file into View Controller
로그아웃을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### 2. Logout API
로그아웃 버튼을 클릭하였을 때, 다음의 로그아웃 API를 구현합니다.

```objectivec
[TCGBGamebase logoutWithCompletion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Logout Succeeded
    } else {
        // To Logout Failed
    }
}];
```









## Withdraw

### 1. Import Header file into View Controller
탈퇴를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### 2. Widthdraw API
탈퇴 버튼을 클릭하였을 때, 다음의 탈퇴 API를 구현합니다.

```objectivec
[TCGBGamebase withdrawWithCompletion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Withdrawal Succeeded
    } else {
        // To Withdrawal Failed
    }
}];
```

## Mapping
Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.<br/>
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때,
각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

<br/>

Mapping 에는 Mapping 추가/해제 API 2개가 있습니다.


### 1. Import Header file into View Controller
Mapping을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```



### 2. Add Mapping API
특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.<br/>
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면,
**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER** 에러를 리턴합니다.

<br/><br/>

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다.<br/>
즉, Mapping은 단순히 IDP를 연동만 해줍니다.<br/>
<br/>

아래의 예시에서는 facebook에 대해서 Mapping을 시도하고 있습니다.

```objectivec
[TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Add Mapping Succeeded
    } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
        // User Already Mapped Facebook to other account.
    } else {
        // To Add Mapping Failed cause of the error
    }
}
}];
```

### 3. Remove Mapping API
특정 IDP에 대한 연동을 해제합니다. <br/>
만약, 해제하고자 하는 IDP가 **유일한 IDP**라면, 실패를 리턴하게 됩니다.<br/>
연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

```objectivec
[TCGBGamebase removeMappingWithType:@"facebook" completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}
}];
```




## Purchase

### 1. Import Header file into View Controller
구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### 2. Purchase Flow
아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.
1. **requestPurchaseWithItemSeq:viewController:completion:** 를 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 **requestItemListOfNotConsumedWithCompletion:** 를 호출하여 미소비 결제내역을 확인합니다.
3. 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume 을 요청합니다.
4. 게임 서버는 IAP서버에 서버 API를 통해 Consume 을 요청합니다.
5. IAP서버에서 Consume이 성공하였다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.

<br/><br/>


스토어 결제는 성공하였으나 에러가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 다음 두 API를 각각 호출하여 재처리 로직을 구현하시기 바랍니다.
1. 미처리 아이템 배송 요청
    1. 로그인이 성공하면 **requestItemListOfNotConsumedWithCompletion:** 를 호출하여 미소비 결제내역을 확인합니다.
    2. 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
2. 결제 오류 재처리 시도
    1. 로그인이 성공하면 **requestRetryTransactionWithCompletion:** 을 호출하여 미처리 내역에 대한 자동 재처리를 시도합니다.
    2. 리턴된 successList 에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
    3. 리턴된 failList 에 값이 존재한다면 해당 값을 게임 서버나 Log&Crash 등을 통해 전송하여 데이터를 확보하고, 빌링 개발팀에 재처리 실패 원인을 문의합니다.



### 3. Purchase Item
구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.

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

### 4. Get a list of Purchasable Items
아이템 목록을 조회하기 위하여 다음의 API를 호출합니다.<br/>
콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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


### 5. Get a list of Not-Consumed Items
아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 **미소비 결제내역**을 요청합니다.<br/>
해당 내역을 받은 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

* 다음 두가지 상황에서 호출해 주세요.
    1. 결제 성공 후 아이템 Consume 처리 전 최종 확인을 위하여 호출.
    2. 로그인 성공 후 Consume하지 못한 아이템이 남아있지는 않은지 확인 하기 위하여 호출.


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

### 6. Reprocess Failed Purchase Transaction
스토어 결제는 정상적으로 이루어졌지만, TOAST Cloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에, 해당 API를 이용하여 재처리를 시도합니다. <br/>
최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.

<br/><br/>

성공/실패 여부에 따라 각 게임별 아이템 배송 로직을 진행하거나 에러 리스트를 어떻게 관리할 것인지는 프로젝트 마다 정책이 다를 수 있으므로 Gamebase SDK는 requestRetryTransaction 를 자동으로 호출해주지 않습니다.<br/>
로그인 성공 후 직접 호출하여야 합니다.

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


### 7. Error Handling
| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| TCGB_ERROR_NOT_SUPPORTED | 10 | GamebaseAdapter가 포함되지 않았습니다.<br/>Error 객체의 도메인이 "TCGB.Gamebase.TCGBPurchase" 인 경우, PurchaseAdapter의 존재 유무를 확인해주시길 바랍니다. |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001 | Gamebase PurchaseAdapter가 초기화되지 않았습니다. |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002 | 구매가 취소되었습니다. |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003 | 이전 구매가 완료되지 않았습니다. |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004 | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다. |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010 | 지원하지 않는 스토어입니다. iOS의 지원가능한 스토어는 "AS" 입니다. |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201 | IAP 라이브러리 에러입니다.<br>error.message 를 확인하세요. |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999 | 정의되지 않은 구매 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |



####  TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR
이 에러는 IAP 모듈에서 발생한 에러입니다.<br/>
에러 코드 확인은 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // Callback 으로 넘어온 TCGBError 인스턴스
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 외부 라이브러리에서 발생한 에러객체
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 아래 [tcgbError description] 호출을 통해서 json format의 전체 에러 정보를 얻을 수 있습니다.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* IAP 에러코드는 다음 문서를 참고하시기 바랍니다.
* [IAP > Error Code Guide > Client API 에러 타입](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)





## Push

### 1. Import Header file into View Controller
구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### 2. Register Push
다음의 API를 호출하여, TOAST Cloud Push에 해당 사용자를 등록합니다.<br/>
Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을
사용자로부터 받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.

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

### 3. Get a Push Setting
사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다.<br/>
콜백으로 오는 TCGBPushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryPushWithCompletion:^(TCGBPushConfiguration *configuration, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Configuration Failed.
        }

        BOOL enablePush = configuration.pushEnabled;
        BOOL enableAdPush = configuration.ADAgreement;
        BOOL enableAdNightPush = configuration.ADAgreementNight;

        // You can handle these variables.
    }];
}
```

### 4. Error Handling
| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | TCPush 라이브러리 에러입니다.<br>DetailCode를 확인하세요. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 PUSH API 호출이 완료되지 않았습니다.<br>이전 PUSH API의 콜백이 실행된 이후에 다시 호출하세요. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR | 5999 | 정의되지 않은 푸시 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |

#### TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR
이 에러는 TOAST Cloud Push 라이브러리에서 발생한 에러입니다.<br/>
에러 코드 확인은 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // Callback 으로 넘어온 TCGBError 인스턴스
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 외부 라이브러리에서 발생한 에러객체
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 아래 [tcgbError description] 호출을 통해서 json format의 전체 에러 정보를 얻을 수 있습니다.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* TCPush 에러코드는 다음 문서를 참고하시기 바랍니다.
* [Push > Client SDK Developer's Guide > Error Code Guide > 오류 처리](http://docs.cloud.toast.com/ko/Notification/Push/ko/Client%20SDK%20Guide/#_5)



## UI

### 1. WebView
Gamebase에서는 기본적인 웹뷰를 지원합니다. 웹뷰의 스타일은 Fullscreen과 Popup 스타일을 지원하며, Customizing이 가능합니다.<br/>
Fullscreen 스타일(Browser)은 네비게이션바를 가지며, Close/GoBack 버튼을 가집니다. 네비게이션바에 타이틀을 지정할 수 있습니다.<br/>
Popup 스타일은 기존화면 위에 모달뷰 형식으로 나타나게 되며, 뒷 배경은 투명한 mask view로 덮어씌워집니다.

<br/><br/>

웹뷰와 관련된 리소스(이미지 및 html, 기타 리소스)는 Gamebase.bundle 에 포함되어있습니다.

```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    [TCGBWebView showWebBrowserWithURL:@"http://cloud.toast.com" viewController:self];
}

// Show Popup Style WebView
- (void)showPopupWebView:(id)sender {
    [TCGBWebView showPopupWithURL:@"http://cloud.toast.com" viewController:self];
}

// Show Customized WebView
- (void)showCustomizedWebView:(id)sender {
    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    [configuration setStyle:TCGBWebViewLaunchFullScreen];    //or TCGBWebViewLaunchPopUp
    [configuration setNavigationBarColor:[UIColor blueColor]];
    [configuration setNavigationBarHeight:50.0];

    [TCGBWebView showWebViewWithURL:@"http://cloud.toast.com" viewController:self configuration:configuration];
}

// Configure Custom Style Configuration to All TCGBWebView Objects
- (void)configureWebViewStyle {
    // After this method is called, every webview(TCGBWebView) is shown with popup style.

    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    [configuration setStyle:TCGBWebViewLaunchPopUp];    //or TCGBWebViewLaunchFullScreen

    [TCGBWebView sharedTCGBWebView].defaultWebConfiguration = configuration;
}
```


### 2. Alert
System Alert 를 위한 API를 제공합니다.<br/>
iOS8 이상에서 동작하는 UIAlertController와, iOS8 미만에서의 UIAlertView 처리를 내부적으로 해줍니다.<br/>
다음의 API를 통해서, 사용자는 Alert에 버튼 및 콜백을 등록할 수 있습니다.

```objectivec
- (void)showAlert:(id)sender {
    void (^positiveBlock)(id) = ^(id title) {
        NSLog(@"Positive Block Clicked");
    };

    void (^negativeBlock)(id) = ^(id title) {
        NSLog(@"Negative Block Clicked");
    };

    [TCGBUtil showAlertWithTitle:@"alert title" message:@"alert message"
            positiveTitle:@"positive" positiveBlock:positiveBlock
            negativeTitle:@"negative" negativeBlock:negativeBlock];
}
```






## Error code

| Category | Sub Category | Error | Error Code | Notes |
| -------- | ------------ | ----- | ---------- | ----- |
| Common |  | TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase 초기화가 되어있지 않습니다. |
|  |  | TCGB\_ERROR\_NOT\_LOGGED\_IN | 2 | 로그인이 필요합니다. |
|  |  | TCGB\_ERROR\_INVALID\_PARAMETER | 3 | 잘못된 파라미터입니다. |
|  |  | TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4 | JSON 포맷 에러입니다. |
|  |  | TCGB\_ERROR\_USER\_PERMISSION | 5 | 권한이 없습니다. |
|  |  | TCGB\_ERROR\_NOT\_SUPPORTED | 10 | 지원하지 않는 기능입니다. |
|  |  | TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11 | Android에서 지원하지 않는 기능입니다. |
|  |  | TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12 | iOS에서 지원하지 않는 기능입니다. |
| Network | Socket | TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101 | 네트워크 상태가 불안정하여 응답이 없습니다. |
|  |  | TCGB\_ERROR\_SOCKET\_ERROR | 110 | 소켓 에러 |
|  |  | TCGB\_ERROR\_UNKNOWN\_ERROR | 999 | 소켓 알 수 없는 에러 |
| Launching |  | TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001 | 런칭 서버 에러입니다. |
|  |  | TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002 | Client ID가 존재하지 않습니다. |
|  |  | TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003 | 등록되지 않은 App 입니다. |
|  |  | TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004 | 등록되지 않은 Client (version) 입니다. |
| Auth | Common | TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001 | 로그인이 취소되었습니다. |
|  |  | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
|  |  | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
|  |  | TCGB\_ERROR\_AUTH\_INVALID\_MEMBER | 3004 | 잘못된 회원에 대한 요청입니다. |
|  |  | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009 | 외부 인증 라이브러리 에러입니다. |
|  | Gamebase Login | TCGB\_ERROR\_AUTH\_TAP\_LOGIN\_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  |  | TCGB\_ERROR\_AUTH\_TAP\_LOGIN\_INVALID\_TOKEN\_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  |  | TCGB\_ERROR\_AUTH\_TAP\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103 | 최근에 로그인한 IDP 정보가 없습니다. |
|  | IDP Login | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201 | IDP 로그인에 실패하였습니다. |
|  |  | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
|  | Add Mapping | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
|  |  | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
|  |  | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303 | 이미 같은 IDP에 맵핑되어있습니다. |
|  |  | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
|  | Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  |  | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IDP는 삭제할 수 없습니다. |
|  |  | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403 | 현재 로그인되어있는 IDP 입니다. |
|  | Logout | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501 | 로그아웃에 실패하였습니다. |
|  | Withdrawal | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601 | 탈퇴에 실패하였습니다. |
|  | Not Playable | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
|  | Unknown | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |
| Purchase |  | TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001 | Gamebase PurchaseAdapter가 초기화되지 않았습니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002 | 구매가 취소되었습니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003 | 이전 구매가 완료되지 않았습니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004 | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010 | 지원하지 않는 스토어입니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201 | 외부 IAP 라이브러리 에러입니다. |
|  |  | TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999 | 알수없는 구매 에러입니다. |
| Push |  | TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101 | 외부 라이브러리 에러입니다. |
|  |  | TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 PUSH API 호출이 완료되지 않았습니다.<br>이전 PUSH API의 콜백이 실행된 이후에 다시 호출하세요. |
|  |  | TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999 | 알수 없는 푸시 에러입니다. (정의되지 않은 푸시 에러입니다.) |
| UI |  | TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999 | 알수 없는 에러입니다. (정의되지 않은 에러입니다.) |
| Server |  | TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001 | 서버 내부 에러 |
|  |  | TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002 | 서버에서 외부 연동중 에러 발생 |
|  |  | TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999 | 서버에서 알 수 없는 에러 |




## Sample Application



## API Reference
SDK 내에 포함되어 있습니다.
