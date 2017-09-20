## Game > Gamebase > Unity Developer's Guide > Getting Started

## Getting Started

Gamebase Unity SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

UnityEditor v5.4 이상

#### 1. Standalone

* Windows 7 이상
* MAC OS 는 지원하지 않습니다.

#### 2. WebGL

Unity Documentation 참고

* [LINK \[WebGL Browser Compatibility\]](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)

### Supported Unity Platforms

Gamebase Unity SDK 은 아래 4개의 플랫폼을 지원합니다.

* UNITY_IOS
* UNITY_ANDROID
* UNITY_STANDALONE
* UNITY_WEBGL

UNITY_EDITOR 에서도 일부 기능을 테스트 할 수 있습니다.

선택한 플랫폼에서 지원하지 않는 Gamebase API 호출 시에는 아래와 같은 에러가 콜백으로 리턴되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

API 별 지원하는 플랫폼은 아래와 같은 icon 으로 구분합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

## Installation

Gamebase Client(iOS, Android, Unity) SDK 는 아래 링크에서 다운로드 가능합니다.

* [LINK \[Download Gamebase Client SDK\]](http://docs.cloud.toast.com/ko/Download/)

Gamebase Unity SDK 를 게임 프로젝트에 추가하는 방법은 다음과 같습니다.

1. Unity 프로젝트 열기
2. Assets > Import Package > Custome Package 메뉴를 선택 후 GamebaseUnitySDK.unitypackage 를 현재 프로젝트에 포함

### Android settings

Unity Android 빌드 시 필요한 Unity 설정에 대해 설명합니다.

* Gamebase Android SDK
* Auth Adapter
* Push
* Purchase

> <font color="red">[WARNING]</font>
>
> 각 모듈 별 중복으로 포함하고 있는 파일이 있을 수 있습니다.<br/>
> 중복된 파일은 하나만 있으면 되므로 삭제하도록 합니다.

#### 1. Gamebase Android SDK

다운로드 받은 Gamabase Android SDK 에서 gamebase-sdk 폴더 및 aar 파일들을 Assets/Plugins/Android/libs/Gamebase 폴더에 추가합니다.

* gamebase-sdk/*
* gamebase-sdk-{version}.aar
* gamebase-sdk-base-{version}.aar

![Add Adroid SDK](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_001_1.2.0.png)

#### 2. Auth Adapter

Gamebase Android SDK 는 Google 과 같은 ID Provider(이하 IDP) 와의 연동을 지원합니다.
Auth Adapter 는 3rd-Party IDP SDK 인터페이스를 구현하고 있으며 Gamebase 와 IDP 연동 시 필요한 정보를 Gamebase로 전달합니다.

![Auth Adapter](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_003_1.2.0.png)

Facebook 인증 연동이 필요할 경우, gamebase-adapter-auth-facebook 폴더 내 파일들을 Assets/Plugins/Android/libs/Gamebase 폴더에 추가합니다.

![Add Facebook Adapter](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_002_1.2.0.png)

#### 3. Push

Gamebase Android SDK 는 FCM(Firebase), Tencent Push 서비스를 지원합니다.
FCM Push 서비스가 필요할 경우, gamebase-adapter-push-fcm 폴더 내 파일들을 Assets/Plugins/Android/libs/Gamebase 폴더에 추가합니다.

#### 4. Purchase

Gamebase는 TOASTCloud 결제 상품인 IAP(In-App Purchase)를 사용하여 결제를 지원합니다.
gamebase-adapter-purchase-iap 폴더 내 파일들을 Assets/Plugins/Android/libs/Gamebase 폴더에 추가합니다.

### Android Lifecycle

Lifecycle 관리를 위해 "com.toast.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.
"com.toast.gamebase.activity.GamebaseMainActivity"는 "com.unity3d.player.UnityPlayerNativeActivity"를 상속받아 구현되어 있습니다.

> <font color="red">[WARNING]</font>
>
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아 만들어야 합니다. <br/>
> GamebaseMainActivity는 GamebaseAndroidPlugin.jar에 포함되어 있습니다.

```xml
<activity android:name="com.toast.gamebase.activity.GamebaseMainActivity"
          android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
          android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

Android SDK 추가 설정은 아래 링크를 참조 하시기 바랍니다

* [LINK \[Android SDK 추가 설정 링크\]](./Android Developer`s Guide#initialization)

![Add Android SDK](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-AndroidSetting_1.2.0.png)<br>

### IOS SDK Settings

1. Unity 프로젝트에서 IOS 빌드를 진행합니다.
2. 생성 된 XCode 프로젝트에 Gamebase IOS SDK 파일 및 설정을 추가 합니다.

IOS SDK에 대한 설정은 아래 가이드를 참조하시기 바랍니다.

* [LINK \[IOS SDK 설정 링크\]](./ios-started#setting-xcode-project-to-use-gamebase)

## API Reference

API Reference 는 GamebaseUnitySDK 내에 포함되어 있습니다.