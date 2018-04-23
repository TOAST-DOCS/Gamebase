## Game > Gamebase > Android SDK ご利用ガイド > 初期化

Gamebase Android SDKを使用するためには、まず初期化を行う必要があります。

### Activate the Application

アプリのライフサイクル(lifecycle)を管理するためには、アプリが有効になったことをGamebase SDKに知らせなければなりません。<br/>
**Application#onCreate()**から**Gamebase#activeApp(Context)**を呼び出します。

```java
public class GamebaseApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Gamebase.activeApp(getApplicationContext());
    }
}
```

### Configuration Settings

Gamebaseを初期化するとき、GamebaseConfiguration.Builderの客体でGamebaseの設定を変更することができます。

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| build()                                  | **M**                      | 設定を終えたBuilderをConfigurationの客体に変換します。<br/>**Gamebase.initialize()**APIで必要です。|
| setAppId(String appId)                   | **M**                      | TOAST Projectから発行したアプリIDを入力します。 |
| setAppVersion(String appVersion)         | **M**                      | アップデート、メンテナンスに該当するかどうかはゲームバージョンで判断します。<br/>ゲームバージョンを指定してください。|
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>システムメンテナンス、利用制限(ban)などゲームユーザーがゲームをプレイすることができない状況の場合、ポップアップなどで理由を表示しなければならないときがあります。<br/>**true**に設定すれば、Gamebaseが該当する状況のとき、案内ポップアップを自動で表示します。<br/>デフォルトは**false**です。<br/>**false**状態では起動結果を通して情報を取得した後に直接UIを設計し、ゲームをプレイすることができない理由を表示してください。|
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>起動結果によりログインできない状態の場合(主にメンテナンス状態)、Gamebaseが自動でポップアップを表示するかどうかを変更することができます。<br/>**enablePopup(true)**の状態でのみ動作します。<br/>デフォルトは**true**です。|
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>ゲームユーザーが利用を制限された状態の場合、Gamebaseが自動でbanされた理由をポップアップで表示するかどうかを変更することができます。<br/>**enablePopup(true)**の状態でのみ動作します。<br/>デフォルトは**true**です。|
| setStoreCode(String storeCode)           | O                          | **[Purchase]**<br/>TOASTの統合アプリ内決済サービス・IAP(In-App Purchase)を使用する場合、どのストアを使用するか設定しなければなりません。<br/>パラメーターは[IAPドキュメント](/Mobile%20Service/IAP/ja/Overview/)をご参考ください。|
| setFCMSenderId(String senderId)          | O                          | **[Push]**<br/>Google Notification(FCM, GCM)を通してPushメッセージを送信する場合、送信者のID(sender ID)を設定する必要があります。|
| setTencentAccessKey(String accessKey)<br/>setTencentAccessId(String accessId) | O                          | **[Push]**<br/>Tencent Pushモジュールを使用する場合、アクセスキー及びアクセスIDの値を設定する必要があります。|

### Debug Mode
* Gamebaseは、警告(warning)とエラーログのみ表示します。
* 開発の参考になるシステムログをオンにしたいときは、**Gamebase.setDebugMode(true)**を呼び出してください。

> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**するときは、必ずソースコードからsetDebugModeの呼び出しを除去したりパラメーターをfalseに変えてからビルドしてください。

### Initialize

**Activity#onCreate(Bundle)**から**Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**を呼び出してGamebase SDKを初期化します。<br/>
また、Gamebaseが正常に動作するよう、必ず**Activity#onActivityResult(int, int, Intent)**から**Gamebase.onActivityResult(int, int, Intent)**を呼び出します。

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Gamebase Configuration.
         */
        GamebaseConfiguration configuration =
                        new GamebaseConfiguration.Builder()
                                .setAppId("T0aStC1d")
                                .setAppVersion("1.0.0")
                                .enableLaunchingStatusPopup(true)
                                .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // 起動状態を確認します。
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // ゲームプレイ
                    } else {
                        // メンテナンスまたはアプリをアップデートする必要があります。
                    }
                } else {
                    // 初期化に失敗すると、Gamebase SDKを利用することができません。
                    // エラーを表示して、ゲームを再起動または終了する必要があります。
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

### Launching Status

Gamebase#initializeの呼び出し結果で起動状態を確認することができます。
```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 起動状態を確認します。
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // メンテナンス中…
                    break;
                ...
            }
            ...
        }
        ...
    }
});
```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | サービスが正常に動作しています。                               |
| RECOMMEND_UPDATE            | 201  | アップデートを推奨します。                                |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | メンテナンス中にはサービスを利用することができませんが、QA端末として登録されている場合はメンテナンスに関係なくサービスに接続してテストすることができます。|
| REQUIRE_UPDATE              | 300  | アップデートが必ず必要です。                                |
| BLOCKED_USER                | 301  | 接続ブロックに登録された端末(デバイスキー)でサービスに接続したケースです。|
| TERMINATED_SERVICE          | 302  | サービスが終了しました。                                 |
| INSPECTING_SERVICE          | 303  | サービスメンテナンス中です。                               |
| INSPECTING_ALL_SERVICES     | 304  | 全体サービスメンテナンス中です。                            |
| INTERNAL_SERVER_ERROR       | 500  | 内部サーバーエラーです。                               |

