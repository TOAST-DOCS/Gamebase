## Game > Gamebase > Unity Developer's Guide > Authentication

## Login

* Gamebase 에서는 Guest 로그인을 기본으로 지원합니다.<br/>
* Guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.<br/>
* AuthAdapter 대한 설정은 다음의 링크를 참고하시길 바랍니다.
	* [LINK \[Android Setting\]](aos-started#dependency)
	* [LINK \[iOS Setting\]](ios-started#installation)


### Login Flow

* 많은 게임이 타이틀 화면에서 로그인을 구현합니다.
	* 앱을 설치 후 처음 실행했다면 타이틀 화면에서 어떤 IDP로 인증할지 선택할 수 있도록 하여 유저가 선택한 IDP로 인증합니다.
	* 로그인에 한번 성공한 이후에는 IDP 선택화면을 표시하지 않고 이전에 로그인에 성공했던 IDP 타입으로 인증합니다.
* 위에서 설명한 로직을 다음과 같은 순서로 구현할 수 있습니다.

#### 1. 이전 로그인 타입 받아오기
* **Gamebase.GetLastLoggedInProvider()**를 호출합니다.
* 리턴된 값이 존재한다면 **'2. 이전 로그인 타입으로 인증하기'**를 진행합니다.
* 리턴된 값이 없다면 유저에게 IDP를 선택하도록 한 다음 **'3. 지정된 IDP로 인증하기'**를 진행합니다.

#### 2. 이전 로그인 타입으로 인증하기

* 이전에 인증했던 기록이 있다면 ID/PW를 입력받지 않고 인증을 시도합니다.
* **Gamebase.LoginForLastLoggedInProvider()**를 호출합니다.

#### 2-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.GetUserID()**로 UserID를 획득하여 게임을 진행하세요.

#### 2-2. 인증이 실패한 경우

* 네트워크 에러
	* 에러코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.LoginForLastLoggedInProvider()** 를 다시 호출 하거나, 잠시 대기했다가 재시도 하도록 합니다.
* 이용 정지 유저
	* 에러 코드가 **AUTH_BANNED_MEMBER(3005)** 인 경우, 이용 정지 유저이므로 인증이 실패한 것입니다.
	* **Gamebase.GetBanInfo()** 로 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 이유를 알려주시기 바랍니다.
	* Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup**값을  true로 한다면 Gamebase 가 이용 정지에 관한 팝업을 자동으로 띄워줍니다.
	* 이용 정지에 관한 팝업은 iOS와 Android에서만 지원합니다.
* 그 외의 에러
	* 이전 로그인 타입으로 인증하기가 실패하였습니다. **'3. 지정된 IDP로 인증하기'**를 진행합니다.

#### 3. 지정된 IDP로 인증하기

* IDP 타입을 직접 지정하여 인증을 시도합니다.
	* 인증 가능한 타입은 **GamebaseAuthProvider** 클래스에 선언되어 있습니다.
* **Gamebase.Login(providerName, callback)** API를 호출합니다.

#### 3-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.GetUserID()** 로 UserID를 획득하여 게임을 진행하세요.

#### 3-2. 인증이 실패한 경우

* 네트워크 에러
	* 에러코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.Login(providerName, callback)** 를 다시 호출 하거나, 잠시 대기했다가 재시도 하도록 합니다.
* 이용 정지 유저
	* 에러 코드가 **AUTH_BANNED_MEMBER(3005)** 인 경우, 이용 정지 유저이므로 인증이 실패한 것입니다.
	* **Gamebase.GetBanInfo()** 로 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 이유를 알려주시기 바랍니다.
	* Gamebase 초기화 시 **GamebaseConfiguration.enablePopup** 및 **GamebaseConfiguration.enableBanPopup**값을  **true**로 한다면 Gamebase 가 이용 정지에 관한 팝업을 자동으로 띄워줍니다.
	* 이용 정지에 관한 팝업은 iOS와 Android에서만 지원합니다.
* 그 외의 에러
	* 에러가 발생했다는 것을 유저에게 알리고, 유저가 인증 IDP 타입을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다.<br/>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다.<br/>
이 때는 [해당 IDP에 대한 로그인](#login-with-idp)을 구현해야합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

Gamebase는 Guest 로그인을 지원합니다.<br/>
디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.<br/>
Guest 로그인은 디바이스키는 초기화 될 수 있고 디바이스키의 초기화 시에 계정이 삭제될 수 있으므로 IDP를 활용한 로그인 방식을 권장합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### Login with IDP

특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**providerName**
* GamebaseAuthProvider.GOOGLE
* GamebaseAuthProvider.GAMECENTER
* GamebaseAuthProvider.FACEBOOK
* GamebaseAuthProvider.PAYCO

> 몇몇 IDP의 로그인시에는 필수적으로 들어가야하는 정보가 있습니다.<br/>
> 예를 들어, facebook 로그인을 구현하기 위해서는 scope 등을 설정해주어야합니다.<br/>
> 이러한 필수 정보들을 설정해주기 위해 static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API를 제공합니다.<br/>
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

### Login with Credential

게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증을 하고 발급받은 AccessToken등을 이용하여, Gamebase 로그인을 할 수 있는 인터페이스 입니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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
로그인 된 IDP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에서 로그아웃 버튼을 두고 클릭시 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 유저 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IDP로 인증했던 기록을 제거하므로 다음 로그인시 ID/PW 입력창이 노출됩니다.

다음과 같이 구현합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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
로그인 상태에서 탈퇴를 시도합니다.<br/>

* 탈퇴에 성공하면, 로그인 했던 IDP와 연동 되어 있던 유저 데이터는 삭제 됩니다.
* 해당 IDP로 다시 로그인 가능하고 새로운 유저 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IDP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IDP 로그아웃을 시도하게 합니다.

탈퇴 버튼을 클릭했을 때 다음과 같이 탈퇴 API를 구현합니다.

**API**<br>

![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

많은 게임들이 하나의 계정에 여러 IDP를 연동(Mapping)할 수 있도록 하고 있습니다.
Gamebase의 Mapping API를 사용하여 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시킬 수 있습니다.<br/>
이렇게 하나의 Gamebase UserID에 다양한 IDP 계정을 연동할 수 있습니다.
즉, 연동 중인 IDP 계정으로 로그인을 시도 한다면 항상 동일한 UserID로 로그인 됩니다.<br/>
주의할 점은, IDP 마다 하나의 계정씩만 연동이 가능합니다.
예시는 다음과 같습니다.
* Gamebase UserID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* Gamebase UserID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> 이미 Google ee 계정이 연동중이므로 Google계정을 추가로 연동할 수 없습니다.**

### Add Mapping

특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.<br/>
Mapping을 하려는 IDP의 계정이 이미 다른 계정에 연동이 되어있다면
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니다.

Mapping이 성공 하더라도 '현재 로그인 중인 IDP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인 한 후, Facebook 계정 Mapping 시도가 성공했다고 해서 '현재 로그인 중인 IDP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.<br/>
Mapping은 단순히 IDP 연동만 추가 해줍니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

### Remove Mapping

특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다.<br/>
연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

UserId에 연동되어 있는 IDP 목록을 리턴하게 됩니다.<br/>

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

## Get Authentication Information for Gamebase
Gamebase를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

### UserID

Gamebase에서 발급한 UserID를 가져올 수 있습니다.
**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### AccessToken

Gamebase에서 발급한 AccessToken을 가져올 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### Last LoggedIn Provider Name

Gamebase에서 마지막 로그인에 성공한 ProviderName을 가져올 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

## Get Authentication Information for External IDP

외부 인증 SDK에서 AccessToken, UserId, Profile 등의 인증 정보를 가져올 수 있습니다.

### UserID

외부 인증 SDK에서 UserID를 가져올 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### AccessToken

외부 인증 SDK에서 AccessToken을 가져올 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### Profile

외부 인증 SDK에서 Profile을 가져올 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

## Get Banned User Infomation

Gamebase Console에 제재된 유저로 등록될 경우,
로그인 시도 시, 이용제한 정보 코드(**AUTH_BANNED_MEMBER(3005)**)가 노출 될 수 있으며, 아래 API를 이용하여 제재 정보를 확인할 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| AUTH_USER_CANCELED | 3001 | 로그인이 취소되었습니다. |
| AUTH_NOT_SUPPORTED_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
| AUTH_NOT_EXIST_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
| AUTH_INVALID_MEMBER | 3004 | 잘못된 회원에 대한 요청입니다. |
| AUTH\_BANNED\_MEMBER | 3005 | 제재된 회원입니다. |
| AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 외부 인증 라이브러리 에러입니다. |
| AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
| AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
| AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IDP 정보가 없습니다. |
| AUTH_IDP_LOGIN_FAILED | 3201 | IDP 로그인에 실패하였습니다. |
| AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
| AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
| AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
| AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IDP에 맵핑되어있습니다. |
| AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
| AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
| AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IDP는 삭제할 수 없습니다. |
| AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인되어있는 IDP 입니다. |
| AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| AUTH_WITHDRAW_FAILED | 3601 | 탈퇴에 실패하였습니다. |
| AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| AUTH_UNKNOWN_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |

* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 에러는 외부 인증 라이브러리에서 발생한 에러입니다.

