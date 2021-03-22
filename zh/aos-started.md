## Game > Gamebase > Android SDK 使用指南 > 开始

## Environments

在Android上应用Gamebase，需要的系统环境如下。

> [最低版本]
>
> * Android API 16 (JellyBean, 4.1)以上
>     * Twitter Login 19 (Kitkat, 4.4)以上
>     * AppleID Login 19 (Kitkat, 4.4)以上
>     * GALAXY Store 21 (Lollipop, 5.0)以上
>         * GALAXY IAP SDK的minSdkVersion为18(OS 4.3)，如果设置小于18的值，则将导致build失败。
>         * 为进行实际Checkout，需要设置Service App。因在API 21(OS 5.0. Lollipop)以下不可以设置Chekcout Service App，无法进行结算。
> * Gradle Android Plugin 2.3.0 以上
> * 开发环境 : Android Studio

## Setting

### Console

* 在应用Gamebase Android SDK之前，您需要从TOAST Console获取App ID。获取应用App ID，请在TOAST Console上在单击**(+)服务**，然后点击**Game > Gamebase**以启用该服务。
* 进行认证时从IdP控制台获取client id，在Gamebase控制台中输入该ID。 
    * [Game > Gamebase > 控制台使用指南 > App > Authentication Information](./oper-app/#authentication-information)
    * 3rd-Party Provider SDK Guide
        * [Facebook for developers](https://developers.facebook.com/docs/android)
        * [Google APIs for Android](https://developers.google.com/android/guides/overview)
        * [Naver for developers](https://developers.naver.com/docs/login/android/)
        * [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
        * [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview)
        * [Line for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
        * [Payco Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
        * [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
        * [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/blob/master/2019SDK/文档)
* 如果想购买道具，在Store控制台中注册App信息，并在Gamebase > 购买(IAP)控制台中输入该信息。
	* [Game > Gamebase > Store控制台指南 > Google控制台指南](./console-google-guide) 
	* [Game > Gamebase > Store控制台指南 > ONEStore控制台指南](./console-onestore-guide)
	* [Game > Gamebase > Store控制台指南 > GALAXY Store控制台指南](./console-galaxy-guide)
    * 请参考以下指南注册道具。
        * [Game > Gamebase > 控制台使用指南 > 结算 > Register](./oper-purchase/#register_1)
* 为实现推送通知功能，在Notification > Push >认证书控制台中输入推送通知服务认证书。 
    * [Notification > Push > Console Guide](/Notification/Push/ko/console-guide/)
* 新Gamebase项目已生成，需要注册AppVersion和StoreCode。
    * 请参考如下指南注册新客户端版本。 
    * [Game > Gamebase > 控制台使用指南 > App > Client > Client List](./oper-app/#client-list)

### Regist as Tester

#### Gamebase Test Device

* 需要在维护中进入游戏时，在Gamebase控制台中注册测试终端机。
    * [Game > Gamebase > 控制台使用指南 > App > Test Device](./oper-app/#test-device)

#### Store's Tester

* 进行付费测试时，请按以下方式添加每个商店的测试人员。（此时不在Gamebase设置Tester，在Store控制台中设置。)
    * Google
        * [Android > 测试购买设置](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > INAPP应用内结算测试](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * 必须使用应用内信息 - 通过测试按钮注册想要沙盒的终端电话号码进行测试。
        * 测试终端必须有USIM并需要注册电话号码(MDN)。
        * 需要安装**ONE store**。
    * GALAXY store
        * [GALAXY store > App > 已注册App > Binary > Beta Test > 设置Tester](https://seller.samsungapps.com/application)
        * 只能在三星终端机上进行付费测试。 

### Gradle

* 请在build.gradle文件中声明要使用的Gamebase版本和要使用的验证、支付、推送模块。
	* Gamebase的最新版本可在[jCenter(LINK)](https://jcenter.bintray.com/com/toast/android/gamebase/gamebase-sdk/)中确认。
	* 为下载Gamebase依赖库，请添加**mavenCentral()**仓库。

```groovy
repositories {
    jcenter()
    mavenCentral()
    ...

    // >>> [Weibo IdP]
    maven { url 'https://dl.bintray.com/thelasterstar/maven/' }

    // >>> [Hangame IdP]
    maven { ”关于url Hangame IdP的设置方法，请询问客服中心” }
}

android {
    defaultConfig {
        // >>> [Weibo IdP]
        ndk {
            abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a'
        }
    }
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
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
}
```

### NDK Library

#### Weibo IdP

* 请从以下路径复制Weibo so库，放入app/src/main/jniLibs文件夹里。
    * [https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so](https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so)
    * 只需用必要的平台。

### Resources

#### Firebase Notification

* 使用Android Studio build时
    * 使用Firebase推送功能时，请参考如下指南设置完Firebase之后，将google-services.json文件添加在项目中。 
		* [TOAST > TOAST SDK使用指南 > TOAST Push > Android > 设置Firebase Cloud Messaging](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* 如果使用Unity build
    * 如果已设置Firebase Unity SDK Package，则使用以下命令执行**generate_xml_from_google_services_json.exe**文件，将json文件转换为xml文件。
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
    * 如果未设置Firebase Unity SDK Package，则需在”Firebase Console > 项目设置中”下载google-services.json文件，按照如下指南制作string resource(xml)文件后，添加在”Assets/Plugins/Android/res/values/”文件夹中。
        根据Firebase服务联动，google-services.json文件的内容可能会有所不同。
        ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 以下为string resource(xml)文件示例。

```xml
<!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="firebase_database_url" translatable="false">https://gamebase-sample-00000000.firebaseio.com</string>
    <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
    <string name="google_storage_bucket" translatable="false">gamebase-sample-00000000.appspot.com</string>
    <string name="project_id" translatable="false">gamebase-sample-00000000</string>
    <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
    <string name="google_app_id" translatable="false">1:000000000000:android:749cbe01c8ada279</string>
    <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
</resources>
```

### AndroidManifest.xml

#### Hangame IdP

* 关于通过在AndroidManifest.xml文件中声明使用Hangame IdP方法，请询问客服中心。

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

#### GALAXY Store

* 在GALAXY store可更改付费测试的成功失败与否及成功或失败时是否显示dialog。

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [GALAXY store] Configurations start -->
        <!-- OPERATION_MODE_TEST: 每次成功 / OPERATION_MODE_TEST_FAILURE: 每次失败 -->
        <meta-data
            android:name="com.toast.sdk.iap.galaxy.operation_mode"
            android:value="OPERATION_MODE_TEST | OPERATION_MODE_TEST_FAILURE" />
        <!-- 显示Error dialog -->
        <meta-data
            android:name="com.toast.sdk.iap.galaxy.error_dialog_enabled"
            android:value="true" />
        <!-- 显示支付成功dialog -->
        <meta-data
            android:name="com.toast.sdk.iap.galaxy.purchase_success_dialog_enabled"
            android:value="true" />
        <!-- [GALAXY store] Configurations end -->
        ...
    </application>
</manifest>
```

| meta-data key| 测试支付结果 | 设定值 |
| --- | --- | --- |
| com.toast.sdk.iap.galaxy.operation_mode<br/>**default** : NONE | 每次成功 | OPERATION_MODE_TEST |
| | 每次失败 | OPERATION_MODE_TEST_FAILURE |
| com.toast.sdk.iap.galaxy.error_dialog_enabled<br/>**default** : false | 出错时显示dialog弹窗。 | true |
| com.toast.sdk.iap.galaxy.purchase_success_dialog_enabled<br/>**default** : false | 支付成功时显示dialog弹窗。 | true |

#### Notification Options

* 请按如下方式设置Notification Options。

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
| com.toast.sdk.push.notification.default_priority | int | 优先顺序.<br/>可设置以下5个值。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 背景色 |
| com.toast.sdk.push.notification.default_light_color | int | LED色 |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED灯亮的时间 |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED灯灭的时间 |
| com.toast.sdk.push.notification.default_small_icon | resource id | 小图标的资源标识符 |
| com.toast.sdk.push.notification.default_sound | String | 提示音文件名<br/>只在安卓8.0 以下OS上运行。<br/>如果指定”res/raw”文件夹的mp3、wav文件名，则可更改提示音。 |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 震动模式 |
| com.toast.sdk.push.notification.badge_enabled | boolean | 是否使用badge图标 |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 是否使用foreground通知 |

## Recommended Flow

* 可以在Sample Project上确认Gamebase推荐的flow。
    * Android Sample Project
        * 以下链接的GamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/ko/Download/#game-gamebase)
            * 请参考GamebaseManager.java文件。
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* 当游戏开始时，初始化Gamebase客户端SDK，若登录成功，则进行”支付再处理”。

![overview flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.19.0.png)

* 通过如下链接确认具体的Flow。
    * [Game > Gamebase > Android SDK 使用指南 > 初始化 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 认证 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 结算 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)

## API Reference

* API Reference包含在SDK中。

## API Deprecate Governance

Gamebase不再支持的API，进行Deprecate处理。
在满足以下条件时，可以删除Deprecated的API，不再另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix
* 至少5个月

