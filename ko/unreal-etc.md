## Game > Gamebase > Unreal SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

### Device Language

* 단말기에 설정된 언어 코드를 반환합니다.
* 여러 개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 반환합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* Gamebase에서 제공하는 UI 및 SystemDialog에 표시되는 언어를 단말기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.
* Gamebase는 클라이언트에 포함되어 있는 메시지를 표시하거나 서버에서 받은 메시지를 표시합니다.
* DisplayLanguage를 설정하면 사용자가 설정한 언어코드(ISO-639)에 적합한 언어로 메시지를 표시합니다.
* 원하는 언어셋을 추가할 수 있습니다. 추가할 수 있는 언어 코드는 다음과 같습니다.

> [참고]
>
> Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함합니다.

#### Gamebase에서 지원하는 언어코드의 종류

| Code | Name |
| --- | --- |
| de | German |
| en | English |
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

해당 언어 코드는 `GamebaseDisplayLanguageCode` 클래스에 정의되어 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase에서 지원하는 언어 코드는 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.


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

#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void Initialize(const FGamebaseConfiguration& Configuration, const FGamebaseLaunchingInfoDelegate& Callback);
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
    Subsystem->Initialize(Configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([Subsystem](const FGamebaseLaunchingInfo* LaunchingInfo, const FGamebaseError* Error)
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

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

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

현재 적용된 Display Language를 조회할 수 있습니다.

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
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    FString displayLanguage = Subsystem->GetDisplayLanguageCode();
}
```

#### 신규 언어셋 추가

Unreal Android, iOS 플랫폼에서의 신규 언어셋 추가 방법은 아래 가이드를 참고하십시오.

* [Android 신규 언어셋 추가](./aos-etc#display-language)
* [iOS 신규 언어셋 추가](./ios-etc#display-language)

#### Display Language 우선순위

초기화 및 SetDisplayLanguageCode API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedstring.json 파일에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 단말기에 설정된 언어코드가 localizedstring.json 파일에 정의되어 있는지 확인합니다.(이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동으로 설정됩니다.

### Country Code

* Gamebase는 System의 국가 코드를 다음과 같은 API로 제공하고 있습니다.
* 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가 코드를 반환합니다.
* USIM에 잘못된 국가 코드가 기록되어 있다 하더라도 추가적인 확인 없이 그대로 반환합니다.
* 값이 비어 있는 경우 'ZZ'를 반환합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* OS로부터 전달받은 단말기 국가 코드를 추가적인 확인 없이 그대로 반환합니다.
* 단말기 국가 코드는 '언어' 설정에 따라 OS가 자동으로 결정합니다.
* 여러 개의 언어가 등록된 경우, 우선권이 가장 높은 언어로 국가 코드를 결정합니다.
* 값이 비어 있는 경우 'ZZ'를 반환합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* USIM, 단말기 언어 설정의 순서로 국가 코드를 확인하여 반환합니다.
* GetCountryCode API는 다음 순서로 동작합니다.
    1. USIM에 기록된 국가 코드를 확인하고, 값이 존재한다면 추가적인 확인 없이 그대로 반환합니다.
    2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인하고, 값이 존재한다면 추가 확인 없이 그대로 반환합니다.
    3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 반환합니다.

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetCountryCode() const;
```

### Gamebase Event Handler

* Gamebase는 각종 이벤트를 **GamebaseEventHandler**라는 하나의 이벤트 시스템에서 모두 처리할 수 있습니다.
* GamebaseEventHandler는 아래 API를 통해 간단하게 Listener를 추가/제거할 수 있습니다.

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
    // Event 종류를 나타냅니다.
    // GamebaseEventCategory 클래스의 값이 할당됩니다.
    FString Category;

    // 각 Category 에 맞는 VO 로 변환할 수 있는 JSON String 데이터입니다.
    FString Data;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

* Category는 GamebaseEventCategory 클래스에 정의되어 있습니다.
* 이벤트는 크게 LoggedOut, ServerPush, Observer, Purchase, Push로 나뉘며, 각 Category에 따라, 아래 표와 같은 방법으로 GamebaseEventMessage.data를 VO로 변환할 수 있습니다.

| Event 종류 | GamebaseEventCategory | VO 변환 방법 | 비고 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | GamebaseEventCategory::IdPRevoked | FGamebaseEventIdPRevokedData::From(Message.Data) | \- |
| LoggedOut | GamebaseEventCategory::LoggedOut | FGamebaseEventLoggedOutData::From(Message.Data) | \- |
| ServerPush | GamebaseEventCategory::ServerPushAppKickOut<br>GamebaseEventCategory::ServerPushAppKickOutMessageReceived<br>GamebaseEventCategory::ServerPushTransferKickout | FGamebaseEventServerPushData::From(Message.Data) | \- |
| Observer | GamebaseEventCategory::ObserverLaunching<br>GamebaseEventCategory::ObserverNetwork<br>GamebaseEventCategory::ObserverHeartbeat | FGamebaseEventObserverData::From(Message.Data) | \- |
| Purchase - 프로모션 결제 | GamebaseEventCategory::PurchaseUpdated | FGamebaseEventPurchasableReceipt::From(Message.Data) | \- |
| Push - 메시지 수신 | GamebaseEventCategory::PushReceivedMessage | FGamebaseEventPushMessage::From(Message.Data) |  |
| Push - 메시지 클릭 | GamebaseEventCategory::PushClickMessage | FGamebaseEventPushMessage::From(Message.Data) |  |
| Push - 액션 클릭 | GamebaseEventCategory::PushClickAction | FGamebaseEventPushAction::From(Message.Data) | RichMessage 버튼 클릭 시 동작합니다. |

#### IdP Revoked

> [참고]
>
> iOS Appleid 로그인을 사용하는 경우에만 발생할 수 있는 이벤트입니다.

* IdP에서 해당 서비스를 삭제하였을 때 발생하는 이벤트입니다.
* 유저에게 IdP가 사용 중지된 것을 알리고, 로그아웃 후 다시 로그인하도록 구현해야 합니다.

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
    {
        if (Message.Category.Equals(GamebaseEventCategory::IdPRevoked))
        {
            // TODO: process logout, then login again.
        }
    }));
}
```

#### Logged Out

* Gamebase Access Token이 만료되어 네트워크 세션을 복구하기 위해 로그인 함수 호출이 필요한 경우 발생하는 이벤트입니다.

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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
    }));
}
```

#### Server Push

* Gamebase 서버에서 클라이언트 단말기로 보내는 메시지입니다.
* Gamebase 에서 지원하는 Server Push Type 은 다음과 같습니다.
    * GamebaseEventCategory::ServerPushAppKickOutMessageReceived
    	* NHN Cloud Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신했을 때 바로 동작하는 이벤트 입니다.
        * '오토 플레이'와 같이 게임이 동작 중인 경우, 게임을 일시 정지 시키는 목적으로 활용할 수 있습니다.
    * GamebaseEventCategory::ServerPushAppKickOut
        * NHN Cloud  Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신했을 때 팝업을 표시하는데, 유저가 팝업을 닫았을 때 동작하는 이벤트 입니다.
    * GamebaseEventCategory::ServerPushTransferKickout
        * Guest 계정을 다른 단말기로 이전을 성공하게 되면 이전 단말기에서 킥아웃 메시지를 받게 됩니다.

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

void USample::CheckServerPush(const FString& Category, const FGamebaseEventServerPushData& Data)
{
    if (Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOut))
    {
        // Kicked out from Gamebase server.(Maintenance, banned or etc.)
        // And the game user closes the kickout pop-up.
        // Return to title and initialize Gamebase again.
    }
    else if (Message.Category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived))
    {
        // Currently, the kickout pop-up is displayed.
        // If your game is running, stop it.
    }
    else if (Message.Category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
    {
        // If the user wants to move the guest account to another device,
        // if the account transfer is successful,
        // the login of the previous device is released,
        // so go back to the title and try to log in again.
    }
}
```

#### Observer

* Gamebase Gamebase의 각종 상태 변동 이벤트를 처리하는 시스템입니다.
* Gamebase 에서 지원하는 Observer Type 은 다음과 같습니다.
    * GamebaseEventCategory::ObserverLaunching
        * 점검이 걸리거나 풀린 경우, 새로운 버전이 배포되어 업데이트가 필요한 경우와 같이, Launching 상태가 변경되었을 때 동작합니다.
        * GamebaseEventObserverData.code: LaunchingStatus 값을 의미합니다.
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
        * 탈퇴 처리 되거나 이용 정지로 인하여 사용자 계정 상태가 변했을 때 동작합니다.
        * GamebaseEventObserverData.code: GamebaseError 값을 의미합니다.
            * GamebaseErrorCode::INVALID_MEMBER: 6
            * GamebaseErrorCode::BANNED_MEMBER: 7
    * GamebaseEventCategory::ObserverNetwork
        *Android, iOS에 한합니다.
        * 네트워크 변동 사항 정보를 받을 수 있습니다.
        * 네트워크가 끊기거나 연결되었을 때, 혹은 Wifi에서 셀룰러 네트워크로 변경되었을 때 동작합니다.
        * GamebaseEventObserverData.code: NetworkManager 값을 의미합니다.
            * EGamebaseNetworkType::Not: 255
            * EGamebaseNetworkType::Mobile: 0
            * EGamebaseNetworkType::Wifi: 1
            * EGamebaseNetworkType::Any: 2
**VO**

```cpp
struct GAMEBASE_API FGamebaseEventObserverData
{
    // 상태값을 나타내는 정보입니다.
    int32 Code;

    // 추가 정보용 예비 필드입니다.
    FString Message;

    // 상태에 관련된 메시지 정보입니다.
    FString Extras;
}
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

* Promotion 코드 입력을 통해 상품을 획득한 경우 발생하는 이벤트입니다.
* 결제 영수증 정보를 획득할 수 있습니다.

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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
    }));
}
```


#### Push Received Message

* Push 메시지가 도착했을때 발생하는 이벤트입니다.
* extras 필드를 JSON으로 변환하여, Push 발송 시 전송했던 커스텀 정보를 얻을 수도 있습니다.
    * **Android**에서는 **isForeground** 필드를 통해 포그라운드에서 메시지를 수신했는지, 백그라운드에서 메시지를 수신했는지 구분할 수 있습니다.

**VO**

```cpp
struct FGamebaseEventPushMessage
{
    // 메시지 고유의 id입니다.
    FString Id;

    // Push 메시지 제목입니다.
    FString Title;

    // Push 메시지 본문 내용입니다.
    FString Body;

    // JSON 형식으로 Push 발송 시 전송했던 커스텀 정보를 확인할 수 있습니다.
    FString Extras;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

* 수신한 Push 메시지를 클릭했을 때 발생하는 이벤트입니다.
* 'GamebaseEventCategory::PushReceivedMessage'와는 다르게 Android에서 extras 필드에 **isForeground** 정보가 존재하지 않습니다.

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

* Rich Message 기능을 통해 생성한 버튼을 클릭했을 때 발생하는 이벤트입니다.
* ActionType은 다음 항목이 제공됩니다.
    * "OPEN_APP"
    * "OPEN_URL"
    * "REPLY"
    * "DISMISS"


**VO**

```cpp
struct FGamebaseEventPushAction
{
    // 버튼 액션 종류입니다.
    FString ActionType;

    // PushMessage 데이터입니다.
    FGamebaseEventPushMessage Message;

    // Push 콘솔에서 입력한 사용자 텍스트입니다.
    FString UserText;
};
```

**Example**

```cpp
void USample::AddEventHandler()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([](const FGamebaseEventMessage& Message)
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

Game지표를 Gamebase Server로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase Analytics에서 지원하는 모든 API는 로그인 후에 호출할 수 있습니다.
>
>
> [TIP]
>
> RequestPurchase API를 호출하여 결제를 완료하면, 자동으로 지표를 전송합니다.
>

Analytics Console 사용법은 아래 가이드를 참고하십시오.

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

게임 로그인 이후 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 SetGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**FGamebaseAnalyticsUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| UserLevel | M | int32 | 게임 유저 레벨을 나타내는 필드입니다. |
| ChannelId | O | FString | 채널을 나타내는 필드입니다. |
| CharacterId | O | FString | 캐릭터 이름을 나타내는 필드입니다. |
| CharacterClassId | O | FString | 직업을 나타내는 필드입니다. |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetGameUserData(const FGamebaseAnalyticsUserData& GameUserData);
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
    Subsystem->GetAnalytics()->SetGameUserData(GameUserData);
}

```

#### Level Up Trace

레벨업이 되었을 경우 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc    |
| -------------------------- | -------------------------- | ---- | ---- |
| UserLevel | M | int32 | 게임 유저 레벨을 나타내는 필드입니다. |
| LevelUpTime | M | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력합니다. |

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
    Subsystem->GetAnalytics()->TraceLevelUp(levelUpData);
}
```

### Contact

Gamebase 는 고객 문의 대응을 위한 기능을 제공합니다.

> [TIP]
>
> NHN Cloud  Contact 서비스와 연동해서 사용하면 보다 쉽고 편리하게 고객 문의에 대응할 수 있습니다.
> 자세한 NHN Cloud  Contact 서비스 이용법은 아래 가이드를 참고하시기 바랍니다.
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ko/Contact%20Center/ko/online-contact-overview/)

#### 권한 설정

* [Game > Gamebase > Android SDK 사용 가이드 > ETC > Contact](aos-etc/#contact)
* [Game > Gamebase > iOS SDK 사용 가이드 > ETC > Contact](ios-etc/#contact)


#### Customer Service Type

**Gamebase 콘솔 > App > Customer service**에서는 아래와 같이 3가지 유형의 고객 센터를 선택할 수 있습니다.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud  Online Contact      | O              |

각 유형에 따라 Gamebase SDK 의 고객 센터 API 는 다음 URL 을 사용합니다.

* 개발사 자체 고객 센터(Developer customer center)
    * **고객 센터 URL** 에 입력한 URL.
* Gamebase 제공 고객 센터(Gamebase customer center)
    * 로그인 전 : 유저 정보가 **없는** 고객 센터 URL.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.
* NHN Cloud  조직 상품(Online Contact)
    * 로그인 전 : NOT_LOGGED_IN(2) 에러가 발생.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.

#### Open Contact WebView

고객 센터 웹뷰를 표시합니다.
URL은 고객 센터 유형에 따라 결정됩니다.
ContactConfiguration으로 URL에 추가 정보를 전달할 수 있습니다.

**FGamebaseContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| UserName      | O             | FString                            | 사용자 이름(닉네임) <br>**default**: ""   |
| AdditionalURL | O             | FString                            | 개발사 자체 고객 센터 URL 뒤에 붙는 추가적인 URL <br>**default**: ""    |
| AdditionalParameters | O      | TMap<string, string>               | 고객 센터 URL 뒤에 붙는 추가적인 파라미터<br>**default**: EmptyMap |
| ExtraData     | O             | TMap<FString, FString>             | 개발사가 원하는 extra data를 고객 센터 오픈 시에 전달<br>**default**: EmptyMap |

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
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize 가 호출되지 않았습니다. |
| NOT\_LOGGED\_IN(2)                                  | 고객 센터 유형이 'NHN Cloud  OC' 인데 로그인 전에 호출하였습니다. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 고객 센터 URL 이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL** 을 확인하세요. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 사용자 식별을 위한 임시 티켓 발급에 실패하였습니다. |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | 고객 센터 웹뷰가 이미 표시중입니다. |

**Example**

```cpp
void USample::OpenContact()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetContact()->OpenContact(FGamebaseErrorDelegate::CreateLambda([Subsystem](const FGamebaseError* Error)
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

#### Request Contact URL

고객 센터 웹뷰를 표시하는 데 사용되는 URL을 반환합니다.

**API**

```cs
void RequestContactURL(const FGamebaseContactUrlDelegate& Callback);
void RequestContactURL(const FGamebaseContactConfiguration& Configuration, const FGamebaseContactUrlDelegate& Callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize 가 호출되지 않았습니다. |
| NOT\_LOGGED\_IN(2)                                  | 고객 센터 유형이 'NHN Cloud  OC' 인데 로그인 전에 호출하였습니다. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 고객 센터 URL 이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL** 을 확인하세요. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 사용자를 식별을 위한 임시 티켓 발급에 실패하였습니다. |

**Example**

``` cs
void USample::RequestContactURL(const FString& userName)
{
    FGamebaseContactConfiguration Configuration{ userName };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetContact()->RequestContactURL(Configuration, FGamebaseContactUrlDelegate::CreateLambda([](FString url, const FGamebaseError* Error)
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


### App Tracking AuthorizationStatus

* ATT 활성화 여부를 확인합니다.

* Authorized: 앱의 추적 요청 허용 동의, iOS 14 미만 기기에서는 항상 AUTHORIZED를 반환
* Denied: 앱의 추적 요청 허용 거부
* NotDetermined: 앱의 추적 요청 허용 미결정
* Restricted: 앱의 추적 요청 제한
* Unknown: 다른 OS이거나 OS에서 정의되지 않은 경우

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
UENUM(BlueprintType)
enum class EGamebaseAppTrackingAuthorizationStatus : uint8
{
    Authorized,
    Denied,
    NotDetermined,
    Restricted,
    Unknown
};

EGamebaseAppTrackingAuthorizationStatus GetAppTrackingAuthorizationStatus();
```

**Example**

```cpp
void USample::GetAppTrackingAuthorizationStatus()
{
    UGamebaseSubsystem* GamebaseSubsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    EGamebaseAppTrackingAuthorizationStatus Status = GamebaseSubsystem->GetUtil()->GetAppTrackingAuthorizationStatus();
    
    switch (Status)
    {
    case EGamebaseAppTrackingAuthorizationStatus::Authorized:
        // Authorized
        break;
    case EGamebaseAppTrackingAuthorizationStatus::Denied:
        // Denied
        break;
    case EGamebaseAppTrackingAuthorizationStatus::NotDetermined:
        // Not determined
        break;
    case EGamebaseAppTrackingAuthorizationStatus::Restricted:
        // Restricted
        break;
    case EGamebaseAppTrackingAuthorizationStatus::Unknown:
        // Unknown
        break;
    }
    
}
```

### IDFA

* 단말기의 광고 식별자 값을 반환합니다.

iOS에서 IDFA 기능을 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>
* [iOS IDFA](./ios-etc/#idfa)<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FString GetIdfa();
```

**Example**

```cpp
public void SampleGetIdfa()
{
    UGamebaseSubsystem* GamebaseSubsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    FString Idfa = GamebaseSubsystem->GetUtil()->GetIdfa();
}
```

> <font color="red">[주의]</font><br/>
>
> iOS 14 이상부터 IDFA 값 요청 시, 사용자 권한을 받아야합니다.
> 사용자 권한 요청할 때 노출시킬 문구를 info.plist에 설정을 해야 합니다.
> info.plist에 'Privacy - Tracking Usage Description'을 설정하십시오.

### Age Signals Support

Texas SB 2420 및 유사한 주 법률은 미성년자 보호를 위해 앱에서 사용자의 연령 확인을 요구합니다.
Gamebase는 Google Play Age Signals API를 래핑하여 이러한 요구사항을 충족할 수 있는 API를 제공합니다.

Android에서 Age Signals 기능을 설정하는 방법은 다음 문서를 참고하세요.

* [Android Age Signals](./aos-etc/#age-signals-support)

#### GetAgeSignal

연령 정보를 확인합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void GetAgeSignal(const FGamebaseAgeSignalResultDelegate& Callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_SUPPORTED(10)                   | Android API 23 미만 기기에서 호출되었습니다. |
| AUTH\_EXTERNAL\_LIBRARY\_ERROR(3009) | Google Play Age Signals API에서 오류를 반환했습니다. |

**Handle results**

FGamebaseAgeSignalResult의 UserStatus로 유저의 상태를 확인할 수 있습니다.
Status 값에 따라 사용자 규제 여부를 판단하시기 바랍니다.

**EGamebaseAgeSignalsVerificationStatus**

사용자 검증 상태 상수입니다.

| Status                      | Description          |
| --------------------------- | -------------------- |
| Verified                    | 18세 이상 성인          |
| Supervised                  | 보호자 동의가 있는 미성년자 |
| SupervisedApprovalPending   | 보호자 승인 대기 중       |
| SupervisedApprovalDenied    | 보호자 승인 거부됨        |
| Unknown                     | 검증되지 않은 사용자       |

**Example**

```cpp
void USample::GetAgeSignal()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->GetAgeSignal(
        FGamebaseAgeSignalResultDelegate::CreateLambda([](const FGamebaseAgeSignalResult* AgeSignalResult, const FGamebaseError* Error)
        {
            if (Gamebase::IsSuccess(Error))
            {
                if (!AgeSignalResult->UserStatus.IsSet())
                {
                    // 사용자가 규제 지역(텍사스, 유타, 루이지애나)에 있지 않음을 의미합니다.
                    // 규제 대상이 아닌 사용자에 대한 앱의 로직을 진행할 수 있습니다.
                    UE_LOG(GamebaseTestResults, Display, TEXT("Not legally applicable"));
                }
                else
                {
                    EGamebaseAgeSignalsVerificationStatus UserStatus =
                        static_cast<EGamebaseAgeSignalsVerificationStatus>(AgeSignalResult->UserStatus.GetValue());
                    
                    switch (UserStatus)
                    {
                        case EGamebaseAgeSignalsVerificationStatus::Verified:
                        {
                            // 18세 이상 성인 사용자
                            // 모든 기능에 대한 접근 허용
                            // AgeLower와 AgeUpper는 설정되지 않음
                            UE_LOG(GamebaseTestResults, Display, TEXT("Age 18 or older"));
                            break;
                        }
                        case EGamebaseAgeSignalsVerificationStatus::Supervised:
                        {
                            // 보호자 동의가 있는 미성년자
                            // Texas SB 2420에 따라 미성년자를 위한 제한된 기능 제공
                            
                            // 연령대를 확인할 수 있습니다.
                            if (AgeSignalResult->AgeLower.IsSet() && AgeSignalResult->AgeUpper.IsSet())
                            {
                                int32 AgeLower = AgeSignalResult->AgeLower.GetValue(); // 예: 13
                                int32 AgeUpper = AgeSignalResult->AgeUpper.GetValue(); // 예: 17
                                UE_LOG(GamebaseTestResults, Display, TEXT("Supervised user, age range: %d - %d"), AgeLower, AgeUpper);
                            }

                            if (AgeSignalResult->InstallId.IsSet())
                            {
                                FString InstallId = AgeSignalResult->InstallId.GetValue();
                                UE_LOG(GamebaseTestResults, Display, TEXT("InstallId: %s"), *InstallId);
                            }
                            
                            break;
                        }
                        case EGamebaseAgeSignalsVerificationStatus::SupervisedApprovalPending:
                        {
                            // 보호자 승인을 기다리는 동안 제한된 기능만 제공
                            // 사용자에게 승인 대기 중임을 알림
                            if (AgeSignalResult->MostRecentApprovalDate.IsSet())
                            {
                                int64 ApprovalDate = AgeSignalResult->MostRecentApprovalDate.GetValue();
                                UE_LOG(GamebaseTestResults, Display, TEXT("Approval pending since: %lld"), ApprovalDate);
                            }
                            break;
                        }
                        case EGamebaseAgeSignalsVerificationStatus::SupervisedApprovalDenied:
                        {
                            // 보호자가 승인을 거부한 경우
                            // 제한된 기능만 제공하거나 서비스 이용 불가 안내
                            UE_LOG(GamebaseTestResults, Display, TEXT("Parent or guardian has denied changes"));
                            break;
                        }
                        case EGamebaseAgeSignalsVerificationStatus::Unknown:
                        {
                            // 해당 관할 지역에서 검증되지 않은 사용자 또는 연령 확인 정보를 사용할 수 없는 경우
                            // 사용자에게 Play 스토어를 방문하여 상태를 해결하도록 요청하세요.
                            UE_LOG(GamebaseTestResults, Display, TEXT("User is not verified or supervised"));
                            break;
                        }
                    }
                }
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("GetAgeSignal failed. (errorCode: %d, errorMessage: %s)"), Error->Code, *Error->Message);
                
                if (Error->Code == GamebaseErrorCode::NOT_SUPPORTED)
                {
                    // Android API 23 미만 기기에서는 지원되지 않습니다.
                    UE_LOG(GamebaseTestResults, Display, TEXT("Age Signals API is not supported on this device"));
                }
                else if (Error->Code == GamebaseErrorCode::AUTH_EXTERNAL_LIBRARY_ERROR)
                {
                    // Google Play 서비스에서 오류가 발생했습니다.
                    UE_LOG(GamebaseTestResults, Display, TEXT("Google Play Age Signals error"));
                }
            }
        }));
}
```
