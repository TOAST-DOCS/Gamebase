## Game > Gamebase > JavaScript SDK User Guide > Initialization

To use the Gamebase JavaScript SDK, you should perform initialization first.

### Configuration Settings

When initializing Gamebase, you can change Gamebase settings using the GamebaseConfiguration object.

| KEY                                                 | Mandatory (M) / Optional (O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId: string| **M**                      | NHN Cloud Project ID. It is a mandatory field. |
| clientVersion: string| **M**                      | Determines the status (such as maintenance, in service, notice, and etc.), whether the game is playable or not, via the game version. <br/> `NHN Cloud Console > Gamebase > App > Client Version > Enter the WEB version. |
| enableDebugMode: boolean                            | O                          | Enables the Debug Mode. Debug log is displayed on the developer console. <br/> The default is **false**. |
| uiConfiguration.enablePopup: boolean                | O                          | **[UI]**<br/>When a game user cannot play games due to system maintenance or (ban) from use, the reason needs to be explained using popups or any other means.<br/> If it is set to **true**, Gamebase will automatically display information via popups.<br/> The default value is **false**. When the value is set to <br/>**false**, get information from launching results and display why user cannot play games by using customized UI. |
| uiConfiguration.enableLaunchingStatusPopup: boolean | O                          | **[UI]**<br/>When login is unavailable (mainly due to maintenance) depending on the launching results, you can decide whether to allow Gamebase to automatically display popups or not.<br/>**Works only when enablePopup(true)** is on.<br/>The default value is **true**. |
| uiConfiguration.enableBanPopup: boolean             | O                          | **[UI]**<br/>When a game user is banned, you can change whether to allow Gamebase to automatically display a popup on the reasons.<br/>**Works only when enablePopup(true)** is on.<br/>The default value is **true**. |
| displayLanguageCode: string| O                          | The language code used in the Gamebase UI. <br/> The default is **browser's language**(navigator.language). |
| displayLanguageTable: json                          | O                          | The (text resource) used for displaying Gamebase UI. <br/> This value is automatically merged with the embedded language set table for use. <br/> With `toast.Gamebase.getDisplayLanguageTable()`, you can check the embedded language set table and its format.|


#### GamebaseConfiguration Example
```javascript
var gamebaseConfiguration = {
    appId: 'T0asTC1oud',    // NHN Cloud Console Project ID
    clientVersion: '1.0.0', // NHN Cloud Console Gamebase App Client Version
    enableDebugMode: true,  // Note: Do not turn it on in production mode.
    uiConfiguration: {
        enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
        enableLaunchingStatusPopup: true,
        enableBanPopup: true,
    },
};
```


### Debug Mode
Setting for Gamebase debug.
* true: All logs of Gamebase are displayed.
* false: Warning (warning) and error (error) logs are displayed.
* Default: false

You can also perform the debug setting in the console and the values set in the console have priority.
Please see the following guide to set in the console.

* [Setting the console test device](./oper-app/#test-device)
* [Setting the console client](./oper-app/#client)


To turn on system logs for development reference, call **toast.Gamebase.setDebugMode(true)**.
> <font color="red">[Caution]</font><br/>
>
> Before **releasing ** a game, make sure to delete setDebugMode call from the source code or change the parameter to 'false'.




### Initialize

**toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ... )** is called to initialize Gamebase SDK.<br/>

```js
function gamebaseInitialize() {
    var gamebaseConfiguration = {
        appId: 'T0asTC1oud',    // NHN Cloud Console Project ID
        clientVersion: '1.0.0', // NHN Cloud Console Gamebase App Client Version
        enableDebugMode: true,  // Note: Do not turn it on in production mode.
        uiConfiguration: {
            enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
            enableLaunchingStatusPopup: true,
            enableBanPopup: true,
        },
    };  


    toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
        if (error) {
            // If initialization fails, you cannot use Gamebase SDK.
            // Make sure that settings of appId, clientVersion, and NHN Cloud Console have been correctly set.
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // For the status value, see the Launching Status Code table below.
            // Game can be played.
            console.log('Playable!');
        } else {
            // Game cannot be played.(maintenance, service terminated, etc.)
            console.log('Not Playable!');
        }
    });

    var isPlayable = function(statusCode) {
        if (statusCode >= 200 && statusCode < 300)
            return true;

        return false;
    };
}
```

### Launching Information

The launch status can be checked by looking at the result of calling toast.Gamebase.initialize.

```js
toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
    if (error) {
        console.log(error);
        return;
    }

	var statusCode = launchingInfo.launching.status.code;
});
```

The LaunchingInfo object can be acquired even after the initialization using toast.Gamebase.Launching.getLaunchingInformations() API.

> <font color="red">[Caution]</font><br/>
>
> The **toast.Gamebase.Launching.getLaunchingInformations()** is not an asynchronous API that retrieves information from the server in real time.
> It returns cached information updated every 2 minutes, so it is not suitable for real-time checking of the current status.
> In that case, use GamebaseEventHandler, which triggers an event when the Launching Status Code is changed.
> [Game > Gamebase > Javascript SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Observer](./js-etc/#observer)

**API**

```js
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
The LaunchingInfo object contains game status and settings configured in Gamebase Console.


#### 1. Launching

Gamebase launching information.

**1.1 Status**

The game status information of the app version used when initializing Gamebase JavaScript SDK.

* code: Game status code (under maintenance, update required, service terminated, etc.)
* message: Game status message

See the table below for status code:

#### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | Normal service status                                 |
| RECOMMEND_UPDATE            | 201  | Update recommended                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | The service is unavailable during maintenance, but if it is registered using a QA device, it can access the service and perform test regardless of maintenance period. |
| IN_TEST                     | 203  | Testing |
| IN_REVIEW                   | 204  | Reviewing |
| REQUIRE_UPDATE              | 300  | Update required                                  |
| BLOCKED_USER                | 301  | Accessed the service using the device of a blocked user (device key). |
| TERMINATED_SERVICE          | 302  | Service terminated                                   |
| INSPECTING_SERVICE          | 303  | Service in maintenance                                 |
| INSPECTING_ALL_SERVICES     | 304  | All services in maintenance                              |
| INTERNAL_SERVER_ERROR       | 500  | Internal server error                                 |

[Console Guide](/Game/Gamebase/en/oper-app/#app)

**1.2 App**

App information registered in the Gamebase Console.

* accessInfo
    * serverAddress: Server address
    * csInfo: Customer Center information
* relatedUrls
    * termsUrl: Terms and Conditions
    * personalInfoCollectionUrl: Personal information collection agreement
    * punishRuleUrl: User ban rules
    * csUrl: Customer Center
* install: Installation URL
* idP: Authentication information

[Console Guide](/Game/Gamebase/en/oper-app/#client)

**1.3 Maintenance**

Maintenance information registered in the Gamebase Console.

* url: Maintenance page URL
* timezone: Timezone
* beginDate: Start time
* endDate: End time
* message: Reason for maintenance

[Console Guide](/Game/Gamebase/en/oper-operation/#maintenance)

**1.4 Notice**

Notification information registered in the Gamebase Console.

* message: Message
* title: Title
* url: Maintenance URL

[Console Guide](/Game/Gamebase/en/oper-operation/#notice)

#### 2. tcProduct

The appKey of the NHN Cloud service associated with Gamebase.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

The IAP store information registered in the NHN Cloud Console.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/en/oper-purchase/)

#### 4. tcLaunching

Launching information registered in the NHN Cloud Console.

* The value entered by users in Console is sent as a JSON string.
 
[Console Guide](/Game/Gamebase/en/oper-management/#config)


### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | Gamebase is not initialized. |
| NOT_LOGGED_IN                | 2          | Requires login.            |
| INVALID_PARAMETER            | 3          | Invalid parameter.           |
| INVALID_JSON_FORMAT          | 4          | JSON format error.          |
| USER_PERMISSION              | 5          | No permissions.               |
| NOT_SUPPORTED                | 10         | The feature is not supported.        |


* For the entire list of error codes, see the following document.
    * [Error Code](./error-code/#client-sdk)
