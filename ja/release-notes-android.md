## Game > Gamebase > リリースノート > Android

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* Display Language機能が改善されました。
    * これまでは言語セットを追加するためにgamebase-sdk-base-version.aarファイルを直接修正する必要がありました。
        * それがプロジェクトのres/rawフォルダにlocalizedstring.jsonファイルを追加する方法に変わりました。
    * これまではUnityガイドのDisplay Language言語セットの追加方法はAndroidに適用できませんでした。
        * それをUnityガイドにしたがってlocalizedstring.jsonファイルを追加してもAndroidビルドに反映されるように改善しました。
        * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Display Language > 新規言語セット追加](./unity-etc/#_1)
    * Display Language言語セットに中国語簡体字(zh-CN)、中国語繁体字(zh-TW)、タイ語(th)が追加されました。
    * デフォルトの言語コードが**en**でしたが、Gamebaseコンソールで設定したデフォルトの言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](./oper-app/#_3)
* showTermsView API呼び出し後に作成することができるPushConfigurationオブジェクトの作成基準が次のように変更されました。
    * 変更前
        * 約款項目中に**Push受信**項目が存在する場合にのみnullではなく有効なPushConfigurationが返されました。
        * ユーザーが昼間、夜間広告性Push受信に全て拒否した場合、PushConfiguration.pushEnabledはfalseで作成されました。
    * 変更後
        * 約款UIが表示されている場合、常にnullではない有効なPushConfigurationが返されます。
        * showTermsViewが返すPushConfigurationオブジェクトのpushEnabled値は常にtrueです。
    * 変更されない点
        * すでに約款に同意して約款UIが表示されない場合はPushConfigurationはnullで返されます。

#### バグ修正
* Push言語設定は特別な補助処理なしで端末の言語コードがそのまま適用され、Pushコンソールから送信したメッセージの言語コードが一致しない問題を修正しました。

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Android.zip)

#### 機能追加
* 月間決済限度機能を追加
    * 月間決済限度を超える場合、**PURCHASE_LIMIT_EXCEEDED(4007)** エラーが発生します。

#### 機能改善/変更
* Android Support Library依存性をAndroidXに変更
* Push項目が存在する約款で PushConfigurationオブジェクト保証
    * 約款UIでPush受信に同意しない場合、Gamebase.Terms.showTermsView API呼び出し結果として作成されるPushConfigurationがnullだったが、約款にPush項目が存在する場合、PushConfigurationオブジェクトが常にリターンされるように変更しました。
    * Push受信を拒否すると、PushConfigurationオブジェクトは(プッシュ同意 = false、広告性プッシュ同意 = false、夜間広告性プッシュ同意 = false)で作成されます。
    * 約款にPush項目が存在しない場合、PushConfigurationオブジェクトはnullです。
* 外部SDKアップデート
    * TOAST Android SDK(0.26.0)
    * Kotlin(1.5.21)
    * Google Play Services Auth(19.0.0)
    * Facebook Android SDK(11.1.0)
    * Naver Android SDK(4.4.1)
    * Line Android SDK(5.6.2)
    * Weibo Android SDK(11.6.0)
* Weiboログイン時に発生するクラッシュを修正

### 2.24.0 (2021.06.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* 内部ローンチURL変更
* SDK添付文書に誤って作成された文言を修正

### 2.23.0 (2021.06.14) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Android.zip)

#### バグ修正
* 利用停止詳細表示Webビューのタイトルが表示されない問題を修正

### 2.22.0 (2021.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* 外部SDKアップデート：TOAST Android SDK(0.25.0)、Hangame Android SDK(1.4.0)

#### バグ修正
* ログアウトした後、他のユーザーIDでログインした時、Google Playストア決済が成功しても、失敗が返されるエラーを修正
* アプリパッケージ名に大文字が含まれている場合、Sign In with Appleログインが失敗するエラーを修正

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

#### バグ修正
* HangameログインをPAYCOで進行中にキャンセルするとクラッシュが発生する問題を修正

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

#### 機能追加
* Hangame日本認証を追加

#### 機能改善/変更
* 外部SDKアップデート：Facebook Android SDK(6.5.1)、Line Android SDK(5.4.0)
	
#### バグ修正 
* Proguardを適用したビルドで決済APIを呼び出すとクラッシュが発生するエラーを修正

### 2.20.2 (2021.03.30) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* Google PlayストアのAndroid 11端末での決済エラーが解決したBilling Client 3.0.3バージョンにアップデート

### 2.20.1 (2021.02.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-Android.zip)

#### バグ修正 
* push-fcmモジュール初期化中にクラッシュが発生する場合があるロジックを修正

### 2.20.0 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Android.zip)

#### 機能追加
* 共通約款機能追加
    * 約款WebViewを開くAPIを追加
    * 約款リストおよびユーザー別同意有無を照会するAPIを追加
    * 各ユーザーの約款同意有無をGamebaseサーバーに保存するAPIを追加

#### 機能改善/変更
* サポートタイプがTOAST組織商品(Online Contact)の場合、ログインしなくてもサポートが表示されるように変更

### 2.19.1 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 2.19.0
    * (共通) Weibo認証を追加
    * (Android) Sign In with Apple認証を追加
    
#### 機能改善/変更
* [SDK] 2.19.0
    * (共通)ローンチステータスコード追加：ベータサービス(205)

#### バグ修正 
* [SDK] 2.19.1
	* (Android) Weiboログイン試行後、他のIdPでログイン時、クラッシュが発生する問題を修正

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Android.zip)

#### 機能追加
* Gamebaseサポートページオープン時、ゲームで定義したextra data伝達：SDK 2.18.2
    * [Console]サポート > 顧客お問い合わせ：顧客お問い合わせ詳細照会画面において追加で登録したextra dataを確認可能
* [SDK] 2.18.2
    * (共通)開発会社が独自のサポートをオープンする時、additionalURLフィールドを追加
    * (共通)決済アイテム情報にローカライズされた商品情報を追加：localizedTitle, localizedDescription

#### 機能改善/変更
* [SDK] 2.18.2
    * (共通) TOAST SDKアップデート: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
    * (Android)暗号化ロジックセキュリティ警告を解決するための外部SDKアップデート：Payco Login SDK(1.5.3), Hangame ID SDK(1.3.2)
    * (Android) Tencent Pushモジュール削除
    * (Android) Gamebase Android SDK 2.6.0でdeprecatedされた関数を削除
        * GamebaseConfiguration.Builder.setFCMSenderId()
        * GamebaseConfiguration.Builder.setTencentAccessKey()
        * GamebaseConfiguration.Builder.setTencentAccessId()

#### バグ修正 
* [SDK] 2.18.2
    * (Android) 5.0～6.0 OS端末でWebビューカスタムスキームが動作しない問題を修正

### 2.18.1 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.1/GamebaseSDK-Android.zip)

#### 機能追加
* Galaxyストア追加：SDK 2.18.0

#### 機能改善/変更
* [SDK] 2.18.0
    * (Android) TOAST SDKアップデート：[Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027)-GooglePlay Billing Library v.3.0.1 適用
    * (Android) WebView SSLセキュリティ警告対応処理を追加

#### バグ修正 
* [SDK] 2.18.1
    * (Android) 2.18.0でGoogle決済後にクラッシュが発生するイシューを修正

### 2.17.1 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Android.zip)
```
ハンゲーム認証を使用したい場合はサポートへご連絡ください。
```

#### 機能追加
* Hangame IdP認証追加：SDK 2.17.0

#### 機能改善/変更
* [SDK] 2.17.0
	* (共通)サポート添付イメージクリック時、ダウンロードサポート
	* (共通) TOAST SDKアップデート：Android(0.23.2), Unity(0.21.2)

#### バグ修正
* [SDK] 2.17.1
	* (Android) 2.17.0でImageNotice APIを呼び出した時、kotlinx-coroutineモジュールでクラッシュが発生する問題を修正

### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Android.zip)

#### 機能追加
* サポート機能追加
	* [SDK] 2.16.0
        * (共通) API追加(Gamebase.Contact.requestContactURL)：サポートURLリターン
        * (共通)サポートAPIにuserNameを設定できるようにContactConfigurationパラメータを追加 

### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Android.zip)

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
    * (共通) TOAST SDKアップデート：Android(0.23.0)、iOS(0.26.0)、Unity(0.21.0)   

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.13.0
    * (Android)イメージ告知のポップアップイメージ比率計算ロジックを修正

#### バグ修正
* [SDK] 2.13.0
    * (Android) Webビュー終了時、終了コールバックからANDROID_ACTIVITY_DESTROYED(31)エラーが返る問題を修正
    * (Android)決済モジュールにProGuard宣言が抜けていた問題を修正
    
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Android.zip)

#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * [SDK] 2.12.0：イメージ告知表示APIを追加
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 2.11.0
    * 決済API追加：商品IDで決済リクエスト, 追加情報(UserPayload)を入力して決済完了時に確認できる

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 2.10.0
    * (共通)既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
        * ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

### 2.9.1 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 2.9.1
    * (Andｒoid)マッピング以降、指標レベルがnullになり決済指標に正常に反映されない問題を修正

### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Android.zip)

#### 機能追加
* 退会猶予機能
    * [SDK] 2.9.0
        *(共通)API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認

#### 機能改善/変更
* [SDK] 2.9.0
    * (共通) TOAST SDKアップデート： Android(v0.21.0)、iOS(v0.23.0)、Unity(0.20.1)
    * (共通) PAYCO Login SDKアップデート： Android(v1.5.0)、iOS(v1.4.0)
	
### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.8.1 
    * (共通) Analytics転送結果を確認するための内部指標を追加
    * (Android)プロセスの再起動後、クラッシュが発生する場合があるコードを修正

### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 2.8.0
    * (共通)決済および商品情報に商品タイプおよび地域価格などの情報を追加

#### 機能改善/変更
* [SDK] 2.8.0 
    * (共通)コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップが表示されるように改善
    * (Android)ログイン直後に決済関連APIを呼び出す時、初期化タイミングの問題で失敗する場合があるコードを修正

### 2.7.2 (2020.03.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.7.2 
      * Gamebase初期化中にToastLogger初期化部分でクラッシュが発生する場合があるコードを修正
      * サーバーバージョンをv1.2.1にアップデートしました。

### 2.7.1 (2020.02.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.7.1
    * (共通) GuestでLoginしてGetAuthProviderUserIDを呼び出した時、値を返すように修正

### 2.7.0 (2020.01.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 2.7.0
    * (Android)サーバーレスポンス(response)でtraceError必須パラメータがなくてもクラッシュが発生しないように修正
    * (Android) Firebaseの設定が行われていない時、例外が発生しないように修正

### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.6.2
    * (共通) TOAST SDKアップデート: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)

### 2.6.1 (2019.12.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 2.6.1
    * (Android)Gamebase.initialize()呼び出し前にGamebase.login()を呼び出すとクラッシュが発生する問題を修正
    * (Android)TOAST Analytics User Dataを誤ってjavaアドレス値で転送する問題を修正
    * (Android)IAPサービスを有効にしていない場合に発生するクラッシュを修正

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Android.zip)

```
Gamebase SDK 2.6.0未満バージョンから2.6.0にアップグレードする場合
必ずUpgrade Guide文書に記載された変更事項を反映してください。 
ガイドの場所：Game > Gamebase > Upgrade Guide
```

#### 機能追加
* [SDK] 2.6.0
    * (共通)データをLog&Crashに転送して、各種分析に利用できるようにTOAST Loggerを追加
    * (Android) Google定期購入決済機能を追加
    * (Android) Gamebase Android SDKがBintrayを通して配布されるため、gradle設定だけでGamebaseを使用可能

### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 2.5.0
    * Consoleで入力したCS URLをWebビューで開くAPIを提供

### 2.4.4 (2019.07.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.4.4
    * (共通)会員エラーコードフォーマットを変更

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.4.2
    * (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加

#### バグ修正
* [SDK] 2.4.2
    * (共通)Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正


### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.4.0
    * (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#game-user-data-settings)]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#level-up-trace)]
    * (Android)Naver SDKバージョンアップデート(v4.2.5)：Naver SDKのバグを修正(Naverログイン中にアプリアイコンからアプリを再起動した場合、Activityが強制終了する問題により、認証プロセスが中断される問題を解決)

### 2.3.1 (2019.05.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.1/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 2.3.1
	    * (Android) 2.3.0バージョンでTwitterログインできない問題を修正



### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Android.zip)

```
Gamebaseを使用すると、10数個の中国ストアと連携が可能です。
中国でのリリースに関心がある方は、サポートへご連絡ください。
```

#### 機能追加
* [SDK] 2.3.0
    * (Android/Unity)中国ストア認証/決済追加

#### 機能改善/変更
* [SDK] 2.3.0
    * (共通)Launching Status Code追加："審査中(204)"、"テスト中(203)"
    * (Android)最後にログインしたProviderでログインおよびWebソケットレスポンス失敗を受け取った場合(Timeout、network disableなど)、AuthTokenを削除処理しないように修正
    * (Android)IdPログイン時、AuthAdapter内部で発生するMemoryLeakを修正

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 2.2.2
    * (Android)Gamebase初期化前にTransferAccount APIを呼び出した時、コールバックが来ない問題を修正

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Android.zip)

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
    * (Android)IAP SDKバージョンを最新バージョンであるv1.5.3バージョンにアップデート

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 2.1.0
    * (共通)TransferKey API削除
        * issueTransferKey：TransferKey発行
        * requestTransfer：TransferKey検証
        
#### バグ修正
* [SDK] 2.1.0
    * (Android)Gamebaseの初期化前に、onActivityResult()が呼び出され、動作異常を起こす問題を修正

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Android.zip)

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
    * (Android)Push SDKアップデート(android:1.7.0)
    * (Android)Adapter API変更
        * Launching情報伝達
        * logout, withdraw APIにCallbackを追加

### 1.14.5 (2018.12.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.5/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.14.5
    * deprecatedになっていた次のAPIが削除されました。
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
    * 決済モジュール(gamebase-adapter-purchase-iap)を修正しました。
        * IAP SDKを1.5.2にアップデート
        * Clientでは使用しないIAP TEST Storeを削除
        * 決済再処理ロジック(requestRetryTransaction)でデータが不完全な時、呼び出しが失敗する問題を修正
        * クラッシュを防止するために、すべてのIAP SDKの呼び出し元に例外処理

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.14.2
    * (Android)メンテナンス時、データ構造でメンテナンス開始/終了時間を意味するepoch timeのタイプをStringからlongにタイプ変更：既存Gamebase Unityと連携後、メンテナンス呼び出し時にタイプ不一致でコールバックが来ない現象を修正

#### バグ修正
* [SDK] 1.14.2
    * (Android)エミュレータ環境でストアアプリ(PlayStore、OneStoreなど)がない状態で、"アプリインストール/アップデート"時にストア未チェックによるcrashする問題を修正

### 1.14.1 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.1/GamebaseSDK-Android.zip)
	
#### 機能追加
* [SDK] 1.14.0
    * (共通)Gamebase Webviewでファイル添付機能を追加：AndroidのAPI 19、Kitcatでは正常に動作しません。

#### 機能改善/変更
* [SDK] 1.14.0
    * (共通)利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして伝送し、クライアントでデコードして処理するように修正
    * Remove API：Webview、Network、Launching
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)        
    * Deprecated  API 
        * (void)Gamebase.WebView.showWebView(Activity, String)
        * (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)

#### バグ修正
* [SDK] 1.14.1
    * (Android)Auth APIを呼び出した後、コールバックで再度Auth APIを重複して呼び出した時、正常に呼び出されない問題を修正

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.13.0
    * (共通)IAP SDK最新バージョン適用(android:1.5.1、iOS:1.6.0)
    * (Android)Push APIを呼び出した時、Gamebase初期化/ログイン状態に応じて、呼び出し失敗に対するエラーメッセージをより明確に改善
        * 初期化前に呼び出し：NOT_INITIALIZED(1)
        * 初期化後に呼び出した時、Pushモジュールがない：NOT_SUPPORTED(10)
        * 初期化成功およびログイン前の呼び出し：NOT_LOGGED_IN(2)        
	
#### バグ修正
* [SDK] 1.13.0
	* (Android)NaverCafe SDKとの衝突で、Naverログイン時に発生するエラーを解決

### 1.12.2 (2018.08.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.12.2
    * (Android)WebSocketタイムアウト時(API呼び出し時間経過)、クラッシュが発生することがある問題について予防ロジック処理
	
#### バグ修正
* [SDK] 1.12.2
    * (Android)auth-twitter-adapterを含んだ状態でTargetSdk 28でビルド時、初期化エラーが発生する問題を修正

### 1.12.1 (2018.08.09)

#### 機能改善/変更
* [SDK] 1.12.1
    * (共通)IAP SDK最新バージョン適用(1.5.0)
    * (共通)Gamebaseメンテナンスページで、メンテナンス時間を端末設定国時間に合わせて表示するように改善
    * (共通)メンテナンスページを外部ページで使用する時、Consoleに入力したメンテナンス情報を使用できる機能を追加
    * (共通)IdPマッピングされたユーザーがゲストマッピング試行時、エラー発生(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (共通)認証APIを重複して呼び出した時、エラー発生(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (Android)TencentPush SDKアップデート(3.2.3)
    * (Android)Onestore v17(API v5)サポート：Gamebaseではv16(ストアコード=TS)は提供しません。

### 1.11.1 (2018.07.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 1.11.1
    * (共通)ゲストログイン後にAddMapping成功時、loginForLastLoggedInPrivderをすると、AddMapping成功したIdPアカウントを使用してログインするように変更
    
#### バグ修正
* [SDK] 1.11.1
    * (共通)メンテナンス解除後にAPI進行(login/push/purchaseなど)ができない問題を修正
    * (Android)Gamebase.addObserver()を通してObserverMessageを受信した場合、 ObserverMessage.data.codeのタイプがintではなくStringになっている問題を修正


### 1.11.0 (2018.06.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Android.zip)

#### 機能追加
* Twitter IdP追加：Android、iOS
* Line IdP追加：Androidのみ提供。iOSは2018年7月に提供予定です。

#### 機能改善/変更
* [SDK] 1.11.0
    * (共通)LocalizedString日本語翻訳を追加
    * (共通)認証APIを呼び出した時に初期化、ログインをしない場合は明確にエラーコードを区別するように内部ロジックを改善
    * (Android)'android.permission.READ_PHONE_STATE'権限を削除
    * (Android)GamebaseConfiguration.Builderの必須設定値であるsetAppId、setAppVersionをコンストラクタで入力できるように変更
    * (Android)GamebaseConfiguration.BuilderのsetServerApiVerseion APIを削除
    * (Android)getAuthBanInfo() API、class AuthBanInfo名を変更：getBanInfo()、class BanInfo

### 1.9.0 (2018.05.03)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Android.zip)

#### 機能追加
* Transfer機能追加
    * ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    * (SDK共通)追加されたAPI 
        * Transfer Key発行API (IssueTransferKey)
        * 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)

#### バグ修正
* [SDK] 1.9.0
    * (Android) Heartbeatで、無効なユーザーと判定される場合、利用停止ポップアップが表示されないように修正(iOSと同じロジックで修正)

### 1.8.1 (2018.04.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 1.8.1
    * (Android. iOS)registerPushを呼び出す時、displayLanguageCodeをnullで渡すと、registerPushが失敗する問題を修正

### 1.8.0 (2018.04.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Android.zip)

#### 機能追加
* Kick out機能追加
    * 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    * (SDK共通)kick outイベントを受け取れるAPIを追加
* Observer機能の開発およびAPI追加
    * (SDK共通)メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

#### 機能改善/変更
* [SDK] 1.8.0
	* (共通)Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)

### 1.7.0 (2018.02.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 1.7.0
    * Naver IdP認証追加
    * Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

### 1.5.0 (2017.12.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Android.zip)
#### 機能追加
* [SDK] 1.5.0
    * WebViewが閉じられる時に発生するClose Callbackを追加
    * WebViewで使用するCustom SchemeのEventを受け取れる機能を追加

### 1.4.0 (2017.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Android.zip)

#### バグ修正
* [SDK] 1.4.0アップデート
    * (Android)Gamebase提供ポップアップを使用しない場合、利用停止情報がnullで返されるエラーを修正

### 1.3.0 (2017.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Android.zip)

#### 機能追加
* [SDK] 1.3.0アップデート
    * Credentialを利用したAddMapping API追加

### 1.2.0 (2017.09.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Android.zip)

#### 機能追加
* 利用停止(ユーザー処罰)機能を追加
* [SDK] 1.2.0アップデート
    * 利用停止ユーザーポップアップ表示


### 1.1.5 (2017.07.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.1.5アップデート
    * システムポップアップAPIを追加(showAlertWithTitle)
    * 国コードを大文字で返すように変更(Android)
    * TCPush SDK 1.4.1にアップデート
    * IAP SDK 1.3.3.20170627にアップデート

### 1.1.4 (2017.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Android.zip)
#### 機能改善/変更
* [SDK] 1.1.4アップデート
    * ランタイムのうち、決済Storeを変更できるAPIを提供
    * (Android)TCPushSdk v1.4適用、Tencent Push機能を提供

### 1.1.3 (2017.04.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.3/GamebaseSDK-Android.zip)
#### 機能改善/変更
* [SDK] 1.1.3アップデート
    * (Android)ローンチ構造およびポップアップ/メンテナンスページ改善：カスタムメンテナンスページ設定機能を追加
    * (Android)認証構造の改善およびログ追加：認証AdapterおよびSDKバージョンログ出力

#### バグ修正
* [SDK] 1.1.3アップデート
    * (Android)Facebook SDK v4.19.0以上で初期化時にクラッシュするエラーを修正


### 1.1.2 (2017.04.04)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.1.2アップデート
    * ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善

### 1.1.0 (2017.03.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Android.zip)

#### 機能改善/変更
* [SDK] 1.1.0アップデート
    * 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加
    * [UI機能追加](./aos-ui)：Custom Webview、AlertDialog

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Android.zip)

#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
    * 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
    * ログアウトおよび会員退会機能を提供
    * 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
    * ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
    * リアルタイムに運営指標を確認できるWebコンソール画面を提供
    * TOAST Cloudサービスと連携：PUSH、IAP
