## Game > Gamebase > Release Notes > Unreal

### 2.76.0 (2025. 11. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.76.0/GamebaseSDK-Unreal.zip)

####  기능 추가

* 가장 최근 게시된 게임 공지의 게시 시간을 제공하기 위해 `FGamebaseLaunchingInfo::FApp::FGameNotice::LatestNoticeTimeMillis` 필드를 추가했습니다.
* (Android) 미국 텍사스, 유타, 루이지애나 등 특정 관할권의 연령 확인 관련 법률 준수를 지원하기 위해 Google Play Age Signals 기반의 연령 확인 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 참고사항 > Age Signals Support](./unreal-etc/#age-signals-support)
* (Windows) Steam 인증 시 Steamworks SDK가 로드되지 않은 경우 외부 브라우저를 통한 로그인을 지원합니다.

#### 기능 개선/변경

* `IGamebasePurchase::RequestItemListAtIAPConsole()` API가 deprecated 되었습니다.
    * `IGamebasePurchase::RequestItemListPurchasable()` API를 사용하세요.
* 내부 로직을 개선했습니다.

#### 버그 수정
* (Windows) Google 결제 시 브라우저 로그인 상태에 따라 결제 완료 후 결과가 게임에 전달되지 않는 문제가 수정되었습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.76.0](./release-notes-android/#2760-2025-11-28)
* [Gamebase iOS SDK 2.75.0](./release-notes-ios/#2750-2025-09-23)

### 2.75.0 (2025. 11. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경

* (Windows) 이미지 공지, 게임 공지 출력 시 고정 크기에서 화면 해상도 비율로 출력되도록 수정되었습니다.
* 내부 로직을 개선했습니다.

#### 버그 수정

* (Windows) 이미지 공지, 게임 공지 클릭 시 엔진 UI 포커스 문제로 클릭을 여러번 해야 반영되는 문제가 수정되었습니다.
* (Windows) 결제 완료 시 지표 전송에 SetGameUserData API 호출 정보가 포함되도록 수정되었습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.75.1](./release-notes-android/#2751-2025-10-17)
* [Gamebase iOS SDK 2.75.0](./release-notes-ios/#2750-2025-09-23)

### 2.74.0 (2025. 08. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.74.0/GamebaseSDK-Unreal.zip)

#### 기능 추가

* (Windows) 계정 매핑 기능이 추가되었습니다.

#### 기능 개선/변경

* (Windows) 게임 공지 출력 시 엔진의 DPI에 영향을 받지 않도록 수정되었습니다.
* 내부 로직을 개선했습니다.

#### 버그 수정

* (Windows) 계정 상태가 변경되었을 때 간헐적으로 크래시가 발생되는 로직이 수정되었습니다.
* (Windows) Twitter 로그인 시 간헐적으로 'Something went wrong' 오류가 발생하지 않도록 수정되었습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.73.1](./release-notes-android/#2731-2025-08-12)
* [Gamebase iOS SDK 2.73.1](./release-notes-ios/#2731-2025-08-12)

### 2.73.1 (2025. 07. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-Unreal.zip)

#### 기능 개선/변경

* (Android) Amazon Appstore가 서비스 중단되어 스토어 설정 및 푸시 설정 기능이 제거되었습니다.
* (Windows) 로그 전송 시 재시도 로직이 개선되었습니다.
* 내부 로직을 개선했습니다.

#### 버그 수정

* 컴파일러 환경에 따라 빌드 오류가 발생하는 로직이 수정되었습니다.
* (Windows) 로그 전송 시 특정 문자가 포함된 데이터를 전송할 때 발생하던 오류가 수정되었습니다.

#### 플랫폼별 변경 사항

* [Gamebase Android SDK 2.73.0](./release-notes-android/#2730-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2730-2025-07-15)

### 2.73.0 (2025. 07. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Unreal.zip)

#### Feature Updates

* (Windows) For IdPs that do not use SDKs, the process has been changed to proceed with external browser login.
    * An API has been added to cancel login when an external browser login is in progress.
        * CancelLoginWithExternalBrowser
        * Please refer to the following guide document for how to call the API.
            * [Game > Gamebase > Unreal SDK User Guide > Authentication> Login > Login with IdP > Cancel Login with External Browser](./unreal-authentication/#cancel-login-with-external-browser)
* (Windows) Added a message when logging into Steam to indicate Steamworks initialization failure to help identify the cause.
* Improved internal logic.

#### Bug Fixes

* Fixed EOSSDK module not being included when not using Epic Games-related features.
* (Windows) Fixed a crash when using an unconfigured store in the console.

#### Platform-specific Changes

* [Gamebase Android SDK 2.73.0](./release-notes-android/#2730-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2730-2025-07-15)

### 2.72.0 (2025. 06. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Unreal.zip)

#### Added Features

* Added the EpicGames authentication.
* (Windows) Added a Customer Center link to the account suspension popup.

#### Feature Updates

* Improved internal logic.

#### Bug Fixes

* (Windows) Fixed to pass the response to the game thread when logging in from an external browser.
* (Windows) Fixed an issue where external browser login could fail on low-performance PCs.
* (Windows) Fixed a crash that could occur when device information could not be retrieved properly.

#### Platform-Specific Changes

* [Gamebase Android SDK 2.72.0](./release-notes-android/#2720-2025-06-24)
* [Gamebase iOS SDK 2.72.0](./release-notes-ios/#2720-2025-06-24)

### 2.71.1 (2025. 4. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Unreal.zip)

#### Feature Updates

* (Windows) Enhanced detailed error messages to aid in debugging when a payment API error occurs.
* Improved internal logic.

#### Bug Fixes

* (Windows) Fixed an issue where the maintenance language was incorrectly retrieved when applying DisplayLanguageCode in FGamebaseConfiguration.
* (Windows) Fixed an issue where re-authentication was not possible in certain failure cases during the authentication process.

#### Platform-Specific Changes

* [Gamebase Android SDK 2.71.1](./release-notes-android/#2711-2025-04-29)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2710-2025-04-15)

### 2.71.0 (2025. 4. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Unreal.zip)

#### Added Features

* Added a new feature: Game Notice
    * To learn how to call the API, see the following link.
        * [Game > Gamebase > Unreal SDK User Guide > UI > GameNotice](./unreal-ui/#gamenotice)
* (Windows) Added Google billing support for Google Play Games.
    * Added `Google Play Store` as an option in Windows Store settings within the [Windows Setting Tool](./unreal-started/#windows-settings).

#### Feature Updates

* (Windows) Modified to generate the CountryCode based on System Settings > Region > Country or Region.
    * Previously, the CountryCode was obtained using the engine’s `FInternationalization::Get().GetDefaultCulture()` method, which provides the Region > Regional Language information.

#### Bug Fixes

* (Windows) Fixed an issue where the application could crash when closing while a WebView was open.
* (Windows) Disabled Steam authentication and payment features in the editor since the Steamworks module included in the engine is not available there.
* (Windows) Fixed an issue where log delivery filtering did not work correctly.
* Improved internal logic.

#### Platform-Specific Changes

* [Gamebase Android SDK 2.71.0](./release-notes-android/#2710-2025-04-15)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2710-2025-04-15)

### 2.70.0 (2025. 3. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Unreal.zip)

#### Added Features

* Added a new error code to indicate that an error was received from the IdP server at sign-in.
    * AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* Added options to configure the navigation bar title color and icon tint color in WebView.
    * `FGamebaseWebViewConfiguration::NavigationBarTitleColor`
    * `FGamebaseWebViewConfiguration::NavigationBarIconTintColor`
* (Android) Added an initialization option for the GPGS Auto Login feature that prompts the user to log in to GPGS only once after installing the app.
    * `FGamebaseConfiguration::bEnableGPGSSignInCheck`
    * By default, this is set to true, which means the GPGS login prompt will appear again during Gamebase initialization even if the user previously declined.
    * If set to false, the GPGS login prompt is shown only once when the app is launched for the first time.

#### Feature Updates

* Improved internal logic.

#### Bug Fixes

* (Windows) Fixed a crash that could occur when receiving additional information via FGamebaseVariantMap during login.

#### Platform-Specific Changes

* [Gamebase Android SDK 2.70.0](./release-notes-android/#2700-2025-03-11)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2700-2025-03-11)

### 2.69.1 (2025. 3. 4.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.1/GamebaseSDK-Unreal.zip)

#### Added Features

* Added terms and conditions information to be available in the launch info.
    * FGamebaseLaunchingInfo::FApp::FTermsService

#### Feature Updates

* Changed the API parameter type from `UGamebaseJsonObject` to `FGamebaseVariantMap(TMap<FName, FVariant>)`.
* Improved internal logic.

#### Bug Fixes

* (Windows) Fixed an issue where an error in the UUID issuance process during guest login was causing them all to generate the same value.
* (Windows) Fixed an issue where the region setting was not applied during LINE IDP login.
* (Windows) Fixed an issue where the ServerPushAppKickOut event was not triggered and the popup was not displayed when a kickout occurred.
* (Windows) Fixed an issue where an error occurred during symbol generation when the engine's Build Configuration was not set to Development.
* (Android) Fixed an issue where RegisterPush did not work correctly depending on the environment.

#### Platform-Specific Changes

* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.69.0 (2025. 2. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unreal.zip)

#### Added Features

* Added the **RequestLastLoggedInProvider asynchronous API**.
    * **GetLastLoggedInProvider() synchronous API** may sometimes fail to return the correct value due to timing issues.
    * (Android) When using the GPGS Auto Login feature, it takes time to retrieve data from the GPGS server. Therefore, calling the GetLastLoggedInProvider() synchronous API immediately after Gamebase initialization may not return a valid value.
        In this case, the asynchronous API RequestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) guarantees the correct value.
        If Auto Login is not used, it is safe to use the GetLastLoggedInProvider() synchronous API.
* (Android) Added GPGS v2 authentication.
    * For more information, see the link below.
        * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Android Settings](./unreal-started/#android-settings)
* (Android) Added the **FGamebaseWebViewConfiguration::CutoutColor field**.
    * When **FGamebaseWebViewConfiguration::bRenderOutSideSafeArea field** is set to **false**, padding is automatically added to the cutout area.
    * The CutoutColor field allows you to set the color of this added padding area.
    * If RenderOutsideSafeArea field is set to false but CutoutColor field is not specified, the padding area's color will be automatically determined by the web page body's background-color value.

#### Feature Updates

* Improved internal logic.

#### Bug Fixes

* Fixed the terms query result API, FGamebaseQueryTermsResult.
    * Fixed an issue where the value of TermsCountryType was not being set.
    * bPushEnabled, bAdAgreement, bAdAgreementNight has been removed.
* (Android) Fixed an issue to prevent errors from occurring in the post-build process when building in the Windows environment.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2690-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2690-2025-01-21)

### 2.68.1 (2025. 01. 21.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Improved internal logic.
* (Windows) Changed to allow the WebView plugin to be selected as an optional component.
    * For more information, see the link below.
        * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Windows Settings > WebView Plugins](./unreal-started/#windows-settings)
* (Windows) Added a feature to generate a compressed file containing symbol files in the project binary path when sending crash logs.
    * For more information, see the link below.
        * [Game > Gamebase > Unreal SDK User Guide > Logger > Crash Reporter](./unreal-logger/#crash-reporter)

#### Bug Fixes
* (Windows) Fixed an issue where a crash could occur during internal log transmission.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2680-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2681-2024-12-10)

### 2.68.0 (2024. 12. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Improved internal logic.
* (Windows) Twitter changed its authentication method to OAuth 2.0, so login will not work without changing the settings below.
    * Issue OAuth 2.0 Client ID and Client Secret
        * Create an OAuth 2.0 Client ID and Client Secret in the Twitter Developer Portal, then register them in the Gamebase console.
    * Set Callback URL
        * Set the Callback URL (https://id-gamebase.toast.com/oauth/callback) in the Gamebase console.
        * Add the same Callback URL to the Twitter Developer Portal.
    * For more information, see the link below.
        * [Game > Gamebase > Console User Guide > App > Authentication Information](./oper-app/#authentication-information)

#### Bug Fixes
* (Windows) Fixed so that crash would not occur when making payment.
* (Windows) Fixed an issue where, when exiting the payment with an ESC key on Steam payment, the following payment APIs would not work.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2680-2024-11-26)
* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2681-2024-12-10)

### 2.67.2 (2024. 11. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.2/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Improved internal logic.

#### Bug Fixes
* (Windows) Fixed an issue that prevented Apple ID sign-in from proceeding properly.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.67.1 (2024. 11. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.1/GamebaseSDK-Unreal.zip)

#### Feature Updates
* (Windows) Changed the Purchase settings to allow selecting only one store.
    * The store reset is required.
* (Windows) The process for registering a handle for the EOS SDK when using the Epic Games Store has changed.
    * When using Online Subsystem EOS, if the StoreCode corresponds to the Epic Games Store during Gamebase initialization, the handle is automatically registered.
    * If Online Subsystem EOS is not used, you need to manually register the EOS handle by following the [Windows Settings Guide](./unreal-started/#windows-settings).
* (Windows) Changed the supported version of the Steamworks SDK to 1.59.
    * For more information, see the following guide document.
        * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Windows Settings > Steamworks Service](./unreal-started/#windows-settings)

#### Bug Fixes
* Fixed header file references to work correctly.
* (Windows) Fixed an issue where multiple initialization attempts could cause a crash.
* (Windows) Fixed an issue where entering a StoreCode corresponding to Steam or Epic Games Store during initialization could cause a crash.
* (Windows) Fixed logic that could cause a crash during login attempts using an external browser.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.67.0 (2024. 10. 30.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added the Steam authentication.
* Added Steam payments.
* Added a new type to the image notice feature.
    * Added the rolling popup type.
    * Existing image notices appear as pop-ups and are not supported on Windows.
* (Windows) Added the LINE authentication.

#### Feature Updates
* Changed the supported version of the engine from 4.27 to 5.4.
* Improved internal logic.

#### Bug Fixes
* Fixed logic that could cause a crash when a crash log is generated.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2670-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2670-2024-10-29)

### 2.66.1 (2024. 09. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Improved internal logic.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.66.3](./release-notes-android/#2663-2024-09-10)
* [Gamebase iOS SDK 2.66.2](./release-notes-ios/#2662-2024-08-27)

### 2.66.0 (2024. 08. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unreal.zip)

#### Feature Updates
* Made changes to how to use APIs.
    * Changed the API from being provided by **IGamebase**, which inherits from `IModuleInterface`, to being provided by **UGamebaseSubsystem**, which inherits from `UGameInstanceSubsystem`.
    * As a subsystem of GameInstance, **UGamebaseSubsystem** follows the GameInstance lifecycle and you must find the subsystem through the GameInstance you use when calling the SDK APIs for use.
* Removed the GamebaseInterface module. When using the Gamebase plugin, delete the GamebaseInterface module before using it.
* (Windows) GameInstance is available in different individual environments.

#### Platform-Specific Changes
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

#### Added Features
* (Android) Changed the Firebase Notification setup to specify the google-services.json path via the [Android Setting Tool](./unreal-started/#android-settings) instead of editing the plugin’s internal file.
* (iOS) Applied Privacy Manifest and code signing to the Gamebase Unreal SDK.

#### Feature Updates
* (iOS) Fixed to prevent errors during build.

#### Platform-Specific Changes
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2630-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2630-2024-04-23)

### 2.62.0 (2024. 03. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unreal.zip)

#### Added Features
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
    * **FGamebaseConfiguration.bEnableKickoutPopup**
* Default values have been added to some fields in FGamebaseConfiguration.
    * The default value of bEnableLaunchingStatusPopup is set to true.
    * The default value of bEnableBanPopup is set to true.
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