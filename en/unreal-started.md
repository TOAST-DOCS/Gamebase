## Game > Gamebase > User Guide for Unreal SDK > Getting Started 

This document describes the environment and initial setting to enable Unreal Gamebase SDK.

### Environments

> [Note] 
>
> Support Versions for Unreal 
>
> * UE 4.24
> * To get a lower-version support for Unreal, please contact [Customer Center](https://toast.com/support/inquiry).

#### Supported Platforms

* iOS
* Android
* Editor
    * Supports partial features only. 

When unsupported Gamebase API is called on a selected platform, errors like below are returned as callback; if a callback is not available, warning logs show as output.  

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_UE4_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE4_EDITOR

Platforms supported by each API can be categorized by the following icon: 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

## Installation

1. Download Unreal Gamebase SDK and create a folder named `Plugins` in the project path and add the downloaded SDK.  
2. From the Unreal editor, display the `Settings > Plugins` window, and find and enable `Project > Gamebase > Gamebase Plugin`.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Android Settings

* Plugins/Gamebase/Source/Gamebase/Gamebase_Android_UPL.xml
    * Add gradle dependency for the authentication, payment, or push module to use.  

```xml
<buildGradleAdditions>
    <insert>
        dependencies {
            // >>> Gamebase Version
            def GAMEBASE_SDK_VERSION = 'x.x.x'

            implementation "com.toast.android.gamebase:gamebase-sdk:$GAMEBASE_SDK_VERSION"
            implementation "com.toast.android.gamebase:gamebase-sdk-base:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Add Auth Adapter (Add the gamebase-adapter-auth module of each IdP for the gradle dependency.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Purchase Adapter (Add the gamebase-adapter-purchase module of the market to use for the gradle dependency.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Push Adapter (Add the gamebase-adapter-purchase module of the push to use for the gradle dependency.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"

            // Add the TOAST Crash Reporter for NDK dependency
            implementation 'com.toast.android:toast-crash-reporter-ndk:0.21.0'
        }
    </insert>
</buildGradleAdditions>
```

#### Unavailability of Authentication and Payment for Google Play 

To authenticate and pay for Google Play, distribution must be configured. 
For more details, see the document as below: 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

### iOS Settings

To enable Gamebase SDK for Unreal, download UE4 source codes. 
See below for relevant guides. 

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/en-US/GettingStarted/DownloadingUnrealEngine/index.html)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

#### Sign in with Apple

To enable Sign in with Apple, com.apple.developer.applesignin must be added to entitlement.  

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

If login to Gamebase AppleId is attempted without a key value added, error shall occur like below:   

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"

```

Since UE4(4.24.3) does not support the feature, below codes must be added above line 196 of the following file.  [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs).

```cs
Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
Text.AppendLine("\t<array>");
Text.AppendLine("\t\t<string>Default</string>");
Text.AppendLine("\t</array>");
```

#### Remote Notification

To enable Gamebase Push, go to Project Settings > Platforms > iOS and activate Enable Remote Notifications Support. (available only on Github sources)

#### Error in Unreal Builds due to Warning Messages of iOS SDK

If a warning message from iOS SDK is converted as error for Unreal build, leading into failure in the buildup, handle the clang compile option code in line 269 of the following file as footnotes: [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs).

```cs
// Result += " -Wall -Werror";
```

## API Deprecate Governance

APIs that are no longer supported by Gamebase are to be deprecated. 
Once deprecated, APIs might be deleted without previous notice if they fulfill the following conditions: 

* Updated more than 5 times for a minor version 
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* At least 5-month old 
