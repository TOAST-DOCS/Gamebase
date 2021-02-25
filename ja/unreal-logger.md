## Game > Gamebase > Unreal SDK使用ガイド > Logger

ここではUnrealからNHN Cloud Logger SDKを使用する方法を説明します。

### Initialize
Log & Crash Searchで発行されたAppKeyで、NHN Cloud Logger SDKを初期化します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Initialize(const FGamebaseLoggerConfiguration& loggerConfiguration);
```

**Example**
```cpp
void Sample::InitializeLogger()
{
    FGamebaseLoggerConfiguration configuration{ "USER_LOGGER_APP_KEY", enableCrashReporter };
    IGamebase::Get().GetLogger().Initialize(configuration);
}
```

### Send Logs
Log & Crash Serverへログを転送します。
NHN Cloud Logger SDKは、下記5つのレベルのログを転送できます。 
* DEBUG
* INFO
* WARN
* ERROR
* FATAL

ログレベルは次のとおりです。

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void Debug(const FString& message, const TMap<FString, FString>& userFields = TMap<FString, FString>());
void Info(const FString& message, const TMap<FString, FString>& userFields = TMap<FString, FString>());
void Warn(const FString& message, const TMap<FString, FString>& userFields = TMap<FString, FString>());
void Error(const FString& message, const TMap<FString, FString>& userFields = TMap<FString, FString>());
void Fatal(const FString& message, const TMap<FString, FString>& userFields = TMap<FString, FString>());
```

**Example**
```cpp
void Sample::DebugLogger()
{
    TMap<FString, FString> userFields;
    userFields.Add("KEY_1", "VALUE_1");
    userFields.Add("KEY_2", "VALUE_2");

    IGamebase::Get().GetLogger().Debug("MESSAGE", userFields);
}

void Sample::InfoLogger()
{
    TMap<FString, FString> userFields;
    userFields.Add("KEY_1", "VALUE_1");
    userFields.Add("KEY_2", "VALUE_2");

    IGamebase::Get().GetLogger().Info("MESSAGE", userFields);
}

void Sample::WarnLogger()
{
    TMap<FString, FString> userFields;
    userFields.Add("KEY_1", "VALUE_1");
    userFields.Add("KEY_2", "VALUE_2");

    IGamebase::Get().GetLogger().Warn("MESSAGE", userFields);
}

void Sample::ErrorLogger()
{
    TMap<FString, FString> userFields;
    userFields.Add("KEY_1", "VALUE_1");
    userFields.Add("KEY_2", "VALUE_2");

    IGamebase::Get().GetLogger().Error("MESSAGE", userFields);
}

void Sample::FatalLogger()
{
    TMap<FString, FString> userFields;
    userFields.Add("KEY_1", "VALUE_1");
    userFields.Add("KEY_2", "VALUE_2");

    IGamebase::Get().GetLogger().Fatal("MESSAGE", userFields);
}
```

### Set User-Defined Fields
ユーザー定義フィールドを設定します。 
ユーザー定義フィールドを設定すると、ログ転送APIを呼び出すたびに設定した値とログをサーバーへ転送します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void SetUserField(const FString& key, const FString& value);
```

**Example**
```cpp
void Sample::SetLoggerUserField()
{
    IGamebase::Get().GetLogger().SetUserField("KEY", "VALUE");
}
```
