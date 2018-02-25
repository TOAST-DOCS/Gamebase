## Game > Gamebase > API 가이드

Gamebase Server API는 RESTful 형식으로, 다음과 같은 API를 제공합니다.

## Advance Notice

서버 API를 사용하기 위해서는 다음과 같은 정보를 알고 있어야 합니다.
<br>

#### Server Address

API를 호출하기 위한 서버 주소는 다음과 같습니다. 해당 주소는 Gamebase Console 화면에서도 확인 가능합니다.
> https://api-gamebase.cloud.toast.com
<br>

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

<br>

#### AppId

앱 ID는 TOAST 프로젝트 ID로 앱 메뉴 화면에서 확인할 수 있습니다.
![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

<br>

#### SecretKey

비밀 키(secret key)는 API에 대한 접근 제어 방안으로, Gamebase Console에서 확인할 수 있습니다. 비밀 키는 Server API를 호출할 때 HTTP 헤더에 필수로 설정해야 합니다.
> [참고]<br>
> 비밀 키가 외부에 노출되어 잘못된 호출이 발생한다면 **생성** 버튼을 클릭하여 새로운 비밀 키를 만든 후, 새 비밀 키를 사용하면 됩니다.
<br>

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

<br>

#### TransactionId

API를 호출하는 서버에서 내부적으로 API 요청을 관리할 수 있는 방안으로 TransactionId 기능을 제공합니다. 호출하는 서버에서 HTTP 헤더에 트랜잭션 ID를 설정하여 API를 호출하면, Gamebase 서버는 응답 HTTP Header 및 응답 결과의 Response Body Header에 해당 TransactionId를 설정하여 결과를 전달합니다.

<br>
<br>
<br>

## Common

#### HTTP Header

API 호출 시 HTTP Header에 다음 항목들을 설정해야 합니다.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |SecretKey 설명 참고 |
| X-TCGB-Transaction-Id | optional | TransactionId 설명 참고 |

<br>

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
| transactionId | String | API 요청 시 HTTP Header에 설정한 값<br>해당 값을 전달하지 않으면 Gamebase 내부적으로 생성된 값을 반환 |
| isSuccessful | boolean | 성공 여부 |
| resultCode | int | 응답 코드 <br>성공 시 0, 실패 시 오류 코드 반환 |
| resultMessage | String | 응답 메시지 |

<br>
<br>
<br>

## Authentication

#### Token Authentication

로그인 사용자에게 발급된 Access Token이 유효한지를 검사합니다. Access Token이 정상이면 해당 사용자의 정보를 리턴합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.0/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

공통 사항 확인
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 로그인한 사용자 아이디 |
| accessToken | String | 로그인한 사용자에게 발급된 Access Token |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | true or false (기본값은 false) <br>Access Token을 발급받을 때 사용된, IdP 관련 정보 포함 여부 |

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
    "regDate": 1488257745000,
    "lastLoginDate": 1488286059000,
    "authList": [
      {
        "userId": "String",
        "authSystem": "String",
        "idPCode": "String",
        "authKey": "String",
        "regDate": 1488257745000
      },
      {
        "userId": "String",
        "authSystem": "String",
        "idPCode": "String",
        "authKey": "String",
        "regDate": 1490922916000
      }
    ]
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| linkedIdP | Object | 로그인한 사용자가 사용한 IdP 정보 |
| linkedIdP.idPCode | String | IdP 정보 <br>guest, payco, facebook 등 |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | 사용자 ID |
| member.lastLoginDate | long | 마지막으로 로그인한 시간<br>처음 로그인한 사용자는 해당 값이 없음 |
| member.appId | String | appId |
| member.valid | String | 로그인에 성공한 사용자의 값은 "Y" <br>(다른 값에 대한 설명은 멤버 API 참고) |
| member.regDate | long | 사용자가 계정을 생성한 시간 |
| authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템<br>추후 사용자 인증 시스템 지원 예정 |
| authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest, payco, facebook 등 |
| authList[].authKey | String | authSystem에서 발급된 사용자 구분 값 |


**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>
<br>

## Member

#### Get member

단일 회원의 상세 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.0/apps/{appId}/members/{userId} |


**[Request Header]**

공통 사항 확인
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 조회 대상 사용자 ID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | true or false (기본값은 true) <br>사용자 단말기, OS 등의 상세 정보 포함 여부 |

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
    "regDate": 1488185201000,
    "lastLoginDate": 1488185201000,
	"authList": [
		  {
			"userId": "String",
			"authSystem": "String",
			"idPCode": "String",
			"authKey": "String",
			"regDate": 1488185201000
		  }
		]
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
| member | Object | 조회된 사용자의 기본 정보 |
| member.userId | String | 사용자 ID |
| member.valid | Enum | Y: 정상 사용자<br>D: 탈퇴된 사용자<br>B: 이용 정지된 사용자<br>M: 유실된 계정|
| member.appId | String | appId |
| member.regDate | long | 사용자가 계정을 생성한 시간 |
| member.lastLoginDate | long | 마지막으로 로그인한 시간<br>처음 로그인한 사용자는 해당 값이 없음 |
| member.authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| member.authList[].userId | String | 사용자 ID |
| member.authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템<br>추후 사용자 인증 시스템 지원 예정 |
| member.authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest, payco, facebook 등 |
| member.authList[].authKey | String | authSystem에서 발급된 사용자 구분 값 |
| member.authList[].regDate | long | IdP 정보가 사용자 계정과 매핑된 시간 |
| memberInfo | Object | 사용자에 대한 부가 정보 |
| memberInfo.deviceCountryCode | String | 사용자 기기의 국가 설정 |
| memberInfo.usmCountryCode | String | 사용자 USIM의 국가 코드 |
| memberInfo.language | String | 사용자 언어 |
| memberInfo.osCode | String | 사용자 기기의 OS 종류 |
| memberInfo.telecom | String | 통신사 |
| memberInfo.storeCode | String | store 코드 |
| memberInfo.network | String | 네트워크 환경<br>3g, WiFi 등|
| memberInfo.deviceModel | String | 사용자 기기의 모델명 |
| memberInfo.osVersion | String | 사용자 기기의 OS 버전 |
| memberInfo.sdkVersion | String | SDK 버전 |
| memberInfo.clientVersion | String | 클라이언트 버전 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get members

다수의 회원 정보를 간략히 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members |

**[Request Header]**

공통 사항 확인
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 조회 대상 사용자 ID<br>  ["userId", "userId", "userId",...]|

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
		"regDate": 1488185201000
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| memberList | Array[Object] | 조회된 사용자의 기본 정보 |
| memberList[].userId | String | 사용자 ID |
| memberList[].valid | Enum | Y: 정상 사용자<br>D: 탈퇴된 사용자<br>B: 이용 정지된 사용자<br>M: 유실된 계정|
| memberList[].appId | String | appId |
| memberList[].regDate | long | 사용자가 계정을 생성한 시간 |


**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get IdP infomation

사용자 ID로 매핑된 IdP 정보를 조회합니다.

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.0/apps/{appId}/auth/authKeys |

**[Request Header]**

공통 사항 확인
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 조회 대상 사용자 ID<br>  ["userId", "userId", "userId",...]|

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
| result | Array[Object] | 조회된 사용자의 기본 정보 <br>userId가 key, IdP 정보가 value인 object|
| authkey | String | authSystem에서 발급된 사용자 구분 값 |
| IdPCode | String | 사용자 인증 IdP 정보 <br>guest, payco, facebook 등 |
| authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템<br>추후 사용자 인증 시스템 지원 예정 |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

#### Get userId infomation with auth key

사용자 인증 키에 매핑된 사용자 ID를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


**[Request Header]**

공통 사항 확인

<br>


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 사용자 인증 시스템 지원 예정 <br>현재는 gbid |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authKeyList | Array[String] | mandatory | authSystem에서 발급된 authKey<br> ["authKey", "authKey", "authKey",...]|

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
| result | Array[Object] | 조회된 사용자의 기본 정보 <br>authKey가 key이고, userId가 value인 object|

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>
<br>

## Maintenance

#### Check Under Maintenance

현재 점검이 설정되어 있는지 여부를 확인합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.0/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

공통 사항 확인
<br>


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Parameter]**

없음
<br>

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
      "message": "maintenance reason"
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

**[Error Code]**

[오류 코드](./error-code/#server)

<br>
<br>
<br>

## Purchase(IAP)

Gamebase는 TOAST IAP 서비스의 서버 API에 대해 **Wrapping** 기능을 제공합니다. Wrapping 기능을 사용하면 사용자 서버에서 일관된 인터페이스로 TOAST 서비스들을 사용할 수 있습니다.


#### Wrapping API

| API | Method | Wrapping URI | IAP URI |
| --- | --- | --- | --- |
| 아이템 소비 | POST | /tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq} | /inapp/v3/consume/{paymentSeq}/items/{itemSeq} |
| 아이템 조회 | GET | /tcgb-inapp/v1.0/apps/{appId}/item/list/{appSeq} | /standard/item/list/{appSeq} |
| 미소비 결제 내역 조회| POST | /tcgb-inapp/v1.0/apps/{appId}/consumable/list | /standard/inapp/v1/consumable/list |

**해당 API에 대한 상세 설명은 다음 링크를 참고하시기 바랍니다.**

<br>
[Mobile Service > IAP > API 가이드](/Mobile%20Service/IAP/ja/api-guide)

<br>

##### API 호출 예시

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

POST https://api-gamebase.cloud.toast.com/tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq}
```

<br>
<br>
<br>

## Leaderboard

Gamebase는 TOAST Leaderboard 서비스의 서버 API에 대해 **Wrapping** 기능을 제공합니다. Wrapping 기능을 사용하면 사용자 서버에서 일관된 인터페이스로 TOAST 서비스들을 사용할 수 있습니다.


#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Factor에 등록된 사용자 수 조회 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| 단일 사용자 점수/순위 조회 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 다수 사용자 점수/순위 조회 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 일정 범위의 전체 점수/순위 조회 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 단일 사용자 점수 등록 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 단일 사용자 점수/ExtraData 등록 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 다수 사용자 점수 등록 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 다수 사용자 점수/ExtraData 등록 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| 단일 사용자 Leaderboard정보 삭제 | DELETE | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**해당 API에 대한 상세 설명은 다음 링크를 참고하시기 바랍니다.**

<br>
[Game > Leaderboard > API 가이드](/Game/Leaderboard/ja/api-guide)

<br>

##### API 호출 예시

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count
```

<br>
<br>
<br>

## Etc

### Support

API 호출 실패 원인에 대한 문의 사항이 있을 경우, **API 호출 URL(HTTP body가 있는 경우는 body와 함께)과 그에 대한 응답 결과**를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다.

<br>

##### API 호출 예시

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance
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
      "uri": "/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful": false
  }
}

```

