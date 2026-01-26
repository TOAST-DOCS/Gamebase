
## Game > Gamebase > Unity SDK 사용 가이드 > 시작하기

Gamebase Unity SDK 사용 환경과 초기 설정 방법을 설명합니다.

## Environments

> [참고] 
>
> Unity 지원 버전
>
> * 2022.3.10f1 ~ 6000.3.5f1

#### Dependencies

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows 7 이상
    * macOS 10.15 이상
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * 일부 기능만 지원합니다.

## Gamebase SDK SettingTool

SettingTool을 이용하여 Gamebase SDK를 간편하게 설치할 수 있습니다.

### SettingTool 스펙

1. Gamebase SDK 설치
2. Gamebase SDK 업데이트
3. Gamebase SDK 설정 관리
4. Gamebase SDK 제거

### SettingTool 설치

1. SettingTool을 다운로드합니다.
    * [Download Gamebase Setting Tool](/Download/#game-gamebase)
2. Unity 프로젝트 실행 후 GamebaseUnitySettingTool\_{version}.unitypackage 파일을 임포트 합니다.

### SettingTool 사용

Unity Editor의 상단 메뉴 바에서 **Tools > Gamebase**를 선택하여 SettingTool 기능을 사용할 수 있습니다.

![unity-developers-guide-started-settingtool-3.0.0-menu](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-menu.png)

1. Setup Wizard
    * 단계적으로 Gamebase SDK 설치를 진행합니다.
2. Update Latest Version
    * 간편하게 Gamebase SDK를 최신 버전으로 업데이트합니다.
3. Customize...
    * 자유롭게 Gamebase SDK 설치하고 편집합니다.
4. Refresh SettingTool
    * SettingTool 데이터를 갱신합니다.

## Gamebase SDK 설치

**Tools > Gamebase > Setup Wizard** 메뉴를 선택합니다.

1. 순차적으로 플랫폼, 인증, 결제, 푸시 등의 기능을 선택해 설치를 진행합니다.
2. 설치할 기능을 선택한 후, **설치하기** 버튼을 클릭하여 설치를 진행합니다.

![unity-developers-guide-started-settingtool-3.0.0-setupwizard](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-setupwizard-ko.png)

> [참고]
>
> * [Required Settings](#required-settings)를 통해 설치에 **필수적인 설정**을 확인할 수 있습니다.

## SDK 최신 버전 업데이트

**Tools > Gamebase > Update Latest Version**메뉴를 선택합니다.

1. 최신 버전으로 업데이트할 수 있는 경우 최신 버전과 **최신 버전으로 업데이트** 버튼이 표시됩니다.
2. 우측 하단의 **최신 버전으로 업데이트** 버튼을 클릭하여 최신 SDK로 업데이트합니다.

![unity-developers-guide-started-settingtool-3.0.0-latestupdate](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-latestupdate-ko.png)

## Gamebase SDK 기능 편집

**Tools > Gamebase > Customize...** 메뉴를 선택합니다.

1. 원하는 설정을 자유롭게 선택해 설치를 진행합니다.
2. 우측 하단의 **설정 적용** 버튼을 클릭하면 해당 설정으로 설치가 진행됩니다.

![unity-developers-guide-started-settingtool-3.0.0-customize](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-3.0.0-customize-ko.png)

> [참고]
>
> * [Required Settings](#required-settings)를 통해 설치에 **필수적인 설정**을 확인할 수 있습니다.

## Required Settings

SettingTool에서는 **필수적인 설정**을 확인하고 수정할 수 있는 UI를 제공합니다.
**Setup Wizard**와 **Customize** 메뉴에서 확인할 수 있으며 모든 설정이 완료되면 해당 항목은 사라집니다.

> <font color="red">[주의]</font>
>
> * Required Settings를 해결하지 않으면 실행하거나 빌드할 때 오류가 발생할 수 있습니다.

### EDM4U 설치

Android, iOS 플랫폼을 사용하는 경우 [EDM4U(External Dependency Manager)](https://github.com/googlesamples/unity-jar-resolver)가 필요합니다.

> [참고]
>
> * 프로젝트에 EDM4U가 설치되어 있지 않다면, 먼저 [다운로드](https://github.com/googlesamples/unity-jar-resolver/raw/refs/heads/master/external-dependency-manager-latest.unitypackage)한 후 UnityPackage 파일을 임포트하여 설치합니다.

### Android Publishing Settings

Gamebase의 Android SDK 설정을 위해 필수적인 파일을 생성해야 합니다.

1. Android Player Setting을 엽니다.
    * **Player Settings > Player > Android**
2. **Publishing Settings**에서 필수적인 파일을 생성합니다.
    * AndroidManifest.xml 생성
        * **Custom Main Manifest**를 활성화
    * mainTemplate.gradle 생성
        * **Custom Gradle Template**을 활성화
    * gradleTemplate.properties 생성
        * **Custom Gradle Properties Template**을 활성화

### Android Activity 설정

Android Lifecycle 관리를 위해 Gamebase에서 제공되는 Activity를 MainActivity로 설정해야 합니다.

Application Entry Point에 따라 설정하는 MainActivity가 다릅니다.

* Unity 2022 이하 버전
    1. Application Entry Point 설정이 없습니다. 값이 Activity처럼 동작합니다.
    2. AndroidManifest.xml에 MainActivity를 설정합니다.
        * com.toast.android.gamebase.activity.GamebaseMainActivity
* Unity 2023 이상 버전
    1. Android Player Setting을 엽니다.
        * **Player Settings > Player > Android**
    2. Application Entry Point 설정을 확인하거나 설정합니다.
        * **Other Settings > Application Entry Point**
            * ![unity-developers-guide-started-settingtool-application-entry-point](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-application-entry-point.png)
     3. 설정된 Application Entry Point에 따라 적절한 MainActivity로 설정합니다.
        * Activity를 활성화한 경우
            * com.toast.android.gamebase.activity.GamebaseMainActivity
        * GameActivity를 활성화한 경우
            * com.toast.android.gamebase.activity.GamebaseMainGameActivity

> <font color="red">[주의]</font>
>
> * MainActivity는 반드시 Gamebase에서 제공되는 Activity를 사용하거나 상속 받아야 합니다.

### AndroidManifest.xml 설정

```xml
<manifest>
    ...
    <application>
        ... 
        <!-- 2022 이하 또는 Activity일 경우 -->
        <!-- android:exported 없을 경우 추가 필요 -->
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
                  android:exported="true" 
                ...>
            ...
        </activity>

        ... 
        <!-- 2023 이상 GameActivity일 경우 -->
        <!-- android:exported 없을 경우 추가 필요 -->
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

### Android EDM4U 설정

**Assets > External Dependency Manager > Android Resolver > Settings > Android Resolver Settings**

* Android Resolver Settings
    * Enable Auto-Resolution: 비활성화
    * Explode AARs: 비활성화
    * Patch mainTemplate.gradle: 활성화
    * Patch gradleTemplate.properties: 활성화
    * ![unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-android-1.2.182.png)

### Android EDM4U 수동 Resolve

Setting Tool로 Gamebase SDK를 설치할 때 EDM4U가 설치되어 있을 경우 Resolve가 자동으로 실행하여 설정합니다.

EDM4U 설치가 되어있지 않거나 별도로 변경이 필요할 때 수동으로 Resolve 할 수 있습니다.

**Assets > External Dependency Manager > Android Resolver > Force Resolve**

### iOS CocoaPods 설치

iOS 플랫폼을 서비스할 경우 CocoaPods가 설치되어 있어야 하며, 설치 및 자세한 설명은 [CocoaPods 공식 사이트](https://cocoapods.org/)를 참고하시기 바랍니다.

EDM4U에서 CocoaPods 설치할 수도 있습니다.

**Assets > External Dependency Manager > iOS Resolver > install Cocoapods**

### iOS EDM4U 설정

> **Assets > External Dependency Manager > iOS Resolver > Settings > iOS Resolver Settings**

* iOS Resolver Settings
    * Use Shell to Execute Cocoapod Tool: 비활성화
        * 이 기능이 활성화되면 Unity에서 iOS 빌드를 할 때, xcworkspace가 생성되지 않는 오류가 발생합니다. (CocoaPods 1.11.x 버그)
        * 활성화해야 하는 사용자는 아래 두 가지 방법 중 하나로 오류를 해결하세요.
            * CocoaPods 1.10.x 버전을 설치합니다.
            * Unity에서 생성한 Xcode 프로젝트에서 **pod install**을 직접 호출합니다.
    * Link frameworks statically: 비활성화
  * ![unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-ios-1.2.182.png)

#### iOS 모듈별 추가 설정

* 선택한 모듈에 따라 Xcode를 통해 직접 설정해야 합니다.

1. Unity 프로젝트에서 iOS 빌드를 진행합니다.
2. 생성된 Xcode 프로젝트를 엽니다.
3. TARGETS > UnityFramework에 iOS SDK 설정을 추가합니다.
    * [iOS SDK 설정 가이드](./ios-started)

## 오류 발생 시

* Setting Tool에서 예기치 않은 오류가 발생하면 창을 닫고 다시 실행하세요.
* 다시 실행해도 오류가 해결되지 않을 경우, **Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts**에서 SettingToolWindow.cs 파일을 열고, ShowWindow 메서드에서 SettingTool.SetDebugMode(true); 코드를 주석 해제 후, 로그를 전달해 주시기 바랍니다.

## API Reference

API Reference는 GamebaseUnitySDK 내에 포함돼 있습니다.

## API Supported Platforms

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

Supported Platforms
<span style="color:#1D76DB;">■</span> UNITY\_IOS
<span style="color:#0E8A16;">■</span> UNITY\_ANDROID
<span style="color:#F9D0C4;">■</span> UNITY\_STANDALONE
<span style="color:#5319E7;">■</span> UNITY\_WEBGL
<span style="color:#B60205;">■</span> UNITY\_EDITOR

플랫폼에서 지원되지 않는 Gamebase API를 사용하면 다음과 같은 오류가 콜백으로 반환되거나 Warning 로그가 출력됩니다.

* GamebaseErrorCode.NOT\_SUPPORTED
* GamebaseErrorCode.NOT\_SUPPORTED\_ANDROID
* GamebaseErrorCode.NOT\_SUPPORTED\_IOS
* GamebaseErrorCode.NOT\_SUPPORTED\_UNITY\_STANDALONE\_WIN
* GamebaseErrorCode.NOT\_SUPPORTED\_UNITY\_STANDALONE\_OSX
* GamebaseErrorCode.NOT\_SUPPORTED\_UNITY\_WEBGL
* GamebaseErrorCode.NOT\_SUPPORTED\_UNITY\_EDITOR

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건이 충족되면 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix
* 최소 5개월 경과