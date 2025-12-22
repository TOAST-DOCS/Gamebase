## Game > Gamebase > API v1.3ガイド

## 変更事項
'- IAP(In App Purchase) APIのリクエストパラメータおよびレスポンス結果項目追加および削除
- `Push Wrapping` API追加
- Gamebase Access Tokenでログインする時に使用されたIdPのプロフィールおよびトークン情報を取得できる`Get IdP Token and Profiles` API追加
- IdP IdでマッピングされたGamebase userIdを取得する`Get UserId Information with IdP Id` API追加
- 特定期間に退会したユーザーのGamebase userIdを取得する`Withdraw Histories` API追加
- 利用停止および利用停止解除を行う`Ban`, `Ban Release` API追加
- 決済トランザクションを照会する`Get Payment Transaction` API追加
- 未消費決済履歴を照会する`List Consumables` APIで一度にN個のストアを対象に照会できるように`marketIds`追加
- サーバーアドレスがhttps://api-gamebase.nhncloudservice.comに変更。既存アドレスも別途の告知前まで維持
- `List Active Subscriptions` APIレスポンス結果にサブスクリプション商品キャンセル/再購入時に元取引購読のマーケット決済番号を表す`linkedPaymentId`追加
- 購読中の商品をキャンセルする`Cancel Subscriptions`, `Revoke Subscriptions` API追加
- `List Active Subscriptions` API request bodyにGoogle購読の無効化状態をリクエストできる`includeInactiveGoogleStatuses`追加
- `List Active Subscriptions` APIレスポンス結果にRENEWED/RECOVERED発生時間を現す`renewTime`追加
- `List Active Subscriptions` API requestに一度にN個のストアを対象に照会できるように`marketIds`追加
- 利用停止状態のユーザーを照会する`Get Ban Members` API追加
- 購読の現在状態を照会する`Get Subscriptions Status` APIを追加
- `Get Payment Transaction` API request bodyにONEStoreのpurchaseIdまたはpurchaseTokenの値を表す`paymentToken`を追加
- `Withdraw Histories` APIのリクエストパラメータにeventLogTypeを追加
- `SIWA Account Webフック` APIを追加

## Advance Notice

Gamebase Server APIは、RESTful形式で、 次のようなAPIを提供します。サーバーAPIを使用するためには、次のような情報が必要です。

#### Server Address

APIを呼び出すためのサーバーアドレスは、次の通りです。該当するアドレスは、Gamebase Console画面からでも確認できます。
> https://api-gamebase.nhncloudservice.com

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_server_address_v1.3.png)

#### AppId

アプリIDとはNHN CloudプロジェクトのIDのことであり、アプリメニューの画面から確認することができます。

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.2.png)

#### SecretKey

シークレットキー(secret key)は、APIに対するアクセスを制御する方法で、Gamebase Consoleから確認することができます。シークレットキーは、ServerAPIを呼び出すとき、HTTPのヘッダーに必ず設定する必要があります。
> [参考]
> シークレットキーが外部に露出して正しくない呼び出しが発生した場合、**作成**ボタンをクリックして新しくシークレットキーを作成し、その新しいシークレットキーを使用してください。

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.2.png)

#### TransactionId

APIを呼び出すサーバーで内部的にAPIリクエストを管理することができる方法としてTransactionId機能を提供します。呼び出すサーバーからHTTPのヘッダーにトランザクションIDを設定してAPIを呼び出すと、Gamebaseのサーバーは、レスポンスHTTP Header及びレスポンス結果のResponse Body Headerに該当するTransactionIdを設定して結果を送ります。

## Common

#### HTTP Header

APIを呼び出す際には、HTTP Headerに次の項目を設定する必要があります。

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | Required | application/json; charset=UTF-8 |
| X-Secret-Key | Required | SecretKey 説明参考 |
| X-TCGB-Transaction-Id | Optional | TransactionId 説明参考 |

#### API Response

すべてのAPIリクエストに対するレスポンスとして**HTTP 200 OK**を送ります。APIリクエストが成功したかどうかは、Response BodyのHeader項目を参考して判断することができます。

**[Request]**

```
Content-Type：application/json
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key：IgsaAP
GET https://api-gamebase.nhncloudservice.com
```

**[Response]**

```
HTTP/1.1 200 OK
Content-Type：application/json;charset=UTF-8
X-TCGB-Transaction-Id：88a1ae42-6b1d-48c8-894e-54e97aca07fq
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
| transactionId | String | APIをリクエストする際に、HTTP Headerに設定した値<br>該当する値を送らない場合、Gamebaseの内部で作成した値を返す |
| isSuccessful | boolean |成功したかどうか |
| resultCode | int | レスポンスコード<br>成功すると0、失敗するとエラーコードを返す |
| resultMessage | String | レスポンスメッセージ |

#### API Version

APIレスポンス結果の特定変数タイプが変更されるとき、APIバージョンが変更されます。すなわち、新規APIが追加されたりレスポンス結果に新規変数が追加されてもAPIバージョンは変更されません。

> [注意]
> APIレスポンス結果に新規変数が追加されてもJSON解析エラーが発生しないように、使用するJSONライブラリのオプションを追加してください。

<br>
<br>

## Authentication

#### Token Authentication

ログインユーザーに発行されたAccess Tokenが有効かどうか検査します。Access Tokenが正常な場合、該当するユーザーの情報を返します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/tokens/{accessToken}?linkedIdP=false |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |
| userId | String | ログインしたユーザーのID |
| accessToken | String | ログインしたユーザーに発行されたAccess Token |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| linkedIdP | boolean | Optional | true or false(デフォルトはfalse) <br>Access Tokenの発行を受ける際に使用された、IdP関連の情報が含まれているかどうか |

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
| linkedIdP | Object | ログインしたユーザーが使用したIdP情報 |
| linkedIdP.idPCode | String | [ユーザー認証IdP](#identity-provider-code) |
| linkedIdP.idPId | String | IdP ID |
| member.userId | String |ユーザーID |
| member.lastLoginDate | String | 最後にログインした時間<br>はじめてログインしたユーザーは、該当する値なし |
| member.appId | String | appId |
| member.valid | String | [ユーザー状態](#member-valid-code)<br>ログインに成功したユーザー値は"Y" |
| member.regDate | String | ユーザーがアカウントを作成した時間 |
| authList | Array[Object] |ユーザー認証IdP関連の情報 |
| authList[].authSystem | String | Gamebase内部で使用される認証システム<br>今後、ユーザー認証システムを実装する予定 |
| authList[].idPCode | String | [ユーザー認証IdP](#identity-provider-code) |
| authList[].authKey | String | authSystemから発行されたユーザーを区別する値 |
| temporaryWithdrawal | Object | 退会猶予関連情報 <br>validが"T"値でのみ提供 |
| temporaryWithdrawal.gracePeriodDate | String | 退会猶予満了時間ISO 8601 |

**[Error Code]**

[エラーコード](./error-code/#server)


<br/>

#### Get IdP Token and Profiles

クライアントから"Login with IdP"でログインが成功した時に発行されたGamebase Access Tokenです。ログインに使用されたIdPのAccess TokenおよびProfiles情報を照会します。

> [注意]
> IdPのAccess Token有効時間はIdPごとに全て異なり、一般的に短いです。
> クライアントから"Login as the Latest Login IdP"を利用してログインに成功し、その後サーバーで該当APIを呼び出す場合、すでにIdPのAccess Tokenが期限切れになり、IdP情報の取得に失敗することがあります。

<br/>

> [参考]
> IdPのAccess Tokenだけで情報を取得できないIdPも存在します。
> 例) appleid / iosgamecenter : Access TokenでServer to Serverから取得できる情報がありません。

<br/>

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/idps/{idPCode}?accessToken={accessToken} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |
| userId | String | ログインしたユーザーID |
| idPCode | String | [ユーザー認証IdP](#identity-provider-code) |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| accessToken | String | Required | ログインしたユーザーに発行されたGamebase Access Token |

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
| idPProfile | Map<String, Object> | ログインしたユーザーが使用したIdPのプロフィール<br>- IdPごとに全てのレスポンス形式(format)が異なる |
| idPToken | Object | ログインしたユーザーが使用したIdPのAccess Token情報 |
| idPToken.idPCode | String | [ユーザー認証IdP](#identity-provider-code) |
| idPToken.accessToken | String | IdP Access Token |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

## Launching

#### Get Simple Launching

コンソールで設定したサーバーアドレス、インストールURL、現在のメンテナンス状態とメンテナンス時間およびメッセージなど、クライアントアプリ起動時に提供されるLaunching情報を確認できます。
現在のメンテナンス設定だけを確認したい場合は、[Check Maintenance] APIを使用してください。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/launching/simple |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| osCode | Enum | true | [OSコード](#os-code) |
| storeCode | Enum | true | [ストアコード](#store-code) |
| clientVersion | String | true | クライアントバージョン |

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

##### メンテナンス
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
| status | Object | 現在のクライアントのステータスを表す情報 |
| status.code | int | クライアントステータスコード <br><br>正常：200 <br>アップデート推奨：201、アップデート必須：300 <br>サービス終了：302 <br>メンテナンス中：303 |
| status.message | String | クライアントステータスメッセージ |
| app | Object | アプリ情報 |
| app.storeCode | String | [ストアコード](#store-code) |
| app.accessInfo | Object | コンソールアプリ画面で設定した情報 |
| app.accessInfo.serverAddress | String | サーバーアドレス<br>クライアントで設定したサーバーアドレスの優先順位が高い。<br>クライアントサーバーアドレスが未設定の時は、アプリ画面で設定したサーバーアドレスが伝達される。|
| app.accessInfo.csInfo | String | サポート情報 |
| app.relatedUrls | Object | アプリ内で使用するアプリ内URL |
| app.relatedUrls.termsUrl | String | 利用約款 |
| app.relatedUrls.csUrl| String | サポート |
| app.relatedUrls.punishRuleUrl | String | 利用停止規定 |
| app.relatedUrls.personalInfoCollectionUrl | String | 個人情報同意 |
| app.install | Object | アプリインストール情報 |
| app.install.url | String | インストールURL |
| maintenance | Object | メンテナンス情報 |
| maintenance.typeCode | String | メンテナンスタイプコード <br>全体メンテナンス：'SYSTEM'、アプリ別メンテナンス： 'APP' |
| maintenance.beginDate | Date | メンテナンス開始時間ISO 8601 |
| maintenance.endDate | Date | メンテナンス終了時間ISO 8601 |
| maintenance.url | String | メンテナンスURL |
| maintenance.reason | String | メンテナンス理由 |
| maintenance.message | String | 基本メンテナンス理由メッセージ |

**[Error Code]**

[エラーコード](./error-code/#server)

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

## Member

#### Get member

単一会員の詳細情報を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/{userId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |
| userId | String | 照会対象のユーザーID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| includeMemberInfo | boolean | Optional | true or false(デフォルトはtrue) <br>ユーザー端末、OSなどの詳細情報を含むかどうか |

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
| member | Object | 照会されたユーザーの基本情報 |
| member.userId | String |ユーザーID |
| member.valid | Enum | [ユーザー状態](#member-valid-code) |
| member.appId | String | appId |
| member.regDate | String | ユーザーがアカウントを作成した時間 |
| member.lastLoginDate | String | 最後にログインした時間 <br>はじめてログインしたユーザーまたは退会したユーザーは該当する値なし |
| member.authList | Array[Object] |ユーザー認証IdP関連の情報 |
| member.authList[].userId | String |ユーザーID |
| member.authList[].authSystem | String | Gamebase内部で使用される認証システム<br>今後ユーザー認証システム実装予定 |
| member.authList[].idPCode | String | [ユーザー認証IdP](#identity-provider-code) |
| member.authList[].authKey | String | authSystemから発行された、ユーザーを区別する値 |
| member.authList[].regDate | String | IdP情報がユーザーアカウントとマッピングされた時間 |
| temporaryWithdrawal | Object | 退会猶予関連情報 <br>validが"T"値でのみ提供 |
| temporaryWithdrawal.gracePeriodDate | String | 退会猶予満了時間ISO 8601 |
| memberInfo | Object | ユーザーに対する付加情報<br>退会したユーザーは該当する情報なし |
| memberInfo.deviceCountryCode | String |ユーザー端末の国家設定 |
| memberInfo.usmCountryCode | String |ユーザーUSIMの国家コード|
| memberInfo.language | String | ユーザー端末言語、 ISO 639-1 |
| memberInfo.osCode | String | [OSコード](#os-code) |
| memberInfo.telecom | String | 通信キャリア |
| memberInfo.storeCode | String | [ストアコード](#store-code) |
| memberInfo.network | String | ネットワーク環境<br>3g、Wi-Fiなど|
| memberInfo.deviceModel | String |ユーザー端末のモデル名 |
| memberInfo.osVersion | String |ユーザー端末のOSバージョン |
| memberInfo.sdkVersion | String | SDKバージョン |
| memberInfo.clientVersion | String | クライアントバージョン |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get Members

複数の会員情報を簡素化して照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 照会対象のユーザーID|

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
| memberList | Array[Object] | 照会されたユーザーの基本情報 |
| memberList[].userId | String |ユーザーID |
| memberList[].valid | Enum | [ユーザー状態](#member-valid-code) |
| memberList[].appId | String | appId |
| memberList[].regDate | String | アカウント作成時間 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get IdP infomation

ユーザーIDにマッピングされたIdP情報を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/auth/authKeys |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |

**[Request Body]**

```json
["userId", "userId", "userId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 照会対象のユーザーID |

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
| result | Array[Object] | 照会されたユーザーの基本情報 <br>userIdがkey、IdP情報がvalueのobject|
| authkey | String | authSystemから発行された、ユーザーを区別する値 |
| IdPCode | String | [ユーザー認証IdP](#identity-provider-code) |
| authSystem | String | Gamebase内部で使用される認証システム<br>今後ユーザー認証実装予定 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get userId infomation with auth key

ユーザー認証キーにマッピングされたユーザーIDを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-member/v1.3/apps/{appId}/members/userIds/authKeys?authSystem={authSystem} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| authSystem | String | Required | Gamebase内部で使用される認証システム<br>今後ユーザー認証システム実装予定 <br>現在はgbid |

**[Request Body]**

```json
["authKey", "authKey", "authKey"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | authSystemから発行されたauthKey |

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
| result | Array[Object] | 照会されたユーザーの基本情報 <br>authKeyがkeyで、userIdがvalueのobject|

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get UserId Information with IdP Id

IdP IDでマッピングされたユーザーID情報を照会します。

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
| idPCode | String | [ユーザー認証IdP](#identity-provider-code) |

**[Request Body]**

```json
["idPId", "idPId", "idPId"]
```

| Type | Required | Value |
| --- | --- | --- |
| Array[String] | Required | 照会対象ユーザーのIdP ID <br> 照会対象リストサイズは最大300 |

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
| result | Map<String, String> | 照会されたユーザーのID情報 <br>- IdP IDがkey、Gamebase userIdがvalue<br>- 照会リクエストしたIdP IDを持つuserId情報がない場合はレスポンス結果に存在しません。 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Ban

ユーザーを利用停止状態に変更します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/ban |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "userIdList": [
        "userId-1", "userId-2"
    ],
    "banTypeCode": "TEMPORARY",
    "end": "2022-05-10T06:03:50.000+09:00",
    "templateCode": 0,
    "banReason": "string",
    "flags": "leaderboard",
    "banCaller": "APP_SERVER",
    "regUser": "GAME-SERVER"
}
```

| Key | Type | Description |
| --- | --- | --- |
| userIdList | Array[String] | 利用停止対象ユーザーID |
| banTypeCode | Enum | 利用停止タイプ。 TEMPORARY or PERMANENT |
| end | String | 利用停止終了時間(ISO 8601標準時間) <br>- TEMPORARYタイプのときの必須値 |
| templateCode | Integer | 利用停止時に表示されるメッセージに使われるテンプレートのテンプレートコード <br>- この値はConsole**利用停止 > テンプレート** 詳細照会画面で確認可能 |
| banReason | String | 利用停止理由 |
| flags | String | 利用停止ユーザーのリーダーボードデータも一緒に削除したい場合は「leaderboard」に設定 |
| banCaller | String | 利用停止APIを呼び出した主体。「APP_SERVER」固定値に設定 |
| regUser | String | Console利用停止画面で表示する名前 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "failedUserIdList": ["userId-1"]
}
```

| Key | Type | Description |
| --- | --- | --- |
| failedUserIdList | Array[String] | 利用停止登録に失敗したユーザーID |

**[Error Code]**

[エラーコード](./error-code/#server)

</br>

#### Ban Histories

ユーザー利用停止履歴を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | Required | 利用停止履歴照会開始時間(ISO 8601標準時間、UTF-8エンコード必要) <br>例) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | Required | 利用停止履歴照会終了時間(ISO 8601標準時間、UTF-8エンコード必要) <br>beginとendの間の時間に利用停止した場合、照会結果に存在 |
| page | String | Optional | 照会するページ。0から開始 |
| size | String | Optional | 1ページ当たりのデータ数 |
| order | String | Optional | 照会データソート方法。 ASC or DESC |

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
            "userId": "String",
            "banCaller": "CONSOLE",
            "banReason": "String",
            "banType": "TEMPORARY",
            "beginDate": "2019-08-27T17:41:05+09:00",
            "endDate": "2019-08-28T17:41:05+09:00",
            "flags": "String",
            "name": "String",
            "templateCode": 0
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 照会されたページ情報 |
| pagingInfo.first | boolean | 最初のページの場合はtrue |
| pagingInfo.last | boolean | 最後のページの場合はtrue |
| pagingInfo.numberOfElements | int | 総データ数 |
| pagingInfo.page | int | ページ番号 |
| pagingInfo.size | int | 1ページ当たりのデータ数 |
| pagingInfo.totalElements | int | 総データ数 |
| pagingInfo.totalPages | int | 総ページ数 |
| result | Array[Object] | 照会された利用停止内訳 |
| result.userId | String | ユーザーID |
| result.banCaller | String | 利用停止指示者 |
| result.banReason | String | 利用停止理由 |
| result.banType | String | 利用停止タイプ。TEMPORARYまたはPERMANENT |
| result.beginDate | String | 利用停止開始時間。ISO 8601標準時間|
| result.endDate | String | 利用停止終了時間。ISO 8601標準時間<br>PERMANENTタイプの場合、この値は存在しない |
| result.flags | String | コンソールから利用停止を登録した時、リーダーボード削除を選択した場合は'leaderboard'を返す |
| result.name | String | コンソールで登録したテンプレート名 |
| result.templateCode | Long | コンソールで登録した利用停止テンプレートコード値 |

**[Error Code]**

[エラーコード](./error-code/#server)

#### Ban Release

ユーザーを利用停止解除状態、すなわち正常状態に変更します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/ban |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "userIdList": [
        "userId-1", "userId-2"
    ],
    "banReleaseReason": "string",
    "banReleaseCaller": "APP_SERVER",
    "releaseUser": "GAME-SERVER"
}
```

| Key | Type | Description |
| --- | --- | --- |
| userIdList | Array[String] | 利用停止解除対象ユーザーID |
| banReleaseReason | String | 利用停止解除理由 |
| banReleaseCaller | String | 利用停止解除APIを呼び出した主体。「APP_SERVER」固定値に設定 |
| releaseUser | String | Console利用停止解除画面で表示する名前 |

**[Response Body]**

```json
{
    "header": {
        "transactionId": "String",
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "failedUserIdList": ["userId-1"]
}
```

| Key | Type | Description |
| --- | --- | --- |
| failedUserIdList | Array[String] | 利用停止解除に失敗したユーザーID |

**[Error Code]**

[エラーコード](./error-code/#server)

</br>

#### Ban Release Histories

ユーザー利用停止解除履歴を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/members/bans/release |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | Required | 利用停止解除履歴照会開始時間(ISO 8601標準時間、UTF-8エンコード必要) <br>例) yyyy-MM-dd'T'HH:mm:ss.SSSXXX |
| end | String | Required | 利用停止解除履歴照会終了時間(ISO 8601標準時間、UTF-8エンコード必要) <br>beginとendの間の時間に利用停止が解除された場合、照会結果に存在 |
| page | String | Optional | 照会するページ。0から開始 |
| size | String | Optional | 1ページ当たりのデータ数 |
| order | String | Optional | 照会データソート方法。 ASC or DESC |

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
            "userId": "String",
            "banCaller": "CONSOLE",
            "banReason": "String",
            "banType": "TEMPORARY",
            "beginDate": "2019-08-27T17:41:05+09:00",
            "endDate": "2019-08-29T17:41:05+09:00",
            "flags": "String",
            "name": "String",
            "templateCode": 0,
            "releaseCaller": "CONSOLE",
            "releaseDate": "2019-08-30T18:41:05+09:00",
            "releaseReason": "String"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 照会されたページ情報 |
| pagingInfo.first | boolean | 最初のページの場合はtrue |
| pagingInfo.last | boolean | 最後のページの場合はtrue |
| pagingInfo.numberOfElements | int | 総データ数 |
| pagingInfo.page | int | ページ番号 |
| pagingInfo.size | int | 1ページ当たりのデータ数 |
| pagingInfo.totalElements | int | 総データ数 |
| pagingInfo.totalPages | int | 総ページ数 |
| result | Array[Object] | 照会された利用停止情報 |
| result.userId | String | ユーザーID |
| result.banCaller | String | 利用停止指示者 |
| result.banReason | String | 利用停止理由 |
| result.banType | String | 利用停止タイプ。TEMPORARYまたはPERMANENT |
| result.beginDate | String | 利用停止開始時間。ISO 8601標準時間 |
| result.endDate | String | 利用停止終了時間。ISO 8601標準時間 |
| result.flags | String | コンソールから利用停止を登録した時、リーダーボード削除を選択した場合は「leaderboard」を返す |
| result.name | String | コンソールで登録したテンプレート名 |
| result.templateCode | Long | コンソールで登録した利用停止テンプレートコード値 |
| result.releaseCaller | String | 利用停止解除者 |
| result.releaseReason | String | 利用停止解除理由 |
| result.releaseDate | String | 利用停止解除時間 |


**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Validate TransferAccount

ゲストアカウントを移行するために、すでに発行されているIDおよびパスワードの有効性をチェックします。有効なTransferAccountの場合、発行されたuserId情報を返します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/transfer-account |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

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
| account.id | String | 有効性検証を行うID |
| account.password | String | 有効性検証を行うパスワード |

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
| member | Object | 照会されたユーザーの基本情報 |
| member.userId | String | ユーザーID |
| member.valid | Enum | [ユーザー状態](#member-valid-code) |
| member.appId | String | アプリID |
| member.regDate | String | ユーザーがアカウントを作成した時間 |
| member.lastLoginDate | String | 最後にログインした時間 <br>初めてログインしたユーザーは該当値がない |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

#### Withdraw

ユーザーアカウントを退会処理します。

> [参考]
> SDKの退会APIを使用せず、サーバー退会APIを使用してアカウント退会を実装した場合、クライアントでは退会成功後にSDKのlogout APIを呼び出してキャッシュされているトークンなどのデータを削除する必要があります。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}?regUser={regUser} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |
| userId | String | 退会対象ユーザーID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | Required | 退会をリクエストしたシステムまたは運営者情報。空白なしで入力 <br> - この情報はConsole > 「メンバー」ページの「退会履歴」画面で確認可能 <br> - 退会履歴画面は退会した利用者の照会時にのみ表示される |

**[Request Body]**

なし

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

[エラーコード](./error-code/#server)


<br>

#### Withdraw

ユーザーアカウントを退会処理します。

> [参考]
> SDKの退会APIを使用せず、サーバー退会APIを使用してアカウント退会を実装した場合、クライアントでは退会成功後にSDKのlogout APIを呼び出してキャッシュされているトークンなどのデータ削除が必要です。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}?regUser={regUser} |

**[Request Header]**

共通事項の確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |
| userId | String | 退会対象ユーザーID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| regUser | String | Required | 退会をリクエストしたシステムまたはユーザー情報<br> - この情報はConsole > 「メンバー」ページの「退会履歴」画面で確認可能 |

**[Request Body]**

なし

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

[エラーコード](./error-code/#server)

</br>

#### Withdraw Histories

特定期間に退会したユーザーを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-member/v1.3/apps/{appId}/logs/withdrawal |

**[Request Header]**

共通事項の確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| begin | String | Required | 履歴照会開始時間(ISO 8601標準時間、 UTF-8 Encoding必要) <br>**過去1年以内のデータのみ提供** |
| end | String | Required | 履歴照会終了時間(ISO 8601標準時間、 UTF-8 Encoding必要) <br>例) yyyy-MM-dd'T'HH:mm:ss.SSSXXX / 2021-09-11T00%3a00%3a00%2b09%3a00 |
| page | String | Optional | 照会したいページ。 0から開始 |
| size | String | Optional | 1ページ当たりのデータ数 |
| order | String | Optional | 照会データのソート方法。 ASC or DESC |
| eventLogType | Enum | Optional | [退会イベント発生経路](#withdrawal-event-type) |

**[Response Body]**

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "transactionId": "String",
        "isSuccessful": true
    },
    "pagingInfo": {
        "totalPages": 1,
        "totalElements": 2,
        "numberOfElements": 2,
        "first": true,
        "last": true,
        "page": 0,
        "size": 100
    },
    "result": [
        {
            "userId": "String",
            "date": "2022-03-27T17:40:00+09:00",
            "regUser": null
        },
        {
            "userId": "String",
            "date": "2022-03-27T17:41:05+09:00",
            "regUser": "String"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 照会されたページング情報 |
| pagingInfo.first | boolean | 最初のページの場合はtrue |
| pagingInfo.last | boolean | 最後のページの場合はtrue |
| pagingInfo.numberOfElements | int | 総データ数 |
| pagingInfo.page | int | ページ番号 |
| pagingInfo.size | int | 1ページ当たりのデータ数 |
| pagingInfo.totalElements | int | 総データ数 |
| pagingInfo.totalPages | int | 総ページ数 |
| result | Array[Object] | 照会された退会ユーザーの内容 |
| result.userId | String | ユーザーID |
| result.date | String | 退会日時 |
| result.regUser | String | 退会APIを呼び出した主体<br>- この値が**null**の場合はclient SDKで呼び出し|

**[Error Code]**

[エラーコード](./error-code/#server)

</br>

#### SIWA Account Webhook

**Sign In with Apple (SIWA)** ユーザーのアカウント状態変更をAppleサーバーから通知を受け取り処理するWebフック APIです。
このWebフックのURIをApple Developer SiteのSign In with Appleサービス設定に登録する必要があります。

> [参考]
> 該当APIはAppleサーバーが直接呼び出すため、ヘッダに別途認証キー(Secret Key)の設定は必要ありません。
</br>

##### 対応イベント及び処理ロジック
該当Webフックイベントは、同意撤回(consent-revoked)とアカウント削除(account-delete)の2つをサポートし、イベントに応じて次のように処理されます。

- 同意撤回 (consent-revoked)
    - 処理: ユーザーのアカウントは維持されますが,現在発行されているGamebase Access Tokenは即時失効します。
- アカウント削除 (account-delete)
    - 処理: ユーザーのアカウントは即時退会処理されます。
    - 退会したアカウントは**Withdraw Histories** APIにて**eventLogType=WAAI**パラメータで照会できます。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/webhooks/apple/notifications |


**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

</br>
</br>

## Maintenance

#### Check Under Maintenance

現在、メンテナンスが設定されているかどうかを確認します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-launching/v1.3/apps/{appId}/maintenances/under-maintenance |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトのID |

**[Request Parameter]**

なし

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
    "maintenance": {
        "typeCode": "APP",
        "beginDate": "2017-01-01T12:10:00+07:00",
        "endDate": "2017-02-01T12:17:00+07:00",
        "url": "http://url.info",
        "reason" : "maintenance reason",
        "message": "maintenance message",
        "targetStores": [
            "GG",
            "AS",
            "ONESTORE"
        ]
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| underMaintenance | boolean | 現在、メンテナンスを設定しているかどうか |
| maintenance | Object | メンテナンスが設定されている場合、メンテナンスの基本情報 |
| maintenance.typeCode | Enum | APP：ゲームで設定したメンテナンス <br>SYSTEM：Gamebaseシステムで設定したメンテナンス |
| maintenance.beginDate | String | メンテナンス開始時間。ISO 8601 |
| maintenance.endDate | String | メンテナンス終了時間。ISO 8601 |
| maintenance.url | String | 詳細なメンテナンスURL |
| maintenance.message | String | メンテナンスメッセージ |
| maintenance.targetStores | Array[Enum] | 特定のクライアントに対してのみメンテナンスを設定した時、メンテナンスが設定されたクライアントの[ストアコード](#store-code) |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

## Coupon

#### Check Validation And Consume Coupon

Consoleを通して発行されたクーポンコードに対して、有効性の検証およびクーポン状態の変更を行います。有効なクーポンの場合、消費状態に変更し、レスポンス結果で支給するアイテム情報を返します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-gateway/v1.3/apps/{appId}/members/{userId}/coupons/{couponCode}?storeCode={storeCode} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |
| userId | String | クーポンを使用するuserId |
| couponCode | String | クーポンコード |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| storeCode | String | Optional | コンソールで特定ストアからインストールしたアプリのみクーポンを使用できるように設定した場合、ストアコードを伝達する必要があります。<br>- 全てのストアの場合はALLまたはパラメータ省略可能<br>- [ストアコード](#store-code) |

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
| result | Object | クーポン情報 |
| result.title | String | クーポンの名前 |
| result.benefits | Array[Object] | 支給するアイテム情報 |
| result.benefits.itemId | String | アイテムID |
| result.benefits.amount | Integer | アイテムの数 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

## Purchase (IAP)

#### Consume

Google Play Store、App Store、ONEStoreなどのストア決済が正常に完了した場合、ユーザーにアイテムを支給し、サーバー内部的に履歴を記録した後、Gmaebaseに決済消費を伝えます。決済1件につき1回のみ決済を消費することができ、決済の状態が正常でない場合は消費されません。

> [参考]
> 商品登録時、商品タイプが消費(CONSUMABLE)または消費性購読(CONSUMABLE_AUTO_RENEWABLE)アイテム決済に対してのみ消費(consume)処理されます。
> 決済1件につき1回消費可能で、決済消費を行っていない決済はIAPではアイテムを支給していないものとみなします。

消費(consume)していない決済は、SDKおよびサーバーの未消費決済履歴照会APIで照会できます。 APIでは未消費決済履歴が存在していても、ゲームサーバー内部にアイテム支給履歴がある場合は、ゲームサーバー内部支給履歴を優先的に判断してください。
(ネットワーク障害などでAPI timeoutが発生した場合、Gamebaseでは支給が完了しているが、ゲームサーバーではAPIレスポンス失敗でユーザーにはアイテム支給がされない場合がある)

> [参考]
> ゲーム内部的にアイテム支給履歴を全て管理できない場合は、該当APIのrequest timeoutを10秒以上にしてAPI timout発生時だけでも履歴を記録して重複支給または未支給問題への対策が必要。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consume |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "paymentSeq": "2019091931571201",
    "accessToken": "90fD1bs1guXwY6aZ7rseEKYW_6gMCISjDASgten4MD6O7XZD7VRjZcs8OTm8lOQVFTegoY4WK78P2WQCMm7cx"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | Required | 決済番号 |
| accessToken | String | Required  | 決済認証トークン(ログイン認証トークンではない)  |

> [参考]
> クライアントでrequestPurchase APIを呼び出した時、返されたpurchaseTokenの値がaccessTokenに使用

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
| result | Object | 決済基本情報 |
| result.price | Long | 決済価格 |
| result.currency  | String  | 決済通貨 |
| result.productSeq | Long | 決済アイテム番号(consoleに登録されたアイテム固有番号) |
| result.marketId | String | [ストアコード](#store-code) |
| result.gamebaseProductId | String | Gamebase商品ID<br>コンソールで商品登録時にユーザーが入力した値 |
| result.payload | String | SDKで設定した追加情報 |

> [参考]
> クライアントで使用するSDKバージョンや決済APIによって、レスポンス結果にgamebaseProductId値が存在します。

> [参考]
> ゲームサーバーではアイテム番号またはストアコードとGamebase商品IDで指定した商品(アイテム)を支給できますが、1個のストアアイテムIDにN個のGamebase商品を登録した場合、ストアコードとGamebase商品IDで支給する必要があります。

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### List Consumables

決済が完了したが、まだ消費(Consume)していない未消費決済履歴を照会できます。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/consumable |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "marketIds": ["GG", "AS"],
    "userId": "QXG774PMRZMWR3BR"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | Optional | [ストアコード](#store-code)<br>- **deprecated**予定で*marketIds*使用 |
| marketIds | Array | Optional | [ストアコード](#store-code)<br>- 空の値(またはnull)の場合、全てのストアを対象に照会<br> - ただし、AMAZONストアが含まれる全てのストアを照会する場合、明示的に照会する**すべてのストア**を羅列する必要がある |
| userId | String | Required  | ユーザーID  |

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
            "paymentId" : "Store Reference Key",
            "gamebaseProductId": "gamebase_prod_001",
            "purchaseTime": "2020-06-02T13:38:56+09:00",
            "payload": "additional info",
            "isTestPurchase" : false
        },
        {
            "paymentSeq": "2016122110023125",
            "productSeq": 1000292,
            "currency": "KRW",
            "price": 1000.0,
            "marketId": "AS",
            "accessToken": "7_3zXyNJub0FNLed3m9XRAAXsSxLWq698t8QyTzk3NeeSoytKxtKGjldTc1wkSktgzjsfkVTKE50DoGihsAvGQ",
            "paymentId" : "Store Reference Key",
            "gamebaseProductId": "gamebase_prod_002",
            "purchaseTime": "2020-06-02T13:37:42+09:00",
            "isTestPurchase" : false
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 決済基本情報 |
| result[].paymentSeq | String |  Gamebaseで発行された決済Transaction ID |
| result[].productSeq | Long | 決済アイテム番号(consoleに登録されたアイテム固有番号) |
| result[].currency  | String  | 決済通貨 |
| result[].price | Float | 決済価格 |
| result[].accessToken | String | 決済認証トークン |
| result[].paymentId | String | ストアで発行された決済ID |
| result[].marketId | String | [ストアコード](#store-code) |
| result[].gamebaseProductId | String | Gamebase商品ID<br>コンソールに商品登録した時にユーザーが入力した値 |
| result[].purchaseTime | String | 決済発生日時 |
| result[].payload | String | SDKで設定した追加情報<br>Amazonストアはその値がない場合があります |
| result[].isTestPurchase | boolean | テスト決済かどうか |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

#### Get Payment Transaction

クライアントSDKを介して取得した未消費決済履歴が有効かどうかを確認できます。
(サーバーでアイテム支給(consume) APIを呼び出す前に、決済番号(paymentSeq)と決済認証トークン(accessToken)の有効性チェックを行いたい場合は該当APIを呼び出す)

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /tcgb-inapp/v1.3/apps/{appId}/payment/transaction?accessToken={accessToken} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

| Name | Type | Required |  Value |
| --- | --- | --- | --- |
| accessToken | String | Required | 決済認証トークン(purchaseToken) |

**[Request Body]**

なし

**[Response Body]**

```json
{
    "header": {
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "isSuccessful": true
    },
    "result": {
        "paymentSeq": "2022041110385239",
        "productSeq": 1003150,
        "currency": "EUR",
        "price": 2.29,
        "marketId": "AS",
        "accessToken": "-Fr8Y7_dvv5qhdd6qVHbs7gKnkX0r7EKPvuK6CI-UBBekc1rE9CVbMKVCNuw6ZtwkBGlzeIHg6DdjaRVeaW7GYlPF4vRa50L8umB6tdBvk8",
        "paymentId" : "Store Reference Key",
        "paymentToken" : "19062709124410111299",        
        "productType": "CONSUMABLE",
        "userId": "AS@QW4M1GM7W97YJDCN",
        "gamebaseProductId": "qa_ksw_prod_as_001",
        "purchaseTime": "2022-04-11T16:47:01+09:00",
        "payload" : "string",
        "isTestPurchase": true,
        "isConsumable": false
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | 決済情報 |
| result.paymentSeq | String | Gamebaseで発行された決済番号 |
| result.productSeq | Long | アイテム番号<br>コンソールで商品登録時、外部ストアアイテムに対して自動作成された値 |
| result.currency  | String | 決済通貨 |
| result.price | Float | 決済価格 |
| result.marketId | String | [ストアコード](#store-code) |
| result.accessToken | String | 決済認証トークン |
| result.paymentId | String | ストアで発行された決済ID |
| result.paymentToken | String | ストアで発行された決済トークンで、ONEStoreの決済案件にのみ存在する<br>- ONEStore V5: purchaseId<br> - ONEStore V6,7: purchaseToken |
| result.productType | String  | 商品(アイテム)タイプ<br>- 消費：CONSUMABLE<br>- 消費性購読：CONSUMABLE_AUTO_RENEWABLE<br>- 購読：AUTO_RENEWABLE |
| result.userId | String  | ユーザーID  |
| result.gamebaseProductId | String | Gamebase商品ID<br>コンソールで商品登録時、ユーザー入力値 |
| result.purchaseTime | String | 決済発生日時 |
| result.payload | String | SDKで設定した追加情報<br>Amazonストアは該当値がない場合あり |
| result.isTestPurchase | boolean | テスト決済かどうか<br>- true：テスト決済 |
| result.isConsumable | boolean | 消費API呼び出し可否<br>- true：現在未消費状態で消費API呼び出し可能 |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

### List Active Subscriptions

ユーザーが現在定期購入中の決済を照会できます。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/active-subscriptions |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "marketIds": [
        "GG",
        "AS"
    ],
    "packageName": "com.nhncloud.gamebase",
    "userId": "QXG774PMRZMWR3BR",
    "includeInactiveGoogleStatuses" : [
        "ON_HOLD"
    ]
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| marketId | String | Optional | [ストアコード](#store-code)<br>- **deprecated**予定で*marketIds*を使用 |
| marketIds | Array[String] | Optional | [ストアコード](#store-code)<br>- 空の値(またはnull)の場合、全体ストアを対象に照会 |
| packageName | String | Required | コンソールに登録したストアアプリID |
| userId | String | Required | ユーザーID |
| includeInactiveGoogleStatuses | Array[String] | Optional | レスポンス結果に含める**Google購読の無効化状態**<br>- 現在'ON_HOLD'状態のみサポート |
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
            "linkedPaymentId": "GPA.3358-3220-2629-70624",
            "price": 1000.0,
            "currency": "KRW",
            "gamebaseProductId": "gamebase_renewal_001",
            "payload": "additional info",
            "purchaseTime": "2020-06-02T13:38:56+09:00",
            "expiryTime": "2020-06-02T13:48:56+09:00",
            "renewTime" : "2020-06-02T13:50:56+09:00",
            "isTestPurchase" : false,
            "referenceStatus" : "PURCHASED"
        }
    ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 決済基本情報 |
| result[].marketId  | String  | [ストアコード](#store-code) |
| result[].userId  | String  | ユーザーID  |
| result[].paymentSeq | String  | 決済番号 |
| result[].accessToken | String | 決済認証トークン |
| result[].productSeq | Long | 決済アイテム番号(consoleに登録されたアイテム固有番号) |
| result[].productId | String  | ストアに登録された商品(アイテム)識別子 |
| result[].productType | String  | 商品(アイテム)タイプ<br>定期購入： AUTO_RENEWABLE |
| result[].currency  | String  | 決済通貨 |
| result[].price | Float | 決済価格 |
| result[].originalPaymentId | String | 最初のストア決済ID |
| result[].paymentId | String | 最近更新されたストア決済ID |
| result[].linkedPaymentId | String | 購読キャンセル/再購入時、元取引の決済ID<br>Google Playストアのみサポート |
| result[].gamebaseProductId | String | Gamebase商品ID<br>コンソールから商品を登録した時にユーザーが入力した値 |
| result[].payload | String | SDKで設定した追加情報 |
| result[].purchaseTime | String | 最近更新された時間 |
| result[].expiryTime | String | 定期購入終了時間 |
| result[].renewTime | String | RENEWED/RECOVERED発生時間 |
| result[].isTestPurchase | boolean | テスト決済かどうか |
| result[].referenceStatus | String | 決済システム(アプリ内決済、外部決済)が提供する[決済参照状態](#store-reference-status)<br>現在Google Playストアのみサポート |

**[Error Code]**

[エラーコード](./error-code/#server)


<br>

### Cancel Subscriptions

購読中の商品について、更新時点以降は更新されなくなり、現在の購読満了まで維持します。

> [参考]
> 現在Google Playストアのみサポートします。
**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/subscriptions/cancel |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "paymentSeq": "2022112110400545",
    "accessToken": "NczL3n4TumMF8n9oRR5l8zXDyMXRVjxSRks0Lk1Saob2A9rdAupqjZSrQ0-hb2GOSFwTx5uDDchH8EB-EkWGGQ"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | Required | 決済番号 |
| accessToken | String | Required | 決済認証トークン |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

### Revoke Subscriptions

現在購読中の商品についてすぐに購読をキャンセルし、返金を行います。

> [参考]
> 現在Google Playストアのみサポートします。
**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/subscriptions/revoke |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
    "paymentSeq": "2022112110400545",
    "accessToken": "NczL3n4TumMF8n9oRR5l8zXDyMXRVjxSRks0Lk1Saob2A9rdAupqjZSrQ0-hb2GOSFwTx5uDDchH8EB-EkWGGQ"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| paymentSeq | String | Required | 決済番号 |
| accessToken | String | Required | 決済認証トークン |

**[Response Body]**

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

**[Error Code]**

[エラーコード](./error-code/#server)

<br>

### Get Subscriptions Status

サブスクリプション商品の現在状態を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /tcgb-inapp/v1.3/apps/{appId}/subscriptions/status |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appId | String | NHN CloudプロジェクトID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
  "payments": [
    {
      "paymentSeq": "2023082410408370",
      "accessToken": "Yk3sMxc-JSaGLLY0X-DnajXDyMXRVjxSRks0Lk1SaoaO7RD7VRjZcs8OTm8lOQVFoP71pgjAb_INjl0Y5KN8_A"
    },
    {
      "paymentSeq": "2023082410408383",
      "accessToken": "qEP1ZeV_ORmJdlNr9xDm9DXDyMXRVjxSRks0Lk1SaoaPiqPX4dG6UstXeWUt1NujyQAwH8BWQJueaPRfmnRyeg"
    }
  ]
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| payments | Array[Object] | 購読決済情報。最大10個まで入力 |
| payments[].paymentSeq | String | Required | 決済番号 |
| payments[].accessToken | String | Required | 決済認証トークン |

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
      "userId": "QXG774PMRZMWR3BR",
      "paymentSeq": "2023082410408389",
      "accessToken": "uddRuwkHm9nFIHjvVuDS2jXDyMXRVjxSRks0Lk1SaoaPiqPX4dG6UstXeWUt1NujyQAwH8BWQJueaPRfmnRyeg",
      "paymentId": "GPA.3333-7714-3477-48799..5",
      "originalPaymentId": "GPA.3333-7714-3477-48799",
      "purchaseTime": "2022-05-16T09:59:27+09:00",
      "expiryTime": "2023-08-24T12:48:45+09:00",
      "renewTime": "2023-08-24T12:48:45+09:00",
      "referenceStatus": "REPURCHASED"
    },
    {
      "userId": "QXG774PMRZMWR3BR",
      "paymentSeq": "2023082410408381",
      "accessToken": "SFkxJL2sk8NlbsPe8ivVGDXDyMXRVjxSRks0Lk1SaoaO7RD7VRjZcs8OTm8lOQVFoP71pgjAb_INjl0Y5KN8_A",
      "paymentId": "GPA.3395-4426-6912-10820..5",
      "originalPaymentId": "GPA.3395-4426-6912-10820",
      "purchaseTime": "2022-05-16T09:59:27+09:00",
      "expiryTime": "2023-08-24T12:48:45+09:00",
      "renewTime": "2023-08-24T12:48:45+09:00",
      "referenceStatus": "EXPIRED"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Array[Object] | 決済基本情報 |
| result[].userId | String  | ユーザーID  |
| result[].paymentSeq | String  | 決済番号 |
| result[].accessToken | String | 決済認証トークン |
| result[].paymentId | String | 最近更新されたストア決済ID |
| result[].originalPaymentId | String | 最初のストア決済ID |
| result[].purchaseTime | String | 最近更新された時間 |
| result[].expiryTime | String | 購読の有効期限 |
| result[].renewTime | String | RENEWED/RECOVERED発生時間 |
| result[].referenceStatus | String | 決済システム(アプリ内決済、外部決済)が提供する[決済参照状態](#store-reference-status)<br>現在Google Playストアのみサポート |

**[Error Code]**

[エラーコード](./error-code/#server)

<br>
<br>

## Leaderboard

Gamebaseは、NHN Cloud LeaderboardサービスのサーバーAPIに対して**Wrapping**機能を提供します。Wrapping機能を使用すれば、ユーザーサーバーにおいて一貫したインターフェースでNHN Cloudサービスを使用することができます。

> [参考]
> Gamebaseを有効化すると、Leaderboard Appkey設定を行わずにGamebase Wrapping APIを呼び出してLeaderboard機能を使用できます。

<br>

#### Wrapping API
| API | Method | Wrapping URI | Leaderboard URI |
| --- | --- | --- | --- |
| Factorに登録されたユーザー数を照会<br>- Get user count in factor | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/user-count |
| Factor全体数検索<br>- Get total factor count | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factor-count | /leaderboard/v2.0/appkeys/{appKey}/factor-count |
| Factor情報照会<br>- Get factor info<br>- Get multiple factor info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors | /leaderboard/v2.0/appkeys/{appKey}/factors |
| 単一ユーザーのスコア/ランキングを照会<br>- Get single user info | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?userId={userId} |
| 複数のユーザーのスコア/ランキングを照会<br>- Get multiple user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/get-users | /leaderboard/v2.0/appkeys/{appKey}/get-users |
| 一定範囲の全体スコア/ランキングを照会<br>- Get multiple user info by range | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?start={start}&size={size} | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users?start={start}&size={size} |
| 特定順位のユーザーを検索<br>- Get selected rank user info | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |
| 特定ユーザーの順位および上位、下位ユーザーの順位検索<br>- Get multiple user info by pivot user | GET | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} | /leaderboard/v2.0/appkeys/{appkey}/factors/{factor}/users?userId={userId}&prevSize={prevSize}&nextSize={nextSize} |
| 単一ユーザーのスコアを登録<br>- Set single user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score |
| 単一ユーザーのスコア/ExtraDataを登録<br>- Set single user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users/{userId}/score-with-extra | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users/{userId}/score-with-extra |
| 複数のユーザーのスコアを登録<br>- Set multiple user score | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores | /leaderboard/v2.0/appkeys/{appKey}/scores |
| 複数のユーザーのスコア/ExtraDataを登録<br>- Set multiple user score with extra data | POST | /tcgb-leaderboard/v1.3/apps/{appId}/scores-with-extra | /leaderboard/v2.0/appkeys/{appKey}/score-with-extra |
| ユーザーLeaderboard情報を削除<br>- Delete single user info<br>- Delete multiple user info | DELETE | /tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/users | /leaderboard/v2.0/appkeys/{appKey}/factors/{factor}/users |

<br/>

**該当するAPIに対する詳細説明は、次のリンクをご参考ください。**
Gamebase Wrapping APIとマッピングされたLeaderboard APIのスペックは、以下のガイドを参考にしてください。
Leaderboard Appkeyを設定しないで、Gamebase AppIdおよびSecretKeyを利用してGamebase Wrapping Leaderboard APIを呼び出せます。


[Leaderboard APIガイド](https://docs.nhncloud.com/ja/Game/Leaderboard/ja/api-guide/)

<br/>

##### API呼び出し例

```
GET https://api-gamebase.nhncloudservice.com/tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count

Content-Type: application/json
X-TCGB-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
```

<br/>
<br/>

## Push

Gamebaseは、NHN Cloud PushサービスのサーバーAPIで**Wrapping**機能を提供します。Wrapping機能を使用すると、ユーザーサーバーで一貫したインターフェイスでNHN Cloudサービスを使用できます。

> [参考]
> Gamebaseを有効化すると、Push Appkeyの設定を行わずにGamebase Wrapping APIを呼び出してPush機能を使用できます。

<br>

#### Wrapping API
|    | API | Method | Wrapping URI | Push URI |
| --- | --- | --- | --- | --- |
| メッセージ | 送信 | POST | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 照会 | GET | /tcgb-push/v1.3/apps/{appId}/messages | /push/v2.4/appkeys/{appkey}/messages |
|   | 送信ログ照会 | GET | /tcgb-push/v1.3/apps/{appId}/logs/message | /push/v2.4/appkeys/{appkey}/logs/message |
| 予約メッセージ | 送信スケジュール作成 | POST | /tcgb-push/v1.3/apps/{appId}/schedules | /push/v2.4/appkeys/{appkey}/schedules |
|   | 作成 | POST | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | リスト照会 | GET | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
|   | 1件照会 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id} | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id} |
|   | 送信完了照会 | GET | /tcgb-push/v1.3/apps/{appId}/reservations/{reservation-id}/messages | /push/v2.4/appkeys/{appkey}/reservations/{reservation-id}/messages |
|   | 修正 | PUT | /tcgb-push/v1.3/apps/{appId}/reservations/{reservationId} | /push/v2.4/appkeys/{appkey}/reservations/{reservationId} |
|   | 削除 | DELETE | /tcgb-push/v1.3/apps/{appId}/reservations | /push/v2.4/appkeys/{appkey}/reservations |
| タグ | 作成 | POST | /tcgb-push/v1.3/apps/{appId}/tags | /push/v2.4/appkeys/{appkey}/tags |
|   | 照会 | GET | /tcgb-push/v1.3/apps/{appId}/tags | /push/v2.4/appkeys/{appkey}/tags |
|   | 修正 | PUT | /tcgb-push/v1.3/apps/{appId}/tags/{tagId} | /push/v2.4/appkeys/{appkey}/tags/{tag-id} |
|   | 削除 | DELETE | /tcgb-push/v1.3/apps/{appId}/tags/{tagId} | /push/v2.4/appkeys/{appkey}/tags/{tag-id} |
| UID | 作成 | POST | /tcgb-push/v1.3/apps/{appId}/uids/{uid}/tag-ids | /push/v2.4/appkeys/{appkey}/uids/{uid}/tag-ids |
|   | 照会 | GET | /tcgb-push/v1.3/apps/{appId}/uids/{uid}/tag-ids | /push/v2.4/appkeys/{appkey}/uids/{uid}/tag-ids |
|   | 修正 | PUT | /tcgb-push/v1.3/apps/{appId}/uids/{uid}/tag-ids | /push/v2.4/appkeys/{appkey}/uids/{uid}/tag-ids |
|   | タグ 削除 | DELETE | /tcgb-push/v1.3/apps/{appId}/uids/{uid}/tag-ids | /push/v2.4/appkeys/{appkey}/uids/{uid}/tag-ids |

<br/>

**当該APIの詳細については次のリンクを参照してください。**
Gamebase Wrapping APIとマッピングされたPush APIのスペックは、以下のガイドを参照してください。
Push Appkeyの設定を行わずに、Gamebase AppIdおよびSecretKeyを利用してGamebase Wrapping Push APIを呼び出せます。

[Push Guide](https://docs.nhncloud.com/ja/Notification/Push/ja/api-guide/)

> [参考1]
> Pushガイドに存在するuid値はgamebase userId値を使用できます。クライアントSDKでプッシュトークン登録時、ユーザー識別子はgamebase userIdに登録されています。
> 1人のユーザーが複数の端末でプッシュ受信を許可した場合、複数の端末でプッシュを受信します。

> [参考2]
> APIを介してプッシュメッセージを送信した場合、送信履歴はGamebase Consoleの**プッシュ > 送信履歴**で確認できません。
> **プッシュ > 設定 > 送信履歴保存**メニューで**Log & Crash**設定から確認できます。

<br/>

##### API呼び出し例

```
GET https://api-gamebase.nhncloudservice.com/tcgb-leaderboard/v1.3/apps/{appId}/factors/{factor}/user-count

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

## Etc

### OS Code

ユーザー端末のOSに対してGamebase内部で定義したコードです。

| Code | 説明 |
| --- | --- |
| AOS | Android |
| IOS | iOS |
| WEB | Web |
| WINDOWS | Windows |
| MACOS | macOS |
<br/>

### Store Code

アプリをインストールしたストアについてGamebase内部で定義したコードです。

| Code | 説明 |
| --- | --- |
| GG | Google Play Store |
| AS | App Store |
| ONESTORE | ONE store |
| GALAXY | Galaxy Store |
| MYCARD | Global MyCard |
| EPIC | Epic Games Store |
| STEAM | STEAM Store |
<br/>

### Identity Provider Code

ユーザー認証に使用されたIdentity ProviderについてGamebase内部で定義したコードです。

- guest
- google
- facebook
- appleid
- iosgamecenter
- payco
- twitter
- naver
- line
- hangame
- kakaogame
- weibo
<br/>

### Member Valid Code

ユーザーの現在状態についてGamebase内部で定義したコードです。

| Code | 説明 |
| --- | --- |
| Y | 正常ユーザー |
| D | 退会したユーザー |
| B | 利用停止したユーザー |
| T | 退会猶予状態のユーザー |
| P | 利用停止猶予状態のユーザー |
| M | 消失したアカウント |
<br/>


### Store Reference Status

決済システム(ストアのアプリ内決済、外部決済)が提供する決済参照状態

| 決済システム | Code | 説明 |
| --- | --- | --- |
| Googleアプリ内 | PURCHASED | 購入完了 |
| | REPURCHASED | 再購入完了 |
| | RESTARTED | 購読再開 |
| | PENDING | 決済遅延中 |
| | RENEWED | 購読更新 |
| | RECOVERED | 購読復旧 |
| | PAUSE_SCHEDULED | 購読中止予定 |
| | PAUSED | 中止 |
| | REVOKED | 払い戻し |
| | CANCELED_PRODUCT | 単品決済キャンセル |
| | CANCELED_SUBSCRIPTION | 購読キャンセル(更新中止)<br>- 現在の回の購読は提供する必要がある |
| | ON_HOLD | 保留中 |
| | IN_GRACE | 猶予中 |
| | EXPIRED | 期限切れ |
| | NOT_APPOINTED | 適切な特定状態なし |
<br/>

### Withdrawal Event Type

ユーザー退会がどこで発生したかを示すイベント発生経路です。

| Type | 説明 |
| --- | --- |
| WAA | アプリ(クライアント)リクエストによるアカウント退会 |
| WACS | コンソール/管理者リクエストによるアカウント退会 |
| WAES | 外部サーバー(ゲームサーバー)による退会<br>- サーバー退会API呼び出し |
| WAAI | Apple ID連携削除による退会 |
| WAHI | ハンゲームアカウント削除による退会 |
| WAGE | 猶予期間満了に伴うシステム自動退会 |
<br/>

### Support

API呼び出し失敗の原因に対するお問い合わせがある場合、**API呼び出しURL(HTTP bodyがある場合は、bodyと一緒に)とそれに対するレスポンス結果**を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。

<br>

##### API呼び出し例

```
GET https://api-gamebase.nhncloudservice.com/tcgb-launching/v1.3/apps/C3JmSctU/maintenances/under-maintenance
```

##### API失敗のレスポンス結果

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
