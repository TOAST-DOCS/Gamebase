## Game > Gamebase > iOS Developer's Guide > Push

### Settings

#### Apple Developer Certificates

This document describes the process of creating Apple developer certificates required to deliver push notifications.

* Go to the [Apple Developer's Site](https://developer.apple.com) and create a certificate with **Apple Push Notification service SSL** from **Add iOS Certificate**.
* Register a keychain and export the created certificate in the Personal Information Exchange (.p12) format.
* To export certificates, set passwords.

#### Register TOAST Console
* Go to **Notification > Push > Certificate** to register the certificate created above at **APNS Certificate** and **APNS (Sandbox) Certificate**.
* Use the passwords set above to register.

#### Set XCode Project
* Set **ON** for **Targets > Capabilities > Push Notifications**.
* Open .entitlements file, which has been automatically created, to set an appropriate value of **APS Environment**.
    * **development** : Sandbox APNS
    * **production** : APNS

#### Import Header File
Import the following header file to the ViewController you want to implement a push API.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

By calling API as below, a user can be registered to TOAST Push.<br/>
With user's agreement to enablePush, enableAdPush, and enableAdNightPush, call following API to complete registration.


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

#### Setting for APNS Sandbox

With SandboxMode, push can be registered to be delivered to APNS Sandbox.
* How to register push with APNS Sandbox mode

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

### Request Push Settings

To retrieve user's push setting, apply API as below.<br/>
From **TCGBPushConfiguration** callback values, you can get user's value set.

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

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | Error in TOAST  Push library.<br>Please check DetailCode. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous PUSH API call is not completed.<br>Please call again after the previous push API callback is executed. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | Unknown push error.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the TOAST Push library.
* Check your error codes as below.

```objectivec
TCGBError *tcgbError = error; // TCGBError instance as callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // Error object occurred at external library
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

//  By calling [tcgbError description] as below, you can get the entire error information of json format.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* Refer to the following document for TOAST Push error codes
    * [Notification > Push > SDK v1.4 Guide > Error Handling](/Notification/Push/ko/sdk-guide-ios/#_11)
