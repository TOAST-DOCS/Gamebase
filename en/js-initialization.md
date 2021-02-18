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
    appId: 'T0asTC1oud',    // TOAST Console Project ID
    clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
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
        appId: 'T0asTC1oud',    // TOAST Console Project ID
        clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
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
            // Make sure that settings of appId, clientVersion, and TOAST Console have been correctly set.
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

### Launching Status

You can check the launching result by call toast.Gamebase.initialize.
```js
toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
    if (error) {
        console.log(error);
        return;
    }

	var statusCode = launchingInfo.launching.status.code;
});
```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | OK                                 |
| RECOMMEND_UPDATE            | 201  | Update recommended                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | Under maintenance, services are unavailable. However, if users have registered using QA devices, they can access the services for QA testing. |
| IN_TEST                     | 203  | Under test |
| IN_REVIEW                   | 204  | Review in progress |
| REQUIRE_UPDATE              | 300  | Update required                                  |
| BLOCKED_USER                | 301  | User whose access has been blocked, when the user has accessed the services using (device key). |
| TERMINATED_SERVICE          | 302  | Service terminated                                   |
| INSPECTING_SERVICE          | 303  | Under maintenance                                 |
| INSPECTING_ALL_SERVICES     | 304  | Whole service under maintenance                              |
| INTERNAL_SERVER_ERROR       | 500  | Internal server error                                 |
