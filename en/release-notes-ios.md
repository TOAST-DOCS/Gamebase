## Game > Gamebase > Release Notes > iOS

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

#### Added Features
* Added the NS_SWIFT_NAME setting to Public API for Swift project users.

#### Feature Updates
* External SDK update: Hangame iOS SDK (1.6.2)
* Fixed an error where, when the showWebView API is called while the device is in landscape mode, a black blank space is displayed at the bottom.

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

#### Added Features
* If you select **Add Popup Button** in the Update Required settings of the Gamebase console, a **Details** button will be added to the client's Update Required popup window.
* Added an API to find out whether the device has allowed notifications or not.
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* Added a VO class that can be used to find out whether the terms and conditions UI was displayed after calling the common terms and conditions API.
    * **TCGBShowTermsViewResult**

#### Feature Updates
* Corrected the issue where the background becomes dark briefly when the Image Notice API has been called but there is no image notice to display
* The following fields have been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**

### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

#### Added Features
* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > iOS SDK User Guide > UI > Terms > showTermsView](./ios-ui/#showtermsview)

#### Feature Updates
* External SDK update: PAYCO iOS SDK (1.5.5)
* Added and changed error codes
    * Changed the error code mapped to the TCGB_ERROR_UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the TCGB_ERROR_SOCKET_UNKNOWN_ERROR error mapped to the error code 999.

### 2.32.1 (2022.01.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* Modified so that, when clicking the **Update Now** button in the Update recommended pop-up window, the pop-up window is not closed.
* Improved the stability of SDK.

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

#### Added Features
* Added the **kTCGBServerPushAppKickoutMessageReceived** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* Added the **kTCGBLoggedOut** type to GamebaseEventCategory of GamebaseEventHandler.
    * [Game > Gamebase > iOS SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

#### Feature Updates
* Changed the default title color of webview navigationBar to **UIColor.whiteColor**.

#### Bug Fixes
* Fixed so that, when calling Hangame logout, thirdIdP is also logged out.

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

#### Added Features
* Added a feature in the maintenance pop-up to dynamically set whether to display the maintenance time.

#### Feature Updates
* External SDK update : TOAST iOS SDK (0.29.2), PAYCO iOS SDK (1.5.4)
* Fixed an issue where it was not possible to register inquiries with banned user information from the Customer Center link in the ban webview.
* Modified so that the Back button is displayed in the maintenance pop-up and ban details webview.

### 2.30.1 (2021.11.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

#### Bug Fixes
* Fixed a bug where errors occur in purchase and push APIs when Cocoapods has been installed in Unity 2019.3 or higher.

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

#### Added Features
* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* Added an API that allows you to change an account to the corresponding IdP when a **TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** error occurs after trying to map to a specific IdP.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > Change Login with ForcingMappingTicket](./ios-authentication/#change-login-with-forcingmappingticket)

#### Bug Fixes
* Fixed a bug where the logout or withdrawal function does not work for a specific IdP after logging in with loginForLastLoggedInProvider.

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* The minimum supported version of Xcode has been changed from 12 to 13.
* External SDK update: TOAST iOS SDK (0.29.1), ToastGamebaseIAP SDK (0.12.1)
* Changed to display the URL of the detailed view of maintenance and notice registered in the console without encoding.

#### Bug Fixes
* Fixed a bug that caused an error when parsing TCGBPushMessage.extras as JSON.

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

#### Added Features
* Added Kakaogame authentication
* Added a 'purchase abuse automatic release' function.
    * [Game > Gamebase > iOS SDK User Guide > Authentication > GraceBan](./ios-authentication/#graceban)
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always check the value of TCGBAuthToken.tcgbMember.graceBanInfo after login. If a valid TCGBGraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

#### Feature Updates
* PAYCO iOS SDK update (1.5.2)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* PAYCO iOS SDK update (1.5.1)
     * Improved authentication flow and UI.
* Hangame iOS SDK update (1.6.1)
     * Fixed an issue where callback could not be called when an error situation occurred in personal verification.
     * Fixed an issue where the navigation bar appears broken in iOS 15 beta.

#### Bug Fixes
* Fixed an issue where PushConfiguration is not returned as nil when the terms and conditions UI is not displayed because the user have already agreed to the terms.

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* Updated PAYCO iOS SDK (1.5.0)
    * So far, only manual login was supported when the PAYCO app is not available. It has been changed so that the quick login feature can be used when the user is logged-in on Safari.

#### Bug Fixes
* Fixed an issue where image notices were not displayed in Unity.
    * If you are using a version lower than Gamebase iOS SDK 2.27.0, image notices may not be displayed in Unity.
    * When using image notices, use Gamebase iOS SDK 2.27.0 or higher.


### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* Improved the Display Language feature.
    * Until now, you had to manually edit the Gamebase.bundle file to add the language set.
        * It has been improved so that you can add a localizedstring.json file to Copy Bundle Resources of Xcode project.
    * Simplified Chinese (zh-CN), Traditional Chinese (zh-TW), and Thai (th) have been added to the Display Language language set.
    * The default language code was **en**, but it has been improved to reflect the default language set in the Gamebase console.
        * [Game > Gamebase > Console User Guide > App > App > Language settings](https://docs.toast.com/en/Game/Gamebase/en/oper-app/#language-settings)
* Changed the creation criteria of the PushConfiguration object that can be created after calling the showTermsView API as follows.
    * Before change
        * A valid non-nil PushConfiguration was returned only when **Receive Push Notification** item exists in the terms and conditions.
        * PushConfiguration.pushEnabled was created as false when the user declines to receive both daytime and nighttime promotional push notifications.
    * After change
        * A valid non-nil PushConfiguration is always returned if the terms and conditions UI was displayed.
        * The pushEnabled value of the PushConfiguration object returned by showTermsView is always true.
    * Same point without change
        * PushConfiguration is returned as nil if the user has already agreed to the terms and conditions and the terms and conditions UI was not displayed.

#### Bug Fixes
* Fixed an issue where the language code of the message sent from the Push console does not match because the language code of the device is applied to the Push notification language setting without any extra processing.

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-iOS.zip)

#### More Features
* Added monthly payment limit feature.
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

#### Feature Updates
* Guaranteed the PushConfiguration object in the terms and conditions with Push notification items.
    * The TCGBPushConfiguration to be created as the result of calling Gamebase.Terms.showTermsView API was null if user did not agreed to receive push notifications in the terms of use. It has now changed so that the TCGBPushConfiguration object is always returned if there is a push notification item in the terms and conditions.
    * When user rejects receiving push notifications, the TCGBPushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * The TCGBPushConfiguration is null when there is no Push notification item in the terms and conditions.
* External SDK Update: TOAST iOS SDK(0.29.0)
* Changed to return TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR error when an ASAuthorizationErrorUnknown error occurs in Sign In with an Apple OS.

#### Bug Fixes
* Fixed a bug where the value of TCGBPushConfiguration and TCGBPushTokenInfo registered through registerPush were different

### 2.24.0 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* Change the internal launch URL

#### Bug Fixes
* Fixed a bug where the terms pop-up did not close after viewing the terms and conditions details

### 2.23.0(2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* Updated the external SDK: TOAST iOS SDK(0.28.0), ToastGamebaseIAP SDK(0.12.0)

### 2.22.0 (2021.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* Updated the external SDK: TOAST iOS SDK(0.27.2), Hangame iOS SDK(1.6.0)

### 2.21.2 (2021.04.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.2/GamebaseSDK-iOS.zip)

#### Feature Updates
* Facebook iOS SDK updated (9.2.0)

#### Bug Fixes
* Fixed an issue where a bitcode error occurs when building an archive

### 2.21.1 (2021.04.19)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-iOS.zip)

#### Bug Fixes
* Fixed an issue where the setting is not properly reflected even if it is set to support bitcode

### 2.21.0 (2021.04.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-iOS.zip)

#### More Features
* Japanese authentication for Hangame added.    

#### Feature Updates
* Changed the system to support bitcode.
* Modified the system to display the Close button first when calling showWebView

### 2.20.2 (2021.03.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-iOS.zip)

#### Feature Updates
* Facebook iOS SDK updated (9.1.0)
* Fixed an issue of failing to call openURL delegate from GamebaseAuthFacebookAdapter in certain cases

### 2.20.1 (2021.03.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* Edited the IDFA acquisition logic in response to iOS 14: added the NSUserTrackingUsageDescription field to info.plist

### 2.20.0 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-iOS.zip)

#### More Features
* Common Terms and Conditions added
    * Added an API that opens the Terms and Conditions webview
    * Added an API that views the Terms and Conditions list and agreement status per user
    * Added an API that saves the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

### 2.19.1 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* Weibo IdPAdapter structure changed    

### January 12, 2021 

```
Changed the minimum supported version of the XCode of Gamebase from ver. 10 to 11. 
```

### 2.19.0 (2020.12.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-iOS.zip)

#### More Features
* [SDK] 2.19.0
    * (Common) Weibo authentication added
    
#### Feature Updates
* [SDK] 2.19.0
    * (Common) Launching status code added: beta service (205)

#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) WebSocket에서 재시도 시 OutOfMemoryException이 발생하는 문제 수정
* [SDK] 2.19.1
    * (Android) Weibo 로그인 시도 후 다른 IdP로 로그인 시 크래시가 발생하는 문제 수정

### 2.18.2 (2020.12.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-iOS.zip)

#### More Features
* [SDK] 2.18.2
    * (Common) additionalURL field added for the case of a developer's own Customer Center being opened
    * (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [SDK] 2.18.2
    * (Common) TOAST SDK update: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
    * (iOS) showWebView: Returns error when invalid URL is delivered, delivered URL is used as is without encoding
    * (iOS) Changed to run the custom scheme regardless of letter case

### 2.18.0 (2020.11.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* [SDK] 2.18.0
    * (iOS) Added API that supports the SceneDelegate of iOS 13 or higher

### 2.17.1 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* [SDK] 2.17.1
    * (iOS) When sending a specific index, an error message is added to it: When push registration fails or when sending a game index

### 2.17.0 (2020.10.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-iOS.zip)


```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
    * (공통) Supports the download feature when a Customer Center attachment image is clicked
    * (공통) TOAST SDK update: Android(0.23.2), Unity(0.21.2)
    * (iOS) Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long.
    * (iOS) Changed the logic so that the title can be displayed again after changing URL and title in a web view

#### Bug Fixes  
* [SDK] 2.17.0
    * (iOS) PAYCO authentication: Fixed an issue where the logout callback would not return when logout was called after lastLoggedInProvider login
    
### 2.16.0 (2020.09.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-iOS.zip)

#### More Features
* Added a feature to Customer Center
    * [SDK] 2.16.0
        * (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
        * (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API

### 2.15.1 (2020.09.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.1/GamebaseSDK-iOS.zip)

#### Feature Updates
* [SDK] 2.15.1
    * (iOS) TOAST SDK update: iOS(0.27.0)
    * A new version of IAP SDK is applied to support the changes made to iOS 14 beta. [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

### 2.15.0 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-iOS.zip)

```
Updated Google Billing Client in the Gamebase SDK 2.15.0 version. 

For 'gamebase-adapter-purchase-google', to upgrade a version below Gamebase SDK 2.15.0 to more than 2.15.0,  
set 'Requires Update' for 'Game Client Version' of the previous version.

This is because, in order to execute reprocessing when an error occurs during purchasing an item,  
you may encounter an issue during reprocessing if a different billing client version is applied to each of many devices.   
```

#### More Features
* [SDK] 2.15.0
    * (Common) Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting  
    * (Common) Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)

#### Feature Updates
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) Added the null check logic for the payload of payment 

### 2.14.0 (2020.08.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* [SDK] 2.14.0
    * (iOS) Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings 
    * (iOS, Unity) Adde the contentMode setting for TCGBWebViewConfiguration

### 2.13.0 (2020.07.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-iOS.zip)

#### Feature Updates
* [SDK] 2.13.0
    * (iOS) Authenticate Sign In With Apple: Supported for iOS 12 or lower 

### 2.12.0 (2020.07.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-iOS.zip)

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order 
    * [SDK] 2.12.0: Added Show Image Notice API 

#### Feature Updates 
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with storeCode(default=AS) set for configuration 
    * (iOS) Fixed failed closing due to lack of the close button while printing webview which cannot load content 
    
### 2.11.0 (2020.06.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-iOS.zip)

#### More Features
* [SDK] 2.11.0
    * Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

### 2.10.1 (2020.06.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-iOS.zip)

#### Feature Updates 
* [SDK] 2.10.1
    * (iOS) Updated to set device language if language code is not configured when user push setting is initialized 

### 2.10.0 (2020.05.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-iOS.zip)

#### More Features
* [SDK] 2.10.0
    * (Common) Added GamebaseEventHandler which has all previous event systems 
        * Includes ServerPush and Observer, and checks promotional purchase or push events 

### 2.9.1 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-iOS.zip)
#### Bug Fixes
* [SDK] 2.9.1
    * (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error 

### 2.9.0 (2020.04.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-iOS.zip)

#### More Features
* Suspension of Membership Withdrawal 
    * [SDK 2.9.0]
        * (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  
#### Feature Updates
* [SDK 2.9.0]
    * (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
    * (Common) Updated Payco Login SDK: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (2020.04.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-iOS.zip)

#### Feature Updates 
* [SDK] 2.8.1 
    * (Common) Added internal indicators to check Analytics delivery results
    
### 2.8.0 (2020.03.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-iOS.zip)

#### More Features 
* [SDK] 2.8.0
    * (Common) Added more purchase and product information, such as product type and regional prices 

#### Feature Updates 
* [SDK] 2.8.0 
    * (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 

### 2.7.1 (2020.02.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-iOS.zip)

#### Feature Updates 
* [SDK] 2.7.1
    * (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called
    
### 2.6.2 (2019.12.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-iOS.zip)

#### More Features
* Conpon > Publish: Added the feature of keyword coupons

#### Feature Updates
* [SDK] 2.6.2
    * (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
    * (iOS) Naver SDK Updates (4.1.0)

### 2.6.1 (2019.12.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-iOS.zip)

#### More Features 
* App > App: Allows to register devices for QA testing via IP as well  

#### Bug Fixes
* [SDK] 2.6.1
  * (iOS) Fixed the issue in which mapping is not available when AddMapping (Forcibly) is applied 
  * (iOS) Fixed crash occurrence by NSNUll object, when displayLanguageCode of PushConfiguration is not set by Unity Plugin 

### 2.6.0 (2019.11.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-iOS.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (iOS) Added authentication for Sign In with Apple 

### 2.5.2 (2019.10.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.2/GamebaseSDK-iOS.zip)

#### Feature Updates 
* [SDK] 2.5.2 
    * (iOS) Changed UIWebView into WKWebView
        
### September 10, 2019 [SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.1/GamebaseSDK-iOS.zip)
    
#### Feature Updates 
* [SDK] 2.5.1
    * (iOS) Updated to TCPushSDK 1.7.0 which is for GamebasePushAdapter
        * Since file has changed from static library to framework for TCPushSDK, TCPushSDK.framework must be added to project. 

### 2.5.0 (August 27, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-iOS.zip)

#### More Features 
* [SDK] 2.5.0
    * Provides API which opens CS URL entered on a console via webview 

### 2.4.3 (July 11, 2019 )
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-iOS.zip)

#### Bug Fixes 
* [SDK] 2.4.3
    * (iOS) Fixed crash occurrence due to parsing attempts of error messages with conflicting formats, regarding authentication

### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-iOS.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
    * (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo
    * (iOS) LINE SDK Updated (v5.0.1)
        * The minimum support OS version for LINE Adapter has changed to iOS 10
        * Login also available via LINE app

#### Bug Fixes
* [SDK] 2.4.2
    * (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 
    * (iOS) Fixed infrequent crashes occurred out of network connection issues 

### 2.4.1 (June 13, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.1/GamebaseSDK-iOS.zip)

#### Bug Fixes
* [SDK] 2.4.1
    * (iOS) Fixed the error in output of indicators due to missing of partial parameters during transfer of Analyticis indicators 
    
### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-iOS.zip)

#### Feature Updates 
* Purchase for HANGAME mix Available for Japan 
    * [SDK] 2.4.0
      * (Unity) Added external purchase for Standalone Japan  
      * (Unity)  Added HANGAME authentication for Standalone Japan 

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Chanage of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#level-up-trace)]
    
### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-iOS.zip)

```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

#### 기능 개선/변경
* [SDK] 2.3.0
    * (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-iOS.zip)

#### 버그수정
* [SDK] 2.2.2
    * (iOS)showBlockingPopup을 NO로 설정 할 경우 Gamebase 초기화 콜백이 호출되지 않는 이슈를 수정

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-iOS.zip)

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

#### 기능 개선/변경
* [SDK] 2.2.0
    * (iOS)LINE SDK의 App 로그인 기능이 비활성화
        * LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adatper에서 Web 로그인만 지원하도록 변경

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 2.1.0
    * (공통)TransferKey API 삭제
        * issueTransferKey : TransferKey 발급
        * requestTransfer : TransferKey 검증
        
#### 버그수정
* [SDK] 2.1.0
    * (iOS)Gamecenter를 Gamebase가 아닌 다른 로직에의해 로그인 한 후, Gamebase를 통하여 Gamecenter로그인을 시도할 때, 반응이 없는 버그 수정

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-iOS.zip)

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* [SDK] 2.0.0
    * (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
        * setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
        * traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출

#### 기능 개선/변경
* [SDK] 2.0.0
    * (iOS)IAP SDK 업데이트
        * 결제 실패 시 간헐적으로 크래시가 발생하던 현상 수정

#### 버그수정
* [SDK] 2.0.0
    * (iOS)iOS 12 이상의 시뮬레이터에서 debugMode On 상태로 Gamebase 초기화 시 크래시가 발생하던 현상 수정

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.14.2
    * (iOS)Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 crash가 발생될 수 있는 구조 수정

### 1.14.0 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* [SDK] 1.14.0
    * (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
    
#### 기능 개선/변경
* [SDK] 1.14.0
    * (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
    * (iOS)Payco SDK의 버전이 1.2.4로 업데이트 
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
    * Deprecated  API 
        * [TCGBGamebase languageCode]

### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* [SDK] 1.13.0
    * (iOS)App Store Promotion IAP를 지원하기 위한 API 추가

#### 기능 개선/변경
* [SDK] 1.13.0
    * (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
    * (iOS)authProviderProfileWithIDPCode api의 호출 결과의 구조가 1depth로 변경 (Android, Unity와 통일)
        
### 1.12.2 (2018.08.28) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.12.2
    * (iOS)Google Auth Adapter, Naver Auth Adapter의 Callback URL Scheme 설정 개선
        * 콘솔에 "url_scheme_ios_only" 값을 설정하지 않으면 Default URL Scheme을 설정 하도록 개선 : Default URL Scheme을 사용하기 위해서는 XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.google 또는 tcgb.{Bundle ID}.naver 등록 필요
    * (iOS)Payco Auth Adapter 개선
        * URL Scheme 미설정으로 인해 의도치 않은 URL Scheme을 호출하던 문제 수정 : 설정 방법이 변경되어 업데이트를 위해서는 반드시 URL Scheme 설정 필요 (XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.payco를 등록)

### 1.12.1 (2018.08.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.1/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.12.1
    * (공통)IAP SDK 최신버전 적용 (1.5.0)
    * (공통)Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
    * (공통)점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
    * (공통)IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (공통)인증 API 중복 호출시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (iOS)에러코드 추가 : Gamecenter 로그인 거부(TCGB_ERROR_IOS_GAMECENTER_DENIED)
    
#### 버그수정
* [SDK] 1.12.1
    * (iOS)Naver 로그인 시 프로필 정보 조회 실패로 인해 로그인이 불가능한 버그 수정 : 프로필 정보 조회 실패하더라도 로그인은 성공하도록 변경    
    
### 1.12.0 (2018.07.24) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.0/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.12.0
    * (iOS)Gamebase 초기화 시 Debug Log에 사용중인 Adapter들의 버전 정보, 앱의 빌드 정보를 출력하는 기능이 추가 
    * (iOS)CocoaPods을 통해 배포 되는 Naver Auth Adapter에서 포함하고 있던 Naver ID Login SDK의 바이너리가 제거 되고 의존성 설정 방식으로 변경

### 1.11.1 (2018.07.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-iOS.zip)
#### 기능 추가
* Line IdP 추가 : iOS
#### 기능 개선/변경
* [SDK] 1.11.1
    * (공통)Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경
    
#### 버그수정
* [SDK] 1.11.1
    * (공통)점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정

### 1.11.0 (2018.06.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* iOS Google IdP 추가 : iOS
* Twitter IdP 추가 : Android, iOS
* Line IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
* Server API 추가 
    * getSimpleLaunching : 클라이언트 앱 기동시 제공되는 Launching 정보 확인용 API
    
#### 기능 개선/변경
* [SDK] 1.11.0
    * (공통)LocalizedString 일본어 번역 추가
    * (공통)인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
    * Naver ID Login SDK 업데이트 : iOS(4.0.10)
* Sample App 
    * ServerPush 기능 및 Observer 기능 추가
    * Gamebase SDK 업데이트 : Android(1.9.0), iOS(1.9.0), Unity(1.10.1)    
    
### 1.9.1 (2018.05.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.1/GamebaseSDK-iOS.zip)
#### 버그수정
* [SDK] 1.9.1
    * (iOS) Gamebase WebView NavigationBar 영역에 타이틀, 뒤로가기, 닫기 버튼이 나타나지 않는 현상을 수정
    
### 1.9.0 (2018.05.03) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)
* 이용정지 등록시 사용자의 리더보드(랭킹) 데이터를 삭제할 수 있는 옵션 추가(TOAST Leaderboard를 사용하는 경우에 한함)
    * 이용정지 등록 메뉴를 이용하거나 App Guard 연동 페이지에서 사용 가능
#### 버그 수정
* [SDK] 1.9.0
    * (iOS) Naver계정을 이용한 로그인 중 App to Web 로그인 시도 시, 서버로부터 받아온 Scheme의 형식이 변경되어, 로그인이 되지 않는 현상 수정
    * (iOS) Adapter로부터 UnderlyingError 객체를 받아서 유저에게 전달되는 에러객체를 생성하는 로직에서 메시지 및 Underlying Error의 설정이 되지 않는 버그 수정

### 1.8.1 (2018.04.12) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-iOS.zip)
#### 버그 수정
* [SDK] 1.8.1
    * (Android. iOS)registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 1.8.0 (2018.04.05) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    * 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    * 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    * (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가
#### 기능 개선/변경
* [SDK] 1.8.0
    * (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
    * (iOS)페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
    * (iOS)iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정
#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정

### 1.7.0 (2018.02.22) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* [SDK] 1.7.0
    * Naver IdP 인증 추가
    * Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 1.6.0 (2018.01.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.6.0/GamebaseSDK-iOS.zip)
#### 버그 수정
* [SDK] 1.6.0
    * (iOS)WebView 호출시, 크래시가 일어날 수 있는 부분에 대한 방어로직 처리

### 1.5.0 (2017.12.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* [SDK] 1.5.0
    * WebView가 닫힐 때 발생하는 Close Callback 추가
    * WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
    * Unity Setting Tool 신규 배포

### 1.4.0 (2017.11.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.4.0 업데이트
    * (iOS)close/back 버튼 리소스가 없을 때, "x", "<" 등의 텍스트로 노출되던 이슈를 디폴트 값으로 대체
#### 버그 수정
* [SDK] 1.4.0 업데이트
    * (iOS)WebView 런치 후, 기기 회전시 NavigationBar Title 이 reset이 되는 오류 수정
    * (iOS)WebView의 NavigationBar Height을 커스터마이징 할 때, NavigationBar 배경 부분이 겹쳐서 노출되는 오류 수정

### 1.3.0 (2017.10.26) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* [SDK] 1.3.0 업데이트
    * Credential을 이용한 AddMapping API추가

### 1.2.0 (2017.09.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-iOS.zip)
#### 기능 추가
* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
    * 이용정지 사용자 팝업 창 노출

### 1.1.5 (2017.07.20) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
    * 시스템 팝업 창 API 추가 (showAlertWithTitle)
    * 국가코드를 대문자로 반환하도록 변경 (Android)
    * TCPush SDK 1.4.1 로 업데이트
    * IAP SDK 1.3.3.20170627 로 업데이트

### 1.1.4 (2017.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
    * 런타임 중 결제 Store를 변경할 수 있는 API 제공

### 1.1.2 (2017.04.04) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-iOS.zip)
#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임론칭시 점검, 긴급공지 팝업 창 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리

### 1.1.0 (2017.03.21) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

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
