## Game > Gamebase > Android Developer's Guide > Push

### Register Push

Call the following API to register the user for NHN Cloud Push.<br/>
Get the values of Accept push (enablePush), Accept advertisement push (enableAdPush), and Accept night advertisement push (enableAdNightPush) from the user, and call the following API to complete the registration.

**API**

```java
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseCallback callback);
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseNotificationOptions notificationOptions,
                                   @NonNull GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = PushConfiguration.newBuilder()
        .enablePush(enablePush)
        .enableAdAgreement(enableAdPush)
        .enableAdAgreementNight(enableAdNightPush)
        .build();

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

### Notification Options

* You can use the Notification Options to change how the notification will be displayed in the device.
* Notification Options can be changed by setting it to AndroidManifest.xml or calling the registerPush API at runtime.

#### Set Notification Options with AndroidManifest.xml

Notification options can be set by defining them to AndroidManifest.xml.<br/>
To find out how to set the options, see the following guide:

[Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > Notification Options](./aos-started/)

#### Set Notification Options with RegisterPush in Runtime

You can also set the notification options at runtime without defining them in AndroidManifest.xml, or you can change the value set in AndroidManifest.xml at runtime as well.
When calling the registerPush API, add the GamebaseNotificationOptions argument to set the notification options.
If you pass the call results for the Gamebase.Push.getNotificationOptions() with the argument of the GamebaseNotificationOptions.newBuilder(), the builder initialized with the current notification options is created. Thus, you can just change the necessary values.<br/>
The following settings are available:

| API                   | Parameter       | Description        |
| --------------------  | ------------ | ------------------ |
| enableForeground      | boolean      | Expose the notifications when the app is in the foreground status<br/>**default**: false |
| enableBadge           | boolean      | Use the badge icon<br/>**default**: true |
| setPriority           | int          | Notification priority. You can set the following five values:<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2<br/>**default**: NoticationComapt.HIGH |
| setSmallIconName         | String       | Small icon file name for notification.<br/>If disabled, app icon is used.<br/>**default**: null |
| setSoundFileName      | String       | Notification sound file name. Works only in AOS 8.0 or earlier.<br/>The notification sound will change as you specify the .mp3 or .wav file name in the 'res/raw' folder.<br/>**default**: null |

**Example**

```java
boolean enableForeground;
boolean enableBadge;
// NotificationCompat.PRIORITY_MIN     : -2
// NotificationCompat.PRIORITY_LOW     : -1
// NotificationCompat.PRIORITY_DEFAULT :  0
// NotificationCompat.PRIORITY_HIGH    :  1
// NotificationCompat.PRIORITY_MAX     :  2
int priority;
String smallIconName;
String soundFileName;

GamebaseNotificationOptions currentOptions = Gamebase.Push.getNotificationOptions(activity);
GamebaseNotificationOptions notificationOptions = GamebaseNotificationOptions.newBuilder(currentOptions)
        .enableForeground(enableForeground)
        .enableBadge(enableBadge)
        .setPriority(priority)
        .setSmallIconName(smallIconName)
        .setSoundFileName(soundFileName)
        .build();

Gamebase.Push.registerPush(activity, pushConfiguration, notificationOptions, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### Get NotificationOptions

Retrieves the notification options value which was set previously when registering the push notification.
Retrieves the most recent settings by including the value set in AndroidManifest.xml and the value which was changed at runtime.

**API**

```java
+ (GamebaseNotificationOptions)Gamebase.Push.getNotificationOptions(@NonNull Context context);
```

### Request Push Settings

To retrieve user's push setting, apply API as below. <br/>
From GamebasePushTokenInfo callback values, you can get user's value set.

**API**

```java
+ (void)Gamebase.Push.queryTokenInfo(@NonNull Activity activity,
                                     @NonNull GamebaseDataCallback<GamebasePushTokenInfo> callback);

// Legacy API
+ (void)Gamebase.Push.queryPush(@NonNull Activity activity,
                                @NonNull GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

```java
Gamebase.Push.queryTokenInfo(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(GamebasePushTokenInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.agreement.pushEnabled;
            boolean enableAdPush = data.agreement.adAgreement;
            boolean enableAdNightPush = data.agreement.adAgreementNight;
            String token = data.token;
        } else {
            // Failed.
        }
    }
});
```

#### GamebasePushTokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | String                | Push token type       |
| token               | String                | Token                 |
| userId              | String                | User ID         |
| deviceCountryCode   | String                | Country code           |
| timezone            | String                | Standard timezone           |
| registeredDateTime  | String                | Token update time    |
| languageCode        | String                | Language settings            |
| agreement           | GamebasePushAgreement | Opt in        |

#### GamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | boolean | Opt in to display notifications           |
| adAgreement      | boolean | Opt in to display advertisement notifications      |
| adAgreementNight | boolean | Opt in to display night advertisement notifications  |

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error in NHN Cloud Push library.<br>Please check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous push API call has not been completed.<br>Please call again after the previous push API callback is executed. |
| PUSH_UNKNOWN_ERROR             | 5999       | Unknown push error.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

* Refer to the following document for the entire error codes:
    * [Entire Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the NHN Cloud Push library.
* Check the error code as below.

```java
Gamebase.Push.registerPush(activity, pushConfiguration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Register push successful");
            ...
        } else {
            Log.e(TAG, "Register push failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PUSH_EXTERNAL_LIBRARY_ERROR) {
                // TOAST Push Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* The NHN Cloud Push SDK error codes are as follows:

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API call is successful |
| NOT_INITIALIZE | 100 | NHN Cloud SDK or NHN Cloud Push SDK is not initialized |
| PROVIDER_SDK_ERROR | 101 | Error occurs from external SDK (Firebase or Tencent) |
| USER_ID_NOT_REGISTERED | 102 | Not logged in |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType is invalid or push library is not included to a project |
| API_SERVER_ERROR | 104 | NHN Cloud Push server API call fails |
| TOKEN_NOT_REGISTERED | 105 | Internally-cached push token does not exist |
