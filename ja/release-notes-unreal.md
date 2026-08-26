<!-- pre-align:aligned sig=1a11e8a39079 -->

<a id="game-gamebase-release-notes-unreal"></a>
## Game > Gamebase > リリースノート > Unreal { #game-gamebase-release-notes-unreal }

<a id="2-81-1-2026-06-23"></a>
### 2.81.1 (2026. 06. 23.) { #2-81-1-2026-06-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.1/GamebaseSDK-Unreal.zip)

<a id="811-2026-06-23-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="811-2026-06-23-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.81.0](./release-notes-android/#2-81-0-2026-06-23)
* [Gamebase iOS SDK 2.81.3](./release-notes-ios/#2-81-3-2026-05-27)

<a id="2-81-0-2026-03-24"></a>
### 2.81.0 (2026. 03. 24.) { #2-81-0-2026-03-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.0/GamebaseSDK-Unreal.zip)

<a id="810-2026-03-24-1"></a>
#### 기능 개선/변경

* 내부 로직을 개선했습니다.

<a id="810-2026-03-24-2"></a>
#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.80.0](./release-notes-android/#2-80-0-2026-02-13)
* [Gamebase iOS SDK 2.80.0](./release-notes-ios/#2-80-0-2026-02-13)

<a id="2-80-0-2026-02-13"></a>
### 2.80.0 (2026. 02. 13.) { #2-80-0-2026-02-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-Unreal.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### 機能改善・変更

* 決済リクエスト時、遅延決済や保護者の同意のように決済完了を待つ必要がある状況が発生した場合、新規追加された**PURCHASE_PENDING(4008)**エラーが発生します。
* Gamebase Event HandlerのGamebaseEventCategory.PURCHASE_UPDATEDイベント機能が拡張されました。
    * アプリの実行中、GamebaseEventHandlerを通じてPending決済(遅延決済、保護者の同意など)の完了イベントを受け取ることができます。
* (iOS) Project Settingsの設定に応じて、Info.plistに必要な項目が自動的に追加されるように改善されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > はじめに > iOS Settings](./unreal-started/#ios-settings)
* 内部ロジックを改善しました。

<a id="800-2026-02-13-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.80.0](./release-notes-android/#2-80-0-2026-02-13)
* [Gamebase iOS SDK 2.80.0](./release-notes-ios/#2-80-0-2026-02-13)

<a id="2-79-0-2026-01-27"></a>
### 2.79.0 (2026. 01. 27.) { #2-79-0-2026-01-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-Unreal.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="790-2026-01-27-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.79.0](./release-notes-android/#2-79-0-2026-01-27)
* [Gamebase iOS SDK 2.79.0](./release-notes-ios/#2-79-0-2026-01-27)

<a id="2-78-0-2026-01-13"></a>
### 2.78.0 (2026. 01. 13.) { #2-78-0-2026-01-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.78.0/GamebaseSDK-Unreal.zip)

<a id="780-2026-01-13-added-features"></a>
#### 機能追加

* Unreal Engineの対応バージョンが変更されました。
    * 4.27 ~ 5.7

<a id="780-2026-01-13-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="780-2026-01-13-bug-fixes"></a>
#### 不具合の修正
* (Windows) Payloadを含むRequestPurchase API呼び出し時、Payloadがコンソールに送信されない問題を修正しました。
* (Windows) NHNWebViewを利用したWebView表示関連機能を初めて表示する際、High DPI関連設定の有無により、高DPI環境でプログラムウィンドウが縮小する問題を修正しました。

<a id="780-2026-01-13-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.78.0](./release-notes-android/#2-78-0-2025-12-23)
* [Gamebase iOS SDK 2.77.0](./release-notes-ios/#2-77-0-2025-12-09)

<a id="2-76-0-2025-11-28"></a>
### 2.76.0 (2025. 11. 28.) { #2-76-0-2025-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.76.0/GamebaseSDK-Unreal.zip)

<a id="760-2025-11-28-added-features"></a>
#### 機能追加

* 最近投稿されたゲーム告知の投稿時間を提供するために、`FGamebaseLaunchingInfo::FApp::FGameNotice::LatestNoticeTimeMillis`フィールドを追加しました。
* (Android) 米国テキサス、ユタ、ルイジアナなど特定管轄権の年齢確認関連法律遵守を支援するために、Google Play Age Signalsベースの年齢確認APIが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 参考事項 > Age Signals Support](./unreal-etc/#age-signals-support)
* (Windows) Steam認証時にSteamworks SDKがロードされていない場合、外部ブラウザを通じたログインをサポートします。

<a id="760-2025-11-28-feature-updates"></a>
#### 機能改善・変更

* `IGamebasePurchase::RequestItemListAtIAPConsole()` APIが非推奨になりました。
    * `IGamebasePurchase::RequestItemListPurchasable()` APIを使用してください。
* 内部ロジックを改善しました。

<a id="760-2025-11-28-bug-fixes"></a>
#### 不具合の修正
* (Windows) Google決済時にブラウザログイン状態によって決済完了後、結果がゲームに伝達されない問題が修正されました。

<a id="760-2025-11-28-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.76.0](./release-notes-android/#2-76-0-2025-11-28)
* [Gamebase iOS SDK 2.75.0](./release-notes-ios/#2-75-0-2025-09-23)

<a id="2-75-0-2025-11-11"></a>
### 2.75.0 (2025. 11. 11.) { #2-75-0-2025-11-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-Unreal.zip)

<a id="750-2025-11-11-feature-updates"></a>
#### 機能改善・変更

* (Windows) 画像お知らせ、ゲームお知らせの表示時、固定サイズから画面解像度の比率で表示されるように修正されました。
* 内部ロジックを改善しました。

<a id="750-2025-11-11-bug-fixes"></a>
#### 不具合の修正

* (Windows) 画像お知らせ、ゲームお知らせをクリックした際、エンジンUIフォーカスの問題により、複数回クリックしないと反映されない問題を修正しました。
* (Windows) 決済完了時の指標送信にSetGameUserData API呼び出し情報が含まれるように修正されました。

<a id="750-2025-11-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.75.1](./release-notes-android/#2-75-1-2025-10-17)
* [Gamebase iOS SDK 2.75.0](./release-notes-ios/#2-75-0-2025-09-23)

<a id="2-74-0-2025-08-26"></a>
### 2.74.0 (2025. 08. 26.) { #2-74-0-2025-08-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.74.0/GamebaseSDK-Unreal.zip)

<a id="740-2025-08-26-added-features"></a>
#### 機能追加

* (Windows) アカウントマッピング機能が追加されました。

<a id="740-2025-08-26-feature-updates"></a>
#### 機能改善・変更

* (Windows) ゲームお知らせ表示時、エンジンのDPIの影響を受けないように修正されました。
* 内部ロジックを改善しました。

<a id="740-2025-08-26-bug-fixes"></a>
#### 不具合の修正

* (Windows) アカウント状態が変更された際、稀にクラッシュが発生するロジックを修正しました。
* (Windows) Twitterログイン時、「Something went wrong」エラーが発生しないように修正されました。

<a id="740-2025-08-26-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.73.1](./release-notes-android/#2-73-1-2025-08-12)
* [Gamebase iOS SDK 2.73.1](./release-notes-ios/#2-73-1-2025-08-12)

<a id="2-73-1-2025-07-29"></a>
### 2.73.1 (2025. 07. 29.) { #2-73-1-2025-07-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-Unreal.zip)

<a id="731-2025-07-29-feature-updates"></a>
#### 機能改善・変更

* (Android) Amazon Appstoreのサービス終了に伴い、ストア設定およびプッシュ通知設定機能を削除しました。
* (Windows)ログ送信時の再試行ロジックを改善しました。
* 内部ロジックを改善しました。

<a id="731-2025-07-29-bug-fixes"></a>
#### 不具合の修正

* コンパイラ環境によってビルドエラーが発生するロジックを修正しました。
* (Windows)ログ送信時に、特定の文字が含まれるデータを送信する際に発生していたエラーを修正しました。

<a id="731-2025-07-29-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.73.0](./release-notes-android/#2-73-0-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2-73-0-2025-07-15)

<a id="2-73-0-2025-07-15"></a>
### 2.73.0 (2025. 07. 15.) { #2-73-0-2025-07-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Unreal.zip)

<a id="730-2025-07-15-feature-updates"></a>
#### 機能改善・変更

* (Windows) SDKを使用しないIdPの場合、外部ブラウザでログインを行うよう変更しました。
    * 外部ブラウザでのログイン進行中に、ログインをキャンセルできるAPIを追加しました。
        * CancelLoginWithExternalBrowser
        * APIの呼び出し方法については、以下のガイドドキュメントをご参照ください。
            * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Login > Login with IdP > Cancel Login with External Browser](./unreal-authentication/#cancel-login-with-external-browser)
* (Windows) Steamログイン時にSteamworksの初期化失敗に関するメッセージを追加し、原因を把握しやすくしました。
* 内部ロジックを改善しました。

<a id="730-2025-07-15-bug-fixes"></a>
#### 不具合の修正

* Epic Games関連の機能を使用しない場合は、EOSSDKモジュールが含まれないよう修正しました。
* (Windows)コンソールで設定されていないストアを使用する際に、クラッシュが発生しないよう修正しました。

<a id="730-2025-07-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.73.0](./release-notes-android/#2-73-0-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2-73-0-2025-07-15)

<a id="2-72-0-2025-06-24"></a>
### 2.72.0 (2025. 06. 24.) { #2-72-0-2025-06-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Unreal.zip)

<a id="720-2025-06-24-added-features"></a>
#### 機能追加

* EpicGames認証が追加されました。
* (Windows)利用停止ポップアップにサポートリンクを追加しました。

<a id="720-2025-06-24-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="720-2025-06-24-bug-fixes"></a>
#### 不具合修正

* (Windows)外部ブラウザでログインする際、レスポンスがゲームスレッドに渡されるように修正しました。
* (Windows)性能の低いPCで外部のブラウザログインが失敗する問題を修正しました。
* (Windows)デバイス情報を取得するプロセスが正常に実行されない場合にクラッシュが発生する問題を修正しました。

<a id="720-2025-06-24-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.72.0](./release-notes-android/#2-72-0-2025-06-24)
* [Gamebase iOS SDK 2.72.0](./release-notes-ios/#2-72-0-2025-06-24)

<a id="2-71-1-2025-04-29"></a>
### 2.71.1 (2025. 4. 29.) { #2-71-1-2025-04-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Unreal.zip)

<a id="711-2025-4-29-feature-updates"></a>
#### 機能改善・変更

* (Windows)決済APIエラー発生時のデバッグを支援するため、詳細なエラーメッセージを強化しました。
* 内部ロジックを改善しました。

<a id="711-2025-4-29-bug-fixes"></a>
#### 不具合修正

* (Windows) FGamebaseConfiguration内のDisplayLanguageCodeを適用する際、チェック言語の値を誤って取得する問題を修正しました。
* (Windows)認証過程の一部失敗ケースで再認証ができない問題を修正しました。

<a id="711-2025-4-29-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.71.1](./release-notes-android/#2-71-1-2025-04-29)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2-71-0-2025-04-15)

<a id="2-71-0-2025-04-15"></a>
### 2.71.0 (2025. 4. 15.) { #2-71-0-2025-04-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Unreal.zip)

<a id="710-2025-4-15-added-features"></a>
#### 機能追加

* 「ゲーム告知」新機能を追加しました。
    * API呼び出し方法は次のガイド文書を参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > UI > GameNotice](./unreal-ui/#gamenotice)
* (Windows) Google Play GamesをサポートするためのGoogle決済機能を追加しました。
    * [Windows設定ツール](./unreal-started/#windows-settings)のWindows Store設定に`Google Play Store`を追加しました。

<a id="710-2025-4-15-feature-updates"></a>
#### 機能改善・変更

* (Windows)システム設定で「地域 > 国または地域」に基づいてCountryCodeを作成するように修正しました。
    * 変更前にはエンジンが提供する`FInternationalization::Get().GetDefaultCulture()`で「地域 > 使用地域言語」情報を取得していました。

<a id="710-2025-4-15-bug-fixes"></a>
#### 不具合修正

* (Windows) WebViewを開いてプログラム終了時にクラッシュしないように修正しました。
* (Windows)エンジンに含まれるSteamworksモジュールがエディタで使用できないため、Steam認証と決済機能をエディタで使用できないように修正しました。
* (Windows)ログ送信フィルタリングが正常に動作しない問題を修正しました。
* 内部ロジックを改善しました。

<a id="710-2025-4-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.71.0](./release-notes-android/#2-71-0-2025-04-15)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2-71-0-2025-04-15)

<a id="2-70-0-2025-03-11"></a>
### 2.70.0 (2025. 3. 11.) { #2-70-0-2025-03-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Unreal.zip)

<a id="700-2025-3-11-added-features"></a>
#### 機能追加

* ログイン時にIdPサーバーでエラーが発生したことを示す新規エラーコードが追加されました。
    * AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* WebViewにナビゲーションバーtitleカラーとicon tintカラー設定オプションを追加しました。
    * `FGamebaseWebViewConfiguration::NavigationBarTitleColor`
    * `FGamebaseWebViewConfiguration::NavigationBarIconTintColor`
* (Android) 「GPGS自動ログイン」機能連動時、ユーザーにGPGSログインをアプリインストール後に一度だけ確認する初期化オプションを追加しました。
    * `FGamebaseConfiguration::bEnableGPGSSignInCheck`
    * 基本設定はtrueで、ユーザーがGPGSログインを拒否してもGamebase初期化時にGPGSログインウィンドウを再度表示します。
    * falseに設定すると、アプリ初回実行時にのみGPGSログインウィンドウが一度表示されます。

<a id="700-2025-3-11-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="700-2025-3-11-bug-fixes"></a>
#### 不具合修正

* (Windows)ログイン時にFGamebaseVariantMapで追加情報を受け取る場合、クラッシュが発生しないように修正しました。

<a id="700-2025-3-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.70.0](./release-notes-android/#2-70-0-2025-03-11)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2-70-0-2025-03-11)

<a id="2-69-1-2025-03-04"></a>
### 2.69.1 (2025. 3. 4.) { #2-69-1-2025-03-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.1/GamebaseSDK-Unreal.zip)

<a id="691-2025-3-4-added-features"></a>
#### 機能追加

* ローンチ情報で約款情報を確認できるように追加しました。
    * FGamebaseLaunchingInfo::FApp::FTermsService

<a id="691-2025-3-4-feature-updates"></a>
#### 機能改善・変更

* API呼び出し時にパラメータとして渡す`UGamebaseJsonObject`を`FGamebaseVariantMap(TMap<FName, FVariant>)`に変更しました。
* 内部ロジックを改善しました。

<a id="691-2025-3-4-bug-fixes"></a>
#### 不具合修正

* (Windows)ゲストログイン時、UUID発行過程のエラーにより全て同じ値が作成される問題を修正しました。
* (Windows) Line IDPログイン時にregion設定が動作しない問題を修正しました。
* (Windows)キックアウト時にServerPushAppKickOutイベントが発生し、ポップアップが表示されない問題を修正しました。
* (Windows)シンボル作成時エンジンのBuild ConfigurationがDevelopmentではない場合、エラーが発生する問題を修正しました。
* (Android)環境によってRegisterPushが動作しない問題を修正しました。

<a id="691-2025-3-4-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase Android SDK 2.69.0](./release-notes-android/#2-69-0-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2-69-0-2025-01-21)

<a id="2-69-0-2025-02-11"></a>
### 2.69.0 (2025. 2. 11.) { #2-69-0-2025-02-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unreal.zip)

<a id="690-2025-2-11-added-features"></a>
#### 機能追加

* **RequestLastLoggedInProvider非同期API**を追加しました。
    * **GetLastLoggedInProvider()同期API**がタイミング上、正常な値を返せない場合があります。
    * (Android) GPGSのAuto Login機能を使用する場合、GPGSサーバーからデータを取得する時間が必要なため、Gamebaseの初期化直後にGetLastLoggedInProvider()同期APIを呼び出すと正常な値を取得できません。
       この時、RequestLastLoggedInProvider非同期(GamebaseDataCallback&lt;String&gt;)非同期APIは正確な値を保証します。
        Auto Loginを使用しない場合は、GetLastLoggedInProvider()同期APIを使用しても構いません。
* (Android) GPGS v2認証を追加しました。
    * 詳細は以下のリンクを参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > はじめる > Android Settings](./unreal-started/#android-settings)
* (Android) **FGamebaseWebViewConfiguration::CutoutColorフィールド**を追加しました。
    * GamebaseWebViewの **FGamebaseWebViewConfiguration::bRenderOutSideSafeAreaフィールド**を**false**に設定した場合、cutout領域に自動的にpadding余白を追加します。
    * CutoutColorフィールドは、このように追加されたpadding領域の色を設定できます。
    * RenderOutsideSafeAreaフィールドをfalseに設定したが、CutoutColorフィールドは設定しない場合には、Webページ'body'の'background-color'値で自動的にpadding領域の色を決定します。

<a id="690-2025-2-11-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="690-2025-2-11-bug-fixes"></a>
#### 不具合修正

* 約款照会結果APIであるFGamebaseQueryTermsResultを修正しました。
    * TermsCountryTypeの値が設定されない問題を修正しました。
    * bPushEnabled, bAdAgreement, bAdAgreementNightを削除しました。
* (Android) Windows環境でビルド時、ポストビルドプロセスでエラーが発生しないように修正しました。

<a id="690-2025-2-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2-69-0-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2-69-0-2025-01-21)

<a id="2-68-1-2025-01-21"></a>
### 2.68.1 (2025. 01. 21.) { #2-68-1-2025-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unreal.zip)

<a id="681-2025-01-21-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。
* (Windows) WebViewプラグインをオプションで選択できるように変更しました。
    * 詳細は以下のリンクを参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > はじめる > Windows Settings > WebViewプラグイン案内](./unreal-started/#windows-settings)
* (Windows)クラッシュログを送信する際に、プロジェクトバイナリパスにシンボルファイルを圧縮したファイルが作成されるように追加されました。
    * 詳細は以下のリンクを参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > Logger > Crash Reporter](./unreal-logger/#crash-reporter)

<a id="681-2025-01-21-bug-fixes"></a>
#### 不具合修正
* (Windows)内部ログ送信時にクラッシュが発生する可能性があるロジックを修正しました。

<a id="681-2025-01-21-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2-68-0-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2-68-1-2024-12-10)

<a id="2-68-0-2024-12-10"></a>
### 2.68.0 (2024. 12. 10.) { #2-68-0-2024-12-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unreal.zip)

<a id="680-2024-12-10-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。
* (Windows) Twitter認証方式をOAuth 2.0に変更し、以下の設定を変更しないとログインが動作しません。
    * OAuth 2.0 Client ID及びClient Secret発行
        * Twitter Developer PortalでOAuth 2.0 Client IDとClient Secretを作成した後、 Gamebaseコンソールに登録します。
    * Callback URL設定
        * GamebaseコンソールにCallback URL(https://id-gamebase.toast.com/oauth/callback)を設定します。 
        * 同じCallback URLをTwitter Developer Portalに追加します。
    * 詳細は次のリンクを参照してください。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information](./oper-app/#authentication-information)

<a id="680-2024-12-10-bug-fixes"></a>
#### 不具合修正
* (Windows)決済プロセスでクラッシュが発生しないように修正しました。
* (Windows)Steam決済中にESCキーで決済を終了した場合、次の決済APIが動作しない問題を修正しました。

<a id="680-2024-12-10-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2-68-0-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2-68-1-2024-12-10)

<a id="2-67-2-2024-11-26"></a>
### 2.67.2 (2024. 11. 26.) { #2-67-2-2024-11-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.2/GamebaseSDK-Unreal.zip)

<a id="672-2024-11-26-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。

<a id="672-2024-11-26-bug-fixes"></a>
#### 不具合修正
* (Windows) Apple IDのログインが正常に行われない問題を修正しました。

<a id="672-2024-11-26-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2-67-0-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2-67-0-2024-10-29)

<a id="2-67-1-2024-11-14"></a>
### 2.67.1 (2024. 11. 14.) { #2-67-1-2024-11-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.1/GamebaseSDK-Unreal.zip)

<a id="671-2024-11-14-feature-updates"></a>
#### 機能改善・変更
* (Windows)Purchase設定時にストアを一つだけ選択できるように変更しました。
    * ストア再設定が必要です。
* (Windows) Epic Games Storeを使用する場合、EOS SDKのハンドルを登録するプロセスを変更しました。
    * Online Subsystem EOSを使用する場合、Gamebase初期化時にStoreCodeがEpic Games Storeの該当する値であれば自動的にハンドルを登録します。
    * Online Subsystem EOSを使用しない場合は、[Windows Settings](./unreal-started/#windows-settings)ガイドを参照してEOSのハンドルを登録する必要があります。
* (Windows) Steamworks SDKサポートバージョンが1.59に変更されました。
    * 詳細は以下のリンクを参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > はじめに > Windows Settings > Steamworksサービス](./unreal-started/#windows-settings)

<a id="671-2024-11-14-bug-fixes"></a>
#### 不具合修正
* ヘッダファイルを正常に参照できるように修正しました。
* (Windows)初初期化を複数回試行する際にクラッシュが発生しないように修正しました。
* (Windows)初期化時にStoreCodeがSteamまたはEpic Games Storeに該当するコードを入力するとクラッシュが発生しないように修正しました。
* (Windows)外部ブラウザでログインしようとするとクラッシュが発生する可能性があるロジックを修正しました。

<a id="671-2024-11-14-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2-67-0-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2-67-0-2024-10-29)

<a id="2-67-0-2024-10-30"></a>
### 2.67.0 (2024. 10. 30.) { #2-67-0-2024-10-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unreal.zip)

<a id="670-2024-10-30-added-features"></a>
#### 機能追加
* Steam認証を追加しました。
* Stema決済を追加しました。
* 画像告知機能に新規タイプを追加しました。
    * ローリングポップアップタイプを追加しました。
    * 既存の画像告知はポップアップタイプで表示され、Windowsではサポートされません。
* (Windows) LINE認証を追加しました。

<a id="670-2024-10-30-feature-updates"></a>
#### 機能改善・変更
* エンジンのサポートバージョンを4.27〜5.4に変更しました。
* 内部ロジックを改善しました。

<a id="670-2024-10-30-bug-fixes"></a>
#### 不具合修正
* クラッシュログ発生時にクラッシュが発生する可能性があるロジックを修正しました。

<a id="670-2024-10-30-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2-67-0-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2-67-0-2024-10-29)

<a id="2-66-1-2024-09-10"></a>
### 2.66.1 (2024. 09. 10.) { #2-66-1-2024-09-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unreal.zip)

<a id="661-2024-09-10-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。

<a id="661-2024-09-10-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.66.3](./release-notes-android/#2-66-3-2024-09-10)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2-66-2-2024-08-27)

<a id="2-66-0-2024-08-27"></a>
### 2.66.0 (2024. 08. 27.) { #2-66-0-2024-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unreal.zip)

<a id="660-2024-08-27-feature-updates"></a>
#### 機能改善・変更
* APIの使用方法を変更しました。
    * `IModuleInterface`を継承した**IGamebase**で提供していたAPIを`UGameInstanceSubsystem`を継承した**UGamebaseSubsytem**で提供するように変更しました。
    * **UGamebaseSubsytem**はGameInstanceのサブシステムであるため、GameInstanceライフサイクルに従い、SDK API呼び出し時に使用するGameInstanceを通じて該当サブシステムを検索してAPIを使用する必要があります。
* GamebaseInterfaceモジュールが削除されました。 Gamebaseプラグイン使用時GamebaseInterfaceモジュールを削除してから使用してください。
* (Windows) GameInstanceが複数ある環境で使用できます。

<a id="660-2024-08-27-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.66.2](./release-notes-android/#2-66-2-2024-08-27)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2-66-2-2024-08-27)

<a id="2-64-0-2024-06-11"></a>
### 2.64.0 (2024. 06. 11.) { #2-64-0-2024-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Unreal.zip)

<a id="640-2024-06-11-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。

<a id="640-2024-06-11-bug-fixes"></a>
#### 不具合修正
* C++環境によって警告が発生し、ビルド時にエラーが発生するコードを修正しました。
* (Android) ProGuard宣言が抜けていてAPI呼び出し時にエラーが発生する内容を修正しました。

<a id="640-2024-06-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2-64-0-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2-64-0-2024-05-28)

<a id="2-63-0-2024-04-23"></a>
### 2.63.0 (2024. 04. 23.) { #2-63-0-2024-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unreal.zip)

<a id="630-2024-04-23-added-features"></a>
#### 機能追加
* (Android) Firebase Notificationの設定方法が変更され、プラグイン内でgoogle-services-json.xmlファイルを修正するのではなく、[Android設定ツール](./unreal-started/#android-settings)でgoogle-services.jsonファイルのパスを指定するように変更されました。
* (iOS) Gamebase Unreal SDKにPrivacy manifestと署名を適用しました。

<a id="630-2024-04-23-feature-updates"></a>
#### 機能改善・変更
* (iOS)ビルド時にエラーが発生しないように修正しました。

<a id="630-2024-04-23-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2-63-0-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2-63-0-2024-04-23)

<a id="2-62-0-2024-03-26"></a>
### 2.62.0 (2024. 03. 26.) { #2-62-0-2024-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unreal.zip)

<a id="620-2024-03-26-added-features"></a>
#### 機能追加
* (iOS) Gamebase SDK内部iOSフレームワークにPrivacy manifestと署名を適用しました。

<a id="620-2024-03-26-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。

<a id="620-2024-03-26-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.62.0](./release-notes-android/#2-62-0-2024-03-26)
* [Gamebase iOS SDK 2.62.0](./release-notes-ios/#2-62-0-2024-03-26)

<a id="2-60-0-2024-02-15"></a>
### 2.60.0 (2024. 02. 15.) { #2-60-0-2024-02-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unreal.zip)

<a id="600-2024-02-15-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックを改善しました。

<a id="600-2024-02-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2-60-0-2024-01-23)
* [Gamebase iOS SDK 2.60.1](./release-notes-ios/#2-60-1-2024-02-15)

<a id="2-58-0-2023-11-28"></a>
### 2.58.0 (2023. 11. 28.) { #2-58-0-2023-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-Unreal.zip)

<a id="580-2023-11-28-bug-fixes"></a>
#### 不具合修正
* (Windows)サーバープッシュが動作しない問題を修正しました。
* 初期化時にクラッシュが発生する可能性があるロジックを修正しました。

<a id="580-2023-11-28-platform-specific-changes"></a>
#### プラットフォーム別の変更点
* [Gamebase Android SDK 2.58.0](./release-notes-android/#2-58-0-2023-11-28)
* [Gamebase iOS SDK 2.58.0](./release-notes-ios/#2-58-0-2023-11-28)

<a id="2-57-0-2023-11-14"></a>
### 2.57.0 (2023. 11. 14.) { #2-57-0-2023-11-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unreal.zip)

<a id="570-2023-11-14-added-features"></a>
#### 機能改善/変更
* Windowsプラットフォームのサポートを追加
    * [Windows設定ツール](./unreal-started/#windows-settings)が追加されました。
    * プラットフォームでサポートするAPIは、各文書の`UNREAL_WINDOWS`項目をご確認ください。

<a id="570-2023-11-14-platform-specific-changes"></a>
#### プラットフォーム別の変更点
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2-57-0-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2-57-0-2023-10-31)

<a id="2-56-0-2023-10-17"></a>
### 2.56.0 (2023. 10. 17.) { #2-56-0-2023-10-17 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Unreal.zip)

<a id="560-2023-10-17-added-features"></a>
#### 機能追加
* [Android設定ツール](./unreal-started/#android-settings)でストア設定が追加されました。
    * Amazon Appstore、Huawei AppGallery、MyCardの選択が追加されました。
    * ONE Storeを選択した場合、ストアのバージョン選択オプションが追加されました。
* [iOS設定ツール](./unreal-started/#ios-settings)でNaver IdP設定が追加されました。
* (Android) LoginForLastLoggedInProvider呼び出し中にローディングアニメーションを非表示にするオプションを指定できる新規APIが追加されました。
    * LoginForLastLoggedInProvider(const UGamebaseJsonObject& additionalInfo、const FGamebaseAuthTokenDelegate& onCallback)
    * API呼び出し方法は、次のガイド文書を参照してください。
        * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Login > Login Flow > Login as the Latest Login IdP](./unreal-authentication/#login-as-the-latest-login-idp)
* (Android) Android 13以上のOSでRegisterPush APIを呼び出した時、Push権限リクエストポップアップが自動的に表示されないようにするFGamebasePushConfiguration.requestNotificationPermissionフィールドが追加されました。
* (iOS)ユーザーがプッシュ権限を拒否してもトークンを登録できるようにFGamebasePushConfiguration.alwaysAllowTokenRegistrationフィールドが追加されました。

<a id="560-2023-10-17-feature-updates"></a>
#### 機能改善・変更
* 提供されるタイプがUSTRUCTから一般構造体に変更されました。
    * 結果として受け取るタイプは基本的に提供されない値の場合、Toptional形式で提供されます。

<a id="560-2023-10-17-bug-fixes"></a>
#### 不具合修正
* ログイン後、退会猶予情報および決済アビューズ自動解除情報が正常に伝達されるように修正しました。

<a id="560-2023-10-17-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.56.0](./release-notes-android/#2-56-0-2023-09-26)
* [Gamebase iOS SDK 2.55.2](./release-notes-ios/#2-55-2-2023-09-26)

<a id="2-49-1-2023-04-14"></a>
### 2.49.1 (2023. 04. 14.) { #2-49-1-2023-04-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-Unreal.zip)

<a id="491-2023-04-14-bug-fixes"></a>
#### 不具合修正
* (iOS)決済商品照会APIを呼び出す際、クラッシュが発生しないように修正しました。

<a id="491-2023-04-14-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2-48-0-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2-49-0-2023-04-11)

<a id="2-49-0-2023-04-11"></a>
### 2.49.0 (2023. 04. 11.) { #2-49-0-2023-04-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unreal.zip)

<a id="490-2023-04-11-added-features"></a>
#### 機能追加
* 未消費履歴照会APIが変更され、新規APIに変更する必要があります。
 
        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
 
* 有効化購読照会APIが変更され、新規APIに変更する必要があります。
    * 既存APIと同じ結果を得るには**FGamebasePurchasableConfiguration.allStores**を**true**に設定する必要があります。
 
            // Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* (Android) IAP購読状態を照会できるRequestSubscriptionsStatus APIが追加されました。
* (Android) Webビューで固定フォントサイズを使用するかどうかを設定するフィールドを再サポートします。
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) Webビューでカットアウト(ノッチ)領域を含むすべての利用可能なスクリーンスペースを使用してレンダリングできる設定が追加されました。
    * GamebaseWebViewConfiguration.renderOutsideSafeArea

<a id="490-2023-04-11-feature-updates"></a>
#### 機能改善・変更
* Unrealの最小サポートバージョンが4.26に変更されました。
* (iOS) Xcode 14.1でビルド時にエラーが発生する問題を修正しました。
    
<a id="490-2023-04-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2-48-0-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2-49-0-2023-04-11)

<a id="2-43-3-2022-10-04"></a>
### 2.43.3 (2022. 10. 04.) { #2-43-3-2022-10-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-Unreal.zip)

<a id="433-2022-10-04-feature-updates"></a>
#### 機能改善・変更
* LINEログインを行う時、サービスを提供するRegionを入力するように変更されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Login with IdP](./unreal-authentication/#login-with-idp)
    
<a id="433-2022-10-04-platform-specific-changes"></a>
#### プラットフォーム別変更事項
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2-43-0-2022-09-07)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2-43-3-2022-10-04)

<a id="2-42-1-2022-08-09"></a>
### 2.42.1 (2022. 08. 09.) { #2-42-1-2022-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unreal.zip)

<a id="421-2022-08-09-added-features"></a>
#### 機能追加
* FGamebaseForcingMappingTicketクラスにマッピングユーザー状態を表すmappedUserValidフィールドが追加されました。
*  [iOS設定ツール](./unreal-started/#ios-settings)でXcodeのパスを指定できるように**Xcode Path**設定が追加されました。

<a id="421-2022-08-09-feature-updates"></a>
#### 機能改善・変更
* キックアウトポップアップウィンドウを表示するかどうかはGamebaseコンソールでキックアウト登録時に設定できるため、次のフィールドは今後は使用しません。
    * **FGamebaseConfiguration.bEnableKickoutPopup**
* FGamebaseConfiguration内の一部フィールドにデフォルト値が追加されました。
    * bEnableLaunchingStatusPopupのデフォルト値がtrueに設定されました。
    * bEnableBanPopupのデフォルト値がtrueに設定されました。
* FWebViewで固定フォントサイズを使用するかどうかを設定するフィールドは、今後は使用しません。
    * **FGamebaseWebViewConfiguration.enableFixedFontSize**
* FGamebaseWebViewConfiguratio内の一部フィールドに基本値が追加されました。
    * ナビゲーションバーの色相フィールドであるcolorR、colorG、colorB、colorAのデフォルト値が18、93、230、255に設定されました。
    * ナビゲーションバーの有効/無効を指定するフィールドであるisNavigationBarVisibleのデフォルト値がtrueに設定されました。
    * Webビュー内の戻るボタンの有効/無効を指定するフィールドであるisBackButtonVisibleのデフォルト値がtrueに設定されました。
    
<a id="421-2022-08-09-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2-42-1-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2-42-1-2022-08-09)

<a id="2-41-0-2022-07-05"></a>
### 2.41.0 (2022. 07. 05.) { #2-41-0-2022-07-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unreal.zip)

<a id="410-2022-07-05-added-features"></a>
#### 機能追加
* GamebaseEventHandlerのGamebaseEventCategoryに**IdPRevoked**タイプが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unreal-etc/#idp-revoked)

<a id="410-2022-07-05-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2-41-0-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2-41-0-2022-07-05)

<a id="2-40-1-2022-06-14"></a>
### 2.40.1 (2022. 06. 14.) { #2-40-1-2022-06-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

<a id="401-2022-06-14-bug-fixes"></a>
#### 不具合修正
* クラッシュが発生することがあるロジックが修正されました。
* (iOS)同じAPIを連続して呼び出すとき、コールバックが正常に伝達されない問題が修正されました。

<a id="2-40-0-2022-05-24"></a>
### 2.40.0 (2022. 05. 24.) { #2-40-0-2022-05-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

<a id="400-2022-05-24-added-features"></a>
#### 機能追加
*  [iOS設定ツール](./unreal-started/#ios-settings)を提供します。
    * 既存プロジェクト設定で**Gamebase**と表示されましたがアップデート後は**Gamebase - Android**、**Gamebase - iOS**と表示されます。
    * iOS設定ツールを提供し、ビルド時に必要なフレームワークのみ含まれるように修正しました。
* 共通約款API呼び出し後に約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * FGamebaseShowTermsViewResult
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush().QueryNotificationAllowed()
* 約款が表示されたかどうかを知ることができるAPIが追加されました。
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms().IsShowingTermsView()
* Webビューでナビゲーションバーを隠すことができるオプションが追加されました。
    * FGamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) Webビューでフォントサイズを固定することができるオプションが追加されました。
    * FGamebaseTermsConfiguration.enableFixedFontSize
* (Android)約款ウィンドウで文字サイズを固定することができるオプションが追加されました。
    * FGamebaseTermsConfiguration.enableFixedFontSize
* 決済時にプロモーションかどうかを知ることができるisPromotionフィールドが追加されました。
    * FGamebasePurchasableReceipt.isPromotion
* 決済時にテスト決済かどうかを知ることができるisTestPurchaseフィールドが追加されました。
    * FGamebasePurchasableReceipt.isTestPurchase
* サポートURLの後ろにパラメータを追加できるように次のフィールドが追加されました。
    * FGamebaseContactConfiguration.additionalParameters

<a id="400-2022-05-24-feature-updates"></a>
#### 機能改善・変更
* API結果コールバック呼び出し時にGameThreadに切り替えて呼び出すように修正しました。
* RequestActivatedPurchases API呼び出し時に内部で2回呼び出される問題が修正されました。
* 一部APIの名前が変更されました。
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* キックアウトポップアップウィンドウを表示するかどうかは、Gamebaseコンソールでキックアウト登録時に設定できるため、次のフィールドは使用しなくなりました。
    * FGamebaseConfiguration.enableKickoutPopup
    
<a id="400-2022-05-24-platform-specific-changes"></a>
#### 各プラットフォームの変更事項
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2-40-0-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2-40-0-2022-05-24)

<a id="2-33-1-2022-02-22"></a>
### 2.33.1 (2022. 02. 22.) { #2-33-1-2022-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.1/GamebaseSDK-Unreal.zip)

<a id="331-2022-02-22-bug-fixes"></a>
#### 不具合修正
* iOSビルド時に発生するエラーを修正しました。

<a id="2-33-0-2022-01-25"></a>
### 2.33.0 (2022. 01. 25.) { #2-33-0-2022-01-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

<a id="330-20220125-added-features"></a>
#### 機能追加
* 「決済アビューズ自動解除」機能が追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > GraceBan](./unreal-authentication/#graceban)
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止になるべきユーザーが「利用停止猶予状態」の場合に、設定した期間内に決済アビューズ解除条件を全て満たすと正常にプレイできます。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常プレイが可能になります。
    * 期間内に条件を満たさなかった場合は利用が停止されます。
* 決済アビューズ自動解除機能を使用するゲームは、ログイン後に常にAuthToken.member.graceBanInfo API値を確認し、nullではない有効なGraceBanInfoオブジェクトをリターンした場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。
* 強制マッピング時にIdPログインをもう一度行わなければいけない不便さを改善した新しい強制マッピングAPIが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping()呼び出し後にAUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)エラーが発生した時、該当アカウントにログインすることができるAPIが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* GamebaseEventHandlerのGamebaseEventCategoryに**GamebaseEventCategory::ServerPushAppKickOutMessageReceived**タイプが追加されました。
    * このイベントの活用方法は次の文書を参照してください。
    * [Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* GamebaseEventHandlerのGamebaseEventCategoryに**GamebaseEventCategory::LoggedOut**タイプが追加されました。
    * Gamebase Access Tokenの有効期限が切れてログインが必要なときに動作します。
    * [Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* 共通約款ウィンドウの設定を変更できる新規APIが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### 機能改善・変更
* エラーコードの追加と変更
    * GamebaseErrorCode::UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode::SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。
    
<a id="330-20220125-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2-33-0-2022-01-25)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2-33-0-2022-01-25)

<a id="2-26-1-2021-11-23"></a>
### 2.26.1 (2021. 11. 23.) { #2-26-1-2021-11-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

<a id="261-20211123-bug-fixes"></a>
#### 不具合修正
* GamebaseDisplayLanguageCodeフィンランド語の誤字を修正
    * Finish → Finnish

<a id="2-26-0-2021-09-28"></a>
### 2.26.0 (2021. 09. 28.) { #2-26-0-2021-09-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

<a id="260-20210928-added-features"></a>
#### 機能追加
* 共通約款機能の追加
    * 約款WebViewを開くAPIの追加
    * 約款リストおよびユーザー別同意有無を照会するAPIを追加
    * ユーザー別の約款同意有無をGamebaseサーバーに保存するAPIを追加

<a id="260-20210928-feature-updates"></a>
#### 機能改善・変更
* サポートのタイプがTOAST組織商品(Online Contact)の場合、ログインを行わなくてもサポートが表示されるように変更
* 内部ローンチURLの変更
* GamebaseからAndroid multidexの適用削除

<a id="2-19-2-2021-06-29"></a>
### 2.19.2 (2021. 06. 29.) { #2-19-2-2021-06-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.2/GamebaseSDK-Unreal.zip)

<a id="192-20210629-bug-fixes"></a>
#### 不具合修正
* イメージ告知ShowImageNotices API呼び出し時にonEventCallbackを登録していない場合、閉じるボタンを押した時にクラッシュが発生する問題を修正
* Android設定ツール - Enable Hangame、Enable Weiboが正常に動作しない問題を修正

<a id="2-19-1-2021-02-09"></a>
### 2.19.1 (2021. 02. 09.) { #2-19-1-2021-02-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Unreal.zip)

<a id="191-20210209-bug-fixes"></a>
#### 不具合修正 
* Unityビルド中に除外されるファイルが生じた時に発生するコンパイルエラーを修正

<a id="2-19-0-2021-01-26"></a>
### 2.19.0 (2021. 01. 26.) { #2-19-0-2021-01-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unreal.zip)

<a id="190-20210126-more-features"></a>
#### 機能追加
* SDK配布：2.16.0～2.19.0累積した内容を反映
	* [Android設定ツール](./unreal-started/#android-settings)提供：Gamebase_Android_UPL.xmlファイルを修正する代わりに設定ツールを使用してください。
	* サポート機能を追加	
	* 認証追加：Hangame、Weibo
	* Galaxyストアを追加
	* 決済アイテム情報にローカライズされた商品情報を追加：localizedTitle、localizedDescription
	* Android設定ツールを提供
	* Unreal 4.26サポート

<a id="2-15-0-2020-10-27"></a>
### 2.15.0 (2020. 10. 27.) { #2-15-0-2020-10-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unreal.zip)

<a id="150-october-27-2020-more-features"></a>
#### 機能追加
* Unreal SDK機能追加：SDK 2.15.0
    * 既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
        * ServerPush、Observer機能が含まれており、プロモーション決済イベントおよびプッシュイベントを確認可能
    * API追加
    	* 商品IDで決済をリクエストし、追加情報(UserPayload)を入力して決済完了時に確認できる決済APIを追加
    	* イメージ告知表示：showImageNotices
    	* Pushトークン情報確認：queryTokenInfo
    * プッシュトークン登録時、NotificationOption設定でアプリがフォアグラウンド(foreground)状態でもプッシュ通知を受け取れるように機能を追加
    * WebViewConfiguration contentMode設定を追加

<a id="150-october-27-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.15.0
    * (Unreal) TOAST SDKアップデート：Android(0.23.0), iOS(0.26.0), Unity(0.21.0)   

<a id="150-october-27-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.15.0    
    * (Unreal)決済モジュールにProGuard宣言が抜けていたエラーを修正

<a id="2-9-1-2020-08-25"></a>
### 2.9.1 (2020. 08. 25.) { #2-9-1-2020-08-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unreal.zip)

<a id="91-august-25-2020-more-features"></a>
#### 機能追加
* [SDK] 2.9.1
    * (Unreal) Unreal 4.22 ～ 4.25サポート
    * (Unreal) PLCrashReporterイシューサポート: [ガイド](./unreal-started/#ios-settings)

<a id="91-august-25-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.9.1
    * (Unreal) iOS Plugin内部Gamebase SDK for iOSバージョンアップデート(2.9.1)
    * (Unreal) UObjectリファレンシング処理が抜けていた部分を修正

<a id="2-9-0-2020-05-12"></a>
### 2.9.0 (2020. 05. 12.) { #2-9-0-2020-05-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

<a id="90-may-12-2020-more-features"></a>
#### 機能追加
* [SDK] 2.9.0
	* (Unreal) SDK新規配布
