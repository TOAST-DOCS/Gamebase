## Game > Gamebase > 릴리스 노트 > iOS

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 현재 약관 창이 화면에 표시되고 있는지 여부를 알 수 있는 API를 추가하였습니다.
    * **[TCGBTerms isShowingTermsView]**

#### 기능 개선/변경
* 기존의 Google 웹 로그인 방식에서 Google SDK 로그인 방식으로 변경하였습니다.
* 한게임 로그인을 도중에 취소했을 경우, **TCGB_ERROR_AUTH_USER_CANCELED(3001)** 에러를 리턴하도록 수정하였습니다.

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

#### 기능 추가
* Swift 프로젝트 사용자를 위해서 Public API에 NS_SWIFT_NAME 설정을 추가하였습니다.

#### 기능 개선/변경
* 외부 SDK 업데이트: Hangame iOS SDK (1.6.2)
* 디바이스가 가로 모드인 상태에서 showWebView API를 호출했을 때, 하단에 검은색 빈 공간이 출력되는 오류를 수정하였습니다.

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* Gamebase 콘솔의 업데이트 필수 설정에서 **팝업 버튼 추가**를 선택하면 클라이언트의 업데이트 필수 팝업 창에 **자세히 보기** 버튼이 추가됩니다.
* 단말기에서 알림을 허용했는지 여부를 알 수 있는 API가 추가되었습니다.
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* 공통 약관 API 호출 후 약관 UI가 표시되었는지 여부를 알 수 있는 VO 클래스가 추가되었습니다.
    * **TCGBShowTermsViewResult**

#### 기능 개선/변경
* 이미지 공지 API를 호출했을 때 표시할 이미지 공지가 없는 경우, 배경이 잠시 어두워지는 현상을 수정하였습니다.
* 킥아웃 팝업 창 표시 여부는 Gamebase 콘솔에서 킥아웃 등록 시 설정할 수 있으므로 다음 필드가 deprecated되었습니다.
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**

### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 공통 약관 창의 설정을 변경할 수 있는 신규 API가 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > UI > Terms > showTermsView](./ios-ui/#showtermsview)

#### 기능 개선/변경
* 외부 SDK 업데이트 : PAYCO iOS SDK (1.5.5)
* 오류 코드 추가 및 변경
    * TCGB_ERROR_UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 TCGB_ERROR_SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.

### 2.32.1 (2022.01.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 업데이트 권장 팝업 창에서 **지금 업데이트** 버튼을 클릭하면 팝업 창이 종료되지 않도록 수정하였습니다.
* SDK 안정성을 개선하였습니다.

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* GamebaseEventHandler의 GamebaseEventCategory에 **kTCGBServerPushAppKickoutMessageReceived** 타입이 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* GamebaseEventHandler의 GamebaseEventCategory에 **kTCGBLoggedOut** 타입이 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

#### 기능 개선/변경
* 웹뷰 navigationBar의 기본 타이틀 색상이 **UIColor.whiteColor**로 변경되었습니다.

#### 버그 수정
* Hangame 로그아웃 호출 시, thirdIdP도 로그아웃되도록 수정하였습니다.

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 점검 팝업 창에서 점검 시간 표시 여부를 동적으로 설정할 수 있게 되었습니다.

#### 기능 개선/변경
* 외부 SDK 업데이트 : TOAST iOS SDK (0.29.2), PAYCO iOS SDK (1.5.4)
* 이용정지 웹뷰 내의 고객센터 링크에서 이용정지 유저 정보로 문의를 등록할 수 없는 문제를 해결하였습니다.
* 점검 팝업 창, 이용정지 자세히보기 웹뷰에서 뒤로가기 버튼이 표시되도록 수정하였습니다.

### 2.30.1 (2021.11.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

#### 버그 수정
* Unity 2019.3 이상에서 Cocoapods 설치를 했을 때, 결제와 푸시 API에서 에러가 발생하는 버그를 수정하였습니다.

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 강제 매핑 시 IdP 로그인을 한 번 더 시도해야 하는 불편함을 개선한 새로운 강제 매핑 API가 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 인증 > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* 특정 IdP로 매핑 시도 후 **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러가 발생했을 때, 해당 IdP로 계정을 변경할 수 있는 API가 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 인증 > Change Login with ForcingMappingTicket](./ios-authentication/#change-login-with-forcingmappingticket)

#### 버그 수정
* loginForLastLoggedInProvider 로그인 이후, 특정 IdP에서 로그아웃 또는 탈퇴 기능이 동작하지 않는 버그를 수정하였습니다.

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Xcode 최소 지원 버전이 12에서 13으로 변경되었습니다. 
* 외부 SDK 업데이트: TOAST iOS SDK(0.29.1), ToastGamebaseIAP SDK(0.12.1)
* 콘솔에 등록한 점검 및 공지 자세히 보기의 URL을 인코딩하지 않고 화면에 출력하도록 변경되었습니다.

#### 버그 수정
* TCGBPushMessage.extras를 json 파싱할 때 에러가 발생하는 버그를 수정하였습니다.

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* Kakaogame 인증 추가
* '결제 어뷰징 자동 해제' 기능이 추가되었습니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 인증 > GraceBan](./ios-authentication/#graceban)
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 '이용 정지 유예 상태' 후 이용 정지가 되도록 합니다.
    * '이용 정지 유예 상태'일 경우, 설정한 기간 내에 이용 정지 해제 조건을 모두 만족하면 정상적으로 플레이할 수 있습니다.
    * 기간 내에 조건을 충족하지 못하면 이용 정지가 됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 TCGBAuthToken.tcgbMember.graceBanInfo 값을 확인하고, null이 아닌 유효한 TCGBGraceBanInfo 객체를 리턴한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리해야 합니다.

#### 기능 개선/변경
* PAYCO iOS SDK 업데이트 (1.5.2)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* PAYCO iOS SDK 업데이트 (1.5.1)
    * 인증 플로우 및 UI 개선
* Hangame iOS SDK 업데이트 (1.6.1)
    * 본인인증에서 에러 상황 발생 시 콜백 호출 안되는 이슈 수정
    * iOS 15 beta에서 내비게이션바가 깨지는 이슈 수정

#### 버그 수정
* 이미 약관에 동의하여 약관 UI가 표시되지 않을 경우, PushConfiguration가 nil로 리턴되지 않는 이슈를 수정했습니다.

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* PAYCO iOS SDK 업데이트 (1.5.0)
    * PAYCO 앱이 없을 때 이전에는 수동 로그인만 가능했으나, Safari에 로그인돼 있다면 간편 로그인 기능을 사용할 수 있도록 변경되었습니다.

#### 버그 수정
* Unity에서 이미지 공지가 출력되지 않는 이슈를 수정했습니다.
    * Gamebase iOS SDK 2.27.0 미만을 사용할 경우 Unity에서 이미지 공지가 출력되지 않을 수 있습니다.
    * 이미지 공지를 사용할 경우에는 Gamebase iOS SDK 2.27.0 이상을 사용하시기 바랍니다.


### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Display Language 기능이 개선되었습니다.
    * 지금까지는 언어셋을 추가하기 위해 Gamebase.bundle 안에 있는 파일을 직접 수정해야 했습니다.
        * 이제 Xcode 프로젝트의 Copy Bundle Resources에 localizedstring.json 파일을 추가하면 되도록 개선하였습니다.
    * Display Language 언어셋에 중국어 간체(zh-CN), 중국어 번체(zh-TW), 태국어(th)가 추가되었습니다.
    * 기본 언어코드가 **en** 이었으나, Gamebase 콘솔에서 설정한 기본언어가 반영되도록 개선하였습니다.
        * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > App > 언어 설정](https://docs.toast.com/en/Game/Gamebase/en/oper-app/#language-settings)
* showTermsView API 호출 후 생성할 수 있는 PushConfiguration 객체의 생성 기준이 다음과 같이 변경되었습니다.
    * 변경 전
        * 약관 항목 중에 **Push 수신** 항목이 존재하는 경우에만 nil이 아닌 유효한 PushConfiguration이 리턴되었습니다.
        * 유저가 주간, 야간 홍보성 Push 수신에 모두 거부한 경우 PushConfiguration.pushEnabled는 false로 생성되었습니다.
    * 변경 후
        * 약관 UI가 표시되었다면 항상 nil이 아닌 유효한 PushConfiguration이 리턴됩니다.
        * showTermsView가 리턴하는 PushConfiguration 객체의 pushEnabled 값은 항상 true입니다.
    * 변경되지 않고 동일한 점
        * 이미 약관에 동의하여 약관 UI가 표시되지 않았다면 PushConfiguration은 nil로 리턴됩니다.

#### 버그 수정
* Push 언어 설정은 별다른 보조 처리가 없이 단말기의 언어코드를 그대로 적용되어, Push 콘솔에서 전송한 메시지의 언어코드가 일치하지 않는 문제를 수정하였습니다.

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 월 결제 한도 기능 추가
    * 월 결제 한도를 넘는 경우 **PURCHASE_LIMIT_EXCEEDED(4007)** 에러가 발생합니다.

#### 기능 개선/변경
* Push 항목이 존재하는 약관에서 PushConfiguration 객체 보장
    * 약관 UI 에서 Push 수신 동의를 하지 않을 경우 Gamebase.Terms.showTermsView API 호출 결과로 생성되는 TCGBPushConfiguration 이 null이었으나, 약관에 Push 항목이 존재한다면 TCGBPushConfiguration 객체가 항상 리턴되도록 변경되었습니다.
    * Push 수신 거부 시 TCGBPushConfiguration 객체는 (푸시 동의 여부 = false, 광고성 푸시 동의 여부 = false, 야간 광고성 푸시 동의 여부 = false)로 생성됩니다.
    * 약관에 Push 항목이 존재하지 않는다면 TCGBPushConfiguration 객체는 null입니다.
* 외부 SDK 업데이트: TOAST iOS SDK(0.29.0)
* Sign In with Apple 의 ASAuthorizationErrorUnknown 에러가 발생했을 경우, TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR 에러를 리턴하도록 변경

#### 버그 수정
* registerPush 를 통해 등록한 TCGBPushConfiguration 값과 TCGBPushTokenInfo 값이 달라지게 되는 버그 수정

### 2.24.0 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 내부 론칭 URL 변경

#### 버그 수정
* 약관 상세 보기 후 약관 새 창이 닫히지 않는 버그 수정

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST iOS SDK(0.28.0), ToastGamebaseIAP SDK(0.12.0)

### 2.22.0 (2021.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST iOS SDK(0.27.2), Hangame iOS SDK(1.6.0)

### 2.21.2 (2021.04.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Facebook iOS SDK 업데이트 (9.2.0)

#### 버그 수정
* 아카이브 빌드 시 bitcode 관련 오류가 발생하는 이슈 수정

### 2.21.1 (2021.04.19)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-iOS.zip)

#### 버그 수정
* bitcode를 지원 가능하도록 설정해도 설정값이 반영되지 않는 문제 수정

### 2.21.0 (2021.04.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* Hangame 일본 인증 추가 

#### 기능 개선/변경
* bitcode 지원이 가능하도록 변경
* showWebView 호출 시, 닫기 버튼을 가장 먼저 화면에 표시되도록 수정

### 2.20.2 (2021.03.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Facebook iOS SDK 업데이트 (9.1.0)
* 특정 경우에 GamebaseAuthFacebookAdapter에서 openURL delegate가 호출되지 않았던 이슈 수정

### 2.20.1 (2021.03.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* iOS 14에 대응하여 IDFA 획득 로직 수정: info.plist에 NSUserTrackingUsageDescription 필드 추가

### 2.20.0 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-iOS.zip)

* 공통 약관 기능 추가
    * 약관 WebView를 여는 API 추가
    * 약관 리스트 및 유저별 동의 여부를 조회하는 API 추가
    * 유저별 약관 동의 여부를 Gamebase 서버에 저장하는 API 추가

#### 기능 개선/변경
* 고객센터 타입이 TOAST 조직 상품(Online Contact)인 경우 로그인을 하지 않아도 고객센터가 표시되도록 변경

### 2.19.1 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Weibo IdPAdapter 구조 변경

### 2021. 01. 12.

```
Gamebase의 XCode 최소 지원 버전이 10에서 11로 변경되었습니다. 
```
    
### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Weibo 인증 추가
    
#### 기능 개선/변경

* 론칭 상태 코드 추가: 베타 서비스(205)

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

#### 기능 추가

* 개발사 자체 고객센터 오픈 시 additionalURL 필드 추가
* 결제 아이템 정보에 지역화된 상품 정보 추가: localizedTitle, localizedDescription

#### 기능 개선/변경

* 외부 SDK 업데이트: TOAST iOS SDK(0.27.1)
* showWebView: 잘못된 URL을 전달했을 경우 에러 반환, 전달받은 URL은 인코딩하지 않고 그대로 사용
* 대소문자 상관없이 커스텀 스킴이 동작하도록 변경


### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* iOS 13 이상부터 제공되는 SceneDelegate 대응 API 추가

### 2.17.1 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 특정 지표 전송 시 오류 메시지를 추가하여 전송: 푸시 등록에 실패 시, 게임 지표 전송 시

### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)

```
한게임 인증 사용을 원하는 경우 고객센터로 미리 연락주세요.
```

#### 기능 추가

* Hangame IdP 인증 추가

#### 기능 개선/변경

* 고객 센터 첨부 이미지 클릭 시 다운로드 지원
* TCGBMember.regDate, TCGBMember.lastLoginDate의 타입을 long long으로 변경
* 웹뷰에서 URL 및 타이틀 변경 시 타이틀을 재출력할 수 있도록 로직 변경

#### 버그 수정

* PAYCO 인증: lastLoggedInProvider 로그인 후 로그아웃 호출 시 로그아웃 콜백이 오지 않는 문제 수정
    
### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 고객센터 기능 추가
    * API 추가(Gamebase.Contact.requestContactURL): 고객 센터 URL 리턴
    * 고객 센터 API 에 userName 을 설정할 수 있도록 ContactConfiguration 파라미터 추가 
        
### 2.15.1 (2020.09.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 외부 SDK 업데이트: TOAST iOS SDK(0.27.0)
* iOS 14 beta 변경 사항을 대응한 IAP SDK 신규 버전이 적용되었습니다. [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 푸시 토큰 등록시 NotificationOption 설정으로 앱이 포그라운드(foreground) 상태에서도 푸시 알림을 받을 수 있도록 기능 추가
* 푸시 API 추가: Push 토큰 정보 확인(Gamebase.Push.queryTokenInfo API)

#### 기능 개선/변경

* 외부 SDK 업데이트: TOAST iOS SDK(0.26.0)
* 결제 payload의 null check 로직 추가
    
### 2.14.0 (2020.08.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* PAYCO IdP의 상수값 제거: PAYCO 문자열로 인한 애플 검수가 리젝되는 경우가 발생하여 제거
* TCGBWebViewConfiguration에 contentMode 설정 추가

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Sign In With Apple 인증: iOS 12 이하 지원
    
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 이미지 공지: 표시 기간과 우선순위에 따라 게임 내 이미지 팝업 창 표시
    * 이미지 공지 표시 API 추가

#### 기능 개선/변경
* Facebook SDK 업데이트(7.1.1)
* configuartion에 설정된 storeCode(default=AS)로 Gamebase 초기화 시도
* 콘텐츠를 로딩할 수 없는 웹뷰 출력 시 **닫기** 버튼이 없어 닫을 수 없는 문제 수정
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 결제 API 추가: 상품 ID로 결제 요청, 추가 정보(UserPayload) 입력해 결제 완료 시 확인할 수 있음

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 사용자 푸시 설정 초기화 시 언어 코드가 설정되어 있지 않으면 디바이스 언어로 설정되도록 변경

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 기존의 모든 이벤트 시스템을 통합하는 GamebaseEventHandler 추가
    * ServerPush, Observer 기능을 포함하고 있고, 프로모션 결제 이벤트 및 푸시 이벤트도 확인 가능


### 2.9.1 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)

#### 버그 수정

* Unreal 엔진에서 빌드 하면, warning을 빌드 오류로 판정해서 빌드가 안되는 부분을 수정
        
### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 탈퇴 유예 기능
    * API 추가: 탈퇴 유예 신청, 탈퇴 유예 신청 취소, 탈퇴 유예 상태에서 즉시 탈퇴, 유저의 탈퇴 유예 여부 확인
        
#### 기능 개선/변경

* 외부 SDK 업데이트: TOAST iOS SDK(0.24.0)
* PAYCO iOS SDK 업데이트 (1.4.0)

### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Analytics 전송 결과 확인을 위한 내부 지표 추가
    
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 결제 및 상품 정보에 상품 타입 및 지역 가격 등의 정보를 추가

#### 기능 개선/변경

* 콘솔에 등록되지 않은 앱 버전으로 초기화 실패할 때 스토어로 이동할 수 있는 팝업 창이 추가로 노출하도록 개선

### 2.7.1 (2020.02.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Guest로 Login 후 GetAuthProviderUserID 호출하면 값을 반환하도록 수정

### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 외부 SDK 업데이트: TOAST iOS SDK(0.20.1)
* NAVER iOS SDK 업데이트 (4.1.0)
    
### 2.6.1 (2019.12.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)
    
#### 버그 수정
* AddMapping(강제, Forcibly) 사용 시, 매핑이 되지 않는 문제 수정
* Unity Plugin으로 PushConfiguration의 displayLanguageCode를 설정하지 않을 경우, NSNull 객체에 의하여 크래시가 발생하는 문제를 수정
    

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 데이터를 Log&Crash 에 전송하여 각종 분석에 이용할 수 있도록 TOAST Logger 추가
* Sign In with Apple 인증 추가

### 2.5.2 (2019.10.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* UIWebView를 WKWebView로 교체

### 2.5.1 (2019.09.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)
    
#### 기능 개선/변경

* GamebasePushAdapter 에서 사용중인 TCPushSDK 를 1.7.0으로 업데이트
    * TCPushSDK가 Static Library 에서 Framework 파일로 변경되었으므로 프로젝트에 TCPushSDK.framework 를 추가해야 합니다.
    
### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Console 에서 입력한 CS URL 을 웹뷰로 오픈하는 API 제공
    
### 2.4.3 (2019.07.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

#### 버그 수정

* 인증 시도 시 오류가 발생했을 경우, 형식에 맞지 않는 오류 메시지 파싱 시도에 따른 크래시 발생 이슈 수정

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* LaunchingInfo에 JSON string 형식의 TOAST Launching 정보를 추가
* LINE iOS SDK 업데이트 (5.0.1)
    * LINE Adpater의 최소 지원 OS 버전이 iOS 10으로 변경 
    * LINE 앱을 통한 로그인 기능 추가

#### 버그수정

* Analytics 버그 수정: 로그아웃, 탈퇴, 계정 이전 시 저장된 지표 데이터를 초기화 하도록 수정
* 네트워크 연결 문제로 간헐적으로 크래시가 발생하던 현상 수정

### 2.4.1 (2019.06.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

#### 버그수정

* Analytics 지표 전송 시 일부 파라미터가 누락 되어 지표가 제대로 출력되지 않는 버그 수정

### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 지표관련 Class 변경
    * LevelUpData Class: userLevel, levelUpTime 파라미터가 필수로 변경 / 그 외 필드 삭제 [자세히보기 [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings)]
    * GameUserData Class: classId(게임유저의 직업) 필드 추가 [자세히보기 [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace)]

    
### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Launching Status Code 추가: "심사중(204)", "테스트중(203)"

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

#### 버그수정

* showBlockingPopup을 NO로 설정 할 경우 Gamebase 초기화 콜백이 호출되지 않는 이슈를 수정

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    * 추가된 API 
        * TransferAccountInfo 발급 API (issueTransferAccount)
        * 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
        * 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
        * 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)        
* 강제 매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
    * 추가된 API 
        * 강제 매핑하는 API (addMappingForcibly)

#### 기능 개선/변경

* LINE iOS SDK의 App 로그인 기능이 비활성화
    * LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adatper에서 Web 로그인만 지원하도록 변경

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* TransferKey API 삭제
    * issueTransferKey : TransferKey 발급
    * requestTransfer : TransferKey 검증
        
#### 버그수정

* Gamecenter를 Gamebase가 아닌 다른 로직에의해 로그인 한 후, Gamebase를 통하여 Gamecenter로그인을 시도할 때, 반응이 없는 버그 수정

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가

* Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK 내부에서 자동 전송)
    * setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
    * traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출

#### 기능 개선/변경

* IAP SDK 업데이트
    * 결제 실패 시 간헐적으로 크래시가 발생하던 현상 수정

#### 버그수정

* iOS 12 이상의 시뮬레이터에서 debugMode On 상태로 Gamebase 초기화 시 크래시가 발생하던 현상 수정

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 crash가 발생될 수 있는 구조 수정

### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Gamebase Webview에서 파일첨부 기능 추가
    
#### 기능 개선/변경
* 이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
* PAYCO iOS SDK 업데이트 (1.2.4)
* Remove API : Webview, Network, Launching
    * [TCGBUtil showToastWithMessage:duration:]
    * [TCGBWebView showWebBrowserWithURL:viewController:]
    * [TCGBWebView showWebViewWithURL:viewController:configuration:]
    * [TCGBLaunching addObserverOnChangedStatusNotification:]
    * [TCGBLaunching removeObserverOnChangedStatusNotification:]
    * [TCGBLaunching addUpdateStatusNotification]
    * [TCGBLaunching removeUpdateStatusNotification]
    * [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
    * [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
* Deprecated API 
    * [TCGBGamebase languageCode]

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* App Store Promotion IAP를 지원하기 위한 API 추가

#### 기능 개선/변경

* IAP SDK 최신버전 적용 (iOS:1.6.0)
* authProviderProfileWithIDPCode api의 호출 결과의 구조가 1depth로 변경 (Android, Unity와 통일)
        
### 1.12.2 (2018.08.28) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Google Auth Adapter, Naver Auth Adapter의 Callback URL Scheme 설정 개선
    * 콘솔에 "url_scheme_ios_only" 값을 설정하지 않으면 Default URL Scheme을 설정 하도록 개선 : Default URL Scheme을 사용하기 위해서는 XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.google 또는 tcgb.{Bundle ID}.naver 등록 필요
* Payco Auth Adapter 개선
    * URL Scheme 미설정으로 인해 의도치 않은 URL Scheme을 호출하던 문제 수정 : 설정 방법이 변경되어 업데이트를 위해서는 반드시 URL Scheme 설정 필요 (XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.payco를 등록)

### 1.12.1 (2018.08.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* IAP SDK 최신버전 적용 (1.5.0)
* Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
* 점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
* IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
* 인증 API 중복 호출시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
* 오류 코드 추가 : Gamecenter 로그인 거부(TCGB_ERROR_IOS_GAMECENTER_DENIED)
    
#### 버그수정

* Naver 로그인 시 프로필 정보 조회 실패로 인해 로그인이 불가능한 버그 수정 : 프로필 정보 조회 실패하더라도 로그인은 성공하도록 변경    
    
### 1.12.0 (2018.07.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Gamebase 초기화 시 Debug Log에 사용중인 Adapter들의 버전 정보, 앱의 빌드 정보를 출력하는 기능이 추가 
* CocoaPods을 통해 배포 되는 Naver Auth Adapter에서 포함하고 있던 Naver ID Login SDK의 바이너리가 제거 되고 의존성 설정 방식으로 변경

### 1.11.1 (2018.07.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)

#### 기능 추가

* Line IdP 추가

#### 기능 개선/변경

* Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경
    
#### 버그수정

* 점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정

### 1.11.0 (2018.06.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* Google IdP 추가
* Twitter IdP 추가
    
#### 기능 개선/변경
* LocalizedString 일본어 번역 추가
* 인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
* Naver ID Login SDK 업데이트 : iOS(4.0.10)
    
### 1.9.1 (2018.05.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)

#### 버그수정

* Gamebase WebView NavigationBar 영역에 타이틀, 뒤로가기, 닫기 버튼이 나타나지 않는 현상을 수정
    
### 1.9.0 (2018.05.03) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * 추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)

#### 버그 수정

* Naver계정을 이용한 로그인 중 App to Web 로그인 시도 시, 서버로부터 받아온 Scheme의 형식이 변경되어, 로그인이 되지 않는 현상 수정
* Adapter로부터 UnderlyingError 객체를 받아서 유저에게 전달되는 에러객체를 생성하는 로직에서 메시지 및 Underlying Error의 설정이 되지 않는 버그 수정

### 1.8.1 (2018.04.12) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)

#### 버그 수정

* registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 1.8.0 (2018.04.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    * 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    * 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    * 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경

* Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
* PAYCO 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
* iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정

#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정

### 1.7.0 (2018.02.22) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Naver IdP 인증 추가
* Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 1.6.0 (2018.01.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)

#### 버그 수정

* WebView 호출시, 크래시가 일어날 수 있는 부분에 대한 방어로직 처리

### 1.5.0 (2017.12.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* WebView가 닫힐 때 발생하는 Close Callback 추가
* WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
* Unity Setting Tool 신규 배포

### 1.4.0 (2017.11.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* close/back 버튼 리소스가 없을 때, "x", "<" 등의 텍스트로 노출되던 이슈를 디폴트 값으로 대체

#### 버그 수정

* WebView 런치 후, 기기 회전시 NavigationBar Title 이 reset이 되는 오류 수정
* WebView의 NavigationBar Height을 커스터마이징 할 때, NavigationBar 배경 부분이 겹쳐서 노출되는 오류 수정

### 1.3.0 (2017.10.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* Credential을 이용한 AddMapping API추가

### 1.2.0 (2017.09.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)

#### 기능 추가

* 이용정지(사용자처벌) 기능 추가
* 이용정지 사용자 팝업 창 노출

### 1.1.5 (2017.07.20) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* 시스템 팝업 창 API 추가 (showAlertWithTitle)
* 국가코드를 대문자로 반환하도록 변경 (Android)
* TCPush SDK 1.4.1 로 업데이트
* IAP SDK 1.3.3.20170627 로 업데이트

### 1.1.4 (2017.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* 런타임 중 결제 Store를 변경할 수 있는 API 제공

### 1.1.2 (2017.04.04) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 게임론칭시 점검, 긴급공지 팝업 창 개선

### 1.1.0 (2017.03.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경

* 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-iOS.zip)

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
    * 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
    * 로그아웃 및 회원탈퇴 기능을 제공
    * 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
    * 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
    * 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
    * TOAST Cloud상품 연동 : PUSH, IAP
