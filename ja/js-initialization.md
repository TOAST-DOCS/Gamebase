## Game > Gamebase > JavaScript SDK使用ガイド > 初期化

Gamebase JavaScript SDKを使用するには、先に初期化をする必要があります。

### Configuration Settings

Gamebaseを初期化する時、GamebaseConfigurationオブジェクトでGamebase設定を変更できます。

| KEY                                                 | Mandatory(M) / Optional(O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId: string                                       | **M**                      | TOAST Project IDです。入力は必須です。|
| clientVersion: string                               | **M**                      | サービス中、メンテナンス、告知事項など、プレイできる状態かどうかをゲームバージョンを通して判断します。<br/> `TOAST Console > Gamebase > App > Client Version > WEB`のバージョンを入力してください。|
| enableDebugMode: boolean                            | O                          | Debug Modeを有効にできます。デバッグログは開発者コンソールに出力されます。<br/> デフォルト値は**false**です。|
| uiConfiguration.enablePopup: boolean                | O                          | **[UI]**<br/>システムメンテナンス、利用制裁(ban)など、ゲームユーザーがゲームをプレイできない状況の時、ポップアップなどで理由を表示する必要がある場合があります。<br/> **true**に設定すると、Gamebaseが該当状況で情報ポップアップを自動的に表示します。<br/> デフォルト値は **false**です。<br/>**false**状態ではローンチ結果で情報を取得した後、独自にUIを実装してゲームをプレイできない理由を表示してください。|
| uiConfiguration.enableLaunchingStatusPopup: boolean | O                          | **[UI]**<br/>ローンチ結果に応じてログインできない状態で(主にメンテナンス状態)、Gamebaseが自動的にポップアップを表示するかどうかを変更できます。<br/>**enablePopup(true)**状態でのみ動作します。<br/>デフォルト値は**true**です。|
| uiConfiguration.enableBanPopup: boolean             | O                          | **[UI]**<br/>ゲームユーザーが利用制裁を受けた状態の時、Gamebaseが自動的に利用停止理由をポップアップ表示するかどうかを変更できます。<br/>**enablePopup(true)**状態でのみ動作します。<br/>デフォルト値は**true**です。|
| displayLanguageCode: string                         | O                          | Gamebase UIで使用する言語コードです。<br/> デフォルト値は**ブラウザのlanguage**(navigator.language)です。|
| displayLanguageTable: json                          | O                          | Gamebase UIを表示する時に使用するテキストリソース(text resource)です。<br/>該当値は、内蔵されている言語セットテーブルと自動的に合併されて使用されます。<br/> `toast.Gamebase.getDisplayLanguageTable()`を通して、内蔵されている言語セットテーブルおよびフォーマットを確認できます。|


#### GamebaseConfiguration Example
```javascript
var gamebaseConfiguration = {
    appId: 'T0asTC1oud',    // TOAST Console Project ID
    clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
    enableDebugMode: true,  // Note: Do not turn it on in production mode.
    uiConfiguration: {
        enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
        enableLaunchingStatusPopup: true,
        enableBanPopup: true,
    },
};
```


### Debug Mode
Gamebaseのデバッグを行うための設定です。
* true：Gamebaseのすべてのログが出力されます。
* false：警告(warning)、エラー(error)ログが出力されます。
* デフォルト値：false

デバッグ設定は、コンソールでも行うことができ、コンソールで設定された値を優先視します。
コンソールの設定方法は、下記のガイドを参照してください。

* [コンソールテスト端末設定](./oper-app/#test-device)
* [コンソールクライアント設定](./oper-app/#client)


開発の参考になるシステムログを有効にするには、**toast.Gamebase.setDebugMode(true)**を呼び出してください。
> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**する時は、必ずソースコードでsetDebugMode呼び出しを削除するか、パラメータをfalseに変更する必要があります。




### Initialize

**toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ... })**を呼び出し、Gamebase SDKを初期化します。<br/>

```js
function gamebaseInitialize() {
    var gamebaseConfiguration = {
        appId: 'T0asTC1oud',    // TOAST Console Project ID
        clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
        enableDebugMode: true,  // Note: Do not turn it on in production mode.
        uiConfiguration: {
            enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
            enableLaunchingStatusPopup: true,
            enableBanPopup: true,
        },
    };  


    toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
        if (error) {
            // 初期化に失敗すると、Gamebase SDKを利用できません。
            // appId、clientVersionおよびTOASTコンソールの設定が正常に入力されているかどうか、確認してください。
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status値は、下のLaunching Status Code表を参照してください。
            // ゲームプレイ可能状態です。
            console.log('Playable!');
        } else {
            // ゲームプレイできない状態です(メンテナンス、サービス終了など)。
            console.log('Not Playable!');
        }
    });

    var isPlayable = function(statusCode) {
        if (statusCode >= 200 && statusCode < 300)
            return true;

        return false;
    };
}
```

### Launching Status

toast.Gamebase.initializeの呼び出し結果です。ローンチ状態を確認できます。
```js
toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
    if (error) {
        console.log(error);
        return;
    }

	var statusCode = launchingInfo.launching.status.code;
});
```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 正常サービス中                           |
| RECOMMEND_UPDATE            | 201  | アップデート推奨                            |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | メンテナンス中はサービスを利用できませんが、QA端末として登録された場合は、メンテナンスに関係なくサービスに接続してテストできます。 |
| IN_TEST                     | 203  | テスト中 |
| IN_REVIEW                   | 204  | 審査中 |
| REQUIRE_UPDATE              | 300  | アップデート必須                            |
| BLOCKED_USER                | 301  | 接続遮断に登録された端末(デバイスキー)でサービスに接続した場合です。 |
| TERMINATED_SERVICE          | 302  | サービス終了                             |
| INSPECTING_SERVICE          | 303  | サービスメンテナンス中                           |
| INSPECTING_ALL_SERVICES     | 304  | 全体サービスメンテナンス中                        |
| INTERNAL_SERVER_ERROR       | 500  | 内部サーバーエラー                           |
