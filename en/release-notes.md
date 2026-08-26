<!-- machine_translated: true -->

<!-- pre-align:aligned sig=4da805cd3daa -->

<a id="game-gamebase"></a>
## Game > Gamebase > Release Notes { #game-gamebase }

<a id="2021-04-19"></a>
### 2021. 04. 19. { #2021-04-19 }

<a id="2021-04-19-1"></a>
#### Bug Fixes
* [SDK] 2.21.1
	* (Android) Fixed a crash that occurred when canceling Hangame login via PAYCO.
	* (iOS) Fixed an issue where the setting was not applied even when configured to support bitcode.

<a id="2021-04-13"></a>
### 2021. 04. 13. { #2021-04-13 }

<a id="2021-04-13-1"></a>
#### Added Features
* [SDK] 2.21.0
	* (Common) Added Hangame Japan authentication.

<a id="2021-04-13-2"></a>
#### Feature Updates
* [Console] 
	* Members > Member: Improved to also display the reception date when the ad reception consent or nighttime ad reception flag is true when viewing push tokens.
	* Purchase (IAP) > Payment Information: Improved to display strings with line breaks in the popup showing additional information.
	* Purchase (IAP) > Payment Abusing Monitoring
		* Improved to allow users to enter the auto-ban detection period (from 1 hour to 48 hours), which was previously fixed at 1 hour.
		* Improved to allow OR conditions in addition to AND conditions for count and amount auto-ban criteria.
* [SDK] 2.21.0	
	* (Android) Updated external SDKs: Facebook Android SDK (6.5.1), Line Android SDK (5.4.0).
	* (iOS) Changed to support bitcode.
	* (iOS) Fixed to display the close button first on screen when showWebView is called.
	
<a id="2021-04-13-3"></a>
#### Bug Fixes
* [SDK] 2.21.0
	* (Android) Fixed a crash that occurred when calling the payment API in a build with Proguard applied.

<a id="2021-03-30"></a>
### 2021. 03. 30. { #2021-03-30 }

<a id="2021-03-30-1"></a>
#### Feature Updates
* [SDK] 2.20.2
	* (Android) Updated to Billing Client 3.0.3, which resolves payment errors on Android 11 devices in the Google Play Store.

<a id="2021-03-23"></a>
### 2021. 03. 23. { #2021-03-23 }

<a id="2021-03-23-1"></a>
#### Feature Updates
* [Console] Members > Download: Improved the number of data records saved in a single file (from 50,000 to 500,000).
* [SDK] 2.20.2
	* (iOS) Updated Facebook iOS SDK (9.1.0).
	* (iOS) Fixed an issue where the openURL delegate was not called in GamebaseAuthFacebookAdapter in certain cases.

<a id="2021-03-09"></a>
### 2021. 03. 09. { #2021-03-09 }

<a id="2021-03-09-1"></a>
#### Added Features
* [Console] App > Terms and Conditions: Added GDPR terms and conditions.
* [Server API] Added an API to retrieve the Gamebase user ID by IdP ID.

<a id="2021-03-09-2"></a>
#### Feature Updates
* [SDK] 2.20.1
	* (iOS) Updated the IDFA retrieval logic for iOS 14 compatibility: added the NSUserTrackingUsageDescription field to info.plist.

<a id="2021-02-23"></a>
### 2021. 02. 23. { #2021-02-23 }

<a id="2021-02-23-1"></a>
#### Added Features
* [Console] 
	* Operations > Kickout: Added the ability to kick out users by client version.
	* Purchase (IAP) > Store: Added the ability to configure the one-time receipt validation step for the Google Play Store.
	
<a id="2021-02-23-2"></a>
#### Bug Fixes
* [SDK] 2.20.1
	* (Android) Fixed a logic that could cause a crash during initialization of the push-fcm module.

<a id="2021-02-15"></a>
### 2021. 02. 15. { #2021-02-15 }

<a id="2021-02-15-1"></a>
#### Bug Fixes
* [Console] Purchase (IAP) > Payment History: Fixed an issue where the product name was incorrectly displayed when downloading a file.

<a id="2021-02-09"></a>
### 2021. 02. 09. { #2021-02-09 }

<a id="2021-02-09-1"></a>
#### Added Features
* Added common terms and conditions feature
	* [Console] New menus opened: App > Terms and Conditions, App > Terms and Conditions Deployment
	* [SDK] 2.20.0
		* (Common) Added an API to open the terms and conditions WebView
		* (Common) Added an API to retrieve the terms and conditions list and each user's consent status
		* (Common) Added an API to save each user's terms and conditions consent status to the Gamebase server

<a id="2021-02-09-2"></a>
#### Feature Updates
* [Console] App > Client: Improved the screen to display client versions separately by store
* [SDK] 2.20.0
	* (Common) Changed so that the customer center is displayed without login when the customer center type is TOAST organization product (Online Contact)
	* (Unity) Removed warning logs
	* (Unity) Updated CEF 2.1.2 in Standalone WebView
		* Fixed an issue where a crash occurred when the URL length exceeded 2,048
		* Improved PostProcessBuild as the library path changed when building in Unity 2019
		* Fixed an intermittent error caused by uninitialized strings
		* Fixed a bug where the WebView did not reopen after navigating to a new scene while using Gamebase WebView

<a id="2021-02-09-3"></a>
#### Bug Fixes
* [SDK] 2.20.0
	* (JavaScript) Fixed an error that occurred during initialization when customer center information was not entered in the console
* [SDK] 2.19.1
	* (Unreal) Fixed a compile error that occurred when files were excluded during Unity build

<a id="2021-01-26"></a>
### 2021. 01. 26. { #2021-01-26 }

```
Removed the Push > Push (old) console menu feature.
```

<a id="2021-01-26-1"></a>
#### Added Features
* [Console] 
	* Ban > AppGuard: Added conditional blocking feature
	* Purchase (IAP) > Payment Abusing Monitoring: Added Apple App Store 
* [SDK] 2.19.0
	* (Unreal) SDK distribution: Reflects cumulative changes from 2.16.0 to 2.19.0
		* Provides [Android Settings Tool](https://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#android-settings): Use the settings tool instead of modifying the Gamebase_Android_UPL.xml file.
		* Added Customer Center feature
		* Added authentication: Hangame, Weibo
		* Added Galaxy Store
		* Added localized product information to purchase item information: localizedTitle, localizedDescription
		* Provides Android Settings Tool
		* Added support for Unreal 4.26

<a id="2021-01-26-2"></a>
#### Feature Updates
* [Console]
	* Management > Permission: Removed revenue access permission [Go to the related notice](https://www.toast.com/kr/support/notice/detail/2101)
* [SDK] 2.19.1
	* (iOS) Changed the Weibo IdPAdapter structure

<a id="2021-01-12"></a>
### 2021. 01. 12. { #2021-01-12 }

```
The minimum supported Xcode version for Gamebase has changed from 10 to 11.
```

<a id="2021-01-12-1"></a>
#### Added Features
* [Console] Added new Push menus
	* Statistics: View push statistics, including delivery, receipt, and token registration
	* Event key: Manage event keys used for push delivery statistics
	* Certificate: Manage certificates used for push delivery
	* Settings: Manage setting values related to push
	
<a id="2020-12-29"></a>
### 2020. 12. 29. { #2020-12-29 }

<a id="2020-12-29-1"></a>
#### Added Features
* [SDK] 2.19.0
	* (Common) Added Weibo authentication
	* (Android) Added Sign In with Apple authentication
	
<a id="2020-12-29-2"></a>
#### Feature Updates
* [Console]
	* App > App: Added a beta service server to server address management
	* App > Client: Added beta service to client status, and added a memo feature to register additional client information
	* Purchase (IAP) > Products: Added a search condition - In Use
	* Purchase (IAP) > Payment Information: Added display of store test payment records in payment history
	* Purchase (IAP) > Sales Status menu closed: Features have been integrated into Analytics > Revenue Metrics.
	* Analytics > Usage Environment > Installation URL menu closed
* [SDK] 2.19.0
	* (Common) Added launching status code: Beta service (205)

<a id="2020-12-29-3"></a>
#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) Fixed an issue where an OutOfMemoryException occurred during WebSocket retry
* [SDK] 2.19.1
	* (Android) Fixed an issue where a crash occurred when logging in with another IdP after attempting Weibo login
	
<a id="2020-12-15"></a>
### December 15, 2020 { #2020-12-15 }

<a id="2020-12-15-1"></a>
#### Added Features
* Added the feature to pass extra data defined by the game when opening the Gamebase Customer Center page: SDK 2.18.2
	* [Console] Customer Center > Customer Inquiries: Added the ability to view additionally registered extra data on the customer inquiry detail screen
* [SDK] 2.18.2
	* (Common) Added the additionalURL field when opening the game company's own customer center
	* (Common) Added localized product information to payment item information: localizedTitle, localizedDescription

<a id="2020-12-15-2"></a>
#### Feature Updates
* [Console]
	* Analytics: Improved to retain the selected search conditions even after changing the date following a filter search
	* Push > Push: Removed Tencent Push
	* Purchase (IAP) > Payment Information: Changed so that the receipt verification button is not displayed when the payment is in the refund status
* [SDK] 2.18.2
    * (Common) Updated TOAST SDK: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) Updated external SDKs to resolve security warnings in the encryption logic: Payco Login SDK(1.5.3), Hangame ID SDK(1.3.2)
	* (Android) Removed the Tencent Push module
	* (Android) Removed functions deprecated in Gamebase Android SDK 2.6.0
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView: Returns an error when an invalid URL is passed; the received URL is used as-is without encoding
	* (iOS) Changed so that custom schemes work regardless of letter case
	* (Unity) Deprecated fields in the GamebaseRequest.GamebaseConfiguration class: zoneType, fcmSenderId

<a id="2020-12-15-3"></a>
#### Bug Fixes
* [Console]
	* Purchase (IAP) > Item: Fixed an issue where items were registered as duplicates when bulk-registering items from a file
* [SDK] 2.18.2
    * (Android) Fixed an issue where the WebView custom scheme did not work on OS 5.0 - 6.0 devices

<a id="2020-12-2"></a>
### December 2, 2020 { #2020-12-2 }

<a id="2020-12-2-1"></a>
#### Added Features
* [Console] 
	* Added 24 Gamebase permission granularity features.
	* Analytics > Group Indicators: Added comparison graphs for new subscribers and purchase amounts by project.    
	* Member > Members: Added a tab at the bottom for viewing customer center inquiry history.
	* Coupon > Issue Coupon: Added an additional coupon issuance feature to issue more coupons on top of already-issued coupons (up to 100,000 per issuance).

<a id="2020-12-2-2"></a>
#### Feature Updates
* [Console]
    * Analytics > Real-time Monitoring: Changed the display color when data rises or falls compared to the previous day's indicators.
		* Rise: Blue → Red, Fall: Red → Blue
	* Analytics > Sales Indicators > Purchase Amount: Added the ability to view sales data by store and by IdP, in addition to the previous country-only comparison.
	* Operation > Notice: Added a setting to connect to the customer center via the View Details link.
	* Customer Center > Customer Inquiry: Added an auto-translation feature to the reply delivery settings.
	* Coupon > Issue Coupon: Increased the maximum initial coupon issuance quantity from 50,000 to 1,000,000.

<a id="2020-12-2-3"></a>
#### Bug Fixes
* [Console]
    * Purchase (IAP) > Payment Information: Fixed an issue where files could not be downloaded when there was a large amount of queried data.

<a id="2020-11-10"></a>
### November 10, 2020 { #2020-11-10 }

<a id="2020-11-10-1"></a>
#### Added Features
* Added Galaxy Store: SDK 2.18.0

<a id="2020-11-10-2"></a>
#### Feature Updates
* [SDK] 2.18.0
    * (Android) Updated TOAST SDK: [Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027) - Applied GooglePlay Billing Library v.3.0.1
    * (Android) Added handling for WebView SSL security warnings
    * (iOS) Added SceneDelegate support API available from iOS 13 and later

<a id="2020-11-10-3"></a>
#### Bug Fixes
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash occurred after Google payment in 2.18.0

<a id="2020-10-27"></a>
### October 27, 2020 { #2020-10-27 }

<a id="2020-10-27-1"></a>
#### Added Features
* Unreal SDK features added: SDK 2.15.0
    * Added GamebaseEventHandler that integrates all existing event systems
        * Includes ServerPush and Observer features, and allows checking promotion payment events and push events
    * Added APIs
    	* Added a payment API that requests payment by product ID and allows confirmation upon payment completion by entering additional information (UserPayload)
    	* Display image notices: showImageNotices
    	* Check push token information: queryTokenInfo
    * Added the ability to receive push notifications even when the app is in the foreground by configuring NotificationOption when registering push tokens
    * Added WebViewConfiguration contentMode setting
    
<a id="2020-10-27-2"></a>
#### Feature Updates
* [SDK] 2.17.1
    * (iOS) Added error messages when sending specific metrics: when push registration fails, when sending game metrics
    * (Unity) Added support for Unity 2017.2.5
* [SDK] 2.15.0
    * (Unreal) Updated TOAST SDK: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)    

<a id="2020-10-27-3"></a>
#### Bug Fixes
* [Console]
    * Analytics > User Metrics: Fixed an issue where weekly and monthly average CCU was displayed abnormally due to incorrect calculation logic
    * Push > Push: Fixed an issue where 'null' was displayed in the title when the title font color was set to a non-black color without entering a title
	* Coupon > Coupon Issuance: Fixed an issue where files could not be downloaded when the number of issued coupons was 50,000 or more
* [SDK] 2.17.1
    * (Unity) Fixed an issue where calling image notices and web views sequentially caused the later-called API to not work	
* [SDK] 2.15.0    
    * (Unreal) Fixed an issue where ProGuard declarations were missing in the payment module


<a id="2020-10-13"></a>
### 2020. 10. 13. { #2020-10-13 }

```
If you want to use Hangame authentication, contact the customer support center in advance.
```

<a id="2020-10-13-1"></a>
#### Added Features
* Added Hangame IdP authentication: SDK 2.17.0

<a id="2020-10-13-2"></a>
#### Feature Updates
* [SDK] 2.17.0
    * (Common) Added support for downloading attached images in the customer center when clicked
    * (Common) Updated TOAST SDK: Android(0.23.2), Unity(0.21.2)
    * (iOS) Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long
    * (iOS) Changed the logic to re-display the title when the URL or title changes in the web view

<a id="2020-10-13-3"></a>
#### Bug Fixes
* [SDK] 2.17.0
    * (iOS) PAYCO authentication: Fixed an issue where the logout callback was not received when logout was called after logging in with lastLoggedInProvider
* [SDK] 2.17.1
    * (Android) Fixed an issue where a crash occurred in the kotlinx-coroutine module when the ImageNotice API was called in 2.17.0

<a id="2020-09-22"></a>
### 2020. 09. 22. { #2020-09-22 }

<a id="2020-09-22-1"></a>
#### Added Features
* Added customer center features
    * [Console] Opened the Customer Center menu: handles customer inquiries and manages FAQ/notices
    * [SDK] 2.16.0
	* (Common) Added API (Gamebase.Contact.requestContactURL): returns the customer center URL
	* (Common) Added the ContactConfiguration parameter to allow setting userName in the customer center API
		
<a id="2020-09-22-2"></a>
#### Feature Updates
* [Console] 
    * Analytics menu (common): Changed the country filter sort order (from metric descending to country name ascending)
    * Analytics > Revenue metrics: The per-store dashboard now displays the total payment amount in addition to the per-country payment amount for each store

<a id="2020-09-16"></a>
### 2020. 09. 16. { #2020-09-16 }

<a id="2020-09-16-1"></a>
#### Feature Updates
* [SDK] 2.15.1
    * (iOS) Updated TOAST SDK: iOS (0.27.0)
	* A new version of the IAP SDK that addresses iOS 14 beta changes has been applied. [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

<a id="2020-09-15"></a>
### 2020. 09. 15. { #2020-09-15 }

<a id="2020-09-15-1"></a>
#### Added Features
* [SDK] 2.15.0
    * (JavaScript) Added GamebaseProductId to the Hangame point payment API

<a id="2020-09-15-2"></a>
#### Bug Fixes
* [Console]
    * Purchase (IAP) > Payment Information: Fixed an issue where the receipt validation display was not working properly

<a id="2020-08-25"></a>
### August 25, 2020 { #2020-08-25 }

```
The Google Billing Client module has been updated in Gamebase SDK version 2.15.0.

If `gamebase-adapter-purchase-google` is being used, the previous Game Client Version must be set to 'Update to the latest version required' if the Gamebase SDK version earlier than 2.15.0 is to be upgraded to 2.15.0 or later.

This is because when purchasing an item while different Billing Client versions have been applied to multiple devices, any resultant error could lead to a reprocessing problem.
```

<a id="2020-08-25-1"></a>
#### Added Features
* [SDK] 2.15.0
    * (Common) Added a feature to receive push notifications when the app is in the foreground by configuring NotificationOption when registering push tokens
    * (Common) Added a push API: Verify push token information (Gamebase.Push.queryTokenInfo API)
* [SDK] 2.9.1
    * (Unreal) Added support for Unreal 4.22 through 4.25
    * (Unreal) Added support for PLCrashReporter issues: [Guide](http://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#ios-settings)

<a id="2020-08-25-2"></a>
#### Feature Updates
* [Console]
    * Push > Push: Updated to allow sending promotional push notifications without entering sender contact information and opt-out consent methods
* [SDK] 2.15.0
    * (Common) Updated TOAST SDK: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) Added null check logic for payment payload
* [SDK] 2.9.1
    * (Unreal) Updated Gamebase SDK for iOS version inside the iOS Plugin (2.9.1)
    * (Unreal) Fixed missing UObject referencing handling

<a id="2020-08-25-3"></a>
#### Bug Fixes
* [Console]
    * Push > Push: Fixed an issue where scheduled push notifications were always calculated and sent in UTC+9 regardless of the entered timezone
    
<a id="2020-08-19"></a>
### August 19, 2020 { #2020-08-19 }

<a id="2020-08-19-1"></a>
#### Bug Fixes
* [Console]
    * Analytics (all menus): Fixed an issue where Excel download did not work

<a id="2020-08-11"></a>
### August 11, 2020 { #2020-08-11 }

<a id="2020-08-11-1"></a>
#### Feature Updates
* [Console]
    * Analytics > User Metrics > Retention: Added display of numerical values in addition to percentages
* [SDK] 2.14.0
    * (iOS) Removed constant values for PAYCO IdP: Removed because the PAYCO string was causing Apple review rejection
    * (iOS, Unity) Added contentMode setting to TCGBWebViewConfiguration
* [Server]
    * Added error codes for the coupon consume API: for cases where a value other than letters or numbers is entered in the coupon code (Error Code: -4000205)

<a id="2020-07-28"></a>
### July 28, 2020 { #2020-07-28 }

<a id="2020-07-28-1"></a>
#### Added Features
* [Console]
    * Analytics: Added WAU (Weekly Active User) and MAU (Monthly Active User) metrics
* [SDK] 2.13.0
    * (Unity) Standalone: Added an API to display image notices

<a id="2020-07-28-2"></a>
#### Feature Updates
* [Console]
    * App > App: Modified to allow additional input of information required for Sign In With Apple authentication on iOS 12 or lower
* [SDK] 2.13.0
    * (Android) Modified the popup image ratio calculation logic for image notices
    * (iOS) Sign In With Apple authentication: Added support for iOS 12 or lower

<a id="2020-07-28-3"></a>
#### Bug Fixes
* [Console]
    * Operation > Image Notices: Fixed an issue where changes were not applied when using the copy function or when switching from selected target countries to all countries
* [SDK] 2.13.0
    * (Android) Fixed an issue where the ANDROID_ACTIVITY_DESTROYED(31) error was returned in the close callback when closing the WebView
    * (Android) Fixed an issue where ProGuard declarations were missing from the payment module
    
<a id="2020-07-14"></a>
### July 14, 2020 { #2020-07-14 }

<a id="2020-07-14-1"></a>
#### Added Features
* Image notices: displays image popups in the game based on display period and priority
    * [Console] Operation > Image Notice: menu added
    * [SDK] 2.12.0: added API for displaying image notices

<a id="2020-07-14-2"></a>
#### Feature Updates
* [Console] 
    * Purchase (IAP) > Products: added the ability to search for products by item number
    * Members > Members: improved so that users in the withdrawal grace period can be changed to normal status
    * Members > Downloads: added deviceKey and IdP code fields to login log history
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with the storeCode (default=AS) set in configuration
    * (iOS) Fixed an issue where a WebView that could not load content had no **Close** button, making it impossible to close
    * (Unity) Updated TOAST Unity SDK (0.20.1.1)
    
<a id="2020-06-23"></a>
### June 23, 2020 { #2020-06-23 }

<a id="2020-06-23-1"></a>
#### Added Features
* [SDK] 2.11.0
	* Added payment API: request payment by product ID, and check additional information (UserPayload) entered upon payment completion

<a id="2020-06-23-2"></a>
#### Feature Updates
* [Console] 
	* Purchase (IAP) > Products: improved so that multiple Gamebase products can be registered and managed under a single store item ID

<a id="2020-06-09"></a>
### June 9, 2020 { #2020-06-09 }

<a id="2020-06-09-1"></a>
#### Feature Updates

* [Console] 
	* Members > Members:  Added display of withdrawal grace period status (withdrawal grace period, withdrawal cancellation, immediate withdrawal) on the **Withdrawal History** screen
* [SDK] 2.10.1
	* (iOS) Changed to set the device language if a language code is not configured when initializing user push settings

<a id="2020-06-09-2"></a>
#### Bug Fixes

* [Console] 
	* Coupon > Issue Coupon: Fixed an issue where SMS delivery history was not downloaded when downloading coupon statistics

* [SDK] 2.10.1
	* (Unity) Fixed an issue where login calls failed because ViewController was not configured in the iOS Plugin
	* (JavaScript) Fixed an issue where an error occurred if StoreCode was not entered during initialization

<a id="2020-05-26"></a>
### May 26, 2020 { #2020-05-26 }

<a id="2020-05-26-1"></a>
#### Added Features
* [Console] 
	* Coupon > Issue Coupon: Added sending statistics feature and coupon sending history download feature
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler that integrates all existing event systems
		* Includes ServerPush and Observer features, and allows you to check promotion payment events and push events

<a id="2020-05-26-2"></a>
#### Feature Updates
* [Console] 
	* General: Modified button/tag UI to conform to the common design guide
* [SDK] 2.10.0 
	* (Unity) Updated CefWebview version inside StandaloneWebviewAdapter: v2.0.4
		* Improved WebviewIndex validation logic
		* Fixed an issue where NullReferenceException occurred intermittently when creating a Webview
	* (Unity) Added error codes related to socket connection to GamebaseErrorCode: SOCKET_CONNECTION_TIMEOUT, SOCKET_CONNECTION_FAIL

<a id="2020-05-12"></a>
### 2020. 05. 12. { #2020-05-12 }

<a id="2020-05-12-1"></a>
#### Added Features
* [SDK] 2.9.0
	* (Unreal) New SDK release
	
<a id="2020-05-12-2"></a>
#### Feature Updates
* [Console] 
	* App > App: Improved to save Toast accounts of users who changed the withdrawal grace period
	* Member > Members: Fixed an issue where information was not displayed correctly when viewing mapping history
	* Purchase (IAP) > Store: Modified to prevent new registration for Test and Legacy ONE Store

<a id="2020-05-12-3"></a>
#### Bug Fixes
* [SDK] 2.9.1
	* (Android) Fixed an error where the analytics level became null after mapping and was not reflected correctly in payment metrics
	* (iOS) Fixed an issue where building with the Unreal Engine treated warnings as build errors, preventing the build from completing

<a id="2020-04-29"></a>
### 2020. 04. 29. { #2020-04-29 }

<a id="2020-04-29-1"></a>
#### Bug Fixes
* [SDK] 2.9.1 
	* (Unity) Fixed an issue where an error occurred when changing the client's service status in the console after Initialize
		* Affected versions: v2.8.0 and later	
		* Affected platforms: Standalone, WebGL, and Editor
		
<a id="2020-04-28"></a>
### April 28, 2020 { #2020-04-28 }

<a id="2020-04-28-1"></a>
#### Added Features
* Withdrawal grace period feature
	* [SDK] 2.9.0
		* (Common) Added APIs: apply for withdrawal grace period, cancel withdrawal grace period application, immediate withdrawal while in withdrawal grace period, check whether a user is in withdrawal grace period
	* [Console]
		* App > App: Added a feature to configure the withdrawal grace period

<a id="2020-04-28-2"></a>
#### Feature Updates
* [SDK] 2.9.0
	* (Common) Updated TOAST SDK: Android (v0.21.0), iOS (v0.23.0), Unity (0.20.1)
	* (Common) Updated PAYCO Login SDK: Android (v1.5.0), iOS (v1.4.0)
* [Console]
	* All menus: Updated console buttons and tag design
	* Operation > Maintenance, Operation > Notice, Push: Added support for automatic multi-language translation
	* Member > Members: Added display of grace period expiration date when viewing member information for users in withdrawal grace period

<a id="2020-04-14"></a>
### April 14, 2020 { #2020-04-14 }

<a id="2020-04-14-1"></a>
#### Feature Updates
* [Console] 
	* Analytics common: Updated TUI chart version and applied it to Frequency7 metrics
* [SDK] 2.8.1 
	* (Common) Added internal metrics to check Analytics delivery results
	
<a id="2020-04-14-2"></a>
#### Bug Fixes
* [Console] 
	* Analytics common: Fixed an issue where the scroll went out of the area when the country name was long
	* Analytics > Real-time Monitoring: Fixed an issue where metrics appeared as 0 when a query was requested while data was being saved
* [SDK] 2.8.1 
	* (Android) Fixed code that could cause a crash after a process restart
	* (JavaScript) Fixed an issue where login with Hangame IdP via credentialInfo login did not work
	
<a id="2020-03-24"></a>
### March 24, 2020 { #2020-03-24 }

<a id="2020-03-24-1"></a>
#### Added Features
* [Console] 
	* New menu opened: Analytics > User Metrics > Frequency 7
		* Provides information on the number of visits and visit rate over a week for DAU. You can get a quick overview of game engagement and loyalty.
	* Coupon > Issue Coupon: Added a feature to send coupon SMS
* [SDK] 2.8.0
	* (Common) Added product type and regional price information to payment and product details
	* (Unity) Updated CefWebview inside StandaloneWebviewAdapter to v2.0.1
		* Added a feature to pass popup information without displaying a popup when PopupType is PASS_INFO
 	* (Javascript) Added support for Hangame channeling: Hangame IdP authentication and HanCoin payment


<a id="2020-03-24-2"></a>
#### Feature Updates
* [Console] 
	* App > Transmission Metrics Settings: Restricted so that only pre-registered meta filters can be used in transmission metrics
		* The number of meta filters is limited, and metrics will not be displayed if exceeded. Please note the following limits: Level (5,000), World/Server/Channel (100), Class/Job (100)
* [SDK] 2.8.0 
	* (Common) Improved so that a popup directing users to the store is additionally displayed when initialization fails due to an app version not registered in the console
	* (Android) Fixed code that could cause failures due to initialization timing issues when calling payment-related APIs immediately after login

<a id="2020-03-24-3"></a>
#### Bug Fixes
* [Console] 
	* Revenue Metrics > Payment Amount
		* Fixed an issue where the currency in the chart tooltip was fixed to KRW instead of displaying the currency configured in the app
		* Fixed an issue where February metrics were not displayed when viewing by month
		
<a id="2020-03-10"></a>
### March 10, 2020 { #2020-03-10 }

<a id="2020-03-10-1"></a>
#### Added Features

- [Console] 
	- App  >  App: Added an option to configure whether to include test payments when displaying Analytics revenue metrics
    		- When set to 'Exclude test payments', all test payments are excluded from Analytics revenue metrics.
		- Purchase (IAP): Added currency code configuration for payment metrics when accessing the Purchase (IAP) menu for the first time
	- Can only be configured once; metrics in Analytics revenue metrics are displayed in the configured currency code.
  	- Added the 'Desktop View' feature to the mobile console (including the TOAST app)

<a id="2020-03-10-2"></a>
#### Feature Updates

- [Console] 
  	- App  >  Installation URL: Added support for additional URL input schemes
    		- Previously: Common ('http://', 'https://'), Android ('market://')
    		- Added: iOS ('itms://', 'itmss://', 'itms-apps://'), Android ('intent://')
- [SDK] 2.7.2 
  	- (Unity) Improved FacebookAdapter
    		- Compatibility tested from v7.9.4 to v7.18.1
    		- Added null exception handling
  	- (Unity) Improved StandaloneWebviewAdapter
    		- Added support for exporting web pages as textures
    		- Added multi-webview support
    		- Added a cookie deletion option
    		- Added texture resizing support
		- Added scrollbar show/hide support
    		- Added page load completion notification
    		- Added transparent background support
  	- Fixed an issue where an error occurred when calling the Initialize API after selecting the Android/iOS platform in the editor

<a id="2020-03-10-3"></a>
#### Bug Fixes
- [Console] 
  	- Analytics: Fixed an issue where revenue metrics were displayed as '0' when the currency code was coin-based

<a id="2020-02-25"></a>
### February 25, 2020 { #2020-02-25 }

<a id="2020-02-25-1"></a>
#### Added Features
* [Console] 
	* Coupon > Issue Coupon: Added a feature to allow issued coupons to be used only in the configured store
	
<a id="2020-02-25-2"></a>
#### Feature Updates
* [SDK] 2.7.1
	* (Common) Modified to return a value when GetAuthProviderUserID is called after logging in as a Guest
* [Console]
	* App > App: Added a notification logic for re-registration after deleting the same client version
	* Purchase (IAP) > Item: Added field values for subscription product registration (App Store - Shared secret, Google store - Domain authentication File Names)

<a id="2020-02-25-3"></a>
#### Bug Fixes
* [Console]
	* Analytics > Real-time Monitoring > Real-time Metrics: Fixed an issue where the CCU field intermittently showed an empty value or infinity after sending a push notification
	* Analytics > Delivery Metrics
		* Fixed a bug where the grid was not updated to No Data when data disappeared
		* Fixed an issue where button alignment was displayed vertically when the filter name was short

<a id="2020-02-11"></a>
### 2020. 02. 11. { #2020-02-11 }

<a id="2020-02-11-1"></a>
#### Added Features
* [Console] 
	* Analytics > User Metrics > Life Cycle: Added a new menu that provides a graph view of user metric trends from project creation, allowing you to track the flow at a glance.
	* Management > Permission: Added the weekly report receiving permission item.
		* The actual 'Weekly Report' email will be sent starting in March.

<a id="2020-02-11-2"></a>
#### Feature Updates
* [Server API] Added validation for the `regUser` length when calling the withdrawal API.
* [Console] 
	* Analytics: Applied Japanese fonts to Grid and Chart.
	* Purchase: Improved the popup message displayed when an error occurs to be more intuitive for users.

<a id="2020-02-11-3"></a>
#### Bug Fixes
* [Console]
	* Analytics: Fixed an issue where the currency was displayed as "Yen (JPY)" instead of "Won (KRW)" when the language was changed to Japanese.

<a id="2020-01-21"></a>
### 2020. 01. 21. { #2020-01-21 }

<a id="2020-01-21-1"></a>
#### Added Features
* [SDK] 2.7.0
	* (Unity) Added support for NaverCafePLUG.

<a id="2020-01-21-2"></a>
#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Fixed an issue where a crash occurred even when the required `traceError` parameter was missing from the server response.
	* (Android) Fixed an issue where an exception occurred when the Firebase configuration was missing.
	* (Unity) Added handling for the `gamebase://dismiss` scheme during web login.
	* (Unity) Fixed an issue where the WebView was intermittently not displayed during a release build.
* [Console]
	* Analytics: Fixed an issue where the user was not redirected to the login page when the session expired.

<a id="2020-01-14"></a>
### 2020. 01. 14. { #2020-01-14 }

<a id="2020-01-14-1"></a>
#### Added Features
* [Server API] Added the user withdrawal API.

<a id="2020-01-14-2"></a>
#### Feature Updates
* [SDK] 2.6.3
	* (Unity) Improved standalone WebView: updated CefWebview.
	* (Unity) Added missing .dll files that caused errors after login.
		* ToastCommon.dll, vcruntime140.dll

<a id="2020-01-14-3"></a>
#### Bug Fixes
* [SDK] 2.6.3
	* (Unity) Fixed an error that occurred when calling the Login(CredentialInfo) API.
	
<a id="2019-12-24"></a>
### 2019. 12. 24. { #2019-12-24 }

<a id="2019-12-24-1"></a>
#### Added Features
* Coupon > Issue Coupon: Added the keyword coupon feature.

<a id="2019-12-24-2"></a>
#### Feature Updates
* [Console]
	* Purchase > Payment Information: Added an additional information column.
* [SDK] 2.6.2
	* (Common) Updated TOAST SDK: Android (0.19.4), iOS (0.20.1), Unity (0.18.0).
	* (iOS) Updated the Naver SDK version (4.1.0).
	
<a id="2019-12-10"></a>
### December 10, 2019 { #2019-12-10 }

<a id="2019-12-10-1"></a>
#### Added Features
* App > App: Added a feature to register QA test devices by IP address during maintenance

<a id="2019-12-10-2"></a>
#### Bug Fixes
* [Console]
	* Fixed incorrect Japanese text
* [SDK] 2.6.1
	* (Android) Fixed a crash that occurred when calling Gamebase.login() before Gamebase.initialize()
	* (Android) Fixed an issue where TOAST Analytics User Data was incorrectly sent as a Java address value
	* (Android) Fixed a crash that occurred when IAP products were not activated
	* (iOS) Fixed an issue where mapping failed when using AddMapping (Force/Forcibly)
	* (iOS) Fixed a crash caused by an NSNull object when displayLanguageCode of PushConfiguration was not set using the Unity Plugin

<a id="2019-11-26"></a>
### November 26, 2019 { #2019-11-26 }

<a id="2019-11-26-1"></a>
#### Bug Fixes
* [Console]
	* Coupon > Coupon Issuance: Fixed an issue where downloading a coupon after session expiration resulted in a corrupted file being downloaded
	* Analytics > Real-time Monitoring > Dashboard: Fixed an issue where yesterday's data was displayed as 0
	* Fixed an issue where the disabled page was not displayed correctly when accessing menus related to TOAST products (IAP, Push, AppGuard, etc.) while the product was disabled

<a id="2019-11-20"></a>
### November 20, 2019 { #2019-11-20 }

<a id="2019-11-20-1"></a>
#### Bug Fixes
* [SDK] 2.6.1
	* (Unity) Fixed a build error on iOS caused by a missing iOS Plugin file in the package; added the missing file: 'toast_sdk_wrap.m'
	* (Unity) Fixed an initialization failure where the Store Code was set to empty when running on a platform other than Standalone in UnityEditor
	* (Unity) Fixed a NullReferenceException caused by an error in the zone type processing inside the Initialize API

<a id="2019-11-13"></a>
### November 13, 2019 { #2019-11-13 }

<a id="2019-11-13-1"></a>
#### Bug Fixes
* GamebaseSettingTool
	* Fixed an issue where files were not updated correctly when updating to Gamebase v2.6.0


<a id="2019-11-12"></a>
### 2019. 11. 12. { #2019-11-12 }

```
If you are upgrading from a Gamebase SDK version earlier than 2.6.0 to 2.6.0, you must apply the changes described in the Upgrade Guide document.
Guide location: Game > Gamebase > Upgrade Guide
```

<a id="2019-11-12-1"></a>
#### Added Features
* Coupon service launched: a feature to create and manage coupons in bulk
	* [Console] Opened the Coupon menu
	* [Server API] Added APIs for coupon validation and consumption
* Added an automatic payment abusing feature
	* [Console] Opened the Purchase (IAP) > Payment Abusing Monitoring menu
		* Added a feature to configure automatic sanctions for payment abusing
		* Allows manual suspension after searching for payment abusing conditions
* Added Google subscription payment feature
	* [SDK] 2.6.0 Android
* [SDK] 2.6.0
	* (Common) Added TOAST Logger to send data to Log&Crash for use in various analyses
	* (iOS) Added Sign In with Apple authentication
	* (Android) Since the Gamebase Android SDK is distributed through Bintray, you can use Gamebase with Gradle settings only

<a id="2019-11-12-2"></a>
#### Feature Updates
* [SDK] 2.6.0
	* (Unity) Fixed an error in the logic for refreshing LaunchingStatus at login
	* (Unity) Fixed an issue where configuring the Debug Log send feature in the Gamebase console caused the client to repeatedly send logs in an infinite loop
* [Console]
	* App > App: Updated to accept server addresses by service status (Testing, Under Review, Service)
	* Purchase (IAP) > Payment Information: Updated the UI to allow searching by selecting search conditions

<a id="2019-10-29"></a>
### 2019. 10. 29. { #2019-10-29 }

<a id="2019-10-29-1"></a>
#### Feature Updates
* [Console]
	* Analytics: Updated pie chart tooltips
	* Analytics > Real-time Monitoring: Added support for Push send targets

<a id="2019-10-15"></a>
### 2019. 10. 15. { #2019-10-15 }

<a id="2019-10-15-1"></a>
#### Feature Updates
* [SDK] 2.5.2
	* (iOS) Replaced UIWebView with WKWebView
* Sample App
	* Updated Gamebase SDK (v2.4.0)
	* Applied Smart Downloader (v1.5.8) and Leaderboard
	* Added features: game resource download, Leaderboard, TAA metrics integration, device transfer feature, and force mapping feature
	* Updated: added ServerPush listener and Observer maintenance detection
	* Game renewal
		
<a id="2019-10-15-2"></a>
#### Bug Fixes
* [Console]	
	* Management > Permissions: Fixed an issue where permissions could not be modified correctly
	* Mobile
		* Fixed an issue where the keyboard was activated when selecting a date in Datepicker
		* Analytics: Fixed an issue where NRU values were displayed in the ARPPU field

<a id="2019-09-24"></a>
### 2019. 09. 24. { #2019-09-24 }

<a id="2019-09-24-1"></a>
#### Feature Updates
* [Console]
	* App > Client: Modified the UI to allow store selection when registering a Web client
		
<a id="2019-09-24-2"></a>
#### Bug Fixes
* [Console]	
	* Management > Permissions: Fixed an issue where permissions could not be modified correctly
	* Mobile
		* Fixed an issue where the keyboard was activated when selecting a date in Datepicker
		* Analytics: Fixed an issue where NRU values were displayed in the ARPPU field
	
<a id="game-gamebase-1"></a>
### 2019. 09. 10. { #game-gamebase-1 }

<a id="game-gamebase-1-1"></a>
#### Added Features
* [Console]
	* Analytics: Added level metrics to the channel/world/server and class/job transmission metrics
	
<a id="game-gamebase-1-2"></a>
#### Feature Updates
* [Console]
	* Analytics: Improved Grid rendering performance (applied tui-grid 4.4.x)
* [SDK] 2.5.1
	* (iOS) Updated TCPushSDK used in GamebasePushAdapter to 1.7.0
		* TCPushSDK has been changed from a Static Library to a Framework file, so you must add TCPushSDK.framework to your project.
	
<a id="game-gamebase-2"></a>
### 2019. 08. 27. { #game-gamebase-2 }

<a id="game-gamebase-2-1"></a>
#### Added Features
* [SDK] 2.5.0
	* Added an API to open the CS URL entered in the Console in a webview
	
<a id="game-gamebase-2-2"></a>
#### Feature Updates
* [Console]
	* Analytics: Improved chart performance

<a id="game-gamebase-2-3"></a>
#### Bug Fixes
* [SDK] Setting Tool 1.4.3
	* Fixed a build error by moving Script files to the Editor folder
	* Fixed an issue where specifying the full path of a Language file for Multilanguage on Mac OS did not work
	
<a id="game-gamebase-3"></a>
### 2019. 08. 02. { #game-gamebase-3 }

<a id="game-gamebase-3-1"></a>
#### Bug Fixes
* [SDK] Setting Tool 1.4.3
	* Fixed a build error by moving Script files to the Editor folder
	* Fixed an issue where specifying the full path of a Language file for Multilanguage on Mac OS did not work

<a id="game-gamebase-4"></a>
### 2019. 07. 23. { #game-gamebase-4 }

<a id="game-gamebase-4-1"></a>
#### Added Features
* [Console]
	* Members > Download new menu opened: You can view and download a game user list as a file based on registration date and last login time

<a id="game-gamebase-4-2"></a>
#### Feature Updates
* [Console] Mobile
	* Maintenance and push registration and modification available
* [SDK] 2.4.4
	* (Common) Changed member error code format
	* (Unity) Added a key to GamebaseServerPushType (TRANSFER_KICKOUT)
* Setting Tool
	* Folder structure changed: `You must completely delete the existing SettingTool and reinstall it.`
	* Added multi-language support

<a id="game-gamebase-4-3"></a>
#### Bug Fixes
* [Console]
	* Analytics > User Metrics: Fixed an issue where chart X-axis dates overlap

<a id="game-gamebase-4-4"></a>
#### Bug Fixes
* [Console]
	* Analytics > User Metrics: Fixed an issue where chart x-axis dates overlap

<a id="game-gamebase-5"></a>
### 2019. 07. 11. { #game-gamebase-5 }

<a id="game-gamebase-5-1"></a>
#### Feature Updates
* [Console]Analytics
	* Improved level-up query performance
	* Added min and max information display in charts
	* Added multi-language support (Chinese)

<a id="game-gamebase-5-2"></a>
#### Bug Fixes
* [SDK] 2.4.3
	* (iOS) Fixed a crash issue that occurred when attempting to parse an improperly formatted error message when an error occurred during authentication
	* (Unity) Fixed an issue where the AddMappingForcibly API did not work when building for iOS and Android
	* (Unity) Fixed a JSON parsing error in iOSPlugin when calling the RequestRetryTransaction API

<a id="game-gamebase-6"></a>
### 2019. 07. 01. { #game-gamebase-6 }

<a id="game-gamebase-6-1"></a>
#### Bug Fixes
* [Console]
	* Management > Alarms: Fixed an issue where modifying alarm settings fails after setting up a Webhook

<a id="game-gamebase-7"></a>
### 2019. 06. 27. { #game-gamebase-7 }

<a id="game-gamebase-7-1"></a>
#### Bug Fixes
* [Console]
	* Ban: Fixed a file upload failure issue for bulk ban registration
* [SDK] Setting Tool 1.4.1
	* Fixed an error where GamebaseSettingTool failed to retrieve existing settings information when launched

<a id="game-gamebase-8"></a>
### 2019. 06. 25. { #game-gamebase-8 }

<a id="game-gamebase-8-1"></a>
#### Added Features
* Added transfer metrics feature
    * [Console] Analytics > Transfer Metrics: Opened a menu to check metrics by Level, Channel, and Class
		* Real-time status
		* Status by level
		* Status by world/server/channel
		* Status by class/occupation
		* Level-up
		* Item sales status
		* Item sales TOP 50

<a id="game-gamebase-8-2"></a>
#### Feature Updates
* [SDK] 2.4.2
	* (Common) Added TOAST Launching information in JSON string format to LaunchingInfo
	* (iOS) Updated LINE SDK (v5.0.1)
		* Changed the minimum supported OS version of LINE Adapter to iOS 10
		* Added login feature via LINE app

<a id="game-gamebase-8-3"></a>
#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed an Analytics bug where stored metrics data was not reset upon logout, withdrawal, and account transfer
	* (iOS) Fixed an issue where the app crashed intermittently due to network connection issues

<a id="game-gamebase-9"></a>
### 2019. 06. 13. { #game-gamebase-9 }

<a id="game-gamebase-9-1"></a>
#### Bug Fixes
* [SDK] 2.4.1
	* (iOS) Fixed a bug where some parameters were missing when sending Analytics metrics, causing metrics to not display correctly
	
<a id="game-gamebase-10"></a>
### May 28, 2019 { #game-gamebase-10 }

<a id="game-gamebase-10-1"></a>
#### Added Features
* Added HANGAME mix Japan payment
    * [SDK] 2.4.0
    	* (Unity) Added Standalone Japan external payment
    	* (Unity) Added Standalone Japan HANGAME authentication
    * [Console] 
    	* Purchase > Store: Added 'HANGAME mix(JAPAN)' store
    	* App > Client: Added store settings when registering a Windows client
    	* App > Installation URL: Added store settings when adding a Windows installation URL

<a id="game-gamebase-10-2"></a>
#### Feature Updates
* [SDK] 2.4.0
	* (Common) Changed metrics-related classes
        * LevelUpData class: The userLevel and levelUpTime parameters are now required; other fields have been removed [For more information, see [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#game-user-data-settings)]
        * GameUserData class: Added the classId (game user's job) field [For more information, see [Android](http://docs.toast.com/ko/Game/Gamebase/ko/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/ko/Game/Gamebase/ko/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/ko/Game/Gamebase/ko/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/ko/Game/Gamebase/ko/js-etc/#level-up-trace)]
    * (Android) Updated Naver SDK version (v4.2.5): Fixed a Naver SDK bug (fixed an issue where the authentication process was interrupted due to the Activity force-closing when the app was restarted via the app icon during Naver login)
    * (Unity) StandaloneWebview now supports 32-bit builds (SDK size increased from 53.6 MB to 99.2 MB)
* [Server]
    * Updated LTV query and fixed failover logic
* [Console]
    * Added LTV Grid ComplexColumns support and Excel download support

<a id="game-gamebase-11"></a>
### May 16, 2019 { #game-gamebase-11 }

<a id="game-gamebase-11-1"></a>
#### Added Features
* [Console]
	* Added a device transfer feature (new menu)
		* App > Device Transfer (Transfer account): Saves settings for using the device transfer feature
		* Member > Device Transfer: Views the status and history of issued keys

<a id="game-gamebase-11-2"></a>
#### Feature Updates
* [Console]
	* App: Turned off the 'Token Re-verification' option for AppleGameCenter and China IdP
		* Removed this option as it is meaningless because these IdPs only check the internal token without checking the actual external IdP
	* Member: Changed the conditions under which mapping can be added
		* Before: Guest account
		* After: Guest account, Missing account

<a id="game-gamebase-11-3"></a>
#### Bug Fixes
* [SDK] 2.3.1
	* (Android) Fixed an issue where Twitter login did not work in version 2.3.0
* [Console]
	* Member: Fixed an issue where receipt verification did not work in purchase history
	* Kickout: Fixed an issue of abnormal operation by adding authentication checks for lookup requests
	
<a id="game-gamebase-12"></a>
### 2019. 04. 23. { #game-gamebase-12 }

```
With Gamebase, you can integrate with more than 50 Chinese stores.
If you are interested in launching in China, contact our customer support.
```

<a id="game-gamebase-12-1"></a>
#### Added Features
* [SDK] 2.3.0
	* (Android/Unity) Added authentication and payment for China stores

<a id="game-gamebase-12-2"></a>
#### Feature Updates
* [SDK] 2.3.0
	* (Common) Added Launching Status Code: "Under Review (204)", "Under Test (203)"
	* (Android) Modified the behavior so that the AuthToken is not deleted when login via the most recently logged-in provider fails or when a WebSocket response failure occurs (timeout, network disable, etc.)
	* (Android) Fixed a memory leak that occurred inside AuthAdapter during IdP login

<a id="game-gamebase-13"></a>
### 2019. 04. 11. { #game-gamebase-13 }

<a id="game-gamebase-13-1"></a>
#### Feature Updates
* [SDK] 2.2.2
	* (Unity) Improved SDK logs
* [Console]
	* Applied multi-language support to the Analytics menu
	* Patched security vulnerabilities related to security review

<a id="game-gamebase-13-2"></a>
#### Bug Fixes
* [SDK] 2.2.2
	* (Android) Fixed an issue where the callback was not received when the TransferAccount API was called before Gamebase initialization
	* (iOS) Fixed an issue where the Gamebase initialization callback was not called when showBlockingPopup was set to NO
	* (Unity) Fixed a crash that occurred when calling the AddMappingForcibly API

<a id="game-gamebase-14"></a>
### 2019. 04. 02. { #game-gamebase-14 }

<a id="game-gamebase-14-1"></a>
#### Bug Fixes
* [SDK] 2.2.1
	* (Unity) Fixed an issue where a server error occurred during initialization when running play mode with the Android platform selected in Unity Editor

<a id="game-gamebase-15"></a>
### 2019. 03. 26. { #game-gamebase-15 }

<a id="game-gamebase-15-1"></a>
#### Added Features
* Added TransferAccount feature: allows guest users to transfer to a new device using up to 2 keys without mapping
    - (SDK Common) Added APIs 
		* TransferAccountInfo issuance API (issueTransferAccount)
		* API to request account transfer using issued TransferAccountInfo (transferAccountWithIdPLogin)
		* API to query issued TransferAccountInfo (queryTransferAccount)
		* API to renew already issued TransferAccountInfo (renewTransferAccount)		
	- (Server API)
		* Server API to validate the ID/PW of issued TransferAccount (validateTransferAccount)
    - (Console) Transfer history can be viewed in the Mapping History tab of the Members menu
* Added forced mapping feature: allows mapping of an IdP account that is already linked to another account
	- (SDK Common) Added APIs 
		* API for forced mapping (addMappingForcibly)

<a id="game-gamebase-15-2"></a>
#### Feature Updates
* [SDK] 2.2.0
	* (Android) Updated IAP SDK to the latest version, v1.5.3
	* (iOS) Disabled the App login feature of LINE SDK
		* Due to a bug in LINE SDK v4 that caused app login to fail on iOS 12, Gamebase Line Adapter was changed to support Web login only
	* (Unity) Changed the package name of GamebaseMainActivity
		* com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

<a id="game-gamebase-16"></a>
### 2019. 02. 26. { #game-gamebase-16 }

<a id="game-gamebase-16-1"></a>
#### Feature Updates
* [SDK] 2.1.0
	* (Common) Removed TransferKey APIs
		* issueTransferKey: Issue TransferKey
		* requestTransfer: Validate TransferKey
		
<a id="game-gamebase-16-2"></a>
#### Bug Fixes
* [SDK] 2.1.0
	* (Android) Fixed a bug where onActivityResult() was called before Gamebase initialization, causing abnormal behavior
	* (iOS) Fixed a bug where there was no response when attempting Gamecenter login through Gamebase after logging in to Gamecenter through logic other than Gamebase

<a id="game-gamebase-17"></a>
### January 29, 2019 { #game-gamebase-17 }

```
To use the improved overall metrics in Gamebase 2.0, you must update the SDK.
```

<a id="game-gamebase-17-1"></a>
#### Added Features
* Console
	* Analytics: Released new Gamebase 2.0 metrics
	* App: Added a feature to change the client's debug log level in real time
* [SDK] 2.0.0
	* (Common) Added APIs for custom metrics (purchases are sent automatically within the SDK upon success)
		* setGameUserData: Sends user level information after game login
		* traceLevelUpData: Called when a game user levels up to track level-up events
    * (JavaScript) Released new SDK

<a id="game-gamebase-17-2"></a>
#### Feature Updates
* [SDK] 2.0.0
	* (Android) Updated Push SDK (android:1.7.0)
	* (Android) Changed Adapter API
		* Passes Launching information
		* Added Callback to logout and withdraw APIs
	* (iOS) Updated IAP SDK
		* Fixed an issue where a crash occurred intermittently upon payment failure

<a id="game-gamebase-17-3"></a>
#### Bug Fixes
* [SDK] 2.0.0
	* (iOS) Fixed an issue where a crash occurred when initializing Gamebase with debugMode enabled on simulators running iOS 12 or later

<a id="game-gamebase-18"></a>
### December 27, 2018 { #game-gamebase-18 }

<a id="game-gamebase-18-1"></a>
#### Added Features
* Console
	* Push - Added a recurring delivery feature

<a id="game-gamebase-18-2"></a>
#### Feature Updates
* [SDK] 1.14.5
	* The following APIs that were previously deprecated have been removed:
		* (void)Gamebase.WebView.showWebBrowser(Activity, String)
		* (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
		* (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
	* Updated the payment module (gamebase-adapter-purchase-iap).
		* Updated IAP SDK to version 1.5.2.
		* Removed the IAP TEST Store that was not used in the client.
		* Fixed an issue where the call failed when data was incomplete in the retry transaction logic (requestRetryTransaction).
		* Added exception handling to all IAP SDK call points to prevent crashes.
* Console
	* Modified to redirect to the login page when authentication expires, including REST API requests.
	* Added filters for querying IAP transactions.

<a id="game-gamebase-19"></a>
### November 15, 2018 { #game-gamebase-19 }

<a id="game-gamebase-19-1"></a>
#### Added Features
* Console
	* Added Chinese language support
	* Members: Added App Store receipt verification feature to purchase history

<a id="game-gamebase-19-2"></a>
#### Feature Updates
* Console
	* Added multi-language support for the calendar
* [SDK] 1.14.2
	* (Android) Changed the type of epoch time, which represents the maintenance start/end time in the data structure, from String to long: Fixed an issue where the callback was not triggered due to a type mismatch when calling maintenance after integrating with Gamebase Unity.
	* (iOS) Fixed a structure that could cause a crash when using Gamebase iOS SDK 1.14.0 and Unity Plugin 1.14.0, due to a change in the JSON string structure of the description method of the TCGBAuthProviderProfile object returned when calling the provider profile retrieval method.

<a id="game-gamebase-19-3"></a>
#### Bug Fixes
* Console
	* Push: Fixed an issue where registration failed when copying and registering a push notification that was registered after sending to a specific target.
* [SDK] 1.14.2
	* (Android) Fixed a crash bug that occurred when selecting "Install/Update App" in an emulator environment without a store app (PlayStore, OneStore, etc.) due to the store not being checked.
	* (Unity) Fixed an issue where a crash occurred when calling the ShowWebView API without a Callback in the parameters.
	* (Unity) Fixed a bug where a compilation error occurred due to code that called a deleted API from the iOS SDK.

<a id="game-gamebase-20"></a>
### October 23, 2018 { #game-gamebase-20 }

<a id="game-gamebase-20-1"></a>
#### Added Features
* Console
	* IAP: Added a feature to verify App Store receipts in the IAP payment information menu.
* [SDK] 1.14.0
	* (Common) Added a file attachment feature to Gamebase WebView: This does not work properly on Android API 19 (KitKat).
	
<a id="game-gamebase-20-2"></a>
#### Feature Updates
* Console
	* IAP: Improved the search conditions for downloading payment history in the IAP payment information menu (from 1 day to 30 days).
* [SDK] 1.14.0
	* (Common) Modified the system to URL-encode messages written by users in the console for bans/maintenance, and decode them on the client side for processing.
	* (iOS) Updated Payco SDK to version 1.2.4.
	* (Unity) Improved the system so that duplicate objects are not created when returning to a scene that contains a GamebaseSDKSetting object.
	* Remove API: Webview, Network, Launching
		* (Android) 5
			- (void)Gamebase.WebView.showWebBrowser(Activity, String)
			- (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
			- (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
		* (iOS) 9
			- [TCGBUtil showToastWithMessage:duration:]
			- [TCGBWebView showWebBrowserWithURL:viewController:]
			- [TCGBWebView showWebViewWithURL:viewController:configuration:]
			- [TCGBLaunching addObserverOnChangedStatusNotification:]
			- [TCGBLaunching removeObserverOnChangedStatusNotification:]
			- [TCGBLaunching addUpdateStatusNotification]
			- [TCGBLaunching removeUpdateStatusNotification]
			- [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
			- [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
		* (Unity) 7
			- ShowWebBrowser(string url)
			- ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
			- ShowToast(string message, int duration)
			- AddUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback) 
			- RemoveUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
			- AddOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			- RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			
	* Deprecated API
		* (Android) 2
			- (void)Gamebase.WebView.showWebView(Activity, String)
			- (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
		* (iOS) 1
			- [TCGBGamebase languageCode]
		* (Unity) 1
			- GetLanguageCode()
* [SDK] Setting Tool		
	* Improved popup and UI.
	
<a id="game-gamebase-20-3"></a>
#### Bug Fixes
* [SDK] 1.14.1
	* (Android) Fixed an issue where Auth API calls did not work properly when the same Auth API was called again from within the callback after an initial Auth API call.
	
<a id="game-gamebase-21"></a>
### October 11, 2018 { #game-gamebase-21 }

<a id="game-gamebase-21-1"></a>
#### Bug Fixes
* Console
	* Ban: Fixed an error that occurred during batch registration.
	
<a id="game-gamebase-22"></a>
### September 20, 2018 { #game-gamebase-22 }

<a id="game-gamebase-22-1"></a>
#### Bug Fixes
* Console
	* Management: Fixed an issue where alarm page processing failed due to a page address error.

<a id="game-gamebase-23"></a>
### September 13, 2018 { #game-gamebase-23 }

<a id="game-gamebase-23-1"></a>
#### Added Features
* Console	
	* Members: Added features to add and delete IdP from an account, and added a feature to search by IdP ID
	* Push: Added a feature to query the delivery history by push status
* [SDK] 1.13.0
	* (iOS) Added an API to support App Store Promotion IAP


<a id="game-gamebase-23-2"></a>
#### Feature Updates
* [SDK] 1.13.0
	* (Common) Applied the latest IAP SDK version (android:1.5.1, iOS:1.6.0)
	* (Android) Improved error messages for Push API call failures based on the Gamebase initialization/login status
		* Call before initialization: NOT_INITIALIZED(1)
		* Call after initialization when the Push module is absent: NOT_SUPPORTED(10)
		* Call after successful initialization but before login: NOT_LOGGED_IN(2)		
	* (iOS) Changed the call result structure of the `authProviderProfileWithIDPCode` API to 1 depth (to align with Android and Unity)
	* (Unity) Improved the output format for JSON data shown in logs for better readability
* Console
	* Ban: Improved the UI for registering bans using AppGuard — data is reset when the feature is turned off, and Leaderboard data deletion settings are displayed only when the status is 'on'
	
<a id="game-gamebase-23-3"></a>
#### Bug Fixes
* [SDK] 1.13.0
	* (Android) Fixed an error that occurred during Naver login due to a conflict with the NaverCafe SDK
	* (Unity) Fixed an error that occurred in websocket close processing when exiting Editor Play Mode in Unity 2017.2 or later
* Console
	* App: Fixed an issue where content after the delete button was cut off when editing information
		
<a id="game-gamebase-24"></a>
### August 28, 2018 { #game-gamebase-24 }

<a id="game-gamebase-24-1"></a>
#### Added Features
* Console	
	* Member: Added feature to change account status, added Push Token lookup
	* Operational metrics (user statistics): Added metrics for users who withdrew today and users who withdrew on the same day they registered

<a id="game-gamebase-24-2"></a>
#### Feature Updates
* [SDK] 1.12.2
	* (Android) Added defensive logic against a potential crash bug that could occur on WebSocket timeout (API call time elapsed)
	* (iOS) Improved Callback URL Scheme configuration for Google Auth Adapter and Naver Auth Adapter
		* Improved so that the Default URL Scheme is set when "url_scheme_ios_only" is not configured in the console. To use the Default URL Scheme, register tcgb.{Bundle ID}.google or tcgb.{Bundle ID}.naver in XCode > Target > Info > URL Types.
	* (iOS) Improved Payco Auth Adapter
		* Fixed an issue where an unintended URL Scheme was invoked due to URL Scheme not being configured. The configuration method has changed; you must configure the URL Scheme to update (register tcgb.{Bundle ID}.payco in XCode > Target > Info > URL Types).
* Console
	* Member: Added ID mapping history lookup feature (changed from viewing the last 3 months to allowing direct configuration of the lookup period)
	* Purchase (IAP): Limited payment information Excel download to 1 day, removed item deletion feature
	
<a id="game-gamebase-24-3"></a>
#### Bug Fixes
* [SDK] 1.12.2
	* (Android) Fixed an issue where an initialization error occurred when building with TargetSdk 28 while including auth-twitter-adapter

<a id="game-gamebase-25"></a>
### August 9, 2018 { #game-gamebase-25 }

<a id="game-gamebase-25-1"></a>
#### Feature Updates
* [SDK] 1.12.1
	* (Common) Applied the latest version of IAP SDK (1.5.0)
	* (Common) Improved the Gamebase maintenance page to display the maintenance time according to the device's country time settings
	* (Common) Added a feature to use maintenance information entered in the Console when using an external page for the maintenance page
	* (Common) An error is now returned when a user mapped to an IdP attempts Guest mapping (TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
	* (Common) An error is now returned when authentication APIs are called redundantly (AUTH_ALREADY_IN_PROGRESS_ERROR)
	* (Android) Updated TencentPush SDK (3.2.3)
	* (Android) Added support for Onestore v17 (API v5): Gamebase does not support v16 (store code = TS).
	* (iOS) Added an error code: Game Center login denied (TCGB_ERROR_IOS_GAMECENTER_DENIED)
* [SDK] Setting Tool
	* Changed the folder name: TOAST → Toast
	* Added popup notifications for errors: File Download failure, File Extract failure, XML parsing failure

<a id="game-gamebase-25-2"></a>
#### Bug Fixes
* [SDK] 1.12.1
	* (iOS) Fixed a bug where login was impossible due to a failure to retrieve profile information during Naver login: Changed so that login succeeds even when profile information retrieval fails
* Console
	* Purchase history: Fixed a bug where the payment status could not be changed from the 'Reserved' status, and an issue where filtering was not applied when downloading Excel

<a id="game-gamebase-26"></a>
### July 24, 2018 { #game-gamebase-26 }

<a id="game-gamebase-26-1"></a>
#### Feature Updates
* [SDK] 1.12.0
	* (iOS) Added a feature to output version information of adapters in use and the app's build information in Debug Log during Gamebase initialization.
	* (iOS) The binary of the Naver ID Login SDK that was included in the Naver Auth Adapter distributed via CocoaPods has been removed and changed to a dependency configuration method.
* Console
	* Applied restrictions on the service statuses that can be selected when registering a web client: Update Recommended and Update Required cannot be selected.
* [SDK] Setting Tool
	* Added an update notification feature when a newer version of Setting Tool is available.
	* Fixed an internal null exception.

<a id="game-gamebase-26-2"></a>
#### Bug Fixes
* [SDK] 1.12.0
	* (Unity) Fixed a bug where an exception occurred when calling the IssueTransferKey API.
	* (Unity) Removed the Unity Google Adapter: developers currently using GoogleAdapter should refer to the update guide below.

**Unity Google Adapter Update Guide**

* If you are using Unity SDK version 1.6.0 or later and below 1.12.0, you must read the following carefully before updating to version 1.12.0. (If you are using a version earlier than 1.6.0, this does not affect you because GoogleAdapter is not used.)
	1. Setting Tool configuration
        * As GoogleAdapter has been removed, the Google item is no longer displayed under the Unity tab.
        * To use Google authentication, enable the Google item under each platform tab.
            * Android > Authentication > Select Google to configure.
            * iOS > Authentication > Select Google to configure.
    2. Gamebase Login API (no changes)
        * Gamebase.Login(GamebaseAuthProvider.GOOGLE, callback);
    3. If you use GPGS features
        * Keep the GPGS SDK for Unity.
        * Manage GPGS-related logic separately in the app.
    4. If you do not use GPGS features
        * Delete the GPGS SDK for Unity.

<a id="game-gamebase-27"></a>
### 2018. 07. 05. { #game-gamebase-27 }

<a id="game-gamebase-27-1"></a>
#### Added Features
* Added Line IdP: iOS

<a id="game-gamebase-27-2"></a>
#### Feature Updates
* [SDK] 1.11.1
	* (Common) Changed to log in using the IdP account for which AddMapping succeeded, when loginForLastLoggedInProvider is called after a successful AddMapping following a Guest login.

<a id="game-gamebase-27-3"></a>
#### Bug Fixes
* [SDK] 1.11.1
	* (Common) Fixed a bug where subsequent APIs (login, push, purchase, etc.) did not proceed after maintenance ended.
	* (Android) Fixed a bug where the type of ObserverMessage.data.code was String instead of int when an ObserverMessage was received through Gamebase.addObserver().
* Console
	* Fixed an issue where the store code was incorrectly registered when registering a Windows client.

<a id="game-gamebase-28"></a>
### June 26, 2018 { #game-gamebase-28 }

<a id="game-gamebase-28-1"></a>
#### Added Features
* Added iOS Google IdP: iOS
* Added Twitter IdP: Android, iOS
* Added Line IdP: Android only. iOS is scheduled for July 2018.
* Added Server API
	* getSimpleLaunching: API for checking Launching information provided when the client app starts

<a id="game-gamebase-28-2"></a>
#### Feature Updates
* [SDK] 1.11.0
	* (Common) Added Japanese translations for LocalizedString
	* (Common) Improved internal logic to clearly distinguish error codes when initialization or login has not been performed upon authentication API calls
	* (Android) Removed the `android.permission.READ_PHONE_STATE` permission
	* (Android) Changed GamebaseConfiguration.Builder so that the required settings setAppId and setAppVersion can be entered in the constructor
	* (Android) Removed the setServerApiVerseion API from GamebaseConfiguration.Builder
	* (Android) Renamed the getAuthBanInfo() API and class AuthBanInfo: getBanInfo(), class BanInfo
	* Updated Naver ID Login SDK: iOS (4.0.10)
* Sample App
	* Added ServerPush and Observer features
	* Updated Gamebase SDK: Android (1.9.0), iOS (1.9.0), Unity (1.10.1)

<a id="game-gamebase-29"></a>
### June 11, 2018 { #game-gamebase-29 }

<a id="game-gamebase-29-1"></a>
#### Bug Fixes
* [SDK] 1.10.1
	* (Unity) Fixed a bug where calling the AddMapping API without a Unity Adapter was internally handled as a login

<a id="game-gamebase-30"></a>
### June 7, 2018 { #game-gamebase-30 }

<a id="game-gamebase-30-1"></a>
#### Added Features
* [SDK] 1.10.0
	* (Unity) StandaloneWebviewAdapter: Added support for HTML source rendering

<a id="game-gamebase-30-2"></a>
#### Feature Updates
* [SDK] 1.10.0
	* (Unity) Modified the interface of Unity Adapter
		* When using v1.10.0 or later, a UnityAdapter version upgrade is required (GamebaseUnitySDK_FacebookAdapter_v1.5.0, GamebaseUnitySDK_StandaloneWebviewAdapter_v1.7.0)
	* (Unity) Changed the logic so that when a Unity Adapter is not present during a Login API call, the native (Android/iOS) login API is called instead: Facebook, Google
	* (Unity) Fixed folder structure and name typos for each Adapter
		* Path: Assets/Gamebase/Scripts/Adapter => Assets/Gamebase/Adapter
		* Typo: Adapater => Adapter

<a id="game-gamebase-31"></a>
### May 29, 2018 { #game-gamebase-31 }

<a id="game-gamebase-31-1"></a>
#### Added Features
* [Console] Added a download feature for operating indicators
	* Monitoring > Concurrent User Changes
	* User Statistics > Daily Indicator Changes
	* Group Concurrent Users > Daily Group Concurrent User Changes


<a id="game-gamebase-31-2"></a>
#### Bug Fixes
* [SDK] 1.9.1
	* (iOS) Fixed an issue where the title, back button, and close button were not displayed in the Gamebase WebView NavigationBar area

<a id="game-gamebase-32"></a>
### May 18, 2018 { #game-gamebase-32 }

<a id="game-gamebase-32-1"></a>
#### Feature Updates
* [SDK] 1.9.0
	* Redeployed Unity SDK (1.9.0) with the Google Adapter replaced with a new version (1.6.2)
    	- Replaced the Google Adapter applied to Unity SDK (1.9.0) released on May 3 with the latest version (1.6.1 -> 1.6.2)

<a id="game-gamebase-33"></a>
### May 3, 2018 { #game-gamebase-33 }

<a id="game-gamebase-33-1"></a>
#### Added Features
* Added the Transfer feature
    - A feature that allows guest users to transfer to a new device without mapping
    - (SDK Common) Added APIs
		* Transfer key issuance API (IssueTransferKey)
		* API for requesting account transfer using the issued TransferKey (RequestTransfer)
    - (Console) Transfer history can be viewed on the Mapping History tab in the Members menu
* Added an option to delete the user's leaderboard (ranking) data when registering a ban (applicable only when using TOAST Leaderboard)
    - Available from the Ban registration menu or the App Guard integration page

<a id="game-gamebase-33-2"></a>
#### Bug Fixes
* [SDK] 1.9.0
	* (iOS) Fixed an issue where login failed when attempting App to Web login while logging in with a Naver account, due to a change in the format of the scheme received from the server
    * (iOS) Fixed a bug where the message and UnderlyingError were not set in the logic that creates the error object passed to the user after receiving the UnderlyingError object from the Adapter
    * (Android) Fixed an issue where the ban popup did not appear when a user was determined to be invalid in Heartbeat (modified to use the same logic as iOS)

<a id="game-gamebase-34"></a>
### April 12, 2018 { #game-gamebase-34 }

<a id="game-gamebase-34-1"></a>
#### Bug Fixes
* [SDK] 1.8.1
	* (Android. iOS) Fixed a bug where registerPush failed when null was passed as the displayLanguageCode when calling registerPush

<a id="game-gamebase-35"></a>
### April 9, 2018 { #game-gamebase-35 }

<a id="game-gamebase-35-1"></a>
#### Bug Fixes
* [SDK] 1.8.1
	* (Unity) Fixed a NullReferenceException caused by a module initialization failure when using the following features on the UnityAndroid platform
		* Launching, Purchase, Push, Util, Webview

<a id="game-gamebase-36"></a>
### 2018. 04. 05. { #game-gamebase-36 }

<a id="game-gamebase-36-1"></a>
#### Added Features
* Added kick out feature
    - A feature to disconnect all users currently in the game (can be used when you want to disconnect all users from the game during maintenance)
    - (Console) Menu added
    - (SDK Common) Added API to receive kick out events
* Improved the maintenance webpage so that users can use an HTML page entered in the Console
    - Previously, only Gamebase-provided webpages or external webpage connections were available
    - Users can now create a maintenance page in the format they want, even without a web server
* Developed Observer feature and added APIs
    - (SDK Common) Added API to batch-process listeners for app status/network status/user status (ban) changes such as maintenance, by registering an Observer

<a id="game-gamebase-36-2"></a>
#### Feature Updates
* [SDK] 1.8.0
	* (Common) The following APIs are deprecated due to the addition of the Observer feature: LaunchingStatus Listener, Network Listener (existing users can continue to use them)
	* (iOS) Applied PAYCO Simple Login 3rd SDK v1.2.2: provides token expiration information (expires_in) upon successful login, improved iPhoneX login UI
	* (iOS) Modified the Webview interface to support iPhoneX

<a id="game-gamebase-36-3"></a>
#### Bug Fixes
* Fixed an issue where concurrent user data was not saved when the country code (country code) was 10 or more characters long
* [SDK] 1.8.0
	* (Setting Tool) Fixed a bug where an error occurred when checking Unity Facebook Adapter

<a id="game-gamebase-37"></a>
### 2018. 03. 13. { #game-gamebase-37 }

<a id="game-gamebase-37-1"></a>
#### Bug Fixes
* [SDK] 1.7.1
	* (Unity) Fixed a bug where the SetDebugMode value set in the Inspector was not applied
	* (Unity) Standalone, WebGL: Fixed missing resource files used in Display Language
	* (Unity) Released Google Adapter 1.6.2: Fixed a bug in Google Adapter 1.6.1 where AuthCode was returned as empty, causing authentication failure

<a id="game-gamebase-38"></a>
### 2018. 02. 22. { #game-gamebase-38 }

<a id="game-gamebase-38-1"></a>
#### Added Features
* [SDK] 1.7.0
	* Added Naver IdP authentication
	* Added Display Language setting: Added a Display Language setting that allows game users to set the language displayed in the game separately from the device language.

<a id="game-gamebase-39"></a>
### January 25, 2018 { #game-gamebase-39 }

<a id="game-gamebase-39-1"></a>
#### Added Features

* [Console]
	* [Push] Added a feature to copy PUSH input values
	* [Operating indicator > Group concurrent users] Added a daily graph showing changes in group concurrent users

* [SDK] 1.6.0
	* (Unity) Added Standalone WinSDK
		* 64-bit support
		* Authentication support: Facebook, Google, PAYCO

<a id="game-gamebase-39-2"></a>
#### Feature Updates
* [Console]
	* [Operating indicator > Monitoring] Fixed an issue where system maintenance items configured before project creation were displayed
	* [App > App] Improved the test device registration screen — improved to allow easy device registration based on User ID login history
	* [Operation > Maintenance] Improved the maintenance preview screen

<a id="game-gamebase-39-3"></a>
#### Bug Fixes
* [SDK] 1.6.0
	* (iOS) Added a defensive logic to handle potential crashes when calling WebView


<a id="game-gamebase-40"></a>
### December 21, 2017 { #game-gamebase-40 }

<a id="game-gamebase-40-1"></a>
#### Added Features

* [Console]
	* [Push] Added the local time push feature
	* [Operating indicator > Sales Status] Added revenue charts by market
	* [Operating indicator > User Statistics] Added a menu to check user metric trends since the app launch
	* [Operation > Maintenance] Added a method for registering the maintenance page displayed to users during maintenance
		* Previous: Gamebase-provided page and external page URL input
		* Added: A feature to pass maintenance details entered in the console to an external page

* [SDK] 1.5.0
	* Added a close callback that fires when the WebView is closed
	* Added a feature to receive events from custom schemes used in the WebView
	* Released a new Unity Setting Tool

<a id="game-gamebase-40-2"></a>
#### Feature Updates
* [Console]
	* [App > Client] Modified so that user-facing message information previously registered in the game can be reused when the client status changes

<a id="game-gamebase-40-3"></a>
#### Bug Fixes
* [SDK] 1.5.0
	* (Unity) Fixed an issue where Guest login did not work in UnityEditor
	* (Unity) Added defensive code to handle a KeyNotFoundException that occurred when calling the Gamebase.Login("facebook") API without registering Facebook authentication information in TOAST Console


<a id="game-gamebase-41"></a>
### November 30, 2017 { #game-gamebase-41 }

<a id="game-gamebase-41-1"></a>
#### Added Features

* [Console]
	* [Management > Alarm] Added feature to register Webhook alarms
	* [Operating indicator > Monitoring] Added Push delivery history lookup

<a id="game-gamebase-41-2"></a>
#### Feature Updates
* [Console]
	* [Operating indicator > Monitoring] Changed chart colors and resolved Timezone issue. Changed the DAU calculation logic (from login time to access time)
* [API] Changed the [Check Maintenance API](./api-guide/#check-maintenance-set) response from a list to a single object

<a id="game-gamebase-41-3"></a>
#### Bug Fixes
* [Console]
	* [Push] Fixed an issue where Push was registered even when the default language was not selected


<a id="game-gamebase-42"></a>
### November 23, 2017 { #game-gamebase-42 }

<a id="game-gamebase-42-1"></a>
#### Added Features

* [SDK] 1.4.0 Update
	* (Unity) Added Gamebase Facebook Adapter: supports Android, iOS, WebGL, Standalone Platform, and UnityEditor

<a id="game-gamebase-42-2"></a>
#### Feature Updates
* [SDK] 1.4.0 Update
	* (iOS) Fixed an issue where "x", "<", or other text was displayed when the close/back button resource was missing; replaced with a default value

<a id="game-gamebase-42-3"></a>
#### Bug Fixes
* [SDK] 1.4.0 Update
	* (Android) Fixed an issue where ban information was returned as null when the Gamebase-provided popup was not used
	* (iOS) Fixed an issue where the NavigationBar title was reset when the device was rotated after launching a WebView
	* (iOS) Fixed an issue where the NavigationBar background was displayed overlapping when customizing the NavigationBar height of the WebView

<a id="game-gamebase-43"></a>
### October 26, 2017 { #game-gamebase-43 }

<a id="game-gamebase-43-1"></a>
#### Added Features

* [SDK] 1.3.0 Update
	* Added AddMapping API using Credential

<a id="game-gamebase-43-2"></a>
#### Feature Updates
* [Console]
	* Applied message handling based on TC Push error codes
	* Changed the ban template message registration screen from Input Textbox to TextArea
	* Fixed an issue where the management menu was not displayed correctly due to the addition of new TC permissions
* [SDK] 1.3.0 Update	
	* (Unity) Fixed a bug where JSON parsing failed in iOSPlugin when calling the Login API using CredentialInfo
	
<a id="game-gamebase-44"></a>
### September 21, 2017 { #game-gamebase-44 }

<a id="game-gamebase-44-1"></a>
#### Added Features

* Added ban (user penalty) feature
* [SDK] 1.2.0 Update
	* Added popup display for banned users
* [Console]
	* Added customer support (email, phone number) registration
	* Opened Ban menu
	* Member menu: Added user purchase history lookup feature


<a id="game-gamebase-44-2"></a>
#### Feature Updates

* [Console]
	* Applied time display based on user's country in each menu
	* Applied decimal price handling in sales status
	* Added multi-language support for concurrent user change notifications (English/Korean selectable)

<a id="game-gamebase-45"></a>
### August 24, 2017 { #game-gamebase-45 }

<a id="game-gamebase-45-1"></a>
#### Feature Updates

* [SDK] 1.1.6 Update
	* Added Push API (for iOS): SetSandboxMode

<a id="game-gamebase-46"></a>
### July 20, 2017 { #game-gamebase-46 }

<a id="game-gamebase-46-1"></a>
#### Feature Updates

* Added a daily batch job to delete related data when the Gamebase product service is suspended
* [SDK] 1.1.5 Update
	* Added system popup API (showAlertWithTitle)
	* Changed country code to be returned in uppercase (Android)
	* Updated TCPush SDK to 1.4.1
	* Updated IAP SDK to 1.3.3.20170627
* [Console]
	* Added TrackingTime display for tracing errors in external integration modules

<a id="game-gamebase-47"></a>
### May 25, 2017 { #game-gamebase-47 }

<a id="game-gamebase-47-1"></a>
#### Feature Updates

* Added a daily batch job to delete related data when the Gamebase product service is suspended
* [SDK] 1.1.4 Update
	* Added an API for changing the payment store at runtime
	* (Android) Applied TCPushSdk v1.4 and added Tencent Push support
* [Console]
	* Added multi-language support
	* Added audit log for Create, Update, and Modify operations in all menus

<a id="game-gamebase-48"></a>
### April 20, 2017 { #game-gamebase-48 }

<a id="game-gamebase-48-1"></a>
#### Feature Updates

* [SDK] 1.1.3 Update
	* (Android) Improved launching structure and popup/maintenance page: added custom maintenance page configuration
	* (Android) Improved authentication structure and added logs: outputs authentication adapter and SDK version logs

<a id="game-gamebase-48-2"></a>
#### Bug Fixes
* [SDK] 1.1.3 Update
	* (Android) Fixed a crash that occurred during initialization with Facebook SDK v4.19.0 or later


<a id="game-gamebase-49"></a>
### April 4, 2017 { #game-gamebase-49 }

<a id="game-gamebase-49-1"></a>
#### Feature Updates
* [SDK] 1.1.2 Update
    * Improved maintenance and urgent notice popups at game launch
    * Added Unity Plugin debug logs and detailed exception handling
* [API] Integrated [IAP](./api-guide/#purchase-iap) API: item inquiry and unconsumed purchase history inquiry
* [API] Added a spec to include IdP information used at login in the checkAccessToken API response
* [Console] Maintenance, urgent notice: Changed to not display stores not used in the game when selecting client versions

<a id="game-gamebase-49-2"></a>
#### Bug Fixes
* [Console] Fixed abnormal market display when registering iOS clients

<a id="game-gamebase-50"></a>
### March 21, 2017 { #game-gamebase-50 }

<a id="game-gamebase-50-1"></a>
#### Feature Updates
* [SDK] 1.1.0 Update
    * Added an interface to receive an external AccessToken and perform idPLogin
    * [Added UI features](./aos-ui): Custom Webview, AlertDialog
* [API] Integrated [Leaderboard](./api-guide/#leaderboard) and [IAP](./api-guide/#purchase-iap) APIs

<a id="game-gamebase-51"></a>
### March 9, 2017 { #game-gamebase-51 }

<a id="game-gamebase-51-1"></a>
#### New Product Release
* A service that provides common features needed for games, enabling efficient game development.
	* Supports various authentication methods: Guest and third-party (Google, Facebook, GameCenter, etc.) authentication
	* Provides logout and account withdrawal features
	* Provides a mapping feature that allows a single user to use multiple external IDPs simultaneously
	* Provides game app status management, maintenance, urgent notices, and other game operation features through a web console
	* Provides a web console screen for viewing real-time operational metrics
	* Integrated with TOAST Cloud products: PUSH, IAP