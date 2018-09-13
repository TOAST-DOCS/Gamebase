## Game > Gamebase > APIガイド

Gamebase Server APIは、RESTful形式で、 次のようなAPIを提供します。

## Advance Notice

サーバーAPIを使用するためには、次のような情報が必要です。
<br>

#### Server Address

APIを呼び出すためのサーバーアドレスは、次の通りです。該当するアドレスは、Gamebase Console画面からでも確認できます。
> https://api-gamebase.cloud.toast.com
<br>

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.2.png)

<br>

#### AppId

アプリIDとはTOASTプロジェクトのIDのことであり、アプリメニューの画面から確認することができます。
![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

<br>

#### SecretKey

シークレットキー(secret key)は、APIに対するアクセスを制御する方法で、Gamebase Consoleから確認することができます。シークレットキーは、ServerAPIを呼び出すとき、HTTPのヘッダーに必ず設定する必要があります。
> [参考]<br>
> シークレットキーが外部に露出して正しくない呼び出しが発生した場合、**作成**ボタンをクリックして新しくシークレットキーを作成し、その新しいシークレットキーを使用してください。
<br>

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

<br>

#### TransactionId

APIを呼び出すサーバーで内部的にAPIリクエストを管理することができる方法としてTransactionId機能を提供します。呼び出すサーバーからHTTPのヘッダーにトランザクションIDを設定してAPIを呼び出すと、Gamebaseのサーバーは、レスポンスHTTP Header及びレスポンス結果のResponse Body Headerに該当するTransactionIdを設定して結果を送ります。


## Common

#### HTTP Header

APIを呼び出す際には、HTTP Headerに次の項目を設定する必要があります。

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory |SecretKey 説明参考 |
| X-TCGB-Transaction-Id | optional | TransactionId 説明参考 |

<br>

#### API Response

すべてのAPIリクエストに対するレスポンスとして**HTTP 200 OK**を送ります。APIリクエストが成功したかどうかは、Response BodyのHeader項目を参考して判断することができます。

**[Request]**

```
Content-Type：application/json
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key：IgsaAP
GET https://api-gamebase.cloud.toast.com
```

**[Response]**

```
HTTP/1.1 200 OK
Content-Type：application/json;charset=UTF-8
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
```

```json
{
    "header"：{
    	"transactionId"："88a1ae42-6b1d-48c8-894e-54e97aca07fq",
        "isSuccessful"：true,
        "resultCode"：0,
        "resultMessage"："Success."
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| transactionId | String | APIをリクエストする際に、HTTP Headerに設定した値<br>該当する値を送らない場合、Gamebaseの内部で作成した値を返す |
| isSuccessful | boolean |成功したかどうか |
| resultCode | int | レスポンスコード<br>成功すると0、失敗するとエラーコードを返す |
| resultMessage | String | レスポンスメッセージ |


## Authentication

#### Token Authentication

ログインユーザーに発行されたAccess Tokenが有効かどうか検査します。Access Tokenが正常な場合、該当するユーザーの情報を返します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.0/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

共通事項確認
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |
| userId | String | ログインしたユーザーのID |
| accessToken | String | ログインしたユーザーに発行されたAccess Token |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | optional | true or false(デフォルトはfalse) <br>Access Tokenの発行を受ける際に使用された、IdP関連の情報が含まれているかどうか |

**[Response Body]**

```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："String",
    "isSuccessful"：true
  },
  "linkedIdP"：{
    "idPCode"："String",
    "idPId"："String"
  },
  "member"：{
    "userId"："String",
    "valid"："Y",
    "appId"："String",
    "regDate"：1488257745000,
    "lastLoginDate"：1488286059000,
    "authList"：[
      {
        "userId"："String",
        "authSystem"："String",
        "idPCode"："String",
        "authKey"："String",
        "regDate"：1488257745000
      },
      {
        "userId"："String",
        "authSystem"："String",
        "idPCode"："String",
        "authKey"："String",
        "regDate"：1490922916000
      }
    ]
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| linkedIdP | Object | ログインしたユーザーが使用したIdP情報 |
| linkedIdP.idPCode | String | IdP情報 <br>guest、payco、facebookなど |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String |ユーザーID |
| member.lastLoginDate | long | 最後にログインした時間<br>はじめてログインしたユーザーは、該当する値なし |
| member.appId | String | appId |
| member.valid | String | ログインに成功したユーザーの値は"Y" <br>(他の値に対する説明は、メンバーAPIを参考) |
| member.regDate | long | ユーザーがアカウントを作成した時間 |
| authList | Array[Object] |ユーザー認証IdP関連の情報 |
| authList[].authSystem | String | Gamebase内部で使用される認証システム<br>今後、ユーザー認証システムを実装する予定 |
| authList[].idPCode | String |ユーザー認証IdP情報<br>guest、payco、facebookなど |
| authList[].authKey | String | authSystemから発行されたユーザーを区別する値 |


**[Error Code]**

[エラーコード](./error-code/#server)

## Launching

#### Get Simple Launching

Console 에서 설정한 서버 주소, 설치 URL 및 현재 점검상태이면 점검 시간 및 메시지 등 클라이언트 앱 기동시 제공되는 Launching 정보들에 대해 간략히 확인할수 있습니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.0/apps/{appId}/launching/simple |


**[Request Header]**

공통 사항 확인

**[Path Variable]**  

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOAST 프로젝트 ID |

**[Request Parameter]**  

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | OsCode | true | OS 코드 <br>AOS, IOS, WEB, WINDOWS |
| clientVersion | String | true | 클라이언트 버전 |

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
| maintenance.typeCode | String | 점검 타입 코드 <br>전체 점검:'SYSTEM', 앱별 점검:'APP' |
| maintenance.beginDate | Date | 점검 시작 시간 ISO 8601 |
| maintenance.endDate | Date | 점검 종료 시간 ISO 8601 |
| maintenance.url | String | 점검 URL |
| maintenance.reason | String | 점검 사유 |
| maintenance.message | String | default 점검 사유 메시지 |

<br>

## Member

#### Get member

単一会員の詳細情報を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.0/apps/{appId}/members/{userId} |


**[Request Header]**

共通事項確認
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |
| userId | String | 照会対象のユーザーID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | optional | true or false(デフォルトはtrue) <br>ユーザー端末、OSなどの詳細情報を含むかどうか |

**[Response Body]**
```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："SUCCESS",
    "isSuccessful"：true
  },
  "member"：{
    "userId"："String",
    "valid"："Y",
    "appId"："String",
    "regDate"：1488185201000,
    "lastLoginDate"：1488185201000,
	"authList"：[
		  {
			"userId"："String",
			"authSystem"："String",
			"idPCode"："String",
			"authKey"："String",
			"regDate"：1488185201000
		  }
		]
	  },
  "memberInfo"：{
    "deviceCountryCode"："String",
    "usimCountryCode"："String",
    "language"："String",
    "osCode"："String",
    "telecom"："String",
    "storeCode"："String",
    "network"："String",
    "deviceModel"："String",
    "osVersion"："String",
    "sdkVersion"："String",
    "clientVersion"："String"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| member | Object | 照会されたユーザーの基本情報 |
| member.userId | String |ユーザーID |
| member.valid | Enum | Y：正常なユーザー<br>D：退会したユーザー<br>B：利用停止中のユーザー<br>M：流出したアカウント |
| member.appId | String | appId |
| member.regDate | long | ユーザーがアカウントを作成した時間 |
| member.lastLoginDate | long | 最後にログインした時間<br>はじめてログインしたユーザーは、該当する値なし |
| member.authList | Array[Object] |ユーザー認証IdP関連の情報 |
| member.authList[].userId | String |ユーザーID |
| member.authList[].authSystem | String | Gamebase内部で使用される認証システム<br>今後ユーザー認証システム実装予定 |
| member.authList[].idPCode | String |ユーザー認証IdP情報 <br>guest、payco、facebookなど |
| member.authList[].authKey | String | authSystemから発行された、ユーザーを区別する値 |
| member.authList[].regDate | long | IdP情報がユーザーアカウントとマッピングされた時間 |
| memberInfo | Object | ユーザーに対する付加情報 |
| memberInfo.deviceCountryCode | String |ユーザー端末の国家設定 |
| memberInfo.usmCountryCode | String |ユーザーUSIMの国家コード|
| memberInfo.language | String | ユーザー言語 |
| memberInfo.osCode | String | ユーザー端末のOS種類 |
| memberInfo.telecom | String | 通信キャリア |
| memberInfo.storeCode | String | storeコード |
| memberInfo.network | String | ネットワーク環境<br>3g、Wi-Fiなど|
| memberInfo.deviceModel | String |ユーザー端末のモデル名 |
| memberInfo.osVersion | String |ユーザー端末のOSバージョン |
| memberInfo.sdkVersion | String | SDKバージョン |
| memberInfo.clientVersion | String | クライアントバージョン |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get members

複数の会員情報を簡素化して照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members |

**[Request Header]**

共通事項確認
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |

**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 照会対象のユーザーID<br>  ["userId", "userId", "userId",...]|

**[Response Body]**

```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："SUCCESS",
    "isSuccessful"：true
  },
  "memberList"：[
    {
		"userId"："String",
		"valid"："Y",
		"appId"："String",
		"regDate"：1488185201000
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| memberList | Array[Object] | 照会されたユーザーの基本情報 |
| memberList[].userId | String |ユーザーID |
| memberList[].valid | Enum | Y：正常なユーザー<br>D：退会したユーザー<br>B：利用停止中のユーザー<br>M：流出したアカウント |
| memberList[].appId | String | appId |
| memberList[].regDate | long | ユーザーがアカウントを作成した時間 |


**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get IdP infomation

ユーザーIDにマッピングされたIdP情報を照会します。

**[Method, URI]**

| Method | Type | URI |
| --- | --- | --- |
| POST | String | /tcgb-member/v1.0/apps/{appId}/auth/authKeys |

**[Request Header]**

共通事項確認
<br>

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userIdList | Array[String] | mandatory | 照会対象のユーザーID<br>  ["userId", "userId", "userId",...]|

**[Response Body]**

```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："SUCCESS",
    "isSuccessful"：true
  },
  "result"：{
    "String"：[
      {
        "authKey"："String",
        "idPCode"："gbid",
        "authSystem"："String"
      }
    ]
  }
}

```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 照会されたユーザーの基本情報 <br>userIdがkey、IdP情報がvalueのobject|
| authkey | String | authSystemから発行された、ユーザーを区別する値 |
| IdPCode | String |ユーザー認証IdP情報 <br>guest、payco、facebookなど |
| authSystem | String | Gamebase内部で使用される認証システム<br>今後ユーザー認証実装予定 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get userId infomation with auth key

ユーザー認証キーにマッピングされたユーザーIDを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.0/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |


**[Request Header]**

共通事項確認

<br>


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |


**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | mandatory | Gamebase内部で使用される認証システム<br>今後ユーザー認証システム実装予定 <br>現在はgbid |


**[Request Body]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authKeyList | Array[String] | mandatory | authSystemから発行されたauthKey<br> ["authKey", "authKey", "authKey",...]|

**[Response Body]**

```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："SUCCESS",
    "isSuccessful"：true
  },
  "result"：{
    "String"："String"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 照会されたユーザーの基本情報 <br>authKeyがkeyで、userIdがvalueのobject|

**[Error Code]**

[エラーコード](./error-code/#server)


#### Ban Histories

사용자 이용 정지 이력을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.0/apps/{appId}/members/bans |


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
| result.beginDate | String | 이용 정지 시작 시간. ISO 8601 표준 시간|
| result.endDate | String | 이용 정지 종료 시간. ISO 8601 표준 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메세지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | String | 이용 정지 해제 시간. ISO 8601 표준 시간 |
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
| GET | /tcgb-member/v1.0/apps/{appId}/members/bans/release |


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
| result.beginDate | String | 이용 정지 시작 시간. ISO 8601 표준 시간 |
| result.endDate | String | 이용 정지 종료 시간. ISO 8601 표준 시간 |
| result.flags | String | 콘솔에서 이용 정지 등록 시 리더보드 삭제를 선택한 경우 'Leaderboard' 로 반환 |
| result.message | String | 이용 정지 메세지 |
| result.name | String | 콘솔에서 등록한 템플릿 이름 |
| result.regUser | String | 이용 정지 등록자 |
| result.releaseCaller | String | 이용 정지 해제 주체 |
| result.releaseDate | String | 이용 정지 해제 시간. ISO 8601 표준 시간 |
| result.releaseReason | String | 이용 정지 해제 사유 |
| result.releaseUser | String | 이용 정지 해제 등록자 |
| result.seq | Long | 이용 정지 내역 순번 |
| result.templateCode | Long | 콘솔에서 등록한 이용 정지 템플릿 코드 값 |
| result.userId | String | 사용자 ID |

**[Error Code]**

[오류 코드](./error-code/#server)

<br>

## Maintenance

#### Check Under Maintenance

現在、メンテナンスが設定されているかどうかを確認します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.0/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

共通事項確認
<br>


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | TOASTプロジェクトのID |


**[Request Parameter]**

なし
<br>

**[Response Body]**

```json
{
  "header"：{
    "transactionId"："String",
    "resultCode"：0,
    "resultMessage"："String",
    "isSuccessful"：true
  },
  "appId"："",
  "underMaintenance"：true,
  "maintenances"：[
    {
      "typeCode"："APP",
      "beginDate"："2017-01-01T12:10:00+07:00",
      "endDate"："2017-02-01T12:17:00+07:00",
      "url"："http://url.info",
      "message"："maintenance reason"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| underMaintenance | boolean | 現在、メンテナンスを設定しているかどうか |
| maintenances | Object | メンテナンスが設定されている場合、メンテナンスの基本情報 |
| maintenances.typeCode | Enum | APP：ゲームで設定したメンテナンス <br>SYSTEM：Gamebaseシステムで設定したメンテナンス |
| maintenances.beginDate | String | メンテナンス開始時間。ISO 8601 |
| maintenances.endDate | String | メンテナンス終了時間。ISO 8601 |
| maintenances.url | String | 詳細なメンテナンスURL |
| maintenances.message | String | メンテナンスメッセージ |

**[Error Code]**

[エラーコード](./error-code/#server)


## Purchase(IAP)

Gamebaseは、TOAST IAPサービスのサーバーAPIに対して**Wrapping**機能を提供します。Wrapping機能を使用すれば、ユーザーサーバーにおいて一貫したインターフェースでTOASTサービスを使用することができます。


#### Wrapping API

| API | Method | Wrapping URI | IAP URI |
| --- | --- | --- | --- |
| アイテム消費 | POST | /tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq} | /inapp/v3/consume/{paymentSeq}/items/{itemSeq} |
| アイテム照会 | GET | /tcgb-inapp/v1.0/apps/{appId}/item/list/{appSeq} | /standard/item/list/{appSeq} |
| 未消費決済内訳の照会| POST | /tcgb-inapp/v1.0/apps/{appId}/consumable/list | /standard/inapp/v1/consumable/list |

**該当するAPIに対する詳細説明は、次のリンクをご参考ください。**

<br>
[Mobile Service > IAP > APIガイド](/Mobile%20Service/IAP/ja/api-guide/)

<br>

##### API呼び出し例

```
Content-Type：application/json
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key：IgsaAP

POST https://api-gamebase.cloud.toast.com/tcgb-inapp/v1.0/apps/{appId}/consume/{paymentSeq}/items/{itemSeq}
```


## Leaderboard

Gamebaseは、TOAST LeaderboardサービスのサーバーAPIに対して**Wrapping**機能を提供します。Wrapping機能を使用すれば、ユーザーサーバーにおいて一貫したインターフェースでTOASTサービスを使用することができます。


#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Factorに登録されたユーザー数を照会 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| 単一ユーザーのスコア/ランキングを照会 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 複数のユーザーのスコア/ランキングを照会 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 一定範囲の全体スコア/ランキングを照会 | GET | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 単一ユーザーのスコアを登録 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 単一ユーザーのスコア/ExtraDataを登録 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 複数のユーザーのスコアを登録 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 複数のユーザーのスコア/ExtraDataを登録 | POST | /tcgb-leaderboard/v1.0/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| 単一ユーザーのLeaderboardの情報を削除 | DELETE | /tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |


**該当するAPIに対する詳細説明は、次のリンクをご参考ください。**

<br>
[Game > Leaderboard > APIガイド](/Game/Leaderboard/ja/api-guide/)

##### API呼び出し例

```
Content-Type：application/json
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key：IgsaAP

GET https://api-gamebase.cloud.toast.com/tcgb-leaderboard/v1.0/apps/{appId}/factors/{factor}/user-count
```


## Etc

### Support

API呼び出し失敗の原因に対するお問い合わせがある場合、**API呼び出しURL(HTTP bodyがある場合は、bodyと一緒に)とそれに対するレスポンス結果**を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。

<br>

##### API呼び出し例

```
GET https://api-gamebase.cloud.toast.com/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance
```

##### API失敗のレスポンス結果

```json
{
  "header"：{
    "transactionId"："18a1ae42-6b1d-54c8-894e-54e97bca07fq",
    "resultCode"：-4010002,
    "resultMessage"："Gamebase product appKey is invalid, appId:C3JmSctU",
    "traceError"：{
      "trackingTime"：1489726350287,
      "throwPoint"："gateway",
      "uri"："/tcgb-launching/v1.0/apps/C3JmSctU/maintenances/under-maintenance"
    },
    "isSuccessful"：false
  }
}

```
