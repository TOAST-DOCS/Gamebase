## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

### Environments


> [INFO]
>
> 最小仕様:iOS8以上または一部のIdPに対してはiOS9以上<br/>
> arm7, arm7s, arm64, i386, x86_64に対応している端末<br/>
> Xcode9以上
>


### Installation

Gamebaseは、次のような方法で設定できます。

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 用途 | External SDK Download Link | Support iOS Version |
| --- | --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle |  | GamebaseのInterface及びメインロジックを含む |  | iOS8 or later |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | Facebook ログインに対応 | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS8 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | Payco ログインに対応 | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS8 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Naverログインに対応 | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenterログインに対応 |  | iOS8 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Googleログインに対応 | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitterログインに対応 | | iOS8 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v4.1.1 | LINEログインに対応 | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS8 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | ゲーム内課金に対応 | Gamebase IAP内に含む | iOS8 or later |
| Gamebase Push | GamebasePushAdapter.framework |  | Pushに対応 | Gamebase内に含む | iOS8 or later |



> <font color="red">[注意]</font><br/>
>
> Gamebase Frameworkファイルのうち、名前に**Adapter**が含まれているファイルは、プロジェクト内で使用するかどうかを選択・決定することができ、該当するAdapter Frameworkを使用するためには、上の表に明記されている外部SDKが必要になることがあります。
> 일부 인증 Adpater의 경우 위의 표에 있는 Support iOS Version에 유의해야합니다.
> (지원 버전이 iOS9이상인 Auth Adpater를 빌드에 포함 시 iOS8이하에서는 runtime Crash가 발생합니다.)

<br/>


> [INFO]
> 
> 各IdPが提供する外部SDKに対する設定は、各IdPのガイドドキュメントをご参考ください。
>

#### Xcode Settings

解凍すると、次のようにGamebase.frameworkなどのSDKを確認することができます。

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) FrameworkファイルをProjectのProject Navigatorにドラッグ・アンド・ドロップでimportします。このときに追加されたFrameworkファイルは、プロジェクトtargetに追加されなければなりません。
* 2) **Gamebase.bundle**ファイルも**Copy Bundle Resources**に追加します。
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) Gamebaseを使用するためには、Gamebaseのframework以外にGamebaseで使用している外部SDKの機能を含めるため、複数のframeworkとlibraryファイルをlinkerで参照できるように追加しなければなりません。次の項目を追加してください。
    * libicucore.tbd (Gamebase SDK v1.1.5以上で追加)
    * libz.tbd
    * libsqlite3.tbd
    * libstdc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) **Target > Build Settings > Linking > Other Linker Flags**に**-ObjC**を追加する必要があります。
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 5) **Target > Build Settings > Enable Bitcode**を**No**に設定します。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 6) NaverAuthAdapterを使用する場合は、NaverSDKで提供する**NaverThirdPartyLogin.framework**ファイルを**Target > General > Embedded Binaries**に追加する必要があります。
 ![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>

#### CocoaPods Settings

Gamebase iOS SDK는 CocoaPods를 통해서도 설정할 수 있습니다.

* 1) Xcode를 실행해 프로젝트를 생성합니다.
* 2) Terminal을 실행해 CocoaPods을 적용하려는 프로젝트의 디렉터리로 이동합니다.
* 3) **pod init** 명령어를 실행해 **Podfile**을 생성합니다.
* 4) 생성된 **Podfile**을 편집기로 열어 다음과 같은 내용을 작성합니다.

```ruby
platform :ios, '9.0'

target 'SampleApplication' do
    pod 'Gamebase'
    pod 'GamebaseAuthFacebookAdapter'
    pod 'GamebaseAuthGamecenterAdapter'
    pod 'GamebaseAuthPaycoAdapter'
    pod 'GamebaseAuthNaverAdapter'
    pod 'GamebaseAuthTwitterAdapter'
    pod 'GamebaseAuthGoogleAdapter'
    pod 'GamebaseAuthLineAdapter'
    pod 'GamebasePushAdapter'
    pod 'GamebasePurchaseIAPAdapter'
end
```

> [INFO]
>
> **target 'SampleApplication' do** 부분에는 생성한 프로젝트의 타겟명을 입력합니다.<br/>
> **pod 'Gamebase', '1.11.1'** 과 같이 작성해 특정 버전을 지정 할 수 있습니다. 각각의 pod에 버전을 명시하지 않으면 최신 버전이 설정됩니다.<br/>
> 특정 Adapter만 선택적으로 적용할 수 있습니다.
>

> <font color="red">[주의]</font><br/>
>
> Gamebase 최신 버전을 사용하지 않으면 일부 Adapter의 사용이 불가능 할 수 있습니다.
>

* 5) Podfile 작성이 완료되면 **pod install** 또는 **pod update** 명령어를 실행해 Gamebase를 설치합니다.
* 6) 설치가 완료되면 **프로젝트명.xcworkspace** 파일이 생성됩니다. 이후부터는 생성된 **xcworkspace** 파일을 통해 개발을 진행합니다.
* 7) Target > Build Settings > Enable Bitcode를 No로 설정합니다. 
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> [INFO]
>
> 더 자세한 CocoaPods 사용법에 대해서는 [CocoaPods Guide](https://guides.cocoapods.org/)의 [Using CocoaPods](https://guides.cocoapods.org/using/index.html) 페이지를 참고하시길 바랍니다.
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

SDKの中に含まれています。

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과
