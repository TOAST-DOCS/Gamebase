## Game > Gamebase > iOS SDK 사용 가이드 > 시작하기

### Environments


> [INFO]
>
> 최소 사양
>
> * 사용자 실행 환경 : iOS 9 이상
> * 빌드 환경 : Xcode 12 (iOS 14 SDK) 이상
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 일부 IdP 지원 시 **하단 3rd Party Gamebase Auth Adapters 표 안의 Support iOS Version 항목을 참고**하세요.
> AppStore 출시 시에는 반드시 Apple 버전 정책에 준수해야 합니다.
>
> * https://developer.apple.com/ios/submit/
>

### Installation

Gamebase는 아래와 같은 방법으로 설정이 가능합니다.

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip 및 필요한 adapter 들을 다운로드 받습니다.<br/>
또한 각 IdP의 인증을 하기위한 SDK파일들을 다운로드 받아야합니다. 해당 IdP의 로그인을 사용할 때만 포함하면 됩니다.<br/>
다운로드한 뒤, 해당 SDK파일을 프로젝트의 target에 포함시켜야 합니다.

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 용도  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.27.1 | Gamebase의 Interface 및 핵심 로직을 포함 | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.1.0 | Facebook 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | Payco 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Naver 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenter 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Google 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitter 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | LINE 로그인을 지원 | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | iOS9 or later<br/>arm64 지원<br/> |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.5.1 | Hangame 로그인을 지원 | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | Weibo 로그인을 지원 | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework<br/>ToastIAP 0.27.1<br/> ToastGamebaseIAP 0.11.0 | 게임 내 결제를 지원 | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.27.1 | Push를 지원 | iOS9 or later |


> <font color="red">[주의]</font><br/>
>
> Sign In with Apple에 필요한 AuthenticationServices.framework를 추가할 경우에는 반드시 Optional로 설정해야 합니다.
> Required로 설정할 경우에는 iOS 11 이하 기기에서는 실행 직후 크래시가 발생합니다.
> 
> Gamebase SDK iOS 2.13.0 이상에서는 iOS 9 이상에서 Sign In with Apple 이 지원되며 추가로 Gamebase Console 에 Service ID를 설정해야합니다.

<br/>

> <font color="red">[주의]</font><br/>
>
> Gamebase Framework 파일 중 이름에 **Adapter**가 포함되어 있는 파일들은 선택해 프로젝트 내에서 사용 여부를 결정할 수 있으며, 사용하지 않는 Adapter Framework는 제거하는 것을 권장합니다.
> 해당 Adapter Framework를 사용하려면 위의 표에 명시된 외부 SDK들이 필요할 수 있습니다.
> 일부 인증 Adapter의 경우 위의 표에 있는 지원하는 iOS 버전에 유의해야 합니다.
> (지원 버전이 iOS 10 이상인 Auth Adpater를 빌드에 포함하면 iOS 9 이하에서는 런타임 크래시가 발생합니다.)

<br/>

> [INFO]
> 
> 각 IdP에서 제공하는 외부 SDK에 대한 설정은 각 IdP의 가이드 문서를 참고하시길 바랍니다.
>

#### Xcode Settings

압축을 풀면, 다음과 같이 Gamebase.framework 등의 SDK를 볼 수 있습니다.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) Framework 파일을 Project의 Project Navigator로 끌어와서 import합니다. 이 때 추가된 Framework 파일들은 프로젝트 target에 추가되어야 합니다. 
* 2) **Gamebase.bundle** 파일도 **Copy Bundle Resources** 에 추가합니다.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) Gamebase를 사용하기 위해서는 Gamebase의 framework외에, Gamebase에서 사용하고 있는 외부 SDK들의 기능을 포함하기 위하여, 여러 framework와 library 파일을 linker에서 참조할 수 있도록 추가해야합니다. 아래 항목들을 추가해야합니다.
    * libicucore.tbd
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * AuthenicationServices.framework (Optional)
    * AppTrackingTransparency.framework (Optional)

![Link Binary With Libraries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)

* 4) **Gamebase iOS SDK 2.12.0 이상**을 사용할 경우 Facebook SDK가 업데이트 됨에 따라 추가 설정이 필요합니다.
    * **Accelerate.framework** 추가
    * 프로젝트 내부에 **빈 swift 파일** 추가 (프로젝트 내부에 swift 파일이 하나도 없을 경우)
* 5) **Target > Build Settings > Linking > Other Linker Flags**에 **-ObjC**를 추가해야 합니다.
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) NaverAuthAdapter를 사용하는 경우에는 NaverSDK에서 제공하는 **NaverThirdPartyLogin.framework** 파일을 **Target > General > Embedded Binaries**에 추가해야 합니다.
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linker에 **-ObjC**옵션 설정은 Static Library에 있는 모든 Objective-C class와 category를 로드합니다. <br/>
> 따라서 이 옵션을 설정하지 않았을 때에 **selector not recognized**와 같은 오류가 Runtime 상에서 발생할 수 있습니다.
>

#### CocoaPods Settings

Gamebase iOS SDK는 CocoaPods를 통해서도 설정할 수 있습니다.

* 1) Xcode를 실행해 프로젝트를 생성합니다.
* 2) Terminal을 실행해 CocoaPods을 적용하려는 프로젝트의 디렉터리로 이동합니다.
* 3) **pod init** 명령어를 실행해 **Podfile**을 생성합니다.
* 4) 생성된 **Podfile**을 편집기로 열어 다음과 같은 내용을 작성합니다.

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
> **target 'SampleApplication' do** 부분에는 생성한 프로젝트의 타깃 이름을 입력합니다.<br/>
> **pod 'Gamebase', '2.6.0'**과 같이 작성해 특정 버전을 지정할 수 있습니다. 각각의 pod에 버전을 명시하지 않으면 최신 버전이 설정됩니다.<br/>
> 특정 Adapter만 선택해서 적용할 수 있습니다.
> 



> <font color="red">[주의]</font><br/>
>
> Gamebase 최신 버전을 사용하지 않으면 일부 Adapter의 사용이 불가능 할 수 있습니다.
>

* 5) Podfile 작성이 완료되면 **pod install** 또는 **pod update** 명령어를 실행해 Gamebase를 설치합니다.
* 6) 설치가 완료되면 **프로젝트명.xcworkspace** 파일이 생성됩니다. 이후부터는 생성된 **xcworkspace** 파일을 통해 개발을 진행합니다.


> [INFO]
>
> 더 자세한 CocoaPods 사용법에 대해서는 [CocoaPods Guide](https://guides.cocoapods.org/)의 [Using CocoaPods](https://guides.cocoapods.org/using/index.html) 페이지를 참고하시길 바랍니다.
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

API Reference는 SDK 내에 포함되어 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과
