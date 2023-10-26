## Game > Gamebase > iOS Developer's Guide > Authentication


## Login

Gamebase supports guest logins by default.

- To log in to a provider other than the guest, the corresponding Provider AuthAdapter is required.
- For AuthAdapter and 3rd-Party Provider SDK settings, see the following.
    - [Game > Gamebase > iOS SDK User Guide > Getting Started > 3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

In some cases, additionalInfo parameter is required for IdP trying a login.
For more details about AdditionalInfo, refer to **IdPs supported by Gamebase** below.


### Import Header File

Import the following header file to the ViewController to implement a login.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

In most games, login is implemented on the title screen.

* Ensures game users are able to select which IdP (identity provider) to use for authentication in the title screen when installing and running the app for the first time.
* Once you log in, the authentication is done via IdP type which was previously logged in without displaying the IdP selection screen.

The logic described in the above can be implemented in the following order.

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password.
* Call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**.

#### 1-1. When Authentication is Successful

* Congratulations! Successfully authenticated
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 1-2. When Authentication Fails

* Network error
    * If the error code is **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, it means the authentication failed due to temporary network issues, so call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]** again or try again later.
* Banned game user
    * If the error code is **TCGB_ERROR_BANNED_MEMBER(7)**, the authentication failed because the game user has been banned.
    * Check the ban information with **[TCGBBanInfo banInfoFromError:error]** and inform the game user why he cannot play the game.
    * If **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called when initializing Gamebase, Gamebase automatically displays a popup explaining the reason for user ban.
* Other errors
    * Authentication with the previous login type has failed. **'2. Authenticate with the designated IdP'**.

#### 2. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
    * Types that can be authenticated are declared in **TCGBAuthIdPs** of the **TCGBConstants.h** file.
* Call **[TCGBGamebase loginWithType:viewController:completion:]** API.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

### 2-2. When Authentication Fails

* Network error
    * If the error code is either **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, authentication failed because of a temporary network problem. In this case, call **[TCGBGamebase loginWithType:viewController:completion:]** again or try authenticating later.
* Banned game users
    * If the error code is **TCGB_ERROR_BANNED_MEMBER(7)**, it means authentication failed because the user has been banned.
    * Check the ban information with **[TCGBBanInfo banInfoFromError:error]** and inform the game user why they cannot play the game.
    * If **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called when initializing Gamebase, Gamebase automatically displays a popup explaining the ban.
* Other errors
    * Informs users of an error and returns them to the state where they can select an authentication IdP type (usually title screen or login screen).

### Login as the Latest Login IdP

Try login with the most recently logged-in IdP. If a token is expired
or its authentication fails, return failure.<br/>
Note that a login should be implemented for the IdP.




```objectivec
- (void)automaticLogin {
    [TCGBGamebase loginForLastLoggedInProviderWithViewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"Login is succeeded.");
            //TODO: 1. Do you want.
        }
        else {
            if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
                NSLog(@"Retry loginForLastLoggedInProviderWithViewController:completion: or Notify to user -\n\terror[%@]", [error description]);
                //TODO: 1. If the error had occured by network problem, you can retry by loginForLastLoggedInProviderWithViewController:completion:
            }
            else {
                NSLog(@"Try to login with loginWithType:viewController:completion:");
                // Last Logged In Provider Name
    			NSString *lastLoggedInProvider = [TCGBGamebase lastLoggedInProvider];
    			if (lastLoggedInProvider == nil || lastLoggedInProvider <= 0) {
                	//TODO: 1. Show your UI what user want to sign in.
                    //2. If the user has selected IdP, set lastLoggedInProvider to it.
                    //3. Invoke loginWithType:viewController:completion: method to try login.
                }

                // Try to login with IdP authentication
                //Warning: If you receive an event asynchronously from async handler(callback), you can use codes below in the async handler.
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
If it is the first login-trial via Gamebase, or login information (access token) has been expired, it is required to use this API to try a login.<br/>
With a login result, you can see if it is successful or not by using **(TCGBError \*) error**.<br/>
In addition, by using **TCGBAuthToken** , you can get user information, such as user ID and token.<br/>
When a login is successful, Gamebase access token is saved at a local storage; to use loginForLastLoggedInProviderWithViewController:completion: method, the stored access token can be applied.<br/>
However, access token of each IdP is managed by SDK of each IdP.<br/>

> [Note]
>
> The IdP supported by iOS is defined as **kTCGBAuthXXXXXX** in the TCGBAuthIDPs area of **TCGBConstants.h**.
>

> [Note]
>
> Some IdPs require additional information when logging in.
> To be able to set the required information, **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API is provided.
> Enter the required information into the parameter additionalInfo in the form of the dictionary.
> If there is a value in the additionalInfo parameter, use that value. if null, use the value registered in [NHN Cloud Console](./oper-app/#authentication-information).

> [Note]
>
> LINE IdPs can set LINE service regions starting with Gamebase SDK 2.43.0.
> The service regions can be set in the additionalInfo.

* How to Set additionalInfo Parameters

| keyname                                  | a use                          | Value type                         |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | Set LINE service region | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

```objectivec
+ (void)loginWithType:(NSString *)type viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
+ (void)loginWithType:(NSString *)type additionalInfo:(nullable NSDictionary<NSString *, id> *)additionalInfo viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

```objectivec
- (void)loginFacebookButtonClick {
    [TCGBGamebase loginWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

```objectivec
 - (void)loginLineButtonClick {
     NSDictionary *additionalInfo = @{ 
        @"key" : @"value" 
    };
    
     [TCGBGamebase loginWithType:kTCGBAuthLine additionalInfo:additionalInfo viewController:viewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
       if ([TCGBGamebase isSuccessWithError:error] == YES){
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

### Login with Credential

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.


* How to Set Credential Parameters

| keyname                                  | Usage                          | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, iosgamecenter, naver, google, twitter, line, appleid, hangame, weibo, kakaogame |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                   | 
| kTCGBAuthLoginWithCredentialIgnoreAlreadyLoggedInKeyname | Allow login attempts using other accounts while logged into Gamebase without logging out  | **BOOL** |      
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | One of the LINE service regions to log in  | [See Login with IdP ](./ios-authentication/#login-with-idp)|                                  


> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>
<br/>

> <font color="red">[Caution]</font><br/>
>
> Development items required by the external SDK must be implemented by using the API of the external SDK, which is not supported by Gamebase.
>

```objectivec
#import "TCGBConstants.h"

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: kTCGBAuthFacebook, kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"Enter the Access Token issued by the Facebook SDK here" };
    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

### Authentication Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout

#### Import Header File

Import the following header file to the ViewController to implement a logout.

```objectivec
#import <Gamebase/Gamebase.h>
```

#### Logout API

Try to log out from logged-in IdP. In many cases, the log-out button is located on the game configuration screen.
Even if a logout is successful, data of game users remain.
When it is successful, as authentication records with a corresponding IdP are removed, ID and passwords will be required for the next login process.<br/><br/>

Following shows a log-out example code with a click of the log-out button.

```objectivec
- (void)authLogout {
    [TCGBGamebase logoutWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Logout Succeeded
        } else {
            // To Logout Failed
        }
    }];
}
```




## Withdraw

### Import Header File

Import the following header file to the ViewController to implement withdrawal.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Withdraw API

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

Following shows an exemplary withdrawal code with a click of the withdraw button.

```objectivec
- (void)authWithdrawal {
    [TCGBGamebase withdrawWithViewController:topViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Withdrawal Succeeded
        } else {
            // To Withdrawal Failed
        }
    }];
}
```

## Mapping

Mapping refers to connecting or disconnecting an existing login account to/from another IdP account.

In many games, one account may have many integrated (mapped) IdPs.<br/>By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.

In short, a login to a mapped IdP account will be made available with a same user ID at all times.<br/><br/>

Note, however, that each IdP can have only one account to map.<br/>
For instance, if a Google account is mapped, no other Google account can be additionally mapped.<br/>
Below shows an example.<br/><br/>

* Gamebase UserID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
* Gamebase UserID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

Mapping API includes Add Mapping API and Remove Mapping API.

> <font color="red">[Caution]</font><br/>
>
> If the mapping is successful during a Guest login, the Guest IdP disappears.
>

### Add Mapping Flow

Implement mapping in the following order.

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. Login
Mapping means to add an IdP account integration to a current account, so login is a prerequisite.
First, call a login API and log in.

#### 2. Mapping

Call **[TCGBGamebase addMappingWithType:viewController:completion:]** to try mapping.

#### 2-1. When mapping is successful

* Congratulations! Successfully added an IdP account integrated with the current account.
* Even if a mapping is successful, 'currently logged-in IdP' will not change. For example, after a user’s login with Gamecenter account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Gamecenter to Facebook. It still stays with Gamecenter account.
    * <font color="red">[Caution]</font><br/> : The Guest account is an exception. If the mapping attempt performed while logged in with the Guest account is successful, the Guest IdP is **deleted** and the 'currently logged-in IdP' is also changed to the mapped IdP.
* Mapping simply adds IdP integration.

#### 2-2. When mapping fails

* Network error
    * If the error code is **TCGB_ERROR_SOCKET_ERROR(110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**, it means that the authentication failed due to temporary network issues, so call **[TCGBGamebase addMappingWithType:viewController:completion:]** again or try again later.
* Error that occurs when already linked to another account
    * If the error code is **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, it means that the account of the IdP to map is already linked to another account. Using the **ForcingMappingTicket** obtained at this point, you can try force mapping (**[TCGBGamebase addMappingForciblyWithTicket:viewController:completion:]**) or changing the login account (**[TCGBGamebase changeLoginWithForcingMappingTicket:viewController:completion:]**).
* Error that occurs when already linked to the same IdP account
	* If the error code is **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, it means that an account of the same type as the IdP you want to map to is already linked.
	* Gamebase mapping allows linking of only one account per IdP. For example, if it is already linked to a Google account, no other Google account can be added.
	* To link another account of the same IdP, call **[TCGBGamebase removeMappingWithType:viewController:completion:]** to remove the linking and try mapping again.
* Other errors
    * The mapping attempt has failed.

### Import Header file into ViewController

Import the following header file to the ViewController to implement mapping.

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

Try mapping to another IdP while logged-in to a specific IdP.<br/>

* How to Set additionalInfo Parameters

| Keyname                                  | Usage                        | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | A region to perform login among LINE service regions  | [See Login with IdP](./ios-authentication/#login-with-idp)|

**API**

```objectivec
+ (void)addMappingWithType:(NSString *)type viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
+ (void)addMappingWithType:(NSString *)type additionalInfo:(nullable NSDictionary<NSString *, id> *)additionalInfo viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

Below is an example of mapping to Facebook.

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### AddMapping with Credential

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.




* How to Set Credential Parameters



| keyname                                  | Usage                          | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, iosgamecenter, naver, google, twitter, line, appleid |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |



> [Note]
>
> May require when original functions of external services (such as Facebook) are in need within a game.
>

<br/>


> <font color="red">[Caution]</font><br/>
>
> Development items required by the external SDK must be implemented by using the API of the external SDK, which is not supported by Gamebase.
>

**API**

```objectivec
+ (void)addMappingWithCredential:(NSDictionary *)credentialInfo viewController:(UIViewController *)viewcontroller completion:(LoginCompletion)completion;
```

**Example**

```objectivec
- (void)authAddMappingCredential {
    UIViewController* topViewController = nil;

    NSString* facebookAccessToken = @"FACEBOOK_ACCESS_TOKEN";
    NSMutableDictionary* credentialInfo = [NSMutableDictionary dictionary];
    credentialInfo[kTCGBAuthLoginWithCredentialProviderNameKeyname] = kTCGBAuthFacebook;
    credentialInfo[kTCGBAuthLoginWithCredentialAccessTokenKeyname] = facebookAccessToken;

    [TCGBGamebase addMappingWithCredential:credentialInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
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

### Add Mapping Forcibly
If there is any account mapped to a specific IdP, try **force** mapping.
When you try **force mapping**, you need `ForcingMappingTicket` obtained from the AddMapping API.

**API**

```objectivec
+ (void)addMappingForciblyWithTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

The following is an example of force mapping to Facebook:

```objectivec
- (void)authAddMappingForcibly {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase addMappingForciblyWithTicket:ticket viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### Change Login with ForcingMappingTicket

When there is an account already mapped to a specific IdP, **change the login account**.
**When changing the login account**, the `ForcingMappingTicket` obtained from the AddMapping API is required.

If the Change Login API call fails, the existing account's login status is maintained.

**API**

```objectivec
+ (void)changeLoginWithForcingMappingTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

The following is an example of trying to change the login account to Facebook.

```objectivec
- (void)authChangeLogin {
    [TCGBGamebase addMappingWithType:kTCGBAuthFacebook viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        } else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        } else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketFromError:error];
            [TCGBGamebase changeLoginWithForcingMappingTicket:ticket viewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Change login succeeded.
                }
                else {
                    // Change login failed.
                    // The login status of the previous account is maintained.
                }
            }];
        } else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```

### Remove Mapping API

Remove mapping with a specific IdP. <br/>
If IdP mapping is not removed, error will occur.<br/>
After mapping is removed, Gamebase processes logout of the IdP.

```objectivec
[TCGBGamebase removeMappingWithType:kTCGBAuthFacebook viewController:topViewController completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // To Remove Mapping Succeeded
    } else {
        // To Remove Mapping Failed cause of the error
    }
}];
```

### Get IdP Mapping List
Check the list of mapped accounts to IdPs.

```objectivec
// Obtaining Names of Mapping IdPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase User`s Information
Process authentication with Gamebase, in order to get information required to create an app.

> <font color="red">[Caution]</font><br/>
>
> Cannot import authentication information when you're logged in with "[TCGBGamebase loginForLastLoggedInProvider]" API.
>
> If authentication information is needed, log in using the "[TCGBGamebase loginWithType:IDP_CODE viewController:topViewController completion:completion];" API, using the {IDP_CODE} parameter that is same as that of the IDPCode to be used, instead of "[TCGBGamebase loginForLastLoggedInProvider]".

### Get Authentication Information for Gamebase
Get authentication information issued by Gamebase.

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];
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
> * "[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]" API is used to log in, authentication information cannot be retrieved.
>     * If you need user information, log in with "[TCGBGamebase loginWithType:viewController:completion:]" API using the same {IDP_CODE} parameter as the IDPCode to be used instead of "[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]".

> <font color="red">[Caution]</font><br/>
>
> For appleid login using iOS 12 or earlier, the authentication information cannot be viewed.
>

### Get Banned User Information

If a user is registered while being banned in Gamebase Console,
the user will see the following usage restriction code when attempting to log in to the game. Ban information can be checked using the **[TCGBBanInfo banInfoFromError:error]** method.

* TCGB_ERROR_BANNED_MEMBER




## TransferAccount
Issues a key to transfer the guest account to another device.

This key is called **TransferAccountInfo**.
The issued TransferAccountInfo calls the **requestTransferAccount** API from another device to transfer the account.

> <font color="red">[Caution]</font><br/>

> The TransferAccountInfo key can be issued while the guest account is logged in.
> Transfer of guest account using TransferAccountInfo is allowed only when logged in to a guest account or not logged in.
> If the logged-in guest account has already been mapped to an IdP (Google, Facebook etc.) account, account transfer is not supported.

### Issue TransferAccount
Issues TransferAccountInfo to transfer the guest account.

**API**

```objectivec
+ (void)issueTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)issueTransferAccount {
     [TCGBGamebase issueTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Issued TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```

### Query TransferAccount
Queries the TransferAccountInfo information issued for guest account transfer to the Gamebase server.

**API**

```objectivec
+ (void)queryTransferAccountWithCompletion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
 - (void)queryTransferAccount {
     [TCGBGamebase queryTransferAccountWithCompletion:^(TCGBTransferAccountInfo* transferAccount, TCGBError *error) {
        NSLog(@"Published TransferAccount => %@, error => %@", [transferAccount description], [error description]);
     }];
 }
```


### Renew TransferAccount
Renews the issued TransferAccountInfo information.
There are two types of renewal: **Auto Renew** and **Manual Renew**. 
You can select either **Renew Password Only** or **Renew Both ID and Password** to renew the TransferAccountInfo information.

```objectivec
+ (void)renewTransferAccountWithConfiguration:(TCGBTransferAccountRenewConfiguration *)config completion:(TransferAccountCompletion)completion;
```

**Example**

```objectivec
- (void)renewTransferAccount {
    // If you want renew the account automatically, use this config.
    TCGBTransferAccountRenewalTargetType renewalTargetType = TCGBTransferAccountRenewalTargetTypeIdPassword;
    TCGBTransferAccountRenewConfiguration* autoConfig = [TCGBTransferAccountRenewConfiguration autoRenewConfigurationWithRenewalTarget:renewalTargetType];
    
    // If you want renew the account manually, use this config.
    TCGBTransferAccountRenewConfiguration* manualConfig = [TCGBTransferAccountRenewConfiguration manualRenewConfigurationWithAccountId:@"ID" accountPassword:@"PASSWORD"];
    [TCGBGamebase renewTransferAccountWithConfiguration:autoConfig completion:^(TCGBTransferAccountInfo *transferAccount, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error]) {
            // Renewing TransferAccount success.
            NSString* accountId = transferAccount.account.accountId;
            NSString* accountPw = transferAccount.account.accountPassword;
            return;
        }
        else {
            // Renewing TransferAccount failed.
        }
    }];
}
```




### Transfer Guest Account to Another Device
Transfers the account with TransferAccount issued with **issueTransfer** API.
When account transfer is successful, a transfer completion message will be displayed from the device where TransferAccount has been issued and a new account will be created when a guest logs in.
On the device where the account transfer was successfully made, the guest account from the previous device where TransferAccount was issued can still be used.

> <font color="red">[Caution]</font><br/>
>
> If migration succeeds while already logged in as a guest, the guest account logged in to the device will be lost.
> If incorrect id/password is attempted multiple times, an **AUTH_TRANSFERACCOUNT_BLOCK(3042)** error occurs and the account migration is blocked for a certain period of time.
> In this case, you can inform the user how long the account migration will be banned through the TCGBTransferAccountFailInfo as shown below.



**API**

```objectivec
+ (void)transferAccountWithIdPLoginWithAccountId:(NSString *)accountId accountPassword:(NSString *)accountPassword completion:(void(^)(TCGBAuthToken* authToken, TCGBError* error))completion;
```

**Example**

```objectivec
 - (void)transferOtherDevice {
    [TCGBGamebase transferAccountWithIdPLoginWithAccountId:@"1Aie0198" accountPassword:@"1Aie0199" completion:^(TCGBAuthToken* authToken, TCGBError* error) {
       if (error.code == TCGB_ERROR_AUTH_TRANSFERACCOUNT_BLOCK) {
            // Transfering Account failed.
            TCGBTransferAccountFailInfo* failInfo = [TCGBTransferAccountFailInfo transferAccountFailInfoFrom:error];
            if (failInfo == nil) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.

                NSString *failedId = failInfo.accountId;
                NSInteger failCount = failInfo.failCount;
                NSDate *blockedDate = [NSDate dateTimeIntervalSince1970:(failInfo.blockEndDate / 1000.0)];
                return;
            }
            // Transfering Account failed by another reason.
            return;  
        }
        // Transfering Account success.
        // TODO: implements post login process
    }];
 }
```



## TemporaryWithdrawal

This is a 'pending withdrawal" feature.
By requesting a temporary withdrawal, the account is not immediately withdrawn. Instead, it is withdrawn after a specific grace period.
The grace period can be changed in the console.

> <font color="red">[Caution]</font><br/>
>
> Do not use **[TCGBGamebase withdrawWithViewController:completion:]** API if you're using the Pending Withdrawal feature.
> The **[TCGBGamebase withdrawWithViewController:completion:]** API immediately withdraws accounts when used.

If login is successful, the AuthToken.getTemporaryWithdrawalInfo() API can be called to determine if the user is in the status of pending withdrawal.

### Request TemporaryWithdrawal

Requests a temporary withdrawal.
The account is automatically withdrawn after a specific grace period set in the console.

**API**

```objectivec
+ (void)requestTemporaryWithdrawalWithViewController:(nullable UIViewController *)viewController completion:(nullable TemporaryWithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | The user is already in the status of temporary withdrawal. |

**Example**

```objectivec
- (void)testRequestWithdraw {
    [TCGBGamebase requestTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBTemporaryWithdrawalInfo *info, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            }
            else {
                // Request temporary withdrawal failed.
                return;
            }
        }

        // Request temporary withdrawal success.
    }];
}
```

### Check TemporaryWithdrawal User

For games using the Pending Withdrawal feature must notify their users that they are in grace period if **TCGBAuthToken.tcgbMember.temporaryWithdrawal** is used and it returns a valid TemporaryWithdrawalInfo object instead of null.

**Example**


```objectivec
- (void)testLogin {
    [TCGBGamebase loginWithType:@"appleid" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // Login failed
            return;
        }

        // Check if user is requesting withdrawal
        if (authToken.tcgbMember.temporaryWithdrawal != nil) {
            // User is under temporary withdrawal
            long gradePeriod = authToken.tcgbMember.temporaryWithdrawal.gracePeriodDate;
        }
        else {
            // Login Success
        }
    }];
}
```


### Cancel TemporaryWithdrawal

Cancels a withdrawal request.
If the grace period is over and the withdrawal process is completed, it cannot be undone.

**API**

```objectivec
+ (void)cancelTemporaryWithdrawalWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | The user is not in the status of temporary withdrawal. |

**Example**

```objectivec
- (void)testCancelWithdraw {
    [TCGBGamebase cancelTemporaryWithdrawalWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            if (error.code == TCGB_ERROR_AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            }
            else {
                // Cancel temporary withdrawal failed.
                return
            }
        }

        // Cancel temporary withdrawal success.
    }];
}
```

### Withdraw Immediately

Immediately withdraws the account, ignoring the grace period.
The internal mechanics are the same as the **[TCGBGamebase withdrawWithViewController:completion:]** API.

Instant withdrawal cannot be undone, so it is important to ask the user several times if they really want to execute the command.

**API**

```objectivec
+ (void)withdrawImmediatelyWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```


**Example**

```objectivec
- (void)testWithdrawImmediately {
    [TCGBGamebase withdrawImmediatelyWithViewController:parentViewController completion:^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // withdraw failed.
            return;
        }

        // Withdraw success.
    }];
}
```

## GraceBan

* This is a 'purchase abuse automatic release' function.
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always check the value of TCGBAuthToken.tcgbMember.graceBanInfo after login. If a valid TCGBGraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

**Example**

```objectivec
- (void)testGraceBanInfo {
    [TCGBGamebase loginWithType:kTCGBAuthAppleID viewController:viewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // Login failed
            return;
        }
        
        // Check if user is under grace ban
        if (authToken.tcgbMember.graceBanInfo != nil) {
            TCGBGraceBanInfo *graceBanInfo = authToken.tcgbMember.graceBanInfo;
            // gracePeriodDate : epoch time in milliseconds
            long long gracePeriodDate = graceBanInfo.gracePeriodDate;
            NSString *message = [graceBanInfo.message stringByRemovingPercentEncoding];
            if (graceBanInfo.paymentStatus != nil) {
                TCGBPaymentStatus *paymentStatus = graceBanInfo.paymentStatus;
                double paymentStatusAmount = paymentStatus.amount;
                int paymentStatusCount = paymentStatus.count;
            }
            if (graceBanInfo.releaseRuleCondition != nil) {
                TCGBReleaseRuleCondition *releaseRuleCondition = graceBanInfo.releaseRuleCondition;
                double releaseRuleConditionAmount = releaseRuleCondition.amount;
                int releaseRuleConditionCount = releaseRuleCondition.count;
                NSString *releaseRuleConditionCurrency = releaseRuleCondition.currency;
                // condition type : "AND", "OR"
                NSString *releaseRuleConditionType = releaseRuleCondition.conditionType;
            }
            // Guide the user through the UI how to finish the grace ban status.
        }
        else {
            // Login Success
        }
    }];
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | Invalid member request.                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | The member is temporarily banned.                               |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | Login has been canceled.                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | The authentication method is not supported.                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | The member either does not exist or withdrew their account.                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR    | 3006       | Failed to initialize the external authentication library.                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | An external authentication library error. <br/> Please check the error details. |
|                | TCGB\_ERROR\_AUTH\_INVALID\_GAMEBASE\_TOKEN | 3011       | You have been logged out due to an invalid Gamebase Access Token.<br/>Please try logging in again. |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | Token login failed.                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                        |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | No recent login IdP information.                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdP login failed.                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | Invalid IdP information. (The console has no information about the IdP.) |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | Additional mapping failed.                           |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to a different member.                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to the same IdP.                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Invalid IdP information. (The console has no information about the IdP.) |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | AddMapping is unavailable with the guest IdP. |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | Failed to delete mapping.                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | The last mapped IdP cannot be deleted.                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | The IdP is currently logged in.                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | Failed to log out.                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | Failed to withdraw the account.                              |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already in the status of temporary withdrawal.                    |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not in the status of temporary withdrawal.                     |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | The game is unavailable at the moment (for maintenance, service termination, or other reasons).        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | An unknown error occurred (undefined error).            |




* For the entire list of error codes, see the following document.
    - [Error Code](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**

* The error is returned when an error occurs in external authentication library.
* The information on the error in external authentication library is included in the error details, and you can find detailed error code and message as follows.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

For detailed error codes, see the Developer page on each external authentication library.


