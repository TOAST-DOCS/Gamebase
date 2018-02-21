## Game > Gamebase > Unity Developer's Guide > Initialization

## Initialization

To use Gamebase Unity SDK, initialization is required in the first place. In addition, App ID and version should be registered in the TOAST Cloud Console.
### Required Settings

Following settings are required for initialization.

| Setting value              | Supported OS |
| -------------------------- | ------------ |
| appID                      | ALL          |
| appVersion                 | ALL          |
| zoneType                   | ALL          |
| idDebugMode                | ALL          |
| enablePopup                | iOS, Android |
| enableLaunchingStatusPopup | iOS, Android |
| enableBanPopup             | iOS, Android |
| storeCode                  | iOS, Android |
| fcmSenderId                | Android      |

#### 1. App ID

Project ID registered in TOAST Cloud.

![App ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_001_1.2.0.png)

#### 2. appVersion

Client version registered in TOAST Cloud.

To check the list of registered clients, go to **Game > Gamebase > App** and click **Client**.

![App Version](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_002_1.2.0.png)


#### 3. zoneType

Leave the value empty, as this setting is provided for external testing.

#### 4. idDebugMode 

Setting for Gamebase debug.

* True: All Gamebase logs.
* False: Warning and error logs.
* Default: False

To inquire about Gamebase, change the setting to True and send logs to [Customer Center](https://cloud.toast.com/support/faq) for a quick resopnse.

> <font color="red">[Caution]</font><br/>
>
> To **RELEASE** a game, make sure to change setting to **false**.

#### 5. enablePopup

This setting regards to applying default pop-ups provided by Gamebase Mobile(iOS, Android) SDK.

* True: Pop-ups are exposed depending on the setting of enableLaunchingStatusPopup and enableBanPopup.
* False: All pop-ups provided by Gamebase are not exposed.

Default value: false

#### 6. enableLaunchingStatusPopup

This setting regards to applying default pop-ups provided by Gamebase, if the LaunchingStatus is unable to play games.
For LaunchingStatus, refer to State, Code below Launching.

* Default: True

#### 7. enableBanPopup

This setting regards to applying default pop-ups provided by Gamebase, if a game user has been banned, when he/she logs in.

* Default: True

#### 8. storeCode

Store information required to initialize In-App Purchase (IAP) of TOAST Cloud.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store   | AS   | only iOS     |
| Google Play | GG   | only Android |
| One Store   | TS   | only Android |

#### 9. fcmSenderId

Sender ID to use FCM (Firebase Cloud Messaging).

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. GamebaseUnitySDKSettings

Settings described in the above can be modified at Inspector of GamebaseUnitySDKSettings.

![GamebaseUnitySDKSettins Inspector](image/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.4.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDK can be initialized as follows:

1. Create an empty game object.
2. Add the GamebaseUnitySDKSettings.cs file as a component of the game object.
3. Fill in the initialization setting at at Inspector.
4. Call the Gamebase.Initialize(callback) API.

> <font color="red">[Caution]</font><br/>
>
> Keep note that if a created game object is deleted, a callback cannot be received after a call of Android or iOS API. <br/>
> When it is deleted by mistake, "Do not destroy this gameObject in order to receive callback." error message will show.

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

When Gamebase Unity SDK is initialized by using Initialize API, LaunchingInfo object is delievered as a result.
This LaunchingInfo object contains settings of the TOAST Cloud Gamebase Console and game status.

#### 1. Launching

Launching information of Gamebase.

**1.1 Status**

Status information of game of the app version set in the Gamebase Unity SDK initialization.

* code: Status code of a game (e.g. Under maintenance, Update is required, Service has been terminated)
* message: Message of game status

To add or modify appVersion, go to TOAST Cloud Console.
In the TOAST Cloud Console, click **Game > Gamebase > App** and click **Client**.

![Launching Status](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_006_1.2.0.png)

For game status codes, refer to the below table.

| Status                      | Status Code | Description                              |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE                  | 200         | Service is now normally provided         |
| RECOMMEND_UPDATE            | 201         | Update is recommended                    |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | Under maintenance now but QA user service is available |
| REQUIRE_UPDATE              | 300         | Update is required                       |
| BLOCKED_USER                | 301         | User whose access has been blocked       |
| TERMINATED_SERVICE          | 302         | Service has been terminated              |
| INSPECTING_SERVICE          | 303         | Under maintenance now                    |
| INSPECTING_ALL_SERVICES     | 304         | Under maintenance for the whole service  |
| INTERNAL_SERVER_ERROR       | 500         | Error of internal server                 |

**1.2 App**

App information registered in the TOAST Cloud Console.

* accessInfo
    * serverAddress: Address of the server
    * csInfo: Customer center information
* relatedUrls
    * termsUrl: Terms of Use
    * personalInfoCollectionUrl: Agreement to Personal Information
    * punishRuleUrl: Rules of Punishment
    * csUrl: Customer Center
* install: Installation URL
* idP: Authentication Information

To set app information in the TOAST Cloud Console, go to **Game > Gamebase > App** and click **App**.

![Launching App](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_005_1.2.0.png)

**1.3 Maintenance**

Maintenance information registered in the TOAST Cloud Console.

* url: URL of the maintenance page
* Timezone: Standard timezone (timezone)
* Start Date: Start time
* End Date: End time
* Message: Cause of maintenance

To register or modify maintenance information in the TOAST Cloud Console, go to **Game > Gamebase > Operation** and click **Maintenance**.

![Launching Maintenance](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_007_1.2.0.png)

**1.4 Notice**

* Message: Messages
* Title: Title
* url: Maintenance URL

To register or modify notification in the TOAST Cloud Console, go to **Game > Gamebase > Operation** and click *Maintenance**.

![Launching Notice](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_009_1.2.0.png)

#### 2. tcProduct

Appkey of TOAST Cloud Products linked to Gamebase.

* Gamebase
* tcLaunching
* Iap
* Push

#### 3. tcIap

IAP store information registered in the TOAST Console.

* ID: App ID
* Name: App Name
* StoreCode: Store Code

In the TOAST Cloud Console, go to **Game > Gamebase > Purchase(IAP)** and click **App**.

![Launching TC IAP](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_008_1.2.0.png)
