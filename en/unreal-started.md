## Game > Gamebase > User Guide for Unreal SDK > Getting Started 

This document describes the environment and initial setting to enable Unreal Gamebase SDK.

## Environments

> [Note] 
>
> Support Versions for Unreal 
>
> * UE 4.27~UE 5.7
> * To get support for another version of Unreal, please contact [Customer Center](https://www.nhncloud.com/kr/support/inquiry).

#### Supported Platforms

* Android
* iOS
* Windows

When unsupported Gamebase API is called on a selected platform, errors like below are returned as callback; if a callback is not available, warning logs show as output.  

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_UE_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE_EDITOR

Platforms supported by each API can be categorized by the following icon: 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

## Installation

1. Download Unreal Gamebase SDK and create a folder named `Plugins` in the project path and add  **NHN Cloud** Folder in the downloaded SDK.
2. From the Unreal editor, display the `Settings > Plugins` window, and find and enable `Project > NHN Cloud > Gamebase Plugin`.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Module Settings

* To use the Gamebase code, you need to add modules as shown below when setting up dependencies in the module's Build.cs file.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase",
            }
        );

### Android Settings

1. Select **Edit > Project Settings** from the editor menu.
2. In the Project Settings window, under Plugin category, select **Gamebase - Android**.

![Unreal Project Settings - Android](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-android-setttings-2.72.0.png)

* Authentication
    * Activate the IdP to use.
    * To use Hangame IdP, please contact our Customer Center.
    * GPGS(Google Play Games Services)
        * Auto Login - Supports GPGS auto-login
* Purchase
    * Select the store to use.
    * ONE Store
        * Select View Option - Full payment screen (Full) or Payment popup window (Popup).
* Push
    * Enable the push service to use.
    * FCM


#### An issue where Google Play authentication and payment does not complete

Processing the authentication and payment for the Google Play service requires Distribution settings.
To find out more, see the following document. 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

#### GPGS (Google Play Services) Settings

When using Sign in with Apple, enter the Application ID of GPGS by adding the following to the /Config/Android/AndroidEngine.ini file in the project.

```ini
[/Script/AndroidRuntimeSettings.AndroidRuntimeSettings]
GamesAppID=
```

#### Enable AndroidX

* From Gamebase Android SDK 2.25.0, AndroidX has been introduced. Therefore, you must add the following setting to the [UPL (Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/) file.

```xml
<gradleProperties>    
  <insert>
    android.useAndroidX=true      
    android.enableJetifier=true    
  </insert>  
</gradleProperties>
```

#### Enable multidex

* From Gamebase Unreal SDK 2.26.0, the multidex-related setting within Gamebase has been removed. Therefore, you must add the following setting to the [UPL (Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/) file.

```xml
<buildGradleAdditions>
  <insert>
  android {
    defaultConfig {
      multiDexEnabled true
    }
  }
  </insert>
</buildGradleAdditions>

<androidManifestUpdates>
    <addAttribute tag="application" name="android:name" value="androidx.multidex.MultiDexApplication"/>
</androidManifestUpdates>
```

#### Epic Games Service

* [Login Credential Type](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) supports PersistentAuth and AccountPortal.
    * If a token for PersistentAuth sign-in was saved from a previous sign-in, it tries to sign in with that token. If it can't sign in with that token, it tries to sign in to AccountPortal and passes the result.
* Refer to the details below.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)

### iOS Settings

To use the Gamebase SDK for Unreal, `UE4 Github source code` has to be used, and the Github account must be linked after joining the Epic games in order to expose the UnrealEngine repository.
See below for relevant guides.  

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/5.0/en-US/downloading-unreal-engine-source-code/)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

>`!Important`
> If you ignore this process, the following guide link does not properly work and the Gamebase SDK for Unreal will become unavailable.

#### Project Settings

1. Select the editor menu **Edit > Project Settings**.
2. In the Project Settings window, select **Gamebase - iOS** from the Plugin category.

![Unreal Project Settings - iOS](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-ios-setttings-2.72.0.png)

* Path
    * Xcode Path: Enter the path of Xcode. (default: /Applications/Xcode.app)
* Authentication
    * Enable the IdP to use.
* Purchase
    * Select the store to use.
* Push
    * Enable the push service to use.

#### Modify the Engine to Use the Gamebase Unreal SDK

To compile frameworks developed in swift from the Gamebase Unreal SDK and external authentication SDKs, you need to add the code below in the [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.26/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) file.
 
 
```cs
// need to tell where to load Framework dylibs
Result += " -rpath /usr/lib/swift";                 // Additional code
Result += " -rpath @executable_path/Frameworks";
```

#### Sign in with Apple

When using Sign in with Apple, add the following to the /Config/IOS/IOSEngine.ini file in your project


```ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
bEnableSignInWithAppleSupport=True
```

#### Remote Notification

1. To enable Gamebase Remote Notification, go to **Project Settings > Platforms > iOS** and activate **Enable Remote Notifications Support**. (available only on Github sources)
2. To receive the Foreground push notification, the code shown below must be removed from the [Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp](https://github.com/EpicGames/UnrealEngine/blob/4.26/Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp) file, or

        - (void)userNotificationCenter:(UNUserNotificationCenter *)center
            willPresentNotification:(UNNotification *)notification
                withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
        {
            // Received notification while app is in the foreground
            HandleReceivedNotification(notification);
        
            completionHandler(UNNotificationPresentationOptionNone);
        }

    modify it as follows:
        
        // AS-IS
        completionHandler(UNNotificationPresentationOptionNone);
            
        // TO-BE
        completionHandler(UNNotificationPresentationOptionAlert);

#### Rich Push Notification

Cannot use the Rich Push Notification function due to the following issues:

* Unreal does not provide any methods for adding the [Notification Service Extension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension?language=objc) to the project.
    * [Create NHN Cloud Push Notification Service Extension](https://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension)

#### Error in Unreal Builds due to Warning Messages of iOS SDK

If a warning message from iOS SDK is converted as error for Unreal build, leading into failure in the buildup, handle the clang compile option code of the following file as footnotes: [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs)

```cs
// Result += " -Wall -Werror";
```

#### PLCrashReporter

There is an issue where the device using the architecture cannot get the memory address because the PLCrashReporter used by UE4 does not support the `arm64e` architecture.

Game developers using the crash analysis of the NHN Cloud Log & Crash Search must refer to the following guide to modify the UE4 internal PLCrashReporter:

1. Unzip the GamebaseSDK-Unreal/Source/Gamebase/ThirdParty/IOS/plcrashreporter.zip file.
2. Replace the file and header file of the UE4 internal PLCrashReporter with the unzipped file.
    * Engine/Source/ThirdParty/PLCrashReporter/plcrashreporter-master-xxxxxxx

#### Epic Games Service

* [Login Credential Type](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) supports PersistentAuth and AccountPortal.
    * If a token for PersistentAuth sign-in was saved from a previous sign-in, it tries to sign in with that token. If it can't sign in with that token, it tries to sign in to AccountPortal and passes the result.
* Refer to the details below.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)


### Windows Settings

1. Select **Edit > Project Settings** from the editor menu.
2. In the Project Settings window, in the Plugin category, select **Gamebase - Windows**.

![Unreal Project Settings - Windows](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-windows-setttings-2.72.0.png)

* Authentication
    * Activate the IdP to use.
* Purchase
    * Select the store to use.
    * Epic Games Store
        * Enter the EOS service information as appropriate for each field.
    * Steamworks
        * Enter the Steamworks service information as appropriate for each field.

#### WebView Plugin

* Plugin activation is required when using content that utilizes WebView.
    * GameNotice
    * ImageNotices
    * WebView
* To use WebView-related features without modifying the engine, open the **Settings > Plugins** window in the Unreal Editor, find the **Project > NHN Cloud > NHNWebView** plugin, and activate it.
* When using the engine's built-in Web Browser plugin, certain features may not function correctly depending on the internal CEF version and the plugin's capabilities.

> [Note]
> The NHNWebView plugin and the Web Browser plugin cannot be used concurrently, and if both are enabled, an error will occur at build time.

#### Epic Games Service

* [Login Credential Type](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) supports ExchangeCode and AccountPortal.
    * If an ExchangeCode is available by launching the game in the launcher, it tries to sign in with that code. If it can't sign in with that token, it tries to sign in to AccountPortal and passes the result.
* Refer to the details below.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)

#### Steamworks Services

* Steam authentication and payment on Windows is proceeded through the Steamworks SDK.
* The version of Steamworks used by Gamebase is **1.57 or later**, so if you are using UE 5.3 or earlier, you need to update Steamworks.
    * If you are not using Online Subsystem Steam, check the Engine Guide to download the Steamworks SDK version 1.57 or later, and update the Steamworks module in your engine to that version.
        * [Note: Steamworks upgrade guide in the engine](https://dev.epicgames.com/documentation/en-us/unreal-engine?application_version=4.27)
    * If you're using Online Subsystem Steam, see the latest version of Online Subsystem and the latest version of Online Subsystem Steam enforcement code for updates.
        * [Note: The commit of the latest version of the Online Subsystem Steam engine](https://github.com/EpicGames/UnrealEngine/commit/f6fd8dcf34a0cc31412dd473c1309c8e507981f3#diff-cd0b8c3bbdff4546195efef417923e90acead93b3625d8d82afe82fe0939b8a6)
* Internally, if OnlineSubsystemSteam.bEnabled is set to true in Engine.ini, it is considered that Online Subsystem Steam is being used. In all other cases, if the Steamworks version required by Gamebase is met, the Steamworks module will be used automatically.

        [OnlineSubsystemSteam]
		bEnabled=True

> [Caution]
> When using Steamworks alone without the Online Subsystem Steam, Gamebase will only get the credentials for using Steamworks inside Gamebase and will not go through the Steamworks SDK process.
> When applying the Steamworks SDK directly, you need to implement your own processing for required processes like initialization, updates, and shutdowns.

## 3rd-Party Provider SDK Settings

### Epic Games

* To use Epic Games features, you must log in using the Epic Online Services (EOS) SDK.
* If the Online Subsystem EOS plugin is enabled and OnlineSubsystemEOS.bEnabled is set to true in Engine.ini, the system will consider that Online Subsystem EOS is being used.

        [OnlineSubsystemEOS]
		bEnabled=True

* The EOS SDK uses the module located at `Engine/Source/ThirdParty/EOSSDK` within the engine path.
    * When updating the EOS SDK, make sure to update it appropriately for each required platform within this module.
        * Windows: The minimum supported version is 1.15.5, and up to version 1.16.3 has been verified.
        * Android, iOS: Verified up to version 1.17.0-CL39599718.
    * To support `PersistentAuth` type among [Login Credential Type](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) in Online Subsystem EOS, code modification is required in UE 4.27.
        * Open the UserManagerEOS.cpp file within the OnlineSubsystemEOS module, and locate the conditional statement that compares the string value of AccountCredentials.Type inside the `FUserManagerEOS::Login` method. You need to add code to support PersistentAuth login in that section.

                else if (AccountCredentials.Type == TEXT("persistentauth"))
                {
                    // Use locally stored token managed by EOSSDK keyring to attempt login.
                    Credentials.Type = EOS_ELoginCredentialType::EOS_LCT_PersistentAuth;
                    Credentials.Id = nullptr;
                    Credentials.Token = nullptr;
                }

    * A build error occurs when using Online Subsystem EOS with Unreal Engine 4.27, so modifications are required.

        > A build error occurs because including the `OnlineSubsystemEOS.h` header to obtain the EOS SDK handle causes issues. Therefore, it is necessary to move the header file from the Private folder to the Public folder within the OnlineSubsystemEOS plugin. (Note: [Inquiry regarding EOS error](https://eoshelp.epicgames.com/s/question/0D54z00007QIJjhCAH/cant-call-get-voice-chat-user-interface-from-game-instance-using-the-eos-plugin-and-eos-voice-plugins-on-unreal-engine4?language=en_US))
        > - SocketSubsystemEOS.h 
        > - EOSSettings.h
        > - EOSHelpers.h
        > - [Platform]/[Platform]EOSHelpers.h
    * If Online Subsystem EOS is not used, you must initialize EOS using the EOSSDK module and configure the platform handle for the EOS SDK.
        * Gamebase only calls the necessary functions based on Epic Games authentication and store settings, so the required EOS SDK lifecycle must be managed directly in the game.
        * Add a module for setting platform handles

                PrivateDependencyModuleNames.AddRange(
                    new[]
                    {
                        "GamebaseSharedEOS"
                    }
                );

        * Setting platform handles

                #include "GamebaseSharedEOS.h"

                void USample::SetEosPlatformHandle(UGameInstance* GameInstance, EOS_HPlatform PlatformHandle)
                {
                    if (const auto GamebaseSharedEOS = UGameInstance::GetSubsystem<UGamebaseSharedEOS>(GameInstance))
                    {
                        // After initializing the EOS SDK, retrieve the platform handle and pass it to the Gamebase SDK
                        GamebaseSharedEOS->SetPlatformHandle(PlatformHandle);
                    }
                }

    * When supporting Android, you must register the corresponding values in the `EOSSDK_strings.xml` file within the EOSSDK module in the engine, referring to the [EOS SDK Guide](https://dev.epicgames.com/docs/epic-online-services/platforms/android#7-how-to-receive-login-callback) in order to receive login responses.


            <?xml version="1.0" encoding="utf-8"?>
            <resources>
                <!-- EOS SDK requires the Client ID to be in lowercase. -->
                <string name="eos_login_protocol_scheme">eos.yourclientidhere</string>
            </resources>



## API Deprecate Governance

APIs that are no longer supported by Gamebase are to be deprecated. 
Once deprecated, APIs might be deleted without previous notice if they fulfill the following conditions: 

* Updated more than 5 times for a minor version 
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix

* At least 5-month old 