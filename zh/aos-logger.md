## Game > Gamebase > Android SDK使用指南 > Logger

下面将了解在Android中使用TOAST Logger SDK的方法。

### Initialize

利用从Log & Crash Search获得的AppKey对TOAST Logger SDK进行初始化。<br/>
为了毫无遗漏地传输运行应用程序时发生的崩溃日志，需要在**Application.onCreate()**中对TOAST Logger进行初始化。

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

### Send Logs

通过Log & Crash Server传输日志
TOAST Logger SDK可传输如下五种级别的日志。

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

日志级别如下所示。

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

### Set User-Defined Fields
设置需要的用户自定义字段。

设置用户自定义字段后，每次调用日志传输API时，将设置的值与日志一同传送至服务器。

**API**

```java
+ (void)setUserField(String field, Object value);
```

**Example**

```java
Gamebase.Logger.setUserField("KEY", "VALUE");
```

### Further Tasks after Sending Logs

若注册listener，传输日志后，可进行补充作业。

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
