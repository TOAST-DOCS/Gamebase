## Game > Gamebase > Release Notes > Unreal

### 2.56.0 (2023. 10. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [Android 설정 툴](./unreal-started/#android-settings)에서 스토어 설정이 추가되었습니다.
    * Amazone Appstore, Huawei AppGallery, MyCard 선택이 추가되었습니다.
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

### 버그 수정
* 로그인 후 탈퇴 유예 정보 및 결제 어뷰징 자동 해제 정보가 정상으로 전달되도록 수정되었습니다.

#### 플랫폼별 변경 사항
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
    * IGamebase::Get().GetPush().QueryNotificationAllowed()
* Added an API to determine whether terms and conditions have been displayed.
    * IGamebase::Get().GetTerms().IsShowingTermsView()
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