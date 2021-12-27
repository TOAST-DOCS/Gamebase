## Game > Gamebase > API v1.3指南

## 更改事项
- 在IAP(In App Purchase)API的请求参数和响应结果中已添加并删除了新的项目。
- 已添加Push Wrapping API。
- 添加了使用Gamebase Access Token可获取登录时使用的IdP Profiles和令牌信息的"Get IdP Token and Profiles" API。
- 添加了用IdP Id获取进行映射的Gamebase userId的"Get UserId Information with IdP Id" API。

## Advance Notice

Gamebase Server API以RESTful类型提供如下API。为了使用服务器API，应了解以下信息。

#### 服务器地址

调用API的服务器地址如下。此地址也可以在Gamebase控制台页面中确认。
> https://api-gamebase.cloud.toast.com

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

可通过NHN Cloud项目ID，在APP菜单页面上确认APP ID。

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

密钥(secret key)是API的访问控制方法，可在Gamebase Console进行确认。调用Server API时，在HTTP标头中将密钥设置为必须。
> [参考]
> 如果密钥被泄露发生了无效调用，则可通过点击**创建**按钮创建新密钥后使用新秘钥。

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

作为调用API的服务器内部管理API请求，提供TransactionId功能。调用API的服务器发送HTTP请求时，在Head中设置Transaction ID，Gamebase服务器也将在响应HTTP Header和响应结果的Response Body Header中设置TransactionId发送结果给调用API的服务器。

## Common

#### HTTP Header

API调用时在HTTP Header中设置以下项目。

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |参考SecretKey说明 |
| X-TCGB-Transaction-Id | optional | 参考TransactionId说明 |

#### API响应

传递**HTTP 200 OK**以响应所有API请求。可以通过Response Body的Header项目来判断API请求是否成功。

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
| transactionId | String | API请求时在HTTP Header设定的值。<br>如果不传递此值，则返回Gamebase内部生成的值。|
| isSuccessful | boolean | 成功与否 |
| resultCode | int | 响应代码<br>成功时为0，失败时返还错误代码。 |
| resultMessage | String | 响应消息 |

<br>
<br>

## Authentication

#### 令牌认证

检查发给登录用户的访问令牌是否有效。如果访问令牌运行正常，则返回该用户的信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| userId | String | 登录用户ID |
| accessToken | String | 发放给登录用户的访问令牌 |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | 是否包含获取true or false (默认值为false) 访问令牌时使用的IdP相关信息。|

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
| linkedIdP | Object | 登录用户使用的IdP信息 |
| linkedIdP.idPCode | String | IdP信息 <br>guest、payco、facebook等 |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | 用户ID |
| member.lastLoginDate | String | 上一次登录的时间 <br>第一次登录的用户没有此值。|
| member.appId | String | appId |
| member.valid | String | 登录成功的用户的值为"Y"。<br>(有关其他值的说明请参考成员API。) |
| member.regDate | String | 用户创建账户的时间 |
| authList | Array[Object] | 用户认证IdP相关信息 |
| authList[].authSystem | String | Gamebase内部使用的认证系统 <br>预计将会支持用户认证系统 |
| authList[].idPCode | String | 用户认证IdP信息 <br>guest、payco、facebook等 |
| authList[].authKey | String | authSystem发放的用户区分值 |
| temporaryWithdrawal | Object | 预约退出信息 <br>仅在valid为"T"值时提供。|
| temporaryWithdrawal.gracePeriodDate | String | 预约退出的到期时间ISO 8601 |

**[Error Code]**

[错误代码](./error-code/#server)

<br/>
#### Get IdP Token and Profiles

是在客户端通过"Login with IdP"登录时发放的Gamebase Access Token。使用此令牌可查看登录时使用的IdP Access Token和Profiles信息。

> [注意]
> 按照各IdP类别，IdP的Access Token有效时间都不同，而且一般很短。
> 在客户端通过"Login as the Latest Login IdP"登录成功后，通过服务器调用相关API时，因IdP的Access Token已过期，有可能无法获取IdP信息。

<br/>

> [参考]
> 如果只用IdP的Access Token，有些IdP无法获取信息。
> ex) appleid / iosgamecenter : 不存在使用Access Token，通过Server to Server可获取的信息。

<br/>

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/idps/{idPCode}?accessToken={accessToken} |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| userId | String | 登录用户ID |
| idPCode | String | 用户认证IdP信息 <br>google、payco、facebook等 |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| accessToken | String | mandatory | 发放给登录用户的Gamebase Access Token |

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
| idPProfile | Map<String, Object> | 登录用户使用过的IdP Profiles <br>- 每个IdP的响应格式(format)都不同。|
| idPToken | Object | 登录用户使用过的IdP的Access Token信息 |
| idPToken.idPCode | String | IdP code |
| idPToken.accessToken | String | IdP Access Token |
<br>
<br>

## Launching

#### Get Simple Launching

可以简单确认在Console设置的服务器地址、安装URL及当前维护状态时的维护时间、消息等启动客户端时提供的Launching信息。
若仅欲确认当前是否设置检查，使用[Check Maintenance] API即可。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/launching/simple |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | OsCode | true | OS代码 <br>AOS, IOS, WEB, WINDOWS |
| storeCode | Enum | true | store代码 <br>- GG: Google<br>- ONESTORE: ONE store<br>- AS: AppStore |
| clientVersion | String | true | 客户端版本 |

**[Response Body]**

##### 正常
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

##### 维护
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
| status | Object | 表示当前客户端状态的信息 |
| status.code | int | 客户端状态代码 <br><br>正常：200 <br>推荐更新：201、强制更新：300 <br>服务终止 : 302 <br>维护中：303 |
| status.message | String | 客户端状态消息 |
| app | Object | App信息 |
| app.storeCode | String | 应用商店代码 <br>"GG"、"AS"等 |
| app.accessInfo | Object | 在控制台应用页面中设置的信息 |
| app.accessInfo.serverAddress | String | 服务器地址<br>在客户端设置的服务地址的优先级高。<br>如果未设置客户端服务器地址时，将传递应用页面中设置的服务器地址。|
| app.accessInfo.csInfo | String | 客户服务信息 |
| app.relatedUrls | Object | 要在应用内使用的应用内URL |
| app.relatedUrls.termsUrl | String | 使用条款 |
| app.relatedUrls.csUrl| String | 客户服务 |
| app.relatedUrls.punishRuleUrl | String | 禁用规定 |
| app.relatedUrls.personalInfoCollectionUrl | String | 个人信息同意 |
| app.install | Object | 应用设置信息 |
| app.install.url | String | 安装URL |
| maintenance | Object | 维护信息 |
| maintenance.typeCode | String | 维护类型代码 <br>维护全部：”SYSTEM”, 按应用维护：”APP” |
| maintenance.beginDate | Date | 维护开始时间ISO 8601 |
| maintenance.endDate | Date | 维护结束时间ISO 8601 |
| maintenance.url | String | 维护URL |
| maintenance.reason | String | 维护原因 |
| maintenance.message | String | default维护原因消息 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

## Member

#### Get Member

查询单个用户的详细信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/{userId} |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| userId | String | 查询用户ID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | 是否包含true or false (默认值为true) 用户终端，OS等的详细信息 |

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
| member | Object | 被查询用户的基本信息 |
| member.userId | String | 用户ID |
| member.valid | Enum | Y：正常用户 <br>D : 已退出的用户 <br>B：禁用的用户 <br>M：丢失的账户 <br>T : 用户已预约退出 |
| member.appId | String | appId |
| member.regDate | String | 用户创建账户的时间 |
| member.lastLoginDate | String | 上一次登录的时间 <br>第一次登录的用户没有此值。|
| member.authList | Array[Object] | 用户认证IdP相关信息 |
| member.authList[].userId | String | 用户ID |
| member.authList[].authSystem | String | Gamebase内部使用的认证系统 <br>预计将会支持用户认证系统 |
| member.authList[].idPCode | String | 用户认证IdP信息 <br>guest, payco, facebook等 |
| member.authList[].authKey | String | authSystem发放的用户区分值 |
| member.authList[].regDate | String | IdP信息与用户账户映射的时间 |
| memberInfo | Object | 用户附加信息 |
| memberInfo.deviceCountryCode | String | 设置用户设备的国家 |
| memberInfo.usmCountryCode | String | 用户USIM的国家代码 |
| memberInfo.language | String | 用户语言 |
| memberInfo.osCode | String | 用户设备的OS类型 |
| memberInfo.telecom | String | 运营商 |
| memberInfo.storeCode | String | store代码 |
| memberInfo.network | String | 网络环境 <br>3g、WiFi等|
| memberInfo.deviceModel | String | 用户设备的型号名称 |
| memberInfo.osVersion | String | 用户设备的OS版本 |
| memberInfo.sdkVersion | String | SDK版本 |
| memberInfo.clientVersion | String | 客户端版本 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

#### Get Members

简单查询多个用户信息。

**[Method、URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | 查询对象用户ID |

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
| memberList | Array[Object] | 查询的用户基本信息 |
| memberList[].userId | String | 用户ID |
| memberList[].valid | Enum | Y：正常用户 <br>D : 已退出的用户 <br>B：禁用的用户 <br>M：丢失的账户 <br>T : 用户已预约退出 |
| memberList[].appId | String | appId |
| memberList[].regDate | String | 用户创建账户的时间 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### 获取IdP信息

查询与用户ID映射的IdP信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/auth/authKeys |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | 查询对象用户ID |

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
| result | Array[Object] | 被查询用户的基本信息 <br> userId为key，IdP信息为value的object |
| authkey | String | authSystem发放的用户区分值 |
| IdPCode | String | 用户认证IdP信息 <br>guest、payco、facebook等 |
| authSystem | String | Gamebase内部使用的认证系统 <br>预计将会提供用户认证系统 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### 使用认证密钥获取User ID信息

查询与用户认证密钥映射的ID。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Gamebase内部使用的认证系统，预计将会支持用户认证系统，目前是gbid。|

**[Request Body]**

```json
["authKey", "authKey", "authKey"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | authSystem发放的authKey |

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
| result | Array[Object] | 查询用户的基本信息authKey是key，用户Id是value的object。 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### Get UserId Information with IdP Id

查看使用IdP ID映射的用户ID信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/idps/{idPCode}/members |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| idPCode | String | IdP信息 <br>- payco、google、facebook、iosgamecenter、appleid、twitter、hangame |

**[Request Body]**

```json
["idPId", "idPId", "idPId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | mandatory | 查询对象用户的IdP ID。 <br> 查询对象列表的最大尺寸为300。 |

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
| result | Map<String, String> | 查询用户的ID信息<br>- IdP ID为key，Gamebase userId为value<br>- 如果不存在包含请求查询的IdP ID的userId信息，则在响应结果中不存在。|

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### 禁用历史记录

查询用户禁用历史记录。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 禁用历史记录查询开始时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory |禁用历史记录查询结束时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>begin ~ end期间被禁用，则在查询结果中存在。|
| page | String | optional | 要查询的页面。从0开始 |
| size | String | optional | 每页的数据数量 |

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
| pagingInfo | Object | 查询的页面信息 |
| pagingInfo.first | boolean | 如果是第一页，则为true。|
| pagingInfo.last | boolean | 如果是最后一页，则为true。|
| pagingInfo.numberOfElements | int | 数据总数 |
| pagingInfo.page | int | 页面编号 |
| pagingInfo.size | int | 每页的数据数量 |
| pagingInfo.totalElements | int | 数据总数 |
| pagingInfo.totalPages | int | 页面总数 |
| result | Array[Object] | 查询的禁用历史记录 |
| result.appId | String | 查询的禁用的NHN Cloud项目ID |
| result.banCaller | String | 禁用调用主体 |
| result.banReason | String | 禁用原因 |
| result.banType | String | 禁用类型 TEMPORARY or PERMANENT |
| result.beginDate | String | 禁用开始时间 ISO 8601标准时间 |
| result.endDate | String | 禁用结束时间 ISO 8601标准时间 |
| result.flags | String | 如果在控制台添加禁用时选择了Leaderboard删除，则返还“Leaderboard”。|
| result.message | String | 禁用消息 |
| result.name | String | 在控制台添加的模板名称 |
| result.regUser | String | 禁用添加者 |
| result.releaseCaller | String | 禁用解除主体 |
| result.releaseDate | String | 禁用解除时间 ISO 8601标准时间 |
| result.releaseReason | String | 禁用解除原因 |
| result.releaseUser | String | 禁用解除添加者 |
| result.seq | Long | 禁用历史记录顺序 |
| result.templateCode | Long | 在控制台添加的禁用模板代码值 |
| result.userId | String | 用户ID |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### 解除禁用历史记录

查询用户禁用解除历史记录。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans/release |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 禁用解除历史记录查询开始时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | 禁用解除历史记录查询结束时间 (ISO 8601标准时间， 需要UTF-8 Encoding) <br>begin ~ end期间禁用解除，则在查询结果中存在。|
| page | String | optional | 要查询的页面。从0开始。|
| size | String | optional | 每页的数据数量 |

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
| pagingInfo | Object | 查询的页面信息 |
| pagingInfo.first | boolean | 如果是第一页，则为true。|
| pagingInfo.last | boolean | 如果是最后一页，则为true。|
| pagingInfo.numberOfElements | int | 数据总数 |
| pagingInfo.page | int | 页面编号 |
| pagingInfo.size | int | 每页的数据数量 |
| pagingInfo.totalElements | int | 数据总数 |
| pagingInfo.totalPages | int | 页面总数 |
| result | Array[Object] | 查询的禁用历史记录 |
| result.appId | String | 查询的禁用的NHN Cloud项目ID |
| result.banCaller | String | 禁用调用主体 |
| result.banReason | String | 禁用原因 |
| result.banType | String | 禁用类型 TEMPORARY or PERMANENT |
| result.beginDate | String | 禁用开始时间 ISO 8601标准时间 |
| result.endDate | String | 禁用结束时间 ISO 8601标准时间 |
| result.flags | String | 如果在控制台登录禁用时选择了Leaderboard删除，则返还“Leaderboard”。|
| result.message | String | 禁用消息 |
| result.name | String | 在控制台注册的模板名称 |
| result.regUser | String |禁用添加者 |
| result.releaseCaller | String | 禁用解除主题 |
| result.releaseDate | String | 禁用解除时间 ISO 8601标准时间 |
| result.releaseReason | String | 禁用解除原因 |
| result.releaseUser | String | 禁用解除添加者 |
| result.seq | Long | 禁用历史记录顺序 |
| result.templateCode | Long | 在控制台添加的禁用模板代码值 |
| result.userId | String | 用户ID |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

#### Validate TransferAccount

检查为转移访客账户获得的ID及密码的有效性。为有效的TransferAccount时，返回获得的userId信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/transfer-account |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

无

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
| account.id | String | 要进行有效性验证的ID |
| account.password | String | 要进行有效性验证的密码 |

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
| member | Object | 查询的用户基本信息 |
| member.userId | String | 用户ID |
| member.valid | Enum | Y : 正常用户<br>D：注销的用户<br>B：停止使用的用户<br>M：丢失的账户<br>T : 已预约退出的用户 |
| member.appId | String | 应用程序ID |
| member.regDate | String | 用户创建账户的时间 |
| member.lastLoginDate | String | 最后一次登录的时间 <br>初次登录的用户无相应值 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

#### Withdraw

用户账号将被删除。

> [参考]
> 调用服务器退出API进行账号退出处理时，若成功退出，则需通过客户端调用SDK的logout API删除令牌等数据。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}?regUser={regUser} |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| userId | String | 需要进行退出处理的用户ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | mandatory | 请求退出的系统或用户信息<br> - 此信息可以在Console > ”member”页面的”退出履历”页面上确认。|

**[Request Body]**

无

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

[错误代码](./error-code/#server)

<br>
<br>

## Maintenance

#### 确认维护设置

确认当前是否设置了维护。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

无

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
| underMaintenance | boolean | 是否设置了当前维护 |
| maintenances | Object | 如果已设置维护，维护基本信息 |
| maintenances.typeCode | Enum | APP：游戏中设置的维护 <br>SYSTEM : Gamebase系统中设置的维护 |
| maintenances.beginDate | String | 维护开始时间 ISO 8601 |
| maintenances.endDate | String | 维护结束时间 ISO 8601 |
| maintenances.url | String | 详细维护URL |
| maintenances.message | String | 维护消息 |
| maintenances.targetStores | Array[Enum] | 仅对特定客户设置进行检查时，设置检查的客户商店代码。<br>- GG : Google<br>- ONESTORE : ONE store<br>- AS : AppStore |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

## Coupon

#### Check Validation And Consume Coupon

对于通过Console获得的优惠券代码，验证有效性并更改优惠券状态。若为有效的优惠券，则更改为消费状态，作为响应结果，返回支付的道具信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/coupons/{couponCode}?storeCode={storeCode} |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |
| userId | String | 要使用优惠券的userId |
| couponCode | String | 优惠券代码 |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| storeCode | String | optional | 若获取的优惠券只能在指定的商店使用，则需传送商店代码。<br>如果是所有的商店，”ALL”或省略参数。<br>- GG : Google<br>- ONESTORE : ONE store<br>- AS : AppStore |

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
| result | Object | 优惠券信息 |
| result.title | String | 优惠券名 |
| result.benefits | Array[Object] | 要支付的道具信息 |
| result.benefits.itemId | String | 道具ID |
| result.benefits.amount | Integer | 道具个数 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

## Purchase(IAP)

#### Consume

若已完成Google Play Store、App Store、ONEStore支付，则需向用户提供道具并将履历注册在服务器后通知Gmaebase。1项支付仅可进行1次支付消费，若支付的状态非正常，则不消费。

> [参考]
> 注册商品时，仅对商品类型为一次性（CONSUMABLE）的道具支付进行消费（consume）处理。
> 1项支付可进行1次消费，未进行支付消费的支付视为IAP尚未提供道具。

可通过调用SDK及服务器的未消费支付明细API查看未消费（consume）支付明细。即使存在未消费支付明细，也要将游戏服务器的履历作为判断基准。
（若因网络故障出现API timeout， 即使Gamebase已提供道具，则会出现因API响应失败游戏服务器无法向用户提供道具的情况。)

> [参考]
> 如果游戏未能管理所有的（提供道具的）历史记录，则需将该API的request timeout设置为10秒以上，至少在出现API timout时注册历史记录，防止出现重复提供或没提供等错误。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consume |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

无

**[Request Body]**

```json
{
    "paymentSeq": "2019091931571201",
    "accessToken": "90fD1bs1guXwY6aZ7rseEKYW_6gMCISjDASgten4MD6O7XZD7VRjZcs8OTm8lOQVFTegoY4WK78P2WQCMm7cx"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | mandatory | 支付编号 |
| accessToken | String | mandatory  | 支付验证令牌（非登录验证令牌） |

> [参考]
> 客户调用requestPurchase API时响应的purchaseToken值作为accessToken使用。

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
| result | Object | 支付基本信息 |
| result.price | Float | 支付价格 |
| result.currency  | String  | 支付货币  |
| result.productSeq | Long | 支付道具编号（console中注册的道具固有编号）|
| result.marketId | String | store代码<br>GG : Google, AS : Apple，ONESTORE : ONE store |
| result.gamebaseProductId | String | Gamebase商品ID<br>用户在控制台中注册商品时的用户输入值 |
| result.payload | String | 在SDK中设置的附加信息 |

> [参考]
> 根据客户端使用的SDK版本及支付API，在响应结果中存在或不存在gamebaseProductId值。

> [参考]
> 游戏服务器端按照道具号码、商店代码及Gamebase商品ID提供商品（道具）， 但若对1个商店道具ID注册N个Gamebase商品时，应按照商店代码和Gamebase商品ID提供商品。

**[Error Code]**

[错误代码](./error-code/#server)

<br>

#### List Consumables

可查询完成支付但尚未消费(Consume)的未消费支付明细。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consumable |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

无

**[Request Body]**

```json
{
    "marketId": "GG",
    "userId": "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | mandatory | 商店代码<br>GG : Google, AS : Apple, ONESTORE : One store |
| userId | String | mandatory  | 用户ID  |

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
| result | Array[Object] | 支付基本信息 |
| result[].paymentSeq | String  | 支付编号 |
| result[].productSeq | Long | 支付道具编号（console中注册的道具固有编号）|
| result[].currency  | String  | 支付货币  |
| result[].price | Float | 支付价格 |
| result[].accessToken | String | 支付验证令牌 |
| result[].marketId | String | store代码 |
| result[].gamebaseProductId | String | Gamebase商品ID<br>在控制台中注册商品时的用户输入值 |
| result[].purchaseTime | String | 支付日期 |
| result[].payload | String | 在SDK中设置的附加信息 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

### List Active Subscriptions

可查询用户当前订阅的支付。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/active-subscriptions |

**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN Cloud项目ID |

**[Request Parameter]**

无

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
| marketId | String | mandatory | 商店代码<br>GG : Google, AS : Apple, ONESTORE : One Store |
| packageName | String | mandatory | 控制台中注册的应用程序的packageName |
| userId | String | mandatory  | 用户ID  |

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
| result | Array[Object] | 支付基本信息 |
| result[].marketId  | String  | 商店代码 |
| result[].userId  | String  | 用户ID |
| result[].paymentSeq | String  | 支付编号 |
| result[].accessToken | String | 支付验证令牌 |
| result[].productSeq | Long | 支付道具编号（console中注册的道具固有编号）|
| result[].productId | String  | 商店注册的商品（道具）标识符 |
| result[].productType | String  | 商品（道具）类型<br>订阅：AUTO_RENEWABLE |
| result[].currency  | String  | 支付货币  |
| result[].price | Float | 支付价格 |
| result[].paymentId | String | 最近更新的商店支付编号 |
| result[].gamebaseProductId | String | Gamebase商品ID<br>在控制台中注册商品时的用户输入值 |
| result[].payload | String | 在SDK中设置的附加信息 |
| result[].purchaseTime | String | 最近更新的时间 |
| result[].expiryTime | String | 订阅到期时间 |

**[Error Code]**

[错误代码](./error-code/#server)

<br>
<br>

## Leaderboard

Gamebase为NHN Cloud Leaderboard服务的服务器API提供**Wrapping**功能。使用Wrapping功能可在用户服务器上通过统一接口来使用NHN Cloud服务。

> [参考]
> 如果启用Gamebase，即使不设置Leaderboard Appkey，也可通过调用Gamebase Wrapping API来使用Leaderboard。

<br>

#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| 查询Factor中的注册用户数<br>- Get user count in factor | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| 查看所有Factor数<br>- Get total factor count | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factor-count | /leaderboard/v2.0/appkeys/{appKey}/factor-count |
| 查询Factor信息<br>- Get factor info<br>- Get multiple factor info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors | /leaderboard/v2.0/appkeys/{appKey}/factors |
| 查询单个用户分数/排名<br>- Get single user info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 查询多个用户分数/排名<br>- Get multiple user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 查询一定范围的整体分数/排名<br>- Get multiple user info by range | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 查询指定顺序的用户<br>- Get selected rank user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |
| 查询指定用户的顺序和上级、下级用户的顺序<br>- Get multiple user info by pivot user | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} | /leaderboard/v2.0/appkeys/{appkey}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} |
| 登录单个用户分数<br>- Set single user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 登录单个用户分数/ExtraData- Set single user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 登录多个用户分数- Set multiple user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 登录多个用户分数/ExtraData<br>- Set multiple user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/scores-with-extra |
| 删除单个用户Leaderboard信息<br>- Delete single user info<br>- Delete multiple user info | DELETE | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |

<br/>

**有关API的详细说明，请参考以下链接。**
关于与Gamebase Wrapping API进行映射的Leaderboard API Spec，请参考以下指南。
即使不设置Leaderboard Appkey，也可使用Gamebase AppId和SecretKey调用Gamebase Wrapping Leaderboard API。

[Leaderboard Guide](/Game/Leaderboard/zh/api-guide/)

<br/>

##### API调用示例

```
GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count

Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
```

<br/>
<br/>

## Push

Gamebase为NHN Cloud Push服务的服务器API提供**Wrapping**功能。如果使用Wrapping功能，可通过用户服务器的单一的统合界面来使用NHN Cloud服务。

> [参考]
> 如果启用Gamebase，即使不设置Push Appkey，也可通过调用Gamebase Wrapping API使用Push功能。


<br>

#### Wrapping API
|    | API | Method | Wrapping URI | Push URI |
| --- | --- | --- | --- | --- |
| 消息 | 发送 | POST | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 查询 | GET | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 查询发送日志 | GET | /tcgb-push/v1.3/apps/{appId}/logs/message | /push/v2.4/appkeys/{appkey}/logs/message |
| 预约的消息 | 生成发送日程 | POST | /tcgb-push/v1.3/apps/{appId}/schedules | /push/v2.4/appkeys/{appkey}/schedules |
|   | 生成 | POST | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | 查询列表 | GET | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | 查询单件 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id} | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id} |
|   | 查询成功发送 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id}/messages | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id}/messages |
|   | 更改 | PUT | /tcgb-push/v1.3/apps/{appId}/reservations/{reservationId} | /push/v2.4/appkeys/{appkey}/reservations/{reservationId} |
|   | 删除 | DELETE | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |

<br/>

**有关相关API的详细说明，请参考以下链接。**
关于与Gamebase Wrapping API进行映射的Push API Spec，请参考以下指南。
即使不设置Push Appkey，也可使用Gamebase AppId和SecretKey调用Gamebase Wrapping Push API。

> [参考]
> 将Push指南上的uid Key指定为gamebase userId值。在客户端SDK中注册推送令牌时，用户标识符将注册为gamebase userId。
> 一个用户同意允许从多个终端机接收推送时，将从所有的终端机接收推送。

[Push Guide](/Notification/Push/zh/api-guide/)

<br/>

##### API调用示例

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

### Support

如需知道API调用失败原因，请将**API调用URL(如有HTTP body将HTTP body一同)及响应结果**发送到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。

##### API调用示例

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance
```

##### API失败响应结果

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
