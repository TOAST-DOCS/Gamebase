## Game > Gamebase > iOS SDK 使用指南 > ETC

## Additional Features
以下描述Gamebase支持的附加功能。


### Device Language

* 返回终端机设置的语言代码。
* 注册多种语言时，仅返回优先权最高的语言。

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

```objectivec
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

* Gamebase以如下API提供系统的国家代码(country code)。
* 各API具有不同特征，因此请选择与用途相符的API。

#### USIM Country Code

* 返回USIM中记录的国家代码。
* 即使USIM中记录的是错误的国家代码也将不进行补充确认就直接返回。
* 若值为空，则返回’ZZ’。

**API**

```objectivec
+ (NSString *)usimCountryCode;
```

#### Device Country Code

* 从OS获得的终端机地区设置直接返回,不进行补充确认。
* 终端机国家代码按照**设置 > 常规 > 语言及地区 > 地区**设置，由OS自动决定。
* 返回使用iOS提供的NSLocaleCountryCode获得的值。

**API**

```objectivec
+ (NSString *)deviceCountryCode;
```

#### Intergrated Country Code

* 按照USIM、终端机地区设置的顺序确认国家代码并返回。
* country API按照如下顺序运行。
    1.确认USIM中记录的国家代码，若存在值，则直接返回，不另行确认。
    2.若USIM国家代码为空值，确认终端机国家代码，若存在值，则直接返回，不另行确认。
    3.若USIM、终端机国家代码均为空值，则返回’ZZ’。

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
    * 如果在NHN Cloud Gamebase控制台的`Operation > Kickout`中注册kickout ServerPush消息，与Gamebase连接的所有客户端的将收到`APP_KICKOUT`消息。

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

### Analytics

可将游戏指标传送至Gamebase服务器。

> <font color="red">[注意]</font><br/>
>
> Gamebase Analytics支持的所有API登录后可调用。

> [TIP]
>
> 调用TCGBPurchase的requestPurchaseWithItemSeq:viewController:completion API付款或调用setPromotionIAPHandler完成促销付款后，自动传送指标。

Analytics控制台使用方法请参考如下指南。

- [Analytics控制台](./oper-analytics)

#### Game User Data Settings

登录游戏后游戏用户级别信息可作为指标传送。

> <font color="red">[注意]</font><br/>
>
> 若登录游戏后不调用SetGameUserData API，则其他指标中可能遗漏级别信息。
>

调用API所需的参数如下。

**GameUserData**

| Name | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示游戏用户级别的字段。 |
| channelId | O | String | 是显示通道的字段。 |
| characterId | O | String | 是显示角色名的字段。 |
| classId | O | String | 是显示职业的字段。 |


**API**

```objectivec
+ (void)setGameUserData:(nonnull TCGBAnalyticsGameUserData *)gameUserData;
```

**Example**

```objectivec
- (void)setGameUserDataWithLevel:(int)level channelId:(NSString *)channelId characterId:(NSString *)characterId {
    TCGBAnalyticsGameUserData* gameUserData = [TCGBAnalyticsGameUserData gameUserDataWithUserLevel:level];
    [gameUserData setChannelId:channelId];
    [gameUserData setCharacterId:characterId];
    [TCGBAnalytics setGameUserData:gameUserData];
}
```

#### Level Up Trace

升级后游戏用户级别信息可作为指标传送。

调用API所需的参数如下。

**LevelUpData**

  | Name | Mandatory (M) / Optional (O) | type | Desc |
  | -------------------------- | -------------------------- | ---- | ---- |
  | userLevel | M | int | 是显示游戏用户级别的字段。 |
  | levelUpTime | M | long | 按Epoch Time输入。</br>按Millisecond单位输入。 |
  | channelId | O | string | |
  | characterId | O | string | |

**API**

```objectivec
+ (void)traceLevelUpWithLevelUpData:(nonnull TCGBAnalyticsLevelUpData *)levelUpData;
```

**Example**

```objectivec
- (void)traceLevelUpWith:(int)level levelUpTime:(long long)levelUpTime channelId:(NSString *)channelId characterId:(NSString *)characterId {
  TCGBAnalyticsLevelUpData* levelUpData = [TCGBAnalyticsLevelUpData levelUpDataWithUserLevel:level levelUpTime:levelUpTime];
  [TCGBAnalytics traceLevelUpWithLevelUpData:levelUpData];
}
```

### Contact

Gamebase提供用于应对客户咨询的功能。

> [TIP]
>
> 若与NHN Cloud Contact商品关联使用，则可更加轻松方便地应对顾客咨询。
> 详细的NHN Cloud Contact商品使用，请参考如下指南。
> [NHN Cloud Online Contact Guide](/Contact%20Center/zh/online-contact-overview/)
>

#### Open Contact WebView

可弹出Gamebase Console中输入的**客服中心URL**页面的功能。
使用**Gamebase Console > App > InApp URL > Service center**中输入的值。

**API**

```objectivec
+ (void)openContactWithViewController:(UIViewController *)viewController completion:(void(^)(TCGBError *error))completion;
```

**Example**

```objectivec
[TCGBContact openContactWithViewController:parentViewController completion:^(TCGBError *error) {
    if (error != NULL && error.code == TCGB_ERROR_WEBVIEW_INVALID_URL) { // 7001
        // TODO: Gamebase Console Service Center URL is invalid.
        //  Please check the url field in the TOAST Gamebase Console.
    } else if (error != NULL) {
        // TODO: Error occur when opening the contact web view.
    } else {
        // A user close the contact web view.
    }
}];
```
