## Game > Gamebase > Unreal SDK使用ガイド > ETC

## Additional Features

Gamebaseでサポートする付加機能を説明します。

### Device Language

* 端末に設定された言語コードを返します。
* 複数の言語が登録されている場合、優先度が最も高い言語のみを返します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetDeviceLanguageCode() const;
```

### Display Language

* Gamebaseで提供するUIおよびSystemDialogに表示される言語を、端末に設定された言語ではない別の言語に変更できます。
* Gamebaseは、クライアントに含まれているメッセージを表示したり、サーバーから取得したメッセージを表示します。
* DisplayLanguageを設定すると、ユーザーが設定した言語コード(ISO-639)に適合した言語でメッセージを表示します。
* 自由に言語セットを追加できます。追加できる言語コードは次のとおりです。

> [参考]
>
> Gamebaseのクライアントメッセージは英語(en)、韓国語(ko)、日本語(ja)のみ含みます。

#### Gamebaseでサポートする言語コードの種類

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

言語コードは`GamebaseDisplayLanguageCode`クラスに定義されています。

> <font color="red">[注意]</font><br/>
>
> Gamebaseでサポートする言語コードは、大文字/小文字を区別します。
> 「EN」または「zh-cn」と設定すると、問題が発生する場合があります。

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

#### Gamebase初期化時のDisplay Language設定

Gamebase初期化時のDisplay Languageを設定できます。

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

Gamebase初期化時、入力されたDisplay Languageを変更できます。

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

現在適用されているDisplay Languageを照会できます。

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

#### 新規言語セットを追加

Unreal Android、iOSプラットフォームでの新規言語セット追加方法は、下記のガイドを参照してください。

* [Android新規言語セット追加](./aos-etc#display-language)
* [iOS新規言語セット追加](./ios-etc#display-language)

#### Display Language優先順位

初期化およびSetDisplayLanguageCode APIでDisplay Languageを設定する場合、最終適用されるDisplay Languageは、入力した値と異なる値が適用される場合があります。

1. 入力したlanguageCodeがlocalizedstring.jsonファイルに定義されているかを確認します。
2. Gamebase初期化時、端末に設定された言語コードがlocalizedstring.jsonファイルに定義されているかを確認します。(この値は初期化後、端末に設定された言語を変更しても維持されます。)
3. Display Languageのデフォルト値である`en`が自動的に設定されます。

### Country Code

* GamebaseはSystemの国コードを次のようにAPIで提供しています。
* 各APIの特徴があるので、用途に合わせてAPIを選択してください。

#### USIM Country Code

* USIMに記録された国コードを返します。
* USIMに無効な国コードが記録されていても、追加のチェックを行わず、そのまま返します。
* 値が空の場合、「ZZ」を返します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID


```cpp
FString GetCountryCodeOfUSIM() const;
```

#### Device Country Code

* OSから伝達された端末国コードを追加のチェックを行わず、そのまま返します。
* 端末国コードは「言語」設定に応じてOSが自動的に決定します。
* 複数の言語が登録された場合、優先度が最も高い言語で国コードを決定します。
* 値が空の場合、「ZZ」を返します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCodeOfDevice() const;
```

#### Intergrated Country Code

* USIM、端末言語設定の順序で国コードを確認して返します。
* GetCountryCode APIは、次の順序で動作します。
    1. USIMに記録された国コードを確認し、値が存在すれば追加のチェックを行わず、そのまま返します。
    2. USIM国コードが空の場合、端末国コードを確認し、値が存在すれば追加のチェックを行わず、そのまま返します。
    3. USIM、端末国コードがどちらも空の場合は「ZZ」を返します。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)


**API**

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FString GetCountryCode() const;
```

### Server Push
* Gamebaseサーバーからクライアント端末に送るServer Push Messageを処理できます。
* GamebaseクライアントでServerPushEvent Listenerを追加すると、該当メッセージをユーザーが受け取って処理できます。また、追加されたServerPushEvent Listenerを削除できます。


#### Server Push Type
現在GamebaseでサポートするServer Push Typeは次のとおりです。

* GamebaseServerPushType::AppKickout (= "appKickout")
    * TOAST Gamebaseコンソールの**Operation > Kickout**からキックアウトServerPushメッセージを登録すると、Gamebaseと接続されたすべてのクライアントから**APP_KICKOUT**メッセージを受け取ります。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Gamebase ClientにServerPushEventを登録してGamebase ConsoleおよびGamebaseサーバーで発行されたPushイベントを処理できます。

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
Gamebaseに登録されたServerPushEventを削除できます。

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
* Gamebase ObserverでGamebaseの各種状態変動イベントを取得して処理できます。
* 状態変動イベント：ネットワークタイプ変動、Launching状態変動(メンテナンスなどによる状態変動)、Heartbeat情報変動(ユーザー利用停止などによるHeartbeat情報変動)など

#### Observer Type
現在GamebaseでサポートするObserver Typeは次のとおりです。

* Networkタイプ変動
    * ネットワーク変動事項情報を取得できます。
    * Type: GamebaseObserverType::Network (= "network")
    * Code: GamebaseNetworkTypeに宣言された定数を参照します。
        * GamebaseNetworkType::TYPE_NOT: 255
        * GamebaseNetworkType::TYPE_MOBILE: 0
        * GamebaseNetworkType::TYPE_WIFI: 1
        * GamebaseNetworkType::TYPE_ANY: 2
* Launching状態変動
    * 周期的にアプリケーションの状態を確認するLaunching Status responseに変動がある時に発生します。例えば、メンテナンス、アップデート推奨などによるイベントがあります。
    * Type: GamebaseObserverType::Launching (= "launching")
    * Code: GamebaseLaunchingStatusに宣言された定数を参照します。
        * GamebaseLaunchingStatus::IN_SERVICE: 200
        * GamebaseLaunchingStatus::RECOMMEND_UPDATE: 201
        * GamebaseLaunchingStatus::IN_SERVICE_BY_QA_WHITE_LIST: 202
        * GamebaseLaunchingStatus::REQUIRE_UPDATE: 300
        * GamebaseLaunchingStatus::BLOCKED_USER: 301
        * GamebaseLaunchingStatus::TERMINATED_SERVICE: 302
        * GamebaseLaunchingStatus::INSPECTING_SERVICE: 303
        * GamebaseLaunchingStatus::INSPECTING_ALL_SERVICES: 304
        * GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR: 500
* Heartbeat情報変動
    * 周期的にGamebaseサーバーと接続を維持するHeartbeat responseに変動がある時に発生します。例えば、ユーザー利用停止によるイベントがあります。
    * Type: GamebaseObserverType::Heartbeat (= "heartbeat")
    * Code: GamebaseErrorCodeに宣言された定数を参照します。
        * GamebaseErrorCode::INVALID_MEMBER: 6
        * GamebaseErrorCode::BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase ClientにObserverを登録して各種状態変動イベントを処理できます。

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
Gamebaseに登録されたObserverを削除できます。

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

Game指標をGamebase Serverへ転送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIはログイン後に呼び出せます。
>
>
> [TIP]
>
> RequestPurchase APIを呼び出して決済が完了すると、自動的に指標を転送します。
>

Analytics Consoleの使用方法は、下記のガイドを参照してください。

* [Analytics Console](./oper-analytics)

#### Game User Data Settings

ゲームログイン後、ゲームユーザーレベル情報を指標として転送できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後、SetGameUserData APIを呼び出さない場合、他の指標からLevel情報が漏れる場合があります。
>

APIの呼び出しに必要なパラメータは下記のとおりです。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| channelId | O | string | チャンネルを表すフィールドです。 |
| characterId | O | string | キャラクター名を表すフィールドです。 |
| characterClassId | O | string | 職業を表すフィールドです。 |

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

レベルアップした時、ゲームユーザーレベル情報を指標として転送できます。

APIの呼び出しに必要なパラメータは下記の通りです。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| levelUpTime | M | long | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |

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

Gamebaseは、顧客からの問い合わせに対応するための機能を提供します。

> [TIP]
>
> TOAST Contactサービスと連携して使用すると、より簡単に顧客からの問い合わせに対応できます。
> 詳しいTOAST Contactサービス利用方法は、下記のガイドを参照してください。
> [TOAST Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)

#### Open Contact WebView

Gamebaseコンソールに入力した**サポートURL**Webビューを表示する機能です。

* **Gamebaseコンソール > App > InApp URL > Service center**に入力した値が使用されます。

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

