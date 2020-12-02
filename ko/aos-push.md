## Game > Gamebase > Android SDK 사용 가이드 > 푸시

### Settings

여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

#### Register TOAST Cloud Console

[Notification > Push > Console Guide](/Notification/Push/ko/console-guide/)를 참고해 콘솔을 설정합니다.

#### Gradle

* build.gradle에 사용할 모듈을 추가합니다.

```groovy
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    
    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'
    
    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
}
```

* 푸시 모듈은 FCM(Firebase Cloud Messaging)과 Tencent 중에서 하나만 추가해야 하지만, 테스트 목적으로 모듈을 여러 개 추가했다면 Gamebase.initialize에서 사용할 PushType을 선택할 수 있습니다.
    * PushProvider.Type.FCM : "FCM"
    * PushProvider.Type.TENCENT : "TENCENT"

```java
String PUSH_TYPE = PushProvider.Type.FCM;	// Firebase Cloud Messaging

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
		.setPushType(PUSH_TYPE)
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### Firebase

* Gradle 빌드를 사용하는 경우
    * Firebase 푸시를 사용하려면 아래 가이드에 따라 Firebase 설정을 완료해야 합니다.
		* [TOAST > TOAST SDK 사용 가이드 > TOAST Push > Android > Firebase Cloud Messaging 설정](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* Unity 빌드인 경우
    * 만일 Firebase Unity SDK Package 를 설치했다면 아래 명령어로 **generate_xml_from_google_services_json.exe** 파일을 실행하여 json 파일을 xml 파일로 변환시킬 수 있습니다.
        ```
        "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
        ```
    * Firebase Unity SDK Package 를 설치하지 않았다면, 'Firebase Console > 프로젝트 설정' 에서 google-services.json 파일을 다운로드 하여 아래 가이드에 따라 string resource(xml) 파일을 직접 만들어서 'Assets/Plugins/Android/res/values/' 폴더에 포함시켜야 합니다.
        Firebase 서비스 연동에 따라서 google-services.json 파일의 내용은 달라질 수 있습니다.
        ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 다음은 직접 제작한 string resource(xml) 파일의 예시입니다.

```xml
<!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="firebase_database_url" translatable="false">https://gamebase-sample-00000000.firebaseio.com</string>
    <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
    <string name="google_storage_bucket" translatable="false">gamebase-sample-00000000.appspot.com</string>
    <string name="project_id" translatable="false">gamebase-sample-00000000</string>
    <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
    <string name="google_app_id" translatable="false">1:000000000000:android:749cbe01c8ada279</string>
    <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
</resources>
```

### Register Push

다음 API를 호출하여, TOAST Push에 해당 사용자를 등록합니다.<br/>
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

**API**

```java
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseCallback callback);
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseNotificationOptions notificationOptions,
                                   @NonNull GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = PushConfiguration.newBuilder()
        .enablePush(enablePush)
        .enableAdAgreement(enableAdPush)
        .enableAdAgreementNight(enableAdNightPush)
        .build();

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

### Notification Options

* 단말기에 표시하는 알림을 어떤 형태로 표시할 것인지 Notification Options 를 통해 변경할 수 있습니다.
* Notification Options 는 AndroidManifest.xml 에 설정하거나 런타임에 registerPush API 를 호출하여 변경할 수 있습니다.

#### Set Notification Options with AndroidManifest.xml

알림 옵션은 AndroidManifest.xml 에 정의하여 설정할 수 있습니다.<br/>
설정 가능한 값은 아래와 같습니다.

| meta-data key | value type | description |
| ------------- | ---------- | ----------- |
| com.toast.sdk.push.notification.default_priority | int | 우선 순위.<br/>아래 5가지 값을 설정할 수 있습니다.<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 배경색. |
| com.toast.sdk.push.notification.default_light_color | int | LED 색. |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED 불이 들어올 때의 시간. |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED 불이 나갈 때의 시간. |
| com.toast.sdk.push.notification.default_small_icon | resource id | 작은 아이콘의 리소스 식별자. |
| com.toast.sdk.push.notification.default_sound | String | 알림음 파일 이름.<br/>안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다. |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 진동의 패턴. |
| com.toast.sdk.push.notification.badge_enabled | boolean | 배지 아이콘 사용 여부. |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 포그라운드 알림 사용 여부. |

**Example**

```xml
<!-- Notification priority -->
<meta-data android:name="com.toast.sdk.push.notification.default_priority"
           android:value="1"/>
<!-- Notification background color -->
<meta-data android:name="com.toast.sdk.push.notification.default_background_color"
           android:resource="@color/defaultNotificationColor"/>
<!-- LED light -->
<meta-data android:name="com.toast.sdk.push.notification.default_light_color"
           android:value="#0000ff"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_on_ms"
           android:value="0"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_off_ms"
           android:value="500"/>
<!-- Small icon -->
<meta-data android:name="com.toast.sdk.push.notification.default_small_icon"
           android:resource="@drawable/ic_notification"/>
<!-- Sound -->
<meta-data android:name="com.toast.sdk.push.notification.default_sound"
           android:value="notification_sound"/>
<!-- Vibrate pattern -->
<meta-data android:name="com.toast.sdk.push.notification.default_vibrate_pattern"
           android:resource="@array/default_vibrate_pattern"/>
<!-- Use badge icon or not -->
<meta-data android:name="com.toast.sdk.push.notification.badge_enabled"
           android:value="true"/>
<!-- Enable notification when application is foreground. -->
<meta-data android:name="com.toast.sdk.push.notification.foreground_enabled"
           android:value="false"/>
```

#### Set Notification Options with RegisterPush in Runtime

registerPush API 호출시 GamebaseNotificationOptions 인자를 추가하여 알림 옵션을 설정할 수 있습니다.
GamebaseNotificationOptions.newBuilder() 의 인자로 Gamebase.Push.getNotificationOptions() 호출 결과를 전달하면, 현재의 알림 옵션으로 초기화 된 Builder 가 생성되므로, 필요한 값만 변경할 수 있습니다.<br/>
설정 가능한 값은 아래와 같습니다.

| API                   | Parameter       | Description        |
| --------------------  | ------------ | ------------------ |
| enableForeground      | boolean      | 앱이 포그라운드 상태일때의 알림 노출 여부<br/>**default**: false |
| enableBadge           | boolean      | 배지 아이콘 사용 여부<br/>**default**: true |
| setPriority           | int          | 알림 우선 순위. 아래 5가지 값을 설정할 수 있습니다.<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2<br/>**default**: NoticationComapt.HIGH |
| setSmallIconName         | String       | 알림용 작은 아이콘 파일 이름.<br/>설정하지 않을 경우 앱 아이콘이 사용됩니다.<br/>**default**: null |
| setSoundFileName      | String       | 알림음 파일 이름. 안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다.<br/>**default**: null |

**Example**

```java
boolean enableForeground;
boolean enableBadge;
// NotificationCompat.PRIORITY_MIN     : -2
// NotificationCompat.PRIORITY_LOW     : -1
// NotificationCompat.PRIORITY_DEFAULT :  0
// NotificationCompat.PRIORITY_HIGH    :  1
// NotificationCompat.PRIORITY_MAX     :  2
int priority;
String smallIconName;
String soundFileName;

GamebaseNotificationOptions currentOptions = Gamebase.Push.getNotificationOptions(activity);
GamebaseNotificationOptions notificationOptions = GamebaseNotificationOptions.newBuilder(currentOptions)
        .enableForeground(enableForeground)
        .enableBadge(enableBadge)
        .setPriority(priority)
        .setSmallIconName(smallIconName)
        .setSoundFileName(soundFileName)
        .build();

Gamebase.Push.registerPush(activity, pushConfiguration, notificationOptions, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### Get NotificationOptions

푸시를 등록할 때 기존에 설정했던 알림 옵션값을 가져옵니다.
AndroidManifest.xml 에 설정한 값과 런타임에 변경된 값을 포함하여 가장 최근 설정을 가져옵니다.

**API**

```java
+ (GamebaseNotificationOptions)Gamebase.Push.getNotificationOptions(@NonNull Context context);
```

### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다. <br/>
콜백으로 오는 GamebasePushTokenInfo 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

```java
+ (void)Gamebase.Push.queryTokenInfo(@NonNull Activity activity,
                                     @NonNull GamebaseDataCallback<GamebasePushTokenInfo> callback);

// Legacy API
+ (void)Gamebase.Push.queryPush(@NonNull Activity activity,
                                @NonNull GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

```java
Gamebase.Push.queryTokenInfo(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(GamebasePushTokenInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.agreement.pushEnabled;
            boolean enableAdPush = data.agreement.adAgreement;
            boolean enableAdNightPush = data.agreement.adAgreementNight;
            String token = data.token;
        } else {
            // Failed.
        }
    }
});
```

#### GamebasePushTokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | String                | Push 토큰 타입       |
| token               | String                | 토큰                 |
| userId              | String                | 사용자 아이디         |
| deviceCountryCode   | String                | 국가 코드           |
| timezone            | String                | 표준시간대           |
| registeredDateTime  | String                | 토큰 업데이트 시간    |
| languageCode        | String                | 언어 설정            |
| agreement           | GamebasePushAgreement | 수신 동의 여부        |

#### GamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | boolean | 알림 표시 동의 여부           |
| adAgreement      | boolean | 광고성 알림 표시 동의 여부      |
| adAgreementNight | boolean | 야간 광고성 알림 표시 동의 여부  |

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR             | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음을 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 TOAST Push 라이브러리에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

```java
Gamebase.Push.registerPush(activity, pushConfiguration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Register push successful");
            ...
        } else {
            Log.e(TAG, "Register push failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PUSH_EXTERNAL_LIBRARY_ERROR) {
                // TOAST Push Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* TOAST Push SDK 의 오류 코드는 다음과 같습니다.

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API 호출 성공 |
| NOT_INITIALIZE | 100 | TOAST SDK 또는 TOAST Push SDK가 초기화되지 않은 경우 |
| PROVIDER_SDK_ERROR | 101 | 외부 SDK(Firebase)에서 오류가 발생한 경우 |
| USER_ID_NOT_REGISTERED | 102 | 로그인하지 않은 경우 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType을 잘못 입력했거나 푸시 라이브러리가 프로젝트에 포함되지 않은 경우 |
| API_SERVER_ERROR | 104 | TOAST Push 서버 API 호출에 실패한 경우 |
| TOKEN_NOT_REGISTERED | 105 | 내부에 캐시된 푸시 토큰이 없는 경우 |
