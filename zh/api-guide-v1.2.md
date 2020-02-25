## Game > Gamebase > API v1.2指南

## 변경 사항
- IAP API가 변경 되었습니다.
- Get Simple Launching API 호출 시 필수 파라미터로 storeCode가 추가 되었습니다.
- Check Maintenance API 응답 결과에 점검 대상에 대한 storeCode 정보가 추가 되었습니다.
- GUEST 계정에 대한 단말기 이전에 사용되는 TransferAccount에 대해, 사전에 발급된 TransferAccount를 검증 할 수 있는 Validate TransferAccount API가 추가 되었습니다.
- API 응답결과의 date 타입이 Epoch time 에서 ISO 8601 형식(yyyy-MM-dd'T'HH:mm:ssXXX)으로 변경되었습니다. Token Authentication, Get Member, Get Members API 응답 결과의 regDate, lastLoginDate 항목
- 쿠폰 소진 API가 추가 되었습니다.

## Advance Notice

Gamebase Server API以RESTful类型提供如下API。 为了使用服务器API，应了解以下信息。

#### 服务器地址

调用API的服务器地址如下。此地址也可以在Gamebase控制台页面中确认。
> https://api-gamebase.cloud.toast.com

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

#### AppId

APP ID 可通过TOAST项目ID，在APP菜单页面中确认。
![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

密钥(secret key)是API的访问控制方法，可在 Gamebase Console进行确认。调用Server API时，在HTTP标头中将密钥设置为必须。
> [参考]
> 如果密钥被泄露发生了无效调用，则可以通过点击**创建**按钮创建新密钥后使用新秘钥。

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

为了支持调用API的服务器内部管理API请求，提供TransactionId功能。调用API的服务器发送HTTP请求时，在Head中设置Transaction ID，Gamebase服务器也将在响应HTTP Header和响应结果的Response Body Header中设置TransactionId发送结果给调用API的服务器。

## Common

#### HTTP Header

API调用时在HTTP Header中设置以下项目。

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |参考SecretKey说明 |
| X-TCGB-Transaction-Id | optional | 参考TransactionId说明 |

#### API 响应

传递**HTTP 200 OK**以响应所有API请求。 可以通过Response Body的Header项目来判断API请求是否成功。

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
| transactionId | String | API请求时在HTTP Header设定的值。<br>如果不传递此值，则返回Gamebase内部生成的值 |
| isSuccessful | boolean | 成功与否 |
| resultCode | int | 响应代码<br>成功时为0，失败时返还错误代码 |
| resultMessage | String | 响应消息 |

## Authentication

#### 令牌认证

检查发给登录用户的访问令牌是否有效。如果访问令牌运行正常，则返回该用户的信息。
**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.2/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |
| userId | String | 登录用户ID |
| accessToken | String | 发放给登录用户的访问令牌 |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | 是否包含获取true or false (默认值为false) 访问令牌时使用的IdP相关信息 |

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
| linkedIdP | Object | 登录用户使用的IdP信息 |
| linkedIdP.idPCode | String | IdP信息 <br>guest, payco, facebook等 |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String | 用户 ID |
| member.lastLoginDate | String | 上一次登录的时间 <br>第一次登录的用户没有此值 |
| member.appId | String | appId |
| member.valid | String | 登录成功的用户的值为"Y" <br>(有关其他值的说明请参考成员API) |
| member.regDate | String | 用户创建账户的时间 |
| authList | Array[Object] | 用户认证IdP相关信息 |
| authList[].authSystem | String | Gamebase内部使用的认证系统 <br>预计将会支持用户认证系统 |
| authList[].idPCode | String | 用户认证IdP信息 <br>guest, payco, facebook等 |
| authList[].authKey | String | authSystem发放的用户区分值 |


**[Error Code]**

[错误代码](./error-code/#server)

## Launching

#### Get Simple Launching

Console 화면에서 설정한 서버 주소, 설치 URL 등의 클라이언트 설정 정보 및 현재 점검 상태/시간/ 메시지 등 클라이언트 앱 기동시 제공되는 Launching 정보들에 대해 서버에서 간략히 확인할수 있습니다.
현재 점검 설정 여부 만을 확인하고 싶다면, [Check Maintenance] API를 사용하면 됩니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/launching/simple |


**[Request Header]**

确认共通事项

**[Path Variable]**  

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |

**[Request Parameter]**  

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | OsCode | true | OS代码 <br>AOS, IOS, WEB, WINDOWS |
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
| status.code | int | 客户端状态代码 <br><br>正常：200 <br>推荐更新：201, 强制更新：300 <br>服务终止: 302 <br>维护中：303 |
| status.message | String | 客户端状态消息 |
| app | Object | App信息 |
| app.storeCode | String | 应用商店代码 <br>"GG", "AS" 等 |
| app.accessInfo | Object | 在控制台应用页面中设置的信息 |
| app.accessInfo.serverAddress | String | 服务器地址<br>在客户端设置的服务地址的优先级高。 <br>如果未设置客户端服务器地址时，将传递应用页面中设置的服务器地址。 |
| app.accessInfo.csInfo | String | 客服中心信息 |
| app.relatedUrls | Object | 要在应用内使用的应用内URL |
| app.relatedUrls.termsUrl | String | 使用条款 |
| app.relatedUrls.csUrl| String | 客服中心 |
| app.relatedUrls.punishRuleUrl | String | 禁用规定 |
| app.relatedUrls.personalInfoCollectionUrl | String | 个人信息同意 |
| app.install | Object | 应用设置信息 |
| app.install.url | String | 安装URL |
| maintenance | Object | 维护信息 |
| maintenance.typeCode | String | 维护类型代码 <br>维护全部：'SYSTEM', 按应用维护：'APP' |
| maintenance.beginDate | Date | 维护开始时间 ISO 8601 |
| maintenance.endDate | Date | 维护结束时间 ISO 8601 |
| maintenance.url | String | 维护URL |
| maintenance.reason | String | 维护原因 |
| maintenance.message | String | default维护原因消息 |

<br>

## Member

#### Get Member

查询单个用户的详细信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/{userId} |


**[Request Header]**

确认共通事项


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |
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
| member.valid | Enum | Y：正常用户 <br>D: 已退出的用户 <br>B：禁用的用户 <br>M：丢失的账户|
| member.appId | String | appId |
| member.regDate | String | 用户创建账户的时间 |
| member.lastLoginDate | String | 上一次登录的时间 <br>第一次登录的用户没有此值 |
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
| memberInfo.network | String | 网络环境 <br>3g, WiFi等|
| memberInfo.deviceModel | String | 用户设备的型号名称 |
| memberInfo.osVersion | String | 用户设备的OS版本 |
| memberInfo.sdkVersion | String | SDK版本 |
| memberInfo.clientVersion | String | 客户端版本 |

**[Error Code]**

[错误代码](./error-code/#server)

#### Get Members

简单查询多个用户信息。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |

**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 查询对象用户ID <br>  ["userId", "userId", "userId",...]|

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
| memberList | Array[Object] | 被查询用户的基本信息 |
| memberList[].userId | String | 用户ID |
| memberList[].valid | Enum | Y：正常用户 <br>D: 已退出的用户 <br>B：禁用的用户 <br>M：丢失的账户|
| memberList[].appId | String | appId |
| memberList[].regDate | String | 用户创建账户的时间 |


**[Error Code]**

[错误代码](./error-code/#server)


#### 获取 IdP 信息

查询与用户ID映射的IdP信息。

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.2/apps/{appId}/auth/authKeys |

**[Request Header]**

确认共同事项


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 查询对象用户ID  ["userId", "userId", "userId",...]|

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
| result | Array[Object] | 被查询用户的基本信息 <br> userId为key，IdP信息为value的object|
| authkey | String | authSystem发放的用户区分值 |
| IdPCode | String | 用户认证IdP信息 <br>guest, payco, facebook等 |
| authSystem | String | Gamebase内部使用的认证系统 <br>预计将会提供用户认证系统 |

**[Error Code]**

[错误代码](./error-code/#server)

#### 使用认证密钥获取User ID信息

查询与用户认证密钥映射的ID。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.2/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Gamebase内部使用的认证系统，预计将会支持用户认证系统，目前是gbid |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authKeyList | Array[String] | mandatory | authSystem发放的authKey ["authKey", "authKey", "authKey",...]|

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
| result | Array[Object] | 被查询用户的基本信息authKey为key，用户Id为value的object|

**[Error Code]**

[错误代码](./error-code/#server)

#### 禁用历史记录

查询用户禁用历史记录。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans |


**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 禁用历史记录查询开始时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory |禁用历史记录查询结束时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>begin ~ end期间被禁用，则在查询结果中存在 |
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
| pagingInfo.first | boolean | 如果是第一页，则为true |
| pagingInfo.last | boolean | 如果是最后一页，则为true |
| pagingInfo.numberOfElements | int | 数据总数 |
| pagingInfo.page | int | 页面编号 |
| pagingInfo.size | int | 每页的数据数量 |
| pagingInfo.totalElements | int | 数据总数 |
| pagingInfo.totalPages | int | 页面总数 |
| result | Array[Object] | 查询的禁用历史记录 |
| result.appId | String | 查询的禁用的TOAST项目ID |
| result.banCaller | String | 禁用调用主体 |
| result.banReason | String | 禁用原因 |
| result.banType | String | 禁用类型。TEMPORARY or PERMANENT |
| result.beginDate | String | 禁用开始时间。 ISO 8601标准时间 |
| result.endDate | String | 禁用结束时间。ISO 8601标准时间 |
| result.flags | String | 如果在控制台添加禁用时选择了Leaderboard删除，则返还“Leaderboard” |
| result.message | String | 禁用消息 |
| result.name | String | 在控制台添加的模板名称 |
| result.regUser | String | 禁用添加者 |
| result.releaseCaller | String | 禁用解除主体 |
| result.releaseDate | String | 禁用解除时间。ISO 8601标准时间 |
| result.releaseReason | String | 禁用解除原因 |
| result.releaseUser | String | 禁用解除添加者 |
| result.seq | Long | 禁用历史记录顺序 |
| result.templateCode | Long | 在控制台添加的禁用模板代码值 |
| result.userId | String | 用户ID |

**[Error Code]**

[错误代码](./error-code/#server)

#### 解除禁用历史记录

查询用户禁用解除历史记录。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.2/apps/{appId}/members/bans/release |


**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | mandatory | 禁用解除历史记录查询开始时间 (ISO 8601标准时间，需要UTF-8 Encoding) <br>ex) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | mandatory | 禁用解除历史记录查询结束时间 (ISO 8601标准时间， 需要UTF-8 Encoding) <br>begin ~ end期间禁用解除，则在查询结果中存在 |
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
| pagingInfo.first | boolean | 如果是第一页，则为true |
| pagingInfo.last | boolean | 如果是最后一页，则为true |
| pagingInfo.numberOfElements | int | 数据总数 |
| pagingInfo.page | int | 页面编号 |
| pagingInfo.size | int | 每页的数据数量 |
| pagingInfo.totalElements | int | 数据总数 |
| pagingInfo.totalPages | int | 页面总数 |
| result | Array[Object] | 查询的禁用历史记录 |
| result.appId | String | 查询的禁用的TOAST项目ID |
| result.banCaller | String | 禁用调用主体 |
| result.banReason | String | 禁用原因 |
| result.banType | String | 禁用类型。TEMPORARY or PERMANENT |
| result.beginDate | String | 禁用开始时间。 ISO 8601标准时间 |
| result.endDate | String | 禁用结束时间。ISO 8601标准时间 |
| result.flags | String | 如果在控制台登录禁用时选择了Leaderboard删除，则返还“Leaderboard” |
| result.message | String | 禁用消息 |
| result.name | String | 在控制台注册的模板名称 |
| result.regUser | String |禁用添加者 |
| result.releaseCaller | String | 禁用解除主题 |
| result.releaseDate | String | 禁用解除时间。ISO 8601标准时间 |
| result.releaseReason | String | 禁用解除原因 |
| result.releaseUser | String | 禁用解除添加者 |
| result.seq | Long | 禁用历史记录顺序 |
| result.templateCode | Long | 在控制台添加的禁用模板代码值 |
| result.userId | String | 用户ID |

**[Error Code]**

[错误代码](./error-code/#server)

#### Validate TransferAccount

检查为转移访客账户获得的ID及密码的有效性。유효한 TransferAccount인 경우 발급 받은 userId 정보를 리턴합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.1.2/apps/{appId}/members/transfer-account |


**[Request Header]**

确认通用事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |


**[Request Parameter]**

无


# Request Body

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

# Response Body

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
| member | Object | 查询的用户的基本信息 |
| member.userId | String | 用户ID |
| member.valid | Enum | Y:正常用户<br>D：注销的用户<br>B：停止使用的用户<br>M：丢失的账户|
| member.appId | String | 应用程序ID |
| member.regDate | String | 用户创建账户的时间 |
| member.lastLoginDate | String | 最后一次登录的时间 <br>初次登录的用户无相应值 |

**[Error Code]**

[错误代码](./error-code/#server)


<br>

## Maintenance

#### 确认维护设置

确认当前是否设置了维护。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.2/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

确认共通事项

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST项目ID |

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
| maintenances.typeCode | Enum | APP：游戏中设置的维护 <br>SYSTEM: Gamebase系统中设置的维护 |
| maintenances.beginDate | String | 维护开始时间。ISO 8601 |
| maintenances.endDate | String | 维护结束时间。ISO 8601 |
| maintenances.url | String | 详细维护URL |
| maintenances.message | String | 维护消息 |
| maintenances.targetStores | Array[Enum] | 특정 클라이언트에 대해서만 점검 설정시, 점검 설정된 클라이언트의 스토어코드<br>- GG: Google<br>- ONESTORE<br>- AS: AppStore |

**[Error Code]**

[错误代码](./error-code/#server)

<br>

## Coupon

#### Check Validation And Consume Coupon

Console을 통해 발급된 쿠폰 코드에 대해 유효성 검증 및 쿠폰 상태를 변경 합니다. 유효한 쿠폰인 경우 소비 상태로 변경을 하고, 응답 결과로 지급할 아이템 정보를 리턴합니다.

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

Gamebase为TOAST Leaderboard服务的服务器API提供**Wrapping**功能。使用Wrapping 功能可在用户服务器上通过统一接口使用TOAST服务


#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| 查询Factor中的注册用户数 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| 查询单个用户分数/排名 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 查询多个用户分数/排名 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 查询一定范围的整体分数/排名 | GET | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 登录单个用户分数 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 登录单个用户分数/ExtraData | POST | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 登录多个用户分数 | POST | /tcgb-leaderboard/v1.2/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 登录多个用户分数/ExtraData | POST | /tcgb-leaderboard/v1.2/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| 删除单个用户Leaderboard信息 | DELETE | /tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**有关API的详细说明，请参考以下链接。**


[Leaderboard Guide](/Game/Leaderboard/ko/api-guide/)

##### API调用示例

```
Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.2/apps/{appId}/factors/{factor}/user-count
```

## Others

### Support

如需咨询API调用失败原因，请将 **API调用URL(如有HTTP body将HTTP body一同)及响应结果**发送到 [客服中心](https://toast.com/support/inquiry)，我们会尽快回复。



##### API调用示例

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.2/apps/C3JmSctU/maintenances/under-maintenance
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
      "uri": "/tcgb-launching/v1.2/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful": false
  }
}

```
