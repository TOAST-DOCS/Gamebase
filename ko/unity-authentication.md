## Game > Gamebase > Unity SDK 사용 가이드 > 인증

## Login

Gamebase에서는 게스트 로그인을 기본으로 지원합니다.<br/>

게스트 이외의 Provider에 로그인하려면 해당 Provider AuthAdapter가 필요합니다.<br/>

### Login Flow

많은 게임이 타이틀 화면에서 로그인을 구현합니다.

* 앱을 설치하고 처음 실행했을 때 타이틀 화면에서 게임 이용자가 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.
* 한 번 로그인한 후에는 IdP 선택 화면을 표시하지 않고 이전에 로그인한 IdP 유형으로 인증합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.

#### 1. 이전 로그인 유형 받아오기
* **Gamebase.GetLastLoggedInProvider()**를 호출합니다.
* 반환된 값이 있으면 **2. 이전 로그인 유형으로 인증**을 진행합니다.
* 반환된 값이 없다면 게임 이용자에게 IdP를 선택하게 한 다음 **3. 지정된 IdP로 인증**을 진행합니다.

#### 2. 이전 로그인 유형으로 인증

* 이전에 인증했던 기록이 있다면 ID와 비밀번호를 입력받지 않고 인증을 시도합니다.
* **Gamebase.LoginForLastLoggedInProvider()**를 호출합니다.

#### 2-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **Gamebase.GetUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 2-2. 인증이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.LoginForLastLoggedInProvider()**를 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 이용자
    * 오류 코드가 **AUTH_BANNED_MEMBER(3005)**인 경우, 이용 정지 게임 이용자이므로 인증에 실패한 것입니다.
    * **Gamebase.GetBanInfo()**로 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 이유를 알려주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup **값을  true로 한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 이전 로그인 유형으로 인증하기가 실패하였습니다. **'3. 지정된 IdP로 인증'**을 진행합니다.

#### 3. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 **GamebaseAuthProvider** 클래스에 선언돼 있습니다.
* **Gamebase.Login(providerName, callback)** API를 호출합니다.

#### 3-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.GetUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 3-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **Gamebase.Login(providerName, callback)**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 이용자
    * 오류 코드가 **AUTH_BANNED_MEMBER(3005)**인 경우, 이용 정지 게임 이용자이므로 인증에 실패한 것입니다.
    * **Gamebase.GetBanInfo()**로 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup **값을  **true**로 한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외의 오류
    * 오류가 발생했다는 것을 게임 이용자에게 알리고, 게임 이용자가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IdP

가장 최근에 로그인한 IdP로 로그인을 시도합니다.
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등에 실패하면 실패를 반환합니다.
이때는 [해당 IdP에 대한 로그인](#login-with-idp)을 구현해야합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void LoginForLastLoggedInProvider(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
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
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user. : {0}", error.message));
            }
            else
            {
                Debug.Log("Try to login using a specifec IdP");
                Login("ProviderName");
            }
        }
    });
}

public void Login(string providerName)
{
    Gamebase.Login(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with GUEST

Gamebase는 게스트 로그인을 지원합니다.
디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
게스트 로그인은 디바이스 키는 초기화될 수 있고 디바이스 키의 초기화 시에 계정이 삭제될 수 있으므로 IdP를 활용한 로그인 방식을 권장합니다.

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
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with IdP

다음은 특정 IdP로 로그인할 수 있게 하는 예시 코드입니다

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**providerName**
* GamebaseAuthProvider.GOOGLE
* GamebaseAuthProvider.GAMECENTER
* GamebaseAuthProvider.FACEBOOK
* GamebaseAuthProvider.PAYCO

> 몇몇 IdP로 로그인할 때는 꼭 필요한 정보가 있습니다.<br/>
> 예를 들어, Facebook 로그인을 구현하려면 scope 등을 설정해야 합니다.<br/>
> 이러한 필수 정보들을 설정할 수 있게 static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API를 제공합니다.<br/>
> 파라미터 additionalInfo에 필수 정보들을 dictionary 형태로 입력하시면 됩니다. (파라미터값이 null일 때는, TOAST Console에 등록한 additionalInfo값으로 채워집니다. 파라미터값이 있을 때는 Console에 등록해 놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.  [TOAST Console에 additionalInfo 설정하기](#authentication-additional-information-settings))

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
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}

public void Login(string providerName, Dictionary<string, object> additionalInfo)
{
    Gamebase.Login(providerName, additionalInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급받은 액세스 토큰 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

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
public void Login(Dictionary<string, object> credentialInfo)
{
	Gamebase.Login(credentialInfo, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Authentication Additional Information Settings

#### Facebook
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
    * Facebook의 경우, OAuth 인증 시도 시, Facebook에 요청할 정보의 종류를 설정해야 합니다.

Facebook 인증 추가 정보 입력 예제

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### PAYCO
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
    * PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_code": "Your Service Name" }
```

## Logout
로그인 된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 이용자 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 노출됩니다.<br/><br/>

다음은 로그아웃 버튼을 클릭하면 로그아웃이 되는 예시 코드입니다.

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

* 탈퇴에 성공하면, 로그인했던 IdP와 연동되어 있던 게임 이용자 데이터는 삭제됩니다.
* 해당 IdP로 다시 로그인 가능하고 새로운 게임 이용자 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IdP 로그아웃을 시도합니다.

다음은 탈퇴 버튼을 클릭하면 탈퇴가 되는 예시 코드입니다.

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
    * 에러 코드가 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)** 인 경우, 매핑하려는 IdP와 같은 종류의 계정이 이미 연동중이라는 뜻입니다.
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

게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증을 하고 발급받은 액세스 토큰등을 이용하여, Gamebase AddMapping을 할 수 있는 인터페이스 입니다.

* Credential 파라미터의 설정방법



| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdP 유형 설정                           | facebook, payco, iosgamecenter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정 |                                |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유기능의 사용이 필요할 때 사용될 수 있습니다.
>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원요구하는 개발사항은 외부SDK의 API를 사용하여 구현해야하며, Gamebase에서는 지원하지 않습니다.
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
public void AddMapping(Dictionary<string, object> credentialInfo)
{
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

Gamebase에서 발급한 액세스 토큰을 가져올 수 있습니다.

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

외부 인증 SDK에서 액세스 토큰, 사용자 ID, Profile 등의 인증 정보를 가져올 수 있습니다.

#### UserID

외부 인증 SDK에서 사용자 ID를 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAuthProviderUserID()
```

**Example**

```cs
public void GetAuthProviderUserID(string providerName)
{
    string authProviderUserID = Gamebase.GetAuthProviderUserID(providerName);
}
```

#### AccessToken

외부 인증 SDK에서 액세스 토큰을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAuthProviderAccessToken(string providerName)
```

**Example**
```cs
public void GetAuthProviderAccessToken(string providerName)
{
    string authProviderAccessToken = Gamebase.GetAuthProviderAccessToken(providerName);
}
```

#### Profile

외부 인증 SDK에서 Profile을 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Auth.AuthProviderProfile GetAuthProviderProfile(string providerName)
```

**Example**
```cs
public void GetAuthProviderProfile(string providerName)
{
    GamebaseRequest.AuthProviderProfile profile = Gamebase.GetAuthProviderProfile(providerName);
}
```

### Get Banned User Infomation

Gamebase Console에 제재된 게임 이용자로 등록될 경우,
로그인 시도 시, 이용 제한 정보 코드(**AUTH_BANNED_MEMBER(3005)**)가 표시될 수 있으며, 아래 API를 이용하여 제재 정보를 확인할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Auth.BanInfo GetBanInfo()
```

**Example**
```cs
public void GetBanInfo()
{
    GamebaseResponse.Auth.BanInfo banInfo = Gamebase.GetBanInfo();
}
```

## Error Handling

| Error                                    | Error Code | Description                                    |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| AUTH_USER_CANCELED | 3001 | 로그인이 취소되었습니다. |
| AUTH_NOT_SUPPORTED_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
| AUTH_NOT_EXIST_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
| AUTH_INVALID_MEMBER | 3004 | 잘못된 회원에 대한 요청입니다. |
| AUTH\_BANNED\_MEMBER | 3005 | 제재된 회원입니다. |
| AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 외부 인증 라이브러리 에러입니다. |
| AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
| AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
| AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IdP 정보가 없습니다. |
| AUTH_IDP_LOGIN_FAILED | 3201 | IdP 로그인에 실패하였습니다. |
| AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
| AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
| AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IdP에 맵핑되어있습니다. |
| AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
| AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IdP는 삭제할 수 없습니다. |
| AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인 되어 있는 IdP 입니다. |
| AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| AUTH_WITHDRAW_FAILED | 3601 | 탈퇴에 실패하였습니다. |
| AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| AUTH_UNKNOWN_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 외부 인증 라이브러리에서 발생한 오류입니다.

