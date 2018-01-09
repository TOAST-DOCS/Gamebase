## Game > Gamebase > Android Developer's Guide  > Authentication

## Login

* Gamebase supports guest logins by default. 
* To log into providers other than guest, a matching Provider AuthAdapter is required.
* For setting of AuthAdapter and 3rd-Party Provider SDK, refer to [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

Many games require that a login should be implemented on a title screen. 
  * Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed.
  * After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP. 

The logic described above can be implemented in the following order:

#### 1. Get Latest Login Type 
* Call **Gamebase.getLastLoggedInProvider()**.
* If there is a returned value, follow **2. Authenticate with Latest Login Type**.
* If there is no returned value, let the game user decided IdP and follow **3. Authenticate with Specified IdP**.

#### 2. Authenticate with Latest Login Type 

* If a previous authentication has been recorded, try to authenticate with no need of ID and passwords.
* Call **Gamebase.loginForLastLoggedInProvider()**.

#### 2-1. When authentication is successful 

* Congratulations! Successfully authenticated. 
* Get a user ID with **Gamebase.getUserID()** to play games.

#### 2-2. When authentication is failed 

* Network Error 
  * If the error code is **SOCKET_ERROR(110)**  or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.loginForLastLoggedInProvider()** again or try again in a minute.
* Banned Game User
  * If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user. 
  * Check ban information with **Gamebase.getAuthBanInfo()** and notify the user with reasons for not being able to play. 
  * When **GamebaseConfiguration.Builder.enablePopup(true)**  and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other Errors 
  * Authenticate with Latest Login Type has failed. Follow **3. Authenticate with Specified IdP**.

#### 3.  Authenticate with Specified IdP 

* Try to authenticate by specifying an IdP type 
  * Types that can be authenticated are declared in the **AuthProvider** class. 
* Call **Gamebase.login(activity, idpType, callback)** API.

#### 3-1. When authentication is successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.getUserID()** to play games. 

#### 3-2. When authentication is failed 

* Network Error 
  - If the error code is **SOCKET_ERROR(110)**  or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call  **Gamebase.login(activity, idpType, callback)** again or try again in a minute.
* Banned Game User
  - If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user. 
  - Check ban information with **Gamebase.getAuthBanInfo()** and notify the user with reasons for not being able to play. 
  - When **GamebaseConfiguration.Builder.enablePopup(true)**  and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a ban pop-up.
* Other Errors 
  * Notify that en error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login ***with*** Latest Login IdP

Try login with the most recently logged-in IdP (Identity Provider). <br/>
If a token is expired or its authentication fails, return failure. <br/>
Note that a login should be implemented for the IdP.

```java
private static void onLogin(final Activity activity) {
    if (!TextUtils.isEmpty(Gamebase.getLastLoggedInProvider())) {
        onLoginForLastLoggedInProvider(activity);
    } else {
        // If there is no last logged-in format, try to authenticate with specified IDP.
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
                    // Gamebase automatically displays a ban pop-up. 
                    //
                    // In order to implement a ban pop-up to fit for Game UI 
                    // Use Gamebase.getAuthBanInfo() to find any ban information and notify the user with reasons for not being able to play game.  
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // For other error cases, try to authenticate with specified IDP. 
                    Gamebase.login(activity, provider, logincallback);
                }
            }
        }
    });
}
```
### Login with GUEST

Gamebase supports guest logins. 

* Create an only key of device to try to log in Gamebase. 
* For guest logins, as accounts may be deleted when an app is deleted or device is initialized, it is recommended to use IdP.

Refer to the below example to implement a guest login.

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
                    // Gamebase automatically displays a ban pop-up. 
                    //
                    // In order to implement a ban popup to fit for Game UI
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


### Login with Specific IdP

Following is a login example with a specific IdP.<br/>
You can find the types of IdP that can log in in the **AuthProvider** class. 

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
                    // Gamebase automatically displays a ban pop-up.
                    //
                    // In order to implement a ban pop-up to fit for Game UI 
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

* How to Set Credential Parameters 

| Keyname                                  | a use                                    | Value Type                               |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                             | AuthProvider.GOOGLE, AuthProvider.FACEBOOK, AuthProvider.PAYCO |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP <br/>Not applied for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |                                          |

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
                    // Gamebase automatically displays a ban pop-up.
                    //
                    // In order to implement a ban pop-up to fit for Game UI 
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

### Set Additional Authentication Information

#### Facebook

- Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**. 
  - When trying OAuth authentication, type of information to request to Facebook should be set. 

Example of how to add authentication information in Facebook 

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### PAYCO

- Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**. 
  - **service_code** and **service_name** should be set as PaycoSDK requires. 

Example of how to add authentication information in PAYCO

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

## Logout

Try to log out from logged-in IdP. In many cases, the log-out button is located on game configuration screen to click to implement. 
Even if a log-out is successful, data of game users remain. 
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords inputs will be required for the next log-in process. <br/><br/>

Following shows a log-out example code with a click of the log-out button. 

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

Below shows an example of how to withdraw game users while they're logged-in. <br/><br/>

* When a user is successfully withdrawn, the user's data interfaced with a login IdP are all deleted. 
* The user can log in with the IdP again, and a new user's data will be created. 
* It means user's withdrawal from Gamebase, not from IdP account. 
* After a successful withdraw, a log-out from IdP will be tried. 

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
            	// Withdraw successful 
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
                    // Withdraw failed 
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

Mapping refers to integrating or disintegrating existing login account to/from another IdP account. 

In many games, one account may have many integrated (mapped) IdPs.<br/>By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.

In short, a login to an integrated IdP account will be made available with a same user ID at all times. <br/><br/>

But keep in mind that each IdP can have only one account integrated. <br/>
For instance, if a Google account is integrated, no other Google account can be additionally integrated.<br/>
Below shows an example of account integration. <br/><br/>

* Gamebase User ID: 123bcabca
  * Google ID: aa
  * Facebook ID: bb
  * Apple Game Center ID: cc
  * Payco ID: dd
* Gamebase User ID : 456abcabc
  * Google ID: ee
  * Google ID: ff **-> As the Google ee account is integrated, another Google account cannot be integrated. **

Mapping API includes Add Mapping API and Remove Mapping API. 

### Add Mapping Flow

Implement mapping in the following order: 

#### 1. Log in 
Mapping means to add an IdP account integration to a current account, so login is a prerequisite.  

First, call a login API and log in. 

#### 2. Mapping 

Call **Gamebase.addMapping(activity, idpType, callback)** to try mapping. 

#### 2-1. When mapping is successful 

* Congratulations! Successfully added an IdP account integrated with the current account.  
* Even if a mapping is successful, 'currently logged-in IdP' does not change. For example, after a user logs in a Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account. 
* Mapping simply adds IdP integration. 

#### 2-2. When mapping is failed 

* Network Error 
  * If the error code is **SOCKET_ERROR(110)**  or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call  **Gamebase.addMapping()** again or try again in a minute. 
* Error of Integration to Another Account 
  * If the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, the IdP account to map has been already integrated to another account. 
    To remove the integrated account, log in the account and call **Gamebase.withdraw()** to withdraw, or call **Gamebase.removeMapping()** to remove integration and try mapping again. 
* Error of Integration to Same IdP Account 
  * If the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, a same type of account to the IdP has already been integrated. 
    * Gamebase mapping allows only one account of integration to an IdP. For example, if your account is already integrated to a PAYCO account, no other PAYCO account can be added.  
    * To integrate another account of a same IdP, call **Gamebase.removeMapping()** to remove integration and try mapping again. 


  * Mapping has failed. 

### Add Mapping

Try mapping to another IdP while logged-in to a specific IdP. <br/>

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

* How to Set Credential Parameters 

| Keyname                                  | a use                                    | Value Type                               |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                             | AuthProvider.GOOGLE, AuthProvider.FACEBOOK, AuthProvider.PAYCO |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP <br/>Not applied for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |                                          |

>  [Note]
> 
>  May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>

>  <font color="red">[Caution]</font><br/>
> 
>  Development items external SDK requires to support need to be implemented by using external SDK's API, which Gamebase does not support. 
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

Remove mapping with a specific IdP. If an IdP to remove mapping is an only IdP, return failure. After mapping is removed, log out the IdP in Gamebase. 

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
                    // Cannot remove mapping with a logged-in account.  
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


## Gamebase User`s Information
Process authentication with Gamebase, in order to get information required to create an app. 


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

For a banned user registered at Gamebase Console, restricted use of information code can be displayed as below, when trying login. The ban information can be found by using the **Gamebase.getAuthBanInfo()** method. 

* AUTH_BANNED_MEMBER(3005)

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | Login has been canceled.                 |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication method is not supported. |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | The member doe not exist or has withdrawn. |
|                | AUTH\_INVALID\_MEMBER                    | 3004       | Request for an invalid member            |
|                | AUTH\_BANNED\_MEMBER                     | 3005       | The member has been banned.              |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | Error in external authentication library. |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | Token login has failed.                  |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Token information is invalid.            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Latest login IdP information is invalid. |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP login has failed.                    |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | IdP information is invalid. (The IdP information is unavailable in console.) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | Add mapping has failed.                  |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.        |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to the same IdP.          |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | IdP information is invalid. (The IdP information is unavailable in console.) |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | Remove mapping has failed.               |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot remove the last mapped IdP.       |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | The IdP is currently logged-in.          |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | Logout has failed.                       |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | Withdraw has failed.                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | Cannot play (due to maintenance or service terminated) |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | The error is unknown (undefined).        |

* Refer to the following document for the entire error codes
  - [Entire Error Codes](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* This error occurred in TOAST Cloud external authentication library. 
