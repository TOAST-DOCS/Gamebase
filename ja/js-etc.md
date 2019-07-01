## Game > Gamebase > JavaScript SDK使用ガイド > ETC

## Additional Features

Gamebaseでサポートする付加機能を説明します。

### Display Language

* Gamebaseで表示する言語(Gamebase内蔵UIに表示されるテキスト)を、端末に設定された言語(device language)ではない別の言語に変更できます。
* Gamebaseは、クライアントに含まれているメッセージや、サーバーから受信したメッセージを表示します。
* Display Languageを設定すると、ユーザーが設定した言語コード(ISO-639)に適した言語でメッセージを表示します。
* 自由に言語セットを追加できます。追加できる言語コードは次の通りです。

> [参考]
>
> Gamebaseのクライアントメッセージは、英語(en)、韓国語(ko)、日本語(ja)のみ含まれています。

#### Gamebaseでサポートする言語コードの種類

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

該当言語コードは`toast.GamebaseDisplayLanguage.DefaultCode`定数に定義されています。

> `[注意]`
>
> Gamebaseでサポートする言語コードは大文字/小文字を区別します。
> 'EN'または'zh-cn'と設定すると、問題が発生する場合があります。

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

#### Gamebase初期化時のDisplay Language設定

Gamebase初期化時に、Display Languageを設定できます。

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
            // 初期化に失敗すると、Gamebase SDKを利用できません。
            // appId、clientVersionおよびTOAST Consoleの設定が正常に入力されているかどうか、確認してください。
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status値は下のLaunching Status Code表を参照してください。
            // ゲームプレイ可能状態です。
            console.log('Playable!');
        } else {
            // ゲームプレイできない状態です。(メンテナンス、サービス終了など)
            console.log('Not Playable!');
        }
    });
}
```

#### Set Display Language

Gamebase初期化時、入力されたDisplay Languageを変更できます。

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

現在適用されているDisplay Languageを照会できます。

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

#### 新規言語セットの追加

Gamebaseで提供する基本言語(en、ko、ja)以外の言語を使用するには、他の言語セットのJSONオブジェクトを作成して入力する必要があります。
該当言語セットをGamebaseに入力するには、次のAPIを呼び出して入力します。

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

デフォルトでGamebaseに内蔵されているlocalized stringは次の通りです。
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

他の言語セットを追加する必要がある時は、上記のAPIに渡すパラメータ値に、次のように
`"${言語コード}":{"key":"value"}`形式で値を追加して呼び出してください。

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

上記のJSON形式で"${言語コード}":{ }内部にキーが抜けている場合には、`端末に設定されている言語またはen`が自動的に入力されます。

#### Display Language優先順位

初期化およびsetDisplayLanguageCode APIを通してDisplay Languageを設定する場合、最終適用されるDisplay Languageは、入力した値と異なる値が適用されることがあります。

1. 入力されたlanguageCodeがlocalized stringに定義されているかを確認します。
2. Gamebaseの初期化時、端末に設定されている言語コードがlocalized stringに定義されているかを確認します。(この値は初期化後、端末に設定されている言語を変更しても維持されます。)
3. Display Languageのデフォルト値である`en`が自動的に設定されます。

### Server Push
* Gamebaseサーバーからクライアント端末にWebSocketを利用して送信するServer Push Messageを処理できます。
* GamebaseクライアントでServerPushEvent Listenerを追加すると、該当メッセージをユーザーが受信して処理でき、追加されたServerPushEvent Listenerを削除できます。

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Server Push Type
現在GamebaseでサポートするServer Push Typeは次の通りです。
**toast.GamebaseServerPushType**定数で確認できます。

* キックアウト(Kickout)
    * TOAST Gamebaseコンソールの`Operation > Kickout`から、キックアウトServerPushメッセージを登録すると、Gamebaseと接続されたすべてのクライアントにメッセージを送信できます。
    * Type: toast.GamebaseServerPushType.APP_KICKOUT (= "appKickout")


#### Add ServerPushEvent
GamebaseクライアントにServerPushEventを登録し、GamebaseコンソールおよびGamebaseサーバーで発行されたPushイベントを処理できます。

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
Gamebaseに登録されたServerPushEventを削除できます。

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
* Gamebase ObserverでGamebaseの各種ステータス変動イベントを受け取って処理できます。
* ステータス変動イベント：ネットワークタイプ変動、Launchingステータス変動(メンテナンスなどによるステータス変動)、Heartbeat情報変動(ユーザー利用停止などによるHeartbeat情報変動)など

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Observer Type
現在GamebaseでサポートするObserver Typeは次の通りです。
**toast.GamebaseObserverType**定数で確認できます。

* Networkタイプ変動
    * ネットワーク変動事項情報を受け取れます。例えばObserverMessage.data.get("code")の値でNetwork Typeを知ることができます。
    * Type: toast.GamebaseObserverType.NETWORK (= "network")
    * Code:**toast.GamebaseNetworkType**に宣言された定数は次の通りです。
        * toast.GamebaseNetworkType.TYPE_NOT: -1
        * toast.GamebaseNetworkType.TYPE_MOBILE: 0
        * toast.GamebaseNetworkType.TYPE_WIFI: 1
        * toast.GamebaseNetworkType.TYPE_ANY: 2
* Launchingステータス変動
    * 周期的にアプリケーションステータスを確認するLaunching Status responseに変動がある時に発生します。例えば、メンテナンス、サービス終了などによるイベントがあります。
    * Type: toast.GamebaseObserverType.LAUNCHING (= "launching")
    * Code:**toast.GamebaseLaunchingStatus**に宣言された定数は次の通りです。
        * toast.GamebaseLaunchingStatus.IN_SERVICE: 200
        * toast.GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * toast.GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * toast.GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * toast.GamebaseLaunchingStatus.BLOCKED_USER: 301
        * toast.GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * toast.GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * toast.GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * toast.GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat情報変動
    * 周期的にGamebaseサーバーと接続を維持するHeartbeat responseに変動がある時に発生します。例えば、ユーザーが利用停止になった時、正常に'ログイン接続'ができないため、ユーザー利用停止イベントが発生します。
    * Type: toast.GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code:**toast.GamebaseConstant**に宣言された定数を参照します。
        * toast.GamebaseConstant.BANNED_MEMBER: 7


#### Add Observer
GamebaseクライアントにObserverを登録し、各種ステータス変動イベントを処理できます。

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
Gamebaseに登録されたObserverを削除できます。

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

ゲーム指標をGamebaseサーバーに伝送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIはログイン後に呼び出すことができます。
>

> [TIP]
>
> Gamebase.Purchase.RequestPurchase APIを呼び出して決済を行った場合、決済が完了するとサーバーに指標が自動的に伝送されます。
>

Analyticsコンソールの使用方法は、下記のガイドを参照してください。

* [Analyticsコンソール](./oper-analytics)

#### Game User Data Settings

ログイン後、ゲームユーザーレベル情報を指標として伝送できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後にsetGameUserData APIを呼び出さない場合、他の指標でレベル情報が抜ける場合があります。
>

APIの呼び出しに必要なパラメータは下記の通りです。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number |  |
| channelId | O | string |  |
| characterId | O | string |  |
| classId | O | string |  |

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

レベルアップすると、ゲームユーザーレベル情報を指標として伝送できます。

APIの呼び出しに必要なパラメータは下記の通りです。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number |  |
| levelUpTime | M | number | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |

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