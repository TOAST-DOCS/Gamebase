<!-- machine_translated: true -->

<a id="game-gamebase-release-notes-ios"></a>

## Game > Gamebase > Release Notes > iOS { #game-gamebase-release-notes-ios }

<a id="820-2026-07-28"></a>

### 2.82.0 (2026. 07. 28.) { #820-2026-07-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.82.0/GamebaseSDK-iOS.zip)

<a id="820-2026-07-28-feature-updates"></a>
#### Feature Updates
* Fixed an issue where the Facebook SDK was not initialized when Gamebase was initialized.

<a id="813-2026-05-27"></a>

### 2.81.3 (2026. 05. 27.) { #813-2026-05-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.3/GamebaseSDK-iOS.zip)

<a id="813-2026-05-27-feature-updates"></a>
#### Feature Updates
* Modified to return a **TCGB_ERROR_NOT_SUPPORTED(10)** error when a Push API is called without GamebasePushAdapter included in the build.
* Improved internal logic

<a id="813-2026-05-27-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where some videos did not play in the Gamebase web view.

<a id="812-2026-04-28"></a>

### 2.81.2 (2026. 04. 28.) { #812-2026-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.2/GamebaseSDK-iOS.zip)

<a id="812-2026-04-28-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the callback was not received when Gamebase was initialized immediately after launch while the app supported SceneDelegate.

<a id="811-2026-03-30"></a>

### 2.81.1 (2026. 03. 30.) { #811-2026-03-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.1/GamebaseSDK-iOS.zip)

<a id="811-2026-03-30-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the callback was not received when attempting addMapping with LINE.

<a id="800-2026-02-13"></a>

### 2.80.0 (2026. 02. 13.) { #800-2026-02-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-iOS.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### Feature Updates
* Updated the minimum supported version of Xcode to 26.0.
* Returns the **PURCHASE_PENDING (4008)** error when a payment is delayed due to "Ask to Buy" or other pending transaction scenarios.
* Expanded the functionality of the kTCGBPurchaseUpdated event in GamebaseEventHandler.
    * You can now receive events for completed App Store promotion purchases or finalized "Ask to Buy" transactions.
* Improved internal logic
* The API is deprecated.
    * **+[TCGBPurchase setPromotionIAPHandler:]**

<a id="790-2026-01-27"></a>

### 2.79.0 (2026. 01. 27.) { #790-2026-01-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-iOS.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### Feature Updates
* Improved internal logic
* The APIs below is deprecated:
    * **+[TCGBConfiguration setStoreCode:]**
    * **-[TCGBPurchase setStoreCode:]**
    * **TCGBPurchase.storeCode**

<a id="770-2025-12-09"></a>

### 2.77.0 (2025. 12. 09.) { #770-2025-12-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.77.0/GamebaseSDK-iOS.zip)

<a id="770-2025-12-09-feature-updates"></a>
#### Feature Updates
* Improved internal payment logic
* The API below is deprecated:
    * **+[TCGBPurchase requestItemListAtIAPConsoleWithCompletion:]**
    
<a id="750-2025-09-23"></a>

### 2.75.0 (2025. 09. 23.) { #750-2025-09-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-iOS.zip)

<a id="750-2025-09-23-feature-updates"></a>
#### Feature Updates
* Updated external SDK
    * Kakaogame iOS SDK (3.20.0)
    
<a id="731-2025-08-12"></a>

### 2.73.1 (2025. 08. 12.) { #731-2025-08-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-iOS.zip)

<a id="731-2025-08-12-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Facebook iOS SDK (18.0.0)

<a id="731-2025-08-12-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue that caused an error when signing in Twitter.

<a id="730-2025-07-15"></a>

### 2.73.0 (2025. 07. 15.) { #730-2025-07-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-iOS.zip)

<a id="730-2025-07-15-feature-updates"></a>
#### Feature Updates
* The minimum supported version of Xcode has been changed to 16.0. 

<a id="730-2025-07-15-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the agreed terms and conditions information was not saved when calling updateTerms after logging in.

<a id="721-july-1-2025"></a>

### 2.72.1 (2025. 07. 01.) { #721-july-1-2025 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.1/GamebaseSDK-iOS.zip)

<a id="721-july-1-2025-feature-updates"></a>
#### Feature Updates
* Fixed a bug that caused GameCenter to crash when logging in on certain iOS 14 devices.

<a id="720-2025-06-24"></a>

### 2.72.0 (2025. 06. 24.) { #720-2025-06-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-iOS.zip)

<a id="720-2025-06-24-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.17.2)
        * Improved internal logic
    * LINE iOS SDK (5.11.2)
        * Removed bitcode settings
        * Fixed Xcode 16 compiler warnings
* Improved internal logic

<a id="720-2025-06-24-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where additional region information from LINE IdP was not applied, causing problems in mapping and loginForLastLoggedInProvider login operations.

<a id="710-2025-04-15"></a>

### 2.71.0 (2025. 04. 15.) { #710-2025-04-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-iOS.zip)

<a id="710-2025-04-15-added-features"></a>
#### Added Features
* Added a new feature: Game Notice
    * To learn how to call the API, see the following link.
        * [Game > Gamebase > iOS SDK User Guide > UI > GameNotice > Open GameNotice](./ios-ui/#open-gamenotice)

<a id="710-2025-04-15-feature-updates"></a>
#### Feature Updates
* Changed behavior to return an **TCGB_ERROR_INVALID_PARAMETER(3)** error instead of throwing an exception when calling Gamebase initialization with storeCode set to nil.

<a id="700-2025-03-11"></a>

### 2.70.0 (2025. 03. 11.) { #700-2025-03-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-iOS.zip)

<a id="700-2025-03-11-feature-updates"></a>
#### Feature Updates
* Added a new error code to indicate that an error was received from the IdP server at sign-in.
    * TCGB_ERROR_AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* Added the option to set the navigation bar title color and icon color to the TCGBWebViewConfiguration.
    * **TCGBWebViewConfiguration.navigationBarTitleColor**
    * **TCGBWebViewConfiguration.navigationBarIconTintColor**

<a id="690-2025-01-21"></a>

### 2.69.0 (2025. 01. 21.) { #690-2025-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-iOS.zip)

<a id="690-2025-01-21-feature-updates"></a>
#### Feature Updates
* External SDK update
    * PAYCO iOS SDK (1.5.13)
        * Modified openURL-related functions to ensure proper operation of PAYCO simple login on iOS 18
    * Hangame iOS SDK (1.17.1)
        * Improved internal logic
    * Weibo iOS SDK (3.4.0)
        * iOS 18 optimizations
* Fixed completion block to run in the main thread.

<a id="690-2025-01-21-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the callback was not triggered when NAVER login was canceled in apps using SceneDelegate.
* Fixed a bug where LINE login would fail if the LINE old clientId was not configured in the Gamebase Console.

<a id="681-2024-12-10"></a>

### 2.68.1 (2024. 12. 10.) { #681-2024-12-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-iOS.zip)

<a id="681-2024-12-10-bug-fixes"></a>
#### Bug Fixes  
* Fixed an error that occurs when importing the Gamebase SDK from a Swift file.

<a id="680-2024-11-26"></a>

### 2.68.0 (2024. 11. 26.) { #680-2024-11-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-iOS.zip)

<a id="680-2024-11-26-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.8.5)
    * Hangame iOS SDK (1.17.0)
* Changed the Google sign-in method from OAuth 2.0 to OpenID Connect.
* Improved internal logic

<a id="670-2024-10-29"></a>

### 2.67.0 (2024. 10. 29.) { #670-2024-10-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-iOS.zip)

<a id="670-2024-10-29-added-features"></a>
#### Added Features
* Added Steam authentication.
* Twitter has changed its authentication method to OAuth 2.0, so login will not work without changing the settings below.
    * Issue OAuth 2.0 Client ID and Client Secret
        * Create an OAuth 2.0 Client ID and Client Secret in the Twitter Developer Portal, then register them in the Gamebase console.
    * Callback URL Settings
        * Set the Callback URL (https://id-gamebase.toast.com/oauth/callback) in the Gamebase console.
        * Add the same Callback URL to the Twitter Developer Portal.
    * For more information, see the following link.
        * [Game > Gamebase > Console User Guide > App > Authentication Information > 6. Twitter](./oper-app/#6-twitter)

<a id="670-2024-10-29-feature-updates"></a>
#### Feature Updates
* External SDK Update
    * PAYCO iOS SDK (1.5.12)
        * Changed the PAYCO SDK to Dynamic Framework.
    * NAVER iOS SDK (4.2.3)
        * Fixed to run correctly in Xcode 16 and iOS 18 environments.
    * Hangame iOS SDK (1.16.2)
        * Fixed a bug that caused login to fail on Apple Silicon Macs.
* Fixed the Gamebase SDK to not include resources from external SDKs.
* Improved internal logic

<a id="670-2024-10-29-bug-fixes"></a>
#### Bug Fixes  
* Fixed a bug where the screen would go black when the Gamebase launch popup window was displayed on top of the system popup window.

<a id="663-2024-09-13"></a>

### 2.66.3 (2024. 09. 13.) { #663-2024-09-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-iOS.zip)

<a id="663-2024-09-13-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.8.4)
        * Made modifications so that duplicate notifications are not received when the app is in the foreground state in iOS 18.
        
<a id="662-2024-08-27"></a>

### 2.66.2 (2024. 08. 27.) { #662-2024-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-iOS.zip)

<a id="662-2024-08-27-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.8.3)
        * Made modifications so that the app store review does not warn about PrivacyInfo manifest.
* Deprecated the following field.
    * **TCGBWebViewConfiguration.orientationMask**
* Made modifications so that `TCGB_ERROR_AUTH_IDP_LOGIN_INVALID_IDP_INFO (3202)` error occurs when attempting to log in with an IdP that is not registered in the console.
* Fixed a failure callback to be called instead of the previous success callback when an error occurs inside the webview of a rolling image announcement.
* Improved internal logic

<a id="660-2024-07-23"></a>

### 2.66.0 (2024. 07. 23.) { #660-2024-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-iOS.zip)

<a id="660-2024-07-23-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Facebook iOS SDK (17.0.2)
    * Hangame iOS SDK (1.15.0)
* Fixed to allow Hangame-Facebook login even if you don't allow app tracking.

<a id="651-2024-06-25"></a>

### 2.65.1 (2024. 06. 25.) { #651-2024-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-iOS.zip)

<a id="651-2024-06-25-feature-updates"></a>
#### Feature Updates
* Fixed so that if there are no images to show on a particular client, a success callback is called instead of an error.

<a id="651-2024-06-25-bug-fixes"></a>
#### Bug Fixes  
* Fixed an issue where an empty image notice is displayed if no registered image announcement is available.

<a id="650-2024-06-11"></a>

### 2.65.0 (2024. 06. 11.) { #650-2024-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-iOS.zip)

<a id="650-2024-06-11-added-features"></a>
#### Added Features
* Added a new type to the image notice feature.
    * Added the `rolling popup` type.
    * Displays the existing image notice as the `individual popup` type.

<a id="650-2024-06-11-feature-updates"></a>
#### Feature Updates
* Fixed to allow Facebook login even if you don't allow app tracking.
* External SDK update
    * Facebook iOS SDK (17.0.1)
        * Changed Facebook SDK to Dynamic Framework.
* Modified the PrivacyInfo.xcprivacy file of Weibo iOS SDK.
* Improved internal logic

<a id="640-2024-05-28"></a>

### 2.64.0 (2024. 05. 28.) { #640-2024-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-iOS.zip)

<a id="640-2024-05-28-feature-updates"></a>
#### Feature Updates
* External SDK update
    * PAYCO iOS SDK (1.5.11)
    * Kakaogame iOS SDK (3.19.0)
* Made modifications so that Gamebase's displayLanguageCode is used when TCGBPushConfiguration.displayLanguageCode is set to an empty string.
* Improved internal logic

<a id="631-2024-05-14"></a>

### 2.63.1 (2024. 05. 14.) { #631-2024-05-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.1/GamebaseSDK-iOS.zip)

<a id="631-2024-05-14-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.13.1)
* Improved internal logic

<a id="630-2024-04-23"></a>

### 2.63.0 (2024. 04. 23.) { #630-2024-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-iOS.zip)

<a id="630-2024-04-23-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Google iOS SDK (7.1.0)
    * Facebook iOS SDK (17.0.0)
    * Weibo iOS SDK (3.3.8)
* Improved internal logic

<a id="620-2024-03-26"></a>

### 2.62.0 (2024. 03. 26.) { #620-2024-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-iOS.zip)

<a id="620-2024-03-26-added-features"></a>
#### Added Features
* Applied Privacy manifest and signature to Gamebase and Gamebase Adapter SDK.
* Added the testDevice field to indicate a test device in LaunchingInfo VO that returns after Gamebase is initialized.
    * **launchingInfo.user.testDevice**

<a id="620-2024-03-26-feature-updates"></a>
#### Feature Updates
* Raised the minimum supported version of Xcode to 15.0. 
* Raised the minimum supported version of iOS to 12.0.
* External SDK update
    * NHN Cloud iOS SDK (1.8.1)
    * LINE iOS SDK (5.11.0)
        * Mininum supported version of LINE changed to 13.0.
    * NAVER iOS SDK (4.2.1)
    * PAYCO iOS SDK (1.5.10)
    * Hangame iOS SDK (1.12.0)
* Improved internal logic

<a id="610-2024-02-27"></a>

### 2.61.0 (2024. 02. 27.) { #610-2024-02-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-iOS.zip)

<a id="610-2024-02-27-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.8.0)
* Improved SDK internal logic

<a id="601-2024-02-15"></a>

### 2.60.1 (2024. 02. 15.) { #601-2024-02-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.1/GamebaseSDK-iOS.zip)

<a id="601-2024-02-15-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the account changes to a GameCenter account even after logging in with a specific Idp.

<a id="600-2024-01-23"></a>

### 2.60.0 (2024. 01. 23.) { #600-2024-01-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-iOS.zip)

<a id="600-2024-01-23-feature-updates"></a>
#### Feature Updates
* Improved SDK internal logic

<a id="591-2023-12-27"></a>

### 2.59.1 (2023. 12. 27.) { #591-2023-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.1/GamebaseSDK-iOS.zip)

<a id="591-2023-12-27-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where an error occurs when logging into Hangame.

<a id="590-2023-12-19"></a>

### 2.59.0 (2023. 12. 19.) { #590-2023-12-19 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-iOS.zip)

<a id="590-2023-12-19-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NAVER iOS SDK (4.2.0)
        * Changed NAVER iOS SDK to xcframework.
* Modified the Terms and Conditions window to display at a fixed size in tablet environments.
* Modified to display an error popup window when the Launching Status Code is INTERNAL_SERVER_ERROR(500).

<a id="590-2023-12-19-bug-fixes"></a>
#### Bug Fixes
* Fixed crash occurrence when calling LINE Login repetitively.

<a id="580-2023-11-28"></a>

### 2.58.0 (2023. 11. 28.) { #580-2023-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-iOS.zip)

<a id="580-2023-11-28-feature-updates"></a>
#### Feature Updates
* External SDK update
    * PAYCO iOS SDK (1.5.9)
        * PAYCO iOS SDK가 xcframework로 변경되었습니다.
    * Kakaogame iOS SDK (3.17.5)
* Improved the logic to get the top most ViewController
* Modified the initialization callback to be called after the Gamebase launch popup window has completely exited.

<a id="570-2023-10-31"></a>

### 2.57.0 (2023. 10. 31.) { #570-2023-10-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-iOS.zip)

<a id="570-2023-10-31-feature-updates"></a>
#### Feature Updates
* Added the Privacy manifest file.
* Changed Gamebase GameCenter login specifications.
    * After canceling and re-requesting a GameCenter login, an error pop-up window would appear with the error TCGB_ERROR_AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED (3203).

<a id="552-2023-09-26"></a>

### 2.55.2 (2023. 09. 26.) { #552-2023-09-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.2/GamebaseSDK-iOS.zip)

<a id="552-2023-09-26-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Weibo iOS SDK (3.3.4)

<a id="552-2023-09-26-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where, when trying to log in to Weibo after first installing the app, callback does not work properly.

<a id="550-2023-09-12"></a>

### 2.55.0 (2023. 09. 12.) { #550-2023-09-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-iOS.zip)

<a id="550-2023-09-12-added-features"></a>
#### Added Features
* Added the TCGBPushConfiguration.alwaysAllowTokenRegistration field to allow users to register tokens even if they deny push permissions

<a id="550-2023-09-12-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.6.2)

<a id="540-2023-08-29"></a>

### 2.54.0 (2023. 08. 29.) { #540-2023-08-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.54.0/GamebaseSDK-iOS.zip)

<a id="540-2023-08-29-feature-updates"></a>
#### Feature Updates
* Changed SDK to xcframework
* External SDK update
    * Facebook iOS SDK (14.1.0)
    * Google iOS SDK (7.0.0)
* Improved SDK internal logic

<a id="530-2023-07-25"></a>

### 2.53.0 (2023. 07. 25.) { #530-2023-07-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.53.0/GamebaseSDK-iOS.zip)

<a id="530-2023-07-25-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.8.6)
* Deprecated fields are as follows.
    * **TCGBWebViewConfiguration.backgroundOpacity**
* Modified to center the ActionSheet on the screen when calling the [TCGBUtil showActionSheetWithTitle:message:blocks:] API on iPad.
* Modified to return the **TCGB_ERROR_AUTH_NOT_SUPPORTED_PROVIDER(3002)** error when using an authentication adapter that is not added to the project.

<a id="530-2023-07-25-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug that prevented the suspension pop-up window from displaying in certain situations.
* Fixed a bug where the webview is not normally displayed on Apple Silicon Mac.

<a id="520-2023-06-27"></a>

### 2.52.0 (2023. 06. 27.) { #520-2023-06-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-iOS.zip)

<a id="520-2023-06-27-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.4.0)
    * Weibo iOS SDK (3.3.3)
* Deprecated APIs as follows.
    * **+[TCGBGamebase countryCode]**
    * **+[TCGBGamebase countryCodeOfUSIM]**
    * **+[TCGBGamebase carrierCode]**
    * **+[TCGBGamebase carrierName]**
    * **+[TCGBUtil countryCode]**
    * **+[TCGBUtil usimCountryCode]**
    * **+[TCGBUtil carrierCode]**
    * **+[TCGBUtil carrierName]**
* Improved SDK internal logic

<a id="510-2023-05-30"></a>

### 2.51.0 (2023. 05. 30.) { #510-2023-05-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.51.0/GamebaseSDK-iOS.zip)

<a id="510-2023-05-30-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.3.1)
    * PAYCO iOS SDK (1.5.8)
* Improved SDK internal logic

<a id="492-2023-04-28"></a>

### 2.49.2 (2023. 04. 28.) { #492-2023-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.2/GamebaseSDK-iOS.zip)

<a id="492-2023-04-28-bug-fixes"></a>
#### Bug Fixes
* Redeployed due to missing changes
    * Fixed a bug where authentication information from an externally authenticated IdP could not be obtained after logging in.

<a id="491-2023-04-25"></a>

### 2.49.1 (2023. 04. 25.) { #491-2023-04-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-iOS.zip)

<a id="491-2023-04-25-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where authentication information from an externally authenticated IdP could not be obtained after logging in.

<a id="490-2023-04-11"></a>

### 2.49.0 (2023. 04. 11.) { #490-2023-04-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-iOS.zip)

<a id="490-2023-04-11-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.8.5)

<a id="480-2023-03-28"></a>

### 2.48.0 (2023. 03. 28.) { #480-2023-03-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-iOS.zip)

<a id="480-2023-03-28-feature-updates"></a>
#### Feature Updates
* Raised the minimum supported version of Xcode to 14.1. 
* Raised the minimum supported version for iOS to 11.0.
* Removed support for armv7, armv7s, i386 architectures.
* Removed support for bitcode.
* External SDK update
    * NHN Cloud iOS SDK (1.3.0)
    * PAYCO iOS SDK (1.5.6)
* Applied the standby domain for Gamebase server in preparation for DNS failure

<a id="480-2023-03-28-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the kickout event would not come through in certain situations.
* Fixed a bug where the webview custom scheme callback is not called.

<a id="470-2023-02-14"></a>

### 2.47.0 (2023. 02. 14.) { #470-2023-02-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.47.0/GamebaseSDK-iOS.zip)

<a id="470-2023-02-14-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.8.4)
    
<a id="460-2023-01-31"></a>

### 2.46.0 (2023. 01. 31.) { #460-2023-01-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-iOS.zip)

<a id="460-2023-01-31-feature-updates"></a>
#### Feature Updates
* External SDK update
    * Hangame iOS SDK (1.8.2)
    * Kakaogame iOS SDK (3.14.14)
* Improved SDK internal logic

<a id="450-2022-12-27"></a>

### 2.45.0 (2022. 12. 27.) { #450-2022-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-iOS.zip)

<a id="450-2022-12-27-added-features"></a>
#### Added Features
* Added the following field to let you know which store the payment receipt is for
    * **TCGBPurchasableReceipt.storeCode**
* Added **TCGBPurchasableConfiguration** VO to allow additional configuration when calling payment API.
    * [Game > Gamebase > iOS SDK User Guide > Payment > TCGBPurchasableConfiguration](./ios-purchase/#tcgbpurchasableconfiguration)
* Added a new Query Unconsumed Purcahses API that receives  a parameter as **TCGBPurchasableConfiguration**.
    * **[TCGBPurchase requestItemListOfNotConsumedWithConfiguration:completion:]**
* Added a new Query Activated Subscription API that receives **TCGBPurchasableConfiguration** as a parameter.
    * **[TCGBPurchase requestActivatedPurchasesWithConfiguration:completion:]**

<a id="450-2022-12-27-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.2.0)
    * Hangame iOS SDK (1.8.0)
* Deprecated APIs as follows
    * **+[TCGBPurchase requestItemListOfNotConsumedWithCompletion:]**
    * **+[TCGBPurchase requestActivatedPurchasesWithCompletion:]**
* Improved SDK internal logic

<a id="440-2022-10-25"></a>

### 2.44.0 (2022. 10. 25.) { #440-2022-10-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-iOS.zip)

<a id="440-2022-10-25-feature-updates"></a>
#### Feature Updates
* Changed dependency for LINE iOS SDK

<a id="433-2022-10-04"></a>

### 2.43.3 (2022. 10. 04.) { #433-2022-10-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-iOS.zip)

<a id="433-2022-10-04-feature-updates"></a>
#### Feature Updates
* Improved the SDK internal logic

<a id="432-2022-09-22"></a>

### 2.43.2 (2022. 09. 22.) { #432-2022-09-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.2/GamebaseSDK-iOS.zip)

<a id="432-2022-09-22-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug that occurred when logging in to Game Center.

<a id="431-2022-09-14"></a>

### 2.43.1 (2022. 09. 14.) { #431-2022-09-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.1/GamebaseSDK-iOS.zip)

<a id="431-2022-09-14-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the LINE Auth Adapter deployed through CocoaPods cannot set a region due to a LINE SDK dependency error.

<a id="430-2022-09-07"></a>

### 2.43.0 (2022. 09. 07.) { #430-2022-09-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-iOS.zip)

<a id="430-2022-09-07-feature-updates"></a>
#### Feature Updates
* External SDK update
    * NHN Cloud iOS SDK (1.0.0)
    * ToastGamebaseIAP iOS SDK (0.14.0)
    * LINE iOS SDK (5.8.2)
    * Kakaogame iOS SDK (3.14.4)
    * Hangame iOS SDK (1.7.1)
* Modified to enter a service region when logging in to LINE.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > Login with IdP](./ios-authentication/#login-with-idp)
* Modified to return an error during initialization when using the Gamebase Adapter that does not guarantee compatibility with Gamebase.
* Improved the SDK internal logic

<a id="422-2022-08-24"></a>

### 2.42.2 (2022. 08. 24.) { #422-2022-08-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.2/GamebaseSDK-iOS.zip)

<a id="422-2022-08-24-feature-updates"></a>
#### Feature Updates
* Removed "itms-services" out of the scheme list used in WebView because it was rejected by App Review.

<a id="421-2022-08-09"></a>

### 2.42.1 (2022. 08. 09.) { #421-2022-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-iOS.zip)

<a id="421-2022-08-09-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the Gamebase popup window is not displayed normally when the Increase Contrast option is enabled.
* Fixed a bug where the Gamebase popup window is not displayed in a project using SceneDelegate.

<a id="420-2022-07-26"></a>

### 2.42.0 (2022. 07. 26.) { #420-2022-07-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-iOS.zip)

<a id="420-2022-07-26-added-features"></a>
#### Added Features
* Added a field to the ForcingMappingTicket VO class that is returned when mapping fails so that the user's current status can be identified.
    * **TCGBForcingMappingTicket.mappedUserValid**
    * For what the value stored in mappedUserValid means, refer to the following.
        * [Game > Gamebase > API Guide > API v1.3 Guide > Others > Mamber Vaild Code](./api-guide/#member-valid-code)

<a id="420-2022-07-26-feature-updates"></a>
#### Feature Updates
* External SDK update: Hangame iOS SDK (1.7.0)

<a id="420-2022-07-26-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where callback is not called when initializing Gamebase with an incorrect AppID.
* Fixed a bug where the **kTCGBIdPRevoked** event of GamebaseEventHandle does not occur for Hangame login users.

<a id="411-2022-07-20"></a>

### 2.41.1 (2022. 07. 20.) { #411-2022-07-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-iOS.zip)

<a id="411-2022-07-20-feature-updates"></a>
#### Feature Updates
* Modified to call callback after the terms and condition window is completely closed.

<a id="410-2022-07-05"></a>

### 2.41.0 (2022. 07. 05.) { #410-2022-07-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-iOS.zip)

<a id="410-2022-07-05-added-features"></a>
#### Added Features
* Added the **kTCGBIdPRevoked** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./ios-etc/#idp-revoked)

<a id="410-2022-07-05-feature-updates"></a>
#### Feature Updates
* Changed to turn image notices depending on the screen direction when they are displayed.

<a id="400-2022-05-24"></a>

### 2.40.0 (2022. 05. 24.) { #400-2022-05-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-iOS.zip)

<a id="400-2022-05-24-feature-updates"></a>
#### Feature Updates
* Improved the internal logic of SDK

<a id="390-2022-05-10"></a>

### 2.39.0 (2022. 05. 10.) { #390-2022-05-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-iOS.zip)

<a id="390-2022-05-10-feature-updates"></a>
#### Feature Updates
* External SDK update: Hangame iOS SDK (1.6.4)

<a id="380-2022-05-03"></a>

### 2.38.0 (2022. 05. 03.) { #380-2022-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-iOS.zip)

<a id="380-2022-05-03-feature-updates"></a>
#### Feature Updates
* Fixed unnatural sentences in the Traditional Chinese (zh-TW) language set of Display Language.

<a id="370-2022-04-26"></a>

### 2.37.0 (2022. 04. 26.) { #370-2022-04-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-iOS.zip)

<a id="370-2022-04-26-added-features"></a>
#### Added Features
* Added the following field so that you can add parameters after the contact center URL.
    * **TCGBContactConfiguration.additionalParameters**

<a id="360-2022-04-12"></a>

### 2.36.0 (2022. 04. 12.) { #360-2022-04-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-iOS.zip)

<a id="360-2022-04-12-added-features"></a>
#### Added Features
* Added the following fields to determine whether the payment is a sandbox payment or a promotion payment in the payment receipt.
    * **TCGBPurchasableReceipt.sandboxPayment**
    * **TCGBPurchasableReceipt.promotionPayment**

<a id="360-2022-04-12-feature-updates"></a>
#### Feature Updates
* External SDK update: TOAST iOS SDK(0.30.0), ToastGamebaseIAP SDK(0.13.0), Hangame iOS SDK (1.6.3)

<a id="350-2022-03-29"></a>

### 2.35.0 (2022. 03. 29.) { #350-2022-03-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

<a id="350-2022-03-29-added-features"></a>
#### Added Features
* Added an API to determine whether the terms and conditions window is currently displayed or not.
    * **[TCGBTerms isShowingTermsView]**

<a id="350-2022-03-29-feature-updates"></a>
#### Feature Updates
* Changed the login method from the Google web login method to the Google SDK login method.
* Modified to return an error **TCGB_ERROR_AUTH_USER_CANCELED(3001)** when Hangame login is canceled during the process.

<a id="341-2022-03-15"></a>

### 2.34.1 (2022. 03. 15.) { #341-2022-03-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

<a id="341-2022-03-15-added-features"></a>
#### Added Features
* Added the NS_SWIFT_NAME setting to Public API for Swift project users.

<a id="341-2022-03-15-feature-updates"></a>
#### Feature Updates
* External SDK update: Hangame iOS SDK (1.6.2)
* Fixed an error where, when the showWebView API is called while the device is in landscape mode, a black blank space is displayed at the bottom.

<a id="340-2022-02-22"></a>

### 2.34.0 (2022. 02. 22.) { #340-2022-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

<a id="340-2022-02-22-added-features"></a>
#### Added Features
* If you select **Add Popup Button** in the Update Required settings of the Gamebase console, a **Details** button will be added to the client's Update Required popup window.
* Added an API to find out whether the device has allowed notifications or not.
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* Added a VO class that can be used to find out whether the terms and conditions UI was displayed after calling the common terms and conditions API.
    * **TCGBShowTermsViewResult**

<a id="340-2022-02-22-feature-updates"></a>
#### Feature Updates
* Corrected the issue where the background becomes dark briefly when the Image Notice API has been called but there is no image notice to display
* The following fields have been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **-[TCGBConfiguration enableKickoutPopup:]**
    * **-[TCGBConfiguration isEnableKickoutPopup]**

<a id="330-20220125"></a>

### 2.33.0 (2022.01.25) { #330-20220125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

<a id="330-20220125-added-features"></a>
#### Added Features
* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > iOS SDK User Guide > UI > Terms > showTermsView](./ios-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### Feature Updates
* External SDK update: PAYCO iOS SDK (1.5.5)
* Added and changed error codes
    * Changed the error code mapped to the TCGB_ERROR_UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the TCGB_ERROR_SOCKET_UNKNOWN_ERROR error mapped to the error code 999.

<a id="321-20220111"></a>

### 2.32.1 (2022.01.11) { #321-20220111 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)

<a id="321-20220111-feature-updates"></a>
#### Feature Updates
* Modified so that, when clicking the **Update Now** button in the Update recommended pop-up window, the pop-up window is not closed.
* Improved the stability of SDK.

<a id="320-20211228"></a>

### 2.32.0 (2021.12.28) { #320-20211228 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

<a id="320-20211228-added-features"></a>
#### Added Features
* Added the **kTCGBServerPushAppKickoutMessageReceived** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* Added the **kTCGBLoggedOut** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

<a id="320-20211228-feature-updates"></a>
#### Feature Updates
* Changed the default title color of webview navigationBar to **UIColor.whiteColor**.

<a id="320-20211228-bug-fixes"></a>
#### Bug Fixes
* Fixed so that, when calling Hangame logout, thirdIdP is also logged out.

<a id="310-20211214"></a>

### 2.31.0 (2021.12.14) { #310-20211214 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

<a id="310-20211214-added-features"></a>
#### Added Features
* Added a feature in the maintenance pop-up to dynamically set whether to display the maintenance time.

<a id="310-20211214-feature-updates"></a>
#### Feature Updates
* External SDK update : TOAST iOS SDK (0.29.2), PAYCO iOS SDK (1.5.4)
* Fixed an issue where it was not possible to register inquiries with banned user information from the Customer Center link in the ban webview.
* Modified so that the Back button is displayed in the maintenance pop-up and ban details webview.

<a id="301-20211125"></a>

### 2.30.1 (2021.11.25) { #301-20211125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

<a id="301-20211125-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where errors occur in purchase and push APIs when Cocoapods has been installed in Unity 2019.3 or higher.

<a id="300-20211123"></a>

### 2.30.0 (2021.11.23) { #300-20211123 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

<a id="300-20211123-added-features"></a>
#### Added Features
* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* Added an API that allows you to change an account to the corresponding IdP when a **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** error occurs after trying to map to a specific IdP.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > Change Login with ForcingMappingTicket](./ios-authentication/#change-login-with-forcingmappingticket)

<a id="300-20211123-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the logout or withdrawal function does not work for a specific IdP after logging in with loginForLastLoggedInProvider.

<a id="290-20211109"></a>

### 2.29.0 (2021.11.09) { #290-20211109 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

<a id="290-20211109-feature-updates"></a>
#### Feature Updates
* The minimum supported version of Xcode has been changed from 12 to 13.
* External SDK update: TOAST iOS SDK (0.29.1), ToastGamebaseIAP SDK (0.12.1)
* Changed to display the URL of the detailed view of maintenance and notice registered in the console without encoding.

<a id="290-20211109-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug that caused an error when parsing TCGBPushMessage.extras as JSON.

<a id="280-20210928"></a>

### 2.28.0 (2021.09.28) { #280-20210928 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

<a id="280-20210928-added-features"></a>
#### Added Features
* Added Kakaogame authentication
* Added a 'purchase abuse automatic release' function.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > GraceBan](./ios-authentication/#graceban)
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always check the value of TCGBAuthToken.tcgbMember.graceBanInfo after login. If a valid TCGBGraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

<a id="280-20210928-feature-updates"></a>
#### Feature Updates
* PAYCO iOS SDK update (1.5.2)

<a id="271-20210914"></a>

### 2.27.1 (2021.09.14) { #271-20210914 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

<a id="271-20210914-feature-updates"></a>
#### Feature Updates
* PAYCO iOS SDK update (1.5.1)
     * Improved authentication flow and UI.
* Hangame iOS SDK update (1.6.1)
     * Fixed an issue where callback could not be called when an error situation occurred in personal verification.
     * Fixed an issue where the navigation bar appears broken in iOS 15 beta.

<a id="271-20210914-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where PushConfiguration is not returned as nil when the terms and conditions UI is not displayed because the user have already agreed to the terms.

<a id="270-20210824"></a>

### 2.27.0 (2021.08.24) { #270-20210824 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

<a id="270-20210824-feature-updates"></a>
#### Feature Updates
* Updated PAYCO iOS SDK (1.5.0)
    * So far, only manual login was supported when the PAYCO app is not available. It has been changed so that the quick login feature can be used when the user is logged-in on Safari.

<a id="270-20210824-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where image notices were not displayed in Unity.
    * If you are using a version lower than Gamebase iOS SDK 2.27.0, image notices may not be displayed in Unity.
    * When using image notices, use Gamebase iOS SDK 2.27.0 or higher.


<a id="260-20210810"></a>

### 2.26.0 (2021.08.10) { #260-20210810 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

<a id="260-20210810-feature-updates"></a>
#### Feature Updates
* Improved the Display Language feature.
    * Until now, you had to manually edit the Gamebase.bundle file to add the language set.
        * It has been improved so that you can add a localizedstring.json file to Copy Bundle Resources of Xcode project.
    * Simplified Chinese (zh-CN), Traditional Chinese (zh-TW), and Thai (th) have been added to the Display Language language set.
    * The default language code was **en**, but it has been improved to reflect the default language set in the Gamebase console.
        * [Game > Gamebase > Console User Guide > App > App > Language settings](./oper-app/#language-settings)
* Changed the creation criteria of the PushConfiguration object that can be created after calling the showTermsView API as follows.
    * Before change
        * A valid non-nil PushConfiguration was returned only when **Receive Push Notification** item exists in the terms and conditions.
        * PushConfiguration.pushEnabled was created as false when the user declines to receive both daytime and nighttime promotional push notifications.
    * After change
        * A valid non-nil PushConfiguration is always returned if the terms and conditions UI was displayed.
        * The pushEnabled value of the PushConfiguration object returned by showTermsView is always true.
    * Same point without change
        * PushConfiguration is returned as nil if the user has already agreed to the terms and conditions and the terms and conditions UI was not displayed.

<a id="260-20210810-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the language code of the message sent from the Push console does not match because the language code of the device is applied to the Push notification language setting without any extra processing.

<a id="250-20210727"></a>

### 2.25.0 (2021.07.27) { #250-20210727 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-iOS.zip)

<a id="250-20210727-more-features"></a>
#### More Features
* Added monthly payment limit feature.
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

<a id="250-20210727-feature-updates"></a>
#### Feature Updates
* Guaranteed the PushConfiguration object in the terms and conditions with Push notification items.
    * The TCGBPushConfiguration to be created as the result of calling Gamebase.Terms.showTermsView API was null if user did not agreed to receive push notifications in the terms of use. It has now changed so that the TCGBPushConfiguration object is always returned if there is a push notification item in the terms and conditions.
    * When user rejects receiving push notifications, the TCGBPushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * The TCGBPushConfiguration is null when there is no Push notification item in the terms and conditions.
* External SDK Update: TOAST iOS SDK(0.29.0)
* Changed to return TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR error when an ASAuthorizationErrorUnknown error occurs in Sign In with an Apple OS.

<a id="250-20210727-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the value of TCGBPushConfiguration and TCGBPushTokenInfo registered through registerPush were different

<a id="240-20210629"></a>

### 2.24.0 (2021.06.29) { #240-20210629 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-iOS.zip)

<a id="240-20210629-feature-updates"></a>
#### Feature Updates
* Change the internal launch URL

<a id="240-20210629-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the terms pop-up did not close after viewing the terms and conditions details

<a id="game-gamebase-release-notes-ios-1"></a>

### 2.23.0 (2021.06.14) { #game-gamebase-release-notes-ios-1 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-iOS.zip)

<a id="game-gamebase-release-notes-ios-1-feature-updates"></a>
#### Feature Updates
* Updated the external SDK: TOAST iOS SDK(0.28.0), ToastGamebaseIAP SDK(0.12.0)

<a id="220-20210525"></a>

### 2.22.0 (2021.05.25) { #220-20210525 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-iOS.zip)

<a id="220-20210525-feature-updates"></a>
#### Feature Updates
* Updated the external SDK: TOAST iOS SDK(0.27.2), Hangame iOS SDK(1.6.0)

<a id="212-20210427"></a>

### 2.21.2 (2021.04.27) { #212-20210427 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.2/GamebaseSDK-iOS.zip)

<a id="212-20210427-feature-updates"></a>
#### Feature Updates
* Facebook iOS SDK updated (9.2.0)

<a id="212-20210427-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where a bitcode error occurs when building an archive

<a id="211-20210419"></a>

### 2.21.1 (2021.04.19) { #211-20210419 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-iOS.zip)

<a id="211-20210419-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the setting is not properly reflected even if it is set to support bitcode

<a id="210-20210413"></a>

### 2.21.0 (2021.04.13) { #210-20210413 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-iOS.zip)

<a id="210-20210413-more-features"></a>
#### More Features
* Japanese authentication for Hangame added.    

<a id="210-20210413-feature-updates"></a>
#### Feature Updates
* Changed the system to support bitcode.
* Modified the system to display the Close button first when calling showWebView

<a id="202-20210323"></a>

### 2.20.2 (2021.03.23) { #202-20210323 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-iOS.zip)

<a id="202-20210323-feature-updates"></a>
#### Feature Updates
* Facebook iOS SDK updated (9.1.0)
* Fixed an issue of failing to call openURL delegate from GamebaseAuthFacebookAdapter in certain cases

<a id="201-20210309"></a>

### 2.20.1 (2021.03.09) { #201-20210309 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-iOS.zip)

<a id="201-20210309-feature-updates"></a>
#### Feature Updates
* Edited the IDFA acquisition logic in response to iOS 14: added the NSUserTrackingUsageDescription field to info.plist

<a id="200-20210209"></a>

### 2.20.0 (2021.02.09) { #200-20210209 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-iOS.zip)

* Common Terms and Conditions added
    * Added an API that opens the Terms and Conditions webview
    * Added an API that views the Terms and Conditions list and agreement status per user
    * Added an API that saves the user agreement data on the Gamebase server

<a id="200-20210209-more-features"></a>
#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

<a id="feature-updates"></a>

### 2.19.1 (2021.01.26) { #feature-updates }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-iOS.zip)

<a id="feature-updates-191-20210126"></a>
#### Feature Updates
* Changed the Weibo IdPAdapter structure

<a id="january-12-2021"></a>

### 2021. 01. 12. { #january-12-2021 }

```
Gamebase's minimum supported XCode version has changed from 10 to 11.
```
    
<a id="190-20201229"></a>

### 2.19.0 (2020.12.29) { #190-20201229 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-iOS.zip)

<a id="190-20201229-more-features"></a>
#### More Features

* Added Weibo authentication
    
<a id="190-20201229-feature-updates"></a>
#### Feature Updates

* Launching status code added: beta service (205)

<a id="182-20201215"></a>

### 2.18.2 (2020.12.15) { #182-20201215 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

<a id="182-20201215-more-features"></a>
#### More Features

* Added the additionalURL field for the case of a developer's own Customer Center being opened
* Added localized product information in the transaction item information: localizedTitle, localizedDescription

<a id="182-20201215-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST iOS SDK (0.27.1)
* showWebView: Returns error when invalid URL is delivered, delivered URL is used as is without encoding
* Changed to run the custom scheme regardless of letter case


<a id="180-20201110"></a>

### 2.18.0 (2020.11.10) { #180-20201110 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

<a id="180-20201110-feature-updates"></a>
#### Feature Updates

* Added API that supports the SceneDelegate of iOS 13 or higher

<a id="171-20201027"></a>

### 2.17.1 (2020.10.27) { #171-20201027 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

<a id="171-20201027-feature-updates"></a>
#### Feature Updates

* When sending a specific index, an error message is added to it: When push registration fails or when sending a game index

<a id="170-20201013"></a>

### 2.17.0 (2020.10.13) { #170-20201013 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)

```
If you want to use Hangame authentication, please contact the Customer Center in advance.
```

<a id="170-20201013-more-features"></a>
#### More Features

* Added Hangame IdP authentication

<a id="170-20201013-feature-updates"></a>
#### Feature Updates

* Supports the download feature when a Customer Center attachment image is clicked
* Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long.
* Changed the logic so that the title can be displayed again after changing URL and title in a web view

<a id="170-20201013-bug-fixes"></a>
#### Bug Fixes

* PAYCO authentication: Fixed an issue where the logout callback would not return when logout was called after lastLoggedInProvider login
    
<a id="160-20200922"></a>

### 2.16.0 (2020.09.22) { #160-20200922 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

<a id="160-20200922-more-features"></a>
#### More Features

* Added a feature to Customer Center
    * Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
    * Added the ContactConfiguration parameter so userName can be configured for Customer Center API
        
<a id="151-20200916"></a>

### 2.15.1 (2020.09.16) { #151-20200916 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

<a id="151-20200916-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST iOS SDK (0.27.0)
* A new version of IAP SDK is applied to support the changes made to iOS 14 beta. [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

<a id="150-20200825"></a>

### 2.15.0 (2020.08.25) { #150-20200825 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

<a id="150-20200825-more-features"></a>
#### More Features

* Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting
* Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)

<a id="150-20200825-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST iOS SDK (0.26.0)
* Added the null check logic for the payload of payment
    
<a id="140-20200811"></a>

### 2.14.0 (2020.08.11) { #140-20200811 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

<a id="140-20200811-feature-updates"></a>
#### Feature Updates

* Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings
* Added the contentMode setting for TCGBWebViewConfiguration

<a id="130-20200728"></a>

### 2.13.0 (2020.07.28) { #130-20200728 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

<a id="130-20200728-feature-updates"></a>
#### More Features

* Authenticate Sign In With Apple: Supported for iOS 12 or lower
    
<a id="120-20200714"></a>

### 2.12.0 (2020.07.14) { #120-20200714 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

<a id="120-20200714-more-features"></a>
#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order
    * Added Show Image Notice API

<a id="120-20200714-feature-updates"></a>
#### Feature Updates
* Facebook SDK updated (7.1.1)
* Attempts Gamebase initialization with storeCode(default=AS) set for configuration
* Fixed failed closing due to lack of the close button while printing webview which cannot load content
    
<a id="110-20200623"></a>

### 2.11.0 (2020.06.23) { #110-20200623 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

<a id="110-20200623-more-features"></a>
#### More Features

* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed

<a id="101-20200609"></a>

### 2.10.1 (2020.06.09) { #101-20200609 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

<a id="101-20200609-feature-updates"></a>
#### Feature Updates

* Updated to set device language if language code is not configured when user push setting is initialized

<a id="100-20200526"></a>

### 2.10.0 (2020.05.26) { #100-20200526 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

<a id="100-20200526-more-features"></a>
#### More Features
* Added GamebaseEventHandler which has all previous event systems
    * Includes ServerPush and Observer, and checks promotional purchase or push events


<a id="91-20200512"></a>

### 2.9.1 (2020.05.12) { #91-20200512 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)

<a id="91-20200512-bug-fixes"></a>
#### Bug Fixes

* Fixed the inavailability of a build on an unreal engine since warning is considered as a build error
        
<a id="90-20200428"></a>

### 2.9.0 (2020.04.28) { #90-20200428 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

<a id="90-20200428-more-features"></a>
#### More Features
* Suspension of Membership Withdrawal
    * Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended
        
<a id="90-20200428-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST iOS SDK (0.24.0)
* PAYCO iOS SDK update (1.4.0)

<a id="81-20200414"></a>

### 2.8.1 (2020.04.14) { #81-20200414 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

<a id="81-20200414-feature-updates"></a>
#### Feature Updates

* Added internal indicators to check Analytics delivery results
    
<a id="80-20200324"></a>

### 2.8.0 (2020.03.24) { #80-20200324 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

<a id="80-20200324-more-features"></a>
#### More Features

* Added more purchase and product information, such as product type and regional prices

<a id="80-20200324-feature-updates"></a>
#### Feature Updates

* Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console

<a id="71-20200225"></a>

### 2.7.1 (2020.02.25) { #71-20200225 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

<a id="71-20200225-feature-updates"></a>
#### Feature Updates

* Updated to return value, after guest login, when GetAuthProviderUserID is called

<a id="62-20191224"></a>

### 2.6.2 (2019.12.24) { #62-20191224 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)

<a id="62-20191224-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST iOS SDK (0.20.1)
* NAVER iOS SDK update (4.1.0)
    
<a id="61-20191210"></a>

### 2.6.1 (2019.12.10) { #61-20191210 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)
    
<a id="61-20191210-bug-fixes"></a>
#### Bug Fixes
* Fixed the issue in which mapping is not available when AddMapping (Forcibly) is applied
* Fixed crash occurrence by NSNUll object, when displayLanguageCode of PushConfiguration is not set by Unity Plugin
    

<a id="60-20191112"></a>

### 2.6.0 (2019.11.12) { #60-20191112 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

<a id="60-20191112-added-features"></a>
#### Added Features

* Added TOAST Logger to send data to Log & Crash for analysis
* Added authentication for Sign In with Apple

<a id="52-20191015"></a>

### 2.5.2 (2019.10.15) { #52-20191015 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

<a id="52-20191015-feature-updates"></a>
#### Feature Updates

* Changed UIWebView to WKWebView

<a id="september-10-2019-sdk-download"></a>

### 2.5.1 (2019.09.10) { #september-10-2019-sdk-download }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)
    
<a id="september-10-2019-sdk-download-feature-updates"></a>
#### Feature Updates

* Updated TCPushSDK to 1.7.0 for GamebasePushAdapter
    * Since the file has changed from a static library to a framework for TCPushSDK, TCPushSDK.framework must be added to the project.
    
<a id="50-august-27-2019"></a>

### 2.5.0 (2019.08.27) { #50-august-27-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

<a id="50-august-27-2019-more-features"></a>
#### More Features

* Provides API which opens CS URL entered on a console via webview
    
<a id="43-july-11-2019"></a>

### 2.4.3 (2019.07.11) { #43-july-11-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

<a id="43-july-11-2019-bug-fixes"></a>
#### Bug Fixes

* Fixed crash occurrence due to parsing attempts of error messages with conflicting formats, regarding authentication

<a id="42-june-25-2019"></a>

### 2.4.2 (2019.06.25) { #42-june-25-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

<a id="42-june-25-2019-features-updateschanges"></a>
#### Features Updates/Changes

* Add TOAST Launching information in the JSON string format to LaunchingInfo
* LINE iOS SDK Updated (v5.0.1)
    * The minimum support OS version for LINE Adapter has changed to iOS 10
    * Login also available via LINE app

<a id="42-june-25-2019-bug-fixes"></a>
#### Bug Fixes

* Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer.
* Fixed infrequent crashes occurred out of network connection issues

<a id="41-june-13-2019"></a>

### 2.4.1 (2019.06.13) { #41-june-13-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

<a id="41-june-13-2019-bug-fixes"></a>
#### Bug Fixes

* Fixed the error in output of indicators due to missing of partial parameters during transfer of Analytics indicators

<a id="40-may-28-2019"></a>

### 2.4.0 (2019.05.28) { #40-may-28-2019 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

<a id="40-may-28-2019-feature-updates"></a>
#### Feature Updates

* Changes to Analytics-related classes
    * LevelUpData Class: userLevel and levelUpTime parameters are now required / other fields removed [View more [iOS](./ios-etc/#level-up-trace)]
    * GameUserData Class: classId (game user's class) field added [View more [iOS](./ios-etc/#game-user-data-settings)]

    
<a id="30-20190423"></a>

### 2.3.0 (2019.04.23) { #30-20190423 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

<a id="30-20190423-1"></a>
#### Feature Updates

* Added Launching Status Code: "Under Review (204)", "Under Testing (203)"

<a id="22-20190411"></a>

### 2.2.2 (2019.04.11) { #22-20190411 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

<a id="22-20190411-1"></a>
#### Bug Fixes

* Fixed an issue where the Gamebase initialization callback was not called when showBlockingPopup was set to NO

<a id="20-20190326"></a>

### 2.2.0 (2019.03.26) { #20-20190326 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)

<a id="20-20190326-1"></a>
#### Added Features
* Added TransferAccount feature: a feature that allows guest users to transfer to a new device using up to 2 keys without mapping
    * Added APIs
        * TransferAccountInfo issuance API (issueTransferAccount)
        * API to request account transfer using the issued TransferAccountInfo (transferAccountWithIdPLogin)
        * API to query the issued TransferAccountInfo (queryTransferAccount)
        * API to renew the already-issued TransferAccountInfo (renewTransferAccount)
* Added forced mapping feature: a feature that allows mapping to an IdP account that is already linked to another account
    * Added APIs
        * Forced mapping API (addMappingForcibly)

<a id="20-20190326-2"></a>
#### Feature Updates

* Disabled the App login feature of LINE iOS SDK
    * Due to a bug in LINE SDK v4, app login fails on iOS 12, so Gamebase LINE Adapter has been changed to support web login only

<a id="10-20190226"></a>

### 2.1.0 (2019.02.26) { #10-20190226 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)

<a id="10-20190226-1"></a>
#### Feature Updates

* Removed TransferKey API
    * issueTransferKey: Issues a TransferKey
    * requestTransfer: Validates a TransferKey
        
<a id="10-20190226-2"></a>
#### Bug Fixes

* Fixed a bug where, after logging in to GameCenter via logic other than Gamebase, attempting to log in to GameCenter through Gamebase results in no response

<a id="00-20190129"></a>

### 2.0.0 (2019.01.29) { #00-20190129 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
SDK update is required to use the improved overall metrics in Gamebase 2.0.
```

<a id="00-20190129-1"></a>
#### Added Features

* Added APIs for custom indicators (automatically sent within the SDK upon a successful purchase)
    * setGameUserData: Sends user level information after game login
    * traceLevelUpData: Called when a game user levels up for level-up tracking

<a id="00-20190129-2"></a>
#### Feature Updates

* Updated IAP SDK
    * Fixed an issue where crashes occurred intermittently upon payment failure

<a id="00-20190129-3"></a>
#### Bug Fixes

* Fixed an issue where a crash occurred when initializing Gamebase with debugMode On in the simulator on iOS 12 or higher

<a id="142-20181115"></a>

### 1.14.2 (2018.11.15) { #142-20181115 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)

<a id="142-20181115-1"></a>
#### Feature Updates

* Fixed a structure that could cause a crash when applying Gamebase iOS SDK 1.14.0 and Unity Plugin 1.14.0, due to a change in the JSON string structure of the description method of the TCGBAuthProviderProfile object returned when calling the Provider Profile acquisition method

<a id="140-20181023"></a>

### 1.14.0 (2018.10.23) { #140-20181023 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)

<a id="140-20181023-1"></a>
#### Added Features

* Added file attachment feature in the Gamebase web view
    
<a id="140-20181023-2"></a>
#### Feature Updates
* Modified to URL-encode the messages written by users in the console for suspension/maintenance, and decode them on the client side for processing
* Updated PAYCO iOS SDK (1.2.4)
* Remove API: Webview, Network, Launching
    * **[TCGBUtil showToastWithMessage:duration:]**
    * **[TCGBWebView showWebBrowserWithURL:viewController:]**
    * **[TCGBWebView showWebViewWithURL:viewController:configuration:]**
    * **[TCGBLaunching addObserverOnChangedStatusNotification:]**
    * **[TCGBLaunching removeObserverOnChangedStatusNotification:]**
    * **[TCGBLaunching addUpdateStatusNotification]**
    * **[TCGBLaunching removeUpdateStatusNotification]**
    * **[TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]**
    * **[TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]**
* Deprecated API
    * **[TCGBGamebase languageCode]**

<a id="130-20180913"></a>

### 1.13.0 (2018.09.13) { #130-20180913 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)

<a id="130-20180913-1"></a>
#### Added Features

* Added API to support App Store Promotion IAP

<a id="130-20180913-2"></a>
#### Feature Updates

* Applied the latest version of IAP SDK (iOS: 1.6.0)
* Changed the structure of the result of calling the authProviderProfileWithIDPCode API to 1 depth (unified with Android and Unity)
        
<a id="122-20180828"></a>

### 1.12.2 (2018.08.28) { #122-20180828 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)

<a id="122-20180828-1"></a>
#### Feature Updates

* Improved Callback URL Scheme settings for Google Auth Adapter and NAVER Auth Adapter
    * Improved to set the Default URL Scheme if "url_scheme_ios_only" is not configured in the console: To use the Default URL Scheme, register tcgb.{Bundle ID}.google or tcgb.{Bundle ID}.naver in XCode > Target > Info > URL Types
* Improved PAYCO Auth Adapter
    * Fixed an issue where an unintended URL Scheme was called due to the URL Scheme not being set: The configuration method has changed, so a URL Scheme must be configured for the update (register tcgb.{Bundle ID}.payco in XCode > Target > Info > URL Types)

<a id="121-20180809"></a>

### 1.12.1 (2018.08.09) { #121-20180809 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)

<a id="121-20180809-1"></a>
#### Feature Updates

* Applied the latest version of IAP SDK (1.5.0)
* Improved the Gamebase maintenance page to display maintenance times according to the country time set on the device
* Added the ability to use the maintenance information entered in the Console when using the maintenance page as an external page
* An error occurs when a user mapped with IdP attempts Guest mapping (TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
* An error occurs when authentication API is called redundantly (AUTH_ALREADY_IN_PROGRESS_ERROR)
* Added error code: GameCenter login denied (TCGB_ERROR_IOS_GAMECENTER_DENIED)
    
<a id="121-20180809-2"></a>
#### Bug Fixes

* Fixed a bug where login was not possible due to a failure to retrieve profile information during NAVER login: Changed so that login succeeds even if profile information retrieval fails
    
<a id="120-20180724"></a>

### 1.12.0 (2018.07.24) { #120-20180724 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)

<a id="120-20180724-1"></a>
#### Feature Updates

* Added a feature to output version information of the Adapters in use and the app's build information to the Debug Log when initializing Gamebase
* Removed the binary of the NAVER ID Login SDK that was included in the NAVER Auth Adapter distributed via CocoaPods, and changed to a dependency configuration method

<a id="111-20180705"></a>

### 1.11.1 (2018.07.05) { #111-20180705 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)

<a id="111-20180705-1"></a>
#### Added Features

* Added LINE IdP

<a id="111-20180705-2"></a>
#### Feature Updates

* Changed so that when AddMapping succeeds after guest login, loginForLastLoggedInProvider logs in using the IdP account for which AddMapping succeeded
    
<a id="111-20180705-3"></a>
#### Bug Fixes

* Fixed a bug where subsequent APIs (login/push/purchase, etc.) could not proceed after maintenance was lifted

<a id="110-20180626"></a>

### 1.11.0 (2018.06.26) { #110-20180626 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)

<a id="110-20180626-1"></a>
#### Added Features
* Added Google IdP
* Added Twitter IdP
    
<a id="110-20180626-2"></a>
#### Feature Updates
* Added Japanese translation to LocalizedString
* Improved internal logic to clearly distinguish error codes when authentication API is called without initialization or login
* Updated NAVER ID Login SDK: iOS (4.0.10)
    
<a id="91-20180529"></a>

### 1.9.1 (2018.05.29) { #91-20180529 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)

<a id="91-20180529-1"></a>
#### Bug Fixes

* Fixed an issue where the title, back button, and close button did not appear in the Gamebase WebView NavigationBar area
    
<a id="90-20180503"></a>

### 1.9.0 (2018.05.03) { #90-20180503 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)

<a id="90-20180503-1"></a>
#### Added Features
* Added Transfer feature
    * A feature that allows guest users to transfer to a new device without mapping
    * Added APIs:
        * Transfer Key issuance API (IssueTransferKey)
        * API to request account transfer using the issued TransferKey (RequestTransfer)

<a id="90-20180503-2"></a>
#### Bug Fixes

* Fixed an issue where login failed due to a change in the format of the Scheme received from the server when attempting App to Web login during NAVER account login
* Fixed a bug where the message and Underlying Error were not set in the logic that creates an error object to be delivered to the user after receiving an UnderlyingError object from the Adapter

<a id="81-20180412"></a>

### 1.8.1 (2018.04.12) { #81-20180412 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)

<a id="81-20180412-1"></a>
#### Bug Fixes

* Fixed a bug where registerPush fails when displayLanguageCode is passed as null when calling registerPush

<a id="80-20180405"></a>

### 1.8.0 (2018.04.05) { #80-20180405 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)

<a id="80-20180405-1"></a>
#### Added Features

* Added Kick out feature
    * A feature that disconnects all users currently playing the game (can be used when you want to disconnect all users from the game during maintenance)
    * Added an API to receive kick out events
* Improved the maintenance webpage so that it can be used as an HTML page entered by the user in the Console
    * Previously, only the webpage provided by Gamebase or an external webpage connection was available
    * Users can now create a maintenance page in any desired format even without a web server
* Developed Observer feature and added APIs
    * Added an API to batch-process Listeners for changes in app status/network status/user status (ban) such as maintenance through Observer registration

<a id="80-20180405-2"></a>
#### Feature Updates

* The following APIs are deprecated due to the addition of the Observer feature: LaunchingStatus Listener, Network Listener (existing users can continue to use them)
* Applied PAYCO simple login 3rd SDK v1.2.2: provides token expiration information (expires_in) upon successful login, improved iPhoneX login UI
* Modified the webview interface to support iPhoneX

<a id="80-20180405-3"></a>
#### Bug Fixes
* Fixed an issue where concurrent access data was not saved when the country code was 10 or more characters long

<a id="70-20180222"></a>

### 1.7.0 (2018.02.22) { #70-20180222 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)

<a id="70-20180222-1"></a>
#### Added Features

* Added NAVER IdP authentication
* Added Display Language settings: Added a Display Language setting so that the language displayed to game users in-game can be set separately from the device language.

<a id="60-20180125"></a>

### 1.6.0 (2018.01.25) { #60-20180125 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)

<a id="60-20180125-1"></a>
#### Bug Fixes

* Applied defensive logic for parts that could cause a crash when calling the webview

<a id="50-20171221"></a>

### 1.5.0 (2017.12.21) { #50-20171221 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)

<a id="50-20171221-1"></a>
#### Added Features

* Added Close Callback that occurs when the webview is closed
* Added a feature to receive events from Custom Schemes used in the webview
* Released a new Unity Setting Tool

<a id="40-20171123"></a>

### 1.4.0 (2017.11.23) { #40-20171123 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)

<a id="40-20171123-1"></a>
#### Feature Updates

* Replaced the issue where text such as "x" or "<" was displayed when close/back button resources were not available with default values

<a id="40-20171123-2"></a>
#### Bug Fixes

* Fixed an issue where the NavigationBar Title was reset when the device was rotated after launching the webview
* Fixed an issue where the NavigationBar background area overlapped when customizing the NavigationBar Height of the webview

<a id="30-20171026"></a>

### 1.3.0 (2017.10.26) { #30-20171026 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)

<a id="30-20171026-1"></a>
#### Added Features

* Added AddMapping API using Credential

<a id="20-20170921"></a>

### 1.2.0 (2017.09.21) { #20-20170921 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)

<a id="20-20170921-1"></a>
#### Added Features

* Added ban (user penalty) feature
* Displays a popup window for banned users

<a id="15-20170720"></a>

### 1.1.5 (2017.07.20) { #15-20170720 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)

<a id="15-20170720-1"></a>
#### Feature Updates

* Added a daily batch feature to delete related data when the Gamebase product is suspended
* Added system popup window API (showAlertWithTitle)
* Changed to return country codes in uppercase (Android)
* Updated to TCPush SDK 1.4.1
* Updated to IAP SDK 1.3.3.20170627

<a id="14-20170525"></a>

### 1.1.4 (2017.05.25) { #14-20170525 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)

<a id="14-20170525-1"></a>
#### Feature Updates

* Added a daily batch feature to delete related data when the Gamebase product is suspended
* Provided an API to change the payment store at runtime

<a id="12-20170404"></a>

### 1.1.2 (2017.04.04) { #12-20170404 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)

<a id="12-20170404-1"></a>
#### Feature Updates

* Improved maintenance and urgent notice popup windows at game launch

<a id="10-20170321"></a>

### 1.1.0 (2017.03.21) { #10-20170321 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)

<a id="10-20170321-1"></a>
#### Feature Updates

* Added an interface that receives an external AccessToken and performs idPLogin

<a id="00-20170309"></a>

### 1.0.0 (2017.03.09) { #00-20170309 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-iOS.zip)

<a id="00-20170309-1"></a>
#### New Product Release
* A service that provides commonly required features for games to enable easy and efficient game development.
    * Supports various authentication methods: Guest, 3rd Party (Google, Facebook, GameCenter, etc.) authentication
    * Provides logout and membership withdrawal features
    * Provides a mapping feature that allows a single user to use multiple external IDPs simultaneously
    * Provides game app status management, maintenance, urgent notices, and other features for game operations via a web console
    * Provides a web console screen for checking real-time operational indicators
    * Integration with TOAST Cloud products: PUSH, IAP