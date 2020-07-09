## Game > Gamebase > Unreal SDK使用ガイド > プッシュ

ここでは各プラットフォームでプッシュ通知を使用するのに必要な設定方法を説明します。

### Settings

AndroidやiOSでプッシュを設定する方法は、次の文書を参照してください。<br/>

* [Android Push Settings](aos-push#settings)
    * Firebaseプッシュを使用する時は、下記のガイドを参照してください。
        * 該当ガイドでUnityビルドの場合はUnrealの場合も同じように設定する必要があります。
            * Androidビルド時、res/values/google-services-json.xmlファイルが含まれている必要があるため、ガイドを参照して作成します。
            * Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xmlパスにファイルがあります。
* [iOS Push Settings](ios-push#settings)

### Register Push

次のAPIを呼び出して、TOAST Pushに該当ユーザーを登録します。
プッシュ同意(enablePush)、広告性プッシュ同意(enableAdPush)、夜間広告性プッシュ同意(enableAdNightPush)値をユーザーから受け取り、次のAPIを呼び出して登録を完了します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight };
    
    IGamebase::Get().GetPush().RegisterPush(FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

### Request Push Settings

ユーザーのプッシュ設定を照会するには、次のAPIを利用します。
コールバックされるPushConfiguration値でユーザー設定値を取得できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void QueryPush(const FGamebasePushConfigurationDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryPush()
{
    IGamebase::Get().GetPush().QueryPush(
        FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebasePushConfiguration* pushAdvertisements, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryPush succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, displayLanguageCode= %s)"),
                pushAdvertisements->pushEnabled ? TEXT("true") : TEXT("fasle"),
                pushAdvertisements->adAgreement ? TEXT("true") : TEXT("fasle"),
                pushAdvertisements->adAgreementNight ? TEXT("true") : TEXT("fasle"),
                *pushAdvertisements->displayLanguageCode.ToString());
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryPush failed. (error: %d)"), error->code);
        }
    }));
}
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Pushライブラリエラーです。<br>DetailCodeを確認してください。 |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 以前のプッシュAPIの呼び出しが完了しませんでした。<br>以前のプッシュAPIのコールバックが実行された後に再度呼び出してください。 |
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないプッシュエラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へご送付ください。迅速に対応いたします。

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、TOAST Pushライブラリで発生したエラーです。
* エラーコードを確認する方法は次のとおりです。

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

* TOAST Pushエラーコードを確認してください。
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
