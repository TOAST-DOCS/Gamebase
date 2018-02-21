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

* [DOWNLOAD Gamebase Android SDK](http://docs.cloud.toast.com/ko/Download/#upcoming-products-gamebase)
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
        compile(name: 'iap-1.3.2', ext: 'aar')
        compile(name: 'mobill-core-1.3.2', ext: 'jar')
        compile 'com.squareup.okhttp:okhttp:1.5.4'
    }

    // Compile Push Modules
    if (usePushFCM) {
        compile(name: "gamebase-adapter-push-fcm-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.32', ext: 'aar')
        compile 'com.google.android.gms:play-services-gcm:10.0.1'
        compile 'com.google.firebase:firebase-messaging:10.0.1'
    } else if (usePushTencent) {
        compile(name: "gamebase-adapter-push-tencent-${gamebaseSdkVersion}", ext: 'aar')
        compile(name: 'pushsdk-release-v1.32', ext: 'aar')
    }
}
```


## Initialization

### 1. Activate the application
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

### 2. Initialization
**Activity#onCreate(Bundle)**에서 **Gamebase#initialize(Activity, GamebaseConfiguration, GamebaseDataCallback)**을 호출하여 Gamebase SDK 초기화를 진행합니다.

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
                                .enableLaunchingStatusPopup(true)
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
                    Log.e(TAG, "Initialize failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });
        ...
    }
    ...
}
```

#### 3. Getting launching status
Gamebase#initialize 호출 결과로 런칭 상태를 확인 할 수 있습니다.
```java
Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // 런칭 상태를 확인합니다.
            LaunchingStatus status = data.getStatus();
            int statusCode = status.getCode();
            switch (statusCode) {
                case LaunchingStatus.INSPECTING_SERVICE:
                    // 점검중...
                    break;
                ...
            }
            ...
        }
        ...
    }
});
```

런칭 상태 코드

| Status | Code | Description |
| --- | --- | --- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업데이트 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | 점검 중이지만 QA 유저 서비스 가능 |
| REQUIRE_UPDATE | 300 | 업데이트 필수 |
| BLOCKED_USER | 301 | 접속 차단 유저 |
| TERMINATED_SERVICE | 302 | 서비스 종료 |
| INSPECTING_SERVICE | 303 | 서비스 점검 중 |
| INSPECTING_ALL_SERVICES | 304 | 전체 서비스 점검 중 |
| INTERNAL_SERVER_ERROR | 500 | 내부 서버 에러 |

## Login
Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음의 링크를 참고하시길 바랍니다.
[3rd-Party Provider SDK Guide](Android Developer`s Guide#3rd-party-provider-sdk-guide)

### 1. Login as the latest login IDP
가장 최근에 로그인한 IDP로의 로그인을 시도합니다. 해당 로그인에 대한 토큰이 만료되었거나,
토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다. 이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.
```java
private static void onLoginLastLoggedIn() {
    Gamebase.loginForLastLoggedInProvider(new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
				// 로그인 성공
                Log.d(TAG, "Login successful");
            } else {
	            // 로그인 실패
	            Log.e(TAG, "Login failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());

	            if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                    exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
	                    // Socket error 입니다.
	                    // 네트워크 상태를 확인 후 loginForLastLoggedInPovider 메소드 호출을 재시도 할 수 있습니다.
                    }
            }
        }
    });
}
```
### 2. Login with GUEST
Gamebase는 Guest 로그인을 지원합니다.
디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
Guest 로그인은 앱 삭제 또는 디바이스 초기화 시에 계정이 삭제될 수 있으므로 IDP를 활용한 로그인 방식을 권장합니다.
```java
private static void onLoginForGuest(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GUEST, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Login successful");
                // 로그인 성공
            } else {
                Log.e(TAG, "Login failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
                // 로그인 실패
            }
        }
    });
}
```


### 3. Login using a specific IDP
특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.
```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                Log.d(TAG, "Login successful");
                // 로그인 성공
            } else {
                Log.e(TAG, "Login failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
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

### 4. Login with access token of external IDP.
외부 인증 라이브러리에서 발급한 access token을 이용하여 Gamebase에 로그인 합니다.

```java
Map<String, Object> credentialInfo = new HashMap<>();
credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

Gamebase.login(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
            @Override
            public void onCallback(AuthToken data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    Log.d(TAG, "Login successful");
                } else {
                    Log.e(TAG, "Gamebase Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        });
```

### 5. Gets Authentication Information for external IDP.

외부 인증 SDK에서 AccessToken, UserId, Profile 등의 정보를 가져올 수 있습니다.

```java
// 유저 ID를 가져옵니다.
String userId = getAuthProviderUserID(AuthProvider.FACEBOOK);
// AccessToken을 가져옵니다.
String accessToken = getAuthProviderAccessToken(AuthProvider.FACEBOOK);
// User Profile 정보를 가져옵니다.
AuthFacebookProfile profile = (AuthFacebookProfile) getAuthProviderProfile(AuthProvider.FACEBOOK);
String name = profile.getName();    // or profile.information.get("name")
String email = profile.getEmail();  // or profile.information.get("email")
```

### 6. Authentication Additional Information Settings.

#### Facebook
* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Facebook의 경우, OAuth 인증 시도 시, Facebook으로 부터 요청할 정보의 종류를 설정해야 합니다.

Facebook 인증 추가 정보 입력 예제

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### Payco
* **TOAST Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* Payco의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**의 설정이 필요합니다.

Payco 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_code": "Your Service Name" }
```

## Logout
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
                Log.e(TAG, "Logout failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
                // 로그아웃 실패
            }
        }
    });
}
```

## Withdraw
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
                Log.e(TAG, "Withdraw failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
                // 로그아웃 실패
            }
        }
    });
}
```

## Mapping
Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때,
각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

### 1. Add mapping
특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면,
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니다.

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
                    Log.e(TAG, "Add mapping failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
                    // 맵핑 추가 성공
                }
            }
        });
    }
```
### 2. Remove mapping
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
                    Log.e(TAG, "Remove mapping failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
                    // 맵핑 해제 실패
                }
            }
        });
    }
```

## Purchase

### 1. Settings

#### 1-1. Store Console

* 다음 IAP 가이드를 참고하여 각 스토어에 앱을 등록 하고 어플리케이션 키를 발급받도록 합니다.
	* [IAP > Store interlocking information](http://docs.cloud.toast.com/ko/Common/IAP/ko/Store%20interlocking%20information/)

#### 1-2. Register as Store's Tester

* 결제 테스트를 위하여 스토어별로 다음과 같이 테스터로 등록합니다.
	* Google
		* [Android > 테스트 구매 설정](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
	* ONE store
		* [ONE store > 인앱결제 테스트](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
		* 반드시 In-App 정보 - 테스트 버튼으로 샌드박스를 원하는 단말기 전화번호를 등록해서 테스트 해야 합니다.
		* 테스트용 단말기는 USIM이 있어야 하고, 전화번호를 등록해야 합니다.(MDN)
		* **ONE store** 어플리케이션이 설치되어 있어야 합니다.

#### 1-3. TOAST Cloud Console

* IAP 가이드를 참고하여 IAP 설정 및 상품등록을 합니다.
	* [IAP > Getting Started](http://docs.cloud.toast.com/ko/Common/IAP/ko/Web%20Console/)

#### 1-4. Download

* 다운로드 받은 SDK의 **gamebase-adapter-purchase-iap** 폴더를 프로젝트에 추가합니다.
	* ONE Store 결제가 필요 없다면 **iap-tstore-x.x.x.jar**, **iap_tstore_plugin_vxx.xx.xx.jar** 파일은 삭제하셔도 됩니다.
	* 반대로 ONE Store 결제를 하신다면 위의 jar파일은 반드시 프로젝트에 포함되어 빌드되어야 합니다.

#### 1-5. AndroidManifest.xml(ONE store only)
* ONE store 사용을 위해서는 다음 설정을 추가하여야 합니다.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!-- 버전 16.XX.XX의 경우, 4를 입력합니다. https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development:개발모드 / release:운영 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 1-6. Initialization

* Gamebase 초기화시 configuration의 **setStoreCode()**를 호출합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
	* GG : Google
	* TS : ONE store
	* TEST : IAP 테스트용

```java
String STORE_CODE = "GG";	// Google

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

### 2. Purchase flow

* 아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.
	1. requestPurchase 를 호출하여 결제를 시도합니다.
	2. 결제가 성공하였다면 requestItemListOfNotConsumed 를 호출하여 미소비 결제내역을 확인합니다.
	3. 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume 을 요청합니다.
	4. 게임 서버는 IAP서버에 서버 API를 통해 Consume 을 요청합니다.
	5. IAP서버에서 Consume이 성공하였다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.
* 스토어 결제는 성공하였으나 에러가 발생하여 정상 종료되지 못하는 경우가 있습니다. 다음과 같은 순서로 재처리 로직을 구현하시기 바랍니다.
	1. 로그인이 성공하면 requestRetryTransaction 을 호출하여 미처리 내역에 대한 자동 재처리를 시도합니다.
	2. 리턴된 successList 에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
	3. 리턴된 failList 에 값이 존재한다면 해당 값을 게임 서버나 Log&Crash 등을 통해 전송하여 데이터를 확보하고, 빌링 개발팀에 재처리 실패 원인을 문의합니다.

### 3. Purchase item

* 구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.
* 유저가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 에러가 리턴됩니다. 취소 처리를 해주시기 바랍니다.

```java
long itemSeq; // The itemSeq value can be got through the requestItemListPurchasable API.

Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else if(exception.getCode() == GamebaseError.PURCHASE_USER_CANCELED) {
            // User canceled.
        } else {
            // To Purchase Item Failed cause of the error
        }
    }
});
```

### 4. Get a list of items purchasable

* 아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### 5. Get a list of items not consumed

* 아이템을 구매는 하였지만, 아직 아이템 배송을 실시하지 않은 **미소비 결제내역**리스트를 요청합니다.
* 리턴받은 리스트가 존재하는 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

```java
Gamebase.Purchase.requestItemListOfNotConsumed(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### 6. Reprocess purchase transaction

* 클라이언트에 결제 실패 로직이 남아있는 경우 재처리를 실행하고, 성공/실패 결과를 콜백함수로 리턴합니다.
	* 스토어 결제는 정상적으로 이루어졌지만, ToastCloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이루어지지 않은 경우 등이 이에 해당합니다.
* 성공/실패 여부에 따라 각 게임별 아이템 배송 로직을 진행하거나 에러 리스트를 어떻게 관리할 것인지는 프로젝트 마다 정책이 다를 수 있으므로 Gamebase SDK는 requestRetryTransaction 를 자동으로 호출해주지 않습니다.
	* 로그인 성공 후 직접 호출하여야 합니다.

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request retry transaction failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### 7. Exception handling

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| PURCHASE_NOT_INITIALIZED | 4001 | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-iap 모듈을 프로젝트에 추가 했는지 확인해주세요. |
| PURCHASE_USER_CANCELED | 4002 | 유저가 아이템 구매를 취소하였습니다. |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다. |
| PURCHASE_NOT_ENOUGH_CASH | 4004 | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다. |
| PURCHASE_NOT_SUPPORTED_MARKET | 4010 | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), TS(ONE Store), TEST 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | IAP 라이브러리 에러입니다.<br>DetailCode를 확인하세요. |
| PURCHASE_UNKNOWN_ERROR | 4999 | 정의되지 않은 구매 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |

#### PURCHASE_EXTERNAL_LIBRARY_ERROR
* 이 에러는 IAP 모듈에서 발생한 에러입니다.
* exception.getDetailCode() 를 통해 IAP 에러 코드를 확인하여야 합니다.
	* IAP 에러코드는 다음 문서를 참고하시기 바랍니다.
	* [IAP > Error Code Guide > Client API 에러 타입](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)

## Push

### 1. Settings

#### 1-1. TOAST Cloud Console

* TCPush 가이드를 참고하여 Console 설정을 합니다.
	* [Push > Developer's Guide](http://docs.cloud.toast.com/ko/Notification/Push/ko/Developer%60s%20Guide/)

#### 1-2. Download

* Firebase 푸쉬를 사용하는 경우
	* 다운로드 받은 SDK의 **gamebase-adapter-push-fcm** 폴더를 프로젝트에 추가합니다.
* Tencent 푸쉬를 사용하는 경우
	* 다운로드 받은 SDK의 **gamebase-adapter-push-tencent** 폴더를 프로젝트에 추가합니다.
> 푸쉬 모듈은 하나만 존재하여야 합니다.
> Firebase 푸쉬와 Tencent 푸쉬를 둘 다 동시에 프로젝트에 추가하지 마십시오.

#### 1-3. AndroidManifest.xml

* Gamebase 푸쉬에 필요한 설정을 추가합니다.
>**${applicationId}**을 **패키지 네임**으로 변경하여야 합니다.

##### Firebase

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

##### Tencent

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

#### 1-4. Google Services Settings (Firebase only)
* Gradle 빌드를 사용하는 경우
    * Firebase 푸쉬를 사용하기 위해서는 google-services.json 설정파일이 필요합니다.
        * [https://firebase.google.com/docs/notifications/android/console-audience#add_firebase_to_your_app](https://firebase.google.com/docs/notifications/android/console-audience#add_firebase_to_your_app)
        * 위 링크를 참조하여 설정파일을 프로젝트에 포함시킵니다.
    * gradle 설정에 **apply plugin: 'com.google.gms.google-services'** 를 추가합니다.
    * 위 설정으로 Google Services Gradle Plugin이 적용되어 google-services.json 파일을 res/google-services/{build_type}/values/values.xml 라는 이름의 string resource로 변경하여 사용하게 됩니다.
* Unity 빌드인 경우
	* Google Services Gradle Plugin을 사용할 수 없으므로 다음 링크의 설명에 따라 직접 string resource를 만들어 프로젝트에 포함하도록 합니다.
	* [https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
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

#### 1-5. Initialization

* Gamebase 초기화시 configuration의 **setPushType()**을 호출합니다.
* Firebase 푸쉬를 사용하는 경우
	* 추가로 **setFCMSenderId()**를 호출합니다.
* Tencent 푸쉬를 사용하는 경우
	* 추가로 **setTencentAccessId()**를 호출합니다.
	* 추가로 **setTencentAccessKey()**를 호출합니다.

```java
private static final String PUSH_FCM_SENDER_ID = "...";
private static final String PUSH_TENCENT_ACCESS_ID = "...";
private static final String PUSH_TENCENT_ACCESS_KEY = "...";

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
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

### 2. Register push

* 다음 API를 호출하여, ToastCloud Push에 해당 사용자를 등록합니다.
* Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을 사용자로부터 받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.

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

### 3. Get push settings

* 사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다.
* 콜백으로 오는 PushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

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

### 4. Exception handling

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | TCPush 라이브러리 에러입니다.<br>DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 PUSH API 호출이 완료되지 않았습니다.<br>이전 PUSH API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR | 5999 | 정의되지 않은 푸시 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |

#### PUSH_EXTERNAL_LIBRARY_ERROR
* 이 에러는 TOAST Cloud Push 라이브러리에서 발생한 에러입니다.
* exception.getDetailCode() 를 통해 TCPush 에러 코드를 확인하여야 합니다.
	* TCPush 에러코드는 다음 문서를 참고하시기 바랍니다.
	* [Push > Client SDK Developer's Guide > Error Code Guide > 오류 처리](http://docs.cloud.toast.com/ko/Notification/Push/ko/Client%20SDK%20Guide/#_5)

## UI
### WebView

#### 1. Browser Style WebView
기본으로 설정된 브라우저 스타일의 WebView를 노출합니다.
```java
Gamebase.WebView.showWebBrowser(activity, "http://cloud.toast.com");
```

#### 2. Popup Style WebView (지원예정)
기본으로 설정된 팝업 스타일의 WebView를 노출합니다.
```java
Gamebase.WebView.showWebPopup(activity, "http://cloud.toast.com");
```

#### 3. Custom WebView
Custom WebView를 노출합니다.
GamebaseWebViewConfiguration 설정으로 WebView를 Customizing 할 수 있습니다.
```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setStyle(GamebaseWebViewStyle.BROWSER)
            .setTitleText("title")                              // 웹뷰 타이틀 설정
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // 웹뷰 스크린 방향 설정
            .setNavigationBarColor(Color.RED)                   // 네비게이션바 색상 설정
            .setNavigationBarHeight(40)                         // 네비게이션바 높이 설정
            .setBackButtonVisible(true)                         // 백 버튼 활성화 여부 설정
            .setBackButtonImageResource(R.id.back_button)       // 백 버튼 이미지 설정
            .setCloseButtonImageResource(R.id.close_button)     // 닫기 버튼 이미지 설정
            .build();
GamebaseWebView.showWebView(MainActivity.this, "http://cloud.toast.com", configuration);
```
| Method | Values | Description |
| --- | --- | --- |
| setStyle(int style) | GamebaseWebViewStyle.BROWSER | 브라우저 스타일의 웹뷰 |
| | GamebaseWebViewStyle.POPUP | 팝업 스타일의 웹뷰 |
| setTitleText(String title) | title | 웹뷰의 타이틀 |
| setScreenOrientation(int orientation) | ScreenOrientation.PORTRAIT | 세로모드 |
| | ScreenOrientation.LANDSCAPE | 가로모드 |
| | ScreenOrientation.LANDSCAPE_REVERSE | 가로모드를 180도 회전 |
| setNavigationBarColor(int color) | Color.argb(a, r, b, b) | 네비게이션바 색상 |
| setBackButtonVisible(boolean visible) | true or false | 백 버튼 활성 or 비활성 |
| setNavigationBarHeight(int height) | height | 네비게이션바 높이 |
| setBackButtonImageResource(int resourceId) | ID of resource | 백 버튼 이미지 |
| setCloseButtonImageResource(int resourceId) | ID of resource | 닫기 버튼 이미지 |

### Alert
Android System Alert Dialog를 간단하게 노출 할 수 있는 API를 제공합니다.

#### 1. Simple Alert Dialog
타이틀과 메시지 입력만으로 간단하게 Alert Dialog를 노출할 수 있습니다.

```java
Gamebase.Util.showAlertDialog(activity, "title", "message");
```

#### 2. Alert Dialog with Listener
Alert Dialog 노출 후 처리 결과를 콜백 받고 싶을 경우 다음 API를 사용합니다.

```java
Gamebase.Util.showAlertDialog(activity,
                            "title",                        // 타이틀 텍스트.
                            "messsage",                     // 메시지 텍스트.
                            "OK",                           // 긍정 버튼 텍스트.
                            positiveButtonEventListener,    // 긍정 버튼이 눌러졌을 때 호출되는 Listener.
                            "Cancel",                       // 부정 버튼 텍스트.
                            negativeButtonEventListener,    // 부정 버튼이 눌러졌을 때 호출되는 Listener.
                            backKeyEventListener,           // Alert Dialog가 취소되면 호출되는 Listener.
                            true);                          // Alert Dialog를 취소할 수 있는지 여부를 설정.
```

### Toast
Android의 Toast를 간단하게 노출 할 수 있는 API를 제공합니다.

```java
Gamebase.Util.showToast(activity,
                        "message",              // 노출 할 메시지 텍스트
                        Toast.LENGTH_SHORT);    // 메시지를 표시하는 시간 (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

### Custom Maintenance Page
점검 상태에서 "자세히 보기" 클릭 시 노출되는 점검 페이지를 변경할 수 있습니다.

* AndroidManifest.xml 에 점검 페이지 등록
AndroidManifest.xml에 `"com.gamebase.maintenance.detail.url"`를 키 값으로 하는 meta-data를 설정합니다.
android:value의 값으로 .html 파일 또는 URL을 입력할 수 있습니다.

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error codes

| Category | Sub Category | Error | Error Code | Notes |
| -------- | ------------ | ----- | ---------- | ----- |
| Common |  | NOT_INITIALIZED | 1 | Gamebase 초기화가 되어있지 않습니다. |
|  |  | NOT_LOGGED_IN | 2 | 로그인이 필요합니다. |
|  |  | INVALID_PARAMETER | 3 | 잘못된 파라미터입니다. |
|  |  | INVALID_JSON_FORMAT | 4 | JSON 포맷 에러입니다. |
|  |  | USER_PERMISSION | 5 | 권한이 없습니다. |
|  |  | NOT_SUPPORTED | 10 | 지원하지 않는 기능입니다. |
| Network | Socket | SOCKET_RESPONSE_TIMEOUT | 101 | 네트워크 상태가 불안정하여 응답이 없습니다. |
|  |  | SOCKET_ERROR | 110 | 소켓 에러 |
|  |  | SOCKET_UNKNOWN_ERROR | 999 | 소켓 알 수 없는 에러 |
| Launching |  | LAUNCHING_SERVER_ERROR | 2001 | 런칭 서버 에러입니다. |
|  |  | LAUNCHING_NOT_EXIST_CLIENT_ID | 2002 | Client ID가 존재하지 않습니다. |
|  |  | LAUNCHING_UNREGISTERED_APP | 2003 | 등록되지 않은 App 입니다. |
|  |  | LAUNCHING_UNREGISTERED_CLIENT | 2004 | 등록되지 않은 Client (version) 입니다. |
| Auth | Common | AUTH_USER_CANCELED | 3001 | 로그인이 취소되었습니다. |
|  |  | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
|  |  | AUTH_NOT_EXIST_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
|  |  | AUTH_INVALID_MEMBER | 3004 | 잘못된 회원에 대한 요청입니다. |
|  |  | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 외부 인증 라이브러리 에러입니다. |
|  | Gamebase Login | AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  |  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  |  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IDP 정보가 없습니다. |
|  | IDP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IDP 로그인에 실패하였습니다. |
|  |  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
|  | Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
|  |  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
|  |  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IDP에 맵핑되어있습니다. |
|  |  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
|  | Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  |  | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | 마지막에 맵핑된 IDP는 삭제할 수 없습니다. |
|  |  | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | 현재 로그인되어있는 IDP 입니다. |
|  | Logout | AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
|  | Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | 탈퇴에 실패하였습니다. |
|  | Not Playable | AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
|  | Unknown | AUTH_UNKNOWN_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |
| Purchase |  | PURCHASE_NOT_INITIALIZED | 4001 | Gamebase PurchaseAdapter가 초기화되지 않았습니다. |
|  |  | PURCHASE_USER_CANCELED | 4002 | 구매가 취소되었습니다. |
|  |  | PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 이전 구매가 완료되지 않았습니다. |
|  |  | PURCHASE_NOT_ENOUGH_CASH | 4004 | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다. |
|  |  | PURCHASE_NOT_SUPPORTED_MARKET | 4010 | 지원하지 않는 스토어입니다. |
|  |  | PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | 외부 IAP 라이브러리 에러입니다. |
|  |  | PURCHASE_UNKNOWN_ERROR | 4999 | 알수없는 구매 에러입니다. (정의되지 않은 에러입니다.) |
| Push |  | PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | 외부 라이브러리 에러입니다. |
|  |  | PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 PUSH API 호출이 완료되지 않았습니다. |
|  |  | PUSH_UNKNOWN_ERROR | 5999 | 알수 없는 푸시 에러입니다. (정의되지 않은 푸시 에러입니다.) |
| UI |  | UI_UNKNOWN_ERROR | 6999 | 알수 없는 UI 관련 에러입니다. (정의되지 않은 에러입니다.) |
| Server |  | SERVER_INTERNAL_ERROR | 8001 |  |
|  |  | SERVER_REMOTE_SYSTEM_ERROR | 8002 |  |
|  |  | SERVER_UNKNOWN_ERROR | 8999 |  |

## Dependency

| Category | Provider | Modules | Dependencies | Description |
| -------- | -------- | ------- | ---------- | ----------- |
| **Gamebase<br>(Required)** | Gamebase | gamebase-sdk-{version}.aar<br>gamebase-sdk-base-{version}.aar | appcompat-v7-24.0.0.aar<br>support-v4-24.0.0.aar<br>support-annotations-24.0.0.jar<br>gson-2.2.4.jar<br>okhttp-3.6.0.jar<br>okio-1.11.0.jar |  |
| **Authentication<br>(Optional)** | Google | gamebase-adapter-auth-google-{version}.aar | play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>play-services-auth-10.0.1.aar<br>play-services-auth-base-10.0.1.aar |  |
|  | Facebook | gamebase-adapter-auth-facebook-{version}.aar | facebook-android-sdk-4.17.0.aar<br>appcompat-v7-24.0.0.aar<br>support-vector-drawable-24.0.0.aar<br>animated-vector-drawable-24.0.0.aar<br>cardview-v7-24.0.0.aar<br>customtabs-24.0.0.aar<br>bolts-android-1.4.0.jar<br>bolts-applinks-1.4.0.jar<br>bolts-tasks-1.4.0.jar |  |
|  | Payco | gamebase-adapter-auth-payco-{version}.aar | paycologin-1.2.9.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-tasks-10.0.1.aar<br>gson-2.2.4.jar |  |
| **Purchase<br>(Optional)** | IAP | gamebase-adapter-purchase-iap-{version}.aar | iap-1.3.2.20170424.aar<br>mobill-core-1.3.2.20170424.jar<br>gson-2.2.4.jar<br>okhttp-1.5.4.jar |  |
|  | IAP - ONE store |  | iap-tstore-1.3.2.20170424.aar<br>iap_tstore_plugin_v16.03.00_20161123.jar | ONE store 사용시 추가해야 합니다. |
| **Push<br>(Optional)** | FCM | gamebase-adapter-push-fcm-{version}.aar | pushsdk-release-v1.4.0.aar<br>firebase-common-10.0.1.jar<br>firebase-iid-10.0.1.jar<br>firebase-messaging-10.0.1.aar<br>play-services-base-10.0.1.aar<br>play-services-basement-10.0.1.aar<br>play-services-gcm-10.0.1.aar<br>play-services-iid-10.0.1.aar<br>play-services-tasks-10.0.1.aar |  |
|  | Tencent | gamebase-adapter-push-tencent-{version}.aar | pushsdk-release-v1.4.0.aar<br>Xg_sdk_v3.1_20170417_0946.jar<br>jg_filter_sdk_1.1.jar<br>mid-core-sdk-3.7.2.jar<br>wup-1.0.0.E-SNAPSHOT.jar |  |

* Required 항목은 필수로 포함되어야 하는 모듈입니다.
* Optional 항목은 해당 기능이 필요할 경우 포함되어야 하는 모듈입니다.
* 중복되는 Dependency 모듈은 하나만 포함하도록 해야합니다.

## 3rd-Party Provider SDK Guide
* Facebook Developers Guide : [Facebook for developers](https://developers.facebook.com/docs/android)
* Google Developers Guide [Google APIs for Android](https://developers.google.com/android/guides/overview)

## API Reference
SDK 내에 포함되어 있습니다.
