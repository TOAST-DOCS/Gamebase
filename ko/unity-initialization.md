## Game > Gamebase > Unity Developer's Guide > Initialization

## Initialization

Gamebase Unity SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 또한 앱 ID, 앱 버전 정보가 TOAST Cloud Console에 반드시 등록돼 있어야 합니다.

### Required Settings

초기화 시 필요한 설정들은 아래와 같습니다.

| Setting value              | Supported OS |
| -------------------------- | ------------ |
| appID | ALL |
| appVersion | ALL |
| zoneType | ALL |
| idDebugMode | ALL |
| enablePopup | iOS, Android |
| enableLaunchingStatusPopup | iOS, Android |
| enableBanPopup | iOS, Android |
| storeCode | iOS, Android |
| fcmSenderId | Android |

#### 1. App ID

TOAST Cloud에 등록된 프로젝트 ID입니다.

![App ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_001_1.2.0.png)

#### 2. appVersion

TOAST Cloud에 등록한 클라이언트 버전입니다.

등록한 클라이언트 목록을 확인하려면 **Game > Gamebase > App**을 클릭한 후 **클라이언트** 탭을 클릭합니다.

![App Version](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_002_1.2.0.png)


#### 3. zoneType

외부 테스트를 위해 제공되고 있는 설정이므로 값을 입력하지 않도록 합니다.

#### 4. isDebugMode

Gamebase 디버그를 위한 설정입니다.

* true: Gamebase의 모든 로그가 출력됩니다.
* false: Warning, Error 로그가 출력됩니다.
* 기본값: false

Gamebase 문의가 필요할 경우에는 해당 설정을 true 로 변경하시고 로그를 [고객 센터](https://cloud.toast.com/support/faq)로 전달해 주셔야 빠른 지원을 받을 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **RELEASE** 할 경우에는 해당 설정을 반드시 **false**로 변경해야 합니다.

#### 5. enablePopup

Gamebase Mobile(iOS, Android) SDK에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* true: enableLaunchingStatusPopup, enableBanPopup 설정에 따라 팝업이 노출 여부가 결정됩니다.
* false: Gamebase에서 제공하는 모든 팝업이 노출되지 않습니다.
* 기본값: false

#### 6. enableLaunchingStatusPopup

LaunchingStatus가 게임을 할 수 없는 상태일 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.
LaunchingStatus는 아래 Launching 절 아래 State, Code 부분을 참고하십시오.

* 기본값: true

#### 7. enableBanPopup

로그인 시 해당 게임 이용자가 이용 정지 상태인 경우, Gamebase에서 제공하는 기본 팝업을 사용할 것인지에 대한 설정입니다.

* 기본값: true

#### 8. storeCode

TOAST Cloud의 통합 인앱 결제 서비스 상품인 IAP(In-App Purchase)를 초기화하기 위해 필요한 스토어 정보입니다.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | TS | only Android |

#### 9. fcmSenderId

Firebase Cloud Messaging(FCM) 사용을 위한 Sender ID입니다.

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. GamebaseUnitySDKSettings

위에서 설명한 설정들은 GamebaseUnitySDKSettings 컴포넌트의 Inspector에서 변경할 수 있습니다.

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.4.0.png)

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

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

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
이 LaunchingInfo 객체에는 TOAST Cloud Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.

#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase Unity SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

appVersion 추가 및 수정은 TOAST Cloud Console에서 할 수 있습니다.
TOAST Cloud Console에서 **Game > Gamebase > App**을 클릭한 후 **클라이언트** 탭을 클릭합니다.

![Launching Status](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_006_1.2.0.png)

상태 코드는 아래 표를 참고하십시오.

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업그레이드 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트 할 수 있습니다. |
| REQUIRE_UPDATE | 300 | 업그레이드 필수 |
| BLOCKED_USER                | 301         | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302         | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303         | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304         | 전체 시스템 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500         | 내부 서버 오류                                 |

**1.2 App**

TOAST Cloud Console에 등록된 앱 정보입니다.

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

TOAST Cloud Console에서 앱 정보를 설정하려면  **Game > Gamebase > App**을 클릭한 후 **앱** 탭을 클릭합니다.

![Launching App](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_005_1.2.0.png)

**1.3 Maintenance**

TOAST Cloud Console에 등록된 점검 정보입니다.

* url: 점검 페이지 URL
* timezone: 표준 시간대(timezone)
* beginDate: 시작 시간
* endDate: 종료 시간
* message: 점검 사유

TOAST Cloud Console에서 점검 정보를 등록하거나 수정하려면 **Game > Gamebase > Operation**을 클릭한 후 **점검** 탭을 클릭합니다.

![Launching Maintenance](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_007_1.2.0.png)

**1.4 Notice**

* message: 메시지
* title: 타이틀
* url: 점검 URL

TOAST Cloud Console에서 공지를 등록하거나 수정하려면 **Game > Gamebase > Operation**을 클릭한 후 **공지** 탭을 클릭합니다.

![Launching Notice](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_009_1.2.0.png)

#### 2. tcProduct

Gamebase와 연계된 TOAST Cloud 상품의 appKey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

TOAST Console에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code

TOAST Cloud Console에서 **Game > Gamebase > Purchase(IAP)**를 클릭한 후 **앱** 탭을 클릭합니다.

![Launching TC IAP](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_008_1.2.0.png)
