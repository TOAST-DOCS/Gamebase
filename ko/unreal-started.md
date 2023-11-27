## Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기

Gamebase Unreal SDK 사용 환경 및 초기 설정에 대해 설명합니다.

## Environments

> [참고] 
>
> Unreal 지원 버전
>
> * UE 4.26 ~ UE 5.0
> * 하위 버전의 Unreal 지원이 필요하면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### Supported Platforms

* iOS
* Android
* Windows

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
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

## Installation

1. Gamebase Unreal SDK를 다운로드한 후 프로젝트 경로에 **Plugins** 폴더를 만들어 다운로드한 SDK 내부 **NHNCloud** 폴더를 추가합니다.
2. Unreal 에디터에서 **Settings > Plugins** 창을 띄우고, **Project > NHN Cloud > Gamebase Plugin** 플러그인을 찾아 활성화합니다.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Module Settings

* Gamebase 코드를 사용하시려면 모듈의 Build.cs 파일에서 의존 모듈 설정시 아래와 같이 2개의 모듈을 추가해야 합니다.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase",
                "GamebaseInterface"
            }
        );

### Android Settings

1. 에디터의 메뉴 **Edit > Project Settings**를 선택합니다.
2. Project Settings 창의 Plugin 카테고리에서 **Gamebase - Android**를 선택합니다.

![Unreal Project Settings - Android](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-android-setttings-2.58.0.png)

* Authentication
    * 사용하려는 IdP를 활성화합니다.
    * Hangame IdP 사용 시 고객 센터로 별도로 문의 바랍니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
    * ONE Store
        * View Option - 전체 결제 화면(Full)과 팝업 결제 화면(Popup) 중 선택합니다.
* Push
    * 사용하려는 푸시 서비스를 활성화 합니다.


#### Google Play 인증 및 결제가 되지 않는 문제

Google Play 서비스에 인증과 결제를 진행하려면 Distribution 설정이 필요합니다.
상세한 내용은 아래 문서를 참고하시기 바랍니다. 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

#### AndroidX 적용

* Gamebase Android SDK 2.25.0 부터 AndroidX가 도입되어 [UPL(Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/) 파일에 아래 설정을 추가해야 합니다.

```xml
<gradleProperties>    
  <insert>
    android.useAndroidX=true      
    android.enableJetifier=true    
  </insert>  
</gradleProperties>
```

#### multidex 적용

* Gamebase Unreal SDK 2.26.0 부터 Gamebase 내부에서 설정하던 multidex 관련 내용이 제거되었으므로, [UPL(Unreal Plugin Language)](https://docs.unrealengine.com/4.27/en-US/SharingAndReleasing/Mobile/UnrealPluginLanguage/) 파일에 아래 설정을 추가해야 합니다.

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

### iOS Settings

Gamebase SDK for Unreal을 사용하려면 `UE4 Github 소스 코드`를 사용해야 하며, Epic games 회원 가입 후 Github 계정을 연결해야 UnrealEngine repository가 노출됩니다.
관련 가이드는 아래 문서를 참고하시기 바랍니다.

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/5.0/en-US/downloading-unreal-engine-source-code/)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

>`!중요`
> 이 과정을 무시할 경우, 아래 가이드 링크가 정상 작동하지 않거나 Gamebase SDK for Unreal 사용이 불가합니다.

#### Project Settings

1. 에디터의 메뉴 **Edit > Project Settings**를 선택합니다.
2. Project Settings 창의 Plugin 카테고리에서 **Gamebase - iOS**를 선택합니다.

![Unreal Project Settings - iOS](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-ios-setttings-2.57.0.png)

* Path
    * Xcode Path: Xcode의 경로를 입력합니다. (기본값: /Applications/Xcode.app)
* Authentication
    * 사용하려는 IdP를 활성화합니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
* Push
    * 사용하려는 푸시 서비스를 활성화합니다.

#### Sign in with Apple

Sign in with Apple 기능을 사용하려면 entitlement에 com.apple.developer.applesignin 키값을 추가해야 합니다.

* [Sign in with Apple Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin)

키값을 추가하지 않고 Gamebsae AppleId 로그인을 진행하면 다음과 같은 오류가 발생합니다.

```
Authorization failed: Error Domain=AKAuthenticationError Code=-7026 "(null)"
```

UE4(4.24.3)는 해당 기능을 지원하지 않으므로 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSExports.cs) 파일에서 아래 코드를 수정해야 합니다.

```cs
// AS-IS
if (bRemoteNotificationsSupported)
{
    Text.AppendLine("\t<key>aps-environment</key>");
    Text.AppendLine(string.Format("\t<string>{0}</string>", bForDistribution ? "production" : "development"));
}

// TO-BE
if (bRemoteNotificationsSupported)
{
    Text.AppendLine("\t<key>aps-environment</key>");
    Text.AppendLine(string.Format("\t<string>{0}</string>", bForDistribution ? "production" : "development"));
    Text.AppendLine("\t<key>com.apple.developer.applesignin</key>");
    Text.AppendLine("\t<array>");
    Text.AppendLine("\t\t<string>Default</string>");
    Text.AppendLine("\t</array>");
}
```

#### Facebook SDK

Facebook IdP를 사용하려면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일에서 아래 코드를 추가해주셔야 합니다.

```cs
// need to tell where to load Framework dylibs
Result += " -rpath /usr/lib/swift";                 // 추가 코드
Result += " -rpath @executable_path/Frameworks";
```

#### Remote Notification

1. Gamebase Remote Notification 기능을 사용하려면 **Project Settings > Platforms > iOS** 설정에서 **Enable Remote Notifications Support** 기능을 활성화해야 합니다. (Github 소스에서만 가능)
2. Foreground 푸시 알림을 받기 위해서는 [Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp](https://github.com/EpicGames/UnrealEngine/blob/4.26/Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp) 파일에서 아래 코드를 제거하거나,

        - (void)userNotificationCenter:(UNUserNotificationCenter *)center
            willPresentNotification:(UNNotification *)notification
                withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
        {
            // Received notification while app is in the foreground
            HandleReceivedNotification(notification);
        
            completionHandler(UNNotificationPresentationOptionNone);
        }

    다음과 같이 수정해야 합니다.
        
            // AS-IS
            completionHandler(UNNotificationPresentationOptionNone);
            
            // TO-BE
            completionHandler(UNNotificationPresentationOptionAlert);
    

#### Rich Push Notification

다음과 같은 이슈로 인해 Rich Push Notification 기능을 사용할 수 없습니다.

* Unreal은 프로젝트에 [Notification Service Extension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension?language=objc)을 추가할 수 있는 방법을 제공하지 않습니다.
    * [NHN Cloud Push Notification Service Extension 생성](https://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension)

#### iOS SDK의 Warning 메시지로 인한 Unreal 빌드 오류

iOS SDK에서 발생하는 Warning 메시지가 Unreal 빌드 시 오류로 변환되어 빌드에 실패하는 현상이 발생하면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일에서 clang 컴파일 옵션 코드를 주석 처리하십시오.

```cs
// Result += " -Wall -Werror";
```

#### PLCrashReporter

UE4에서 사용 중인 PLCrashReporter가 `arm64e` architecture를 지원하지 않아, 해당 architecture를 사용하는 디바이스에서 메모리 주솟값을 획득하지 못하는 이슈가 있습니다.

NHN Cloud Log & Crash Search에서 크래시 분석을 사용하는 게임 개발사는 다음 가이드를 참고하여 UE4 내부 PLCrashReporter를 수정해야 합니다.

1. GamebaseSDK-Unreal/Source/Gamebase/ThirdParty/IOS/GamebaseSDK-iOS/externals/plcrashreporter.zip 파일을 압축 해제합니다.
2. UE4 내부 PLCrashReporter의 a 파일과 header 파일을 압축 해제한 파일로 교체합니다.
    * Engine/Source/ThirdParty/PLCrashReporter/plcrashreporter-master-xxxxxxx

### Windows Settings

1. 에디터의 메뉴 **Edit > Project Settings**를 선택합니다.
2. Project Settings 창의 Plugin 카테고리에서 **Gamebase - Windows**를 선택합니다.

![Unreal Project Settings - Windows](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-windows-setttings-2.57.0.png)

* Authentication
    * 사용하려는 IdP를 활성화합니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
    * Epic Store
        * EOS 서비스 정보를 각 항목에 맞게 입력합니다.

#### Epic Store 서비스

* UE 4.27 이상 버전에서 지원하며 엔진 내부에 EOSSDK 모듈이 사용되고 있습니다.
* 에픽 스토어를 사용하기 위해서는 EOSSDK를 사용하여 로그인되어야 합니다.
* Gamebase에서 사용하는 EOS 버전은 1.15.5.0으로 엔진 경로 `Engine\Source\ThirdParty\EOSSDK\SDK`에 해당 버전을 설치하여 업그레이드가 필요합니다.
    * [참고: EOS SDK 업그레이드 가이드](https://docs.unrealengine.com/5.2/en/upgrading-the-eos-sdk-in-unreal-engine/)
* 게임 시작 시 EOS Handle 설정이 필요합니다.
    * 엔진에 포함된 Online Subsystem EOS를 사용하는 경우 아래 코드와 같이 설정이 가능합니다.

            #include "OnlineSubsystemEOS.h" 
            #include "IEOSSDKManager.h"
            #include "GamebaseStandalonePurchaseEpicAdapterModule.h"

            void UGamebasePurchaseEpicSupportTestCase::SetEosPlatformInstance()
            {
                IOnlineSubsystem* Subsystem = Online::GetSubsystem(GetWorld());

                if (const FOnlineSubsystemEOS* EosSubsystem = static_cast<FOnlineSubsystemEOS*>(Subsystem))
                {
                    EOS_HPlatform PlatformHandle = *EosSubsystem->EOSPlatformHandle;
                    FGamebaseStandalonePurchaseEpicAdapterModule::SetEosPlatformInstance(*Handle);
                }
            }

        > `OnlineSubsystemEOS.h` 헤더를 포함하면 빌드 오류가 발생하므로 OnlineSubsystemEOS 플러그인의 Private 폴더 안 Header 파일을 Public 폴더로 이동해 주는 과정이 필요합니다. (참고: [EOS 오류 관련 문의](https://eoshelp.epicgames.com/s/question/0D54z00007QIJjhCAH/cant-call-get-voice-chat-user-interface-from-game-instance-using-the-eos-plugin-and-eos-voice-plugins-on-unreal-engine4?language=en_US))
        > - SocketSubsystemEOS.h 
        > - EOSSettings.h
        > - EOSHelpers.h
        > - [Platform]/[Platform]EOSHelpers.h


## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix

* 최소 5개월 경과