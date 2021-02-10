## Game > Gamebase > iOS SDK使用指南 > 开始

### Environments


> [INFO]
>
> 最低规格：iOS9以上或支持部分IDP时iOS10以上<br/>
> iOS, iOS Simulator
> Xcode10以上可创建
>


### Installation

Gamebase可以通过以下方式安装。

#### 下载

* [下载Gamebase iOS SDK](/Download/#game-gamebase)

下载Gamebase.framework.zip和所需的adapter。<br/>
为了各IdP认证，需下载相应的SDK文件，仅在该IdP登录使用时包含就可以。<br/>
下载后，必须在项目target中打包相应的SDK文件。

**第三方SDK下载**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 目的 | External SDK Download Link | Support iOS Version |
| --- | --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle | NHN CloudSDK 0.19.3 | 包含Gamebase的Interface及核心逻辑 | 包含于Gamebase内 | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v5.6.0 | 支持Facebook登录 | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.3.2 | 支持Payco登录 | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | 支持Naver登录 | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | 支持Gamecenter登录 |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | 支持Google登录 | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | 支持Twitter登录 | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | 支持LINE登录 | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | AuthenticationServices.framework设置为Optional | iOS13 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework, NHN CloudIAP 0.19.8, NHN CloudGamebaseIAP 0.9.7 | 支持游戏内支付 | 包含于Gamebase IAP内 | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | NHN CloudPush 0.19.3 | 支持Push | 包含于Gamebase Push内 | iOS9 or later |


> <font color="red">[注意]</font><br/>
>
> 添加Sign In with Apple需要的AuthenticationServices.framework时必须设置为Optional。
> 设置为Required时，iOS 11以下设备运行后会发生崩溃。
> 
<br/>


> <font color="red">[注意]</font><br/>
>
> Gamebase Framework文件中名称中含有**Adapter**的文件可决定是否选择性地在项目中使用，为使用相应Adapter Framework可能需要上表中列出的外部SDK。
> 对于部分验证Adpater，应注意上表中的Support iOS Version。
>（Build中包含支持版本为iOS10以上的Auth Adpater时，iOS9以下发生runtime Crash。）

<br/>


> [INFO]
> 
> 有关每个IdP提供的外部SDK的设置，请参考IdP指南。
>

#### Xcode设定

解压后，显示如下Gamebase.framework等SDK文件。

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) 将Framework文件拖到Project的Project Navigator中并导入。添加的Framework文件应添加到项目target中。
* 2) 将**Gamebase.bundle** 文件添加到 **Copy Bundle Resources**中。
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) 为了使用Gamebase，除了Gamebase的framework之外，您还需要添加几个framework和library 文件，以便 linker可以引用集成了Gamebase中使用的外部SDK的功能， 您需要添加以下项目。
    * libicucore.tbd (在Gamebase SDK v1.1.5或更高版本中添加)
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * AuthenicationServices.framework (Optional)
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) **Target > Build Settings > Linking > Other Linker Flags**中需要添加**-ObjC**。
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 5) 将**Target > Build Settings > Enable Bitcode**设置为**No**。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 6) 如果要使用NaverAuthAdapter，则需要将 NaverSDK提供的 **NaverThirdPartyLogin.framework**文件添加到**Target > General > Embedded Binaries**中。
 ![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> 链接器上的 **-ObjC**选项设置会将加载Static Library中的所有Objective-C class和 category。<br/>
> 因此，如果未设置此选项，则在Runtime会出现 **selector not recognized**错误。
>

#### CocoaPods设定

Gamebase iOS SDK可以通过CocoaPods来设置。

* 1) 运行Xcode以创建项目。
* 2) 运行Terminal并转到要应用CocoaPods的项目目录。
* 3) 执行**pod init** 命令，创建 **Podfile**。
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
    pod 'GamebasePushAdapter'
    pod 'GamebasePurchaseIAPAdapter'

end
```

> [INFO]
>
> 在**target ‘SampleApplication’ do**部分输入创建的项目的目标名。<br/>
> 按照**pod ‘Gamebase’, ‘2.6.0’**编制，可指定特定版本。若各pod未列出版本，则设置为最新版。<br/>
> 仅可选择性应用特定Adapter。
>
> <font color="red">[注意]</font><br/>
>
> 如果您不使用最新版本的Gamebase，则可能无法使用某些Adapter。
>

* 5) 完成Podfile的创建后，执行命令**pod install**或**pod update**来安装Gamebase。
* 6) 安装完成后，将创建**项目名称.xcworkspace** 文件。之后通过创建的 **xcworkspace**继续进行开发。
* 7) 将Target > Build Settings > Enable Bitcode设置为No。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> [INFO]
>
> 详细的CocoaPods使用方法，请参考[CocoaPods指南](https://guides.cocoapods.org/)中的[使用CocoaPods](https://guides.cocoapods.org/using/index.html)页面。 
>
>

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/ios)
* [Naver for developers](https://developers.naver.com/docs/login/ios/)
* [Twitter Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth)
* [Line for developers](https://developers.line.me/en/docs/line-login/ios/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)

## API Reference

API Reference包含在SDK中。

## API Deprecate Governance

Gamebase不再支持的API，进行Deprecate处理。
在满足以下条件时，可以删除Deprecated的API，不再另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 至少5个月
