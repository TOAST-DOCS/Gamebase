## Game > Gamebase > Android Developer's Guide  > Getting Started

## Getting Started

Android에서 Gamebase를 사용하기 위한 시스템 환경은 다음과 같습니다.

### Environments


> [최소 사양]
>
> Android API 15 (IceCreamSandwichMR1, 4.0.3) 이상 <br/>
> 개발 환경: Android Studio

### Installation

Gamebase Android SDK 파일을 다운로드합니다.

Gamebase Android SDK를 사용하기 전에 TOAST Cloud Console에서 앱 아이디를 발급받아야 합니다. 앱 아이디를 발급받으려면 TOAST Cloud Console에서 생성한 프로젝트를 선택하고 Gamebase **상품 이용** 버튼을 클릭합니다.

#### Download

* [DOWNLOAD Gamebase Android SDK](http://docs.cloud.toast.com/ko/Download/#upcoming-products-gamebase)
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
def gamebaseSdkVersion = '1.0.0'

/* Set the Gamebase authentication modules. */
def useAuthFacebook = true;
def useAuthGoogle = true;
def useAuthPayco = true;

/* Set the Gamebase purchase modules. */
def usePurchaseIAP = true;

/* Set the Gamebase push modules. */
def usePushFCM = true;
def usePushTencent = false; // Not supported yet.
```

* 3) 아래 내용을 repositories에 추가합니다.

```gradle
repositories {
    flatDir {
        dirs "${gamebaseDir}"
        dirs "${gamebaseDir}/gamebase-sdk"
        dirs "${gamebaseDir}/gamebase-adapter-auth-google"
        dirs "${gamebaseDir}/gamebase-adapter-auth-facebook"
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

    // Compile Gamebase SDKs
    compile(name: "gamebase-sdk-${gamebaseSdkVersion}", ext: 'aar')
    compile(name: "gamebase-sdk-base-${gamebaseSdkVersion}", ext: 'aar')

    // Compile Gamebase External Libraries
    compile 'com.google.code.gson:gson:2.2.4'
    compile 'com.squareup.okhttp3:okhttp:3.6.0'

    // Compile Authentication Modules
    if (useAuthFacebook) {
        compile(name: "gamebase-adapter-auth-facebook-${gamebaseSdkVersion}", ext: 'aar')
        compile 'com.facebook.android:facebook-android-sdk:4.17.0'
    }
    if (useAuthGoogle) {
        compile 'com.google.android.gms:play-services-auth:10.0.1'
        compile(name: "gamebase-adapter-auth-google-${gamebaseSdkVersion}", ext: 'aar')
    }
    if (useAuthPayco) {
        compile(name: "gamebase-adapter-auth-payco-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'paycologin-1.2.9', ext: 'aar') {
            exclude group: 'com.google.code.gson', module: 'gson'
        }
        compile 'com.google.android.gms:play-services-base:10.0.1'
    }

    // Compile Purchase Modules
    if (usePurchaseIAP) {
        compile(name: "gamebase-adapter-purchase-iap-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'iap-1.3.5.1', ext: 'aar')
        compile(name: 'mobill-core-1.3.5.1', ext: 'jar')
        compile 'com.squareup.okhttp:okhttp:1.5.4'
    }

    // Compile Push Modules
    if (usePushFCM) {
        compile(name: "gamebase-adapter-push-fcm-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.4.1', ext: 'aar')
        compile 'com.google.android.gms:play-services-gcm:10.0.1'
        compile 'com.google.firebase:firebase-messaging:10.0.1'
    } else if (usePushTencent) {
        compile(name: "gamebase-adapter-push-tencent-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.4.1', ext: 'aar')
    }
}
```


## Dependency

Gamebase SDK에서는 3rd Party SDK 및 Dependency가 있는 모듈의 버전에 대하여 호환을 보장합니다.

| Category                         | Provider        | Modules                                  | Dependencies                             | Description              |
| -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------- | ------------------------ |
| **Gamebase<br>(required)**       | Gamebase        | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-24.0.0.aar<br>support-v4-24.0.0.aar<br>support-annotations-24.0.0.jar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |                          |
| **Authentication<br>(optional)** | Google          | gamebase-adapter-auth-google-{version}.aar | play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>play-services-auth-10.0.1.aar<br>play-services-auth-base-10.0.1.aar |                          |
|                                  | Facebook        | gamebase-adapter-auth-facebook-{version}.aar | facebook-android-sdk-4.17.0.aar<br>appcompat-v7-24.0.0.aar<br>support-vector-drawable-24.0.0.aar<br>animated-vector-drawable-24.0.0.aar<br>cardview-v7-24.0.0.aar<br>customtabs-24.0.0.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar |                          |
|                                  | Payco           | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.2.9.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>gson-2.2.4.jar |                          |
| **Purchase<br>(optional)**       | IAP             | gamebase-adapter-purchase-iap-{version}.aar | iap-1.3.2.20170424.aar<br>mobill-core-1.3.2.20170424.jar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |                          |
|                                  | IAP - ONE store |                                          | iap-tstore-1.3.2.20170424.aar<br>iap_tstore_plugin_v16.03.00_20161123.jar | ONE store 사용 시 추가해야 합니다. |
| **Push<br>(optional)**           | FCM             | gamebase-adapter-push-fcm-{version}.aar  | pushsdk-release-v1.4.0.aar<br>firebase-common-10.0.1.jar<br>firebase-iid-10.0.1.jar<br>firebase-messaging-10.0.1.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-gcm-10.0.1.aar<br>play-services-iid-10.0.1.aar<br>play-services-tasks-10.0.1.aar |                          |
|                                  | Tencent         | gamebase-adapter-push-tencent-{version}.aar | pushsdk-release-v1.4.0.aar<br>Xg_sdk_v3.1_20170417_0946.jar<br>jg_filter_sdk_1.1.jar<br>mid-core-sdk-3.7.2.jar<br>wup-1.0.0.E-SNAPSHOT.jar |                          |

* required 항목은 필수로 포함해야 하는 모듈입니다.
* optional 항목은 해당 기능이 필요할 경우 포함해야 하는 모듈입니다.
* 중복되는 Dependency 모듈은 하나만 포함해야 합니다.

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)

## API Reference

API Reference는 SDK 내에 포함돼 있습니다.

