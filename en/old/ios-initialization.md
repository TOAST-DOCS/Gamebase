## Game > Gamebase > iOS Developer's Guide > Initialization

## Initialization

To use Gamebase iOS SDK, initialization is required in the first place..

### Import Header File

First, import Gamebase header file to the app. <br/>
Get the following header file to where Gamebase functions will be initialized, such as AppDelegate.h.

```objectivec
#import <Gamebase/Gamebase.h>
```


### Configuration Settings

When Gamebase is initialized, Gamebase setting can be modified with TCGBConfiguration.

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M                          | Set App ID and version of TCGBConfiguration.<br/>Status of update or maintenance can be decided upon a game version. <br/>Specify a game version. |
| enablePopup:                       | O                          | **[UI]**<br/>When a game user cannot play games due to system maintenance or ban of use, reasons need to be displayed by pop-ups.<br/>If it is set **YES**, Gamebase will automatically display information pop-ups in such instances.<br/> **NO** is set as default. <br/>When it is **NO**, get information from launching results and display why user cannot play games on your own UI. |
| enableLaunchingStatusPopup:        | O                          | **[UI]**<br/>Depending on the launching results, when a login is unavailable (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups. <br/> Works only when **enablePopup:YES** is on. <br/> **YES** is set as default |
| enableBanPopup:                    | O                          | **[UI]**<br/> When a game user is banned for use, you can change whether to allow Gamebase to automatically display a pop-up on the reasons. <br/> Works only when **enablePopup:**is on.<br/> **YES** is set as default. |


### Debug Mode
Gamebase shows warning and error logs only.
To turn on system logs for the reference of development, call **[TCGBGamebase setDebugMode:YES]**.

> <font color="red">[Caution]</font><br/>
>
> To **release** a game, be sure to delete setDebugMode call from a source code or change the parameter to NO before a build.


### Initialize
Process initialization, in **application:didFinishLaunchingWithOptions:**.


> <font color="red">[Caution]</font><br/>
>
> To initialize Gamebase, a call of **initializeWithConfiguration:launchOptions:completion:** can be made from **application:didFinishLaunchingWithOptions:**, as well. 
>

<br/>


> <font color="red">[Caution]</font><br/>
>
> If a different Gamebase API is called, instead of the **initializeWithConfiguration:launchOptions:completion:** method, Initialization may not work properly. 

1. Create **TCGBConfiguration**, and set each property.
2. By using the **TCGBConfiguration** object, call **initializeWithConfiguration:launchOptions:completion:**.
3. Check the **TCGBError** object delivered to the **completion** block to decide whether succeeded; if initialization failed, try again.



```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration enablePopup:YES];
    [configuration enableLaunchingStatusPopup:YES];
    [configuration enableBanPopup:YES];

    [TCGBGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```



### Launching Status

Check launching status on the call result of Initialize Gamebase. <br/>
Need to call launching status after Gamebase is initialized.

```objectivec
- (void)myMethodAfterGamebaseInitialized {
    TCGBLaunchingStatus launchingStatus = [TCGBLaunching launchingStatus];

    // You can check whether if Gamebase was initialized or not using this launchingStatus
    if (launchingStatus == 0) {
        NSLog(@"Service is not initialized.");
    }

    // After Initialize Complete
    if (launchingStatus == INSPECTING_SERVICE) {
        NSLog(@"Service in Maintenance");
    } else if (launchingStatus == IN_SERVICE) {
        NSLog(@"Service in Service");
    } else {
        ...
    }
}

```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Service is now normally provided         |
| RECOMMEND_UPDATE            | 201  | Update is recommended                    |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available |
| REQUIRE_UPDATE              | 300  | Update is required                       |
| BLOCKED_USER                | 301  | User whose access has been blocked
| TERMINATED_SERVICE          | 302  | Service has been terminated                                   |
| INSPECTING_SERVICE          | 303  | Under maintenance now                                |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service        |
| INTERNAL_SERVER_ERROR       | 500  | Error of internal server              |


## Lifecycle Event

To manage iOS app events, implement the following **UIApplicationDelegate** protocol.

### OpenURL Event

Call **application:openURL:sourceApplication:annotation:**, to notify each authentication SDK of IdP to operate as required, in case of switching app-based authentication.



> <font color="red">[Caution]</font><br/>
>
> If **application:openURL:options:** of UIApplicationDelegate has been already overriden, call of **application:openURL:sourceApplication:annotation:** may not work.
>

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[UIApplication sharedApplication] setApplicationIconBadgeNumber:0];
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### DidBecomeActive Event

Call **applicationDidBecomeActive:**, to notify each authentication SDK of IdP to see if app has been activated and operate as required.

```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### DidEnterBackground Event

Call **applicationDidEnterBackground**, to notify whether an app has been converted to background.

```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### WillEnterForeground Event

Call **applicationWillEnterForeground**, to notify that app will be converted to foreground.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```


### Error Handling

| Error                         | Error Code | Description                  |
| ----------------------------- | ---------- | ---------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase is not initialized. |
| TCGB\_ERROR\_NOT\_LOGGED\_IN       | 2          | Login is required.
| TCGB\_ERROR\_INVALID\_PARAMETER    | 3          | Invalid parameter.           |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | Error in JSON format.         |
| TCGB\_ERROR\_USER\_PERMISSION      | 5          | Not authorized.              |
| TCGB\_ERROR\_NOT\_SUPPORTED        | 10         | Not supported.         |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS   | 12         | Not supported by iOS.   |



* Refer to the following document for the entire error codes.
  - [Entire Error Codes](./error-codes#client-sdk)
