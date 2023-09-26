## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

## Environments


> [INFO]
>
> 要件
>
> * ユーザー実行環境：iOS 11以上
> * ビルド環境：Xcode 14.1 (iOS 16.1 SDK)以上
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

Gamebase.xcframework及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**Gamebase iOS SDK Components**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage  | Support iOS Version |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.xcframework<br/>Gamebase.bundle | NHNCloudSDK 1.6.2 | GamebaseのInterfaceおよびコアロジックを含む | iOS 11 or later |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.xcframework | FacebookSDK 14.1.0 | Facebookログインをサポート | iOS 11 or later |
|  | GamebaseAuthPaycoAdapter.xcframework | PaycoID Login 3rd SDK v1.5.6 | PAYCOログインをサポート | iOS 11 or later |
|  | GamebaseAuthNaverAdapter.xcframework | naveridlogin-sdk-ios-4.1.1 | NAVERログインをサポート | iOS 11 or later |
|  | GamebaseAuthGamecenterAdapter.xcframework | GameKit.xcframework | Gamecenterログインをサポート | iOS 11 or later |
|  | GamebaseAuthGoogleAdapter.xcframework | GoogleSignIn 7.0.0 | Googleログインをサポート | iOS 11 or later |
|  | GamebaseAuthTwitterAdapter.xcframework | | Twitterログインをサポート | iOS 11 or later |
|  | GamebaseAuthLineAdapter.xcframework | LineSDK 5.8.2 | LINEログインをサポート | iOS 11 or later |
|  | GamebaseAuthAppleidAdapter.xcframework |  | Sign In with Apple | iOS 11 or later<br/>arm64サポート<br/> |
|  | GamebaseAuthHangameAdapter.xcframework | HangameID SDK 1.8.6 | Hangameログインをサポート | iOS 11 or later |
|  | GamebaseAuthWeiboAdapter.xcframework | weibo_ios_sdk-3.3.3 | Weiboログインをサポート | iOS 11 or later |
|  | GamebaseAuthKakaogameAdapter.xcframework | KakaoGame 3.14.14 | Kakaoログインをサポート | iOS 11 or later |
| Gamebase IAP Adapters | GamebasePurchaseIAPAdapter.xcframework | StoreKit.xcframework<br/>NHNCloudIAP 1.6.2 | ゲーム内決済をサポート | iOS 11 or later |
| Gamebase Push Adapters | GamebasePushAdapter.xcframework | NHNCloudPush 1.6.2 | Pushをサポート | iOS 11 or later |


> <font color="red">[注意]</font><br/>
>
> Sign In with Appleに必要なAuthenticationServices.frameworkを追加する場合は必ずOptionalに設定する必要があります。
> Requiredに設定すると、iOS 11以下の端末では実行直後にクラッシュが発生します。
> 
> Gamebase SDK iOS 2.13.0からiOS 11以上でSign In with Appleがサポートされ、追加でGamebase ConsoleにService IDを設定する必要があります。

<br/>

> <font color="red">[Caution]</font><br/>
>
> Gamebase Frameworkファイルのうち、名前に**Adapter**が含まれているファイルは、選択してプロジェクト内で使用有無を決定することができ、使用しないAdapter Frameworkは削除することを推奨します。
> 該当Adapter Frameworkを使用するには、上の表に明示された外部SDKが必要な場合があります。
> 一部の認証Adapterの場合は、上の表にあるサポートするiOSバージョンに注意する必要があります。
> (サポートバージョンがiOS 11以上のAuth Adpaterをビルドに含めると、iOS 10以下ではランタイムクラッシュが発生します。)

<br/>

> [INFO]
> 
> 各IdPが提供する外部SDKに対する設定は、各IdPのガイドドキュメントをご参考ください。
>

### Xcode Settings

解凍すると、次のようにGamebase.xcframeworkなどのSDKを確認することができます。

![unzip gamebase](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) FrameworkファイルをProjectのProject Navigatorにドラッグ・アンド・ドロップでimportします。このときに追加されたFrameworkファイルは、プロジェクトtargetに追加されなければなりません。
* 2) **Gamebase.bundle**ファイルも**Copy Bundle Resources**に追加します。
![Gamebase.bundle Bundle Resources](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) Gamebaseを使用するには、Gamebaseのframeworkの他に、Gamebaseで使用している外部SDKの機能を含めるために、複数のframeworkとlibraryファイルをlinkerから参照できるように追加する必要があります。以下の項目を追加する必要があります。
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

* 4) **Gamebase iOS SDK 2.12.0以上**を使用する場合、Facebook SDKがアップデートされたことに伴い、追加設定が必要です。
    * **Accelerate.framework**追加
    * プロジェクト内部に**空のswiftファイル**追加(プロジェクト内部にswiftファイルが1つもない場合)
* 5) **Target > Build Settings > Linking > Other Linker Flags**に**-ObjC**を追加する必要があります。
![Other Linker Flags](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 6) NaverAuthAdapterを使用する場合にはNAVER SDKで提供する**NaverThirdPartyLogin.framework**ファイルを**Target > Build Phases > Embeded Frameworks**に追加する必要があります。
 ![Naver Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.8.0.png)
 * 7) LineAuthAdapterを使用する場合にはLINE SDKで提供する**LineSDK.xcframework**ファイルを**Target > Build Phases > Embeded Frameworks**に追加する必要があります。
 ![LINE Embeded Frameworks](https://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.9.1.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> * Unity(2019.3以上)をビルドする場合、 Gamebase iOS SDKを**UnityFramework**ターゲットにのみimportします。
> * Unityビルドを行うと、Xcodeプロジェクトのターゲットに**Unity-iPhone**、**UnityFramework**が生じます。
> * 各ターゲットにGamebase iOS SDKを重複してimportすると、動作に問題が生じることがあるため、注意する必要があります。
> 

### CocoaPods Settings

Gamebase iOS SDKは、CocoaPodsを使用して設定できます。

* 1) Xcodeを実行し、プロジェクトを作成します。
* 2) Terminalを実行し、CocoaPodsを適用するプロジェクトのディレクトリに移動します。
* 3) **pod init**コマンドを実行し、**Podfile**を作成します。
* 4) 作成された**Podfile**をエディタで開き、次のような内容を作成します。

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

    # 次のモジュールの使用方法はサポートへお問い合わせください。
    pod 'GamebaseAuthHangameAdapter'
    pod 'GamebaseAuthKakaogameAdapter'
end
```

> [参考]
>
> **target 'SampleApplication' do**部分には作成したプロジェクトのターゲット名を入力します。<br/>
> **pod 'Gamebase', '2.48.0'**のように入力して、特定バージョンを指定できます。それぞれのpodにバージョンを明示しない場合は最新バージョンが設定されます。<br/>
> 特定Adapterのみを任意で適用できます。
>

<br/>

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

#### Facebook

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**fb{Facebook開発者サイトに登録したアプリのApp ID}**を追加する必要があります。
* Info.plistファイルにSchemeを登録します。
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>fbapi</string>
    <string>fb-messenger-share-api</string>
</array>
```

#### Google

* URL Schemeを設定する必要があります。
    * **Google Cloud Platform > APIs & Services > Credentials**で発行されたiOS URL schemeを**Xcode > Target > Info > URL Types**に追加する必要があります。
* Gamebase iOS SDK 2.34.1以下は追加設定が必要です。

    * [Game > Gamebase > iOS SDK使用ガイド > 始める > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### PAYCO

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.payco**を追加する必要があります。
    * **Xcode > Target > Info > URL Types**に**paycologinsdk**を追加する必要があります。

#### NAVER

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.naver**を追加する必要があります。
    * **NAVER Developers > マイアプリケーション > API設定 > iOS > URL Scheme**に**tcgb.{Bundle ID}.naver**を追加する必要があります。
* Info.plistファイルでSchemeを登録します。
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>naversearchthirdlogin</string>
    <string>naversearchapp</string>
</array>
```
* Gamebase iOS SDK 1.12.1以下は追加設定が必要です。
    * [Game > Gamebase > iOS SDK使用ガイド > 始める > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

#### Twitter

* URL Schemeを設定する必要があります。
    * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.twitter**を追加する必要があります。
* TwitterのDeveloperサイトのApps > 対象プロジェクト > App Details > Callback URL項目を設定する必要があります。
    *  **tcgb.{Bundle ID}.twitter://**を追加します。

#### LINE

* URL Schemeを設定する必要があります。
	* **Xcode > Target > Info > URL Types**に**line3rdp.{Bundle ID}**を追加する必要があります。

* ATS設定を行うためにInfo.plistファイルにSchemeを登録します。
```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lineauth2</string>
</array>
```
* Gamebase iOS SDK 2.42.2以下は追加設定が必要です。
    * [Game > Gamebase > iOS SDK使用ガイド > はじめる > IdP settings (Legacy)](./ios-started/#idp-settings-legacy)

### IdP Settings (Legacy)

**Google**

* Gamebase iOS SDK 2.34.1以下
    * URL Schemeを設定する必要があります。
        * **Xcode > Target > Info > URL Types**に**tcgb.{Bundle ID}.google**を追加する必要があります。
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


**NAVER**

* Gamebase iOS SDK 1.12.1以下
	* **NHN Cloud Console > Gamebase > App > 認証情報 > 追加情報& Callback URL**の**追加情報**項目にJSON String形式の情報を設定する必要があります。
		* NAVERの場合、ログイン同意ウィンドウに表示するアプリ名である**service_name**を設定する必要があります。
		* iOSアプリで必要な情報である**url_scheme_ios_only**を追加で設定する必要があります。
	* URL Schemeを設定する必要があります。
		* **Xcode > Target > Info > URL Types**
* NAVER追加認証情報の入力例

```json
{ "url_scheme_ios_only": "Your URL Scheme", "service_name": "Your Service Name" }
```

![gamebase_auth_naver_console_01](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_auth_naver_console_01.png)

**LINE**

* Gamebase iOS SDK 2.42.2以下
    * LINEから発行されたChannelIDをInfo.plistファイルに設定する必要があります。

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
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_ios_sdk/blob/3.3.3/%E5%BE%AE%E5%8D%9AiOS%E5%B9%B3%E5%8F%B0SDK%E6%96%87%E6%A1%A3V3.3.3.pdf)
* [Google Sign-In for iOS](https://developers.google.com/identity/sign-in/ios)
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

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
