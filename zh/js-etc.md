## Game > Gamebase > JavaScript SDK使用指南 > ETC

## Additional Features

介绍Gamebase支持的附加功能。

### Display Language


Gamebase显示终端机设置的语言。 

但有些游戏允许通过额外选项更改终端机设置的语言。
若终端机设置的默认语言是英语，则需将游戏的显示语言转换为日语时，即使您要将Gamebase的显示语言也转换为日语，Gamebase仍显示终端机设置的默认语言（en）。 

因此Gamebase向需要以终端机设置的语言之外的其他语言显示Gamebase消息的应用程序，提供”Display Language“功能。

Gamebase显示消息时，以设置为Display Language的语言显示。 
在Display Language中输入语言代码时，只能使用以下列表中（**Gamebase支持的语言代码种类**）指定的代码。

> <font color="red">[注意]</font><br/>
>
> * 无论终端机设置的语言如何，只需要更改Gamebase显示语言时使用Display Language Gamebase功能。  
> * Display Language Code为区分大小写的ISO-639形态的值。
> 若按”EN”或”zh-cn”进行设置，可能出现问题。
> * 若输入的Display Language Code值不在以下列表时（**Gamebase支持的语言代码的种类**）, 则将Display Langauge Code自动设置为默认语言（en）。

> [参考]
>
> * 因Gamebase客户端消息中仅包含英语（en）、韩语（ko）、日语（ja），即使是下列表指定的语言代码，指定英语（en）、韩语（ko）、日语（ja）之外的语言时，也将设置为默认语言(en)。
> * 可以直接添加未注册在Gamebase客户端的语言集合。
> 请参考**添加新语言集合**项目。

#### Gamebase支持的语言代码类型

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

该语言代码在`toast.GamebaseDisplayLanguage.DefaultCode`常数中定义。

```js
var toast.GamebaseDisplayLanguage.DefaultCode = {
    German:'de',
    English:'en',
    Spanish:'es',
    Finish:'fi',
    French:'fr',
    Indonesian:'id',
    Italian:'it',
    Japanese:'ja',
    Korean:'ko',
    Portuguese:'pt',
    Russian:'ru',
    Thai:'th',
    Vietnamese:'vn',
    Malay:'ms',
    Chinese_Simplified:'zh-CN',
    Chinese_Traditional:'zh-TW',
};
```

#### Gamebase初始化时设置Display Language

Gamebase初始化时可设置Display Language。

- API：

```js
var gamebaseConfiguration = {
    appId:'T0asTC1oud',    // TOAST Console Project ID
    clientVersion:'1.0.0', // TOAST Console Gamebase App Client Version
    displayLanguageCode:toast.GamebaseDisplayLanguage.DefaultCode.English,
};
toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ...});
```

**Example**

```js
function initialize() {
    var gamebaseConfiguration = {
        appId:'T0asTC1oud',
        clientVersion:'1.0.0',
        displayLanguageCode:toast.GamebaseDisplayLanguage.DefaultCode.English,
    };

    toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
       if (error) {
            // 若初始化失败，不可使用Gamebase SDK。
            // 请确认是否正常输入了appId、clientVersion及TOAST Console的设置。
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status值请参考下端的Launching Status Code表。
            // 为游戏可玩状态。
            console.log('Playable!');
        } else {
            // 为游戏不可玩状态。（检查、服务结束等）
            console.log('Not Playable!');
        }
    });
}
```

#### Set Display Language

可更改Gamebase初始化时输入的Display Language。

- API：

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

可查询当前使用的Display Language。

- API：

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

#### 添加新语言集合

除Gamebase提供的默认语言(en, ko, ja)外，若欲使用其他语种，应创建与其他语言集合相关的JSON对象并输入。
若欲在Gamebase中输入相应语言集合，调用如下API然后输入。

- API：

```js
// When try to initialize Gamebase
var displayLanguageTable = {
    en:{
        common_ok_button:'Customized OK Text',
        common_cancel_button:'Customized Cancel Text',
    }, ...
};
var gamebaseConfiguration = { ..., displayLanguageTable, ...};
toast.Gamebase.initialize(gamebaseConfiguration, (lanchingInfo, error) => { ...});

// After Gamebase was initialized.
toast.Gamebase.setDisplayLanguageTable(displayLanguageTable) {
```

默认内置于Gamebase的localized string如下。
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
    “common_ok_button”: “确认",
    “common_cancel_button”: “取消",
    ...
    “launching_service_closed_title”: “服务结束"
  },
  "ja": {
    “common_ok_button”: “确认",
    “common_cancel_button”: “取消",
    ...
    “launching_service_closed_title”: “服务结束"
  },
}
```

需要添加其他语言集合时，利用通过上述API传递的参数值，以如下
`”${语言代码}”:{”key”:”value”}`形式添加值并调用即可。

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    “common_ok_button”: “确认",
    “common_cancel_button”: “取消",
    ...
    “launching_service_closed_title”: “服务结束"
  },
  "ja": {
    “common_ok_button”: “确认",
    “common_cancel_button”: “取消",
    ...
    “launching_service_closed_title”: “服务结束"
  },
  “${语言代码}": {
      "common_ok_button": "...",
      ...
  }
}
```

以上JSON格式中”${语言代码}”:{ }内部遗漏密钥时，自动输入`终端机设置的语言或en`。

#### Display Language优先顺序

通过初始化及setDisplayLanguageCode API设置Display Language时，最终应用的Display Language可使用与输入值不同的值。

1.确认输入的languageCode是否定义于localized string。
2.Gamebase初始化时，确认终端机设置的语言代码是否定义于localized string。（该值初始化后，即使更改终端机设置的语言也会保留。）
3.Display Language的默认值`en`自动设置。

### Gamebase Event Handler

* Gamebase通过**GamebaseEventHandler**事件系统处理所有事件。
* GamebaseEventHandler通过以下API简单添加或删除Listener。

- API：

```js
toast.Gamebase..addEventHandler((event) => { ...});
toast.Gamebase..removeEventHandler((event) => { ...});
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

* Category在GamebaseEventCategory类中定义。

#### Server Push

* 从Gamebase服务器向客户端终端机发送的消息如下。
* Gamebase支持的Server Push Type如下。
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* 如果在TOAST Gamebase控制台**Operation > Kickout**中注册Kickout ServerPush消息，则从与Gamebase连接的所有客户端接收Kickout消息。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* 将Guest账号成功转移到其他终端机时，从转移之前的终端机接收Kickout消息。

**Example**

```js
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

* 是处理Gamebase的各状态变动事件的系统。
* Gamebase支持的Observer Type如下。
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 当开始或结束维护、发布新版本必须进行更新等Launching状态出现变动时运行。
    	* GamebaseEventObserverData.code : 为LaunchingStatus值。
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
    	* 已被退出或禁用，用户账户状态出现变动时运行。
    	* GamebaseEventObserverData.code : 为GamebaseError值。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 可接收网络变动信息。
    	* 网络被连接或断开、从Wifi转为Cellular网络时运行。
    	* GamebaseEventObserverData.code : 为NetworkManager值。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * 在Standalone/WebGL上进行登录后，session延长失败时运行。

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
        console.log('Heartbeat code:' + code);
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

可将游戏指标传送至Gamebase服务器。

> <font color="red">[注意]</font><br/>
>
> Gamebase Analytics支持的所有API登录后可调用。
>

> [TIP]
>
> 调用Gamebase.Purchase.RequestPurchase API进行付款时，付款完成后指标自动传送至服务器。
>

Analytics控制台使用方法请参考如下指南。

* [Analytics控制台](./oper-analytics)

#### Game User Data Settings

登录后游戏用户级别信息可作为指标传送。

> <font color="red">[注意]</font><br/>
>
> 若登录游戏后不调用SetGameUserData API，则其他指标中可能会遗漏级别信息。
>

调用API所需的参数如下。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number | 是显示游戏用户级别的字段。 |
| channelId | O | string | 是显示通道的字段。 |
| characterId | O | string | 是显示角色名的字段。 |
| characterClassId | O | string | 是显示职业的字段。 |

- API：

```js
var gameUserData = {
	userLevel:${User Level},
    channelId:${Channel Id},
    characterId:${Character Id},
}

toast.Gamebase.Analytics.setGameUserData(gameUserData);
```

**Example**

``` js
function setGameUserData(userLevel, channelId, characterId) {
    var gameUserData = {
        userLevel:userLevel,
        channelId:channelId,
        characterId:characterId,
    }

    toast.Gamebase.Analytics.setGameUserData(gameUserData);
}
```

#### Level Up Trace

升级后游戏用户级别信息可作为指标传送。

调用API所需的参数如下。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | number | 是显示游戏用户级别的字段。 |
| levelUpTime | O | number | Enter Epoch Time</br>in millisecond. |
| channelId | O | string | 是显示角色名的字段。 |
| characterId | O | string | 是显示职业的字段。 |

- API：

```js
var levelUpData = {
	userLevel:${User Level},
    levelUpTime:${LevelUp Time},
    channelId:${Channel Id},
    characterId:${Character Id},
}

toast.Gamebase.Analytics.traceLevelUp(levelUpData);
```

**Example**

``` js
function traceLevelUp(userLevel, levelUpTime, channelId, characterId)
{
    var levelUpData = {
        userLevel:userLevel,
        levelUpTime:levelUpTime,
        channelId:channelId,
        characterId:characterId,
    }

    toast.Gamebase.Analytics.traceLevelUp(levelUpData);
}
```
