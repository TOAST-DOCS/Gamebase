## Game > Gamebase > JavaScript SDK使用指南 > 验证

## Login
Gamebase默认支持访客登录。
若欲使用其他IdP（identity provider，例如Google、Facebook、Line、NAVER、Twitter），请参考”Login with IdP”章节。


### Login Flow
很多游戏在标题界面实现登录。
* 可在游戏初始标题界面选择以何种IdP进行验证。

以上说明的逻辑可按照如下顺序实现。
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)

#### 1.以指定的IdP验证

* 直接指定IdP类型尝试验证。
    * 可验证的类型如下。
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * PAYCO('payco')
        * NAVER('naver')
        * Twitter('twitter')
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
* Twitter('twitter')
* Line('line')

> <font color="red">[注意]</font><br/>
>
> Gamebase中为进行验证，使用’弹出窗口’。若未允许使用‘弹出窗口’，可能会一直停留在正在’尝试登录’的状态。
> 应提前显示’验证前请允许使用弹出窗口’的文字内容，避免给用户带来不便。
>

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

                // 若欲按照Game UI直接实现停止使用的弹出窗口，请利用toast.Gamebase.getBanInfo()
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

使用IdP提供的SDK、REST API等在游戏中直接验证，并可利用发放的访问令牌等登录Gamebase的接口。

**Credential参数设置方法**

| key name             | a use                                                               |
| -------------------- | ------------------------------------------------------------------- |
| providerName         | 设置IdP类型                                                        |
| accessToken          | 设置登录IdP后获得的验证信息（访问令牌）                          |
| accessTokenSecret    | 设置登录IdP后获得的验证信息（访问令牌机密）                    |

> [参考]
>
> 游戏中必须使用外部服务（Facebook等）的固有功能时可能需要。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDK要求支持的开发事项应使用外部SDK的API实现，Gamebase不支持。
>

```js
var credential = {
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

如下为在登录状态下实现游戏用户注销的示例代码。<br/><br/>

* 若注销成功，与登录的IdP关联的游戏用户数据将被删除。
* 可以用相应的IdP重新登录，创建新的游戏用户数据。
* 表示注销Gamebase，但不表示注销IdP账户。
* 注销成功时尝试注退出IdP。

> <font color="red">[注意]</font><br/>
>
> 当多个IdP关联时，所有IdP关联解除，Gamebase游戏用户数据被删除。
>

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
以Gamebase进行验证程序后，可获得制作应用程序时需要的信息。

### Get Authentication Information for Gamebase
可导入Gamebase提供的验证信息。

```js
// Obtaining Gamebase UserID
var userId =  toast.Gamebase.getUserID();

// Obtaining Gamebase AccessToken
var accessToken = toast.Gamebase.getAccessToken();
```

### Get Banned User Information
以禁用的用户注册NHN Cloud Gamebase控制台时，
若尝试登录，显示如下限制使用信息代码。可使用**toast.Gamebase.getBanInfo()**方法确认禁用信息。

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)


## Error Handling

| Category       | Error                                                   | Error Code | Description                                                       |
| -------------- | ------------------------------------------------------- | ---------- | ----------------------------------------------------------------- |
| Auth           | INVALID\_MEMBER                                         | 6          | 为关于无效会员的请求。                                            |
|                | BANNED\_MEMBER                                          | 7          | 为禁用的会员。                                                     |
|                | AUTH\_USER\_CANCELED                                    | 3001       | 登录已取消。                                                |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | 为不支持的验证方式。                                           |
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | 为不存在或注销的会员。                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | 之前的验证流程未完成。                                   |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | 令牌登录失败。                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | 令牌信息无效。                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | 无最近登录的IdP信息。                                      |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdP登录失败。                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | IdP信息无效。（控制台中无相应IdP信息。）           |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       | 退出失败。                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | 注销失败。                                                   |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | 为无法玩游戏的状态（检查或服务结束等）。                        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | 未知错误（未定义的错误）。                             |

* 全部错误代码请参考如下文件。
    * [Entire Error Codes](./error-code/#client-sdk)
