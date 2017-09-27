## Game > Gamebase > Android Developer's Guide  > Authentication

## Login

* Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
* guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음의 링크를 참고하시길 바랍니다.
* [LINK \[3rd-Party Provider SDK Guide\]](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

* 많은 게임이 타이틀 화면에서 로그인을 구현합니다.
	* 앱을 설치 후 처음 실행했다면 타이틀 화면에서 어떤 IDP로 인증할지 선택할 수 있도록 하여 유저가 선택한 IDP로 인증합니다.
	* 로그인에 한번 성공한 이후에는 IDP 선택화면을 표시하지 않고 이전에 로그인에 성공했던 IDP 타입으로 인증합니다.
* 위에서 설명한 로직을 다음과 같은 순서로 구현할 수 있습니다.

#### 1. 이전 로그인 타입 받아오기
* **Gamebase.getLastLoggedInProvider()**를 호출합니다.
* 리턴된 값이 존재한다면 **'2. 이전 로그인 타입으로 인증하기'**를 진행합니다.
* 리턴된 값이 없다면 유저에게 IDP를 선택하도록 한 다음 **'3. 지정된 IDP로 인증하기'**를 진행합니다.

#### 2. 이전 로그인 타입으로 인증하기

* 이전에 인증했던 기록이 있다면 ID/PW를 입력받지 않고 인증을 시도합니다.
* **Gamebase.loginForLastLoggedInProvider()**를 호출합니다.

#### 2-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.getUserID()**로 UserID를 획득하여 게임을 진행하세요.

#### 2-2. 인증이 실패한 경우

* 네트워크 에러
	* 에러코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.loginForLastLoggedInProvider()** 를 다시 호출 하거나, 잠시 대기했다가 재시도 하도록 합니다.
* 이용 정지 유저
	* 에러 코드가 **AUTH_BANNED_MEMBER(3005)** 인 경우, 이용 정지 유저이므로 인증이 실패한 것입니다.
	* **Gamebase.getAuthBanInfo()** 로 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 이유를 알려주시기 바랍니다.
	* Gamebase 초기화시 **GamebaseConfiguration.Builder.enablePopup(true)** 및 **enableBanPopup(true)**를 호출한다면 Gamebase 가 이용 정지에 관한 팝업을 자동으로 띄워줍니다.
* 그 외의 에러
	* 이전 로그인 타입으로 인증하기가 실패하였습니다. **'3. 지정된 IDP로 인증하기'**를 진행합니다.

#### 3. 지정된 IDP로 인증하기

* IDP 타입을 직접 지정하여 인증을 시도합니다.
	* 인증 가능한 타입은 **AuthProvider** 클래스에 선언되어 있습니다.
* **Gamebase.login(activity, idpType, callback)** API를 호출합니다.

#### 3-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공하였습니다.
* **Gamebase.getUserID()** 로 UserID를 획득하여 게임을 진행하세요.

#### 3-2. 인증이 실패한 경우

* 네트워크 에러
	* 에러코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.login(activity, idpType, callback)** 를 다시 호출 하거나, 잠시 대기했다가 재시도 하도록 합니다.
* 이용 정지 유저
	* 에러 코드가 **AUTH_BANNED_MEMBER(3005)** 인 경우, 이용 정지 유저이므로 인증이 실패한 것입니다.
	* **Gamebase.getAuthBanInfo()** 로 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 이유를 알려주시기 바랍니다.
	* Gamebase 초기화시 **GamebaseConfiguration.Builder.enablePopup(true)** 및 **enableBanPopup(true)** 를 호출한다면 Gamebase 가 이용 정지에 관한 팝업을 자동으로 띄워줍니다.
* 그 외의 에러
	* 에러가 발생했다는 것을 유저에게 알리고, 유저가 인증 IDP 타입을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다. <br/>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때 실패를 리턴합니다. <br/>
이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.

```java
Gamebase.loginForLastLoggedInProvider(activity, new GamebaseDataCallback<AuthToken>() {
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

```
### Login with GUEST

* Gamebase는 Guest 로그인을 지원합니다.
* 디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
* Guest 로그인은 앱 삭제 또는 디바이스 초기화 시에 계정이 삭제될 수 있으므로 IDP를 활용한 로그인 방식을 권장합니다.
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


### Login with IDP

특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.<br/>
로그인 가능한 IDP 타입은 **AuthProvider**클래스에서 확인할 수 있습니다.

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

### Login with Credential

게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증을 하고 발급받은 AccessToken등을 이용하여, Gamebase 로그인을 할 수 있는 인터페이스 입니다.


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

### Authentication Additional Information Settings

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

로그인 된 IDP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에서 로그아웃 버튼을 두고 클릭시 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 유저 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IDP로 인증했던 기록을 제거하므로 다음 로그인시 ID/PW 입력창이 노출됩니다.<br/><br/>

로그아웃 버튼을 클릭했을 때, 다음과 같이 로그아웃 API를 구현합니다.

```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(activity, new GamebaseCallback() {
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

로그인 상태에서 탈퇴를 시도합니다.<br/><br/>

* 탈퇴에 성공하면, 로그인 했던 IDP와 연동 되어 있던 유저 데이터는 삭제 됩니다.
* 해당 IDP로 다시 로그인 가능하고 새로운 유저 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IDP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IDP 로그아웃을 시도하게 됩니다.

탈퇴 버튼을 클릭했을 때, 다음과 같이 탈퇴 API를 구현합니다.

```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(activity, new GamebaseCallback() {
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

많은 게임들이 하나의 계정에 여러 IDP를 연동(Mapping)할 수 있도록 하고 있습니다.<br/>
Gamebase의 Mapping API를 사용하여 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시킬 수 있습니다.<br/><br/>

이렇게 하나의 Gamebase UserID에 다양한 IDP 계정을 연동할 수 있습니다.<br/>
즉, 연동 중인 IDP 계정으로 로그인을 시도 한다면 항상 동일한 UserID로 로그인 됩니다.<br/><br/>

주의할 점은, IDP 마다 하나의 계정씩만 연동이 가능합니다.<br/>
예시는 다음과 같습니다.<br/><br/>

* Gamebase UserID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* Gamebase UserID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> 이미 Google ee 계정이 연동중이므로 Google계정을 추가로 연동할 수 없습니다.**

Mapping 에는 Mapping 추가/해제 API 2개가 있습니다.

### Add Mapping

특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.<br/>
Mapping을 하려는 IDP의 계정이 이미 다른 계정에 연동이 되어있다면<br/>
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니다.<br/><br/>

Mapping이 성공 하더라도 '현재 로그인 중인 IDP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인 한 후, Facebook 계정 Mapping 시도가 성공했다고 해서 '현재 로그인 중인 IDP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.<br/>
Mapping은 단순히 IDP 연동만 추가 해줍니다.<br/><br/>

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

### Remove Mapping

특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다.<br/>
연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

```java
private static void removeMappingForFacebook(final Activity activity) {
        Gamebase.removeMapping(activity, AuthProvider.FACEBOOK, new GamebaseCallback() {
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


## Gamebase User`s Informations
Gamebase를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.


### Get Authentication Information for Gamebase
Gamebase에서 발급한 인증 정보를 가져올 수 있습니다.

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();

// Obtaining Ban Information
AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
```


### Get Authentication Information for External IDP

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

### Get Banned User Information

Gamebase Console에 제재된 유저로 등록될 경우,
로그인 시도 시, 아래와 같은 이용제한 정보 코드가 노출 될 수 있으며, **Gamebase.getAuthBanInfo()** 메서드를 이용하여 제재 정보를 확인할 수 있습니다.

* AUTH_BANNED_MEMBER(3005)

## Error Handling

| Category |  Error | Error Code | Notes |
| -------- | ------ | ---------- | ----- |
| Auth | AUTH\_USER\_CANCELED | 3001 | 로그인이 취소되었습니다. |
|  | AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002 | 지원하지 않는 인증 방식입니다. |
|  | AUTH\_NOT\_EXIST\_MEMBER | 3003 | 존재하지 않거나 탈퇴한 회원입니다. |
|  | AUTH\_INVALID\_MEMBER | 3004 | 잘못된 회원에 대한 요청입니다. |
|  | AUTH\_BANNED\_MEMBER | 3005 | 제재된 회원입니다. |
|  | AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009 | 외부 인증 라이브러리 에러입니다. |
| Auth (Login) | AUTH\_TOKEN\_LOGIN\_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103 | 최근에 로그인한 IDP 정보가 없습니다. |
| IDP Login | AUTH\_IDP\_LOGIN\_FAILED | 3201 | IDP 로그인에 실패하였습니다. |
|  | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
| Add Mapping | AUTH\_ADD\_MAPPING\_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
|  | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
|  | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303 | 이미 같은 IDP에 맵핑되어있습니다. |
|  | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304 | IDP 정보가 유효하지 않습니다. (Console에 해당 IDP 정보가 없습니다.) |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IDP는 삭제할 수 없습니다. |
|  | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403 | 현재 로그인되어있는 IDP 입니다. |
| Logout | AUTH\_LOGOUT\_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| Withdrawal | AUTH\_WITHDRAW\_FAILED | 3601 | 탈퇴에 실패하였습니다. |
| Not Playable | AUTH\_NOT\_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| Auth(Unknown) | AUTH\_UNKNOWN\_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |

* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 에러는 TOAST Cloud 외부 인증 라이브러리에서 발생한 에러입니다.
