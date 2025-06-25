## Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기

Gamebase Unreal SDK 사용 환경 및 초기 설정에 대해 설명합니다.

## Environments

> [참고] 
>
> Unreal 지원 버전
>
> * UE 4.27~UE 5.4
> * 다른 버전의 지원이 필요하면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### Supported Platforms

* Android
* iOS
* Windows

선택한 플랫폼에서 지원하지 않는 Gamebase API를 호출할 때는 아래와 같은 오류가 콜백으로 반환되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GamebaseErrorCode::NOT_SUPPORTED
* GamebaseErrorCode::NOT_SUPPORTED_ANDROID
* GamebaseErrorCode::NOT_SUPPORTED_IOS
* GamebaseErrorCode::NOT_SUPPORTED_UE_STANDALONE
* GamebaseErrorCode::NOT_SUPPORTED_UE_EDITOR

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

## Installation

1. Gamebase Unreal SDK를 다운로드한 뒤 프로젝트 경로에 **Plugins** 폴더를 만들고, 다운로드한 SDK 내부 **NHNCloud** 폴더를 추가합니다.
2. Unreal 에디터에서 **Settings > Plugins** 창을 띄우고, **Project > NHN Cloud > Gamebase Plugin** 플러그인을 찾아 활성화합니다.

* [Download Gamebase Unreal SDK](/Download/#game-gamebase)

### Module Settings

* Gamebase 코드를 사용하려면 모듈의 Build.cs 파일에서 의존 모듈 설정 시 아래와 같이 모듈을 추가해야 합니다.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase"
            }
        );

### Android Settings

1. 에디터의 메뉴 **Edit > Project Settings**를 선택합니다.
2. Project Settings 창의 Plugin 카테고리에서 **Gamebase - Android**를 선택합니다.

![Unreal Project Settings - Android](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-android-setttings-2.72.0.png)

* Authentication
    * 사용하려는 IdP를 활성화합니다.
    * Hangame IdP 사용 시 고객 센터로 별도로 문의 바랍니다.
    * GPGS(Google Play Games Services)
        * Auto Login - GPGS 자동 로그인을 지원 
* Push
    * 사용하려는 푸시 서비스를 활성화합니다.
    * FCM
        * 해당 기능을 사용하는 경우 활성화됩니다.
        * GoogleServicesFilePath - [Firebase Notification Settings](./aos-started/#firebase-notification) 시 다운로드한 google-services.json 파일의 경로를 지정합니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
    * ONE Store
        * 해당 스토어를 사용하는 경우 활성화됩니다.
        * View Option - 전체 결제 화면(Full)과 팝업 결제 화면(Popup) 중 선택합니다.

#### Google Play 인증 및 결제가 되지 않는 문제

Google Play 서비스에 인증과 결제를 진행하려면 Distribution 설정이 필요합니다.
상세한 내용은 아래 문서를 참고하시기 바랍니다. 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

#### GPGS(Google Play Games Services) 설정

Sign in with Apple 사용 시 프로젝트에서 /Config/Android/AndroidEngine.ini 파일에 아래 내용을 추가하여 GPGS의 Application ID를 입력합니다.

```ini
[/Script/AndroidRuntimeSettings.AndroidRuntimeSettings]
GamesAppID=
```

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

#### Epic Games 서비스

* [로그인 인증 타입](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) 지원은 PersistentAuth, AccountPortal을 지원합니다.
    * 이전에 로그인하여 PersistentAuth 로그인을 위한 토큰이 저장되었다면 해당 토큰으로 로그인을 시도한 후 로그인을 할 수 없는 경우 AccountPortal 로그인을 시도하여 결과를 전달합니다.
* 상세 내용은 아래 내용을 참고하시어 진행 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)

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

![Unreal Project Settings - iOS](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-ios-setttings-2.72.0.png)

* Path
    * Xcode Path: Xcode의 경로를 입력합니다. (기본값: /Applications/Xcode.app)
* Authentication
    * 사용하려는 IdP를 활성화합니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
* Push
    * 사용하려는 푸시 서비스를 활성화합니다.

#### Gamebase Unreal SDK 사용을 위한 엔진 수정

Gamebase Unreal SDK 및 외부 인증 SDK에서 swift로 개발된 프레임워크를 컴파일하려면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.26/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일에서 아래 코드를 추가해야 합니다.

```cs
// need to tell where to load Framework dylibs
Result += " -rpath /usr/lib/swift";                 // 추가 코드
Result += " -rpath @executable_path/Frameworks";
```

#### Sign in with Apple

Sign in with Apple 사용 시 프로젝트에서 /Config/IOS/IOSEngine.ini 파일에 아래 내용을 추가합니다.

```ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
bEnableSignInWithAppleSupport=True
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
    * [NHN Cloud Push Notification Service Extension 생성](https://docs.toast.com/e  n/TOAST/en/toast-sdk/push-ios/#notification-service-extension)

#### iOS SDK의 Warning 메시지로 인한 Unreal 빌드 오류

iOS SDK에서 발생하는 Warning 메시지가 Unreal 빌드 시 오류로 변환되어 빌드에 실패하는 현상이 발생하면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일에서 clang 컴파일 옵션 코드를 주석 처리하십시오.

```cs
// Result += " -Wall -Werror";
```

#### PLCrashReporter

UE4에서 사용 중인 PLCrashReporter가 `arm64e` architecture를 지원하지 않아, 해당 architecture를 사용하는 디바이스에서 메모리 주솟값을 획득하지 못하는 이슈가 있습니다.

NHN Cloud Log & Crash Search에서 크래시 분석을 사용하는 게임 개발사는 다음 가이드를 참고하여 UE4 내부 PLCrashReporter를 수정해야 합니다.

1. GamebaseSDK-Unreal/Source/Gamebase/ThirdParty/IOS/plcrashreporter.zip 파일을 압축 해제합니다.
2. UE4 내부 PLCrashReporter의 a 파일과 header 파일을 압축 해제한 파일로 교체합니다.
    * Engine/Source/ThirdParty/PLCrashReporter/plcrashreporter-master-xxxxxxx

#### Epic Games 서비스

* [로그인 인증 타입](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) 지원은 PersistentAuth, AccountPortal을 지원합니다.
    * 이전에 로그인하여 PersistentAuth 로그인을 위한 토큰이 저장되었다면 해당 토큰으로 로그인을 시도한 후 로그인을 할 수 없는 경우 AccountPortal 로그인을 시도하여 결과를 전달합니다.
* 상세 내용은 아래 내용을 참고하시어 진행 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)

### Windows Settings

1. 에디터의 메뉴 **Edit > Project Settings**를 선택합니다.
2. Project Settings 창의 Plugin 카테고리에서 **Gamebase - Windows**를 선택합니다.

![Unreal Project Settings - Windows](https://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-windows-setttings-2.72.0.png)

* Authentication
    * 사용하려는 IdP를 활성화합니다.
* Purchase
    * 사용하려는 스토어를 선택합니다.
    * Epic Games Store
        * EOS 서비스 정보를 각 항목에 맞게 입력합니다.

#### WebView 플러그인 안내

* WebView 사용 콘텐츠를 사용 시 플러그인 활성화가 필요합니다.
    * Login with IdP
        * GUEST 이외의 IdP를 지원하시는 경우 웹뷰를 통해 로그인 프로세스를 진행합니다.
    * ImageNotices
    * WebView
* 별도의 엔진 수정 없이 WebView 관련 기능을 사용할 경우 Unreal 에디터에서 **Settings > Plugins** 창을 띄우고, **Project > NHN Cloud > NHNWebView** 플러그인을 찾아 활성화합니다.
* 엔진에서 제공하는 Web Browser 플러그인을 사용할 경우 UE 5.4 이상이 필요하며, 그 이하 버전에서는 CEF 버전이 낮아 정상적으로 웹 기능이 동작하지 않으므로 Web Browser 플러그인과 연관된 모듈의 업데이트가 필요합니다.
    * ThirdParty
        * CEF3
    * Runtime
        * CEF3Utils
        * WebBrowser
        * WebBrowserTexture
    * Program
        * EpicWebHelper

> [주의]
> NHNWebView 플러그인과 Web Browser 플러그인은 동시의 사용이 불가능하며, 두 플러그인이 모두 활성화되어 있는 경우 빌드 시 오류가 발생합니다.

#### Epic Games 서비스

* [로그인 인증 타입](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) 지원은 ExchangeCode, AccountPortal을 지원합니다.
    * 런처에서 게임을 실행하여 ExchangeCode를 사용할 수 있다면 해당 코드로 로그인을 시도한 후 로그인을 할 수 없는 경우 AccountPortal 로그인을 시도하여 결과를 전달합니다.
* 상세 내용은 아래 내용을 참고하시어 진행 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > 3rd-Party SDK Provider Settings > Epic Games](./unreal-started/#epic-games)

#### Steamworks 서비스

* Windows에서 Steam 인증 및 결제는 Steamworks SDK를 통해 진행됩니다.
* Gamebase에서 지원하는 Steamworks의 버전은 1.59 입니다. UE 5.3 이하를 사용하는 경우 Steamworks를 업데이트해야 합니다.
    * 엔진 가이드를 확인하여 엔진의 Steamworks 모듈을 해당 버전으로 업데이트하세요.
        * [참고: 엔진 내 Steamworks 업그레이드 가이드](https://dev.epicgames.com/documentation/en-us/unreal-engine/online-subsystem-steam?application_version=4.27)
    * Online Subsystem Steam을 사용하는 경우 최신 버전의 Online Subsystem과 Online Subsystem Steam의 최신 버전 적용 코드를 참조하여 업데이트해야 합니다.
        * [참고: Online Subsystem Steam 엔진 최신 버전 커밋](https://github.com/EpicGames/UnrealEngine/commit/f6fd8dcf34a0cc31412dd473c1309c8e507981f3#diff-cd0b8c3bbdff4546195efef417923e90acead93b3625d8d82afe82fe0939b8a6)
* 내부에서는 Engine.ini의 OnlineSubsystemSteam의 bEnabled이 활성화 된 경우 Online Subsystem Steam을 사용하는 것으로 간주합니다. 그 외의 경우 Gamebase에서 사용하는 Steamworks 지원버전을 충족하면 자동으로 Steamworks 모듈을 사용합니다.

        [OnlineSubsystemSteam]
		bEnabled=True

> [주의]
> Online Subsystem Steam 없이 Steamworks만 사용 시 Gamebase 내부에서 Steamworks를 사용한 인증 정보를 받아 오는 작업만 진행하며 Steamworks SDK 프로세스를 진행하지 않습니다.
> Steamworks SDK를 직접 적용 시 초기화, 업데이트, 종료 등 필수적인 처리에 대해서는 직접 구현해야 합니다.

## 3rd-Party Provider SDK Settings

### Epic Games

* Epic Games의 기능을 사용하기 위해서는 Epic Online Services(EOS) SDK를 사용하여 로그인되어야 합니다.
* Online Subsystem EOS 플러그인 활성화되고, Engine.ini의 OnlineSubsystemEOS 섹션 내 bEnabled이 활성화 된 경우 Online Subsystem EOS를 사용하는 것으로 간주합니다.

        [OnlineSubsystemEOS]
		bEnabled=True

* EOS SDK는 엔진 경로 내 `Engine/Source/ThirdParty/EOSSDK` 모듈을 사용합니다.
    * EOS SDK 업데이트 시 해당 모듈 내에서 필요한 플랫폼에 맞게 업데이트 바랍니다.
        * Windows : 최소 버전은 1.15.5으로 1.16.3 버전까지 확인되었습니다.
        * Android, iOS : 1.17.0-CL39599718 버전까지 확인되었습니다.
    * Online Subsystem EOS에서 [로그인 인증 타입](https://dev.epicgames.com/docs/api-ref/enums/eos-e-login-credential-type) 중 `PersistentAuth` 타입을 지원하려면 UE 4.27에서는 코드 수정이 필요합니다.
        * OnlineSubsystemEOS 모듈 내 UserManagerEOS.cpp 파일을 열어 `FUserManagerEOS::Login` 메서드 내에 AccountCredentials.Type의 스트링을 비교하는 조건문을 찾아 PersistentAuth 로그인을 위한 코드 추가가 필요합니다.

                else if (AccountCredentials.Type == TEXT("persistentauth"))
                {
                    // Use locally stored token managed by EOSSDK keyring to attempt login.
                    Credentials.Type = EOS_ELoginCredentialType::EOS_LCT_PersistentAuth;
                    Credentials.Id = nullptr;
                    Credentials.Token = nullptr;
                }

    * UE 4.27에서 Online Subsystem EOS를 사용 시 빌드 오류가 발생하므로 수정이 필요합니다.

        > EOS SDK의 핸들을 가져오기 위해 `OnlineSubsystemEOS.h` 헤더를 포함하게 되어 빌드 오류가 발생하므로 OnlineSubsystemEOS 플러그인의 Private 폴더 내 헤더 파일을 Public 폴더로 이동해 주는 과정이 필요합니다. (참고: [EOS 오류 관련 문의](https://eoshelp.epicgames.com/s/question/0D54z00007QIJjhCAH/cant-call-get-voice-chat-user-interface-from-game-instance-using-the-eos-plugin-and-eos-voice-plugins-on-unreal-engine4?language=en_US))
        > - SocketSubsystemEOS.h 
        > - EOSSettings.h
        > - EOSHelpers.h
        > - [Platform]/[Platform]EOSHelpers.h

    * Online Subsystem EOS를 사용하지 않을 시 EOSSDK 모듈을 이용해 따로 EOS 초기화를 진행하여 EOS SDK의 플랫폼 핸들을 설정해야 합니다.
        * Gamebase에서는 Epic Games 인증 및 스토어 설정에 따라 필요한 기능만 호출하며 EOS SDK의 필수 라이프 사이클은 게임에서 직접 호출해주셔야 합니다. 
        * 플랫폼 핸들 설정을 위한 모듈 추가

            PrivateDependencyModuleNames.AddRange(
                new[]
                {
                    "GamebaseSharedEOS"
                }
            );

        * 플랫폼 핸들 설정

                #include "GamebaseSharedEOS.h"

                void USample::SetEosPlatformHandle(UGameInstance* GameInstance, EOS_HPlatform PlatformHandle)
                {
                    if (const auto GamebaseSharedEOS = UGameInstance::GetSubsystem<UGamebaseSharedEOS>(GameInstance))
                    {
                        // EOS SDK 초기화 후 플랫폼 핸들을 가져와 Gamebase SDK로 전달
                        GamebaseSharedEOS->SetPlatformHandle(PlatformHandle);
                    }
                }
    
    * Android 지원 시, 로그인 응답을 받기 위해 [EOS SDK 가이드](https://dev.epicgames.com/docs/epic-online-services/platforms/android#7-how-to-receive-login-callback)를 참고하여 엔진 내 EOSSDK 모듈 내 `EOSSDK_strings.xml` 파일에 해당하는 값을 등록해주셔야 합니다.

            <?xml version="1.0" encoding="utf-8"?>
            <resources>
                <!-- EOS SDK requires the Client ID to be in lowercase. -->
                <string name="eos_login_protocol_scheme">eos.yourclientidhere</string>
            </resources>



## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix

* 최소 5개월 경과