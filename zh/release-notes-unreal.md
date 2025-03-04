## Game > Gamebase > Release Notes > Unreal

### 2.69.1 (2025. 3. 4.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.1/GamebaseSDK-Unreal.zip)

#### 기능 추가

* (Windows) 런칭 정보에서 약관 정보를 확인할 수 있도록 추가했습니다.
    * FGamebaseLaunchingInfo::FApp::FTermsService

#### 기능 개선/변경

* API 호출 시 매개변수로 전달받는 `UGamebaseJsonObject`를 `FGamebaseVariantMap(TMap&lt;FName, FVariant&gt;)`으로 변경했습니다.
* 내부 로직을 개선하였습니다.

#### 버그 수정

* (Windows) 게스트 로그인에서 UUID 발급과정 오류로 인해 모두 동일한 값이 생성되는 문제를 수정했습니다.
* (Windows) Line IDP 로그인 시 region 설정이 동작하지 않는 문제를 수정했습니다.
* (Windows) 킥아웃 시 ServerPushAppKickOut 이벤트 발생과 팝업이 노출되지 않는 문제를 수정했습니다.
* (Windows) 심볼 생성 시 엔진의 Build Configuration이 Development가 아닌 경우 오류가 발생하는 문제를 수정했습니다.
* (Android) 환경에 따라 RegisterPush가 동작하지 않는 문제를 수정했습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.69.0 (2025. 2. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unreal.zip)

#### 기능 추가

* **RequestLastLoggedInProvider 비동기 API**를 추가했습니다.
    * **GetLastLoggedInProvider() 동기 API**가 타이밍상 정상적인 값을 반환하지 못할 때가 있습니다.
    * (Android) GPGS의 Auto Login 기능을 사용 시 GPGS 서버에서 데이터를 획득하는 시간이 필요하므로 Gamebase 초기화 직후 GetLastLoggedInProvider() 동기 API를 호출하면 정상적인 값을 획득할 수 없습니다.
        이때 RequestLastLoggedInProvider 비동기(GamebaseDataCallback&lt;String&gt;) 비동기 API는 정확한 값을 보장합니다.
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

#### 기능 개선
* 엔진의 지원 버전이 4.27~5.4로 변경되었습니다.
* 내부 로직을 개선했습니다.

#### 버그 수정
* 크래시 로그 발생 시 크래시가 발생할 수 있는 로직을 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.66.1 (2024. 09. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unreal.zip)

#### 기능 개선
* 내부 로직을 개선했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.66.3](./release-notes-android/#2663-2024-09-10)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2662-2024-08-27)

### 2.66.0 (2024. 08. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unreal.zip)

#### 기능 개선
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

#### Feature Updates
* Improved internal logic.

#### Bug Fixes
* Fixed code that caused warnings depending on C++ environment, resulting in errors on build 
* (Android) Fixed an error that occurred on API calls due to missing ProGuard declaration.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2640-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2640-2024-05-28)

### 2.63.0 (2024. 04. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* (Android) Firebase Notification 설정 방식이 변경되어 플러그인 내부에 google-services-json.xml 파일 수정이 아닌 [Android 설정 툴](./unreal-started/#android-settings)에서 google-services.json 파일 경로를 지정하도록 변경되었습니다.
* (iOS) Gamebase Unreal SDK에 Privacy manifest와 서명을 적용했습니다.

#### 기능 개선
* (iOS) 빌드 시 오류가 발생하지 않도록 수정되었습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2630-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2630-2024-04-23)

### 2.62.0 (2024. 03. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unreal.zip)

#### Added Feature
* (iOS) Applied privacy manifest and signature to Gamebase SDK.

#### Feature Updates
* Improved internal logic.

#### Platform-specific Changes
* [Gamebase Android SDK 2.62.0](./release-notes-android/#2620-2024-03-26)
* [Gamebase iOS SDK 2.62.0](./release-notes-ios/#2620-2024-03-26)

### 2.60.0 (2024. 02. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Updated internal logic.

#### Platform-specific Changes
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2600-2024-01-23)
* [Gamebase iOS SDK 2.60.1](./release-notes-ios/#2601-2024-02-15)

### 2.58.0 (2023. 11. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* (Windows) Fixed an issue where server push would not work.
* Fixed logic that could cause a crash on initialization.

#### Platform-specific Changes
* [Gamebase Android SDK 2.58.0](./release-notes-android/#2580-2023-11-28)
* [Gamebase iOS SDK 2.58.0](./release-notes-ios/#2580-2023-11-28)

### 2.57.0 (2023. 11. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added support for Windows platform
    * Added [Windows Settings Tool](./unreal-started/#windows-settings).
    * To check APIs supported by platforms, see `UNREAL-WINDOWS` in each documentation.

#### Platform-specific Changes
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2570-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2570-2023-10-31)

### 2.56.0 (2023. 10. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added Store settings in the [Android settings tool](./unreal-started/#android-settings).
    * Added Amazon Appstore, Huawei AppGallery, and MyCard to the list.
    * Added the option to select the version of the store when selecting ONE Store.
* Added the Naver IdP setting in the [iOS settings tool](./unreal-started/#ios-settings).
* (Android) Added a new API to specify an option to hide the loading animation during the LoginForLastLoggedInProvider call.
    * LoginForLastLoggedInProvider(const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback)
    * For more information, see the following guide document.
        * [Game > Gamebase > Unreal SDK User Guide > Authentication > Login > Login Flow > Login as the Latest Login IdP](./unreal-authentication/#login-as-the-latest-login-idp)
* (Android) Added the FGamebasePushConfiguration.requestNotificationPermission field to prevent the Push permission request popup from automatically appearing when calling the RegisterPush API on Android 13 and later.
* (iOS) Added the FGamebasePushConfiguration.alwaysAllowTokenRegistration field to allow users to register tokens even if they deny push permissions.

#### Feature Updates
* Modified the provided type from USTRUCT to a generic structure.
    * If the type received as a result is a value that is not provided by default, it is provided in TOptional form.

#### Bug Fixes
* Modified to ensure that the withdrawal delay information and payment abusing auto cancellation information are delivered normally after logging in.

#### Platform-specific Changes
* [Gamebase Android SDK 2.56.0](./release-notes-android/#2560-2023-09-26)
* [Gamebase iOS SDK 2.55.2](./release-notes-ios/#2552-2023-09-26)

### 2.49.1 (2023. 04. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* (iOS) Fixed a crash when calling the API to query purchases.

#### Platform-specific Changes
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.49.0 (2023. 04. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unreal.zip)

#### Added Features
*  Please update to a new API due to changes to the Query Unconsumed Purchases API.
 
        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
 
* Make sure to update to a new API due to changes to the Query Activated Subscription API.
    * To get the same results as the existing API, set **FGamebasePurchasableConfiguration.allStores** to **true**.
 
            // Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* (Android) Added the RequestSubscriptionsStatus API to view the IAP subscription status.
* (Android) Re-supported the field to set whether to use fixed font sizes from the WebView.
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) Added a setting to allow WebView to render using all available screen spaces, including cutout (notched) areas.
    * GamebaseWebViewConfiguration.renderOutsideSafeArea

#### Feature Updates
* Raised the minimum supported version of Unreal to 4.26.
* (iOS) Fixed an error that occurred when running a build from Xcode 14.1.
    
#### Platform-specific Changes
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.43.3 (2022. 10. 04.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Modified to enter a service region when logging in to LINE.
    * [Game > Gamebase > Unreal SDK User Guide > Authentication > Login with IdP](./unreal-authentication/#login-with-idp)
    
#### Platform-specific Changes
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unreal.zip)

#### Added Features
* Added the mappedUserValid field that represents the mapped user status to the FGamebaseForcingMappingTicket class.
* Added the **Xcode Path** setting to specify the path for Xcode in [iOS Setting Tool](./unreal-started/#ios-settings).

#### Feature Updates
* The following field has been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **FGamebaseConfiguration.enableKickoutPopup**
* Default values have been added to some fields in FGamebaseConfiguration.
    * The default value of enableLaunchingStatusPopup is set to true.
    * The default value of enableBanPopup is set to true.
* The field to set whether to use the fixed font size in FWebView is no longer used.
    * **FGamebaseWebViewConfiguration.enableFixedFontSize**
* Default values have been added to some fields in FGamebaseWebViewConfiguration.
    *  The default values of navigation bar colorR, colorG, colorB, and colorA are set to 18, 93, 230, 255.
    * The default value of the isNavigationBarVisible field to enable the navigation bar is set to true.
    * The default value of the isBackButtonVisible field to enable the Go Back button in WebView is set to true.
    
#### Platform-specific Changes
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added the **IdPRevoked** type to GamebaseEventCategory of GamebaseEventHandler
    * [Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unreal-etc/#idp-revoked)

#### Platform-specific Changes
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.1 (2022. 06. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed the logic that could cause a crash.
* (iOS) Fixed an issue where the callback was not passed normally when calling the same API consecutively.

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

#### Added Features
*  Provides [iOS settings tool](./unreal-started/#ios-settings).
    * In the previous project settings, it was displayed as **Gamebase**, but after the update, it is displayed as **Gamebase - Android**, **Gamebase - iOS**.
    * Made modifications so that only the necessary frameworks when running a build are included, with the iOS settings tool.
* Added the VO class to determine whether the Terms UI is displayed after calling the Common Terms API.
    * FGamebaseShowTermsViewResult
* Added an API to determine whether the device has allowed notifications or not.
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush().QueryNotificationAllowed()
* Added an API to determine whether terms and conditions have been displayed.
    * UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms().IsShowingTermsView()
* Added an option to hide the navigation bar in WebView.
    * FGamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) Added an option to fix the font size in WebView
    * FGamebaseTermsConfiguration.enableFixedFontSize
* (Android) Added an option to fix the font size in terms and conditions window.
    * FGamebaseTermsConfiguration.enableFixedFontSize
* Added the isPromotion field to determine whether it is a promotion or not when making payment.
    * FGamebasePurchasableReceipt.isPromotion
* Added the isTestPurchase field to determine whether it is a test purchase or not when making payment.
    * FGamebasePurchasableReceipt.isTestPurchase
* Added the following field to add parameters after the Customer Center URL.
    * FGamebaseContactConfiguration.additionalParameters

#### Feature Updates
* Made modifications so that, when calling an API result callback, the callback is called after switching to GameThread.
* Fixed an issue where, when calling the RequestActivatedPurchases API, the API is called twice internally.
* Changed the name of the following APIs.
    * FGamebaseAnalyticsLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* Deprecated the following fields, because it is possible to set whether to display the kickout popup window during kickout registration in the Gamebase console.
     * FGamebaseConfiguration.enableKickoutPopup
    
#### Platform-specific Changes
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

### 2.33.1 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed an error that occurred when running the iOS build.

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added a 'purchase abuse automatic release' function.
    * [Game > Gamebase > Unreal SDK User Guide > Authentication > GraceBan](./unreal-authentication/#graceban)
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always check the value of AuthToken.member.graceBanInfo API after login. If a valid GraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.
* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > Unreal SDK User Guide > Authentication > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* Added an API that allows you to log in to the corresponding account when an AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) error occurs after calling Gamebase.AddMapping().
    * [Game > Gamebase > Unreal SDK User Guide > Authentication > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* Added the **GamebaseEventCategory::ServerPushAppKickOutMessageReceived** type to GamebaseEventCategory of GamebaseEventHandler.
    * For information on how to use this event, refer to the following document.
    * [Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* Added the **GamebaseEventCategory::LoggedOut** type to GamebaseEventCategory of GamebaseEventHandler.
    * This type is applied when login is required because the Gamebase Access Token has expired.
    * [Game > Gamebase > Unreal SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > Unreal SDK User Guide > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

#### Feature Updates
* Added and changed error codes
    * Changed the error code mapped to the GamebaseErrorCode::UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the GamebaseErrorCode::SOCKET_UNKNOWN_ERROR error mapped to the error code 999.
    
#### Platform-specific Changes
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed a typo of Finnish language on GamebaseDisplayLanguageCode
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added a common Terms and Conditions feature
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that stores the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).
* Changed the internal launching URL
* Removed Android multidex-related setting from Gamebase

### 2.19.2 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.2/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed a crash that occurs when the Close button is clicked while onEventCallback is not registered when calling the Image Notification ShowImageNotices API
* Android setting tools -  Fixed a problem where Enable Hangame and Enable Weibo did not work properly

### 2.19.1 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed a compile error caused by files excluded during Unity Build

### 2.19.0 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unreal.zip)

#### More Features
* (Unreal) SDK distribution: accumulated features from 2.16.0 to 2.19.0 reflected
	* [Android Settings Tool](./unreal-started/#android-settings) provided: please use this settings tool rather than modifying the Gamebase_Android_UPL.xml file.
	* Customer Center feature added	
	* Authentication for Hangame, Weibo added
	* Galaxy Store added
	* Localized product information added in the Paid Item information field: localizedTitle, localizedDescription
	* Android settings tool provided
	* Unreal 4.26 supported

### ### 2.15.0 (October 27, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unreal.zip)

#### More Features
- Added the Unreal SDK feature: SDK 2.15.0
    - Added GamebaseEventHandler that integrates all existing event systems
        * It includes the ServerPush and Observer features and checks promotion transaction event and push event.
    * Added API
    	* Added an API that can be used to send a transaction request with product ID, enter additional information (UserPayload), and check them after the transaction is complete
    	* Display image notification: showImageNotices
    	* Check the Push token information: queryTokenInfo
    * Added a feature that allows foreground apps to receive push notifications using the NotificationOption setting if push token is registered.
    * Added the WebViewConfiguration contentMode setting

#### Feature Updates
* [SDK] 2.15.0
    * (Unreal) TOAST SDK update: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)  

#### Bug Fixes
* [SDK] 2.15.0    
    * (Unreal) Fixed an issue where the ProGuard declaration was missing in the transaction module

### ### 2.9.1 (August 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unreal.zip)

#### More Features
* [SDK] 2.9.1
    * (Unreal) Supports Unreal 4.22 ~ 4.25
    * (Unreal) Supports PLCrashReporter Issue: [Guide](./unreal-started/#ios-settings)

#### Feature Updates
* [SDK] 2.9.1
    * (Unreal) Updated Gamebase SDK version for iOS within iOS Plugin (2.9.1)
    * (Unreal) Fixed the missing part of UObject referencing 

### 2.9.0 (May 12, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### More Features
* [SDK] 2.9.0
	* (Unreal) Newly released SDK