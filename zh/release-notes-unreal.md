## Game > Gamebase > Release Notes > Unreal

### 2.49.1 (2023. 04. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* (iOS) 결제 상품 조회 API를 호출 시 크래시가 발생하지 않도록 수정했습니다.

#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.49.0 (2023. 04. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* 미소비 내역 조회 API가 변경되어 신규 API로 변경해야 합니다.
 
        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);
 
* 활성화 구독 조회 API가 변경되어 신규 API로 변경해야 합니다.
    * 기존 API와 동일한 결과를 받으려면 **FGamebasePurchasableConfiguration.allStores**를 **true**로 설정해야 합니다.
 
            // Unity: Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // Unity: New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* (Android) IAP 구독 상태를 조회할 수 있는 RequestSubscriptionsStatus API가 추가되었습니다.
* (Android) 웹뷰에서 고정 폰트 사이즈 사용 여부를 설정하는 필드를 재지원합니다.
    * GamebaseWebViewConfiguration.enableFixedFontSize
* (Android) 웹뷰에서 컷아웃(노치) 영역을 비롯한 모든 이용 가능한 스크린 공간을 사용하여 렌더링할 수 있는 설정이 추가되었습니다.
    * GamebaseWebViewConfiguration.renderOutsideSafeArea

#### 기능 개선/변경
* Unreal의 최소 지원 버전이 4.26으로 변경되었습니다.
* (iOS) Xcode 14.1에서 빌드 시 오류가 발생되는 이슈가 수정되었습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.48.0](./release-notes-android/#2480-2023-03-28)
* [Gamebase iOS SDK 2.49.0](./release-notes-ios/#2490-2023-04-11)

### 2.43.3 (2022. 10. 04.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.3/GamebaseSDK-Unreal.zip)

#### 改善/修改功能
* 修改后当登录LINE时可输入提供服务的Region。
    * [Game > Gamebase > Unreal SDK使用指南 > 认证 > Login with IdP](./unreal-authentication/#login-with-idp)
    
#### 各平台更改项目
* [Gamebase Android SDK 2.43.0](./release-notes-android/#2430-2022-09-07)
* [Gamebase iOS SDK 2.43.3](./release-notes-ios/#2433-2022-10-04)

### 2.42.1 (2022. 08. 09.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Unreal.zip)

#### 添加功能
* 在FGamebaseForcingMappingTicket类中添加了显示映射用户状态的mappedUserValid字段。 
* [iOS设置工具]添加了**Xcode Path**设置，允许您在（./unreal start/#ios设置）指定Xcode的路径。

#### 改善/修改功能
* 在Gamebase控制台中注册Kickout时可以设置是否要显示Kickout弹窗，因此不需要使用以下字段。
    * **FGamebaseConfiguration.enableKickoutPopup**
* 在FGamebaseConfiguration中的一部分字段添加了默认值。
    * enableLaunchingStatusPopup的默认值已设置为true。
    * enableBanPopup的默认值已设置为true。
* 不使用用于设置是否在FWebView中使用固定字体大小的字段。
    * **FGamebaseWebViewConfiguration.enableFixedFontSize**
* 在FGamebaseWebViewConfiguratio内一部分字段添加了默认值。
    *  导航栏的颜色字段colorR、colorG、colorB、colorA的默认值被设置为18、93、230、255。 
    * 指定导航栏是否是启用状态的“isNavigationBarVisible”字段的默认值已设置为true。
    * 指定是否启用Webview内的返回按钮的“isBackButtonVisible”字段的默认值已设置为true。

#### 各平台更改项目
* [Gamebase Android SDK 2.42.1](./release-notes-android/#2421-2022-07-26)
* [Gamebase iOS SDK 2.42.1](./release-notes-ios/#2421-2022-08-09)

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Unreal.zip)

#### 添加功能
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**IdPRevoked**类型。
    * [Game > Gamebase > Unreal SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > IdP Revoked](./unreal-etc/#idp-revoked)

#### 各平台更改项目
* [Gamebase Android SDK 2.41.0](./release-notes-android/#2410-2022-07-05)
* [Gamebase iOS SDK 2.41.0](./release-notes-ios/#2410-2022-07-05)

### 2.40.1 (2022. 06. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.1/GamebaseSDK-Unreal.zip)

#### 修改程序错误
* 修改了可能出现崩溃的逻辑。
* (iOS) 修改了连续调用同样的API时未能传送回调的问题。     

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Unreal.zip)

#### 添加功能
*  提供[iOS设置工具](./unreal-started/#ios-settings)。
    * 在以前的项目设置中被显示为**Gamebase**，而升级后被显示为**Gamebase - Android**, **Gamebase - iOS**。
    * 更改后当提供iOS设置工具打包时只包含必要的框架。
* 添加了可在调用共同条款API后确认是否显示了条款UI的VO类。
    * FGamebaseShowTermsViewResult   
* 添加了可确认终端机是否允许通知的API。
    * IGamebase::Get().GetPush().QueryNotificationAllowed()
* 添加了可确认是否显示条款的API。
    * IGamebase::Get().GetTerms().IsShowingTermsView()
* 添加了可在Webview隐藏导航栏的选项。
    * FGamebaseWebViewConfiguration.isNavigationBarVisible
* 添加了可在(Android)WebView固定字体大小的选项。
    * FGamebaseTermsConfiguration.enableFixedFontSize
* 添加了可在(Android)条款窗固定文字大小的选项。
    * FGamebaseTermsConfiguration.enableFixedFontSize	    
* 添加了当付款时可以确认是否是Promotion的isPromotion字段。 
    * FGamebasePurchasableReceipt.isPromotion
* 添加了当付款时可以确认是否是测试付款的isTestPurchase字段。 
    * FGamebasePurchasableReceipt.isTestPurchase
* 为了在客户服务URL后边添加参数添加了以下字段。
    * FGamebaseContactConfiguration.additionalParameters

#### 改善/修改功能
* 更改后调用API结果的回调时，将转换为GameThread后调用回调。
* 修复了调用RequestActivatedPurchases API时，API在内部被调用两次的问题。
* 更改了一些API的名称。
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData        
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
    
#### 各平台更改项目
* [Gamebase Android SDK 2.40.0](./release-notes-android/#2400-2022-05-24)
* [Gamebase iOS SDK 2.40.0](./release-notes-ios/#2400-2022-05-24)

### 2.33.1 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.1/GamebaseSDK-Unreal.zip)

#### 修改错误
* 修改了iOS打包时出现的错误。

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

#### 添加功能
* 添加了“结算Abusing自动解除”功能。        
    * [Game > Gamebase > Unreal SDK使用指南 > 认证 > GraceBan](./unreal-authentication/#graceban)
    * 结算Abusing自动解除功能是当存在需通过“结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为“预约禁用”状态，在设定的时期内满足解除条件，则可正常玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认AuthToken.member.graceBanInfo API的值，如果返还GraceBanInfo对象，而不返还null，要告知相关用户禁用解除条件、时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。
* 添加了新的强制映射API，以改善进行强制映射时再次尝试IdP登录的不便。
    * [Game > Gamebase > Unreal SDK使用指南 > 认证 > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* 添加了当调用Gamebase.AddMapping()后出现AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)时您可使用相关账户进行登录的API。
    * [Game > Gamebase > Unreal SDK使用指南 > 认证 > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**GamebaseEventCategory::ServerPushAppKickOutMessageReceived**类型。
    * 关于此事件的应用方法，请参考以下文件。
    * [Game > Gamebase > Unreal SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**GamebaseEventCategory::LoggedOut**类型。
    * 当Gamebase Access Token已过期，需要登录时启动。 
    * [Game > Gamebase > Unreal SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* 添加了可更改共同条款窗设置的新API。
    * [Game > Gamebase > Unreal SDK使用指南 > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

#### 改善/修改功能
* 添加或更改错误代码
    * GamebaseErrorCode::UNKNOWN_ERROR的错误代码从999更改为9999。
    * 添加了映射到999错误代码的GamebaseErrorCode::SOCKET_UNKNOWN_ERROR错误。
    
#### 各平台变更项目
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### 修改程序错误
* 修改了GamebaseDisplayLanguageCode芬兰语错误。
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### 添加功能
* 添加共同条款功能
    * 添加打开条款WebView的API。
    * 添加查询“条款列表”和“用户是否同意”API。
    * 添加将”用户是否同意条款”保存在Gamebase服务器的API。

#### 改善/修改功能
* 客户服务类型为TOAST组织服务(Online Contact)时，即使不登录，也显示客户服务。
* 更改内部Launching URL 
* 从Gamebase中删除Android multidex适用。

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