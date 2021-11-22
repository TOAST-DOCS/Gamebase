## Game > Gamebase > Android SDK 사용 가이드 > 푸시

### Settings

#### Android Notification Icon

푸시 알림이 도착했을때 표시되는 아이콘은, 기본 상태에서는 앱 아이콘이 사용됩니다.

하지만 Android의 푸시 아이콘은 알파 영역을 적용한 단색 이미지를 사용해야 올바르게 표시됩니다.
[매터리얼 디자인 가이드 - Android Notification \[LINK\]](https://material.io/design/platform-guidance/android-notifications.html#anatomy-of-a-notification)

단말기 해상도 별로 아이콘 사이즈도 정해져 있으므로 다음을 참고하여 푸시 아이콘을 준비하시기 바랍니다.
* MDPI - 24 x 24  (drawable-mdpi)
* HDPI - 36 x 36  (drawable-hdpi)
* XHDPI - 48 x 48  (drawable-xhdpi)
* XXHDPI - 72 x 72  (drawable-xxhdpi)
* XXXHDPI - 96 x 96  (drawable-xxxhdpi)

푸시 아이콘 파일 이름에 공백, 영어 대문자, 특수문자는 피해야 합니다.

그리고 다음 Notification Options 가이드를 참고하여 **default_small_icon**(AndroidManifest.xml) 또는 **setSmallIconName**(API)을 설정해야 합니다.
[Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)
[Game > Gamebase > Android SDK 사용 가이드 > 푸시 > Notification Options > Set Notification Options with RegisterPush in Runtime](./aos-push/#set-notification-options-with-registerpush-in-runtime)

### Register Push

다음 API를 호출하여, NHN Cloud Push에 해당 사용자를 등록합니다.<br/>
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

> <font color="red">[주의]</font><br/>
>
> UserID 마다 푸시 설정이 다를 수 있고, 푸시 토큰이 만료되는 경우도 발생할 수 있으므로 로그인 이후에는 매번 registerPush API를 호출할 것을 권장합니다.

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
설정 방법은 아래 가이드를 확인해주시기 바랍니다.

[Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)

#### Set Notification Options with RegisterPush in Runtime

알림 옵션을 AndroidManifest.xml 에 정의하지 않고 런타임에 설정할 수도 있습니다. 또는 AndroidManifest.xml 에 정의한 값을 런타임에 변경할 수도 있습니다.
registerPush API 호출시 GamebaseNotificationOptions 인자를 추가하여 알림 옵션을 설정합니다.
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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR             | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음을 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud Push 라이브러리에서 발생한 오류입니다.
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

* NHN Cloud Push SDK 의 오류 코드는 다음과 같습니다.

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API 호출 성공 |
| NOT_INITIALIZE | 100 | NHN Cloud SDK 또는 NHN Cloud Push SDK가 초기화되지 않은 경우 |
| PROVIDER_SDK_ERROR | 101 | 외부 SDK(Firebase)에서 오류가 발생한 경우 |
| USER_ID_NOT_REGISTERED | 102 | 로그인하지 않은 경우 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType을 잘못 입력했거나 푸시 라이브러리가 프로젝트에 포함되지 않은 경우 |
| API_SERVER_ERROR | 104 | NHN Cloud Push 서버 API 호출에 실패한 경우 |
| TOKEN_NOT_REGISTERED | 105 | 내부에 캐시된 푸시 토큰이 없는 경우 |
