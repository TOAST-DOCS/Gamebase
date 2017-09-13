## Game > Gamebase > Developer's Guide (Android)  > Login

## Login

* Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
* guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음의 링크를 참고하시길 바랍니다.
* [LINK \[3rd-Party Provider SDK Guide\]](aos-started#3rd-party-provider-sdk-guide)

### 1. Login as the latest login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다. <br/>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때 실패를 리턴합니다. 이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.

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

* Login을 호출한 Activity의 Activity#onActivityResult(int, int, Intent)에서 Gamebase.onActivityResult(int, int, Intent)을 호출합니다.
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
