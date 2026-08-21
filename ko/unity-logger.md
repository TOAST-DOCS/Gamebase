<!-- pre-align:aligned sig=9d40086bc5d8 -->

<a id="game-gamebase-unity-developers-guide-logger"></a>
## Game > Gamebase > Unity SDK 사용 가이드 > Logger { #game-gamebase-unity-developers-guide-logger }

여기에서는 Log & Crash Search 전송 API를 사용하는 방법을 알아보겠습니다.

<a id="initialize"></a>
### Initialize { #initialize }
Log & Crash Search에서 발급 받은 앱키로 NHN Cloud Logger SDK를 초기화합니다.

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

<a id="send-logs"></a>
### Send Logs { #send-logs }
Log & Crash Server로 로그를 전송합니다
NHN Cloud Logger SDK는 아래 다섯 가지 레벨의 로그를 전송할 수 있습니다. 
* DEBUG
* INFO
* WARN
* ERROR
* FATAL

로그 레벨은 다음과 같습니다.
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

<a id="set-user-defined-fields"></a>
### Set User-Defined Fields { #set-user-defined-fields }
원하는 사용자 정의 필드를 설정합니다. 
사용자 정의 필드를 설정하면 로그 전송 API를 호출할 때마다 설정한 값을 로그와 함께 서버로 전송합니다.

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

<a id="further-tasks-after-sending-logs"></a>
### Further Tasks after Sending Logs { #further-tasks-after-sending-logs }
리스너를 등록하면 로그 전송 후 추가 작업을 진행할 수 있습니다.

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

<a id="specifications-for-setcrashlistener-api"></a>
### Specifications for SetCrashListener API { #specifications-for-setcrashlistener-api }
유니티를 이용하다보면 수집을 원하지 않는 예외 로그 혹은 크래시 로그들이 수집될 수 있습니다.
NHN Cloud Logger SDK는 수집을 원하지 않는 크래시 로그를 필터링 하는 기능을 지원합니다.
crashFilter의 return값이 true이면 로그는 필터링 됩니다.

> <font color="red">[주의]</font><br/>
>
> 해당 기능은 유니티 예외에 한정된 기능입니다.

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

<a id="send-handled-exceptions"></a>
### Send Handled Exceptions { #send-handled-exceptions }

일반/크래시 로그뿐만 아니라, try/catch 구문에서 예외와 관련된 내용을 Report API를 사용하여 전송할 수 있습니다.
이렇게 전송한 예외 로그는 **Log & Crash Search** 콘솔 > **App Crash Search** 탭의 **오류 유형**에서 'Handled'로 필터링하여 조회할 수 있습니다.

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