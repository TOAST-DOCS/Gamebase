## Game > Gamebase > JavaScript SDK使用指南 > 认证

## Login
Gamebase支持访客登录。
若欲使用其他IdP（identity provider，例如Google、Facebook、Line、NAVER、Twitter），请参考”Login with IdP”章节。


### Login Flow
很多游戏在标题界面实现登录。
* 可在游戏初始标题界面选择以何种IdP进行验证。

以上说明的逻辑可按照如下顺序实现。
![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1.以指定的IdP认证

* 直接指定IdP类型尝试认证。
    * 可验证的类型如下。
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * PAYCO('payco')
        * NAVER('naver')
        * Line('line')
* **toast.Gamebase.login(providerName, (authToken, error) => { ...})** 调用API。

#### 1-1.验证成功时

* 祝贺！验证成功。
* 以**toast.Gamebase.getUserID()**获得用户ID实现游戏逻辑即可。

#### 1-2.验证失败时

* 网络错误
    * 错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**时，由于暂时性网络问题导致验证失败，因此重新调用**toast.Gamebase.login(providerName, callback)**或稍后重试。
* 停止使用游戏用户
    * 错误代码为**BANNED_MEMBER(7)**时，为停止使用游戏用户，因此验证失败。
    * 请利用**toast.Gamebase.getBanInfo()**确认禁用信息，告知用户无法玩游戏的原因。
    * 若Gamebase初始化时将**enablePopup**设置为true，则Gamebase自动跳出与停止使用相关的弹出窗口。
* 其他错误
    * 告知游戏用户发生错误，返回至游戏用户可以选择验证IdP类型的状态（主要为标题界面或登录界面）。

### Login with IdP

如下为能够以特定IdP登录的示例代码。

可登录的IdP类型如下。

* Guest('guest')
* Google('google')
* Facebook('facebook')
* PAYCO('payco')
* NAVER('naver')
* Line('line')

> <font color="red">[注意]</font><br/>
>
> Gamebase为进行认证使用“弹窗”。若无法向用户显示“弹窗”，可能一直显示‘’尝试登录’’。
> 通过提前向用户显示“您必须允许显示弹窗”的语句，避免给用户带来不便。
>
> 按照”连接Facebook的浏览器政策‘’， 
> 在IE浏览器使用Facebook Idp登录Gamebase时，一直会强制跳转到Edge浏览器。 
> 因此使用Facebook Idp登录Gamebase时，要通过Chrome、Edge等浏览器进行连接。 
> 但如需在IE浏览器登录Facebook时，
> 则可按以下顺序防止强制跳转到Edge浏览器。
> 1. 进入Edge浏览器设置。
> 2. 在设置界面菜单中选择”默认浏览器”。
> 3. 将Internet Explorer的兼容性更改为”不适用‘’。

```js
toast.Gamebase.login(providerName, (authToken, error) => { ...});
```

**Example**
```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // 表示为由于Socket错误暂时无法访问网络的状态。
                // 请确认网络状态或稍后重试。
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // 尝试登录的用户为停止使用状态。
                // 若以GamebaseConfiguration.uiConfiguration.enablePopup(true)及
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true)初始化，
                // 则Gamebase自动显示与停止使用相关的UI。

                // 若欲按照Game UI直接实现停止使用的弹出窗口，请利用toast.Gamebase.getBanInfo()。
                // 确认禁用信息，为用户标注无法玩游戏的原因。
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // 登录失败
            }

            return;
        }

        // 登录成功
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

### Login with Credential

使用IdP提供的SDK、REST API等在游戏中直接验证，并可利用发放的Access Token等登录Gamebase的接口。

**Credential参数设置方法**

| key name             | a use                                                               |
| -------------------- | ------------------------------------------------------------------- |
| providerName         | 设置IdP类型                                                        |
| accessToken          | 设置登录IdP后获得的验证信息（Access Token）                          |
| accessTokenSecret    | 设置登录IdP后获得的验证信息（Access Token机密）                    |

> [参考]
>
> 游戏中使用外部服务（Facebook等）的固有功能时可能需要。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 必须调用外部SDK API实现外部SDK要求支持的开发事项（Gamebase不支持）。
>

  
    providerName:${IdP},
    acessToken:${IdP AccessToken},
    accessTokenSecret:${IdP AccessTokenSecret}, // This is only nessassary in the case that IdP give you this value.
};

toast.Gamebase.loginWithCredential(credential, callback);
```

**Example**

```js
function loginWithFacebookCredential() {
    var credential = {
        providerName:'facebook',
        accessToken:'EAAL24qLuXHwBAFL1hS72BzZCUrJyDMRxUq1H5ZBGFIT..........',
    };

    toast.Gamebase.loginWithCredential(credential, function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // 表示为由于Socket错误暂时无法访问网络的状态。
                // 请确认网络状态或稍后重试。
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // 尝试登录的用户为停止使用状态。
                // 若以GamebaseConfiguration.uiConfiguration.enablePopup(true)及
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true)初始化，
                // 则Gamebase自动显示与停止使用相关的UI。

                // 若欲按照Game UI直接实现停止使用的弹出窗口，请利用Gamebase.getBanInfo()
                // 确认禁用信息，为用户标注无法玩游戏的原因。
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // 登录失败
            }

            return;
        }

        // 登录成功
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

## Logout

尝试从登录的IdP退出。大多数情况是在游戏的设置界面设置注退出按钮，实现单击按钮执行。
即使退出成功，也会保留游戏用户数据。
若退出成功，则会删除以相应IdP验证的记录，因此下一次登录时显示ID、密码输入窗口。<br/><br/>

如下为单击**退出**按钮后注销的示例代码。

```js
toast.Gamebase.logout((error) => { ...})
```

**Example**
```js
function logout() {
    toast.Gamebase.logout(function (error) {
        if (error) {
            // 退出失败
            console.log(error);
            return;
        }

        // 退出成功
        console.log(data);
    });
}
```

## Withdraw

登录后尝试退出。

* 成功退出时
  * 已登录过的IdP的游戏用户数据将被删除。 
  * 可使用此IdP重新登录。将创建新的游戏用户数据。
  * 用户将从连接的所有IdP注销。 
* 这表示退出Gamebase，而不表示退出IdP账户。

> <font color="red">[注意]</font><br/>
>
> 多个IdP被连接时，所有IdP的连接将解除，而Gamebase用户数据也将被删除。
>

游戏用户登录后退出时，调用退出函数的示例如下。

```js
toast.Gamebase.withdraw(callback) 
```

**Example**
```js
function withdraw() {
    toast.Gamebase.withdraw(function (data, error) {
        if (error) {
            // 注销失败
            console.log(error);
            return;
        }

        // 注销成功
         console.log(data);
    });
}
```

## Gamebase User's Information
通过Gamebase进行验证后，可获得为创建应用程序所需的信息。

### Get Authentication Information for Gamebase
可获取Gamebase提供的验证信息。

```js
// Obtaining Gamebase UserID
var userId =  toast.Gamebase.getUserID();

// Obtaining Gamebase AccessToken
var accessToken = toast.Gamebase.getAccessToken();
```

### Get Banned User Information
在NHN Cloud Gamebase Console中注册为禁用的游戏用户时，
若尝试登录，显示如下限制使用信息代码。可使用**toast.Gamebase.getBanInfo()**方法确认禁用信息。

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)

## TemporaryWithdrawal

为”预约退出”功能。
请求临时退出后无法立即退出，预约时间后会自动退出。
可在控制台中修改预约时间。

> ”注意”
>
> 使用预约退出功能时，不应调用**Gamebase.withdraw()** API。 
> 调用**Gamebase.withdraw()** API，可立即退出账号。

登录成功后，通过调用AuthToken.member.temporaryWithdrawal来判断是否是预约退出的用户。 

### Request TemporaryWithdrawal

请求临时退出。
预约时间后会自动退出。

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

登录使用预约退出功能的游戏时，若AuthToken.member.temporaryWithdrawal不是null，则需提示用户正在进行退出处理。

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

取消退出请求。
如果请求退出后预约时间过期，则无法取消。

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

无论预约时间如何，立即退出。
实际内置运行与Gamebase.withdraw() API相同。

因无法取消”立即退出”，需提示用户是否继续执行。

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
| Auth           | INVALID\_MEMBER                                         | 6          | 无效的用户请求                                            |
|                | BANNED\_MEMBER                                          | 7          | 是禁用用户。                                                      |
|                | AUTH\_USER\_CANCELED                                    | 3001       | 登录信息已被取消 。                                             |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | 是不支持的认证方式 。                                           | 
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | 是不存在或已退出（删除数据）的用户。                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | 未完成上一次的认证程序。                                 |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | 令牌登录失败                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | 无效的令牌信息                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | 无近期登录的IdP信息。             |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdP登录失败                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | 无效的IdP信息（Console中没有此IdP信息。）         |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       |  退出登录失败                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | 退出（删除数据）失败                                                 |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出。                   |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出。                    |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | 是无法玩游戏的状态(维护或已下线等)。                |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | 未知错误(未定义的错误)                      |

* 所有错误代码，请参考以下文档。
    * [Entire Error Codes](./error-code/#client-sdk)
