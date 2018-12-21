## Game > Gamebase > iOS SDK 使用指南 > ETC

## Additional Features
以下描述Gamebase支持的附加功能。


### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language
* 可以将Gamebase显示的语言更改为设备上设置的语言以外的语言。
*  Gamebase显示客户端中包含的信息或从服务器接收的信息。
* 如果设置DisplayLanguage，将以用户设置的语言代码（ISO-639）的语言显示信息。
* 可以添加所需的语言。 以下是可以添加的语言代码：

> [参考]
>
> Gamebase中的客户端信息仅包括英语（en）和韩语（ko）。


#### Gamebase支持的语言代码种类。
| Code | Name |
| --- | --- |
| de | German |
| en |English  |
| es | Spanish |
| fi | Finish |
| fr | French |
| id | Indonesian |
| it | Italian |
| ja | Japanese |
| ko | Korean |
| pt | Portuguese |
| ru | Russian |
| th | Thai |
| vi | Vietnamese |
| ms | Malay | 
| zh-CN | Chinese-Simplified |
| zh-TW | Chinese-Traditional |

相应的语言代码在“TCGBConstants.h”类中定义。

> `[注意]`
>
> Gamebase支持的语言代码区分大小写。
> 将其设置为“EN”或“zh-cn”可能会出现问题。

```objectivec
#pragma mark - DisplayLanguageCode
extern NSString* const kTCGBDisplayLanguageCodeGerman;
extern NSString* const kTCGBDisplayLanguageCodeEnglish;
extern NSString* const kTCGBDisplayLanguageCodeSpanish;
extern NSString* const kTCGBDisplayLanguageCodeFinish;
extern NSString* const kTCGBDisplayLanguageCodeFrench;
extern NSString* const kTCGBDisplayLanguageCodeIndonesian;
extern NSString* const kTCGBDisplayLanguageCodeItalian;
extern NSString* const kTCGBDisplayLanguageCodeJapanese;
extern NSString* const kTCGBDisplayLanguageCodeKorean;
extern NSString* const kTCGBDisplayLanguageCodePortuguese;
extern NSString* const kTCGBDisplayLanguageCodeRussian;
extern NSString* const kTCGBDisplayLanguageCodeThai;
extern NSString* const kTCGBDisplayLanguageCodeVietnamese;
extern NSString* const kTCGBDisplayLanguageCodeMalay;
extern NSString* const kTCGBDisplayLanguageCodeChineseSimplified;
extern NSString* const kTCGBDisplayLanguageCodeChineseTraditional;
```


#### 在Gamebase初始化时设置显示语言 

在Gamebase初始化时可以设置显示语言。

**API**

```objectivec
+ (void)initializeWithConfiguration:(TCGBConfiguration *)configuration completion:(InitializeCompletion)completion;
```

**Example**

```objectivec
- (void)initializeWithDisplayLanguageConfiguration {
        TCGBConfiguration* config = [TCGBConfiguration configurationWithAppID:@"your app(project) ID" appVersion:@"your app version"];
        [config setDisplayLanguageCode:kTCGBDisplayLanguageCodeEnglish];
        [TCGBGamebase initializeWithConfiguration:config completion:^(LaunchingInfo launchingData, TCGBError *error) {
            if ([TCGBGamebase isSuccessWithError:error] == YES) {
                NSLog(@"TCGBGamebase initialization is succeeded");
                // Check status of you app.
                // If status of app is maintenance or terminated service or etc, you must blocking UI and you should make user cannot log in your service.
                // You can use [TCGBLaunching launchingStatus] method to check status of your app.
            }
            else {
                NSLog(@"TCGBGamebase initialization is failed with error:[%@]", [error description]);
            }
        }];
    }
```

#### 设置显示语言

Gamebase初始化时可更改输入的 Display Language。

**API**

```objectivec
+ (void)setDisplayLanguageCode:(NSString *)languageCode;
```

**Example**

```objectivec
- (void)setDisplayLanguageCode {
    [TCGBGamebase setDisplayLanguageCode:kTCGBDisplayLanguageCodeEnglish];
}
```

#### 查看显示语言

可以查询当前使用的显示语言。

**API**

```objectivec
+ (NSString *)displayLanguageCode;
```

**示例**

``` cs
- (void)getDisplayLanguageCode()
{
    NSString* displayLanguage = [TCGBGamebase displayLanguageCode];
}
```

#### 添加新语言集

如果要使用Gamebase提供的默认语言(ko, en)外其他语言，将值添加到Gamebase.bundle 文件的 Resource文件夹中 **localizedstring.json** 文件。

localizedstring.json中定义的格式如下。

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
}
```

如果需要添加另一种语言集，可在localizedstring.json文件中添加 `"${语言代码}":{"key":"value"}` 形式的值。

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
  "${语言代码}": {
      "common_ok_button": "...",
      ...
  }
}
```

如果在上述JSON文件的格式"${语言代码}":{ }中缺少 key，则会自动输入`在设备上设置的语言`或`en`。

#### 显示语言优先级

通过初始化或SetDisplayLanguageCode API设置的Display Language时，最终应用的Display Language可以与输入的值不同。

1. 确认输入的languageCode是否在localizedstring.json文件中定义。
2. 初始化Gamebase时，确认在localizedstring.json文件中定义了设备上设置的语言代码（即使在初始化后更改了设备上设置的语言，此值也将保留）。
3. 自动设置Display Language的默认值为`en`。


### Country Code

* Gamebase는 System의 Country Code를 다음과 같은 API로 제공하고 있습니다.
* 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가코드를 리턴합니다.
* USIM에 잘못된 국가코드가 기록되어 있다 하더라도 추가적인 체크 없이 그대로 리턴합니다.
* 값이 비어있는 경우 'ZZ'를 리턴합니다.

**API**

```objectivec
+ (NSString *)usimCountryCode;
```

#### Device Country Code

* OS 로부터 전달받은 단말기 지역 설정을 추가적인 체크 없이 그대로 리턴합니다.
* 단말기 국가코드는 '설정 > 일반 > 언어 및 지역 > 지역' 설정에 따라 OS가 결정합니다.
* iOS 에서 제공하는 NSLocaleCountryCode 를 사용하여 획득한 값을 리턴합니다.

**API**

```objectivec
+ (NSString *)deviceCountryCode;
```

#### Intergrated Country Code

* USIM, 단말기 지역 설정의 순서로 국가 코드를 확인하여 리턴합니다.
* country API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 리턴합니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```objectivec
+ (NSString *)countryCode;
```



### Server Push
* 可以处理从Gamebase服务器发送到客户端设备的 Server Push Message。
* 在Gamebase客户端上添加ServerPushEvent Listener允许用户接收和处理信息，可以删除添加的ServerPushEvent Listener。


#### Server Push类型
目前，Gamebase支持的Server Push类型如下。

* kTCGBServerPushNotificationTypeAppKickout (= "appKickout")
    * 如果在TOAST Gamebase控制台的`Operation > Kickout`中注册kickout ServerPush消息，与Gamebase连接的所有客户端的将收到`APP_KICKOUT`消息。
* kTCGBServerPushNotificationTypeTransferKickout (= "transferKickout")
	* 如果通过TransferKey成功转移Guest帐户，则会向接收TransferKey的终端发送`TRANSFER_KICKOUT`信息。


![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### 注册 ServerPushEvent
可以在Gamebase Client中注册ServerPushEvent来处理Gamebase Console和Gamebase服务器发出的Push事件。

**API**

```objectivec
+ (void)addServerPushEvent:(void(^)(TCGBServerPushMessage *))handler;
```


**示例**
```objectivec
- (void)wannaToReceiveServerPush {
	void(^pushHandler)(TCGBServerPushMessage *) = ^(TCGBServerPushMessage *message) {
        NSString* msg = [NSString stringWithFormat:@"[Sample] receive server push =>\ntype: %@\ndata: %@", message.type, message.data];
        [self printLogAndShowAlertWithData:msg error:nil alertTitle:@"server push"];
        
        if ([message.type caseInsensitiveCompare:kTCGBServerPushNotificationTypeAppKickout] == NSOrderedSame) {
        	// Logout
            // Go to Main
        }
        else if ([message.type caseInsensitiveCompare:kTCGBServerPushNotificationTypeTransferKickout] == NSOrderedSame) {
        	// Logout
            // Go to Main
        }
        else {
        	...
        }
    };
    [TCGBGamebase addServerPushEvent:pushHandler];
}

```


#### 删除 ServerPushEvent
可以使用以下API删除在Gamebase注册的 ServerPushEvent。

**API**
```objectivec
+ (void)removeServerPushEvent:(void(^)(TCGBServerPushMessage *))handler;
+ (void)removeAllServerPushEvent;
```

**示例**
```objectivec
- (void)wannaToDiscardServerPush {
	void(^pushHandler)(TCGBServerPushMessage *) = ^(TCGBServerPushMessage *message) {
        NSString* msg = [NSString stringWithFormat:@"[Sample] receive server push =>\ntype: %@\ndata: %@", message.type, message.data];
        [self printLogAndShowAlertWithData:msg error:nil alertTitle:@"server push"];
    };
    [TCGBGamebase removeServerPushEvent:pushHandler];
}
```





### Observer
* Gamebase Observer允许接收并处理Gamebase的各种状态变动事件。
* 状态变动事件：网络类型变动，Launching状态变动(由于维护等引起的状态变动)，Heartbeat信息变动(用户禁用而导致Heartbeat信息变动）等。



#### Observer类型
Gamebase目前支持的 Observer类型如下。

* Network 类型变动
    * 可以接收网络变动信息。例如，可以用 ObserverMessage.data.get("code") 值获取 Network Type。
    * Type : kTCGBObserverMessageTypeNetwork (= @"network")
    * Code : 请参考NetworkStatus中声明的常量。
        * NotReachable : -1
        * ReachableViaWWAN : 0
        * ReachableViaWifi : 1        
        * ReachabilityIsNotDefined : -100
* Launching 状态变动
    * 当定期检查应用程序状态的Launching Status response发生变动时触发。例如，有维或推荐更新等产生的事件。
    * Type : kTCGBObserverMessageTypeLaunching (= @"launching")
    * Code : 请参考TCGBLaunchingStatus中声明的常量。
        * IN_SERVICE : 200
        * RECOMMEND_UPDATE : 201
        * IN_SERVICE_BY_QA_WHITE_LIST : 202
        * REQUIRE_UPDATE : 300
        * BLOCKED_USER : 301
        * TERMINATED_SERVICE : 302
        * INSPECTING_SERVICE : 303
        * INSPECTING_ALL_SERVICES : 304
        * INTERNAL_SERVER_ERROR : 500
* Heartbeat 信息变动
    * 定期与Gamebase服务器保持连接的Heartbeat response发生变动时触发。例如，由用户禁用引起的事件。
    * Type : ObserverkTCGBObserverMessageTypeHeartbeat (= @"heartbeat")
    * Code : 请参考TCGBErrorCode 中声明的常量
        * TCGB_ERROR_INVALID_MEMBER : 6
        * TCGB_ERROR_BANNED_MEMBER : 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### 注册 Observer
可以在Gamebase Client上注册Observer来处理各种状态变动事件。

**API**
```objectivec
+ (void)addObserver:(void(^)(TCGBObserverMessage *))handler;
```

**示例**
```objectivec
- (void)addObserver {
    void(^observerHandler)(TCGBObserverMessage *) = ^(TCGBObserverMessage *message) {
        NSString* msg = [NSString stringWithFormat:@"[Sample] receive from observer =>\ntype: %@\ndata: %@", message.type, message.data];
        [self printLogAndShowAlertWithData:msg error:nil alertTitle:@"Observer"];
            // You can check the changed network status in here.
        if ([message.type caseInsensitiveCompare:kTCGBObserverMessageTypeNetwork] == NSOrderedSame) {
            NSNumber* networkStatusNumber = message.data[@"code"];
            NSInteger networkStatus = [networkStatusNumber integerValue];
            // TODO: Check Netwokr Status by networkStatus.
        }
        else if ([message.type caseInsensitiveCompare:kTCGBObserverMessageTypeLaunching] == NSOrderedSame) {
            // You can check the changed launching status in here.
            NSNumber* launchingStatusNumber = message.data[@"code"];
            NSInteger launchingStatus = [launchingStatusNumber integerValue];
            // TODO: Check Launching Status by launchingStatus.
        }
        else if ([message.type caseInsensitiveCompare:kTCGBObserverMessageTypeHeartbeat] == NSOrderedSame) {
        	// You can check the invalid user session in here.
        	NSNumber* errorCodeNumber = message.data[@"code"];
        	NSInteger errorCode = [errorCodeNumber integerValue];
            if (errorCode == TCGB_ERROR_BANNED_MEMBER) {
            	// TODO: Execute User Ban Proccess.
			}
        }
    };

    [TCGBGamebase addObserver:observerHandler];
}
```


#### 删除 Observer
可以删除在Gamebase中注册的Observer。
 
**API**
```objectivec
+ (void)removeObserver:(void(^)(TCGBObserverMessage *))handler;
+ (void)removeAllObserver;
```

**示例**

```objectivec
- (void)removeObserver {
	void(^observerHandler)(TCGBObserverMessage *) = ^(TCGBObserverMessage *message) {
    	...
    };
    
    // Remove a Observer
    [TCGBGamebase removeObserver:observerHandler];
    
    // Remove all Observers
    [TCGBGamebase removeAllObserver];
}
```
