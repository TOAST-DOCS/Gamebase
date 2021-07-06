## Game > Gamebase > JavaScript SDK 사용 가이드 > 인증

## Login
Gamebase에서는 게스트 로그인을 기본으로 지원합니다.
다른 IdP(Identity Provider, ex) google, facebook, payco, line, naver 등)를 사용하시려면 "Login with IdP" 섹션을 참고하시기 바랍니다.


### Login Flow
많은 게임이 타이틀 화면에 로그인을 구현합니다.

* 게임 초기 타이틀 화면에서 이용자에게 어떤 IdP(identity provider)로 인증할지 선택할 수 있게 합니다.

위에서 설명한 로직은 다음과 같은 순서로 구현할 수 있습니다.
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_2.6.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)

#### 1. 지정된 IdP로 인증

* IdP 유형을 직접 지정하여 인증을 시도합니다.
    * 인증 가능한 유형은 아래와 같습니다.
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * Payco('payco')
        * Naver('naver')
        * Line('line')
* **toast.Gamebase.login(providerName, (authToken, error) => { ... })** API를 호출합니다.

#### 1-1. 인증에 성공한 경우

* 축하합니다! 인증에 성공했습니다.
* **toast.Gamebase.getUserID()** 로 사용자 ID를 획득하여 게임 로직을 구현하시면 됩니다.

#### 1-2. 인증에 실패한 경우

* 네트워크 오류
    * 오류 코드가 **SOCKET_ERROR(110)** 또는 **SOCKET_RESPONSE_TIMEOUT(101)** 인 경우, 일시적인 네트워크 문제로 인증에 실패한 것이므로 **toast.Gamebase.login(providerName, callback)**을 다시 호출하거나, 잠시 후 다시 시도합니다.
* 이용 정지 게임 사용자
    * 오류 코드가 **BANNED_MEMBER(7)** 인 경우, 이용 정지 게임 이용자이므로 인증에 실패한 것입니다.
    * **toast.Gamebase.getBanInfo()** 로 제재 정보를 확인하여 게임 이용자에게 게임을 플레이할 수 없는 이유를 알려주시기 바랍니다.
    * Gamebase 초기화 시 **enablePopup** 을 true로 설정한다면 Gamebase가 이용 정지에 관한 팝업을 자동으로 띄웁니다.
* 그 외 오류
    * 오류가 발생했다는 것을 게임 이용자에게 알리고, 게임 이용자가 인증 IdP 유형을 선택할 수 있는 상태(주로 타이틀 화면 또는 로그인 화면)로 되돌아갑니다.

### Login with IdP

다음은 특정 IdP로 로그인할 수 있게 하는 예시 코드입니다.

로그인할 수 있는 IdP 유형은 아래와 같습니다.

* Guest('guest')
* Google('google')
* Facebook('facebook')
* Payco('payco')
* Naver('naver')
* Line('line')

> <font color="red">[주의]</font><br/>
>
> Gamebase에서는 인증을 위해 "팝업"을 이용합니다. 만약 사용자의 "팝업"이 허용되어있지 않는다면, "로그인 시도" 중인 상태로 계속 대기할 수 있습니다.
> 사전에 사용자에게 인증 전 "팝업" 허용을 해달라는 문구를 노출하여 사용자가 불편함을 느끼지 않도록 해야합니다.
>
> 페이스북의 접속 가능 브라우저 정책 변경에 따라 
> IE 브라우저에서 페이스북으로 로그인시 Edge 브라우저로 강제 전환이 되고 있습니다.
> 따라서, 페이스북으로 로그인을 하시는 경우 크롬, Edge 브라우저를 이용하여 접속해 주셔야 합니다.
> 다만, 부득이하게 IE 브라우저에서 페이스북 로그인을 하셔야 한다면
> 아래와 같은 방법으로 Edge 브라우저로 강제 전환을 막아 이용하실 수는 있으니 참조하시기 바랍니다.
> 1. Edge 브라우저의 설정에 접속합니다.
> 2. 설정 화면 메뉴 중 '기본 브라우저'를 선택합니다.
> 3. Internet Explorer 호환성을 '안함'으로 변경합니다.

```js
toast.Gamebase.login(providerName, (authToken, error) => { ... });
```

**Example**
```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // 로그인을 시도한 유저가 이용정지 상태입니다.
                // GamebaseConfiguration.uiConfiguration.enablePopup(true) 및
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true) 로 초기화를 하였다면,
                // Gamebase가 이용정지에 관한 UI를 자동으로 띄워줍니다.

                // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 toast.Gamebase.getBanInfo()로
                // 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 사유를 표시해 주시기 바랍니다.
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // 로그인 실패
            }

            return;
        }

        // 로그인 성공
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

### Login with Credential

IdP에서 제공하는 SDK, RestAPI 등을 사용해 게임에서 직접 인증한 후 발급받은 액세스 토큰 등을 이용하여, Gamebase에 로그인할 수 있는 인터페이스입니다.

**Credential 파라미터 설정 방법**

| key name             | a use                                                               |
| -------------------- | ------------------------------------------------------------------- |
| providerName         | IdP 유형 설정                                                        |
| accessToken          | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정                          |
| accessTokenSecret    | IdP 로그인 이후 받은 인증 정보(액세스 토큰 시크릿) 설정                    |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

```js
var credential = {
    providerName: ${IdP},
    acessToken: ${IdP AccessToken},
    accessTokenSecret: ${IdP AccessTokenSecret}, // This is only nessassary in the case that IdP give you this value.
};

toast.Gamebase.loginWithCredential(credential, callback);
```

**Example**

```js
function loginWithFacebookCredential() {
    var credential = {
        providerName: 'facebook',
        accessToken: 'EAAL24qLuXHwBAFL1hS72BzZCUrJyDMRxUq1H5ZBGFIT..........',
    };

    toast.Gamebase.loginWithCredential(credential, function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Socket error 로 일시적인 네트워크 접속 불가 상태임을 의미합니다.
                // 네트워크 상태를 확인하거나 잠시 대기 후 재시도 하세요.
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // 로그인을 시도한 유저가 이용정지 상태입니다.
                // GamebaseConfiguration.uiConfiguration.enablePopup(true) 및
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true) 로 초기화를 하였다면,
                // Gamebase가 이용정지에 관한 UI를 자동으로 띄워줍니다.

                // Game UI에 맞게 직접 이용정지 팝업을 구현하고자 한다면 Gamebase.getBanInfo()로
                // 제재 정보를 확인하여 유저에게 게임을 플레이 할 수 없는 사유를 표시해 주시기 바랍니다.
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // 로그인 실패
            }

            return;
        }

        // 로그인 성공
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

## Logout

로그인된 IdP에서 로그아웃을 시도합니다. 주로 게임의 설정 화면에 로그아웃 버튼을 두고, 버튼을 클릭하면 실행되도록 구현하는 경우가 많습니다.
로그아웃이 성공하더라도, 게임 이용자 데이터는 유지됩니다.
로그아웃에 성공하면 해당 IdP로 인증했던 기록을 제거하므로 다음에 로그인할 때 ID, 비밀번호 입력 창이 표시됩니다.<br/><br/>

다음은 로그아웃 버튼을 클릭하면 로그아웃이 되는 예시 코드입니다.

```js
toast.Gamebase.logout((error) => { ... })
```

**Example**
```js
function logout() {
    toast.Gamebase.logout(function (error) {
        if (error) {
            // 로그아웃 실패
            console.log(error);
            return;
        }

        // 로그아웃 성공
        console.log(data);
    });
}
```

## Withdraw

로그인 상태에서 탈퇴를 시도합니다.

* 탈퇴 성공 시
  * 로그인했던 IdP의 게임 이용자 데이터는 삭제됩니다.
  * 해당 IdP로 다시 로그인할 수 있으며, 새로운 게임 이용자 데이터를 생성합니다.
  * 연동 중인 모든 IdP가 로그아웃 처리됩니다.
* Gamebase 탈퇴를 의미하며, IdP 계정 탈퇴를 의미하지는 않습니다.

> <font color="red">[주의]</font><br/>
>
> 여러 IdP를 연동 중인 경우, 모든 IdP 연동이 해제되고 Gamebase 유저 데이터가 삭제됩니다.
>

다음은 로그인 상태에서 게임 이용자 탈퇴를 구현하는 예시 코드입니다.

```js
toast.Gamebase.withdraw(callback)
```

**Example**
```js
function withdraw() {
    toast.Gamebase.withdraw(function (data, error) {
        if (error) {
            // 탈퇴 실패
            console.log(error);
            return;
        }

        // 탈퇴 성공
         console.log(data);
    });
}
```

## Gamebase User's Information
Gamebase로 인증 절차를 진행한 후, 앱을 제작할 때 필요한 정보를 얻을 수 있습니다.

### Get Authentication Information for Gamebase
Gamebase에서 발급한 인증 정보를 가져올 수 있습니다.

```js
// Obtaining Gamebase UserID
var userId =  toast.Gamebase.getUserID();

// Obtaining Gamebase AccessToken
var accessToken = toast.Gamebase.getAccessToken();
```

### Get Banned User Information
NHN Cloud Gamebase Console에 제재된 게임 이용자로 등록될 경우,
로그인을 시도하면 아래와 같은 이용 제한 정보 코드가 표시됩니다. **toast.Gamebase.getBanInfo()** 메서드를 이용해 제재 정보를 확인할 수 있습니다.

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)

## TemporaryWithdrawal

'탈퇴 유예' 기능입니다.
임시 탈퇴를 요청하여 즉시 탈퇴가 진행되지 않고 일정 기간의 유예 기간이 지나면 탈퇴가 이루어집니다.
유예 기간은 콘솔에서 변경할 수 있습니다.

> `주의`
>
> 탈퇴 유예 기능을 사용하는 경우에는 **Gamebase.withdraw()** API 를 사용하지 마세요.
> **Gamebase.withdraw()** API 는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.member.temporaryWithdrawal로 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.requestWithdrawal(callback)
```

**Example**

```js
function requestWithdrawal() {
    gamebase.TemporaryWithdrawal.requestWithdrawal(function (data, error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            } else {
                // Request temporary withdrawal failed.
            }
            return;
        }
        
        // Request temporary withdrawal success.
    });
}
```

### Check TemporaryWithdrawal User

탈퇴 유예를 사용하는 게임은 로그인 후 항상 AuthToken.member.temporaryWithdrawal가 null이 아니라면 해당 유저에게 탈퇴 진행중이라는 사실을 알려주어야 합니다.

**Example**

```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
            // Login failed
            return;
        }

        if(authToken.member.temporaryWithdrawal != null) {    
            // User is under temporary withdrawal    
            var gracePeriodDate = authToken.member.temporaryWithdrawal.gracePeriodDate;            
        } else {
            // Login success.
        }
    });
}
```

### Cancel TemporaryWithdrawal

탈퇴를 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.cancelWithdrawal(callback)
```
**Example**

```js
function cancelWithdrawal() {
    gamebase.TemporaryWithdrawal.cancelWithdrawal(function (error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            } else {
                // Cancel temporary withdrawal failed.
            }
            return;
        }
        
        // Cancel temporary withdrawal success.
    });
}
```

### Withdraw Immediately

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 Gamebase.withdraw() API 와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

**API**

```js
toast.Gamebase.TemporaryWithdrawal.withdrawImmediately(callback)
```

**Example**

```js
function withdrawImmediately() {
    gamebase.TemporaryWithdrawal.withdrawImmediately(function (error) {
        if (error) {
            // Withdraw failed.
            return;
        }
        
        // Withdraw success.
    });
}
```

## Error Handling

| Category       | Error                                                   | Error Code | Description                                                       |
| -------------- | ------------------------------------------------------- | ---------- | ----------------------------------------------------------------- |
| Auth           | INVALID\_MEMBER                                         | 6          | 잘못된 회원에 대한 요청입니다.                                            |
|                | BANNED\_MEMBER                                          | 7          | 제재된 회원입니다.                                                     |
|                | AUTH\_USER\_CANCELED                                    | 3001       | 로그인이 취소되었습니다.                                                |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | 지원하지 않는 인증 방식입니다.                                           | 
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | 이전 인증 프로세스가 완료되지 않았습니다.                                   |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | 토큰 로그인에 실패했습니다.                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | 토큰 정보가 유효하지 않습니다.                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                                      |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdP 로그인에 실패했습니다.                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.)           |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       | 로그아웃에 실패했습니다.                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | 탈퇴에 실패했습니다.                                                   |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴중인 유저 입니다.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴중인 유저가 아닙니다.                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | 플레이할 수 없는 상태입니다(점검 또는 서비스 종료 등).                        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | 알수 없는 오류입니다.(정의되지 않은 오류입니다).                             |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Entire Error Codes](./error-code/#client-sdk)