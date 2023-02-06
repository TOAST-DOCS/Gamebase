## Game > Gamebase > Unity SDK ご利用ガイド > 認証

## Login

Gamebaseでは基本的にゲストログインに対応しています。<br/>


* ゲスト以外のProviderでログインするためには、該当するProvider AuthAdapterが必要です。
* AuthAdapter及び3rd-Party Provider SDKの設定は、次をご参考ください。
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)

### Login Flow

多くのゲームがタイトル画面にログインを設計しています。

* アプリをインストールして初めて起動したとき、タイトル画面からゲームユーザーがどのIdP(identity provider)で認証するか選択できるようにします。
* 一度ログインした後は、IdP選択画面を表示せずに前回ログインしたIdPタイプで認証します。

上述したロジックは、次のような手順で設計することができます。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 前回のログインタイプで認証

* 前回の認証記録がある場合、IDとパスワードを入力させずに認証を試みます。
* **Gamebase.LoginForLastLoggedInProvider()**を呼び出します。

#### 1-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **Gamebase.GetUserID()**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 1-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.LoginForLastLoggedInProvider()**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)**で制裁情報を確認し、ゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebaseを初期化する際に**GamebaseConfiguration.enablePopup**及び**GamebaseConfiguration.enableBanPopup**の値をtrueすると、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * 前回のログインタイプで認証に失敗しました。**'3. 指定されたIdPで認証'**を進めます。

#### 2. 指定されたIdPで認証

* IdPのタイプを直接指定して認証を試みます。
    * 認証可能なタイプは、**GamebaseAuthProvider**クラスに宣言されています。
* **Gamebase.Login(providerName, callback)**APIを呼び出します。

#### 2-1. 認証に成功した場合

* おめでとうございます！認証に成功しました。
* **Gamebase.GetUserID()**でユーザーIDを取得し、ゲームロジックを設計してください。

#### 2-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.Login(providerName, callback)**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 利用停止中のゲームユーザー
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **GamebaseResponse.Auth.BanInfo.From(GamebaseError error)**で制裁情報を確認し、ゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebaseを初期化する際に**GamebaseConfiguration.enablePopup**及び**GamebaseConfiguration.enableBanPopup**の値をtrueすると、Gamebaseが利用停止に関するポップアップを自動で表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに知らせ、ゲームユーザーが認証IdPのタイプを選択できる状態(主にタイトル画面またはログイン画面)に戻ります。

### Login as the Latest Login IdP

最後にログインしたIdPでログインを試みます。
該当するログイントークンの期限が切れていたり、トークン検証などに失敗した場合、失敗を返します。
この場合、[該当するIdPに対するログイン](#login-with-idp)を設計する必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void LoginForLastLoggedInProvider(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void LoginForLastLoggedInProvider()
{
	Gamebase.LoginForLastLoggedInProvider((authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Login succeeded.");
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("Login failed. error is {0}", error));
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
            else
            {
                Debug.Log("Try to login using a specifec IdP");
                Gamebase.Login("ProviderName", (authToken, error) => {});
            }
        }
    });
}
```

### Login with GUEST

Gamebaseは、ゲストログインに対応しています。

* デバイス固有のキーを作成し、Gamebaseに対しログインを試みます。
* ゲストログインはデバイスキーが初期化されることがあり、デバイスキーを初期化する場合はアカウントが削除されることがあるため、IdPを使ったログイン方式を推奨します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.GUEST, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

### Login with IdP

次は特定のIdPでログインできるようにするコード例です
ログインできるIdPタイプは**GamebaseAuthProvider**クラスで確認できます。

> [参考]
>
> ログインする時、追加情報を必要とするIdPもあります。
> このような追加情報を設定できるようにstatic void Login(string providerName, Dictionary additionalInfo, GamebaseCallback.GamebaseDelegate callback) APIを提供します。
>additionalInfoパラメータに必須情報をdictionary形式で入力してください。
>additionalInfo値がある場合はその値を使用し、nullの場合は[NHN Cloud Console](./oper-app/#authentication-information)に登録された値を使用します。

> [参考]
>
> Line IdPはGamebase SDK 2.43.0からLineサービス提供地域設定をできます。
> 該当地域はAdditionalInfoに設定できます。 

> <font color="red">[注意]</font><br/>
>
> StandaloneではWebViewAdapterを通してログインをサポートし、WebViewが開かれている時、UIに入力されるEventをBlockingしません。
>
> Standalone WebViewAdapterを使用してログインを行うにはIdP開発者サイトで以下のCallbackURLを設定する必要があります。
> - https://id-gamebase.toast.com/oauth/callback
>

* additionalInfoパラメータ設定方法

| keyname                                  | a use                                    | 値種類                                   |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | Lineサービス提供地域設定 | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.FACEBOOK, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}

public void LoginWithAdditionalInfo()
{
    var additionalInfo = new Dictionary<string, object>
    {
        { "key", "value" }
    };

    Gamebase.Login(GamebaseAuthProvider.FACEBOOK, additionalInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {            
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

### Login with Credential

IdPが提供するSDKを使ってゲームで直接認証した後、発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェースです。

> <font color="red">[注意]</font><br/>
>
> Standalone、WebGLプラットフォームでGoogleログインを行う場合にはGamebaseAuthProviderCredential.REDIRECT_URIを入力する必要があります。入力していない場合は`redirect_uri_mismatch`エラーが発生します。
>
> REDIRECT_URIは**Google Cloud Console > APIおよびサービス > ユーザー認証情報 > Webクライアント**の承認されたリダイレクションURIに追加された値を入力します。 (GoogleログインペーでAuthCodeを受け取るURI)
>
> REDIRECT_URIを入力していない場合はデフォルト値が適用されます。
>   - Standalone: http://localhost:8080/
>   - WebGL: http://localhost/

* Credentialパラメーターの設定方法

| keyname | a use | 値の種類 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdPタイプ設定                         | GamebaseAuthProvider.GOOGLE<br>GamebaseAuthProvider.GAMECENTER<br> GamebaseAuthProvider.FACEBOOK<br>GamebaseAuthProvider.NAVER<br>GamebaseAuthProvider.TWITTER<br>GamebaseAuthProvider.LINE<br>GamebaseAuthProvider.HANGAME<br>GamebaseAuthProvider.APPLEID<br>GamebaseAuthProvider.WEIBO<br>GamebaseAuthProvider.KAKAOGAME<br>GamebaseAuthProvider.PAYCO |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)の設定<br/>Google認証の場合は使用しない |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Googleログイン後に取得できるOTAC(one time authorization code)の入力 |                                          |
| GamebaseAuthProviderCredential.GAMEBASE_ACCESS_TOKEN | IdP認証情報ではなくGamebase Access Tokenでログインを行いたい場合に使用 |  |
| GamebaseAuthProviderCredential.IGNORE_ALREADY_LOGGED_IN | Gamebaseログイン状態からログアウトを行わずに別のアカウントへのログイン試行を許可する | **bool** |
| GamebaseAuthProviderCredential.LINE_CHANNEL_REGION | Lineサービス提供地域設定 | [Login with IdP参考](./unity-authentication/#login-with-idp) |
| GamebaseAuthProviderCredential.REDIRECT_URI | **Google Cloud Console > 承認されたリダイレクションURI**に登録されたredirect_uriを追加<br>**Standalone default**: http://localhost:8080/<br>**WebGL default**: http://localhost/<br/>**Standalone, WebGLプラットフォームでGoogleログイン時に限る** |  |

> [参考]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>

> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は、外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

UnityEditorでは、Facebookログインのみ対応しています。

```cs
static void Login(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void LoginWithCredential()
{
    var credentialInfo = new Dictionary<string, object>();
    
    // facebook
    credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.FACEBOOK);
    credentialInfo.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, "facebook access token");
    
    // google
    // credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.GOOGLE);
    // credentialInfo.Add(GamebaseAuthProviderCredential.AUTHORIZATION_CODE, "google auchorization code");
    
    Gamebase.Login(credentialInfo, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error) == true)
        {
            
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            // Check the error code and handle the error appropriately.
        	Debug.Log(string.Format("Login failed. error is {0}", error));
            if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry Login or notify an error message to the user.: {0}", error.message));
            }
            else if (error.code == GamebaseErrorCode.BANNED_MEMBER)
            {
                GamebaseResponse.Auth.BanInfo banInfo = GamebaseResponse.Auth.BanInfo.From(error);
                if (banInfo != null)
                {
                }
            }
        }
    });
}
```

## Logout

ログインされたIdPからのログアウトを試みます。主にゲームの設定画面にログアウトボタンを設け、ボタンをクリックすると実行されるように設計するケースが多いです。
ログアウトに成功してもゲームユーザーのデータは維持されます。
ログアウトに成功した場合、該当するIdPで認証を行った記録が削除されるため、次回ログインする時にID・パスワードの入力ウィンドウが表示されます。<br/><br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Logout(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void Logout()
{
    Gamebase.Logout((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Logout succeeded.");
        }
        else
        {
        	Debug.Log(string.Format("Logout failed. error is {0}", error));
        }
    });
}
```

## Withdraw

ログインした状態で退会を試みます。

* 退会成功時
  * ログインしていたIdPのゲーム利用者データは削除されます。
  * 該当IdPで再度ログインできます。その場合は新しいゲーム利用者データを作成します。
  * 連携中のすべてのIdPがログアウト処理されます。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連動中の場合、すべてのIdP連動が解除され、Gamebaseユーザーデータが削除されます。
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Withdraw(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void Withdraw()
{
    Gamebase.Withdraw((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Withdraw succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Withdraw failed. error is {0}", error));
        }
    });
}
```

## Mapping

マッピングは、既にログインされているアカウントに他のIdPアカウントを連携させたり、解除する機能です。

ほとんどのゲームにおいて、一つのアカウントに複数のIdPを連携(Mapping)させることができるようになっています。
GamebaseのMappingAPIを使用して既にログインされているアカウントに他のIdPのアカウントを連携させたり、解除することができます。<br/>

このように、一つのGamebaseユーザーIDに様々なIdPアカウントを連携することができます。
つまり、連携中のIdPアカウントでログインを試みる場合、常に同じユーザーIDでログインされることになります。<br/>

注意すべき点は、各IdPは一つのアカウントにのみ連携させることができるという点です。
例は、次の通りです。<br/>

* GamebaseユーザーID: 123bcabca
	* Google ID: aa
	* Facebook ID: bb
	* AppleGameCenter ID: cc
	* PAYCO ID: dd
* GamebaseユーザーID: 456abcabc
	* Google ID: ee
	* Google ID: ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

Mappingには、Mapping追加APIと解除APIの2つがあります。

### Add Mapping Flow

マッピングは、次の手順で設計することができます。

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. ログイン
マッピングは、現在のアカウントにIdPアカウントの連携を追加する機能であるため、ログインされた状態でなければなりません。
まず、ログインAPIを呼び出してログインします。

#### 2. マッピング

**Gamebase.AddMapping()**を呼び出してマッピングを試みます。

#### 2-1. マッピングに成功した場合

* おめでとうございます！現在のアカウントと連携しているIdPアカウントが追加されました。
* マッピングに成功しても、「現在ログイン中のIdP」は変わりません。<br>つまり、Googleアカウントでログインした後、Facebookアカウントのマッピングを試み、それが成功したからといって「現在ログイン中のIdP」がGoogleからFacebookに変更されるわけではありません。Googleのままで維持されます。
* マッピングは、単にIdP連携だけを追加する機能です。

#### 2-2. マッピングに失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワーク問題により認証に失敗したケースであるため、**Gamebase.AddMapping()**をもう一度呼び出したり、しばらくしてからもう一度試します。
* 既に他のアカウントに連携している場合に発生するエラー
    * エラーコード**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**は、マッピングしようとしているIdPのアカウントが既に他のアカウントに連携しているという意味です。連携済みのアカウントを解除したい場合、該当するアカウントでログインしてから**Gamebase.Withdraw()**を呼び出して退会したり、**Gamebase.RemoveMapping()**を呼び出して連携を解除した後、もう一度マッピングを試みてください。
* 既に同じIdPアカウントに連携されている場合に発生するエラー
	* エラーコード**AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**は、マッピングしようとしているIdPと同じ種類のアカウントが既に連携しているという意味です。
	* Gamebaseのマッピングは、IdP一つにつき一つのアカウントのみ連携させることができます。例えば、既にPAYCOアカウントに連携している場合は、これ以上PAYCOアカウントを追加することができません。
	* 同じIdPの他のアカウントを連携させるためには、**Gamebase.RemoveMapping()**を呼び出して連携を解除してからもう一度マッピングを試みてください。
* その他のエラー
    * マッピングに失敗しました。


### Add Mapping

特定のIdPにログインされた状態で他のIdPへのMappingを試みます。
MappingしようとしているIdPのアカウントが既に他のアカウントに連携している場合、
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**エラーを返します。<br/>

Mappingに成功しても、「現在ログイン中のIdP」は変わりません。つまり、Googleアカウントでログインした後、FacebookアカウントのMappingを試み、それが成功したからといって「現在ログイン中のIdP」がGoogleからFacebookに変更されるわけではありません。Googleのままで維持されます。
Mappingは、 単にIdP連携だけを追加する機能です。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMapping(string providerName)
{
    Gamebase.AddMapping(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("AddMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("AddMapping failed. error is {0}", error));
        }
    });
}
```

### AddMapping with Credential

ゲームで直接ID Providerに提供するSDKで、予め認証を行い発行されたアクセストークンなどを利用してGamebase AddMappingをすることができるインターフェースです。


* Credentialパラメーターの設定方法

| keyname | a use | 値の種類 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdPタイプの設定                            | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)設定<br/>Google認証の場合は使用しない |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Googleログイン後に取得できるOTAC(one time authorization code)を入力 |                                          |

> [TIP]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用しなければならないとき、必要になることがあります。
>


> <font color="red">[注意]</font><br/>
>
> 外部のSDKで対応を求める開発事項は外部SDKのAPIを使用して設計する必要があり、Gamebaseでは対応しておりません。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingWithCredential()
{
    var credentialInfo = new Dictionary<string, object>();

    // facebook
    credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.FACEBOOK);
    credentialInfo.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, "facebook access token");

    // google
    // credentialInfo.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, GamebaseAuthProvider.GOOGLE);
    // credentialInfo.Add(GamebaseAuthProviderCredential.AUTHORIZATION_CODE, "google auchorization code");

    Gamebase.AddMapping(credentialInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("AddMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("AddMapping failed. error is {0}", error));
        }
    });
}
```

### Add Mapping Forcibly
特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

次はFacebookに強制マッピングを試行する例です。

**API**

```cs
static void AddMappingForcibly(GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
// Legacy API
static void AddMappingForcibly(string providerName, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void AddMappingForcibly(string providerName, string forcingMappingKey, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void AddMappingForcibly(Dictionary<string, object> credentialInfo, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(string idPName)
{
    Gamebase.AddMapping(idPName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // マッピング追加成功
        }
        else
        {
            // まずAddMapping APIを呼び出し、すでに連携されているアカウントでマッピングを試行し、次のようにForcingMappingTicketを取得できます。
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // ForcingMappingTicketクラスのFrom()メソッドを利用してForcingMappingTicketインスタンスを取得します。
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

                // 強制マッピングを試行します。
                Gamebase.AddMappingForcibly(forcingMappingTicket, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // 強制マッピング追加成功
                    }
                    else
                    {
                        // 強制マッピング追加失敗
                        // エラーコードを確認し、エラーを解決します。
                    }
                });
            }
            else
            {
                // エラーコードを確認し、エラーを解決します。
            }
        }
    });
}
```

### Change Login with ForcingMappingTicket

特定IdPにすでにマッピングされているアカウントがある時、現在のアカウントをログアウトして、すでにマッピングされているアカウントでログインします。
この時、AddMapping APIから取得した`ForcingMappingTicket`が必要です。

Change Login APIの呼び出しが失敗した場合、Gamebaseログイン状態は既存のUserIDで維持されます。

**API**

```cs
static void ChangeLogin(GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

次はFacebookでマッピングを試みた後、Facebookにすでにマッピングされているアカウントが存在するため、該当アカウントにログインを変更する例です。

```cs
public void ChangeLoginWithFacebook()
{
    Gamebase.AddMapping(GamebaseAuthProvider.FACEBOOK, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // マッピング追加成功
            return;
        }
        
        // まずAddMapping APIを呼び出し、すでに連携されているアカウントでマッピングを試行して、次のようにForcingMappingTicketを取得できます。
        if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
        {
            // ForcingMappingTicketクラスのFrom()メソッドを利用してForcingMappingTicketインスタンスを取得します。
            GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.From(error);

            // ForcingMappingTicketのUserIDでログインします。
            Gamebase.ChangeLogin(forcingMappingTicket, (authTokenForcibly, errorForcibly) =>
            {
                if (Gamebase.IsSuccess(errorForcibly) == true)
                {
                    // ログイン変更成功
                }
                else
                {
                    // ログイン変更失敗
                    // エラーコードを確認し、適切な処理を行います。
                }
            });
        }
        else
        {
            // Add Mapping Failed.
            // エラーコードを確認し、適切な処理を行います。
        }
    });
}
```

### Remove Mapping

特定のIDPに対する連携を解除します。解除しようとしているIdP以外にIdPがない場合、失敗を返します。
連携を解除した後は、Gamebase内部で該当するIdPに対するログアウト処理を行います。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RemoveMapping(string providerName, GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void RemoveMapping(string providerName)
{
    Gamebase.RemoveMapping(providerName, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RemoveMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RemoveMapping failed. error is {0}", error));
        }
    });
}
```

### Get Mapping List

ユーザーIDに連携されているIdPリストを返します。<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static List<string> GetAuthMappingList()
```

**Example**

```cs
public void GetAuthMappingList()
{
    List<string> mappingList = Gamebase.GetAuthMappingList();
}
```
## Gamebase User`s Information

Gamebaseを通して認証フローを進めた後、アプリを制作する際に必要な情報を取得することができます。

### Get Authentication Information for Gamebase
Gamebaseを通して認証フローを進めた後、アプリを制作する際に必要な情報を取得することができます。

#### UserID

Gamebaseから発行されたUserIDを取得することができます。
**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetUserID()
```

**Example**
```cs
public void GetUserID()
{
    string userID = Gamebase.GetUserID();
}
```

#### AccessToken

Gamebaseから発行されたアクセストークンを取得することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAccessToken()
```

**Example**
```cs
public void GetAccessToken()
{
    string accessToken = Gamebase.GetAccessToken();
}
```

#### Last LoggedIn Provider Name

Gamebaseから最後にログインに成功したProviderNameを取得することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetLastLoggedInProvider()
```

**Example**
```cs
public void GetLastLoggedInProvider()
{
    string lastLoggedInProvider = Gamebase.GetLastLoggedInProvider();
}
```

### Get Authentication Information for External IdP

* 外部認証IdPのアクセストークン、ユーザーID、Profileなどの情報はログイン後、ゲームサーバーでGamebase Server APIを呼び出して取得できます。
    * [Game > Gamebase > APIガイド > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[注意]</font><br/>
>
> * 外部IdPの認証情報はセキュリティのためにゲームサーバーで呼び出すことを推奨します。
> * IdPによってはアクセストークンの有効期限が短い場合があります。
>     * 例えばGoogleはログインしてから2時間後にはアクセストークンの有効期限が切れます。
>     * ユーザー情報が必要な場合はログイン後、すぐにGamebase Server APIを呼び出してください。
> * "Gamebase.LoginForLastLoggedInProvider()" APIでログインした場合には認証情報を取得できません。
>     * ユーザー情報が必要な場合は"Gamebase.LoginForLastLoggedInProvider()"の代わりに、使用したいIDPCodeと同じ{IDP_CODE}をパラメータにして"Gamebase.Login(IDP_CODE, callback)" APIでログインする必要があります。

### Get Banned User Information

Gamebase Consoleに制裁中のゲームユーザーと登録されている場合、
ログインを試行すると下記のような利用制限情報コードが表示されることがあります。**GamebaseResponse.Auth.BanInfo.from(GamebaseError error)**メソッドを利用して制裁情報を確認できます。

* BANNED_MEMBER(7)

## TransferAccount
ゲストアカウントを他の端末に移行するためのキーを発行する機能です。

このキーを**TransferAccountInfo**と呼びます。
発行されたTransferAccountInfoは、他の端末で**requestTransferAccount**APIを呼び出してアカウント移行ができます。

> <font color="red">[注意]</font><br/>
> TransferAccountInfoは、ゲストログイン状態でのみ発行できます。
> TransferAccountInfoを利用したアカウント移行は、ゲストログイン状態またはログインしていない状態でのみ可能です。
> ログインしたゲストアカウントがすでに他の外部IdP(Google、Facebook、PAYCOなど)アカウントとマッピングされている場合は、アカウント移行がサポートされません。

### Issue TransferAccount
ゲストアカウントを移行するためにTransferAccountInfoを発行します。

**API**

```cs
static void IssueTransferAccount(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void IssueTransferAccount()
{
    Gamebase.IssueTransferAccount((transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Issuing TransferAccount success.
        }
        else
        {
            // Issuing TransferAccount failed.
        }
    });
}
```

### Query TransferAccount
ゲストアカウントを移行するために、すでに発行されているTransferAccountInfo情報をGamebaseサーバーに問い合わせます。

**API**

```cs
static void QueryTransferAccount(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void QueryTransferAccount()
{
    Gamebase.QueryTransferAccount((transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Querying TransferAccount success.
        }
        else
        {
            // Querying TransferAccount failed.
        }
    });
}
```


### Renew TransferAccount
すでに発行されたTransferAccountInfo情報を更新します。
更新方法には**自動更新**と**手動更新**があり、**パスワードのみ更新**、**IDとパスワードを更新**を選択してTransferAccountInfo情報を更新できます。

```cs
static void RenewTransferAccount(GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void RenewTransferAccountManualIdPassword(string accountId, string accountPassword)
{
    // 手動設定
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountId, accountPassword); // ID + Password
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountPassword); // Password

    // 自動設定
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeAutoRenewConfiguration(type);
    
    Gamebase.RenewTransferAccount(configuration, (transfer, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Renewing TransferAccount success.
        }
        else
        {
            // Renewing TransferAccount failed.
        }
    });
}
```




### Transfer Guest Account to Another Device
**issueTransfer**APIで発行したTransferAccountでアカウントを移行する機能です。
アカウントの移行に成功した時、TransferAccountを発行した端末で移行完了メッセージが表示される場合があり、ゲストログインすると新規のアカウントが作成されます。
アカウント移行が成功した端末では、TransferAccountを発行した端末のゲストアカウントを継続して使用できます。

> <font color="red">[注意]</font><br/>
> ゲストでログインした状態でアカウントを移行すると、ゲストアカウントは消滅します。
> * 誤ったid/passwordを連続して入力すると、**AUTH_TRANSFERACCOUNT_BLOCK(3042)**エラーが発生し、アカウント移行が一定時間できなくなります。
> この場合は、次の例のようにTransferAccountFailInfo値を利用して、いつまでアカウント移行ができないかをユーザーに伝えることができます。

**API**

```cs
static void TransferAccountWithIdPLogin(string accountId, string accountPassword, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void TransferAccountWithIdPLogin(string accountId, string accountPassword)
{
    Gamebase.TransferAccountWithIdPLogin(accountId, accountPassword, (authToken, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // Transfering Account success.
            // TODO: implements post login process
        }
        else
        {
            // Check the error code and handle the error appropriately.
            if (error.code == GamebaseErrorCode.AUTH_TRANSFERACCOUNT_BLOCK)
            {
                GamebaseResponse.Auth.TransferAccountFailInfo transferAccountFailInfo = GamebaseResponse.Auth.TransferAccountFailInfo.From(error);
                if (transferAccountFailInfo != null)
                {
                    // Transfering Account failed by entering the wrong id / pw multiple times.
                    // You can tell when the account transfer is blocked by the TransferAccountFailInfo.
                    string failId = transferAccountFailInfo.id;
                    int failCount = transferAccountFailInfo.failCount;
                    DateTime dateTime = new DateTime(transferAccountFailInfo.blockEndDate);
                }
            }
        }
    });
}
```

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

> 退会猶予機能を使用する場合には**Gamebase.withdraw()**APIを使用しないでください。

**API**

```cs
public static void RequestWithdrawal(GamebaseCallback.GamebaseDelegate<GamebaseResponse.TemporaryWithdrawalInfo> callback)
```

**Example**

```cs
public void SampleRequestWithdrawal()
{
    Gamebase.TemporaryWithdrawal.RequestWithdrawal((data, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            long gracePeriodDate = data.gracePeriodDate;
            Debug.Log(string.Format("RequestWithdrawal succeeded. The date when you can withdraw your withdrawal is {0}", gracePeriodDate));
        }
        else
        {
            Debug.Log(string.Format("RequestWithdrawal failed. error:{0}", error));
        }
    });
}
```

### Check TemporaryWithdrawal User

退会猶予を使用するゲームは、AuthToken.member.temporaryWithdrawalがnullではない場合、ログイン後に常に該当ユーザーに退会進行中であることを伝える必要があります。

**Example**

```cs
public void LoginSample()
{
    Gamebase.Login(GamebaseAuthProvider.XXX, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            if(authToken.member.temporaryWithdrawal != null)
            {
                long gracePeriodDate = authToken.member.temporaryWithdrawal.gracePeriodDate;
                Debug.Log(string.Format("User is under temporary withdrawa. GracePeriodDate: {0}", error));
            }            
            else
            {
                string userId = authToken.member.userId;            
                Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.            
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}

```

### Cancel TemporaryWithdrawal

退会リクエストをキャンセルします。
退会リクエストした後、期間が満了して退会が完了すると、キャンセルができません。

> 退会猶予機能を使用する場合には**Gamebase.withdraw()**APIを使用しないでください。

**API**

```cs
static void CancelWithdrawal(GamebaseCallback.ErrorDelegate callback)
```
**Example**

```cs
public void SampleCancelWithdrawal()
{
    Gamebase.TemporaryWithdrawal.CancelWithdrawal((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("CancelWithdrawal succeeded.");
        }
        else
        {
            Debug.Log(string.Format("CancelWithdrawal failed. error:{0}", error));
        }
    });
}
```

### Withdraw Immediately

退会猶予期間を無視して、即時退会を進行します。
実際の内部動作はGamebase.withdraw() APIと同じです。

即時退会はキャンセルできないため、実行するかどうかをユーザーによく確認してください。

**API**

```cs
static void WithdrawImmediately(GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void SampleWithdrawImmediately()
{
    Gamebase.TemporaryWithdrawal.WithdrawImmediately((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("WithdrawImmediately succeeded.");
        }
        else
        {
            Debug.Log(string.Format("SampleWithdrawImmediately failed. error:{0}", error));
        }
    });
}
```

* 「決済アビューズ自動解除」機能です。
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止にならなければいけないユーザーが利用停止猶予状態後、利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常にプレイが可能になります。
    * 期間内に条件を満たせなかった場合、利用停止になります。
* 決済アビューズ自動解除機能を使用するゲームはログイン後、常にAuthToken.getGraceBanInfo() APIを呼び出して結果がnullではない有効なGraceBanInfoオブジェクトを返した場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

**Example**

```cs
public void Login()
{
	Gamebase.Login(GamebaseAuthProvider.GUEST, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error) == false)
        {
            // Login failed
            return;
        }

        // Check if user is under grace ban
        GamebaseResponse.Common.Member.GraceBanInfo graceBanInfo = authToken.member.graceBan;
        if (graceBanInfo != null)
        {
            string periodDate = string.Format("{0:yyyy/MM/dd HH:mm:ss}", 
                new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(graceBanInfo.gracePeriodDate));
            string message = graceBanInfo.message;
            
            GamebaseResponse.Common.Member.GraceBanInfo.ReleaseRuleCondition releaseRuleCondition = graceBanInfo.releaseRuleCondition;
            if (releaseRuleCondition != null)
            {
                // condition type: "AND", "OR"
                string releaseRule = string.Format("{0}{1} {2} {3}time(s)", releaseRuleCondition.amount,
                    releaseRuleCondition.currency, releaseRuleCondition.conditionType, releaseRuleCondition.count);
            }

            GamebaseResponse.Common.Member.GraceBanInfo.PaymentStatus paymentStatus = graceBanInfo.paymentStatus;
            if (paymentStatus != null) {
                String paidAmount = paymentStatus.amount + paymentStatus.currency;
                String paidCount = paymentStatus.count + "time(s)";
            }

            // Guide the user through the UI how to finish the grace ban status.
        }
        else
        {
            // Login success.
        }
    });
}
```


## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 無効な会員へのリクエストです。                        |
|                | BANNED\_MEMBER                           | 7          | 制裁を受けている会員です。                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | ログインがキャンセルされました。                            |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | サポートしていない認証方式です。                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 存在しないか退会した会員です。                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 外部認証ライブラリの初期化に失敗しました。 |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部認証ライブラリエラーです。<br/>詳細エラーを確認してください。 |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | 以前の認証プロセスが完了していません。 |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | Gamebase Access Tokenが有効ではないためログアウトしました。<br/>ログインを再試行してください。 |
| TransferKey | SAME\_REQUESTOR | 8 | 発行したTransferKeyを同じ端末で使用しました。 |
|             | NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | ゲストではないアカウントから移行しようとしたか、アカウントにゲスト以外のIdPが連携されています。 |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccountの有効期限が切れました。 |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 無効なTransferAccountを複数回入力したため、アカウント移行機能がロックされました。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccountのIDが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccountのパスワードが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccountが設定されていません。<br/>先にNHN Cloud Gamebaseコンソールで設定してください。 |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccountが存在しません。TransferAccountを先に発行してください。 |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccountがすでに存在します。 |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccountは使われています。 |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 |トークンログインに失敗しました。|
|              | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 |トークン情報が有効ではありません。|
|              | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 最近ログインしたIdPの情報がありません。|
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdPログインに失敗しました。|
|           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | マッピング追加に失敗しました。|
|             | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 既に他のメンバーにマッピングされています。 |
|             | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 既に同じIdPにマッピングされています。 |
|             | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdPではAddMappingができません。 |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 強制マッピングキー(ForcingMappingKey)が存在しません。<br/>ForcingMappingTicketをもう一度確認してください。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 強制マッピングキー(ForcingMappingKey)がすでに使われています。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 強制マッピングキー(ForcingMappingKey)の有効期限が切れました。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 強制マッピングキー(ForcingMappingKey)が他のIdPに使用されました。<br/>発行したForcingMappingKeyは同じIdPに強制マッピングを試行するのに使用されます。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 強制マッピングキー(ForcingMappingKey)が他のアカウントに使用されました。<br/>発行したForcingMappingKeyは、同じIdPおよびアカウントに強制マッピングを試行するのに使用されます。 |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | マッピング削除に失敗しました。|
|                | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 最後にマッピングされたIdPは、削除することができません。|
|                | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 現在ログイン中のIdPです。|
| Logout | AUTH_LOGOUT_FAILED | 3501 | ログアウトに失敗しました。|
| Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | 退会に失敗しました。|
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | すでに一時退会中のユーザーです。                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 一時退会中のユーザーではありません。                     |   
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | プレイできない状態です。(メンテナンスまたはサービス終了など) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 不明なエラーです。(定義されていないエラーです。) |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)


**AUTH_EXTERNAL_LIBRARY_ERROR**

* このエラーは外部認証ライブラリでエラーが発生した時に返されます。
* 外部認証ライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError) == true)
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    if (gamebaseError.code == GamebaseErrorCode.AUTH_EXTERNAL_LIBRARY_ERROR) 
    {
        GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
        if (null != moduleError)
        {
            int moduleErrorCode = moduleError.code;
            string moduleErrorMessage = moduleError.message;        
            Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
        }
    }
}
```

* 詳細エラーコードは、それぞれの外部認証ライブラリのDeveloperページを参照してください。
