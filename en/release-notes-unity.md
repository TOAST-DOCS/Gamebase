<!-- pre-align:aligned sig=602ad029c65d -->

<a id="game-gamebase-release-notes-unity"></a>
## Game > Gamebase > Release Notes > Unity { #game-gamebase-release-notes-unity }

<a id="2-82-0-2026-08-11"></a>
### 2.82.0 (2026. 08. 11.) { #2-82-0-2026-08-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.82.0/GamebaseSDK-Unity.zip)

<a id="820-2026-08-11-feature-updates"></a>
#### Feature Updates
* (Android, iOS) Improved the validation logic for invalid data.

<a id="820-2026-08-11-bug-fixes"></a>
#### Bug Fixes
* (Windows, macOS) Fixed an issue where the gamebase://openbrowser scheme was not processed in the WebView.

<a id="820-2026-08-11-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.82.0](./release-notes-android/#2-82-0-2026-07-28)
* [Gamebase iOS SDK 2.82.0](./release-notes-ios/#2-82-0-2026-07-28)

<a id="820-2026-08-11-setting-tool-v301"></a>
#### Setting Tool (v3.0.1)

* Added support for installing a dedicated adapter for the WebGL platform.

<a id="2-81-4-2026-07-14"></a>
### 2.81.4 (2026. 07. 14.) { #2-81-4-2026-07-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.4/GamebaseSDK-Unity.zip)

<a id="814-2026-07-14-bug-fixes"></a>
#### Bug Fixes
* (Windows, macOS) Fixed an intermittent error that occurred when attempting to reconnect via WebSocket in an unstable network environment.

<a id="814-2026-07-14-feature-updates"></a>
#### Feature Updates
* Applied Assembly Definition (.asmdef) on the Gamebase SDK.

<a id="814-2026-07-14-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.81.0](./release-notes-android/#2-81-0-2026-06-23)
* [Gamebase iOS SDK 2.81.3](./release-notes-ios/#2-81-3-2026-05-27)

<a id="2-81-3-2026-05-27"></a>
### 2.81.3 (2026. 05. 27.) { #2-81-3-2026-05-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.3/GamebaseSDK-Unity.zip)

<a id="813-2026-05-27-bug-fixes"></a>
#### Bug Fixes
* (Windows, MacOS, WebGL) Fixed an issue where an error occurred when the value of a key-value pair entered in userFields was null when sending logs to the Log&Crash server.
* (Windows, MacOS, WebGL) Fixed an issue where logType was displayed as "NORMAL" on the Log&Crash page even when logType was set when sending logs to the Log&Crash server.

<a id="813-2026-05-27-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.80.2](./release-notes-android/#2-80-2-2026-04-28)
* [Gamebase iOS SDK 2.81.3](./release-notes-ios/#2-81-3-2026-05-27)

<a id="2-81-1-2026-04-28"></a>
### 2.81.1 (2026. 04. 28.) { #2-81-1-2026-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.1/GamebaseSDK-Unity.zip)

<a id="811-2026-04-28-bug-fixes"></a>
#### Bug Fixes
* (Windows, macOS) Fixed an issue where the close callback was not received when ShowImageNotices was called again after checking "Do not show today" in the image notice.
* (Windows, macOS) Fixed an issue where changes to the game screen size while running in windowed mode were not reflected in the WebView area.

<a id="811-2026-04-28-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.80.2](./release-notes-android/#2-80-2-2026-04-28)
* [Gamebase iOS SDK 2.81.2](./release-notes-ios/#2-81-2-2026-04-28)

<a id="2-81-0-2026-03-24"></a>
### 2.81.0 (2026. 03. 24.) { #2-81-0-2026-03-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.0/GamebaseSDK-Unity.zip)

<a id="810-2026-03-24-1"></a>
#### 기능 추가
* (Windows, macOS) 외부 브라우저 로그인 IDP에 Epicgames가 추가되었습니다.

<a id="810-2026-03-24-2"></a>
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.80.0](./release-notes-android/#2-80-0-2026-02-13)
* [Gamebase iOS SDK 2.80.0](./release-notes-ios/#2-80-0-2026-02-13)

<a id="2-80-1-2026-03-10"></a>
### 2.80.1 (2026. 03. 10.) { #2-80-1-2026-03-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.1/GamebaseSDK-Unity.zip)

<a id="801-2026-03-10-1"></a>
#### 버그 수정
* (iOS) GameCenter에서 AddMapping 수행 시 오류가 발생하던 문제를 수정했습니다.
* (Windows) WebView 오픈시, 타이틀 영역을 클릭하면 웹뷰가 닫히는 문제를 수정했습니다.

<a id="801-2026-03-10-2"></a>
#### 기능 개선
* (Windows) WebView 내부 로직을 개선하였습니다.

<a id="2-80-0-2026-02-13"></a>
### 2.80.0 (2026. 02. 13.) { #2-80-0-2026-02-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-Unity.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### Feature Updates
* (Android, iOS) Added **PURCHASE_PENDING (4008)**, which is returned when a transaction awaits completion (e.g., slow-process payments or parental consent).
* (Android, iOS) Gamebase Event Handler's GamebaseEventCategory.PURCHASE_UPDATED event feature is expanded.
  * The GamebaseEventHandler now supports receiving completion events for pending payments (e.g., slow payments, parental consent) while the app is running.

<a id="800-2026-02-13-bug-fixes"></a>
#### Bug Fixes
* (Windows, macOS) Fixed an issue where the WebView could navigate back to a previous session after being closed and reopened.
* (Windows, macOS) Fixed an issue where the top of the WebView was obscured by the navigation bar.
* (Android) Fixed an issue where the game notice background was displayed as transparent.

<a id="2-79-0-2026-01-27"></a>
### 2.79.0 (2026. 01. 27.) { #2-79-0-2026-01-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-Unity.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### Feature Updates
* (Windows, macOS) Fixed missing navigation when WebView barHeight is not defined.
* (Windows, macOS) Fixed Close button visibility issues when WebView isBackButtonVisible is enabled.

<a id="2-77-0-2025-12-09"></a>
### 2.77.0 (2025. 12. 09.) { #2-77-0-2025-12-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.77.0/GamebaseSDK-Unity.zip)

<a id="770-2025-12-09-added-features"></a>
####  Added Features
* (WebGL) Added support for browser login

<a id="770-2025-12-09-feature-updates"></a>
#### Feature Updates
* Gamebase.Purchase.RequestItemListAtIAPConsole() API is deprecated.
  * Using Gamebase.Purchase.RequestItemListPurchasable() API is recommended.

<a id="770-2025-12-09-bug-fixes"></a>
#### Bug Fixes
* (WebGL) Fixed an issue where guest login failed.
  
<a id="2-76-0-2025-11-28"></a>
### 2.76.0 (2025. 11. 28.) { #2-76-0-2025-11-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.76.0/GamebaseSDK-Unity.zip)

<a id="760-2025-11-28-added-features"></a>
####  Added Features
* Added the launching.app.gameNotice.latestNoticeTimeMillis field to provide the post time of the most recently posted game announcement.
* (Android) Added the API to verify the age based on Google Play Age Signals to assist with compliance with age verification laws in certain jurisdictions, including Texas, Utah, and Louisiana, USA.
    * [Game > Gamebase > Unreal SDK User Guide > Note > Age Signals Support](./unreal-etc/#age-signals-support)

<a id="2-75-1-2025-10-17"></a>
### 2.75.1 (2025. 10. 17.) { #2-75-1-2025-10-17 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.1/GamebaseSDK-Unity.zip)

<a id="751-2025-10-17-bug-fixes"></a>
#### Bug Fixes
* (Windows) Fixed an exception that occurred when AdditionalInfo was null.
* (macOS) Fixed a DllNotFoundException issue in GamebaseUtil.

<a id="2-75-0-2025-09-23"></a>
### 2.75.0 (2025. 09. 23.) { #2-75-0-2025-09-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-Unity.zip)

<a id="750-2025-09-23-added-features"></a>
#### Added Features
* (Windows) Added Mapping feature

<a id="750-2025-09-23-feature-updates"></a>
#### Feature Updates
* (Android) Respond to Google Play's 16KB page constraint
* Improved internal logic.

<a id="2-74-0-2025-08-26"></a>
### 2.74.0 (2025. 08. 26.) { #2-74-0-2025-08-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.74.0/GamebaseSDK-Unity.zip)

<a id="740-2025-08-26-bug-fixes"></a>
#### Bug Fixes
* Fixed a crash issue that occurred when performing (iOS) ChangeLogin .
* Fixed a DllNotFoundException issue that occurred in (macOS) GamebaseUtil.

<a id="740-2025-08-26-1"></a>
#### Other
* The minimum supported version has been increased to Unity 2022.3.10.

<a id="2-73-2-2025-07-29"></a>
### 2.73.2 (2025. 07. 29.) { #2-73-2-2025-07-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.2/GamebaseSDK-Unity.zip)

<a id="732-2025-07-29-feature-updates"></a>
#### Feature Updates
* The additional support for (Standalone) login IDP: Twitter, Apple, Line

<a id="732-2025-07-29-end-of-support"></a>
#### End of Support
* Supporting Amazon Appstore ends.

<a id="2-73-1-2025-07-22"></a>
### 2.73.1 (2025. 07. 22.) { #2-73-1-2025-07-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-Unity.zip)

<a id="731-2025-07-22-bug-fixes"></a>
#### Bug Fixes
* (iOS) Fixed build error
* (macOS) Fixed build error for WebView adapter

<a id="2-73-0-2025-07-15"></a>
### 2.73.0 (2025. 07. 15.) { #2-73-0-2025-07-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Unity.zip)

<a id="730-2025-07-15-added-features"></a>
#### Added Features

<a id="730-2025-07-15-feature-updates"></a>
#### Feature Updates
* (Windows, macOS) Changed from webview to external browser when logging in to IdP.
    * Supported Browsers
        * Windows : every browser
        * macOS : Chrome, Safari, Firefox, whale

* Added external browser login cancellation API.
    * To change the IDP during an ongoing external browser login request, cancel the existing request.
    * CancelLoginWithExternalBrowser()

<a id="730-2025-07-15-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.73.0](./release-notes-android/#2-73-0-2025-07-15)
* [Gamebase iOS SDK 2.73.0](./release-notes-ios/#2-73-0-2025-07-15)

<a id="2-72-0-2025-06-24"></a>
### 2.72.0 (2025. 06. 24.) { #2-72-0-2025-06-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Unity.zip)

<a id="720-2025-06-24-added-features"></a>
#### Added Features

* (Windows) Added a "View Details" button to the update popup.
* (Windows) Added a Customer Center link to the account suspension popup.

<a id="720-2025-06-24-feature-updates"></a>
#### Feature Updates

* (Windows) Improved internal logic.

<a id="720-2025-06-24-bug-fixes"></a>
#### Bug Fixes

* (Windows) Fixed an issue where the maintenance status was not updated.

<a id="720-2025-06-24-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.72.0](./release-notes-android/#2-72-0-2025-06-24)
* [Gamebase iOS SDK 2.72.0](./release-notes-ios/#2-72-0-2025-06-24)

<a id="2-71-1-2025-06-11"></a>
### 2.71.1 (2025. 06. 11.) { #2-71-1-2025-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Unity.zip)

<a id="711-2025-06-11-bug-fixes"></a>
#### Bug Fixes

* (macOS) Fixed DllNotFoundException issue of GamebaseUtil.

<a id="2-71-0-2025-04-15"></a>
### 2.71.0 (2025. 04. 15.) { #2-71-0-2025-04-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Unity.zip)

<a id="710-2025-04-15-added-features"></a>
#### Added Features
* Added a new feature: Game Notice
    * Gamebase.GameNotice.OpenGameNotice(GamebaseCallback.ErrorDelegate callback)
    * To learn how to call the API, see the following link.
        * [Game > Gamebase > Unity SDK User Guide > UI > GameNotice](./unity-ui/#gamenotice)

<a id="710-2025-04-15-feature-updates"></a>
#### Feature Updates

* Improved internal logic.
* (iOS) Improved ViewController setup logic to prevent initialization failure errors.

<a id="710-2025-04-15-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue that caused a crash on macOS 15.4.

<a id="710-2025-04-15-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.71.0](./release-notes-android/#2-71-0-2025-04-15)
* [Gamebase iOS SDK 2.71.0](./release-notes-ios/#2-71-0-2025-04-15)

<a id="2-70-1-2025-03-13"></a>
### 2.70.1 (2025. 03. 13.) { #2-70-1-2025-03-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.1/GamebaseSDK-Unity.zip)

<a id="701-2025-03-13-bug-fixes"></a>
#### Bug Fixes

* (Android) ShowWebView, ShowTermsView 호출 시 Configuration가 없으면 크래시가 발생하는 문제를 수정했습니다.

<a id="701-2025-03-13-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.70.1](./release-notes-android/#2-70-1-2025-03-13)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2-70-0-2025-03-11)

<a id="2-70-0-2025-03-11"></a>
### 2.70.0 (2025. 03. 11.) { #2-70-0-2025-03-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Unity.zip)

<a id="700-2025-03-11-added-features"></a>
#### Added Features

* (Android) Added an initialization option for the GPGS Auto Login feature that prompts the user to log in to GPGS only once after installing the app.
  * GamebaseRequest.GamebaseConfiguration enableGPGSSignInCheck
    * By default, this is set to true, which means the GPGS login prompt will appear again during Gamebase initialization even if the user previously declined.
    * If set to false, the GPGS login prompt is shown only once when the app is launched for the first time.
* Added options to configure the navigation bar title color and icon tint color in GamebaseWebView.
    * WebView.Configuration navigationTitleColor
    * WebView.Configuration navigationIconTintColor

<a id="700-2025-03-11-feature-updates"></a>
#### Feature Updates

* Improved internal logic.

<a id="700-2025-03-11-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.70.0](./release-notes-android/#2-70-0-2025-03-11)
* [Gamebase iOS SDK 2.70.0](./release-notes-ios/#2-70-0-2025-03-11)

<a id="700-2025-03-11-setting-tool-v300"></a>
#### Setting Tool (v3.0.0)

* Improved the UX to be more user-friendly and aligned with its intended purpose.
* Provides intuitive features to make configuration and updates easier.
* Enhanced flexibility for smoother updates during deployment.

<a id="2-69-0-2025-01-21"></a>
### 2.69.0 (2025. 1. 21.) { #2-69-0-2025-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Unity.zip)

<a id="690-2025-1-21-added-features"></a>
#### Added Features

* Added the RequestLastLoggedInProvider API
* (Android) Added WebView Cutout color feature
* (Windows, macOS) Added X(Twitter) Login

<a id="690-2025-1-21-feature-updates"></a>
#### Feature Updates

* Improved internal logic.
* Improved code related to setting WebView color.
  * Added a new field to the Configuration.
    * WebView.Configuration navigationColor
    * Community.Configuration backgroundColor
    * ImageNotice.Configuration backgroundColor

<a id="690-2025-1-21-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.69.0](./release-notes-android/#2-69-0-2025-01-21)
* [Gamebase iOS SDK 2.69.0](./release-notes-ios/#2-69-0-2025-01-21)

<a id="2-68-1-2024-12-10"></a>
### 2.68.1 (2024. 12. 10.) { #2-68-1-2024-12-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.1/GamebaseSDK-Unity.zip)

<a id="681-2024-12-10-feature-updates"></a>
#### Feature Updates

* Improved internal logic.

<a id="681-2024-12-10-platform-specific-changes"></a>
#### Platform-Specific Changes

* [Gamebase iOS SDK 2.68.1](./release-notes-ios/#2-68-1-2024-12-10)

<a id="2-68-0-2024-11-26"></a>
### 2.68.0 (2024. 11. 26.) { #2-68-0-2024-11-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Unity.zip)

<a id="680-2024-11-26-ended-support"></a>
#### Ended Support

* Ended support for FacebookAdapter for Unity.

<a id="680-2024-11-26-added-features"></a>
#### Added Features

* (Android) Support GameActivity.

<a id="680-2024-11-26-feature-updates"></a>
#### Feature Updates

* Improved internal logic.

<a id="680-2024-11-26-bug-fixes"></a>
#### Bug Fixes

* Improved an issue that caused a JSON parsing error when enabling Network Insights settings in NHN Cloud Console.

<a id="680-2024-11-26-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.68.0](./release-notes-android/#2-68-0-2024-11-26)
* [Gamebase iOS SDK 2.68.0](./release-notes-ios/#2-68-0-2024-11-26)

<a id="2-67-0-2024-10-29"></a>
### 2.67.0 (2024. 10. 29.) { #2-67-0-2024-10-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Unity.zip)

<a id="670-2024-10-29-added-features"></a>
#### Added Features

* (Android, iOS) Added Steam authentication

<a id="670-2024-10-29-feature-updates"></a>
#### Feature Updates

* Changed Unity minimum supported version: 2020.3.16f1
* Changed the failure callback to be called when an exception occurs inside the WebView of a rolling image announcement.
* Improved internal logic.

<a id="670-2024-10-29-bug-fixes"></a>
#### Bug Fixes

* Fixed errors occurred by storeCodeStandalone.

<a id="670-2024-10-29-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.67.0](./release-notes-android/#2-67-0-2024-10-29)
* [Gamebase iOS SDK 2.67.0](./release-notes-ios/#2-67-0-2024-10-29)

<a id="2-66-3-2024-09-10"></a>
### 2.66.3 (2024. 09. 10.) { #2-66-3-2024-09-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

<a id="663-2024-09-10-feature-updates"></a>
#### Feature Updates
* Changed the minimum supported version of Unity to 2020.3.0f1

<a id="2-66-3-2024-09-05"></a>
### 2.66.3 (2024. 09. 05.) { #2-66-3-2024-09-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Unity.zip)

<a id="663-2024-09-05-bug-fixes"></a>
#### Bug Fixes
* (iOS) Fixed an issue where crash occurs after payment on iOS 12.

<a id="2-66-2-2024-08-27"></a>
### 2.66.2 (2024. 08. 27.) { #2-66-2-2024-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-Unity.zip)

<a id="662-2024-08-27-feature-updates"></a>
#### Feature Updates

* Deprecated the following field from iOS. The field is only available in Android.
    * GamebaseWebViewConfiguration.orientation deprecated

<a id="2-66-1-2024-07-23"></a>
### 2.66.1 (2024. 07. 23.) { #2-66-1-2024-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Unity.zip)

<a id="661-2024-07-23-added-features"></a>
#### Added Features

* (macOS) Responeded to the privacy policy.

<a id="661-2024-07-23-feature-updates"></a>
#### Feature Updates

* Improved internal logic.

<a id="661-2024-07-23-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.66.1](./release-notes-android/#2-66-1-2024-07-23)
* [Gamebase iOS SDK 2.66.0](./release-notes-ios/#2-66-0-2024-07-23)

<a id="2-66-0-2024-07-12"></a>
### 2.66.0 (2024. 07. 12.) { #2-66-0-2024-07-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Unity.zip)

<a id="660-2024-07-12-added-features"></a>
#### Added Features

* (Android) Added GPGS v2 authentication.

<a id="660-2024-07-12-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.66.0](./release-notes-android/#2-66-0-2024-07-10)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2-65-1-2024-06-25)

<a id="660-2024-07-12-setting-tool-v290"></a>
#### Setting Tool (v2.9.0)

* Added GPGS v2 authentication.GPGS V2. (Only for Android)

<a id="2-65-1-2024-06-25"></a>
### 2.65.1 (2024. 06. 25.) { #2-65-1-2024-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-Unity.zip)

<a id="651-2024-06-25-feature-updates"></a>
#### Feature Updates
* Fixed so that if there are no images to show on a particular client, a success callback is called instead of an error.

<a id="651-2024-06-25-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where an empty image notice is displayed when there is no registered image notice.
* (macOS) Fixed an error in UnityEditor where the GamebaseUtils.bundle file was not referenced.

<a id="651-2024-06-25-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.65.1](./release-notes-android/#2-65-1-2024-06-25)
* [Gamebase iOS SDK 2.65.1](./release-notes-ios/#2-65-1-2024-06-25)

<a id="2-65-0-2024-06-11"></a>
### 2.65.0 (2024. 06. 11.) { #2-65-0-2024-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-Unity.zip)

<a id="650-2024-06-11-added-features"></a>
#### Added Features

* Added a new type to the image notice feature.
    * Added the `rolling popup` type.
    * Displays the existing image notice as the `individual popup` type.
* Improved internal logic.

<a id="650-2024-06-11-platform-specific-changes"></a>
#### Platform-Specific Changes
* [Gamebase Android SDK 2.65.0](./release-notes-android/#2-65-0-2024-06-11)
* [Gamebase iOS SDK 2.65.0](./release-notes-ios/#2-65-0-2024-06-11)

<a id="2-64-0-2024-05-28"></a>
### 2.64.0 (2024. 05. 28.) { #2-64-0-2024-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Unity.zip)

<a id="640-2024-05-28-added-features"></a>
#### Added Features

* Improved internal logic.

<a id="640-2024-05-28-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.64.0](./release-notes-android/#2-64-0-2024-05-28)
* [Gamebase iOS SDK 2.64.0](./release-notes-ios/#2-64-0-2024-05-28)

<a id="2-63-0-2024-04-23"></a>
### 2.63.0 (2024. 04. 23.) { #2-63-0-2024-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Unity.zip)

<a id="630-2024-04-23-added-features"></a>
#### Added Features

* (MacOS) Added support for WebView

<a id="630-2024-04-23-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.63.0](./release-notes-android/#2-63-0-2024-04-23)
* [Gamebase iOS SDK 2.63.0](./release-notes-ios/#2-63-0-2024-04-23)

<a id="2-62-0-2024-03-26"></a>
### 2.62.0 (2024. 03. 26.) { #2-62-0-2024-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Unity.zip)

<a id="620-2024-03-26-added-features"></a>
#### Added Features
* Responded to iOS privacy policy.
    * Applied Privacy manifest and signature to Gamebase SDK.
* Added the testDevice field to indicate a test device in LaunchingInfo VO that returns after Gamebase is initialized.
* (MacOS, Windows) Added the feature to open FAQs/announcements directly for TOAST-type customer support.

<a id="620-2024-03-26-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.62.0](./release-notes-android/#2-62-0-2024-03-26)
* [Gamebase iOS SDK 2.62.0](./release-notes-ios/#2-62-0-2024-03-26)

<a id="2-61-0-2024-02-27"></a>
### 2.61.0 (2024. 02. 27.) { #2-61-0-2024-02-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-Unity.zip)

<a id="610-2024-02-27-bug-fixes"></a>
#### Bug Fixes
* (macOS) Fixed an issue where internal bundle files are not loaded normally.

<a id="610-2024-02-27-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2-60-0-2024-01-23)
* [Gamebase iOS SDK 2.61.0](./release-notes-ios/#2-61-0-2024-02-27)

<a id="2-60-0-2024-01-23"></a>
### 2.60.0 (2024. 01. 23.) { #2-60-0-2024-01-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Unity.zip)

<a id="600-2024-01-23-added-features"></a>
#### Added Features
* Improved internal logic.

<a id="600-2024-01-23-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.60.0](./release-notes-android/#2-60-0-2024-01-23)
* [Gamebase iOS SDK 2.60.0](./release-notes-ios/#2-60-0-2024-01-23)

<a id="2-59-0-2023-12-19"></a>
### 2.59.0 (2023. 12. 19.) { #2-59-0-2023-12-19 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-Unity.zip)

<a id="590-2023-12-19-added-features"></a>
#### Added Features
* (Standalone) Added support for macOS.
* Improved internal logic.

<a id="590-2023-12-19-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.59.0](./release-notes-android/#2-59-0-2023-12-19)
* [Gamebase iOS SDK 2.59.0](./release-notes-ios/#2-59-0-2023-12-19)

<a id="2-57-0-2023-10-31"></a>
### 2.57.0 (2023. 10. 31.) { #2-57-0-2023-10-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Unity.zip)

<a id="570-2023-10-31-added-features"></a>
#### Added Features
(Common) Added the Gamebase.Logger.Report API to send logs related to exceptions in try/catch statements.
* (iOS) Added the AUTH_IDP_LOGIN_EXTERNAL_AUTHENTICATION_REQUIRED error code.

<a id="570-2023-10-31-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.57.0](./release-notes-android/#2-57-0-2023-10-31)
* [Gamebase iOS SDK 2.57.0](./release-notes-ios/#2-57-0-2023-10-31)

<a id="2-55-0-2023-09-12"></a>
### 2.55.0 (2023. 09. 12.) { #2-55-0-2023-09-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-Unity.zip)

<a id="550-2023-09-12-added-features"></a>
#### Added Features
* (iOS) Add the GamebaseRequest.Push.PushConfiguration.alwaysAllowTokenRegistration to allow users to register tokens even if they deny push permissions.

<a id="550-2023-09-12-feature-updates"></a>
#### Feature Updates
* Removed the NHN Cloud Unity SDK from the Gamebase Unity SDK as it was discontinued.
* Improved internal logic.

<a id="550-2023-09-12-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.55.0](./release-notes-android/#2-55-0-2023-09-12)
* [Gamebase iOS SDK 2.55.0](./release-notes-ios/#2-55-0-2023-09-12)

<a id="2-54-0-2023-08-29"></a>
### 2.54.0 (2023. 08. 29.) { #2-54-0-2023-08-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.54.0/GamebaseSDK-Unity.zip)

<a id="540-2023-08-29-added-features"></a>
#### Added Features
* (Android) Added **GamebaseRequest.Push.PushConfiguration.requestNotificationPermission** field to prevent the Push permission request popup from automatically appearing when calling the RegisterPush API on Android 13 and later.
* (Android) Added a new API to specify an option to hide the loading animation during the loginForLastLoggedInProvider call.
    * Gamebase.LoginForLastLoggedInProvider(Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback);
    * For how to call the API, see the following document.
        * [Game > Gamebase > Unity SDK User Guide > Authentication > Login > Login Flow > Login as the Latest Login IdP](./unity-authentication/#login-as-the-latest-login-idp)

<a id="540-2023-08-29-bug-fixes"></a>
#### Bug Fixes
* (Standalone) Modified not to display a service error page when calling Gamebase Customer Center.

<a id="540-2023-08-29-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.53.0](./release-notes-android/#2-53-0-2023-08-17)
* [Gamebase iOS SDK 2.54.0](./release-notes-ios/#2-54-0-2023-08-29)

<a id="2-52-1-2023-07-25"></a>
### 2.52.1 (2023. 07. 25.) { #2-52-1-2023-07-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Unity.zip)

<a id="521-2023-07-25-bug-fixes"></a>
#### Bug Fixes
* (Standalone) Fixed an issue where sending a log before Gamebase Logger initialization was complete would result in a null reference exception.

<a id="521-2023-07-25-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.52.1](./release-notes-android/#2-52-1-2023-07-17)
* [Gamebase iOS SDK 2.53.0](./release-notes-ios/#2-53-0-2023-07-25)

<a id="2-52-0-2023-06-27"></a>
### 2.52.0 (2023. 06. 27.) { #2-52-0-2023-06-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-Unity.zip)

<a id="520-2023-06-27-added-features"></a>
#### Added Features
* Setting Tool (v2.7.0)
    * Added ONE Store v21 Purchase Adapter. (Android only)
    * Added Gamebase Custom Push Receiver Adapter. (Android only)

<a id="520-2023-06-27-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK(0.28.3)
* Improved internal logic.

<a id="520-2023-06-27-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.52.0](./release-notes-android/#2-52-0-2023-06-27)
* [Gamebase iOS SDK 2.52.0](./release-notes-ios/#2-52-0-2023-06-27)

<a id="2-51-0-2023-05-30"></a>
### 2.51.0 (2023. 05. 30.) { #2-51-0-2023-05-30 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.51.0/GamebaseSDK-Unity.zip)

<a id="510-2023-05-30-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK(0.28.1)
* Improved internal logic.

<a id="510-2023-05-30-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2-50-0-2023-05-16)
* [Gamebase iOS SDK 2.51.0](./release-notes-ios/#2-51-0-2023-05-30)

<a id="2-50-0-2023-05-16"></a>
### 2.50.0 (2023. 05. 16.) { #2-50-0-2023-05-16 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.50.0/GamebaseSDK-Unity.zip)

<a id="500-2023-05-16-added-features"></a>
#### Added Features
* (Android) Added MyCard store.
* Setting Tool (v2.5.0)
    * Added MyCard store. (Only for Android)
    * Added the feature to automatically set up the Huawei repository when adding a Huawei IAP.
<a id="500-2023-05-16-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK(0.28.0)

<a id="500-2023-05-16-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.50.0](./release-notes-android/#2-50-0-2023-05-16)
* [Gamebase iOS SDK 2.49.2](./release-notes-ios/#2-49-2-2023-04-28)

<a id="2-49-0-2023-04-25"></a>
### 2.49.0 (2023. 04. 25.) { #2-49-0-2023-04-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unity.zip)

<a id="490-2023-04-25-feature-updates"></a>
#### Feature Updates
* (iOS) Improved the internal logic.

<a id="490-2023-04-25-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.49.0](./release-notes-android/#2-49-0-2023-04-25)
* [Gamebase iOS SDK 2.49.1](./release-notes-ios/#2-49-1-2023-04-25)

<a id="2-48-0-2023-03-28"></a>
### 2.48.0 (2023. 03. 28.) { #2-48-0-2023-03-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-Unity.zip)

<a id="480-2023-03-28-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK (0.27.4)
* Applied standby domain for Gamebase server (GSLB redundancy)
* iOS
    * Raised the minimum supported version of Xcode to 14.1. 
    * Raised the minimum supported version of iOS to 11.0.
    * Removed support for armv7, armv7s, i386 architectures.
    * Removed support for bitcode.

<a id="480-2023-03-28-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2-48-0-2023-03-28)
* [Gamebase iOS SDK 2.48.0](./release-notes-ios/#2-48-0-2023-03-28)

<a id="2-46-0-2023-01-31"></a>
### 2.46.0 (2023. 01. 31.) { #2-46-0-2023-01-31 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-Unity.zip)

<a id="460-2023-01-31-added-features"></a>
#### Added Features
* (WebGL) Added Google login feature.
* (Android) Re-supported the field that sets whether to use a fixed font size in the WebView.
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) Added a setting to allow the WebView to render using all available screen space, including cutout (notched) areas.
    * GamebaseWebViewConfiguration.renderOutsideSafeArea
* (Android) Added the RequestSubscriptionsStatus API to view IAP subscription status.

<a id="460-2023-01-31-bug-fixes"></a>
#### Bug Fixes
* (Standalone) Fixed an error where ReflectionTypeLoadException error occurs intermittently on initialization.

<a id="460-2023-01-31-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.46.0](./release-notes-android/#2-46-0-2023-01-31)
* [Gamebase iOS SDK 2.46.0](./release-notes-ios/#2-46-0-2023-01-31)

<a id="2-45-0-2022-12-27"></a>
### 2.45.0 (2022. 12. 27.) { #2-45-0-2022-12-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-Unity.zip)

<a id="450-2022-12-27-added-features"></a>
#### Added Features
* Make sure to update to a new API due to changes to the Query Unconsumed Purcahses API. 

        // Deprecated API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
         
        // New API 
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                       GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);

* Make sure to update to a new API due to changes to the Query Activated Subscription API.
    * To get the same result as the existing API, set the value of **GamebaseRequest.Purchase.PurchasableConfiguration.allStores** to **true**.

            // Deprecated API 
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
             
            // New API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                        GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);

<a id="450-2022-12-27-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK (0.27.1)

<a id="450-2022-12-27-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.45.0](./release-notes-android/#2-45-0-2022-12-27)
* [Gamebase iOS SDK 2.45.0](./release-notes-ios/#2-45-0-2022-12-27)

<a id="2-44-2-2022-11-29"></a>
### 2.44.2 (2022. 11. 29.) { #2-44-2-2022-11-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.2/GamebaseSDK-Unity.zip)

<a id="442-2022-11-29-added-features"></a>
#### Added Features

* Setting Tool (v2.5.0)
    * Added ONE store v19 Purcahse Adapter. (Android Only)
    * You must reinstall the latest version of SettingTool after removing the existing SettingTool from Unity projects.

<a id="442-2022-11-29-bug-fixes"></a>
#### Bug Fixes
* (iOS) Fixed an issue where, when changing Screen.orientation during game play, the APIs affected by the view controller, such as WebView and Customer Center, were not displayed normally.

<a id="442-2022-11-29-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.44.2](./release-notes-android/#2-44-2-2022-11-29)
* [Gamebase iOS SDK 2.44.0](./release-notes-ios/#2-44-0-2022-10-25)

<a id="2-44-0-2022-10-11"></a>
### 2.44.0 (2022. 10. 11.) { #2-44-0-2022-10-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Unity.zip)

<a id="440-2022-10-11-feature-updates"></a>
#### Feature Updates
* External SDK update: NHN Cloud Unity SDK(0.26.2)

<a id="440-2022-10-11-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.44.0](./release-notes-android/#2-44-0-2022-10-11)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2-43-3-2022-10-04)

<a id="2-43-0-2022-09-07"></a>
### 2.43.0 (2022. 09. 07.) { #2-43-0-2022-09-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Unity.zip)

<a id="430-2022-09-07-feature-updates"></a>
#### Feature Updates
* External SDK update: TOAST Unity SDK(0.26.1), Kakaogame Unity SDK(3.14.5)
* Modified to enter a service region when logging in to LINE.
  * [Game > Gamebase > Unity SDK User Guide > Authentication > Login with IdP](./unity-authentication/#login-with-idp)

<a id="430-2022-09-07-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2-43-0-2022-09-07)
* [Gamebase iOS SDK 2.43.0](./release-notes-ios/#2-43-0-2022-09-07)

<a id="2-42-1-2022-08-09"></a>
### 2.42.1 (2022. 08. 09.) { #2-42-1-2022-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unity.zip)

<a id="421-2022-08-09-added-features"></a>
#### Added Features
* Added the mappedUserValid field that represents the mapped user status to the ForcingMappingTicket class.

<a id="421-2022-08-09-feature-updates"></a>
#### Feature Updates
* The field to set whether to use the fixed font size in WebView is no longer used.
    * **GamebaseWebViewConfiguration.enableFixedFontSize**
* Added the default values of GamebaseWebViewConfiguration.
    * The default values of navigation bar colorR, colorG, colorB, and colorA are set to 18, 93, 230, 255.
    * The default value of the isNavigationBarVisible field to enable the navigation bar is set to true.
    * The default value of the isBackButtonVisible field to enable the Go Back button in WebView is set to true.

<a id="421-2022-08-09-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2-42-1-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2-42-1-2022-08-09)

<a id="2-41-0-2022-07-05"></a>
### 2.41.0 (2022. 07. 05.) { #2-41-0-2022-07-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unity.zip)

<a id="410-2022-07-05-added-features"></a>
#### Added Features
* External SDK update: TOAST Unity SDK(0.25.6)
* Added the **IDP_REVOKED** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > Unity SDK User Guide > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unity-etc/#idp-revoked)

<a id="410-2022-07-05-feature-updates"></a>
#### Feature Updates
* Fixed an issue where memory leakage occurs when using the Burst package of Unity.
* Setting Tool (v2.4.0)
    * Added internal stability metrics.
    * You need to install the latest version of Setting Tool after completely deleting the previous version of SettingTool from Unity projects.
    * SettingTool v1 is no longer supported.

<a id="410-2022-07-05-bug-fixes"></a>
#### Bug Fixes
* (iOS) Fixed an issue where a crash would occur in a specific environment after payment.

<a id="410-2022-07-05-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2-41-0-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2-41-0-2022-07-05)

<a id="2-40-0-2022-05-24"></a>
### 2.40.0 (2022. 05. 24.) { #2-40-0-2022-05-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unity.zip)

<a id="400-2022-05-24-added-features"></a>
#### Added Features
* External SDK update: TOAST Unity SDK(0.25.5)
* (Standalone) Made changes to support the following Terms API.
    * Gamebase.Terms.QueryTerms
    * Gamebase.Terms.UpdateTerms

<a id="400-2022-05-24-feature-updates"></a>
#### Feature Updates
* Improved an issue where Hangul is displayed in Unicode.
* (iOS) Made a fix to support bitcode.

<a id="400-2022-05-24-bug-fixes"></a>
#### Bug Fixes
* (Android) Fixed an issue where Configuration.additionalParameters were not applied when calling the OpenContact API.

<a id="400-2022-05-24-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2-40-0-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2-40-0-2022-05-24)

<a id="2-39-0-2022-05-10"></a>
### 2.39.0 (2022. 05. 10.) { #2-39-0-2022-05-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Unity.zip)

<a id="390-2022-05-10-added-features"></a>
#### Added Features
* External SDK update: TOAST Unity SDK(0.25.4)

<a id="390-2022-05-10-bug-fixes"></a>
#### Bug Fixes
* Made a fix so that JsonException does not occur when calling the GetLaunchingInformations() API before initialization.

<a id="390-2022-05-10-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.39.0](./release-notes-android/#2-39-0-2022-05-10)
* [Gamebase iOS SDK 2.39.0](./release-notes-ios/#2-39-0-2022-05-10)

<a id="2-38-0-2022-05-03"></a>
### 2.38.0 (2022. 05. 03.) { #2-38-0-2022-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Unity.zip)

<a id="380-2022-05-03-added-features"></a>
#### Added Features
* External SDK update: TOAST Unity SDK(0.25.3)

<a id="380-2022-05-03-feature-updates"></a>
#### Feature Updates
* Fixed unnatural sentences in the Traditional Chinese (zh-TW) language set of Display Language.

<a id="380-2022-05-03-bug-fixes"></a>
#### Bug Fixes
* (Android) Fixed an issue where an error occurred when calling certain APIs under API Level 24.
    * Gamebase.Purchase.RequestActivatedPurchases()
    * Gamebase.Purchase.RequestItemListOfNotConsumed()

<a id="380-2022-05-03-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.38.0](./release-notes-android/#2-38-0-2022-05-03)
* [Gamebase iOS SDK 2.38.0](./release-notes-ios/#2-38-0-2022-05-03)

<a id="2-37-0-2022-04-26"></a>
### 2.37.0 (2022. 04. 26.) { #2-37-0-2022-04-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Unity.zip)

<a id="370-2022-04-26-added-features"></a>
#### Added Features
* Added the following field so that you can add parameters after the contact center URL.
    * GamebaseRequest.Contact.Configuration.additionalParameters

<a id="370-2022-04-26-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.37.0](./release-notes-android/#2-37-0-2022-04-26)
* [Gamebase iOS SDK 2.37.0](./release-notes-ios/#2-37-0-2022-04-26)

<a id="2-36-0-2022-04-12"></a>
### 2.36.0 (2022. 04. 12.) { #2-36-0-2022-04-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Unity.zip)

<a id="360-2022-04-12-added-features"></a>
#### Added Features
* External SDK update: TOAST Unity SDK(0.25.2)
* Added the isPromotion field to determine whether it is promotion payment while purchasing
    * GamebaseResponse.Purchase.PurchasableReceipt.isPromotion
* Added the isTestPurchase field to determine whether it is test payment while purchasing
    * GamebaseResponse.Purchase.PurchasableReceipt.isTestPurchase

<a id="360-2022-04-12-bug-fixes"></a>
#### Bug Fixes
* Fixed an error where, when a device was set to a specific cultural area, price information of the purchase product was entered as 0.
* (iOS) Fixed an issue where, when clicking on push notifications, deep links did not work.
* (iOS) Fixed an error where, when the orientation of the project was set to Auto Rotation and Gamebase API was called in Awake of MonoBehaviour included in the first scene of the project, UI such as WebView was not displayed properly.

<a id="360-2022-04-12-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.36.0](./release-notes-android/#2-36-0-2022-04-12)
* [Gamebase iOS SDK 2.36.0](./release-notes-ios/#2-36-0-2022-04-12)

<a id="2-35-0-2022-03-29"></a>
### 2.35.0 (2022. 03. 29.) { #2-35-0-2022-03-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Unity.zip)

<a id="350-2022-03-29-added-features"></a>
#### Added Features

* External SDK update: TOAST Unity SDK(0.25.1)
* Added an API to determine whether the terms and conditions window is displayed or not.
    * Gamebase.Terms.IsShowingTermsView()
* Added an option to hide the navigation bar in WebView.
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.isNavigationBarVisible
* (Android) Added an option to fix the font size in WebView.
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) Added an option to fix the font size in the terms and conditions window.
    * GamebaseRequest.Terms.GamebaseTermsConfiguration.enableFixedFontSize
* Setting Tool
    * (Android) Added Amazon store.
    * (Android) Added Huawei store.

<a id="350-2022-03-29-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.35.0](./release-notes-android/#2-35-0-2022-03-29)
* [Gamebase iOS SDK 2.35.0](./release-notes-ios/#2-35-0-2022-03-29)

<a id="2-34-1-2022-03-15"></a>
### 2.34.1 (2022. 03. 15.) { #2-34-1-2022-03-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-Unity.zip)

<a id="341-2022-03-15-added-features"></a>
#### Added Features
* Added an API to determine whether the device has allowed notifications or not.
    * Gamebase.Push.QueryNotificationAllowed

<a id="341-2022-03-15-bug-fixes"></a>
#### Bug Fixes
* Fixed an error where the isBackButtonVisible setting of GamebaseWebViewConfiguration was not applied on iOS.

<a id="341-2022-03-15-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2-34-0-2022-02-22)
* [Gamebase iOS SDK 2.34.1](./release-notes-ios/#2-34-1-2022-03-15)

<a id="2-34-0-2022-02-22"></a>
### 2.34.0 (2022. 02. 22.) { #2-34-0-2022-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Unity.zip)

<a id="340-2022-02-22-added-features"></a>
#### Added Features
* Added a VO class that can be used to find out whether the terms and conditions UI was displayed after calling the common terms and conditions API.
    * **GamebaseResponse.Terms.ShowTermsViewResult**

<a id="340-2022-02-22-feature-updates"></a>
#### Feature Updates
* The following field has been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **GamebaseConfiguration.enableKickoutPopup**

<a id="340-2022-02-22-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2-34-0-2022-02-22)
* [Gamebase iOS SDK 2.34.0](./release-notes-ios/#2-34-0-2022-02-22)

<a id="2-33-0-2022-01-25"></a>
### 2.33.0 (2022. 01. 25.) { #2-33-0-2022-01-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unity.zip)

<a id="330-20220125-added-features"></a>
#### Added Features
* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > Unity SDK User Guide > UI > Terms > showTermsView](./unity-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### Feature Updates
* Added and changed error codes
    * Changed the error code mapped to the GamebaseErrorCode.UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the GamebaseErrorCode.SOCKET_UNKNOWN_ERROR error mapped to the error code 999.
    
<a id="330-20220125-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2-33-0-2022-01-25)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2-33-0-2022-01-25)

<a id="2-32-0-2021-12-28"></a>
### 2.32.0 (2021. 12. 28.) { #2-32-0-2021-12-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Unity.zip)

<a id="320-20211228-feature-updates"></a>
#### Feature Updates
* Added the **GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED** type to GamebaseEventCategory of GamebaseEventHandler.
    * Please refer to the following document for how to use this event.
    * [Game > Gamebase > Unity SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./unity-etc/#server-push)
* Added the **GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category, which works when Gamebase Access Token expires and login is required.
    * [Game > Gamebase > Unity SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unity-etc/#logged-out)

<a id="320-20211228-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.32.0](./release-notes-android/#2-32-0-2021-12-28)
* [Gamebase iOS SDK 2.32.0](./release-notes-ios/#2-32-0-2021-12-28)

<a id="2-31-0-2021-12-14"></a>
### 2.31.0 (2021. 12. 14.) { #2-31-0-2021-12-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Unity.zip)

<a id="310-20211214-feature-updates"></a>
#### Feature Updates
* External SDK update: TOAST Unity SDK(0.25.0)
* Added a feature in the Standalone maintenance pop-up to dynamically set whether to display the maintenance time.
* Setting Tool
    * Added PAYCO IDP.
    * You must delete the existing Setting Tool completely and then reinstall the tool.

<a id="310-20211214-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.31.0](./release-notes-android/#2-31-0-2021-12-14)
* [Gamebase iOS SDK 2.31.0](./release-notes-ios/#2-31-0-2021-12-14)

<a id="2-30-0-2021-11-23"></a>
### 2.30.0 (2021. 11. 23.) { #2-30-0-2021-11-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Unity.zip)

<a id="300-20211123-added-features"></a>
#### Added Features
* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > Unity SDK User Guide > Authentication > Mapping > Add Mapping Forcibly](./unity-authentication/#add-mapping-forcibly)
* Added an API that allows you to log in to the corresponding account when an AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) error occurs after calling Gamebase.AddMapping().
    * [Game > Gamebase > Unity SDK User Guide > Authentication > Mapping > Change Login with ForcingMappingTicket](./unity-authentication/#change-login-with-forcingmappingticket)

<a id="300-20211123-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.30.0](./release-notes-android/#2-30-0-2021-11-23)
* [Gamebase iOS SDK 2.30.0](./release-notes-ios/#2-30-0-2021-11-23)

<a id="2-29-0-2021-11-09"></a>
### 2.29.0 (2021. 11. 09.) { #2-29-0-2021-11-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Unity.zip)

<a id="290-20211109-feature-updates"></a>
#### Feature Updates
* External SDK update: TOAST Unity SDK(0.23.5)
* Setting Tool
    * v2.0.0 has been released.
    * You must reinstall the tool after removing the previous version of SettingTool completely.
    * Refer to the guide below for what has changed and how to use it.
        * [Game > Gamebase > Unity SDK User Guide > Getting Started > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

<a id="290-20211109-bug-fixes"></a>
#### Bug Fixes
* Fixed a typo of Finnish language on GamebaseDisplayLanguageCode
    * Finish → Finnish

<a id="290-20211109-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.29.0](./release-notes-android/#2-29-0-2021-11-09)
* [Gamebase iOS SDK 2.29.0](./release-notes-ios/#2-29-0-2021-11-09)

<a id="2-28-1-2021-10-26"></a>
### 2.28.1 (2021. 10. 26.) { #2-28-1-2021-10-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.1/GamebaseSDK-Unity.zip)

<a id="281-20211026-bug-fixes"></a>
#### Bug Fixes
* (Android) Fixed an issue where DisplayLanguage was set to an incorrect value if not set.
* (Standalone) Fixed a timeout error that occurred when it took a long time in the previous frame.

<a id="2-28-0-2021-09-28"></a>
### 2.28.0 (2021. 09. 28.) { #2-28-0-2021-09-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Unity.zip)

<a id="280-20210928-added-features"></a>
#### Added Features
* Added Kakaogame authentication
* Added a 'purchase abuse automatic release' function.
    * [Game > Gamebase > Unity SDK User Guide > Authentication > GraceBan](./unity-authentication/#graceban)
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always call the AuthToken.member.graceBanInfo API after login. If a valid GraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

<a id="280-20210928-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.28.0](./release-notes-android/#2-28-0-2021-09-28)
* [Gamebase iOS SDK 2.28.0](./release-notes-ios/#2-28-0-2021-09-28)

<a id="2-27-1-2021-09-14"></a>
### 2.27.1 (2021. 09. 14.) { #2-27-1-2021-09-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Unity.zip)

<a id="271-20210914-feature-updates"></a>
#### Feature Updates
* Improved the Display Language feature.
     * So far, the default language code was **en**. It has been improved to reflect the default language set in the Gamebase console.
         * [Game > Gamebase > Console User Guide > App > App > Language Settings](./oper-app/#language-settings)

<a id="271-20210914-bug-fixes"></a>
#### Bug Fixes
* Fixed a bug where the 'Unregistered Game Version' error pop-up was displayed only in English.
* Fixed a bug where the Chinese text was not displayed in the maintenance pop-up.

<a id="271-20210914-platform-specific-changes"></a>
#### Platform-specific Changes
* [Gamebase Android SDK 2.27.1](./release-notes-android/#2-27-1-2021-09-14)
* [Gamebase iOS SDK 2.27.1](./release-notes-ios/#2-27-1-2021-09-14)

<a id="2-27-0-2021-08-24"></a>
### 2.27.0 (2021. 08. 24.) { #2-27-0-2021-08-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Unity.zip)

<a id="270-20210824-feature-updates"></a>
#### Feature Updates
* Updated the external SDK: TOAST Unity SDK (0.27.1)
* Added ONE Store V16 store

<a id="270-20210824-bug-fixes"></a>
#### Bug Fixes
* Removed erroneously added files in Unity SDK 2.25.0
    * Path: Assets/Gamebase/Toast/IAP/Plugins

<a id="2-26-0-2021-08-10"></a>
### 2.26.0 (2021. 08. 10.) { #2-26-0-2021-08-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unity.zip)

<a id="260-20210810-feature-updates"></a>
#### Feature Updates
* Improved the Display Language feature.
    * Simplified Chinese (zh-CN), Traditional Chinese (zh-TW), and Thai (th) have been added to the Display Language language set.
* Changed the creation criteria of the PushConfiguration object that can be created after calling the showTermsView API as follows.
    * Before change
        * A valid non-null PushConfiguration was returned only when **Receive Push Notification** item exists in the terms and conditions.
        * PushConfiguration.pushEnabled was created as false when the user declines to receive both daytime and nighttime promotional push notifications.
    * After change
        * A valid non-null PushConfiguration is always returned if the terms and conditions UI was displayed.
        * The pushEnabled value of the PushConfiguration object returned by showTermsView is always true.
    * Same point without change
        * PushConfiguration is returned as null if the user has already agreed to the terms and conditions and the terms and conditions UI was not displayed.

<a id="260-20210810-bug-fixes"></a>
#### Bug Fixes
* Fixed an issue where the language code of the message sent from the Push console does not match because the language code of the device is applied to the Push notification language setting without any extra processing.


<a id="2-25-0-2021-07-26"></a>
### 2.25.0 (2021. 07. 26.) { #2-25-0-2021-07-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Unity.zip)

<a id="250-20210726-added-features"></a>
#### Added Features
* Added a monthly payment limit feature
    * If the monthly payment limit is exceeded, the **PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

<a id="250-20210726-feature-updates"></a>
#### Feature Updates
* The existence of a PushConfiguration object is ensured in terms and conditions with Push notification items
    * Until now, the PushConfiguration created as a result of calling the Gamebase.Terms.ShowTermsView API was null if the user declines to receive Push notifications in the terms and conditions UI. However, it has been changed so that the PushConfiguration object is always returned if there are Push notification items in the terms and conditions.
    * When user rejects push notifications, the PushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * If there is no Push notification item in the terms and conditions, the PushConfiguration object is null.
* Changed the minimum supported version of Unity: 2018.4.0f1
* External SDK Update: TOAST Unity SDK(0.23.0)

<a id="2-24-0-2021-06-29"></a>
### 2.24.0 (2021. 06. 29.) { #2-24-0-2021-06-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-1-feature-updates"></a>
#### Feature Updates
* Change the internal launch URL

<a id="2-23-0-2021-06-14"></a>
### 2.23.0 (2021. 06. 14.) { #2-23-0-2021-06-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-2-feature-updates"></a>
#### Feature Updates
* External SDK Update: TOAST Unity SDK(0.22.1)
* Removed warnings from Unity 2020.2 or later
* Improved initialization speed in Standalone and Unity Editor

<a id="game-gamebase-release-notes-unity-2-bug-fixes"></a>
#### Bug Fixes
* Fixed a problem where the PushConfiguration result is not null when ShowTermsView API is called even after agreeing to the terms

<a id="2-22-0-2021-05-25"></a>
### 2.22.0 (2021. 05. 25.) { #2-22-0-2021-05-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-3-feature-updates"></a>
#### Feature Updates
* Updated the external SDK: TOAST Unity SDK(0.22.0)

<a id="2-21-0-2021-04-13"></a>
### 2.21.0 (2021. 04. 13.) { #2-21-0-2021-04-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-4-more-features"></a>
#### More Features
* Japanese authentication for Hangame added.	

<a id="2-20-0-2021-02-09"></a>
### 2.20.0 (2021. 02. 09.) { #2-20-0-2021-02-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Unity.zip)

<a id="game-gamebase-release-notes-unity-5-more-features"></a>
#### More Features
* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

<a id="game-gamebase-release-notes-unity-5-feature-updates"></a>
#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).
* Removed warning logs
* Updated CEF 2.1.2 for Standalone WebView
	* Fixed an issue where a crash occurs when the URL length is longer than 2,048
	* Changed the library path to improve PostProcessBuild when building in Unity 2019
	* Fixed an occasional error arising out of not initializing the string
	* Fixed a bug where the webview does not open again after moving to another scene while using the GameBase webview

<a id="2-19-0-2020-12-29"></a>
### 2.19.0 (December 29, 2020) { #2-19-0-2020-12-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unity.zip)

<a id="190-december-29-2020-more-features"></a>
#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	
<a id="190-december-29-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

<a id="190-december-29-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) WebSocket에서 재시도 시 OutOfMemoryException이 발생하는 문제 수정

<a id="2-18-2-2020-12-15"></a>
### 2.18.2 (December 15, 2020) { #2-18-2-2020-12-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Unity.zip)

<a id="182-december-15-2020-more-features"></a>
#### More Features
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

<a id="182-december-15-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.18.2
    * (Common) TOAST SDK update: [Android(0.24.2)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-android/#0242-november-24-2020), [iOS(0.27.1)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) External SDK update to resolve encryption logic security warnings: PAYCO Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView: Returns error when invalid URL is delivered, delivered URL is used as is without encoding
	* (iOS) Changed to run the custom scheme regardless of letter case
	* (Unity) The following fields of GamebaseRequest.GamebaseConfiguration class deprecated: zoneType, fcmSenderId

<a id="182-december-15-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

<a id="2-18-0-2020-11-10"></a>
### 2.18.0 (November 10, 2020) { #2-18-0-2020-11-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-Unity.zip)

<a id="180-november-10-2020-more-features"></a>
#### More Features
* Added Galaxy Store: SDK 2.18.0

<a id="180-november-10-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: [Android(0.24.1)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-android/#0240-october-27-2020) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings
    * (iOS) Added API that supports the SceneDelegate of iOS 13 or higher

<a id="180-november-10-2020-bug-fixes"></a>
#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

<a id="2-17-1-2020-10-27"></a>
### 2.17.1 (October 27, 2020) { #2-17-1-2020-10-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Unity.zip)

<a id="171-october-27-2020-more-features"></a>
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

<a id="171-october-27-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.17.1
    * (Unity) Supports Unity 2017.2.5

<a id="171-october-27-2020-bug-fixes"></a>
#### Bug Fixes  
* [SDK] 2.17.1
    * (Unity) Fixed an issue where the latter API would not work when the image notification API and web view API were called in turn.

<a id="2-17-0-2020-10-13"></a>
### 2.17.0 (October 13, 2020 ) { #2-17-0-2020-10-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-Unity.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

<a id="170-october-13-2020-more-features"></a>
#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

<a id="170-october-13-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.17.0
	* (Common) Supports the download feature when a Customer Center attachment image is clicked
	* (Common) TOAST SDK update: Android(0.23.2), Unity(0.21.2)
	* (iOS) Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long.
	* (iOS) Changed the logic so that the title can be displayed again after changing URL and title in a web view

<a id="170-october-13-2020-bug-fixes"></a>
#### Bug Fixes  
* [SDK] 2.17.0
	* (iOS) PAYCO authentication: Fixed an issue where the logout callback would not return when logout was called after lastLoggedInProvider login
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
<a id="2-16-0-2020-09-22"></a>
### 2.16.0 (September 22, 2020) { #2-16-0-2020-09-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Unity.zip)

<a id="160-september-22-2020-more-features"></a>
#### More Features
* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
<a id="2-15-0-2020-08-25"></a>
### 2.15.0 (August 25, 2020) { #2-15-0-2020-08-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unity.zip)

```
Updated Google Billing Client in the Gamebase SDK 2.15.0 version. 

For 'gamebase-adapter-purchase-google', to upgrade a version below Gamebase SDK 2.15.0 to more than 2.15.0,  
set 'Requires Update' for 'Game Client Version' of the previous version.

This is because, in order to execute reprocessing when an error occurs during purchasing an item,  
you may encounter an issue during reprocessing if a different billing client version is applied to each of many devices.   
```

<a id="150-august-25-2020-more-features"></a>
#### More Features
* [SDK] 2.15.0
    * (Common) Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting  
    * (Common) Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)

<a id="150-august-25-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) Added the null check logic for the payload of payment

<a id="2-14-0-2020-08-11"></a>
### 2.14.0 (August 11, 2020) { #2-14-0-2020-08-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-Unity.zip)

<a id="140-august-11-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.14.0
    * (iOS) Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings 
    * (iOS, Unity) Adde the contentMode setting for TCGBWebViewConfiguration

<a id="2-13-0-2020-07-28"></a>
### 2.13.0 (July 28, 2020) { #2-13-0-2020-07-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Unity.zip)

<a id="130-july-28-2020-more-features"></a>
#### More Features
* [SDK] 2.13.0
    * (Unity) Standalone: Added Show Notice on Image API     

<a id="130-july-28-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 
    * (iOS) Authenticate Sign In With Apple: Supported for iOS 12 or lower 

<a id="130-july-28-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

<a id="2-12-0-2020-07-14"></a>
### 2.12.0 (July 14, 2020) { #2-12-0-2020-07-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Unity.zip)

<a id="120-july-14-2020-more-features"></a>
#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order
    * [SDK] 2.12.0: Added Show Image Notice API 

<a id="120-july-14-2020-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with storeCode(default=AS) set for configuration 
    * (iOS) Fixed failed closing due to lack of the close button while printing webview which cannot load content 
    * (Unity) Updated TOAST Unity SDK (0.20.1.1)
    
<a id="2-11-0-2020-06-23"></a>
### 2.11.0 (June 23, 2020) { #2-11-0-2020-06-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Unity.zip)

<a id="110-june-23-2020-more-features"></a>
#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

<a id="2-10-1-2020-06-09"></a>
### 2.10.1 (June 9, 2020) { #2-10-1-2020-06-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-Unity.zip)

<a id="101-june-9-2020-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.10.1
	* (iOS) Updated to set device language if language code is not configured when user push setting is initialized 

<a id="101-june-9-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.10.1
	* (Unity) Fixed failed login calls since ViewController is not configured at iOS Plugin 

<a id="2-10-0-2020-05-26"></a>
### 2.10.0 (May 26, 2020) { #2-10-0-2020-05-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Unity.zip)

<a id="100-may-26-2020-more-features"></a>
#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

<a id="100-may-26-2020-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.10.0 
	* (Unity) Updated CefWebview version with StandaloneWebviewAdapter: v2.0.4
		* Updated the logic of WebviewIndex validation  
		* Fixed infrequent error of NullReferenceException while Webview is created 
    * (Unity) GamebaseErrorCode에 소켓 연결에 관한 에러 코드 추가: SOCKET_CONNECTION_TIMEOUT, SOCKET_CONNECTION_FAIL

<a id="100-may-26-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.9.1
    * (Andoird) 매핑 이후 지표 레벨이 null이 되어 결제 지표에 정상적으로 반영되지 않는 오류 수정
    * (iOS) unreal 엔진에서 빌드 하면, warning을 빌드 오류로 판정해서 빌드가 안되는 부분을 수정

<a id="2-9-1-2020-04-29"></a>
### 2.9.1 (April 29, 2020) { #2-9-1-2020-04-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unity.zip)

<a id="91-april-29-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.9.1 
	* (Unity) Fixed a error which occurs when client service status is changed after initialized on console 
		* Versions at Issue: v2.8.0 or higher	
		* Platforms at Issue: Standalone, WebGL, and Editor

<a id="2-9-0-2020-04-28"></a>
### 2.9.0 (April 28, 2020) { #2-9-0-2020-04-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unity.zip)

<a id="90-april-28-2020-more-features"></a>
#### More Features
* Suspension of Membership Withdrawal 
	* [SDK 2.9.0]
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

<a id="90-april-28-2020-feature-updates"></a>
#### Feature Updates
* [SDK 2.9.0]
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated PAYCO Login SDK: Android(v1.5.0), iOS(v1.4.0)

<a id="2-8-1-2020-04-14"></a>
### 2.8.1 (April 14, 2020) { #2-8-1-2020-04-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Unity.zip)

<a id="81-april-14-2020-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	
<a id="2-8-0-2020-03-24"></a>
### 2.8.0 (March 24, 2020) { #2-8-0-2020-03-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Unity.zip)

<a id="80-march-24-2020-more-features"></a>
#### More Features 
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 
	* (Unity) Updated CefWebview to v2.0.1 within StandaloneWebviewAdapter 
		* When the PopupType is PASS_INFO, popup data can be delivered without a popup 

<a id="80-march-24-2020-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 

<a id="2-7-2-2020-03-10"></a>
### 2.7.2 (March 10, 2020) { #2-7-2-2020-03-10 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Unity.zip)

<a id="72-march-10-2020-feature-updates"></a>
#### Feature Updates
- [SDK] 2.7.2 
  	- (Unity) Updated FacebookAdapter  
    		- Compatibility testig from v7.9.4 to v7.18.1 
    		- Null exception handling   
  	- (Unity) Updated StandaloneWebviewAdapter 
    		- Added the feature of exporting webpages in texture 
    		- Support multiple webviews  
    		- Added the option of deleting cookies  
    		- Supports sizing of texture  
		- Supports showing/hiding the scrollbar 
    		- Notifies the completion of page loading  
    		- Supports transparent background
  	- (Unity) Fixed an error which occurs when Android/iOS is selected from Editor and Initialize API is called 

<a id="2-7-0-2020-01-21"></a>
### 2.7.0 (January 21, 2020) { #2-7-0-2020-01-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Unity.zip)

<a id="70-january-21-2020-more-features"></a>
#### More Features
* [SDK] 2.7.0
	* (Unity) Supports NaverCafePLUG 

<a id="70-january-21-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 
	* (Unity) Added the gamebase://dismiss scheme handling for a web login
	* (Unity) Modified infrequent failure in the display of webview for a release build 	

<a id="2-6-3-2020-01-14"></a>
### 2.6.3 (January 14, 2020) { #2-6-3-2020-01-14 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.3/GamebaseSDK-Unity.zip)

<a id="63-january-14-2020-feature-updates"></a>
#### Feature Updates
* [SDK] 2.6.3
	* (Unity) Updated Standalone Webview: Updated CefWebview 	
	* (Unity) Added .dll file missing from an error occured after login 
		* ToastCommon.dll, vcruntime140.dll

<a id="63-january-14-2020-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.6.3
	* (Unity) Fixed error that occur when Login(CredentialInfo) API is called
	
<a id="2-6-2-2019-12-24"></a>
### 2.6.2 (December 24, 2019) { #2-6-2-2019-12-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Unity.zip)

<a id="62-december-24-2019-more-features"></a>
#### More Features
* Conpon > Publish: Added the feature of keyword coupons

<a id="62-december-24-2019-feature-updates"></a>
#### Feature Updates
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) NAVER SDK Updates (4.1.0)

<a id="2-6-1-2019-11-20"></a>
### 2.6.1 (November 20, 2019) { #2-6-1-2019-11-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Unity.zip)

<a id="61-november-20-2019-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.6.1
  * (Unity) Added the iOS Plugin file (toast_sdk_wrap.m) to Package in order to prevent errors for an iOS build  
  * (Unity) Fixed the error of UnityEditor in which the store code comes as empty on a platform other than Standalone, resulting in failed initialization  
  * (Unity) Fixed the error of NullReferenceException due to errors from processing zone type within Initialize API 

<a id="november-13-2019"></a>
### November 13, 2019 { #november-13-2019 }

<a id="november-13-2019-bug-fixes"></a>
#### Bug Fixes
* GamebaseSettingTool
  * Fixed the error in which files are not properly updated, with the version updated to Gamebase v2.6.0


<a id="2-6-0-2019-11-12"></a>
### 2.6.0 (November 12, 2019) { #2-6-0-2019-11-12 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Unity.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

<a id="60-november-12-2019-more-features"></a>
#### More Features 
* Added the payment feature for Google subscription  
  * [SDK] 2.6.0 Android
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (iOS) Added authentication for Sign In with Apple 
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase. 

<a id="60-november-12-2019-feature-updates"></a>
#### Feature Updates 
* [SDK] 2.6.0
  * (Unity) Fixed the error in the logic of updating LaunchingStatus for a login 
  * (Unity) Fixed the error of endless repetition of log delivery from the client, if delivery of debug logs is set on the Gamebase console

<a id="october-15-2019"></a>
### October 15, 2019 { #october-15-2019 }
<a id="october-15-2019-feature-updates"></a>
#### Feature Updates 
* Sample App
	* Updated Gamebase SDK (v2.4.0)
	* Applied Smart Downloader (v1.5.8) and Leaderboard 
	* More Features: Downloading game resources, integrating Leaderboard and TAA indicators, transferring devices, and forced mapping 
	* Updates: Added ServerPush listeners and detection of observer maintenance  
	* Renewed games 
		
<a id="2-5-0-2019-08-27"></a>
### 2.5.0 (August 27, 2019) { #2-5-0-2019-08-27 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Unity.zip)

<a id="50-august-27-2019-more-features"></a>
#### More Features 
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
<a id="august-2-2019"></a>
### August 2, 2019 { #august-2-2019 }
<a id="august-2-2019-bug-fixes"></a>
#### Bug Fixes 
* [SDK] Setting Tool 1.4.3
	* Fixed the build error by moving down the script file below the editor folder  
	* Fixed failed operations when Multilanguage is provided with the entire path of the language file on MAC OS. 

<a id="2-4-4-2019-07-23"></a>
### 2.4.4 (July 23, 2019) { #2-4-4-2019-07-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Unity.zip)

<a id="44-july-23-2019-feature-updates"></a>
#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)
* Setting Tool
	* Change of Folder Structure: Must reinstall after previous SettingTool is completely deleted  
	* More languages are supported 
	
<a id="2-4-3-2019-07-11"></a>
### 2.4.3 (July 11, 2019) { #2-4-3-2019-07-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-Unity.zip)

<a id="43-july-11-2019-bug-fixes"></a>
#### Bug Fixes 
* [SDK] 2.4.3
	* (Unity) Fixed failed operations of AddMappingForcibly API, for the builds in iOS or Android 
	* (Unity) Fixed the json parsing error at iOSPlugin when RequestRetryTransaction API is called 

<a id="june-272019"></a>
### June 27,2019 { #june-272019 }

<a id="june-272019-bug-fixes"></a>
#### Bug Fixes
* [SDK] Setting Tool 1.4.1
	* Fixed the error in uploading existing setting data when GamebaseSettingTool was executed

<a id="2-4-2-2019-06-25"></a>
### 2.4.2 (June 25, 2019) { #2-4-2-2019-06-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Unity.zip)

<a id="42-june-25-2019-features-updateschanges"></a>
#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

<a id="42-june-25-2019-bug-fixes"></a>
#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 

<a id="2-4-0-2019-05-28"></a>
### 2.4.0 (May 28, 2019) { #2-4-0-2019-05-28 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Unity.zip)

<a id="40-may-28-2019-feature-updates"></a>
#### Feature Updates 
* Purchase for HANGAME mix Available for Japan 
    * [SDK] 2.4.0
      * (Unity) Added external purchase for Standalone Japan  
      * (Unity)  Added HANGAME authentication for Standalone Japan 

<a id="40-may-28-2019-feature-updateschanges"></a>
#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Change of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]

    * (Android) NAVER SDK Version Updated (v4.2.5): Bug of NAVER SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while NAVER login was underway)  
    * (Unity) StandaloneWebview supports 32bit Build (SDK volume upgraded from 53.6MB to 99.2MB)

<a id="2-3-0-2019-04-23"></a>
### 2.3.0 (2019. 04. 23.) { #2-3-0-2019-04-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Unity.zip)

```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

<a id="30-20190423-1"></a>
#### 기능 추가
* [SDK] 2.3.0
	* (Android/Unity)중국스토어 인증/결제 추가

<a id="30-20190423-2"></a>
#### 기능 개선/변경
* [SDK] 2.3.0
	* (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"

<a id="2-2-2-2019-04-11"></a>
### 2.2.2 (2019. 04. 11.) { #2-2-2-2019-04-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Unity.zip)

<a id="22-20190411-1"></a>
#### 기능 개선/변경
* [SDK] 2.2.2
	* (Unity)SDK 로그 개선

<a id="22-20190411-2"></a>
#### 버그수정
* [SDK] 2.2.2
	* (Unity)AddMappingForcibly API를 호출하면 크래쉬가 발생하여 수정

<a id="2-2-1-2019-04-02"></a>
### 2.2.1 (2019. 04. 02.) { #2-2-1-2019-04-02 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.1/GamebaseSDK-Unity.zip)
<a id="21-20190402-1"></a>
#### 버그수정
* [SDK] 2.2.1
	* (Unity) Unity Editor에서 Android 플랫폼을 선택하고 플레이를 하면 initialize시 서버에서 에러가 발생하는 이슈 수정

<a id="2-2-0-2019-03-26"></a>
### 2.2.0 (2019. 03. 26.) { #2-2-0-2019-03-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Unity.zip)
<a id="20-20190326-1"></a>
#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * TransferAccountInfo 발급 API (issueTransferAccount)
        * 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
        * 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
        * 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)        
* 강제매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
    * (SDK공통)추가된 API 
        * 강제매핑하는 API (addMappingForcibly)

<a id="20-20190326-2"></a>
#### 기능 개선/변경
* [SDK] 2.2.0
	* (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트
	* (iOS)LINE SDK의 App 로그인 기능이 비활성화
		* LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adapter에서 Web 로그인만 지원하도록 변경
	* (Unity)GamebaseMainActivity의 Package Name이 변경
		* com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

<a id="2-1-0-2019-02-26"></a>
### 2.1.0 (2019. 02. 26.) { #2-1-0-2019-02-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Unity.zip)
<a id="10-20190226-1"></a>
#### 기능 개선/변경
* [SDK] 2.1.0
	* (공통)TransferKey API 삭제
		* issueTransferKey : TransferKey 발급
		* requestTransfer : TransferKey 검증

<a id="2-0-0-2019-01-29"></a>
### 2.0.0 (2019. 01. 29.) { #2-0-0-2019-01-29 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Unity.zip)
```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

<a id="00-20190129-1"></a>
#### 기능 추가
* [SDK] 2.0.0
	* (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
		* setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
		* traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출

<a id="1-14-2-2018-11-15"></a>
### 1.14.2 (2018. 11. 15.) { #1-14-2-2018-11-15 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.1/GamebaseSDK-Unity.zip)
<a id="142-20181115-1"></a>
#### 기능 개선/변경
* [SDK] 1.14.2
	* (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정
	* (iOS)Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 crash가 발생될 수 있는 구조 수정

<a id="142-20181115-2"></a>
#### 버그수정
* [SDK] 1.14.2
	* (Unity)ShowWebView API 호출시 파라메타에 Callback을 넣지 않으면 crash가 발생되는 부분 수정
	* (Unity)iOS SDK의 Deleted API를 호출하는 코드가 있어 컴파일시 오류가 발생 되는 버그 수정

<a id="1-14-0-2018-10-23"></a>
### 1.14.0 (2018. 10. 23.) { #1-14-0-2018-10-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-Unity.zip)

<a id="140-20181023-1"></a>
#### 기능 추가
* [SDK] 1.14.0
    * (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
    
<a id="140-20181023-2"></a>
#### 기능 개선/변경
* [SDK] 1.14.0
    * (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
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
    
<a id="1-13-0-2018-09-13"></a>
### 1.13.0 (2018. 09. 13.) { #1-13-0-2018-09-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Unity.zip)

<a id="130-20180913-1"></a>
#### 기능 개선/변경
* [SDK] 1.13.0
    * (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
    * (Unity)로그에서 보여주는 json 데이터를 알아보기 쉽도록 출력 포맷 개선
    
<a id="130-20180913-2"></a>
#### 버그수정
* [SDK] 1.13.0
    * (Unity)Unity 2017.2 이상 버전에서 Editor Play Mode 종료 시 websocke close 처리에서 발생하던 오류 수정
      
<a id="1-12-1-2018-08-09"></a>
### 1.12.1 (2018. 08. 09.) { #1-12-1-2018-08-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-Unity.zip)

<a id="121-20180809-1"></a>
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
    * 에러발생시 팝업 창 알림 추가 : File Download 실패, File Extract 실패, XML 파싱 실패
    
<a id="1-12-0-2018-07-24"></a>
### 1.12.0 (2018. 07. 24.) { #1-12-0-2018-07-24 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-Unity.zip)

<a id="120-20180724-1"></a>
#### 기능 개선/변경
* [SDK] Setting Tool
    * Setting Tool 최신 버전이 있을 경우 업데이트 알림 기능 추가 
    * 내부 null Exception 수정
    
<a id="120-20180724-2"></a>
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

<a id="1-11-0-2018-06-26"></a>
### 1.11.0 (2018. 06. 26.) { #1-11-0-2018-06-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Unity.zip)

<a id="110-20180626-1"></a>
#### 기능 추가
* iOS Google IdP 추가 : iOS
* Twitter IdP 추가 : Android, iOS
* LINE IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
  
<a id="110-20180626-2"></a>
#### 기능 개선/변경
* [SDK] 1.11.0
    * (공통)LocalizedString 일본어 번역 추가
    * (공통)인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
    * (Android)'android.permission.READ_PHONE_STATE' 권한 제거
    * (Android)GamebaseConfiguration.Builder의 필수 설정값인 setAppId, setAppVersion을 생성자에서 입력할 수 있도록 변경
    * (Android)GamebaseConfiguration.Builder 의 setServerApiVerseion API를 제거
    * (Android)getAuthBanInfo() API, class AuthBanInfo 이름을 변경 : getBanInfo(), class BanInfo
    * NAVER ID Login SDK 업데이트 : iOS(4.0.10)
* Sample App 
    * ServerPush 기능 및 Observer 기능 추가
    * Gamebase SDK 업데이트 : Android(1.9.0), iOS(1.9.0), Unity(1.10.1)    
    
<a id="1-10-1-2018-06-11"></a>
### 1.10.1 (2018. 06. 11.) { #1-10-1-2018-06-11 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.1/GamebaseSDK-Unity.zip)

<a id="101-20180611-1"></a>
#### 버그수정
* [SDK] 1.10.1
    * (Unity)Unity Adapter가 없는 경우 AddMapping API 호출 시 내부적으로 로그인으로 처리하던 버그 수정

<a id="1-10-0-2018-06-07"></a>
### 1.10.0 (2018. 06. 07.) { #1-10-0-2018-06-07 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.10.0/GamebaseSDK-Unity.zip)

<a id="100-20180607-1"></a>
#### 기능 추가
* [SDK] 1.10.0
    * (Unity)StandaloneWebviewAdapter: html source rendering 지원    

<a id="100-20180607-2"></a>
#### 기능 개선/변경
* [SDK] 1.10.0
    * (Unity)Unity Adapter의 interface가 수정
        * v1.10.0 이상 사용 시에는 UnityAdapter 버전 업그레이드가 필요(GamebaseUnitySDK_FacebookAdapter_v1.5.0, GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
    * (Unity)Login API 호출 시 Unity Adapter가 없는 경우 네이티브(Android/iOS)의 로그인 API를 호출하도록 로직 변경 : facebook, Google
    * (Unity)각 Adapter 폴더 구조 및 이름 오타 수정
        * 경로: Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
        * 오타: Adapater => Adapter    
    
<a id="1-9-0-2018-05-18"></a>
### 1.9.0 (2018. 05. 18.) { #1-9-0-2018-05-18 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

<a id="90-20180518-1"></a>
#### 기능 개선/변경
* [SDK] 1.9.0
    * Unity SDK(1.9.0) Google Adapter 신규버전(1.6.2)으로 교체하여 재배포
        * 5/3 배포된 Unity SDK(1.9.0)에 적용된 Google Adapter를 최신버전으로 교체(1.6.1->1.6.2)
    
<a id="1-9-0-2018-05-03"></a>
### 1.9.0 (2018. 05. 03.) { #1-9-0-2018-05-03 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Unity.zip)

<a id="90-20180503-1"></a>
#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)
* 이용정지 등록시 사용자의 리더보드(랭킹) 데이터를 삭제할 수 있는 옵션 추가(TOAST Leaderboard를 사용하는 경우에 한함)
    * 이용정지 등록 메뉴를 이용하거나 App Guard 연동 페이지에서 사용 가능

<a id="1-8-1-2018-04-09"></a>
### 1.8.1 (2018. 04. 09.) { #1-8-1-2018-04-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Unity.zip)
<a id="81-20180409-1"></a>
#### 버그 수정
* [SDK] 1.8.1
    * (Unity)UnityAndroid 플랫폼에서 아래 기능 사용 시 모듈 초기화가 되지 않아 NullReferenceException이 발생하여 수정
        * Launching, Purchase, Push, Util, Webview

<a id="1-8-0-2018-04-05"></a>
### 1.8.0 (2018. 04. 05.) { #1-8-0-2018-04-05 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Unity.zip)

<a id="80-20180405-1"></a>
#### 기능 추가
* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    * 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    * 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    * (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

<a id="80-20180405-2"></a>
#### 기능 개선/변경
* [SDK] 1.8.0
    * (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
    * (iOS)페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
    * (iOS)iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정

<a id="80-20180405-3"></a>
#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정
* [SDK] 1.8.0
    * (Setting Tool)Unity Facebook Adapter를 체크하면 에러가 나는 버그 수정

<a id="1-7-1-2018-03-13"></a>
### 1.7.1 (2018. 03. 13.) { #1-7-1-2018-03-13 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.1/GamebaseSDK-Unity.zip)

<a id="71-20180313-1"></a>
#### 버그 수정
* [SDK] 1.7.1
    * (Unity)Inspector에서 설정된 SetDebugMode 값이 반영 안 되던 버그 수정
    * (Unity)Standalone, WebGL: Display Language에서 사용되는 리소스 파일 누락 부분 수정
    * (Unity)Google Adapter 1.6.2 배포: Google Adapter 1.6.1에서 AuthCode가 Empty로 반환되어 인증 실패하는 버그 수정

<a id="1-7-0-2018-02-22"></a>
### 1.7.0 (2018. 02. 22.) { #1-7-0-2018-02-22 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Unity.zip)
<a id="70-20180222-1"></a>
#### 기능 추가
* [SDK] 1.7.0
    * NAVER IdP 인증 추가
    * Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

<a id="1-6-0-2018-01-25"></a>
### 1.6.0 (2018. 01. 25.) { #1-6-0-2018-01-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-Unity.zip)

<a id="60-20180125-1"></a>
#### 기능 추가
* [SDK] 1.6.0
    * (Unity)Standalone WinSDK 추가
        * 64비트 지원
        * 인증 지원 : facebook, google, payco

<a id="1-5-0-2017-12-21"></a>
### 1.5.0 (2017. 12. 21.) { #1-5-0-2017-12-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Unity.zip)

<a id="50-20171221-1"></a>
#### 기능 추가
* [SDK] 1.5.0
    * WebView가 닫힐 때 발생하는 Close Callback 추가
    * WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
    * Unity Setting Tool 신규 배포

<a id="50-20171221-2"></a>
#### 버그 수정
* [SDK] 1.5.0
    * (Unity)UnityEditor에서 Guest로그인이 되지 않는 현상 수정
    * (Unity)TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가

<a id="1-4-0-2017-11-23"></a>
### 1.4.0 (2017. 11. 23.) { #1-4-0-2017-11-23 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Unity.zip)

<a id="40-20171123-1"></a>
#### 기능 추가
* [SDK] 1.4.0 업데이트
    * (Unity)Gamebase Facebook Adapter가 추가 : Android, iOS, WebGL, Standalone Platform 및 UnityEditor 지원

<a id="1-3-0-2017-10-26"></a>
### 1.3.0 (2017. 10. 26.) { #1-3-0-2017-10-26 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Unity.zip)

<a id="30-20171026-1"></a>
#### 기능 추가
* [SDK] 1.3.0 업데이트
    * Credential을 이용한 AddMapping API추가

<a id="30-20171026-2"></a>
#### 기능 개선/변경
* [SDK] 1.3.0 업데이트
    * (Unity)CredentialInfo를 사용하는 Login API호출 시 iOSPlugin에서 Json 파싱이 안되던 버그를 수정

<a id="1-2-0-2017-09-21"></a>
### 1.2.0 (2017. 09. 21.) { #1-2-0-2017-09-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Unity.zip)

<a id="20-20170921-1"></a>
#### 기능 추가
* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
    * 이용정지 사용자 팝업 창 노출

<a id="1-1-5-2017-07-20"></a>
### 1.1.5 (2017. 07. 20.) { #1-1-5-2017-07-20 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Unity.zip)

<a id="15-20170720-1"></a>
#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
    * 시스템 팝업 창 API 추가 (showAlertWithTitle)
    * 국가코드를 대문자로 반환하도록 변경 (Android)
    * TCPush SDK 1.4.1 로 업데이트
    * IAP SDK 1.3.3.20170627 로 업데이트

<a id="1-1-4-2017-05-25"></a>
### 1.1.4 (2017. 05. 25.) { #1-1-4-2017-05-25 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Unity.zip)

<a id="14-20170525-1"></a>
#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
    * 런타임 중 결제 Store를 변경할 수 있는 API 제공
    * (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공

<a id="1-1-2-2017-04-04"></a>
### 1.1.2 (2017. 04. 04.) { #1-1-2-2017-04-04 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Unity.zip)

<a id="12-20170404-1"></a>
#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임 론칭시 점검, 긴급공지 팝업 창 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리

<a id="1-1-0-2017-03-21"></a>
### 1.1.0 (2017. 03. 21.) { #1-1-0-2017-03-21 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Unity.zip)

<a id="10-20170321-1"></a>
#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

<a id="1-0-0-2017-03-09"></a>
### 1.0.0 (2017. 03. 09.) { #1-0-0-2017-03-09 }
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Unity.zip)

<a id="00-20170309-1"></a>
#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
    * 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
    * 로그아웃 및 회원탈퇴 기능을 제공
    * 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
    * 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
    * 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
    * TOAST Cloud상품 연동 : PUSH, IAP
