## Game > Gamebase > Android Developer's Guide > Getting Started

## Environments

To execute Gamebase in Android, following system environment is required.

> [Minimum Specifications]
>
> * Android API 16 (JellyBean, 4.1) or higher
>     * For Twitter Login, 19 (Kitkat, 4.4) or later
>     * For AppleID Login, 19 (Kitkat, 4.4) or later
>     * For Line Login, 17(Kitkat, 4.2) or later
>     * For Weibo Login, 19 (Kitkat, 4.4) or later
>     * For GALAXY Store, 21 (Lollipop, 5.0) or later
>         * Since minSdkVersion of the GALAXY IAP SDK is 18 (OS 4.3), the build will fail when the set value is smaller than this.
>         * Checkout service app must be installed for actual payment, but Checkout service app will not be installed if it is lower than API 21(OS 5.0. Lollipop). In this case, the payment will not be processed.
> * Gradle Android Plugin 2.3.0 or higher
> * Development Environment: Android Studio

## Setting

### Console

> <font color="red">[주의]</font><br/>
>
> * NHN Cloud Console 에서 새 프로젝트를 생성하여 Gamebase 서비스를 활성화 하였는지 꼭 확인하세요.
> * 각 IdP 콘솔에서 Client ID 를 발급받아 Gamebase 콘솔에 입력하였는지 꼭 확인하세요.

* Before applying Gamebase Android SDK, you need an App ID issued at the NHN Cloud Console: select a project created in the NHN Cloud Console and click **(+)Service** **Game > Gamebase**.
* For authentication, get the client id from the IdP and enter it in the Gamebase console.
    * [Game > Gamebase > Console User Guide > App > Authentication Information](./oper-app/#authentication-information)
* To enable item purchase, register the app info in the Store console and enter it in Gamebase > Purchase(IAP) console.
	* [Game > Gamebase > Store Console Guide > Google Console Guide](./console-google-guide)
	* [Game > Gamebase > Store Console Guide > ONEStore Console Guide](./console-onestore-guide)
	* [Game > Gamebase > Store Console Guide > GALAXY Store Console Guide](./console-galaxy-guide)
    * See the following guide to register items.
        * [Game > Gamebase > Console User Guide > Payment > Register](./oper-purchase/#register_1)
* For push notifications, go to Gamebase > Push > Certificate Console and enter the push notification service certificate.
    * [Game > Gamebase > Console User Guide > Push > Authentication > Authentication register](./oper-push/#authentication-register)
* Now that a new Gamebase project has been created, you need to register AppVersion and StoreCode.
    * Use the following guide to register a new client version.
    * [Game > Gamebase > Console User Guide > App > Client > Client List](./oper-app/#client-list)

### Regist as Tester

#### Gamebase Test Device

* To be able to access games even during maintenance, register the test device in the Gamebase console.
    * [Game > Gamebase > Console User Guide > App > Test Device](./oper-app/#test-device)

#### Store's Tester

* For payment test, register as a tester as follows for each store: (This is not settings for Gamebase Tester registration but for test payment of the store.)
    * Google
        * [Android > Test Purchase Settings](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE Store > APPS > Article Status > In-App Information > Payment Test > Register/Manage Test ID](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY store
        * [GALAXY Store > App > Registered Apps > Binary > Beta Test > Tester Settings](https://seller.samsungapps.com/application)
        * Payment test is available only on Samsung devices.

### Gradle

* Declare Gamebase version and authentication to use, and the payment and the push modules in the build.gradle file.
	* Find the latest Gamebase version at [Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/).
	* Add the  `mavenCentral()`  storage. 
    * Android Studio 에서 빌드하는 경우, kotlin-gradle-plugin 을 위해  **'maven { url "https://plugins.gradle.org/m2/" }'** 저장소를 추가할 필요가 있습니다.

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
    maven { url 'To learn how to set the Hangame IdP, please contact our Customer Center.' }
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

* You need to copy Weibo so library from the following path and add it in app/src/main/jniLibs folder.
    * [https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so](https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so)
    * Just use only necessary platforms.

### Resources

#### Firebase Notification

* For Android Studio build
    * To use the Firebase push, you need to follow the guide below to complete the Firebase settings, and then include the google-services.json file in the project.
		* [NHN Cloud > NHN Cloud SDK User Guide > NHN Cloud Push > Android > Firebase Cloud Messaging Settings](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#firebase-cloud-messaging)
* For a Unity build
    * If the Firebase Unity SDK Package has been installed, you can use the following command to execute **generate_xml_from_google_services_json.exe** file to convert json files into xml files.
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
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

### AndroidManifest.xml

#### Facebook IdP

* Facebook SDK 초기화를 위해 App ID 를 선언합니다.
    * 해당 값을 직접 선언하는 것 보다는 아래 예시와 같이 resources 를 참조하도록 설정하는 것이 좋습니다.
    * Gamebase SDK 가 내부적으로 Facebook SDK 초기화 함수를 호출하고 있으므로 현재는 필수 설정은 아닙니다.

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

* Line SDK 내부에 **android:allowBackup="false"** 로 선언되어 있어 어플리케이션 빌드시 Manifest merger 에서 fail 이 발생할 수 있습니다. 이렇게 빌드가 실패한다면 다음과 같이 application 태그에 **tools:replace="android:allowBackup"** 선언을 추가하시기 바랍니다.

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

#### Hangame IdP

* As for AndroidManifest.xml settings for proper operation of Hangame IdP, please contact our Customer Center.

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

* Android 11 은 빌드시 미리 선언된 어플리케이션이 아니면 다른 어플리케이션이 실행되지 않습니다.
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* 이를 위해 targetSdkVersion 을 30 이상으로 설정하는 경우에는 반드시 AndroidManifest.xml 에 **queries** 태그를 통해 허용할 어플리케이션을 미리 선언해두어야 합니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 Gradle 5.6.4 이상 버전에서만 빌드가 가능합니다.
>     * 그러므로 IDE 에서 Gradle 5.6.4 이상이 지원되지 않는 환경에서는 targetSdkVersion 을 29 이하로 설정해야 합니다.
> * Gradle 5.6.4 이상 버전이 적용된 IDE 는 다음과 같습니다.
>     * Android Studio : 3.6.1 이상
>     * Unity : 2020.1 이상
>     * Unreal : 지원 불가

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

* Gamebase 2.21.0 미만 버전은 Proguard 적용시 Proguard Rule 에 다음 선언을 추가하지 않으면 결제 API 호출시 크래쉬가 발생합니다.
    * Gamebase 2.21.0 버전에서 수정되었습니다.

```
# ---------------------- [Gamebase TOAST IAP] defines start ----------------------
# For using reflection
-keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
# ---------------------- [Gamebase TOAST IAP] defines end ----------------------
```

## Recommended Flow

* The flow recommended by Gamebase is identically implemented in the Sample Project.
    * Android Sample Project
        * Please see GamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/en/Download/#game-gamebase)
            * GamebaseManager.java file in the following link.
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* Implement to reset the Gamebase client SDK at the start of the game and start to reprocess the payment after a successful login.

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.19.0.png)

* You can see the detailed flow in the following link:
    * [Game > Gamebase > Android SDK User Guide > Initialization > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK User Guide > Authentication > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK User Guide > Payment > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)

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

