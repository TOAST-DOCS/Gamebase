## Game > Gamebase > Unity SDK使用指南 > ETC

## Additional Features

以下描述Gamebase支持的附加功能。

### Device Language

* 返回终端机设置的语言代码。
* 注册多种语言时，仅返回优先权最高的语言。

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

> [参考]
>
> 为Editor on Windows、Standalone on Windows时参考[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)返回语言代码。
>
> Editor on Mac、WebGL参考[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)值返回语言代码。<br/>例如，为Application.systemLanguage == SystemLanguage.Korean时返回’ko’。

### Display Language

* 可以将Gamebase提供的UI及SystemDialog显示的语言更改为设备上设置的语言以外的语言。
* Gamebase显示客户端中包含的信息或从服务器接收的信息。
* 如果设置DisplayLanguage，将以用户设置的语言代码（ISO-639）的语言显示信息。
* 可以添加所需的语言。 以下是可以添加的语言代码。

> [参考]
>
> Gamebase的客户信息仅包含英文(en)、韩文(ko)、日文(ja)。

#### Gamebase支持的语言代码种类

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

相应的语言代码在`GamebaseDisplayLanguageCode`类中定义。

> <font color="red">[注意]</font>
>
> Gamebase支持的语言代码区分大小写。
> 将其设置为'EN'或'zh-cn'可能会出现问题。

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

#### 在Gamebase初始化时设置显示语言

在Gamebase初始化时可以设置显示语言。

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

**示例**

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

#### 设置显示语言

Gamebase初始化时可更改输入的 Display Language。

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

**示例**

``` cs
public void SetDisplayLanguageCode()
{
    Gamebase.SetDisplayLanguageCode(GamebaseDisplayLanguageCode.English);
}
```

#### 查询显示语言

可以查询当前使用的显示语言。

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

**示例**

``` cs
public void GetDisplayLanguageCode()
{
    string displayLanguage = Gamebase.GetDisplayLanguageCode();
}
```

#### 添加新语言集

提供UnityEditor及Unity Standalone, WebGL平台服务时，如果要使用Gamebase提供的默认语言(ko, en)外的其他语言，需要在Assets > StreamingAssets > Gamebase中的 localizedstring.json文件中添加值。

![localizedstring.json](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

localizedstring.json中定义的格式如下。

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

如果需要添加另一种语言集，可在localizedstring.json文件中添加`"${语言代码}":{"key":"value"}`形式的值。

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
  "${语言代码}": {
      "common_ok_button": "...",
      ...
  }
}
```

如果在上述JSON文件的格式"${语言代码}":{ }中缺少key，则会自动输入`在设备上设置的语言`或`en`。

Unity Android, iOS平台中添加新语言集的方法请参考以下指南。

* [Android添加新语言集](./aos-etc#display-language)
* [iOS添加新语言集](./ios-etc#display-language)

#### 显示语言优先级

通过初始化或SetDisplayLanguageCode API设置Display Language时，最终应用的Display Language可以与输入的值不同。

1. 确认输入的languageCode是否在localizedstring.json文件中定义。
2. 初始化Gamebase时，确认是否在localizedstring.json文件中定义了设备上设置的语言代码（即使在初始化后更改了设备上设置的语言，此值也将保留）。
3. 自动设置Display Language的默认值为`en`。

### Country Code

* Gamebase以如下API提供系统的国家代码(country code)。
* 各API具有不同特征，因此请选择与用途相符的API。

#### USIM Country Code

* 返回USIM中记录的国家代码。
* 即使USIM中记录的是错误的国家代码也将不进行确认就直接返回。
* 若值为空，则返回’ZZ’。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static string GetCountryCodeOfUSIM()
```

#### Device Country Code

* 从OS接收的终端机国家代码直接返回，不另行确认。
* 终端机国家代码根据’语言’设置，由OS自动决定。
* 注册多种语言时，以优先权最高的语言决定国家代码。
* 若值为空，则返回’ZZ’。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetCountryCodeOfDevice()
```

#### Intergrated Country Code

* 按照USIM、设备语言设置的顺序确认国家代码并返回。
* GetCountryCode API按照如下顺序运行。
	1. 确认USIM中记录的国家代码，若存在值，则直接返回，不另行确认。
	2. 若USIM国家代码为空值，确认终端机国家代码，若存在值，则直接返回，不另行确认。
	3. 若USIM、终端机国家代码均为空值，则返回’ZZ’。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

> [参考] 
>
> 为Editor on Windows、Standalone on Windows时参考[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)返回国家代码。
>
> Editor on Mac、WebGL参考[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)值返回国家代码。<br/>例如，为Application.systemLanguage == SystemLanguage.Korean时返回’KR’。






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

* 可以处理从Gamebase服务器发送到客户端设备的Server Push Message。
* 在Gamebase客户端上添加ServerPushEvent Listener允许用户接收和处理信息，可以删除添加的ServerPushEvent Listener。


#### Server Push类型
目前，Gamebase支持的Server Push Type如下。

* GamebaseServerPushType.APP_KICKOUT (= "appKickout")
    * 如果在NHN Cloud Gamebase控制台的`Operation > Kickout`中注册ServerPush 消息，与Gamebase连接的所有客户端的将收到`APP_KICKOUT`消息。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### 注册ServerPushEvent
可以在Gamebase Client中注册ServerPushEvent来处理Gamebase Console和 Gamebase服务器发出的Push事件。

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

**示例**

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


#### 删除 ServerPushEvent
可以删除在Gamebase注册的 ServerPushEvent。

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

**示例**

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
* Gamebase Observer允许接收并处理Gamebase的各种状态变动事件。
* 状态变动事件：网络类型变动，Launching状态变动(由于维护等引起的状态变动)，Heartbeat信息变动(用户禁用而导致Heartbeat信息变动）等。


#### Observer Type
Gamebase目前支持的 Observer类型如下。

* Network类型变动
    * 可以接收网络变动信息。
    * Type: GamebaseObserverType.NETWORK (= "network")
    * Code: 请参考GamebaseNetworkType中声明的常量。
        * GamebaseNetworkType.TYPE_NOT: -1
        * GamebaseNetworkType.TYPE_MOBILE: 0
        * GamebaseNetworkType.TYPE_WIFI: 1
        * GamebaseNetworkType.TYPE_ANY: 2
* Launching状态变动
    * 当定期检查应用程序状态的Launching Status response有变动时发生触发。例如，有维护或推荐更新等产生的事件。
    * Type: GamebaseObserverType.LAUNCHING (= "launching")
    * Code: 请参考在GamebaseLaunchingStatus中声明的常量。
        * GamebaseLaunchingStatus.IN_SERVICE: 200
        * GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus.BLOCKED_USER: 301
        * GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat信息变动
    * 定期与Gamebase服务器保持连接的Heartbeat response有变动时发生触发。例如，由用户禁用引起的事件。
    * Type: GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code: 请参考GamebaseErrorCode中声明的常量。
        * GamebaseErrorCode.INVALID_MEMBER: 6
        * GamebaseErrorCode.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### 注册Observer
可以在Gamebase Client上注册Observer来处理各种状态变动事件。

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

**示例**

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


#### 删除 Observer
可以删除在Gamebase中注册的Observer。

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

**示例**

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

可将游戏指标传送至Gamebase服务器。

> <font color="red">[注意]</font><br/>
>
> Gamebase Analytics支持的所有API登录后可调用。
>
>
> [TIP]
>
> 调用Gamebase.Purchase.RequestPurchase API付款完成后，自动传送指标。
>

Analytics控制台使用方法请参考如下指南。

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

登录游戏后游戏用户级别信息可作为指标传送。

> <font color="red">[注意]</font><br/>
>
> 若登录游戏后不调用SetGameUserData API，则其他指标中可能会遗漏级别信息。
>

调用API所需的参数如下。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示游戏用户级别的字段。 |
| channelId | O | string | 是显示通道的字段。 |
| characterId | O | string | 是显示角色名的字段。 |
| characterClassId | O | string | 是显示职业的字段。 |

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
public void SetGameUserData(int userLevel, string channelId, string characterId, string characterClassId)
{
    GamebaseRequest.Analytics.GameUserData gameUserData = new GamebaseRequest.Analytics.GameUserData(userLevel);
    gameUserData.channelId = channelId;
    gameUserData.characterId = characterId;
    gameUserData.characterClassId = characterClassId;

    Gamebase.Analytics.SetGameUserData(gameUserData);
}
```

#### Level Up Trace

升级后游戏用户级别信息可作为指标传送。

调用API所需的参数如下。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示游戏用户级别的字段。 |
| levelUpTime | O | long | Enter Epoch Time</br>in millisecond. |
| channelId | O | string | 是显示角色名的字段。 |
| characterId | O | string | 是显示职业的字段。 |

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
public void TraceLevelUp(int userLevel, long levelUpTime)
{
    GamebaseRequest.Analytics.LevelUpData levelUpData = new GamebaseRequest.Analytics.LevelUpData(userLevel, levelUpTime);

    Gamebase.Analytics.TraceLevelUp(levelUpData);
}
```

### Contact

Gamebase提供用于应对客户咨询的功能。

> [TIP]
>
> 与NHN Cloud Contact商品关联使用，则可更加轻松方便地应对顾客咨询。
> NHN Cloud Contact商品使用，请参考如下指南。
> [NHN Cloud Online Contact Guide](/Contact%20Center/zh/online-contact-overview/)

#### Open Contact WebView

可弹出Gamebase Console中输入的**客服中心URL**页面的功能。

* 使用**Gamebase Console > App > InApp URL > Service center**中输入的值。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void OpenContact(GamebaseCallback.ErrorDelegate callback)
```

**Example**

``` cs
public void SampleOpenContact()
{
    Gamebase.Contact.OpenContact((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("OpenContact succeeded.");
        }
        else
        {
            Debug.Log(string.Format("OpenContact failed. error:{0}", error));

            if (error.code == GamebaseErrorCode.WEBVIEW_INVALID_URL)
            {
                // Gamebase Console Service Center URL is invalid.
                // Please check the url field in the TOAST Gamebase Console.
                var launchingInfo = Gamebase.Launching.GetLaunchingInformations();
                Debug.Log(string.Format("csUrl:{0}", launchingInfo.launching.app.relatedUrls.csUrl));
            }
        }
    });
}
```