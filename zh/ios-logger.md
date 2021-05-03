## Game > Gamebase > iOS SDK使用指南 > Logger

下面将了解在iOS中使用TOAST Logger SDK的方法。

### Initialize

利用从Log & Crash Search获得的AppKey对TOAST Logger SDK进行初始化。

**API**

```objectivec
+ (void)initializeWithConfiguration:(TCGBLoggerConfiguration *)configuration;
```

**Example**
```objectivec
- (void)initializeSample {
    TCGBLoggerConfiguration *configuration = [TCGBLoggerConfiguration configurationWithAppKey:@"YOUR_APP_KEY"];

// Default value of enableCrashReport is YES
// You can set NO if you don't want use a crash report feature.
//    TCGBLoggerConfiguration *configuration = [TCGBLoggerConfiguration configurationWithAppKey:@"YOUR_APP_KEY" enableCrashReporter:NO];

    [TCGBLogger initializeWithConfiguration:configuration];
}
```

### Send Logs

将日志传输到Log & Crash服务器。
TOAST Logger SDK可传输如下五种级别的日志。

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

日志级别如下所示。

* DEBUG > INFO > WARN > ERROR > FATAL

**API**

```objectivec
+ (void)debug:(NSString *)message;
+ (void)info:(NSString *)message;
+ (void)warn:(NSString *)message;
+ (void)error:(NSString *)message;
+ (void)fatal:(NSString *)message;

+ (void)debug:(NSString *)message userFields:(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)info:(NSString *)message userFields:(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)warn:(NSString *)message userFields:(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)error:(NSString *)message userFields:(NSDictionary<NSString *, NSString *> *)userFields;
+ (void)fatal:(NSString *)message userFields:(NSDictionary<NSString *, NSString *> *)userFields;

+ (void)debugWithFormat:(NSString *)format, ...;
+ (void)infoWithFormat:(NSString *)format, ...;
+ (void)warnWithFormat:(NSString *)format, ...;
+ (void)errorWithFormat:(NSString *)format, ...;
+ (void)fatalWithFormat:(NSString *)format, ...;
```

**Example**

```objectivec
- (void)debugSample
{
    // Default
    [TCGBLogger debug:@"Message"];

    // With userFields
    NSDictionary *myUserFields = @{ @"KEY" : @"VALUE" };
    [TCGBLogger debug:@"Message" userFields:myUserFields];

    // Formatted string
    [TCGBLogger debugWithFormat:@"VALUE1: %@, VALUE2: %@", @"myValue1", @"myValue2"];
}
```

### Set User-Defined Fields
设置需要的用户自定义字段。 

设置用户自定义字段后，每次调用日志传输API时，将设置的值与日志一同传送至服务器。

**API**

```objectivec
+ (void)setUserFieldWithValue:(NSString *)value forKey:(NSString *)key;
```

**Example**

```objectivec
- (void)setUserFieldSample()
{
    [TCGBLogger setUserFieldWithValue:@"VALUE" forKey:@"KEY"];
}
```

### Further Tasks after Sending Logs

若注册delegate，传输日志后，可进行补充作业。

**API**

```objectivec
+ (void)setDelegate:(id<TCGBLoggerDelegate>)delegate;
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
