## Game > Gamebase > iOS Developer's Guide > Getting Started

### Environments


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

### Installation

Gamebase can be setup as below.

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Download Gamebase.framework.zip and required adapters.<br/>
Also download SDK files to authenticate each IdP, which are required only for a login.<br/>
Then, include corresponding SDK files to a target of your project.

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage | External SDK Download Link | Support iOS Version |
| --- | --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.27.1 | Includes the interface and key logic of Gamebase | Included in Gamebase | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.1.0 | Supports Facebook login | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | Supports Payco login | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Supports Naver login | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Supports Game Center login |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Supports Google login | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Supports Twitter login | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | Supports LINE login | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | Set AuthenticationServices.framework to Optional | iOS13 or later |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.5.1 | Supports Hangame login | | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | Supports Weibo login |[LINK \[Go to Download\]](https://github.com/sinaweibosdk/weibo_ios_sdk/archive/3.2.7.zip) | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework, ToastIAP 0.27.1, ToastGamebaseIAP 0.11.0 | Supports in-game purchase| Included in Gamebase IAP | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.27.1 | Supports Push | Included in Gamebase Push | iOS9 or later |


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

#### Xcode Settings

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
* 6) Go to **Target > Build Settings > Enable Bitcode** and select **No**.
![Enable Bitcode](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 7) When using the NaverAuthAdapter, **NaverThirdPartyLogin.framework** file provided by NaverSDK should be added to **Target > General > Embedded Binaries**.
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> The **-ObjC option** to the Linker allows all Objective-C classes and categories of Static Library to be loaded. <br/>
> Therefore, if this option is not set, an error like **selector not recognized** may occur during runtime.
>

#### CocoaPods Settings

You can set the Gamebase iOS SDK with CocoaPods.

1. Execute Xcode to create a project.
2. Execute the terminal to navigate to the directory of the project where CocoaPods will be applied.
3. Execute the **pod init** command to create **Podfile**.
4. Open the created **Podfile** with the editor and enter the following.

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

5. After completing writing the Podfile, execute **pod install** or **pod update** command to install Gamebase.
6. After installation is complete, the file **project_name.xcworkspace** is created. From now on, use the created **xcworkspace** file for development.
7. **Set Target > Build Settings > Enable Bitcode** to **No**.
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> [Note]
>
> For more detailed information on how to use CocoaPods, see [Using CocoaPods](https://guides.cocoapods.org/using/index.html) at [CocoaPods Guide](https://guides.cocoapods.org/).
>
>

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
