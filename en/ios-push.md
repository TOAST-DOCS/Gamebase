## Game > Gamebase > iOS Developer's Guide > Push

> <font color="red">[Caution]</font><br/>
>
> If you use a 3rd party push plugin or module such as Unreal or Unity, it may affect the Gamebase push function.
>

### Settings

#### Getting Authentication Information for APNS JWT

This document describes the process of getting authentication information for APNS JWT required to deliver push notifications.

* Go to [Notification > Push > Console Guide > Getting APNS JWT credentials](https://docs.toast.com/en/Notification/Push/en/console-guide/#get-authentication-information-for-apns-jwt) and get the authentication information required to register ANPS JWT.

#### Registering Gamebase Console 
* Go to **Gamebase > Push > Certificate** and enter the information you get in **APNS JWT**.

#### Implementing Notification Service Extension
* For tasks such as collecting inbound indicators and setting the notification sound, see [NHN Cloud Push Guide](https://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension) to implement the **Notification Service Extension** for the application.


#### Setting up Xcode Project
* Go to **Targets > Capabilities > Push Notifications** and set it to **ON**.
* Open the .entitlements file automatically created, and set the value of the **APS Environment** key to an appropriate value.
    * **development**: Sandbox APNS
    * **production**:  APNS

#### Import Header File
Import the following header file to the ViewController you want to implement a push API.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

Call the following API to register the user for NHN Cloud Push.

Get the values of consent to receiving push (TCGBPushConfiguration) from the user, and call the following API to complete the registration.

> <font color="red">[Caution]</font><br/>
>
> It is recommended that you always call the registerPush API after logging in, because it is not certain when the push token will expire.
>

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```

#### TCGBPushConfiguration

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| pushEnabled                   | M             | BOOL         | Consent to receiving pushes or not |
| ADAgreement                   | M             | BOOL         | Consent to receiving advertising pushes or not |
| ADAgreementNight              | M             | BOOL         | Consent to receiving nighttime advertising pushes |
| alwaysAllowTokenRegistration  | O             | BOOL         | Whether to register the token if the user denies push permissions<br>Set to YES to register the token even if push permissions are not obtained.<br>**default**: NO    |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;
    BOOL alwaysAllowTokenRegistration;
    
    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                            ADAgreement:enableAdPush
                                                                        ADAgreementNight:enableAdNightPush
                                                            alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

<br/>

When registering the user for the NHN Cloud Push, the notification option can be set using the TCGBNotificationOptions object.

Get the values of Enable foreground push (foregroundEnabled), Enable badge (badgeEnabled), and Enable notification sound (soundEnabled) from the user. Then you can call the following API to set the notification option.

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                      notificationOptions:(nullable TCGBNotificationOptions *)notificationOptions
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```
#### TCGBNotificationOptions
| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| foregroundEnabled   | M     | BOOL         | 앱이 포그라운드 상태일때의 알림 노출 여부<br/>**default**: NO           |
| badgeEnabled        | M     | BOOL         | 배지 아이콘 사용 여부<br/>**default**: YES           |
| soundEnabled        | M     | BOOL         | 알림음 사용 여부<br/>**default**: YES           |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    BOOL foregroundEnabled;
    BOOL alwaysAllowTokenRegistration;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.
    
        TCGBPushConfiguration *pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                                   ADAgreement:enableAdPush
                                                                              ADAgreementNight:enableAdNightPush
                                                                  alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];

 TCGBNotificationOptions *options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled 
                                                                                            badgeEnabled:badgeEnabled 
                                                                                            soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

### Setting for APNS Sandbox

By turning on the SandboxMode, it can be registered so that the push will be sent with the APNS Sandbox.

* How to set the client

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* Send push from Console

Select **iOS Sandbox** as the **Target** from the Push menu and send push.

### Get NotificationOptions

Retrieve the notification option value which was set when registering for the push notification.

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

> [Note]
>
> foregroundEnabled option can be changed at runtime.
> badgeEnabled and soundEnabled options are applied only when registerPush API gets called for the first time, and any changes to them at runtime are not guaranteed.
>


### Query Token Info

To view the push settings of the user, the following API is used.
You can get the push info registered with the TCGBPushTokenInfo value which comes as callback.

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
| pushType                               | string                           | Push token type                       |
| token                                  | string                           | Token                                |
| userId                                 | string                           | User ID                         |
| deviceCountryCode                      | string                           | Country code                            |
| timezone                               | string                           | Standard timezone                            |
| registeredDateTime                     | string                           | Token update time                      |
| languageCode                           | string                           | Language settings                             |
| sandbox                                | YES or NO                        | Checks if the token is registered in the sandbox environment       |
| agreement                              | TCGBPushAgreement                | Opt in                         |

#### TCGBPushAgreement

| Parameter                              | Values                            | Description               |
| -------------------------------------- | --------------------------------- | ------------------------- |
| pushEnabled                            | YES or NO                         | Opt in to display notifications           |
| ADAgreement                            | YES or NO                         | Opt in to display advertisement notifications      |
| ADAgreementNight                       | YES or NO                         | Opt in to display night advertisement notifications  |

### Event Handling

* You can receive events when a push message is received or clicked.
* For how to register event handlers, refer to the GamebaseEventHandler guide.
    * [ Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./ios-etc/#push-received-message)
    * [ Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./ios-etc/#push-click-message)
    * [ Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./ios-etc/#push-click-action)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | Error in NHN Cloud Push library.<br>Please check the error details. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous PUSH API call is not completed.<br>Please call again after the previous push API callback is executed. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | Unknown push error.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN Cloud Push library.
* The information on the error in NHN Cloud Push library is included in the error details, and you can find detailed error code and message as follows.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];
// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```



* The NHN Cloud Push error codes are as follows:
    
| Error Code |  Description |
| --- | --- |
| NHNCloudPushErrorUnknown |           Unknown |
| NHNCloudPushErrorNotInitialized |    Not initialized |
| NHNCloudPushErrorUserInvalid |       User ID is not set |
| NHNCloudPushErrorPermissionDenied |  Failed to get permission |
| NHNCloudPushErrorSystemFailed |      System failed |
| NHNCloudPushErrorTokenInvalid |      Token value is empty or invalid |
| NHNCloudPushErrorAlreadyInProgress | Already in progress |
| NHNCloudPushErrorParameterInvalid |  Invalid parameter |
| NHNCloudPushErrorNotSupported |      Not supported feature |
| NHNCloudPushErrorClientFailed |      Server error |
