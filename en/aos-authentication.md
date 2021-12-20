## Game > Gamebase > Android Developer's Guide > Authentication

## Login

Gamebase supports Guest logins by default.

* To log in a Provider other than Guest, a matching Provider AuthAdapter is required.
* For setting of AuthAdapter and 3rd-Party Provider SDK, refer to
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle](./aos-started/#gradle)
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Console > 3rd-Party Provider SDK Guide](./aos-started/#console)

### Login Flow

In many games, login is implemented on a title page.

* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after it is installed.
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described above can be implemented in the following order:

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and passwords.
* Call **Gamebase.loginForLastLoggedInProvider()**.

#### 1-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.getUserID()** to implement a game logic.

#### 1-2. When Authentication Fails

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.loginForLastLoggedInProvider()** again or try again in a moment.
* Banned game user
    * If the error code is **BANNED_MEMBER(7)**, the authentication has failed because the game user is banned.
    * Check ban information with **BanInfo.from(exception)** and notify the user with reasons for not being able to play.
    * When **GamebaseConfiguration.Builder.enablePopup(true)** and **enableBanPopup(true)** are called during Gamebase initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * As authentication with latest login type has failed, follow **'2. Authenticate with Specified IdP'**.

#### 2. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type
    * Types that can be authenticated are declared in the **AuthProvider** class.
* Call **Gamebase.login(activity, idpType, callback)** API.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **Gamebase.getUserID()** to implement a game logic.

#### 2-2. When Authentication Fails

* Network error
    * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **Gamebase.login(activity, idpType, callback)** again or try again in a moment.
* Banned game user
    * If the error code is **BANNED_MEMBER(7)**, the authentication has failed due to banned game user.
    * Check ban information with **BanInfo.from(exception)** and notify the user with reasons for not being able to play.
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
            } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                // The user who try to log in has been banned.
                // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                // Gamebase automatically displays a pop-up on banning.
                //
                // In order to implement a pop-up on banning to fit for Game UI
                // Use BanInfo.from(exception) to find any ban information and notify the user with reasons for not being able to play game.
                BanInfo banInfo = BanInfo.from(exception);
            } else {
                // For other error cases, try to authenticate with a specified IdP.
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
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a popup on banning to fit for Game UI
                    // Use BanInfo.from(exception) to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = BanInfo.from(exception);
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

> <font color="red">[Caution]</font><br/>
>
> Even though PAYCO IdP is a certified module, it is sometimes rejected by iOS as it is falsely detected as an external transaction
> and we are no longer providing the constant of the AuthProvider.PAYCO.,
> the string called "payco" should be directly passed to the parameter.

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.login(Activity activity, AuthProvider provider, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
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
                    // Use BanInfo.from(exception) to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = BanInfo.from(exception);
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
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>AuthProvider.HANGAME<br>AuthProvider.APPLEID<br>AuthProvider.WEIBO<br>AuthProvider.KAKAOGAME<br>"payco" |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP.<br/>Not applied for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter One Time Authorization (OTAC) which can be obtained after Google login. |                                          |
| AuthProviderCredentialConstants.GAMEBASE_ACCESS_TOKEN | Used when logging in with Gamebase Access Token instead of IdP authentication information |  |
| AuthProviderCredentialConstants.IGNORE_ALREADY_LOGGED_IN | While logged in to Gamebase, allow login attempts with other account without logging out | **boolean** |

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
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // The user who try to log in has been banned.
                    // If GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) has been called
                    // Gamebase automatically displays a pop-up on banning.
                    //
                    // In order to implement a banning pop-up to fit for Game UI
                    // Use BanInfo.from(exception) to find any ban information and notify the user with reasons for not being able to play game.
                    BanInfo banInfo = BanInfo.from(exception);
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

Attempts account withdrawal while logged in.

* Upon successful withdrawal
  * The game user’s data of the IdP logged in will be deleted.
  * You can login again with the IdP. A new game user’s data will be created.
  * All linked IdPs will be logged out.
* It means user's withdrawal from Gamebase, not from IdP account.

> <font color="red">[Caution]</font><br/>
>
> If multiple IdPs are linked, all IdP linkages will be unlinked and the user data in Gamebase will be deleted.
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
    * AppleID ID: cc
    * Twitter ID: dd
* Gamebase User ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

> <font color="red">[Caution]</font><br/>
>
> When mapping is successfully done during guest login, guest IdP disappears. 
>

### Add Mapping Flow

Implement mapping in the following order:

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. Login

Mapping means to add an IdP account integration to a current account, so login is a prerequisite.
First, call a login API and log in.

#### 2. Mapping

Call **Gamebase.addMapping(activity, idpType, callback)** to try mapping.

#### 2-1. When mapping is successful

* Congratulations! Successfully added an IdP account integrated with the current account.
* Even if a mapping is successful, "currently logged-in IdP" will not change. For example, after a user's login with Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
    * <font color="red">[Caution]</font> : The Guest account is an exception. If the mapping attempt performed while logged in with the Guest account is successful, the Guest IdP is **deleted** and the 'currently logged-in IdP' is also changed to the mapped IdP.
* Mapping simply adds IdP integration.

#### 2-2. When mapping fails

* Network error
    * If the error code is **SOCKET_ERROR (110)** or **SOCKET_RESPONSE_TIMEOUT (101)**, it means that the authentication failed due to temporary network issues, so call **Gamebase.addMapping()** again or try again later.
* Error that occurs when already linked to another account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER (3302)**, it means that the account of the IdP to map to is already linked to another account. Using the **ForcingMappingTicket** obtained at this point, you can try force mapping (**Gamebase.AddMappingForcibly()**) or changing the login account (**Gamebase.ChangeLogin()**).
* Error that occurs when already linked to the same IdP account
    * If the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP (3303)**, it means that an account of the same type as the IdP you want to map to is already linked.
        * Gamebase mapping allows linking of only one account per IdP. For example, if an account is already linked to a PAYCO account, no other PAYCO account can be added.
        * To link another account of the same IdP, call **Gamebase.removeMapping()** to remove the linking and try mapping again.
* Other errors
    * The mapping attempt has failed.

### Add Mapping

Try mapping to another IdP while logged-in to a specific IdP.<br/>

Below is an example of mapping to Facebook.

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, String providerName, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMapping(Activity activity, String providerName, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
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
                final ForcingMappingTicket forcingMappingTicket = ForcingMappingTicket.from(exception);
                Gamebase.addMappingForcibly(activity, forcingMappingTicket, new GamebaseDataCallback<AuthToken>() {
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
| AuthProviderCredentialConstants.PROVIDER_NAME | Set IdP type                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>AuthProvider.APPLEID<br>AuthProvider.WEIBO<br>AuthProvider.KAKAOGAME<br>"payco" |
| AuthProviderCredentialConstants.ACCESS_TOKEN | Set authentication information (access token) received after login IdP.<br/>Not applied for Google authentication. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Enter one time authorization code (OTAC) which can be obtained after Google login. |                                          |

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
                final ForcingMappingTicket forcingMappingTicket = ForcingMappingTicket.from(exception);
                Gamebase.addMappingForcibly(activity, forcingMappingTicket, new GamebaseDataCallback<AuthToken>() {
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
+ (void)Gamebase.addMappingForcibly(Activity activity, ForcingMappingTicket forcingMappingTicket, GamebaseDataCallback<AuthToken> callback);

// Legacy API
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMappingForcibly(Activity activity, Map<String, Object> credentialInfo, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
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
                final ForcingMappingTicket forcingMappingTicket = ForcingMappingTicket.from(exception);

                // Try force mapping.
                Gamebase.addMappingForcibly(activity, forcingMappingTicket, new GamebaseDataCallback<AuthToken>() {
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

### Change Login with ForcingMappingTicket

When there is an account already mapped to a specific IdP, log out from the current account and log in with the account already mapped to the IdP.
At this time, you need the `ForcingMappingTicket` obtained from the AddMapping API.

If the Change Login API call fails, the Gamebase login status remains as the existing UserID.

The following example shows a situation where, after attempting to map to Facebook, an account already mapped to Facebook is found and the login is changed to the account.

**API**

```java
+ (void)Gamebase.changeLogin(Activity activity, ForcingMappingTicket forcingMappingTicket, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void changeLoginFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
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
                final ForcingMappingTicket forcingMappingTicket = ForcingMappingTicket.from(exception);

                // Log in with the UserID of ForcingMappingTicket
                Gamebase.changeLogin(activity, forcingMappingTicket, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException changeLoginException) {
                        if (Gamebase.isSuccess(changeLoginException)) {
                            // Successfully changed login
                            Log.d(TAG, "Change Login successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }

                        // Failed to change login
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

### Get Authentication Information for Gamebase
Get authentication information issued by Gamebase.

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
```

**Example**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();
```

### Get Authentication Information for External IdP

* Information such as access token, user ID, and profile of an external authentication IdP can be gotten by calling Gamebase Server API after login.
    * [Game > Gamebase > API Guide > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[Caution]</font><br/>
>
> * For security reasons, it is recommended to call the authentication information of an external IdP from the game server.
> * Access token may expire relatively sooner depending on the IdP.
>     * For example, the access token of Google will expire within 2 hours from the time of login.
>     * If you need user information, it is recommended to call Gamebase Server API immediately after login.
> * When logged in with the "Gamebase.loginForLastLoggedInProvider()" API, the authentication info cannot be retrieved.
>     * If you need the user info, instead of "Gamebase.loginForLastLoggedInProvider()", use the {IDP_CODE} identical to the IDPCode that you want to use as the parameter to log in as the "Gamebase.login(activity, IDP_CODE, callback)" API.

### Get Banned User Information

For users who are registered as banned in the Gamebase Console,
information codes of restricted use will be displayed as below, when they try to log in. The ban information can be found by using the **BanInfo.from(exception)** method.

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

> <font color="red">[Caution]</font><br/>
>
> * If account transfer is executed while logged in as a guest, the guest account logged in on the device will be lost.
> * When an invalid ID or password is entered consecutively, an error occurs like  **AUTH_TRANSFERACCOUNT_BLOCK(3042)** and the account transfer is blocked during a certain period. 
> In such case, user can be notified by TransferAccountFailInfo, of how long the blockage will sustain, like below: 

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken authToken, GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            TransferAccountFailInfo failInfo = TransferAccountFailInfo.from(exception);
            if (failInfo != null) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.
                String failedId = failInfo.id;
                int failCount = failInfo.failCount;
                Date blockedDate = new Date(failInfo.blockEndDate);
                return;
            }

            // Transfering Account failed by another reason.
            return;
        }

        // Transfering Account success.
        // TODO: implements post login process
    }
});
```

## TemporaryWithdrawal

'Suspension of Withdrawal' is available. 
At the request for a temporary withdrawal, user can withdraw after certain period of suspension.  
Suspension period can be changed from the console. 

> <font color="red">[Caution]</font><br/>
>
> To suspend withdrawal, please do not use **Gamebase.withdraw()** API.
> **Gamebase.withdraw()** API regards to immediate withdrawal from account. 

After login, the withdrawal status of a user can be found with  AuthToken.getTemporaryWithdrawalInfo() API to see if the user is suspended from withdrawal.  

### Request TemporaryWithdrawal

Send a request for a temporary withdrawal. 
After a certain period specified on console, user withdrawal is automatically processed.  

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.requestWithdrawal(@NonNull Activity activity,
                                                       @Nullable GamebaseDataCallback<TemporaryWithdrawalInfo> callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | User is already under the process of temporary withdrawal.  |

**Example**

```java
public static void testRequestWithdraw() {
    Gamebase.TemporaryWithdrawal.requestWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(TemporaryWithdrawalInfo data GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                    // Already requested temporary withdrawal before.
                } else {
                    // Request temporary withdrawal failed.
                    return;
                }
            }

            // Request temporary withdrawal success.
        }
    });
}
```

### Check TemporaryWithdrawal User

Games enabling the suspension of withdrawal must always call AuthToken.getTemporaryWithdrawalInfo() API, after login; and if the result returns a valid  TemporaryWithdrawalInfo object, the user must be notified that withdrawal is underway.  

**Example**

```java
public static void testLogin() {
    Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Login failed
                return;
            }

            // Check if user is requesting withdrawal
            if (data.getTemporaryWithdrawalInfo() != null) {
                // User is under temporary withdrawal
                long gracePeriodDate = data.getTemporaryWithdrawalInfo().getGracePeriodDate();
            } else {
                // Login success.
            }
        }
    });
}
```

### Cancel TemporaryWithdrawal

Cancel request for a withdrawal.   
After withdrawal is completed and if the request has expired, withdrawal cannot be reverted.

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.cancelWithdrawal(@NonNull Activity activity,
                                                      @Nullable GamebaseCallback callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | The user is not under temporary withdrawal. |

**Example**

```java
public static void testCancelWithdraw() {
    Gamebase.TemporaryWithdrawal.cancelWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                    // Never requested temporary withdrawal before.
                } else {
                    // Cancel temporary withdrawal failed.
                    return;
                }
            }

            // Cancel temporary withdrawal success.
        }
    });
}
```

### Withdraw Immediately

Withdraw immediately without considering the suspension period.  
It runs the same as Gamebase.withdraw() API. 

Since immediate withdrawal cannot be cancelled, it is recommended to be confirmed by the user several times.  

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.withdrawImmediately(@NonNull Activity activity,
                                                         @Nullable GamebaseCallback callback);
```

**Example**

```java
public static void testWithdrawImmediately() {
    Gamebase.TemporaryWithdrawal.withdrawImmediately(activity, new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Withdraw failed.
                return;
            }

            // Withdraw success.
        }
    });
}
```

## GraceBan

* This is a 'purchase abuse automatic release' function.
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always call the AuthToken.getGraceBanInfo() API after login. If a valid GraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

**Example**

```java
public static void testLogin() {
    Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken token, GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Login failed
                return;
            }

            // Check if user is under grace ban
            GraceBanInfo graceBanInfo = token.getGraceBanInfo();
            if (graceBanInfo != null) {
                String periodDate = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss", Locale.getDefault())
                        .format(new Date(graceBanInfo.getGracePeriodDate()));
                String message = URLDecoder.decode(graceBanInfo.getMessage(), "utf-8");
                GraceBanInfo.ReleaseRuleCondition releaseRuleCondition =
                            graceBanInfo.releaseRuleCondition();
                GraceBanInfo.PaymentStatus paymentStatus = graceBanInfo.getPaymentStatus();
                if (releaseRuleCondition != null) {
                    // condition type : "AND", "OR"
                    String releaseRule = releaseRuleCondition.getAmount() +
                            releaseRuleCondition.getCurrency() +
                            " " + releaseRuleCondition.getConditionType() + " " +
                            releaseRuleCondition.getCount() + "time(s)";
                }
                if (paymentStatus != null) {
                    String paidAmount = paymentStatus.getAmount() + paymentStatus.getCurrency();
                    String paidCount = paymentStatus.getCount() + "time(s)";
                }
                // Guide the user through the UI how to finish the grace ban status.
            } else {
                // Login success.
            }
        }
    });
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | Request for invalid member.                        |
|                | BANNED\_MEMBER                           | 7          | Named member has been banned.                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | Login is cancelled.                          |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication is not supported.                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | Named member does not exist or has withdrawn.                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | Failed to initialize external authentication library.  |
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
| IdP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP login has failed.                         |
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
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already under temporary withdrawal.              |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not under temporary withdrawal.                 |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | Not playable (due to maintenance or service closed)         |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | Unknown error (Undefined error)             |

* Refer to the following document for the entire error codes
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* Occurs in SDK of each IdP.
* Check the error code as below.

```java
Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
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
