
## Game > Gamebase > Unity SDK 사용 가이드 > 시작하기

Gamebase Unity SDK 사용 환경 및 초기 설정에 대해 설명합니다.

## Environments

> [참고] 
>
> Unity 지원 버전
>
> * 2018.4.0~2021.2.19
> * 하위 버전의 Unity 지원이 필요하면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### Android
> <font color="red">[주의]</font>
>
> 2019년 8월 1일부터 Google Play에 게시되는 신규 앱에서는 64비트 아키텍처를 지원해야 합니다.
> [Google Play 정책 및 64비트 지원 Unity 버전 확인](https://developer.android.com/distribute/best-practices/develop/64-bit#unity-developers)

#### Dependencies

| Gamebase SDK | External SDK |
| --- | --- |
| Gamebase | TOAST Unity SDK 0.25.3 |

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows7 이상
	* MAC OS는 지원하지 않습니다.
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * 일부 기능만 지원합니다.

선택한 플랫폼에서 지원하지 않는 Gamebase API를 호출할 때는 아래와 같은 오류가 콜백으로 반환되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

## Installation

Gamebase SDK를 쉽게 설치할 수 있도록 Setting Tool을 제공하고 있습니다.

* [Download Gamebase Unity SDK](/Download/#game-gamebase)

### Specification of Setting Tool

1. SDK 다운로드
    * 최신 버전의 SDK 다운로드를 지원합니다.
2. SDK 설치
    * 다운로드된 SDK 설치를 지원합니다.
        * Unity: Unitypackage
        * Android: Gradle
        * iOS: CocoaPods
3. SDK 삭제
    * 설치된 SDK 삭제를 지원합니다.
4. SDK 업데이트
    * 업데이트 기능은 지원하지 않습니다.
    * SDK 제거 후, 재설정으로 업데이트 기능을 대신합니다.

### Using the Setting Tool

* Gamebase SettingTool **v2.0.0**이 새로 배포되었습니다. 
    * 기존 v1.5.0과는 호환이 되지 않으니, 완전히 제거 후 v2.0.0 이상을 사용하십시오.

**AS-IS**

1. Unity 프로젝트에 Gamebase SDK for Android, iOS를 포함하여 빌드를 진행합니다.
2. Gradle, CocoaPods이 지원되지 않습니다.

**TO-BE**

1. Gradle, CocoaPods을 지원합니다.
2. EDM4U(External Dependency Manager for Unity)가 필수 라이브러리로 채택되었습니다.
    * [EDM4U Github](https://github.com/googlesamples/unity-jar-resolver)에서 EDM4U를 다운로드 후, 설치해야 합니다.
    * EDM4U가 없을 경우에는 Gamebase SDK for Android, iOS 설정이 되지 않습니다.
    * Facebook, GPGS SDK, Firebase와 같이 EDM4U를 이미 포함하고 있는 SDK를 사용할 경우에는 EDM4U를 다운로드하지 않아도 됩니다.
3. Android 플랫폼을 서비스할 경우에는 상단 메뉴 > **Assets > External Dependency Manager > Android Resolver > Settings**를 선택하여 Android Resolver Settings 창을 열고 아래와 같이 설정하십시오.
    * Enable Auto-Resolution: 비활성화
    * Explode AARs: 비활성화
    * Patch mainTemplate.gradle: 활성화
    * Use Jetifier: 활성화
    * ![Android Resolver Settings](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-1_2.0.0.png)
4. iOS 플랫폼을 서비스할 경우에는 상단 메뉴 > **Assets > External Dependency Manager > iOS Resolver > Settings**를 선택하여 iOS Resolver Settings 창을 열고 아래와 같이 설정하십시오.
    * Use Shell to Execute Cocoapod Tool: 비활성화
        * 해당 기능이 활성화되어 있으면 Unity에서 iOS 빌드 시, xcworkspace가 생성되지 않는 오류가 발생합니다. (CocoaPods 1.11.x 버그)
        * 해당 기능을 활성화해야 하는 사용자는 아래 2가지 방법 중 하나로 오류를 해결하십시오.
            * CocoaPods 1.10.x 버전을 설치합니다.
            * Unity에서 생성한 Xcode 프로젝트에서 **pod install**을 직접 호출합니다.

> <font color="red">[주의]</font>
>
> iOS 플랫폼을 서비스할 경우에는 CocoaPods가 설치되어 있어야 하며, CocoaPods 설치 및 자세한 설명은 [cocoapods.org](https://cocoapods.org/)를 참고하십시오.

#### SDK 설치

1. Unity 프로젝트를 오픈합니다.
2. GamebaseUnitySettingTool_{version}.unitypackage를 import 합니다.
3. 상단 메뉴 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**를 선택합니다.
4. SDK Download 항목에서 [Gamebase SDK] 버튼을 클릭해서 최신 SDK를 다운로드합니다.
5. 사용하려는 플랫폼을 선택합니다.
    * Android
    * iOS
6. 각 플랫폼별 사용할 모듈을 선택합니다.
    * authentication은 Google 과 같은 ID Provider(이하 IDP)와의 연동을 지원합니다.
    * push는 FCM(Firebase), APNS Push 서비스를 지원합니다.
    * pruchase는 NHN Cloud 결제 서비스인 IAP(In-App Purchase)를 사용하여 결제를 지원합니다.
7. [Settings] 버튼 클릭해서 SDK를 설치합니다.
8. Android, iOS 모듈을 선택하였다면 EDM4U의 resolve를 실행해야 합니다.
    * Android: 상단 메뉴 > **Assets > External Dependency Manager > Android Resolver > Force Resolve**를 선택합니다.
    * iOS: 상단 메뉴 > **Assets > External Dependency Manager > iOS Resolver > install Cocoapods**을 선택합니다.

> <font color="red">[주의]</font>
>
> EDM4U가 없을 경우에는 Gamebase SDK for Android, iOS 설정이 되지 않습니다.<br/>
> EDM4U의 resolve를 실행하기 전, **Build Settings** 창에서 Switch Platform 버튼을 클릭하여 빌드하려는 플랫폼으로 전환이 되어 있어야 합니다. Android 플랫폼이 선택되어 있다면 **Player Settings > Publishing Settings**에서 Custom Gradle Template을 활성화하여 mainTemplate.gradle 파일을 생성해야 합니다.<br/>
> `Unity 2019.3 이상` 사용 시, **Player Settings > Publishing Settings**에서 Custom Gradle Properties Template을 활성화하여 gradleTemplate.properties 파일을 생성해야 합니다.


#### SDK 업데이트

1. 상단 메뉴 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**를 선택합니다.
2. **SDK Download** 항목에서 [Gamebase SDK] 버튼을 클릭해서 최신 SDK를 다운로드합니다.
    * 이미 최신 SDK가 다운로드되어 있을 경우에는 해당 버튼이 비활성화됩니다.
3. [Settings] 버튼 클릭해서 SDK를 설치합니다.
    * 기존에 선택한 플랫폼별 모듈은 변경이 가능합니다


#### SDK 삭제

1. 상단 메뉴 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**를 선택합니다.
2. [Remove] 버튼 클릭해서 설치된 SDK를 삭제합니다.

> [참고]
> 
> Setting Tool에서 예기치 못한 오류가 발생할 경우, 창을 닫고 재실행하시기 바랍니다. <br/>
> 재실행하여도 오류가 해결되지 않을 경우, **Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts**에서 SettingToolWindow.cs 파일을 열고, ShowWindow 메서드에서 SettingTool.SetDebugMode(true); 코드를 주석 해제 후, 로그를 전달해 주시기 바랍니다.<br/><br/>
> Unity Facebook Authentication을 사용하는 경우, Facebook Unity SDK는 별도로 다운로드해야 합니다. [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> Unity Facebook Authentication에서 지원하는 Facebook Unity SDK 버전은 같이 제공되는 README 파일을 참고하시기 바랍니다. <br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Setting Tool Update

Setting Tool의 업데이트가 필요한 경우 Setting Tool에서 업데이트 여부를 알려드립니다.
업데이트 종류에 따라서 Setting Tool에서 제공하는 일부 기능에 제한이 있을 수 있습니다.

#### 강제 업데이트

* 업데이트 필수
* SDK 다운로드 제한
	* 기존에 다운로드 된 SDK를 이용하여 설치, 삭제 가능

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 선택 업데이트

* 업데이트 선택
* SDK 다운로드 가능

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

Lifecycle 관리를 위해 "com.toast.android.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.
"com.toast.android.gamebase.activity.GamebaseMainActivity"는 "com.unity3d.player.UnityPlayerActivity"를 상속받아 구현되어 있습니다.

> <font color="red">[주의]</font>
>
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아 만들어야 합니다.
> GamebaseMainActivity는 GamebasePlugin.jar에 포함되어 있습니다.
> launchMode는 singleTask로 해야 합니다.(Unity 기본 Activity도 singleTask로 고정됩니다.) 그렇지 않을 경우 앱을 처음 시작할 때 크래시가 발생할 수 있습니다.
>
> 해당 Lifecycle을 변경 시에는 Project Settings > Settings for Android > Publish Settings > Build > Custom Main Manifest를 활성화 하여 해당 AndroidManifest.xml에 수정해야 합니다.

> <font color="red">[주의]</font>
> 
> Android의 targetSdkVersion을 31 이상으로 설정하는 경우, intent-filter가 존재하는 태그에는 반드시 android:exported 특성을 선언해야 합니다.
> Gamebase에서 Lifecycle을 관리하기 위해 제공하는 **GamebaseMainActivity**를 MainActivity로 설정할 때에도 특성에 **android:exported="true"**가 추가되어야 합니다.

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

> <font color="red">[주의]</font>
>
> * Unity 2019.3 이상 주의 사항
>     * TARGETS > UnityFramework 에 iOS SDK 설정을 추가 합니다.
>

1. Unity 프로젝트에서 iOS 빌드를 진행합니다.
2. 생성된 XCode 프로젝트에 설정을 추가 합니다.       
    * [iOS SDK 설정 가이드](./ios-started)

## API Reference

API Reference는 GamebaseUnitySDK 내에 포함돼 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 최소 5개월 경과
