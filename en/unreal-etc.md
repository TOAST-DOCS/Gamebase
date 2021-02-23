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

### Server Push
* The Gamebase server can process Server Push Messages sent onto a client device. 
* Add ServerPushEvent Listener to Gamebase client, and the user may receive to process the message and may delete the added ServerPushEvent Listener.


#### Server Push Type
Gamebase currently supports the following server push type: 

* GamebaseServerPushType::AppKickout (= "appKickout")
    * When a kickout ServerPush message is registered at **Operation > Kickout** on a NHN Cloud Gamebase console, and you'll receive **APP_KICKOUT** message on all clients connected to Gamebase. 

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
By registering ServerPushEvent for Gamebase Client, push events issued by Gamebase console or server can be processed. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FDelegateHandle AddServerPushEvent(const FGamebaseServerPushDelegate::FDelegate& event);
```

**Example**

```cpp
void Sample::AddServerPushEvent()
{
    FDelegateHandle handle = IGamebase::Get().AddServerPushEvent(FGamebaseServerPushDelegate::FDelegate::CreateLambda([=](const FGamebaseServerPushMessage& message)
    {
        if (message.type.Equals(GamebaseServerPushType::AppKickout))
        {
            // Logout
            // Go to Main
        }
        else if (message.type.Equals(GamebaseServerPushType::TransferKickout))
        {
            // Logout
            // Go to Main
        }
    }));
}
```


#### Remove ServerPushEvent
You can delete ServerPushEvent registered at Gamebase. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RemoveServerPushEvent(const FDelegateHandle& handle);
void RemoveAllServerPushEventerPushEvent();
```

**Example**

```cpp
void Sample::RemoveServerPushEvent(const FDelegateHandle& handle)
{
    IGamebase::Get().RemoveServerPushEvent(handle);
}

void Sample::RemoveAllServerPushEvent()
{
    IGamebase::Get().RemoveAllServerPushEvent();
}
```

### Observer
* With Gamebase Observer, Gamebase status change events can be delivered and processed. 
* Status Change Events: Change of network type, launching status (due to maintenance, and etc.), or heartbeat information (e.g. change of heartbeat information due to user banned), and others 

#### Observer Type
Gamebase supports the following observer types: 

* Change of Network Types 
    * You can get information on network changes. 
    * Type: GamebaseObserverType::Network (= "network")
    * Code: See constant value declared at each GamebaseNetworkType. 
        * GamebaseNetworkType::TYPE_NOT: 255
        * GamebaseNetworkType::TYPE_MOBILE: 0
        * GamebaseNetworkType::TYPE_WIFI: 1
        * GamebaseNetworkType::TYPE_ANY: 2
* Change of Launching Status 
    * Occurs when there is a change in the Launching Status Response which periodically checks application status. For instance, event occurrences due to maintenance, or update recommendation. 
    * Type: GamebaseObserverType::Launching (= "launching")
    * Code: See constant value declared at each GamebaseLaunchingStatus.
        * GamebaseLaunchingStatus::IN_SERVICE: 200
        * GamebaseLaunchingStatus::RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus::IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus::REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus::BLOCKED_USER: 301
        * GamebaseLaunchingStatus::TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus::INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus::INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR: 500
* Change of Heartbeat Information 
    * Occurs when there is change in the heartbeat response which periodically stays connected to Gamebase server. For instance, event occurrence due to banning on user. 
    * Type: GamebaseObserverType::Heartbeat (= "heartbeat")
    * Code: See constant value declared at each GamebaseErrorCode.
        * GamebaseErrorCode::INVALID_MEMBER: 6
        * GamebaseErrorCode::BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
By registering an observer at the Gamebase client, status change events can be processed. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
static void AddObserver(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ObserverMessage> observer)
```

**Example**

```cpp
void Sample::AddObserver()
{
    FDelegateHandle handle = IGamebase::Get().AddObserver(FGamebaseObserverDelegate::FDelegate::CreateLambda([=](const FGamebaseObserverMessage& message)
    {
        if (message.type.Equals(GamebaseObserverType::Network))
        {
            // Code : Refer to GamebaseLaunchingStatus.
            int code        = (int)data["code"];

            // Message
            string message  = (string)data["message"];
        }
        else if (message.type.Equals(GamebaseObserverType::Launching))
        {
            // Code: Refer to GamebaseNetworkType.
            int code        = (int)data["code"];

            // Message
            string message  = (string)data["message"];
        }
        else if (message.type.Equals(GamebaseObserverType::Heartbeat))
        {
            // Code : GamebaseErrorCode.INVALID_MEMBER, GamebaseErrorCode.BANNED_MEMBER
            int code = (int)data["code"];
        }
    }));
}
```

#### Remove Observer
Oberservers registered at Gamebase can be removed. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RemoveObserver(const FDelegateHandle& handle);
void RemoveAllObserver();
```

**Example**

```cpp
void Sample::RemoveObserver(const FDelegateHandle& handle)
{
    IGamebase::Get().RemoveObserver(handle);
}

void Sample::RemoveAllObserver()
{
    IGamebase::Get().RemoveAllObserver();
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

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | Type | Desc. |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | Refers to the level of a game user. |
| channelId | O | string | Indicates a channel. |
| characterId | O | string | Refers to the name of a character. |
| characterClassId | O | string | Indicates an occupation. |

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
| userLevel | M | int | Refers to the level of a game user. |
| levelUpTime | M | long | Enter by Epoch Time.</br> Enter by the millisecond. |

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

#### Open Contact WebView

Shows the webview of **Customer Center URL** entered on Gamebase console. 

* Value is applied same as **Gamebase Console > App > InApp URL > Service Center**.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void OpenContact(const FGamebaseErrorDelegate& onCloseCallback);
```

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
                Debug.Log(string.Format("csUrl:{0}", launchingInfo->launching.app.relatedUrls.csUrl));
            }

        }
    }));
}
```
