## Game > Gamebase > Android SDK 사용 가이드 > 초기화

Gamebase Android SDK를 사용하려면 먼저 초기화를 진행해야 합니다.

### Activate the Application

앱의 수명 주기(lifecycle)를 관리하려면 앱이 활성화된 것을 Gamebase SDK에 알려야 합니다.<br/>
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

Gamebase를 초기화할 때, GamebaseConfiguration.Builder 객체로 Gamebase 설정을 변경할 수 있습니다.

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| Builder(String appId, String appVersion) | **M**                      | GamebaseConfiguration.Builder 생성자에 appId와 appVersion을 필수 파라미터로 넘겨주어 초기화해야합니다. <br/><br/> **appId**는 TOAST Project로 발급받은 앱 ID를 입력합니다. <br/> **appVersion**은 게임이 서비스 상태, 업데이트 상태 혹은 점검 상태 등에 해당하는지 판단하는 곳에 쓰입니다. 게임 버전을 지정해 주세요. |
| build()                                  | **M**                      | 설정을 마친 Builder를 Configuration 객체로 변환합니다.<br/>**Gamebase.initialize()** API에서 필요합니다. |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>시스템 점검, 이용 제재(ban) 등 게임 유저가 게임을 플레이할 수 없는 상황에서 팝업 등으로 사유를 표시해야 할 때가 있습니다.<br/>**true**로 설정하면 Gamebase가 해당 상황에서 정보 팝업을 자동으로 표시합니다.<br/>기본값은 **false**입니다.<br/>**false** 상태에서는 론칭 결과를 통해 정보를 획득한 후 자체 UI를 구현해 게임을 플레이할 수 없는 이유를 표시해 주시기 바랍니다. |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>론칭 결과에 따라 로그인할 수 없는 상태에서(주로 점검 상태) Gamebase가 자동으로 팝업을 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>게임 유저가 이용 제재를 당한 상태일 때 Gamebase가 자동으로 제재 사유를 팝업으로 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| setStoreCode(String storeCode)           | O                          | **[Purchase]**<br/>TOAST 통합 인앱 결제 서비스인 IAP(In-App Purchase)를 사용한다면 어떤 스토어를 사용할지 설정해야 합니다.<br/>파라미터는 [IAP 문서](/Mobile%20Service/IAP/ko/Overview/)를 참고하세요. |
| setFCMSenderId(String senderId)          | O                          | **[Push]**<br/>Google Notification(FCM, GCM)을 통해 푸시 메시지를 전송하는 경우 발신자 ID(sender ID)를 설정해야 합니다. |
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O                          | **[Push]**<br/>Tencent 푸시 모듈을 사용하는 경우, 액세스 키 및 액세스 ID 값을 설정해야 합니다. |

### Debug Mode
* Gamebase는 경고(warning)와 오류 로그만을 표시합니다.
* 개발에 참고할 수 있는 시스템 로그를 켜려면 **Gamebase.setDebugMode(true)**를 호출하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **릴리스**할 때는 반드시 소스 코드에서 setDebugMode 호출을 제거하거나 파라미터를 false로 바꿔서 빌드하세요.

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)


### Initialize

**Activity#onCreate(Bundle)**에서 **Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**을 호출하여 Gamebase SDK를 초기화합니다.<br/>
또한 Gamebase의 정상적인 동작을 위해 반드시 **Activity#onActivityResult(int, int, Intent)**에서 **Gamebase.onActivityResult(int, int, Intent)**를 호출합니다.

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
                    // 론칭 상태를 확인합니다.
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // 게임 플레이
                    } else {
                        // 점검 또는 앱을 업데이트해야 합니다.
                    }
                } else {
                    // 초기화에 실패하면 Gamebase SDK를 이용할 수 없습니다.
                    // 오류를 표시하고 게임을 재시작 또는 종료해야 합니다.
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

### Launching Information

Gamebase#initialize 호출 결과로 론칭 상태를 확인할 수 있습니다.

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 론칭 상태를 확인합니다.
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // 점검 중...
                    break;
                ...
            }
            ...
        }
        ...
    }
});
```

getLaunchingInformations() API를 이용하면 초기화 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
LaunchingInfo 객체에는 Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.


#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase Android SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

상태 코드는 아래 표를 참고하십시오.

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 정상 서비스 중                                 |
| RECOMMEND_UPDATE            | 201  | 업데이트 권장                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트할 수 있습니다. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| REQUIRE_UPDATE              | 300  | 업데이트 필수                                  |
| BLOCKED_USER                | 301  | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302  | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303  | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304  | 전체 서비스 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500  | 내부 서버 오류                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

Gamebase Console에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: 서버 주소
    * csInfo: 고객 센터 정보
* relatedUrls
    * termsUrl: 이용약관
    * personalInfoCollectionUrl: 개인 정보 동의
    * punishRuleUrl: 이용 정지 규정
    * csUrl : 고객센터
* install: 설치 URL
* idP: 인증 정보

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

Gamebase Console에 등록된 점검 정보입니다.

* url: 점검 페이지 URL
* timezone: 표준 시간대(timezone)
* beginDate: 시작 시간
* endDate: 종료 시간
* message: 점검 사유

[Console Guide](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

Gamebase Console에 등록된 공지 정보입니다.

* message: 메시지
* title: 타이틀
* url: 점검 URL

[Console Guide](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

Gamebase와 연계된 TOAST 서비스의 appKey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

TOAST Console에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

TOAST Launching Console에서 사용자가 입력한 정보입니다

* 사용자가 입력한 값을 JSON string으로 전달합니다.
* TOAST Launching 상세 설정은 아래 가이드를 참고해주시길 바랍니다.
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)




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
