## Game > Gamebase > 릴리스 노트

### 2018.05.03

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
	
### 2018.04.12

#### 버그 수정
* [SDK] 1.8.1
	* (Android. iOS) registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 2018.04.09

#### 버그 수정
* [SDK] 1.8.1
	* (Unity) UnityAndroid 플랫폼에서 아래 기능 사용 시 모듈 초기화가 되지 않아 NullReferenceException이 발생하여 수정
		* Launching, Purchase, Push, Util, Webview

### 2018.04.05

#### 기능 추가
* Kick out 기능 추가
    - 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    - (console) 메뉴 추가
    - (SDK 공통) kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    - 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    - 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    - (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
	* (SDK 공통) Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
	* (iOS) 페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
	* (iOS) iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정

#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정
* [SDK] 1.8.0
	* (Setting Tool) Unity Facebook Adapter를 체크하면 에러가 나는 버그 수정

### 2018.03.13

#### 버그 수정
* [SDK] 1.7.1
	* (Unity) Inspector에서 설정된 SetDebugMode 값이 반영 안 되던 버그 수정
	* (Unity) Standalone, WebGL: Display Language에서 사용되는 리소스 파일 누락 부분 수정
	* (Unity) Google Adapter 1.6.2 배포: Google Adapter 1.6.1에서 AuthCode가 Empty로 반환되어 인증 실패하는 버그 수정

### 2018.02.22

#### 기능 추가
* [SDK] 1.7.0
	* Naver IdP 인증 추가
	* Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 2018.01.25

#### 기능 추가

* [Console]
	* [Push] PUSH 입력값 복사기능 추가
	* [Operating indicator>그룹 동접] 일간 그룹 동접 변화 그래프 추가

* [SDK] 1.6.0
	* (Unity) Standalone WinSDK 추가
		* 64비트 지원
		* 인증 지원 : facebook, google, payco

#### 기능 개선/변경
* [Console]
	* [Operating indicator>모니터링] 프로젝트 생성 이전 설정된 시스템 점검 항목이 노출 되는 문제 수정
	* [App>앱] 테스트 단말기 등록화면 개선 - User ID 로그인이력을 바탕으로 손쉽게 단말기 등록가능하도록 개선
	* [Operation>점검] 점검 미리보기 화면 개선

#### 버그 수정
* [SDK] 1.6.0
	* (iOS) WebView 호출시, 크래시가 일어날 수 있는 부분에 대한 방어로직 처리


### 2017.12.21

#### 기능 추가

* [Console]
	* [Push] 현지시간대 발송(Local Time Push) 기능 추가
	* [Operating indicator>판매 현황] 마켓별 매출 차트 추가
	* [Operating indicator>유저 통계] 앱 출시 이후의 사용자 지표 추이 확인하는 메뉴 추가
	* [Operation>점검] 점검상태에서 사용자에게 보여주는 점검페이지 등록 방법 추가
		* 기존 : Gamebase 자체 제공 페이지, 외부 페이지 URL 입력
		* 추가 : Console에서 입력한 점검내용을 외부페이지에 전달하는 기능 추가

* [SDK] 1.5.0
	* WebView가 닫힐 때 발생하는 Close Callback 추가
	* WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
	* Unity Setting Tool 신규 배포

#### 기능 개선/변경
* [Console]
	* [App>클라이언트] 클라이언트 상태 변경시 이전에 게임에서 등록한 사용자 노출메시지 정보를 재사용할 수 있도록 수정	

#### 버그 수정
* [SDK] 1.5.0
	* (Unity) UnityEditor에서 Guest로그인이 되지 않는 현상 수정
	* (Unity) TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가


### 2017.11.30

#### 기능 추가

* [Console]
	* [Management>알람] Webhook 알람 등록하는 기능 추가
	* [Operating indicator>모니터링] Push 발송 이력 조회 추가

#### 기능 개선/변경
* [Console]
	* [Operating indicator>모니터링] 차트 색상 변경, Timezone 이슈. DAU 계산로직 변경(Login시간기준->접속시간기준)
* [API] [점검 조회 API](./api-guide/#check-under-maintenance) 결과를 List 에서 단일 객체로 변경

#### 버그 수정
* [Console]
	* [Push] Push 등록 시 기본언어가 선택되어 있지 않아도 등록되는 오류 수정


### 2017.11.23

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

### 2017.10.26

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
	
### 2017.09.21

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

### 2017.08.24

#### 기능 개선/변경

* [SDK] 1.1.6 업데이트
	* Push API 추가(for iOS) : SetSandboxMode

### 2017.07.20

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
	* 시스템 팝업 API 추가 (showAlertWithTitle)
	* 국가코드를 대문자로 반환하도록 변경 (Android)
	* TCPush SDK 1.4.1 로 업데이트
	* IAP SDK 1.3.3.20170627 로 업데이트
* [Console]
	* 외부 연동 모듈에서 오류 발생시, 추적을 위한 TrackingTime 추가 노출

### 2017.05.25

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
	* 런타임 중 결제 Store를 변경할 수 있는 API 제공
	* (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공
* [Console]
	* 다국어 지원
	* 모든 메뉴의 Create, Update, Modify 수행시에 Audit log 기능 추가

### 2017.04.20

#### 기능 개선/변경

* [SDK] 1.1.3 업데이트
	* (Android)런칭 구조 및 팝업/점검 페이지 개선 :커스텀 점검 페이지 설정 기능 추가
	* (Android)인증 구조 개선 및 로그 추가 : 인증 Adapter 및 SDK 버전 로그 출력

#### 버그 수정
* [SDK] 1.1.3 업데이트
	* (Android)Facebook SDK v4.19.0 이상에서 초기화시 크래시 오류 수정


### 2017.04.04

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임런칭시 점검, 긴급공지 팝업 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리
* [API] [IAP](./api-guide/#purchaseiap) API 연동 : 아이템 조회, 미소비내역 조회
* [API] checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가
* [Console] 점검, 긴급공지 : 클라이언트 버전 선택 시 게임에서 사용하지 않는 스토어는 노출되지 않도록 변경

#### 버그 수정
* [Console] iOS 클라이언트 등록 시 마켓 비정상 노출 수정

### 2017.03.21

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog
* [API] [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API 연동

### 2017.03.09

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
