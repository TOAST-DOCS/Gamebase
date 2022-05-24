## Game > Gamebase > Unity SDK ご利用ガイド > はじめる


Gamebase Unity SDKの使用環境及び初期設定について説明します。

## Environments

> [参考]
> 
> Unity対応バージョン
>
> * 2018.4.0～2021.2.19
> * 下位バージョンのUnityのサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。


#### Android
> <font color="red">[注意]</font>
>
> 2019年8月1日から、Google Playに公開する新規アプリは64bitアーキテクチャをサポートする必要があります。
> [Google Playポリシーおよび64bitをサポートするUnityバージョン確認](https://developer.android.com/distribute/best-practices/develop/64-bit#unity-developers)

#### Dependencies

| Gamebase SDK | External SDK |
| --- | --- |
| Gamebase | TOAST Unity SDK 0.25.4 |

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows7以上
	* MAC OSには対応しておりません。
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * 一部の機能のみ対応しています。

選択したプラットフォームで対応していないGamebaseAPIを呼び出すときは、次のようなエラーがコールバックで返され、コールバックがない場合、Warningログが出力されます。

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

APIごとの対応プラットフォームは、次のようなアイコンで区別します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

## Installation

Gamebase SDKを手軽にインストールすることができるように、Setting Toolを提供しています。

* [Download Gamebase Unity SDK](/Download/#game-gamebase)

### Specification of Setting Tool

1. SDKダウンロード
    * 最新バージョンのSDKのダウンロードに対応しています。
2. SDKインストール
    * ダウンロードされたSDKのインストールに対応しています。
        * Unity：Unitypackage
        * Android：Gradle
        * iOS：CocoaPods
3. SDK削除
    * インストールされたSDKの削除に対応しています。
4. SDKアップデート
    * アップデート機能には対応しておりません。
    * SDKを削除した後、再設定でアップデート機能を置き換えます。

### Using the Setting Tool

* Gamebase SettingTool **v2.0.0**が配布されました。 
    * 従来のv1.5.0とは互換性がありませんので、完全に削除してv2.0.0以上をご利用ください。
    
**AS-IS**

1. UnityプロジェクトにGamebase SDK for Android、iOSを含めてビルドを行います。
2. Gradle、CocoaPodsがサポートされません。

**TO-BE**

1. Gradle, CocoaPodsをサポートします。
2. EDM4U(External Dependency Manager for Unity)が必須ライブラリとして選ばれました。
    * [EDM4U Github](https://github.com/googlesamples/unity-jar-resolver)からEDM4Uをダウンロードしてインストールする必要があります。
    * EDM4Uがない場合はGamebase SDK for Android、iOSの設定ができません。
    * Facebook、GPGS SDK、Firebaseなど、EDM4Uがすでに含まれているSDKを使用する場合はEDM4Uをダウンロードする必要はありません。
3. Androidプラットフォームをサービスする場合にはメニュー上部 > **Assets > External Dependency Manager > Android Resolver > Settings**を選択してAndroid Resolver Settingsウィンドウを開き、以下のように設定してください。
    * Enable Auto-Resolution：無効
    * Explode AARs：無効
    * Patch mainTemplate.gradle：有効
    * Use Jetifier：有効
    * ![Android Resolver Settings](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-1_2.0.0.png)
4. iOSプラットフォームをサービスする場合はメニュー上部 > **Assets > External Dependency Manager > iOS Resolver > Settings**を選択してiOS Resolver Settingsウィンドウを開き、以下のように設定してください。
    * Use Shell to Execute Cocoapod Tool：無効
        * この機能が有効になっている場合、UnityでiOSビルドすると、xcworkspaceが作成されないエラーが発生します。 (CocoaPods 1.11.xバグ)
        * この機能を有効にする必要があるユーザーは、次の2つの方法のいずれかでエラーを解決してください。
            * CocoaPods 1.10.xバージョンをインストールします。
            * Unityで作成したXcodeプロジェクトから**pod install**を直接呼び出します。

> <font color="red">[注意]</font>
>
> iOSプラットフォームをサービスする場合は、CocoaPodsがインストールされている必要があります。CocoaPodsのインストールおよび詳細については[cocoapods.org](https://cocoapods.org/)を参照してください。

#### SDKインストール

1. Unityプロジェクトを開きます。
2. GamebaseUnitySettingTool_{version}.unitypackageをインポートします。
3. メニュー上部 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**を選択します。
4. SDK Download項目から[Gamebase SDK]ボタンをクリックして最新SDKをダウンロードします。
5. 使用するプラットフォームを選択します。
    * Android
    * iOS
6. プラットフォームごとに使用するモジュールを選択します。
    * authenticationは、Googleと同じID Provider(以下、IDP)との連携に対応しています。
    * pushは、FCM(Firebase)、Tencent Push、APNS Pushサービスに対応しています。
    * pruchaseは、NHN Cloudの決済サービスであるIAP(In-App Purchase)を使用して決済に対応しています。
7. [Settings]ボタンをクリックしてSDKをインストールします。
8. Android、iOSモジュールを選択した場合は、EDM4Uのresolveを実行する必要があります。
    * Android：メニュー上部 > **Assets > External Dependency Manager > Android Resolver > Force Resolve**を選択します。
    * Ios：メニュー上部 > **Assets > External Dependency Manager > iOS Resolver > install Cocoapods**を選択します。

> <font color="red">[注意]</font>
>
> EDM4Uがない場合は、Gamebase SDK for Android、Iosの設定ができません。<br/>
> EDM4Uのresolveを実行する前に、**Build Settings**ウィンドウでSwitch Platformボタンをクリックしてビルドするプラットフォームに切り替える必要があります。Androidプラットフォームが選択されている場合は、**Player Settings > Publishing Settings**でCustom Gradle Templateを有効にしてmainTemplate.gradleファイルを作成する必要があります。<br/>
> `Unity 2019.3以降を`使用時、**Player Settings > Publishing Settings**でCustom Gradle Properties Templateを有効にしてgradleTemplate.propertiesファイルを作成する必要があります。


#### SDKアップデート
1. メニュー上部 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**を選択します。
2. **SDK Download**項目から[Gamebase SDK]ボタンをクリックして最新SDKをダウンロードします。
    * すでに最新SDKがダウンロードされている場合はボタンが無効になります。
3. [Settings]ボタンクリックしてSDKをインストールします。
    * 既に選択したプラットフォーム別モジュールは変更が可能です。


#### SDK削除
1. メニュー上部 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**を選択します。
2. [Remove]ボタンをクリックしてインストールされたSDKを削除します。

<br/>
> [参考]
> 
> Setting Toolで予期しないエラーが発生した場合、ウィンドウを閉じて再度実行してください。 <br/>
> 再度実行してもエラーが解決しない場合は、**Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts**でSettingToolWindow.csファイルを開き、ShowWindowメソッドでSettingTool.SetDebugMode(true);コードをコメント解除した後、ログをお送りください。<br/><br/>
> Unity Facebook Authenticationを使用する場合、 Facebook Unity SDKは別途ダウンロードする必要があります。 [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> Unity Facebook Authenticationで対応しているFacebook Unity SDKバージョンについては、一緒に提供されるREADMEファイルをご参考ください。<br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Setting Tool Update

Setting Toolのアップデートが必要な場合、Setting Toolでアップデートするかどうかをお知らせします。
アップデートの種類によって、Setting Toolで提供する一部機能に制限がかかる場合があります。

#### 強制アップデート

* アップデート必須
* SDKダウンロード制限
	* 既にダウンロードしたSDKを利用してインストール、削除可能

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 選択アップデート

* アップデート選択
* SDKダウンロード可能

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

Lifecycle管理のために"com.toast.gamebase.activity.GamebaseMainActivity"をMainActivityにする必要があります。
"com.toast.gamebase.activity.GamebaseMainActivity"は、"com.unity3d.player.UnityPlayerActivity"を受け継いで設計されています。

> <font color="red">[注意]</font>
>
> AndroidPluginを開発する際にもGamebaseMainActivityを受け継いで制作しなければなりません。
> GamebaseMainActivityは、GamebasePlugin.jarに含まれています。 
> launchModeは、singleTaskにする必要があります。(Unityの基本ActivityもsingleTaskで固定されます。) そうでない場合、アプリを初めて始める際にクラッシュが発生することがあります。
>
> 該当Lifecycleを変更する時はProject Settings > Settings for Android > Publish Settings > Build > Custom Main Manifestを有効にして該当AndroidManifest.xmlに修正する必要があります。

> <font color="red">[注意]</font>
> 
> AndroidのtargetSdkVersionを31以上に設定する場合、intent-filterが存在するタグには必ずandroid:exported特性を宣言する必要があります。
> GamebaseでLifecycleを管理するために提供する**GamebaseMainActivity**をMainActivityに設定する時にも特性に**android:exported="true"**が追加されている必要があります。


```xml
<manifest>
	...
    <application>
    ...
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
        	android:launchMode="singleTask"
        	android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name">
            android:exported="true">
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

### iOS Settings

> <font color="red">[注意]</font>
>
> * Unity 2019.3以上の注意事項
>     * TARGETS > UnityFrameworkにiOS SDK設定を追加します。
>

1. UnityプロジェクトでiOSビルドを進行します。
2. 作成されたXCodeプロジェクトに設定を追加します。       
    * [iOS SDK設定ガイド](./ios-started)

## API Reference

API Referenceは、GamebaseUnitySDKの中に含まれています。

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebaseバージョン形式 - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 最低5ヶ月経過
