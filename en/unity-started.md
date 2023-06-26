
## Game > Gamebase > Unity Developer's Guide > Getting Started

This guide describes the environments and initial setting of Gamebase Unity SDK.

## Environments

> [Note]
> 
> Supported Unity versions
>
> * 2018.4.0 ~ 2022.2.21
> * To support a lower version of Unity, contact [Customer Center](https://toast.com/support/inquiry).

#### Android
> <font color="red">[Caution]</font>
>
> Since August 1 of 2019, new apps on Google Play must support 64-bit architecture. 
> [See Google Play policy and Unity versions supporting 64 bits](https://developer.android.com/games/optimize/64-bit?#unity-developers)

#### Dependencies

| Gamebase SDK | External SDK |
| --- | --- |
| Gamebase | NHN Cloud Unity SDK 0.28.3 |

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

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

Setting Tool is provided to install Gamebase SDK easily.

* [Download Gamebase Client SDK](/Download/#game-gamebase)

### Specification of Setting Tool

1. Download SDK
    * Supports download of the latest version of SDK.
2. Install SDK
    * Supports installation of downloaded SDK.
        * Unity: Unitypackage
        * Android: Gradle
        * iOS: CocoaPods
3. Delete SDK
    * Supports deletion of installed SDK.
4. Update SDK
    * Update is not supported.
    * Resetting after removing the SDK replaces the update feature.

### Using the Setting Tool

* Gamebase SettingTool **v2.0.0** has been newly released.
    * It is not compatible with the existing v1.5.0, so please use v2.0.0 or later after completely uninstalling it.

**AS-IS**

1. Proceed with build by including the Gamebase SDK for Android and iOS in the Unity project.
2. Gradle and CocoaPods are not supported.

**TO-BE**

1. Gradle and CocoaPods are supported.
2. EDM4U (External Dependency Manager for Unity) has been adopted as a required library.
    * After downloading EDM4U from [EDM4U Github](https://github.com/googlesamples/unity-jar-resolver), install it.
    * If there is no EDM4U, Gamebase SDK for Android and iOS settings are not available.
    * If there is EDM4U in a project, you do not need to download EDM4U.
3. If you provide a service for the Android platform, select the Top Menu > **Assets > External Dependency Manager > Android Resolver > Settings** to open the Android Resolver Settings window and set the options as follows.
    * Enable Auto-Resolution: Disable
    * Explode AARs: Disabled
    * Patch mainTemplate.gradle: Enable
    * Use Jetifier: Enabled
    * ![Android Resolver Settings](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-1_2.0.0.png)
4. If you provide a service for the iOS platform, select the Top Menu > **Assets > External Dependency Manager > iOS Resolver > Settings** to open the iOS Resolver Settings window and set the options as follows.
    * Use Shell to Execute Cocoapod Tool: Disable
        * If this feature is enabled, an error occurs where xcworkspace is not created when building iOS in Unity. (CocoaPods 1.11.x bug)
        * For users who need to enable this feature, solve the error in one of the 2 ways below.
            * Install CocoaPods version 1.10.x.
            * Call **pod install** directly from the Xcode project created in Unity.

> <font color="red">[Caution]</font>
>
> If you provide a service for the iOS platform, CocoaPods must be installed. For CocoaPods installation and detailed instructions, refer to [cocoapods.org](https://cocoapods.org/).

#### Install SDK

1. Open a Unity project.
2. Import GamebaseUnitySettingTool_{version}.unitypackage.
3. In the top menu, select **Tools > NhnCloud > Gamebase > SettingTool > Settings**.
4. In SDK Download, click the [Gamebase SDK] button to download the latest version of SDK.
5. Select a platform.
    * Android
    * iOS
6. Select a module for each platform.
    * For authentication, integration with an ID Provider (IdP), like Google, is supported.
    * For push, FCM(Firebase), Tencent and APNS Push services are supported.
    * For purchase, In-App Purchase (IAP) of NHN Cloud is provided.
7. Click [Settings] and install SDK.
8. If you selected Android and iOS modules, you need to execute EDM4U resolve.
    * Android: Select Top Menu > **Assets > External Dependency Manager > Android Resolver > Force Resolve**.
    * iOS: Select Top Menu > **Assets > External Dependency Manager > iOS Resolver > install Cocoapods**.

> <font color="red">[Caution]</font>
>
> If there is no EDM4U, Gamebase SDK for Android and iOS settings are not available.<br/>
> Before executing EDM4U resolve, click the Switch Platform button in the **Build Settings** window to switch to the platform you want to build for. If the Android platform is selected, you need to enable Custom Gradle Template in **Player Settings > Publishing Settings** to create the mainTemplate.gradle file.<br/>
> When using `Unity 2019.3 or later`, you need to enable Custom Gradle Properties Template in **Player Settings > Publishing Settings** to create a gradleTemplate.properties file.


#### Update SDK

1. In the top menu, select **Tools > NhnCloud > Gamebase > SettingTool > Settings**.
2. In SDK Download, click the [Gamebase SDK] button to download the latest version of SDK.
    * If the latest SDK is already downloaded, the button is disabled.
3. Click the [Settings] button to install the SDK.
    * Previously selected modules per platform can be changed.

#### Delete SDK

1. In the top menu, select **Tools > NhnCloud > Gamebase > SettingTool > Settings**.
2. Click [Remove] to delete installed SDKs.

> [Note]
> 
> If an unexpected error occurs at Setting Tool, close the window and try again. <br/>
> If the error is not resolved by re-running, open the SettingToolWindow.cs file in **Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts**, uncomment SettingTool.SetDebugMode(true); code in the ShowWindow method, and send the log.<br/><br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Setting Tool Update

If the Setting Tool needs to be updated, the Setting Tool informs whether update is available or not.
Depending on the update type, some functions provided by Setting Tool may be limited.

#### Force update

* Update required
* SDK download limited
	* You can install or uninstall using the SDK downloaded before.

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### Selective update

* Select update
* SDK download available

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Add Facebook Authentication

Facebook SDK for iOS and Android are included in Facebook SDK.

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-facebook_001.jpg)

When you enable Facebook authentication in Unity settings

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-facebook_002.png)

and also enable Facebook authentication in Android and iOS settings,

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-facebook_003.png)

Facebook SDKs are included as duplicate in the project, resulting in errors such as authentication failure or build failure.
To prevent such errors, SettingTool is configured to prevent the user from enabling Facebook authentication both in **Unity settings** and **Android & iOS settings**.

> `[Caution]`
> You cannot enable Facebook authentication both in Unity settings and Android & iOS settings.
> When you enable Facebook authentication in Unity settings, you need to [Download Facebook SDK for Unity](https://developers.facebook.com/docs/unity/).

Refer to the table below for SettingTool settings for each game company.

| | **SettingTool > Unity** settings > Enable Facebook authentication | **SettingTool > Android, iOS** settings > Enable Facebook authentication |
| --- | --- | --- |
| Features required for games | Gamebase Facebook Login (Android, iOS)<br>Use features such as ShareLink or FeedShare | Gamebase Facebook Login (Android, iOS) |
| Android, iOS Login API | [Gamebase Login with Credential](https://docs.toast.com/en/Game/Gamebase/en/unity-authentication/#login-with-credential) | [Gamebase Login with ID Provider](https://docs.toast.com/en/Game/Gamebase/en/unity-authentication/#login-with-idp) |
| Download Facebook SDK for Unity | O | X |

* Case 1. When only using Gamebase Facebook login on Android and iOS platforms
    * Enable Facebook authentication in **SettingTool > Android, iOS** settings.
* Case 2. When you need to use Gamebase Facebook login and Facebook's FeedShare function in your Unity project
    * Enable Facebook authentication in **SettingTool > Unity** settings.
    * Gamebase does not support functions other than Facebook authentication, so you must implement it yourself using the Facebook SDK for Unity.
* Case 3. When you enable Facebook authentication in **SettingTool > Android, iOS** settings, and Facebook SDK for Unity is included in your project
    * If you are only using Gamebase Facebook Login, please remove the Facebook SDK for Unity included in your project.
    * If you are using Facebook's FeedShare function other than Gamebase Facebook login, enable Facebook authentication in  **SettingTool > Unity**  settings.
        * In this case, if Android or iOS settings are configured, they are automatically released.

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

