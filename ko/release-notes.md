## Game > Gamebase > 릴리스 노트

### 2020. 07. 14.

#### 기능 추가
* 이미지 공지: 표시 기간과 우선순위에 따라 게임 내 이미지 팝업 표시
    * [Console] 운영 > 이미지 공지: 메뉴 추가
    * [SDK] 2.12.0: 이미지 공지 표시 API 추가

#### 기능 개선/변경

* [Console] 
    * 구매(IAP) > 상품: 아이템 번호로 상품 조회 가능하도록 추가
    * 멤버 > 회원: 탈퇴 유예 상태의 유저를 정상 상태로 변경할 수 있도록 개선
    * 멤버 > 다운로드: 로그인 로그 이력에 deveiceKey, IdP 코드 항목 추가
* [SDK] 2.12.0
    * (iOS) Facebook SDK 업데이트(7.1.1)
    * (iOS) configuartion에 설정된 storeCode(default=AS)로 Gamebase 초기화 시도
    * (iOS) 콘텐츠를 로딩할 수 없는 웹뷰 출력 시 **닫기** 버튼이 없어 닫을 수 없는 문제 수정
    * (Unity) TOAST Unity SDK 업데이트(0.20.1.1)
    
### 2020. 06. 23.

#### 기능 추가
* [SDK] 2.11.0
	* 결제 API 추가: 상품 ID로 결제 요청, 추가 정보(UserPayload) 입력해 결제 완료 시 확인할 수 있음

#### 기능 개선/변경
* [Console] 
	* 구매(IAP) > 상품: 스토어 아이템 ID에 여러 개의 Gamebase 상품을 등록해 관리할 수 있도록 개선

### 2020. 06. 09.

#### 기능 개선/변경
* [Console] 
	* 멤버 > 회원:  **탈퇴 이력 조회** 화면에 탈퇴 유예 상태(탈퇴 유예, 탈퇴 취소, 즉시 탈퇴) 추가 표시
* [SDK] 2.10.1
	* (iOS) 사용자 푸시 설정 초기화 시 언어 코드가 설정되어 있지 않으면 디바이스 언어로 설정되도록 변경

#### 버그 수정
* [Console] 
	* 쿠폰 > 쿠폰 발급: 쿠폰 통계 다운로드 시 SMS로 발송한 내역이 다운로드되지 않는 문제 수정

* [SDK] 2.10.1
	* (Unity) iOS Plugin에서 ViewController가 설정되지 않아 로그인 호출 시 실패하는 문제 수정
	* (JavaScript) 초기화 시 StoreCode를 입력하지 않으면 오류가 발생하는 문제 수정

### 2020. 05. 26.

#### 기능 추가
* [Console] 
	* 쿠폰 > 쿠폰 발급: 발송 통계 기능, 쿠폰 발송 내역 다운로드 기능 추가
* [SDK] 2.10.0
	* (공통) 기존의 모든 이벤트 시스템을 통합하는 GamebaseEventHandler 추가
		* ServerPush, Observer 기능을 포함하고 있고, Promotion 결제 이벤트 및 Push 이벤트도 확인 가능

#### 기능 개선/변경
* [Console] 
	* 전체: 공통 디자인 가이드에 맞도록 버튼/태그 UI 수정
* [SDK] 2.10.0 
	* (Unity) StandaloneWebviewAdapter 내부의 CefWebview 버전 업데이트: v2.0.4
		* WebviewIndex 검증 로직을 개선
		* Webview 생성 시, 간헐적으로 NullReferenceException이 발생하는 오류를 개선
	* (Unity) GamebaseErrorCode에 소켓 연결에 관한 에러 코드 추가: SOCKET_CONNECTION_TIMEOUT, SOCKET_CONNECTION_FAIL

### 2020. 05. 12.

#### 기능 추가
* [SDK] 2.9.0
	* (Unreal) SDK 신규 배포
	
#### 기능 개선/변경
* [Console] 
	* 앱 > 앱: 탈퇴 유예 기간을 변경한 사용자의 토스트 계정을 저장하도록 개선
	* 멤버 > 회원: 매핑 이력 조회 시 정보가 제대로 보이지 않는 문제 수정
	* 구매(IAP) > 스토어: 테스트, 구) 원스토어는 신규 등록이 되지 않도록 수정

#### 버그 수정
* [SDK] 2.9.1
	* (Andoird) 매핑 이후 지표 레벨이 null이 되어 결제 지표에 정상적으로 반영되지 않는 오류 수정
	* (iOS) unreal 엔진에서 빌드 하면, warning을 빌드 오류로 판정해서 빌드가 안되는 부분을 수정

### 2020. 04. 29.

#### 버그 수정
* [SDK] 2.9.1 
	* (Unity) Initialize 이후 콘솔에서 클라이언트의 서비스 상태를 변경하면 오류가 발생하는 문제를 수정
		* 이슈 발생 버전: v2.8.0 이상	
		* 이슈가 있는 플랫폼: Standalone, WebGL, Editor
		
### 2020. 04. 28.

#### 기능 추가
* 탈퇴 유예 기능
	* [SDK] 2.9.0
		* (공통) API 추가: 탈퇴 유예 신청, 탈퇴 유예 신청 취소, 탈퇴 유예 상태에서 즉시 탈퇴, 유저의 탈퇴 유예 여부 확인
	* [Console]
		* 앱 > 앱: 탈퇴 유예 기간 설정할 수 있도록 기능 추가
		
#### 기능 개선/변경
* [SDK] 2.9.0
	* (공통) TOAST SDK 업데이트: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (공통) PAYCO Login SDK 업데이트: Android(v1.5.0), iOS(v1.4.0)
* [Console]
	* 전체 메뉴: 콘솔 버튼, 태그 디자인 수정
	* 운영 > 점검, 운영 > 공지, 푸시: 다국어 자동 번역 기능 지원
	* 멤버 > 회원: 탈퇴 유예 유저의 회원 조회 시 유예 만료 기간을 추가로 표시

### 2020. 04. 14.

#### 기능 개선/변경
* [Console] 
	* Analytics 공통: TUI 차트 버전 업데이트, Frequency7 지표에 적용
* [SDK] 2.8.1 
	* (공통) Analytics 전송 결과 확인을 위한 내부 지표 추가
	
#### 버그 수정
* [Console] 
	* Analytics 공통: 국가명이 길어질 경우 스크롤이 영역을 벗어나는 이슈 수정
	* Analytics > 실시간 모니터링: 데이터 저장 중에 조회 요청시 지표가 0으로 보이는 현상 수정
* [SDK] 2.8.1 
	* (Android) 프로세스 재시작 이후 크래쉬가 발생할 수 있는 코드를 수정
	* (JavaScript) credentialInfo 로그인에서 Hangame IdP로 로그인이 안되는 문제를 수정
	
### 2020. 03. 24.

#### 기능 추가
* [Console] 
	* 신규 메뉴 오픈: Analytics > 이용자 지표 > Frequency 7
		* DAU의 일주일간 방문수와 비율 정보를 제공합니다. 게임몰입도, 충성도 등을 한 눈에 파악할 수 있습니다.
	* 쿠폰 > 쿠폰 발급: 쿠폰 문자 발송 기능 추가
* [SDK] 2.8.0
	* (공통) 결제 및 상품 정보에 상품 타입 및 지역 가격 등의 정보를 추가
	* (Unity) StandaloneWebviewAdapter 내부의 CefWebview가 v2.0.1 버전으로 업데이트
		* PopupType이 PASS_INFO일 경우, 팝업을 띄우지 않고 팝업 정보를 전달하는 기능을 추가
 	* (Javascript) 한게임 채널링 지원: 한게임 IdP 인증, 한코인 결제 추가


#### 기능 개선/변경
* [Console] 
	* 앱 > 전송 지표 세팅: 미리 등록한 메타 필터만 전송 지표에 사용할 수 있도록 제한
		* 메타필터 개수 제한하여 그 이상 전송하는 경우 지표가 노출되지 않으니 주의해 주세요.: 레벨(5,000개), 월드/서버/채널(100개), 직업/클래스(100개)
* [SDK] 2.8.0 
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

#### 기능 추가
* 앱 > 앱: 점검 중 QA 테스트 단말기 등록시 IP 로도 등록할 수 있는 기능 추가	

#### 버그 수정
* [Console]
	* 의미에 맞지 않는 일본어 문구 수정
* [SDK] 2.6.1
	* (Android)Gamebase.initialize() 호출 전에 Gamebase.login() 을 호출할 때 크래쉬가 발생하는 문제 수정
	* (Android)TOAST Analytics User Data 를 java 주소 값으로 잘못 전송하는 문제 수정
	* (Android)IAP 상품을 활성화 시키지 않은 경우 발생하는 크래쉬 수정
	* (iOS)AddMapping(강제, Forcibly) 사용 시, 매핑이 되지 않는 문제 수정
	* (iOS)Unity Plugin으로 PushConfiguration의 displayLanguageCode를 설정하지 않을 경우, NSNull 객체에 의하여 크래시가 발생하는 문제를 수정
	
### 2019. 11. 26.

#### 버그 수정
* [Console]
	* 쿠폰 > 쿠폰발급: 세션 만료 후 쿠폰 다운로드 시 비정상적인 파일로 다운로드 되던 문제 수정
	* Analytics > 실시간 모니터링 > 대시보드: 어제 데이터 0으로 노출되는 현상 수정
	* TOAST상품(IAP, Push, AppGuard 등) 관련 메뉴 접근 시 상품이 비활성화 된 경우 비활성화 페이지가 정상 노출되지 않던 문제 수정	

### 2019. 11. 20.

#### 버그 수정
* [SDK] 2.6.1
	* (Unity)iOS Plugin 파일이 Package에 누락되어 iOS 빌드시 에러가 발생하여 해당 파일을 추가: 'toast_sdk_wrap.m'
	* (Unity)UnityEditor에서 Standalone 이외의 플랫폼으로 실행시 Store Code가 Empty로 입력되어 초기화에 실패하는 오류 수정
	* (Unity)Initialize API 내부 zone type 처리 부분에서의 오류로 NullReferenceException 발생하던 오류 수정

### 2019. 11. 13.

#### 버그 수정
* GamebaseSettingTool
	* Gamebase v2.6.0 업데이트 시, 파일이 정상적으로 변경되지 않는 오류 수정


### 2019. 11. 12.

```
Gamebase SDK 2.6.0 미만 버전에서 2.6.0으로 업그레이드 하는 경우
반드시 Upgrade Guide문서에 설명된 변경 사항을 반영해 주시기 바랍니다. 
가이드 위치: Game > Gamebase > Upgrade Guide
```

#### 기능 추가
* 쿠폰 서비스 신규 오픈: 쿠폰을 대량으로 생성하고 관리하는 기능
	* [Console] Coupon 메뉴 신규 오픈
	* [Server API] 쿠폰 확인 및 소비 API 추가
* 자동 결제 어뷰징 기능 추가
	* [Console] 구매(IAP) > 결제 어뷰징 모니터링 메뉴 신규 오픈
		* 결제 어뷰징 자동 제재 설정 기능
		* 결제 어뷰징 조건 검색 후 수동 이용 정지 가능
* Google 구독 결제 기능 추가
	* [SDK] 2.6.0 Android
* [SDK] 2.6.0
	* (공통) 데이터를 Log&Crash 에 전송하여 각종 분석에 이용할 수 있도록 TOAST Logger 추가
	* (iOS) Sign In with Apple 인증 추가
	* (Android) Gamebase Android SDK 가 Bintray 를 통해 배포되므로 gradle 설정만으로 Gamebase 를 사용할 수 있음

#### 기능 개선/변경
* [SDK] 2.6.0
	* (Unity) 로그인시 LaunchingStatus를 갱신하는 로직에 오류가 있어 수정
	* (Unity) Debug Log를 전송하는 기능을 Gamebase 콘솔에서 설정할 경우 Client에서 로그 전송을 무한 반복하는 오류 수정
* [Console]
	* 앱 > 앱: 서버 주소를 서비스 상태별(테스트, 심사중, 서비스)로 입력 받을 수 있도록 변경
	* 구매(IAP) > 결제 정보: 검색 조건 선택하여 검색할 수 있도록 UI 변경

### 2019. 10. 29.

#### 기능 개선/변경
* [Console]
	* Analytics: 파이 차트 툴팁 변경
	* Analytics > 실시간 모니터링: Push 발송 대상 추가 작업

### 2019. 10. 15.

#### 기능 개선/변경
* [SDK] 2.5.2
	* (iOS) UIWebView를 WKWebView로 교체
* Sample App
	* Gamebase SDK 업데이트(v2.4.0)
	* Smart Downloader 적용(v1.5.8), Leaderboard 적용
	* 기능 추가: 게임리소스 다운로드, Leaderboard, TAA 지표 연동, 단말기 이전 기능, 강제 매핑 기능
	* 개선/변경: ServerPush 리스너 추가, Observer 점검 여부 감지 추가
	* 게임 리뉴얼
		
#### 버그 수정
* [Console]	
	* 관리 > 권한: 권한 수정이 정상적으로 되지 않는 문제 수정
	* 모바일
		* Datepicker 선택 시 키보드가 활성화되는 현상 수정
		* Analytics: ARPPU 항목에 NRU 값이 노출되는 현상 수정
		
### 2019. 09. 24.

#### 기능 개선/변경
* [Console]
	* 앱 > 클라이언트: Web 클라이언트 등록 시에도 스토어를 선택하여 등록할 수 있도록 UI 수정
		
#### 버그 수정
* [Console]	
	* 관리 > 권한: 권한 수정이 정상적으로 되지 않는 문제 수정
	* 모바일
		* Datepicker 선택시 키보드가 활성화 되는 현상 수정
		* Analytics: ARPPU항목에 NRU 값이 노출되던 현상 수정
	
### 2019. 09. 10.

#### 기능 추가
* [Console]
	* Analytics: 채널/월드/서버, 직업/클래스 전송지표에 레벨 지표 추가 노출
	
#### 기능 개선/변경
* [Console]
	* Analytics: Grid 렌더링 성능 개선(tui-grid 4.4.x 적용)
* [SDK] 2.5.1
	* (iOS) GamebasePushAdapter에서 사용중인 TCPushSDK를 1.7.0으로 업데이트
		* TCPushSDK가 Static Library에서 Framework 파일로 변경되었으므로 프로젝트에 TCPushSDK.framework를 추가해야 합니다.
	
### 2019. 08. 27.

#### 기능 추가
* [SDK] 2.5.0
	* Console에서 입력한 CS URL을 웹뷰로 오픈하는 API 제공
	
#### 기능 개선/변경
* [Console]
	* Analytics: 차트 성능 개선

#### 버그 수정
* [SDK] Setting Tool 1.4.3
	* Script 파일의 위치를 Editor 폴더 아래로 이동하여 빌드 오류를 해결
	* Mac OS에서 Multilanguage에 Language 파일의 전체 경로를 지정하면 동작하지 않던 문제 수정
	
### 2019. 08. 02.

#### 버그 수정
* [SDK] Setting Tool 1.4.3
	* Script 파일의 위치를 Editor 폴더 아래로 이동하여 빌드 오류를 해결
	* Mac OS에서 Multilanguage에 Language 파일의 전체 경로를 지정하면 동작하지 않던 문제 수정

### 2019. 07. 23.

#### 기능 추가
* [Console]
	* 멤버 > 다운로드 신규 메뉴 오픈: 가입일, 마지막 로그인 시간 기준으로 게임 유저 목록을 조회하고 파일로 다운로드할 수 있음

#### 기능 개선/변경
* [Console] 모바일
	* 점검, 푸시 등록과 수정 가능
* [SDK] 2.4.4
	* (공통) 회원 오류 코드 포맷 변경
	* (Unity) GamebaseServerPushType에 키 추가(TRANSFER_KICKOUT)
* Setting Tool
	* 폴더 구조 변경: `기존 SettingTool을 완전히 삭제한 후 재설치해야 합니다.`
	* 다국어 지원 추가
	
#### 버그 수정
* [Console]
	* Analytics > 이용자 지표: 차트 X축 날짜 겹치는 문제 수정

#### 버그 수정
* [Console]
	* Analytics > 이용자 지표: 차트 x축 날짜 겹치는 이슈 수정
	
### 2019. 07. 11.

#### 기능 개선/변경
* [Console]Analytics
	* 레벨업 쿼리 성능 개선
	* 차트 내 min, max 정보 노출
	* 다국어 적용(중국어)

#### 버그 수정
* [SDK] 2.4.3
	* (iOS)인증 시도 시 오류가 발생했을 경우, 형식에 맞지 않는 오류 메시지 파싱 시도에 따른 크래시 발생 이슈 수정
	* (Unity)iOS와 Android로 빌드시 AddMappingForcibly API 가 동작하지 않는 오류 수정
	* (Unity)RequestRetryTransaction API 호출시 iOSPlugin에서 JSON 파싱 오류가 있어 수정

### 2019. 07. 01.

#### 버그 수정
* [Console]
	* 관리 > 알람: Webhook 설정 후 알람 설정값 수정에 실패하는 현상 수정

### 2019. 06. 27.

#### 버그수정
* [Console]
	* 이용정지: 이용정지 대량등록을 위한 파일 업로드 실패 현상 수정
* [SDK] Setting Tool 1.4.1
	* GamebaseSettingTool 실행시 기존 설정 정보를 가져오지 못하는 오류가 발생하여 수정

### 2019. 06. 25.

#### 기능 추가
* 전송 지표 기능 추가
    * [Console]Analytics > 전송지표: Level, Channel, Class별 지표 확인 메뉴 오픈
		* 실시간 현황
		* 레벨별 현황
		* 월드/서버/채널별 현황
		* 클래스/직업별 현황
		* 레벨업
		* 아이템 판매 현황
		* 아이템 판매 TOP 50

#### 기능 개선/변경
* [SDK] 2.4.2
	* (공통)LaunchingInfo에 JSON string 형식의 TOAST Launching 정보를 추가
	* (iOS)LINE SDK 업데이트(v5.0.1)
		* LINE Adpater의 최소 지원 OS 버전이 iOS 10으로 변경 
		* LINE 앱을 통한 로그인 기능 추가

#### 버그수정
* [SDK] 2.4.2
	* (공통)Analytics 버그 수정: 로그아웃, 탈퇴, 계정 이전 시 저장된 지표 데이터를 초기화 하도록 수정
	* (iOS)네트워크 연결 문제로 간헐적으로 크래시가 발생하던 현상 수정

### 2019. 06. 13.

#### 버그수정
* [SDK] 2.4.1
	* (iOS)Analytics 지표 전송 시 일부 파라미터가 누락 되어 지표가 제대로 출력되지 않는 버그 수정
	
### 2019. 05. 28.

#### 기능 추가
* HANGAME mix 일본결제 추가
    * [SDK] 2.4.0
    	* (Unity)Standalone 일본 외부결제 추가
    	* (Unity)Standalone 일본 HANGAME 인증 추가
    * [Console] 
    	* 구매 > 스토어: 'HANGAME mix(JAPAN)' Store 추가
    	* 앱 > 클라이언트: Windows 클라이언트 등록 시 스토어 설정 항목 추가
    	* 앱 > 설치URL: Windo 설치 URL 추가 시 스토어 설정 항목 추가

#### 기능 개선/변경
* [SDK] 2.4.0
	* (공통) 지표관련 Class 변경
        * LevelUpData Class: userLevel, levelUpTime 파라미터가 필수로 변경 / 그 외 필드 삭제 [자세히보기 [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#game-user-data-settings)]
        * GameUserData Class: classId(게임유저의 직업) 필드 추가 [자세히보기 [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#level-up-trace)]
    * (Android)Naver SDK 버전 업데이트(v4.2.5): Naver SDK 버그 수정(Naver 로그인 도중에 앱 아이콘을 통해 앱을 재시작할 경우, Activity가 강제종료 되는 이슈로 인해 인증 프로세스가 중단되는 이슈가 해결)
    * (Unity)StandaloneWebview가 32bit Build를 지원 (SDK 용량 53.6MB에서 99.2MB로 증가)
* [Server]
    * LTV 쿼리 수정 및 failover 로직 수정
* [Console]
    * LTV Grid ComplexColumns 지원 및 엑셀 다운로드 지원

### 2019. 05. 16.

#### 기능 추가
* [Console]
	* 단말기 이전 기능 추가(신규 메뉴)
		* 앱 > 기기 이전(Transfer account): 기기이전 기능 사용을 위한 설정값 저장
		* 회원 > 기기 이전: 발급된 키의 상태 및 이력 조회

#### 기능 개선/변경
* [Console]
	* 앱: AppleGameCenter, China IdP에 '토큰재검증' 옵션 Off
		* 해당 IdP에서는 실제 외부IdP 체크하지않고 내부토큰만 체크하므로 의미없는 옵션이므로 제거
	* 회원: 매핑 추가 가능한 조건 변경
		* 기존:Guest계정
		* 변경:Guest계정, Missing 계정

#### 버그수정
* [SDK] 2.3.1
	* (Android)2.3.0버전에서 Twitter 로그인 되지 않던 문제 수정
* [Console]
	* 회원: 구매 이력에서 영수증 검증이 되지 않던 문제 수정
	* Kickout: 조회 요청시 인증체크 추가하여 비정상 동작하던 이슈 수정
	
### 2019. 04. 23.

```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

#### 기능 추가
* [SDK] 2.3.0
	* (Android/Unity)중국스토어 인증/결제 추가

#### 기능 개선/변경
* [SDK] 2.3.0
	* (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"
	* (Android)최근 로그인한 Provider로 로그인 및 웹소켓 응답 실패를 받았을 경우(Timeout, network disable 등) AuthToken을 삭제 처리하지 않도록 수정
	* (Android)IdP로그인 시 AuthAdapter 내부에서 발생하는 MemoryLeak을 수정

### 2019. 04. 11.

#### 기능 개선/변경
* [SDK] 2.2.2
	* (Unity)SDK 로그 개선
* [Console]
	* Analytics 메뉴 다국어 적용
	* 보안검수 관련 취약점 패치

#### 버그수정
* [SDK] 2.2.2
	* (Android)Gamebase 초기화 이전 TransferAccount API 호출시, 콜백이 오지 않는 이슈를 수정
	* (iOS)showBlockingPopup을 NO로 설정 할 경우 Gamebase 초기화 콜백이 호출되지 않는 이슈를 수정
	* (Unity)AddMappingForcibly API를 호출하면 크래쉬가 발생하여 수정

### 2019. 04. 02.

#### 버그수정
* [SDK] 2.2.1
	* (Unity) Unity Editor에서 Android 플랫폼을 선택하고 플레이를 하면 initialize시 서버에서 에러가 발생하는 이슈 수정

### 2019. 03. 26.

#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    - (SDK공통)추가된 API 
		* TransferAccountInfo 발급 API (issueTransferAccount)
		* 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
		* 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
		* 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)		
	- (Server API)
		* 발급된 TransferAccount의 ID/PW 검증하는 서버 API (validateTransferAccount)
    - (console)회원메뉴의 매핑이력조회 탭에서 Transfer 이력 확인이 가능
* 강제매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
	- (SDK공통)추가된 API 
		* 강제매핑하는 API (addMappingForcibly)

#### 기능 개선/변경
* [SDK] 2.2.0
	* (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트
	* (iOS)LINE SDK의 App 로그인 기능이 비활성화
		* LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adatper에서 Web 로그인만 지원하도록 변경
	* (Unity)GamebaseMainActivity의 Package Name이 변경
		* com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

### 2019. 02. 26.

#### 기능 개선/변경
* [SDK] 2.1.0
	* (공통)TransferKey API 삭제
		* issueTransferKey : TransferKey 발급
		* requestTransfer : TransferKey 검증
		
#### 버그수정
* [SDK] 2.1.0
	* (Android)Gamebase 초기화 이전, onActivityResult() 가 호출되면서 이상 동작하던 버그 수정
	* (iOS)Gamecenter를 Gamebase가 아닌 다른 로직에의해 로그인 한 후, Gamebase를 통하여 Gamecenter로그인을 시도할 때, 반응이 없는 버그 수정

### 2019. 01. 29.

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* Console
	* Analytics : Gamebase 2.0 지표 신규 오픈
	* 앱 : 클라이언트의 디버그 로그를 실시간으로 변경할 수 있는 기능 추가
* [SDK] 2.0.0
	* (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
		* setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
		* traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출
    * (JavaScript)SDK 신규 배포

#### 기능 개선/변경
* [SDK] 2.0.0
	* (Android)Push SDK 업데이트(android:1.7.0)
	* (Android)Adapter API 변경
		* Launching 정보 전달
		* logout, withdraw API에 Callback 추가
	* (iOS)IAP SDK 업데이트
		* 결제 실패 시 간헐적으로 크래시가 발생하던 현상 수정

#### 버그수정
* [SDK] 2.0.0
	* (iOS)iOS 12 이상의 시뮬레이터에서 debugMode On 상태로 Gamebase 초기화 시 크래시가 발생하던 현상 수정

### 2018. 12. 27.

#### 기능 추가
* Console
	* Push - 반복발송 기능 추가

#### 기능 개선/변경
* [SDK] 1.14.5
	* deprecated 되었던 다음 API가 제거되었습니다.
		* (void)Gamebase.WebView.showWebBrowser(Activity, String)
		* (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
		* (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
	* 결제 모듈(gamebase-adapter-purchase-iap) 수정되었습니다.
		* IAP SDK를 1.5.2로 업데이트
		* Client에서는 사용되지 않는 IAP TEST Store 제거
		* 결제 재처리 로직(requestRetryTransaction)에서 데이터가 불완전할 때 호출이 실패하는 문제를 수정
		* 크래쉬를 방지하기 위해 모든 IAP SDK 호출부에 예외 처리
* Console
	* 인증이 풀렸을 때 Rest API 요청에도 로그인페이지로 이동하도록 수정
	* IAP Transaction 조회 필터 추가

### 2018. 11. 15.

#### 기능 추가
* Console
	* 중국어 적용
	* 회원 : 구매이력에 App Store 영수증 검증 기능 추가	

#### 기능 개선/변경
* Console
	* 달력 다국어 지원 추가
* [SDK] 1.14.2
	* (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정
	* (iOS)Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 crash가 발생될 수 있는 구조 수정

#### 버그수정
* Console
	* 푸시 : 특정대상 발송 이후 등록된 푸시건을 복사하여 등록할 때 등록 실패하던 문제 수정	
* [SDK] 1.14.2
	* (Android)에뮬레이터 환경에서 스토어앱(PlayStore, OneStore 등)이 없는 상태에서 "앱 설치/업데이트"시 스토어 미체크로 인한 crash 버그를 수정
	* (Unity)ShowWebView API 호출시 파라메타에 Callback을 넣지 않으면 crash가 발생되는 부분 수정
	* (Unity)iOS SDK의 Deleted API를 호출하는 코드가 있어 컴파일시 오류가 발생 되는 버그 수정
	
### 2018. 10. 23.

#### 기능 추가
* Console
	* IAP : 결제 정보메뉴에서 App Store 영수증 검증 기능 추가
* [SDK] 1.14.0
	* (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
	
#### 기능 개선/변경
* Console
	* IAP : 결제 정보메뉴에서 결제내역 다운로드 검색 조건 개선(1일 ->30일)
* [SDK] 1.14.0
	* (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
	* (iOS)Payco SDK의 버전이 1.2.4로 업데이트 
	* (Unity)GamebaseSDKSetting 오브젝트가 있는 씬으로 돌아갈 경우 오브젝트가 중복으로 생기지 않도록 개선
	* Remove API : Webview, Network, Launching
		* (Android)5개
			- (void)Gamebase.WebView.showWebBrowser(Activity, String)
			- (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
			- (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
		* (iOS)9개
			- [TCGBUtil showToastWithMessage:duration:]
			- [TCGBWebView showWebBrowserWithURL:viewController:]
			- [TCGBWebView showWebViewWithURL:viewController:configuration:]
			- [TCGBLaunching addObserverOnChangedStatusNotification:]
			- [TCGBLaunching removeObserverOnChangedStatusNotification:]
			- [TCGBLaunching addUpdateStatusNotification]
			- [TCGBLaunching removeUpdateStatusNotification]
			- [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
			- [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
		* (Unity)7개
			- ShowWebBrowser(string url)
			- ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
			- ShowToast(string message, int duration)
			- AddUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback) 
			- RemoveUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
			- AddOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			- RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			
	* Deprecated  API 
		* (Android)2개
			- (void)Gamebase.WebView.showWebView(Activity, String)
			- (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
		* (iOS)1개
			- [TCGBGamebase languageCode]
		* (Unity)1개
			- GetLanguageCode()
* [SDK] Setting Tool		
	* 팝업 및 UI 개선
	
#### 버그수정
* [SDK] 1.14.1
	* (Android)Auth API 호출 후 콜백에서 다시 Auth API 중복 호출시 정상 호출이 되지 않는 버그 수정
	
### 2018. 10. 11.

#### 버그수정
* Console
	* 이용정지 : 대량등록시 발생하던 오류 수정
	
### 2018. 09. 20.

#### 버그수정
* Console
	* 관리 : 페이지 주소 오류로 인한 알람페이지 처리 실패 발생 수정

### 2018. 09. 13.

#### 기능 추가
* Console	
	* 회원: 계정의 IdP 추가 및 삭제 기능 추가, IdP ID 검색 기능 추가
	* 푸시: 푸시상태별로 발송이력 조회하는 기능 추가
* [SDK] 1.13.0
	* (iOS)App Store Promotion IAP를 지원하기 위한 API 추가


#### 기능 개선/변경
* [SDK] 1.13.0
	* (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
	* (Android)Push API 호출 시, Gamebase 초기화/로그인 상태에 따라 호출 실패에 대한 에러 메시지를 보다 명확하게 개선
		* 초기화 전 호출 : NOT_INITIALIZED(1)
		* 초기화 이후 호출시 Push 모듈이 없음 : NOT_SUPPORTED(10)
		* 초기화 성공 및 로그인 이전 호출 : NOT_LOGGED_IN(2)		
	* (iOS)authProviderProfileWithIDPCode api의 호출 결과의 구조가 1depth로 변경 (Android, Unity와 통일)
	* (Unity)로그에서 보여주는 json 데이터를 알아보기 쉽도록 출력 포맷 개선
* Console
	* 이용정지 : 앱가드를 이용한 이용정지 등록하는 UI 개선 - 기능 off시 데이터 초기화, Leaderboard 데이터 삭제 설정을 상태가 'on'인 경우에만 노출하도록 개선
	
#### 버그수정
* [SDK] 1.13.0
	* (Android)NaverCafe SDK와의 충돌로 Naver 로그인시 발생하던 오류 해결
	* (Unity)Unity 2017.2 이상 버전에서 Editor Play Mode 종료 시 websocke close 처리에서 발생하던 오류 수정
* Console
	* App : 정보 수정시 삭제버튼 뒤의 내용이 잘리는 현상 수정
		
### 2018. 08. 28.

#### 기능 추가
* Console	
	* 회원: 계정상태 변경 기능 추가, Push Token 조회 추가
	* 운영지표(유저통계) : 오늘 탈퇴자, 당일 가입 후 탈퇴자 지표 추가

#### 기능 개선/변경
* [SDK] 1.12.2
	* (Android)WebSocket 타입아웃시 (API 호출 시간 경과), 크래시가 날 수 있는 버그에 대해 방어로직 처리
	* (iOS)Google Auth Adapter, Naver Auth Adapter의 Callback URL Scheme 설정 개선
		* 콘솔에 "url_scheme_ios_only" 값을 설정하지 않으면 Default URL Scheme을 설정 하도록 개선 : Default URL Scheme을 사용하기 위해서는 XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.google 또는 tcgb.{Bundle ID}.naver 등록 필요
	* (iOS)Payco Auth Adapter 개선
		* URL Scheme 미설정으로 인해 의도치 않은 URL Scheme을 호출하던 문제 수정 : 설정 방법이 변경되어 업데이트를 위해서는 반드시 URL Scheme 설정 필요 (XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.payco를 등록)
* Console
	* 회원 : 아이디 매핑 이력 조회 기능 추가(최근 3개월 조회 -> 조회기간 직접 설정하도록 변경)
	* 구매(IAP) : 결제정보 엑셀 다운로드 1일로 제한, 아이템 삭제 기능 삭제
	
#### 버그수정
* [SDK] 1.12.2
	* (Android)auth-twitter-adapter 를 포함한 상태에서 TargetSdk 28로 빌드시 초기화 에러가 발생하는 문제 수정

### 2018. 08. 09.

#### 기능 개선/변경
* [SDK] 1.12.1
	* (공통)IAP SDK 최신버전 적용 (1.5.0)
	* (공통)Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
	* (공통)점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
	* (공통)IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
	* (공통)인증 API 중복 호출시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
	* (Android)TencentPush SDK 업데이트 (3.2.3)
	* (Android)Onestore v17(API v5) 지원 : Gamebase에서는 v16(스토어코드=TS)은 제공하지 않습니다.
	* (iOS)에러코드 추가 : Gamecenter 로그인 거부(TCGB_ERROR_IOS_GAMECENTER_DENIED)
* [SDK] Setting Tool
	* 폴더명 변경 : TOAST -> Toast
	* 에러발생시 팝업 알림 추가 : File Download 실패, File Extract 실패, XML 파싱 실패
	
#### 버그수정
* [SDK] 1.12.1
	* (iOS)Naver 로그인 시 프로필 정보 조회 실패로 인해 로그인이 불가능한 버그 수정 : 프로필 정보 조회 실패하더라도 로그인은 성공하도록 변경	
* Console
	* 결제 내역: 'Reserved'상태에서 결제 상태 변경이 되지 않는 버그와 엑셀 다운로드 시 필터링이 적용되지 않던 문제 수정
	
### 2018. 07. 24.

#### 기능 개선/변경
* [SDK] 1.12.0
	* (iOS)Gamebase 초기화 시 Debug Log에 사용중인 Adapter들의 버전 정보, 앱의 빌드 정보를 출력하는 기능이 추가 
	* (iOS)CocoaPods을 통해 배포 되는 Naver Auth Adapter에서 포함하고 있던 Naver ID Login SDK의 바이너리가 제거 되고 의존성 설정 방식으로 변경
* Console
	* Web 클라이언트 등록일 경우 선택할 수 있는 서비스상태에 대한 제한 적용 : 업데이트권한, 업데이트필수 선택 불가능
* [SDK] Setting Tool
	* Setting Tool 최신 버전이 있을 경우 업데이트 알림 기능 추가 
	* 내부 null Exception 수정
	
#### 버그수정
* [SDK] 1.12.0
	* (Unity)IssueTransferKey API 호출시 exception 발생하던 버그 수정
	* (Unity)Unity Google Adapter 제거 : 기존에 GoogleAdapter 사용중인 개발사는 아래 업데이트 가이드 참고
	
**Unity Google Adapter 업데이트 가이드**

* Unity SDK 1.6.0이상 1.11.0 이상 버전을 사용하는 경우 1.12.0 버전으로 업데이트 하기 전에 아래 내용을 필히 숙지하셔야 합니다.(1.6.0 미만 버전 사용중인 경우에는 GoogleAdapter를 미사용하기 때문에 영향이 없습니다.)
	1. Setting Tool 설정
        * GoogleAdapter가 제거됨에 따라 더이상 Unity 탭에서 Google 항목이 노출되지 않는다.
        * Google 인증을 사용할 경우에는 각 플랫폼 탭에서 Google 항목을 활성화한다.
            * Android > Authentication > Google 선택해서 설정
            * iOS > Authentication > Google 선택해서 설정
    2. Gamebase Login API (변경 없음)
        * Gamebase.Login(GamebaseAuthProvider.GOOGLE, callback);
    3. GPGS 기능을 사용하는 경우
        * GPGS SDK for Unity 유지
        * GPGS 관련 로직은 앱에서 별도로 관리
    4. GPGS 기능을 사용하지 않는 경우
        * GPGS SDK for Unity 삭제 

### 2018. 07. 05.

#### 기능 추가
* Line IdP 추가 : iOS

#### 기능 개선/변경
* [SDK] 1.11.1
	* (공통)Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경
	
#### 버그수정
* [SDK] 1.11.1
	* (공통)점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정
	* (Android)Gamebase.addObserver()를 통해 ObserverMessage를 수신하였을 경우, ObserverMessage.data.code의 타입이 int가 아니라 String인 버그를 수정
* Console
	* Windows client 등록 시 스토어코드가 잘못 등록되던 문제 수정

### 2018. 06. 26.

#### 기능 추가
* iOS Google IdP 추가 : iOS
* Twitter IdP 추가 : Android, iOS
* Line IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
* Server API 추가 
	* getSimpleLaunching : 클라이언트 앱 기동시 제공되는 Launching 정보 확인용 API
	
#### 기능 개선/변경
* [SDK] 1.11.0
	* (공통)LocalizedString 일본어 번역 추가
	* (공통)인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
	* (Android)'android.permission.READ_PHONE_STATE' 권한 제거
	* (Android)GamebaseConfiguration.Builder의 필수 설정값인 setAppId, setAppVersion을 생성자에서 입력할 수 있도록 변경
	* (Android)GamebaseConfiguration.Builder 의 setServerApiVerseion API를 제거
	* (Android)getAuthBanInfo() API, class AuthBanInfo 이름을 변경 : getBanInfo(), class BanInfo
	* Naver ID Login SDK 업데이트 : iOS(4.0.10)
* Sample App 
	* ServerPush 기능 및 Observer 기능 추가
	* Gamebase SDK 업데이트 : Android(1.9.0), iOS(1.9.0), Unity(1.10.1)	
	
### 2018. 06. 11.

#### 버그수정
* [SDK] 1.10.1
	* (Unity)Unity Adapter가 없는 경우 AddMapping API 호출 시 내부적으로 로그인으로 처리하던 버그 수정

### 2018. 06. 07.

#### 기능 추가
* [SDK] 1.10.0
	* (Unity)StandaloneWebviewAdapter: html source rendering 지원	

#### 기능 개선/변경
* [SDK] 1.10.0
	* (Unity)Unity Adapter의 interface가 수정
		* v1.10.0 이상 사용 시에는 UnityAdapter 버전 업그레이드가 필요(GamebaseUnitySDK_FacebookAdapter_v1.5.0, GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
	* (Unity)Login API 호출 시 Unity Adapter가 없는 경우 네이티브(Android/iOS)의 로그인 API를 호출하도록 로직 변경 : facebook, Google
	* (Unity)각 Adapter 폴더 구조 및 이름 오타 수정
		* 경로: Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
		* 오타: Adapater => Adapter	
	
### 2018. 05. 29.

#### 기능 추가
* [Console] 지표(Operating indicator) 다운로드 기능 추가
	* 모니터링 > '동접변화'
	* 유저통계 > '일별지표변화'
	* 그룹동접 > '일간그룹동접변화'


#### 버그수정
* [SDK] 1.9.1
	* (iOS) Gamebase WebView NavigationBar 영역에 타이틀, 뒤로가기, 닫기 버튼이 나타나지 않는 현상을 수정

### 2018. 05. 18.

#### 기능 개선/변경
* [SDK] 1.9.0
	* Unity SDK(1.9.0) Google Adapter 신규버전(1.6.2)으로 교체하여 재배포
    	- 5/3 배포된 Unity SDK(1.9.0)에 적용된 Google Adapter를 최신버전으로 교체(1.6.1->1.6.2)
    
### 2018. 05. 03.

#### 기능 추가
* Transfer 기능 추가
    - guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    - (SDK공통)추가된 API 
		* Transfer Key 발급 API (IssueTransferKey)
		* 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)
    - (console)회원메뉴의 매핑이력조회 탭에서 Transfer 이력 확인이 가능
* 이용정지 등록시 사용자의 리더보드(랭킹) 데이터를 삭제할 수 있는 옵션 추가(TOAST Leaderboard를 사용하는 경우에 한함)
    - 이용정지 등록 메뉴를 이용하거나 App Guard 연동 페이지에서 사용 가능

#### 버그 수정
* [SDK] 1.9.0
	* (iOS) Naver계정을 이용한 로그인 중 App to Web 로그인 시도 시, 서버로부터 받아온 Scheme의 형식이 변경되어, 로그인이 되지 않는 현상 수정
    * (iOS) Adapter로부터 UnderlyingError 객체를 받아서 유저에게 전달되는 에러객체를 생성하는 로직에서 메시지 및 Underlying Error의 설정이 되지 않는 버그 수정
    * (Android) Heartbeat 에서 잘못된 사용자로 판정되는 경우 이용정지 팝업이 뜨지 않도록 수정(iOS 와 동일한 로직으로 수정)

### 2018. 04. 12.

#### 버그 수정
* [SDK] 1.8.1
	* (Android. iOS)registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 2018. 04. 09.

#### 버그 수정
* [SDK] 1.8.1
	* (Unity)UnityAndroid 플랫폼에서 아래 기능 사용 시 모듈 초기화가 되지 않아 NullReferenceException이 발생하여 수정
		* Launching, Purchase, Push, Util, Webview

### 2018. 04. 05.

#### 기능 추가
* Kick out 기능 추가
    - 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    - (console)메뉴 추가
    - (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    - 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    - 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    - (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
	* (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
	* (iOS)페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
	* (iOS)iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정

#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정
* [SDK] 1.8.0
	* (Setting Tool)Unity Facebook Adapter를 체크하면 에러가 나는 버그 수정

### 2018. 03. 13.

#### 버그 수정
* [SDK] 1.7.1
	* (Unity)Inspector에서 설정된 SetDebugMode 값이 반영 안 되던 버그 수정
	* (Unity)Standalone, WebGL: Display Language에서 사용되는 리소스 파일 누락 부분 수정
	* (Unity)Google Adapter 1.6.2 배포: Google Adapter 1.6.1에서 AuthCode가 Empty로 반환되어 인증 실패하는 버그 수정

### 2018. 02. 22.

#### 기능 추가
* [SDK] 1.7.0
	* Naver IdP 인증 추가
	* Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 2018. 01. 25.

#### 기능 추가

* [Console]
	* [Push] PUSH 입력값 복사기능 추가
	* [Operating indicator > 그룹 동접] 일간 그룹 동접 변화 그래프 추가

* [SDK] 1.6.0
	* (Unity)Standalone WinSDK 추가
		* 64비트 지원
		* 인증 지원 : facebook, google, payco

#### 기능 개선/변경
* [Console]
	* [Operating indicator > 모니터링] 프로젝트 생성 이전 설정된 시스템 점검 항목이 노출 되는 문제 수정
	* [App > 앱] 테스트 단말기 등록화면 개선 - User ID 로그인이력을 바탕으로 손쉽게 단말기 등록가능하도록 개선
	* [Operation > 점검] 점검 미리보기 화면 개선

#### 버그 수정
* [SDK] 1.6.0
	* (iOS)WebView 호출시, 크래시가 일어날 수 있는 부분에 대한 방어로직 처리


### 2017. 12. 21.

#### 기능 추가

* [Console]
	* [Push] 현지시간대 발송(Local Time Push) 기능 추가
	* [Operating indicator > 판매 현황] 마켓별 매출 차트 추가
	* [Operating indicator > 유저 통계] 앱 출시 이후의 사용자 지표 추이 확인하는 메뉴 추가
	* [Operation > 점검] 점검상태에서 사용자에게 보여주는 점검페이지 등록 방법 추가
		* 기존 : Gamebase 자체 제공 페이지, 외부 페이지 URL 입력
		* 추가 : Console에서 입력한 점검내용을 외부페이지에 전달하는 기능 추가

* [SDK] 1.5.0
	* WebView가 닫힐 때 발생하는 Close Callback 추가
	* WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
	* Unity Setting Tool 신규 배포

#### 기능 개선/변경
* [Console]
	* [App > 클라이언트] 클라이언트 상태 변경시 이전에 게임에서 등록한 사용자 노출메시지 정보를 재사용할 수 있도록 수정	

#### 버그 수정
* [SDK] 1.5.0
	* (Unity)UnityEditor에서 Guest로그인이 되지 않는 현상 수정
	* (Unity)TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가


### 2017. 11. 30.

#### 기능 추가

* [Console]
	* [Management > 알람] Webhook 알람 등록하는 기능 추가
	* [Operating indicator > 모니터링] Push 발송 이력 조회 추가

#### 기능 개선/변경
* [Console]
	* [Operating indicator > 모니터링] 차트 색상 변경, Timezone 이슈. DAU 계산로직 변경(Login시간기준->접속시간기준)
* [API] [점검 조회 API](./api-guide/#check-under-maintenance) 결과를 List 에서 단일 객체로 변경

#### 버그 수정
* [Console]
	* [Push] Push 등록 시 기본언어가 선택되어 있지 않아도 등록되는 오류 수정


### 2017. 11. 23.

#### 기능 추가

* [SDK] 1.4.0 업데이트
	* (Unity)Gamebase Facebook Adapter가 추가 : Android, iOS, WebGL, Standalone Platform 및 UnityEditor 지원

#### 기능 개선/변경
* [SDK] 1.4.0 업데이트
	* (iOS)close/back 버튼 리소스가 없을 때, "x", "<" 등의 텍스트로 노출되던 이슈를 디폴트 값으로 대체

#### 버그 수정
* [SDK] 1.4.0 업데이트
	* (Android)Gamebase 제공 팝업을 사용하지 않는 경우 이용정지 정보가 null로 리턴되는 오류 수정
	* (iOS)WebView 런치 후, 기기 회전시 NavigationBar Title 이 reset이 되는 오류 수정
	* (iOS)WebView의 NavigationBar Height을 커스터마이징 할 때, NavigationBar 배경 부분이 겹쳐서 노출되는 오류 수정

### 2017. 10. 26.

#### 기능 추가

* [SDK] 1.3.0 업데이트
	* Credential을 이용한 AddMapping API추가

#### 기능 개선/변경
* [Console]
	* TC Push 에러코드에 따른 메시지 처리 작업 적용
	* 이용 정지 템플릿 메시지 등록 화면을 Input Textbox 에서 TextArea로 변경
	* TC 신규 권한 추가에 따라 관리메뉴가 정상적으로 보이지 않던 문제 수정
* [SDK] 1.3.0 업데이트	
	* (Unity)CredentialInfo를 사용하는 Login API호출 시 iOSPlugin에서 Json 파싱이 안되던 버그를 수정
	
### 2017. 09. 21.

#### 기능 추가

* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
	* 이용정지 사용자 팝업 노출
* [Console]
	* 고객센터(email, 전화번호) 등록
	* Ban 메뉴 오픈
	* Member메뉴 : 사용자 결제이력 조회 기능 추가


#### 기능 개선/변경

* [Console]
	* 각 메뉴에서 사용자 국가기준으로 시간 노출
	* 판매현황 소수점 이하 가격처리
	* 동접변화량 알람 발송 다국어 지원(영어/한국어 선택 가능)

### 2017. 08. 24.

#### 기능 개선/변경

* [SDK] 1.1.6 업데이트
	* Push API 추가(for iOS) : SetSandboxMode

### 2017. 07. 20.

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
	* 시스템 팝업 API 추가 (showAlertWithTitle)
	* 국가코드를 대문자로 반환하도록 변경 (Android)
	* TCPush SDK 1.4.1 로 업데이트
	* IAP SDK 1.3.3.20170627 로 업데이트
* [Console]
	* 외부 연동 모듈에서 오류 발생시, 추적을 위한 TrackingTime 추가 노출

### 2017. 05. 25.

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
	* 런타임 중 결제 Store를 변경할 수 있는 API 제공
	* (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공
* [Console]
	* 다국어 지원
	* 모든 메뉴의 Create, Update, Modify 수행시에 Audit log 기능 추가

### 2017. 04. 20.

#### 기능 개선/변경

* [SDK] 1.1.3 업데이트
	* (Android)런칭 구조 및 팝업/점검 페이지 개선 :커스텀 점검 페이지 설정 기능 추가
	* (Android)인증 구조 개선 및 로그 추가 : 인증 Adapter 및 SDK 버전 로그 출력

#### 버그 수정
* [SDK] 1.1.3 업데이트
	* (Android)Facebook SDK v4.19.0 이상에서 초기화시 크래시 오류 수정


### 2017. 04. 04.

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임런칭시 점검, 긴급공지 팝업 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리
* [API] [IAP](./api-guide/#purchaseiap) API 연동 : 아이템 조회, 미소비내역 조회
* [API] checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가
* [Console] 점검, 긴급공지 : 클라이언트 버전 선택 시 게임에서 사용하지 않는 스토어는 노출되지 않도록 변경

#### 버그 수정
* [Console] iOS 클라이언트 등록 시 마켓 비정상 노출 수정

### 2017. 03. 21.

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog
* [API] [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API 연동

### 2017. 03. 09.

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
