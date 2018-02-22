## Game > Gamebase > Unity Developer's Guide > Initialization

To use Gamebase Unity SDK, initialization is required, and App ID and app version should be registered in the TOAST Console.

### Required Settings

Following settings are required for initialization.

| Setting value              | Supported OS |
| -------------------------- | ------------ |
| appID | ALL |
| appVersion | ALL |
| zoneType | ALL |
| isDebugMode | ALL |
| enablePopup | iOS, Android |
| enableLaunchingStatusPopup | iOS, Android |
| enableBanPopup | iOS, Android |
| storeCode | iOS, Android |
| fcmSenderId | Android |

#### 1. App ID

Project ID registered in TOAST.

[Console Guide](/Game/Gamebase/en/oper-app/#app)

#### 2. appVersion

Client version registered in TOAST.

[Console Guide](/Game/Gamebase/en/oper-app/#client)


#### 3. zoneType

Leave the value empty, as this setting is provided for external testing.

#### 4. isDebugMode

Setting for Gamebase debug.

* True: All Gamebase logs.
* False: Warning and error logs.
* Default: False

To inquire about Gamebase, change the setting to True and send logs to [Customer Center](https://toast.com/support/inquiry) for a quick response.

> <font color="red">[Caution]</font><br/>
>
> When **releasing** a game, make sure to change **parameter** to **false**.

#### 5. enablePopup

This setting regards to applying default pop-ups provided by Gamebase Mobile (iOS, Android) SDK.

* True: Pop-ups are exposed depending on the setting of enableLaunchingStatusPopup and enableBanPopup.
* False: All pop-ups provided by Gamebase are not exposed.
* Default: False

#### 6. enableLaunchingStatusPopup

This setting regards to applying default pop-ups provided by Gamebase, when the LaunchingStatus is disabled to play games. For LaunchingStatus, refer to Status/Code below Launching.

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

#### 10. GamebaseUnitySDKSettings

Settings described above can be modified at Inspector of the GamebaseUnitySDKSettings component.

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.4.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDK can be initialized as follows:

1. Create an empty game object.
2. Add the GamebaseUnitySDKSettings.cs file as a component of the game object.
3. Fill in the initialization setting at Inspector.
4. Call Gamebase.Initialize (callback) API.

> <font color="red">[Caution]</font><br/>
>
> Keep note that if a created game object is deleted, a callback cannot be received after a call of Android or iOS API. When it is deleted by mistake, <br/>
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

{@수정}Gamebaes Console에 등록된 공지 정보입니다.

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

[Console Guide](/Game/Gamebase/en/oper-purchase/)
