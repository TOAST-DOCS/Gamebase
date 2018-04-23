## Game > Gamebase > Unity SDK ご利用ガイド > ETC

## Additional Features

Gamebaseで対応している付加機能について説明します。

### Display Language

* Gamebaseで提供するUI及びSystemDialogに表示される言語をデバイスに設定されている言語ではなく、ユーザーが設定した言語に変更することができます。
* Gamebaseは、クライアントに含まれているメッセージを表示したり、サーバーから取得したメッセージを表示します。
* DisplayLanguageを設定すると、ユーザーが設定した言語コード(ISO-639)に対応する言語でメッセージを表示します。
* 必要な場合、ユーザーがサポートしたい言語セットを追加することができます。(下の対応言語コードを参考)

> [参考]
>
> Gamebaseのクライアントメッセージには、英語(en)、韓国語(ko)のみ含まれます。

#### Gamebaseでサポートしている言語コードの種類

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

該当する言語コードは、`GamebaseDisplayLanguageCode`クラスに定義されています。

> `[注意]`
>
> Gamebaseでサポートしている言語コードは、大文字・小文字を区別します。
> “EN”や"zh-cn"のように設定する場合、問題が発生することがあります。

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

#### Gamebaseを初期化する際のDisplay Languageの設定

Gamebaseを初期化する際にDisplay Languageを設定することができます。

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

Gamebaseを初期化する際に入力されたDisplay Languageを変更することができます。

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

現在適用されているDisplay Languageを照会することができます。

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

#### 言語セットの新規追加

UnityEditor及びUnity Standalone、WebGLプラットフォームサービスを提供する際にGamebaseで提供するデフォルト言語(ko、en)以外に他の言語を使用しなければならない場合、Assets > StreamingAssets > GamebaseにあるlocalizedString.jsonファイルに値を追加しなければなりません。

![localizedString.json](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.7.0.png)

localizedString.jsonに定義されている形式は、次の通りです。

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
  }
}
```

日本語の追加が必要な場合、localizedString.jsonファイルに`"ja":{"key":"value"}`の形で値を追加してください。

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
  "ja":{
    "common_ok_button":"確認",
    "common_cancel_button":"キャンセル",
    ...
    "launching_service_closed_title":"サービス終了"
  }
}
```

上記のjson形式で"ja":{ }内部にkeyが抜けている場合、「デバイスに設定されている言語」または`en`で自動入力されます。

Unity Android、iOSプラットフォームでの言語セットの新規追加方法は、次のガイドをご参考ください。

* [Android言語セットの新規追加](./aos-etc#display-language)
* [iOS言語セットの新規追加](./ios-etc#display-language)

#### Display Languageの優先順位

初期化及びSetDisplayLanguageCodeAPIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。


1. 入力されたlanguageCodeがlocalizedString.jsonファイルに定義されているかどうかを確認します。
2. Gamebaseを初期化する際に、デバイスに設定されている言語コードがlocalizedString.jsonファイルに定義されているかどうかを確認します。(この値は、初期化後にデバイスに設定されている言語を変更した場合でも維持されます。)
3. Display Languageのデフォルト値である`en`が自動で設定されます。


### Server Push
* Gamebase 서버에서 클라이언트 기기로 보내는 Server Push Message를 처리할 수 있습니다.
* Gamebase 클라이언트에서 ServerPushEvent Listener를 추가 하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent Listener를 삭제 할 수 있습니다.


#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.

* 킥아웃 (Kickout)
    * TOAST Gamebase 콘솔의 `Operation > Kickout` 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에게 메시지를 보낼 수 있습니다.
    * Type : GamebaseServerPushType.APP_KICKOUT (= "appKickout")


#### Add ServerPushEvent
Gamebase에 ServerPushEvent를 등록하여 처리할 수 있습니다.

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
    GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ServerPushMessage> serverPushEvent = (data) =>
    {
        GamebaseResponse.SDK.ServerPushMessage serverPushMessage = data;
    };

    Gamebase.AddServerPushEvent(serverPushEvent);
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
    Gamebase.RemoveServerPushEvent(observer);
}

public void RemoveAllServerPushEvent()
{
    Gamebase.RemoveAllServerPushEvent();
}
```

### Observer
* Gamebase Observer를 통하여 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* Observer를 추가하면 들어 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등에 대한 이벤트를 사용자가 전달받아 처리 할 수 있습니다.


#### Observer Type
현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.

* Network 타입 변동
    * 네트워크 변동사항에 대한 정보를 받을 수 있습니다.
    * Type : GamebaseObserverType.NETWORK (= "network")
    * Code : GamebaseNetworkType에 선언된 상수를 참고합니다.
        * GamebaseNetworkType.TYPE_NOT : -1
        * GamebaseNetworkType.TYPE_MOBILE : 0
        * GamebaseNetworkType.TYPE_WIFI : 1
        * GamebaseNetworkType.TYPE_ANY : 2
* Launching 상태 변동
    * 주기적으로 어플리케이션의 상태를 체크하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어서, 점검, 업데이트 권장 등에 의한 이벤트가 있습니다.
    * Type : GamebaseObserverType.LAUNCHING (= "launching")
    * Code : GamebaseLaunchingStatus에 선언된 상수를 참고합니다.
        * GamebaseLaunchingStatus.IN_SERVICE : 200
        * GamebaseLaunchingStatus.RECOMMEND_UPDATE : 201
        * GamebaseLaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST : 202
        * GamebaseLaunchingStatus.REQUIRE_UPDATE : 300
        * GamebaseLaunchingStatus.BLOCKED_USER : 301
        * GamebaseLaunchingStatus.TERMINATED_SERVICE : 302
        * GamebaseLaunchingStatus.INSPECTING_SERVICE : 303
        * GamebaseLaunchingStatus.INSPECTING_ALL_SERVICES : 304
        * GamebaseLaunchingStatus.INTERNAL_SERVER_ERROR : 500
* Heartbeat 정보 변동
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어서, 사용자 이용 정지에 의한 이벤트가 있습니다.
    * Type : GamebaseObserverType.HEARTBEAT (= "heartbeat")
    * Code : GamebaseErrorCode에 선언된 상수를 참조합니다.
        * GamebaseErrorCode.BANNED_MEMBER : 7


#### Add Observer
Gamebase에 Observer를 등록하여 처리할 수 있습니다.

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
	GamebaseCallback.DataDelegate<GamebaseResponse.SDK.ObserverMessage> observer = (data) =>
    {
        GamebaseResponse.SDK.ObserverMessage observerMessage = data;
    };

    Gamebase.AddObserver(observer);
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

