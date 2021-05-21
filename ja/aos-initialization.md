## Game > Gamebase > Android SDK ご利用ガイド > 初期化

Gamebase Android SDKを使用するためには、まず初期化を行う必要があります。

### onActivityResult

Gamebaseが正常に動作するよう、必ず**Activity#onActivityResult(int, int, Intent)**から**Gamebase.onActivityResult(int, int, Intent)**を呼び出します。

**API**

```java
+ (void)Gamebase.onActivityResult(int requestCode, int resultCode, Intent data);
```

### Initialization Flow

ゲームが始まったらDebug Modeを設定し、Gamebaseを初期化して、Launching Status Codeに従ってゲームに進入可否を決定するように、以下のフローのように実装してください。

![initialization flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

Gamebaseを初期化するとき、GamebaseConfiguration.Builderの客体でGamebaseの設定を変更することができます。

| API                                      | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------------- | -------------------------- | ---------------------------------------- |
| newBuilder(String appId, String appVersion, String storeCode) | **M**                      | GamebaseConfiguration.BuilderオブジェクトはnewBuilder()関数で作成できます。<br/><br/> **appId**はNHN Cloud Projectで発行したアプリIDを入力します。<br/> **appVersion**はアップデート、メンテナンスに該当するかどうかはゲームバージョンで判断します。ゲームバージョンを指定してください。 <br/> **storeCode**はAPKが配布されるストアを意味するコードです。次のガイドで各ストアのコードを確認できます。 [Purchase - Initialization](./aos-purchase/#6-initialization) |
| build()                                  | **M**                      | 設定を終えたBuilderをConfigurationの客体に変換します。<br/>**Gamebase.initialize()**APIで必要です。|
| enablePopup(boolean enable)              | O                          | **[UI]**<br/>システムメンテナンス、利用制限(ban)などゲームユーザーがゲームをプレイすることができない状況の場合、ポップアップなどで理由を表示しなければならないときがあります。<br/>**true**に設定すれば、Gamebaseが該当する状況のとき、案内ポップアップを自動で表示します。<br/>デフォルトは**false**です。<br/>**false**状態では起動結果を通して情報を取得した後に直接UIを設計し、ゲームをプレイすることができない理由を表示してください。|
| enableLaunchingStatusPopup(boolean enable) | O                          | **[UI]**<br/>起動結果によりログインできない状態の場合(主にメンテナンス状態)、Gamebaseが自動でポップアップを表示するかどうかを変更することができます。<br/>**enablePopup(true)**の状態でのみ動作します。<br/>デフォルトは**true**です。|
| enableBanPopup(boolean enable)           | O                          | **[UI]**<br/>ゲームユーザーが利用を制限された状態の場合、Gamebaseが自動でbanされた理由をポップアップで表示するかどうかを変更することができます。<br/>**enablePopup(true)**の状態でのみ動作します。<br/>デフォルトは**true**です。|

### Debug Mode
* Gamebaseは、警告(warning)とエラーログのみ表示します。
* 開発の参考になるシステムログをオンにしたいときは、**Gamebase.setDebugMode(true)**を呼び出してください。

> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**するときは、必ずソースコードからsetDebugModeの呼び出しを除去したりパラメーターをfalseに変えてからビルドしてください。

デバッグ設定は、コンソールでも行うことができ、コンソールで設定された値を優先視します。
コンソールの設定方法は、下記のガイドを参照してください。

* [コンソールテスト端末設定](./oper-app/#test-device)
* [コンソールクライアント設定](./oper-app/#client)


### Initialize

**Activity#onCreate(Bundle)**から**Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**を呼び出してGamebase SDKを初期化します。

**API**

```java
+ (void)Gamebase.initialize(Activity activity, GamebaseConfiguration configuration, GamebaseDataCallback<LaunchingInfo> callback);
```

**Example**

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Show gamebase debug message.
         *
         * CAUTION!
         * Set 'false' when build RELEASE.
         */
        Gamebase.setDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        String storeCode = "GG";
        GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(appId, appVersio, storeCode)
                .enablePopup(true)
                .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // ゲームログインを許可するかどうかは、ローンチコードに応じて判断してください。
                    ...
                } else {
                    // 初期化に失敗すると、Gamebase SDKを利用することができません。
                    // エラーを表示して、ゲームを再起動または終了する必要があります。
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

Gamebase#initializeの呼び出し結果で起動状態を確認することができます。<br/>
ローンチコードに応じてゲームプレイ可否を判断してください。

```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 起動状態を確認します。
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
                // ゲームプレイを開始します。
            } else {
                // ゲーム不可の理由を表示してゲームを中止します。
            }
        }
        ...
    }
});
```

getLaunchingInformations APIを利用すると、初期化後にもLaunchingInfoオブジェクトを取得できます。

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
LaunchingInfoオブジェクトにはGamebase Consoleに設定した値、ゲーム状態などが含まれています。


#### 1. Launching

Gamebaseローンチ情報です。

**1.1 Status**

Gamebase Android SDKの初期化設定に入力したアプリバージョンのゲーム状態情報です。

* code：ゲームステータスコード(メンテナンス中、アップデート必須、サービス終了など)
* message：ゲーム状態メッセージ

ステータスコードは下記の表を参照してください。

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | サービスが正常に動作しています。                               |
| RECOMMEND_UPDATE            | 201  | アップデートを推奨します。                                |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | メンテナンス中にはサービスを利用することができませんが、QA端末として登録されている場合はメンテナンスに関係なくサービスに接続してテストすることができます。|
| IN_TEST                     | 203  | テスト中 |
| IN_REVIEW                   | 204  | 審査中 |
| IN_BETA                     | 205  | ベータサーバー環境 |
| REQUIRE_UPDATE              | 300  | アップデートが必ず必要です。                                |
| BLOCKED_USER                | 301  | 接続遮断に登録された端末(デバイスキー)でサービスに接続した場合です。 |
| TERMINATED_SERVICE          | 302  | サービスが終了しました。                                 |
| INSPECTING_SERVICE          | 303  | サービスメンテナンス中です。                               |
| INSPECTING_ALL_SERVICES     | 304  | 全体サービスメンテナンス中です。                            |
| INTERNAL_SERVER_ERROR       | 500  | 内部サーバーエラーです。                               |

[コンソールガイド](/Game/Gamebase/ja/oper-app/#app)

**1.2 App**

Gamebase Consoleに登録されたアプリ情報です。

* accessInfo
    * serverAddress：サーバーアドレス
* customerService
    * accessInfo : サポート情報
    * type：サポートタイプ
    * url : サポート URL
* relatedUrls
    * termsUrl：利用規約
    * personalInfoCollectionUrl：個人情報同意
    * punishRuleUrl：利用停止規定
* install：インストールURL
* idP：認証情報

[Game > Gamebase > コンソール使用ガイド > アプリ > Client](./oper-app/#client)

**1.3 Maintenance**

Gamebase Consoleに登録されたメンテナンス情報です。

* url：メンテナンスページURL
* timezone：標準時間帯(timezone)
* beginDate：開始時間
* endDate：終了時間
* message：メンテナンス理由

[Game > Gamebase > コンソール使用ガイド > 運営 > Maintenance](./oper-operation/#maintenance)

**1.4 Notice**

Gamebase Consoleに登録された告知情報です。

* message：メッセージ
* title：タイトル
* url：メンテナンスURL

[Game > Gamebase > コンソール使用ガイド > 運営 > Notice](./oper-operation/#notice)

#### 2. tcProduct

Gamebaseと連携したNHN CloudサービスのappKeyです。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud Consoleに登録されたIAPストア情報です。

* id: App ID
* name: App Name
* storeCode: Store Code

[Game > Gamebase > コンソール使用ガイド > 決済](./oper-purchase/)

#### 4. tcLaunching

TNHN Cloud Launching Consoleでユーザーが入力した情報です。

* ユーザーが入力した値をJSON stringで伝達します。
* NHN Cloud Launchingの詳細設定は下記のガイドを参照してください。
 
[Game > Gamebase > コンソール使用ガイド > 管理 > Config](./oper-management/#config)

### Handling Unregistered Version

Gamebaseコンソールに登録されていないGameClientVersionを初期化すると**LAUNCHING_UNREGISTERED_CLIENT(2004)**エラーが発生します。
enablePopup(true), enableLaunchingStatusPopup(true)状態の場合、強制アップデートポップアップが表示され、マーケットに移動します。
Gamebaseポップアップを使用しない場合はUpdateInfoをGamebaseExceptionオブジェクトから取得してユーザーがマーケットに移動できるようにゲームで直接UIを実装できます。

**VO**

```java
class UpdateInfo {
    // 最新バージョンをダウンロードできるストアインストールURL
    String installUrl;
    // ユーザーに表示されるメッセージで、ユーザーの端末言語に合わせて伝達されます。
    // 言語が「en」の場合、メッセージは下記のとおりです。
    // 'The version is not supported. Please get the latest update version.'
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
| NOT_INITIALIZED              | 1          | Gamebaseが初期化されていません。 |
| NOT_LOGGED_IN                | 2          | ログインが必要です。            |
| INVALID_PARAMETER            | 3          | 無効なパラメータです。           |
| INVALID_JSON_FORMAT          | 4          | JSONフォーマットエラーです。          |
| USER_PERMISSION              | 5          | 権限がありません。               |
| NOT_SUPPORTED                | 10         | サポートしていない機能です。        |
| NOT_SUPPORTED_ANDROID        | 11         | Androidでサポートしていない機能です。   |
| ANDROID_ACTIVEAPP_NOT_CALLED | 32         | activeApp APIが呼び出されませんでした。   |


* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
