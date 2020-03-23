## Game > Gamebase > リリースノート

### 2020. 03. 24.

#### 기능 추가
* [Console] 
	* 신규 메뉴 오픈: Analytics > 이용자 지표 > Frequency 7
		* DAU의 일주일간 방문수와 비율 정보를 제공합니다. 게임몰입도, 충성도 등을 한 눈에 파악할 수 있습니다.
	* 쿠폰 > 쿠폰 발급: 쿠폰 문자 발송 기능 추가
* [SDK] 2.7.3
	* (공통) 결제 및 상품 정보에 상품 타입 및 지역 가격 등의 정보를 추가
	* (Unity) StandaloneWebviewAdapter 내부의 CefWebview가 v2.0.1 버전으로 업데이트
		* PopupType이 PASS_INFO일 경우, 팝업을 띄우지 않고 팝업 정보를 전달하는 기능을 추가
 	* (Javascript) 한게임 채널링 지원: 한게임 IdP 인증, 한코인 결제 추가


#### 기능 개선/변경
* [Console] 
	* 앱 > 전송 지표 세팅: 미리 등록한 메타 필터만 전송 지표에 사용할 수 있도록 제한
		* 메타필터 개수 제한하여 그 이상 전송하는 경우 지표가 노출되지 않으니 주의해 주세요.: 레벨(5,000개), 월드/서버/채널(100개), 직업/클래스(100개)
* [SDK] 2.7.3 
	* (공통) 콘솔에 등록되지 않은 앱 버전으로 초기화 실패할 때 스토어로 이동할 수 있는 팝업이 추가로 노출하도록 개선
	* (Android) 로그인 직후 결제 관련 API를 호출할 때 초기화 타이밍 문제로 실패가 발생할 수 있는 코드를 수정

#### 버그 수정
* [Console] 
	* 매출 지표 > 결제 금액
		* 차트 툴 팁에 통화가 원(KRW)으로 고정되어 노출되어 앱에서 설정한 통화로 보이도록 수정
		* 월별 조회시 2월 지표가 노출안되는 이슈 수정
		
### 2020. 03. 10.

#### 기능 추가

- [Console] 
	- 앱  >  앱: Analytics 매출 지표를 표시할 때 테스트 결제 포함 여부 설정  
    		- '테스트 결제 제외'로 설정하면 Analytics 매출 지표에서 테스트 결제는 모두 제외하고 보여줍니다. 
		- 구매(IAP): 구매(IAP) 메뉴 최초 접근 시 결제 지표 통화 코드 설정 
	- 최초 한 번만 설정 가능하며 Analytics 매출 지표에는 설정된 통화 코드로 지표가 표시됩니다.  
  	- 모바일 콘솔(TOAST 앱 포함)에 '데스크톱 보기' 기능 추가

#### 기능 개선/변경

- [Console] 
  	- 앱  >  설치 URL: URL 입력 가능한 스킴(scheme) 추가 적용 
    		- 기존: 공통('http://', 'https://'), Android('market://') 
    		- 추가: iOS('itms://', 'itmss://', 'itms-apps://'), Android('intent://')
- [SDK] 2.7.2 
  	- (Unity) FacebookAdapter 개선 
    		- v7.9.4~v7.18.1 버전까지 호환성 테스트
    		- Null 예외 처리 
  	- (Unity) StandaloneWebviewAdapter 개선 
    		- 웹 페이지를 텍스처(texture)로 내보내기 추가
    		- 멀티 웹뷰 지원 
    		- 쿠키 삭제 옵션 추가 
    		- 텍스처(texture) 크기 조절 지원 
		- 스크롤바 표시/숨기기 지원 
    		- 페이지 로드 완료 알림 
    		- 투명 배경 지원 
  	- (Unity) 에디터에서 Android/iOS 플랫폼을 선택하고 Initialize API를 호출하면 오류가 발생하는 문제 해결

#### 버그 수정

- [Console] 
  	- Analytics: 통화 코드가 코인성인 경우 매출 지표가 '0'으로 표시되는 문제 해결

### 2020. 02. 25.

#### 기능 추가
* [Console] 
	* 쿠폰 > 쿠폰 발급: 발급한 쿠폰을 설정한 스토어에서만 사용할 수 있도록 기능 추가
	
#### 기능 개선/변경
* [SDK] 2.7.1
	* (Common) Guest로 Login 후 GetAuthProviderUserID 호출하면 값을 반환하도록 수정
* [Console]
	* 앱 > 앱: 동일한 클라이언트 버전 삭제 이후 재등록 시 알림 로직 추가
	* 구매(IAP) > Item: 등록 시 구독 상품 등록을 위한 등록 필드값 추가(App Store - Shared secret,Google store - Domain authentication File Names)

#### 버그 수정
* [Console]
	* Analytics > 실시간 모니터링 > 실시간 지표: 간헐적으로 푸시 발송 후 ccu 항목에 빈 값 혹은 infinity로 나타나는 현상 수정
	* Analytics > 전송 지표
		* 그리드에 데이터가 있다가 없어지면 No Data로 갱신되지 않는 버그 수정
		* 필터 이름이 짧을 때 버튼 정렬이 세로로 나오는 현상 수정

### 2020. 02. 11.

#### 기능 추가
* [Console] 
	* Analytics > 이용자 지표 > Life Cycle 메뉴 신규 오픈 프로젝트 생성부터 이용자 지표의 흐름을 그래프로 한눈에 파악할 수 있도록 기능 제공
	* 관리 > 권한: 위클리 리포트 수신 권한 항목 추가
		* 실제 '위클리 리포트' 메일은 3월부터 전송될 예정입니다.

#### 기능 개선/변경
* [서버 API] 탈퇴 API 호출 시 regUser 길이에 대한 유효성 검사(validation) 추가
* [Console] 
	* Analytics: Grid, Chart에 일본어 폰트 적용
	* 구매: 오류 발생 시 나타나는 팝업 메시지를 사용자가 직관적으로 알 수 있게 개선

#### 버그 수정
* [Console]
	* Analytics: 일본어로 언어 변경 시 통화가 '엔(JPY)'으로 표시되던 것을 '원(KRW)'으로 표시되도록 수정

### 2020. 01. 21.

#### 기능 추가
* [SDK] 2.7.0
	* (Unity) NaverCafePLUG 지원

#### 버그 수정
* [SDK] 2.7.0
	* (Android) 서버 응답(response)에서 traceError 필수 파라미터가 없더라도 크래시가 발생하지 않도록 수정
	* (Android) Firebase 설정이 누락되어 있을 때 예외가 발생하지 않도록 수정
	* (Unity) Web Login 시, gamebase://dismiss 스킴 처리 추가
	* (Unity) 릴리스 빌드 시, 간헐적으로 Webview가 노출되지 않는 문제 수정	
* [Console]
	* Analytics: 유저 세션 만료 시 로그인 페이지로 리디렉트되지 않는 현상 수정

### 2020. 01. 14.

#### 기능 추가
* [서버 API] 사용자 탈퇴 API 추가

#### 기능 개선/변경
* [SDK] 2.6.3
	* (Unity) Standalone Webview 개선: CefWebview 업데이트	
	* (Unity) 로그인 이후 오류가 발생하여 누락된 .dll 파일 추가
		* ToastCommon.dll, vcruntime140.dll

#### 버그 수정
* [SDK] 2.6.3
	* (Unity) Login(CredentialInfo) API 호출 시 오류가 발생하여 수정
	
### 2019. 12. 24.

#### 기능 추가
* 쿠폰 > 쿠폰 발급: 키워드 쿠폰 기능 추가

#### 기능 개선/변경
* [Console]
	* 구매 > 결제 정보 조회: 추가 정보 칼럼 추가
* [SDK] 2.6.2
	* (공통) TOAST SDK 업데이트: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) Naver SDK 버전 업데이트(4.1.0)

### 2019. 12. 10.

#### 機能追加
* アプリ > アプリ：メンテナンス中にQAテスト端末を登録すると、IPでも登録できる機能を追加

#### バグ修正
* [Console]
	* 日本語文言を修正
* [SDK] 2.6.1
	* (Android)Gamebase.initialize()呼び出し前にGamebase.login()を呼び出すとクラッシュが発生する問題を修正
	* (Android)TOAST Analytics User Dataを誤ってjavaアドレス値で転送する問題を修正
	* (Android)IAPサービスを有効にしていない場合に発生するクラッシュを修正
	* (iOS)AddMapping(強制、Forcibly)使用時、マッピングされない問題を修正
	* (iOS)Unity PluginにPushConfigurationのdisplayLanguageCodeを設定していない場合、NSNullオブジェクトによりクラッシュが発生する問題を修正

### 2019. 11. 26.

#### バグ修正
* [Console]
	* クーポン > クーポン発行：セッション終了後にクーポンをダウンロードすると、異常なファイルをダウンロードする問題を修正
	* Analytics > リアルタイムモニタリング > ダッシュボード：昨日のデータが0と表示される現象を修正
	* TOASTサービス(IAP、Push、AppGuardなど)関連メニューにアクセスした時、サービス無効になっている場合に無効ページが正常に表示されない問題を修正

### 2019. 11. 20.

#### バグ修正
* [SDK] 2.6.1
	* (Unity)iOS PluginファイルがPackageに入っておらず、iOSビルド時にエラーが発生するため、該当ファイルを追加：'toast_sdk_wrap.m'
	* (Unity)UnityEditorでStandalone以外のプラットフォームで実行時、Store CodeがEmptyになり初期化に失敗するエラーを修正
	* (Unity)Initialize API内部zone type処理部分でのエラーで、NullReferenceExceptionが発生するエラーを修正

### 2019. 11. 13.

#### バグ修正
* GamebaseSettingTool
	* Gamebase v2.6.0アップデートの際、ファイルが正常に変更されないエラーを修正


### 2019. 11. 12.

```
Gamebase SDK 2.6.0未満バージョンから2.6.0にアップグレードする場合
必ずUpgrade Guide文書に記載された変更事項を反映してください。 
ガイドの場所：Game > Gamebase > Upgrade Guide
```

#### 機能追加
* クーポンサービス新規オープン：クーポンを大量に作成し、管理する機能
	* [Console]Couponメニュー新規オープン
	* [Server API]クーポン確認および消費APIを追加
* 自動決済アビューズ機能を追加
	* [Console]購入(IAP) > 決済アビューズモニタリングメニューを新規オープン
		* 決済アビューズ自動制裁設定機能
		* 決済アビューズ条件検索後、手動で利用停止が可能
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
* [Console]
	* アプリ > アプリ：サーバーアドレスをサービス状態別(テスト、審査中、サービス)に入力できるように変更
	* 購入(IAP) > 決済情報：検索条件を選択して検索できるようにUIを変更

### 2019. 10. 29.

#### 機能改善/変更
* [Console]
	* Analytics：円グラフのツールチップを変更
	* Analytics > リアルタイムモニタリング：Push送信対象追加作業

### 2019. 10. 15.

#### 機能改善/変更
* [SDK] 2.5.2
	* (iOS) UIWebViewをWKWebViewに変更
* Sample App
	* Gamebase SDKアップデート(v2.4.0)
	* Smart Downloader適用(v1.5.8)、Leaderboard適用
	* 機能追加：ゲームリソースのダウンロード、 Leaderboard、TAA指標連携、端末移行機能、強制マッピング機能
	* 改善/変更：ServerPushリスナーを追加、 Observerメンテナンス中かどうかの検知を追加
	* ゲームリニューアル
		

#### バグ修正
* [Console]	
	* 管理 > 権限：権限の修正が正常にできない問題を修正
	* モバイル
		* Datepicker選択時、キーボードが有効化される現象を修正
		* Analytics：ARPPU項目にNRU値が表示される現象を修正
	
### 2019. 09. 24.

#### 機能改善/変更
* [Console]
	* アプリ > クライアント：Webクライアント登録時にも、ストアを選択して登録できるようにUIを修正
		
#### バグ修正
* [Console]	
	* 管理 > 権限：権限の修正が正常にできない問題を修正
	* モバイル
		* Datepicker選択時、キーボードが有効化される現象を修正
		* Analytics：ARPPU項目にNRU値が表示される現象を修正


### 2019.09.10.

#### 機能追加
* [Console]
	* Analytics：チャンネル/ワールド/サーバー、職業/クラス転送指標にレベル指標を追加表示

#### 機能改善/変更
* [Console]
	* Analytics：Gridレンダリング性能を改善(tui-grid 4.4.x適用)
* [SDK] 2.5.1
	* (iOS) GamebasePushAdapterで使用中のTCPushSDKを1.7.0にアップデート
		* TCPushSDKが、Static LibraryからFrameworkファイルに変更されたため、プロジェクトにTCPushSDK.frameworkを追加する必要があります。

### 2019.08.27.

#### 機能追加
* [SDK] 2.5.0
	* Consoleで入力したCS URLをWebビューで開くAPIを提供

#### 機能改善/変更
* [Console]
	* Analytics：Chart性能を改善
	
### 2019.08.02.

#### バグ修正
* [SDK] Setting Tool 1.4.3
	* Scriptファイルの位置をEditorフォルダの下に移動してビルドエラーを解決
	* Mac OSでMultilanguageにLanguageファイルの全体パスを与えると動作しない問題を修正

### 2019.07.23

#### 機能追加
* [Console]
	* メンバー > ダウンロード新規メニューオープン：加入日、最終ログイン時間を基準にゲームユーザーリストを照会し、ファイルでダウンロードできる

#### 機能改善/変更
* [Console]モバイル
	* メンテナンス、プッシュ登録と修正可能
* [SDK] 2.4.4
	* (共通)会員エラーコードフォーマットを変更
	* (Unity)GamebaseServerPushTypeにKeyを追加(TRANSFER_KICKOUT)
* Setting Tool
	* フォルダ構造変更：`既存SettingToolを完全に削除した後、再インストールする必要があります。`
	* 多言語サポートを追加

#### バグ修正
* [Console]
	* Analytics > 利用者指標：チャートx軸の日付が重なるイシューを修正

### 2019.07.01

#### バグ修正

* [Console]
	* 管理>アラーム：Webhook設定後、修正が失敗する現象を修正
	
### 2019.06.27

#### バグ修正
* [Console]
	* 利用停止：利用停止大量登録を行うためのファイルアップロードが失敗する現象を修正
* [SDK] Setting Tool 1.4.1
	* GamebaseSettingTool実行時、既存設定情報を取得できないエラーを修正

### 2019.06.25

#### 機能追加
* 転送指標機能追加
    * [Console]Analytics>転送指標：Level、Channel、Class別指標確認メニューオープン
		* リアルタイム状況
		* レベル別状況
		* ワールド/サーバー/チャンネル別状況
		* クラス/職業別状況
		* レベルアップ
		* アイテム販売状況
		* アイテム販売TOP 50

#### 機能改善/変更
* [SDK] 2.4.2
	* (共通)LaunchingInfoにJSON string形式のTOAST Launching情報を追加
	* (iOS)LINE SDKアップデート(v5.0.1)
		* LINE Adpaterの対応OSバージョンがiOS 10以上に変更
		* LINEアプリによるログイン機能追加

#### バグ修正
* [SDK] 2.4.2
	* (共通)Analyticsバグ修正：ログアウト、退会、アカウント移行時に保存された指標データを初期化するように修正
	* (iOS)ネットワーク接続問題により、断続的にクラッシュが発生する現象を修正

### 2019.06.13

#### バグ修正
* [SDK] 2.4.1
	* (iOS)Analytics指標転送時、一部パラメータが抜けて指標が正常に出力されないバグを修正
	
### 2019.05.28

#### 機能追加
* HANGAME mix日本決済追加
    * [SDK] 2.4.0
    	* (Unity)Standalone日本外部決済追加
    	* (Unity)Standalone日本HANGAME認証追加
    * [Console] 
    	* 購入>ストア：'HANGAME mix(JAPAN)' Store追加
    	* アプリ>クライアント：Windowsクライアント登録時、ストア設定項目追加
    	* アプリ>インストールURL：WindowsインストールURL追加時、ストア設定項目追加

#### 機能改善/変更
* [SDK] 2.4.0
	* (共通)指標関連Class変更
        * LevelUpData Class：userLevel、levelUpTimeパラメータが必須に変更 / その他フィールド削除[詳細表示[Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#game-user-data-settings)]
        * GameUserData Class：classId(ゲームユーザーの職業)フィールド追加[詳細表示[Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#level-up-trace)]
    * (Android)Naver SDKバージョンアップデート(v4.2.5)：Naver SDKバグ修正(Naverログイン中にアプリアイコンからアプリを再起動した場合、Activityが強制終了するイシューにより、認証プロセスが中断されるイシューを解決)
    * (Unity)StandaloneWebviewが32bit Buildをサポート(SDK容量53.6MBから99.2MBに増加)
* [Server]
    * LTVクエリー修正およびfailoverロジック修正
* [Console]
    * LTV Grid ComplexColumnsサポートおよびExcelダウンロードサポート

### 2019.05.16

#### 機能追加
- [Console]
  - メニュー追加
    - アプリ > 端末移行(Transfer account)：端末移行機能を使用するための設定値を保存
    - 会員 > 端末移行：発行されたキーの状態および履歴照会

#### 機能改善/変更
- [Console]
  - アプリ：AppleGameCenter、China IdPに'トークン再検証'オプション削除
    - 該当IdPでは、実際の外部IdPを確認せず、内部トークンのみ確認するため、意味がないオプションを削除
  - 会員：マッピング追加可能な条件を変更
    - 既存：ゲストアカウント(Guest)
    - 変更：ゲストアカウント(Guest)、消滅アカウント(missing)

#### バグ修正
- [SDK] 2.3.1
  - (Android) 2.3.0バージョンでTwitterログインできない問題を修正
- [Console]
  - 会員：購入履歴で、領収書の検証ができない問題を修正
  - キックアウト(kickout)：照会リクエスト時、認証確認を追加し、動作異常イシューを修正

### 2019.04.23

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

### 2019.04.11

#### 機能改善/変更
* [SDK] 2.2.2
	* (Unity)SDKログ改善
* [Console]
	* Analyticsメニューの多言語適用
	* セキュリティ検収関連脆弱性パッチ

#### バグ修正
* [SDK] 2.2.2
	* (Android)Gamebase初期化前にTransferAccount APIを呼び出した時、コールバックが来ないイシューを修正
	* (iOS)showBlockingPopupをNOに設定した場合、Gamebase初期化コールバックが呼び出されないイシューを修正
	* (Unity)AddMappingForcibly APIを呼び出すとクラッシュする問題を修正

### 2019.04.02

#### バグ修正
* [SDK] 2.2.1
	* (Unity) Unity EditorでAndroidプラットフォームを選択してプレイすると、initializeの時にサーバーでエラーが発生するイシューを修正

### 2019.03.26

#### 機能追加
* TransferAccount機能追加：ゲストユーザーがマッピングを行わずに最大2個のキーを利用して新しい端末に移行できる機能
    - (SDK共通)追加されたAPI 
		* TransferAccountInfo発行API (issueTransferAccount)
		* 発行されたTransferAccountInfoを使用して、アカウント移行をリクエストするAPI (transferAccountWithIdPLogin)
		* 発行されたTransferAccountInfoを確認するAPI (queryTransferAccount)
		* すでに発行されたTransferAccountInfoを更新するAPI (renewTransferAccount)		
	- (Server API)
		* 発行されたTransferAccountのID/PWを検証するサーバーAPI (validateTransferAccount)
    - (console)会員メニューのマッピング履歴照会タブで、Transfer履歴の確認が可能
* 強制マッピング機能を追加：すでに他のアカウントに連携しているIdPアカウントをマッピングできる機能
	- (SDK共通)追加されたAPI 
		* 強制マッピングするAPI (addMappingForcibly)

#### 機能改善/変更
* [SDK] 2.2.0
	* (Android)IAP SDKバージョンを最新バージョンであるv1.5.3バージョンにアップデート
	* (iOS)LINE SDKのAppログイン機能が無効化
		* LINE SDK v4のバグにより、iOS 12でアプリログインが失敗するイシューがあり、Gamebase Line AdatperでWebログインのみサポートするように変更
	* (Unity)GamebaseMainActivityのPackage Nameが変更
		* com.toast.gamebase.activity.GamebaseMainActivity → com.toast.android.gamebase.activity.GamebaseMainActivity

### 2019.02.26

#### 機能改善/変更
* [SDK] 2.1.0
	* (共通)TransferKey API削除
		* issueTransferKey：TransferKey発行
		* requestTransfer：TransferKey検証
		
#### バグ修正
* [SDK] 2.1.0
	* (Android)Gamebaseの初期化前に、onActivityResult()が呼び出され、動作異常を起こすバグを修正
	* (iOS)GamecenterをGamebaseではない別のロジックによりログインした後、Gamebaseを通してGamecenterログインを試行した時、反応がないバグを修正

### 2019.01.29

```
Gamebase 2.0の改善された全体指標を活用するためには、SDKのアップデートが必要です。
```

#### 機能追加
* Console
	* Analytics：Gamebase 2.0指標新規オープン
	* アプリ：クライアントのデバッグログをリアルタイムで変更できる機能を追加
* [SDK] 2.0.0
	* (共通)Custom指標のためのAPIを追加(購入成功の場合、SDK内部で自動伝送)
		* setGameUserData：ゲームログイン後、ゲームユーザーレベル情報を伝送
		* traceLevelUpData：レベルアップ追跡のために、ゲームユーザーがレベルアップした時に呼び出す
    * (JavaScript)SDK新規配布

#### 機能改善/変更
* [SDK] 2.0.0
	* (Android)Push SDKアップデート(android:1.7.0)
	* (Android)Adapter API変更
		* Launching情報伝達
		* logout、withdraw APIにCallbackを追加
	* (iOS)IAP SDKアップデート
		* 決済失敗時、断続的にクラッシュが発生する現象を修正

#### バグ修正
* [SDK] 2.0.0
	* (iOS)iOS 12以上のシミュレータでdebugMode Onの状態でGamebaseを初期化すると、クラッシュが発生する現象を修正

### 2018.12.27

#### 機能追加
* Console
	* Push - 繰り返し送信機能を追加

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
* Console
	* 認証が完了した時、Rest APIリクエストにもログインページに移動するように修正
	* IAP Transaction照会フィルタの追加

### 2018.11.15

#### 機能追加
* Console
	* 中国語を適用
	* 会員：購入履歴にApp Store領収書の検証機能を追加	

#### 機能改善/変更
* Console
	* カレンダーの多言語サポートを追加
* [SDK] 1.14.2
	* (Android)メンテナンス時、データ構造でメンテナンス開始/終了時間を意味するepoch timeのタイプをStringからlongにタイプ変更：既存Gamebase Unityと連携後、メンテナンス呼び出し時にタイプ不一致でコールバックが来ない現象を修正
	* (iOS)Provider Profile取得メソッドを呼び出した時に返されるTCGBAuthProviderProfileオブジェクトのdescriptionメソッドのJSON文字列構造変更により、Gamebase iOS SDK 1.14.0とUnity Plugin 1.14.0適用時にcrashが発生することがある構造を修正

#### バグ修正
* Console
	* プッシュ：特定対象に送信後、登録されたプッシュをコピーして登録する時に登録失敗する問題を修正	
* [SDK] 1.14.2
	* (Android)エミュレータ環境でストアアプリ(PlayStore、OneStoreなど)がない状態で、"アプリインストール/アップデート"時にストア未チェックによるcrashバグを修正
	* (Unity)ShowWebView APIを呼び出した時、パラメータにCallbackを入れない場合、crashが発生する部分を修正
	* (Unity)iOS SDKのDeleted APIを呼び出すコードがあり、コンパイル時にエラーが発生するバグを修正
	
### 2018.10.23

#### 機能追加
* Console
	* IAP：決済情報メニューでApp Store領収書の検証機能を追加
* [SDK] 1.14.0
	* (共通)Gamebase Webviewでファイル添付機能を追加：AndroidのAPI 19、Kitcatでは正常に動作しません。

#### 機能改善/変更
* Console
	* IAP：決済情報メニューで、決済履歴ダウンロード検索条件を改善(1日→30日)
* [SDK] 1.14.0
	* (共通)利用停止/メンテナンスについて、ユーザーがコンソールに作成したメッセージをURLエンコードして伝送し、クライアントでデコードして処理するように修正
	* (iOS)Payco SDKのバージョンを1.2.4にアップデート
	* (Unity)GamebaseSDKSettingオブジェクトがあるシーンに戻る場合、オブジェクトが重複して生成されないように改善
	* Remove API：Webview、Network、Launching
		* (Android)5個
			- (void)Gamebase.WebView.showWebBrowser(Activity, String)
			- (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
			- (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
		* (iOS)9個
			- [TCGBUtil showToastWithMessage:duration:]
			- [TCGBWebView showWebBrowserWithURL:viewController:]
			- [TCGBWebView showWebViewWithURL:viewController:configuration:]
			- [TCGBLaunching addObserverOnChangedStatusNotification:]
			- [TCGBLaunching removeObserverOnChangedStatusNotification:]
			- [TCGBLaunching addUpdateStatusNotification]
			- [TCGBLaunching removeUpdateStatusNotification]
			- [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
			- [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
		* (Unity)7個
			- ShowWebBrowser(string url)
			- ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
			- ShowToast(string message, int duration)
			- AddUpdateStatusListener(GamebaseCallback.DataDelegate< GamebaseResponse.Launching.LaunchingStatus> callback)
			- RemoveUpdateStatusListener(GamebaseCallback.DataDelegate< GamebaseResponse.Launching.LaunchingStatus> callback)
			- AddOnChangedStatusListener(GamebaseCallback.DataDelegate< GamebaseNetworkType> callback)
			- RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate< GamebaseNetworkType> callback)
	* Deprecated  API 
		* (Android)2個
			- (void)Gamebase.WebView.showWebView(Activity, String)
			- (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
		* (iOS)1個
			- [TCGBGamebase languageCode]
		* (Unity)1個
			- GetLanguageCode()
* [SDK] Setting Tool
	* ポップアップおよびUI改善
	
#### バグ修正
* [SDK] 1.14.1
	* (Android)Auth APIを呼び出した後、コールバックで再度Auth APIを重複して呼び出した時、正常に呼び出されないバグを修正
	
### 2018.10.11

#### バグ修正
* Console
	* 利用停止：大量登録時に発生するエラーを修正
	
### 2018.09.20

#### バグ修正
* Console
	* 管理：ページアドレスエラーによるアラームページ処理失敗発生を修正

### 2018.09.13

#### 機能追加
* Console	
	* 会員：アカウントのIdP追加および削除機能を追加、IdP ID検索機能を追加
	* プッシュ：プッシュ状態別に送信履歴を照会する機能を追加
* [SDK] 1.13.0
	* (iOS)App Store Promotion IAPをサポートするためのAPIを追加


#### 機能改善/変更
* [SDK] 1.13.0
	* (共通)IAP SDK最新バージョン適用(android:1.5.1、iOS:1.6.0)
	* (Android)Push APIを呼び出した時、Gamebase初期化/ログイン状態に応じて、呼び出し失敗に対するエラーメッセージをより明確に改善
		* 初期化前に呼び出し：NOT_INITIALIZED(1)
		* 初期化後に呼び出した時、Pushモジュールがない：NOT_SUPPORTED(10)
		* 初期化成功およびログイン前の呼び出し：NOT_LOGGED_IN(2)		
	* (iOS)authProviderProfileWithIDPCode apiの呼び出し結果の構造が1depthに変更(Android、Unityと同じ)
	* (Unity)ログで表示するjsonデータの出力フォーマットを改善
* Console
	* 利用停止：アプリガードを利用した利用停止登録を行うUIの改善 - 機能offの時はデータ初期化、 Leaderboardデータ削除設定は、状態が'on'の場合にのみ表示するように改善
	
#### バグ修正
* [SDK] 1.13.0
	* (Android)NaverCafe SDKとの衝突で、Naverログイン時に発生するエラーを解決
	* (Unity)Unity 2017.2以上のバージョンでEditor Play Mode終了時、websocke close処理で発生するエラーを修正
* Console
	* App：情報修正時、削除ボタンの後ろの内容が見切れる現象を修正

### 2018.08.28

#### 機能追加
* Console	
	* 会員：アカウント状態の変更機能を追加、 Push Token照会を追加
	* 運営指標(ユーザー統計)：今日の退会者、加入当日の退会者指標を追加

#### 機能改善/変更
* [SDK] 1.12.2
	* (Android)WebSocketタイムアウト時(API呼び出し時間経過)、クラッシュが発生することがあるバグについて防御ロジック処理
	* (iOS)Google Auth Adapter、Naver Auth AdapterのCallback URL Scheme設定を改善
		* コンソールに"url_scheme_ios_only"値を設定しない場合、Default URL Schemeを設定するように改善：Default URL Schemeを使用するためには、XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.googleまたはtcgb.{Bundle ID}.naver登録が必要
	* (iOS)Payco Auth Adapterの改善
		* URL Schemeの未設定により、意図していないURL Schemeを呼び出す問題を修正：設定方法が変更され、アップデートするためには必ずURL Scheme設定が必要(XCode > Target > Info > URL Typesにtcgb.{Bundle ID}.paycoを登録)
* Console
	* 会員：IDマッピング履歴照会機能を追加(直近3ヶ月照会→照会期間を直接設定するように変更)
	* 購入(IAP)：決済情報Excelダウンロードを1日に制限、アイテム削除機能を削除
	
#### バグ修正
* [SDK] 1.12.2
	* (Android)auth-twitter-adapterを含んだ状態でTargetSdk 28でビルド時、初期化エラーが発生する問題を修正

### 2018.08.09

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
	
#### バグ修正
* [SDK] 1.12.1
	* (iOS)Naverログイン時、プロフィール情報照会失敗により、ログインできないバグを修正：プロフィール情報の照会に失敗してもログインは成功するように変更	
* Console
	* 決済履歴：'Reserved'状態で決済状態の変更ができないバグとExcelダウンロード時にフィルタリングが適用されない問題を修正
	
### 2018.07.24

#### 機能改善/変更
* [SDK] 1.12.0
	* (iOS)Gamebaseの初期化時、Debug Logに使用中のAdapterのバージョン情報、アプリのビルド情報を出力する機能を追加
	* (iOS)CocoaPodsを通して配布されるNaver Auth Adapterに含まれていたNaver ID Login SDKのバイナリが削除され、依存性設定方式に変更
* Console
	* Webクライアント登録の場合、選択できるサービス状態に対する制限を適用：アップデート権限、アップデート必須選択不可
* [SDK] Setting Tool
	* Setting Toolの最新バージョンがある場合、アップデート通知機能を追加
	* 内部null Exception修正
	
#### バグ修正
* [SDK] 1.12.0
	* (Unity)IssueTransferKey APIを呼び出した時、exceptionが発生するバグを修正
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


​	
### 2018.07.05

#### 機能追加
* Line IdP追加：iOS

#### 機能改善/変更
* [SDK] 1.11.1
	* (共通)ゲストログイン後にAddMapping成功時、loginForLastLoggedInPrivderをすると、AddMapping成功したIdPアカウントを使用してログインするように変更
	
#### バグ修正
* [SDK] 1.11.1
	* (共通)メンテナンス解除後にAPI進行(login/push/purchaseなど)ができないバグを修正
	* (Android)Gamebase.addObserver()を通してObserverMessageを受信した場合、 ObserverMessage.data.codeのタイプがintではなくStringになっているバグを修正
* Console
	* Windows client登録時、ストアコードの登録に誤りがある問題を修正

### 2018.06.26

#### 機能追加
* iOS Google IdP追加：iOS
* Twitter IdP追加：Android、iOS
* Line IdP追加：Androidのみ提供。iOSは2018年7月に提供予定です。
* Server API追加
	* getSimpleLaunching：クライアントアプリ起動時に提供されるLaunching情報の確認用API
	
#### 機能改善/変更
* [SDK] 1.11.0
	* (共通)LocalizedString日本語翻訳を追加
	* (共通)認証APIを呼び出した時に初期化、ログインをしない場合は明確にエラーコードを区別するように内部ロジックを改善
	* (Android)'android.permission.READ_PHONE_STATE'権限を削除
	* (Android)GamebaseConfiguration.Builderの必須設定値であるsetAppId、setAppVersionをコンストラクタで入力できるように変更
	* (Android)GamebaseConfiguration.BuilderのsetServerApiVerseion APIを削除
	* (Android)getAuthBanInfo() API、class AuthBanInfo名を変更：getBanInfo()、class BanInfo
	* Naver ID Login SDKアップデート：iOS(4.0.10)
* Sample App 
	* ServerPush機能およびObserver機能を追加
	* Gamebase SDKアップデート：Android(1.9.0)、iOS(1.9.0)、Unity(1.10.1)	
	
### 2018.06.11

#### バグ修正
* [SDK] 1.10.1
	* (Unity)Unity Adapterがない場合、AddMapping APIを呼び出した時、内部的にログインで処理していたバグを修正

### 2018.06.07

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
	
### 2018.05.29

#### 機能追加
* [Console]指標(Operating indicator)ダウンロード機能を追加
	* モニタリング > '同時接続変化'
	* ユーザー統計 > '日別指標変化'
	* グループ同時接続 > '日間グループ同時接続変化'


#### バグ修正
* [SDK] 1.9.1
	* (iOS) Gamebase WebView NavigationBar領域にタイトル、戻る、閉じるボタンが表示されない現象を修正

### 2018.05.18

#### 機能改善/変更
* [SDK] 1.9.0
	* Unity SDK(1.9.0) Google Adapterを新規バージョン(1.6.2)に変更して再配布
    	- 5/3配布されたUnity SDK(1.9.0)に適用されたGoogle Adapterを最新バージョンに変更(1.6.1→1.6.2)
  
### 2018.05.03

#### 機能追加
* Transfer機能追加
    - ゲストユーザーがマッピングを行わずに新しい端末に移行できる機能
    - (SDK共通)追加されたAPI 
		* Transfer Key発行API (IssueTransferKey)
		* 発行されたTransferKeyを使用して、アカウント移行をリクエストするAPI (RequestTransfer)
    - (console)会員メニューのマッピング履歴照会タブでTransfer履歴の確認が可能
* 利用停止の登録時、ユーザーのリーダーボード(ランキング)データを削除できるオプションを追加(TOAST Leaderboardを使用する場合に限る)
    - 利用停止登録メニューまたは、App Guard連携ページで使用可能

#### バグ修正
* [SDK] 1.9.0
	* (iOS) Naverアカウントを利用してログイン中にApp to Webログインを試行時、サーバーから受け取ったSchemeの形式が変更され、ログインされない現象を修正
    * (iOS) AdapterからUnderlyingErrorオブジェクトを受け取ってゲームユーザーに伝達するエラーオブジェクトを作成するロジックで、メッセージおよびUnderlying Errorの設定ができていないバグを修正
    * (Android) Heartbeatで、無効なユーザーと判定される場合、利用停止ポップアップが表示されないように修正(iOSと同じロジックで修正)

### 2018.04.12

#### バグ修正
* [SDK] 1.8.1
	* (Android. iOS)registerPushを呼び出す時、displayLanguageCodeをnullで渡すと、registerPushが失敗するバグを修正

### 2018.04.09

#### バグ修正
* [SDK] 1.8.1
	* (Unity)UnityAndroidプラットフォームで下記の機能を使用時、モジュールが初期化されずにNullReferenceExceptionが発生する問題を修正
		* Launching, Purchase, Push, Util, Webview

### 2018.04.05

#### 機能追加
* Kick out機能追加
    - 現在ゲーム中の全ユーザーの接続を切る機能(メンテナンスの時、ゲームで全ユーザーの接続を切りたい時に使用できる)
    - (console)メニュー追加
    - (SDK共通)kick outイベントを受け取れるAPIを追加
* メンテナンスWebページに、ユーザーがConsoleで入力したHTMLページを使用できるように機能を改善
    - 以前は、Gamebaseで提供するWebページや外部Webページ接続のみ可能だった
    - Webサーバーがない場合でも、メンテナンスページをユーザーが望む形式で作成できる。
* Observer機能の開発およびAPI追加
    - (SDK共通)メンテナンスなど、アプリ状態/ネットワーク状態/ゲームユーザー状態(利用停止)変更事項に対するListenerを、Observerの登録を通して一括処理できるAPIを追加

#### 機能改善/変更
* [SDK] 1.8.0
	* (共通)Observer機能追加に伴い、次のAPIがDeprecated：LaunchingStatus Listener、Network Listener(既存ユーザーは継続して使用可能)
	* (iOS)PAYCO簡単ログイン3rd SDK v1.2.2適用：ログイン成功時、トークン有効期限切れ情報(expires_in)を提供、iPhoneXログインUI改善
	* (iOS)iPhoneXをサポートするために、Webviewを使用したインターフェイスを修正

#### バグ修正
* 国コード(contry code)が10文字以上の場合、同時接続データが保存されない現象を修正
* [SDK] 1.8.0
	* (Setting Tool)Unity Facebook Adapterをチェックすると、エラーが発生するバグを修正

### 2018.03.13

#### バグ修正
* [SDK] 1.7.1
	* (Unity)Inspectorで設定されたSetDebugModeの値が反映されないバグを修正
	* (Unity)Standalone、WebGL：Display Languageで使用されるリソースファイルの欠損部分を修正
	* (Unity)Google Adapter 1.6.2配布：Google Adapter 1.6.1でAuthCodeがEmptyで返され、認証が失敗するバグを修正

### 2018.02.22

#### 機能追加
* [SDK] 1.7.0
	* Naver IdP認証追加
	* Display Language設定を追加：端末言語とは別に、ゲーム内でゲームユーザーの表示言語を設定できるようにDisplay言語を追加しました。

### 2018.01.25

#### 機能追加

* [Console]
	* [Push] PUSH入力値コピー機能追加
	* [Operating indicator>グループ同時接続]日間グループ同時接続変化グラフを追加

* [SDK] 1.6.0
	* (Unity)Standalone WinSDK追加
		* 64bitサポート
		* 認証サポート：facebook、google、payco

#### 機能改善/変更
* [Console]
	* [Operating indicator>モニタリング]プロジェクト作成前に設定されたシステムメンテナンス項目が表示される問題を修正
	* [App>アプリ]テスト端末登録画面を改善 - User IDログイン履歴を元に簡単に端末登録ができるように改善
	* [Operation>メンテナンス]メンテナンスプレビュー画面の改善

#### バグ修正
* [SDK] 1.6.0
	* (iOS)WebViewを呼び出した時、クラッシュが発生することがある部分に対する防御ロジック処理


### 2017.12.21

#### 機能追加

* [Console]
	* [Push]現地時間帯送信(Local Time Push)機能を追加
	* [Operating indicator>販売状況]ストア別売上チャートを追加
	* [Operating indicator>ユーザー統計]アプリリリース後のユーザー指標推移を確認するメニューを追加
	* [Operation>メンテナンス]メンテナンス状態で、ユーザーに表示するメンテナンスページの登録方法を追加
		* 従来：Gamebase独自提供ページ、外部ページURLを入力
		* 追加：Consoleで入力したメンテナンス内容を外部ページに伝達する機能を追加

* [SDK] 1.5.0
	* WebViewが閉じられる時に発生するClose Callbackを追加
	* WebViewで使用するCustom SchemeのEventを受け取れる機能を追加
	* Unity Setting Tool新規配布

#### 機能改善/変更
* [Console]
	* [App>クライアント]クライアントの状態変更時、前にゲームで登録したユーザー表示メッセージ情報を再使用できるように修正	

#### バグ修正
* [SDK] 1.5.0
	* (Unity)UnityEditorでゲストログインされない現象を修正
	* (Unity)TOAST ConsoleにFacebook認証情報を登録しないでGamebase.Login("facebook") APIを呼び出した場合、KeyNotFoundExceptionが発生したため、防御コードを追加


### 2017.11.30

#### 機能追加

* [Console]
	* [Management>アラーム] Webhookアラームを登録する機能を追加
	* [Operating indicator>モニタリング] Push送信履歴照会を追加

#### 機能改善/変更
* [Console]
	* [Operating indicator>モニタリング]チャートの色を変更、Timezoneイシュー。DAU計算ロジックを変更(Login時間基準→接続時間基準)
* [API] [メンテナンス照会API](./api-guide/#check-under-maintenance)結果をListから単一オブジェクトに変更

#### バグ修正
* [Console]
	* [Push] Push登録時、基本言語が選択されていなくても登録されるエラーを修正


### 2017.11.23

#### 機能追加

* [SDK] 1.4.0アップデート
	* (Unity)Gamebase Facebook Adapterを追加：Android、iOS、WebGL、Standalone PlatformおよびUnityEditorをサポート

#### 機能改善/変更
* [SDK] 1.4.0アップデート
	* (iOS)close/backボタンリソースがない時、'x', '<'などのテキストが表示されていたイシューをデフォルト値に変更

#### バグ修正
* [SDK] 1.4.0アップデート
	* (Android)Gamebase提供ポップアップを使用しない場合、利用停止情報がnullで返されるエラーを修正
	* (iOS)WebViewローンチ後、端末の回転時にNavigationBar Titleがresetされるエラーを修正
	* (iOS)WebViewのNavigationBar Heightをカスタマイズする時、NavigationBarの背景部分が重なって表示されるエラーを修正

### 2017.10.26

#### 機能追加

* [SDK] 1.3.0アップデート
	* Credentialを利用したAddMapping API追加

#### 機能改善/変更
* [Console]
	* TC Pushエラーコードに応じたメッセージ処理作業を適用
	* 利用停止テンプレートメッセージ登録画面をInput TextboxからTextAreaに変更
	* TC新規権限追加によって管理メニューが正常に表示されない問題を修正
* [SDK] 1.3.0アップデート	
	* (Unity)CredentialInfoを使用するLogin APIを呼び出した時、iOSPluginでJson解析がされないバグを修正
	
### 2017.09.21

#### 機能追加

* 利用停止(ユーザー処罰)機能を追加
* [SDK] 1.2.0アップデート
	* 利用停止ユーザーポップアップ表示
* [Console]
	* サポート(email、電話番号)登録
	* Banメニューオープン
	* Memberメニュー：ユーザー決済履歴照会機能を追加


#### 機能改善/変更

* [Console]
	* 各メニューで、ユーザーの国を基準に時間を表示
	* 販売状況で、小数点以下の価格を処理
	* 同時接続変化量アラームの送信で多言語サポート(英語/韓国語選択可能)

### 2017.08.24

#### 機能改善/変更

* [SDK] 1.1.6アップデート
	* Push API追加(for iOS)：SetSandboxMode

### 2017.07.20

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.5アップデート
	* システムポップアップAPIを追加(showAlertWithTitle)
	* 国コードを大文字で返すように変更(Android)
	* TCPush SDK 1.4.1にアップデート
	* IAP SDK 1.3.3.20170627にアップデート
* [Console]
	* 外部連携モジュールでエラー発生時、追跡のためのTrackingTimeを追加表示

### 2017.05.25

#### 機能改善/変更

* Gamebaseサービスの利用を中止した時、関連データを削除するためのバッチ機能を追加
* [SDK] 1.1.4アップデート
	* ランタイムのうち、決済Storeを変更できるAPIを提供
	* (Android)TCPushSdk v1.4適用、Tencent Push機能を提供
* [Console]
	* 多言語サポート
	* すべてのメニューのCreate、Update、Modify実行時にAudit log機能を追加

### 2017.04.20

#### 機能改善/変更

* [SDK] 1.1.3アップデート
	* (Android)ローンチ構造およびポップアップ/メンテナンスページ改善：カスタムメンテナンスページ設定機能を追加
	* (Android)認証構造の改善およびログ追加：認証AdapterおよびSDKバージョンログ出力

#### バグ修正
* [SDK] 1.1.3アップデート
	* (Android)Facebook SDK v4.19.0以上で初期化時にクラッシュするエラーを修正


### 2017.04.04

#### 機能改善/変更
* [SDK] 1.1.2アップデート
    * ゲームローンチ時、メンテナンス、緊急告知ポップアップを改善
    * Unity Pluginデバッグログ追加および例外詳細処理
* [API] [IAP](./api-guide/#purchaseiap) API連携：アイテム照会、未消費内訳照会
* [API] checkAccessToken APIレスポンス結果に、ログイン時に使用されたIdP関連情報を含むスペックを追加
* [Console]メンテナンス、緊急告知：クライアントバージョン選択時、ゲームで使用しないストアは表示されないように変更

#### バグ修正
* [Console] iOSクライアント登録時、ストアの異常表示を修正

### 2017.03.21

#### 機能改善/変更
* [SDK] 1.1.0アップデート
    * 外部AccessTokenを受け取って、idPLoginするインターフェイスを追加
    * [UI機能追加](./aos-ui)：Custom Webview、AlertDialog
* [API] [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API連携

### 2017.03.09

#### 新規サービスリリース
* ゲームで共通して必要な機能を提供し、簡単かつ効率的にゲーム開発ができるようにサポートするサービスです。
	* 多様な認証をサポート：ゲストログイン、3rd Party(Google、Facebook、GameCenterなど)認証
	* ログアウトおよび会員退会機能を提供
	* 1人のUserが複数の外部IDPを同時に使用できるようにmapping機能を提供
	* ゲーム運営のためのゲームアプリ状態管理、メンテナンス、緊急告知などの機能をWebコンソールで提供
	* リアルタイムに運営指標を確認できるWebコンソール画面を提供
	* TOAST Cloudサービスと連携：PUSH、IAP

