## Game > Gamebase > iOS Developer's Guide > Getting Started

### Environments


> [INFO]
>
> Minimum specifications: iOS 9 or later or if some IdP are supported**, see Support iOS Version in the 3rd Party Gamebase Auth Adapters table below: <br/>
> iOS, iOS Simulator
> Can be built on Xcode 10 or later
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
| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.19.3 | Includes the interface and key logic of Gamebase | Included in Gamebase | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v5.6.0 | Supports Facebook login | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | Supports Payco login | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Supports Naver login | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Supports Game Center login |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Supports Google login | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Supports Twitter login | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | Supports LINE login | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | Set AuthenticationServices.framework to Optional | iOS13 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework, ToastIAP 0.19.8, ToastGamebaseIAP 0.9.7 | Supports in-game purchase| Included in Gamebase IAP | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.19.3 | Supports Push | Included in Gamebase Push | iOS9 or later |


> <font color="red">[Caution]</font><br/>
>
> When adding the AuthenticationServices.framework required for Sign In with Apple, it must be set to Optional.
> If it is set to Required, the devices running on iOS version 11 or earlier crashes immediately after launching the app.
> 
<br/>


> <font color="red">[Caution]</font><br/>
>
> Those files that contain **Adapter** in their name among the Gamebase Framework files can be selected and determined whether they can be used in a project. To use the Adapter Framework, some of the SDKs listed in the table above may be needed.
> For some Auth Adapters, be mindful of the supported iOS versions listed in the table above.
> (If an Auth Adapter that supports iOS version 10 or later is included in the build, it crashes on iOS version 9 or earlier.)

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
* 3) To include functions of external SDKs used in Gamebase, other than Gamebase framework, many frameworks and library files should be added to the linker for reference, as below.
    * libicucore.tbd (for Gamebase SDK v1.1.5 or higher)
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * AuthenicationServices.framework (Optional)
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) Add **-ObjC** to **Target > Build Settings > Linking > Other Linker Flags**.
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 5) Set **No** for **Target > Build Settings > Enable Bitcode**.
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 6) For NaverAuthAdapter, add the **NaverThirdPartyLogin.framework** file provided by NaverSDK to **Target > General > Embedded Binaries**.
 ![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

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
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/ios-sdk/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)

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
