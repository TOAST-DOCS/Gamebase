## Game > Gamebase > Release Notes > iOS

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 외부 SDK 업데이트
  * Line iOS SDK (5.8.2)
  * Kakaogame iOS SDK (3.14.4)
  * Hangame iOS SDK (1.7.1)
* Line Login을 수행 시 서비스를 제공할 Region을 입력하도록 변경되었습니다.
* Adpater 버전 불일치시  **TCGB_ERROR_NOT_INITIALIZED(1)** 오류를 리턴하도록 수정하였습니다.   
* SDK 내부 로직 개선

### 2.42.2 (2022. 08. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.2/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 웹뷰에서 사용하는 스킴 목록 중 "itms-services"가 앱 스토어 심사에서 거절되는 경우가 발생하여 제거하였습니다.

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-iOS.zip)

#### 버그 수정
* 대비 증가 옵션을 활성화한 경우 Gamebase 팝업 창이 정상적으로 표시되지 않는 버그를 수정하였습니다.
* SceneDelegate를 사용하는 프로젝트에서 Gamebase 팝업 창이 표시되지 않는 버그를 수정하였습니다.

### 2.42.0 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-iOS.zip)

#### 기능 추가
* 매핑 실패 시 반환되는 ForcingMappingTicket VO 클래스에 유저의 현재 상태를 알 수 있도록 필드가 추가되었습니다.
    * **TCGBForcingMappingTicket.mappedUserValid**
    * mappedUserValid에 저장된 값의 의미는 아래를 참고해주세요.
        * [Game > Gamebase > API 가이드 > API v1.3 가이드 > Others > Mamber Vaild Code](./api-guide/#member-valid-code)

#### 기능 개선/변경
* 외부 SDK 업데이트: Hangame iOS SDK (1.7.0)

#### 버그 수정
* 잘못된 AppID로 Gamebase 초기화를 했을 때 콜백이 호출되지 않는 버그를 수정하였습니다.
* 한게임 로그인 사용자의 경우 GamebaseEventHandler의 **kTCGBIdPRevoked** 이벤트가 오지 않는 버그를 수정하였습니다.

### 2.41.1 (2022. 07. 20.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-iOS.zip)

#### 기능 개선/변경
* 약관 창이 완전히 닫힌 후에 콜백을 호출하도록 수정하였습니다.

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBIdPRevoked**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./ios-etc/#idp-revoked)

#### 改善/修复功能
* 更改了图片通知，使其在显示时根据屏幕方向旋转。

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-iOS.zip)

#### 改善/修复功能
* 改善了SDK内部逻辑

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-iOS.zip)

#### 改善/修复功能
* 外部SDK升级 : Hangame iOS SDK (1.6.4)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-iOS.zip)

#### 改善/修复功能
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

#### 改善/修复功能
* 外部SDK升级 : TOAST iOS SDK(0.30.0), ToastGamebaseIAP SDK(0.13.0), Hangame iOS SDK (1.6.3)

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加了用于确认是否显示条款窗的API。
    * **[TCGBTerms isShowingTermsView]**

#### 改善/修复功能
* 将登录方式从Google网络登录方式更改为Google SDK登录方式。
* 更改后，在登录过程中取消Hangame登录时返回**TCGB_ERROR_AUTH_USER_CANCELED(3001)**错误 。

### 2.34.1 (2022. 03. 15.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.1/GamebaseSDK-iOS.zip)

#### 添加功能
* 为了Swift项目用户，在Public API添加了NS_SWIFT_NAME设置。

#### 改善/修复功能
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

#### 改善/更改功能
* 当调用图片通知API时没有可以显示的图片通知时，背景突然变暗的现象被修改。
* 由于在Gamebase控制台中注册Kickout时可以设置是否显示kickout弹窗，因此以下字段已被deprecated。
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**
    
### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 添加了可更改共同条款窗设置的新API。
    * [Game > Gamebase > iOS SDK使用指南 > UI > Terms > showTermsView](./ios-ui/#showtermsview)

#### 改善/更改功能
* 外部SDK升级 : PAYCO iOS SDK (1.5.5)
* 添加或更改错误代码
    * TCGB_ERROR_UNKNOWN_ERROR的错误代码从999更改为9999。
    * 添加了映射到999错误代码的TCGB_ERROR_SOCKET_UNKNOWN_ERROR错误。 
    
### 2.32.1 (2022.01.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.1/GamebaseSDK-iOS.zip)
 
#### 改善/修复功能 
* 点击推荐更新弹窗里的**立即更新**按钮时弹窗被关闭的问题已修复。
* 提高了SDK稳定性。

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBServerPushAppKickoutMessageReceived**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./ios-etc/#server-push)
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**kTCGBLoggedOut**类型。
    * [Game > Gamebase > iOS SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./ios-etc/#logged-out)

#### 改善/修复功能
* Webview navigationBar的default title Color被更改为**UIColor.whiteColor**。

#### 修改程序错误
* 修改后，现在当调用Hangame注销时，thirdIdP也将注销。

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-iOS.zip)

#### 添加功能
* 可以更改维护弹窗“是否显示维护时间”。

#### 改善/修复功能
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

#### 改善/修复功能
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

#### 改善/修复功能
* PAYCO iOS SDK升级(1.5.2) 

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-iOS.zip)

#### 改善/修复功能
* PAYCO iOS SDK升级(1.5.1)
    * 验证流程和UI的改善
* Hangame iOS SDK升级(1.6.1)
    * 已修改在身份验证阶段发生错误时未能调用回调的问题。
    * 已修改在iOS 15 beta上导航条断开的问题。

#### 修改错误
* 已经修改了因已同意条款，pushconfiguration未能返回为nil的问题。

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-iOS.zip)

#### 改善/修复功能
* PAYCO iOS SDK升级(1.5.0)
    * 以前如果不能使用PAYCO应用程序，则只能手动登录。但目前若已登录到Safari，则可使用“简单登录”功能。

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-iOS.zip)

#### 改善/修复功能
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
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / [JavaScript](./js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / [JavaScript](./js-etc/#level-up-trace)]
    
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
