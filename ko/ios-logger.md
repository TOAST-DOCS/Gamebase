## Game > Gamebase > iOS SDK 사용 가이드 > Logger

여기에서는 Log & Crash 전송 API를 사용하는 방법을 알아보겠습니다.

### Initialize
Log & Crash Search에서 발급 받은 앱키로  NHN Cloud Logger SDK를 초기화합니다.

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
원하는 사용자 정의 필드를 설정합니다. 

사용자 정의 필드를 설정하면 로그 전송 API를 호출할 때마다 설정한 값을 로그와 함께 서버로 전송합니다.

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

델리게이트(delegate)를 등록하면 로그 전송 후 추가 작업을 진행할 수 있습니다.

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
