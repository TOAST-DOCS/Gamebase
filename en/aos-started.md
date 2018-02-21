## Game > Gamebase > Android Developer's Guide > Getting Started

## Getting Started

To execute Gamebase in Android, following system environment is required.

### Environments

> [Minimum Specifications]
>
> Android API 15 (IceCreamSandwichMR1, 4.0.3) or higher<br/>
> Development Environment: Android Studio

### Installation

Download Gamebase Android SDK.

Before applying Gamebase Android SDK, you need an App ID issued at the Toast Cloud Console: select a project created in the TOAST Cloud Console and click **Enable** Gamebase.

#### Download

* [DOWNLOAD Gamebase Android SDK](/en/Download/#game-gamebase)
- Add aar files in the following folder of the downloaded SDK to a project.
    - **gamebase-sdk/**
- Add an authentication module.
    - Add the **gamebase-adapter-auth-{provider}** folder of downloaded SDK to project.
    - Add all authentication modules among Google, Facebook, and PAYCO.


#### Package Includes (SDK)

- Decompress the SDK package and modules as below will show.
    ![Package Includes](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-installation-001_1.5.0.png)

### Setting build.gradle


- 1) Copy downloaded Gamebase SDK to app's root path.
- 2) Set the path and version of Gamebase, authentication to be used, and purchase/push modules.
    - When a module is set true, the module will be added to dependency.
    - Set false for the modules that are not included because of not in use.

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

* 3) Add below to repositories.

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

* 4) Add below to dependencies.

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
        compile(name: 'iap-1.3.7', ext: 'aar')
        compile(name: 'mobill-core-1.3.7', ext: 'aar')
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

Gamebase SDK ensures that those modules with 3rd Party SDK and dependency are interchangeable.

| Category                         | Provider        | Modules                                  | Dependencies                             | Description              |
| -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------- | ------------------------ |
| **Gamebase<br>(required)**       | Gamebase        | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-24.0.0.aar<br>support-v4-24.0.0.aar<br>support-annotations-24.0.0.jar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |                          |
| **Authentication<br>(optional)** | Google          | gamebase-adapter-auth-google-{version}.aar | play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>play-services-auth-10.0.1.aar<br>play-services-auth-base-10.0.1.aar |                          |
|                                  | Facebook        | gamebase-adapter-auth-facebook-{version}.aar | facebook-android-sdk-4.17.0.aar<br>appcompat-v7-24.0.0.aar<br>support-vector-drawable-24.0.0.aar<br>animated-vector-drawable-24.0.0.aar<br>cardview-v7-24.0.0.aar<br>customtabs-24.0.0.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar |                          |
|                                  | Payco           | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.2.9.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>gson-2.2.4.jar |                          |
|                                  | Naver           | gamebase-adapter-auth-naver-{version}.aar | naveridlogin-android-sdk-4.2.0.aar<br>animated-vector-drawable-27.0.2.aar<br>appcompat-v7-27.0.2.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>customtabs-27.0.2.aar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar<br>support-vector-drawable-27.0.2.aar |                          |
| **Purchase<br>(optional)**       | IAP             | gamebase-adapter-purchase-iap-{version}.aar | iap-1.3.7.aar<br>mobill-core-1.3.7.jar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |                          |
|                                  | IAP - ONE store |                                          | iap-tstore-1.3.7.aar<br>iap_tstore_plugin_v16.03.00_20161123.jar | For ONE Store |
| **Push<br>(optional)**           | FCM             | gamebase-adapter-push-fcm-{version}.aar  | pushsdk-release-v1.4.0.aar<br>firebase-common-10.0.1.jar<br>firebase-iid-10.0.1.jar<br>firebase-messaging-10.0.1.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-gcm-10.0.1.aar<br>play-services-iid-10.0.1.aar<br>play-services-tasks-10.0.1.aar |                          |
|                                  | Tencent         | gamebase-adapter-push-tencent-{version}.aar | pushsdk-release-v1.4.0.aar<br>Xg_sdk_v3.1_20170417_0946.jar<br>jg_filter_sdk_1.1.jar<br>mid-core-sdk-3.7.2.jar<br>wup-1.0.0.E-SNAPSHOT.jar |                          |


- "Required" refers to the modules that must be included.
- "Optional" refers to the modules that must be included when specific functions are in need.
- Need to include only one of the duplicate modules in Dependency.


## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)

## API Reference

API Reference is included in SDK.

