## Game > Gamebase > JavaScript SDK User Guide > Authentication

## Login
The Gamebase supports guest login by default.
To use other IdPs (identity providers, such as Google, Facebook, Line, NAVER, Twitter), please see the section "Login with IdP".


### Login Flow
In most games, login is implemented on the title screen.
* This allows you to select an IdP to be used for authentication on the initial title screen of the game.

The logic described above can be implemented in the following sequence.
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)

#### 1. Authenticate with the specified IdP

* Try authentication by specifying the IdP type.
    * The authenticable types are as follows:
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * PAYCO('payco')
        * NAVER('naver')
        * Line('line')
* **toast.Gamebase.login(providerName, (authToken, error) => { ... })** Calls the API.

#### 1-1. When authentication is successful

* Congratulations! Successfully authenticated.
* Get a user ID with **toast.Gamebase.getUserID()** to implement a game logic.

#### 1-2. When authentication fails

* Network error
    * If the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **toast.Gamebase.login(providerName, callback)** again, or try again in a moment.
* Banned game user
    * If the error code is **BANNED_MEMBER(7)**, the authentication has failed due to banned game user.
    * Check ban information with **toast.Gamebase.getBanInfo()** and notify the user with reasons for not being able to play.
    * When **enablePopup** is set to true during Gamebase initialization, Gamebase will automatically display a popup on banning.
* Other errors
    * Notifies that an error has occurred and returns to the state (mostly title or login screen) in which user can select an authentication IdP type.

### Login with IdP

Below is an example of logging in with a specific IdP.

The IdP types available for login are as follows:

* Guest('guest')
* Google('google')
* Facebook('facebook')
* PAYCO('payco')
* NAVER('naver')
* Line('line')

> <font color="red">[Caution]</font><br/>
>
> 'Popup' is used for authentication in the Gamebase. If 'popup' is not allowed, the user may have to keep waiting in the 'trying to log in' state.
> A message such as 'Please allow popup for the authentication step' should be displayed in advance for convenient user experience.
>

```js
toast.Gamebase.login(providerName, (authToken, error) => { ... });
```

**Example**
```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Indicates that the network cannot be accessed temporarily due to socket error.
                // Check the network status or try again after a while.
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // The user who tried logging in is banned.
                // GamebaseConfiguration.uiConfiguration.enablePopup(true) 및
                // If initialized with GamebaseConfiguration.uiConfiguration.enableBanPopup(true),
                // The Gamebase displays the UI for ban automatically.

                // To implement the ban popup directly according to the game UI, check the ban information with toast.Gamebase.getBanInfo()
                // and notify the user with reasons for not being able to play.
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // Login failed
            }

            return;
        }

        // Login successful
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

### Login with Credential

This is the interface which allows users to log in to the Gamebase with the access token issued by authenticating in game with SDK or REST API provided by the IdP.

**How to set the Credential parameter**

| key name             | a use                                                               |
| -------------------- | ------------------------------------------------------------------- |
| providerName         | IdP type setting                                                        |
| accessToken          | Set the authentication information (access token) received after login to IdP                          |
| accessTokenSecret    | Set the authentication information (access token secret) received after login to IdP                    |

> [Note]
>
> This may be needed to use unique functions of external services (e.g. Facebook).
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Developments requested by an external SDK must be implement using APIs from the external SDK; Gamebase does not support those development features.
>

```js
var credential = {
    providerName: ${IdP},
    acessToken: ${IdP AccessToken},
    accessTokenSecret: ${IdP AccessTokenSecret}, // This is only necessary in the case that IdP gives you this value.
};

toast.Gamebase.loginWithCredential(credential, callback);
```

**Example**

```js
function loginWithFacebookCredential() {
    var credential = {
        providerName: 'facebook',
        accessToken: 'EAAL24qLuXHwBAFL1hS72BzZCUrJyDMRxUq1H5ZBGFIT..........',
    };

    toast.Gamebase.loginWithCredential(credential, function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Indicates that the network cannot be accessed temporarily due to socket error.
                // Check the network status or try again after a while.
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // The user who tried logging in is banned.
                // GamebaseConfiguration.uiConfiguration.enablePopup(true) 및
                // If initialized with GamebaseConfiguration.uiConfiguration.enableBanPopup(true),
                // The Gamebase displays the UI for ban automatically.

                // To implement the ban popup directly according to the game UI, check the ban information with Gamebase.getBanInfo()
                // and notify the user with reasons for not being able to play.
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // Login failed
            }

            return;
        }

        // Login successful
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

## Logout

Try logging out from the IdP currently logged in. In general, the logout button is placed in the Setting screen of the game and the button is executed with click.
Even if logged out, the game user data is maintained.
When logout is successful, the record authenticated with the corresponding IdP is removed. Therefore, the window with the ID/password fields are displayed for the next login.<br/><br/>

The following example shows the code that lets you log out just by clicking the **Logout** button.

```js
toast.Gamebase.logout((error) => { ... })
```

**Example**
```js
function logout() {
    toast.Gamebase.logout(function (error) {
        if (error) {
            // Logout failed
            console.log(error);
            return;
        }

        // Logout successful
        console.log(data);
    });
}
```

## Withdraw

The following example shows the code of how a game user withdraws while logged-in.<br/><br/>

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase but not IdP account.
* After a successful withdrawal, a logout from IdP will be tried.

> <font color="red">[Caution]</font><br/>
>
> When several IdPs are linked with a user, all IdP links are released and the Gamebase game user data is removed.
>

```js
toast.Gamebase.withdraw(callback)
```

**Example**
```js
function withdraw() {
    toast.Gamebase.withdraw(function (data, error) {
        if (error) {
            // Withdrawal failed
            console.log(error);
            return;
        }

        // Withdrawal successful
         console.log(data);
    });
}
```

## Gamebase User's Information
Proceed the authentication with Gamebase to get information required to create an app.

### Get Authentication Information for Gamebase
Get authentication information issued by Gamebase.

```js
// Obtaining Gamebase UserID
var userId =  toast.Gamebase.getUserID();

// Obtaining Gamebase AccessToken
var accessToken = toast.Gamebase.getAccessToken();
```

### Get Banned User Information
If you are registered as a banned game user in the NHN Cloud Gamebase console,
the following ban information code will be displayed when you try to log in. You can check the ban information with the **toast.Gamebase.getBanInfo()** method.

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)

## TemporaryWithdrawal

This is a 'pending withdrawal" feature.
By requesting a temporary withdrawal, the account is not immediately withdrawn. Instead, it is withdrawn after a specific grace period.
The grace period can be changed in the console.

> 'Caution'
>
> Do not use **Gamebase.withdraw()** API if you're using the Pending Withdrawal feature.
> The **Gamebase.withdraw()** API immediately withdraws accounts when used.

If login is successful, AuthToken.member.temporaryWithdrawal can be used to determine if the user is in the status of pending withdrawal.

### Request TemporaryWithdrawal

Requests a temporary withdrawal.
The account is automatically withdrawn after a specific grace period set in the console.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.requestWithdrawal(callback)
```

**Example**

```js
function requestWithdrawal() {
    gamebase.TemporaryWithdrawal.requestWithdrawal(function (data, error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            } else {
                // Request temporary withdrawal failed.
            }
            return;
        }
        
        // Request temporary withdrawal success.
    });
}
```

### Check TemporaryWithdrawal User

For games using the Pending Withdrawal feature must notify its users that they are in grace period if AuthToken.member.temporaryWithdrawal is not null after login.

**Example**

```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
            // Login failed
            return;
        }

        if(authToken.member.temporaryWithdrawal != null) {    
            // User is under temporary withdrawal    
            var gracePeriodDate = authToken.member.temporaryWithdrawal.gracePeriodDate;            
        } else {
            // Login success.
        }
    });
}
```

### Cancel TemporaryWithdrawal

Cancels a withdrawal request.
If the grace period is over and the withdrawal process is completed, it cannot be undone.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.cancelWithdrawal(callback)
```
**Example**

```js
function cancelWithdrawal() {
    gamebase.TemporaryWithdrawal.cancelWithdrawal(function (error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            } else {
                // Cancel temporary withdrawal failed.
            }
            return;
        }
        
        // Cancel temporary withdrawal success.
    });
}
```

### Withdraw Immediately

Immediately withdraws the account, ignoring the grace period.
The internal mechanics are the same as the Gamebase.withdraw() API.

Instant withdrawal cannot be undone, so it is important to ask the user several times if they really want to execute the command.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.withdrawImmediately(callback)
```

**Example**

```js
function withdrawImmediately() {
    gamebase.TemporaryWithdrawal.withdrawImmediately(function (error) {
        if (error) {
            // Withdraw failed.
            return;
        }
        
        // Withdraw success.
    });
}
```

## Error Handling
| Category       | Error                                                   | Error Code | Description                                                       |
| -------------- | ------------------------------------------------------- | ---------- | ----------------------------------------------------------------- |
| Auth           | INVALID\_MEMBER                                         | 6          | Invalid user request.                                            |
|                | BANNED\_MEMBER                                          | 7          | The user is temporarily banned.                                                     |
|                | AUTH\_USER\_CANCELED                                    | 3001       | Login has been canceled.                                                |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | The authentication method is not supported.                                           | 
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | The member either does not exist or withdrew their account.                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | The previous authentication process is not complete.                                   |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | Token login failed.                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | Invalid token information.                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | No recent login IdP information.                                      |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdP login failed.                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | Invalid IdP information. (The console has no information about the IdP.)           |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       | Failed to log out.                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | Failed to withdraw the account.                                                   |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already in the status of temporary withdrawal.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not in the status of temporary withdrawal.                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | The game is unavailable at the moment (for maintenance, service termination, or other reasons).                        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | An unknown error occurred (undefined error).                             |

* For the entire list of error codes, see the following document.
    * [Entire Error Codes] (./error-code/#client-sdk)
    