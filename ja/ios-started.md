## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

### Environments


> [INFO]
>
> 最小仕様:iOS8以上または一部のIdPに対してはiOS9以上<br/>
> arm7, arm7s, arm64, i386, x86_64に対応している端末<br/>
> Xcode10以上
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
> Gamebase Frameworkファイルのうち、名前に**Adapter**が含まれているファイルは、プロジェクト内で使用するかどうかを選択・決定することができ、該当するAdapter Frameworkを使用するためには、上記の表に明記されている外部SDKが必要になることがあります。
> 一部の認証Adpaterの場合、上記の表にあるSupport iOS Versionに注意する必要があります。
>(サポートバージョンがiOS 9以上のAuth Adpaterをビルドに含めると、iOS 8以下ではランタイムクラッシュが発生します。)

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
    * libc++.tbd
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

Gamebase iOS SDKは、CocoaPodsを使用して設定できます。

1. Xcodeを実行し、プロジェクトを作成します。
2. Terminalを実行し、CocoaPodsを適用するプロジェクトのディレクトリに移動します。
3. **pod init**コマンドを実行し、**Podfile**を作成します。
4. 作成された**Podfile**をエディタで開き、次のような内容を作成します。

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

> [参考]
>
> **target 'SampleApplication' do**部分には作成したプロジェクトの対象名を入力します。<br/>
> **pod 'Gamebase', '1.11.1'**と入力して特定バージョンを指定できます。各々のpodにバージョンを明示しない場合は、最新バージョンが設定されます。<br/>
> 特定Adapterのみを選択的に適用できます。
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
* [Twitter Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth)
* [Line for developers](https://developers.line.me/en/docs/line-login/ios/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)

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
