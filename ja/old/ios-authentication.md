## Game > Gamebase > iOS Developer's Guide > Authentication


## Login

Gamebase supports guest logins by default.

- To log into providers other than guest, a matching Provider AuthAdapter is required.
- For setting of AuthAdapter and 3rd-Party Provider SDK, refer to the following.
  [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)

In some cases, additionalInfo parameter is required for IdP trying a login. <br/>
For more details about AdditionalInfo, refer to **IdPs supported by Gamebase** below.


### Import Header File

Import the following header file to the ViewController to implement a login.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

Many games require that a login should be implemented on a title screen.
* Allow a game user to decide which IdP to authenticate on a title screen, when an app is implemented for the first time after installed.
* After initial login, the IdP selection screen does not show and authentication is made with the latest logged-in IdP.

The logic described in the above can be implemented in the following order:

#### 1. Get Latest Login Type
* Call **[TCGBGamebase lastLoggedInProvider]**.
* If there is a returned value, follow **2. Authenticate with Latest Login Type**.
* If there is no returned value, let the game user decide IdP and follow **3.Authenticate with Specified IdP**.

#### 2. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password inputs.
* Call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network Error
  * If the error code is **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to a temporary network problem, so call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]** again or try again in a minute.
* Banned Game User
  * If the error code is **TCGB_ERROR_AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
  * Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
  * When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other Errors
  * Authentication with Latest Login Type has failed. Follow **3. Authenticate with Specified IdP**.

#### 3. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
  * Types that can be authenticated are declared in **TCGBAuthIdPs** of the **TCGBConstants.h** file.
* Call **[TCGBGamebase loginWithType:viewController:completion:]** API.

#### 3-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 3-2. When Authentication is Failed

* Network Error
  * If the error code is **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, the authentication has failed due to temporary network problem, so call **[TCGBGamebase loginWithType:viewController:completion:]** again or try again in a minute.
* Banned Game User
  * If the error code is **TCGB_ERROR_AUTH_BANNED_MEMBER(3005)**, the authentication has failed due to banned game user.
  * Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
  * When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other Errors
  * Notify that an error has occurred to a user, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login with Latest Login IdP

When a login button is clicked for a specific IdP, following login API will be implemented.<br/>
Try login with the most recently logged-in IdP. If a token is expired or its authentication fails, return failure.<br/>
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

To call a specific IdP login, call **[TCGBGamebase loginWithType:viewController:completion:]**.<br/>
If it is the first login-trial via Gamebase, or login information (access token) has been expired, it is required to use this API to try a login. <br/>
With a login result, you can see if it is successful or not by using **(TCGBError *)error**. <br/>
In addition, by using **TCGBAuthToken**, you can get user information, such as user ID and token.<br/>
When a login is successful, Gamebase access token is saved at a Local Storage; to use loginForLastLoggedInProviderWithViewController:completion: method, the stored access token can be applied. <br/>
However, access token of each IdP is managed by SDK of each IdP. <br/>

<br/><br/>
There is information which must be included for login with some IdPs. <br/>
For instance, scope must be set to implement a Facebook login. <br/>
In order to set such necessary information, the **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API is provided.<br/>
You can enter those information to additionalInfo in the dictionary type.<br/>
(When the parameter value is nil, the additionalInfo registered in the TOAST Cloud Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console.)


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
- Set AdditionalInfo.
    * Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**.
    * When trying OAuth authentication, type of information to request to Facebook should be set.

Example of Adding Authentication Information to Facebook
```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```
- To setup a project for the use of Facebook SDK, refer to the following link.
* [Facebook Developer Guide](https://developers.facebook.com/docs/ios/getting-started)

#### PAYCO
- Set AdditionalInfo.
    * Go to **TOAST Cloud Console > Gamebase > App > Authentication Information > Additional Information & Callback URL** to set json string-type information to **Additional Information**.
    * **service_code** and **service_name** should be set as PaycoSDK requires.

Example of Adding Authentication Information to PAYCO
```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

#### Game Center
Requires no additional setting other than TOAST Cloud Console setting.



### Login with Credential

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.




* How to Set Credential Parameters



| Keyname                                  | Use                                      | Value Type                     |
| ---------------------------------------- | ---------------------------------------- | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                             | facebook, payco, iosgamecenter |
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
#import "TCGBConstants.h"

- (void)auth_login_with_credential {
	NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"여기에 facebook SDK에서 발급받은 Access Token을 입력하세요" };
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

Try to log out from logged-in IdP. In many cases, the log-out button is located on game configuration screen to click to implement.
Even if a log-out is successful, data of game users remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords inputs will be required for the next log-in process.<br/><br/>

Following shows an exemplary log-out code with a click of the log-out button.

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

Below shows an example of how to withdraw game users while they're logged-in. <br/><br/>

* When a user is successfully withdrawn, the user's data interfaced with a login IdP are all deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdraw, a log-out from IdP will be tried.


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

In many games, one account may have many integrated (mapped) IdPs.<br/>
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.<br/><br/>

In short, a login to an mapped IdP account will be made available with a same user ID at all times.<br/><br/>

Keep in mind, though, each IdP can have only one account to map.<br/>
Below shows an example of account mapping.<br/><br/>

* Gamebase UserID: 123bcabca
  * Google ID: aa
  * Facebook ID: bb
  * Apple Game Center ID: cc
  * Payco ID: dd
* Gamebase UserID: 456abcabc
  * Google ID: ee
  * Google ID: ff **-> As the Google ee account is integrated, another Google account cannot be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.


### Import Header file into View Controller

Import the following header file to the ViewController to implement mapping.

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

Try mapping to another IdP while logged-in to a specific IdP.<br/>
If an IdP account to map has already been integrated to another account,<br/>
return the **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER** error.<br/><br/>

Even if a mapping is successful, 'currently logged-in IdP' does not change. For example, after a user logs in a Google account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Google to Facebook. It still stays with Google account. <br/>
Mapping simply adds IdP integration. <br/><br/>

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



| Keyname                                  | Use                                      | Value Type                     |
| ---------------------------------------- | ---------------------------------------- | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                             | facebook, payco, iosgamecenter |
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

Remove mapping with a specific IdP. <br/>
If an IdP to remove mapping is an **only IdP**, return failure.<br/>
After mapping is removed, log out the IdP in Gamebase.

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

For a banned user registered at Gamebase Console,
restricted use of information code can be displayed as below, when trying login. The ban information can be found by using the **[TCGBGamebase banInfo]** method.

* TCGB_ERROR_AUTH_BANNED_MEMBER



## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | Login has been canceled.                 |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication method is not supported. |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | The member does not exist or has withdrawn. |
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





* Refer to the following document for the entire error codes.
  - [Entire Error Codes](./error-codes#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**
* This error occurred in each IdP SDK.
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
