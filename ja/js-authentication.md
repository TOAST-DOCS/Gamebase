## Game > Gamebase > JavaScript SDK使用ガイド > 認証

## Login
Gamebaseでは、ゲストログインをデフォルトでサポートします。
他のIdP(identity provider、例えばGoogle、Facebook、LINE、NAVER、Twitter)を使用するには、"Login with IdP"を参照してください。


### Login Flow
多くのゲームがタイトル画面にログインを設計しています。

* アプリをインストールして初めて起動したとき、タイトル画面からゲームユーザーがどのIdP(identity provider)で認証を行うか選択できるようにします。
* 一度ログインした後はIdP選択画面を表示せずに、前回ログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 指定されたIdPで認証

* IdPタイプを直接指定して認証を試行します。
    * 認証可能なタイプは下記の通りです。
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * PAYCO('payco')
        * NAVER('naver')
        * Line('line')
* **toast.Gamebase.login(providerName, (authToken, error) => { ... })**APIを呼び出します。

#### 1-1. 認証に成功した場合

* おめでとうございます。認証に成功しました。
* **toast.Gamebase.getUserID()**でユーザーIDを取得し、ゲームロジックを実装してください。

#### 1-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または **SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証に失敗したということなので、**toast.Gamebase.login(providerName, callback)**を再度呼び出すか、しばらくしてから再度試行します。
* 利用停止ゲームユーザー
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **toast.Gamebase.getBanInfo()**で制裁情報を確認し、ゲームユーザーにゲームをプレイできない理由を知らせてください。
    * Gamebaseの初期化時、**enablePopup**をtrueに設定すると、Gamebaseが利用停止に関するポップアップを自動的に表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに伝え、ゲームユーザーが認証IdPタイプを選択できる状態(主にタイトル画面またはログイン画面)に戻ります。

### Login with IdP

次は特定IdPでログインできるようにするコード例です。

ログインできるIdPタイプは下記の通りです。

* Guest('guest')
* Google('google')
* Facebook('facebook')
* Payco('payco')
* Naver('naver')
* Line('line')

> <font color="red">[注意]</font><br/>
>
> Gamebaseでは、認証のために'ポップアップ'を利用します。'ポップアップ'が許可されていない場合は、'ログイン試行'中の状態で継続して待機することがあります。
> 事前に'認証前にポップアップを許可してください'という文言を表示し、ユーザーが煩わしさを感じないようにする必要があります。
>
> Facebookの接続可能ブラウザポリシー変更に伴い
> IEブラウザでFacebookにログイン時、Edgeブラウザに強制的に切り替わります。
> 従って、Facebookでログインする場合、Chrome、Edgeブラウザを利用して接続する必要があります。
> ただし、やむを得ずIEブラウザからFacebookログインをしなければならない場合は、
> 以下のような方法でEdgeブラウザに強制的に切り替わることを防いで利用することはできるので参考にしてください。
> 1. Edgeブラウザの設定に接続します。
> 2. 設定画面メニューから「規定のブラウザ」を選択します。
> 3. Internet Explorer互換性モードを「オフ」に変更します。

```js
toast.Gamebase.login(providerName, (authToken, error) => { ... });
```

**Example**
```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Socketエラーです。一時的なネットワーク接続不可状態を意味します。
                // ネットワーク状態を確認するか、しばらくしてから再度お試しください。
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // ログインを試行したユーザーが利用停止状態です。
                // GamebaseConfiguration.uiConfiguration.enablePopup(true)および
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true)で初期化した場合、
                // Gamebaseが利用停止に関するUIを自動的に表示します。

                // Game UIに合わせて直接利用停止ポップアップを実装するには、toast.Gamebase.getBanInfo()で
                // 制裁情報を確認し、ユーザーにゲームをプレイできない理由を表示してください。
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // ログイン失敗
            }

            return;
        }

        // ログイン成功
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

### Login with Credential

IdPで提供するSDK、REST APIなどを使用し、ゲームで直接認証して発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェイスです。

**Credentialパラメータ設定方法**

| key name             | a use                                                               |
| -------------------- | ------------------------------------------------------------------- |
| providerName         | IdPタイプ設定                                                  |
| accessToken          | IdPログイン後に取得した認証情報(アクセストークン)設定                    |
| accessTokenSecret    | IdPログイン後に取得した認証情報(アクセストークンシークレット)設定              |

> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用する際に、必要な場合があります。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDKでサポートを要求する開発事項は外部SDKのAPIを使用して実装する必要があり、Gamebaseではサポートしません。
>

```js
var credential = {
    providerName: ${IdP},
    acessToken: ${IdP AccessToken},
    accessTokenSecret: ${IdP AccessTokenSecret}, // This is only nessassary in the case that IdP give you this value.
};

toast.Gamebase.loginWithCredential(credential, callback);
```

**Example**

```js
function loginWithFacebookCredential() {
    var credential = {
        providerName: 'facebook',
        accessToken: 'EAAL24qLuXHwBAFL1hS72BzZCUrJyDMRxUq1H5ZBGFIT..........',
    };

    toast.Gamebase.loginWithCredential(credential, function (authToken, error) {
        if (error) {
               if (error.code == toast.GamebaseConstant.SOCKET_ERROR ||
                error.code == toast.GamebaseConstant.SOCKET_RESPONSE_TIMEOUT) {
                // Socketエラーです。一時的なネットワーク接続不可状態を意味します。
                // ネットワーク状態を確認するか、しばらくしてから再度お試しください。
            } else if (error.code == toast.GamebaseConstant.BANNED_MEMBER) {
                // ログインを試行したユーザーが利用停止状態です。
                // GamebaseConfiguration.uiConfiguration.enablePopup(true)および
                // GamebaseConfiguration.uiConfiguration.enableBanPopup(true)で初期化した場合、
                // Gamebaseが利用停止に関するUIを自動的に表示します。

                // Game UIに合わせて直接利用停止ポップアップを実装するには、Gamebase.getBanInfo()で
                // 制裁情報を確認し、ユーザーにゲームをプレイできない理由を表示してください。
                var banInfo = toast.Gamebase.getBanInfo();
            } else {
                // ログイン失敗
            }

            return;
        }

        // ログイン成功
        console.log('login success');
        var userId = authToken.member.userId; // Gamebase UserId
        var accessToken = authToken.token.accessToken; // Gamebase AccessToken
    });
}
```

## Logout

ログインしたIdPからログアウトを試行します。主にゲームの設定画面にログアウトボタンを置き、ボタンをクリックしたら実行されるように実装する場合が多いです。
ログアウトが成功しても、ゲームユーザーデータは維持されます。
ログアウトに成功すると、該当IdPで認証した記録を削除するため、次回ログイン時にID、パスワード入力ウィンドウが表示されます。<br/><br/>

```js
toast.Gamebase.logout((error) => { ... })
```

**Example**
```js
function logout() {
    toast.Gamebase.logout(function (error) {
        if (error) {
            // ログアウト失敗
            console.log(error);
            return;
        }

        // ログアウト成功
        console.log(data);
    });
}
```

## Withdraw

次は、ログイン状態でゲームユーザー退会を実装するコード例です。<br/><br/>

* 退会に成功すると、ログインしたIdPと連携していたゲームユーザーデータは削除されます。
* 該当IdPで再度ログインでき、新規のゲームユーザーデータを作成します。
* Gamebaseの退会を意味し、IdPアカウントの退会を意味しません。
* 退会に成功すると、IdPログアウトを試行します。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連携中の場合、すべてのIdP連携が解除され、Gamebaseゲームユーザーデータが削除されます。
>

```js
toast.Gamebase.withdraw(callback)
```

**Example**
```js
function withdraw() {
    toast.Gamebase.withdraw(function (data, error) {
        if (error) {
            // 退会失敗
            console.log(error);
            return;
        }

        // 退会成功
        console.log(data);
    });
}
```

## Gamebase User's Information
Gamebaseで認証手続きを行った後、アプリを製作する時に必要な情報を取得できます。

### Get Authentication Information for Gamebase
Gamebaseで発行した認証情報をインポートできます。

```js
// Obtaining Gamebase UserID
var userId =  toast.Gamebase.getUserID();

// Obtaining Gamebase AccessToken
var accessToken = toast.Gamebase.getAccessToken();
```

### Get Banned User Information

Gamebaseコンソールに、制裁されたゲームユーザーとして登録された場合、
ログインを試行すると、下記のような利用制限情報コードが表示されます。**toast.Gamebase.getBanInfo()**メソッドを利用して制裁情報を確認できます。

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)

## TemporaryWithdrawal

「退会猶予」機能です。
一時退会をリクエストして即時に退会が行われずに一定期間の猶予期間が過ぎると、退会が行われます。
猶予期間はコンソールで変更できます。

> `注意`
>
> 退会猶予機能を使用する場合には**Gamebase.withdraw()**APIを使用しないでください。
> **Gamebase.withdraw()**APIは即時にアカウントを退会します。

ログインが成功すると、AuthToken.member.temporaryWithdrawalで退会猶予状態のユーザーかを判断できます。

### Request TemporaryWithdrawal

一時退会をリクエストします。
コンソールに指定した期間が過ぎると自動的に退会進行が完了します。

**API**

```js
toast.Gamebase.TemporaryWithdrawal.requestWithdrawal(callback)
```

**Example**

```js
function requestWithdrawal() {
    gamebase.TemporaryWithdrawal.requestWithdrawal(function (data, error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                // Already requested temporary withdrawal before.
            } else {
                // Request temporary withdrawal failed.
            }
            return;
        }
        
        // Request temporary withdrawal success.
    });
}
```

### Check TemporaryWithdrawal User

退会猶予を使用するゲームは、AuthToken.member.temporaryWithdrawalがnullではない場合、ログイン後に常に該当ユーザーに退会進行中であることを伝える必要があります。

**Example**

```js
function gamebaseLogin() {
    toast.Gamebase.login('google', function (authToken, error) {
        if (error) {
            // Login failed
            return;
        }

        if(authToken.member.temporaryWithdrawal != null) {    
            // User is under temporary withdrawal    
            var gracePeriodDate = authToken.member.temporaryWithdrawal.gracePeriodDate;            
        } else {
            // Login success.
        }
    });
}
```

### Cancel TemporaryWithdrawal

退会リクエストをキャンセルします。
退会リクエストした後、期間が満了して退会が完了すると、キャンセルができません。

**API**

```js
toast.Gamebase.TemporaryWithdrawal.cancelWithdrawal(callback)
```
**Example**

```js
function cancelWithdrawal() {
    gamebase.TemporaryWithdrawal.cancelWithdrawal(function (error) {
        if (error) {
            if (error.code == GamebaseConstant.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                // Never requested temporary withdrawal before.
            } else {
                // Cancel temporary withdrawal failed.
            }
            return;
        }
        
        // Cancel temporary withdrawal success.
    });
}
```

### Withdraw Immediately

退会猶予期間を無視して、即時退会を進行します。
実際の内部動作はGamebase.withdraw() APIと同じです。

即時退会はキャンセルできないため、実行するかどうかをユーザーによく確認してください。

**API**

```js
toast.Gamebase.TemporaryWithdrawal.withdrawImmediately(callback)
```

**Example**

```js
function withdrawImmediately() {
    gamebase.TemporaryWithdrawal.withdrawImmediately(function (error) {
        if (error) {
            // Withdraw failed.
            return;
        }
        
        // Withdraw success.
    });
}
```

## Error Handling

| Category       | Error                                                   | Error Code | Description                                                       |
| -------------- | ------------------------------------------------------- | ---------- | ----------------------------------------------------------------- |
| Auth           | INVALID\_MEMBER                                         | 6          | 無効な会員へのリクエストです。                                            |
|                | BANNED\_MEMBER                                          | 7          | 制裁中の会員です。                                                     |
|                | AUTH\_USER\_CANCELED                                    | 3001       | ログインがキャンセルされました。                                                |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | サポートしていない認証方式です。                                           | 
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | 存在しないか、退会した会員です。                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | 以前の認証プロセスが完了していません。                                   |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | トークンのログインに失敗しました。                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | トークン情報が有効ではありません。                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | 最近ログインしたIdP情報がありません。                                      |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdPログインに失敗しました。                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | IdP情報が有効ではありません。 (Consoleに該当IdP情報がありません。)           |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       | ログアウトに失敗しました。                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | 退会に失敗しました。                                                   |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | すでに一時退会中のユーザーです。                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 一時退会中のユーザーではありません。                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。                        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | 不明なエラーです。(定義されていないエラーです)。                             |

* エラーコードは次の文書を参照してください。
    * [Entire Error Codes](./error-code/#client-sdk)