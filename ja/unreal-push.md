## Game > Gamebase > Unreal SDK使用ガイド > プッシュ

ここでは各プラットフォームでプッシュ通知を使用するのに必要な設定方法を説明します。

### Settings

AndroidやiOSでプッシュを設定する方法は、次の文書を参照してください。

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/₩1#firebase-notification)
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
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void RegisterPush(const FGamebasePushConfiguration& Configuration, const FGamebaseErrorDelegate& Callback);
void RegisterPush(const FGamebasePushConfiguration& Configuration, const FGamebaseNotificationOptions& notificationOptions, const FGamebaseErrorDelegate& Callback);
```

#### FGamebasePushConfiguration
| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| bPushEnabled                   | M             | bool         | プッシュ同意の有無 |
| bAdAgreement                   | M             | bool         | 広告性プッシュ同意の有無 |
| bAdAgreementNight | M          | bool         | 夜間広告性プッシュ同意の有無 |
| bRequestNotificationPermission | O             | bool         | Android 13以上のOSでRegisterPush APIを呼び出した場合、Push権限リクエストポップアップを自動出力するかどうか<br>**default**: true<br/>**Androidに限る** |
| bAlwaysAllowTokenRegistration  | O             | bool         | ユーザーがプッシュ権限を拒否してもトークンを登録するかどうか<br>trueに設定した場合、プッシュ権限を取得できなくてもトークンを登録します。<br>**default**: false<br/>**iOSに限る** |

**Example**

```cpp
void USample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration Configuration;
    Configuration.bPushEnabled = pushEnabled;
    Configuration.bAdAgreement = adAgreement;
    Configuration.bAdAgreementNight = adAgreementNight;
    Configuration.bRequestNotificationPermission = bRequestNotificationPermission;
    Configuration.bAlwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->RegisterPush(Configuration, FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

### Notification Options

* 端末に表示する通知をどのような形式で表示するかをNotification Optionsで変更できます。
* Notification Optionsは、AndroidManifest.xmlに設定したり、ランタイムにregisterPush APIを呼び出して変更できます。

#### Set Notification Options with RegisterPush in Runtime

RegisterPush APIを呼び出す時、FGamebaseNotificationOptionsの引数を追加して通知オプションを設定できます。
FGamebaseNotificationOptionsの作成者にGamebaseSubsystem->GetPush()->GetNotificationOptions()呼び出し結果を渡すと、現在の通知オプションで初期化されたオブジェクトが作成されるため、必要な値のみ変更できます。<br/>
設定可能な値は次のとおりです。

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| bForegroundEnabled      | bool         | アプリがフォアグラウンド状態の時の通知表示有無<br/>**default**: false |
| bBadgeEnabled           | bool         | バッジアイコン使用有無<br/>**default**: true |
| bSoundEnabled           | bool         | 通知音使用有無<br/>**default**: true<br/>**iOS Only** |
| Priority               | int32        | 通知の優先順位。以下の5つの値を設定できます。<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| SmallIconName          | FString       | 通知用の小さなアイコンファイル名。<br/>設定しない場合、アプリアイコンが使用されます。<br/>**default**: null<br/>**Android Only** |
| SoundFileName          | FString       | 通知音ファイル名。Android 8.0未満のOSでのみ動作します。<br/>「res/raw」フォルダのmp3、wavファイル名を指定すると、通知音が変更されます。<br/>**default**: null<br/>**Android Only** |

**Example**

```cpp
void USample::RegisterPushWithOption(bool pushEnabled, bool adAgreement, bool adAgreementNight, const FString& displayLanguage, bool foregroundEnabled, bool badgeEnabled, bool soundEnabled, int32 priority, const FString& smallIconName, const FString& soundFileName)
{
    FGamebasePushConfiguration Configuration;
    Configuration.bPushEnabled = pushEnabled;
    Configuration.bAdAgreement = adAgreement;
    Configuration.bAdAgreementNight = adAgreementNight;
    Configuration.bRequestNotificationPermission = bRequestNotificationPermission;
    Configuration.bAlwaysAllowTokenRegistration = bAlwaysAllowTokenRegistration;
    
    FGamebaseNotificationOptions NotificationOptions;
    NotificationOptions.bForegroundEnabled = bForegroundEnabled;
    NotificationOptions.bBadgeEnabled = bBadgeEnabled;
    NotificationOptions.bSoundEnabled = bSoundEnabled;
    NotificationOptions.Priority = Priority;
    NotificationOptions.SmallIconName = SmallIconName;
    NotificationOptions.SoundFileName = SoundFileName;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            // Check the Error code and handle the Error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

#### Get NotificationOptions

プッシュを登録する時、既に設定している通知オプション値を取得します。

**API**

Supported Platforms

<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
FGamebaseNotificationOptionsPtr GetNotificationOptions();
```

**Example**

```cpp
void USample::GetNotificationOptions()
{
    auto NotificationOptions = UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->GetNotificationOptions();
    if (result.IsValid())
    {
        NotificationOptions->ForegroundEnabled = true;
        NotificationOptions->SmallIconName = TEXT("notification_icon_name");
        
        FGamebasePushConfiguration Configuration;
        
        UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
        Subsystem->GetPush()->RegisterPush(Configuration, NotificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error) { }));
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
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void QueryTokenInfo(const FGamebasePushTokenInfoDelegate& Callback);

// Legacy API
void QueryPush(const FGamebasePushConfigurationDelegate& Callback);
```

**Example**

```cpp
void USample::QueryTokenInfo()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->QueryTokenInfo(FGamebasePushTokenInfoDelegate::CreateLambda([=](const FGamebasePushTokenInfo* TokenInfo, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, TokenInfo= %s)"),
                TokenInfo->PushEnabled ? TEXT("true") : TEXT("false"),
                TokenInfo->bAdAgreement ? TEXT("true") : TEXT("false"),
                TokenInfo->bAdAgreementNight ? TEXT("true") : TEXT("false"),
                *TokenInfo->Token);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo failed. (Error: %d)"), Error->Code);
        }
    }));
}
```


#### FGamebasePushTokenInfo

| Parameter           | Values                 | Description         |
| --------------------| -----------------------| ------------------- |
| PushType            | FString                | Pushトークンタイプ    |
| Token               | FString                | トークン              |
| UserId              | FString                | ユーザーID         |
| DeviceCountryCode   | FString                | 国コード        |
| Timezone            | FString                | 標準時間帯        |
| RegisteredDateTime  | FString                | トークンアップデート時間 |
| LanguageCode        | FString                | 言語設定         |
| Agreement           | FGamebasePushAgreement | 受信同意有無       |
| bSandbox             | bool                   | sandboxかどうか(iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| bPushEnabled      | bool | 通知表示同意有無          |
| bAdAgreement      | bool | 広告性通知表示同意有無     |
| bAdAgreementNight | bool | 夜間広告性通知表示同意有無 |


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
void USample::SetSandboxMode(bool isSandbox)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPush()->SetSandboxMode(isSandbox);
}
```


### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 以前のプッシュAPIの呼び出しが完了しませんでした。<br>以前のプッシュAPIのコールバックが実行された後に再度呼び出してください。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないプッシュエラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./Error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud Pushライブラリでエラーが発生した時に返されます。
* NHN Cloud Pushライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Messsage);

    GamebaseInnerError* moduleError = gamebaseError.Error; // GamebaseError.Error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleerror->Code, *moduleerror->Messsage);
    }
}
```

* NHN Cloud Pushエラーコードを確認してください。
    * [Android](aos-push#Error-handling)
    * [iOS](ios-push#Error-handling)
