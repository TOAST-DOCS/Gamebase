## Game > Gamebase > Unity Developer's Guide > Authentication

## Login

Gamebase 에서는 guest 로그인을 기본으로 지원합니다.</br>
guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.</br>
AuthAdapter 대한 설정은 다음의 링크를 참고하시길 바랍니다.

* Android : [설정 링크](./Android Developer`s Guide#dependency)
* iOS : [설정 링크](./iOS Developer`s Guide#setting-xcode-project-to-use-gamebase)

### Login flow
* **LoginForLastLoggedInProvider**를 호출하여, 이전 로그인한 정보를 사용하여 Gamebase 로그인 시도
* Network와 관련된 로그인 실패 시, **LoginForLastLoggedInProvider** 메소드를 사용하여 로그인 재 시도
	* 네트워크 에러 : **SOCKET_ERROR(110)**, **SOCKET_RESPONSE_TIMEOUT(101)**
* Gamebase Server 로그인 실패 시, **Login** 메서드를 호출하여, 로그인 시도

### Banned user of login
이용정지 회원일 경우 LoginForLastLoggedInProvider/Login API를 호출하면 **AUTH_BANNED_MEMBER(3005)** 에러를 리턴합니다.</br>
[GetBanInfo](#gets-banned-user-infomation) API로 ban정보를 가져올 수 있습니다.

### Login as the latest login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다.</br>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다.</br>
이 때는 [해당 IDP에 대한 로그인](#login-using-a-specific-idp)을 구현해야합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

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
                Debug.Log("Try to login using a specifec IDP");
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

Gamebase는 Guest 로그인을 지원합니다.</br>
디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.</br>
Guest 로그인은 디바이스키는 초기화 될 수 있고 디바이스키의 초기화 시에 계정이 삭제될 수 있으므로 IDP를 활용한 로그인 방식을 권장합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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
        }Login with access token of external IDP.
        else
        {
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login using a specific IDP

특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

```cs
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**providerName**
* GamebaseAuthProvider.GOOGLE
* GamebaseAuthProvider.GAMECENTER
* GamebaseAuthProvider.FACEBOOK
* GamebaseAuthProvider.PAYCO


> 몇몇 IDP의 로그인시에는 필수적으로 들어가야하는 정보가 있습니다.</br>
> 예를 들어, facebook 로그인을 구현하기 위해서는 scope 등을 설정해주어야합니다.</br>
> 이러한 필수 정보들을 설정해주기 위해 static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API를 제공합니다.</br>
> 파라미터 additionalInfo에 필수 정보들을 Dictionary 형태로 입력하시면 됩니다. (파라미터 값이 null일 때는, TOAST Cloud Console에 등록한 additionalInfo 값으로 채워집니다. 파라미터 값이 있을 때는 Console에 등록해놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.  [TOAST Cloud Console에 additionalInfo 설정하기](#authentication-additional-information-settings))

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

### Login with access token of external IDP

특정 IDP에 대한 로그인을 직접 구현하고 로그인 후 받아온 AccessToken을 사용하여, 다음 로그인 API를 구현합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

### Authentication additional Information settings

#### Facebook
* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Facebook의 경우, OAuth 인증 시도 시, Facebook으로 부터 요청할 정보의 종류를 설정해야 합니다.

Facebook 인증 추가 정보 입력 예제

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### Payco
* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Payco의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**의 설정이 필요합니다.

Payco 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_code": "Your Service Name" }
```


## Logout

로그아웃 버튼을 클릭했을 때, 다음과 같이 로그아웃 API를 구현합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

탈퇴 버튼을 클릭했을 때, 다음과 같이 탈퇴 API를 구현합니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.</br>
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때, 각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

### Add mapping

특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.</br>
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면, **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니다.

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다.</br>
즉, Mapping은 단순히 IDP를 연동만 해줍니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

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

### Remove mapping

특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다.</br>
연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

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

## Gets mapping list

UserId에 연동되어 있는 IDP 목록을 리턴하게 됩니다.</br>

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

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

## Gets authentication information for Gamebase
Gamebase에서 발급한 인증 정보를 가져올 수 있습니다.

### Gets user id

Gamebase에서 발급한 UserID를 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

### Gets accessToken

Gamebase에서 발급한 AccessToken을 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

### Gets last loggedIn provider

Gamebase에서 마지막 로그인에 성공한 ProviderName을 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

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


## Gets authentication Information for external IDP

외부 인증 SDK에서 AccessToken, UserId, Profile 등의 인증 정보를 가져올 수 있습니다.

### Gets authentication user id for external IDP

외부 인증 SDK에서 UserID를 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

### Gets authentication accessToken for external IDP

외부 인증 SDK에서 AccessToken을 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

### Gets authentication profile for external IDP

외부 인증 SDK에서 Profile을 가져올 수 있습니다.

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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

## Gets banned user infomation

이용정지 정보를 리턴합니다.</br>

**API**<br>
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)
</div>

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