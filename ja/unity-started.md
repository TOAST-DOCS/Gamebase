## Game > Gamebase > Unity SDK ご利用ガイド > はじめる

Gamebase Unity SDKの使用環境及び初期設定について説明します。

### Environments

> [参考]
> 
> Unity対応バージョン
> 
> * Unity 5.x : 5.6.6
> * Unity 2017.x : ~ 2017.4.x(LTS)
> * Unity 2018.x : ~ 2018.4.x(LTS)
> * Unity 2019.x : ~ 2019.1.10

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
    * 最新バージョンのダウンロードに対応しています。
2. SDKインストール
    * ダウンロードされたSDKのインストールに対応しています。
3. SDK削除
    * インストールされたSDKの削除に対応しています。
4. SDKアップデート
    * アップデート機能には対応しておりません。
    * 削除後のインストールがアップデート機能の代わりに使われます。

### Using the Setting Tool

#### SDKインストール
1. Unityプロジェクトを開きます。
2. GamebaseUnitySettingTool_{version}.unitypackageをインポートします。
3. Menu > Tools > Gamebase > SDKSettings > Setting Toolを起動します。
	* v1.0.1以下 : Menu > Gamebase > SDKSettings > Setting Tool
4. [Download SDK]ボタンをクリックしてSDKをダウンロードします。
5. 利用するプラットフォームを選択します。
    * Android
    * iOS
6. プラットフォームごとに使用するモジュールを選択します。
    * Authenticationは、Googleと同じID Provider(以下、IDP)との連携に対応しています。
    * Pushは、FCM(Firebase)、Tencent Push、APNS Pushサービスに対応しています。
    * Pruchaseは、TOASTの決済サービスであるIAP(In-App Purchase)を使用して決済に対応しています。
7. [Settings]ボタンをクリックしてSDKをインストールします。

#### SDK削除
1. Menu > Tools > Gamebase > SDKSettings > Setting Toolを起動します。
	* v1.0.1以下 : Menu > Gamebase > SDKSettings > Setting Tool
2. [Remove]ボタンをクリックしてインストールされたSDKを削除します。

<br/>
> [参考]
> 
> Setting Toolで不測のエラーが発生する場合、ウィンドウを閉じてからもう一度起動してください。<br/>
> Unity Facebook Authenticationを使用する場合、Facebook Unity SDKは、別途ダウンロードする必要があります。[Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> Unity Facebook Authenticationで対応しているFacebook Unity SDKバージョンについては、一緒に提供されるREADMEファイルをご参考ください。<br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Update of Setting Tool

Setting Toolのアップデートが必要な場合、Setting Toolでアップデートするかどうかをお知らせします。
アップデートの種類によって、Setting Toolで提供する一部機能に制限がかかる場合があります。

#### 強制アップデート

* アップデート必須
* SDKダウンロード制限
	* 既にダウンロードしたSDKを利用してインストール、削除可能

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 選択アップデート

* アップデート選択
* SDKダウンロード可能

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

Lifecycle管理のために"com.toast.gamebase.activity.GamebaseMainActivity"をMainActivityにする必要があります。
"com.toast.gamebase.activity.GamebaseMainActivity"は、"com.unity3d.player.UnityPlayerActivity"を受け継いで設計されています。

> <font color="red">[注意]</font>
>
> AndroidPluginを開発する際にもGamebaseMainActivityを受け継いで制作しなければなりません。<br/>
> GamebaseMainActivityは、GamebasePlugin.jarに含まれています。 <br/>
> launchModeは、singleTaskにする必要があります。(Unityの基本ActivityもsingleTaskで固定されます。) そうでない場合、アプリを初めて始める際にクラッシュが発生することがあります。


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

### Android Gradle Build Settting

> [参考]
>
> Gradle buildは、必須ではなく選択です。<br/>
> 該当する内容は、Gradleビルドの例です。<br/>
> 該当する内容は、Android projectへエクスポートする(export)方法ではありません。<br/>
> [Unity manual](https://docs.unity3d.com/Manual/android-gradle-overview.html)

#### 1. Build Settings
Build SystemにGradle(new)を選択します。
![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-003_1.3.0.png)

#### 2. mainTemplate.gradleファイルの設定
UnityインストールフォルダにあるmainTemplate.gradleのファイルをAssets/Plugins/Android/フォルダへコピーした後、開発環境に合わせて設定します。

> [参考]
>
> **compileSdkVersion、buildToolsVersion、targetSdkVersion**は、環境に合わせて設定してください。<br/>
> **applicationId**は、package nameを設定してください。<br/>
> **dependencies**リストには、使用するlibraryファイルを追加してください。<br/>
> **multiDexEnabled**をtrueに設定します。<br/>
> **storeFile**は、keystore pathを設定してください。<br/>
> **storePassword**は、keystore passwordを設定してください。<br/>
> **keyAlias**は、keystore aliasを設定してください。<br/>
> **keyPassword**は、keystore alias passwordを設定してください。<br/>

```groovy
// GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
buildscript {
	repositories {
		jcenter()
	}

	dependencies {
		classpath 'com.android.tools.build:gradle:2.1.0'
	}
}

allprojects {
   repositories {
      flatDir {
        dirs 'libs'
      }
   }
}

apply plugin: 'com.android.application'

dependencies {
	compile fileTree(dir: 'libs', include: ['*.jar'])
	compile(name: 'animated-vector-drawable-24.0.0', ext:'aar')
	compile files('libs/bolts-applinks-1.4.0.jar')
    compile project('gamebase-sdk')
    ....
}

android {
	compileSdkVersion 23
	buildToolsVersion '25.0.0'

	defaultConfig {
		targetSdkVersion 23
		applicationId 'package name'

		// Enabling multidex support.
        multiDexEnabled true
	}

	lintOptions {
		abortOnError false
	}

	aaptOptions {
		noCompress '.unity3d', '.ress', '.resource', '.obb'
	}

	signingConfigs { release {
		storeFile file('keysotre path')
		storePassword 'keystore password'
		keyAlias 'keystore alias'
		keyPassword 'keystore alias password'
	} }

	buildTypes {
		debug {
			jniDebuggable true
		}
		release {
			// Set minifyEnabled to true if you want to run ProGuard on your project
			minifyEnabled false
			proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-unity.txt'
			signingConfig signingConfigs.release
		}
	}
}
```


### iOS Settings

1. UnityプロジェクトでiOSのビルドを進めます。
2. 作成されたXCodeプロジェクトに設定を追加します。

iOS SDKに対する設定は、次のガイドをご参考ください。

* [iOS SDK設定リンク](./ios-started)

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
