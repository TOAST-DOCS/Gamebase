## Game > Gamebase > Unity SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

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

점검 팝업과 같이 Gamebase 가 표시하는 언어는 단말기에 설정된 언어로 표시됩니다.

그런데 게임에서 표시하는 언어를 단말기에 설정된 언어가 아닌, 별도의 옵션으로 언어를 변경할 수 있는 게임이 있습니다.
이와 같은 게임에서 단말기에 설정된 언어는 `en` 이지만 게임 표시 언어를 `ja` 로 변경한 경우, Gamebase 에서 표시하는 언어도 `ja` 로 변경하고 싶지만 Gamebase 는 단말기 언어인 `en` 으로 메세지를 표시합니다.

이렇게 `단말기에 설정된 언어가 아닌, 강제로 다른 언어로 Gamebase 메세지를 표시하고 싶은` 개발자를 위해 `Display Language` 라는 기능이 제공됩니다.

Gamebase 는 Display Language 로 설정한 언어로 Gamebase 메세지를 표시합니다.
Display Language 에 입력하는 언어 코드는 반드시 아래의 표에 지정된 코드만을 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * Display Language 는 단말기 설정 언어와 무관하게 Gamebase 의 표시 언어를 변경하고 싶은 경우에만 사용하시기 바랍니다.
> * Display Language Code 는 ISO-639 형태의 값으로, 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
> * 만일 Display Language Code 로 입력한 값이 아래의 표(`Gamebase에서 지원하는 언어코드의 종류`)에 존재하지 않는다면, Display Langauge Code 는 자동으로 기본값인 영어(en)로 지정됩니다.

> [참고]
>
> * Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함하고 있으므로 아래의 표에 존재하는 언어 코드라 할지라도 영어(en), 한글(ko), 일본어(ja) 이외의 언어를 지정하면 기본값인 영어(en)로 자동 설정됩니다.
> * Gamebase의 클라이언트에 포함되어 있지 않은 언어셋은 직접 추가할 수 있습니다.
> `신규 언어셋 추가` 항목을 참조하시기 바랍니다.

#### Gamebase에서 지원하는 언어코드의 종류

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

해당 언어코드는 `GamebaseDisplayLanguageCode` 클래스에 정의되어 있습니다.

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

#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

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

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

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

현재 적용된 Display Language를 조회할 수 있습니다.

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

#### 신규 언어셋 추가

UnityEditor 및 Unity Standalone, WebGL 플랫폼 서비스 시, Gamebase에서 제공하는 기본 언어(ko, en) 외 다른 언어를 사용하려면 Assets > StreamingAssets > Gamebase에 있는 localizedstring.json 파일에 값을 추가해야 합니다.

![localizedstring.json](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

localizedstring.json에 정의되어 있는 형식은 아래와 같습니다.

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

다른 언어셋을 추가해야 할 경우에는 localizedstring.json 파일에 `"${언어 코드}":{"key":"value"}` 형태로 값을 추가하면 됩니다.

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

위 JSON 형식에서 "${언어코드}":{ } 내부에 key가 누락될 경우에는 `단말기에 설정된 언어` 또는 `en`이 자동으로 입력됩니다.

Unity Android, iOS 플랫폼에서의 신규 언어셋 추가 방법은 아래 가이드를 참고하십시오.

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

* USIM, 단말기 언어 설정의 순서로 국가 코드를 확인하여 리턴합니다.
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
* Gamebase 서버에서 클라이언트 단말기로 보내는 Server Push Message를 처리할 수 있습니다.
* Gamebase 클라이언트에서 ServerPushEvent Listener를 추가하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent Listener를 삭제할 수 있습니다.


#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.

* GamebaseServerPushType.APP_KICKOUT (= "appKickout")
    * TOAST Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 **APP_KICKOUT** 메시지를 받게 됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Gamebase Client에 ServerPushEvent를 등록하여 Gamebase Console 및 Gamebase 서버에서 발급된 Push 이벤트를 처리할 수 있습니다

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
Gamebase에 등록된 ServerPushEvent를 삭제할 수 있습니다.

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
* Gamebase Observer로 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* 상태 변동 이벤트 : 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등


#### Observer Type
현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.

* Network 타입 변동
    * 네트워크 변동 사항 정보를 받을 수 있습니다.
    * Type: GamebaseObserverType.NETWORK (= "network")
    * Code: GamebaseNetworkType에 선언된 상수를 참고합니다.
        * GamebaseNetworkType.TYPE_NOT: -1
        * GamebaseNetworkType.TYPE_MOBILE: 0
        * GamebaseNetworkType.TYPE_WIFI: 1
        * GamebaseNetworkType.TYPE_ANY: 2
* Launching 상태 변동
    * 주기적으로 애플리케이션 상태를 확인하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어 점검, 업데이트 권장 등에 의한 이벤트가 있습니다.
    * Type: GamebaseObserverType.LAUNCHING (= "launching")
    * Code: GamebaseLaunchingStatus에 선언된 상수를 참고합니다.
        * GamebaseLaunchingStatus.IN_SERVICE: 200
        * GamebaseLaunchingStatus.RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus.REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus.BLOCKED_USER: 301
        * GamebaseLaunchingStatus.TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus.INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat 정보 변동
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어 사용자 이용 정지에 의한 이벤트가 있습니다.
    * Type: GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code: GamebaseErrorCode에 선언된 상수를 참조합니다.
        * GamebaseErrorCode.INVALID_MEMBER: 6
        * GamebaseErrorCode.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase Client에 Observer를 등록하여 각종 상태 변동 이벤트를 처리할 수 있습니다.

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
Gamebase에 등록된 Observer를 삭제할 수 있습니다.

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

레벨업이 되었을 경우 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 게임 유저 레벨을 나타내는 필드입니다. |
| levelUpTime | M | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |

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