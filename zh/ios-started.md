## Game > Gamebase > iOS SDK使用指南 > 开始

## Environments


> [INFO]
>
> 最低规格
>
> * 用户执行环境 : iOS 9以上
> * 打包环境 : Xcode 12 (iOS 14 SDK)以上
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 支持部分IdP时，请参考**以下3rd Party Gamebase Auth Adapters列表中的Support iOS Version项目**。
> 推出AppStore时，必须遵守Apple版本政策。 
>
> * https://developer.apple.com/ios/submit/
>

## Setting

Gamebase可以通过以下方式安装。

### Download

* [下载Gamebase iOS SDK](/Download/#game-gamebase)

下载Gamebase.framework和必要的Adapter。<br/>
并且需要下载进行各IdP认证的SDK文件。仅在使用相关IdP的登录时添加即可。<br/>
下载后，需要将SDK文件添加在项目的target当中。

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 用途 | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework<br/>Gamebase.bundle | ToastSDK 0.28.0 | 包含Gamebase的Interface和核心逻辑。| iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.2.0 | 支持Facebook登录。| iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | 支持Payco登录。| iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | 支持Naver登录。| iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | 支持Gamecenter登录。| iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | 支持Google登录。| iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | 支持Twitter登录。| iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | 支持LINE登录。| iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | iOS9 or later<br/>支持arm64。<br/> |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.6.0 | 支持Hangame登录。| iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | 支持Weibo登录。| iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework<br/>ToastIAP 0.28.0<br/> ToastGamebaseIAP 0.12.0 | 支持游戏内支付。| iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.28.0 | 支持Push。| iOS9 or later |


> <font color="red">[注意]</font><br/>
>
> 添加Sign In with Apple需要的AuthenticationServices.framework时必须设置为Optional。
> 如果设置为Required，在iOS 11以下设备运行时将会发生崩溃。
> 
> Gamebase SDK iOS 2.13.0支持iOS 9以上Sign In with Apple，需要在Gamebase Console中输入Service ID。

<br>

> <font color="red">[注意]</font><br/>
>
> 从Gamebase Framework文件当中选择文件名包括**Adapter**的文件后, 在项目内决定是否使用，若存在不使用的Adapter Framework，推荐删除。
> 为了使用Adapter Framework，可能需要上述表中显示的外部SDK。
> 对于部分验证Adpater，应注意上表中的Support iOS Version。
> (如果支持版本为iOS 10以上的Auth Adpater被包括在build中，在iOS 9以下设备运行时会发生崩溃。)

<br/>

> [INFO]
> 
> 有关每个IdP提供的外部SDK的设置，请参考IdP指南。
>

### Xcode Settings

解压后，显示如下Gamebase.framework等SDK文件。

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)

* 1) 将Framework文件拖到Project的Project Navigator中并导入。添加的Framework文件应添加到项目target中。
* 2) 将**Gamebase.bundle**文件添加到**Copy Bundle Resources**中。
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) 为了使用Gamebase，除了Gamebase的framework之外，还需包括Gamebase使用的各种外部SDK功能, 为此需要添加framework和library文件，通过linker进行参考。请添加以下的几个项目。
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

* 4) 使用**Gamebase iOS SDK 2.12.0以上**版本时，更新Facebook SDK的同时要添加设置。 
    * 添加**Accelerate.framework**。
    * 在项目内添加**空swift文件**(在项目内没有swift文件时添加)。
* 5) 需要在**Target > Build Settings > Linking > Other Linker Flags**中添加**-ObjC**。
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) 使用NaverAuthAdapter时，需要将NaverSDK提供的**NaverThirdPartyLogin.framework**文件添加在**Target > General > Embedded Binaries**中。
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> 链接器上的**-ObjC**选项设置会将加载Static Library中的所有Objective-C class和category。
> 因此，如果未设置此选项，则在Runtime会出现**selector not recognized**错误。
>

### CocoaPods Settings

Gamebase iOS SDK可以通过CocoaPods来设置。

* 1) 运行Xcode以创建项目。
* 2) 运行Terminal并转到要应用CocoaPods的项目目录。
* 3) 执行**pod init** 命令，创建**Podfile**。
* 4) 使用编辑器打开已创建的**Podfile**并编写以下内容。

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

> [INFO]
>
> 在**target 'SampleApplication' do**部分中输入创建项目的target名称。
> **按照pod ”Gamebase”，”2.6.0”**编制，可以指定特定版本。若各pod未列出版本，则设置为最新版。
> 只能选择特定Adapter适用。 
> 



> <font color="red">[注意]</font><br/>
>
> 如果您不使用最新版本的Gamebase，一部分Adapter可能无法使用。
>

* 5) 创建Podfile后，通过执行**pod install**或**pod update**命令来设置Gamebase。
* 6) 设置完后**项目名.xcworkspace**的文件将被创建。创建之后使用**xcworkspace**文件进行开发。 


> [INFO]
>
> 详细的CocoaPods使用方法，请参考[CocoaPods指南](https://guides.cocoapods.org/)中的[使用CocoaPods](https://guides.cocoapods.org/using/index.html)页面。 
> 
>

### IdP Settings

> <font color="red">[注意]</font><br/>
>
> * 在NHN Cloud Console中生成新项目后，必须确认Gamebase服务是否已被启用。 
> * 从各IdP控制台中获取Client ID后，请确认是否已在Gamebase控制台中输入。 

* 为了进行认证，在IdP控制台中获取Client ID，将其在Gamebase控制台中输入。 
    * [Game > Gamebase > 控制台使用指南 > 应用程序 > Authentication Information](./oper-app/#authentication-information)
*  

#### Google

* 需要设置URL Scheme。
    * 在**Xcode > Target > Info > URL Types**中添加**tcgb.{Bundle ID}.google**。

* Gamebase iOS SDK 1.12.1以下版本需要额外的设置。 
    * [Game > Gamebase > iOS SDK使用指南 > 开始 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Payco

* 需要设置URL Scheme。
    * 在**Xcode > Target > Info > URL Types**中添加**tcgb.{Bundle ID}.payco**。

#### Naver

* 需要设置URL Scheme。
    * 在**Xcode > Target > Info > URL Types**中添加**tcgb.{Bundle ID}.naver**。

* Gamebase iOS SDK 1.12.1以下版本需要额外的设置。 
    * [Game > Gamebase > iOS SDK使用指南 > 开始 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Twitter

* 需要设置URL Scheme。
    * 在**Xcode > Target > Info > URL Types**中添加**tcgb.{Bundle ID}.twitter**。
* 需要设置Twitter的Developer网页的Apps > 对象项目 > App Details > Callback URL项目。
    * 添加**tcgb.{Bundle ID}.twitter://**。

#### Line

* 需要设置URL Scheme。
	* 在**Xcode > Target > Info > URL Types**中添加**line3rdp.{App Bundle ID}**。

* 需要设置Info.plist文件。
	* 需要设置从LINE获取的ChannelID。
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* 为了ATS设置，需要注册Scheme。 
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
* 关于使用LINE Login时所需的项目设置，请参考以下链接。(需要验证)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)

### IdP Settings (Legacy)

**Google**

* Gamebase iOS SDK 1.12.1以下
    * 要在**NHN Cloud Console > Gamebase > App > 认证信息 > 附加信息 & Callback URL**的**附加信息**项目中设定JSON string格式的信息。 
        * 使用Google时，需要在iOS应用程序中设置**url_scheme_ios_only**必要信息。  
        * **url_scheme_ios_only**的值要与在Xcode的URL Scheme中注册的值中的一个相一致。 
    * 需要设置URL Scheme。
        * **Xcode > Target > Info > URL Types**
* 输入Google附加认证信息的示例 

```json
{ "url_scheme_ios_only": "Your URL Scheme" }
```

![gamebase_auth_google_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_google_console_01.png)


**Naver**

* Gamebase iOS SDK 1.12.1以下
	* 要在**NHN Cloud Console > Gamebase > App > 认证信息 > 附加信息 & Callback URL**的**附加信息**项目中设定JSON String格式的信息。
		* 使用NAVER时，需要在同意登录的窗口设置**service_name**（应用程序名称）。
		* iOS应用程序需要额外的设定。要添加**url_scheme_ios_only**必要信息。
	* 需要设置URL Scheme。 
		* **Xcode > Target > Info > URL Types**
* 输入Naver附加认证信息的示例 

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

API Reference已被包含在SDK中。

## API Deprecate Governance

将对Gamebase不再支持的API，进行Deprecate处理。
在满足以下条件时，可以删除Deprecated的API，不再另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 至少5个月
