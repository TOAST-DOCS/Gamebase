## Game > Gamebase > API Guide

Gamebase Server API provides APIs as follows, in the RESTful format.  

## Advance Notice

Following information is required to use Server API. 

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
| transactionId | String | alue set at HTTP Header when API is requested. <br>If the value is not delivered, return value that is created within Gamebase. |
| isSuccessful | boolean | Whether it is successful or not.  |
| resultCode | int | Result code <br>0 for success; return error codes, for failure |
| resultMessage | String | Result message  |


## Authentication

#### Token Authentication

Authenticates an Access Token issued to a login user. If it is normal, return information of a corresponding user.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.0/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

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
| linkedIdP | Object | Login user's IdP information  |
| linkedIdP.idPCode | String | IdP information <br>e.g. Guest, PAYCO, and Facebook |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | User ID |
| member.lastLoginDate | long | Last login time  <br>Not available for first-time login user  |
| member.appId | String | appId |
| member.valid | String | Value of a successful login user is "Y" <br>(For description of other values, refer to member API.)) |
| member.regDate | long | Time when a user created an account |
| authList | Array[Object] | Information related to user-authenticated IdP.  |
| authList[].authSystem | String | Authentication system internally used within Gamebase <br>User authentication system to be provided. |
| authList[].idPCode | String | User-authenticated IdP information <br>e.g. Guest, PAYCO, and Facebook  |
| authList[].authKey | String | User separator issued at authSystem  |


**[Error Code]**

[Error Code](./error-code/#server)


## Member

#### Get Member

Retrieve detailed information of a single member.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.0/apps/{appId}/members/{userId} |


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
| member | Object | Basic information of a retrieved user|
| member.userId | String | User ID |
| member.valid | Enum | Y: Normal user <br>D: Withdrawn user  <br>B: Banned user <br>M: Missing account|
| member.appId | String | appId |
| member.regDate | long | Time when a user created an account   |
| member.lastLoginDate | long | Last login time <br>Not available for a first-time login user |
| member.authList | Array[Object] | Information related to user-authenticated IdP  |
| member.authList[].userId | String | User ID |
| member.authList[].authSystem | String |  Authentication system used internally within Gamebase <br>User authentication system to be provided |
| member.authList[].idPCode | String | User-authenticated IdP information <br>e.g. Guest, PAYCO, and Facebook |
| member.authList[].authKey | String |  User separator issued at authSystem   |
| member.authList[].regDate | long | Mapping time between IdP information with user account |
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
| POST | /tcgb-member/v1.0/apps/{appId}/members |

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
		"regDate": 1488185201000
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
| memberList[].regDate | Long           | Time when a user created an account      |


**[Error Code]**

[Error Code](./error-code/#server)

<br>

#### Get IdP Information

Retrieve IdP information mapped with user ID. 

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.0/apps/{appId}/auth/authKeys |

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
| POST | /tcgb-member/v1.0/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


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

## Maintenance

#### Check Maintenance Set

Check whether maintenance is currently set.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.0/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

Check common requirements. 

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST project ID |

**[Request Parameter]**

N/A 
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
| underMaintenance       | Boolean | Whether maintenance is currently set     |
| maintenances           | Object  | Default maintenance information, if maintenance is set |
| maintenances.typeCode  | Enum    | APP: Maintenance set in a game <br>SYSTEM: Maintenance set by the Gamebase system |
| maintenances.beginDate | String  | Start time of maintenance. ISO 8601      |
| maintenances.endDate   | String  | End time of maintenance. ISO 8601        |
| maintenances.url       | String  | Detailed maintenance URL                 |
| maintenances.message   | String  | Maintenance message                      |


**[Error Code]**

[Error Code](./error-code/#server)

## Purchase(IAP)

Gamebase provides **Wrapping** to Server API of TOAST IAP. With Wrapping, TOAST products become available at a user server on a consistent interface.  

#### Wrapping API

| API | Method | Wrapping URI | IAP URI |
| --- | --- | --- | --- |
|  Consume Items    | POST | /tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq} | /inapp/v3/consume/{paymentSeq}/items/{itemSeq} |
|  Retrieve Items     | GET | /tcgb-inapp/v1.0/apps/{appId}/item/list/{appSeq} | /standard/item/list/{appSeq} |
| Retrieve List of Non-consumed Purchases| POST | /tcgb-inapp/v1.0/apps/{appId}/consumable/list | /standard/inapp/v1/consumable/list |

**For more information of the API, click the following link.**

[IAP Guide](/Mobile%20Service/IAP/en/api-guide/)

##### Example of API Call 

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

POST https://api-gamebase.cloud.toast.com/tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq}
```

## Leaderboard

Gamebase provides Wrapping to server API of TOAST Leaderboard. With Wrapping, TOAST products become available at a user server on a consistent interface. 

#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Retrieve User Count Registered at Factor | GET    | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| Retrieve Score/Rank of a Single User     | GET    | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| Retrieve Scores/Ranks of Multiple Users  | POST   | /tcgb-leaderboard/v1.0/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| Retrieve Entire Scores/Ranks of Range    | GET    | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| Register Score of a Single User          | POST   | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| Register Score/ExtraData of a Single User | POST   | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| Register Scores of Multiple Users        | POST   | /tcgb-leaderboard/v1.0/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| Register Scores/ExtraData of Multiple Users | POST   | /tcgb-leaderboard/v1.0/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| Delete Leaderboard Information of a Single User | DELETE | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**For more information of the API, click the following link.**

[Leaderboard Guide](/Game/Leaderboard/en/api-guide/)

##### Example of API Call  

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count
```

## Others 

### Support

To inquire about causes of failure in API call, upload **API Call URL (with HTTP body, if available) along with response results** to [Customer Center](https://cloud.toast.com/support/faq), and we'll respond ASAP. 

##### Example of API Call  

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance
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
      "uri": "/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful": false
  }
}

```

