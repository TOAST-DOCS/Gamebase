## Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기

Gamebase Unreal SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

> [참고] 
>
> Unreal 지원 버전
>
> * UE 4.22 ~ UE 4.26
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

1. 에디터의 메뉴 **Edit > Project Settings** 를 선택합니다.
2. Project Settings 창에서 Plugin 카테고리에서 **Gamebase**를 선택합니다.

![Unreal Project Settings - Android](http://static.toastoven.net/prod_gamebase/UnrealDevelopersGuide/unreal-developers-guide-started-android-setttings-2.19.0.png)

* Android - Authentication
    * 사용하려는 IdP를 활성화 합니다.
    * Hangame IdP 사용 시 고객센터로 별도로 문의 바랍니다.
* Android - Push
    * 사용하려는 Push를 활성화 합니다.
* Android - Purchase
    * 사용할 스토어를 선택합니다.
    * ONE Store
        * View Option - 전체 결제 화면(Full)과 팝업 결제 화면(Popup) 중 선택합니다.


#### Google Play 인증 및 결제가 되지 않는 문제

Google Play 서비스에 인증과 결제를 진행하려면 Distribution 설정이 필요합니다.
상세한 내용은 아래 문서를 참고하시기 바랍니다. 

* [Signing Projects for Release](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/DistributionSigning/index.html)

### iOS Settings

Gamebase SDK for Unreal을 사용하려면 `UE4 Github 소스 코드`를 사용해야 하며, Epic games 회원 가입 후 Github 계정을 연결해야 UnrealEngine repository가 노출됩니다.
관련 가이드는 아래 문서를 참고하시기 바랍니다.

* [Downloading Unreal Engine Source Code](https://docs.unrealengine.com/en-US/GettingStarted/DownloadingUnrealEngine/index.html)
* [Getting up and running](https://github.com/EpicGames/UnrealEngine#getting-up-and-running)

>`!중요`
> 이 과정을 무시할 경우, 아래 가이드 링크가 정상 작동하지 않거나 Gamebase SDK for Unreal 사용이 불가합니다.

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

#### Remote Notification

1. Gamebase Remote Notification 기능을 사용하려면 **Project Settings > Platforms > iOS** 설정에서 **Enable Remote Notifications Support** 기능을 활성화해야 합니다. (Github 소스에서만 가능)
2. Foreground 푸시 알림을 받기 위해서는 [Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Runtime/ApplicationCore/Private/IOS/IOSAppDelegate.cpp) 파일에서 아래 코드를 제거하거나,

    ```objectivec
    - (void)userNotificationCenter:(UNUserNotificationCenter *)center
        willPresentNotification:(UNNotification *)notification
            withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
        // Received notification while app is in the foreground
        HandleReceivedNotification(notification);
        
        completionHandler(UNNotificationPresentationOptionNone);
    }
    ```

    다음과 같이 수정해야 합니다.

    ```objectivec
    // AS-IS
    completionHandler(UNNotificationPresentationOptionNone);

    // TO-BE
    completionHandler(UNNotificationPresentationOptionAlert);
    ```

#### Rich Push Notification

다음과 같은 이슈로 인해 Rich Push Notification 기능을 사용할 수 없습니다.

* Unreal은 프로젝트에 [Notification Service Extension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension?language=objc)을 추가할 수 있는 방법을 제공하지 않습니다.
    * [TOAST Push Notification Service Extension 생성](https://docs.toast.com/en/TOAST/en/toast-sdk/push-ios/#notification-service-extension)

#### iOS SDK의 Warning 메시지로 인한 Unreal 빌드 오류

iOS SDK에서 발생하는 Warning 메시지가 Unreal 빌드 시 오류로 변환되어 빌드에 실패하는 현상이 발생하면 [Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs](https://github.com/EpicGames/UnrealEngine/blob/4.24/Engine/Source/Programs/UnrealBuildTool/Platform/IOS/IOSToolChain.cs) 파일에서 clang 컴파일 옵션 코드를 주석 처리하십시오.

```cs
// Result += " -Wall -Werror";
```

#### PLCrashReporter

UE4에서 사용 중인 PLCrashReporter가 `arm64e` architecture를 지원하지 않아, 해당 architecture를 사용하는 디바이스에서 메모리 주솟값을 획득하지 못하는 이슈가 있습니다.

TOAST Log & Crash Search에서 크래시 분석을 사용하는 게임 개발사는 다음 가이드를 참고하여 UE4 내부 PLCrashReporter를 수정해야 합니다.

1. GamebaseSDK-Unreal/Source/Gamebase/ThirdParty/IOS/GamebaseSDK-iOS/externals/plcrashreporter.zip 파일을 압축 해제합니다.
2. UE4 내부 PLCrashReporter의 a 파일과 header 파일을 압축 해제한 파일로 교체합니다.
    * Engine/Source/ThirdParty/PLCrashReporter/plcrashreporter-master-xxxxxxx

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix

* 최소 5개월 경과