## Game > Gamebase > User Guide for Unreal SDK > Push

This document describes setting requirements to enable push notifications on each platform. 

### Settings

Regarding push settings on Android or iOS, see the following documents. <br/>

* [Android Push Settings](aos-push#settings)
    * For a Firebase push, read the guide as below.
        * In the guide, Unity build setting must be applied the same for Unreal.  
            * For Android buildup, see the guide as reference so as to include the res/values/google-services-json.xml file. 
            * File exists on the Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml path. 
* [iOS Push Settings](ios-push#settings)

### Register Push

Call the following API to register users for NHN Cloud Push. 
Get a consent from user for EnablePush, EnableAdPush, and EnableAdNightPush, and call the following API to complete with registration. 

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

* You can use the Notification Options to change how the notification will be displayed in the device.
* You can call the RegisterPush API at runtime to change it.

#### Set Notification Options with RegisterPush in Runtime

When calling the RegisterPush API, you can add the FGamebaseNotificationOptions argument to set the notification options.
When you pass the IGamebase::Get().GetPush().GetNotificationOptions() call results to the creator of the FGamebaseNotificationOptions, the object initialized with the current notification options is created, and you can just change the necessary values.<br/>
The following settings are available:

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | Expose the notifications when the app is in the foreground status<br/>**default**: false |
| badgeEnabled           | bool         | Use the badge icon<br/>**default**: true |
| soundEnabled           | bool         | Whether to use the notification sound<br/>**default**: true<br/>**iOS Only** |
| priority               | int32        | Notification priority. You can set the following five values:<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| smallIconName          | FString       | Small icon file name for notification.<br/>If disabled, the app icon is used.<br/>**default**: null<br/>**Android Only** |
| soundFileName          | FString       | Notification sound file name. Works only in OS with Android 8.0 or less.<br/>The notification sound changes if you specify the mp3, wav file name in the 'res/raw' folder.<br/>**default**: null<br/>**Android Only** |

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

Retrieves the notification options value which was set previously when registering the push notification.

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

Use the following API to query user's push setting. 
You can get user configuration value from the FGamebasePushTokenInfo value of the incoming callback.

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
| pushType            | FString                | Push token type       |
| token               | FString                | Token                 |
| userId              | FString                | User ID         |
| deviceCountryCode   | FString                | Country code           |
| timezone            | FString                | Standard timezone           |
| registeredDateTime  | FString                | Token update time    |
| languageCode        | FString                | Language settings            |
| agreement           | FGamebasePushAgreement | Opt in        |
| sandbox             | bool                   | Whether to use sandbox (iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | Opt in to display notifications           |
| adAgreement      | bool | Opt in to display advertisement notifications      |
| adAgreementNight | bool | Opt in to display night advertisement notifications  |


#### Setting for APNS Sandbox

* Enable the SandboxMode, and push messages can be registered and sent via APNS Sandbox. 
* Sending from Console
    * Select **iOS Sandbox** from **Target** of the Push menu, and send.

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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error of NHN Cloud Push library.<br> Check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous call for push API is yet to be completed. <br> Wait until the previous push API callback is executed and call again. |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error. <br> Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Read the following document for the entire error codes: 
    * [Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* This error occurs from NHN Cloud Push library. 
* Check your error codes like below: 

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

* Please check NHN Cloud Push error codes.  
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
