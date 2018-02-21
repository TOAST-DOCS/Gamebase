## Game > Gamebase > iOS Developer's Guide > Initialization

To use Gamebase iOS SDK, initialization is required.

### Import Header File

First, import Gamebase header file to the app.
Get the following header file to where Gamebase functions will be initialized, such as AppDelegate.h.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Configuration Settings

When Gamebase is initialized, Gamebase setting can be modified with TCGBConfiguration.

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M | Set App ID and app version of TCGBConfiguration.Status of update or maintenance can be decided upon a game version.<br/>Specify a game version. |
| enablePopup: | O | **[UI]**<br/> When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.<br/>If it is set **YES**, Gamebase will automatically display information via pop-ups.<br/>**NO** is set as default.<br/>When set to **NO**, get information from launching results and display why user cannot play games by using customized UI. |
| enableLaunchingStatusPopup: | O | **[UI]**<br/>Depending on the launching results, when unavailable to login (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups. Works only when **enablePopup:YES** is on.<br/>**YES** is set as default. |
| enableBanPopup: | O | **[UI]**<br/>When a game user is banned, you can change whether to allow Gamebase to automatically display a pop-up on the reasons.  Works only when **enablePopup:** is on.<br/>**YES** is set as default. |


### Debug Mode

Gamebase shows warning and error logs only.
To turn on system logs for the reference of development, call **[TCGBGamebase setDebugMode:YES]**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, be sure to delete setDebugMode call from a source code or change the parameter to NO.


### Initialize

Process initialization, in **application:didFinishLaunchingWithOptions:**.

> <font color="red">[Caution]</font><br/>
>
> The **initializeWithConfiguration:launchOptions:completion:** method call can be made from **application:didFinishLaunchingWithOptions:** , as well.
>

<br/>


> <font color="red">[Caution]</font><br/>
>
> The **initializeWithConfiguration:launchOptions:completion:** method must be called before a call is made for another Gamebase API.

1. Create **TCGBConfiguration** object, and set each property.
2. Call **initializeWithConfiguration:launchOptions:completion:** by using the **TCGBConfiguration** object.
3. Check the **completion** block by using **TCGBError** object to decide whether it is successful. If initialization fails, try again; make sure to include the logic of initialization to prevent potential problems.


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

Check launching status by calling Gamebase#initialize.
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
| IN_SERVICE                  | 200  | Service is now normally provided.                           |
| RECOMMEND_UPDATE            | 201  | Update is recommended.                              |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available. |
| REQUIRE_UPDATE              | 300  | Update is required.                                  |
| BLOCKED_USER                | 301  | User whose access has been blocked. |
| TERMINATED_SERVICE          | 302  | Service has been terminated.                                   |
| INSPECTING_SERVICE          | 303  |  Under maintenance now.                                 |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service.                              |
| INTERNAL_SERVER_ERROR       | 500  | Error of internal server.                                 |


## Lifecycle Event

To manage iOS app events, implement the following **UIApplicationDelegate** protocol.

### OpenURL Event

Call **application:openURL:sourceApplication:annotation:** method to notify Gamebase when application's external URL was tried to be open.
Gamebase will deliver a corresponding value to authentication SDK of each IdP to make it operate as required.

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

Call **applicationDidBecomeActive:** method to notify Gamebase whether an app has been activated or not.
Gamebase delivers a corresponding value to authentication SDK of each IdP to make it operate as required.


```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### DidEnterBackground Event

Call **applicationDidEnterBackground**, to notify Gamebase that an app will be converted to background.


```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### WillEnterForeground Event

Call **applicationWillEnterForeground**, to notify Gamebase that an app will be converted to foreground.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```


### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase is not initialized. |
| TCGB\_ERROR\_NOT\_LOGGED\_IN | 2 | Login is required.
| TCGB\_ERROR\_INVALID\_PARAMETER | 3 | Invalid parameter. |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4 | Invalid JSON format. |
| TCGB\_ERROR\_USER\_PERMISSION | 5 | User is not authorized. |
| TCGB\_ERROR\_NOT\_SUPPORTED | 10 | The function is not supported. |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12 | The function is not supported by iOS. |



* Refer to the following document for the entire error codes.
  - [Entire Error Codes](./error-code/#client-sdk)
