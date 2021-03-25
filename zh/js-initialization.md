## Game > Gamebase > JavaScript SDK使用指南 > 初始化

若欲使用Gamebase JavaScript SDK，应先进行初始化。

### Configuration Settings

对Gamebase进行初始化时，可利用GamebaseConfiguration对象更改Gamebase设置。

| KEY                                                 | Mandatory(M) / Optional(O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId:string                                       | **M**                      | 为NHN Cloud Project ID。必须输入。|
| clientVersion:string                               | **M**                      | 通过游戏版本确认是否为正在服务、检查、通知事项等可以玩游戏的状态。<br/> 请输入`NHN Cloud Console > Gamebase > App > Client Version > WEB`的版本。|
| enableDebugMode:boolean                            | O                          | 可启用Debug Mode。调试日志输出至开发者控制台。<br/> 默认值为**false**。|
| uiConfiguration.enablePopup:boolean                | O                          | 在**[UI]**<br/>系统检查、禁用(ban)等游戏用户无法玩游戏的情况下，有时应利用弹出窗口等显示原因。<br/> 若设置为**true**，Gamebase在相应情况下自动显示信息弹出窗口。<br/> 默认值为**false**。<br/>在**false**状态下，请通过推出结果获得信息后实现自主UI，显示无法玩游戏的原因。|
| uiConfiguration.enableLaunchingStatusPopup:boolean | O                          | 由于**[UI]**<br/>推出结果无法登录的状态下（主要为检查状态），可更改Gamebase是否自动显示弹出窗口。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| uiConfiguration.enableBanPopup:boolean             | O                          | **[UI]**<br/>游戏用户为被禁用状态时，可更改Gamebase是否通过自动弹出窗口来显示停止使用的原因。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| displayLanguageCode:string                         | O                          | 为Gamebase UI中使用的语言代码。<br/> 默认值为**浏览器的language**(navigator.language)。|
| displayLanguageTable:json                          | O                          | 为显示Gamebase UI时使用的文本源(text resource)。<br/> 该值与内置的语言集合表自动合并使用。<br/> 可通过`toast.Gamebase.getDisplayLanguageTable()`确认内置的语言集合表及格式。|


#### GamebaseConfiguration Example
```javascript
var gamebaseConfiguration = {
    appId:'T0asTC1oud',    // NHN Cloud Console Project ID
    clientVersion:'1.0.0', // NHN Cloud Console Gamebase App Client Version
    enableDebugMode:true,  // Note:Do not turn it on in production mode.
    uiConfiguration:{
        enablePopup:true,  // Default is false.If you want to use the Gamebase UI, please turn it on.
        enableLaunchingStatusPopup:true,
        enableBanPopup:true,
    },
};
```


### Debug Mode
用于Gamebase调试的设置。
* true:输出Gamebase的所有日志。
* false:输出警告(warning)、错误(error)日志。
* 默认值：false

调试设置也可在控制台进行，优先考虑在控制台中设置的值。
控制台设置方法请参考如下指南。

* [控制台测试终端机设置](./oper-app/#test-device)
* [控制台客户设置](./oper-app/#client)


若欲打开开发中可以参考的系统日志，请调用**toast.Gamebase.setDebugMode(true)**。
> <font color="red">[注意]</font><br/>
>
> **发布**游戏时，应将参数设置为false或不调用setDebugMode API。
>




### Initialize

**toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ...})调用**，对Gamebase SDK进行初始化<br/>

```js
function gamebaseInitialize() {
    var gamebaseConfiguration = {
        appId:'T0asTC1oud',    // NHN Cloud Console Project ID
        clientVersion:'1.0.0', // NHN Cloud Console Gamebase App Client Version
        enableDebugMode:true,  // Note:Do not turn it on in production mode.
        uiConfiguration:{
            enablePopup:true,  // Default is false.If you want to use the Gamebase UI, please turn it on.
            enableLaunchingStatusPopup:true,
            enableBanPopup:true,
        },
    };  


    toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
        if (error) {
            // 若初始化失败，则不可使用Gamebase SDK。
            // 请确认是否正常输入了appId、clientVersion及NHN Cloud控制台的设置。
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status值请参考下端的Launching Status Code表。
            // 为游戏可玩状态。
            console.log('Playable!');
        } else {
            // 为游戏不可玩状态（检查、服务结束等）
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

通过调用toast.Gamebase.initialize可以确认推出状态。

```js
toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
    if (error) {
        console.log(error);
        return;
    }

	var statusCode = launchingInfo.launching.status.code;
});
```

通过调用"toast.Gamebase.Launching.getLaunchingInformations()" API，初始化之后可获取LaunchingInfo对象。

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
在LaunchingInfo对象中存在Gamebase Console中设置的多个值和游戏状态等信息。


#### 1. Launching

为Gamebase推出信息。

**1.1 Status**
是在Gamebase JavaScript SDK初始化设置中输入的App版本游戏状态信息。

* code : 游戏状态代码(维护中、必须更新、终止服务等)
* message : 游戏状态消息

关于状态代码，请参考以下列表。

#### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 正常服务中                             |
| RECOMMEND_UPDATE            | 201  | 推荐更新                                 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 维护期间该服务不可用，但如果登记为测试设备，则无论维护如何，都可以连接和测试该服务。|
| IN_TEST                     | 203  | 正在测试 |
| IN_REVIEW                   | 204  | 正在审查 |
| IN_BETA                     | 205  | Beta服务器环境  |
| REQUIRE_UPDATE              | 300  | 强制更新                                |
| BLOCKED_USER                | 301  | 访问权限已被禁用的用户。|
| TERMINATED_SERVICE          | 302  | 终止服务                                  |
| INSPECTING_SERVICE          | 303  | 服务正在维护中                             |
| INSPECTING_ALL_SERVICES     | 304  | 所有服务正在维护中                              |
| INTERNAL_SERVER_ERROR       | 500  | 内部服务器错误                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

是在Gamebase Console中注册的App信息。

* accessInfo
    * serverAddress : 服务器地址
    * csInfo : 客服中心信息
* relatedUrls
    * termsUr l : 服务条款
    * personalInfoCollectionUrl : 个人隐私协议
    * punishRuleUr l: 禁用条款
    * csUrl : 客服中心
* install : 安装URL
* idP : 认证信息

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

是在Gamebase Console中注册的维护信息。

* url: 维护页面URL
* timezone: 标准时区
* beginDate: 开始时间
* endDate: 终止时间
* message: 维护原因

[Console Guide](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

是在Gamebase Console中注册的公告信息。

* message: 消息
* title: 标题
* url: 维护URL

[Console Guide](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

是与Gamebase相关的NHN Cloud服务appKey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

是在NHN Cloud Console中注册的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

是在NHN Cloud Console中注册的Launching信息。

* 以JSON string格式传送用户在Console中输入的值。
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)


### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | 未初始化Gamebase |
| NOT_LOGGED_IN                | 2          | 需要登录            |
| INVALID_PARAMETER            | 3          | 错误参数           |
| INVALID_JSON_FORMAT          | 4          | JSON格式错误          |
| USER_PERMISSION              | 5          | 没有权限               |
| NOT_SUPPORTED                | 10         | 不支持的功能        |


* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
