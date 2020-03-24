## Game > Gamebase > Android SDK 사용 가이드 > 시작하기

## Environments

Android에서 Gamebase를 사용하기 위한 시스템 환경은 다음과 같습니다.

> [최소 사양]
>
> Android API 16 (JellyBean, 4.1) 이상
> Gradle Android Plugin 2.3.0 이상 <br/>
> 개발 환경: Android Studio

## Setting

* Gamebase Android SDK를 사용하기 전에 TOAST Console에서 앱 아이디를 발급받아야 합니다. 앱 아이디를 발급받으려면 TOAST Console 에서 **(+)서비스 선택**을 클릭하여 **Game > Gamebase** 를 클릭하여 서비스를 활성화 합니다.
* 사용할 Gamebase 버전, 사용할 인증, 결제, 푸시 모듈을 build.gradle 파일에 선언하세요.
	* Gamebase 최신 버전은 [jCenter(LINK)](https://jcenter.bintray.com/com/toast/android/gamebase/gamebase-sdk/) 에서 확인할 수 있습니다.
	* Gamebase 에서 의존하는 라이브러리 다운로드를 위해 `mavenCentral()` 저장소를 추가 해주세요.

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

* API Reference는 SDK 내에 포함되어 있습니다.

## Sample Codes

* 빌드 및 실행이 가능한 Sample Project 는 [다운로드](https://docs.toast.com/ko/Download/) 페이지에서 배포되는 Gamebase Android SDK 의 Zip 파일에 포함되어 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix
* 최소 5개월 경과

