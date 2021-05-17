## Game > Gamebase > iOS SDK User Guide > Logger

Here, we will learn how to use NHN Cloud Logger SDK in iOS.

### Initialize
Initialize NHN Cloud Logger SDK using the app key issued via Log & Crash Search.

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

Sends logs to Log & Crash Server.
NHN Cloud Logger SDK can send five different levels of logs listed below:

* DEBUG
* INFO
* WARN
* ERROR
* FATAL

Log levels are as follows:

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
Set the user-defined fields you need. 

With user-defined field setting, set values are delivered to a server along with logs, every time  send logs API is called. 

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

Additional tasks are available after logs are sent if a delegate is registered.

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
