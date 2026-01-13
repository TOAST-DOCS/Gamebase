## Game > Gamebase > Unreal SDK 사용 가이드 > 초기화

Gamebase Unreal SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 또한 앱 ID, 앱 버전 정보가 NHN Cloud Console에 반드시 등록돼 있어야 합니다.


### Include Header File

Gamebase API를 사용하기 위해서는 다음의 헤더 파일을 가져옵니다.

```cpp
#include "GamebaseSubsystem.h"
```

### FGamebaseConfiguration 

초기화 시 필요한 설정들은 아래와 같습니다.

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| AppID | ALL | M | 
| AppVersion | ALL | M |
| StoreCode | ALL | M |
| bEnablePopup | ALL | O |
| bEnableLaunchingStatusPopup | ALL | O |
| bEnableBanPopup | ALL | O |
| bEnableGPGSSignInCheck | Android | O |

#### 1. AppID

Gamebase Console에 등록된 프로젝트 ID입니다.

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > App](./oper-app/#app)


#### 2. AppVersion

Gamebase Console에 등록한 클라이언트 버전입니다.

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > Client](./oper-app/#client)

#### 3. StoreCode

NHN Cloud 통합 인앱 결제 서비스인 IAP(In-App Purchase)를 초기화하기 위해 필요한 스토어 정보입니다.

| Store       | Code | GamebaseStoreCode | Description  |
| ----------- | ---- | ------------ | ------------ |
| App Store | AS | GamebaseStoreCode::AppStore | iOS에 한함 |
| Google Play | GG | GamebaseStoreCode::Google | Android, Windows에 한함 |
| One Store | ONESTORE | GamebaseStoreCode::OneStore | Android에 한함 |
| Galaxy Store | GALAXY | GamebaseStoreCode::Galaxy | Android에 한함 |
| Huawei AppGallery | HUAWEI | GamebaseStoreCode::Huawei | Android에 한함 |
| MyCard | MYCARD | GamebaseStoreCode::MyCard | Android에 한함 |
| Windows Custom | WIN | GamebaseStoreCode::Windows | Windows에 한함 |
| Epic Games Store | EPIC | GamebaseStoreCode::EpicGames | Windows에 한함 |
| Steam | STEAM | GamebaseStoreCode::Steam | Windows에 한함 |

#### 4. bEnablePopup

시스템 점검, 이용 제재(ban) 등 게임 유저가 게임을 플레이할 수 없는 상황에서 팝업 창 등으로 사유를 표시해야 할 때가 있습니다.
Gamebase에서 제공하는 기본 팝업 창을 사용할 것인지에 대한 설정입니다.

* true: enableLaunchingStatusPopup, enableBanPopup 설정에 따라 팝업 창 노출 여부가 결정됩니다.
* false: Gamebase에서 제공하는 모든 팝업 창이 노출되지 않습니다.
* 기본값: false

#### 5. bEnableLaunchingStatusPopup

LaunchingStatus가 게임을 할 수 없는 상태일 경우, Gamebase에서 제공하는 기본 팝업 창을 사용할 것인지에 대한 설정입니다.
LaunchingStatus는 아래 Launching 절 아래 State, Code 부분을 참고하십시오.

* 기본값: true

#### 6. bEnableBanPopup

로그인 시 해당 게임 유저가 이용 정지 상태인 경우, Gamebase에서 제공하는 기본 팝업 창을 사용할 것인지에 대한 설정입니다.

* 기본값: true

#### 7. bEnableGPGSSignInCheck

Android 플랫폼에서 'GPGS 자동 로그인' 기능 연동 시 유저에게 GPGS 로그인을 앱 설치 후 한번만 물어보는 설정입니다.

* true: 유저가 GPGS 로그인을 거부하더라도 Gamebase 초기화 때 GPGS 로그인 창을 다시 표시합니다.
* false: 앱 최초 실행 시에만 GPGS 로그인 창이 한번 표시됩니다.
* 기본값: true

### Debug Mode

* Gamebase는 경고(warning)와 오류 로그만을 표시합니다.
* 개발에 참고할 수 있는 시스템 로그를 켜려면 **GamebaseSubsystem->SetDebugMode(true)**를 호출하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **릴리스**할 때는 반드시 소스 코드에서 SetDebugMode 호출을 제거하거나 파라미터를 false로 바꿔서 빌드하세요.

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetDebugMode(bool bIsDebugMode);
```

**Example**

```cpp
void USample::SetDebugMode(bool bIsDebugMode)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->SetDebugMode(bIsDebugMode);
}
```

### Initialize

SDK를 초기화합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Initialize(const FGamebaseConfiguration& Configuration, const FGamebaseLaunchingInfoDelegate& Callback);
```

**Example**

```cpp
void USample::Initialize(const FString& AppID, const FString& AppVersion)
{
    FGamebaseConfiguration Configuration;
    Configuration.AppID = AppID;
    Configuration.AppVersion = AppVersion;
    Configuration.StoreCode = GamebaseStoreCode::Google;
    Configuration.bEnablePopup = true;
    Configuration.bEnableLaunchingStatusPopup = true;
    Configuration.bEnableBanPopup = true;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Initialize(Configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([](const FGamebaseLaunchingInfo* LaunchingInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
        
            // Following notices are registered in the Gamebase Console
            auto Notice = LaunchingInfo->Launching.Notice;
            if (Notice != null)
            {
                if (string.IsNullOrEmpty(Notice.message) == false)
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("title: %s"), Notice.title);
                    UE_LOG(GamebaseTestResults, Display, TEXT("message: %s"), Notice.message);
                    UE_LOG(GamebaseTestResults, Display, TEXT("url: %s"), Notice.url);
                }
            }
            
            // Status information of game app version set in the Gamebase Unreal SDK initialization.
            auto Status = LaunchingInfo->Launching.Status;
    
            // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
            // refer to GamebaseLaunchingStatus
            if (Status.Code == GamebaseLaunchingStatus::IN_SERVICE)
            {
                // Service is now normally provided.
            }
            else
            {
                switch (Status.Code)
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
            // Check the Error code and handle the Error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

### Launching Information

Initialize API를 사용하여 Gamebase Unreal SDK를 초기화하면 LaunchingInfo 객체가 결괏값으로 전달됩니다.
이 LaunchingInfo 객체에는 Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.

#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase Unreal SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

상태 코드는 아래 표를 참고하십시오.

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 정상 서비스 중                                 |
| RECOMMEND_UPDATE            | 201  | 업데이트 권장                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트할 수 있습니다. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| IN_BETA                     | 205  | 베타 서버 환경 |
| REQUIRE_UPDATE              | 300  | 업데이트 필수                                  |
| BLOCKED_USER                | 301  | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302  | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303  | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304  | 전체 서비스 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500  | 내부 서버 오류                                 |

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > App](./oper-app/#app)

**1.2 App**

Gamebase Console에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: 서버 주소
* customerService
    * accessInfo : 고객 센터 연락처
    * type : 고객 센터 유형
    * url : 고객 센터 URL
* relatedUrls
    * termsUrl: 이용 약관
    * personalInfoCollectionUrl: 개인 정보 동의
    * punishRuleUrl: 이용 정지 규정
* install: 설치 URL
* idP: 인증 정보

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > Client](./oper-app/#client)

**1.3 Maintenance**

Gamebase Console에 등록된 점검 정보입니다.

* url: 점검 페이지 URL
* timezone: 표준 시간대(timezone)
* beginDate: 시작 시간
* endDate: 종료 시간
* message: 점검 사유
* hideDate: 점검 시작, 종료 시간을 표시할 것인지 여부

[Game > Gamebase > 콘솔 사용 가이드 > 운영 > Maintenance](./oper-operation/#maintenance)

**1.4 Notice**

Gamebase Console에 등록된 공지 정보입니다.

* message: 메시지
* title: 타이틀
* url: 점검 URL

[Game > Gamebase > 콘솔 사용 가이드 > 운영 > Notice](./oper-operation/#Notice)

#### 2. tcProduct

Gamebase와 연계된 NHN Cloud 서비스의 Appkey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud Console에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Game > Gamebase > 콘솔 사용 가이드 > 결제](./oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launching 콘솔에서 사용자가 입력한 정보입니다

* 사용자가 입력한 값을 JSON string으로 전달합니다.
* NHN Cloud Launching 상세 설정은 아래 가이드를 참고하시기 바랍니다.
 
[Game > Gamebase > 콘솔 사용 가이드 > 관리 > Config](./oper-management/#config)

### Get Launching Information

GetLaunchingInformations API를 이용하면 Initialize 이후에도 LaunchingInfo 객체를 얻을 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> GetLaunchingInformations API 는 실시간으로 서버에서 정보를 가져오는 비동기 API가 아닙니다.
> 2분 주기로 업데이트 되는 캐시 정보를 반환하므로, 실시간으로 현재의 점검 여부를 판단하는 용도로는 적합하지 않습니다.
> 이런 경우에는 Launching Status Code가 변경되었을때 이벤트가 동작하는 GamebaseEventHandler 를 활용하시기 바랍니다.
> [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Observer](./unreal-etc/#observer)

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
const FGamebaseLaunchingInfoPtr GetLaunchingInformations() const;
```

**Example**

```cpp
void USample::GetLaunchingInformations()
{
    auto LaunchingInformation = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLaunching().GetLaunchingInformations();
    if (LaunchingInformation.IsValid() == false)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found launching info."));
        return;
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
    * [오류 코드](./Error-code/#client-sdk)