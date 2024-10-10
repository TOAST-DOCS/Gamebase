## Game > Gamebase > Unreal SDK使用ガイド > 初期化

Gamebase Unreal SDKを使用するには、初期化を行う必要があります。またアプリID、アプリバージョン情報がTOAST Consoleに登録されている必要があります。


### Include Header File

Gamebase APIを使用するには、次のヘッダファイルをインクルードします。

```cpp
#include "Gamebase.h"
```

### FGamebaseConfiguration 

初期化時に必要な設定は下記の通りです。

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| AppID | ALL | M |
| AppVersion | ALL | M |
| StoreCode | ALL | M |
| DisplayLanguageCode | ALL | O |
| bEnablePopup | ALL | O |
| bEnableLaunchingStatusPopup | ALL | O |
| bEnableBanPopup | ALL | O |

#### 1. App ID

Gamebase Consoleに登録されたプロジェクトIDです。

[Game > Gamebase > コンソール使用ガイド > アプリ > App](./oper-app/#app)


#### 2. appVersion

Gamebase Consoleに登録したクライアントバージョンです。

[Game > Gamebase > コンソール使用ガイド > アプリ > Client](./oper-app/#client)

#### 3. storeCode
NHN Cloud統合アプリ内決済サービスであるIAP(In-App Purchase)を初期化するために必要なストア情報です。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | ONESTORE | only Android |
| Galaxy Store | GALAXY | only Android |

#### 4. displayLanguageCode

Gamebaseで提供するUIおよびSystemDialogに表示される言語を、端末に設定された言語ではない別の言語に変更できます。

[Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Display Language](./unreal-etc/#display-language)

#### 5. enablePopup

システムメンテナンス、利用制裁(ban)など、ゲームユーザーがゲームをプレイできない状況で、ポップアップなどで理由を表示する必要がある時があります。
Gamebaseで提供する基本ポップアップを使用するかの設定です。

* true：enableLaunchingStatusPopup、enableBanPopup設定に応じてポップアップが表示されるかどうかが決定されます。
* false：Gamebaseで提供するすべてのポップアップが表示されません。
* デフォルト値：false

#### 6. enableLaunchingStatusPopup

LaunchingStatusがゲームをできない状態の場合、Gamebaseで提供する基本ポップアップを使用するかの設定です。
LaunchingStatusは、下記Launching項目下のState、Code部分を参照してください。

* デフォルト値：true

#### 7. enableBanPopup

ログイン時、該当ゲームユーザーが利用停止状態の場合、Gamebaseで提供する基本ポップアップを使用するかどうかの設定です。

* デフォルト値：true

### Debug Mode

* Gamebaseは警告(warning)とエラーログのみを表示します。
* 開発の参考にできるシステムログをオンにするには、**GamebaseSubsystem->SetDebugMode(true)**を呼び出してください。

> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**する時は、必ずソースコードからSetDebugModeの呼び出しを削除するか、パラメータをfalseに変更してビルドしてください。

デバッグ設定はConsoleでも行えます。Consoleで設定された値を優先視します。
Console設定方法は、以下のガイドを参考にしてください。

* [Consoleテスト端末設定](./oper-app/#test-device)
* [Console Client設定](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetDebugMode(bool isDebugMode);
```

**Example**

```cpp
void USample::SetDebugMode(bool isDebugMode)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->SetDebugMode(isDebugMode);
}
```

### Initialize

SDKを初期化します。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Initialize(const FGamebaseConfiguration& Configuration, const FGamebaseLaunchingInfoDelegate& Callback);
```

**Example**

```cpp
void USample::Initialize(const FString& AppID, const FString& AppVersion)
{
    FGamebaseConfiguration Configuration;
    Configuration.AppID = AppID;
    Configuration.AppVersion = AppVersion;
    Configuration.StoreCode = GamebaseStoreCode.Google;
    Configuration.bEnablePopup = true;
    Configuration.bEnableLaunchingStatusPopup = true;
    Configuration.bEnableBanPopup = true;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Initialize(Configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([=](const FGamebaseLaunchingInfo* LaunchingInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
        
            // Following notices are registered in the Gamebase Console
            auto Notice = LaunchingInfo->Launching.Notice;
            if (Notice != null)
            {
                if (string.IsNullOrEmpty(Notice.message) == false)
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("title: %s"), Notice.title);
                    UE_LOG(GamebaseTestResults, Display, TEXT("message: %s"), Notice.message);
                    UE_LOG(GamebaseTestResults, Display, TEXT("url: %s"), Notice.url);
                }
            }
            
            // Status information of game app version set in the Gamebase Unreal SDK initialization.
            auto Status = LaunchingInfo->Launching.Status;
    
            // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
            // refer to GamebaseLaunchingStatus
            if (Status.Code == GamebaseLaunchingStatus::IN_SERVICE)
            {
                // Service is now normally provided.
            }
            else
            {
                switch (Status.Code)
                {
                    case GamebaseLaunchingStatus::RECOMMEND_UPDATE:
                    {
                        // Update is recommended.
                        break;
                    }
                    // ... 
                    case GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR:
                    {
                        // Error in internal server.
                        break;
                    }
                }
            }
        }
        else
        {
            // Check the Error code and handle the Error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

### Launching Information

Initialize APIを使用してGamebase Unreal SDKを初期化すると、LaunchingInfoオブジェクトが結果値として伝達されます。
このLaunchingInfoオブジェクトにはGamebase Consoleに設定した値とゲーム状態などが含まれています。

#### 1. Launching

Gamebaseローンチ情報です。

**1.1 Status**

Gamebase Unreal SDK初期化設定に入力したアプリバージョンのゲーム状態情報です。

* code：ゲームステータスコード(メンテナンス中、アップデート必須、サービス終了など)
* message：ゲーム状態メッセージ

ステータスコードは、下記の表を参照してください。

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 正常サービス中                               |
| RECOMMEND_UPDATE            | 201  | アップデート推奨                                |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | メンテナンス中はサービスを利用できませんが、QA端末に登録されている場合はメンテナンスに関係なくサービスに接続してテストできます。 |
| IN_TEST                     | 203         | テスト中 |
| IN_REVIEW                   | 204         | 審査中 |
| IN_BETA                     | 205         | ベータサーバー環境 |
| REQUIRE_UPDATE              | 300  | アップデート必須                                |
| BLOCKED_USER                | 301  | 接続遮断に登録された端末(デバイスキー)でサービスに接続した場合です。 |
| TERMINATED_SERVICE          | 302  | サービス終了                                 |
| INSPECTING_SERVICE          | 303  | サービスメンテナンス中                               |
| INSPECTING_ALL_SERVICES     | 304  | 全サービスメンテナンス中                            |
| INTERNAL_SERVER_ERROR       | 500  | 内部サーバーエラー                               |

[Game > Gamebase > コンソール使用ガイド > アプリ > App](./oper-app/#app)

**1.2 App**

Gamebase Consoleに登録されたアプリ情報です。

* accessInfo
    * serverAddress：サーバーアドレス
* customerService
    * accessInfo : サポート情報
    * type：サポートタイプ
    * url : ：サポートURL
* relatedUrls
    * termsUrl: 利用約款
    * personalInfoCollectionUrl: 個人情報同意
    * punishRuleUrl: 利用停止規定
* install: インストールURL
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

[Game > Gamebase > コンソール使用ガイド > 運営 > Notice](./oper-operation/#Notice)

#### 2. tcProduct

Gamebaseと連携されたTOASTサービスのappKeyです。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud Consoleに登録されたIAPストア情報です。

* id： App ID
* name： App Name
* storeCode： Store Code
 
[Game > Gamebase > コンソール使用ガイド > 決済](./oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launchingコンソールでユーザーが入力した情報です。

* ユーザーが入力した値をJSON stringで伝達します。
* NHN Cloud Launchingの詳細設定は、下記のガイドを参照してください。
 
[Game > Gamebase > コンソール使用ガイド > 管理 > Config](./oper-management/#config)

### Get Launching Information

GetLaunchingInformations APIを利用すると、Initialize後にもLaunchingInfoオブジェクトを取得できます。

> <font color="red">[注意]</font><br/>
>
> GetLaunchingInformations APIは、リアルタイムでサーバーから情報を取得する非同期APIではありません。
> 2分毎にアップデートされるキャッシュ情報を返すめ、リアルタイムで現在のメンテナンス状況を判断する用途には適していません。
> このような場合にはLaunching Status Codeが変更されれ時にイベントが動作するGamebaseEventHandlerを活用してください。
> [Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Observer](./unreal-etc/#observer)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
const FGamebaseLaunchingInfoPtr GetLaunchingInformations() const;
```

**Example**

```cpp
void USample::GetLaunchingInformations()
{
    auto launchingInformation = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLaunching().GetLaunchingInformations();
    if (launchingInformation.IsValid() == false)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found launching info."));
        return;
    }
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | Gamebaseが初期化されていません。 |
| NOT\_LOGGED\_IN       | 2          | ログインが必要です。            |
| INVALID\_PARAMETER    | 3          | 無効なパラメータです。           |
| INVALID\_JSON\_FORMAT | 4          | JSONフォーマットエラーです。         |
| USER\_PERMISSION      | 5          | 権限がありません。              |
| NOT\_SUPPORTED        | 10         | サポートしない機能です。         |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./Error-code/#client-sdk)
