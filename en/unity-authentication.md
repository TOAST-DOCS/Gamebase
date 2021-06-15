## Game > Gamebase > Unity Developer's Guide > Authentication

## Login

Gamebase supports Guest logins by default.<br/>

* To log into providers other than guest, a matching Provider AuthAdapter is required.
* For setting of AuthAdapter and 3rd-Party Provider SDK, refer to
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)

### Login Flow

In many games, login is implemented on a title page.

* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed.
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described in the above can be implemented in the following order.

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password inputs.
* Call **Gamebase.LoginForLastLoggedInProvider()**.

#### 1-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 1-2.When Authentication is Failed

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a moment.
* Banned game user
    * When the error code is found as **BANNED_MEMBER(7)** , authentication has failed because the user is banned from the game.
    * Check the reason for ban using **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)** and inform the game user why they cannot play the game.
    * When **GamebaseConfiguration.enablePopup** and **GamebaseConfiguration.enableBanPopup** are set as true during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * Authentication with latest login type has failed. Follow **3. Authenticate with Specified IdP**.

#### 2. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
    * Types that can be authenticated are declared in the **GamebaseAuthProvider** class.
* Call **Gamebase.Login(providerName, callback)** API.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.GetUserID()** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.LoginForLastLoggedInProvider()** again, or try again in a minute.
* Banned game user
    * When the error code is found as **BANNED_MEMBER(7)**, authentication has failed because the user is banned from the game.
    * Check the reason for ban using **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)** and inform the game user why they cannot play the game.
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
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("Login failed. error is {0}", error));
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user. : {0}", error.message));
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

Gamebase supports Guest logins. 

* Create an only key of device to try to log in Gamebase. 
* As the device key may be initialized and account may be deleted, it is recommended to use IdP for a Guest login.

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
            	Debug.Log(string.Format("Retry Login or notify an error message to the user. : {0}", error.message));
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

Following is a login example with a specific IdP. 
For more information on IdP types that can be used to log in, refer to the **GamebaseAuthProvider** class.

> [Notes]
>
> Some IdPs require additional information when logging in.
> The static void Login(string providerName, Dictionary additionalInfo, and GamebaseCallback.GamebaseDelegate callback) APIs are provided to configure these additional information.
>Enter the required information into the additionalInfo parameter in the form of the dictionary.
>> If additionalInfo has a value, use that value. If null, use the value registered in [NHN Cloud Console](./oper-app/#authentication-information).


> <font color="red">[Caution]</font><br/>
>
> In Standalone, login is supported through WebViewAdapter. It does not block events entered via UI when WebView is open.
>
> To log in using Standalone WebViewAdapter, the CallbackURL below must be configured on the IdP Developer website.
> - https://id-gamebase.toast.com/oauth/callback
>

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
            	Debug.Log(string.Format("Retry Login or notify an error message to the user. : {0}", error.message));
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
            	Debug.Log(string.Format("Retry Login or notify an error message to the user. : {0}", error.message));
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

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.

* How to Set Credential Parameters

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (access token) received after login IdP.<br/>Not applied for Google authentication. |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Enter One Time Authorization Code (OTAC) which can be obtained after Google login. |                                          |

> [Note]
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
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

UnityEditor supports Facebook login only.

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
            	Debug.Log(string.Format("Retry Login or notify an error message to the user. : {0}", error.message));
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

Try to log out from logged-in IdP. In many cases, the logout button is located on the game configuration screen.
Even if a logout is successful, a game user's data remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next log-in process.<br/><br/>

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

Attempts account withdrawal while logged in.

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdrawal, a log-out from IdP will be tried.

> <font color="red">[Caution]</font><br/>
>
> If multiple IdPs are linked, all IdP linkages will be unlinked and the game user data in Gamebase will be deleted.
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
* Gamebase UserID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

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
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (access token) received after login IdP |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Enter One Time Authorization Code (OTAC) which can be obtained after Google login. |                                          |

> [Note]
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
If there is any account mapped to a specific IdP, try **force** mapping.
When you try **force mapping**, you need `ForcingMappingTicket` obtained from the AddMapping API.

The following is an example of force mapping to Facebook:

**API**

```cs
static void AddMappingForcibly(string providerName, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(string idPName)
{
    Gamebase.AddMapping(idPName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Successfully added mapping
        }
        else
        {
            // First, call the addMapping API to try mapping to an already linked account and get ForcingMappingTicket as follows.
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // Gets the ForcingMappingTicket instance using the From() method of the ForcingMappingTicket class.
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

                // Try force mapping.
                Gamebase.AddMappingForcibly(idPName, forcingMappingTicket.forcingMappingKey, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // Successfully added force mapping
                    }
                    else
                    {
                        // Failed to add force mapping
                        // Check the error code and resolve the error.
                    }
                });
            }
            else
            {
                // Check the error code and resolve the error.
            }
        }
    });
}
```


### Add Mapping Forcibly with Credential
If there is any account mapped to a specific IdP, try **force** mapping.
When you try **force mapping**, you need `ForcingMappingTicket` obtained from the AddMapping API.

This interface allows you to perform authentication in the game with the SDK provided by IdP first and then to call the Gamebase AddMappingForcibly by using the access token issued.

* How to set the Credential parameter

| keyname | a use | Value type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP type setting                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set the authentication information (access token) received after IdP login.<br/>It is not used for Google authentication. |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Set the authentication information (Authorization Code) received after login with Google |                                        |

> [TIP]
>
> This may be needed to use unique functions of external services (e.g. Facebook).
>


> <font color="red">[Caution]</font><br/>
>
> Developments requested by an external SDK must be implement using APIs from the external SDK; Gamebase does not support those development features.
>

The following shows an example of force mapping.

**API**

```cs
static void AddMappingForcibly(Dictionary<string, object> credentialInfo, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(Dictionary<string, object> credential)
{
    Gamebase.AddMapping(credential, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Successfully added mapping
        }
        else
        {
            // First, call the addMapping API to try mapping to an already linked account and get ForcingMappingTicket as follows.
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // Gets the ForcingMappingTicket instance using the From() method of the ForcingMappingTicket class.
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

                // Try force mapping.
                Gamebase.AddMappingForcibly(credential, forcingMappingTicket.forcingMappingKey, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // Successfully added force mapping
                    }
                    else
                    {
                        // Failed to add force mapping
                        // Check the error code and resolve the error.
                    }
                });
            }
            else
            {
                // Add Mapping Failed.
                // Check the error code and resolve the error.
            }
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

### Get Banned User Information

If a user is registered while being banned in Gamebase Console,
the user will see the following usage restriction code when attempting to log in to the game. The **GamebaseResponse.Auth.BanInfo.from(GamebaseError error)** method can be used to check the ban information.

* BANNED_MEMBER(7)

## TransferAccount
Issues a key to transfer the guest account to another device.

This key is called **TransferAccountInfo**.
The issued TransferAccountInfo calls the **requestTransferAccount** API from another device to transfer the account.

> <font color="red">[Caution]</font><br/>
> The TransferAccountInfo key can be issued while the guest account is logged in.
> Transfer of guest account using TransferAccountInfo is allowed only when logged in to a guest account or not logged in.
> If the logged-in guest account has already been mapped to an IdP ((Google, Facebook, PAYCO, etc.)) account, account transfer is not supported.

### Issue TransferAccount
Issues TransferAccountInfo to transfer the guest account.

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
Queries the TransferAccountInfo information issued for guest account transfer to the Gamebase server.

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
Renews the issued TransferAccountInfo information.
There are two types of renewal: **Auto Renew** and **Manual Renew**. You can select either **Renew Password Only** or **Renew Both ID and Password** to renew the TransferAccountInfo information.

```cs
static void RenewTransferAccount(GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void RenewTransferAccountManualIdPassword(string accountId, string accountPassword)
{
    // Manual Setting
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountId, accountPassword); // ID + Password
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountPassword); // Password

    // Auto Setting  
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
Transfers the account with TransferAccount issued with **issueTransfer** API.
When account transfer is successful, a transfer completion message will be displayed from the device where TransferAccount has been issued and a new account will be created when a guest logs in.
On the device where the account transfer was successfully made, the guest account from the previous device where TransferAccount was issued can still be used.

> <font color="red">[Caution]</font><br/>
> If account transfer is made while logged in to the guest account, the guest account will be lost.
> * If incorrect id/password is attempted multiple times, an **AUTH_TRANSFERACCOUNT_BLOCK(3042)** error occurs and the account migration is blocked for a certain period of time.
> In this case, you can inform the user how long the account migration will be banned through the TransferAccountFailInfo value as shown below.

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

This is a 'pending withdrawal" feature.
By requesting a temporary withdrawal, the account is not immediately withdrawn. Instead, it is withdrawn after a specific grace period.
The grace period can be changed in the console.

> 'Caution'
>
> Do not use **Gamebase.Withdraw()** API if you're using the Pending Withdrawal feature.
> The **Gamebase.Withdraw()** API immediately withdraws accounts when used.

If login is successful, AuthToken.member.temporaryWithdrawal can be used to determine if the user is in the status of pending withdrawal.

### Request TemporaryWithdrawal

Requests a temporary withdrawal.
The account is automatically withdrawn after a specific grace period set in the console.

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

For games using the Pending Withdrawal feature must notify its users that they are in grace period if AuthToken.member.temporaryWithdrawal is not null after login.

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
                Debug.Log(string.Format("User is under temporary withdrawa. GracePeriodDate : {0}", error));
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

Cancels a withdrawal request.
If the grace period is over and the withdrawal process is completed, it cannot be undone.

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

Immediately withdraws the account, ignoring the grace period.
The internal mechanics are the same as the Gamebase.Withdraw() API.

Instant withdrawal cannot be undone, so it is important to ask the user several times if they really want to execute the command.

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

## Error Handling

| Category | Error                                    | Error Code | Description                                    |
| ---  | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth | INVALID_MEMBER | 6 | Request for invalid member. |
|      | BANNED_MEMBER | 7 | Named member has been banned. |
|      | AUTH_USER_CANCELED | 3001 | Login is cancelled. |
|      | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | The authentication is not supported. |
|      | AUTH_NOT_EXIST_MEMBER | 3003 | Named member does not exist or has withdrawn. |
|  | AUTH_EXTERNAL_LIBRARY_INITIALIZATION_ERROR | 3006 | Failed to initialize the external authentication library. |
|      | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | Error in external authentication library. <br/>Check DetailCode and DetailMessage. |
|  | AUTH_ALREADY_IN_PROGRESS_ERROR | 3010 | Previous authentication process is not complete.
| TransferKey | SAME\_REQUESTOR | 8 | The issued TransferKey has been used on the same device. |
|             | NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | You have tried transferring with a non-guest account or the account is linked with a non-guest IdP. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | The date of TransferAccount has expired. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | You have entered a wrong TransferAccount several times, so the account transfer function has been locked. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | Invalid TransferAccount ID. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | Invalid TransferAccount Password. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount has not been set. <br/> Please set it on the NHN Cloud Gamebase console first. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount does not exist. Please issue TransferAccount first. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount exists. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount has already been used. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | Token login has failed. |
|              | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | Invalid token information |
|              | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | Invalid last login IDP information |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IDP login has failed. |
|           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | Invalid IDP information (IDP information does not exist in the Console.) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | Add mapping has failed. |
|             | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | Already mapped to another member. |
|             | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | Already mapped to same IDP. |
|             | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | Invalid IDP information (IDP information does not exist in the Console.) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | AddMapping is not available with Guest IdP. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | The force mapping key (ForcingMappingKey) has not been found.<br/>Please check ForcingMappingTicket again. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | The force mapping key (ForcingMappingKey) has already been used. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | The date of force mapping key (ForcingMappingKey) has expired. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | The force mapping key (ForcingMappingKey) has been used for a different IdP.<br/>The issued ForcingMappingKey is used for trying to force map to the same IdP. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | The force mapping key (ForcingMappingKey) has been used for a different account.<br/>The issued ForcingMappingKey is used for trying to force map to the same IdP and account. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | Remove mapping has failed. |
|                | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | Cannot delete last mapped IDP. |
|                | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | Currently logged-in IDP |
| Logout | AUTH_LOGOUT_FAILED | 3501 | Logout has failed. |
| Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | Withdrawal has failed. |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already in the status of temporary withdrawal.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not in the status of temporary withdrawal.                     |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | Not playable (due to maintenance or service closed) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | Unknown error (Undefined error) |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* Occurs in NHN Cloud external authentication library.
* Check the error code as below:

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

* Check error codes of IdP SDK at each developer's page.
