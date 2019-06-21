## Game > Gamebase > Unity SDK 사용 가이드 > 초기화

Gamebase Unity SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 또한 앱 ID, 앱 버전 정보가 TOAST Console에 반드시 등록돼 있어야 합니다.

### Inspector Settings

초기화 시 필요한 설정들은 아래와 같습니다.

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| isDebugMode | ALL | O |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| storeCode | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

Gamebase Console에 등록된 프로젝트 ID입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

#### 2. appVersion

Gamebase Console에 등록한 클라이언트 버전입니다.

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

#### 3. isDebugMode

Gamebase 디버그를 위한 설정입니다.

* true: Gamebase의 모든 로그가 출력됩니다.
* false: Warning, Error 로그가 출력됩니다.
* 기본값: false

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)

Gamebase 문의가 필요할 경우에는 해당 설정을 true 로 변경하시고 로그를 [고객 센터](https://toast.com/support/inquiry)로 전달해 주셔야 빠른 지원을 받을 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **RELEASE** 할 경우에는 해당 설정을 반드시 **false**로 변경해야 합니다.

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

#### 8. storeCode

TOAST 통합 인앱 결제 서비스인 IAP(In-App Purchase)를 초기화하기 위해 필요한 스토어 정보입니다.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | TS | only Android |

#### 9. fcmSenderId

Firebase Messaging(FCM) 사용을 위한 Sender ID입니다.

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

Standalone 플랫폼에서 WebView를 통해서 로그인을 할 것인지에 대한 설정입니다. 

#### 11. GamebaseUnitySDKSettings

위에서 설명한 설정들은 GamebaseUnitySDKSettings 컴포넌트의 Inspector에서 변경할 수 있습니다.

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.12.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDK를 초기화하는 방법은 다음과 같습니다.

1. 빈 게임 오브젝트를 생성합니다.
2. GamebaseUnitySDKSettings.cs 파일을 생성한 게임 오브젝트의 컴포넌트로 추가합니다.
3. Inspector에서 초기화 설정을 입력합니다.
4. Gamebase.Initialize(callback) API를 호출합니다.

> <font color="red">[주의]</font><br/>
>
> 생성한 게임 오브젝트를 삭제하면 Android, iOS API 호출 후 콜백을 받을 수 없으므로 주의하시기 바랍니다. <br/>
> 실수로 삭제된 경우 "Do not destroy this gameObject in order to receive callback." 오류가 나타납니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        Gamebase.Initialize((launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error))
            {
                Debug.Log("Gamebase initialization is succeeded.");

                if (IsPlayable(launchingInfo.launching.status.code))
                {
                    Debug.Log("Playable");
                }
                else
                {
                    if (launchingInfo.launching.status.code == GamebaseLaunchingStatus.REQUIRE_UPDATE)
                    {
                        Debug.Log(string.Format("message:{0}", launchingInfo.launching.status.message));
                    }
                }
            }
            else
            {
                Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
            }
        });
    }

    private bool IsPlayable(int status)
    {
        if (status >= 200 && status < 300)
            return true;

        return false;
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

Gamebaes Console에 등록된 공지 정보입니다.

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