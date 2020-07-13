## Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기

Gamebase Unreal SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

> [참고] 
>
> Unreal 지원 버전
>
> * UE 4.24
> * 하위 버전의 Unreal 지원이 필요하면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### Supported Platforms

* iOS
* Android
* Editor
    * 일부 기능만 지원합니다.

선택한 플랫폼에서 지원하지 않는 Gamebase API를 호출할 때는 아래와 같은 오류가 콜백으로 반환되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_UE4_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE4_EDITOR

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

## Installation

1. Gamebase Unreal SDK를 다운로드한 후 프로젝트 경로에 **Plugins** 폴더를 만들어 다운로드한 SDK를 추가합니다.
2. Unreal 에디터에서 **Settings > Plugins** 창을 띄우고, **Project > Gamebase > Gamebase Plugin** 플러그인을 찾아 활성화합니다.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Android Settings

* Plugins/Gamebase/Source/Gamebase/Gamebase_Android_UPL.xml
    * 사용할 인증, 결제, 푸시 모듈 gradle 의존성을 추가합니다.

```xml
<buildGradleAdditions>
    <insert>
        dependencies {
            // >>> Gamebase Version
            def GAMEBASE_SDK_VERSION = 'x.x.x'

            implementation "com.toast.android.gamebase:gamebase-sdk:$GAMEBASE_SDK_VERSION"
            implementation "com.toast.android.gamebase:gamebase-sdk-base:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Add Auth Adapter (사용하려는 IdP에 맞게 gamebase-adapter-auth 모듈을 gradle 의존성에 추가합니다.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Purchase Adapter (사용하려는 마켓의 gamebase-adapter-purchase 모듈을 gradle 의존성에 추가합니다.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"

            // >>> Gamebase - Select Push Adapter (사용하려는 Push의 gamebase-adapter-purchase 모듈을 gradle 의존성에 추가합니다.)
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
            //implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"

            // Add the TOAST Crash Reporter for NDK dependency
            implementation 'com.toast.android:toast-crash-reporter-ndk:0.21.0'
        }
    </insert>
</buildGradleAdditions>
```

#### Google Play 인증 및 결제가 되지 않는 문제

Google Play 서비스에 인증과 결제를 진행하려면 Distribution 설정이 필요합니다.
상세한 내용은 아래 문서를 참고하시기 바랍니다. 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

### iOS Settings

Gamebase SDK for Unreal을 사용하려면 UE4 소스 코드를 다운로드해 사용해야 합니다.
관련 가이드는 아래 문서를 참고하시기 바랍니다.

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/en-US/GettingStarted/DownloadingUnrealEngine/index.html)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

#### Sign in with Apple

Sign in with Apple 기능을 사용하려면 entitlement에 com.apple.developer.applesignin 키값을 추가해야 합니다.

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

키값을 추가하지 않고 Gamebsae AppleId 로그인을 진행하면 다음과 같은 오류가 발생합니다.

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"

```

UE4(4.24.3)는 해당 기능을 지원하지 않으므로 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs) 파일의 296번 라인 위에 아래 코드를 추가해야 합니다.

```cs
Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
Text.AppendLine("\t<array>");
Text.AppendLine("\t\t<string>Default</string>");
Text.AppendLine("\t</array>");
```

#### Remote Notification

Gamebase Push 기능을 사용하려면 **Project Settings > Platforms > iOS** 페이지에서 **Enable Remote Notifications Support** 기능을 활성화해야 합니다(Github 소스에서만 가능).

#### iOS SDK의 Warning 메시지로 인한 Unreal 빌드 오류

iOS SDK에서 발생하는 Warning 메시지가 Unreal 빌드 시 오류로 변환되어 빌드에 실패하는 현상이 발생하면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일의 269 라인에 있는 clang 컴파일 옵션 코드를 주석 처리하십시오.

```cs
// Result += " -Wall -Werror";
```

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과