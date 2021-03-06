## Game > Gamebase > Upgrade Guide

## 2.21.2

### iOS

* Gamebase iOS SDK 2.21.1 에서 bitcode 를 활성화 한 후에 **아카이브 빌드**를 하면 에러가 발생합니다.
    * bitcode 사용을 원하시는 경우엔 위 이슈가 해결된 Gamebase iOS SDK 2.21.2 를 사용하시기 바랍니다.

## 2.21.0

### Android

* Gamebase Android SDK 2.21.0 은 jCenter 에는 잘못된 빌드가 배포되어, **jcenter()** 를 **mavenCentral()** 보다 먼저 선언 했다면 모든 Gamebase API 에서 크래시가 발생할 수 있습니다.
    * 정상적으로 배포된 Gamebase Android SDK 2.21.1 을 사용하시거나 **mavenCentral()** 을 **jcenter()** 보다 먼저 선언하시기 바랍니다.
* Maven Repository
    * jCenter 가 일반 사용자를 위한 서비스를 종료하여 ( [https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) ) 더 이상 새로운 빌드는 업로드는 할 수 없게 되었습니다.(jCenter 의 접근 또한 2022 년 2월 1일에 종료됩니다.)
    * 그래서 Gamebase Android SDK 2.21.0 부터는 **jcenter()** 가 아닌 **mavenCentral()** 에서만 배포가 되므로 gradle repository 에 mavenCentral 을 추가하세요.

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
}
```

#### Line IdP

* Line IdP 를 사용하는 경우, Line SDK 업데이트로 인해 아래와 같이 Gradle 에 **JavaVersion.VERSION_1_8** 설정을 하지 않으면 빌드가 실패합니다.
* Line IdP 를 사용하는 경우, Line SDK 내부에 **android:allowBackup="false"** 로 선언되어 있어 어플리케이션 빌드시 Manifest merger 에서 fail 이 발생할 수 있습니다. 이렇게 빌드가 실패한다면 다음과 같이 application 태그에 **tools:replace="android:allowBackup"** 선언을 추가하시기 바랍니다.

```groovy
android {
    compileOptions {
        // >>> [Line IdP]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    
}
```

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

### iOS

* Gamebase iOS SDK 2.21.0 에서 bitcode 를 사용할 경우 에러가 발생합니다.
    * bitcode 사용을 원하시는 경우엔 Gamebase iOS SDK 2.21.1 을 사용하시기 바랍니다.

## 2.20.2

### iOS

#### Facebook IdP

* Gamebase iOS SDK 2.20.2 에서 Facebook SDK가 9.1.0으로 업데이트 되었습니다. 
    * Facebook SDK에 추가 설정이 필요하게 되어 info.plist에 아래의 값을 추가해주시기 바랍니다. 설정하지 않을 경우 크래쉬가 발생할 수 있습니다.
        * FacebookAutoLogAppEventsEnabled
        * FacebookAdvertiserIDCollectionEnabled
* 자세한 내용은 [Facebook iOS SDK 가이드](https://developers.facebook.com/docs/app-events/getting-started-app-events-ios) 를 참고하시기 바랍니다.

## 2.19.0

### Android

#### Weibo IdP

* Gamebase Android SDK 2.19.0 에서 Weibo IdP 로그인과 다른 IdP 로그인을 번갈아가며 호출하는 경우 크래쉬가 발생합니다.
    * Weibo IdP 를 사용한다면 이슈가 수정된 Gamebase Android SDK 2.19.1 을 사용하시기 바랍니다.

## 2.18.2

### Android

#### Removed APIs

* Gamebase Android SDK 2.6.0 에서 deprecated 되었던 아래 함수들이 제거되었습니다.
    * **GamebaseConfiguration.Builder.setFCMSenderId()**
    * **GamebaseConfiguration.Builder.setTencentAccessKey()**
    * **GamebaseConfiguration.Builder.setTencentAccessId()**

## 2.18.0

### Android

#### Purchase Google

* Gamebase Android SDK 2.18.0 에서 Google 아이템 결제를 호출하면 크래쉬가 발생합니다.
    * 이슈가 수정된 Gamebase Android SDK 2.18.1 을 사용하시기 바랍니다.

## 2.17.0

### Android

* Gamebase Android SDK 2.17.0 에서 Gamebase.ImageNotice.showImageNotices API 를 호출하면 크래쉬가 발생합니다.
    * 2.17.0 의 크래쉬 및 OS 5.0~6.0 에서 커스텀 스킴 이벤트가 동작하지 않는 이슈가 수정된 Gamebase Android SDK 2.17.4 를 사용하시기 바랍니다.

## 2.15.1

### iOS

* SDK에서 정의한 타입 **GamebaseEventCategory**를 NSString 대신에 사용할 경우, 해당 타입을 **TCGBGamebaseEventCategory**로 수정해야 합니다.

## 2.15.0

### Android

#### Purchase Google

* **gamebase-adapter-purchase-google** 을 사용한다면 Gamebase SDK 2.15.0 미만 버전에서 2.15.0 이상으로 업그레이드 하는 경우 반드시 **이전 버전의 Game Client Version 을 업데이트 필수** 로 설정해야 합니다.
    * Google Billing Client 모듈이 업데이트 되어, 여러개의 단말기에서 서로 다른 Billing Client 버전이 적용된 상태에서 아이템을 구매하는 경우 오류가 발생했을때 재처리에 문제가 생길 수 있기 때문입니다.

## 2.6.0

### Unity

#### Android Limitation

* Android Support Library 버전이 28.0.0으로 올라, Unity 5, Unity 2017.1, Unity 2017.2에서는 Android 빌드에 실패합니다.
	* Unity 2017.3 미만 버전의 에디터를 사용한다면 Unity 2017.3 이상 버전을 설치하고 'Editor/Data/PlaybackEngines/AndroidPlayer/Tools/gradle/lib' 폴더를 복사해 사용 중인 Unity Editor의 동일한 경로에 덮어씁니다. 그리고 mainTemplate.gradle 파일을 아래와 같이 수정하시기 바랍니다.

```groovy
// GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
buildscript {
	repositories {
		jcenter()
        // >>> For download Gradle Android Plugin
        maven {
            url 'https://maven.google.com'
        }
	}

	dependencies {
		//classpath 'com.android.tools.build:gradle:2.1.0'
        // >>> Update Gradle Android Plugin version
        classpath 'com.android.tools.build:gradle:2.3.0'
	}
}
```

#### Firebase Push

* Firebase Cloud Messaging 을 사용하는 경우, Firebase Console 에서 다운로드 받은 google-services.json 파일을 xml 리소스로 변환하여 프로젝트에 포함하여야 Push 가 정상동작 합니다.
    * Gamebase 2.5.0 이전 버전에서는 xml 리소스가 없어도 Push 가 동작했지만, Gamebase 2.6.0 이상부터는 xml 리소스가 반드시 필요합니다.
* 아래 가이드를 참고하여 구현하시기 바랍니다.
    * [\[Game > Gamebase > Android SDK 사용 가이드 > 푸시 > Settings > Firebase\]](./aos-push/#firebase)

#### Standalone

* Removed Japan Purchase
	* 일본 결제 서비스가 종료되었습니다.
	* GamebaseUnitySDK_IAPAdapter를 사용 중이면 아래 폴더를 직접 삭제해 주시기 바랍니다.
        * Asset/Toast/Common
        * Asset/Toast/Core
        * Asset/Toast/IAP
        * Asset/Toast/Standalone

### Android

#### Limitation

* minSdkVersion이 15(IceCreamSandwichMR1, 4.0.3)에서 16(Jelly Bean, 4.1)으로 변경되었습니다.
	* OS 4.1 미만의 단말기에서는 제대로 동작하지 않을 수 있으니 프로젝트의 minSdkVersion이 15인 경우, 16으로 변경해 주시기 바랍니다.

#### Removed APIs

* 제거된 함수는 다음과 같습니다. 대체 함수로 변경하시기 바랍니다.
    * **Gamebase.getAuthBanInfo()**가 제거되었습니다. **Gamebase.getBanInfo()**로 변경하세요.
    * **Gamebase.getLanguageCode()**가 제거되었습니다. **Gamebase.getDeviceLanguageCode()**로 변경하세요.
    * **new GamebaseConfiguration.Builder(void)**가 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.
    * **new GamebaseConfiguration.Builder.setAppId()**가 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.
    * **new GamebaseConfiguration.Builder.setAppVersion()**이 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.

#### Changed/Deprecated APIs

* Gamebase.activeApp()은 자동으로 호출되므로 더 이상 호출하지 않아도 됩니다.
* Gamebase.initialize()의 인자로 필요한 GamebaseConfiguration의 생성 방법이 변경되었습니다.
    * **new GamebaseConfiguration.Builder(String, String)** 대신 **GamebaseConfiguration.newBuilder()**를 호출하세요.
* LaunchingStatus.isPlayable()은 더 이상 호출하지 마세요.
	* [\[Game > Gamebase > Android SDK 사용 가이드 > 초기화 > Launching Information\]](./aos-initialization/#launching-information) 문서를 참고하여 Launching Status Code에 따라 게임 플레이가 가능한지 여부를 직접 결정하시기 바랍니다.
* Purchase
    * Store Code는 변경할 수 없으므로 **GamebaseConfiguration.newBuilder()**에서 Store Code를 전달해야 합니다.
        * Gamebase.Purchase.getStoreCode()/Gamebase.Purchase.setStoreCode()는 제거될 예정입니다. 더 이상 사용하지 마세요.
    * Gamebase.Purchase.requestRetryTransaction()은 이제 호출하지 않아도 됩니다.
* Push
    * Gamebase Android SDK 2.6.0 이상부터는 푸시 메시지를 발송할 때, Gamebase 콘솔의 **푸시** 탭의 메뉴를 이용해 발송해야 합니다.
        * Gamebase Android SDK 2.6.0 미만이라면 Gamebase 콘솔의 **푸시(구)** 탭에서 푸시를 발송해야 합니다.
    * GamebaseConfiguration.Builder.setFCMSenderId()는 이제 호출하지 않아도 됩니다.
    * GamebaseConfiguration.Builder.setTencentAccessKey(), GamebaseConfiguration.Builder.setTencentAccessId()를 호출하는 경우 API 호출을 제거하고 build.gradle에 다음과 같이 선언해야 합니다.

```groovy
android {
    defaultConfig {
        ...
        // >>> For Tencent Push Notification
        manifestPlaceholders = [
            XG_ACCESS_ID : "1234567890",
            XG_ACCESS_KEY : "ABCDEFGHIJKL",
        ]
    }
}
```

## 2.4.4

### Unity

* Setting Tool이 업데이트되었습니다.
    * 폴더 구조가 변경되어, 이전 버전의 Setting Tool을 완전히 삭제한 후 재설치해야 합니다.

## 2.2.2

### Unity

* GamebaseUnitySDKSettings 클래스의 **storeCodeAOS** 변수명이 **storeCodeAndroid**로 변경되었습니다.
    * **storeCodeAOS**를 참조하여 Store Code를 정의하는 코드나 Prefab이 있다면 변수 참조에 실패하므로 **storeCodeAndroid** 변수로 변경하시기 바랍니다.

## 2.2.0

### Unity

* GamebaseMainActivity의 Package Name이 변경되었습니다.
    * AndroidManifest.xml의 MainActivity 선언을 아래와 같이 변경하지 않으면 크래시가 발생합니다.
    * **com.toast.gamebase.activity.GamebaseMainActivity** -> **com.toast.android.gamebase.activity.GamebaseMainActivity**

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

## 2.1.0

### Common

#### Removed APIs

* 사용되지 않는 TransferKey 기능이 제거되었습니다.
	* Guest 계정 이전 기능이 필요하다면 Gamebase SDK 2.2.1부터 추가된 [\[Game > Gamebase > Unity SDK 사용 가이드 > 인증 > TransferAccount\]](./unity-authentication/#transferaccount) 기능을 이용하세요.