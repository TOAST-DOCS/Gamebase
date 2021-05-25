## Game > Gamebase > User Guide for Unreal SDK > ETC

## Additional Features

This document describes additional features supported by Gamebase. 

### Device Language

* Return the language code configured for your device. 
* When there's many number of registered languagues, return only the language of the highest priority.   

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* You may change language displayed on Gamebase UI or SystemDialog to another one which is different from the configured language on device. 
* Gamebase shows messages that are included to client or received from the server. 
* With DisplayLanguage, messages are displayed in an appropriate language for the user-configured language code (ISO-639). 
* More language sets could be added as required. Following language codes are available: 

> [Note]
>
> Gamebase client messages are available in English (en), Korean (ko), and Japanese (ja) only. 

#### Language Codes Supported by Gamebase

| Code | Name |
| --- | --- |
| de | German |
| en |English  |
| es | Spanish |
| fi | Finish |
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

Each language code is defined in the `GamebaseDisplayLanguageCode` class. 

> <font color="red">[Caution]</font><br/>
>
> Gamebase-supported language codes are case-sensitive. 
> Settings like 'EN' or 'zh-cn' may cause trouble. 

```cpp
namespace GamebaseDisplayLanguageCode
{
    static const FString German(TEXT("de"));
    static const FString English(TEXT("en"));
    static const FString Spanish(TEXT("es"));
    static const FString Finish(TEXT("fi"));
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

#### Display Language Setting for Gamebase Initialization

Display Language can be configured when Gamebase is initialized.  

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

Display Language can be changed from Gamebase initialization. 

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

Current Display Language can be queried.

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

#### Add New Language Sets

Regarding how to add new language sets on Unreal Android or iOS platform, see the following guides:  

* [Adding New Language Sets on Android](./aos-etc#display-language)
* [Addig New Languages Sets on iOS](./ios-etc#display-language)

#### Priority of Display Languages

When Display Language is set by initialization or SetDisplayLanguageCode API, the final value might be different from actual input. 

1. Check if the languageCode input is defined in the localizedstring.json file. 
2. When Gamebase is initialized, see if the language set on device is defined in the localizedstring.json file. (This value, after initialization, shall remain the same even with the change of language set on device.)
3. The default `en` is automatically set for Display Language. 

### Country Code

* Gamebase provides country codes of a system on the following APIs.  
* Each API has different features, so select one to suit your needs. 

#### USIM Country Code

* Return the country code recorded at USIM. 
* Return without further checks even with invalid country code recorded at USIM. 
* If value is empty, return 'zz'. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID


```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* Return the device country code delivered from OS without further checks. 
* Device language code is automatically determined by OS, according to each 'Language' setting. 
* When many languages are registered, a language of the highest priority order is determined as the country code.   
* If the value is empty, return 'zz'. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* Check and return country code in the order of language setting of USIM and device. 
* GetCountryCode API operates in the following order:  
    1. Check country code recorded at USIM, and if a value exists, return it without further checks. 
    2. If USIM country code is empty, check the device country code; if a value exists, return it without further checks.  
    3. If both USIM and device have empty country code, return 'zz'.  

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCode() const;
```

### Gamebase Event Handler

* Gamebase can process all kinds of events in a single event system called **GamebaseEventHandler**.
* GamebaseEventHandler can simply add or remove a Listener through the API below:

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
	// Represents the type of an event.
    // The value of the GamebaseEventCategory class is assigned.
    FString category;

    // JSON String data that can be converted into a VO that is appropriate for each category.
    FString data;
};
```

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
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

* Category is defined in the GamebaseEventCategory class.
* In general, events can be categorized as ServerPush, Observer, Purchase, or Push; GamebaseEventMessage.data can be converted into a VO in the ways shown in the table shown below for each Category.

| Event type | GamebaseEventCategory | VO conversion method | Remarks |
| --------- | --------------------- | ----------- | --- |
| ServerPush | GamebaseEventCategory::ServerPushAppKickOut<br>GamebaseEventCategory::ServerPushTransferKickout | FGamebaseEventServerPushData::From(message.data) | \- |
| Observer | GamebaseEventCategory::ObserverLaunching<br>GamebaseEventCategory::ObserverNetwork<br>GamebaseEventCategory::ObserverHeartbeat | FGamebaseEventObserverData::From(message.data) | \- |
| Purchase - Promotion payment | GamebaseEventCategory::PurchaseUpdated | FGamebaseEventPurchasableReceipt::From(message.data) | \- |
| Push - Message received | GamebaseEventCategory::PushReceivedMessage | FGamebaseEventPushMessage::From(message.data) | Checks whether or not a message was received in the Foreground using the **isForeground** value. |
| Push - Message clicked | GamebaseEventCategory::PushClickMessage | FGamebaseEventPushMessage::From(message.data) | The **isForeground** value does not exist. |
| Push - Action clicked | GamebaseEventCategory::PushClickAction | FGamebaseEventPushAction::From(message.data) | Operates when the RichMessage button is clicked. |


#### Server Push

* This is a message sent from the Gamebase server to the client's device.
* The Server Push Types supported from Gamebase are as follows:
    * GamebaseEventCategory::ServerPushAppKickOut
    	* If you register a kickout ServerPush message in **Operation > Kickout** of the NHN Cloud Gamebase Console, then all clients connected to Gamebase will receive the kickout message.
    * GamebaseEventCategory::ServerPushTransferKickout
    	* If the guest account is successfully transferred to another device, the previous device receives a kickout message.

**Example**

```cpp
void Sample::AddEventHandler()
{
    IGamebase::Get().AddEventHandler(FGamebaseEventDelegate::FDelegate::CreateLambda([=](const FGamebaseEventMessage& message)
    {
        if (message.category.Equals(GamebaseEventCategory::ServerPushAppKickOut) ||
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
        // Return to title and initialize Gamebase again.
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

* It is a system used to handle many different status-changing events in Gamebase.
* The Observer Types supported by Gamebase are as follows:
    * GamebaseEventCategory::ObserverLaunching
    	* It operates when the Launching status is changed, for instance when the server is under maintenance, or the maintenance is over, or a new version is deployed and update is required.
        * GamebaseEventObserverData.code : Indicates the LaunchingStatus value.
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
    	* Operates when the status of a user account changes, for instance when the user account is deleted or banned.
        * GamebaseEventObserverData.code : Indicates the GamebaseError value.
            * GamebaseErrorCode::INVALID_MEMBER: 6
            * GamebaseErrorCode::BANNED_MEMBER: 7
    * GamebaseEventCategory::ObserverNetwork
    	* Can receive the information about the changes in the network.
    	* Operates when the network is disconnected or connected, or switched from Wi-Fi to a cellular network.
        * GamebaseEventObserverData.code : Indicates the NetworkManager value.
            * EGamebaseNetworkType::Not: 255
            * EGamebaseNetworkType::Mobile: 0
            * EGamebaseNetworkType::Wifi: 1
            * EGamebaseNetworkType::Any: 2

**VO**

```cpp
struct GAMEBASE_API FGamebaseEventObserverData
{
	// This information represents the status value.
    int32 code;

    // This information shows the message about status.
    FString message;

    // A reserved field for additional information.
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

* This event is triggered when a product is acquired by redeeming a promotion code.
* Can acquire payment receipt information.

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

* This event is triggered when a push message is received.
* Can determine whether the message is received in the foreground through the **isForeground** field or in the background.
* You can also acquire custom information that was sent along with push by converting the extras field to JSON.

**VO**

```cpp
struct FGamebaseEventPushMessage
{
	// The unique ID of a message.
    FString id;

    // The title of the push message.
    FString title;

    // The body of the push message.
    FString body;

    // You can check all information by converting them to JSONObject.
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
            }
        }
    }));
}
```

#### Push Click Message

* This event is triggered when a received message is clicked.
* Unlike GamebaseEventCategory.PUSH_RECEIVED_MESSAGE, there is no **isForeground** field.

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

* This event is triggered when the button created by the Rich Message feature is clicked.
* actionType provides the following:
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```cpp
struct FGamebaseEventPushAction
{
	// Button action type.
    FString actionType;

	// PushMessage data.
    FGamebaseEventPushMessage message;

	// User text typed in Push console.
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

Game indicators can be sent to Gamebase Server. 

> <font color="red">[Caution]</font><br/>
>
> All APIs supported by Gamebase Analytics can be called after login. 
>
>
> [TIP]
>
> When a purchase is completed by calling RequestPurchase API, indicators are automatically deliverd. 
>

Regarding the usage of Analytics Console, see the following guide:  

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

Level information of a game user can be delivered to indicators, after a user login.  

> <font color="red">[Caution]</font><br/>
>
> If SetGameUserData API is not called after a login to game, level information might be missing from other indicators. 
>

Following parameters are required to call APIs:  

**FGamebaseAnalyticsUserData**

| Name                       | Mandatory(M) / Optional(O) | Type | Desc. |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int32 | Refers to the level of a game user. |
| channelId | O | FString | Indicates a channel. |
| characterId | O | FString | Refers to the name of a character. |
| characterClassId | O | FString | Indicates an occupation. |

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

Updated level information of a game user can be delivered to indicators. 

Following paratemers are required to call APIs:

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | Type | Desc.	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int32 | Refers to the level of a game user. |
| levelUpTime | M | int64 | Enter by Epoch Time.</br> Enter by the millisecond. |

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

Gamebase provides features for customer response. 

> [TIP]
>
> By integrating with NHN Cloud Contact, customer inquiries can be handled with more ease and convenience.  
> For more details on NHN Cloud Contact, see the guide as below: 
> [NHN Cloud Online Contact Guide](/Contact%20Center/en/online-contact-overview/)

#### Customer Service Type

In the **Gamebase Console > App > InApp URL > Service Center**, you can choose from three different types of Customer Centers.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud  Online Contact      | O              |

Gamebase SDK's Customer Center API uses the following URLs based on the type:

* Developer's Customer Center
    * URL specified in the **Customer Center URL** field.
* Gamebase's Customer Center
    * Before login: Customer Center URL **without** user information.
    * After login: Customer Center URL with user information.
* NHN Cloud  organization product (Online Contact)
    * Before login : NOT_LOGGED_IN(2) error has occurred.
    * After login: Customer Center URL with user information.

#### Open Contact WebView

Displays the Customer Center WebView.
URL is determined by the customer center type.
You can pass the additional information to the URL using ContactConfiguration.

**FGamebaseContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | FString                            | User name (nickname) <br>**default** : ""   |
| additionalURL | O             | FString                            | Additional URL which is appended after the developer's own customer center URL <br>**default** : ""    |
| extraData     | O             | TMap<FString, FString>             | Passes the extra data wanted by the developer when opening the customer<br>**default** : EmptyMap |

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
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize has not been called. |
| NOT\_LOGGED\_IN(2)                                  | The customer center type is 'NHN Cloud  OC', and it was called before login. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | Failed to issue a temporary ticket for user identification. |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | The Customer Center WebView is already being displayed. |

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
                // Please check the url field in the NHN Cloud Gamebase Console.
                auto launchingInfo = IGamebase::Get().GetLaunching().GetLaunchingInformations();
                UE_LOG(GamebaseTestResults, Display, TEXT("csUrl: %s"), *launchingInfo->launching.app.relatedUrls.csUrl);
            }
        }
    }));
}
```


> <font color="red">[Caution]</font><br/>
>
> Contacting the Customer Center may require file attachment.
> To do so, permissions for using the camera or using the storage must be acquired from the user at runtime.
>
> Android user
>
> * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> * For Unreal, activate the built-in **Android Runtime Permission** plugin in the engine, and then refer to the following API Reference to acquire necessary permissions.
> [Unreal API Reference : AndroidPermission](https://docs.unrealengine.com/en-US/API/Plugins/AndroidPermission/index.html)
>
> iOS user
>
> * Please set 'Privacy - Camera Usage Description', 'Privacy - Photo Library Usage Description' in info.plist.

#### Request Contact URL

Returns the URL used for displaying the Customer Center WebView.

**API**

```cs
void RequestContactURL(const FGamebaseContactUrlDelegate& onCallback);
void RequestContactURL(const FGamebaseContactConfiguration& configuration, const FGamebaseContactUrlDelegate& onCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize has not been called. |
| NOT\_LOGGED\_IN(2)                                  | The customer center type is 'NHN Cloud  OC', and it was called before login. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | Failed to issue a temporary ticket for user identification. |

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
                // Please check the url field in the NHN Cloud Gamebase Console.
            }
            else
            {
                // An error occur when requesting the contact web view url.
            }
        }
    }));
}
```
