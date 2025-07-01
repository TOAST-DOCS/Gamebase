## Game > Gamebase > 릴리스 노트 > Unity

### 2.72.0 (2025. 06. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* (Windows) 업데이트 팝업에 자세히 보기 버튼을 추가하였습니다.
* (Windows) 이용 정지 팝업에 고객 센터 링크를 추가하였습니다.

#### 기능 개선/변경

* (Windows) 내부 로직을 개선하였습니다.

#### 버그 수정

* (Windows) 점검 상태로 갱신되지 않던 문제를 수정하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.72.0](./release-notes-android/#2720-2025-06-24)
* [Gamebase iOS SDK 2.72.0](./release-notes-ios/#2720-2025-06-24)

### 2.71.1 (2025. 06. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Unity.zip)

#### 버그 수정

* (macOS) GamebaseUtil의 DllNotFoundException 문제를 수정했습니다.

### 2.71.0 (2025. 04. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* '게임 공지' 신규 기능이 추가되었습니다.
    * Gamebase.GameNotice.OpenGameNotice(GamebaseCallback.ErrorDelegate callback)
    * API 호출 방법은 다음 가이드 문서를 참고하시기 바랍니다.
        * [Game > Gamebase > Unity SDK 사용 가이드 > 공지 > 게임 공지](./unity-ui/#gamenotice)

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.
* (iOS) ViewController 설정 로직을 개선하여 초기화 실패 오류를 방지합니다.

#### 버그 수정

* macOS 15.4에서 크래시가 발생하는 이슈를 수정하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.71.0](./release-notes-android/#2710-2025-04-15)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2710-2025-04-15)

### 2.70.1 (2025. 03. 13.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.1/GamebaseSDK-Unity.zip)

#### 버그 수정

* (Android) ShowWebView, ShowTermsView 호출 시 Configuration이 없으면 크래시가 발생하는 문제를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.70.1](./release-notes-android/#2701-2025-03-13)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2700-2025-03-11)

### 2.70.0 (2025. 03. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* (Android) 'GPGS 자동 로그인' 기능 연동 시 유저에게 GPGS 로그인을 앱 설치 후 한번만 물어보는 초기화 옵션을 추가했습니다.
  * GamebaseRequest.GamebaseConfiguration enableGPGSSignInCheck
    * 기본 설정은 true로, 유저가 GPGS 로그인을 거부하더라도 Gamebase 초기화 때 GPGS 로그인 창을 다시 표시합니다.
    * false로 설정하면 앱 최초 실행 시에만 GPGS 로그인 창이 한번 표시됩니다.
* GamebaseWebView에 내비게이션 바 타이틀과 아이콘 틴트 컬러 설정 옵션을 추가했습니다.
    * WebView.Configuration navigationTitleColor
    * WebView.Configuration navigationIconTintColor

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.70.0](./release-notes-android/#2700-2025-03-11)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2700-2025-03-11)

#### Setting Tool (v3.0.0)

* 사용 목적에 맞게 사용자 친화적인 UX로 개선했습니다.
* 직관적인 기능 제공으로 설정 및 업데이트가 더 쉬워졌습니다.
* 배포 시 유연하게 업데이트할 수 있도록 개선했습니다.

### 2.69.0 (2025. 1. 21.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* RequestLastLoggedInProvider API 추가
* (Android) WebView Cutout color 기능 추가
* (Windows, macOS) X(Twitter) 로그인 지원

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.
* WebView 색상 설정 관련 코드 개선
    * Configuration 내부에 필드 추가
        * WebView.Configuration navigationColor
        * Community.Configuration backgroundColor
        * ImageNotice.Configuration backgroundColor

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.68.1 (2024. 12. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.

#### 플랫폼별 변경 사항

* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2681-2024-12-10)

### 2.68.0 (2024. 11. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unity.zip)

#### 지원 종료

* FacebookAdapter for Unity 지원이 종료됩니다.

#### 기능 추가

* (Android) GameActivity를 지원합니다.

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.

#### 버그 수정

* NHN Cloud Console에서 네트워크 인사이트 설정을 활성화하면 JSON 파싱 오류가 발생하는 현상이 개선되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2680-2024-11-26)
* [Gamebase iOS SDK 2.68.0](./release-notes-ios/#2680-2024-11-26)

### 2.67.0 (2024. 10. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* (Android, iOS) Steam 인증 추가

#### 기능 개선/변경

* Unity 최소 지원 버전 변경: 2020.3.16f1
* 롤링 이미지 공지의 WebView 내부에서 exception이 발생한 경우, 실패 콜백이 호출되도록 변경되었습니다.
* 내부 로직을 개선하였습니다.

#### 버그 수정

* storeCodeStandalone 코드로 인해 발생하는 오류가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.66.3 (2024. 09. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* Unity 최소 지원 버전 변경: 2020.3.0f1

### 2.66.3 (2024. 09. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

#### 버그 수정
* (iOS) iOS 12 에서 결제 후 크래시가 발생하는 문제를 수정했습니다.

### 2.66.2 (2024. 08. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 아래 필드가 iOS에서 deprecated 되었습니다. Android에서만 사용할 수 있습니다.
    * `GamebaseWebViewConfiguration.orientation`

### 2.66.1 (2024. 07. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unity.zip)

#### 기능 추가

* (macOS) 개인정보 보호 정책이 대응되었습니다.

#### 기능 개선

* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.66.1](./release-notes-android/#2661-2024-07-23)
* [Gamebase iOS SDK 2.66.0](./release-notes-ios/#2660-2024-07-23)

### 2.66.0 (2024. 07. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* (Android) GPGS V2 인증이 추가되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.66.0](./release-notes-android/#2660-2024-07-10)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2651-2024-06-25)

#### Setting Tool (v2.9.0)

* GPGS V2 인증이 추가되었습니다. (Android에 한함)

### 2.65.1 (2024. 06. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 표시할 이미지 공지가 없는 경우 오류 대신 성공 콜백이 호출되도록 변경하였습니다.

#### 버그 수정
* 등록된 이미지 공지가 없을 경우 빈 공지 화면이 노출되는 문제를 수정하였습니다.
* (macOS) UnityEditor에서 GamebaseUtils.bundle 파일이 참조되지 않는 오류를 수정하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.65.1](./release-notes-android/#2651-2024-06-25)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2651-2024-06-25)

### 2.65.0 (2024. 06. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* 이미지 공지 기능에 신규 타입이 추가되었습니다.
    * 롤링 팝업 타입이 추가되었습니다.
    * 기존의 이미지 공지는 팝업 타입으로 표기됩니다.
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.65.0](./release-notes-android/#2650-2024-06-11)
* [Gamebase iOS SDK 2.65.0](./release-notes-ios/#2650-2024-06-11)

### 2.64.0 (2024. 05. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2620-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2620-2024-05-28)

### 2.63.0 (2024. 04. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* (MacOS) WebView 신규 지원

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2620-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2620-2024-04-23)

### 2.62.0 (2024. 03. 26.) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* iOS 개인정보 보호 정책이 대응되었습니다.
    * Gamebase SDK에 Privacy manifest와 서명을 적용했습니다.
* Gamebase 초기화 후 반환되는 LaunchingInfo VO에서 테스트 단말기임을 알 수 있도록 필드가 추가되었습니다.
    * **launchingInfo.user.testDevice**
* (MacOS, Windows) TOAST 타입 고객센터에 대해 FAQ/공지사항을 직접 열 수 있는 기능을 추가했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.62.0](./release-notes-android/#2620-2024-03-26)
* [Gamebase iOS SDK 2.62.0](./release-notes-ios/#2620-2024-03-26)

### 2.61.0 (2024. 02. 27.) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-Unity.zip)

#### 버그 수정
* (macOS) 내부 bundle 파일이 정상적으로 로드되지 않는 문제를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.61.0](./release-notes-android/#2610-2024-02-27)
* [Gamebase iOS SDK 2.61.0](./release-notes-ios/#2610-2024-02-27)

### 2.60.0 (2024. 01. 23.) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2600-2024-01-23)
* [Gamebase iOS SDK 2.60.0](./release-notes-ios/#2600-2024-01-23)

### 2.59.0 (2023. 12. 19.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (Standalone) macOS 지원이 추가되었습니다.
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.59.0](./release-notes-android/#2590-2023-12-19)
* [Gamebase iOS SDK 2.59.0](./release-notes-ios/#2590-2023-12-19)

### 2.57.0 (2023. 10. 31.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (공통) try/catch 구문에서 예외와 관련된 로그를 전송할 수 있는 Gamebase.Logger.Report API가 추가되었습니다.
* (iOS) AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED 오류 코드가 추가되었습니다. 

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2570-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2570-2023-10-31)

### 2.55.0 (2023. 09. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (iOS) 사용자가 푸시 권한을 거부해도 토큰을 등록할 수 있도록 GamebaseRequest.Push.PushConfiguration.alwaysAllowTokenRegistration 필드가 추가되었습니다.

#### 기능 개선
* NHN Cloud Unity SDK가 서비스 종료 됨에 따라 Gamebase Unity SDK 내에서 제거되었습니다.
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.55.0](./release-notes-android/#2550-2023-09-12)
* [Gamebase iOS SDK 2.55.0](./release-notes-ios/#2550-2023-09-12)

### 2.54.0 (2023. 08. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.54.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (Android) Android 13 이상의 OS에서 RegisterPush API를 호출했을 때 Push 권한 요청 팝업이 자동으로 뜨지 않도록 할 수 있는 GamebaseRequest.Push.PushConfiguration.requestNotificationPermission 필드가 추가되었습니다.
* (Android) loginForLastLoggedInProvider 호출 중에 로딩 애니메이션을 숨기는 옵션을 지정할 수 있는 신규 API가 추가되었습니다.
    * Gamebase.LoginForLastLoggedInProvider(Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback);
    * API 호출 방법은 다음 가이드 문서를 참고하시기 바랍니다.
        * [Game > Gamebase > Unity SDK 사용 가이드 > 인증 > Login > Login Flow > Login as the Latest Login IdP](./unity-authentication/#login-as-the-latest-login-idp)

#### 버그 수정
* (Standalone) Gamebase 고객 센터 호출 시 서비스 오류 페이지가 출력되지 않도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.53.0](./release-notes-android/#2530-2023-08-17)
* [Gamebase iOS SDK 2.54.0](./release-notes-ios/#2540-2023-08-29)

### 2.52.1 (2023. 07. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Unity.zip)

#### 버그 수정
* (Standalone) Gamebase Logger 초기화가 완료되기 전에 로그를 전송하면 null reference exception이 발생하는 오류가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.52.1](./release-notes-android/#2521-2023-07-17)
* [Gamebase iOS SDK 2.53.0](./release-notes-ios/#2530-2023-07-25)

### 2.52.0 (2023. 06. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* Setting Tool (v2.7.0)
    * ONE Store v21 결제 어댑터가 추가되었습니다. (Android에 한함)
    * Gamebase Custom Push Receiver 어댑터가 추가되었습니다. (Android에 한함)

#### 기능 개선
* 외부 SDK 업데이트: NHN Cloud Unity SDK(0.28.3)
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.52.0](./release-notes-android/#2520-2023-06-27)
* [Gamebase iOS SDK 2.52.0](./release-notes-ios/#2520-2023-06-27)

### 2.51.0 (2023. 05. 30.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.51.0/GamebaseSDK-Unity.zip)

#### 기능 개선
* 외부 SDK 업데이트: NHN Cloud Unity SDK(0.28.1)
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2500-2023-05-16)
* [Gamebase iOS SDK 2.51.0](./release-notes-ios/#2510-2023-05-30)

### 2.50.0 (2023. 05. 16.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.50.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (Android) MyCard 스토어가 추가되었습니다.
* Setting Tool (v2.5.0)
    * MyCard 스토어가 추가되었습니다. (Android에 한함)
    * Huawei IAP 추가 시 Huawei repository 자동 설정 기능이 추가되었습니다.

#### 기능 개선
* 외부 SDK 업데이트: NHN Cloud Unity SDK(0.28.0)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2500-2023-05-16)
* [Gamebase iOS SDK 2.49.2](./release-notes-ios/#2492-2023-04-28)

### 2.49.0 (2023. 04. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unity.zip)

#### 기능 개선
* (iOS) 내부 로직을 개선하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.49.0](./release-notes-android/#2490-2023-04-25)
* [Gamebase iOS SDK 2.49.1](./release-notes-ios/#2491-2023-04-25)

### 2.48.0 (2023. 03. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-Unity.zip)

#### 기능 개선
* 외부 SDK 업데이트: NHN Cloud Unity SDK (0.27.4)
* DNS 장애를 대비한 Gamebase 서버 예비 도메인 적용
* iOS
    * Xcode 최소 지원 버전이 14.1로 변경되었습니다. 
    * iOS 최소 지원 버전이 11.0으로 변경되었습니다.
    * armv7, armv7s, i386 아키텍처 지원을 중단하였습니다.
    * 더 이상 bitcode를 지원하지 않습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.48.0](./release-notes-ios/#2480-2023-03-28)

### 2.46.0 (2023. 01. 31.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* (WebGL) Google 로그인 기능이 추가되었습니다.
* (Android) 웹뷰에서 고정 폰트 사이즈 사용 여부를 설정하는 필드를 재지원합니다.
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) 웹뷰에서 컷아웃(노치) 영역을 비롯한 모든 이용 가능한 스크린 공간을 사용하여 렌더링할 수 있는 설정이 추가되었습니다.
    * GamebaseWebViewConfiguration.renderOutsideSafeArea
* (Android) IAP 구독 상태를 조회할 수 있는 RequestSubscriptionsStatus API가 추가되었습니다.

#### 버그 수정
* (Standalone) 초기화 시 간헐적으로 ReflectionTypeLoadException 오류가 발생하는 문제를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.46.0](./release-notes-android/#2460-2023-01-31)
* [Gamebase iOS SDK 2.46.0](./release-notes-ios/#2460-2023-01-31)

### 2.45.0 (2022. 12. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 미소비 내역 조회 API가 변경되어 신규 API로 변경해야 합니다.
 
        // Deprecated API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
         
        // New API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                       GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
 
* 활성화 구독 조회 API가 변경되어 신규 API로 변경해야 합니다.
    * 기존 API와 동일한 결과를 받으려면 **GamebaseRequest.Purchase.PurchasableConfiguration.allStores**의 값을 **true**로 설정해야 합니다.
 
            // Deprecated API 
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
 
            // New API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                        GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);

#### 기능 개선/변경
* 외부 SDK 업데이트: NHN Cloud Unity SDK (0.27.1)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.45.0](./release-notes-android/#2450-2022-12-27)
* [Gamebase iOS SDK 2.45.0](./release-notes-ios/#2450-2022-12-27)

### 2.44.2 (2022. 11. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.2/GamebaseSDK-Unity.zip)

#### 기능 추가

* Setting Tool (v2.5.0)
    * ONE Store v19 결제 어댑터가 추가되었습니다. (Android에 한함)
    * 기존 Setting Tool은 Unity 프로젝트에서 완전히 제거한 뒤 최신 버전으로 다시 설치해야 합니다.

#### 버그 수정
* (iOS) 게임 중 Screen.orientation을 변경하는 경우 웹뷰, 고객 센터 등 뷰 컨트롤러의 영향을 받는 API가 정상적으로 노출되지 않는 이슈를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.44.2](./release-notes-android/#2442-2022-11-29)
* [Gamebase iOS SDK 2.44.0](./release-notes-ios/#2440-2022-10-25)

### 2.44.0 (2022. 10. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: NHN Cloud Unity SDK(0.26.2)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.44.0](./release-notes-android/#2440-2022-10-11)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.26.1), Kakaogame Unity SDK(3.14.5)
* LINE 로그인을 수행 시 서비스를 제공할 Region을 입력하도록 변경되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > 인증 > Login with IdP](./unity-authentication/#login-with-idp)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.0](./release-notes-ios/#2430-2022-09-07)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unity.zip)

#### 기능 추가
* ForcingMappingTicket 클래스에 매핑 유저 상태를 나타내는 mappedUserValid 필드가 추가되었습니다.

#### 기능 개선/변경
* 웹뷰에서 고정 폰트 사이즈 사용 여부를 설정하는 필드는 더 이상 사용되지 않습니다.
    * **GamebaseWebViewConfiguration.enableFixedFontSize**
* GamebaseWebViewConfiguration의 기본값이 추가되었습니다.
    * 내비게이션 바의 색상 필드인 colorR, colorG, colorB, colorA의 기본값이 18, 93, 230, 255로 설정되었습니다.
    * 내비게이션 바 활성 여부를 지정하는 필드인 isNavigationBarVisible의 기본값이 true로 설정되었습니다.
    * 웹뷰 내 뒤로 가기 버튼 활성 여부를 지정하는 필드인 isBackButtonVisible의 기본값이 true로 설정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.6)
* GamebaseEventHandler의 GamebaseEventCategory에 **IDP_REVOKED** 타입이 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unity-etc/#idp-revoked)

#### 기능 개선/변경
* Unity의 Burst 패키지를 사용할 때 메모리 누수가 발생하는 이슈를 수정했습니다.
* Setting Tool (v2.4.0)
    * 내부 안정화 지표가 추가되었습니다.
    * 기존 SettingTool은 Unity 프로젝트에서 완전히 제거한 뒤 최신 버전으로 다시 설치해야 합니다.
    * SettingTool v1은 더 이상 지원하지 않습니다.

#### 버그 수정
* (iOS) 특정 환경에서 결제 후 크래시가 발생하는 문제를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.5)
* (Standalone) 아래 약관 API를 지원하도록 변경되었습니다.
    * Gamebase.Terms.QueryTerms
    * Gamebase.Terms.UpdateTerms

#### 기능 개선/변경
* 한글이 유니코드로 표시되는 현상이 개선되었습니다.
* (iOS) bitcode 지원하도록 수정되었습니다.

#### 버그 수정
* (Android) OpenContact API 호출 시 Configuration.additionalParameters가 적용되지 않는 문제가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.4)

#### 버그 수정
* 초기화 전에 GetLaunchingInformations() API를 호출 시 JsonException이 발생하지 않도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.39.0](./release-notes-android/#2390-2022-05-10)
* [Gamebase iOS SDK 2.39.0](./release-notes-ios/#2390-2022-05-10)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.3)

#### 기능 개선/변경
* Display Language의 중국어 번체(zh-TW) 언어셋에서 어색한 문장이 수정되었습니다.

#### 버그 수정
* (Android) API Level 24 미만에서 특정 API 호출 시 오류가 발생하지 않도록 수정되었습니다.
    * Gamebase.Purchase.RequestActivatedPurchases()
    * Gamebase.Purchase.RequestItemListOfNotConsumed()

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.38.0](./release-notes-android/#2380-2022-05-03)
* [Gamebase iOS SDK 2.38.0](./release-notes-ios/#2380-2022-05-03)

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 고객 센터 URL 뒤에 파라미터를 추가할 수 있도록 다음 필드가 추가되었습니다.
    * GamebaseRequest.Contact.Configuration.additionalParameters

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.37.0](./release-notes-android/#2370-2022-04-26)
* [Gamebase iOS SDK 2.37.0](./release-notes-ios/#2370-2022-04-26)

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.2)
* 결제 시 프로모션 여부를 알 수 있는 isPromotion 필드가 추가되었습니다.
    * GamebaseResponse.Purchase.PurchasableReceipt.isPromotion
* 결제 시 테스트 결제 여부를 알 수 있는 isTestPurchase 필드가 추가되었습니다.
    * GamebaseResponse.Purchase.PurchasableReceipt.isTestPurchase

#### 버그 수정
* 디바이스가 특정 문화권으로 설정되었을 때 결제 상품 가격 정보가 0으로 입력되는 오류가 수정되었습니다.
* (iOS) Push 알림 클릭 시 딥 링크가 동작하지 않는 오류가 수정되었습니다.
* (iOS) 프로젝트의 orientation이 Auto Rotation으로 설정되어 있고, 프로젝트 첫 신(scene)에 포함된 MonoBehaviour의 Awake에서 Gamebase API 호출 시 웹뷰 등의 UI 출력이 정상적으로 되지 않는 오류가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.36.0](./release-notes-android/#2360-2022-04-12)
* [Gamebase iOS SDK 2.36.0](./release-notes-ios/#2360-2022-04-12)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Unity.zip)

#### 기능 추가

* 외부 SDK 업데이트: TOAST Unity SDK(0.25.1)
* 약관이 표시되었는지를 알 수 있는 API가 추가되었습니다.
    * Gamebase.Terms.IsShowingTermsView()
* 웹뷰에서 내비게이션 바를 숨길 수 있는 옵션이 추가되었습니다.
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) 웹뷰에서 폰트 사이즈를 고정할 수 있는 옵션이 추가되었습니다.
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) 약관 창에서 글자 크기를 고정할 수 있는 옵션이 추가되었습니다.
    * GamebaseRequest.Terms.GamebaseTermsConfiguration.enableFixedFontSize
* Setting Tool
    * (Android) Amazon 스토어가 추가되었습니다.
    * (Android) Huawei 스토어가 추가되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.35.0](./release-notes-android/#2350-2022-03-29)
* [Gamebase iOS SDK 2.35.0](./release-notes-ios/#2350-2022-03-29)

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-Unity.zip)

#### 기능 추가
* 단말기에서 알림을 허용했는지 여부를 알 수 있는 API가 추가되었습니다.
    * Gamebase.Push.QueryNotificationAllowed()

#### 버그 수정
* iOS에서 GamebaseWebViewConfiguration의 isBackButtonVisible 설정이 적용되지 않는 오류가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-2022-02-22)
* [Gamebase iOS SDK 2.34.1](./release-notes-ios/#2341-2022-03-15)

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 공통 약관 API 호출 후 약관 UI가 표시되었는지 여부를 알 수 있는 VO 클래스가 추가되었습니다.
    * **GamebaseResponse.Terms.ShowTermsViewResult**

#### 기능 개선/변경
* 킥아웃 팝업 창 표시 여부는 Gamebase 콘솔에서 킥아웃 등록 시 설정할 수 있으므로 다음 필드가 deprecated되었습니다.
    * **GamebaseConfiguration.enableKickoutPopup**
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-2022-02-22)
* [Gamebase iOS SDK 2.34.0](./release-notes-ios/#2340-2022-02-22)

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 공통 약관 창의 설정을 변경할 수 있는 신규 API가 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > UI > Terms > showTermsView](./unity-ui/#showtermsview)

#### 기능 개선/변경
* 오류 코드 추가 및 변경
    * GamebaseErrorCode.UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 GamebaseErrorCode.SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.32.0 (2021.12.28)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED** 타입이 추가되었습니다.
    * 이 이벤트의 활용 방법은 다음 문서를 참고하시기 바랍니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unity-etc/#server-push)
* Gamebase Access Token이 만료되어 로그인이 필요할 때 동작하는 **GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category가 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unity-etc/#logged-out)
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.32.0](./release-notes-android/#2320-20211228)
* [Gamebase iOS SDK 2.32.0](./release-notes-ios/#2320-20211228)

### 2.31.0 (2021.12.14)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.25.0)
* Standalone 점검 팝업 창에서 점검 시간 표시 여부를 동적으로 설정할 수 있도록 변경되었습니다.
* Setting Tool
    * PAYCO IDP가 추가되었습니다.
    * 기존 SettingTool을 완전히 삭제한 후 재설치해야 합니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.31.0](./release-notes-android/#2310-20211214)
* [Gamebase iOS SDK 2.31.0](./release-notes-ios/#2310-20211214)

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 강제 매핑 시 IdP 로그인을 한 번 더 시도해야 하는 불편함을 개선한 새로운 강제 매핑 API가 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > 인증 > Mapping > Add Mapping Forcibly](./unity-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping() 호출 후 AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) 에러가 발생했을 때, 해당 계정으로 로그인을 할 수 있는 API가 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > 인증 > Mapping > Change Login with ForcingMappingTicket](./unity-authentication/#change-login-with-forcingmappingticket)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.30.0](./release-notes-android/#2300-20211123)
* [Gamebase iOS SDK 2.30.0](./release-notes-ios/#2300-20211123)

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.23.5)
* Setting Tool
    * 2.0.0이 새로 배포되었습니다.
    * 기존 Setting Tool을 완전히 삭제한 후 재설치해야 합니다.
    * 변경된 내용 및 사용 방법은 아래 가이드를 확인하십시오.
        * [Game > Gamebase > Unity SDK 사용 가이드 > 시작하기 > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

#### 버그 수정
* GamebaseDisplayLanguageCode 핀란드어 오타 수정
    * Finish → Finnish

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.29.0](./release-notes-android/#2290-20211109)
* [Gamebase iOS SDK 2.29.0](./release-notes-ios/#2290-2021109)

### 2.28.1 (2021.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.1/GamebaseSDK-Unity.zip)

#### 버그 수정
* (Android) DisplayLanguage 설정을 하지 않을 경우 잘못된 값으로 설정되는 문제가 수정되었습니다.
* (Standalone) 이전 프레임에서 시간이 오래 걸릴 경우 발생하는 Timeout 오류가 수정되었습니다.

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* Kakaogame 인증 추가
* '결제 어뷰징 자동 해제' 기능이 추가되었습니다.
    * [Game > Gamebase > Unity SDK 사용 가이드 > 인증 > GraceBan](./unity-authentication/#graceban)
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 '이용 정지 유예 상태' 후 이용 정지가 되도록 합니다.
    * '이용 정지 유예 상태'일 경우, 설정한 기간 내에 이용 정지 해제 조건을 모두 만족하면 정상적으로 플레이할 수 있습니다.
    * 기간 내에 조건을 충족하지 못하면 이용 정지가 됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.member.graceBanInfo API 값을 확인하고, null이 아닌 유효한 GraceBanInfo 객체를 반환한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리해야 합니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.28.0](./release-notes-android/#2280-20210928)
* [Gamebase iOS SDK 2.28.0](./release-notes-ios/#2280-20210928)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* Display Language 기능이 개선되었습니다.
    * 기본 언어코드가 **en** 이었으나, Gamebase 콘솔에서 설정한 기본언어가 반영되도록 개선하였습니다.
        * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > App > 언어 설정](./oper-app/#language-settings)

#### 버그 수정
* '등록되지 않은 게임 버전' 에러 팝업 창이 영어로만 표시되는 버그를 수정하였습니다.
* 점검 팝업 창에 중국어가 표시되지 않는 버그를 수정하였습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.27.1](./release-notes-android/#2271-20210914)
* [Gamebase iOS SDK 2.27.1](./release-notes-ios/#2271-20210914)

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.23.2)
* ONE Store V16 스토어 추가

#### 버그 수정
* Unity SDK 2.25.0에서 잘못 추가된 파일 제거
    * 경로: Assets/Gamebase/Toast/IAP/Plugins

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* Display Language 기능이 개선되었습니다.
    * Display Language 언어셋에 중국어 간체(zh-CN), 중국어 번체(zh-TW), 태국어(th)가 추가되었습니다.
* ShowTermsView API 호출 후 생성할 수 있는 PushConfiguration 객체의 생성 기준이 다음과 같이 변경되었습니다.
    * 변경 전
        * 약관 항목 중에 **Push 수신** 항목이 존재하는 경우에만 null이 아닌 유효한 PushConfiguration이 반환되었습니다.
        * 유저가 주간, 야간 홍보성 Push 수신에 모두 거부한 경우 PushConfiguration.pushEnabled는 false로 생성되었습니다.
    * 변경 후
        * 약관 UI가 표시되었다면 항상 null이 아닌 유효한 PushConfiguration이 반환됩니다.
        * ShowTermsView가 반환하는 PushConfiguration 객체의 pushEnabled 값은 항상 true입니다.
    * 변경되지 않고 동일한 점
        * 이미 약관에 동의하여 약관 UI가 표시되지 않았다면 PushConfiguration은 null로 반환됩니다.

#### 버그 수정
* Push 언어 설정은 별다른 보조 처리가 없이 단말기의 언어코드를 그대로 적용되어, Push 콘솔에서 전송한 메시지의 언어코드가 일치하지 않는 문제를 수정하였습니다.

### 2.25.0 (2021.07.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 월 결제 한도 기능 추가
    * 월 결제 한도를 넘는 경우 **PURCHASE_LIMIT_EXCEEDED(4007)** 에러가 발생합니다.

#### 기능 개선/변경
* Push 항목이 존재하는 약관에서 PushConfiguration 객체 보장
    * 약관 UI에서 Push 수신 동의를 하지 않을 경우 Gamebase.Terms.ShowTermsView API 호출 결과로 생성되는 PushConfiguration이 null이었으나, 약관에 Push 항목이 존재한다면 PushConfiguration 객체가 항상 반환되도록 변경되었습니다.
    * Push 수신 거부 시 PushConfiguration 객체는 (푸시 동의 여부 = false, 광고성 푸시 동의 여부 = false, 야간 광고성 푸시 동의 여부 = false)로 생성됩니다.
    * 약관에 Push 항목이 존재하지 않는다면 PushConfiguration 객체는 null입니다.
* Unity 최소 지원 버전 변경: 2018.4.0f1
* 외부 SDK 업데이트: TOAST Unity SDK(0.23.0)

### 2.24.0 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 내부 론칭 URL 변경

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.22.1)
* Unity 2020.2 이후 버전에서 발생하는 Warning 제거
* Standalone과 Unity Editor에서 초기화 속도 개선

#### 버그 수정
* 약관 동의를 했음에도 ShowTermsView API 호출하면 PushConfiguration 결과가 null이 아닌 문제 수정

### 2.22.0 (2021.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Unity SDK(0.22.0)

### 2.21.0 (2021.04.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 인증 추가: 일본 한게임(Hange)

### 2.20.0 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 공통 약관 기능 추가
    * 약관 웹뷰를 여는 API 추가
    * 약관 리스트 및 유저별 동의 여부를 조회하는 API 추가
    * 유저별 약관 동의 여부를 Gamebase 서버에 저장하는 API 추가

#### 기능 개선/변경
* 고객 센터 타입이 TOAST 조직 상품(Online Contact)인 경우 로그인을 하지 않아도 고객 센터가 표시되도록 변경
* Warning 로그 제거
* Standalone 웹뷰에 CEF 2.1.2 업데이트
    * URL의 길이가 2,048보다 길 경우 크래시가 발생하는 이슈 수정
    * Unity 2019에서 빌드 시 라이브러리 경로가 변경되어 PostProcessBuild 개선
    * string 초기화를 하지 않아 간헐적으로 발생하는 오류 수정
    * Gamebase 웹뷰 사용 중 웹뷰가 신(scene)을 이동한 이후에는 다시 열리지 않는 버그 수정

### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.19.0
    * (공통) Weibo 인증 추가
    
#### 기능 개선/변경
* [SDK] 2.19.0
    * (공통) 론칭 상태 코드 추가: 베타 서비스(205)

#### 버그 수정
* [SDK] 2.19.0
    * (Unity) WebSocket에서 재시도 시 OutOfMemoryException이 발생하는 문제 수정
    
### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.18.2
    * (공통) 개발사 자체 고객 센터 오픈 시 additionalURL 필드 추가
    * (공통) 결제 아이템 정보에 지역화된 상품 정보 추가: localizedTitle, localizedDescription

#### 기능 개선/변경
* [SDK] 2.18.2
    * (공통) TOAST SDK 업데이트: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
    * (Android) 암호화 로직 보안 경고 해결을 위한 외부 SDK 업데이트: Payco Login SDK(1.5.3), Hangame ID SDK(1.3.2)
    * (Android) Tencent Push 모듈 제거
    * (Android) Gamebase Android SDK 2.6.0에서 deprecated된 함수 제거
        * GamebaseConfiguration.Builder.setFCMSenderId()
        * GamebaseConfiguration.Builder.setTencentAccessKey()
        * GamebaseConfiguration.Builder.setTencentAccessId()
    * (iOS) showWebView: 잘못된 URL을 전달했을 경우 에러 반환, 전달받은 URL은 인코딩하지 않고 그대로 사용
    * (iOS) 대소문자 상관없이 커스텀 스킴이 동작하도록 변경
    * (Unity) GamebaseRequest.GamebaseConfiguration 클래스의 필드 deprecated: zoneType, fcmSenderId

#### 버그 수정
* [SDK] 2.18.2
    * (Android) 5.0~6.0 OS 단말기에서 웹뷰 커스텀 스킴이 동작하지 않는 문제 수정

### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* Galaxy 스토어 추가: SDK 2.18.0

#### 기능 개선/변경
* [SDK] 2.18.0
    * (Android) TOAST SDK 업데이트: [Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027)-GooglePlay Billing Library v.3.0.1 적용
    * (Android) WebView SSL 보안 경고 대응 처리 추가
    * (iOS) iOS 13 이상부터 제공되는 SceneDelegate 대응 API 추가

#### 버그 수정
* [SDK] 2.18.1
    * (Android) 2.18.0 에서 Google 결제 후 크래시가 발생하는 문제 수정

### 2.17.1 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Unity.zip)

#### 기능 추가
* Unreal SDK 기능 추가: SDK 2.15.0
    * 기존의 모든 이벤트 시스템을 통합하는 GamebaseEventHandler 추가
        * ServerPush, Observer 기능을 포함하고 있고, 프로모션 결제 이벤트 및 푸시 이벤트 확인 가능
    * API 추가
        * 상품 ID로 결제 요청하고 추가 정보(UserPayload)를 입력해 결제 완료 시 확인 가능한 결제 API 추가
        * 이미지 공지 표시: showImageNotices
        * Push 토큰 정보 확인: queryTokenInfo
    * 푸시 토큰 등록 시 NotificationOption 설정으로 앱이 포그라운드(foreground) 상태에서도 푸시 알림을 받을 수 있도록 기능 추가
    * WebViewConfiguration contentMode 설정 추가
    
#### 기능 개선/변경
* [SDK] 2.17.1
    * (Unity) Unity 2017.2.5 지원

#### 버그 수정
* [SDK] 2.17.1
    * (Unity) 이미지 공지와 웹뷰를 차례로 호출하면 뒤에 호출한 API가 동작하지 않는 오류 수정    

### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-Unity.zip)

```
한게임 인증 사용을 원하는 경우 고객 센터로 미리 연락주세요.
```

#### 기능 추가
* Hangame IdP 인증 추가: SDK 2.17.0

#### 기능 개선/변경
* [SDK] 2.17.0
    * (공통) 고객 센터 첨부 이미지 클릭 시 다운로드 지원
    * (공통) TOAST SDK 업데이트: Android(0.23.2), Unity(0.21.2)
    * (iOS) TCGBMember.regDate, TCGBMember.lastLoginDate의 타입을 long long으로 변경
    * (iOS) 웹뷰에서 URL 및 타이틀 변경 시 타이틀을 재출력할 수 있도록 로직 변경

#### 버그 수정
* [SDK] 2.17.0
    * (iOS) PAYCO 인증: lastLoggedInProvider 로그인 후 로그아웃 호출 시 로그아웃 콜백이 오지 않는 문제 수정
* [SDK] 2.17.1
    * (Android) 2.17.0에서 ImageNotice API 호출 시 kotlinx-coroutine 모듈에서 크래시가 발생하는 문제 수정
    
### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 고객 센터 기능 추가
    * [SDK] 2.16.0
    * (공통) API 추가(Gamebase.Contact.requestContactURL): 고객 센터 URL 반환
    * (공통) 고객 센터 API 에 userName 을 설정할 수 있도록 ContactConfiguration 파라미터 추가 
        
### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unity.zip)

```
Gamebase SDK 2.15.0 버전에서 Google Billing Client 모듈이 업데이트 되었습니다.

'gamebase-adapter-purchase-google'을 사용한다면 Gamebase SDK 2.15.0 미만 버전에서 2.15.0 이상으로 업그레이드하는 경우 
반드시 이전 버전의 'Game Client Version'을 '업데이트 필수'로 설정해야 합니다.

아이템을 구매하다 오류가 발생하면 재처리를 수행하게 되는데 
여러 개의 단말기에서 서로 다른 Billing Client 버전이 적용된 상태에서는 재처리 수행 중에 문제가 생길 수 있기 때문입니다.
```

#### 기능 추가
* [SDK] 2.15.0
    * (공통) 푸시 토큰 등록시 NotificationOption 설정으로 앱이 포그라운드(foreground) 상태에서도 푸시 알림을 받을 수 있도록 기능 추가
    * (공통) 푸시 API 추가: Push 토큰 정보 확인(Gamebase.Push.queryTokenInfo API)

#### 기능 개선/변경
* [SDK] 2.15.0
    * (공통) TOAST SDK 업데이트: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) 결제 payload의 null check 로직 추가
 
### 2.14.0 (2020.08.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.14.0
    * (iOS) PAYCO IdP의 상수값 제거: PAYCO 문자열로 인한 앱 스토어 심사에서 거절되는 경우가 발생하여 제거
    * (iOS, Unity) TCGBWebViewConfiguration에 contentMode 설정 추가

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.13.0
    * (Unity) Standalone: 이미지 공지 표시 API 추가    

#### 기능 개선/변경
* [SDK] 2.13.0
    * (Android) 이미지 공지의 팝업 창 이미지 비율 계산 로직 수정
    * (iOS) Sign In With Apple 인증: iOS 12 이하 지원

#### 버그 수정
* [SDK] 2.13.0
    * (Android) 웹뷰 종료 시 종료 콜백에서 ANDROID_ACTIVITY_DESTROYED(31) 오류가 반환되는 문제 수정
    * (Android) 결제 모듈에 ProGuard 선언이 누락된 오류 수정
    
### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 이미지 공지: 표시 기간과 우선순위에 따라 게임 내 이미지 팝업 창 표시
    * [SDK] 2.12.0: 이미지 공지 표시 API 추가

#### 기능 개선/변경
* [SDK] 2.12.0
    * (iOS) Facebook SDK 업데이트(7.1.1)
    * (iOS) configuartion에 설정된 storeCode(default=AS)로 Gamebase 초기화 시도
    * (iOS) 콘텐츠를 로딩할 수 없는 웹뷰 출력 시 **닫기** 버튼이 없어 닫을 수 없는 문제 수정
    * (Unity) TOAST Unity SDK 업데이트(0.20.1.1)
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.11.0
    * 결제 API 추가: 상품 ID로 결제 요청, 추가 정보(UserPayload) 입력해 결제 완료 시 확인할 수 있음

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.10.1
    * (iOS) 사용자 푸시 설정 초기화 시 언어 코드가 설정되어 있지 않으면 디바이스 언어로 설정되도록 변경

#### 버그 수정
* [SDK] 2.10.1
    * (Unity) iOS Plugin에서 ViewController가 설정되지 않아 로그인 호출 시 실패하는 문제 수정

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.10.0
    * (공통) 기존의 모든 이벤트 시스템을 통합하는 GamebaseEventHandler 추가
        * ServerPush, Observer 기능을 포함하고 있고, 프로모션 결제 이벤트 및 푸시 이벤트도 확인 가능

#### 기능 개선/변경
* [SDK] 2.10.0 
    * (Unity) StandaloneWebviewAdapter 내부의 CefWebview 버전 업데이트: v2.0.4
        * WebviewIndex 검증 로직을 개선
        * 웹뷰 생성 시, 간헐적으로 NullReferenceException이 발생하는 오류를 개선
    * (Unity) GamebaseErrorCode에 소켓 연결에 관한 에러 코드 추가: SOCKET_CONNECTION_TIMEOUT, SOCKET_CONNECTION_FAIL

#### 버그 수정
* [SDK] 2.9.1
    * (Andoird) 매핑 이후 지표 레벨이 null이 되어 결제 지표에 정상적으로 반영되지 않는 오류 수정
    * (iOS) unreal 엔진에서 빌드 하면, warning을 빌드 오류로 판정해서 빌드가 안되는 부분을 수정

### 2.9.1 (2020.04.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unity.zip)

#### 버그 수정
* [SDK] 2.9.1 
    * (Unity) Initialize 이후 콘솔에서 클라이언트의 서비스 상태를 변경하면 오류가 발생하는 문제를 수정
        * 이슈 발생 버전: v2.8.0 이상    
        * 이슈가 있는 플랫폼: Standalone, WebGL, Editor
        
### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 탈퇴 유예 기능
    * [SDK] 2.9.0
        * (공통) API 추가: 탈퇴 유예 신청, 탈퇴 유예 신청 취소, 탈퇴 유예 상태에서 즉시 탈퇴, 유저의 탈퇴 유예 여부 확인
        
#### 기능 개선/변경
* [SDK] 2.9.0
    * (공통) TOAST SDK 업데이트: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
    * (공통) PAYCO Login SDK 업데이트: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.8.1 
    * (공통) Analytics 전송 결과 확인을 위한 내부 지표 추가
    
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.8.0
    * (공통) 결제 및 상품 정보에 상품 타입 및 지역 가격 등의 정보를 추가
    * (Unity) StandaloneWebviewAdapter 내부의 CefWebview가 v2.0.1 버전으로 업데이트
        * PopupType이 PASS_INFO일 경우, 팝업 창을 띄우지 않고 팝업 창 정보를 전달하는 기능을 추가

#### 기능 개선/변경
* [SDK] 2.8.0 
    * (공통) 콘솔에 등록되지 않은 앱 버전으로 초기화 실패할 때 스토어로 이동할 수 있는 팝업 창이 추가로 노출하도록 개선
    * (Android) 로그인 직후 결제 관련 API를 호출할 때 초기화 타이밍 문제로 실패가 발생할 수 있는 코드를 수정
    
### 2.7.2 (2020.03.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.7.2 
      * (Unity) FacebookAdapter 개선 
            * v7.9.4~v7.18.1 버전까지 호환성 테스트
            * Null 예외 처리 
      * (Unity) StandaloneWebviewAdapter 개선 
            * 웹 페이지를 텍스처(texture)로 내보내기 추가
            * 멀티 웹뷰 지원 
            * 쿠키 삭제 옵션 추가 
            * 텍스처(texture) 크기 조절 지원 
        * 스크롤바 표시/숨기기 지원 
            * 페이지 로드 완료 알림 
            * 투명 배경 지원 
      * (Unity) 에디터에서 Android/iOS 플랫폼을 선택하고 Initialize API를 호출하면 오류가 발생하는 문제 해결

### 2.7.0 (2020.01.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.7.0
    * (Unity) NaverCafePLUG 지원

#### 버그 수정
* [SDK] 2.7.0
    * (Android) 서버 응답(response)에서 traceError 필수 파라미터가 없더라도 크래시가 발생하지 않도록 수정
    * (Android) Firebase 설정이 누락되어 있을 때 예외가 발생하지 않도록 수정
    * (Unity) Web Login 시, gamebase://dismiss 스킴 처리 추가
    * (Unity) 릴리스 빌드 시, 간헐적으로 웹뷰가 노출되지 않는 문제 수정    

### 2.6.3 (2020.01.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.3/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.6.3
    * (Unity) Standalone 웹뷰 개선: CefWebview 업데이트    
    * (Unity) 로그인 이후 오류가 발생하여 누락된 .dll 파일 추가
        * ToastCommon.dll, vcruntime140.dll

#### 버그 수정
* [SDK] 2.6.3
    * (Unity) Login(CredentialInfo) API 호출 시 오류가 발생하여 수정
    
### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Unity.zip)

#### 기능 추가
* 쿠폰 > 쿠폰 발급: 키워드 쿠폰 기능 추가

#### 기능 개선/변경
* [SDK] 2.6.2
    * (공통) TOAST SDK 업데이트: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
    * (iOS) NAVER SDK 버전 업데이트(4.1.0)

### 2.6.1 (2019.11.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Unity.zip)

#### 버그 수정
* [SDK] 2.6.1
    * (Unity)iOS Plugin 파일이 Package에 누락되어 iOS 빌드 시 에러가 발생하여 해당 파일을 추가: 'toast_sdk_wrap.m'
    * (Unity)UnityEditor에서 Standalone 이외의 플랫폼으로 실행시 Store Code가 Empty로 입력되어 초기화에 실패하는 오류 수정
    * (Unity)Initialize API 내부 zone type 처리 부분에서의 오류로 NullReferenceException 발생하던 오류 수정

### 2019. 11. 13.

#### 버그 수정
* GamebaseSettingTool
    * Gamebase v2.6.0 업데이트 시, 파일이 정상적으로 변경되지 않는 오류 수정


### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Unity.zip)

```
Gamebase SDK 2.6.0 미만 버전에서 2.6.0으로 업그레이드 하는 경우
반드시 Upgrade Guide문서에 설명된 변경 사항을 반영해 주시기 바랍니다. 
가이드 위치: Game > Gamebase > Upgrade Guide
```

#### 기능 추가
* Google 구독 결제 기능 추가
    * [SDK] 2.6.0 Android
* [SDK] 2.6.0
    * (공통) 데이터를 Log&Crash 에 전송하여 각종 분석에 이용할 수 있도록 TOAST Logger 추가
    * (iOS) Sign In with Apple 인증 추가
    * (Android) Gamebase Android SDK가 Bintray 를 통해 배포되므로 gradle 설정만으로 Gamebase 를 사용할 수 있음

#### 기능 개선/변경
* [SDK] 2.6.0
    * (Unity) 로그인시 LaunchingStatus를 갱신하는 로직에 오류가 있어 수정
    * (Unity) Debug Log를 전송하는 기능을 Gamebase 콘솔에서 설정할 경우 Client에서 로그 전송을 무한 반복하는 오류 수정

### 2019. 10. 15.
#### 기능 개선/변경
* Sample App
    * Gamebase SDK 업데이트(v2.4.0)
    * Smart Downloader 적용(v1.5.8), Leaderboard 적용
    * 기능 추가: 게임리소스 다운로드, Leaderboard, TAA 지표 연동, 단말기 이전 기능, 강제 매핑 기능
    * 개선/변경: ServerPush 리스너 추가, Observer 점검 여부 감지 추가
    * 게임 리뉴얼
        
### 2.5.0 (2019.08.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 2.5.0
    * Console에서 입력한 CS URL을 웹뷰로 오픈하는 API 제공
    
### 2019. 08. 02.
#### 버그 수정
* [SDK] Setting Tool 1.4.3
    * Script 파일의 위치를 Editor 폴더 아래로 이동하여 빌드 오류를 해결
    * Mac OS에서 Multilanguage에 Language 파일의 전체 경로를 지정하면 동작하지 않던 문제 수정

### 2.4.4 (2019.07.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.4.4
    * (공통) 회원 오류 코드 포맷 변경
    * (Unity) GamebaseServerPushType에 키 추가(TRANSFER_KICKOUT)
* Setting Tool
    * 폴더 구조 변경: `기존 SettingTool을 완전히 삭제한 후 재설치해야 합니다.`
    * 다국어 지원 추가
    
### 2.4.3 (2019.07.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-Unity.zip)

#### 버그 수정
* [SDK] 2.4.3
    * (Unity)iOS와 Android로 빌드시 AddMappingForcibly API가 동작하지 않는 오류 수정
    * (Unity)RequestRetryTransaction API 호출 시 iOSPlugin에서 JSON 파싱 오류가 있어 수정

### 2019. 06. 27.

#### 버그수정
* [SDK] Setting Tool 1.4.1
    * GamebaseSettingTool 실행시 기존 설정 정보를 가져오지 못하는 오류가 발생하여 수정

### 2.4.2 (2019.06.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.4.2
    * (공통)LaunchingInfo에 JSON string 형식의 TOAST Launching 정보를 추가

#### 버그수정
* [SDK] 2.4.2
    * (공통)Analytics 버그 수정: 로그아웃, 탈퇴, 계정 이전 시 저장된 지표 데이터를 초기화 하도록 수정

### 2.4.0 (2019.05.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* HANGAME mix 일본결제 추가
    * [SDK] 2.4.0
        * (Unity)Standalone 일본 외부결제 추가
        * (Unity)Standalone 일본 HANGAME 인증 추가

#### 기능 개선/변경
* [SDK] 2.4.0
    * (공통) 지표관련 Class 변경
        * LevelUpData Class: userLevel, levelUpTime 파라미터가 필수로 변경 / 그 외 필드 삭제 [자세히보기 [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
        * GameUserData Class: classId(게임유저의 직업) 필드 추가 [자세히보기 [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]
    * (Android)NAVER SDK 버전 업데이트(v4.2.5): NAVER SDK 버그 수정(NAVER 로그인 도중에 앱 아이콘을 통해 앱을 재시작할 경우, Activity가 강제종료 되는 이슈로 인해 인증 프로세스가 중단되는 이슈가 해결)
    * (Unity)StandaloneWebview가 32bit 빌드를 지원 (SDK 용량 53.6MB에서 99.2MB로 증가)

    
### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Unity.zip)

```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객 센터로 연락주세요.
```

#### 기능 추가
* [SDK] 2.3.0
    * (Android/Unity)중국스토어 인증/결제 추가

#### 기능 개선/변경
* [SDK] 2.3.0
    * (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 2.2.2
    * (Unity)SDK 로그 개선

#### 버그수정
* [SDK] 2.2.2
    * (Unity)AddMappingForcibly API를 호출하면 크래시가 발생하여 수정

### 2.2.1 (2019.04.02)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.1/GamebaseSDK-Unity.zip)
#### 버그수정
* [SDK] 2.2.1
    * (Unity) Unity Editor에서 Android 플랫폼을 선택하고 플레이를 하면 initialize시 서버에서 에러가 발생하는 이슈 수정
 
### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Unity.zip)
#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * TransferAccountInfo 발급 API (issueTransferAccount)
        * 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
        * 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
        * 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)        
* 강제 매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
    * (SDK공통)추가된 API 
        * 강제 매핑하는 API (addMappingForcibly)

#### 기능 개선/변경
* [SDK] 2.2.0
    * (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트
    * (iOS)LINE SDK의 App 로그인 기능이 비활성화
        * LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adatper에서 Web 로그인만 지원하도록 변경
    * (Unity)GamebaseMainActivity의 Package Name이 변경
        * com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Unity.zip)
#### 기능 개선/변경
* [SDK] 2.1.0
    * (공통)TransferKey API 삭제
        * issueTransferKey : TransferKey 발급
        * requestTransfer : TransferKey 검증

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Unity.zip)
```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* [SDK] 2.0.0
    * (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
        * setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
        * traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Unity.zip)
#### 기능 개선/변경
* [SDK] 1.14.2
    * (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정
    * (iOS)Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 크래시가 발생될 수 있는 구조 수정

#### 버그수정
* [SDK] 1.14.2
    * (Unity)ShowWebView API 호출 시 파라메타에 Callback을 넣지 않으면 크래시가 발생되는 부분 수정
    * (Unity)iOS SDK의 Deleted API를 호출하는 코드가 있어 컴파일시 오류가 발생 되는 버그 수정
    
### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 1.14.0
    * (공통)Gamebase 웹뷰에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
    
#### 기능 개선/변경
* [SDK] 1.14.0
    * (공통)이용 정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
    * (Unity)GamebaseSDKSetting 오브젝트가 있는 씬으로 돌아갈 경우 오브젝트가 중복으로 생기지 않도록 개선
    * Remove API : Webview, Network, Launching
        * ShowWebBrowser(string url)
        * ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
        * ShowToast(string message, int duration)
        * AddUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback) 
        * RemoveUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
        * AddOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
        * RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
    * Deprecated  API 
        * GetLanguageCode()
* [SDK] Setting Tool        
    * 팝업 창 및 UI 개선
    
### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 1.13.0
    * (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
    * (Unity)로그에서 보여주는 json 데이터를 알아보기 쉽도록 출력 포맷 개선
    
#### 버그수정
* [SDK] 1.13.0
    * (Unity)Unity 2017.2 이상 버전에서 Editor Play Mode 종료 시 websocke close 처리에서 발생하던 오류 수정
        
### 1.12.1 (2018.08.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 1.12.1
    * (공통)IAP SDK 최신버전 적용 (1.5.0)
    * (공통)Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
    * (공통)점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
    * (공통)IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (공통)인증 API 중복 호출 시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (Android)TencentPush SDK 업데이트 (3.2.3)
    * (Android)Onestore v17(API v5) 지원 : Gamebase에서는 v16(스토어코드=TS)은 제공하지 않습니다.
    * (iOS)오류 코드 추가 : Gamecenter 로그인 거부(TCGB_ERROR_IOS_GAMECENTER_DENIED)
* [SDK] Setting Tool
    * 폴더명 변경 : TOAST -> Toast
    * 에러 발생 시 팝업 창 알림 추가 : File Download 실패, File Extract 실패, XML 파싱 실패
    
### 1.12.0 (2018.07.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] Setting Tool
    * Setting Tool 최신 버전이 있을 경우 업데이트 알림 기능 추가 
    * 내부 null Exception 수정
    
#### 버그수정
* [SDK] 1.12.0
    * (Unity)IssueTransferKey API 호출 시 exception 발생하던 버그 수정
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

### 1.11.0 (2018.06.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* iOS Google IdP 추가 : iOS
* Twitter IdP 추가 : Android, iOS
* LINE IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
    
#### 기능 개선/변경
* [SDK] 1.11.0
    * (공통)LocalizedString 일본어 번역 추가
    * (공통)인증 API 호출 시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
    * (Android)'android.permission.READ_PHONE_STATE' 권한 제거
    * (Android)GamebaseConfiguration.Builder의 필수 설정값인 setAppId, setAppVersion을 생성자에서 입력할 수 있도록 변경
    * (Android)GamebaseConfiguration.Builder 의 setServerApiVerseion API를 제거
    * (Android)getAuthBanInfo() API, class AuthBanInfo 이름을 변경 : getBanInfo(), class BanInfo
    * NAVER ID Login SDK 업데이트 : iOS(4.0.10)
* Sample App 
    * ServerPush 기능 및 Observer 기능 추가
    * Gamebase SDK 업데이트 : Android(1.9.0), iOS(1.9.0), Unity(1.10.1)    
    
### 1.10.1 (2018.06.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.1/GamebaseSDK-Unity.zip)

#### 버그수정
* [SDK] 1.10.1
    * (Unity)Unity Adapter가 없는 경우 AddMapping API 호출 시 내부적으로 로그인으로 처리하던 버그 수정

### 1.10.0 (2018.06.07)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.0/GamebaseSDK-Unity.zip)

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
    
### 1.9.0 (2018.05.18)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 1.9.0
    * Unity SDK(1.9.0) Google Adapter 신규버전(1.6.2)으로 교체하여 재배포
        * 5/3 배포된 Unity SDK(1.9.0)에 적용된 Google Adapter를 최신버전으로 교체(1.6.1->1.6.2)
    
### 1.9.0 (2018.05.03)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)
* 이용 정지 등록시 사용자의 리더보드(랭킹) 데이터를 삭제할 수 있는 옵션 추가(TOAST Leaderboard를 사용하는 경우에 한함)
    * 이용 정지 등록 메뉴를 이용하거나 App Guard 연동 페이지에서 사용 가능

### 1.8.1 (2018.04.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Unity.zip)
#### 버그 수정
* [SDK] 1.8.1
    * (Unity)UnityAndroid 플랫폼에서 아래 기능 사용 시 모듈 초기화가 되지 않아 NullReferenceException이 발생하여 수정
        * Launching, Purchase, Push, Util, Webview

### 1.8.0 (2018.04.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    * 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    * 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    * (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용 정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
    * (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
    * (iOS)페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
    * (iOS)iPhoneX 지원을 위하여, 웹뷰 사용 인터페이스 수정

#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정
* [SDK] 1.8.0
    * (Setting Tool)Unity Facebook Adapter를 체크하면 에러가 나는 버그 수정

### 1.7.1 (2018.03.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.1/GamebaseSDK-Unity.zip)

#### 버그 수정
* [SDK] 1.7.1
    * (Unity)Inspector에서 설정된 SetDebugMode 값이 반영 안 되던 버그 수정
    * (Unity)Standalone, WebGL: Display Language에서 사용되는 리소스 파일 누락 부분 수정
    * (Unity)Google Adapter 1.6.2 배포: Google Adapter 1.6.1에서 AuthCode가 Empty로 반환되어 인증 실패하는 버그 수정

### 1.7.0 (2018.02.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Unity.zip)
#### 기능 추가
* [SDK] 1.7.0
    * Naver IdP 인증 추가
    * Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 1.6.0 (2018.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 1.6.0
    * (Unity)Standalone WinSDK 추가
        * 64비트 지원
        * 인증 지원 : facebook, google, payco

### 1.5.0 (2017.12.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 1.5.0
    * 웹뷰가 닫힐 때 발생하는 Close Callback 추가
    * 웹뷰에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
    * Unity Setting Tool 신규 배포

#### 버그 수정
* [SDK] 1.5.0
    * (Unity)UnityEditor에서 Guest로그인이 되지 않는 현상 수정
    * (Unity)TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가

### 1.4.0 (2017.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 1.4.0 업데이트
    * (Unity)Gamebase Facebook Adapter가 추가 : Android, iOS, WebGL, Standalone Platform 및 UnityEditor 지원

### 1.3.0 (2017.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* [SDK] 1.3.0 업데이트
    * Credential을 이용한 AddMapping API추가

#### 기능 개선/변경
* [SDK] 1.3.0 업데이트
    * (Unity)CredentialInfo를 사용하는 Login API호출 시 iOSPlugin에서 Json 파싱이 안되던 버그를 수정

### 1.2.0 (2017.09.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Unity.zip)

#### 기능 추가
* 이용 정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
    * 이용 정지 사용자 팝업 창 노출

### 1.1.5 (2017.07.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
    * 시스템 팝업 창 API 추가 (showAlertWithTitle)
    * 국가코드를 대문자로 반환하도록 변경 (Android)
    * TCPush SDK 1.4.1 로 업데이트
    * IAP SDK 1.3.3.20170627 로 업데이트

### 1.1.4 (2017.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
    * 런타임 중 결제 Store를 변경할 수 있는 API 제공
    * (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공

### 1.1.2 (2017.04.04)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임 론칭시 점검, 긴급공지 팝업 창 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리

### 1.1.0 (2017.03.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Unity.zip)

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

### 1.0.0 (2017.03.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Unity.zip)

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
    * 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
    * 로그아웃 및 회원탈퇴 기능을 제공
    * 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
    * 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
    * 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
    * TOAST Cloud상품 연동 : PUSH, IAP
