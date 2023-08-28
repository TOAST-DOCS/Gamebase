## Game > Gamebase > Unity SDK 사용 가이드 > 인증

## Login

Gamebase에서는 게스트 로그인을 기본으로 지원합니다.<br/>

* 게스트 이외의 Provider에 로그인하려면 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음을 참고하시기 바랍니다.
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

많은 게임이 타이틀 화면에서 로그인을 구현합니다.

* 앱을 설치하고 처음 실행했을 때 타이틀 화면에서 게임 유저가 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.
* 한 번 로그인한 후에는 IdP 선택 화면을 표시하지 않고 이전에 로그인한 IdP 유형으로 인증합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 이전 로그인 유형으로 인증

* 이전에 인증했던 기록이 있다면 ID와 비밀번호를 입력받지 않고 인증을 시도합니다.
* **Gamebase.LoginForLastLoggedInProvider()**를 호출합니다.

#### 1-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **Gamebase.GetUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 1-2. 인증이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.LoginForLastLoggedInProvider()**를 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 유저
    * 오류 코드가 **BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)**으로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup **값을  true로 한다면 Gamebase가 이용 정지에 관한 팝업 창을 자동으로 띄웁니다.
* 그 외 오류
    * 이전 로그인 유형으로 인증하기가 실패하였습니다. **'2. 지정된 IdP로 인증'**을 진행합니다.

#### 2. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 **GamebaseAuthProvider** 클래스에 선언돼 있습니다.
* **Gamebase.Login(providerName, callback)** API를 호출합니다.

#### 2-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.GetUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 2-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **Gamebase.Login(providerName, callback)**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 유저
    * 오류 코드가 **BANNED_MEMBER(7)**인 경우, 이용 정지 게임 유저이므로 인증에 실패한 것입니다.
    * **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)**으로 제재 정보를 확인하여 게임 유저에게 게임을 플레이할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup **값을  **true**로 한다면 Gamebase가 이용 정지에 관한 팝업 창을 자동으로 띄웁니다.
* 그 외의 오류
    * 오류가 발생했다는 것을 게임 유저에게 알리고, 게임 가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IdP

가장 최근에 로그인한 IdP로 로그인을 시도합니다.
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등에 실패하면 실패를 반환합니다.
이때는 [해당 IdP에 대한 로그인](#login-with-idp)을 구현해야합니다.

* AdditionalInfo 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API 호출이 끝날때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void LoginForLastLoggedInProvider(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void LoginForLastLoggedInProvider(Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void LoginForLastLoggedInProvider()
{
	Gamebase.LoginForLastLoggedInProvider((authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Login succeeded.");
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("Login failed. error is {0}", error));
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
            else
            {
                Debug.Log("Try to login using a specifec IdP");
                Gamebase.Login("ProviderName", (authToken, error) => {});
            }
        }
    });
}
```

### Login with GUEST

Gamebase는 게스트 로그인을 지원합니다.

* 디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
* 게스트 로그인은 디바이스 키는 초기화될 수 있고 디바이스 키의 초기화 시에 계정이 삭제될 수 있으므로 IdP를 활용한 로그인 방식을 권장합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.GUEST, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

### Login with IdP

다음은 특정 IdP로 로그인할 수 있게 하는 예시 코드입니다.
로그인할 수 있는 IdP 유형은 **GamebaseAuthProvider** 클래스에서 확인할 수 있습니다.

> [참고]
>
> 로그인할 때 추가정보를 필요로 하는 IdP도 있습니다.
> 이러한 추가 정보들을 설정할 수 있게 static void Login(string providerName, Dictionary additionalInfo, GamebaseCallback.GamebaseDelegate callback) API를 제공합니다.
>additionalInfo 파라미터에 필수 정보들을 dictionary 형태로 입력하시면 됩니다.
>additionalInfo 값이 있을 경우에는 해당 값을 사용하고 null 일 경우에는 [NHN Cloud Console](./oper-app/#authentication-information)에 등록된 값을 사용합니다.

> [참고]
>
> LINE IdP는 Gamebase SDK 2.43.0부터 LINE 서비스 제공 지역을 설정할 수 있습니다.
> 해당 지역은 AdditionalInfo에 설정할 수 있습니다. 

> <font color="red">[주의]</font><br/>
>
> Standalone에서는 WebViewAdapter를 통해서 로그인을 지원하며 웹뷰가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.
>
> Standalone WebViewAdapter를 사용하여 로그인을 하기 위해서는 IdP 개발자 사이트에서 아래 CallbackURL을 설정 하여야 합니다.
> - https://id-gamebase.toast.com/oauth/callback
>

* additionalInfo 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API 호출이 끝날때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | LINE 서비스 제공 지역 설정 | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.FACEBOOK, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}

public void LoginWithAdditionalInfo()
{
    var additionalInfo = new Dictionary<string, object>
    {
        { "key", "value" }
    };

    Gamebase.Login(GamebaseAuthProvider.FACEBOOK, additionalInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {            
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급 받은 Access Token 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

> <font color="red">[주의]</font><br/>
>
> Standalone, WebGL 플랫폼에서 Google 로그인을 진행할 경우에는 GamebaseAuthProviderCredential.REDIRECT_URI를 입력해야 합니다. 입력하지 않을 경우 `redirect_uri_mismatch` 오류가 발생합니다.
>
> REDIRECT_URI는 **Google Cloud Console > API 및 서비스 > 사용자 인증 정보 > 웹 클라이언트**의 승인된 리디렉션 URI에 추가된 값을 입력합니다. (Google 로그인 페이지에서 AuthCode를 받는 URI)
>
> REDIRECT_URI를 입력하지 않을 경우 기본값이 적용됩니다.
>   - Standalone: http://localhost:8080/
>   - WebGL: http://localhost/

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | GamebaseAuthProvider.GOOGLE<br>GamebaseAuthProvider.GAMECENTER<br> GamebaseAuthProvider.FACEBOOK<br>GamebaseAuthProvider.NAVER<br>GamebaseAuthProvider.TWITTER<br>GamebaseAuthProvider.LINE<br>GamebaseAuthProvider.HANGAME<br>GamebaseAuthProvider.APPLEID<br>GamebaseAuthProvider.WEIBO<br>GamebaseAuthProvider.KAKAOGAME<br>GamebaseAuthProvider.PAYCO |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(Access Token) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |
| GamebaseAuthProviderCredential.GAMEBASE_ACCESS_TOKEN | IdP 인증 정보가 아닌 Gamebase Access Token으로 로그인하는 경우 사용 |  |
| GamebaseAuthProviderCredential.IGNORE_ALREADY_LOGGED_IN | Gamebase에 로그인한 상태에서 로그아웃을 하지 않고 다른 계정을 이용해 로그인을 시도하는 것을 허용 | **bool** |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API 호출이 끝날때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | LINE 서비스 제공 지역 설정 | [Login with IdP 참고](./unity-authentication/#login-with-idp) |
| GamebaseAuthProviderCredential.REDIRECT_URI | **Google Cloud Console > 승인된 리디렉션 URI**에 등록된 redirect_uri를 추가<br>**Standalone default**: http://localhost:8080/<br>**WebGL default**: http://localhost/<br/>**Standalone, WebGL 플랫폼에서 Google 로그인 시에 한함** |  |

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
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

UnityEditor에서는 Facebook로그인만 지원합니다.

```cs
static void Login(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void LoginWithCredential()
{
    var credentialInfo = new Dictionary<string, object>();
    
    // facebook
    credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.FACEBOOK);
    credentialInfo.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, "facebook access token");
    
    // google
    // credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.GOOGLE);
    // credentialInfo.Add(GamebaseAuthProviderCredential.AUTHORIZATION_CODE, "google auchorization code");
    
    Gamebase.Login(credentialInfo, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error) == true)
        {
            
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

## Logout
로그인 된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 유저 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 노출됩니다.<br/><br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Logout(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void Logout()
{
    Gamebase.Logout((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Logout succeeded.");
        }
        else
        {
        	Debug.Log(string.Format("Logout failed. error is {0}", error));
        }
    });
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
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Withdraw(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void Withdraw()
{
    Gamebase.Withdraw((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Withdraw succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Withdraw failed. error is {0}", error));
        }
    });
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

* Gamebase 사용자 ID: 123bcabca
	* Google ID: aa
	* Facebook ID: bb
	* AppleGameCenter ID: cc
	* PAYCO ID: dd
* Gamebase 사용자 ID: 456abcabc
	* Google ID: ee
	* Google ID: ff **-> 이미 Google ee 계정이 연동중이므로 Google계정을 추가로 연동할 수 없습니다.**

Mapping 에는 Mapping 추가/해제 API 2개가 있습니다.

### Add Mapping Flow

매핑은 다음 순서로 구현할 수 있습니다.

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. 로그인
매핑은 현재 계정에 IdP 계정 연동을 추가하는 것이므로 우선 로그인이 돼 있어야 합니다.
먼저 로그인 API를 호출해 로그인합니다.

#### 2. 매핑

**Gamebase.AddMapping()**을 호출해 매핑을 시도합니다.

#### 2-1. 매핑이 성공한 경우

* 축하합니다! 현재 계정과 연동중인 IdP 계정이 추가되었습니다.
* 매핑에 성공해도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. <br>즉, Google 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
* 매핑은 단순히 IdP 연동만 추가해 줍니다.

#### 2-2. 매핑이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.AddMapping()**을 다시 호출하거나, 잠시 대기했다가 재시도 합니다.
* 이미 다른 계정에 연동 중일 때 발생하는 오류
    * 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**인 경우, 매핑하려는 IdP의 계정이 이미 다른 계정에 연동 중이라는 뜻입니다. 이미 연동된 계정을 해제하려면 해당 계정으로 로그인하여 **Gamebase.Withdraw()**를 호출하여 탈퇴하거나 **Gamebase.RemoveMapping()**를 호출하여 연동을 해제한 후 다시 매핑을 시도하세요.
* 이미 동일한 IdP 계정에 연동돼 발생하는 오류
	* 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**인 경우 매핑하려는 IdP와 같은 종류의 계정이 이미 연동 중이라는 뜻입니다.
	* Gamebase 매핑은 한 IdP당 하나의 계정만 연동 가능합니다. 예를 들어 PAYCO 계정에 이미 연동 중이라면 더 이상 PAYCO 계정을 추가할 수 없습니다.
	* 동일 IdP의 다른 계정을 연동하기 위해서는 **Gamebase.RemoveMapping()**을 호출해 연동을 해제한 후 다시 매핑을 시도하세요.
* 그 외의 오류
    * 매핑 시도가 실패했습니다.

### Add Mapping

특정 IdP에 로그인 된 상태에서 다른 IdP로 Mapping을 시도합니다.
Mapping을 하려는 IdP의 계정이 이미 다른 계정에 연동이 되어있다면
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 오류를 반환합니다.<br/>

Mapping이 성공 하더라도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인 한 후, Facebook 계정 Mapping 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
Mapping은 단순히 IdP 연동만 추가 해줍니다.

* additionalInfo 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API 호출이 끝날때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | LINE 서비스 제공 지역 설정 | [Login with IdP 참고](./unity-authentication/#login-with-idp) |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMapping(string providerName)
{
    Gamebase.AddMapping(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("AddMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("AddMapping failed. error is {0}", error));
        }
    });
}
```

### AddMapping with Credential

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증을 하고 발급 받은 Access Token 등을 이용하여, Gamebase AddMapping을 할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(Access Token) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                          |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API 호출이 끝날때까지 로딩 애니메이션을 표시<br>**Android에 한함** | **bool**<br>**default**: true |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | LINE 서비스 제공 지역 설정 | [Login with IdP 참고](./unity-authentication/#login-with-idp) |

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
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingWithCredential()
{
    var credentialInfo = new Dictionary<string, object>();

    // facebook
    credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.FACEBOOK);
    credentialInfo.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, "facebook access token");

    // google
    // credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.GOOGLE);
    // credentialInfo.Add(GamebaseAuthProviderCredential.AUTHORIZATION_CODE, "google auchorization code");

    Gamebase.AddMapping(credentialInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("AddMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("AddMapping failed. error is {0}", error));
        }
    });
}
```

### Add Mapping Forcibly
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```cs
static void AddMappingForcibly(GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)

// Legacy API
static void AddMappingForcibly(string providerName, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void AddMappingForcibly(string providerName, string forcingMappingKey, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void AddMappingForcibly(Dictionary<string, object> credentialInfo, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(string idPName)
{
    Gamebase.AddMapping(idPName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // 매핑 추가 성공
        }
        else
        {
            // 우선 AddMapping API 호출 및 이미 연동되어 있는 계정으로 매핑을 시도하여, 다음과 같이 ForcingMappingTicket을 얻을 수 있습니다.
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // ForcingMappingTicket 클래스의 From() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

                // 강제 매핑을 시도합니다.
                Gamebase.AddMappingForcibly(forcingMappingTicket, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // 강제 매핑 추가 성공
                    }
                    else
                    {
                        // 강제 매핑 추가 실패
                        // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                    }
                });
            }
            else
            {
                // 오류 코드를 확인하고 적절한 처리를 진행합니다.
            }
        }
    });
}
```

### Change Login with ForcingMappingTicket

특정 IdP에 이미 매핑되어 있는 계정이 있을 때, 현재 계정을 로그아웃하고 이미 매핑되어 있던 해당 계정으로 로그인합니다.
이때, AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

Change Login API 호출이 실패하는 경우, Gamebase 로그인 상태는 기존의 UserID로 유지됩니다.

**API**

```cs
static void ChangeLogin(GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

다음은 Facebook으로 매핑 시도 후 Facebook에 이미 매핑된 계정이 존재하자, 해당 계정으로 로그인을 변경하는 예시입니다.

```cs
public void ChangeLoginWithFacebook()
{
    Gamebase.AddMapping(GamebaseAuthProvider.FACEBOOK, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // 매핑 추가 성공
            return;
        }
        
        // 우선 AddMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
        if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
        {
            // ForcingMappingTicket 클래스의 From() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
            GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

            // ForcingMappingTicket의 UserID로 로그인합니다.
            Gamebase.ChangeLogin(forcingMappingTicket, (authTokenForcibly, errorForcibly) =>
            {
                if (Gamebase.IsSuccess(errorForcibly) == true)
                {
                    // 로그인 변경 성공
                }
                else
                {
                    // 로그인 변경 실패
                    // 오류 코드를 확인하고 적절한 처리를 진행합니다.
                }
            });
        }
        else
        {
            // Add Mapping Failed.
            // 오류 코드를 확인하고 적절한 처리를 진행합니다.
        }
    });
}
```

### Remove Mapping

특정 IdP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IdP가 유일한 IdP라면, 실패를 반환합니다.
연동 해제후에는 Gamebase 내부에서, 해당 IdP에 대한 로그아웃처리를 해줍니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RemoveMapping(string providerName, GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void RemoveMapping(string providerName)
{
    Gamebase.RemoveMapping(providerName, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RemoveMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RemoveMapping failed. error is {0}", error));
        }
    });
}
```

### Get Mapping List

사용자 ID에 연동되어 있는 IdP 목록을 반환합니다.<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static List<string> GetAuthMappingList()
```

**Example**

```cs
public void GetAuthMappingList()
{
    List<string> mappingList = Gamebase.GetAuthMappingList();
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
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetUserID()
```

**Example**
```cs
public void GetUserID()
{
    string userID = Gamebase.GetUserID();
}
```

#### AccessToken

Gamebase에서 발급한 Access Token을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAccessToken()
```

**Example**
```cs
public void GetAccessToken()
{
    string accessToken = Gamebase.GetAccessToken();
}
```

#### Last LoggedIn Provider Name

Gamebase에서 마지막 로그인에 성공한 ProviderName을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetLastLoggedInProvider()
```

**Example**
```cs
public void GetLastLoggedInProvider()
{
    string lastLoggedInProvider = Gamebase.GetLastLoggedInProvider();
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

Gamebase Console에 제재된 게임 유저로 등록될 경우,
로그인을 시도하면 아래와 같은 이용 제한 정보 코드가 표시될 수 있습니다. **GamebaseResponse.Auth.BanInfo.from(GamebaseError error)** 메서드를 이용해 제재 정보를 확인할 수 있습니다.

* BANNED_MEMBER(7)

## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급 받은 TransferAccountInfo는 다른 단말기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> <font color="red">[주의]</font><br/>
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, PAYCO 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

**API**

```cs
static void IssueTransferAccount(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void IssueTransferAccount()
{
    Gamebase.IssueTransferAccount((transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Issuing TransferAccount success.
        }
        else
        {
            // Issuing TransferAccount failed.
        }
    });
}
```

### Query TransferAccount
게스트 계정 이전을 위해 이미 발급 받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

**API**

```cs
static void QueryTransferAccount(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void QueryTransferAccount()
{
    Gamebase.QueryTransferAccount((transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Querying TransferAccount success.
        }
        else
        {
            // Querying TransferAccount failed.
        }
    });
}
```


### Renew TransferAccount
이미 발급 받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```cs
static void RenewTransferAccount(GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void RenewTransferAccountManualIdPassword(string accountId, string accountPassword)
{
    // 수동 설정
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountId, accountPassword); // ID + Password
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountPassword); // Password

    // 자동 설정  
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeAutoRenewConfiguration(type);
    
    Gamebase.RenewTransferAccount(configuration, (transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Renewing TransferAccount success.
        }
        else
        {
            // Renewing TransferAccount failed.
        }
    });
}
```




### Transfer Guest Account to Another Device
**issueTransfer** API로 발급 받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급 받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
> * 이미 게스트 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.
> * 만일 잘못된 id/password를 연속해서 입력하면 **AUTH_TRANSFERACCOUNT_BLOCK(3042)** 오류가 발생하며 계정 이전이 일정 시간 차단됩니다.
> 이 경우에는 아래의 예제와 같이 TransferAccountFailInfo 값을 통해 언제까지 계정 이전이 차단되는지 유저에게 알려줄 수 있습니다.

**API**

```cs
static void TransferAccountWithIdPLogin(string accountId, string accountPassword, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void TransferAccountWithIdPLogin(string accountId, string accountPassword)
{
    Gamebase.TransferAccountWithIdPLogin(accountId, accountPassword, (authToken, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Transfering Account success.
            // TODO: implements post login process
        }
        else
        {
            // Check the error code and handle the error appropriately.
            if (error.code == GamebaseErrorCode.AUTH_TRANSFERACCOUNT_BLOCK)
            {
                GamebaseResponse.Auth.TransferAccountFailInfo transferAccountFailInfo = GamebaseResponse.Auth.TransferAccountFailInfo.From(error);
                if (transferAccountFailInfo != null)
                {
                    // Transfering Account failed by entering the wrong id / pw multiple times.
                    // You can tell when the account transfer is blocked by the TransferAccountFailInfo.
                    string failId = transferAccountFailInfo.id;
                    int failCount = transferAccountFailInfo.failCount;
                    DateTime dateTime = new DateTime(transferAccountFailInfo.blockEndDate);
                }
            }
        }
    });
}
```

## TemporaryWithdrawal

'탈퇴 유예' 기능입니다.
임시 탈퇴를 요청하여 즉시 탈퇴가 진행되지 않고 일정 기간의 유예 기간이 지나면 탈퇴가 이루어집니다.
유예 기간은 콘솔에서 변경할 수 있습니다.

> `주의`
>
> 탈퇴 유예 기능을 사용하는 경우에는 **Gamebase.Withdraw()** API 를 사용하지 마세요.
> **Gamebase.Withdraw()** API 는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.member.temporaryWithdrawal로 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

**API**

```cs
public static void RequestWithdrawal(GamebaseCallback.GamebaseDelegate<GamebaseResponse.TemporaryWithdrawalInfo> callback)
```

**Example**

```cs
public void SampleRequestWithdrawal()
{
    Gamebase.TemporaryWithdrawal.RequestWithdrawal((data, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            long gracePeriodDate = data.gracePeriodDate;
            Debug.Log(string.Format("RequestWithdrawal succeeded. The date when you can withdraw your withdrawal is {0}", gracePeriodDate));
        }
        else
        {
            Debug.Log(string.Format("RequestWithdrawal failed. error:{0}", error));
        }
    });
}
```

### Check TemporaryWithdrawal User

탈퇴 유예를 사용하는 게임은 로그인 후 항상 AuthToken.member.temporaryWithdrawal가 null이 아니라면 해당 유저에게 탈퇴 진행중이라는 사실을 알려주어야 합니다.

**Example**

```cs
public void LoginSample()
{
    Gamebase.Login(GamebaseAuthProvider.XXX, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            if(authToken.member.temporaryWithdrawal != null)
            {
                long gracePeriodDate = authToken.member.temporaryWithdrawal.gracePeriodDate;
                Debug.Log(string.Format("User is under temporary withdrawa. GracePeriodDate: {0}", error));
            }            
            else
            {
                string userId = authToken.member.userId;            
                Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.            
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}

```

### Cancel TemporaryWithdrawal

탈퇴를 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

**API**

```cs
static void CancelWithdrawal(GamebaseCallback.ErrorDelegate callback)
```
**Example**

```cs
public void SampleCancelWithdrawal()
{
    Gamebase.TemporaryWithdrawal.CancelWithdrawal((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("CancelWithdrawal succeeded.");
        }
        else
        {
            Debug.Log(string.Format("CancelWithdrawal failed. error:{0}", error));
        }
    });
}
```

### Withdraw Immediately

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 Gamebase.Withdraw() API 와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

**API**

```cs
static void WithdrawImmediately(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void SampleWithdrawImmediately()
{
    Gamebase.TemporaryWithdrawal.WithdrawImmediately((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("WithdrawImmediately succeeded.");
        }
        else
        {
            Debug.Log(string.Format("SampleWithdrawImmediately failed. error:{0}", error));
        }
    });
}
```

## GraceBan

* '결제 어뷰징 자동 해제' 기능입니다.
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 '이용 정지 유예 상태' 후 이용 정지가 되도록 합니다.
    * '이용 정지 유예 상태'일 경우, 설정한 기간 내에 이용 정지 해제 조건을 모두 만족하면 정상적으로 플레이할 수 있습니다.
    * 기간 내에 조건을 충족하지 못하면 이용 정지가 됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.getGraceBanInfo() API를 호출하여, 결과가 null이 아닌 유효한 GraceBanInfo 객체를 반환한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리해야 합니다.

**Example**

```cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.GUEST, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error) == false)
        {
            // Login failed
            return;
        }

        // Check if user is under grace ban
        GamebaseResponse.Common.Member.GraceBanInfo graceBanInfo = authToken.member.graceBan;
        if (graceBanInfo != null)
        {
            string periodDate = string.Format("{0:yyyy/MM/dd HH:mm:ss}", 
                new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(graceBanInfo.gracePeriodDate));
            string message = graceBanInfo.message;
            
            GamebaseResponse.Common.Member.GraceBanInfo.ReleaseRuleCondition releaseRuleCondition = graceBanInfo.releaseRuleCondition;
            if (releaseRuleCondition != null)
            {
                // condition type: "AND", "OR"
                string releaseRule = string.Format("{0}{1} {2} {3}time(s)", releaseRuleCondition.amount,
                    releaseRuleCondition.currency, releaseRuleCondition.conditionType, releaseRuleCondition.count);
            }

            GamebaseResponse.Common.Member.GraceBanInfo.PaymentStatus paymentStatus = graceBanInfo.paymentStatus;
            if (paymentStatus != null) {
                String paidAmount = paymentStatus.amount + paymentStatus.currency;
                String paidCount = paymentStatus.count + "time(s)";
            }

            // Guide the user through the UI how to finish the grace ban status.
        }
        else
        {
            // Login success.
        }
    });
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
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 외부 인증 라이브러리 오류입니다.<br/>상세 오류를 확인하십시오. |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | 이전 인증 프로세스가 완료되지 않았습니다. |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | Gamebase Access Token이 유효하지 않아 로그아웃되었습니다.<br/>로그인을 다시 시도하십시오. |
| TransferAccount| SAME\_REQUESTOR                          | 8          | 발급한 TransferAccount를 동일한 단말기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> NHN Cloud Gamebase Console에서 먼저 설정해주세요. |
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
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제 매핑 키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제 매핑 키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제 매핑 키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제 매핑 키(ForcingMappingKey)가 다른 IDP에 사용되었습니다. <br/>발급 받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제 매핑 키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급 받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IdP는 삭제할 수 없습니다. |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인 되어 있는 IdP입니다. |
| Logout | AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 탈퇴에 실패했습니다.                              |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴를 요청한 유저입니다.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴를 요청한 유저가 아닙니다.                     |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 알 수 없는 오류입니다. (정의되지 않은 오류입니다.) |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 외부 인증 라이브러리에서 오류가 발생했을 때 반환됩니다.
* 외부 인증 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError) == true)
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    if (gamebaseError.code == GamebaseErrorCode.AUTH_EXTERNAL_LIBRARY_ERROR) 
    {
        GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
        if (null != moduleError)
        {
            int moduleErrorCode = moduleError.code;
            string moduleErrorMessage = moduleError.message;        
            Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
        }
    }
}
```

* 상세 오류 코드는 각각의 외부 인증 라이브러리의 Developer 페이지를 참고하시기 바랍니다.
