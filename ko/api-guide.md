## Game > Gamebase > API v1.3 가이드

## 변경 사항
- IAP(In App Purchase) API의 요청 파라미터 및 응답 결과에 새로운 항목이 추가 및 삭제 되었습니다.
- Push Wrapping API가 추가 되었습니다.
- Gamebase Access Token으로 로그인시에 사용된 IdP의 프로필 및 토큰 정보를 획득할 수 있는 "Get IdP Token and Profiles" API가 추가되었습니다.
- IdP Id로 매핑된 Gamebase userId 를 획득하는 "Get UserId Information with IdP Id" API가 추가되었습니다.

## Advance Notice

Gamebase Server API는 RESTful 형식으로, 서버 API를 사용하기 위해서는 다음 정보들을 알고 있어야 합니다.

#### Server Address

API를 호출하기 위한 서버 주소는 다음과 같습니다. 해당 주소는 Gamebase Console 화면에서도 확인 가능합니다.
> https://api-gamebase.cloud.toast.com

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

앱 ID는 NHN Cloud 프로젝트 ID로 앱 메뉴 화면에서 확인할 수 있습니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

비밀 키(secret key)는 API에 대한 접근 제어 방안으로, Gamebase Console에서 확인할 수 있습니다. 비밀 키는 Server API를 호출할 때 HTTP 헤더에 필수로 설정해야 합니다.
> [참고]
> 비밀 키가 외부에 노출되어 잘못된 호출이 발생한다면 **생성** 버튼을 클릭하여 새로운 비밀 키를 만든 후, 새 비밀 키를 사용하면 됩니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

API를 호출하는 서버에서 내부적으로 API 요청을 관리할 수 있는 방안으로 TransactionId 기능을 제공합니다. 호출하는 서버에서 HTTP 헤더에 트랜잭션 ID를 설정하여 API를 호출하면, Gamebase 서버는 응답 HTTP Header 및 응답 결과의 Response Body Header에 해당 TransactionId를 설정하여 결과를 전달합니다.

## Common

#### HTTP Header

API 호출 시 HTTP Header에 다음 항목들을 설정해야 합니다.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | Required | application/json; charset=UTF-8 |
| X-Secret-Key | Required |SecretKey 설명 참고 |
| X-TCGB-Transaction-Id | Optional | TransactionId 설명 참고 |

#### API Response

모든 API 요청에 대한 응답으로 **HTTP 200 OK**를 전달합니다. API 요청 성공 유무는 Response Body의 Header 항목을 참고하여 판단할 수 있습니다.

**[Request]**

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
GET https://api-gamebase.cloud.toast.com
```

**[Response]**

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
```

```json
{
    "header" : {
    	"transactionId": "88a1ae42-6b1d-48c8-894e-54e97aca07fq",
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "Success."
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| transactionId | String | API 요청 시 HTTP Header에 설정한 값.<br>해당 값을 전달하지 않으면 Gamebase 내부적으로 생성된 값을 반환 |
| isSuccessful | boolean | 성공 여부 |
| resultCode | int | 응답 코드<br>성공 시 0, 실패 시 오류 코드 반환 |
| resultMessage | String | 응답 메시지 |

<br>
<br>

## Authentication

#### Token Authentication

로그인 유저에게 발급된 Gamebase Access Token이 유효한지를 검증합니다. Access Token이 정상이면 해당 유저의 정보를 리턴합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| userId | String | 로그인한 유저 아이디 |
| accessToken | String | 로그인한 유저에게 발급된 Gamebase Access Token |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | Optional | true or false (기본값은 false) Access Token을 발급받을 때 사용된, IdP 관련 정보 포함 여부 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "String",
        "isSuccessful": true
    },
    "linkedIdP": {
        "idPCode": "String",
        "idPId": "String"
    },
    "member": {
        "userId": "String",
        "valid": "Y",
        "appId": "String",
        "regDate": "2019-08-27T17:41:05+09:00",
        "lastLoginDate": "2019-08-27T17:41:05+09:00",
        "authList": [
            {
                "userId": "String",
                "authSystem": "String",
                "idPCode": "String",
                "authKey": "String",
                "regDate": "2019-08-27T17:41:05+09:00"
            },
            {
                "userId": "String",
                "authSystem": "String",
                "idPCode": "String",
                "authKey": "String",
                "regDate": "2019-08-27T17:41:05+09:00"
            }
        ],
        "temporaryWithdrawal": {
            "gracePeriodDate": "2020-04-18T09:12:01+09:00"
        }
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| linkedIdP | Object | 로그인한 유저가 사용한 IdP 정보 |
| linkedIdP.idPCode | String | [유저 인증 IdP](#identity-provider-code) |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | 유저 ID |
| member.lastLoginDate | String | 마지막으로 로그인한 시간 ISO 8601 <br>처음 로그인한 유저는 해당 값이 없음 |
| member.appId | String | appId |
| member.valid | String | [유저 상태](#member-valid-code)<br>로그인에 성공한 유저 값은 "Y" |
| member.regDate | String | 유저가 계정을 생성한 시간 |
| authList | Array[Object] | 유저 인증 IdP 관련 정보 |
| authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 유저 인증 시스템 지원 예정 |
| authList[].idPCode | String | [유저 인증 IdP](#identity-provider-code) |
| authList[].authKey | String | authSystem에서 IdP Id 별로 발급된 유저 구분 값 |
| temporaryWithdrawal | Object | 탈퇴 유예 관련 정보 <br>valid 가 "T" 값에서만 제공 |
| temporaryWithdrawal.gracePeriodDate | String | 탈퇴 유예 만료 시간 ISO 8601 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br/>
#### Get IdP Token and Profiles

클라이언트에서 "Login with IdP"로 로그인 성공시 발급된 Gamebase Access Token으로, 로그인에 사용된 IdP의 Access Token 및 Profiles 정보를 조회합니다.

> [주의]
> IdP의 Access Token 유효시간은 IdP 별로 모두 다르고 일반적으로 짧습니다.
> 클라이언트에서 "Login as the Latest Login IdP"를 통해 로그인을 성공하고 이후 서버에서 해당 API를 호출한다면, 이미 IdP의 Access Token 이 만료되어, IdP 정보 획득에 실패 할수 있습니다.

<br/>

> [참고]
> IdP의 Access Token 만으로 정보를 획득 할 수 없는 IdP 들도 존재합니다.
> ex) appleid / iosgamecenter / kakaogame : Access Token으로 Server to Server에서 가져올 수 있는 정보가 없다.

<br/>

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/idps/{idPCode}?accessToken={accessToken} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| userId | String | 로그인한 유저 아이디 |
| idPCode | String | [유저 인증 IdP](#identity-provider-code) |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| accessToken | String | Required | 로그인한 유저에게 발급된 Gamebase Access Token |

**[Response Body]**

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "transactionId": "String",
        "isSuccessful": true
    },
    "idPProfile": {
        "sub": "String",
        "name": "String",
        "given_name": "String",
        "locale": "ko",
        "picture": "String"
    },
    "idPToken": {
        "idPCode": "google",
        "accessToken": "ya29.a0AfH6SMCF-MjD_-Eqi62Jm-51IPxnS6HpahqpxqbuaWZPXc68YMmW3sRdif4k7Dmp2Ppn1xzH-JQwPLDv4tMrDFAknG4m_lrHQt4J4En7DAG0bZV4z8uJZE1zYOXHp8"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| idPProfile | Map<String, Object> | 로그인한 유저가 사용한 IdP의 프로필<br>- IdP별로 모두 응답 형태(format)가 다르다 |
| idPToken | Object | 로그인한 유저가 사용한 IdP의 Access Token 정보 |
| idPToken.idPCode | String | [유저 인증 IdP](#identity-provider-code) |
| idPToken.accessToken | String | IdP Access Token |
<br>
<br>

## Launching

#### Get Simple Launching

Console 화면에서 설정한 서버 주소, 설치 URL 등의 클라이언트 설정 정보 및 현재 점검 상태/시간/ 메시지 등 클라이언트 앱 기동시 제공되는 Launching 정보들에 대해 서버에서 간략히 확인할수 있습니다.
현재 점검 설정 여부만을 확인하고 싶다면, [Check Maintenance] API를 사용하면 됩니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/launching/simple |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | Enum | true | [OS 코드](#os-code) |
| storeCode | Enum | true | [스토어 코드](#store-code) |
| clientVersion | String | true | 콘솔에서 설정한 클라이언트 버전 |

**[Response Body]**

##### 정상
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "String",
        "isSuccessful": true
    },
    "launchingData": {
        "status": {
            "code": 200,
            "message": "String"
        },
        "app": {
            "storeCode": "String",
            "accessInfo": {
                "serverAddress": "String",
                "csInfo": "String"
            },
            "relatedUrls": {
                "termsUrl": "String",
                "csUrl": "String",
                "punishRuleUrl": "String",
                "personalInfoCollectionUrl": "String"
            },
            "install": {
                "url": "String"
            }
        }
    }
}
```

##### 점검
```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "String",
        "isSuccessful": true
    },
    "launchingData": {
        "status": {
            "code": 303,
            "message": "String"
        },
        "app": {
            "storeCode": "String",
            "accessInfo": {
                "serverAddress": "String",
                "csInfo": "String"
            },
            "relatedUrls": {
                "termsUrl": "String",
                "csUrl": "String",
                "punishRuleUrl": "String",
                "personalInfoCollectionUrl": "String"
            },
            "install": {
                "url": "String"
            }
        },
        "maintenance": {
            "typeCode": "String",
            "beginDate": "2018-05-23T10:44:00+09:00",
            "endDate": "2022-01-01T10:44:00+09:00",
            "url": null,
            "reason": "String",
            "message": "String"
        }
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| status | Object | 현재 클라이언트 상태를 나타내는 정보 |
| status.code | int | 클라이언트 상태코드 <br><br>정상: 200 <br>업데이트 권장: 201, 업데이트 필수: 300 <br>서비스 종료: 302 <br>점검 중: 303 |
| status.message | String | 클라이언트 상태 메시지 |
| app | Object | 앱의 정보 |
| app.storeCode | String | [스토어 코드](#store-code) |
| app.accessInfo | Object | 콘솔 앱 화면에서 설정한 정보 |
| app.accessInfo.serverAddress | String | 서버 주소<br>클라이언트에서 설정한 서버 주소의 우선순위가 높음. <br>클라이언트 서버 주소 미설정시, 앱 화면에서 설정한 서버 주소가 전달됨. |
| app.accessInfo.csInfo | String | 고객 센터 정보 |
| app.relatedUrls | Object | 앱 내에서 사용할 인앱 URL |
| app.relatedUrls.termsUrl | String | 이용약관 |
| app.relatedUrls.csUrl| String | 고객센터 |
| app.relatedUrls.punishRuleUrl | String | 이용 정지 규정 |
| app.relatedUrls.personalInfoCollectionUrl | String | 개인 정보동의 |
| app.install | Object | 앱 설치 정보 |
| app.install.url | String | 설치 URL |
| maintenance | Object | 점검 정보 |
| maintenance.typeCode | String | 점검 타입 코드 <br>APP: 게임에서 설정한 점검 <br>SYSTEM: Gamebase 시스템에서 설정한 점검 |
| maintenance.beginDate | Date | 점검 시작 시간 ISO 8601 |
| maintenance.endDate | Date | 점검 종료 시간 ISO 8601 |
| maintenance.url | String | 점검 URL |
| maintenance.reason | String | 점검 사유 |
| maintenance.message | String | default 점검 사유 메시지 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>

## Member

#### Get Member

단일 유저에 대해 상세 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/{userId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| userId | String | 조회 대상 유저 ID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | Optional | true or false (기본값은 true) 유저 단말기, OS 등의 상세 정보 포함 여부 |

**[Response Body]**
```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "member": {
        "userId": "String",
        "valid": "Y",
        "appId": "String",
        "regDate": "2019-08-27T17:41:05+09:00",
        "lastLoginDate": "2019-08-27T17:41:05+09:00",
        "authList": [
            {
                "userId": "String",
                "authSystem": "String",
                "idPCode": "String",
                "authKey": "String",
                "regDate": "2019-08-27T17:41:05+09:00"
            }
        ]
    },
    "temporaryWithdrawal": {
        "gracePeriodDate": "2020-04-18T09:12:01+09:00"
    },
    "memberInfo": {
        "deviceCountryCode": "String",
        "usimCountryCode": "String",
        "language": "String",
        "osCode": "String",
        "telecom": "String",
        "storeCode": "String",
        "network": "String",
        "deviceModel": "String",
        "osVersion": "String",
        "sdkVersion": "String",
        "clientVersion": "String"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| member | Object | 조회된 유저의 기본 정보 |
| member.userId | String | 유저 ID |
| member.valid | Enum | [유저 상태](#member-valid-code) |
| member.appId | String | appId |
| member.regDate | String | 유저가 계정을 생성한 시간 |
| member.lastLoginDate | String | 마지막으로 로그인한 시간 <br>처음 로그인한 유저는 해당 값이 없음 |
| member.authList | Array[Object] | 유저 인증 IdP 관련 정보 |
| member.authList[].userId | String | 유저 ID |
| member.authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 유저 인증 시스템 지원 예정 |
| member.authList[].idPCode | String | [유저 인증 IdP](#identity-provider-code) |
| member.authList[].authKey | String | authSystem에서 발급된 유저 구분 값 |
| member.authList[].regDate | String | IdP 정보가 유저 계정과 매핑된 시간 |
| temporaryWithdrawal | Object | 탈퇴 유예 관련 정보 <br>valid 가 "T" 값에서만 제공 |
| temporaryWithdrawal.gracePeriodDate | String | 탈퇴 유예 만료 시간 ISO 8601 |
| memberInfo | Object | 유저에 대한 부가 정보 |
| memberInfo.deviceCountryCode | String | 유저 단말기의 국가 설정 |
| memberInfo.usmCountryCode | String | 유저 USIM의 국가 코드 |
| memberInfo.language | String | 유저 언어 |
| memberInfo.osCode | String | [OS 코드](#os-code) |
| memberInfo.telecom | String | 통신사 |
| memberInfo.storeCode | String | [스토어 코드](#store-code) |
| memberInfo.network | String | 네트워크 환경 <br>3g, WiFi 등|
| memberInfo.deviceModel | String | 유저 단말기의 모델명 |
| memberInfo.osVersion | String | 유저 단말기의 OS 버전 |
| memberInfo.sdkVersion | String | SDK 버전 |
| memberInfo.clientVersion | String | 클라이언트 버전 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get Members

다수의 유저 정보를 간략히 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 조회 대상 유저 ID |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "memberList": [
        {
            "userId": "String",
            "valid": "Y",
            "appId": "String",
            "regDate": "2019-08-27T17:41:05+09:00"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| memberList | Array[Object] | 조회된 유저의 기본 정보 |
| memberList[].userId | String | 유저 ID |
| memberList[].valid | Enum | [유저 상태](#member-valid-code) |
| memberList[].appId | String | appId |
| memberList[].regDate | String | 계정 생성 시간 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get IdP Information

유저 ID로 매핑된 IdP 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/auth/authKeys |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 조회 대상 유저 ID |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "String": [
            {
                "authKey": "String",
                "idPCode": "gbid",
                "authSystem": "String"
            }
        ]
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 조회된 유저의 기본 정보 <br>userId가 key, IdP 정보가 value인 object|
| authkey | String | authSystem에서 발급된 유저 구분 값 |
| IdPCode | String | [유저 인증 IdP](#identity-provider-code) |
| authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 유저 인증 시스템 지원 예정 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get UserId Information with Auth key

유저 인증 키에 매핑된 유저 ID를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | Required | Gamebase 내부적으로 사용되는 인증 시스템 추후 유저 인증 시스템 지원 예정 현재는 gbid |

**[Request Body]**

```json
["authKey", "authKey", "authKey"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | authSystem에서 발급된 authKey |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "String": "String"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 조회된 유저의 기본 정보 authKey가 key이고, userId가 value인 object|

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get UserId Information with IdP Id

IdP ID로 매핑된 유저 ID 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/idps/{idPCode}/members |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| idPCode | String | [유저 인증 IdP](#identity-provider-code) |

**[Request Body]**

```json
["idPId", "idPId", "idPId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 조회 대상 유저의 IdP ID <br> 조회 대상 리스트 크기는 최대 300 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "idPId": "userId",
        "idPId": "userId",
        "idPId": "userId"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Map<String, String> | 조회된 유저의 ID 정보 <br>- IdP ID가 key, Gamebase userId 가 value<br>- 조회 요청한 IdP ID를 가지는 userId 정보가 없을 경우 응답 결과에 존재하지 않습니다. |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Ban Histories

유저 이용 정지 이력을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | Required | 이용 정지 이력 조회 시작 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | Required | 이용 정지 이력 조회 종료 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>begin ~ end 사이 시간에 이용정지가 되었다면 조회 결과에 존재 |
| page | String | Optional | 조회하고자 하는 페이지. 0부터 시작 |
| size | String | Optional | 한 페이지당 데이터 개수 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "pagingInfo": {
        "first": true,
        "last": true,
        "numberOfElements": 0,
        "page": 0,
        "size": 0,
        "totalElements": 0,
        "totalPages": 0
    },
    "result": [
        {
            "appId": "String",
            "banCaller": "CONSOLE",
            "banReason": "String",
            "banType": "TEMPORARY",
            "beginDate": 0,
            "endDate": 0,
            "flags": "String",
            "message": "String",
            "name": "String",
            "regUser": "String",
            "releaseCaller": "CONSOLE",
            "releaseDate": 0,
            "releaseReason": "String",
            "releaseUser": "String",
            "seq": 0,
            "templateCode": 0,
            "userId": "String"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 조회된 페이징 정보 |
| pagingInfo.first | boolean | 첫번째 페이지이면 true |
| pagingInfo.last | boolean | 마지막 페이지이면 true |
| pagingInfo.numberOfElements | int | 전체 데이터 수 |
| pagingInfo.page | int | 페이지 번호 |
| pagingInfo.size | int | 한 페이지당 데이터 개수 |
| pagingInfo.totalElements | int | 전체 데이터 수 |
| pagingInfo.totalPages | int | 전체 페이징 수 |
| result | Array[Object] | 조회된 이용 정지 내역 |
| result.appId | String | 조회된 이용 정지의 NHN Cloud 프로젝트 ID |
| result.banCaller | String | 이용 정지 호출 주체 |
| result.banReason | String | 이용 정지 사유 |
| result.banType | String | 이용 정지 타입. TEMPORARY or PERMANENT |
| result.beginDate | Long | 이용 정지 시작 시간 |
| result.endDate | Long | 이용 정지 종료 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메시지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | Long | 이용 정지 해제 시간 |
| result.releaseReason | String | 이용 정지 해제 사유 |
| result.releaseUser | String | 이용 정지 해제 등록자 |
| result.seq | Long | 이용 정지 내역 순번 |
| result.templateCode | Long | 콘솔에서 등록한 이용 정지 템플릿 코드 값 |
| result.userId | String | 유저 ID |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Ban Release Histories.

유저 이용 정지 해제 이력을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans/release |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | Required | 이용 정지 해제 이력 조회 시작 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | Required | 이용 정지 해제 이력 조회 종료 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>begin ~ end 사이 시간에 이용정지가 해제 되었다면 조회 결과에 존재 |
| page | String | Optional | 조회하고자 하는 페이지. 0부터 시작 |
| size | String | Optional | 한 페이지당 데이터 개수 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "pagingInfo": {
        "first": true,
        "last": true,
        "numberOfElements": 0,
        "page": 0,
        "size": 0,
        "totalElements": 0,
        "totalPages": 0
    },
    "result": [
        {
            "appId": "String",
            "banCaller": "CONSOLE",
            "banReason": "String",
            "banType": "TEMPORARY",
            "beginDate": 0,
            "endDate": 0,
            "flags": "String",
            "message": "String",
            "name": "String",
            "regUser": "String",
            "releaseCaller": "CONSOLE",
            "releaseDate": 0,
            "releaseReason": "String",
            "releaseUser": "String",
            "seq": 0,
            "templateCode": 0,
            "userId": "String"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 조회된 페이징 정보 |
| pagingInfo.first | boolean | 첫번째 페이지이면 true |
| pagingInfo.last | boolean | 마지막 페이지이면 true |
| pagingInfo.numberOfElements | int | 전체 데이터 수 |
| pagingInfo.page | int | 페이지 번호 |
| pagingInfo.size | int | 한 페이지당 데이터 개수 |
| pagingInfo.totalElements | int | 전체 데이터 수 |
| pagingInfo.totalPages | int | 전체 페이징 수 |
| result | Array[Object] | 조회된 이용 정지 정보 |
| result.appId | String | 조회된 이용 정지의 NHN Cloud 프로젝트 ID |
| result.banCaller | String | 이용 정지 호출 주체 |
| result.banReason | String | 이용 정지 사유 |
| result.banType | String | 이용 정지 타입. TEMPORARY or PERMANENT |
| result.beginDate | Long | 이용 정지 시작 시간 |
| result.endDate | Long | 이용 정지 종료 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메시지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | Long | 이용 정지 해제 시간 |
| result.releaseReason | String | 이용 정지 해제 사유 |
| result.releaseUser | String | 이용 정지 해제 등록자 |
| result.seq | Long | 이용 정지 내역 순번 |
| result.templateCode | Long | 콘솔에서 등록한 이용 정지 템플릿 코드 값 |
| result.userId | String | 유저 ID |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Validate TransferAccount

게스트 계정 이전을 위해 발급 받은 ID 및 PASSWORD 의 유효성 검사를 수행합니다. 유효한 TransferAccount인 경우 발급받은 userId 정보를 반환합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/transfer-account |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
    "account": {
        "id": "198704206255",
        "password": "Zw548q7zE"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| account.id | String | 유효성 검증을 수행할 ID |
| account.password | String | 유효성 검증을 수행할 PASSWORD |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "member": {
        "userId": "String",
        "valid": "Y",
        "appId": "String",
        "regDate": "2019-08-27T17:41:05+09:00",
        "lastLoginDate": "2019-08-27T17:41:05+09:00"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| member | Object | 조회된 유저의 기본 정보 |
| member.userId | String | 유저 ID |
| member.valid | Enum | [유저 상태](#member-valid-code) |
| member.appId | String | appId |
| member.regDate | String | 유저가 계정을 생성한 시간 |
| member.lastLoginDate | String | 마지막으로 로그인한 시간 <br>처음 로그인한 유저는 해당 값이 없음 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Withdraw

유저 계정을 탈퇴 처리합니다.

> [참고]
> SDK의 탈퇴 API를 사용하지 않고 서버 탈퇴 API를 사용하여 계정 탈퇴를 구현한 경우, 클라이언트에서는 탈퇴 성공 후 SDK의 logout API를 호출하여 캐시되어 있는 토큰 등의 데이터 삭제가 필요합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}?regUser={regUser} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| userId | String | 탈퇴 대상 유저 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | Required | 탈퇴를 요청한 시스템 혹은 사용자 정보 <br> - 해당 정보는 Console > '멤버' 페이지의 '탈퇴 이력' 화면에서 확인 가능 |

**[Request Body]**

없음

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    }
}
```

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>

## Maintenance

#### Check Maintenance Set

현재 점검이 설정되어 있는지 여부를 확인합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

없음

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "String",
        "isSuccessful": true
    },
    "appId": "",
    "underMaintenance": true,
    "maintenances": [
        {
            "typeCode": "APP",
            "beginDate": "2017-01-01T12:10:00+07:00",
            "endDate": "2017-02-01T12:17:00+07:00",
            "url": "http://url.info",
            "message": "maintenance message",
            "targetStores": [
                "GG",
                "AS",
                "ONESTROE"
            ]
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| underMaintenance | boolean | 현재 점검 설정 여부 |
| maintenances | Object | 점검이 설정되어 있다면 점검 기본 정보 |
| maintenances.typeCode | Enum | APP: 게임에서 설정한 점검 <br>SYSTEM: Gamebase 시스템에서 설정한 점검 |
| maintenances.beginDate | String | 점검 시작 시간. ISO 8601 |
| maintenances.endDate | String | 점검 종료 시간. ISO 8601 |
| maintenances.url | String | 상세 점검 URL |
| maintenances.message | String | 점검 메시지 |
| maintenances.targetStores | Array[Enum] | 특정 클라이언트에 대해서만 점검을 설정했을 때 점검 설정된 클라이언트의 [스토어 코드](#store-code) |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>

## Coupon

#### Check Validation And Consume Coupon

콘솔에서 발급된 쿠폰 코드의 유효성 검증 및 쿠폰 상태를 변경합니다. 유효한 쿠폰이면 소비 상태로 변경하고, 응답 결과로 지급할 아이템 정보를 반환합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/coupons/{couponCode}?storeCode={storeCode} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |
| userId | String | 쿠폰을 사용할 userId |
| couponCode | String | 쿠폰 코드 |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| storeCode | String | Optional | 콘솔에서 특정 스토어에서 설치한 앱에 대해서만 쿠폰 사용이 가능하도록 설정하였다면, 스토어 코드를 전달해야 함<br>- 전체 스토어인 경우 ALL 또는 파라미터 생략 가능<br>- [스토어 코드](#store-code) |

**[Response Body]**

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "title": "Coupon Title",
        "benefits": [
            {
                "itemId": "heart",
                "amount": 10
            },
            {
                "itemId": "diamond",
                "amount": 20
            }
        ]
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | 쿠폰 정보 |
| result.title | String | 쿠폰 이름 |
| result.benefits | Array[Object] | 지급할 아이템 정보 |
| result.benefits.itemId | String | 아이템 ID |
| result.benefits.amount | Integer | 아이템 개수 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>

## Purchase(IAP)

#### Consume

Google Play Store, App Store, ONEStore 등 스토어 결제가 정상으로 완료되었다면 유저에게 아이템 지급 및 서버 내부적으로 이력을 기록한 후에, Gmaebase에 결제 소비를 알립니다. 결제 1건당 1번만 결제를 소비할 수 있으며 결제 상태가 정상이 아니면 소비되지 않습니다.

> [참고]
> 상품 등록 시 상품 유형이 일회성(CONSUMABLE)인 아이템 결제에 대해서만 소비(consume) 처리됩니다.
> 결제 1건당 1번 소비 가능하며, 결제 소비를 하지 않은 결제는 IAP에서는 아이템을 지급하지 않은 것으로 간주합니다.

소비(consume)하지 않은 결제 내역은 SDK 및 서버의 미소비 결제 내역 조회 API를 통해 조회할 수 있습니다. API를 통해 미소비 결재 내역이 존재하더라고, 게임서버 내부적으로 아이템 지급에 대한 이력을 가지고 있다면 게임 서버 내부 지급 이력을 우선으로 판단하면 됩니다.
(네트워크 장애 등으로 API timeout이 발생하면 Gamebase에서는 지급이 완료되지만, 게임 서버에서는 API 응답 실패로 유저에게 아이템이 지급되지 않을 수 있음)

> [참고]
> 게임 내부적으로 아이템 지급 이력을 모두 관리할 수 없다면 해당 API의 request timeout을 10초 이상으로 하고, API timout 발생시 만이라도 이력을 기록하여 중복 지급 혹은 미지급 이슈에 대한 방안이 필요함

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consume |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
    "paymentSeq": "2019091931571201",
    "accessToken": "90fD1bs1guXwY6aZ7rseEKYW_6gMCISjDASgten4MD6O7XZD7VRjZcs8OTm8lOQVFTegoY4WK78P2WQCMm7cx"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | Required | 결제 번호 |
| accessToken | String | Required  | 결제 인증 토큰(로그인 인증 토큰이 아님) |

> [참고]
> 클라이언트에서 requestPurchase API 호출시 응답으로 오는 purchaseToken 값이 accessToken으로 사용

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": {
        "price": 1500.0,
        "currency": "KRW",
        "productSeq": 1000292,
        "marketId": "GG",
        "gamebaseProductId": "tap_prod_001",
        "payload": "additional info"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | 결제 기본 정보 |
| result.price | Float | 결제 가격 |
| result.currency | String  | 결제 통화  |
| result.productSeq | Long | 아이템 번호<br>콘솔에서 상품 등록 시, 외부 스토어 아이템에 대해 자동 생성된 값 |
| result.marketId | String | [스토어 코드](#store-code) |
| result.gamebaseProductId | String | Gamebase 상품 아이디<br>콘솔에서 상품 등록 시, 사용자 입력 값 |
| result.payload | String | SDK에서 설정한 추가 정보 |

> [참고]
> 클라이언트에서 사용하는 SDK 버전 및 결제 API에 따라 응답 결과에 gamebaseProductId 값이 존재합니다.

> [참고]
> 게임 서버에서는 아이템 번호 또는 스토어 코드와 Gamebase 상품 아이디로 지정한 상품(아이템)을 지급할 수 있지만, 1개의 스토어 아이템 아이디에 N개의 Gamebase 상품을 등록한 경우 스토어 코드와 Gamebase 상품 아이디로 지급해야 합니다.

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### List Consumables

결제가 완료되었지만 아직 소비(consume)되지 않은, 미소비 결제 내역을 조회할 수 있습니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consumable |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
    "marketId": "GG",
    "userId": "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | Required | [스토어 코드](#store-code) |
| userId | String | Required | 유저 ID  |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "success"
    },
    "result": [
        {
            "paymentSeq": "2020060210364966",
            "productSeq": 1000312,
            "currency": "KRW",
            "price": 2500.0,
            "marketId": "AS",
            "accessToken": "ja5SBJBfr7rYUdjFr6dRe7gKnkX0r7EKPvuK6CIUBBekc1rE9CVbMKVCNuw6ZtwmcpDRXrToR9l26NF9zub6ol",
            "gamebaseProductId": "gamebase_prod_001",
            "purchaseTime": "2020-06-02T13:38:56+09:00",
            "payload": "additional info"
        },
        {
            "paymentSeq": "2016122110023125",
            "productSeq": 1000292,
            "currency": "KRW",
            "price": 1000.0,
            "marketId": "AS",
            "accessToken": "7_3zXyNJub0FNLed3m9XRAAXsSxLWq698t8QyTzk3NeeSoytKxtKGjldTc1wkSktgzjsfkVTKE50DoGihsAvGQ",
            "gamebaseProductId": "gamebase_prod_002",
            "purchaseTime": "2020-06-02T13:37:42+09:00"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 결제 기본 정보 |
| result[].paymentSeq | String |  결제 번호 |
| result[].productSeq | Long | 아이템 번호<br>콘솔에서 상품 등록 시, 외부 스토어 아이템에 대해 자동 생성된 값 |
| result[].currency  | String | 결제 통화  |
| result[].price | Float | 결제 가격 |
| result[].accessToken | String | 결제 인증 토큰 |
| result[].marketId | String | [스토어 코드](#store-code) |
| result[].gamebaseProductId | String | Gamebase 상품 아이디<br>콘솔에서 상품 등록 시, 유저 입력 값 |
| result[].purchaseTime | String | 결제 발생 일시 |
| result[].payload | String | SDK에서 설정한 추가 정보 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

### List Active Subscriptions

사용자가 현재 구독 중인 결제를 조회할 수 있습니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/active-subscriptions |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
    "marketId": "GG",
    "packageName": "com.toast.gamebase",
    "userId": "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | Required | [스토어 코드](#store-code) |
| packageName | String | Required | 콘솔에 등록한 앱의 packageName |
| userId | String | Required | 유저 ID  |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result": [
        {
            "marketId": "GG",
            "userId": "QXG774PMRZMWR3BR",
            "paymentSeq": "2020052810364755",
            "accessToken": "NczL3n4TumMF8n9oRR5l8zXDyMXRVjxSRks0Lk1Saob2A9rdAupqjZSrQ0-hb2GOSFwTx5uDDchH8EB-EkWGGQ",
            "productSeq": 1001221,
            "productId": "money_100",
            "productType": "AUTO_RENEWABLE",
            "paymentId": "GPA.3302-8679-7228-41195",
            "price": 1000.0,
            "currency": "KRW",
            "gamebaseProductId": "gamebase_renewal_001",
            "payload" : "additional info",
            "purchaseTime": "2020-06-02T13:38:56+09:00",
            "expiryTime": "2020-06-02T13:48:56+09:00"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 결제 기본 정보 |
| result[].marketId  | String  | [스토어 코드](#store-code) |
| result[].userId | String  | 유저 ID  |
| result[].paymentSeq | String  | 결제 번호 |
| result[].accessToken | String | 결제 인증 토큰 |
| result[].productSeq | Long | 아이템 번호<br>콘솔에서 상품 등록 시, 외부 스토어 아이템에 대해 자동 생성된 값 |
| result[].productId | String | 스토어에 등록된 상품(아이템) 식별자 |
| result[].productType | String  | 상품(아이템) 타입<br>구독: AUTO_RENEWABLE |
| result[].currency  | String  | 결제 통화 |
| result[].price | Float | 결제 가격 |
| result[].paymentId | String | 최근 갱신된 스토어 결제 번호 |
| result[].gamebaseProductId | String | Gamebase 상품 아이디<br>콘솔에서 상품 등록 시, 사용자 입력 값 |
| result[].payload | String | SDK에서 설정한 추가 정보 |
| result[].purchaseTime | String | 최근 갱신된 시간 |
| result[].expiryTime | String | 구독 만료 시간 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>

## Leaderboard

Gamebase는 NHN Cloud Leaderboard 서비스의 서버 API에 대해 **Wrapping** 기능을 제공합니다. Wrapping 기능을 사용하면 사용자 서버에서 일관된 인터페이스로 NHN Cloud 서비스들을 사용할 수 있습니다.

> [참고]
> Gamebase를 활성화 하면 Leaderboard Appkey 설정 없이 Gamebase Wrapping API를 호출하여 Leaderboard 기능을 사용할 수 있습니다.

<br>

#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Factor에 등록된 유저 수 조회<br>- Get user count in factor | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| Factor 전체 수 검색<br>- Get total factor count | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factor-count | /leaderboard/v2.0/appkeys/{appKey}/factor-count |
| Factor 정보 조회<br>- Get factor info<br>- Get multiple factor info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors | /leaderboard/v2.0/appkeys/{appKey}/factors |
| 단일 유저 점수/순위 조회<br>- Get single user info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 다수 유저 점수/순위 조회<br>- Get multiple user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 일정 범위의 전체 점수/순위 조회<br>- Get multiple user info by range | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 특정 순위의 유저들을 검색<br>- Get selected rank user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |
| 특정 유저의 순위 및 상위, 하위 유저들의 순위 검색<br>- Get multiple user info by pivot user | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} | /leaderboard/v2.0/appkeys/{appkey}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} |
| 단일 유저 점수 등록<br>- Set single user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 단일 유저 점수/ExtraData 등록<br>- Set single user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 다수 유저 점수 등록<br>- Set multiple user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 다수 유저 점수/ExtraData 등록<br>- Set multiple user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| 유저 Leaderboard 정보 삭제<br>- Delete single user info<br>- Delete multiple user info | DELETE | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |

<br/>

**해당 API에 대한 상세 설명은 다음 링크를 참고하시기 바랍니다.**
Gamebase Wrapping API와 매핑된 Leaderboard API 스펙은 아래 가이드를 참고하세요.
Leaderboard Appkey 설정 없이 Gamebase AppId 및 SecretKey를 이용해서 Gamebase Wrapping Leaderboard API를 호출하시면 됩니다.


[Leaderboard Guide](/Game/Leaderboard/ko/api-guide/)

<br/>

##### API 호출 예시

```
GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count

Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
```

<br/>
<br/>

## Push

Gamebase는 NHN Cloud Push 서비스의 서버 API에 대해 **Wrapping** 기능을 제공합니다. Wrapping 기능을 사용하면 사용자 서버에서 일관된 인터페이스로 NHN Cloud 서비스들을 사용할 수 있습니다.

> [참고]
> Gamebase를 활성화 하면 Push Appkey 설정 없이 Gamebase Wrapping API를 호출하여 Push 기능을 사용할 수 있습니다.


<br>

#### Wrapping API
|    | API | Method | Wrapping URI | Push URI |
| --- | --- | --- | --- | --- |
| 메시지 | 발송 | POST | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 조회 | GET | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 발송 로그 조회 | GET | /tcgb-push/v1.3/apps/{appId}/logs/message | /push/v2.4/appkeys/{appkey}/logs/message |
| 예약 메시지 | 발송 스케줄 생성 | POST | /tcgb-push/v1.3/apps/{appId}/schedules | /push/v2.4/appkeys/{appkey}/schedules |
|   | 생성 | POST | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | 목록 조회 | GET | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | 단건 조회 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id} | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id} |
|   | 발송 완료 조회 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id}/messages | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id}/messages |
|   | 수정 | PUT | /tcgb-push/v1.3/apps/{appId}/reservations/{reservationId} | /push/v2.4/appkeys/{appkey}/reservations/{reservationId} |
|   | 삭제 | DELETE | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |

<br/>

**해당 API에 대한 상세 설명은 다음 링크를 참고하시기 바랍니다.**
Gamebase Wrapping API와 매핑된 Push API 스펙은 아래 가이드를 참고하세요.
Push Appkey 설정 없이 Gamebase AppId 및 SecretKey를 이용해서 Gamebase Wrapping Push API를 호출하시면 됩니다.

> [참고]
> Push 가이드에 존재하는 uid 값은 gamebase userId 값을 사용하시면 됩니다. 클라이언트 SDK에서 푸시 토큰 등록 시 유저 식별자는 gamebase userId로 등록이 되어 있습니다.
> 한 명의 유저가 다수의 단말기에서 모두 푸시 수신을 허용하였다면, 다수의 단말기에서 모두 푸시를 수신하게 됩니다.

[Push Guide](/Notification/Push/ko/api-guide/)

<br/>

##### API 호출 예시

```
POST https://api-gamebase.cloud.toast.com/tcgb-push/v1.3/apps/{appId}/messages

Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

{
    "target" : {
        "type" : "UID",
        "to": ["gamebase userId-1", "gamebase userId-2"]
    },
    "content" : {
        "default" : {
            "title": "title",
            "body": "body"
        }
    },
    "messageType" : "AD",
    "contact": "1588-1588",
    "removeGuide": "메뉴 > 설정",
    "timeToLiveMinute": 1,
    "provisionedResourceId": "id",
    "adWordPosition": "TITLE"
}
```

<br/>
<br/>

## Others

### OS Code

유저 단말기의 OS에 대해 Gamebase 내부적으로 정의한 코드입니다.

| Code | 설명 |
| --- | --- |
| AOS | Android |
| IOS | iOS |
| WEB | Web |
| WINDOWS | Windows |
<br/>

### Store Code

앱을 설치한 스토어에 대해 Gamebase 내부적으로 정의한 코드입니다.

| Code | 설명 |
| --- | --- |
| GG | Google Play Store |
| AS | App Store |
| ONESTORE | ONE store |
| GALAXY | Galaxy Store |
| AMAZON | Amazon Appstore |
| HUAWEI | Huawei AppGallery |
<br/>

### Identity Provider Code

유저 인증에 사용된 Identity Provider들에 대해 Gamebase 내부적으로 정의한 코드입니다.

- guest
- google
- facebook
- appleid
- iosgamecenter
- payco
- hangame
- twitter
- naver
- line
- kakaogame
- weibo
<br/>

### Member Valid Code

유저의 현재 상태에 대해 Gamebase 내부적으로 정의한 코드입니다.

| Code | 설명 |
| --- | --- |
| Y | 정상 유저 |
| D | 탈퇴된 유저 |
| B | 이용 정지된 유저 |
| T | 탈퇴 유예 상태인 유저 |
| P | 이용 정지 유예 상태인 유저 |
| M | 유실된 계정 |
<br/>


### Support

API 호출 실패 원인에 대한 문의 사항이 있을 경우, **API 호출 URL(HTTP body가 있는 경우는 body와 함께)과 그에 대한 응답 결과**를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다.

##### API 호출 예시

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance
```

##### API 실패 응답 결과

```json
{
    "header": {
        "transactionId": "18a1ae42-6b1d-54c8-894e-54e97bca07fq",
        "resultCode": -4010002,
        "resultMessage": "Gamebase product appKey is invalid, appId:C3JmSctU",
        "traceError": {
            "trackingTime": 1489726350287,
            "throwPoint": "gateway",
            "uri": "/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance"
        },
        "isSuccessful": false
    }
}
```
