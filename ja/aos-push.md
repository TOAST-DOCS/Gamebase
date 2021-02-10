## Game > Gamebase > Android SDK ご利用ガイド > Push

### Settings

ここではプラットフォームごとにPush通知を使用するために必要な設定方法についてご案内いたします。

#### Register NHN Cloud Cloud Console

[Notification > Push > Console Guide](/Notification/Push/ja/console-guide/)を参照してConsoleを設定します。

#### Gradle

* build.gradleに使用するモジュールを追加します。

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

* プッシュモジュールはFCM(Firebase Cloud Messaging)とTencentのどちらか1つを追加する必要がありますが、テスト目的で複数のモジュールを追加した場合はGamebase.initializeで使用するPushTypeを選択することもできます。
    * PushProvider.Type.FCM ： "FCM"
    * PushProvider.Type.TENCENT ： "TENCENT"

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

#### Firebase

* Gradleビルドを使用する場合
    * Firebaseプッシュを使用するには、下記のガイドに従ってFirebaseの設定を完了させる必要があります。
		* [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud Push > Android > Firebase Cloud Messaging設定](/toast-sdk/push-android/#firebase-cloud-messaging)
* Unityビルドの場合
    * 直接string resource(xml)ファイルを作成して、Assets/Plugins/Android/res/values/ フォルダに含める必要があります。
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 次は、string resourceファイルの例です。
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
        * string resource(xml)ファイルで設定する各値は、**Firebase Console > プロジェクト設定**をクリックし、**google-services.json**ファイルをダウンロードして確認できます。
            Firebaseサービス連携に応じて、google-services.jsonファイルの内容は変わる場合があります。
            ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)

#### Tencent

* Tencentプッシュを使用するには、下記のガイドに従ってTencentの設定を完了させる必要があります。
	* [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud Push > Android > Tencent Push Notification設定](/toast-sdk/push-android/#tencent-push-notification)

### Register Push

次のAPIを呼び出してNHN Cloud Pushに該当するユーザーを登録します。<br/>
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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次ドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud Pushライブラリーで発生したエラーです。
* NHN Cloud Push エラーの詳細は次のように確認できます。

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

* NHN Cloud Push SDKのエラーコードは次の通りです。

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API呼び出し成功 |
| NOT_INITIALIZE | 100 | NHN Cloud SDKまたはNHN Cloud Push SDKが初期化されていない場合。 |
| PROVIDER_SDK_ERROR | 101 | 外部SDK(Firebase, Tencent)でエラーが発生した場合。 |
| USER_ID_NOT_REGISTERED | 102 | ログインしていない場合。 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushTypeの入力に誤りがあるか、プッシュライブラリがプロジェクトに含まれていない場合。 |
| API_SERVER_ERROR | 104 | NHN Cloud PushサーバーAPIの呼び出しに失敗した場合。 |
| TOKEN_NOT_REGISTERED | 105 | 内部にキャッシュされたPush Tokenが存在しない場合。 |
