## Game > Gamebase > Android Developer's Guide > Getting Started

## Environments

To execute Gamebase in Android, the following system environment is required.

> [Minimum Specifications]
>
> * User execution environment: Android API 19 (KitKat, OS 4.4) or higher
> * Build environment: Android Gradle Plugin 3.2.0 or higher
> * Development environment: Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | Purpose | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk-base<br>gamebase-sdk | nhncloud-core-1.6.0<br>nhncloud-common<br>nhncloud-crash-reporter-ndk<br>nhncloud-logger<br>gson-2.8.9<br>okhttp-3.12.13<br>kotlin-stdlib-1.8.0<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.6.4<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | Include the interface and core logic of Gamebase | API 19(KitKat, OS 4.4) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | Support Sign In With Apple login | - |
|  | gamebase-adapter-auth-facebook | facebook-login-16.1.2 | Support Facebook login | - |
|  | gamebase-adapter-auth-google | play-services-auth-20.3.0 | Support Google login | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.7.1 | Support Hangame login | - |
|  | gamebase-adapter-auth-line | linesdk-5.8.1 | Support LINE login | - |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-5.8.1 | Support NAVER login | API 21(Lollipop, OS 5.0) |
|  | gamebase-adapter-auth-payco | payco-login-1.5.12 | Support PAYCO login | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | Support Twitter login | - |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-13.5.0 | Support Weibo login | - |
|  | gamebase-adapter-auth-weibo-v4 | openDefault-4.4.4 | Support Weibo login | - |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.14.14<br>kakaogame.gamesdk<br>kakaogame.common<br>kakao.sdk.v2-auth-2.11.1<br>kakao.sdk.v2-partner-auth<br>kakao.sdk.v2-common<br>play-services-ads-identifier-17.0.0 | Support Kakao login | API 21(Lollipop, OS 5.0) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | nhncloud-iap-core | Support in-app purchase | - |
|  | gamebase-adapter-purchase-amazon | nhncloud-iap-amazon | Support Amazon Appstore | - |
|  | gamebase-adapter-purchase-galaxy | nhncloud-iap-galaxy | Support Samsung Galaxy Store | API 21(Lollipop, OS 5.0)<br>Although minSdkVersion of Galaxy IAP SDK is 18, the minSdkVersion of Checkout service app that must be installed for actual purchase is 21. |
|  | gamebase-adapter-purchase-google | billingclient.billing-5.0.0<br>nhncloud-iap-google | Support Google Play | - |
|  | gamebase-adapter-purchase-huawei | nhncloud-iap-huawei | Support Huawei AppGallery | - |
|  | gamebase-adapter-purchase-onestore | nhncloud-iap-onestore | Support ONE store v17 | - |
|  | gamebase-adapter-purchase-onestore-v19 | nhncloud-iap-onestore-v19 | Support ONE store v19| - |
|  | gamebase-adapter-purchase-onestore-v21 | nhncloud-iap-onestore-v21 | Support ONE store v21 | API 23(Marshmallow, OS 6.0) |
|  | gamebase-adapter-purchase-onestore-external | nhncloud-iap-onestore-external | Support ONE store external payment function | - |
|  | gamebase-adapter-purchase-mycard | nhncloud-iap-mycard | Support MyCard payment function | - |
| Gamebase Push Adapters | gamebase-adapter-toastpush | nhncloud-push-analytics<br>nhncloud-push-core<br>nhncloud-push-notification | Support Push | - |
|  | gamebase-adapter-push-adm | nhncloud-push-adm | Support Amazon Device Messaging | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>nhncloud-push-fcm | Support Firebase Cloud Messaging | - |


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
    * [Game > Gamebase > Store Console Guide > GALAXY Store Console Guide](./console-galaxy-guide)
    * [Game > Gamebase > Store Console Guide > Amazon Appstore Console Guide](./console-amazon-guide)
    * [Game > Gamebase > Store Console Guide > Huawei App Gallery Console Guide](./console-huawei-guide)
    * [Game > Gamebase > Store Console Guide > MyCard Console Guide](./console-mycard-guide)
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
        * [ONE store > SUPPORT > Development Tool > (OLD Version) In-App Purcahse Guide > Introduction and Download for ONE store In-App Purchase API V5 (SDK V17) > In-App Purchase Test and Security > Register/Manage Test ID](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v17/undefined-5#id-id)
        * [ONE store > SUPPORT > Development Tool > (OLD Version) In-App Purcahse Guide > Introduction and Download for ONE store In-App Purchase API V6 (SDK V19) > In-App Purchase Test and Security > Register/Manage Test ID](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v19/undefined-4#id-id)
        * [ONE store > SUPPORT > Development Tool > Introduction and Download for ONE store In-App Purchase API V7 (SDK V21) > 03. Purchase Test and Security > Register/Manage Test ID](https://onestore-dev.gitbook.io/dev/tools/tools/v21/03.#id-03.-id)
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
        
#### Root level build.gradle

* To use Huawei IAP, add the following declaration to build.gradle or settings.gradle (AGP 7.1 or later) at the project level (root level).

```groovy
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
```

#### Define Adapters

* Declare Gamebase version and authentication to use, and the payment and the push modules in the build.gradle file.
    * Find the latest Gamebase version at [Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/).
    * Add the  `mavenCentral()`  storage. 

```groovy
// >>> [Huawei App Gallery] agconnect plugin for huawei - when Native Android build
apply plugin: 'com.huawei.agconnect'

repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
    
    // >>> [Huawei App Gallery]
    maven { url 'https://developer.huawei.com/repo/' }

    // >>> [ONE store v21]
    maven { url 'https://repo.onestore.co.kr/repository/onestore-sdk-public' }
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
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v21:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-mycard:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"
    
    // >>> Regarding how to use the following modules, please contact the Customer Center.
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejp:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejpemail:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
    // >>> [Weibo v4]
    // https://github.com/nhn/toast.gamebase.android.sample/tree/main/weibo_sdk
    implementation files('libs/openDefault-4.4.4.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo-v4:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v16]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v16:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v17]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v19]
    // https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs
    implementation files('libs/iap_sdk-v19.00.02.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
    // >>> [Push Custom Receiver]
    implementation "com.toast.android.gamebase:gamebase-adapter-push-notification:$GAMEBASE_SDK_VERSION"
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

#### Weibo IdP

* Depending on your build target, download the so files from the following URLs and copy them to your project.
    * https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so
* In case of Android Studio build
    * Copy under the project's src/main/java/jniLibs folder.
    * ![Add so file to Android Studio project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-android-studio-2.53.0.png)
* In case of Unity build
    * Copy the so and foler under the Assets/Plugins/Android/libs.
    * ![Add so file to Unity project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-unity-2.53.0.png)

#### Huawei Store

* You must add the AppGallery Connection configuration file (agconnect-services.json) to the assets folder.
    * Log in to [AppGallery Connect](https://developer.huawei.com/consumer/en/ervice/josp/agc/index.html) and click on **My Projects**.
    * Select an app from your project.
    * Go to **Project settings** > **General information**.
    * Download the **agconnect-services.json** file from **App information**.
    * For Android Studio builds
        * Copy the **agconnect-services.json** file to the **assets** folder in your project.
    * For Unity builds
        * Copy the **agconnect-services.json** file to the **Assets/StreamingAssets** folder in your project.

#### Firebase Notification

* For Android Studio build
    * To use the Firebase push, you need to follow the guide below to complete the Firebase settings, and then include the google-services.json file in the project.
        * [NHN Cloud > SDK User Guide> Push > Android > Firebase Cloud Messaging Settings](/TOAST/en/toast-sdk/push-android/#firebase-cloud-messaging)
* For a Unity build
    * **Caution**: It is not necessary to install the Firebase Unity SDK Package, and even if you do not, push works properly.
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

#### Contact

* To make an inquiry in [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Contact](./aos-etc/#contact) with photos and media, you need permission to read storage.
        
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        
* If your app is targeting Android 13 (API Level 33) or later, in addition to the storage read permission, you also need to decare the following detailed media permissions.
        
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        
        <uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />
        <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
        
* If permissions are declared, the Gamebase SDK will automatically request runtime permissions at file upload time.

#### Facebook IdP

* Declares the App ID and the client token to initialize Facebook SDK.
    * Instead of declaring the values directly, set them to reference resources, as shown in the example below.
    * The App ID is not required, but the client token is required starting with Facebook SDK v13.0 for successful login.
    * The client token can be found in Facebook for Developers > Settings > Advanced Settings > Security.

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

#### Huawei Store

* When building multi platforms like Unity, add the following instead of the apply plugin to enable normal payment.
* Enter the values of the cp_id and app_id fields in agconnect-services.json into the meta-data of the AndroidManifest.xml.

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
Caution: Huawei App Gallery must be installed on your device to make payment normally.

#### MyCard

* To integrate with MyCard payment, you must use GamebaseMyCardApplication. Add the following to the AndroidManifest.xml.

```xml
<application
    android:name="com.toast.android.gamebase.purchase.mycard.GamebaseMyCardApplication"
  ...>
  ...
</application>

```

* If you define and use your application class by extending Applcation class, inherit from GamebaseMyCardApplication.

```kotlin
class MyApplication: GamebaseMyCardApplication() {
    ...
}
```

* To run the payment test, add 'test_mode' to the AndroidManifest.xml. Otherwise, the default value is false.
```xml
<application>
  <meta-data android:name="iap:test_mode" android:value="true | false"/>
</application>
```

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
        * [NHN Cloud > SDK User Guide > Push > Android > Amazon Device Messaging Settings > Download the ADM SDK](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#adm-sdk)
        * [NHN Cloud > SDK User Guide > Push > Android > Amazon Device Messaging Settings > Proguard Settings](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard)
* For Gamebase versions earlier than 2.21.0, calling Payment API without adding a declaration at the end of Proguard Rule when applying Proguard would result in a crash.
    * This issue has been fixed in Gamebase 2.21.0 version.

            # ---------------------- [Gamebase TOAST IAP] defines start ----------------------
            # For using reflection
            -keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
            # ---------------------- [Gamebase TOAST IAP] defines end ----------------------


## Recommended Flow

* The flow recommended by Gamebase is identically implemented in the Sample Project.
    * Android Sample Project
        * [https://github.com/nhn/toast.gamebase.android.sample](https://github.com/nhn/toast.gamebase.android.sample)
            * Please see kt files from app/src/main/java/com/toast/android/gamebase/sample/gamebase_manager.
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
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

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

