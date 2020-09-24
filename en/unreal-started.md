## Game > Gamebase > User Guide for Unreal SDK 사용 가이드 > Getting Started 시작하기

This document describes the environment and initial settings to enable Gamebase Unreal SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

> [Note] 
>
> Support Versions for Unreal 지원 버전
>
> * UE 4.24
> * To get a lower-version support for Unreal, please contact 하위 버전의 Unreal 지원이 필요하면 [Customer Center](https://toast.com/support/inquiry).

#### Supported Platforms

* iOS
* Android
* Editor
    * Supports partially only. 일부 기능만 지원합니다.

When unsupported Gamebase API is called on a selected platform, errors like below are returned as callback; if a callback is not available, warning logs show as output.  선택한 플랫폼에서 지원하지 않는 Gamebase API를 호출할 때는 아래와 같은 오류가 콜백으로 반환되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_UE4_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE4_EDITOR

Platforms supported by each API can be categorized by the following icon: API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

## Installation

1. Download Unreal Gamebase SDK and create a folder named `Plugins` in the project path and add the downloaded SDK.  폴더를 만들어 다운로드 받은 SDK를 추가합니다.
2. From the Unreal editor 에디터에서, display the `Settings > Plugins` window and find and enable  창을 띄우고, `Project > Gamebase > Gamebase Plugin` 플러그인을 찾아 활성화합니다.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Android Settings

* Plugins/Gamebase/Source/Gamebase/Gamebase_Android_UPL.xml
    * Add gradle dependency for the authentication, payment, or push module to use.  사용하려는 인증, 결제, 푸시 모듈 gradle 의존성을 추가합니다.

```xml
<buildGradleAdditions>
    <insert>
        dependencies {
            // >>> Gamebase Version
            def GAMEBASE_SDK_VERSION = 'x.x.x'

            implementation "com.toast.android.gamebase:gamebase-sdk:$GAMEBASE_SDK_VERSION"
            implementation "com.toast.android.gamebase:gamebase-sdk-base:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Add Auth Adapter (Add the gamebase-adapter-auth module of each IdP for the gradle dependency 의존성에 추가합니다.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Purchase Adapter (Add the gamebase-adapter-purchase module of the market to use for the gradle dependency.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Push Adapter (Add the 사용하려는 Push의 gamebase-adapter-purchase module of the push to use for the gradle dependency.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"

            // Add the TOAST Crash Reporter for NDK dependency
            implementation 'com.toast.android:toast-crash-reporter-ndk:0.21.0'
        }
    </insert>
</buildGradleAdditions>
```

#### Google Play 인증 및 결제 진행이 안되는 문제 Unavailability of Authentication and Payment for Google Play 

Google Play 서비스에 인증과 결제를 진행하기 위해서는 Distribution 설정이 필요합니다. To authenticate and pay for Google Play, distribution must be configured. 
상세한 내용은 아래 문서를 참고 바랍니다. For more details, see the document as below: 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

### iOS Settings

Gamebase SDK for Unreal을 사용하려면 UE4 소스 코드를 다운로드하여 사용해야 합니다. To enable Gamebase SDK for Unreal, download UE4 source codes. 
관련 가이드는 아래 문서를 참고하십시오.See below for relevant guides. 

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/en-US/GettingStarted/DownloadingUnrealEngine/index.html)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

#### Sign in with Apple

To enable Sign in with Apple,  기능을 사용하려면 entitlement에 com.apple.developer.applesignin must be added to entitlement.  키값이 추가되어야 합니다.

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

If login to Gamebase AppleId is attempted without adding the key value, error shall occur like below:   해당 키값을 추가하지 않고 Gamebsae AppleId 로그인을 진행할 경우, 아래와 같은 오류가 발생합니다.

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"

```

Since UE4(4.24.3) does not support the feature는 해당 기능을 지원하지 않으므로, below codes must be added above line 196 of the following file.  [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs) 파일의 296번 라인 위에 아래 코드를 추가해야 합니다.

```cs
Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
Text.AppendLine("\t<array>");
Text.AppendLine("\t\t<string>Default</string>");
Text.AppendLine("\t</array>");
```

#### Remote Notification

To enable Gamebase Push, go to 기능을 사용하려면 Project Settings > Platforms > iOS and activate Enable Remote Notifications Support 기능을 활성화해야 합니다. (available only on Github sources 소스에서만 가능)

#### Error in Unreal Builds due to Warning Messages of iOS SDK의 Warning 메시지로 인한 Unreal 빌드 오류

If a warning message from iOS SDK is converted as error for Unreal build, leading into failure in the buildup, handle the clang compile option code in line 269 of the following file as footnotes: 에서 발생하는 Warning 메시지가 Unreal 빌드 시 오류로 변환되어 빌드에 실패하는 현상이 발생한다면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일의 269 라인에 있는 clang 컴파일 옵션 코드를 주석 처리하십시오.

```cs
// Result += " -Wall -Werror";
```

## API Deprecate Governance

APIs that are no longer supported by Gamebase are to be deprecated. 에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Once deprecated, APIs might be deleted without previous notice if they fulfill the following conditions: Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* Updated more than 5 times for a minor version 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* At least 5-month old 최소 5개월 경과
