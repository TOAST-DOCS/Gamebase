## Game > Gamebase > Unity SDK使用指南 > 初始化

如需使用Gamebase Unity SDK，必须先执行初始化。此外，还需在TOAST控制台中注册APP ID和APP版本信息。

### GamebaseConfiguration 

初始化所需的设置如下。

| 设定值                      | 支持的Platform | 强制(M) / 可选(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| storeCode | ALL | M |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| enableKickoutPopup | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

在Gamebase控制台中注册的项目ID。

[Game > Gamebase > 控制台使用指南> APP > App](./oper-app/#app)

#### 2. appVersion

在Gamebase控制台中注册的客户端版本。

[Game > Gamebase > 控制台使用指南> APP > Client](./oper-app/#client)

#### 3. storeCode

为初始化TOAST应用程序内结算服务IAP(In-App Purchase)而所需的商店信息。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | 仅限iOS。 |
| Google Play | GG | 仅限Android。 |
| ONE Store | ONESTORE | 仅限Android。 |
| GALAXY Store | GALAXY | 仅限Android。 |
| Huawei AppGallery | HUAWEI | 仅限Android。 |
| MyCard | MYCARD | 仅限Android。 |
| Windows | WIN | only Unity Standalone |
| Web | WEB | only Unity WebGL |

#### 4. displayLanguageCode

可以将Gamebase提供的UI及SystemDialog中显示的语言更改为设备上设置的语言以外的语言。

[Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Display Language](./unity-etc/#display-language)

#### 5. enablePopup

因系统维护、禁用(ban)等原因用户无法玩游戏时，需要通过弹出窗口等方式显示原因。
此设置为是否使用Gamebase提供的默认弹出窗口的设置。

* true：根据enableLaunchingStatusPopup、enableBanPopup的设置决定是否显示弹出窗口。
* false：Gamebase提供的所有弹出窗口均不显示。
* 默认值 : false

#### 6. enableLaunchingStatusPopup

此设置是当LaunchingStatus不能进行游戏时是否使用Gamebase提供默认弹出窗口的设置。
LaunchingStatus请参考下面Launching段落下面的State、Code部分。

* 默认值: true

#### 7. enableBanPopup

此设置是当登录时该游戏用户为禁用状态时，是否使用Gamebase提供的默认弹窗的设置。

* 默认值: true

#### 8. useWebViewLogin

此设置为是否在Standalone平台上通过WebView进行登录的设置。

### Debug Mode
* Gamebase仅显示警告(warning)和错误日志。 
* 如果要打开系统日志进行开发，请调用**Gamebase.SetDebugMode(true)**。

> <font color="red">[注意]</font><br/>
>
> 当**发布**游戏时，请务必从源代码中删除SetDebugMode调用，或者将参数更改为false之后再打包。

可在Console中设置Debug，而在Console中设置的值的优先权最高。
关于Console的设置方法，请参考如下指南。

* [Console测试终端机的设置](./oper-app/#test-device)
* [Console Client的设置](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void SetDebugMode(bool isDebugMode)
```

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
}
```

### Initialize

需要初始化SDK。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**示例**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        /**
         * Show gamebase debug message.
         * set 'false' when build RELEASE.
         */
        Gamebase.SetDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        /**
         * Gamebase Initialize.
         */
        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("Initialization succeeded.");

                //Following notices are registered in the Gamebase Console
                var notice = launchingInfo.launching.notice;
                if (notice != null)
                {
                    if (string.IsNullOrEmpty(notice.message) == false)
                    {
                        Debug.Log(string.Format("title:{0}", notice.title));
                        Debug.Log(string.Format("message:{0}", notice.message));
                        Debug.Log(string.Format("url:{0}", notice.url));
                    }
                }
        
                //Status information of game app version set in the Gamebase Unity SDK initialization.
                var status = launchingInfo.launching.status;
        
                // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
                // refer to GamebaseLaunchingStatus
                if (status.code == GamebaseLaunchingStatus.IN_SERVICE)
                {
                    // Service is now normally provided.
                }
                else
                {
                    switch (status.code)
                    {
                        case GamebaseLaunchingStatus.RECOMMEND_UPDATE:
                        {
                            // Update is recommended.
                            break;
                        }
                        // ... 
                        case GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR:
                        {
                            // Error in internal server.
                            break;
                        }
                    }
                }
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Update is require.
                    }
                }
            }
        });
    }
}
```

### Launching Information

使用Initialize API来初始化Gamebase Unity SDK，LaunchingInfo对象将作为结果值传递。
此LaunchingInfo对象包含了Gamebase Console中设置的值和游戏状态等。

#### 1. Launching

Gamebase发布信息。

**1.1 状态**

Gamebase Unity SDK初始化设置是输入的APP版本的游戏状态信息。

* code：游戏状态码(维护中，强制更新，已下线等)
* message：游戏状态消息

有关状态代码，请参考下表。

| 状态                      | Code | 描述                                    |
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

[Game > Gamebase > 控制台使用指南> APP > App](./oper-app/#app)

**1.2 App**

Gamebase Console中注册的APP信息。

* accessInfo
    * serverAddress : 服务器地址
* customerService
    * accessInfo : 客户服务信息
    * type : 客户服务的类型
    * url : 客户服务URL
* relatedUrls
    * termsUrl : 使用条款
    * personalInfoCollectionUrl : 同意个人信息
    * punishRuleUrl : 停止使用规定
* install : 安装URL
* idP : 验证信息

[Game > Gamebase > 控制台使用指南 > APP > Client](./oper-app/#client)

**1.3维护**

在Gamebase Console设置的维护信息。

* url：维护页面URL
* timezone：标准时间段(timezone)
* beginDate：开始时间
* endDate：结束时间
* message：维护理由
* hideDate : 是否显示维护的开始和结束时间

[Game > Gamebase > 控制台使用指南 > 运营 > Maintenance](./oper-operation/#maintenance)

**1.4 公告**

在Gamebaes Console中设置的公告信息。

* message：消息
* title：标题
* url：维护URL 

[Game > Gamebase > 控制台使用指南 > 运营 > Notice](./oper-operation/#notice)

#### 2. tcProduct

与Gamebase关联的TOAST服务的appKey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

在TOAST Console中登记的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Game > Gamebase > 控制台使用指南 > IAP](./oper-purchase/)

#### 4. tcLaunching

是TOAST Launching Console中用户输入的信息

* 用户输入的值传至JSON string。
* TOAST Launching具体设置请参考如下指南。
 
[Game > Gamebase > 操控台使用指南 > 管理 > Config](./oper-management/#config)

### Get Launching Information

使用GetLaunchingInformations API，Initialize之后也可获取LaunchingInfo对象。

> <font color="red">[注意]</font><br/>
>
> GetLaunchingInformations API不是从服务器实时获取信息的异步API。
> 因每两分钟返还更新的现金信息，不适合实时判断当前是否维护。
> 在此情况下，请使用Launching Status Code被更改时启动事件的GamebaseEventHandler。
> [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Observer](./unity-etc/#observer)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Launching.LaunchingInfo GetLaunchingInformations()
```

**示例**

```cs
public GamebaseResponse.Launching.LaunchingInfo GetLaunchingInformations()
{
    return Gamebase.Launching.GetLaunchingInformations();
}
```

### Handling Unregistered Version
 	 
初始化未在Gamebase控制台中注册的GameClientVersion时，将出现**LAUNCHING_UNREGISTERED_CLIENT(2004)**错误。
如果是enablePopup(true)、enableLaunchingStatusPopup(true)状态，则显示强制更新弹窗，并可跳转到商店。
若不使用Gamebase弹窗，则可从GamebaseError对象获取商店URL等的Update信息。 

**VO**

```cs
public class UpdateInfo {
	// 是可以下载最新版本的”安装商店的URL”。
	string installUrl;
    // 按用户终端机设置的语言向用户显示信息。
    // 语言为”ko”时，提示以下消息。
    // ”是不支持的版本。请更新至最新版本。”
    string message;
}
```

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
GamebaseResponse.Launching.UpdateInfo GamebaseResponse.Launching.UpdateInfo.From(GamebaseError error);
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                // Gamebase initialization succeeded.
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Unregistered game client version.
                        // Open market url to update application.
                        string installUrl = updateInfo.installUrl; // Market URL.
                        string message updateInfo.message; // Message from launching server.
                    }
                }
            }
        });
    }
}
```

### Error Handling

| Error                              | Error Code | 描述            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | 未初始化Gamebase。 |
| NOT\_LOGGED\_IN       | 2          | 需要登录。            |
| INVALID\_PARAMETER    | 3          | 无效的参数           |
| INVALID\_JSON\_FORMAT | 4          | JSON格式错误         |
| USER\_PERMISSION      | 5          | 无权限。              |
| NOT\_SUPPORTED        | 10         | 不支持此功能。         |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)