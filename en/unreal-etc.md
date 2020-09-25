## Game > Gamebase > User Guide for Unreal SDK 사용 가이드 > ETC

## Additional Features

This document describes additional features supported by Gamebase. 에서 지원하는 부가 기능을 설명합니다.

### Device Language

* Return the language code configured for your device. 단말기에 설정된 언어 코드를 리턴합니다.
* When there's many number of registered languagues, return only the language of the highest priority.     여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* You may change language displayed on Gamebase UI or SystemDialog to another one which is different from the configured language on device.  Gamebase에서 제공하는 UI 및 SystemDialog에 표시되는 언어를 단말기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.
* Gamebase shows messages that are included to client or received from the server. 는 클라이언트에 포함되어 있는 메시지를 표시하거나 서버에서 받은 메시지를 표시합니다.
* With DisplayLanguage, messages are displayed in an appropriate language for the user-configured language code (ISO-639). 를 설정하면 사용자가 설정한 언어코드(ISO-639)에 적합한 언어로 메시지를 표시합니다.
* More language sets could be added as required. Following language codes are available: 원하는 언어셋을 추가할 수 있습니다. 추가할 수 있는 언어코드는 다음과 같습니다.

> [Note 참고]
>
> Gamebase client messages are available in English (en), Korean (ko), and Japanese (ja). 의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함합니다.

#### Language Codes Supported by Gamebase에서 지원하는 언어코드의 종류

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

Each language code is defined in the `GamebaseDisplayLanguageCode` class. 클래스에 정의되어 있습니다.

> <font color="red">[Caution]</font><br/>
>
> Gamebase-supported language codes are case-sensitive. Gamebase에서 지원하는 언어코드는 대소문자를 구분합니다.
> Settings like 'EN' or이나 'zh-cn' may cause trouble. 과 같이 설정하면 문제가 발생할 수 있습니다.

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

#### Display Language Setting for Gamebase Initialization 초기화 시 Display Language 설정

Display Language can be set when Gamebase is initialized.  초기화 시 Display Language를 설정할 수 있습니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

Display Language can be changed from Gamebase initialization. Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

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

Current Display Language can be queried. 현재 적용된 Display Language를 조회할 수 있습니다.

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

#### 신규 언어셋 추가 Add New Language Sets

Regarding how to add new language sets on Unreal Android or iOS platform, see the following guides:  플랫폼에서의 신규 언어셋 추가 방법은 아래 가이드를 참고하십시오.

* [Adding New Language Sets on Android 신규 언어셋 추가](./aos-etc#display-language)
* [Addig New Languages Sets on iOS 신규 언어셋 추가](./ios-etc#display-language)

#### Priority of Display Language 우선순위

When Display Language is set by initialization or 초기화 및 SetDisplayLanguageCode API, the final value might be different from actual input. 를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. Check if input 입력된 languageCode is defined in the가 localizedstring.json file. 파일에 정의되어 있는지 확인합니다.
2. When Gamebase is initialized, see if the language set on device is defined in the  초기화 시, 단말기에 설정된 언어코드가 localizedstring.json file. 파일에 정의되어 있는지 확인합니다.(This value, after initialization, shall remain the same even with the change of language set on device. 이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인The default `en` is automatically set for Display Language. 이 자동으로 설정됩니다.

### Country Code

* Gamebase provides country codes of a system on the following APIs. 는 System의 국가 코드를 다음과 같은 API로 제공하고 있습니다. 
* Each API has different features, so select one to suit your needs. 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가 코드를 리턴합니다. Return the country code recorded at USIM. 
* USIM에 잘못된 국가 코드가 기록되어 있다 하더라도 추가적인 체크 없이 그대로 리턴합니다. Return without further checks even with invalid country code recorded at USIM. 
* 값이 비어있는 경우 'ZZ'를 리턴합니다.If value is empty, return 'zz'. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID


```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* OS로부터 전달받은 단말기 국가 코드를 추가적인 체크 없이 그대로 리턴합니다. Return the device country code delivered from OS without further checks. 
* 단말기 국가 코드는 '언어' 설정에 따라 OS가 자동으로 결정합니다. Device language code is automatically determined by OS, according to each 'Language' setting. 
* 여러 개의 언어가 등록된 경우, 우선권이 가장 높은 언어로 국가 코드를 결정합니다. When many languages are registered, a language of the highest priority order is determined as the country code.   
* 값이 비어있는 경우 'ZZ'를 리턴합니다. If the value is empty, return 'zz'. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* USIM, 단말기 언어 설정의 순서로 국가 코드를 확인하여 리턴합니다.Check and return country code in the order of language setting of USIM and device. 
* GetCountryCode API는 다음 순서로 동작합니다. operates in the following order:  
    1. Check country code recorded at USIM, and if a value exists, return it without further checks. 에 기록된 국가 코드를 확인하고, 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
    2. If USIM country code is empty, check the device country code; if a value exists, return it without further checks.  국가 코드가 빈 값이라면 단말기 국가 코드를 확인하고, 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
    3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 리턴합니다. If both USIM and device have empty value of country code, return 'zz'.  

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCode() const;
```

### Server Push
* Gamebase 서버에서 클라이언트 단말기로 보내는 Server Push Message를 처리할 수 있습니다. The Gamebase server can process Server Push Messages sent to a client device. 
* Gamebase 클라이언트에서 ServerPushEvent Listener를 추가하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent Listener를 삭제할 수 있습니다.


#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다. Gamebase currently supports the following server push type: 

* GamebaseServerPushType::AppKickout (= "appKickout")
    * TOAST Gamebase 콘솔의 When a kickout ServerPush message is registered at **Operation > Kickout**, and you receive **APP_KICKOUT** message on all clients connected to Gamebase. 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 **APP_KICKOUT** 메시지를 받게 됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
By registering ServerPushEvent for Gamebase Client, push events issued by Gamebase console or server can be processed. 에 ServerPushEvent를 등록하여 Gamebase Console 및 Gamebase 서버에서 발급된 Push 이벤트를 처리할 수 있습니다

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
You can delete ServerPushEvent registered at Gamebase. 에 등록된 ServerPushEvent를 삭제할 수 있습니다.

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
* With Gamebase Observer, Gamebase status change events can be delivered and processed. 로 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* Status Change Events상태 변동 이벤트 : Change of network type, launching status (due to maintenance, and etc.), or heartbeat information, and others 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등

#### Observer Type
Gamebase supports the followint observer types: 현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.

* Network 타입 변동 Change of Network Types 
    * 네트워크 변동 사항 정보를 받을 수 있습니다. You can get information on network changes. 
    * Type: GamebaseObserverType::Network (= "network")
    * Code: GamebaseNetworkType에 선언된 상수를 참고합니다.
        * GamebaseNetworkType::TYPE_NOT: 255
        * GamebaseNetworkType::TYPE_MOBILE: 0
        * GamebaseNetworkType::TYPE_WIFI: 1
        * GamebaseNetworkType::TYPE_ANY: 2
* Launching 상태 변동 Change of Launching Status 
    * 주기적으로 애플리케이션 상태를 확인하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어 점검, 업데이트 권장 등에 의한 이벤트가 있습니다.
    * Type: GamebaseObserverType::Launching (= "launching")
    * Code: GamebaseLaunchingStatus에 선언된 상수를 참고합니다.
        * GamebaseLaunchingStatus::IN_SERVICE: 200
        * GamebaseLaunchingStatus::RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus::IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus::REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus::BLOCKED_USER: 301
        * GamebaseLaunchingStatus::TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus::INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus::INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR: 500
* Heartbeat 정보 변동 Change of Heartbeat Information 
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어 사용자 이용 정지에 의한 이벤트가 있습니다.
    * Type: GamebaseObserverType::Heartbeat (= "heartbeat")
    * Code: GamebaseErrorCode에 선언된 상수를 참조합니다.
        * GamebaseErrorCode::INVALID_MEMBER: 6
        * GamebaseErrorCode::BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase Client에 Observer를 등록하여 각종 상태 변동 이벤트를 처리할 수 있습니다. By registering an observer at the Gamebase client, status change events can be processed. 

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
Gamebase에 등록된 Observer를 삭제할 수 있습니다.

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

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 게임 유저 레벨을 나타내는 필드입니다. |
| channelId | O | string | 채널을 나타내는 필드입니다. |
| characterId | O | string | 캐릭터 이름을 나타내는 필드입니다. |
| characterClassId | O | string | 직업을 나타내는 필드입니다. |

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

레벨업이 되었을 경우 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 게임 유저 레벨을 나타내는 필드입니다. |
| levelUpTime | M | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |

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

Gamebase 는 고객 문의 대응을 위한 기능을 제공합니다.

> [TIP]
>
> TOAST Contact 서비스와 연동해서 사용하면 보다 쉽고 편리하게 고객 문의에 대응할 수 있습니다.
> 자세한 TOAST Contact 서비스 이용법은 아래 가이드를 참고하시기 바랍니다.
> [TOAST Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)

#### Open Contact WebView

Gamebase 콘솔에 입력한 **고객 센터 URL** 웹뷰를 나타낼 수 있는 기능입니다.

* **Gamebase 콘솔 > App > InApp URL > Service center** 에 입력한 값이 사용됩니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
