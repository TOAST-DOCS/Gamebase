## Game > Gamebase > User Guide for Unreal SDK > Authentication 

## Login

Gamebase basically supports guest login. <br/>

* To login to a provider other than guests, AuthAdapter for each provider is required.  
* Regarding setting for AuthAdapter and 3rd-Party Provider SDK, see the following: 
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

Many games allow login execution on the title page.  

* On the initial execution after app is installed, game users are allowed to select IdP (Identity Provider) on the title page. 
* After initial login, the IdP option page does not show again and it is authenticated with the previous login IdP type. 

The above login can be implemented in the following order: 

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_2.6.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)

#### 1. Authenticate with Previous Login Type 

* If a previous record exists for authentication, authentication can be tried without ID or password. 
* Call **LoginForLastLoggedInProvider API**. 

#### 1-1. When Authentication is Successful 

* Congratuations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic. 

#### 1-2. When Authentication Failed 

* Network Error 
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network issue, so call  **LoginForLastLoggedInProvider API** again or try again in a moment. 
* Banned Game User
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **GetBanInfo API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set true for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Erros 
    * Authentication failed with the previous login type. Execute **'3. Authenticate with Specified IdP'**.

#### 2. Authenticate with Specified IdP

* Specify IdP type to authenticate.  
    * Available types of authentication are declared in the **GamebaseAuthProvider** class. 
* Call **Login(providerName, callback) API**.

#### 2-1. When Authentication is Successful 

* Congratuations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic.

#### 2-2. When Authentication Failed

* Network Error
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network, so call **Login(providerName, callback) API** again, or try again in a moment. 
* Banned Game User 
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **GetBanInfo API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set **True** for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Errors 
    * Notify game users of the occurrence of error, and it is reverted to a status (mostly, title or login page) in which game user can select an IdP type of authetication.   

### Login with the Latest Login IdP

가장 최근에 로그인한 IdP로 로그인을 시도합니다. Try a login with the most recent login IdP. 
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등에 실패하면 실패를 반환합니다. When token for the login has expired, or if token verification has failed, return such failure.  
To that end, it is required to implement [Login for IdP](#login-with-idp).

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void LoginForLastLoggedInProvider(const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::LoginForLastLoggedInProvider()
{
    IGamebase::Get().LoginForLastLoggedInProvider(FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("LoginForLastLoggedInProvider succeeded."));
        }
        else
        {
            if (error->code == GamebaseErrorCode::SOCKET_ERROR || error->code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an error message to the user. : %s"), *error->message);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Try to login using a specifec IdP"));
            }
        }
    }));
}
```

### Login with GUEST

Gamebase supports guest login. 는 게스트 로그인을 지원합니다.
Create a unique key of device to try login to Gamebase. 디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
With guest login, device key might be initialized, which may cause your account to be deleted; so, it is recommended to use IdP to log in. login 게스트 로그인은 디바이스 키는 초기화될 수 있고 디바이스 키의 초기화 시에 계정이 삭제될 수 있으므로 IdP를 활용한 로그인 방식을 권장합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Login(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Login with IdP

Following codes are login examples with particular IdP. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Login(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
void Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**providerName**

| Provider    | Define                          | Support Platform | 
| --------    | ------------------------------- | ---------------- |
| Google      | GamebaseAuthProvider::Google     | Android<br/>iOS |
| Game Center | GamebaseAuthProvider::GameCenter | iOS |
| Apple ID    | GamebaseAuthProvider::AppleId    | iOS |
| Facebook    | GamebaseAuthProvider::Facebook   | Android<br/>iOS |
| Payco       | GamebaseAuthProvider::Payco      | Android<br/>iOS |
| Naver       | GamebaseAuthProvider::Naver      | Android<br/>iOS |
| Twitter     | GamebaseAuthProvider::Twitter    | Android<br/>iOS |
| Line        | GamebaseAuthProvider::Line       | Android<br/>iOS |


> Some IdP logins require particular information. 몇몇 IdP로 로그인할 때는 꼭 필요한 정보가 있습니다.<br/>
> For instance, to log in to Facebook, the setting for scope is needed.  를 들어, Facebook 로그인을 구현하려면 scope 등을 설정해야 합니다.<br/>
> To configure such required information, 이러한 필수 정보들을 설정할 수 있게 API for static void Login (string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) is provided. 를 제공합니다.<br/>
> You may enter information for the additionalInfo parameter in the format of dictionary.  파라미터에 필수 정보들을 dictionary 형태로 입력하시면 됩니다.
When there's available value for additionalInfo, use it; otherwise (null), apply default value registered on TOAST Console.   값이 있을 경우에는 해당 값을 사용하고 없을 경우에는(null) TOAST Console에 등록된 값을 사용합니다.
([Setting additionalInfo for TOAST Console](./oper-app/#authentication-information))<br/>
> Standalone supports a login via WebViewAdapter and does not block event input via UIs when WebView is open. 를 통해서 로그인을 지원하며 WebView가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}

void Sample::LoginWithAdditionalInfo()
{
    UGamebaseJsonObject* additionalInfo = NewObject<UGamebaseJsonObject>();
    additionalInfo->SetStringField(TEXT("Key"), TEXT("Value"));

    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, *additionalInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급받은 액세스 토큰 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[Caution]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Login(const UGamebaseJsonObject& credentialInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::LoginWithCredential()
{
    UGamebaseJsonObject* credentialInfo = NewObject<UGamebaseJsonObject>();

    // google
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    IGamebase::Get().Login(*credentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Authentication Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout
로그인 된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 유저 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 노출됩니다.<br/><br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Logout(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::Logout()
{
    IGamebase::Get().Logout(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Logout succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Logout failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```


## Withdraw
로그인 상태에서 탈퇴를 시도합니다.

* 탈퇴에 성공하면, 로그인했던 IdP와 연동되어 있던 게임 유저 데이터는 삭제됩니다.
* 해당 IdP로 다시 로그인 가능하고 새로운 게임 유저 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IdP 로그아웃을 시도합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Withdraw(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::Withdraw()
{
    IGamebase::Get().Withdraw(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Withdraw succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Withdraw failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

## Mapping

매핑은 기존에 로그인된 계정에 다른 IdP의 계정을 연동하거나 해제시키는 기능입니다.

많은 게임들이 하나의 계정에 여러 IdP를 연동(Mapping)할 수 있도록 하고 있습니다.
Gamebase의 Mapping API를 사용하여 기존에 로그인된 계정에 다른 IdP의 계정을 연동/해제시킬 수 있습니다.<br/>

이렇게 하나의 Gamebase 사용자 ID에 다양한 IdP 계정을 연동할 수 있습니다.
즉, 연동 중인 IdP 계정으로 로그인을 시도 한다면 항상 동일한 사용자 ID로 로그인됩니다.<br/>

주의할 점은, IdP 마다 하나의 계정씩만 연동이 가능합니다.
예시는 다음과 같습니다.<br/>

* Gamebase 사용자 ID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* Gamebase 사용자 ID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> 이미 Google ee 계정이 연동중이므로 Google계정을 추가로 연동할 수 없습니다.**

Mapping 에는 Mapping 추가/해제 API 2개가 있습니다.

### Add Mapping Flow

매핑은 다음 순서로 구현할 수 있습니다.

#### 1. 로그인
매핑은 현재 계정에 IdP 계정 연동을 추가하는 것이므로 우선 로그인이 돼 있어야 합니다.
먼저 로그인 API를 호출해 로그인합니다.

#### 2. 매핑

**AddMapping API**을 호출해 매핑을 시도합니다.

#### 2-1. 매핑이 성공한 경우

* 축하합니다! 현재 계정과 연동중인 IdP 계정이 추가되었습니다.
* 매핑에 성공해도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. <br>즉, Google 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
* 매핑은 단순히 IdP 연동만 추가해 줍니다.

#### 2-2. 매핑이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **AddMapping API**을 다시 호출하거나, 잠시 대기했다가 재시도 합니다.
* 이미 다른 계정에 연동 중일 때 발생하는 오류
    * 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**인 경우, 매핑하려는 IdP의 계정이 이미 다른 계정에 연동 중이라는 뜻입니다. 이미 연동된 계정을 해제하려면 해당 계정으로 로그인하여 **Withdraw API**를 호출하여 탈퇴하거나 **RemoveMapping API**를 호출하여 연동을 해제한 후 다시 매핑을 시도하세요.
* 이미 동일한 IdP 계정에 연동돼 발생하는 오류
	* 에러 코드가 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)** 인 경우, 매핑하려는 IdP와 같은 종류의 계정이 이미 연동중이라는 뜻입니다.
	* Gamebase 매핑은 한 IdP당 하나의 계정만 연동 가능합니다. 예를 들어 PAYCO 계정에 이미 연동 중이라면 더 이상 PAYCO 계정을 추가할 수 없습니다.
	* 동일 IdP의 다른 계정을 연동하기 위해서는 **RemoveMapping API**을 호출해 연동을 해제한 후 다시 매핑을 시도하세요.
* 그 외의 오류
    * 매핑 시도가 실패했습니다.


### Add Mapping

특정 IdP에 로그인 된 상태에서 다른 IdP로 Mapping을 시도합니다.
Mapping을 하려는 IdP의 계정이 이미 다른 계정에 연동이 되어있다면
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 오류를 반환합니다.<br/>

Mapping이 성공 하더라도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인 한 후, Facebook 계정 Mapping 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
Mapping은 단순히 IdP 연동만 추가 해줍니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void AddMapping(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
void AddMapping(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback)
```

**Example**

```cpp
void Sample::AddMapping(const FString& providerName)
{
    IGamebase::Get().AddMapping(providerName, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### AddMapping with Credential

The interface allows to authenticate with SDK provided by IdP of a game to enable Gamebase AddMapping by using a given access token.  게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증을 하고 발급받은 액세스 토큰등을 이용하여, Gamebase AddMapping을 할 수 있는 인터페이스 입니다.

* How to Set Credential Parameters  파라미터의 설정 방법

| Keyname | Usage | Value Type 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (access token) given after IdP login 로그인 이후 받은 인증 정보(액세스 토큰) 설정<br/> Not for Google authentication 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Set authorization code given after Google login  Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |

> [TIP]
>
> Might be required to use original features of external services (e.g. Facebook) for a game.  게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[Caution]</font><br/>
>
> Development issues required to be supported by external SDKs must be implemented by using APIs of such external SDK, which is not supported by Gamebase.  외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void AddMapping(const UGamebaseJsonObject& credentialInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingWithCredential()
{
    UGamebaseJsonObject* credentialInfo = NewObject<UGamebaseJsonObject>();

    // google
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    IGamebase::Get().AddMapping(*credentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Add Mapping Forcibly

For an account which is already mapped to a particular IdP, try mapping **With Force **. 특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
When trying **Force Mapping강제 매핑**, you need 을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket` acquiried from AddMpping API. 이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다. Following are examples of trying to force mapping: 

**API**

```cpp
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingForcibly(const FString& providerName)
{
    IGamebase::Get().AddMapping(providerName, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // By calling addMapping API and mapping to an account already integrated, you can get  우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicket 클래스의 From() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // Try to force mapping. 강제 매핑을 시도합니다.
                IGamebase::Get().AddMappingForcibly(providerName, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
                    {
                        // 강제 매핑 추가 성공
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```


### Add Mapping Forcibly with Credential
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMappingForcibly를 호출 할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                        |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[Caution]</font><br/>
>
> Development issues required to be supported by external SDKs must be 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

다음은 강제 매핑을 시도하는 예시입니다.

**API**

```cpp
void AddMappingForcibly(const UGamebaseJsonObject& credentialInfo, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingForcibly()
{
    UGamebaseJsonObject* credentialInfo = NewObject<UGamebaseJsonObject>();

    // google
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    IGamebase::Get().AddMapping(*credentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicket 클래스의 From() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // 강제 매핑을 시도합니다.
                IGamebase::Get().AddMappingForcibly(*credentialInfo, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
                    {
                        // 강제 매핑 추가 성공
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```


### Remove Mapping

특정 IdP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IdP가 유일한 IdP라면, 실패를 반환합니다.
연동 해제후에는 Gamebase 내부에서, 해당 IdP에 대한 로그아웃처리를 해줍니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RemoveMapping(const FString& providerName, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RemoveMapping(const FString& providerName)
{
    IGamebase::Get().RemoveMapping(providerName, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RemoveMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RemoveMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Get Mapping List

사용자 ID에 연동되어 있는 IdP 목록을 반환합니다.<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
TArray<FString> GetAuthMappingList() const;
```

**Example**

```cpp
void Sample::GetAuthMappingList()
{
    auto mappingList = IGamebase::Get().GetAuthMappingList();
    
    for (FString provider : mappingList)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetAuthMappingList - %s"), *provider);
    }
}
```

## Gamebase User`s Information

Gamebase를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

### Get Authentication Information for Gamebase
Gamebase를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

#### UserID

Gamebase에서 발급한 UserID를 가져올 수 있습니다.
**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetUserID() const;
```

**Example**
```cpp
void Sample::GetUserID()
{
    FString userID = IGamebase::Get().GetUserID();
}
```

#### AccessToken

Gamebase에서 발급한 액세스 토큰을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAccessToken() const;
```

**Example**
```cpp
void Sample::GetAccessToken()
{
    FString accessToken = IGamebase::Get().GetAccessToken();
}
```

#### Last LoggedIn Provider Name

Gamebase에서 마지막 로그인에 성공한 ProviderName을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetLastLoggedInProvider() const;
```

**Example**
```cpp
void Sample::GetLastLoggedInProvider()
{
    FString lastLoggedInProvider = IGamebase::Get().GetLastLoggedInProvider();
}
```

### Get Authentication Information for External IdP

외부 인증 SDK에서 액세스 토큰, 사용자 ID, Profile 등의 인증 정보를 가져올 수 있습니다.

#### UserID

외부 인증 SDK에서 사용자 ID를 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAuthProviderUserID(const FString& providerName) const;
```

**Example**

```cpp
void Sample::GetLastLoggedInProvider(const FString& providerName)z
{
    FString authProviderUserID = IGamebase::Get().GetAuthProviderUserID(GetProviderName(providerName));
}
```

#### AccessToken

외부 인증 SDK에서 액세스 토큰을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAuthProviderAccessToken(const FString& providerName) const;
```

**Example**
```cpp
void Sample::GetAuthProviderAccessToken(const FString& providerName)
{
    FString authProviderAccessToken = IGamebase::Get().GetAuthProviderAccessToken(providerName);
}
```

#### Profile

외부 인증 SDK에서 Profile을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
const FGamebaseAuthProviderProfilePtr GetAuthProviderProfile(const FString& providerName) const;
```

**Example**
```cpp
void Sample::GetAuthProviderProfile(const FString& providerName)
{
    auto authProviderProfile = IGamebase::Get().GetAuthProviderProfile(providerName);
    if (authProviderProfile.IsValid())
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetAuthProviderProfile: %s"), *authProviderProfile->information->EncodeJson());
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not auth provider profile."));
    }
}
```

### Get Banned User Infomation

Gamebase Console에 제재된 게임 유저로 등록될 경우,
로그인 시도 시, 이용 제한 정보 코드(**BANNED_MEMBER(7)**)가 표시될 수 있으며, 아래 API를 이용하여 제재 정보를 확인할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
const FGambaseBanInfoPtr GetBanInfo() const;
```

**Example**
```cpp
void Sample::GetBanInfo()
{
    auto banInfo = IGamebase::Get().GetBanInfo();
    if (banInfo.IsValid())
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetBanInfo: %s"), *banInfo->userId);
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found ban info."));
    }
}
```

## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급받은 TransferAccountInfo는 다른 단말기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> <font color="red">[주의]</font><br/>
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

**API**

```cpp
void IssueTransferAccount(const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::IssueTransferAccount()
{
    IGamebase::Get().IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Issuing TransferAccount success.
        }
        else
        {
            // Issuing TransferAccount failed.
        }
    }));
}
```

### Query TransferAccount
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

**API**

```cpp
void QueryTransferAccount(const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryTransferAccount()
{
    IGamebase::Get().IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Querying TransferAccount success.
        }
        else
        {
            // Querying TransferAccount failed.
        }
    }));
}
```

### Renew TransferAccount
이미 발급받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& configuration, const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::RenewTransferAccount(const FString& accountId, const FString& accountPassword)
{
    // 수동 설정
    FGamebaseTransferAccountRenewConfiguration configuration{ accountId, accountPassword };
    //FGamebaseTransferAccountRenewConfiguration configuration{ accountPassword };

    // 자동 설정
    //FGamebaseTransferAccountRenewConfiguration configuration{ type };

    IGamebase::Get().RenewTransferAccount(configuration, FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Renewing TransferAccount success.
        }
        else
        {
            // Renewing TransferAccount failed.
        }
    }));
}
```


### Transfer Guest Account to Another Device
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
> 이미 게스트 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

**API**

```cpp
void TransferAccountWithIdPLogin(const FString& accountId, const FString& accountPassword, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::TransferAccountWithIdPLogin(const FString& accountId, const FString& accountPassword)
{
    IGamebase::Get().TransferAccountWithIdPLogin(accountId, accountPassword, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Transfering Account success.
            // TODO: implements post login process
        }
        else
        {
            // Transfering Account failed.
        }
    }));
}
```


## TemporaryWithdrawal

'탈퇴 유예' 기능입니다.
임시 탈퇴를 요청하여 즉시 탈퇴가 진행되지 않고 일정 기간의 유예 기간이 지나면 탈퇴가 이루어집니다.
유예 기간은 콘솔에서 변경할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 탈퇴 유예 기능을 사용하는 경우에는 **Withdraw API** 를 사용하지 마세요.
> **Withdraw API**는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.member.temporaryWithdrawal로 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

**API**

```cpp
void RequestWithdrawal(const FGamebaseTemporaryWithdrawalDelegate& onCallback);
```

**Example**

```cpp
void Sample::RequestWithdrawal()
{
    IGamebase::Get().GetTemporaryWithdrawal().RequestWithdrawal(FGamebaseTemporaryWithdrawalDelegate::CreateLambda([=](const FGamebaseTemporaryWithdrawalInfo* info, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestWithdrawal succeeded. The date when you can withdraw your withdrawal is %d"), info->gracePeriodDate);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestWithdrawal failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Check TemporaryWithdrawal User

탈퇴 유예를 사용하는 게임은 로그인 후 항상 AuthToken.member.temporaryWithdrawal가 null이 아니라면 해당 유저에게 탈퇴 진행중이라는 사실을 알려주어야 합니다.

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            if (authToken->member.temporaryWithdrawal.gracePeriodDate > 0)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User is under temporary withdrawa. GracePeriodDate : %d"), authToken->member.temporaryWithdrawal.gracePeriodDate);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Cancel TemporaryWithdrawal

탈퇴를 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

**API**

```cpp
void CancelWithdrawal(const FGamebaseErrorDelegate& onCallback);
```
**Example**

```cpp
void Sample::CancelTemporaryWithdrawal()
{
    IGamebase::Get().GetTemporaryWithdrawal().CancelTemporaryWithdrawal(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("CancelTemporaryWithdrawal succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("CancelTemporaryWithdrawal failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Withdraw Immediately

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 Withdraw API 와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

**API**

```cpp
void WithdrawImmediately(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::WithdrawImmediately()
{
    IGamebase::Get().GetTemporaryWithdrawal().WithdrawImmediately(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("WithdrawImmediately succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("WithdrawImmediately failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

## Error Handling

| Category | Error | Error Code | Description |
| --- | --- | --- | --- |
| Auth | INVALID_MEMBER | 6 | 잘못된 회원에 대한 요청입니다. |
|  | BANNED_MEMBER | 7 | 제재된 회원입니다. |
|  | AUTH_USER_CANCELED | 3001 | 로그인이 취소되었습니다. |
|  | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
|  | AUTH_NOT_EXIST_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
|  | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 외부 인증 라이브러리 오류입니다. <br/> DetailCode 및 DetailMessage를 확인해주세요.  |
|  | AUTH_ALREADY_IN_PROGRESS_ERROR | 3010 | 이전 인증 프로세스가 완료되지 않았습니다.
| TransferAccount| SAME\_REQUESTOR                          | 8          | 발급한 TransferAccount를 동일한 단말기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IdP 정보가 없습니다. |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP 로그인에 실패하였습니다. |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IdP에 맵핑되어있습니다. |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IDP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IdP는 삭제할 수 없습니다. |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인 되어 있는 IdP 입니다. |
| Logout | AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 탈퇴에 실패했습니다.                              |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴중인 유저 입니다.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴중인 유저가 아닙니다.                     |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 외부 인증 라이브러리에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* IdP SDK의 오류 코드는 각 개발자 페이지를 참고하시기 바랍니다.
