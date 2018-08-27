## Game > Gamebase > Android SDK ご利用ガイド > Push

### Settings

ここではプラットフォームごとにPush通知を使用するために必要な設定方法についてご案内いたします。

#### TOAST Cloud Consoleの登録

まず[Notification > Push > API v2.0 ガイド](/Notification/Push/ja/api-guide/)をご参考の上、Consoleを設定してください。

#### Download

* Firebase Pushを使用する場合
    * ダウンロードしたSDKの**gamebase-adapter-push-fcm**フォルダをプロジェクトに追加します。
* Tencent Pushを使用する場合
    * ダウンロードしたSDKの**gamebase-adapter-push-tencent**フォルダをプロジェクトに追加します。

> <font color="red">[重要]</font><br/>
>
> Pushモジュールは、一つでなければなりません。<br/>
> Firebase PushとTencent Pushをプロジェクトに同時に追加しないでください。


#### AndroidManifest.xml

* Gamebase Pushに必要な設定を追加します。

> <font color="red">[重要]</font><br/>
>
> **${applicationId}**を**パッケージネーム**に変更する必要があります。
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

#### Google Services Settings (Firebase only)

* Gradleビルドを使用する場合
    * Firebase Pushを使用するためには、google-services.jsonの設定ファイルが必要です。設定ファイルをプロジェクトに含める方法は、[Firebase cloud messaging](https://firebase.google.com/docs/cloud-messaging/#add_firebase_to_your_app)の説明をご参考ください。
    * gradleの設定に**apply plugin:'com.google.gms.google-services'**を追加します。
    * 上記の設定によりGoogle Services Gradle Pluginが適用され、google-services.jsonファイルをres/google-services/{build_type}/values/values.xmlという名前のstring resourceに変更して使用することになります。
* Unityビルドの場合
    * 직접 string resource(xml) 파일을 만들어서 Assets/Plugins/Android/res/values/ 폴더에 포함시켜야 합니다. 
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 次は、string resourceファイルの例です。
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
        * string resource(xml) 파일에서 설정하는 각각의 값은 Firebase Console > 프로젝트 설정 > google-services.json 파일을 다운로드해서 확인 가능합니다. 
            Firebase 서비스 연동에 따라서 google-services.json 파일의 내용은 달라질 수 있습니다.
            ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)

#### Initialization

* Gamebaseを初期化する際にconfigurationの**setPushType()**を呼び出します。
* Firebase Pushを使用する場合
    * 追加で**setFCMSenderId()**を呼び出します。
* Tencent Pushを使用する場合
    * 追加で**setTencentAccessId()**を呼び出します。
    * 追加で**setTencentAccessKey()**を呼び出します。

```java
private static final String PUSH_FCM_SENDER_ID = "...";
private static final String PUSH_TENCENT_ACCESS_ID = "...";
private static final String PUSH_TENCENT_ACCESS_KEY = "...";

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setFCMSenderId(PUSH_FCM_SENDER_ID)				// irebaseは、SenderIdが必要です。
        .setTencentAccessId(PUSH_TENCENT_ACCESS_ID)		// Tencent AccessIdが必要です。
        .setTencentAccessKey(PUSH_TENCENT_ACCESS_KEY)	// Tencent AccessKeyが必要です。
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Register Push

次のAPIを呼び出してTOAST Pushに該当するユーザーを登録します。<br/>
Pushの同意状態(enablePush)、Push型広告の同意状態(enableAdPush)、夜間のPush型広告の同意状態(enableAdNightPush)の値をユーザーから取得し、次のAPIを呼び出して登録を完了させます。

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, PushConfiguration configuration, GamebaseCallback callback);
```

**Example**

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

ユーザーのPush設定を照会するために、次のAPIを利用します。<br/>
コールバックによるPushConfigurationの値からユーザー設定値を取得することができます。

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次ドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、TOAST Pushライブラリーで発生したエラーです。
* TOAST Push エラーの詳細は次のように確認できます。

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

* TOAST Pushのエラーコードは、次のドキュメントをご参考ください。
    * [Notification > Push > エラーコード](/Notification/Push/ja/sdk-guide/#_10)




    
