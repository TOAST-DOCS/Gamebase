## Game > Gamebase > iOS Developer's Guide > Authentication


## Login

Gamebase supports guest logins by default.

- To log into providers other than guest, a matching Provider AuthAdapter is required.
- For setting of AuthAdapter and 3rd-Party Provider SDK, refer to the following.
    - [3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

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

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. Authenticate with Latest Login Type

* If a previous authentication has been recorded, try to authenticate with no need of ID and password.
* Call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**.

#### 1-1. When Authentication is Successful

* Congratulations! Successfully authenticated
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 1-2. When Authentication is Failed

* Network error
    * If the error code is **TCGB_ERROR_SOCKET_ERROR (110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to a temporary network problem, so call **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]** again or try again in a moment.
* Banned game user
    * If the error code is **TCGB_ERRO_AUTH\_BANNED_MEMBER (3005)**, the authentication has failed due to banned game user.
    * Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
    * When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * Authentication with latest login type has failed. Follow **3. Authenticate with Specified IdP**.

#### 2. Authenticate with Specified IdP

* Try to authenticate by specifying an IdP type.
    * Types that can be authenticated are declared in **TCGBAuthIdPs** of the **TCGBConstants.h** file.
* Call **[TCGBGamebase loginWithType:viewController:completion:]** API.

#### 2-1. When Authentication is Successful

* Congratulations! Successfully authenticated.
* Get a user ID with **[TCGBGamebase userID]** to implement a game logic.

#### 2-2. When Authentication is Failed

* Network error
    * If the error code is **TCGB_ERROR_SOCKET_ERROR (110)** or **TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT (101)**, the authentication has failed due to temporary network problem, so call **[TCGBGamebase loginWithType:viewController:completion:]** again or try again in a moment.
* Banned game user
    * If the error code is **TCGB_ERROR_AUTH_BANNED_MEMBER (3005)**, the authentication has failed due to banned game user.
    * Check ban information with **[TCGBGamebase banInfo]** and notify the user with reasons for not being able to play.
    * When **[TCGBConfiguration enablePopup:YES]** and **[TCGBConfiguration enableBanPopup:YES]** are called during initialization, Gamebase will automatically display a pop-up on banning.
* Other errors
    * Notify that an error has occurred, and return to the state (mostly in title or login screen) in which user can select an authentication IdP type.

### Login as the Latest Login IdP

When a login button is clicked for a specific IdP, following login API will be implemented.<br/>
Try login with the most recently logged-in IdP. If a token is expired
or its authentication fails, return failure.<br/>
Note that a login should be implemented for the IdP.




```objectivec
- (void)automaticLogin {
    [TCGBGamebase loginForLastLoggedInProviderWithViewController:self completion:^(TCGBAuthToken *authToken, TCGBError *error){
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

<br/><br/>
There is information which must be included for login with some IdPs.<br/>
For instance, scope must be set to implement a Facebook login.<br/>
In order to set such necessary information, the **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API is provided.<br/>
You can enter those information to additionalInfo in the dictionary type.<br/>
(When the parameter value is nil, the additionalInfo registered in the NHN Cloud Console will be applied. Generally, the parameter value will take precedence over the value registered in the Console.)


> [Note]
>
> The IdP supported by iOS is defined as **kTCGBAuthXXXXXX** in the area of TCGBAuthIDPs of **TCGBConstants.h**.
>

```objectivec
- (void)loginPaycoButtonClick {
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

#### IdPs supported by Gamebase
Please refer to [Console Guide](./oper-app/#authentication-information).

### Login with Credential

This game interface allows authentication to be made with SDK provided by IdP, before login to Gamebase with provided access token.




* How to Set Credential Parameters



| keyname                                  | Usage                          | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, payco, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set authentication information (access token) received after login IdP |                                |



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

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"Enter the Access Token issued by the Facebook SDK here" };
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
    [TCGBGamebase logoutWithViewController:self completion:^(TCGBError *error) {
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

### Widthdraw API

Below shows an example of how a game user withdraws while logged-in.<br/><br/>

* When a user is successfully withdrawn, the user's data interfaced with a login IdP will be deleted.
* The user can log in with the IdP again, and a new user's data will be created.
* It means user's withdrawal from Gamebase, not from IdP account.
* After a successful withdrawal, a log-out from IdP will be tried.

Following shows an exemplary withdrawal code with a click of the withdraw button.

```objectivec
- (void)authWithdrawal {
    [TCGBGamebase withdrawWithViewController:self completion:^(TCGBError *error) {
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

In many games, one account may have many integrated (mapped) IdPs.<br/>
By using Gamebase Mapping API, other IdP accounts can be integrated or removed to/from another existing IdP account.

In short, a login to a mapped IdP account will be made available with a same user ID at all times.<br/><br/>

Note, however, that each IdP can have only one account to map.<br/>
For instance, if a Google account is mapped, no other Google account can be additionally mapped.<br/>
Below shows an example.<br/><br/>

* Gamebase UserID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
    * Payco ID: dd
* Gamebase UserID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> As the Google ee account is integrated, no additional Google account can be integrated.**

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
* Even if a mapping is successful, 'currently logged-in IdP' will not change. For example, after a user’s login with Gamecenter account and has successfully mapped with a Facebook account, the user's 'currently logged-in IdP' does not change from Gamecenter to Facebook. It still stays with Gamecenter account.
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

Try mapping to another IdP while logged-in to a specific IdP.<br/>

Below is an example of mapping to Facebook.

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
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
            }
        }
    }];
}
```

### AddMapping with Credential

This game interface allows authentication to be made with SDK provided by IdP, before applying Gamebase AddMapping with provided access token.




* How to Set Credential Parameters



| keyname                                  | Usage                          | Value Type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | Set IdP type                      | facebook, payco, iosgamecenter, naver, google, twitter |
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

The following is an example of force mapping to Facebook:

```objectivec
- (void)authAddMapping {
    [TCGBGamebase addMappingWithType:@"facebook" viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            NSLog(@"AddMapping is succeeded.");
        }
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketWithError:error];
            [TCGBGamebase addMappingForciblyWithType:ticket.idPCode forcingMappingKey:ticket.forcingMappingKey viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
            NSLog(@"AddMapping Error - %@", [error description]);
        }
    }];
}
```


### Add Mapping Forcibly with Credential
If there is any account mapped to a specific IdP, try **force** mapping.
When you try **force mapping**, you need `ForcingMappingTicket` obtained from the AddMapping API.

This interface allows you to perform authentication in the game with the SDK provided by IdP first and then to call the Gamebase AddMappingForcibly by using the access token issued.

* How to set the Credential parameter


| keyname                                  | a use                          | Value type                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | IdP type setting                      | facebook, payco, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | Set the authentication information (access token) received after login to IdP |                                           |

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

```objc
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
        else if (error.code == TCGB_ERROR_SOCKET_ERROR || error.code == TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT) {
            NSLog(@"Retry addMapping");
        }
        else if (error.code == TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
            NSLog(@"Already mapped to other member");
            TCGBForcingMappingTicket* ticket = [TCGBForcingMappingTicket forcingMappingTicketWithError:error];
            [TCGBGamebase addMappingWithCredential:credentialInfo forcingMappingKey:ticket.forcingMappingKey viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
                if ([TCGBGamebase isSuccessWithError:error]) {
                    // Mapping success.
                }
                else {
                    // Mapping failed.
                }
            }];
        }
        else {
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
[TCGBGamebase removeMappingWithType:@"facebook" viewController:self completion:^(TCGBError *error) {
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
> To obtain authentication information, log in with "[TCGBGamebase loginWithType:IDP_CODE viewController: self-completion: completion];" API with {IDP_CODE} parameter, which is same as IDPCode to use, instead of "[TCGBGamebase loginForLastLoggedInProvider]".

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

For users who are registered banned in the Gamebase Console,
information codes of restricted use will be displayed as below, when they try to log in. The ban information can be found by using the **[TCGBGamebase banInfo]** method.

* TCGB_ERROR_BANNED_MEMBER




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
There are two types of renewal: **Auto Renew** and **Manual Renew**. You can select either **Renew Password Only** or **Renew Both ID and Password** to renew the TransferAccountInfo information.

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

> `Caution`
> If account transfer is made while logged in to the guest account, the guest account will be lost.

**API**

```objectivec
+ (void)transferAccountWithIdPLoginWithAccountId:(NSString *)accountId accountPassword:(NSString *)accountPassword completion:(void(^)(TCGBAuthToken* authToken, TCGBError* error))completion;
```

**Example**

```objectivec
 - (void)transferOtherDevice {
    [TCGBGamebase transferAccountWithIdPLoginWithAccountId:@"1Aie0198" accountPassword:@"1Aie0199" completion:^(TCGBAuthToken* authToken, TCGBError* error) {
        NSLog(@"Transfered => %@,\nerror => %@", [authToken description], [error description]);
    }];
 }
```


## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | Request for invalid member.                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | Named member has been banned.                               |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | Login is cancelled.                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | The authentication is not supported.                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | Named member does not exist or has withdrawn.                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | Error in external authentication library. <br/> Check DetailCode and DetailMessage. |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | Token login has failed.                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | Invalid token information.                        |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | Invalid last login IdP information.                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdP login has failed.                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP information is invalid. (The IdP information is unavailable in console.) |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | Add mapping has failed.                           |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | Already mapped to another member.                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | Already mapped to same IdP.                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | Invalid IDP information.(IDP information does not exist in the Console.) |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | Mapping with guest IdP is unavailable. |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | Remove mapping has failed.                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | Cannot delete last mapped IDP.                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | Currently logged-in IDP.                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | Logout has failed.                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | Withdrawal has failed.                              |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | Not playable.(due to maintenance or service closed).        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | Unknown error(Undefined error)            |





* Refer to the following document for the entire error codes.
    - [Entire Error Codes](./error-code/#client-sdk)



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


