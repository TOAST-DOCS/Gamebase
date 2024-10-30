## Game > Gamebase > User Guide for Unreal SDK > Push

This document describes setting requirements to enable push notifications on each platform. 

### Settings

For how to set up push on Android or iOS, refer to the following documents.

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/#firebase-notification)
        * When building for Android, the res/values/google-services-json.xml file must be included, so prepare the file by referring to the guide.
        * The file is in the path Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml.
* iOS
    * [iOS Push Settings](ios-push#settings)

> <font color="red">[Caution]</font><br/>
>
> If there is push-related processing in an external plugin, the Gamebase push function may not work properly.

### Register Push

Call the following API to register the user for NHN Cloud Push.
Get the values of consent to receiving push (enablePush), consent to receiving advertisement push (enableAdPush), and consent to receiving night-time advertisement push (enableAdNightPush) from the user, and call the following API to complete the registration.

> <font color="red">[Caution]</font><br/>
>
> It is recommended to call the registerPush API every time after logging in because the push settings may be different for each UserID and the push token may expire.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void RegisterPush(const FGamebasePushConfiguration& Configuration, const FGamebaseErrorDelegate& Callback);
void RegisterPush(const FGamebasePushConfiguration& Configuration, const FGamebaseNotificationOptions& notificationOptions, const FGamebaseErrorDelegate& Callback);
```

#### FGamebasePushConfiguration

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| bPushEnabled                   | M             | bool         | Consent to receiving push |
| bAdAgreement                   | M             | bool         | Consent to receiving advertisement push |
| bAdAgreementNight              | M          | bool         | Consent to receiving night-time advertisement push |
| bRequestNotificationPermission | O             | bool         | Whether to automatically display a Push permission request pop-up when calling the RegisterPush API on Android 13 or higher OS<br>**default**: true<br/>**Only for Android** |
| bAlwaysAllowTokenRegistration  | O             | bool         | Whether to register a token even if the user denies push permission<br>If set to true, the token will be registered even if push permission is not obtained.<br>**default**: false<br/>**Only for iOS** |

**Example**

```cpp
void USample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration Configuration;
    Configuration.bPushEnabled = pushEnabled;
    Configuration.bAdAgreement = adAgreement;
    Configuration.bAdAgreementNight = adAgreementNight;
    Configuration.bRequestNotificationPermission = bRequestNotificationPermission;
    Configuration.bAlwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->RegisterPush(Configuration, FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (Error: %d)"), Error->Code);
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
| bForegroundEnabled      | bool         | Expose the notifications when the app is in the foreground status<br/>**default**: false |
| bBadgeEnabled           | bool         | Use the badge icon<br/>**default**: true |
| bSoundEnabled           | bool         | Whether to use the notification sound<br/>**default**: true<br/>**iOS Only** |
| Priority               | int32        | Notification priority. You can set the following five values:<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| SmallIconName          | FString       | Small icon file name for notification.<br/>If disabled, the app icon is used.<br/>**default**: null<br/>**Android Only** |
| SoundFileName          | FString       | Notification sound file name. Works only in OS with Android 8.0 or less.<br/>The notification sound changes if you specify the mp3, wav file name in the 'res/raw' folder.<br/>**default**: null<br/>**Android Only** |

**Example**

```cpp
void USample::RegisterPushWithOption(bool pushEnabled, bool adAgreement, bool adAgreementNight, const FString& displayLanguage, bool foregroundEnabled, bool badgeEnabled, bool soundEnabled, int32 priority, const FString& smallIconName, const FString& soundFileName)
{
    FGamebasePushConfiguration Configuration;
    Configuration.bPushEnabled = pushEnabled;
    Configuration.bAdAgreement = adAgreement;
    Configuration.bAdAgreementNight = adAgreementNight;
    Configuration.bRequestNotificationPermission = bRequestNotificationPermission;
    Configuration.bAlwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;
    
    FGamebaseNotificationOptions NotificationOptions;
    NotificationOptions.bForegroundEnabled = bForegroundEnabled;
    NotificationOptions.bBadgeEnabled = bBadgeEnabled;
    NotificationOptions.bSoundEnabled = bSoundEnabled;
    NotificationOptions.Priority = Priority;
    NotificationOptions.SmallIconName = SmallIconName;
    NotificationOptions.SoundFileName = SoundFileName;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            // Check the Error code and handle the Error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

#### Get NotificationOptions

Retrieves the notification options value which was set previously when registering the push notification.

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FGamebaseNotificationOptionsPtr GetNotificationOptions();
```

**Example**

```cpp
void USample::GetNotificationOptions()
{
    auto NotificationOptions = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->GetNotificationOptions();
    if (result.IsValid())
    {
        NotificationOptions->ForegroundEnabled = true;
        NotificationOptions->SmallIconName = TEXT("notification_icon_name");
        
        FGamebasePushConfiguration Configuration;
        
        UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
        Subsystem->GetPush()->RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error) { }));
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
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void QueryTokenInfo(const FGamebasePushTokenInfoDelegate& Callback);

// Legacy API
void QueryPush(const FGamebasePushConfigurationDelegate& Callback);
```

**Example**

```cpp
void USample::QueryTokenInfo()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->QueryTokenInfo(FGamebasePushTokenInfoDelegate::CreateLambda([=](const FGamebasePushTokenInfo* TokenInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, TokenInfo= %s)"),
                TokenInfo->PushEnabled ? TEXT("true") : TEXT("false"),
                TokenInfo->bAdAgreement ? TEXT("true") : TEXT("false"),
                TokenInfo->bAdAgreementNight ? TEXT("true") : TEXT("false"),
                *TokenInfo->Token);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo failed. (Error: %d)"), Error->Code);
        }
    }));
}
```


#### FGamebasePushTokenInfo

| Parameter           | Values                 | Description         |
| --------------------| -----------------------| ------------------- |
| PushType            | FString                | Push token type       |
| Token               | FString                | Token                 |
| UserId              | FString                | User ID         |
| DeviceCountryCode   | FString                | Country code           |
| Timezone            | FString                | Standard timezone           |
| RegisteredDateTime  | FString                | Token update time    |
| LanguageCode        | FString                | Language settings            |
| Agreement           | FGamebasePushAgreement | Opt in        |
| bSandbox             | bool                   | Whether to use sandbox (iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| bPushEnabled      | bool | Opt in to display notifications           |
| bAdAgreement      | bool | Opt in to display advertisement notifications      |
| bAdAgreementNight | bool | Opt in to display night advertisement notifications  |


### Event Handling

* You can handle events when a push message is received or clicked.
* For how to register event handlers, refer to the GamebaseEventHandler guide.
    * [ Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./unreal-etc/#push-received-message)
    * [ Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./unreal-etc/#push-click-message)
    * [ Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./unreal-etc/#push-click-action)


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
void USample::SetSandboxMode(bool isSandbox)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->SetSandboxMode(isSandbox);
}
```


### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error of NHN Cloud Push library.<br> Check the error details. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous call for push API is yet to be completed. <br> Wait until the previous push API callback is executed and call again. |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error. <br> Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Read the following document for the entire error codes: 
    * [Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN Cloud Push library.
* The information on the error in NHN Cloud Push library is included in the error details, and you can find detailed error code and message as follows.


```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Messsage);

    GamebaseInnerError* moduleError = gamebaseError.Error; // GamebaseError.Error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleerror->Code, *moduleerror->Messsage);
    }
}
```

* Please check NHN Cloud Push error codes.  
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
