## Game > Gamebase > Unity SDK ご利用ガイド > 初期化

Gamebase Unity SDKを使用するためには、まず初期化を行う必要があります。また、アプリID、アプリバージョン情報がNHN Cloud Consoleに必ず登録されていなければなりません。

### GamebaseConfiguration 

初期化する際に必要な設定は、次の通りです。

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| storeCode | ALL | M |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| enableKickoutPopup | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

Gamebase Consoleに登録されたプロジェクトIDです。

[Console Guide](/Game/Gamebase/ja/oper-app/#app)

#### 2. appVersion

Gamebase Consoleに登録したクライアントバージョンです。

[Console Guide](/Game/Gamebase/ja/oper-app/#client)

#### 3. storeCode

NHN Cloudの統合アプリ内決済サービスであるIAP(In-App Purchase)を初期化するために必要なストア情報です。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| ONE Store | ONESTORE | only Android |
| GALAXY Store | GALAXY | only Android |
| Windows | WIN | only Unity Standalone |
| Web | WEB | only Unity WebGL and JavaScript |

#### 4. displayLanguageCode

Gamebaseで提供するUI及びSystemDialogに表示される言語をデバイスに設定されている言語ではなく、ユーザーが設定した言語に変更することができます。

[Display Language](./unity-etc/#display-language)

#### 5. enablePopup

システムメンテナンス、利用制限(ban)などゲームユーザーがゲームをプレイすることができない状況の場合、ポップアップなどで理由を表示しなければならないときがあります。
Gamebase SDKで提供する基本ポップアップを使用するかどうかに対する設定です。

* true:enableLaunchingStatusPopup、enableBanPopupの設定によりポップアップをの表示するかどうかが決定されます。
* false:Gamebaseで提供するすべてのポップアップが表示されません。
* デフォルト:false

#### 6. enableLaunchingStatusPopup

LaunchingStatusがゲームをプレイすることができない状態の場合、Gamebaseで提供する基本ポップアップを使用するかどうかに対する設定です。
LaunchingStatusは、次のLaunchingチャプターの下のState、Code部分をご参考ください。

* デフォルト:true

#### 7. enableBanPopup

ログインする際に該当するゲームユーザーが利用停止状態の場合、Gamebaseで提供する基本ポップアップを使用するかどうかに対する設定です。

* デフォルト:true

#### 8. enableKickoutPopup

GamebaseサーバーからKickoutイベントを取得した場合、Gamebaseで提供する基本ポップアップを使用するかどうかの設定です。

* デフォルト値：true


#### 9. fcmSenderId

Firebase Messaging(FCM)を使用するためのSender IDです。

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

スタンドアローン(standalone)プラットフォームで、WebViewでログインするかどうかに対する設定です。

### Debug Mode
* Gamebaseは警告(warning)とエラーログだけを表示します。
* 開発の参考にできるシステムログをオンにするには、**Gamebase.SetDebugMode(true)**を呼び出してください。

> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**する時は、必ずソースコードからSetDebugModeの呼び出しを削除するか、パラメータをfalseに変更してビルドしてください。

デバッグ設定はConsoleでも行えます。Consoleで設定された値を優先視します。
Console設定方法は、以下のガイドを参考にしてください。

* [Consoleテスト端末設定](./oper-app/#test-device)
* [Console Client設定](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void SetDebugMode(bool isDebugMode)
```

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
}
```

### Initialize

SDKを初期化します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        /**
         * Show gamebase debug message.
         * set 'false' when build RELEASE.
         */
        Gamebase.SetDebugMode(true);

        /**
         * Gamebase Configuration.
         */
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        /**
         * Gamebase Initialize.
         */
        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("Initialization succeeded.");

                //Following notices are registered in the Gamebase Console
                var notice = launchingInfo.launching.notice;
                if (notice != null)
                {
                    if (string.IsNullOrEmpty(notice.message) == false)
                    {
                        Debug.Log(string.Format("title:{0}", notice.title));
                        Debug.Log(string.Format("message:{0}", notice.message));
                        Debug.Log(string.Format("url:{0}", notice.url));
                    }
                }
        
                //Status information of game app version set in the Gamebase Unity SDK initialization.
                var status = launchingInfo.launching.status;
        
                // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
                // refer to GamebaseLaunchingStatus
                if (status.code == GamebaseLaunchingStatus.IN_SERVICE)
                {
                    // Service is now normally provided.
                }
                else
                {
                    switch (status.code)
                    {
                        case GamebaseLaunchingStatus.RECOMMEND_UPDATE:
                        {
                            // Update is recommended.
                            break;
                        }
                        // ... 
                        case GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR:
                        {
                            // Error in internal server.
                            break;
                        }
                    }
                }
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Update is require.
                    }
                }
            }
        });
    }
}
```

### Launching Information

InitializeAPIを使用してGamebase Unity SDKを初期化すると、LaunchingInfoの客体が結果値として送られます。
このLaunchingInfoの客体には、Gamebase Consoleに設定した値やゲーム状態などが含まれています。

#### 1. Launching

Gamebaseの起動情報です。

**1.1 Status**

Gamebase Unity SDKの初期化の設定に入力したアプリバージョンのゲームステータス情報です。

* code:ゲームステータスコード(サービスをメンテナンス中です、アップデートが必ず必要です、サービスが終了しましたなど)
* message:ゲームステータスメッセージ

ステータスコードは、次の表をご参考ください。

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | サービスが正常に動作しています。 |
| RECOMMEND_UPDATE | 201 | アップデートを推奨します。 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | メンテナンス中にはサービスを利用できませんが、QA端末に登録された場合にはメンテナンスに関係なくサービスに接続してテストすることができます。|
| IN_TEST                     | 203  | テスト中 |
| IN_REVIEW                   | 204  | 審査中 |
| REQUIRE_UPDATE | 300 | アップデートが必ず必要です。 |
| BLOCKED_USER                | 301         | 接続ブロックに登録された端末(デバイスキー)でサービスに接続したケースです。|
| TERMINATED_SERVICE          | 302         | サービスが終了しました。                                   |
| INSPECTING_SERVICE          | 303         | サービスをメンテナンス中です。                                 |
| INSPECTING_ALL_SERVICES     | 304         | 全体サービスをメンテナンス中です。                              |
| INTERNAL_SERVER_ERROR       | 500         | 内部サーバーエラーです。                                 |

[Console Guide](/Game/Gamebase/ja/oper-app/#app)


**1.2 App**

Gamebase Consoleに登録されたアプリ情報です。

* accessInfo
    * serverAddress:サーバーアドレス
    * csInfo:カスタマーセンターの情報
* relatedUrls
    * termsUrl:利用規約
    * personalInfoCollectionUrl:個人情報同意
    * punishRuleUrl:利用停止規定
    * csUrl:カスタマーセンター
* install:インストールURL
* idP:認証情報

[Console Guide](/Game/Gamebase/ja/oper-app/#client)

**1.3 Maintenance**

Gamebase Consoleに登録されたメンテナンス情報です。

* url:メンテナンスページのURL
* timezone:標準時(timezone)
* beginDate:開始時間
* endDate:終了時間
* message:メンテナンス理由

[Console Guide](/Game/Gamebase/ja/oper-operation/#maintenance)

**1.4 Notice**

Gamebaes Consoleに登録されたお知らせ情報です。

* message:メッセージ
* title:タイトル
* url:メンテナンスURL

[Console Guide](/Game/Gamebase/ja/oper-operation/#notice)

#### 2. tcProduct

Gamebaseと連携しているNHN CloudサービスのappKeyです。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

**NHN Cloud** Consoleに登録されたIAPストアの情報です。

* id:App ID
* name:App Name
* storeCode:Store Code
 
[Console Guide](/Game/Gamebase/ja/oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launching Consoleでユーザーが入力した情報です。

* ユーザーが入力した値をJSON stringで伝達します。
* NHN Cloud Launchingの詳細設定は、下記のガイドを参照してください。
 
[Console Guide](/Game/Gamebase/ja/oper-management/#config)

### Get Launching Information

GetLaunchingInformations APIを利用すると、Initialize後もLaunchingInfoオブジェクトを取得できます。

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

**Example**

```cs
public GamebaseResponse.Launching.LaunchingInfo GetLaunchingInformations()
{
    return Gamebase.Launching.GetLaunchingInformations();
}
```

### Handling Unregistered Version
 	 
Gamebaseコンソールに登録されていないGameClientVersionを初期化すると**LAUNCHING_UNREGISTERED_CLIENT(2004)**エラーが発生します。
enablePopup(true), enableLaunchingStatusPopup(true)状態の場合、強制アップデートポップアップが表示され、マーケットに移動します。
Gamebaseポップアップを使用しない場合はマーケットURLのようなアップデート情報をGamebaseErrorオブジェクトから取得できます。

**VO**

```cs
public class UpdateInfo {
	// 最新バージョンをダウンロードできるストアインストールURL
	string installUrl;
    // ユーザーに表示されるメッセージで、ユーザーの端末言語に合わせて伝達されます。
    // 言語が「ja」の場合、メッセージは下記のとおりです。
    // 'サポートしないバージョンです。最新バージョンへアップデートしてください。'
    string message;
}
```

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
GamebaseResponse.Launching.UpdateInfo GamebaseResponse.Launching.UpdateInfo.From(GamebaseError error);
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        var configuration = new GamebaseRequest.GamebaseConfiguration();
        configuration.appID = "appID";
        configuration.appVersion = "appVersion;"
        configuration.displayLanguageCode = GamebaseDisplayLanguageCode.English;
    #if UNITY_ANDROID
        configuration.storeCode = GamebaseStoreCode.GOOGLE;
    #elif UNITY_IOS
        configuration.storeCode = GamebaseStoreCode.APPSTORE;
    #elif UNITY_WEBGL
        configuration.storeCode = GamebaseStoreCode.WEBGL;
    #elif UNITY_STANDALONE
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #else
        configuration.storeCode = GamebaseStoreCode.WINDOWS;
    #endif

        Gamebase.Initialize(configuration, (launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                // Gamebase initialization succeeded.
            }
            else
            {
                // Check the error code and handle the error appropriately.
                Debug.Log(string.Format("Initialization failed. error is {0}", error));

                if (error.code == GamebaseErrorCode.LAUNCHING_UNREGISTERED_CLIENT)
                {
                    GamebaseResponse.Launching.UpdateInfo updateInfo = GamebaseResponse.Launching.UpdateInfo.From(error);
                    if (updateInfo != null)
                    {
                        // Unregistered game client version.
                        // Open market url to update application.
                        string installUrl = updateInfo.installUrl; // Market URL.
                        string message updateInfo.message; // Message from launching server.
                    }
                }
            }
        });
    }
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebaseが初期化されていません。 |
| NOT\_LOGGED\_IN       | 2          | ログインする必要があります。            |
| INVALID\_PARAMETER    | 3          | 無効なパラメータです。           |
| INVALID\_JSON\_FORMAT | 4          | JSONフォーマットエラーです。         |
| USER\_PERMISSION      | 5          | 権限がありません。              |
| NOT\_SUPPORTED        | 10         | サポートしていない機能です。         |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
