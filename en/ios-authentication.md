## Game > Gamebase > iOS Developer's Guide > Authentication


## Login

Gamebase supports guest logins by default.

- To log into providers other than guest, a matching Provider AuthAdapter is required.
- For setting of AuthAdapter and 3rd-Party Provider SDK, refer to the following.
    - [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)

In some cases, additionalInfo parameter is required for IdP trying a login.
For more details about AdditionalInfo, refer to **IdPs supported by Gamebase** below.


### Import Header File

Import the following header file to the ViewController to implement a login.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

In many games, login is implemented on a title page.
* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed.
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described in the above can be implemented in the following order.

#### 1. Get Latest Login Type

* Call **[TCGBGamebase lastLoggedInProvider]**.
* If there is a returned value, follow **2. Authenticate with Latest Login Type**.
* If there is no returned value, let the game user decide IdP and follow **3. Authenticate with Specified IdP**.

#### 2. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password.
* Call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network error
	* If the error code is **TCGB\_ERROR\_SOCKET\_ERROR (110)** or **TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]** again or try again in a moment.
* Banned game user
	* If the error code is **TCGB\_ERROR\_AUTH\_BANNED\_MEMBER (3005)**, the authentication has failed due to banned game user.
	* Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
	* When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
	* Authentication with latest login type has failed. Follow **3. Authenticate with Specified IdP**.


#### 3. Authenticate with Specified IdP

- Try to authenticate by specifying an IdP type.
  - Types that can be authenticated are declared in **TCGBAuthIdPs** of the **TCGBConstants.h** file.
- Call **[TCGBGamebase loginWithType:viewController:completion:]** API.

#### 3-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 3-2. When Authentication is Failed

* Network error
	* If the error code is **TCGB\_ERROR\_SOCKET\_ERROR (110)** or **TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT (101)**, the authentication has failed due to temporary network problem, so call **[TCGBGamebase loginWithType:viewController:completion:]** again or try again in a moment.
* Banned game user
	* If the error code is **TCGB\_ERROR\_AUTH\_BANNED\_MEMBER (3005)**, the authentication has failed due to banned game user.
	* Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
	* When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
	* Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login as the Latest Login IdP

When a login button is clicked for a specific IdP, following login API will be implemented.
Try login with the most recently logged-in IdP. If a token is expired or its authentication fails, return failure.
Note that a login should be implemented for the IdP.

```objectivec
- (void)automaticLogin {
    // Last Logged In Provider Name
    NSString *lastLoggedInProvider = [TCGBGamebase lastLoggedInProvider];

    [TCGBGamebase loginForLastLoggedInProviderWithViewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithViewController:completion: or Notify to user -\n\terror[%@]", [error description]);
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");

                [TCGBGamebase loginWithType:lastLoggedInProvider viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                    if ([TCGBGamebase isSuccessWithError:error] == YES) {
                        NSLog(@"Login is succeeded.");
                    }
                    else {
                        NSLog(@"Login is failed.");
                    }
                }];
            }
        }
    }];
}
```

### Login with IdP

To call a specific IdP login, call **[TCGBGamebase loginWithType:viewController:completion:]**.
If it is the first login-trial via Gamebase, or login information (access token) has been expired, it is required to use this API to try a login. With a login result, you can see if it is successful or not by using **(TCGBError \*) error**.
In addition, by using **TCGBAuthToken** , you can get user information, such as user ID and token.
When a login is successful, Gamebase access token is saved at a local storage; to use loginForLastLoggedInProviderWithViewController:completion: method, the stored access token can be applied.
However, access token of each IdP is managed by SDK of each IdP.

<br/><br/>
There is information which must be included for login with some IdPs.
For instance, scope must be set to implement a Facebook login.
In order to set such necessary information, the **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API is provided.
You can enter those information to additionalInfo in the dictionary type.
(When the parameter value is nil, the additionalInfo registered in the TOAST Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console.)


> [Note]
>
> The IdP supported by iOS is defined as **kTCGBAuthXXXXXX** in the area of TCGBAuthIDPs of **TCGBConstants.h**.
>

```objectivec
- (void)loginFacebookButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthPayco viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember.userId];
        } else {
            // To Login Failed
        }
    }];
}
```

#### IdPs Supported by Gamebase
#### Guest
#### Facebook

* Set AdditionalInfo.
	* Go to **TOAST  Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**.
	* When trying OAuth authentication, type of information to request to Facebook should be set.


Example of Adding Authentication Information to Facebook
```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

* To setup a project for the use of Facebook SDK, refer to the following link.
* [LINK \[Facebook Developer Guide\]](https://developers.facebook.com/docs/ios/getting-started)

#### PAYCO

* Set AdditionalInfo.
	* Go to **TOAST Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**.
	* **service\_code** and **service\_name** should be set as PaycoSDK requires.

Example of Adding Authentication Information to PAYCO
```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

#### Game Center
Requires no additional setting other than TOAST Console setting.

### Login with Credential

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.

* How to Set Credential Parameters

| keyname                                  | Usage                          | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, payco, iosgamecenter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP  |                                |

> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>


> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires need to be implemented by using external SDK's API, which Gamebase does not support.
>

```objectivec
#import "TCGBConstants.h"

- (void)auth_login_with_credential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"Enter Access Token issued by Facebook SDK" };
    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

## Logout

#### Import Header File

Import the following header file to the ViewController to implement a logout.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### Logout API

Try to log out from logged-in IdP. In many cases, the log-out button is located on the game configuration screen.
Even if a logout is successful, data of game users remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next login process.

Following shows a log-out example code with a click of the log-out button.

```objectivec
[TCGBGamebase logoutWithViewController:self completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Logout Succeeded
    } else {
        // To Logout Failed
    }
}];
```

## Withdraw

### Import Header File

Import the following header file to the ViewController to implement withdrawal.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Widthdraw API

Below shows an example of how a game user withdraws while logged-in.

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdrawal, a log-out from IdP will be tried.

Following shows an exemplary withdrawal code with a click of the withdraw button.

```objectivec
[TCGBGamebase withdrawWithViewController:self completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Withdrawal Succeeded
    } else {
        // To Withdrawal Failed
    }
}];
```

## Mapping

In many games, one account may have many integrated (mapped) IdPs.

By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.

In short, a login to a mapped IdP account will be made available with a same user ID at all times.

Note, however, that each IdP can have only one account to map.
Below shows an example.

* Gamebase UserID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
    * Payco ID: dd
* Gamebase UserID: 456abcabc
    * Google ID: ee
    * Google ID: ff **->  As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

### Add Mapping Flow

Implement mapping in the following order.

#### 1. Login

Mapping means to add an IdP account integration to a current account, so login is a prerequisite.
First, call a login API and log in.

#### 2. Mapping

Call **[TCGBGamebase addMappingWithType:viewController:completion:]** to try mapping.

#### 2-1. When mapping is successful

* Congratulations! Successfully added an IdP account integrated with the current account.
* Even if a mapping is successful, 'currently logged-in IdP' will not change. For example, after a user’s login with Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account.
* Mapping simply adds IdP integration.

#### 2-2. When mapping is failed

* Network error
    * If the error code is **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **[TCGBGamebase addMappingWithType:viewController:completion:]** again or try again in a moment.
* Error of integration to another account
    * If the error code is **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, the IdP account to map has been already integrated to another account.To remove the integrated account, log in the account and call **[TCGBGamebase withdrawWithViewController:completion:]** to withdraw, or call **[TCGBGamebase removeMappingWithType:viewController:completion:]** to remove integration and try mapping again.
* Error of integration to a same IdP account
    * If the error code is **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, a same type of account to the IdP has already been integrated.
	* Gamebase mapping allows only one account of integration to an IdP. For example, if your account is already integrated to a PAYCO account, no other PAYCO account can be added.
	* To integrate another account of a same IdP, call **[TCGBGamebase removeMappingWithType:viewController:completion:]** to remove integration and try mapping again.
* Other Errors
    * Mapping has failed.


### Import Header file into View Controller

Import the following header file to the ViewController to implement mapping.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Add Mapping API

Try mapping to another IdP while logged-in to a specific IdP.

Below is an example of mapping to Facebook.

```objectivec
[TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
                 NSLog(@"AddMapping is succeeded.");
             }
             else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_RESPONSE_TIMEOUT) {
                 NSLog(@"Retry addMapping")
             }
             else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                 NSLog(@"Already mapped to other member");
             }
             else {
                 NSLog(@"AddMapping Error - %@", [error description]);
             }
        }
}];
```

### AddMapping with Credential

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.

* How to Set Credential Parameters

| keyname                                  | Usage                          | Value Type                          |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, payco, iosgamecenter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |

> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> Development items external SDK requires to support need to be implemented by using external SDK's API, which Gamebase does not support.
>


```objectivec
     - (void)onButtonLogin {
         UIViewController* topViewController = nil;
 
         NSString* facebookAccessToken = @"feijla;feij;fdklvda;hfihsdfeuipivaipef/131fcusp";
         NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
         credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = @"facebook";
         credentialInfo[kTCGBAuthLoginWithCredentialAccessTokenKeyname] = facebookAccessToken;
 
         [TCGBGamebase addMappingWithCredential:credentialInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
             if ([TCGBGamebase isSuccessWithError:error] == YES) {
                 NSLog(@"AddMapping is succeeded.");
             }
             else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_RESPONSE_TIMEOUT) {
                 NSLog(@"Retry addMapping")
             }
             else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                 NSLog(@"Already mapped to other member");
             }
             else {
                 NSLog(@"AddMapping Error - %@", [error description]);
             }
         }];
     }
```


### Remove Mapping API

Remove mapping with a specific IdP.
If IdP mapping is not removed, error will occur.
After mapping is removed, Gamebase processes logout of the IdP.

```objectivec
[TCGBGamebase removeMappingWithType:@"facebook" viewController:self completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}
}];
```

### Get IdP Mapping List

Check the list of mapped accounts to IdPs.

```objectivec
// Obtaining Names of Mapping IDPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase User`s Information

Process authentication with Gamebase, in order to get information required to create an app.

> <font color="red">[Caution]</font><br/>
>
> Cannot import authentication information when you're logged in with "[TCGBGamebase lastLoggedInProvider]" API.
>
> To obtain authentication information, log in with "[TCGBGamebase loginWithType:IDP_CODE viewController: self-completion: completion];" API with {IDP_CODE} parameter, which is same as IDPCode to use, instead of "[TCGBGamebase lastLoggedInProvider]".

### Get Authentication Information for Gamebase

Get authentication information issued by Gamebase.

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];

// Obtaining Ban Information
TCGBBanInfo* banInfo = [TCGBGamebase banInfo];
```

### Get Authentication Information for External IdP

Get access token, User ID, and profiles from externally authenticated SDK.

```objectivec
// Example for obtaining ID Provider's Authentication Information

// Obtaining Facebook UserID
NSString *userID = [TCGBGamebase authProviderUserIDWithIDPCode:@"facebook"];

// Obtaining Facebook AccessToken
NSString *accessTokenOfIDP = [TCGBGamebase authProviderAccessTokenWithIDPCode:@"facebook"];

// Obtaining Facebook Profile
TCGBAuthProviderProfile *providerProfile = [TCGBGamebase authProviderProfileWithIDPCode:@"facebook"];
```

### Get Banned User Information

For users who are registered banned in the Gamebase Console, information codes of restricted use will be displayed as below, when they try to log in.
The ban information can be found by using the **[TCGBGamebase banInfo]** method.

* TCGB_ERROR_AUTH_BANNED_MEMBER

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | Login is cancelled.                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | The authentication is not supported.                       |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | Named member does not exist or has withdrawn.                      |
|                | TCGB\_ERROR\_AUTH\_INVALID\_MEMBER       | 3004       | Request for invalid member.                        |
|                | TCGB\_ERROR\_AUTH\_BANNED\_MEMBER        | 3005       | Named member has been banned.                               |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | Error in external authentication library.                       |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | Token login has failed.                         |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                        |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IDP information.                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       |  IDP login has failed.                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP information is invalid. (The IdP information is unavailable in console.) |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | Add mapping has failed.                         |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IDP.                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Invalid IDP information.(IDP information does not exist in the Console.) |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | Remove mapping has failed.                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IDP.               |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | Currently logged-in IDP.                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | Logout has failed.                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | Withdrawal has failed.                              |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | Not playable.(due to maintenance or service closed).       |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | Unknown error(Undefined error)            |




* Refer to the following document for the entire error codes.
	* [Entire Error Codes](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**

* Occurs in each IdP SDK.
* Can check error codes as follows.
* For error codes of IdP SDK, refer to each Developer page.


```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


