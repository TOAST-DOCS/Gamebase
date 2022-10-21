## Game > Gamebase > Release Notes > Unity

### 2.44.0 (2022. 10. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 外部SDK升级 : NHN Cloud Unity SDK(0.26.2)

#### 各平台更改项目
* [Gamebase Android SDK 2.44.0](./release-notes-android/#2440-2022-10-11)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Unity SDK(0.26.1), Kakaogame Unity SDK(3.14.5)
* 修改后当登录LINE时可输入提供服务的Region。
    * [Game > Gamebase > Unity SDK使用指南 > 认证 > Login with IdP](./unity-authentication/#login-with-idp)

#### 各平台更改项目
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.0](./release-notes-ios/#2430-2022-09-07)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unity.zip)

#### 添加功能
* 在ForcingMappingTicket类中添加了显示映射用户状态的mappedUserValid字段。

#### 改善/修改功能
* 已不使用用于设置是否在WebView中使用固定字体大小的字段。
    * **GamebaseWebViewConfiguration.enableFixedFontSize**
* 添加了GamebaseWebViewConfiguration的默认值。
    * 导航栏的颜色字段colorR、colorG、colorB、colorA的默认值被设置为18、93、230、255。
    * 指定导航栏是否是启用状态的“isNavigationBarVisible”字段的默认值已设置为true。
    * 指定是否启用Webview内的返回按钮的“isBackButtonVisible”字段的默认值已设置为true。

#### 各平台变更项目
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.6)
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**IDP_REVOKED**类型。
    * [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unity-etc/#idp-revoked)

#### 改善/修改功能
* 修复了使用Unity的Burst Package时导致内存泄漏的问题。
* Setting Tool (v2.4.0)
    * 添加了内部稳定指标。
    * 必须从Unity项目中完全删除现有的SettingTool，并使用最新版本重新安装。
    * 目前不支持SettingTool v1。

#### 修改程序错误
* (iOS) 修复了在某些情况下导致付款后崩溃的问题。

#### 各平台变更项目
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.5)
* 更改后支持(Standalone)以下条款API。
    * Gamebase.Terms.QueryTerms
    * Gamebase.Terms.UpdateTerms

#### 改善/修改功能
* 改善后韩语不再显示为统一码。
* 修改后支持(iOS) bitcode。

#### 修改程序错误
* 修复了调用(Android) OpenContact API时未能应用Configuration.additionalParameters的问题。

#### 各平台更改项目
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.4)

#### 修改程序错误
* 修改后，即使初始化前调用GetLaunchingInformations()也不出现JsonException。

#### 各平台更改项目
* [Gamebase Android SDK 2.39.0](./release-notes-android/#2390-2022-05-10)
* [Gamebase iOS SDK 2.39.0](./release-notes-ios/#2390-2022-05-10)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.3)

#### 改善/修改功能
* 更改了Display Language中汉语繁体(zh-TW)语言集中的错误句子。

#### 修改程序错误
* 修改后在低于(Android) API Level 24上调用指定API时不出现错误。
    * Gamebase.Purchase.RequestActivatedPurchases()
    * Gamebase.Purchase.RequestItemListOfNotConsumed()

#### 各平台更改项目
* [Gamebase Android SDK 2.38.0](./release-notes-android/#2380-2022-05-03)
* [Gamebase iOS SDK 2.38.0](./release-notes-ios/#2380-2022-05-03)

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 为了在客户服务URL后边添加参数，添加了以下字段。
    * GamebaseRequest.Contact.Configuration.additionalParameters

#### 各平台更改项目
* [Gamebase Android SDK 2.37.0](./release-notes-android/#2370-2022-04-26)
* [Gamebase iOS SDK 2.37.0](./release-notes-ios/#2370-2022-04-26)

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.2)
* 添加了付款时确认是否是Promotion付款的isPromotion字段。 
    * GamebaseResponse.Purchase.PurchasableReceipt.isPromotion。
* 添加了付款时确认是否是测试付款的isTestPurchase字段。
    * GamebaseResponse.Purchase.PurchasableReceipt.isTestPurchase

#### 修改程序错误
* 修复了设备设置为指定文化圈时，将支付商品价格信息输入为0的错误。
* 修复了点击(iOS) Push通知时未能启动DeepLink的错误。
* 修复了(iOS)项目的orientation设置为Auto Rotation，在包含于项目第一scene的MonoBehaviour的Awake中调用Gamebase。 API时未能输出Webview等的UI的错误。

#### 各平台更改项目
* [Gamebase Android SDK 2.36.0](./release-notes-android/#2360-2022-04-12)
* [Gamebase iOS SDK 2.36.0](./release-notes-ios/#2360-2022-04-12)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 外部SDK升级 : TOAST Unity SDK(0.25.1)
* 添加了可确认是否显示条款的API。
    * Gamebase.Terms.IsShowingTermsView()
* 添加了可在Webview隐藏导航栏的选项。
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.isNavigationBarVisible
* 添加了可在(Android)Webview固定字形大小的选项。
    * GamebaseRequest.Webview.GamebaseWebViewConfiguration.enableFixedFontSize
* 添加了可在(Android)条款窗固定文字大小的选项。
    * GamebaseRequest.Terms.GamebaseTermsConfiguration.enableFixedFontSize
* Setting Tool
    * 添加了(Android) Amazon商店。
    * 添加了(Android) Huawei商店。

#### 各平台更改项目
* [Gamebase Android SDK 2.35.0](./release-notes-android/#2350-2022-03-29)
* [Gamebase iOS SDK 2.35.0](./release-notes-ios/#2350-2022-03-29)

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-Unity.zip)

#### 添加功能
* 添加了可确认终端机是否允许通知的API。
    * Gamebase.Push.QueryNotificationAllowed()

#### 修改程序错误
* 修复了未能在iOS应用GamebaseWebViewConfiguration的isBackButtonVisible设置的错误。 

#### 各平台更改项目
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-2022-02-22)
* [Gamebase iOS SDK 2.34.1](./release-notes-ios/#2341-2022-03-15)

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 添加了当调用共同条款API后可确认条款UI是否被显示的VO类。
    * **GamebaseResponse.Terms.ShowTermsViewResult**

#### 改善/修改功能
* 由于在Gamebase控制台中注册Kickout时可以设置是否显示kickout弹窗，因此以下字段已被deprecated。
    * **GamebaseConfiguration.enableKickoutPopup**
    
#### 各平台更改事项 
* [Gamebase Android SDK 2.34.0](./release-notes-android/#2340-2022-02-22)
* [Gamebase iOS SDK 2.34.0](./release-notes-ios/#2340-2022-02-22)

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 添加了可更改共同条款窗设置的新API。
    * [Game > Gamebase > Unity SDK使用指南 > UI > Terms > showTermsView](./unity-ui/#showtermsview)

#### 改善/修改功能
* 添加或更改错误代码
    * GamebaseErrorCode.UNKNOWN_ERROR的错误代码从999更改为9999。
    * 添加了映射到999错误代码的GamebaseErrorCode.SOCKET_UNKNOWN_ERROR错误。
    
#### 各平台变更项目
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.32.0 (2021.12.28)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED**类型。
    * 关于此事件的适用方法，请参考如下指南。
    * [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unity-etc/#server-push)
* 添加了当Gamebase Access Token过期，登录时需要启动的**GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category。
    * [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unity-etc/#logged-out)

#### 各平台变更项目
* [Gamebase Android SDK 2.32.0](./release-notes-android/#2320-20211228)
* [Gamebase iOS SDK 2.32.0](./release-notes-ios/#2320-20211228)

### 2.31.0 (2021.12.14)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Unity SDK(0.25.0)
* 可以更改Standalone维护弹窗“是否显示维护时间”。 
* Setting Tool
    * 添加了Payco IDP。
    * 需要删除以前的SettingTool后重新进行设置。 

#### 各平台变更项目
* [Gamebase Android SDK 2.31.0](./release-notes-android/#2310-20211214)
* [Gamebase iOS SDK 2.31.0](./release-notes-ios/#2310-20211214)

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 为了改善强制映射时需要再次尝试IdP登录的不便，添加了一个新的强制映射API。
    * [Game > Gamebase > Unity SDK使用指南 > 认证 > Mapping > Add Mapping Forcibly](./unity-authentication/#add-mapping-forcibly)
* 为了解决调用Gamebase.AddMapping()后出现AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)错误的问题，添加了可将帐户转换为已有的帐户后登录的API。
    * [Game > Gamebase > Unity SDK使用指南 > 认证 > Mapping > Change Login with ForcingMappingTicket](./unity-authentication/#change-login-with-forcingmappingticket)

#### 各平台变更项目
* [Gamebase Android SDK 2.30.0](./release-notes-android/#2300-20211123)
* [Gamebase iOS SDK 2.30.0](./release-notes-ios/#2300-20211123)

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Unity SDK(0.23.5)
* Setting Tool
    * 发布了v2.0.0版本。
    * 需要先删除已有的SettingTool后再进行设置。
    * 关于修改的信息和使用方法，请参考以下指南。 
        * [Game > Gamebase > Unity SDK使用指南 > 开始 > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

#### 修改程序错误
* 修改了GamebaseDisplayLanguageCode芬兰语错误。 
    * Finish → Finnish

#### 各平台变更项目
* [Gamebase Android SDK 2.29.0](./release-notes-android/#2290-20211109)
* [Gamebase iOS SDK 2.29.0](./release-notes-ios/#2290-2021109)

### 2.28.1 (2021.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.1/GamebaseSDK-Unity.zip)

#### 修改程序错误
* 修改了未设置(Android) DisplayLanguage时，使用错误值进行设置的问题。
* 修改了在(Standalone)前帧需要较长时间时发生的Timeout错误。 

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Unity.zip)

#### 添加功能
* 添加Kakaogame认证
* 添加了“结算Abusing自动解除”功能。 
    * [Game > Gamebase > Unity SDK使用指南 > 认证 > GraceBan](./unity-authentication/#graceban)
    * 结算Abusing自动解除功能是当存在需通过“结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为“预约禁用”状态，在设定的时期内满足解除条件，则可玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认AuthToken.member.graceBanInfo API值，如果返还GraceBanInfo对象，而不返还null，要向相关用户通知禁用解除条件和时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。

#### 各平台项目变更
* [Gamebase Android SDK 2.28.0](./release-notes-android/#2280-20210928)
* [Gamebase iOS SDK 2.28.0](./release-notes-ios/#2280-20210928)  

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 改善了Display Language功能。
    * 默认语言代码为**en**，但经过改善已可适用Gamebase控制台中设置的默认语言。
        * [Game > Gamebase > 控制台使用指南 > 应用程序 > App > 语言设置](./oper-app/#language-settings)

#### 修改错误
* 修改了只用英语显示“未注册的游戏版本”错误弹窗的问题。 
* 修改了维护弹窗不显示汉语的错误。

#### 各平台变更项目
* [Gamebase Android SDK 2.27.1](./release-notes-android/#2271-20210914)
* [Gamebase iOS SDK 2.27.1](./release-notes-ios/#2271-20210914)

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Unity SDK(0.23.2)
* 添加ONE Store V16商店

#### 修改错误
* 在Unity SDK 2.25.0中删除不应添加的文件。 
    * 路径 : Assets/Gamebase/Toast/IAP/Plugins

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unity.zip)

#### 改善/修改功能
* 改善了Display Language功能。
    * 在Display Language语言集中添加了汉语简体(zh-CN)、汉语繁体(zh-TW)及泰国语(th)。
* 调用showTermsView API后，可创建PushConfiguration对象的基准被更改为；
    * 修改前
        * 仅当在条款项目中存在**接收Push**项目时，才返还有效的PushConfiguration，而不返还null。
        * 如果用户拒绝在白天和夜间接收广告性Push，PushConfiguration.pushEnabled则会创建为false。
    * 修改后
        * 如果显示了条款UI，将返还有效的PushConfiguration，而不始终返还null。
        * ShowTermsView返还的PushConfiguration对象的pushEnabled值始终为true。
    * 未修改项目
        * 若因已同意条款，未显示条款UI时，PushConfiguration将会返还为null。

#### 修改错误
* 修改了因终端机的语言代码和Push控制台中的语言代码不一致，而导致未能正常设置Push语言的问题。

### 2.25.0 (2021.07.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Unity.zip)

#### Added Features
* Added a monthly payment limit feature
    * If the monthly payment limit is exceeded, the **PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

#### Feature Updates
* The existence of a PushConfiguration object is ensured in terms and conditions with Push notification items
    * Until now, the PushConfiguration created as a result of calling the Gamebase.Terms.ShowTermsView API was null if the user declines to receive Push notifications in the terms and conditions UI. However, it has been changed so that the PushConfiguration object is always returned if there are Push notification items in the terms and conditions.
    * When user rejects push notifications, the PushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * If there is no Push notification item in the terms and conditions, the PushConfiguration object is null.
* Changed the minimum supported version of Unity: 2018.4.0f1
* External SDK Update: TOAST Unity SDK(0.23.0)

### 2.24.0(2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Unity.zip)

#### Feature Updates
* Change the internal launch URL

### 2.23.0(2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Unity.zip)

#### Feature Updates
* External SDK Update: TOAST Unity SDK(0.22.1)
* Removed warnings from Unity 2020.2 or later
* Improved initialization speed in Standalone and Unity Editor

#### Bug Fixes
* Fixed a problem where the PushConfiguration result is not null when ShowTermsView API is called even after agreeing to the terms

### 2.22.0(2021.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Unity.zip)

#### Feature Updates
* Updated the external SDK: TOAST Unity SDK(0.22.0)

### 2.21.0(2021.04.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Unity.zip)

#### More Features
* Japanese authentication for Hangame added.	

### 2.20.0(2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Unity.zip)

#### More Features
* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).
* Removed warning logs
* Updated CEF 2.1.2 for Standalone WebView
	* Fixed an issue where a crash occurs when the URL length is longer than 2,048
	* Changed the library path to improve PostProcessBuild when building in Unity 2019
	* Fixed an occasional error arising out of not initializing the string
	* Fixed a bug where the webview does not open again after moving to another scene while using the GameBase webview

### 2.19.0 (December 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) Fixed an issue that caused OutOfMemoryException on retry in WebSocket
* [SDK] 2.19.1
	* (Android) Fixed a crash when logging in with a different IdP after trying to log in to Weibo IdP

### 2.18.2 (December 15, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

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

#### Bug Fixes
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### 2.18.0 (November 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.0/GamebaseSDK-Unity.zip)

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: [Android(0.24.1)](https://docs.toast.com/en/TOAST/en/toast-sdk/release-notes-android/#0240-october-27-2020) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings
    * (iOS) Added API that supports the SceneDelegate of iOS 13 or higher

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### 2.17.1 (October 27, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Unity.zip)

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
    * (Unity) Supports Unity 2017.2.5

#### Bug Fixes  
* [SDK] 2.17.1
    * (Unity) Fixed an issue where the latter API would not work when the image notification API and web view API were called in turn.

### 2.17.0 (October 13, 2020 )
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.0/GamebaseSDK-Unity.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
	* (Common) Supports the download feature when a Customer Center attachment image is clicked
	* (Common) TOAST SDK update: Android(0.23.2), Unity(0.21.2)
	* (iOS) Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long.
	* (iOS) Changed the logic so that the title can be displayed again after changing URL and title in a web view

#### Bug Fixes  
* [SDK] 2.17.0
	* (iOS) PAYCO authentication: Fixed an issue where the logout callback would not return when logout was called after lastLoggedInProvider login
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
### 2.16.0 (September 22, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Unity.zip)

#### More Features
* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
### 2.15.0 (August 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unity.zip)

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

### 2.14.0 (August 11, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.14.0/GamebaseSDK-Unity.zip)

#### Feature Updates
* [SDK] 2.14.0
    * (iOS) Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings 
    * (iOS, Unity) Adde the contentMode setting for TCGBWebViewConfiguration

### 2.13.0 (July 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.13.0
    * (Unity) Standalone: Added Show Notice on Image API     

#### Feature Updates
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 
    * (iOS) Authenticate Sign In With Apple: Supported for iOS 12 or lower 

#### Bug Fixes
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

### 2.12.0 (July 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Unity.zip)

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order
    * [SDK] 2.12.0: Added Show Image Notice API 

#### Feature Updates 
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with storeCode(default=AS) set for configuration 
    * (iOS) Fixed failed closing due to lack of the close button while printing webview which cannot load content 
    * (Unity) Updated TOAST Unity SDK (0.20.1.1)
    
### 2.11.0 (June 23, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

### 2.10.1 (June 9, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.1/GamebaseSDK-Unity.zip)

#### Feature Updates 
* [SDK] 2.10.1
	* (iOS) Updated to set device language if language code is not configured when user push setting is initialized 

#### Bug Fixes
* [SDK] 2.10.1
	* (Unity) Fixed failed login calls since ViewController is not configured at iOS Plugin 
 
### 2.10.0 (May 26, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

#### Feature Updates 
* [SDK] 2.10.0 
	* (Unity) Updated CefWebview version with StandaloneWebviewAdapter: v2.0.4
		* Updated the logic of WebviewIndex validation  
		* Fixed infrequent error of NullReferenceException while Webview is created 
    * (Unity) Added the error code regarding socket connection to the GamebaseErrorCode definition: SOCKET_CONNECTION_TIMEOUT, SOCKET_CONNECTION_FAIL

#### Bug Fixes
* [SDK] 2.9.1
    * (Andoird) Fixed an error where the indicator level became null after mapping and not reflected normally in the payment indicator
    * (iOS) When building in unreal engine, a warning is judged as a build error and the build is not possible.    

### 2.9.1 (April 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unity.zip)

#### Bug Fixes
* [SDK] 2.9.1 
	* (Unity) Fixed a error which occurs when client service status is changed after initialized on console 
		* Versions at Issue: v2.8.0 or higher	
		* Platforms at Issue: Standalone, WebGL, and Editor

### 2.9.0 (April 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unity.zip)

#### More Features
* Suspension of Membership Withdrawal 
	* [SDK 2.9.0]
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

#### Feature Updates
* [SDK 2.9.0]
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated PAYCO Login SDK: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (April 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Unity.zip)

#### Feature Updates 
* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	
### 2.8.0 (March 24, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Unity.zip)

#### More Features 
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 
	* (Unity) Updated CefWebview to v2.0.1 within StandaloneWebviewAdapter 
		* When the PopupType is PASS_INFO, popup data can be delivered without a popup 

#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 

### 2.7.2 (March 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Unity.zip)

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

### 2.7.0 (January 21, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Unity.zip)

#### More Features
* [SDK] 2.7.0
	* (Unity) Supports NaverCafePLUG 

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 
	* (Unity) Added the gamebase://dismiss scheme handling for a web login
	* (Unity) Modified infrequent failure in the display of webview for a release build 	

### 2.6.3 (January 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.3/GamebaseSDK-Unity.zip)

#### Feature Updates
* [SDK] 2.6.3
	* (Unity) Updated Standalone Webview: Updated CefWebview 	
	* (Unity) Added .dll file missing from an error occured after login 
		* ToastCommon.dll, vcruntime140.dll

#### Bug Fixes
* [SDK] 2.6.3
	* (Unity) Fixed error that occur when Login(CredentialInfo) API is called
	
### 2.6.2 (December 24, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Unity.zip)

#### More Features
* Conpon > Publish: Added the feature of keyword coupons

#### Feature Updates
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) NAVER SDK Updates (4.1.0)

### 2.6.1 (November 20, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Unity.zip)

#### Bug Fixes
* [SDK] 2.6.1
  * (Unity) Added the iOS Plugin file (toast_sdk_wrap.m) to Package in order to prevent errors for an iOS build  
  * (Unity) Fixed the error of UnityEditor in which the store code comes as empty on a platform other than Standalone, resulting in failed initialization  
  * (Unity) Fixed the error of NullReferenceException due to errors from processing zone type within Initialize API 

### November 13, 2019

#### Bug Fixes
* GamebaseSettingTool
  * Fixed the error in which files are not properly updated, with the version updated to Gamebase v2.6.0


### 2.6.0 (November 12, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Unity.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features 
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

### October 15, 2019
#### Feature Updates 
* Sample App
	* Updated Gamebase SDK (v2.4.0)
	* Applied Smart Downloader (v1.5.8) and Leaderboard 
	* More Features: Downloading game resources, integrating Leaderboard and TAA indicators, transferring devices, and forced mapping 
	* Updates: Added ServerPush listeners and detection of observer maintenance  
	* Renewed games 
		
### 2.5.0 (August 27, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Unity.zip)

#### More Features 
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
### August 2, 2019 
#### Bug Fixes 
* [SDK] Setting Tool 1.4.3
	* Fixed the build error by moving down the script file below the editor folder  
	* Fixed failed operations when Multilanguage is provided with the entire path of the language file on MAC OS. 

### 2.4.4 (July 23, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Unity.zip)

#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)
* Setting Tool
	* Change of Folder Structure: Must reinstall after previous SettingTool is completely deleted  
	* More languages are supported 
	
### 2.4.3 (July 11, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.3/GamebaseSDK-Unity.zip)

#### Bug Fixes 
* [SDK] 2.4.3
	* (Unity) Fixed failed operations of AddMappingForcibly API, for the builds in iOS or Android 
	* (Unity) Fixed the json parsing error at iOSPlugin when RequestRetryTransaction API is called 

### June 27,2019

#### Bug Fixes
* [SDK] Setting Tool 1.4.1
	* Fixed the error in uploading existing setting data when GamebaseSettingTool was executed

### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Unity.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 

### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Unity.zip)

#### Feature Updates 
* Purchase for HANGAME mix Available for Japan 
    * [SDK] 2.4.0
      * (Unity) Added external purchase for Standalone Japan  
      * (Unity)  Added HANGAME authentication for Standalone Japan 

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Change of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]

    * (Android) NAVER SDK Version Updated (v4.2.5): Bug of NAVER SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while NAVER login was underway)  
    * (Unity) StandaloneWebview supports 32bit Build (SDK volume upgraded from 53.6MB to 99.2MB)