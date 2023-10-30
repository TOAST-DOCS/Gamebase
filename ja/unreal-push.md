## Game > Gamebase > Unreal SDK使用ガイド > プッシュ

ここでは各プラットフォームでプッシュ通知を使用するのに必要な設定方法を説明します。

### Settings

AndroidやiOSでプッシュを設定する方法は、次の文書を参照してください。

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/#firebase-notification)
        * Androidビルド時にres/values/google-services-json.xmlファイル含まれている必要があるため、ガイドを参考にして作成します。
        * Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xmlパスにファイルがあります。
* iOS
    * [iOS Push Settings](ios-push#settings)

> <font color="red">[注意]</font><br/>
>
> 外部プラグインでプッシュ関連処理がある場合、Gamebaseプッシュ機能が正常に動作しない可能性があります。

### Register Push

次のAPIを呼び出して、TOAST Pushに該当ユーザーを登録します。
プッシュ同意(enablePush)、広告性プッシュ同意(enableAdPush)、夜間広告性プッシュ同意(enableAdNightPush)値をユーザーから受け取り、次のAPIを呼び出して登録を完了します。

> <font color="red">[注意]</font><br/>
>
> UserIDごとにプッシュ設定が異なる場合があり、プッシュトークンの有効期限切れも発生することがあるため、ログイン後は毎回registerPush APIを呼び出すことを推奨します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseErrorDelegate& onCallback);
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseNotificationOptions& notificationOptions, const FGamebaseErrorDelegate& onCallback);
```

#### FGamebasePushConfiguration
| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| pushEnabled                   | M             | bool         | プッシュ同意の有無 |
| adAgreement                   | M             | bool         | 広告性プッシュ同意の有無 |
| ADAgreemadAgreementNightentNight | M          | bool         | 夜間広告性プッシュ同意の有無 |
| requestNotificationPermission | O             | bool         | Android 13以上のOSでRegisterPush APIを呼び出した場合、Push権限リクエストポップアップを自動出力するかどうか<br>**default**: true<br/>**Androidに限る** |
| alwaysAllowTokenRegistration  | O             | bool         | ユーザーがプッシュ権限を拒否してもトークンを登録するかどうか<br>trueに設定した場合、プッシュ権限を取得できなくてもトークンを登録します。<br>**default**: false<br/>**iOSに限る** |

**Example**

```cpp
void Sample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration Configuration;
    Configuration.pushEnabled = pushEnabled;
    Configuration.adAgreement = adAgreement;
    Configuration.adAgreementNight = adAgreementNight;
    Configuration.requestNotificationPermission = bRequestNotificationPermission;
    Configuration.alwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;

    IGamebase::Get().GetPush().RegisterPush(Configuration, FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

### Notification Options

* 端末に表示する通知をどのような形式で表示するかをNotification Optionsで変更できます。
* Notification Optionsは、AndroidManifest.xmlに設定したり、ランタイムにregisterPush APIを呼び出して変更できます。

#### Set Notification Options with RegisterPush in Runtime

RegisterPush APIを呼び出す時、FGamebaseNotificationOptionsの引数を追加して通知オプションを設定できます。
FGamebaseNotificationOptionsの作成者にIGamebase::Get().GetPush().GetNotificationOptions()呼び出し結果を渡すと、現在の通知オプションで初期化されたオブジェクトが作成されるため、必要な値のみ変更できます。<br/>
設定可能な値は次のとおりです。

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | アプリがフォアグラウンド状態の時の通知表示有無<br/>**default**: false |
| badgeEnabled           | bool         | バッジアイコン使用有無<br/>**default**: true |
| soundEnabled           | bool         | 通知音使用有無<br/>**default**: true<br/>**iOS Only** |
| priority               | int32        | 通知の優先順位。以下の5つの値を設定できます。<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| smallIconName          | FString       | 通知用の小さなアイコンファイル名。<br/>設定しない場合、アプリアイコンが使用されます。<br/>**default**: null<br/>**Android Only** |
| soundFileName          | FString       | 通知音ファイル名。Android 8.0未満のOSでのみ動作します。<br/>「res/raw」フォルダのmp3、wavファイル名を指定すると、通知音が変更されます。<br/>**default**: null<br/>**Android Only** |

**Example**

```cpp
void Sample::RegisterPushWithOption(bool pushEnabled, bool adAgreement, bool adAgreementNight, const FString& displayLanguage, bool foregroundEnabled, bool badgeEnabled, bool soundEnabled, int32 priority, const FString& smallIconName, const FString& soundFileName)
{
    FGamebasePushConfiguration Configuration;
    Configuration.pushEnabled = pushEnabled;
    Configuration.adAgreement = adAgreement;
    Configuration.adAgreementNight = adAgreementNight;
    Configuration.requestNotificationPermission = bRequestNotificationPermission;
    Configuration.alwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;

    FGamebaseNotificationOptions NotificationOptions;
    NotificationOptions.foregroundEnabled = bForegroundEnabled;
    NotificationOptions.badgeEnabled = bBadgeEnabled;
    NotificationOptions.soundEnabled = bSoundEnabled;
    NotificationOptions.priority = Priority;
    NotificationOptions.smallIconName = SmallIconName;
    NotificationOptions.soundFileName = SoundFileName;

    IGamebase::Get().GetPush().RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            // Check the error code and handle the error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

#### Get NotificationOptions

プッシュを登録する時、既に設定している通知オプション値を取得します。

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FGamebaseNotificationOptionsPtr GetNotificationOptions();
```

**Example**

```cpp
void Sample::GetNotificationOptions()
{
    auto NotificationOptions = IGamebase::Get().GetPush().GetNotificationOptions();
    if (result.IsValid())
    {
        NotificationOptions->foregroundEnabled = true;
        NotificationOptions->smallIconName = TEXT("notification_icon_name");
        
        FGamebasePushConfiguration Configuration;
        
        IGamebase::Get().GetPush().RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) { }));
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("No GetNotificationOptions"));
    }
}
```


### Request Push Settings

ユーザーのプッシュ設定を照会するには、次のAPIを利用します。
コールバックされるPushConfiguration値でユーザー設定値を取得できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void QueryTokenInfo(const FGamebasePushTokenInfoDelegate& onCallback);

// Legacy API
void QueryPush(const FGamebasePushConfigurationDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryTokenInfo()
{
    IGamebase::Get().GetPush().QueryTokenInfo(FGamebasePushTokenInfoDelegate::CreateLambda([=](const FGamebasePushTokenInfo* tokenInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, tokenInfo= %s)"),
                tokenInfo->pushEnabled ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreement ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreementNight ? TEXT("true") : TEXT("false"),
                *tokenInfo->token);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo failed. (error: %d)"), error->code);
        }
    }));
}
```


#### FGamebasePushTokenInfo

| Parameter           | Values                 | Description         |
| --------------------| -----------------------| ------------------- |
| pushType            | FString                | Pushトークンタイプ    |
| token               | FString                | トークン              |
| userId              | FString                | ユーザーID         |
| deviceCountryCode   | FString                | 国コード        |
| timezone            | FString                | 標準時間帯        |
| registeredDateTime  | FString                | トークンアップデート時間 |
| languageCode        | FString                | 言語設定         |
| agreement           | FGamebasePushAgreement | 受信同意有無       |
| sandbox             | bool                   | sandboxかどうか(iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 通知表示同意有無          |
| adAgreement      | bool | 広告性通知表示同意有無     |
| adAgreementNight | bool | 夜間広告性通知表示同意有無 |


### Event Handling

* プッシュメッセージが到着したときやプッシュメッセージをクリックしたときに、イベント処理を行うことができます。
* イベントの登録方法はGamebaseEventHandlerガイドを参照してください。
    * [ Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./unreal-etc/#push-received-message)
    * [ Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./unreal-etc/#push-click-message)
    * [ Game > Gamebase > Unreal SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./unreal-etc/#push-click-action)


#### Setting for APNS Sandbox

* SandboxModeを有効にすると、APNS SandboxへPushを送信するように登録できます。
* コンソールから送信する方法
    * Pushメニューの**対象**から**iOS Sandbox**を選択した後、送信します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void SetSandboxMode(bool isSandbox);
```

**Example**

```cpp
void Sample::SetSandboxMode(bool isSandbox)
{
    IGamebase::Get().GetPush().SetSandboxMode(isSandbox);
}
```


### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 以前のプッシュAPIの呼び出しが完了しませんでした。<br>以前のプッシュAPIのコールバックが実行された後に再度呼び出してください。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないプッシュエラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud Pushライブラリでエラーが発生した時に返されます。
* NHN Cloud Pushライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* NHN Cloud Pushエラーコードを確認してください。
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
