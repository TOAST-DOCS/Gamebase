## Game > Gamebase > User Guide for Unreal SDK > Authentication 

## Login

Gamebase basically supports guest login. <br/>

* To login to a provider other than guests, AuthAdapter for each provider is required.  
* Regarding setting for AuthAdapter and 3rd-Party Provider SDK, see the following: 
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

Many games allow login execution on the title page.  

* On the initial execution after app is installed, game users are allowed to select IdP (Identity Provider) on the title page. 
* After initial login, the IdP option page does not show again and it is authenticated with the previous login IdP type. 

The above login can be implemented in the following order: 

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_001_2.6.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_002_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_003_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_004_1.10.0.png)
![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_flow_005_1.10.0.png)

#### 1. Authenticate with Previous Login Type 

* If a previous record exists for authentication, authentication can be tried without ID or password. 
* Call **LoginForLastLoggedInProvider API**. 

#### 1-1. When Authentication is Successful 

* Congratuations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic. 

#### 1-2. When Authentication Failed 

* Network Error 
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network issue, so call  **LoginForLastLoggedInProvider API** again or try again in a moment. 
* Banned Game User
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **GetBanInfo API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set true for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Erros 
    * Authentication failed with the previous login type. Execute **'3. Authenticate with Specified IdP'**.

#### 2. Authenticate with Specified IdP

* Specify IdP type to authenticate.  
    * Available types of authentication are declared in the **GamebaseAuthProvider** class. 
* Call **Login(providerName, callback) API**.

#### 2-1. When Authentication is Successful 

* Congratuations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic.

#### 2-2. When Authentication Failed

* Network Error
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network, so call **Login(providerName, callback) API** again, or try again in a moment. 
* Banned Game User 
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **GetBanInfo API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set **True** for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Errors 
    * Notify game users of the occurrence of error, and it is reverted to a status (mostly, title or login page) in which game user can select an IdP type of authetication.   

### Login with the Latest Login IdP

Attempt to log in with the most recent login IdP. 
When token for the login has expired, or if token verification has failed, return such failure.  
To that end, it is required to implement [Login for IdP](#login-with-idp).

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("LoginForLastLoggedInProvider succeeded."));
        }
        else
        {
            if (error->code == GamebaseErrorCode::SOCKET_ERROR || error->code == GamebaseErrorCode::SOCKET_RESPONSE_TIMEOUT)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("Retry LoginForLastLoggedInProvider or notify an error message to the user. : %s"), *error->message);
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

Gamebase supports guest login. 
Create a unique key of device to try a login to Gamebase. 
With guest login, device key might be initialized, which may cause your account to be deleted; therefore, it is recommended to use IdP for a login. 

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

### Login with IdP

Following codes are login examples with particular IdP. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Login(const FString& providerName, const FGamebaseAuthTokenDelegate& onCallback);
void Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**providerName**

| Provider    | Define                          | Support Platform | 
| --------    | ------------------------------- | ---------------- |
| Google      | GamebaseAuthProvider::Google     | Android<br/>iOS |
| Game Center | GamebaseAuthProvider::GameCenter | iOS |
| Apple ID    | GamebaseAuthProvider::AppleId    | iOS |
| Facebook    | GamebaseAuthProvider::Facebook   | Android<br/>iOS |
| Payco       | GamebaseAuthProvider::Payco      | Android<br/>iOS |
| Naver       | GamebaseAuthProvider::Naver      | Android<br/>iOS |
| Twitter     | GamebaseAuthProvider::Twitter    | Android<br/>iOS |
| Line        | GamebaseAuthProvider::Line       | Android<br/>iOS |


> Some IdP logins require particular information. <br/>
> For instance, to log in to Facebook, the setting for scope is needed. <br/>
> To configure such required information, API for static void Login (string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) is provided. <br/>
> You may enter information for the additionalInfo parameter in the format of dictionary.  
When there's available value for additionalInfo, use it; otherwise (null), apply default value registered on TOAST Console.   
([Setting additionalInfo for TOAST Console](./oper-app/#authentication-information))<br/>
> Standalone supports a login via WebViewAdapter and does not block event input via UIs when WebView is open. 

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
        }
    }));
}

void Sample::LoginWithAdditionalInfo()
{
    UGamebaseJsonObject* additionalInfo = NewObject<UGamebaseJsonObject>();
    additionalInfo->SetStringField(TEXT("Key"), TEXT("Value"));

    IGamebase::Get().Login(GamebaseAuthProvider::Facebook, *additionalInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

This interface allows login to Gamebase with SDKs provided by IdP and authenticated in each game, by using access token.  

* Setting Credential Parameters 

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type                          | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (e.g. access token) given after IdP login <br/> Disabled for Google authentication |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Set authorization code given after Google login |                                          |

> [TIP]
>
> Might be required to use unique features of an external service (e.g. Facebook) for a game.  
>


> <font color="red">[Caution]</font><br/>
>
> Development issues requiring the support of an external SDK must be implemented by using API of such SDK, which is not supported by Gamebase.
>

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

### Authentication of Additional Information Settings

[Console Guide](./oper-app/#authentication-information)

## Logout
Attempt to log out from login IdP. In most cases, the logout button is located on the setting page of a game, to be clicked for execution. 
Even with a successful logout, game user's data remains. 
When it is successfully logged out, authentication records with IdP are removed, and therefore, popup will show to enter ID and password for the next login attempt. <br/><br/>

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
Attempt to withdraw while it is logged in. 

* When it is successfully withdrawn, game user's data integrated with logged IdP will be removed. 
* It is available to log in again with the IdP, which creates data of a new game user.   
* Withdrawing from Gamebase does not mean withdrwaing from an IdP account. 
* When it is successfully withdrawn, logout from IdP is attempted. 

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

Mapping refers to integrating or disintegrating other IdP accounts with a previously logged-in account. 

In many games, one account is allowed to be mapped with many IdPs. 
With Mapping API of Gamebase, accounts of other IdP can be mapped/unmapped with previously logged-in account. <br/>

As such, one Gamebase user ID can be mapped with many IdP accounts. 
That is, to log in with mapped IdP account, it is logged in with same user ID at all times. <br/>

Note, however, that each IdP allows only one account for a mapping. 
See the following for example: <br/>

* Gamebase User ID : 123bcabca
	* Google ID : aa
	* Facebook ID : bb
	* AppleGameCenter ID : cc
	* Payco ID : dd
* Gamebase 사용자 ID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> Unable to map further Google account, since the Google ee account is already mapped.**

Regarding mapping, two APIs exist: Add/Cancel Mapping API.  

### Add Mapping Flow

Mapping can be implemented in the following order. 

#### 1. Login 
Since mapping refers to adding IdP account integration to a current account, login is required. 
Call login API first to log in. 

#### 2. Mapping 

Call **AddMapping API** to attempt a mapping. 

#### 2-1. When Mapping is Successful

* Congratulations! Just added an IdP account now mapped with the current account. 
* Even with a successful mapping, 'Currently logged IdP' does not change. <br> For instance, if you've been logged with Google account, and even if an attempt of mapping with Facebook has been successful, the 'Currently logged IdP' does not change from Google to Facebook.
* Mapping simply adds IdP integration. 

#### 2-2. When Mapping Fails 

* Network Error
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network issue, so call **AddMapping API** again or try again in a moment. 
* Error from Mapping to Another Account
    * When the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, the IdP account about to map is already mapped to another account. To disintegrate such account, login to the account and call **Withdraw API** to withdraw, or call **RemoveMapping API** to cancel mapping and try a new mapping again.  
* Error from Mapping to Same IdP Account 
	* When the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, it means the same type of account with an IdP about to map is already mapped.  
	* Gamebase mapping allows mapping of only one account from each IdP. For example, if an account is already mapped to a PAYCO account, no further PAYCO account can be added.  
	* To map to another account of same IdP, call **RemoveMapping API** to cancel mapping and try a new mapping again. 
* Other Errors 
    * An attempt to map has failed. 


### Add Mapping

Attempt to map with another IdP while it is logged in to a specific IdP. 
If an account of IdP about to mapp is already mapped to another account, 
return the error of **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**.<br/>

Even with a successful mapping, 'Currently logged IdP' does not change. For instance, if you've been logged with Google account, and even if an attempt of mapping with Facebook has been successful, the 'Currently logged IdP' does not change from Google to Facebook.
Mapping simply adds IdP integration.

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

This interface allows to authenticate with SDK provided by IdP of a game to enable Gamebase AddMapping by using a given access token.  

* Setting Credential Parameters  

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type                            | google, facebook, payco, iosgamecenter, naver, twitter, line, appleid |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (e.g. access token) given after IdP login <br/> Disabled for Google authentication |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Set authorization code given after Google login  |                                          |

> [TIP]
>
> Might be required to use unique features of an external service (e.g. Facebook) for a game.  
>


> <font color="red">[Caution]</font><br/>
>
> Development issues requiring the support of an external SDK must be implemented by using API of such SDK, which is not supported by Gamebase.
>

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

When there's an account which is already mapped to a particular IdP, attempt to **Force Mapping**. 
To attempt a **Force Mapping**, you need `ForcingMappingTicket` acquired from AddMpping API. 

See the following example for the attempt of force mapping: 

**API**

```cpp
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingForcibly(const FString& providerName)
{
    IGamebase::Get().AddMapping(providerName, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // Call addMapping API and map to an account already integrated, and get ForcingMappingTicket, like below.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // Use the From() method of the ForcingMappingTicket class to get an ForcingMappingTicket instance. 
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // Attempt to force mapping. 
                IGamebase::Get().AddMappingForcibly(providerName, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
                    {
                        // Successfully added force mapping
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // Check error code and take an appropriate processing. 
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // Check error code and take an appropriate processing. 
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```


### Add Mapping Forcibly with Credential
When there's an account which is already mapped to a particular IdP, attempt to **Force Mapping**. 
To attempt a **Force Mapping**, you need `ForcingMappingTicket` acquired from AddMpping API. 

The interface enables to authenticate with SDK provided by game IdP to call Gamebase AddMappingForcibly, by using given access token. 

* Setting Credential Parameters 

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential.PROVIDER_NAME | Set IdP type                           | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential.ACCESS_TOKEN | Set authentication information (e.g. access token) given after IdP login <br/> Disabled for Google authentication |                                |
| GamebaseAuthProviderCredential.AUTHORIZATION_CODE | Set authorization code given after Google login |                                        |

> [TIP]
>
> Might be required to use unique features of an external service (e.g. Facebook) for a game. 
>


> <font color="red">[Caution]</font><br/>
>
> Development issues requiring the support of an external SDK must be implemented by using API of such SDK, which is not supported by Gamebase.
>

See the following example of attempting force mapping. 

**API**

```cpp
void AddMappingForcibly(const UGamebaseJsonObject& credentialInfo, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

```cpp
void Sample::AddMappingForcibly()
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
        else
        {
            // Call addMapping API and map to an account already integrated, and get ForcingMappingTicket, like below.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // Use the From() method of the ForcingMappingTicket class to get an ForcingMappingTicket instance.
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // Attempt to force mapping. 
                IGamebase::Get().AddMappingForcibly(*credentialInfo, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
                    {
                        // Successfully added force mapping 
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // Check error code and take an appropriate processing. 
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // Check error code and take an appropriate processing. 
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```


### Remove Mapping

Remove mapping from a particular IdP. If there's only one IdP, return failure. 
After mapping is removed, log out from the IdP within Gamebase. 

**API**

Supported Platforms
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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

Return the list of IdPs mapped with user ID. <br/>

**API**

Supported Platforms
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

Execute authentication via Gamebase, to obtain information required to produce an app. 를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

### Get Authentication Information for Gamebase
Execute authentication via Gamebase, to obtain information required to produce an app. 를 통하여 인증절차를 진행 후, 앱을 제작할 때 필요한 정보를 획득할 수 있습니다.

#### UserID

Get UserID issued from Gamebase. 
**API**

Supported Platforms
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

Get access token issued from Gamebase.

**API**

Supported Platforms
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

Get ProviderName that was successful with the last login from Gamebase.

**API**

Supported Platforms
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

Get authentication information, such as access token, User ID, and profile from externally authenticated SDK. 

#### UserID

Get user ID from externally authenticated SDK. 외부 인증 SDK에서 사용자 ID를 가져올 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAuthProviderUserID(const FString& providerName) const;
```

**Example**

```cpp
void Sample::GetLastLoggedInProvider(const FString& providerName)z
{
    FString authProviderUserID = IGamebase::Get().GetAuthProviderUserID(GetProviderName(providerName));
}
```

#### AccessToken

Get access token from externally authenticated SDK.  

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetAuthProviderAccessToken(const FString& providerName) const;
```

**Example**
```cpp
void Sample::GetAuthProviderAccessToken(const FString& providerName)
{
    FString authProviderAccessToken = IGamebase::Get().GetAuthProviderAccessToken(providerName);
}
```

#### Profile

Get profile from externally authenticated SDK. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
const FGamebaseAuthProviderProfilePtr GetAuthProviderProfile(const FString& providerName) const;
```

**Example**
```cpp
void Sample::GetAuthProviderProfile(const FString& providerName)
{
    auto authProviderProfile = IGamebase::Get().GetAuthProviderProfile(providerName);
    if (authProviderProfile.IsValid())
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetAuthProviderProfile: %s"), *authProviderProfile->information->EncodeJson());
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not auth provider profile."));
    }
}
```

### Get Banned User Infomation

When a user is registered as banned from Gamebase Console, 
with his login attempt, servic restriction code (**BANNED_MEMBER(7)**) may be displayed, and punishment information can be found by using the following API: 아래 API를 이용하여 제재 정보를 확인할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
const FGambaseBanInfoPtr GetBanInfo() const;
```

**Example**
```cpp
void Sample::GetBanInfo()
{
    auto banInfo = IGamebase::Get().GetBanInfo();
    if (banInfo.IsValid())
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("GetBanInfo: %s"), *banInfo->userId);
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found ban info."));
    }
}
```

## TransferAccount
With this feature, key to transfer guest account onto another device can be issued.  

The key is called **TransferAccountInfo**.
The issued TransferAccountInfo enables account transfer by calling **requestTransferAccount** API from another device. 

> <font color="red">[Caution]</font><br/>
> TransferAccountInfo can be issued only under guest login.  
> Account transfer by using TransferAccountInfo is available only with a guest login or logout. 
> If a login guest account is already mapped with another external IdP (e.g. Google, Facebook, or Payco), account transfer is not supported. 

### Issue TransferAccount
Issue TransferAccountInfo to transfer account. 

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
게스트 계정 이전을 위해 이미 발급받은 TransferAccountInfo 정보를 게임베이스 서버에 질의합니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
Renew 이미 발급받은 TransferAccountInfo which has already been issued.  정보를 갱신합니다.
"자동 갱신", "수동 갱신"의 방법이 있으며, "Password만 갱신", "ID와 Password 모두 갱신" 등의 설정을 통해
TransferAccountInfo 정보를 갱신 할 수 있습니다.

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& configuration, const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::RenewTransferAccount(const FString& accountId, const FString& accountPassword)
{
    // 수동 설정
    FGamebaseTransferAccountRenewConfiguration configuration{ accountId, accountPassword };
    //FGamebaseTransferAccountRenewConfiguration configuration{ accountPassword };

    // 자동 설정
    //FGamebaseTransferAccountRenewConfiguration configuration{ type };

    IGamebase::Get().RenewTransferAccount(configuration, FGamebaseTransferAccountDelegate::CreateLambda([=](const FGamebaseTransferAccountInfo* transferAccountInfo, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
**issueTransfer** API로 발급받은 TransferAccount를 통해 계정을 이전하는 기능입니다.
계정 이전 성공 시 TransferAccount를 발급받은 단말기에서 이전 완료 메시지가 표시될 수 있고, Guest 로그인 시 새로운 계정이 생성됩니다.
계정 이전이 성공한 단말기에서는 TransferAccount를 발급받았던 단말기의 게스트 계정을 계속해서 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
> 이미 게스트 로그인이 되어 있는 상태에서 이전이 성공하게 되면, 단말기에 로그인되어 있던 게스트 계정은 유실됩니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

'탈퇴 유예' 기능입니다.
임시 탈퇴를 요청하여 즉시 탈퇴가 진행되지 않고 일정 기간의 유예 기간이 지나면 탈퇴가 이루어집니다.
유예 기간은 콘솔에서 변경할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 탈퇴 유예 기능을 사용하는 경우에는 **Withdraw API** 를 사용하지 마세요.
> **Withdraw API**는 즉시 계정을 탈퇴합니다.

로그인이 성공하면 AuthToken.member.temporaryWithdrawal로 탈퇴 유예 상태인 유저인지 판단할 수 있습니다.

### Request TemporaryWithdrawal

임시 탈퇴를 요청합니다.
콘솔에 지정된 기간이 지나면 자동으로 탈퇴 진행이 완료됩니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

탈퇴 유예를 사용하는 게임은 로그인 후 항상 AuthToken.member.temporaryWithdrawal가 null이 아니라면 해당 유저에게 탈퇴 진행중이라는 사실을 알려주어야 합니다.

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

탈퇴를 요청을 취소합니다.
탈퇴 요청 후 기간이 만료되어 탈퇴가 완료되면 취소가 불가능합니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

탈퇴 유예 기간을 무시하고 즉시 탈퇴를 진행합니다.
실제 내부 동작은 Withdraw API 와 동일합니다.

즉시 탈퇴는 취소가 불가능하므로 유저에게 실행 여부를 거듭 확인하시기 바랍니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

## Error Handling

| Category | Error | Error Code | Description |
| --- | --- | --- | --- |
| Auth | INVALID_MEMBER | 6 | Requested for invalid member.  잘못된 회원에 대한 요청입니다. |
|  | BANNED_MEMBER | 7 | The member has been banned. 제재된 회원입니다. |
|  | AUTH_USER_CANCELED | 3001 | Cancelled login. 로그인이 취소되었습니다. |
|  | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | The authentication method is not supported. 지원하지 않는 인증 방식입니다. |
|  | AUTH_NOT_EXIST_MEMBER | 3003 | The member does not exist or has withdrawn. 존재하지 않거나 탈퇴한 회원입니다. |
|  | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | Error of external authentication library. 외부 인증 라이브러리 오류입니다. <br/> Check DetailCode and DetailMessage.  |
|  | AUTH_ALREADY_IN_PROGRESS_ERROR | 3010 | 이전 인증 프로세스가 완료되지 않았습니다.
| TransferAccount| SAME\_REQUESTOR                          | 8          | 발급한 TransferAccount를 동일한 단말기에서 사용했습니다. |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount의 유효기간이 만료됐습니다. |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount의 ID가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount의 Password가 유효하지 않습니다. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount 설정이 되어있지 않습니다. <br/> TOAST Gamebase Console에서 먼저 설정해주세요. |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount가 이미 존재합니다. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount가 이미 사용되었습니다. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | 토큰 로그인에 실패하였습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | 토큰 정보가 유효하지 않습니다. |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 최근에 로그인한 IdP 정보가 없습니다. |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP 로그인에 실패하였습니다. |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | 맵핑 추가에 실패하였습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | 이미 다른 멤버에 맵핑되어있습니다. |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | 이미 같은 IdP에 맵핑되어있습니다. |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP 정보가 유효하지 않습니다. (Console에 해당 IdP 정보가 없습니다.) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 강제매핑키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 강제매핑키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 강제매핑키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 강제매핑키(ForcingMappingKey)가 다른 IDP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 강제매핑키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | 맵핑 삭제에 실패하였습니다. |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 마지막에 맵핑된 IdP는 삭제할 수 없습니다. |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 현재 로그인 되어 있는 IdP 입니다. |
| Logout | AUTH_LOGOUT_FAILED | 3501 | 로그아웃에 실패하였습니다. |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 탈퇴에 실패했습니다.                              |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴중인 유저 입니다.                    |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴중인 유저가 아닙니다.                     |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | 플레이할 수 없는 상태입니다. (점검 또는 서비스 종료 등) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 알수 없는 에러입니다. (정의 되지 않은 에러입니다.) |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* The error occurs from externally authenticated library. 
* Check error codes like below: 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* Please see each developer's page for error codes of IdP SDK. 
