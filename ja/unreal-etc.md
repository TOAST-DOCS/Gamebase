## Game > Gamebase > Unreal SDK使用ガイド > ETC

## Additional Features

Gamebaseでサポートする付加機能を説明します。

### Device Language

* 端末に設定された言語コードを返します。
* 複数の言語が登録されている場合、優先度が最も高い言語のみを返します。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* Gamebaseで提供するUIおよびSystemDialogに表示される言語を、端末に設定された言語ではない別の言語に変更できます。
* Gamebaseは、クライアントに含まれているメッセージを表示したり、サーバーから取得したメッセージを表示します。
* DisplayLanguageを設定すると、ユーザーが設定した言語コード(ISO-639)に適合した言語でメッセージを表示します。
* 自由に言語セットを追加できます。追加できる言語コードは次のとおりです。

> [参考]
>
> Gamebaseのクライアントメッセージは英語(en)、韓国語(ko)、日本語(ja)のみ含みます。

#### Gamebaseでサポートする言語コードの種類

| Code | Name |
| --- | --- |
| de | German |
| en | English   |
| es | Spanish |
| fi | Finnish |
| fr | French |
| id | Indonesian |
| it | Italian |
| ja | Japanese |
| ko | Korean |
| pt | Portuguese |
| ru | Russian |
| th | Thai |
| vi | Vietnamese |
| ms | Malay |
| zh-CN | Chinese-Simplified |
| zh-TW | Chinese-Traditional |

言語コードは`GamebaseDisplayLanguageCode`クラスに定義されています。

> <font color="red">[注意]</font><br/>
>
> Gamebaseでサポートする言語コードは、大文字/小文字を区別します。
> 「EN」または「zh-cn」と設定すると、問題が発生する場合があります。


```cpp
namespace GamebaseDisplayLanguageCode
{
    static const FString German(TEXT("de"));
    static const FString English(TEXT("en"));
    static const FString Spanish(TEXT("es"));
    static const FString Finnish(TEXT("fi"));
    static const FString French(TEXT("fr"));
    static const FString Indonesian(TEXT("id"));
    static const FString Italian(TEXT("it"));
    static const FString Japanese(TEXT("ja"));
    static const FString Korean(TEXT("ko"));
    static const FString Portuguese(TEXT("pt"));
    static const FString Russian(TEXT("ru"));
    static const FString Thai(TEXT("th"));
    static const FString Vietnamese(TEXT("vi"));
    static const FString Malay(TEXT("ms"));
    static const FString Chinese_Simplified(TEXT("zh-CN"));
    static const FString Chinese_Traditional(TEXT("zh-TW"));
}
```

#### Gamebase初期化時のDisplay Language設定

Gamebase初期化時のDisplay Languageを設定できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void Initialize(const FGamebaseConfiguration& configuration, const FGamebaseLaunchingInfoDelegate& Callback);
```

**Example**

```cpp
void USample::Initialize(const FString& appID, const FString& appVersion)
{
    FGamebaseConfiguration Configuration;
    ...
    Configuration.displayLanguageCode = displayLanguage;
    ...

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->Initialize(Configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([=](const FGamebaseLaunchingInfo* LaunchingInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
            FString displayLanguage = Subsystem->GetDisplayLanguageCode();
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

#### Set Display Language

Gamebase初期化時、入力されたDisplay Languageを変更できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetDisplayLanguageCode(const FString& languageCode);
```

**Example**

```cpp
void USample::SetDisplayLanguageCode(cosnt FString& displayLanguage)
{
    FString displayLanguage = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->SetDisplayLanguageCode(displayLanguage);
}
```

#### Get Display Language

現在適用されているDisplay Languageを照会できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
FString GetDisplayLanguageCode() const;
```

**Example**

```cpp
void USample::GetDisplayLanguageCode()
{
    FString displayLanguage = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetDisplayLanguageCode();
}
```

#### 新規言語セットを追加

Unreal Android、iOSプラットフォームでの新規言語セット追加方法は、下記のガイドを参照してください。

* [Android新規言語セット追加](./aos-etc#display-language)
* [iOS新規言語セット追加](./ios-etc#display-language)

#### Display Language優先順位

初期化およびSetDisplayLanguageCode APIでDisplay Languageを設定する場合、最終適用されるDisplay Languageは、入力した値と異なる値が適用される場合があります。

1. 入力したlanguageCodeがlocalizedstring.jsonファイルに定義されているかを確認します。
2. Gamebase初期化時、端末に設定された言語コードがlocalizedstring.jsonファイルに定義されているかを確認します。(この値は初期化後、端末に設定された言語を変更しても維持されます。)
3. Display Languageのデフォルト値である`en`が自動的に設定されます。

### Country Code

* GamebaseはSystemの国コードを次のようにAPIで提供しています。
* 各APIの特徴があるので、用途に合わせてAPIを選択してください。

#### USIM Country Code

* USIMに記録された国コードを返します。
* USIMに無効な国コードが記録されていても、追加のチェックを行わず、そのまま返します。
* 値が空の場合、「ZZ」を返します。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* OSから伝達された端末国コードを追加のチェックを行わず、そのまま返します。
* 端末国コードは「言語」設定に応じてOSが自動的に決定します。
* 複数の言語が登録された場合、優先度が最も高い言語で国コードを決定します。
* 値が空の場合、「ZZ」を返します。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* USIM、端末言語設定の順序で国コードを確認して返します。
* GetCountryCode APIは、次の順序で動作します。
    1. USIMに記録された国コードを確認し、値が存在すれば追加のチェックを行わず、そのまま返します。
    2. USIM国コードが空の場合、端末国コードを確認し、値が存在すれば追加のチェックを行わず、そのまま返します。
    3. USIM、端末国コードがどちらも空の場合は「ZZ」を返します。

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCode() const;
```

### Gamebase Event Handler

* Gamebaseは各種イベントを**GamebaseEventHandler**という1つのイベントシステムで全て処理できます。
* GamebaseEventHandlerは以下のAPIを利用して簡単にListenerを追加/削除できます。

* GamebaseEventHandlerは下記のAPIを利用して簡単にHandlerを追加/削除できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cs
FDelegateHandle AddHandler(const FGamebaseEventDelegate::FDelegate& Callback);
void RemoveHandler(const FDelegateHandle& handle);
void RemoveAllHandler();
```

**VO**

```cpp
struct GAMEBASE_API FGamebaseEventMessage
{
	// Eventの種類を表します。
    // GamebaseEventCategoryクラスの値が割り当てられます。
    FString Category;

    // 各Categoryに合ったVOに変換できるJSON Stringデータです。
    FString Data;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::IdPRevoked))
        {
            auto IdpRevokedData = FGamebaseEventIdPRevokedData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::LoggedOut))
        {
            auto LoggedOutData = FGamebaseEventLoggedOutData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
                 Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived) ||
                 Message.Category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
        {
            auto ServerPushData = FGamebaseEventServerPushData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ObserverLaunching))
        {
            auto ObserverData = FGamebaseEventObserverData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ObserverNetwork))
        {
            auto ObserverData = FGamebaseEventObserverData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ObserverHeartbeat))
        {
            auto ObserverData = FGamebaseEventObserverData::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::PurchaseUpdated))
        {
            auto PurchasableReceipt = FGamebaseEventPurchasableReceipt::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::PushReceivedMessage))
        {
            auto PushMessage = FGamebaseEventPushMessage::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::PushClickMessage))
        {
            auto PushMessage = FGamebaseEventPushMessage::From(Message.Data);
        }
        else if (Message.Category.Equals(GamebaseEventCategory::PushClickAction))
        {
            auto PushAction = FGamebaseEventPushAction::From(Message.Data);
        }
    }));
}
```
* CategoryはGamebaseEventCategoryクラスに定義されています。
* イベントは大きくLoggedOut、ServerPush、Observer、Purchase、Pushに分けられ、各Categoryに基づいて、GamebaseEventMessage.dataを次の表のような方法でVOに変換できます。


| Event種類 | GamebaseEventCategory | VO変換方法 | 備考 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | GamebaseEventCategory::IdPRevoked | FGamebaseEventIdPRevokedData::From(Message.Data) | \- |
| LoggedOut | GamebaseEventCategory::LoggedOut | FGamebaseEventLoggedOutData::From(Message.Data) | \- |
| ServerPush | GamebaseEventCategory::ServerPushAppKickOut<br>GamebaseEventCategory::ServerPushAppKickOutMessageReceived<br>GamebaseEventCategory::ServerPushTransferKickout | FGamebaseEventServerPushData::From(Message.Data) | \- |
| Observer | GamebaseEventCategory::ObserverLaunching<br>GamebaseEventCategory::ObserverNetwork<br>GamebaseEventCategory::ObserverHeartbeat | FGamebaseEventObserverData::From(Message.Data) | \- |
| Purchase - プロモーション決済 | GamebaseEventCategory::PurchaseUpdated | FGamebaseEventPurchasableReceipt::From(Message.Data) | \- |
| Push - メッセージ受信 | GamebaseEventCategory::PushReceivedMessage | FGamebaseEventPushMessage::From(Message.Data) |  |
| Push - メッセージクリック | GamebaseEventCategory::PushClickMessage | FGamebaseEventPushMessage::From(Message.Data) |  |
| Push - アクションクリック | GamebaseEventCategory::PushClickAction | FGamebaseEventPushAction::From(Message.Data) | RichMessageボタンを押すと動作します。 |

#### IdP Revoked

> [参考]
>
> iOS Appleidログインを使用する場合にのみ発生するイベントです。

* IdPで該当サービスを削除したときに発生するイベントです。
* ユーザーにIdPが使用停止したことを知らせ、同じIdPでログインするとき、userIDを新たに発行できるように実装する必要があります。
* FGamebaseEventIdPRevokedData.code: GamebaseIdPRevokedCode値を意味します。
    * WITHDRAW : 600
        * 現在使用停止しているIdPでログインしていて、マッピングされたIdPリストがないことを意味します。
        * withdraw APIを呼び出して現在のアカウントを退会させる必要があります。
    * OverwriteLoginAndRemoveMapping : 601
        * 現在使用停止しているIdPでログインしていて、使用停止しているIdP以外の他のIdPがマッピングされている場合を意味します。
        * マッピングされたIdPリストのうちの1つのIdPにログインし、removeMapping APIを呼び出して使用停止しているIdPの連動を解除する必要があります。
    * RemoveMapping : 602
        * 現在アカウントにマッピングされているIdPのうち、使用停止しているIdPがある場合を意味します。
        * RemoveMapping APIを呼び出して使用停止しているIdPの連動を解除する必要があります。
* FGamebaseEventIdPRevokedData.idpType：使用停止しているIdPタイプを意味します。
* FGamebaseEventIdPRevokedData.authMappingList：現在アカウントにマッピングされているIdPリストを意味します。

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::IdPRevoked))
        {
            auto IdpRevokedData = FGamebaseEventIdPRevokedData::From(Message.Data);
            if (IdpRevokedData.IsValid())
            {
                ProcessIdPRevoked(IdpRevokedData);
            }
        }
    }));
}

void USample::ProcessIdPRevoked(const FGamebaseEventIdPRevokedData& Data)
{
    auto RevokedIdP = Data->IdpType;
    switch (Data->code)
    {
        // 現在使用停止しているIdPでログインしていて、マッピングされたIdPリストがないことを意味します。
        // ユーザーに現在のアカウントが退会していることを伝えてください。
        case GamebaseIdPRevokeCode::Withdraw:
        {
            Subsystem->Withdraw(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
            {
                ...
            }));
            break;
        }
        case GamebaseIdPRevokeCode::OverwriteLoginAndRemoveMapping:
        {
            // 現在使用停止しているIdPでログインしていて、使用停止したIdP以外の他のIdPがマッピングされている場合を意味します。
            // ユーザーがauthMappingListのうちどのIdPで再度ログインするか選択し、選択したIdPでログインした後、使用停止したIdPについては連動を解除してください。
            auto SelectedIdP = "ユーザーが選択したIdP";
            FGamebaseVariantMap AdditionalInfo;
            AdditionalInfo.Add(GamebaseAuthProviderCredential::IgnoreAlreadyLoggedIn, true);

            Subsystem->Login(SelectedIdP, *AdditionalInfo, FGamebaseAuthTokenDelegate::CreateLambda([=](const FGamebaseAuthToken* AuthToken, const FGamebaseError* Error)
            {
                if (Gamebase::IsSuccess(Error))
                {
                    Subsystem->RemoveMapping(RevokedIdP, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
                    {
                        ...
                    }));
                }
            }));
            break;
        }
        case GamebaseIdPRevokeCode::RemoveMapping:
        {
            // 現在のアカウントにマッピングされているIdPのうち使用停止しているIdPがある場合を意味します。
            // ユーザーに現在アカウントで使用停止しているIdPが連動解除されていることを伝えてください。
            Subsystem->RemoveMapping(RevokedIdP, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
            {
                ...
            }));
            break;
        }
    }
}
```

#### Logged Out

* Gamebase Access Tokenの有効期限が切れてネットワークセッションを復元するためにログイン関数の呼び出しが必要な場合に発生するイベントです。

**Example**

```cpp
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}
private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.LOGGED_OUT:
            {
                GamebaseResponse.Event.GamebaseEventLoggedOutData loggedData = GamebaseResponse.Event.GamebaseEventLoggedOutData.From(message.data);
                if (loggedData != null)
                {
                    // There was a problem with the access token.
                    // Call login again.
                }
                break;
            }
    }
}
voidvoid USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::LoggedOut))
        {
            auto LoggedOutData = FGamebaseEventLoggedOutData::From(Message.Data);
            if (loggedData.IsValid() == true)
            {
                // There was a problem with the access token.
                // Call login again.
            }
        }
        }
    }));
}
```

#### Server Push

* Gamebaseサーバーからクライアント端末へ送信するメッセージです。
* GamebaseでサポートするServer Push Typeは次の通りです。
    * GamebaseEventCategory::ServerPushAppKickOutMessageReceived
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受け取ります。
        * クライアント端末でサーバーメッセージを受信したときに動作するイベントです。
        * ゲームで「オートプレイ」などが動作中の場合に、ゲームを一時停止させる目的で活用できます。
    * GamebaseEventCategory::ServerPushAppKickOut
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受信します。
        * クライアント端末でサーバーメッセージを受信したときにポップアップを表示しますが、ユーザーがポップアップを閉じたときに動作するイベントです。
    * GamebaseEventCategory::ServerPushTransferKickout
    	* Guestアカウントを他の端末へ移行すると、以前の端末でキックアウトメッセージを受信します。

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
            Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived) ||
            Message.Category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
        {
            auto serverPushData = FGamebaseEventServerPushData::From(Message.Data);
            if (serverPushData.IsVaild())
            {
                CheckServerPush(Message.Category, *serverPushData);
            }
        }
    }));
}

void Sample::CheckServerPush(const FString& category, const FGamebaseEventServerPushData& data)
{
    if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut))
    {
        // Kicked out from Gamebase server.(Maintenance, banned or etc.)
        // And the game user closes the kickout pop-up.
        // Return to title and initialize Gamebase again.
    }
   else if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived))
    {
        // Currently, the kickout pop-up is displayed.
        // If your game is running, stop it.
    }
    else if (message.category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
    {
        // If the user wants to move the guest account to another device,
        // if the account transfer is successful,
        // the login of the previous device is released,
        // so go back to the title and try to log in again.
    }
}
```

#### Observer

* Gamebase Gamebaseの各種状態変動イベントを処理するシステムです。
* GamebaseでサポートするObserver Typeは次の通りです。
    * GamebaseEventCategory::ObserverLaunching
    	* メンテナンスが開始または終了した場合、新しいバージョンが配布されてアップデートが必要な場合など、Launching状態が変更された時に動作します。
        * GamebaseEventObserverData.code : LaunchingStatus値を意味します。
            * GamebaseLaunchingStatus::IN_SERVICE: 200
            * GamebaseLaunchingStatus::RECOMMEND_UPDATE: 201
            * GamebaseLaunchingStatus::IN_SERVICE_BY_QA_WHITE_LIST: 202
            * GamebaseLaunchingStatus::REQUIRE_UPDATE: 300
            * GamebaseLaunchingStatus::BLOCKED_USER: 301
            * GamebaseLaunchingStatus::TERMINATED_SERVICE: 302
            * GamebaseLaunchingStatus::INSPECTING_SERVICE: 303
            * GamebaseLaunchingStatus::INSPECTING_ALL_SERVICES: 304
            * GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR: 500
    * GamebaseEventCategory::ObserverHeartbeat
    	* 退会処理や利用停止により、ユーザーアカウントの状態が変わった時に動作します。
        * GamebaseEventObserverData.code : GamebaseError値を意味します。
            * GamebaseErrorCode::INVALID_MEMBER: 6
            * GamebaseErrorCode::BANNED_MEMBER: 7
    * GamebaseEventCategory::ObserverNetwork
        * Android, iOSに限る
    	* ネットワーク変動事項情報を受け取れます。
    	* ネットワークが切断されたり、接続された時、またはWi-FiからCellularネットワークに変更された時に動作します。
        * GamebaseEventObserverData.code : NetworkManager値を意味します。
            * EGamebaseNetworkType::Not: 255
            * EGamebaseNetworkType::Mobile: 0
            * EGamebaseNetworkType::Wifi: 1
            * EGamebaseNetworkType::Any: 2
**VO**

```cpp
struct GAMEBASE_API FGamebaseEventObserverData
{
	// 状態値を表す情報です。
    int32 Code;

    // 状態に関連するメッセージ情報です。
    FString Message;

    // 追加情報用の予備フィールドです。
    FString Extras;
}
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::ObserverLaunching))
        {
            auto observerData = FGamebaseEventObserverData::From(Message.Data);
            if (observerData.IsVaild())
            {
                CheckLaunchingStatus(*observerData);
            }
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ObserverNetwork))
        {
            auto observerData = FGamebaseEventObserverData::From(Message.Data);
            if (observerData.IsVaild())
            {
                CheckNetwork(*observerData);
            }
        }
        else if (Message.Category.Equals(GamebaseEventCategory::ObserverHeartbeat))
        {
            auto observerData = FGamebaseEventObserverData::From(Message.Data);
            if (observerData.IsVaild())
            {
                CheckHeartbeat(*observerData);
            }
        }
    }));
}

void USample::CheckLaunchingStatus(const FGamebaseEventObserverData& Data)
{
    switch (Data.code)
    {
        case GamebaseLaunchingStatus::IN_SERVICE:
            {
                // Service is now normally provided.
                break;
            }
        // ... 
        case GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR:
            {
                // Error in internal server.
                break;
            }
    }
}

void USample::CheckNetwork(const FGamebaseEventObserverData& Data)
{
    switch ((GamebaseNetworkType)Data.code)
    {
        case EGamebaseNetworkType::Not:
            {
                // Network disconnected.
                break;
            }
        case EGamebaseNetworkType::Mobile:
        case EGamebaseNetworkType::Wifi:
        case EGamebaseNetworkType::Any:
            {
                // Network connected.
                break;
            }
    }
}

void USample::CheckHeartbeat(const FGamebaseEventObserverData& Data)
{
    switch (Data.code)
    {
        case EGGamebaseErrorCode::INVALID_MEMBER:
            {
                // You can check the invalid user session in here.
                // ex) After transferred account to another device.
                break;
            }
        case EGGamebaseErrorCode::BANNED_MEMBER:
            {
                // You can check the banned user session in here.
                break;
            }
    }
}
```

#### Purchase Updated

* Promotionコードを入力して商品を獲得した場合に発生するイベントです。
* 決済領収書情報を取得できます。

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::PurchaseUpdated))
        {
            auto purchasableReceipt = FGamebaseEventPurchasableReceipt::From(Message.Data);
            if (purchasableReceipt.IsVaild())
            {
                // If the user got item by 'Promotion Code',
                // this event will be occurred.
            }
            }
        }
    }));
}
```


#### Push Received Message

* Pushメッセージが到着した時に発生するイベントです。
* extrasフィールドをJSONに変換して、Push送信時に送信したカスタム情報を取得することもできます。
    * **Android**では**isForeground**フィールドでフォアグラウンドでメッセージを受信したのか、バックグラウンドでメッセージを受信したのかを区別できます。

**VO**

```cpp
struct FGamebaseEventPushMessage
{
	// メッセージ固有のidです。
    FString Id

    // Pushメッセージタイトルです。
    FString Title;

    // Pushメッセージ本文内容です。
    FString Body;

    // JSON形式でPush送信時、送信したカスタム情報を確認できます。
    FString Extras;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::PushReceivedMessage))
        {
            auto PushMessage = FGamebaseEventPushMessage::From(Message.Data);
            if (PushMessage.IsVaild())
            {
                // When you clicked push Message.

                // By converting the extras field of the push Message to JSON,
                // you can get the custom information added by the user when sending the push.
                // (For Android, an 'isForeground' field is included so that you can check if received in the foreground state.)
            }
        }
    }));
}
```

#### Push Click Message

* 受信したPushメッセージをクリックした時に発生するイベントです。
* 'GamebaseEventCategory::PushReceivedMessage'とは異なり、Androidのextrasフィールドに**isForeground**情報が存在しません。

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::PushClickMessage))
        {
            auto PushMessage = FGamebaseEventPushMessage::From(Message.Data);
            if (PushMessage.IsVaild())
            {
                // When you clicked push Message.
            }
        }
    }));
}
```


#### Push Click Action

* Rich Message機能を利用して作成したボタンをクリックした時に発生するイベントです。
* ActionType は、次の項目が提供されます。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```cpp
struct FGamebaseEventPushAction
{
	// ボタンアクション種類です。
    FString ActionType ;

	// PushMessageデータです。
    FGamebaseEventPushMessage Message;

	// Pushコンソールで入力したユーザーテキストです。
    FString UserText;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::PushClickAction))
        {
            auto PushAction = FGamebaseEventPushAction::From(Message.Data);
            if (PushAction.IsVaild())
            {
                // When you clicked action button by 'Rich Message'.
            }
        }
    }));
}
```


### Analytics

Game指標をGamebase Serverへ転送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIはログイン後に呼び出せます。
>
>
> [TIP]
>
> RequestPurchase APIを呼び出して決済が完了すると、自動的に指標を転送します。
>

Analytics Consoleの使用方法は、下記のガイドを参照してください。

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

ゲームログイン後、ゲームユーザーレベル情報を指標として転送できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後、SetGameUserData APIを呼び出さない場合、他の指標からLevel情報が漏れる場合があります。
>

APIの呼び出しに必要なパラメータは下記のとおりです。

**FGamebaseAnalyticsUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| UserLevel | M | int32 | ゲームユーザーレベルを表すフィールドです。 |
| ChannelId | O | FString | チャンネルを表すフィールドです。 |
| CharacterId | O | FString | キャラクター名を表すフィールドです。 |
| CharacterClassId | O | FString | 職業を表すフィールドです。 |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetGameUserData(const FGamebaseAnalyticsUserData& gameUserData);
```

**Example**

```cpp
void USample::SetGameUserData(int32 UserLevel, const FString& ChannelId, const FString& CharacterId, const FString& CharacterClassId)
{
    FGamebaseAnalyticsUserData GameUserData;
    GameUserData.UserLevel = UserLevel;
    GameUserData.ChannelId = ChannelId;
    GameUserData.CharacterId = CharacterId;
    GameUserData.CharacterClassId = CharacterClassId;
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetAnalytics().SetGameUserData(GameUserData);
}

```

#### Level Up Trace

レベルアップした時、ゲームユーザーレベル情報を指標として転送できます。

APIの呼び出しに必要なパラメータは下記の通りです。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc    |
| -------------------------- | -------------------------- | ---- | ---- |
| UserLevel | M | int32 | ゲームユーザーレベルを表すフィールドです。 |
| LevelUpTime | M | int64 | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void TraceLevelUp(const FGamebaseAnalyticesLevelUpData& levelUpData);
```

**Example**

```cpp
void USample::TraceLevelUpNow(int32 UserLevel)
{
    FGamebaseAnalyticesLevelUpData levelUpData{ UserLevel, FDateTime::Now().ToUnixTimestamp() };
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetAnalytics().TraceLevelUp(levelUpData);
}
```

### Contact

Gamebaseは、顧客からの問い合わせに対応するための機能を提供します。

> [TIP]
>
> NHN Cloud Contactサービスと連携して使用すると、より簡単に顧客からの問い合わせに対応できます。
> 詳しいNHN Cloud Contactサービス利用方法は、下記のガイドを参照してください。
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ja/Contact%20Center/ja/online-contact-overview/)

#### Customer Service Type

**Gamebaseコンソール > App > InApp URL > Service center**では、以下の3つのタイプのサポートを選択できます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud  Online Contact      | O              |

タイプに応じてGamebase SDKのサポートAPIは次のURLを使用します。

* 開発会社独自のサポート(Developer customer center)
    * **サポートURL**に入力したURL.
* Gamebase提供サポート(Gamebase customer center)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。
* NHN Cloud組織商品(Online Contact)
    * ログイン前：NOT_LOGGED_IN(2)エラーが発生。
    * ログイン後：ユーザー情報が含まれたサポートURL。

#### Open Contact WebView

サポートWebビューを表示します。
URLはサポートタイプに基づいて決定されます。
ContactConfigurationでURLに追加情報を伝達できます。

**FGamebaseContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| UserName      | O             | FString                            | ユーザー名前(ニックネーム) <br>**default** : ""   |
| AdditionalURL | O             | FString                            | 開発会社独自のサポートURLの後ろにつく追加のURL <br>**default** : ""    |
| AdditionalParameters | O      | TMap<string, string>               | サポートURLの後ろにつく追加のパラメータ<br>**default**：EmptyMap |
| ExtraData     | O             | TMap<FString, FString>             | 開発会社が任意のextra dataをサポートオープン時に伝達<br>**default** : EmptyMap |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void OpenContact(const FGamebaseErrorDelegate& onCloseCallback);
void OpenContact(const FGamebaseContactConfiguration& Configuration, const FGamebaseErrorDelegate& onCloseCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| NOT\_LOGGED\_IN(2)                                  | サポートタイプが'NHN Cloud  OC'なのにログイン前に呼び出しました。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | サポートWebビューがすでに表示中です。 |

**Example**

```cpp
void USample::OpenContact()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetContact()->OpenContact(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("OpenContact succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("OpenContact failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Messsage);

            if (Error->Code == GamebaseErrorCode::WEBVIEW_INVALID_URL)
            {
                // Gamebase Console Service Center URL is invalid.
                // Please check the url field in the TOAST Gamebase Console.
                auto LaunchingInfo = Subsystem->GetLaunching().GetLaunchingInformations();
                UE_LOG(GamebaseTestResults, Display, TEXT("csUrl: %s"), *LaunchingInfo->Launching.App.RelatedUrls.CsUrl);
            }
        }
    }));
}
```

> <font color="red">[注意]</font><br/>
>
> サポートへお問い合わせする時、ファイルの添付が必要な場合があります。
> そのため、ユーザーからカメラ撮影やStorage保存の権限をランタイムに取得する必要があります。
>
> Androidユーザー
>
> * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> * Unrealの場合エンジンに内蔵されている **Android Runtime Permission**プラグインを有効にした後、以下のAPI Referenceを確認して必要な権限を取得してください。
> [Unreal API Reference : AndroidPermission](https://docs.unrealengine.com/en-US/API/Plugins/AndroidPermission/index.html)
>
> iOSユーザー
>
> * info.plistに'Privacy - Camera Usage Description'、'Privacy - Photo Library Usage Description'の設定を行ってください。

#### Request Contact URL

サポートのWebビューを表示するのに使用されるURLを返します。

**API**

```cs
void RequestContactURL(const FGamebaseContactUrlDelegate& Callback);
void RequestContactURL(const FGamebaseContactConfiguration& configuration, const FGamebaseContactUrlDelegate& Callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| NOT\_LOGGED\_IN(2)                                  | サポートタイプが'NHN Cloud  OC'なのにログイン前に呼び出しました。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |

**Example**

``` cs
void USample::RequestContactURL(const FString& userName)
{
    FGamebaseContactConfiguration Configuration{ userName };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetContact()->RequestContactURL(Configuration, FGamebaseContactUrlDelegate::CreateLambda([=](FString url, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            // Open webview with 'contactUrl'
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestContactURL succeeded. (url = %s)"), *url);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestContactURL failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Messsage);

            if (Error->Code == GamebaseErrorCode::UI_CONTACT_FAIL_INVALID_URL)
            {
                // Gamebase Console Service Center URL is invalid.
                // Please check the url field in the TOAST Gamebase Console.
            }
            else
            {
                // An Error occur when requesting the contact web view url.
            }
        }
    }));
}
```
