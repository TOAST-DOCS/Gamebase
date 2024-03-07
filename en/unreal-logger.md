## Game > Gamebase > User Guide for Unreal SDK > Logger

This document describes how to Log & Crach Search API.

### Initialize
Initialize NHN Cloud Logger SDK with appkey issued from Log & Crash Search. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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
Send logs to Log & Crash Server. 
NHN Cloud Logger SDK can send logs of the five levels as below: 

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

Logs are levelled as follows:

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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
Set user-defined field as needed. 
With user-defined field setting, set value is sent to server along with logs every time Send Logs API is called. 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

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
