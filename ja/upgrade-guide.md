## Game > Gamebase > Upgrade Guide

## 2.27.0

### iOS

#### ImageNotice

* Unityで画像告知が表示されない問題を修正しました。
    * Gamebase iOS SDK 2.27.0未満を使用する場合、Unityで画像告知が表示されないことがあります。
    * 画像告知を使用する場合は、Gamebase iOS SDK 2.27.0以上を使用してください。

## 2.26.0

### Unity

* 該当バージョンを使用する時は`Assets/Gamebase/Toast/IAP/Plugins`を直接削除してから使用してください。

## 2.25.0

### Android

#### Changed Minimum Support Version

* 最小サポートAndroid Gradle Plugin(AGP)バージョンが2.3.0から3.2.0に変更されました。
    * しかしtargetSdkVersionを30以上に設定する場合、Android 11端末に対応するためにはAGP 3.3.3以上が必要です。
        * 次の文書を参照してください。
        * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Android 11](./aos-started/#android-11)
* 下位バージョンのAGPサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。

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

#### Line IdP

* Line IdPを使用する場合、Line SDK内部に**&lt;queries&gt;**タグが存在するため、AGPのバージョンによってはビルドが失敗することがあります。
    * 次のガイドを参考にして「queries」タグビルドが可能なAGPバージョンにアップグレードしてください。
    * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Android 11](./aos-started/#android-11)
* Line IdPを使用する場合、Line SDK内部に**android:allowBackup="false"**が宣言されており、アプリケーションビルド時にManifest mergerでfailが発生することがあります。ビルドが失敗する場合は、次のようにapplicationタグに**tools:replace="android:allowBackup"**宣言を追加してください。

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

### iOS

* Sign In with AppleのASAuthorizationErrorUnknownエラーが発生した場合、 TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR (3009)エラーをリターンするように変更されました。

### Unity

* 該当バージョンを使用する時は`Assets/Gamebase/Toast/IAP/Plugins`を直接削除してから使用してください。

#### Changed Minimum Support Version

* 最小サポートUnityバージョンが2017.4.16から2018.4.0に変更されました。
* 下位バージョンのUnityサポートが必要な場合は[サポート](https://toast.com/support/inquiry)へお問い合わせください。

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

### Unreal

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

## 2.21.2

### iOS

* Gamebase iOS SDK 2.21.1でbitcodeを有効にした後に**アーカイブビルド**を行うとエラーが発生します。
    * bitcodeを使用したい場合は、上記の問題が修正されたGamebase iOS SDK 2.21.2を使用してください。

## 2.21.0

### Android

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

#### Line IdP

* Line IdPを使用する場合、Line SDKのアップデートにより以下のようにGradleに**JavaVersion.VERSION_1_8**を設定していない場合はビルドが失敗します。

```groovy
android {
    compileOptions {
        // >>> [Line IdP]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

### iOS

* Gamebase iOS SDK 2.21.0でbitcodeを使用する場合はエラーが発生します。
    * bitcodeを使用したい場合はGamebase iOS SDK 2.21.1を使用してください。

## 2.20.2

### iOS

#### Facebook IdP

* Gamebase iOS SDK 2.20.2でFacebook SDKが9.1.0にアップデートされました。 
    * Facebook SDKに追加設定が必要なため、info.plistに以下の値を追加してください。設定しない場合はクラッシュが発生する場合がります。
        * FacebookAutoLogAppEventsEnabled
        * FacebookAdvertiserIDCollectionEnabled
* 詳細な内容は[Facebook iOS SDKガイド](https://developers.facebook.com/docs/app-events/getting-started-app-events-ios)を参照してください。

## 2.19.0

### Android

#### Weibo IdP

* Gamebase Android SDK 2.19.0でWeibo IdPログインと別のIdPログインを交互に呼び出すとクラッシュが発生します。
    * Weibo IdPを使用する場合は問題が修正されたGamebase Android SDK 2.19.1を使用してください。

## 2.18.2

### Android

#### Removed APIs

* Gamebase Android SDK 2.6.0でdeprecatedとなっていた以下の関数が削除されました。
    * **GamebaseConfiguration.Builder.setFCMSenderId()**
    * **GamebaseConfiguration.Builder.setTencentAccessKey()**
    * **GamebaseConfiguration.Builder.setTencentAccessId()**

## 2.18.0

### Android

#### Purchase Google

* Gamebase Android SDK 2.18.0でGoogleアイテム決済を呼び出すとクラッシュが発生します。
    * 問題が修正されたGamebase Android SDK 2.18.1を使用してください。

## 2.17.0

### Android

* Gamebase Android SDK 2.17.0でGamebase.ImageNotice.showImageNotices APIを呼び出すとクラッシュが発生します。
    * 2.17.0のクラッシュおよびOS 5.0～6.0でカスタムスキームイベントが動作しない問題が修正されたGamebase Android SDK 2.17.4を使用してください。

## 2.15.1

### iOS

* SDKで定義したタイプ **GamebaseEventCategory**をNSStringの代わりに使用する場合、該当タイプを **TCGBGamebaseEventCategory**に修正する必要があります。

## 2.15.0

### Android

#### Purchase Google

* **gamebase-adapter-purchase-google**を使用する場合、Gamebase SDK 2.15.0未満バージョンから2.15.以上にアップグレードする場合は、必ず**以前のバージョンのGame Client Versionをアップデート必須**に設定する必要があります。
	* Google Billing Clientモジュールがアップデートされ、複数の端末で別々のBilling Clientバージョンが適用された状態でアイテムを購入する場合、エラーが発生した時の再処理に問題が発生することがあるためです。

## 2.6.0

### Unity

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

#### Firebase Push

* Firebase Cloud Messagingを使用している場合Firebaseコンソールからgoogle-services.jsonファイルをダウンロードし、xmlリソースに変換してプロジェクトに追加しないとPushが正常に動作しません。
    * Gamebase 2.5.0 以前のバージョンではxmlリソースなしてPush動作に問題ありませんでしたが、Gamebase 2.6.0からは必ずxmlリソースが必要です。
* 下のガイドを参考にしてPush設定を行ってください。
    * [\[Game > Gamebase > Android SDK ご利用ガイド > Push > Settings > Firebase\]](./aos-push/#firebase)

#### Standalone

* Removed Japan Purchase
	* 日本決済がFadeOutしました。
	* GamebaseUnitySDK_IAPAdapterを使用している場合は、下記のフォルダを直接削除してください。
        * Asset/Toast/Common
        * Asset/Toast/Core
        * Asset/Toast/IAP
        * Asset/Toast/Standalone

### Android

#### Limitation

* minSdkVersionが15(IceCreamSandwichMR1, 4.0.3)から16(JellyBean, 4.1)に変更されました。
	* OS 4.1未満の端末では正常な動作を保障しないため、プロジェクトのminSdkVersionが15の場合、16に変更してください。

#### Removed APIs

* 削除された関数は次のとおりです。代替関数に変更してください。
    * **Gamebase.getAuthBanInfo()**を削除しました。**Gamebase.getBanInfo()**に変更してください。
    * **Gamebase.getLanguageCode()**を削除しました。**Gamebase.getDeviceLanguageCode()**に変更してください。
    * **new GamebaseConfiguration.Builder(void)**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。
    * **new GamebaseConfiguration.Builder.setAppId()**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。
    * **new GamebaseConfiguration.Builder.setAppVersion()**を削除しました。**GamebaseConfiguration.newBuilder()**に変更してください。

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

## 2.4.4

### Unity

* Setting Toolがアップデートされました。
    * フォルダ構造が変更され、以前のバージョンのSettingToolを完全に削除した後、再インストールする必要があります。

## 2.2.2

### Unity

* GamebaseUnitySDKSettingsクラスの**storeCodeAOS**の変数名が **storeCodeAndroid**に変更されました。
    * **storeCodeAOS**を参照してStore Codeを定義するコードやPrefabがある場合は変数の参照に失敗するため、**storeCodeAndroid**変数に変更してください。

## 2.2.0

### Unity

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

## 2.1.0

### Common

#### Removed APIs

* 使用していないTransferKey機能を削除しました。
	* Guestアカウント移行機能が必要な場合は、Gamebase SDK 2.2.1から追加された[\[Game > Gamebase > Unity SDK使用ガイド > 認証 > TransferAccount\]](./unity-authentication/#transferaccount)機能を利用してください。
