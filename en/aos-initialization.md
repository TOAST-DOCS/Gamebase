## Game > Gamebase > Android Developer's Guide > Initialization

To use Gamebase Android SDK, initialization is required.

### Activate the Application

To manage app's lifecycle, Gamebase SDK should be notified of app's activation.<br/>
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

To initialize Gamebase, Gamebase setting can be modified with GamebaseConfiguration.Builder.

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| Builder(String appId, String appVersion) | **M**                      | GamebaseConfiguration.Builder 생성자에 appId와 appVersion을 필수 파라미터로 넘겨주어 초기화해야합니다. <br/><br/> **appId:** Enter an App ID issued from TOAST Cloud Project.<br/> **appVersion:** Status of update or maintenance can be decided upon a game version. Specify a game version. |
| build()                                  | **M**                      | Convert Builder completed with setting to a configuration object.<br/>Required for **Gamebase.initialize ()** API. |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.<br/>If it is set **true** , Gamebase will automatically display information via pop-ups.<br/>**false** is set as default.<br/>When set to **false** , get information from launching results and display why user cannot play games by using customized UI. |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>Depending on the launching results, when available to log in (mainly due to maintenance), you may decide whether to allow Gamebase to automatically display pop-ups.<br/>Works only when **enablePopup (true)** is on.<br/>**true** is set as default. |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>When game user is banned, you can change whether to allow Gamebase to automatically display a pop-up on the reasons.<br/>Works only when **enablePopup (true)** is on.<br/>**true** is set as default. |
| setStoreCode(String storeCode)           | O                          | **[Purchase]**<br/>Need to set which store to use for In-App Purchase (IAP).<br/>For parameters, refer to the [IAP Document](/Mobile%20Service/IAP/en/Overview/). |
| setFCMSenderId(String senderId)          | O                          | **[Push]**<br/>To send push messages via Google Notification (FCM, GCM), set a sender ID. |
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O                          | **[Push]**<br/>To use Tencent push modules, set an access key and access ID. |

### Debug Mode
* Gamebase shows warning and error logs only.
* To turn on system logs for the reference of development, call **Gamebase.setDebugMode(true)**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, be sure to delete 'setDebugMode' call from a source code or change the parameter to 'false'.

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)


### Initialize

Call **Gamebase#initialize(Activity, GamebaseConfiguration, and GamebaseDataCallback)** from **Activity#onCreate(Bundle)** to initialize Gamebase SDK.<br/>
And, for normal operations of Gamebase, make sure to call **Gamebase.onActivityResult(int, int, Intent)** from **Activity#onActivityResult(int, int, Intent)**.

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
         * Gamebase Configuration.
         */
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(appId, appVersion)
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
                    // If initialization fails, cannot use Gamebase SDK.
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

Check launching status by calling 'Gamebase#initialize()'.

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

getLaunchingInformations API를 이용하면 초기화 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```





### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Service is now normally provided                                 |
| RECOMMEND_UPDATE            | 201  | Update is recommended                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance now but QA user service is available. |
| REQUIRE_UPDATE              | 300  | Update is required                                  |
| BLOCKED_USER                | 301  | User whose access has been blocked |
| TERMINATED_SERVICE          | 302  | Service has been terminated                                   |
| INSPECTING_SERVICE          | 303  | Under maintenance now                                 |
| INSPECTING_ALL_SERVICES     | 304  | Under maintenance for the whole service                              |
| INTERNAL_SERVER_ERROR       | 500  | Error in internal server                                 |




### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | Gamebase 초기화돼 있지 않습니다. |
| NOT_LOGGED_IN                | 2          | 로그인이 필요합니다.            |
| INVALID_PARAMETER            | 3          | 잘못된 파라미터입니다.           |
| INVALID_JSON_FORMAT          | 4          | JSON 포맷 오류입니다.          |
| USER_PERMISSION              | 5          | 권한이 없습니다.               |
| NOT_SUPPORTED                | 10         | 지원하지 않는 기능입니다.        |
| NOT_SUPPORTED_ANDROID        | 11         | Android에서 지원하지 않는 기능입니다.   |
| ANDROID_ACTIVEAPP_NOT_CALLED | 32         | activeApp API가 호출되지 않았습니다.   |


* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
