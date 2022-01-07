## Game > Gamebase > API v1.3 Guide

## Updates
- IAP(In App Purchase) New items have been added to or deleted from the request parameters and response results of API.
- Added Push Wrapping API.
- Added the "Get IdP Token and Profiles" API capable of acquiring the profile and token info of the IdP used for logging in with the Gamebase Access Token.
- Added the "Get UserId Information with IdP Id" API which acquires the Gamebase userId mapped with IdP Id.

## Advance Notice

Gamebase Server API provides APIs as follows, in the RESTful format. Following information is required to use Server API.

#### Server Address

To call API, below address is needed, which is also available in the Gamebase Console.
> https://api-gamebase.cloud.toast.com

![image alt](https://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

App ID, as a project ID of NHN Cloud, can be found on the **Project List** page of the Console.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

Secret Key, as a control access of API, can be found in the Gamebase Console. It must be set at the HTTP header to call Server API.
> [Note]<br>
> When a secret key is exposed and a wrong call is made, click **Create** to create a new secret key and replace the old one.

![image alt](https://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

AS part of managing API internally within a server that calls API, TransactionId is provided. By setting a transaction ID at the HTTP header from a calling server to call API, the Gamebase server delivers results with corresponding TransactionId set at the response HTTP Header and Response Body Header of results.

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
    "header": {
        "transactionId": "88a1ae42-6b1d-48c8-894e-54e97aca07fq",
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "Success."
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| transactionId | String | The value set at HTTP Header when API is requested.<br>If the value is not delivered, return value that is created within Gamebase. |
| isSuccessful | boolean | Whether it is successful or not.  |
| resultCode | int | Result code<br>0 for success; return error codes, for failure |
| resultMessage | String | Result message  |

<br>
<br>

## Authentication

#### Token Authentication

Authenticates an Access Token issued to a login user. If it is normal, returns information of the corresponding user.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID  |
| userId | String | ID of the logged-in user  |
| accessToken | String | Access Token issued to the logged-in user |

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
        ],
        "temporaryWithdrawal": {
            "gracePeriodDate": "2020-04-18T09:12:01+09:00"
        }
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| linkedIdP | Object | Logged-in user's IdP information  |
| linkedIdP.idPCode | String | [User authentication IdP](#identity-provider-code]) |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | User ID |
| member.lastLoginDate | String | Last login time ISO 8601 <br>Not available for a first-time login user  |
| member.appId | String | appId |
| member.valid | String | [User status](#member-valid-code)<br>Value of a successful login user is "Y" |
| member.regDate | String | Time when a user created an account |
| authList | Array[Object] | Information related to user authentication IdP  |
| authList[].authSystem | String | Authentication system internally used within Gamebase <br>User authentication system to be provided |
| authList[].idPCode | String | [User authentication IdP](#identity-provider-code])  |
| authList[].authKey | String | User classification value issued per IdP ID by authSystem  |
| temporaryWithdrawal | Object | Information related to pending withdrawal <br>Provided only when the value of valid is "T" |
| temporaryWithdrawal.gracePeriodDate | String | Expiration time for pending withdrawal ISO 8601 |

**[Error Code]**

[Error Code](./error-code/#server)

<br/>
#### Get IdP Token and Profiles

Gamebase Access Token which is issued upon successful login with "Login with IdP" from the client side. It retrieves the Access Token and Profiles information of the IdP used for login.

> [Caution]
> IdP's Access Token expiration date varies by IdP, and they are usually short.
> If you successfully log in with "Login as the Latest Login IdP" from the client and call the API from the server, you may not be able to acquire IdP info because IdP's Access Token has been expired.

<br/>

> [Note]
> There are also IdPs that are unable to acquire info with the IdP's Access Token only.
> e.g. appleid / iosgamecenter : there are no information you can retrieve from Server to Server with Access Token.

<br/>

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/idps/{idPCode}?accessToken={accessToken} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |
| userId | String | ID of the logged-in user |
| idPCode | String | [User authentication IdP](#identity-provider-code]) |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| accessToken | String | mandatory | Gamebase Access Token issued to the logged-in user |

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
| idPProfile | Map<String, Object> | IdP's profile used by the logged-in user<br>- All IdPs have different response formats |
| idPToken | Object | Access Token information of IdP used by the logged-in user |
| idPToken.idPCode | String | [User authentication IdP](#identity-provider-code]) |
| idPToken.accessToken | String | IdP Access Token |
<br>
<br>

## Launching

#### Get Simple Launching

In the console, you can view the launching information provided when starting up a client app, such as the server address, install URL, current maintenance status, maintenance time, and messages.
To check only if the current maintenance setting is enabled, use [Check Maintenance] API.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/launching/simple |

**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud Project ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | OsCode | true | [OS code](#os-code) |
| storeCode | Enum | true | [Store code](#store-code]) |
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
| app.storeCode | String | [Store code](#store-code]) |
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

**[Error Code]**

[Error code](./error-code/#server)](./error-code/#server)

<br>
<br>

## Member

#### Get Member

Retrieves detailed information of a single user.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/{userId} |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String |  NHN Cloud project ID |
| userId | String | User ID to retrieve |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | true or false (default value: true) Whether to include detailed information of user device, OS, etc. |

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
| member | Object | Basic information of a retrieved user|
| member.userId | String | User ID |
| member.valid | Enum | [User status](#member-valid-code) |
| member.appId | String | appId |
| member.regDate | String | Time when a user created an account   |
| member.lastLoginDate | String | Last login time <br>Not available for a first-time login user |
| member.authList | Array[Object] | Information related to user authentication IdP  |
| member.authList[].userId | String | User ID |
| member.authList[].authSystem | String |  Authentication system used internally within Gamebase <br>User authentication system to be provided |
| member.authList[].idPCode | String | [User authentication IdP](#identity-provider-code]) |
| member.authList[].authKey | String |  User classification value issued per IdP ID by authSystem   |
| member.authList[].regDate | String | Time when the IdP information was mapped with the user account |
| temporaryWithdrawal | Object | Information related to pending withdrawal <br>Provided only when the value of valid is "T" |
| temporaryWithdrawal.gracePeriodDate | String | Expiration time for pending withdrawal ISO 8601 |
| memberInfo                   | Object        | Additional user information              |
| memberInfo.deviceCountryCode | String        | Country code of user device              |
| memberInfo.usmCountryCode    | String        | Country code of user USIM                |
| memberInfo.language          | String        | User language                            |
| memberInfo.osCode            | String        | [OS code](#os-code)                      |
| memberInfo.telecom           | String        | Telecommunication provider               |
| memberInfo.storeCode         | String        | [Store code](#store-code])               |
| memberInfo.network           | String        | Network environment <br>e.g. 3G and WiFi |
| memberInfo.deviceModel       | String        | Model name of user device                |
| memberInfo.osVersion         | String        | OS version of user device                |
| memberInfo.sdkVersion        | String        | SDK version                              |
| memberInfo.clientVersion     | String        | Client version                           |

**[Error Code]**

[Error Code](./error-code/#server)

<br>

#### Get Members

Retrieves brief information about multiple users.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | User ID to retrieve |

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
| memberList[].valid   | Enum           | [User status](#member-valid-code)        |
| memberList[].appId   | String         | appId                                    |
| memberList[].regDate | String         | Time when a user created an account      |

**[Error Code]**

[Error Code](./error-code/#server)

<br>

#### Get IdP Information

Retrieves IdP information mapped with user ID.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/auth/authKeys |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | User ID to retrieve |

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
| result     | Array [Object] | Basic information of retrieved users <br>An object with userId as key and IdP information as value |
| authkey    | String         | User classification value issued per IdP ID by authSystem      |
| IdPCode    | String         | [User authentication IdP](#identity-provider-code]) |
| authSystem | String         | Authentication system used internally within Gamebase <br>Authentication system to be provided |

**[Error Code]**

[Error Code](./error-code/#server)

<br>

#### Get UserId Information with Auth key

Retrieves a user ID mapped to user authentication key.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Authentication system used internally within Gamebase <br>User authentication system to be provided  <br>Currently provides gbid |

**[Request Body]**

```json
["authKey", "authKey", "authKey"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | authKey issued at authSystem |

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

<br>

#### Get UserId Information with IdP Id

Retrieves the information of user ID mapped with IdP ID.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/idps/{idPCode}/members |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |
| idPCode | String | [User authentication IdP](#identity-provider-code]) |

**[Request Body]**

```json
["idPId", "idPId", "idPId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | IdP ID of users to retrieve <br> Max size of the list to be searched is 300 |

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
| result | Map<String, String> | ID info of the retrieved users <br>- IdP ID is the key, and Gamebase userId is the value<br>- If the userID info containing the IdP ID requested for retrieval does not exist, it won't be available in the response result. |

**[Error Code]**

[Error code](./error-code/#server)

<br>

#### Ban Histories

Retrieves the user ban history.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans |

**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud Project ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | Ban history query start time (ISO 8601 standard time, UTF-8 encoding required) <br>E.g. yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | Ban history query end time (ISO 8601 standard time, UTF-8 encoding required)<br>If banned between the start and end time, the query result shows this. |
| page | String | optional | Page to retrieve, starting from 0 |
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
| pagingInfo | Object | Retrieved paging information |
| pagingInfo.first | boolean | True if it is the first page |
| pagingInfo.last | boolean | True if it is the last page |
| pagingInfo.numberOfElements | int | Total number of data |
| pagingInfo.page | int | Page No. |
| pagingInfo.size | int | Number of data per page |
| pagingInfo.totalElements | int | Total number of data |
| pagingInfo.totalPages | int | Total number of pages |
| result | Array[Object] | Retrieved ban history details |
| result.appId | String | NHN Cloud Project ID of the retrieved ban |
| result.banCaller | String | Subject of calling ban |
| result.banReason | String | Reason of ban |
| result.banType | String | Type of ban. TEMPORARY or PERMANENT |
| result.beginDate | Long | Start date of ban|
| result.endDate | Long | End date of ban |
| result.flags | String | Returns 'Leaderboard' when you have selected Delete Leaderboard upon Registering Ban in the console. |
| result.message | String | Ban message |
| result.name | String | Template name registered in the console |
| result.regUser | String | Banned user |
| result.releaseCaller | String | Subject of ban release |
| result.releaseDate | Long | Date of ban release |
| result.releaseReason | String | Reason of ban release |
| result.releaseUser | String | Unbanned user |
| result.seq | Long | Sequence number of ban history |
| result.templateCode | Long | Code value of ban template registered in the console |
| result.userId | String | User ID |

**[Error Code]**

[Error code](./error-code/#server)

<br>

#### Ban Release Histories.

Retrieves the user ban release history.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans/release |

**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud Project ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | Ban release history query start time (ISO 8601 standard time, UTF-8 encoding required) <br>E.g. yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | Ban release history query end time (ISO 8601 standard time, UTF-8 encoding required)<br>If unbanned between the start and end time, the query result shows this. |
| page | String | optional | Page to retrieve, starting from 0 |
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
| pagingInfo | Object | Retrieved paging information |
| pagingInfo.first | boolean | True if it is the first page |
| pagingInfo.last | boolean | True if it is the last page |
| pagingInfo.numberOfElements | int | Total number of data |
| pagingInfo.page | int | Page No. |
| pagingInfo.size | int | Number of data per page |
| pagingInfo.totalElements | int | Total number of data |
| pagingInfo.totalPages | int | Total number of pages |
| result | Array[Object] | Retrieved ban information |
| result.appId | String | NHN Cloud Project ID of the retrieved ban |
| result.banCaller | String | Subject of calling ban |
| result.banReason | String | Reason of ban |
| result.banType | String | Type of ban. TEMPORARY or PERMANENT |
| result.beginDate | String | Start date of ban |
| result.endDate | String | End date of ban |
| result.flags | String | Returns 'Leaderboard' when you have selected Delete Leaderboard upon Registering Ban in the console. |
| result.message | String | Ban message |
| result.name | String | Template name registered in the console |
| result.regUser | String | Banned user |
| result.releaseCaller | String | Subject of ban release |
| result.releaseDate | String | Date of ban release |
| result.releaseReason | String | Reason of ban release |
| result.releaseUser | String | Unbanned user |
| result.seq | Long | Sequence number of ban history |
| result.templateCode | Long | Code value of ban template registered in the console |
| result.userId | String | User ID |

**[Error Code]**

[Error code](./error-code/#server)

<br>

#### Validate TransferAccount

Validates the ID and password issued for transferring the guest account. For valid TransferAccount, return issued userID information.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/transfer-account |

**[Request Header]**

Check Common Factors

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud Project ID |

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
| member | Object | Basic information of the retrieved user |
| member.userId | String | User ID |
| member.valid | Enum | [User status](#member-valid-code) |
| member.appId | String | App ID |
| member.regDate | String | The time when the user created the account |
| member.lastLoginDate | String | The last login time <br>Not available for a first-time login user |

**[Error Code]**

[Error code](./error-code/#server)

<br>

#### Withdraw

Withdraws a user account.

> [Note]
> If the account withdrawal has been implemented using the server withdraw API instead of the SDK's withdrawal API, the client needs to delete data such as cached tokens by calling the logout API of SDK after the successful withdrawal.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}?regUser={regUser} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |
| userId | String | ID of the withdrawal target user |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | mandatory | The system or user information of the entity that requested withdrawal <br> - The information can be viewed from Console > 'Withdrawal History' in the 'Member' page <br> - The withdrawal history can only be viewed by the withdrawn user |

**[Request Body]**

None

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

[Error code](./error-code/#server)

<br>
<br>

## Maintenance

#### Check Maintenance Set

Checks whether maintenance is currently set.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

Check common requirements.

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

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
| maintenances.targetStores | Array[Enum] | Store code of a client for the maintenance setting of a particular client only <br>- GG: Google<br>- ONESTORE: ONE store<br>- AS: AppStore |

**[Error Code]**

[Error Code](./error-code/#server)

<br>
<br>

## Coupon

#### Check Validation And Consume Coupon

Validates published coupon code and change coupon status via console. For valid coupons, change to consume status and return item information to be paid as response result.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/coupons/{couponCode}?storeCode={storeCode} |

**[Request Header]**

Check common issues

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |
| userId | String | User ID to use coupons |
| couponCode | String | Coupon code |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| storeCode | String | optional | If you set the coupon to be used only for apps installed from a specific store in the console, you must pass the store code<br>- If it's usable at all stores, set the parameter to ALL or omit the parameter<br>- [Store code](#store-code]) |

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
| result | Object | Coupon Information |
| result.title | String | Coupon name |
| result.benefits | Array[Object] | Information of item to be provided |
| result.benefits.itemId | String | Item ID |
| result.benefits.amount | Integer | Item count |

**[Error Code]**

[Error Code](./error-code/#server)

<br>
<br>

## Purchase (IAP)

#### Consume

If the store payment (Google Play Store, App Store, ONEStore, etc.) has successfully been made, it issues the purchased items to the user, records the purchase history in the server, and then informs the Gamebase of the payment consumption. You can consume payment only once per payment, and the payment is not consumed if the payment status is not normal.

> [Note]
> Only the item payment with the product type CONSUMABLE at the time of registration will be consumed.
> Can consume once per payment, and IAP regards any payment without consumption as not issuing the purchased item.

Unconsumed payment history can be viewed through SDK and View Unconsumed Payment History API. Even if the unconsumed payment history exists through the API, the provisioning history within the game server becomes the priority if the game server has the history about the item provisioning.
(If an API timeout occurs due to a network failure, etc., there might be cases where payment is completed in Gamebase whereas the item is not issued to the user due to API response failure in the game server.)

> [Note]
> If the game cannot manage all item issuance history internally, at least set the API request timeout to 10 sec or longer and record the history at the time of API timeout as safety measures to resolve issues regarding repeated/failed issuance of items.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consume |

**[Request Header]**

Check common issues

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Parameter]**

N/A

**[Request Body]**

```json
{
    "paymentSeq": "2019091931571201",
    "accessToken": "90fD1bs1guXwY6aZ7rseEKYW_6gMCISjDASgten4MD6O7XZD7VRjZcs8OTm8lOQVFTegoY4WK78P2WQCMm7cx"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | mandatory | Payment number |
| accessToken | String | mandatory  | Payment authentication token (not a login authentication token) |

> [Note]
> When client calls requestPurchase API, the purchaseToken for response is used as accessToken

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
| result | Object | Basic payment information |
| result.price | Float | Payment price |
| result.currency  | String  | Payment currency |
| result.productSeq | Long | Payment item number (original item number registered on console) |
| result.marketId | String | [Store code](#store-code]) |
| result.gamebaseProductId | String | Gamebase product ID<br>The value to be entered by user when registering products using the console |
| result.payload | String | Additional information configured in SDK |

> [Note]
> The gamebaseProductId value exists in the response result according to the SDK version and payment API used by the client.

> [Note]
> In the game server, specified products (items) can be issued using the item number or store code together with the Gamebase product ID, but if multiple Gamebase products are registered with a single store item ID, the products need to be issued using the store code and Gamebase product ID.

**[Error Code]**

[Error Code](./error-code/#server)

<br>

#### List Consumables

Lists non-consumed payment, which is not consumed even if paid up.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consumable |

**[Request Header]**

Check common issues

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Parameter]**

N/A

**[Request Body]**

```json
{
    "marketId": "GG",
    "userId": "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | mandatory | [Store code](#store-code]) |
| userId | String | mandatory  | User ID |

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
| result | Array[Object] | Basic payment information |
| result[].paymentSeq | String  | Payment number |
| result[].productSeq | Long | Payment item number (original item number registered on console) |
| result[].currency  | String  | Payment currency |
| result[].price | Float | Payment price |
| result[].accessToken | String | Payment authentication token |
| result[].marketId | String | [Store code](#store-code]) |
| result[].gamebaseProductId | String | Gamebase product ID<br>The value to be entered by user when registering a product using the console |
| result[].purchaseTime | String | Time and date of payment |
| result[].payload | String | Additional information configured in SDK |

**[Error Code]**

[Error Code](./error-code/#server)

<br>

### List Active Subscriptions

Lists payment of user's current subscriptions.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/active-subscriptions |

**[Request Header]**

Check common issues

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud project ID |

**[Request Parameter]**

N/A

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
| marketId | String | mandatory | [Store code](#store-code]) |
| packageName | String | mandatory | packageName of the app registered on console |
| userId | String | mandatory  | User ID |

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
            "payload": "additional info",
            "purchaseTime": "2020-06-02T13:38:56+09:00",
            "expiryTime": "2020-06-02T13:48:56+09:00"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | Basic payment information |
| result[].marketId  | String  | [Store code](#store-code]) |
| result[].userId  | String  | User ID |
| result[].paymentSeq | String  | Payment number |
| result[].accessToken | String | Payment validation token |
| result[].productSeq | Long | Payment item number (original item number registered on console) |
| result[].productId | String  | Identifier of product (item) registered at store |
| result[].productType | String  | Product (item) type <br>Subscription: AUTO_RENEWABLE |
| result[].currency  | String  | Payment currency |
| result[].price | Float | Payment price |
| result[].paymentId | String | Recently updated store payment number |
| result[].gamebaseProductId | String | Gamebase product ID<br>The value to be entered by user when registering a product using the console |
| result[].payload | String | Additional information configured in SDK |
| result[].purchaseTime | String | Recent updated time |
| result[].expiryTime | String | Subscription expiration time |

**[Error Code]**

[Error Code](./error-code/#server)

<br>
<br>

## Leaderboard

Gamebase provides Wrapping to server API of NHN Cloud Leaderboard. With Wrapping, NHN Cloud products become available at a user server on a consistent interface.

> [Note]
> Once the Gamebase is activated, you can call Gamebase Wrapping API to use the Leaderboard function without setting the Leaderboard Appkey.

<br>

#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Get user count in factor<br>- Get user count in factor | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| Get total factor count <br>- Get total factor count | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factor-count | /leaderboard/v2.0/appkeys/{appKey}/factor-count |
| Get factor info<br>- Get factor info<br>- Get multiple factor info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors | /leaderboard/v2.0/appkeys/{appKey}/factors |
| Get single user info (score/ranking)<br>- Get single user info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| Get multiple user info (score/ranking)<br>- Get multiple user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| Get the entire info (score/ranking) by range<br>- Get multiple user info by range | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| Get selected rank user info<br>- Get selected rank user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |
| Get ranking of a specific user or upper and lower users<br>- Get multiple user info by pivot user | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} | /leaderboard/v2.0/appkeys/{appkey}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} |
| Set single user score<br>- Set single user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| Set single user score with extra data<br>- Set single user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| Set multiple user score<br>- Set multiple user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| Set multiple user score with extra data<br>- Set multiple user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| Delete user leaderboard information<br>- Delete single user info<br>- Delete multiple user info | DELETE | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |

<br/>

**For more information of the API, click the following link.**
To find out about Leaderboard API specs mapped with Gamebase Wrapping API, see the following guide.
Use the Gamebase AppId and SecretKey to call the Gamebase Wrapping Leaderboard API without setting the Leaderboard Appkey.


[Leaderboard Guide](/Game/Leaderboard/en/api-guide/)

<br/>

##### Example of API Call

```
GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count

Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
```

<br/>
<br/>

## Push

Gamebase provides **Wrapping** function for the Server API of the NHN Cloud Push service. By using the Wrapping function, you can use the NHN Cloud services on the user server with consistent interfaces.

> [Note]
> Once the Gamebase is activated, you can call the Gamebase Wrapping API to use the Push function without setting the Push Appkey.


<br>

#### Wrapping API
|    | API | Method | Wrapping URI | Push URI |
| --- | --- | --- | --- | --- |
| Message | Send | POST | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | View | GET | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | View sent log | GET | /tcgb-push/v1.3/apps/{appId}/logs/message | /push/v2.4/appkeys/{appkey}/logs/message |
| Scheduled message | Create send schedule | POST | /tcgb-push/v1.3/apps/{appId}/schedules | /push/v2.4/appkeys/{appkey}/schedules |
|   | Create | POST | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | View list | GET | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | View a single item | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id} | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id} |
|   | View sent ones | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id}/messages | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id}/messages |
|   | Modify | PUT | /tcgb-push/v1.3/apps/{appId}/reservations/{reservationId} | /push/v2.4/appkeys/{appkey}/reservations/{reservationId} |
|   | Delete | DELETE | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |

<br/>

**For more information of the API, click the following link.**
To find out about the Push API spec mapped with Gamebase Wrapping API, see the following guide.
Use the Gamebase AppId and SecretKey to call the Gamebase Wrapping Push API without setting the Push Appkey.

> [Notes]
> User can use the gamebase userId value for the uid value exists in Push guide. When registering push token in client SDK, the user identifier is registered as gamebase userId.
> When a single user allows all push notifications on multiple devices, the user will receive all pushes on multiple devices.

[Push Guide](/Notification/Push/en/api-guide/)

<br/>

##### Example of API Call

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
    "removeGuide": "Menu > Setting",
    "timeToLiveMinute": 1,
    "provisionedResourceId": "id",
    "adWordPosition": "TITLE"
}
```

<br/>
<br/>

## Others

### OS Code

The code defined internally by Gamebase for the OS of the user device.

| Code | Description |
| --- | --- |
| AOS | Android |
| IOS | iOS |
| WEB | Web |
| WINDOWS | Windows |
<br/>

### Store Code

The code defined internally by Gamebase for the store where the app is installed.

| Code | Description |
| --- | --- |
| GG | Google Play Store |
| AS | App Store |
| ONESTORE | ONE store |
| GALAXY | Galaxy Store |
<br/>

### Identity Provider Code

The code defined internally by Gamebase for the Identity Providers used for user authentication.

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

The code defined internally by Gamebase for the user's current status.

| Code | Description |
| --- | --- |
| Y | Normal user |
| D | Withdrawn user |
| B | Banned user |
| T | Withdrawal-suspended user |
| P | Ban-suspended user |
| M | Missing account |
<br/>


### Support

To inquire about causes of failure in API call, upload **API call URL (with HTTP body, if available) along with response results** to [Customer Center](https://toast.com/support/inquiry), and we'll respond as soon as possible.

##### Example of API Call

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance
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
            "uri": "/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance"
        },
        "isSuccessful": false
    }
}
```
