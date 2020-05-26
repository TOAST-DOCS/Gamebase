## Game > Gamebase > JavaScript SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

### Display Language

점검 팝업과 같이 Gamebase 가 표시하는 언어는 단말기에 설정된 언어로 표시됩니다.

그런데 게임에서 표시하는 언어를 단말기에 설정된 언어가 아닌, 별도의 옵션으로 언어를 변경할 수 있는 게임이 있습니다.
예를 들어, 단말기에 설정된 언어는 영어 이지만 게임 표시 언어를 일본어로 변경한 경우, Gamebase 에서 표시하는 언어도 일본어로 변경하고 싶지만 Gamebase 가 표시하는 언어는 단말기에 설정된 언어인 영어로 표시됩니다.

이와 같이 `단말기에 설정된 언어가 아닌, 다른 언어로 Gamebase 메세지를 표시하고 싶은` 어플리케이션을 위해 Gamebase 는 `Display Language` 라는 기능을 제공합니다.

Gamebase 는 Display Language 로 설정한 언어로 Gamebase 메세지를 표시합니다.
Display Language 에 입력하는 언어 코드는 반드시 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 지정된 코드만을 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * Display Language 는 단말기 설정 언어와 무관하게 Gamebase 의 표시 언어를 변경하고 싶은 경우에만 사용하시기 바랍니다.
> * Display Language Code 는 ISO-639 형태의 값으로, 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
> * 만일 Display Language Code 로 입력한 값이 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 존재하지 않는다면, Display Langauge Code 는 자동으로 기본값인 영어(en)로 지정됩니다.

> [참고]
>
> * Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함하고 있으므로 아래의 표에 존재하는 언어 코드라 할지라도 영어(en), 한글(ko), 일본어(ja) 이외의 언어를 지정하면 기본값인 영어(en)로 자동 설정됩니다.
> * Gamebase의 클라이언트에 포함되어 있지 않은 언어셋은 직접 추가할 수 있습니다.
> **신규 언어셋 추가** 항목을 참조하시기 바랍니다.

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

### Gamebase Event Handler

* Gamebase 는 각종 이벤트를 **GamebaseEventHandler** 라는 하나의 이벤트 시스템에서 모두 처리할 수 있습니다.
* GamebaseEventHandler 는 아래 API 를 통해 간단하게 Listener 를 추가/제거 할 수 있습니다.

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

* Category 는 GamebaseEventCategory 클래스에 정의되어 있습니다.

#### Server Push

* Gamebase 서버에서 클라이언트 단말기로 보내는 메세지 입니다.
* Gamebase 에서 지원하는 Server Push Type 은 다음과 같습니다.
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* TOAST Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guest 계정을 다른 단말기로 이전을 성공하게 되면 이전 단말기에서 킥아웃 메세지를 받게 됩니다.

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

* Gamebase Gamebase의 각종 상태 변동 이벤트를 처리하는 시스템 입니다.
* Gamebase 에서 지원하는 Observer Type 은 다음과 같습니다.
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 점검이 걸리거나 풀린 경우, 새로운 버전이 배포되어 업데이트가 필요한 경우와 같이, Launching 상태가 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code : LaunchingStatus 값을 의미합니다.
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
    	* 탈퇴 처리 되거나 이용 정지로 인하여 사용자 계정 상태가 변했을 때 동작합니다.
    	* GamebaseEventObserverData.code : GamebaseError 값을 의미합니다.
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 네트워크 변동 사항 정보를 받을 수 있습니다.
    	* 네트워크가 끊기거나 연결되었을 때, 혹은 Wifi 에서 셀룰러 네트워크로 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code : NetworkManager 값을 의미합니다.
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * Standalone/WebGL에서 로그인 후 세션 연장에 실패할 때 동작합니다.

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