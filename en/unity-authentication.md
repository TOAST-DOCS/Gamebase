## Game > Gamebase > Unity Developer's Guide > Authentication

## Login

Gamebase supports guest logins by default.<br/>

To log into providers other than guest, a matching Provider AuthAdapter is required.<br/>

### Login Flow

Many games require that a login should be implemented on a title screen.

- Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed.
- After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described in the above can be implemented in the following order.

#### 1. Get Latest Login Type
* Call **[TCGBGamebase lastLoggedInProvider]**.
* If there is a returned value, follow **2. Authenticate with Latest Login Type**.
* If there is no returned value, let the game user decide IdP and follow **3.Authenticate with Specified IdP**.

#### 2. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password inputs.
* Call **Gamebase.LoginForLastLoggedInProvider()**.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network Error
  * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a minute.
* Banned Game User
  * If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
  * Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
  * When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup ** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
  * Pop-ups on banning are supported by iOS and Android only.
* Other Errors
  * Authentication with Latest Login Type has failed. Follow **3. Authenticate with Specified IdP**.

#### 3. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
  * Types that can be authenticated are declared in the **GamebaseAuthProvider** class.
* Call **Gamebase.Login(providerName, callback)** API.

#### 3-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 3-2. When Authentication is Failed

* Network Error
  * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a minute.
* Banned Game User
  * If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
  * Check ban information with **Gamebase.GetBanInfo()** and notify the user with reasons for not being able to play.
  * When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup ** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
  * Pop-ups on banning are supported by iOS and Android only.
* Other Errors
  * Notify that an error has occurred to a user, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.


### Login with Latest Login IdP

Try login with the most recently logged-in IdP.<br/>
If a token is expired or its authentication fails, return failure.<br/>
Note that [a login for the IdP](#login-with-idp) should be implemented.

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

Gamebase supports guest logins.<br/>
Create an only key of device to try to log in Gamebase.<br/>
For guest logins, as accounts may be deleted when an app is deleted or device is initialized, it is recommended to use IdP.

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

### Login with IdP

Following is an exemplary code for a login with a specific IdP.

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

> There is information which must be included for login with some IdPs.<br/>
> For instance, scope must be set to implement a Facebook login. <br/>
> In order to set such necessary information, static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API is provided.<br/>
> You can enter those information to additionalInfo in the dictionary type. (When the parameter value is null, the additionalInfo registered in the TOAST Cloud Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console. [Setting additionalInfo in TOAST Cloud Console](#authentication-additional-information-settings))

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

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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

### Set Additional Authentication Information

#### Facebook
* Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL to set json string-type information to **Additional Information**.
  * When trying OAuth authentication, type of information to request to Facebook should be set.

Example of Adding Authentication Information to Facebook

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### PAYCO
* Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL to set json string-type information to **Additional Information**.
  *  **service_code** and **service_name** should be set as PaycoSDK requires.

Example of Adding Authentication Information to PAYCO

```json
{ "service_code": "HANGAME", "service_code": "Your Service Name" }
```

## Logout
Try to log out from logged-in IdP. In many cases, the log-out button is located on game configuration screen to click to implement.
Even if a log-out is successful, data of game users remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords inputs will be required for the next log-in process.<br/><br/>

Following shows an exemplary log-out code with a click of the log-out button.

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
Below shows an example of how to withdraw game users while they're logged-in. <br/><br/>

* When a user is successfully withdrawn, the user's data interfaced with a login IdP are all deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdraw, a log-out from IdP will be tried.

Following shows an exemplary withdrawal code with a click of the withdraw button.

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

In many games, one account may have many integrated (mapped) IdPs.<br/>
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account. <br/><br/>

As such, one Gamebase UserID can be integrated with many IdP accounts.<br/>
In short, a login to an mapped IdP account will be made available with a same user ID at all times.<br/><br/>

In short, a login to an mapped IdP account will be made available with a same user ID at all times.<br/>
Below is an example. <br/><br/>

* Gamebase UserID : 123bcabca
  * Google ID : aa
  * Facebook ID : bb
  * Apple Game Center ID : cc
  * Payco ID : dd
* Gamebase UserID : 456abcabc
  * Google ID : ee
  * Google ID : ff **-> As the Google ee account is integrated, another Google account cannot be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

### Add Mapping

Try mapping to another IdP while logged-in to a specific IdP.<br/>
If an IdP account to map has already been integrated to another account <br/>
return the **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** error. <br/><br/>

Even if a mapping is successful, 'currently logged-in IdP' does not change. For example, after a user logs in a Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.<br/>
Mapping simply adds IdP integration. <br/>

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

### AddMapping with Credential

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.

* How to Set Credential Parameters



| Keyname                                  | Use                                      | Value Type                     |
| ---------------------------------------- | ---------------------------------------- | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                             | facebook, payco, iosgamecenter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |

> [TIP]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK's API, which Gamebase does not support.
>

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

Remove mapping with a specific IdP. If an IdP to remove mapping is an only IdP, return failure.<br/>
After mapping is removed, log out the IdP in Gamebase.

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

Return the list of IdPs mapped to user IDs.<br/>

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
## Gamebase User`s Information

Process authentication with Gamebase, in order to get information required to create an app.

### Get Authentication Information for Gamebase
Process authentication with Gamebase, in order to get information required to create an app.

#### UserID

Get User ID issued by Gamebase.
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

#### AccessToken

Get AccessToken issued by Gamebase.

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

#### Last LoggedIn Provider Name

Get the last logged-in Provider Name in Gamebase.

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

### Get Authentication Information for External IdP

Get access token, User ID, and profiles from externally authenticated SDK.

#### UserID

Get User ID from externally authenticated SDK.

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

#### AccessToken

Get Access Token from externally authentication SDK.

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

#### Profile

Get Profile from externally authenticated SDK.

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

### Get Banned User Infomation

For a banned user registered at Gamebase Console,
restricted use of information code (**AUTH_BANNED_MEMBER(3005)**) can be displayed as below, when trying login. The ban information can be found by using the API as below.

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

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| AUTH_USER_CANCELED                       | 3001       | Login has been canceled.                 |
| AUTH_NOT_SUPPORTED_PROVIDER              | 3002       | The authentication method is not supported. |
| AUTH_NOT_EXIST_MEMBER                    | 3003       | The member does not exist or has withdrawn. |
| AUTH_INVALID_MEMBER                      | 3004       | Request for an invalid member.           |
| AUTH\_BANNED\_MEMBER                     | 3005       | The member has been banned.              |
| AUTH_EXTERNAL_LIBRARY_ERROR              | 3009       | Error in external authentication library. |
| AUTH_TOKEN_LOGIN_FAILED                  | 3101       | Token login has failed.                  |
| AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO      | 3102       | Token information is invalid.            |
| AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103       | Latest login IdP information is invalid. |
| AUTH_IDP_LOGIN_FAILED                    | 3201       | IdP login has failed.                    |
| AUTH_IDP_LOGIN_INVALID_IDP_INFO          | 3202       | IdP information is invalid. (The IdP information is unavailable in console.) |
| AUTH_ADD_MAPPING_FAILED                  | 3301       | Add mapping has failed.                  |
| AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302       | Already mapped to another member.        |
| AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP    | 3303       | Already mapped to the same IdP.          |
| AUTH_ADD_MAPPING_INVALID_IDP_INFO        | 3304       | IdP information is invalid. (The IdP information is unavailable in console.) |
| AUTH_REMOVE_MAPPING_FAILED               | 3401       | Remove mapping has failed.               |
| AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP     | 3402       | Cannot remove the last mapped IdP.       |
| AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP       | 3403       | The IdP is currently logged-in.          |
| AUTH_LOGOUT_FAILED                       | 3501       | Logout has failed.                       |
| AUTH_WITHDRAW_FAILED                     | 3601       | Withdraw has failed.                     |
| AUTH_NOT_PLAYABLE                        | 3701       | Cannot play (due to maintenance or service terminated) |
| AUTH_UNKNOWN_ERROR                       | 3999       | The error is unknown (undefined).        |

* Refer to the following document for the entire error codes.
  - [Entire Error Codes](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* This error occurred in TOAST Cloud external authentication library.
