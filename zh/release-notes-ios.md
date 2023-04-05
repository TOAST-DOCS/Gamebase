## Game > Gamebase > Release Notes > iOS

### 2.49.0 (2023. 04. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트
    * Hangame iOS SDK (1.8.5)

### 2.48.0 (2023. 03. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* Xcode 최소 지원 버전이 14.1로 변경되었습니다. 
* iOS 최소 지원 버전이 11.0으로 변경되었습니다.
* armv7, armv7s, i386 아키텍쳐 지원을 중단하였습니다.
* 더 이상 bitcode를 지원하지 않습니다.
* 외부 SDK 업데이트
    * NHN Cloud iOS SDK (1.3.0)
    * PAYCO iOS SDK (1.5.6)
* DNS 장애를 대비한 Gamebase 서버 예비 도메인 적용

#### 버그 수정
* 특정 상황에서 킥아웃 이벤트가 오지 않는 버그를 수정하였습니다.
* 웹뷰 커스텀 스킴 콜백이 호출되지 않는 버그를 수정하였습니다.

### 2.47.0 (2023. 02. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.47.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 外部SDK升级
    * Hangame iOS SDK (1.8.4)
    
### 2.46.0 (2023. 01. 31.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 外部SDK升级
    * Hangame iOS SDK (1.8.2)
    * Kakaogame iOS SDK (3.14.14)
* 改善了SDK内部逻辑

### 2.45.0 (2022. 12. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-iOS.zip)

#### 添加功能    
* 添加了以下字段以区分是对哪个商店的付款收据。
    * **TCGBPurchasableReceipt.storeCode**
* 添加了**TCGBPurchasableConfiguration** VO，它允许在调用支付API时进行其他设置。
    * [Game > Gamebase > iOS SDK使用指南 > 结算 > TCGBPurchasableConfiguration](./ios-purchase/#tcgbpurchasableconfiguration)
* 添加了将**TCGBPurchasableConfiguration**接收为参数的“新未消费明细API”。
    * **[TCGBPurchase requestItemListOfNotConsumedWithConfiguration:completion:]**
* 添加了将**TCGBPurchasableConfiguration**接收为参数的“新激活订阅查询API”。
    * **[TCGBPurchase requestActivatedPurchasesWithConfiguration:completion:]**
 
#### 改善/修改功能
* 外部SDK升级
    * NHN Cloud iOS SDK (1.2.0)
    * Hangame iOS SDK (1.8.0)
* 以下API已被deprecated。
    * **[TCGBPurchase requestItemListOfNotConsumedWithCompletion:]**
    * **[TCGBPurchase requestActivatedPurchasesWithCompletion:]**
* 改善了SDK内部逻辑。

### 2.44.0 (2022. 10. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 更改了LINE iOS SDK的依赖性。

### 2.43.3 (2022. 10. 04.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 改善了SDK内部逻辑。

### 2.43.2 (2022. 09. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.2/GamebaseSDK-iOS.zip)

#### 修改程序错误
* 修改了登录Game Center时出现错误的程序错误。

### 2.43.1 (2022. 09. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.1/GamebaseSDK-iOS.zip)

#### 修改程序错误
* 修改了由于Line SDK依赖性错误导致通过CocoaPods发布的Line Auth Adpater无法设置Region的错误。

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 外部SDK升级
    * NHN Cloud iOS SDK (1.0.0)
    * ToastGamebaseIAP iOS SDK (0.14.0)
    * LINE iOS SDK (5.8.2)
    * Kakaogame iOS SDK (3.14.4)
    * Hangame iOS SDK (1.7.1)
* 修改后当登录LINE时可输入提供服务的区域。
    * [Game > Gamebase > iOS SDK使用指南 > 认证 > Login with IdP](./ios-authentication/#login-with-idp)
* 已被更改为允许在初始化过程中使用不与Gamebase兼容的Gamebase Adapter时发生错误。
* 改善了SDK内部逻辑。

### 2.42.2 (2022. 08. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.2/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 由于在App Store审查中被拒绝，WebView中使用的Scheme中的“itms-services”被删除。

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-iOS.zip)

#### 修改程序错误
* 修复了启用对比度增加选项时Gamebase弹窗未正确显示的错误。
* 修复了使用SceneDelegate的项目中不显示Gamebase弹窗的错误。

### 2.42.0 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 在映射失败时返还的ForcingMappingTicket VO类中添加了字段，以让您了解用户的当前状态。
    * **TCGBForcingMappingTicket.mappedUserValid**
    * 关于mappedUserValid中保存的值，请参考以下指南。
        * [Game > Gamebase > API指南 > API v1.3指南 > Others > Mamber Vaild Code](./api-guide/#member-valid-code)

#### 改善/修改功能
* 外部SDK升级 : Hangame iOS SDK (1.7.0)

#### 修改程序错误
* 修改了使用错误的AppID初始化Gamebase时未能调用回调的错误。
* 修复了Hangame登录用户未能收到GamebaseEventHandler的**kTCGBIdPRevoked**事件的错误。

### 2.41.1 (2022. 07. 20.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 修改为在条款窗完全关闭后调用回调。

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBIdPRevoked**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./ios-etc/#idp-revoked)

#### 改善/修改功能
* 更改了图片通知，使其在显示时根据屏幕方向旋转。

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 改善了SDK内部逻辑

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 外部SDK升级 : Hangame iOS SDK (1.6.4)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* 更改了Display Language中汉语繁体(zh-TW)语言集中的错误句子。

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 为了在客户服务URL后边添加参数，添加了以下字段。
    * **TCGBContactConfiguration.additionalParameters**

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加了以下字段以确定付款收据中的付款是Sandbox付款还是Promotion付款。
    * **TCGBPurchasableReceipt.sandboxPayment**
    * **TCGBPurchasableReceipt.promotionPayment** 

#### 改善/修改功能
* 外部SDK升级 : TOAST iOS SDK(0.30.0), ToastGamebaseIAP SDK(0.13.0), Hangame iOS SDK (1.6.3)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加了用于确认是否显示条款窗的API。
    * **[TCGBTerms isShowingTermsView]**

#### 改善/修改功能
* 将登录方式从Google网络登录方式更改为Google SDK登录方式。
* 更改后，在登录过程中取消Hangame登录时返回**TCGB_ERROR_AUTH_USER_CANCELED(3001)**错误 。

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

#### 添加功能
* 为了Swift项目用户，在Public API添加了NS_SWIFT_NAME设置。

#### 改善/修改功能
* 外部SDK升级 : Hangame iOS SDK (1.6.2)
* 更改了当设备为横向模式调用showWebView API时，在下端输出黑色空白的错误。 

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 当在Gamebase控制台的必须更新设定中选择**添加弹窗按钮**时，客户端的必须更新弹窗中将添加一个**查看更多**按钮。
* 添加了可确认终端机是否允许提示通知的API。
    * **[TCGBPush queryNotificationAllowedWithCompletion:]**
* 添加了当调用共同条款API后可确认条款UI是否被显示的VO类。 
    * **TCGBShowTermsViewResult**

#### 改善/修改功能
* 当调用图片通知API时没有可以显示的图片通知时，背景突然变暗的现象被修改。
* 由于在Gamebase控制台中注册Kickout时可以设置是否显示kickout弹窗，因此以下字段已被deprecated。
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**
    
### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加了可更改共同条款窗设置的新API。
    * [Game > Gamebase > iOS SDK使用指南 > UI > Terms > showTermsView](./ios-ui/#showtermsview)

#### 改善/修改功能
* 外部SDK升级 : PAYCO iOS SDK (1.5.5)
* 添加或更改错误代码
    * TCGB_ERROR_UNKNOWN_ERROR的错误代码从999更改为9999。
    * 添加了映射到999错误代码的TCGB_ERROR_SOCKET_UNKNOWN_ERROR错误。 
    
### 2.32.1 (2022.01.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)
 
#### 改善/修改功能 
* 点击推荐更新弹窗里的**立即更新**按钮时弹窗被关闭的问题已修复。
* 提高了SDK稳定性。

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBServerPushAppKickoutMessageReceived**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBLoggedOut**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

#### 改善/修改功能
* Webview navigationBar的default title Color被更改为**UIColor.whiteColor**。

#### 修改程序错误
* 修改后，现在当调用Hangame注销时，thirdIdP也将注销。

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 可以更改维护弹窗“是否显示维护时间”。

#### 改善/修改功能
* 外部SDK升级 : TOAST iOS SDK (0.29.2), PAYCO iOS SDK (1.5.4)
* 解决了已被禁用的用户（使用禁用信息）未能通过“禁用Webview”内的客户服务链接注册查询的问题。
* 修复之后，现在当用户按维护和禁用弹窗中的“查看更多”按钮时，在Webview中可显示“返回上一页”按钮。

### 2.30.1 (2021.11.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.1/GamebaseSDK-iOS.zip)

#### 修改程序错误
* 在Unity 2019.3或更高版本中设置Cocoapods时调用结算和推送API时发生的程序错误已被修改。

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 为了改善强制映射时需要再次尝试IdP登录的不便，添加了一个新的强制映射API。
    * [Game > Gamebase > iOS SDK使用指南 > 认证 > Add Mapping Forcibly](./ios-authentication/#add-mapping-forcibly)
* 为了解决因使用特定IdP尝试映射，出现**TCGB_ERROR_AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**错误的问题，添加了将帐户转换为已有的帐户后可进行登录的API。
    * [Game > Gamebase > iOS SDK使用指南 > 认证 > Change Login](./ios-authentication/#change-login)

#### 修改程序错误
* 修改了调用loginForLastLoggedInProvider登录后，使用特定IdP时未能注销或退出的程序错误。

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* Xcode支持的最低版本从12升级到13。
* 外部SDK升级 : TOAST iOS SDK(0.29.1), ToastGamebaseIAP SDK(0.12.1)
* 即使未对”在控制台中注册的维护”和”查看公告详情”URL进行编码，也可在页面上显示。

#### 修改程序错误
* 已修改将TCGBPushMessage.extras转换为json格式时出现错误的问题。

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加Kakaogame认证
* “结算Abusing自动解除”功能已被添加。 
    * [Game > Gamebase > iOS SDK使用指南 > 认证 > GraceBan](./ios-authentication/#graceban)
    * 结算Abusing自动解除功能是当存在需通过”结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为“预约禁用状态”，在设定的时期内满足解除条件，则可正常玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认TCGBAuthToken.tcgbMember.graceBanInfo值，如果返还TCGBGraceBanInfo对象，而不返还null，要向相关用户通知禁用解除条件、时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。

#### 改善/修改功能
* PAYCO iOS SDK升级(1.5.2) 

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* PAYCO iOS SDK升级(1.5.1)
    * 验证流程和UI的改善
* Hangame iOS SDK升级(1.6.1)
    * 已修改在身份验证阶段发生错误时未能调用回调的问题。
    * 已修改在iOS 15 beta上导航条断开的问题。

#### 修改错误
* 已经修改了因已同意条款，pushconfiguration未能返回为nil的问题。

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* PAYCO iOS SDK升级(1.5.0)
    * 以前如果不能使用PAYCO应用程序，则只能手动登录。但目前若已登录到Safari，则可使用“简单登录”功能。

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### 改善/修改功能
* Display Language功能已被改善。
    * 为了添加语言集，以前必须直接修改Gamebase.bundle里的文件。
        * 但经过功能改善，将localizedstring.json文件添加到Xcode项目的Copy Bundle Resources中就可添加语言。
    * 在Display Language语言集中添加了汉语简体(zh-CN)、汉语繁体(zh-TW)及泰国语(th)。
    * 默认语言代码为**en**，但经过改善已可适用Gamebase控制台中设置的默认语言。
        * [Game > Gamebase > 控制台使用指南 > 应用程序 > App > 语言设置](./oper-app/#language-settings)
* 调用showTermsView API后，可创建PushConfiguration对象的基准被更改为；
    * 修改前
        * 仅当在条款项目中存在**接收Push**项目时，才返还有效的PushConfiguration，而不返还nil。
        * 如果用户拒绝在白天和夜间接收广告性Push，PushConfiguration.pushEnabled则会创建为false。
    * 修改后
        * 如果显示了条款UI，将返还有效的PushConfiguration，而不始终返还nil。
        * showTermsView返还的PushConfiguration对象的pushEnabled值始终为true。
    * 修改错误
        * 若因已同意条款，未显示条款UI时，PushConfiguration将会返还为nil。

#### 修改错误
* 已修改因终端机的语言代码和Push控制台中的语言代码不一致，而导致未能正常设置Push语言的问题。

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
    * (Unity) Fixed an issue that caused OutOfMemoryException on retry in WebSocket
* [SDK] 2.19.1
	* (Android) Fixed a crash when logging in with a different IdP after trying to log in to Weibo IdP

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
    * (Common) Supports the download feature when a Customer Center attachment image is clicked
    * (Common) TOAST SDK update: Android(0.23.2), Unity(0.21.2)
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
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / [JavaScript](./js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / [JavaScript](./js-etc/#level-up-trace)]
