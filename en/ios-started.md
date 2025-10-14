## Game > Gamebase > iOS Developer's Guide > Getting Started

## Environments


> [INFO]
>
> Minimum specifications
>
> * User run environment : iOS 12 or later, Swift 5 or later
> * Build environment : Xcode 16 (iOS 18 SDK) or later
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

Download Gamebase.xcframework.zip and required adapters.<br/>
Also download SDK files to authenticate each IdP, which are required only for a login.<br/>
Then, include corresponding SDK files to a target of your project.

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External SDK & Compatible Version | Usage  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.xcframework<br/>Gamebase.bundle | NHNCloudSDK 1.8.5 | Includes the interface and key logic of Gamebase | iOS 12 or later |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.xcframework | FacebookSDK 18.0.0 | Supports Facebook login | iOS 12 or later |
|  | GamebaseAuthPaycoAdapter.xcframework | PaycoID Login 3rd SDK v1.5.13 | Supports PAYCO login | iOS 12 or later |
|  | GamebaseAuthNaverAdapter.xcframework | naveridlogin-sdk-ios-4.2.3 | Supports NAVER login | iOS 12 or later |
|  | GamebaseAuthGamecenterAdapter.xcframework | GameKit.framework | Supports Game Center login | iOS 12 or later |
|  | GamebaseAuthGoogleAdapter.xcframework | GoogleSignIn 7.0.0 | Supports Google login | iOS 12 or later |
|  | GamebaseAuthTwitterAdapter.xcframework | | Supports Twitter login | iOS 12 or later |
|  | GamebaseAuthLineAdapter.xcframework | LineSDK 5.11.2 | Supports LINE login  | iOS 13 or later |
|  | GamebaseAuthAppleidAdapter.xcframework |  | Sign In with Apple | iOS 12 or later |
|  | GamebaseAuthHangameAdapter.xcframework | HangameID SDK 1.17.2 | Supports Hangame login | iOS 12 or later |
|  | GamebaseAuthWeiboAdapter.xcframework | weibo_ios_sdk-3.4.0 | Supports Weibo login | iOS 12 or later |
|  | GamebaseAuthKakaogameAdapter.xcframework | KakaoGame 3.20.0 | Supports Kakao login | iOS 13 or later<br/>Xcode 16.2 이상 |
|  | GamebaseAuthSteamAdapter.xcframework |  | Supports Steam login | iOS 14 or later |
| Gamebase IAP Adapters | GamebasePurchaseIAPAdapter.xcframework | StoreKit<br/>NHNCloudIAP 1.8.5 | Supports in-game purchase | iOS 12 or later |
| Gamebase Push Adapters | GamebasePushAdapter.xcframework | NHNCloudPush 1.8.5 | Supports Push | iOS 12 or later |


> <font color="red">[Caution]</font><br/>
> 
> Gamebase SDK iOS 2.13.0 or later supports Sign In with Apple in iOS 9 or later, and additionally the Service ID needs to be set in the Gamebase Console.

<br/>

> <font color="red">[Caution]</font><br/>
>
> The Gamebase Framework files that contain **Adapter** in their name can be selected to determine whether they will be used within the project or not. It is recommended to remove any unused Adapter Frameworks.
> To use these Adapter Frameworks, the external SDKs specified in the above table might be required.
> For some Auth Adapters, be aware of the supported iOS versions listed in the table above.
> (If an Auth Adapter that supports iOS version 12 or later is included in the build, it will crash on iOS 11 or earlier.)

<br/>

> [INFO]
> 
> For setting of external SDKs which each IdP provides, refer to each IdP guide.
>

### Xcode Settings

By decompression, following SDKs will show, including Gamebase.xcframework.

![unzip gamebase](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_2.54.0.png)


* 1) Drag framework files to Project Navigator of the project and import. Note that added framework files should be added to a project target.
* 2) Also add the **Gamebase.bundle** file to **Copy Bundle Resources**.
![Gamebase.bundle Bundle Resources](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) To use Gamebase, a number of frameworks and library files must be added so that they can be referenced by the linker. This is to include the features of the external SDKs used by Gamebase in addition to the framework of Gamebase. You need to add the following:
    * libicucore.tbd
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * Security.framework
    * AuthenticationServices.framework
    * AppTrackingTransparency.framework (Optional)

![Link Binary With Libraries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)

* 4) When using **Gamebase iOS SDK 2.12.0 or later**, additional settings are required as Facebook SDK is updated.
    * Adding **Accelerate.framework**
    * Add an **empty swift file** within the project (When there are not any swift files within the project)
* 5) Go to **Target > Build Settings > Linking > Other Linker Flags** and add **-ObjC**.
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) When using NaverAuthAdapter, the **NaverThirdPartyLogin.xcframework** file provided by NAVER SDK should be added to **Target > Build Phases > Embeded Frameworks**.
 ![Naver Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_2.59.0.png)
 * 7) When using LineAuthAdapter, the **LineSDK.xcframework** file provided by LINE SDK should be added to **Target > Build Phases > Embeded Frameworks**.
 ![LINE Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.9.1.png)
 * 8) If you are using the FacebookAuthAdapter, you must add the Facebook SDK to **Target > Build Phases > Embedded Frameworks**.
![Facebook Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_2.65.0-facebook-embeded.png)
* 9) If you are using the PaycoAuthAdapter, you must add the PAYCO SDK to **Target > Build Phases > Embeded Frameworks**.
![PAYCO Embeded Frameworks](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/iOSDevelopersGuide/started-xcode-settings-payco-embeded-20241025.png)

> [INFO]
>
> The **-ObjC option** to the Linker allows all Objective-C classes and categories of Static Library to be loaded. <br/>
> Therefore, if this option is not set, an error like **selector not recognized** may occur during runtime.
>

<br/>

> <font color="red">[Caution]</font><br/>
>
> * When building with Unity (2019.3 or later), import the Gamebase iOS SDK only to the **UnityFramework** target.
> * When you run Unity build, **Unity-iPhone** and **UnityFramework** are created in the Xcode project target.
> * Note that there may be problems with operation if you import the Gamebase iOS SDK in duplicate for each target.
> 

### CocoaPods Settings

You can set the Gamebase iOS SDK with CocoaPods.

* 1) Execute Xcode to create a project.
* 2) Execute the terminal to navigate to the directory of the project where CocoaPods will be applied.
* 3) Execute the **pod init** command to create **Podfile**.
* 4) Open the created **Podfile** with the editor and enter the following.

```ruby
platform :ios, '12.0'

target 'SampleApplication' do
    use_frameworks!
    
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
    pod 'GamebaseAuthSteamAdapter'
    pod 'GamebasePushAdapter'
    pod 'GamebasePurchaseIAPAdapter'

    # Regarding how to use the following modules, please contact the Customer Center.
    pod 'GamebaseAuthHangameAdapter'
    pod 'GamebaseAuthKakaogameAdapter'
end
```

> [Note]
>
> Enter the target name of the created project in the **target 'SampleApplication' do** part.<br/>
> You can specify versions by writing in this way: **pod 'Gamebase', '2.48.0'**. If no version is specified in each pod, the newest version is used.<br/>
> Only some specific Adapters can be selected and applied.
> 

<br/>

> <font color="red">[Caution]</font><br/>
>
> If you do not use the latest Gamebase version, some adapter may not be available.
>

* 5) Install Gamebase using the **pod install** or **pod update** command after Podfile is written.
* 6) A **[project name].xcworkspace** file will be created when the installation is complete. From this point on, product is developed using the created **xcworkspace** file.


> [Note]
>
> For more detailed information on how to use CocoaPods, see [Using CocoaPods](https://guides.cocoapods.org/using/index.html) at [CocoaPods Guide](https://guides.cocoapods.org/).
>
>

### IdP Settings

> <font color="red">[Caution]</font><br/>
>
> * Make sure that Gamebase service is enabled by creating a new project from NHN Cloud Console.
> * Make sure that Client ID is issued by each IdP console and the IDs are entered in the Gamebase console.

* For verification, get Client ID from the IdP and enter it in the Gamebase console.
    * [Game > Gamebase > Console User Guide > App > Authentication Information](./oper-app/#authentication-information)
* Gamebase iOS SDK requires an additional, separate setting per IdP.

#### Facebook

* URL Scheme must be configured.
    * Go to **Xcode > Target > Info > URL Types** and add **fb{App ID of the app you registered on the Facebook developers' site}**
* Register the Scheme in the Info.plist file.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>fbapi</string>
    <string>fb-messenger-share-api</string>
</array>
```

* Register FacebookAppID, FacebookClientToken, and FacebookDisplayName to the Info.plist file.
```
<key>FacebookAppID</key>
<string>{FACEBOOK_APP_ID}</string>
<key>FacebookClientToken</key>
<string>{FACEBOOK_CLIENT_TOKEN}</string>
<key>FacebookDisplayName</key>
<string>{FACEBOOK_DISPLAY_NAME}</string>
```

#### Google

* URL Scheme must be configured.
    * Add the iOS URL scheme obtained from **Google Cloud Platform > APIs & Services > Credentials** to **Xcode > Target > Info > URL Types**.
* An additional setting is required for Gamebase iOS SDK 2.34.1 or earlier versions.
    * [Game > Gamebase > iOS SDK User Guide > Getting Started > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### PAYCO

* URL Scheme must be configured.
    * Go to **Xcode > Target > Info > URL Types** and add **tcgb.{Bundle ID}.payco**
    * Go to **Xcode > Target > Info > URL Types** and add **paycologinsdk**.

#### NAVER

* URL Scheme must be configured.
    * Go to **Xcode > Target > Info > URL Types** and add **tcgb.{Bundle ID}.naver**
    * In **NAVER Developers > My Application > API Settings > iOS > URL Scheme**, add **tcgb.{Bundle ID}.naver**.
* Register the Scheme in the Info.plist file.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>naversearchthirdlogin</string>
    <string>naversearchapp</string>
</array>
```
* An additional setting is required for Gamebase iOS SDK 1.12.1 or earlier versions.
    * [Game > Gamebase > iOS SDK User Guide > Getting Started > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### LINE

* URL Scheme must be configured.
	* Go to **Xcode > Target > Info > URL Types** and add **line3rdp.{Bundle ID}**

* For ATS setting, register the Scheme in the Info.plist file.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lineauth2</string>
</array>
```
* For Gamebase iOS SDK 2.42.2 or earlier, additional settings are required.
    * [Game > Gamebase > iOS SDK User Guide > Getting Started > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Weibo

* URL Scheme must be configured.
    * Go to **Xcode > Target > Info > URL Types** and add **wb{App Key for apps registered on the Weibo developer site}**.
* Register the scheme in the Info.plist file.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>sinaweibo</string>
    <string>weibosdk</string>
    <string>weibosdk2.5</string>
    <string>weibosdk3.3</string>
</array>
```

### IdP Settings (Legacy)

**Google**

* Gamebase iOS SDK 2.34.1 or earlier
    * URL Scheme must be configured.
        * Add **tcgb.{Bundle ID}.google** in **Xcode > Target > Info > URL Types**.
* Gamebase iOS SDK 1.12.1 or earlier
    * You need to provide JSON string-type data in the **Additional Info** field in **NHN Cloud Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
        * For Google, you need to set **url_scheme_ios_only** required for iOS apps.
        * The value of **url_scheme_ios_only** must match one of the values registered for the URL Scheme of Xcode.
    * URL Scheme must be configured.
        * **Xcode > Target > Info > URL Types**
* Example of entering the additional authentication information for Google

```json
{ "url_scheme_ios_only": "Your URL Scheme" }
```

![gamebase_auth_google_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_google_console_01.png)


**NAVER**

* Gamebase iOS SDK 1.12.1 or earlier
	* You need to provide JSON String-type data in the **Additional Info** field in **NHN Cloud Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
		* For NAVER, **service_name**, which is the app name to be displayed on the Agree to Login window, needs to be configured.
		* **url_scheme_ios_only**, information needed by iOS apps, needs to be configured as well.
	* URL Scheme must be configured.
		* **Xcode > Target > Info > URL Types**
* Example of entering the additional authentication information for NAVER

```json
{ "url_scheme_ios_only": "Your URL Scheme", "service_name": "Your Service Name" }
```

![gamebase_auth_naver_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_naver_console_01.png)

**LINE**

* Gamebase iOS SDK 2.42.2 or earlier
    * You need to provide the ChannelID issued by LINE in the Info.Plist file.

```
<key>LineSDKConfig</key>
<dict>
    <key>ChannelID</key>
    <string>{Issued LINE ChannleID}</string>
</dict>
```
#### Twitter

* Gamebase iOS SDK 2.66.3 or below
    * URL Scheme must be configured.
        * Go to **Xcode > Target > Info > URL Types** and add **tcgb.{Bundle ID}.twitter**
    * Need to configure Apps > Target Project > App Details > Callback URL on the Developer site of Twitter.
        *  Add **tcgb.{Bundle ID}.twitter://**.


## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/ios)
* [NAVER for developers](https://developers.naver.com/docs/login/ios/ios.md)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [LINE for developers](https://developers.line.biz/en/docs/line-login-sdks/ios-sdk/swift/overview/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_ios_sdk/blob/3.3.4/iOS%E5%B9%B3%E5%8F%B0SDK%E6%96%87%E6%A1%A3V3.3.4.pdf)
* [Google Sign-In for iOS](https://developers.google.com/identity/sign-in/ios)
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

## Sample App

* [https://github.com/nhn/toast.gamebase.ios.sample](https://github.com/nhn/toast.gamebase.ios.sample)

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
