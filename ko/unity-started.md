## Game > Gamebase > Developer's Guide (Unity) > Getting Started

## Getting Started

Gamebase Unity SDK 사용에 대해 설명합니다.

### Environments

> [INFO]
>
> 최소사양 : Unity5.4 이상
>

### Supported Unity Platforms

Gamebase Unity SDK는 현재 아래 플랫폼에 대해서만 지원하고 있습니다.

* UNITY_ANDROID
* UNITY_IOS
* UNITY_EDITOR (일부 기능)

추후 아래 플랫폼을 지원할 예정입니다.

* UNITY_STANDALONE
* UNITY_WEBGL

Gamebase Unity SDK 에서 지원하는 플랫폼 선택 후, 선택한 플랫폼에서 지원하지 않는 API 호출 시에는 아래와 같은 에러가 콜백으로 리턴되며 콜백이 없는 API의 경우에는 Data type의 초기값이 전달됩니다.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL

API 별 지원하는 플랫폼은 아래와 같은 icon 으로 구분합니다.<br>
<div align="left">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-webgl_1.0.0.png)
</div> 

## Installation

Gamebase Unity SDK는 유니티 패키지 형태(.unitypackage)로 배포되며 아래 링크에서 다운로드 가능합니다.

* [LINK \[Download Gamebase Unity SDK\]](http://docs.cloud.toast.com/ko/Download/)


Gamebase Unity SDK를 게임 프로젝트에 추가하는 방법은 다음과 같습니다.

1. Unity에서 이미 생성된 게임 프로젝트나 새로 생성한 프로젝트 열기.
2. Unity Menu에서 Assets > Import Package > Custome Package를 선택하여 배포된 GamebaseUnitySDK.unitypackage Import.

#### Android SDK settings

Unity Android 빌드 시 필요한 설정입니다.

다운로드 받은 Android SDK에서 아래 폴더 내 파일들 및 aar 파일들을 프로젝트의 Assets/Plugins/Android/libs 폴더에 추가합니다.

* gamebase-sdk/*
* gamebase-sdk-VERSION.aar
* gamebase-sdk-base-VERSION.aar

인증 모듈 추가

1. 다운로드 받은 SDK의 gamebase-adapter-auth-IDP_NAME 폴더 내 파일들을 프로젝트의 Assets/Plugins/Android/libs 폴더에 추가합니다.
2. google, facebook, payco 중에서 사용할 인증 모듈은 모두 추가합니다.

푸쉬 모듈 추가

1. 다운로드 받은 SDK의 gamebase-adapter-push-NAME 폴더 내 파일들을 프로젝트의 Assets/Plugins/Android/libs 폴더에 추가합니다.
2. fcm(Firebase), tencent 중에서 사용할 푸쉬 모듈 하나만 추가합니다.

결제 모듈 추가

1. 다운로드 받은 SDK의 gamebase-adapter-purchase-iap 폴더 내 파일들을 프로젝트의 Assets/Plugins/Android/libs 폴더에 추가합니다.


> [WARNING]
>
> 각 모듈에서 중복으로 포함하고 있는 파일들이 있을 수 있습니다. <br/>
> 중복된 파일은 하나만 가지고 있으면 됩니다.
>

### AndroidManifest.xml
Lifecycle 관리를 위해 "com.toast.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.


> [WARNING]
>
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아 만들어야 합니다. <br/>
> GamebaseMainActivity는 GamebaseAndroidPlugin.jar에 포함되어 있습니다.
>

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

![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-AndroidSetting_1.0.0.png)<br>
**그림. Android SDK 추가하기**

#### IOS SDK settings

1. Unity에서 IOS 빌드를 통해 XCode 프로젝트를 생성 합니다.
2. XCode에서 IOS SDK 설정을 추가 합니다.

IOS SDK에 대한 설정은 아래 가이드를 참조하시기 바랍니다.

* [LINK \[IOS SDK 설정 링크\]](./iOS Developer`s Guide#setting-xcode-project-to-use-gamebase)



## API Reference
SDK 내에 포함되어 있습니다.