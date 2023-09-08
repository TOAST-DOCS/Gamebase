## Game > Gamebase > Release Notes > Console

### 2023. 07. 25.

#### Feature Updates
* App > App
    * Improved so that administrator can directly set additional parameters for NHN Cloud Online Contact Customer Center
* Customer Center > Customer Inquiry
    * Improved so that, when pressing the Cancel/Save button after entering the details page from the customer inquiry page list, move to the previous page list.
* Customer Center > Customer Inquiry
    * Added a language item to the customer inquiry reception search item (But, all previously created inquiries are applied in the default language, and new inquiries can be searched in the set language)

#### Bug Fixes
* Push > Push
    * Fixed an error where, when repeatitive push notification was set for time zones other than UTC+9, the sending time was incorrectly set

### 2023. 07. 11.

#### Feature Updates
* Operation > Kickout
    * Improved so that, when only some clients are selectable, clients with the Update Required status can be seletable
* Mobile Customer Center
    * Improved so that the popup window can be closed with the go back button on the device

#### Bug Fixes
* App > Deploy Terms and Conditions
    * Fixed an error that allowed deployment to a country where deployment was already conducted
* Mobile Customer Center
    * Fixed an error that caused unexposed posts to be retrieved via FAQ search

### June 27, 2023

#### Added Features
* Purchase (IAP) > Store
	* Added ONE store API v7(SDK v21)
* Purchase (IAP) > Payment Information
	* Added a feature to view payment details

#### Feature Updates
* App > App
    * Changed the URL of token verification entered in NHN Cloud Online Contact screen from `https://gamebase-web.cloud.toast.com` to `https://web-gamebase.nhncloud.com`

### June 13, 2023

#### Feature Updates
* App > App
    * Add a universalLink entry to Additional information for Weibo login authentication.
	
### May 16, 2023

#### Added Features
* Purchase (IAP) > Store
    * Added MyCard store
* Coupon > Issue Coupon
    * Added Galaxy store

### April 25, 2023

#### Added Features
* Push > Push 
    * Added paging to the scheduled push list
* Analytics > User Metrics > Inflow/Outflow
    * Added monthly inflow/outflow metrics

### April 11, 2023

#### Feature Updates
* Analytics > Sales Metrics > Paying Users
	* Improved visibility of user metrics and filters

### March 28, 2023

#### Added Features
* Purchase (IAP) > Payment Information
    * Added the feature to view and register payments with product information unregistered and register

#### Feature Updates
* Analytics > User Metrics > User Metrics
	* Added accumulative DAU metrics that were missing when downloading excel files

### March 14, 2023

#### Feature Updates
* Purchase (IAP) > Payment Information
    * Improved so that the status can be changed from 'Reserved' to 'Success' (Google Play Store excluded)

### February 28, 2023

#### Feature Updates
* Purchase (IAP) > Product
    * Improved so that the status that changes for each store item can also be applied to Gamebase products

#### Added Features
* Member > Member
    * Added a feature to search for members by Device Key

### February 14, 2023

#### Feature Updates
* Management > Alarm > Recipient List
	* Added support for SMS alarm receiving feature for IAM members

### January 10, 2023

#### Added Features
* App > App
    * Added the feature to set product support types (single product/multiple products)
* Push > Analytics
    * Added the feature to check the cause of message delivery failure
* Analytics
	* Added scroll feature to the country item of multiple select filter

#### Feature Updates
* Purchase (IAP) > Product
    * Made a change so that, only one Gamebase product can be registered per external store item for games that have set 'single product'

#### December 27, 2022

#### Feature Updates
* Analytics > User Indicators > Retention
    * Changed the maximum retention period from 90 days to 180 days

### November 29, 2022

#### Added Features
* Analytics > Sales Indicators > Items Sales Indicators
	* Added PU for each item on daily view
* Analytics > Sales Indicators > First Purchase
	* Added the first purchase period indicator

### November 15, 2022

#### Added Features
* Analytics > User Indicators > User Indicators
	* Added the monthly cumulative user item on daily view

### October 25, 2022

#### Added Features
* Analytics > Sales Indicators > Purchase Amount
	* Added the monthly cumulative payment amount on daily view

#### Feature Updates
* Analytics > User Indicators > Service Environment
	* Improved to display all devices when downloading data files if the search condition is set to device

### October 4, 2022

#### Added Features
* Analytics > Sales Indicators > Purchase Amount
	* Added the feature to see more stores
* Analytics > User Indicators > User Indicators
	* Added the average number of login item
* Analytics > Sales Indicators > Paying Users
	* Added the monthly cumulative PU item on daily view

#### Feature Updates
* Analytics
	* Improved the filter that allows multiple selection on menu items other than Transmission Indicator
	* Improved to organize sheets by country when downloading the excel file
* Analytics > Sales Indicators > Purchase Amount
	* Improved to fix the order to output stores

### September 14, 2022

#### Feature Updates
* App > App
	* Added multi channels (Japan/Thailand/Taiwan/Indonesia) feature for LINE IdP
* Purchase (IAP) > Store
	* Added ONE store API v6(SDK v19)
* Purchase(IAP) > Payment Information
	* Added country code and additional information item to Payment History

### Bug Fixes
* App > App
	* Fixed an issue where activation failed intermittently when activating 'Customer Center Provided by Gamebase' for the first time

### July 26, 2022

#### Feature Updates
* Purchase (IAP) > Payment Information
	* Added the payment date item to Payment History

#### Bug Fixes
* Purchas(IAP) > Payment Information
	*  Fixed an error in saving files when the payment history is very large
* Mobile Customer Center
	* Fixed a download error when there is a space in the attached file name

### June 14, 2022

#### Feature Updates
* Coupon > Publish
	* Added a feature to set the hour and minute in the coupon validity period

### May 24, 2022

#### Feature Updates
* Purchase (IAP) > Store
	* Added ONE store external payment

#### Bug Fixes
* App > Terms
	* Fixed an issue where unnecessary horizontal scroll is generated when the manually entered terms and conditions detailed page is displayed in Android WebView

### April 26, 2022

#### Feature Updates
* App > App
	* Added a feature to enter a content provider ID (CPID) in the NHN Cloud organization product (Online Contact) Customer Center
* App > Client
	* Made a change so that, when a newline character ('\n') is entered in the service status display message, newline processing is performed by the client SDK
* Member > Member
	* Added a feature to terminate the connection if the withdrawn user is currently logged in when performing the user withdrawal processing
* Purchase (IAP) > Store
	* Provided an additional service account integration method for Google Play Store

#### Bug Fixes
* Customer Center > FAQ
	* Fixed an issue where, when an error occurs due to input of special characters in the FAQ type management, an incorrect message is displayed as a duplicate category error

### April 12, 2022

#### Feature Updates
* Purchase (IAP) > Product
	* For Amazon Store, changed to register only one 'Gamebase Product' in 'Store Item ID'

### March 29, 2022

#### Feature Updates
* App > App
	* Added login using Google SDK on iOS

#### Bug Fixes
* App > Installation URL
	* Fixed an issue where, when saving data for the first time, an error occurred when saving data in different windows at the same time

### March 15, 2022

#### Bug Fixes
* Purchase (IAP) > Transactions
	* Fixed an error where the item name was displayed incorrectly when searching for a receipt
	* Fixed an error in saving data as an Excel file for query with a large amount of payment history
* Operation > Maintenance
	* Fixed an error where the maintenance content could not be modified when the maintenance page is an external page
* Push > Setting
	* Fixed an error where settings were not saved after modifying the settings

### February 22, 2022

#### Feature Updates
* Customer Center
	* Added Chinese (Traditional) and Russian to supported languages for customer center provided by Gamebase
* App > Client
	* Added a **Details** button to the Update Required pop-up window
* Purchase (IAP) > Store
	* Added Amazon Appstore and Huawei AppGallery store
* Push > Push
	* Added a message click action feature and custom fields
* Push > Settings
	* Added a feature to reserve a message for consent to receive push advertisements

#### Bug Fixes
* Purchase (IAP) > Product
	* Fixed an error where an error message does not appear in certain situations when product registration using a CSV file fails

### January 25, 2022

#### Feature Updates
* Operation > Kickout
	* Added a feature to select whether or not to expose a kickout popup

### January 11, 2022

#### Feature Updates
* Customer Center
	* Added a feature that enables users to select a display language among the supported languages in the Customer Center

### December 28, 2021

#### Bug Fixes
* Analytics > User Indicators > Inflow/Outflow
	* Fixed an error where the numbers of new/withdrawn users were displayed as twice the actual value

### December 14, 2021

#### Feature Updates
* App > App > Customer Center
	* Added a feature that allows users to access the Customer Center from a web browser and submit direct 1:1 inquiry
* Operation > Maintenance
	* Added a feature to select whether to display the maintenance time on the Maintenance page
* Customer Center > Template
	* Changed so that templates in languages that are no longer supported by the Customer Center among previously registered templates are not displayed and deleted.

#### Bug Fixes
* App > App > Customer Center
	* Fixed an error where Chinese was not displayed in the Customer Center even if Chinese was set as the supported language

### November 23, 2021

#### Feature Updates
* App > Client
	* Added description about server address and installation URL
* Ban > Ban
	* Improved the ban registration page

#### Bug Fixes
* Ban > Ban
	* Fixed an error where it was impossible to select the day/time of the period when the screen width was reduced

### November 9, 2021

#### Feature Updates
* App > Install URL
	* The domain of the ShortURL provided for the newly created Gamebase has been changed to https://nh.nu

#### Bug Fixes
* Operation > Maintenance
	* Fixed an error where the preview was displayed incorrectly when registering custom page HTML (Webview) maintenance

### October 26, 2021

#### Feature Updates
* Analytics
	* Changed the file download format to .csv and .xlsx
* Operation > Kickout
	* Added a feature to copy kickout details

### October 12, 2021

#### Feature Updates
* Push > Statistics > Send/Receive, Receive Settings
	* Changed the file download format to .csv
* Coupon > History, Coupon publish list
	* Changed the file download format to .csv

#### Bug Fixes
* Analytics > User Indicators > Life Cycle
	* Fixed a bug where the withdrawn user indicator is displayed as 0 on the day of registration

### September 28, 2021

#### Feature Updates
* Purchase (IAP) > Payment Abusing Monitoring
    * Added an automatic release feature for payment abusing
* Purchase (IAP) > Store
    * ONE store SDK v16 support

#### Bug Fixes
* Purchase (IAP) > Product
    * Fixed an error where, when registering items with a file, the items are registered incorrectly if there is multiple store information
* Push > Push
    * Fixed an error where message sending fails when setting the text color for messages that have only content without a subject

### September 14, 2021

#### Feature Updates
* Push > Statistics > Inbound settings
	* Changed the text related to inbound settings statistics
* Customer Center
	* Added a template feature where users can enter details depending on an inquiry type
	* Added an internationalization feature to the customer inquiry reply template
* Analytics
    * Added a feature to provide MCU/ACU data when a filter is selected
        * MCU data is provided from August 11
* Analytics > Real-Time Monitoring > Concurrent Users
	* Added a feature to provide push notifications scheduling history

#### Bug Fixes
* Customer Center > Customer Inquiry
	* Fixed an error where a wrong calendar is exposed when the user selects submission period dates

### August 24, 2021

#### Feature Updates
* Customer Center > Customer Inquiry
	* Added the inquiry history in the customer information at the bottom
* Purchase (IAP)
	* For the initial currency setting, a base currency is displayed according to the language selected in NHN Cloud

### August 10, 2021

#### Feature Updates
* Analytics > Real-time Monitoring > Real-time CCU
    * Fixed the graph of change in the number of concurrent connected users (CCU) to GMT+9 timezone
* Push > Push Registration
	* Added the feature of deleting all entered messages
* Customer Center > Customer Inquiry > Send Reply Settings
	* Improved to expose all languages selected in App > Language Settings > Supported Language, if Send Reply is not set up
* Coupon > Issue Coupon
	* Modified the notification message about the number of coupons issued

#### Bug Fixes
* App > Terms and Conditions
	* Fixed the error of some country names not being displayed in Preview Terms and Conditions
* Purchase (IAP) > Store
	* Fixed the error of no alarm message popping up even though this is a required value
* Members > Members
	* Changed the text in the popup window when looking for a non-existing member


### July 27, 2021

#### Feature Updates
* Customer Center > Customer Inquiry
	* Improved to leave multiple answers when handling customer inquiries.
	* Improved to allow users to leave additional inquiries after leaving an initial inquiry.
	* Added Status: To be used when needing additional confirmation after pending-replied.
* Purchase(IAP) > Payment Abusing Monitoring: Improved to display the user status in multiple languages in the payment history by user section.

#### Bug Fixes
* Coupon > Issue Coupon: Fixed an error where statistical data was omitted when sending coupon through SMS.

### July 13, 2021

#### More Features
* New mobile menu open: Retention

### June 29, 2021

#### Bug Fixes
* Analytics > Dashboard: Fixed a bug where the PU accumulation value of the downloaded Excel file included duplicate PUs

### May 25, 2021

#### Feature Updates
* Purchase(IAP) > Item: Added a feature of allowing users to confirm low-level ID information when changing status of store Item

### May 11, 2021

#### Feature Updates
* App > App: Improved the view so that users can group and edit apps by feature

### April 13, 2021

#### Feature Updates
* Member > Member: Improved system to display the opt in date if opt in for advertising and opt in for night-time advertising are both true when viewing the push token
* Purchase (IAP) > Payment Information: Updated strings on the pop-up window for additional information to be displayed with line breaks
* Purchase (IAP) > Payment Abusing Monitoring
	* Updated automatic restriction detection period which was fixed to one hour is now customizable by users (1 hour to 48 hours)
	* Updated automatic restriction condition input of numbers and prices that only allowed AND condition to also allow OR condition

### March 23, 2021

#### Feature Updates
* Member > Download: Increased the size of data stored in a file (from 50,000 to > 500,000)

### March 09, 2021

#### More Features
* App > Terms and Conditions: Added the GDPR terms

### February 23, 2021

#### More Features
* Operation > Kickout: Added a feature to enable kickout for each client version
* Purchase (IAP) > Store: Added a feature to set one-time receipt verification phase for Google Play store

### February 15, 2021

#### Bug Fixes
* Purchase (IAP) > Payment History: Fixed an error displaying wrong item name on file download

### February 9, 2021

#### More Features
* Common Terms and Conditions added
	* New menus available: App > Terms and Conditions, App > Deploy Terms and Conditions

#### Feature Updates
* App > Client: Improved the screen to classify and display the client version per store

### January 26, 2021

```
Push > Push (Old) Console menu feature has been removed.
```

#### More Features
* User Ban > AppGuard: Conditional Ban added
* Purchase (IAP) > Payment Abusing Monitoring: Apple App Store added

#### Feature Updates
* Manage > Permissions: Sales Access Permissions removed [See the notice](https://www.toast.com/kr/support/notice/detail/2101)

### January 12, 2021

#### More Features
* Added a new push menu
	* Statistics: Checks push statistics such as push sent, received, token registration, etc.
	* Event key: Manages the event key used in push send statistics
	* Certificate: Manages the certificate used to push send
	* Setting: Manages the values related to push

### December 29, 2020

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added

#### Feature Updates
* App > App: beta service server added to Manage Server Address
* App > Client: beta service added to Client Status, memo function added so that additional client information can be registered
* Purchase (IAP) > Product: search condition added - whether currently being used or not
* Purchase (IAP) > Payment Information: now store test payment is displayed in the payment history
* Purchase (IAP) > Terminate Sales Menu: Analytics > Sales Metrics and functions are integrated now.
* Analytics > Preferences > Terminate Installation URL Menu

### December 15, 2020

#### More Features
* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [Console]
	* Analytics: Improved to maintain the selected search condition even when the date is changed after filtered search
	* Push > Push: Tencent Push removed
	* Purchase (IAP) > Transaction information: Changed not to show the Check Receipt button in the refund state
* [SDK] 2.18.2
    * (Common) TOAST SDK update: [Android(0.24.2)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) External SDK update to resolve encryption logic security warnings: PAYCO Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView: Returns error when invalid URL is delivered, delivered URL is used as is without encoding
	* (iOS) Changed to run the custom scheme regardless of letter case
	* (Unity) The following fields of GamebaseRequest.GamebaseConfiguration class deprecated: zoneType, fcmSenderId

#### Bug Fixes
* [Console]
	* Purchase (IAP) > item: Fixed the issue where duplicate items are registered when a batch of items is added using a file
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### December 2, 2020

#### More Features
* [Console]
	* Newly added subdivided Gamebase permission functions: 24
	* Analytics > Group Metrics: new subscribers per project and payment amount comparison graph added
	* Members > Members: View Customer Center Inquiry History tab added at the bottom
	* Coupon > Issue Coupon: Issue Additional Coupons function added so that more coupons can be issued to an already issued coupon (100,000 at one time)

#### Feature Updates
* [Console]
	* Analytics > Live Monitoring: display color changes when there is a gain/loss in day-over-day metric data
		* Increase: blue->red, decrease: red->blue
	* Analytics > Sales Metrics > Payment Amount: Now sales data comparison can be viewed "per store" and "per IdP" in addition to the previous "per country"
	* Operation > announcement: setting to move to Customer Center added in the Read More link
	* Customer Center > Customer Inquiry: Auto Translation function added in the Send Reply settings
	* Coupon > Issue Coupon: increased the initial coupon issuance quantity from 50,000 to 1,000,000

#### Bug Fixes
* [Console]
    * Purchase (IAP) > Payment Information: fixed the issue where files cannot be downloaded when too much data is viewed

### November 10, 2020

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: [Android(0.24.1)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-android/#0240-20201027) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings
    * (iOS) Added API that supports the SceneDelegate of iOS 13 or higher

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### October 27, 2020

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
* [SDK] 2.17.1
    * (iOS) When sending a specific index, an error message is added to it: When push registration fails or when sending a game index
    * (Unity) Supports Unity 2017.2.5
* [SDK] 2.15.0
    * (Unreal) TOAST SDK update: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)  

#### Bug Fixes  
* [Console]
    * Analytics > User Indexes: Fixed an issue where the weekly and monthly average CCU were displayed abnormally with their logic modified.
    * Push > Push: Fixed an issue where null was displayed on the title when a non-black color was used without entering any text for the title.
	* Coupon > Issue Coupon: Fixed an issue where the coupon file could not be downloaded when there were more than 50,000 coupons issued.
* [SDK] 2.17.1
    * (Unity) Fixed an issue where the latter API would not work when the image notification API and web view API were called in turn.
* [SDK] 2.15.0    
    * (Unreal) Fixed an issue where the ProGuard declaration was missing in the transaction module

### October 13, 2020

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
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0

### September 22, 2020

#### More Features
* Added a feature to Customer Center
	* [Console] The Customer Center menu is now available: To handle customer queries and manage FAQ/notifications
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API

#### Feature Updates
* [Console]
	* Analytics menu (common): Changed the way filters are sorted for each country (index descending -> name ascending)    
    * Analytics > Sale index: Displays the total transaction amount as well as the transaction amount for each country for the country store in store-specific dashboards

### September 16, 2020

#### Feature Updates
* [SDK] 2.15.1
    * (iOS) TOAST SDK update: iOS(0.27.0)
	* A new version of IAP SDK is applied to support the changes made to iOS 14 beta. [TOAST SDK Release Notes](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-ios/#0270-20200911)

### September 15, 2020

#### More Features
* [SDK] 2.15.0
    * (JavaScript) Added GamebaseProductId to Purchase API of Hangame points

#### Bug Fixes    
* [Console]
    * Fixed Purchase (IAP) > Payment Information: Fixed an issue in which authentication by receipt did not properly show

### August 25, 2020

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
* [SDK] 2.9.1
    * (Unreal) Supports Unreal 4.22 ~ 4.25
    * (Unreal) Supports PLCrashReporter Issue: [Guide](http://docs.toast.com/en/Game/Gamebase/en/unreal-started/#ios-settings)

#### Feature Updates
* [Console]
    * Push > Push: Modified to allow sending without sender's contact information or method of unsubscription when notification is sent for promotional push
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) Added the null check logic for the payload of payment
* [SDK] 2.9.1
    * (Unreal) Updated Gamebase SDK version for iOS within iOS Plugin (2.9.1)
    * (Unreal) Fixed the missing part of UObject referencing

#### Bug Fixes
* [Console]
    * Push > Push: Fixed an issue in which time was identically applied with UTC+9, for delivering repetitive push notification, regardless of timezone

### August 19, 2020

#### Bug Fixes
* [Console]
    * The Entire Menus of Analytics: Fixed the unavailability of downloading excel files

### August 11, 2020

#### Feature Updates
* [Console]
    * Analytics > User Indicators > Retention: Show numbers, as well as %
* [SDK] 2.14.0
    * (iOS) Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings
    * (iOS, Unity) Adde the contentMode setting for TCGBWebViewConfiguration
* [Server]
    * Added error code for Coupon Expired API: When a coupon code includes a value other than English or numbers (Error Code:-4000205)

### July 28, 2020

#### More Features
* [Console]
    * Analytics: Added the WAU (Weekly Active User) and MAU (Monthly Active User) indicators
* [SDK] 2.13.0
    * (Unity) Standalone: Added Show Notice on Image API     

#### Feature Updates
* [Console]
    * App > App: Modified to enter further information to authenticate Sign In With Apple on iOS 12 or lower versions  
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image
    * (iOS) Authenticate Sign In With Apple: Supported for iOS 12 or lower

#### Bug Fixes
* [Console]
    * Operations > Notice on Image: Fixed the feature of copying, as well as error in which selected countries are not properly changed to all countries
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module

### July 14, 2020

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order
    * [Console] Operations > Image Notices: Menu added  
    * [SDK] 2.12.0: Added Show Image Notice API

#### Feature Updates
* [Console]
    * Purchase (IAP) > Products: Products can be queried by item number
    * Membership > Member: Updated to change the status of users who are suspended from withdrawal to normal
    * Membership > Download: Added deviceKey and IdP code to the history of login logs
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with storeCode(default=AS) set for configuration
    * (iOS) Fixed failed closing due to lack of the close button while printing webview which cannot load content
    * (Unity) Updated TOAST Unity SDK (0.20.1.1)

### June 23, 2020

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed

#### Feature Updates
* [Console]
	* Purchase (IAP) > Products: Updated to register and manage many Gamebase products for a store item ID  

### June 9, 2020

#### Feature Updates
* [Console]
	* Membership > Member: Additionally shows the status of withdrwal suspension (withdrawal suspended, cancelled, or immediately withdrawn) on the **Query Withdrawal History**
* [SDK] 2.10.1
	* (iOS) Updated to set device language if language code is not configured when user push setting is initialized

#### Bug Fixes
* [Console]
	* Coupons > Issue Coupons: Fixed the inavailability of downloading history of coupon statistics sent via SMS

* [SDK] 2.10.1
	* (Unity) Fixed failed login calls since ViewController is not configured at iOS Plugin
	* (JavaScript) Fixed errors that occur if StoreCode is not entered during initialization


### May 26, 2020

#### More Features
* [Console]
	* Coupons > Issue Coupons: Added features of delivery statistics and downloading history of coupon deliveries  
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems
		* Includes ServerPush and Observer, and checks promotional purchase or push events

#### Feature Updates
* [Console]
	* All: Updated button/tag UIs to suit for common design guides  
* [SDK] 2.10.0
	* (Unity) Updated CefWebview version with StandaloneWebviewAdapter: v2.0.4
		* Updated the logic of WebviewIndex validation  
		* Fixed infrequent error of NullReferenceException while Webview is created

### May 12, 2020

#### More Features
* [SDK] 2.9.0
	* (Unreal) Newly released SDK

#### Feature Updates
* [Console]
	* App > App: Updated to save TOAST accounts of the users who changed the grace period of withdrawal  
	* Member > Member: Fixed an issue in which data does not properly show when mapping history is queried
	* Purchase (IAP) > Store: Test, Old) OneStore is updated not to allow new registration  

#### Bug Fixes
* [SDK] 2.9.1
	* (Andoird) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error

### April 29, 2020

#### Bug Fixes
* [SDK] 2.9.1
	* (Unity) Fixed a error which occurs when client service status is changed after initialized on console
		* Versions at Issue: v2.8.0 or higher
		* Platforms at Issue: Standalone, WebGL, and Editor

### April 28, 2020

#### More Features
* Suspension of Membership Withdrawal
	* [SDK 2.9.0]
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  
	* [Console]
		* Apps > Apps: Added features allowing to set suspension period for withdrawal
#### Feature Updates
* [SDK 2.9.0]
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated PAYCO Login SDK: Android(v1.5.0), iOS(v1.4.0)
* [Console]
	* All Menus: Changed the design of buttons and tags on console
	* Operations > Maintenance, Operations > Notice, Push: Supports auto-translation in multiple languages
	* Members > Membership: Further shows suspension period expired, when querying members who are suspended for withdrawal

### April 14, 2020

#### Feature Updates
* [Console]
	* Analytics Common: Updated the TUI chart version, and applied to Frequency7 indicators
* [SDK] 2.8.1
	* (Common) Added internal indicators to check Analytics delivery results

#### Bug Fixes
* [Console]
	* Analytics Common: Fixed an issue in which the scroll is deviated from area for a long country name
	* Analytics > Real-time Monitoring: Fixed an issue in which indicator shows 0 when query is requested while saving data
* [SDK] 2.8.1
	* (Android) Modified codes that may cause crashes after process restarts
	* (JavaScript) Modified an issue in which credentialInfo login is unavailable with Hangame IdP

### March 24, 2020

#### More Features
* [Console]
	* Release of New Menus: Analytics > User Indicators > Frequency 7
		* Provides weekly visits and rate of DAU. You can find the level of immersion and loyalty for a game.
	* Coupon > Publish: Texting coupons is now available  
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices
	* (Unity) Updated CefWebview to v2.0.1 within StandaloneWebviewAdapter
		* When the PopupType is PASS_INFO, popup data can be delivered without a popup
 	* (Javascript) Supports Hangame channeling: Hangame IdP authentication and purchase with Hancoin


#### Feature Updates
* [Console]
	* App > Transfer Indicator Setting: Allows pre-registered meta filters only for transfer indicators
		* Meta filter indicators that go beyond limit are not displayed.: Level (5,000), World/Server/Channel (100), and Occupation/Class (100)
* [SDK] 2.8.0
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login

#### Bug Fixes
* [Console]
	* Sales Indicators > Purchase Amount
		* Modified the display of currency, which is currently fixed at Won (KRW) for chart tooltips, to show as configured on the app  
		* Fixed the issue of not showing February indicators for monthly search

### March 10, 2020

#### More Features

- [Console]
	- App  >  App: Set whether to include test payment or not, when analytics sales indicators are displayed   
    		- With 'Exclude Test Payment', the analytics sales indicators show all but test payment.
		- Purchase (IAP): Set currency code for payment indicators on the initial access to purchase (IAP) menu
	- Only the initial setting is available, and the Analytics sales indicators show in the configured currency code.
  	- Added the 'View on Desktop' feature on the mobile console (including TOAST app)

#### Feature Updates

- [Console]
  	- App  >  Installation URL: Additionally apply available scheme for input URL  
    		- Previously: Common ('http://', 'https://'), Android('market://')
    		- Now: iOS('itms://', 'itmss://', 'itms-apps://'), Android('intent://')
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

#### Bug Fixes

- [Console]
  	- Analytics: Fixed an issue in which the sales indicator is displayed as '0' when the currency code is in coins

### February 25, 2020

#### More Features
* [Console]
	* Coupon > Publish: Added the feature of allowing published coupons to be used only at certain stores.   

#### Feature Updates
* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called
* [Console]
	* App > App: Added the notification logic when a same client is re-registered after deleted
	* Purchase (IAP) > Item: Added the registration field to register subscription product (App Store - Shared secret,Google store - Domain authentication File Names)

#### Bug Fixes
* [Console]
	* Analytics > Real-time Monitoring > Real-time Indicators: Fixed empty or infinity issues that infrequently occur for ccu after push is sent
	* Analytics > Transfer Indicators
		* Fixed bugs that are not updated to No Data when data is gone from grid
		* Fixed the vertical display of buttons when the filter name is short

### February 11, 2020

#### More Features
* [Console]
	* Allows to see the flow of user indicators at a glance, from creating a new menu release project at Analytics > User Indicators > Life Cycle
	* Management > Authority: Added the role of receiving Weekly Report
		* The 'Weekly Report' is to be mailed from March.

#### Feature Updates
* [Server API] Added validation for the regUser length when Withdraw API is called
* [Console]
	* Analytics: Apply Japanese fonts for Grid, Chart
	* Purchase: Updated to let users intuitively view popup message display when error occurs

#### Bug Fixes
* [Console]
	* Analytics: Modified the currency display from 'Yen (JPY)' to 'Won (KRW)' when the language is changed to Japanese

### January 21, 2020

#### More Features
* [SDK] 2.7.0
	* (Unity) Supports NaverCafePLUG

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response
	* (Android) Modified not to occur exceptions when Firebase setting is missing
	* (Unity) Added the gamebase://dismiss scheme handling for a web login
	* (Unity) Modified infrequent failure in the display of webview for a release build 	
* [Console]
	* Analytics: Fixed failed re-direction to a login page when the user session is expired

### January 14, 2020

#### More Features
* [Server API] Added Withdraw Users API

#### Feature Updates
* [SDK] 2.6.3
	* (Unity) Updated Standalone Webview: Updated CefWebview 	
	* (Unity) Added .dll file missing from an error occured after login
		* ToastCommon.dll, vcruntime140.dll

#### Bug Fixes
* [SDK] 2.6.3
	* (Unity) Fixed error that occur when Login(CredentialInfo) API is called

### December 24, 2019

#### More Features
* Conpon > Publish: Added the feature of keyword coupons

#### Feature Updates
* [Console]
	* Purchase > Query Payment Information: Added the column for additional information
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) NAVER SDK Updates (4.1.0)

### December 10, 2019

#### More Features
* App > App: Allows to register devices for QA testing via IP as well  

#### Bug Fixes
* [Console]
  * Modified incorrect Japanese
* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize()
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address
  * (Android) Fixed crash occurrence when IAP is not enabled  
  * (iOS) Fixed the issue in which mapping is not available when AddMapping (Forcibly) is applied
  * (iOS) Fixed crash occurrence by NSNUll object, when displayLanguageCode of PushConfiguration is not set by Unity Plugin

### November 26, 2019

#### Bug Fixes
* [Console]
  * Coupons >Issuance: Fixed the abnormal downloading when downloading a coupon after session is expired  
  * Analytics > Real-time Monitoring > Dashboard: Fixed data of the previous date wrongly displayed as 0  
     Fixed the issue in which the page is not properly displayed for a disabled product, when accessing relevant menu of TOAST Product (e.g. IAP, Push, or AppGuard)

### November 20, 2019

#### Bug Fixes
* [SDK] 2.6.1
  * (Unity) Added the iOS Plugin file (toast_sdk_wrap.m) to Package in order to prevent errors for an iOS build  
  * (Unity) Fixed the error of UnityEditor in which the store code comes as empty on a platform other than Standalone, resulting in failed initialization  
  * (Unity) Fixed the error of NullReferenceException due to errors from processing zone type within Initialize API

### November 13, 2019

#### Bug Fixes
* GamebaseSettingTool
  * Fixed the error in which files are not properly updated, with the version updated to Gamebase v2.6.0


### November 12, 2019

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features
* Coupon Service Newly Open: Create and manage coupons in large quantity
  * [Console] Newly opened the Coupon Menu             
  * [Server API] Find coupons and add Consume API
* Auto anti-abusive payment practices  
  * [Console] Purchase (IAP) > Newly opened monitoring menu for abusive payment practices
    * Auto banning against abusive payment
    * Manual service suspension after searching conditions for abusive payment
* Added the payment feature for Google subscription  
  * [SDK] 2.6.0 Android
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis
  * (iOS) Added authentication for Sign In with Apple
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase.

#### Feature Updates
* [SDK] 2.6.0
  * (Unity) Fixed the error in the logic of updating LaunchingStatus for a login
  * (Unity) Fixed the error of endless repetition of log delivery from the client, if delivery of debug logs is set on the Gamebase console
* [Console]
  * App >App: Allowed to enter each service status (e.g. testing, under inspection, or in service) of a server address  
  * Purchase (IAP) > Payment Information: Changed UI to search by selecting search conditions

### October 29, 2019

#### Feature Updates
* [Console]
  * Analytics: Changed tooltips in the pie chart
  * Analytics > Real-time Monitoring: More targets for push delivery


### October 15, 2019

#### Feature Updates
* [SDK] 2.5.2
	* (iOS) Changed UIWebView into WKWebView
* Sample App
	* Updated Gamebase SDK (v2.4.0)
	* Applied Smart Downloader (v1.5.8) and Leaderboard
	* More Features: Downloading game resources, integrating Leaderboard and TAA indicators, transferring devices, and forced mapping
	* Updates: Added ServerPush listeners and detection of observer maintenance  
	* Renewed games

#### Bug Fixes
* [Console]
	* Management > Authority: Fixed failed modification of authority
	* For Mobile
		* Fixed the issue of keyword enabled by selecting Datepicker
		* Analytics: Fixed the issue of NRU value exposed for ARPPU

### September 24, 2019

#### Feature Updates
* [Console]
	* App >Client: Modified UI to select stores for the registration of web client  
#### Bug Fixes
* [Console]
	* Management > Authority: Fixed the failed modification of authority
	* For Mobile
		* Fixed the issue of keyword enabled by selecting Datepicker
		* Analytics: Fixed the issue of NRU value exposed for ARPPU

### September 10, 2019

#### More Features
* [Console]
	* Analytics: Further shows the level indicator for channel/world/server and occupation/class transfer indicators

#### Feature Updates
* [Console]
	* Analytics: Performance updated for grid rendering (with tui-grid 4.4x)
* [SDK] 2.5.1
	* (iOS) Updated to TCPushSDK 1.7.0 which is for GamebasePushAdapter
		* Since file has changed from static library to framework for TCPushSDK, TCPushSDK.framework must be added to project.

### August 27, 2019

#### More Features
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview

#### Feature Updates/Changes
* [Console]
	* Analytics: Updated chart performances

### August 2, 2019

#### Bug Fixes
* [SDK] Setting Tool 1.4.3
	* Fixed the build error by moving down the script file below the editor folder  
	* Fixed failed operations when Multilanguage is provided with the entire path of the language file on MAC OS.

### July 23, 2019

#### Added
* [Console]
	* New Menu Released on Member > Download: As of the last login time on the date of service joining, list of game users can be queried and downloaded in files   

#### Feature Updates
* [Console] Mobile
	* Can register and modify maintenance and push  
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)
* Setting Tool
	* Change of Folder Structure: Must reinstall after previous SettingTool is completely deleted  
	* More languages are supported

#### Bug Fixes
* [Console]
	* Analytics > User Indicators: Fixed the issue of date overlaps on the axis X

### July 11, 2019

#### Feature Updates/Changes
* [Console] Analytics
	* Improved in level-up query performance
	* Shows Min./Max. data within a chart
	* Supported in multiple languages (Chinese)

#### Bug Fixes
* [SDK] 2.4.3
	* (iOS) Fixed crash occurrence due to parsing attempts of error messages with conflicting formats, regarding authentication
	* (Unity) Fixed failed operations of AddMappingForcibly API, for the builds in iOS or Android
	* (Unity) Fixed the json parsing error at iOSPlugin when RequestRetryTransaction API is called

### July 1, 2019

#### Bug Fixes
* [Console]
	* Management > Alarms: Fixed the failure in modifying set alarm after Webhook is set up  

### June 27,2019

#### Bug Fixes
* [Console]
	* Service Suspension: Fixed failure in uploading files to register suspension of service in mass
* [SDK] Setting Tool 1.4.1
	* Fixed the error in uploading existing setting data when GamebaseSettingTool was executed

### June 25, 2019

#### Feature Added
* More Transfer Indicators
    * [Console]Analytics>Transfer Indicators: Menu available to check indicators per Level, Channel, or Class
		* Status in real time
		* Status by level
		* Status of the world, or each server or channel
		* Status of each class or profession
		* Level-ups
		* Sales status of items
		* Top 50 best-selling items

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

### June 13, 2019

#### Bug Fixes
* [SDK] 2.4.1
	* (iOS) Fixed the error in output of indicators due to missing of partial parameters during transfer of Analyticis indicators

### May 28, 2019

#### Feature Updates
* Purchase for HANGAME mix Available for Japan
    * [SDK] 2.4.0
      * (Unity) Added external purchase for Standalone Japan  
      * (Unity)  Added HANGAME authentication for Standalone Japan
    * [Console]
      * Purchase>Store: Added 'HANGAME mix (JAPAN)' Store
      * App>Client: Added store setting for client registration on Windows
      * App> Installation URL: Added store setting to add installation URL on Windows

#### Feature Updates/Changes
* [SDK] 2.4.0

  * (Common) Change of Classes Relevant to Indicators
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#level-up-trace)]

    * (Android) NAVER SDK Version Updated (v4.2.5): Bug of NAVER SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while NAVER login was underway)  
    * (Unity) StandaloneWebview supports 32bit Build (SDK volume upgraded from 53.6MB to 99.2MB)
* [Server]

    * Modified LTV queries and the failover logic
* [Console]

    * Support available for LTV Grid ComplexColumns and excel downloading

### 2019.05.16

#### 기능 추가
* [Console]
	* 단말기 이전 기능 추가(신규 메뉴)
		* 앱>기기 이전(Transfer account): 기기이전 기능 사용을 위한 설정값 저장
		* 회원>기기 이전: 발급된 키의 상태 및 이력 조회

#### 기능 개선/변경
* [Console]
	* 앱: AppleGameCenter, China IdP에 '토큰재검증' 옵션 Off
		* 해당 IdP에서는 실제 외부IdP 체크하지않고 내부토큰만 체크하므로 의미없는 옵션이므로 제거
	* 회원: 매핑 추가 가능한 조건 변경
		* 기존:Guest계정
		* 변경:Guest계정, Missing 계정

#### 버그수정
* [SDK] 2.3.1
	* (Android)2.3.0버전에서 Twitter 로그인 되지 않던 문제 수정
* [Console]
	* 회원: 구매 이력에서 영수증 검증이 되지 않던 문제 수정
	* Kickout: 조회 요청시 인증체크 추가하여 비정상 동작하던 이슈 수정

### 2019.04.23

```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

#### 기능 추가
* [SDK] 2.3.0
	* (Android/Unity)중국스토어 인증/결제 추가

#### 기능 개선/변경
* [SDK] 2.3.0
	* (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"
	* (Android)최근 로그인한 Provider로 로그인 및 웹소켓 응답 실패를 받았을 경우(Timeout, network disable 등) AuthToken을 삭제 처리하지 않도록 수정
	* (Android)IdP로그인 시 AuthAdapter 내부에서 발생하는 MemoryLeak을 수정

### 2019.04.11

#### 기능 개선/변경
* [SDK] 2.2.2
	* (Unity)SDK 로그 개선
* [Console]
	* Analytics 메뉴 다국어 적용
	* 보안검수 관련 취약점 패치

#### 버그수정
* [SDK] 2.2.2
	* (Android)Gamebase 초기화 이전 TransferAccount API 호출시, 콜백이 오지 않는 이슈를 수정
	* (iOS)showBlockingPopup을 NO로 설정 할 경우 Gamebase 초기화 콜백이 호출되지 않는 이슈를 수정
	* (Unity)AddMappingForcibly API를 호출하면 크래쉬가 발생하여 수정

### 2019.04.02

#### 버그수정
* [SDK] 2.2.1
	* (Unity) Unity Editor에서 Android 플랫폼을 선택하고 플레이를 하면 initialize시 서버에서 에러가 발생하는 이슈 수정

### 2019.03.26

#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    - (SDK공통)추가된 API
		* TransferAccountInfo 발급 API (issueTransferAccount)
		* 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
		* 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
		* 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)		
	- (Server API)
		* 발급된 TransferAccount의 ID/PW 검증하는 서버 API (validateTransferAccount)
    - (console)회원메뉴의 매핑이력조회 탭에서 Transfer 이력 확인이 가능
* 강제매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
	- (SDK공통)추가된 API
		* 강제매핑하는 API (addMappingForcibly)

#### 기능 개선/변경
* [SDK] 2.2.0
	* (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트
	* (iOS)LINE SDK의 App 로그인 기능이 비활성화
		* LINE SDK v4의 버그로 인해 iOS 12에서 앱 로그인이 실패 하는 이슈가 있어 Gamebase Line Adapter에서 Web 로그인만 지원하도록 변경
	* (Unity)GamebaseMainActivity의 Package Name이 변경
		* com.toast.gamebase.activity.GamebaseMainActivity -> com.toast.android.gamebase.activity.GamebaseMainActivity

### 2019.02.26

#### 기능 개선/변경
* [SDK] 2.1.0
	* (공통)TransferKey API 삭제
		* issueTransferKey : TransferKey 발급
		* requestTransfer : TransferKey 검증

#### 버그수정
* [SDK] 2.1.0
	* (Android)Gamebase 초기화 이전, onActivityResult() 가 호출되면서 이상 동작하던 버그 수정
	* (iOS)Gamecenter를 Gamebase가 아닌 다른 로직에의해 로그인 한 후, Gamebase를 통하여 Gamecenter로그인을 시도할 때, 반응이 없는 버그 수정

### 2019.01.29

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* Console
	* Analytics : Gamebase 2.0 지표 신규 오픈
	* 앱 : 클라이언트의 디버그 로그를 실시간으로 변경할 수 있는 기능 추가
* [SDK] 2.0.0
	* (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
		* setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
		* traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출
    * (JavaScript)SDK 신규 배포

#### 기능 개선/변경
* [SDK] 2.0.0
	* (Android)Push SDK 업데이트(android:1.7.0)
	* (Android)Adapter API 변경
		* Launching 정보 전달
		* logout, withdraw API에 Callback 추가
	* (iOS)IAP SDK 업데이트
		* 결제 실패 시 간헐적으로 크래시가 발생하던 현상 수정

#### 버그수정
* [SDK] 2.0.0
	* (iOS)iOS 12 이상의 시뮬레이터에서 debugMode On 상태로 Gamebase 초기화 시 크래시가 발생하던 현상 수정

### 2018.12.27

#### 기능 추가
* Console
	* Push - 반복발송 기능 추가

#### 기능 개선/변경
* [SDK] 1.14.5
	* deprecated 되었던 다음 API가 제거되었습니다.
		* (void)Gamebase.WebView.showWebBrowser(Activity, String)
		* (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
		* (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
		* (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
	* 결제 모듈(gamebase-adapter-purchase-iap) 수정되었습니다.
		* IAP SDK를 1.5.2로 업데이트
		* Client에서는 사용되지 않는 IAP TEST Store 제거
		* 결제 재처리 로직(requestRetryTransaction)에서 데이터가 불완전할 때 호출이 실패하는 문제를 수정
		* 크래쉬를 방지하기 위해 모든 IAP SDK 호출부에 예외 처리
* Console
	* 인증이 풀렸을 때 Rest API 요청에도 로그인페이지로 이동하도록 수정
	* IAP Transaction 조회 필터 추가

### 2018.11.15

#### 기능 추가
* Console
	* 중국어 적용
	* 회원 : 구매이력에 App Store 영수증 검증 기능 추가

#### 기능 개선/변경
* Console
	* 달력 다국어 지원 추가
* [SDK] 1.14.2
	* (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정
	* (iOS)Provider Profile 획득 메서드 호출 시, 반환하는 TCGBAuthProviderProfile 객체의 description 메서드의 JSON 문자열 구조 변경으로 인하여 Gamebase iOS SDK 1.14.0와 Unity Plugin 1.14.0 적용시 crash가 발생될 수 있는 구조 수정

#### 버그수정
* Console
	* 푸시 : 특정대상 발송 이후 등록된 푸시건을 복사하여 등록할 때 등록 실패하던 문제 수정
* [SDK] 1.14.2
	* (Android)에뮬레이터 환경에서 스토어앱(PlayStore, OneStore 등)이 없는 상태에서 "앱 설치/업데이트"시 스토어 미체크로 인한 crash 버그를 수정
	* (Unity)ShowWebView API 호출시 파라메타에 Callback을 넣지 않으면 crash가 발생되는 부분 수정
	* (Unity)iOS SDK의 Deleted API를 호출하는 코드가 있어 컴파일시 오류가 발생 되는 버그 수정

### 2018.10.23

#### 기능 추가
* Console
	* IAP : 결제 정보메뉴에서 App Store 영수증 검증 기능 추가
* [SDK] 1.14.0
	* (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.

#### 기능 개선/변경
* Console
	* IAP : 결제 정보메뉴에서 결제내역 다운로드 검색 조건 개선(1일 ->30일)
* [SDK] 1.14.0
	* (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
	* (iOS)PAYCO SDK의 버전이 1.2.4로 업데이트
	* (Unity)GamebaseSDKSetting 오브젝트가 있는 씬으로 돌아갈 경우 오브젝트가 중복으로 생기지 않도록 개선
	* Remove API : Webview, Network, Launching
		* (Android)5개
			- (void)Gamebase.WebView.showWebBrowser(Activity, String)
			- (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
			- (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
			- (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
		* (iOS)9개
			- [TCGBUtil showToastWithMessage:duration:]
			- [TCGBWebView showWebBrowserWithURL:viewController:]
			- [TCGBWebView showWebViewWithURL:viewController:configuration:]
			- [TCGBLaunching addObserverOnChangedStatusNotification:]
			- [TCGBLaunching removeObserverOnChangedStatusNotification:]
			- [TCGBLaunching addUpdateStatusNotification]
			- [TCGBLaunching removeUpdateStatusNotification]
			- [TCGBNetwork addObserverOnChangedNetworkStatusWithHandler:]
			- [TCGBNetwork removeObserverOnChangedNetworkStatusWithHandler:]
		* (Unity)7개
			- ShowWebBrowser(string url)
			- ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
			- ShowToast(string message, int duration)
			- AddUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
			- RemoveUpdateStatusListener(GamebaseCallback.DataDelegate<GamebaseResponse.Launching.LaunchingStatus> callback)
			- AddOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)
			- RemoveOnChangedStatusListener(GamebaseCallback.DataDelegate<GamebaseNetworkType> callback)

	* Deprecated  API
		* (Android)2개
			- (void)Gamebase.WebView.showWebView(Activity, String)
			- (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
		* (iOS)1개
			- [TCGBGamebase languageCode]
		* (Unity)1개
			- GetLanguageCode()
* [SDK] Setting Tool		
	* 팝업 및 UI 개선

#### 버그수정
* [SDK] 1.14.1
	* (Android)Auth API 호출 후 콜백에서 다시 Auth API 중복 호출시 정상 호출이 되지 않는 버그 수정

### 2018.10.11

#### 버그수정
* Console
	* 이용정지 : 대량등록시 발생하던 오류 수정

### 2018.09.20

#### 버그수정
* Console
	* 관리 : 페이지 주소 오류로 인한 알람페이지 처리 실패 발생 수정

### 2018.09.13

#### 기능 추가
* Console
	* 회원: 계정의 IdP 추가 및 삭제 기능 추가, IdP ID 검색 기능 추가
	* 푸시: 푸시상태별로 발송이력 조회하는 기능 추가
* [SDK] 1.13.0
	* (iOS)App Store Promotion IAP를 지원하기 위한 API 추가


#### 기능 개선/변경
* [SDK] 1.13.0
	* (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
	* (Android)Push API 호출 시, Gamebase 초기화/로그인 상태에 따라 호출 실패에 대한 에러 메시지를 보다 명확하게 개선
		* 초기화 전 호출 : NOT_INITIALIZED(1)
		* 초기화 이후 호출시 Push 모듈이 없음 : NOT_SUPPORTED(10)
		* 초기화 성공 및 로그인 이전 호출 : NOT_LOGGED_IN(2)		
	* (iOS)authProviderProfileWithIDPCode api의 호출 결과의 구조가 1depth로 변경 (Android, Unity와 통일)
	* (Unity)로그에서 보여주는 json 데이터를 알아보기 쉽도록 출력 포맷 개선
* Console
	* 이용정지 : 앱가드를 이용한 이용정지 등록하는 UI 개선 - 기능 off시 데이터 초기화, Leaderboard 데이터 삭제 설정을 상태가 'on'인 경우에만 노출하도록 개선

#### 버그수정
* [SDK] 1.13.0
	* (Android)NaverCafe SDK와의 충돌로 NAVER 로그인 시 발생하던 오류 해결
	* (Unity)Unity 2017.2 이상 버전에서 Editor Play Mode 종료 시 websocket close 처리에서 발생하던 오류 수정
* Console
	* App : 정보 수정시 삭제버튼 뒤의 내용이 잘리는 현상 수정

### 2018.08.28

#### 기능 추가
* Console
	* 회원: 계정상태 변경 기능 추가, Push Token 조회 추가
	* 운영지표(유저통계) : 오늘 탈퇴자, 당일 가입 후 탈퇴자 지표 추가

#### 기능 개선/변경
* [SDK] 1.12.2
	* (Android)WebSocket 타입아웃시 (API 호출 시간 경과), 크래시가 날 수 있는 버그에 대해 방어로직 처리
	* (iOS)Google Auth Adapter, Naver Auth Adapter의 Callback URL Scheme 설정 개선
		* 콘솔에 "url_scheme_ios_only" 값을 설정하지 않으면 Default URL Scheme을 설정 하도록 개선 : Default URL Scheme을 사용하기 위해서는 XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.google 또는 tcgb.{Bundle ID}.naver 등록 필요
	* (iOS)PAYCO Auth Adapter 개선
		* URL Scheme 미설정으로 인해 의도치 않은 URL Scheme을 호출하던 문제 수정 : 설정 방법이 변경되어 업데이트를 위해서는 반드시 URL Scheme 설정 필요 (XCode > Target > Info > URL Types에 tcgb.{Bundle ID}.payco를 등록)
* Console
	* 회원 : 아이디 매핑 이력 조회 기능 추가(최근 3개월 조회 -> 조회기간 직접 설정하도록 변경)
	* 구매(IAP) : 결제정보 엑셀 다운로드 1일로 제한, 아이템 삭제 기능 삭제

#### 버그수정
* [SDK] 1.12.2
	* (Android)auth-twitter-adapter 를 포함한 상태에서 TargetSdk 28로 빌드시 초기화 에러가 발생하는 문제 수정

### 2018.08.09

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
	* 에러발생시 팝업 알림 추가 : File Download 실패, File Extract 실패, XML 파싱 실패

#### 버그수정
* [SDK] 1.12.1
	* (iOS)NAVER 로그인 시 프로필 정보 조회 실패로 인해 로그인이 불가능한 버그 수정 : 프로필 정보 조회 실패하더라도 로그인은 성공하도록 변경
* Console
	* 결제 내역: 'Reserved'상태에서 결제 상태 변경이 되지 않는 버그와 엑셀 다운로드 시 필터링이 적용되지 않던 문제 수정

### 2018.07.24

#### 기능 개선/변경
* [SDK] 1.12.0
	* (iOS)Gamebase 초기화 시 Debug Log에 사용중인 Adapter들의 버전 정보, 앱의 빌드 정보를 출력하는 기능이 추가
	* (iOS)CocoaPods을 통해 배포 되는 Naver Auth Adapter에서 포함하고 있던 NAVER ID Login SDK의 바이너리가 제거 되고 의존성 설정 방식으로 변경
* Console
	* Web 클라이언트 등록일 경우 선택할 수 있는 서비스상태에 대한 제한 적용 : 업데이트권한, 업데이트필수 선택 불가능
* [SDK] Setting Tool
	* Setting Tool 최신 버전이 있을 경우 업데이트 알림 기능 추가
	* 내부 null Exception 수정

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



### 2018.07.05

#### 기능 추가
* LINE IdP 추가 : iOS

#### 기능 개선/변경
* [SDK] 1.11.1
	* (공통)Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경

#### 버그수정
* [SDK] 1.11.1
	* (공통)점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정
	* (Android)Gamebase.addObserver()를 통해 ObserverMessage를 수신하였을 경우, ObserverMessage.data.code의 타입이 int가 아니라 String인 버그를 수정
* Console
	* Windows client 등록 시 스토어코드가 잘못 등록되던 문제 수정

### 2018.06.26

#### 기능 추가
* iOS Google IdP 추가 : iOS
* Twitter IdP 추가 : Android, iOS
* LINE IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
* Server API 추가
	* getSimpleLaunching : 클라이언트 앱 기동시 제공되는 Launching 정보 확인용 API

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

### 2018.06.11

#### 버그수정
* [SDK] 1.10.1
	* (Unity)Unity Adapter가 없는 경우 AddMapping API 호출 시 내부적으로 로그인으로 처리하던 버그 수정

### 2018.06.07

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

### 2018.05.29

#### 기능 추가
* [Console] 지표(Operating indicator) 다운로드 기능 추가
	* 모니터링 > '동접변화'
	* 유저통계 > '일별지표변화'
	* 그룹동접 > '일간그룹동접변화'


#### 버그수정
* [SDK] 1.9.1
	* (iOS) Gamebase WebView NavigationBar 영역에 타이틀, 뒤로가기, 닫기 버튼이 나타나지 않는 현상을 수정

### 2018.05.18

#### 기능 개선/변경
* [SDK] 1.9.0
	* Unity SDK(1.9.0) Google Adapter 신규버전(1.6.2)으로 교체하여 재배포
    	- 5/3 배포된 Unity SDK(1.9.0)에 적용된 Google Adapter를 최신버전으로 교체(1.6.1->1.6.2)

### 2018.05.03

#### 기능 추가
* Transfer 기능 추가
    - guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    - (SDK공통)추가된 API
		* Transfer Key 발급 API (IssueTransferKey)
		* 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)
    - (console)회원메뉴의 매핑이력조회 탭에서 Transfer 이력 확인이 가능
* 이용정지 등록시 사용자의 리더보드(랭킹) 데이터를 삭제할 수 있는 옵션 추가(TOAST Leaderboard를 사용하는 경우에 한함)
    - 이용정지 등록 메뉴를 이용하거나 App Guard 연동 페이지에서 사용 가능

#### 버그 수정
* [SDK] 1.9.0
	* (iOS) NAVER 계정을 이용한 로그인 중 App to Web 로그인 시도 시, 서버로부터 받아온 Scheme의 형식이 변경되어, 로그인이 되지 않는 현상 수정
    * (iOS) Adapter로부터 UnderlyingError 객체를 받아서 유저에게 전달되는 에러객체를 생성하는 로직에서 메시지 및 Underlying Error의 설정이 되지 않는 버그 수정
    * (Android) Heartbeat 에서 잘못된 사용자로 판정되는 경우 이용정지 팝업이 뜨지 않도록 수정(iOS 와 동일한 로직으로 수정)

### 2018.04.12

#### 버그 수정
* [SDK] 1.8.1
	* (Android. iOS)registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 2018.04.09

#### 버그 수정
* [SDK] 1.8.1
	* (Unity)UnityAndroid 플랫폼에서 아래 기능 사용 시 모듈 초기화가 되지 않아 NullReferenceException이 발생하여 수정
		* Launching, Purchase, Push, Util, Webview

### 2018.04.05

#### 기능 추가
* Kick out 기능 추가
    - 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    - (console)메뉴 추가
    - (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* 점검 웹페이지를 사용자가 Console에서 입력한 HTML 페이지로 사용할 수 있도록 기능을 개선
    - 이전에는 Gamebase에서 제공하는 웹페이지나 외부 웹페이지 연결만 가능했음
    - 웹서버가 없는 경우에도 점검페이지를 사용자가 원하는 형태로 만들 수 있음
* Observer 기능 개발 및 API 추가
    - (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
	* (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)
	* (iOS)페이코 간편로그인 3rd SDK v1.2.2 적용 : 로그인 성공 시 토큰 만료 정보(expires_in) 제공, iPhoneX 로그인 UI 개선
	* (iOS)iPhoneX 지원을 위하여, Webview 사용 인터페이스 수정

#### 버그 수정
* 국가코드(contry code)가 10자 이상인 경우 동접 데이터가 저장되지 않는 현상 수정
* [SDK] 1.8.0
	* (Setting Tool)Unity Facebook Adapter를 체크하면 에러가 나는 버그 수정

### 2018.03.13

#### 버그 수정
* [SDK] 1.7.1
	* (Unity)Inspector에서 설정된 SetDebugMode 값이 반영 안 되던 버그 수정
	* (Unity)Standalone, WebGL: Display Language에서 사용되는 리소스 파일 누락 부분 수정
	* (Unity)Google Adapter 1.6.2 배포: Google Adapter 1.6.1에서 AuthCode가 Empty로 반환되어 인증 실패하는 버그 수정

### 2018.02.22

#### 기능 추가
* [SDK] 1.7.0
	* NAVER IdP 인증 추가
	* Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 2018.01.25

#### 기능 추가

* [Console]
	* [Push] PUSH 입력값 복사기능 추가
	* [Operating indicator>그룹 동접] 일간 그룹 동접 변화 그래프 추가

* [SDK] 1.6.0
	* (Unity)Standalone WinSDK 추가
		* 64비트 지원
		* 인증 지원 : facebook, google, payco

#### 기능 개선/변경
* [Console]
	* [Operating indicator>모니터링] 프로젝트 생성 이전 설정된 시스템 점검 항목이 노출 되는 문제 수정
	* [App>앱] 테스트 단말기 등록화면 개선 - User ID 로그인이력을 바탕으로 손쉽게 단말기 등록가능하도록 개선
	* [Operation>점검] 점검 미리보기 화면 개선

#### 버그 수정
* [SDK] 1.6.0
	* (iOS)WebView 호출시, 크래시가 일어날 수 있는 부분에 대한 방어로직 처리


### 2017.12.21

#### 기능 추가

* [Console]
	* [Push] 현지시간대 발송(Local Time Push) 기능 추가
	* [Operating indicator>판매 현황] 마켓별 매출 차트 추가
	* [Operating indicator>유저 통계] 앱 출시 이후의 사용자 지표 추이 확인하는 메뉴 추가
	* [Operation>점검] 점검상태에서 사용자에게 보여주는 점검페이지 등록 방법 추가
		* 기존 : Gamebase 자체 제공 페이지, 외부 페이지 URL 입력
		* 추가 : Console에서 입력한 점검내용을 외부페이지에 전달하는 기능 추가

* [SDK] 1.5.0
	* WebView가 닫힐 때 발생하는 Close Callback 추가
	* WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가
	* Unity Setting Tool 신규 배포

#### 기능 개선/변경
* [Console]
	* [App>클라이언트] 클라이언트 상태 변경시 이전에 게임에서 등록한 사용자 노출메시지 정보를 재사용할 수 있도록 수정

#### 버그 수정
* [SDK] 1.5.0
	* (Unity)UnityEditor에서 Guest로그인이 되지 않는 현상 수정
	* (Unity)TOAST Console에 Facebook 인증 정보를 등록하지 않고 Gamebase.Login("facebook") API를 호출할 경우, KeyNotFoundException이 발생하여 방어코드 추가


### 2017.11.30

#### 기능 추가

* [Console]
	* [Management>알람] Webhook 알람 등록하는 기능 추가
	* [Operating indicator>모니터링] Push 발송 이력 조회 추가

#### 기능 개선/변경
* [Console]
	* [Operating indicator>모니터링] 차트 색상 변경, Timezone 이슈. DAU 계산로직 변경(Login시간기준->접속시간기준)
* [API] [점검 조회 API](./api-guide/#check-under-maintenance) 결과를 List 에서 단일 객체로 변경

#### 버그 수정
* [Console]
	* [Push] Push 등록 시 기본언어가 선택되어 있지 않아도 등록되는 오류 수정


### 2017.11.23

#### 기능 추가

* [SDK] 1.4.0 업데이트
	* (Unity)Gamebase Facebook Adapter가 추가 : Android, iOS, WebGL, Standalone Platform 및 UnityEditor 지원

#### 기능 개선/변경
* [SDK] 1.4.0 업데이트
	* (iOS)close/back 버튼 리소스가 없을 때, "x", "<" 등의 텍스트로 노출되던 이슈를 디폴트 값으로 대체

#### 버그 수정
* [SDK] 1.4.0 업데이트
	* (Android)Gamebase 제공 팝업을 사용하지 않는 경우 이용정지 정보가 null로 리턴되는 오류 수정
	* (iOS)WebView 런치 후, 기기 회전시 NavigationBar Title 이 reset이 되는 오류 수정
	* (iOS)WebView의 NavigationBar Height을 커스터마이징 할 때, NavigationBar 배경 부분이 겹쳐서 노출되는 오류 수정

### 2017.10.26

#### 기능 추가

* [SDK] 1.3.0 업데이트
	* Credential을 이용한 AddMapping API추가

#### 기능 개선/변경
* [Console]
	* TC Push 에러코드에 따른 메시지 처리 작업 적용
	* 이용 정지 템플릿 메시지 등록 화면을 Input Textbox 에서 TextArea로 변경
	* TC 신규 권한 추가에 따라 관리메뉴가 정상적으로 보이지 않던 문제 수정
* [SDK] 1.3.0 업데이트
	* (Unity)CredentialInfo를 사용하는 Login API호출 시 iOSPlugin에서 Json 파싱이 안되던 버그를 수정

### 2017.09.21

#### 기능 추가

* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
	* 이용정지 사용자 팝업 노출
* [Console]
	* 고객센터(email, 전화번호) 등록
	* Ban 메뉴 오픈
	* Member메뉴 : 사용자 결제이력 조회 기능 추가


#### 기능 개선/변경

* [Console]
	* 각 메뉴에서 사용자 국가기준으로 시간 노출
	* 판매현황 소수점 이하 가격처리
	* 동접변화량 알람 발송 다국어 지원(영어/한국어 선택 가능)

### 2017.08.24

#### 기능 개선/변경

* [SDK] 1.1.6 업데이트
	* Push API 추가(for iOS) : SetSandboxMode

### 2017.07.20

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.5 업데이트
	* 시스템 팝업 API 추가 (showAlertWithTitle)
	* 국가코드를 대문자로 반환하도록 변경 (Android)
	* TCPush SDK 1.4.1 로 업데이트
	* IAP SDK 1.3.3.20170627 로 업데이트
* [Console]
	* 외부 연동 모듈에서 오류 발생시, 추적을 위한 TrackingTime 추가 노출

### 2017.05.25

#### 기능 개선/변경

* Gamebase 상품 이용 중지시 관련 데이터 삭제를 위한 일 배치 기능 추가
* [SDK] 1.1.4 업데이트
	* 런타임 중 결제 Store를 변경할 수 있는 API 제공
	* (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공
* [Console]
	* 다국어 지원
	* 모든 메뉴의 Create, Update, Modify 수행시에 Audit log 기능 추가

### 2017.04.20

#### 기능 개선/변경

* [SDK] 1.1.3 업데이트
	* (Android)런칭 구조 및 팝업/점검 페이지 개선 :커스텀 점검 페이지 설정 기능 추가
	* (Android)인증 구조 개선 및 로그 추가 : 인증 Adapter 및 SDK 버전 로그 출력

#### 버그 수정
* [SDK] 1.1.3 업데이트
	* (Android)Facebook SDK v4.19.0 이상에서 초기화시 크래시 오류 수정


### 2017.04.04

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임런칭시 점검, 긴급공지 팝업 개선
    * Unity Plugin 디버그로그 추가 및 익셉션 상세처리
* [API] [IAP](./api-guide/#purchaseiap) API 연동 : 아이템 조회, 미소비내역 조회
* [API] checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가
* [Console] 점검, 긴급공지 : 클라이언트 버전 선택 시 게임에서 사용하지 않는 스토어는 노출되지 않도록 변경

#### 버그 수정
* [Console] iOS 클라이언트 등록 시 마켓 비정상 노출 수정

### 2017.03.21

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog
* [API] [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API 연동

### 2017.03.09

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
