## Game > Gamebase > Unity SDK ご利用ガイド > 初期化

Gamebase Unity SDKを使用するためには、まず初期化を行う必要があります。また、アプリID、アプリバージョン情報がTOAST Consoleに必ず登録されていなければなりません。

### Inspector Settings

初期化する際に必要な設定は、次の通りです。

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| isDebugMode | ALL | O |
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| storeCode | ALL | O |
| fcmSenderId | Android | O |
| useWebview | Standalone | O |

#### 1. App ID

Gamebase Consoleに登録されたプロジェクトIDです。

[Console Guide](/Game/Gamebase/ja/oper-app/#app)

#### 2. appVersion

Gamebase Consoleに登録したクライアントバージョンです。

[Console Guide](/Game/Gamebase/ja/oper-app/#client)

#### 3. isDebugMode

Gamebaseデバッグのための設定です。

* true:Gamebaseのすべてのログが出力されます。
* false:Warning、Errorログが出力されます。
* デフォルト:false

Gamebaseに関するお問い合わせがある場合、該当する設定をtrueに変更してからログを[カスタマーセンター](https://toast.com/support/inquiry)まで送っていただけましたら、迅速に対応いたします。

> <font color="red">[注意]</font><br/>
>
> ゲームを**RELEASE**する場合は、該当する設定を必ず**false**に変更する必要があります。

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

#### 8. storeCode

TOASTの統合アプリ内決済サービスであるIAP(In-App Purchase)を初期化するために必要なストア情報です。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | TS | only Android |

#### 9. fcmSenderId

Firebase Messaging(FCM)を使用するためのSender IDです。

![FCM Sender ID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_004_1.2.0.png)

#### 10. useWebview

Standalone 플랫폼에서 WebView를 통해서 로그인을 할 것인지에 대한 설정입니다. 

#### 11. GamebaseUnitySDKSettings

上で説明した設定は、GamebaseUnitySDKSettingsコンポーネントのInspectorで変更することができます。

![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-initialization_003_1.12.0.png)

### Initialize with Inspector Settings

Gamebase Unity SDKを初期化する方法は、次の通りです。

1. 新しいゲームオブジェクトを作成します。
2. GamebaseUnitySDKSettings.csファイルを作成したゲームオブジェクトのコンポーネントに追加します。
3. Inspectorから初期化設定を入力します。
4. Gamebase.Initialize(callback)APIを呼び出します。

> <font color="red">[注意]</font><br/>
>
> 作成したゲームオブジェクトを削除すると、Android、iOSのAPIを呼び出した後、コールバックを受け取ることができなくなりますので、ご注意ください。<br/>
> 誤って削除した場合、"Do not destroy this gameObject in order to receive callback."エラーが表示されます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public class SampleInitialization
{
    public void Initialize()
    {
        Gamebase.Initialize((launchingInfo, error) =>
        {
            if (Gamebase.IsSuccess(error))
            {
                Debug.Log("Gamebase initialization is succeeded.");

                if (IsPlayable(launchingInfo.launching.status.code))
                {
                    Debug.Log("Playable");
                }
                else
                {
                    if (launchingInfo.launching.status.code == GamebaseLaunchingStatus.REQUIRE_UPDATE)
                    {
                        Debug.Log(string.Format("message:{0}", launchingInfo.launching.status.message));
                    }
                }
            }
            else
            {
                Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
            }
        });
    }

    private bool IsPlayable(int status)
    {
        if (status >= 200 && status < 300)
            return true;

        return false;
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

Gamebaseと連携しているTOASTサービスのappKeyです。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

TOAST Consoleに登録されたIAPストアの情報です。

* id:App ID
* name:App Name
* storeCode:Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

### Get Launching Information

GetLaunchingInformations API를 이용하면 Initialize 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

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

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebase 초기화돼 있지 않습니다. |
| NOT\_LOGGED\_IN       | 2          | 로그인이 필요합니다.            |
| INVALID\_PARAMETER    | 3          | 잘못된 파라미터입니다.           |
| INVALID\_JSON\_FORMAT | 4          | JSON 포맷 오류입니다.         |
| USER\_PERMISSION      | 5          | 권한이 없습니다.              |
| NOT\_SUPPORTED        | 10         | 지원하지 않는 기능입니다.         |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
