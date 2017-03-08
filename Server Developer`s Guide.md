## Upcoming Products > Gamebase > Server Developer's Guide

Gamebase Server API는 RESTful 형식으로 다음과 같은 API들을 제공합니다.

## 사전 확인 사항
API를 사용하기 위해서는 다음과 같은 정보를 알고 있어야 합니다.

##### 서버 주소
API를 호출하기 위한 서버 주소는 다음과 같습니다. 해당 주소는 Gamebase 콘솔 화면에서도 확인 가능합니다.
```
https://api-gamebase.cloud.toast.com
```

##### appId
appId는 TOAST Cloud의 프로젝트ID로 콘솔 화면 Project list 화면에서 확인 가능합니다. 

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.0.png)

##### secretKey
secretKey는 API에 대한 접근 제어 방안으로 Gamebase 콘솔에서 확인 가능합니다. 해당 값은 API 호출시 HTTP Header에 설정되어야 합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.0.png)

##### transactionId
호출하는 서버 내부적으로 API 요청을 관리 할 수 있도록 transactionId 기능을 제공합니다. 호출하는 서버에서 HTTP Header에 transactionId를 설정하여 API를 호출하면, 응답 HTTP 헤더 및 응답 결과의 Response Body 의 header 항목에 해당 transactionId를 설정하여 결과를 전달합니다.




## 공통

##### HTTP Header

API 호출 시 HTTP Header에 다음 항목들을 설정해야 합니다.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |secretKey 설명 확인 |
| X-TCGB-Transaction-Id | optional | transactionId 설명 확인 |




#### 응답
모든 API 요청에 대한 응답으로 HTTP 200 OK 로 전달합니다. 응답 결과는 Response Body 의 header 항목을 참고하여 성공 유무를 판단할 수 있습니다.

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
X-TCGB-Transaction-Id: String
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
| transactionId | String | API 요청시 HTTP Header에 설정한 값<br>해당 값을 전달하지 않으면 Gamebase 내부적으로 생성된 값을 반환 |
| isSuccessful | boolean | 성공 여부 |
| resultCode | int | 응답 코드 <br>성공시 0 실패시 에러코드 반환 |
| resultMessage | String | 응답 메시지 |





## 인증
#### 토큰 검증

로그인 사용자에게 발급된 Accss Token 이 유효한지를 검사합니다. Access Token이 정상이면 해당 사용자의 정보를 리턴합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.0/apps/{appId}/members/{userId}/tokens/{accessToken} |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |
| userId | 로그인한 사용자 아이디 |
| accessToken | 로그인한 사용자에게 발급된 Access Token |

###### Request Parameter
없음

###### Response Body
```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
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
      }
    ]
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| member.userId | String | 사용자 ID |
| member.lastLoginDate | long | 마지막으로 로그인한 시간<br>처음 로그인한 사용자는 해당 값이 없음 |
| member.appId | String | appId |
| member.valid | String | 로그인에 성공한 사용자의 값은 "Y" <br>(다른 값에 대한 설명은 멤버 API 참고) |
| member.regDate | long | 사용자가 계정을 생성한 시간 |
| authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 이름 <br>추후 사용자가 개발한 인증 시스템 지원 예정 |
| authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest / payco / facebook 등 |
| authList[].authKey | String | authSystem 에서 발급된 사용자 구분 값 |


## 멤버

#### 회원 조회
하나의 회원에 대해 상세 정보를 조회합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.0/apps/{appId}/members/{userId} |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |
| userId | 조회 대상 사용자 ID |

###### Request Parameter
없음

###### Response Body
```json
{
  "header": {
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
			"idPCode": "gbid",
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
| member | Object | 조회된 사용자에 대한 기본 정보 |
| member.userId | String | 사용자 ID |
| member.valid | Enum | Y : 정상 사용자<br>D : 탈퇴된 사용자<br>B : 이용 정지된 사용자<br>M : 유실된 계정|
| member.appId | String | appId |
| member.regDate | long | 사용자가 계정을 생성한 시간 |
| member.lastLoginDate | long | 마지막으로 로그인한 시간<br>처음 로그인한 사용자는 해당 값이 없음 |
| member.authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| member.authList[].userId | String | 사용자 ID |
| member.authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 이름 <br>추후 사용자가 개발한 인증 시스템 지원 예정 |
| member.authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest / payco / facebook 등 |
| member.authList[].authKey | String | authSystem 에서 발급된 사용자 구분 값 |
| member.authList[].regDate | long | IdP정보가 사용자 계정과 매핑된 시간 |
| memberInfo | Object | 사용자에 대한 부가 정보 |
| memberInfo.deviceCountryCode | String | 사용자 기기의 국가 설정 |
| memberInfo.usmCountryCode | String | 사용자 USIM의 국가 코드 |
| memberInfo.language | String | 사용자 언어 |
| memberInfo.osCode | String | 사용자 기기의 OS 종류 |
| memberInfo.telecom | String | 통신사 |
| memberInfo.storeCode | String | store 코드 |
| memberInfo.network | String | network 환경<br>3g / wifi 등|
| memberInfo.deviceModel | String | 사용자 기기의 모델명 |
| memberInfo.osVersion | String | 사용자 기기의 OS 버전 |
| memberInfo.sdkVersion | String | sdk 버전 |
| memberInfo.clientVersion | String | 클라이언트 버전 |

#### 다수의 회원 조회
여러 회원 정보를 간략히 조회합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members/{userId} |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |

###### Request Parameter
| Name | Mandatory | Type | Value |
| --- | --- | --- | --- |
| userIdList | true | Array[String] | 조회 대상 사용자 ID |

###### Response Body
```json
{
  "header": {
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
| memberList | Array[Object] | 조회된 사용자에 대한 기본 정보 |
| memberList[].userId | String | 사용자 ID |
| memberList[].valid | Enum | Y : 정상 사용자<br>D : 탈퇴된 사용자<br>B : 이용 정지된 사용자<br>M : 유실된 계정|
| memberList[].appId | String | appId |
| memberList[].regDate | long | 사용자가 계정을 생성한 시간 |

#### 사용자 ID로 매핑된 IdP 정보 조회
사용자 ID로 여러 사용자의 IdP정보를 조회합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/auth/authKeys |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |

###### Request Parameter
| Name | Mandatory | Type | Value |
| --- | --- | --- | --- |
| userIdList | true | Array[String] | 사용자 ID |

###### Response Body
```json
{
  "header": {
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
| result | Array[Object] | 조회된 사용자에 대한 기본 정보 <br>userId가 key, IdP정보가 value인 object|
| authkey | String | authSystem 에서 발급된 사용자 구분 값 |
| IdPCode | String | 사용자 인증 IdP 정보 <br>guest / payco / facebook 등 |
| authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 이름 <br>추후 사용자가 개발한 인증 시스템 지원 예정 |

#### 사용자 인증키로 매핑된 ID 정보 조회
authSystem의 authKey로 여러 사용자의 사용자 ID를 조회합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members/userIds/authKeys |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |

###### Request Parameter
| Name | Mandatory | Type | Value |
| --- | --- | --- | --- |
| authSystem | true | String | Gamebase 내부적으로 사용되는 인증 시스템 이름 <br>추후 사용자가 개발한 인증 시스템 지원 예정 |
| authKeyList | true | Array[String] | authSystem에서 발급된 authKey |

###### Response Body
```json
{
  "header": {
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
| result | Array[Object] | 조회된 사용자에 대한 기본 정보 <br>authKey가 key, userId가 value인 object|


## 점검

#### 점검 조회

현재 점검이 설정되어 있는지 여부를 확인합니다.

###### Method, URI
| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.0/apps/{appId}/maintenances/under-maintenance |

###### Request Header
공통 사항 확인

###### Path Variable
| Name | Value |
| --- | --- |
| appId | TOAST Cloud 프로젝트 ID |

###### Request Parameter
없음

###### Response Body
```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  },
  "appId": "",
  "underMaintenance": true,
  "maintenances": [
    {
      "typeCode": "APP",
      "beginDate": "2017-01-01T12:10+07:00",
      "endDate": "2017-02-01T12:17+07:00",
      "url": "http://url.info",
      "message": "String"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| underMaintenance | boolean | 현재 점검 설정 여부 |
| maintenances | Object | 점검이 설정되어 있다면 점검에 대한 기본 정보 |
| maintenances.typeCode | Enum | APP: 게임에서 설정한 점검 <br>SYSTEM: Gamebase 시스템에서 설정한 점검<br>(SYSTEM 점검은 사전에 공지됩니다) |
| maintenances.beginDate | String | 점검 시작 시간. ISO 8601 |
| maintenances.endDate | String | 점검 종료 시간. ISO 8601 |
| maintenances.url | String | 상세 점검 URL |
| maintenances.message | String | 점검 메시지 |




## 기타

### API 응답 실패 지원 방안
API 호출 실패 원인에 대한 문의 사항이 있을 경우, API **호출 URL** (HTTP Body가 있는 경우는 body와 함께)과 그에 대한 **응답 결과**를 함께 전달해 주시면 보다 빠른 지원이 가능합니다.

###### API 호출 예시
```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance
```

###### API 실패 응답 결과
```json
{
  "header": {
    "resultCode": -4010004,
    "resultMessage": "Product secretKey is empty or invald, appId:C3JmSctU, secretKey:null",
    "traceError": {
      "trackingTime": 1488698172426,
      "throwPoint": "gateway",
      "uri": "/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful": false
  }
}
```
### 에러코드

API 호출 실패 시 Response Body의 Header 항목 중 **resultCode** 에 대해 에러코드를 확인 할 수 있습니다.
(참고로 Gamebase는 API 호출 실패에 대해서도 HTTP 200 OK 응답을 전달합니다.)

| Code | Description |
| --- | --- |
| -4000001<br>-4000006 | 잘못된 파라미터 타입으로 전달 <br>parameter 는 int 형으로 선언되어 있는데, String 형 데이터가 전달됨 |
| -4000002<br>-4000004 | 필수 parameter 가 생략되었거나 값이 없을때 |
| -4000003 | Request body에 정의되지 않은 값이 전달된 경우 |
| -4000005 | 필수 파라미터가 생략되었거나, 부적절한 값으로 호출될 때 |
| -4010001 | 잘못된 appId가 호출 됨 |
| -4010002 | 잘못된 appKey가 호출 됨 |
| -4010004 | 잘못된 secretKey가 호출 됨 |
| -4060001 | HTTP Header에 Content-Type을 잘 못 설정 |
| -4090001 ~ 3 | DB에 중복된 데이터가 존재하거나, 컬럼 규칙에 맞지 않는 데이터가 전달된 경우 |
| -4090004 | DB 데이터 정합성 문제<br>EX) 컬럼 길이가 10인데 데이터 길이가 15<br>EX) 컬럼 데이터 타입과 요청 데이터가 다름 |
| -5000001 ~ 15 | 내부 시스템 오류 발생 |
| -4000401 | AppId를 잘못 입력했을 때 |
| -4000402 | UserId를 잘못 입력 했을 때 |
| -4000403 | 잘못된 회원에 대한 요청일 때 |
| -4000404 | 잘못된 Auth에 대한 요청일 때 |
| -4040401 | 존재하지 않거나 탈퇴된 회원에 대한 요청일 때 |
| -4100401 | 이미 탈퇴된 회원에 대한 요청일 때 |
| -4220401 | 사용자 auth 데이터가 정상적이지 않을 때 |

