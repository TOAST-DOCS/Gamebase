## Game > Gamebase > Unity SDK 使用指南 > 认证

## Login

Gamebase默认支持访客登录。<br/>

* 使用游客以外的Provider登录，需要Provider AuthAdapter。
* AuthAdapter和第三方提供的SDK设置，请参考以下内容。
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

多数游戏在标题页上实现登录。

* 当App首次安装和启动时，游戏用户可以在标题页选择要进行的IdP(identity provider)类型。
* 登录过一次后，您将不会看到IdP选择画面，将使用之前登录的IdP类型进行认证。

上述逻辑可以按以下顺序实现。

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_006_1.10.0.png)

#### 1. 按上一次的登录类型认证

* 如果存在已做过的认证的记录，则尝试进行认证，不需要输入ID和密码。
* 调用**Gamebase.LoginForLastLoggedInProvider()**。

#### 1-1. 如果认证成功

* 恭喜您！ 认证成功。
* 可以使用**Gamebase.GetUserID()**取用户ID并实现游戏逻辑。

#### 1-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**Gamebase.LoginForLastLoggedInProvider()**，或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**Gamebase.GetBanInfo()**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时将**GamebaseConfiguration.enablePopup**和**GamebaseConfiguration.enableBanPopup** 的值设置为**true**，Gamebase会自动弹出禁用窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**'3. 使用指定的IdP进行认证**。

#### 2. 使用指定的Idp进行认证

* 通过直接指定IdP类型来尝试进行认证。
    * 可以认证的类型在**GamebaseAuthProvider**类中声明。
* 调用**Gamebase.Login(providerName, callback)** API。

#### 2-1. 如果认证成功

* 恭喜您！ 认证成功。
* 可以使用**Gamebase.GetUserID()**获取用户ID并实现游戏逻辑。

#### 2-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**，需要重新调用**Gamebase.Login(providerName, callback)**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**Gamebase.GetBanInfo()**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase 时将**GamebaseConfiguration.enablePopup** 和**GamebaseConfiguration.enableBanPopup**的值设置为**true**，Gamebase会自动弹出禁用窗口。
* 其他错误
    * 通知游戏用户发生了错误，并返回到游戏用户可以选择认证IdP类型的页面（通常是标题页面或登录页面）。

### Login as the Latest Login IdP

尝试使用最近登录的IdP登录。
如果该登录的令牌已过期，或者令牌认证失败，则返回失败。
此时，须实现[对该IdP的登录](#login-with-idp) 。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void LoginForLastLoggedInProvider(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**示例**

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

Gamebase支持游客登录。
尝试通过为设备创建的唯一密钥来登录Gamebase。
建议基于IdP的登录方式，因为游客登录方式，在删除App或设备初始化时，账户可能会被删除。

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

**示例**

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

以下是允许您使用特定IdP登录的示例代码。

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


> 个别IdP登录，需要一些特定信息。<br/>
> 例如，要实现Facebook登录，您需要设置scope等。<br/>
> 为了设置这些信息，提供了static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API。<br/>
> 可以用dictionary格式把信息输入到参数additionalInfo中。
当参数值为nil时，它将填充在TOAST Console中注册的additionalInfo值。如果参数值存在，则覆盖在Console中注册的值。
([在TOAST控制台中设置additionalInfo](./oper-app/#authentication-information))<br/>
> Stansalone支持通过WebViewAdapter登录，并且在WebView打开时，不会阻止在UI中输入的Event。


要是用Standalone WebViewAdapter登录，您需要在IdP开发者网站上设置以下 CallbackURL。

* https://alpha-id-gamebase.toast.com/oauth/callback
* https://beta-id-gamebase.toast.com/oauth/callback
* https://id-gamebase.toast.com/oauth/callback

**示例**

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

是通过IdP提供的SDK在游戏中进行认证后，使用获取到的访问令牌，登录到Gamebase的接口。

* Credential参数设置方法

| keyname | a use | 值类型 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | 设定IdP类型                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | 设置登录IdP后收到的认证信息（访问令牌）<br/>不用于Google认证 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | 输入登录Google后可以获取的认证码(Authorization Code)  |                                          |

> [TIP]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

UnityEditor只支持Facebook登录。

```cs
static void Login(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**示例**

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
尝试从登录中的IdP退出。 通常，在游戏的设置画面有退出登录（退出账号）按钮，然后点击该按钮执行。
即使退出登录成功，也会保留游戏用户数据。
如果退出登录成功，将会删除IDP认证记录，则下次登录时将显示ID和密码输入窗口。<br/><br/>

以下是点击“退出登录”按钮时的示例代码。

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

**示例**

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
在登录状态尝试退出（删除数据）。

* 如果退出（删除数据）成功，则将删除与登录的IdP账户相关联的游戏用户数据。
* 您可以使用该IdP重新登录并生成新的游戏用户数据。
* 这意味着退出Gamebase，并不是退出IdP帐户。
* 成功退出（删除数据）时，也将退出IdP登录。

以下是游戏用户登录状态下，“退出（删除数据）”的示例代码。

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

**示例**

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

映射（Mapping）是将已登录的现有游戏帐号和IdP帐户关联或解除关联的功能。

在大多数游戏中，可以将多个IdP帐户映射（Mapping）到同一个游戏账户。
Gamebase的Mapping API，允许将您的其他IdP帐户和您之前登录的游戏帐户进行关联或解除关联操作。<br/>

利用此操作可以将多个IdP账户关联到一个Gamebase用户ID。
换句话说，当您尝试使用同步的IdP帐户登录时，可以始终使用相同的用户ID登录。<br/>

请注意，每个IdP只能关联一个同域名帐户。
以下是帐户关联的示例。<br/>

* Gamebase用户ID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* Gamebase用户ID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

映射（Mapping）包括添加Mapping/解除Mapping API。

### Add Mapping Flow

映射（Mapping）可以按以下顺序实现。

#### 1. 登录
Mapping是为当前帐户添加IdP帐户链接，因此您必须先登录。
首先通过调用登录API登录。

#### 2. 映射（Mapping）

调用**Gamebase.AddMapping()**尝试进行映射（Mapping）。

#### 2-1. 如果映射（Mapping）成功

* 恭喜您! 已成功添加与当前账户关联的IdP帐户。
* 映射（Mapping）成功后，“当前登录的IdP”也不会更改。<br>换句话说，在已使用Google帐户登录后，成功映射（Mapping）到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
* 映射（Mapping）只是添加IdP关联。

#### 2-2. 如果映射（Mapping）失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)** 或**SOCKET_RESPONSE_TIMEOUT(101)**，则重新调用**Gamebase.AddMapping()**或稍后再试。
* 与其他账户关联时发生的错误
    * 错误代码为 **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**，则表示尝试映射（Mapping）的IdP上的帐户已关联了另一个帐户，要解除已关联的帐户，请使用该帐户登录并通过调用**Gamebase.Withdraw()**退出（删除数据），或者通过调用**Gamebase.RemoveMapping()**解除关联，再尝试映射（Mapping）。
* 关联相同IdP帐户发生的错误
	* 错误代码为 **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**，则表示已关联了与IDP相同类型的账户。
	* Gamebase映射（Mapping），每个IdP只能关联一个游戏帐户。例如，已经关联了PAYCO帐户，则无法添加其他PAYCO帐户。
	* 要关联同一IdP中的另一个帐户，请调用**Gamebase.RemoveMapping()**，解除关联后，再尝试映射（Mapping）。
* 其他错误
    * 尝试映射（Mapping）失败。


### Add Mapping

登录特定IdP状态下，尝试用其他IdP Mapping。
如果想要映射（Mapping）的IdP账户已与其他账户关联，则返还**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**错误。<br/>

映射（Mapping）成功后，“当前登录的IdP”也不会更改。<br>换句话说，在已使用Google帐户登录后，成功映射（Mapping）到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
映射（Mapping）只是添加IdP关联。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**示例**

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

游戏中直接使用ID Provider提供的SDK进行认证后，使用发行的访问令牌，进行GameBase AddMapping的接口。


* Credential参数设置方法

| keyname | a use | 值类型 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | 设定IdP类型                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | 设置登录IdP后收到的认证信息（访问令牌）<br/>不用于Google认证 |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | 输入登录Google 后可以获取的认证码(Authorization Code) |                                          |

> [TIP]
>
> 当要在游戏中使用外部服务（例如Facebook）的特有功能时，可能需要它。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求的开发事项需使用外部SDK的API实现，Gamebase不支持。
>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void AddMapping(Dictionary<string, object> credentialInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**示例**

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

解除特定IDP的关联。如果尝试解除当前登录中的帐户，则会失败。
解除关联之后，Gamebase将该IdP退出登录。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RemoveMapping(string providerName, GamebaseCallback.ErrorDelegate callback)
```

**示例**

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

获取与用户ID关联的IdP列表。<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static List<string> GetAuthMappingList()
```

**示例**

```cs
public void GetAuthMappingList()
{
    List<string> mappingList = Gamebase.GetAuthMappingList();
}
```
## Gamebase User`s Information

在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

### Get Authentication Information for Gamebase
在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

#### UserID

可以获取Gamebase发行的UserID。
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

**示例**
```cs
public void GetUserID()
{
    string userID = Gamebase.GetUserID();
}
```

#### AccessToken

可以获取Gamebase发放的访问令牌。

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

**示例**
```cs
public void GetAccessToken()
{
    string accessToken = Gamebase.GetAccessToken();
}
```

#### Last LoggedIn Provider Name

可以获取在Gamebase最后一次登录成功的ProviderName。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetLastLoggedInProvider()
```

**示例**
```cs
public void GetLastLoggedInProvider()
{
    string lastLoggedInProvider = Gamebase.GetLastLoggedInProvider();
}
```

### Get Authentication Information for External IdP

从外部认证SDK，可获取访问令牌、用户ID、Profile等信息。

#### UserID

可以从外部认证SDK获取用户ID。

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

**示例**

```cs
public void GetAuthProviderUserID(string providerName)
{
    string authProviderUserID = Gamebase.GetAuthProviderUserID(providerName);
}
```

#### AccessToken

可以从外部认证SDK获取访问令牌。

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

**示例**
```cs
public void GetAuthProviderAccessToken(string providerName)
{
    string authProviderAccessToken = Gamebase.GetAuthProviderAccessToken(providerName);
}
```

#### Profile

可以从外部认证SDK获取Profile。

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

**示例**
```cs
public void GetAuthProviderProfile(string providerName)
{
    GamebaseRequest.AuthProviderProfile profile = Gamebase.GetAuthProviderProfile(providerName);
}
```

### Get Banned User Infomation

如果在Gamebase Console中登记为受到制裁的游戏用户，当该用户尝试登录时，
可能会看到以下限制信息代码。您可以使用(**BANNED_MEMBER(7)**) 方法，确认制裁信息。


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

**示例**
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

| Category | Error | Error Code | Description |
| --- | --- | --- | --- |
| Auth | INVALID_MEMBER | 6 | 无效的用户请求。 |
|  | BANNED_MEMBER | 7 | 被制裁的用户。 |
|  | AUTH_USER_CANCELED | 3001 | 登录信息已被取消。 |
|  | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | 不支持的认证方式。 |
|  | AUTH_NOT_EXIST_MEMBER | 3003 | 不存在或已退出（删除数据）的用户。 |
|  | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | 外部认证库错误。 <br/> 请确认DetailCode和DetailMessage。  |
| TransferKey | SAME\_REQUESTOR | 8 | 在同一台设备上使用了相同的TransferKey。 |
|  | NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | 非游客帐户尝试了转移或帐户已关联了游客以外的IDP。 |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | 令牌登录失败。 |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 无效的令牌信息。 |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 无近期登录的IdP信息。 |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP登录失败。 |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | 无效的 IdP信息。 (Console中没有此IdP信息。) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 添加映射（Mapping）失败。 |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 已与其他账户映射（Mapping）。 |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 已映射（Mapping）到相同的IdP。 |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | 无效的IdP信息（Console中没有此IdP信息）。 |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IDP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 解除映射（Mapping）失败。 |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 无法解除最后映射（Mapping）的IdP。 |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 当前登录的IdP。 |
| Logout | AUTH_LOGOUT_FAILED | 3501 | 退出登录失败。 |
| Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | 退出（删除数据）失败。 |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 无法玩游戏的状态(维护或已下线等)。 |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 未知错误(未定义的错误)。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)


**AUTH_EXTERNAL_LIBRARY_ERROR**

* 该错误为各IdP的SDK中发生的错误。
* 确认错误代码方式如下。

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

* IdP SDK的错误代码，请参考相应Developer页面。
