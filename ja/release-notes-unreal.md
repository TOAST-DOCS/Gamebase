## Game > Gamebase > リリースノート > Unreal

### 2.40.1 (2022. 06. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* 크래시가 발생할 수 있는 로직이 수정되었습니다.
* (iOS) 동일한 API를 연속해서 호출 시 콜백이 정상적으로 전달되지 않는 문제가 수정되었습니다.

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
*  [iOS 설정 툴](./unreal-started/#ios-settings)을 제공합니다.
    * 기존 프로젝트 설정에서 **Gamebase**으로 표시 되었지만 업데이트 이후 **Gamebase - Android**, **Gamebase - iOS**로 표시됩니다.
    * iOS 설정 툴을 제공하면서 빌드 시 필요한 프레임워크만 포함되도록 수정되었습니다.
* 공통 약관 API 호출 후 약관 UI가 표시되었는지 여부를 알 수 있는 VO 클래스가 추가되었습니다.
    * FGamebaseShowTermsViewResult
* 단말기에서 알림을 허용했는지 여부를 알 수 있는 API가 추가되었습니다.
    * IGamebase::Get().GetPush().QueryNotificationAllowed()
* 약관이 표시되었는지를 알 수 있는 API가 추가되었습니다.
    * IGamebase::Get().GetTerms().IsShowingTermsView()
* 웹뷰에서 내비게이션 바를 숨길 수 있는 옵션이 추가되었습니다.
    * FGamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) 웹뷰에서 폰트 사이즈를 고정할 수 있는 옵션이 추가되었습니다
    * FGamebaseTermsConfiguration.enableFixedFontSize
* (Android) 약관 창에서 글자 크기를 고정할 수 있는 옵션이 추가되었습니다.
    * FGamebaseTermsConfiguration.enableFixedFontSize
* 결제 시 프로모션 여부를 알 수 있는 isPromotion 필드가 추가되었습니다.
    * FGamebasePurchasableReceipt.isPromotion
* 결제 시 테스트 결제 여부를 알 수 있는 isTestPurchase 필드가 추가되었습니다.
    * FGamebasePurchasableReceipt.isTestPurchase
* 고객 센터 URL 뒤에 파라미터를 추가할 수 있도록 다음 필드가 추가되었습니다.
    * FGamebaseContactConfiguration.additionalParameters

#### 기능 개선/변경
* API 결과 콜백 호출 시 GameThread로 전환하여 호출하도록 수정되었습니다.
* RequestActivatedPurchases API 호출 시 내부에서 2회 호출되는 문제가 수정되었습니다.
* 일부 API의 이름이 변경되었습니다.
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* 킥아웃 팝업 창 표시 여부는 Gamebase 콘솔에서 킥아웃 등록 시 설정할 수 있으므로 다음 필드가 deprecated되었습니다.
    * FGamebaseConfiguration.enableKickoutPopup
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-20220524)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-20220524)

### 2.33.1 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.1/GamebaseSDK-Unreal.zip)

#### バグ修正
* iOSビルド時に発生するエラーを修正しました。

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

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

#### 機能改善/変更
* エラーコードの追加と変更
    * GamebaseErrorCode::UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode::SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。
    
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### バグ修正
* GamebaseDisplayLanguageCodeフィンランド語の誤字を修正
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### 機能追加
* 共通約款機能の追加
    * 約款WebViewを開くAPIの追加
    * 約款リストおよびユーザー別同意有無を照会するAPIを追加
    * ユーザー別の約款同意有無をGamebaseサーバーに保存するAPIを追加

#### 機能改善/変更
* サポートのタイプがTOAST組織商品(Online Contact)の場合、ログインを行わなくてもサポートが表示されるように変更
* 内部ローンチURLの変更
* GamebaseからAndroid multidexの適用削除

### 2.19.2 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.2/GamebaseSDK-Unreal.zip)

#### バグ修正
* イメージ告知ShowImageNotices API呼び出し時にonEventCallbackを登録していない場合、閉じるボタンを押した時にクラッシュが発生する問題を修正
* Android設定ツール - Enable Hangame、Enable Weiboが正常に動作しない問題を修正

### 2.19.1(2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Unreal.zip)

#### バグ修正 
* Unityビルド中に除外されるファイルが生じた時に発生するコンパイルエラーを修正

### 2.19.0 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unreal.zip)

#### 機能追加
* SDK配布：2.16.0～2.19.0累積した内容を反映
	* [Android設定ツール](./unreal-started/#android-settings)提供：Gamebase_Android_UPL.xmlファイルを修正する代わりに設定ツールを使用してください。
	* サポート機能を追加	
	* 認証追加：Hangame、Weibo
	* Galaxyストアを追加
	* 決済アイテム情報にローカライズされた商品情報を追加：localizedTitle、localizedDescription
	* Android設定ツールを提供
	* Unreal 4.26サポート

### 2.15.0 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unreal.zip)

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
* [SDK] 2.15.0
    * (Unreal) TOAST SDKアップデート：Android(0.23.0), iOS(0.26.0), Unity(0.21.0)   

#### バグ修正
* [SDK] 2.15.0    
    * (Unreal)決済モジュールにProGuard宣言が抜けていたエラーを修正

### 2.9.1 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unreal.zip)

#### 機能追加
* [SDK] 2.9.1
    * (Unreal) Unreal 4.22 ～ 4.25サポート
    * (Unreal) PLCrashReporterイシューサポート: [ガイド](./unreal-started/#ios-settings)

#### 機能改善/変更
* [SDK] 2.9.1
    * (Unreal) iOS Plugin内部Gamebase SDK for iOSバージョンアップデート(2.9.1)
    * (Unreal) UObjectリファレンシング処理が抜けていた部分を修正

### 2020. 05. 12.
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### 機能追加
* [SDK] 2.9.0
	* (Unreal) SDK新規配布
