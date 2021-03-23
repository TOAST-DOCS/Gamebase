## Game > Gamebase > Android SDK 사용 가이드 > 시작하기

## Environments

Android에서 Gamebase를 사용하기 위한 시스템 환경은 다음과 같습니다.

> [최소 사양]
>
> * Android API 16 (JellyBean, OS 4.1) 이상
>     * Twitter Login 은 19(Kitkat, 4.4) 이상
>     * AppleID Login 은 19(Kitkat, 4.4) 이상
>     * Weibo Login 은 19(Kitkat, 4.4) 이상
>     * GALAXY Store 는 21(Lollipop, 5.0) 이상
>         * 갤럭시 IAP SDK 의 minSdkVersion 은 18(OS 4.3) 이므로 이보다 작은 값을 설정하는 경우 빌드가 실패합니다.
>         * 하지만 실제 결제를 위해서는 Checkout 서비스앱의 설치가 필요한데, Chekcout 서비스앱은 API 21(OS 5.0. Lollipop) 미만에서는 설치가 실패하므로 결제를 진행할 수 없습니다.
> * Gradle Android Plugin 2.3.0 이상
> * 개발 환경: Android Studio

## Setting

### Console

* Gamebase Android SDK를 사용하기 전에 NHN Cloud Console에서 앱 아이디를 발급받아야 합니다. 앱 아이디를 발급받으려면 NHN Cloud Console 에서 **(+)서비스 선택**을 클릭하여 **Game > Gamebase** 를 클릭하여 서비스를 활성화 합니다.
* 인증을 위해 IdP 콘솔에서 client id 를 발급받아 Gamebase 콘솔에 입력합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Authentication Information](./oper-app/#authentication-information)
    * 3rd-Party Provider SDK Guide
        * [Facebook for developers](https://developers.facebook.com/docs/android)
        * [Google APIs for Android](https://developers.google.com/android/guides/overview)
        * [Naver for developers](https://developers.naver.com/docs/login/android/)
        * [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
        * [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
        * [Line for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
        * [Payco Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
        * [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
        * [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/blob/master/2019SDK/文档)
* 아이템 구매를 위해 Store 콘솔에서 앱 정보를 등록하여 Gamebase > 구매(IAP) 콘솔에 입력합니다.
	* [Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드](./console-google-guide)
	* [Game > Gamebase > 스토어 콘솔 가이드 > ONEStore 콘솔 가이드](./console-onestore-guide)
	* [Game > Gamebase > 스토어 콘솔 가이드 > GALAXY Store 콘솔 가이드](./console-galaxy-guide)
    * 아래 가이드를 참고하여 아이템을 등록합니다.
        * [Game > Gamebase > 콘솔 사용 가이드 > 결제 > Register](./oper-purchase/#register_1)
* 푸시 알림을 위해 푸시 알림 서비스 인증서를 Notification > Push > 인증서 콘솔에 입력합니다.
    * [Notification > Push > Console Guide](/Notification/Push/ko/console-guide/)
* 새로운 Gamebase 프로젝트가 생성되었으니 AppVersion 과 StoreCode 를 등록해야 합니다.
    * 다음 가이드를 따라 새로운 클라이언트 버전을 등록하시기 바랍니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Client > Client List](./oper-app/#client-list)

### Regist as Tester

#### Gamebase Test Device

* 점검중에도 정상적으로 게임에 접근하고자 한다면 Gamebase 콘솔에 테스트 단말기를 등록합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Test Device](./oper-app/#test-device)

#### Store's Tester

* 결제 테스트를 위하여 스토어별로 다음과 같이 테스터로 등록합니다.(Gamebase Tester 등록이 아닌, 스토어의 테스트 결제를 위한 설정입니다.)
    * Google
        * [Android > 테스트 구매 설정](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > APPS > 상품현황 > In-App정보 > 결제테스트 > 테스트 ID 등록/관리](https://dev.onestore.co.kr/wiki/ko/doc/%EA%B0%9C%EB%B0%9C%EB%8F%84%EA%B5%AC/api-v5-sdk-v17/%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%B0%8F-%EB%B3%B4%EC%95%88#id-%EA%B2%B0%EC%A0%9C%ED%85%8C%EC%8A%A4%ED%8A%B8%EB%B0%8F%EB%B3%B4%EC%95%88-%ED%85%8C%EC%8A%A4%ED%8A%B8ID%EB%93%B1%EB%A1%9D/%EA%B4%80%EB%A6%AC)
    * GALAXY store
        * [GALAXY store > 앱 > 등록한 앱 > 바이너리 > Beta Test > Tester 설정](https://seller.samsungapps.com/application)
        * 삼성 단말기에서만 결제 테스트가 가능합니다.

### Gradle

* 사용할 Gamebase 버전, 사용할 인증, 결제, 푸시 모듈을 build.gradle 파일에 선언하세요.
	* Gamebase 최신 버전은 [jCenter(LINK)](https://jcenter.bintray.com/com/toast/android/gamebase/gamebase-sdk/) 에서 확인할 수 있습니다.
	* Gamebase 에서 의존하는 라이브러리 다운로드를 위해 **mavenCentral()** 저장소를 추가 해주세요.

```groovy
repositories {
    //  >>> For the 'kotlin-gradle-plugin'
    maven { url "https://plugins.gradle.org/m2/" }

    jcenter()
    mavenCentral()
    ...

    // >>> [Weibo IdP]
    maven { url 'https://dl.bintray.com/thelasterstar/maven/' }

    // >>> [Hangame IdP]
    maven { url 'Hangame IdP 설정 방법은 고객 센터로 문의 하시기 바랍니다.' }
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

* 다음 경로에서 Weibo so 라이브러리를 복사하여 app/src/main/jniLibs 폴더에 추가해야 합니다.
    * [https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so](https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so)
    * 필요한 플랫폼만 사용하면 됩니다.

### Resources

#### Firebase Notification

* Android Studio 빌드인 경우
    * Firebase 푸시를 사용하려면 아래 가이드에 따라 Firebase 설정을 완료한 후 google-services.json 파일을 프로젝트에 포함시켜야 합니다.
		* [NHN Cloud > NHN Cloud SDK 사용 가이드 > NHN Cloud Push > Android > Firebase Cloud Messaging 설정](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* Unity 빌드인 경우
    * 만일 Firebase Unity SDK Package 를 설치했다면 아래 명령어로 **generate_xml_from_google_services_json.exe** 파일을 실행하여 json 파일을 xml 파일로 변환시킬 수 있습니다.
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
    * Firebase Unity SDK Package 를 설치하지 않았다면, 'Firebase Console > 프로젝트 설정' 에서 google-services.json 파일을 다운로드 하여 아래 가이드에 따라 string resource(xml) 파일을 직접 만들어서 'Assets/Plugins/Android/res/values/' 폴더에 포함시켜야 합니다.
        Firebase 서비스 연동에 따라서 google-services.json 파일의 내용은 달라질 수 있습니다.
        ![Download google-services.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 다음은 직접 제작한 string resource(xml) 파일의 예시입니다.

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

#### Facebook IdP

* targetSdkVersion 을 30 이상으로 설정하는 경우, **queries** 태그 선언이 필요합니다.
    * 그렇지 않으면 Android 11 이상의 단말기에서는 Facebook 앱이 설치되어 있더라도 빠른 로그인(Express login)이 아닌, ID/PW 를 직접 입력해야 하는 웹로그인 창이 뜨게 됩니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 Gradle 5.6.4 이상 버전에서만 빌드가 가능합니다.
>     * 그러므로 IDE 에서 Gradle 5.6.4 이상이 지원되지 않는 환경에서는 targetSdkVersion 을 29 이하로 설정해야 Android 11 이상의 단말기에서도 빠른 로그인이 가능합니다.
> * Gradle 5.6.4 이상 버전이 적용된 IDE 는 다음과 같습니다.
>     * Android Studio : 3.6.1 이상
>     * Unity : 2020.1 이상
>     * Unreal : 지원 불가

```xml
<!-- [Facebook] Configurations begin -->
<queries>
    <package android:name="com.facebook.katana" />
</queries>
<!-- [Facebook] Configurations end -->
```

#### Payco IdP

* targetSdkVersion 을 30 이상으로 설정하는 경우, **queries** 태그 선언이 필요합니다.
    * 그렇지 않으면 Android 11 이상의 단말기에서는 Payco 앱이 설치되어 있더라도 간편한 앱로그인이 아닌, ID/PW 를 직접 입력해야 하는 웹로그인 창이 뜨게 됩니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 Gradle 5.6.4 이상 버전에서만 빌드가 가능합니다.
>     * 그러므로 IDE 에서 Gradle 5.6.4 이상이 지원되지 않는 환경에서는 targetSdkVersion 을 29 이하로 설정해야 Android 11 이상의 단말기에서도 앱로그인이 가능합니다.
> * Gradle 5.6.4 이상 버전이 적용된 IDE 는 다음과 같습니다.
>     * Android Studio : 3.6.1 이상
>     * Unity : 2020.1 이상
>     * Unreal : 지원 불가

```xml
<!-- [Payco] Configurations begin -->
<queries>
    <package android:name="com.nhnent.payapp" />
</queries>
<!-- [Payco] Configurations end -->
```

#### Hangame IdP

* Hangame IdP 정상 동작을 위한 AndroidManifest.xml 설정은 고객 센터로 문의 하시기 바랍니다.

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

* Weibo IdP 가 정상 동작 하기 위해서는 **application** 태그에 **android:networkSecurityConfig** attribute 를 추가하고, weibo, sina 관련 URL 을 선언한 xml 파일 이름을 설정해야 합니다.

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

* targetSdkVersion 을 30 이상으로 설정하는 경우, 반드시 **queries** 태그 선언이 필요합니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 Gradle 5.6.4 이상 버전에서만 빌드가 가능합니다.
>     * 그러므로 IDE 에서 Gradle 5.6.4 이상이 지원되지 않는 환경에서는 targetSdkVersion 을 29 이하로 설정해야 합니다.
> * Gradle 5.6.4 이상 버전이 적용된 IDE 는 다음과 같습니다.
>     * Android Studio : 3.6.1 이상
>     * Unity : 2020.1 이상
>     * Unreal : 지원 불가

```xml
<!-- [ONE store] Configurations begin -->
<queries>
    <intent>
        <action android:name="com.onestore.ipc.iap.IapService.ACTION" />
    </intent>
    <intent>
        <action android:name="android.intent.action.VIEW" />
        <data android:scheme="onestore" />
    </intent>
</queries>
<!-- [ONE store] Configurations end -->
```

* ONE store 는 전체 결제 화면과 팝업 결제 화면을 지원합니다.
    * AndroidManifest.xml에 meta-data를 추가하여 전체 결제 화면("full") 또는 팝업 결제 화면("popup")을 선택할 수 있습니다.
    * meta-data를 설정하지 않으면 기본값("full")이 적용됩니다.

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup:팝업 결제 화면 / full:전체 결제 화면 -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| 결제 화면 | 설정 값 |
| --- | --- |
| 전체 결제 화면 | "full" |
| 팝업 결제 화면 | "popup" |


#### Galaxy Store

* targetSdkVersion 을 30 이상으로 설정하는 경우, 반드시 **queries** 태그 선언이 필요합니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 Gradle 5.6.4 이상 버전에서만 빌드가 가능합니다.
>     * 그러므로 IDE 에서 Gradle 5.6.4 이상이 지원되지 않는 환경에서는 targetSdkVersion 을 29 이하로 설정해야 합니다.
> * Gradle 5.6.4 이상 버전이 적용된 IDE 는 다음과 같습니다.
>     * Android Studio : 3.6.1 이상
>     * Unity : 2020.1 이상
>     * Unreal : 지원 불가

```xml
<!-- [Galaxy store] Configurations begin -->
<queries>
    <package android:name="com.sec.android.app.samsungapps" />
</queries>
<!-- [Galaxy store] Configurations end -->
```

#### Notification Options

* 다음과 같은 방법으로 알림 옵션을 설정할 수 있습니다.

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
| com.toast.sdk.push.notification.default_priority | int | 우선 순위.<br/>아래 5가지 값을 설정할 수 있습니다.<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 배경색. |
| com.toast.sdk.push.notification.default_light_color | int | LED 색. |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED 불이 들어올 때의 시간. |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED 불이 나갈 때의 시간. |
| com.toast.sdk.push.notification.default_small_icon | resource id | 작은 아이콘의 리소스 식별자. |
| com.toast.sdk.push.notification.default_sound | String | 알림음 파일 이름.<br/>안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다. |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 진동의 패턴. |
| com.toast.sdk.push.notification.badge_enabled | boolean | 배지 아이콘 사용 여부. |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 포그라운드 알림 사용 여부. |

## Recommended Flow

* Gamebase 에서 권장하는 flow 는 Sample Project 에도 동일하게 구현되어 있습니다.
    * Android Sample Project
        * 아래 링크의 GamebaseAndroidSDK/sample
        * [https://docs.toast.com/ko/Download/#game-gamebase](https://docs.toast.com/ko/Download/#game-gamebase)
            * GamebaseManager.java 파일을 참고하시면 됩니다.
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* 게임이 시작되었을 때 Gamebase 클라이언트 SDK 를 초기화 하고 로그인이 성공하면 결제 재처리가 시작되도록 구현하세요.

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.19.0.png)

* 상세 flow 는 다음 링크에서 확인할 수 있습니다.
    * [Game > Gamebase > Android SDK 사용 가이드 > 초기화 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK 사용 가이드 > 인증 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK 사용 가이드 > 결제 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)

## API Reference

* API Reference는 SDK 내에 포함되어 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix
* 최소 5개월 경과

