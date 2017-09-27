## Game > Gamebase > Android Developer's Guide  > Initialization

## Initialization

### Activate the Application

앱의 Lifecycle 관리를 위해 앱이 활성화 되었음을 Gamebase SDK에 알립니다.<br/>
**Application#onCreate()**에서 **Gamebase#activeApp(Context)**을 호출합니다.

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

Gamebase 초기화시 GamebaseConfiguration.Builder 객체를 통해 Gamebase 설정을 변경할 수 있습니다.

| API | Mandatory(M) / Optional(O) | 기능 설명 |
| --- | --- | --- |
| build() | **M** | 설정을 마친 Builder를 Configuration 객체로 변환합니다.<br/>**Gamebase.initialize()** API에서 필요로 합니다. |
| setAppId(String appId) | **M** | TOAST Cloud Project로 발급받은 AppId 를 입력합니다. |
| setAppVersion(String appVersion) | **M** | 업데이트, 점검에 해당하는지 여부는 게임 버전으로 판단합니다.<br/>게임 버전을 지정해 주세요. |
| setAppName(String appName) | O | Payco 로그인/로그아웃 시 화면에 표시될 게임의 이름을 지정합니다.<br/>설정하지 않은 경우, AndroidManifest.xml 파일의 application 노드에서 android:label 값을 가져와서 사용합니다. |
| enablePopup(boolean enable) | O | **[UI]**<br/>시스템 점검, 유저 밴 등 유저가 게임을 플레이 할 수 없는 상황에서 팝업 등을 통해 사유를 표시해야 할 필요가 있습니다.<br/>**true**로 설정하는 경우 Gamebase가 해당 상황에서 정보 팝업을 자동으로 표시합니다.<br/>기본값은 **false** 입니다.<br/>**false** 상태에서는 Launching 결과를 통해 정보를 획득하여 자체 UI를 구현하여 게임을 플레이 할 수 없는 이유를 표시해주시기 바랍니다. |
| enableLaunchingStatusPopup(boolean enable) | O | **[UI]**<br/>Launching결과에 따라 로그인 할 수 없는 상태에서(주로 점검 상태가 해당됩니다.) Gamebase가 자동으로 팝업을 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| enableBanPopup(boolean enable) | O | **[UI]**<br/>유저가 이용 제재를 당한 상태일때 Gamebase가 자동으로 제재 사유를 팝업으로 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| setStoreCode(String storeCode) | O | **[Purchase]**<br/>결제(Purchase 모듈)를 사용하신다면 어떤 마켓을 사용하는지 설정해야 합니다.<br/>Parameter는 Purchase문서를 참고해 주세요. |
| setFCMSenderId(String senderId) | O | **[Push]**<br/>Google Notification(FCM, GCM)을 통해 Push를 사용하는 경우 senderId값을 설정해야 합니다. |
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O | **[Push]**<br/>Tencent Push모듈을 사용하는 경우, AccessKey 및 AccessId 값을 설정해야 합니다. |

### Debug Mode
* Gamebase는 warning 및 error 로그만을 표시합니다.
* 개발에 참고하기 위해 시스템 로그를 켜기 위해서는 **Gamebase.setDebugMode(true)**를 호출해 주시기 바랍니다.

> <font color="red">[WARNING]</font><br/>
>
> 게임을 **RELEASE** 할 때는 반드시 소스코드에서 setDebugMode 호출을 제거하거나 파라메터를 false로 바꿔 빌드하세요.

### Initialize

**Activity#onCreate(Bundle)**에서 **Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**을 호출하여 Gamebase SDK 초기화를 진행합니다.<br/>
또한 Gamebase의 정상적인 동작을 위해 반드시 **Activity#onActivityResult(int, int, Intent)**에서 **Gamebase.onActivityResult(int, int, Intent)**를 호출합니다.

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
                    // 런칭 상태를 확인합니다.
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // 게임 플레이
                    } else {
                        // 점검 또는 앱을 업데이트 해야합니다.
                    }
                } else {
                    // 초기화에 실패하면 Gamebase SDK를 이용할 수 없습니다.
                    // 에러를 노출 후 게임을 재시작 또는 종료해야합니다.
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

Gamebase#initialize 호출 결과로 런칭 상태를 확인 할 수 있습니다.
```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 런칭 상태를 확인합니다.
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // 점검중...
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

| Status | Code | Description |
| --- | --- | --- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업데이트 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | 점검 중이지만 QA 유저 서비스 가능 |
| REQUIRE_UPDATE | 300 | 업데이트 필수 |
| BLOCKED_USER | 301 | 접속 차단 유저 |
| TERMINATED_SERVICE | 302 | 서비스 종료 |
| INSPECTING_SERVICE | 303 | 서비스 점검 중 |
| INSPECTING_ALL_SERVICES | 304 | 전체 서비스 점검 중 |
| INTERNAL_SERVER_ERROR | 500 | 내부 서버 에러 |

