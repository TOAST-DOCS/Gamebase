## Game > Gamebase > Developer's Guide (Unity) > Initialization

## Initialization

Gamebase Unity SDK 를 사용하기 전에 초기화를 수행해야 하며, App ID, App Version 정보는 사전에 TOAST Cloud Console에 반드시 등록되어 있어야 합니다.

초기화 시 필요한 값들은 아래와 같습니다.

* appID : TOAST Cloud에 등록한 프로젝트 ID 입니다.
* appVersion : TOAST Cloud에 등록한 클라이언트 버전입니다.
* zoneType : "REAL" 또는 "" 만 사용하시길 바랍니다.
* idDebugMode : true일 경우 Gamebase의 모든 로그가 출력되고, false일 경우 Error 로그만 출력됩니다.
* enableLaunchingStatusPopup : true일 경우 LaunchingStatus에서 받은 서버 상태가 Gamebase에서 제공하는 기본팝업으로 노출됩니다.
* storeCode : 스토어 코드 입니다.
* fcmSenderId : Firebase Cloud Messaging (FCM) 사용을 위한 Sender ID 입니다.(Only Android.)


> [WARNING]
>
> zoneType은 "REAL" 또는 " " 으로 설정하십시오. (" " 설정 시 자동으로 "REAL"로 설정됩니다.) <br/>
> 그외 다른 값을 입력할 경우 정상 작동하지 않습니다.
>

### 1. Initialize UnitySDK using Inspector settings

1. 빈 게임 오브젝트를 생성합니다. (아래 그림의 GamebaseUnitySDK)
2. 생성한 게임 오브젝트에 GamebaseUnitySDKSettings.cs 파일을 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.
3. 추가 된 Component에서 appID, appVersion, zoneType, enableLaunchingStatusPopup 등을 입력합니다.
4. Gamebase.Initialize(callback) API를 호출합니다.


> [WARNING]
>
> 생성한 게임 오브젝트를 삭제하면 Android, iOS API 호출 후 콜백을 받을 수 없으므로 주의하시기 바랍니다. <br/>
> 실수로 삭제된 경우 "Do not destroy this gameObject in order to receive callback." 에러 메시지가 노출됩니다.
>

![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-Initialization_1.1.0.png)<br>
**그림. Inspector를 이용한 초기화**

**API**<br>
<div align="left">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)
</div> 

```cs
static void Initialize(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public void Initialize()
{
    Gamebase.Initialize((launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Gamebase initialization is succeeded.");

            if (GamebaseLaunchingStatus.IsPlayable(launchingInfo.launching.status.code))
            {
                Debug.Log("Playable");
            }
            else
            {
                if (launchingInfo.launching.status.code == GamebaseLaunchingStatus.REQUIRE_UPDATE)
                {
                    Debug.Log("RequireUpdate");
                }
            }
        }
        else
        {
        	Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
        }
    });
}
```

### 2. Initialize UnitySDK with parameters

인자를 이용한 초기화를 하기 위해서는 GamebaseRequest.GamebaseConfiguration 인스턴스에 값을 설정하여 Initialize API를 호출해야 합니다.
Android, iOS 플랫폼을 사용하는 경우에는 Native에서 Callback을 받기 위해 PluginCallbackManager를 GameObject에 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.

1. 빈 게임 오브젝트를 생성합니다.
2. 생성한 게임 오브젝트에 PluginCallbackManager.cs 파일을 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.
3. GamebaseRequest.GamebaseConfiguration 인스턴스에 appID, appVersion, zoneType을 설정합니다.
4. Gamebase.Initialize(configuration, callback) API 호출를 호출합니다.

**API**<br>
<div align="left">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)
</div> 

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public void Initialize()
{
    var configuration = new GamebaseRequest.GamebaseConfiguration();
    configuration.appID = GamebaseUnitySDK.AppID;
    configuration.appVersion = GamebaseUnitySDK.AppVersion;

    configuration.zoneType = "REAL";
    configuration.enableLaunchingStatusPopup = true; // Gamebase에서 기본으로 제공해주는 팝업 사용 여부
    configuration.storeCode = "GG";
    configuration.fcmSenderId = "fcmSenderId"; // Android Only

    Gamebase.Initialize(configuration, (launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Gamebase initialization is succeeded");

            if (GamebaseLaunchingStatus.IsPlayable(launchingInfo.launching.status.code))
            {
                Debug.Log("Playable");
            }
            else
            {
                if (launchingInfo.launching.status.code == GamebaseLaunchingStatus.REQUIRE_UPDATE)
                {
                    Debug.Log("RequireUpdate");
                }
            }
        }
        else
        {
        	Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
        }
    });
}
```

### Launching Info

Initialize API를 사용하여 UnitySDK를 초기화하면 LaunchingInfo 객체가 결과 값으로 전달됩니다.
이 LaunchingInfo 객체에는 TOASTCloud Gamebase console에 설정한 값들과 게임 상태등이 포함되어 있습니다.

* launching
	* status : 게임의 현재 상태 (점검 중, 업데이트 필수, 서비스 종료 등등)
	* app : 앱 관련 정보
	* maintenance : 점검 관련 정보
	* notice : 공지 관련 정보
* tcProduct : Gamebase와 연계된 TOASTCloud 상품의 appKey
* tcIap : IAP 정보

### Launching Status Code

LaunchingInfo 객체의 status.code 정보는 아래와 같습니다.

| Status | Status Code | Notes |
| --- | --- | --- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업그레이드 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | 점검중이지만 QA리스트로 인한 게임 접속 |
| REQUIRE_UPDATE | 300 | 업그레이드 필수 |
| BLOCKED_USER | 301 | 블랙리스트에 의한 접속 차단 |
| TERMINATED_SERVICE | 302 | 서비스 종료됨 |
| INSPECTING_SERVICE | 303 | 점검 |
| INSPECTING_ALL_SERVICES | 304 | 전체 시스템 점검 |
| INTERNAL_SERVER_ERROR | 500 | 내부 서버 에러 |

**GamebaseLaunchingStatus.CS 참고**


