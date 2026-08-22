<!-- machine_translated: true -->

<a id="game-gamebase-release-notes-ios"></a>
## Game > Gamebase > リリースノート > iOS { #game-gamebase-release-notes-ios }

<a id="820-2026-07-28"></a>
### 2.82.0 (2026. 07. 28.) { #820-2026-07-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.82.0/GamebaseSDK-iOS.zip)

<a id="820-2026-07-28-feature-updates"></a>
#### 機能改善・変更
* Gamebaseの初期化時にFacebook SDKも初期化されるように修正しました。

<a id="813-2026-05-27"></a>
### 2.81.3 (2026. 05. 27.) { #813-2026-05-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.3/GamebaseSDK-iOS.zip)

<a id="813-2026-05-27-feature-updates"></a>
#### 機能改善・変更
* GamebasePushAdapterをビルドに含めていない状態でPush APIを呼び出した際に、**TCGB_ERROR_NOT_SUPPORTED(10)**エラーを返すように修正しました。
* 内部ロジックの改善

<a id="813-2026-05-27-bug-fixes"></a>
#### 不具合の修正
* GamebaseのWebビューで一部の動画が再生されないバグを修正しました。

<a id="812-2026-04-28"></a>
### 2.81.2 (2026. 04. 28.) { #812-2026-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.2/GamebaseSDK-iOS.zip)

<a id="812-2026-04-28-bug-fixes"></a>
#### 不具合の修正
* アプリがSceneDelegateをサポートしている状態で、実行直後にGamebaseを初期化した際、コールバックが返されないバグを修正しました。

<a id="811-2026-03-30"></a>
### 2.81.1 (2026. 03. 30.) { #811-2026-03-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.1/GamebaseSDK-iOS.zip)

<a id="811-2026-03-30-bug-fixes"></a>
#### 不具合の修正
* LINEでaddMappingを実行した際に、コールバックが返されないバグを修正しました。

<a id="800-2026-02-13"></a>
### 2.80.0 (2026. 02. 13.) { #800-2026-02-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-iOS.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### 機能改善・変更
* Xcodeの最小サポートバージョンが26.0に変更されました。
* 決済リクエスト時、Ask to Buyなどにより遅延決済が遅延した場合、**TCGB_ERROR_PURCHASE_PENDING(4008)**エラーが発生します。
* Gamebase Event HandlerのkTCGBPurchaseUpdatedイベント機能が拡張されました。
    * App Storeのプロモーション商品の購入完了、またはAsk to Buyなどにより遅延した決済が完了した際、イベントを受信できます。
* 内部ロジックの改善
* 以下のAPIが非推奨(deprecated)になりました。
    * **+[TCGBPurchase setPromotionIAPHandler:]**

<a id="790-2026-01-27"></a>
### 2.79.0 (2026. 01. 27.) { #790-2026-01-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-iOS.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### 機能改善・変更
* 内部ロジックの改善
* 以下のAPIが非推奨になりました。
    * **+[TCGBConfiguration setStoreCode:]**
    * **-[TCGBPurchase setStoreCode:]**
    * **TCGBPurchase.storeCode**

<a id="770-2025-12-09"></a>
### 2.77.0 (2025. 12. 09.) { #770-2025-12-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.77.0/GamebaseSDK-iOS.zip)

<a id="770-2025-12-09-feature-updates"></a>
#### 機能改善・変更
* 決済関連の内部ロジックを改善
* 以下のAPIが非推奨となりました。
    * **+[TCGBPurchase requestItemListAtIAPConsoleWithCompletion:]**

<a id="750-2025-09-23"></a>
### 2.75.0 (2025. 09. 23.) { #750-2025-09-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-iOS.zip)

<a id="750-2025-09-23-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Kakaogame iOS SDK (3.20.0)

<a id="731-2025-08-12"></a>
### 2.73.1 (2025. 08. 12.) { #731-2025-08-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-iOS.zip)

<a id="731-2025-08-12-feature-updates"></a>
#### 機能改善・変更
* 外部SDKのアップデート
    * Facebook iOS SDK (18.0.0)

<a id="731-2025-08-12-bug-fixes"></a>
#### 不具合の修正
* Twitterログイン時にエラーが発生する不具合を修正しました。

<a id="730-2025-07-15"></a>
### 2.73.0 (2025. 07. 15.) { #730-2025-07-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-iOS.zip)

<a id="730-2025-07-15-feature-updates"></a>
#### 機能改善・変更
* Xcode最小サポートバージョンが16.0に変更されました。 

<a id="730-2025-07-15-bug-fixes"></a>
#### 不具合修正
* ログイン後にupdateTermsを呼び出した際、同意した利用規約情報が保存されないバグを修正しました。

<a id="721-july-1-2025"></a>
### 2.72.1 (2025. 07. 01.) { #721-july-1-2025 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.1/GamebaseSDK-iOS.zip)

<a id="721-july-1-2025-feature-updates"></a>
#### 機能改善・変更
* iOS 14の特定のデバイスでGameCenterログイン時にクラッシュが発生するバグを修正しました。

<a id="720-2025-06-24"></a>
### 2.72.0 (2025. 06. 24.) { #720-2025-06-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-iOS.zip)

<a id="720-2025-06-24-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.17.2)
        * 内部ロジック改善
    * LINE iOS SDK (5.11.2)
        * bitcode設定除去
        * Xcode 16コンパイラ警告修正
* 内部ロジック改善

<a id="720-2025-06-24-bug-fixes"></a>
#### 不具合修正
* LINE IdPのRegion追加情報に対応していなかったため、マッピング及びloginForLastLoggedInProviderログイン関連動作で発生していた問題を修正しました。

<a id="710-2025-04-15"></a>
### 2.71.0 (2025. 04. 15.) { #710-2025-04-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-iOS.zip)

<a id="710-2025-04-15-added-features"></a>
#### 機能追加
* 「ゲーム告知」新機能を追加しました。
    * API呼び出し方法は次のガイド文書を参照してください。
        * [Game > Gamebase > iOS SDK使用ガイド > UI > GameNotice > Open GameNotice](./ios-ui/#open-gamenotice)

<a id="710-2025-04-15-feature-updates"></a>
#### 機能改善・変更
* storeCodeをnilに設定してGamebase初期化を呼び出した際に、例外が発生する代わりに**TCGB_ERROR_INVALID_PARAMETER(3)** エラーを返すように動作を変更しました。 

<a id="700-2025-03-11"></a>
### 2.70.0 (2025. 03. 11.) { #700-2025-03-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-iOS.zip)

<a id="700-2025-03-11-feature-updates"></a>
#### 機能改善・変更
* ログイン時にIdPサーバーでエラーが発生したことを示す新規エラーコードが追加されました。
    * TCGB_ERROR_AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* TCGBWebViewConfigurationにナビゲーションバータイトル色とアイコン色を設定できるオプションを追加しました。
    * **TCGBWebViewConfiguration.navigationBarTitleColor**
    * **TCGBWebViewConfiguration.navigationBarIconTintColor**

<a id="690-2025-01-21"></a>
### 2.69.0 (2025. 01. 21.) { #690-2025-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-iOS.zip)

<a id="690-2025-01-21-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * PAYCO iOS SDK (1.5.13)
        * iOS 18で正常なPAYCO簡単ログインを利用するためのopenURL関連関数を修正
    * Hangame iOS SDK (1.17.1)
        * 内部ロジック改善
    * Weibo iOS SDK (3.4.0)
        * iOS 18最適化
* completion blockがmain threadで実行されるように修正しました。

<a id="690-2025-01-21-bug-fixes"></a>
#### 不具合修正
* SceneDelegateを使用するアプリでNAVERログインキャンセル時にcallbackが来ないバグを修正しました。
* GamebaseコンソールにLINE old clientIdを設定していない場合、LINEログインが失敗するバグを修正しました。

<a id="681-2024-12-10"></a>
### 2.68.1 (2024. 12. 10.) { #681-2024-12-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-iOS.zip)

<a id="681-2024-12-10-bug-fixes"></a>
#### 不具合修正 
* SwiftファイルからGamebase SDKをimportする時に発生していたエラーを修正しました。

<a id="680-2024-11-26"></a>
### 2.68.0 (2024. 11. 26.) { #680-2024-11-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-iOS.zip)

<a id="680-2024-11-26-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.8.5)
    * Hangame iOS SDK (1.17.0)
* Googleログイン方式を既存のOAuth 2.0からOpenID Connectに変更しました。
* 内部ロジック改善

<a id="670-2024-10-29"></a>
### 2.67.0 (2024. 10. 29.) { #670-2024-10-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-iOS.zip)

<a id="670-2024-10-29-added-features"></a>
#### 機能追加
* Steam認証が追加されました。
* Twitter認証方式をOAuth 2.0に変更し、以下の設定を変更しないとログインが動作しません。
    * OAuth 2.0 Client ID及びClient Secret発行
        * Twitter Developer PortalでOAuth 2.0 Client IDとClient Secretを作成した後、 Gamebaseコンソールに登録します。
    * Callback URL設定
        * GamebaseコンソールにCallback URL(https://id-gamebase.toast.com/oauth/callback)を設定します。 
        * 同じCallback URLをTwitter Developer Portalに追加します。
    * 詳細は以下のリンクをご覧ください。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information > 6. Twitter](./oper-app/#6-twitter)

<a id="670-2024-10-29-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * PAYCO iOS SDK (1.5.12)
        * PAYCO SDKがDynamic Frameworkに変更されました。
    * NAVER iOS SDK (4.2.3)
        * Xcode 16とiOS 18環境で正常に動作するように修正しました。
    * Hangame iOS SDK (1.16.2)
        * Apple Silicon Macでログインに失敗するバグを修正しました。
* Gamebase SDKが外部SDKのリソースを含まないように修正しました。
* 内部ロジック改善

<a id="670-2024-10-29-bug-fixes"></a>
#### 不具合修正 
* システムポップアップウィンドウの上にGamebaseローンチポップアップウィンドウが表示されると画面が黒くなるバグを修正しました。

<a id="663-2024-09-13"></a>
### 2.66.3 (2024. 09. 13.) { #663-2024-09-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-iOS.zip)

<a id="663-2024-09-13-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.8.4)
        * iOS 18でアプリがForeground状態のときにNotificationを重複して受信しないように修正しました。

<a id="662-2024-08-27"></a>
### 2.66.2 (2024. 08. 27.) { #662-2024-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-iOS.zip)

<a id="662-2024-08-27-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.8.3)
        * アプリストア審査でPrivacyInfo manifest関連の警告メールが来ないように修正しました。
* 以下のフィールドが非推奨になりました。
    * **TCGBWebViewConfiguration.orientationMask**
* コンソールに登録されていないIdPでログインを試行すると、TCGB_ERROR_AUTH_IDP_LOGIN_INVALID_IDP_INFO(3202)エラーが発生するように修正しました。
* ローリングイメージ告知のWebビュー内部でエラーが発生した場合、従来の成功コールバック呼び出しの代わりに失敗コールバックが呼び出されるように修正しました。
* 内部ロジックの改善

<a id="660-2024-07-23"></a>
### 2.66.0 (2024. 07. 23.) { #660-2024-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-iOS.zip)

<a id="660-2024-07-23-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Facebook iOS SDK (17.0.2)
    * Hangame iOS SDK (1.15.0)
* アプリの追跡を許可しなくてもHangame-Facebookログインができるように修正しました。

<a id="651-2024-06-25"></a>
### 2.65.1 (2024. 06. 25.) { #651-2024-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-iOS.zip)

<a id="651-2024-06-25-feature-updates"></a>
#### 機能改善・変更
* 特定のクライアントで表示する画像がない場合、エラーの代わりに成功コールバックが呼び出されるように修正しました。

<a id="651-2024-06-25-bug-fixes"></a>
#### 不具合修正 
* 登録されたイメージ告知がない場合、空白のイメージ告知が表示されるイシューを修正しました。

<a id="650-2024-06-11"></a>
### 2.65.0 (2024. 06. 11.) { #650-2024-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-iOS.zip)

<a id="650-2024-06-11-added-features"></a>
#### 機能追加
* イメージ告知機能に新規タイプが追加されました。
    * `ローリングポップアップ`タイプが追加されました。
    * 既存のイメージ告知は`個別ポップアップ`タイプと表記されます。

<a id="650-2024-06-11-feature-updates"></a>
#### 機能改善・変更
* アプリの追跡を許可しなくてもFacebookログインができるように修正しました。
* 外部SDKアップデート
    * Facebook iOS SDK (17.0.1)
        * Facebook SDKがDynamic Frameworkに変更されました。
* Weibo iOS SDKのPrivacyInfo.xcprivacyファイルが修正されました。
* 内部ロジック改善

<a id="640-2024-05-28"></a>
### 2.64.0 (2024. 05. 28.) { #640-2024-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-iOS.zip)

<a id="640-2024-05-28-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * PAYCO iOS SDK (1.5.11)
    * Kakaogame iOS SDK (3.19.0)
* TCGBPushConfiguration.displayLanguageCodeを空の文字列に設定した場合、GamebaseのdisplayLanguageCodeを使用するように修正しました。
* 内部ロジック改善

<a id="631-2024-05-14"></a>
### 2.63.1 (2024. 05. 14.) { #631-2024-05-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.1/GamebaseSDK-iOS.zip)

<a id="631-2024-05-14-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.13.1)
* 内部ロジック改善

<a id="630-2024-04-23"></a>
### 2.63.0 (2024. 04. 23.) { #630-2024-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-iOS.zip)

<a id="630-2024-04-23-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Google iOS SDK (7.1.0)
    * Facebook iOS SDK (17.0.0)
    * Weibo iOS SDK (3.3.8)
* 内部ロジック改善

<a id="620-2024-03-26"></a>
### 2.62.0 (2024. 03. 26.) { #620-2024-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-iOS.zip)

<a id="620-2024-03-26-added-features"></a>
#### 機能追加
* GamebaseとGamebase Adapter SDKにPrivacy manifestと署名を適用しました。
* Gamebase初期化後に返されるLaunchingInfo VOでテスト端末であることを知らせるためのフィールドが追加されました。
    * **launchingInfo.user.testDevice**

<a id="620-2024-03-26-feature-updates"></a>
#### 機能改善・変更
* Xcodeの最小サポートバージョンが15.0に変更されました。
* iOSの最小サポートバージョンが12.0に変更されました。
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.8.1)
    * LINE iOS SDK (5.11.0)
        * LINE認証の最小サポートバージョンが13.0に変更されました。
    * NAVER iOS SDK (4.2.1)
    * PAYCO iOS SDK (1.5.10)
    * Hangame iOS SDK (1.12.0)
* 内部ロジック改善

<a id="610-2024-02-27"></a>
### 2.61.0 (2024. 02. 27.) { #610-2024-02-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-iOS.zip)

<a id="610-2024-02-27-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.8.0)
* SDK内部ロジック改善

<a id="601-2024-02-15"></a>
### 2.60.1 (2024. 02. 15.) { #601-2024-02-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.1/GamebaseSDK-iOS.zip)

<a id="601-2024-02-15-bug-fixes"></a>
#### 不具合修正
* 特定のIdPでログインした後、GameCenterアカウントに変更されるバグを修正しました。

<a id="600-2024-01-23"></a>
### 2.60.0 (2024. 01. 23.) { #600-2024-01-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-iOS.zip)

<a id="600-2024-01-23-feature-updates"></a>
#### 機能改善・変更
* SDK内部ロジック改善

<a id="591-2023-12-27"></a>
### 2.59.1 (2023. 12. 27.) { #591-2023-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.1/GamebaseSDK-iOS.zip)

<a id="591-2023-12-27-bug-fixes"></a>
#### 不具合修正
* Hangame ログイン時にエラーが発生するバグを修正しました。

<a id="590-2023-12-19"></a>
### 2.59.0 (2023. 12. 19.) { #590-2023-12-19 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-iOS.zip)

<a id="590-2023-12-19-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NAVER iOS SDK (4.2.0)
        * NAVER iOS SDKがxcframeworkに変更されました。
* 約款ウィンドウがタブレット環境で固定サイズで表示されるように修正しました。
* Launching Status CodeがINTERNAL_SERVER_ERROR(500)の時にエラーポップアップを表示するように修正しました。

<a id="590-2023-12-19-bug-fixes"></a>
#### 不具合修正
* LINE ログインを重複して呼び出すとクラッシュが発生するバグを修正しました。

<a id="580-2023-11-28"></a>
### 2.58.0 (2023. 11. 28.) { #580-2023-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-iOS.zip)

<a id="580-2023-11-28-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * PAYCO iOS SDK (1.5.9)
        * PAYCO iOS SDKがxcframeworkに変更されました。
    * Kakaogame iOS SDK (3.17.5)
* 最上位ViewController取得ロジックを改善
* Gamebaseローンチポップアップウィンドウが完全に終了した後に初期化コールバックが呼び出されるように修正しました。

<a id="570-2023-10-31"></a>
### 2.57.0 (2023. 10. 31.) { #570-2023-10-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-iOS.zip)

<a id="570-2023-10-31-feature-updates"></a>
#### 機能改善・変更
* Privacy manifestファイルを追加しました。
* Gamebase GameCenterログインスペックが変更されました。
    * GameCenterログインキャンセル後、再リクエストすると、エラーポップアップウィンドウが表示され、TCGB_ERROR_AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED(3203)エラーが発生するように修正しました。

<a id="552-2023-09-26"></a>
### 2.55.2 (2023. 09. 26.) { #552-2023-09-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.2/GamebaseSDK-iOS.zip)

<a id="552-2023-09-26-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Weibo iOS SDK (3.3.4)

<a id="552-2023-09-26-bug-fixes"></a>
#### 不具合修正
* アプリを初めてインストールした後、Weiboログインをしようとすると、コールバックが正常に動作しないバグを修正しました。

<a id="550-2023-09-12"></a>
### 2.55.0 (2023. 09. 12.) { #550-2023-09-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-iOS.zip)

<a id="550-2023-09-12-added-features"></a>
#### 機能追加
* ユーザーがプッシュ権限を拒否してもトークンを登録できるようにTCGBPushConfiguration.alwaysAllowTokenRegistrationフィールドを追加

<a id="550-2023-09-12-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.6.2)

<a id="540-2023-08-29"></a>
### 2.54.0 (2023. 08. 29.) { #540-2023-08-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.54.0/GamebaseSDK-iOS.zip)

<a id="540-2023-08-29-feature-updates"></a>
#### 機能改善・変更
* SDKをxcframeworkに変更
* 外部SDKアップデート
    * Facebook iOS SDK (14.1.0)
    * Google iOS SDK (7.0.0)
* SDK内部ロジックを改善

<a id="530-2023-07-25"></a>
### 2.53.0 (2023. 07. 25.) { #530-2023-07-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.53.0/GamebaseSDK-iOS.zip)

<a id="530-2023-07-25-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.8.6)
* 以下のフィールドがdeprecatedされました。
    * **TCGBWebViewConfiguration.backgroundOpacity**
* iPadで[TCGBUtil showActionSheetWithTitle:message:blocks:]API呼び出し時、ActionSheetが画面中央に来るように修正しました。
* プロジェクトに追加していない認証Adapterを使用する場合、**TCGB_ERROR_AUTH_NOT_SUPPORTED_PROVIDER(3002)**エラーを返すように修正しました。

<a id="530-2023-07-25-bug-fixes"></a>
#### 不具合修正
* 特定状況で利用停止ポップアップウィンドウが表示されないバグを修正しました。
* Apple Silicon MacでWebビューが正常に表示されないバグを修正しました。

<a id="520-2023-06-27"></a>
### 2.52.0 (2023. 06. 27.) { #520-2023-06-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-iOS.zip)

<a id="520-2023-06-27-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.4.0)
    * Weibo iOS SDK (3.3.3)
* 以下のAPIがdeprecatedされました。
    * **+[TCGBGamebase countryCode]**
    * **+[TCGBGamebase countryCodeOfUSIM]**
    * **+[TCGBGamebase carrierCode]**
    * **+[TCGBGamebase carrierName]**
    * **+[TCGBUtil countryCode]**
    * **+[TCGBUtil usimCountryCode]**
    * **+[TCGBUtil carrierCode]**
    * **+[TCGBUtil carrierName]**
* SDK内部ロジック改善

<a id="510-2023-05-30"></a>
### 2.51.0 (2023. 05. 30.) { #510-2023-05-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.51.0/GamebaseSDK-iOS.zip)

<a id="510-2023-05-30-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.3.1)
    * PAYCO iOS SDK (1.5.8)
* SDK内部ロジックの改善

<a id="492-2023-04-28"></a>
### 2.49.2 (2023. 04. 28.) { #492-2023-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.2/GamebaseSDK-iOS.zip)

<a id="492-2023-04-28-bug-fixes"></a>
#### 不具合修正
* 変更事項漏れによる再配布
    * ログイン後に外部認証IdPの認証情報を取得できないバグを修正しました。

<a id="491-2023-04-25"></a>
### 2.49.1 (2023. 04. 25.) { #491-2023-04-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-iOS.zip)

<a id="491-2023-04-25-bug-fixes"></a>
#### 不具合修正
* ログイン後に外部認証IdPの認証情報を取得できないバグを修正しました。

<a id="490-2023-04-11"></a>
### 2.49.0 (2023. 04. 11.) { #490-2023-04-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-iOS.zip)

<a id="490-2023-04-11-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.8.5)

<a id="480-2023-03-28"></a>
### 2.48.0 (2023. 03. 28.) { #480-2023-03-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-iOS.zip)

<a id="480-2023-03-28-feature-updates"></a>
#### 機能改善・変更
* Xcode最小サポートバージョンが14.1に変更されました。 
* iOS最小サポートバージョンが11.0に変更されました。
* armv7、armv7s、i386アーキテクチャのサポートを中断しました。
* bitcodeのサポートを終了しました。
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.3.0)
    * PAYCO iOS SDK (1.5.6)
* DNS障害に備えたGamebaseサーバー予備ドメイン適用

<a id="480-2023-03-28-bug-fixes"></a>
#### 不具合修正
* 特定状況でキックアウトイベントが発生しないバグを修正しました。
* Webビューカスタムスキームコールバックが呼び出されないバグを修正しました。

<a id="470-2023-02-14"></a>
### 2.47.0 (2023. 02. 14.) { #470-2023-02-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.47.0/GamebaseSDK-iOS.zip)

<a id="470-2023-02-14-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.8.4)

<a id="460-2023-01-31"></a>
### 2.46.0 (2023. 01. 31.) { #460-2023-01-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-iOS.zip)

<a id="460-2023-01-31-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * Hangame iOS SDK (1.8.2)
    * Kakaogame iOS SDK (3.14.14)
* SDK内部ロジック改善

<a id="450-2022-12-27"></a>
### 2.45.0 (2022. 12. 27.) { #450-2022-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-iOS.zip)

<a id="450-2022-12-27-added-features"></a>
#### 機能追加
* どのストアの決済領収書なのかを知ることができるように、次のフィールドが追加されました。
    * **TCGBPurchasableReceipt.storeCode**
* 決済API呼び出し時に追加設定を行うことができる**TCGBPurchasableConfiguration**VOを追加しました。
    * [Game > Gamebase > iOS SDK使用ガイド > 決済 > TCGBPurchasableConfiguration](./ios-purchase/#tcgbpurchasableconfiguration)
* **TCGBPurchasableConfiguration**をパラメータとして受け取る新規未消費履歴照会APIを追加しました。
    * **[TCGBPurchase requestItemListOfNotConsumedWithConfiguration:completion:]**
* **TCGBPurchasableConfiguration**をパラメータとして受け取る新規有効化購読照会APIを追加しました。
    * **[TCGBPurchase requestActivatedPurchasesWithConfiguration:completion:]**

<a id="450-2022-12-27-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.2.0)
    * Hangame iOS SDK (1.8.0)
* 以下のAPIがdeprecatedされました。
    * **+[TCGBPurchase requestItemListOfNotConsumedWithCompletion:]**
    * **+[TCGBPurchase requestActivatedPurchasesWithCompletion:]**
* SDK内部ロジック改善

<a id="440-2022-10-25"></a>
### 2.44.0 (2022. 10. 25.) { #440-2022-10-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-iOS.zip)

<a id="440-2022-10-25-feature-updates"></a>
#### 機能改善・変更
* LINE iOS SDK依存関係を変更

<a id="433-2022-10-04"></a>
### 2.43.3 (2022. 10. 04.) { #433-2022-10-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-iOS.zip)

<a id="433-2022-10-04-feature-updates"></a>
#### 機能改善・変更
* SDK内部ロジック改善

<a id="432-2022-09-22"></a>
### 2.43.2 (2022. 09. 22.) { #432-2022-09-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.2/GamebaseSDK-iOS.zip)

<a id="432-2022-09-22-bug-fixes"></a>
#### 不具合の修正
* Game Centerログイン時にエラーが発生するバグを修正しました。

<a id="431-2022-09-14"></a>
### 2.43.1 (2022. 09. 14.) { #431-2022-09-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.1/GamebaseSDK-iOS.zip)

<a id="431-2022-09-14-bug-fixes"></a>
#### 不具合の修正
* CocoaPodsを通じて配布されるLine Auth AdpaterがLINE SDK依存関係エラーでRegionを設定できないバグを修正しました。

<a id="430-2022-09-07"></a>
### 2.43.0 (2022. 09. 07.) { #430-2022-09-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-iOS.zip)

<a id="430-2022-09-07-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート
    * NHN Cloud iOS SDK (1.0.0)
    * ToastGamebaseIAP iOS SDK (0.14.0)
    * LINE iOS SDK (5.8.2)
    * Kakaogame iOS SDK (3.14.4)
    * Hangame iOS SDK (1.7.1)
* LINEログインを行う時にサービスを提供するregionを入力できるように変更しました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > Login with IdP](./ios-authentication/#login-with-idp)
* Gamebaseと互換性が保障されていないGamebase Adapterを使用する場合、初期化時にエラーが発生するように修正しました。
* SDK内部ロジックを改善

<a id="422-2022-08-24"></a>
### 2.42.2 (2022. 08. 24.) { #422-2022-08-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.2/GamebaseSDK-iOS.zip)

<a id="422-2022-08-24-feature-updates"></a>
#### 機能改善・変更
* Webビューで使用するスキームリストのうち"itms-services"がAppleの審査でリジェクトされる場合があり削除しました。

<a id="421-2022-08-09"></a>
### 2.42.1 (2022. 08. 09.) { #421-2022-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-iOS.zip)

<a id="421-2022-08-09-bug-fixes"></a>
#### 不具合の修正
* コントラスト増加オプションを有効にした場合、Gamebaseポップアップ窓が正常に表示されないバグを修正しました。
* SceneDelegateを使用するプロジェクトでGamebaseポップアップ窓が表示されないバグを修正しました。

<a id="420-2022-07-26"></a>
### 2.42.0 (2022. 07. 26.) { #420-2022-07-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-iOS.zip)

<a id="420-2022-07-26-added-features"></a>
#### 機能追加
* マッピングが失敗したときに返されるForcingMappingTicket VOクラスにユーザーの現在状態を知ることができるフィールドが追加されました。
    * **TCGBForcingMappingTicket.mappedUserValid**
    * mappedUserValidに保存された値の意味は以下を参照してください。
        * [Game > Gamebase > APIガイド > API v1.3ガイド > Others > Mamber Vaild Code](./api-guide/#member-valid-code)

<a id="420-2022-07-26-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: Hangame iOS SDK (1.7.0)

<a id="420-2022-07-26-bug-fixes"></a>
#### 不具合の修正
* 無効なAppIDでGamebaseを初期化したときにコールバックが呼び出されないバグを修正しました。
* ハンゲームIdPでログインした状態でGamebaseのEventHandlerの **kTCGBIdPRevoked**イベントが発生しないバグを修正しました。

<a id="411-2022-07-20"></a>
### 2.41.1 (2022. 07. 20.) { #411-2022-07-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-iOS.zip)

<a id="411-2022-07-20-feature-updates"></a>
#### 機能改善・変更
* 約款ウィンドウが完全に閉じたあとにコールバックを呼び出すように修正しました。

<a id="410-2022-07-05"></a>
### 2.41.0 (2022. 07. 05.) { #410-2022-07-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-iOS.zip)

<a id="410-2022-07-05-added-features"></a>
#### 機能追加
* GamebaseEventHandlerのGamebaseEventCategoryに**kTCGBIdPRevoked**タイプが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./ios-etc/#idp-revoked)

<a id="410-2022-07-05-feature-updates"></a>
#### 機能改善・変更
* 画像告知が表示中のとき、画面の向きに応じて回転するように変更しました。

<a id="400-2022-05-24"></a>
### 2.40.0 (2022. 05. 24.) { #400-2022-05-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-iOS.zip)

<a id="400-2022-05-24-feature-updates"></a>
#### 機能改善・変更
* SDK内部ロジック改善

<a id="390-2022-05-10"></a>
### 2.39.0 (2022. 05. 10.) { #390-2022-05-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-iOS.zip)

<a id="390-2022-05-10-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: Hangame iOS SDK (1.6.4)

<a id="380-2022-05-03"></a>
### 2.38.0 (2022. 05. 03.) { #380-2022-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-iOS.zip)

<a id="380-2022-05-03-feature-updates"></a>
#### 機能改善・変更
* Display Languageの中国語繁体字(zh-TW)言語セットで不自然な文章を修正しました。

<a id="370-2022-04-26"></a>
### 2.37.0 (2022. 04. 26.) { #370-2022-04-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-iOS.zip)

<a id="370-2022-04-26-added-features"></a>
#### 機能追加
* サポートURLの後ろにパラメータを追加できるように次のフィールドが追加されました。
    * **TCGBContactConfiguration.additionalParameters**

<a id="360-2022-04-12"></a>
### 2.36.0 (2022. 04. 12.) { #360-2022-04-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-iOS.zip)

<a id="360-2022-04-12-added-features"></a>
#### 機能追加
* 決済領収書でsandboxおよびプロモーション決済なのかどうかを知ることができるように、次のフィールドが追加されました。
    * **TCGBPurchasableReceipt.sandboxPayment**
    * **TCGBPurchasableReceipt.promotionPayment**

<a id="360-2022-04-12-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：TOAST iOS SDK(0.30.0)、ToastGamebaseIAP SDK(0.13.0)、Hangame iOS SDK (1.6.3)

<a id="350-2022-03-29"></a>
### 2.35.0 (2022. 03. 29.) { #350-2022-03-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

<a id="350-2022-03-29-added-features"></a>
#### 機能追加
* 現在約款ウィンドウが画面に表示されているかどうかを知ることができるAPIを追加しました。
    * **[TCGBTerms isShowingTermsView]**

<a id="350-2022-03-29-feature-updates"></a>
#### 機能改善・変更
* Google Webログイン方式からGoogle SDKログイン方式に変更しました。
* ハンゲームログインを途中でキャンセルした場合、**TCGB_ERROR_AUTH_USER_CANCELED(3001)**エラーを返すように修正しました。

<a id="341-2022-03-15"></a>
### 2.34.1 (2022. 03. 15.) { #341-2022-03-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

<a id="341-2022-03-15-added-features"></a>
#### 機能追加
* SwiftプロジェクトユーザーのためにPublic APIにNS_SWIFT_NAME設定を追加しました。

<a id="341-2022-03-15-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート：Hangame iOS SDK (1.6.2)
* デバイスが横モードの状態でshowWebView APIを呼び出したとき、下部に黒い空スペースが表示されるエラーを修正しました。

<a id="340-2022-02-22"></a>
### 2.34.0 (2022. 02. 22.) { #340-2022-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

<a id="340-2022-02-22-added-features"></a>
#### 機能追加
* Gamebaseコンソールのアップデート必須設定で**ポップアップボタン追加**を選択すると、クライアントのアップデート必須ポップアップに**詳細表示**ボタンが追加されます。
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* 共通約款API呼び出し後、約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * **TCGBShowTermsViewResult**

<a id="340-2022-02-22-feature-updates"></a>
#### 機能改善・変更
* イメージ告知APIを呼び出したときに表示するイメージ告知がない場合、背景が少しの間暗くなる現象を修正しました。
* キックアウトポップアップの表示有無はGamebaseコンソールでキックアウト登録時に設定することができるため、次のAPIがdeprecatedになりました。
    * **-[TCGBConfiguration enableKickoutPopup:]**
    * **-[TCGBConfiguration isEnableKickoutPopup]**

<a id="330-20220125"></a>
### 2.33.0 (2022.01.25) { #330-20220125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

<a id="330-20220125-added-features"></a>
#### 機能追加
* 共通約款ウィンドウの設定を変更できる新規APIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > UI > Terms > showTermsView](./ios-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: PAYCO iOS SDK (1.5.5)
* エラーコード追加および変更
    * TCGB_ERROR_UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたTCGB_ERROR_SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。

<a id="321-20220111"></a>
### 2.32.1 (2022.01.11) { #321-20220111 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)

<a id="321-20220111-feature-updates"></a>
#### 機能改善・変更
* アップデート推奨ポップアップウィンドウで **[今すぐアップデート]** ボタンをクリックした際、ポップアップウィンドウが閉じないように修正しました。
* SDKの安定性を改善しました。

<a id="320-20211228"></a>
### 2.32.0 (2021.12.28) { #320-20211228 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

<a id="320-20211228-added-features"></a>
#### 機能追加
* GamebaseEventHandlerのGamebaseEventCategoryに **kTCGBServerPushAppKickoutMessageReceived** タイプが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* GamebaseEventHandlerのGamebaseEventCategoryに **kTCGBLoggedOut** タイプが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

<a id="320-20211228-feature-updates"></a>
#### 機能改善・変更
* WebビューnavigationBarの基本タイトル色が **UIColor.whiteColor** に変更されました。

<a id="320-20211228-bug-fixes"></a>
#### 不具合修正
* Hangameログアウト呼び出し時、thirdIdPもログアウトされるように修正しました。

<a id="310-20211214"></a>
### 2.31.0 (2021.12.14) { #310-20211214 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

<a id="310-20211214-added-features"></a>
#### 機能追加
* メンテナンスポップアップウィンドウでメンテナンス時間の表示有無を動的に設定できるようになりました。

<a id="310-20211214-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST iOS SDK (0.29.2)、PAYCO iOS SDK (1.5.4)
* 利用停止WebビューのカスタマーサポートリンクからBAN済みユーザー情報でお問い合わせを登録できない問題を修正しました。
* メンテナンスポップアップウィンドウ、利用停止詳細表示Webビューで戻るボタンが表示されるように修正しました。

<a id="301-20211125"></a>
### 2.30.1 (2021.11.25) { #301-20211125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

<a id="301-20211125-bug-fixes"></a>
#### 不具合修正
* Unity 2019.3以上でCocoapodsをインストールした際に、決済とプッシュAPIでエラーが発生するバグを修正しました。

<a id="300-20211123"></a>
### 2.30.0 (2021.11.23) { #300-20211123 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

<a id="300-20211123-added-features"></a>
#### 機能追加
* 強制マッピング時にIdPログインを再度試行しなければならない手間を改善した、新しい強制マッピングAPIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* 特定IdPでマッピング試行後、**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** エラーが発生した際、該当IdPでアカウントを変更できるAPIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > Change Login with ForcingMappingTicket](./ios-authentication/#change-login-with-forcingmappingticket)

<a id="300-20211123-bug-fixes"></a>
#### 不具合修正
* loginForLastLoggedInProviderログイン後、特定IdPでログアウトまたは退会機能が動作しないバグを修正しました。

<a id="290-20211109"></a>
### 2.29.0 (2021.11.09) { #290-20211109 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

<a id="290-20211109-feature-updates"></a>
#### 機能改善・変更
* Xcodeの最小サポートバージョンが12から13に変更されました。
* 外部SDKアップデート: TOAST iOS SDK (0.29.1)、ToastGamebaseIAP SDK (0.12.1)
* コンソールに登録したメンテナンスおよび告知詳細表示のURLをエンコードせずに画面に表示するように変更されました。

<a id="290-20211109-bug-fixes"></a>
#### 不具合修正
* TCGBPushMessage.extrasをJSON解析する際にエラーが発生するバグを修正しました。

<a id="280-20210928"></a>
### 2.28.0 (2021.09.28) { #280-20210928 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

<a id="280-20210928-added-features"></a>
#### 機能追加
* Kakaogame認証を追加
* 「決済不正行為自動解除」機能が追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > GraceBan](./ios-authentication/#graceban)
    * 決済不正行為自動解除機能は、決済不正行為自動制裁で利用停止になるべきユーザーが「利用停止猶予状態」を経た後、利用停止になるようにします。
    * 「利用停止猶予状態」の場合、設定した期間内に解除条件をすべて満たすと正常にプレイできます。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済不正行為自動解除機能を使用するゲームはログイン後、常にTCGBAuthToken.tcgbMember.graceBanInfo値を確認し、nullではない有効なTCGBGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

<a id="280-20210928-feature-updates"></a>
#### 機能改善・変更
* PAYCO iOS SDKアップデート (1.5.2)

<a id="271-20210914"></a>
### 2.27.1 (2021.09.14) { #271-20210914 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

<a id="271-20210914-feature-updates"></a>
#### 機能改善・変更
* PAYCO iOS SDKアップデート (1.5.1)
    * 認証フローおよびUI改善
* Hangame iOS SDKアップデート (1.6.1)
    * 本人認証でエラーが発生した際にコールバックが呼び出されないイシューを修正
    * iOS 15 betaでナビゲーションバーが正常に表示されないイシューを修正

<a id="271-20210914-bug-fixes"></a>
#### 不具合修正
* すでに約款に同意して約款UIが表示されない場合、PushConfigurationがnilで返されないイシューを修正しました。

<a id="270-20210824"></a>
### 2.27.0 (2021.08.24) { #270-20210824 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

<a id="270-20210824-feature-updates"></a>
#### 機能改善・変更
* PAYCO iOS SDKアップデート (1.5.0)
    * PAYCOアプリがない場合、以前は手動ログインのみ可能でしたが、Safariにログインしている場合は簡単ログイン機能を使用できるように変更されました。

<a id="270-20210824-bug-fixes"></a>
#### 不具合修正
* Unityで画像告知が表示されないイシューを修正しました。
    * Gamebase iOS SDK 2.27.0未満を使用する場合、Unityで画像告知が表示されないことがあります。
    * 画像告知を使用する場合は、Gamebase iOS SDK 2.27.0以上を使用してください。

<a id="260-20210810"></a>
### 2.26.0 (2021.08.10) { #260-20210810 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

<a id="260-20210810-feature-updates"></a>
#### 機能改善・変更
* Display Language機能が改善されました。
    * これまでは言語セットを追加するためにGamebase.bundle内のファイルを直接修正する必要がありました。
        * XcodeプロジェクトのCopy Bundle Resourcesにlocalizedstring.jsonファイルを追加する方法に変更しました。
    * Display Language言語セットに中国語簡体字(zh-CN)、中国語繁体字(zh-TW)、タイ語(th)が追加されました。
    * デフォルトの言語コードが **en** でしたが、Gamebaseコンソールで設定したデフォルト言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](./oper-app/#language-settings)
* showTermsView API呼び出し後に生成できるPushConfigurationオブジェクトの生成基準が次のように変更されました。
    * 変更前
        * 約款項目中に **Push受信** 項目が存在する場合にのみnilではない有効なPushConfigurationが返されました。
        * ユーザーが昼間・夜間広告性Push受信をすべて拒否した場合、PushConfiguration.pushEnabledはfalseで生成されました。
    * 変更後
        * 約款UIが表示された場合、常にnilではない有効なPushConfigurationが返されます。
        * showTermsViewが返すPushConfigurationオブジェクトのpushEnabled値は常にtrueです。
    * 変更されない点
        * すでに約款に同意して約款UIが表示されなかった場合、PushConfigurationはnilで返されます。

<a id="260-20210810-bug-fixes"></a>
#### 不具合修正
* Push言語設定は特別な補助処理なしで端末の言語コードがそのまま適用され、Pushコンソールから送信したメッセージの言語コードが一致しない問題を修正しました。

<a id="250-20210727"></a>
### 2.25.0 (2021.07.27) { #250-20210727 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-iOS.zip)

<a id="250-20210727-more-features"></a>
#### 機能追加
* 月間決済限度機能を追加
    * 月間決済限度を超える場合、**PURCHASE_LIMIT_EXCEEDED(4007)** エラーが発生します。

<a id="250-20210727-feature-updates"></a>
#### 機能改善・変更
* Push項目が存在する約款でPushConfigurationオブジェクトを保証
    * 約款UIでPush受信に同意しない場合、Gamebase.Terms.showTermsView API呼び出し結果として生成されるTCGBPushConfigurationがnullでしたが、約款にPush項目が存在する場合はTCGBPushConfigurationオブジェクトが常に返されるように変更されました。
    * Push受信を拒否すると、TCGBPushConfigurationオブジェクトは(プッシュ同意 = false、広告性プッシュ同意 = false、夜間広告性プッシュ同意 = false)で生成されます。
    * 約款にPush項目が存在しない場合、TCGBPushConfigurationオブジェクトはnullです。
* 外部SDKアップデート: TOAST iOS SDK (0.29.0)
* Sign In with AppleのASAuthorizationErrorUnknownエラーが発生した場合、TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERRORエラーを返すように変更

<a id="250-20210727-bug-fixes"></a>
#### 不具合修正
* registerPushを通じて登録したTCGBPushConfiguration値とTCGBPushTokenInfo値が異なるバグを修正

<a id="240-20210629"></a>
### 2.24.0 (2021.06.29) { #240-20210629 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-iOS.zip)

<a id="240-20210629-feature-updates"></a>
#### 機能改善・変更
* 内部ローンチURL変更

<a id="240-20210629-bug-fixes"></a>
#### 不具合修正
* 約款詳細表示後、約款ポップアップウィンドウが閉じないバグを修正

<a id="game-gamebase-release-notes-ios-1"></a>
### 2.23.0 (2021.06.14) { #game-gamebase-release-notes-ios-1 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-iOS.zip)

<a id="game-gamebase-release-notes-ios-1-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST iOS SDK (0.28.0)、ToastGamebaseIAP SDK (0.12.0)

<a id="220-20210525"></a>
### 2.22.0 (2021.05.25) { #220-20210525 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-iOS.zip)

<a id="220-20210525-feature-updates"></a>
#### 機能改善・変更
* 外部SDKアップデート: TOAST iOS SDK (0.27.2)、Hangame iOS SDK (1.6.0)

<a id="212-20210427"></a>
### 2.21.2 (2021.04.27) { #212-20210427 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.2/GamebaseSDK-iOS.zip)

<a id="212-20210427-feature-updates"></a>
#### 機能改善・変更
* Facebook iOS SDKアップデート (9.2.0)

<a id="212-20210427-bug-fixes"></a>
#### 不具合修正
* アーカイブビルド時にbitcode関連エラーが発生するイシューを修正

<a id="211-20210419"></a>
### 2.21.1 (2021.04.19) { #211-20210419 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-iOS.zip)

<a id="211-20210419-bug-fixes"></a>
#### 不具合修正
* bitcodeをサポートできるように設定しても設定値が反映されない問題を修正

<a id="210-20210413"></a>
### 2.21.0 (2021.04.13) { #210-20210413 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-iOS.zip)

<a id="210-20210413-more-features"></a>
#### 機能追加
* Hangame日本認証を追加

<a id="210-20210413-feature-updates"></a>
#### 機能改善・変更
* bitcodeをサポートできるように変更
* showWebView呼び出し時、閉じるボタンが最初に画面に表示されるように修正

<a id="202-20210323"></a>
### 2.20.2 (2021.03.23) { #202-20210323 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-iOS.zip)

<a id="202-20210323-feature-updates"></a>
#### 機能改善・変更
* Facebook iOS SDKアップデート(9.1.0)
* 特定の場合においてGamebaseAuthFacebookAdapterでopenURL delegateが呼び出されないイシューを修正

<a id="201-20210309"></a>
### 2.20.1 (2021.03.09) { #201-20210309 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-iOS.zip)

<a id="201-20210309-feature-updates"></a>
#### 機能改善・変更
* iOS 14に対応し、IDFA取得ロジックを修正：info.plistにNSUserTrackingUsageDescriptionフィールドを追加

<a id="200-20210209"></a>
### 2.20.0 (2021.02.09) { #200-20210209 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-iOS.zip)

* 共通約款機能を追加
    * 約款WebビューをオープンするAPIを追加
    * 約款リストおよびユーザーごとの同意有無を照会するAPIを追加
    * ユーザーごとの約款同意有無をGamebaseサーバーに保存するAPIを追加

<a id="200-20210209-more-features"></a>
#### 機能改善・変更
* 고객 센터 타입이 TOAST 조직 상품(Online Contact)인 경우 로그인을 하지 않아도 고객 센터가 표시되도록 변경

<a id="feature-updates"></a>
### 2.19.1 (2021.01.26) { #feature-updates }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-iOS.zip)

<a id="feature-updates-191-20210126"></a>
#### 機能改善・変更
* Weibo IdPAdapterの構造を変更

<a id="january-12-2021"></a>
### 2021. 01. 12. { #january-12-2021 }

```
Gamebase の XCode 最小サポートバージョンが 10 から 11 に変更されました。
```

<a id="190-20201229"></a>
### 2.19.0 (2020.12.29) { #190-20201229 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-iOS.zip)

<a id="190-20201229-more-features"></a>
#### 機能追加

* Weibo認証を追加

<a id="190-20201229-feature-updates"></a>
#### 機能改善・変更

* ローンチステータスコード追加：ベータサービス(205)

<a id="182-20201215"></a>
### 2.18.2 (2020.12.15) { #182-20201215 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

<a id="182-20201215-more-features"></a>
#### 機能追加

* 開発会社が独自のサポートをオープンする時、additionalURLフィールドを追加
* 決済アイテム情報にローカライズされた商品情報を追加：localizedTitle、localizedDescription

<a id="182-20201215-feature-updates"></a>
#### 機能改善・変更

* 外部SDKアップデート：TOAST iOS SDK(0.27.1)
* showWebView：無効なURLを伝達した場合、エラーを返す。伝達されたURLはエンコードせずにそのまま使用
* 大文字/小文字に関係なく、カスタムスキームが動作するように変更

<a id="180-20201110"></a>
### 2.18.0 (2020.11.10) { #180-20201110 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

<a id="180-20201110-feature-updates"></a>
#### 機能改善・変更

* iOS 13以上から提供されるSceneDelegate対応APIを追加

<a id="171-20201027"></a>
### 2.17.1 (2020.10.27) { #171-20201027 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

<a id="171-20201027-feature-updates"></a>
#### 機能改善・変更

* 特定指標の転送時にエラーメッセージを追加して転送：プッシュ登録に失敗した時、ゲーム指標を転送する時

<a id="170-20201013"></a>
### 2.17.0 (2020.10.13) { #170-20201013 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)

```
한게임認証のご利用を希望される場合は、事前にカスタマーセンターまでお問い合わせください。
```

<a id="170-20201013-more-features"></a>
#### 機能追加

* Hangame IdP認証追加

<a id="170-20201013-feature-updates"></a>
#### 機能改善・変更

* サポート添付イメージクリック時、ダウンロードサポート
* TCGBMember.regDate、TCGBMember.lastLoginDateのタイプをlong longに変更
* WebビューからURLおよびタイトルを変更した時、タイトルを再出力できるようにロジックを変更

<a id="170-20201013-bug-fixes"></a>
#### 不具合修正

* PAYCO認証：lastLoggedInProviderログイン後、ログアウトを呼び出した時、ログアウトコールバックが来ない問題を修正

<a id="160-20200922"></a>
### 2.16.0 (2020.09.22) { #160-20200922 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

<a id="160-20200922-more-features"></a>
#### 機能追加

* サポート機能追加
    * API追加(Gamebase.Contact.requestContactURL)：サポートURLリターン
    * サポートAPIにuserNameを設定できるようにContactConfigurationパラメータを追加

<a id="151-20200916"></a>
### 2.15.1 (2020.09.16) { #151-20200916 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

<a id="151-20200916-feature-updates"></a>
#### 機能改善・変更

* 外部SDKアップデート：TOAST iOS SDK(0.27.0)
* iOS 14 beta変更事項を対応したIAP SDK新規バージョンが適用されました。 [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

<a id="150-20200825"></a>
### 2.15.0 (2020.08.25) { #150-20200825 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

<a id="150-20200825-more-features"></a>
#### 機能追加

* プッシュトークン登録時に、アプリのNotificationOption設定がForeground状態でもプッシュ通知を受け取れるように機能追加
* プッシュAPI追加：Pushトークン情報確認(Gamebase.Push.queryTokenInfo API)

<a id="150-20200825-feature-updates"></a>
#### 機能改善・変更

* 外部SDKアップデート：TOAST iOS SDK(0.26.0)
* 決済payloadのnull checkロジック追加

<a id="140-20200811"></a>
### 2.14.0 (2020.08.11) { #140-20200811 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

<a id="140-20200811-feature-updates"></a>
#### 機能改善・変更

* PAYCO IdPの定数値を削除：PAYCO文字列によるApple審査でリジェクトされる場合があり削除
* TCGBWebViewConfigurationにcontentMode設定を追加

<a id="130-20200728"></a>
### 2.13.0 (2020.07.28) { #130-20200728 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

<a id="130-20200728-feature-updates"></a>
#### 機能追加

* Sign In With Apple認証：iOS 12以下をサポート

<a id="120-20200714"></a>
### 2.12.0 (2020.07.14) { #120-20200714 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

<a id="120-20200714-more-features"></a>
#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * イメージ告知表示APIを追加

<a id="120-20200714-feature-updates"></a>
#### 機能改善・変更
* Facebook SDKアップデート(7.1.1)
* configuartionに設定されたstoreCode(default=AS)でGamebaseの初期化を試行
* コンテンツをローディングできないWebビューを出力時、**閉じる**ボタンがなくて閉じられない問題を修正

<a id="110-20200623"></a>
### 2.11.0 (2020.06.23) { #110-20200623 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

<a id="110-20200623-more-features"></a>
#### 機能追加

* 決済API追加：商品IDで決済リクエスト、追加情報(UserPayload)を入力して決済完了時に確認できる

<a id="101-20200609"></a>
### 2.10.1 (2020.06.09) { #101-20200609 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

<a id="101-20200609-feature-updates"></a>
#### 機能改善・変更

* ユーザープッシュ設定の初期化時、言語コードが設定されていない場合、デバイス言語で設定されるように変更

<a id="100-20200526"></a>
### 2.10.0 (2020.05.26) { #100-20200526 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

<a id="100-20200526-more-features"></a>
#### 機能追加
* 既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
    * ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

<a id="91-20200512"></a>
### 2.9.1 (2020.05.12) { #91-20200512 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)

<a id="91-20200512-bug-fixes"></a>
#### 不具合修正

* Unrealエンジンでビルドすると、警告(warning)をビルドエラーと判定してビルドができない問題を修正

<a id="90-20200428"></a>
### 2.9.0 (2020.04.28) { #90-20200428 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

<a id="90-20200428-more-features"></a>
#### 機能追加
* 退会猶予機能
    * API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認

<a id="90-20200428-feature-updates"></a>
#### 機能改善・変更

* 外部SDKアップデート：TOAST iOS SDK(0.24.0)
* PAYCO iOS SDKアップデート(1.4.0)

<a id="81-20200414"></a>
### 2.8.1 (2020.04.14) { #81-20200414 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

<a id="81-20200414-feature-updates"></a>
#### 機能改善・変更

* Analytics転送結果を確認するための内部指標を追加

<a id="80-20200324"></a>
### 2.8.0 (2020.03.24) { #80-20200324 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

<a id="80-20200324-more-features"></a>
#### 機能追加

* 決済および商品情報に商品タイプおよび地域価格などの情報を追加

<a id="80-20200324-feature-updates"></a>
#### 機能改善・変更

* コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップウィンドウが表示されるように改善

<a id="71-20200225"></a>
### 2.7.1 (2020.02.25) { #71-20200225 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

<a id="71-20200225-feature-updates"></a>
#### 機能改善・変更

* GuestでLoginしてGetAuthProviderUserIDを呼び出した時、値を返すように修正

<a id="62-20191224"></a>
### 2.6.2 (2019.12.24) { #62-20191224 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)

<a id="62-20191224-feature-updates"></a>
#### 機能改善・変更

* 外部SDKアップデート：TOAST iOS SDK(0.20.1)
* NAVER iOS SDKアップデート(4.1.0)

<a id="61-20191210"></a>
### 2.6.1 (2019.12.10) { #61-20191210 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)

<a id="61-20191210-bug-fixes"></a>
#### 不具合修正
* AddMapping(強制、Forcibly)使用時、マッピングされない問題を修正
* Unity PluginでPushConfigurationのdisplayLanguageCodeを設定しない場合、NSNullオブジェクトによりクラッシュが発生する問題を修正

<a id="60-20191112"></a>
### 2.6.0 (2019.11.12) { #60-20191112 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

<a id="60-20191112-added-features"></a>
#### 機能追加

* データをLog&Crashに転送して、各種分析に利用できるようにTOAST Loggerを追加
* Sign In with Apple認証を追加

<a id="52-20191015"></a>
### 2.5.2 (2019.10.15) { #52-20191015 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

<a id="52-20191015-feature-updates"></a>
#### 機能改善・変更

* UIWebViewをWKWebViewに変更

<a id="september-10-2019-sdk-download"></a>
### 2.5.1 (2019.09.10) { #september-10-2019-sdk-download }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)

<a id="september-10-2019-sdk-download-feature-updates"></a>
#### 機能改善・変更

* GamebasePushAdapterで使用中のTCPushSDKを1.7.0にアップデート
    * TCPushSDKがStatic LibraryからFrameworkファイルに変更されたため、プロジェクトにTCPushSDK.frameworkを追加する必要があります。

<a id="50-august-27-2019"></a>
### 2.5.0 (2019.08.27) { #50-august-27-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

<a id="50-august-27-2019-more-features"></a>
#### 機能追加

* Consoleで入力したCS URLをWebビューで開くAPIを提供

<a id="43-july-11-2019"></a>
### 2.4.3 (2019.07.11) { #43-july-11-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

<a id="43-july-11-2019-bug-fixes"></a>
#### 不具合修正

* 認証試行時にエラーが発生した場合、形式に合わないエラーメッセージの解析試行によるクラッシュ発生の問題を修正

<a id="42-june-25-2019"></a>
### 2.4.2 (2019.06.25) { #42-june-25-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

<a id="42-june-25-2019-features-updateschanges"></a>
#### 機能改善・変更

* LaunchingInfoにJSON string形式のTOAST Launching情報を追加
* LINE iOS SDKアップデート (5.0.1)
    * LINE Adapterの最小サポートOSバージョンがiOS 10に変更
    * LINEアプリによるログイン機能を追加

<a id="42-june-25-2019-bug-fixes"></a>
#### 不具合修正

* Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正
* ネットワーク接続問題により、断続的にクラッシュが発生する現象を修正

<a id="41-june-13-2019"></a>
### 2.4.1 (2019.06.13) { #41-june-13-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

<a id="41-june-13-2019-bug-fixes"></a>
#### 不具合修正

* Analytics指標転送時、一部パラメータが欠落して指標が正常に出力されないバグを修正

<a id="40-may-28-2019"></a>
### 2.4.0 (2019.05.28) { #40-may-28-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

<a id="40-may-28-2019-feature-updates"></a>
#### 機能改善・変更

* 指標関連Classの変更
    * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除 [詳細表示 [iOS](./ios-etc/#game-user-data-settings)]
    * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加 [詳細表示 [iOS](./ios-etc/#level-up-trace)]

<a id="30-20190423"></a>
### 2.3.0 (2019.04.23) { #30-20190423 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

<a id="30-20190423-1"></a>
#### 機能改善・変更

* Launching Status Code追加：「審査中(204)」、「テスト中(203)」

<a id="22-20190411"></a>
### 2.2.2 (2019.04.11) { #22-20190411 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

<a id="22-20190411-1"></a>
#### 不具合修正

* showBlockingPopupをNOに設定した場合、Gamebase初期化コールバックが呼び出されないバグを修正

<a id="20-20190326"></a>
### 2.2.0 (2019.03.26) { #20-20190326 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)

<a id="20-20190326-1"></a>
#### 機能追加
* TransferAccount機能追加：ゲストユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行できる機能
    * 追加されたAPI
        * TransferAccountInfo発行API (issueTransferAccount)
        * 発行されたTransferAccountInfoを使用して、アカウント移行をリクエストするAPI (transferAccountWithIdPLogin)
        * 発行されたTransferAccountInfoを確認するAPI (queryTransferAccount)
        * すでに発行されたTransferAccountInfoを更新するAPI (renewTransferAccount)
* 強制マッピング機能追加：すでに他のアカウントに連携しているIdPアカウントをマッピングできる機能
    * 追加されたAPI
        * 強制マッピングするAPI (addMappingForcibly)

<a id="20-20190326-2"></a>
#### 機能改善・変更

* LINE iOS SDKのAppログイン機能が無効化
    * LINE SDK v4のバグにより、iOS 12でアプリログインが失敗するバグがあり、Gamebase Line AdapterでWebログインのみサポートするように変更

<a id="10-20190226"></a>
### 2.1.0 (2019.02.26) { #10-20190226 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)

<a id="10-20190226-1"></a>
#### 機能改善・変更

* TransferKey APIを削除
    * issueTransferKey：TransferKey発行
    * requestTransfer：TransferKey検証

<a id="10-20190226-2"></a>
#### 不具合修正

* GamecenterをGamebaseではない別のロジックによりログインした後、Gamebaseを通してGamecenterログインを試みた際に反応がないバグを修正

<a id="00-20190129"></a>
### 2.0.0 (2019.01.29) { #00-20190129 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
Gamebase 2.0 の改善された全体指標を活用するには、SDK のアップデートが必要です。
```

<a id="00-20190129-1"></a>
#### 機能追加

* Custom指標のためのAPIを追加（購入成功の場合、SDK内部で自動転送）
    * setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を転送
    * traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした際に呼び出す

<a id="00-20190129-2"></a>
#### 機能改善・変更

* IAP SDKアップデート
    * 決済失敗時、断続的にクラッシュが発生する現象を修正

<a id="00-20190129-3"></a>
#### 不具合修正

* iOS 12以上のシミュレータでdebugMode On状態でGamebaseを初期化するとクラッシュが発生する現象を修正

<a id="142-20181115"></a>
### 1.14.2 (2018.11.15) { #142-20181115 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)

<a id="142-20181115-1"></a>
#### 機能改善・変更

* Provider Profile取得メソッドを呼び出した際に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にクラッシュが発生する可能性がある構造を修正

<a id="140-20181023"></a>
### 1.14.0 (2018.10.23) { #140-20181023 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)

<a id="140-20181023-1"></a>
#### 機能追加

* Gamebase WebビューにファイルI添付機能を追加

<a id="140-20181023-2"></a>
#### 機能改善・変更
* 利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして送信し、クライアントでデコードして処理するように修正
* PAYCO iOS SDKアップデート (1.2.4)
* Remove API：Webview、Network、Launching
    * **[TCGBUtil showToastWithMessage:duration:]**
    * **[TCGBWebView showWebBrowserWithURL:viewController:]**
    * **[TCGBWebView showWebViewWithURL:viewController:configuration:]**
    * **[TCGBLaunching addObserverOnChangedStatusNotification:]**
    * **[TCGBLaunching removeObserverOnChangedStatusNotification:]**
    * **[TCGBLaunching addUpdateStatusNotification]**
    * **[TCGBLaunching removeUpdateStatusNotification]**
    * **[TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]**
    * **[TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]**
* Deprecated API
    * **[TCGBGamebase languageCode]**

<a id="130-20180913"></a>
### 1.13.0 (2018.09.13) { #130-20180913 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)

<a id="130-20180913-1"></a>
#### 機能追加

* App Store Promotion IAPをサポートするためのAPIを追加

<a id="130-20180913-2"></a>
#### 機能改善・変更

* IAP SDK最新バージョン適用 (iOS:1.6.0)
* authProviderProfileWithIDPCode APIの呼び出し結果の構造が1depthに変更（Android、Unityと統一）

<a id="122-20180828"></a>
### 1.12.2 (2018.08.28) { #122-20180828 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)

<a id="122-20180828-1"></a>
#### 機能改善・変更

* Google Auth Adapter、NAVER Auth AdapterのCallback URL Scheme設定を改善
    * コンソールに「url_scheme_ios_only」値を設定しない場合、Default URL Schemeを設定するように改善：Default URL Schemeを使用するためには、XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.googleまたはtcgb.{Bundle ID}.naver登録が必要
* PAYCO Auth Adapterの改善
    * URL Scheme未設定により、意図しないURL Schemeを呼び出す問題を修正：設定方法が変更されたため、アップデートするためには必ずURL Scheme設定が必要（XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.paycoを登録）

<a id="121-20180809"></a>
### 1.12.1 (2018.08.09) { #121-20180809 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)

<a id="121-20180809-1"></a>
#### 機能改善・変更

* IAP SDK最新バージョン適用 (1.5.0)
* Gamebaseメンテナンスページでメンテナンス時間を端末設定国時間に合わせて表示するように改善
* メンテナンスページを外部ページとして使用する際に、Consoleに入力したメンテナンス情報を使用できる機能を追加
* IdPマッピングされたユーザーがゲストマッピングを試みた際にエラーが発生（TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP）
* 認証APIを重複して呼び出した際にエラーが発生（AUTH_ALREADY_IN_PROGRESS_ERROR）
* エラーコード追加：Gamecenterログイン拒否（TCGB_ERROR_IOS_GAMECENTER_DENIED）

<a id="121-20180809-2"></a>
#### 不具合修正

* NAVERログイン時、プロフィール情報照会失敗によりログインできないバグを修正：プロフィール情報の照会に失敗してもログインは成功するように変更

<a id="120-20180724"></a>
### 1.12.0 (2018.07.24) { #120-20180724 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)

<a id="120-20180724-1"></a>
#### 機能改善・変更

* Gamebase初期化時にDebug Logに使用中のAdapterのバージョン情報、アプリのビルド情報を出力する機能を追加
* CocoaPodsを通じて配布されるNAVER Auth AdapterからNAVER ID Login SDKのバイナリが削除され、依存性設定方式に変更

<a id="111-20180705"></a>
### 1.11.1 (2018.07.05) { #111-20180705 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)

<a id="111-20180705-1"></a>
#### 機能追加

* LINE IdPを追加

<a id="111-20180705-2"></a>
#### 機能改善・変更

* ゲストログイン後にAddMapping成功時、loginForLastLoggedInPrivderを実行すると、AddMapping成功したIdPアカウントを使用してログインするように変更

<a id="111-20180705-3"></a>
#### 不具合修正

* メンテナンス解除後に後続API（login/push/purchaseなど）が実行されないバグを修正

<a id="110-20180626"></a>
### 1.11.0 (2018.06.26) { #110-20180626 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)

<a id="110-20180626-1"></a>
#### 機能追加
* Google IdPを追加
* Twitter IdPを追加

<a id="110-20180626-2"></a>
#### 機能改善・変更
* LocalizedString日本語翻訳を追加
* 認証APIを呼び出した際に初期化、ログインをしていない場合、明確にエラーコードを区別するように内部ロジックを改善
* NAVER ID Login SDKアップデート：iOS(4.0.10)

<a id="91-20180529"></a>
### 1.9.1 (2018.05.29) { #91-20180529 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)

<a id="91-20180529-1"></a>
#### 不具合修正

* Gamebase WebビューのNavigationBar領域にタイトル、戻る、閉じるボタンが表示されない現象を修正

<a id="90-20180503"></a>
### 1.9.0 (2018.05.03) { #90-20180503 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)

<a id="90-20180503-1"></a>
#### 機能追加
* Transfer機能追加
    * ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    * 追加されたAPI 
        * Transfer Key発行API (IssueTransferKey)
        * 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)

<a id="90-20180503-2"></a>
#### 不具合修正

* NAVERアカウントを利用してログイン中にApp to Webログインを試行時、サーバーから受け取ったSchemeの形式が変更され、ログインされない現象を修正
* AdapterからUnderlyingErrorオブジェクトを受け取ってゲームユーザーに伝達するエラーオブジェクトを作成するロジックで、メッセージおよびUnderlying Errorの設定ができていない問題を修正

<a id="81-20180412"></a>
### 1.8.1 (2018.04.12) { #81-20180412 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)

<a id="81-20180412-1"></a>
#### 不具合修正

* registerPushを呼び出す時、displayLanguageCodeをnullで渡すと、registerPushが失敗する問題を修正

<a id="80-20180405"></a>
### 1.8.0 (2018.04.05) { #80-20180405 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)

<a id="80-20180405-1"></a>
#### 機能追加

* Kick out機能追加
    * 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    * kick outイベントを受け取れるAPIを追加
* メンテナンスWebページに、ユーザーがConsoleで入力したHTMLページを使用できるように機能を改善
    * 以前は、Gamebaseで提供するWebページや外部Webページ接続のみ可能だった
    * Webサーバーがない場合でも、メンテナンスページをユーザーが望む形式で作成できる。
* Observer機能の開発およびAPI追加
    * メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

<a id="80-20180405-2"></a>
#### 機能改善/変更

* Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)
* PAYCO簡単ログイン3rd SDK v1.2.2適用：ログイン成功時、トークン有効期限切れ情報(expires_in)を提供、iPhoneXログインUI改善
* iPhoneXをサポートするために、Webビューを使用したインターフェイスを修正

<a id="80-20180405-3"></a>
#### 不具合修正
* 国コード(contry code)が10文字以上の場合、同時接続データが保存されない現象を修正

<a id="70-20180222"></a>
### 1.7.0 (2018.02.22) { #70-20180222 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)

<a id="70-20180222-1"></a>
#### 機能追加

* NAVER IdP認証追加
* Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

<a id="60-20180125"></a>
### 1.6.0 (2018.01.25) { #60-20180125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)

<a id="60-20180125-1"></a>
#### 不具合修正

* WebViewを呼び出した時、クラッシュが発生することがある部分に対する防御ロジック処理

<a id="50-20171221"></a>
### 1.5.0 (2017.12.21) { #50-20171221 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)

<a id="50-20171221-1"></a>
#### 機能追加

* WebViewが閉じられる時に発生するClose Callbackを追加
* WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
* Unity Setting Tool新規配布

<a id="40-20171123"></a>
### 1.4.0 (2017.11.23) { #40-20171123 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)

<a id="40-20171123-1"></a>
#### 機能改善/変更

* close/backボタンリソースがない時、"x"、"<"などのテキストが表示されていた問題をデフォルト値に変更

<a id="40-20171123-2"></a>
#### 不具合修正

* WebViewローンチ後、端末の回転時にNavigationBar Titleがresetされるエラーを修正
* WebViewのNavigationBar Heightをカスタマイズする時、NavigationBarの背景部分が重なって表示されるエラーを修正

<a id="30-20171026"></a>
### 1.3.0 (2017.10.26) { #30-20171026 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)

<a id="30-20171026-1"></a>
#### 機能追加

* Credentialを利用したAddMapping API追加

<a id="20-20170921"></a>
### 1.2.0 (2017.09.21) { #20-20170921 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)

<a id="20-20170921-1"></a>
#### 機能追加

* 利用停止(ユーザー処罰)機能を追加
* 利用停止ユーザーポップアップ画面を表示

<a id="15-20170720"></a>
### 1.1.5 (2017.07.20) { #15-20170720 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)

<a id="15-20170720-1"></a>
#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* システムポップアップAPIを追加(showAlertWithTitle)
* 国コードを大文字で返すように変更(Android)
* TCPush SDK 1.4.1にアップデート
* IAP SDK 1.3.3.20170627にアップデート

<a id="14-20170525"></a>
### 1.1.4 (2017.05.25) { #14-20170525 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)

<a id="14-20170525-1"></a>
#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* ランタイム中に決済Storeを変更できるAPIを提供

<a id="12-20170404"></a>
### 1.1.2 (2017.04.04) { #12-20170404 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)

<a id="12-20170404-1"></a>
#### 機能改善/変更

* ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善

<a id="10-20170321"></a>
### 1.1.0 (2017.03.21) { #10-20170321 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)

<a id="10-20170321-1"></a>
#### 機能改善/変更

* 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加

<a id="00-20170309"></a>
### 1.0.0 (2017.03.09) { #00-20170309 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-iOS.zip)

<a id="00-20170309-1"></a>
#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
    * 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
    * ログアウトおよび会員退会機能を提供
    * 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
    * ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
    * リアルタイムに運営指標を確認できるWebコンソール画面を提供
    * TOAST Cloudサービスと連携：PUSH、IAP