## Game > Gamebase > Android SDK使用指南 > push

### Settings

#### Android Notification Icon

```
Not translated yet
```

### Settings  

#### Android Notification Icon

在基本状态下推送通知时一般使用应用程序图标。

但Android上的推送图标只有在使用应用了alpha区域单色图像时，才能正确地显示。
[Material Design指南 - Android Notification \[LINK\]](https://material.io/design/platform-guidance/android-notifications.html#anatomy-of-a-notification)

由于图标大小由终端机分辨率决定，请参考以下规则设置推送图标。

* MDPI - 24 x 24  (drawable-mdpi)
* HDPI - 36 x 36  (drawable-hdpi)
* XHDPI - 48 x 48  (drawable-xhdpi)
* XXHDPI - 72 x 72  (drawable-xxhdpi)
* XXXHDPI - 96 x 96  (drawable-xxxhdpi)

推送图标文件名中不能包含空格、大写英文字母和特殊字符。

请参考以下Notification Options指南设置**default_small_icon**(AndroidManifest.xml)或**setSmallIconName**(API)。
[Game > Gamebase > Android SDK使用指南 > 开始 > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)
[Game > Gamebase > Android SDK使用指南 > 推送 > Notification Options > Set Notification Options with RegisterPush in Runtime](./aos-push/#set-notification-options-with-registerpush-in-runtime)

### Register Push

调用以下API在TOAST Push注册用户。<br/>
接受来自用户的推送协议（enablePush）、广告推送协议（enableAdPush）、夜间广告推送协议（enableAdNightPush）的值，并调用以下API完成注册。

> <font color="red">[注意]</font><br/>
>
> 由于每个UserID的推送设置有可能不同，并且推送令牌也可能过期，登录后推荐每次都调用registerPush API。

**API**

```java
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseCallback callback);
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseNotificationOptions notificationOptions,
                                   @NonNull GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = PushConfiguration.newBuilder()
        .enablePush(enablePush)
        .enableAdAgreement(enableAdPush)
        .enableAdAgreementNight(enableAdNightPush)
        .build();

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

### Notification Options

* 通过使用Notification Options功能可更改终端机显示的推送通知类型。 
* 需要进行更改时在AndroidManifest.xml文件中定义或在运行时调用registerPush API。

#### Set Notification Options with AndroidManifest.xml

设置时在AndroidManifest.xml文件中声明。<br/>
关于Notification Options的设置方法，请参考如下指南。
[Game > Gamebase > Android SDK使用指南 > 开始 > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)

#### Set Notification Options with RegisterPush in Runtime

需要设置时，在AndroidManifest.xml中声明或在运行时设置（在运行时可修改在文件中定义的值）。
调用registerPush API时，通过添加GamebaseNotificationOptions参数设置Notification Options。
如果使用GamebaseNotificationOptions.newBuilder()参数传送”调用Gamebase.Push.getNotificationOptions()获得的结果"，则生成使用当前的Notification Options功能初始化的Builder。通过该Builder可以更改值。<br/>
以下为可以设置的值。

| API                   | Parameter       | Description        |
| --------------------  | ------------ | ------------------ |
| enableForeground      | boolean      | App为Foreground状态时是否允许推送通知<br/>**default**: false  |
| enableBadge           | boolean      | 是否使用Badge图标<br/>**default**: true |
| setPriority           | int          | 推送通知的优先顺序-可以设置以下5个值。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2<br/>**default**: NoticationComapt.HIGH |
| setSmallIconName         | String       | 手机顶端状态栏的通知提示小图标文件名称<br/>-如果不设置，则使用APP图标。<br/>**default**: null |
| setSoundFileName      | String       | 提示音文件名称-仅在Android 8.0以下的OS上运行。<br/>如果指定”res/raw”文件夹的mp3、wav文件名称，则可更改提示音。<br/>**default**: null |

**示例**

```java
boolean enableForeground;
boolean enableBadge;
// NotificationCompat.PRIORITY_MIN     : -2
// NotificationCompat.PRIORITY_LOW     : -1
// NotificationCompat.PRIORITY_DEFAULT :  0
// NotificationCompat.PRIORITY_HIGH    :  1
// NotificationCompat.PRIORITY_MAX     :  2
int priority;
String smallIconName;
String soundFileName;

GamebaseNotificationOptions currentOptions = Gamebase.Push.getNotificationOptions(activity);
GamebaseNotificationOptions notificationOptions = GamebaseNotificationOptions.newBuilder(currentOptions)
        .enableForeground(enableForeground)
        .enableBadge(enableBadge)
        .setPriority(priority)
        .setSmallIconName(smallIconName)
        .setSoundFileName(soundFileName)
        .build();

Gamebase.Push.registerPush(activity, pushConfiguration, notificationOptions, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### Get NotificationOptions

通过调用Get NotificationOptions，注册推送时可获取已设置的通知选项值。
可以获取（包括在AndroidManifest.xml文件中定义的值和在运行时更改的值）最新设置值。

**API**

```java
+ (GamebaseNotificationOptions)Gamebase.Push.getNotificationOptions(@NonNull Context context);
```

### Request Push Settings

要查询用户的推送设置，请使用以下API。<br/>
可以通过来自回调的GamebasePushTokenInfo值获取用户设置值。

**API**

```java
+ (void)Gamebase.Push.queryTokenInfo(@NonNull Activity activity,
                                     @NonNull GamebaseDataCallback<GamebasePushTokenInfo> callback);

// Legacy API
+ (void)Gamebase.Push.queryPush(@NonNull Activity activity,
                                @NonNull GamebaseDataCallback<PushConfiguration> callback);
```

**示例**

```java
Gamebase.Push.queryTokenInfo(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(GamebasePushTokenInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.agreement.pushEnabled;
            boolean enableAdPush = data.agreement.adAgreement;
            boolean enableAdNightPush = data.agreement.adAgreementNight;
            String token = data.token;
        } else {
            // Failed.
        }
    }
});
```

#### GamebasePushTokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | String                | 推送令牌种类       |
| token               | String                | 令牌                 |
| userId              | String                | 用户ID         |
| deviceCountryCode   | String                | 国家代码           |
| timezone            | String                | 标准时区           |
| registeredDateTime  | String                | 令牌更新时间    |
| languageCode        | String                | 语言设置            |
| agreement           | GamebasePushAgreement | 是否同意接收        |

#### GamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | boolean | 是否允许显示推送通知          |
| adAgreement      | boolean | 是否同意接收广告性推送通知      |
| adAgreementNight | boolean | 是否允许在夜间显示广告性推送通知  |

### Event Handling

* 当接受了推送消息或点击推送消息时可处理事件。
* 关于事件注册方法，请参考GamebaseEventHandler指南。
    * [ Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./aos-etc/#push-received-message)
    * [ Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./aos-etc/#push-click-message)
    * [ Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./aos-etc/#push-click-action)

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Push库错误<br>请确认DetailCode。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 上一次的推送API调用未完成。<br>上一次推送API回调执行后请重新调用。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 未知推送错误<br>请将全部的Log上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 全部错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 这是在TOAST Push库中发生的错误。
* 检查错误代码的方法如下。

```java
Gamebase.Push.registerPush(activity, pushConfiguration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Register push successful");
            ...
        } else {
            Log.e(TAG, "Register push failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PUSH_EXTERNAL_LIBRARY_ERROR) {
                // TOAST Push Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* TOAST Push SDK错误代码如下。

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | 调用API成功 |
| NOT_INITIALIZE | 100 | 未对TOAST SDK或TOAST Push SDK进行初始化时 |
| PROVIDER_SDK_ERROR | 101 | 外部SDK(Firebase)发生错误时 |
| USER_ID_NOT_REGISTERED | 102 | 未登录时 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType输入错误或项目中不包含推送库 |
| API_SERVER_ERROR | 104 | 调用TOAST Push服务器API失败时 |
| TOKEN_NOT_REGISTERED | 105 | 内部缓存的Push Token不存在时 |
| INVALID_PARAMETER | 106 | 为错误参数时 |
