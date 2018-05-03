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
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a moment.
* Banned game user
    * If the error code is **AUTH_BANNED_MEMBER (3005)**, the authentication has failed due to banned game user.
    * Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
    * When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
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
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a minute.
* Banned game user
    * If the error code is **AUTH_BANNED_MEMBER (3005)**, the authentication has failed due to banned game user.
    * Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
    * When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login with Latest Login IdP

Try login with the most recently logged-in IdP.
If a token is expired or its authentication fails, return failure.
Note that a [login for the IdP](#login-with-idp) should be implemented.

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
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE


```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**providerName**
* GamebaseAuthProvider.GOOGLE(Android/Standalone Only)
* GamebaseAuthProvider.GAMECENTER(iOS Only)
* GamebaseAuthProvider.FACEBOOK
* GamebaseAuthProvider.PAYCO
* GamebaseAuthProvider.NAVER

> There is information which must be included for login with some IdPs.<br/>
> For instance, scope must be set to implement a Facebook login.<br/>
> In order to set such necessary information, static void Login (string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API is provided.<br/>
> You can enter those information to additionalInfo in the dictionary type. When the parameter value is null, the additionalInfo registered in the TOAST Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console. ([Setting additionalInfo in TOAST Console](#authentication-additional-information-settings))<br/>
> Stansalone에서는 WebViewAdapter를 통해서 로그인을 지원하며 WebView가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.


Standalone WebViewAdapter를 사용하여 로그인을 하기 위해서는 IDP 개발자 사이트에서 아래 CallbackURL을 설정 하여야 합니다.

* https://alpha-id-gamebase.toast.com/oauth/callback
* https://beta-id-gamebase.toast.com/oauth/callback
* https://id-gamebase.toast.com/oauth/callback

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

#### NAVER
* Go to **TOAST Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information **to**  **Additional Information**.

* Set URL Schemes.
	* **XCode > Target > Info > URL Types**

Example of Adding Authentication Information to NAVER
```json
{ "url_scheme_ios_only": "Your URL Schemes", "service_name": "Your Service Name" }
```
![Naver URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)


## Logout
Try to log out from logged-in IdP. In many cases, the logout button is located on the game configuration screen.
Even if a logout is successful, a game user's data remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next log-in process.<br/><br/>

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

Mapping refers to connecting or disconnecting an existing login account to/from another IdP account.

In many games, one account may have many integrated (mapped) IdPs.
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.<br/>

As such, one Gamebase UserID can be integrated with many IdP accounts.
In short, a login to a mapped IdP account will be made available with a same user ID at all times.<br/>

Note, however, that each IdP can have only one account to map.
Below shows an example.<br/>

* Gamebase UserID : 123bcabca
    * Google ID : aa
    * Facebook ID : bb
    * AppleGameCenter ID : cc
    * Payco ID : dd
* Gamebase UserID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

### Add Mapping Flow

Implement mapping in the following order.

#### 1. Login
Mapping means to add an IdP account integration to a current account, so login is a prerequisite.
First, call a login API and log in.

#### 2. Mapping

Call **Gamebase.AddMapping()** to try mapping.

#### 2-1.When mapping is successful

* Congratulations! Successfully added an IdP account integrated with the current account.
* Even if a mapping is successful, 'currently logged-in IdP' will not change.<br/>For example, after a user’s login with Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
* Mapping simply adds IdP integration.

#### 2-2. When mapping is failed

* Network error
    * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.AddMapping()** again or try again in a moment.
* Error of integration to another account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, the IdP account to map has been already integrated to another account.To remove the integrated account, log in the account and call **Gamebase.Withdraw()** to withdraw, or call **Gamebase.RemoveMapping()** to remove integration and try mapping again.
* Error of integration to a same IdP account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, a same type of account to the IdP has already been integrated.
	* Gamebase mapping allows only one account of integration to an IdP. For example, if your account is already integrated to a PAYCO account, no other PAYCO account can be added.
	* To integrate another account of a same IdP, call **Gamebase.RemoveMapping()** to remove integration and try mapping again.
* Other Errors
    * Mapping hsa failed.


### Add Mapping

Try mapping to another IdP while logged-in to a specific IdP.
If an IdP account to map has already been integrated to another account,
return the **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER (3302)** error.<br/>

Even if a mapping is successful, 'currently logged-in IdP' does not change. For example, after a user logs in a Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
Mapping simply adds IdP integration.

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



| keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                          | facebook, payco, iosgamecenter, naver |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |

> [TIP]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>


> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK's API, which Gamebase does not support.
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

Remove mapping with a specific IdP. If IdP mapping is not removed, error will occur.
After mapping is removed, Gamebase processes logout of the IdP.

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

Return the list of IdPs mapped to user IDs.<br/>

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

For a banned user registered at Gamebase Console,
restricted use of information code (**AUTH_BANNED_MEMBER(3005)**) can be displayed as below, when trying login. The ban information can be found by using the API as below.

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

## TransferKey

게스트 계정을 다른 단말기로 이전하기 위해 발급받는 키입니다.
발급받은 TransferKey는 다른 기기에서 **requestTransfer** API를 호출하여 계정 이전을 할 수 있습니다.

> `[주의]`
> TransferKey는 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferKey를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑되어 있다면 계정 이전이 지원되지 않습니다.



### Issue TransferKey

게스트 계정 이전을 위한 TransferKey를 발급합니다.
TransferKey의 형식은 영문자 **"소문자/대문자/숫자"를 포함한 8자리의 문자열**입니다.
또한 발급 시간 및 만료 시간을 같이 발급하며, 형식은 epoch time입니다.
* 참고: https://www.epochconverter.com/

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void IssueTransferKey(long expiresIn, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferKeyInfo> callback)
```

**Example**
```cs
public void IssueTransferKey(long expiresIn)
{
	Gamebase.IssueTransferKey(expiresIn, (transferKeyInfo, error) =>
    {
    	if (true == Gamebase.IsSuccess(error))
        {
        	Debug.Log(string.Format("transferKey:{0}", transferKeyInfo.transferKey));
            Debug.Log(string.Format("regDate:{0}", transferKeyInfo.regDate));
            Debug.Log(string.Format("expireDate:{0}", transferKeyInfo.expireDate));
        }
        else
        {
        	Debug.Log(string.Format("IssueTransferKey failed. error is {0}", error));
        }
    });
}
```

### Transfer Guest Account to Another Device
**IssueTransferKey** API로 발급받은 TransferKey를 사용하여 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferKey를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferKey를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.


> `[주의]`
> 이미 Guest 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestTransfer(string transferKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**
```cs
public void RequestTransfer(string transferKey)
{
	Gamebase.RequestTransfer(transferKey, (authToken, error) =>
    {
    	if (true == Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
            Debug.Log(string.Format("RequestTransfer succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
        	Debug.Log(string.Format("RequestTransfer failed. error is {0}", error));
        }
    });
}
```


## Error Handling


| Category | Error                                    | Error Code | Description                                    |
| ---  | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth | AUTH_USER_CANCELED | 3001 | Login is cancelled. |
|      | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | The authentication is not supported. |
|      | AUTH_NOT_EXIST_MEMBER | 3003 | Named member does not exist or has withdrawn. |
|      | AUTH_INVALID_MEMBER | 3004 | Request for invalid member |
|      | AUTH_BANNED_MEMBER | 3005 | Named member has been banned. |
|      | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | Error in external authentication library |
| TransferKey | SAME\_REQUESTOR | 8 | 발급한 TransferKey를 동일한 기기에서 사용했습니다. |
|             | NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IDP가 연동되어 있습니다. |
|             | AUTH\_TRANSFERKEY\_EXPIRED | 3031 | TransferKey의 유효기간이 만료됐습니다. |
|             | AUTH\_TRANSFERKEY\_CONSUMED | 3032 | TransferKey가 이미 사용됐습니다. |
|             | AUTH\_TRANSFERKEY\_NOT\_EXIST | 3033 | TransferKey가 유효하지 않습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | Token login has failed. |
|              | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | Invalid token information |
|              | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | Invalid last login IDP information |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IDP login has failed. |
|           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | Invalid IDP information (IDP information does not exist in the Console.) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | Add mapping has failed. |
|             | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | Already mapped to another member. |
|             | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | Already mapped to same IDP. |
|             | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | Invalid IDP information (IDP information does not exist in the Console.) |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | Remove mapping has failed. |
|                | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | Cannot delete last mapped IDP. |
|                | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | Currently logged-in IDP |
| Logout | AUTH_LOGOUT_FAILED | 3501 | Logout has failed. |
| Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | Withdrawal has failed. |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | Not playable (due to maintenance or service closed) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | Unknown error (Undefined error) |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-codes#client-sdk)



**AUTH_EXTERNAL_LIBRARY_ERROR**

* Occurs in TOAST external authentication library.
* 오류 코드 확인은 다음과 같이 확인하실 수 있습니다.

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    Error moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* IdP SDK의 오류 코드는 각각의 Developer 페이지를 참고하시기 바랍니다.