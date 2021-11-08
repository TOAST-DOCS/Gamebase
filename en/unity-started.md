
## Game > Gamebase > Unity Developer's Guide > Getting Started

This guide describes the environments and initial setting of Gamebase Unity SDK.

### Environments

> [Note]
> 
> Supported Unity versions
>
> * 2018.4.0 - 2021.1.22
> * To support a lower version of Unity, contact [Customer Center](https://toast.com/support/inquiry).

#### Android
> <font color="red">[Caution]</font>
>
> Since August 1 of 2019, new apps on Google Play must support 64-bit architecture. 
> [See Google Play policy and Unity versions supporting 64 bits](https://developer.android.com/distribute/best-practices/develop/64-bit#unity-developers)


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
4. Click [Download SDK] to download SDK.
5. Select a platform.
    * Unity Adapter
    * Android
    * iOS
6. Select a module for each platform.
    * For Authentication, integration with an ID Provider (IdP), like Google, is supported.
    * For Push, FCM(Firebase), Tencent and APNS Push services are supported.
    * For Purchase, In-App Purchase (IAP) of NHN Cloud is provided.
7. Click [Settings] and install SDK.

#### Update SDK
1. Run Menu > Tools > Gamebase > SDKSettings > Setting Tool.
	* v1.0.1 or earlier: Menu > Gamebase > SDKSettings > Setting Tool
2. Click the [Download SDK] button to download the latest SDK.
3. Click the [Settings] button to install the SDK.
    * Previously selected modules per platform can be changed.

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


### Update of Setting Tool

If the Setting Tool needs to be updated, the Setting Tool informs whether update is available or not.
Depending on the update type, some functions provided by Setting Tool may be limited.

#### Force update

* Update required
* SDK download limited
	* You can install or uninstall using the SDK downloaded before.

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### Selective update

* Select update
* SDK download available

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

To manage lifecycle, set "com.toast.gamebase.activity.GamebaseMainActivity" as the MainActivity.
"com.toast.gamebase.activity.GamebaseMainActivity" has been inherited from "com.unity3d.player.UnityPlayerNativeActivity".

> <font color="red">[Caution]</font>
>
> AndroidPlugin should be developed in inheritance of GamebaseMainActivity.
> GamebaseMainActivity is included to GamebasePlugin.jar.
> The launchMode should be a singleTask (Unity&'s default activity will also be fixed as singleTask). Otherwise, crash may occur when an app starts.
> 
> To change the Lifecycle, you need to activate Project Settings > Settings for Android > Publish Settings > Build > Custom Main Manifest and edit the AndroidManifest.xml.

> <font color="red">[Caution]</font>
> 
> When setting Android's targetSdkVersion to 31 or higher, you must declare the android:exported attribute in the tag where the intent-filter exists.
> Even when setting **GamebaseMainActivity**, which is provided by Gamebase to manage lifecycle, as MainActivity, **android:exported="true"** must be added to the attribute.

```xml
<manifest>
	...
    <application>
    ...
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
        	android:launchMode="singleTask"
        	android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name"
            android:exported="true">
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

### iOS Settings

> <font color="red">[Caution]</font>
>
> * Caution for Unity 2019.3 or later
>     * Go to TARGETS > UnityFramework and add iOS SDK setting.
>

1. Perform iOS build in the Unity project.
2. Add the setting to the created XCode project.       
    * [iOS SDK Settings Guide](./ios-started)

## API Reference

API Reference is included in GamebaseUnitySDK.

## API Deprecate Governance

The API which is not supported by Gamebase anymore is processed as deprecated (deprecate).
A deprecated API can be deleted without any prior notice when the following conditions are met:

- Minor version updates of five or more times.
  - Gamebase version format - XX.YY.ZZ
    - XX: Major
    - YY: Minor
    - ZZ: Hotfix

- Time elapse of at least five months

