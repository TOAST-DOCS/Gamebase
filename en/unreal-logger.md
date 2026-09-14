<!-- pre-align:aligned sig=02b9b48cba43 -->

<a id="game-gamebase-user-guide-for-unreal-sdk-logger"></a>
## Game > Gamebase > User Guide for Unreal SDK > Logger { #game-gamebase-user-guide-for-unreal-sdk-logger }

This document describes how to Log & Crach Search API.

<a id="settings"></a>
### Settings { #settings }

* Windows
    * To set the value of ProjectVersion in Log & Crash Search, you must enter the appropriate version for ProjectVersion in GeneralProjectSettings in DefaultGame.ini.

            [/Script/EngineSettings.GeneralProjectSettings]
            ProjectVersion=1.0.0
            
                
<a id="initialize"></a>
### Initialize { #initialize }
Initialize NHN Cloud Logger SDK with appkey issued from Log & Crash Search. 

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
Set user-defined field as needed. 
With user-defined field setting, set value is sent to server along with logs every time Send Logs API is called. 

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
    * To interpret crashes that occur in your SDK in Log & Crash Search, you need to upload a symbol file to the console
    * During the build process, a .sym file and a .zip file containing the compressed .sym file are generated in the project's binary output path.
    * Check out the [Log & Crash Search console guide](/Data%20&%20Analytics/Log%20&%20Crash%20Search/en/console-guide/#symbol-file) to upload.