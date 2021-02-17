## Game > Gamebase > JavaScript SDK 사용 가이드 > 인증

## Login
Gamebaseでは、ゲストログインをデフォルトでサポートします。
他のIdP(identity provider、例えばGoogle、Facebook、LINE、NAVER、Twitter)を使用するには、"Login with IdP"を参照してください。


### Login Flow
多くのゲームがタイトル画面にログインを実装します。
* ゲーム初期タイトル画面で、どのIdPで認証するかを選択できるようにします。

上で説明したロジックは、次のような順序で実装できます。
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)

#### 1. 指定されたIdPで認証

* IdPタイプを直接指定して認証を試行します。
    * 認証可能なタイプは下記の通りです。
        * Guest('guest')
        * Google('google')
        * Facebook('facebook')
        * PAYCO('payco')
        * NAVER('naver')
        * Twitter('twitter')
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
NHN Cloud Gamebaseコンソールに、制裁されたゲームユーザーとして登録された場合、
ログインを試行すると、下記のような利用制限情報コードが表示されます。**toast.Gamebase.getBanInfo()**メソッドを利用して制裁情報を確認できます。

```js
// Obtaining Ban Information
var banInfo = toast.Gamebase.getBanInfo();
```
* BANNED_MEMBER (7)


## Error Handling

| Category       | Error                                                   | Error Code | Description                                                       |
| -------------- | ------------------------------------------------------- | ---------- | ----------------------------------------------------------------- |
| Auth           | INVALID\_MEMBER                                         | 6          | 無効な会員のリクエストです。                                            |
|                | BANNED\_MEMBER                                          | 7          | 制裁された会員です。                                                     |
|                | AUTH\_USER\_CANCELED                                    | 3001       | ログインがキャンセルされました。                                                |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER                          | 3002       | サポートしない認証方式です。                                           |
|                | AUTH\_NOT\_EXIST\_MEMBER                                | 3003       | 存在しないか、退会した会員です。                                        |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR                          | 3010       | 移行認証プロセスが完了していません。                                   |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED                              | 3101       | トークンログインに失敗しました。                                              |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO                | 3102       | トークン情報が有効ではありません。                                            |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP      | 3103       | 最近ログインしたIdP情報がありません。                                     |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                                | 3201       | IdPログインに失敗しました。                                              |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO                    | 3202       | IdP情報が有効ではありません。(コンソールに該当IdP情報がありません。)           |
| Logout         | AUTH\_LOGOUT\_FAILED                                    | 3501       | ログアウトに失敗しました。                                                |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                                  | 3601       | 退会に失敗しました。                                                   |
| Not Playable   | AUTH\_NOT\_PLAYABLE                                     | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。                        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                                    | 3999       | 不明なエラーです。(定義されていないエラーです)。                             |

* エラーコードの一覧は、次の文書を参照してください。
    * [Entire Error Codes](./error-code/#client-sdk)
