## Game > Gamebase > iOS SDK 使用指南 > Push

### 设置

#### Apple Developer证书

以下介绍了为推送通知发送而创建Apple Developer证书的过程。

* 在[Apple Developer网站](https://developer.apple.com)的 **Add iOS Certificate**上利用 **Apple Push Notification service SSL**创建证书。
* 注册Keychain后，以 Personal Information Exchange (.p12) 格式导出生成的证书。
* 导出(export)证书时设置密码。

#### TOAST Console 登记
* **Notification > Push > Certificate**中注册 **APNS Certificate**和 **APNS (Sandbox) Certificate**中生成的证书。
* 使用制作上述认证书时设定的密码进行登记。

#### XCode Project 设置
* 请将**Targets > Capabilities > Push Notifications **项目设置为**ON**。
* 打开自动生成的.entitlements文件，并将**APS Environment**键的值设置为适当的值。
    * **development**: Sandbox APNS
    * **production**:  APNS

#### 导入Header文件
以下头文件将导入到要实现Push API的ViewController中。

```objectivec
#import <Gamebase/Gamebase.h>
```

### 注册Push

调用以下API在TOAST Push注册该用户。<br/>
接受来自用户的推送协议（enablePush），广告推送协议（enableAdPush），夜间广告推送协议（enableAdNightPush）的值，并调用以下API完成注册。


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


#### APNS Sandbox设定

当打开SandboxMode时，可以登记将Push发送到APNS Sandbox。
* 客户端设置方法

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* 发送控制台的方法
从推送菜单中的**对象**中选择**iOS Sandbox**并发送。

### 请求Push设定

要查询用户的推送设置，请使用以下API。<br/>
可以通过来自回调的PushConfiguration值获取用户设置值。

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryPushWithCompletion:^(TCGBPushConfiguration *configuration, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Configuration Failed.
        }

        BOOL enablePush = configuration.pushEnabled;
        BOOL enableAdPush = configuration.ADAgreement;
        BOOL enableAdNightPush = configuration.ADAgreementNight;

        // You can handle these variables.
    }];
}
```

### Error处理

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | TOAST Push库错误。<br>请确认DetailCode。 |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 上一次的推送API调用未完成。<br>上一次的推送 API回调执行后请重新调用。|
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 未知推送错误。<br>请将全部的Log上传到[客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* 这是在TOAST Push库中发生的错误。
* 检查错误代码的方法如下。

```objectivec
TCGBError *tcgbError = error; // 转到Callback的TCGBError实例
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 来自外部库的错误对象
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 您可以通过调用以下 [tcgbError description] 获取 json format的所有错误信息。
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* TOAST Push错误代码，请参考以下文档。  
    * [Notification > Push > SDK v1.4 使用指南 > 错误处理](/Notification/Push/ko/sdk-guide-ios/#_11)

