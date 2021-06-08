## Game > Gamebase > Android SDK ご利用ガイド > はじめる

## Environments

AndroidでGamebaseを利用するためのシステム環境は、次の通りです。

> [最小仕様]
>
> * Android API 16 (JellyBean, 4.1)以上
>     * Twitter Loginは19(Kitkat, 4.4)以上
>     * AppleID Loginは19(Kitkat, 4.4)以上
>     * Line Loginは17(Kitkat, 4.2)以上
>     * Weibo Loginは19(Kitkat, 4.4)以上
>     * GALAXY Storeは21(Lollipop, 5.0)以上
>         * ギャラクシーIAP SDKのminSdkVersionは18(OS 4.3)のため、これより小さい値を設定する場合、ビルドが失敗します。
>         * しかし、実際に決済を行うにはCheckoutサービスアプリのインストールが必要ですが、ChekcoutサービスアプリはAPI 21(OS 5.0. Lollipop)未満ではインストールが失敗するため、決済を進行できません。
> * Gradle Android Plugin 2.3.0 以上
> * 開発環境:Android Studio

## Setting

### Console

> <font color="red">[注意]</font><br/>
>
> * NHN Cloud Consoleで新しいプロジェクトを作成してGamebaseサービスが有効になっていることを必ず確認してください。
> * 各IdPのコンソールでClient IDを発行してGamebaseコンソールに入力していることを必ず確認してください。

* Gamebase Android SDKを使用する前に、NHN Cloud ConsoleからアプリIDを発行する必要があります。アプリIDを発行するためには、NHN Cloud Consoleから**(+)サービス選択**をクリックし、**Game > Gamebase**をクリックしてサービスを有効にします。
* 認証するためにIdPコンソールでclient idを発行し、Gamebaseコンソールに入力します。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information](./oper-app/#authentication-information)
* アイテムを購入するためにStoreコンソールでアプリ情報を登録してGamebase > 購入(IAP)コンソールに入力します。
	* [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド](./console-google-guide)
	* [Game > Gamebase > ストアコンソールガイド > ONEStoreコンソールガイド](./console-onestore-guide)
	* [Game > Gamebase > ストアコンソールガイド > GALAXY Storeコンソールガイド](./console-galaxy-guide)
    * 以下のガイドを参考にしてアイテムを登録します。
        * [Game > Gamebase > コンソール使用ガイド > 決済 > Register](./oper-purchase/#register_1)
* プッシュ通知を行うためにプッシュ通知サービス証明書をGamebase > Push > 証明書コンソールに入力します。
    * [Game > Gamebase > Console ご利用ガイド > Push > Authentication > Authentication register](./oper-push/#authentication-register)
* 新しいGamebaseプロジェクトが作成されたのでAppVersionとStoreCodeを登録する必要があります。
    * 次のガイドに従って新しいクライアントバージョンを登録してください。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Client > Client List](./oper-app/#client-list)

### Regist as Tester

#### Gamebase Test Device

* メンテナンス中も正常にゲームにアクセスしたい場合は、Gamebaseコンソールにテスト端末を登録します。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Test Device](./oper-app/#test-device)

#### Store's Tester

* 決済テストを行うために、ストアごとに次のようにテスターとして登録します。(Gamebase Tester登録ではなく、ストアのテスト決済を行うための設定です。)
    * Google
        * [Android > テスト購入設定](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > APPS > 商品状況 > In-App情報 > 決済テスト > テストID登録/管理](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY store
        * [GALAXY store > アプリ > 登録したアプリ > バイナリ > Beta Test > Tester設定](https://seller.samsungapps.com/application)
        * サムソン端末でのみ決済テストが可能です。

### Gradle

* 使用するGamebaseバージョン、使用する認証、決済、プッシュモジュールをbuild.gradleファイルで宣言してください。
	* Gamebaseの最新バージョンは[Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/)で確認できます。
	* `mavenCentral()`保存場所を追加してください。
    * Android Studioでビルドする場合、kotlin-gradle-pluginのために**'maven { url "https://plugins.gradle.org/m2/" }'**保存場所を追加する必要があります。

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    
    //  >>> For the 'kotlin-gradle-plugin'
    maven { url "https://plugins.gradle.org/m2/" }

    ...
    
    //  >>> For the 'kotlin-gradle-plugin'
    maven { url "https://plugins.gradle.org/m2/" }

    // >>> [Weibo IdP]
    // Download weibo sdk from here:
    // https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/2019SDK/aar
    flatDir { dirs 'The directory containing weibo sdk.' }

    // >>> [Hangame IdP]
    maven { url 'Hangame IdPの設定方法は、サポートへお問い合わせください。' }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'

    // >>> Gamebase - Add Auth Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-appleid:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"

    // >>> [Weibo IdP]
    implementation (name: 'openDefault-10.10.0', ext: 'aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
}

android {
    compileOptions {
        // >>> [Line IdP]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        // >>> [Weibo IdP]
        ndk {
            abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a'
        }
    }
    
    // >>> If your AGP(Android Gradle Plugin) version is lower than 3.4.0,
    //     the build will fail due to duplicate META-INF resources.
    //     In this case, please add the following declaration.
    packagingOptions {
        // >>> Avoid duplication of 'coroutines.pro' from kotlinx-coroutines
        exclude 'META-INF/proguard/coroutines.pro'

        // >>> Avoid duplication of 'LICENSE.txt' from jackson
        exclude 'META-INF/DEPENDENCIES.txt'
        exclude 'META-INF/LICENSE.txt'
        exclude 'META-INF/NOTICE.txt'
        exclude 'META-INF/NOTICE'
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/DEPENDENCIES'
        exclude 'META-INF/notice.txt'
        exclude 'META-INF/license.txt'
        exclude 'META-INF/dependencies.txt'
        exclude 'META-INF/LGPL2.1'
    }
}
```

### NDK Library

#### Weibo IdP

* 次のパスからWeibo soライブラリをコピーしてapp/src/main/jniLibsフォルダに追加する必要があります。
    * [https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so](https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so)
    * 必要なプラットフォームのみ使用してください。

### Resources

#### Firebase Notification

* Android Studioビルドの場合
    * Firebaseプッシュを使用するには、以下のガイドに従ってFirebaseの設定を完了した後、google-services.jsonファイルをプロジェクトに含める必要があります。
		* [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud Push > Android > Firebase Cloud Messaging設定](https://docs.toast.com/ja/TOAST/ja/toast-sdk/push-android/#firebase-cloud-messaging)
* Unityビルドの場合
    * Firebase Unity SDK Packageをインストールした場合は、以下のコマンドで**generate_xml_from_google_services_json.exe**ファイルを実行してjsonファイルをxmlファイルに変換できます。
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
    * Firebase Unity SDK Packageをインストールしていない場合は、「Firebase Console > プロジェクト設定」からgoogle-services.jsonファイルをダウンロードして、以下のガイドに従ってstring resource(xml)ファイルを直接作って「Assets/Plugins/Android/res/values/」フォルダに含める必要があります。
        Firebaseサービスの連携によってgoogle-services.jsonファイルの内容は異なる場合があります。
        ![Download google-services.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 次は、直接作成したstring resource(xml)ファイルの例です。

```xml
<!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="firebase_database_url" translatable="false">https://gamebase-sample-00000000.firebaseio.com</string>
    <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
    <string name="google_storage_bucket" translatable="false">gamebase-sample-00000000.appspot.com</string>
    <string name="project_id" translatable="false">gamebase-sample-00000000</string>
    <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
    <string name="google_app_id" translatable="false">1:000000000000:android:abcd0123abcd0123</string>
    <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
</resources>
```

### AndroidManifest.xml

#### Facebook IdP

* Facebook SDKを初期化するためにApp IDを宣言します。
    * 値を直接宣言するよりは、以下の例のようにresourcesを参照するように設定するのが良いでしょう。
    * Gamebase SDKが内部的にFacebook SDK初期化関数を呼び出しているため、現在は必須設定ではありません。

**AndroidManifest.xml**

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [Facebook] Configurations begin -->
        <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id" />
        <!-- [Facebook] Configurations end -->
        ...
    </application>
</manifest>
```

**res/values/strings.xml**

```xml
<resources>
    <!-- [Facebook] Facebook APP ID -->
    <string name="facebook_app_id">123456789012345</string>
</resources>
```

#### Line IdP

* Line SDKの内部に**android:allowBackup="false"**が宣言されており、アプリケーションビルド時にManifest mergerでfailが発生することがあります。ビルドが失敗した場合は、次のようにapplicationタグに**tools:replace="android:allowBackup"**宣言を追加してください。

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

#### Hangame IdP

* Hangame IdPが正常に動作するためのAndroidManifest.xmlの設定は、サポートへお問い合わせください。

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [Hangame] Configurations begin -->
        <meta-data ... />
        <!-- [Hangame] Configurations end -->
        ...
    </application>
</manifest>
```

#### Weibo IdP

* Weibo IdPが正常に動作するには、**application**タグに**android:networkSecurityConfig** attributeを追加し、weibo、sina関連URLを宣言したxmlファイル名を設定する必要があります。

```xml
<application android:networkSecurityConfig="@xml/my_network_security_config"
    ...>
    ...
</application>
```

* res/xml/my_network_security_config.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <!-- Weibo SDK start -->
        <domain includeSubdomains="true">sina.cn</domain>
        <domain includeSubdomains="true">weibo.cn</domain>
        <domain includeSubdomains="true">weibo.com</domain>
        <domain includeSubdomains="true">sinaimg.cn</domain>
        <domain includeSubdomains="true">sinajs.cn</domain>
        <domain includeSubdomains="true">sina.com.cn</domain>
        <!-- Weibo SDK end -->
    </domain-config>
</network-security-config>
```

#### ONE Store

* ONE storeは、全体決済画面とポップアップ決済画面をサポートします。
    * AndroidManifest.xmlにmeta-dataを追加して全体決済画面("full")またはポップアップ決済画面("popup")を選択できます。
    * meta-dataを設定しない場合はデフォルト値("full")が適用されます。

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup：ポップアップ決済画面 / full：全体決済画面 -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| 決済画面 | 設定値 |
| --- | --- |
| 全体決済画面 | "full" |
| ポップアップ決済画面 | "popup" |

#### Notification Options

* 次のような方法で通知オプションを設定できます。

**Example**

```xml
<!-- Notification priority -->
<meta-data android:name="com.toast.sdk.push.notification.default_priority"
           android:value="1"/>
<!-- Notification background color -->
<meta-data android:name="com.toast.sdk.push.notification.default_background_color"
           android:resource="@color/defaultNotificationColor"/>
<!-- LED light -->
<meta-data android:name="com.toast.sdk.push.notification.default_light_color"
           android:value="#0000ff"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_on_ms"
           android:value="0"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_off_ms"
           android:value="500"/>
<!-- Small icon -->
<meta-data android:name="com.toast.sdk.push.notification.default_small_icon"
           android:resource="@drawable/ic_notification"/>
<!-- Sound -->
<meta-data android:name="com.toast.sdk.push.notification.default_sound"
           android:value="notification_sound"/>
<!-- Vibrate pattern -->
<meta-data android:name="com.toast.sdk.push.notification.default_vibrate_pattern"
           android:resource="@array/default_vibrate_pattern"/>
<!-- Use badge icon or not -->
<meta-data android:name="com.toast.sdk.push.notification.badge_enabled"
           android:value="true"/>
<!-- Enable notification when application is foreground. -->
<meta-data android:name="com.toast.sdk.push.notification.foreground_enabled"
           android:value="false"/>
```

| meta-data key | value type | description |
| ------------- | ---------- | ----------- |
| com.toast.sdk.push.notification.default_priority | int | 優先順位。<br/>以下の5つの値を設定できます。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 背景色。 |
| com.toast.sdk.push.notification.default_light_color | int | LED色。 |
| com.toast.sdk.push.notification.default_light_on_ms | int | LEDが点灯する時の時間。 |
| com.toast.sdk.push.notification.default_light_off_ms | int | LEDが消える時の時間。 |
| com.toast.sdk.push.notification.default_small_icon | resource id | 小さいアイコンのリソース識別子。 |
| com.toast.sdk.push.notification.default_sound | String | 通知音ファイル名。<br/>Android 8.0未満のOSでのみ動作します。<br/>「res/raw」フォルダのmp3、wavファイル名を指定すると、通知音が変更されます。 |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 振動のパターン。 |
| com.toast.sdk.push.notification.badge_enabled | boolean | バッジアイコンの使用有無。 |
| com.toast.sdk.push.notification.foreground_enabled | boolean | フォアグラウンド通知の使用有無。 |

### Android 11

* Android 11は、ビルド時にあらかじめ宣言されたアプリケーションでなければ、他のアプリケーションが実行されません。
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* そのため、targetSdkVersionを30以上に設定する場合には必ずAndroidManifest.xmlに**queries**タグを利用して許可するアプリケーションをあらかじめ宣言しておく必要があります。

> <font color="red">[注意]</font><br/>
>
> * 'queries'タグはGradle 5.6.4以上のバージョンでのみビルドが可能です。
>     * そのためIDEでGradle 5.6.4以上がサポートされない環境ではtargetSdkVersionを29以下に設定する必要があります。
> * Gradle 5.6.4以上のバージョンが適用されたIDEは次のとおりです。
>     * Android Studio：3.6.以上
>     * Unity：2020.1以上
>     * Unreal：サポート不可

```xml
<queries>
    <!-- [Facebook] Configurations begin -->
    <package android:name="com.facebook.katana" />
    <!-- [Facebook] Configurations end -->

    <!-- [Payco/Hangame] Configurations begin -->
    <package android:name="com.nhnent.payapp" />
    <!-- [Payco/Hangame] Configurations end -->

    <!-- [Line] Configurations begin -->
    <package android:name="jp.naver.line.android" />
    <intent>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="https" />
    </intent>
    <!-- [Line] Configurations end -->

    <!-- [ONE store] Configurations begin -->
    <intent>
        <action android:name="com.onestore.ipc.iap.IapService.ACTION" />
    </intent>
    <intent>
        <action android:name="android.intent.action.VIEW" />
        <data android:scheme="onestore" />
    </intent>
    <!-- [ONE store] Configurations end -->

    <!-- [Galaxy store] Configurations begin -->
    <package android:name="com.sec.android.app.samsungapps" />
    <!-- [Galaxy store] Configurations end -->
</queries>
```

### Proguard

* Gamebase 2.21.0未満のバージョンはProguard適用時、Proguard Ruleに次の宣言を追加していない場合、決済API呼び出し時にクラッシュが発生します。
    * Gamebase 2.21.0バージョンで修正されました。

```
# ---------------------- [Gamebase TOAST IAP] defines start ----------------------
# For using reflection
-keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
# ---------------------- [Gamebase TOAST IAP] defines end ----------------------
```

## Recommended Flow

* Gamebaseで推奨するflowは、Sample Projectにも同じように実装されています。
    * Android Sample Project
        * 以下のリンクのGamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/ja/Download/#game-gamebase)
            * GamebaseManager.javaファイルを参考にしてください。
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* ゲームが始まった時、GamebaseクライアントSDKを初期化し、ログインが成功したら決済の再処理が始まるように実装してください。

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.19.0.png)

* 詳細flowは、次のリンクで確認できます。
    * [Game > Gamebase > Android SDK使用ガイド > 初期化 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK使用ガイド > 認証 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK使用ガイド > 決済 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [Payco Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
* [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/blob/master/2019SDK/文档)

## API Reference

* API Referenceは、SDKの中に含まれています。

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebaseバージョン形式 - XX.YY.ZZ
		* XX：Major
		* YY：Minor
		* ZZ：Hotfix
* 最低5ヶ月経過
