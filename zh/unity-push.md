## Game > Gamebase > Unity SDK使用指南 > push

以下介绍如何为每个平台设置必要的推送通知。

### Settings


在Android和iOS设置推送的方法请参考以下文档。<br/>

* [Android Push Settings](aos-push#settings)<br/>
* [iOS Push Settings](ios-push#settings)


### Register Push

调用以下API在TOAST Push注册该用户。
接受来自用户的推送协议(enablePush)，广告推送协议(enableAdPush)，夜间广告推送协议(enableAdNightPush)的值，并调用以下API完成注册。


**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback);
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseRequest.Push.NotificationOptions options, GamebaseCallback.ErrorDelegate callback);
```

**示例**

```cs
public void RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    GamebaseRequest.Push.PushConfiguration pushConfiguration = new GamebaseRequest.Push.PushConfiguration();
    pushConfiguration.pushEnabled = pushEnabled;
    pushConfiguration.adAgreement = adAgreement;
    pushConfiguration.adAgreementNight = adAgreementNight;

	// You should receive the above values to the logged-in user.

    Gamebase.Push.RegisterPush(pushConfiguration, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

### Notification Options

* 通过使用Notification Options功能可更改终端机显示的通知类型。 
* 可以在运行时调用registerPush API进行变更。

#### Set Notification Options with RegisterPush in Runtime

调用RegisterPush API时，可以通过添加GamebaseRequest.Push.NotificationOptions参数来设置通知选项。
通过向FGamebaseNotificationOptions生成器传送IGamebase::Get ().GetPush().GetNotificationOptions()的调用结果, 可创建一个用当前通知选项初始化的对象, 只对所需的值进行更改。<br/>
可以设置的值如下。

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | 应用程序为Foreground状态时是否允许推送通知。<br/>**default**: false |
| badgeEnabled           | bool         | 是否使用Badge图标<br/>**default**: true |
| soundEnabled           | bool         | 提示音的使用与否<br/>**default**: true<br/>**iOS Only** |
| priority               | int          | 推送通知的优先顺序/可设置以下5个值。<br/>GamebaseNotificationPriority.MIN : -2<br/> GamebaseNotificationPriority.LOW : -1<br/>GamebaseNotificationPriority.DEFAULT : 0<br/>GamebaseNotificationPriority.HIGH : 1<br/>GamebaseNotificationPriority.MAX : 2<br/>**default**: GamebaseNotificationPriority.HIGH<br/>**Android Only** |
| smallIconName          | string       | 手机顶端状态栏的通知提示小图标文件名称<br/      >如果不设置时，则使用应用程序图标。<br/>**default**: null<br/>**Android Only** |
| soundFileName          | string       | 提示音文件名称/只在Android 8.0以下的OS上运行。<br/>如果指定”res/raw”文件夹的mp3、wav文件名称，则可更改提示音。<br/>**default**: null<br/>**Android Only** |

**Example**

```cs
public void RegisterPushSample(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    var configuration = new GamebaseRequest.Push.PushConfiguration
    {
        pushEnabled = pushEnabled,
        adAgreement = adAgreement,
        adAgreementNight = adAgreementNight,
        displayLanguageCode = GamebaseDisplayLanguageCode.Korean
    };

    var notificationOptions = new GamebaseRequest.Push.NotificationOptions
    {
        foregroundEnabled = true,
        priority = GamebaseNotificationPriority.HIGH
    };

    Gamebase.Push.RegisterPush(configuration, (error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

#### Get NotificationOptions

注册Push时， 返还以前设置的通知选项值。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static GamebaseResponse.Push.NotificationOptions GetNotificationOptions();
```

**Example**

```cs
public void GetNotificationOptionsSample()
{
    GamebaseResponse.Push.NotificationOptions notificationOptions = Gamebase.Push.GetNotificationOptions();

    GamebaseRequest.Push.NotificationOptions options = new GamebaseRequest.Push.NotificationOptions(notificationOptions);
    options.foregroundEnabled = true;
    options.smallIconName = "notification_icon_name";

    GamebaseRequest.Push.PushConfiguration configuration = new GamebaseRequest.Push.PushConfiguration();
    Gamebase.Push.RegisterPush(configuration, options, (error)=> { });
}
```

### Request Push Settings

要查询用户的推送设置，请使用以下API。<br/>
可以通过来自回调的GamebasePushTokenInfo值获取用户设置值。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void QueryTokenInfo(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.TokenInfo> callback);

// Legacy API
public static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback);
```

**示例**

```cs
public void QueryTokenInfoSample(bool isSandbox)
{
    Gamebase.Push.QueryTokenInfo((data, error)=> 
    {
        if (Gamebase.IsSuccess(error)) 
        {
            // Succeeded.
            bool enablePush = data.agreement.pushEnabled;
            bool enableAdPush = data.agreement.adAgreement;
            bool enableAdNightPush = data.agreement.adAgreementNight;
            string token = data.token;
        } 
        else 
        {
        // Failed.
        }
    });
}
```

#### GamebaseResponse.Push.TokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | string                | Push令牌种类       |
| token               | string                | 令牌                 |
| userId              | string                | 用户ID         |
| deviceCountryCode   | string                | 国家代码           |
| timezone            | string                | 标准时区           |
| registeredDateTime  | string                | 令牌更新时间    |
| languageCode        | string                | 语言设置            | 
| agreement           | GamebaseResponse.Push.Agreement | 是否同意接收         |
| sandbox             | bool                  | sandbox与否(iOS Only)        |

#### GamebaseResponse.Push.Agreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 是否允许显示推送通知           |
| adAgreement      | bool | 是否同意接收广告性推送通知      |
| adAgreementNight | bool | 是否允许在夜间显示广告性推送通知  | 


#### Setting for APNS Sandbox
* 在APNS Sandbox环境下，为了正常启动Push功能，需要开启SandboxMode。 
* Push发送方法
    * 在Push菜单的**对象**中选择**iOS Sandbox**后发送。 

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
public static void SetSandboxMode(bool isSandbox)
```

**Example**

```cs
public void SetSandboxModeSample()
{
    Gamebase.Push.SetSandboxMode(true);
}
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Push 库错误。<br>请确认DetailCode。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 上一次的推送API调用未完成。<br>上一次推送 API回调执行后请重新调用。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 未知推送错误。<br>请将全部的Log上传到[客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 全部错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 这是在TOAST Push库中发生的错误。
* 检查错误代码的方法如下。

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* 请确认TOAST Push错误代码。
    * [Android](aos-push#error-handling)<br/>
    * [iOS](ios-push#error-handling)
