## Game > Gamebase > Unity SDK User Guide > ETC

## Additional Features

Additional functions provided by Gamebase are described as below:

### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetDeviceLanguageCode()
```

> [참고]
>
> Editor on Windows, Standalone on Windows인 경우에는 [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)를 참고하여 언어 코드를 리턴합니다.
>
> Editor on Mac, WebGL은 [Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html) 값을 참고하여 언어 코드를 리턴합니다.<br/>예를 들어, Application.systemLanguage == SystemLanguage.Korean인 경우에는 'ko'를 리턴합니다.


### Display Language

* The display language on the Gamebase UI and SystemDialog can be changed into another language, which is not set on a device, as the user wants. 
* Gamebase displays messages which are included in a client or as received by a server. 
* With DisplayLanguage, messages are displayed in an appropriate language for the language code (ISO-639) set by the user. 
*  If necessary, language sets can be added as the user wants. The list of available language codes is as follows: 

> [Note]
>
> Client messages of Gamebase include English(en), Korean(ko) and Japanese(ja) only.

#### Types of Language Codes Supported by Gamebase

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

Each language code is defined in `GamebaseDisplayLanguageCode`.

> `[Warning]`
>
> Gamebase distinguishes the language code between the upper and lower case. 
> For example, settings like 'EN' or 'zh-ch' may cause a problem. 

```cs
namespace Toast.Gamebase
{
    public class GamebaseDisplayLanguageCode
    {
        public const string German = "de";
        public const string English = "en";
        public const string Spanish = "es";
        public const string Finish = "fi";
        public const string French = "fr";
        public const string Indonesian = "id";
        public const string Italian = "it";
        public const string Japanese = "ja";
        public const string Korean = "ko";
        public const string Portuguese = "pt";
        public const string Russian = "ru";
        public const string Thai = "th";
        public const string Vietnamese = "vi";
        public const string Malay = "ms";
        public const string Chinese_Simplified = "zh-CN";
        public const string Chinese_Traditional = "zh-TW";
    }
}
```

#### Set Display Language with Gamebase Initialization

Display Language can be set when Gamebase is initialized.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

``` cs
public void InitializeWithConfiguration()
{
    var configuration = new GamebaseRequest.GamebaseConfiguration();
    ...
    configuration.displayLanguageCode = displayLanguage;
    ...
    
    Gamebase.Initialize(configuration, (launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Gamebase initialization succeeded.");
            string displayLanguage = Gamebase.GetDisplayLanguageCode();
        }
        else
        {
            Debug.Log(string.Format("Gamebase initialization failed. error is {0}", error));
        }
    });
}
```

#### Set Display Language

You can change the initial setting of Display Language.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void SetDisplayLanguageCode(string languageCode)
```

**Example**

``` cs
public void SetDisplayLanguageCode()
{
    Gamebase.SetDisplayLanguageCode(GamebaseDisplayLanguageCode.English);
}
```

#### Get Display Language

You can retrieve the current application of Display Language.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static string GetDisplayLanguageCode()
```

**Example**

``` cs
public void GetDisplayLanguageCode()
{
    string displayLanguage = Gamebase.GetDisplayLanguageCode();
}
```

#### Add New Language Sets

For UnityEditor and Unity Standalone, or WebGL platform services, to use another language in addition to default Gamebase languages (ko, en, ja), go to Assets > StreamingAssets > Gamebase and add a value to the localizedstring.json file. 

![localizedstring.json](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

The localizedstring.json has a format defined as below:

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
}
```

To add another language, add `"${language code}":{"key":"value"}` to the localizedstring.json file. 

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
  "${언어코드}": {
      "common_ok_button": "...",
      ...
  }
}
```

If key is missing from inside of "${language code}":{ } of the json format above, `Language Set on Device` or `en` will be automatically entered. 

Refer to the guides below to learn how to add new language sets for Unity Android and  iOS platforms.

* [Add New Language Sets for Android](./aos-etc#display-language)
* [Add New Language Sets for iOS](./ios-etc#display-language)

#### Priority in Display Language

If Display Language is set via initialization and SetDisplayLanguageCode API, the final application may be different from what has been entered.

1. Check if the languageCode you enter is defined in the localizedstring.json file. 
2. See if, during Gamebase initialization, the language code set on the device is defined in the localizedstring.json file. (This value shall maintain even if the language set on device changes after initialization.)
3. `en`, which is the default value of Display Language, is automatically set.

### Country Code

* Gamebase는 System의 국가 코드를 다음과 같은 API로 제공하고 있습니다.
* 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가 코드를 리턴합니다.
* USIM에 잘못된 국가 코드가 기록되어 있다 하더라도 추가적인 체크 없이 그대로 리턴합니다.
* 값이 비어있는 경우 'ZZ'를 리턴합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static string GetCountryCodeOfUSIM()
```

#### Device Country Code

* OS로부터 전달받은 단말기 국가 코드를 추가적인 체크 없이 그대로 리턴합니다.
* 단말기 국가 코드는 '언어' 설정에 따라 OS가 자동으로 결정합니다.
* 여러 개의 언어가 등록된 경우, 우선권이 가장 높은 언어로 국가 코드를 결정합니다.
* 값이 비어있는 경우 'ZZ'를 리턴합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetCountryCodeOfDevice()
```

#### Intergrated Country Code

* USIM, 기기 언어 설정의 순서로 국가 코드를 확인하여 리턴합니다.
* GetCountryCode API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인하고, 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인하고, 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 리턴합니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

> [참고] 
>
> Editor on Windows, Standalone on Windows인 경우에는 [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)를 참고하여 국가 코드를 리턴합니다.
>
> Editor on Mac, WebGL은 [Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html) 값을 참고하여 국가 코드를 리턴합니다.<br/>예를 들어,Application.systemLanguage == SystemLanguage.Korean인 경우에는 'KR'을 리턴합니다.






**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
public static string GetCountryCode()
```

### Server Push
* Handles Server Push Messages from Gamebase server to a client device. 
* Add ServerPushEvent Listener to Gamebase Client, and the user can handle messages; the added ServerPushEvent Listener can be deleted.


#### Server Push Type
Server Push Types currently supported by Gamebase are as follows:

* GamebaseServerPushType.APP_KICKOUT (= "appKickout")
    * Go to **Operation > Kickout**  in the TOAST Gamebase console and register Kickout ServerPush messages, and **APP_KICKOUT** messages are sent to all clients connected to Gamebase.
* GamebaseServerPushType.TRANSFER_KICKOUT (= "transferKickout")
	* TransferKey 를 통해 게스트 계정 이전이 성공한 경우, TransferKey를 발급받았던 단말기로 **TRANSFER_KICKOUT** 메세지가 전송됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Use the API below, register ServerPushEvent to handle the push event triggered from the Gamebase Console and Gamebase server.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void AddServerPushEvent(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ServerPushMessage> serverPushEvent)
```

**Example**

```cs
public void AddServerPushEvent()
{
    Gamebase.AddServerPushEvent(ServerPushEventHandler);
}

private void ServerPushEventHandler(GamebaseResponse.SDK.ServerPushMessage message)
{
    switch(message.type)
    {
        case GamebaseServerPushType.APP_KICKOUT:
            {
                // Logout
                // Go to Main
                break;
            }
        case GamebaseServerPushType.TRANSFER_KICKOUT:
            {
                // Logout
                // Go to Main
                break;
            }
    }
}
```


#### Remove ServerPushEvent
Delete ServerPushEvent registered in Gamebase.  

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void RemoveServerPushEvent(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ServerPushMessage> serverPushEvent)
static void RemoveAllServerPushEvent()
```

**Example**

```cs
public void RemoveServerPushEvent(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ServerPushMessage> serverPushEvent)
{
    Gamebase.RemoveServerPushEvent(serverPushEvent);
}

public void RemoveAllServerPushEvent()
{
    Gamebase.RemoveAllServerPushEvent();
}
```

### Observer
* With Gamebase Observer, receive and process status change events of Gamebase.
* Status change events : change of network type, change of launching status (change of status due to maintenance, and etc.), and change of heartbeat information (change of heartbeat information due to service suspension), and etc.


#### Observer Type
The Observer Types currently supported by Gamebase are as follows:

* Change of Network Type
    * Receive information on changes of a network.
    * Type: GamebaseObserverType.NETWORK (= "network")
    * Code: Refer to the constant numbers declared in GamebaseNetworkType.
        * GamebaseNetworkType.TYPE_NOT: -1
        * GamebaseNetworkType.TYPE_MOBILE: 0
        * GamebaseNetworkType.TYPE_WIFI: 1
        * GamebaseNetworkType.TYPE_ANY: 2
* Change of Launching Status
    * Occurs when there is a change in the launching status response which periodically checks application status. For example, events occur for maintenance, or update recommendations.
    * Type: GamebaseObserverType.LAUNCHING (= "launching")
    * Code: Refer to the constant numbers declared in GamebaseLaunchingStatus.
        * GamebaseLaunchingStatus.IN_SERVICE: 200
        * GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus.BLOCKED_USER: 301
        * GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Change of Heartbeat Information
    * Occurs when there is a change in the heartbeat response which periodically maintains connection with the Gamebase server. For example, an event occurs for service suspension.
    * Type: GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code: Refer to the constant numbers declared in GamebaseErrorCode.
        * GamebaseErrorCode.INVALID_MEMBER: 6
        * GamebaseErrorCode.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Use the API below, register Observer to handle the status change events of Gamebase.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void AddObserver(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ObserverMessage> observer)
```

**Example**

```cs
public void AddObserver()
{
    Gamebase.AddObserver(ObserverHandler);
}

private void ObserverHandler(GamebaseResponse.SDK.ObserverMessage observerMessage)
{
    switch (observerMessage.type)
    {
        // Launching
        case GamebaseObserverType.LAUNCHING:
            {
                CheckLaunchingStatus(observerMessage.data);
                break;
            }
        // Heartbeat
        case GamebaseObserverType.HEARTBEAT:
            {
                CheckHeartbeat(observerMessage.data);
                break;
            }
        // Network
        case GamebaseObserverType.NETWORK:
            {
                CheckNetworkStatus(observerMessage.data);
                break;
            }
    }
}

private void CheckLaunchingStatus(Dictionary<string, object> data)
{
    // Code : Refer to GamebaseLaunchingStatus.
    int code        = (int)data["code"];

    // Message
    string message  = (string)data["message"];
}

private void CheckHeartbeat(Dictionary<string, object> data)
{
    // Code : GamebaseErrorCode.INVALID_MEMBER, GamebaseErrorCode.BANNED_MEMBER
    int code = (int)data["code"];
}

private void CheckNetworkStatus(Dictionary<string, object> data)
{
    // Code: Refer to GamebaseNetworkType.
    int code        = (int)data["code"];

    // Message
    string message  = (string)data["message"];
}
```


#### Remove Observer
Delete Observer registered in Gamebase.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void RemoveObserver(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ObserverMessage> observer)
static void RemoveAllObserver()
```

**Example**

```cs
public void RemoveObserver(GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ObserverMessage> observer)
{
    Gamebase.RemoveObserver(observer);
}

public void RemoveAllObserver()
{
    Gamebase.RemoveAllObserver();
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
> Gamebase.Purchase.RequestPurchase API를 호출하여 결제를 완료하면, 자동으로 지표를 전송합니다.
>

Analytics Console 사용법은 아래 가이드를 참고하십시오.

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

게임 로그인 이후 유저 레벨 정보를 지표로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 SetGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int |  |
| channelId | O | string |  |
| characterId | O | string |  |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void SetGameUserData(GamebaseRequest.Analytics.GameUserData gameUserData)
```

**Example**

``` cs
public void SetGameUserData(int userLevel, string channelId, string characterId)
{
    GamebaseRequest.Analytics.GameUserData gameUserData = new GamebaseRequest.Analytics.GameUserData(userLevel);
    gameUserData.channelId = channelId;
    gameUserData.characterId = characterId;

    Gamebase.Analytics.SetGameUserData(gameUserData);
}
```

#### Level Up Trace

레벨업이 되었을 경우 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int |  |
| levelUpTime | O | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |
| channelId | O | string |  |
| characterId | O | string |  |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void TraceLevelUp(GamebaseRequest.Analytics.LevelUpData levelUpData)
```

**Example**

``` cs
public void TraceLevelUp(int userLevel, long levelUpTime, string channelId, string characterId)
{
    GamebaseRequest.Analytics.LevelUpData levelUpData = new GamebaseRequest.Analytics.LevelUpData(userLevel);
    levelUpData.levelUpTime = levelUpTime;
    levelUpData.channelId = channelId;
    levelUpData.characterId = characterId;

    Gamebase.Analytics.TraceLevelUp(levelUpData);
}
```