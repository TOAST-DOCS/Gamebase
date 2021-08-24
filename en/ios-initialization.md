## Game > Gamebase > iOS Developer's Guide > Initialization

To use Gamebase iOS SDK, initialization is required.

### Import Header File

First, import Gamebase header file to the app.<br/>
Get the following header file to where Gamebase functions will be initialized, such as AppDelegate.h.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Initialization Flow

When the game starts, enable the Debug Mode and reset the Gamebase to implement the flow as shown below so that entering the game will be determined based on the Launching Status Code.

![initialization flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

When Gamebase is initialized, Gamebase setting can be modified with TCGBConfiguration.

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M                          | Set App ID and app version of TCGBConfiguration.<br/>Status of update or maintenance can be decided upon a game version.<br/>Specify a game version. |
| enablePopup:                       | O                          | **[UI]**<br/>When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.<br/>If it is set **YES**, Gamebase will automatically display information via pop-ups.<br/>**NO** is set as default.<br/>When set to **NO**, get information from launching results and display why user cannot play games by using customized UI. |
| enableLaunchingStatusPopup:        | O                          | **[UI]**<br/>Depending on the launching results, when unavailable to login (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups.<br/>Works only when **enablePopup:YES** is on.<br/>**YES** is set as default. |
| enableBanPopup:                    | O                          | **[UI]**<br/>When a game user is banned, you can change whether to allow Gamebase to automatically display a pop-up on the reasons.<br/>Works only when **enablePopup:** is on.<br/>**YES** is set as default. |


### Debug Mode
Gamebase shows warning and error logs only.
To turn on system logs for the reference of development, call **[TCGBGamebase setDebugMode:YES]**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, be sure to delete setDebugMode call from a source code or change the parameter to NO.

You can also perform the debug setting in the console and the values set in the console have priority.
Please see the following guide to set in the console.

* [Setting the console test device](./oper-app/#test-device)
* [Setting the console client](./oper-app/#client)



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



### Launching Information

Check launching status by calling Gamebase#initialize.<br/>
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

With the launchingInformations API, you can get the LaunchingInfo object after initialization.

**API**

```objectivec
#import <Gamebase/Gamebase.h>

NSDictionary* launchingInfo = [TCGBLaunching launchingInformations];
```


#### 1. Launching

Refers to Gamebase launching data.

**1.1 Status**

The game status information belongs to the app version entered for the setting of Gamebase Android SDK initialization.

* code: Game status code (Under Maintenance, Requires Update, Service Closed, and etc.)
* message: Status message of a game

Refer to the table for status codes:

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Service is now normally provided.                                 |
| RECOMMEND_UPDATE            | 201  | Update is recommended.                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available. |
| IN_TEST                     | 203  | Under test |
| IN_REVIEW                   | 204  | Review in progress |
| IN_BETA                     | 205  | Beta server environment |
| REQUIRE_UPDATE              | 300  | Update is required.                                  |
| BLOCKED_USER                | 301  | User whose access has been blocked. |
| TERMINATED_SERVICE          | 302  | Service has been terminated.                                   |
| INSPECTING_SERVICE          | 303  | Under maintenance now.                                 |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service.                              |
| INTERNAL_SERVER_ERROR       | 500  | Error of internal server.                                 |


[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

This is information about apps registered in the Gamebase Console.

* accessInfo
    * serverAddress: server address
* customerService
    * accessInfo : Customer Center contact
    * type : Customer Center type
    * url : Customer Center URL
* relatedUrls
    * termsUrl: Terms and Conditions
    * personalInfoCollectionUrl: privacy agreement
    * punishRuleUrl: user ban rules
* install: installation URL
* idP: authentication information

[Console Guide] (/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

This is information about maintenance registered in the Gamebase Console.

* url: URL for maintenance page
* timezone: Standard time zone (timezone)
* beginDate: Start time
* endDate: End time
* message: Cause of maintenance

[Console Guide] (/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

This is information about notification registered in the Gamebase Console.

* message: Message
* title: Title
* url: Maintenance URL

[Console Guide] (/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

The appKey of the NHN Cloud service associated with Gamebase.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

This is information about IAP stores registered in the NHN Cloud console.

* id: App ID
* name: App Name
* storeCode: Store Code

[Console Guide] (/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

The information users entered in the NHN Cloud Launching Console.

* The value entered by users is sent as a JSON string.
* See the guide below for the detailed NHN Cloud Launching settings.

[Console Guide] (/Game/Gamebase/ko/oper-management/#config)


### Handling Unregistered Version
      
If GameClientVersion that is not registered in the Gamebase Console is initialized, the **LAUNCHING_UNREGISTERED_CLIENT(2004)** error occurs.
In the enablePopup(true), enableLaunchingStatusPopup(true) status, it forces the update popup to be displayed and users can move to the market.
If the Gamebase popup is not used, the related UI can be manually implemented so users can move to the market by acquiring UpdateInfo from the TCGBError object.

**VO**

```objectivec
@interface TCGBUpdateInfo : NSObject

// Store installation URL that can download the latest version.
@property (nonatomic, strong, nullable) NSString* installUrl;

// A message that is exposed to users; it is delivered in the language set by the user's device.
// The message is as below if the language is 'en.'
// 'The version is not supported. Please get the latest update version.'
@property (nonatomic, strong, nullable) NSString* message;

@end
```

> <font color="red">[Caution]</font><br/>
>
> The launchingInformations API is not an asynchronous API that retrieves information from the server in real time.
> It returns cached information updated every 2 minutes, so it is not suitable for real-time checking of the current status.
> In that case, use GamebaseEventHandler, which triggers an event when the Launching Status Code is changed.
> [Game > Gamebase > iOS SDK User Guide  > ETC > Additional Features > Gamebase Event Handler > Observer](./ios-etc/#observer)

**API**

```objectivec
+ (nullable TCGBUpdateInfo *)updateInfoFromError:(nonnull TCGBError *)error;
```


**Example**

```objectivec
- (void)initializeGamebase {
    TCGBConfiguration* config = [TCGBConfiguration configurationWithAppID:@"YOUR_APP_ID" appVersion:@"YOUR_APP_VERSION" zoneType:@"YOUR_ZONE_TYPE"];
    [TCGBGamebase initializeWithConfiguration:config completion:^(id launchingData, TCGBError *result) {

        if (result == nil) {
            // Gamebase initialization succeeded.
        } else {
            // Gamebase initialization failed.
            TCGBUpdateInfo* updateInfo = [TCGBUpdateInfo updateInfoFromError:result];
            if (updateInfo) {
                // Unregistered game client version.
                // Open market url to update application.
                NSLog(@"UpdateInfo after initialize => \n%@", [updateInfo prettyJsonString]);
            }
        
        }
    }];
}
```

## Lifecycle Event

To manage iOS app events, implement the following **UIApplicationDelegate** protocol.

> <font color="red">[Caution]</font><br/>
>
> If you are using SceneDelegate (iOS 13 or later), **UISceneDelegate** protocol must be implemented.
>

### OpenURL Event
Call **application:openURL:sourceApplication:annotation:** method to notify Gamebase when application's external URL was tried to be open. Gamebase will deliver a corresponding value to authentication SDK of each IdP to make it operate as required.

> <font color="red">[Caution]</font><br/>
>
> If **application:openURL:options:** of UIApplicationDelegate has been already overriden, call of **application:openURL:sourceApplication:annotation:** may not work.
>


> <font color="red">[Caution]</font><br/>
>
> When using the WeiboAuthAdapter, the implementation of **application:openURL:sourceApplication:annotation:** is mandatory.
>

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [TCGBGamebase application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

If you are using SceneDelegate (iOS 13 or later), **scene:openURLContexts:** method must be called.

```objectivec
// SceneDelegate.m
- (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts {
    [TCGBGamebase scene:scene openURLContexts:URLContexts];
}
```


### DidBecomeActive Event
Call **applicationDidBecomeActive:** method to notify Gamebase whether an app has been activated or not. Gamebase delivers a corresponding value to authentication SDK of each IdP to make it operate as required.



```objectivec
// AppDelegate.m
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

If you are using SceneDelegate (iOS 13 or later), **sceneDidBecomeActive:** method must be called.

```objectivec
// SceneDelegate.m
- (void)sceneDidBecomeActive:(UIScene *)scene {
    [TCGBGamebase sceneDidBecomeActive:scene];
}
```

### DidEnterBackground Event
Call **applicationDidEnterBackground**, to notify Gamebase that an app will be converted to background.


```objectivec
// AppDelegate.m
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

If you are using SceneDelegate (iOS 13 or later), **sceneDidEnterBackground:** method must be called.

```objectivec
// SceneDelegate.m
- (void)sceneDidEnterBackground:(UIScene *)scene {
    [TCGBGamebase sceneDidEnterBackground:scene];
}
```

### WillEnterForeground Event
Call **applicationWillEnterForeground**, to notify Gamebase that an app will be converted to foreground.

```objectivec
// AppDelegate.m
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```

If you are using SceneDelegate (iOS 13 or later), **sceneWillEnterForeground:** method must be called.

```objectivec
// SceneDelegate.m
- (void)sceneWillEnterForeground:(UIScene *)scene {
    [TCGBGamebase sceneWillEnterForeground:scene];
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED      | 1          | Gamebase is not initialized. |
| TCGB\_ERROR\_NOT\_LOGGED\_IN       | 2          | Login is required.            |
| TCGB\_ERROR\_INVALID\_PARAMETER    | 3          | Invalid parameter.           |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | Invalid JSON format.         |
| TCGB\_ERROR\_USER\_PERMISSION      | 5          | User is not authorized.              |
| TCGB\_ERROR\_NOT\_SUPPORTED        | 10         | The function is not supported.         |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS   | 12         | The function is not supported by iOS.   |



* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)
