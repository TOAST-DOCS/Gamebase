## Game > Gamebase > iOS Developer's Guide > Push

### Settings

#### Apple Developer Certificates

This document describes the process of creating Apple developer certificates required to deliver push notifications.

* Go to the [Apple Developer&#39;s Site](https://developer.apple.com) and create a certificate with **Apple Push Notification service SSL** from **Add iOS Certificate**.
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

By calling API as below, a user can be registered to TOAST Push.
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

### Request Push Settings

To retrieve user's push setting, apply API as below.
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
| TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101 | Error in TOAST  Push library.Please check DetailCode. |
| TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102 | Previous PUSH API call is not completed.Please call again after the previous push API callback is executed. |
| TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999 | Unknown push error. Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

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
    * [Notification > Push > SDK v1.4 Guide > Error Handling](/en/Notification/Push/en/Client%20SDK%20Guide/#_5)


