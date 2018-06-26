## Game > Gamebase > iOS Developer's Guide > Getting Started

### Environments


> [INFO]
>
> Minimum Requirements: iOS8 or 일부 IDP지원 시 iOS9 이상 <br/>
> Supports arm7, arm7s, arm64, i386, x86_64<br/>
> Xcode9 or higher
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
| Gamebase | Gamebase.framework, Gamebase.bundle |  | Includes Gamebase interface and key logics |  | iOS8 or later |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | Supports Facebook logins | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS8 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | Supports Payco logins | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS8 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.9 | Supports Naver logins | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Supports Gamecenter logins |  | iOS8 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Google 로그인을 지원 | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitter 로그인을 지원 | | iOS8 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | Supports in-game purchase | Gamebase Included in IAP | iOS8 or later |
| Gamebase Push | GamebasePushAdapter.framework |  | Supports Push | Included in Gamebase | iOS8 or later |



> <font color="red">[Caution]</font><br/>
>
> Among Gamebase Framework files, **Adapter** files can be selectively used in a project to that end, external SDKs may be required as specified in the above table.
> 일부 인증 Adpater의 경우 위의 표에 있는 Support iOS Version에 유의해야합니다.
> (지원 버전이 iOS9이상인 Auth Adpater를 빌드에 포함 시 iOS8이하에서는 runtime Crash가 발생합니다.)

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
    * libstdc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
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

Gamebase iOS SDK는 CocoaPods를 통해서도 설정할 수 있습니다.

* 1) Xcode를 실행해 프로젝트를 생성합니다.
* 2) Terminal을 실행해 CocoaPods을 적용하려는 프로젝트의 디렉터리로 이동합니다.
* 3) 다음 명령어를 입력해 **Podfile**을 생성합니다.
    ```shell
    $ pod init
    ``` 
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
    * **target 'SampleApplication' do** 부분에는 생성한 프로젝트의 타겟명을 입력합니다.
    * 특정 Adapter만 선택적으로 적용할 수 있습니다.
    * 각각의 pod에 버전을 명시하지 않으면 최신 버전이 설정됩니다.
    * **pod 'Gamebase', '1.11.0'** 과 같이 작성해 특정 버전을 지정 할 수 있습니다.
    > <font color="red">[주의]</font><br/>
    >
    > Gamebase 최신 버전을 사용하지 않으면 일부 Adapter의 사용이 불가능 할 수 있습니다.
    >

* 5) Podfile 작성이 완료되면 다음 명령어를 실행해 Gamebase를 설치합니다.
    ```shell
    $ pod install
    ```
* 6) 설치가 완료되면 **프로젝트명.xcworkspace** 파일이 생성됩니다. 이후부터는 생성된 **xcworkspace** 파일을 통해 개발을 진행합니다.
* 7) Target > Build Settings > Enable Bitcode를 No로 설정합니다. 
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)


> [INFO]
>
> 더 자세한 CocoaPods 사용법에 대해서는 [CocoaPods Guide](https://guides.cocoapods.org/)의 [Using CocoaPods](https://guides.cocoapods.org/using/index.html) 페이지를 참고하시길 바랍니다.
>

## API Reference

Included in SDK.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과
