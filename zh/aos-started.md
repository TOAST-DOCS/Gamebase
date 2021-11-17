## Game > Gamebase > Android SDK使用指南 > 开始

## Environments

在Android上应用Gamebase，需要的系统环境如下。

> [最低版本]
>
> * 用户运行环境 : Android API 16 (JellyBean, OS 4.1)以上
> * 打包环境 : Android Gradle Plugin 3.2.0以上
> * 开发环境 : Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | 用途 | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk-base<br>gamebase-sdk | toast-core-0.27.1<br>toast-common<br>toast-crash-reporter-ndk<br>toast-logger<br>gson-2.8.7<br>okhttp-3.12.5<br>kotlin-stdlib-1.5.21<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.5.1<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | 包含Gamebase的Interface和核心逻辑。 | API 16 (JellyBean, OS 4.1) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | 支持Sign In With Apple登录 | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-facebook | facebook-login-11.1.0 | 支持Facebook登录 | - |
|  | gamebase-adapter-auth-google | play-services-auth-19.0.0 | 支持Google登录 | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.4.1 | 支持Hangame登录 | - |
|  | gamebase-adapter-auth-line | linesdk-5.6.2 | 支持Line登录 | API 17(Kitkat, OS 4.2) |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-4.4.1 | 支持Naver登录 | - |
|  | gamebase-adapter-auth-payco | payco-login-1.5.6 | 支持Payco登录 | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | 支持Twitter登录 | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-11.8.1 | 支持Weibo登录 | API 19(Kitkat, OS 4.4) |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.11.5<br>kakaogame.gamesdk<br>kakaogame.common<br>kakao.sdk.v2-auth-2.5.2<br>kakao.sdk.v2-partner-auth<br>kakao.sdk.v2-common<br>play-services-ads-identifier-17.0.0 | 支持Kakao登录 | API 21(Lollipop, OS 5.0) |
| Gamebase IAP | gamebase-adapter-toastiap | toast-gamebase-iap-0.16.0<br>toast-iap-core | 支持游戏内支付 | - |
|  | gamebase-adapter-purchase-galaxy | toast-iap-galaxy | 支持Galaxy Store | API 21(Lollipop, OS 5.0)<br>Galaxy IAP SDK的minSdkVersion是18，为了实际结算，需要设置的Checkout服务应用程序的minSdkVersion是21。 |
|  | gamebase-adapter-purchase-google | billingclient.billing-3.0.3<br>toast-iap-google | 支持Google Store | - |
|  | gamebase-adapter-purchase-onestore | toast-iap-onestore | 支持ONE Store | - |
| Gamebase Push | gamebase-adapter-toastpush | toast-push-analytics<br>toast-push-core<br>toast-push-notification | 支持Push | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>toast-push-fcm | 支持Firebase Notification | - |

## Setting

### Console

> <font color="red">[注意]</font><br/>
>
> * 在NHN Cloud Console中创建新项目后，请确认是否启用了Gamebase服务。
> * 从各IdP控制台中获取Client ID后，请确认是否在Gamebase控制台中输入了ID。 
 
* 在应用Gamebase Android SDK之前，您需要从NHN Cloud Console获取App ID。获取应用App ID，请在NHN Cloud Console上在单击**(+)服务**，然后点击**Game > Gamebase**以启用该服务。
* 进行认证时从IdP控制台获取client id，在Gamebase控制台中输入。 
    * [Game > Gamebase > 控制台使用指南 > App > Authentication Information](./oper-app/#authentication-information)
* 如果想购买道具，在Store控制台中注册App信息，在Gamebase > 购买(IAP)控制台中输入相关信息。
	* [Game > Gamebase > Store控制台指南 > Google控制台指南](./console-google-guide) 
	* [Game > Gamebase > Store控制台指南 > ONEStore控制台指南](./console-onestore-guide)
	* [Game > Gamebase > Store控制台指南 > GALAXY Store控制台指南](./console-galaxy-guide)
    * 注册道具时，请参考以下指南。
        * [Game > Gamebase > 控制台使用指南 > 结算 > Register](./oper-purchase/#register_1)
* 为了推送通知，要将推送通知服务认证书在Gamebase > 推送 > 认证书控制台中输入。
    * [Game > Gamebase > 控制台使用指南 > 推送 > Authentication > Authentication register](./oper-push/#authentication)
* 新Gamebase项目已生成，需要注册AppVersion和StoreCode。
    * 注册新客户端版本时，请参考如下指南。 
    * [Game > Gamebase > 控制台使用指南 > App > Client > Client List](./oper-app/#client-list)

### Register as Tester

#### Gamebase Test Device

* 需要在维护中进入游戏时，在Gamebase控制台中注册测试终端机。
    * [Game > Gamebase > 控制台使用指南 > App > Test Device](./oper-app/#test-device)

#### Store's Tester

* 进行付费测试时，请按以下方式添加每个商店的测试人员。（不应在Gamebase设置Tester，要在Store控制台中设置。)
    * Google
        * [Android > 测试购买设置](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > APPS > 商品现状 > In-App信息 > 付费测试 > 注册/管理测试ID](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY store
        * [GALAXY store > App > 已注册App > Binary > Beta Test > 设置Tester](https://seller.samsungapps.com/application)
        * 只能在三星终端机上进行付费测试。 

### Gradle

#### Using AndroidX

* 请将AndroidX使用声明添加到打包设置中。
    * Android Studio
        ```groovy
        # gradle.properties
        # >>> [AndroidX]
        android.useAndroidX=true
        android.enableJetifier=true
        ```
    * Unity 2019.2以下
        ```groovy
        // mainTemplate.gradle
        ([rootProject] + (rootProject.subprojects as List)).each {
            ext {
                it.setProperty("android.useAndroidX", true)
                it.setProperty("android.enableJetifier", true)
            }
        }
        ```
    * Unity 2019.3以上
        ```groovy
        // gradleTemplate.properties
        android.useAndroidX=true
        android.enableJetifier=true
        ```
    * Unreal
        ```xml
        <gradleProperties>    
          <insert>      
            android.useAndroidX=true      
            android.enableJetifier=true    
          </insert>  
        </gradleProperties>
        ```

#### Define Adapters

* 请在build.gradle文件中声明要使用的Gamebase版本和需要使用的验证、支付及推送模块。
	* Gamebase的最新版本可在[Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/)上确认。
	* 请添加**mavenCentral()**仓库。

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
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
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    
    // >>> 关于下一个模块儿的使用方法，请参考客户服务。
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
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

#### Firebase Notification

* 使用Android Studio build时
    * 使用Firebase推送功能时，请参考以下指南设置Firebase之后，将google-services.json文件添加在项目中。 
		* [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud Push > Android > 设置Firebase Cloud Messaging](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* 如果使用Unity build
    * 如果已设置Firebase Unity SDK Package，则使用以下命令执行**generate_xml_from_google_services_json.exe**文件，将json文件转换为xml文件。
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
    * 如果未设置Firebase Unity SDK Package，则需在”Firebase Console > 项目设置中下载google-services.json文件，按照如下指南制作string resource(xml)文件后，添加在”Assets/Plugins/Android/res/values/”文件夹里。
        根据Firebase服务联动，google-services.json文件的内容可能会有所不同。
        根据Firebase服务联动，google-services.json文件的内容可能会有所不同。
        ![Download google-services.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 以下为string resource(xml)文件的示例。

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

* 如果是Unreal打包，当前在发布包含空google-service-json.xml文件的Gamebase Unreal SDK，因此需要修改为符合相关游戏信息的值。
    * 如果有自动生成类似于EasyFirebase格式的XML的Content，因资源的重复，可能出现打包错误。这时需要删除google-service-json.xml文件。

### AndroidManifest.xml

#### Facebook IdP

* 初始化Facebook SDK时，需要在AndroidManifest.xmlApp中声明App ID。
    * 如下列所示，若设置方式是可以参考resources的，则不必将其值直接在AndroidManifest.xml中声明。
    * 但目前，在Gamebase SDK内已调用初始化Facebook SDK的函数，不必设置。

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

* 因已在Line SDK中声明为**android:allowBackup="false"**，应用程序打包时，有可能在Manifest merger出现fail。如果打包失败，按照下面的示例，请将**tools:replace=“android:allowBackup”**声明添加到应用程序标签中。

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

#### Weibo IdP

* 为了使Weibo IdP正常启动，需要在**application**标签中添加**android:networkSecurityConfig** attribute，并需要设置声明为与weibo, sina相关的URL的xml文件名。 

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

* ONE store支持支付页面和支付页面弹窗。
    * 通过在AndroidManifest.xml添加meta-data，可选择支付页面("full")或支付页面弹窗("popup")。
    * 如果不设置meta-data，则适用默认值("full")。

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup : 支付页面弹窗 / full : 支付页面 -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| 支付页面 | 设定值 |
| --- | --- |
| 支付页面 | "full" |
| 支付页面弹窗 | "popup" |

#### Notification Options

* 请按以下方式设置Notification Options。

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
| com.toast.sdk.push.notification.default_priority | int | 优先顺序<br/>可设置以下5个值。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 背景色 |
| com.toast.sdk.push.notification.default_light_color | int | LED色 |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED灯亮的时间 |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED灯灭的时间 |
| com.toast.sdk.push.notification.default_small_icon | resource id | 小图标的资源标识符 |
| com.toast.sdk.push.notification.default_sound | String | 提示音文件名<br/>只在安卓8.0以下OS上运行。<br/>如果指定”res/raw”文件夹的mp3、wav文件名，则可更改提示音。 |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 震动模式 |
| com.toast.sdk.push.notification.badge_enabled | boolean | 是否使用badge图标 |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 是否使用foreground通知 |

### Android 11

* Android 11打包时，只能运行提前在AndroidManifest.xml中声明的应用程序。
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* 为此，如果要将targetSdkVersion设置为30以上，通过**queries**标签必须将允许运行的应用程序在AndroidManifest.xml中声明。

> <font color="red">[注意]</font><br/>
>
> * 如果以前的Android Gradle Plugin(AGP)认知不到”queries”标签，将出现打包失败。
> * 通过参考以下指南和列表，请升级到可进行”queries”标签打包的AGP版本。 
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * 如果使用AGP 3.2.*以下版本，请升级到3.3.3以上。
>     * 如果使用AGP 4.1.0以上版本，不用进行AGP升级。

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
        
        <!-- [Naver] Configurations begin -->
        <package android:name="com.nhn.android.search" />
        <!-- [Naver] Configurations end -->
        
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
    <!-- [Android11] settings end -->
</manifest>
```

### Proguard

* 当Proguard适用于gamebase 2.21.0及2.21.0之前版本时，调用结算API而不添加Proguard Rule时会发生崩溃。
    * 在Gamebase 2.21.0版本上已被修改。

```
# ---------------------- [Gamebase TOAST IAP] defines start ----------------------
# For using reflection
-keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
# ---------------------- [Gamebase TOAST IAP] defines end ----------------------
```
 
## Recommended Flow

* 可在Sample Project上确认Gamebase推荐的flow。
    * Android Sample Project
        * 请参考以下链接的GamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/ko/Download/#game-gamebase)
            *  GamebaseManager.java文件
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* 当游戏开始，初始化Gamebase客户端SDK，若登录成功，则进行”支付再处理”。

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.19.0.png)

* 通过以下链接确认具体的Flow。
    * [Game > Gamebase > Android SDK 使用指南 > 初始化 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 认证 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 结算 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)

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
* [Kakaogame SDK Guide for Channeling](https://tech-wiki.kakaogames.com/display/SDK/Kakaogame+SDK+Guide+for+Channeling)

## API Reference

* API Reference已被包含在SDK中。

## API Deprecate Governance

对Gamebase已不再支持的API，进行Deprecate处理。
满足以下条件时，可以删除Deprecated的API，不另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix
* 至少5个月
