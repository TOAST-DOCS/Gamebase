## Game > Gamebase > Android SDK ご利用ガイド > はじめる

AndroidでGamebaseを利用するためのシステム環境は、次の通りです。

### Environments


> [最小仕様]
>
> Android API 15(IceCreamSandwichMR1, 4.0.3)以上 <br/>
> 開発環境:Android Studio

### Installation

Gamebase Android SDKファイルをダウンロードします。

Gamebase Android SDKを使用する前に、TOAST ConsoledからアプリIDを発行する必要があります。アプリIDを発行するためには、TOAST Consoleから**(+)サービス選択**をクリックし、Game > Gamebaseをクリックしてサービスを有効にします。

#### Download

* [Download Gamebase Android SDK](/Download/#game-gamebase)
* ダウンロードしたSDKから次のフォルダにあるaarファイルをプロジェクトに追加します。
    * **gamebase-sdk/**
* 認証モジュール追加
    * ダウンロードしたSDKの**gamebase-adapter-auth-{provider}**フォルダをプロジェクトに追加します。
    * Google、Facebook、PAYCOの中から使用する認証モジュールをすべて追加します。


#### Package Includes (SDK)

* SDKパッケージを解凍すると、次のようなモジュールが表示されます。
  ![Package Includes](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-installation-001_1.5.0.png)

### Setting build.gradle

* 1) ダウンロードしたGamebase SDKをアプリのルート(root)にコピーします。
* 2) Gamebaseのルート及びバージョン、使用する認証、決済、Pushモジュールを設定します。
    * 使用したいモジュールをtrueに設定すると、該当するモジュールがdependencyに追加されます。
    * 逆に、使用しないため含ませないモジュールはfalseに設定します。

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

* 3) 次の内容をrepositoriesに追加します。

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

* 4) 次の内容をdependenciesに追加します。

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

Gamebase SDKでは、3rd Party SDK及びDependencyのあるモジュールのバージョンに対し互換性を保障します。

| Category                         | Provider        | Modules                                  | Dependencies                             | Description              |
| -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------- | ------------------------ |
| **Gamebase<br>(required)**       | Gamebase        | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-27.0.2.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |                          |
| **Authentication<br>(optional)** | Google          | gamebase-adapter-auth-google-{version}.aar | play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>play-services-tasks-11.8.0.aar<br>play-services-auth-11.8.0.aar<br>play-services-auth-base-11.8.0.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Facebook        | gamebase-adapter-auth-facebook-{version}.aar | facebook-core-4.30.0.aar<br>facebook-common-4.30.0.aar<br>facebook-login-4.30.0.aar<br>appcompat-v7-27.0.2.aar<br>support-vector-drawable-27.0.2.aar<br>animated-vector-drawable-27.0.2.aar<br>cardview-v7-27.0.2.aar<br>customtabs-27.0.2.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Naver           | gamebase-adapter-auth-naver-{version}.aar | naveridlogin-android-sdk-4.2.0.aar<br>animated-vector-drawable-27.0.2.aar<br>appcompat-v7-27.0.2.aar<br>customtabs-27.0.2.aar<br>support-vector-drawable-27.0.2.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Payco           | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.3.2.aar<br>play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>gson-2.2.4.jar |                          |
| **Purchase<br>(optional)**       | IAP             | gamebase-adapter-purchase-iap-{version}.aar | iap-1.3.8.aar<br>mobill-core-1.3.8.aar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |                          |
|                                  | IAP - ONE store |                                          | iap-tstore-1.3.8.aar | ONE store使用時に追加する必要があります。|
| **Push<br>(optional)**           | FCM             | gamebase-adapter-push-fcm-{version}.aar  | pushsdk-1.4.2.aar<br>firebase-common-11.8.0.jar<br>firebase-iid-11.8.0.jar<br>firebase-messaging-11.8.0.aar<br>play-services-base-11.8.0.aar<br>play-services-basement-11.8.0.aar<br>play-services-gcm-11.8.0.aar<br>play-services-iid-11.8.0.aar<br>play-services-tasks-11.8.0.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-27.0.2.jar<br>support-compat-27.0.2.aar<br>support-core-ui-27.0.2.aar<br>support-core-utils-27.0.2.aar<br>support-fragment-27.0.2.aar<br>support-media-compat-27.0.2.aar<br>support-v4-27.0.2.aar |                          |
|                                  | Tencent         | gamebase-adapter-push-tencent-{version}.aar | pushsdk-tencent-1.4.2.aar |                          |

* required項目は、必須で含めるべきモジュールです。
* optional項目は、該当する機能が必要な場合に含めるべきモジュールです。
* 重複するDependencyモジュールは、一つだけ含めてください。

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)

## API Reference

API Referenceは、SDKの中に含まれています。

