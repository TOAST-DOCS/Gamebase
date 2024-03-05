## Game > Gamebase > iOS SDK使用ガイド > Logger

ここではLog & Crash Search転送APIを使用する方法を説明します。

### Initialize
Log & Crash Searchで発行されたアプリケーションキーでNHN Cloud Logger SDKを初期化します。

**API**

```objectivec
+ (void)initializeWithConfiguration：(TCGBLoggerConfiguration *)configuration;
```

**Example**
```objectivec
- (void)initializeSample {
    TCGBLoggerConfiguration *configuration = [TCGBLoggerConfiguration configurationWithAppKey:@"YOUR_APP_KEY"];

// Default value of enableCrashReport is YES
// You can set NO if you don't want use a crash report feature.
//    TCGBLoggerConfiguration *configuration = [TCGBLoggerConfiguration configurationWithAppKey:@"YOUR_APP_KEY" enableCrashReporter:NO];

    [TCGBLogger initializeWithConfiguration：configuration];
}
```

### Send Logs

Log & Crashサーバーにログを送信します。
NHN Cloud Logger SDKは、次の5つのレベルのログを送信できます。

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

ログレベルは次のとおりです。

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

```objectivec
+ (void)debug：(NSString *)message;
+ (void)info：(NSString *)message;
+ (void)warn：(NSString *)message;
+ (void)error：(NSString *)message;
+ (void)fatal：(NSString *)message;

+ (void)debug：(NSString *)message userFields：(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)info：(NSString *)message userFields：(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)warn：(NSString *)message userFields：(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)error：(NSString *)message userFields：(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)fatal：(NSString *)message userFields：(NSDictionary<NSString *, NSString *> *)userFields;

+ (void)debugWithFormat：(NSString *)format, ...;
+ (void)infoWithFormat：(NSString *)format, ...;
+ (void)warnWithFormat：(NSString *)format, ...;
+ (void)errorWithFormat：(NSString *)format, ...;
+ (void)fatalWithFormat：(NSString *)format, ...;
```

**Example**

```objectivec
- (void)debugSample
{
    // Default
    [TCGBLogger debug：@"Message"];

    // With userFields
    NSDictionary *myUserFields = @{ @"KEY"：@"VALUE" };
    [TCGBLogger debug：@"Message" userFields：myUserFields];

    // Formatted string
    [TCGBLogger debugWithFormat：@"VALUE1：%@, VALUE2： %@", @"myValue1", @"myValue2"];
}
```

### Set User-Defined Fields
ユーザー定義フィールドを設定します。

ユーザー定義フィールドを設定すると、ログ転送APIを呼び出すたびに設定した値とログをサーバーに転送します。

**API**

```objectivec
+ (void)setUserFieldWithValue：(NSString *)value forKey：(NSString *)key;
```

**Example**

```objectivec
- (void)setUserFieldSample()
{
    [TCGBLogger setUserFieldWithValue：@"VALUE" forKey：@"KEY"];
}
```

### Further Tasks after Sending Logs

デリゲート(delegate)を登録すると、ログを送信した後に追加作業を進行できます。

**API**

```objectivec
+ (void)setDelegate：(id<TCGBLoggerDelegate>)delegate;
```

**Example**

```objectivec
@interface LoggerSample : TCGBLoggerDelegate

@end


@implementation LoggerSample

- (void)setLoggerDelegate {
    [TCGBLogger setDelegate:self];
}

- (void)tcgbLogDidSuccess:(TCGBLog *)log {
    // Sending logs succeeded
}

- (void)tcgbLogDidFail:(TCGBLog *)log error:(TCGBError *)error {
    // Sending logs failed
}

- (void)tcgbLogDidSave:(TCGBLog *)log {
    // If log transmission fails due to network disconnection, the log is saved in a file for log retransmission.(The saved file cannot be checked.)
}

- (void)tcgbLogDidFilter:(TCGBLog *)log logFilter:(TCGBLogFilter *)logFilter {
    // The logs were filtered out and not sent.
}

@end
```
