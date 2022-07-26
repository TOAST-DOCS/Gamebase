## Game > Gamebase > Android SDK ご利用ガイド > はじめる

## Environments

AndroidでGamebaseを利用するためのシステム環境は、次の通りです。

> [最小仕様]
>
> * 使用者実行環境：Android API 16 (JellyBean, OS 4.1)以上
> * ビルド環境：Android Gradle Plugin 3.2.0以上
> * 開発環境：Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | 用途 | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk-base<br>gamebase-sdk | toast-core-0.31.1<br>toast-common<br>toast-crash-reporter-ndk<br>toast-logger<br>gson-2.8.7<br>okhttp-3.12.5<br>kotlin-stdlib-1.5.21<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.5.1<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | Gamebaseのインタフェースおよびコアロジックを含む | API 16 (JellyBean, OS 4.1) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | Sign In With Appleログインをサポート | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-facebook | facebook-login-11.3.0 | Facebookログインをサポート | - |
|  | gamebase-adapter-auth-google | play-services-auth-19.0.0 | Googleログインをサポート | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.5.2 | Hangameログインをサポート | - |
|  | gamebase-adapter-auth-line | linesdk-5.8.0 | Lineログインをサポート | API 17(JellyBean MR1, OS 4.2) |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-4.4.1 | Naverログインをサポート | - |
|  | gamebase-adapter-auth-payco | payco-login-1.5.7 | Paycoログインをサポート | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | Twitterログインをサポート | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-12.5.0 | Weiboログインをサポート | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-weibo-v4 | openDefault-4.4.4 | Weiboログインをサポート | - |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.11.5<br>kakaogame.gamesdk<br>kakaogame.common<br>kakao.sdk.v2-auth-2.5.2<br>kakao.sdk.v2-partner-auth<br>kakao.sdk.v2-common<br>play-services-ads-identifier-17.0.0 | Kakaoログインをサポート | API 21(Lollipop, OS 5.0) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | toast-gamebase-iap-0.18.5<br>toast-iap-core | ゲーム内決済をサポート | - |
|  | gamebase-adapter-purchase-amazon | toast-iap-amazon | Amazon Appstoreをサポート | - |
|  | gamebase-adapter-purchase-galaxy | toast-iap-galaxy | Galaxy Storeをサポート | API 21(Lollipop, OS 5.0)<br>Galaxy IAP SDKのminSdkVersionは18ですが、<br>実際の決済のためにインストールしなければいけないCheckoutサービスアプリの<br>minSdkVersionは21です。 |
|  | gamebase-adapter-purchase-google | billingclient.billing-3.0.3<br>toast-iap-google | Google Play Storeをサポート | - |
|  | gamebase-adapter-purchase-huawei | toast-iap-huawei | Huawei App Galleryをサポート | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-purchase-onestore | toast-iap-onestore | ONE Store v17をサポート<br>現在v19はサポート不可 | - |
| Gamebase Push Adapters | gamebase-adapter-toastpush | toast-push-analytics<br>toast-push-core<br>toast-push-notification | Pushをサポート | - |
|  | gamebase-adapter-push-adm | toast-push-adm | Amazon Device Messagingをサポート | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>toast-push-fcm | Firebase Notificationをサポート | - |


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
        * ONE Storeは現在v17のみサポートします。
        * ONE Storeでアプリを作成する時、v19で作成しないように注意してください。
        * ONE Storev19のサポートは検討中の段階です。
	* [Game > Gamebase > ストアコンソールガイド > GALAXYコンソールガイド](./console-galaxy-guide)
	* [Game > Gamebase > ストアコンソールガイド > Amazon Appstoreコンソールガイド](./console-amazon-guide)
	* [Game > Gamebase > ストアコンソールガイド > Huawei App Galleryコンソールガイド](./console-huawei-guide)
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
        * [ONE store > APPS > 商品状況 > In-App情報 > 決済テスト > テストID登録/管理](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY Store
        * [Samsung Developers > Samsung IAP > Technical Documents > Test Guide > 3. IAP Testing > 3.2 Test Type > (3) Production Closed Beta Test](https://developer.samsung.com/iap/iap-test-guide.html)
        * [GALAXY store > アプリ > 登録したアプリ > バイナリ > Beta Test > Tester設定](https://seller.samsungapps.com/application)
        * サムソン端末でのみ決済テストが可能です。
    * Amazon Appstore
        * [Amazon Appstore > In-App Purchasing > Test IAP Apps > IAP Testing Overview](https://developer.amazon.com/docs/in-app-purchasing/iap-testing-overview.html)
    * Huawei App Gallery
        * [Huawei Developers > HMS Core > App Services > In-App Purchases > Guides > Sandbox Testing](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/sandbox-testing-0000001050035039)

### Gradle

#### Using AndroidX

* AndroidX使用宣言をビルド設定に追加してください。
    * Android Studio
        
            # gradle.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
        
    * Unity 2019.2以下
            
            // mainTemplate.gradle
            ([rootProject] + (rootProject.subprojects as List)).each {
                ext {
                    // >>> [AndroidX]
                    it.setProperty("android.useAndroidX", true)
                    it.setProperty("android.enableJetifier", true)
                }
            }
            
    * Unity 2019.3以上
            
            # gradleTemplate.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
            
    * Unreal
            
            <gradleProperties>
              <insert>
                android.useAndroidX=true
                android.enableJetifier=true
              </insert>
            </gradleProperties>
            
        
#### Under AGP 3.4.0

* Android Gradle Pluginバージョンが3.4.0未満の場合、ビルドが失敗するため、次の宣言が必要です。
    
        # gradle.properties
        # >>> Fix for AGP under 3.4.0
        android.enableD8.desugaring=true
        android.enableIncrementalDesugaring=false
    
* Unityの場合、Editorバージョンが2018.4.3以下または2019.1.6以下の場合、これに該当します。(AGPバージョンが3.2.0)
        
        // mainTemplate.gradle
        ([rootProject] + (rootProject.subprojects as List)).each {
            ext {
                // >>> Fix for AGP under 3.4.0
                it.setProperty("android.enableD8.desugaring", true)
                it.setProperty("android.enableIncrementalDesugaring", false)
            }
        }
        

#### Define Adapters

* 使用するGamebaseバージョン、使用する認証、決済、プッシュモジュールをbuild.gradleファイルで宣言してください。
	* Gamebaseの最新バージョンは[Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/)で確認できます。
	* `mavenCentral()`保存場所を追加してください。

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...

    // >>> [Huawei App Gallery]
    maven { url 'https://developer.huawei.com/repo/' }
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
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    // >>> ONE Storeはv17のみ使用することができ、v19は現在サポートしていません。
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"

    // >>> 次のモジュールの使用方法はサポートへお問い合わせください。
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejp:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejpemail:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo-v4:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v16:$GAMEBASE_SDK_VERSION"
}

android {
    compileOptions {
        // >>> [AndroidX]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        // >>> [Weibo IdP]
        ndk {
            abiFilters 'armeabi' // , 'armeabi-v7a', 'arm64-v8a'
        }
    }
    }
}
```

### Resources

#### Huawei Store

* AppGallery Connection構成ファイル(agconnect-service.json)をassetsフォルダに追加する必要があります。
    * [AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)にログインした後、**マイプロジェクト**をクリックします。
    * プロジェクトでアプリを選択します。
    * **Project settings** > **General information**に移動します。
    * **App information**から**agconnect-service.json**ファイルをダウンロードします。
    * Android Studioビルドの場合
        * **agconnect-service.json** ファイルをプロジェクトの**assets**フォルダにコピーします。
    * Unityビルドの場合
        * **agconnect-service.json** ファイルをプロジェクトの **Assets/StreamingAssets**フォルダにコピーします。

#### Firebase Notification

* Android Studioビルドの場合
    * Firebaseプッシュを使用するには、以下のガイドに従ってFirebaseの設定を完了した後、google-services.jsonファイルをプロジェクトに含める必要があります。
		* [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud Push > Android > Firebase Cloud Messaging設定](https://docs.toast.com/ja/TOAST/ja/toast-sdk/push-android/#firebase-cloud-messaging)
* Unityビルドの場合
    * Firebase Unity SDK Packageをインストールした場合は、以下のコマンドで**generate_xml_from_google_services_json.exe**ファイルを実行してjsonファイルをxmlファイルに変換できます。
            
            "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
            
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

* Unrealビルドの場合、Gamebase Unreal SDKで空のgoogle-service-json.xmlファイルを含めて配布しているため、該当ゲーム情報に合った値に変更してください。
    * もしEasyFirebaseのように、似たような形式のxmlを自動作成するContentがある場合、リソース重複により ビルドエラーが発生する場合があります。この時はgoogle-service-json.xmlファイルを除去してください。

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

#### LINE IdP

* LINE SDKの内部に**android:allowBackup="false"**が宣言されており、アプリケーションビルド時にManifest mergerでfailが発生することがあります。ビルドが失敗した場合は、次のようにapplicationタグに**tools:replace="android:allowBackup"**宣言を追加してください。

```xml
<application
      tools:replace="android:allowBackup"
      ... >
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
> * 'queries'タグは既存Android Gradle Plugin(AGP)では認識できず、ビルドが失敗します。
> * 以下のガイドおよび表を参考にして'queries'タグビルドが可能なAGPバージョンにアップグレードしてください。
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * AGP 3.2.*以下のバージョンを使用する場合、3.3.3以上にアップグレードする必要があります。
>     * AGP 4.1.0以上のバージョンを使用する場合は、AGPのアップグレードは行わなくても構いません。

| If you are using<br>the Android Gradle<br>plugin version... | ...upgrade to: | Unity Editor |
| --- | --- | --- |
| 4.1.* | N/A (no upgrade needed)| \- |
| 4.0.* | 4.0.1 | \- |
| 3.6.* | 3.6.4 | 2020.1 ~ |
| 3.5.* | 3.5.4 | \- |
| 3.4.* | 3.4.3 | 2018.4.4 ~<br>2019.1.7 ~ |
| 3.3.* | 3.3.3 | \- |
| 3.2.* | Not supported | 2017.4.17 ~<br>2018.3 ~ 2018.4.3<br>2019.1.0 ~ 2019.1.6 |
| 3.0.* | Not supported | 2018.2 |
| 2.3.* | Not supported | 2017.3 ~ 2017.4.16<br>2018.1 |
| 2.1.* | Not supported | Unity 5<br>2017.1 ~ 2017.2 |

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

        <!-- [LINE] Configurations begin -->
        <package android:name="jp.naver.line.android" />
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="https" />
        </intent>
        <!-- [LINE] Configurations end -->

        <!-- [NAVER] Configurations begin -->
        <package android:name="com.nhn.android.search" />
        <!-- [NAVER] Configurations end -->

        <!-- [Weibo] Configurations begin -->
        <package android:name="com.weico.international" />
        <package android:name="com.sina.weibo" />
        <!-- [Weibo] Configurations end -->

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
        * [NHN Cloud > SDK使用ガイド > TOAST Push > Android > Amazon Device Messaging設定 > ADM SDKダウンロード](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#download-the-adm-sdk)
        * [NHN Cloud > SDK使用ガイド > TOAST Push > Android > Amazon Device Messaging設定 > Proguard設定](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard-settings)
* Gamebase 2.21.0未満のバージョンはProguard適用時、Proguard Ruleに次の宣言を追加していない場合、決済API呼び出し時にクラッシュが発生します。
    * Gamebase 2.21.0バージョンで修正されました。

            # ---------------------- [Gamebase TOAST IAP] defines start ----------------------
            # For using reflection
            -keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
            # ---------------------- [Gamebase TOAST IAP] defines end ----------------------


## Recommended Flow

* Gamebaseで推奨するflowは、Sample Projectにも同じように実装されています。
    * Android Sample Project
        * 以下のリンクのGamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/ja/Download/#game-gamebase)
            * GamebaseManager.javaファイルを参考にしてください。
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
* [NAVER for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [LINE for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [PAYCO Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
* [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/blob/master/2019SDK/文档)
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
