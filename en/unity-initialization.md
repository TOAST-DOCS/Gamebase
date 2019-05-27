## Game > Gamebase > Unity Developer's Guide > Initialization

To use Gamebase Unity SDK, initialization is required, and App ID and app version should be registered in the TOAST Console.

### Inspector Settings

Following settings are required for initialization.

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

Project ID registered in TOAST.

[Console Guide](/Game/Gamebase/en/oper-app/#app)

#### 2. appVersion

Client version registered in TOAST.

[Console Guide](/Game/Gamebase/en/oper-app/#client)

#### 3. isDebugMode

Setting for Gamebase debug.

* True: All Gamebase logs.
* False: Warning and error logs.
* Default: False

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)

To inquire about Gamebase, change the setting to True and send logs to [Customer Center](https://toast.com/support/inquiry) for a quick response.

> <font color="red">[Caution]</font><br/>
>
> When **releasing** a game, make sure to change **parameter** to **false**.

#### 4. displayLanguageCode

The display language on the Gamebase UI and SystemDialog can be changed into another language, which is not set on a device, as the user wants. 

[Display Language](./unity-etc/#display-language)

#### 5. enablePopup

When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.
This setting regards to applying default pop-ups provided by Gamebase SDK.

* True: Pop-ups are exposed depending on the setting of enableLaunchingStatusPopup and enableBanPopup.
* False: All pop-ups provided by Gamebase are not exposed.
* Default: False

#### 6. enableLaunchingStatusPopup

This setting regards to applying default pop-ups provided by Gamebase, when the LaunchingStatus is disabled to play games.
For LaunchingStatus, refer to Status/Code below Launching.

* Default: True

#### 7. enableBanPopup

This setting regards to applying default pop-ups provided by Gamebase, when the game user has been banned.

* Default: True

#### 8. storeCode

Store information required to initialize In-App Purchase (IAP) of TOAST.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | TS | only Android |

#### 9. fcmSenderId

Sender ID to use FCM (Firebase Cloud Messaging).

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

Standalone 플랫폼에서 WebView를 통해서 로그인을 할 것인지에 대한 설정입니다. 

#### 11. GamebaseUnitySDKSettings

Settings described above can be modified at Inspector of the GamebaseUnitySDKSettings component.

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.12.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDK can be initialized as follows:

1. Create an empty game object.
2. Add the GamebaseUnitySDKSettings.cs file as a component of the game object.
3. Fill in the initialization setting at Inspector.
4. Call Gamebase.Initialize (callback) API.

> <font color="red">[Caution]</font><br/>
>
> Keep note that if a created game object is deleted, a callback cannot be received after a call of Android or iOS API. <br/>
> When it is deleted by mistake, "Do not destroy this gameObject in order to receive callback." error message will show.

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

When Gamebase Unity SDK is initialized by using Initialize API, LaunchingInfo object results will be delievered.
This LaunchingInfo object contains settings of the TOAST Gamebase Console and game status.

#### 1. Launching

Launching information of Gameabse.

**1.1 Status**

Status information of game app version set in the Gamebase Unity SDK initialization.

* Code: Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
* Message: Game status message

For game status codes, refer to the table below.

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | Service is now normally provided |
| RECOMMEND_UPDATE | 201 | Update is recommended |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | Under maintenance now but QA user service is available. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| REQUIRE_UPDATE | 300 | Update is required |
| BLOCKED_USER | 301 | User whose access has been blocked. |
| TERMINATED_SERVICE | 302 | Service has been terminated |
| INSPECTING_SERVICE | 303 | Under maintenance now |
| INSPECTING_ALL_SERVICES | 304 | Under maintenance for the whole service |
| INTERNAL_SERVER_ERROR | 500 | Error of internal server |

[Console Guide](/Game/Gamebase/en/oper-app/#app)


**1.2 App**

App information registered in the TOAST Console.

* accessInfo
    * serverAddress: Address of the server
    * csInfo: Customer center information
* relatedUrls
    * termsUrl: Terms of Use
    * personalInfoCollectionUrl: Agreement to Personal Information
    * punishRuleUrl: Rules of Punishment
    * csUrl: Customer Center
* install: Installation URL
* idP: ID Provider

[Console Guide](/Game/Gamebase/en/oper-app/#client)

**1.3 Maintenance**

Maintenance information registered in the TOAST Console is as follows.

* url: Maintenance page url
* timezone: Standard timezone (timezone)
* beginDate: Start time
* endDate: End time
* message: Purpose of maintenance

[Console Guide](/Game/Gamebase/en/oper-operation/#maintenance)

**1.4 Notice**

Following notices are registered in the Gamebase Console:

* message: Messages
* title: Title
* url: Maintenance url

[Console Guide](/Game/Gamebase/en/oper-operation/#notice)

#### 2. tcProduct

Appkey of TOAST Products linked to Gamebase.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

IAP store information registered in the TOAST Console.

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
