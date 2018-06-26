## Game > Gamebase > Android SDK 사용 가이드 > 푸시

### Settings

여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

#### TOAST Cloud Console 등록

먼저 [Notification > Push > API v2.0 가이드](/Notification/Push/ko/api-guide/)를 참고하여 Console을 설정합니다.

#### Download

* Firebase 푸시를 사용하는 경우
    * 다운로드한 SDK의 **gamebase-adapter-push-fcm** 폴더를 프로젝트에 추가합니다.
* Tencent 푸시를 사용하는 경우
    * 다운로드한 SDK의 **gamebase-adapter-push-tencent** 폴더를 프로젝트에 추가합니다.

> <font color="red">[중요]</font><br/>
>
> 푸시 모듈은 하나만 있어야 합니다. <br/>
> Firebase 푸시와 Tencent 푸시를 동시에 프로젝트에 추가하지 마십시오.


#### AndroidManifest.xml

* Gamebase 푸시에 필요한 설정을 추가합니다.

> <font color="red">[중요]</font><br/>
>
> **${applicationId}**를 **패키지 네임**으로 변경해야 합니다.
>

*Firebase*

```xml
<manifest>
    ...
    <permission
        android:name="${applicationId}.permission.C2D_MESSAGE"
        android:protectionLevel="signature" />
    <uses-permission android:name="${applicationId}.permission.C2D_MESSAGE" />
    ...
    <application>
    ...
        <provider
            android:name="com.google.firebase.provider.FirebaseInitProvider"
            android:authorities="${applicationId}.firebaseinitprovider"
            android:exported="false"
            android:initOrder="100" />
        <receiver
            android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
            android:exported="true"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />

                <category android:name="${applicationId}" />
            </intent-filter>
        </receiver>
    ...
    </application>
</manifest>
```

*Tencent*

```xml
<manifest>
    ...
    <application>
    ...
        <provider
            android:name="com.tencent.android.tpush.XGPushProvider"
            android:authorities="${applicationId}.AUTH_XGPUSH"
            android:exported="true" />
        <provider
            android:name="com.tencent.android.tpush.SettingsContentProvider"
            android:authorities="${applicationId}.TPUSH_PROVIDER"
            android:exported="false" />
        <provider
            android:name="com.tencent.mid.api.MidProvider"
            android:authorities="${applicationId}.TENCENT.MID.V3"
            android:exported="true" />
    ...
    </application>
</manifest>
```

#### Google Services Settings (Firebase only)

* Gradle 빌드를 사용하는 경우
    * Firebase 푸시를 사용하기 위해서는 google-services.json 설정 파일이 필요합니다. 설정 파일을 프로젝트에 포함하는 방법은 [Firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/#add_firebase_to_your_app) 설명을 참고합니다.
    * gradle 설정에 **apply plugin: 'com.google.gms.google-services'**를 추가합니다.
    * 위 설정으로 Google Services Gradle Plugin이 적용되어 google-services.json 파일을 res/google-services/{build_type}/values/values.xml라는 이름의 string resource로 변경하여 사용하게 됩니다.
* Unity 빌드인 경우
    * Google Services Gradle Plugin을 사용할 수 없습니다. 직접 string resource를 만들어 프로젝트에 포함하려면 [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file) 설명을 참고합니다. 
        * 다음은 string resource 파일의 예시입니다.

```xml
<!-- res/values/google-services-json.xml -->
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

#### Initialization

* Gamebase 초기화 시 configuration의 **setPushType()**을 호출합니다.
* Firebase 푸시를 사용하는 경우
    * 추가로 **setFCMSenderId()**를 호출합니다.
* Tencent 푸시를 사용하는 경우
    * 추가로 **setTencentAccessId()**를 호출합니다.
    * 추가로 **setTencentAccessKey()**를 호출합니다.

```java
private static final String PUSH_FCM_SENDER_ID = "...";
private static final String PUSH_TENCENT_ACCESS_ID = "...";
private static final String PUSH_TENCENT_ACCESS_KEY = "...";

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setFCMSenderId(PUSH_FCM_SENDER_ID)				// Firebase는 SenderId가 필요합니다.
        .setTencentAccessId(PUSH_TENCENT_ACCESS_ID)		// Tencent AccessId가 필요합니다.
        .setTencentAccessKey(PUSH_TENCENT_ACCESS_KEY)	// Tencent AccessKey가 필요합니다.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

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

* TOAST Push 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Notification > Push > 오류 코드](/Notification/Push/ko/sdk-guide/#_5)




