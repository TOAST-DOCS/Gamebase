## Game > Gamebase > リリースノート > iOS

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* 外部SDKアップデート：Hangame iOS SDK (1.6.4)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Display Languageの中国語繁体字(zh-TW)言語セットで不自然な文章を修正

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-iOS.zip)

#### 機能追加
* サポートURLの後ろにパラメータを追加できるように次のフィールドが追加されました。
    * **TCGBContactConfiguration.additionalParameters**

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 決済領収書でsandboxおよびプロモーション決済なのかどうかを知ることができるように、次のフィールドが追加されました。
    * **TCGBPurchasableReceipt.sandboxPayment**
    * **TCGBPurchasableReceipt.promotionPayment**

#### 機能改善/変更
* 外部SDKアップデート：TOAST iOS SDK(0.30.0)、ToastGamebaseIAP SDK(0.13.0)、Hangame iOS SDK (1.6.3)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 現在約款ウィンドウが画面に表示されているかどうかを知ることができるAPIを追加しました。
    * **[TCGBTerms isShowingTermsView]**

#### 機能改善/変更
* Google Webログイン方式からGoogle SDKログイン方式に変更しました。
* ハンゲームログインを途中でキャンセルした場合、**TCGB_ERROR_AUTH_USER_CANCELED(3001)**エラーを返すように修正しました。

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

#### 機能追加
* SwiftプロジェクトユーザーのためにPublic APIにNS_SWIFT_NAME設定を追加しました。

#### 機能改善/変更
* 外部SDKアップデート：Hangame iOS SDK (1.6.2)
* デバイスが横モードの状態でshowWebView APIを呼び出したとき、下部に黒い空スペースが表示されるエラーを修正しました。

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Gamebaseコンソールのアップデート必須設定で「ポップアップボタン追加」項目を選択すると、クライアントのアップデート必須ポップアップに「詳細表示」ボタンが追加されます。
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* 共通約款API呼び出し後、約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * **TCGBShowTermsViewResult**

#### 機能改善/変更
* イメージ告知APIを呼び出したときに表示するイメージ告知がない場合、背景が少しの間暗くなる現象を修正しました。
* キックアウトポップアップの表示有無はGamebaseコンソールでキックアウト登録時に設定することができるため、次のAPIがdeprecatedになりました。
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**
    
### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 共通約款ウィンドウの設定を変更できる新規APIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > UI > Terms > showTermsView](./ios-ui/#showtermsview)

#### 機能改善/変更
* 外部SDKアップデート: PAYCO iOS SDK (1.5.5)
* エラーコード追加および変更
    * TCGB_ERROR_UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたTCGB_ERROR_SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。
    
### 2.32.1 (2022.01.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* アップデート推奨ポップアップの**今アップデート**ボタンクリックしたとき、ポップアップが終了しないように修正しました。
* SDKの安定性を改善しました。

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

#### 機能追加
* GamebaseEventHandlerのGamebaseEventCategoryに**kTCGBServerPushAppKickoutMessageReceived**タイプが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* GamebaseEventHandlerのGamebaseEventCategoryに**kTCGBLoggedOut**タイプが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

#### 機能改善/変更
* WebビューnavigationBarの基本タイトル色を**UIColor.whiteColor**に変更しました。

#### バグ修正
* Hangameログアウトを呼び出した時、thirdIdPもログアウトするように修正しました。

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

#### 機能追加
* メンテナンスポップアップでメンテナンス時間を表示するかどうかを動的に設定できるようにしました。

#### 機能改善/変更
* 外部SDKアップデート：TOAST iOS SDK (0.29.2), PAYCO iOS SDK (1.5.4)
* 利用停止Webビュー内のサポートリンクから利用停止ユーザー情報でお問い合わせを登録することができない問題を修正しました。
* メンテナンスポップアップ、利用停止詳細表示Webビューで戻るボタンが表示されるように修正しました。

### 2.30.1 (2021.11.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

#### バグ修正
* Unity 2019.3以上でCocoapodsをインストールした時、決済とプッシュAPIでエラーが発生するバグを修正しました。

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 強制マッピングを行う時、IdPログインをもう一度試行しなければいけない煩わしさを改善した、新しい強制マッピングAPIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* 特定IdPでマッピング試行後、**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**エラーが発生した時、該当IdPにアカウントを変更することができるAPIが追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > Change Login with ForcingMappingTicket](./ios-authentication/#change-login-with-forcingmappingticket)

#### バグ修正
* loginForLastLoggedInProviderログイン後、特定IdPでログアウトまたは退会機能が動作しないバグを修正しました。

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Xcode最小サポートバージョンが12から13に変更されました。
* 外部SDKアップデート：TOAST iOS SDK(0.29.1), ToastGamebaseIAP SDK(0.12.1)
* コンソールに登録したメンテナンスおよび告知詳細表示のURLをエンコードせずに画面に表示するように変更しました。

#### バグ修正
* TCGBPushMessage.extrasをjson解析する時にエラーが発生する問題を修正しました。

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Kakaogame認証を追加
* 「決済アビューズ自動解除」機能が追加されました。
    * [Game > Gamebase > iOS SDK使用ガイド > 認証 > GraceBan](./ios-authentication/#graceban)
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にTCGBAuthToken.tcgbMember.graceBanInfo値を確認し、nullではない有効なTCGBGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

#### 機能改善/変更
* PAYCO iOS SDKアップデート(1.5.2)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* PAYCO iOS SDKアップデート(1.5.1)
    * 認証フローおよびUI改善
* Hangame iOS SDKアップデート(1.6.1)
    * 本人認証でエラーが発生した時、コールバックの呼び出しができないイシューを修正
    * iOS 15 betaでナビゲーションバーが正常に表示されないイシューを修正

#### バグ修正
* すでに約款に同意して約款UIが表示されない場合、 PushConfigurationがnilでリターンされないイシューを修正しました。

### 2.27.0 (2021.08.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* PAYCO iOS SDKアップデート(1.5.0)
    * PAYCOアプリがない時、以前は手動ログインのみ可能でしたが、Safariにログインしている場合は、簡単ログイン機能を使用できるようにしました。

#### バグ修正
* Unityで画像告知が表示されない問題を修正しました。
    * Gamebase iOS SDK 2.27.0未満を使用する場合、Unityで画像告知が表示されないことがあります。
    * 画像告知を使用する場合は、Gamebase iOS SDK 2.27.0以上を使用してください。
    
### 2.26.0 (2021.08.10) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* Display Language機能が改善されました。
    * これまでは言語セットを追加するためにGamebase.bundle内にあるファイルを直接修正する必要がありました。
        * それをXcodeプロジェクトのCopy Bundle Resourcesにlocalizedstring.jsonファイルを追加する方法に変更しました。
    * Display Language言語セットに中国語簡体字(zh-CN)、中国語繁体字(zh-TW)、タイ語(th)が追加されました。
    * デフォルトの言語コードが**en**でしたが、Gamebaseコンソールで設定したデフォルトの言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](https://docs.toast.com/en/Game/Gamebase/en/oper-app/#language-settings)
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
* Hangame日本認証を追加

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

#### 機能追加
* 共通約款機能追加
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
* [SDK] 2.19.0
    * (共通) Weibo認証を追加
    
#### 機能改善/変更
* [SDK] 2.19.0
    * (共通)ローンチステータスコード追加：ベータサービス(205)

#### バグ修正 
* [SDK] 2.19.0
    * (Unity) WebSocketで再試行した時、 OutOfMemoryExceptionが発生する問題を修正
* [SDK] 2.19.1
    * (Android) Weiboログイン試行後、他のIdPでログイン時、クラッシュが発生する問題を修正

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

#### 機能追加
* Gamebaseサポートページオープン時、ゲームで定義したextra data伝達：SDK 2.18.2
    * [Console]サポート > 顧客お問い合わせ：顧客お問い合わせ詳細照会画面において追加で登録したextra dataを確認可能
* [SDK] 2.18.2
    * (共通)開発会社が独自のサポートをオープンする時、additionalURLフィールドを追加
    * (共通)決済アイテム情報にローカライズされた商品情報を追加：localizedTitle, localizedDescription

#### 機能改善/変更
* [SDK] 2.18.2
    * (共通) TOAST SDKアップデート: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
    * (iOS) showWebView：無効なURLを伝達した場合、エラーを返されたURLはエンコードせず、そのまま使用
    * (iOS)大文字/小文字に関係なく、カスタムスキームが動作するように変更

### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.18.0
    * (iOS) iOS 13以上から提供されるSceneDelegate対応APIを追加

### 2.17.1 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.17.1
    * (iOS)特定指標の転送時にエラーメッセージを追加して転送：プッシュ登録に失敗した時、ゲーム指標を転送する時
    
### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)

```
ハンゲーム認証を使用したい場合はサポートへご連絡ください。
```

#### 機能追加
* Hangame IdP認証追加：SDK 2.17.0

#### 機能改善/変更
* [SDK] 2.17.0
    * (共通)サポート添付イメージクリック時、ダウンロードサポート
    * (共通) TOAST SDKアップデート：Android(0.23.2), Unity(0.21.2)
    * (iOS) TCGBMember.regDate、TCGBMember.lastLoginDateのタイプをlong longに変更
    * (iOS) WebビューからURLおよびタイトルを変更した時、タイトルを再出力できるようにロジックを変更

#### バグ修正
* [SDK] 2.17.0
    * (iOS) PAYCO認証：lastLoggedInProviderログイン後、ログアウトを呼び出した時、ログアウトコールバックが来ない問題を修正
    
    
### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

#### 機能追加
* サポート機能追加
    * [SDK] 2.16.0
        * (共通) API追加(Gamebase.Contact.requestContactURL)：サポートURLリターン
        * (共通)サポートAPIにuserNameを設定できるようにContactConfigurationパラメータを追加 

### 2.15.1 (2020.09.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.15.1
    * (iOS) TOAST SDKアップデート：iOS(0.27.0)
    * iOS 14 beta変更事項を対応したIAP SDK新規バージョンが適用されました。 [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

```
Gamebase SDK 2.15.0バージョンでGoogle Billing Clientモジュールがアップデートされました。

gamebase-adapter-purchase-googleを使用する場合、Gamebase SDK 2.15.0未満バージョンから2.15.0以上にアップグレードするには
前バージョンの「Game Client Version」を「アップデート必須」に設定する必要があります。

アイテムを購入中にエラーが発生すると再処理を行いますが
複数の端末で異なるBilling Clientバージョンが適用された状態では再処理中に問題が発生することがあるためです。
```

#### 機能追加
* [SDK] 2.15.0
    * (共通)プッシュトークン登録時に、アプリのNotificationOption設定がForeground状態でもプッシュ通知を受け取れるように機能追加
    * (共通)プッシュAPI追加：Pushトークン情報確認(Gamebase.Push.queryTokenInfo API)

#### 機能改善/変更
* [SDK] 2.15.0
    * (共通) TOAST SDKアップデート: Android(0.23.0)、iOS(0.26.0)、Unity(0.21.0)
    * (iOS)決済payloadのnull checkロジック追加

### 2.14.0 (2020.08.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.14.0
    * (iOS) PAYCO IdPの定数値を削除：PAYCO文字列によるApple検収がリジェクトされる場合があり削除
    * (iOS、Unity) TCGBWebViewConfigurationにcontentMode設定を追加

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.13.0
    * (iOS) Sign In With Apple認証：iOS 12以下をサポート
   
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * [SDK] 2.12.0：イメージ告知表示APIを追加

#### 機能改善/変更
* [SDK] 2.12.0
    * (iOS)Facebook SDKアップデート(7.1.1)
    * (iOS)configuartionに設定されたstoreCode(default=AS)でGamebaseの初期化を試行
    * (iOS)コンテンツをローディングできないWebビューを出力時、閉じるボタンがなくて閉じられない問題を修正
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 2.11.0
    * 決済API追加：商品IDで決済リクエスト, 追加情報(UserPayload)を入力して決済完了時に確認できる

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.10.1
    * (iOS)ユーザープッシュ設定の初期化時、言語コードが設定されていない場合、デバイス言語で設定されるように変更

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 2.10.0
    * (共通)既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
        * ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

### 2.9.1 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)

#### バグ修正
* [SDK] 2.9.1
    * (iOS) Unrealエンジンでビルドすると、警告(warning)をビルドエラーと判定してビルドができない問題を修正

### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

#### 機能追加
* 退会猶予機能
    * [SDK] 2.9.0
        *(共通)API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認

#### 機能改善/変更
* [SDK] 2.9.0
    * (共通) TOAST SDKアップデート： Android(v0.21.0)、iOS(v0.23.0)、Unity(0.20.1)
    * (共通) PAYCO Login SDKアップデート： Android(v1.5.0)、iOS(v1.4.0)
    
### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.8.1 
    * (共通) Analytics転送結果を確認するための内部指標を追加
    
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 2.8.0
    * (共通)決済および商品情報に商品タイプおよび地域価格などの情報を追加

#### 機能改善/変更
* [SDK] 2.8.0 
    * (共通)コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップが表示されるように改善
    * (Android)ログイン直後に決済関連APIを呼び出す時、初期化タイミングの問題で失敗する場合があるコードを修正

### 2.7.1 (2020.02.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.7.1
    * (Common) GuestでLoginしてGetAuthProviderUserIDを呼び出した時、値を返すように修正
    
### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)
#### 機能追加
* クーポン > クーポン発行：キーワードクーポン機能を追加
#### 機能改善/変更
* [SDK] 2.6.2
    * (共通) TOAST SDKアップデート: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
    * (iOS) NAVER SDKバージョンをアップデート(4.1.0)

### 2.6.1 (2019.12.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)

#### 機能追加
* アプリ > アプリ：メンテナンス中にQAテスト端末を登録すると、IPでも登録できる機能を追加

#### バグ修正
* [SDK] 2.6.1
    * (iOS)AddMapping(強制、Forcibly)使用時、マッピングされない問題を修正
    * (iOS)Unity PluginにPushConfigurationのdisplayLanguageCodeを設定していない場合、NSNullオブジェクトによりクラッシュが発生する問題を修正

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

```
Gamebase SDK 2.6.0未満バージョンから2.6.0にアップグレードする場合
必ずUpgrade Guide文書に記載された変更事項を反映してください。 
ガイドの場所：Game > Gamebase > Upgrade Guide
```
#### 機能追加
* [SDK] 2.6.0
    * (共通)データをLog&Crashに転送して、各種分析に利用できるようにTOAST Loggerを追加
    * (iOS) Sign In with Apple認証を追加

### 2.5.2 (2019.10.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.5.2
    * (iOS) UIWebViewをWKWebViewに変更

### 2.5.1 (2019.09.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.5.1
    * (iOS) GamebasePushAdapterで使用中のTCPushSDKを1.7.0にアップデート
        * TCPushSDKが、Static LibraryからFrameworkファイルに変更されたため、プロジェクトにTCPushSDK.frameworkを追加する必要があります。

### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 2.5.0
    * Consoleで入力したCS URLをWebビューで開くAPIを提供

### 2.4.3 (July 11, 2019 )
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

#### Bug Fixes 
* [SDK] 2.4.3
    * (iOS) Fixed crash occurrence due to parsing attempts of error messages with conflicting formats, regarding authentication

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.4.2
    * (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加
    * (iOS)LINE SDKアップデート(v5.0.1)
        * LINE Adpaterの対応OSバージョンがiOS 10以上に変更
        * LINEアプリによるログイン機能追加

#### バグ修正
* [SDK] 2.4.2
    * (共通)Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正
    * (iOS)ネットワーク接続問題により、断続的にクラッシュが発生する現象を修正

### 2.4.1 (2019.06.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

#### バグ修正
* [SDK] 2.4.1
    * (iOS)Analytics指標転送時、一部パラメータが抜けて指標が正常に出力されない問題を修正
    
### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.4.0
    * (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#game-user-data-settings)]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#level-up-trace)]
    * (Android)NAVER SDKバージョンアップデート(v4.2.5)：NAVER SDKのバグを修正(NAVERログイン中にアプリアイコンからアプリを再起動した場合、Activityが強制終了する問題により、認証プロセスが中断される問題を解決)
    * (Unity)StandaloneWebviewが32bit Buildをサポート(SDK容量53.6MBから99.2MBに増加)

### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

```
Gamebaseを使用すると、10数個の中国ストアと連携が可能です。
中国でのリリースに関心がある方は、サポートへご連絡ください。
```

#### 機能改善/変更
* [SDK] 2.3.0
    * (共通)Launching Status Code追加："審査中(204)"、"テスト中(203)"

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

#### バグ修正
* [SDK] 2.2.2
    * (iOS)showBlockingPopupをNOに設定した場合、Gamebase初期化コールバックが呼び出されない問題を修正

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)
#### 機能追加
* TransferAccount機能追加：ゲストユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行できる機能
    * (SDK共通)追加されたAPI 
        * TransferAccountInfo発行API (issueTransferAccount)
        * 発行されたTransferAccountInfoを使用して、アカウント移行をリクエストするAPI (transferAccountWithIdPLogin)
        * 発行されたTransferAccountInfoを確認するAPI (queryTransferAccount)
        * すでに発行されたTransferAccountInfoを更新するAPI (renewTransferAccount)        
* 強制マッピング機能を追加：すでに他のアカウントに連携しているIdPアカウントをマッピングできる機能
    * (SDK共通)追加されたAPI 
        * 強制マッピングするAPI (addMappingForcibly)

#### 機能改善/変更
* [SDK] 2.2.0
    * (iOS)LINE SDKのAppログイン機能が無効化
        * LINE SDK v4の問題により、iOS 12でアプリログインが失敗する問題があり、Gamebase Line AdatperでWebログインのみサポートするように変更

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 2.1.0
    * (共通)TransferKey API削除
        * issueTransferKey：TransferKey発行
        * requestTransfer：TransferKey検証
        
#### バグ修正
* [SDK] 2.1.0
    * (iOS)GamecenterをGamebaseではない別のロジックによりログインした後、Gamebaseを通してGamecenterログインを試行した時、反応がない問題を修正

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

#### 機能追加
* [SDK] 2.0.0
    * (共通)Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
        * setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
        * traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す

#### 機能改善/変更
* [SDK] 2.0.0
    * (iOS)IAP SDKアップデート
        * 決済失敗時、断続的にクラッシュが発生する現象を修正

#### バグ修正
* [SDK] 2.0.0
    * (iOS)iOS 12以上のシミュレータでdebugMode Onの状態でGamebaseを初期化すると、クラッシュが発生する現象を修正

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 1.14.2
    * (iOS)Provider Profile取得メソッドを呼び出した時に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にcrashが発生することがある構造を修正

### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 1.14.0
    * (共通)Gamebase Webviewでファイル添付機能を追加：AndroidのAPI 19、Kitcatでは正常に動作しません。

#### 機能改善/変更
* [SDK] 1.14.0
    * (共通)利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして伝送し、クライアントでデコードして処理するように修正
    * (iOS)PAYCO SDKのバージョンを1.2.4にアップデート
    * Remove API：Webview、Network、Launching
        * [TCGBUtil showToastWithMessage:duration:]
        * [TCGBWebView showWebBrowserWithURL:viewController:]
        * [TCGBWebView showWebViewWithURL:viewController:configuration:]
        * [TCGBLaunching addObserverOnChangedStatusNotification:]
        * [TCGBLaunching removeObserverOnChangedStatusNotification:]
        * [TCGBLaunching addUpdateStatusNotification]
        * [TCGBLaunching removeUpdateStatusNotification]
        * [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
        * [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
    * Deprecated  API 
        * (iOS)1個
            * [TCGBGamebase languageCode]

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)

#### 機能追加
* [SDK] 1.13.0
    * (iOS)App Store Promotion IAPをサポートするためのAPIを追加

#### 機能改善/変更
* [SDK] 1.13.0
    * (共通)IAP SDK最新バージョン適用(android:1.5.1、iOS:1.6.0)
    * (iOS)authProviderProfileWithIDPCode apiの呼び出し結果の構造が1depthに変更(Android、Unityと同じ)

### 1.12.2 (2018.08.28) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 1.12.2
    * (iOS)Google Auth Adapter、NAVER Auth AdapterのCallback URL Scheme設定を改善
        * コンソールに"url_scheme_ios_only"値を設定しない場合、Default URL Schemeを設定するように改善：Default URL Schemeを使用するためには、XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.googleまたはtcgb.{Bundle ID}.naver登録が必要
    * (iOS)PAYCO Auth Adapterの改善
        * URL Schemeの未設定により、意図していないURL Schemeを呼び出す問題を修正：設定方法が変更され、アップデートするためには必ずURL Scheme設定が必要(XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.paycoを登録)

### 1.12.1 (2018.08.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 1.12.1
    * (共通)IAP SDK最新バージョン適用(1.5.0)
    * (共通)Gamebaseメンテナンスページで、メンテナンス時間を端末設定国時間に合わせて表示するように改善
    * (共通)メンテナンスページを外部ページで使用する時、Consoleに入力したメンテナンス情報を使用できる機能を追加
    * (共通)IdPマッピングされたユーザーがゲストマッピング試行時、エラー発生(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (共通)認証APIを重複して呼び出した時、エラー発生(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (iOS)エラーコード追加：Gamecenterログイン拒否(TCGB_ERROR_IOS_GAMECENTER_DENIED)

#### バグ修正
* [SDK] 1.12.1
    * (iOS)NAVERログイン時、プロフィール情報照会失敗により、ログインできない問題を修正：プロフィール情報の照会に失敗してもログインは成功するように変更    
    
### 1.12.0 (2018.07.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)

#### 機能改善/変更
* [SDK] 1.12.0
    * (iOS)Gamebaseの初期化時、Debug Logに使用中のAdapterのバージョン情報、アプリのビルド情報を出力する機能を追加
    * (iOS)CocoaPodsを通して配布されるNAVER Auth Adapterに含まれていたNAVER ID Login SDKのバイナリが削除され、依存性設定方式に変更
    
### 1.11.1 (2018.07.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)

#### 機能追加
* LINE IdP追加：iOS

#### 機能改善/変更
* [SDK] 1.11.1
    * (共通)ゲストログイン後にAddMapping成功時、loginForLastLoggedInPrivderをすると、AddMapping成功したIdPアカウントを使用してログインするように変更
    
#### バグ修正
* [SDK] 1.11.1
    * (共通)メンテナンス解除後にAPI進行(login/push/purchaseなど)ができない問題を修正

### 1.11.0 (2018.06.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)

#### 機能追加
* iOS Google IdP追加：iOS
* Twitter IdP追加：Android、iOS
* LINE IdP追加：Androidのみ提供。iOSは2018年7月に提供予定です。
* Server API追加
    * getSimpleLaunching：クライアントアプリ起動時に提供されるLaunching情報の確認用API
    
#### 機能改善/変更
* [SDK] 1.11.0
    * (共通)LocalizedString日本語翻訳を追加
    * (共通)認証APIを呼び出した時に初期化、ログインをしない場合は明確にエラーコードを区別するように内部ロジックを改善
    * NAVER ID Login SDKアップデート：iOS(4.0.10)
* Sample App 
    * ServerPush機能およびObserver機能を追加
    * Gamebase SDKアップデート：Android(1.9.0)、iOS(1.9.0)、Unity(1.10.1)    

### 1.9.1 (2018.05.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)

#### バグ修正
* [SDK] 1.9.1
    * (iOS) Gamebase WebView NavigationBar領域にタイトル、戻る、閉じるボタンが表示されない現象を修正
  
### 1.9.0 (2018.05.03) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Transfer機能追加
    * ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    * (SDK共通)追加されたAPI 
        * Transfer Key発行API (IssueTransferKey)
        * 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)
* 利用停止の登録時、ユーザーのリーダーボード(ランキング)データを削除できるオプションを追加(TOAST Leaderboardを使用する場合に限る)
    * 利用停止登録メニューまたは、App Guard連携ページで使用可能

#### バグ修正
* [SDK] 1.9.0
    * (iOS) NAVERアカウントを利用してログイン中にApp to Webログインを試行時、サーバーから受け取ったSchemeの形式が変更され、ログインされない現象を修正
    * (iOS) AdapterからUnderlyingErrorオブジェクトを受け取ってゲームユーザーに伝達するエラーオブジェクトを作成するロジックで、メッセージおよびUnderlying Errorの設定ができていない問題を修正

### 1.8.1 (2018.04.12) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)
#### バグ修正
* [SDK] 1.8.1
    * (Android. iOS)registerPushを呼び出す時、displayLanguageCodeをnullで渡すと、registerPushが失敗する問題を修正

### 1.8.0 (2018.04.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)

#### 機能追加
* Kick out機能追加
    * 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    * (SDK共通)kick outイベントを受け取れるAPIを追加
* メンテナンスWebページに、ユーザーがConsoleで入力したHTMLページを使用できるように機能を改善
    * 以前は、Gamebaseで提供するWebページや外部Webページ接続のみ可能だった
    * Webサーバーがない場合でも、メンテナンスページをユーザーが望む形式で作成できる。
* Observer機能の開発およびAPI追加
    * (SDK共通)メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

#### 機能改善/変更
* [SDK] 1.8.0
    * (共通)Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)
    * (iOS)PAYCO簡単ログイン3rd SDK v1.2.2適用：ログイン成功時、トークン有効期限切れ情報(expires_in)を提供、iPhoneXログインUI改善
    * (iOS)iPhoneXをサポートするために、Webviewを使用したインターフェイスを修正

#### バグ修正
* 国コード(contry code)が10文字以上の場合、同時接続データが保存されない現象を修正

### 1.7.0 (2018.02.22) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)
#### 機能追加
* [SDK] 1.7.0
    * NAVER IdP認証追加
    * Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

### 1.6.0 (2018.01.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)
#### バグ修正
* [SDK] 1.6.0
    * (iOS)WebViewを呼び出した時、クラッシュが発生することがある部分に対する防御ロジック処理

### 1.5.0 (2017.12.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)
#### 機能追加
* [SDK] 1.5.0
    * WebViewが閉じられる時に発生するClose Callbackを追加
    * WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
    * Unity Setting Tool新規配布

### 1.4.0 (2017.11.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)
#### 機能改善/変更
* [SDK] 1.4.0アップデート
    * (iOS)close/backボタンリソースがない時、'x', '<'などのテキストが表示されていた問題をデフォルト値に変更

#### バグ修正
* [SDK] 1.4.0アップデート
    * (iOS)WebViewローンチ後、端末の回転時にNavigationBar Titleがresetされるエラーを修正
    * (iOS)WebViewのNavigationBar Heightをカスタマイズする時、NavigationBarの背景部分が重なって表示されるエラーを修正

### 1.3.0 (2017.10.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)
#### 機能追加
* [SDK] 1.3.0アップデート
    * Credentialを利用したAddMapping API追加

### 1.2.0 (2017.09.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)
#### 機能追加
* 利用停止(ユーザー処罰)機能を追加
* [SDK] 1.2.0アップデート
    * 利用停止ユーザーポップアップ表示

### 1.1.5 (2017.07.20) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)
#### 機能改善/変更
* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.5アップデート
    * システムポップアップAPIを追加(showAlertWithTitle)
    * 国コードを大文字で返すように変更(Android)
    * TCPush SDK 1.4.1にアップデート
    * IAP SDK 1.3.3.20170627にアップデート

### 1.1.4 (2017.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)
#### 機能改善/変更
* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.4アップデート
    * ランタイムのうち、決済Storeを変更できるAPIを提供

### 1.1.2 (2017.04.04) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)
#### 機能改善/変更
* [SDK] 1.1.2アップデート
    * ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善
    * Unity Pluginデバッグログ追加および例外詳細処理

### 1.1.0 (2017.03.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)
#### 機能改善/変更
* [SDK] 1.1.0アップデート
    * 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加
    * [UI機能追加](./aos-ui)：Custom Webview、AlertDialog

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-iOS.zip)

#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
    * 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
    * ログアウトおよび会員退会機能を提供
    * 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
    * ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
    * リアルタイムに運営指標を確認できるWebコンソール画面を提供
    * TOAST Cloudサービスと連携：PUSH、IAP
