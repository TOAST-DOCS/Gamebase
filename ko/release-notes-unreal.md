## Game > Gamebase > 릴리스 노트 > Unreal

### 2.69.1 (2025. 3. 4.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.1/GamebaseSDK-Unreal.zip)

#### 기능 추가

* (Windows) 런칭 정보에서 약관 정보를 확인할 수 있도록 추가했습니다.
    * FGamebaseLaunchingInfo::FApp::FTermsService

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.
* 로그인 API에서 UGamebaseJsonObject 대신 FGamebaseVariantMap(TMap<FName, FVariant>) 형태로 변경되었습니다.

#### 버그 수정

* (Windows) 게스트 로그인에서 UUID 발급과정 오류로 인해 모두 동일한 값이 생성되는 문제를 수정했습니다.
* (Windows) Line IDP 로그인 시 region 설정이 동작하지 않는 문제를 수정했습니다.
* (Windows) 킥아웃 시 ServerPushAppKickOut 이벤트 발생과 팝업이 노출되지 않는 문제를 수정했습니다.
* (Windows) 심볼 생성 시 엔진의 Build Configuration이 Development가 아닌 경우 오류가 발생하는 문제를 수정했습니다.
* (Android) RegisterPush가 동작하지 않는 문제를 수정했습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.69.0 (2025. 2. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unreal.zip)

#### 기능 추가

* **RequestLastLoggedInProvider 비동기 API**를 추가했습니다.
    * GetLastLoggedInProvider() 동기 API가 타이밍상 정상적인 값을 반환하지 못할 때가 있습니다.
    * (Android) GPGS의 Auto Login 기능을 사용 시 GPGS 서버에서 데이터를 획득하는 시간이 필요하므로 Gamebase 초기화 직후 GetLastLoggedInProvider() 동기 API를 호출하면 정상적인 값을 획득할 수 없습니다.
        이때 RequestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) 비동기 API는 정확한 값을 보장합니다.
        Auto Login을 사용하지 않는다면 GetLastLoggedInProvider() 동기 API를 사용해도 무방합니다.
* (Android) GPGS v2 인증 추가되었습니다.
    * 자세한 내용은 다음 링크를 참고하세요.
        * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Android Settings](./unreal-started/#android-settings)
* (Android) **FGamebaseWebViewConfiguration::CutoutColor 필드**를 추가했습니다.
    * GamebaseWebView의 **FGamebaseWebViewConfiguration::bRenderOutSideSafeArea 필드**를 **false**로 설정한 경우, cutout 영역에 자동으로 padding 여백을 추가합니다.
    * CutoutColor 필드는 이렇게 추가된 padding 영역의 색을 설정할 수 있습니다.
    * RenderOutsideSafeArea 필드를 false로 설정했지만 CutoutColor 필드는 설정하지 않는 경우에는 웹 페이지 'body'의 'background-color' 값으로 자동으로 padding 영역의 색상을 결정합니다.

#### 기능 개선/변경

* 내부 로직을 개선하였습니다.

#### 버그 수정

* 약관 조회 결과 API인 FGamebaseQueryTermsResult가 수정되었습니다.
    * TermsCountryType의 값이 설정되지 않는 문제를 수정했습니다.
    * bPushEnabled, bAdAgreement, bAdAgreementNight가 제거되었습니다.
* (Android) Windows 환경에서 빌드 시 포스트 빌드 프로세스에서 오류가 발생하지 않도록 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.68.1 (2025. 01. 21.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.
* (Windows) WebView 플러그인을 옵션으로 선택할 수 있도록 변경되었습니다.
    * 자세한 내용은 다음 링크를 참고하세요.
        * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Windows Settings > WebView 플러그인 안내](./unreal-started/#windows-settings)
* (Windows) 크래시 로그 전송 시 프로젝트 바이너리 경로에 심벌 파일을 압축한 파일이 생성되도록 추가되었습니다.
    * 자세한 내용은 다음 링크를 참고하세요.
        * [Game > Gamebase > Unreal SDK 사용 가이드 > Logger > Crash Reporter](./unreal-logger/#crash-reporter)

#### 버그 수정
* (Windows) 내부 로그 전송 시 크래시가 발생할 수 있는 로직이 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2680-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2681-2024-12-10)

### 2.68.0 (2024. 12. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.
* (Windows) Twitter 인증 방식을 OAuth 2.0으로 변경하여, 아래의 설정 변경 없이는 로그인이 동작하지 않습니다.
    * OAuth 2.0 Client ID 및 Client Secret 발급
        * Twitter Developer Portal에서 OAuth 2.0 Client ID와 Client Secret을 생성한 후, Gamebase 콘솔에 등록합니다.
    * Callback URL 설정
        * Gamebase 콘솔에 Callback URL(https://id-gamebase.toast.com/oauth/callback)을 설정합니다. 
        * 동일한 Callback URL을 Twitter Developer Portal에 추가합니다.
    * 자세한 내용은 다음 링크를 참고하세요.
        * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Authentication Information](./oper-app/#authentication-information)

#### 버그 수정
* (Windows) 결제 프로세스에서 크래시가 발생하지 않도록 수정했습니다.
* (Windows) Steam 결제 중 ESC 키로 결제를 종료하는 경우 다음 결제 API가 동작하지 않는 이슈를 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2680-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2681-2024-12-10)

### 2.67.2 (2024. 11. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.2/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.

#### 버그 수정
* (Windows) Apple ID 로그인을 정상적으로 진행하지 못하는 문제가 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.67.1 (2024. 11. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.1/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* (Windows) Purchase 설정 시 스토어를 하나만 선택할 수 있도록 변경되었습니다.
    * 스토어 재설정이 필요합니다.
* (Windows) Epic Games Store 사용 시 EOS SDK의 핸들을 등록하는 과정이 변경되었습니다.
    * Online Subsystem EOS를 사용하는 경우 Gamebase 초기화 시 StoreCode가 Epic Games Store의 해당하는 값이면 자동으로 핸들을 등록합니다.
    * Online Subsystem EOS를 사용하지 않는 경우 [Windows Settings](./unreal-started/#windows-settings) 가이드를 참고하여 EOS의 핸들을 등록하는 과정이 필요합니다.
* (Windows) Steamworks SDK 지원 버전이 1.59로 변경되었습니다.
    * 자세한 내용은 다음 링크를 참고하세요.
        * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Windows Settings > Steamworks 서비스](./unreal-started/#windows-settings)

#### 버그 수정
* 헤더 파일을 정상적으로 참조할 수 있도록 수정했습니다.
* (Windows) 초기화를 여러번 시도 시 크래시가 발생하지 않도록 수정되었습니다.
* (Windows) 초기화 시 StoreCode가 Steam 혹은 Epic Games Store에 해당하는 코드를 입력 시 크래시가 발생하지 않도록 수정되었습니다.
* (Windows) 외부 브라우저를 이용한 로그인 시도 시 크래시가 발생할 수 있는 로직이 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.67.0 (2024. 10. 30.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* Steam 인증이 추가되었습니다.
* Steam 결제가 추가되었습니다.
* 이미지 공지 기능에 신규 타입이 추가되었습니다.
    * 롤링 팝업 타입이 추가되었습니다.
    * 기존의 이미지 공지는 팝업 타입으로 표기되며, Windows에서는 지원되지 않습니다.
* (Windows) LINE 인증이 추가되었습니다.

#### 기능 개선/변경
* 엔진의 지원 버전이 4.27~5.4로 변경되었습니다.
* 내부 로직을 개선했습니다.

#### 버그 수정
* 크래시 로그 발생 시 크래시가 발생할 수 있는 로직을 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.66.1 (2024. 09. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.66.3](./release-notes-android/#2663-2024-09-10)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2662-2024-08-27)

### 2.66.0 (2024. 08. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* API 사용 방식이 변경되었습니다.
    * `IModuleInterface`를 상속 받은 **IGamebase**에서 제공하던 API를 `UGameInstanceSubsystem`을 상속 받은 **UGamebaseSubsytem**에서 제공하도록 변경했습니다.
    * **UGamebaseSubsytem**은 GameInstance의 서브시스템이므로 GameInstance 생명 주기를 따르며 SDK API 호출 시 사용하는 GameInstance를 통해 해당 서브시스템을 찾아서 API를 사용해야 합니다.
* GamebaseInterface 모듈이 제거되었습니다. Gamebase 플러그인 사용 시 GamebaseInterface 모듈을 삭제 후 사용하시길 바랍니다.
* (Windows) GameInstance가 여러 개인 환경에서 사용할 수 있습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.66.2](./release-notes-android/#2662-2024-08-27)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2662-2024-08-27)

### 2.64.0 (2024. 06. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.

#### 버그 수정
* C++ 환경에 따라 경고가 발생하여 빌드 시 오류가 발생하는 코드가 수정되었습니다.
* (Android) ProGuard 선언이 누락되어 API 호출 시 오류가 발생하는 내용이 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2640-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2640-2024-05-28)

### 2.63.0 (2024. 04. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* (Android) Firebase Notification 설정 방식이 변경되어 플러그인 내부에 google-services-json.xml 파일 수정이 아닌 [Android 설정 툴](./unreal-started/#android-settings)에서 google-services.json 파일 경로를 지정하도록 변경되었습니다.
* (iOS) Gamebase Unreal SDK에 Privacy manifest와 서명을 적용했습니다.

#### 기능 개선/변경
* (iOS) 빌드 시 오류가 발생하지 않도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2630-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2630-2024-04-23)

### 2.62.0 (2024. 03. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* (iOS) Gamebase SDK 내부 iOS 프레임워크에 Privacy manifest와 서명을 적용했습니다.

#### 기능 개선/변경
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.62.0](./release-notes-android/#2620-2024-03-26)
* [Gamebase iOS SDK 2.62.0](./release-notes-ios/#2620-2024-03-26)

### 2.60.0 (2024. 02. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2600-2024-01-23)
* [Gamebase iOS SDK 2.60.1](./release-notes-ios/#2601-2024-02-15)

### 2.58.0 (2023. 11. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-Unreal.zip)

#### 버그 수정
* (Windows) 서버 푸시가 동작하지 않는 이슈가 수정되었습니다.
* 초기화 시 크래시가 발생할 수 있는 로직이 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.58.0](./release-notes-android/#2580-2023-11-28)
* [Gamebase iOS SDK 2.58.0](./release-notes-ios/#2580-2023-11-28)

### 2.57.0 (2023. 11. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* Windows 플랫폼 지원 추가
    * [Windows 설정 툴](./unreal-started/#windows-settings)이 추가되었습니다.
    * 플랫폼에서 지원하는 API는 각 문서에 `UNREAL_WINDOWS` 항목을 확인하시기 바랍니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2570-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2570-2023-10-31)

### 2.56.0 (2023. 10. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [Android 설정 툴](./unreal-started/#android-settings)에서 스토어 설정이 추가되었습니다.
    * Amazon Appstore, Huawei AppGallery, MyCard 선택이 추가되었습니다.
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

#### 버그 수정
* 로그인 후 탈퇴 유예 정보 및 결제 어뷰징 자동 해제 정보가 정상으로 전달되도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.56.0](./release-notes-android/#2560-2023-09-26)
* [Gamebase iOS SDK 2.55.2](./release-notes-ios/#2552-2023-09-26)

### 2.49.1 (2023. 04. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* (iOS) 결제 상품 조회 API를 호출 시 크래시가 발생하지 않도록 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.49.0 (2023. 04. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* 미소비 내역 조회 API가 변경되어 신규 API로 변경해야 합니다.
 
        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
 
* 활성화 구독 조회 API가 변경되어 신규 API로 변경해야 합니다.
    * 기존 API와 동일한 결과를 받으려면 **FGamebasePurchasableConfiguration.allStores**를 **true**로 설정해야 합니다.
 
            // Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* (Android) IAP 구독 상태를 조회할 수 있는 RequestSubscriptionsStatus API가 추가되었습니다.
* (Android) 웹뷰에서 고정 폰트 사이즈 사용 여부를 설정하는 필드를 재지원합니다.
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) 웹뷰에서 컷아웃(노치) 영역을 비롯한 모든 이용 가능한 스크린 공간을 사용하여 렌더링할 수 있는 설정이 추가되었습니다.
    * GamebaseWebViewConfiguration.renderOutsideSafeArea

#### 기능 개선/변경
* Unreal의 최소 지원 버전이 4.26으로 변경되었습니다.
* (iOS) Xcode 14.1에서 빌드 시 오류가 발생되는 이슈가 수정되었습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.43.3 (2022. 10. 04.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경
* LINE 로그인을 수행 시 서비스를 제공할 Region을 입력하도록 변경되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Login with IdP](./unreal-authentication/#login-with-idp)
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unreal.zip)

#### 기능 추가
* FGamebaseForcingMappingTicket 클래스에 매핑 유저 상태를 나타내는 mappedUserValid 필드가 추가되었습니다.
* [iOS 설정 툴](./unreal-started/#ios-settings)에서 Xcode의 경로를 지정할 수 있도록 **Xcode Path** 설정이 추가되었습니다.

#### 기능 개선/변경
* 킥아웃 팝업 창 표시 여부는 Gamebase 콘솔에서 킥아웃 등록 시 설정할 수 있으므로 다음 필드는 더 이상 사용하지 않습니다.
    * **FGamebaseConfiguration.bEnableKickoutPopup**
* FGamebaseConfiguration 내 일부 필드에 기본값이 추가되었습니다.
    * bEnableLaunchingStatusPopup의 기본값이 true로 설정되었습니다.
    * bEnableBanPopup의 기본값이 true로 설정되었습니다.
* 웹뷰에서 고정 폰트 사이즈 사용 여부를 설정하는 필드는 더 이상 사용되지 않습니다.
    * **FGamebaseWebViewConfiguration.enableFixedFontSize**
* FGamebaseWebViewConfiguratio 내 일부 필드에 기본값이 추가되었습니다.
    * 내비게이션 바의 색상 필드인 colorR, colorG, colorB, colorA의 기본값이 18, 93, 230, 255로 설정되었습니다.
    * 내비게이션 바 활성 여부를 지정하는 필드인 isNavigationBarVisible의 기본값이 true로 설정되었습니다.
    * 웹뷰 내 뒤로 가기 버튼 활성 여부를 지정하는 필드인 isBackButtonVisible의 기본값이 true로 설정되었습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* GamebaseEventHandler의 GamebaseEventCategory에 **IdPRevoked** 타입이 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unreal-etc/#idp-revoked)

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.1 (2022. 06. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* 크래시가 발생할 수 있는 로직이 수정되었습니다.
* (iOS) 동일한 API를 연속해서 호출 시 콜백이 정상적으로 전달되지 않는 문제가 수정되었습니다.

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
*  [iOS 설정 툴](./unreal-started/#ios-settings)을 제공합니다.
    * 기존 프로젝트 설정에서 **Gamebase**으로 표시되었지만 업데이트 이후 **Gamebase - Android**, **Gamebase - iOS**로 표시됩니다.
    * iOS 설정 툴을 제공하면서 빌드 시 필요한 프레임워크만 포함되도록 수정되었습니다.
* 공통 약관 API 호출 후 약관 UI가 표시되었는지를 알 수 있는 VO 클래스가 추가되었습니다.
    * FGamebaseShowTermsViewResult
* 단말기에서 알림을 허용했는지 여부를 알 수 있는 API가 추가되었습니다.
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush().QueryNotificationAllowed()
* 약관이 표시되었는지를 알 수 있는 API가 추가되었습니다.
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms().IsShowingTermsView()
* 웹뷰에서 내비게이션 바를 숨길 수 있는 옵션이 추가되었습니다.
    * FGamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) 웹뷰에서 폰트 사이즈를 고정할 수 있는 옵션이 추가되었습니다.
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
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

### 2.33.1 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* iOS 빌드 시 발생하는 오류를 수정했습니다.

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* '결제 어뷰징 자동 해제' 기능이 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > GraceBan](./unreal-authentication/#graceban)
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 '이용 정지 유예 상태' 후 이용 정지가 되도록 합니다.
    * '이용 정지 유예 상태'일 경우, 설정한 기간 내에 이용 정지 해제 조건을 모두 만족하면 정상적으로 플레이할 수 있습니다.
    * 기간 내에 조건을 충족하지 못하면 이용이 정지됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.member.graceBanInfo API 값을 확인하고, null이 아닌 유효한 GraceBanInfo 객체를 반환한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리해야 합니다.
* 강제 매핑 시 IdP 로그인을 한 번 더 시도해야 하는 불편함을 개선한 새로운 강제 매핑 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping() 호출 후 AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) 에러가 발생했을 때, 해당 계정으로 로그인을 할 수 있는 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::ServerPushAppKickOutMessageReceived** 타입이 추가되었습니다.
    * 이 이벤트의 활용 방법은 다음 문서를 참고하시기 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::LoggedOut** 타입이 추가되었습니다.
    * Gamebase Access Token이 만료되어 로그인이 필요할 때 동작합니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* 공통 약관 창의 설정을 변경할 수 있는 신규 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

#### 기능 개선/변경
* 오류 코드 추가 및 변경
    * GamebaseErrorCode::UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 GamebaseErrorCode::SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* GamebaseDisplayLanguageCode 핀란드어 오타 수정
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* 공통 약관 기능 추가
    * 약관 웹뷰를 여는 API 추가
    * 약관 리스트 및 유저별 동의 여부를 조회하는 API 추가
    * 유저별 약관 동의 여부를 Gamebase 서버에 저장하는 API 추가

#### 기능 개선/변경
* 고객 센터 타입이 TOAST 조직 상품(Online Contact)인 경우 로그인을 하지 않아도 고객 센터가 표시되도록 변경
* 내부 론칭 URL 변경
* Gamebase에서 Android multidex 적용 제거

### 2.19.2 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.2/GamebaseSDK-Unreal.zip)

#### 버그 수정
* 이미지 공지 ShowImageNotices API 호출 시 onEventCallback을 등록하지 않는 경우 닫기 버튼을 눌렀을 때 크래시가 발생하는 문제 수정
* Android 설정 툴 - Enable Hangame, Enable Weibo가 정상 동작하지 않는 문제 수정

### 2.19.1 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* Unity 빌드 중 제외되는 파일이 생길 때 발생하는 컴파일 오류 수정

### 2.19.0 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* SDK 배포: 2.16.0 ~ 2.19.0 누적된 내역 반영
    * [Android 설정 툴](./unreal-started/#android-settings) 제공: Gamebase_Android_UPL.xml 파일을 수정하는 대신 설정 툴을 사용바랍니다.
    * 고객 센터 기능 추가    
    * 인증 추가: Hangame, Weibo
    * Galaxy 스토어 추가
    * 결제 아이템 정보에 지역화된 상품 정보 추가: localizedTitle, localizedDescription
    * Android 설정 툴 제공
    * Unreal 4.26 지원
    
### 2.15.0 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unreal.zip)

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
* [SDK] 2.15.0
    * (Unreal) TOAST SDK 업데이트: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)    

#### 버그 수정
* [SDK] 2.15.0    
    * (Unreal) 결제 모듈에 ProGuard 선언이 누락된 오류 수정

### 2.9.1 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [SDK] 2.9.1
    * (Unreal) Unreal 4.22 ~ 4.25 지원
    * (Unreal) PLCrashReporter 이슈 지원: [가이드](./unreal-started/#ios-settings)

#### 기능 개선/변경
* [SDK] 2.9.1
    * (Unreal) iOS Plugin 내부 Gamebase SDK for iOS 버전 업데이트(2.9.1)
    * (Unreal) UObject 레퍼런싱 처리가 누락된 부분을 수정  
   
### 2.9.0 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [SDK] 2.9.0
    * (Unreal) SDK 신규 배포
