## Game > Gamebase > JavaScript SDK User Guide > ETC

## Additional Features

Additional functions provided by Gamebase are described as below:

### Display Language
Similar to the Maintenance popup, the language used by the device will be displayed as the Gamebase language.

However, there are games that may use a language different from the device language with separate options.
For example, if the language configured for the device is English and you changed the game language to Japanese, the language displayed will be still English, even though you might want to see Japanese on the Gamebase screen.

For this, Gamebase provides a Display Language feature for applications that want to use a language that is not the language configured by the device for Gamebase.

Gamebase displays its messages in the language set in Display Language.
The language code entered for Display Language should be one of the codes listed in the table (**Types of language codes supported by Gamebase) below:

> <font color="red">[Caution]</font><br/>
>
> * Use Display Language only when you want to change the language displayed in Gamebase to a language other than the one configured by the device.
> * Display Language Code is a case-sensitive value in the form of ISO-639.
> There could be a problem if it is configured as a value such as 'EN' or 'zh-cn'.
> * If the value entered as Display Language Code does not exist in the table (**Types of language codes supported by Gamebase) below, Display Language Code is automatically set to English (en) by default.

> [Note]
>
> * As the client messages of Gamebase include only English (en), Korean (ko), and Japanese (ja), if you try to set a language other than English (en), Korean (ko), or Japanese (ja), even though the language code might be listed in the table below, the value is automatically set to English (en) by default.
> * You can manually add a language set that is not included in the Gamebase client.
> See the **Add New Language Set** section.

#### Types of Language Codes Supported by Gamebase

| Code     | Name                      |
| -------- | ------------------------- |
| de       | German                    |
| en       | English                   |
| es       | Spanish                   |
| fi       | Finish                    |
| fr       | French                    |
| id       | Indonesian                |
| it       | Italian                   |
| ja       | Japanese                  |
| ko       | Korean                    |
| ms       | Malay                     |
| pt       | Portuguese                |
| ru       | Russian                   |
| th       | Thai                      |
| vi       | Vietnamese                |
| zh-CN    | Chinese-Simplified        |
| zh-TW    | Chinese-Traditional       |

Each language code is defined in the `toast.GamebaseDisplayLanguage.DefaultCode` constant.

```js
var toast.GamebaseDisplayLanguage.DefaultCode = {
    German: 'de',
    English: 'en',
    Spanish: 'es',
    Finish: 'fi',
    French: 'fr',
    Indonesian: 'id',
    Italian: 'it',
    Japanese: 'ja',
    Korean: 'ko',
    Portuguese: 'pt',
    Russian: 'ru',
    Thai: 'th',
    Vietnamese: 'vn',
    Malay: 'ms',
    Chinese_Simplified: 'zh-CN',
    Chinese_Traditional: 'zh-TW',
};
```

#### Set Display Language at Gamebase Initialization

Display Language can be set when Gamebase is initialized.

**API**

```js
var gamebaseConfiguration = {
    appId: 'T0asTC1oud',    // NHN Cloud Console Project ID
    clientVersion: '1.0.0', // NHN Cloud Console Gamebase App Client Version
    displayLanguageCode: toast.GamebaseDisplayLanguage.DefaultCode.English,
};
toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ... });
```

**Example**

```js
function initialize() {
    var gamebaseConfiguration = {
        appId: 'T0asTC1oud',
        clientVersion: '1.0.0',
        displayLanguageCode: toast.GamebaseDisplayLanguage.DefaultCode.English,
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
            // Game cannot be played. (maintenance, service terminated, etc.)
            console.log('Not Playable!');
        }
    });
}
```

#### Set Display Language

You can change the initial setting of Display Language set in Gamebase initialization.

**API**

```js
var languageCode = '${ISO-639 LanguageCode}';
toast.Gamebase.setDisplayLanguageCode(languageCode);
```

**Example**

```js
function setDisplayLanguageCode() {
    var languageCode = toast.GamebaseDisplayLanguage.DefaultCode.English;
    toast.Gamebase.setDisplayLanguageCode(languageCode);
}
```

#### Get Display Language

You can retrieve the current application of Display Language.

**API**

```js
toast.Gamebase.getDisplayLanguageCode();
```

**Example**

```js
function getDisplayLanguageCode() {
    const language = toast.Gamebase.getDisplayLanguageCode();
    console.log(language);
}
```

#### Add New Language Sets

To use another language in addition to default Gamebase languages (en, ko, ja), you have to create and enter a JSON object for other language sets.
To enter the language sets in Gamebase, call the following API and enter the following:

**API**

```js
// When try to initialize Gamebase
var displayLanguageTable = {
    en: {
        common_ok_button: 'Customized OK Text',
        common_cancel_button: 'Customized Cancel Text',
    }, ...
};
var gamebaseConfiguration = { ..., displayLanguageTable, ... };
toast.Gamebase.initialize(gamebaseConfiguration, (lanchingInfo, error) => { ... });

// After Gamebase was initialized.
toast.Gamebase.setDisplayLanguageTable(displayLanguageTable) {
```

By default, the localized string embedded in Gamebase is as follows:
**Format**
```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
}
```

To add another language sets, add the value in the format of
`"${language code}":{"key":"value"}` as the parameter passed to the higher API.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
  "${Language code}": {
      "common_ok_button": "...",
      ...
  }
}
```

If a key is missing in ${language code}":{ } of the JSON format above, the language set for the device or 'en' will be automatically entered.

#### Priority in Display Language

If Display Language is set via initialization and setDisplayLanguageCode API, the final application may be different from what has been entered.

1. Check if the languageCode you enter is defined in the localized string.
2. See if, during Gamebase initialization, the language code set for the device is defined in the localized string. (This value shall maintain even if the language set for the device changes after initialization.)
3. 'en', which is the default value of Display Language, is automatically set.

### Gamebase Event Handler

* Gamebase can process all kinds of events in a single event system called **GamebaseEventHandler**.
* GamebaseEventHandler can simply add or remove a Listener through the API below:

**API**

```cs
toast.Gamebase..addEventHandler(eventHandler);
toast.Gamebase..removeEventHandler(eventHandler);
toast.Gamebase..removeAllEventHandler();
```

**Example**

```javascript
toast.Gamebase.addEventHandler((message) => {
    const category = message.category;
    const data = message.data;
    switch (category) {
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
            // Kicked out from Gamebase server.(Maintenance, banned or etc..)
            // Return to title and initialize Gamebase again.
            break;
        case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
            // If the user wants to move the guest account to another device,
            // if the account transfer is successful,
            // the login of the previous device is released,
            // so go back to the title and try to log in again.
            break;
        case GamebaseEventCategory.OBSERVER_LAUNCHING:
            checkLaunchingStatus(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_NETWORK:
            checkNetworkStatus(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_HEARTBEAT:
            checkHeartbeat(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_INTROSPECT:
            // Introspect error
            var observerData = JSON.parse(message.data);
            var errorCode = observerData.code;
            var errorMessage = observerData.message;
            break;
    }
});
```

* Category is defined in the GamebaseEventCategory class.

#### Server Push

* This is a message sent from the Gamebase server to the client's device.
* The Server Push Types supported from Gamebase are as follows:
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* If you register a kickout ServerPush message in **Operation > Kickout** of the NHN Cloud Gamebase Console, then all clients connected to Gamebase will receive the kickout message.
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* If the guest account is successfully transferred to another device, the previous device receives a kickout message.

**Example**

```javascript
toast.Gamebase.addEventHandler((message) => {
    const category = message.category;
    const data = message.data;
    switch (category) {
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
            // Kicked out from Gamebase server.(Maintenance, banned or etc..)
            // Return to title and initialize Gamebase again.
            break;
        case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
            // If the user wants to move the guest account to another device,
            // if the account transfer is successful,
            // the login of the previous device is released,
            // so go back to the title and try to log in again.
            break;
        default:
            break;
    }
});
```

#### Observer

* It is a system used to handle many different status-changing events in Gamebase.
* The Observer Types supported by Gamebase are as follows:
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* It operates when the Launching status is changed, for instance when the server is under maintenance, or the maintenance is over, or a new version is deployed and update is required.
    	* GamebaseEventObserverData.code: Indicates the LaunchingStatus value.
            * LaunchingStatus.IN_SERVICE: 200
            * LaunchingStatus.RECOMMEND_UPDATE: 201
            * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
            * LaunchingStatus.REQUIRE_UPDATE: 300
            * LaunchingStatus.BLOCKED_USER: 301
            * LaunchingStatus.TERMINATED_SERVICE: 302
            * LaunchingStatus.INSPECTING_SERVICE: 303
            * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
            * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
    * GamebaseEventCategory.OBSERVER_HEARTBEAT
    	* Operates when the status of a user account changes, for instance when the user account is deleted or banned.
    	* GamebaseEventObserverData.code: Indicates the GamebaseError value.
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* Can receive the information about the changes in the network.
    	* Operates when the network is disconnected or connected, or switched from Wi-Fi to a cellular network.
    	* GamebaseEventObserverData.code: Indicates the NetworkManager value.
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * Operates when logged in from Standalone/WebGL and fails to extend the session.

**Example**

```javascript
toast.Gamebase.addEventHandler((message) => {
    const category = message.category;
    const data = message.data;
    switch (category) {        
        case GamebaseEventCategory.OBSERVER_LAUNCHING:
            checkLaunchingStatus(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_NETWORK:
            checkNetworkStatus(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_HEARTBEAT:
            checkHeartbeat(JSON.parse(message.data));
            break;
        case GamebaseEventCategory.OBSERVER_INTROSPECT:
            // Introspect error
            var observerData = JSON.parse(message.data);
            var errorCode = observerData.code;
            var errorMessage = observerData.message;
            break;
        default:
            break;
    }
});

function checkLaunchingStatus(data) {
    var code = data.code;
    var isPlayable = toast.GamebaseLaunching.isPlayable(code);
    if (isPlayable) {
        // 'The Game is playable'
    } else {
        // 'The Game is not playable'
    }
}

function checkHeartbeat(data) {
    var code = data.code;
    if (code === toast.GamebaseConstant.INVALID_MEMBER) {
        // You should to write the code necessary in game. (End the session.)
    } else if (code === toast.GamebaseConstant.BANNED_MEMBER) {
        // The ban information can be found by using the GetBanInfo API.
        // Show kickout message to user and need kickout in game.
    } else {
        console.log('Heartbeat code: ' + code);
    }
}

function checkNetworkStatus(data) {
    var code = data.code;
    if (code === toast.GamebaseNetworkType.TYPE_NOT) {
        // Network disconnected
    } else {
        // Network connected
    }
}
```

### Analytics

The game index can be transferred to the Gamebase server.

> <font color="red">[Caution]</font><br/>
>
> All APIs supported by the Gamebase Analytics can be called after login.
>

> [TIP]
>
> If payment was made by calling the Gamebase.Purchase.RequestPurchase API, the index is transmitted to the server automatically when the payment is completed.
>

Please see the following guide for how to use Analytics console.

* [Analytics console](./oper-analytics)

#### Game User Data Settings

The game user level information can be transmitted as an index after logging in.

> <font color="red">[Caution]</font><br/>
>
> If the SetGameUserData API is not called after login to the game, the level information may be missed from other indexes.
>

Parameters required for calling the API are as follows:

**GameUserData**

| Name                       | Mandatory (M) / Optional (O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number | This field represents game user's level. |
| channelId | O | string | This field represents channel. |
| characterId | O | string | This field represents character name. |
| classId | O | string | This field represents class. |

**API**

```js
var gameUserData = {
	userLevel: ${User Level},
    channelId: ${Channel Id},
    characterId: ${Character Id},
    classId: ${ClassId Id},
}

toast.Gamebase.Analytics.setGameUserData(gameUserData);
```

**Example**

``` js
function setGameUserData(userLevel, channelId, characterId, classId) {
    var gameUserData = {
        userLevel: userLevel,
        channelId: channelId,
        characterId: characterId,
        classId: classId
    }

    toast.Gamebase.Analytics.setGameUserData(gameUserData);
}
```

#### Level Up Trace

The game user level information can be transmitted as an index after leveling up.

Parameters required for calling the API are as follows:

**LevelUpData**

| Name                       | Mandatory (M) / Optional (O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number | This field represents game user's level. |
| levelUpTime | O | number | Enter in Epoch time.</br>The unit is milliseconds. |
| channelId | O | string | This field represents channel. |
| characterId | O | string | This field represents class. |

**API**

```js
var levelUpData = {
	userLevel: ${User Level},
    levelUpTime: ${LevelUp Time},
    channelId: ${Channel Id},
    characterId: ${Character Id},
}

toast.Gamebase.Analytics.traceLevelUp(levelUpData);
```

**Example**

``` js
function traceLevelUp(userLevel, levelUpTime, channelId, characterId)
{
    var levelUpData = {
        userLevel: userLevel,
        levelUpTime: levelUpTime,
        channelId: channelId,
        characterId: characterId,
    }

    toast.Gamebase.Analytics.traceLevelUp(levelUpData);
}
```
