## Game > Gamebase > Android SDK 使用指南 > push

### Settings

以下介绍如何为每个平台设置必要的推送通知。

#### Register NHN Cloud Console

参考[Notification > Push > Console Guide](/Notification/Push/zh/console-guide/)设置Console。

#### Gradle

* 在build.gradle中添加要使用的模块。

```groovy
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    
    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'
    
    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"
}
```

* 推送模块应在FCM(Firebase Cloud Messaging)与Tencent中仅添加一种，但若按照测试目的添加了多个模块，也可以选择欲在Gamebase.initialize中使用的PushType。
    * PushProvider.Type.FCM : "FCM"
    * PushProvider.Type.TENCENT : "TENCENT"

```java
String PUSH_TYPE = PushProvider.Type.FCM;	// Firebase Cloud Messaging

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
		.setPushType(PUSH_TYPE)
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### Google服务设定(仅Firebase)

* 使用Gradle build时
    * F为使用Firebase推送，应按照如下指南完成Firebase设置。
		* [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud Push > Android > 设置Firebase Cloud Messaging](/toast-sdk/push-android/#firebase-cloud-messaging)
* 如果使用Unity build
    * 需要创建 string resource(xml) 文件，并将其包含在 Assets/Plugins/Android/res/values/文件夹中。
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 以下是 string resource(xml) 文件的示例。
            ```
            <!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
            <?xml version="1.0" encoding="utf-8"?>
            <resources>
            <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
            <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
            <string name="firebase_database_url" translatable="false">https://tap-development-00000000.firebaseio.com</string>
            <string name="google_app_id" translatable="false">1:000000000000:android:749cbe01c8ada279</string>
            <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
            <string name="google_storage_bucket" translatable="false">tap-development-00000000.appspot.com</string>
            </resources>
            ```
        * string resource(xml) 文件中设置的各值为 Firebase Console > 项目设置 >下载并查看google-services.json文件。 
            根据Firebase服务联动，google-services.json文件的内容可能会有所不同。
            ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)

#### Tencent

* 为使用Tencent推送，应按照如下指南完成Tencent设置。
	* [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud Push > Android > 设置Tencent Push Notification](/toast-sdk/push-android/#tencent-push-notification)

### Register Push

调用以下API在NHN Cloud Push注册该用户。<br/>
接受来自用户的推送协议（enablePush），广告推送协议（enableAdPush），夜间广告推送协议（enableAdNightPush）的值，并调用以下API完成注册。

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, PushConfiguration configuration, GamebaseCallback callback);
```

**示例**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = new PushConfiguration(enablePush, enableAdPush, enableAdNightPush);

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Register push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Request Push Settings

要查询用户的推送设置，请使用以下API。<br/>
可以通过来自回调的PushConfiguration值获取用户设置值。

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, GamebaseDataCallback<PushConfiguration> callback);
```

**示例**

```java
Gamebase.Push.queryPush(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(PushConfiguration data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.pushEnabled;
            boolean enableAdPush = data.adAgreement;
            boolean enableAdNightPush = data.adAgreementNight;
        } else {
            // Failed.
            Log.e(TAG, "Query push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Push库错误。<br>请确认DetailCode。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 上一次的推送API调用未完成。<br>上一次推送 API回调执行后请重新调用。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 未知推送错误。<br>请将全部的Log上传到[客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 全部错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 这是在NHN Cloud Push库中发生的错误。
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

* NHN Cloud Push SDK错误代码如下。

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | 调用API成功 |
| NOT_INITIALIZE | 100 | 未对NHN Cloud SDK或NHN Cloud Push SDK进行初始化时。 |
| PROVIDER_SDK_ERROR | 101 | 外部SDK(Firebase, Tencent)发生错误时。 |
| USER_ID_NOT_REGISTERED | 102 | 未登录时。 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType输入错误或项目中不包含推送库。 |
| API_SERVER_ERROR | 104 | 调用NHN Cloud Push服务器API失败时 |
| TOKEN_NOT_REGISTERED | 105 | 内部缓存的Push Token不存在时。 |
