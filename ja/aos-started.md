## Game > Gamebase > Android SDK ご利用ガイド > はじめる

## Environments

AndroidでGamebaseを利用するためのシステム環境は、次の通りです。

> [最小仕様]
>
> * 使用者実行環境：Android API22 (Lollipop MR1, OS 5.1)以上
> * ビルド環境：Android Gradle Plugin 7.4.2以上
> * 開発環境：Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | 用途 | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk | nhncloud-core-1.9.5<br>nhncloud-common<br>nhncloud-crash-reporter-ndk<br>nhncloud-logger<br>gson-2.8.9<br>okhttp-3.12.13<br>kotlin-stdlib-1.8.0<br>kotlin-stdlib-jdk8<br>kotlinx-coroutines-core-1.6.4<br>kotlinx-coroutines-android | Gamebaseのインターフェイス及び核心ロジックを含む | API 21(Lollipop, OS 5.0) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | Sign In With Appleログインをサポート | - |
|  | gamebase-adapter-auth-facebook | facebook-login-16.1.2 | Facebookログインをサポート | - |
|  | gamebase-adapter-auth-google | credentials-play-services-auth-1.3.0<br>play-services-auth-20.3.0 | Googleログインをサポート | - |
|  | gamebase-adapter-auth-gpgs-v2 | play-services-games-v2-20.1.2 | GPGS(Google Play Games Services) V2ログインをサポート<br>Player IDベース | - |
|  | gamebase-adapter-auth-gpgs-autologin | play-services-games-v2-20.1.2 | GPGS(Google Play Games Services)自動ログインをサポート | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.17.2 | Hangameログインをサポート | - |
|  | gamebase-adapter-auth-line | linesdk-5.8.1 | Lineログインをサポート | - |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-5.7.0 | NAVERログインをサポート | - |
|  | gamebase-adapter-auth-payco | payco-login-1.5.15 | Paycoログインをサポート | - |
|  | gamebase-adapter-auth-twitter | - | Twitterログインをサポート | - |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-13.5.0 | Weiboログインをサポート | - |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.19.3<br>kakaogame.gamesdk-3.19.3<br>kakaogame.common-3.19.3<br>kakao.sdk.v2-auth-2.17.0<br>kakao.sdk.v2-partner-auth-2.17.0<br>kakao.sdk.v2-common-2.17.0<br>play-services-ads-identifier-17.0.0 | Kakaoログインをサポート | API 23(Marshmallow, OS 6.0) |
|  | gamebase-adapter-auth-steam | - | Steamログインをサポート | API 25(Nougat, OS 7.1.1) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | toast-gamebase-iap-0.21.0<br>nhncloud-iap-core | ゲーム内決済をサポート | - |
|  | gamebase-adapter-purchase-amazon | nhncloud-iap-amazon | Amazon Appstoreをサポート | - |
|  | gamebase-adapter-purchase-galaxy | nhncloud-iap-galaxy | Galaxy Storeをサポート | - |
|  | gamebase-adapter-purchase-google | billing-7.1.1<br>nhncloud-iap-google | Google Playをサポート | API 24(Nougat, OS 7.0)<br>API 23以下のバージョンをサポートするには、[desugaring宣言](https://developer.android.com/studio/write/java8-support#library-desugaring)が必要 |
|  | gamebase-adapter-purchase-huawei | nhncloud-iap-huawei | Huawei App Galleryをサポート | - |
|  | gamebase-adapter-purchase-onestore | nhncloud-iap-onestore | ONE store v17をサポート | - |
|  | gamebase-adapter-purchase-onestore-v19 | nhncloud-iap-onestore-v19 | ONE store v19をサポート | - |
|  | gamebase-adapter-purchase-onestore-v21 | nhncloud-iap-onestore-v21 | ONE store v21をサポート | API 23(Marshmallow, OS 6.0) |
|  | gamebase-adapter-purchase-onestore-external | nhncloud-iap-onestore-external | ONE store外部決済機能をサポート | - |
|  | gamebase-adapter-purchase-mycard | nhncloud-iap-mycard | MyCard決済機能をサポート | - |
| Gamebase Push Adapters | gamebase-adapter-toastpush | nhncloud-push-analytics<br>nhncloud-push-core<br>nhncloud-push-notification | Pushをサポート | - |
|  | gamebase-adapter-push-adm | nhncloud-push-adm | Amazon Device Messagingをサポート | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>nhncloud-push-fcm | Firebase Notificationをサポート | - |

## Setting

### Console

> <font color="red">[注意]</font><br/>
>
> * NHN Cloud Consoleで新しいプロジェクトを作成してGamebaseサービスが有効になっていることを必ず確認してください。
> * 各IdPのコンソールでClient IDを発行してGamebaseコンソールに入力していることを必ず確認してください。

* Gamebase Android SDKを使用する前に、NHN Cloud ConsoleからアプリIDを発行する必要があります。アプリIDを発行するためには、NHN Cloud Consoleから**(+)サービス選択**をクリックした後、**Game > Gamebase**をクリックしてサービスを有効にします。
* 認証するためにIdPコンソールでclient idを発行し、Gamebaseコンソールに入力します。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Authentication Information](./oper-app/#authentication-information)
* アイテムを購入するためにStoreコンソールでアプリ情報を登録してGamebase > 購入(IAP)コンソールに入力します。
    * [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド](./console-google-guide)
    * [Game > Gamebase > ストアコンソールガイド > ONE Storeコンソールガイド](./console-onestore-guide)
    * [Game > Gamebase > ストアコンソールガイド > GALAXYコンソールガイド](./console-galaxy-guide)
    * [Game > Gamebase > ストアコンソールガイド > Amazon Appstoreコンソールガイド](./console-amazon-guide)
    * [Game > Gamebase > ストアコンソールガイド > Huawei App Galleryコンソールガイド](./console-huawei-guide)
    * [Game > Gamebase > ストアコンソールガイド > MyCardコンソールガイド](./console-mycard-guide)
    * 以下のガイドを参考にしてアイテムを登録します。
        * [Game > Gamebase > コンソール使用ガイド > 決済 > Register](./oper-purchase/#register_1)
* プッシュ通知を行うためにプッシュ通知サービス証明書をGamebase > Push > 証明書コンソールに入力します。
    * [Game > Gamebase > Console ご利用ガイド > Push > Authentication > Authentication register](./oper-push/#authentication-register)
* 新しいGamebaseプロジェクトが作成されたのでAppVersionとStoreCodeを登録する必要があります。
    * 次のガイドに従って新しいクライアントバージョンを登録してください。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Client > Client List](./oper-app/#client-list)

### Register as Tester

#### Gamebase Test Device

* メンテナンス中も正常にゲームにアクセスしたい場合は、Gamebaseコンソールにテスト端末を登録します。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > Test Device](./oper-app/#test-device)

#### Store's Tester

* 決済テストを行うために、ストアごとに次のようにテスターとして登録します。(Gamebase Tester登録ではなく、ストアのテスト決済を行うための設定です。)
    * Google Play Store
        * [Android > テスト購入設定](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > SUPPORT > 開発ツール > (OLD Version)アプリ内決済ガイド > ONEstoreアプリ内決済API V5 (SDK V17)案内およびダウンロード > アプリ内決済テストおよびセキュリティ > テストID登録/管理](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v17/undefined-5#id-id)
        * [ONE store > SUPPORT > 開発ツール > (OLD Version)アプリ内決済ガイド > ONEstoreアプリ内決済API V6 (SDK V19)案内およびダウンロード > アプリ内決済テストおよびセキュリティ > テストID登録/管理](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v19/undefined-4#id-id)
        * [ONE store > SUPPORT > 開発ツール > ONEstoreアプリ内決済API V7 (SDK V21)案内およびダウンロード > 03. 決済テストおよびセキュリティ > テストID登録/管理](https://onestore-dev.gitbook.io/dev/tools/tools/v21/03.#id-03.-id)
    * GALAXY Store
        * [Samsung Developers > Samsung IAP > Technical Documents > Test Guide > 3. IAP Testing > 3.2 Test Type > (3) Production Closed Beta Test](https://developer.samsung.com/iap/iap-test-guide.html)
        * [GALAXY store > アプリ > 登録したアプリ > バイナリ > Beta Test > Tester設定](https://seller.samsungapps.com/application)
        * サムソン端末でのみ決済テストが可能です。
    * Amazon Appstore
        * [Amazon Appstore > In-App Purchasing > Test IAP Apps > IAP Testing Overview](https://developer.amazon.com/docs/in-app-purchasing/iap-testing-overview.html)
    * Huawei App Gallery
        * [Huawei Developers > HMS Core > App Services > In-App Purchases > Guides > Sandbox Testing](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/sandbox-testing-0000001050035039)

### Gradle

#### Root level build.gradle

* Huawei IAPを使用するにはプロジェクトレベル(root level)のsettings.gradleに次の宣言を追加してください。

        buildscript {
            repositories {
                ...
                // [Huawei App Gallery] Maven repository address for the HMS Core SDK.
                maven { url 'https://developer.huawei.com/repo/' }
            }
            
            dependencies {
                ...
                // [Huawei App Gallery] AppGallery Connect plugin configuration. please use the latest plugin version.
                classpath 'com.huawei.agconnect:agcp:1.6.0.300'
            }
        }            

#### Define Adapters

* 使用するGamebaseバージョン、使用する認証、決済、プッシュモジュールをbuild.gradleファイルで宣言してください。
    * Gamebaseの最新バージョンは[Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/)で確認できます。
    * `mavenCentral()`保存場所を追加してください。

```groovy
// >>> [Huawei App Gallery] agconnect plugin for huawei - when Native Android build
apply plugin: 'com.huawei.agconnect'

repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...

    // >>> [Huawei App Gallery]
    maven { url 'https://developer.huawei.com/repo/' }
}

    // >>> [ONE store v21]
    maven { url 'https://repo.onestore.co.kr/repository/onestore-sdk-public' }

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'

    // >>> Gamebase - Add Auth Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-gpgs-v2:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-appleid:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-steam:$GAMEBASE_SDK_VERSION"

    // >>> [Purchase Support under Android 7.0(API Level 24)]
    // desugar_jdk_libs 2.+ needs AGP 7.4+
    coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.5")
    
    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v21:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-mycard:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"

    // >>> 次のモジュールの使用方法はサポートへお問い合わせください。
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejp:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejpemail:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v16]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v16:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v17]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v19]
    // https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs
    implementation files('libs/iap_sdk-v19.01.00.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
    // >>> [GPGS Auto Login]
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-gpgs-autologin:$GAMEBASE_SDK_VERSION"
    // >>> [Push Custom Receiver]
    implementation "com.toast.android.gamebase:gamebase-adapter-push-notification:$GAMEBASE_SDK_VERSION"
}

android {
    compileOptions {
        // >>> [Purchase Support under Android 7.0(API Level 24)]
        coreLibraryDesugaringEnabled true
    }
}
```

### Resources

#### Huawei Store

* AppGallery Connection構成ファイル(agconnect-services.json)をassetsフォルダに追加する必要があります。
    * [AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)にログインした後、**マイプロジェクト**をクリックします。
    * プロジェクトでアプリを選択します。
    * **Project settings** > **General information**に移動します。
    * **App information**から**agconnect-services.json**ファイルをダウンロードします。
    * Android Studioビルドの場合
        * **agconnect-services.json** ファイルをプロジェクトの**assets**フォルダにコピーします。
    * Unityビルドの場合
        * **agconnect-services.json** ファイルをプロジェクトの **Assets/StreamingAssets**フォルダにコピーします。

#### Firebase Notification

* Android Studioビルドの場合
    * Firebaseプッシュを使用するには、以下のガイドに従ってFirebaseの設定を完了した後、google-services.jsonファイルをプロジェクトに含める必要があります。
        * [NHN Cloud > SDK使用ガイド > Push > Android > Firebase Cloud Messaging設定](https://docs.toast.com/ja/TOAST/ja/toast-sdk/push-android/#firebase-cloud-messaging)
* Unityビルドの場合
    * 'Firebase Console > プロジェクト設定' からgoogle-services.jsonファイルをダウンロードし、xml変換のための**[generate_xml_from_google_services_json.exe](https://github.com/firebase/firebase-cpp-sdk/blob/main/generate_xml_from_google_services_json.exe)**ファイルもダウンロードした後、下記のコマンドを実行してjsonファイルを xmlファイルに変換できます。
            
            "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
            
    * 変換したxmlファイルは'Androidライブラリプロジェクト'にリソースとして追加する必要があります。
        * 'Androidライブラリプロジェクト'はフォルダ名に'.androidlib'を含める必要があり、AndroidManifest.xmlファイルを持っている必要があります。
            * [https://docs.unity3d.com/kr/2023.2/Manual/android-library-project-import.html](https://docs.unity3d.com/kr/2023.2/Manual/android-library-project-import.html)
        * 次の例示パスは'Androidライブラリプロジェクト'に追加したStringリソースのパスを示しています。
            * 'Assets/Plugins/Android/MyAndroidProject.androidlib/res/values/google-services.xml'

* Unrealビルドの場合
    * Unrealのプロジェクトの[Gamebase Android設定](./unreal-started/#android-settings)で`GoogleServicesFilePath`の値をFirebaseコンソールからダウンロードした`google-services.json`のパスに指定します。
    * Firebase関連で他のプラグインを使う時、google-services.jsonのデータをAndroidリソースにする過程がある場合、Gamebaseが処理するリソース処理と重複してビルド中にエラーが発生する場合があります。この場合、Gamebase Androidの設定で`GoogleServicesFilePath`の値を空白にしておけば、GamebaseはそのjsonをAndroidリソースに変換する作業を行いません。

### AndroidManifest.xml

#### Contact

* サポートページ([Game > Gamebase > Android SDK使用ガイド > ETC > Additional Features > Contact](./aos-etc/#contact))でお問い合わせの際、写真やメディアを添付するために~API 21でストレージの読み取り権限宣言が必要です。

        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="21"/>

* Android API Level 22以上の端末でもREAD_EXTERNAL_STORAGE権限が必要な場合は、**android:maxSdkVersion="21"**構文を削除し、ランタイム権限リクエストを実装する必要があります。

#### Facebook IdP

* Facebook SDKを初期化するためにApp IDとClient Tokenを宣言します。
    * 値を直接宣言せず、以下の例のようにresourcesを参照するように設定してください。
    * App IDは必須値ではありませんが、Client TokenはFacebook SDK v13.0から必ず入力する必要があります。
        * Client TokenはFacebook開発者サイト > 設定 > 高度な設定 > セキュリティ項目にあります。

**AndroidManifest.xml**

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [Facebook] Configurations begin -->
        <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id" />
        <meta-data android:name="com.facebook.sdk.ClientToken" android:value="@string/facebook_client_token"/>
        <!-- [Facebook] Configurations end -->
        ...
    </application>
</manifest>
```

**res/values/strings.xml**

```xml
<resources>
    <!-- [Facebook] Facebook APP ID & Client Token -->
    <string name="facebook_app_id">123456789012345</string>
    <string name="facebook_client_token">a01234bc56de7fg89012hi3j45k67890</string>
</resources>
```

#### GPGS IdP

* GPGS v2認証やGPGS自動ログインに必要なライブラリを初期化するためにApp IDを宣言します。
    * この値を直接宣言せず、下記の例のようにresourcesを参照するように設定してください。

**AndroidManifest.xml**

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [GPGS] Configurations begin -->
        <meta-data android:name="com.google.android.gms.games.APP_ID" android:value="@string/google_play_game_services_project_id" />
        <!-- [GPGS] Configurations end -->
        ...
    </application>
</manifest>
```

**res/values/strings.xml**

```xml
<resources>
    <!-- [GPGS] Google Play Games Application ID -->
    <string name="google_play_game_services_project_id">1234567890</string>
</resources>
```

* GPGS自動ログイン機能を使用するためには、コンソールにGoogleサービスアカウントの設定も必要です。
    * [Game > Gamebase > コンソール使用ガイド > アプリ > GPGS Automatic Login Settings](./oper-app/#gpgs-automatic-login-settings)

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



* Unityなどのmulti platformをビルドする時、apply pluginの代わりに下記の内容を追加すると、通常の決済が可能です。
* agconnect-services.jsonのcp_id, app_idフィールドの値をAndroidManifest.xmlのmeta-dataに入力してください。

```xml
<meta-data  
    android:name="com.huawei.hms.client.appid"  
    android:value="appid=123456789">  
</meta-data>
<meta-data
    android:name="com.huawei.hms.client.cpid"
    android:value="cpid=1234567891234">
</meta-data>
```
注意：ユーザー端末にHuawei App Galleryがインストールされている時のみ正常に決済が可能です。

#### MyCard

* MyCard決済連動のためにはGamebaseMyCardApplicationを使用する必要があります。 AndroidManifest.xmlに次の内容を追加してください。

```xml
<application
    android:name="com.toast.android.gamebase.purchase.mycard.GamebaseMyCardApplication"
  ...>
  ...
</application>

```

* 直接Applicationを定義して使用している場合は、GamebaseMyCardApplicationを継承して使用してください。

```kotlin
class MyApplication: GamebaseMyCardApplication() {
    ...
}
```

* 決済テストを行うにはAndroidManifest.xmlに'test_mode'を追加してください。 'test_mode'を設定していない場合のデフォルト値はfalseです。
```xml
<application>
  <meta-data android:name="iap:test_mode" android:value="true | false"/>
</application>
```

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
<!-- Vibration setup -->
<meta-data android:name="com.toast.sdk.push.notification.vibration_enabled"
           android:resource="true"/>           
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
| com.toast.sdk.push.notification.vibration_enabled | boolean | バイブレーションの使用有無 |
| com.toast.sdk.push.notification.badge_enabled | boolean | バッジアイコンの使用有無。 |
| com.toast.sdk.push.notification.foreground_enabled | boolean | フォアグラウンド通知の使用有無。 |

### Android 11

* Android 11は、ビルド時にあらかじめ宣言されたアプリケーションでなければ、他のアプリケーションが実行されません。
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* そのため、targetSdkVersionを30以上に設定する場合には必ずAndroidManifest.xmlに**queries**タグを利用して許可するアプリケーションをあらかじめ宣言しておく必要があります。

> <font color="red">[注意]</font><br/>
>
> * 'queries'タグは既存Android Gradle Plugin(AGP)では認識できず、ビルドが失敗します。
> * 以下のガイドおよび表を参考にして'queries'タグビルドが可能なAGPバージョンにアップグレードしてください。
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * AGP 4.1.0以上のバージョンを使用する場合は、AGPのアップグレードは行わなくても構いません。

| If you are using<br>the Android Gradle<br>plugin version... | ...upgrade to: | Unity Editor |
| --- | --- | --- |
| 4.1.* | N/A (no upgrade needed)| \- |
| 4.0.* | 4.0.1 | \- |
| 3.6.* | 3.6.4 | 2020.1 ~ |

```xml
<manifest>
    <!-- [Android11] settings start -->
    <queries>
        <!-- [All SDK] AppToWeb Authenthcation support start -->
        <package android:name="com.android.chrome" />
        <package android:name="com.chrome.beta" />
        <package android:name="com.chrome.dev" />
        <package android:name="com.sec.android.app.sbrowser" />
        <!-- [All SDK] AppToWeb Authenthcation support end -->

        <!-- [Facebook] Configurations begin -->
        <package android:name="com.facebook.katana" />
        <!-- [Facebook] Configurations end -->

        <!-- [PAYCO/Hangame] Configurations begin -->
        <package android:name="com.nhnent.payapp" />
        <!-- [PAYCO/Hangame] Configurations end -->

        <!-- [Hangame] Configurations begin -->
        <package android:name="com.nhn.hangameotp" />
        <package android:name="com.sci.siren24.ipin" />
        <package android:name="kr.co.samsungcard.mpocket" />
        <package android:name="com.lcacApp" />
        <package android:name="com.shcard.smartpay" />
        <package android:name="com.hyundaicard.appcard" />
        <package android:name="com.kbcard.cxh.appcard" />
        <package android:name="com.hanaskcard.paycla" />
        <package android:name="kvp.jjy.MispAndroid320" />
        <package android:name="nh.smart.nhallonepay" />
        <!-- [Hangame] Configurations end -->

        <!-- [NAVER] Configurations begin -->
        <package android:name="com.nhn.android.search" />
        <!-- [NAVER] Configurations end -->

        <!-- [Weibo] Configurations begin -->
        <package android:name="com.weico.international" />
        <package android:name="com.sina.weibo" />
        <!-- [Weibo] Configurations end -->

        <!-- [ONE store] Configurations begin -->
        <!-- Android 2.60.0以上からはONE store queries宣言が必要ありません。 -->
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
    
    <!-- [Amazon Appstore] Configuration begin -->
    <uses-permission
        android:name="android.permission.QUERY_ALL_PACKAGES"
        tools:ignore="QueryAllPackagesPermission" />
    <!-- [Amazon Appstore] Configuration end -->
    
    <!-- [Android11] settings end -->
</manifest>
```

* Amazon Appstoreでは「queries」要素の代わりに**QUERY_ALL_PACKAGES**権限を追加します。

> <font color="red">[注意]</font><br/>
>
> * **QUERY_ALL_PACKAGES**権限はAmazon Appstore専用宣言のため、Google Playビルド時には適用しないようにご注意ください。

### Proguard

* Amazon Device Messaging
    * Amazon Device Messaging(ADM)でProguardを適用する場合、次のガイドを確認して適用する必要があります。
        * [NHN Cloud > SDK使用ガイド > Push > Android > Amazon Device Messaging設定 > ADM SDKダウンロード](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#download-the-adm-sdk)
        * [NHN Cloud > SDK使用ガイド > Push > Android > Amazon Device Messaging設定 > Proguard設定](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard-settings)

## Recommended Flow

* Gamebaseで推奨するflowは、Sample Projectにも同じように実装されています。
    * Android Sample Project
        * [https://github.com/nhn/toast.gamebase.android.sample](https://github.com/nhn/toast.gamebase.android.sample)
            * app/src/main/java/com/toast/android/gamebase/sample/gamebase_managerフォルダのktファイルを参考にしてください。
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* ゲームの開始時にGamebaseクライアントSDKを初期化し、ログインが成功したら、決済の再処理を始めてプッシュトークンを登録してください。

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.30.1.png)

* 詳細flowは、次のリンクで確認できます。
    * [Game > Gamebase > Android SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler](./aos-etc/#gamebase-event-handler)
    * [Game > Gamebase > Android SDK使用ガイド > 初期化 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK使用ガイド > 認証 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK使用ガイド > 決済 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)
    * [Game > Gamebase > Android SDK使用ガイド > プッシュ > Register Push](./aos-push/#register-push)

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Google Play Games Services](https://developer.android.com/games/pgs/start)
* [NAVER for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [LINE for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [PAYCO Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
* [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/doc)
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

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
