## Game > Gamebase > Android Developer's Guide > Getting Started

## Environments

To execute Gamebase in Android, following system environment is required.

> [Minimum Specifications]
>
> Android API 16 (JellyBean, 4.1) or higher
> Gradle Android Plugin 2.3.0 or higher <br/>
> Development Environment: Android Studio

## Setting

* Before applying Gamebase Android SDK, you need an App ID issued at the NHN Cloud Console: select a project created in the NHN Cloud Console and click **(+)Service** **Game > Gamebase**.
* Declare Gamebase version and authentication to use, and the payment and the push modules in the build.gradle file.
	* Find the latest Gamebase version at [jCenter(LINK)](https://jcenter.bintray.com/com/toast/android/gamebase/gamebase-sdk/).
	* To download a library that depends on Gamebase, add the  `mavenCentral()`  storage. 

```groovy
repositories {
    jcenter()
    mavenCentral()
    ...
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'

    // >>> Gamebase - Add Auth Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"
}
```

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [Naver for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview)
* [Line for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/android)

## API Reference

* API Reference is included in SDK.

## Sample Codes

* Sample projects available for a build and execution are included to the zip file of Gamebase Android SDK which is deployed from the  [Download](https://docs.toast.com/en/Download/) page.

## API Deprecate Governance

The API which is not supported by Gamebase anymore is processed as deprecated (deprecate).
A deprecated API can be deleted without any prior notice when the following conditions are met:

* Minor version updates of five or more times.
	* Gamebase version format - XX.YY.ZZ
		* XX: Major
		* YY: Minor
		* ZZ: Hotfix
* Time elapse of at least five months

