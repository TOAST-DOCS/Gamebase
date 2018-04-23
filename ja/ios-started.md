## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

### Environments


> [INFO]
>
> 最小仕様:iOS8以上 <br/>
> arm7, arm7s, arm64, i386, x86_64に対応している端末<br/>
> Xcode7以上
>


### Installation

Gamebaseは、次のような方法で設定できます。

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | 用途 | External SDK Download Link |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle |  | GamebaseのInterface及びメインロジックを含む |  |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | Facebook ログインに対応 | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | Payco ログインに対応 | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.9 | Naverログインに対応 | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenterログインに対応 |  |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | ゲーム内課金に対応 | Gamebase IAP内に含む |
| Gamebase Push | GamebasePushAdapter.framework |  | Pushに対応 | Gamebase内に含む |



> <font color="red">[注意]</font><br/>
>
> Gamebase Frameworkファイルのうち、名前に**Adapter**が含まれているファイルは、プロジェクト内で使用するかどうかを選択・決定することができ、該当するAdapter Frameworkを使用するためには、上の表に明記されている外部SDKが必要になることがあります。
> GamebaseNaverAuthNaverAdapter.frameworkは、iOS 9以上でのみ対応します。

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
 -![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>




## API Reference

SDKの中に含まれています。

