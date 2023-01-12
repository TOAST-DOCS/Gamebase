## Game > Gamebase > iOS SDK 使用指南 > Push

> <font color="red">[注意]</font><br/>
>
> 使用3rd party结算Plug-In或模块（如Unreal或Unity）时，可能会影响Gamebase的结算功能。
>

### Settings

#### 获取APNS JWT认证信息

以下介绍了为推送通知发送而创建 APNS JWT认证信息的过程。

* 请参考[Notification > Push > Console Guide > 获取APNS JWT认证信息](https://docs.toast.com/en/Notification/Push/en/console-guide/#get-authentication-information-for-apns-jwt) 指南获取注册ANPS JWT所需的认证信息。

#### 注册Gamebase Console
* 在**Gamebase > Push > Certificate**的**APNS JWT**中输入上述信息。

#### 实现Notification Service Extension
* 为了搜集“接收指标”或设置提示音，请参考[TOAST Push指南](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-ios/#notification-service-extension)，在应用程序内实现**Notification Service Extension**。

#### Xcode Project设置
* 将**Targets > Capabilities > Push Notifications**项目设置为**ON**。
* 打开自动被创建的.entitlements文件，将**APS Environment**key值设置为适合的值。 
    * **development** : Sandbox APNS
    * **production** : APNS

#### 导入Header文件
以下头文件将导入到要实现Push API的ViewController中。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

调用以下API在TOAST Push注册该用户。<br/>
接受来自用户的推送协议（enablePush），广告推送协议（enableAdPush），夜间广告推送协议（enableAdNightPush）的值，并调用以下API完成注册。

> <font color="red">[注意]</font><br/>
>
> 由于无法确认推送令牌的有效期，建议您在登录后始终调用registerPush API。
>  

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

在TOAST Push中注册用户时，可通过TCGBNotificationOptions对象设置通知选项。<Mb>
通过从用户接收”foreground推送与否(foregroundEnabled)、badge使用与否(badgeEnabled)、提示音使用与否(soundEnabled)”值，可通过调用以下API设置通知选项。 

```objectivec
- (void)didLoginSucceeded {  
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.
    
    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];
    
    TCGBNotificationOptions* options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled badgeEnabled:badgeEnabled soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

#### Setting for APNS Sandbox

在APNS Sandbox环境下，为了正常启动Push功能，需要开启SandboxMode。 

* 客户端SDK设置方法

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* Push发送方法

在Push菜单的**对象**中选择**iOS Sandbox**后发送。 

#### Get NotificationOptions

可以获取注册推送时设置的通知选项值。

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

#### TCGBNotificationOptions

| Parameter             | Values       | Description        |
| --------------------  | ------------ | ------------------ |
| foregroundEnabled     | YES or NO    | 应用程序为foreground状态时是否显示通知。<br/>**default**: NO           |
| badgeEnabled          | YES or NO    | badge图标使用与否<br/>**default**: YES           |
| soundEnabled          | YES or NO    | 提示音使用与否<br/>**default**: YES           |


> [参考]
>
> 可以在运行时更改foregroundEnabled选项。 
> 只能在第一次调用registerPush API时适用badgeEnabled、soundEnabled选项，而不能保证是否能在运行时修改。
>


### Request Push Settings

需要查看用户的推送设置时, 请使用以下API。
使用通过回调返还的TCGBPushTokenInfo值，可以获取Push信息。

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryTokenInfoWithCompletion:^(TCGBPushTokenInfo *tokenInfo, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Token Info Failed.
        }

        NSString *pushType = tokenInfo.pushType;
        NSString *token = tokenInfo.token;
        ...
        // You can handle these variables.
    }];
}
```

#### TCGBPushTokenInfo

| Parameter                              | Values                           | Description                        |
| -------------------------------------- | -------------------------------- | ---------------------------------- |
| pushType                               | string                           | Push令牌类型                       |
| token                                  | string                           | 令牌                                |
| userId                                 | string                           | 用户ID                         |
| deviceCountryCode                      | string                           | 国家代码                            |
| timezone                               | string                           | 标准时区                            |
| registeredDateTime                     | string                           | 令牌更新时间                      |
| languageCode                           | string                           | 语言设置                        |
| sandbox                                | YES or NO                        | 确认是否是在Sandbox环境下注册的令牌。       |
| agreement                              | TCGBPushAgreement                | 是否接收   |

#### TCGBPushAgreement

| Parameter                              | Values                            | Description               |
| -------------------------------------- | --------------------------------- | ------------------------- |
| pushEnabled                            | YES or NO                         | 是否同意提示推送通知           |
| ADAgreement                            | YES or NO                         | 是否同意接收广告性推送通知      |
| ADAgreementNight                       | YES or NO                         | 是否允许在夜间推送广告  |

### Event Handling

* 接收推送消息或点击推送消息时可接收事件。 
* 关于注册事件的方法，请参考GamebaseEventHandler指南。
    * [ Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./ios-etc/#push-received-message)
    * [ Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./ios-etc/#push-click-message)
    * [ Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./ios-etc/#push-click-action)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | 是NHN Cloud Push库错误。<br/>请确认详细错误。 |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 上一次推送API的调用未完成。<br>将上一次推送API回调执行后重新调用。| 
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 未知推送错误<br>请将所有Log上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。 |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* 当在NHN Cloud Push库中发生错误时，返还此错误。 
* 在NHN Cloud Push库发生的错误信息包含在详细错误中，而详细的错误代码和消息如下。

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];
// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


* NHN Cloud Push错误代码如下。
    
| 错误代码 | 说明 |
| --- | --- |
| TCPushErrorNotInitialized | 未初始化。 |
| TCPushErrorInvalidParameters | 参数错误 |
| TCPushErrorPermissionDenined | 未获得权限。 |
| TCPushErrorSystemFail | 系统警报注册失败 |
| TCPushErrorNetworkFail | 网络收发信失败 |
| TCPushErrorServerFail | 服务器响应失败 |
| TCPushErrorInvalidUrl | 无效的URL请求 |
| TCPushErrorNetworkNotReachable | 网络未连接。 |

