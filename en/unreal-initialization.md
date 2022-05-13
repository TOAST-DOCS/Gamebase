## Game > Gamebase > User Guide for Unreal SDK > Initialization 

To use Unreal Gamebase SDK, initialization is required. In addition, app ID, and app version information must be registered on NHN Cloud Console. 

### Include Header File

To enable Gamebase API, import the following header file. 

```cpp
#include "Gamebase.h"
```

### FGamebaseConfiguration 

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
| enableKickoutPopup | ALL | O |

#### 1. App ID

Refers to Project ID registered on Gamebase Console.

[Game > Gamebase > Console Guide > App > App](./oper-app/#app)

#### 2. appVersion

Refers to Client Version registered on Gamebase Console. 

[Game > Gamebase > Console Guide > App > Client](./oper-app/#client)

#### 3. storeCode

Find store information as below, required to initialize NHN Cloud In-App Purchase. 

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | ONESTORE | only Android |
| Galaxy Store | GALAXY | only Android |

#### 4. displayLanguageCode

The language displayed on Gamebase UI and SystemDialog can be changed to languages other than language set on device.  

[Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Display Language](./unreal-etc/#display-language)

#### 5. enablePopup

Game users may be required to show reasons for not being able to play games due to system maintenance or user banned on a popup.  
This setting allows to enable default Gamebase popups. 

* True: Exposure of popups depends on the setting of enableLaunchingStatusPopup or enableBanPopup. 
* False: Do not show all Gamebase popups.
* Default: false

#### 6. enableLaunchingStatusPopup

This setting regards to using default Gamebase popups, when game is unavailable by LaunchingStatus.
See State, Code below the Launching paragraph to check LaunchingStatus. 

* Default: true

#### 7. enableBanPopup

This setting regards to using default Gamebase popups, when a game user is found, with login, to have been banned.  

* Default: true

#### 8. enableKickoutPopup

This setting regards to using default Gamebase popups, when a kickout event is received from the Gamebase Server. 

* Default: true


### Debug Mode

* Gamebase only displays the warning and error log.
* To turn on system logs for development reference, please call **IGamebase::Get().SetDebugMode(true)**.

> <font color="red">[Caution]</font><br/>
>
> Before **releasing** a game, make sure to delete SetDebugMode call from the source code or change the parameter to false before building.

Debug setting is also available in the Console, and this setting in the Console takes precedence over the other.
To find out how to set up the Console, see the following guide.

* [Setting Console test device](./oper-app/#test-device)
* [Setting Console Client](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void SetDebugMode(bool isDebugMode);
```

**Example**

```cpp
void Sample::SetDebugMode(bool isDebugMode)
{
    IGamebase::Get().SetDebugMode(isDebugMode);
}
```


### Initialize

Initialize SDKs. 

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
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
        
            // Following notices are registered in the Gamebase Console
            auto notice = launchingInfo->launching.notice;
            if (notice != null)
            {
                if (FString.IsNullOrEmpty(notice.message) == false)
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

With Unreal Gamebae SDK initialized by using Initialize API, the LaunchingInfo object is delivered as result value.  
This LaunchingInfo object includes values set on Gamebase console, as well as and game status. 

#### 1. Launching

Refers to Gamebase launching information. 

**1.1 Status**

Refers to game status information of the app version for the initialization setting of Unreal Gamebase SDK. 

* Code: Codes for game status (e.g. Under Maintenance, Update Required, or Service Closed) 
* Message: Message for game status 

See the below table for status codes: 

| Status                      | Code | Description                                    |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Under normal service |
| RECOMMEND_UPDATE            | 201  | Update is recommended |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Service is unavailable during maintenance; but on QA device, access to service is allowed for testing, regardless of maintenance. |
| IN_TEST                     | 203  | Under testing  |
| IN_REVIEW                   | 204  | Under review  |
| IN_BETA                     | 205  | Beta server environment |
| REQUIRE_UPDATE              | 300  | Upgrade is required  |
| BLOCKED_USER                | 301  | Service is accessed on device (device key) which is registered to be blocked from access. |
| TERMINATED_SERVICE          | 302  | Service is closed                                   |
| INSPECTING_SERVICE          | 303  | Service under maintenance                                |
| INSPECTING_ALL_SERVICES     | 304  | All services under maintenance                            |
| INTERNAL_SERVER_ERROR       | 500  | Error of internal server                                 |

[Game > Gamebase > Console Guide > App > App](./oper-app/#app)

**1.2 App**

Refers to app information registered on Gamebase Console. 

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

Refers to maintenance information registered on Gamebase Console.

* url: URL for maintenance page 
* timezone: Standard timezone (timezone)
* beginDate: Start time 
* endDate: End time 
* message: Cause of maintenance 

[Game > Gamebase > Console Guide > Operation > Maintenance](./oper-operation/#maintenance)

**1.4 Notice**

Information for notice registered on Gamebase Console.  

* message: Message
* title: Title
* url: URL for maintenance 

[Console Guide](./oper-operation/#notice)

#### 2. tcProduct

Refers to appkey of NHN Cloud service associated with Gamebase.  

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

Refers to IAP store information registered on NHN Cloud Console. 

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Game > Gamebase > Console Guide > Purchase](./oper-purchase/)

#### 4. tcLaunching

User-input information on the console of NHN Cloud Launching.  

* Deliver user-input value to JSON string. 
* Refer to the guide as below for detail setting of NHN Cloud Launching.  

[Game > Gamebase > Console Guide > Management > Config](./oper-management/#config)

### Get Launching Information

With GetLaunchingInformations API, you can get LaunchingInfo object even after initialization. 

> <font color="red">[Caution]</font><br/>
>
> The getLaunchingInformations API is not an asynchronous API that retrieves information from the server in real time.
> It returns cached information updated every 2 minutes, so it is not suitable for real-time checking of the current status.
> In that case, use GamebaseEventHandler, which triggers an event when the Launching Status Code is changed.
> [Game > Gamebase > Unreal SDK User Guide  > ETC > Additional Features > Gamebase Event Handler > Observer](./unreal-etc/#observer)

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
| NOT\_INITIALIZED      | 1          | Gamebase is not initialized.   |
| NOT\_LOGGED\_IN       | 2          | Requires a login.             |
| INVALID\_PARAMETER    | 3          | Invalid parameter.            |
| INVALID\_JSON\_FORMAT | 4          | Error of JSON format.          |
| USER\_PERMISSION      | 5          | Not authorized.             |
| NOT\_SUPPORTED        | 10         | Not supported.         |

* For the entire error codes, see the following document. 
    * [Error Codes](./error-code/#client-sdk)
