## Game > Gamebase > リリースノート > Unreal

### 2.56.0 (2023. 10. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [Android 설정 툴](./unreal-started/#android-settings)에서 스토어 설정이 추가되었습니다.
    * Amazone Appstore, Huawei AppGallery, MyCard 선택이 추가되었습니다.
    * ONE Store를 선택 시 스토어의 버전 선택 옵션이 추가되었습니다.
* [iOS 설정 툴](./unreal-started/#ios-settings)에서 Naver IdP 설정이 추가되었습니다.
* (Android) LoginForLastLoggedInProvider 호출 중에 로딩 애니메이션을 숨기는 옵션을 지정할 수 있는 신규 API가 추가되었습니다.
    * LoginForLastLoggedInProvider(const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback)
    * API 호출 방법은 다음 가이드 문서를 참고하시기 바랍니다.
        * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Login > Login Flow > Login as the Latest Login IdP](./unreal-authentication/#login-as-the-latest-login-idp)
* (Android) Android 13 이상의 OS에서 RegisterPush API를 호출했을 때 Push 권한 요청 팝업이 자동으로 뜨지 않도록 할 수 있는 FGamebasePushConfiguration.requestNotificationPermission 필드가 추가되었습니다.
* (iOS) 사용자가 푸시 권한을 거부해도 토큰을 등록할 수 있도록 FGamebasePushConfiguration.alwaysAllowTokenRegistration 필드가 추가되었습니다.

#### 기능 개선/변경
* 제공되는 타입이 USTRUCT에서 일반 구조체로 변경되었습니다.
    * 결과로 받는 타입의 경우 기본적으로 제공되지 않는 값인 경우 TOptional 형태로 제공됩니다.

### 버그 수정
* 로그인 후 탈퇴 유예 정보 및 결제 어뷰징 자동 해제 정보가 정상으로 전달되도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.56.0](./release-notes-android/#2560-2023-09-26)
* [Gamebase iOS SDK 2.55.2](./release-notes-ios/#2552-2023-09-26)

### 2.49.1 (2023. 04. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-Unreal.zip)

#### バグ修正
* (iOS)決済商品照会APIを呼び出す際、クラッシュが発生しないように修正しました。

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.49.0 (2023. 04. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unreal.zip)

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

#### 機能改善/変更
* Unrealの最小サポートバージョンが4.26に変更されました。
* (iOS) Xcode 14.1でビルド時にエラーが発生する問題を修正しました。
    
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.43.3 (2022. 10. 04.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-Unreal.zip)

#### 機能改善/変更
* LINEログインを行う時、サービスを提供するRegionを入力するように変更されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > 認証 > Login with IdP](./unreal-authentication/#login-with-idp)
    
#### プラットフォーム別変更事項
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unreal.zip)

#### 機能追加
* FGamebaseForcingMappingTicketクラスにマッピングユーザー状態を表すmappedUserValidフィールドが追加されました。
*  [iOS設定ツール](./unreal-started/#ios-settings)でXcodeのパスを指定できるように**Xcode Path**設定が追加されました。

#### 機能改善/変更
* キックアウトポップアップウィンドウを表示するかどうかはGamebaseコンソールでキックアウト登録時に設定できるため、次のフィールドは今後は使用しません。
    * **FGamebaseConfiguration.enableKickoutPopup**
* FGamebaseConfiguration内の一部フィールドにデフォルト値が追加されました。
    * enableLaunchingStatusPopupのデフォルト値がtrueに設定されました。
    * enableBanPopupのデフォルト値がtrueに設定されました。
* FWebViewで固定フォントサイズを使用するかどうかを設定するフィールドは、今後は使用しません。
    * **FGamebaseWebViewConfiguration.enableFixedFontSize**
* FGamebaseWebViewConfiguratio内の一部フィールドに基本値が追加されました。
    * ナビゲーションバーの色相フィールドであるcolorR、colorG、colorB、colorAのデフォルト値が18、93、230、255に設定されました。
    * ナビゲーションバーの有効/無効を指定するフィールドであるisNavigationBarVisibleのデフォルト値がtrueに設定されました。
    * Webビュー内の戻るボタンの有効/無効を指定するフィールドであるisBackButtonVisibleのデフォルト値がtrueに設定されました。
    
#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unreal.zip)

#### 機能追加
* GamebaseEventHandlerのGamebaseEventCategoryに**IdPRevoked**タイプが追加されました。
    * [Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unreal-etc/#idp-revoked)

#### プラットフォーム別の変更事項
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.1 (2022. 06. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

#### バグ修正
* クラッシュが発生することがあるロジックが修正されました。
* (iOS)同じAPIを連続して呼び出すとき、コールバックが正常に伝達されない問題が修正されました。

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

#### 機能追加
*  [iOS設定ツール](./unreal-started/#ios-settings)を提供します。
    * 既存プロジェクト設定で**Gamebase**と表示されましたがアップデート後は**Gamebase - Android**、**Gamebase - iOS**と表示されます。
    * iOS設定ツールを提供し、ビルド時に必要なフレームワークのみ含まれるように修正しました。
* 共通約款API呼び出し後に約款UIが表示されたかどうかを知ることができるVOクラスが追加されました。
    * FGamebaseShowTermsViewResult
* 端末で通知を許可したかどうかを知ることができるAPIが追加されました。
    * IGamebase::Get().GetPush().QueryNotificationAllowed()
* 約款が表示されたかどうかを知ることができるAPIが追加されました。
    * IGamebase::Get().GetTerms().IsShowingTermsView()
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

#### 機能改善/変更
* API結果コールバック呼び出し時にGameThreadに切り替えて呼び出すように修正しました。
* RequestActivatedPurchases API呼び出し時に内部で2回呼び出される問題が修正されました。
* 一部APIの名前が変更されました。
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* キックアウトポップアップウィンドウを表示するかどうかは、Gamebaseコンソールでキックアウト登録時に設定できるため、次のフィールドは使用しなくなりました。
    * FGamebaseConfiguration.enableKickoutPopup
    
#### 各プラットフォームの変更事項
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

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
