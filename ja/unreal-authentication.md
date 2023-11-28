## Game > Gamebase > Unreal SDK使用ガイド > 認証

## Login

Gamebaseではゲストログインをデフォルトでサポートします。<br/>

* ゲスト以外のProviderにログインするには該当Provider AuthAdapterが必要です。
* AuthAdapterおよび3rd-Party Provider SDKの設定は、次を参照してください。
    * [3rd-Party Provider SDK Guide](aos-started#3rd-party-provider-sdk-guide)


### Login Flow

多くのゲームがタイトル画面でログインを実装します。

* アプリをインストールして最初に実行した時、タイトル画面でゲームユーザーがどのIdP(identity provider)で認証するかを選択できるようにします。
* 一度ログインすればIdP選択画面を表示せず、以前にログインしたIdPタイプで認証します。

上で説明したロジックは、次のような順序で実装できます。

![last provider login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/login_for_last_logged_in_provider_flow_2.19.0.png)
![idp login flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/idp_login_flow_2.19.0.png)

#### 1. 以前のログインタイプで認証

* 以前に認証した記録があれば、IDとパスワードを入力しないで認証を試行します。
* **LoginForLastLoggedInProvider API**を呼び出します。

#### 1-1. 認証が成功した場合

* 認証に成功しました。
* **GetUserID API**でユーザーIDを取得し、ゲームロジックを実装してください。

#### 1-2. 認証が失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証が失敗したということです。**LoginForLastLoggedInProvider API**を再度呼び出すか、しばらくしてからもう一度試行します。
* 利用停止ゲームユーザー
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **FGamebaseBanInfo::From API**で制裁情報を確認してゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebase初期化時、**FGamebaseConfiguration.enablePopup**および**FGamebaseConfiguration.enableBanPopup**値をtrueにすると、Gamebaseが利用停止に関するポップアップを自動的に表示します。
* その他のエラー
    * 以前のログインタイプで認証できませんでした。**「2. 指定されたIdPで認証」**を行います。

#### 2. 指定されたIdPで認証

* IdPタイプを直接指定して認証を試行します。
    * 認証可能なタイプは**GamebaseAuthProvider**クラスに宣言されています。
* **Login(providerName, callback) API**を呼び出します。

#### 2-1. 認証に成功した場合

* 認証に成功しました。
* **GetUserID API**でユーザーIDを取得し、ゲームロジックを実装してください。

#### 2-2. 認証に失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証に失敗したということです。**Login(providerName, callback) API**を再度呼び出すか、しばらくしてからもう一度試行します。
* 利用停止ゲームユーザー
    * エラーコードが**BANNED_MEMBER(7)**の場合、利用停止ゲームユーザーのため認証に失敗したということです。
    * **FGamebaseBanInfo::From API**で制裁情報を確認してゲームユーザーにゲームをプレイできない理由を伝えてください。
    * Gamebase初期化時、**FGamebaseConfiguration.enablePopup**および**FGamebaseConfiguration.enableBanPopup**値を**true**にすると、Gamebaseが利用停止に関するポップアップを自動的に表示します。
* その他のエラー
    * エラーが発生したことをゲームユーザーに伝え、ゲームが認証IdPタイプを選択できる状態(主にタイトル画面またはログイン画面)へ戻ります。

### Login as the Latest Login IdP

最近ログインしたIdPでログインを試行します。
該当ログインに対するトークンの有効期限が切れているか、トークンの検証などに失敗すると、失敗を返します。
この場合は[該当IdPに対するログイン](#login-with-idp)を実装する必要があります。

* AdditionalInfoパラメータの設定方法

| keyname                                  | a use                                    | 値の種類                                    |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential.SHOW_LOADING_ANIMATION | API呼び出しが終わるまでローディングアニメーションを表示<br>**Androidに限る** | **bool**<br>**default**: true |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void LoginForLastLoggedInProvider(const FGamebaseAuthTokenDelegate& onCallback);
void LoginForLastLoggedInProvider(const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
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

Gamebaseはゲストログインをサポートします。

* デバイスの唯一のキーを作成してGamebaseにログインを試行します。
* ゲストログインは、デバイスキーが初期化されることがあり、デバイスキーの初期化時にアカウントが削除される場合があるため、IdPを活用したログイン方式を推奨します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS
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

次は、特定IdPでログインできるようにするサンプルコードです。
ログインすることができるIdPタイプは **GamebaseAuthProvider**クラスで確認できます。

> [参考]
>
> ログインする時に追加情報を必要とするIdPもあります。
> このような追加情報を設定できるようにvoid Login(const FString& providerName, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback) APIを提供します。
>additionalInfoパラメータに必須情報をdictionary形式で入力してください。
>additionalInfo値がある場合にはその値を使用し、nullの場合には[NHN Cloud Console](./oper-app/#authentication-information)に登録された値を使用します。

> [参考]
>
> Line IdPはGamebase SDK 2.43.0からLineサービス提供地域を設定できます。
> 該当地域はAdditionalInfoに設定できます。 
* additionalInfoパラメータの設定方法

| keyname                                  | a use                                    | 値種類                                   |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| GamebaseAuthProviderCredential::LineChannelRegion | Lineサービス提供地域設定 | "japan"<br/>"thailand"<br/>"taiwan"<br/>"indonesia" |

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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
            UE_LOG(GamebaseTestResults, Display, TEXT("Login failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);\

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

IdPで提供するSDKを使用して、ゲームで直接認証した後、発行されたアクセストークンなどを利用して、Gamebaseにログインできるインターフェイスです。

* Credentialパラメータの設定方法

| keyname | a use | 値種類 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::ProviderName | IdPタイプ設定                         | google, facebook, payco, iosgamecenter, naver, twitter, line |
| GamebaseAuthProviderCredential::AccessToken | IdPログイン後に取得した認証情報(Access Token)設定<br/>Google認証時には使用しない |  
| GamebaseAuthProviderCredential::AuthorizationCode | Googleログイン後に取得した認証情報(Authorization Code)設定 |                                          |
| GamebaseAuthProviderCredential::GamebaseAccessToken | IdP認証情報ではなくGamebase Access Tokenでログインを行いたい場合に使用 |  |
| GamebaseAuthProviderCredential::LineChannelRegion | Lineサービス提供地域設定 | [Login with IdP参照](./unreal-authentication/#login-with-idp) |

> [TIP]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用する必要がある時に必要な場合があります。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDKでサポートを要求する開発事項は外部SDKのAPIを使用して実装する必要があり、Gamebaseではサポートしません。
>

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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
ログインしたIdPからログアウトを試行します。主にゲームの設定画面にログアウトボタンを設置し、ボタンをクリックすると実行されるように実装する場合が多いです。
ログアウトが成功しても、ゲームユーザーデータは維持されます。
ログアウトに成功すると、該当IdPで認証した記録を消去するため、次にログインする時、ID、パスワード入力ウィンドウが表示されます。<br/><br/>

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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

ログイン状態で退会を試行します。

* 退会成功時
  * ログインしていたIdPのゲーム利用者データは削除されます。
  * 該当IdPで再度ログインできます。その場合は新しいゲーム利用者データを作成します。
  * 連携中のすべてのIdPがログアウト処理されます。
* Gamebaseの退会を意味し、IdPアカウントを退会するわけではありません。

> <font color="red">[注意]</font><br/>
>
> 複数のIdPを連携中の場合、すべてのIdP連携が解除され、Gamebaseユーザーデータが削除されます。
>

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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

マッピングは、既にログインしているアカウントに他のIdPのアカウントを連携または、解除する機能です。

多くのゲームが1つのアカウントに複数のIdPを連携(Mapping)できるようにしています。
GamebaseのMapping APIを使用して既にログインしているアカウントに他のIdPのアカウントを連携/解除できます。<br/>

このように、1つのGamebaseユーザーIDに多くのIdPアカウントを連携できます。
すなわち、連携中のIdPアカウントでログインを試行すると、常に同じユーザーIDでログインします。<br/>

注意する点は、IdPごとに1つのアカウントのみ連携が可能なことです。
例は次のとおりです。<br/>

* GamebaseユーザーID：123bcabca
	* Google ID：aa
	* Facebook ID：bb
	* AppleGameCenter ID：cc
	* Payco ID：dd
* GamebaseユーザーID：456abcabc
	* Google ID：ee
	* Google ID：ff**-> すでにGoogle eeアカウントが連携中のため、Googleアカウントをさらに連携できません。**

MappingにはMapping追加/解除APIがあります。

### Add Mapping Flow

マッピングは、次の順序で実装できます。

![add mapping flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/auth_add_mapping_flow_2.30.0.png)

#### 1. ログイン
マッピングは、現在のアカウントにIdPアカウント連携を追加することです。優先的にログインする必要があります。
先にログインAPIを呼び出してログインします。

#### 2. マッピング

**AddMapping API**を呼び出してマッピングを試行します。

#### 2-1. マッピングが成功した場合

* 現在のアカウントと連携中のIdPアカウントが追加されました。
* マッピングに成功しても「現在ログイン中のIdP」は変わりません。<br>すなわち、Googleアカウントでログインした後、Facebookアカウントマッピングの試行が成功したからといって、「現在ログイン中のIdP」がGoogleからFacebookには変更されません。Googleの状態で維持されます。
* マッピングは単純にIdPの連携のみ追加します。

#### 2-2. マッピングが失敗した場合

* ネットワークエラー
    * エラーコードが**SOCKET_ERROR(110)**または**SOCKET_RESPONSE_TIMEOUT(101)**の場合、一時的なネットワークの問題で認証が失敗したということです。**AddMapping API**を再度呼び出すか、しばらくしてから再試行します。
* すでに他のアカウントに連携中の時に発生するエラー
    * エラーコードが**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**の場合、マッピングしようとするIdPのアカウントが、すでに他のアカウントに連携中という意味です。連携中のアカウントを解除するには、該当アカウントでログインして**Withdraw API**を呼び出して退会するか、**RemoveMapping API**を呼び出して連携を解除した後、再度マッピングを試行してください。
* すでに同じIdPアカウントに連携していて発生するエラー
	* エラーコードが**AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP(3303)**の場合、マッピングしようとするIdPと同じ種類のアカウントがすでに連携中という意味です。
	* Gamebaseマッピングは、1つのIdPにつき1つのアカウントのみ連携可能です。例えば、PAYCOアカウントにすでに連携している場合、それ以上PAYCOアカウントを追加できません。
	* 同じIdPの他のアカウントを連携するには、**RemoveMapping API**を呼び出して連携を解除した後、再度マッピングを試行してください。
* その他のエラー
    * マッピングの試行が失敗しました。


### Add Mapping

特定IdPにログインした状態で、他のIdPにMappingを試行します。
MappingをしようとするIdPのアカウントがすでに他のアカウントに連携している場合、
**AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)**エラーを返します。<br/>

Mappingが成功しても、「現在ログイン中のIdP」が変わりません。すなわち、Googleアカウントでログインした後、FacebookアカウントのMappingが成功したからといって、「現在ログイン中のIdP」がGoogleからFacebookには変更されません。Googleの状態で維持されます。
Mappingは、単純にIdP連携のみ追加します。

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

ゲームで直接IdPから提供するSDKで先に認証を行い、発行されたアクセストークンなどを利用して、Gamebase AddMappingを行うことができるインターフェイスです。

* Credentialパラメータの設定方法

| keyname | a use | 値種類 |
| ---------------------------------------- | ------------------------------------ | ------------------------------ |
| GamebaseAuthProviderCredential::PROVIDER_NAME | IdPタイプ設定 | GamebaseAuthProvider::Google<br> GamebaseAuthProvider::Facebook<br>GamebaseAuthProvider::Naver<br>GamebaseAuthProvider::Twitter<br>GamebaseAuthProvider::Line<br>GamebaseAuthProvider::Hangame<br>GamebaseAuthProvider::AppleId<br>GamebaseAuthProvider::Weibo<br>GamebaseAuthProvider::GameCenter<br>GamebaseAuthProvider::Payco |
| GamebaseAuthProviderCredential::ACCESS_TOKEN | IdPログイン後に取得した認証情報(アクセストークン)設定<br/>Google認証時には使用しない |                                |
| GamebaseAuthProviderCredential::AUTHORIZATION_CODE | Googleログイン後に取得した認証情報(Authorization Code)設定 |    

> [TIP]
>
> ゲーム内で外部サービス(Facebookなど)の固有機能を使用する必要がある時に必要な場合があります。
>


> <font color="red">[注意]</font><br/>
>
> 外部SDKでサポートを要求する開発事項は、外部SDKのAPIを使用して実装する必要があり、Gamebaseではサポートしません。
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

特定IdPにすでにマッピングされているアカウントがある時、**強制的に**マッピングを試行します。
**強制マッピング**を試行する時は、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

次は、Facebookに強制マッピングを試行する例です。

**API**

```cpp
void AddMappingForcibly(const FGamebaseForcingMappingTicket& forcingMappingTicket, const FGamebaseAuthTokenDelegate& onCallback);

// Legacy API
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const FGamebaseAuthTokenDelegate& onCallback);
void AddMappingForcibly(const FString& providerName, const FString& forcingMappingKey, const UGamebaseJsonObject& additionalInfo, const FGamebaseAuthTokenDelegate& onCallback);
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
            // まず、addMapping APIを呼び出し、すでに連携されているアカウントでマッピングを試行し、次のようにForcingMappingTicketを取得できます。
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicketクラスのFrom()メソッドを利用してForcingMappingTicketインスタンスを取得します。
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid() == false)
                {
                    // Unexpected error occurred. Contact Administrator.
                }
                
                // 強制マッピングを試行します。
                IGamebase::Get().AddMappingForcibly(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                    FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* innerAuthToken, const FGamebaseError* innerError)
                {
                    if (Gamebase::IsSuccess(error))
                    {
                        // 強制マッピング追加成功
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly succeeded."));
                    }
                    else
                    {
                        // エラーコードを確認し、適切な処理を行います。
                        UE_LOG(GamebaseTestResults, Display, TEXT("AddMappingForcibly failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);
                    }
                }));
            }
            else
            {
                // エラーコードを確認し、適切な処理を行います。
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```


### Change Login with ForcingMappingTicket

特定IdPにすでにマッピングされているアカウントがある場合、現在のアカウントからログアウトし、マッピングされたアカウントにログインします。
このとき、AddMapping APIで取得した`ForcingMappingTicket`が必要です。

Change Login APIの呼び出しが失敗した場合、 Gamebaseログイン状態は既存のUserIDのままです。

**API**

```cs
void ChangeLogin(const FGamebaseForcingMappingTicket& forcingMappingTicket, const FGamebaseAuthTokenDelegate& onCallback);
```

**Example**

次はFacebookにマッピング試行後、Facebookにすでにマッピングされたアカウントが存在し、該当アカウントにログインを変更する例です。

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
            // まずAddMapping APIの呼び出しと、すでに連動されているアカウントにマッピングを試行して、次のようにForcingMappingTicketを取得できます。
            if (error->code == GamebaseErrorCode::AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER)
            {
                // ForcingMappingTicketクラスのFrom()メソッドを利用してForcingMappingTicketインスタンスを取得します。
                auto forcingMappingTicket = FGamebaseForcingMappingTicket::From(error);
                if (forcingMappingTicket.IsValid())
                {   
                    // 強制マッピングを試行します。
                    IGamebase::Get().ChangeLogin(forcingMappingTicket, forcingMappingTicket->forcingMappingKey,
                        FGamebaseAuthTokenDelegate::CreateLambda([](const FGamebaseAuthToken* authTokenForcibly, const FGamebaseError* innerError)
                    {
                        if (Gamebase::IsSuccess(error))
                        {
                            // ログイン変更成功
                        }
                        else
                        {
                            // ログイン変更失敗
                            // エラーコードを確認し、適切な処理を行います。
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
                // エラーコードを確認し、適切な処理を行います。
                UE_LOG(GamebaseTestResults, Display, TEXT("AddMapping failed."));
            }
        }
    }));
}
```

### Remove Mapping

特定IdPの連携を解除します。もし解除するIdが唯一のIdPの場合、失敗を返します。
連携解除後は、Gamebase内部で該当IdPのログアウト処理を行います。

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

ユーザーIDに連携されているIdPリストを返します。<br/>

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

Gamebaseを通して認証手順を進行した後、アプリを製作する時に必要な情報を取得できます。

### Get Authentication Information for Gamebase

#### UserID

Gamebaseで発行したUserIDを取得できます。
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

Gamebaseで発行したアクセストークンを取得できます。

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

Gamebaseで最後にログインに成功したProviderNameを取得できます。

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

### Get Banned User Infomation

Gamebase Consoleに制裁中のゲームユーザーで登録する場合、
ログイン試行時、利用制限情報コード(**BANNED_MEMBER(7)**)が表示される場合があり、**FGamebaseBanInfo::From API**を利用して制裁情報を確認できます。


## TransferAccount
ゲストアカウントを他の端末へ移行するためにアカウント移行用のキーを発行する機能です。

このキーを**TransferAccountInfo**と呼びます。
発行されたTransferAccountInfoは、他の端末で**requestTransferAccount**APIを呼び出してアカウント移行を行えます。

> <font color="red">[注意]</font><br/>
> TransferAccountInfoは、ゲストログイン状態でのみ発行できます。
> TransferAccountInfoを利用したアカウント移行は、ゲストログイン状態またはログインしていない状態でのみ可能です。
> ログインしたゲストアカウントがすでに他の外部IdP (Google、Facebook、Paycoなど)アカウントとマッピングされている場合、アカウント移行がサポートされません。

### Issue TransferAccount
ゲストアカウント移行のためのTransferAccountInfoを発行します。

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
ゲストアカウント移行のためにすでに発行したTransferAccountInfo情報をGamebaseサーバーに問い合わせます。

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
発行されたTransferAccountInfo情報を更新します。
方法は「自動更新」と「手動更新」があり、「Passwordのみ更新」、「IDとPasswordを更新」などの設定をしてTransferAccountInfo情報を更新できます。

```cpp
void RenewTransferAccount(const FGamebaseTransferAccountRenewConfiguration& configuration, const FGamebaseTransferAccountDelegate& onCallback);
```

**Example**

```cpp
void Sample::RenewTransferAccount(const FString& accountId, const FString& accountPassword)
{
    // 手動設定
    FGamebaseTransferAccountRenewConfiguration configuration{ accountId, accountPassword };
    //FGamebaseTransferAccountRenewConfiguration configuration{ accountPassword };

    // 自動設定
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
**issueTransfer**APIで発行されたTransferAccountでアカウントを移行する機能です。
アカウントの移行に成功すると、TransferAccountを発行した端末で移行完了メッセージが表示される場合があり、Guestログイン時に新しいアカウントが作成されます。
アカウント移行が成功した端末では、TransferAccountを発行した端末のゲストアカウントを継続して使用できます。

> <font color="red">[注意]</font><br/>
> ゲストログインしている状態で移行が成功すると、端末にログインしていたゲストアカウントは消えます。

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

退会猶予機能です。
一時退会をリクエストすると、即時に退会が行われずに一定の猶予期間が過ぎると退会が行われます。
猶予期間は、コンソールで変更できます。

> <font color="red">[注意]</font><br/>
>
> 退会猶予機能を使用する場合は**Withdraw API**を使用しないでください。
> **Withdraw API**は、即時にアカウントを退会します。

ログインが成功すると、AuthToken.member.temporaryWithdrawalで退会猶予状態のユーザーかを判断できます。

### Request TemporaryWithdrawal

一時退会をリクエストします。
コンソールに指定した期間が過ぎると自動的に退会処理が完了します。

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

退会猶予を使用するゲームは、AuthToken.member.temporaryWithdrawalがnullでない場合、ログイン後に該当ユーザーへ退会進行中という事実を毎回伝える必要があります。

**Example**

```cpp
void Sample::Login()
{
    IGamebase::Get().Login(GamebaseAuthProvider::Guest, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* authToken, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            if (authToken->member.temporaryWithdrawal.IsSet())
            {
                const auto temporaryWithdrawal = authToken->member.temporaryWithdrawal.GetValue();
                // gracePeriodDate: epoch time in milliseconds
                const FDateTime PeriodDate = FDateTime(1970, 1, 1) + FTimespan::FromMilliseconds(temporaryWithdrawal.gracePeriodDate);
                UE_LOG(GamebaseTestResults, Display, TEXT("User is under temporary withdrawa. GracePeriodDate : %s"), *PeriodDate.ToString());
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

退会リクエストをキャンセルします。
退会リクエスト後、猶予期間が過ぎて退会が完了すると、キャンセルできません。

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

退会猶予期間を無視して即時に退会を行います。
実際の内部動作はWithdraw APIと同じです。

即時退会はキャンセルができないため、ユーザーに実行するかどうかを重ねて確認してください。

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

* 「決済アビューズ自動解除」機能です。
    * 決済アビューズ自動解除機能は、決済アビューズ自動制裁で利用停止になるべきユーザーが利用停止猶予状態後に利用停止になるようにします。
    * 利用停止猶予状態の場合、設定した期間内に解除条件を全て満たすと正常プレイが可能になります。
    * 期間内に条件を満たさなかった場合は利用が停止されます。
* 決済アビューズ自動解除機能を使用するゲームは、ログイン後に常にAuthToken.getGraceBanInfo() APIを呼び出して、結果がnullではない有効なGraceBanInfoオブジェクトをリターンした場合、該当ユーザーに利用停止解除条件、期間などを案内する必要があります。
    * 利用停止猶予状態のユーザーのゲーム内アクセス制御はゲームで処理する必要があります。

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
        
        if (authToken->member.graceBan.IsSet())
        {
            const auto graceBan = authToken->member.graceBan.GetValue();

            // gracePeriodDate: epoch time in milliseconds
            const FDateTime PeriodDate = FDateTime(1970, 1, 1) + FTimespan::FromMilliseconds(graceBan.gracePeriodDate);

            if (graceBan.releaseRuleCondition.IsSet())
            {
                const auto releaseRuleCondition = graceBan.releaseRuleCondition.GetValue();

                // condition type: "AND", "OR"
                FString releaseRule = FString::Printf(TEXT("%lld%s %s %dtime(s)"), releaseRuleCondition.amount,
                    *releaseRuleCondition.currency, *releaseRuleCondition.conditionType, releaseRuleCondition.count);
            }

            if (graceBan.paymentStatus.IsSet())
            {
                const auto paymentStatus = graceBan.paymentStatus.GetValue();

                FString paidAmount = FString::Printf(TEXT("%lld%s"), paymentStatus.amount, *paymentStatus.currency);
                FString paidCount = paymentStatus.count + "time(s)";
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Login succeeded. Gamebase userId is %s"), *authToken->member.userId);
        }
    }));
}
```

## Error Handling

| Category       | Error                                    | Error Code | Description                              |
| -------------- | ---------------------------------------- | ---------- | ---------------------------------------- |
| Auth           | INVALID\_MEMBER                          | 6          | 無効な会員に対するリクエストです。 |
|                | BANNED\_MEMBER                           | 7          | 制裁中の会員です。 |
|                | AUTH\_USER\_CANCELED                     | 3001       | ログインがキャンセルされました。 |
|                | AUTH\_NOT\_SUPPORTED\_PROVIDER           | 3002       | サポートしない認証方式です。 |
|                | AUTH\_NOT\_EXIST\_MEMBER                 | 3003       | 存在しないか、退会した会員です。 |
|                | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006 | 外部認証ライブラリの初期化に失敗しました。 |
|                | AUTH\_EXTERNAL\_LIBRARY\_ERROR           | 3009       | 外部認証ライブラリエラーです。 <br/>詳細エラーを確認してください。 |
|                | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR       | 3010       | 以前の認証プロセスが完了していません。 |
|                | AUTH\_INVALID\_GAMEBASE\_TOKEN           | 3011       | Gamebase Access Tokenが有効ではないためログアウトしました。<br/>もう一度ログインを行ってください。 |
| TransferAccount| SAME\_REQUESTOR                          | 8          | 発行したTransferAccountを同じ端末で使用しました。 |
|                | NOT\_GUEST\_OR\_HAS\_OTHERS              | 9          | ゲストではないアカウントで移行を試行したか、アカウントにゲスト以外のIdPが連携されています。 |
|                | AUTH_TRANSFERACCOUNT_EXPIRED             | 3041       | TransferAccountの有効期限が切れました。 |
|                | AUTH_TRANSFERACCOUNT_BLOCK               | 3042       | 無効なTransferAccountを複数回入力したため、アカウント移行機能がロックされました。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_ID          | 3043       | TransferAccountのIDが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_INVALID_PASSWORD    | 3044       | TransferAccountのPasswordが有効ではありません。 |
|                | AUTH_TRANSFERACCOUNT_CONSOLE_NO_CONDITION | 3045      | TransferAccountが設定されていません。<br/> NHN Cloud Gamebase Consoleで先に設定してください。 |
|                | AUTH_TRANSFERACCOUNT_NOT_EXIST           | 3046       | TransferAccountが存在しません。TransferAccountを先に発行してください。|
|                | AUTH_TRANSFERACCOUNT_ALREADY_EXIST_ID    | 3047       | TransferAccountがすでに存在します。 |
|                | AUTH_TRANSFERACCOUNT_ALREADY_USED        | 3048       | TransferAccountがすでに使用されました。 |
| Auth (Login) | AUTH_TOKEN_LOGIN_FAILED | 3101 | トークンログインに失敗しました。 |
|  | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | トークン情報が有効ではありません。 |
|  | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | 最近ログインしたIdP情報がありません。 |
| IdP Login | AUTH_IDP_LOGIN_FAILED | 3201 | IdPログインに失敗しました。 |
|  | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3202 | IdP情報が有効ではありません。(Consoleに該当IdP情報がありません。) |
| Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | マッピングの追加に失敗しました。 |
|  | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | すでに他のメンバーにマッピングされています。 |
|  | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | すでに同じIdPにマッピングされています。 |
|  | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | IdP情報が有効ではありません。(Consoleに該当IdP情報がありません。) |
|                | AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP    | 3305       | Guest IdPではAddMappingができません。 |
| Add Mapping Forcibly | AUTH_ADD_MAPPING_FORCIBLY_NOT_EXIST_KEY         | 3311       | 強制マッピングキー(ForcingMappingKey)が存在しません。<br/>ForcingMappingTicketをもう一度確認してください。|
|                      | AUTH_ADD_MAPPING_FORCIBLY_ALREADY_USED_KEY      | 3312       | 強制マッピングキー(ForcingMappingKey)がすでに使用されました。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_EXPIRED_KEY           | 3313       | 強制マッピングキー(ForcingMappingKey)の有効期限が切れました。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_IDP         | 3314       | 強制マッピングキー(ForcingMappingKey)が他のIDPに使用されました。<br/>発行されたForcingMappingKeyは、同じIdPに強制マッピングを試行するのに使われます。 |
|                      | AUTH_ADD_MAPPING_FORCIBLY_DIFFERENT_AUTHKEY     | 3315       | 強制マッピングキー(ForcingMappingKey)が他のアカウントに使用されました。<br/>発行されたForcingMappingKeyは、同じIdPおよびアカウントに強制マッピングを試行するのに使われます。|
| Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | マッピングの削除に失敗しました。 |
|  | AUTH_REMOVE_MAPPING_LAST_MAPPED\_IDP | 3402 | 最後にマッピングされたIdPは削除できません。 |
|  | AUTH_REMOVE_MAPPING_LOGGED_IN\_IDP | 3403 | 現在ログインしているIdPです。 |
| Logout | AUTH_LOGOUT_FAILED | 3501 | ログアウトに失敗しました。 |
| Withdrawal     | AUTH\_WITHDRAW\_FAILED                   | 3601       | 退会に失敗しました。                             |
|                | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | すでに一時退会中のユーザーです。                   |
|                | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 一時退会中のユーザーではありません。                    |
| Not Playable | AUTH_NOT_PLAYABLE | 3701 | プレイできない状態です。(メンテナンスまたはサービス終了など) |
| Auth(Unknown) | AUTH_UNKNOWN_ERROR | 3999 | 不明なエラーです。(定義されていないエラーです。) |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**AUTH_EXTERNAL_LIBRARY_ERROR**

* このエラーは外部認証ライブラリでエラーが発生した時に返されます。
* 外部認証ライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

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

* 詳細エラーコードは、それぞれの外部認証ライブラリのDeveloperページを参照してください。
