## Upcoming Products > Gamebase > Android Developer's Guide

## Configuration

### Getting Started

#### Environments

> [ 최소사양 ]
> Android API 15 (IceCreamSandwichMR1, 4.0.3) 이상

* 개발 환경 : Android Studio

#### Installation
Gamebase Android SDK를 사용하기 전에 TOAST Cloud Console에서 App ID를 발급 받아야 합니다.

##### Download Link

* [DOWNLOAD Gamebase Android SDK](http://docs.cloud.toast.com/ko/Download/)
* 다운로드 받은 SDK에서 다음 폴더 및 aar 파일을 프로젝트에 추가합니다.
	* **gamebase-sdk/**
	* **gamebase-sdk-{version}.aar**
	* **gamebase-sdk-base-{version}.aar**

* 인증 모듈 추가
	* 다운로드 받은 SDK의 **gamebase-adapter-auth-{provider}** 폴더를 프로젝트에 추가합니다.
	* google, facebook, payco 중에서 사용할 인증 모듈을 모두 추가합니다.

#### build.gradle Settings

* 1) 다운로드 받은 Gamebase SDK를 Application의 Root 경로에 복사합니다.
* 2) Gamebase 경로 및 버전, 사용할 인증, 결제, 푸시 모듈을 설정합니다.
    * 사용하고자 하는 모듈을 true로 설정하면 해당 모듈이 dependency에 추가됩니다.

```
/* Set the Gamebase path. */
def gamebaseDir = '../Gamebase'

/* Set the Gamebase version. */
def gamebaseSdkVersion = '1.0.0'

/* Set the Gamebase authentication modules. */
def useAuthFacebook = true;
def useAuthGoogle = true;
def useAuthPayco = true;

/* Set the Gamebase purchase modules. */
def usePurchaseIAP = true;

/* Set the Gamebase push modules. */
def usePushFCM = true;
def usePushTencent = false; // Not supported yet.
```

* 3) Repositories Settings
    * 아래 내용을 repositories에 추가합니다.

```
repositories {
    flatDir {
        dirs "${gamebaseDir}"
        dirs "${gamebaseDir}/gamebase-sdk"
        dirs "${gamebaseDir}/gamebase-adapter-auth-google"
        dirs "${gamebaseDir}/gamebase-adapter-auth-facebook"
        dirs "${gamebaseDir}/gamebase-adapter-auth-payco"
        dirs "${gamebaseDir}/gamebase-adapter-purchase-iap"
        dirs "${gamebaseDir}/gamebase-adapter-push-fcm"
        dirs "${gamebaseDir}/gamebase-adapter-push-tencent"
    }
}
```

* 4) Dependencies Settings
    * 아래 내용을 dependencies에 추가합니다.

```
dependencies {
    ...

    // Compile Gamebase SDKs
    compile(name: "gamebase-sdk-${gamebaseSdkVersion}", ext: 'aar')
    compile(name: "gamebase-sdk-base-${gamebaseSdkVersion}", ext: 'aar')

    // Compile Gamebase External Libraries
    compile 'com.google.code.gson:gson:2.2.4'
    compile 'com.squareup.okhttp3:okhttp:3.6.0'

    // Compile Authentication Modules
    if (useAuthFacebook) {
        compile(name: "gamebase-adapter-auth-facebook-${gamebaseSdkVersion}", ext: 'aar')
        compile 'com.facebook.android:facebook-android-sdk:4.17.0'
    }
    if (useAuthGoogle) {
        compile 'com.google.android.gms:play-services-auth:10.0.1'
        compile(name: "gamebase-adapter-auth-google-${gamebaseSdkVersion}", ext: 'aar')
    }
    if (useAuthPayco) {
        compile(name: "gamebase-adapter-auth-payco-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'paycologin-1.2.6', ext: 'aar') {
            exclude group: 'com.google.code.gson', module: 'gson'
        }
        compile 'com.google.android.gms:play-services-base:10.0.1'
    }

    // Compile Purchase Modules
    if (usePurchaseIAP) {
        compile(name: "gamebase-adapter-purchase-iap-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'iap-release-1.3.1', ext: 'aar')
        compile(name: 'mobill-core-release-1.3.1', ext: 'jar')
        compile 'com.squareup.okhttp:okhttp:1.5.4'
    }

    // Compile Push Modules
    if (usePushFCM) {
        compile(name: "gamebase-adapter-push-fcm-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.3', ext: 'aar')
        compile 'com.google.android.gms:play-services-gcm:10.0.1'
        compile 'com.google.firebase:firebase-messaging:10.0.1'
    } else if (usePushTencent) {
        compile(name: "gamebase-adapter-push-tencent-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.3', ext: 'aar')
    }
}
```

#### Dependency

| Category | Provider | Modules | Dependency | Description |
| -------- | -------- | ------- | ---------- | ----------- |
| **Gamebase<br>(Required)** | Gamebase | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-24.0.0.aar<br>support-v4-24.0.0.aar<br>support-annotations-24.0.0.jar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |  |
| **Authentication<br>(Optional)** | Google | gamebase-adapter-auth-google-{version}.aar | play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>play-services-auth-10.0.1.aar<br>play-services-auth-base-10.0.1.aar |  |
|  | Facebook | gamebase-adapter-auth-facebook-{version}.aar | facebook-android-sdk-4.17.0.aar<br>appcompat-v7-24.0.0.aar<br>support-vector-drawable-24.0.0.aar<br>animated-vector-drawable-24.0.0.aar<br>cardview-v7-24.0.0.aar<br>customtabs-24.0.0.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar |  |
|  | Payco | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.2.6.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar |  |
| **Purchase<br>(Optional)** | IAP | gamebase-adapter-purchase-iap-{version}.aar | iap-release-1.3.1.aar<br>iap-tstore-release-1.3.1.aar<br>mobill-core-release-1.3.1.jar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |  |
| **Push<br>(Optional)** | FCM | gamebase-adapter-push-fcm-{version}.aar | pushsdk-release-v1.3.aar<br>firebase-common-10.0.1.jar<br>firebase-iid-10.0.1.jar<br>firebase-messaging-10.0.1.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-gcm-10.0.1.aar<br>play-services-iid-10.0.1.aar<br>play-services-tasks-10.0.1.aar |  |
|  | Tencent | gamebase-adapter-push-tencent-{version}.aar | pushsdk-release-v1.3.aar | 현재 지원되지 않습니다. |

* Required 항목은 필수로 포함되어야 하는 모듈입니다.
* Optional 항목은 해당 기능이 필요할 경우 포함되어야 하는 모듈입니다.
* 중복되는 Dependency 모듈은 하나만 포함하도록 해야합니다.
<br>
* 외부 SDK 가이드
    * Facebook Developers Guide : [Facebook for developers](https://developers.facebook.com/docs/android)
    * Google Developers Guide [Google APIs for Android](https://developers.google.com/android/guides/overview)

### Initialization

#### 1. Activate the application
앱의 Lifecycle 관리를 위해 앱이 활성화 되었음을 Gamebase SDK에 알립니다.
**Application#onCreate()**에서 **Gamebase#activeApp(Context)**을 호출합니다.

```java
public class GamebaseApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Gamebase.activeApp(getApplicationContext());
    }
}
```

#### 2. Initialization
**Activity#onCreate(Bundle)**에서 **Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback\<LaunchingInfo\>)**을 호출하여 Gamebase SDK 초기화를 진행합니다.

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /**
         * Gamebase Configuration.
         */
        GamebaseConfiguration configuration =
                        new GamebaseConfiguration.Builder()
                                .setAppId("T0aStC1d")
                                .setAppVersion("1.0.0")
                                .build();
        /**
         * Gamebase Initialize.
         */
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    // 런칭 상태를 확인합니다.
                    LaunchingStatus status = data.getStatus();
                    if (status.isPlayable()) {
                        // 게임 플레이
                    } else {
                        // 점검 또는 앱을 업데이트 해야합니다.
                    }
                } else {
                    // 초기화에 실패하면 Gamebase SDK를 이용할 수 없습니다.
                    // 에러를 노출 후 게임을 재시작 또는 종료해야합니다.
                }
            }
        });
        ...
    }
    ...
}
```

### Login
Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음의 링크를 참고하시길 바랍니다.
{링크}

#### 1. Log in as the latest login IDP
가장 최근에 로그인한 IDP로의 로그인을 시도합니다. 해당 로그인에 대한 토큰이 만료되었거나,
토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다. 이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.
```java
private static void onLoginLastLoggedIn() {
    Gamebase.loginForLastLoggedInProvider(new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Login successful");
                // 로그인 성공
            } else {
                Log.e(TAG, "Login failed.");
                // 로그인 실패
            }
        }
    });
}
```

#### 2. Log in using a specific IDP
특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.
```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Login successful");
                // 로그인 성공
            } else {
                Log.e(TAG, "Login failed.");
                // 로그인 실패
            }
        }
    });
}
```

Login을 호출한 Activity의 Activity#onActivityResult(int, int, Intent)에서 Gamebase.onActivityResult(int, int, Intent)을 호출합니다.
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    Gamebase.onActivityResult(requestCode, resultCode, data);
}
```

### Logout
로그아웃 버튼을 클릭했을 때, 다음과 같이 로그아웃 API를 구현합니다.
```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Logout successful");
                // 로그아웃 성공
            } else {
                Log.e(TAG, "Logout failed.");
                // 로그아웃 실패
            }
        }
    });
}
```

### Withdraw
탈퇴 버튼을 클릭했을 때, 다음과 같이 로그아웃 API를 구현합니다.
```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Withdraw successful");
                // 탈퇴 성공
            } else {
                Log.e(TAG, "Withdraw failed.");
                // 로그아웃 실패
            }
        }
    });
}
```

### Mapping
Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때,
각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

#### 1. Add mapping
특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면,
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다. 즉, Mapping은 단순히 IDP를 연동만 해줍니다.

아래의 예시에서는 facebook에 대해서 Mapping을 시도하고 있습니다.
```java
public static void addMappingForFacebook(final Activity activity) {
        Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
            @Override
            public void onCallback(AuthToken result, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    Log.d(TAG, "Add Mapping successful");
                    // 맵핑 추가 성공
                } else {
                    Log.d(TAG, "Add Mapping failed");
                    // 맵핑 추가 성공
                }
            }
        });
    }
```
#### 2. Remove mapping
특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다. 연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.
```java
private static void removeMappingForFacebook() {
        Gamebase.removeMapping(AuthProvider.FACEBOOK, new GamebaseCallback() {
            @Override
            public void onCallback(GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    Log.d(TAG, "Remove mapping successful");
                    // 맵핑 해제 성공
                } else {
                    Log.d(TAG, "Remove mapping failed");
                    // 맵핑 해제 실패
                }
            }
        });
    }
```

### Purchase

#### 1. Settings

##### 1-1. Download

* 다운로드 받은 SDK의 **gamebase-adapter-purchase-iap** 폴더를 프로젝트에 추가합니다.

##### 1-2. Initialization

* Gamebase 초기화시 configuration의 **setStoreCode()**를 호출합니다.
* 사용 가능한 마켓 리스트는 다음 가이드에 나와 있습니다.
	* [IAP-AndroidManifest 설정 방법](http://docs.cloud.toast.com/ko/Common/IAP/Android%20Developer%60s%20Guide/#androidmanifestxml_1)
	* **com.toast.iap.config.market** 항목을 참고합니다.

```java
String STORE_CODE = PurchaseProvider.StoreCode.GOOGLE;

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
        .setStoreCode(STORE_CODE)	// Store code를 반드시 선언합니다.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### 2. Purchase item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
        	// Succeeded.
        } else if(exception.getCode() == GamebaseError.PURCHASE_USER_CANCELED) {
        	// User canceled.
        } else {
            // Failed.
        }
    }
});
```

#### 3. Get a list of items purchasable

아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### 4. Get a list of items not consumed

아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 **미소비 결제내역**을 요청합니다. 해당 내역을 받은 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

```java
Gamebase.Purchase.requestItemListOfNotConsumed(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### 5. Reprocess purchase transaction

스토어 결제는 정상적으로 이루어졌지만, ToastCloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에,
해당 API를 이용하여 재처리를 시도합니다. 최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

### Push

#### 1. Settings

##### 1-1. Download

* Firebase 푸쉬를 사용하는 경우
	* 다운로드 받은 SDK의 **gamebase-adapter-push-fcm** 폴더를 프로젝트에 추가합니다.

##### 1-2. AndroidManifest.xml (Firebase only)

* Gamebase 푸시에 필요한 설정을 추가합니다.
>**${applicationId}**을 **패키지 네임**으로 변경하여야 합니다.

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

##### 1-3. Initialization

* Gamebase 초기화시 configuration의 **setPushType()**을 호출합니다.
* Firebase 푸쉬를 사용하는 경우
	* 추가로 **setFCMSenderId()**를 호출합니다.

```java
private static final String PUSH_FCM_SENDER_ID = "...";

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
        .setFCMSenderId(PUSH_FCM_SENDER_ID)	// Firebase는 SenderId가 필요합니다.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### 2. Register push
다음의 API를 호출하여, ToastCloud Push에 해당 사용자를 등록합니다.
Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을 사용자로부터 받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.

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
        }
    }
});
```

#### 3. Get push settings
사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다.
콜백으로 오는 PushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

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
        }
    }
});
```

### UI

## Error codes

| Category | Sub Category | Error | Error Code | Notes |
| --- | --- | --- | --- | --- |
|Common| | NOT_INITIALIZED | 1 | |
|      | | NOT_LOGGED_IN | 2 | |
|      | | INVALID_PARAMETER | 3 | |
|      | | INVALID_JSON_FORMAT | 4 | |
|      | | USER_PERMISSION | 5 | |
|      | | NOT_SUPPORTED | 10 | |
| Network | Socket | SOCKET_RESPONSE_TIMEOUT | 101 | |
|         | | SOCKET_ERROR | 110 | |
|         | | SOCKET_UNKNOWN_ERROR | 999 | |
| Launching | | LAUNCHING_SERVER_ERROR | 2001 | |
| Auth | Common | AUTH_USER_CANCELED | 3001 | |
|      |        | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | |
|      |        | AUTH_NOT_EXIST_MEMBER | 3003 | |
|      |        | AUTH_INVALID_MEMBER | 3004 | |
|      |        | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | |
|      | Gamebase Login | AUTH_TAP_LOGIN_FAILED | 3101 | |
|      |          | AUTH_TAP_LOGIN_INVALID_TOKEN_INFO | 3102 | |
|      |          | AUTH_TAP_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | |
|      | IDP Login | AUTH_IDP_LOGIN_FAILED | 3201 | |
|      |           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3201 | |
|      | Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | |
|      |            | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | |
|      |            | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | |
|      |            | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | |
|      | Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | |
|      |               | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | |
|      |               | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | |
|      | Logout | AUTH_LOGOUT_FAILED | 3501 | |
|      | Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | |
|      | Not Playable | AUTH_NOT_PLAYABLE | 3701 | |
|      | Unknown | AUTH_UNKNOWN_ERROR | 3999 | |
| Purchase | | PURCHASE_NOT_INITIALIZED | 4001 | |
|          | | PURCHASE_USER_CANCELED | 4002 | |
|          | | PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | |
|          | | PURCHASE_NOT_SUPPORTED_MARKET | 4010 | |
|          | | PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | |
|          | | PURCHASE_UNKNOWN_ERROR | 4999 | |
| Push | | PUSH_NOT_REGISTERED | 5001 | |
|      | | PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | |
|      | | PUSH_UNKNOWN_ERROR | 5999 | |
| UI | | UI_UNKNOWN_ERROR | 6999 | |
| Server | | SERVER_INTERNAL_ERROR | 8001 | |
|        | | SERVER_REMOTE_SYSTEM_ERROR | 8002 | |
|        | | SERVER_UNKNOWN_ERROR | 8999 | |

## Sample Application

## API Reference
