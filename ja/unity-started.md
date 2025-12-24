## Game > Gamebase > Unity SDK ご利用ガイド > はじめる


Gamebase Unity SDKの使用環境及び初期設定について説明します。

## Environments

> [参考]
> 
> Unity対応バージョン
>
> * 2022.3.10f1 ~ 6000.2.15f1

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows 7以上
    * macOS 10.15以上
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * 一部の機能のみ対応しています。

## Gamebase SDK SettingTool

SettingToolを使用して、Gamebase SDKを簡単にインストールできます。

### Specification of Setting Tool

1. Gamebase SDKのインストール
2. Gamebase SDKのアップデート
3. Gamebase SDKの設定管理
4. Gamebase SDKのアンインストール

### SettingToolのインストール

1. SettingToolをダウンロードします。
     * [Download Gamebase Setting Tool](/Download/#game-gamebase)
2. Unityプロジェクトを開き、GamebaseUnitySettingTool_{version}.unitypackageファイルをインポートします。

### SettingToolの使用

Unityエディタの上部メニューバーで**Tools > Gamebase**を選択してSettingToolの機能を使用できます。

![unity-developers-guide-started-settingtool-3.0.0-menu](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-menu.png)

1. Setup Wizard
    * Gamebase SDKのインストールを段階的に進めます。
2. Update Latest Version
    * Gamebase SDKを簡単に最新バージョンにアップデートできます。
3. Customize...
    * Gamebase SDKを自由にインストールおよび編集できます。
4. Refresh SettingTool
    * SettingToolのデータを更新します。

## Gamebase SDKのインストール

**Tools > Gamebase > Setup Wizard**メニューを選択します。

1. プラットフォーム、認証、決済、プッシュなどの機能を順番に選択してインストールを進めます。
2. インストールする機能を選択後、**インストール**ボタンをクリックしてインストールを実行します。

![unity-developers-guide-started-settingtool-3.0.0-setupwizard](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-setupwizard-ja.png)

> [参考]
>
> * [Required Settings](#required-settings)を通じてインストールに**必須設定**を確認できます。

## SDKの最新バージョンのアップデート

**Tools > Gamebase > Update Latest Version**メニューを選択します。

1. 最新バージョンにアップデートできる場合、最新バージョンと**最新バージョンアップデート**ボタンが表示されます。
2. 右下の**最新バージョンアップデート**ボタンをクリックして、最新SDKにアップデートします。

![unity-developers-guide-started-settingtool-3.0.0-latestupdate](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-latestupdate-ja.png)

## Gamebase SDKの機能編集

**Tools > Gamebase > Customize...**メニューを選択します。

1. 必要な設定を自由に選択してインストールを進めます。
2. 右下の**設定適用**ボタンをクリックすると、その設定でインストールが進行します。

![unity-developers-guide-started-settingtool-3.0.0-customize](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-customize-ja.png)

> [参考]
>
> * [Required Settings](#required-settings)を通じてインストールに**必須設定**を確認できます。

## Required Settings

SettingToolでは**必須設定**を確認および修正できるUIを提供しています。 
**Setup Wizard**と**Customize**メニューで確認でき、すべての設定が完了すると、該当項目は消えます。

> <font color="red">[注意]</font>
> * 必須設定を解決しないと、実行またはビルド時に**エラーが発生**する可能性があります。

### EDM4Uのインストール

Android、iOSプラットフォームを使用する場合、[EDM4U(External Dependency Manager)](https://github.com/googlesamples/unity-jar-resolver)が必要です。

> [参考]
>
> * プロジェクトにEDM4Uがインストールされていない場合, まず [ダウンロード](https://github.com/googlesamples/unity-jar-resolver/raw/refs/heads/master/external-dependency-manager-latest.unitypackage)してからUnityPackageファイルをインポートしてインストールします。

### Android Publishing Settings

GamebaseのAndroid SDK設定を行うために、必要なファイルを生成する必要があります。

1. Android Player Settingを開きます。
    * **Player Settings > Player > Android**
2. **Publishing Settings**で必要なファイルを生成します。
    * AndroidManifest.xml: 生成
        * **Custom Main Manifest**: 有効
    * mainTemplate.gradle: 生成
        * **Custom Gradle Template**: 有効
    * gradleTemplate.properties: 生成
        * **Custom Gradle Properties Template**: 有効

### Android Activity設定

Android Lifecycle管理のため、Gamebaseが提供するActivityをMainActivityとして設定する必要があります。

Application Entry Pointにより、設定するMainActivityが異なります。

* Unity 2022以下の場合
    1. Application Entry Point設定はありません。値がActivityのように動作します。
    2. AndroidManifest.xmlでMainActivityを設定します。
        * com.toast.android.gamebase.activity.GamebaseMainActivity
* Unity 2023以上の場合
    1. Android Player Settingを開きます。
        * **Player Settings > Player > Android**
    2. Application Entry Point設定を確認または設定します。
        * **Other Settings > Application Entry Point**
            * ![unity-developers-guide-started-settingtool-application-entry-point](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-application-entry-point.png)
    3. 設定されたApplication Entry Pointに従い、適切なMainActivityを設定します。
        * Activityを有効化した場合
            * com.toast.android.gamebase.activity.GamebaseMainActivity
        * GameActivityを有効化した場合
            * com.toast.android.gamebase.activity.GamebaseMainGameActivity

> <font color="red">[注意]</font>
>
> * MainActivityは必ずGamebaseが提供するActivityを使用するか、継承する必要があります。

### AndroidManifest.xml設定

```xml
<manifest>
    ...
    <application>
        ... 
        <!-- 2022以下またはActivityの場合 -->
        <!-- android:exportedが無い場合、追加が必要 -->
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
                  android:exported="true" 
                ...>
            ...
        </activity>

        ... 
        <!-- 2023以上の場合GameActivity -->
        <!-- android:exportedが無い場合、追加が必要 -->
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainGameActivity"
                  android:exported="true" 
                ...>
            ...
        </activity>
        ...
    </application>
    ...
</manifest>
```

### Android EDM4U設定

**Assets > External Dependency Manager > Android Resolver > Settings > Android Resolver Settings**

* Android Resolver Settings
    * Enable Auto-Resolution: 無効
    * Explode AARs: 無効
    * Patch mainTemplate.gradle: 有効
    * Patch gradleTemplate.properties: 有効
    * ![unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182.png)

### Android EDM4U手動Resolve

Setting ToolでGamebase SDKをインストールする際に、EDM4Uがインストールされている場合はResolveが自動で実行され設定されます。

EDM4Uがインストールされていない場合や別途変更が必要な場合は、手動でResolveを実行できます。

**Assets > External Dependency Manager > Android Resolver > Force Resolve**

### iOS CocoaPodsインストール

iOSプラットフォームをサービスする場合、CocoaPodsがインストールされている必要があり、インストール方法については[CocoaPods公式サイト](https://cocoapods.org/)を参照してください。

EDM4UからCocoaPodsをインストールすることもできます。

**Assets > External Dependency Manager > iOS Resolver > install Cocoapods**

### iOS EDM4U設定

> **Assets > External Dependency Manager > iOS Resolver > Settings > iOS Resolver Settings**

* iOS Resolver Settings
    * Use Shell to Execute Cocoapod Tool: 無効
        * この機能が有効になっている場合、UnityでiOSビルドすると、xcworkspaceが作成されないエラーが発生します。 (CocoaPods 1.11.xバグ)
        * この機能を有効にする必要があるユーザーは、次の2つの方法のいずれかでエラーを解決してください。
            * CocoaPods 1.10.xバージョンをインストールします。
            * Unityで作成したXcodeプロジェクトから**pod install**を直接呼び出します。
    * Link frameworks statically: 無効
    * ![unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182.png)

#### iOSモジュールごとの追加設定

* 選択したモジュールに応じてXcodeで直接設定を行う必要があります。

1. UnityプロジェクトでiOSビルドを行います。
2.生成されたXcodeプロジェクトを開きます。
3. TARGETS > UnityFrameworkにiOS SDK設定を追加します。
    * [iOS SDK設定ガイド](./ios-started)

## エラー発生時

* Setting Toolで予期しないエラーが発生した場合、ウィンドウを閉じて再度実行してください。
* 再度実行してもエラーが解決しない場合は、Assets/NhnCloud/GamebaseTools/SettingTool/Editor/ScriptsでSettingToolWindow.csファイルを開き、ShowWindowメソッドでSettingTool.SetDebugMode(true);コードをコメント解除した後、ログをお送りください。

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

## API Supported Platforms

APIごとの対応プラットフォームは、次のようなアイコンで区別します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE_WIN
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE_OSX
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

選択したプラットフォームで対応していないGamebaseAPIを呼び出すときは、次のようなエラーがコールバックで返され、コールバックがない場合、Warningログが出力されます。

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebaseバージョン形式 - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 最低5ヶ月経過
