<!-- pre-align:aligned sig=02b9b48cba43 -->

<a id="game-gamebase-user-guide-for-unreal-sdk-logger"></a>
## Game > Gamebase > Unreal SDK 사용 가이드 > Logger { #game-gamebase-user-guide-for-unreal-sdk-logger }

여기에서는 Log & Crash Search 전송 API를 사용하는 방법을 알아보겠습니다.

<a id="settings"></a>
### Settings { #settings }

* Windows
    * Log & Crash Search에서 ProjectVersion의 값을 설정하려면 DefaultGame.ini에서 GeneralProjectSettings의 ProjectVersion에 알맞은 버전을 입력해야 합니다.
            
            [/Script/EngineSettings.GeneralProjectSettings]
            ProjectVersion=1.0.0
    

<a id="initialize"></a>
### Initialize { #initialize }
Log & Crash Search에서 발급 받은 앱키로 NHN Cloud Logger SDK를 초기화합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Initialize(const FGamebaseLoggerConfiguration& loggerConfiguration);
```

**Example**
```cpp
void USample::InitializeLogger()
{
    FGamebaseLoggerConfiguration Configuration;
    Configuration.AppKey = TEXT("USER_LOGGER_APP_KEY");
    Configuration.bEnableCrashReporter = true;
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Initialize(Configuration);
}
```

<a id="send-logs"></a>
### Send Logs { #send-logs }
Log & Crash Server로 로그를 전송합니다.
NHN Cloud  Logger SDK는 아래 다섯 가지 레벨의 로그를 전송할 수 있습니다.

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

로그 레벨은 다음과 같습니다.

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void Debug(const FString& Message, const TMap<FString, FString>& UserFields = TMap<FString, FString>());
void Info(const FString& Message, const TMap<FString, FString>& UserFields = TMap<FString, FString>());
void Warn(const FString& Message, const TMap<FString, FString>& UserFields = TMap<FString, FString>());
void Error(const FString& Message, const TMap<FString, FString>& UserFields = TMap<FString, FString>());
void Fatal(const FString& Message, const TMap<FString, FString>& UserFields = TMap<FString, FString>());
```

**Example**

```cpp
void USample::DebugLogger()
{
    TMap<FString, FString> UserFields;
    UserFields.Add("KEY_1", "VALUE_1");
    UserFields.Add("KEY_2", "VALUE_2");

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Debug("MESSAGE", UserFields);
}

void USample::InfoLogger()
{
    TMap<FString, FString> UserFields;
    UserFields.Add("KEY_1", "VALUE_1");
    UserFields.Add("KEY_2", "VALUE_2");

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Info("MESSAGE", UserFields);
}

void USample::WarnLogger()
{
    TMap<FString, FString> UserFields;
    UserFields.Add("KEY_1", "VALUE_1");
    UserFields.Add("KEY_2", "VALUE_2");

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Warn("MESSAGE", UserFields);
}

void USample::ErrorLogger()
{
    TMap<FString, FString> UserFields;
    UserFields.Add("KEY_1", "VALUE_1");
    UserFields.Add("KEY_2", "VALUE_2");

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Error("MESSAGE", UserFields);
}

void USample::FatalLogger()
{
    TMap<FString, FString> UserFields;
    UserFields.Add("KEY_1", "VALUE_1");
    UserFields.Add("KEY_2", "VALUE_2");

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->Fatal("MESSAGE", UserFields);
}
```

<a id="set-user-defined-fields"></a>
### Set User-Defined Fields { #set-user-defined-fields }
원하는 사용자 정의 필드를 설정합니다. 
사용자 정의 필드를 설정하면 로그 전송 API를 호출할 때마다 설정한 값을 로그와 함께 서버로 전송합니다.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void SetUserField(const FString& key, const FString& value);
```

**Example**
```cpp
void USample::SetLoggerUserField()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetLogger()->SetUserField("KEY", "VALUE");
}
```

<a id="crash-reporter"></a>
### Crash Reporter { #crash-reporter }

* Windows
    * Log & Crash Search에서 SDK에서 발생한 크래시를 해석하려면 심벌 파일을 콘솔에 업로드해야 합니다.
    * 빌드 시 프로젝트의 바이너리 생성 경로에 .sym 파일과 해당 파일을 압축한 .zip 파일 생성됩니다.
    * [Log & Crash Search 콘솔 가이드](https://docs.nhncloud.com/ko/Data%20&%20Analytics/Log%20&%20Crash%20Search/ko/console-guide/#_21)를 확인하여 업로드합니다.
