## Game > Gamebase > Release Notes

### 2017.12.21

#### 기능 추가

* [Console]
	* [Push] 현지시간대 발송(Local Time Push) 기능 추가
	* [Operating indicator]>[판매 현황] 마켓별 매출 차트 추가
	* [Operating indicator]>[유저 통계] 앱 출시 이후의 사용자 지표 추이 확인하는 메뉴 추가
	* [Operation]>[점검] 점검상태에서 사용자에게 보여주는 점검페이지 등록 방법 추가
		* 기존 : Gamebase 자체 제공 페이지, 외부 페이지 URL 입력
		* 추가 : (1)html 직접 입력 (2)Console에서 입력한 점검내용을 외부페이지에 전달하는 기능 추가

* [SDK] 1.5.0
	* WebView가 닫힐 때 발생하는 Close Callback 추가
	* WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
	* Unity Setting Tool 신규 배포

#### 기능 개선/변경
* [Console]
	* [App]>[클라이언트] 클라이언트 상태 변경시 이전에 게임에서 등록한 사용자 노출메시지 정보를 재사용할 수 있도록 수정	

#### 버그 수정
* [SDK] 1.5.0
	* (Unity) UnityEditor에서 Guest로그인이 되지 않는 현상 수정
	* (Unity) TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가


### 2017.11.30

#### 기능 추가

* [Console]
	* [Management]>[알람] : Webhook 알람 등록하는 기능 추가
	* [Operating indicator]>[모니터링]: Push 발송 이력 조회 추가

#### 기능 개선/변경
* [Console]
	* [Operating indicator]>[모니터링]: 차트 색상 변경, Timezone 이슈. DAU 계산로직 변경(Login시간기준->접속시간기준)
* [API] [점검 조회 API](http://docs.cloud.toast.com/ko/Game/Gamebase/ko/Server%20Developer%60s%20Guide/#check-under-maintenance) 결과를 List 에서 단일 객체로 변경

#### 버그 수정
* [Console]
	* [Push] : Push 등록 시 기본언어가 선택되어 있지 않아도 등록되는 오류 수정


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
* [API] [IAP](/Upcoming%20Products/Gamebase/ko/Server%20Developer%60s%20Guide/#purchaseiap) API 연동 : 아이템 조회, 미소비내역 조회
* [API] checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가
* [Console] 점검, 긴급공지 : 클라이언트 버전 선택 시 게임에서 사용하지 않는 스토어는 노출되지 않도록 변경

#### 버그 수정
* [Console] iOS 클라이언트 등록 시 마켓 비정상 노출 수정

### 2017.03.21

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](/Upcoming%20Products/Gamebase/ko/aos-ui/#ui) : Custom Webview, AlertDialog
* [API] [Leaderboard](/Upcoming%20Products/Gamebase/ko/Server%20Developer%60s%20Guide/#leaderboard), [IAP](/Upcoming%20Products/Gamebase/ko/Server%20Developer%60s%20Guide/#purchaseiap) API 연동

### 2017.03.09

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
