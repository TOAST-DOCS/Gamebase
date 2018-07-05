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

/* Set external libraries version. */
def supportVersion = '27.0.2'
def playServicesVersion = '11.8.0'
def gsonVersion = '2.2.4'
def okhttpVersion = '3.6.0'
def facebookSdkVersion = '4.30.0'
def naverSdkVersion = '4.2.0'
def signpostVersion = '1.2.1.2'
def lineSdkVersion = '4.0.8'
def paycoSdkVersion = '1.3.2'
def iapSdkVersion = '1.3.8'
def pushSdkVersion = '1.4.2'

/* Set the Gamebase version. */
def gamebaseSdkVersion = '1.11.1'
def gamebaseFacebookAdapterVersion = '1.11.0'
def gamebaseGoogleAdapterVersion = '1.9.0'
def gamebaseNaverAdapterVersion = '1.7.0'
def gamebaseTwitterAdapterVersion = '1.11.0'
def gamebaseLineAdapterVersion = '1.11.0'
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
def useAuthTwitter = true;
def useAuthLine = true;
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
        dirs "${gamebaseDir}/gamebase-adapter-auth-twitter"
        dirs "${gamebaseDir}/gamebase-adapter-auth-line"
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
        implementation "com.android.support:support-v4:${supportVersion}"
        implementation "com.google.firebase:firebase-core:${playServicesVersion}"
    }

    // Compile Gamebase External Libraries
    implementation "com.android.support:support-compat:${supportVersion}"
    implementation "com.google.code.gson:gson:${gsonVersion}"
    implementation "com.squareup.okhttp3:okhttp:${okhttpVersion}"

    // Compile Gamebase SDKs
    implementation (name: "gamebase-sdk-base-${gamebaseSdkVersion}", ext: 'aar')
    implementation (name: "gamebase-sdk-${gamebaseSdkVersion}", ext: 'aar')

    // Compile Authentication Modules
    if (useAuthFacebook) {
        implementation "com.android.support:support-v4:${supportVersion}"
        implementation "com.android.support:appcompat-v7:${supportVersion}"
        implementation "com.android.support:cardview-v7:${supportVersion}"
        implementation "com.android.support:customtabs:${supportVersion}"
        implementation "com.facebook.android:facebook-login:${facebookSdkVersion}"
        implementation(name: "gamebase-adapter-auth-facebook-${gamebaseFacebookAdapterVersion}", ext: 'aar')
    }
    if (useAuthGoogle) {
        implementation "com.android.support:support-v4:${supportVersion}"
        implementation "com.google.android.gms:play-services-auth:${playServicesVersion}"
        implementation(name: "gamebase-adapter-auth-google-${gamebaseGoogleAdapterVersion}", ext: 'aar')
    }
    if (useAuthNaver) {
        implementation "com.android.support:appcompat-v7:${supportVersion}"
        implementation "com.android.support:support-core-utils:${supportVersion}"
        implementation "com.android.support:customtabs:${supportVersion}"
        implementation "com.android.support:support-v4:${supportVersion}"
        implementation "com.naver.nid:naveridlogin-android-sdk:${naverSdkVersion}"
        implementation(name: "gamebase-adapter-auth-naver-${gamebaseNaverAdapterVersion}", ext: 'aar')
    }
    if (useAuthTwitter) {
        implementation "oauth.signpost:signpost-core:${signpostVersion}"
        implementation(name: "gamebase-adapter-auth-twitter-${gamebaseTwitterAdapterVersion}", ext: 'aar')
    }
    if (useAuthLine) {
        implementation "com.android.support:appcompat-v7:${supportVersion}"
        implementation "com.android.support:customtabs:${supportVersion}"
        implementation(name: "line-sdk-${lineSdkVersion}", ext: 'aar')
        implementation(name: "gamebase-adapter-auth-line-${gamebaseLineAdapterVersion}", ext: 'aar')
    }
    if (useAuthPayco) {
        implementation("com.google.android.gms:play-services-basement:${playServicesVersion}") {
            transitive = false
        }
        implementation(name: "paycologin-${paycoSdkVersion}", ext: 'aar')
        implementation(name: "gamebase-adapter-auth-payco-${gamebasePaycoAdapterVersion}", ext: 'aar')
    }

    // Compile Purchase Modules
    if (usePurchaseIAP) {
        implementation "com.toast.iap:iap:${iapSdkVersion}"
        implementation(name: "gamebase-adapter-purchase-iap-${gamebaseIAPAdapterVersion}", ext: 'aar')

        if (usePurchaseIAPOneStore) {
            implementation(name: "iap-tstore-${iapSdkVersion}", ext: 'aar')
        }
    }

    // Compile Push Modules
    if (usePushFCM) {
        implementation "com.android.support:support-v4:${supportVersion}"
        implementation "com.google.android.gms:play-services-gcm:${playServicesVersion}"
        implementation "com.google.firebase:firebase-messaging:${playServicesVersion}"
        implementation(name: "pushsdk-${pushSdkVersion}", ext: 'aar')
        implementation(name: "gamebase-adapter-push-fcm-${gamebaseFCMAdapterVersion}", ext: 'aar')
    } else if (usePushTencent) {
        compile(name: "pushsdk-tencent-${pushSdkVersion}", ext: 'aar')
        implementation(name: "gamebase-adapter-push-tencent-${gamebaseTencentAdapterVersion}", ext: 'aar')
    }
}
```

## Dependency

Gamebase SDKでは、3rd Party SDK及びDependencyのあるモジュールのバージョンに対し互換性を保障します。

| Category                         | Provider        | Modules                                  | Dependencies                             |
| -------------------------------- | --------------- | ---------------------------------------- | ---------------------------------------- |
| **Gamebase<br>(required)**       | Gamebase        | gamebase-sdk<br>-{gamebaseSdkVersion}.aar<br>gamebase-sdk-base<br>-{gamebaseSdkVersion}.aar | appcompat-v7-{supportVersion}.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar<br>support-media-compat-{supportVersion}.aar<br>support-v4-{supportVersion}.aar<br>gson-{gsonVersion}.jar<br>okhttp-{okhttpVersion}.jar<br>okio-1.11.0.jar |
| **Authentication<br>(optional)** | Google          | gamebase-adapter-auth-google<br>-{gamebaseGoogleAdapterVersion}.aar | play-services-base-{playServicesVersion}.aar<br>play-services-basement-{playServicesVersion}.aar<br>play-services-tasks-{playServicesVersion}.aar<br>play-services-auth-{playServicesVersion}.aar<br>play-services-auth-base-{playServicesVersion}.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar<br>support-media-compat-{supportVersion}.aar<br>support-v4-{supportVersion}.aar |
|                                  | Facebook        | gamebase-adapter-auth-facebook<br>-{gamebaseFacebookAdapterVersion}.aar | facebook-core-{facebookSdkVersion}.aar<br>facebook-common-{facebookSdkVersion}.aar<br>facebook-login-{facebookSdkVersion}.aar<br>appcompat-v7-{supportVersion}.aar<br>support-vector-drawable-{supportVersion}.aar<br>animated-vector-drawable-{supportVersion}.aar<br>cardview-v7-{supportVersion}.aar<br>customtabs-{supportVersion}.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar<br>support-media-compat-{supportVersion}.aar<br>support-v4-{supportVersion}.aar |
|                                  | Naver           | gamebase-adapter-auth-naver<br>-{gamebaseNaverAdapterVersion}.aar | naveridlogin-android-sdk-{naverSdkVersion}.aar<br>animated-vector-drawable-{supportVersion}.aar<br>appcompat-v7-{supportVersion}.aar<br>customtabs-{supportVersion}.aar<br>support-vector-drawable-{supportVersion}.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar<br>support-media-compat-{supportVersion}.aar<br>support-v4-{supportVersion}.aar |
|                                  | Twitter         | gamebase-adapter-auth-twitter-{gamebaseTwitterAdapterVersion}.aar | signpost-core-{signpostVersion}.jar |                          |
|                                  | Line            | gamebase-adapter-auth-line<br>-{gamebaseLineAdapterVersion}.aar | line-sdk-{lineSdkVersion}.aar<br>animated-vector-drawable-{supportVersion}.aar<br>appcompat-v7-{supportVersion}.aar<br>customtabs-{supportVersion}.aar<br>support-vector-drawable-{supportVersion}.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar |
|                                  | Payco           | gamebase-adapter-auth-payco<br>-{gamebasePaycoAdapterVersion}.aar | paycologin-{paycoSdkVersion}.aar<br>play-services-base-{playServicesVersion}.aar<br>play-services-basement-{playServicesVersion}.aar<br>gson-{gsonVersion}.jar |
| **Purchase<br>(optional)**       | IAP             | gamebase-adapter-purchase-iap<br>-{gamebaseIAPAdapterVersion}.aar | iap-{iapSdkVersion}.aar<br>mobill-core-{iapSdkVersion}.aar<br>gson-{gsonVersion}.jar<br>okhttp-1.5.4.jar<br>* okhttp-1.5.4.jarはgamebase sdkから使用するokhttp-3.xとは違います。IAP SDKから使用するライブラリーです。|
|                                  | IAP - ONE store |                                          | iap-tstore-{iapSdkVersion}.aar<br>* ONE store使用時に追加する必要があります。 |
| **Push<br>(optional)**           | FCM             | gamebase-adapter-push-fcm<br>-{gamebaseFCMAdapterVersion}.aar  | pushsdk-{pushSdkVersion}.aar<br>firebase-common-{playServicesVersion}.jar<br>firebase-iid-{playServicesVersion}.jar<br>firebase-messaging-{playServicesVersion}.aar<br>play-services-base-{playServicesVersion}.aar<br>play-services-basement-{playServicesVersion}.aar<br>play-services-gcm-{playServicesVersion}.aar<br>play-services-iid-{playServicesVersion}.aar<br>play-services-tasks-{playServicesVersion}.aar<br>common-1.0.0.jar<br>common-1.0.3.jar<br>runtime-1.0.3.aar<br>support-annotations-{supportVersion}.jar<br>support-compat-{supportVersion}.aar<br>support-core-ui-{supportVersion}.aar<br>support-core-utils-{supportVersion}.aar<br>support-fragment-{supportVersion}.aar<br>support-media-compat-{supportVersion}.aar<br>support-v4-{supportVersion}.aar |
|                                  | Tencent         | gamebase-adapter-push-tencent<br>-{gamebaseTencentAdapterVersion}.aar | pushsdk-tencent-{pushSdkVersion}.aar |

* required項目は、必須で含めるべきモジュールです。
* optional項目は、該当する機能が必要な場合に含めるべきモジュールです。
* 重複するDependencyモジュールは、一つだけ含めてください。

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth)
* [Line for developers](https://developers.line.me/en/docs/line-login/android/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/android)

## API Reference

API Referenceは、SDKの中に含まれています。

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과

