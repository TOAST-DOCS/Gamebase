## Game > Gamebase > Unity SDK 사용 가이드 > 초기화

Gamebase Unity SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 또한 앱 ID, 앱 버전 정보가 NHN Cloud Console에 반드시 등록돼 있어야 합니다.

### GamebaseConfiguration 

초기화 시 필요한 설정들은 아래와 같습니다.

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| storeCode | ALL | M |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| enableKickoutPopup | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

Gamebase Console에 등록된 프로젝트 ID입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

#### 2. appVersion

Gamebase Console에 등록한 클라이언트 버전입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

#### 3. storeCode

NHN Cloud 통합 인앱 결제 서비스인 IAP(In-App Purchase)를 초기화하기 위해 필요한 스토어 정보입니다.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| ONE Store | ONESTORE | only Android |
| GALAXY Store | GALAXY | only Android |
| Windows | WIN | only Unity Standalone |
| Web | WEB | only Unity WebGL and JavaScript |

#### 4. displayLanguageCode

Gamebase에서 제공하는 UI 및 SystemDialog에 표시되는 언어를 단말기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.

[Display Language](./unity-etc/#display-language)

#### 5. enablePopup

시스템 점검, 이용 제재(ban) 등 게임 유저가 게임을 플레이할 수 없는 상황에서 팝업 등으로 사유를 표시해야 할 때가 있습니다.
Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* true: enableLaunchingStatusPopup, enableBanPopup 설정에 따라 팝업이 노출 여부가 결정됩니다.
* false: Gamebase에서 제공하는 모든 팝업이 노출되지 않습니다.
* 기본값: false

#### 6. enableLaunchingStatusPopup

LaunchingStatus가 게임을 할 수 없는 상태일 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.
LaunchingStatus는 아래 Launching 절 아래 State, Code 부분을 참고하십시오.

* 기본값: true

#### 7. enableBanPopup

로그인 시 해당 게임 유저가 이용 정지 상태인 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* 기본값: true

#### 8. enableKickoutPopup

Gamebase Server로 부터 Kickout 이벤트를 받은 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* 기본값: true


#### 9. fcmSenderId

Firebase Messaging(FCM) 사용을 위한 Sender ID입니다.

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

Standalone 플랫폼에서 WebView를 통해서 로그인을 할 것인지에 대한 설정입니다. 

### Debug Mode
* Gamebase는 경고(warning)와 오류 로그만을 표시합니다.
* 개발에 참고할 수 있는 시스템 로그를 켜려면 **Gamebase.SetDebugMode(true)**를 호출하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **릴리스**할 때는 반드시 소스 코드에서 SetDebugMode 호출을 제거하거나 파라미터를 false로 바꿔서 빌드하세요.

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void SetDebugMode(bool isDebugMode)
```

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
}
```

### Initialize

SDK를 초기화 합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        /**
         * Show gamebase debug message.
         * set 'false' when build RELEASE.
         */
        Gamebase.SetDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        /**
         * Gamebase Initialize.
         */
        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("Initialization succeeded.");

                //Following notices are registered in the Gamebase Console
                var notice = launchingInfo.launching.notice;
                if (notice != null)
                {
                    if (string.IsNullOrEmpty(notice.message) == false)
                    {
                        Debug.Log(string.Format("title:{0}", notice.title));
                        Debug.Log(string.Format("message:{0}", notice.message));
                        Debug.Log(string.Format("url:{0}", notice.url));
                    }
                }
        
                //Status information of game app version set in the Gamebase Unity SDK initialization.
                var status = launchingInfo.launching.status;
        
                // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
                // refer to GamebaseLaunchingStatus
                if (status.code == GamebaseLaunchingStatus.IN_SERVICE)
                {
                    // Service is now normally provided.
                }
                else
                {
                    switch (status.code)
                    {
                        case GamebaseLaunchingStatus.RECOMMEND_UPDATE:
                        {
                            // Update is recommended.
                            break;
                        }
                        // ... 
                        case GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR:
                        {
                            // Error in internal server.
                            break;
                        }
                    }
                }
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Update is require.
                    }
                }
            }
        });
    }
}
```

### Launching Information

Initialize API를 사용하여 Gamebase Unity SDK를 초기화하면 LaunchingInfo 객체가 결과값으로 전달됩니다.
이 LaunchingInfo 객체에는 Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.

#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase Unity SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

상태 코드는 아래 표를 참고하십시오.

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업그레이드 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트 할 수 있습니다. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| IN_BETA                     | 205  | 베타 서버 환경 |
| REQUIRE_UPDATE | 300 | 업그레이드 필수 |
| BLOCKED_USER                | 301         | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302         | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303         | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304         | 전체 시스템 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500         | 내부 서버 오류                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

Gamebase Console에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: 서버 주소
* customerService
    * accessInfo : 고객센터 연락처
    * type : 고객센터 유형
    * url : 고객센터 URL
* relatedUrls
    * termsUrl: 이용약관
    * personalInfoCollectionUrl: 개인 정보 동의
    * punishRuleUrl: 이용 정지 규정
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

Gamebase와 연계된 NHN Cloud 서비스의 appKey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud Console에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launching 콘솔에서 사용자가 입력한 정보입니다

* 사용자가 입력한 값을 JSON string으로 전달합니다.
* NHN Cloud Launching 상세 설정은 아래 가이드를 참고하시기 바랍니다.
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)

### Get Launching Information

GetLaunchingInformations API를 이용하면 Initialize 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Launching.LaunchingInfo GetLaunchingInformations()
```

**Example**

```cs
public GamebaseResponse.Launching.LaunchingInfo GetLaunchingInformations()
{
    return Gamebase.Launching.GetLaunchingInformations();
}
```

### Handling Unregistered Version
 	 
Gamebase 콘솔에 등록되지 않은 GameClientVersion 을 초기화를 하면 **LAUNCHING_UNREGISTERED_CLIENT(2004)** 에러가 발생합니다.
enablePopup(true), enableLaunchingStatusPopup(true) 상태라면 강제 업데이트 팝업이 표시되고, 마켓으로 이동할 수 있습니다.
Gamebase 팝업을 사용하지 않을 경우에는 마켓 URL과 같은 Udpate정보를 GamebaseError 객체로부터 얻을 수 있습니다.

**VO**

```cs
public class UpdateInfo {
	// 최신 버전을 다운로드 할 수 있는 스토어 설치 URL.
	string installUrl;
    // 사용자에게 노출할 수 있는 메시지로 사용자의 단말기 언어에 맞게 전달됩니다.
    // 만일 언어가 'ko'인 경우 메시지는 아래와 같습니다.
    // '지원하지 않는 버전입니다. 최신 버전으로 업데이트해 주세요.'
    string message;
}
```

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
GamebaseResponse.Launching.UpdateInfo GamebaseResponse.Launching.UpdateInfo.From(GamebaseError error);
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                // Gamebase initialization succeeded.
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Unregistered game client version.
                        // Open market url to update application.
                        string installUrl = updateInfo.installUrl; // Market URL.
                        string message updateInfo.message; // Message from launching server.
                    }
                }
            }
        });
    }
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebase 초기화돼 있지 않습니다. |
| NOT\_LOGGED\_IN       | 2          | 로그인이 필요합니다.            |
| INVALID\_PARAMETER    | 3          | 잘못된 파라미터입니다.           |
| INVALID\_JSON\_FORMAT | 4          | JSON 포맷 오류입니다.         |
| USER\_PERMISSION      | 5          | 권한이 없습니다.              |
| NOT\_SUPPORTED        | 10         | 지원하지 않는 기능입니다.         |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)