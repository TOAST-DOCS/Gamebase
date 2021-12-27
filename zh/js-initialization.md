## Game > Gamebase > JavaScript SDK使用指南 > 初始化

若欲使用Gamebase JavaScript SDK，请先进行初始化。

### Configuration Settings

初始化Gamebase时，可利用GamebaseConfiguration对象更改Gamebase设置。

| KEY                                                 | Mandatory(M) / Optional(O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId:string                                       | **M**                      | 是NHN Cloud Project ID。您必须输入。|
| clientVersion:string                               | **M**                      | 通过游戏版本确认是否是服务、检查、通知事项等可玩游戏的状态。<br/> 请输入“NHN Cloud Console > Gamebase > App > Client Version > WEB”版本。|
| enableDebugMode:boolean                            | O                          | 可以启用Debug Mode。在开发人员控制台中将显示Debug log。<br/> 默认值为**false**。|
| uiConfiguration.enablePopup:boolean                | O                          | 因**[UI]**<br/>系统检查、禁用(ban)游戏用户无法玩游戏时，通过弹窗提示原因。<br/> 若设置为**true**，Gamebase在相应情况下自动显示信息弹窗。<br/> 默认值为**false**。<br/>在**false**状态下，请通过推出结果获得信息后实现自主UI，显示无法玩游戏的原因。|
| uiConfiguration.enableLaunchingStatusPopup:boolean | O                          | 由于**[UI]**<br/>推出结果无法登录的状态（主要为检查状态）下，可更改Gamebase是否自动显示弹窗。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| uiConfiguration.enableBanPopup:boolean             | O                          | **[UI]**<br/>游戏用户为禁用状态时，可更改Gamebase是否通过自动弹窗来显示禁用原因。<br/>仅在**enablePopup(true)**状态下运行。<br/>默认值为**true**。|
| displayLanguageCode:string                         | O                          | 是Gamebase UI中使用的语言代码。<br/> 默认值为**浏览器的language**(navigator.language)。|
| displayLanguageTable:json                          | O                          | 是显示Gamebase UI时使用的文本源(text resource)。<br/> 此值与SDK内的语言集合表合并使用。<br/> 可通过“toast.Gamebase.getDisplayLanguageTable()”确认SDK内的语言集合表和格式。|


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
是用于Gamebase调试的设置。 
* true : 显示Gamebase的所有日志。
* false : 显示警告(warning)、错误(error)日志。
* 默认值：false

还可在控制台中进行调试设置，请优先考虑在控制台中设置的值。
关于控制台设置方法，请参考如下指南。

* [控制台测试终端机的设置](./oper-app/#test-device)
* [控制台客户的设置](./oper-app/#client)


若要打开开发时参考的系统日志，请调用**toast.Gamebase.setDebugMode(true)**。
> <font color="red">[注意]</font><br/>
>
> **发布**游戏时，必须在Source code中删除setDebugMode调用或将参数更改为false。
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
            // 若初始化失败，则无法使用Gamebase SDK。
            // 请确认是否正常输入appId、clientVersion及NHN Cloud控制台设置。
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // 关于Status值，请参考下端Launching Status Code表。
            // 是可玩游戏的状态。
            console.log('Playable!');
        } else {
            // 是不可玩游戏的状态（检查、服务结束等）
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

通过调用"toast.Gamebase.Launching.getLaunchingInformations()" API，初始化后可获取LaunchingInfo对象。

> <font color="red">[注意]</font><br/>  
>
> **toast.Gamebase.Launching.getLaunchingInformations()** API不是实时从服务器获取信息的异步API。 
> 因每2分钟返还更新的现金信息，不适合实时判断当前是否维护。
> 在上述情况下，当Launching Status Code被更改时，请适用启动事件的GamebaseEventHandler。
> [Game > Gamebase > Javascript SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Observer](./js-etc/#observer)

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
在LaunchingInfo对象包含Gamebase Console中设置的多个值和游戏状态等信息。


#### 1. Launching

是Gamebase推出信息。

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
| BLOCKED_USER                | 301  | 访问权限已被禁用的用户 |
| TERMINATED_SERVICE          | 302  | 终止服务                                  |
| INSPECTING_SERVICE          | 303  | 服务正在维护中                             |
| INSPECTING_ALL_SERVICES     | 304  | 所有服务正在维护中                              |
| INTERNAL_SERVER_ERROR       | 500  | 内部服务器错误                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

是登记在Gamebase Console中的App信息。

* accessInfo
    * serverAddress : 服务器地址
    * csInfo : 客户服务信息
* relatedUrls
    * termsUrl : 服务条款
    * personalInfoCollectionUrl : 个人隐私协议
    * punishRuleUrl: 禁用条款
    * csUrl : 客户服务
* install : 安装URL
* idP : 认证信息

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

是登记在Gamebase Console中的维护信息。

* url : 维护页面URL
* timezone : 标准时区
* beginDate : 开始时间
* endDate : 终止时间
* message : 维护原因

[Console Guide](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

是登记在Gamebase Console中的公告信息。

* message : 消息
* title : 标题
* url : 维护URL

[Console Guide](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

是与Gamebase有关的NHN Cloud服务appKey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

是登记在NHN Cloud Console中的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

是登记在NHN Cloud Console中的Launching信息。

* 以JSON string格式传送用户在Console中输入的值。
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)


### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | 未初始化Gamebase。 |
| NOT_LOGGED_IN                | 2          | 需要登录。            |
| INVALID_PARAMETER            | 3          | 错误参数           |
| INVALID_JSON_FORMAT          | 4          | JSON格式错误          |
| USER_PERMISSION              | 5          | 没有权限。               |
| NOT_SUPPORTED                | 10         | 是不支持的功能。        |


* 关于所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
