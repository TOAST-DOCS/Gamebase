## Game > Gamebase > Android Developer's Guide > Getting Started

## Environments

To execute Gamebase in Android, the following system environment is required.

> [Minimum Specifications]
>
> * User execution environment: Android API 16 (JellyBean, OS 4.1) or higher
> * Build environment: Android Gradle Plugin 3.2.0 or higher
> * Development environment: Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | Purpose | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk-base<br>gamebase-sdk | toast-core-0.31.0<br>toast-common<br>toast-crash-reporter-ndk<br>toast-logger<br>gson-2.8.7<br>okhttp-3.12.5<br>kotlin-stdlib-1.5.21<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.5.1<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | Include interface and core logic of Gamebase | API 16 (JellyBean, OS 4.1) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | Support Sign In With Apple login | API 19 (Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-facebook | facebook-login-11.1.0 | Support Facebook login | - |
|  | gamebase-adapter-auth-google | play-services-auth-19.0.0 | Support Google login | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.4.5 | Support Hangame login | - |
|  | gamebase-adapter-auth-line | linesdk-5.6.2 | Support LINE login | API 17 (Kitkat, OS 4.2) |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-4.4.1 | Support NAVER login | - |
|  | gamebase-adapter-auth-payco | payco-login-1.5.7 | Support PAYCO login | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | Support Twitter login | API 19 (Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-11.8.1 | Support Weibo login | API 19 (Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.11.5<br>kakaogame.gamesdk<br>kakaogame.common<br>kakao.sdk.v2-auth-2.5.2<br>kakao.sdk.v2-partner-auth<br>kakao.sdk.v2-common<br>play-services-ads-identifier-17.0.0 | Support Kakao login | API 21(Lollipop, OS 5.0) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | toast-gamebase-iap-0.18.5<br>toast-iap-core | Support in-app purchase | - |
|  | gamebase-adapter-purchase-amazon | toast-iap-amazon | Support Amazon Appstore | - |
|  | gamebase-adapter-purchase-galaxy | toast-iap-galaxy | Support Galaxy Store | API 21 (Lollipop, OS 5.0)<br>Although minSdkVersion of Galaxy IAP SDK is 18, the minSdkVersion of Checkout service app that must be installed for actual purchase is 21. |
|  | gamebase-adapter-purchase-google | billingclient.billing-3.0.3<br>toast-iap-google | Support Google Play Store | - |
|  | gamebase-adapter-purchase-huawei | toast-iap-huawei | Support Huawei App Gallery | API 19 (Kitkat, OS 4.4) |
|  | gamebase-adapter-purchase-onestore | toast-iap-onestore | Support ONE store v17<br>Currently v19 is not supported | - |
|  | gamebase-adapter-purchase-onestore-external | toast-iap-onestore-external | Support ONE store external payment function | - |
| Gamebase Push Adapters | gamebase-adapter-toastpush | toast-push-analytics<br>toast-push-core<br>toast-push-notification | Support push notifications | - |
|  | gamebase-adapter-push-adm | toast-push-adm | Support Amazon Device Messaging | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>toast-push-fcm | Support Firebase Notification | - |


## Setting

### Console

> <font color="red">[Caution]</font><br/>
>
> * Make sure that Gamebase service is enabled by creating a new project from NHN Cloud Console.
> * Make sure that Client ID is issued by each IdP console and the IDs are entered in the Gamebase console.

* Before using the Gamebase Android SDK, you need an App ID issued at the NHN Cloud Console. To get an App ID, click **(+)Service** on the NHN Cloud Console and then click **Game > Gamebase** to enable the service.
* For authentication, get the Client ID from the IdP and enter it in the Gamebase console.
    * [Game > Gamebase > Console User Guide > App > Authentication Information](./oper-app/#authentication-information)
* To enable item purchase, register the app info in the Store console and enter it in Gamebase > Purchase(IAP) console.
	* [Game > Gamebase > Store Console Guide > Google Console Guide](./console-google-guide)
	* [Game > Gamebase > Store Console Guide > ONE store Console Guide](./console-onestore-guide)
        * For ONE Store, currently only v17 is supported.
        * When creating an app in the ONE Store, please be careful not to create it in v19.
        * ONE Store v19 support is under consideration.
	* [Game > Gamebase > Store Console Guide > GALAXY Store Console Guide](./console-galaxy-guide)
	* [Game > Gamebase > Store Console Guide > Amazon Appstore Console Guide](./console-amazon-guide)
	* [Game > Gamebase > Store Console Guide > Huawei App Gallery Console Guide](./console-huawei-guide)
    * See the following guide to register items.
        * [Game > Gamebase > Console User Guide > Payment > Register](./oper-purchase/#register_1)
* For push notifications, go to Gamebase > Push > Certificate Console and enter the push notification service certificate.
    * [Game > Gamebase > Console User Guide > Push > Authentication > Authentication register](./oper-push/#authentication-register)
* Now that a new Gamebase project has been created, you need to register AppVersion and StoreCode.
    * Use the following guide to register a new client version.
    * [Game > Gamebase > Console User Guide > App > Client > Client List](./oper-app/#client-list)

### Register as Tester

#### Gamebase Test Device

* To be able to access games even during maintenance, register the test device in the Gamebase console.
    * [Game > Gamebase > Console User Guide > App > Test Device](./oper-app/#test-device)

#### Store's Tester

* For payment test, register as a tester as follows for each store: (This is not settings for Gamebase Tester registration but for test payment of the store.)
    * Google Play Store
        * [Android > Test Purchase Settings](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE Store > APPS > Article Status > In-App Information > Payment Test > Register/Manage Test ID](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY Store
        * [Samsung Developers > Samsung IAP > Technical Documents > Test Guide > 3. IAP Testing > 3.2 Test Type > (3) Production Closed Beta Test](https://developer.samsung.com/iap/iap-test-guide.html)
        * [GALAXY Store > App > Registered Apps > Binary > Beta Test > Tester Settings](https://seller.samsungapps.com/application)
        * Payment test is available only on Samsung devices.
    * Amazon Appstore
        * [Amazon Appstore > In-App Purchasing > Test IAP Apps > IAP Testing Overview](https://developer.amazon.com/docs/in-app-purchasing/iap-testing-overview.html)
    * Huawei App Gallery
        * [Huawei Developers > HMS Core > App Services > In-App Purchases > Guides > Sandbox Testing](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/sandbox-testing-0000001050035039)

### Gradle

#### Using AndroidX

* Add the terms of use for AndroidX to build settings.
    * Android Studio
        
            # gradle.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
        
    * Unity 2019.2 or earlier
            
            // mainTemplate.gradle
            ([rootProject] + (rootProject.subprojects as List)).each {
                ext {
                    // >>> [AndroidX]
                    it.setProperty("android.useAndroidX", true)
                    it.setProperty("android.enableJetifier", true)
                }
            }
            
    * Unity 2019.3 or later
            
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

* If the Android Gradle Plugin version is lower than 3.4.0, the build will fail, so the following declaration is required:
    
        # gradle.properties
        # >>> Fix for AGP under 3.4.0
        android.enableD8.desugaring=true
        android.enableIncrementalDesugaring=false
    
* For Unity, the following declaration is required if the Editor version is 2018.4.3 or lower or 2019.1.6 or lower. (AGP version is 3.2.0)
        
        // mainTemplate.gradle
        ([rootProject] + (rootProject.subprojects as List)).each {
            ext {
                // >>> Fix for AGP under 3.4.0
                it.setProperty("android.enableD8.desugaring", true)
                it.setProperty("android.enableIncrementalDesugaring", false)
            }
        }
        

#### Define Adapters

* Declare Gamebase version and authentication to use, and the payment and the push modules in the build.gradle file.
	* Find the latest Gamebase version at [Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/).
	* Add the  `mavenCentral()`  storage. 

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
    // >>> For ONE Store, only v17 can be used and v19 is currently not supported.
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"
    
    // >>> Regarding how to use the following modules, please contact the Customer Center.
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
```

### Resources

#### Huawei Store

* You must add the AppGallery Connection configuration file (agconnect-service.json) to the assets folder.
    * Log in to [AppGallery Connect](https://developer.huawei.com/consumer/en/ervice/josp/agc/index.html) and click on **My Projects**.
    * Select an app from your project.
    * Go to **Project settings** > **General information**.
    * Download the **agconnect-service.json** file from **App information**.
    * For Android Studio builds
        * Copy the **agconnect-service.json** file to the **assets** folder in your project.
    * For Unity builds
        * Copy the **agconnect-service.json** file to the **Assets/StreamingAssets** folder in your project.

#### Firebase Notification

* For Android Studio build
    * To use the Firebase push, you need to follow the guide below to complete the Firebase settings, and then include the google-services.json file in the project.
		* [NHN Cloud > NHN Cloud SDK User Guide > NHN Cloud Push > Android > Firebase Cloud Messaging Settings](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#firebase-cloud-messaging)
* For a Unity build
    * If the Firebase Unity SDK Package has been installed, you can use the following command to execute **generate_xml_from_google_services_json.exe** file to convert json files into xml files.
            
            "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
            
    * If the Firebase Unity SDK Package is not installed, go to 'Firebase Console > Project Settings' to download google-services.json file, and then follow the guide below to directly create a string resource (xml) file. The created file must be included in 'Assets/Plugins/Android/res/values/' folder.
        Depending on the Firebase service link, the contents of the google-services.json file may vary.
        ![Download google-services.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * The following is the example of the string resource(xml) file directly created.

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

* In the case of Unreal build, it is distributed by Gamebase Unreal SDK which included empty google-service-json.xml files, so please change it to the appropriate value for the game information.
    * If there is Content automatically creating xml in a similar form like EasyFirebase, there may be a build error due to resource duplication. At this time, remove the google-service-json.xml file.

### AndroidManifest.xml

#### Facebook IdP

* Declares App ID to initialize Facebook SDK.
    * It is better to configure so that the value refers to resources rather than directly declaring it.
    * This setting is not required at the moment as Gamebase SDK internally calls a function to initialize Facebook SDK.

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

* As **android:allowBackup="false"** is declared in LINE SDK, Manifest merger might fail while building the application. If a build fails in this way, add **tools:replace="android:allowBackup"** declaration to the application tag.

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

#### Weibo IdP

* In order for Weibo IdP to work properly, **android:networkSecurityConfig** attribute must be added to **application** tag, and the name of the xml file which has declared the weibo/sina-related URL must be set.

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

* ONE store supports both full payment screen and payment popup window.
    * You can add AndroidManifest.xml to meta-data to select the full payment screen ("full") or payment popup window ("popup") screen.
    * If meta-data is not set, the default ("full") is applied.

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup:payment popup window / full:full payment screen -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| Payment screen | Set value |
| --- | --- |
| Full payment screen | "full" |
| Payment popup window | "popup" |

#### Notification Options

* You can use the following method to set the notification option:

**Example**

```xml
<!-- When you have multiple applications sharing an Gamebase project, use this field to identify each application. -->
<meta-data android:name="com.nhncloud.sdk.push.deviceId.salt"
           android:value="ApplicationForGoogleStore" />

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
| com.nhncloud.sdk.push.deviceId.salt | String | If different applications share a single Gamebase project, push does not work properly.<br/>You must specify a different random 'salt' value for each app. |
| com.toast.sdk.push.notification.default_priority | int | Priorities.<br/>You can set the following five values:<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | Background color. |
| com.toast.sdk.push.notification.default_light_color | int | LED color. |
| com.toast.sdk.push.notification.default_light_on_ms | int | Time when the LED light turns on. |
| com.toast.sdk.push.notification.default_light_off_ms | int | Time when the LED light turns off. |
| com.toast.sdk.push.notification.default_small_icon | resource id | Resource identifier in small icon. |
| com.toast.sdk.push.notification.default_sound | String | Notification sound file name.<br/>Works only in AOS 8.0 or earlier.<br/>The notification sound will change as you specify the .mp3 or .wav file name in the 'res/raw' folder. |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | Vibration pattern. |
| com.toast.sdk.push.notification.badge_enabled | boolean | Whether to use a badge icon or not. |
| com.toast.sdk.push.notification.foreground_enabled | boolean | Whether to use the foreground notification or not. |

### Android 11

* When building on Android 11, no other applications will run unless pre-declared.
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* When configuring targetSdkVersion to a number equal to or greater than 30 to circumvent this problem, the application to approved must be declared in AndroidManifest.xml using the **queries** tag.

> <font color="red">[Caution]</font><br/>
>
> * “queries” tag cannot be recognized by the existing Android Gradle Plugin(AGP), so the build fails.
> * Refer to the guide and table below and upgrade to the AGP version, which allows “queries” tag build.
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * If using a version lower than AGP 3.2.*, it needs to be upgraded to 3.3.3 or higher.
>     * If using a version higher than AGP 4.1.0, the AGP does not need to be upgraded. 

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

* For Amazon Appstore, add the **QUERY_ALL_PACKAGES** permission instead of the 'queries' element.

> <font color="red">[Caution]</font><br/>
>
> * The **QUERY_ALL_PACKAGES** permission is declared exclusively for Amazon Appstore, so be careful not to apply it when running Google Play builds.

### Proguard

* Amazon Device Messaging
    * To use Proguard with Amazon Device Messaging (ADM), you must apply Proguard by referring to the following guide.
        * [NHN Cloud > SDK User Guide > TOAST Push > Android > Amazon Device Messaging Settings > Download the ADM SDK](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#download-the-adm-sdk)
        * [NHN Cloud > SDK User Guide > TOAST Push > Android > Amazon Device Messaging Settings > Proguard settings](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard-settings)
* For Gamebase versions earlier than 2.21.0, calling Payment API without adding a declaration at the end of Proguard Rule when applying Proguard would result in a crash.
    * This issue has been fixed in Gamebase 2.21.0 version.

            # ---------------------- [Gamebase TOAST IAP] defines start ----------------------
            # For using reflection
            -keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
            # ---------------------- [Gamebase TOAST IAP] defines end ----------------------


## Recommended Flow

* The flow recommended by Gamebase is identically implemented in the Sample Project.
    * Android Sample Project
        * Please see GamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/en/Download/#game-gamebase)
            * GamebaseManager.java file in the following link.
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* Initialize the Gamebase client SDK when the game starts, and if the login is successful, start reprocessing the payment and register the push token.

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.30.1.png)

* You can see the detailed flow in the following link:
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler](./aos-etc/#gamebase-event-handler)
    * [Game > Gamebase > Android SDK User Guide > Initialization > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK User Guide > Authentication > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK User Guide > Payment > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)
    * [Game > Gamebase > Android SDK User Guide > Push > Register Push](./aos-push/#register-push)

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
* [Kakaogame SDK Guide for Channeling](https://tech-wiki.kakaogames.com/display/SDK/Kakaogame+SDK+Guide+for+Channeling)

## API Reference

* API Reference is included in SDK.

## API Deprecate Governance

The API which is not supported by Gamebase anymore is processed as deprecated (deprecate).
A deprecated API can be deleted without any prior notice when the following conditions are met:

* Minor version updates of five or more times.
	* Gamebase version format - XX.YY.ZZ
		* XX: Major
		* YY: Minor
		* ZZ: Hotfix
* Time elapse of at least five months

