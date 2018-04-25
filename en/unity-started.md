## Game > Gamebase > Unity Developer's Guide > Getting Started

Below describes environments and initial setting of Gamebase Unity SDK.

### Environments

> [Note]
> 
> Unity support version: 5.5.4 or higher

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows7 or higher
* MAC OS is not supported.
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * Supports functions only partially.

To call a Gamebase API which is not supported by a selected platform, following errors are returned as callback. If there is no callback, the output will come with a warning log.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

Platforms supporting each API are classified by the icons as below.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

## Installation

Setting Tool is provided to install Gamebase SDK with more at ease.

* [Download Gamebase Client SDK](/Download/#game-gamebase)

### Specification of Setting Tool
1. Download SDK
    * Supports the latest download.
2. Install SDK
    * Supports installation of downloaded SDK.
3. Delete SDK
    * Supports deletion of installed SDK.
4. Update SDK
    * Update is not supported.
    * Instead, installation after deletion is supported.

### Using the Setting Tool

#### Install SDK
1. Open a Unity project.
2. Import GamebaseUnitySettingTool_{version}.unitypackage.
3. Execute Menu > Tools > Gamebase > SDKSettings > Setting Tool.
    * v1.0.1 or lower : Menu > Gamebase > SDKSettings > Setting Tool
4. Click [Browse] and select a location to download SDK.
    * Default path: project/Gamebase/
    * The location you choose must be accessible from Setting Tool.
5. Click [Download SDK] to download SDK.
6. Select a platform.
    * Unity Adapter
    * Android
    * iOS
7. Select a module for each platform.
    * For Authentication, integration with an ID Provider (IdP), like Google, is supported.
    * For Push, FCM (Firebase) and Tencent Push services are supported.
    * For Purchase, In-App Purchase (IAP) of TOAST is provided.
8. Click [Settings] and install SDK.

#### Delete SDK
1. Execute Menu > Tools > Gamebase > SDKSettings > Setting Tool.
    * v1.0.1 or lower : Menu > Gamebase > SDKSettings > Setting Tool
2. Click [Remove] to delete installed SDKs.

<br/>
> [Note]
> 
> If an unexpected error occurs at Setting Tool, close the window and try again. <br/>
> In case of Unity Facebook Authentication, need to download Facebook Unity SDK. [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> To check the version of Facebook Unity SDK supported by Unity Facebook Authentication, refer to the README file which is also provided. <br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>



### Android Lifecycle

To manage lifecycle, set "com.toast.gamebase.activity.GamebaseMainActivity" as the MainActivity.
"com.toast.gamebase.activity.GamebaseMainActivity" has been inherited from "com.unity3d.player.UnityPlayerNativeActivity".

> <font color="red">[Caution]</font>
>
> AndroidPlugin should be developed in inheritance of GamebaseMainActivity. <br/>
> GamebaseMainActivity is included to GamebaseAndroidPlugin.jar. <br/>
> The launchMode should be a singleTask (Unity&'s default activity will also be fixed as singleTask). Otherwise, crash may occur when an app starts.

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

> [Note]
>
> Gradle build is not required, but can be selected. <br/>
> An example of gradle build is as follows.<br/>
> The guide is not about exporting to Android project.<br/>
> [Unity manual](https://docs.unity3d.com/Manual/android-gradle-overview.html)

#### 1. Build Settings
Select Gradle (new) for a Build System.
![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-003_1.3.0.png)

#### 2. mainTemplate.gradle Setting
Copy the mainTemplate.gradle file in the Unity installation folder to the Assets/Plugins/Android/ folder and set in accordance with the development environment.

> [Note]
>
> Set **compileSdkVersion, buildToolsVersion, targetSdkVersion** to fit for environment. <br/>
> Set a package name for **applicationId**.<br/>
> Add library files to the list of **dependencies**. <br/>
> Set true for **multiDexEnabled**.<br/>
> Set a keystore path for **storeFile**. <br/>
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

1. Execute iOS build in a Unity project.
2. Add settings to a new XCode project.

For setting of iOS SDK, refer to the guide as below.

* [Link to iOS SDK Setting](./ios-started)

## API Reference

API Reference is included in GamebaseUnitySDK.
