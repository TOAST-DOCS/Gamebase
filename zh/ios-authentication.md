## Game > Gamebase > iOS SDK使用指南 > 认证


## Login

Gamebase默认支持Guest登录。

- 使用游客以外的Provider登录，需要Provider AuthAdapter。
- AuthAdapter和第三方提供的SDK设置，请参考以下内容。
    - [Game > Gamebase > iOS SDK使用指南 > 开始 > 3rd-Party Provider SDK Guide](ios-started#3rd-party-provider-sdk-guide)

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

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 按上一次的登录类型认证

* 如果存在已做过的认证的记录，则尝试进行认证，不需要输入ID和密码。
* 调用**[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**。

#### 1-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用**[TCGBGamebase userID]**获取用户ID并实现游戏逻辑。

#### 1-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为TCGB_ERROR_SOCKET_ERROR(110)或TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)。需要重新调用**[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为TCGB_ERROR_BANNED_MEMBER(7)。
    * 请使用**[TCGBBanInfo banInfoFromError:error]**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase时调用**[TCGBConfiguration enablePopup:YES]**和**[TCGBConfiguration enableBanPopup:YES]**Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**'2. 使用指定的IdP进行认证。

#### 2. 使用指定的IdP进行认证

* 通过直接指定IdP类型来尝试进行认证。
    * 可认证的类型在**TCGBConstants.h**文件的**TCGBAuthIdPs**类中声明。
* 调用**[TCGBGamebase loginWithType:viewController:completion:]**API。

#### 2-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用**[TCGBGamebase userID]**获取用户ID并实现游戏逻辑。

#### 2-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**TCGB_ERROR_SOCKET_ERROR(110)**或**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**[TCGBGamebase loginWithType:viewController:completion:]**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**TCGB_ERROR_BANNED_MEMBER(7)**。
    * 请使用**[TCGBBanInfo banInfoFromError:error]**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase时调用**[TCGBConfiguration enablePopup:YES]**和**[TCGBConfiguration enableBanPopup:YES]**，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 通知游戏用户发生了错误，并返回到游戏用户可以选择认证IdP类型的页面（通常是标题页面或登录页面）。

### Login as the Latest Login IdP

点击特定IdP的登录按钮时，将执行以下登录API。<br/>
尝试使用最近登录的IdP登录。如果该登录的令牌已过期，或者令牌认证失败，则返回失败。<br/>
此时，必须实现对该IdP的登录。




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

如果要调用特定的IdP登录，可以调用**[TCGBGamebase loginWithType:viewController:completion:]**方法。<br/>
如果是第一次尝试通过Gamebase登录，或者登录信息（Access Token）已过期，则可以尝试使用此API登录。<br/>
作为登录的结果，可以使用**(TCGBError *)error**对象来确定是否成功。<br/>
您还可以使用**TCGBAuthToken**对象来获取如用ID等用户信息和令牌信息。<br/>
如果登录成功，Gamebase Access Token将存储在Local Storage中，并在调用loginForLastLoggedInProviderWithViewController:completion:方法后，可以应用存储的Access Token。<br/>
但是，IdP的Access Token是由每个IdP提供的SDK管理。<br/>

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

<br/><br/>
个别IdP登录，需要一些特定信息。<br/>
例如，要实现Facebook登录，您需要设置scope等。<br/>
为了设置这些信息，提供了**[TCGBGamebase loginWithType:additionalInfo:viewController:completion:]** API。<br/>
可以用dictionary格式把信息输入到参数additionalInfo中。<br/>
（当参数值为nil时，它将填充在NHN Cloud Console中注册的additionalInfo值。如果参数值存在，则覆盖在Console中注册的值。）

* additionalInfo 파라미터 설정 방법

| keyname                                  | a use                          | 값 종류                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | Line 서비스 제공 지역 중 로그인을 수행할 하나의 지역 | **String**(ex: japan, thailand, taiwan, indonesia)|

```objectivec
- (void)loginLineButtonClick {

    NSDictionary *additionalInfo = @{ kTCGBAuthLoginWithCredentialLineChannelRegionKeyname: @"japan"};

    [TCGBGamebase loginWithType:kTCGBAuthLine additionalInfo:additionalInfo viewController:topViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {

       if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Login Succeeded
            NSString *userId = [authToken.tcgbMember userId];
        } else {
            // To Login Failed
        }
    }];
}
```

> [참고]
>
> Line로그인은 Console에 서비스를 제공할 지역을 복수개 등록할 수 있습니다. IdP로 로그인을 할 때는 additionalInfo 파라미터로 서비스를 제공할 하나의 지역을 직접 입력해야 합니다.
> 


> [参考]
>
> iOS支持的IdP在**TCGBConstants.h**的TCGBAuthIDPs区域中定义为**kTCGBAuthXXXXXX**。
>


#### Gamebase支持的IdP
请参考[控制台使用指南](./oper-app/#authentication-information)。

### Login with Credential

是通过IdP提供的SDK，在游戏中进行认证后，并使用获取到的Access Token，登录到Gamebase的接口。




* Credential参数设置方法



| keyname                                  | a use                          | 值类型                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | 设定IdP类型                     | facebook, iosgamecenter, naver, google, twitter, line, appleid, hangame, weibo, kakaogame |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname |设定登录IdP后收到的认证信息（Access Token）。|                                |
| kTCGBAuthLoginWithCredentialIgnoreAlreadyLoggedInKeyname | Gamebase에 로그인한 상태에서 로그아웃을 하지 않고 다른 계정을 이용해 로그인을 시도하는 것을 허용 | **BOOL** |
|kTCGBAuthLoginWithCredentialLineChannelRegionKeyname | Line 서비스 제공 지역 중 로그인을 수행할 하나의 지역 | **String**(ex: japan, thailand, taiwan, indonesia)|




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
    NSDictionary *credentialDic = @{ kTCGBAuthLoginWithCredentialProviderNameKeyname: kTCGBAuthFacebook, kTCGBAuthLoginWithCredentialAccessTokenKeyname:@"此处输入来自facebook SDK的Access Token" };    

    [TCGBGamebase loginWithCredential:credentialDic viewController:parentViewController completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        NSLog([authToken description]);
    }];
}
```

### Authentication Additional Information Settings

[控制台使用指南](./oper-app/#authentication-information)

## Logout

#### Import Header File

将以下头文件导入到ViewController中来实现退出登录。

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

将以下头文件导入到ViewController中来实现退出（删除数据）。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Withdraw API

在登录状态下尝试退出。

* 成功退出时
  * 登录IdP的游戏用户数据将会被删除。
  * 可通过相关IdP重新登录。将创建新的游戏用户数据。
  * 所有连接的IDP都将被注销。
* 表示退出Gamebase，而不表示退出IdP账户。 

> <font color="red">[注意]</font><br/>
> 
> 连接多个IdP时，所有IdP的连接将被解除，Gamebase用户数据也将被删除。 
>

以下是点击“退出”按钮时，将删除数据的示例代码。

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

映射（Mapping）是将已登录的现有游戏帐号和IdP帐户关联或解除关联的功能。

在大多数游戏中，可以将多个IdP帐户映射（Mapping）到同一个游戏账户。<br/>Gamebase的Mapping API，允许将您的其他IdP帐户和您之前登录的游戏帐户进行关联或解除关联操作。

换句话说，当您尝试使用同步的IdP帐户登录时，可以始终使用相同的用户ID登录。<br/> <br/>

请注意，每个IdP只能关联一个同域名帐户。<br/>
例如，如果您已经关联了Google帐户，则无法添加其他Google帐户。<br/>
以下是帐户关联的示例。<br/><br/>

* Gamebase用户 ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * AppleGameCenter ID: cc
* Gamebase用户 ID: 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

Mapping API中有添加映射和解除映射的功能。

### Add Mapping Flow

映射（Mapping）可以按以下顺序实现。

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. 登录
Mapping是为当前帐户添加IdP帐户链接，因此您必须先登录。
首先通过调用登录API登录。

#### 2. 映射（Mapping）                   

调用**[TCGBGamebase addMappingWithType:viewController:completion:]**尝试进行映射（Mapping）。

#### 2-1. 如果映射（Mapping）成功
* 恭喜！ 已成功添加与当前账户关联的IdP帐户。
*    映射（Mapping）成功后，“当前登录的IdP”也不会更改。 换句话说，在已使用Gamecenter帐户登录后，成功映射（Mapping）到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Gamecenter的状态。
    * <font color="red">[注意]</font><br/> : Guest账户是个例外。若使用Guest账户登录后映射成功，Guest IdP将被**删除**，而“当前登录的IdP”也将更改为映射的IdP。
* 映射（Mapping）只是添加IdP关联。

#### 2-2. 如果映射（Mapping）失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**TCGB_ERROR_SOCKET_ERROR(110)**和**TCGB_ERROR_SOCKET_RESPONSE_TIMEOUT(101)**，则重新调用**[TCGBGamebase addMappingWithType:viewController:completion:]**或稍后再试。
* 与其他帐户关联时发生的错误
    * 错误代码为**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**，则表示尝试映射（Mapping）的IdP上的帐户已关联了另一个帐户。请使用已获取的**ForcingMappingTicket**强制映射(**[TCGBGamebase addMappingForciblyWithTicket:viewController:completion:]**)或在尝试更改登录账号(**[TCGBGamebase changeLoginWithForcingMappingTicket:viewController:completion:]**)。
* 关联相同IdP帐户发生的错误
	* 错误代码为**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**，则表示已关联了与IDP相同类型的账户。
	* Gamebase映射（Mapping），每个IdP只能关联一个游戏帐户。例如，已经关联了Google帐户，则无法添加其他Google帐户。
	* 要关联同一IdP中的另一个帐户，请调用**[TCGBGamebase removeMappingWithType:viewController:completion:]**，解除关联后，再尝试映射（Mapping）。
* 其他错误                            
    * 尝试映射（Mapping）失败。              

### Import Header file into ViewController

将以下头文件导入到ViewController中，来实现映射（Mapping）。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Add Mapping API

在登录特定IdP状态下，尝试用其他IdP Mapping。<br/>

**API**

```objectivec
+ (void)addMappingWithType:(NSString *)type viewController:(UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

如下为对Facebook尝试强制映射的范例。

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

游戏中直接使用IdP提供的SDK进行认证后，使用发行的Access Token，进行Gamebase AddMapping的接口。




* Credential参数设置方法



| keyname                                  | a use                          | 值类型                           |
| ---------------------------------------- | ------------------------------ | ------------------------------ |
| kTCGBAuthLoginWithCredentialProviderNameKeyname | 设定IdP类型                     | facebook, iosgamecenter, naver, google, twitter, line, appleid |
| kTCGBAuthLoginWithCredentialAccessTokenKeyname | 设定登录IdP后收到的认证信息（Access Token）。|                                |




> [参考]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
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
若特定IdP有已映射的账户，尝试**强制**映射。
尝试**强制映射**时需要从AddMapping API获得的”ForcingMappingTicket”。

**API**

```objectivec
+ (void)addMappingForciblyWithTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

如下为对Facebook尝试强制映射的范例。

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

若特定IdP有已映射的账户，尝试**更改登录账号**。
尝试**更改登录账号**时需要从AddMapping API获得的“ForcingMappingTicket”。

若调用Change Login API失败，上一个账号保持登录状态。

**API**

```objectivec
+ (void)changeLoginWithForcingMappingTicket:(TCGBForcingMappingTicket *)ticket viewController:(nullable UIViewController *)viewController completion:(LoginCompletion)completion;
```

**Example**

如下为对Facebook尝试强制映射的范例。

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
                    // Change login successed.
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

解除特定IdP的关联。 <br/>
如果您要解除关联的IdP是**唯一的IdP**，则会返还失败。<br/>
解除关联之后，Gamebase将该IdP退出登录。

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
可以查看当前帐户映射（Mapping）到各IdP的列表。

```objectivec
// Obtaining Names of Mapping IdPs
NSArray* authMappingList = [TCGBGamebase authMappingList];
```


## Gamebase Users`s Information
在使用Gamebase完成认证过程后，制作App时可获取您所需要的信息。

> <font color="red">[注意]</font><br/>
>
> 如果您使用“[TCGBGamebase loginForLastLoggedInProvider]”API登录，则无法获取认证信息。
>
> 如果需要认证信息，使用IDPCode和同一个{IDP_CODE}作为参数（不是使用"[TCGBGamebase loginForLastLoggedInProvider]"），调用"[TCGBGamebase loginWithType:IDP_CODE viewController:topViewController completion:completion];" API登录，才可获取正确的认证信息。

### Get Authentication Information for Gamebase
可以获取Gamebase发行的认证信息。

```objectivec
// Obtaining Gamebase UserID
NSString* gamebaseUserID = [TCGBGamebase userID];

// Obtaining Gamebase AccessToken
NSString* gamebaseAccessToken = [TCGBGamebase accessToken];

// Obtaining Last Logged In Provider
NSString* lastProviderName = [TCGBGamebase lastLoggedInProvider];
```

### Get Authentication Information for External IdP

* 登录后，通过游戏服务器调用Gamebase Server API，可获取外部认证IdP的Access Token、用户ID及Profile等信息。
    * [Game > Gamebase > API指南 > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[注意]</font><br/>
>   
> * 为了安全起见，建议通过游戏服务器调用外部IdP的认证信息。
> * 根据IdP访问令牌类型，可能会很快过期。
>     * 例如，登录Google过2小时后，Access Token将会过期。
>     * 如果您需要用户信息，登录后，请直接调用Gamebase Server API。
> * 如果调用"[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]" API登录，则无法接收认证信息。
>     * 如需用户信息，需要通过与IDPCode相同的{IDP_CODE}作为参数，调用"[TCGBGamebase loginWithType:viewController:completion:]" API登录，而不调用"[TCGBGamebase loginForLastLoggedInProviderWithViewController:completion:]"。

> <font color="red">[注意]</font><br/>
>
> 在iOS 12上使用appleid登录时，无法查看认证信息。
>

### Get Banned User Information

如果在Gamebase Console中登记为受到制裁的游戏用户，
当该用户尝试登录时，可能会看到以下限制信息代码。您可以使用**[TCGBBanInfo banInfoFromError:error]**方法，确认制裁信息。

* TCGB_ERROR_BANNED_MEMBER

## TransferAccount
获得将访客账户转移至其他终端机密钥的功能。

此密钥称为**TransferAccountInfo**。
获得的TransferAccountInfo可从其他终端机调用**requestTransferAccount** API，并转移账户。

> <font color="red">[注意]</font><br/>
> 
> TransferAccountInfo仅在访客登录状态下可获得。
> 使用TransferAccountInfo的账户转移仅可在访客登录状态或未登录状态下实现。
> 若登录的访客账户已与其他外部IdP(Google、Facebook等) 映射，则不支持账户转移。

### Issue TransferAccount
为了转移访客账户，发放TransferAccountInfo。

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
为了转移访客账户，向Gamebase服务器查询已获得的TransferAccountInfo信息。

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
更新已获得的TransferAccountInfo信息。
更新方法有**自动更新**与**手动更新**，可选择**仅更新密码**、**同时更新ID与密码**更新TransferAccountInfo信息。

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
向从**issueTransfer** API获得的TransferAccount转移账户的功能。
账户转移成功时获得TransferAccount的终端机可显示转移完成信息，访客登录时创建新的账户。
在账户转移成功的终端机中可继续使用获得TransferAccount的终端机的账户信息。

> <font color="red">[注意]</font><br/>
> 
> 若以访客登录的状态转移账户，访客账户将丢失。
> 若连续输入错误的id/password，将发生**AUTH_TRANSFERACCOUNT_BLOCK(3042)**错误，账户被暂时禁用。 
> 如果出现上述错误，如下所示，可通过调用TCGBTransferAccountFailInfo值通知用户账户转移将被阻止多长时间。 


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

为“预约退出‘’功能。
由于请求了临时退出，不立即退出，预约时期过后退出。
可以在控制台中修改预约时期。

> <font color="red">[注意]</font><br/>
>
> 如果使用预约退出功能，不应调用**[TCGBGamebase withdrawWithViewController:completion:]** API。
> 通过调用**[TCGBGamebase withdrawWithViewController:completion:]** API，可立即退出。

如果登录成功，您可通过调用AuthToken.getTemporaryWithdrawalInfo() API，确认用户是否在预约退出状态。


### Request TemporaryWithdrawal

请求临时退出。
在控制台中指定的预约时间过后，自动完成退出处理。

**API**

```objectivec
+ (void)withdrawWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | 用户已临时退出。|

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

登录使用预约退出的游戏后始终使用**TCGBAuthToken.tcgbMember.temporaryWithdrawal**，若返还有效的TemporaryWithdrawalInfo对象，而不返还null，则需通知相关用户正在进行退出处理。

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

取消退出请求。
若预约退出时间过期，则无法退出。 

**API**

```objectivec
+ (void)cancelTemporaryWithdrawalWithViewController:(UIViewController *)viewController completion:(WithdrawCompletion)completion;
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) |  用户未临时退出。 |

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

无论预约退出时期的设置状态如何，都立即退出。
实际的内置启动与**[TCGBGamebase withdrawWithViewController:completion:]** API相同。

无法取消立即退出，因此要反复提示用户是否继续执行。

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

* 是“结算Abusing自动解除”功能。
    * 结算Abusing自动解除功能是当存在需通过“结算Abusing自动制裁”来禁止用户使用时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为“预约禁用状态”，在设定的时期内满足解除条件，则可玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认TCGBAuthToken.tcgbMember.graceBanInfo值，如果返还有效的TCGBGraceBanInfo对象，而不返还null，要向相关用户通知解除禁用的条件、时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。   

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
| Auth           | TCGB\_ERROR\_INVALID\_MEMBER             | 6          | 请求了错误的成员。                        |
|                | TCGB\_ERROR\_BANNED\_MEMBER              | 7          | 此成员已被禁用。                            |
|                | TCGB\_ERROR\_AUTH\_USER\_CANCELED        | 3001       | 已取消登录。                            |
|                | TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 是不支持的认证方式。                        |
|                | TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER    | 3003       | 是不存在或已退出的成员。                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR    | 3006       |  第三方认证库初始化失败                      |
|                | TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 第三方认证库出现错误。<br/> 请确认DetailCode和DetailMessage。  |
|                | TCGB\_ERROR\_AUTH\_INVALID\_GAMEBASE\_TOKEN | 3011       | 由于Gamebase Access Token无效已注销。<br/>请稍后重新登录。 |
| Auth (Login)   | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED  | 3101       | 令牌登录失败                          |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 是无效的令牌信息。                      |
|                | TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近登录的IdP信息不存在。                   |
| IdP Login      | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED    | 3201       | IdP登录失败                        |
|                | TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | 是无效的IdP信息（在Console中不存在该IdP信息）。 |
| Add Mapping    | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED  | 3301       | 添加映射失败                          |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 已关联了其他成员。                      |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 已关联了相同类型的IdP。                     |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | 是无效的IdP信息 (在Console中不存在该IdP信息)。 |
|                | TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305  | 用客户IdP无法进行AddMapping。|
| Remove Mapping | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 映射删除失败                           |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 最后关联的IdP无法删除。                |
|                | TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 是已登录的IdP。                     |
| Logout         | TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED        | 3501       | 注销失败                            |
| Withdrawal     | TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED      | 3601       | 退出失败                              |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出。                    |
|                | TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出。                     |
| Not Playable   | TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE         | 3701       | 无法进入游戏 （维护或终止服务等）。        |
| Auth(Unknown)  | TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR        | 3999       | 发生未知错误 (出现未定义错误)。            |

* 有关所有的错误代码，请参考如下文件。
    - [错误代码](./error-code/#client-sdk)


**TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR**

* 在各IdP的SDK中发生此错误。
* 确认错误代码方式如下。

* 关于IdP SDK的错误代码，请参考相应Developer页面。

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```
