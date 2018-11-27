## Game > Gamebase > Android SDK使用指南> 初始化

在使用Gamebase Android SDK之前，必须先执行初始化。

### 启用App

要管理App的生命周期(lifecycle)，须通知Gamebase SDK您的应用处于有效状态。<br/>
**Application#onCreate()**调用 **Gamebase#activeApp(Context)**。

```java
public class GamebaseApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Gamebase.activeApp(getApplicationContext());
    }
}
```

### 配置设定

初始化Gamebase时，可以使用GamebaseConfiguration.Builder对象修改Gamebase设置。

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| Builder(String appId, String appVersion) | **M**                      | 需要使用appId和appVersion，初始化gamebaseConfiguration.Builder构造函数作为必要参数。<br/><br/> ** appId **是TOAST Project发放的App的ID。<br/> **appVersion**用于判断游戏是处于服务状态、更新状态还是维护状态。请指定游戏版本。 |
| build()                                  | **M**                      | 将设置完的 Builder转换为Configuration对象。<br/>**Gamebase.initialize()** API要求。 |
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>因系统维护或设置禁用（ban）等原因，导致游戏用户无法玩游戏的状态下，有时需要通过弹出窗口显示原因。<br/>如果设置为**true**，Gamebase将在该情况下自动弹出窗口公告信息。<br/>默认值为 **false**。<br/>**false**的情况下，从Launching结果中获取信息，并使用自定义UI，显示用户无法玩游戏的原因。 |
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>根据Launching结果，可以更改Gamebase是否在无法登录时，自动显示弹出窗口（维护状态为主）。<br/>仅适用于**enablePopup(true)** 状态下。<br/>默认值为 **true**。 |
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>当游戏用户被禁用时，Gamebase可以设定是否将制裁原因以弹出窗口的形式显示给用户。<br/>仅适用于**enablePopup(true)**状态下。<br/>默认值为 **true**。 |
| setStoreCode(String storeCode)           | O                          | **[Purchase]**<br/>如果要使用TOAST的IAP(In-App Purchase)服务，则需要设置商店类型。<br/>有关参数，请参考 [IAP指南](/Mobile%20Service/IAP/ko/Overview/) |
| setFCMSenderId(String senderId)          | O                          | **[Push]**<br/>如果要通过Google Notification（FCM，GCM）发送推送消息，则需要设置发件人ID。 |
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O                          | **[Push]**<br/> 如果您使用的是腾讯推送模块，则需要设置访问密钥和访问ID。|

### Debug模式
* Gamebase仅显示警告(warning)和错误日志。
* 要打开系统日志进行开发，请调用** Gamebase.setDebugMode（true）**。

> <font color="red">[注意]</font><br/>
>
> 当**发布**游戏时，请务必从源代码中删除setDebugMode调用，或者将参数更改为false之后再打包。

### 初始化

通过在**Activity#onCreate(Bundle)**调用**Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**来初始化Gamebase SDK。<br/>
另外，还需要从**Activity#onActivityResult(int, int, Intent)**调用 **Gamebase.onActivityResult(int, int, Intent)**，以进行Gamebase的正常操作。

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
         * Gamebase Configuration.
         */
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(appId, appVersion)
                                            .enableLaunchingStatusPopup(true)
                                            .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // 确认Launching状态。
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // 玩游戏
                    } else {
                        // 维护或更新App。
                    }
                } else {
                    // 如果初始化失败，您将无法使用Gamebase SDK。
                    // 显示错误，重新启动或关闭游戏。
                    Log.e(TAG, "Initialize failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });

        /**
         * Show gamebase debug message.
		 * set 'false' when build RELEASE.
         */
        Gamebase.setDebugMode(true);
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

### Launching状态

可以通过调用Gamebase#initialize来确认Launching状态。

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            //确认Launching状态。
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // 维护中...
                    break;
                ...
            }
            ...
        }
        ...
    }
});
```

使用getLaunchingInformations API，允许在初始化后获取LaunchingInfo对象。

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```





### Launching状态码

| 状态                      | Code | 描述                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 正常服务中                             |
| RECOMMEND_UPDATE            | 201  | 推荐更新                                 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 维护期间该服务不可用，但如果登记为测试设备，则无论维护如何，都可以连接和测试该服务。|
| REQUIRE_UPDATE              | 300  | 强制更新                                |
| BLOCKED_USER                | 301  | 访问权限已被禁用的用户。|
| TERMINATED_SERVICE          | 302  | 终止服务                                  |
| INSPECTING_SERVICE          | 303  | 服务正在维护中                             |
| INSPECTING_ALL_SERVICES     | 304  | 所有服务正在维护中                              |
| INTERNAL_SERVER_ERROR       | 500  | 内部服务器错误                                 |




### Error处理

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | Gamebase未初始化。 |
| NOT_LOGGED_IN                | 2          | 需要登录。            |
| INVALID_PARAMETER            | 3          | 无效的参数。          |
| INVALID_JSON_FORMAT          | 4          | JSON格式错误。         |
| USER_PERMISSION              | 5          | 无权限。               |
| NOT_SUPPORTED                | 10         | 不支持此功能。       |
| NOT_SUPPORTED_ANDROID        | 11         | Android不支持此功能。|
| ANDROID_ACTIVEAPP_NOT_CALLED | 32         | 未调用activeApp API。|


* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
