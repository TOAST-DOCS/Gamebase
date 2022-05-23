## Game > Gamebase > iOS Developer's Guide > Push

> <font color="red">[Caution]</font><br/>
>
> If you use a 3rd party push plugin or module such as Unreal or Unity, it may affect the Gamebase push function.
>

### Settings

#### Apple Developer Certificates

This document describes the process of creating Apple developer certificates required to deliver push notifications.

* Go to the [Apple Developer's Site](https://developer.apple.com) and create a certificate with **Apple Push Notification service SSL** from **Add iOS Certificate**.
* Register a keychain and export the created certificate in the Personal Information Exchange (.p12) format.
* To export certificates, set passwords.

#### Registering NHN Cloud Console
* Go to **Notification > Push > Certificate** and register the certificate that was created from above to **APNS Certificate** and **APNS (Sandbox) Certificate**.
* The password you used to create the above certificate will be used for registration.

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

Call the following API to register the user for NHN Cloud Push.<br/>
Get the values of consent to receiving push (enablePush), consent to receiving advertisement push (enableAdPush), and consent to receiving night-time advertisement push (enableAdNightPush) from the user, and call the following API to complete the registration.

> <font color="red">[Caution]</font><br/>
>
> It is recommended that you always call the registerPush API after logging in, because it is not certain when the push token will expire.
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

When registering the user for the NHN Cloud Push, the notification option can be set using the TCGBNotificationOptions object.<Mb>
Get the values of Enable foreground push (foregroundEnabled), Enable badge (badgeEnabled), and Enable notification sound (soundEnabled) from the user. Then you can call the following API to set the notification option.

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

#### Get NotificationOptions

Retrieve the notification option value which was set when registering for the push notification.

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
| foregroundEnabled     | YES or NO    | Expose the notification when the app is in the foreground<br/>**default**: NO           |
| badgeEnabled          | YES or NO    | Enable badge icon<br/>**default**: YES           |
| soundEnabled          | YES or NO    | Enable notification sound<br/>**default**: YES           |


> [Note]
>
> foregroundEnabled option can be changed at runtime.
> badgeEnabled and soundEnabled options are applied only when registerPush API gets called for the first time, and any changes to them at runtime are not guaranteed.
>


### Request Push Settings

To view the push settings of the user, the following API is used.<br/>
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
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | Error in NHN Cloud Push library.<br>Please check DetailCode. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous PUSH API call is not completed.<br>Please call again after the previous push API callback is executed. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | Unknown push error.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the NHN Cloud Push library.
* Check your error codes as below.

```objectivec
TCGBError *tcgbError = error; // TCGBError instance as callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // Error object occurred at external library
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

//  By calling [tcgbError description] as below, you can get the entire error information of json format.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* The NHN Cloud Push error codes are as follows:
    
| Error Code |  Description |
| --- | --- |
| TCPushErrorNotInitialized | Not initialized |
| TCPushErrorInvalidParameters | Parameter error |
| TCPushErrorPermissionDenined | Permission not obtained |
| TCPushErrorSystemFail | System alert registration failed |
| TCPushErrorNetworkFail | Transmission on the network failed |
| TCPushErrorServerFail | Server response failed |
| TCPushErrorInvalidUrl | Invalid URL request |
| TCPushErrorNetworkNotReachable | Network not connected |
