## Game > Gamebase > Unity Developer's Guide > Getting Started

## Getting Started

Environments and initial installation for Gamebase Unity SDK are described as below .

### Environments

UnityEditor v5.4 or higher

#### 1. Standalone

* Windows 7 or higher 
* MAC OS is not supported.

#### 2. WebGL

Refer to Unity document as below.

* [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)

### Supported Unity Platforms

Gamebase Unity SDK supports the following four platforms.

* UNITY_IOS
* UNITY_ANDROID
* UNITY_STANDALONE
* UNITY_WEBGL

UNITY_EDITOR also provides functional tests in part. 

To call a Gamebase API which is not supported by a selected platform, following errors are returned as callback. If there is no callback, the output will come with a warning log. 

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

Platforms supporting each API are classified by the icons as below.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)

## Installation

You can download Gamebase Client (iOS, Android, Unity) SDK in the below link. 

* [Download Gamebase Client SDK](http://docs.cloud.toast.com/ko/Download/)

To add Gamebase Unity SDK to a game project, 

1. Open Unity project  
2. Select Assets > Import Package > Custom Package and include GamebaseUnitySDK.unitypackage to a current project 

### Android Settings

Below describes Unity setting required to build Unity Android.

* Gamebase Android SDK
* Auth Adapter
* Push
* Purchase

> <font color="red">[Caution]</font>
>
> Each module may include duplicate files and they need to be deleted. <br/>

#### 1. Gamebase Android SDK

Add the gamebase-sdk folder and aar files downloaded from Gamabase Android SDK to the Assets/Plugins/Android/libs/Gamebase folder. 

* gamebase-sdk/*
* gamebase-sdk-{version}.aar
* gamebase-sdk-base-{version}.aar

![Add Adroid SDK](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_001_1.2.0.png)

#### 2. Auth Adapter

Gamebase Android SDK supports integration with IdPs like Google. 
Auth Adapter has implemented a 3rd-Party IdP SDK interface and delivers information required for integration between Gamebase and an IdP, to Gamebase.

![Auth Adapter](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_003_1.2.0.png)

To integrate with Facebook authentication, add files of the gamebase-adapter-auth-facebook folder to the Assets/Plugins/Android/libs/Gamebase folder.

![Add Facebook Adapter](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started_002_1.2.0.png)

#### 3. Push

Gamebase Android SDK supports FCM (Firebase) and Tencent push services.  
When FCM push is required, add files in the gamebase-adapter-push-fcm folder to the Assets/Plugins/Android/libs/Gamebase folder.

#### 4. Purchase

Gamebase supports In-App Purchase (IAP) of TOAST Cloud. 
Add files of the gamebase-adapter-purchase-iap folder to the Assets/Plugins/Android/libs/Gamebase folder. 

### Android Lifecycle

To manage lifecycle, set "com.toast.gamebase.activity.GamebaseMainActivity" as the MainActivity.
"com.toast.gamebase.activity.GamebaseMainActivity" has been inherited from  "com.unity3d.player.UnityPlayerNativeActivity".

> <font color="red">[Caution]</font>
>
> AndroidPlugin should be developed in inheritance of GamebaseMainActivity. <br/>
> GamebaseMainActivity is included to GamebaseAndroidPlugin.jar. <br/><br/>
> The launchMode should be a singleTask (Unity's default activity will also be fixed as singleTask).  Otherwise, crash may occur when an app starts.

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

### Android Gradle Build Setting

> [Note]
>
> Unity Support Version: 5.5.2 or higher  <br/>
> Gradle build is not required, but can be selected. <br/>
> The guide is not about exporting to Android project. <br/>
> [Unity manual](https://docs.unity3d.com/Manual/android-gradle-overview.html)

#### 1. Build Settings
Select Gradle (new) for Build System. 
![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-003_1.3.0.png)

#### 2. MainTemplate.bundle Setting 
Copy mainTemplate.gradle in the Unity installation folder to the Assets/Plugins/Android/ folder and set in accordance with the development environment.  

> [Note]
>
> Set **compileSdkVersion, buildToolsVersion, targetSdkVersion** to fit for environment. <br/>
> Set a package name for**applicationId**.<br/>
> Add library files at use to the list of **dependencies**. <br/>
> Set true for **multiDexEnabled**.<br/>
> Set a keystore path for**storeFile**. <br/>
> Set a keystore password for **storePassword**. <br/>
> Set a keystore alias for **keyAlias**. <br/>
> Set a keystore alias password for **keyPassword**. <br/>

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


### iOS Settings

1. Execute iOS build in Unity project. 
2. Add the Gamebase iOS SDK file and setting to a new XCode project. 

For setting of iOS SDK, refer to the guide as below. 

* [Link to iOS SDK Setting](./ios-started)

## API Reference

API Reference is included in GamebaseUnitySDK. 