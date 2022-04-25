## Game > Gamebase > Release Notes > Android

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Android.zip)

#### 기능 추가
* 고객센터 URL 뒤에 파라미터를 추가할 수 있도록 다음 필드가 추가되었습니다.
    * **ContactConfiguration.Builder.setAdditionalParameters(Map<String, String>)**

#### 기능 개선/변경
* 외부 SDK 업데이트 : Toast Gamebase IAP 0.18.3
* Amazon appstore 결제 데이터에서 userId, gamebaseProductId가 누락될 시 userId, gamebaseProductId를 자동으로 채우도록 개선되었습니다.

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Android SDK(0.29.2), TOAST Gamebase IAP Android SDK(0.18.2), Hangame Android SDK(1.4.5)
* Hangame Android SDK에서 v1.4.5에서 sms_hash가 내부에서 생성되도록 개선되었습니다.
    * 더 이상 sms_hash를 설정하지 않아도 됩니다.

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Android.zip)

```
Gamebase Android SDK는 이제 Maven Central로만 배포합니다.
더 이상 배포용 zip 파일에서 aar 파일을 포함하지 않습니다.
```

#### 기능 추가
* 약관 창이 표시되었는지 여부를 알 수 있는 API가 추가되었습니다.
    * **Gamebase.Terms.isShowingTermsView()**
* 웹뷰에서 폰트 사이즈를 고정할 수 있는 옵션이 추가되었습니다.
    * **GamebaseWebViewConfiguration.Builder.enableFixedFontSize(boolean)**
* 약관 창에서 폰트 사이즈를 고정할 수 있는 옵션이 추가되었습니다.
    * **GamebaseTermsConfiguration.Builder.enableFixedFontSize(boolean)**
* Facebook, Naver 로그인시 Facebook, Naver 앱이 설치되어 있더라도 강제로 웹로그인을 진행하는 기능이 추가되었습니다.
    * 이 기능을 사용하기 위해서는 Gamebase Console의 AdditionalInfo에 다음과 같이 설정하세요.

```
{"enforce_app2web":true}
```

* 이제 Naver 로그아웃시 토큰을 삭제하지 않습니다.
    * 재로그인 할 때 정보 제공 동의 창이 뜨지 않습니다.
    * 웹로그인시에는 계정이 변경되지 않습니다.
    * 이전 동작을 유지하기 위해서는 Gamebase Console의 AdditionalInfo에 다음과 같이 설정하세요.

```
{"logout_and_delete_token":true}
```

#### 기능 개선/변경
* 외부 SDK 업데이트: TOAST Android SDK(0.29.1), Hangame Android SDK(1.4.4)
* 약관 창이 표시될때 흰색 배경이 길게 표시되지 않도록 개선했습니다.

#### 버그 수정
* 웹뷰의 네비게이션바를 숨기는 **GamebaseWebViewConfiguration.Builder.setNavigationBarVisible()** API가 정상동작 하지 않는 이슈를 수정했습니다.

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Android.zip)

#### 添加功能
* 在Gamebase控制台的“必须更新”设定中选择**添加弹窗按钮**时，客户端的必须更新弹窗中将添加一个**查看更多**按钮。
* 添加了可确认终端机是否允许提示通知的API。
    * **Gamebase.Push.queryNotificationAllowed()**
* 添加了当调用共同条款API后可确认条款UI是否被显示的VO类。 
    * **GamebaseShowTermsViewResult**

#### 改善/更改功能
* 由于在Gamebase控制台中注册Kickout时可以设置是否显示kickout弹窗，因此以下字段已被deprecated。
    * **UIPopupConfiguration.enableKickoutPopup**

#### 修改错误    
* 即使在图片通知中已点击**24小时内不显示**，但过24小时也不显示图片通知的错误被修改。 

### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加了可通过更改共同条款显示选项的新API。
    * 关于可以更改设置的项目，请参考以下指南。
    * [Game > Gamebase > Android SDK使用指南 > UI > Terms > showTermsView](./aos-ui/#showtermsview)

#### 改善/更改功能
* 外部SDK升级 : PAYCO Android SDK(1.5.7), Hangame Android SDK(1.4.3.1), TOAST Gamebase IAP Andoid SDK(0.18.1)
* 添加了登录成功后可检查Launching信息是否被更改的逻辑。   

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Android.zip)

#### 添加功能 
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED**类型。
    * 关于此事件的适用方法，请参考如下指南。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./aos-etc/#server-push)
* 添加了当Gamebase Access Token过期，登录时需要启动的**GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./aos-etc/#logged-out)
 
#### 改善/修复功能
* 改善Webview后，现在可启动Webview URL地址开始为**onestore://**的ONE store Deeplink。

#### 修改程序错误
* 修改了已在Gamebase Android SDK 2.31.0中调用退出函数，但因未调用IdP登录函数，无法更改IdP账户的程序错误。 

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Android.zip)

#### 改善/修复功能
* 外部SDK升级 : TOAST Android SDK(0.29.0)
* 解决了被禁用的用户（使用禁用信息）无法通过“禁用Webview”内客户服务链接注册查询的问题。
* 修复了打开应用程序后立即调用Gamebase初始化函数时，Launching弹窗显示英语的问题。
* 改善Scheduler之后，现在您可以始终确认当应用程序从后台转为前台时Launching信息是否已被更改。
    
### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Android.zip)

#### 添加功能   
* 为了改善强制映射时需要再次尝试IdP登录的不便，添加了一个新的强制映射API。
    * [Game > Gamebase > Android SDK使用指南 > 认证 > Mapping > Add Mapping Forcibly](./aos-authentication/#add-mapping-forcibly) 
* 为了解决调用Gamebase.addMapping()后出现AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)错误的问题，添加了将帐户转换为已有的帐户后可进行登录的API。
    * [Game > Gamebase > Android SDK使用指南 > 认证 > Mapping > Change Login with ForcingMappingTicket](./aos-authentication/#change-login-with-forcingmappingticket)

#### 改善/修复功能
* 外部SDK升级 : Hangame Android SDK(1.4.2)
* 改善后，用户可直接修改或使用Gamebase提供的”查看维护详情Webview”html。 
    * [Game > Gamebase > Android SDK使用指南 > 初始化 > Launching Information > 1. Launching > 1.3 Maintenance > Change Default Maintenance HTML](./aos-initialization/#change-default-maintenance-html)
* 修改了即使设置DisplayLanguageCode也仍以终端机设置的语言显示维护Webview时间的错误。 
* 改善了因以断开的连接尝试通信而反复出现网络通信错误的问题。

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Android.zip)

#### 添加功能
* 已添加登录Google时声明Scope的功能。
    * [https://developers.google.com/identity/protocols/oauth2/scopes](https://developers.google.com/identity/protocols/oauth2/scopes)
    * 通过Scope添加**email**时，可在简介中获取Email信息。 
    * 如下所示，若在Gamebase Console的AdditionalInfo中进行设置，Gamebase登录谷歌时将自动设置Scope。

```
{"scope":["email","myscope1","myscope2",...]}
```

#### 改善/修复功能
* 外部SDK升级 : TOAST Android SDK(0.27.4)
* 只在DisplayLanguage指南上描述，实际上未包含在SDK的DisplayLanguage.Code类已被添加。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Display Language > Gamebase支持的语言代码种类](./aos-etc/#gamebase)

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加Kakaogame认证
* ”结算Abusing自动解除”功能已被添加。 
    * [Game > Gamebase > Android SDK使用指南 > 认证 > GraceBan](./aos-authentication/#graceban)
    * 结算Abusing自动解除功能是当存在需通过”结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为”预约禁用”状态，在设定的时期内满足解除条件，则可正常玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认AuthToken.getGraceBanInfo() API值，如果返还GraceBanInfo对象，而不返还null，要向相关用户通知禁用解除条件、时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。
* 等待登录响应时，显示等待图标。  

#### 改善/修复功能 
* 外部SDK升级 : PAYCO Android SDK(1.5.6)  
  
### 2.27.1 (2021.09.14) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Android.zip)   

#### 改善/修复功能
* 外部SDK升级 : PAYCO Android SDK(1.5.5), Hangame Android SDK(1.4.1), Weibo Android SDK(11.8.1)
* 通过反复重试，改善了模拟器和Rooting terminal不正常显示Webview的问题。
    * 对象包括通过Webview启动的图片通知，客户服务和共同条款。
* 通过改善Weibo IdP认证，提高了稳定性。
    * Gamebase对实际上是同步API，但启动为异步API而导致错误的API添加了意外处理、等待、重试等的辅助处理。  

#### 修改错误
* 已修改只用英语显示”未注册的游戏版本”错误弹窗的问题。 
* 已修改维护弹窗不显示汉语的错误。
* 已修改当进行[Credential Login](./aos-authentication/#login-with-credential)时，[Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp)调用失败的错误。 

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Android.zip)

#### 改善/修复功能
* 外部SDK升级 : TOAST Android SDK(0.27.1)
* 添加ONE Store V16商店

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Android.zip)

#### 改善/修复功能
* Display Language功能已被改善。
    * 以前为了添加语言集，必须直接修改gamebase-sdk-base-version.aar文件。
        * 但经过功能改善，将localizedstring.json文件放入res/raw文件夹里就可添加语言了。
    * 以前未能将Unity指南的Display Language语言集添加方法适用于Android。
        * 但经过功能改善，按照Unity指南添加localizedstring.json文件时，可将其方法适用于Android打包了。
        * [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Display Language > 添加新语言集](https://docs.toast.com/en/Game/Gamebase/en/unity-etc/#add-new-language-sets)
    * 在Display Language语言集中添加了汉语简体(zh-CN)、汉语繁体(zh-TW)及泰国语(th)。
    * 默认语言代码为**en**，但经过改善已可适用Gamebase控制台中设置的默认语言。
        * [Game > Gamebase > 控制台使用指南 > 应用程序 > App > 语言设置](https://docs.toast.com/en/Game/Gamebase/en/oper-app/#language-settings)
* 调用showTermsView API后，可创建PushConfiguration对象的基准被更改为；
    * 修改前
        * 仅当在条款项目中存在**接收Push**项目时，才返还有效的PushConfiguration，而不返还null。
        * 用户拒绝在白天和夜间接收广告性Push时，PushConfiguration.pushEnabled将会创建为false。
    * 修改后
        * 如果显示了条款UI，将返还有效的PushConfiguration，而不始终返还null。
        * showTermsView返还的PushConfiguration对象的pushEnabled值始终为true。
    * 未修改项目
        * 若因已同意条款，条款UI未被显示时，PushConfiguration将会返回为null。

#### 修改错误
* 已修改因终端机的语言代码和Push控制台中的语言代码不一致，而导致未能正常设置Push语言的问题。

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Android.zip)

#### More Features
* Add monthly payment limit feature
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

#### Feature Updates
* Change the dependency of Android Support Library to AndroidX
* Guarantee the PushConfiguration object in the terms and conditions with Push notification items
    * The PushConfiguration to be created as the result of calling Gamebase.Terms.showTermsView API was null if user did not agree to receive push notifications in the terms of UI. It has now changed so that the PushConfiguration object is always returned if there is a Push notification item in the terms and conditions.
    * When user rejects push notifications, the PushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * The PushConfiguration is null when there is no Push notification item in the terms and conditions.
* External SDK Update
    * TOAST Android SDK(0.26.0)
    * Kotlin(1.5.21)
    * Google Play Services Auth(19.0.0)
    * Facebook Android SDK(11.1.0)
    * Naver Android SDK(4.4.1)
    * Line Android SDK(5.6.2)
    * Weibo Android SDK(11.6.0)
* Fixed a crash that occurred when logging in to Weibo.

### 2.24.0 (2021.06.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Change the internal launch URL
* Fixed incorrect wording in SDK attachments

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed the issue of the title of the suspended view details web view not being displayed

### 2.22.0 (2021.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated the external SDK: TOAST Android SDK(0.25.0), Hangame Android SDK(1.4.0)

#### Bug Fixes
* The following error has been fixed: When a user logs out and logs in again with another user ID, a payment at Google Play Store is successful but the return value is sometimes "Failed."
* The following error has been fixed: When the name of an app package contains a capital letter, the "Sign In with Apple" log-in fails.

### 2.21.1 (2021.04.19) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed an issue where the system crashes when canceling Hangame login via PAYCO

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

#### More Features
* Japanese authentication for Hangame added.	 	

#### Feature Updates
* External SDK update: Facebook Android SDK (6.5.1), Line Android SDK (5.4.0)
	
#### Bug Fixes
* Fixed a crashing error caused when calling payment API on build with Proguard applied.

### 2.20.2 (2021.03.30) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated to Billing Client Version 3.0.3 where payment errors caused by Android 11 devices in Google Play Store are fixed

### 2.20.1 (2021.02.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed a logic that could cause the push-fcm module to crash during initialization

### 2.20.0 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Android.zip)

#### More Features
* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

### 2.19.1 (December 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added
	
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) WebSocket에서 재시도 시 OutOfMemoryException이 발생하는 문제 수정
* [SDK] 2.19.1
	* (Android) Weibo 로그인 시도 후 다른 IdP로 로그인 시 크래시가 발생하는 문제 수정

### 2.18.2 (December 15, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Android.zip)

#### More Features
* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [SDK] 2.18.2
    * (Common) TOAST SDK update: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) External SDK update to resolve encryption logic security warnings: Payco Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
#### Bug Fixes
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### 2.18.1 (November 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.1/GamebaseSDK-Android.zip)

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: [Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### 2.17.1 (October 13, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Android.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
	* (공통) Supports the download feature when a Customer Center attachment image is clicked
	* (공통) TOAST SDK update: Android(0.23.2), Unity(0.21.2)

#### Bug Fixes  
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
### 2.16.0 (September 22, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Android.zip)

#### More Features
* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
### 2.15.0 (August 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Android.zip)
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

### 2.13.0 (July 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 

#### Bug Fixes
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

### 2.12.0 (July 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Android.zip)

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order 
    * [SDK] 2.12.0: Added Show Image Notice API 
  
### 2.11.0 (June 23, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

### 2.10.0 (May 26, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

### 2.9.1 (May 12, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.9.1
	* (Andoird) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error 

### 2.9.0 (April 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Android.zip)

#### More Features
* Suspension of Membership Withdrawal 
	* [SDK] 2.9.0
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

#### Feature Updates
* [SDK] 2.9.0
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated Payco Login SDK: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (April 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	
#### Bug Fixes
* [SDK] 2.8.1 
	* (Android) Modified codes that may cause crashes after process restarts

### 2.8.0 (March 24, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 

#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 
	
### 2.7.2 (March 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.7.2 
      * Gamebase 초기화중 ToastLogger 초기화 부분에서 크래쉬가 발생할 수 있는 코드를 수정
      * 서버 버전을 v1.2.1 로 업데이트 하였습니다.

### 2.7.1 (February 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called

### 2.7.0 (January 21, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 

### 2.6.2 (December 24, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)

### 2.6.1 (December 10, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize() 
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address 
  * (Android) Fixed crash occurrence when IAP is not enabled  

### 2.6.0 (November 12, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Android.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features 
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (Android) Added the payment feature for Google subscription  
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase. 

### 2.5.0 (August 27, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
### 2.4.4 (July 23, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)

### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Android.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 
	
### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Android.zip)

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Chanage of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#level-up-trace)]

    * (Android) Naver SDK Version Updated (v4.2.5): Bug of Naver SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while login to Naver was underway)  

### 2.3.1 (2019.05.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.1/GamebaseSDK-Android.zip)

#### 버그수정
* [SDK] 2.3.1
    * (Android)2.3.0버전에서 Twitter 로그인 되지 않던 문제 수정

### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Android.zip)
    
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

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Android.zip)

#### 버그수정
* [SDK] 2.2.2
    * (Android)Gamebase 초기화 이전 TransferAccount API 호출시, 콜백이 오지 않는 이슈를 수정

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Android.zip)

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
    * (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 2.1.0
    * (공통)TransferKey API 삭제
        * issueTransferKey : TransferKey 발급
        * requestTransfer : TransferKey 검증
        
#### 버그수정
* [SDK] 2.1.0
    * (Android)Gamebase 초기화 이전, onActivityResult()가 호출되면서 이상 동작하던 버그 수정

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Android.zip)

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
    * (Android)Push SDK 업데이트(android:1.7.0)
    * (Android)Adapter API 변경
        * Launching 정보 전달
        * logout, withdraw API에 Callback 추가

### 1.14.5 (2018.12.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.5/GamebaseSDK-Android.zip)

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
        * 크래시를 방지하기 위해 모든 IAP SDK 호출부에 예외 처리

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.14.2
    * (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정

#### 버그수정
* [SDK] 1.14.2
    * (Android)에뮬레이터 환경에서 스토어앱(PlayStore, OneStore 등)이 없는 상태에서 "앱 설치/업데이트"시 스토어 미체크로 인한 crash 버그를 수정
    
### 1.14.1 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.1/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.14.0
    * (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
    
#### 기능 개선/변경
* [SDK] 1.14.0
    * (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
    * Remove API : Webview, Network, Launching
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)        
    * Deprecated  API 
        * (void)Gamebase.WebView.showWebView(Activity, String)
        * (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
    
#### 버그수정
* [SDK] 1.14.1
    * (Android)Auth API 호출 후 콜백에서 다시 Auth API 중복 호출시 정상 호출이 되지 않는 버그 수정
    
### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.13.0
    * (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
    * (Android)Push API 호출 시, Gamebase 초기화/로그인 상태에 따라 호출 실패에 대한 에러 메시지를 보다 명확하게 개선
        * 초기화 전 호출 : NOT_INITIALIZED(1)
        * 초기화 이후 호출시 Push 모듈이 없음 : NOT_SUPPORTED(10)
        * 초기화 성공 및 로그인 이전 호출 : NOT_LOGGED_IN(2)        
    
#### 버그수정
* [SDK] 1.13.0
    * (Android)NaverCafe SDK와의 충돌로 Naver 로그인시 발생하던 오류 해결
        
### 1.12.2 (2018.08.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.12.2
    * (Android)WebSocket 타입아웃시 (API 호출 시간 경과), 크래시가 날 수 있는 버그에 대해 방어로직 처리
    
#### 버그수정
* [SDK] 1.12.2
    * (Android)auth-twitter-adapter 를 포함한 상태에서 TargetSdk 28로 빌드시 초기화 에러가 발생하는 문제 수정

### 1.12.1 (2018.08.09)

#### 기능 개선/변경
* [SDK] 1.12.1
    * (공통)IAP SDK 최신버전 적용 (1.5.0)
    * (공통)Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
    * (공통)점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
    * (공통)IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (공통)인증 API 중복 호출시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (Android)TencentPush SDK 업데이트 (3.2.3)
    * (Android)Onestore v17(API v5) 지원 : Gamebase에서는 v16(스토어코드=TS)은 제공하지 않습니다.

### 1.11.1 (2018.07.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.11.1
    * (공통)Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경
    
#### 버그수정
* [SDK] 1.11.1
    * (공통)점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정
    * (Android)Gamebase.addObserver()를 통해 ObserverMessage를 수신하였을 경우, ObserverMessage.data.code의 타입이 int가 아니라 String인 버그를 수정


### 1.11.0 (2018.06.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Twitter IdP 추가 : Android, iOS
* Line IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
    
#### 기능 개선/변경
* [SDK] 1.11.0
    * (공통)LocalizedString 일본어 번역 추가
    * (공통)인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
    * (Android)'android.permission.READ_PHONE_STATE' 권한 제거
    * (Android)GamebaseConfiguration.Builder의 필수 설정값인 setAppId, setAppVersion을 생성자에서 입력할 수 있도록 변경
    * (Android)GamebaseConfiguration.Builder 의 setServerApiVerseion API를 제거
    * (Android)getAuthBanInfo() API, class AuthBanInfo 이름을 변경 : getBanInfo(), class BanInfo

### 1.9.0 (2018.05.03)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)

#### 버그 수정
* [SDK] 1.9.0
    * (Android) Heartbeat 에서 잘못된 사용자로 판정되는 경우 이용정지 팝업 창이 뜨지 않도록 수정(iOS 와 동일한 로직으로 수정)

### 1.8.1 (2018.04.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Android.zip)

#### 버그 수정
* [SDK] 1.8.1
    * (Android. iOS)registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 1.8.0 (2018.04.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* Observer 기능 개발 및 API 추가
    * (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
    * (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)

### 1.7.0 (2018.02.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.7.0
    * Naver IdP 인증 추가
    * Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 1.5.0 (2017.12.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Android.zip)
#### 기능 추가
* [SDK] 1.5.0
    * WebView가 닫힐 때 발생하는 Close Callback 추가
    * WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가

### 1.4.0 (2017.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Android.zip)

#### 버그 수정
* [SDK] 1.4.0 업데이트
    * (Android)Gamebase 제공 팝업 창을 사용하지 않는 경우 이용정지 정보가 null로 리턴되는 오류 수정

### 1.3.0 (2017.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.3.0 업데이트
    * Credential을 이용한 AddMapping API추가

### 1.2.0 (2017.09.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Android.zip)

#### 기능 추가
* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
    * 이용정지 사용자 팝업 창 노출


### 1.1.5 (2017.07.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.5 업데이트
    * 시스템 팝업 창 API 추가 (showAlertWithTitle)
    * 국가코드를 대문자로 반환하도록 변경 (Android)
    * TCPush SDK 1.4.1 로 업데이트
    * IAP SDK 1.3.3.20170627 로 업데이트

### 1.1.4 (2017.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Android.zip)
#### 기능 개선/변경
* [SDK] 1.1.4 업데이트
    * 런타임 중 결제 Store를 변경할 수 있는 API 제공
    * (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공

### 1.1.3 (2017.04.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.3/GamebaseSDK-Android.zip)
#### 기능 개선/변경
* [SDK] 1.1.3 업데이트
    * (Android)론칭 구조 및 팝업 창/점검 페이지 개선 :커스텀 점검 페이지 설정 기능 추가
    * (Android)인증 구조 개선 및 로그 추가 : 인증 Adapter 및 SDK 버전 로그 출력

#### 버그 수정
* [SDK] 1.1.3 업데이트
    * (Android)Facebook SDK v4.19.0 이상에서 초기화시 크래시 오류 수정


### 1.1.2 (2017.04.04)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임론칭시 점검, 긴급공지 팝업 창 개선

### 1.1.0 (2017.03.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Android.zip)

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
    * 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
    * 로그아웃 및 회원탈퇴 기능을 제공
    * 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
    * 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
    * 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
    * TOAST Cloud상품 연동 : PUSH, IAP
