## Upcoming Products > Gamebase > Developer's Guide (iOS) > Login

## Login
* Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
* guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party SDK에 대한 설정은 위에 있는 '외부 SDK 다운로드' 링크를 참고하시길 바랍니다.


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


> [INFO]
> 
> iOS에서 지원하는 IDP는 **TCGBConstants.h**의 TCGBAuthIDPs 영역의 **kTCGBAuthXXXXXX**로 정의되어 있습니다.
>

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
[LINK \[Facebook Developer Guide\]](https://developers.facebook.com/docs/ios/getting-started)

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





### 4. Login with access token of external IDP
게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증을 하고 발급받은 AccessToken등을 이용하여, Gamebase 로그인을 할 수 있는 인터페이스 입니다.

* Credential 파라미터의 설정방법
    * NSDictionary 타입으로 설정합니다.
    * **kTCGBAuthLoginWithCredentialProviderNameKeyname** 키에는 idp종류를 설정합니다. (faceboo, payco, iosgamecenter)
    * **kTCGBAuthLoginWithCredentialAccessTokenKeyname** 키에는 외부 SDK로부터 받은 인증정보(AccessToken)를 입력합니다.


> [TIP]
> 
> 게임 내에서 외부 서비스(Facebook 등)의 고유기능의 사용이 필요할 때 사용될 수 있습니다.
>

<br/>


> [WARNING]
> 
> 외부 SDK에서 요구하는 개발사항은 Gamebase에서는 지원이 불가능합니다.
>

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
