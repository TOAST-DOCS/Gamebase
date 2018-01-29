## Game > Gamebase > Unity Developer's Guide > Authentication

## Login

Gamebase supports Guest logins by default.<br/>

To log into providers other than guest, a matching Provider AuthAdapter is required.<br/>

### Login Flow

In many games, login is implemented on a title page. 

* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed. 
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described in the above can be implemented in the following order.

#### 1. Get Latest Login Type
* Call **[TCGBGamebase lastLoggedInProvider].
* If there is a returned value, follow **2. Authenticate with Latest Login Type**.
* If there is no returned value, let the game user decide IdP and follow **3.Authenticate with Specified IdP**.

#### 2. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password inputs.
* Call **Gamebase.LoginForLastLoggedInProvider()**.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 2-2.When Authentication is Failed

* Network error
	* If the error code is **SOCKET\_ERROR (110)** or **SOCKET\_RESPONSE\_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a moment.
* Banned game user
	* If the error code is **AUTH\_BANNED\_MEMBER (3005)**, the authentication has failed due to banned game user.
  	* Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
  	* When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
	* Pop-ups on banning are supported by iOS and Android only.
* Other errors
  	* Authentication with latest login type has failed. Follow **3. Authenticate with Specified IdP**.

#### 3. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
  * Types that can be authenticated are declared in the **GamebaseAuthProvider** class.
* Call **Gamebase.Login(providerName, callback)** API.

#### 3-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 3-2. When Authentication is Failed

* Network error
	* If the error code is **SOCKET\_ERROR (110)** or **SOCKET\_RESPONSE\_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a minute.
* Banned game user
	* If the error code is **AUTH\_BANNED\_MEMBER (3005)**, the authentication has failed due to banned game user.
	* Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
  	* When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
  	* Pop-ups on banning are supported by iOS and Android only.
* Other errors
  	* Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login with Latest Login IdP

Try login with the most recently logged-in IdP.
If a token is expired or its authentication fails, return failure.
Note that a login for the IdP should be implemented.

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

Gamebase supports Guest logins.
Create an only key of device to try to log in Gamebase.
As the device key may be initialized and account may be deleted, it is recommended to use IdP for a Guest login.

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

Following is a login example with a specific IdP.

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

> There is information which must be included for login with some IdPs.<br/>
> For instance, scope must be set to implement a Facebook login.<br/>
> In order to set such necessary information, static void Login (string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API is provided.<br/>
> You can enter those information to additionalInfo in the dictionary type. (When the parameter value is null, the additionalInfo registered in the TOAST Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console. Setting additionalInfo in TOAST Console)

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

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

UnityEditor supports Facebook login only.

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
* Go to **TOAST Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type informationto **Additional Information**.
  * When trying OAuth authentication, type of information to request to Facebook should be set.


Example of adding authentication information in Facebook

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### PAYCO

* Go to **TOAST Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information **to**  **Additional Information**.
  * **service_code** and **service_name** should be set as PaycoSDK requires.


Example of adding authentication information in PAYCO

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

## Logout
Try to log out from logged-in IdP. In many cases, the logout button is located on the game configuration screen. Even if a logout is successful, a game user's data remain. When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next log-in process.<br/><br/>

Following shows an example logout code with a click of the logout button.

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

Below shows an example of how a game user withdraws while logged-in.

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdrawal, a log-out from IdP will be tried.

Following shows an example withdrawal code with a click of the withdrawal button.

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

In many games, one account may have many integrated (mapped) IdPs.
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.<br/>

As such, one Gamebase UserID can be integrated with many IdP accounts.
In short, a login to a mapped IdP account will be made available with a same user ID at all times.<br/>

Note, however, that each IdP can have only one account to map.
Below shows an example.<br/>

- Gamebase UserID : 123bcabca
  - Google ID : aa
  - Facebook ID : bb
  - AppleGameCenter ID : cc
  - Payco ID : dd

Gamebase UserID: 456abcabc
Google ID: ee
Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.


### {@수정}Add Mapping Flow

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

Try mapping to another IdP while logged-in to a specific IdP. If an IdP account to map has already been integrated to another account, return the **AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER (3302)** error.

Even if a mapping is successful, 'currently logged-in IdP' does not change. For example, after a user logs in a Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account. Mapping simply adds IdP integration.

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

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.

* How to Set Credential Parameters



| keyname | a use | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                          | facebook, payco, iosgamecenter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |

> [TIP]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>


> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK&#39;s API, which Gamebase does not support.
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

Remove mapping with a specific IdP. If IdP mapping is not removed, error will occur.  After mapping is removed, Gamebase processes logout of the IdP.

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

Return the list of IdPs mapped to user IDs.

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

Process authentication with Gamebase, in order to get information required to create an app.

### Get Authentication Information for Gamebase

Process authentication with Gamebase, in order to get information required to create an app.

#### UserID

Get User ID issued by Gamebase.

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

Get AccessToken issued by Gamebase.

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

Get the last logged-in Provider Name in Gamebase.

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

Get access token, User ID, and profiles from externally authenticated SDK.

#### UserID

Get User ID from externally authenticated SDK.

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

Get Access Token from externally authentication SDK.

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

Get Profile from externally authenticated SDK.

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

For a banned user registered at Gamebase Console,restricted use of information code (**AUTH\_BANNED\_MEMBER(3005)**) can be displayed as below, when trying login. The ban information can be found by using the API as below.

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
| AUTH_USER_CANCELED | 3001 | Login is cancelled. |
| AUTH_NOT_SUPPORTED_PROVIDER | 3002 | The authentication is not supported. |
| AUTH_NOT_EXIST_MEMBER | 3003 | Named member does not exist or has withdrawn. |
| AUTH_INVALID_MEMBER | 3004 | Request for invalid member |
| AUTH_BANNED_MEMBER | 3005 | Named member has been banned. |
| AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | Error in external authentication library |
| AUTH_TOKEN_LOGIN_FAILED | 3101 | Token login has failed. |
| AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | Invalid token information |
| AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | Invalid last login IDP information |
| AUTH_IDP_LOGIN_FAILED | 3201 | IDP login has failed. |
| AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | Invalid IDP information (IDP information does not exist in the Console.) |
| AUTH_ADD_MAPPING_FAILED | 3301 | Add mapping has failed. |
| AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | Already mapped to another member. |
| AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | Already mapped to same IDP. |
| AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | Invalid IDP information (IDP information does not exist in the Console.) |
| AUTH_REMOVE_MAPPING_FAILED | 3401 | Remove mapping has failed. |
| AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | Cannot delete last mapped IDP. |
| AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | Currently logged-in IDP |
| AUTH_LOGOUT_FAILED | 3501 | Logout has failed. |
| AUTH_WITHDRAW_FAILED | 3601 | Withdrawal has failed. |
| AUTH_NOT_PLAYABLE | 3701 | Not playable (due to maintenance or service closed) |
| AUTH_UNKNOWN_ERROR | 3999 | Unknown error (Undefined error) |

* Refer to the following document for the entire error codes.
  * [Entire Error Codes](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* Occurs in TOAST external authentication library.

