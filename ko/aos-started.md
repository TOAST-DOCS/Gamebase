## Game > Gamebase > Android SDK 사용 가이드 > 시작하기

Android에서 Gamebase를 사용하기 위한 시스템 환경은 다음과 같습니다.

### Environments


> [최소 사양]
>
> Android API 15 (IceCreamSandwichMR1, 4.0.3) 이상 <br/>
> 개발 환경: Android Studio

### Installation

Gamebase Android SDK 파일을 다운로드합니다.

Gamebase Android SDK를 사용하기 전에 TOAST Console에서 앱 아이디를 발급받아야 합니다. 앱 아이디를 발급받으려면 TOAST Console에서 **(+)서비스 선택**을 클릭하여 Game > Gamebase를 클릭하여 서비스를 활성화 합니다.

#### Download

* [Download Gamebase Android SDK](/Download/#game-gamebase)
* 다운로드 받은 SDK에서 다음 폴더안의 aar 파일을 프로젝트에 추가합니다.
    * **gamebase-sdk/**
* 인증 모듈 추가
    * 다운로드한 SDK의 **gamebase-adapter-auth-{provider}** 폴더를 프로젝트에 추가합니다.
    * Google, Facebook, PAYCO 중에서 사용할 인증 모듈을 모두 추가합니다.


#### Package Includes (SDK)

* SDK 패키지의 압축을 풀면 다음과 같은 모듈이 표시됩니다.
  ![Package Includes](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-installation-001_1.5.0.png)

### Setting build.gradle

* 1) 다운로드한 Gamebase SDK를 앱의 루트(root) 경로에 복사합니다.
* 2) Gamebase 경로 및 버전, 사용할 인증, 결제, 푸시 모듈을 설정합니다.
    * 사용하고자 하는 모듈을 true로 설정하면 해당 모듈이 dependency에 추가됩니다.
    * 반대로 사용하지 않아 포함시키지 않는 모듈은 false로 설정합니다.

```gradle
/* Set the Gamebase path. */
def gamebaseDir = '../Gamebase'

/* Set the Gamebase version. */
def gamebaseSdkVersion = '1.8.0'
def gamebaseFacebookAdapterVersion = '1.7.0'
def gamebaseGoogleAdapterVersion = '1.7.0'
def gamebaseNaverAdapterVersion = '1.7.0'
def gamebasePaycoAdapterVersion = '1.7.0'
def gamebaseIAPAdapterVersion = '1.3.0'		// Not all adapters have the same version.
def gamebaseFCMAdapterVersion = '1.7.0'
def gamebaseTencentAdapterVersion = '1.7.0'

/* Set if defined google-services plugin */
def useGoogleServicesPlugin = true

/* Set the Gamebase authentication modules. */
def useAuthFacebook = true;
def useAuthGoogle = true;
def useAuthNaver = true;
def useAuthPayco = true;

/* Set the Gamebase purchase modules. */
def usePurchaseIAP = true;
def usePurchaseIAPOneStore = true;

/* Set the Gamebase push modules. */
def usePushFCM = true;
def usePushTencent = false; // Do not use all push modules. Select one.
```

* 3) 아래 내용을 repositories에 추가합니다.

```gradle
repositories {
    flatDir {
        dirs "${gamebaseDir}"
        dirs "${gamebaseDir}/gamebase-sdk"
        dirs "${gamebaseDir}/gamebase-adapter-auth-google"
        dirs "${gamebaseDir}/gamebase-adapter-auth-facebook"
        dirs "${gamebaseDir}/gamebase-adapter-auth-naver"
        dirs "${gamebaseDir}/gamebase-adapter-auth-payco"
        dirs "${gamebaseDir}/gamebase-adapter-purchase-iap"
        dirs "${gamebaseDir}/gamebase-adapter-push-fcm"
        dirs "${gamebaseDir}/gamebase-adapter-push-tencent"
    }
}
```

* 4) 아래 내용을 dependencies에 추가합니다.

```gradle
dependencies {
    ...

    // if defined "apply plugin: 'com.google.gms.google-services'"
    if (useGoogleServicesPlugin) {
        implementation 'com.android.support:support-v4:27.0.2'
        implementation 'com.google.firebase:firebase-core:11.8.0'
    }

    // Compile Gamebase External Libraries
    implementation 'com.android.support:support-compat:27.0.2'
    implementation 'com.google.code.gson:gson:2.2.4'
    implementation 'com.squareup.okhttp3:okhttp:3.6.0'

    // Compile Gamebase SDKs
    implementation (name: "gamebase-sdk-base-${gamebaseSdkVersion}", ext: 'aar')
    implementation (name: "gamebase-sdk-${gamebaseSdkVersion}", ext: 'aar')

    // Compile Authentication Modules
    if (useAuthFacebook) {
        implementation 'com.android.support:support-v4:27.0.2'
        implementation 'com.android.support:appcompat-v7:27.0.2'
        implementation 'com.android.support:cardview-v7:27.0.2'
        implementation 'com.android.support:customtabs:27.0.2'
        implementation 'com.facebook.android:facebook-login:4.30.0'
        implementation(name: "gamebase-adapter-auth-facebook-${gamebaseFacebookAdapterVersion}", ext: 'aar')
    }
    if (useAuthGoogle) {
        implementation 'com.android.support:support-v4:27.0.2'
        implementation 'com.google.android.gms:play-services-auth:11.8.0'
        implementation(name: "gamebase-adapter-auth-google-${gamebaseGoogleAdapterVersion}", ext: 'aar')
    }
    if (useAuthNaver) {
        implementation 'com.android.support:appcompat-v7:27.0.2'
        implementation 'com.android.support:support-core-utils:27.0.2'
        implementation 'com.android.support:customtabs:27.0.2'
        implementation 'com.android.support:support-v4:27.0.2'
        implementation 'com.naver.nid:naveridlogin-android-sdk:4.2.0'
        implementation(name: "gamebase-adapter-auth-naver-${gamebaseNaverAdapterVersion}", ext: 'aar')
    }
    if (useAuthPayco) {
        implementation('com.google.android.gms:play-services-basement:11.8.0') {
            transitive = false
        }
        implementation(name: 'paycologin-1.3.2', ext: 'aar')
        implementation(name: "gamebase-adapter-auth-payco-${gamebasePaycoAdapterVersion}", ext: 'aar')
    }

    // Compile Purchase Modules
    if (usePurchaseIAP) {
        implementation 'com.toast.iap:iap:1.3.8'
        implementation(name: "gamebase-adapter-purchase-iap-${gamebaseIAPAdapterVersion}", ext: 'aar')

        if (usePurchaseIAPOneStore) {
            implementation(name: "iap-tstore-1.3.8", ext: 'aar')
        }
    }

    // Compile Push Modules
    if (usePushFCM) {
        implementation 'com.android.support:support-v4:27.0.2'
        implementation 'com.google.android.gms:play-services-gcm:11.8.0'
        implementation 'com.google.firebase:firebase-messaging:11.8.0'
        implementation(name: 'pushsdk-1.4.2', ext: 'aar')
        implementation(name: "gamebase-adapter-push-fcm-${gamebaseFCMAdapterVersion}", ext: 'aar')
    } else if (usePushTencent) {
        compile(name: 'pushsdk-tencent-1.4.2', ext: 'aar')
        implementation(name: "gamebase-adapter-push-tencent-${gamebaseTencentAdapterVersion}", ext: 'aar')
    }
}
```

## Dependency

Gamebase SDK에서는 3rd Party SDK 및 Dependency가 있는 모듈의 버전에 대하여 호환을 보장합니다.

| Category                         | Provider        | Modules                                  | Dependencies                             | Description              |
| -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------- | ------------------------ |
| **Gamebase<br>(required)**       | Gamebase        | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-27.0.2.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |                          |
| **Authentication<br>(optional)** | Google          | gamebase-adapter-auth-google-{version}.aar | play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>play-services-tasks-11.8.0.aar<br>play-services-auth-11.8.0.aar<br>play-services-auth-base-11.8.0.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Facebook        | gamebase-adapter-auth-facebook-{version}.aar | facebook-core-4.30.0.aar<br>facebook-common-4.30.0.aar<br>facebook-login-4.30.0.aar<br>appcompat-v7-27.0.2.aar<br>support-vector-drawable-27.0.2.aar<br>animated-vector-drawable-27.0.2.aar<br>cardview-v7-27.0.2.aar<br>customtabs-27.0.2.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Naver           | gamebase-adapter-auth-naver-{version}.aar | naveridlogin-android-sdk-4.2.0.aar<br>animated-vector-drawable-27.0.2.aar<br>appcompat-v7-27.0.2.aar<br>customtabs-27.0.2.aar<br>support-vector-drawable-27.0.2.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Payco           | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.3.2.aar<br>play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>gson-2.2.4.jar |                          |
| **Purchase<br>(optional)**       | IAP             | gamebase-adapter-purchase-iap-{version}.aar | iap-1.3.8.aar<br>mobill-core-1.3.8.aar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |                          |
|                                  | IAP - ONE store |                                          | iap-tstore-1.3.8.aar | ONE store 사용 시 추가해야 합니다. |
| **Push<br>(optional)**           | FCM             | gamebase-adapter-push-fcm-{version}.aar  | pushsdk-1.4.2.aar<br>firebase-common-11.8.0.jar<br>firebase-iid-11.8.0.jar<br>firebase-messaging-11.8.0.aar<br>play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>play-services-gcm-11.8.0.aar<br>play-services-iid-11.8.0.aar<br>play-services-tasks-11.8.0.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Tencent         | gamebase-adapter-push-tencent-{version}.aar | pushsdk-tencent-1.4.2.aar |                          |

* required 항목은 필수로 포함해야 하는 모듈입니다.
* optional 항목은 해당 기능이 필요할 경우 포함해야 하는 모듈입니다.
* 중복되는 Dependency 모듈은 하나만 포함해야 합니다.

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)

## API Reference

API Reference는 SDK 내에 포함돼 있습니다.

