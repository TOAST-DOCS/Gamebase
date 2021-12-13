## Game > Gamebase > Unity Developer's Guide > Push

This document describes how to set push notifications for each platform.

### Settings

For how to set up push on Android or iOS, refer to the following documents.

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/#firebase-notification)
* iOS
    * [iOS Push Settings](ios-push#settings)


### Register Push

Call the following API to register the user for NHN Cloud Push.
Get the values of consent to receiving push (enablePush), consent to receiving advertisement push (enableAdPush), and consent to receiving night-time advertisement push (enableAdNightPush) from the user, and call the following API to complete the registration.

> <font color="red">[Caution]</font><br/>
>
> It is recommended to call the registerPush API every time after logging in because the push settings may be different for each UserID and the push token may expire.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback);
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseRequest.Push.NotificationOptions options, GamebaseCallback.ErrorDelegate callback);
```

**Example**

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
#### Setting for APNS Sandbox
* Enable the SandboxMode, and push messages can be registered and sent via APNS Sandbox. 
* Sending from Console
    * Select **iOS Sandbox** from **Target** of the Push menu, and send.

### Notification Options

* You can use the Notification Options to change how the notification will be displayed in the device.
* You can call the registerPush API at runtime to change it.

#### Set Notification Options with RegisterPush in Runtime

When calling the RegisterPush API, you can add the GamebaseRequest.Push.NotificationOptions argument to set the notification options.
If you pass the Gamebase.Push.GetNotificationOptions() call results to the creator of the GamebaseRequest.Push.NotificationOptions, the object initialized with the current notification options is created, and you can just change the necessary values.<br/>
The following settings are available:

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | Expose the notifications when the app is in the foreground status<br/>**default**: false |
| badgeEnabled           | bool         | Use the badge icon<br/>**default**: true |
| soundEnabled           | bool         | Whether to use the notification sound<br/>**default**: true<br/>**iOS Only** |
| priority               | int          | Notification priority. You can set the following five values:<br/>GamebaseNotificationPriority.MIN : -2<br/> GamebaseNotificationPriority.LOW : -1<br/>GamebaseNotificationPriority.DEFAULT : 0<br/>GamebaseNotificationPriority.HIGH : 1<br/>GamebaseNotificationPriority.MAX : 2<br/>**default**: GamebaseNotificationPriority.HIGH<br/>**Android Only** |
| smallIconName          | string       | Small icon file name for notification.<br/>If disabled, the app icon is used.<br/>**default**: null<br/>**Android Only** |
| soundFileName          | string       | Notification sound file name. Works only in OS with Android 8.0 or less.<br/>The notification sound changes if you specify the mp3, wav file name in the 'res/raw' folder.<br/>**default**: null<br/>**Android Only** |

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

    Gamebase.Push.RegisterPush(configuration, notificationOptions, (error) =>
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

Retrieves the notification options value which was set previously when registering the push notification.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
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

To retrieve user's push setting, apply API as below.
From GamebaseResponse.Push.TokenInfo callback values, you can get user's value set.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void QueryTokenInfo(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.TokenInfo> callback);

// Legacy API
public static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback);
```

**Example**

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
| pushType            | string                | Push token type       |
| token               | string                | Token                 |
| userId              | string                | User ID         |
| deviceCountryCode   | string                | Country code           |
| timezone            | string                | Standard timezone           |
| registeredDateTime  | string                | Token update time    |
| languageCode        | string                | Language settings            |
| agreement           | GamebaseResponse.Push.Agreement | Opt in        |
| sandbox             | bool                  | Whether to use sandbox (iOS Only)        |

#### GamebaseResponse.Push.Agreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | Opt in to display notifications           |
| adAgreement      | bool | Opt in to display advertisement notifications      |
| adAgreementNight | bool | Opt in to display night advertisement notifications  |


#### Setting for APNS Sandbox
* By turning on the SandboxMode, it can be registered so that the push will be sent with the APNS Sandbox.
* How to send console
    * In the **Target** of the Push menu, select **iOS Sandbox** to send it.

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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error in NHN Cloud Push library.<br/>Please check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | Previous Push API call is not completed.<br/>Please call again after the previous push API callback is executed.  |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error.<br/>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |


* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the NHN Cloud Push library.
* Check the error code as below:

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

* NHN Cloud Push error codes are as follows:
    * [Android](aos-push#error-handling)<br/>
    * [iOS](ios-push#error-handling)
