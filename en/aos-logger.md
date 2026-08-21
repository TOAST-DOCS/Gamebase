<!-- pre-align:aligned sig=640feae42eaf -->

<a id="game-gamebase-android-sdk-user-guide-logger"></a>
## Game > Gamebase > Android SDK User Guide > Logger { #game-gamebase-android-sdk-user-guide-logger }

This document describes how to use Log & Crash Search API.

<a id="initialize"></a>
### Initialize { #initialize }

Initialize NHN Cloud Logger SDK with appkey issued from Log & Crash Search.<br/>
To send crash logs, without a miss, occurring immediately with app execution, NHN Cloud Logger must be initialized from **Application.onCreate()**.

**API**

```java
+ (void)initialize(Context context, LoggerConfiguration configuration);
```

**Example**

```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        // Initialize TOAST Logger
        final String LOG_AND_CRASH_APPKEY = "AaBbCcDdEeFfGgHh";
        final boolean sendCrashLog = true;
        final LoggerConfiguration.Builder configBuilder = LoggerConfiguration.newBuilder(
                LOG_AND_CRASH_APPKEY, sendCrashLog);
        Gamebase.Logger.initialize(context, configBuilder.build());
    }
}
```

<a id="send-logs"></a>
### Send Logs { #send-logs }

Send logs to Log & Crash Server.
NHN Cloud Logger SDK provides the following five level of logs to send:

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

Log levels are as follows:

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

```java
+ (void)debug(String message);
+ (void)info(String message);
+ (void)warn(String message);
+ (void)error(String message);
+ (void)fatal(String message);

+ (void)debug(String message, Map<String, String> userFields);
+ (void)info(String message, Map<String, String> userFields);
+ (void)warn(String message, Map<String, String> userFields);
+ (void)error(String message, Map<String, String> userFields);
+ (void)fatal(String message, Map<String, String> userFields);

+ (void)debug(String format, Object... args);
+ (void)info(String format, Object... args);
+ (void)warn(String format, Object... args);
+ (void)error(String format, Object... args);
+ (void)fatal(String format, Object... args);
```

**Example**

```java
// Default
Gamebase.Logger.debug("Message", userField);

// With userFields
final Map<String, String> userField = new HashMap<>();
userField.put("KEY", "VALUE");
Gamebase.Logger.debug("Message", userField);

// Formatted string
final String val1 = "myValue1";
final String val2 = "maValue2";
Gamebase.Logger.debug("VALUE1: %s, VALUE2: %s", val1, val2);
```

<a id="set-user-defined-fields"></a>
### Set User-Defined Fields { #set-user-defined-fields }
Set the user-defined fields you need.

With user-defined field setting, set values are delivered to a server along with logs, every time  send logs API is called.

**API**

```java
+ (void)setUserField(String field, Object value);
```

**Example**

```java
Gamebase.Logger.setUserField("KEY", "VALUE");
```

<a id="further-tasks-after-sending-logs"></a>
### Further Tasks after Sending Logs { #further-tasks-after-sending-logs }

With the listener registered, further tasks can be executed after log is sent.

**API**

```java
+ (void)setLoggerListener(LoggerListener listener);
```

**Example**

```java
Gamebase.Logger.setLoggerListener(new LoggerListener() {
    @Override
    public void onSuccess(@NotNull LogEntry logEntry) {
    	// Sending logs succeeded
    }

    @Override
    public void onFilter(@NotNull LogEntry logEntry, @NotNull LogFilter logFilter) {
    	// The logs were filtered out and not sent.
    }

    @Override
    public void onSave(@NotNull LogEntry logEntry) {
    	// If log transmission fails due to network disconnection, the log is saved in a file for log retransmission.(The saved file cannot be checked.)
    }

    @Override
    public void onError(@NotNull LogEntry logEntry, @NotNull GamebaseException exception) {
    	// Sending logs failed
    }
});
```

<a id="use-handled-exception-api"></a>
### Use Handled Exception API { #use-handled-exception-api }

On the Android platform, you can send exception-related content in the try/catch syntax using NHN Cloud Logger's Handled Exception API.
The exception logs you sent can be viewed in the console by clicking **Log & Crash Search > App Crash Search** and clicking **Handled** under **Error Type**.
For more information on how to use the Log & Crash Search console, [Console User Guide](https://docs.nhncloud.com/en/Data%20&%20Analytics/Log%20&%20Crash%20Search/en/console-guide/).

**API**

```java
+ (void)report(String message, Throwable throwable);
+ (void)report(String message, Throwable throwable, Map<String, String> userFields);
```

**Example**

```java
try {
    // User Codes...
} catch (Exception e) {
    final Map<String, Object> userFields = new HashMap<>();
    userField.put("KEY", "VALUE");
    Gamebase.Logger.report("message", e, userFields);
}
```
