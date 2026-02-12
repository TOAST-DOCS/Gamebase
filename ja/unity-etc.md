## Game > Gamebase > Unity SDK ご利用ガイド > ETC

## Additional Features

Gamebaseで対応している付加機能について説明します。

### Device Language

* 端末に設定されている言語コードを返します。
* 複数の言語が登録されている場合、優先権が最も高い言語だけを返します。

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
> Editor on Windows, Standalone on Windowsの場合には[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)を参照して言語コードを返します。
>
> Editor on Mac, WebGLは[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)の値を参照して言語コードを返します。<br/>例えば、Application.systemLanguage == SystemLanguage.Koreanの場合は'ko'を返します。


### Display Language

メンテナンスポップアップなどでGamebaseが表示する言語は、端末に設定された言語と同じです。

しかしゲームで表示する言語を、端末に設定した言語ではなく、別のオプションで変更できるゲームがあります。
例えば、端末に設定された言語は英語ですが、ゲーム表示言語を日本語に変更した場合、 Gamebaseで表示する言語も日本語に変更したいですが、Gamebaseが表示する言語は端末に設定された言語の英語が表示されます。

このように`端末に設定された言語ではなく、他の言語でGamebaseのメッセージを表示したい`アプリケーションのためにGamebaseは`Display Language`という機能を提供します。

GamebaseはDisplay Languageで設定した言語でGamebaseのメッセージを表示します。
Display Languageに入力する言語コードは、以下の表(**Gamebaseでサポートする言語コードの種類**)で指定したコードだけを使用できます。

> <font color="red">[注意]</font><br/>
>
> * Display Languageは、端末設定言語と関係なくGamebaseの表示言語を変更したい場合にのみ使用してください。
> * Display Language CodeはISO-639形式の値で、大文字/小文字を区別します。
> 'EN'または'zh-cn'と設定すると問題が発生する場合があります。
> * もしDisplay Language Codeに入力した値が以下の表(**Gamebaseでサポートする言語コードの種類**)に存在しない場合、Display Langauge Codeは自動的にデフォルト値の英語(en)が指定されます。

> [Note]
>
> * Gamebaseのクライアントメッセージは英語(en)、韓国語(ko)、日本語(ja)のみ含んでいるため、下記の表に存在する言語コードであっても、英語(en)、韓国語(ko)、日本語(ja)以外の言語を指定するとデフォルト値の英語(en)に自動設定されます。
> * Gamebaseのクライアントに含まれていない言語セットは直接追加できます。
> **新規言語セット追加**項目を参照してください。

#### Gamebaseでサポートしている言語コードの種類

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

該当する言語コードは、`GamebaseDisplayLanguageCode`クラスに定義されています。

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

UnityEditor及びUnity Standalone、WebGLプラットフォームサービスを提供する際にGamebaseで提供するデフォルト言語(ko、en、ja)以外に他の言語を使用しなければならない場合、Assets > StreamingAssets > Gamebaseにあるlocalizedstring.jsonファイルに値を追加しなければなりません。

![localizedstring.json](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-etc_001_1.11.0.png)

localizedstring.jsonに定義されている形式は、次の通りです。

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

他の言語の追加が必要な場合、localizedstring.jsonファイルに`"${言語コード}":{"key":"value"}`の形で値を追加してください。

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
    "launching_service_closed_title": "サービス終了"
  },
  "${言語コード}": {
      "common_ok_button": "...",
      ...
  }
}
```

Unity Android、iOSプラットフォームでの言語セットの新規追加方法は、次のガイドをご参考ください。

* [Android言語セットの新規追加](./aos-etc#display-language)
* [iOS言語セットの新規追加](./ios-etc#display-language)

#### Display Languageの優先順位

初期化及びSetDisplayLanguageCodeAPIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。

1. 入力されたlanguageCodeがlocalizedstring.jsonファイルに定義されているかどうかを確認します。
2. Gamebaseを初期化する際に、デバイスに設定されている言語コードがlocalizedstring.jsonファイルに定義されているかどうかを確認します。(この値は、初期化後にデバイスに設定されている言語を変更した場合でも維持されます。)
3. Display Languageのデフォルト値である`en`が自動で設定されます。

### Country Code

* Gamebaseは、システムの国コード(country code)を次のようなAPIで提供しています。
* APIごとに特徴があるため、使用用途に合ったAPIを選択してください。

#### USIM Country Code

* USIMに記録された国コードを返します。
* USIMに無効な国コードが記録されていても、確認せずにそのまま返します。
* 値が空の場合、'ZZ'を返します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID


```cs
static string GetCountryCodeOfUSIM()
```

#### Device Country Code

* OSから伝達された端末国コードを、確認しないでそのまま返します。
* 端末国コードは、'言語'設定に応じてOSが自動的に決定します。
* 複数の言語が登録されている場合、優先権が最も高い言語で国コードを決定します。
* 値が空の場合、'ZZ'を返します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static string GetCountryCodeOfDevice()
```

#### Intergrated Country Code

* USIM、端末の言語設定の順序で国コードを確認して返します。
* getCountryCode APIは次の順序で動作します。
	1. USIMに記録されている国コードを確認し、値があれば別途確認しないでそのまま返します。
	2. USIM国コードが空の値の場合、端末国コードを確認し、値があれば別途確認しないでそのまま返します。
	3. USIM、端末国コードがどちらも空の値の場合は、'ZZ'を返します。

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)
sb
> [参考] 
>
> Editor on Windows, Standalone on Windowsの場合は、[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo?view=netframework-4.7.2)を参照して国コードを返します。
>
> Editor on Mac, WebGLは、[Application.systemLanguage](https://docs.unity3d.com/ScriptReference/SystemLanguage.html)の値を参照して国コードを返します。<br/>例えば、Application.systemLanguage == SystemLanguage.Koreanの場合は'KR'を返します。

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

* Gamebaseは各種イベントを**GamebaseEventHandler**という1つのイベントシステムで全て処理できます。
* GamebaseEventHandlerは以下のAPIを利用して簡単にListenerを追加/削除できます。

* GamebaseEventHandlerは下記のAPIを利用して簡単にHandlerを追加/削除できます。

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
	// Eventの種類を表します。
    // GamebaseEventCategoryクラスの値が割り当てられます。
    public string category;

    // 各categoryに合ったVOに変換できるJSON Stringデータです。
     public string data;
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

* CategoryはGamebaseEventCategoryクラスに定義されています。
* イベントは大きくIdPRevoked、LoggedOut、ServerPush、Observer、Purchase、Pushに分けられ、各Categoryに基づいて、GamebaseEventMessage.dataを次の表のような方法でVOに変換できます。

| Event種類 | GamebaseEventCategory | VO変換方法 | 備考 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | GamebaseEventCategory.IDP_REVOKED | GamebaseResponse.Event.GamebaseEventIdPRevokedData.from(message.data) | \- |
| LoggedOut | GamebaseEventCategory.LOGGED_OUT | GamebaseResponse.Event.GamebaseEventLoggedOutData.from(message.data) | \- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED<br>GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseResponse.Event.GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseResponse.Event.GamebaseEventObserverData.from(message.data) | \- |
| Purchase - プロモーション決済 | GamebaseEventCategory.PURCHASE_UPDATED | GamebaseResponse.Event.PurchasableReceipt.from(message.data) | \- |
| Push - メッセージ受信 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) | |
| Push - メッセージクリック | GamebaseEventCategory.PUSH_CLICK_MESSAGE | GamebaseResponse.Event.PushMessage.from(message.data) | |
| Push - アクションクリック | GamebaseEventCategory.PUSH_CLICK_ACTION | GamebaseResponse.Event.PushAction.from(message.data) | RichMessageボタンを押すと動作します。 |

#### IdP Revoked

> [参考]
>
> iOS Appleidログインを使用する場合にのみ発生するイベントです。

* IdPで該当サービスを削除したときに発生するイベントです。
* ユーザーにIdPが利用停止となったことを通知し、ログアウトしてから再度ログインするように実装する必要があります。

* GamebaseEventIdPRevokedData.idpType：使用停止しているIdPタイプを意味します。
* GamebaseEventIdPRevokedData.authMappingList：現在アカウントにマッピングされているIdPリストを意味します。

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
                    // Call logout, then login again.
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

#### Logged Out

* Gamebase Access Tokenの有効期限が切れてネットワークセッションを復元するためにログイン関数の呼び出しが必要な場合に発生するイベントです。

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

* Gamebaseサーバーからクライアント端末へ送信するメッセージです。
* GamebaseでサポートするServer Push Typeは次の通りです。
    * GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseと接続したすべてのクライアントでキックアウトメッセージを受け取ります。
        * クライアント端末でサーバーメッセージを受信した直後に発生するイベントです。
        * 「オートプレイ」のようにゲームが動作中の場合、ゲームを一時停止させる目的で活用できます。
    * GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受信します。
        * クライアント端末でサーバーメッセージを受信した時、ポップアップを表示しますが、ユーザーがポップアップを閉じたときに発生するイベントです。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guestアカウントを他の端末へ移行すると、以前の端末でキックアウトメッセージを受信します。

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

* Gamebase Gamebaseの各種状態変動イベントを処理するシステムです。
* GamebaseでサポートするObserver Typeは次の通りです。
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* メンテナンスが開始または終了した場合、新しいバージョンが配布されてアップデートが必要な場合など、Launching状態が変更された時に動作します。
    	* GamebaseEventObserverData.code : LaunchingStatus値を意味します。
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
    	* 退会処理や利用停止により、ユーザーアカウントの状態が変わった時に動作します。
    	* GamebaseEventObserverData.code : GamebaseError値を意味します。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
        * Android, iOSに限る
    	* ネットワーク変動事項情報を受け取れます。
    	* ネットワークが切断されたり、接続された時、またはWi-FiからCellularネットワークに変更された時に動作します。
    	* GamebaseEventObserverData.code : NetworkManager値を意味します。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2
    * GamebaseEventCategory.OBSERVER_WEBVIEW
        * Standaloneに限る
        * StandaloneでWebビューを開閉した時に動作します。
            * GamebaseWebViewEventType.OPENED: 1
            * GamebaseWebViewEventType.CLOSED: 2
    * GamebaseEventCategory.OBSERVER_INTROSPECT
        * Standalone, WebGLに限る
        * ログイン後、セッション延長に失敗した場合に動作します。

**VO**

```cs
public class GamebaseEventObserverData 
{
	// 状態値を表す情報です。
    public int code;

    // 状態に関連するメッセージ情報です。
    public string message;

    // 追加情報用の予備フィールドです。
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

* App Store 프로모션 상품 구매 완료 또는 Ask to Buy 등으로 지연된 결제가 완료되었을 때 발생하는 이벤트입니다.
* 決済領収書情報を取得できます。

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
                    // If a promotion or pending purchase is completed, this event will be occurred.
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

* Pushメッセージが到着した時に発生するイベントです。
* extrasフィールドをJSONに変換して、Push送信時に送信したカスタム情報を取得することもできます。
    * **Android**では**isForeground**フィールドでフォアグラウンドでメッセージを受信したのか、バックグラウンドでメッセージを受信したのかを区別できます。

**VO**

```cs
public class PushMessage 
{
	// メッセージ固有のidです。
    public string id;

    // Pushメッセージタイトルです。
    public string title;

    // Pushメッセージ本文内容です。
    public string body;

    // JSON形式でPush送信時、送信したカスタム情報を確認できます。
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

* 受信したPushメッセージをクリックした時に発生するイベントです。
* 'GamebaseEventCategory.PUSH_RECEIVED_MESSAGE'とは異なり、Androidのextrasフィールドに**isForeground**情報が存在しません。

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

* Rich Message機能を利用して作成したボタンをクリックした時に発生するイベントです。
* actionTypeは、次の項目が提供されます。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```cs
class PushAction 
{
	// ボタンアクション種類です。
    public string actionType;

	// PushMessageデータです。
    public PushMessage message;

	// Pushコンソールで入力したユーザーテキストです。
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

Game指標をGamebase Serverに伝送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIは、ログイン後に呼び出すことができます。
>
>
> [TIP]
>
> Gamebase.Purchase.RequestPurchase APIを呼び出して決済を完了すると、自動的に指標を伝送します。
>

Analyticsコンソールの使用方法は、下記のガイドを参照してください。

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

ゲームログイン後、ゲームユーザーレベル情報を指標として伝送できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後にsetGameUserData APIを呼び出さない場合、他の指標でレベル情報が抜ける場合があります。
>

APIの呼び出しに必要なパラメータは下記の通りです。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| channelId | O | string | チャンネルを表すフィールドです。 |
| characterId | O | string | キャラクター名を表すフィールドです。 |
| characterClassId | O | string | 職業を表すフィールドです。 |

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

レベルアップすると、ゲームユーザーレベル情報を指標として伝送できます。

APIの呼び出しに必要なパラメータは下記の通りです。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| levelUpTime | O | long | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |

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

Gamebaseは、顧客の問い合わせに対応するための機能を提供します。

> [TIP]
>
> NHN Cloud Contactサービスと連携して使用すると、顧客からの問い合わせに簡単に対応できます。
> 詳細はNHN Cloud Contactサービスの利用ガイドを参照してください。
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ja/Contact%20Center/ja/online-contact-overview/)

#### 権限設定

* [Game > Gamebase > Android SDK使用ガイド > ETC > Contact](aos-etc/#contact)
* [Game > Gamebase > iOS SDK使用ガイド > ETC > Contact](ios-etc/#contact)


#### Customer Service Type

**Gamebase コンソール > App > Customer service**で、以下の3つのタイプのサポートを選択できます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud Online Contact      | △              |

タイプに応じてGamebase SDKのサポートAPIは次のURLを使用します。

* 開発会社独自のサポート(Developer customer center)
    * **サポートURL**に入力したURL.
* Gamebase提供サポート(Gamebase customer center)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。
* NHN Cloud組織商品(Online Contact)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。

#### Open Contact WebView

サポートWebビューを表示します。
URLはサポートタイプに基づいて決定されます。
ContactConfigurationでURLに追加情報を伝達できます。

**GamebaseRequest.Contact.Configuration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | ユーザー名(ニックネーム)<br>**default** : null    |
| additionalURL | O             | string                             | 開発会社独自のサポートURLの後ろにつく追加のURL<br>**default** : null    |
| additionalParameters | O      | Dictionary<string, string>         | サポートURLの後ろにつく追加のパラメータ<br>**default** : null    |
| extraData     | O             | Dictionary<string, string>         | 開発会社が任意のextra dataをサポートオープン時に伝達<br>**default** : null    |

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
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | サポートWebビューがすでに表示中です。 |

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

#### Request Contact URL

サポートWebビューを表示するのに使用されるURLをリターンします。

**API**

```cs
static void RequestContactURL(GamebaseCallback.GamebaseDelegate<string> callback);
static void RequestContactURL(GamebaseRequest.Contact.Configuration configuration, GamebaseCallback.GamebaseDelegate<string> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |

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

### App Tracking AuthorizationStatus

* ATTが有効かどうかを確認します。

* AUTHORIZED : アプリのトラッキング要求を許可、iOS 14未満の端末では常にAUTHORIZEDを返却
* DENIED: アプリのトラッキング要求を拒否
* NOT_DETERMINED : アプリのトラッキング要求許可が未決定
* RESTRICTED : アプリのトラッキング要求を制限
* UNKNOWN : 他のOSであるか、OSで定義されていない場合

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
namespace Toast.Gamebase
{
    public enum GamebaseAppTrackingAuthorizationStatus
    {
        AUTHORIZED,
        DENIED,
        NOT_DETERMINED,
        RESTRICTED,
        UNKNOWN
    }
}

static Toast.Gamebase.GamebaseAppTrackingAuthorizationStatus GetAppTrackingAuthorizationStatus();
```

**Example**

``` cs
public void GetAppTrackingAuthorizationStatusSample()
{
#if UNITY_IOS
    switch (Gamebase.Util.GetAppTrackingAuthorizationStatus() ) iOS only
    {
        case GamebaseAppTrackingAuthorizationStatus.AUTHORIZED:
        {
        }
        break; 
        
        case GamebaseAppTrackingAuthorizationStatus.DENIED:
        {
        }
        break;
        
        case GamebaseAppTrackingAuthorizationStatus.NOT_DETERMINED:
        {
        }
        break;
        
        case GamebaseAppTrackingAuthorizationStatus.RESTRICTED:
        {
        }
        break;
        
        case GamebaseAppTrackingAuthorizationStatus.UNKNOWN:
        {
        }
        break;
    }
#endif
}
```

### IDFA

* 端末の広告識別子(IDFA)の値を返却します。

iOSでIDFA機能を設定する方法は、次のドキュメントを参照してください。<br/>
* [iOS IDFA](./ios-etc/#idfa)<br/>

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetIdfa();
```

**Example**

``` cs
public void SampleGetIdfa()
{
#if UNITY_IOS
    string idfa = Gamebase.Util.GetIdfa(); iOS only
#endif
}
```

### Age Signals Support

Texas SB 2420及び類似する州の法律は、未成年者の保護のためにアプリでユーザーの年齢確認を求めています。
Gamebaseは、Google Play Age Signals APIをラッピングし、このような要件を満たすAPIを提供します。

AndroidでAge Signals機能を設定する方法は、次のドキュメントを参照してください。<br/>
* [Android Age Signals](./aos-etc/#age-signals-support)<br/>
  
Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

#### GetAgeSignal

年齢情報を確認します。

**API**

```cs
static void GetAgeSignal(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Util.AgeSignalResult> callback)
            HandleUnknownUser(result);
            break;
    }
}
```
