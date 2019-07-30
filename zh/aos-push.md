## Game > Gamebase > Android SDK 使用指南 > push

### Settings

以下介绍如何为每个平台设置必要的推送通知。

#### TOAST Cloud Console登记

首先参考 [Notification > Push > API v2.0 指南](/Notification/Push/ko/api-guide/)设置 Console。

#### 下载

* 如果您使用Firebase 推送
    * 将下载的 SDK **gamebase-adapter-push-fcm** 文件夹添加到项目中。
* 如果您使用Tencent 推动
    * 将下载的 SDK **gamebase-adapter-push-tencent** 文件夹添加到项目中。

> <font color="red">[重要]</font><br/>
>
> 只能有一个推送模块。<br/>
> 不要同时将Firebase 推送和 Tencent 推送添加到项目中。


#### AndroidManifest.xml

* 添加Gamebase 推送所需的设置。

> <font color="red">[重要]</font><br/>
>
> 需要将**${applicationId}**变更为 **Package name**。
>

*Firebase*

```xml
<manifest>
    ...
    <permission
        android:name="${applicationId}.permission.C2D_MESSAGE"
        android:protectionLevel="signature" />
    <uses-permission android:name="${applicationId}.permission.C2D_MESSAGE" />
    ...
    <application>
    ...
        <provider
            android:name="com.google.firebase.provider.FirebaseInitProvider"
            android:authorities="${applicationId}.firebaseinitprovider"
            android:exported="false"
            android:initOrder="100" />
        <receiver
            android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
            android:exported="true"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />

                <category android:name="${applicationId}" />
            </intent-filter>
        </receiver>
    ...
    </application>
</manifest>
```

*Tencent*

```xml
<manifest>
    ...
    <application>
    ...
        <provider
            android:name="com.tencent.android.tpush.XGPushProvider"
            android:authorities="${applicationId}.AUTH_XGPUSH"
            android:exported="true" />
        <provider
            android:name="com.tencent.android.tpush.SettingsContentProvider"
            android:authorities="${applicationId}.TPUSH_PROVIDER"
            android:exported="false" />
        <provider
            android:name="com.tencent.mid.api.MidProvider"
            android:authorities="${applicationId}.TENCENT.MID.V3"
            android:exported="true" />
    ...
    </application>
</manifest>
```

#### Google服务设定(仅Firebase)

* 使用Gradle build时
    * 要使用Firebase推送，需要google-services.json配置文件。如何在项目中包含配置文件，请参考以下说明 [Firebase 云消息传递](https://firebase.google.com/docs/cloud-messaging/#add_firebase_to_your_app) 。
    * 将 **apply plugin: 'com.google.gms.google-services'**添加到gradle 设置中。
	* 使用上述设置，将应用Google Services Gradle Plugin，并将google-services.json文件重命名为res / google-services / {build_type} /values/values.xml使用。
* 如果使用Unity build
    * 需要创建 string resource(xml) 文件，并将其包含在 Assets/Plugins/Android/res/values/文件夹中。
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 以下是 string resource(xml) 文件的示例。
            ```xml
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

#### 初始化

* Gamebase初始化时调用configuration的**setPushType()**。
* 如果使用Firebase推送
    * 追加调用**setFCMSenderId()**。
* 如果使用Tencent推送
    * 追加调用**setTencentAccessId()**。
    * 追加调用**setTencentAccessKey()**。

```java
private static final String PUSH_FCM_SENDER_ID = "...";
private static final String PUSH_TENCENT_ACCESS_ID = "...";
private static final String PUSH_TENCENT_ACCESS_KEY = "...";

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setFCMSenderId(PUSH_FCM_SENDER_ID)				// Firebase需要 SenderId。
        .setTencentAccessId(PUSH_TENCENT_ACCESS_ID)		// 需要Tencent AccessId。
        .setTencentAccessKey(PUSH_TENCENT_ACCESS_KEY)	// 需要Tencent AccessKey。
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Register Push

调用以下API在TOAST Push注册该用户。<br/>
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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Push库错误。<br>请确认DetailCode。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 上一次的推送API调用未完成。<br>上一次推送 API回调执行后请重新调用。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 未知推送错误。<br>请将全部的Log上传到[客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

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

* TOAST Push 오류 코드는 다음과 같습니다.
    
| 오류 코드 |  설명 |
| --- | --- |
| ERROR_SYSTEM_FAIL | 시스템 문제로 토큰 획득에 실패한 경우 |
| ERROR_NETWORK_FAIL | 네트워크 문제로 요청에 실패한 경우 |
| ERROR_SERVER_FAIL | 서버에서 실패 응답을 반환한 경우 |
| ERROR_ALREADY_IN_PROGRESS | 토큰 등록/조회가 이미 실행 중인 경우 |
| ERROR_INVALID_PARAMETERS | 파라미터가 잘못된 경우 |
| ERROR_PERMISSION_REQUIRED | 권한이 필요한 경우(Tencent만 해당) |
| ERROR_PARSE_JSON_FAIL | 서버 응답을 파싱하지 못한 경우 |




