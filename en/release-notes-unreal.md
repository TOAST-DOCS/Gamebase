## Game > Gamebase > Release Notes > Unreal

### 2.33.0 (2022.01.25)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* '결제 어뷰징 자동 해제' 기능이 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > GraceBan](./unreal-authentication/#graceban)
    * 결제 어뷰징 자동 해제 기능은 결제 어뷰징 자동 제재로 이용 정지가 되어야 할 사용자가 이용 정지 유예 상태 후 이용 정지가 되도록 합니다.
    * 이용 정지 유예 상태일 경우 설정한 기간 내에 해제 조건을 모두 만족하면 정상플레이가 가능해집니다.
    * 기간 내에 조건을 충족하지 못하면 이용 정지가 됩니다.
* 결제 어뷰징 자동 해제 기능을 사용하는 게임은 로그인 후 항상 AuthToken.member.graceBanInfo API 값을 확인하고, null이 아닌 유효한 GraceBanInfo 객체를 리턴한다면 해당 유저에게 이용 정지 해제 조건, 기간 등을 안내해야 합니다.
    * 이용 정지 유예 상태인 유저의 게임 내 접근 제어는 게임에서 처리하셔야 합니다.
* 강제매핑 시 IdP 로그인을 한번 더 시도해야 하는 불편함을 개선한 새로운 강제매핑 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Add Mapping Forcibly](./unreal-authentication/#add-mapping-forcibly)
* Gamebase.AddMapping() 호출 후 AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) 에러가 발생했을 때, 해당 계정으로 로그인을 할 수 있는 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 인증 > Mapping > Change Login with ForcingMappingTicket](./unreal-authentication/#change-login-with-forcingmappingticket)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::ServerPushAppKickOutMessageReceived** 타입이 추가되었습니다.
    * 이 이벤트의 활용 방법은 다음 문서를 참고하시기 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Server Push](./unreal-etc/#server-push)
* GamebaseEventHandler의 GamebaseEventCategory에 **GamebaseEventCategory::LoggedOut** 타입이 추가되었습니다.
    * Gamebase Access Token이 만료되어 로그인이 필요할 때 동작합니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./unreal-etc/#logged-out)
* 공통약관 창의 설정을 변경할 수 있는 신규 API가 추가되었습니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > UI > Terms > showTermsView](./unreal-ui/#showtermsview)

#### 기능 개선/변경
* 에러코드 추가 및 변경
    * GamebaseErrorCode::UNKNOWN_ERROR 에러에 매핑된 에러코드를 999에서 9999로 변경하였습니다.
    * 에러코드 999에 매핑 시킨 GamebaseErrorCode::SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.
    
#### 플랫폼별 변경 사항
* [Gamebase Android SDK 2.33.0](./release-notes-android/#2330-20220125)
* [Gamebase iOS SDK 2.33.0](./release-notes-ios/#2330-20220125)

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### Bug Fixes
* Fixed a typo of Finnish language on GamebaseDisplayLanguageCode
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### Added Features
* Added a common Terms and Conditions feature
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that stores the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).
* Changed the internal launching URL
* Removed Android multidex-related setting from Gamebase

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
	* [Android Settings Tool](https://docs.toast.com/en/Game/Gamebase/en/unreal-started/#android-settings) provided: please use this settings tool rather than modifying the Gamebase_Android_UPL.xml file.
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
    * (Unreal) Supports PLCrashReporter Issue: [Guide](http://docs.toast.com/en/Game/Gamebase/en/unreal-started/#ios-settings)

#### Feature Updates
* [SDK] 2.9.1
    * (Unreal) Updated Gamebase SDK version for iOS within iOS Plugin (2.9.1)
    * (Unreal) Fixed the missing part of UObject referencing 

### 2.9.0 (May 12, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### More Features
* [SDK] 2.9.0
	* (Unreal) Newly released SDK