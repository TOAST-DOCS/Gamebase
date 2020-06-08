## Game > Gamebase > Unreal SDK 사용 가이드 > Logger

여기에서는 Unreal에서 TOAST Logger SDK를 사용하는 방법을 알아 보겠습니다.

### Initialize
Log & Crash Search에서 발급받은 AppKey로  TOAST Logger SDK를 초기화 합니다

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
Log & Crash Server로 로그를 전송합니다
TOAST Logger SDK는 아래 다섯 가지 레벨의 로그를 전송할 수 있습니다.

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

로그 레벨은 다음과 같습니다.

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
원하는 사용자 정의 필드를 설정합니다. 
사용자 정의 필드를 설정하면 로그 전송 API를 호출할 때마다 설정한 값을 로그와 함께 서버로 전송합니다.

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