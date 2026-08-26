<!-- pre-align:aligned sig=d60b1633360c -->

<a id="game-gamebase-upgrade-guide"></a>
## Game > Gamebase > Upgrade Guide { #game-gamebase-upgrade-guide }

<a id="section-1"></a>
## 2.81.4 { #section-1 }

<a id="unity"></a>
### Unity { #unity }

* Gamebase Unity SDKソースにAssembly Definition(.asmdef)が適用され、SDKが基本アセンブリ(Assembly-CSharp)から分離された別の**Gamebase**アセンブリとしてコンパイルされます。
    * GamebaseアセンブリはautoReferencedが有効化されているため、別途Assembly Definitionを使用しないプロジェクトは、追加設定なしで従来と同様にGamebase APIを使用できます。
    * ゲームコードで独自のAssembly Definition(.asmdef)を使用する場合、Gamebase APIを呼び出すアセンブリの**Assembly Definition References**に**Gamebase**アセンブリを追加する必要があります。

<a id="section-2"></a>
## 2.81.2 { #section-2 }

<a id="ios"></a>
### iOS { #ios }

* Gamebase iOS SDK 2.81.2未満で次の問題が発生します。
    * アプリがSceneDelegateをサポートしている状態で、実行直後にGamebaseを初期化した際、コールバックが返されない問題が発生します。
    * 問題が解決されたGamebase iOS SDK 2.81.2を使用してください。

<a id="section-3"></a>
## 2.81.0 { #section-3 }

<a id="android"></a>
### Android { #android }

* Gamebase Android SDK 2.81.0では、R8 8.0.44未満のバージョンを使用するゲームプロジェクトでビルドが失敗する問題が存在します。
    * R8バージョンは、Unity EditorのAGPによって決定されます。Unity 2022 LTS以下で発生し、Unity 2023・Unity 6以上では発生しません。
    * 問題が解決されたGamebase Android SDK 2.82.0を使用するか、次のようにR8バージョンを強制的にアップデートすると問題が解決します。

            // baseProjectTemplate.gradle
            buildscript {
                repositories {
                    maven { url "https://storage.googleapis.com/r8-releases/raw" }
                }
                dependencies {
                    // 最小R8 8.0.44、Gamebase検証バージョン8.3.37推奨
                    classpath("com.android.tools:r8:8.3.37")
                }
            }

<a id="section-4"></a>
## 2.80.1 { #section-4 }

<a id="section-4-unity"></a>
### Unity { #section-4-unity }

* Auth.AuthToken의 extraParams 타입이 Dictionary&lt;string, string&gt;에서 Dictionary&lt;string, object&gt;로 변경되었습니다.

<a id="section-5"></a>
## 2.80.0 { #section-5 }

<a id="section-5-android"></a>
### Android { #section-5-android }

* Gamebase Android SDK 2.80.0では、以下の問題が発生します。
    * Pendingイベント関連のロジックにより、IAPサーバーに負荷がかかる問題があります。
    * この問題が修正されたGamebase Android SDK 2.80.1をご利用ください。

<a id="section-5-ios"></a>
### iOS { #section-5-ios }

* Xcodeの最小サポートバージョンが16.0から26.0に変更されました。
* **+[TCGBPurchase setPromotionIAPHandler:]** APIが非推奨(deprecated)になりました。

<a id="unreal"></a>
### Unreal { #unreal }

* (iOS) Project Settingsで有効化した機能に応じて、Info.plistに必要な項目が自動的に追加されます。
    * `AdditionalPlistData`で直接管理するには、[iOS Settings](./unreal-started/#ios-settings)で**Disable Auto Info.plist Update**を有効化してください。

<a id="section-6"></a>
## 2.79.0 { #section-6 }

<a id="section-6-ios"></a>
### iOS { #section-6-ios }

* **+[TCGBConfiguration setStoreCode:]** APIが非推奨となりました。
* **-[TCGBPurchase setStoreCode:]** APIが非推奨となりました。
* **TCGBPurchase.storeCode** APIが非推奨となりました。

<a id="section-7"></a>
## 2.77.0 { #section-7 }

<a id="common"></a>
### Common { #common }

* Apple IDアカウントの連携を解除(Revoke)した際に発生するGamebaseEventHandlerのIdP Revokedイベントの推奨ガイドを変更しました。
    * ユーザーにIdPが利用停止となったことを通知し、退会ではなくログアウトしてから再度ログインできるように変更してください。

<a id="section-7-ios"></a>
### iOS { #section-7-ios }

* **+[TCGBPurchase requestItemListAtIAPConsoleWithCompletion:]** APIが非推奨となりました。
    * **+[TCGBPurchase requestItemListPurchasableWithCompletion:]** APIを使用してください。

<a id="section-7-unity"></a>
### Unity { #section-7-unity }

* **Gamebase.Purchase.RequestItemListAtIAPConsole():** APIが非推奨となりました。
    * **Gamebase.Purchase.RequestItemListPurchasable()** APIを使用してください。

<a id="section-8"></a>
## 2.76.0 { #section-8 }

<a id="section-8-android"></a>
### Android { #section-8-android }

* **Gamebase.Purchase.requestItemListAtIAPConsole()** APIが非推奨になりました。
    * **Gamebase.Purchase.requestItemListPurchasable()** APIを使用してください。
* 米国テキサス、ユタ、ルイジアナなどの管轄区域における特定の年齢確認法に準拠するために追加された年齢確認APIは、Play Age Signalsライブラリのバージョンがベータ(0.0.1-beta02)状態であるため、常に例外が発生します。
    * [Game > Gamebase > Android SDK利用ガイド > ETC > Age Signals Support](./aos-etc/#age-signals-support)
    * 今後正常に動作させるためには、Play Age Signalsライブラリのバージョンが0.0.2にアップデートされたGamebase Android SDK 2.78.0を使用してください。

<a id="section-8-unreal"></a>
### Unreal { #section-8-unreal }

* `IGamebasePurchase::RequestItemListAtIAPConsole()` APIが非推奨になりました。
    * `IGamebasePurchase::RequestItemListPurchasable()` APIを使用してください。

<a id="section-9"></a>
## 2.75.0 { #section-9 }

<a id="section-9-ios"></a>
### iOS { #section-9-ios }

* Kakaogame認証のXcode最小サポートバージョンが16.0から16.2に変更されました。

<a id="section-10"></a>
## 2.71.2 { #section-10 }

<a id="section-10-android"></a>
### Android { #section-10-android }

* Gamebase Android SDK 2.71.2では、以下の問題が発生します。
    * ネットワーク接続が切断された後に復旧した場合や、アプリをバックグラウンドにしてからフォアグラウンドに復帰させた場合に、断続的にWebSocketモジュールでArrayIndexOutOfBoundsExceptionによるクラッシュが発生することがあります。
    * この問題が解決されたGamebase Android SDK 2.72.0を使用してください。

<a id="section-11"></a>
## 2.70.0 { #section-11 }

<a id="section-11-android"></a>
### Android { #section-11-android }

* Gamebase Android SDK 2.70.0で使用するGoogle Play Billing Library 7.1.1は、Android 7.0(API Level 24未満の端末で決済を試みる場合、クラッシュが発生します。
    * この問題を解決するためには、Gradleに下位OSのための[Java 8+ APIデシュガーリングサポート](https://developer.android.com/studio/write/java8-support#library-desugaring)宣言を追加する必要があります。
    * アプリモジュールのGradle、Unityの場合、launcherTemplate.gradleに次の宣言を追加してください。
    
            android {
                compileOptions {
                    // Flag to enable support for the new language APIs
                    coreLibraryDesugaringEnabled true
                }
            }

            dependencies {
                // desugar_jdk_libs 2.+ needs AGP 7.4+
                coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.5")
            }
    
    * desugar_jdk_libs 1.xバージョンはKakaogameログイン時にクラッシュが発生するため、2.xバージョンの適用を推奨します。
        * Unity EditorのバージョンによってAGPバージョンが異なるため、AGPおよびGradleバージョンのアップデートが必要な場合があります。
        
<a id="section-12"></a>
## 2.69.0 { #section-12 }

<a id="section-12-unity"></a>
### Unity { #section-12-unity }

* GPGS AutoLoginを使用する場合、**GetLastLoggedInProvider()**同期APIの代わりに新しく追加された **RequestLastLoggedInProvider(GamebaseCallback.GamebaseDelegate\<string> callback)**非同期APIを使用してください。

<a id="section-12-unreal"></a>
### Unreal { #section-12-unreal }

* 約款照会結果APIであるFGamebaseQueryTermsResultを修正しました。
    * TermsCountryTypeの値が設定されない問題を修正しました。
    * bPushEnabled, bAdAgreement, bAdAgreementNightを削除しました。
* GPGS AutoLoginを使用する場合、**GetLastLoggedInProvider()**同期APIの代わりに新しく追加された**RequestLastLoggedInProvider(GamebaseCallback.GamebaseDelegate\<string> callback)**非同期APIを使用してください。

<a id="section-12-android"></a>
### Android { #section-12-android }

* **gamebase-adapter-auth-gpgs-autologin**モジュールをビルドに含める場合は、**getLastLoggedInProvider()**同期APIの代わりに新しく追加された **requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;)**非同期APIを使用してください。

<a id="section-13"></a>
## 2.68.1 { #section-13 }

<a id="section-13-unreal"></a>
### Unreal { #section-13-unreal }

* (Windows) WebViewプラグインをオプションで選択できるように変更されました。
    * [WebViewプラグインガイド](./unreal-started/#windows-settings)を確認してアップデートが必要です。
* (Windows)クラッシュログを送信する際に、プロジェクトバイナリパスにシンボルファイルを圧縮したファイルが作成されるように追加されました。
    * [クラッシュログ送信ガイド](./unreal-logger/#crash-reporter)

<a id="section-14"></a>
## 2.68.0 { #section-14 }

<a id="section-14-android"></a>
### Android { #section-14-android }

<a id="section-14-android-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートバージョンがAndroid 5.0以上に引き上げられました。(minSdk 19→21)

<a id="section-15"></a>
## 2.67.1 { #section-15 }

<a id="section-15-unreal"></a>
### Unreal { #section-15-unreal }

* (Windows) Purchase設定時にストアを一つだけ選択できるように変更されました。
    * ストアの再設定が必要です。
* (Windows) Epic Games Store使用時にEOS SDKのハンドルを登録するプロセスが変更されました。
    * Online Subsystem EOSを使用する場合、Gamebase初期化時にStoreCodeがEpic Games Storeの該当する値であれば自動的にハンドルを登録します。
    * Online Subsystem EOSを使用しない場合、[Windows Settings](./unreal-started/#windows-settings)ガイドを参考にしてEOSのハンドルを登録するプロセスが必要です。
* (Windows) Steamworks SDKサポートバージョンが1.59に変更されました。
    * [Steamworksアップグレードガイド](./unreal-started/#windows-settings)を確認してアップデートが必要です。

<a id="section-16"></a>
## 2.67.0 { #section-16 }

<a id="section-16-unity"></a>
### Unity { #section-16-unity }

<a id="section-16-unity-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートUnityバージョンが2020.3.0から2020.3.16に変更されました。
* 下位バージョンのUnityサポートが必要な場合は[サポート](https://toast.com/support/inquiry)にお問い合わせください。

<a id="section-16-unreal"></a>
### Unreal { #section-16-unreal }

<a id="section-16-unreal-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートバージョンがUE 4.26からUE 4.27に変更されました。

<a id="android-ios"></a>
### Android, iOS { #android-ios }

* Twitter認証方式をOAuth 2.0に変更し、以下の設定を変更しないとログインが動作しません。
    * OAuth 2.0 Client ID及びClient Secret発行
        * Twitter Developer PortalでOAuth 2.0 Client IDとClient Secretを生成した後、Gamebaseコンソールに登録します。
    * Callback URL設定
        * GamebaseコンソールにCallback URL(https://id-gamebase.toast.com/oauth/callback)を設定します。 
        * 同じCallback URLをTwitter Developer Portalに追加します。
    * 詳細は以下のリンクをご覧ください。
        * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information > 6. Twitter](./oper-app/#6-twitter)

<a id="section-17"></a>
## 2.66.3 { #section-17 }

<a id="section-17-unity"></a>
### Unity { #section-17-unity }

<a id="section-17-unity-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートUnityバージョンを2018.4.0から2020.3.0に変更しました。
* 下下位バージョンのUnityのサポートが必要な場合は、[サポート](https://toast.com/support/inquiry)にお問い合わせください。

<a id="section-18"></a>
## 2.66.2 { #section-18 }

<a id="section-18-ios"></a>
### iOS { #section-18-ios }

<a id="section-18-ios-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs

* 以下のフィールドは非推奨となりました。
    * **TCGBWebViewConfiguration.orientationMask**
    
<a id="section-19"></a>
## 2.66.0 { #section-19 }

<a id="section-19-unreal"></a>
### Unreal { #section-19-unreal }
* APIの使用方法を変更しました。
    * `IModuleInterface`を継承した**IGamebase**で提供していたAPIを`UGameInstanceSubsystem`を継承した**UGamebaseSubsytem**で提供するように変更しました。
    * **UGamebaseSubsytem**はGameInstanceのサブシステムであるため、GameInstanceライフサイクルに従い、SDK API呼び出し時に使用するGameInstanceを通じて該当サブシステムを検索してAPIを使用する必要があります。
    * Unreal Coding Standardの命名規則に準拠するように変更しました。
```cpp
if (UGamebaseSubsystem* GamebaseSubsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance()))
{
    GamebaseSubsystem->Initialize(...);
}
```

* **GamebaseInterace** モジュールが削除されたため、Gamebaseを使用する場合は、モジュールの依存関係から削除する必要があります。
        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase"
            }
        );
        
<a id="section-20"></a>
## 2.65.0 { #section-20 }

<a id="section-20-common"></a>
### Common { #section-20-common }

* Gamebase SDK 2.65.0で画像告知機能を使用する際に発生する問題を修正しました。
    * 表示する画像告知がない場合、エラーの代わりに成功コールバックが呼び出されるように変更しました。
    * 登録された画像告知がない場合、空白の告知画面が表示され、この時、Androidでは「今日は表示しない」をチェックした後、閉じるとcrashが発生する問題を修正しました。
    * 問題が解決されたGamebase SDK 2.65.1以上を使用してください。

<a id="section-20-android"></a>
### Android { #section-20-android }

* Google billing client version 6.2.1バージョンが適用され、Android OS 4.4(API Level 19)端末で決済するには追加設定が必要です。
    * 詳細は[Game > Gamebase > Android SDK使用ガイド > はじめる > Setting > Gradle > Root level build.gradle](./aos-started/#root-level-buildgradle)ガイドを参照してください。

<a id="section-20-ios"></a>
### iOS { #section-20-ios }

* Facebook SDKが17.0.1にアップデートされ、Dynamic Frameworkに変更されました。
    * Gamebase SDKをダウンロードしてXcodeに直接設定する場合、Facebook SDKをEmbeded Frameworksに追加する必要があります。
    * 詳細は[Game > Gamebase > iOS SDK使用ガイド > はじめる > Setting > Xcode Settings](./ios-started/#xcode-settings)ガイドを参照してください。

<a id="section-21"></a>
## 2.64.0 { #section-21 }

<a id="section-21-ios"></a>
### iOS { #section-21-ios }

* Kakaogame認証の最小サポートバージョンが12.0から13.0に変更されました。

<a id="section-22"></a>
## 2.63.0 { #section-22 }

<a id="section-22-ios"></a>
### iOS { #section-22-ios }

* Facebook SDKが17.0.0にアップデートされ、Info.plistにFacebookClientTokenとFacebookDisplayNameを追加する必要があります。
```
<key>FacebookClientToken</key>
<string>{FACEBOOK_CLIENT_TOKEN}</string>
<key>FacebookDisplayName</key>
<string>{FACEBOOK_DISPLAY_NAME}</string>
```

<a id="section-22-unreal"></a>
### Unreal { #section-22-unreal }

* Android Firebase Notificationの設定方法が変更され、プラグイン内のgoogle-services-json.xmlファイルではなく、設定ツールで直接指定するように変更されました。
    * 提供していたGamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xmlファイルが削除されました。
    * google-services-json.xmlではなく、Firebaseコンソールからダウンロードした google-services.jsonファイルを[Android設定ツール](./unreal-started/#android-settings)のPush項目内のFCMの下にある`GoogleServicesFilePath`の値を設定してください。

<a id="section-23"></a>
## 2.62.0 { #section-23 }

<a id="section-23-android"></a>
### Android { #section-23-android }

* Gamebase Android SDK 2.62.0は、Android 7.0(API Level 24)未満の端末で以下の問題が発生します。
    * Gamebase.loginForLastLoggedInProviderの呼び出しが常に失敗します。
    * Guestアカウントが失われます。
    * 問題が解決されたGamebase Android SDK 2.62.1を使用してください。

<a id="section-23-ios"></a>
### iOS { #section-23-ios }
* Xcode の最小サポートバージョンが 14.1 から 15 に変更されました。
* Gamebase iOSの最小サポートバージョンが11.0から12.0に変更されました。
* GamebaseとGamebase AdapterにPrivacy Manifestと署名を適用しました。
    * 2024年5月1日以降に新規リリースまたはアップデートする場合、Appleのポリシーに基づき、Gamebase iOS SDK 2.62.0以上を適用する必要があります。
* LINE認証の最小サポートバージョンが11.0から13.0に変更されました。

<a id="section-23-unity"></a>
### Unity { #section-23-unity }

* Apple個人情報保護ポリシーに準拠するための対応が完了しました。
    * Privacy Manifestファイルが追加されました。
    * Framework に署名が適用されました。
    * 2024年5月1日以降、新しいリリースやアップデートには、Appleのポリシーに基づき、Gamebase SDK for Unity 2.62.0以降を適用する必要があります。

<a id="section-24"></a>
## 2.59.0 { #section-24 }

<a id="section-24-ios"></a>
### iOS { #section-24-ios }

* GamebaseAuthNaverAdapterで使用するNAVER iOS SDKがxcframeworkに変更されました。

<a id="section-25"></a>
## 2.58.0 { #section-25 }

<a id="section-25-android"></a>
### Android { #section-25-android }

<a id="section-25-android-twitter-idp"></a>
#### Twitter IdP
* Twitter api serverの証明書のアップデートにより、minSDKVersionが19から21に更新されました。

<a id="section-25-ios"></a>
### iOS { #section-25-ios }

* GamebaseAuthPaycoAdapterで使用するPAYCO iOS SDKがxcframeworkに変更されました。

<a id="section-26"></a>
## 2.57.0 { #section-26 }

<a id="section-26-ios"></a>
### iOS { #section-26-ios }

* Privacy manifestファイルを追加しました。
    * Privacy manifestファイルでGamebase iOS SDKが収集するデータと許可された理由を明示しなければならないAPIのリストを見ることができます。
    * Appleのポリシーに基づき、2024年春までにGamebase iOS SDK 2.57.0以上にアップデートしてください。

<a id="section-26-unreal"></a>
### Unreal { #section-26-unreal }
 
* Gamebase モジュールが分離されました。Gamebaseコードを使用するには、モジュールのBuild.csファイル内の**GamebaseInterface**モジュールを依存モジュールとして追加する必要があります。

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase",
                "GamebaseInterface"
            }
        );

<a id="section-27"></a>
## 2.56.0 { #section-27 }

<a id="section-27-unreal"></a>
### Unreal { #section-27-unreal }

* 提供されるタイプがUSTRUCTから一般構造体に変更されました。
    * 結果として受け取るタイプは基本的に提供されない値の場合、Toptional形式で提供され、すでに値を使用していた場合は、Value.IsSet()APIを通じて設定された値かどうか確認後にValue.GetValue()を通じて値を使用できます。

<a id="section-28"></a>
## 2.55.0 { #section-28 }

<a id="section-28-android"></a>
### Android { #section-28-android }

<a id="section-28-android-naver-idp"></a>
#### Naver IdP
* Naver Login SDKのアップデートでminSDKが19から21に上がりました。

<a id="section-28-android-mycard-adapter"></a>
#### MyCard Adapter
* NHN Cloud SDKのアップデートでminSDKが19から21に上がりました。

<a id="section-29"></a>
## 2.54.0 { #section-29 }

<a id="section-29-ios"></a>
### iOS { #section-29-ios }

* Gamebase SDKがxcframeworkに変更されました。
* Facebook iOS SDKが14.1.0にアップデートされました。Gamebase ConsoleのAdditionalInfoにFacebook Client Tokenを設定してください。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > App > Authentication Information > 1. Facebook](./oper-app/#1-facebook)    

<a id="section-30"></a>
## 2.53.0 { #section-30 }

<a id="section-30-android"></a>
### Android { #section-30-android }

<a id="section-30-android-contact"></a>
#### Contact

* 'サポート'機能を使用する場合、添付ファイル選択時に権限要求が正常に動作するように、以下のガイドに従ってAndroidManifest.xmlに権限設定を追加する必要があります。
    * [Game > Gamebase > Android SDK使用ガイド > はじめる > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)

<a id="section-30-android-line-idp"></a>
#### Line IdP

* 既存['はじめる'文書](./aos-started)でLine IdP使用時にAndroidManifest.xmlに宣言するように案内した下記の内容は、Line SDKのアップデートにより不要になったので削除してください。

```xml
<manifest>
    <!-- [Android11] settings start -->
    <queries>
        <!-- [LINE] Configurations begin -->
        <package android:name="jp.naver.line.android" />
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="https" />
        </intent>
        <!-- [LINE] Configurations end -->
    </queries>
    <!-- [Android11] settings end -->
        
    <application
          tools:replace="android:allowBackup"
          ... >
</manifest>
```

<a id="section-31"></a>
## 2.52.0 { #section-31 }

<a id="section-31-android"></a>
### Android { #section-31-android }

* Android 4.4(OS 19 Kitkat)端末でcrashが発生します。
    * イシューが修正されたGamebase Android SDK 2.52.1を使用してください。

<a id="section-31-unity"></a>
### Unity { #section-31-unity }

* '**IapOnestore**'と表示されていた**ONE Store v17** 決済アダプタがGamebase Setting Tool (v2.7.0)から'**IapOnestoreV17**'と表示されます。

<a id="section-31-ios"></a>
### iOS { #section-31-ios }

<a id="section-31-ios-weibo-idp"></a>
#### Weibo IdP

* WeiboSDKが3.3.3にアップデートされ、info.plistにweibosdk3.3を追加する必要があります。
```
<key>LSApplicationQueriesSchemes</key>
<array>
    …
    <string>weibosdk</string>
    <string>weibosdk2.5</string>
    <string>weibosdk3.3</string>
    ...
</array>
</key>
```

<a id="section-31-ios-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs

* iOS 16.4からAppleがCTCarrier classがdeprecatedされたため、下記のAPIがdeprecatedされました。
    * **+[TCGBGamebase countryCode]**
    * **+[TCGBGamebase countryCodeOfUSIM]**
    * **+[TCGBGamebase carrierCode]**
    * **+[TCGBGamebase carrierName]**
    * **+[TCGBUtil countryCode]**
    * **+[TCGBUtil usimCountryCode]**
    * **+[TCGBUtil carrierCode]**
    * **+[TCGBUtil carrierName]**

<a id="section-32"></a>
## 2.50.0 { #section-32 }

<a id="section-32-android"></a>
### Android { #section-32-android }

* Android 4.4(OS 19 Kitkat)端末でcrashが発生します。
    * 問題が修正されたGamebase Android SDK 2.50.1を使用してください。

<a id="section-33"></a>
## 2.49.0 { #section-33 }

<a id="section-33-unreal"></a>
### Unreal { #section-33-unreal }

* 最小サポートバージョンが4.22から4.26に変更されました。
* 未消費履歴照会APIが変更され、新規APIに変更する必要があります。

        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* 有効化購読照会APIが変更され、新規APIに変更する必要があります。
    * 既存APIと同じ結果を得るには**FGamebasePurchasableConfiguration.allStores**を**true**に設定する必要があります。

            // Unity: Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // Unity: New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

<a id="section-33-android"></a>
### Android { #section-33-android }

```
最小サポートバージョンがAndroid 4.4以上になりました。(minSdk 16 -> 19)
```

<a id="section-34"></a>
## 2.47.0 { #section-34 }

<a id="section-34-android"></a>
### Android { #section-34-android }

* UnityでProguard適用時、Purchase関連APIの呼び出しに失敗します。
    * 当該イシューは2.48.0で修正されました。

<a id="section-35"></a>
## 2.45.0 { #section-35 }

<a id="android-ios-unity"></a>
### Android, iOS, Unity { #android-ios-unity }

* 未消費履歴照会APIが変更されましたので新規APIに変更してください。

        // Unity: Deprecated API
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
        // Unity: New API
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                       GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
        // Android: Deprecated API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity,
                                                       GamebaseDataCallback<List<PurchasableReceipt>> callback);
        // Android: New API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity,
                                                       PurchasableConfiguration configuration,
                                                       GamebaseDataCallback<List<PurchasableReceipt>> callback);
        // iOS: Deprecated API
        + (void)requestItemListOfNotConsumedWithCompletion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
        // iOS: New API
        + (void)requestItemListOfNotConsumedWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                                   completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;

* 有効な購読照会APIが変更されたため、新規APIに変更してください。
    * 既存APIと同じ結果を得るには**PurchasableConfiguration.allStores**を**true**に設定してください。

            // Unity: Deprecated API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
            // Unity: New API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                        GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
            // Android: Deprecated API
            Gamebase.Purchase.requestActivatedPurchases(Activity activity,
                                                        GamebaseDataCallback<List<PurchasableReceipt>> callback);
            // Android: New API
            Gamebase.Purchase.requestActivatedPurchases(Activity activity,
                                                        PurchasableConfiguration configuration,
                                                        GamebaseDataCallback<List<PurchasableReceipt>> callback);
            // iOS: Deprecated API
            + (void)requestActivatedPurchasesWithCompletion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
            // iOS: New API
            + (void)requestActivatedPurchasesWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                                    completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;

<a id="section-36"></a>
## 2.42.2 { #section-36 }

<a id="section-36-unity"></a>
### Unity { #section-36-unity }

* Gamebase SettingTool(v2.5.0)にOnestore v19決済Adapterが追加されました。
    * **SettingTool > Android**設定でOnestore v19 Adapter有効にする時、 iap_sdk-v19.xx.xx.aarダウンロードページに接続し、当該ファイルを**Assets > Plugins > Android**フォルダにコピーする必要があります。

<a id="section-37"></a>
## 2.44.0 { #section-37 }

<a id="section-37-android"></a>
### Android { #section-37-android }

* Gamebase Android SDK 2.44.0でregisterPushを呼び出すとAndroid 6.0(M, API Level 23)端末でクラッシュが発生することが確認されました。
    * 問題が修正されたGamebase Android SDK 2.44.1を使用してください。

<a id="section-38"></a>
## 2.43.3 { #section-38 }

<a id="section-38-unreal"></a>
### Unreal { #section-38-unreal }

* Google Billing Client 5.0.0バージョンに変更されました。Unrealで提供するOnline SubSystem GooglePlayプラグイン使用時、/Config/Android/AndroidEngine.iniファイルに該当値を追加すると、ビルド時にエラーが発生しません。

            [OnlineSubsystemGooglePlay.Store]
            bUseGooglePlayBillingApiV2=False

<a id="section-39"></a>
## 2.42.1 { #section-39 }

<a id="section-39-unity"></a>
### Unity { #section-39-unity }
    
<a id="section-39-unity-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs
* FGamebaseWebViewConfigurationでenableFixedFontSizeフィールドは今後はサポートしません。
* GamebaseWebViewConfiguration一部フィールドにデフォルト値が追加され、値を設定していない場合、従来と異なる動作をする場合があります。
    * ナビゲーションバーの色相フィールドであるcolorR、colorG、colorB、colorAのデフォルト値が18、93、230、255に設定されました。
    * ナビゲーションバーの有効/無効を指定するフィールドであるisNavigationBarVisibleのデフォルト値がtrueに設定されました。
    * Webビュー内の戻るボタンの有効/無効を指定するフィールドであるisBackButtonVisibleのデフォルト値がtrueに設定されました。

<a id="section-39-unreal"></a>
### Unreal { #section-39-unreal }

* (iOS) [iOS設定ツール](./unreal-started/#ios-settings)でXcodeのパスを変更できるように**Xcode Path**設定が追加されました。
    * 変更しない場合、デフォルト値に設定されます。 (デフォルト値：/Applications/Xcode.app)

<a id="section-39-unreal-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs
* FGamebaseConfigurationのenableKickoutPopupプロパティは、今後はサポートしません。
* FGamebaseConfigurationの一部フィールドにデフォルト値が追加され、値を設定していない場合、従来と異なる動作をする場合があります。
    * enableLaunchingStatusPopupのデフォルト値がtrueに設定されました。
    * enableBanPopupのデフォルト値がtrueに設定されました。
* FGamebaseWebViewConfigurationでenableFixedFontSizeフィールドは今後はサポートしません。
* FGamebaseWebViewConfigurationの一部フィールドにデフォルト値が追加され、値を設定していない場合、従来と異なる動作をする場合があります。
    * ナビゲーションバーの色相フィールドであるcolorR、colorG、colorB、colorAのデフォルト値が18、93、230、255に設定されました。
    * ナビゲーションバーの有効/無効を指定するフィールドであるisNavigationBarVisibleのデフォルト値がtrueに設定されました。
    * Webビュー内の戻るボタンの有効/無効を指定するフィールドであるisBackButtonVisibleのデフォルト値がtrueに設定されました。

<a id="section-40"></a>
## 2.41.0 { #section-40 }

<a id="section-40-android"></a>
### Android { #section-40-android }

* Webビューに登録したカスタムスキームイベントが動作するとき、自動的にWebビューが終了するようになります。
    * 以前のようにカスタムスキームイベントが動作してもWebビューを維持させたい場合には**GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)**APIを呼び出してください。
* Gamebase Android SDK 2.41.0は約款ウィンドウの「表示」ボタンが動作しないバグが存在します。
    * Gamebase約款ウィンドウを使用する場合、問題が解決したGamebase Android SDK 2.41.1を使用してください。

<a id="section-40-unity"></a>
### Unity { #section-40-unity }

* Gamebase SettingTool必須アップデートが追加されました。 (v2.4.0)
    * 既存SettingToolはUnityプロジェクトから完全に削除した後、最新バージョンを再インストールする必要があります。
    * SettingTool v1のサポートを終了します。

<a id="section-41"></a>
## 2.40.0 { #section-41 }

<a id="section-41-unreal"></a>
### Unreal { #section-41-unreal }

* FGamebaseConfigurationのenableKickoutPopupプロパティをサポートしなくなりました。
* 一部APIの名前が変更されました。
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* (iOS) iOS設定ツールを提供し、ビルド時に必要なフレームワークのみ含まれるように修正しました。
* (iOS) PLCrashReporterがアップデートされ、エンジン内部にPLCrashReporterもアップデートが必要です。
    * [Game > Gamebase > Unreal SDK使用ガイド > はじめる > Installation > iOS Settings > PLCrashReporter](./unreal-started/#ios-settings)
* (iOS) Facebook iOS SDKが9.2.0バージョンにアップデートされ、swiftを使用するためにエンジンコードの修正が必要です。
    * [Game > Gamebase > Unreal SDK使用ガイド > はじめる > Installation > iOS Settings > Facebook SDK](./unreal-started/#ios-settings)

<a id="section-42"></a>
## 2.36.0 { #section-42 }

<a id="section-42-android"></a>
### Android { #section-42-android }

<a id="section-42-android-hangame-sdk"></a>
#### Hangame SDK
* Hangame Android SDK v1.4.5でsms_hashが内部で作成されるように改善されました。
    * これ以上sms_hashを設定する必要はありません。

<a id="section-43"></a>
## 2.35.0 { #section-43 }

<a id="section-43-android"></a>
### Android { #section-43-android }

<a id="section-43-android-naver-idp"></a>
#### NAVER IdP

* NAVERログアウト時にトークンを削除しません。
    * 再ログインするとき、情報提供同意ウィンドウが表示されません。
    * Webログイン時にはアカウントが変更されません。
    * 以前の動作を維持するにはGamebase ConsoleのAdditionalInfoに次のように設定してください。

```
{"logout_and_delete_token":true}
```

<a id="section-44"></a>
## 2.34.0 { #section-44 }

<a id="section-44-android"></a>
### Android { #section-44-android }

<a id="section-44-android-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs

* キックアウトポップアップの表示有無は、Gamebaseコンソールでキックアウト登録時に設定することができるため、次のフィールドがdeprecatedになりました。
    * **UIPopupConfiguration.enableKickoutPopup**

<a id="section-44-ios"></a>
### iOS { #section-44-ios }

<a id="section-44-ios-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs

* キックアウトポップアップの表示有無は、Gamebaseコンソールでキックアウト登録時に設定できるため、次のAPIがdeprecatedになりました。
    * **-[TCGBConfiguration enableKickoutPopup:]**
    * **-[TCGBConfiguration isEnableKickoutPopup]**

<a id="section-44-unity"></a>
### Unity { #section-44-unity }

* GamebaseConfigurationのenableKickoutPopupプロパティをサポートしません。

<a id="section-45"></a>
## 2.33.0 { #section-45 }

<a id="section-45-ios"></a>
### iOS { #section-45-ios }

* TCGB_ERROR_UNKNOWN_ERRORエラーにマッピングされたエラーコードが変更されました。
    * TCGB_ERROR_UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたTCGB_ERROR_SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。

<a id="section-45-unity"></a>
### Unity { #section-45-unity }

* GamebaseErrorCode.UNKNOWN_ERRORエラーにマッピングされたエラーコードが変更されました。
    * GamebaseErrorCode.UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode.SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。

<a id="section-45-unreal"></a>
### Unreal { #section-45-unreal }

* GamebaseErrorCode.UNKNOWN_ERRORエラーにマッピングされたエラーコードが変更されました。
    * GamebaseErrorCode::UNKNOWN_ERRORエラーにマッピングされたエラーコードを999から9999に変更しました。
    * エラーコード999にマッピングしたGamebaseErrorCode::SOCKET_UNKNOWN_ERRORエラーを新たに追加しました。

<a id="section-46"></a>
## 2.32.0 { #section-46 }

<a id="section-46-android"></a>
### Android { #section-46-android }

* Gamebase Access Tokenの有効期限が切れて復元できなかったときに発生するGamebaseEventHandlerイベントcategoryが**GamebaseEventCategory.OBSERVER_HEARTBEAT**から**GamebaseEventCategory.LOGGED_OUT**に変更されました。
    * **GamebaseEventCategory.OBSERVER_HEARTBEAT**イベントでGamebaseEventObserverData.codeの値が**GamebaseError.AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO(3102)**のときにログインするように実装した場合は、**GamebaseEventCategory.LOGGED_OUT**イベントでログインを行うように変更してください。

<a id="section-47"></a>
## 2.29.0 { #section-47 }
 
<a id="section-47-ios"></a>
### iOS { #section-47-ios }

* Xcode最低サポートバージョンが12から13に変更されました。
    * Xcode 12でアーカイブビルドを行うとエラーが発生します。Xcode 13にアップデートしてください。

<a id="section-47-unity"></a>
### Unity { #section-47-unity }
 
* Setting Tool 2.0.0が配布されました。
    * フォルダ構造が変更され、以前のバージョンのSetting Toolを完全に削除した後、再インストールする必要があります。 
    * Setting Tool 1.5.0以下のユーザーは、下記ディレクトリにあるGamebase関連ライブラリを全て削除する必要があります。 
        * **Assets/Plugins/Android**  
        * **Assets/Plugins/iOS** 
    * 変更された内容と使用方法は以下のガイドを確認してください。 
        * [Game > Gamebase > Unity SDK使用ガイド > はじめる > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)

<a id="section-48"></a>
## 2.26.0 { #section-48 }

<a id="section-48-unity"></a>
### Unity { #section-48-unity }

* 該当バージョンを使用する時は**Assets/Gamebase/Toast/IAP/Plugins**を直接削除してから使用してください。
    * Gamebase Unity SDK 2.27.0以上のバージョンが適用された場合には削除する必要がありません。

<a id="section-48-unreal"></a>
### Unreal { #section-48-unreal }

* Gamebaseでmultidex設定が削除されました。設定するには以下のガイドを参照してください。
    * [Game > Gamebase > Unreal SDK使用ガイド > はじめる > Installation > Android Settings > multidex適用](./unreal-started/#android-settings)


<a id="section-49"></a>
## 2.25.0 { #section-49 }

<a id="section-49-android"></a>
### Android { #section-49-android }

<a id="section-49-android-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートAndroid Gradle Plugin(AGP)バージョンが2.3.0から3.2.0に変更されました。
    * しかしtargetSdkVersionを30以上に設定する場合、Android 11端末に対応するためにはAGP 3.3.3以上が必要です。
        * 次の文書を参照してください。
        * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Android 11](./aos-started/#android-11)
* 下位バージョンのAGPサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。

<a id="section-49-android-androidx"></a>
#### AndroidX

* Android Support Library依存性がAndroidXに変更されたため、Gradleに次の変更事項を適用してください。

* gradle.propertiesファイルにAndroidXに対応していないライブラリのためのマイグレーション宣言を追加してください。

```groovy
# >>> [AndroidX]
android.useAndroidX=true
android.enableJetifier=true
```

* build.gradleファイルに最新AndroidXのためのJava 8ビルド設定を追加してください。

```groovy
android {
    compileOptions {
        // >>> [AndroidX]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

<a id="section-49-android-under-agp-340"></a>
#### Under AGP 3.4.0

* Android Gradle Pluginバージョンが3.4.0未満の場合、ビルドが失敗するため、gradle.propertiesファイルに次の宣言が必要です。

```groovy
# gradle.properties
# >>> Fix for AGP under 3.4.0
android.enableD8.desugaring=true
android.enableIncrementalDesugaring=false
```

<a id="section-49-android-line-idp"></a>
#### LINE IdP

* LINE IdPを使用する場合、LINE SDK内部に**&lt;queries&gt;**タグが存在するため、AGPのバージョンによってはビルドが失敗することがあります。
    * 次のガイドを参考にして「queries」タグビルドが可能なAGPバージョンにアップグレードしてください。
    * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Android 11](./aos-started/#android-11)
* LINE IdPを使用する場合、LINE SDK内部に**android:allowBackup="false"**が宣言されており、アプリケーションビルド時にManifest mergerでfailが発生することがあります。ビルドが失敗する場合は、次のようにapplicationタグに**tools:replace="android:allowBackup"**宣言を追加してください。

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

<a id="section-49-ios"></a>
### iOS { #section-49-ios }

* Sign In with AppleのASAuthorizationErrorUnknownエラーが発生した場合、 TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR (3009)エラーをリターンするように変更されました。

<a id="section-49-unity"></a>
### Unity { #section-49-unity }

* 該当バージョンを使用する時は**Assets/Gamebase/Toast/IAP/Plugins**を直接削除してから使用してください。
    * Gamebase Unity SDK 2.27.0以上のバージョンが適用された場合には削除する必要がありません。

<a id="section-49-unity-changed-minimum-support-version"></a>
#### Changed Minimum Support Version

* 最小サポートUnityバージョンが2017.4.16から2018.4.0に変更されました。
* 下位バージョンのUnityサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。

<a id="section-49-unity-androidx-build"></a>
#### AndroidX Build

* Gamebase Android SDKのAndroidX移行により、Androidビルド時に次の宣言を追加してください。
* 2019.3未満

```groovy
// mainTemplate.gradle
([rootProject] + (rootProject.subprojects as List)).each {
    ext {
        it.setProperty("android.useAndroidX", true)
        it.setProperty("android.enableJetifier", true)
    }
}
```

* 2019.3以上

```groovy
// gradleTemplate.properties
android.useAndroidX=true
android.enableJetifier=true
```

<a id="section-49-unity-under-agp-340"></a>
#### Under AGP 3.4.0

* Unity Editorバージョンが2018.4.3以下または2019.1.6以下の場合、AGPバージョンが低くて(3.2.0)ビルドが失敗するため、次の宣言を追加してください。

```groovy
// mainTemplate.gradle
([rootProject] + (rootProject.subprojects as List)).each {
    ext {
        // >>> Fix for AGP under 3.4.0
        it.setProperty("android.enableD8.desugaring", true)
        it.setProperty("android.enableIncrementalDesugaring", false)
    }
}
```

<a id="section-49-unreal"></a>
### Unreal { #section-49-unreal }

<a id="section-49-unreal-androidx-build"></a>
#### AndroidX Build

* Gamebase Android SDKのAndroidX移行によりAndroidビルド時にUPLに次の宣言を追加してください。

```xml
<gradleProperties>    
  <insert>      
    android.useAndroidX=true      
    android.enableJetifier=true    
  </insert>  
</gradleProperties>
```

<a id="section-50"></a>
## 2.21.2 { #section-50 }

<a id="section-50-ios"></a>
### iOS { #section-50-ios }

* Gamebase iOS SDK 2.21.1でbitcodeを有効にした後に**アーカイブビルド**を行うとエラーが発生します。
    * bitcodeを使用したい場合は、上記の問題が修正されたGamebase iOS SDK 2.21.2を使用してください。

<a id="section-51"></a>
## 2.21.0 { #section-51 }

<a id="section-51-android"></a>
### Android { #section-51-android }

* Gamebase Android SDK 2.21.0は、jCenterには誤ったビルドが配布され、**jcenter()**を**mavenCentral()**より先に宣言した場合、すべてのGamebase APIでクラッシュが発生する可能性があります。
    * 正常に配布されたGamebase Android SDK 2.21.1を使用するか、**mavenCentral()**を**jcenter()**より先に宣言してください。
* Maven Repository
    * jCenterが一般ユーザー向けのサービスを終了して( [https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) )新しいビルドのアップロードはできなくなりました。(jCenterのアクセスも2022年2月1日に終了します。)
    * それでGamebase Android SDK 2.21.0からは**jcenter()**ではなく**mavenCentral()**でのみ配布が行われるためgradle repositoryにmavenCentralを追加してください。

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
}
```

<a id="section-51-android-line-idp"></a>
#### LINE IdP

* LINE IdPを使用する場合、LINE SDKのアップデートにより以下のようにGradleに**JavaVersion.VERSION_1_8**を設定していない場合はビルドが失敗します。

```groovy
android {
    compileOptions {
        // >>> [LINE IdP]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

<a id="section-51-ios"></a>
### iOS { #section-51-ios }

* Gamebase iOS SDK 2.21.0でbitcodeを使用する場合はエラーが発生します。
    * bitcodeを使用したい場合はGamebase iOS SDK 2.21.1を使用してください。

<a id="section-52"></a>
## 2.20.2 { #section-52 }

<a id="section-52-ios"></a>
### iOS { #section-52-ios }

<a id="section-52-ios-facebook-idp"></a>
#### Facebook IdP

* Gamebase iOS SDK 2.20.2でFacebook SDKが9.1.0にアップデートされました。 
    * Facebook SDKに追加設定が必要なため、info.plistに以下の値を追加してください。設定しない場合はクラッシュが発生する場合がります。
        * FacebookAutoLogAppEventsEnabled
        * FacebookAdvertiserIDCollectionEnabled
* 詳細な内容は[Facebook iOS SDKガイド](https://developers.facebook.com/docs/app-events/getting-started-app-events-ios)を参照してください。

<a id="section-53"></a>
## 2.19.0 { #section-53 }

<a id="section-53-android"></a>
### Android { #section-53-android }

<a id="section-53-android-weibo-idp"></a>
#### Weibo IdP

* Gamebase Android SDK 2.19.0でWeibo IdPログインと別のIdPログインを交互に呼び出すとクラッシュが発生します。
    * Weibo IdPを使用する場合は問題が修正されたGamebase Android SDK 2.19.1を使用してください。

<a id="section-54"></a>
## 2.18.2 { #section-54 }

<a id="section-54-android"></a>
### Android { #section-54-android }

<a id="section-54-android-removed-apis"></a>
#### Removed APIs

* Gamebase Android SDK 2.6.0でdeprecatedとなっていた以下の関数が削除されました。
    * **GamebaseConfiguration.Builder.setFCMSenderId()**
    * **GamebaseConfiguration.Builder.setTencentAccessKey()**
    * **GamebaseConfiguration.Builder.setTencentAccessId()**

<a id="section-55"></a>
## 2.18.0 { #section-55 }

<a id="section-55-android"></a>
### Android { #section-55-android }

<a id="section-55-android-purchase-google"></a>
#### Purchase Google

* Gamebase Android SDK 2.18.0でGoogleアイテム決済を呼び出すとクラッシュが発生します。
    * 問題が修正されたGamebase Android SDK 2.18.1を使用してください。

<a id="section-56"></a>
## 2.17.0 { #section-56 }

<a id="section-56-android"></a>
### Android { #section-56-android }

* Gamebase Android SDK 2.17.0でGamebase.ImageNotice.showImageNotices APIを呼び出すとクラッシュが発生します。
    * 2.17.0のクラッシュおよびOS 5.0～6.0でカスタムスキームイベントが動作しない問題が修正されたGamebase Android SDK 2.17.4を使用してください。

<a id="section-57"></a>
## 2.15.1 { #section-57 }

<a id="section-57-ios"></a>
### iOS { #section-57-ios }

* SDKで定義したタイプ **GamebaseEventCategory**をNSStringの代わりに使用する場合、該当タイプを **TCGBGamebaseEventCategory**に修正する必要があります。

<a id="section-58"></a>
## 2.15.0 { #section-58 }

<a id="section-58-android"></a>
### Android { #section-58-android }

<a id="section-58-android-purchase-google"></a>
#### Purchase Google

* **gamebase-adapter-purchase-google**を使用する場合、Gamebase SDK 2.15.0未満バージョンから2.15.以上にアップグレードする場合は、必ず**以前のバージョンのGame Client Versionをアップデート必須**に設定する必要があります。
	* Google Billing Clientモジュールがアップデートされ、複数の端末で別々のBilling Clientバージョンが適用された状態でアイテムを購入する場合、エラーが発生した時の再処理に問題が発生することがあるためです。

<a id="section-59"></a>
## 2.6.0 { #section-59 }

<a id="section-59-unity"></a>
### Unity { #section-59-unity }

<a id="section-59-unity-android-limitation"></a>
#### Android Limitation

* Android Support Libraryバージョンが28.0.0に上がり、Unity 5、Unity 2017.1、Unity 2017.2ではAndroidのビルドが失敗します。
	* Unity 2017.3未満のバージョンのEditorを使用している場合はUnity 2017.3以上のバージョンをインストールし、「Editor/Data/PlaybackEngines/AndroidPlayer/Tools/gradle/lib」フォルダをコピーして使用中のUnity Editorの同じパスに上書きしてmainTemplate.gradleファイルを下記のように修正してください。

```groovy
// GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
buildscript {
	repositories {
		jcenter()
        // >>> For download Gradle Android Plugin
        maven {
            url 'https://maven.google.com'
        }
	}

	dependencies {
		//classpath 'com.android.tools.build:gradle:2.1.0'
        // >>> Update Gradle Android Plugin version
        classpath 'com.android.tools.build:gradle:2.3.0'
	}
}
```

<a id="section-59-unity-firebase-push"></a>
#### Firebase Push

* Firebase Cloud Messagingを使用している場合Firebaseコンソールからgoogle-services.jsonファイルをダウンロードし、xmlリソースに変換してプロジェクトに追加しないとPushが正常に動作しません。
    * Gamebase 2.5.0 以前のバージョンではxmlリソースなしてPush動作に問題ありませんでしたが、Gamebase 2.6.0からは必ずxmlリソースが必要です。
* 下のガイドを参考にしてPush設定を行ってください。
    * [\[Game > Gamebase > Android SDK ご利用ガイド > Push > Settings > Firebase\]](./aos-push/#firebase)

<a id="section-59-unity-standalone"></a>
#### Standalone

* Removed Japan Purchase
	* 日本決済がFadeOutしました。
	* GamebaseUnitySDK_IAPAdapterを使用している場合は、下記のフォルダを直接削除してください。
        * Asset/Toast/Common
        * Asset/Toast/Core
        * Asset/Toast/IAP
        * Asset/Toast/Standalone

<a id="section-59-android"></a>
### Android { #section-59-android }

<a id="section-59-android-limitation"></a>
#### Limitation

* minSdkVersionが15(IceCreamSandwichMR1, 4.0.3)から16(JellyBean, 4.1)に変更されました。
	* OS 4.1未満の端末では正常な動作を保障しないため、プロジェクトのminSdkVersionが15の場合、16に変更してください。

<a id="section-59-android-removed-apis"></a>
#### Removed APIs

* 削除された関数は次のとおりです。代替関数に変更してください。
    * **Gamebase.getAuthBanInfo()**を削除しました。**Gamebase.getBanInfo()**に変更してください。
    * **Gamebase.getLanguageCode()**を削除しました。**Gamebase.getDeviceLanguageCode()**に変更してください。
    * **new GamebaseConfiguration.Builder(void)**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。
    * **new GamebaseConfiguration.Builder.setAppId()**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。
    * **new GamebaseConfiguration.Builder.setAppVersion()**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。

<a id="section-59-android-changeddeprecated-apis"></a>
#### Changed/Deprecated APIs

* Gamebase.activeApp()は自動的に呼び出されるため、今後は呼び出す必要がありません。
* Gamebase.initialize()の引数に必要なGamebaseConfigurationの作成方法が変更されました。
    * **new GamebaseConfiguration.Builder(String, String)**の代わりに**GamebaseConfiguration.newBuilder()**を呼び出してください。
* LaunchingStatus.isPlayable()は今後、呼び出さないでください。
	* [\[Game > Gamebase > Android SDK使用ガイド > 初期化 > Launching Information\]](./aos-initialization/#launching-information)文書を参照してLaunching Status Codeに応じてゲームプレイが可能かどうかを直接決定してください。
* Purchase
    * Store Codeの変更ができないため、**GamebaseConfiguration.newBuilder()**でStore Codeを伝達する必要があります。
        * Gamebase.Purchase.getStoreCode() / Gamebase.Purchase.setStoreCode()は削除される予定です。今後は使用しないでください。
    * Gamebase.Purchase.requestRetryTransaction()は呼び出す必要がありません。
* Push
    * Gamebase Android SDK 2.6.0以上からは、プッシュメッセージを送信する時、Gamebaseコンソールの**プッシュ**タブのメニューから送信する必要があります。
        * Gamebase Android SDK 2.6.0未満の場合、Gamebaseコンソールの**プッシュ(旧)** タブからプッシュを送信する必要があります。
    * GamebaseConfiguration.Builder.setFCMSenderId()は呼び出す必要がありません。
    * GamebaseConfiguration.Builder.setTencentAccessKey(), GamebaseConfiguration.Builder.setTencentAccessId()を呼び出している場合、APIの呼び出しを削除し、build.gradleに次のように宣言する必要があります。

```groovy
android {
    defaultConfig {
        ...
        // >>> For Tencent Push Notification
        manifestPlaceholders = [
            XG_ACCESS_ID : "1234567890",
            XG_ACCESS_KEY : "ABCDEFGHIJKL",
        ]
    }
}
```

<a id="section-60"></a>
## 2.4.4 { #section-60 }

<a id="section-60-unity"></a>
### Unity { #section-60-unity }

* Setting Toolがアップデートされました。
    * フォルダ構造が変更され、以前のバージョンのSettingToolを完全に削除した後、再インストールする必要があります。

<a id="section-61"></a>
## 2.2.2 { #section-61 }

<a id="section-61-unity"></a>
### Unity { #section-61-unity }

* GamebaseUnitySDKSettingsクラスの**storeCodeAOS**の変数名が **storeCodeAndroid**に変更されました。
    * **storeCodeAOS**を参照してStore Codeを定義するコードやPrefabがある場合は変数の参照に失敗するため、**storeCodeAndroid**変数に変更してください。

<a id="section-62"></a>
## 2.2.0 { #section-62 }

<a id="section-62-unity"></a>
### Unity { #section-62-unity }

* GamebaseMainActivityのPackage Nameが変更されました。
    * AndroidManifest.xmlのMainActivity宣言を下記のように変更していない場合、クラッシュが発生します。
    * **com.toast.gamebase.activity.GamebaseMainActivity** -> **com.toast.android.gamebase.activity.GamebaseMainActivity**

```xml
<manifest>
    ...
    <application>
    ...
        <activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
            android:launchMode="singleTask"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    ...
    </application>
    ...
</manifest>
```

<a id="section-63"></a>
## 2.1.0 { #section-63 }

<a id="section-63-common"></a>
### Common { #section-63-common }

<a id="section-63-common-removed-apis"></a>
#### Removed APIs

* 使用していないTransferKey機能を削除しました。
	* Guestアカウント移行機能が必要な場合は、Gamebase SDK 2.2.1から追加された[\[Game > Gamebase > Unity SDK使用ガイド > 認証 > TransferAccount\]](./unity-authentication/#transferaccount)機能を利用してください。
