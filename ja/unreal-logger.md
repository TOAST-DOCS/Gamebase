<!-- pre-align:aligned sig=02b9b48cba43 -->

<a id="game-gamebase-user-guide-for-unreal-sdk-logger"></a>
## Game > Gamebase > Unreal SDK使用ガイド > Logger { #game-gamebase-user-guide-for-unreal-sdk-logger }

ここではLog & Crash Search転送APIを使用する方法を説明します。

<a id="settings"></a>
### Settings { #settings }

* Windows
    * Log & Crash SearchでProjectVersionの値を設定するには、DefaultGame.iniでGeneralProjectSettingsのProjectVersionに合ったバージョンを入力する必要があります。

            [/Script/EngineSettings.GeneralProjectSettings]
            ProjectVersion=1.0.0
            
            
<a id="initialize"></a>
### Initialize { #initialize }
Log & Crash Searchで発行されたAppKeyで、NHN Cloud Logger SDKを初期化します。

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
ユーザー定義フィールドを設定します。 
ユーザー定義フィールドを設定すると、ログ転送APIを呼び出すたびに設定した値とログをサーバーへ転送します。

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
    * Log & Crash SearchでSDKで発生したクラッシュを解析するには、シンボルファイルをコンソールにアップロードする必要があります。
    * ビルド時、プロジェクトのバイナリ作成パスに .symファイルと該当ファイルを圧縮した .zipファイル作成されます。
    * [Log & Crash Searchコンソールガイド](/Data%20&%20Analytics/Log%20&%20Crash%20Search/ja/console-guide/#_21)を確認してアップロードします。
