## Upcoming Products > Gamebase > Android Developer's Guide  > Authentication

## Login

* Gamebase 에서는 기본적으로 guest 로그인을 지원합니다.
* guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음의 링크를 참고하시길 바랍니다.
* [LINK \[3rd-Party Provider SDK Guide\]](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

* 앱에서의 Gamebase인증상태에 따른 처리 방법을 설명합니다.

* 앱을 처음 설치했을 때, 타이틀 화면에서 로그인을 시도할 경우 로그인 정보(Gamebase AccessToken)가 존재하지 않기 때문에, ID/PW등을 입력하여, 로그인 할 수 있도록 합니다.
	1. 타이틀 화면 등에서는 명확하게 로그인이 되지 않은 상태로 판단하여, **IDP login API**를 호출하여 로그인을 시도합니다.
	2. 로그인 성공 시에는 게임을 진행할 수 있도록 합니다.
	3. 로그인이 실패는 시에는 **IDP login API**을 다시 호출시도할 수 있도록 합니다.
		* 로그인 실패 사유가 **AUTH_BANNED_MEMBER(3005)** 와 같은 경우라면 로그인이 항상 실패할 것이기 때문에 적절한 안내와 함께 게임 진입이되지 않도록 처리합니다.

* 앱을 처음 실행하는 것이 아니라서, 로그인 정보(Gamebase AccessToken)가 남아있을 경우
	1. 앱을 background에서 foreground로 전환할 때와 같이 local에 로그인 정보가 남아 있을 경우, **loginForLastLoggedInProvider**를 호출하여, ID/PW를 입력 받지 않고 로그인을 시도합니다.
	2. 로그인 성공 시에는 게임을 진행할 수 있도록 합니다.
	3. 로그인 실패 시에는, 에러별로 다른 처리가 필요합니다.
		* 로그인 실패 사유가 Network 오류일 경우: **loginForLastLoggedInProvider**를 재시도 하도록 합니다.
			* 네트워크 에러 : **SOCKET_ERROR(110)**, **SOCKET_RESPONSE_TIMEOUT(101)**
		* 로그인 실패 사유가 서버 오류일 경우: 기존의 로그인 정보가 인증을 받을 수 없는 상태이기 때문에 **IDP login API**을 다시 호출할 수 있도록 합니다. (Title Scene으로의 화면 전환 등)
    	* 로그인 실패 사유가 **AUTH_BANNED_MEMBER(3005)** 와 같은 경우라면 로그인이 항상 실패할 것이기 때문에 적절한 안내와 함께 게임 진입이 되지 않도록 처리합니다.
		

### Banned User of Login

이용정지 회원일 경우 loginForLastLoggedInProvider/Login API를 호출하면 AUTH_BANNED_MEMBER(3005) 에러를 리턴합니다.
GetBanInfo API로 ban정보를 가져올 수 있습니다.


### Login as the Latest Login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다. <br/>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때 실패를 리턴합니다. <br/>
이 때는 해당 IDP에 대한 로그인을 구현해주어야합니다.

```java
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

로그인 된 IDP에서 로그아웃을 시도합니다.
로그아웃이 성공하더라도, 유저 데이터는 유지됩니다.
로그아웃에 성공 하면 해당 IDP 로그아웃을 시도하게 됩니다.

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

로그인 상태에서 탈퇴를 시도합니다.<br/><br/>

* 탈퇴에 성공하면, 로그인 했던 IDP와 연동 되어 있던 유저 데이터는 삭제 됩니다.
* 해당 IDP로 다시 로그인 가능하고 새로운 유저 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IDP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IDP 로그아웃을 시도하게 됩니다.

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

Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다.<br/>
특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때 각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

### Add Mapping

특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다.<br/>
Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면 
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)** 에러를 리턴합니다.

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다. 즉, Mapping은 단순히 IDP를 연동만 해줍니다.

<br/>
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

특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다. 
연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.
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
| -------- | ----- | ---------- | ----- |
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


