## Game > Gamebase > iOS Developer's Guide > Getting Started

## Environments


> [INFO]
>
> Minimum specifications
>
> * User run environment : iOS 9 or later
> * Build environment : Xcode 12 (iOS 14 SDK) or later
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> If some IdPs are supported, **see the Support iOS Version field in the 3rd Party Gamebase Auth Adapters table** shown below.
> When releasing the AppStore, Apple version policy must be complied with.
>
> * https://developer.apple.com/ios/submit/
>

## Setting

Gamebase can be setup as below.

### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Download Gamebase.framework.zip and required adapters.<br/>
Also download SDK files to authenticate each IdP, which are required only for a login.<br/>
Then, include corresponding SDK files to a target of your project.

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework<br/>Gamebase.bundle | ToastSDK 0.27.2 | Includes the interface and key logic of Gamebase | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.2.0 | Supports Facebook login | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | Supports Payco login | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Supports Naver login | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Supports Game Center login | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Supports Google login | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Supports Twitter login | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | Supports Line login | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | iOS9 or later<br/>arm64 지원<br/> |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.6.0 | Supports Hangame login | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | Supports Weibo login | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework<br/>ToastIAP 0.27.2<br/> ToastGamebaseIAP 0.11.0 | Supports in-game purchase | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.27.2 | Supports Push | iOS9 or later |


> <font color="red">[Caution]</font><br/>
>
> When adding the AuthenticationServices.framework required for Sign In with Apple, it must be set to Optional.
> If it is set to Required, the devices running on iOS version 11 or earlier crashes immediately after launching the app.
> 
> Gamebase SDK iOS 2.13.0 or later supports Sign In with Apple in iOS 9 or later, and additionally the Service ID needs to be set in the Gamebase Console.

<br/>

> <font color="red">[Caution]</font><br/>
>
> The Gamebase Framework files that contain **Adapter** in their name can be selected to determine whether they will be used within the project or not. It is recommended to remove any unused Adapter Frameworks.
> To use these Adapter Frameworks, the external SDKs specified in the above table might be required.
> For some Auth Adapters, be aware of the supported iOS versions listed in the table above.
> (If an Auth Adapter that supports iOS version 10 or later is included in the build, it will crash on iOS 9 or earlier.)

<br/>

> [INFO]
> 
> For setting of external SDKs which each IdP provides, refer to each IdP guide.
>

### Xcode Settings

By decompression, following SDKs will show, including Gamebase.framework.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) Drag framework files to Project Navigator of the project and import. Note that added framework files should be added to a project target.
* 2) Also add the **Gamebase.bundle** file to **Copy Bundle Resources**.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) To use Gamebase, a number of frameworks and library files must be added so that they can be referenced by the linker. This is to include the features of the external SDKs used by Gamebase in addition to the framework of Gamebase. You need to add the following:
    * libicucore.tbd
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * AuthenticationServices.framework (Optional)
    * AppTrackingTransparency.framework (Optional)

![Link Binary With Libraries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)

* 4) When using **Gamebase iOS SDK 2.12.0 or later**, additional settings are required as Facebook SDK is updated.
    * Adding **Accelerate.framework**
    * Add an **empty swift file** within the project (When there are not any swift files within the project)
* 5) Go to **Target > Build Settings > Linking > Other Linker Flags** and add **-ObjC**.
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) NaverAuthAdapter를 사용하는 경우에는 NaverSDK에서 제공하는 **NaverThirdPartyLogin.framework** 파일을 **Target > General > Embedded Binaries**에 추가해야 합니다.
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> The **-ObjC option** to the Linker allows all Objective-C classes and categories of Static Library to be loaded. <br/>
> Therefore, if this option is not set, an error like **selector not recognized** may occur during runtime.
>

### CocoaPods Settings

You can set the Gamebase iOS SDK with CocoaPods.

* 1) Execute Xcode to create a project.
* 2) Execute the terminal to navigate to the directory of the project where CocoaPods will be applied.
* 3) Execute the **pod init** command to create **Podfile**.
* 4) Open the created **Podfile** with the editor and enter the following.

```ruby
platform :ios, '10.0'

target 'SampleApplication' do
    pod 'Gamebase'
    pod 'GamebaseAuthFacebookAdapter'
    pod 'GamebaseAuthGamecenterAdapter'
    pod 'GamebaseAuthPaycoAdapter'
    pod 'GamebaseAuthNaverAdapter'
    pod 'GamebaseAuthTwitterAdapter'
    pod 'GamebaseAuthGoogleAdapter'
    pod 'GamebaseAuthLineAdapter'
    pod 'GamebaseAuthAppleidAdapter'
    pod 'GamebaseAuthWeiboAdapter'
    pod 'GamebasePushAdapter'
    pod 'GamebasePurchaseIAPAdapter'

end
```

> [Note]
>
> Enter the target name of the created project in the **target 'SampleApplication' do** part.<br/>
> You can specify versions by writing in this way: **pod 'Gamebase', '2.6.0'**. If no version is specified in each pod, the newest version is used.<br/>
> Only some specific Adapters can be selected and applied.
> 



> <font color="red">[Caution]</font><br/>
>
> If you do not use the latest Gamebase version, some adapter may not be available.
>

* 5) Podfile 작성이 완료되면 **pod install** 또는 **pod update** 명령어를 실행해 Gamebase를 설치합니다.
* 6) 설치가 완료되면 **프로젝트명.xcworkspace** 파일이 생성됩니다. 이후부터는 생성된 **xcworkspace** 파일을 통해 개발을 진행합니다.


> [Note]
>
> For more detailed information on how to use CocoaPods, see [Using CocoaPods](https://guides.cocoapods.org/using/index.html) at [CocoaPods Guide](https://guides.cocoapods.org/).
>
>

### IdP Settings

> <font color="red">[주의]</font><br/>
>
> * NHN Cloud Console 에서 새 프로젝트를 생성하여 Gamebase 서비스를 활성화 하였는지 꼭 확인하세요.
> * 각 IdP 콘솔에서 Client ID 를 발급받아 Gamebase 콘솔에 입력하였는지 꼭 확인하세요.

* 인증을 위해 IdP 콘솔에서 Client ID 를 발급받아 Gamebase 콘솔에 입력합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Authentication Information](./oper-app/#authentication-information)
* Gaembase iOS SDK 는 각 IdP 별로 추가 설정이 필요합니다.

#### Google

* URL Scheme 를 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.google**를 추가해야 합니다.

* Gamebase iOS SDK 1.12.1 이하는 추가 설정이 필요합니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Payco

* URL Scheme 를 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.payco**를 추가해야 합니다.

#### Naver

* URL Scheme 를 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.naver**를 추가해야 합니다.

* Gamebase iOS SDK 1.12.1 이하는 추가 설정이 필요합니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Twitter

* URL Scheme 를 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.
* Twitter 의 Developer 사이트의 Apps > 대상 프로젝트 > App Details > Callback URL 항목을 설정해야 합니다.
    *  **tcgb.{Bundle ID}.twitter://** 를 추가합니다.

#### Line

* URL Scheme 를 설정해야 합니다.
	* **Xcode > Target > Info > URL Types**에 **line3rdp.{App Bundle ID}**를 추가해야 합니다.

* Info.plist 파일을 설정해야합니다.
	* LINE에서 발급받은 ChannelID를 설정합니다.
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS 설정을 위하여 Scheme 를 등록합니다.
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
* LINE Login 을 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다. (인증 필요)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)

### IdP Settings (Legacy)

**Google**

* Gamebase iOS SDK 1.12.1 이하
    * **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
        * Google 의 경우, iOS 앱에서 필요한 정보 **url_scheme_ios_only**의 설정이 필요합니다.
        * **url_scheme_ios_only**의 값은 Xcode의 URL Scheme에 등록된 값들 중 한개와 일치해야 합니다.
    * URL Scheme 를 설정해야 합니다.
        * **Xcode > Target > Info > URL Types**
* Google 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Scheme" }
```

![gamebase_auth_google_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_google_console_01.png)


**Naver**

* Gamebase iOS SDK 1.12.1 이하
	* **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.
		* iOS 앱에서 필요한 정보인 **url_scheme_ios_only**를 추가로 설정해야 합니다.
	* URL Scheme 를 설정해야 합니다.
		* **Xcode > Target > Info > URL Types**
* Naver 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Scheme", "service_name": "Your Service Name" }
```

![gamebase_auth_naver_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_naver_console_01.png)

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/ios)
* [Naver for developers](https://developers.naver.com/docs/login/ios/)
* [Twitter Developer's guide - Log in with Twitter](https://developer.twitter.com/en/docs/basics/authentication/guides/log-in-with-twitter)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/ios-sdk/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_ios_sdk/blob/3.2.7/%E5%BE%AE%E5%8D%9AiOS%E5%B9%B3%E5%8F%B0SDK%E6%96%87%E6%A1%A3V3.2.7.pdf)

## API Reference

Included in SDK.

## API Deprecate Governance

The API which is not supported by Gamebase anymore is processed as deprecated (deprecate).
A (deprecated) API can be deleted without any prior notice when the following conditions are met:

* Minor version updates of five or more times.
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* Time elapse of at least five months
