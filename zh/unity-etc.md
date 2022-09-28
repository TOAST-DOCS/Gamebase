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
> Editor on Mac、WebGL参考[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)值返回语言代码。<br/>例如，为Application.systemLanguage == SystemLanguage.Korean时返回“ko”。

### Display Language

正如游戏维护弹窗显示语言，Gamebase也显示终端机设置的语言。

但有些游戏允许通过额外选项更改终端机设置的语言。
终端机设置的默认语言是英语，但需将游戏的显示语言转换为日语时，即使要将Gamebase的显示语言也转换为日语，Gamebase仍显示终端机设置的默认语言（en）。

因此Gamebase向需以终端机设置语言之外的其他语言显示Gamebase消息的应用程序，提供“Display Language”功能。 

Gamebase显示消息时，按照注册为Display Language的语言显示消息。
在Display Language输入语言代码时，只能使用以下列表中（**Gamebase支持的语言代码种类**）指定的代码。

> <font color="red">[注意]</font><br/>
>
> * 无论终端机设置的语言如何，只需要更改Gamebase显示的语言时使用Display Language Gamebase功能。
> * Display Language Code是区分大小写的ISO-639形态的值。
> 若按“EN”或“zh-cn”进行设置，可能出现问题。
> * 若输入的Display Language Code值不在以下列表时（**Gamebase支持的语言代码种类**）, 则将Display Langauge Code自动设置为默认语言(en)。

> [参考]
>
> * 因Gamebase客户端消息中仅包含英语（en）、韩语（ko）、日语（ja），即使是下列表指定的语言代码，指定英语（en）、韩语（ko）、日语（ja）之外的语言时，也将自动设置为默认语言(en)。
> * 可以直接添加未注册在Gamebase客户端的语言集合。
> 请参考**添加新语言集合**项目。 

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

相应的语言代码在`GamebaseDisplayLanguageCode`类中定义。

```cs
namespace Toast.Gamebase
{
    public class GamebaseDisplayLanguageCode
    {
        public const string German = "de";
        public const string English = "en";
        public const string Spanish = "es";
        public const string Finnish = "fi";
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

#### 初始化Gamebase时设置显示语言

初始化Gamebase时可以设置显示语言。

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

初始化Gamebase时可更改输入的Display Language。

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

提供UnityEditor、Unity Standalone及WebGL平台服务时，如果要使用Gamebase提供的默认语言(ko, en)外的其他语言，需要在Assets > StreamingAssets > Gamebase中的localizedstring.json文件中添加值。

![localizedstring.json](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

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

如果需要添加另一种语言集，则在localizedstring.json文件中添加`"${语言代码}":{"key":"value"}`形式的值。

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

关于Unity Android、iOS平台中添加新语言集的方法，请参考以下指南。

* [Android添加新语言集](./aos-etc#display-language)
* [iOS添加新语言集](./ios-etc#display-language)

  

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
* 若值为空，则返回“ZZ”。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static string GetCountryCodeOfUSIM()
```

#### Device Country Code

* 从OS接收的终端机国家代码直接返回，不另行确认。
* 终端机国家代码根据语言设置，由OS自动决定。
* 注册多种语言时，以优先权最高的语言决定国家代码。
* 若值为空，则返回“ZZ”。

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
	3. 若USIM、终端机国家代码均为空值，则返回“ZZ”。

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

> [参考] 
>
> 为Editor on Windows、Standalone on Windows时参考[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)返回国家代码。
>
> Editor on Mac、WebGL参考[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)值返回国家代码。<br/>例如，为Application.systemLanguage == SystemLanguage.Korean时返回“KR”。

**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
public static string GetCountryCode()
```

### Gamebase Event Handler

* Gamebase通过**GamebaseEventHandler**事件系统处理所有的事件。
* GamebaseEventHandler通过以下API简单添加或删除Listener 。

**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
public static void Gamebase.AddEventHandler(GamebaseCallback.DataDelegate<GamebaseResponse.Event.GamebaseEventMessage> eventHandler);
public static void Gamebase.RemoveEventHandler(GamebaseCallback.DataDelegate<GamebaseResponse.Event.GamebaseEventMessage> eventHandler);
public static void Gamebase.RemoveAllEventHandler();
```

**VO**

```cs
public class GamebaseEventMessage 
{
	// 显示Event种类。
    // 分配GamebaseEventCategory类中定义的值。
    public string category;

    // 是可转换为符合category的VO的JSON String数据。
     public string data;
}
```

**示例**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
case GamebaseEventCategory.IDP_REVOKED:
            {
                GamebaseResponse.Event.GamebaseEventIdPRevokedData idPRevokedData = GamebaseResponse.Event.GamebaseEventIdPRevokedData.From(message.data);
                if (idPRevokedData != null)
                {
                    ProcessIdPRevoked(idPRevokedData);
                }
                break;
            }
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
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED:
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
        case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
            {
                GamebaseResponse.Event.GamebaseEventServerPushData serverPushData = GamebaseResponse.Event.GamebaseEventServerPushData.From(message.data);
                if (serverPushData != null)
                {
                    CheckServerPush(message.category, serverPushData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_LAUNCHING:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if(observerData != null)
                {
                    CheckLaunchingStatus(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_NETWORK:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckNetwork(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_HEARTBEAT:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckHeartbeat(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_WEBVIEW:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckWebView(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_INTROSPECT:
            {
                // Introspect error
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                int errorCode = observerData.code;
                string errorMessage = observerData.message;
                break;
            }
        case GamebaseEventCategory.PURCHASE_UPDATED:
            {
                GamebaseResponse.Event.PurchasableReceipt purchasableReceipt = GamebaseResponse.Event.PurchasableReceipt.From(message.data);
                if (purchasableReceipt != null)
                {
                    // If the user got item by 'Promotion Code',
                    // this event will be occurred.
                }
                break;
            }
        case GamebaseEventCategory.PUSH_RECEIVED_MESSAGE:
            {
                GamebaseResponse.Event.PushMessage pushMessage = GamebaseResponse.Event.PushMessage.From(message.data);
                if (pushMessage != null)
                {
                    // When you received push message.
                    
                    // By converting the extras field of the push message to JSON,
                    // you can get the custom information added by the user when sending the push.
                    // (For Android, an 'isForeground' field is included so that you can check if received in the foreground state.)
                }
                break;
            }
        case GamebaseEventCategory.PUSH_CLICK_MESSAGE:
            {
                GamebaseResponse.Event.PushMessage pushMessage = GamebaseResponse.Event.PushMessage.From(message.data);
                if (pushMessage != null)
                {
                    // When you clicked push message.
                }
                break;
            }
        case GamebaseEventCategory.PUSH_CLICK_ACTION:
            {
                GamebaseResponse.Event.PushAction pushAction = GamebaseResponse.Event.PushAction.From(message.data);
                if (pushAction != null)
                {
                    // When you clicked action button by 'Rich Message'.
                }
                break;
            }
    }
}
```

    
* 事件大体分为LoggedOut、ServerPush、Observer、Purchase、Push，并按照各Category, 按如下列表的方式将GamebaseEventMessage.data转换为VO。


| Event种类 | GamebaseEventCategory | VO转换方法 | 备注 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | GamebaseEventCategory.IDP_REVOKED | GamebaseResponse.Event.GamebaseEventIdPRevokedData.from(message.data) | \- |
| LoggedOut | GamebaseEventCategory.LOGGED_OUT | GamebaseResponse.Event.GamebaseEventLoggedOutData.from(message.data) | \- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED<br>GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseResponse.Event.GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseResponse.Event.GamebaseEventObserverData.from(message.data) | \- |
| Purchase - Promotion支付 | GamebaseEventCategory.PURCHASE_UPDATED | GamebaseResponse.Event.PurchasableReceipt.from(message.data) | \- |
| Push - 接收消息 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) | |
| Push - 点击消息 | GamebaseEventCategory.PUSH_CLICK_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) | |
| Push - 动态点击 | GamebaseEventCategory.PUSH_CLICK_ACTION | GamebaseResponse.Event.PushAction.from(message.data) | 点击RichMessage按键时启动。|

#### IdP Revoked

* 是当在IdP中删除相关服务时出现的事件。
* 需要通知用户IdP已被禁用，并使用户使用相同的IdP登录时收到新的userID。
* GamebaseEventIdPRevokedData.code : 为GamebaseIdPRevokedCode值。
    * WITHDRAW : 600
        * 表示当前使用禁用的IdP登录，并且没有映射的IdP列表。
        * 必须通过调用Withdraw API对当前帐户进行退出处理。
    * OVERWRITE_LOGIN_AND_REMOVE_MAPPING : 601
        * 表示当前使用禁用的IdP登录，而除了禁用的IdP还有其他IdP被映射。
        * 需要使用被映射的IdP当中的一个IdP登录，并通过调用RemoveMapping API解除禁用的IdP的链接。
    * REMOVE_MAPPING : 602
        * 表示映射到当前账户的IdP当中有禁用IdP。
        * 需要通过调用RemoveMapping API解除禁用的IdP的链接。
* GamebaseEventIdPRevokedData.idpType : 是禁用的IdP类型。
* GamebaseEventIdPRevokedData.authMappingList : 是映射到当前账户的IdP列表。

**Example**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}
private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.IDP_REVOKED:
            {
                GamebaseResponse.Event.GamebaseEventIdPRevokedData idPRevokedData = GamebaseResponse.Event.GamebaseEventIdPRevokedData.From(message.data);
                if (idPRevokedData != null)
                {
                    ProcessIdPRevoked(idPRevokedData);
                }
                break;
            }
        default:
            {
                break;
            }
    }
}
private void ProcessIdPRevoked(string category, GamebaseResponse.Event.GamebaseEventIdPRevokedData data)
{
    var revokedIdP = data.idPType;
    switch (data.code)
    {
        case GamebaseIdPRevokedCode.WITHDRAW:
            {
                // 表示当前使用禁用的IdP登录，并且没有被映射的IdP列表。
                // 请通知用户当前账户已被退出。
                Gamebase.Withdraw((error) =>
                {
                    ...
                });
                break;                            
            }
        case GamebaseIdPRevokedCode.OVERWRITE_LOGIN_AND_REMOVE_MAPPING:
            {
                // 表示当前使用禁用的IdP登录，而除了禁用的IdP还有其他IdP被映射。
                // 让用户从authMappingList中选择要再次登录的IdP，并在使用所选IdP登录后解除禁用的IdP的链接。
                var selectedIdP = "用户选择的IdP";
                var additionalInfo = new Dictionary<string, object>()
                {
                    { GamebaseAuthProviderCredential.IGNORE_ALREADY_LOGGED_IN, true }
                };
                Gamebase.Login(selectedIdP, additionalInfo, (authToken, loginError) =>
                {
                    if (Gamebase.IsSuccess(loginError) == true)
                    {
                        Gamebase.RemoveMapping(revokedIdP, (mappingError) =>
                        {
                            ...          
                        });
                    }
                });
                break;
            }
        case GamebaseIdPRevokedCode.REMOVE_MAPPING:
            {
                // 表示映射到当前账户的IdP当中有禁用IdP。
                // 请通知用户在当前账户中禁用IdP的链接被解除。
                Gamebase.RemoveMapping(revokedIdP, (error) =>
                {
                    ...
                });
                break;
            }
    }
}
```

#### Logged Out

* 是当Gamebase Access Token无效，为了恢复网络会话，需要调用登录函数时出现的事件。

**Example**

```cs
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
```

#### Server Push

* 是从Gamebase服务器向客户端终端机传送的消息。 
* Gamebase支持的Server Push Type如下。
    * GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED
    	* 如果在NHN Cloud Gamebase控制台的**Operation > Kickout**中注册Kickout ServerPush消息， 则将从与Gamebase连接的所有客户端接收Kickout消息。
        * 是在客户端终端机中接收了服务器消息时立即启动的事件。 
        * 正如“Autoplay”，当游戏运行时，它可以用于暂停游戏。
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* 如果在TOAST Gamebase控制台的**Operation > Kickout**中注册Kickout ServerPush消息，则从与Gamebase连接的所有客户端接收Kickout消息。
        * 是当在客户端终端机接收了服务器消息时显示弹窗，而用户关闭其弹窗时启动的事件。
    	* 在NHN Cloud Gamebase控制台的**Operation > Kickout**中注册Kickout ServerPush消息，将从与Gamebase连接的所有客户端接收Kickout消息。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT    
    	* 将Guest账号成功转移到其他终端机时，从转移之前的终端机接收Kickout消息。
   
**示例**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED:
        case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
        case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
            {
                GamebaseResponse.Event.GamebaseEventServerPushData serverPushData = GamebaseResponse.Event.GamebaseEventServerPushData.From(message.data);
                if (serverPushData != null)
                {
                    CheckServerPush(message.category, serverPushData);
                }
                break;
            }
        default:
            {
                break;
            }
    }
}

private void CheckServerPush(string category, GamebaseResponse.Event.GamebaseEventServerPushData data)
{
    if (category.Equals(GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT) == true)
    {
        // Kicked out from Gamebase server.(Maintenance, banned or etc.)
        // And the game user closes the kickout pop-up.
        // Return to title and initialize Gamebase again.
    }
    else if (category.Equals(GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED) == true)
    {
        // Currently, the kickout pop-up is displayed.
        // If your game is running, stop it.
    }
    else if (category.Equals(GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT) == true)
    {
        // If the user wants to move the guest account to another device,
        // if the account transfer is successful,
        // the login of the previous device is released,
        // so go back to the title and try to log in again.
    }
}
```

#### Observer

* 是处理Gamebase各状态的变动事件的系统。 
* Gamebase支持的Observer Type如下。
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 当维护开始、结束时或发布新版本必须进行更新等Launching状态出现变动时启动。
    	* GamebaseEventObserverData.code: 为LaunchingStatus值。 
            * LaunchingStatus.IN_SERVICE: 200
            * LaunchingStatus.RECOMMEND_UPDATE: 201
            * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
            * LaunchingStatus.REQUIRE_UPDATE: 300
            * LaunchingStatus.BLOCKED_USER: 301
            * LaunchingStatus.TERMINATED_SERVICE: 302
            * LaunchingStatus.INSPECTING_SERVICE: 303
            * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
            * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
    * GamebaseEventCategory.OBSERVER_HEARTBEAT
    	* 当因已被退出或禁用、用户账号状态出现变化时启动。
    	* GamebaseEventObserverData.code: 为GamebaseError值。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 可以接收网络变动信息。 
    	* 当网络断开或被连接时、从Wifi转为Cellular网络时启动。
    	* GamebaseEventObserverData.code: 为NetworkManager值。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_WEBVIEW
        * 在Standalone中打开或关闭Webview时启动。 
            * GamebaseWebViewEventType.OPENED: 1
            * GamebaseWebViewEventType.CLOSED: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * 登录Standalone/WebGL后延长session失败时启动。

**VO**

```cs
public class GamebaseEventObserverData 
{
	// 为显示状态值的信息。
    public int code;

    // 为描述状态的message信息。 
    public string message;

    // 为用于附加信息的保留字段。
    public string extras;
}
```

**示例**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.OBSERVER_LAUNCHING:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if(observerData != null)
                {
                    CheckLaunchingStatus(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_NETWORK:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckNetwork(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_HEARTBEAT:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckHeartbeat(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_WEBVIEW:
            {
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                if (observerData != null)
                {
                    CheckWebView(observerData);
                }
                break;
            }
        case GamebaseEventCategory.OBSERVER_INTROSPECT:
            {
                // Introspect error
                GamebaseResponse.Event.GamebaseEventObserverData observerData = GamebaseResponse.Event.GamebaseEventObserverData.From(message.data);
                int errorCode = observerData.code;
                string errorMessage = observerData.message;
                break;
            }
        default:
            {
                break;
            }
    }
}

private void CheckLaunchingStatus(GamebaseResponse.Event.GamebaseEventObserverData observerData)
{
    switch (observerData.code)
    {
        case GamebaseLaunchingStatus.IN_SERVICE:
            {
                // Service is now normally provided.
                break;
            }
        // ... 
        case GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR:
            {
                // Error in internal server.
                break;
            }
    }
}

private void CheckNetwork(GamebaseResponse.Event.GamebaseEventObserverData observerData)
{
    switch ((GamebaseNetworkType)observerData.code)
    {
        case GamebaseNetworkType.TYPE_NOT:
            {
                // Network disconnected
                break;
            }
        case GamebaseNetworkType.TYPE_MOBILE:
            {
                // Network connected
                break;
            }
        case GamebaseNetworkType.TYPE_WIFI:
            {
                // Network connected
                break;
            }
        case GamebaseNetworkType.TYPE_ANY:
            {
                // Network connected
                break;
            }
    }
}

private void CheckHeartbeat(GamebaseResponse.Event.GamebaseEventObserverData observerData)
{
    switch (observerData.code)
    {
        case GamebaseErrorCode.INVALID_MEMBER:
            {
                // You should to write the code necessary in game. (End the session.)
                break;
            }
        case GamebaseErrorCode.BANNED_MEMBER:
            {
                // The ban information can be found by using the GetBanInfo API.
                // Show kickout message to user and need kickout in game.
                break;
            }
    }
}

private void CheckWebView(GamebaseResponse.Event.GamebaseEventObserverData observerData)
{
    switch (observerData.code)
    {
        case GamebaseWebViewEventType.OPENED:
            {
                // Webview open
                break;
            }
        case GamebaseWebViewEventType.CLOSED:
            {
                // Webview close
                break;
            }
    }
}
```


#### Purchase Updated

* 是通过输入Promotion代码获取商品时出现的事件。 
* 可以获取结算票据信息。

**Example**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.PURCHASE_UPDATED:
            {
                GamebaseResponse.Event.PurchasableReceipt purchasableReceipt = GamebaseResponse.Event.PurchasableReceipt.From(message.data);
                if (purchasableReceipt != null)
                {
                    // If the user got item by 'Promotion Code',
                    // this event will be occurred.
                }
                break;
            }
        default:
            {
                break;
            }
    }
}
```

#### Push Received Message

* 是接收Push消息时出现的事件。
* 通过将extras字段转换为JSON，可获取发送Push时传送的自定义信息。
    * 在**Android**上可以通过**isForeground**字段区分是否是在前台接收的消息还是在后台接收的消息。

**VO**

```cs
public class PushMessage 
{
	// 为消息的固有id。
    public string id;

    // 为Push消息的标题。
    public string title;
	
    // 为Push消息的身体。
    public string body;

    // 当以JSON格式发送Push时，可查看传送的自定义信息。
    public string extras;
}
```

**Example**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.PUSH_RECEIVED_MESSAGE:
            {
                GamebaseResponse.Event.PushMessage pushMessage = GamebaseResponse.Event.PushMessage.From(message.data);
                if (pushMessage != null)
                {
                    // When you received push message.

                    // By converting the extras field of the push message to JSON,
                    // you can get the custom information added by the user when sending the push.
                    // (For Android, an 'isForeground' field is included so that you can check if received in the foreground state.
                }
                break;
            }
        default:
            {
                break;
            }
    }
}
```

#### Push Click Message

* 是点击“已接收的Push消息”时出现的事件。
* 与“GamebaseEventCategory.PUSH_RECEIVED_MESSAGE”不同，在Android上的extras字段不存在**isForeground**信息。

**Example**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {
        case GamebaseEventCategory.PUSH_CLICK_MESSAGE:
            {
                GamebaseResponse.Event.PushMessage pushMessage = GamebaseResponse.Event.PushMessage.From(message.data);
                if (pushMessage != null)
                {
                    // When you clicked push message.
                }
                break;
            }
        default:
            {
                break;
            }
    }
}
```

#### Push Click Action

* 是通过Rich Message功能，点击生成的按钮时出现的事件。
* actionType中存在以下值。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```cs
class PushAction 
{
	// 为ButtonAction种类。 
    public string actionType;

	// 为PushMessage数据。
    public PushMessage message;

	// 为在Push控制台中输入的用户文本。
    public string userText;
}
```

**示例**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseEventHandler);
}

private void GamebaseEventHandler(GamebaseResponse.Event.GamebaseEventMessage message)
{
    switch (message.category)
    {    
        case GamebaseEventCategory.PUSH_CLICK_ACTION:
            {
                GamebaseResponse.Event.PushAction pushAction = GamebaseResponse.Event.PushAction.From(message.data);
                if (pushAction != null)
                {
                    // When you clicked action button by 'Rich Message'.
                }
                break;
            }
        default:
            {
                break;
            }
    }
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

关于Analytics控制台使用方法，请参考如下指南。

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
> 与TOAST Contact商品关联使用，则可更加轻松方便地应对顾客咨询。
> TOAST Contact商品使用，请参考如下指南。
> [TOAST Online Contact Guide](/Contact%20Center/zh/online-contact-overview/)

#### Customer Service Type

**Gamebase控制台 > App > InApp URL > Service center**当中选择如下3个客户服务类型中的一个。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| TOAST Online Contact      | O              |

Gamebase SDK的客户服务API根据各类型使用如下URL。

* 开发公司自建客户服务(Developer customer center)
    * 在**客户服务URL**输入的URL
* Gamebase提供的客户服务(Gamebase customer center)
    * 登录前 : **不包含**用户信息的客户服务URL
    * 登录后 : 包含用户信息的客户服务URL
* TOAST组织服务(Online Contact)
    * 登录前 : 出现NOT_LOGGED_IN(2)错误。
    * 登录后 : 包含用户信息的客户服务URL

#### Open Contact WebView

显示客户服务WebView。
根据客户服务类型选择URL。
可通过ContactConfiguration向URL传送附加信息。

**GamebaseRequest.Contact.Configuration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | 用户名(nickname)<br>**default** : null    |
| additionalURL | O             | string                             | 添加在开发公司自建客户服务URL后面的附加URL<br>**default** : null    |
| extraData     | O             | dictionary<string, string>         | 开始客户服务服务时传送开发公司所需的extra data。<br>**default** : null    |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void OpenContact(GamebaseCallback.ErrorDelegate callback);
static void OpenContact(GamebaseRequest.Contact.Configuration configuration, GamebaseCallback.ErrorDelegate callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize。|
| NOT\_LOGGED\_IN(2)                                  | 客户服务的类型为“TOAST OC”时，登录前已调用ContactConfiguration函数。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客户服务URL不存在。<br>请确认Gamebase控制台的**客户服务URL**。|
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 识别用户的临时ticket发放失败 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | 已显示客户服务WebView。|

**Example**

``` cs
public void SampleOpenContact()
{
    Gamebase.Contact.OpenContact((error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            // A user close the contact web view.
        }
        else if (error.code == GamebaseErrorCode.UI_CONTACT_FAIL_INVALID_URL)  // 6911
        {
            // TODO: Gamebase Console Service Center URL is invalid.
            // Please check the url field in the TOAST Gamebase Console.
        } 
        else if (error.code == GamebaseErrorCode.UI_CONTACT_FAIL_ANDROID_DUPLICATED_VIEW) // 6913
        { 
            // The customer center web view is already opened.
        } 
        else 
        {
            // An error occur when opening the contact web view.
        }
    });
}
```

> <font color="red">[注意]</font>
>
> 联系客服时，您可能需要添附文件。
> 为此，需要在运行时从用户获得有关相机拍照或Storage存储的权限。 
>
> Android用户
>
> * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> * 如果您是Unity用户，请参考如下指南执行上述程序 。
> [Unity Guide : Requesting Permissions](https://docs.unity3d.com/2018.4/Documentation/Manual/android-RequestingPermissions.html)
>
> iOS用户
>
> 请在* info.plist中设置“Privacy - Camera Usage Description”、“Privacy - Photo Library Usage Description”。

#### Request Contact URL

返还显示客户服务WebView时使用的URL。

**API**

```cs
static void RequestContactURL(GamebaseCallback.GamebaseDelegate<string> callback);
static void RequestContactURL(GamebaseRequest.Contact.Configuration configuration, GamebaseCallback.GamebaseDelegate<string> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize。|
| NOT\_LOGGED\_IN(2)                                  | 客户服务的类型为“TOAST OC”时，登录前已调用ContactConfiguration函数。|
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客户服务URL不存在。<br>请确认Gamebase控制台的**客户服务URL**。|
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 识别用户的临时ticket发放失败 |

**Example**

``` cs
public void SampleRequestContactURL()
{
    var configuration = new GamebaseRequest.Contact.Configuration()
                            {
                                userName = "User Name"
                            };

    Gamebase.Contact.RequestContactURL(configuration, (url, error) => 
    {
        if (Gamebase.IsSuccess(error) == true) 
        {
            // Open webview with 'contactUrl'
        } 
        else if (error.code == GamebaseErrorCode.UI_CONTACT_FAIL_INVALID_URL) // 6911
        { 
            // TODO: Gamebase Console Service Center URL is invalid.
            // Please check the url field in the TOAST Gamebase Console.
        } 
        else 
        {
            // An error occur when requesting the contact web view url.
        }
    });
}
```