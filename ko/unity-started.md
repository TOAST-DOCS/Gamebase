## Game > Gamebase > Unity Developer's Guide > Getting Started

## Getting Started

Gamebase Unity SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

> [INFO]
> 
> Unity 지원 버전 : 5.4 이상

#### Supported Platforms

* iOS
* Android
* Standalone
	* Windows7 이상
	* MAC OS는 지원하지 않습니다.
* WebGL
	* [LINK \[WebGL Browser Compatibility\]](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
	* 일부 기능만 지원합니다.

API를 지원하지 않는 플랫폼에서는 아래와 같은 에러가 콜백으로 리턴되며, 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

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

Gamebase SDK를 쉽게 설치할 수 있도록 Setting Tool을 제공하고 있습니다.
* [LINK \[Download Gamebase Client SDK\]](http://docs.cloud.toast.com/ko/Download/)

### Specification of Setting Tool
1. SDK 다운로드
	- 최신 버전 다운로드를 지원합니다.

2. SDK 설치
	- 다운로드 된 SDK 설치를 지원합니다.
3. SDK 삭제
	- 설치 된 SDK 삭제를 지원합니다.

4. SDK 업데이트
	- 업데이트 기능은 제공 안 하고 있습니다.
	- 삭제 후 설치로 업데이트 기능을 대신 합니다.

### Using the Setting Tool

#### SDK 설치
1. Unity 프로젝트를 오픈합니다.
2. GamebaseUnitySettingTool_{version}.unitypackage를 임포트합니다.
3. Menu > Gamebase > SDKSettings > Setting Tool을 실행합니다.
4. [Broswe] 버튼 클릭해서 SDK 다운로드 위치를 선택합니다.
	- 디폴트 경로 : projcet/Gamebase/
	- Setting Tool에서 접근 가능한 위치를 선택하셔야 합니다.
5. [Download SDK] 버튼 클릭해서 SDK를 다운로드 합니다.
6. 원하는 플랫폼을 선택합니다.
	- Unity Adapter
	- Android
	- iOS
7. 각 플랫폼별 사용할 모듈을 선택합니다.
	- Authentication은 Google 과 같은 ID Provider(이하 IDP)와의 연동을 지원합니다.
	- Push는 FCM(Firebase), Tencent Push 서비스를 지원합니다.
	- Pruchase는 TOAST 결제 상품인 IAP(In-App Purchase)를 사용하여 결제를 지원합니다.
8. [Settings] 버튼 클릭해서 SDK를 설치합니다.

#### SDK 삭제
1. Menu > Gamebase > SDKSettings > Setting Tool을 실행합니다.
2. [Remove] 버튼 클릭해서 설치된 SDK를 삭제합니다.

<br/>
> [INFO]
> Setting Tool에서 예기치 못한 에러가 발생할 경우 창을 닫고 다시 실행하시기 바랍니다. <br/>
> Unity Facebook Authentication을 사용하는 경우, Facebook Unity SDK는 별도로 다운로드 받으셔야 합니다. [LINK \[Go to Download\]](https://developers.facebook.com/docs/unity/)<br/>
> Unity Facebook Authentication에서 지원하는 Facebook Unity SDK 버전은 같이 제공되는 README 파일을 참고하시기 바랍니다. <br/>

### Video of Setting Tool Usage

![![Setting Tool Movie]()](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool_movie_1.5.0.mp4)



### Android Lifecycle

Lifecycle 관리를 위해 "com.toast.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.
"com.toast.gamebase.activity.GamebaseMainActivity"는 "com.unity3d.player.UnityPlayerActivity"를 상속받아 구현되어 있습니다.

> <font color="red">[WARNING]</font>
>
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아서 만들어야 합니다. <br/>
> GamebaseMainActivity는 GamebaseAndroidPlugin.jar에 포함되어 있습니다. <br/><br/>
> launchMode는 singleTask로 해야 합니다.(Unity 기본 Activity도 singleTask로 고정됩니다.) 그렇지 않을 경우 앱 기동중 크래쉬가 발생할 수 있습니다.

```xml
<manifest>
	...
    <application>
    ...
    	<activity android:name="com.toast.gamebase.activity.GamebaseMainActivity"
        	android:launchMode="singleTask"
        	android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name">
            <intent-filter>
            	<action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    ...
    </application>
    ...
</manifest>
```

### Android Gradle Build Settting

> [INFO]
>
> Unity 지원 버전 : 5.5.2 이상  <br/>
> Gradle build는 필수가 아닌 선택입니다. <br/>
> 가이드 내용은 Android project로 export 하는 방법이 아닙니다.<br/>
> [LINK \[Unity manual\]](https://docs.unity3d.com/Manual/android-gradle-overview.html)

#### 1. Build Settings
Build System을 Gradle(new)로 선택합니다.
![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-003_1.3.0.png)

#### 2. mainTemplate.bundle 파일 설정
Unity 설치 폴더에 있는 mainTemplate.gradle 파일을 Assets/Plugins/Android/ 폴더로 복사 후 개발 환경에 맞게 설정합니다.

> [INFO]
> 
> **compileSdkVersion, buildToolsVersion, targetSdkVersion**은 환경에 맞게 셋팅하시면 됩니다. <br/>
> **applicationId**는 package name을 설정하시면 됩니다.<br/>
> **dependencies** 목록에는 사용하시는 library 파일들을 추가하시면 됩니다. <br/>
> **multiDexEnabled**를 true로 설정하시면 됩니다.<br/>
> **storeFile**은 keystore path를 설정하시면 됩니다. <br/>
> **storePassword**는 keystore password를 설정하시면 됩니다. <br/>
> **keyAlias**는 keystore alias를 설정하시면 됩니다. <br/>
> **keyPassword**는 keystore alias password를 설정하시면 됩니다. <br/>

```groovy
// GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
buildscript {
	repositories {
		jcenter()
	}

	dependencies {
		classpath 'com.android.tools.build:gradle:2.1.0'
	}
}

allprojects {
   repositories {
      flatDir {
        dirs 'libs'
      }
   }
}

apply plugin: 'com.android.application'

dependencies {
	compile fileTree(dir: 'libs', include: ['*.jar'])
	compile(name: 'animated-vector-drawable-24.0.0', ext:'aar')
	compile files('libs/bolts-applinks-1.4.0.jar')
    compile project('gamebase-sdk')
    ....
}

android {
	compileSdkVersion 23
	buildToolsVersion '25.0.0'

	defaultConfig {
		targetSdkVersion 23
		applicationId 'package name'
		
		// Enabling multidex support.
        multiDexEnabled true
	}

	lintOptions {
		abortOnError false
	}

	aaptOptions {
		noCompress '.unity3d', '.ress', '.resource', '.obb'
	}

	signingConfigs { release {
		storeFile file('keysotre path')
		storePassword 'keystore password'
		keyAlias 'keystore alias'
		keyPassword 'keystore alias password'
	} }

	buildTypes {
		debug {
			jniDebuggable true
		}
		release {
			// Set minifyEnabled to true if you want to run ProGuard on your project
			minifyEnabled false
			proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-unity.txt'
			signingConfig signingConfigs.release
		}
	}
}
```


### IOS Settings

1. Unity 프로젝트에서 IOS 빌드를 진행합니다.
2. 생성 된 XCode 프로젝트에 Gamebase IOS SDK 파일 및 설정을 추가 합니다.

IOS SDK에 대한 설정은 아래 가이드를 참조하시기 바랍니다.

* [LINK \[IOS SDK 설정 링크\]](./ios-started)

## API Reference

API Reference 는 GamebaseUnitySDK 내에 포함되어 있습니다.