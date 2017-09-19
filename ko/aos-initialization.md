## Game > Gamebase > Android Developer's Guide  > Initialization

## Initialization

### Activate the application

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

### Initialization

**Activity#onCreate(Bundle)**에서 **Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**을 호출하여 Gamebase SDK 초기화를 진행합니다.

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
        ...
    }
    ...
}
```

### Getting launching status

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

