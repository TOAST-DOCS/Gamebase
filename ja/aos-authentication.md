## Game > Gamebase > Android SDK ご利用ガイド > 認証

## Login

Gamebaseでは基本的にゲストログインに対応しています。

* ゲスト以外のProviderでログインするためには、該当するProvider AuthAdapterが必要です。
* AuthAdapter及び3rd-Party Provider SDKの設定は、次をご参考ください。
    * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Gradle](./aos-started/#gradle)
    * [Game > Gamebase > Android SDK使用ガイド > 始める > Setting > Console > 3rd-Party Provider SDK Guide](./aos-started/#console)

### Login Flow

多くのゲームがタイトル画面にログインを設計しています。

* アプリをインストールして初めて起動したとき、タイトル画面からゲームユーザーがどのIdP(identity provider)で認証を行うか選択できるようにします。
* 一度ログインした後はIdP選択画面を表示せずに、前回ログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

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
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **BanInfo.from(exception)**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由についてご案内ください。
    * Gamebaseを初期化する際に**GamebaseConfiguration.Builder.enablePopup(true)**及び**enableBanPopup(true)**を呼び出せば、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * 前回のログインタイプで認証に失敗しているため、**'2. 指定されたIdPで認証'**を進めてください。

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
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **BanInfo.from(exception)**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由についてご案内ください。
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
            } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                // ログインを試みたゲームユーザーが利用停止状態です。
                // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出すと、
                // Gamebaseが利用停止に関するポップアップを自動で表示します。
                //
                // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、BanInfo.from(exception)で
                // 利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由を表示してください。
                BanInfo banInfo = BanInfo.from(exception);
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
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // ログインを試みたゲームユーザーが利用停止状態です。
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出した場合
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、BanInfo.from(exception)で
                    // 利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = BanInfo.from(exception);
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
ログイン可能なIdPのタイプは、**AuthProvider**クラスから確認することができます。

> <font color="red">[注意]</font><br/>
>
> PAYCO IdPは、iOSで認証モジュールであるにもかかわらず、外部決済と誤認されてリジェクトされるケースが発生して
> AuthProvider.PAYCOの定数を提供しなくなったため、
> "payco"という文字列を直接パラメータとして渡す必要があります。

**API**

```java
+ (void)Gamebase.login(Activity activity, AuthProvider provider, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.login(Activity activity, AuthProvider provider, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
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
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出した場合
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、BanInfo.from(exception)で
                    // 利用制限情報を確認し、ユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = BanInfo.from(exception);
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
| AuthProviderCredentialConstants.PROVIDER_NAME | IdPタイプの設定                              | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>AuthProvider.HANGAME<br>AuthProvider.APPLEID<br>AuthProvider.WEIBO<br>AuthProvider.KAKAOGAME<br>"payco" |
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
                } else if (exception.getCode() == GamebaseError.BANNED_MEMBER) {
                    // ログインを試みたゲームユーザーが利用停止状態です。
                    // GamebaseConfiguration.Builder.enablePopup(true).enableBanPopup(true)を呼び出した場合
                    // Gamebaseが利用停止に関するポップアップを自動で表示します。
                    //
                    // Game UIに合わせて直接利用停止案内のポップアップを設計したい場合、BanInfo.from(exception)で
                    // 利用制限情報を確認し、ユーザーに対しゲームプレイができない理由を表示してください。
                    BanInfo banInfo = BanInfo.from(exception);
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

[Console Guide](./oper-app/#authentication-information)

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

ログイン状態で退会を試行します。

* 退会成功時
  * ログインしていたIdPのゲーム利用者データは削除されます。
  * 該当IdPで再度ログインできます。その場合は新しいゲーム利用者データを作成します。
  * 連携中のすべてのIdPがログアウト処理されます。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連携している場合、IdP連携がすべて解除され、Gamebaseのユーザーデータが削除されます。
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
    * AppleID ID: cc
    * Twitter ID: dd
* GamebaseユーザーID：456abcabc
    * Google ID：ee
    * Google ID：ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

マッピングAPIにはマッピング追加APIとマッピング解除APIがあります。

> <font color="red">[注意]</font><br/>
>
> Guestログイン中にMappingに成功するとGuest IdPは消滅します。
>

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
+ (void)Gamebase.addMapping(Activity activity, String providerName, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMapping(Activity activity, String providerName, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // マッピング追加失敗
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
                // Mappingを試行しているIDPアカウントが、既に他のアカウントに連携されています。
                // 強制的に連携を解除するには該当アカウントを退会するか、マッピング(mapping)を解除します。または次のように
                // ForcingMappingTicketを取得した後、addMappingForcibly()メソッドを利用して強制マッピングを試行します。
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, mappingProvider, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 詳細はaddMappingForcibly API文書を参照してください。
                    }
                }
            } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                // Mappingを試みているIDPアカウントが既に追加されています。
                // Gamebase Mappingは、IdP一つにつき一つのアカウントのみ連携させることができます。
                // IDPアカウントを変更したい場合、既に連携中のアカウントはMappingを解除する必要があります。
                Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
            } else {
                // マッピング追加失敗
                Log.e(TAG, "Add Mapping failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
            }
        }
    });
}
```

### Add Mapping with Credential

ゲームで直接IdPのSDKで取得したアクセストークンを用いてGamebase AddMappingをするインターフェースです。

* Credentialパラメーターの設定方法

| keyname                                  | a use                                    | 値の種類                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdPタイプの設定                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>"payco" |
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
                return;
            }

            // マッピング追加失敗
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
                // Mappingを試行しているIDPアカウントが、既に他のアカウントに連携されています。
                // 強制的に連携を解除するには該当アカウントを退会するか、マッピング(mapping)を解除します。または次のように
                // ForcingMappingTicketを取得した後、addMappingForcibly()メソッドを利用して強制マッピングを試行します。
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 詳細はaddMappingForcibly API文書を参照してください。
                    }
                }
            } else if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP) {
                // Mappingを試みているIDPアカウントが既に追加されています。
                // Gamebase Mappingは、IdP一つにつき一つのアカウントのみ連携させることができます。
                // IDPアカウントを変更したい場合、既に連携中のアカウントはMappingを解除する必要があります。
                Log.e(TAG, "Add Mapping failed- ALREADY_HAS_SAME_IDP");
            } else {
                // マッピング追加失敗
                Log.e(TAG, "Add Mapping failed- "
                        + "errorCode: " + exception.getCode()
                        + "errorMessage: " + exception.getMessage());
            }
        }
    });
}
```

### Add Mapping Forcibly
特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

次はFacebookに強制マッピングを試行する例です。

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    String mappingProvider = AuthProvider.FACEBOOK;
    Gamebase.addMapping(activity, mappingProvider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // まずaddMapping APIを呼び出し、すでに連携されているアカウントでマッピングを試行し、次のようにForcingMappingTicketを取得できます。
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicketクラスのfrom()メソッドを利用してForcingMappingTicketインスタンスを取得します。
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 強制マッピングを試行します。
                Gamebase.addMappingForcibly(activity, mappingProvider, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 強制マッピング追加成功
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }

                        // 強制マッピング追加失敗
                        // エラーコードを確認し、エラーを解決します。
                    }
                }
            } else {
                ...
            }
        }
    });
}
```


### Add Mapping Forcibly with Credential
特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

ゲームで直接IdPが提供するSDKにより先に認証し、発行されたアクセストークンなどを利用して、Gamebase AddMappingForciblyを呼び出すことができるインターフェイスです。

* Credentialパラメータ設定方法

| keyname                                  | a use                                    | 値種類                                 |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdPタイプ設定                            | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.NAVER<br>AuthProvider.TWITTER<br>AuthProvider.LINE<br>"payco" |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)設定。<br/>Google認証時には使用しない。 |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Googleログイン後に取得できるOTOC(one time authorization code)入力 |                                          |

> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用する際に必要な場合があります。
>

<br/>

> <font color="red">[注意]</font><br/>
>
> 外部SDKでサポートを要求する開発事項は、外部SDKのAPIを使用して実装する必要があり、Gamebaseではサポートしません。
>

次はFacebookに強制マッピングを試行する例です。

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, Map<String, Object> credentialInfo, String forcingMappingKey, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    final Map<String, Object> credentialInfo = new HashMap<>();
    credentialInfo.put(AuthProviderCredentialConstants.PROVIDER_NAME, AuthProvider.FACEBOOK);
    credentialInfo.put(AuthProviderCredentialConstants.ACCESS_TOKEN, facebookAccessToken);

    Gamebase.addMapping(activity, credentialInfo, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // まずaddMapping APIを呼び出し、すでに連携されているアカウントでマッピングを試行し、次のようにForcingMappingTicketを取得できます。
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicketクラスのfrom()メソッドを利用して、ForcingMappingTicketインスタンスを取得します。
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 強制マッピングを試行します。
                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 強制マッピング追加成功
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }

                        // 強制マッピング追加失敗
                        // エラーコードを確認し、エラーを解決します。
                    }
                }
            } else {
                ...
            }
        }
    });
}
```



### Remove Mapping

特定のIdPに対する連携を解除します。現在ログインしているアカウントを解除しようとした場合は、失敗を返します。<br/>
連携を解除した後は、Gamebase内部で該当するIdPに対するログアウト処理を行います。

**API**

```java
+ (void)Gamebase.removeMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
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

### Get Authentication Information for Gamebase
Gamebaseから発行された認証情報を取得することができます。

**API**

```java
+ (String)Gamebase.getUserID();
+ (String)Gamebase.getAccessToken();
+ (String)Gamebase.getLastLoggedInProvider();
```

**Example**

```java

// Obtaining Gamebase UserID
String userId = Gamebase.getUserID();

// Obtaining Gamebase AccessToken
String accessToken = Gamebase.getAccessToken();

// Obtaining Last Logged In Provider
String lastLoggedInProvider = Gamebase.getLastLoggedInProvider();
```

### Get Authentication Information for External IdP

* 外部認証IdPのアクセストークン、ユーザーID、Profileなどの情報はログイン後、ゲームサーバーでGamebase Server APIを呼び出して取得できます。
    * [Game > Gamebase > APIガイド > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[注意]</font><br/>
>
> * 外部IdPの認証情報はセキュリティのためにゲームサーバーで呼び出すことを推奨します。
> * IdPによってはアクセストークンの有効期限が短い場合があります。
>     * 例えばGoogleは、ログインしてから2時間後にはアクセストークンの有効期限が切れます。
>     * ユーザー情報が必要な場合は、ログイン後すぐにGamebase Server APIを呼び出してください。
> * "Gamebase.loginForLastLoggedInProvider()" APIでログインした場合には、認証情報を取得できません。
>     * ユーザー情報が必要な場合は、"Gamebase.loginForLastLoggedInProvider()"の代わりに、使用したいIDPCodeと同じ{IDP_CODE}をパラメータにして"Gamebase.login(activity, IDP_CODE, callback)"APIでログインする必要があります。

### Get Banned User Information

Gamebase Consoleで利用制限対象のゲームユーザーに登録された場合、
ログインを試みると、次のような利用制限情報コードが表示されることがあります。**BanInfo.from(exception)**メソッドを利用して利用制限情報を確認することができます。

* BANNED_MEMBER(7)

## TransferAccount
ゲストアカウントを他の端末に移行するためのキーを発行する機能です。

このキーを**TransferAccountInfo**と呼びます。
発行されたTransferAccountInfoは、他の端末で**requestTransferAccount**APIを呼び出してアカウントを移行できます。

> <font color="red">[注意]</font><br/>
> TransferAccountInfoは、ゲストログイン状態でのみ発行できます。
> TransferAccountInfoを利用したアカウント移行は、ゲストログイン状態またはログインしていない状態でのみ可能です。
> ログインしたゲストアカウントがすでに他の外部IdP (Google、Facebook、PAYCOなど)アカウントとマッピングされている場合は、アカウント移行がサポートされません。

### Issue TransferAccount
ゲストアカウントを移行するためにTransferAccountInfoを発行します。

**API**

```java
+ (void)Gamebase.issueTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
Gamebase.issueTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Issuing TransferAccount failed.
            return;
        }

        // Issuing TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```

### Query TransferAccount
ゲストアカウントを移行するために、すでに発行されているTransferAccountInfo情報をGamebaseサーバーに問い合わせます。

**API**

```java
+ (void)Gamebase.queryTransferAccount(final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
Gamebase.queryTransferAccount(new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccount, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Querying TransferAccount failed.
            return;
        }

        // Querying TransferAccount success.
        final String account = transferAccount.account.id;
        final String password = transferAccount.account.password;
    }
});
```


### Renew TransferAccount
すでに発行されたTransferAccountInfo情報を更新します。
更新方法には**自動更新**と**手動更新**があり、**パスワードのみ更新**、**IDとパスワードを更新**を選択してTransferAccountInfo情報を更新できます。

```java
+ (void)Gamebase.renewTransferAccount(final TransferAccountRenewConfiguration config, final GamebaseDataCallback<TransferAccountInfo> callback);
```

**Example**

```java
// If you want renew the account automatically, use this config.
final RenewalTargetType renewalTargetType = RenewalTargetType.ID_PASSWORD; // RenewalTargetType.PASSWORD
final TransferAccountRenewConfiguration autoConfig = TransferAccountRenewConfiguration.newAutoRenewConfiguration(renewalTargetType);

// If you want renew the account manually, use this config.
final TransferAccountRenewConfiguration manualConfig = TransferAccountRenewConfiguration.newManualRenewConfiguration("id", "password");
Gamebase.renewTransferAccount(autoConfig, new GamebaseDataCallback<TransferAccountInfo>() {
    @Override
    public void onCallback(final TransferAccountInfo transferAccountInfo, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Renewing TransferAccount failed.
            return;
        }

        // Renewing TransferAccount success.
        final String renewedAccount = transferAccount.account.id;
        final String renewedPassword = transferAccount.account.password;
    }
});
```

### Transfer Guest Account to Another Device
**issueTransfer**APIで発行したTransferAccountでアカウントを移行する機能です。
アカウントの移行に成功した時、TransferAccountを発行した端末で移行完了メッセージが表示される場合があり、ゲストログインすると新規のアカウントが作成されます。
アカウント移行が成功した端末では、TransferAccountを発行した端末のゲストアカウントを継続して使用できます。

> <font color="red">[Caution]</font><br/>
>
> * すでにゲストログインしている状態で移行すると、端末にログインしていたゲストアカウントは消滅します。
> * 無効なIDやパスワードを連続して入力すると、**AUTH_TRANSFERACCOUNT_BLOCK(3042)**エラーが発生し、アカウント移行が一定時間できなくなります。
> この場合は、下記の例のようにTransferAccountFailInfoの値でいつまでアカウント移行ができないかをユーザーに伝えることができます。

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken authToken, GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            TransferAccountFailInfo failInfo = TransferAccountFailInfo.from(exception);
            if (failInfo != null) {
                // Transfering Account failed by entering the wrong id / pw multiple times.
                // You can tell when the account transfer is blocked by the TransferAccountFailInfo.
                String failedId = failInfo.id;
                int failCount = failInfo.failCount;
                Date blockedDate = new Date(failInfo.blockEndDate);
                return;
            }

            // Transfering Account failed by another reason.
            return;
        }

        // Transfering Account success.
        // TODO: implements post login process
    }
});
```

## TemporaryWithdrawal

退会猶予機能です。
一時退会をリクエストして即時に退会が行われず、一定期間が過ぎると退会されます。
猶予期間はコンソールで変更できます。

> <font color="red">[注意]</font><br/>
>
> 退会猶予機能を使用する場合は**Gamebase.withdraw()**APIを使用しないでください。
> **Gamebase.withdraw()**APIを使用するとアカウントを即時に退会させます。

ログインに成功するとAuthToken.getTemporaryWithdrawalInfo() APIを呼び出して退会猶予状態のユーザーなのかを判断できます。

### Request TemporaryWithdrawal

一時退会をリクエストします。
コンソールで設定した期間が過ぎると、自動的に退会が行われます。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.requestWithdrawal(@NonNull Activity activity,
                                                       @Nullable GamebaseDataCallback<TemporaryWithdrawalInfo> callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW(3602) | すでに一時退会中のユーザーです。 |

**Example**

```java
public static void testRequestWithdraw() {
    Gamebase.TemporaryWithdrawal.requestWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(TemporaryWithdrawalInfo data GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_ALREADY_TEMPORARY_WITHDRAW) {
                    // Already requested temporary withdrawal before.
                } else {
                    // Request temporary withdrawal failed.
                    return;
                }
            }

            // Request temporary withdrawal success.
        }
    });
}
```

### Check TemporaryWithdrawal User

退会猶予を使用するゲームはログイン後、常にAuthToken.getTemporaryWithdrawalInfo() APIを呼び出し、結果がnullではない有効なTemporaryWithdrawalInfoオブジェクトがリターンされた場合、該当ユーザーに退会進行中ということを伝える必要があります。

**Example**

```java
public static void testLogin() {
    Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken data, GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Login failed
                return;
            }

            // Check if user is requesting withdrawal
            if (data.getTemporaryWithdrawalInfo() != null) {
                // User is under temporary withdrawal
                long gracePeriodDate = data.getTemporaryWithdrawalInfo().getGracePeriodDate();
            } else {
                // Login success.
            }
        }
    });
}
```

### Cancel TemporaryWithdrawal

退会リクエストをキャンセルします。
退会リクエスト後、猶予期間が過ぎて退会が完了した場合、キャンセルできません。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.cancelWithdrawal(@NonNull Activity activity,
                                                      @Nullable GamebaseCallback callback);
```

**ErrorCode**

|Error Code | Description |
| --- | --- |
| AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW(3603) | 一時退会中のユーザーではありません。 |

**Example**

```java
public static void testCancelWithdraw() {
    Gamebase.TemporaryWithdrawal.cancelWithdrawal(new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                if (exception.getCode() == GamebaseError.AUTH_WITHDRAW_NOT_TEMPORARY_WITHDRAW) {
                    // Never requested temporary withdrawal before.
                } else {
                    // Cancel temporary withdrawal failed.
                    return;
                }
            }

            // Cancel temporary withdrawal success.
        }
    });
}
```

### Withdraw Immediately

退会猶予期間を無視して即時退会を進行します。
実際の内部動作はGamebase.withdraw() APIと同じです。

即時退会はキャンセルができないため、ユーザーに実行するかどうかをよく確認してください。

**API**

```java
+ (void)Gamebase.TemporaryWithdrawal.withdrawImmediately(@NonNull Activity activity,
                                                         @Nullable GamebaseCallback callback);
```

**Example**

```java
public static void testWithdrawImmediately() {
    Gamebase.TemporaryWithdrawal.withdrawImmediately(activity, new GamebaseCallback() {
        @Override
        public void onCallback(final GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Withdraw failed.
                return;
            }

            // Withdraw success.
        }
    });
}
```

## GraceBan
* 「決済アビューズ自動解除」機能です。
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にAuthToken.getGraceBanInfo() APIを呼び出して結果がnullではない有効なGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。
**Example**
```java
public static void testLogin() {
    Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken token, GamebaseException exception) {
            if (!Gamebase.isSuccess(exception)) {
                // Login failed
                return;
            }
            // Check if user is under grace ban
            GraceBanInfo graceBanInfo = token.getGraceBanInfo();
            if (graceBanInfo != null) {
                String periodDate = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss", Locale.getDefault())
                        .format(new Date(graceBanInfo.getGracePeriodDate()));
                String message = URLDecoder.decode(graceBanInfo.getMessage(), "utf-8");
                GraceBanInfo.ReleaseRuleCondition releaseRuleCondition =
                            graceBanInfo.releaseRuleCondition();
                GraceBanInfo.PaymentStatus paymentStatus = graceBanInfo.getPaymentStatus();
                if (releaseRuleCondition != null) {
                    // condition type : "AND", "OR"
                    String releaseRule = releaseRuleCondition.getAmount() +
                            releaseRuleCondition.getCurrency() +
                            " " + releaseRuleCondition.getConditionType() + " " +
                            releaseRuleCondition.getCount() + "time(s)";
                }
                if (paymentStatus != null) {
                    String paidAmount = paymentStatus.getAmount() + paymentStatus.getCurrency();
                    String paidCount = paymentStatus.getCount() + "time(s)";
                }
                // Guide the user through the UI how to finish the grace ban status.
            } else {
                // Login success.
            }
        }
    });
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 正しくない会員に対するリクエストです。                        |
|                | BANNED\_MEMBER                           | 7          | 利用制限対象の会員です。                               |
| Auth           | AUTH\_USER\_CANCELED                     | 3001       | ログインがキャンセルされました。                           |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | この認証方式には対応しておりません。                      |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 退会されているか、存在しない会員です。                    |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 外部認証ライブラリの初期化に失敗しました。 |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部認証ライブラリーエラーです。<br/> DetailCodeおよびDetailMessageを確認してください。  |
|                | AUTH_ALREADY_IN_PROGRESS_ERROR           | 3010       | 移行認証プロセスが完了していません。 |
| TransferAccount| SAME\_REQUESTOR                          | 8          | 発行したTransferAccountを同じ端末で使用しました。 |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | ゲストではないアカウントから移行しようとしたか、アカウントにゲスト以外のIdPが連携されています。 |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccountの有効期限が切れました。 |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 無効なTransferAccountを複数回入力したため、アカウント移行機能がロックされました。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccountのIDが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccountのパスワードが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccountが設定されていません。<br/>先にTOAST Gamebaseコンソールで設定してください。 |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccountがありません。先にTransferAccountを発行してください。 |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccountがすでに存在します。 |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccountはすでに使われています。 |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       |トークンログインに失敗しました。                         |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       |トークン情報が有効ではありません。                       |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近ログインしたIdPの情報がありません。                  |
| IdP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdPログインに失敗しました。                        |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | マッピング追加に失敗しました。                          |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 既に他のメンバーにマッピングされています。                     |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 既に同じIdPにマッピングされています。                    |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | IdP情報が有効でありません。(Consoleに該当するIdPの情報がありません。) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | ゲストIdPではAddMappingができません。 |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 強制マッピングキー(ForcingMappingKey)がありません。<br/>ForcingMappingTicketを再度確認してください。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 強制マッピングキー(ForcingMappingKey)がすでに使われています。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 強制マッピングキー(ForcingMappingKey)の有効期限が切れました。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 強制マッピングキー(ForcingMappingKey)が他のIdPに使用されました。<br/>発行したForcingMappingKeyは、同じIdPに強制マッピングを試行するのに使用されます。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 強制マッピングキー(ForcingMappingKey)が他のアカウントに使用されました。<br/>発行したForcingMappingKeyは、同じIdPおよびアカウントに強制マッピングを試行するのに使用されます。 |
| Remove Mapping | AUTH\_REMOVE\_MAPPING\_FAILED            | 3401       | マッピング削除に失敗しました。                          |
|                | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 最後にマッピングされたIdPは削除することができません。              |
|                | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP   | 3403       | 現在ログイン中のIdPです。                    |
| Logout         | AUTH\_LOGOUT\_FAILED                     | 3501       | ログアウトに失敗しました。                           |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退会に失敗しました。                             |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | すでに一時退会中のユーザーです。                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 一時退会中のユーザーではありません。                     |
| Not Playable   | AUTH\_NOT\_PLAYABLE                      | 3701       | プレイできない状態です(メンテナンスまたはサービス終了など)。        |
| Auth(Unknown)  | AUTH\_UNKNOWN\_ERROR                     | 3999       | 不明なエラーです。(定義されていないエラーです)。            |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [Entire Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、外部認証ライブラリーで発生したエラーです。
* 外部ライブラリーエラーの詳細は次のように確認できます。

```java
Gamebase.login(activity, provider, new GamebaseDataCallback<AuthToken>() {
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

* IdP SDKのエラーコードは各IdPのDeveloperページをお参照ください。
