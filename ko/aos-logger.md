## Game > Gamebase > Android SDK 사용 가이드 > Logger

여기에서는 Log & Crash 전송 API를 사용하는 방법을 알아보겠습니다.

### Initialize

Log & Crash Search에서 발급 받은 앱키(Appkey)로  NHN Cloud Logger SDK를 초기화합니다.<br/>
앱이 실행되자마자 발생하는 크래시 로그도 빠짐없이 전송하려면 **Application.onCreate()**에서 NHN Cloud Logger를 초기화해야 합니다.

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

Log & Crash 서버로 로그를 전송합니다
NHN Cloud Logger SDK는 아래 5가지 레벨의 로그를 전송할 수 있습니다.

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

로그 레벨은 다음과 같습니다.

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
원하는 사용자 정의 필드를 설정합니다.

사용자 정의 필드를 설정하면 로그 전송 API를 호출할 때마다 설정한 값을 로그와 함께 서버로 전송합니다.

**API**

```java
+ (void)setUserField(String field, Object value);
```

**Example**

```java
Gamebase.Logger.setUserField("KEY", "VALUE");
```

### Further Tasks after Sending Logs

리스너(listener)를 등록하면 로그 전송 후 추가 작업을 진행할 수 있습니다.

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

### Use Handled Exception API

Android 플랫폼에서는 try/catch 구문에서 예외와 관련된 내용을 NHN Cloud Logger의 Handled Exception API를 사용하여 전송할 수 있습니다.
이렇게 전송한 예외 로그는 콘솔에서 **Log & Crash Search > 앱 크래시 검색**을 클릭하고 **오류 유형**에서 **Handled**를 클릭해 조회할 수 있습니다.
자세한 Log & Crash Search 콘솔 사용 방법은 [콘솔 사용 가이드](https://docs.nhncloud.com/ko/Data%20&%20Analytics/Log%20&%20Crash%20Search/ko/console-guide/)를 참고하세요.

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