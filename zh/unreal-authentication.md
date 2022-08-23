## Game > Gamebase > Unreal SDK使用指南 > 认证

## Login

Gamebase基本上支持访客登录。<br/>

* 如果登录到访客以外的Provider，则需使用相关Provider AuthAdapter。
* 关于AuthAdapter和3rd-Party Provider SDK的设置，请参考以下指南。
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)

 
### Login Flow

多数游戏在标题页上实现登录。 

* 当App首次安装和启动时，游戏用户可以在标题页选择要进行的IdP(identity provider)类型。
* 登录过一次后，您将不会看到IdP选择画面，将使用之前登录的IdP类型进行认证。 

上述逻辑可以按以下顺序实现。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 按上一次的登录类型认证

* 如果存在已做过认证的记录，则尝试进行认证，不需要输入ID和密码。
* 调用**LoginForLastLoggedInProvider API**。

#### 1-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用**GetUserID API**获取用户ID并实现游戏逻辑。

#### 1-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**LoginForLastLoggedInProvider API**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**FGamebaseBanInfo::From API**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase时，将**FGamebaseConfiguration.enablePopup**和**FGamebaseConfiguration.enableBanPopup**值设置为true，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 因为使用上一次的登录类型认证失败，请进行**"2. 使用指定的IdP进行认证"**。

#### 2. 按指定的IdP认证

* 通过直接指定IdP类型尝试进行认证。 
    * 可以进行认证的类型已在**GamebaseAuthProvider**类中定义。
* 调用**Login(providerName, callback) API**。

#### 2-1. 如果认证成功

* 恭喜！ 认证成功。
* 可以使用**GetUserID API**获取用户ID并实现游戏逻辑。

#### 2-2. 如果认证失败

* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**Login(providerName, callback) API**或稍后再试。
* 禁用游戏用户
    * 由于用户是禁用状态，认证失败，错误代码为**BANNED_MEMBER(7)**。
    * 请使用**FGamebaseBanInfo::From API**确认制裁信息，并告知游戏用户无法进行游戏的原因。
    * 初始化Gamebase时，将**FGamebaseConfiguration.enablePopup**和**FGamebaseConfiguration.enableBanPopup**的值设置为**true**，Gamebase会自动弹出禁用的窗口。
* 其他错误
    * 通知游戏用户发生了错误，并返回到游戏用户可以选择认证IdP类型的页面（通常是标题页面或登录页面）。

### Login as the Latest Login IdP

点击特定IdP的登录按钮时，将执行以下登录API。
如果该登录的令牌已过期，或者令牌认证失败，则返回失败。
此时，必须实现对[该IdP的登录](#login-with-idp)。

**API**

支持的平台
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void LoginForLastLoggedInProvider(const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::LoginForLastLoggedInProvider()
{
    IGamebase::Get().LoginForLastLoggedInProvider(FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("LoginForLastLoggedInProvider succeeded."));
        }
        else
        {
            if (error->code == GamebaseErrorCode::SOCKET_ERROR || error->code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an error message to the user. : %s"), *error->message);
            }
            else if (error->code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto banInfo = FGamebaseBanInfo::From(error);
                if (banInfo.IsValid())
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("This user has been banned. Gamebase userId is %s"), *banInfo->userId);
                }
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Try to login using a specifec IdP"));
            }
        }
    }));
}
```

### Login with GUEST

Gamebase基本上支持访客登录。

* 通过生成设备的特定密钥来尝试Gamebase登录。
* 进行访客登录时，设备密钥可能会被初始化，账号被删除掉，因此推荐使用IdP的登录方式。

**API**

支持的平台
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Login(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);

            if (error->code == GamebaseErrorCode::SOCKET_ERROR || error->code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an error message to the user. : %s"), *error->message);
            }
            else if (error->code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto banInfo = FGamebaseBanInfo::From(error);
                if (banInfo.IsValid())
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("This user has been banned. Gamebase userId is %s"), *banInfo->userId);
                }
            }
        }
    }));
}
```

### Login with IdP

以下是允许您使用特定IdP进行登录的示例代码。
可在**GamebaseAuthProvider**类中查看登录时使用的IdP类型。 

> [参考]
>
> 使用IdP登录时有些IdP需要附加信息。
> 为了设置附加信息提供void Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback) API。
> 将以dictionary格式在additionalInfo参数中输入必要信息即可。 
> 如有additionalInfo值，则使用相应值，而如果为null，则使用[NHN Cloud Console](./oper-app/#authentication-information)中注册的值。

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Login(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
void Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);

            if (error->code == GamebaseErrorCode::SOCKET_ERROR || error->code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an error message to the user. : %s"), *error->message);
            }
            else if (error->code == GamebaseErrorCode::BANNED_MEMBER)
            {
                auto banInfo = FGamebaseBanInfo::From(error);
                if (banInfo.IsValid())
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("This user has been banned. Gamebase userId is %s"), *banInfo->userId);
                }
            }
        }
    }));
}

void Sample::LoginWithAdditionalInfo()
{
    UGamebaseJsonObject* additionalInfo = NewObject<UGamebaseJsonObject>();
    additionalInfo->SetStringField(TEXT("Key"), TEXT("Value"));

    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, *additionalInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Login with Credential

使用IdP提供的SDK在游戏中直接进行认证后，利用已发放的Access Token登录Gamebase界面。

* Credential参数的设置方法

| keyname | a use | 值类型 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::ProviderName | IdP类型设置                           | google、facebook、payco、iosgamecenter、 naver、twitter、line |
| GamebaseAuthProviderCredential::AccessToken | 进行IdP登录后接收的认证信息(Access Token)设置<br/>Google认证时不使用。 |                                |
| GamebaseAuthProviderCredential::AuthorizationCode | Google登录后接收的认证信息(Authorization Code)设置 |                                          |
| GamebaseAuthProviderCredential::GamebaseAccessToken | 通过Gamebase Access Token登录，而不是通过IdP认证信息登录时使用。 |  |
| GamebaseAuthProviderCredential::IgnoreAlreadyLoggedIn | 在Gamebase登录状态下，不注销也允许其他帐户登录尝试。 | **bool** |                             

> [TIP]
>
> 游戏中必须使用外部服务（Facebook等）的固有功能时，您可能需要它。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求支持的开发事项应使用外部SDK的API来实现，Gamebase不支持。
>

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Login(const UGamebaseJsonObject& credentialInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::LoginWithCredential()
{
    UGamebaseJsonObject* credentialInfo = NewObject<UGamebaseJsonObject>();

    // google
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    IGamebase::Get().Login(*credentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```


## Logout
尝试从登录中的IdP退出。通常，在游戏的设置画面有退出登录（退出账号）按钮，然后点击该按钮执行。
即使退出成功，也会保留游戏用户数据。
如果退出成功，将会删除IDP认证记录，则下次登录时将显示ID和密码输入窗口。<br/><br/>

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Logout(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::Logout()
{
    IGamebase::Get().Logout(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Logout succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Logout failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```


## Withdraw

登录后尝试退出。 

* 成功退出时
  * 已登录过的IdP的游戏用户数据将被删除。 
  * 可使用此IdP重新登录。将创建新的游戏用户数据。
  * 用户将从连接的所有IdP注销。 
* 这表示退出Gamebase，而不表示退出IdP账户。

> <font color="red">[注意]</font><br/>
>
> 多个IdP被连接时，所有IdP的连接将解除，而Gamebase用户数据也将被删除。
>

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Withdraw(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::Withdraw()
{
    IGamebase::Get().Withdraw(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Withdraw succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Withdraw failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

## Mapping

映射（Mapping）是将已登录的现有游戏帐号和IdP帐户进行关联或解除关联的功能。

在大多数游戏中，可以将多个IdP帐户映射（Mapping）到同一个游戏账户。
Gamebase的Mapping API，允许将您的其他IdP帐户和您之前登录的游戏帐户进行关联或解除关联操作。<br/>

可以将多个IdP帐户和一个Gamebase用户ID进行关联。
换句话说，当您尝试使用同步的IdP帐户登录时，可以始终使用相同的用户ID登录。<br/>

请注意，每个IdP只能关联一个同域名帐户。
以下是帐户关联的示例。<br/>

* Gamebase用户ID : 123bcabca
    * Google ID : aa
    * Facebook ID : bb
    * AppleGameCenter ID : cc
    * PAYCO ID : dd
* Gamebase用户ID : 456abcabc
    * Google ID : ee
    * Google ID : ff **-> 由于您已关联Google ee帐户，因此无法添加其他Google帐户。**

Mapping中有添加映射和解除映射的功能。

### Add Mapping Flow
 
映射（Mapping）可以按以下顺序实现。

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. 登录
Mapping是为当前帐户添加IdP帐户链接，因此您必须先登录。
首先通过调用登录API登录。

#### 2. 映射

调用**AddMapping API**尝试进行映射。

#### 2-1. 如果映射成功

* 恭喜！ 已成功添加与当前账户关联的IdP帐户。
* 映射成功后，“当前登录的IdP”也不会更改。<br>换句话说，在已使用Google帐户登录后，成功映射到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
* 映射只是添加IdP关联。

#### 2-2. 如果映射失败
   
* 网络错误
    * 由于突发的网络问题，认证失败，错误代码为**SOCKET_ERROR(110)**或**SOCKET_RESPONSE_TIMEOUT(101)**。需要重新调用**AddMapping API**或稍后再试。
* 与其他帐户关联时发生的错误
    * 错误代码为**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)*则表示尝试映射的IdP上的帐户已关联了另一个帐户，要解除已关联的帐户，请使用该帐户登录并通过调用**Withdraw API**退出或者通过调用**RemoveMapping API**解除关联，再尝试映射。
* 关联相同IdP帐户时发生的错误
    * 错误代码为**AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**，则表示已关联了与IDP相同类型的账户。
    * Gamebase映射，每个IdP只能关联一个游戏帐户。例如，已经关联了PAYCO帐户，则无法添加其他PAYCO帐户。
    * 要关联同一IdP中的另一个帐户，请调用**RemoveMapping API**解除关联后，再尝试映射。
* 其他错误
    * 尝试映射失败。

### Add Mapping

在登录特定IdP状态下，尝试用其他IdP Mapping。
要进行映射的IdP账户已与其他账户关联时，
返还**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**错误。<br/>

映射成功后，“当前登录的IdP”也不会更改。换句话说，在已使用Google帐户登录后，成功映射到您的Facebook帐户，也不会将您“当前登录的IdP”从Google更改为Facebook，会保持Google的状态。
映射只是添加IdP关联。

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void AddMapping(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
void AddMapping(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback)
```

**Example**

```cpp
void Sample::AddMapping(const FString& providerName)
{
    IGamebase::Get().AddMapping(providerName, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### AddMapping with Credential

游戏中先直接以IdP提供的SDK进行验证，并可利用发放的Access Token等调用Gamebase AddMapping的接口。

* Credential参数的设置方法

| keyname | a use | 值类型 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | IdP类型                           | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | 进行IdP登录后获取的认证信息(Access Token)<br/>进行Google认证时不使用。|                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Google登录后获取的认证信息(Authorization Code) |                                          |

> [TIP]
>
> 游戏中必须使用外部服务（Facebook等）的固有功能时，您可能需要它。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDK要求支持的开发事项应使用外部SDK的API来实现，Gamebase不支持。
>

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void AddMapping(const UGamebaseJsonObject& credentialInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingWithCredential()
{
    UGamebaseJsonObject* credentialInfo = NewObject<UGamebaseJsonObject>();

    // google
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Google);
    //credentialInfo->SetStringField(GamebaseAuthProviderCredential::AuthorizationCode, TEXT("google auchorization code"));

    // facebook
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::ProviderName, GamebaseAuthProvider::Facebook);
    credentialInfo->SetStringField(GamebaseAuthProviderCredential::AccessToken, TEXT("facebook access token"));

    IGamebase::Get().AddMapping(*credentialInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Add Mapping Forcibly

若特定IdP有已映射的账户，尝试**强制**映射。
尝试强制映射时需要从AddMapping API获得的“ForcingMappingTicket”。

如下为对Facebook尝试强制映射的范例。

**API**

```cpp
void AddMappingForcibly(const FGamebaseForcingMappingTicket& forcingMappingTicket, const FGamebaseAuthTokenDelegate& onCallback);
// Legacy API
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
void AddMappingForcibly(const UGamebaseJsonObject& credentialInfo, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingForcibly(const FString& providerName)
{
    IGamebase::Get().AddMapping(providerName, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // 先通过调用addMapping API并利用已关联的账户尝试Mapping来获取以下ForcingMappingTicket。
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // 利用ForcingMappingTicket类中的From()方法获取ForcingMappingTicket实例。
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // 尝试强制Mapping。
                IGamebase::Get().AddMappingForcibly(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (Gamebase::IsSuccess(error))
                    {
                        // 成功添加强制Mapping。
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // 确认错误代码后进行相应的处理。
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // 确认错误代码后进行相应的处理。 
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```

### Change Login with ForcingMappingTicket

如果已经有一个映射到特定IdP的帐户，注销当前帐户并使用映射的帐户登录。
这时，需要从AddMapping API获取的“ForcingMappingTicket”。

当Change Login API调用失败，将以当前的UserID维持Gamebase登录。 

**API**

```cs
void ChangeLogin(const FGamebaseForcingMappingTicket& forcingMappingTicket, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

下面是一个在尝试映射到Facebook后存在已映射到Facebook的帐户时将登录更改为相应账户的示例。

```cpp
void Sample::ChangeLoginWithFacebook(const FString& providerName)
{
    IGamebase::Get().AddMapping(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)  
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // 先通过调用addMapping API并利用已关联的账户尝试Mapping来获取以下ForcingMappingTicket。
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // 通过利用ForcingMappingTicket类中的From()方法获取ForcingMappingTicket实例。
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                  if (forcingMappingTicket.IsValid())
                  {   
                    // 尝试强制映射。
                    IGamebase::Get().ChangeLogin(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                        FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* authTokenForcibly, const FGamebaseError* innerError)
                    {
                        if (Gamebase::IsSuccess(error))
                        {
                            // 登录更改成功
                        }
                        else
                        {
                            // 登录更改失败
                            // 确认错误代码后进行适当的处理。
                        }
                    }));
                }
                else
                {
                    // Unexpected error occurred. Contact Administrator.
                }
            }
            else
            {
                // 确认错误代码后进行相关处理。
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```
  
### Remove Mapping

解除特定IdP的关联。如果您要解除关联的IdP是唯一的IdP，则会返回失败。
解除关联之后，Gamebase将该IdP退出登录。

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RemoveMapping(const FString& providerName, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RemoveMapping(const FString& providerName)
{
    IGamebase::Get().RemoveMapping(providerName, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RemoveMapping succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RemoveMapping failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Get Mapping List

返还与用户ID相关联的IdP列表。<br/>

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
TArray<FString> GetAuthMappingList() const;
```

**Example**

```cpp
void Sample::GetAuthMappingList()
{
    auto mappingList = IGamebase::Get().GetAuthMappingList();
    
    for (FString provider : mappingList)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetAuthMappingList - %s"), *provider);
    }
}
```

## Gamebase User`s Information

在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

### Get Authentication Information for Gamebase
在使用Gamebase完成认证过程后，制作App时可获取到所需的信息。

#### UserID

可以获取Gamebase发行的认证信息。
**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetUserID() const;
```

**Example**
```cpp
void Sample::GetUserID()
{
    FString userID = IGamebase::Get().GetUserID();
}
```

#### AccessToken

可以获取Gamebase发行的Access Token。

**API**

支持的平台  

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAccessToken() const;
```

**Example**
```cpp
void Sample::GetAccessToken()
{
    FString accessToken = IGamebase::Get().GetAccessToken();
}
```

#### Last LoggedIn Provider Name

可以从Gamebase获取最后登录成功的ProviderName。

**API**

支持的平台

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetLastLoggedInProvider() const;
```

**Example**
```cpp
void Sample::GetLastLoggedInProvider()
{
    FString lastLoggedInProvider = IGamebase::Get().GetLastLoggedInProvider();
}
```

### Get Authentication Information for External IdP

* 登录后可通过游戏服务器调用Gamebase Server API来获取外部认证IdP的Access Token、用户ID及Profile等信息。
    * [Game > Gamebase > API指南 > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[注意]</font><br/>
>
> * 为了安全起见，请通过游戏服务器调用外部IdP的认证信息。 
> * 根据IdP类型，访问令牌可很快过期。
>     * 例如，登录Google后过2小时后Access Token将过期。 
>     * 如需用户信息，登录后直接调用Gamebase Server API。
> * 使用"Gamebase.LoginForLastLoggedInProvider()" API登录时无法获取认证信息。
>     * 如需用户信息，不需要使用"Gamebase.LoginForLastLoggedInProvider()"，而需通过与您要使用的IDPCode相同的{IDP_CODE}作为参数来调用"Gamebase.Login(IDP_CODE, callback)" API进行登录。

### Get Banned User Information

如果在Gamebase Console中登记为受到制裁的游戏用户，当该用户尝试登录时，可能会看到以下限制信息代码(**BANNED_MEMBER(7)**)。
您可以使用**FGamebaseBanInfo::From API**方法，确认制裁信息。


## TransferAccount
是获得将访客账户转移至其他终端机的密钥的功能。

该密钥称为**TransferAccountInfo**。
获得的TransferAccountInfo可从其他终端机调用**requestTransferAccount**API，并转移账户。

> <font color="red">[注意]</font><br/>
> TransferAccountInfo仅在访客登录状态下可获得。
> 使用TransferAccountInfo的账户转移仅可在访客登录状态或未登录状态下实现。
> 若登录的访客账户已与其他外部IdP (Google、Facebook、PAYCO等)账户映射，则不支持账户转移。

### Issue TransferAccount
为转移访客账户，发放TransferAccountInfo。  

**API**

```cpp
void IssueTransferAccount(const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::IssueTransferAccount()
{
    IGamebase::Get().IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Issuing TransferAccount success.
        }
        else
        {
            // Issuing TransferAccount failed.
        }
    }));
}
```

### Query TransferAccount
为转移访客账户，向Gamebase服务器查询已获得的TransferAccountInfo信息。

**API**

```cpp
void QueryTransferAccount(const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryTransferAccount()
{
    IGamebase::Get().IssueTransferAccount(FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Querying TransferAccount success.
        }
        else
        {
            // Querying TransferAccount failed.
        }
    }));
}
```

### Renew TransferAccount
更新已获得的TransferAccountInfo信息。
更新方法有“自动更新”与“手动更新”，可通过仅更新Password、同时更新ID与Password等的设置更新TransferAccountInfo信息。

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& configuration, const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::RenewTransferAccount(const FString& accountId, const FString& accountPassword)
{
    // 手动设置
    FGamebaseTransferAccountRenewConfiguration configuration{ accountId, accountPassword };
    //FGamebaseTransferAccountRenewConfiguration configuration{ accountPassword };

    // 自动设置
    //FGamebaseTransferAccountRenewConfiguration configuration{ type };

    IGamebase::Get().RenewTransferAccount(configuration, FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Renewing TransferAccount success.
        }
        else
        {
            // Renewing TransferAccount failed.
        }
    }));
}
```


### Transfer Guest Account to Another Device
是从**issueTransfer API**获得TransferAccount后，转移账户的功能。
账户转移成功时获得TransferAccount的终端机可显示转移完成信息，访客登录时创建新的账户。
在账户转移成功的终端机中可继续使用获得TransferAccount的终端机的账户信息。

> <font color="red">[注意]</font><br/>
> 若以访客登录的状态转移账户，访客账户将丢失。
 
**API**

```cpp
void TransferAccountWithIdPLogin(const FString& accountId, const FString& accountPassword, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::TransferAccountWithIdPLogin(const FString& accountId, const FString& accountPassword)
{
    IGamebase::Get().TransferAccountWithIdPLogin(accountId, accountPassword, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Transfering Account success.
            // TODO: implements post login process
        }
        else
        {
            // Transfering Account failed.
        }
    }));
}
```


## TemporaryWithdrawal

为“预约退出”功能。
由于请求了临时退出，不立即退出，预约时期过后退出。
可以在控制台中修改预约时期。

> <font color="red">[注意]</font><br/>
>
> 如果使用预约退出功能，不应调用**Withdraw API**。
> 通过调用**Withdraw API**API，可立即退出。

如果登录成功，您可通过调用AuthToken.member.temporaryWithdrawal，确认用户是否在预约退出状态。

### Request TemporaryWithdrawal

请求临时退出。
在控制台中指定的预约时间过后，自动完成退出处理。

**API**

```cpp
void RequestWithdrawal(const FGamebaseTemporaryWithdrawalDelegate& onCallback);
```

**Example**

```cpp
void Sample::RequestWithdrawal()
{
    IGamebase::Get().GetTemporaryWithdrawal().RequestWithdrawal(FGamebaseTemporaryWithdrawalDelegate::CreateLambda([=](const FGamebaseTemporaryWithdrawalInfo* info, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestWithdrawal succeeded. The date when you can withdraw your withdrawal is %d"), info->gracePeriodDate);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestWithdrawal failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Check TemporaryWithdrawal User

如果用户登录使用预约退出功能的游戏，您需要调用AuthToken.member.temporaryWithdrawal，若返还的结果不是null，则需通知用户正在进行退出处理。

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            if (authToken->member.temporaryWithdrawal.gracePeriodDate > 0)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User is under temporary withdrawa. GracePeriodDate : %d"), authToken->member.temporaryWithdrawal.gracePeriodDate);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Cancel TemporaryWithdrawal

取消退出请求。
若预约退出时间到期，则无法退出。 

**API**

```cpp
void CancelWithdrawal(const FGamebaseErrorDelegate& onCallback);
```
**Example**

```cpp
void Sample::CancelTemporaryWithdrawal()
{
    IGamebase::Get().GetTemporaryWithdrawal().CancelTemporaryWithdrawal(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("CancelTemporaryWithdrawal succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("CancelTemporaryWithdrawal failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

### Withdraw Immediately

无论预约退出时期的设置状态如何，都立即退出。
实际的内置启动与Withdraw API相同。

无法取消立即退出，因此要反复提示用户是否继续执行。

**API**

```cpp
void WithdrawImmediately(const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::WithdrawImmediately()
{
    IGamebase::Get().GetTemporaryWithdrawal().WithdrawImmediately(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("WithdrawImmediately succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("WithdrawImmediately failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}
```

## GraceBan

* 是“结算Abusing自动解除”功能。
    * 结算Abusing自动解除功能是当存在需通过“结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 处于“禁用预约”状态时，若在设定的时期内满足所有的解除条件则可玩游戏。
    * 如果在所定的时期内未能满足条件，则将禁止使用。
* 如果是使用结算Abusing自动解除功能的游戏，每当登录时要调用AuthToken.member.graceBanInfo API。若返还结果为有效的GraceBanInfo对象，而不为null，则需通知用户解除禁用的条件和时期等。
    * 如需禁止处于禁用预约状态的用户进入游戏，则需在游戏中进行处理。

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error) == false)
        {
            // Login failed
            return;
        }
        
        // Check if user is under grace ban
        GamebaseResponse.Common.Member.GraceBanInfo graceBanInfo = authToken->member.graceBan;
        if (graceBanInfo != null)
        {
            string periodDate = string.Format("{0:yyyy/MM/dd HH:mm:ss}", 
                new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(graceBanInfo.gracePeriodDate));
            string message = graceBanInfo.message;
            
            GamebaseResponse.Common.Member.GraceBanInfo.ReleaseRuleCondition releaseRuleCondition = graceBanInfo.releaseRuleCondition;
            if (releaseRuleCondition != null)
            {
                // condition type : "AND", "OR"
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
    }));
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 是对错误会员的请求。                        |
|                | BANNED\_MEMBER                           | 7          | 是禁用用户。                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | 登录被取消。                            |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | 是不支持的认证方式。                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 是不存在或退出的会员。                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 外部认证库初始化失败 |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部认证库错误 <br/> 请确认DetailCode和DetailMessage。  |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | 尚未完成上一个认证程序。 |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | 您已注销，因为Gamebase Access Token无效。<br/>请再尝试登录。 |   
| TransferAccount| SAME\_REQUESTOR                          | 8          | 在同一个终端机使用了TransferAccount。|
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 使用访客账户之外的账户尝试了转移，或账户与访客之外的IdP关联。|
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount的有效期已过。|
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 因连续输入错误的TransferAccount，账户转移功能被锁定。|
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount的ID无效。|
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount的Password无效。|
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | 未设置TransferAccount。<br/> 请先在TOAST Gamebase Console中进行设置。|
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount不存在。请先获取TransferAccount。|
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount已经存在。|
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | 已使用TransferAccount。|
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | 令牌登录失败 |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 令牌信息无效 |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 没有最近登录的IdP信息。|
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP登录失败 |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP信息无效(在Console中没有相关IdP信息) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 添加映射失败 |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 已经映射到其他成员。|
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 已经映射到同样的IdP。|
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP信息无效(在Console中没有相关IdP信息) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | 使用Guest IdP无法进行AddMapping。|
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 强制映射密钥(ForcingMappingKey)不存在。<br/>请再确认ForcingMappingTicket。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 已经使用强制映射密钥(ForcingMappingKey)。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 强制映射密钥(ForcingMappingKey)已过期。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 强制映射密钥(ForcingMappingKey)用于其他IdP。<br/>获取的ForcingMappingKey将用于尝试强制映射同样的IdP。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 强制映射密钥(ForcingMappingKey)用于其他账户。<br/>获取的ForcingMappingKey将用于尝试强制映射同样的IdP和账户。|
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 删除映射失败 |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 不能删除最后映射的IdP。|
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 是已登录的IdP。|
| Logout | AUTH_LOGOUT_FAILED | 3501 | 注销失败 |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退出失败                              |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 用户已临时退出。                   |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 用户未临时退出。                    |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 无法玩游戏的状态(正在维护中或终止服务等) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 未知错误(未定义的错误) |

* 关于错误的代码，请参考如下文档。
    * [错误代码](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* 是在外部认证库中发生的错误。
* 确认错误代码的方式如下。

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    if (error->code == GamebaseErrorCode::AUTH_EXTERNAL_LIBRARY_ERROR)
    {
        FGamebaseErrorInner moduleError = error->error;
        if (moduleError.code != GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError.code, *moduleError.message);
        }
    }
}
```

* 关于IdP SDK的错误代码，请参考相应Developer页面。
