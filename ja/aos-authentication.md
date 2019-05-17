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
+ (void)Gamebase.addMapping(Activity activity, String providerName, null, GamebaseDataCallback<AuthToken> callback);
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
                // Mappingを試みているIDPアカウントが既に他のアカウントに連携されています。
                // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 하거나, 다음과 같이
                // ForcingMappingTicket을 획득 후, addMappingForcibly() 메소드를 이용하여 강제 매핑을 시도합니다.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 자세한 내용은 addMappingForcibly API 문서를 참고하세요.    
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
                // Mappingを試みているIDPアカウントが既に他のアカウントに連携されています。
                // 강제로 연동을 해제하기 위해서는 해당 계정의 탈퇴나 Mapping 해제를 하거나, 다음과 같이
                // ForcingMappingTicket을 획득 후, addMappingForcibly() 메소드를 이용하여 강제 매핑을 시도합니다.
                Log.e(TAG, "Add Mapping failed- ALREADY_MAPPED_TO_OTHER_MEMBER");
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException exception) {
                        ...
                        // 자세한 내용은 addMappingForcibly API 문서를 참고하세요.    
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
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```java
+ (void)Gamebase.addMappingForcibly(Activity activity, String providerName, String forcingMappingKey, Map<String, Object> additionalInfo, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
private static void addMappingForciblyFacebook(final Activity activity) {
    Gamebase.addMapping(activity, AuthProvider.FACEBOOK, null, new GamebaseDataCallback<AuthToken>() {
        @Override
        public void onCallback(AuthToken result, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // マッピング追加成功
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicket 클래스의 from() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 강제 매핑을 시도합니다.
                Gamebase.addMappingForcibly(activity, AuthProvider.FACEBOOK, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 강제 매핑 추가 성공
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
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
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMappingForcibly를 호출 할 수 있는 인터페이스입니다.

* Credential 파라미터 설정방법

| keyname                                  | a use                                    | 값 종류                                     |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| AuthProviderCredentialConstants.PROVIDER_NAME | IdP 유형 설정                                | AuthProvider.GOOGLE<br> AuthProvider.FACEBOOK<br>AuthProvider.PAYCO<br>AuthProvider.NAVER |
| AuthProviderCredentialConstants.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰)설정.<br/>Google 인증 시에는 사용 안 함. |                                          |
| AuthProviderCredentialConstants.AUTHORIZATION_CODE | Google 로그인 이후 획득할 수 있는 OTOC(one time authorization code) 입력 |                                          |

> [참고]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>

<br/>

> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발사항은 외부 SDK의 API를 사용하여 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

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
                // 매핑 추가 성공
                Log.d(TAG, "Add Mapping successful");
                String userId = Gamebase.getUserID();
                return;
            }

            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (exception.getCode() == GamebaseError.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) {
                // ForcingMappingTicket 클래스의 from() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                final ForcingMappingTicket ticket = ForcingMappingTicket.from(exception);
                final String forcingMappingKey = ticket.forcingMappingKey;

                // 강제 매핑을 시도합니다.
                Gamebase.addMappingForcibly(activity, credentialInfo, forcingMappingKey, null, new GamebaseDataCallback<AuthToken>() {
                    @Override
                    public void onCallback(AuthToken data, GamebaseException addMappingForciblyException) {
                        if (Gamebase.isSuccess(addMappingForciblyException)) {
                            // 강제 매핑 추가 성공
                            Log.d(TAG, "Add Mapping Forcibly successful");
                            String userId = Gamebase.getUserID();
                            return;
                        }           

                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
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


### Get Authentication Information for External IdP

外部の認証SDKでアクセストークン、ユーザーID、Profileなどの情報を取得することができます。

**API**

```java
+ (String)Gamebase.getAuthProviderUserID(String providerName);
+ (String)Gamebase.getAuthProviderAccessToken(String providerName);
+ (AuthProviderProfile)Gamebase.getAuthProviderProfile(String providerName);
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

## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급받은 TransferAccountInfo는 다른 기기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> `주의`
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

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
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

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
이미 발급받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

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
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> `주의`
> 이미 Guest 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

**API**

```java
+ (void)Gamebase.transferAccountWithIdPLogin(String accountId, String accountPassword, GamebaseDataCallback<AuthToken> callback);
```

**Example**

```java
Gamebase.transferAccountWithIdPLogin(accountId, accountPassword, new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(final AuthToken authToken, final GamebaseException exception) {
        if (!Gamebase.isSuccess(exception)) {
            // Transfering Account failed.
            return;
        }
        // Transfering Account success.
        // TODO: implements post login process
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
|                | AUTH_ALREADY_IN_PROGRESS_ERROR           | 3010       | 이전 인증 프로세스가 완료되지 않았습니다. |
| TransferAccount| SAME\_REQUESTOR                          | 8          | 발급한 TransferAccount를 동일한 기기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 Id가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login)   | AUTH\_TOKEN\_LOGIN\_FAILED               | 3101       |トークンログインに失敗しました。                         |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       |トークン情報が有効ではありません。                       |
|                | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 最近ログインしたIdPの情報がありません。                  |
| IDP Login      | AUTH\_IDP\_LOGIN\_FAILED                 | 3201       | IdPログインに失敗しました。                        |
|                | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO     | 3202       | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping    | AUTH\_ADD\_MAPPING\_FAILED               | 3301       | マッピング追加に失敗しました。                          |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 既に他のメンバーにマッピングされています。                     |
|                | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 既に同じIdPにマッピングされています。                    |
|                | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO   | 3304       | IdP情報が有効でありません。(Consoleに該当するIdPの情報がありません。) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IDP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IdP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
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

* IdP SDKのエラーコードは各IdPのDeveloperページをお参照ください。
