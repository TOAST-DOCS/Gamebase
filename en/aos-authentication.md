## Game > Gamebase > Android Developer's Guide > Authentication

## Login

Gamebase supports Guest logins by default.

* To log in a Provider other than Guest, a matching Provider AuthAdapter is required.
* For setting of AuthAdapter and 3rd-Party Provider SDK, refer to
  [3rd-Party Provider SDK Guide](./aos-started#3rd-party-provider-sdk-guide)


### Login Flow

In many games, login is implemented on a title page.
* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after it is installed.
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described above can be implemented in the following order:

![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and passwords.
* Call **Gamebase.loginForLastLoggedInProvider()**.

#### 1-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.getUserID()** to implement a game logic.

#### 1-2. When Authentication is Failed

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.loginForLastLoggedInProvider()** again or try again in a moment.
* Banned game user
    * If the error code is **AUTH_BANNED_MEMBER (3005)**, the authentication has failed because the game user is banned.
    * Check ban information with **Gamebase.getBanInfo ()** and notify the user with reasons for not being able to play.
    * When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * As authentication with latest login type has failed, follow **3. Authenticate with Specified IdP**.

#### 2. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type
    * Types that can be authenticated are declared in the **AuthProvider** class.
* Call **Gamebase.login(activity, idpType, callback)** API.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.getUserID()** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network error
    * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.login(activity, idpType, callback)** again or try again in a moment.
* Banned game user
    * If the error code is **AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
    * Check ban information with **Gamebase.getBanInfo()** and notify the user with reasons for not being able to play.
    * When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login as the Latest Login IdP

Try login with the most recently logged-in IdP. <br/>
If a token is expired or its authentication fails, return failure. <br/>
Note that a login should be implemented for the IdP.


**API**

```java
+ (void)Gamebase.loginForLastLoggedInProvider(Activity activity, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
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
                // Use Gamebase.getBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                BanInfo banInfo = Gamebase.getBanInfo();
            } else {
                // For other error cases, try to authenticate with a specified IDP.
                Gamebase.login(activity, provider, logincallback);
            }
        }
    }
}
```
### Login with GUEST

Gamebase supports Guest logins.

* Create an only key of device to try to log in Gamebase.
* As the account may be deleted, when an app is deleted or device is initialized, it is recommended to use IdP for a Guest login.

Refer to the below example to implement a Guest login.


**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider.GUEST, GamebaseDataCallback<AuthToken> callback);
```

**Example**

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
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a popup on banning to fit for Game UI
                    // Use Gamebase.getBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = Gamebase.getBanInfo();
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

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
```

**Example**

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
                    // Use Gamebase.getBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = Gamebase.getBanInfo();
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

| Keyname | Usage | Value Type |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP.<br/>Not applied for Google authentication. |                                          |
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

**API**

```java
+ (void)Gamebase.login(Activity activity, Map<String, Object> credentialInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

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
                    // Use Gamebase.getBanInfo() to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = Gamebase.getBanInfo();
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

[Console Guide](./oper-app/#authentication-information)

## Logout

Try to log out from logged-in IdP. In many cases, the logout button is located on the game configuration screen.
Even if a logout is successful, a game user's data remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next login process.<br/><br/>

Following shows an example logout code with a click of the log-out button.

**API**

```java
+ (void)Gamebase.logout(Activity activity, GamebaseCallback callback);
```

**Example**

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

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdrawal, a logout from IdP will be tried.

> <font color="red">[Caution]</font><br/>
>
> When a user has many interfaced IdPs, all IdP interfaces will be removed and user data of Gamebase will be deleted.
>

**API**

```java
+ (void)Gamebase.withdraw(Activity activity, GamebaseCallback callback);
```

**Example**

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

In many games, one account may have many integrated (mapped) IdPs.<br/>By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.

In short, a login to a mapped IdP account will be made available with a same user ID at all times.<br/><br/>

Note, however, that each IdP can have only one account to map.<br/>
For instance, if a Google account is mapped, no other Google account can be additionally mapped.<br/>
Below shows an example.<br/><br/>

* Gamebase User ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * Apple Game Center ID: cc
    * Payco ID: dd
* Gamebase User ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

### Add Mapping Flow

Implement mapping in the following order:

#### 1. Login
Mapping means to add an IdP account integration to a current account, so login is a prerequisite.
First, call a login API and log in.

#### 2. Mapping

Call **Gamebase.addMapping(activity, idpType, callback)** to try mapping.

#### 2-1. When mapping is successful

* Congratulations! Successfully added an IdP account integrated with the current account.
* Even if a mapping is successful, "currently logged-in IdP" will not change. For example, after a user's login with Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
* Mapping simply adds IdP integration.

#### 2-2. When mapping is failed

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.addMapping()** again or try again in a moment.
* Error of integration to another account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER (3302)**, the IdP account to map has been already integrated to another account.To remove the integrated account, log in the account and call **Gamebase.withdraw()** to withdraw, or call **Gamebase.removeMapping()** to remove integration and try mapping again.
* Error of integration to a same IdP account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP (3303)**, a same type of account to the IdP has already been integrated.
        * Gamebase mapping allows only one account of integration to an IdP. For example, if your account is already integrated to a PAYCO account, no other PAYCO account can be added.
        * To integrate another account of a same IdP, call **Gamebase.removeMapping()** to remove integration and try mapping again.
* Other Errors
    * Mapping has failed.

### Add Mapping

Try mapping to another IdP while logged-in to a specific IdP.<br/>

Below is an example of mapping to Facebook.

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Add Mapping successful
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // Add Mapping failed.
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
                // IdP account for mapping has already been integrated to another account.
                // To force the account to unlink, delete or unmap it. Or, as shown below,
                // get ForcingMappingTicket and then try force mapping using the addMappingForcibly() method.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // For more details, see the addMappingForcibly API document.
                    }
                }
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
    });
}
```

### Add Mapping with Credential

This interface can be used for Gamebase AddMapping by an access token issued by the game client directly from the IdP.

* How to Set Credential Parameters

| Keyname                                  | Usage                                    | Value Type                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP.<br/>Not applied for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |                                          |

> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK&#39;s API, which Gamebase does not support.
>

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, Map<String, Object> credentialInfo, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

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
                return;
            }

            // Add Mapping failed.
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
                // IdP account for mapping has already been integrated to another account.
                // To force the account to unlink, delete or unmap it. Or, as shown below,
                // get ForcingMappingTicket and then try force mapping using the addMappingForcibly() method.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // For more details, see the addMappingForcibly API document.
                    }
                }
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
    });
}
```

### Add Mapping Forcibly
If there is any account mapped to a specific IdP, try **force** mapping.
When you try **force mapping**, you need `ForcingMappingTicket` obtained from the AddMapping API.

The following is an example of force mapping to Facebook:

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Successfully added mapping
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // First, call the addMapping API to try mapping to an already linked account and get ForcingMappingTicket as follows.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // Use the from() method of the ForcingMappingTicket class to obtain the ForcingMappingTicket instance.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // Try force mapping.
                Gamebase.addMappingForcibly(activity, AuthProvider.FACEBOOK, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // Successfully added force mapping
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // Failed to add force mapping
                        // Check the error code and resolve the error.
                    }
                }
            } else {
                ...
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

| keyname                                  | a use                                    | Value type                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdP type setting                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set the authentication information (access token) received after IdP login.<br/>It is not used for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter the OTOC (one-time authorization code) which can be obtained after Google login. |                                          |

> [Note]
>
> This may be needed to use unique functions of external services (e.g. Facebook).
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Developments requested by an external SDK must be implement using APIs from the external SDK; Gamebase does not support those development features.
>

The following is an example of force mapping to Facebook:

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, Map<String, Object> credentialInfo, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    final Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Successfully added mapping
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // First, call the addMapping API to try mapping to an already linked account and get ForcingMappingTicket as follows.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // Use the from() method of the ForcingMappingTicket class to obtain the ForcingMappingTicket instance.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // Try force mapping.
                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // Successfully added force mapping
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // Failed to add force mapping
                        // Check the error code and resolve the error.
                    }
                }
            } else {
                ...
            }
        }
    });
}
```



### Remove Mapping

Unlink a specific IdP. Returns fail when trying to disconnect an account currently logged in.<br/>
After mapping is removed, Gamebase processes logout of the IdP.

**API**

```java
+ (void)Gamebase.removeMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

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


## Gamebase User`s Information
Process authentication with Gamebase, in order to get information required to create an app.

> <font color="red">[Caution]</font><br/>
>
> Cannot obtain authentication information when you&#39;re logged in with "Gamebase.loginForLastLoggedInProvider()" API.
>
> To obtain authentication information, log in with "Gamebase.login (activity, IDP_CODE, callback)" API with {IDP_CODE} parameter, which is same as IDPCode to use, instead of "Gamebase.loginForLastLoggedInProvider()".

### Get Authentication Information for Gamebase
Get authentication information issued by Gamebase.

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
+ (BanInfo)Gamebase.getBanInfo();
```

**Example**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();

// Obtaining Ban Information
BanInfo banInfo = Gamebase.getBanInfo();
```


### Get Authentication Information for External IdP

Get access token, User ID, and profiles from externally authenticated SDK.

**API**

```java
+ (String)Gamebase.getAuthProviderUserID(String providerName);
+ (String)Gamebase.getAuthProviderAccessToken(String providerName);
+ (AuthProviderProfile)Gamebase.getAuthProviderProfile(String providerName);
```

**Example**

```java
String userId = Gamebase.getAuthProviderUserID(AuthProvider.FACEBOOK);

// Get AccessToken.
String accessToken = Gamebase.getAuthProviderAccessToken(AuthProvider.FACEBOOK);

// Get User Profile.
AuthProviderProfile profile = Gamebase.getAuthProviderProfile(AuthProvider.FACEBOOK);
Map<String, Object> profileMap = profile.information;
```

### Get Banned User Information

For users who are registered as banned in the Gamebase Console,
information codes of restricted use will be displayed as below, when they try to log in. The ban information can be found by using the **Gamebase.getBanInfo()** method.

* BANNED_MEMBER(7)

## TransferAccount
Issues a key to transfer the guest account to another device.

This key is called **TransferAccountInfo**.
The issued TransferAccountInfo calls the **requestTransferAccount** API from another device to transfer the account.

> `Caution`
> The TransferAccountInfo key can be issued while the guest account is logged in.
> Transfer of guest account using TransferAccountInfo is allowed only when logged in to a guest account or not logged in.
> If the logged-in guest account has already been mapped to an IdP ((Google, Facebook, PAYCO, etc.)) account, account transfer is not supported.

### Issue TransferAccount
Issues TransferAccountInfo to transfer the guest account.

**API**

```java
+ (void)Gamebase.issueTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
Gamebase.issueTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Issuing TransferAccount failed.
            return;
        }

        // Issuing TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```

### Query TransferAccount
Queries the TransferAccountInfo information issued for guest account transfer to the Gamebase server.

**API**

```java
+ (void)Gamebase.queryTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
Gamebase.queryTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Querying TransferAccount failed.
            return;
        }

        // Querying TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```


### Renew TransferAccount
Renews the issued TransferAccountInfo information.
There are two types of renewal: **Auto Renew** and **Manual Renew**. You can select either **Renew Password Only** or **Renew Both ID and Password** to renew the TransferAccountInfo information.

```java
+ (void)Gamebase.renewTransferAccount(final TransferAccountRenewConfiguration config, final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
// If you want renew the account automatically, use this config.
final RenewalTargetType renewalTargetType = RenewalTargetType.ID_PASSWORD; // RenewalTargetType.PASSWORD
final TransferAccountRenewConfiguration autoConfig = TransferAccountRenewConfiguration.newAutoRenewConfiguration(renewalTargetType);

// If you want renew the account manually, use this config.
final TransferAccountRenewConfiguration manualConfig = TransferAccountRenewConfiguration.newManualRenewConfiguration("id", "password");
Gamebase.renewTransferAccount(autoConfig, new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccountInfo, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Renewing TransferAccount failed.
            return;
        }

        // Renewing TransferAccount success.
        final String renewedAccount = transferAccount.account.id;
        final String renewedPassword = transferAccount.account.password;
    }
});
```




### Transfer Guest Account to Another Device
Transfers the account with TransferAccount issued with **issueTransfer** API.
When account transfer is successful, a transfer completion message will be displayed from the device where TransferAccount has been issued and a new account will be created when a guest logs in.
On the device where the account transfer was successfully made, the guest account from the previous device where TransferAccount was issued can still be used.

> `Caution`
> If account transfer is executed while logged in as a guest, the guest account logged in on the device will be lost.

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(final AuthToken authToken, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            return;
        }
        // Transfering Account success.
        // TODO: implements post login process
    }
});
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | Request for invalid member.                        |
|                | BANNED\_MEMBER                           | 7          | Named member has been banned.                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | Login is cancelled.                          |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication is not supported.                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | Named member does not exist or has withdrawn.                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | Error in external authentication library. <br/>Check DetailCode and DetailMessage. |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR           | 3010       | The previous authentication process has not been completed. |
| TransferAccount| SAME\_REQUESTOR                          | 8          | The issued TransferAccount has been used on the same device. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | You have tried transferring with a non-guest account or the account is linked with a non-guest IdP. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | The date of TransferAccount has expired. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | You have entered a wrong TransferAccount several times, so the account transfer function has been locked. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | Invalid TransferAccount ID. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | Invalid TransferAccount Password. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount has not been set. <br/> Please set it on the TOAST Gamebase console first. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | No TransferAccount found. Please issue TransferAccount first. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount already exists. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount has already been used. |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | Token login has failed.                          |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                        |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IdP information.                   |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP login has failed.                         |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | Invalid IdP information (IdP information does not exist in the Console.) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | Add mapping has failed.                           |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.                      |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IDP.                     |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | Invalid IdP information (IdP information does not exist in the Console.) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | AddMapping is not available with a guest IdP. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | No force mapping key (ForcingMappingKey) found. <br/>Please check ForcingMappingTicket again. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | The force mapping key (ForcingMappingKey) has already been used. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | The date of force mapping key (ForcingMappingKey) has expired. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | The force mapping key (ForcingMappingKey) has been used for a different IdP. <br/>The issued ForcingMappingKey is used for trying to force map to the same IdP. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | The force mapping key (ForcingMappingKey) has been used for a different account. <br/>The issued ForcingMappingKey is used for trying to force map to the same IdP and account. |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | Remove mapping has failed.                           |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IdP.                |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | Currently logged-in IdP.                      |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | Logout has failed.                            |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | Withdrawal has failed.                              |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | Not playable (due to maintenance or service closed)         |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | Unknown error (Undefined error)             |

* Refer to the following document for the entire error codes
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* Occurs in SDK of each IdP.
* Check the error code as below.

```java
Gamebase.login(activity, AuthProvider.GOOGLE, additionalInfo, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Login successful");
            ...
        } else {
            Log.e(TAG, "Login failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();

            if (errorCode == GamebaseError.AUTH_EXTERNAL_LIBRARY_ERROR) {
                // Third Party Detail Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* Check error codes of IdP SDK at each developer's page.

`Last Update: 2019.05.28`