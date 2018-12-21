## Game > Gamebase > iOS SDK 使用指南 > 认证


## Login

Gamebase默认支持Guest登录。

- 使用游客以外的Provider登录，需要Provider AuthAdapter。
- AuthAdapter和第三方提供的SDK设置，请参考以下内容。
    - [第三方提供的SDK指南](ios-started#3rd-party-provider-sdk-guide)

根据尝试登录的IdP，可能需要输入additionalInfo参数。<br/>
有关AdditionalInfo的说明，请参考以下**Gamebase支持的IdP说明**。


### Import Header File

在ViewController中，导入要实现登录的以下头文件。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Login Flow

多数游戏在标题页上实现登录。
* 当App首次安装和启动时，游戏用户可以在标题页选择要进行的IdP(identity provider)类型。
* 登录过一次后，您将不会看到IdP选择画面，将使用之前登录的IdP类型进行认证。

上述逻辑可以按以下顺序实现。

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. 按上一次的登录类型认证

* 如果存在已做过的认证的记录，则尝试进行认证，不需要输入ID和密码。
* 调用 **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**。

#### 1-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用 **[TCGBGamebase userID]** 获取用户ID并实现游戏逻辑。

#### 1-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**TCGB_ERROR_SOCKET_ERROR(110)**或**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用 **[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**，或稍后再试。
* 禁用游戏用户。
    * 由于用户是禁用状态，认证失败，错误代码为**TCGB_ERROR_BANNED_MEMBER(7)**。
* 请使用 **[TCGBGamebase banInfo]** 确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时调用 **[TCGBConfiguration enablePopup:YES]** 和 **[TCGBConfiguration enableBanPopup:YES]** ，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**3. 使用指定的IdP进行认证**。

#### 2. 使用指定的IdP进行认证

* 通过直接指定IdP类型来尝试进行认证。
    * 可认证的类型在**TCGBConstants.h**文件的**TCGBAuthIdPs**类中声明。
* 调用 **[TCGBGamebase loginWithType:viewController:completion:]** API。

#### 2-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用 **[TCGBGamebase userID]** 获取用户ID并实现游戏逻辑。

#### 2-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**TCGB_ERROR_SOCKET_ERROR(110)**或**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**，需要重新调用 **[TCGBGamebase loginWithType:viewController:completion:]**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**TCGB_ERROR_BANNED_MEMBER(7)**。
    * 请使用 **[TCGBGamebase banInfo]**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时调用 **[TCGBConfiguration enablePopup:YES]**和 **[TCGBConfiguration enableBanPopup:YES]**，Gamebase会自动弹出禁用窗口。
* 其他错误
    * 通知游戏用户发生了错误，并返回到游戏用户可以选择认证IdP类型的页面（通常是标题页面或登录页面）。

### Login as the Latest Login IdP

点击特定IdP的登录按钮时，将执行以下登录API。<br/>
尝试使用最近登录的IdP登录。如果该登录的令牌已过期，或者令牌认证失败，则返回失败。<br/>
此时，须实现对该IdP的登录。




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

如果要调用特定的IdP登录，可以调用 **[TCGBGamebase loginWithType:viewController:completion:]**方法。<br/>
如果是第一次尝试通过Gamebase登录，或者登录信息（访问令牌）已过期，则可以尝试使用此API登录。<br/>
作为登录的结果，可以使用 **(TCGBError *)error**对象来确定是否成功。<br/>
您还可以使用**TCGBAuthToken**对象来获取如用ID等用户信息和令牌信息。<br/>
如果登录成功，Gamebase访问令牌将存储在Local Storage中，并在调用loginForLastLoggedInProviderWithViewController:completion:方法后，可以应用存储的访问令牌。<br/>
但是，IdP的访问令牌是由每个IdP提供的SDK管理。<br/>

<br/><br/>
个别IdP登录，需要一些特定信息。<br/>
例如，要实现Facebook登录，您需要设置scope等。<br/>
为了设置这些信息，提供了 **[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API。<br/>
可以用dictionary格式把信息输入到参数additionalInfo中。<br/>
（当参数值为nil时，它将填充在TOAST Console中注册的additionalInfo值。如果参数值存在，则覆盖在Console中注册的值。）


> [参考]
>
> iOS支持的IdP在**TCGBConstants.h**的TCGBAuthIDPs区域中定义为**kTCGBAuthXXXXXX**。
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

#### Gamebase支持的IdP
请参考[控制台使用指南](./oper-app/#authentication-information)。

### Login with Credential

是通过IdP提供的SDK在游戏中进行认证后，并使用获取到的访问令牌，登录到Gamebase的接口。




* Credential 参数设置方法



| keyname                                  | a use                          | 值类型                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | 设定IdP类型                     | facebook, payco, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname |设置登录IdP后收到的认证信息（访问令牌） |                                |



> [参考]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
>

```objectivec
#import "TCGBConstants.h"

- (void)authLoginWithCredential {
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: @"facebook", kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"此处输入来自 facebook SDK的Access Token" };
    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

### Authentication Additional Information Settings

[控制台使用指南](./oper-app/#authentication-information)

## Logout

#### Import Header File

将以下头文件导入到ViewController中，来实现退出登录。

```objectivec
#import <Gamebase/Gamebase.h>
```

#### Logout API

尝试从登录中的IdP退出。 通常，在游戏的设置画面有退出登录（退出账号）按钮，然后点击该按钮执行。
即使退出登录成功，也会保留游戏用户数据。
如果退出登录成功，将会删除IDP认证记录，则下次登录时将显示ID和密码输入窗口。<br/> <br/>

以下是点击“退出登录”按钮时的示例代码。

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

将以下头文件导入到ViewController中，来实现退出（删除数据）。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Withdraw API

以下是游戏用户登录状态下，“退出（删除数据）”的示例代码。<br/><br/>

* 如果退出（删除数据）成功，则将删除与登录的IdP账户相关联的游戏用户数据。
* 您可以使用该IdP重新登录并生成新的游戏用户数据。
* 这意味着退出Gamebase，并不是退出IdP帐户。
* 成功退出（删除数据）时，也将退出IdP登录。

以下是点击“退出”按钮时，将删除数据的示例代码。

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

映射（Mapping）是将已登录的现有游戏帐号和IdP帐户关联或解除关联的功能。

在大多数游戏中，可以将多个IdP帐户映射（Mapping）到同一个游戏账户。<br/>Gamebase的Mapping API，允许将您的其他IdP帐户和您之前登录的游戏帐户进行关联或解除关联操作。

换句话说，当您尝试使用同步的IdP帐户登录时，可以始终使用相同的用户ID登录。<br/> <br/>

请注意，每个IdP只能关联一个同域名帐户。<br/>
例如，如果您已经关联了Google帐户，则无法添加其他Google帐户。<br/>
以下是帐户关联的示例。<br/><br/>

* Gamebase 用户 ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
    * Payco ID: dd
* Gamebase 用户 ID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

Mapping API中有添加映射和解除映射的功能。

### Add Mapping Flow

映射（Mapping）可以按以下顺序实现。

#### 1. 登录
Mapping是为当前帐户添加IdP帐户链接，因此您必须先登录。
首先通过调用登录API登录。

#### 2. 映射（Mapping）

调用 **[TCGBGamebase addMappingWithType:viewController:completion:]** 尝试进行映射（Mapping）。

#### 2-1. 如果映射（Mapping）成功

* 恭喜！ 已成功添加与当前账户关联的IdP帐户。
* 映射（Mapping）成功后，“当前登录的IdP”也不会更改。 换句话说，在已使用Google帐户登录后，成功映射（Mapping）到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
* 映射（Mapping）只是添加IdP关联。

#### 2-2. 如果映射（Mapping）失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**TCGB_ERROR_SOCKET_ERROR(110)**或**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**，则重新调用 **[TCGBGamebase addMappingWithType:viewController:completion:]**或稍后再试。
* 与其他帐户关联时发生的错误
    *错误代码为**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**，则表示尝试映射（Mapping）的IdP上的帐户已关联了另一个帐户，要解除已关联的帐户，请使用该帐户登录并通过调用 **[TCGBGamebase withdrawWithViewController:completion:]** 退出（删除数据），或者通过调用 **[TCGBGamebase removeMappingWithType:viewController:completion:]** 解除关联，再尝试映射（Mapping）。
* 关联相同IdP帐户发生的错误
	* 错误代码为**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**，则表示已关联了与IDP相同类型的账户。
	* Gamebase映射（Mapping），每个IdP只能关联一个游戏帐户。例如，已经关联了PAYCO帐户，则无法添加其他PAYCO帐户。
	* 要关联同一IdP中的另一个帐户，请调用 **[TCGBGamebase removeMappingWithType:viewController:completion:]**，解除关联后，再尝试映射（Mapping）。
* 其他错误
    * 尝试映射（Mapping）失败。

### Import Header file into View Controller

将以下头文件导入到ViewController中，来实现映射（Mapping）。

```objectivec
#import <Gamebase/Gamebase.h>
```



### Add Mapping API

在登录特定IdP状态下，尝试用其他IdP Mapping。<br/>

以下是尝试映射（Mapping）到Facebook的示例。

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

游戏中直接使用ID Provider提供的SDK进行认证后，使用发行的访问令牌，进行GameBase AddMapping的接口。




* Credential 参数设置方法



| keyname                                  | a use                          | 值类型                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | 设定IdP类型                     | facebook, payco, iosgamecenter, naver, google, twitter |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | 设置登录IdP后收到的认证信息（访问令牌） |                                |



> [参考]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
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

解除特定IdP的关联。 <br/>
如果您要解除关联的IdP是 **唯一的IdP**，则会返回失败。<br/>
解除关联之后，Gamebase将该IdP退出登录。

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
可以查看当前帐户映射（Mapping）到哪些IdP的列表。

```objectivec
// Obtaining Names of Mapping IDPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase Users`s Information
在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

> <font color="red">[注意]</font><br/>
>
> 如果您使用“[TCGBGamebase loginForLastLoggedInProvider]”API登录，则无法获取认证信息。
>
> 如果需要认证信息，代替“[TCGBGamebase loginForLastLoggedInProvider]”使用IDPCode和同一个{IDP_CODE}作为参数，来使用“[TCGBGamebase loginWithType:IDP_CODE viewController:self completion:completion];”API登录，才可以获取到正常的认证信息。

### Get Authentication Information for Gamebase
可以获取Gamebase发行的认证信息。

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

从外部认证SDK，可获取访问令牌、用户ID、Profile等信息。

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

如果在Gamebase Console中登记为受到制裁的游戏用户，当该用户尝试登录时，
可能会看到以下限制信息代码。您可以使用 **[TCGBGamebase banInfo]** 方法，确认制裁信息。

* TCGB_ERROR_BANNED_MEMBER


## TransferKey
获取密钥以将访客帐户转移到其他设备的功能。
此密匙称为**TransferKey**。
获取的TransferKey可以通过从另一台设备调用**requestTransfer**API来转移帐户。


> `注意`
> 只有在游客登录方式时，才能发放TransferKey。
> 只有游客登录或未登录的状态，才能使用TransferKey转移帐户。
> 如果您登录的游客帐户已映射（Mapping）到其他外部IdP（Google，Facebook，Payco等），则不支持转移帐户。



### Issue TransferKey
发放TransferKey以转移游客帐户。
TransferKey的格式为**包含“小写/大写/数字”的8位数的字符串**。
同时，发行发放时间和到期时间，格式为epoch time。
* 参考：https://www.epochconverter.com/

```objectivec
- (void)issueTransferKeyToTransfer {
    [TCGBGamebase issueTransferKeyWithExpiresIn:3600 completion:^(TCGBTransferKeyInfo *transferKey, TCGBError *error){
        if ([TCGBGamebase isSuccessWithError:error] == YES){}
            NSLog(@"Published TransferKey => %@", [transferKey description]);
            NSString* transferKeyString = [transferKey transferKey];
            long regDat = [transferKey legDate];
            long expireDate = [transferKey expireDate];
        }
        else {
            // Handling Errors.
            NSLog(@"Issue TranferKey Occur Error => %@", [error description]);
        }
    }];
}
```

### Transfer Guest Account to Another Device
通过**issueTransfer** API发放的TransferKey转移帐户的功能。
在成功转移帐户后，在TransferKey的原设备上显示转移完成的消息，并且用在游客登录时将创建新帐户。
在成功转移帐户的新设备上，您可以继续使用原游客帐户。

> `注意`
> 如果在游客登录状态下转移成功，原设备中登录过的游客信息将丢失。

```objectivec
- (void)transferGuestAccount {
    [TCGBGamebase requestTransferWithTransferKey:@"1i9eiAi7" completion:^(TCGBAuthToken *token, TCGBError *error)( {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // TCGBAuthToken 对象相当于调用 `loginWithType:viewController:completion:` 方法
            NSLog(@"Transfer Information > %@", [token description]);
            NSString* userID = token.tcgbMember.userId;
            NSArray* authList = token.tcgbMember.authList;
        }
        else {
            // Handling Errors.
            NSLog(@"Transfer Occur Error => %@", [error description]);
        }
    });
}
```


## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | 无效的用户请求。                       |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | 被制裁的用户。                             |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | 登录信息已被取消。                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 不支持的认证方式。                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | 不存在或已退出（删除数据）的用户。                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 外部认证库错误。<br/>请确认 DetailCode和DetailMessage。  |
| TransferKey    | TCGB\_ERROR\_SAME\_REQUESTOR             | 8			 | 在同一台设备上使用了相同的TransferKey。 |
|                | TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9          | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。 |
|				 | TCGB\_ERROR\_IOS\_GAMECENTER\_DENIED     | 51         | GameCenter登录被拒绝。<br/>连续三次以上，在游戏内的GameCenter登录画面取消登录后，再尝试登录时，登录画面可能不再显示。|
|                | TCGB\_ERROR\_AUTH\_TRANSFERKEY\_EXPIRED  | 3031       | TransferKey已过期。|
|                | TCGB\_ERROR\_AUTH\_TRANSFERKEY\_CONSUMED | 3032       | TransferKey已使用。 |
|                | TCGB\_ERROR\_AUTH\_TRANSFERKEY\_NOT\_EXIST | 3033     | TransferKey无效。 |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | 令牌登录失败。                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 无效的令牌信息。                       |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 无近期登录的IdP信息。                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdP登录失败。                       |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | 无效的IdP信息（Console中没有此IdP信息）。 |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | 添加映射（Mapping）失败。                           |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已经与其他帐户映射（Mapping）。                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 已映射（Mapping）到相同的IdP                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | 无效的IdP信息（Console中没有此IdP信息）。 |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | 游客IDP无法进行AddMapping。 |
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 解除映射（Mapping）失败。                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 无法解除最后映射（Mapping）的IdP。                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 当前登录的IdP。                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | 退出登录失败。                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | 退出（删除数据）失败。                              |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | 无法玩游戏的状态（维护或已下线等）。        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | 未知错误(未定义的错误)。            |




* 所有错误代码，请参考以下文档。
    - [错误代码](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**
* 该错误为各IdP的SDK中发生的错误。
* 确认错误代码方式如下。

* IdP SDK的错误代码，请参考相应Developer页面。

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


