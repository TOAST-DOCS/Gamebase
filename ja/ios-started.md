## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

### Environments


> [INFO]
>
> 要件
>
> * ユーザー実行環境：iOS 9以上
> * ビルド環境：Xcode 12 (iOS 14 SDK)以上
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 一部のIdPサポートをする時は**下段3rd Party Gamebase Auth Adapters表内のSupport iOS Version項目を参考**にしてください。
> AppStoreでリリースする時には、必ずAppleバージョンポリシーを遵守する必要があります。
>
> * https://developer.apple.com/ios/submit/
>

### Installation

Gamebaseは、次のような方法で設定できます。

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage | External SDK Download Link | Support iOS Version |
| --- | --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.27.1 | GamebaseのInterfaceおよびコアロジックを含む | Gamebase内に含む | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.1.0 | Facebookログインをサポート | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.4.0 | Paycoログインをサポート | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Naverログインをサポート | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenterログインをサポート |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Googleログインをサポート | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitterログインをサポート | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | LINEログインをサポート | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | AuthenticationServices.frameworkをOptionalに設定 | iOS13 or later |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.5.1 | Hangameログインをサポート | | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | Weiboログインをサポート |[LINK \[Go to Download\]](https://github.com/sinaweibosdk/weibo_ios_sdk/archive/3.2.7.zip) | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework, ToastIAP 0.27.1, ToastGamebaseIAP 0.11.0 | ゲーム内決済をサポート | Gamebase IAP内に含まれる | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.27.1 | Pushをサポート | Gamebase Push内に含まれる | iOS9 or later |


> <font color="red">[注意]</font><br/>
>
> Sign In with Appleに必要なAuthenticationServices.frameworkを追加する場合は必ずOptionalに設定する必要があります。
> Requiredに設定すると、iOS 11以下の端末では実行直後にクラッシュが発生します。
> 
> Gamebase SDK iOS 2.13.0以上では、iOS 9以上でSign In with Appleがサポートされ、追加でGamebase ConsoleにService IDを設定する必要があります。

<br/>

> <font color="red">[Caution]</font><br/>
>
> Gamebase Frameworkファイルのうち、名前に**Adapter**が含まれているファイルは、選択してプロジェクト内で使用有無を決定することができ、使用しないAdapter Frameworkは削除することを推奨します。
> 該当Adapter Frameworkを使用するには、上の表に明示された外部SDKが必要な場合があります。
> 一部の認証Adapterの場合は、上の表にあるサポートするiOSバージョンに注意する必要があります。
> (サポートバージョンがiOS 10以上のAuth Adpaterをビルドに含めると、iOS 9以下ではランタイムクラッシュが発生します。)

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
* 3) Gamebaseを使用するには、Gamebaseのframeworkの他に、Gamebaseで使用している外部SDKの機能を含めるために、複数のframeworkとlibraryファイルをlinkerから参照できるように追加する必要があります。以下の項目を追加する必要があります。
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

* 4) **Gamebase iOS SDK 2.12.0以上**を使用する場合、Facebook SDKがアップデートされたことに伴い、追加設定が必要です。
    * **Accelerate.framework**追加
    * プロジェクト内部に**空のswiftファイル**追加(プロジェクト内部にswiftファイルが1つもない場合)
* 5) **Target > Build Settings > Linking > Other Linker Flags**に**-ObjC**を追加する必要があります。
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) **Target > Build Settings > Enable Bitcode**を **No**に設定します。
![Enable Bitcode](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 7) NaverAuthAdapterを使用する場合には、NaverSDKで提供する **NaverThirdPartyLogin.framework**ファイルを**Target > General > Embedded Binaries**に追加する必要があります。
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>

#### CocoaPods Settings

Gamebase iOS SDKは、CocoaPodsを使用して設定できます。

1. Xcodeを実行し、プロジェクトを作成します。
2. Terminalを実行し、CocoaPodsを適用するプロジェクトのディレクトリに移動します。
3. **pod init**コマンドを実行し、**Podfile**を作成します。
4. 作成された**Podfile**をエディタで開き、次のような内容を作成します。

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

> [参考]
>
> **target 'SampleApplication' do**部分には作成したプロジェクトのターゲット名を入力します。<br/>
> **pod 'Gamebase', '2.6.0'**のように入力して、特定バージョンを指定できます。それぞれのpodにバージョンを明示しない場合は最新バージョンが設定されます。<br/>
> 特定Adapterのみを任意で適用できます。
>



> <font color="red">[注意]</font><br/>
>
> Gamebase最新バージョンを使用しない場合、一部のAdapterを使用できないことがあります。
>

5. Podfileの作成が完了したら、**pod install**または**pod update**コマンドを実行してGamebaseをインストールします。
6. インストールが完了したら、**プロジェクト名.xcworkspace**ファイルが作成されます。以降は作成された**xcworkspace**ファイルを使用して開発を行います。
7. **Target > Build Settings > Enable Bitcode**を**No**に設定します。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> [参考]
>
> 詳細なCocoaPods使用方法は、[CocoaPods Guide](https://guides.cocoapods.org/)の[Using CocoaPods](https://guides.cocoapods.org/using/index.html)ページを参照してください。
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

SDKの中に含まれています。

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebase Version Format - XX.YY.ZZ
		* XX：Major
		* YY：Minor
		* ZZ：Hotfix

* 最低5ヶ月経過
