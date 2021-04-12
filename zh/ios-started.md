## Game > Gamebase > iOS SDK使用指南 > 开始

### Environments


> [INFO]
>
>最低规格 : iOS 9以上或支持部分IdP时，**请参考下方3rd Party Gamebase Auth Adapters表中的Support iOS Version项目**。<br/>
> iOS, iOS Simulator<br/>
>Xcode10以上可创建(有关部分IdP专用SDK，请参考以下表。)
>


### Installation

Gamebase可以通过以下方式安装。

#### 下载

* [下载Gamebase iOS SDK](/Download/#game-gamebase)

下载Gamebase.framework.zip和所需的adapter。
为了各IdP认证，需下载相应的SDK文件，仅在该IdP登录使用时包含就可以。
下载后，必须在项目target中打包相应的SDK文件。

**第三方SDK下载**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 目的 | External SDK Download Link | Support iOS Version |
| --- | --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.27.1 | 包括Gamebase的Interface和核心逻辑 | 包括在Gamebase内 | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v7.1.1 | 支持Facebook登录 | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later<br/>&<br/>Xcode 11以上 |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | 支持Payco登录 | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | 支持Naver登录 | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | 支持Gamecenter登录 |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | 支持Google登录 | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | 支持Twitter登录 | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | 支持LINE登录 | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | 将AuthenticationServices.framework设置为Optional | 支持iOS9 or later<br/>arm64<br/> |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.5.1 | 支持Hangame登录 | | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | 支持Weibo登录 |[LINK \[Go to Download\]](https://github.com/sinaweibosdk/weibo_ios_sdk/archive/3.2.7.zip) | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework<br/>ToastIAP 0.27.1<br/> ToastGamebaseIAP 0.11.0 | 支持游戏内付款 | 包括在Gamebase IAP内 | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.27.1 | 支持Push | 包括在Gamebase Push内 | iOS9 or later |

> <font color="red">[注意]</font><br/>
>
> 添加Sign In with Apple需要的AuthenticationServices.framework时必须设置为Optional。
> 设置为Required时，iOS 11以下设备运行后会发生崩溃。
> 
> Gamebase SDK iOS 2.13.0支持iOS 9以上Sign In with Apple，需要在Gamebase Console中输入Service ID。

<br>

> <font color="red">[注意]</font><br/>
>
> 从Gamebase Framework文件当中选择文件名包括**Adapter**的文件后, 在项目内决定是否使用，若存在不使用的Adapter Framework，推荐删除。
> 为了使用Adapter Framework，可能需要上述表中显示的外部SDK。
> 对于部分验证Adpater，应注意上表中的Support iOS Version。
> (如果支持版本为iOS 10以上的Auth Adpater被包括在build，iOS 9以下设备运行后会发生崩溃。)

<br/>

> [INFO]
> 
> 有关每个IdP提供的外部SDK的设置，请参考IdP指南。
>

#### Xcode设定

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
    * AuthenicationServices.framework (Optional)

![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) 使用**Gamebase iOS SDK 2.12.0以上**时，如果Facebook SDK被更新，还需要进行其他设置。 
    * 添加**Accelerate.framework**
    * 在项目内添加**空的swift文件**(在项目内一个swift文件都没有时)。
* 5) 需要在**Target > Build Settings > Linking > Other Linker Flags**中添加**-ObjC**。
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) 将**Target > Build Settings > Enable Bitcode**设置为**No**。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 7) 使用NaverAuthAdapter时，需要将NaverSDK提供的**NaverThirdPartyLogin.framework**文件添加在**Target > General > Embedded Binaries**。
 ![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> 链接器上的 **-ObjC**选项设置会将加载Static Library中的所有Objective-C class和 category。
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
* [Twitter Developer's guide - Log in with Twitter](https://developer.twitter.com/en/docs/basics/authentication/guides/log-in-with-twitter)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/ios-sdk/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_ios_sdk/blob/3.2.7/%E5%BE%AE%E5%8D%9AiOS%E5%B9%B3%E5%8F%B0SDK%E6%96%87%E6%A1%A3V3.2.7.pdf)

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
