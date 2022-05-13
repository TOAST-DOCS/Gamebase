## Game > Gamebase > Android Developer's Guide > Initialization

To use Gamebase Android SDK, initialization is required.

### onActivityResult

For normal operations of Gamebase, make sure to call **Gamebase.onActivityResult(int, int, Intent)** from **Activity#onActivityResult(int, int, Intent)**.

**API**

```java
+ (void)Gamebase.onActivityResult(int requestCode, int resultCode, Intent data);
```

### Initialization Flow

When the game starts, enable the Debug Mode and reset the Gamebase to implement the flow as shown below so that entering the game will be determined based on the Launching Status Code.

![initialization flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

To initialize Gamebase, Gamebase setting can be modified with GamebaseConfiguration.Builder.

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| newBuilder(String appId, String appVersion, String storeCode) | **M**                      | The GamebaseConfiguration.Builder object can be created with the newBuilder() function.<br/><br/> **appId:** Enter an App ID issued from NHN Cloud Project.<br/> **appVersion:** Update or maintenance status can be decided upon a game version. Specify a game version. <br/> **storeCode** refers to the store in which APK is deployed. Find each store code in the following guide. [Purchase - Initialization](./aos-purchase/#6-initialization) |
| build()                                  | **M**                      | Convert Builder completed with setting to a configuration object.<br/>Required for **Gamebase.initialize()** API. |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.<br/>If it is set to **true** , Gamebase will automatically display information via pop-ups.<br/>The default value is **false**.<br/>When set to **false** , get information from launching results and display why user cannot play games by using customized UI. |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>Depending on the launching results, when available to log in (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups.<br/>Works only when **enablePopup(true)**.<br/>The default value is **true**. |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>When game user is banned, you can change whether to allow Gamebase to automatically display a pop-up on the reasons.<br/>Works only when **enablePopup(true)**.<br/>The default value is **true**. |

### Debug Mode
* Gamebase shows warning and error logs only.
* To turn on system logs for the reference of development, call **Gamebase.setDebugMode(true)**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, be sure to delete 'setDebugMode' call from a source code or change the parameter to 'false'.

You can also perform the debug setting in the console and the values set in the console have priority.
Please see the following guide to set in the console.

* [Setting the console test device](./oper-app/#test-device)
* [Setting the console client](./oper-app/#client)


### Initialize

Call **Gamebase#initialize(Activity, GamebaseConfiguration, and GamebaseDataCallback)** from **Activity#onCreate(Bundle)** to initialize Gamebase SDK.

**API**

```java
+ (void)Gamebase.initialize(Activity activity, GamebaseConfiguration configuration, GamebaseDataCallback<LaunchingInfo> callback);
```

**Example**

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Show gamebase debug message.
         *
         * CAUTION!
         * Set 'false' when build RELEASE.
         */
        Gamebase.setDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        String storeCode = "GG";
        GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(appId, appVersion, storeCode)
                .enablePopup(true)
                .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // Follow the launch code to decide whether to allow entry to the game.
                    ...
                } else {
                    // If initialization fails, cannot use Gamebase SDK.
                    // Display errors, and restart or close a game.
                    Log.e(TAG, "Initialize failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });

        ...
    }
    ...

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        /**
         * Pass onActivityResult event to the Gamebase.
         */
        Gamebase.onActivityResult(requestCode, resultCode, data);
        ...
    }
}
```

### Launching Information

Check launching status by calling 'Gamebase#initialize()'.<br/>
Follow each launching code to decide on the game play.

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Check launching status.
            boolean canPlay = true;
            String errorLog = "";
            switch (launchingInfo.getStatus().getCode()) {
                case LaunchingStatus.IN_SERVICE:
                    break;
                case LaunchingStatus.RECOMMEND_UPDATE:
                    Log.d(TAG, "There is a new version of this application.");
                    break;
                case LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST:
                case LaunchingStatus.IN_TEST:
                case LaunchingStatus.IN_REVIEW:
                case LaunchingStatus.IN_BETA:
                    Log.d(TAG, "You logged in because you are developer.");
                    break;
                case LaunchingStatus.REQUIRE_UPDATE:
                    canPlay = false;
                    errorLog = "You have to update this application.";
                    break;
                case LaunchingStatus.BLOCKED_USER:
                    canPlay = false;
                    errorLog = "You are blocked user!";
                    break;
                case LaunchingStatus.TERMINATED_SERVICE:
                    canPlay = false;
                    errorLog = "Game is closed!";
                    break;
                case LaunchingStatus.INSPECTING_SERVICE:
                case LaunchingStatus.INSPECTING_ALL_SERVICES:
                    canPlay = false;
                    errorLog = "Under maintenance.";
                    break;
                case LaunchingStatus.INTERNAL_SERVER_ERROR:
                default:
                    canPlay = false;
                    errorLog = "Unknown internal error.";
                    break;
            }
            if (canPlay) {
                // Game play starts.
            } else {
                // Disclose why you cannot play and suspend the game.
            }
        }
        ...
    }
});
```

With the getLaunchingInformations API, you can get the LaunchingInfo object after initialization.

> <font color="red">[Caution]</font><br/>
>
> The getLaunchingInformations() API is not an asynchronous API that retrieves information from the server in real time.
> It returns cached information updated every 2 minutes, so it is not suitable for real-time checking of the current status.
> In that case, use GamebaseEventHandler, which triggers an event when the Launching Status Code is changed.
> [Game > Gamebase > Android SDK User Guide > Notes > Additional Features > Gamebase Event Handler > Observer](./aos-etc/#observer)

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
LaunchingInfo includes values set on Gamebase Console, as well as game status.


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
| IN_SERVICE                  | 200  | Service is now normally provided                                 |
| RECOMMEND_UPDATE            | 201  | Update is recommended                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available. |
| IN_TEST                     | 203  | Under test |
| IN_REVIEW                   | 204  | Review in progress |
| IN_BETA                     | 205  | Beta server environment |
| REQUIRE_UPDATE              | 300  | Update is required                                  |
| BLOCKED_USER                | 301  | Accessed to service with a device (device key) blocked from access. |
| TERMINATED_SERVICE          | 302  | Service has been terminated                                   |
| INSPECTING_SERVICE          | 303  | Under maintenance now                                 |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service                              |
| INTERNAL_SERVER_ERROR       | 500  | Error in internal server                                 |

[Console Guide](./oper-app/#app)

**1.2 App**

App information registered on Gamebase console.

* accessInfo
    * serverAddress: Server address
* customerService
    * accessInfo : Customer Center contact information
    * type : Customer Center type
    * url : Customer Center URL
* relatedUrls
    * termsUrl: Terms of use
    * personalInfoCollectionUrl: Agreement to collection of personal information
    * punishRuleUrl: User ban rules
* install: Installation URL
* idP: Authentication information

[Game > Gamebase > Console User Guide > App > Client](./oper-app/#client)

**1.3 Maintenance**

Maintenance information registered on Gamebase Console:

* url: URL for maintenance page
* timezone: Standard time zone (timezone)
* beginDate: Start time
* endDate: End time
* message: Cause of maintenance
* hideDate: Whether to display maintenance start and end times

[Game > Gamebase > Console User Guide > Operation > Maintenance](./oper-operation/#maintenance)
<br/>
##### Change Default Maintenance HTML

If both the `enablePopup` and `enableLaunchingStatusPopup` values are `true`, a maintenance popup will be automatically displayed if the game is in maintenance status.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_popup_android_2.30.0.png)

If you click the **DETAILS** button here, the maintenance information is automatically displayed in a webview.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_webview_android_2.30.0.png)

If you want to modify the displayed HTML file, download the HTML file from the following link, modify it as you need, and place it in the 'assets/Gamebase' folder. Then the HTML file will be used to display maintenance information instead of the default HTML file included in the Gamebase SDK.
[HTML file download link](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gamebase-maintenance.html)

**1.4 Notice**

Notice information registered on Gamebase console:

* message: Message
* title: Title
* url: Maintenance URL

[Game > Gamebase > Console User Guide > Operation > Notice](./oper-operation/#notice)

#### 2. tcProduct

AppKey of NHN Cloud linked to Gamebase:

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

IAP store information registered on NHN Cloud console:

* id: App ID
* name: App Name
* storeCode: Store Code

[Game > Gamebase > Console User Guide > Payment](./oper-purchase/)

#### 4. tcLaunching

User-input information for NHN Cloud launching console:

* Send user-input values in JSON string.
* For further details of NHN Cloud Launching, see the guide as below:

[Game > Gamebase > Console User Guide > Management > Config](./oper-management/#config)

### Handling Unregistered Version

By initializing GameClientVersion which is not registered on Gamebase console, error occurs like follows: **LAUNCHING_UNREGISTERED_CLIENT(2004)**.  
Under enablePopup(true), or enableLaunchingStatusPopup(true), popup shows for a forced update, and the user could be linked to the market.  
In case Gamebase popup is disabled, UI can be executed in the game by getting UpdateInfo from GamebaseException so that user can go to the market.  

**VO**

```java
class UpdateInfo {
    // URL for store installation to download the latest version 
    String installUrl;
    // User can find a message in the language set on device.
    // When the language is 'en', the message shows like follows.
    // 'This version is not supported. Please get the most updated version.'
    String message;
}
```

**API**

```java
+ (UpdateInfo)UpdateInfo.from(GamebaseException exception);
```

**Example**

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Gamebase initialization succeeded.
        } else {
            // Gamebase initialization failed.

            UpdateInfo updateInfo = UpdateInfo.from(exception);
            if (updateInfo != null) {
                // Unregistered game client version.
                // Open market url to update application.
                updateInfo.installUrl; // Market URL.
                updateInfo.message;    // Message from launching server.
                return;
            }

            // Another initialization error.
            ...
        }
        ...
    }
});
```

### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | Gamebase not initialized. |
| NOT_LOGGED_IN                | 2          | Login required.            |
| INVALID_PARAMETER            | 3          | Invalid parameter.           |
| INVALID_JSON_FORMAT          | 4          | JSON format error.          |
| USER_PERMISSION              | 5          | No permissions.               |
| NOT_SUPPORTED                | 10         | Function not supported.        |
| NOT_SUPPORTED_ANDROID        | 11         | Function not supported in Android.   |
| ANDROID_ACTIVEAPP_NOT_CALLED | 32         | The activeApp API has not been called.   |


* Refer to the following document for all error codes.
    * [Error Code](./error-code/#client-sdk)
