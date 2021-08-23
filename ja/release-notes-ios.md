## Game > Gamebase > リリースノート > iOS

### 2.27.0 (2021.08.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* PAYCO iOS SDK 업데이트 (1.5.0)
    * PAYCO 앱이 없는 경우 기존 수동 로그인만 가능했던 부분에서 Safari 에 로그인이 되어 있다면 간편로그인 기능을 사용할 수 있도록 변경되었습니다.

### 2.26.0 (2021.08.10) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Display Language機能が改善されました。
    * これまでは言語セットを追加するためにGamebase.bundle内にあるファイルを直接修正する必要がありました。
        * それをXcodeプロジェクトのCopy Bundle Resourcesにlocalizedstring.jsonファイルを追加する方法に変更しました。
    * Display Language言語セットに中国語簡体字(zh-CN)、中国語繁体字(zh-TW)、タイ語(th)が追加されました。
    * デフォルトの言語コードが**en**でしたが、Gamebaseコンソールで設定したデフォルトの言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](./oper-app/#_3)
* showTermsView API呼び出し後に作成することができるPushConfigurationオブジェクトの作成基準が次のように変更されました。
    * 変更前
        * 約款項目中に**Push受信**項目が存在する場合にのみnilではなく有効なPushConfigurationが返されました。
        * ユーザーが昼間、夜間広告性Push受信に全て拒否した場合、PushConfiguration.pushEnabledはfalseで作成されました。
    * 変更後
        * 約款UIが表示されている場合、常にnilではない有効なPushConfigurationが返されます。
        * showTermsViewが返すPushConfigurationオブジェクトのpushEnabled値は常にtrueです。
    * 変更されない点
        * すでに約款に同意して約款UIが表示されない場合はPushConfigurationはnilで返されます。

#### バグ修正
* Push言語設定は特別な補助処理なしで端末の言語コードがそのまま適用され、Pushコンソールから送信したメッセージの言語コードが一致しない問題を修正しました。

### 2.25.0 (2021.07.27) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 月間決済限度機能を追加
    * 月間決済限度を超える場合、**PURCHASE_LIMIT_EXCEEDED(4007)** エラーが発生します。

#### 機能改善/変更
* Push項目が存在する約款で PushConfigurationオブジェクト保証
    * 約款UIでPush受信に同意しない場合、Gamebase.Terms.showTermsView API呼び出し結果として作成されるTCGBPushConfigurationがnullだったが、約款にPush項目が存在する場合、TCGBPushConfigurationオブジェクトが常にリターンされるように変更しました。
    * Push受信を拒否すると、TCGBPushConfigurationオブジェクトは(プッシュ同意 = false、広告性プッシュ同意 = false、夜間広告性プッシュ同意 = false)で作成されます。
    * 約款にPush項目が存在しない場合、TCGBPushConfigurationオブジェクトはnullです。
* 外部SDKアップデート：TOAST iOS SDK(0.29.0)
* Sign In with AppleのASAuthorizationErrorUnknownエラーが発生した場合、 TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERRORエラーをリターンするように変更

#### バグ修正
* registerPushを利用して登録したTCGBPushConfiguration値とTCGBPushTokenInfo値が変わるバグを修正

### 2.24.0 (2021.06.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* 内部ローンチURL変更

#### バグ修正
* 約款詳細表示後、約款ポップアップが閉じないバグを修正

### 2.23.0 (2021.06.14) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* 外部SDKアップデート: TOAST iOS SDK(0.28.0), ToastGamebaseIAP SDK(0.12.0)

### 2.22.0 (2021.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* 外部SDKアップデート: TOAST iOS SDK(0.27.2), Hangame iOS SDK(1.6.0)

### 2.21.2 (2021.04.27) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Facebook iOS SDKアップデート(9.2.0)

#### バグ修正
* アーカイブビルドを行うと、bitcode関連エラーが発生する問題を修正

### 2.21.1 (2021.04.19) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-iOS.zip)

#### バグ修正
* bitcodeをサポートできるように 設定しても設定値が反映されない問題を修正

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Hangame日本認証追加 

#### 機能改善/変更
* bitcodeをサポートできるように変更
* showWebView呼び出し時、閉じるボタンが最初に画面に表示されるように修正

### 2.20.2 (2021.03.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Facebook iOS SDKアップデート(9.1.0)
* 特定の場合においてGamebaseAuthFacebookAdapterでopenURL delegateが呼び出されないイシューを修正

### 2.20.1 (2021.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* iOS 14に対応し、IDFA取得ロジックを修正：info.plistにNSUserTrackingUsageDescriptionフィールドを追加

### 2.20.0 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-iOS.zip)

* 共通約款機能を追加
    * 約款Webビューを開くAPIを追加
    * 約款リストおよびユーザーごとに同意の有無を照会するAPIを追加
    * ユーザーごとに約款の同意有無をGamebaseサーバーに保存するAPIを追加

#### 機能改善/変更
* サポートタイプがTOAST組織商品(Online Contact)の場合、ログインしなくてもサポートが表示されるように変更

### 2.19.1 (2021.01.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Weibo IdPAdapterの構造を変更

### 2021. 01. 12.

```
GamebaseのXcodeの最低サポートバージョンを10から11に変更しました。
```
    
### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Weibo認証追加
    
#### 機能改善/変更

* ローンチステータスコード追加：ベータサービス(205)

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

#### 機能追加

* 開発会社独自のサポートをオープンする時、additionalURLフィールドを追加
* 決済アイテム情報にローカライズされた商品情報を追加：localizedTitle、localizedDescription

#### 機能改善/変更

* 外部SDKアップデート: TOAST iOS SDK(0.27.1)
* showWebView：無効なURLを伝達した場合、エラーを返す。伝達されたURLはエンコードしないでそのまま使用
* 大文字/小文字に関係なくカスタムスキームが動作するように変更


### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* iOS 13以上から提供されるSceneDelegate対応APIを追加

### 2.17.1 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

    * 特定指標の転送時にエラーメッセージを追加して転送：プッシュ登録に失敗した時、ゲーム指標を転送する時

### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)

```
ハンゲーム認証を使用したい場合はサポートへご連絡ください。
```

#### 機能追加

* Hangame IdP認証追加

#### 機能改善/変更

* サポート添付イメージクリック時にダウンロードをサポート
* TCGBMember.regDate, TCGBMember.lastLoginDateのタイプをlong longに変更
* WebビューでURLおよびタイトル変更時、タイトルを再出力できるようにロジック変更

#### バグ修正

* PAYCO認証：lastLoggedInProviderログイン後、ログアウト呼び出し時にログアウトコールバックが来ない問題を修正
    
### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

#### 機能追加

* サポート機能追加
    * API追加(Gamebase.Contact.requestContactURL):サポートURLリターン
    * サポートAPIにuserNameを設定できるようにContactConfigurationパラメータ追加 
        
### 2.15.1 (2020.09.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* 外部SDKアップデート：TOAST iOS SDK(0.27.0)
* iOS 14 beta変更事項に対応したIAP SDK新規バージョンが適用されました。 [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

#### 機能追加

* プッシュトークン登録時、NotificationOption設定でアプリがフォアグラウンド(foreground)状態でもプッシュ通知を受け取れるように機能追加
* プッシュAPI追加：Pushトークン情報確認(Gamebase.Push.queryTokenInfo API)

#### 機能改善/変更

* 外部SDKアップデート：TOAST iOS SDK(0.26.0)
* 決済payloadのnull checkロジック追加
    
### 2.14.0 (2020.08.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* PAYCO IdPの定数値を削除：PAYCO文字列によるApple検収がリジェクトされる場合があり削除
* TCGBWebViewConfigurationにcontentMode設定を追加

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Sign In With Apple認証：iOS 12以下をサポート
    
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * イメージ告知表示APIを追加

#### 機能改善/変更
* Facebook SDKアップデート(7.1.1)
* configuartionに設定されたstoreCode(default=AS)でGamebaseの初期化を試行
* コンテンツをローディングできないWebビューを出力時、閉じるボタンがなくて閉じられない問題を修正
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

#### 機能追加

* 決済API追加：商品IDで決済リクエスト, 追加情報(UserPayload)を入力して決済完了時に確認できる

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* ユーザープッシュ設定の初期化時、言語コードが設定されていない場合、デバイス言語で設定されるように変更

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 既存のすべてのイベントシステムを統合するGamebaseEventHandler追加
    * ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能


### 2.9.1 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)

#### バグ修正

* Unrealエンジンでビルドすると、警告(warning)をビルドエラーと判定してビルドができない問題を修正
        
### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 退会猶予機能
    * API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認
        
#### 機能改善/変更

* 外部SDKアップデート: TOAST iOS SDK(0.24.0)
* PAYCO iOS SDKアップデート(1.4.0)

### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Analytics転送結果を確認するための内部指標追加
    
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

#### 機能追加

* 決済および商品情報に商品タイプおよび地域価格などの情報を追加

#### 機能改善/変更

* コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動することができるポップアップを追加で表示するように改善

### 2.7.1 (2020.02.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* GuestでLoginしてGetAuthProviderUserIDを呼び出した時、値を返すように修正

### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* 外部SDKアップデート: TOAST iOS SDK(0.20.1)
* NAVER iOS SDKアップデート(4.1.0)
    
### 2.6.1 (2019.12.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)
    
#### バグ修正
* AddMapping(強制、 Forcibly)使用時、マッピングされない問題を修正
* Unity PluginでPushConfigurationのdisplayLanguageCodeを設定していない場合、 NSNullオブジェクトによりクラッシュが発生する問題を修正
    

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

#### 機能追加

* データをLog&Crashに転送して各種分析に利用できるようにTOAST Loggerを追加
* Sign In with Apple認証追加

### 2.5.2 (2019.10.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* UIWebViewをWKWebViewに変更

### 2.5.1 (2019.09.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)
    
#### 機能改善/変更

* GamebasePushAdapterで使用中のTCPushSDKを1.7.0にアップデート
    * TCPushSDKがStatic LibraryからFrameworkファイルに変更されたため、プロジェクトにTCPushSDK.frameworkを追加する必要があります。
    
### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Consoleで入力したCS URLをWebビューでオープンするAPIを提供
    
### 2.4.3 (2019.07.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

#### バグ修正

* 認証試行時にエラーが発生した場合、形式に合っていないエラーメッセージ解析試行に伴うクラッシュ発生イシューを修正

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* LaunchingInfoにJSON string形式のTOAST Launching情報を追加
* LINE iOS SDKアップデート(5.0.1)
    * LINE Adpaterの最小サポートOSバージョンをiOS 10に変更 
    * LINEアプリを用いたログイン機能を追加

#### バグ修正

* Analyticsバグ修正：ログアウト、退会、アカウント移行時、保存された指標データを初期化するように修正
* )ネットワーク接続問題により、断続的にクラッシュが発生する現象を修正

### 2.4.1 (2019.06.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

#### バグ修正

* Analytics指標転送時、一部パラメータが抜けて指標が正常に出力されない問題を修正

### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* 指標関連Class変更
    * LevelUpData Class: userLevel, levelUpTimeパラメータが必須に変更 / その他のフィールド削除[詳細表示[iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings)]
    * GameUserData Class: classId(ゲームユーザーの職業)フィールド追加[詳細表示[iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace)]

    
### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Launching Status Code追加："審査中(204)", "テスト中(203)"

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

#### バグ修正

* showBlockingPopupをNOに設定した場合、Gamebase初期化コールバックが呼び出されない問題を修正

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)

#### 機能追加
* TransferAccount機能追加：guestユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行することができる機能
    * 追加されたAPI 
        * TransferAccountInfo発行API (issueTransferAccount)
        * 発行されたTransferAccountInfoを使用してアカウント移行をリクエストするAPI (transferAccountWithIdPLogin)
        * 発行された TransferAccountInfoを確認するAPI (queryTransferAccount)
        * すでに発行された TransferAccountInfoを更新するAPI (renewTransferAccount)        
* 強制マッピング機能追加：すでに他のアカウントに連動されているIdPアカウントをマッピングすることができる機能
    * 追加された API 
        * 強制マッピングするAPI (addMappingForcibly)

#### 機能改善/変更

* LINE iOS SDKのAppログイン機能が無効化
    * LINE SDK v4のバグによりiOS 12でアプリログインが失敗するイシューがあるため、Gamebase Line AdatperでWebログインのみサポートするように変更

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* TransferKey API削除
    * issueTransferKey : TransferKey発行
    * requestTransfer : TransferKey検証
        
#### バグ修正

* GamecenterをGamebaseではなく、他のロジックによりログインした後、Gamebaseを介してGamecenterログインを試行する時、反応がないバグを修正

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

#### 機能追加

* Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
    * setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
    * traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す

#### 機能改善/変更

* IAP SDKアップデート
    * 決済失敗時、断続的にクラッシュが発生する現象を修正

#### バグ修正

* iOS 12以上のシミュレータでdebugMode Onの状態でGamebaseを初期化すると、クラッシュが発生する現象を修正

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Provider Profile取得メソッドを呼び出した時に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にcrashが発生することがある構造を修正

### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Gamebase Webviewでファイル添付機能を追加
    
#### 機能改善/変更
* 利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして伝送し、クライアントでデコードして処理するように修正
* PAYCO iOS SDKアップデート(1.2.4)
* Remove API : Webview, Network, Launching
    * [TCGBUtil showToastWithMessage:duration:]
    * [TCGBWebView showWebBrowserWithURL:viewController:]
    * [TCGBWebView showWebViewWithURL:viewController:configuration:]
    * [TCGBLaunching addObserverOnChangedStatusNotification:]
    * [TCGBLaunching removeObserverOnChangedStatusNotification:]
    * [TCGBLaunching addUpdateStatusNotification]
    * [TCGBLaunching removeUpdateStatusNotification]
    * [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
    * [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
* Deprecated API 
    * [TCGBGamebase languageCode]

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)

#### 機能追加

* App Store Promotion IAPをサポートするためのAPIを追加

#### 機能改善/変更

* IAP SDK最新バージョン適用(iOS:1.6.0)
* authProviderProfileWithIDPCode apiの呼び出し結果の構造が1depthに変更(Android、Unityと同じ)
        
### 1.12.2 (2018.08.28) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Google Auth Adapter、Naver Auth AdapterのCallback URL Scheme設定を改善
    * コンソールに"url_scheme_ios_only"値を設定しない場合、Default URL Schemeを設定するように改善：Default URL Schemeを使用するためには、XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.googleまたはtcgb.{Bundle ID}.naver登録が必要
* Payco Auth Adapter改善
    * URL Schemeの未設定により、意図していないURL Schemeを呼び出す問題を修正：設定方法が変更され、アップデートするためには必ずURL Scheme設定が必要(XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.paycoを登録)

### 1.12.1 (2018.08.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* IAP SDK最新バージョン適用(1.5.0)
* Gamebaseメンテナンスページで、メンテナンス時間を端末設定国時間に合わせて表示するように改善
* メンテナンスページを外部ページで使用する時、Consoleに入力したメンテナンス情報を使用できる機能を追加
* IdPマッピングされたユーザーがゲストマッピング試行時、エラー発生(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
* 認証APIを重複して呼び出した時、エラー発生(AUTH_ALREADY_IN_PROGRESS_ERROR)
* エラーコード追加：Gamecenterログイン拒否(TCGB_ERROR_IOS_GAMECENTER_DENIED)
    
#### バグ修正

* Naverログイン時、プロフィール情報照会失敗により、ログインできない問題を修正：プロフィール情報の照会に失敗してもログインは成功するように変更
    
### 1.12.0 (2018.07.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Gamebaseの初期化時、Debug Logに使用中のAdapterのバージョン情報、アプリのビルド情報を出力する機能を追加
* CocoaPodsを通して配布されるNaver Auth Adapterに含まれていたNaver ID Login SDKのバイナリが削除され、依存性設定方式に変更

### 1.11.1 (2018.07.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)

#### 機能追加

* Line IdP追加

#### 機能改善/変更

* ゲストログイン後にAddMapping成功時、loginForLastLoggedInPrivderをすると、AddMapping成功したIdPアカウントを使用してログインするように変更
    
#### バグ修正

* メンテナンス解除後にAPI進行(login/push/purchaseなど)ができない問題を修正

### 1.11.0 (2018.06.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Google IdP追加
* Twitter IdP追加
    
#### 機能改善/変更
* LocalizedString日本語翻訳を追加
* 認証APIを呼び出した時に初期化、ログインをしない場合は明確にエラーコードを区別するように内部ロジックを改善
* Naver ID Login SDKアップデート: iOS(4.0.10)
    
### 1.9.1 (2018.05.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)

#### バグ修正

* Gamebase WebView NavigationBar領域にタイトル、戻る、閉じるボタンが表示されない現象を修正
    
### 1.9.0 (2018.05.03) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Transfer機能追加
    * ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    * 追加された API 
        * Transfer Key発行API (IssueTransferKey)
        * 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)

#### バグ修正

* Naverアカウントを利用してログイン中にApp to Webログインを試行時、サーバーから受け取ったSchemeの形式が変更され、ログインされない現象を修正
* AdapterからUnderlyingErrorオブジェクトを受け取ってゲームユーザーに伝達するエラーオブジェクトを作成するロジックで、メッセージおよびUnderlying Errorの設定ができていない問題を修正

### 1.8.1 (2018.04.12) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)

#### バグ修正

* registerPushを呼び出す時、displayLanguageCodeをnullで渡すと、registerPushが失敗する問題を修正

### 1.8.0 (2018.04.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Kick out機能追加
    * 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    * kick outイベントを受け取れるAPIを追加
* メンテナンスWebページに、ユーザーがConsoleで入力したHTMLページを使用できるように機能を改善
    * 以前は、Gamebaseで提供するWebページや外部Webページ接続のみ可能だった
    * Webサーバーがない場合でも、メンテナンスページをユーザーが望む形式で作成できる。
* Observer機能開発およびAPI追加
    * メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

#### 機能改善/変更

* Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)
* PAYCO簡単ログイン3rd SDK v1.2.2適用：ログイン成功時、トークン有効期限切れ情報(expires_in)を提供、iPhoneXログインUI改善
* iPhoneXをサポートするために、Webviewを使用したインターフェイスを修正

#### バグ修正
* 国コード(contry code)が10文字以上の場合、同時接続データが保存されない現象を修正

### 1.7.0 (2018.02.22) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Naver IdP認証追加
* Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

### 1.6.0 (2018.01.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)

#### バグ修正

* WebViewを呼び出す時、クラッシュが発生することがある部分に対する防御ロジック処理

### 1.5.0 (2017.12.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)

#### 機能追加

* WebViewが閉じられる時に発生するClose Callbackを追加
* WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
* Unity Setting Tool新規配布

### 1.4.0 (2017.11.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* close/backボタンリソースがない時、'x', '<'などのテキストが表示されていた問題をデフォルト値に変更

#### バグ修正

* WebViewローンチ後、端末の回転時にNavigationBar Titleがresetされるエラーを修正
* WebViewのNavigationBar Heightをカスタマイズする時、NavigationBarの背景部分が重なって表示されるエラーを修正

### 1.3.0 (2017.10.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)

#### 機能追加

* Credentialを利用したAddMapping API追加

### 1.2.0 (2017.09.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)

#### 機能追加

* 利用停止(ユーザー処罰)機能を追加
* 利用停止ユーザーポップアップ表示

### 1.1.5 (2017.07.20) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* システムポップアップAPIを追加(showAlertWithTitle)
* 国コードを大文字で返すように変更(Android)
* TCPush SDK 1.4.1にアップデート
* IAP SDK 1.3.3.20170627にアップデート

### 1.1.4 (2017.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* ランタイム中に決済Storeを変更できるAPIを提供

### 1.1.2 (2017.04.04) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善

### 1.1.0 (2017.03.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更

* 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-iOS.zip)

#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
    * さまざまな認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
    * ログアウトおよび会員退会機能を提供
    * 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
    * ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
    * リアルタイムに運営指標を確認できるWebコンソール画面を提供
    * TOAST Cloudサービスと連携：PUSH、IAP
