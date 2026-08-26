<!-- pre-align:aligned sig=640feae42eaf -->

<a id="game-gamebase-android-sdk-user-guide-logger"></a>
## Game > Gamebase > Android SDK使用ガイド > Logger { #game-gamebase-android-sdk-user-guide-logger }

ここではLog & Crash Search転送APIを使用する方法を説明します。

<a id="initialize"></a>
### Initialize { #initialize }

Log & Crash Searchで発行したAppKeyでNHN Cloud Logger SDKを初期化します。<br/>
アプリ起動直後に発生するクラッシュログも漏れなく転送するには**Application.onCreate()**でNHN Cloud Loggerを初期化する必要があります。

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

Log & Crash Serverにログを転送します。
NHN Cloud Logger SDKは、下記の5つのレベルのログを転送できます。

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

ログレベルは次のとおりです。

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
Gamebase.Logger.debug("VALUE1： %s, VALUE2： %s", val1, val2);
```

<a id="set-user-defined-fields"></a>
### Set User-Defined Fields { #set-user-defined-fields }
ユーザー定義フィールドを設定します。

ユーザー定義フィールドを設定すると、ログ転送APIを呼び出すたびに設定した値をログと一緒にサーバーに転送します。

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

リスナー(listener)を登録すると、ログ転送後、追加作業を進行できます。

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

Androidプラットフォームでは、try/catch構文で例外に関する内容をNHN Cloud LoggerのHandled Exception APIを使用して送信できます。
このように送信した例外ログは、コンソールで**Log & Crash Search > アプリクラッシュ検索**をクリックし、**エラータイプ**の**Handled**をクリックして照会できます。
詳しいLog & Crashコンソールの使用方法は[コンソール使用ガイド](/Data%20&%20Analytics/Log%20&%20Crash%20Search/ko/console-guide/)を参照してください。

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
