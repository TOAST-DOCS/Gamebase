## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

## Environments


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

## Setting

Gamebaseは、次のような方法で設定できます。

### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework<br/>Gamebase.bundle | ToastSDK 0.29.0 | GamebaseのInterfaceおよびコアロジックを含む | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v9.2.0 | Facebookログインをサポート | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.5.0 | Paycoログインをサポート | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.1.1 | Naverログインをサポート | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenterログインをサポート | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Googleログインをサポート | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitterログインをサポート | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | LINEログインをサポート | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | iOS9 or later<br/>arm64サポート<br/> |
|  | GamebaseAuthHangameAdapter.framework | HangameID SDK 1.6.0 | Hangameログインをサポート | iOS9 or later |
|  | GamebaseAuthWeiboAdapter.framework | weibo_ios_sdk-3.2.7 | Weiboログインをサポート | iOS9 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework<br/>ToastIAP 0.29.0<br/> ToastGamebaseIAP 0.12.0 | ゲーム内決済をサポート | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.29.0 | Pushをサポート | iOS9 or later |


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

### Xcode Settings

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
* 6) NaverAuthAdapterを使用する場合にはNaverSDKで提供する**NaverThirdPartyLogin.framework**ファイルを**Target > General > Embedded Binaries**に追加する必要があります。
 ![Naver Embeded Binaries](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>

### CocoaPods Settings

Gamebase iOS SDKは、CocoaPodsを使用して設定できます。

* 1) Xcodeを実行し、プロジェクトを作成します。
* 2) Terminalを実行し、CocoaPodsを適用するプロジェクトのディレクトリに移動します。
* 3) **pod init**コマンドを実行し、**Podfile**を作成します。
* 4) 作成された**Podfile**をエディタで開き、次のような内容を作成します。

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

* 5) Podfileの作成が完了したら**pod install**または**pod update**コマンドを実行してGamebaseをインストールします。
* 6)インストールが完了したら**プロジェクト名.xcworkspace**ファイルが作成されます。その後は作成された**xcworkspace**ファイルを利用して開発を行います。


> [参考]
>
> 詳細なCocoaPods使用方法は、[CocoaPods Guide](https://guides.cocoapods.org/)の[Using CocoaPods](https://guides.cocoapods.org/using/index.html)ページを参照してください。
>
>

### IdP Settings

> <font color="red">[注意]</font><br/>
>
> * NHN Cloud Consoleで新しいプロジェクトを作成してGamebaseサービスが有効になっていることを必ず確認してください。
> * 各IdPのコンソールでClient IDを発行してGamebaseコンソールに入力していることを必ず確認してください。

* 認証のためにIdPコンソールでClient IDを発行してGamebaseコンソールに入力します。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information](./oper-app/#authentication-information)
* Gamebase iOS SDKは、IdPごとに追加設定を行う必要があります。

#### Google

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.google**を追加する必要があります。

* Gamebase iOS SDK 1.12.1以下は追加設定が必要です。
    * [Game > Gamebase > iOS SDK使用ガイド > 始める > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Payco

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.payco**を追加する必要があります。

#### Naver

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.naver**を追加する必要があります。

* Gamebase iOS SDK 1.12.1以下は追加設定が必要です。
    * [Game > Gamebase > iOS SDK使用ガイド > 始める > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Twitter

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.twitter**を追加する必要があります。
* TwitterのDeveloperサイトのApps > 対象プロジェクト > App Details > Callback URL項目を設定する必要があります。
    *  **tcgb.{Bundle ID}.twitter://**を追加します。

#### Line

* URL Schemeを設定する必要があります。
	* **Xcode > Target > Info > URL Types**に**line3rdp.{App Bundle ID}**を追加する必要があります。

* Info.plistファイルを設定する必要があります。
	* LINEで発行されたChannelIDを設定します。
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS設定を行うためにSchemeを登録します。
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
* LINE Loginを使用するためのプロジェクト設定は次のリンクを参照します。(認証必要)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)

### IdP Settings (Legacy)

**Google**

* Gamebase iOS SDK 1.12.1以下
    * **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目にJSON string形式の情報を設定する必要があります。
        * Googleの場合、iOSアプリで必要な情報**url_scheme_ios_only**の設定が必要です。
        * **url_scheme_ios_only**の値はXcodeのURL Schemeに登録された値のいずれか1つと一致する必要があります。
    * URL Schemeを設定する必要があります。
        * **Xcode > Target > Info > URL Types**
* Google追加認証情報の入力例

```json
{ "url_scheme_ios_only": "Your URL Scheme" }
```

![gamebase_auth_google_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_google_console_01.png)


**Naver**

* Gamebase iOS SDK 1.12.1以下
	* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目にJSON String形式の情報を設定する必要があります。
		* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。
		* iOSアプリで必要な情報である**url_scheme_ios_only**を追加で設定する必要があります。
	* URL Schemeを設定する必要があります。
		* **Xcode > Target > Info > URL Types**
* Naver追加認証情報の入力例

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
