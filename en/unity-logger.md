## Game > Gamebase > Unity Developer's Guide > Logger

Here, we will explain how to use NHN Cloud Logger SDK in Unity.

### Initialize
Initializes NHN Cloud Logger SDK using the AppKey issued via Log & Crash Search.

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
Sends logs to Log & Crash Server.
NHN Cloud Logger SDK can send five different levels of logs listed below: 
* DEBUG
* INFO
* WARN
* ERROR
* FATAL

The log levels are as follows:
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
Sets user-defined fields you want. 
If a custom field is set, the setting value are sent along with a log to the server whenever Send Log API is called.

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
Additional tasks are available after logs are sent if a listener is registered.

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
When using Unity, sometimes unwanted exception or crash logs are collected.
NHN Cloud Logger SDK supports a feature that filters out crash logs that you do not want to collect.
If crashFilter returns true, the log is filtered out.

> <font color="red">[Caution]</font><br/>
>
> This feature is only to deal with Unity exceptions

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

In addition to general/crash logs, you can send exception-related content in the try/catch syntax using Report API.
The exception logs you sent can be viewed in the console by clicking **Log & Crash Search > App Crash Search** and clicking **Handled** under **Error Type**.

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