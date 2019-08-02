## Game > Gamebase > JavaScript SDK使用指南 > 初始化

若欲使用Gamebase JavaScript SDK，应先进行初始化。

### Configuration Settings

对Gamebase进行初始化时，可利用GamebaseConfiguration对象更改Gamebase设置。

| KEY                                                 | Mandatory(M) / Optional(O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId:string                                       | **M**                      | 为TOAST Project ID。必须输入。|
| clientVersion:string                               | **M**                      | 通过游戏版本确认是否为正在服务、检查、通知事项等可以玩游戏的状态。<br/> 请输入`TOAST Console > Gamebase > App > Client Version > WEB`的版本。|
| enableDebugMode:boolean                            | O                          | 可启用Debug Mode。调试日志输出至开发者控制台。<br/> 默认值为**false**。|
| uiConfiguration.enablePopup:boolean                | O                          | 在**[UI]**<br/>系统检查、禁用(ban)等游戏用户无法玩游戏的情况下，有时应利用弹出窗口等显示原因。<br/> 若设置为**true**，Gamebase在相应情况下自动显示信息弹出窗口。<br/> 默认值为**false**。<br/>在**false**状态下，请通过推出结果获得信息后实现自主UI，显示无法玩游戏的原因。|
| uiConfiguration.enableLaunchingStatusPopup:boolean | O                          | 由于**[UI]**<br/>推出结果无法登录的状态下（主要为检查状态），可更改Gamebase是否自动显示弹出窗口。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| uiConfiguration.enableBanPopup:boolean             | O                          | **[UI]**<br/>游戏用户为被禁用状态时，可更改Gamebase是否通过自动弹出窗口来显示停止使用的原因。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| displayLanguageCode:string                         | O                          | 为Gamebase UI中使用的语言代码。<br/> 默认值为**浏览器的language**(navigator.language)。|
| displayLanguageTable:json                          | O                          | 为显示Gamebase UI时使用的文本源(text resource)。<br/> 该值与内置的语言集合表自动合并使用。<br/> 可通过`toast.Gamebase.getDisplayLanguageTable()`确认内置的语言集合表及格式。|


#### GamebaseConfiguration Example
```javascript
var gamebaseConfiguration = {
    appId:'T0asTC1oud',    // TOAST Console Project ID
    clientVersion:'1.0.0', // TOAST Console Gamebase App Client Version
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
> **发布**游戏时必须从源代码中删除调用setDebugMode或将参数更改为false。




### Initialize

**toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ...})调用**，对Gamebase SDK进行初始化<br/>

```js
function gamebaseInitialize() {
    var gamebaseConfiguration = {
        appId:'T0asTC1oud',    // TOAST Console Project ID
        clientVersion:'1.0.0', // TOAST Console Gamebase App Client Version
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
            // 请确认是否正常输入了appId、clientVersion及TOAST控制台的设置。
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

### Launching Status

可通过toast.Gamebase.initialize调用结果确认推出状态。
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
| IN_SERVICE                  | 200  | 正在正常服务                                 |
| RECOMMEND_UPDATE            | 201  | 建议升级                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 检查中无法使用服务，但以QA终端机注册时，无论是否检查，都可访问服务进行测试。|
| IN_TEST                     | 203  | 正在测试 |
| IN_REVIEW                   | 204  | 正在审查 |
| REQUIRE_UPDATE              | 300  | 必须升级                                  |
| BLOCKED_USER                | 301  | 因访问断开，利用注册的终端机（设备密钥）访问服务的情况。|
| TERMINATED_SERVICE          | 302  | 服务结束                                   |
| INSPECTING_SERVICE          | 303  | 正在检查服务                                 |
| INSPECTING_ALL_SERVICES     | 304  | 正在检查所有服务                             |
| INTERNAL_SERVER_ERROR       | 500  | 内部服务器错误                                 |
