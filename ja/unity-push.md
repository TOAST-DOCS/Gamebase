## Game > Gamebase > Unity SDK ご利用ガイド > Push

ここではプラットフォームごとにPush通知を使用するために必要な設定方法についてご案内いたします。

### Settings


AndroidやiOSでPushを設定する方法は、次のドキュメントを参照してください。<br/>

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/#firebase-notification)
* iOS
    * [iOS Push Settings](ios-push#settings)


### Register Push

次のAPIを呼び出してNHN Cloud Pushに該当するユーザーを登録します。
Pushの同意状態(enablePush)、Push型広告の同意状態(enableAdPush)、夜間のPush型広告の同意状態(enableAdNightPush)の値をユーザーから取得し、次のAPIを呼び出して登録を完了させます。

> <font color="red">[注意]</font><br/>
>
> UserIDごとにプッシュ設定が異なる場合があり、プッシュトークンの有効期限切れも発生することがあるため、ログイン後は毎回registerPush APIを呼び出すことを推奨します。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback);
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseRequest.Push.NotificationOptions options, GamebaseCallback.ErrorDelegate callback);
```

**Example**

```cs
public void RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    GamebaseRequest.Push.PushConfiguration pushConfiguration = new GamebaseRequest.Push.PushConfiguration();
    pushConfiguration.pushEnabled = pushEnabled;
    pushConfiguration.adAgreement = adAgreement;
    pushConfiguration.adAgreementNight = adAgreementNight;

	// You should receive the above values to the logged-in user.

    Gamebase.Push.RegisterPush(pushConfiguration, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

#### Setting for APNS Sandbox
* SandboxModeを有効にすると、APNS SandboxへPushを送信するように登録できます。
* コンソールから送信する方法
    * Pushメニューの**対象**から**iOS Sandbox**を選択した後、送信します。

### Notification Options

* 端末に表示する通知をどのような形式で表示するかをNotification Optionsで変更できます。
* 実行時にregisterPush APIを呼び出して変更できます。

#### Set Notification Options with RegisterPush in Runtime

RegisterPush APIを呼び出す時、GamebaseRequest.Push.NotificationOptions引数を追加して通知オプションを設定できます。
GamebaseRequest.Push.NotificationOptionsの作成者にGamebase.Push.GetNotificationOptions()呼び出し結果を伝達すると、現在の通知オプションで初期化されたオブジェクトが作成されるため、必要な値のみ変更できます。<br/>
設定可能な値は次の通りです。

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | アプリがフォアグラウンド状態の時の通知表示有無<br/>**default**: false |
| badgeEnabled           | bool         | バッジアイコン使用有無<br/>**default**: true |
| soundEnabled           | bool         | 通知音使用有無<br/>**default**: true<br/>**iOS Only** |
| priority               | int          | 通知の優先順位。以下の5つの値を設定できます。<br/>GamebaseNotificationPriority.MIN : -2<br/> GamebaseNotificationPriority.LOW : -1<br/>GamebaseNotificationPriority.DEFAULT : 0<br/>GamebaseNotificationPriority.HIGH : 1<br/>GamebaseNotificationPriority.MAX : 2<br/>**default**: GamebaseNotificationPriority.HIGH<br/>**Android Only** |
| smallIconName          | string       | 通知用の小さいアイコンのファイル名。<br/>設定していない場合、アプリアイコンが使用されます。<br/>**default**: null<br/>**Android Only** |
| soundFileName          | string       | 通知音ファイル名。 Android 8.0未満のOSでのみ動作します。<br/>'res/raw'フォルダのmp3、wavファイル名を指定すると、通知音が変更されます。<br/>**default**: null<br/>**Android Only** |

**Example**

```cs
public void RegisterPushSample(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    var configuration = new GamebaseRequest.Push.PushConfiguration
    {
        pushEnabled = pushEnabled,
        adAgreement = adAgreement,
        adAgreementNight = adAgreementNight,
        displayLanguageCode = GamebaseDisplayLanguageCode.Korean
    };

    var notificationOptions = new GamebaseRequest.Push.NotificationOptions
    {
        foregroundEnabled = true,
        priority = GamebaseNotificationPriority.HIGH
    };

    Gamebase.Push.RegisterPush(configuration, notificationOptions, (error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

#### Get NotificationOptions


**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static GamebaseResponse.Push.NotificationOptions GetNotificationOptions();
```

**Example**

```cs
public void GetNotificationOptionsSample()
{
    GamebaseResponse.Push.NotificationOptions notificationOptions = Gamebase.Push.GetNotificationOptions();

    GamebaseRequest.Push.NotificationOptions options = new GamebaseRequest.Push.NotificationOptions(notificationOptions);
    options.foregroundEnabled = true;
    options.smallIconName = "notification_icon_name";

    GamebaseRequest.Push.PushConfiguration configuration = new GamebaseRequest.Push.PushConfiguration();
    Gamebase.Push.RegisterPush(configuration, options, (error)=> { });
}
```

### Request Push Settings

ユーザーのPush設定を照会するために、次のAPIを利用します。<br/>
コールバックによるGamebaseResponse.Push.TokenInfoの値からユーザー設定値を取得することができます。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void QueryTokenInfo(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.TokenInfo> callback);

// Legacy API
public static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback);
```

**Example**

```cs
public void QueryTokenInfoSample(bool isSandbox)
{
    Gamebase.Push.QueryTokenInfo((data, error)=> 
    {
        if (Gamebase.IsSuccess(error)) 
        {
            // Succeeded.
            bool enablePush = data.agreement.pushEnabled;
            bool enableAdPush = data.agreement.adAgreement;
            bool enableAdNightPush = data.agreement.adAgreementNight;
            string token = data.token;
        } 
        else 
        {
        // Failed.
        }
    });
}
```

#### GamebaseResponse.Push.TokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | string                | Pushトークンタイプ    |
| token               | string                | トークン              |
| userId              | string                | ユーザーID         |
| deviceCountryCode   | string                | 国コード        |
| timezone            | string                | 標準時間帯        |
| registeredDateTime  | string                | トークンアップデート時間 |
| languageCode        | string                | 言語設定         |
| agreement           | GamebaseResponse.Push.Agreement | 受信同意有無       |
| sandbox             | bool                  | sandboxかどうか(iOS Only)        |

#### GamebaseResponse.Push.Agreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 通知表示同意有無          |
| adAgreement      | bool | 広告性通知表示同意有無     |
| adAgreementNight | bool | 夜間広告性通知表示同意有無 |

### Event Handling

* プッシュメッセージが到着した場合、またはプッシュメッセージをクリックしたときにイベント処理を行うことができます。
* イベント登録方法はGamebaseEventHandlerガイドを参照してください。
    * [ Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./unity-etc/#push-received-message)
    * [ Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./unity-etc/#push-click-message)
    * [ Game > Gamebase > Unity SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./unity-etc/#push-click-action)

#### Setting for APNS Sandbox
* SandboxModeをオンにすると、APNS SandboxでPushを送信するように登録できます。
* コンソール送信方法
    * Pushメニューの**対象**から**iOS Sandbox**を選択した後に送信します。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
public static void SetSandboxMode(bool isSandbox)
```

**Example**

```cs
public void SetSandboxModeSample()
{
    Gamebase.Push.SetSandboxMode(true);
}
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 前回のPush APIの呼び出しが完了していません。<br> 前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|


* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud Pushライブラリーで発生したエラーです。
* エラーコードは次のように確認できます。

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* NHN Cloud Pushのエラーコードを確認してください。
    * [Android](aos-push#error-handling)<br/>
    * [iOS](ios-push#error-handling)
