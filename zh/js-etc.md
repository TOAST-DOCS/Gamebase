## Game > Gamebase > JavaScript SDK使用指南 > ETC

## Additional Features

介绍Gamebase支持的附加功能。

### Display Language

* 可将Gamebase中显示的语言（Gamebase内置UI显示的文本）更改为与终端机设置的语言(device language)不同的语言。
* Gamebase显示包含于客户的信息或显示从服务器接收的信息。
* 若设置Display Language，可以用符合用户设置的语言代码(ISO-639)的语种来显示信息。
* 可添加需要的语言集合。可添加的语言代码如下。

> [参考]
>
> Gamebase的客户信息仅包含英文(en)、韩文(ko)、日文(ja)。

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

> `[注意]`
>
> Gamebase支持的语言代码区分大小写。
> 若按‘EN’或’zh-cn’进行设置，可能出现问题。

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

### Server Push
* 可以处理Gamebase服务器中利用WebSocket向客户终端机发送的Server Push Message。
* 若在Gamebase客户中添加ServerPushEvent Listener，用户可接收该信息并处理，添加的ServerPushEvent Listener可删除。

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Server Push Type
当前Gamebase支持的Server Push Type如下。
可在**toast.GamebaseServerPushType**常数中确认。

* 踢出(Kickout)
    * 若在TOAST Gamebase控制台的`Operation > Kickout`中注册踢出ServerPush信息，则可向与Gamebase连接的所有客户发送信息。
    * Type:toast.GamebaseServerPushType.APP_KICKOUT (= "appKickout")


#### Add ServerPushEvent
可向Gamebase客户注册ServerPushEvent，处理Gamebase控制台及Gamebase服务器发送的Push事件。

- API：

```js
toast.Gamebase.addServerPushEvent((serverPushEvent) => { ...});
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
可删除向Gamebase注册的ServerPushEvent。

- API：

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
* 可利用Gamebase Observer接收并处理Gamebase的各种状态变动事件。
* 状态变动事件：网络类型变动、Launching状态变动（检查等导致的状态变动）、Heartbeat信息变动（用户停止使用等导致的Heartbeat信息变动）等

#### Flow
![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Observer Type
当前Gamebase支持的Observer Type如下。
可在**toast.GamebaseObserverType**常数中确认。

* Network类型变动
    * 可接收网络变动事项信息。例如，可通过ObserverMessage.data.get(”code”)的值了解Network Type。
    * Type:toast.GamebaseObserverType.NETWORK (= "network")
    * Code:参考**toast.GamebaseNetworkType**宣布的常数。
        * toast.GamebaseNetworkType.TYPE_NOT:-1
        * toast.GamebaseNetworkType.TYPE_MOBILE:0
        * toast.GamebaseNetworkType.TYPE_WIFI:1
        * toast.GamebaseNetworkType.TYPE_ANY:2
* Launching状态变动
    * 周期性确认应用程序状态的Launching Status response存在变动时发生。例如，存在因检查、服务结束等导致的事件。
    * Type:toast.GamebaseObserverType.LAUNCHING (= "launching")
    * Code:参考**toast.GamebaseLaunchingStatus**宣布的常数。
        * toast.GamebaseLaunchingStatus.IN_SERVICE:200
        * toast.GamebaseLaunchingStatus.RECOMMEND_UPDATE:201
        * toast.GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST:202
        * toast.GamebaseLaunchingStatus.REQUIRE_UPDATE:300
        * toast.GamebaseLaunchingStatus.BLOCKED_USER:301
        * toast.GamebaseLaunchingStatus.TERMINATED_SERVICE:302
        * toast.GamebaseLaunchingStatus.INSPECTING_SERVICE:303
        * toast.GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES:304
        * toast.GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR:500
* Heartbeat信息变动
    * 周期性与Gamebase服务器保持连接的Heartbeat response存在变动时发生。例如，用户被停止使用时，无法正常’登录连接’，因此发生用户停止使用事件。
    * Type:toast.GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code:参考**toast.GamebaseConstant**宣布的常数。
        * toast.GamebaseConstant.BANNED_MEMBER:7


#### Add Observer
可向Gamebase客户注册Observer，处理各种状态变动事件。

- API：

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
        console.log('Heartbeat code:' + code);
    }
}

function checkNetworkStatus(data) {
    var code = data.code;       // Code:toast.GamebaseNetworkType
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
可删除向Gamebase注册的Observer。

- API：

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
| userLevel | M | number |  |
| channelId | O | string |  |
| characterId | O | string |  |

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
| userLevel | M | number |  |
| levelUpTime | O | number | 按Epoch Time输入。</br>按Millisecond单位输入。|
| channelId | O | string |  |
| characterId | O | string |  |

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
