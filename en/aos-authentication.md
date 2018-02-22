## Game > Gamebase > Android Developer's Guide > Authentication

## Login

Gamebase supports Guest logins by default.

* To log in a Provider other than Guest, a matching Provider AuthAdapter is required.
* For setting of AuthAdapter and 3rd-Party Provider SDK, refer to [3rd-Party Provider SDK Guide](./aos-started#3rd-party-provider-sdk-guide)




### Login Flow
In many games, login is implemented on a title page. Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after it is installed. After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described above can be implemented in the following order:


#### 1. Get Latest Login Type

- Call **Gamebase.getLastLoggedInProvider()**.
- If there is a returned value, follow **2. Authenticate with Lates#### **2-2. When Authentication is Failed**

- Network error
  - If the error code is **SOCKET\_ERROR (110)** or **SOCKET\_RESPONSE\_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.loginForLastLoggedInProvider()** again or try again in a moment.
- Banned game user
  - If the error code is **AUTH\_BANNED\_MEMBER (3005)**, the authentication has failed because the game user is banned.
  - Check ban information with **Gamebase.getAuthBanInfo ()** and notify the user with reasons for not being able to play.
  - When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
- Other errors
  - As authentication with latest login type has failed, follow **3. Authenticate with Specified IdP**.
t Login Type**.
- If there is no returned value, let the game user decided IdP and follow **3. Authenticate with Specified IdP**.



#### 2. Authenticate with Latest Login Type
- If a previous authentication has been recorded, try to authenticate with no need of ID and passwords.
- Call **Gamebase.loginForLastLoggedInProvider()**.

#### 2-1. When Authentication is Successful

- Congratulations! Successfully authenticated.
- Get a user ID with **Gamebase.getUserID()** to implement a game logic.


#### 2-2. When Authentication is Failed

- Network error
    - If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.loginForLastLoggedInProvider()** again or try again in a moment.
- Banned game user
    - If the error code is **AUTH_BANNED_MEMBER (3005)**, the authentication has failed because the game user is banned.
    - Check ban information with **Gamebase.getAuthBanInfo ()** and notify the user with reasons for not being able to play.
    - When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
- Other errors
    - As authentication with latest login type has failed, follow **3. Authenticate with Specified IdP**.






#### 3. Authenticate with Specified IdP

- Try to authenticate by specifying an IdP type
    - Types that can be authenticated are declared in the **AuthProvider** class.
- Call **Gamebase.login(activity, idpType, callback)** API.

#### 3-1. When Authentication is Successful
- Congratulations! Successfully authenticated.
- Get a user ID with **Gamebase.getUserID()** to implement a game logic.

#### 3-2. When Authentication is Failed
- Network error
    - If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.login(activity, idpType, callback)** again or try again in a moment.
- Banned game user
    - If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
    - Check ban information with **Gamebase.getAuthBanInfo()** and notify the user with reasons for not being able to play.
    - When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
- Other errors
    - Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.




### Login as the Latest Login IdP

Try login with the most recently logged-in IdP. <br/>
If a token is expired or its authentication fails, return failure. <br/>
Note that a login should be implemented for the IdP.

```java
private static void onLogin(final Activity activity) {
    if (!TextUtils.isEmpty(Gamebase.getLastLoggedInProvider())) {
        onLoginForLastLoggedInProvider(activity);
    } else {
        // If there is no last login format, try to authenticate with a specified IDP.
        Gamebase.login(activity, provider, logincallback);
    }
}

private static void onLoginForLastLoggedInProvider(final Activity activity) {
    Gamebase.loginForLastLoggedInProvider(activity, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Login successful
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForLastLoggedInProvider(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a pop-up on banning to fit for Game UI
                    // Use Gamebase.getAuthBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // For other error cases, try to authenticate with a specified IDP.
                    Gamebase.login(activity, provider, logincallback);
                }
            }
        }
    });
}
```


### Login with GUEST

Gamebase supports Guest logins.

- Create an only key of device to try to log in Gamebase.
-  As the account may be deleted, when an app is deleted or device is initialized, it is recommended to use IdP for a Guest login.

Refer to the below example to implement a Guest login.

```java
private static void onLoginForGuest(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GUEST, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Login successful
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGuest(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    //  The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a popup on banning to fit for Game UI
                    // Use Gamebase.getAuthBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // Login failed
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


### Login with IdP

Following is a login example with a specific IdP.<br/>
You can find the types of IdP that can login, with **AuthProvider** class.

```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Login successful
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGoogle(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a pop-up on banning to fit for Game UI
                    // Use Gamebase.getAuthBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // Login failed
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Login with Credential

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.

- How to Set Credential Parameters


| Keyname | Usage | Value Type |
| --- | --- | --- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type | AuthProvider.GOOGLE<br>AuthProvider.FACEBOOK<br> AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP. Not applied for Google authentication. |   |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |   |

> [Note]
> 
> May require when original functions of external services (such as Facebook) are in need within a game.
> 

<br/>

> <font color="red">[Caution]</font><br/>
> 
> Development items external SDK requires to support need to be implemented by using external SDK's API, which Gamebase does not support.
> 


```java
private static void onLoginWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.login(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Login successful
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a banning pop-up to fit for Game UI
                    // Use Gamebase.getAuthBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // Login failed
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```



### Authentication Additional Information Settings

#### Facebook

- Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information &Callback URL** to set json string-type information to **Additional Information**.
    - When trying OAuth authentication, it is required to set additional information to request to Facebook.

Example of adding authentication information in Facebook

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```


#### PAYCO
- Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information &Callback URL** to set json string-type information to **Additional Information**.
  - **service_code** and **service_name** should be set as PaycoSDK requires.

Example of adding authentication information in PAYCO

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

#### NAVER
- Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information &Callback URL** to set json string-type information to **Additional Information**.
	* **service_name** and **url_scheme_ios_only** should be set as NaverSDK requires. 

Example of adding authentication information in NAVER 

```json
{ "url_scheme_ios_only": "Your Url Scheme", "service_name": "Your Service Name" }
```

## Logout

Try to log out from logged-in IdP. In many cases, the logout button is located on the game configuration screen. Even if a logout is successful, a game user's data remain. When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next login process.<br/><br/>

Following shows an example logout code with a click of the log-out button.

```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// Logout successful
                Log.d(TAG, "Logout successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLogout(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // Logout failed
                    Log.e(TAG, "Logout failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

## Withdraw

Below shows an example of how a game user withdraws while logged-in.<br/><br/>

- When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
- The user can log in with the IdP again, and a new user's data will be created.
- It means user's withdrawal from Gamebase, not from IdP account.
- After a successful withdrawal, a logout from IdP will be tried.


> <font color="red">[Caution]</font><br/>
> 
> When a user has many interfaced IdPs, all IdP interfaces will be removed and user data of Gamebase will be deleted.
>

```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Withdrawal successful
                Log.d(TAG, "Withdraw successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onWithdraw(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // Withdrawal failed
                    Log.e(TAG, "Withdraw failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```




## Mapping

Mapping refers to connecting or disconnecting an existing login account to/from another IdP account.
In many games, one account may have many integrated (mapped) IdPs. <br/>
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account. <br/>
In short, a login to a mapped IdP account will be made available with a same user ID at all times.

Note, however, that each IdP can have only one account to map. <br/>
For instance, if a Google account is mapped, no other Google account can be additionally mapped. <br/>

Below shows an example. <br/>

- Gamebase User ID: 123bcabca
  - Google ID: aa
  - Facebook ID: bb
  - Apple Game Center ID: cc
  - Payco ID: dd
- Gamebase User ID : 456abcabc
  - Google ID: ee
  - Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.


### Add Mapping Flow

Implement mapping in the following order:

#### 1. Login
Mapping means to add an IdP account integration to a current account, so login is a prerequisite.

First, call a login API and log in.


#### 2. Mapping

Call **Gamebase.addMapping(activity, idpType, callback)** to try mapping.


#### 2-1. When mapping is successful

- Congratulations! Successfully added an IdP account integrated with the current account.
- Even if a mapping is successful, "currently logged-in IdP" will not change. For example, after a user's login with Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
- Mapping simply adds IdP integration.


#### 2-2. When mapping is failed
- Network error
    - If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.addMapping()** again or try again in a moment.
- Error of integration to another account
    - If the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER (3302)**, the IdP account to map has been already integrated to another account.To remove the integrated account, log in the account and call **Gamebase.withdraw()** to withdraw, or call **Gamebase.removeMapping()** to remove integration and try mapping again.
- Error of integration to a same IdP account
  - If the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP (3303)**, a same type of account to the IdP has already been integrated.
    - Gamebase mapping allows only one account of integration to an IdP. For example, if your account is already integrated to a PAYCO account, no other PAYCO account can be added.
    - To integrate another account of a same IdP, call **Gamebase.removeMapping()** to remove integration and try mapping again.
- Other Errors
- Mapping has failed.



### Add Mapping
Try mapping to another IdP while logged-in to a specific IdP.<br/>

Below is an example of mapping to Facebook.


```java
private static void addMappingForFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Add Mapping successful
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // IDP account for mapping has already been integrated to another account.
                    // To remove integration, withdraw from the account or remove mapping.
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // IdP account for mapping has already been added.
                    // Gamebase Mapping allows only one account of integration to an IdP.
                    // To change IdP account, remove mapping of the integrated account.
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // Add Mapping failed.
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Add Mapping with Credential

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.

- How to Set Credential Parameters

| Keyname | Usage | Value Type |
| --- | --- | --- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type | AuthProvider.GOOGLE<br>AuthProvider.FACEBOOK <br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP. Not applied for Google authentication. |   |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |   |




> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK&#39;s API, which Gamebase does not support.
>

```java
private static void addMappingWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Add Mapping successful
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // IDP account for mapping has already been integrated to another account.
                    // To remove integration, withdraw from the account or remove mapping.
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // IdP account for mapping has already been added.
                    // Gamebase Mapping allows only one account of integration to an IdP.
                    // To change IdP account, remove mapping of the integrated account.
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // Add Mapping failed.
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Remove Mapping

Remove mapping with a specific IdP. If IdP mapping is not removed, error will occur. <br/>
After mapping is removed, Gamebase processes logout of the IdP.

```java
private static void removeMappingForFacebook(final Activity activity) {
    Gamebase.removeMapping(activity, AuthProvider.FACEBOOK, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Remove Mapping successful
                Log.d(TAG, "Remove mapping successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error means network access is temporarily unavailable.
                    // Check network status or retry after a moment.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                removeMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_REMOVE_MAPPING_LOGGED_IN_IDP) {
                    // Cannot remove mapping with a login account.
                    // Log in with another account to remove mapping or withdraw.
                    Log.e(TAG, "Remove Mapping failed- LOGGED_IN_IDP");
                } else {
                    // Remove Mapping failed.
                    Log.e(TAG, "Remove mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


## Gamebase User's Information
Process authentication with Gamebase, in order to get information required to create an app.

> <font color="red">[Caution]</font><br/>
>
> Cannot obtain authentication information when you&#39;re logged in with "Gamebase.loginForLastLoggedInProvider()" API.
>
> To obtain authentication information, log in with "Gamebase.login (activity, IDP_CODE, callback)" API with {IDP_CODE} parameter, which is same as IDPCode to use, instead of "Gamebase.loginForLastLoggedInProvider()".


### Get Authentication Information for Gamebase
Get authentication information issued by Gamebase.

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();

// Obtaining Ban Information
AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
```


### Get Authentication Information for External IDP
Get access token, User ID, and profiles from externally authenticated SDK.


```java
// Get User ID.
String userId = getAuthProviderUserID(AuthProvider.FACEBOOK);

// Get AccessToken.
String accessToken = getAuthProviderAccessToken(AuthProvider.FACEBOOK);

// Get User Profile.
AuthFacebookProfile profile = (AuthFacebookProfile) getAuthProviderProfile(AuthProvider.FACEBOOK);
String name = profile.getName();    // or profile.information.get("name")
String email = profile.getEmail();  // or profile.information.get("email")
```

### Get Banned User Information

For users who are registered as banned in the Gamebase Console, information codes of restricted use will be displayed as below, when they try to log in. The ban information can be found by using the **Gamebase.getAuthBanInfo()** method.


* AUTH_BANNED_MEMBER(3005)

## Error Handling

| Category       | Error                                    | Error Code | Description                               |
| -------------- | ---------------------------------------- | ---------- | ----------------------------------------- |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | Login is cancelled.                       |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication is not supported.      |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | Named member does not exist or has withdrawn.   |
|                | AUTH\_INVALID\_MEMBER                    | 3004       | Request for invalid member.               |
|                | AUTH\_BANNED\_MEMBER                     | 3005       | Named member has been banned.             |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | Error in external authentication library. |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | Token login has failed.                   |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                    |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IDP information. |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IDP login has failed.                         |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | Invalid IDP information (IDP information does not exist in the Console.) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | Add mapping has failed.                   |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.  |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IDP.            |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | Invalid IDP information (IDP information does not exist in the Console.) |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | Remove mapping has failed.                |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IDP.            |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | Currently logged-in IDP.                  |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | Logout has failed.                        |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | Withdrawal has failed.                    |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | Not playable (due to maintenance or service closed)  |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | Unknown error (Undefined error)           |


- Refer to the following document for the entire error codes
    - [Entire Error Codes](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

- Occurs in TOAST Cloud external authentication library.

