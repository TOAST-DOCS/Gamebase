## Game > Gamebase > Unreal SDK使用指南 > ETC

## Additional Features

描述Gamebase支持的附加功能。

### Device Language

* 返还终端机设置的语言代码。
* 注册多个语言时，返还优先权最高的语言。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
 
```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* 可以将Gamebase提供的UI和在SystemDialog中显示的语言更改为终端机设置的语言之外的其他语言。 
* Gamebase显示包含在客户端的消息或从服务器接收的消息。
* 设置DisplayLanguage时，以符合用户设置的语言代码(ISO-639)的语言显示消息。 
* 可以添加所需的语言集合。可以添加的语言代码如下。

> [参考]
>
> Gamebase的客户端消息只包含英语(en)、韩语(ko)及日语(ja)。

#### Gamebase支持的语言代码种类

| Code | Name |
| --- | --- |
| de | German |
| en |English  |
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

相关语言代码在“GamebaseDisplayLanguageCode”类中定义。

> <font color="red">[注意]</font><br/>
>
> Gamebase支持的语言代码区分大小写字母。
> 若按“EN”或“zh-cn”进行设置，可能出现问题。

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

#### 初始化Gamebase时的Display Language设置

初始化Gamebase时可以设置Display Language。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Initialize(const FGamebaseConfiguration& configuration, const FGamebaseLaunchingInfoDelegate& onCallback);
```

**Example**

```cpp
void Sample::Initialize(const FString& appID, const FString& appVersion)
{
    FGamebaseConfiguration configuration;
    ...
    configuration.displayLanguageCode = displayLanguage;
    ...

    IGamebase::Get().Initialize(configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([=](const FGamebaseLaunchingInfo* launchingInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
            FString displayLanguage = IGamebase::Get().GetDisplayLanguageCode();
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

#### Set Display Language

初始化Gamebase时可更改已输入的Display Language。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void SetDisplayLanguageCode(const FString& languageCode);
```

**Example**

```cpp
void Sample::SetDisplayLanguageCode(cosnt FString& displayLanguage)
{
    FString displayLanguage = IGamebase::Get().SetDisplayLanguageCode(displayLanguage);
}
```

#### Get Display Language

可以查看当前设置的Display Language。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetDisplayLanguageCode() const;
```

**Example**

```cpp
void Sample::GetDisplayLanguageCode()
{
    FString displayLanguage = IGamebase::Get().GetDisplayLanguageCode();
}
```

#### 添加新语言集合

有关在Unreal Android、iOS平台上添加新语言集合的方法，请参考以下指南。

* [添加Android的新语言集合](./aos-etc#display-language)
* [添加iOS的新语言集合](./ios-etc#display-language)

#### Display Language的优先顺序

通过初始化或调用SetDisplayLanguageCode API设置Display Language时，最终适用的Display Language值与输入的值不同。

1. 确认输入到的languageCode是否在localizedstring.json文件中定义。
2. 初始化Gamebase时，确认终端机设置的语言代码是否在localizedstring.json文件中定义。( 一旦初始化DisplayLanguageCode值，即使终端机设置的语言被更改，也会保持初始值。）                          
3. 自动设置Display Language的默认值“en”。  

### Country Code

* Gamebase使用以下API提供System的国家代码。 
* 每个API都有不同的特征，请选择符合用途的API。

#### USIM Country Code

* 返还USIM中存储的国家代码。
* 即使USIM中的值当中有错误的国家代码，也不再确认，而直接返还。
* 如果为空值，则返还“ZZ”。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
 
```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* 不再进一步确认，直接返还从OS接收的终端机国家代码。                
* OS按照“语言”设置自动选择终端机的国际代码。
* 注册的语言为多个语言时，选择优先权最高的语言。
* 如果为空值，则返还“ZZ”。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* 按照USIM、终端机的语言设置顺序确认国家代码后返还。 
* GetCountryCode API按以下顺序启动。 
    1. 如果USIM中的国家代码当中存在值，则不再确认，而直接返还。
    2. 如果USIM国家代码为空值，需要确认终端机国家代码，若存在值，则不再确认，而直接返还。
    3. 若USIM、终端机国家代码为空值，则返还“ZZ”。 

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp 
FString GetCountryCode() const;
```

### Gamebase Event Handler

* Gamebase通过**GamebaseEventHandler**事件系统处理各种事件。 
* GamebaseEventHandler通过以下API添加或删除Listener。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cs
FDelegateHandle AddHandler(const FGamebaseEventDelegate::FDelegate& onCallback);
void RemoveHandler(const FDelegateHandle& handle);
void RemoveAllHandler();
```

**VO**

```cpp
struct GAMEBASE_API FGamebaseEventMessage
{
    // 显示Event种类。
    // 分配GamebaseEventCategory类的值。 
    FString category;

    // 是可以转换为符合各category的VO的JSON String数据。
    FString data;
};
```

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::LoggedOut))
        {
            auto loggedOutData = FGamebaseEventLoggedOutData::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
                 message.category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived) ||
                 message.category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
        {
            auto serverPushData = FGamebaseEventServerPushData::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::ObserverLaunching))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::ObserverNetwork))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::ObserverHeartbeat))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::PurchaseUpdated))
        {
            auto purchasableReceipt = FGamebaseEventPurchasableReceipt::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::PushReceivedMessage))
        {
            auto pushMessage = FGamebaseEventPushMessage::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::PushClickMessage))
        {
            auto pushMessage = FGamebaseEventPushMessage::From(message.data);
        }
        else if (message.category.Equals(GamebaseEventCategory::PushClickAction))
        {
            auto pushAction = FGamebaseEventPushAction::From(message.data);
        }
    }));
}
```

* Category在GamebaseEventCategory类中定义。 
* 事件大体分为LoggedOut、ServerPush、Observer、Purchase及Push，根据各Category将GamebaseEventMessage.data按以下表的方式转换为VO。 

| Event种类 | GamebaseEventCategory | VO转换方法 | 备注 |
| --------- | --------------------- | ----------- | --- |
| LoggedOut | GamebaseEventCategory::LoggedOut | FGamebaseEventLoggedOutData::From(message.data) | \- |
| ServerPush | GamebaseEventCategory::ServerPushAppKickOut<br>GamebaseEventCategory::ServerPushAppKickOutMessageReceived<br>GamebaseEventCategory::ServerPushTransferKickout | FGamebaseEventServerPushData::From(message.data) | \- |
| Observer | GamebaseEventCategory::ObserverLaunching<br>GamebaseEventCategory::ObserverNetwork<br>GamebaseEventCategory::ObserverHeartbeat | FGamebaseEventObserverData::From(message.data) | \- |
| Purchase - Promotion支付 | GamebaseEventCategory::PurchaseUpdated | FGamebaseEventPurchasableReceipt::From(message.data) | \- |
| Push - 接收消息 | GamebaseEventCategory::PushReceivedMessage | FGamebaseEventPushMessage::From(message.data) | |
| Push - 点击消息 | GamebaseEventCategory::PushClickMessage | FGamebaseEventPushMessage::From(message.data) | |
| Push - 动态点击 | GamebaseEventCategory::PushClickAction | FGamebaseEventPushAction::From(message.data) | 点击RichMessage时启动。|

#### Logged Out

* 当Gamebase Access Token过期，需要登录函数调用来恢复网络会话时，会引发此事件。

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
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::LoggedOut))
        {
            auto loggedOutData = FGamebaseEventLoggedOutData::From(message.data);
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

* 是从Gamebase服务器向客户端终端机传送的消息。 
* Gamebase支持的Server Push Type如下。
  * GamebaseEventCategory::ServerPushAppKickOutMessageReceived
    	* 如果在NHN Cloud Gamebase控制台中的**Operation > Kickout**中注册Kickout ServerPush消息，则与Gamebase连接的所有 客户端接收Kickout消息。
        * 是在客户终端机接收服务器消息时立即启动的事件。
        * 它可以用于在游戏运行时暂停游戏，例如“AutoPlay”。
    * GamebaseEventCategory::ServerPushAppKickOut
        * 通过在NHN Cloud Gamebase控制台的**Operation > Kickout**中注册Kickout ServerPush消息，将从与Gamebase连接的所有客户端接收Kickout消息。
       * 在客户终端机接收服务器消息时显示弹窗，而当用户关闭该弹窗时启动此事件。 
    * GamebaseEventCategory::ServerPushTransferKickout
        * 将Guest账户成功转移至其他终端机时，从以前的终端机接收Kickout消息。

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
            message.category.Equals(GamebaseEventCategory::ServerPushAppKickOutMessageReceived) ||
            message.category.Equals(GamebaseEventCategory::ServerPushTransferKickout))
        {
            auto serverPushData = FGamebaseEventServerPushData::From(message.data);
            if (serverPushData.IsVaild())
            {
                CheckServerPush(message.category, *serverPushData);
            }
        }
    }));
}

void Sample::CheckServerPush(const FString& category, const FGamebaseEventServerPushData& data)
{
    if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut))
    {
        // Kicked out from Gamebase server.(Maintenance, banned or etc..)
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

  

* 是处理Gamebase的各种状态变动事件的系统。 
* Gamebase支持的Observer Type如下。
    * GamebaseEventCategory::ObserverLaunching
        * 当维护开始、结束时或发布新版本必须进行更新等Launching状态出现变动时启动。
        * GamebaseEventObserverData.code : 为LaunchingStatus值。
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
        * 当因已被退出或禁用、用户账号状态出现变化时启动。
        * GamebaseEventObserverData.code : 为GamebaseError值。
            * GamebaseErrorCode::INVALID_MEMBER: 6
            * GamebaseErrorCode::BANNED_MEMBER: 7
    * GamebaseEventCategory::ObserverNetwork
        * 可以接收网络变动信息。 
        * 当网络断开或被连接时、从Wifi转为Cellular网络时启动。
        * GamebaseEventObserverData.code : 为NetworkManager值。 
            * EGamebaseNetworkType::Not: 255
            * EGamebaseNetworkType::Mobile: 0
            * EGamebaseNetworkType::Wifi: 1
            * EGamebaseNetworkType::Any: 2

**VO**

```cpp
struct GAMEBASE_API FGamebaseEventObserverData
{
    // 为显示状态值的信息。
    int32 code;

    // 是用于附加信息的保留字段。
    FString message;

    // 是有关状态的消息信息。 
    FString extras;
}
```

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::ObserverLaunching))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
            if (observerData.IsVaild())
            {
                CheckLaunchingStatus(*observerData);
            }
        }
        else if (message.category.Equals(GamebaseEventCategory::ObserverNetwork))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
            if (observerData.IsVaild())
            {
                CheckNetwork(*observerData);
            }
        }
        else if (message.category.Equals(GamebaseEventCategory::ObserverHeartbeat))
        {
            auto observerData = FGamebaseEventObserverData::From(message.data);
            if (observerData.IsVaild())
            {
                CheckHeartbeat(*observerData);
            }
        }
    }));
}

void Sample::CheckLaunchingStatus(const FGamebaseEventObserverData& data)
{
    switch (data.code)
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

void Sample::CheckNetwork(const FGamebaseEventObserverData& data)
{
    switch ((GamebaseNetworkType)data.code)
    {
        case EGamebaseNetworkType::Not:
            {
                // Network disconnected
                break;
            }
        case EGamebaseNetworkType::Mobile:
            {
                // Network connected
                break;
            }
        case EGamebaseNetworkType::Wifi:
            {
                // Network connected
                break;
            }
        case EGamebaseNetworkType::Any:
            {
                // Network connected
                break;
            }
    }
}

void Sample::CheckHeartbeat(const FGamebaseEventObserverData& data)
{
    switch (data.code)
    {
        case EGGamebaseErrorCode::INVALID_MEMBER:
            {
                // You should to write the code necessary in game. (End the session.)
                break;
            }
        case EGGamebaseErrorCode::BANNED_MEMBER:
            {
                // The ban information can be found by using the GetBanInfo API.
                // Show kickout message to user and need kickout in game.
                break;
            }
    }
}
```

#### Purchase Updated

* 是输入Promotion代码获取商品时出现的事件。
* 可以获取结算票据信息。

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::PurchaseUpdated))
        {
            auto purchasableReceipt = FGamebaseEventPurchasableReceipt::From(message.data);
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

* 是接收Push消息时出现的事件。
* 通过将extras字段转换为JSON，可获取发送Push时传送的自定义信息。
    *  在**Android**上可通过**isForeground**字段可区分是在Foreground状态还是在Backgroud状态接收的消息。 
        
**VO**

```cpp
struct FGamebaseEventPushMessage
{
    // 为消息的固有id。
    FString id;

    // 为Push消息的标题。 
    FString title;  

    // 为Push消息的身体。
    FString body;

    // 可以确认以JSON格式发送Push时传送的自定义信息。 
    FString extras;
};
```

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::PushReceivedMessage))
        {
            auto pushMessage = FGamebaseEventPushMessage::From(message.data);
            if (pushMessage.IsVaild())
            {
                // When you clicked push message.

                // By converting the extras field of the push message to JSON,
                // you can get the custom information added by the user when sending the push.
                // (For Android, an 'isForeground' field is included so that you can check if received in the foreground state.)
            }
        }
    }));
}
```

#### Push Click Message

* 是点击“已接收的Push消息”时出现的事件。
* 与“GamebaseEventCategory::PushReceivedMessage”不同，Android上的extras字段中没有**isForeground**信息。

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::PushClickMessage))
        {
            auto pushMessage = FGamebaseEventPushMessage::From(message.data);
            if (pushMessage.IsVaild())
            {
                // When you clicked push message.
            }
        }
    }));
}
```


#### Push Click Action

* 是通过Rich Message功能，点击生成按钮时出现的事件。
* actionType中存在以下值。
    * "OPEN_APP"
    * "OPEN_URL"
    * "REPLY"
    * "DISMISS"


**VO**

```cpp
struct FGamebaseEventPushAction
{
    // 为ButtonAction种类。 
    FString actionType;

    // 为PushMessage数据。
    FGamebaseEventPushMessage message;

    // 为在Push控制台中输入的用户文本。
    FString userText;
};
```

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::PushClickAction))
        {
            auto pushAction = FGamebaseEventPushAction::From(message.data);
            if (pushAction.IsVaild())
            {
                // When you clicked action button by 'Rich Message'.
            }
        }
    }));
}
```


### Analytics

可将Game指标传送到Gamebase Server。

> <font color="red">[注意]</font><br/>
>
> 登录后可以调用Gamebase Analytics支持的所有API。
>
>
> [TIP]
>
> 调用RequestPurchase API完成支付时，自动传送指标。
>

有关Analytics Console的使用方法，请参考以下指南。

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

登录游戏后，可将游戏用户级别信息传送到指标。

> <font color="red">[注意]</font><br/>
>
> 登录游戏后，若不调用SetGameUserData API，其他指标可能缺漏Level信息。
>

调用API时需要的参数如下。

**FGamebaseAnalyticsUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int32 | 是显示游戏用户级别的字段。|
| channelId | O | FString | 是显示渠道的字段。|
| characterId | O | FString | 是显示游戏人物名称的字段。|
| characterClassId | O | FString | 是显示职业的字段。|

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void SetGameUserData(const FGamebaseAnalyticsUserData& gameUserData);
```

**Example**

```cpp
void Sample::SetGameUserData(int32 userLevel, const FString& channelId, const FString& characterId, const FString& characterClassId)
{
    FGamebaseAnalyticsUserData gameUserData{ userLevel, channelId, characterId, characterClassId };
    IGamebase::Get().GetAnalytics().SetGameUserData(gameUserData);
}

```

#### Level Up Trace

升级时，可以将游戏用户的级别信息传送到指标。

调用API时所需的参数如下。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc    |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int32 | 是显示游戏用户级别的字段。|
| levelUpTime | M | long | 按Epoch time输入。</br>按Millisecond单位输入。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void TraceLevelUp(const FGamebaseAnalyticesLevelUpData& levelUpData);
```

**Example**

```cpp
void Sample::TraceLevelUpNow(int32 userLevel)
{
    FGamebaseAnalyticesLevelUpData levelUpData{ userLevel, FDateTime::Now().ToUnixTimestamp() };
    IGamebase::Get().GetAnalytics().TraceLevelUp(levelUpData);
}
```

### Contact

Gamebase提供回复客户咨询的功能。 

> [TIP]
>
> 若与NHN Cloud Contact服务联动后使用，则可更容易应对客户咨询。
> 有关详细的NHN Cloud Contact服务使用方法，请参考以下指南。
> [NHN Cloud Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)


#### Customer Service Type

**在Gamebase控制台 > App > InApp URL > Service center**中可以选择以下3个类型中的一个客户服务。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud  Online Contact      | O              |

Gamebase SDK的客户服务API根据各类型使用如下URL。

* 开发公司自建客户服务(Developer customer center)
    * 在**客户服务URL**输入的URL
* Gamebase提供的客户服务(Gamebase customer center)
    * 登录前 : **不包含**用户信息的客户服务URL
    * 登录后 : 包含用户信息的客户服务URL
* NHN Cloud组织服务(Online Contact)
    * 登录前 : **不包含**用户信息的客户服务URL
    * 登录后 : 包含用户信息的客户服务URL


#### Open Contact WebView


显示客户服务WebView。
根据客户服务类型选择URL。
可通过ContactConfiguration向URL传送附加信息。

**FGamebaseContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | FString                            | 用户名(nickname) <br>**default**: ""   |
| additionalURL | O             | FString                            | 在开发公司客户服务URL后面添加的附加URL <br>**default**: ""    |
| additionalParameters | O      | TMap<string, string>               | 客户服务URL后面添加的附加参数<br>**default** : EmptyMap |
| extraData     | O             | TMap<FString, FString>             | 开始客户服务时传送开发公司需要的extra data。<br>**default**: EmptyMap |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS 
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void OpenContact(const FGamebaseErrorDelegate& onCloseCallback);
void OpenContact(const FGamebaseContactConfiguration& configuration, const FGamebaseErrorDelegate& onCloseCallback);
```

**ErrorCode**

| Error Code | Description |                     
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize。|
| NOT\_LOGGED\_IN(2)                                  | 客户服务类型为“NHN Cloud  OC”， 但在登录之前已被调用。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客户服务URL不存在。<br>请确认Gamebase控制台中的**客户服务URL**。|
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 识别用户的临时发票发布失败 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | 已经显示客户服务WebView。|

**Example**

```cpp
void Sample::OpenContact()
{
    IGamebase::Get().GetContact().OpenContact(FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("OpenContact succeeded."));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("OpenContact failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);

            if (error->code == GamebaseErrorCode::WEBVIEW_INVALID_URL)
            {
                // Gamebase Console Service Center URL is invalid.
                // Please check the url field in the TOAST Gamebase Console.
                auto launchingInfo = IGamebase::Get().GetLaunching().GetLaunchingInformations();
                UE_LOG(GamebaseTestResults, Display, TEXT("csUrl: %s"), *launchingInfo->launching.app.relatedUrls.csUrl);
            }
        }
    }));
}
```


> <font color="red">[注意]</font><br/>
>
> 在客户服务注册咨询时可能需要附加文件。 
> 为此，需要在运行时从用户获取拍照和Storage储存权限。
>
> Android用户
>
> * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> * 使用Unreal时，通过启用内置于引擎中的**Android Runtime Permission**plug-in，查看以下API Reference后在获取所需的权限时参考。 
> [Unreal API Reference : AndroidPermission](https://docs.unrealengine.com/en-US/API/Plugins/AndroidPermission/index.html)
>
> iOS用户
>
> * 请在info.plist中设置“Privacy - Camera Usage Description”和“Privacy - Photo Library Usage Description”。 

#### Request Contact URL

返还显示客户服务WebView时使用的URL。 

**API**

```cs
void RequestContactURL(const FGamebaseContactUrlDelegate& onCallback);
void RequestContactURL(const FGamebaseContactConfiguration& configuration, const FGamebaseContactUrlDelegate& onCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize。|
| NOT\_LOGGED\_IN(2)                                  | 客户服务类型为“NHN Cloud  OC”，但在登录之前已被调用。|
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客户服务URL不存在。<br>请确认Gamebase控制台中的**客户服务URL**。|
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 识别用户的临时发票发放失败 |

**Example**

``` cs
void Sample::RequestContactURL(const FString& userName)
{
    FGamebaseContactConfiguration configuration{ userName };

    IGamebase::Get().GetContact().RequestContactURL(configuration, FGamebaseContactUrlDelegate::CreateLambda([=](FString url, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Open webview with 'contactUrl'
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestContactURL succeeded. (url = %s)"), *url);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestContactURL failed. (errorCode: %d, errorMessage: %s)"), error->code, *error->message);

            if (error->code == GamebaseErrorCode::UI_CONTACT_FAIL_INVALID_URL)
            {
                // Gamebase Console Service Center URL is invalid.
                // Please check the url field in the TOAST Gamebase Console.
            }
            else
            {
                // An error occur when requesting the contact web view url.
            }
        }
    }));
}
```