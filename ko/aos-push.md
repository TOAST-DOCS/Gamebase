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
    implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"
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
    * 직접 string resource(xml) 파일을 만들어서 Assets/Plugins/Android/res/values/ 폴더에 포함시켜야 합니다.
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 다음은 string resource(xml) 파일의 예시입니다.
            ```
            <!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
            <?xml version="1.0" encoding="utf-8"?>
            <resources>
            <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
            <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
            <string name="firebase_database_url" translatable="false">https://tap-development-00000000.firebaseio.com</string>
            <string name="google_app_id" translatable="false">1:000000000000:android:749cbe01c8ada279</string>
            <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
            <string name="google_storage_bucket" translatable="false">tap-development-00000000.appspot.com</string>
            </resources>
            ```
        * string resource(xml) 파일에서 설정하는 각각의 값은 Firebase Console > 프로젝트 설정 > google-services.json 파일을 다운로드해서 확인 가능합니다.
            Firebase 서비스 연동에 따라서 google-services.json 파일의 내용은 달라질 수 있습니다.
            ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)

#### Tencent

* Tencent 푸시를 사용하려면 아래 가이드에 따라 Tencent 설정을 완료해야 합니다.
	* [TOAST > TOAST SDK 사용 가이드 > TOAST Push > Android > Tencent Push Notification 설정](/TOAST/ko/toast-sdk/push-android/#tencent-push-notification)

### Register Push

다음 API를 호출하여, TOAST Push에 해당 사용자를 등록합니다.<br/>
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, PushConfiguration configuration, GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = new PushConfiguration(enablePush, enableAdPush, enableAdNightPush);

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Register push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다. <br/>
콜백으로 오는 PushConfiguration 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

```java
Gamebase.Push.queryPush(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(PushConfiguration data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.pushEnabled;
            boolean enableAdPush = data.adAgreement;
            boolean enableAdNightPush = data.adAgreementNight;
        } else {
            // Failed.
            Log.e(TAG, "Query push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

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
| PROVIDER_SDK_ERROR | 101 | 외부 SDK(Firebase, Tencent)에서 오류가 발생한 경우 |
| USER_ID_NOT_REGISTERED | 102 | 로그인하지 않은 경우 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType을 잘못 입력했거나 푸시 라이브러리가 프로젝트에 포함되지 않은 경우 |
| API_SERVER_ERROR | 104 | TOAST Push 서버 API 호출에 실패한 경우 |
| TOKEN_NOT_REGISTERED | 105 | 내부에 캐시된 푸시 토큰이 없는 경우 |
