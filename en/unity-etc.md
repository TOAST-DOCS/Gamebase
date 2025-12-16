## Game > Gamebase > Unity SDK User Guide > ETC

## Additional Features

Additional functions provided by Gamebase are described as below:

### Device Language

* Returns the language code from the device.
* If there are several languages registered, only the language of top priority is returned.

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

> [Note]
>
> In case of Editor on Windows or Standalone on Windows, the language code is returned based on the [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2).
>
> In case of Editor on Mac or WebGL, the language code is returned based on the [Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html) value.<br/>For example, when Application.systemLanguage == SystemLanguage.Korean, 'ko' is returned.


### Display Language

Similar to the Maintenance popup, the language used by the device will be displayed as the Gamebase language.

However, there are games that may use a language different from the device language with separate options.
For example, if the language configured for the device is English and you changed the game language to Japanese, the language displayed will be still English, even though you might want to see Japanese on the Gamebase screen.

For this, Gamebase provides a Display Language feature for applications that want to use a language that is not the language configured by the device for Gamebase.

Gamebase displays its messages in the language set in Display Language.
The language code entered for Display Language should be one of the codes listed in the table (**Types of language codes supported by Gamebase) below:

> <font color="red">[Caution]</font><br/>
>
> * Use Display Language only when you want to change the language displayed in Gamebase to a language other than the one configured by the device.
> * Display Language Code is a case-sensitive value in the form of ISO-639.
> There could be a problem if it is configured as a value such as 'EN' or 'zh-cn'.
> * If the value entered as Display Language Code does not exist in the table (**Types of language codes supported by Gamebase) below, Display Language Code is automatically set to English (en) by default.

> [Note]
>
> * As the client messages of Gamebase include only English (en), Korean (ko), and Japanese (ja), if you try to set a language other than English (en), Korean (ko), or Japanese (ja), even though the language code might be listed in the table below, the value is automatically set to English (en) by default.
> * You can manually add a language set that is not included in the Gamebase client.
> See the **Add New Language Set** section.

#### Types of Language Codes Supported by Gamebase

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

Each language code is defined in `GamebaseDisplayLanguageCode`.

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

![localizedstring.json](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

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

Refer to the guides below to learn how to add new language sets for Unity Android and  iOS platforms.

* [Add New Language Sets for Android](./aos-etc#display-language)
* [Add New Language Sets for iOS](./ios-etc#display-language)

#### Priority in Display Language

If Display Language is set via initialization and SetDisplayLanguageCode API, the final application may be different from what has been entered.

1. Check if the languageCode you enter is defined in the localizedstring.json file. 
2. See if, during Gamebase initialization, the language code set on the device is defined in the localizedstring.json file. (This value shall maintain even if the language set on device changes after initialization.)
3. `en`, which is the default value of Display Language, is automatically set.

### Country Code

* Gamebase provides (country codes) for the system in the following APIs.
* Please select an appropriate API that best fits your purpose as each API has its own characteristics.

#### USIM Country Code

* Returns a country code written in the USIM.
* Even if a wrong country code has been written in the USIM, it will be returned as it is without any verification.
* 'ZZ' is returned when the value is empty.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static string GetCountryCodeOfUSIM()
```

#### Device Country Code

* Returns the country code received from the OS as it is without any verification.
* The country code of the device is automatically determined by the OS based on the "Language" setting.
* If there are several languages registered, the country code is determined based on the language of top priority.
* 'ZZ' is returned when the value is empty.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetCountryCodeOfDevice()
```

#### Intergrated Country Code

* Verifies and returns the country code in the order of the language settings in the USIM.
* GetCountryCode API operates in the following order:
	1. Checks the country code written in the USIM. If a value exists, returns the value without any separate verification.
	2. If the country code in the USIM is empty, checks the country code of the device. If a value exists, returns the value without any separate verification.
	3. 'ZZ' is returned when the country code values of USIM and device are empty.

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

> [Note]
>
> In case of Editor on Windows or Standalone on Windows, the country code is returned based on the [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2).
>
> In case of Editor on Mac or WebGL, the country code is returned based on the [Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html) value.<br/>For example, when Application.systemLanguage == SystemLanguage.Korean, 'KR' is returned.

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

* Gamebase can process all kinds of events in a single event system called **GamebaseEventHandler**.
* GamebaseEventHandler can simply add or remove a Listener through the API below:

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
	// Represents the type of an event.
    // The value of the GamebaseEventCategory class is assigned.
    public string category;

    // JSON String data that can be converted into a VO that is appropriate for each category.
    public string data;
}
```

**Example**

```cs
public void AddEventHandlerSample()
{
    Gamebase.AddEventHandler(GamebaseObserverHandler);
}

private void GamebaseObserverHandler(GamebaseResponse.Event.GamebaseEventMessage message)
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

* Category is defined in the GamebaseEventCategory class.
* In general, events can be categorized into IdPRevoked, LoggedOut, ServerPush, Observer, Purchase, or Push. GamebaseEventMessage.data can be converted into a VO in the ways shown in the following table for each Category.

| Event type | GamebaseEventCategory | VO conversion method | Remarks |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | GamebaseEventCategory.IDP_REVOKED | GamebaseResponse.Event.GamebaseEventIdPRevokedData.from(message.data) | \- |
| LoggedOut | GamebaseEventCategory.LOGGED_OUT | GamebaseResponse.Event.GamebaseEventLoggedOutData.from(message.data) | \- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED<br>GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseResponse.Event.GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseResponse.Event.GamebaseEventObserverData.from(message.data) | \- |
| Purchase - Promotion payment | GamebaseEventCategory.PURCHASE_UPDATED | GamebaseResponse.Event.PurchasableReceipt.from(message.data) | \- |
| Push - Message received | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) |  |
| Push - Message clicked | GamebaseEventCategory.PUSH_CLICK_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) |  |
| Push - Action clicked | GamebaseEventCategory.PUSH_CLICK_ACTION | GamebaseResponse.Event.PushAction.from(message.data) | Operates when the RichMessage button is clicked. |

#### IdP Revoked

> [Note]
>
> This is an event that only occur when using iOS Appleid login.

* This event occurs when the service is deleted from the IdP.
* Notifies the user that the IdP has been revoked, and issues a new userID when the user logs in with the same IdP.
* GamebaseEventIdPRevokedData.code:  Indicates the GamebaseIdPRevokedCode value.
    * WITHDRAW : 600
        * Indicates that the user is logged in with a revoked IdP, and there is no list of mapped IdPs.
        * You need to call the Withdraw API to remove the current account.
    * OVERWRITE_LOGIN_AND_REMOVE_MAPPING : 601
        * Indicates that the user is logged in with a revoked IdP and IdPs other than the revoked IdP are mapped.
        * You need to log in with one of the mapped IdPs and call the RemoveMapping API to remove mapping with the revoked IdP.
    * REMOVE_MAPPING : 602
        * Indicates that there is a revoked IdP among IdPs mapped to the current account.
        * You need to call the RemoveMapping API to remove mapping with the revoked IdP.
* GamebaseEventIdPRevokedData.idpType: Indicates the revoked IdP type.
* GamebaseEventIdPRevokedData.authMappingList: Indicates the list of IdPs mapped to the current account.

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
                // The user is logged in with a revoked IdP and there is no list of mapped IdPs. 
                // Notifies the user that the current account has been removed.
                Gamebase.Withdraw((error) =>
                {
                    ...
                });
                break;
            }
        case GamebaseIdPRevokedCode.OVERWRITE_LOGIN_AND_REMOVE_MAPPING:
            {
                // The user is logged in with a revoked IdP and IdPs other than the revoked IdP are mapped.
                // Allows the user to select an IdP to login in to among the authMappingList, and removes mapping with the revoked IdP after login with the selected IdP.
                var selectedIdP = "the IdP selected by the user";
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
                // There is a revoked IdP among IdPs mapped to the current account.
                // Notifies the user that mapping with the revoked IdP is removed from the current account.
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

* This event occurs when the Gamebase Access Token has expired and a login function call is required to recover the network session.

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

* This is a message sent from the Gamebase server to the client's device.
* The Server Push Types supported from Gamebase are as follows:
    * GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED
    	* If you register a kickout ServerPush message in **Operation > Kickout** in the NHN Cloud Gamebase console, all clients connected to Gamebase will receive a kickout message.
        * This event occurs immediately after receiving a server message from the client device.
        * It can be used to pause the game when the game is running, as in the case of 'Auto Play'.
    * GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* If you register a kickout ServerPush message in **Operation > Kickout** of the NHN Cloud Gamebase Console, then all clients connected to Gamebase will receive the kickout message.
        * A pop-up is displayed when the client device receives a server message. This event occurs when the user closes this pop-up.
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* If the guest account is successfully transferred to another device, the previous device receives a kickout message.

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

* It is a system used to handle many different status-changing events in Gamebase.
* The Observer Types supported by Gamebase are as follows:
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* It operates when the Launching status is changed, for instance when the server is under maintenance, or the maintenance is over, or a new version is deployed and update is required.
    	* GamebaseEventObserverData.code: Indicates the LaunchingStatus value.
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
    	* Operates when the status of a user account changes, for instance when the user account is deleted or banned.
    	* GamebaseEventObserverData.code: Indicates the GamebaseError value.
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
        * Only Android and iOS
    	* Can receive the information about the changes in the network.
    	* Operates when the network is disconnected or connected, or switched from Wi-Fi to a cellular network.
    	* GamebaseEventObserverData.code: Indicates the NetworkManager value.
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_WEBVIEW
        * Only Standalone
        * Operates when opening or closing the WebView on Standalone.
            * GamebaseWebViewEventType.OPENED: 1
            * GamebaseWebViewEventType.CLOSED: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * Only Standalone and WebGL
        * Operates when session extension fails after login.

**VO**

```cs
public class GamebaseEventObserverData 
{
	// This information represents the status value.
    public int code;

    // This information shows the message about status.
    public string message;

    // A reserved field for additional information.
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
                // Network disconnected.
                break;
            }
        case GamebaseNetworkType.TYPE_MOBILE:
        case GamebaseNetworkType.TYPE_WIFI:
        case GamebaseNetworkType.TYPE_ANY:
            {
                // Network connected.
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
                // You can check the invalid user session in here.
                // ex) After transferred account to another device.
                break;
            }
        case GamebaseErrorCode.BANNED_MEMBER:
            {
				// You can check the banned user session in here.
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
                // WebView opened.
                break;
            }
        case GamebaseWebViewEventType.CLOSED:
            {
                // WebView closed.
                break;
            }
    }
}
```


#### Purchase Updated

* This event is triggered when a product is acquired by redeeming a promotion code.
* Can acquire payment receipt information.

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

* This event is triggered when a push message is received.
* You can also acquire custom information that was sent along with push by converting the extras field to JSON.
    * In **Android**, you can determine whether the message was received in the foreground or in the background through the **isForeground** field.

**VO**

```cs
public class PushMessage 
{
	// The unique ID of a message.
    public string id;

    // The title of the push message.
    public string title;

    // The body of the push message.
    public string body;

    // You can check the custom information sent when sending a push in JSON format.
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

* This event is triggered when a received message is clicked.
* Unlike 'GamebaseEventCategory.PUSH_RECEIVED_MESSAGE', there is no **isForeground** information in the extras field on Android.

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

* This event is triggered when the button created by the Rich Message feature is clicked.
* actionType provides the following:
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```cs
class PushAction 
{
	// Button action type.
    public string actionType;

	// PushMessage data.
    public PushMessage message;

	// User text typed in Push console.
    public string userText;
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

The game index can be transferred to the Gamebase server.

> <font color="red">[Caution]</font><br/>
>
> All APIs supported by the Gamebase Analytics can be called after login.
>
>
> [TIP]
>
> When the Gamebase.Purchase.RequestPurchase API is called and payment is completed, an index is automatically transferred.
>

Please see the following guide for how to use Analytics console.

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

The game user level information can be transmitted as an index after logging in to the game.

> <font color="red">[Caution]</font><br/>
>
> If the SetGameUserData API is not called after login to the game, the level information may be missed from other indexes.
>

Parameters required for calling the API are as follows:

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | Describes the level of game user. |
| channelId | O | string | Describes the channel. |
| characterId | O | string | Describes the name of character. |
| characterClassId | O | string | Describes the occupation. |

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

The game user level information can be transmitted as an index after leveling up.

Parameters required for calling the API are as follows:

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | Describes the level of game user. |
| levelUpTime | M | long | Enter Epoch Time</br>in millisecond. |


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

Gamebase provides features to respond to customer inquiries. 

> [TIP]
>
> Associate it with the NHN Cloud Contact service to easily respond to inquiries from customers.
> See the guide below if you want to know how to use the NHN Cloud Contact service in detail.
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/en/Contact%20Center/en/online-contact-overview/)


#### Customer Service Type

In the **Gamebase Console > App > Customer service**, you can choose from three different types of Customer Centers.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud Online Contact      | △              |

Gamebase SDK's Customer Center API uses the following URLs based on the type:

* Developer's Customer Center
    * URL specified in the **Customer Center URL** field.
* Gamebase's Customer Center
    * Before login: Customer Center URL **without** user information.
    * After login: Customer Center URL with user information.
* NHN Cloud organization product (Online Contact)
    * Before login: Customer Center URL **without** user information.
    * After login: Customer Center URL with user information.

#### Open Contact WebView

Displays the Customer Center WebView.
URL is determined by the customer center type.
You can pass the additional information to the URL using ContactConfiguration.

**GamebaseRequest.Contact.Configuration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | User name (nickname)<br>**default**: null    |
| additionalURL | O             | string                             | Additional URL appended to the developer's own customer center URL<br>**default**: null    |
| additionalParameters | O      | Dictionary<string, string>         | Additional parameters appended to the contact center URL<br>**default**: null    |
| extraData     | O             | Dictionary<string, string>         | Passes the extra data wanted by the developer when opening the customer center<br>**default**: null    |

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
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize has not been called. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | Failed to issue a temporary ticket for user identification. |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | The Customer Center WebView is already being displayed. |

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
            // Please check the url field in the NHN Cloud Gamebase Console.
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

> <font color="red">[Caution]</font><br/>
>
> Contacting the Customer Center may require file attachment.
> To do so, permissions for using the camera or using the storage must be acquired from the user at runtime.
>
> Android user
>
> * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> * Unity users can implement this by referring to the following guide.
> [Unity Guide : Requesting Permissions](https://docs.unity3d.com/2018.4/Documentation/Manual/android-RequestingPermissions.html)
>
> iOS user
>
> * Please set 'Privacy - Camera Usage Description', 'Privacy - Photo Library Usage Description' in info.plist.

#### Request Contact URL

Returns the URL used for displaying the Customer Center WebView.

**API**

```cs
static void RequestContactURL(GamebaseCallback.GamebaseDelegate<string> callback);
static void RequestContactURL(GamebaseRequest.Contact.Configuration configuration, GamebaseCallback.GamebaseDelegate<string> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize has not been called. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | Failed to issue a temporary ticket for user identification. |

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
            // Please check the url field in the NHN Cloud Gamebase Console.
        } 
        else 
        {
            // An error occur when requesting the contact web view url.
        }
    });
}
```
