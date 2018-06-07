## Game > Gamebase > Android Developer's Guide > Getting Started

To execute Gamebase in Android, following system environment is required.

### Environments


> [Minimum Specifications]
>
> Android API 15 (IceCreamSandwichMR1, 4.0.3) or higher <br/>
> Development Environment: Android Studio

### Installation

Download Gamebase Android SDK.

Before applying Gamebase Android SDK, you need an App ID issued at the TOAST Cloud Console: select a project created in the TOAST Cloud Console and click **(+)Service** Game > Gamebase.

#### Download

* [Download Gamebase Android SDK](/Download/#game-gamebase)
* Add aar files in the following folder of the downloaded SDK to a project.
    * **gamebase-sdk/**
* Add an authentication module.
    * Add the **gamebase-adapter-auth-{provider}** folder of downloaded SDK to project.
    * Add all authentication modules among Google, Facebook, Naver, and PAYCO.


#### Package Includes (SDK)

* Decompress the SDK package and modules as below will show.
  ![Package Includes](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-installation-001_1.5.0.png)

### Setting build.gradle

* 1) Copy downloaded Gamebase SDK to app's root path.
* 2) Set the path and version of Gamebase, authentication to be used, and purchase/push modules.
    * When a module is set true, the module will be added to dependency.
    * Set false for the modules that are not included because of not in use.

```gradle
/* Set the Gamebase path. */
def gamebaseDir = '../Gamebase'

/* Set the Gamebase version. */
def gamebaseSdkVersion = '1.9.0'
def gamebaseFacebookAdapterVersion = '1.7.0'
def gamebaseGoogleAdapterVersion = '1.9.0'
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

* 3) Add below to repositories.

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

* 4) Add below to dependencies.

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

Gamebase SDK ensures that those modules with 3rd Party SDK and dependency are interchangeable.

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

* "Required" refers to the modules that must be included.
* "Optional" refers to the modules that must be included when specific functions are in need.
* Need to include only one of the duplicate modules in Dependency.

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)

## API Reference

API Reference is included in SDK.

