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

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. Authenticate with Previous Login Type 

* If a previous record exists for authentication, authentication can be tried without ID or password. 
* Call **LoginForLastLoggedInProvider API**. 

#### 1-1. When Authentication is Successful 

* Congratulations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic. 

#### 1-2. When Authentication Fails

* Network Error 
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network issue, so call  **LoginForLastLoggedInProvider API** again or try again in a moment. 
* Banned Game User
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **FGamebaseBanInfo::From API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set true for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Errors 
    * Authentication failed with the previous login type. Execute **'2. Authenticate with Specified IdP'**.

#### 2. Authenticate with Specified IdP

* Specify IdP type to authenticate.  
    * Available types of authentication are declared in the **GamebaseAuthProvider** class. 
* Call **Login(providerName, callback) API**.

#### 2-1. When Authentication is Successful 

* Congratulations! Successfully authenticated.
* Get a user ID with **GetUserID API** and implement game logic.

#### 2-2. When Authentication Fails

* Network Error
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, authentication has failed due to temporary network, so call **Login(providerName, callback) API** again, or try again in a moment. 
* Banned Game User 
    * When the error code is **BANNED_MEMBER(7)**, authentication has failed since the game user is banned.  
    * Check information with **FGamebaseBanInfo::From API** to notify game user of why game play is unavailable. 
    * For Gamebase initialization, set **True** for **FGamebaseConfiguration.enablePopup** and **FGamebaseConfiguration.enableBanPopup**, and Gamebase automatically loads the popup on the ban. 
* Other Errors 
    * Notify game users of the occurrence of error, and it is reverted to a status (mostly, title or login page) in which game user can select an IdP type of authentication.   

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

Gamebase supports guest login. 

* Create a unique key of device to try a login to Gamebase. 
* With guest login, device key might be initialized, which may cause your account to be deleted; therefore, it is recommended to use IdP for a login. 

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

Following codes are login examples with particular IdP. 
The types of IdPs that can be used for login can be found in the **GamebaseAuthProvider** class.

> [Note]
>
> Some IdPs require additional information for login.
> To set these additional information, void Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback) API is provided.
> You can input required information in the additionalInfo parameter in the form of a dictionary.
> If additionalInfo is available, it is used. If it is null, the value registered in [NHN Cloud Console](./oper-app/#authentication-information) is used.

**API**

Supported Platforms

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

This interface allows login to Gamebase with SDKs provided by IdP and authenticated in each game, by using access token.  

* Setting Credential Parameters 

| Keyname | Usage | Value Type |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::ProviderName | Set IdP type                          | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential::AccessToken | Set authentication information (e.g. access token) given after IdP login <br/> Disabled for Google authentication |                                |
| GamebaseAuthProviderCredential::AuthorizationCode | Set authorization code given after Google login |                                          |
| GamebaseAuthProviderCredential::GamebaseAccessToken | Used when logging in with Gamebase Access Token instead of IdP authentication information |  |
| GamebaseAuthProviderCredential::IgnoreAlreadyLoggedIn | Allow login attempts with another account without logging out while logged in to Gamebase | **bool** |

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
Attempts account withdrawal while logged in.

* Upon successful withdrawal
  * The game user’s data of the IdP logged in will be deleted.
  * You can login again with the IdP. A new game user’s data will be created.
  * All linked IdPs will be logged out.
* It means user's withdrawal from Gamebase, not from IdP account.

> <font color="red">[Caution]</font><br/>
>
> If multiple IdPs are linked, all IdP linkages will be unlinked and the user data in Gamebase will be deleted.
>

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
* Gamebase User ID : 456abcabc
	* Google ID : ee
	* Google ID : ff **-> Unable to map further Google account, since the Google ee account is already mapped.**

Regarding mapping, two APIs exist: Add/Cancel Mapping API.  

### Add Mapping Flow

Mapping can be implemented in the following order. 

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

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

* Network error
    * When the error code is **SOCKET_ERROR(110)** or **SOCKET_RESPONSE_TIMEOUT(101)**, it means that the authentication failed due to temporary network issues, so call **AddMapping API** again or try again later. 
* Error that occurs when already linked to another account
    * When the error code is **AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**, it means that the account of the IdP to map to is already linked to another account. To disintegrate such account, login to the account and call **Withdraw API** to withdraw, or call **RemoveMapping API** to cancel mapping and try a new mapping again.  
* Error that occurs when already linked to the same IdP account
	* When the error code is **AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**, it means that an account of the same type as the IdP you want to map to is already linked.
	* Gamebase mapping allows linking of only one account per IdP. For example, if an account is already linked to a PAYCO account, no other PAYCO account can be added.  
	* To link another account of the same IdP, call **RemoveMapping API** to remove the linking and try mapping again.
* Other errors 
    * The mapping attempt has failed.


### Add Mapping

Attempt to map with another IdP while it is logged in to a specific IdP. 
If an account of IdP about to map to is already mapped to another account, 
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

When there's an account which is already mapped to a particular IdP, attempt to **Force Mapping**. 
To attempt a **Force Mapping**, you need `ForcingMappingTicket` acquired from AddMpping API. 

See the following example for the attempt of force mapping: 

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
            // Call addMapping API and map to an account already integrated, and get ForcingMappingTicket, like below.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // Use the From() method of the ForcingMappingTicket class to get an ForcingMappingTicket instance. 
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // Attempt force mapping. 
                IGamebase::Get().AddMappingForcibly(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (Gamebase::IsSuccess(error))
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

### Change Login with ForcingMappingTicket

When there's an account which is already mapped to a particular IdP, log out from the current account and log in with the mapped account. 
In this case, `ForcingMappingTicket` acquired from AddMapping API is required. 

If the Change Login API call fails, the Gamebase login status is maintained with the existing UserID.

**API**

```cs
void ChangeLogin(const FGamebaseForcingMappingTicket& forcingMappingTicket, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

The following example shows a situation where, after attempting to map to Facebook, an account already mapped to Facebook is found and the login is changed to the account.

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
            // Call addMapping API and map to an account already integrated, and get ForcingMappingTicket, like below.
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // Use the From() method of the ForcingMappingTicket class to get an ForcingMappingTicket instance.
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid())
                {   
                    // Attempt force mapping.
                    IGamebase::Get().ChangeLogin(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                        FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* authTokenForcibly, const FGamebaseError* innerError)
                    {
                        if (Gamebase::IsSuccess(error))
                        {
                            // Successfully changed login
                        }
                        else
                        {
                            // Failed to change login
                            // Check the error code and resolve the error.
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
                // Check the error code and resolve the error.
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

Execute authentication via Gamebase, to obtain information required to produce an app. 

### Get Authentication Information for Gamebase

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

* Information such as access token, user ID, and profile of an external authentication IdP can be received by calling the Gamebase Server API after login.
    * [Game > Gamebase > API Guide > Authentication > Get IdP Token and Profiles](./api-guide/#get-idp-token-and-profiles)

> <font color="red">[Caution]</font><br/>
>
> * For security reasons, it is recommended that you call the authentication information of an external IdP from the game server.
* Access token may expire relatively sooner depending on the IdP.
>     * For example, the access token of Google will expire within 2 hours from the time of login.
>     * If you need user information, it is recommended that you call the Gamebase Server API immediately after login.
> * When logged in with the "Gamebase.LoginForLastLoggedInProvider()” API, the authentication info cannot be retrieved.
>     * If you need the user info, instead of "Gamebase.LoginForLastLoggedInProvider()” , use the {IDP_CODE} identical to the IDPCode that you want to use as the parameter to log in as the "Gamebase.Login(IDP_CODE, callback)" API.

### Get Banned User Information

When a user is registered as banned from Gamebase Console, 
with his login attempt, servic restriction code (**BANNED_MEMBER(7)**) may be displayed, and punishment information can be found by using the **FGamebaseBanInfo::From API**. 


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
Query TransferAccountInfo, already issued for the transfer of guest account, to a Gamebase server. 

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
Renew TransferAccountInfo which has already been issued.  
It is available either by "Auto Renewal", or "Manual Renewal", and TransferAccountInfo can be updated by setting "Renew Password Only", or "Renew Both ID and Password". 

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& configuration, const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::RenewTransferAccount(const FString& accountId, const FString& accountPassword)
{
    // Manual Setting
    FGamebaseTransferAccountRenewConfiguration configuration{ accountId, accountPassword };
    //FGamebaseTransferAccountRenewConfiguration configuration{ accountPassword };

    // Auto Setting
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
Account can be transferred via TransferAccount with the **issueTransfer** API. 
When account transfer is successful, transfer completion message may be displayed on the device that issued TransferAccount, and a new account will be created with a guest login.  
On a device with successful account transfer, you can continue to apply the guest account of device which issued TransferAccount.  

> <font color="red">[Caution]</font><br/>
> If transfer is successful while guest is logged in, the guest account logged in the device will be lost. 

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

'Suspension of Withdrawal' is available. 
At the request for a temporary withdrawal, user can withdraw after certain period of suspension. 
Suspension period can be changed from the console.

> <font color="red">[Caution]</font><br/>
>
> To suspend withdrawal, do not use **Withdraw API**.
> **Withdraw API** regards to immediate withdrawal from account. 

When it is successfully logged in, the withdrawal status of a user can be found with AuthToken.member.temporaryWithdrawal.

### Request TemporaryWithdrawal

Send a request for a temporary withdrawal. 
After a certain period specified on console, user withdrawal is automatically processed.  

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

Games enabling the suspension of withdrawal must always notify the user after login that withdrawal is underway, unless AuthToken.member.temporaryWithdrawal is null. 

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

Cancel request for a withdrawal.
After withdrawal is completed and if the request has expired, withdrawal cannot be reverted.  

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

Withdraw immediately without considering the suspension period. 
It runs the same as Withdraw API. 

Since immediate withdrawal cannot be cancelled, it is recommended to be confirmed by the user several times.  

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

* This is a 'purchase abuse automatic release' function.
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always check the value of AuthToken.member.graceBanInfo API after login. If a valid GraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.

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

| Category | Error | Error Code | Description |
| --- | --- | --- | --- |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | A request for invalid member.                        |
|                | BANNED\_MEMBER                           | 7          | The member has been banned.                               |
|                | AUTH\_USER\_CANCELED                     | 3001       | Tje login has been cancelled.                            |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | The authentication method is not supported.                        |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | The member does not exist or has withdrawn.                      |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | Failed to initialize an external authentication library. |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | Error occurred in the external authentication library. <br/> Check DetailCode and DetailMessage.  |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | The previous authentication process has not been completed. |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | Logged out because the Gamebase Access Token is not valid.<br/>Please try logging in again. |
| TransferAccount| SAME\_REQUESTOR                          | 8          | Used TransferAccount on a same device.  |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | Attempted to transfer on a non-guest account, or mapped a non-guest IdP to account.  |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccount has been expired.  |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | Account transfer is locked due to many inputs of invalid TransferAccount. |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccount ID is invalid. |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccount password is invalid. |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccount is not set up. <br/> Please enable it first on NHN Cloud Gamebase Console.  |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccount does not exist. Please get TransferAccount issued.  |
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccount already exists. |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccount has already been used. |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | Token login failed.  |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | Token information is invalid.  |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | There's no last login IdP information.  |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdP login has failed.   |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP information is invalid. (IdP information is unavialable on console.) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | Failed to add mapping. |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | Mapped to another member. |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | Already mapped to same IdP. |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP information is invalid (IdP information is unavailable on console).  |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | AddMapping is unavailable with Guest IdP.  |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | ForcingMappingKey does not exist. <br/> Check ForcingMappingTicket again. |
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | ForcingMappingKey has already been used.  |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | ForcingMappingKey is expired.  |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | ForcingMappingKey has been used for another IdP. <br/> The ForcingMappingKey is applied for the attempt of force mapping to a same IdP.  |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | ForcingMappingKey has been used for another account.  <br/> The ForcingMappingKey is applied for the attempt of force mapping to a same IdP and account.  |
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | Failed to delete mapping  |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | Unable to delete the last mapped IdP. |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | The IdP is currently logged-in.  |
| Logout | AUTH_LOGOUT_FAILED | 3501 | Failed to log out. |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | Failed to withdraw.                             |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | The user is already under temporary withdrawal.                   |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | The user is not under temporary withdrawal.                    |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | Unavailable to play game (e.g. due to maintenance or service closed.)  |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | Unknown error (undefined error). |

* Please see the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* The error occurs from external authentication library. 
* Check error codes like below: 

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

* Please see each developer's page for error codes of IdP SDK. 
