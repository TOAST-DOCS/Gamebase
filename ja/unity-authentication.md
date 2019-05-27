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

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

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
    * エラーコードが**AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **Gamebase.GetBanInfo()**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由についてご案内ください。
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
    * エラーコードが**AUTH_BANNED_MEMBER(3005)**の場合、利用停止中のゲームユーザーであるため認証に失敗したケースです。
    * **Gamebase.GetBanInfo()**で利用制限情報を確認し、ゲームユーザーに対しゲームプレイができない理由について知らせてください。
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
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user. : {0}", error.message));
            }
            else
            {
                Debug.Log("Try to login using a specifec IdP");
                Login("ProviderName");
            }
        }
    });
}

public void Login(string providerName)
{
    Gamebase.Login(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with GUEST

Gamebaseは、ゲストログインに対応しています。
デバイス固有のキーを作成し、Gamebaseに対しログインを試みます。
ゲストログインはデバイスキーが初期化されることがあり、デバイスキーを初期化する場合はアカウントが削除されることがあるため、IdPを使ったログイン方式を推奨します。

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
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with IdP

次は特定のIdPでログインできるようにするコード例です

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE


```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**providerName**

| Provider    | Define                          | Support Platform | 
| --------    | ------------------------------- | ---------------- |
| Google      | GamebaseAuthProvider.GOOGLE     | Android<br/>iOS<br/>Standalone |
| Game Center | GamebaseAuthProvider.GAMECENTER | iOS |
| Facebook    | GamebaseAuthProvider.FACEBOOK   | Android<br/>iOS<br/>Standalone |
| Payco       | GamebaseAuthProvider.PAYCO      | Android<br/>iOS<br/>Standalone |
| Naver       | GamebaseAuthProvider.NAVER      | Android<br/>iOS |
| Twitter     | GamebaseAuthProvider.TWITTER    | Android<br/>iOS |
| Line        | GamebaseAuthProvider.LINE       | Android |


> IdPの中には、ログインする際に必ず必要な情報があるものがあります。<br/>
> 例えば、Facebookログインを設計する場合、scopeなどを設定する必要があります。<br/>
> このような必須情報を設定することができるようにstatic void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)APIを提供します。<br/>
> パラメーターのadditionalInfoに必須情報をdictionary形式で入力してください。パラメーター値がの場合、TOAST Consoleに登録したadditionalInfoの値が埋められます。パラメーター値がある場合、Consoleに登録に登録してある値よりもこちらを優先してその値を上書きします。([TOAST ConsoleにadditionalInfoを設定する](./oper-app/#authentication-information))<br/>
> Stansalone에서는 WebViewAdapter를 통해서 로그인을 지원하며 WebView가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.


Standalone WebViewAdapter를 사용하여 로그인을 하기 위해서는 IdP 개발자 사이트에서 아래 CallbackURL을 설정 하여야 합니다.

* https://alpha-id-gamebase.toast.com/oauth/callback
* https://beta-id-gamebase.toast.com/oauth/callback
* https://id-gamebase.toast.com/oauth/callback

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
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}

public void Login(string providerName, Dictionary<string, object> additionalInfo)
{
    Gamebase.Login(providerName, additionalInfo, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Login with Credential

IdPが提供するSDKを使ってゲームで直接認証した後、発行されたアクセストークンなどを利用してGamebaseにログインできるインターフェースです。

* Credentialパラメーターの設定方法

| keyname | a use | 値の種類 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)の設定<br/>Google認証の場合は使用しない |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Googleログイン後に取得できるOTAC(one time authorization code)の入力 |                                          |

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
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Authentication Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout
ログインされたIdPからのログアウトを試みます。主にゲームの設定画面にログアウトボタンを設け、ボタンをクリックすると実行されるように設計するケースが多いです。
ログアウトに成功してもゲームユーザーのデータは維持されます。
ログアウトに成功した場合、該当するIdPで認証を行った記録が削除されるため、次回ログインする時にID・パスワードの入力ウィンドウが表示されます。<br/><br/>


次は、ログアウトボタンをクリックするとログアウトされるコード例です。

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

* 退会に成功した場合、ログインしたIdPに紐づいていたゲームユーザーデータは削除されます。
* 該当するIdPでもう一度ログインすることができ、新しいゲームユーザーデータを作成します。
* Gamebaseからの退会を意味するもので、IdPアカウントからの退会を意味するものではありません。
* 退会に成功すると、IdPログアウトを試みます。

次は、退会ボタンをクリックすると退会されるコード例です。

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

* GamebaseユーザーID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* GamebaseユーザーID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> すでにGoogleのeeアカウントに連携されているため、Googleアカウントを追加で連携させることができません。**

Mappingには、Mapping追加APIと解除APIの2つがあります。

### Add Mapping Flow

マッピングは、次の手順で設計することができます。

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
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdPタイプの設定                            | google, facebook, payco, iosgamecenter, naver, twitter, line |
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
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

다음은 Facebook에 강제 매핑을 시도하는 예시입니다.

**API**

```cs
static void AddMappingForcibly(string providerName, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(string idPName)
{
    Gamebase.AddMapping(idPName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // 매핑 추가 성공
        }
        else
        {
            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // ForcingMappingTicket 클래스의 MakeForcingMappingTicket() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.MakeForcingMappingTicket(error);

                // 강제 매핑을 시도합니다.
                Gamebase.AddMappingForcibly(idPName, forcingMappingTicket.forcingMappingKey, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // 강제 매핑 추가 성공
                    }
                    else
                    {
                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                    }
                });
            }
            else
            {
                // 에러 코드를 확인하고 적절한 처리를 진행합니다.
            }
        }
    });
}
```


### Add Mapping Forcibly with Credential
특정 IdP에 이미 매핑되어있는 계정이 있을 때, **강제로** 매핑을 시도합니다.
**강제 매핑**을 시도할 때는 AddMapping API에서 획득한 `ForcingMappingTicket`이 필요합니다.

게임에서 직접 IdP에서 제공하는 SDK로 먼저 인증하고 발급받은 액세스 토큰 등을 이용하여, Gamebase AddMappingForcibly를 호출 할 수 있는 인터페이스입니다.

* Credential 파라미터의 설정 방법

| keyname | a use | 값 종류 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP 유형 설정                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | IdP 로그인 이후 받은 인증 정보(액세스 토큰) 설정<br/>Google 인증 시에는 사용 안 함 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google 로그인 이후 받은 인증 정보(Authorization Code) 설정 |                                        |

> [TIP]
>
> 게임 내에서 외부 서비스(Facebook 등)의 고유 기능을 사용해야 할 때 필요할 수 있습니다.
>


> <font color="red">[주의]</font><br/>
>
> 외부 SDK에서 지원 요구하는 개발 사항은 외부 SDK의 API를 사용해 구현해야 하며, Gamebase에서는 지원하지 않습니다.
>

다음은 강제 매핑을 시도하는 예시입니다.

**API**

```cs
static void AddMappingForcibly(Dictionary<string, object> credentialInfo, string forcingMappingKey, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

```cs
public void AddMappingForcibly(Dictionary<string, object> credential)
{
    Gamebase.AddMapping(credential, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // 매핑 추가 성공
        }
        else
        {
            // 우선 addMapping API 호출 및, 이미 연동되어있는 계정으로 매핑을 시도하여, 다음과 같이, ForcingMappingTicket을 얻을 수 있습니다.
            if (error.code.Equals(GamebaseErrorCode.AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER) == true)
            {
                // ForcingMappingTicket 클래스의 MakeForcingMappingTicket() 메소드를 이용하여 ForcingMappingTicket 인스턴스를 얻습니다.
                GamebaseResponse.Auth.ForcingMappingTicket forcingMappingTicket = GamebaseResponse.Auth.ForcingMappingTicket.MakeForcingMappingTicket(error);

                // 강제 매핑을 시도합니다.
                Gamebase.AddMappingForcibly(credential, forcingMappingTicket.forcingMappingKey, (authTokenForcibly, errorForcibly) =>
                {
                    if (Gamebase.IsSuccess(error) == true)
                    {
                        // 강제 매핑 추가 성공
                    }
                    else
                    {
                        // 강제 매핑 추가 실패
                        // 에러 코드를 확인하고 적절한 처리를 진행합니다.
                    }
                });
            }
            else
            {
                // Add Mapping Failed.
                // 에러 코드를 확인하고 적절한 처리를 진행합니다.
            }
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

外部の認証SDKからアクセストークン、ユーザーID、Profileなどの認証情報を取得することができます。

#### UserID

外部認証SDKからユーザーIDを取得することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAuthProviderUserID()
```

**Example**

```cs
public void GetAuthProviderUserID(string providerName)
{
    string authProviderUserID = Gamebase.GetAuthProviderUserID(providerName);
}
```

#### AccessToken

外部の認証SDKからアクセストークンを取得することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetAuthProviderAccessToken(string providerName)
```

**Example**
```cs
public void GetAuthProviderAccessToken(string providerName)
{
    string authProviderAccessToken = Gamebase.GetAuthProviderAccessToken(providerName);
}
```

#### Profile

外部の認証SDKからProfileを取得することができます。
**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Auth.AuthProviderProfile GetAuthProviderProfile(string providerName)
```

**Example**
```cs
public void GetAuthProviderProfile(string providerName)
{
    GamebaseRequest.AuthProviderProfile profile = Gamebase.GetAuthProviderProfile(providerName);
}
```

### Get Banned User Infomation

Gamebase Consoleで利用制限対象のゲームユーザーに登録された場合、
ログインを試みると、利用制限情報コード(**BANNED_MEMBER(7)**)が表示されることがあり、次のAPIを利用して利用制限情報を確認することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static GamebaseResponse.Auth.BanInfo GetBanInfo()
```

**Example**
```cs
public void GetBanInfo()
{
    GamebaseResponse.Auth.BanInfo banInfo = Gamebase.GetBanInfo();
}
```

## TransferAccount
게스트 계정을 다른 단말기로 이전하기 위해 계정 이전을 위한 키를 발급받는 기능입니다.

이 키를 **TransferAccountInfo** 라고 부릅니다.
발급받은 TransferAccountInfo는 다른 기기에서 **requestTransferAccount** API를 호출하여 계정 이전을 할 수 있습니다.

> <font color="red">[주의]</font><br/>
> TransferAccountInfo의 발급은 게스트 로그인 상태에서만 발급이 가능합니다.
> TransferAccountInfo를 이용한 계정 이전은 게스트 로그인 상태 또는 로그인되어 있지 않은 상태에서만 가능합니다.
> 로그인한 게스트 계정이 이미 다른 외부 IdP (Google, Facebook, Payco 등) 계정과 매핑이 되어 있다면 계정 이전이 지원되지 않습니다.

### Issue TransferAccount
게스트 계정 이전을 위한 TransferAccountInfo를 발급합니다.

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
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

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
이미 발급받은 TransferAccountInfo 정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```cs
static void RenewTransferAccount(GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.TransferAccountInfo> callback)
```

**Example**

```cs
public void RenewTransferAccountManualIdPassword(string accountId, string accountPassword)
{
    // 수동 설정
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountId, accountPassword); // ID + Password
    GamebaseRequest.Auth.TransferAccountRenewConfiguration configuration = GamebaseRequest.Auth.TransferAccountRenewConfiguration.MakeManualRenewConfiguration(accountPassword); // Password

    // 자동 설정  
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
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
> 이미 Guest 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

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
            // Transfering Account failed.
        }
    });
}
```

## Error Handling

| Category | Error                                    | Error Code | Description                                    |
| ---  | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth | INVALID_MEMBER | 6 | 正しくない会員に対するリクエストです。 |
|      | BANNED_MEMBER | 7 | 利用制限対象の会員です。 |
|      | AUTH_USER_CANCELED | 3001 | ログインがキャンセルされました。|
|      | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | この認証方式には対応しておりません。|
|      | AUTH_NOT_EXIST_MEMBER | 3003 | 退会されているか、存在しない会員です。|
|      | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 外部認証ライブラリーエラーです。<br/> DetailCode 및 DetailMessage를 확인해 주세요.|
| TransferKey | SAME\_REQUESTOR | 8 | 발급한 TransferKey를 동일한 기기에서 사용했습니다. |
|             | NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IDP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 |トークンログインに失敗しました。|
|              | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 |トークン情報が有効ではありません。|
|              | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 最近ログインしたIdPの情報がありません。|
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdPログインに失敗しました。|
|           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | マッピング追加に失敗しました。|
|             | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 既に他のメンバーにマッピングされています。 |
|             | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 既に同じIdPにマッピングされています。 |
|             | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP情報が有効ではありません。(Consoleに該当するIdPの情報がありません。) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IDP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | マッピング削除に失敗しました。|
|                | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 最後にマッピングされたIdPは、削除することができません。|
|                | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 現在ログイン中のIdPです。|
| Logout | AUTH_LOGOUT_FAILED | 3501 | ログアウトに失敗しました。|
| Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | 退会に失敗しました。|
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | プレイできない状態です。(メンテナンスまたはサービス終了など) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 不明なエラーです。(定義されていないエラーです。) |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)


**AUTH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、外部認証ライブラリーで発生したエラーです。
* 오류 코드 확인은 다음과 같이 확인하실 수 있습니다.

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* IDP SDK의 오류 코드는 각각의 Developer 페이지를 참고하시기 바랍니다.
