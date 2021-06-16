## Game > Gamebase > Unreal SDK使用指南 > Push

下面将了解如何为每个平台设置必要的推送通知。

### Settings

关于在Android和iOS设置推送的方法，请参考以下文档。<br/>

* [Android Push Settings](aos-push#settings)
    * 如需使用Firebase推送，请参考以下指南。 
        * 如果使用Unity build，必须对Unreal适用同样的设置。
            * Android打包时，必须包含res/values/google-services-json.xml文件，因此请参考以下指南。
            * 在Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml路径中存在文件。 
* [iOS Push Settings](ios-push#settings)

### Register Push

通过调用以下API，在TOAST Push中注册相关用户。  
从用户获得”是否允许显示推送通知(enablePush)‘’、”是否同意接收广告性推送通知(enableAdPush)”、”是否允许在夜间显示广告性推送通知(enableAdNightPush)”的同意，通过调用以下API进行注册。 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseErrorDelegate& onCallback);
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseNotificationOptions& notificationOptions, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight };
    
    IGamebase::Get().GetPush().RegisterPush(FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

### Notification Options

* 可以通过使用Notification Options功能更改终端机显示的推送通知类型。 
* 在运行时可通过调用RegisterPush API来进行更改。

#### Set Notification Options with RegisterPush in Runtime

调用RegisterPush API时， 可以通过添加FGamebaseNotificationOptions参数来设置通知选项值。
当向FGamebaseNotificationOptions的生成器传送IGamebase::Get ().GetPush().GetNotificationOptions()的调用结果时，创建用当前通知选项初始化的对象, 因此可以更改需要更改的值。<br/>
可以设置的值如下。

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | 应用程序为Foreground状态时是否允许推送通知。<br/>**default**: false |
| badgeEnabled           | bool         | 是否使用Badge图标<br/>**default**: true |
| soundEnabled           | bool         | 提示音使用与否<br/>**default**: true<br/>**iOS Only** |
| priority               | int32        | 推送通知的优先顺序/可以设置的5个值如下。<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| smallIconName          | FString       | 手机顶端状态栏的通知提示小图标文件名称<br/>如果不设置，则使用应用程序图标。<br/>**default**: null<br/>**Android Only** |
| soundFileName          | FString       | 提示音文件名称/只在Android 8.0以下的OS上运行。<br/>如果指定”res/raw”文件夹的mp3、wav文件名称，则可更改提示音。<br/>**default**: null<br/>**Android Only** |

**Example**

```cpp
void Sample::RegisterPushWithOption(bool pushEnabled, bool adAgreement, bool adAgreementNight, const FString& displayLanguage, bool foregroundEnabled, bool badgeEnabled, bool soundEnabled, int32 priority, const FString& smallIconName, const FString& soundFileName)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight, displayLanguage };
    FGamebaseNotificationOptions notificationOptions{ foregroundEnabled, badgeEnabled, soundEnabled, priority, smallIconName, soundFileName };

    IGamebase::Get().GetPush().RegisterPush(configuration, notificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            // Check the error code and handle the error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

#### Get NotificationOptions

注册推送时，返还以前设置的通知选项值。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FGamebaseNotificationOptionsPtr GetNotificationOptions();
```

**Example**

```cpp
void Sample::GetNotificationOptions()
{
    auto notificationOptions = IGamebase::Get().GetPush().GetNotificationOptions();
    if (result.IsValid())
    {
        notificationOptions->foregroundEnabled = true;
        notificationOptions->smallIconName = TEXT("notification_icon_name");
        
        FGamebasePushConfiguration configuration;
        
        IGamebase::Get().GetPush().RegisterPush(configuration, notificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) { }));
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("No GetNotificationOptions"));
    }
}
```


### Request Push Settings

需要查看用户的推送设置时，请调用以下API。
利用通过回调获取的FGamebasePushTokenInfo值，可以获取自定义值。 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void QueryTokenInfo(const FGamebasePushTokenInfoDelegate& onCallback);

// Legacy API
void QueryPush(const FGamebasePushConfigurationDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryTokenInfo()
{
    IGamebase::Get().GetPush().QueryTokenInfo(FGamebasePushTokenInfoDelegate::CreateLambda([=](const FGamebasePushTokenInfo* tokenInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, tokenInfo= %s)"),
                tokenInfo->pushEnabled ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreement ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreementNight ? TEXT("true") : TEXT("false"),
                *tokenInfo->token);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo failed. (error: %d)"), error->code);
        }
    }));
}
```


#### FGamebasePushTokenInfo

| Parameter           | Values                 | Description         |
| --------------------| -----------------------| ------------------- |
| pushType            | FString                | Push令牌种类       |
| token               | FString                | 令牌                  |
| userId              | FString                | 用户ID          |
| deviceCountryCode   | FString                | 国家代码            |
| timezone            | FString                | 标准时区             |
| registeredDateTime  | FString                | 令牌更新时间    |
| languageCode        | FString                | 语言设置            |
| agreement           | FGamebasePushAgreement | 是否同意接收        |
| sandbox             | bool                   | sandbox与否(iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 是否允许显示推送通知            |
| adAgreement      | bool | 是否同意接收广告性推送通知      |
| adAgreementNight | bool | 是否允许在夜间显示广告性推送通知  |


#### Setting for APNS Sandbox

* 在APNS Sandbox环境下，为了正常启动Push功能，需要开启SandboxMode。 
* Push发送方法
    * 在Push菜单的**对象**中选择**iOS Sandbox**后发送。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void SetSandboxMode(bool isSandbox);
```

**Example**

```cpp
void Sample::SetSandboxMode(bool isSandbox)
{
    IGamebase::Get().GetPush().SetSandboxMode(isSandbox);
}
```


### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | 是TOAST Push库错误。<br>请确认DetailCode。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 未成功调用上一次的推送API。<br>执行上一次的推送API回调之后，请再调用。| 
| PUSH_UNKNOWN_ERROR             | 5999       | 是未定义的推送错误。<br>请您将所有日志上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。|

* 关于所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 相关错误是在TOAST Push库出现的。 
* 确认错误代码的方法如下。 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* 请确认TOAST Push的错误代码。 
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
