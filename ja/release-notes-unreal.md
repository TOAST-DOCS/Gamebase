## Game > Gamebase > リリースノート > Unreal

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* '결제 어뷰징 자동 해제' 기능이 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > GraceBan](./unreal-authentication/#graceban)
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 이용 정지 유예 상태 후 이용 정지가 되도록 합니다.
    * 이용 정지 유예 상태일 경우 설정한 기간 내에 해제 조건을 모두 만족하면 정상플레이가 가능해집니다.
    * 기간 내에 조건을 충족하지 못하면 이용 정지가 됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.member.graceBanInfo API 값을 확인하고, null이 아닌 유효한 GraceBanInfo 객체를 리턴한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리하셔야 합니다.
* 강제매핑 시 IdP 로그인을 한번 더 시도해야 하는 불편함을 개선한 새로운 강제매핑 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping() 호출 후 AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) 에러가 발생했을 때, 해당 계정으로 로그인을 할 수 있는 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::ServerPushAppKickOutMessageReceived** 타입이 추가되었습니다.
    * 이 이벤트의 활용 방법은 다음 문서를 참고하시기 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::LoggedOut** 타입이 추가되었습니다.
    * Gamebase Access Token이 만료되어 로그인이 필요할 때 동작합니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* 공통약관 창의 설정을 변경할 수 있는 신규 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

#### 기능 개선/변경
* 에러코드 추가 및 변경
    * GamebaseErrorCode::UNKNOWN_ERROR 에러에 매핑된 에러코드를 999에서 9999로 변경하였습니다.
    * 에러코드 999에 매핑 시킨 GamebaseErrorCode::SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.
    
#### 플랫폼별 변경 사항
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
	* [Android設定ツール](https://docs.toast.com/ja/Game/Gamebase/ja/unreal-started/#android-settings)提供：Gamebase_Android_UPL.xmlファイルを修正する代わりに設定ツールを使用してください。
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
    * (Unreal) PLCrashReporterイシューサポート: [ガイド](http://docs.toast.com/ja/Game/Gamebase/ja/unreal-started/#ios-settings)

#### 機能改善/変更
* [SDK] 2.9.1
    * (Unreal) iOS Plugin内部Gamebase SDK for iOSバージョンアップデート(2.9.1)
    * (Unreal) UObjectリファレンシング処理が抜けていた部分を修正

### 2020. 05. 12.
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### 機能追加
* [SDK] 2.9.0
	* (Unreal) SDK新規配布
