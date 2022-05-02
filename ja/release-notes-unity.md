## Game > Gamebase > リリースノート > Unity

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.3)

#### 기능 개선/변경
* 중국어 번체 문장 수정

#### 버그 수정
* (Android) API Level 24 미만에서 특정 API 호출 시 오류가 발생하지 않도록 수정되었습니다.
    * Gamebase.Purchase.RequestActivatedPurchases()
    * Gamebase.Purchase.RequestItemListOfNotConsumed()

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.38.0](./release-notes-android/#2380-20220503)
* [Gamebase iOS SDK 2.38.0](./release-notes-ios/#2380-20220503)

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 고객센터 URL 뒤에 파라미터를 추가할 수 있도록 다음 필드가 추가되었습니다.
    * GamebaseRequest.Contact.Configuration.additionalParameters

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.37.0](./release-notes-android/#2370-20220426)
* [Gamebase iOS SDK 2.37.0](./release-notes-ios/#2370-20220426)

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 外部SDKアップデート：TOAST Unity SDK(0.25.2)
* 決済時にプロモーションかどうかを知ることができるisPromotionフィールドが追加されました。
    * GamebaseResponse.Purchase.PurchasableReceipt.isPromotion
* 決済時、テスト決済かどうかを知ることができるisTestPurchaseフィールドが追加されました。
    * GamebaseResponse.Purchase.PurchasableReceipt.isTestPurchase

#### バグ修正
* デバイスが特定文化圏に設定されているとき、決済商品の価格情報が0と入力されるエラーが修正されました。
* (iOS) Push通知をクリックしたときにディープリンクが動作しないエラーが修正されました。
* (iOS)プロジェクトのorientationがAuto Rotationに設定されており、プロジェクトの最初のシーン(scene)に含まれているMonoBehaviourのAwakeでGamebase API呼び出し時にWebビューなどのUI出力が正常に行われないエラーが修正されました。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.36.0](./release-notes-android/#2360-20220412)
* [Gamebase iOS SDK 2.36.0](./release-notes-ios/#2360-20220412)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Unity.zip)

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

#### プラットフォーム別変更事項
* [Gamebase Android SDK 2.35.0](./release-notes-android/#2350-20220329)
* [Gamebase iOS SDK 2.35.0](./release-notes-ios/#2350-20220329)

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-Unity.zip)

#### 機能追加
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * Gamebase.Push.QueryNotificationAllowed

#### バグ修正
* iOSでGamebaseWebViewConfigurationのisBackButtonVisible設定が適用されないエラーが修正されました。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-20220222)
* [Gamebase iOS SDK 2.34.1](./release-notes-ios/#2341-20220315)

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 共通約款API呼び出し後、約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * **GamebaseResponse.Terms.ShowTermsViewResult**

#### 機能改善/変更
* キックアウトポップアップの表示有無はGamebaseコンソールでキックアウト登録時に設定することができるため、次のフィールドがdeprecatedになりました。
    * **GamebaseConfiguration.enableKickoutPopup**

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-20220222)
* [Gamebase iOS SDK 2.34.0](./release-notes-ios/#2340-20220222)

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 共通約款ウィンドウの設定を変更できる新規APIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > UI > Terms > showTermsView](./unity-ui/#showtermsview)

#### 機能改善/変更
* エラーコードの追加と変更
    * GamebaseErrorCode.UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode.SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。
    
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.32.0 (2021.12.28)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* GamebaseEventHandlerのGamebaseEventCategoryに**GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED**タイプが追加されました。
    * このイベントの活用方法は、次の文書を参照してください。
    * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Server Push](./unity-etc/#server-push)
* Gamebase Access Tokenの有効期限が切れてログインが必要なときに動作する**GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler categoryが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unity-etc/#logged-out)

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.32.0](./release-notes-android/#2320-20211228)
* [Gamebase iOS SDK 2.32.0](./release-notes-ios/#2320-20211228)

### 2.31.0 (2021.12.14)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 外部SDKアップデート：TOAST Unity SDK(0.25.0)
* Standaloneメンテナンスポップアップでメンテナンス時間を表示するかどうかを動的に設定できるように変更しました。
* Setting Tool
    * PAYCO IDPが追加されました。
    * 既存のSettingToolを完全に削除した後、再インストールする必要があります。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.31.0](./release-notes-android/#2310-20211214)
* [Gamebase iOS SDK 2.31.0](./release-notes-ios/#2310-20211214)

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 強制マッピングを行う時、IdPログインをもう一度試行しなければいけない煩わしさを改善した、新しい強制マッピングAPIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Mapping > Add Mapping Forcibly](./unity-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping()呼び出し後、AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)エラーが発生した時、該当アカウントでログインすることができるAPIが追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > Mapping > Change Login with ForcingMappingTicket](./unity-authentication/#change-login-with-forcingmappingticket)

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.30.0](./release-notes-android/#2300-20211123)
* [Gamebase iOS SDK 2.30.0](./release-notes-ios/#2300-20211123)

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 外部SDKアップデート：TOAST Unity SDK(0.23.5)
* Setting Tool
    * v2.0.0が新しく配布されました。
    * 既存SettingToolを完全に削除して、再インストールする必要があります。
    * 変更された内容および使用方法は、以下のガイドを確認してください。
        * [Game > Gamebase > Unity SDK使用ガイド > 始める > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

#### バグ修正
* GamebaseDisplayLanguageCodeフィンランド語の誤字を修正
    * Finish → Finnish

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.29.0](./release-notes-android/#2290-20211109)
* [Gamebase iOS SDK 2.29.0](./release-notes-ios/#2290-2021109)

### 2.28.1 (2021.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.1/GamebaseSDK-Unity.zip)

#### バグ修正
* (Android) DisplayLanguageを設定していない場合、誤った値に設定される問題が修正されました。
* (Standalone)前のフレームで時間がかかる場合に発生するTimeoutエラーが修正されました。

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Unity.zip)

#### 機能追加
* Kakaogame認証追加
* 「決済アビューズ自動解除」機能が追加されました。
    * [Game > Gamebase > Unity SDK使用ガイド > 認証 > GraceBan](./unity-authentication/#graceban)
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にAuthToken.member.graceBanInfo API値を確認し、nullではない有効なGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.28.0](./release-notes-android/#2280-20210928)
* [Gamebase iOS SDK 2.28.0](./release-notes-ios/#2280-20210928)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* Display Language機能が改善されました。
    * 基本言語コードが**en**でしたが、Gamebaseコンソールで設定した基本言語が反映されるように改善しました。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > App > 言語設定](https://docs.toast.com/en/Game/Gamebase/en/oper-app/#language-settings)

#### バグ修正
* 「登録されていないゲームバージョン」エラーポップアップが英語でのみ表示されるバグを修正しました。
* メンテナンスポップアップに中国語が表示されないバグを修正しました。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.27.1](./release-notes-android/#2271-20210914)
* [Gamebase iOS SDK 2.27.1](./release-notes-ios/#2271-20210914)

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 外部SDKアップデート：TOAST Android SDK(0.23.2)
* ONE Store V16ストア追加

#### バグ修正
* Unity SDK 2.25.0で誤って追加されたファイルを削除
    * パス：Assets/Gamebase/Toast/IAP/Plugins

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
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

#### バグ修正
* Push言語設定は特別な補助処理なしで端末の言語コードがそのまま適用され、Pushコンソールから送信したメッセージの言語コードが一致しない問題を修正しました。


### 2.25.0 (2021.07.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 月決済限度機能を追加
    * 月決済限度を超える場合、**PURCHASE_LIMIT_EXCEEDED(4007)**エラーが発生します。

#### 機能改善/変更
* Push項目が存在する約款でPushConfigurationオブジェクト保障
    * 約款UIでPush受信に同意しない場合、Gamebase.Terms.ShowTermsView API呼び出し結果として作成されるPushConfigurationがnullでしたが、約款にPush項目が存在すればPushConfigurationオブジェクトが常に返されるように変更しました。
    * Push受信を拒否した時、PushConfigurationオブジェクトは(プッシュ同意有無= false、広告性プッシュ同意有無= false、夜間広告性プッシュ同意有無= false)で作成されます。
    * 約款にPush項目が存在しない場合、PushConfigurationオブジェクトはnullです。
* Unity最小サポートバージョン変更：2018.4.0f1
* 外部SDKアップデート: TOAST Unity SDK(0.23.0)

### 2.24.0 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 内部ローンチURL変更

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 外部SDKアップデート: TOAST Unity SDK(0.22.1)
* Unity 2020.2以降のバージョンで発生するWarningを除去
* StandaloneとUnity Editorで初期化速度を改善

#### バグ修正
* 約款同意を行ってもShowTermsView API呼び出すとPushConfiguration結果がnullではない問題を修正

### 2.22.0 (2021.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* 外部SDKアップデート: TOAST Unity SDK(0.22.0)

### 2.21.0(2021.04.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Unity.zip)

#### 機能追加
* Hangame日本認証を追加

### 2.20.0(2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 共通約款機能追加
	* 約款Webビューを開くAPIを追加
	* 約款リストおよびユーザーごとに同意の有無を照会するAPIを追加
	* ユーザーごとに約款の同意有無をGamebaseサーバーに保存するAPIを追加

#### 機能改善/変更
* サポートタイプがTOAST組織商品(Online Contact)の場合、ログインしなくてもサポートが表示されるように変更
* Warningログ削除
* Standalone WebViewにCEF 2.1.2アップデート
	* URLの長さが2,048を超えるとクラッシュが発生するイシューを修正
	* Unity 2019でビルドした時、ライブラリパスが変更されてPostProcessBuild改善
	* stringの初期化を行っていないことでエラーが発生する問題を修正
	* Gamebase WebView使用中にシーン(scene)を移動した後、WebViewが開けなくなるバグを修正

### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.19.0
	* (共通) Weibo認証を追加
	
#### 機能改善/変更
* [SDK] 2.19.0
	* (共通)ローンチステータスコード追加：ベータサービス(205)

#### バグ修正 
* [SDK] 2.19.0
    * (Unity) WebSocketで再試行した時、 OutOfMemoryExceptionが発生する問題を修正

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.18.2
	* (共通)開発会社が独自のサポートをオープンする時、additionalURLフィールドを追加
	* (共通)決済アイテム情報にローカライズされた商品情報を追加：localizedTitle, localizedDescription

#### 機能改善/変更
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

#### バグ修正 
* [SDK] 2.18.2
    * (Android) 5.0～6.0 OS端末でWebビューカスタムスキームが動作しない問題を修正

### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-Unity.zip)

#### 機能追加
* Galaxyストア追加：SDK 2.18.0

#### 機能改善/変更
* [SDK] 2.18.0
    * (Android) TOAST SDKアップデート：[Android(0.24.1)](https://docs.toast.com/ja/TOAST/ja/toast-sdk/release-notes-android/#0240-20201027)-GooglePlay Billing Library v.3.0.1 適用
    * (Android) WebView SSLセキュリティ警告対応処理を追加
    * (iOS) iOS 13以上から提供されるSceneDelegate対応APIを追加

#### バグ修正 
* [SDK] 2.18.1
    * (Android) 2.18.0でGoogle決済後にクラッシュが発生するイシューを修正

### 2.17.1 (2020.10.27) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Unity.zip)

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

#### 機能改善/変更
* [SDK] 2.17.1
    * (Unity) Unity 2017.2.5サポート

#### バグ修正
* [SDK] 2.17.1
    * (Unity)イメージ告知とWebビューを順番に呼び出すと、後で呼び出したAPIが動作しないエラーを修正	
	
### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-Unity.zip)

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
* [SDK] 2.17.1
	* (Android) 2.17.0でImageNotice APIを呼び出した時、kotlinx-coroutineモジュールでクラッシュが発生する問題を修正

### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Unity.zip)

#### 機能追加
* サポート機能追加
	* [SDK] 2.16.0
		* (共通) API追加(Gamebase.Contact.requestContactURL)：サポートURLリターン
		* (共通)サポートAPIにuserNameを設定できるようにContactConfigurationパラメータを追加 
		
### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unity.zip)

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
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.14.0
    * (iOS) PAYCO IdPの定数値を削除：PAYCO文字列によるApple検収がリジェクトされる場合があり削除
    * (iOS、Unity) TCGBWebViewConfigurationにcontentMode設定を追加

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.13.0
    * (Unity) Standalone:イメージ告知表示APIを追加   

#### 機能改善/変更
* [SDK] 2.13.0
    * (Android)イメージ告知のポップアップイメージ比率計算ロジックを修正
    * (iOS) Sign In With Apple認証：iOS 12以下をサポート
  
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Unity.zip)

#### 機能追加
* イメージ告知：表示期間と優先順位に応じてゲーム内でイメージをポップアップ表示
    * [SDK] 2.12.0：イメージ告知表示APIを追加

#### 機能改善/変更
* [SDK] 2.12.0
    * (iOS)Facebook SDKアップデート(7.1.1)
    * (iOS)configuartionに設定されたstoreCode(default=AS)でGamebaseの初期化を試行
    * (iOS)コンテンツをローディングできないWebビューを出力時、閉じるボタンがなくて閉じられない問題を修正
    * (Unity)TOAST Unity SDKアップデート(0.20.1.1)
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.11.0
	* 決済API追加：商品IDで決済リクエスト, 追加情報(UserPayload)を入力して決済完了時に確認できる

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.10.1
	* (iOS)ユーザープッシュ設定の初期化時、言語コードが設定されていない場合、デバイス言語で設定されるように変更

#### バグ修正
* [SDK] 2.10.1
	* (Unity) iOS Pluginで、ViewControllerが設定されておらず、ログイン呼び出し時に失敗する問題を修正

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.10.0
	* (共通)既存のすべてのイベントシステムを統合するGamebaseEventHandlerを追加
		* ServerPush、Observer機能が含まれていて、プロモーション決済イベントおよびプッシュイベントも確認可能

#### 機能改善/変更
* [SDK] 2.10.0 
	* (Unity) StandaloneWebviewAdapter内部のCefWebviewバージョンアップデート：v2.0.4
		* WebviewIndex検証ロジックを改善
		* Webview作成時、断続的にNullReferenceExceptionが発生するエラーを修正

### 2.9.1 (2020.04.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unity.zip)

#### バグ修正
* [SDK] 2.9.1 
	* (Unity) Initialize後、コンソールでクライアントのサービスの状態を変更するとエラーが発生する問題を修正

### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unity.zip)

#### 機能追加
* 退会猶予機能
	* [SDK] 2.9.0
		*(共通)API追加：退会猶予申請、退会猶予申請キャンセル、退会猶予状態から即時退会、ユーザーの退会猶予状態を確認
	
#### 機能改善/変更
* [SDK] 2.9.0
	* (共通) TOAST SDKアップデート： Android(v0.21.0)、iOS(v0.23.0)、Unity(0.20.1)
	* (共通) PAYCO Login SDKアップデート： Android(v1.5.0)、iOS(v1.4.0)
	
### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.8.1 
	* (共通) Analytics転送結果を確認するための内部指標を追加
	
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.8.0
	* (共通)決済および商品情報に商品タイプおよび地域価格などの情報を追加
	* (Unity) StandaloneWebviewAdapter内部のCefWebviewがv2.0.1バージョンにアップデート
		* PopupTypeがPASS_INFOの場合、ポップアップを表示させずにポップアップ情報を伝達する機能を追加

#### 機能改善/変更
* [SDK] 2.8.0 
	* (共通)コンソールに登録されていないアプリバージョンで初期化に失敗した時、ストアに移動できるポップアップが表示されるように改善
	* (Android)ログイン直後に決済関連APIを呼び出す時、初期化タイミングの問題で失敗する場合があるコードを修正

		
### 2.7.2 (2020.03.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Unity.zip)

#### 機能改善/変更
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

### 2.7.0 (2020.01.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.7.0
	* (Unity) NaverCafePLUGサポート

#### バグ修正
* [SDK] 2.7.0
	* (Android)サーバーレスポンス(response)でtraceError必須パラメータがなくてもクラッシュが発生しないように修正
	* (Android) Firebaseの設定が行われていない時、例外が発生しないように修正
	* (Unity) Web Login時、 gamebase://dismissスキーム処理を追加
	* (Unity)リリースビルド時、Webviewが表示されない問題を修正	

### 2.6.3 (2020.01.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.3/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.6.3
	* (Unity) Standalone Webview改善：CefWebviewアップデート	
	* (Unity)ログイン後、エラーが発生していたため、抜けていた.dllファイルを追加
		* ToastCommon.dll、vcruntime140.dll

#### バグ修正
* [SDK] 2.6.3
	* (Unity) Login(CredentialInfo) API呼び出し時にエラーが発生していた問題を修正
	
### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Unity.zip)

#### 機能追加
* クーポン > クーポン発行：キーワードクーポン機能を追加

#### 機能改善/変更
* [SDK] 2.6.2
	* (共通) TOAST SDKアップデート: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) NAVER SDKバージョンをアップデート(4.1.0)

### 2.6.1 (2019.11.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Unity.zip)

#### バグ修正
* [SDK] 2.6.1
	* (Unity)iOS PluginファイルがPackageに入っておらず、iOSビルド時にエラーが発生するため、該当ファイルを追加：'toast_sdk_wrap.m'
	* (Unity)UnityEditorでStandalone以外のプラットフォームで実行時、Store CodeがEmptyになり初期化に失敗するエラーを修正
	* (Unity)Initialize API内部zone type処理部分でのエラーで、NullReferenceExceptionが発生するエラーを修正

### 2019. 11. 13.

#### バグ修正
* GamebaseSettingTool
	* Gamebase v2.6.0アップデートの際、ファイルが正常に変更されないエラーを修正

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Unity.zip)

```
Gamebase SDK 2.6.0未満バージョンから2.6.0にアップグレードする場合
必ずUpgrade Guide文書に記載された変更事項を反映してください。 
ガイドの場所：Game > Gamebase > Upgrade Guide
```

#### 機能追加
* Google定期購入決済機能を追加
	* [SDK] 2.6.0 Android
* [SDK] 2.6.0
	* (共通)データをLog&Crashに転送して、各種分析に利用できるようにTOAST Loggerを追加
	* (iOS) Sign In with Apple認証を追加
	* (Android) Gamebase Android SDKがBintrayを通して配布されるため、gradle設定だけでGamebaseを使用可能

#### 機能改善/変更
* [SDK] 2.6.0
	* (Unity)ログイン時にLaunchingStatusを更新するロジックにエラーがあったため修正
	* (Unity) Debug Logを転送する機能をGamebaseコンソールで設定する場合、Clientからログ転送を無限に繰り返すエラーを修正

### 2019. 10. 15.
#### 機能改善/変更
* Sample App
	* Gamebase SDKアップデート(v2.4.0)
	* Smart Downloader適用(v1.5.8)、Leaderboard適用
	* 機能追加：ゲームリソースのダウンロード、 Leaderboard、TAA指標連携、端末移行機能、強制マッピング機能
	* 改善/変更：ServerPushリスナーを追加、 Observerメンテナンス中かどうかの検知を追加
	* ゲームリニューアル

### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 2.5.0
	* Consoleで入力したCS URLをWebビューで開くAPIを提供

### 2019.08.02.
#### バグ修正
* [SDK] Setting Tool 1.4.3
	* Scriptファイルの位置をEditorフォルダの下に移動してビルドエラーを解決
	* Mac OSでMultilanguageにLanguageファイルの全体パスを与えると動作しない問題を修正

### 2.4.4 (2019.07.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.4.4
	* (共通)会員エラーコードフォーマットを変更
	* (Unity)GamebaseServerPushTypeにKeyを追加(TRANSFER_KICKOUT)
* Setting Tool
	* フォルダ構造変更：`既存SettingToolを完全に削除した後、再インストールする必要があります。`
	* 多言語サポートを追加

### 2.4.3 (2019.07.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-Unity.zip)

#### バグ修正
* [SDK] 2.4.3
	* (Unity)iOSとAndroidでビルド時、AddMappingForcibly APIが動作しないエラーを修正
	* (Unity)RequestRetryTransaction APIの呼び出し時、iOSPluginでJSON解析エラーを修正
	
### 2019.06.27

#### バグ修正
* [SDK] Setting Tool 1.4.1
	* GamebaseSettingTool実行時、既存設定情報を取得できないエラーを修正

### 2019.06.25 [SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.4.2
	* (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加

#### バグ修正
* [SDK] 2.4.2
	* (共通)Analyticsのバグを修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正

### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Unity.zip)

#### 機能追加
* HANGAME mix日本決済追加
    * [SDK] 2.4.0
    	* (Unity)Standalone日本外部決済追加
    	* (Unity)Standalone日本HANGAME認証追加

#### 機能改善/変更
* [SDK] 2.4.0
	* (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#game-user-data-settings)]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](http://docs.toast.com/ja/Game/Gamebase/ja/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ja/Game/Gamebase/ja/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ja/Game/Gamebase/ja/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ja/Game/Gamebase/ja/js-etc/#level-up-trace)]
    * (Android)NAVER SDKバージョンアップデート(v4.2.5)：NAVER SDKのバグを修正(NAVERログイン中にアプリアイコンからアプリを再起動した場合、Activityが強制終了する問題により、認証プロセスが中断される問題を解決)
    * (Unity)StandaloneWebviewが32bit Buildをサポート(SDK容量53.6MBから99.2MBに増加)

### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Unity.zip)

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

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 2.2.2
	* (Unity)SDKログ改善

#### バグ修正
* [SDK] 2.2.2
	* (Unity)AddMappingForcibly APIを呼び出すとクラッシュする問題を修正

### 2.2.1 (2019.04.02)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.1/GamebaseSDK-Unity.zip)
#### バグ修正
* [SDK] 2.2.1
	* (Unity) Unity EditorでAndroidプラットフォームを選択してプレイすると、initializeの時にサーバーでエラーが発生する問題を修正

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Unity.zip)
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

#### 機能改善/変更
* [SDK] 2.2.0
	* (Android)IAP SDKバージョンを最新バージョンであるv1.5.3バージョンにアップデート
	* (iOS)LINE SDKのAppログイン機能が無効化
		* LINE SDK v4の問題により、iOS 12でアプリログインが失敗する問題があり、Gamebase Line AdatperでWebログインのみサポートするように変更
	* (Unity)GamebaseMainActivityのPackage Nameが変更
		* com.toast.gamebase.activity.GamebaseMainActivity → com.toast.android.gamebase.activity.GamebaseMainActivity

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Unity.zip)
#### 機能改善/変更
* [SDK] 2.1.0
	* (共通)TransferKey API削除
		* issueTransferKey：TransferKey発行
		* requestTransfer：TransferKey検証

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Unity.zip)
```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

#### 機能追加
* [SDK] 2.0.0
	* (共通)Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
		* setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
		* traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Unity.zip)
#### 機能改善/変更
* [SDK] 1.14.2
	* (Android)メンテナンス時、データ構造でメンテナンス開始/終了時間を意味するepoch timeのタイプをStringからlongにタイプ変更：既存Gamebase Unityと連携後、メンテナンス呼び出し時にタイプ不一致でコールバックが来ない現象を修正
	* (iOS)Provider Profile取得メソッドを呼び出した時に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にcrashが発生することがある構造を修正

#### バグ修正
* [SDK] 1.14.2
	* (Android)エミュレータ環境でストアアプリ(PlayStore、OneStoreなど)がない状態で、"アプリインストール/アップデート"時にストア未チェックによるcrashする問題を修正
	* (Unity)ShowWebView APIを呼び出した時、パラメータにCallbackを入れない場合、crashが発生する部分を修正
	* (Unity)iOS SDKのDeleted APIを呼び出すコードがあり、コンパイル時にエラーが発生する問題を修正
	
### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.14.0
	* (共通)Gamebase Webviewでファイル添付機能を追加：AndroidのAPI 19、Kitcatでは正常に動作しません。

#### 機能改善/変更
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
	

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 1.13.0
	* (共通)IAP SDK最新バージョン適用(android:1.5.1、iOS:1.6.0)
	* (Unity)ログで表示するjsonデータの出力フォーマットを改善
	
#### バグ修正
* [SDK] 1.13.0
	* (Android)NaverCafe SDKとの衝突で、NAVERログイン時に発生するエラーを解決
	* (Unity)Unity 2017.2以上のバージョンでEditor Play Mode終了時、websocke close処理で発生するエラーを修正

### 1.12.1 (2018.08.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-Unity.zip)

#### 機能改善/変更
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
	
	
### 1.12.0 (2018.07.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] Setting Tool
	* Setting Toolの最新バージョンがある場合、アップデート通知機能を追加
	* 内部null Exception修正
	
#### バグ修正
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

### 1.11.0 (2018.06.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Unity.zip)

#### 機能追加
* iOS Google IdP追加：iOS
* Twitter IdP追加：Android、iOS
* LINE IdP追加：Androidのみ提供。iOSは2018年7月に提供予定です。
	
#### 機能改善/変更
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
	
### 1.10.1 (2018.06.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.1/GamebaseSDK-Unity.zip)

#### バグ修正
* [SDK] 1.10.1
	* (Unity)Unity Adapterがない場合、AddMapping APIを呼び出した時、内部的にログインで処理していた問題を修正

### 1.10.0 (2018.06.07)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.10.0
	* (Unity)StandaloneWebviewAdapter: html source renderingサポート	

#### 機能改善/変更
* [SDK] 1.10.0
	* (Unity)Unity Adapterのinterfaceを修正
		* v1.10.0以上を使用時はUnityAdapterバージョンのアップグレードが必要(GamebaseUnitySDK_FacebookAdapter_v1.5.0、GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
	* (Unity)Login APIを呼び出した時、Unity Adapterがない場合、ネイティブ(Android/iOS)のログインAPIを呼び出すようにロジックを変更：facebook、Google
	* (Unity)がAdapterフォルダ構造および名前の誤字を修正
		* パス：Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
		* 誤字：Adapater → Adapter	

### 1.9.0 (2018.05.18)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 1.9.0
	* Unity SDK(1.9.0) Google Adapterを新規バージョン(1.6.2)に変更して再配布
    	* 5/3配布されたUnity SDK(1.9.0)に適用されたGoogle Adapterを最新バージョンに変更(1.6.1→1.6.2)
  
### 1.9.0 (2018.05.03)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

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
    * (Android) Heartbeatで、無効なユーザーと判定される場合、利用停止ポップアップが表示されないように修正(iOSと同じロジックで修正)

### 1.8.1 (2018.04.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Unity.zip)
#### バグ修正
* [SDK] 1.8.1
	* (Unity)UnityAndroidプラットフォームで下記の機能を使用時、モジュールが初期化されずにNullReferenceExceptionが発生する問題を修正
		* Launching, Purchase, Push, Util, Webview

### 1.8.0 (2018.04.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Unity.zip)

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
* [SDK] 1.8.0
	* (Setting Tool)Unity Facebook Adapterをチェックすると、エラーが発生する問題を修正

### 1.7.1 (2018.03.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.1/GamebaseSDK-Unity.zip)

#### バグ修正
* [SDK] 1.7.1
	* (Unity)Inspectorで設定されたSetDebugModeの値が反映されない問題を修正
	* (Unity)Standalone、WebGL：Display Languageで使用されるリソースファイルの欠損部分を修正
	* (Unity)Google Adapter 1.6.2配布：Google Adapter 1.6.1でAuthCodeがEmptyで返され、認証が失敗する問題を修正

### 1.7.0 (2018.02.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Unity.zip)
#### 機能追加
* [SDK] 1.7.0
	* NAVER IdP認証追加
	* Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

### 1.6.0 (2018.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.6.0
	* (Unity)Standalone WinSDK追加
		* 64bitサポート
		* 認証サポート：facebook、google、payco

### 1.5.0 (2017.12.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.5.0
	* WebViewが閉じられる時に発生するClose Callbackを追加
	* WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
	* Unity Setting Tool新規配布

#### バグ修正
* [SDK] 1.5.0
	* (Unity)UnityEditorでゲストログインされない現象を修正
	* (Unity)TOAST ConsoleにFacebook認証情報を登録しないでGamebase.Login("facebook") APIを呼び出した場合、KeyNotFoundExceptionが発生したため、防御コードを追加

### 1.4.0 (2017.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.4.0アップデート
	* (Unity)Gamebase Facebook Adapterを追加：Android、iOS、WebGL、Standalone PlatformおよびUnityEditorをサポート

### 1.3.0 (2017.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Unity.zip)

#### 機能追加
* [SDK] 1.3.0アップデート
	* Credentialを利用したAddMapping API追加

#### 機能改善/変更
* [SDK] 1.3.0アップデート	
	* (Unity)CredentialInfoを使用するLogin APIを呼び出した時、iOSPluginでJson解析がされない問題を修正
	
### 1.2.0 (2017.09.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Unity.zip)

#### 機能追加

* 利用停止(ユーザー処罰)機能を追加
* [SDK] 1.2.0アップデート
	* 利用停止ユーザーポップアップ表示

### 1.1.5 (2017.07.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Unity.zip)

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.5アップデート
	* システムポップアップAPIを追加(showAlertWithTitle)
	* 国コードを大文字で返すように変更(Android)
	* TCPush SDK 1.4.1にアップデート
	* IAP SDK 1.3.3.20170627にアップデート

### 1.1.4 (2017.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Unity.zip)

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.4アップデート
	* ランタイムのうち、決済Storeを変更できるAPIを提供
	* (Android)TCPushSdk v1.4適用、Tencent Push機能を提供

### 1.1.2 (2017.04.04)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 1.1.2アップデート
    * ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善
    * Unity Pluginデバッグログ追加および例外詳細処理

### 1.1.0 (2017.03.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Unity.zip)

#### 機能改善/変更
* [SDK] 1.1.0アップデート
    * 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加
    * [UI機能追加](./aos-ui)：Custom Webview、AlertDialog

### 1.0.0 (2017.03.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Unity.zip)

#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
	* 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
	* ログアウトおよび会員退会機能を提供
	* 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
	* ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
	* リアルタイムに運営指標を確認できるWebコンソール画面を提供
	* TOAST Cloudサービスと連携：PUSH、IAP
