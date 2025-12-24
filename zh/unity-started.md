
## Game > Gamebase > Unity Developer's Guide > Getting Started

This guide describes the environments and initial setting of Gamebase Unity SDK.

## Environments

> [Note]
> 
> Supported Unity versions
>
> * 2022.3.10f1 ~ 6000.2.15f1

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows 7 or higher
    * macOS 10.15 or higher
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * Supports functions only partially.

## Gamebase SDK SettingTool

Can easily install the Gamebase SDK using the SettingTool.

### Specification of Setting Tool

1. Install Gamebase SDK
2. Update Gamebase SDK
3. Manage Gamebase SDK settings
4. Uninstall Gamebase SDK

### Installing SettingTool

1. Download the SettingTool.
    * [Download Gamebase Setting Tool](/Download/#game-gamebase)
2. After running the Unity project, import the GamebaseUnitySettingTool_{version}.unitypackage file.

### Using the Setting Tool

Can use the SettingTool features by selecting **Tools > Gamebase** from the top menu bar in the Unity Editor.

![unity-developers-guide-started-settingtool-3.0.0-menu](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-menu.png)

1. Setup Wizard
    * Install the Gamebase SDK step by step.
2. Update Latest Version
    * Easily update the Gamebase SDK to the latest version.
3. Customize...
    * Freely install and edit the Gamebase SDK.
4. Refresh SettingTool
    * Refresh the SettingTool data.

## Installing Gamebase SDK

Select **Tools > Gamebase > Setup Wizard** menu.

1. Sequentially select features such as platform, authentication, payment, push, etc., and proceed with the installation.
2. After selecting the features to install, click the **Install** button to proceed with the installation.

![unity-developers-guide-started-settingtool-3.0.0-setupwizard](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-setupwizard-en.png)

> [Note]
>
> * can check the **required settings** through [Required Settings](#required-settings).

## Updating the SDK to the Latest Version

Select **Tools > Gamebase > Update Latest Version** menu.

1. If an update to the latest version is available, the latest version and the **Update to latest version** button will be displayed.
2. Click the **Update to latest version** button at the bottom right to update to the latest SDK.

![unity-developers-guide-started-settingtool-3.0.0-latestupdate](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-latestupdate-en.png)

## Editing Gamebase SDK Features

Select **Tools > Gamebase > Customize...** menu.

1. Freely select the settings you want to install.
2. Click the **Apply Settings** button at the bottom right to proceed with the installation using the selected settings.

![unity-developers-guide-started-settingtool-3.0.0-customize](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-customize-en.png)

> [Note]
>
> * can check the **required settings** through [Required Settings](#required-settings).

## Required Settings

The SettingTool provides a UI where Can view and modify **required settings**. 
Can find them in both the **Setup Wizard** and **Customize** menus. 
Once all required settings are completed, the corresponding item will disappear.

> <font color="red">[Caution]</font>
>
> If the Required Settings are not resolved, **errors may occur** when running or building the project.

### Installing EDM4U

If you're using Android or iOS platforms, [EDM4U(External Dependency Manager)](https://github.com/googlesamples/unity-jar-resolver) is required.

> [Note]
>
> * If EDM4U is not installed in the project, first [download](https://github.com/googlesamples/unity-jar-resolver/raw/refs/heads/master/external-dependency-manager-latest.unitypackage) the UnityPackage file and import it to install.

### Android Publishing Settings

To configure the Gamebase Android SDK, essential files need to be generated.

1. Open Android Player Settings.
    * **Player Settings > Player > Android**
2. Generate the required files under **Publishing Settings**.
    * Create AndroidManifest.xml
        * Enable **Custom Main Manifest**
    * Create mainTemplate.gradle
        * Enable **Custom Gradle Template**
    * Create gradleTemplate.properties
        * Enable **Custom Gradle Properties Template**

### Android Activity Settings

To manage the Android Lifecycle, you need to set the Activity provided by Gamebase as the MainActivity.

The MainActivity setting depends on the Application Entry Point.

* Unity version 2022 or below
    1. No Application Entry Point setting exists. It behaves like an Activity.
    2. Set the MainActivity in AndroidManifest.xml.
        * com.toast.android.gamebase.activity.GamebaseMainActivity
* Unity version 2023 or above
    1. Open Android Player Setting
        * **Player Settings > Player > Android**
    2. Check or configure the Application Entry Point setting
        * **Other Settings > Application Entry Point**
            * ![unity-developers-guide-started-settingtool-application-entry-point](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-application-entry-point.png)
    3. Set the appropriate MainActivity based on the configured Application Entry Point
        * If Activity is enabled:
            * com.toast.android.gamebase.activity.GamebaseMainActivity
        * If GameActivity is enabled:
            * com.toast.android.gamebase.activity.GamebaseMainGameActivity

> <font color="red">[Caution]</font>
>
> * The MainActivity must be the one provided by Gamebase or inherit from it.

### AndroidManifest.xml Configuration

```xml
<manifest>
    ...
    <application>
        ...
        <!-- For versions below 2022 or if it is an Activity -->
        <!-- If android:exported is missing, it needs to be added -->
        <activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
                  android:exported="true"
            ...>
            ...
        </activity>
    
        ...
        <!-- For versions 2023 and above, if it's GameActivity -->
        <!-- If android:exported is missing, it needs to be added -->
        <activity android:name="com.toast.android.gamebase.activity.GamebaseMainGameActivity"
                  android:exported="true"
            ...>
            ...
        </activity>
    ...
    </application>
    ...
</manifest>
```

### Android EDM4U Settings

**Assets > External Dependency Manager > Android Resolver > Settings > Android Resolver Settings**

* Android Resolver Settings
    * Enable Auto-Resolution: Disabled
    * Explode AARs: Disabled
    * Patch mainTemplate.gradle: Enabled
    * Patch gradleTemplate.properties: Enabled
    * ![unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182.png)

### Android EDM4U Manual Resolve

When installing the Gamebase SDK using the Setting Tool, if EDM4U is installed, the resolve process will run automatically to configure the settings.

If EDM4U is not installed or if changes are needed, manual resolution can be performed.

**Assets > External Dependency Manager > Android Resolver > Force Resolve**

### iOS CocoaPods Installation

If you're servicing the iOS platform, CocoaPods must be installed. For installation and detailed instructions, refer to the [CocoaPods official site](https://cocoapods.org/).

CocoaPods can also be installed through EDM4U.

**Assets > External Dependency Manager > iOS Resolver > install Cocoapods**

### iOS EDM4U Settings

> **Assets > External Dependency Manager > iOS Resolver > Settings > iOS Resolver Settings**

* iOS Resolver Settings
    * Use Shell to Execute Cocoapod Tool: Disabled
        * If this feature is enabled, an error occurs where xcworkspace is not created when building iOS in Unity. (CocoaPods 1.11.x bug)
        * For users who need to enable this feature, solve the error in one of the 2 ways below.
            * Install CocoaPods version 1.10.x.
            * Call **pod install** directly from the Xcode project created in Unity.
    * Link frameworks statically: Disable
    * ![unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182.png)

#### Additional iOS Module Settings

* Depending on the selected module, May need to configure settings directly through Xcode.

1. Build the iOS project in Unity.
2. Open the generated Xcode project.
3. Add iOS SDK settings to TARGETS > UnityFramework.
    * [iOS SDK Settings Guide](./ios-started)

## In case of errors

* If an unexpected error occurs at Setting Tool, close the window and try again.
* If the error is not resolved by re-running, open the SettingToolWindow.cs file in Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts, uncomment SettingTool.SetDebugMode(true); code in the ShowWindow method, and send the log.

## API Reference

API Reference is included in GamebaseUnitySDK.

## API Supported Platforms

**API**

Platforms supporting each API are classified by the icons as below.

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

To call a Gamebase API which is not supported by a selected platform, following errors are returned as callback. If there is no callback, the output will come with a warning log.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE_WIN
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE_OSX
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

## API Deprecate Governance

The API which is not supported by Gamebase anymore is processed as deprecated (deprecate).
A deprecated API can be deleted without any prior notice when the following conditions are met:

- Minor version updates of five or more times.
  - Gamebase version format - XX.YY.ZZ
    - XX: Major
    - YY: Minor
    - ZZ: Hotfix

- Time elapse of at least five months

