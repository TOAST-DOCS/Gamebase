<!-- pre-align:aligned sig=602ad029c65d -->

<a id="game-gamebase-release-notes-unity"></a>
## Game > Gamebase > リリースノート > Unity { #game-gamebase-release-notes-unity }

<a id="2-82-0-2026-08-11"></a>
### 2.82.0 (2026. 08. 11.) { #2-82-0-2026-08-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.82.0/GamebaseSDK-Unity.zip)

<a id="820-2026-08-11-feature-updates"></a>
#### 機能改善・変更
* (Android、iOS) 無効なデータに対する検証ロジックを改善しました。

<a id="820-2026-08-11-bug-fixes"></a>
#### 不具合の修正
* (Windows、macOS) Webビューでgamebase://openbrowserスキームが処理されない不具合を修正しました。

<a id="820-2026-08-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.82.0](./release-notes-android/#2-82-0-2026-07-28)
* [Gamebase iOS SDK 2.82.0](./release-notes-ios/#2-82-0-2026-07-28)

<a id="820-2026-08-11-setting-tool-v301"></a>
#### Setting Tool (v3.0.1)

* WebGLプラットフォーム専用アダプタのインストールが可能になります。

<a id="2-81-4-2026-07-14"></a>
### 2.81.4 (2026. 07. 14.) { #2-81-4-2026-07-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.4/GamebaseSDK-Unity.zip)

<a id="814-2026-07-14-bug-fixes"></a>
#### 不具合の修正
* (Windows, macOS) 不安定なネットワーク環境でWebsocketの再接続を試みる際、断続的にエラーが発生する問題を修正しました。

<a id="814-2026-07-14-feature-updates"></a>
#### 機能改善
* Gamebase SDKにAssembly Definition(.asmdef)を適用しました。

<a id="814-2026-07-14-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.81.0](./release-notes-android/#2-81-0-2026-06-23)
* [Gamebase iOS SDK 2.81.3](./release-notes-ios/#2-81-3-2026-05-27)

<a id="2-81-3-2026-05-27"></a>
### 2.81.3 (2026. 05. 27.) { #2-81-3-2026-05-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.3/GamebaseSDK-Unity.zip)

<a id="813-2026-05-27-bug-fixes"></a>
#### 不具合の修正
* (Windows, MacOS, WebGL) Log&Crashサーバーへのログ送信時、userFieldsに入力するキーと値のペアの値がnullの場合にエラーが発生していた問題を修正しました。
* (Windows, MacOS, WebGL) Log&Crashサーバーへのログ送信時、logTypeを設定してもLog&CrashページでlogTypeが"NORMAL"と表示される問題を修正しました。

<a id="813-2026-05-27-platform-specific-changes"></a>
#### プラットフォーム別の変更点
* [Gamebase Android SDK 2.80.2](./release-notes-android/#2-80-2-2026-04-28)
* [Gamebase iOS SDK 2.81.3](./release-notes-ios/#2-81-3-2026-05-27)

<a id="2-81-1-2026-04-28"></a>
### 2.81.1 (2026. 04. 28.) { #2-81-1-2026-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.1/GamebaseSDK-Unity.zip)

<a id="811-2026-04-28-bug-fixes"></a>
#### 不具合の修正
* (Windows、MacOS) 画像お知らせの「今日はこれ以上表示しない」にチェックを入れた後、ShowImageNoticesを再度呼び出した際に、終了コールバックを受け取れない問題を修正しました。
* (Windows、MacOS) ウィンドウモードで実行中にゲーム画面のサイズ変更がWebビュー領域に反映されない問題を修正しました。

<a id="811-2026-04-28-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.80.2](./release-notes-android/#2-80-2-2026-04-28)
* [Gamebase iOS SDK 2.81.2](./release-notes-ios/#2-81-2-2026-04-28)

<a id="2-81-0-2026-03-24"></a>
### 2.81.0 (2026. 03. 24.) { #2-81-0-2026-03-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.0/GamebaseSDK-Unity.zip)

<a id="810-2026-03-24-1"></a>
#### 기능 추가
* (Windows, macOS) 외부 브라우저 로그인 IDP에 Epicgames가 추가되었습니다.

<a id="810-2026-03-24-2"></a>
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.80.0](./release-notes-android/#2-80-0-2026-02-13)
* [Gamebase iOS SDK 2.80.0](./release-notes-ios/#2-80-0-2026-02-13)

<a id="2-80-1-2026-03-10"></a>
### 2.80.1 (2026. 03. 10.) { #2-80-1-2026-03-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.1/GamebaseSDK-Unity.zip)

<a id="801-2026-03-10-1"></a>
#### 버그 수정
* (iOS) GameCenter에서 AddMapping 수행 시 오류가 발생하던 문제를 수정했습니다.
* (Windows) WebView 오픈시, 타이틀 영역을 클릭하면 웹뷰가 닫히는 문제를 수정했습니다.

<a id="801-2026-03-10-2"></a>
#### 기능 개선
* (Windows) WebView 내부 로직을 개선하였습니다.

<a id="2-80-0-2026-02-13"></a>
### 2.80.0 (2026. 02. 13.) { #2-80-0-2026-02-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-Unity.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### 機能改善
* (Android、iOS) 決済リクエスト時、遅延決済や保護者の同意のように決済完了を待つ必要がある状況が発生した場合、新規追加された**PURCHASE_PENDING(4008)**エラーが発生します。
* (Android、iOS) Gamebase Event HandlerのGamebaseEventCategory.PURCHASE_UPDATEDイベント機能が拡張されました。
  * アプリの実行中、GamebaseEventHandlerを通じてPending決済(遅延決済、保護者の同意など)の完了イベントを受け取ることができます。

<a id="800-2026-02-13-bug-fixes"></a>
#### 不具合の修正
* (Windows、macOS) WebViewを閉じた後に再度開いた際、既存のウィンドウへ戻ることが可能であった問題を修正しました。
* (Windows、macOS) WebViewナビゲーションによって上部のWebビューが隠れる問題を修正しました。
* (Android) ゲームの告知背景が透明に表示される問題を修正しました。

<a id="2-79-0-2026-01-27"></a>
### 2.79.0 (2026. 01. 27.) { #2-79-0-2026-01-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-Unity.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### 기능 개선
* (Windows, macOS) WebViewのbarHeightが設定されていない場合、ナビゲーションが表示されなかった問題を修正しました。
* (Windows, macOS) WebViewのisBackButtonVisible設定時、Closeボタンが表示されなかった問題を修正しました。

<a id="2-77-0-2025-12-09"></a>
### 2.77.0 (2025. 12. 09.) { #2-77-0-2025-12-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.77.0/GamebaseSDK-Unity.zip)

####  機能追加
* (WebGL) ブラウザログインをサポート

<a id="770-2025-12-09-added-features"></a>
#### 機能追加

<!-- TODO: translate body -->

<a id="770-2025-12-09-feature-updates"></a>
#### 機能改善
* Gamebase.Purchase.RequestItemListAtIAPConsole() APIが非推奨となりました。
  * Gamebase.Purchase.RequestItemListPurchasable() APIの使用を推奨します。

<a id="770-2025-12-09-bug-fixes"></a>
#### 不具合の修正
* (WebGL) ゲストログインに失敗する問題を修正しました。

<a id="2-76-0-2025-11-28"></a>
### 2.76.0 (2025. 11. 28.) { #2-76-0-2025-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.76.0/GamebaseSDK-Unity.zip)

<a id="760-2025-11-28-added-features"></a>
#### 機能追加
* 最近投稿されたゲーム告知の投稿時間を提供するために、launching.app.gameNotice.latestNoticeTimeMillisフィールドを追加しました。
* (Android) 米国テキサス、ユタ、ルイジアナなど特定管轄権の年齢確認関連法律遵守を支援するために、Google Play Age Signalsベースの年齢確認APIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 参考事項 > Age Signals Support](./unity-etc/#age-signals-support)

<a id="2-75-1-2025-10-17"></a>
### 2.75.1 (2025. 10. 17.) { #2-75-1-2025-10-17 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.1/GamebaseSDK-Unity.zip)

<a id="751-2025-10-17-bug-fixes"></a>
#### 不具合の修正
* (Windows) AdditionalInfoがnullの場合に発生していた例外を修正しました。
* (macOS) GamebaseUtilで発生していたDllNotFoundException問題を修正しました。

<a id="2-75-0-2025-09-23"></a>
### 2.75.0 (2025. 09. 23.) { #2-75-0-2025-09-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-Unity.zip)

<a id="750-2025-09-23-added-features"></a>
#### 機能追加
* (Windows) Mapping機能追加

<a id="750-2025-09-23-feature-updates"></a>
#### 機能改善・変更
* (Android) Google Playの16KBページ制限対応
* 内部ロジックを改善しました。

<a id="2-74-0-2025-08-26"></a>
### 2.74.0 (2025. 08. 26.) { #2-74-0-2025-08-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.74.0/GamebaseSDK-Unity.zip)

<a id="740-2025-08-26-bug-fixes"></a>
#### 不具合の修正
* (iOS) ChangeLogin時に発生していたクラッシュ問題を修正しました。
* (macOS) GamebaseUtilで発生していたDllNotFoundException問題を修正しました。

<a id="740-2025-08-26-1"></a>
#### その他
* 最小サポートバージョンがUnity 2022.3.10に引き上げられました。

<a id="2-73-2-2025-07-29"></a>
### 2.73.2 (2025. 07. 29.) { #2-73-2-2025-07-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.2/GamebaseSDK-Unity.zip)

<a id="732-2025-07-29-feature-updates"></a>
#### 機能改善
* (Standalone)ログインIDPの追加サポート: Twitter、Apple、LINE

<a id="732-2025-07-29-end-of-support"></a>
#### サポート終了
* Amazon Appstoreのサポートを終了します。

<a id="2-73-1-2025-07-22"></a>
### 2.73.1 (2025. 07. 22.) { #2-73-1-2025-07-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-Unity.zip)

<a id="731-2025-07-22-bug-fixes"></a>
#### 不具合修正
* (iOS)ビルドエラー修正
* (macOS) Webビューアダプタビルドエラーの修正

<a id="2-73-0-2025-07-15"></a>
### 2.73.0 (2025. 07. 15.) { #2-73-0-2025-07-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Unity.zip)

<a id="730-2025-07-15-added-features"></a>
#### 機能追加

<a id="730-2025-07-15-feature-updates"></a>
#### 機能改善・変更
* (Windows, macOS) IdPログイン時にWebビューから外部ブラウザに変更しました。
	* サポートブラウザ
		* Windows :全てのブラウザ
		* macOS : Chrome, Safari, Firefox, whale

* 外部ブラウザログインキャンセルAPIを追加しました。
* 進行中の外部ブラウザログインリクエスト中にIDPを変更したい場合、既存のリクエストをキャンセルするため。
	* CancelLoginWithExternalBrowser()

<a id="730-2025-07-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.73.0](./release-notes-android/#2-73-0-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2-73-0-2025-07-15)

<a id="2-72-0-2025-06-24"></a>
### 2.72.0 (2025. 06. 24.) { #2-72-0-2025-06-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Unity.zip)

<a id="720-2025-06-24-added-features"></a>
#### 機能追加

* (Windows)アップデートポップアップに詳細表示ボタンを追加しました。
* (Windows)利用停止ポップアップにサポートリンクを追加しました。

<a id="720-2025-06-24-feature-updates"></a>
#### 機能改善・変更

* (Windows)内部ロジックを改善しました。

<a id="720-2025-06-24-bug-fixes"></a>
#### 不具合修正

* (Windows)点検状態に更新されない問題を修正しました。

<a id="720-2025-06-24-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.72.0](./release-notes-android/#2-72-0-2025-06-24)
* [Gamebase iOS SDK 2.72.0](./release-notes-ios/#2-72-0-2025-06-24)

<a id="2-71-1-2025-06-11"></a>
### 2.71.1 (2025. 06. 11.) { #2-71-1-2025-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Unity.zip)

<a id="711-2025-06-11-bug-fixes"></a>
#### 不具合修正

* (macOS) GamebaseUtilのDllNotFoundException問題を修正しました。

<a id="2-71-0-2025-04-15"></a>
### 2.71.0 (2025. 04. 15.) { #2-71-0-2025-04-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Unity.zip)

<a id="710-2025-04-15-added-features"></a>
#### 機能追加
* 「ゲーム告知」新機能を追加しました。
    * Gamebase.GameNotice.OpenGameNotice(GamebaseCallback.ErrorDelegate callback)
    * API呼び出し方法は次のガイド文書を参照してください。
        * [Game > Gamebase > Unity SDK使用ガイド > 告知 > ゲーム告知](./unity-ui/#gamenotice)

<a id="710-2025-04-15-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。
* (iOS) ViewController設定ロジックを改善して初期化失敗エラーを防止します。

<a id="710-2025-04-15-bug-fixes"></a>
#### 不具合修正

* macOS 15.4でクラッシュが発生する問題を修正しました。

<a id="710-2025-04-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.71.0](./release-notes-android/#2-71-0-2025-04-15)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2-71-0-2025-04-15)

<a id="2-70-1-2025-03-13"></a>
### 2.70.1 (2025. 03. 13.) { #2-70-1-2025-03-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.1/GamebaseSDK-Unity.zip)

<a id="701-2025-03-13-bug-fixes"></a>
#### 不具合修正

* (Android) ShowWebView、ShowTermsViewの呼び出し時にConfigurationがないとクラッシュが発生する問題を修正しました。

<a id="701-2025-03-13-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.70.1](./release-notes-android/#2-70-1-2025-03-13)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2-70-0-2025-03-11)

<a id="2-70-0-2025-03-11"></a>
### 2.70.0 (2025. 03. 11.) { #2-70-0-2025-03-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Unity.zip)

<a id="700-2025-03-11-added-features"></a>
#### 機能追加

* (Android) 「GPGS自動ログイン」機能連動時、ユーザーにGPGSログインをアプリインストール後に一度だけ確認する初期化オプションを追加しました。
  * GamebaseRequest.GamebaseConfiguration enableGPGSSignInCheck
    * 基本設定はtrueで、ユーザーがGPGSログインを拒否してもGamebase初期化時にGPGSログインウィンドウを再度表示します。
    * falseに設定すると、アプリ初回実行時のみGPGSログインウィンドウが一度表示されます。
* GamebaseWebViewにナビゲーションバーのtitleカラーとicon tintカラー設定オプションを追加しました。
    * WebView.Configuration navigationTitleColor
    * WebView.Configuration navigationIconTintColor

<a id="700-2025-03-11-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="700-2025-03-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.70.0](./release-notes-android/#2-70-0-2025-03-11)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2-70-0-2025-03-11)

<a id="700-2025-03-11-setting-tool-v300"></a>
#### Setting Tool (v3.0.0)

* 使用目的に合わせてユーザーフレンドリーなUXに改善しました。
* 直感的な機能提供により、設定とアップデートがより簡単になりました。
* 配布時に柔軟にアップデートできるように改善しました。

<a id="2-69-0-2025-01-21"></a>
### 2.69.0 (2025. 1. 21.) { #2-69-0-2025-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unity.zip)

<a id="690-2025-1-21-added-features"></a>
#### 機能追加

* RequestLastLoggedInProvider API追加
* (Android) WebView Cutout color機能追加
* (Windows, macOS) X(Twitter)ログインをサポート

<a id="690-2025-1-21-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。
* WebView色設定関連コードを改善
  * Configuration内部にフィールド追加
    * WebView.Configuration navigationColor
    * Community.Configuration backgroundColor
    * ImageNotice.Configuration backgroundColor

<a id="690-2025-1-21-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2-69-0-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2-69-0-2025-01-21)

<a id="2-68-1-2024-12-10"></a>
### 2.68.1 (2024. 12. 10.) { #2-68-1-2024-12-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unity.zip)

<a id="681-2024-12-10-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="681-2024-12-10-platform-specific-changes"></a>
#### プラットフォーム別の変更事項

* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2-68-1-2024-12-10)

<a id="2-68-0-2024-11-26"></a>
### 2.68.0 (2024. 11. 26.) { #2-68-0-2024-11-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unity.zip)

<a id="680-2024-11-26-ended-support"></a>
#### サポート終了

* FacebookAdapter for Unityサポートが終了されます。

<a id="680-2024-11-26-added-features"></a>
#### 機能追加

* (Android) GameActivityをサポートします。

<a id="680-2024-11-26-feature-updates"></a>
#### 機能改善・変更

* 内部ロジックを改善しました。

<a id="680-2024-11-26-bug-fixes"></a>
#### 不具合修正

* NHN Cloud Consoleでネットワークインサイト設定を有効にするとJSON解析エラーが発生する現象を改善しました。

<a id="680-2024-11-26-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2-68-0-2024-11-26)
* [Gamebase iOS SDK 2.68.0](./release-notes-ios/#2-68-0-2024-11-26)

<a id="2-67-0-2024-10-29"></a>
### 2.67.0 (2024. 10. 29.) { #2-67-0-2024-10-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unity.zip)

<a id="670-2024-10-29-added-features"></a>
#### 機能追加

* (Android, iOS) Steam認証追加

<a id="670-2024-10-29-feature-updates"></a>
#### 機能改善・変更

* Unity最小サポートバージョン変更: 2020.3.16f1
* ローリング画像告知のWebView内部で例外が発生した場合、失敗コールバックが呼び出されるように変更しました。
* 内部ロジックを改善しました。

<a id="670-2024-10-29-bug-fixes"></a>
#### 不具合修正

* storeCodeStandaloneコードにより発生するエラーを修正しました。

<a id="670-2024-10-29-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2-67-0-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2-67-0-2024-10-29)

<a id="2-66-3-2024-09-10"></a>
### 2.66.3 (2024. 09. 10.) { #2-66-3-2024-09-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

<a id="663-2024-09-10-feature-updates"></a>
#### 機能改善・変更
* Unity最小サポートバージョン変更: 2020.3.0f1

<a id="2-66-3-2024-09-05"></a>
### 2.66.3 (2024. 09. 05.) { #2-66-3-2024-09-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

<a id="663-2024-09-05-bug-fixes"></a>
#### 不具合修正
* (iOS) iOS 12で決済後にクラッシュが発生する問題を修正しました。

<a id="2-66-2-2024-08-27"></a>
### 2.66.2 (2024. 08. 27.) { #2-66-2-2024-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-Unity.zip)

<a id="662-2024-08-27-feature-updates"></a>
#### 機能改善・変更
* 以下のフィールドはiOSでは非推奨となりました。Androidでのみ使用できます。
    * GamebaseWebViewConfiguration.orientation deprecated

<a id="2-66-1-2024-07-23"></a>
### 2.66.1 (2024. 07. 23.) { #2-66-1-2024-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unity.zip)

<a id="661-2024-07-23-added-features"></a>
#### 機能追加

* (macOS)個人情報保護ポリシーに対応しました。

<a id="661-2024-07-23-feature-updates"></a>
#### 機能改善

* 内部ロジックを改善しました。

<a id="661-2024-07-23-platform-specific-changes"></a>
#### プラットフォームごとの変更事項
* [Gamebase Android SDK 2.66.1](./release-notes-android/#2-66-1-2024-07-23)
* [Gamebase iOS SDK 2.66.0](./release-notes-ios/#2-66-0-2024-07-23)

<a id="2-66-0-2024-07-12"></a>
### 2.66.0 (2024. 07. 12.) { #2-66-0-2024-07-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unity.zip)

<a id="660-2024-07-12-added-features"></a>
#### 機能追加

* (Android) GPGS V2認証が追加されました。

<a id="660-2024-07-12-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.66.0](./release-notes-android/#2-66-0-2024-07-10)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2-65-1-2024-06-25)

<a id="660-2024-07-12-setting-tool-v290"></a>
#### Setting Tool (v2.9.0)

* GPGS V2認証が追加されました。 (Androidのみ)

<a id="2-65-1-2024-06-25"></a>
### 2.65.1 (2024. 06. 25.) { #2-65-1-2024-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-Unity.zip)

<a id="651-2024-06-25-feature-updates"></a>
#### 機能改善・変更
* 特定のクライアントで表示する画像がない場合、エラーの代わりに成功コールバックが呼び出されるように修正しました。

<a id="651-2024-06-25-bug-fixes"></a>
#### 不具合修正
* 登録されたイメージ告知がない場合、空白のイメージ告知が表示されるイシューを修正しました。
* (macOS) UnityEditorで GamebaseUtils.bundleファイルが参照されないエラーを修正しました。

<a id="651-2024-06-25-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.65.1](./release-notes-android/#2-65-1-2024-06-25)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2-65-1-2024-06-25)

<a id="2-65-0-2024-06-11"></a>
### 2.65.0 (2024. 06. 11.) { #2-65-0-2024-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-Unity.zip)

<a id="650-2024-06-11-added-features"></a>
#### 機能追加

* イメージ告知機能に新規タイプが追加されました。
    * ローリングポップアップタイプが追加されました。
    * 既存のイメージ告知はポップアップタイプと表記されます。
* 内部ロジックを改善しました。

<a id="650-2024-06-11-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.65.0](./release-notes-android/#2-65-0-2024-06-11)
* [Gamebase iOS SDK 2.65.0](./release-notes-ios/#2-65-0-2024-06-11)

<a id="2-64-0-2024-05-28"></a>
### 2.64.0 (2024. 05. 28.) { #2-64-0-2024-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Unity.zip)

<a id="640-2024-05-28-added-features"></a>
#### 機能追加

* 内部ロジックを改善しました。

<a id="640-2024-05-28-platform-specific-changes"></a>
#### プラットフォームごとの変更事項
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2-64-0-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2-64-0-2024-05-28)

<a id="2-63-0-2024-04-23"></a>
### 2.63.0 (2024. 04. 23.) { #2-63-0-2024-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unity.zip)

<a id="630-2024-04-23-added-features"></a>
#### 機能追加

* (MacOS) WebView新規サポート

<a id="630-2024-04-23-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2-63-0-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2-63-0-2024-04-23)

<a id="2-62-0-2024-03-26"></a>
### 2.62.0 (2024. 03. 26.) { #2-62-0-2024-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unity.zip)

<a id="620-2024-03-26-added-features"></a>
#### 機能追加
* iOS個人情報保護ポリシーに対応しました。
    * Gamebase SDKにPrivacy manifestと署名を適用しました。
* Gamebase初期化後に返されるLaunchingInfo VOでテスト端末であることを知らせるためのフィールドが追加されました。
    * **launchingInfo.user.testDevice**
* (MacOS, Windows) TOASTタイプサポートに 対してFAQ/告知事項を直接開くことができる機能を追加しました。

<a id="620-2024-03-26-platform-specific-changes"></a>
#### プラットフォーム別変更事項

<!-- TODO: translate body -->

<a id="2-61-0-2024-02-27"></a>
### 2.61.0 (2024. 02. 27.) { #2-61-0-2024-02-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-Unity.zip)

<a id="610-2024-02-27-bug-fixes"></a>
#### 不具合修正
* (macOS)内部bundleファイルが正常にロードされない問題を修正しました。

<a id="610-2024-02-27-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.61.0](./release-notes-android/#2-61-0-2024-02-27)
* [Gamebase iOS SDK 2.61.0](./release-notes-ios/#2-61-0-2024-02-27)

<a id="2-60-0-2024-01-23"></a>
### 2.60.0 (2024. 01. 23.) { #2-60-0-2024-01-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unity.zip)

<a id="600-2024-01-23-added-features"></a>
#### 機能追加
* 内部ロジックを改善しました。

<a id="600-2024-01-23-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2-60-0-2024-01-23)
* [Gamebase iOS SDK 2.60.0](./release-notes-ios/#2-60-0-2024-01-23)

<a id="2-59-0-2023-12-19"></a>
### 2.59.0 (2023. 12. 19.) { #2-59-0-2023-12-19 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-Unity.zip)

<a id="590-2023-12-19-added-features"></a>
#### 機能追加
* (Standalone) macOSサポートが追加されました。
* 内部ロジックを改善しました。

<a id="590-2023-12-19-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.59.0](./release-notes-android/#2-59-0-2023-12-19)
* [Gamebase iOS SDK 2.59.0](./release-notes-ios/#2-59-0-2023-12-19)

<a id="2-57-0-2023-10-31"></a>
### 2.57.0 (2023. 10. 31.) { #2-57-0-2023-10-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unity.zip)

<a id="570-2023-10-31-added-features"></a>
#### 機能追加
* (共通) try/catch構文で例外に関するログを送信できるGamebase.Logger.Report APIを追加しました。
* (iOS) AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED エラーコードを追加しました。

<a id="570-2023-10-31-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2-57-0-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2-57-0-2023-10-31)

<a id="2-55-0-2023-09-12"></a>
### 2.55.0 (2023. 09. 12.) { #2-55-0-2023-09-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-Unity.zip)

<a id="550-2023-09-12-added-features"></a>
#### 機能追加
* (iOS)ユーザーがプッシュ権限を拒否してもトークンを登録できるようにGamebaseRequest.Push.PushConfiguration.alwaysAllowTokenRegistrationフィールドが追加されました。

<a id="550-2023-09-12-feature-updates"></a>
#### 機能改善
* NHN Cloud Unity SDKのサービス終了に伴い、Gamebase Unity SDK内から削除されました。
* 内部ロジックを改善しました。

<a id="550-2023-09-12-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.55.0](./release-notes-android/#2-55-0-2023-09-12)
* [Gamebase iOS SDK 2.55.0](./release-notes-ios/#2-55-0-2023-09-12)

<a id="2-54-0-2023-08-29"></a>
### 2.54.0 (2023. 08. 29.) { #2-54-0-2023-08-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.54.0/GamebaseSDK-Unity.zip)

<a id="540-2023-08-29-added-features"></a>
#### 機能追加
* (Android)Android 13以降のOSでRegisterPush APIを呼び出した時に、Push権限要求ポップアップが自動的に表示されないようにできるGamebaseRequest.Push.PushConfiguration.requestNotificationPermissionフィールドが追加されました。
* (Android) loginForLastLoggedInProviderの呼び出し中にローディングアニメーションを非表示にするオプションを指定できるAPIが追加されました。
    * Gamebase.LoginForLastLoggedInProvider(Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback);
    * API呼び出し方法は次のガイド文書を参照してください。
        * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Login > Login Flow > Login as the Latest Login IdP](./unity-authentication/#login-with-latest-login-idp)

<a id="540-2023-08-29-bug-fixes"></a>
#### 不具合修正
* (Standalone) Gamebaseサポートを呼び出す際にサービスエラーページが表示されないように修正しました。

<a id="540-2023-08-29-platform-specific-changes"></a>
#### プラットフォーム別の変更点
* [Gamebase Android SDK 2.53.0](./release-notes-android/#2-53-0-2023-08-17)
* [Gamebase iOS SDK 2.54.0](./release-notes-ios/#2-54-0-2023-08-29)

<a id="2-52-1-2023-07-25"></a>
### 2.52.1 (2023. 07. 25.) { #2-52-1-2023-07-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Unity.zip)

<a id="521-2023-07-25-bug-fixes"></a>
#### 不具合修正
* (Standalone) Gamebase Loggerの初期化が完了する前にログを送信するとnull reference exceptionが発生するエラーを修正しました。

<a id="521-2023-07-25-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.52.1](./release-notes-android/#2-52-1-2023-07-17)
* [Gamebase iOS SDK 2.53.0](./release-notes-ios/#2-53-0-2023-07-25)

<a id="2-52-0-2023-06-27"></a>
### 2.52.0 (2023. 06. 27.) { #2-52-0-2023-06-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-Unity.zip)

<a id="520-2023-06-27-added-features"></a>
#### 機能追加
* Setting Tool (v2.7.0)
    * ONE Store v21決済アダプタが追加されました。 (Androidのみ)
    * Gamebase Custom Push Receiverアダプタが追加されました。 (Androidのみ)

<a id="520-2023-06-27-feature-updates"></a>
#### 機能改善
* 外部SDKアップデート: NHN Cloud Unity SDK(0.28.3)
* 内部ロジックを改善しました。

<a id="520-2023-06-27-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.52.0](./release-notes-android/#2-52-0-2023-06-27)
* [Gamebase iOS SDK 2.52.0](./release-notes-ios/#2-52-0-2023-06-27)

<a id="2-51-0-2023-05-30"></a>
### 2.51.0 (2023. 05. 30.) { #2-51-0-2023-05-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.51.0/GamebaseSDK-Unity.zip)

<a id="510-2023-05-30-feature-updates"></a>
#### 機能改善
* 外部SDKアップデート：NHN Cloud Unity SDK(0.28.1)
* 内部ロジックを改善しました。

<a id="510-2023-05-30-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2-50-0-2023-05-16)
* [Gamebase iOS SDK 2.51.0](./release-notes-ios/#2-51-0-2023-05-30)

<a id="2-50-0-2023-05-16"></a>
### 2.50.0 (2023. 05. 16.) { #2-50-0-2023-05-16 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.50.0/GamebaseSDK-Unity.zip)

<a id="500-2023-05-16-added-features"></a>
#### 機能追加
* (Android) MyCardストアが追加されました。
* Setting Tool (v2.5.0)
    * MyCardストアが追加されました。 (Androidのみ)
    * Huawei IAP追加時、 Huawei repository自動設定機能が追加されました。

<a id="500-2023-05-16-feature-updates"></a>
#### 機能改善
* 外部SDKアップデート: NHN Cloud Unity SDK(0.28.0)

<a id="500-2023-05-16-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2-50-0-2023-05-16)
* [Gamebase iOS SDK 2.49.2](./release-notes-ios/#2-49-2-2023-04-28)

<a id="2-49-0-2023-04-25"></a>
### 2.49.0 (2023. 04. 25.) { #2-49-0-2023-04-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unity.zip)

<a id="490-2023-04-25-feature-updates"></a>
#### 機能改善
* (iOS)内部ロジックを改善しました。

<a id="490-2023-04-25-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.49.0](./release-notes-android/#2-49-0-2023-04-25)
* [Gamebase iOS SDK 2.49.1](./release-notes-ios/#2-49-1-2023-04-25)

<a id="2-48-0-2023-03-28"></a>
### 2.48.0 (2023. 03. 28.) { #2-48-0-2023-03-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-Unity.zip)

<a id="480-2023-03-28-feature-updates"></a>
#### 機能改善
* 外部SDKアップデート：NHN Cloud Unity SDK (0.27.4)
* Gamebaseサーバー予備ドメイン適用(GSLB冗長化)
* iOS
    * Xcode最小サポートバージョンが14.1に変更されました。 
    * iOS最小サポートバージョンが11.0に変更されました。
    * armv7、armv7s、i386アーキテクチャのサポートを中断しました。
    * bitcodeのサポートを終了しました。

<a id="480-2023-03-28-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2-48-0-2023-03-28)
* [Gamebase iOS SDK 2.48.0](./release-notes-ios/#2-48-0-2023-03-28)

<a id="2-46-0-2023-01-31"></a>
### 2.46.0 (2023. 01. 31.) { #2-46-0-2023-01-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-Unity.zip)

<a id="460-2023-01-31-added-features"></a>
#### 機能追加
* (WebGL) Googleログイン機能が追加されました。
* (Android) Webビューで固定フォントサイズを使用するかどうかを設定するフィールドを再サポートします。
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) Webビューでカットアウト(ノッチ)領域を含むすべての利用可能なスクリーンスペースを使用してレンダリングできる設定が追加されました。
    * GamebaseWebViewConfiguration.renderOutsideSafeArea
* (Android) IAP購読状態を照会できるRequestSubscriptionsStatus APIが追加されました。

<a id="460-2023-01-31-bug-fixes"></a>
#### 不具合修正
* (Standalone)初期化時に断続的にReflectionTypeLoadExceptionエラーが発生する問題を修正しました。

<a id="460-2023-01-31-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.46.0](./release-notes-android/#2-46-0-2023-01-31)
* [Gamebase iOS SDK 2.46.0](./release-notes-ios/#2-46-0-2023-01-31)

<a id="2-45-0-2022-12-27"></a>
### 2.45.0 (2022. 12. 27.) { #2-45-0-2022-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-Unity.zip)

<a id="450-2022-12-27-added-features"></a>
#### 機能追加
* 未消費履歴照会APIが変更されましたので新規APIに変更してください。 

        // Deprecated API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
         
        // New API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                       GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);

* 有効化購読照会APIが変更されましたので新規APIに変更してください。 
    * 既存APIと同じ結果を受け取るには**GamebaseRequest.Purchase.PurchasableConfiguration.allStores**の値を**true**に設定してください。 

            // Deprecated API 
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
             
            // New API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                        GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);

<a id="450-2022-12-27-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: NHN Cloud Unity SDK (0.27.1)

<a id="450-2022-12-27-platform-specific-changes"></a>
#### 各プラットフォームの変更事項
* [Gamebase Android SDK 2.45.0](./release-notes-android/#2-45-0-2022-12-27)
* [Gamebase iOS SDK 2.45.0](./release-notes-ios/#2-45-0-2022-12-27)

<a id="2-44-2-2022-11-29"></a>
### 2.44.2 (2022. 11. 29.) { #2-44-2-2022-11-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.2/GamebaseSDK-Unity.zip)

<a id="442-2022-11-29-added-features"></a>
#### 機能追加

* Setting Tool (v2.5.0)
    * Onestore v19決済Adapterが追加されました。 (Android Only)
    * 既存SettingToolはUnityプロジェクトから完全に削除した後、最新バージョンで再度インストールする必要があります。

<a id="442-2022-11-29-bug-fixes"></a>
#### 不具合修正
* (iOS)ゲーム中にScreen.orientationを変更する場合、Webビュー、サポートなどのビューコントローラーの影響を受けるAPIが正常に表示されない問題を修正しました。

<a id="442-2022-11-29-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.44.2](./release-notes-android/#2-44-2-2022-11-29)
* [Gamebase iOS SDK 2.44.0](./release-notes-ios/#2-44-0-2022-10-25)

<a id="2-44-0-2022-10-11"></a>
### 2.44.0 (2022. 10. 11.) { #2-44-0-2022-10-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Unity.zip)

<a id="440-2022-10-11-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：NHN Cloud Unity SDK(0.26.2)

<a id="440-2022-10-11-platform-specific-changes"></a>
#### プラットフォーム別変更事項
* [Gamebase Android SDK 2.44.0](./release-notes-android/#2-44-0-2022-10-11)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2-43-3-2022-10-04)

<a id="2-43-0-2022-09-07"></a>
### 2.43.0 (2022. 09. 07.) { #2-43-0-2022-09-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Unity.zip)

<a id="430-2022-09-07-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST Unity SDK(0.26.1)、Kakaogame Unity SDK(3.14.5)
* LINE Loginを行う時にサービスを提供するRegionを入力するように変更しました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Login with IdP](./unity-authentication/#login-with-idp)

<a id="430-2022-09-07-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2-43-0-2022-09-07)
* [Gamebase iOS SDK 2.43.0](./release-notes-ios/#2-43-0-2022-09-07)

<a id="2-42-1-2022-08-09"></a>
### 2.42.1 (2022. 08. 09.) { #2-42-1-2022-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unity.zip)

<a id="421-2022-08-09-added-features"></a>
#### 機能追加
* ForcingMappingTicketクラスにマッピングユーザー状態を表すmappedUserValidフィールドが追加されました。

<a id="421-2022-08-09-feature-updates"></a>
#### 機能改善・変更
* WebViewで固定フォントサイズを使用するかどうかを設定するフィールドは、今後は使用しません。
    * **GamebaseWebViewConfiguration.enableFixedFontSize**
* GamebaseWebViewConfigurationのデフォルト値が追加されました。
    * ナビゲーションバーの色相フィールドであるcolorR、colorG、colorB、colorAのデフォルト値が18、93、230、255に設定されました。
    * ナビゲーションバーの有効/無効を指定するフィールドであるisNavigationBarVisibleのデフォルト値がtrueに設定されました。
    * Webビュー内の戻るボタンの有効/無効を指定するフィールドであるisBackButtonVisibleのデフォルト値がtrueに設定されました。

<a id="421-2022-08-09-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2-42-1-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2-42-1-2022-08-09)

<a id="2-41-0-2022-07-05"></a>
### 2.41.0 (2022. 07. 05.) { #2-41-0-2022-07-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unity.zip)

<a id="410-2022-07-05-added-features"></a>
#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.6)
* GamebaseEventHandlerのGamebaseEventCategoryに**IDP_REVOKED**タイプが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unity-etc/#idp-revoked)

<a id="410-2022-07-05-feature-updates"></a>
#### 機能改善・変更
* UnityのBurstパッケージを使用するとき、メモリリークが発生する問題を修正しました。
* Setting Tool (v2.4.0)
    * 内部安定化指標が追加されました。
    * 既存SettingToolはUnityプロジェクトから完全に削除した後、最新バージョンを再インストールする必要があります。
    * SettingTool v1のサポートを終了します。

<a id="410-2022-07-05-bug-fixes"></a>
#### 不具合修正
* (iOS)特定環境で決済後にクラッシュが発生する問題を修正しました。

<a id="410-2022-07-05-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2-41-0-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2-41-0-2022-07-05)

<a id="2-40-0-2022-05-24"></a>
### 2.40.0 (2022. 05. 24.) { #2-40-0-2022-05-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unity.zip)

<a id="400-2022-05-24-added-features"></a>
#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.5)
* (Standalone)以下の約款APIをサポートするように変更しました。
    * Gamebase.Terms.QueryTerms
    * Gamebase.Terms.UpdateTerms

<a id="400-2022-05-24-feature-updates"></a>
#### 機能改善・変更
* ハングルがUnicodeで表示される現象が改善されました。
* (iOS) bitcodeをサポートするように修正しました。

<a id="400-2022-05-24-bug-fixes"></a>
#### 不具合修正
* (Android) OpenContact API呼び出し時にConfiguration.additionalParametersが適用されない問題が修正されました。

<a id="400-2022-05-24-platform-specific-changes"></a>
#### 各プラットフォームの変更事項
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2-40-0-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2-40-0-2022-05-24)

<a id="2-39-0-2022-05-10"></a>
### 2.39.0 (2022. 05. 10.) { #2-39-0-2022-05-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Unity.zip)

<a id="390-2022-05-10-added-features"></a>
#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.4)

<a id="390-2022-05-10-bug-fixes"></a>
#### 不具合修正
* 初期化前にGetLaunchingInformations() APIを呼び出したときにJsonExceptionが発生しないように修正しました。

<a id="390-2022-05-10-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.39.0](./release-notes-android/#2-39-0-2022-05-10)
* [Gamebase iOS SDK 2.39.0](./release-notes-ios/#2-39-0-2022-05-10)

<a id="2-38-0-2022-05-03"></a>
### 2.38.0 (2022. 05. 03.) { #2-38-0-2022-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Unity.zip)

<a id="380-2022-05-03-added-features"></a>
#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.3)

<a id="380-2022-05-03-feature-updates"></a>
#### 機能改善・変更
* Display Languageの中国語繁体字(zh-TW)言語セットで不自然な文章を修正しました。

<a id="380-2022-05-03-bug-fixes"></a>
#### 不具合修正
* (Android) API Level 24未満で特定API呼び出し時にエラーが発生しないように修正しました。
    * Gamebase.Purchase.RequestActivatedPurchases()
    * Gamebase.Purchase.RequestItemListOfNotConsumed()

<a id="380-2022-05-03-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.38.0](./release-notes-android/#2-38-0-2022-05-03)
* [Gamebase iOS SDK 2.38.0](./release-notes-ios/#2-38-0-2022-05-03)

<a id="2-37-0-2022-04-26"></a>
### 2.37.0 (2022. 04. 26.) { #2-37-0-2022-04-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Unity.zip)

<a id="370-2022-04-26-added-features"></a>
#### 機能追加
* サポートURLの後ろにパラメータを追加できるように次のフィールドが追加されました。
    * GamebaseRequest.Contact.Configuration.additionalParameters

<a id="370-2022-04-26-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.37.0](./release-notes-android/#2-37-0-2022-04-26)
* [Gamebase iOS SDK 2.37.0](./release-notes-ios/#2-37-0-2022-04-26)

<a id="2-36-0-2022-04-12"></a>
### 2.36.0 (2022. 04. 12.) { #2-36-0-2022-04-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Unity.zip)

<a id="360-2022-04-12-added-features"></a>
#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.2)
* 決済時にプロモーションかどうかを知ることができるisPromotionフィールドが追加されました。
    * GamebaseResponse.Purchase.PurchasableReceipt.isPromotion
* 決済時、テスト決済かどうかを知ることができるisTestPurchaseフィールドが追加されました。
    * GamebaseResponse.Purchase.PurchasableReceipt.isTestPurchase

<a id="360-2022-04-12-bug-fixes"></a>
#### 不具合修正
* デバイスが特定文化圏に設定されているとき、決済商品の価格情報が0と入力されるエラーが修正されました。
* (iOS) Push通知をクリックしたときにディープリンクが動作しないエラーが修正されました。
* (iOS)プロジェクトのorientationがAuto Rotationに設定されており、プロジェクトの最初のシーン(scene)に含まれているMonoBehaviourのAwakeでGamebase API呼び出し時にWebビューなどのUI出力が正常に行われないエラーが修正されました。

<a id="360-2022-04-12-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.36.0](./release-notes-android/#2-36-0-2022-04-12)
* [Gamebase iOS SDK 2.36.0](./release-notes-ios/#2-36-0-2022-04-12)

<a id="2-35-0-2022-03-29"></a>
### 2.35.0 (2022. 03. 29.) { #2-35-0-2022-03-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Unity.zip)

<a id="350-2022-03-29-added-features"></a>
#### 機能追加

* 外部SDKアップデート: TOAST Unity SDK(0.25.1)
* 約款が表示されているかどうかを知ることができるAPIが追加されました。
    * Gamebase.Terms.IsShowingTermsView()
* Webビューでナビゲーションバーを隠すことができるオプションが追加されました。
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) Webビューで文字サイズを固定することができるオプションが追加されました
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.enableFixedFontSize
* (Android)約款ウィンドウで文字サイズを固定することができるオプションが追加されました。
    * GamebaseRequest.Terms.GamebaseTermsConfiguration.enableFixedFontSize
* Setting Tool
    * (Android) Amazonストアが追加されました。
    * (Android) Huaweiストアが追加されました。

<a id="350-2022-03-29-platform-specific-changes"></a>
#### プラットフォーム別変更事項
* [Gamebase Android SDK 2.35.0](./release-notes-android/#2-35-0-2022-03-29)
* [Gamebase iOS SDK 2.35.0](./release-notes-ios/#2-35-0-2022-03-29)

<a id="2-34-1-2022-03-15"></a>
### 2.34.1 (2022. 03. 15.) { #2-34-1-2022-03-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-Unity.zip)

<a id="341-2022-03-15-added-features"></a>
#### 機能追加
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * Gamebase.Push.QueryNotificationAllowed

<a id="341-2022-03-15-bug-fixes"></a>
#### 不具合修正
* iOSでGamebaseWebViewConfigurationのisBackButtonVisible設定が適用されないエラーが修正されました。

<a id="341-2022-03-15-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2-34-0-2022-02-22)
* [Gamebase iOS SDK 2.34.1](./release-notes-ios/#2-34-1-2022-03-15)

<a id="2-34-0-2022-02-22"></a>
### 2.34.0 (2022. 02. 22.) { #2-34-0-2022-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Unity.zip)

<a id="340-2022-02-22-added-features"></a>
#### 機能追加
* 共通約款API呼び出し後、約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * **GamebaseResponse.Terms.ShowTermsViewResult**

<a id="340-2022-02-22-feature-updates"></a>
#### 機能改善・変更
* キックアウトポップアップの表示有無はGamebaseコンソールでキックアウト登録時に設定することができるため、次のフィールドがdeprecatedになりました。
    * **GamebaseConfiguration.enableKickoutPopup**

<a id="340-2022-02-22-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2-34-0-2022-02-22)
* [Gamebase iOS SDK 2.34.0](./release-notes-ios/#2-34-0-2022-02-22)

<a id="2-33-0-2022-01-25"></a>
### 2.33.0 (2022. 01. 25.) { #2-33-0-2022-01-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unity.zip)

<a id="330-20220125-added-features"></a>
#### 機能追加
* 共通約款ウィンドウの設定を変更できる新規APIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > UI > Terms > showTermsView](./unity-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### 機能改善・変更
* エラーコードの追加と変更
    * GamebaseErrorCode.UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode.SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。
    
<a id="330-20220125-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2-33-0-2022-01-25)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2-33-0-2022-01-25)

<a id="2-32-0-2021-12-28"></a>
### 2.32.0 (2021. 12. 28.) { #2-32-0-2021-12-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Unity.zip)

<a id="320-20211228-feature-updates"></a>
#### 機能改善・変更
* GamebaseEventHandlerのGamebaseEventCategoryに**GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED**タイプが追加されました。
    * このイベントの活用方法は、次の文書を参照してください。
    * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Server Push](./unity-etc/#server-push)
* Gamebase Access Tokenの有効期限が切れてログインが必要なときに動作する**GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler categoryが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unity-etc/#logged-out)

<a id="320-20211228-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.32.0](./release-notes-android/#2-32-0-2021-12-28)
* [Gamebase iOS SDK 2.32.0](./release-notes-ios/#2-32-0-2021-12-28)

<a id="2-31-0-2021-12-14"></a>
### 2.31.0 (2021. 12. 14.) { #2-31-0-2021-12-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Unity.zip)

<a id="310-20211214-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：TOAST Unity SDK(0.25.0)
* Standaloneメンテナンスポップアップでメンテナンス時間を表示するかどうかを動的に設定できるように変更しました。
* Setting Tool
    * PAYCO IDPが追加されました。
    * 既存のSettingToolを完全に削除した後、再インストールする必要があります。

<a id="310-20211214-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.31.0](./release-notes-android/#2-31-0-2021-12-14)
* [Gamebase iOS SDK 2.31.0](./release-notes-ios/#2-31-0-2021-12-14)

<a id="2-30-0-2021-11-23"></a>
### 2.30.0 (2021. 11. 23.) { #2-30-0-2021-11-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Unity.zip)

<a id="300-20211123-added-features"></a>
#### 機能追加
* 強制マッピングを行う時、IdPログインをもう一度試行しなければいけない煩わしさを改善した、新しい強制マッピングAPIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Mapping > Add Mapping Forcibly](./unity-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping()呼び出し後、AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)エラーが発生した時、該当アカウントでログインすることができるAPIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Mapping > Change Login with ForcingMappingTicket](./unity-authentication/#change-login-with-forcingmappingticket)

<a id="300-20211123-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.30.0](./release-notes-android/#2-30-0-2021-11-23)
* [Gamebase iOS SDK 2.30.0](./release-notes-ios/#2-30-0-2021-11-23)

<a id="2-29-0-2021-11-09"></a>
### 2.29.0 (2021. 11. 09.) { #2-29-0-2021-11-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Unity.zip)

<a id="290-20211109-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：TOAST Unity SDK(0.23.5)
* Setting Tool
    * v2.0.0が新しく配布されました。
    * 既存SettingToolを完全に削除して、再インストールする必要があります。
    * 変更された内容および使用方法は、以下のガイドを確認してください。
        * [Game > Gamebase > Unity SDK使用ガイド > 始める > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

<a id="290-20211109-bug-fixes"></a>
#### 不具合修正
* GamebaseDisplayLanguageCodeフィンランド語の誤字を修正
    * Finish → Finnish

<a id="290-20211109-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.29.0](./release-notes-android/#2-29-0-2021-11-09)
* [Gamebase iOS SDK 2.29.0](./release-notes-ios/#2-29-0-2021-11-09)

<a id="2-28-1-2021-10-26"></a>
### 2.28.1 (2021. 10. 26.) { #2-28-1-2021-10-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.1/GamebaseSDK-Unity.zip)

<a id="281-20211026-bug-fixes"></a>
#### 不具合修正
* (Android) DisplayLanguageを設定していない場合、誤った値に設定される問題が修正されました。
* (Standalone)前のフレームで時間がかかる場合に発生するTimeoutエラーが修正されました。

<a id="2-28-0-2021-09-28"></a>
### 2.28.0 (2021. 09. 28.) { #2-28-0-2021-09-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Unity.zip)

<a id="280-20210928-added-features"></a>
#### 機能追加
* Kakaogame認証追加
* 「決済アビューズ自動解除」機能が追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > GraceBan](./unity-authentication/#graceban)
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にAuthToken.member.graceBanInfo API値を確認し、nullではない有効なGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

<a id="280-20210928-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.28.0](./release-notes-android/#2-28-0-2021-09-28)
* [Gamebase iOS SDK 2.28.0](./release-notes-ios/#2-28-0-2021-09-28)

<a id="2-27-1-2021-09-14"></a>
### 2.27.1 (2021. 09. 14.) { #2-27-1-2021-09-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Unity.zip)

<a id="271-20210914-feature-updates"></a>
#### 機能改善・変更
* Display Language機能が改善されました。
    * 基本言語コードが**en**でしたが、Gamebaseコンソールで設定した基本言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](./oper-app/#language-settings)

<a id="271-20210914-bug-fixes"></a>
#### 不具合修正
* 「登録されていないゲームバージョン」エラーポップアップが英語でのみ表示されるバグを修正しました。
* メンテナンスポップアップに中国語が表示されないバグを修正しました。

<a id="271-20210914-platform-specific-changes"></a>
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.27.1](./release-notes-android/#2-27-1-2021-09-14)
* [Gamebase iOS SDK 2.27.1](./release-notes-ios/#2-27-1-2021-09-14)

<a id="2-27-0-2021-08-24"></a>
### 2.27.0 (2021. 08. 24.) { #2-27-0-2021-08-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Unity.zip)

<a id="270-20210824-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：TOAST Android SDK(0.23.2)
* ONE Store V16ストア追加

<a id="270-20210824-bug-fixes"></a>
#### 不具合修正
* Unity SDK 2.25.0で誤って追加されたファイルを削除
    * パス：Assets/Gamebase/Toast/IAP/Plugins

<a id="2-26-0-2021-08-10"></a>
### 2.26.0 (2021. 08. 10.) { #2-26-0-2021-08-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unity.zip)

<a id="260-20210810-feature-updates"></a>
#### 機能改善・変更
* Display Language機能が改善されました。
    * Display Language言語セットに中国語簡体字(zh-CN)、中国語繁体字(zh-TW)、タイ語(th)が追加されました。
* showTermsView API呼び出し後に作成することができるPushConfigurationオブジェクトの作成基準が次のように変更されました。
    * 変更前
        * 約款項目中に**Push受信**項目が存在する場合にのみnullではなく有効なPushConfigurationが返されました。
        * ユーザーが昼間、夜間広告性Push受信に全て拒否した場合、PushConfiguration.pushEnabledはfalseで作成されました。
    * 変更後
        * 約款UIが表示されている場合、常にnullではない有効なPushConfigurationが返されます。
        * showTermsViewが返すPushConfigurationオブジェクトのpushEnabled値は常にtrueです。
    * 変更されない点
        * すでに約款に同意して約款UIが表示されない場合はPushConfigurationはnullで返されます。

<a id="260-20210810-bug-fixes"></a>
#### 不具合修正
* Push言語設定は特別な補助処理なしで端末の言語コードがそのまま適用され、Pushコンソールから送信したメッセージの言語コードが一致しない問題を修正しました。


<a id="2-25-0-2021-07-26"></a>
### 2.25.0 (2021. 07. 26.) { #2-25-0-2021-07-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Unity.zip)

<a id="250-20210726-added-features"></a>
#### 機能追加
* 月決済限度機能を追加
    * 月決済限度を超える場合、**PURCHASE_LIMIT_EXCEEDED(4007)**エラーが発生します。

<a id="250-20210726-feature-updates"></a>
#### 機能改善・変更
* Push項目が存在する約款でPushConfigurationオブジェクト保障
    * 約款UIでPush受信に同意しない場合、Gamebase.Terms.ShowTermsView API呼び出し結果として作成されるPushConfigurationがnullでしたが、約款にPush項目が存在すればPushConfigurationオブジェクトが常に返されるように変更しました。
    * Push受信を拒否した時、PushConfigurationオブジェクトは(プッシュ同意有無= false、広告性プッシュ同意有無= false、夜間広告性プッシュ同意有無= false)で作成されます。
    * 約款にPush項目が存在しない場合、PushConfigurationオブジェクトはnullです。
* Unity最小サポートバージョン変更：2018.4.0f1
* 外部SDKアップデート: TOAST Unity SDK(0.23.0)

<a id="2-24-0-2021-06-29"></a>
### 2.24.0 (2021. 06. 29.) { #2-24-0-2021-06-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-1-feature-updates"></a>
#### 機能改善・変更
* 内部ローンチURL変更

<a id="2-23-0-2021-06-14"></a>
### 2.23.0 (2021. 06. 14.) { #2-23-0-2021-06-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-2-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST Unity SDK(0.22.1)
* Unity 2020.2以降のバージョンで発生するWarningを除去
* StandaloneとUnity Editorで初期化速度を改善

<a id="game-gamebase-release-notes-unity-2-bug-fixes"></a>
#### 不具合修正
* 約款同意を行ってもShowTermsView API呼び出すとPushConfiguration結果がnullではない問題を修正

<a id="2-22-0-2021-05-25"></a>
### 2.22.0 (2021. 05. 25.) { #2-22-0-2021-05-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-3-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST Unity SDK(0.22.0)

<a id="2-21-0-2021-04-13"></a>
### 2.21.0 (2021. 04. 13.) { #2-21-0-2021-04-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-4-more-features"></a>
#### 機能追加
* Hangame日本認証を追加

<a id="2-20-0-2021-02-09"></a>
### 2.20.0 (2021. 02. 09.) { #2-20-0-2021-02-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-5-more-features"></a>
#### 機能追加
* 共通約款機能追加
	* 約款Webビューを開くAPIを追加
	* 約款リストおよびユーザーごとに同意の有無を照会するAPIを追加
	* ユーザーごとに約款の同意有無をGamebaseサーバーに保存するAPIを追加

<a id="game-gamebase-release-notes-unity-5-feature-updates"></a>
#### 機能改善・変更
* サポートタイプがTOAST組織商品(Online Contact)の場合、ログインしなくてもサポートが表示されるように変更
* Warningログ削除
* Standalone WebViewにCEF 2.1.2アップデート
	* URLの長さが2,048を超えるとクラッシュが発生するイシューを修正
	* Unity 2019でビルドした時、ライブラリパスが変更されてPostProcessBuild改善
	* stringの初期化を行っていないことでエラーが発生する問題を修正
	* Gamebase WebView使用中にシーン(scene)を移動した後、WebViewが開けなくなるバグを修正

<a id="2-19-0-2020-12-29"></a>
### 2.19.0 (2020. 12. 29.) { #2-19-0-2020-12-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unity.zip)

<a id="190-december-29-2020-more-features"></a>
#### 機能追加
* [SDK] 2.19.0
	* (共通) Weibo認証を追加
	
<a id="190-december-29-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.19.0
	* (共通)ローンチステータスコード追加：ベータサービス(205)

<a id="190-december-29-2020-bug-fixes"></a>
#### 不具合修正 
* [SDK] 2.19.0
    * (Unity) WebSocketで再試行した時、 OutOfMemoryExceptionが発生する問題を修正

<a id="2-18-2-2020-12-15"></a>
### 2.18.2 (2020. 12. 15.) { #2-18-2-2020-12-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Unity.zip)

<a id="182-december-15-2020-more-features"></a>
#### 機能追加
* [SDK] 2.18.2
	* (共通)開発会社が独自のサポートをオープンする時、additionalURLフィールドを追加
	* (共通)決済アイテム情報にローカライズされた商品情報を追加：localizedTitle, localizedDescription

<a id="182-december-15-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.18.2
    * (共通) TOAST SDKアップデート: [Android(0.24.2)](https://docs.toast.com/ja/TOAST/ja/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ja/TOAST/ja/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ja/TOAST/ja/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android)暗号化ロジックセキュリティ警告を解決するための外部SDKアップデート：PAYCO Login SDK(1.5.3), Hangame ID SDK(1.3.2)
	* (Android) Tencent Pushモジュール削除
	* (Android) Gamebase Android SDK 2.6.0でdeprecatedされた関数を削除
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView：無効なURLを伝達した場合、エラーを返されたURLはエンコードせず、そのまま使用
	* (iOS)大文字/小文字に関係なく、カスタムスキームが動作するように変更
	* (Unity) GamebaseRequest.GamebaseConfigurationクラスのフィールドdeprecated: zoneType, fcmSenderId

<a id="182-december-15-2020-bug-fixes"></a>
#### 不具合修正 
* [SDK] 2.18.2
    * (Android) 5.0～6.0 OS端末でWebビューカスタムスキームが動作しない問題を修正

<a id="2-18-0-2020-11-10"></a>
### 2.18.0 (2020. 11. 10.) { #2-18-0-2020-11-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-Unity.zip)

<a id="180-november-10-2020-more-features"></a>
#### 機能追加
* Galaxyストア追加：SDK 2.18.0

<a id="180-november-10-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.18.0
    * (Android) TOAST SDKアップデート：[Android(0.24.1)](https://docs.toast.com/ja/TOAST/ja/toast-sdk/release-notes-android/#0240-20201027)-GooglePlay Billing Library v.3.0.1 適用
    * (Android) WebView SSLセキュリティ警告対応処理を追加
    * (iOS) iOS 13以上から提供されるSceneDelegate対応APIを追加

<a id="180-november-10-2020-bug-fixes"></a>
#### 不具合修正 
* [SDK] 2.18.1
    * (Android) 2.18.0でGoogle決済後にクラッシュが発生するイシューを修正

<a id="2-17-1-2020-10-27"></a>
### 2.17.1 (2020. 10. 27.) { #2-17-1-2020-10-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Unity.zip)

<a id="171-october-27-2020-more-features"></a>
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

<a id="171-october-27-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.17.1
    * (Unity) Unity 2017.2.5サポート

<a id="171-october-27-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.17.1
    * (Unity)イメージ告知とWebビューを順番に呼び出すと、後で呼び出したAPIが動作しないエラーを修正	
	
<a id="2-17-0-2020-10-13"></a>
### 2.17.0 (2020. 10. 13.) { #2-17-0-2020-10-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-Unity.zip)

```
ハンゲーム認証を使用したい場合はサポートへご連絡ください。
```

<a id="170-october-13-2020-more-features"></a>
#### 機能追加
* Hangame IdP認証追加：SDK 2.17.0

<a id="170-october-13-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.17.0
	* (共通)サポート添付イメージクリック時、ダウンロードサポート
	* (共通) TOAST SDKアップデート：Android(0.23.2), Unity(0.21.2)
	* (iOS) TCGBMember.regDate、TCGBMember.lastLoginDateのタイプをlong longに変更
	* (iOS) WebビューからURLおよびタイトルを変更した時、タイトルを再出力できるようにロジックを変更

<a id="170-october-13-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.17.0
	* (iOS) PAYCO認証：lastLoggedInProviderログイン後、ログアウトを呼び出した時、ログアウトコールバックが来ない問題を修正
* [SDK] 2.17.1
	* (Android) 2.17.0でImageNotice APIを呼び出した時、kotlinx-coroutineモジュールでクラッシュが発生する問題を修正

<a id="2-16-0-2020-09-22"></a>
### 2.16.0 (2020. 09. 22.) { #2-16-0-2020-09-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Unity.zip)

<a id="160-september-22-2020-more-features"></a>
#### 機能追加
* サポート機能追加
	* [SDK] 2.16.0
		* (共通) API追加(Gamebase.Contact.requestContactURL)：サポートURLリターン
		* (共通)サポートAPIにuserNameを設定できるようにContactConfigurationパラメータを追加 
		
<a id="2-15-0-2020-08-25"></a>
### 2.15.0 (2020. 08. 25.) { #2-15-0-2020-08-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unity.zip)

```
Gamebase SDK 2.15.0バージョンでGoogle Billing Clientモジュールがアップデートされました。

gamebase-adapter-purchase-googleを使用する場合、Gamebase SDK 2.15.0未満バージョンから2.15.0以上にアップグレードするには
前バージョンの「Game Client Version」を「アップデート必須」に設定する必要があります。

アイテムを購入中にエラーが発生すると再処理を行いますが
複数の端末で異なるBilling Clientバージョンが適用された状態では再処理中に問題が発生することがあるためです。
```

<a id="150-august-25-2020-more-features"></a>
#### 機能追加
* [SDK] 2.15.0
    * (共通)プッシュトークン登録時に、アプリのNotificationOption設定がForeground状態でもプッシュ通知を受け取れるように機能追加
    * (共通)プッシュAPI追加：Pushトークン情報確認(Gamebase.Push.queryTokenInfo API)

<a id="150-august-25-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.15.0
    * (共通) TOAST SDKアップデート: Android(0.23.0)、iOS(0.26.0)、Unity(0.21.0)
    * (iOS)決済payloadのnull checkロジック追加

<a id="2-14-0-2020-08-11"></a>
### 2.14.0 (2020. 08. 11.) { #2-14-0-2020-08-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-Unity.zip)

<a id="140-august-11-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.14.0
    * (iOS) PAYCO IdPの定数値を削除：PAYCO文字列によるApple検収がリジェクトされる場合があり削除
    * (iOS、Unity) TCGBWebViewConfigurationにcontentMode設定を追加

<a id="2-13-0-2020-07-28"></a>
### 2.13.0 (2020. 07. 28.) { #2-13-0-2020-07-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Unity.zip)

<a id="130-july-28-2020-more-features"></a>
#### 機能追加
* [SDK] 2.13.0
    * (Unity) Standalone:イメージ告知表示APIを追加   

<a id="130-july-28-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.13.0
    * (Android)イメージ告知のポップアップイメージ比率計算ロジックを修正
    * (iOS) Sign In With Apple認証：iOS 12以下をサポート

<a id="130-july-28-2020-bug-fixes"></a>
#### バグ修正

<!-- TODO: translate body -->

<a id="2-12-0-2020-07-14"></a>
### 2.12.0 (2020. 07. 14.) { #2-12-0-2020-07-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Unity.zip)

<a id="120-july-14-2020-more-features"></a>
#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * [SDK] 2.12.0：イメージ告知表示APIを追加

<a id="120-july-14-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.12.0
    * (iOS)Facebook SDKアップデート(7.1.1)
    * (iOS)configuartionに設定されたstoreCode(default=AS)でGamebaseの初期化を試行
    * (iOS)コンテンツをローディングできないWebビューを出力時、閉じるボタンがなくて閉じられない問題を修正
    * (Unity)TOAST Unity SDKアップデート(0.20.1.1)
    
<a id="2-11-0-2020-06-23"></a>
### 2.11.0 (2020. 06. 23.) { #2-11-0-2020-06-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Unity.zip)

<a id="110-june-23-2020-more-features"></a>
#### 機能追加
* [SDK] 2.11.0
	* 決済API追加：商品IDで決済リクエスト, 追加情報(UserPayload)を入力して決済完了時に確認できる

<a id="2-10-1-2020-06-09"></a>
### 2.10.1 (2020. 06. 09.) { #2-10-1-2020-06-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-Unity.zip)

<a id="101-june-9-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.10.1
	* (iOS)ユーザープッシュ設定の初期化時、言語コードが設定されていない場合、デバイス言語で設定されるように変更

<a id="101-june-9-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.10.1
	* (Unity) iOS Pluginで、ViewControllerが設定されておらず、ログイン呼び出し時に失敗する問題を修正

<a id="2-10-0-2020-05-26"></a>
### 2.10.0 (2020. 05. 26.) { #2-10-0-2020-05-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Unity.zip)

<a id="100-may-26-2020-more-features"></a>
#### 機能追加
* [SDK] 2.10.0
	* (共通)既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
		* ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

<a id="100-may-26-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.10.0 
	* (Unity) StandaloneWebviewAdapter内部のCefWebviewバージョンアップデート：v2.0.4
		* WebviewIndex検証ロジックを改善
		* Webview作成時、断続的にNullReferenceExceptionが発生するエラーを修正

<a id="100-may-26-2020-bug-fixes"></a>
#### バグ修正

<!-- TODO: translate body -->

<a id="2-9-1-2020-04-29"></a>
### 2.9.1 (2020. 04. 29.) { #2-9-1-2020-04-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unity.zip)

<a id="91-april-29-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.9.1 
	* (Unity) Initialize後、コンソールでクライアントのサービスの状態を変更するとエラーが発生する問題を修正

<a id="2-9-0-2020-04-28"></a>
### 2.9.0 (2020. 04. 28.) { #2-9-0-2020-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unity.zip)

<a id="90-april-28-2020-more-features"></a>
#### 機能追加
* 退会猶予機能
	* [SDK] 2.9.0
		*(共通)API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認
	
<a id="90-april-28-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.9.0
	* (共通) TOAST SDKアップデート： Android(v0.21.0)、iOS(v0.23.0)、Unity(0.20.1)
	* (共通) PAYCO Login SDKアップデート： Android(v1.5.0)、iOS(v1.4.0)
	
<a id="2-8-1-2020-04-14"></a>
### 2.8.1 (2020. 04. 14.) { #2-8-1-2020-04-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Unity.zip)

<a id="81-april-14-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.8.1 
	* (共通) Analytics転送結果を確認するための内部指標を追加
	
<a id="2-8-0-2020-03-24"></a>
### 2.8.0 (2020. 03. 24.) { #2-8-0-2020-03-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Unity.zip)

<a id="80-march-24-2020-more-features"></a>
#### 機能追加
* [SDK] 2.8.0
	* (共通)決済および商品情報に商品タイプおよび地域価格などの情報を追加
	* (Unity) StandaloneWebviewAdapter内部のCefWebviewがv2.0.1バージョンにアップデート
		* PopupTypeがPASS_INFOの場合、ポップアップを表示させずにポップアップ情報を伝達する機能を追加

<a id="80-march-24-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.8.0 
	* (共通)コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップが表示されるように改善
	* (Android)ログイン直後に決済関連APIを呼び出す時、初期化タイミングの問題で失敗する場合があるコードを修正

		
<a id="2-7-2-2020-03-10"></a>
### 2.7.2 (2020. 03. 10.) { #2-7-2-2020-03-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Unity.zip)

<a id="72-march-10-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.7.2 
  	* (Unity) FacebookAdapter改善 
    		* v7.9.4～v7.18.1バージョンまで互換性テスト
    		* Null例外処理 
  	* (Unity) StandaloneWebviewAdapterを改善 
    		* Webページをテスクチャ(texture)でエクスポート追加
    		* マルチWebビューをサポート 
    		* Cookie削除オプションを追加 
    		* テクスチャ(texture)のサイズ調節をサポート 
		* スクロールバー表示/非表示をサポート 
    		* ページロード完了通知 
    		* 透明背景をサポート 
  	* (Unity)エディタでAndroid/iOSプラットフォームを選択してInitialize APIを呼び出すとエラーが発生する問題を修正

<a id="2-7-0-2020-01-21"></a>
### 2.7.0 (2020. 01. 21.) { #2-7-0-2020-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Unity.zip)

<a id="70-january-21-2020-more-features"></a>
#### 機能追加
* [SDK] 2.7.0
	* (Unity) NaverCafePLUGサポート

<a id="70-january-21-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.7.0
	* (Android)サーバーレスポンス(response)でtraceError必須パラメータがなくてもクラッシュが発生しないように修正
	* (Android) Firebaseの設定が行われていない時、例外が発生しないように修正
	* (Unity) Web Login時、 gamebase://dismissスキーム処理を追加
	* (Unity)リリースビルド時、Webviewが表示されない問題を修正	

<a id="2-6-3-2020-01-14"></a>
### 2.6.3 (2020. 01. 14.) { #2-6-3-2020-01-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.3/GamebaseSDK-Unity.zip)

<a id="63-january-14-2020-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.6.3
	* (Unity) Standalone Webview改善：CefWebviewアップデート	
	* (Unity)ログイン後、エラーが発生していたため、抜けていた.dllファイルを追加
		* ToastCommon.dll、vcruntime140.dll

<a id="63-january-14-2020-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.6.3
	* (Unity) Login(CredentialInfo) API呼び出し時にエラーが発生していた問題を修正
	
<a id="2-6-2-2019-12-24"></a>
### 2.6.2 (2019. 12. 24.) { #2-6-2-2019-12-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Unity.zip)

<a id="62-december-24-2019-more-features"></a>
#### 機能追加
* クーポン > クーポン発行：キーワードクーポン機能を追加

<a id="62-december-24-2019-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.6.2
	* (共通) TOAST SDKアップデート: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) NAVER SDKバージョンをアップデート(4.1.0)

<a id="2-6-1-2019-11-20"></a>
### 2.6.1 (2019. 11. 20.) { #2-6-1-2019-11-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Unity.zip)

<a id="61-november-20-2019-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.6.1
	* (Unity)iOS PluginファイルがPackageに入っておらず、iOSビルド時にエラーが発生するため、該当ファイルを追加：'toast_sdk_wrap.m'
	* (Unity)UnityEditorでStandalone以外のプラットフォームで実行時、Store CodeがEmptyになり初期化に失敗するエラーを修正
	* (Unity)Initialize API内部zone type処理部分でのエラーで、NullReferenceExceptionが発生するエラーを修正

<a id="november-13-2019"></a>
### 2019. 11. 13. { #november-13-2019 }

<a id="november-13-2019-bug-fixes"></a>
#### 不具合修正
* GamebaseSettingTool
	* Gamebase v2.6.0アップデートの際、ファイルが正常に変更されないエラーを修正

<a id="2-6-0-2019-11-12"></a>
### 2.6.0 (2019. 11. 12.) { #2-6-0-2019-11-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Unity.zip)

```
Gamebase SDK 2.6.0未満バージョンから2.6.0にアップグレードする場合
必ずUpgrade Guide文書に記載された変更事項を反映してください。 
ガイドの場所：Game > Gamebase > Upgrade Guide
```

<a id="60-november-12-2019-more-features"></a>
#### 機能追加
* Google定期購入決済機能を追加
	* [SDK] 2.6.0 Android
* [SDK] 2.6.0
	* (共通)データをLog&Crashに転送して、各種分析に利用できるようにTOAST Loggerを追加
	* (iOS) Sign In with Apple認証を追加
	* (Android) Gamebase Android SDKがBintrayを通して配布されるため、gradle設定だけでGamebaseを使用可能

<a id="60-november-12-2019-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.6.0
	* (Unity)ログイン時にLaunchingStatusを更新するロジックにエラーがあったため修正
	* (Unity) Debug Logを転送する機能をGamebaseコンソールで設定する場合、Clientからログ転送を無限に繰り返すエラーを修正

<a id="october-15-2019"></a>
### 2019. 10. 15. { #october-15-2019 }
<a id="october-15-2019-feature-updates"></a>
#### 機能改善・変更
* Sample App
	* Gamebase SDKアップデート(v2.4.0)
	* Smart Downloader適用(v1.5.8)、Leaderboard適用
	* 機能追加：ゲームリソースのダウンロード、 Leaderboard、TAA指標連携、端末移行機能、強制マッピング機能
	* 改善/変更：ServerPushリスナーを追加、 Observerメンテナンス中かどうかの検知を追加
	* ゲームリニューアル

<a id="2-5-0-2019-08-27"></a>
### 2.5.0 (2019. 08. 27.) { #2-5-0-2019-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Unity.zip)

<a id="50-august-27-2019-more-features"></a>
#### 機能追加
* [SDK] 2.5.0
	* Consoleで入力したCS URLをWebビューで開くAPIを提供

<a id="august-2-2019"></a>
### 2019.08.02. { #august-2-2019 }
<a id="august-2-2019-bug-fixes"></a>
#### 不具合修正
* [SDK] Setting Tool 1.4.3
	* Scriptファイルの位置をEditorフォルダの下に移動してビルドエラーを解決
	* Mac OSでMultilanguageにLanguageファイルの全体パスを与えると動作しない問題を修正

<a id="2-4-4-2019-07-23"></a>
### 2.4.4 (2019. 07. 23.) { #2-4-4-2019-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Unity.zip)

<a id="44-july-23-2019-feature-updates"></a>
#### 機能改善・変更
* [SDK] 2.4.4
	* (共通)会員エラーコードフォーマットを変更
	* (Unity)GamebaseServerPushTypeにKeyを追加(TRANSFER_KICKOUT)
* Setting Tool
	* フォルダ構造変更：`既存SettingToolを完全に削除した後、再インストールする必要があります。`
	* 多言語サポートを追加

<a id="2-4-3-2019-07-11"></a>
### 2.4.3 (2019. 07. 11.) { #2-4-3-2019-07-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-Unity.zip)

<a id="43-july-11-2019-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.4.3
	* (Unity)iOSとAndroidでビルド時、AddMappingForcibly APIが動作しないエラーを修正
	* (Unity)RequestRetryTransaction APIの呼び出し時、iOSPluginでJSON解析エラーを修正
	
<a id="june-272019"></a>
### 2019. 06. 27. { #june-272019 }

<a id="june-272019-bug-fixes"></a>
#### バグ修正
* [SDK] Setting Tool 1.4.1
	* GamebaseSettingTool実行時、既存設定情報を取得できないエラーを修正

<a id="2-4-2-2019-06-25"></a>
### 2.4.2 (2019. 06. 25.) { #2-4-2-2019-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Unity.zip)

<a id="42-june-25-2019-features-updateschanges"></a>
#### 機能改善・変更
* [SDK] 2.4.2
	* (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加

<a id="42-june-25-2019-bug-fixes"></a>
#### 不具合修正
* [SDK] 2.4.2
	* (共通)Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正

<a id="2-4-0-2019-05-28"></a>
### 2.4.0 (2019. 05. 28.) { #2-4-0-2019-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Unity.zip)

<a id="40-may-28-2019-feature-updates"></a>
#### 機能追加
* HANGAME mix日本決済追加
    * [SDK] 2.4.0
    	* (Unity)Standalone日本外部決済追加
    	* (Unity)Standalone日本HANGAME認証追加

<a id="40-may-28-2019-feature-updateschanges"></a>
#### 機能改善・変更
* [SDK] 2.4.0
	* (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]
    * (Android)NAVER SDKバージョンアップデート(v4.2.5)：NAVER SDKのバグを修正(NAVERログイン中にアプリアイコンからアプリを再起動した場合、Activityが強制終了する問題により、認証プロセスが中断される問題を解決)
    * (Unity)StandaloneWebviewが32bit Buildをサポート(SDK容量53.6MBから99.2MBに増加)

<a id="2-3-0-2019-04-23"></a>
### 2.3.0 (2019. 04. 23.) { #2-3-0-2019-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Unity.zip)

```
Gamebaseを使用すると、10数個の中国ストアと連携が可能です。
中国でのリリースに関心がある方は、サポートへご連絡ください。
```

<a id="30-20190423-1"></a>
#### 機能追加
* [SDK] 2.3.0
	* (Android/Unity)中国ストア認証/決済追加

<a id="30-20190423-2"></a>
#### 機能改善・変更
* [SDK] 2.3.0
	* (共通)Launching Status Code追加："審査中(204)"、"テスト中(203)"

<a id="2-2-2-2019-04-11"></a>
### 2.2.2 (2019. 04. 11.) { #2-2-2-2019-04-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Unity.zip)

<a id="22-20190411-1"></a>
#### 機能改善・変更
* [SDK] 2.2.2
	* (Unity)SDKログ改善

<a id="22-20190411-2"></a>
#### 不具合修正
* [SDK] 2.2.2
	* (Unity)AddMappingForcibly APIを呼び出すとクラッシュする問題を修正

<a id="2-2-1-2019-04-02"></a>
### 2.2.1 (2019. 04. 02.) { #2-2-1-2019-04-02 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.1/GamebaseSDK-Unity.zip)
<a id="21-20190402-1"></a>
#### バグ修正
* [SDK] 2.2.1
	* (Unity) Unity EditorでAndroidプラットフォームを選択してプレイすると、initializeの時にサーバーでエラーが発生する問題を修正

<a id="2-2-0-2019-03-26"></a>
### 2.2.0 (2019. 03. 26.) { #2-2-0-2019-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Unity.zip)
<a id="20-20190326-1"></a>
#### 機能追加
* TransferAccount機能追加：ゲストユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行できる機能
    * (SDK共通)追加されたAPI 
		* TransferAccountInfo発行API (issueTransferAccount)
		* 発行されたTransferAccountInfoを使用して、アカウント移行をリクエストするAPI (transferAccountWithIdPLogin)
		* 発行されたTransferAccountInfoを確認するAPI (queryTransferAccount)
		* すでに発行されたTransferAccountInfoを更新するAPI (renewTransferAccount
* 強制マッピング機能を追加：すでに他のアカウントに連携しているIdPアカウントをマッピングできる機能
	* (SDK共通)追加されたAPI 
		* 強制マッピングするAPI (addMappingForcibly)

<a id="20-20190326-2"></a>
#### 機能改善・変更
* [SDK] 2.2.0
	* (Android)IAP SDKバージョンを最新バージョンであるv1.5.3バージョンにアップデート
	* (iOS)LINE SDKのAppログイン機能が無効化
		* LINE SDK v4の問題により、iOS 12でアプリログインが失敗する問題があり、Gamebase Line AdatperでWebログインのみサポートするように変更
	* (Unity)GamebaseMainActivityのPackage Nameが変更
		* com.toast.gamebase.activity.GamebaseMainActivity → com.toast.android.gamebase.activity.GamebaseMainActivity

<a id="2-1-0-2019-02-26"></a>
### 2.1.0 (2019. 02. 26.) { #2-1-0-2019-02-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Unity.zip)
<a id="10-20190226-1"></a>
#### 機能改善・変更
* [SDK] 2.1.0
	* (共通)TransferKey API削除
		* issueTransferKey：TransferKey発行
		* requestTransfer：TransferKey検証

<a id="2-0-0-2019-01-29"></a>
### 2.0.0 (2019. 01. 29.) { #2-0-0-2019-01-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Unity.zip)
```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

<a id="00-20190129-1"></a>
#### 機能追加
* [SDK] 2.0.0
	* (共通)Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
		* setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
		* traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す

<a id="1-14-2-2018-11-15"></a>
### 1.14.2 (2018. 11. 15.) { #1-14-2-2018-11-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Unity.zip)
<a id="142-20181115-1"></a>
#### 機能改善・変更
* [SDK] 1.14.2
	* (Android)メンテナンス時、データ構造でメンテナンス開始/終了時間を意味するepoch timeのタイプをStringからlongにタイプ変更：既存Gamebase Unityと連携後、メンテナンス呼び出し時にタイプ不一致でコールバックが来ない現象を修正
	* (iOS)Provider Profile取得メソッドを呼び出した時に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にcrashが発生することがある構造を修正

<a id="142-20181115-2"></a>
#### 不具合修正
* [SDK] 1.14.2
	* (Android)エミュレータ環境でストアアプリ(PlayStore、OneStoreなど)がない状態で、"アプリインストール/アップデート"時にストア未チェックによるcrashする問題を修正
	* (Unity)ShowWebView APIを呼び出した時、パラメータにCallbackを入れない場合、crashが発生する部分を修正
	* (Unity)iOS SDKのDeleted APIを呼び出すコードがあり、コンパイル時にエラーが発生する問題を修正
	
<a id="1-14-0-2018-10-23"></a>
### 1.14.0 (2018. 10. 23.) { #1-14-0-2018-10-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-Unity.zip)

<a id="140-20181023-1"></a>
#### 機能追加
* [SDK] 1.14.0
	* (共通)Gamebase Webviewでファイル添付機能を追加：AndroidのAPI 19、Kitcatでは正常に動作しません。

<a id="140-20181023-2"></a>
#### 機能改善・変更
* [SDK] 1.14.0
	* (共通)利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして伝送し、クライアントでデコードして処理するように修正
	* (iOS)PAYCO SDKのバージョンを1.2.4にアップデート
	* (Unity)GamebaseSDKSettingオブジェクトがあるシーンに戻る場合、オブジェクトが重複して生成されないように改善
	* Remove API：Webview、Network、Launching
		* ShowWebBrowser(string url)
		* ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
		* ShowToast(string message, int duration)
		* AddUpdateStatusListener(GamebaseCallback.DataDelegate< GamebaseResponse.Launching.LaunchingStatus> callback)
		* RemoveUpdateStatusListener(GamebaseCallback.DataDelegate< GamebaseResponse.Launching.LaunchingStatus> callback)
		* AddOnChangedStatusListener(GamebaseCallback.DataDelegate< GamebaseNetworkType> callback)
		* RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate< GamebaseNetworkType> callback)
	* Deprecated API 
		* GetLanguageCode()
* [SDK] Setting Tool
	* ポップアップおよびUI改善
	

<a id="1-13-0-2018-09-13"></a>
### 1.13.0 (2018. 09. 13.) { #1-13-0-2018-09-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Unity.zip)

<a id="130-20180913-1"></a>
#### 機能改善・変更
* [SDK] 1.13.0
	* (共通)IAP SDK最新バージョン適用(android:1.5.1、iOS:1.6.0)
	* (Unity)ログで表示するjsonデータの出力フォーマットを改善
	
<a id="130-20180913-2"></a>
#### 不具合修正
* [SDK] 1.13.0
	* (Android)NaverCafe SDKとの衝突で、NAVERログイン時に発生するエラーを解決
	* (Unity)Unity 2017.2以上のバージョンでEditor Play Mode終了時、websocke close処理で発生するエラーを修正

<a id="1-12-1-2018-08-09"></a>
### 1.12.1 (2018. 08. 09.) { #1-12-1-2018-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-Unity.zip)

<a id="121-20180809-1"></a>
#### 機能改善・変更
* [SDK] 1.12.1
	* (共通)IAP SDK最新バージョン適用(1.5.0)
	* (共通)Gamebaseメンテナンスページで、メンテナンス時間を端末設定国時間に合わせて表示するように改善
	* (共通)メンテナンスページを外部ページで使用する時、Consoleに入力したメンテナンス情報を使用できる機能を追加
	* (共通)IdPマッピングされたユーザーがゲストマッピング試行時、エラー発生(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
	* (共通)認証APIを重複して呼び出した時、エラー発生(AUTH_ALREADY_IN_PROGRESS_ERROR)
	* (Android)TencentPush SDKアップデート(3.2.3)
	* (Android)Onestore v17(API v5)サポート：Gamebaseではv16(ストアコード=TS)は提供しません。
	* (iOS)エラーコード追加：Gamecenterログイン拒否(TCGB_ERROR_IOS_GAMECENTER_DENIED)
* [SDK] Setting Tool
	* フォルダ名変更：TOAST → Toast
	* エラー発生時、ポップアップ通知追加：File Download失敗、File Extract失敗、XML解析失敗
	
	
<a id="1-12-0-2018-07-24"></a>
### 1.12.0 (2018. 07. 24.) { #1-12-0-2018-07-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-Unity.zip)

<a id="120-20180724-1"></a>
#### 機能改善・変更
* [SDK] Setting Tool
	* Setting Toolの最新バージョンがある場合、アップデート通知機能を追加
	* 内部null Exception修正
	
<a id="120-20180724-2"></a>
#### 不具合修正
* [SDK] 1.12.0
	* (Unity)IssueTransferKey APIを呼び出した時、exceptionが発生する問題を修正
	* (Unity)Unity Google Adapter削除：GoogleAdapterを使用中の場合は、下記のアップデートガイドを参照
	

**Unity Google Adapterアップデートガイド**

* Unity SDK 1.6.0以上1.11.0以上のバージョンを使用する場合、1.12.0バージョンにアップデートする前に下記の内容を熟知する必要があります。(1.6.0未満のバージョンを使用中の場合は、GoogleAdapterを使用しないため影響がありません)
	1. Setting Tool設定
        * GoogleAdapterの削除に伴い、今後UnityタブでGoogle項目が表示されない。
        * Google認証を使用する場合は、各プラットフォームタブでGoogle項目を有効にする。
            * Android > Authentication > Googleを選択して設定
            * iOS > Authentication > Googleを選択して設定
    2. Gamebase Login API (変更なし)
        * Gamebase.Login(GamebaseAuthProvider.GOOGLE, callback);
    3. GPGS機能を使用する場合
        * GPGS SDK for Unity維持
        * GPGS関連ロジックはアプリで別途管理
    4. GPGS機能を使用しない場合
        * GPGS SDK for Unityを削除

<a id="1-11-0-2018-06-26"></a>
### 1.11.0 (2018. 06. 26.) { #1-11-0-2018-06-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Unity.zip)

<a id="110-20180626-1"></a>
#### 機能追加
* iOS Google IdP追加：iOS
* Twitter IdP追加：Android、iOS
* LINE IdP追加：Androidのみ提供。iOSは2018年7月に提供予定です。
	
<a id="110-20180626-2"></a>
#### 機能改善・変更
* [SDK] 1.11.0
	* (共通)LocalizedString日本語翻訳を追加
	* (共通)認証APIを呼び出した時に初期化、ログインをしない場合は明確にエラーコードを区別するように内部ロジックを改善
	* (Android)'android.permission.READ_PHONE_STATE'権限を削除
	* (Android)GamebaseConfiguration.Builderの必須設定値であるsetAppId、setAppVersionをコンストラクタで入力できるように変更
	* (Android)GamebaseConfiguration.BuilderのsetServerApiVerseion APIを削除
	* (Android)getAuthBanInfo() API、class AuthBanInfo名を変更：getBanInfo()、class BanInfo
	* NAVER ID Login SDKアップデート：iOS(4.0.10)
* Sample App 
	* ServerPush機能およびObserver機能を追加
	* Gamebase SDKアップデート：Android(1.9.0)、iOS(1.9.0)、Unity(1.10.1)	
	
<a id="1-10-1-2018-06-11"></a>
### 1.10.1 (2018. 06. 11.) { #1-10-1-2018-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.1/GamebaseSDK-Unity.zip)

<a id="101-20180611-1"></a>
#### 不具合修正
* [SDK] 1.10.1
	* (Unity)Unity Adapterがない場合、AddMapping APIを呼び出した時、内部的にログインで処理していた問題を修正

<a id="1-10-0-2018-06-07"></a>
### 1.10.0 (2018. 06. 07.) { #1-10-0-2018-06-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.0/GamebaseSDK-Unity.zip)

<a id="100-20180607-1"></a>
#### 機能追加
* [SDK] 1.10.0
	* (Unity)StandaloneWebviewAdapter: html source renderingサポート	

<a id="100-20180607-2"></a>
#### 機能改善・変更
* [SDK] 1.10.0
	* (Unity)Unity Adapterのinterfaceを修正
		* v1.10.0以上を使用時はUnityAdapterバージョンのアップグレードが必要(GamebaseUnitySDK_FacebookAdapter_v1.5.0、GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
	* (Unity)Login APIを呼び出した時、Unity Adapterがない場合、ネイティブ(Android/iOS)のログインAPIを呼び出すようにロジックを変更：facebook、Google
	* (Unity)がAdapterフォルダ構造および名前の誤字を修正
		* パス：Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
		* 誤字：Adapater → Adapter	

<a id="1-9-0-2018-05-18"></a>
### 1.9.0 (2018. 05. 18.) { #1-9-0-2018-05-18 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

<a id="90-20180518-1"></a>
#### 機能改善・変更
* [SDK] 1.9.0
	* Unity SDK(1.9.0) Google Adapterを新規バージョン(1.6.2)に変更して再配布
    	* 5/3配布されたUnity SDK(1.9.0)に適用されたGoogle Adapterを最新バージョンに変更(1.6.1→1.6.2)
  
<a id="1-9-0-2018-05-03"></a>
### 1.9.0 (2018. 05. 03.) { #1-9-0-2018-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

<a id="90-20180503-1"></a>
#### 機能追加
* Transfer機能追加
    * ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    * (SDK共通)追加されたAPI 
		* Transfer Key発行API (IssueTransferKey)
		* 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)
* 利用停止の登録時、ユーザーのリーダーボード(ランキング)データを削除できるオプションを追加(TOAST Leaderboardを使用する場合に限る)
    * 利用停止登録メニューまたは、App Guard連携ページで使用可能

<a id="1-8-1-2018-04-09"></a>
### 1.8.1 (2018. 04. 09.) { #1-8-1-2018-04-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Unity.zip)
<a id="81-20180409-1"></a>
#### 不具合修正
* [SDK] 1.8.1
	* (Unity)UnityAndroidプラットフォームで下記の機能を使用時、モジュールが初期化されずにNullReferenceExceptionが発生する問題を修正
		* Launching, Purchase, Push, Util, Webview

<a id="1-8-0-2018-04-05"></a>
### 1.8.0 (2018. 04. 05.) { #1-8-0-2018-04-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Unity.zip)

<a id="80-20180405-1"></a>
#### 機能追加
* Kick out機能追加
    * 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    * (SDK共通)kick outイベントを受け取れるAPIを追加
* メンテナンスWebページに、ユーザーがConsoleで入力したHTMLページを使用できるように機能を改善
    * 以前は、Gamebaseで提供するWebページや外部Webページ接続のみ可能だった
    * Webサーバーがない場合でも、メンテナンスページをユーザーが望む形式で作成できる。
* Observer機能の開発およびAPI追加
    * (SDK共通)メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

<a id="80-20180405-2"></a>
#### 機能改善・変更
* [SDK] 1.8.0
	* (共通)Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)
	* (iOS)PAYCO簡単ログイン3rd SDK v1.2.2適用：ログイン成功時、トークン有効期限切れ情報(expires_in)を提供、iPhoneXログインUI改善
	* (iOS)iPhoneXをサポートするために、Webviewを使用したインターフェイスを修正

<a id="80-20180405-3"></a>
#### 不具合修正
* 国コード(contry code)が10文字以上の場合、同時接続データが保存されない現象を修正
* [SDK] 1.8.0
	* (Setting Tool)Unity Facebook Adapterをチェックすると、エラーが発生する問題を修正

<a id="1-7-1-2018-03-13"></a>
### 1.7.1 (2018. 03. 13.) { #1-7-1-2018-03-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.1/GamebaseSDK-Unity.zip)

<a id="71-20180313-1"></a>
#### 不具合修正
* [SDK] 1.7.1
	* (Unity)Inspectorで設定されたSetDebugModeの値が反映されない問題を修正
	* (Unity)Standalone、WebGL：Display Languageで使用されるリソースファイルの欠損部分を修正
	* (Unity)Google Adapter 1.6.2配布：Google Adapter 1.6.1でAuthCodeがEmptyで返され、認証が失敗する問題を修正

<a id="1-7-0-2018-02-22"></a>
### 1.7.0 (2018. 02. 22.) { #1-7-0-2018-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Unity.zip)
<a id="70-20180222-1"></a>
#### 機能追加
* [SDK] 1.7.0
	* NAVER IdP認証追加
	* Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

<a id="1-6-0-2018-01-25"></a>
### 1.6.0 (2018. 01. 25.) { #1-6-0-2018-01-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-Unity.zip)

<a id="60-20180125-1"></a>
#### 機能追加
* [SDK] 1.6.0
	* (Unity)Standalone WinSDK追加
		* 64bitサポート
		* 認証サポート：facebook、google、payco

<a id="1-5-0-2017-12-21"></a>
### 1.5.0 (2017. 12. 21.) { #1-5-0-2017-12-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Unity.zip)

<a id="50-20171221-1"></a>
#### 機能追加
* [SDK] 1.5.0
	* WebViewが閉じられる時に発生するClose Callbackを追加
	* WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
	* Unity Setting Tool新規配布

<a id="50-20171221-2"></a>
#### 不具合修正
* [SDK] 1.5.0
	* (Unity)UnityEditorでゲストログインされない現象を修正
	* (Unity)TOAST ConsoleにFacebook認証情報を登録しないでGamebase.Login("facebook") APIを呼び出した場合、KeyNotFoundExceptionが発生したため、防御コードを追加

<a id="1-4-0-2017-11-23"></a>
### 1.4.0 (2017. 11. 23.) { #1-4-0-2017-11-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Unity.zip)

<a id="40-20171123-1"></a>
#### 機能追加
* [SDK] 1.4.0アップデート
	* (Unity)Gamebase Facebook Adapterを追加：Android、iOS、WebGL、Standalone PlatformおよびUnityEditorをサポート

<a id="1-3-0-2017-10-26"></a>
### 1.3.0 (2017. 10. 26.) { #1-3-0-2017-10-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Unity.zip)

<a id="30-20171026-1"></a>
#### 機能追加
* [SDK] 1.3.0アップデート
	* Credentialを利用したAddMapping API追加

<a id="30-20171026-2"></a>
#### 機能改善・変更
* [SDK] 1.3.0アップデート	
	* (Unity)CredentialInfoを使用するLogin APIを呼び出した時、iOSPluginでJson解析がされない問題を修正
	
<a id="1-2-0-2017-09-21"></a>
### 1.2.0 (2017. 09. 21.) { #1-2-0-2017-09-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Unity.zip)

<a id="20-20170921-1"></a>
#### 機能追加

* 利用停止(ユーザー処罰)機能を追加
* [SDK] 1.2.0アップデート
	* 利用停止ユーザーポップアップ表示

<a id="1-1-5-2017-07-20"></a>
### 1.1.5 (2017. 07. 20.) { #1-1-5-2017-07-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Unity.zip)

<a id="15-20170720-1"></a>
#### 機能改善・変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.5アップデート
	* システムポップアップAPIを追加(showAlertWithTitle)
	* 国コードを大文字で返すように変更(Android)
	* TCPush SDK 1.4.1にアップデート
	* IAP SDK 1.3.3.20170627にアップデート

<a id="1-1-4-2017-05-25"></a>
### 1.1.4 (2017. 05. 25.) { #1-1-4-2017-05-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Unity.zip)

<a id="14-20170525-1"></a>
#### 機能改善・変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.4アップデート
	* ランタイムのうち、決済Storeを変更できるAPIを提供
	* (Android)TCPushSdk v1.4適用、Tencent Push機能を提供

<a id="1-1-2-2017-04-04"></a>
### 1.1.2 (2017. 04. 04.) { #1-1-2-2017-04-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Unity.zip)

<a id="12-20170404-1"></a>
#### 機能改善・変更
* [SDK] 1.1.2アップデート
    * ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善
    * Unity Pluginデバッグログ追加および例外詳細処理

<a id="1-1-0-2017-03-21"></a>
### 1.1.0 (2017. 03. 21.) { #1-1-0-2017-03-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Unity.zip)

<a id="10-20170321-1"></a>
#### 機能改善・変更
* [SDK] 1.1.0アップデート
    * 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加
    * [UI機能追加](./aos-ui)：Custom Webview、AlertDialog

<a id="1-0-0-2017-03-09"></a>
### 1.0.0 (2017. 03. 09.) { #1-0-0-2017-03-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Unity.zip)

<a id="00-20170309-1"></a>
#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
	* 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
	* ログアウトおよび会員退会機能を提供
	* 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
	* ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
	* リアルタイムに運営指標を確認できるWebコンソール画面を提供
	* TOAST Cloudサービスと連携：PUSH、IAP
