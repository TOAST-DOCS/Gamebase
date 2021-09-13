
## Game > Gamebase > Unity SDK 사용 가이드 > 시작하기

Gamebase Unity SDK 사용 환경 및 초기 설정에 대해 설명합니다.

### Environments

> [참고] 
>
> Unity 지원 버전
>
> * 2018.4.0 ~ 2021.1.20
> * 하위 버전의 Unity 지원이 필요하면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### Android
> <font color="red">[주의]</font>
>
> 2019년 8월 1일부터 Google Play에 게시되는 신규 앱에서는 64비트 아키텍처를 지원해야 합니다.
> [Google Play 정책 및 64비트 지원 Unity 버전 확인](https://developer.android.com/distribute/best-practices/develop/64-bit#unity-developers)

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
    * 최신 버전 다운로드를 지원합니다.
2. SDK 설치
    * 다운로드 된 SDK 설치를 지원합니다.
3. SDK 삭제
    * 설치 된 SDK 삭제를 지원합니다.
4. SDK 업데이트
    * 업데이트 기능은 지원하지 않습니다.
    * 삭제 후 설치로 업데이트 기능을 대신 합니다.

### Using the Setting Tool

#### SDK 설치
1. Unity 프로젝트를 오픈합니다.
2. GamebaseUnitySettingTool_{version}.unitypackage를 임포트합니다.
3. Menu > Tools > Gamebase > SDKSettings > Setting Tool을 실행합니다.
	* v1.0.1 이하 : Menu > Gamebase > SDKSettings > Setting Tool
4. [Download SDK] 버튼 클릭해서 SDK를 다운로드 합니다.
5. 원하는 플랫폼을 선택합니다.
    * Android
    * iOS
6. 각 플랫폼별 사용할 모듈을 선택합니다.
    * Authentication은 Google 과 같은 ID Provider(이하 IDP)와의 연동을 지원합니다.
    * Push는 FCM(Firebase), APNS Push 서비스를 지원합니다.
    * Pruchase는 NHN Cloud 결제 서비스인 IAP(In-App Purchase)를 사용하여 결제를 지원합니다.
7. [Settings] 버튼 클릭해서 SDK를 설치합니다.


#### SDK 업데이트
1. Menu > Tools > Gamebase > SDKSettings > Setting Tool을 실행합니다.
	* v1.0.1 이하 : Menu > Gamebase > SDKSettings > Setting Tool
2. [Download SDK] 버튼 클릭해서 최신 SDK를 다운로드 합니다.
3. [Settings] 버튼 클릭해서 SDK를 설치합니다.
    * 기존에 선택한 플랫폼별 모듈은 변경이 가능합니다


#### SDK 삭제
1. Menu > Tools > Gamebase > SDKSettings > Setting Tool을 실행합니다.
	* v1.0.1 이하 : Menu > Gamebase > SDKSettings > Setting Tool
2. [Remove] 버튼 클릭해서 설치된 SDK를 삭제합니다.

<br/>
> [참고]
> 
> Setting Tool에서 예기치 못한 에러가 발생할 경우 창을 닫고 다시 실행하시기 바랍니다. <br/>
> Unity Facebook Authentication을 사용하는 경우, Facebook Unity SDK는 별도로 다운로드 받으셔야 합니다. [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> Unity Facebook Authentication에서 지원하는 Facebook Unity SDK 버전은 같이 제공되는 README 파일을 참고하시기 바랍니다. <br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Update of Setting Tool

Setting Tool의 업데이트가 필요한 경우 Setting Tool에서 업데이트 여부를 알려드립니다.
업데이트 종류에 따라서 Setting Tool에서 제공하는 일부 기능에 제한이 있을 수 있습니다.

#### 강제 업데이트

* 업데이트 필수
* SDK 다운로드 제한
	* 기존에 다운로드 된 SDK를 이용하여 설치, 삭제 가능

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 선택 업데이트

* 업데이트 선택
* SDK 다운로드 가능

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

Lifecycle 관리를 위해 "com.toast.android.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.
"com.toast.android.gamebase.activity.GamebaseMainActivity"는 "com.unity3d.player.UnityPlayerActivity"를 상속받아 구현되어 있습니다.

> <font color="red">[주의]</font>
>
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아 만들어야 합니다. <br/>
> GamebaseMainActivity는 GamebasePlugin.jar에 포함되어 있습니다. <br/>
> launchMode는 singleTask로 해야 합니다.(Unity 기본 Activity도 singleTask로 고정됩니다.) 그렇지 않을 경우 앱을 처음 시작할 때 크래시가 발생할 수 있습니다.
>
> 해당 Lifecycle을 변경 시에는 Project Settings > Settings for Android > Publish Settings > Build > Custom Main Manifest를 활성화 하여 해당 AndroidManifest.xml에 수정해야 합니다.

```xml
<manifest>
	...
    <application>
    ...
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
        	android:launchMode="singleTask"
        	android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name">
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
