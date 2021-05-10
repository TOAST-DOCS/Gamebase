## Game > Gamebase > Unreal SDK使用指南 > Logger

下面将了解在Unreal中使用TOAST Logger SDK的方法。

### Initialize
利用从Log & Crash Search获得的AppKey对TOAST Logger SDK进行初始化。

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
将日志传输到Log & Crash服务器。
TOAST Logger SDK可传输如下五种级别的日志。 

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

日志的级别如下。

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
设置所需的自定义字段。  
如果设置自定义字段，每当调用日志传送API时，则将设置的值和日志传送到服务器。

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