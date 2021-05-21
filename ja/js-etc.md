## Game > Gamebase > JavaScript SDK使用ガイド > ETC

## Additional Features

Gamebaseでサポートする付加機能を説明します。

### Display Language

メンテナンスポップアップなどでGamebaseが表示する言語は、端末に設定された言語と同じです。

しかしゲームで表示する言語を、端末に設定した言語ではなく、別のオプションで変更できるゲームがあります。
例えば、端末に設定された言語は英語ですが、ゲーム表示言語を日本語に変更した場合、 Gamebaseで表示する言語も日本語に変更したいですが、Gamebaseが表示する言語は端末に設定された言語の英語が表示されます。

このように`端末に設定された言語ではなく、他の言語でGamebaseのメッセージを表示したい`アプリケーションのためにGamebaseは`Display Language`という機能を提供します。

GamebaseはDisplay Languageで設定した言語でGamebaseのメッセージを表示します。
Display Languageに入力する言語コードは、以下の表(**Gamebaseでサポートする言語コードの種類**)で指定したコードだけを使用できます。

> <font color="red">[注意]</font><br/>
>
> * Display Languageは、端末設定言語と関係なくGamebaseの表示言語を変更したい場合にのみ使用してください。
> * Display Language CodeはISO-639形式の値で、大文字/小文字を区別します。
> 'EN'または'zh-cn'と設定すると問題が発生する場合があります。
> * もしDisplay Language Codeに入力した値が以下の表(**Gamebaseでサポートする言語コードの種類**)に存在しない場合、Display Langauge Codeは自動的にデフォルト値の英語(en)が指定されます。

> [参考]
>
> * Gamebaseのクライアントメッセージは英語(en)、韓国語(ko)、日本語(ja)のみ含んでいるため、下記の表に存在する言語コードであっても、英語(en)、韓国語(ko)、日本語(ja)以外の言語を指定するとデフォルト値の英語(en)に自動設定されます。
> * Gamebaseのクライアントに含まれていない言語セットは直接追加できます。
> **新規言語セット追加**項目を参照してください。

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


### Gamebase Event Handler

* Gamebaseは各種イベントを**GamebaseEventHandler**という1つのイベントシステムで全て処理できます。
* GamebaseEventHandlerは下記のAPIを利用して簡単にListenerを追加/削除できます。

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

* CategoryはGamebaseEventCategoryクラスに定義されています。


#### Server Push

* Gamebaseサーバーからクライアント端末へ送信するメッセージです。
* GamebaseでサポートするServer Push Typeは次の通りです。
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受信します。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guestアカウントを他の端末へ移行すると、以前の端末でキックアウトメッセージを受信します。

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

* Gamebase Gamebaseの各種状態変動イベントを処理するシステムです。
* GamebaseでサポートするObserver Typeは次の通りです。
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* メンテナンスが開始または終了した場合、新しいバージョンが配布されてアップデートが必要な場合など、Launching状態が変更された時に動作します。
    	* GamebaseEventObserverData.code : LaunchingStatus値を意味します。
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
    	* 退会処理や利用停止により、ユーザーアカウントの状態が変わった時に動作します。
    	* GamebaseEventObserverData.code : GamebaseError値を意味します。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* ネットワーク変動事項情報を受け取れます。
    	* ネットワークが切断されたり、接続された時、またはWi-FiからCellularネットワークに変更された時に動作します。
    	* GamebaseEventObserverData.code : NetworkManager値を意味します。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * Standalone/WebGLでログイン後、セッションの延長に失敗した時に動作します。

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
| userLevel | M | number | ゲームユーザーレベルを表すフィールドです。 |
| channelId | O | string | チャンネルを表すフィールドです。 |
| characterId | O | string | キャラクター名を表すフィールドです。 |
| characterClassId | O | string | 職業を表すフィールドです。 |

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
| channelId | O | string |  |
| characterId | O | string |  |

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