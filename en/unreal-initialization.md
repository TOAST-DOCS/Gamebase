## Game > Gamebase > User Guide for Unreal SDK 사용 가이드 > Initialization 초기화

Gamebase Unreal SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 또한 앱 ID, 앱 버전 정보가 TOAST Console에 반드시 등록돼 있어야 합니다. To use Unreal Gamebase SDK, initialization is required. In addition, app ID, and app version information must be registered on TOAST console. 

### Include Header File

To enable Gamebase API, import the following header file. 를 사용하기 위해서는 다음의 헤더 파일을 가져옵니다.

```cpp
#include "Gamebase.h"
```

### GamebaseConfiguration 

초기화 시 필요한 설정들은 아래와 같습니다. Following settings are required for initialization. 

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

#### 1. App ID

Refers to Project ID registered on Gamebase Console에 등록된 프로젝트 ID입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

#### 2. appVersion

Refers to Client Version registered on Gamebase Console. 에 등록한 클라이언트 버전입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

#### 3. storeCode

Store information required to initialize In-App Purchase. TOAST 통합 인앱 결제 서비스인 IAP(In-App Purchase)를 초기화하기 위해 필요한 스토어 정보입니다.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | ONESTORE | only Android |

#### 4. displayLanguageCode

Language change is available other than default language on device for Gamebase UI and SystemDialog.  Gamebase에서 제공하는 UI 및 SystemDialog에 표시되는 언어를 단말기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.

[Display Language](./unreal-etc/#display-language)

#### 5. enablePopup

Game users may be required to show reasons for not being able to play games due to system maintenance or banning on a popup.  시스템 점검, 이용 제재(ban) 등 게임 유저가 게임을 플레이할 수 없는 상황에서 팝업 등으로 사유를 표시해야 할 때가 있습니다.
This setting allows to enable default Gamebase popups.  Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* True: Exposure of popups depends on the setting of enableLaunchingStatusPopup or enableBanPopup. 설정에 따라 팝업이 노출 여부가 결정됩니다.
* False: Do not show all Gamebase popups. Gamebase에서 제공하는 모든 팝업이 노출되지 않습니다.
* Default기본값: false

#### 6. enableLaunchingStatusPopup

This setting regards to using default Gamebase popups, when game is unavailable by LaunchingStatus, 가 게임을 할 수 없는 상태일 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.
See State, Code below the Launching paragraph to check LaunchingStatus. 는 아래 Launching 절 아래 State, Code 부분을 참고하십시오.

* Default: true

#### 7. enableBanPopup

This setting regards to using default Gamebase popups, when a game user is found to be banned with login.  로그인 시 해당 게임 유저가 이용 정지 상태인 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* Default: true

#### 8. enableKickoutPopup

This setting regards to using default Gamebase popups, when a kickout event is received from the Gamebase Server. 로 부터 Kickout 이벤트를 받은 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* Default: true

### Initialize

Initialize SDKs. 를 초기화 합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Initialize(const FGamebaseConfiguration& configuration, const FGamebaseLaunchingInfoDelegate& onCallback);
```

**Example**

```cpp
void Sample::Initialize(const FString& appID, const FString& appVersion)
{
    FGamebaseConfiguration configuration{ "AppID", "AppVersion", "real", GamebaseDisplayLanguageCode.Korean, true, true, true, true, GamebaseStoreCode.Google, true };

    IGamebase::Get().Initialize(configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([=](const FGamebaseLaunchingInfo* launchingInfo, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
        
            // Following notices are registered in the Gamebase Console
            auto notice = launchingInfo->launching.notice;
            if (notice != null)
            {
                if (string.IsNullOrEmpty(notice.message) == false)
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("title: %s"), notice.title);
                    UE_LOG(GamebaseTestResults, Display, TEXT("message: %s"), notice.message);
                    UE_LOG(GamebaseTestResults, Display, TEXT("url: %s"), notice.url);
                }
            }
            
            // Status information of game app version set in the Gamebase Unreal SDK initialization.
            auto status = launchingInfo->launching.status;
    
            // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
            // refer to GamebaseLaunchingStatus
            if (status.code == GamebaseLaunchingStatus::IN_SERVICE)
            {
                // Service is now normally provided.
            }
            else
            {
                switch (status.code)
                {
                    case GamebaseLaunchingStatus::RECOMMEND_UPDATE:
                    {
                        // Update is recommended.
                        break;
                    }
                    // ... 
                    case GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR:
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
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

### Launching Information

With Unreal Gamebae SDK initialized by using Initialize API, the 를 사용하여 Gamebase Unreal SDK를 초기화하면 LaunchingInfo object is delivered as result value.  객체가 결과값으로 전달됩니다.
This LaunchingInfo object includes values set on Gamebase console, as well as and game status. 객체에는 Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.

#### 1. Launching

Refers to Gamebase launching information. 론칭 정보입니다.

**1.1 Status**

Refers to game status information of the app version for the initialization setting of Unreal Gamebase SDK.  초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* Code: Codes for game status (e.g. Under Maintenance, Update Required, or Service Closed)  게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* Message: Message for game status 게임 상태 메시지

See the below table for status codes: 상태 코드는 아래 표를 참고하십시오.

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | Under normal service 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | Upgrade is recommended업그레이드 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | Service is unavailable during maintenance; but on QA device, access to service is allowed for testing, regardless of maintenance점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트 할 수 있습니다. |
| IN_TEST                     | 203  | Under testing 테스트 중 |
| IN_REVIEW                   | 204  | Under review 심사 중 |
| REQUIRE_UPDATE | 300 | Upgrade is required 업그레이드 필수 |
| BLOCKED_USER                | 301         | Service is accessed on device (device key) which is registered to be blocked from access (접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302         | Service is closed  서비스 종료                                   |
| INSPECTING_SERVICE          | 303         | Service under maintenance 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304         | All systems under maintenance 전체 시스템 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500         | Error of internal server 내부 서버 오류                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

Refers to app information registered on Gamebase Console. 에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: Server address 
    * csInfo: Customer center information
* relatedUrls
    * termsUrl: Terms of Service  
    * personalInfoCollectionUrl: Consent to Personal Information 
    * punishRuleUrl: 이용 정지 규정 Punishment regulations 
    * csUrl : 고객센터 Customer Center 
* install: Installation URL
* idP: Authentication information 인증 정보

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

Refers to maintenance information registered on Gamebase Console에 등록된 점검 정보입니다.

* url: URL for maintenance page 점검 페이지 URL
* timezone: Standard timezone 표준 시간대(timezone)
* beginDate: Start time 시작 시간
* endDate: End time 종료 시간
* message: Cause of maintenance 점검 사유

[Console Guide](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

Gamebase Console에 등록된 공지 정보입니다. 

* message: Message
* title: Title
* url: URL for maintenance 

[Console Guide](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

Refers to appkey of TOAST service associated with Gamebase.  연계된 TOAST 서비스의 appKey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

Refers to IAP store information registered on TOAST Console. 에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

User-input information on the console of TOAST Launching.  콘솔에서 사용자가 입력한 정보입니다

* Deliver user-input value to JSON string. 사용자가 입력한 값을 JSON string으로 전달합니다.
* Refer to the guide as below for detail setting of TOAST Launching.  상세 설정은 아래 가이드를 참고하시기 바랍니다.
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)

### Get Launching Information

With GetLaunchingInformations API, you can get 를 이용하면 Initialize 이후에도 LaunchingInfo object even after initialization. 객체를 획득할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
const FGamebaseLaunchingInfoPtr GetLaunchingInformations() const;
```

**Example**

```cpp
void Sample::GetLaunchingInformations()
{
    auto launchingInformation = IGamebase::Get().GetLaunching().GetLaunchingInformations();
    if (launchingInformation.IsValid() == false)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found launching info."));
        return;
    }
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebase is not initialized.  초기화돼 있지 않습니다. |
| NOT\_LOGGED\_IN       | 2          | Requires a login. 로그인이 필요합니다.            |
| INVALID\_PARAMETER    | 3          | Invalid parameter. 잘못된 파라미터입니다.           |
| INVALID\_JSON\_FORMAT | 4          | Error of JSON format.  포맷 오류입니다.         |
| USER\_PERMISSION      | 5          | Not authorized. 권한이 없습니다.              |
| NOT\_SUPPORTED        | 10         | Not supported. 지원하지 않는 기능입니다.         |

* For the entire error codes, see the following document. 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Error Codes](./error-code/#client-sdk)
