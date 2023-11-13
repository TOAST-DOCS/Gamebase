## Game > Gamebase > Unity SDK使用ガイド > Logger

ここではUnityでNHN Cloud Logger SDKを使用する方法を説明します。

### Initialize
Log & Crash Searchで発行されたAppKeyでNHN Cloud Logger SDKを初期化します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void Initialize(GamebaseRequest.Logger.Configuration configuration)
```

**Example**
```cs
public static void InitializeSample()
{
    var configuration = new GamebaseRequest.Logger.Configuration("USER_LOGGER_APP_KEY");
    // configuration.enableCrashReporter = true;    // Whether to send crash logs.
    // configuration.enableCrashErrorLog =  true;   // Errro log is excluded by default. Use it if you want to collect error logs.
    Gamebase.Logger.Initialize(configuration);
}
```

### Send Logs
Log & Crash Serverへログを送信します。
NHN Cloud Logger SDKは、次の5つのレベルのログを送信できます。 
* DEBUG
* INFO
* WARN
* ERROR
* FATAL

ログレベルは次の通りです。
* DEBUG > INFO > WARN > ERROR > FATAL

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void Debug(string message, Dictionary<string, string> userFields = null)
static void Info(string message, Dictionary<string, string> userFields = null)
static void Warn(string message, Dictionary<string, string> userFields = null)
static void Error(string message, Dictionary<string, string> userFields = null)
static void Fatal(string message, Dictionary<string, string> userFields = null)
```

**Example**
```cs
public void DebugSample()
{
    Dictionary<string, string> userFields = new Dictionary&lt;string, string>()
    {
        { "KEY_1", "VALUE_1" },
        { "KEY_2", "VALUE_2" },
    };
        
    Gamebase.Logger.Debug(
        "MESSAGE", 
        userFields);
}

public void InfoSample()
{
    Dictionary<string, string> userFields = new Dictionary&lt;string, string>()
    {
        { "KEY_1", "VALUE_1" },
        { "KEY_2", "VALUE_2" },
    };
        
    Gamebase.Logger.Info(
        "MESSAGE", 
        userFields);
}

public void WarnSample()
{
    Dictionary<string, string> userFields = new Dictionary&lt;string, string>()
    {
        { "KEY_1", "VALUE_1" },
        { "KEY_2", "VALUE_2" },
    };
        
    Gamebase.Logger.Warn(
        "MESSAGE", 
        userFields);
}

public void ErrorSample()
{
    Dictionary<string, string> userFields = new Dictionary&lt;string, string>()
    {
        { "KEY_1", "VALUE_1" },
        { "KEY_2", "VALUE_2" },
    };
        
    Gamebase.Logger.Error(
        "MESSAGE", 
        userFields);
}

public void FatalSample()
{
    Dictionary<string, string> userFields = new Dictionary&lt;string, string>()
    {
        { "KEY_1", "VALUE_1" },
        { "KEY_2", "VALUE_2" },
    };
        
    Gamebase.Logger.Fatal(
        "MESSAGE", 
        userFields);
}
```

### Set User-Defined Fields
任意のユーザー定義フィールドを設定します。 
ユーザー定義フィールドを設定すると、ログ転送APIを呼び出すたびに、設定した値をログと一緒にサーバーに送信します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void SetUserField(string key, string value)
```

**Example**
```cs
public void SetUserFieldSample()
{
    Gamebase.Logger.SetUserField("KEY", "VALUE");
}
```

### Further Tasks after Sending Logs
リスナーを登録すると、ログ送信後に追加作業を実行できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void SetLoggerListener(GamebaseCallback.Logger.ILoggerListener listener)
```

**Example**
```cs
public class LoggerListener : GamebaseCallback.Logger.ILoggerListener
{
    public void OnError(GamebaseResponse.Logger.LogEntry log, string errorMessage)
    {
        // Sending logs failed
    }

    public void OnFilter(GamebaseResponse.Logger.LogEntry log, GamebaseResponse.Logger.LogFilter filter)
    {
        // The logs were filtered out and not sent.(Refer to AddClashFilter API Guide)
    }

    public void OnSave(GamebaseResponse.Logger.LogEntry log)
    {
        // If log transmission fails due to network disconnection, the log is saved in a file for log retransmission.(The saved file cannot be checked.)
    }

    public void OnSuccess(GamebaseResponse.Logger.LogEntry log)
    {
        // Sending logs succeeded
    }
}                    

public void SetLoggerListenerSample()
{
    LoggerListener listener = new LoggerListener();
    Gamebase.Logger.SetLoggerListener(listener);
}
```

### Specifications for SetCrashListener API
Unityを利用していると、収集を望まない例外ログやクラッシュログが収集されることがあります。
NHN Cloud Logger SDKは、収集を望まないクラッシュログをフィルタリングする機能をサポートします。
crashFilterのreturn値がtrueの場合、ログはフィルタリングされます。

> <font color="red">[注意]</font><br/>
>
> この機能はUnityの例外に限定した機能です。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void AddCrashFilter(GamebaseCallback.Logger.CrashFilter filter)
static void RemoveCrashFilter(GamebaseCallback.Logger.CrashFilter filter)
```

**Example**
```cs
public GamebaseCallback.Logger.CrashFilter crashFilter = (crashLogData) =>
{
    // If the return value is true, no log is sent.
    // The properties of CrashLogData are the same as the parameters of Application.LogCallback on Unity.
    // https://docs.unity3d.com/ScriptReference/Application.LogCallback.html
    return crashLogData.Condition.Contains("UnityEngine.Debug.Log");
};

public void AddCrashFilterSample()
{
    Gamebase.Logger.AddCrashFilter(crashFilter);
}

public void RemoveCrashFilterSample()
{
    Gamebase.Logger.RemoveCrashFilter(crashFilter);
}
```

### Send Handled Exceptions

一般/クラッシュログだけでなく、try/catch構文の例外に関する内容をReport APIを使用して送信できます。
このように送信した例外ログは「Log & Crash Searchコンソール」>「App Crash Searchタブ」のエラータイプで「Handled」でフィルタリングして照会できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_WEBGL

```cs
static void Report(GamebaseLoggerConst.LogLevel logLevel, string message, string logString, string stackTrace)
```

**Example**
```cs
public void ReportSample(Exception e)
{
    Gamebase.Logger.Report(GamebaseLoggerConst.LogLevel.ERROR, "message", e.Message, e.StackTrace);
}
```
