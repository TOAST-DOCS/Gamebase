## Game > Gamebase > Android SDK ご利用ガイド > 認証

## Login

Gamebaseでは基本的にゲストログインに対応しています。

* ゲスト以外のProviderでログインするためには、該当するProvider AuthAdapterが必要です。
* AuthAdapter及び3rd-Party Provider SDKの設定は、次をご参考ください。
  [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

多くのゲームがタイトル画面にログインを設計しています。
* アプリをインストールして初めて起動したとき、タイトル画面からゲームユーザーがどのIdP(identity provider)で認証を行うか選択できるようにします。
* 一度ログインした後はIdP選択画面を表示せずに、前回ログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![auth flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. 前回のログインタイプで認証

* 前回の認証記録がある場合、IDとパスワードを入力させずに認証を試みます。
* **Gamebase.loginForLastLoggedInProvider()**を呼び出します。

#### 1-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **Gamebase.getUserID()**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 1-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコード**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.loginForLastLoggedInProvider()**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **Gamebase.getBanInfo()**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由についてご案内ください。
    * Gamebaseを初期化する際に**GamebaseConfiguration.Builder.enablePopup(true)**及び**enableBanPopup(true)**を呼び出せば、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * 前回のログインタイプで認証に失敗しているため、**3. 指定されたIdPで認証**を進めてください。

#### 2. 指定されたIdPで認証

* IdPのタイプを直接指定して認証を試みます。
    * 認証可能なタイプは、**AuthProvider**クラスに宣言されています。
* **Gamebase.login(activity, idpType, callback)**APIを呼び出します。

#### 2-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **Gamebase.getUserID()**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 2-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.login(activity, idpType, callback)**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **Gamebase.getBanInfo()**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由についてご案内ください。
    * Gamebaseを初期化する際に**GamebaseConfiguration.Builder.enablePopup(true)**及び**enableBanPopup(true)**を呼び出せば、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに知らせ、ゲームユーザーが認証IdPのタイプを選択できる状態(主にタイトル画面またはログイン画面)に戻ります。

### Login as the Latest Login IdP

最後にログインしたIdPでログインを試みます。<br/>
該当するログイントークンの期限が切れていたり、トークン検証などに失敗した場合、失敗を返します。<br/>
この場合、該当するIdPに対するログインを設計する必要があります。


**API**

```java
+ (void)Gamebase.loginForLastLoggedInProvider(Activity activity, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.loginForLastLoggedInProvider(activity, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // ログイン成功
            Log.d(TAG, "Login successful");
            String userId = Gamebase.getUserID();
        } else {
            if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                    exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            Thread.sleep(2000);
                            onLoginForLastLoggedInProvider(activity);
                        } catch (InterruptedException e) {}
                    }
                }).start();
            } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                // ログインを試みたゲームユーザーが利用停止状態です。
                // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出すと、
                // Gamebaseが利用停止に関するポップアップを自動で表示します。
                //
                // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、Gamebase.getBanInfo()で
                // 利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由を表示してください。
                BanInfo banInfo = Gamebase.getBanInfo();
            } else {
                // その他のエラーが発生した場合、指定されたIdPで認証を試みます。
                Gamebase.login(activity, provider, logincallback);
            }
        }
    }
}
```
### Login with GUEST

Gamebaseは、ゲストログインに対応しています。

* デバイス固有のキーを作成し、Gamebaseに対しログインを試みます。
* ゲストログインは、アプリを削除したりデバイスを初期化した場合、アカウントが削除されることがあるためIdPを使ったログイン方式を推奨します。

ゲストログインを設計する方法については、次のコード例をご参考ください。


**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider.GUEST, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void onLoginForGuest(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GUEST, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // ログイン成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGuest(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // ログインを試みたゲームユーザーが利用停止状態です。
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出し하였다면
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、Gamebase.getBanInfo()で
                    // 利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // ログイン失敗
                    Log.e(TAG, "Login failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```


### Login with IdP

次は特定のIdPでログインできるようにするコード例です。<br/>
ログイン可能なIdPのタイプは、**AuthProvider **クラスから確認することができます。

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void onLoginForGoogle(final Activity activity) {
    Gamebase.login(activity, AuthProvider.GOOGLE, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // ログイン成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginForGoogle(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // ログインを試みたユーザーが利用停止状態です。
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出し하였다면
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、Gamebase.getBanInfo()で
                    // 利用制限情報を確認し、ユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // ログイン失敗
                    Log.e(TAG, "Login failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

### Login with Credential

IdPが提供するSDKを使ってゲームで直接認証した後、発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェースです。

* Credentialパラメーターの設定方法

| keyname                                  | a use                                    | 値の種類                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdPタイプの設定                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)の設定<br/>Google認証の場合は使用しない |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Googleログイン後に取得できるOTAC(one time authorization code)の入力 |                                          |

> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は、外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。
>

**API**

```java
+ (void)Gamebase.login(Activity activity, Map<String, Object> credentialInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void onLoginWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.login(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // ログイン成功
                Log.d(TAG, "Login successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLoginWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_BANNED_MEMBER) {
                    // ログインを試みたゲームユーザーが利用停止状態です。
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出し하였다면
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、Gamebase.getBanInfo()で
                    // 利用制限情報を確認し、ユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = Gamebase.getBanInfo();
                } else {
                    // ログイン失敗
                    Log.e(TAG, "Login failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

### Authentication Additional Information Settings

#### Google

1. Google 인증을 위해서는 Google Cloud Console에서 **Web Application Client ID**를 발급받아야 합니다.
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-001_1.11.0.png)
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-002_1.11.0.png)
2. 승인된 리디렉션 URI 란에 다음 값을 입력합니다.
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-003_1.11.0.png)

#### Facebook
* **TOAST Console > Gamebase > App > 認証情報 > 追加情報 & Callback URL**の**追加情報**項目にJSON Stringタイプの情報を設定する必要があります。
    * Facebookの場合、OAuth認証を試みるとき、Facebookにリクエストする情報の種類を設定する必要があります。

Facebook認証追加情報の入力例

```json
{ "facebook_permission"：[ "public_profile", "email"]}
```

#### PAYCO
* **TOAST Console > Gamebase > App > 認証情報 > 追加情報 & Callback URL**の**追加情報**項目にJSON Stringタイプの情報を設定する必要があります。
    * PAYCOの場合、PaycoSDKが求める**service_code**と**service_name**の設定が必要です。

PAYCO追加認証情報の入力例

```json
{ "service_code"："HANGAME", "service_name"："Your Service Name" }
```

#### NAVER
* **TOAST Console > Gamebase > App > 認証情報 > 追加情報 & Callback URL**の**追加情報**項目にJSON Stringタイプの情報を設定する必要があります。
	* NAVERの場合、ログイン同意ウィンドウに表示されるアプリ名**service_name**の設定が必要です。
		* 만일 iOS 빌드도 필요하다면 **url_scheme_ios_only** 값도 설정해야 합니다.
		* 설정 방법은 iOS 가이드를 참고하시기 바랍니다. : [iOS Developer's Guide > Authentication > NAVER](./ios-authentication/#naver)

NAVER追加認証情報の入力例

```json
{ "service_name": "Your Service Name" }
```

## Logout

ログインされたIdPからのログアウトを試みます。主にゲームの設定画面にログアウトボタンを設け、ボタンをクリックすると実行されるように設計するケースが多いです。
ログアウトに成功してもゲームユーザーのデータは保持されます。
ログアウトに成功した場合、該当するIdPで認証を行った記録が削除されるため、次回ログインする時にID・パスワードの入力ウィンドウが表示されます。<br/><br/>

次は、ログアウトボタンをクリックするとログアウトされるコード例です。

**API**

```java
+ (void)Gamebase.logout(Activity activity, GamebaseCallback callback);
```

**Example**

```java
private static void onLogout(final Activity activity) {
    Gamebase.logout(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// ログアウト成功
                Log.d(TAG, "Logout successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onLogout(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // ログアウト失敗
                    Log.e(TAG, "Logout failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

## Withdraw

次は、ログイン状態でのゲームユーザー退会を設計するコード例です。<br/><br/>

* 退会に成功した場合、ログインしたIdPに紐づいていたゲームユーザーデータは削除されます。
* 該当するIdPでもう一度ログインすることができ、新しいゲームユーザーデータを作成します。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。
* 退会に成功すると、IdPログアウトを試みます。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連携している場合、IdP連携がすべて解除され、Gamebaseのゲームユーザーデータが削除されます。
>

**API**

```java
+ (void)Gamebase.withdraw(Activity activity, GamebaseCallback callback);
```

**Example**

```java
private static void onWithdraw(final Activity activity) {
    Gamebase.withdraw(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
            	// 退会成功
                Log.d(TAG, "Withdraw successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                onWithdraw(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else {
                    // 退会失敗
                    Log.e(TAG, "Withdraw failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

## Mapping

マッピングは、既にログインされているアカウントに他のIdPアカウントを連携させたり、解除する機能です。

ほとんどのゲームにおいて、一つのゲームユーザーアカウントに複数のIdPを連携(マッピング)させることができます。<br/>GamebaseのマッピングAPIを使用すれば、既にログインされているアカウントに他のIdPアカウントを連携させたり、解除することができます。

つまり、連携中のIdPアカウントでログインを試みる場合、常に同じユーザーIDでログインされることになります。<br/><br/>

注意すべき点は、各IdPは一つのアカウントにのみ連携させることができるという点です。<br/>
例えば、Googleアカウントに連携中の場合、他のGoogleアカウントを追加で連携させることはできません。<br/>
アカウント連携の例は、次の通りです。<br/><br/>

* GamebaseユーザーID：123bcabca
    * Google ID：aa
    * Facebook ID：bb
    * Apple Game Center ID：cc
    * Payco ID：dd
* GamebaseユーザーID：456abcabc
    * Google ID：ee
    * Google ID：ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

マッピングAPIにはマッピング追加APIとマッピング解除APIがあります。

### Add Mapping Flow

マッピングは、次の手順で設計することができます。

#### 1. ログイン
マッピングは、現在のアカウントにIdPアカウントの連携を追加する機能であるため、ログインされた状態でなければなりません。
まず、ログインAPIを呼び出してログインします。

#### 2. マッピング

**Gamebase.addMapping(activity, idpType, callback)**を呼び出してマッピングを試みます。

#### 2-1. マッピングに成功した場合

* おめでとうございます！現在のアカウントと連携しているIdPアカウントが追加されました。
* マッピングに成功しても、「現在ログイン中のIdP」は変わりません。つまり、Googleアカウントでログインした後、Facebookアカウントのマッピングを試み、それが成功したからといって「現在ログイン中のIdP」がGoogleからFacebookに変更されるわけではありません。Googleのままで維持されます。
* マッピングは、単にIdP連携だけを追加する機能です。

#### 2-2. マッピングに失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.addMapping()**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 既に他のアカウントに連携している場合に発生するエラー
    * エラーコード**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**は、マッピングしようとしているIdPのアカウントが既に他のアカウントに連携しているという意味です。連携済みのアカウントを解除したい場合、該当するアカウントでログインしてから**Gamebase.withdraw()**を呼び出して退会したり、**Gamebase.removeMapping()**を呼び出して連携を解除した後、もう一度マッピングを試みてください。
* 既に同じIdPアカウントに連携されている場合に発生するエラー
    * エラーコード**AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**は、マッピングしようとしているIdPと同じ種類のアカウントが既に連携しているという意味です。
        * Gamebaseのマッピングは、IdP一つにつき一つのアカウントのみ連携させることができます。例えば、既にPAYCOアカウントに連携している場合は、これ以上PAYCOアカウントを追加することができません。
        * 同じIdPの他のアカウントを連携させるためには、**Gamebase.removeMapping()**を呼び出して連携を解除してからもう一度マッピングを試みてください。
* その他のエラー
    * マッピングに失敗しました。

### Add Mapping

特定のIdPにログインされた状態で他のIdPへのマッピングを試みます。<br/>

次は、Facebookにマッピングを試みる例です。

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, AuthProvider authProvider, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // Mappingを試みているIDPアカウントが既に他のアカウントに連携されています。
                    // 強制的に連携を解除するためには、該当するアカウントから退会したり、Mappingを解除する必要があります。
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // Mappingを試みているIDPアカウントが既に追加されています。
                    // Gamebase Mappingは、IdP一つにつき一つのアカウントのみ連携させることができます。
                    // IDPアカウントを変更したい場合、既に連携中のアカウントはMappingを解除する必要があります。
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // マッピング追加失敗
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

### Add Mapping with Credential

ゲームで直接ID Providerに提供するSDKで、予め認証を行い発行されたアクセストークンなどを利用してGamebase AddMappingをすることができるインターフェースです。

* Credentialパラメーターの設定方法

| keyname                                  | a use                                    | 値の種類                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdPタイプの設定                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)の設定<br/>Google認証の場合は使用しない  |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Googleログイン後に取得できるOTOC(one time authorization code)の入力 |                                          |

> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。
>

**API**

```java
+ (void)Gamebase.addMapping(Activity activity, Map<String, Object> credentialInfo, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingWithCredential(final Activity activity) {
    Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                addMappingWithCredential(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                    // Mappingを試みているIDPアカウントが既に他のアカウントに連携されています。
                    // 強制的に連携を解除するためには、該当するアカウントから退会したり、Mappingを解除する必要があります。
                    Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                    // Mappingを試みているIDPアカウントが既に追加されています。
                    // Gamebase Mappingは、IdP一つにつき一つのアカウントのみ連携させることができます。
                    // IDPアカウントを変更したい場合、既に連携中のアカウントはMappingを解除する必要があります。
                    Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
                } else {
                    // マッピング追加失敗
                    Log.e(TAG, "Add Mapping failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```

### Remove Mapping

特定のIDPに対する連携を解除します。現在ログインしているアカウントを解除しようとした場合は、失敗を返します。<br/>
連携を解除した後は、Gamebase内部で該当するIdPに対するログアウト処理を行います。

**API**

```java
+ (void)Gamebase.removeMapping(Activity activity, AuthProvider authProvider, null, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void removeMappingForFacebook(final Activity activity) {
    Gamebase.removeMapping(activity, AuthProvider.FACEBOOK, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング解除成功
                Log.d(TAG, "Remove mapping successful");
            } else {
                if (exception.getCode() == GamebaseError.SOCKET_ERROR ||
                        exception.getCode() == GamebaseError.SOCKET_RESPONSE_TIMEOUT) {
                    // Socket errorにより一時的にネットワークに接続できない状態であることを意味します。
                    // ネットワーク状態を確認したり、しばらくしてからもう一度試してください。
                    new Thread(new Runnable() {
                        @Override
                        public void run() {
                            try {
                                Thread.sleep(2000);
                                removeMappingForFacebook(activity);
                            } catch (InterruptedException e) {}
                        }
                    }).start();
                } else if (exception.getCode() == GamebaseError.AUTH_REMOVE_MAPPING_LOGGED_IN_IDP) {
                    // ログイン中のアカウントではMappingを解除することができません。
                    // 他のアカウントでログインしてからMappingを解除したり退会する必要があります。
                    Log.e(TAG, "Remove Mapping failed- LOGGED_IN_IDP");
                } else {
                    // マッピング解除失敗
                    Log.e(TAG, "Remove mapping failed- "
                            + "errorCode：" + exception.getCode()
                            + "errorMessage：" + exception.getMessage());
                }
            }
        }
    });
}
```


## Gamebase User`s Information
Gamebaseで認証フローを進めた後、アプリを制作する際に必要な情報を取得することができます。

> <font color="red">[注意]</font><br/>
>
> "Gamebase.loginForLastLoggedInProvider()"APIでログインした場合、認証情報を取得することができません。
>
> 認証情報が必要な場合、"Gamebase.loginForLastLoggedInProvider()"の代わりに使用したいIDPCodeと同じ{IDP_CODE}をパラメーターとし、"Gamebase.login(activity, IDP_CODE, callback)"APIでログインしないと認証情報を正常に取得することができません。

### Get Authentication Information for Gamebase
Gamebaseから発行された認証情報を取得することができます。

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
+ (BanInfo)Gamebase.getBanInfo();
```

**Example**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();

// Obtaining Ban Information
BanInfo banInfo = Gamebase.getBanInfo();
```


### Get Authentication Information for External IDP

外部の認証SDKでアクセストークン、ユーザーID、Profileなどの情報を取得することができます。

**API**

```java
+ (String)Gamebase.getAuthProviderUserID(AuthProvider authProvider);
+ (String)Gamebase.getAuthProviderAccessToken(AuthProvider authProvider);
+ (AuthProviderProfile)Gamebase.getAuthProviderProfile(AuthProvider authProvider);
```

**Example**

```java
// ユーザIDを取得します。
String userId = Gamebase.getAuthProviderUserID(AuthProvider.FACEBOOK);

// アクセストークンを取得します。
String accessToken = Gamebase.getAuthProviderAccessToken(AuthProvider.FACEBOOK);

// User Profile情報を取得します。
AuthProviderProfile profile = Gamebase.getAuthProviderProfile(AuthProvider.FACEBOOK);
Map<String, Object> profileMap = profile.information;
```

### Get Banned User Information

Gamebase Consoleで利用制限対象のゲームユーザーに登録された場合、
ログインを試みると、次のような利用制限情報コードが表示されることがあります。**Gamebase.getBanInfo()**メソッドを利用して利用制限情報を確認することができます。

* BANNED_MEMBER(7)

## TransferKey
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.
이 키를 **TransferKey** 라고 부릅니다.
발급받은 TransferKey는 다른 기기에서 **requestTransfer** API를 호출하여 계정 이전을 할 수 있습니다.

> `주의`
> TransferKey의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferKey를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferKey
게스트 계정 이전을 위한 TransferKey를 발급합니다.
TransferKey의 형식은 영문자 **"소문자/대문자/숫자"를 포함한 8자리의 문자열**입니다.
또한 발급 시간 및 만료 시간을 같이 발급하며, 형식은 epoch time입니다.
* 참고: https://www.epochconverter.com/

**API**

```java
+ (void)Gamebase.issueTransferKey(int expiredTime, GamebaseDataCallback<TransferKeyInfo> callback);
```

**Example**

```java
Gamebase.issueTransferKey(3600 * 24, new GamebaseDataCallback<TransferKeyInfo>() {
    @Override
    public void onCallback(TransferKeyInfo transferKeyData, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Issue TransferKey successful");
            Log.i(TAG, "transferKey : " + transferKeyData.getTransferKey());
            Log.i(TAG, "regDate : " + transferKeyData.getRegDate());
            Log.i(TAG, "expireDate : " + transferKeyData.getExpireDate());
            ...
        } else {
            Log.e(TAG, "Issue TransferKey failed");
            ...
        }
    }
});
```

### Transfer Guest Account to Another Device
**issueTransfer** API로 발급받은 TransferKey를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferKey를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferKey를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> `주의`
> 이미 Guest 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

**API**

```java
+ (void)Gamebase.requestTransfer(String transferKey, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.requestTransfer(transferKey, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Transfer account successful");
            ...
        } else {
            Log.e(TAG, "Transfer account failed");
            ...
        }
    }
});
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 正しくない会員に対するリクエストです。                        |
|                | BANNED\_MEMBER                           | 7          | 利用制限対象の会員です。                               |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | ログインがキャンセルされました。                           |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | この認証方式には対応しておりません。                      |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 退会されているか、存在しない会員です。                    |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部認証ライブラリーエラーです。 <br/> DetailCode 및 DetailMessage를 확인해주세요.  |
| TransferKey    | SAME\_REQUESTOR                          | 8          | 발급한 TransferKey를 동일한 기기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IDP가 연동되어 있습니다. |
|                | AUTH\_TRANSFERKEY\_EXPIRED               | 3031       | TransferKey의 유효기간이 만료됐습니다. |
|                | AUTH\_TRANSFERKEY\_CONSUMED              | 3032       | TransferKey가 이미 사용됐습니다. |
|                | AUTH\_TRANSFERKEY\_NOT\_EXIST            | 3033       | TransferKey가 유효하지 않습니다. |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       |トークンログインに失敗しました。                         |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       |トークン情報が有効ではありません。                       |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近ログインしたIdPの情報がありません。                  |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdPログインに失敗しました。                        |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | マッピング追加に失敗しました。                          |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 既に他のメンバーにマッピングされています。                     |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 既に同じIdPにマッピングされています。                    |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | IdP情報が有効でありません。(Consoleに該当するIdPの情報がありません。) |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | マッピング削除に失敗しました。                          |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 最後にマッピングされたIdPは削除することができません。              |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | 現在ログイン中のIdPです。                    |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | ログアウトに失敗しました。                           |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退会に失敗しました。                             |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | 不明なエラーです。(定義されていないエラーです)。            |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、外部認証ライブラリーで発生したエラーです。
* 外部ライブラリーエラーの詳細は次のように確認できます。

```java
Gamebase.login(activity, AuthProvider.GOOGLE, additionalInfo, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Login successful");
            ...
        } else {
            Log.e(TAG, "Login failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();

            if (errorCode == GamebaseError.AUTH_EXTERNAL_LIBRARY_ERROR) {
                // Third Party Detail Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```
<<<<<<< HEAD
=======

* IdP SDKのエラーコードは各IdPのDeveloperページをお参照ください。
>>>>>>> beta
