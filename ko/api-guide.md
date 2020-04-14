## Game > Gamebase > API v1.2 가이드

## 변경 사항
- IAP(in app purchase) API가 변경되었습니다.
- Get Simple Launching API 호출 시 필수 파라미터로 storeCode가 추가되었습니다.
- Check Maintenance API 응답 결과에 점검 대상에 대한 storeCode 정보가 추가되었습니다.
- GUEST 계정의 단말기 이전에 사용되는 TransferAccount에 대해, 사전에 발급된 TransferAccount를 검증할 수 있는 Validate TransferAccount API가 추가되었습니다.
- API 응답 결과의 date 타입이 Epoch time에서 ISO 8601 형식(yyyy-MM-dd'T'HH:mm:ssXXX)으로 변경되었습니다. Token Authentication, Get Member, Get Members API 응답 결과의 regDate, lastLoginDate 항목
- 쿠폰 소진 API가 추가되었습니다.
- 사용자 탈퇴 API가 추가 되었습니다.
- Purchase(IAP)의 구매 가격(price) 데이터 타입이 가이드상에서 Long 으로 잘못 표기된 것을 Float 타입으로 변경하였습니다.

## Advance Notice

Gamebase Server API는 RESTful 형식으로, 서버 API를 사용하기 위해서는 다음 정보들을 알고 있어야 합니다.

#### Server Address

API를 호출하기 위한 서버 주소는 다음과 같습니다. 해당 주소는 Gamebase Console 화면에서도 확인 가능합니다.
> https://api-gamebase.cloud.toast.com

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

앱 ID는 TOAST 프로젝트 ID로 앱 메뉴 화면에서 확인할 수 있습니다.
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
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |SecretKey 설명 참고 |
| X-TCGB-Transaction-Id | optional | TransactionId 설명 참고 |

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

## Authentication

#### Token Authentication

로그인 사용자에게 발급된 Gamebase Access Token이 유효한지를 검증합니다. Access Token이 정상이면 해당 사용자의 정보를 리턴합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.2/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 로그인한 사용자 아이디 |
| accessToken | String | 로그인한 사용자에게 발급된 Access Token |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | true or false (기본값은 false) Access Token을 발급받을 때 사용된, IdP 관련 정보 포함 여부 |

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
| member.lastLoginDate | String | 마지막으로 로그인한 시간 ISO 8601 <br>처음 로그인한 사용자는 해당 값이 없음 |
| member.appId | String | appId |
| member.valid | String | 로그인에 성공한 사용자의 값은 "Y" <br>(다른 값에 대한 설명은 멤버 API 참고) |
| member.regDate | String | 사용자가 계정을 생성한 시간 |
| authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 사용자 인증 시스템 지원 예정 |
| authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest, payco, facebook 등 |
| authList[].authKey | String | authSystem에서 발급된 사용자 구분 값 |


**[Error Code]**

[오류 코드](./error-code/#server)


## Launching

#### Get Simple Launching

Console 화면에서 설정한 서버 주소, 설치 URL 등의 클라이언트 설정 정보 및 현재 점검 상태/시간/ 메시지 등 클라이언트 앱 기동시 제공되는 Launching 정보들에 대해 서버에서 간략히 확인할수 있습니다.
현재 점검 설정 여부만을 확인하고 싶다면, [Check Maintenance] API를 사용하면 됩니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/launching/simple |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | Enum | true | OS 코드 <br>- AOS, IOS, WEB, WINDOWS |
| storeCode | Enum | true | 스토어 코드 <br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |
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
| app.storeCode | String | 앱 스토어코드 <br>"GG", "AS" 등 |
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

<br>

## Member

#### Get Member

단일 사용자에 대해 상세 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/{userId} |


**[Request Header]**

공통 사항 확인


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 조회 대상 사용자 ID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | true or false (기본값은 true) 사용자 단말기, OS 등의 상세 정보 포함 여부 |

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
| member.valid | Enum | Y: 정상 사용자 <br>D: 탈퇴된 사용자 <br>B: 이용 정지된 사용자 <br>M: 유실된 계정|
| member.appId | String | appId |
| member.regDate | String | 사용자가 계정을 생성한 시간 |
| member.lastLoginDate | String | 마지막으로 로그인한 시간 <br>처음 로그인한 사용자는 해당 값이 없음 |
| member.authList | Array[Object] | 사용자 인증 IdP 관련 정보 |
| member.authList[].userId | String | 사용자 ID |
| member.authList[].authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 사용자 인증 시스템 지원 예정 |
| member.authList[].idPCode | String | 사용자 인증 IdP 정보 <br>guest, payco, facebook 등 |
| member.authList[].authKey | String | authSystem에서 발급된 사용자 구분 값 |
| member.authList[].regDate | String | IdP 정보가 사용자 계정과 매핑된 시간 |
| memberInfo | Object | 사용자에 대한 부가 정보 |
| memberInfo.deviceCountryCode | String | 사용자 단말기의 국가 설정 |
| memberInfo.usmCountryCode | String | 사용자 USIM의 국가 코드 |
| memberInfo.language | String | 사용자 언어 |
| memberInfo.osCode | String | 사용자 단말기의 OS 종류 |
| memberInfo.telecom | String | 통신사 |
| memberInfo.storeCode | String | store 코드 |
| memberInfo.network | String | 네트워크 환경 <br>3g, WiFi 등|
| memberInfo.deviceModel | String | 사용자 단말기의 모델명 |
| memberInfo.osVersion | String | 사용자 단말기의 OS 버전 |
| memberInfo.sdkVersion | String | SDK 버전 |
| memberInfo.clientVersion | String | 클라이언트 버전 |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Get Members

다수의 사용자 정보를 간략히 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 조회 대상 사용자 ID <br>  ["userId", "userId", "userId",...]|

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
| memberList | Array[Object] | 조회된 사용자의 기본 정보 |
| memberList[].userId | String | 사용자 ID |
| memberList[].valid | Enum | Y: 정상 사용자 <br>D: 탈퇴된 사용자 <br>B: 이용 정지된 사용자 <br>M: 유실된 계정|
| memberList[].appId | String | appId |
| memberList[].regDate | String | 사용자가 계정을 생성한 시간 |


**[Error Code]**

[오류 코드](./error-code/#server)


#### Get IdP Information

사용자 ID로 매핑된 IdP 정보를 조회합니다.

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.2/apps/{appId}/auth/authKeys |

**[Request Header]**

공통 사항 확인


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 조회 대상 사용자 ID  ["userId", "userId", "userId",...]|

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
| authSystem | String | Gamebase 내부적으로 사용되는 인증 시스템 <br>추후 사용자 인증 시스템 지원 예정 |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Get UserId Information with Auth key

사용자 인증 키에 매핑된 사용자 ID를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Gamebase 내부적으로 사용되는 인증 시스템 추후 사용자 인증 시스템 지원 예정 현재는 gbid |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authKeyList | Array[String] | mandatory | authSystem에서 발급된 authKey ["authKey", "authKey", "authKey",...]|

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
| result | Array[Object] | 조회된 사용자의 기본 정보 authKey가 key이고, userId가 value인 object|

**[Error Code]**

[오류 코드](./error-code/#server)

#### Ban Histories

사용자 이용 정지 이력을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans |


**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 이용 정지 이력 조회 시작 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | 이용 정지 이력 조회 종료 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>begin ~ end 사이 시간에 이용정지가 되었다면 조회 결과에 존재 |
| page | String | optional | 조회하고자 하는 페이지. 0부터 시작 |
| size | String | optional | 한 페이지당 데이터 개수 |


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
| result.appId | String | 조회된 이용 정지 의 TOAST 프로젝트 ID |
| result.banCaller | String | 이용 정지 호출 주체 |
| result.banReason | String | 이용 정지 사유 |
| result.banType | String | 이용 정지 타입. TEMPORARY or PERMANENT |
| result.beginDate | Long | 이용 정지 시작 시간 |
| result.endDate | Long | 이용 정지 종료 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메세지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | Long | 이용 정지 해제 시간 |
| result.releaseReason | String | 이용 정지 해제 사유 |
| result.releaseUser | String | 이용 정지 해제 등록자 |
| result.seq | Long | 이용 정지 내역 순번 |
| result.templateCode | Long | 콘솔에서 등록한 이용 정지 템플릿 코드 값 |
| result.userId | String | 사용자 ID |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Ban Release Histories.

사용자 이용 정지 해제 이력을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans/release |


**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 이용 정지 해제 이력 조회 시작 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | 이용 정지 해제 이력 조회 종료 시간 (ISO 8601 표준 시간, UTF-8 Encoding 필요) <br>begin ~ end 사이 시간에 이용정지가 해제 되었다면 조회 결과에 존재 |
| page | String | optional | 조회하고자 하는 페이지. 0부터 시작 |
| size | String | optional | 한 페이지당 데이터 개수 |


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
| result.appId | String | 조회된 이용 정지 의 TOAST 프로젝트 ID |
| result.banCaller | String | 이용 정지 호출 주체 |
| result.banReason | String | 이용 정지 사유 |
| result.banType | String | 이용 정지 타입. TEMPORARY or PERMANENT |
| result.beginDate | Long | 이용 정지 시작 시간 |
| result.endDate | Long | 이용 정지 종료 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메세지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | Long | 이용 정지 해제 시간 |
| result.releaseReason | String | 이용 정지 해제 사유 |
| result.releaseUser | String | 이용 정지 해제 등록자 |
| result.seq | Long | 이용 정지 내역 순번 |
| result.templateCode | Long | 콘솔에서 등록한 이용 정지 템플릿 코드 값 |
| result.userId | String | 사용자 ID |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Validate TransferAccount

게스트 계정 이전을 위해 발급 받은 ID 및 PASSWORD 의 유효성 검사를 수행합니다. 유효한 TransferAccount인 경우 발급받은 userId 정보를 반환합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.2/apps/{appId}/members/transfer-account |


**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |


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
| member | Object | 조회된 사용자의 기본 정보 |
| member.userId | String | 사용자 ID |
| member.valid | Enum | Y: 정상 사용자 <br>D: 탈퇴된 사용자 <br>B: 이용 정지된 사용자 <br>M: 유실된 계정|
| member.appId | String | appId |
| member.regDate | String | 사용자가 계정을 생성한 시간 |
| member.lastLoginDate | String | 마지막으로 로그인한 시간 <br>처음 로그인한 사용자는 해당 값이 없음 |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Withdraw

사용자 계정을 탈퇴 처리합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.2/apps/{appId}/members/{userId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 탈퇴 대상 사용자 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | mandatory | 탈퇴를 요청한 시스템 혹은 사용자 정보 <br> - 해당 정보는 Console > '멤버' 페이지의 '탈퇴 이력' 화면에서 확인 가능 <br> - 탈퇴 이력 화면은 탈퇴된 이용자 조회시에만 노출됨 |

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

## Maintenance

#### Check Maintenance Set

현재 점검이 설정되어 있는지 여부를 확인합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

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
| maintenances.targetStores | Array[Enum] | 특정 클라이언트에 대해서만 점검을 설정했을 때 점검 설정된 클라이언트의 스토어 코드<br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

## Coupon

#### Check Validation And Consume Coupon

콘솔에서 발급된 쿠폰 코드의 유효성 검증 및 쿠폰 상태를 변경합니다. 유효한 쿠폰이면 소비 상태로 변경하고, 응답 결과로 지급할 아이템 정보를 반환합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.2/apps/{appId}/members/{userId}/coupons/{couponCode} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |
| userId | String | 쿠폰을 사용할 userId |
| couponCode | String | 쿠폰 코드 |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| storeCode | String | optional | 콘솔에서 특정 스토어만 사용 가능하도록 쿠폰을 발급 받았다면, 스토어 코드를 전달해야 함<br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |

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

## Purchase(IAP)

#### Consume

Google Play Store, App Store, ONEStore 등 스토어 결제가 완료된 후에 유저에게 아이템을 지급하기 전에 결제를 소비할 것을 알려야 합니다. 결제 1건당 1번만 결제를 소비할 수 있으며, 결제 상태가 정상이 아니면 소비되지 않습니다.
(결재 소비가 완료되었다면 유저의 결제 및 아이템 지급이 정상적으로 완료되었다고 판단)

소비(consume)하지 않은 결제 내역은 SDK 및 서버의 미소비 결제 내역 조회 API를 통해 조회할 수 있습니다. 참고로 아이템 등록 시 상품 유형이 일회성(CONSUMABLE)인 아이템 결제에 대해서만 소비(consume) 처리됩니다.

> [참고]
> 결제 1건당 1번 소비 가능하며, 결제 소비를 하지 않은 결제는 IAP에서 아이템을 지급하지 않은 것으로 간주합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.2/apps/{appId}/consume |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
  "paymentSeq": "2019091931571201",
  "accessToken" : "90fD1bs1guXwY6aZ7rseEKYW_6gMCISjDASgten4MD6O7XZD7VRjZcs8OTm8lOQVFTegoY4WK78P2WQCMm7cx"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | mandatory | 결제 번호 |
| accessToken | String | mandatory  | 결제 인증 토큰(로그인 인증 토큰이 아님) |

> [참고]
> 클라이언트에서 requestPurchase API 호출시 응답으로 오는 purchaseToken 값이 accessToken으로 사용


**[Response Body]**

```json
{
   "header":{
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "result":{
        "price": 1500,
        "currency": "KRW",
        "productSeq": 12345
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | 결제 기본 정보 |
| result.price | Float | 결제 가격 |
| result.currency  | String  | 결제 통화  |
| result.productSeq | Long | 결제 아이템 번호(콘솔에 등록된 아이템 고유 번호) |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Get Consumable List

결제가 완료되었지만 아직 소비(consume)되지 않은, 미소비 결제 내역을 조회할 수 있습니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.2/apps/{appId}/consumable |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
  "marketId": "GG",
  "userChannel" : "GF",
  "userKey" : "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | mandatory | 스토어 코드<br>GG: Google, AS: Apple, ONESTORE: 원스토어 |
| userChannel | String | mandatory  | 유저 채널<br>현재는 미구현 상태로 항상 `GF`값을 설정  |
| userKey | String | mandatory  | 유저 ID  |

**[Response Body]**

```json
{
    "header":{
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "success"
    },
    "result":[
        {
            "paymentSeq": "2016122110023124",
            "productSeq": 1000292,
            "currency": "KRW",
            "price": 1000,
            "accessToken": "oJgM1EfDRjnQY7yqhWCUVgAXsSxLWq698t8QyTzk3NeeSoytKxtKGjldTc1wkSktgzjsfkVTKE50DoGihsAvGQ"
        },

        {
            "paymentSeq": "2016122110023125",
            "productSeq": 1000292,
            "currency": "KRW",
            "price": 1000,
            "accessToken": "7_3zXyNJub0FNLed3m9XRAAXsSxLWq698t8QyTzk3NeeSoytKxtKGjldTc1wkSktgzjsfkVTKE50DoGihsAvGQ"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 결제 기본 정보 |
| result[].paymentSeq | String  |  결제 번호 |
| result[].productSeq | Long | 결제 아이템 번호(콘솔에 등록된 아이템 고유 번호) |
| result[].currency  | String  | 결제 통화  |
| result[].price | Float | 결제 가격 |
| result[].accessToken | String | 결제 인증 토큰 |

**[Error Code]**

[오류 코드](./error-code/#server)

### Get ActiveSubscription List

유저가 현재 구독 중인 결제를 조회할 수 있습니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.2/apps/{appId}/active-subscriptions |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
  "marketId": "GG",
  "packageName" : "com.toast.gamebase",
  "userChannel" : "GF",
  "userKey" : "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | mandatory | 스토어 코드<br>GG: Google, AS: Apple, ONESTORE: 원스토어 |
| packageName | String | mandatory | 콘솔에 등록한 앱의 packageName |
| userChannel | String | mandatory  | 유저 채널<br>현재는 미구현 상태로 항상 `GF`값을 설정  |
| userKey | String | mandatory  | 유저 ID  |

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
      "channel": "GF",
      "userId": "string",
      "paymentSeq": "2018102610330423",
      "appId": "com.toast.gamebase",
      "productId": "subs_p1w",
      "productType": "AUTO_RENEWABLE",
      "productSeq": 1002904,
      "currency": "KRW",
      "price": 1000,
      "paymentId": "GPA.3375-2193-1175-57698",
      "originalPaymentId": "GPA.3375-2193-1175-57698",
      "purchaseTimeMillis": 1540522998289,
      "expiryTimeMillis": 1541134994548
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 결제 기본 정보 |
| result[].channel  | String  | 유저 채널  |
| result[].userId  | String  | 유저 ID  |
| result[].paymentSeq | String  |  결제 번호 |
| result[].appId | String  |  패키지 이름 |
| result[].productId | String  |  스토어에 등록된 상품(아이템) 식별자 |
| result[].productType | String  |  상품(아이템) 타입<br>구독: AUTO_RENEWABLE |
| result[].productSeq | Long | 결제 아이템 번호(콘솔에 등록된 아이템 고유 번호) |
| result[].currency  | String  | 결제 통화  |
| result[].price | Float | 결제 가격 |
| result[].paymentId | String | 최근 갱신된 스토어 결제 번호 |
| result[].originalPaymentId | String | 최초 스토어 결제 번호 |
| result[].purchaseTimeMillis | Long | 최근 갱신된 시간 |
| result[].expiryTimeMillis | Long | 구독 만료 시간 |


**[Error Code]**

[오류 코드](./error-code/#server)

<br>

## Leaderboard

Gamebase는 TOAST Leaderboard 서비스의 서버 API에 대해 **Wrapping** 기능을 제공합니다. Wrapping 기능을 사용하면 사용자 서버에서 일관된 인터페이스로 TOAST 서비스들을 사용할 수 있습니다.


#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Factor에 등록된 사용자 수 조회 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| 단일 사용자 점수/순위 조회 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 다수 사용자 점수/순위 조회 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 일정 범위의 전체 점수/순위 조회 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 단일 사용자 점수 등록 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 단일 사용자 점수/ExtraData 등록 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 다수 사용자 점수 등록 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 다수 사용자 점수/ExtraData 등록 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| 단일 사용자 Leaderboard정보 삭제 | DELETE | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**해당 API에 대한 상세 설명은 다음 링크를 참고하시기 바랍니다.**


[Leaderboard Guide](/Game/Leaderboard/ko/api-guide/)

##### API 호출 예시

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count
```

<br>

## Others

### Support

API 호출 실패 원인에 대한 문의 사항이 있을 경우, **API 호출 URL(HTTP body가 있는 경우는 body와 함께)과 그에 대한 응답 결과**를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다.



##### API 호출 예시

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.2/apps/C3JmSctU/maintenances/under-maintenance
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
      "uri": "/tcgb-launching/v1.2/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful": false
  }
}

```
