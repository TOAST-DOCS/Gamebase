## Game > Gamebase > JavaScript SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

### Display Language

* Gamebase에서 표시하는 언어(Gamebase 내장 UI에 표시되는 텍스트)를 단말기에 설정된 언어(device language)가 아닌 다른 언어로 변경할 수 있습니다.
* Gamebase는 클라이언트에 포함되어 있는 메시지를 표시하거나 서버에서 받은 메시지를 표시합니다.
* DisplayLanguage를 설정하면 사용자가 설정한 언어코드(ISO-639)에 적합한 언어로 메시지를 표시합니다.
* 원하는 언어셋을 추가할 수 있습니다. 추가할 수 있는 언어코드는 다음과 같습니다.

> [참고]
>
> Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함합니다.

#### Gamebase에서 지원하는 언어코드의 종류

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

해당 언어코드는 `toast.GamebaseDisplayLanguage.DefaultCode` 상수에 정의되어 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase에서 지원하는 언어코드는 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
>

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

#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

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
            // 초기화에 실패하면 Gamebase SDK를 이용할 수 없습니다.
            // appId, clientVersion 및 TOAST Console의 설정이 정상적으로 입력되었는지 확인하세요.
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status 값은 하단의 Launching Status Code 표를 참조하시길 바랍니다.
            // 게임 플레이 가능상태입니다.
            console.log('Playable!');
        } else {
            // 게임 플레이 불가능상태입니다. (점검, 서비스 종료 등)
            console.log('Not Playable!');
        }
    });
}
```

#### Set Display Language

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

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

현재 적용된 Display Language를 조회할 수 있습니다.

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

#### 신규 언어셋 추가

Gamebase에서 제공하는 기본 언어(en, ko, ja) 외 다른 언어를 사용하려면, 다른 언어셋에 대한 JSON 객체를 만들어서 입력해주어야합니다.
해당 언어셋을 Gamebase에 입력하려면 다음의 API를 호출하여 입력합니다.

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

기본적으로 Gamebase에 내장되어있는 localized string은 다음과 같습니다.
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

다른 언어셋을 추가해야 할 경우에는 위의 API로 넘기는 파라미터값으로 다음과 같이 
`"${언어 코드}":{"key":"value"}` 형태로 값을 추가하여 호출하면 됩니다

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
  "${언어코드}": {
      "common_ok_button": "...",
      ...
  }
}
```

위 JSON 형식에서 "${언어코드}":{ } 내부에 key가 누락될 경우에는 `단말기에 설정된 언어 또는 en`이 자동으로 입력됩니다.

#### Display Language 우선순위

초기화 및 setDisplayLanguageCode API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localized string에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 단말기에 설정된 언어코드가 localized string에 정의되어 있는지 확인합니다.(이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동으로 설정됩니다.

### Server Push
* Gamebase 서버에서 클라이언트 단말기로 WebSocket을 이용하여 보내는 Server Push Message를 처리할 수 있습니다.
* Gamebase 클라이언트에서 ServerPushEvent Listener를 추가하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent Listener를 삭제할 수 있습니다.

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.
**toast.GamebaseServerPushType** 상수에서 확인가능합니다.

* 킥아웃(Kickout)
    * TOAST Gamebase 콘솔의 `Operation > Kickout`에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에게 메시지를 보낼 수 있습니다.
    * Type: toast.GamebaseServerPushType.APP_KICKOUT (= "appKickout")


#### Add ServerPushEvent
Gamebase Client에 ServerPushEvent를 등록하여 Gamebase Console 및 Gamebase 서버에서 발급된 Push 이벤트를 처리할 수 있습니다.

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
Gamebase에 등록된 ServerPushEvent를 삭제할 수 있습니다.

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
* Gamebase Observer로 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* 상태 변동 이벤트 : 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Observer Type
현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.
**toast.GamebaseObserverType** 상수에서 확인가능합니다.

* Network 타입 변동
    * 네트워크 변동 사항 정보를 받을 수 있습니다. 예를 들어 ObserverMessage.data.get("code") 의 값으로 Network Type을 알 수 있습니다.
    * Type: toast.GamebaseObserverType.NETWORK (= "network")
    * Code: **toast.GamebaseNetworkType** 에 선언된 상수를 참고합니다.
        * toast.GamebaseNetworkType.TYPE_NOT: -1
        * toast.GamebaseNetworkType.TYPE_MOBILE: 0
        * toast.GamebaseNetworkType.TYPE_WIFI: 1
        * toast.GamebaseNetworkType.TYPE_ANY: 2
* Launching 상태 변동
    * 주기적으로 애플리케이션 상태를 확인하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어 점검, 서비스 종료 등에 의한 이벤트가 있습니다.
    * Type: toast.GamebaseObserverType.LAUNCHING (= "launching")
    * Code: **toast.GamebaseLaunchingStatus** 에 선언된 상수를 참고합니다.
        * toast.GamebaseLaunchingStatus.IN_SERVICE: 200
        * toast.GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * toast.GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * toast.GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * toast.GamebaseLaunchingStatus.BLOCKED_USER: 301
        * toast.GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * toast.GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * toast.GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * toast.GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat 정보 변동
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어 사용자가 이용정지를 당했을 때, 정상적인 "로그인 연결"을 맺지 못하므로, 사용자 이용 정지 이벤트가 발생합니다.
    * Type: toast.GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code: **toast.GamebaseConstant** 에 선언된 상수를 참조합니다.
        * toast.GamebaseConstant.BANNED_MEMBER: 7


#### Add Observer
Gamebase Client에 Observer를 등록하여 각종 상태 변동 이벤트를 처리할 수 있습니다.

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
Gamebase에 등록된 Observer를 삭제할 수 있습니다.

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

Game지표를 Gamebase Server로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase Analytics에서 지원하는 모든 API는 로그인 후에 호출이 가능합니다..
>

> [TIP]
>
> Gamebase.Purchase.RequestPurchase API를 호출하여 결제를 진행한 경우, 결제가 완료되면 자동으로 서버로 지표가 전송됩니다.
>

Analytics Console 사용법은 아래 가이드를 참고하십시오.

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

로그인 이후 유저 레벨 정보를 지표로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 SetGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 유저 레벨을 나타내는 필드입니다. |
| channelId | O | String | 채널을 나타내는 필드입니다. |
| characterId | O | String | 케릭터 명을 나타내는 필드입니다. |
| classId | O | String | 직업을 나타내는 필드입니다. |

**API**

```js
var gameUserData = new toast.GameUserData(userLevel);
gameUserData.channelId = channelId;
gameUserData.characterId = characterId;
gameUserData.classId = classId;

toast.Gamebase.Analytics.setGameUserData(gameUserData);
```

**Example**

``` js
function setGameUserData(userLevel, channelId, characterId, classId) {
    var gameUserData = new toast.GameUserData(userLevel);
    gameUserData.channelId = channelId;
    gameUserData.characterId = characterId;
    gameUserData.classId = classId;

    toast.Gamebase.Analytics.setGameUserData(gameUserData);
}
```

#### Level Up Trace

레벨업이 되었을 경우 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 유저 레벨을 나타내는 필드입니다. |
| levelUpTime | M | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |

**API**

```js
var levelUpData = new toast.LevelUpData(userLevel, levelUpTime);

toast.Gamebase.Analytics.traceLevelUp(levelUpData);
```

**Example**

``` js
function traceLevelUp(userLevel, levelUpTime) {
    var levelUpData = new toast.LevelUpData(userLevel, levelUpTime);
    toast.Gamebase.Analytics.traceLevelUp(levelUpData);
}
```