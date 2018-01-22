## Game > Gamebase > Android SDK 사용 가이드 > 인증

## Login

Gamebase에서는 게스트 로그인을 기본으로 지원합니다.

* 게스트 이외의 Provider에 로그인하려면 해당 Provider AuthAdapter가 필요합니다.
* AuthAdapter 및 3rd-Party Provider SDK에 대한 설정은 다음을 참고하시기 바랍니다.
  [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

많은 게임이 타이틀 화면에 로그인을 구현합니다.
* 앱을 설치하고 처음 실행했을 때 타이틀 화면에서 게임 이용자가 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.
* 한 번 로그인한 후에는 IdP 선택 화면을 표시하지 않고 이전에 로그인한 IdP 유형으로 인증합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.

#### 1. 이전 로그인 유형 받아오기
* **Gamebase.getLastLoggedInProvider()**를 호출합니다.
* 반환된 값이 있으면 **2. 이전 로그인 유형으로 인증**를 진행합니다.
* 반환된 값이 없다면 게임 이용자에게 IdP를 선택하게 한 다음 **3. 지정된 IdP로 인증**을 진행합니다.

#### 2. 이전 로그인 유형으로 인증

* 이전에 인증했던 기록이 있다면 ID와 비밀번호를 입력받지 않고 인증을 시도합니다.
* **Gamebase.loginForLastLoggedInProvider()**를 호출합니다.

#### 2-1. 인증이 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **Gamebase.getUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 2-2. 인증이 실패한 경우

* 네트워크 오류
    * 오류 코드 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.loginForLastLoggedInProvider()**를 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 이용자
    * 오류 코드가 **AUTH_BANNED_MEMBER(3005)**인 경우, 이용 정지 게임 이용자이므로 인증에 실패한 것입니다.
    * **Gamebase.getAuthBanInfo()**로 제재 정보를 확인하여 게임 이용자에게 게임을 할 수 없는 이유를 알려 주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.Builder.enablePopup(true)** 및 **enableBanPopup(true)**를 호출한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 이전 로그인 유형으로 인증에 실패했기 때문에 **3. 지정된 IdP로 인증**을 진행하시기 바랍니다.

#### 3. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 **AuthProvider** 클래스에 선언돼 있습니다.
* **Gamebase.login(activity, idpType, callback)** API를 호출합니다.

#### 3-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **Gamebase.getUserID()**로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 3-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **Gamebase.login(activity, idpType, callback)**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 사용자
    * 오류 코드가 **AUTH_BANNED_MEMBER(3005)**인 경우, 이용 정지 게임 이용자이므로 인증에 실패한 것입니다.
    * **Gamebase.getAuthBanInfo()**로 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 이유를 알려주시기 바랍니다.
    * Gamebase 초기화 시 **GamebaseConfiguration.Builder.enablePopup(true)** 및 **enableBanPopup(true)**를 호출한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 오류가 발생했다는 것을 게임 이용자에게 알리고, 게임 이용자가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login as the Latest Login IdP

가장 최근에 로그인한 IdP로 로그인을 시도합니다. <br/>
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하면 실패를 반환합니다. <br/>
이때는 해당 IdP에 대한 로그인을 구현해야 합니다.

```java
private static void onLogin(final Activity activity) {
    if (!TextUtils.isEmpty(Gamebase.getLastLoggedInProvider())) {
        onLoginForLastLoggedInProvider(activity);
    } else {
        // 이전에 로그인한 유형이 존재하지 않는다면 지정된 IDP로 인증하기를 시도합니다.
        Gamebase.login(activity, provider, logincallback);
    }
}

private static void onLoginForLastLoggedInProvider(final Activity activity) {
    Gamebase.loginForLastLoggedInProvider(activity, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 로그인 성공
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForLastLoggedInProvider(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // 로그인을 시도한 게임 이용자가 이용 정지 상태입니다.
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) 를 호출하였다면
                    // Gamebase가 이용정지에 관한 팝업을 자동으로 띄워줍니다.
                    //
                    // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 Gamebase.getAuthBanInfo()로
                    // 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 사유를 표시해 주시기 바랍니다.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // 그 외의 오류가 발생하는 경우 지정된 IdP로 인증을 시도합니다.
                    Gamebase.login(activity, provider, logincallback);
                }
            }
        }
    });
}
```
### Login with GUEST

Gamebase는 게스트 로그인을 지원합니다.

* 디바이스의 유일한 키를 생성하여 Gamebase에 로그인을 시도합니다.
* 게스트 로그인은 앱 삭제 또는 디바이스 초기화 시에 계정이 삭제될 수 있으므로 IdP를 활용한 로그인 방식을 권장합니다.

게스트 로그인을 구현하는 방법은 아래 예시 코드를 참고하세요.

```java
private static void onLoginForGuest(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GUEST, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 로그인 성공
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGuest(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // 로그인을 시도한 게임 이용자가 이용 정지 상태입니다.
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) 를 호출하였다면
                    // Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
                    //
                    // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 Gamebase.getAuthBanInfo()로
                    // 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 사유를 표시해 주시기 바랍니다.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // 로그인 실패
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


### Login with IdP

다음은 특정 IdP로 로그인할 수 있게 하는 예시 코드입니다.<br/>
로그인할 수 있는 IdP 유형은 **AuthProvider **클래스에서 확인할 수 있습니다.

```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 로그인 성공
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGoogle(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // 로그인을 시도한 유저가 이용정지 상태입니다.
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) 를 호출하였다면
                    // Gamebase가 이용정지에 관한 팝업을 자동으로 띄워줍니다.
                    //
                    // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 Gamebase.getAuthBanInfo()로
                    // 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 사유를 표시해 주시기 바랍니다.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // 로그인 실패
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Login with Credential

IdP에서 제공하는 SDK를 사용해 게임에서 직접 인증한 후 발급받은 액세스 토큰 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

* Credential 파라미터 설정 방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdP 유형 설정                                | AuthProvider.GOOGLE, AuthProvider.FACEBOOK, AuthProvider.PAYCO |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정<br/>Google 인증 시에는 사용 안 함 |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Google 로그인 이후 획득할 수 있는 OTAC(one time authorization code) 입력 |                                          |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

```java
private static void onLoginWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.login(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 로그인 성공
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // 로그인을 시도한 게임 이용자가 이용 정지 상태입니다.
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true) 를 호출하였다면
                    // Gamebase가 이용정지에 관한 팝업을 자동으로 띄워줍니다.
                    //
                    // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 Gamebase.getAuthBanInfo()로
                    // 제재 정보를 확인하여 사용자에게 게임을 플레이할 수 없는 사유를 표시해 주시기 바랍니다.
                    AuthBanInfo authBanInfo = Gamebase.getAuthBanInfo();
                } else {
                    // 로그인 실패
                    Log.e(TAG, "Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Authentication Additional Information Settings

#### Facebook
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
    * Facebook의 경우, OAuth 인증 시도 시, Facebook에 요청할 정보의 종류를 설정해야 합니다.

Facebook 인증 추가 정보 입력 예제

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"]}
```

#### PAYCO
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
    * PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**의 설정이 필요합니다.

PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

## Logout

로그인된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 이용자 데이터는 유지됩니다.
로그아웃에 성공하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 표시됩니다.<br/><br/>

다음은 로그아웃 버튼을 클릭하면 로그아웃이 되는 예시 코드입니다.

```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// 로그아웃 성공
                Log.d(TAG, "Logout successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLogout(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // 로그아웃 실패
                    Log.e(TAG, "Logout failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

## Withdraw

다음은 로그인 상태에서 게임 이용자 탈퇴를 구현하는 예시 코드입니다.<br/><br/>

* 탈퇴에 성공하면, 로그인했던 IdP와 연동되어 있던 게임 이용자 데이터는 삭제됩니다.
* 해당 IdP로 다시 로그인할 수 있으며, 새 게임 이용자 데이터를 생성합니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.
* 탈퇴 성공 시 IdP 로그아웃을 시도하게 됩니다.

> <font color="red">[주의]</font><br/>
>
> 여러 IdP를 연동 중인 경우 모든 IdP 연동이 해제되고 Gamebase 게임 이용자 데이터가 삭제됩니다.
>

```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// 탈퇴 성공
                Log.d(TAG, "Withdraw successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onWithdraw(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // 탈퇴 실패
                    Log.e(TAG, "Withdraw failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

## Mapping

매핑은 기존에 로그인된 계정에 다른 IdP의 계정을 연동하거나 해제시키는 기능입니다.

대다수의 게임에서는 게임 이용자 계정 하나에 여러 IdP를 연동(매핑)할 수 있습니다.<br/>Gamebase의 매핑 API를 사용하면 기존에 로그인된 계정에 다른 IdP 계정을 연동하거나 해제할 수 있습니다.

즉, 연동 중인 IdP 계정으로 로그인을 시도하면 항상 같은 사용자 ID로 로그인됩니다.<br/><br/>

주의할 점은, IdP마다 하나의 계정만 연동할 수 있다는 점입니다.<br/>
예를 들어 Google 계정을 연동 중이면, 다른 Google 계정을 추가로 연동할 수 없습니다.<br/>
계정 연동 예시는 다음과 같습니다.<br/><br/>

* Gamebase 사용자 ID: 123bcabca
    * Google ID: aa
    * Facebook ID: bb
    * Apple Game Center ID: cc
    * Payco ID: dd
* Gamebase 사용자 ID : 456abcabc
    * Google ID: ee
    * Google ID: ff **-> 이미 Google ee 계정이 연동 중이므로 Google 계정을 추가로 연동할 수 없습니다.**

매핑 API에는 매핑 추가와 매핑 해제 API가 있습니다.

### Add Mapping Flow

매핑은 다음 순서로 구현할 수 있습니다.

#### 1. 로그인
매핑은 현재 계정에 IdP 계정 연동을 추가하는 것이므로 우선 로그인이 돼 있어야 합니다.
먼저 로그인 API를 호출해 로그인합니다.

#### 2. 매핑

**Gamebase.addMapping(activity, idpType, callback)**을 호출해 매핑을 시도합니다.

#### 2-1. 매핑이 성공한 경우

* 축하합니다! 현재 계정과 연동중인 IdP 계정이 추가되었습니다.
* 매핑에 성공해도 '현재 로그인 중인 IdP'가 바뀌지는 않습니다. 즉, Google 계정으로 로그인한 후, Facebook 계정 매핑 시도가 성공했다고 해서 '현재 로그인 중인 IdP'가 Google에서 Facebook으로 변경되지는 않습니다. Google 상태로 유지됩니다.
* 매핑은 단순히 IdP 연동만 추가해 줍니다.

#### 2-2. 매핑이 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)**인 경우, 일시적인 네트워크 문제로 인증이 실패한 것이므로 **Gamebase.addMapping()**을 다시 호출하거나, 잠시 대기했다가 재시도 합니다.
* 이미 다른 계정에 연동 중일 때 발생하는 오류
    * 오류 코드가 **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**인 경우, 매핑하려는 IdP의 계정이 이미 다른 계정에 연동 중이라는 뜻입니다. 이미 연동된 계정을 해제하려면 해당 계정으로 로그인하여 **Gamebase.withdraw()**를 호출하여 탈퇴하거나 **Gamebase.removeMapping()**를 호출하여 연동을 해제한 후 다시 매핑을 시도하세요.
* 이미 동일한 IdP 계정에 연동돼 발생하는 오류
    * 에러 코드가 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)** 인 경우, 매핑하려는 IdP와 같은 종류의 계정이 이미 연동중이라는 뜻입니다.
        * Gamebase 매핑은 한 IdP당 하나의 계정만 연동 가능합니다. 예를 들어 PAYCO 계정에 이미 연동 중이라면 더 이상 PAYCO 계정을 추가할 수 없습니다.
        * 동일 IdP의 다른 계정을 연동하기 위해서는 **Gamebase.removeMapping()**을 호출해 연동을 해제한 후 다시 매핑을 시도하세요.
* 그 외의 오류
    * 매핑 시도가 실패했습니다.

### Add Mapping

특정 IdP에 로그인된 상태에서 다른 IdP로 매핑을 시도합니다.<br/>

다음은 Facebook에 매핑을 시도하는 예시입니다.

```java
private static void addMappingForFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 매핑 추가 성공
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // Mapping을 시도하는 IDP계정이 이미 다른 계정에 연동되어 있습니다.
                    // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 해야 합니다.
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // Mapping을 시도하는 IDP의 계정이 이미 추가되어 있습니다.
                    // Gamebase Mapping은 한 IDP당 하나의 계정만 연동 가능합니다.
                    // IDP계정을 변경하려면 이미 연동중인 계정은 Mapping 해제를 해야 합니다.
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // 매핑 추가 실패
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Add Mapping with Credential

게임에서 직접 ID Provider에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMapping을 할 수 있는 인터페이스입니다.

* Credential 파라미터 설정방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IDP 유형 설정                                | AuthProvider.GOOGLE, AuthProvider.FACEBOOK, AuthProvider.PAYCO |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰)설정.<br/>Google 인증 시에는 사용 안 함. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Google 로그인 이후 획득할 수 있는 OTOC(one time authorization code) 입력 |                                          |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원요구하는 개발사항은 외부SDK의 API를 사용하여 구현해야하며, Gamebase에서는 지원하지 않습니다.
>

```java
private static void addMappingWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 매핑 추가 성공
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // Mapping을 시도하는 IDP계정이 이미 다른 계정에 연동되어 있습니다.
                    // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 해야 합니다.
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // Mapping을 시도하는 IDP의 계정이 이미 추가되어 있습니다.
                    // Gamebase Mapping은 한 IDP당 하나의 계정만 연동 가능합니다.
                    // IDP계정을 변경하려면 이미 연동중인 계정은 Mapping 해제를 해야 합니다.
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // 매핑 추가 실패
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```

### Remove Mapping

특정 IDP에 대한 연동을 해제합니다. 만약, 현재 로그인 중인 계정을 해제하려고 하면 실패를 반환합니다.<br/>
연동 해제 후에는 Gamebase 내부에서, 해당 IdP에 대한 로그아웃을 처리합니다.

```java
private static void removeMappingForFacebook(final Activity activity) {
    Gamebase.removeMapping(activity, AuthProvider.FACEBOOK, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // 매핑 해제 성공
                Log.d(TAG, "Remove mapping successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                    // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                removeMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_REMOVE_MAPPING_LOGGED_IN_IDP) {
                    // 로그인중인 계정으로는 Mapping 해제를 할 수 없습니다.
                    // 다른 계정으로 로그인 하여 Mapping 해제 하거나 탈퇴하여야 합니다.
                    Log.e(TAG, "Remove Mapping failed- LOGGED_IN_IDP");
                } else {
                    // 매핑 해제 실패
                    Log.e(TAG, "Remove mapping failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                }
            }
        }
    });
}
```


## Gamebase User`s Information
Gamebase로 인증 절차를 진행한 후, 앱을 제작할 때 필요한 정보를 얻을 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> "Gamebase.loginForLastLoggedInProvider()" API 로 로그인한 경우에는 인증 정보를 가져올 수 없습니다.
>
> 인증 정보가 필요하다면 "Gamebase.loginForLastLoggedInProvider()" 대신, 사용하고자 하는 IDPCode 와 동일한 {IDP_CODE} 를 파라미터로 하여 "Gamebase.login(activity, IDP_CODE, callback)" API 로 로그인 해야 정상적으로 인증정보를 획득할 수 있습니다.

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

외부 인증 SDK에서 액세스 토큰, 사용자 ID, Profile 등의 정보를 가져올 수 있습니다.

```java
// 유저 ID를 가져옵니다.
String userId = getAuthProviderUserID(AuthProvider.FACEBOOK);
// 액세스 토큰을 가져옵니다.
String accessToken = getAuthProviderAccessToken(AuthProvider.FACEBOOK);
// User Profile 정보를 가져옵니다.
AuthFacebookProfile profile = (AuthFacebookProfile) getAuthProviderProfile(AuthProvider.FACEBOOK);
String name = profile.getName();    // or profile.information.get("name")
String email = profile.getEmail();  // or profile.information.get("email")
```

### Get Banned User Information

Gamebase Console에 제재된 게임 이용자로 등록될 경우,
로그인을 시도하면 아래와 같은 이용 제한 정보 코드가 표시될 수 있습니다. **Gamebase.getAuthBanInfo()** 메서드를 이용해 제재 정보를 확인할 수 있습니다.

* AUTH_BANNED_MEMBER(3005)

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | 로그인이 취소되었습니다.                            |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                | AUTH\_INVALID\_MEMBER                    | 3004       | 잘못된 회원에 대한 요청입니다.                        |
|                | AUTH\_BANNED\_MEMBER                     | 3005       | 제재된 회원입니다.                               |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 외부 인증 라이브러리 오류입니다.                       |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       | 토큰 로그인에 실패했습니다.                          |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 토큰 정보가 유효하지 않습니다.                        |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                   |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdP 로그인에 실패했습니다.                         |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | 매핑 추가에 실패했습니다.                           |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 이미 다른 멤버에 매핑돼 있습니다.                      |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 이미 같은 IdP에 매핑돼 있습니다.                     |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | 매핑 삭제에 실패했습니다.                           |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 마지막에 매핑된 IdP는 삭제할 수 없습니다.                |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | 현재 로그인되어 있는 IdP입니다.                      |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | 로그아웃에 실패했습니다.                            |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 탈퇴에 실패했습니다.                              |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | 플레이할 수 없는 상태입니다(점검 또는 서비스 종료 등).         |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | 알수 없는 오류입니다.(정의되지 않은 오류입니다).             |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 TOAST 외부 인증 라이브러리에서 발생한 오류입니다.
