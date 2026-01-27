## Game > Gamebase > Unreal SDK 사용 가이드 > 인증

## Login

Gamebase에서는 게스트 로그인을 기본으로 지원합니다.<br/>

* 게스트 이외의 Provider에 로그인하려면 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음을 참고하시기 바랍니다.
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

많은 게임이 타이틀 화면에 로그인을 구현합니다.

* 앱을 설치하고 처음 실행했을 때 타이틀 화면에서 게임 유저가 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.
* 한 번 로그인한 후에는 IdP 선택 화면을 표시하지 않고 이전에 로그인한 IdP 유형으로 인증합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 이전 로그인 유형으로 인증

* 이전에 인증했던 기록이 있다면 ID와 비밀번호를 입력받지 않고 인증을 시도합니다.
* **LoginForLastLoggedInProvider API**를 호출합니다.

#### 1-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **GetUserID API**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 1-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **LoginForLastLoggedInProvider API**를 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 유저
    * 오류 코드가 **BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **FGamebaseBanInfo::From API**로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려주시기 바랍니다.
    * Gamebase 초기화 시 **FGamebaseConfiguration.bEnablePopup** 및 **FGamebaseConfiguration.bEnableBanPopup** 값을 true로 한다면 Gamebase가 이용 정지에 관한 팝업 창을 자동으로 띄웁니다.
* 그 외 오류
    * 이전 로그인 유형으로 인증에 실패했습니다. **'2. 지정된 IdP로 인증'**을 진행합니다.

#### 2. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 **GamebaseAuthProvider** 클래스에 선언돼 있습니다.
* **Login(ProviderName, callback) API**를 호출합니다.

#### 2-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **GetUserID API**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 2-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **Login(ProviderName, callback) API**를 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 유저
    * 오류 코드가 **BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **FGamebaseBanInfo::From API**로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **FGamebaseConfiguration.bEnablePopup** 및 **FGamebaseConfiguration.bEnableBanPopup** 값을 **true**로 지정하면 Gamebase가 이용 정지에 관한 팝업 창을 자동으로 표시합니다.
* 그 외의 오류
    * 오류가 발생했다는 것을 게임 유저에게 알리고, 게임 유저가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IdP

가장 최근에 로그인한 IdP로 로그인을 시도합니다.
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등에 실패하면 실패를 반환합니다.
이때는 [해당 IdP에 대한 로그인](#login-with-idp)을 구현해야 합니다.

* AdditionalInfo 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential::ShowLoadingAnimation | API 호출이 끝날 때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void LoginForLastLoggedInProvider(const FGamebaseAuthTokenDelegate& Callback);
void LoginForLastLoggedInProvider(const FGamebaseVariantMap& AdditionalInfo, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::LoginForLastLoggedInProvider()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->LoginForLastLoggedInProvider(FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("LoginForLastLoggedInProvider succeeded."));
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::SOCKET_ERROR || Error->Code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(LogTemp, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an Error message to the user. : %s"), *Error->Message);
            }
            else if (Error->Code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto BanInfo = FGamebaseBanInfo::From(Error);
                if (BanInfo.IsValid())
                {
                    UE_LOG(LogTemp, Display, TEXT("This user has been banned. Gamebase userId is %s"), *BanInfo->UserId);
                }
            }
            else
            {
                UE_LOG(LogTemp, Display, TEXT("Try to login using a specifec IdP"));
            }
        }
    }));
}
```

### Login with GUEST

Gamebase는 게스트 로그인을 지원합니다.

* 디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
* 게스트 로그인은 디바이스 키는 초기화될 수 있고 디바이스 키의 초기화 시에 계정이 삭제될 수 있으므로 IdP를 활용한 로그인 방식을 권장합니다.

> <font color="red">[주의]</font><br/>
>
> Windows 환경에서는 GUEST 로그인은 개발 용도로 제공되었기 때문에 실제 게임에서 사용 시 주의가 필요합니다.
>

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Login(const FString& ProviderName, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::Login()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {1
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);

            if (Error->Code == GamebaseErrorCode::SOCKET_ERROR || Error->Code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(LogTemp, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an Error message to the user. : %s"), *Error->Message);
            }
            else if (Error->Code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto BanInfo = FGamebaseBanInfo::From(Error);
                if (BanInfo.IsValid())
                {
                    UE_LOG(LogTemp, Display, TEXT("This user has been banned. Gamebase userId is %s"), *BanInfo->userId);
                }
            }
        }
    }));
}
```

### Login with IdP

다음은 특정 IdP로 로그인할 수 있게 하는 예시 코드입니다
로그인할 수 있는 IdP 유형은 **GamebaseAuthProvider** 클래스에서 확인할 수 있습니다.

> [참고]
>
> 로그인할 때 추가 정보를 필요로 하는 IdP도 있습니다.
> 이러한 추가 정보들을 설정할 수 있게 void Login(const FString& ProviderName, const FGamebaseVariantMap& AdditionalInfo, const FGamebaseAuthTokenDelegate& Callback) API를 제공합니다.
>AdditionalInfo 파라미터에 필수 정보들을 dictionary 형태로 입력하시면 됩니다.
>AdditionalInfo 값이 있을 경우에는 해당 값을 사용하고 null일 경우에는 [NHN Cloud Console](./oper-app/#authentication-information)에 등록된 값을 사용합니다.

> [참고]
>
> LINE IdP는 Gamebase SDK 2.43.0부터 LINE 서비스 제공 지역을 설정할 수 있습니다.
> 해당 지역은 AdditionalInfo에 설정할 수 있습니다. 

* AdditionalInfo 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential::LineChannelRegion | LINE 서비스 제공 지역 설정 | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Login(const FString& ProviderName, const FGamebaseAuthTokenDelegate& Callback);
void Login(const FString& ProviderName, const FGamebaseVariantMap& AdditionalInfo, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::Login()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);

            if (Error->Code == GamebaseErrorCode::SOCKET_ERROR || Error->Code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(LogTemp, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an Error message to the user. : %s"), *Error->Message);
            }
            else if (Error->Code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto BanInfo = FGamebaseBanInfo::From(Error);
                if (BanInfo.IsValid())
                {
                    UE_LOG(LogTemp, Display, TEXT("This user has been banned. Gamebase userId is %s"), *BanInfo->UserId);
                }
            }
        }
    }));
}

void USample::LoginWithAdditionalInfo()
{
    FGamebaseVariantMap AdditionalInfo;
    AdditionalInfo.Add(TEXT("Key"), TEXT("Value"));

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(GamebaseAuthProvider::Facebook, AdditionalInfo, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

#### Cancel Login with External Browser

Windows 환경에서 SDK를 사용하지 않는 IdP의 경우 외부 브라우저를 통해 로그인을 진행합니다.
로그인 과정 중 외부 브라우저를 통한 로그인 플로우인 경우 해당 API를 호출하여 Login 과정을 중단하고 결과를 전달할 수 있습니다.

**API**

Supported Platforms

<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void CancelLoginWithExternalBrowser();
```

**Example**

```cpp
void USample::CancelLogin()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->CancelLoginWithExternalBrowser();
}
```

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급 받은 Access Token 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::ProviderName | IdP 유형 설정                           | GamebaseAuthProvider::Google<br> GamebaseAuthProvider::Facebook<br>GamebaseAuthProvider::Naver<br>GamebaseAuthProvider::Twitter<br>GamebaseAuthProvider::Line<br>GamebaseAuthProvider::Hangame<br>GamebaseAuthProvider::AppleId<br>GamebaseAuthProvider::Weibo<br>GamebaseAuthProvider::GameCenter<br>GamebaseAuthProvider::Payco<br>GamebaseAuthProvider::Steam<br>GamebaseAuthProvider::EpicGames |
| GamebaseAuthProviderCredential::AccessToken | IdP 로그인 이후 받은 인증 정보(Access Token) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential::AuthorizationCode | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |
| GamebaseAuthProviderCredential::GamebaseAccessToken | IdP 인증 정보가 아닌 Gamebase Access Token으로 로그인하는 경우 사용 |  |
| GamebaseAuthProviderCredential::IgnoreAlreadyLoggedIn | Gamebase에 로그인한 상태에서 로그아웃을 하지 않고 다른 계정을 이용해 로그인을 시도하는 것을 허용 | **bool** |
| GamebaseAuthProviderCredential::LineChannelRegion | LINE 서비스 제공 지역 설정 | [Login with IdP 참고](./unreal-authentication/#login-with-idp) |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Login(const FGamebaseVariantMap& CredentialInfo, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::LoginWithCredential()
{
    FGamebaseVariantMap CredentialInfo;

    // google
    //CredentialInfo.Add(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //CredentialInfo.Add(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    CredentialInfo.Add(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    CredentialInfo.Add(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(*CredentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```


## Logout
로그인된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 유저 데이터는 유지됩니다.
로그아웃에 성공하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 나타납니다.<br/><br/>

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Logout(const FGamebaseErrorDelegate& Callback);
```

**Example**

```cpp
void USample::Logout()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Logout(FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Logout succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Logout failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```


## Withdraw

로그인 상태에서 탈퇴를 시도합니다.

* 탈퇴 성공 시
  * 로그인했던 IdP의 게임 이용자 데이터는 삭제됩니다.
  * 해당 IdP로 다시 로그인할 수 있으며, 새로운 게임 이용자 데이터를 생성합니다.
  * 연동 중인 모든 IdP가 로그아웃 처리됩니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.

> <font color="red">[주의]</font><br/>
>
> 여러 IdP를 연동 중인 경우, 모든 IdP 연동이 해제되고 Gamebase 유저 데이터가 삭제됩니다.
>

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Withdraw(const FGamebaseErrorDelegate& Callback);
```

**Example**

```cpp
void USample::Withdraw()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Withdraw(FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("Withdraw succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Withdraw failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
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
    * AppleID ID: cc
    * Twitter ID: dd
* Gamebase 사용자 ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 이미 Google ee 계정이 연동 중이므로 Google 계정을 추가로 연동할 수 없습니다.**

매핑 API에는 매핑 추가와 매핑 해제 API가 있습니다.

> <font color="red">[주의]</font><br/>
>
> Guest 로그인 중에 매핑을 성공하면 Guest IdP는 사라집니다.
>

### Add Mapping Flow

매핑은 다음 순서로 구현할 수 있습니다.

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. 로그인
매핑은 현재 계정에 IdP 계정 연동을 추가하는 것이므로 우선 로그인이 돼 있어야 합니다.
먼저 로그인 API를 호출해 로그인합니다.

#### 2. 매핑

**AddMapping API**를 호출해 매핑을 시도합니다.

#### 2-1. 매핑에 성공한 경우

* 축하합니다! 현재 계정과 연동 중인 IdP 계정이 추가되었습니다.
* 매핑에 성공해도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. <br>즉, Google 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
* 매핑은 단순히 IdP 연동만 추가합니다.

#### 2-2. 매핑에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **AddMapping API**를 다시 호출하거나, 잠시 대기했다가 재시도 합니다.
* 이미 다른 계정에 연동 중일 때 발생하는 오류
    * 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**인 경우, 매핑하려는 IdP의 계정이 이미 다른 계정에 연동 중이라는 뜻입니다. 이미 연동된 계정을 해제하려면 해당 계정으로 로그인하여 **Withdraw API**를 호출하여 탈퇴하거나 **RemoveMapping API**를 호출하여 연동을 해제한 후 다시 매핑을 시도하세요.
* 이미 동일한 IdP 계정에 연동돼 발생하는 오류
    * 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)** 인 경우, 매핑하려는 IdP와 같은 종류의 계정이 이미 연동 중이라는 뜻입니다.
    * Gamebase 매핑은 한 IdP당 하나의 계정만 연동 가능합니다. 예를 들어 PAYCO 계정에 이미 연동 중이라면 더 이상 PAYCO 계정을 추가할 수 없습니다.
    * 동일 IdP의 다른 계정을 연동하기 위해서는 **RemoveMapping API**를 호출해 연동을 해제한 후 다시 매핑을 시도하세요.
* 그 외의 오류
    * 매핑 시도에 실패했습니다.


### Add Mapping

특정 IdP에 로그인된 상태에서 다른 IdP로 매핑을 시도합니다.
매핑하려는 IdP의 계정이 이미 다른 계정에 연동되어 있다면
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 오류를 반환합니다.<br/>

매핑에 성공하더라도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
매핑은 단순히 IdP 연동만 추가합니다.

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void AddMapping(const FString& ProviderName, const FGamebaseAuthTokenDelegate& Callback);
void AddMapping(const FString& ProviderName, const FGamebaseVariantMap& AdditionalInfo, const FGamebaseAuthTokenDelegate& Callback)
```

**Example**

```cpp
void USample::AddMapping(const FString& ProviderName)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddMapping(ProviderName, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### AddMapping with Credential

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급 받은 Access Token 등을 이용해, Gamebase AddMapping을 할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::ProviderName | IdP 유형 설정 | GamebaseAuthProvider::Google<br> GamebaseAuthProvider::Facebook<br>GamebaseAuthProvider::Naver<br>GamebaseAuthProvider::Twitter<br>GamebaseAuthProvider::Line<br>GamebaseAuthProvider::Hangame<br>GamebaseAuthProvider::AppleId<br>GamebaseAuthProvider::Weibo<br>GamebaseAuthProvider::GameCenter<br>GamebaseAuthProvider::Payco<br>GamebaseAuthProvider::Steam |
| GamebaseAuthProviderCredential::AccessToken | IdP 로그인 이후 받은 인증 정보(Access Token) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential::AuthorizationCode | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void AddMapping(const FGamebaseVariantMap& CredentialInfo, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::AddMappingWithCredential()
{
    FGamebaseVariantMap CredentialInfo;

    // google
    //CredentialInfo.Add(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //CredentialInfo.Add(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    CredentialInfo.Add(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    CredentialInfo.Add(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddMapping(*CredentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### Add Mapping Forcibly

특정 IdP에 이미 매핑된 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```cpp
void AddMappingForcibly(const FGamebaseForcingMappingTicket& ForcingMappingTicket, const FGamebaseAuthTokenDelegate& Callback);

// Legacy API
void AddMappingForcibly(const FString& ProviderName, const FString& ForcingMappingKey, const FGamebaseAuthTokenDelegate& Callback);
void AddMappingForcibly(const FString& ProviderName, const FString& ForcingMappingKey, const FGamebaseVariantMap& AdditionalInfo, const FGamebaseAuthTokenDelegate& Callback);
void AddMappingForcibly(const FGamebaseVariantMap& CredentialInfo, const FString& ForcingMappingKey, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::AddMappingForcibly(const FString& ProviderName)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddMapping(GetProviderName(LoginType), FGamebaseAuthTokenDelegate::CreateLambda([Subsystem](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicket 클래스의 From() 메서드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                const FGamebaseForcingMappingTicketPtr ForcingMappingTicket = FGamebaseForcingMappingTicket::From(Error);
                if (!ForcingMappingTicket.IsValid())
                {
                    // Unexpected error occurred. Contact Administrator.
                }

                // 강제 매핑을 시도합니다.
                Subsystem->AddMappingForcibly(*ForcingMappingTicket, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* InnerAuthToken, const FGamebaseError* InnerError)
                {
                    if (Gamebase::IsSuccess(InnerError))
                    {
                        // 강제 매핑 추가 성공
                        UE_LOG(LogTemp, Display, TEXT("ChangeLogin succeeded. Gamebase userId is %s"), *InnerAuthToken->Member.UserId);
                    }
                    else
                    {
                        // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                        UE_LOG(LogTemp, Display, TEXT("ChangeLogin failed."));
                    }
                }));
            }
            else
            {
                // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                UE_LOG(LogTemp, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```

### Change Login with ForcingMappingTicket

특정 IdP에 이미 매핑된 계정이 있다면 현재 계정에서 로그아웃하고 매핑된 계정으로 로그인합니다.
이때, AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

Change Login API 호출이 실패하는 경우, Gamebase 로그인 상태는 기존의 UserID로 유지됩니다.

**API**

```cpp
void ChangeLogin(const FGamebaseForcingMappingTicket& ForcingMappingTicket, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

다음은 Facebook으로 매핑 시도 후 Facebook에 이미 매핑된 계정이 존재하자, 해당 계정으로 로그인을 변경하는 예시입니다.

```cpp
void USample::ChangeLoginWithFacebook(const FString& ProviderName)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddMapping(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicket 클래스의 From() 메서드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                const FGamebaseForcingMappingTicketPtr ForcingMappingTicket = FGamebaseForcingMappingTicket::From(Error);
                if (!ForcingMappingTicket.IsValid())
                {
                    // Unexpected error occurred. Contact Administrator.
                }

                // 로그인 변경을 시도합니다.
                Subsystem->ChangeLogin(*ForcingMappingTicket, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* InnerAuthToken, const FGamebaseError* InnerError)
                {
                    if (Gamebase::IsSuccess(InnerError))
                    {
                        // 로그인 변경 성공
                        UE_LOG(LogTemp, Display, TEXT("ChangeLogin succeeded. Gamebase userId is %s"), *InnerAuthToken->Member.UserId);
                    }
                    else
                    {
                        // 로그인 변경 실패
                        // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                        UE_LOG(LogTemp, Display, TEXT("ChangeLogin failed."));
                    }
                }));
            }
            else
            {
                // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                UE_LOG(LogTemp, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```

### Remove Mapping

특정 IdP에 대한 연동을 해제합니다. 만약, 해제할 IdP가 유일한 IdP라면, 실패를 반환합니다.
연동 해제 후에는 Gamebase 내부에서, 해당 IdP에 대한 로그아웃을 처리합니다.

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RemoveMapping(const FString& ProviderName, const FGamebaseErrorDelegate& Callback);
```

**Example**

```cpp
void USample::RemoveMapping(const FString& ProviderName)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->RemoveMapping(ProviderName, FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RemoveMapping succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RemoveMapping failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### Get Mapping List

사용자 ID에 연동된 IdP 목록을 반환합니다.<br/>

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
TArray<FString> GetAuthMappingList() const;
```

**Example**

```cpp
void USample::GetAuthMappingList()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    auto MappingList = Subsystem->GetAuthMappingList();
    
    for (FString provider : MappingList)
    {
        UE_LOG(LogTemp, Display, TEXT("GetAuthMappingList - %s"), *provider);
    }
}
```

## Gamebase User's Information

Gamebase에서 인증 절차를 진행한 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

### Get Authentication Information for Gamebase
Gamebase를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 얻을 수 있습니다.

#### UserID

Gamebase에서 발급한 UserID를 가져올 수 있습니다.
**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetUserID() const;
```

**Example**
```cpp
void USample::GetUserID()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    FString UserID = Subsystem->GetUserID();
}
```

#### AccessToken

Gamebase에서 발급한 Access Token을 가져올 수 있습니다.

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetAccessToken() const;
```

**Example**
```cpp
void USample::GetAccessToken()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    FString AccessToken = Subsystem->GetAccessToken();
}
```

#### Last LoggedIn Provider Name

Gamebase에서 마지막 로그인에 성공한 ProviderName을 가져올 수 있습니다.

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void RequestLastLoggedInProvider(const FGamebaseLastLoggedInProviderDelegate& Callback) const;
FString GetLastLoggedInProvider() const;
```

**Example**
```cpp
void USample::GetLastLoggedInProvider()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());

    // Obtaining Last Logged In Provider - Sync
    FString LastLoggedInProvider = Subsystem->GetLastLoggedInProvider();
    
    // Obtaining Last Logged In Provider - Async
    // If GetLastLoggedInProvider() returns 'NOT_INITIALIZED_YET',
    // use the following async function instead:
    Subsystem->RequestLastLoggedInProvider(FGamebaseLastLoggedInProviderDelegate::CreateLambda([](const FString& lastLoggedInProviderAsync, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("LastLoggedInProvider: %s"), *lastLoggedInProviderAsync);
        }
    }));
}
```

### Get Authentication Information for External IdP

* 외부 인증 IdP의 Access Token, 사용자 ID, Profile 등의 정보는 로그인 후 게임 서버에서 Gamebase Server API를 호출하여 가져올 수 있습니다.
    * [Game > Gamebase > API 가이드 > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[주의]</font><br/>
>
> * 외부 IdP의 인증 정보는 보안을 위해 게임 서버에서 호출할 것을 권장합니다.
> * IdP에 따라 Access Token이 빠른 시간에 만료될 수 있습니다.
>     * 예를 들어 Google은 로그인 2시간 후에는 Access Token이 만료되어 버립니다.
>     * 사용자 정보가 필요하다면 로그인 후 바로 Gamebase Server API를 호출하시기 바랍니다.
> * "Gamebase.LoginForLastLoggedInProvider()" API로 로그인한 경우에는 인증 정보를 가져올 수 없습니다.
>     * 사용자 정보가 필요하다면 "Gamebase.LoginForLastLoggedInProvider()" 대신, 사용하고자 하는 IDPCode와 동일한 {IDP_CODE}를 파라미터로 하여 "Gamebase.Login(IDP_CODE, callback)" API로 로그인 해야 합니다.

### Get Banned User Information

Gamebase Console에 제재된 게임 유저로 등록될 경우, 로그인을 시도하면 아래와 같은 이용 제한 정보 코드(**BANNED_MEMBER(7)**)가 표시될 수 있습니다.
**FGamebaseBanInfo::From API**를 이용해 제재 정보를 확인할 수 있습니다.


## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo**라고 부릅니다.
발급 받은 TransferAccountInfo는 다른 단말기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> <font color="red">[주의]</font><br/>
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, PAYCO 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

**API**

```cpp
void IssueTransferAccount(const FGamebaseTransferAccountDelegate& Callback);
```

**Example**

```cpp
void USample::IssueTransferAccount()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([](const FGamebaseTransferAccountInfo* TransferAccountInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
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
게스트 계정 이전을 위해 이미 발급 받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

**API**

```cpp
void QueryTransferAccount(const FGamebaseTransferAccountDelegate& Callback);
```

**Example**

```cpp
void USample::QueryTransferAccount()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([](const FGamebaseTransferAccountInfo* TransferAccountInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
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
이미 발급 받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신할 수 있습니다.

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& Configuration, const FGamebaseTransferAccountDelegate& Callback);
```

**Example**

```cpp
void USample::RenewTransferAccount(const FString& AccountId, const FString& AccountPassword)
{
    // 수동 설정
    FGamebaseTransferAccountRenewConfiguration Configuration;
    Configuration.AccountId = AccountId;
    Configuration.AccountPassword = AccountPassword;
    //FGamebaseTransferAccountRenewConfiguration Configuration { AccountPassword };

    // 자동 설정
    //FGamebaseTransferAccountRenewConfiguration Configuration{ type };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->RenewTransferAccount(Configuration, FGamebaseTransferAccountDelegate::CreateLambda([](const FGamebaseTransferAccountInfo* TransferAccountInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
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
**issueTransfer** API로 발급 받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급 받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
> 이미 게스트 로그인된 상태에서 이전에 성공하면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

**API**

```cpp
void TransferAccountWithIdPLogin(const FString& AccountId, const FString& AccountPassword, const FGamebaseAuthTokenDelegate& Callback);
```

**Example**

```cpp
void USample::TransferAccountWithIdPLogin(const FString& AccountId, const FString& AccountPassword)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->TransferAccountWithIdPLogin(AccountId, AccountPassword, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
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
> 탈퇴 유예 기능을 사용하는 경우에는 **Withdraw API**를 사용하지 마세요.
> **Withdraw API**는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.member.temporaryWithdrawal로 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

**API**

```cpp
void RequestWithdrawal(const FGamebaseTemporaryWithdrawalDelegate& Callback);
```

**Example**

```cpp
void USample::RequestWithdrawal()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTemporaryWithdrawal()->RequestWithdrawal(FGamebaseTemporaryWithdrawalDelegate::CreateLambda([](const FGamebaseTemporaryWithdrawalInfo* Info, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestWithdrawal succeeded. The date when you can withdraw your withdrawal is %d"), Info->GracePeriodDate);
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestWithdrawal failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### Check TemporaryWithdrawal User

탈퇴 유예를 사용하는 게임은 로그인 후 항상 AuthToken.member.temporaryWithdrawal가 null이 아니라면 해당 유저에게 탈퇴 진행 중이라는 사실을 알려야 합니다.

**Example**

```cpp
void USample::Login()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            if (AuthToken->Member.TemporaryWithdrawal.IsSet())
            {
                const auto TemporaryWithdrawal = AuthToken->Member.TemporaryWithdrawal.GetValue();
                // GracePeriodDate: epoch time in milliseconds
                const FDateTime PeriodDate = FDateTime(1970, 1, 1) + FTimespan::FromMilliseconds(TemporaryWithdrawal.GracePeriodDate);
                UE_LOG(LogTemp, Display, TEXT("User is under temporary withdrawa. GracePeriodDate : %s"), *PeriodDate.ToString());
            }
            else
            {
                UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### Cancel TemporaryWithdrawal

탈퇴 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

**API**

```cpp
void CancelWithdrawal(const FGamebaseErrorDelegate& Callback);
```
**Example**

```cpp
void USample::CancelTemporaryWithdrawal()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTemporaryWithdrawal()->CancelTemporaryWithdrawal(FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("CancelTemporaryWithdrawal succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("CancelTemporaryWithdrawal failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

### Withdraw Immediately

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 Withdraw API와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

**API**

```cpp
void WithdrawImmediately(const FGamebaseErrorDelegate& Callback);
```

**Example**

```cpp
void USample::WithdrawImmediately()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTemporaryWithdrawal()->WithdrawImmediately(FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("WithdrawImmediately succeeded."));
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("WithdrawImmediately failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
        }
    }));
}
```

## GraceBan

* '결제 어뷰징 자동 해제' 기능입니다.
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 '이용 정지 유예 상태' 후 이용 정지가 되도록 합니다.
    * '이용 정지 유예 상태'일 경우, 설정한 기간 내에 이용 정지 해제 조건을 모두 만족하면 정상적으로 플레이할 수 있습니다.
    * 기간 내에 조건을 충족하지 못하면 이용이 정지됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.member.graceBanInfo API 값을 확인하고, null이 아닌 유효한 GraceBanInfo 객체를 반환한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리해야 합니다.

**Example**

```cpp
void USample::Login()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error) == false)
        {
            // Login failed
            return;
        }
        
        if (AuthToken->Member.GraceBan.IsSet())
        {
            const auto GraceBan = AuthToken->Member.raceBan.GetValue();

            // GracePeriodDate: epoch time in milliseconds
            const FDateTime PeriodDate = FDateTime(1970, 1, 1) + FTimespan::FromMilliseconds(GraceBan.GracePeriodDate);

            if (GraceBan.ReleaseRuleCondition.IsSet())
            {
                const auto ReleaseRuleCondition = GraceBan.ReleaseRuleCondition.GetValue();

                // condition type: "AND", "OR"
                FString releaseRule = FString::Printf(TEXT("%lld%s %s %dtime(s)"), ReleaseRuleCondition.Amount,
                    *ReleaseRuleCondition.Currency, *ReleaseRuleCondition.ConditionType, ReleaseRuleCondition.Count);
            }

            if (GraceBan.PaymentStatus.IsSet())
            {
                const auto paymentStatus = GraceBan.PaymentStatus.GetValue();

                FString paidAmount = FString::Printf(TEXT("%lld%s"), PaymentStatus.Amount, *PaymentStatus.Currency);
                FString paidCount = PaymentStatus.count + "time(s)";
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("Login succeeded. Gamebase userId is %s"), *AuthToken->Member.UserId);
        }
    }));
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 잘못된 회원에 대한 요청입니다.                        |
|                | BANNED\_MEMBER                           | 7          | 제재된 회원입니다.                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | 로그인이 취소되었습니다.                            |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 외부 인증 라이브러리 초기화에 실패하였습니다. |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 외부 인증 라이브러리 오류입니다. <br/>상세 오류를 확인하십시오. |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | 이전 인증 프로세스가 완료되지 않았습니다. |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | Gamebase Access Token이 유효하지 않아 로그아웃되었습니다.<br/>로그인을 다시 시도하십시오. |
|                | AUTH\_AUTHENTICATION\_SERVER\_ERROR      | 3012       | 인증 서버로부터 오류가 발생했습니다. |
| TransferAccount| SAME\_REQUESTOR                          | 8          | 발급한 TransferAccount를 동일한 단말기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효 기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러 번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> NHN Cloud Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아 주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IdP 정보가 없습니다. |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP 로그인에 실패하였습니다. |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패했습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어 있습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IdP에 맵핑되어 있습니다. |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제 매핑 키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제 매핑 키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제 매핑 키(ForcingMappingKey)의 유효 기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제 매핑 키(ForcingMappingKey)가 다른 IdP에 사용되었습니다. <br/>발급 받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는 데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제 매핑 키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급 받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 매핑 삭제에 실패하였습니다. |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 매핑된 IdP는 삭제할 수 없습니다. |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인 되어 있는 IdP입니다. |
| Logout | AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 탈퇴에 실패했습니다.                              |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴를 요청한 유저입니다.                    | 
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴를 요청한 유저가 아닙니다.                     | 
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다(점검 또는 서비스 종료 등). |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 알 수 없는 오류입니다. (정의되지 않은 오류입니다.) |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./Error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 외부 인증 라이브러리에서 오류가 발생했을 때 반환됩니다.
* 외부 인증 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 

```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(LogTemp, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Message);

    if (Error->Code == GamebaseErrorCode::AUTH_EXTERNAL_LIBRARY_ERROR)
    {
        FGamebaseErrorInner moduleError = Error->Error;
        if (moduleError.code != GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(LogTemp, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError.code, *moduleError.message);
        }
    }
}
```

* 상세 오류 코드는 각각의 외부 인증 라이브러리의 Developer 페이지를 참고하시기 바랍니다.
