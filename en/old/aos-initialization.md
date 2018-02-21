## Game > Gamebase > Android Developer's Guide  > Initialization

## Initialization

To use Gamebase Android SDK, initialization is required in the first place.
### Activate the Application

To manage app's lifecycle, Gamebase SDK should be notified of app's activation. <br/>
Call **Gamebase#activeApp(Context)** from **Application#onCreate()**.

```java
public class GamebaseApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Gamebase.activeApp(getApplicationContext());
    }
}
```

### Configuration Settings

When Gamebase is initialized, Gamebase setting can be modified with the GamebaseConfiguration.Builder object.

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| build()                                  | **M**                      | Convert Builder completed with setting to a configuration setting. <br/> Required for **Gamebase.initialize()** API. |
| setAppId(String appId)                   | **M**                      | Enter an app ID issued from TOAST Cloud Project. |
| setAppVersion(String appVersion)         | **M**                      | Status of update or maintenance can be decided upon a game version.<br/> Specify a game version. |
| setAppName(String appName)               | O                          | Specify a name of game to show on the PAYCO Login/Logout screen.<br/> If not specified, android:label value of application node of the AndroidManifest.xml file shall be applied. |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/> When a game user cannot play games due to system maintenance or ban of use, reasons need to be displayed by pop-ups.<br/>If it is set **true**, Gamebase will automatically display information pop-ups in such instances.<br/> **False** is set as default.<br/> When it is **false**, get information from launching results and display why user cannot play games on your own UI. |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/> Depending on the launching results, when a login is unavailable (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups. <br/> Works only when **enablePopup(true)** is on. <br/> **True** is set as default. |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/> When a game user is banned for use, you can change whether to allow Gamebase to automatically display a pop-up on the reasons. <br/> Works only when **enablePopup(true)**is on.<br/> **True** is set as default. |
| setStoreCode(String storeCode)           | O                          | **[Purchase]**<br/>If you're using IAP (In-App Purchase) of TOAST Cloud, you need to set which to store to use. <br/> For parameters, refer to the [IAP Document](http://docs.cloud.toast.com/ko/Common/IAP/ko/Overview/). |
| setFCMSenderId(String senderId)          | O                          | **[Push]**<br/>  In case push messages are sent via Google Notification(FCM, GCM), sender ID should be set. |
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O                          | **[Push]**<br/> In case of Tencent push modules, access keys and access ID should be set. |

### Debug Mode
* Gamebase shows warning and error logs only.
* To turn on system logs for the reference of development, call **Gamebase.setDebugMode(true)**.

> <font color="red">[Caution]</font><br/>
>
> To **release** a game, be sure to delete setDebugMode call from a source code or change the parameter to false before a build.

### Initialize

Call **Gamebase#initialize(Activity, GamebaseConfiguration, and GamebaseDataCallback)** from **Activity#onCreate(Bundle)** to initialize Gamebase SDK.<br/>
And, for normal operations of Gamebase, make sure to call **Gamebase.onActivityResult(int, int, Intent)** from **Activity#onActivityResult(int, int, Intent)**.

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Gamebase Configuration.
         */
        GamebaseConfiguration configuration =
                        new GamebaseConfiguration.Builder()
                                .setAppId("T0aStC1d")
                                .setAppVersion("1.0.0")
                                .enableLaunchingStatusPopup(true)
                                .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // Check launching status.
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // Play games.
                    } else {
                        // Requires maintenance or app updates.
                    }
                } else {
                    // If Initialization fails, cannot use Gamebase SDK.
                    // Display errors, and restart or close a game.
                    Log.e(TAG, "Initialize failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });

        /**
         * Show gamebase debug message.
		 * set 'false' when build RELEASE.
         */
        Gamebase.setDebugMode(true);
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

### Launching Status

Check launching status on the call result of Gamebase#initialize.
```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Check launching status.
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // Under maintenance...
                    break;
                ...
            }
            ...
        }
        ...
    }
});
```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Service is now normally provided         |
| RECOMMEND_UPDATE            | 201  | Update is recommended                    |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available |
| REQUIRE_UPDATE              | 300  | Update is required                       |
| BLOCKED_USER                | 301  | User whose access has been blocked       |
| TERMINATED_SERVICE          | 302  | Service has been terminated              |
| INSPECTING_SERVICE          | 303  | Under maintenance now                    |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service  |
| INTERNAL_SERVER_ERROR       | 500  | Error of internal server                 |
