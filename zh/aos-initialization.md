## Game > Gamebase > Android SDK使用指南> 初始化

在使用Gamebase Android SDK之前，必须先执行初始化。

### onActivityResult

还需要从**Activity#onActivityResult(int, int, Intent)**调用 **Gamebase.onActivityResult(int, int, Intent)**，以进行Gamebase的正常操作。

**API**

```java
+ (void)Gamebase.onActivityResult(int requestCode, int resultCode, Intent data);
```

### Initialization Flow

按照如下程序，当游戏开始时设置调试，初始化Gamebase，根据Launching Status Code判断是否应进入游戏。
![initialization flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

初始化Gamebase时，可以使用GamebaseConfiguration.Builder对象更改Gamebase设置。

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| newBuilder(String appId, String appVersion, String storeCode) | **M**                      | 可以使用newBuilder()函数生成GamebaseConfiguration.Builder对象。<br/><br/>**appId**是NHN Cloud Project发放的App的ID。<br/>**appVersion**用于判断游戏是处于服务状态、更新状态还是维护状态。请指定游戏版本。 <br/> **storeCode**是代表APK分配的商店的代码。以下指南中有关于各商店代码的说明。
| build()                                  | **M**                      | 将设置完的 Builder转换为Configuration对象。<br/>**Gamebase.initialize()** API要求。 |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>因系统维护或设置禁用（ban）等原因，导致游戏用户无法玩游戏的状态下，有时需要通过弹出窗口显示原因。<br/>如果设置为**true**，Gamebase将在该情况下自动弹出窗口公告信息。<br/>默认值为 **false**。<br/>**false**的情况下，从Launching结果中获取信息，并使用自定义UI，显示用户无法玩游戏的原因。 |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>根据Launching结果，可以更改Gamebase是否在无法登录时，自动显示弹出窗口（维护状态为主）。<br/>仅适用于**enablePopup(true)** 状态下。<br/>默认值为 **true**。 |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>当游戏用户被禁用时，Gamebase可以设定是否将制裁原因以弹出窗口的形式显示给用户。<br/>仅适用于**enablePopup(true)**状态下。<br/>默认值为 **true**。 |

### Debug Mode
* Gamebase仅显示警告(warning)和错误日志。
* 要打开系统日志进行开发，请调用** Gamebase.setDebugMode（true）**。

> <font color="red">[注意]</font><br/>
>
> 当**发布**游戏时，请务必从源代码中删除setDebugMode调用，或者将参数更改为false之后再打包。

调试设置也可在控制台进行，优先考虑在控制台中设置的值。
控制台设置方法请参考如下指南。

* [控制台测试终端机设置](./oper-app/#test-device)
* [控制台客户设置](./oper-app/#client)


### Initialize

通过在**Activity#onCreate(Bundle)**调用**Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**来初始化Gamebase SDK。

**API**

```java
+ (void)Gamebase.initialize(Activity activity, GamebaseConfiguration configuration, GamebaseDataCallback<LaunchingInfo> callback);
```

**示例**

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Show gamebase debug message.
         * set 'false' when build RELEASE.
         */
        Gamebase.setDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        String storeCode = "GG";
        GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(appId, appVersion, storeCode)
                                            .enableLaunchingStatusPopup(true)
                                            .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // 请根据启动代码判断是否允许进入游戏。
                    ...
                } else {
                    // 如果初始化失败，您将无法使用Gamebase SDK。
                    // 显示错误，重新启动或关闭游戏。
                    Log.e(TAG, "Initialize failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });

        ...
    }
    ...

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        /**
         * Pass onActivityResult event to the Gamebase.
         */
        Gamebase.onActivityResult(requestCode, resultCode, data);
        ...
    }
}
```

### Launching Information

可以通过调用Gamebase#initialize来确认Launching状态。<br/>
请根据启动代码判断是否玩游戏。

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            //确认Launching状态。
            boolean canPlay = true;
            String errorLog = "";
            switch (launchingInfo.getStatus().getCode()) {
                case LaunchingStatus.IN_SERVICE:
                    break;
                case LaunchingStatus.RECOMMEND_UPDATE:
                    Log.d(TAG, "There is a new version of this application.");
                    break;
                case LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST:
                case LaunchingStatus.IN_TEST:
                case LaunchingStatus.IN_REVIEW:
                case LaunchingStatus.IN_BETA:
                    Log.d(TAG, "You logged in because you are developer.");
                    break;
                case LaunchingStatus.REQUIRE_UPDATE:
                    canPlay = false;
                    errorLog = "You have to update this application.";
                    break;
                case LaunchingStatus.BLOCKED_USER:
                    canPlay = false;
                    errorLog = "You are blocked user!";
                    break;
                case LaunchingStatus.TERMINATED_SERVICE:
                    canPlay = false;
                    errorLog = "Game is closed!";
                    break;
                case LaunchingStatus.INSPECTING_SERVICE:
                case LaunchingStatus.INSPECTING_ALL_SERVICES:
                    canPlay = false;
                    errorLog = "Under maintenance.";
                    break;
                case LaunchingStatus.INTERNAL_SERVER_ERROR:
                default:
                    canPlay = false;
                    errorLog = "Unknown internal error.";
                    break;
            }
            if (canPlay) {
                // 开始玩游戏。
            } else {
                // 显示不可玩游戏的原因并终止游戏，
            }
        }
        ...
    }
});
```

使用getLaunchingInformations API，允许在初始化后获取LaunchingInfo对象。

> <font color="red">[注意]</font><br/>
>
> getLaunchingInformations() API不是实时从服务器获取信息的异步API。
> 因每两分钟返还被更新的现金信息，不适合实时判断当前是否维护。
> 在这种情况下，请使用Launching Status Code被更改时启动事件的GamebaseEventHandler。
> [Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Observer](./aos-etc/#observer)

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
LaunchingInfo对象中包含Gamebase Console中设置的值和游戏状态等。


#### 1. Launching

是Gamebase启动信息。

**1.1 Status**

是Gamebase Android SDK初始化设置中输入的应用程序版本的游戏状态信息。

* code: 游戏状态代码（正在检查、必须升级、结束服务等）
* message: 游戏状态信息

状态代码请参考下表。

##### Launching Status Code

| 状态                      | Code | 描述                              |
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

[Game > Gamebase > 控制台使用指南> APP > App](./oper-app/#app)

**1.2 App**

是Gamebase Console中创建的应用程序信息。

* accessInfo
    * serverAddress: 服务器地址
* customerService
    * accessInfo : 客服中心信息
    * type : 客服中心的类型
    * url : 客服中心URL
* relatedUrls
    * termsUrl: 使用条款
    * personalInfoCollectionUrl: 同意个人信息
    * punishRuleUrl: 停止使用规定
* install: 安装URL
* idP: 验证信息

[Game > Gamebase > 控制台使用指南> APP > Client](./oper-app/#client)

**1.3 Maintenance**

是Gamebase Console中创建的检查信息。

* url: 检查页面URL
* timezone: 标准时间段(timezone)
* beginDate: 开始时间
* endDate: 结束时间
* message: 检查原因

[Game > Gamebase > 控制台使用指南 > 运营 > Maintenance](./oper-operation/#maintenance)

**1.4 Notice**

是Gamebase Console中创建的公告信息。

* message: 信息
* title: 标题
* url: 检查URL

[Game > Gamebase > 控制台使用指南 > 运营 > Notice](./oper-operation/#notice)

#### 2. tcProduct

是与Gamebase相关的NHN Cloud服务的appKey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

是NHN Cloud Console中创建的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code

[Game > Gamebase > 控制台使用指南 > IAP](./oper-purchase/)

#### 4. tcLaunching

是NHN Cloud Launching Console中用户输入的信息

* 用户输入的值传至JSON string。
* NHN Cloud Launching具体设置请参考如下指南。

[Game > Gamebase > 操控台使用指南 > 管理 > Config](./oper-management/#config)


### Handling Unregistered Version

如果初始化未注册在Gamebase Console上的GameClientVersion，则出现**LAUNCHING_UNREGISTERED_CLIENT(2004)**错误信息。
如果是enablePopup(true)、enableLaunchingStatusPopup(true)状态，则弹出强制更新窗口并可跳转到商店。
若不使用Gamebase弹窗，则可从GamebaseException对象获取UpdateInfo创建UI，允许用户跳转到商店。

**VO**

```java
class UpdateInfo {
    // 是可下载最新版本的”安装商店URL”。
    String installUrl;
    // 按用户终端机设置的语言向用户显示信息。
    // 语言为”en”时，提示以下消息。 
    // 'The version is not supported. Please get the latest update version.‘
    String message;
}
```

**API**

```java
+ (UpdateInfo)UpdateInfo.from(GamebaseException exception);
```

**Example**

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Gamebase initialization succeeded.
        } else {
            // Gamebase initialization failed.

            UpdateInfo updateInfo = UpdateInfo.from(exception);
            if (updateInfo != null) {
                // Unregistered game client version.
                // Open market url to update application.
                updateInfo.installUrl; // Market URL.
                updateInfo.message;    // Message from launching server.
                return;
            }

            // Another initialization error.
            ...
        }
        ...
    }
});
```

### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | 未初始化Gamebase。 |
| NOT_LOGGED_IN                | 2          | 需要登录。            |
| INVALID_PARAMETER            | 3          | 是无效的参数。          |
| INVALID_JSON_FORMAT          | 4          | JSON格式错误         |
| USER_PERMISSION              | 5          | 无权限              |
| NOT_SUPPORTED                | 10         | 不支持此功能。       |
| NOT_SUPPORTED_ANDROID        | 11         | Android不支持此功能。 |
| ANDROID_ACTIVEAPP_NOT_CALLED | 32         | 未调用activeApp API。 |


* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
