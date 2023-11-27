## Game > Gamebase > iOS SDK 사용 가이드 > 시작하기

## Environments


> [INFO]
>
> 최소 사양
>
> * 사용자 실행 환경: iOS 11 이상
> * 빌드 환경: Xcode 14.1(iOS 16.1 SDK) 이상
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 일부 IdP 지원 시 **하단 3rd Party Gamebase Auth Adapters 표 안의 Support iOS Version 항목을 참고**하세요.
> AppStore 출시 시에는 반드시 Apple 버전 정책에 준수해야 합니다.
>
> * https://developer.apple.com/ios/submit/
>

## Setting

Gamebase는 아래와 같은 방법으로 설정이 가능합니다.

### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.xcframework 및 필요한 Adapter들을 다운로드합니다.<br/>
또한 각 IdP의 인증을 위한 SDK 파일들을 다운로드해야 합니다. 해당 IdP의 로그인을 사용할 때만 포함하면 됩니다.<br/>
다운로드한 뒤, 해당 SDK 파일을 프로젝트의 target에 포함시켜야 합니다.

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 용도  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.xcframework<br/>Gamebase.bundle | NHNCloudSDK 1.6.2 | Gamebase의 Interface 및 핵심 로직을 포함 | iOS 11 or later |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.xcframework | FacebookSDK 14.1.0 | Facebook 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthPaycoAdapter.xcframework | PaycoID Login 3rd SDK v1.5.9 | PAYCO 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthNaverAdapter.xcframework | naveridlogin-sdk-ios-4.1.1 | NAVER 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthGamecenterAdapter.xcframework | GameKit.framework | Gamecenter 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthGoogleAdapter.xcframework | GoogleSignIn 7.0.0 | Google 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthTwitterAdapter.xcframework | | Twitter 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthLineAdapter.xcframework | LineSDK 5.8.2 | LINE 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthAppleidAdapter.xcframework |  | Sign In with Apple | iOS 11 or later<br/>arm64 지원<br/> |
|  | GamebaseAuthHangameAdapter.xcframework | HangameID SDK 1.8.6 | Hangame 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthWeiboAdapter.xcframework | weibo_ios_sdk-3.3.4 | Weibo 로그인을 지원 | iOS 11 or later |
|  | GamebaseAuthKakaogameAdapter.xcframework | KakaoGame 3.17.5 | Kakao 로그인을 지원 | iOS 11 or later |
| Gamebase IAP Adapters | GamebasePurchaseIAPAdapter.xcframework | StoreKit.framework<br/>NHNCloudIAP 1.6.2 | 게임 내 결제 지원 | iOS 11 or later |
| Gamebase Push Adapters | GamebasePushAdapter.xcframework | NHNCloudPush 1.6.2 | Push를 지원 | iOS 11 or later |


> <font color="red">[주의]</font><br/>
>
> Sign In with Apple에 필요한 AuthenticationServices.framework를 추가할 경우에는 반드시 Optional로 설정해야 합니다.
> Required로 설정할 경우에는 iOS 11 이하 기기에서는 실행 직후 크래시가 발생합니다.
> 
> Gamebase SDK iOS 2.13.0부터 iOS 9 이상에서 Sign In with Apple이 지원되며, 추가로 Gamebase Console에 Service ID를 설정해야 합니다.

<br/>

> <font color="red">[주의]</font><br/>
>
> Gamebase Framework 파일 중 이름에 **Adapter**가 포함되어 있는 파일들은 선택해 프로젝트 내에서 사용 여부를 결정할 수 있으며, 사용하지 않는 Adapter Framework는 제거하는 것을 권장합니다.
> 해당 Adapter Framework를 사용하려면 위의 표에 명시된 외부 SDK들이 필요할 수 있습니다.
> 일부 인증 Adapter의 경우 위의 표에 있는 지원하는 iOS 버전에 유의해야 합니다.
> (지원 버전이 iOS 11 이상인 Auth Adpater를 빌드에 포함하면 iOS 10 이하에서는 런타임 크래시가 발생합니다.)

<br/>

> [INFO]
> 
> 각 IdP에서 제공하는 외부 SDK에 대한 설정은 각 IdP의 가이드 문서를 참고하시길 바랍니다.
>

### Xcode Settings

압축을 풀면 다음과 같이 Gamebase.xcframework 등의 SDK를 볼 수 있습니다.

![unzip gamebase](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_2.54.0.png)


* 1) Framework 파일을 Project의 Project Navigator로 끌어와서 import합니다. 이 때 추가된 Framework 파일들은 프로젝트 target에 추가되어야 합니다. 
* 2) **Gamebase.bundle** 파일도 **Copy Bundle Resources**에 추가합니다.
![Gamebase.bundle Bundle Resources](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) Gamebase의 Framework 외에 Gamebase에서 사용하고 있는 외부 SDK들의 기능을 포함하기 위한 여러 Framework와 Library 파일을 Linker에서 참조할 수 있도록 아래 항목을 추가해야 합니다.
    * libicucore.tbd
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * Security.framework
    * AuthenticationServices.framework (Optional)
    * AppTrackingTransparency.framework (Optional)

![Link Binary With Libraries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)

* 4) **Gamebase iOS SDK 2.12.0 이상**을 사용할 경우 Facebook SDK가 업데이트 됨에 따라 추가 설정이 필요합니다.
    * **Accelerate.framework** 추가
    * 프로젝트 내부에 **빈 swift 파일** 추가 (프로젝트 내부에 swift 파일이 하나도 없을 경우)
* 5) **Target > Build Settings > Linking > Other Linker Flags**에 **-ObjC**를 추가해야 합니다.
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) NaverAuthAdapter를 사용하는 경우에는 NAVER SDK에서 제공하는 **NaverThirdPartyLogin.framework** 파일을 **Target > Build Phases > Embeded Frameworks**에 추가해야 합니다.
 ![Naver Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.8.0.png)
 * 7) LineAuthAdapter를 사용하는 경우에는 LINE SDK에서 제공하는 **LineSDK.xcframework** 파일을 **Target > Build Phases > Embeded Frameworks**에 추가해야 합니다.
 ![LINE Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.9.1.png)

> [INFO]
>
> Linker에 **-ObjC**옵션 설정은 Static Library에 있는 모든 Objective-C class와 category를 로드합니다. <br/>
> 따라서 이 옵션을 설정하지 않았을 때에 **selector not recognized**와 같은 오류가 Runtime 상에서 발생할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> * Unity(2019.3 이상) 빌드할 경우, Gamebase iOS SDK를 **UnityFramework** 타겟에만 import 해줍니다.
> * Unity 빌드를 하게 되면 Xcode 프로젝트 타겟에 **Unity-iPhone**, **UnityFramework**가 생깁니다.
> * 각 타겟에 Gamebase iOS SDK를 중복해서 import 할 경우, 동작에 문제가 있을 수 있으니 유의해야 합니다.
> 

### CocoaPods Settings

Gamebase iOS SDK는 CocoaPods를 통해서도 설정할 수 있습니다.

* 1) Xcode를 실행해 프로젝트를 생성합니다.
* 2) Terminal을 실행해 CocoaPods을 적용하려는 프로젝트의 디렉터리로 이동합니다.
* 3) **pod init** 명령어를 실행해 **Podfile**을 생성합니다.
* 4) 생성된 **Podfile**을 편집기로 열어 다음과 같은 내용을 작성합니다.

```ruby
platform :ios, '11.0'

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

    # 다음 모듈의 사용 방법은 고객 센터로 문의하시기 바랍니다.
    pod 'GamebaseAuthHangameAdapter'
    pod 'GamebaseAuthKakaogameAdapter'
end
```

> [INFO]
>
> **target 'SampleApplication' do** 부분에는 생성한 프로젝트의 타깃 이름을 입력합니다.<br/>
> **pod 'Gamebase', '2.48.0'**과 같이 작성해 특정 버전을 지정할 수 있습니다. 각각의 pod에 버전을 명시하지 않으면 최신 버전이 설정됩니다.<br/>
> 특정 Adapter만 선택해서 적용할 수 있습니다.
> 

<br/>

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

### IdP Settings

> <font color="red">[주의]</font><br/>
>
> * NHN Cloud Console에서 새 프로젝트를 생성하여 Gamebase 서비스를 활성화했는지 반드시 확인하세요.
> * 각 IdP 콘솔에서 Client ID를 발급 받아 Gamebase 콘솔에 입력했는지 반드시 확인하세요.

* 인증을 위해 IdP 콘솔에서 Client ID를 발급 받아 Gamebase 콘솔에 입력합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Authentication Information](./oper-app/#authentication-information)
* Gamebase iOS SDK는 각 IdP별로 추가 설정이 필요합니다.

#### Facebook

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **fb{Facebook 개발자 사이트에 등록한 앱의 App ID}**를 추가해야 합니다.
* Info.plist 파일에 Scheme을 등록합니다.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>fbapi</string>
    <string>fb-messenger-share-api</string>
</array>
```

#### Google

* URL Scheme을 설정해야 합니다.
    * **Google Cloud Platform > APIs & Services > Credentials**에서 발급 받은 iOS URL scheme을 **Xcode > Target > Info > URL Types**에 추가해야 합니다.
* Gamebase iOS SDK 2.34.1 이하는 추가 설정이 필요합니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### PAYCO

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.payco**를 추가해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **paycologinsdk**를 추가해야 합니다.

#### NAVER

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.naver**를 추가해야 합니다.
    * **NAVER Developers > 내 애플리케이션 > API 설정 > iOS > URL Scheme**에 **tcgb.{Bundle ID}.naver**를 추가해야 합니다.
* Info.plist 파일에서 Scheme을 등록합니다.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>naversearchthirdlogin</string>
    <string>naversearchapp</string>
</array>
```
* Gamebase iOS SDK 1.12.1 이하는 추가 설정이 필요합니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Twitter

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.
* Twitter 의 Developer 사이트의 Apps > 대상 프로젝트 > App Details > Callback URL 항목을 설정해야 합니다.
    *  **tcgb.{Bundle ID}.twitter://** 를 추가합니다.

#### LINE

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **line3rdp.{Bundle ID}**를 추가해야 합니다.

* ATS 설정을 위해서 Info.plist 파일에 Scheme을 등록합니다.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lineauth2</string>
</array>
```
* Gamebase iOS SDK 2.42.2 이하는 추가 설정이 필요합니다.
    * [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Weibo

* URL Scheme을 설정해야 합니다.
    * **Xcode > Target > Info > URL Types**에 **wb{Weibo 개발자 사이트에 등록한 앱의 App Key}**를 추가해야 합니다.
* Info.plist 파일에서 Scheme을 등록합니다.
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

* Gamebase iOS SDK 2.34.1 이하
    * URL Scheme을 설정해야 합니다.
        * **Xcode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.google**를 추가해야 합니다.
* Gamebase iOS SDK 1.12.1 이하
    * **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
        * Google 의 경우, iOS 앱에서 필요한 정보 **url_scheme_ios_only**의 설정이 필요합니다.
        * **url_scheme_ios_only**의 값은 Xcode의 URL Scheme에 등록된 값들 중 한개와 일치해야 합니다.
    * URL Scheme을 설정해야 합니다.
        * **Xcode > Target > Info > URL Types**
* Google 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Scheme" }
```

![gamebase_auth_google_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_google_console_01.png)


**NAVER**

* Gamebase iOS SDK 1.12.1 이하
	* **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야 합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.
		* iOS 앱에서 필요한 정보인 **url_scheme_ios_only**를 추가로 설정해야 합니다.
	* URL Scheme 를 설정해야 합니다.
		* **Xcode > Target > Info > URL Types**
* NAVER 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Scheme", "service_name": "Your Service Name" }
```

![gamebase_auth_naver_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_naver_console_01.png)

**LINE**

* Gamebase iOS SDK 2.42.2 이하
    * LINE에서 발급 받은 ChannelID를 Info.plist 파일에 설정해야 합니다.

```
<key>LineSDKConfig</key>
<dict>
    <key>ChannelID</key>
    <string>{Issued LINE ChannleID}</string>
</dict>
```

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/ios)
* [NAVER for developers](https://developers.naver.com/docs/login/ios/ios.md)
* [Twitter Developer's guide - Log in with Twitter](https://developer.twitter.com/en/docs/authentication/guides/log-in-with-twitter)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [LINE for developers](https://developers.line.biz/en/docs/line-login-sdks/ios-sdk/swift/overview/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_ios_sdk/blob/3.3.4/iOS%E5%B9%B3%E5%8F%B0SDK%E6%96%87%E6%A1%A3V3.3.4.pdf)
* [Google Sign-In for iOS](https://developers.google.com/identity/sign-in/ios)
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

## Sample App

* [https://github.com/nhn/toast.gamebase.ios.sample](https://github.com/nhn/toast.gamebase.ios.sample)

## API Reference

API Reference는 SDK 내에 포함되어 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX: Major
		* YY: Minor
		* ZZ: Hotfix

* 최소 5개월 경과
