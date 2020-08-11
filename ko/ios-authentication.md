## Game > Gamebase > iOS SDK 사용 가이드 > 인증


## Login

Gamebase에서는 게스트 로그인을 기본으로 지원합니다.

- 게스트 이외의 Provider에 로그인하려면 해당 Provider AuthAdapter가 필요합니다.
- AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음을 참고하시기 바랍니다.
    - [3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

로그인을 시도하려는 IdP별로, additionalInfo 파라미터를 입력해야 하는 경우가 있습니다.<br/>
AdditionalInfo에 대한 설명은 하단의 **Gamebase에서 지원 중인 IdP** 설명을 참고하시기 바랍니다.


### Import Header File

로그인을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

많은 게임이 타이틀 화면에서 로그인을 구현합니다.
* 앱을 설치하고 처음 실행했을 때 타이틀 화면에서 게임 유저가 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.
* 한 번 로그인한 후에는 IdP 선택 화면을 표시하지 않고 이전에 로그인한 IdP 유형으로 인증합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_2.6.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)

#### 1. 이전 로그인 유형으로 인증

* 이전에 인증했던 기록이 있다면 ID와 비밀번호를 입력받지 않고 인증을 시도합니다.
* **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**을 호출합니다.

#### 1-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **[TCGBGamebase userID]**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 1-2. 인증이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **TCGB_ERROR_SOCKET_ERROR(110)** 또는 **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 유저
    * 오류 코드가 **TCGB_ERROR_BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **[TCGBBanInfo banInfoFromError:error]**로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **[TCGBConfiguration enablePopup:YES]** 및 **[TCGBConfiguration enableBanPopup:YES]**를 호출한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 이전 로그인 유형으로 인증하기가 실패하였습니다. **3. 지정된 IdP로 인증**을 진행합니다.

#### 2. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 **TCGBConstants.h** 파일의 **TCGBAuthIdPs**에 선언돼 있습니다.
* **[TCGBGamebase loginWithType:viewController:completion:]** API를 호출합니다.

#### 2-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **[TCGBGamebase userID]**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 2-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **TCGB_ERROR_SOCKET_ERROR(110)** 또는 **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **[TCGBGamebase loginWithType:viewController:completion:]**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 사용자
    * 오류 코드가 **TCGB_ERROR_BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **[TCGBBanInfo banInfoFromError:error]** 로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **[TCGBConfiguration enablePopup:YES]** 및 **[TCGBConfiguration enableBanPopup:YES]**를 호출한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 오류가 발생했다는 것을 게임 유저에게 알리고, 게임 유저가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IdP

특정 IdP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.<br/>
가장 최근에 로그인한 IdP로 로그인을 시도합니다. 해당 로그인에 대한 토큰이 만료되었거나,
토큰에 대한 검증 등에 실패하면 실패를 반환합니다.<br/>
이때는 해당 IdP에 대한 로그인을 구현해야 합니다.




```objectivec
- (void)automaticLogin {
    [TCGBGamebase loginForLastLoggedInProviderWithViewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
            //TODO: 1. Do you want.
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithViewController:completion: or Notify to user -\n\terror[%@]", [error description]);
                //TODO: 1. If the error had occured by network problem, you can retry by loginForLastLoggedInProviderWithViewController:completion:
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");
                // Last Logged In Provider Name
    			NSString *lastLoggedInProvider = [TCGBGamebase lastLoggedInProvider];
    			if (lastLoggedInProvider == nil || lastLoggedInProvider <= 0) {
                	//TODO: 1. Show your UI what user want to sign in.
                    //2. If the user has selected IdP, set lastLoggedInProvider to it.
                    //3. Invoke loginWithType:viewController:completion: method to try login.
                }

                // Try to login with IdP authentication
                //Warning: If you receive an event asynchronously from async handler(callback), you can use codes below in the async handler.
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

### Login with IdP

특정 IdP 로그인 호출을 위해서 **[TCGBGamebase loginWithType:viewController:completion:]** 메서드를 호출합니다.<br/>
Gamebase를 통하여 로그인을 처음 시도하거나, 로그인 정보(액세스 토큰) 등이 만료되었다면, 이 API를 사용하여 로그인을 시도해야 합니다.<br/>
로그인 결과로 **(TCGBError *)error** 객체를 이용해 성공 여부를 판단할 수 있습니다. <br/>
또한 **TCGBAuthToken** 객체를 이용하여 사용자 ID 등의 사용자 정보 및 토큰 정보를 얻을 수 있습니다.<br/>
로그인에 성공하면, Gamebase 액세스 토큰이 Local Storage에 저장되며 이후 loginForLastLoggedInProviderWithViewController:completion: 메서드를 사용할 때 저장된 액세스 토큰을 사용하게 됩니다.<br/>
하지만 IdP의 액세스 토큰은 각 IdP가 제공하는 SDK가 관리합니다.<br/>

<br/><br/>
몇몇 IdP로 로그인할 때는 꼭 필요한 정보가 있습니다.<br/>
예를 들어, Facebook 로그인을 구현하려면 scope 등을 설정해야 합니다.<br/>
이러한 필수 정보들을 설정할 수 있게 **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API를 제공합니다.<br/>
파라미터 additionalInfo에 필수 정보들을 dictionary 형태로 입력하시면 됩니다.<br/>
(파라미터 값이 nil일 때는, TOAST Console에 등록한 additionalInfo 값으로 채워집니다. 파라미터 값이 있을 때는 Console에 등록해 놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.)


> [참고]
>
> iOS에서 지원하는 IdP는 **TCGBConstants.h**의 TCGBAuthIDPs 영역의 **kTCGBAuthXXXXXX**로 정의되어 있습니다.
>

```objectivec
- (void)loginFacebookButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

#### Gamebase에서 지원 중인 IdP
[Console Guide](./oper-app/#authentication-information)를 참고하시기 바랍니다.

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급받은 액세스 토큰 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.




* Credential 파라미터 설정 방법



| keyname                                  | a use                          | 값 종류                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdP 유형 설정                      | facebook, iosgamecenter, naver, google, twitter, line, appleid |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정 |                                |



> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

```objectivec
#import "TCGBConstants.h"

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: kTCGBAuthFacebook, kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"여기에 facebook SDK에서 발급받은 Access Token을 입력하세요" };
    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

### Authentication Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout

#### Import Header File

로그아웃을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### Logout API

로그인된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 유저 데이터는 유지됩니다.
로그아웃에 성공하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 표시됩니다.<br/><br/>

다음은 로그아웃 버튼을 클릭하면 로그아웃이 되는 예시 코드입니다.

```objectivec
- (void)authLogout {
    [TCGBGamebase logoutWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Logout Succeeded
        } else {
            // To Logout Failed
        }
    }];
}
```




## Withdraw

### Import Header File

탈퇴를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Widthdraw API

다음은 로그인 상태에서 게임 유저 탈퇴를 구현하는 예시 코드입니다.<br/><br/>

* 탈퇴에 성공하면, 로그인했던 IdP와 연동되어 있던 게임 유저 데이터는 삭제됩니다.
* 해당 IdP로 다시 로그인할 수 있으며, 새 게임 유저 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IdP 로그아웃을 시도하게 됩니다.

다음은 탈퇴 버튼을 클릭하면 탈퇴가 되는 예시 코드입니다.

```objectivec
- (void)authWithdrawal {
    [TCGBGamebase withdrawWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Withdrawal Succeeded
        } else {
            // To Withdrawal Failed
        }
    }];
}
```

## Mapping

매핑은 기존에 로그인된 계정에 다른 IdP의 계정을 연동하거나 해제시키는 기능입니다.

대다수의 게임에서는 게임 유저 계정 하나에 여러 IdP를 연동(매핑)할 수 있습니다.<br/>Gamebase의 매핑 API를 사용하면 기존에 로그인된 계정에 다른 IdP 계정을 연동하거나 해제할 수 있습니다.

즉, 연동 중인 IdP 계정으로 로그인을 시도하면 항상 같은 사용자 ID로 로그인됩니다.<br/><br/>

주의할 점은, IdP마다 하나의 계정만 연동할 수 있다는 점입니다.<br/>
예를 들어 Google 계정을 연동 중이면, 다른 Google 계정을 추가로 연동할 수 없습니다.<br/>
계정 연동 예시는 다음과 같습니다.<br/><br/>

* Gamebase 사용자 ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
* Gamebase 사용자 ID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 이미 Google ee 계정이 연동중이므로 Google 계정을 추가로 연동할 수 없습니다.**

매핑 API에는 매핑 추가와 매핑 해제 API가 있습니다.

### Add Mapping Flow

매핑은 다음 순서로 구현할 수 있습니다.

#### 1. 로그인
매핑은 현재 계정에 IdP 계정 연동을 추가하는 것이므로 우선 로그인이 돼 있어야 합니다.
먼저 로그인 API를 호출해 로그인합니다.

#### 2. 매핑

**[TCGBGamebase addMappingWithType:viewController:completion:]**을 호출해 매핑을 시도합니다.

#### 2-1. 매핑이 성공한 경우

* 축하합니다! 현재 계정과 연동중인 IdP 계정이 추가되었습니다.
* 매핑에 성공해도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. 즉, Gamecenter 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Gamecenter에서 Facebook으로 변경되지는 않습니다. Gamecenter 상태로 유지됩니다.
* 매핑은 단순히 IdP 연동만 추가해 줍니다.

#### 2-2. 매핑이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **TCGB_ERROR_SOCKET_ERROR(110)** 또는 **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **[TCGBGamebase addMappingWithType:viewController:completion:]**을 다시 호출하거나, 잠시 대기했다가 재시도 합니다.
* 이미 다른 계정에 연동 중일 때 발생하는 오류
    * 오류 코드가 **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**인 경우, 매핑하려는 IdP의 계정이 이미 다른 계정에 연동 중이라는 뜻입니다. 이미 연동된 계정을 해제하려면 해당 계정으로 로그인하여 **[TCGBGamebase withdrawWithViewController:completion:]**를 호출하여 탈퇴하거나 **[TCGBGamebase removeMappingWithType:viewController:completion:]**를 호출하여 연동을 해제한 후 다시 매핑을 시도하세요.
* 이미 동일한 IdP 계정에 연동돼 발생하는 오류
	* 에러 코드가 **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)** 인 경우, 매핑하려는 IdP와 같은 종류의 계정이 이미 연동중이라는 뜻입니다.
	* Gamebase 매핑은 한 IdP당 하나의 계정만 연동 가능합니다. 예를 들어 Google 계정에 이미 연동 중이라면 더 이상 Google 계정을 추가할 수 없습니다.
	* 동일 IdP의 다른 계정을 연동하기 위해서는 **[TCGBGamebase removeMappingWithType:viewController:completion:]**을 호출해 연동을 해제한 후 다시 매핑을 시도하세요.
* 그 외의 오류
    * 매핑 시도가 실패했습니다.

### Import Header file into View Controller

매핑을 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

특정 IdP에 로그인된 상태에서 다른 IdP로 매핑을 시도합니다.<br/>

다음은 Facebook에 매핑을 시도하는 예시입니다.

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
                     NSLog(@"AddMapping is succeeded.");
                 }
                 else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                     NSLog(@"Retry addMapping");
                 }
                 else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                     NSLog(@"Already mapped to other member");
                 }
                 else {
                     NSLog(@"AddMapping Error - %@", [error description]);
                 }
            }
        }
    }];
}
```

### AddMapping with Credential

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMapping을 할 수 있는 인터페이스입니다.




* Credential 파라미터 설정 방법



| keyname                                  | a use                          | 값 종류                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdP 유형 설정                      | facebook, iosgamecenter, naver, google, twitter, line, appleid |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정 |                                |



> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>


```objectivec
     - (void)onButtonLogin {
         UIViewController* topViewController = nil;
 
         NSString* facebookAccessToken = @"feijla;feij;fdklvda;hfihsdfeuipivaipef/131fcusp";
         NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
         credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = kTCGBAuthFacebook;
         credentialInfo[kTCGBAuthLoginWithCredentialAccessTokenKeyname] = facebookAccessToken;
 
         [TCGBGamebase addMappingWithCredential:credentialInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
             if ([TCGBGamebase isSuccessWithError:error] == YES) {
                 NSLog(@"AddMapping is succeeded.");
             }
             else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                 NSLog(@"Retry addMapping");
             }
             else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                 NSLog(@"Already mapped to other member");
             }
             else {
                 NSLog(@"AddMapping Error - %@", [error description]);
             }
         }];
     }
```

### Add Mapping Forcibly
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase addMappingForciblyWithType:ticket.idPCode forcingMappingKey:ticket.forcingMappingKey viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```


### Add Mapping Forcibly with Credential
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMappingForcibly를 호출 할 수 있는 인터페이스입니다.

* Credential 파라미터 설정방법


| keyname                                  | a use                          | 값 종류                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdP 유형 설정                      | facebook, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정 |                                           |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

```objc
- (void)onButtonLogin {
    UIViewController* topViewController = nil;
    
    NSString* facebookAccessToken = @"feijla;feij;fdklvda;hfihsdfeuipivaipef/131fcusp";
    NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
    credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = kTCGBAuthFacebook;
    credentialInfo[kTCGBAuthLoginWithCredentialAccessTokenKeyname] = facebookAccessToken;
    
    [TCGBGamebase addMappingWithCredential:credentialInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase addMappingWithCredential:credentialInfo forcingMappingKey:ticket.forcingMappingKey viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```


### Remove Mapping API

특정 IdP에 대한 연동을 해제합니다. <br/>
만약, 해제하고자 하는 IdP가 **유일한 IdP**라면, 실패를 반환합니다.<br/>
연동 해제 후에는 Gamebase 내부에서, 해당 IdP에 대한 로그아웃을 처리합니다.

```objectivec
[TCGBGamebase removeMappingWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}];
```

### Get IdP Mapping List
현재의 계정이 어떤 IdP들과 매핑되어 있는지 목록을 확인할 수 있습니다.

```objectivec
// Obtaining Names of Mapping IdPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase User`s Information
Gamebase로 인증 절차를 진행한 후, 앱을 제작할 때 필요한 정보를 얻을 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> "[TCGBGamebase loginForLastLoggedInProvider]" API 로 로그인한 경우에는 인증 정보를 가져올 수 없습니다.
>
> 인증 정보가 필요하다면 "[TCGBGamebase loginForLastLoggedInProvider]" 대신, 사용하고자 하는 IDPCode 와 동일한 {IDP_CODE} 를 파라미터로 하여 "[TCGBGamebase loginWithType:IDP_CODE viewController:topViewController completion:completion];" API 로 로그인 해야 정상적으로 인증정보를 획득할 수 있습니다.

### Get Authentication Information for Gamebase
Gamebase에서 발급한 인증 정보를 가져올 수 있습니다.

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];
```


### Get Authentication Information for External IdP

외부 인증 SDK에서 액세스 토큰, 사용자 ID, 프로파일 등의 정보를 가져올 수 있습니다.

```objectivec
// Example for obtaining ID Provider's Authentication Information

// Obtaining Facebook UserID
NSString *userID = [TCGBGamebase authProviderUserIDWithIDPCode:kTCGBAuthFacebook];

// Obtaining Facebook AccessToken
NSString *accessTokenOfIDP = [TCGBGamebase authProviderAccessTokenWithIDPCode:kTCGBAuthFacebook];

// Obtaining Facebook Profile
TCGBAuthProviderProfile *providerProfile = [TCGBGamebase authProviderProfileWithIDPCode:kTCGBAuthFacebook];
```

### Get Banned User Information

Gamebase Console에 제재된 게임 유저로 등록될 경우,
로그인을 시도하면 아래와 같은 이용 제한 정보 코드가 표시될 수 있습니다. **[TCGBBanInfo banInfoFromError:error]** 메서드를 이용해 제재 정보를 확인할 수 있습니다.

* TCGB_ERROR_BANNED_MEMBER




## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급받은 TransferAccountInfo는 다른 단말기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> `주의`
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

**API**

```objectivec
+ (void)issueTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)issueTransferAccount {
     [TCGBGamebase issueTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Issued TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```

### Query TransferAccount
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

**API**

```objectivec
+ (void)queryTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)queryTransferAccount {
     [TCGBGamebase queryTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Published TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```


### Renew TransferAccount
이미 발급받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```objectivec
+ (void)renewTransferAccountWithConfiguration:(TCGBTransferAccountRenewConfiguration *)config completion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
- (void)renewTransferAccount {
    // If you want renew the account automatically, use this config.
    TCGBTransferAccountRenewalTargetType renewalTargetType = TCGBTransferAccountRenewalTargetTypeIdPassword;
    TCGBTransferAccountRenewConfiguration* autoConfig = [TCGBTransferAccountRenewConfiguration autoRenewConfigurationWithRenewalTarget:renewalTargetType];
    
    // If you want renew the account manually, use this config.
    TCGBTransferAccountRenewConfiguration* manualConfig = [TCGBTransferAccountRenewConfiguration manualRenewConfigurationWithAccountId:@"ID" accountPassword:@"PASSWORD"];
    [TCGBGamebase renewTransferAccountWithConfiguration:autoConfig completion:^(TCGBTransferAccountInfo *transferAccount, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error]) {
            // Renewing TransferAccount success.
            NSString* accountId = transferAccount.account.accountId;
            NSString* accountPw = transferAccount.account.accountPassword;
            return;
        }
        else {
            // Renewing TransferAccount failed.
        }
    }];
}
```




### Transfer Guest Account to Another Device
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, 게스트 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> `주의`
> 이미 게스트 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.
> 만일 잘못된 id/password 를 연속해서 입력하면 **AUTH_TRANSFERACCOUNT_BLOCK(3042)** 에러가 발생하며 계정 이전이 일정 시간 차단됩니다.
> 이 경우에는 아래의 예제와 같이 TCGBTransferAccountFailInfo 값을 통해 언제까지 계정 이전이 차단되는지 유저에게 알려줄 수 있습니다.



**API**

```objectivec
+ (void)transferAccountWithIdPLoginWithAccountId:(NSString *)accountId accountPassword:(NSString *)accountPassword completion:(void(^)(TCGBAuthToken* authToken, TCGBError* error))completion;
```

**Example**

```objectivec
 - (void)transferOtherDevice {
    [TCGBGamebase transferAccountWithIdPLoginWithAccountId:@"1Aie0198" accountPassword:@"1Aie0199" completion:^(TCGBAuthToken* authToken, TCGBError* error) {
       if (error.code == TCGB_ERROR_AUTH_TRANSFERACCOUNT_BLOCK) {
            // Transfering Account failed.
            TCGBTransferAccountFailInfo* failInfo = [TCGBTransferAccountFailInfo transferAccountFailInfoFrom:error];
            if (failInfo == nil) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.

                NSString *failedId = failInfo.accountId;
                NSInteger failCount = failInfo.failCount;
                NSDate *blockedDate = [NSDate dateTimeIntervalSince1970:(failInfo.blockEndDate / 1000.0)];
                return;
            }
            // Transfering Account failed by another reason.
            return;  
        }
        // Transfering Account success.
        // TODO: implements post login process
    }];
 }
```



## TemporaryWithdrawal

'탈퇴 유예' 기능입니다.
임시 탈퇴를 요청하여 즉시 탈퇴가 진행되지 않고 일정 기간의 유예 기간이 지나면 탈퇴가 이루어집니다.
유예 기간은 콘솔에서 변경할 수 있습니다.

> `주의`
>
> 탈퇴 유예 기능을 사용하는 경우에는 **[TCGBGamebase withdrawWithViewController:completion:]** API 를 사용하지 마세요.
> **[TCGBGamebase withdrawWithViewController:completion:]** API 는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.getTemporaryWithdrawalInfo() API 를 호출하여 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

**API**

```objectivec
+ (void)withdrawWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | 이미 임시 탈퇴중인 유저 입니다. |

**Example**

```objectivec
- (void)testRequestWithdraw {
    TCGBGamebase requestTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBTemporaryWithdrawalInfo *info, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            }
            else {
                // Request temporary withdrawal failed.
                return;
            }
        }

        // Request temporary withdrawal success.
    }];
}
```

### Check TemporaryWithdrawal User

탈퇴 유예를 사용하는 게임은 로그인 후 항상 **TCGBAuthToken.tcgbMember.temporaryWithdrawal** 를 사용하여하여, 결과가 null 이 아닌 유효한 TemporaryWithdrawalInfo 객체를 리턴한다면 해당 유저에게 탈퇴 진행중이라는 사실을 알려주어야 합니다.

**Example**


```objectivec
- (void)testLogin {
    [TCGBGamebase loginWithType:@"appleid" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // Login failed
            return;
        }

        // Check if user is requesting withdrawal
        if (authToken.tcgbMember.temporaryWithdrawal != nil) {
            // User is under temporary withdrawal
            long gradePeriod = authToken.tcgbMember.temporaryWithdrawal.gracePeriodDate;
        }
        else {
            // Login Success
        }
    }];
}
```


### Cancel TemporaryWithdrawal

탈퇴를 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

**API**

```objectivec
+ (void)cancelTemporaryWithdrawalWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | 임시 탈퇴중인 유저가 아닙니다. |

**Example**

```objectivec
- (void)testCancelWithdraw {
    [TCGBGamebase cancelTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            }
            else {
                // Cancel temporary withdrawal failed.
                return
            }
        }

        // Cancel temporary withdrawal success.
    }];
}
```

### Withdraw Immediately

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 **[TCGBGamebase withdrawWithViewController:completion:]** API 와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

**API**

```objectivec
+ (void)withdrawImmediatelyWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```


**Example**

```objectivec
- (void)testWithdrawImmediately {
    [TCGBGamebase withdrawImmediatelyWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // withdraw failed.
            return;
        }

        // Withdraw success.
    }];
}
```


## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | 잘못된 회원에 대한 요청입니다.                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | 제재된 회원입니다.                               |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | 로그인이 취소되었습니다.                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR    | 3006       |  외부 인증 라이브러리 초기화에 실패하였습니다.                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 외부 인증 라이브러리 오류입니다. <br/> DetailCode 및 DetailMessage를 확인해주세요.  |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | 토큰 로그인에 실패했습니다.                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 토큰 정보가 유효하지 않습니다.                        |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdP 로그인에 실패하였습니다.                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | 매핑 추가에 실패했습니다.                           |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 이미 다른 멤버에 매핑돼 있습니다.                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 이미 같은 IdP에 매핑돼 있습니다.                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | 게스트 IdP로는 AddMapping이 불가능합니다. |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 맵핑 삭제에 실패했습니다.                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 마지막에 맵핑된 IdP는 삭제할 수 없습니다.                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 현재 로그인되어 있는 IdP 입니다.                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | 로그아웃에 실패했습니다.                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | 탈퇴에 실패했습니다.                              |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴중인 유저 입니다.                    |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴중인 유저가 아닙니다.                     |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | 플레이할 수 없는 상태입니다 (점검 또는 서비스 종료 등).        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | 알수 없는 오류입니다. (정의되지 않은 오류입니다.)            |




* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    - [오류 코드](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**
* 이 오류는 각 IdP의 SDK에서 발생한 오류입니다.
* 오류 코드 확인은 다음과 같이 확인하실 수 있습니다.

* IdP SDK의 오류 코드는 각각의 Developer 페이지를 참고하시기 바랍니다.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


