## Game > Gamebase > Unity SDK使用指南 > 初始化

如需使用Gamebase Unity SDK，必须先执行初始化。此外，还需在TOAST控制台中注册APP ID、APP版本信息。

### Inspector Settings

初始化所需的设置如下。

| 设定值                      | 支持的Platform | 强制(M) / 可选(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| isDebugMode | ALL | O |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| storeCode | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

在Gamebase控制台中注册的项目ID。

[Console Guide](/Game/Gamebase/zh/oper-app/#app)

#### 2. appVersion

在Gamebase控制台中注册的客户端版本。

[Console Guide](/Game/Gamebase/zh/oper-app/#client)

#### 3. isDebugMode

Gamebase Debug的设置。

* true：输出Gamebase的所有日志。
* false：输出Warning、Error日志。
* 默认值：false

调试设置也可在控制台进行，优先考虑在控制台中设置的值。
控制台设置方法请参考如下指南。

* [控制台测试终端机设置](./oper-app/#test-device)
* [控制台客户设置](./oper-app/#client)

如需咨询Gamebase，请将该设置更改为true，并将日志发送到[客服中心 ](https://toast.com/support/inquiry)，我们会尽快回复。

> <font color="red">[注意]</font><br/>
>
> 如需**RELEASE** 游戏，务必将该设置更改为**false**。

#### 4. displayLanguageCode

可以将Gamebase提供的UI及SystemDialog中显示的语言更改为设备上设置的语言以外的语言。

[Display Language](./unity-etc/#display-language)

#### 5. enablePopup

因系统维护、禁用(ban)等原因用户无法玩游戏时，需要通过弹出窗口等方式显示原因。
此设置为是否使用Gamebase提供的默认弹出窗口的设置。

* true：根据enableLaunchingStatusPopup, enableBanPopup的设置决定是否显示弹出窗口。
* false：Gamebase提供的所有弹出窗口均不显示。
* 默认值: false

#### 6. enableLaunchingStatusPopup

此设置是当LaunchingStatus不能进行游戏时是否使用Gamebase提供默认弹出窗口的设置。
LaunchingStatus请参考下面Launching段落下面的State, Code部分。

* 默认值: true

#### 7. enableBanPopup

此设置是当登录时该游戏用户为禁用状态时，是否使用Gamebase提供的默认弹窗的设置。

* 默认值: true

#### 8. storeCode

为初始化TOAST应用程序内结算服务IAP(In-App Purchase)而所需的商店信息。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | TS | only Android |

#### 9. fcmSenderId

使用Firebase Messaging(FCM)所需的Sender ID。

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

此设置为是否在Standalone平台上通过WebView进行登录的设置。

#### 11. GamebaseUnitySDKSettings

上述的设置可以在GamebaseUnitySDKSettings组件的Inspector中进行更改。

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.12.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDK初始化方法如下。

1. 创建空的游戏对象。
2. 将GamebaseUnitySDKSettings.cs文件添加为已创建的游戏对象的组件。
3. 在Inspector中输入初始化设置。
4. 调用Gamebase.Initialize(callback) API。

> <font color="red">[注意]</font><br/>
>
> 请注意，如删除了已创建的游戏对象，则在调用Android, iOS API 后无法接收回调。 <br/>
> 如被意外删除，则显示提示"Do not destroy this gameObject in order to receive callback."。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**示例**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        Gamebase.Initialize((launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error))
            {
                Debug.Log("Gamebase initialization is succeeded.");

                if (IsPlayable(launchingInfo.launching.status.code))
                {
                    Debug.Log("Playable");
                }
                else
                {
                    if (launchingInfo.launching.status.code == GamebaseLaunchingStatus.REQUIRE_UPDATE)
                    {
                        Debug.Log(string.Format("message:{0}", launchingInfo.launching.status.message));
                    }
                }
            }
            else
            {
                Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
            }
        });
    }

    private bool IsPlayable(int status)
    {
        if (status >= 200 && status < 300)
            return true;

        return false;
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
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | 正常服务中 |
| RECOMMEND_UPDATE | 201 | 推荐更新 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | 维护期间该服务不可用，但如果登记为测试设备，则无论维护如何，都可以连接和测试该服务。 |
| IN_TEST                     | 203  | 正在测试 |
| IN_REVIEW                   | 204  | 正在审查 |
| REQUIRE_UPDATE | 300 | 强制更新 |
| BLOCKED_USER                | 301         | 访问权限已被禁用的用户。 |
| TERMINATED_SERVICE          | 302         | 终止服务                                   |
| INSPECTING_SERVICE          | 303         | 服务正在维护中                                 |
| INSPECTING_ALL_SERVICES     | 304         | 所有服务正在维护中                              |
| INTERNAL_SERVER_ERROR       | 500         | 内部服务器出错                                 |

[Console Guide](/Game/Gamebase/zh/oper-app/#app)


**1.2 App**

Gamebase Console中注册的APP信息。

* accessInfo
    * serverAddress：服务器地址
    * csInfo：客服中心信息
* relatedUrls
    * termsUrl：使用条款
    * personalInfoCollectionUrl：隐私条款
    * punishRuleUrl: 禁用规定
    * csUrl : 客服中心
* install: 安装URL
* idP: 认证信息

[Console Guide](/Game/Gamebase/zh/oper-app/#client)

**1.3维护**

在Gamebase Console设置的维护信息。

* url：维护页面URL
* timezone：标准时间段(timezone)
* beginDate：开始时间
* endDate：结束时间
* message：维护理由

[Console Guide](/Game/Gamebase/zh/oper-operation/#maintenance)

**1.4 公告**

在Gamebaes Console中设置的公告信息。

* message：消息
* title：标题
* url：维护URL

[Console Guide](/Game/Gamebase/zh/oper-operation/#notice)

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
 
[Console Guide](/Game/Gamebase/zh/oper-purchase/)

### Get Launching Information

使用GetLaunchingInformations API，Initialize之后也可获取LaunchingInfo对象。

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

### Error Handling

| Error                              | Error Code | 描述            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebase未初始化。 |
| NOT\_LOGGED\_IN       | 2          | 需要登录。            |
| INVALID\_PARAMETER    | 3          | 无效的参数。           |
| INVALID\_JSON\_FORMAT | 4          | JSON格式错误。         |
| USER\_PERMISSION      | 5          | 无权限。              |
| NOT\_SUPPORTED        | 10         | 不支持此功能。         |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)