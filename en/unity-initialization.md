## Game > Gamebase > Unity Developer's Guide > Initialization

To use Gamebase Unity SDK, initialization is required, and App ID and app version should be registered in the NHN Cloud Console.

### GamebaseConfiguration 

Following settings are required for initialization.

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| storeCode | ALL | M |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| useWebViewLogin | Standalone | O |
| enableGPGSSignInCheck | Android | O | 

#### 1. App ID

Project ID registered in NHN Cloud.

[Game > Gamebase > Console Guide > App > App](./oper-app/#app)

#### 2. appVersion

Client version registered in NHN Cloud.

[Game > Gamebase > Console Guide > App > Client](./oper-app/#client)

#### 3. storeCode

Store information required to initialize In-App Purchase (IAP) of NHN Cloud.

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| ONE Store | ONESTORE | only Android |
| GALAXY Store | GALAXY | only Android |
| Huawei AppGallery | HUAWEI | only Android |
| My Card | MYCARD | only Android |
| Windows | WIN | only Unity Standalone |
| macOS | MAC | only Standalone |
| Web | WEB | only Unity WebGL|

#### 4. displayLanguageCode

The display language on the Gamebase UI and SystemDialog can be changed into another language, which is not set on a device, as the user wants. 

[Game > Gamebase > Unity SDK User Guide > ETC > Additional Features > Display Language](./unity-etc/#display-language)

#### 5. enablePopup

When a game user cannot play games due to system maintenance or banned from use, reasons need to be displayed by pop-ups.
This setting is related to applying default pop-ups provided by Gamebase SDK.

* True: Pop-ups are exposed depending on the setting of enableLaunchingStatusPopup and enableBanPopup.
* False: All pop-ups provided by Gamebase are not exposed.
* Default: False

#### 6. enableLaunchingStatusPopup

This setting is related to applying default pop-ups provided by Gamebase, when the LaunchingStatus is disabled to play games.
For LaunchingStatus, refer to Status/Code below Launching.

* Default: True

#### 7. enableBanPopup

This setting is related to applying default pop-ups provided by Gamebase, when the game user has been banned.

* Default: True

#### 8. useWebViewLogin

Set whether or not to log in to WebView on a (Standalone) platform.

#### 9. enableGPGSSignInCheck

Android 플랫폼에서 'GPGS 자동 로그인' 기능 연동시 유저에게 GPGS 로그인을 앱 설치 후 한번만 물어보는 설정입니다.

* true : 유저가 GPGS 로그인을 거부하더라도 Gamebase 초기화 때 GPGS 로그인 창을 다시 표시합니다.
* false : 앱 최초 실행시에만 GPGS 로그인 창이 한번 표시됩니다.
* 기본값: true

### Debug Mode
* Gamebase only displays the (warning) and error log.
* To turn on system logs for development reference, call **Gamebase.SetDebugMode(true)**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, make sure to delete SetDebugMode call from the source code or change the parameter to false before building.

Debug setting is also available in the Console, and this setting in the Console takes precedence over the other.
To find out how to set up the Console, see the following guide.

* [Setting Console test device](./oper-app/#test-device)
* [Setting Console Client](./oper-app/#client)

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

Initialize SDK.

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
        configuration.appVersion = "appVersion";
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_EDITOR_OSX || UNITY_STANDALONE_OSX
        configuration.storeCode = GamebaseStoreCode.MACOS;
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

When Gamebase Unity SDK is initialized by using Initialize API, LaunchingInfo object results will be delievered.
This LaunchingInfo object contains settings of the NHN Cloud Gamebase Console and game status.

#### 1. Launching

Launching information of Gamebase.

**1.1 Status**

Status information of game app version set in the Gamebase Unity SDK initialization.

* Code: Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
* Message: Game status message

For game status codes, refer to the table below.

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Service is provided normally                                 |
| RECOMMEND_UPDATE            | 201  | Update is recommended                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | The service cannot be used during maintenance, but if the device is registered as a QA device, you can access and test the service regardless of maintenance status. |
| IN_TEST                     | 203  | Test in progress |
| IN_REVIEW                   | 204  | Review in progress |
| IN_BETA                     | 205  | Beta server environment |
| REQUIRE_UPDATE              | 300  | Update is required                                  |
| BLOCKED_USER                | 301  | The service has been accessed with a device (device key) registered as access blocked. |
| TERMINATED_SERVICE          | 302  | Service has been terminated                                   |
| INSPECTING_SERVICE          | 303  | Service maintenance in progress                                 |
| INSPECTING_ALL_SERVICES     | 304  | The whole service maintenance in progress                              |
| INTERNAL_SERVER_ERROR       | 500  | Internal server error                                 |

[Game > Gamebase > Console Guide > App > App](./oper-app/#app)

**1.2 App**

App information registered in the NHN Cloud Console.

* accessInfo
    * serverAddress: Server address
* customerService
    * accessInfo : Customer Center contact information
    * type : Customer Center type
    * url : Customer Center URL
* relatedUrls
    * termsUrl: Terms of use
    * personalInfoCollectionUrl: Agreement to collection of personal information
    * punishRuleUrl: User ban rules
* install: Installation URL
* idP: Authentication information

[Game > Gamebase > Console Guide > App > Client](./oper-app/#client)

**1.3 Maintenance**

Maintenance information registered in the NHN Cloud Console is as follows.

* url: Maintenance page url
* timezone: Standard timezone (timezone)
* beginDate: Start time
* endDate: End time
* message: Purpose of maintenance
* hideDate: Whether to display maintenance start and end times

[Game > Gamebase > Console Guide > Operation > Maintenance](./oper-operation/#maintenance)

##### Change Default Maintenance HTML

If both the `enablePopup` and `enableLaunchingStatusPopup` values are `true`, a maintenance popup will be automatically displayed if the game is in maintenance status.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_popup_android_2.30.0.png)

If you click the **DETAILS** button here, the maintenance information is automatically displayed in a webview.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_webview_android_2.30.0.png)

If you want to modify the displayed HTML file, download the HTML file from the following link, modify it as you need, and place it in the 'Assets > StreamingAssets > Gamebase' folder. Then the HTML file will be used to display maintenance information instead of the default HTML file included in the Gamebase SDK.
[HTML file download link](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gamebase-maintenance.html)

**1.4 Notice**

Following notices are registered in the Gamebase Console.

* message: Messages
* title: Title
* url: Maintenance url

[Game > Gamebase > Console Guide > Operation > Notice](./oper-operation/#notice)

**1.5 user**

Below is the user information who initialized Gamebase.

* testDevice: Test device information (Forwarded when Status is in the 200s)
    * matchingFlag: Whether the user device matches the test device set in the Gamebase console
    * matchingTypes
        * Type matched with the test device information
        * Forwarded when matchingFlag is true
        
#### 2. tcProduct

Appkey of NHN Cloud Products linked to Gamebase.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

IAP store information registered in the NHN Cloud Console.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Game > Gamebase > Console Guide > Purchase](./oper-purchase/)

#### 4. tcLaunching

Refers to user-input information on NHN Cloud Launching Console.  

* Deliver user-input values via JSON string. 
* For detail setting of NHN Cloud Launching, see the guide as below. 
 
[Game > Gamebase > Console Guide > Management > Config](./oper-management/#config)

### Get Launching Information

With the GetLaunchingInformations API, you can get the LaunchingInfo object even after initialization.

> <font color="red">[Caution]</font><br/>
>
> The GetLaunchingInformations API is not an asynchronous API that retrieves information from the server in real time.
> It returns cached information updated every 2 minutes, so it is not suitable for real-time checking of the current status.
> In that case, use GamebaseEventHandler, which triggers an event when the Launching Status Code is changed.
> [Game > Gamebase > Unity SDK ser Guide > ETC > Additional Features > Gamebase Event Handler > Observer](./unity-etc/#observer)

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
 	 
By initializing GameClientVersion which is not registered on Gamebase console, error occurs like follows: **LAUNCHING_UNREGISTERED_CLIENT(2004)**.  
Under enablePopup(true), or enableLaunchingStatusPopup(true), popup shows for a forced update, and the user could be linked to the market.
If Gamebase popup is disabled, updates like market URL can be obtained from the GamsebaseError object. 

**VO**

```cs
public class UpdateInfo {
	// URL for store installation to download the latest version.
	string installUrl;
    // User can find a message in the language set on device.
    // When the language is 'ko', the message shows like follows.
    // 'This version is not supported. Please update to the latest version.'
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
    #elif UNITY_EDITOR_OSX || UNITY_STANDALONE_OSX
        configuration.storeCode = GamebaseStoreCode.MACOS;
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
| NOT\_INITIALIZED      | 1          | Gamebase not initialized. |
| NOT\_LOGGED\_IN       | 2          | Login required.            |
| INVALID\_PARAMETER    | 3          | Invalid parameter.           |
| INVALID\_JSON\_FORMAT | 4          | JSON format error.         |
| USER\_PERMISSION      | 5          | No permissions.              |
| NOT\_SUPPORTED        | 10         | Function not supported.         |

* Refer to the following document for all error codes.
    * [Error code](./error-code/#client-sdk)
