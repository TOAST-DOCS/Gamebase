## Game > Gamebase > JavaScript SDK User Guide > ETC

## Additional Features

Additional functions provided by Gamebase are described as below:

### Display Language

* The language displayed on Gamebase (text displayed on the UI embedded in Gamebase) can be changed into another language which is not a (device language) set for the device.
* Gamebase displays messages which are included in a client or as received by a server.
* With Display Language, messages are displayed in an appropriate language for the language code (ISO-639).
* If necessary, language sets can be added as the user wants. The list of available language codes is as follows:

> [Note]
>
> Client messages of Gamebase include only English (en), Korean (ko), and Japanese (ja).

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

> `[Caution]`
>
> Language codes supported by Gamebase are case-sensitive.
> For example, spellings like 'EN' or 'zh-ch' may cause a problem.

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
    appId: 'T0asTC1oud',    // TOAST Console Project ID
    clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
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

### Server Push
* Handles Server Push Messages sent from the Gamebase server to client devices using WebSocket.
* Add ServerPushEvent Listener to Gamebase Client, and the user can receive and handle messages. The added ServerPushEvent Listener can be deleted.

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Server Push Type
Server Push Types currently supported by Gamebase are as follows:
You can check it in the **toast.GamebaseServerPushType** constant.

* Kickout (Kickout)
    * Go to Operation > Kickout in the TOAST Gamebase console and register Kickout ServerPush messages. Then, messages will be sent to all clients connected to Gamebase.
    * Type: toast.GamebaseServerPushType.APP_KICKOUT (= "appKickout")


#### Add ServerPushEvent
Registers ServerPushEvent to the Gamebase Client to handle the push event issued by the Gamebase Console and Gamebase server.

**API**

```js
toast.Gamebase.addServerPushEvent((serverPushEvent) => { ... });
```

**Example**

```js
var serverPushEvent = (event) => {
    var type = event.type;
    var data = event.data;

    if (type === toast.GamebaseServerPushType.APP_KICKOUT) {
        console.log('User is kicked out.');
    } else {
        console.log('Unknown server push event is arrived');
        console.log(event);
    }
};

function addServerPush() {
    toast.Gamebase.addServerPushEvent(serverPushEvent);
}
```


#### Remove ServerPushEvent
Deletes ServerPushEvent registered in Gamebase.

**API**

```js
toast.Gamebase.removeServerPushEvent(serverPushEvent);
toast.Gamebase.removeAllServerPushEvent();
```

**Example**

```js
function removeServerPush() {
    toast.Gamebase.removeServerPushEvent(serverPushEvent);
}

function removeAllServerPush() {
    toast.Gamebase.removeAllServerPushEvent();
}
```





### Observer
* With Gamebase Observer, get and proceed with status change events of Gamebase.
* Status change events: Change of network type, change of launching status (change of status due to maintenance), change of heartbeat information(, change of heartbeat information due to user ban), and etc.

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Observer Type
Observer Types currently supported by Gamebase are as follows:
You can check it in the **toast.GamebaseObserverType** constant.

* Change of Network Type
    * Receive information about network changes. For instance, find a network type with the ObserverMessage.data.get("code") value.
    * Type: toast.GamebaseObserverType.NETWORK (= "network")
    * Code: Refer to the constant declared in **toast.GamebaseNetworkType**.
        * toast.GamebaseNetworkType.TYPE_NOT: -1
        * toast.GamebaseNetworkType.TYPE_MOBILE: 0
        * toast.GamebaseNetworkType.TYPE_WIFI: 1
        * toast.GamebaseNetworkType.TYPE_ANY: 2
* Launching status change
    * Occurs when there is a change in the launching status response that periodically checks application status. For example, events occur for maintenance, service termination, etc.
    * Type: toast.GamebaseObserverType.LAUNCHING (= "launching")
    * Code: Refer to the constant declared in **toast.GamebaseLaunchingStatus**.
        * toast.GamebaseLaunchingStatus.IN_SERVICE: 200
        * toast.GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * toast.GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * toast.GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * toast.GamebaseLaunchingStatus.BLOCKED_USER: 301
        * toast.GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * toast.GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * toast.GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * toast.GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat Information Change
    * Occurs when there is a change in the heartbeat response which periodically maintains connection with the Gamebase server. For example, a user ban event occurs because a normal 'login connection' cannot be made when a user has been banned.
    * Type: toast.GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code: Refer to the constant declared in **toast.GamebaseConstant**.
        * toast.GamebaseConstant.BANNED_MEMBER: 7


#### Add Observer
Register Observer to handle the status change events of Gamebase client.

**API**

```js
var observerEvent = (event) => {
    var type = event.type;
    var data = event.data;

    if (type === toast.GamebaseServerPushType.APP_KICKOUT) {
        console.log('User is kicked out.');
    } else {
        console.log('Unknown server push event is arrived');
        console.log(event);
    }
};
toast.Gamebase.addObserver(observerEvent);
```

**Example**

```js
function addObserver() {
    toast.Gamebase.addObserver(observerEvent);
}

var observerEvent = (event) => {
    var typeOfEvent = event.type;
    var data = event.data;

    if (typeOfEvent === toast.GamebaseObserverType.NETWORK) {
        checkNetworkStatus(data);
    } else if (typeOfEvent === toast.GamebaseObserverType.HEARTBEAT) {
        checkHeartbeat(data);
    } else if (typeOfEvent === toast.GamebaseObserverType.LAUNCHING) {
        checkLaunchingStatus(data);
    }
};

function checkLaunchingStatus(data) {
    var code = data.code;       // Code : toast.GamebaseLaunchingStatus
    var message = data.message;

    console.log(message);

    var isPlayable = toast.GamebaseLaunching.isPlayable(code);
    if (isPlayable) {
        console.log('The Game is playable');
    } else {
        console.log('The Game is not playable');
    }
}

function checkHeartbeat(data) {
    // Code : toast.GamebaseConstant.INVALID_MEMBER, toast.GamebaseConstant.BANNED_MEMBER
    var code = data.code;
    var message = data.message;

    console.log('Heartbeat status is changed.');
    console.log(message);

    if (code === toast.GamebaseConstant.INVALID_MEMBER) {
        console.log('This user is an invalid member.');
        console.log('Maybe the user was withdrawn or has a trouble with his/her account.');
    } else if (code === toast.GamebaseConstant.BANNED_MEMBER) {
        console.log('This user is banned.');
    } else {
        console.log('Heartbeat code: ' + code);
    }
}

function checkNetworkStatus(data) {
    var code = data.code;       // Code: toast.GamebaseNetworkType
    var message = data.message;

    console.log('Network status is changed.');
    console.log(message);

    if (code === toast.GamebaseNetworkType.TYPE_NOT) {
        console.log('Network is unreachable.');
    } else {
        console.log('Network is rechable.');
    }
}
```


#### Remove Observer
Deletes Observer registered in Gamebase.

**API**

```js
toast.Gamebase.removeObserver(observerEvent);
toast.Gamebase.removeAllObserver();
```

**Example**

```js
function removeObserver() {
    toast.Gamebase.removeObserver(observerEvent);
}

function removeAllObserver() {
    toast.Gamebase.removeAllObserver();
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
| userLevel | M | number |  |
| channelId | O | string |  |
| characterId | O | string |  |

**API**

```js
var gameUserData = {
	userLevel: ${User Level},
    channelId: ${Channel Id},
    characterId: ${Character Id},
}

toast.Gamebase.Analytics.setGameUserData(gameUserData);
```

**Example**

``` js
function setGameUserData(userLevel, channelId, characterId) {
    var gameUserData = {
        userLevel: userLevel,
        channelId: channelId,
        characterId: characterId,
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
| userLevel | M | number |  |
| levelUpTime | O | number | Enter Epoch Time</br>in millisecond. |
| channelId | O | string |  |
| characterId | O | string |  |

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
