## Game > Gamebase > API v1.2 Guide

## 변경 사항
- IAP API가 변경 되었습니다.
- Get Simple Launching API 호출 시 필수 파라미터로 storeCode가 추가 되었습니다.
- Check Maintenance API 응답 결과에 점검 대상에 대한 storeCode 정보가 추가 되었습니다.
- GUEST 계정에 대한 단말기 이전에 사용되는 TransferAccount에 대해, 사전에 발급된 TransferAccount를 검증 할 수 있는 Validate TransferAccount API가 추가 되었습니다.
- API 응답결과의 date 타입이 Epoch time 에서 ISO 8601 형식(yyyy-MM-dd'T'HH:mm:ssXXX)으로 변경되었습니다. Token Authentication, Get Member, Get Members API 응답 결과의 regDate, lastLoginDate 항목
- 쿠폰 소진 API가 추가 되었습니다.

## Advance Notice

Gamebase Server API provides APIs as follows, in the RESTful format. Following information is required to use Server API.

#### Server Address

To call API, below address is needed, which is also available in the Gamebase Console.
> https://api-gamebase.cloud.toast.com

![image alt](./image/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

App ID, as a project ID of TOAST, can be found on the **Project List** page of the Console.
![image alt](./image/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

Secret Key, as a control access of API, can be found in the Gambase Console. It must be set at the HTTP header to call Server API.
> [Note]<br>
> When a secret key is exposed and a wrong call is made, click **Create** to create a new secret key and replace the old one.

![image alt](./image/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

AS part of managing API internally within a server that calls API, TransactionId is provided.  By setting a transaction ID at the HTTP header from a calling server to call API, the Gamebase server delivers results with corresponding TransactionId set at the response HTTP Header and Response Body Header of results.

## Common

#### HTTP Header

Following items should be set at the HTTP Header to call API.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |Refer to description of SecretKey  |
| X-TCGB-Transaction-Id | optional | Refer to description of TransactionId |

#### API Response

As a response to all API requests, **HTTP 200 OK** is delivered. Whether an API request is successful or not can be determined in reference of the Header of Response Body.

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
| transactionId | String | The value set at HTTP Header when API is requested.<br>If the value is not delivered, return value that is created within Gamebase. |
| isSuccessful | boolean | Whether it is successful or not.  |
| resultCode | int | Result code<br>0 for success; return error codes, for failure |
| resultMessage | String | Result message  |

## Authentication

#### Token Authentication

Authenticates an Access Token issued to a login user. If it is normal, return information of a corresponding user.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.2/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID  |
| userId | String | ID of a login user  |
| accessToken | String |Access Token issued to a login user |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | True or false (Default is false) <br>Whether IdP-related information, used when Access Token is issued, is included or not |

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
| linkedIdP | Object | Login user's IdP information  |
| linkedIdP.idPCode | String | IdP information <br>e.g. Guest, PAYCO, and Facebook |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | User ID |
| member.lastLoginDate | String | Last login time ISO 8601 <br>Not available for first-time login user  |
| member.appId | String | appId |
| member.valid | String | Value of a successful login user is "Y" <br>(For description of other values, refer to member API.)) |
| member.regDate | long | Time when a user created an account |
| authList | Array[Object] | Information related to user-authenticated IdP.  |
| authList[].authSystem | String | Authentication system internally used within Gamebase <br>User authentication system to be provided. |
| authList[].idPCode | String | User-authenticated IdP information <br>e.g. Guest, PAYCO, and Facebook  |
| authList[].authKey | String | User separator issued at authSystem  |


**[Error Code]**

[Error Code](./error-code/#server)


## Launching

#### Get Simple Launching

In the console, you can view the launching information provided when starting up a client app, such as the server address, install URL, current maintenance status, maintenance time, and messages.
현재 점검 설정 여부 만을 확인하고 싶다면, [Check Maintenance] API를 사용하면 됩니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/launching/simple |

**[Request Header]**

Check Common Factors

**[Path Variable]**  

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST Project ID |

**[Request Parameter]**  

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | OsCode | true | OS code <br>AOS, IOS, WEB, WINDOWS |
| storeCode | Enum | true | Store Code <br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |
| clientVersion | String | true | Client version |

**[Response Body]**  

##### OK
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

##### Maintenance
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
| status | Object | Information which shows the current client status |
| status.code | int | Client status code <br><br>OK: 200 <br>Update recommended: 201, Update required: 300 <br>Service terminated: 302 <br>Maintenance in progress: 303 |
| status.message | String | Client status message |
| app | Object | App information |
| app.storeCode | String | App Store code <br>'GG', 'AS', etc. |
| app.accessInfo | Object | Information set on the console app screen |
| app.accessInfo.serverAddress | String | Server address<br>The server address set on the client side has a higher priority. <br>When no client server address is set, the server address set on the app screen is delivered. |
| app.accessInfo.csInfo | String | Customer Center information |
| app.relatedUrls | Object | In-app URL to be used within the app |
| app.relatedUrls.termsUrl | String | Terms and Conditions |
| app.relatedUrls.csUrl| String | Customer Center |
| app.relatedUrls.punishRuleUrl | String | Ban Rules |
| app.relatedUrls.personalInfoCollectionUrl | String | Privacy Information Agreement |
| app.install | Object | App Installation information |
| app.install.url | String | Install URL |
| maintenance | Object | Maintenance Information |
| maintenance.typeCode | String | Maintenance type code <br>Overall maintenance : 'SYSTEM', Maintenance per App: 'APP' |
| maintenance.beginDate | Date | Maintenance start date ISO 8601 |
| maintenance.endDate | Date | Maintenance end date ISO 8601 |
| maintenance.url | String | Maintenance URL |
| maintenance.reason | String | Maintenance reason |
| maintenance.message | String | Default maintenance reason message |

<br>

## Member

#### Get Member

Retrieve detailed information of a single member.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/{userId} |


**[Request Header]**

Check common requirements.


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String |  TOAST project ID |
| userId | String | User ID to retrieve |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | true or false (default value true) Including detailed information of user device, OS, etc. |

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
| member | Object | Basic information of a retrieved user|
| member.userId | String | User ID |
| member.valid | Enum | Y: Normal user <br>D: Withdrawn user  <br>B: Banned user <br>M: Missing account|
| member.appId | String | appId |
| member.regDate | String | Time when a user created an account   |
| member.lastLoginDate | String | Last login time <br>Not available for a first-time login user |
| member.authList | Array[Object] | Information related to user-authenticated IdP  |
| member.authList[].userId | String | User ID |
| member.authList[].authSystem | String |  Authentication system used internally within Gamebase <br>User authentication system to be provided |
| member.authList[].idPCode | String | User-authenticated IdP information <br>e.g. Guest, PAYCO, and Facebook |
| member.authList[].authKey | String |  User separator issued at authSystem   |
| member.authList[].regDate | String | Mapping time between IdP information with user account |
| memberInfo                   | Object        | Additional user information              |
| memberInfo.deviceCountryCode | String        | Country code of user device              |
| memberInfo.usmCountryCode    | String        | Country code of user USIM                |
| memberInfo.language          | String        | User language                            |
| memberInfo.osCode            | String        | OS type of user device                   |
| memberInfo.telecom           | String        | Telecommunication provider               |
| memberInfo.storeCode         | String        | Store code                               |
| memberInfo.network           | String        | Network environment <br>e.g. 3G and WiFi |
| memberInfo.deviceModel       | String        | Model name of user device                |
| memberInfo.osVersion         | String        | OS version of user device                |
| memberInfo.sdkVersion        | String        | SDK version                              |
| memberInfo.clientVersion     | String        | Client version                           |

**[Error Code]**

[Error Code](./error-code/#server)

#### Get Members

Retrieves brief information about multiple members.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID |

**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | User ID to retrieve <br>  ["userId", "userId", "userId",...]|

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
| memberList           | Array [Object] | Basic information of retrieved users     |
| memberList[].userId  | String         | User ID                                  |
| memberList[].valid   | Enum           | Y: Normal user <br>D: Withdrawn user <br>B: Banned user <br>M: Missing account |
| memberList[].appId   | String         | appId                                    |
| memberList[].regDate | String         | Time when a user created an account      |


**[Error Code]**

[Error Code](./error-code/#server)


#### Get IdP Information

Retrieve IdP information mapped with user ID.

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.2/apps/{appId}/auth/authKeys |

**[Request Header]**

Check common requirements.


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory |  User ID to retrieve <br>  ["userId", "userId", "userId",...] |

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
| result     | Array [Object] | Basic information of retrieved users <br>An object, with userId as key, and IdP information as value |
| authkey    | String         | User separator issued at authSystem      |
| IdPCode    | String         | User-authenticated IdP information <br>e.g. Guest, PAYCO, and Facebook |
| authSystem | String         | Authentication system used internally within Gamebase <br>Authentication system to be provided |

**[Error Code]**

[Error Code](./error-code/#server)

#### Get UserId Information with Auth key

Retrieve a user ID mapped to user authentication key.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Authentication system used internally within Gamebase <br>User authentication system to be provided  <br>Currently provides gbid |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authKeyList | Array[String] | mandatory | authKey issued at authSystem <br> ["authKey", "authKey", "authKey",...]|

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
| result | Array [Object] | Basic information of a retrieved user <br>An object with authKey as key, and useID as value. |

**[Error Code]**

[Error Code](./error-code/#server)

#### Ban Histories

Looks up users' ban history.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans |


**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST Project ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | Ban history query start time (ISO 8601 standard time, UTF-8 encoding required) <br>E.g. yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | Ban history query end time (ISO 8601 standard time, UTF-8 encoding required)<br>If banned between the start and end time, the query result shows this. |
| page | String | optional | Page to query about. Starting from 0 |
| size | String | optional | Number of data per page |


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
| pagingInfo | Object | Queried page information |
| pagingInfo.first | boolean | True if it is the first page |
| pagingInfo.last | boolean | True if it is the last page |
| pagingInfo.numberOfElements | int | Total number of data |
| pagingInfo.page | int | Page No. |
| pagingInfo.size | int | Number of data per page |
| pagingInfo.totalElements | int | Total number of data |
| pagingInfo.totalPages | int | Total number of pages |
| result | Array[Object] | Queried ban history details |
| result.appId | String | TOAST Project ID of the queried ban |
| result.banCaller | String | Subject of calling ban |
| result.banReason | String | Reason of ban |
| result.banType | String | Type of ban TEMPORARY or PERMANENT |
| result.beginDate | Long | Start date of ban epoch time|
| result.endDate | Long | End date of ban epoch time |
| result.flags | String | Returns 'Leaderboard' when you have selected Delete Leaderboard upon Registering Ban in the console. |
| result.message | String | Ban message |
| result.name | String | Template name registered in the console |
| result.regUser | String | Banned user |
| result.releaseCaller | String | Subject of unban |
| result.releaseDate | Long | Date of unban epoch time |
| result.releaseReason | String | Reason of unban |
| result.releaseUser | String | Unbanned user |
| result.seq | Long | Sequence number of ban history |
| result.templateCode | Long | Code value of ban template registered in the console |
| result.userId | String | User ID |

**[Error Code]**

[Error code](./error-code/#server)

#### Ban Release Histories.

Queries the user's unban history.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans/release |


**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST Project ID |
|   |   |   |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | Unban history query start time (ISO 8601 standard time, UTF-8 encoding required) <br>E.g. yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | Unban history query end time (ISO 8601 standard time, UTF-8 encoding required)<br>If unbanned between the start and end time, the query result shows this. |
| page | String | optional | Page to query about. Starting from 0 |
| size | String | optional | Number of data per page |


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
| pagingInfo | Object | Queried page information |
| pagingInfo.first | boolean | True if it is the first page |
| pagingInfo.last | boolean | True if it is the last page |
| pagingInfo.numberOfElements | int | Total number of data |
| pagingInfo.page | int | Page No. |
| pagingInfo.size | int | Number of data per page |
| pagingInfo.totalElements | int | Total number of data |
| pagingInfo.totalPages | int | Total number of pages |
| result | Array[Object] | Queried ban information |
| result.appId | String | TOAST Project ID of the queried ban |
| result.banCaller | String | Subject of calling ban |
| result.banReason | String | Reason of ban |
| result.banType | String | Type of ban TEMPORARY or PERMANENT |
| result.beginDate | String | Start date of ban epoch time |
| result.endDate | String | End date of ban epoch time |
| result.flags | String | Returns 'Leaderboard' when you have selected Delete Leaderboard upon Registering Ban in the console. |
| result.message | String | Ban message |
| result.name | String | Template name registered in the console |
| result.regUser | String | Banned user |
| result.releaseCaller | String | Subject of unban |
| result.releaseDate | String | Date of unban epoch time |
| result.releaseReason | String | Reason of unban |
| result.releaseUser | String | Unbanned user |
| result.seq | Long | Sequence number of ban history |
| result.templateCode | Long | Code value of ban template registered in the console |
| result.userId | String | User ID |

**[Error Code]**

[Error code](./error-code/#server)

#### Validate TransferAccount

Validates the ID and password issued for transferring the guest account. 유효한 TransferAccount인 경우 발급 받은 userId 정보를 리턴합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.1.2/apps/{appId}/members/transfer-account |


**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST Project ID |


**[Request Parameter]**

None


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
| account.id | String | ID to be validated |
| account.password | String | Password to be validated |

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
| member | Object | Basic information of the queried user |
| member.userId | String | User ID |
| member.valid | Enum | Y: Normal user <br>D: Withdrawn user <br>B: Banned user<br>M: Lost account|
| member.appId | String | App ID |
| member.regDate | String | The time when the user created the account |
| member.lastLoginDate | String | The last login time <br>The user who logged in for the first time has no value |

**[Error Code]**

[Error code](./error-code/#server)

<br>

## Maintenance

#### Check Maintenance Set

Check whether maintenance is currently set.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID |

**[Request Parameter]**

N/A

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
| underMaintenance       | Boolean | Whether maintenance is currently set     |
| maintenances           | Object  | Default maintenance information, if maintenance is set |
| maintenances.typeCode  | Enum    | APP: Maintenance set in a game <br>SYSTEM: Maintenance set by the Gamebase system |
| maintenances.beginDate | String  | Start time of maintenance. ISO 8601      |
| maintenances.endDate   | String  | End time of maintenance. ISO 8601        |
| maintenances.url       | String  | Detailed maintenance URL                 |
| maintenances.message   | String  | Maintenance message                      |
| maintenances.targetStores | Array[Enum] | 특정 클라이언트에 대해서만 점검 설정시, 점검 설정된 클라이언트의 스토어코드<br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |

**[Error Code]**

[Error Code](./error-code/#server)

<br>

## Coupon

#### Check Validation And Consume Coupon

콘솔을 통해 발급된 쿠폰 코드에 대해 유효성 검증 및 쿠폰 상태를 변경 합니다. 유효한 쿠폰인 경우 소비 상태로 변경을 하고, 응답 결과로 지급할 아이템 정보를 리턴합니다.

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

없음

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

Google Play Store, App Store, ONEStore 등 스토어 결제가 완료된 후에 유저에게 아이템을 지급하기 전에 결제를 소비 할 것을 알려야 합니다. 결제 1건당 1번만 결제소비 가능하며, 결제의 상태가 정상이 아니면 소비되지 않습니다.
(결재 소비가 완료 되었다면 유저의 결제 및 아이템 지급이 정상적으로 완료 되었다고 판단)

소비 (Consume) 하지 않은 결제내역은 SDK 및 서버의 미소비 결제 내역조회 API를 통해 조회할 수 있습니다. 참고로 아이템 등록시 상품 유형이 일회성(CONSUMABLE)인 아이템 결제건에 대해서만 consume 처리 됩니다.

> [참고]
> 결제 1건당 1번 소비 가능하며, 결제소비 하지 않은 결제는 IAP에서 아이템을 지급하지 않은 것으로 간주합니다.

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
| accessToken | String | mandatory  | 결제 인증 토큰 (로그인 인증 토큰이 아님)  |

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
| result.price | Long | 결제 가격 |
| result.currency  | String  | 결제 통화  |
| result.productSeq | Long | 결제 아이템 번호 (console에 등록된 아이템 고유 번호) |

**[Error Code]**

[오류 코드](./error-code/#server)

#### Get Consumable List

결제가 완료되었지만 아직 소비(Consume)되지 않은, 미소비 결제내역을 조회할 수 있습니다.

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
| marketId | String | mandatory | 스토어코드<br>GG : Google, AS : Apple, ONESTORE : 원스토어 |
| userChannel | String | mandatory  | 유저 채널<br>현재는 미구현 상태로 항상 `GF` 값을 설정  |
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
| result[].productSeq | Long | 결제 아이템 번호 (console에 등록된 아이템 고유 번호) |
| result[].currency  | String  | 결제 통화  |
| result[].price | Long | 결제 가격 |
| result[].accessToken | String | 결제 인증 토큰 |

**[Error Code]**

[오류 코드](./error-code/#server)

### Get ActiveSubscription List

유저가 현재 구독중인 결제를 조회 할 수 있습니다.

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
| marketId | String | mandatory | 스토어코드<br>GG : Google, AS : Apple, ONESTORE : 원스토어 |
| packageName | String | mandatory | 콘솔에 등록한 앱의 packageName |
| userChannel | String | mandatory  | 유저 채널<br>현재는 미구현 상태로 항상 `GF` 값을 설정  |
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
| result[].productSeq | Long | 결제 아이템 번호 (console에 등록된 아이템 고유 번호) |
| result[].currency  | String  | 결제 통화  |
| result[].price | Long | 결제 가격 |
| result[].paymentId | String | 최근 갱신된 스토어 결제 번호 |
| result[].originalPaymentId | String | 최초 스토어 결제 번호 |
| result[].purchaseTimeMillis | Long | 최근 갱신된 시간 |
| result[].expiryTimeMillis | Long | 구독 만료 시간 |


**[Error Code]**

[오류 코드](./error-code/#server)

<br>

## Leaderboard

Gamebase provides Wrapping to server API of TOAST Leaderboard. With Wrapping, TOAST products become available at a user server on a consistent interface.


#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Retrieve User Count Registered at Factor | GET    | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| Retrieve Score/Rank of a Single User     | GET    | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| Retrieve Scores/Ranks of Multiple Users  | POST   | /tcgb-leaderboard/v1.2/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| Retrieve Entire Scores/Ranks of Range    | GET    | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| Register Score of a Single User          | POST   | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| Register Score/ExtraData of a Single User | POST   | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| Register Scores of Multiple Users        | POST   | /tcgb-leaderboard/v1.2/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| Register Scores/ExtraData of Multiple Users | POST   | /tcgb-leaderboard/v1.2/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| Delete Leaderboard Information of a Single User | DELETE | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**For more information of the API, click the following link.**


[Leaderboard Guide](/Game/Leaderboard/en/api-guide/)

##### Example of API Call

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count
```

<br>

## Others

### Support

To inquire about causes of failure in API call, upload **API Call URL (with HTTP body, if available) along with response results** to [Customer Center](https://cloud.toast.com/support/faq), and we'll respond ASAP.



##### Example of API Call

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.2/apps/C3JmSctU/maintenances/under-maintenance
```

##### Result of Failed API Response

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
