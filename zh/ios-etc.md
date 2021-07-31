## Game > Gamebase > iOS SDK 使用指南 > ETC

## Additional Features
以下描述Gamebase支持的附加功能。

### IDFA

* 返还终端机的广告标识符值。 

**API**

```objectivec
+ (NSString *)idfa;
```

> <font color="red">[注意]</font><br/>
>
> 如果在iOS 14以上版本上请求IDFA值，则需要获得用户权限。
> 需要设定用户权限时，在info.plist中设定您要显示的内容。 
> 请在info.plist中设定”Privacy - Tracking Usage Description”。




### Device Language

* 返回终端机设置的语言代码。
* 注册多种语言时，仅返回优先权最高的语言。

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language

正如游戏维护弹窗显示语言，Gamebase也显示终端机设置的语言。

但有些游戏允许通过额外选项更改终端机设置的语言。
终端机设置的默认语言是英语，但需将游戏的显示语言转换为日语时，即使要将Gamebase的显示语言也转换为日语，Gamebase仍显示终端机设置的默认语言（en）。
因此Gamebase向需以终端机设置语言之外的其他语言显示Gamebase消息的应用程序，提供”Display Language“功能。 

Gamebase显示消息时，按照注册为Display Language的语言显示消息。
在Display Language输入语言代码时，只能使用以下列表中（**Gamebase支持的语言代码种类**）指定的代码。

> <font color="red">[注意]</font><br/>
>
> * 无论终端机设置的语言如何，只需要更改Gamebase显示的语言时使用Display Language Gamebase功能。
> * Display Language Code是区分大小写的ISO-639形态的值。 
> 若按”EN"或"zh-cn"进行设置，可能出现问题。
> * 若输入的Display Language Code值不在以下列表时（**Gamebase支持的语言代码种类**）, 则将Display Langauge Code自动设置为默认语言(en)。

> [参考]
>
> * 因Gamebase客户端消息中仅包含英语（en）、韩语（ko）、日语（ja），即使是下列表指定的语言代码，指定英语（en）、韩语（ko）、日语（ja）之外的语言时，也将自动设置为默认语言(en)。
> * 可以直接添加未注册在Gamebase客户端的语言集合。
> **请参考**添加新语言集合**项目。

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


#### 初始化Gamebase时设定显示语言 

初始化Gamebase时可以设定显示语言。

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

#### 设定显示语言

初始化Gamebase时可更改输入的Display Language。

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

#### 查询显示语言

可以查看当前使用的显示语言。

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

如果要使用Gamebase提供的默认语言(ko, en)以外的其他语言，将值添加到Gamebase.bundle文件的Resource文件夹中的**localizedstring.json**文件。

在localizedstring.json中定义的格式如下。

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

如果需要添加另一种语言集，可在localizedstring.json文件中添加”"${语言代码}":{"key":"value"}”形式的值。

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

如果在上述JSON文件的格式"${语言代码}":{ }中缺少key，则会自动输入在设备上设置的语言或”en”。

#### 显示语言优先级

通过初始化或SetDisplayLanguageCode API设置的Display Language时，最终应用的Display Language可以与输入的值不同。

1. 确认输入的languageCode是否在localizedstring.json文件中定义。
2. 初始化Gamebase时，确认在localizedstring.json文件中是否定义了设备上设置的语言代码（即使在初始化后更改了设备上设置的语言，此值也将保留）。
3. 自动设置Display Language的默认值为”en”。


### Country Code

* Gamebase以如下API提供系统的国家代码(country code)。
* 各API具有不同特征，因此请选择与用途相符的API。

#### USIM Country Code

* 返回USIM中记录的国家代码。
* 即使USIM中记录的是错误的国家代码也将不进行补充确认就直接返回。
* 若值为空，则返回”ZZ”。

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
    3.若USIM、终端机国家代码均为空值，则返回”ZZ”。

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```objectivec
+ (NSString *)countryCode;
```


### Gamebase Event Handler

* Gamebase通过**GamebaseEventHandler**事件系统处理所有的事件。
* GamebaseEventHandler通过以下API简单添加或删除Handler。 

**API**

```objectivec
+ (void)addEventHandler:(GamebaseEventHandler)handler;
+ (void)removeEventHandler:(GamebaseEventHandler)handler;
+ (void)removeAllEventHandler;
```

**VO**

```objectivec
@interface TCGBGamebaseEventMessage : NSObject <TCGBValueObject>

@property (nonatomic, strong, nonnull)  NSString* category;
@property (nonatomic, strong, nullable) NSString* data;

@end
```

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBServerPushAppKickout] == YES
            || [message.category isEqualToString:kTCGBServerPushTransferKickout] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gaembaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push
            }
        }
        else if ([message.category isEqualToString:kTCGBObserverLaunching] == YES
                 || [message.category isEqualToString:kTCGBObserverHeartbeat] == YES
                 || [message.category isEqualToString:kTCGBObserverNetwork] == YES) {
            TCGBGamebaseEventObserverData* observerData = [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data];
            if (observerData != nil) {
                //TODO: process observer
            }
        }
        else if ([message.category isEqualToString:kTCGBPurchaseUpdated] == YES) {
            
        }
        else if ([message.category isEqualToString:kTCGBPushReceivedMessage] == YES) {
            
        }
        else if ([message.category isEqualToString:kTCGBPushClickMessage] == YES) {
            
        }
        else if ([message.category isEqualToString:kTCGBPushClickAction] == YES) {
            
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

* Category在GamebaseEventCategory类中定义。
* 事件大体分为ServerPush、Observer、Purchase及Push，并按各Category, 按如下列表的方式，将GamebaseEventMessage.data转换为VO。

| Event种类 | GamebaseEventCategory | VO转换方法 | 备注 |
| --------- | --------------------- | ----------- | --- |
| ServerPush | kTCGBServerPushAppKickout<br>kTCGBServerPushTransferKickout | [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data] | \- |
| Observer | kTCGBObserverLaunching<br>kTCGBObserverHeartbeat<br>kTCGBObserverNetwork | [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data] | \- |
| Purchase - Promotion支付 | kTCGBPurchaseUpdated | [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data] | \- |
| Push - 接收消息 | kTCGBPushReceivedMessage | [TCGBPushMessage pushMessageFromJsonString:message.data] | \- |
| Push - - 点击消息 | kTCGBPushClickMessage | [TCGBPushMessage pushFromJsonString:message.data] | \- |
| Push - 动态点击 | kTCGBPushClickAction | [TCGBPushMessage pushFromJsonString:message.data] | 点击RichMessage按键时启动。|

#### Server Push

* 是从Gamebase服务器向客户端终端机传送的消息。
* Gamebase支持的Server Push Type如下。
	* kTCGBServerPushAppKickout
    	* 如果在NHN Cloud Gamebase控制台**Operation > Kickout**中注册Kickout ServerPush消息，则从与Gamebase连接的所有客户端接收Kickout消息。
    * kTCGBServerPushTransferKickout
    	* Guest账号成功转移到其他终端机时，将会从转移之前的终端机接收Kickout消息。

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        [self printLogAndShowAlertWithData:[message prettyJsonString] error:nil alertTitle:@"addEventHandler Result"];
        if ([message.category isEqualToString:kTCGBServerPushAppKickout] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push
            }
        }
        esle if ([message.category isEqualToString:kTCGBServerPushTransferKickout] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}

```

#### Observer

* 是处理Gamebase各状态的变动事件的系统。 
* Gamebase支持的Observer Type如下。
    * kTCGBObserverLaunching
    	* 当维护开始、结束时或发布新版本必须进行更新等Launching状态出现变动时启动。
    	* TCGBGamebaseEventObserverData.code : 为TCGBLaunchingStatus值。
            * IN_SERVICE: 200
            * RECOMMEND_UPDATE: 201
            * IN_SERVICE_BY_QA_WHITE_LIST: 202
            * REQUIRE_UPDATE: 300
            * BLOCKED_USER: 301
            * TERMINATED_SERVICE: 302
            * INSPECTING_SERVICE: 303
            * INSPECTING_ALL_SERVICES: 304
            * INTERNAL_SERVER_ERROR: 500
    * kTCGBObserverHeartbeat
    	* 当因已被退出或禁用、用户账号状态出现变化时启动。
    	* TCGBGamebaseEventObserverData.code : 为TCGBError值。 
            * TCGB_ERROR_INVALID_MEMBER: 6
            * TCGB_ERROR_BANNED_MEMBER: 7
    * kTCGBObserverNetwork
    	* 可以接收网络变动信息。 
    	* 当网络断开或被连接时、从Wifi转为Cellular网络时启动。
    	* TCGBGamebaseEventObserverData.code : 为NetworkManager值。
            * ReachabilityIsNotDefined = -100
            * NotReachable = -1
            * ReachableViaWWAN = 0
            * ReachableViaWifi = 1

**VO**

```objectivec
@interface TCGBGamebaseEventObserverData : NSObject <TCGBValueObject>

@property (nonatomic, assign)           int64_t     code;
@property (nonatomic, strong, nullable) NSString*   message;
@property (nonatomic, strong, nullable) NSString*   extras;
```

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBObserverLaunching] == YES) {
            TCGBGamebaseEventObserverData* observerData = [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data];
            if (observerData != nil) {
                int launchingStatusCode = observerData.code;
                NSString* launchingMessage = observerData.message;
                switch (launchingStatusCode) {
                    case IN_SERVICE:
                    // Finished maintenance.
                    break;
                case INSPECTING_SERVICE:
                case INSPECTING_ALL_SERVICES:
                    // Under maintenance.
                    break;
                ...
        }
            }
        }
        else if ([message.category isEqualToString:kTCGBObserverHeartbeat] == YES) {
            TCGBGamebaseEventObserverData* observerData = [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data];
            int errorCode = observerData.code;
            switch (errorCode) {
            case TCGB_ERROR_INVALID_MEMBER:
                // You can check the invalid user session in here.
                // ex) After transferred account to another device.
                break;
            case TCGB_ERROR_BANNED_MEMBER:
                // You can check the banned user session in here.
                break;
        }
        }
        else if ([message.category isEqualToString:kTCGBObserverNetwork] == YES) {
            TCGBGamebaseEventObserverData* observerData = [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data];
            NetworkStatus networkTypeCode = observerData.code;
            // You can check the changed network status in here.
            if (networkTypeCode == NotReachable || networkTypeCode == ReachabilityIsNotDefined) {
                // Network disconnected.
            } else {
                // Network connected.
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```


#### Purchase Updated

* * 是输入Promotion代码获取商品时出现的事件。
* 可以获取结算票据信息。

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        TCGBPurchasableReceipt* receipt = [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data];
        if (receipt != nil) {
            // If user purchase item from appstore promoting iap
            // this event will be occurred.
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

#### Push Received Message

* 是接收Push消息时出现的事件。
* 通过将extras字段转换为JSON，可获取发送Push时传送的自定义信息。


**VO**

```objectivec
@interface TCGBPushMessage : NSObject <TCGBValueObject>

@property (nonatomic, strong, nonnull) NSString* identifier;
@property (nonatomic, strong, nullable) NSString* title;
@property (nonatomic, strong, nullable) NSString* body;
@property (nonatomic, strong, nonnull) NSString* extras;

@end
```

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBPushReceivedMessage] == YES) {
            TCGBPushMessage* pushMessage = [TCGBPushMessage pushMessageFromJsonString:message.data];
            if (pusMessage != nil) {
                //TODO: process 
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

#### Push Click Message

* 是点击”已接收的Push消息”时出现的事件。

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBPushClickMessage] == YES) {
            TCGBPushMessage* pushMessage = [TCGBPushMessage pushMessageFromJsonString:message.data];
            if (pusMessage != nil) {
                //TODO: process 
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

#### Push Click Action

* 是通过Rich Message功能，点击生成按钮时出现的事件。
* actionType中存在以下值。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```objectivec
@interface TCGBPushAction : NSObject <TCGBValueObject>

@property (nonatomic, strong, nonnull) NSString* actionType;
@property (nonatomic, strong, nullable) TCGBPushMessage* message;
@property (nonatomic, strong, nullable) NSString* userText;

@end
```

**示例**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBPushClickAction] == YES) {
            TCGBPushAction* pushMessage = [TCGBPushAction pushActionFromJsonString:message.data];
            if (pushAction != nil) {
                //TODO: process 
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```


 
### Analytics

可将游戏指标传送到Gamebase服务器。

> <font color="red">[注意]</font><br/>
>
> 登录后可调用Gamebase Analytics支持的所有API。

> [TIP]
>
> 调用TCGBPurchase的requestPurchaseWithItemSeq:viewController:completion API付款或调用setPromotionIAPHandler完成促销付款后，自动传送指标。

Analytics控制台使用方法，请参考如下指南。

- [Analytics控制台](./oper-analytics)

#### Game User Data Settings

登录游戏后，可将游戏用户级别信息作为指标传送。

> <font color="red">[注意]</font><br/>
>
> 若登录游戏后不调用SetGameUserData API，则其他指标中可能遗漏级别信息。
>

调用API所需的参数如下。

**GameUserData**

| Name | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示游戏用户级别的字段。|
| channelId | O | String | 是显示通道的字段。|
| characterId | O | String | 是显示角色名的字段。|
| classId | O | String | 是显示职业的字段。|


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

升级后可将游戏用户级别信息作为指标传送。

调用API所需的参数如下。

**LevelUpData**

| Name | Mandatory (M) / Optional (O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示游戏用户级别的字段。|
| levelUpTime | M | long | 按Epoch time输入。</br>按Millisecond(ms)单位输入。|
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

Gamebase提供应对客户咨询的功能。

> [TIP]
>
> 如果与NHN Cloud Contact服务联动使用，则可更加轻松方便地应对客户咨询。
> 详细的NHN Cloud Contact服务使用，请参考如下指南。
> [NHN Cloud Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)

#### Customer Service Type

**Gamebase控制台 > App > InApp URL > 您可从以下Service center**客户服务类型当中选择一个类型。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △              |
| NHN Cloud Online Contact      | △              |

Gamebase SDK的客户服务API根据类型使用以下URL。

* 开发公司自建客户服务(Developer customer center)
    * 在**客户服务URL**中输入的URL。
* Gamebase提供的客户服务(Gamebase customer center)
    * 登录前 : **不包含**用户信息的客户服务URL。
    * 登录后 : 包含用户信息的客户服务URL。
* NHN Cloud组织服务(Online Contact)
    * 登录前 : **不包含**用户信息的客户服务URL。
    * 登录后 : 包含用户信息的客户服务URL。

#### Open Contact WebView

是显示Gamebase控制台中输入的**客户服务URL**Webview的功能。
可通过TCGBContactConfiguration向URL传送附加信息。 

**TCGBContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | 用户名(nickname)<br>**default** : nil    | 
| additionalURL | O             | string                             | 是添加在开发公司自建客户服务URL后面的附加URL。<br>只能在客户服务类型为”CUSTOM”时使用。<br>**default** : nil    |
| extraData     | O             | dictionary<string, string>         | 客户服务open时传送开发公司需要的extra data。<br>**default** : nil    | 

**API**

```objectivec
+ (void)openContactWithViewController:(UIViewController *)viewController 
                           completion:(void(^)(TCGBError *error))completion;

+ (void)openContactWithViewController:(UIViewController *)viewController
                        configuration:(TCGBContactConfiguration *)configuration
                           completion:(void(^)(TCGBError *error))completion;
```

**Error Code**

| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | 未调用Gamebase。|
| TCGB\_ERROR\_NOT\_LOGGED\_IN | 2       | 客户服务的类型为”NHN Cloud Online Contact”时，登录前已调用了函数。|
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 客户服务URL不存在。<br>请确认Gamebase控制台中的**客户服务URL**。|
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 识别用户的临时ticket发放失败 |

**Example**

```objectivec
[TCGBContact openContactWithViewController:self completion:^(TCGBError *error) {
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        // A user close the contact web view.
    } else if (error.code == TCGB_ERROR_UI_CONTACT_FAIL_INVALID_URL) {
        // TODO: Gamebase Console Service Center URL is invalid.
        // Please check the url field in the TOAST Gamebase Console.
    } else {
        // TODO: Error occur when opening the contact web view.
    }
}];
```

> <font color="red">[注意]</font><br/>
> 
> 向客服提问咨询时，为了添附文件可能需要允许访问相机或相册权限。
> 请在info.plist设置”Privacy - Camera Usage Description“、”Privacy - Photo Library Usage Description”。 

#### Request Contact URL

可以获取显示客户服务Webview时使用的URL。

**API**

```objectivec
+ (void)requestContactURLWithCompletion:(void(^)(NSString *contactUrl, TCGBError *error))completion;

+ (void)requestContactURLWithConfiguration:(TCGBContactConfiguration *)configuration
                                completion:(void(^)(NSString *contactUrl, TCGBError *error))completion;
```

**Error Code**

| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | 未调用Gamebase。|
| TCGB\_ERROR\_NOT\_LOGGED\_IN | 2       | 客户服务的类型为”NHN Cloud Online Contact”时，登录前已调用了函数。|
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 客服中心URL不存在。<br>请确认Gamebase控制台中的**客户服务URL**。|
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 识别用户的临时ticket发放失败 |

**Example**

```objectivec
[TCGBContact requestContactURLWithCompletion^(NSString *contactUrl, TCGBError *error){
    if ([TCGBGamebase isSuccessWithError:error] == YES) {
        NSLog(@"ContactURL : %@", contactUrl);
    } else if (error.code == TCGB_ERROR_UI_CONTACT_FAIL_INVALID_URL) {
        // TODO: Gamebase Console Service Center URL is invalid.
        // Please check the url field in the TOAST Gamebase Console.
    } else {
        // TODO: Error occur when request contact url.
    }
}];
```
