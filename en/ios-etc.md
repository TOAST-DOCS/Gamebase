## Game > Gamebase > iOS SDK User Guide > ETC

## Additional Features
Additional functions provided by Gamebase are described as below:


### Device Language

* Returns the language code from the device.
* If there are several languages registered, only the language of top priority is returned.

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language

Similar to the Maintenance popup, the language used by the device will be displayed as the Gamebase language.

However, there are games that may use a language different from the device language with separate options.
For example, if the language configured for the device is English and you changed the game language to Japanese, the language displayed will be still English, even though you might want to see Japanese on the Gamebase screen.

For this, Gamebase provides a Display Language feature for applications that want to use a language that is not the language configured by the device for Gamebase.

Gamebase displays its messages in the language set in Display Language.
The language code entered for Display Language should be one of the codes listed in the table (**Types of language codes supported by Gamebase) below:

> <font color="red">[Caution]</font><br/>
>
> * Use Display Language only when you want to change the language displayed in Gamebase to a language other than the one configured by the device.
> * Display Language Code is a case-sensitive value in the form of ISO-639.
> There could be a problem if it is configured as a value such as 'EN' or 'zh-cn'.
> * If the value entered as Display Language Code does not exist in the table (**Types of language codes supported by Gamebase) below, Display Language Code is automatically set to English (en) by default.

> [Note]
>
> * As the client messages of Gamebase include only English (en), Korean (ko), and Japanese (ja), if you try to set a language other than English (en), Korean (ko), or Japanese (ja), even though the language code might be listed in the table below, the value is automatically set to English (en) by default.
> * You can manually add a language set that is not included in the Gamebase client.
> See the **Add New Language Set** section.


#### Types of Language Codes Supported by Gamebase 
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

Each language code is defined in `TCGBConstants.h`.


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


#### Set Display Language with Gamebase Initialization 

Display Language can be set when Gamebase is initialized.

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

#### Set Display Language

You can change the initial setting of Display Language.

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

#### Get Display Language

You can retrieve the current application of Display Language.

**API**

```objectivec
+ (NSString *)displayLanguageCode;
```

**Example**

```objectivec
- (void)getDisplayLanguageCode()
{
    NSString* displayLanguage = [TCGBGamebase displayLanguageCode];
}
```

#### Add New Language Sets

To use another language in addition to default Gamebase languages (en, ko, ja), add a value to the **localizedstring.json** file of the Resource folder under the Gamebase.bundle file.

The localizedstring.json has a format defined as below:

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

To add another language, add `"${language code}":{"key":"value"}` to the localizedstring.json file. 

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
  "${언어코드}": {
      "common_ok_button": "...",
      ...
  }
}
```

If key is missing from inside of "${language code}":{ } of the json format above, `Language Set on Device` or `en` will be automatically entered. 

#### Priority in Display Language

If Display Language is set via initialization and SetDisplayLanguageCode API, the final application may be different from what has been entered.

1. Check if the languageCode you enter is defined in the localizedstring.json file. 
2. See if, during Gamebase initialization, the language code set on the device is defined in the localizedstring.json file. (This value shall maintain even if the language set on device changes after initialization.)
3. `en`, which is the default value of Display Language, is automatically set.


### Country Code

* Gamebase provides (country codes) for the system in the following APIs.
* Please select an appropriate API that best fits your purpose as each API has its own characteristics.

#### USIM Country Code

* Returns a country code written in the USIM.
* Even if a wrong country code has been written in the USIM, it will be returned as it is without any further verification.
* 'ZZ' is returned when the value is empty.

**API**

```objectivec
+ (NSString *)usimCountryCode;
```

#### Device Country Code

* Returns the device region setting received from the OS as it is without any further verification.
* The country code of the device is automatically determined by the OS according to the **Setting> General > Language and Region > Region** settings.
* Returns the value obtained using NSLocaleCountryCode provided by iOS.

**API**

```objectivec
+ (NSString *)deviceCountryCode;
```

#### Intergrated Country Code

* Verifies and returns the country code in the order of the region setting in the USIM.
* The country API operates in the following order:
	1. Checks the country code written in the USIM. If a value exists, returns the value without any separate verification.
	2. If the country code in the USIM is empty, checks the country code of the device. If a value exists, returns the value without any further verification.
	3. 'ZZ' is returned when the country code values of USIM and device are empty.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```objectivec
+ (NSString *)countryCode;
```


### Gamebase Event Handler

* Gamebase can process all kinds of events in a single event system called **GamebaseEventHandler**.
* GamebaseEventHandler can simply add or remove a Handler through the API below:

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

* Category is defined in the GamebaseEventCategory class.
* In general, events can be categorized as ServerPush, Observer, Purchase, or Push; TCGBGamebaseEventMessage.data can be converted into a VO in the ways shown in the table shown below for each Category.

| Event type | GamebaseEventCategory | VO conversion method | Remarks |
| --------- | --------------------- | ----------- | --- |
| ServerPush | kTCGBServerPushAppKickout<br>kTCGBServerPushTransferKickout | [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data] | \- |
| Observer | kTCGBObserverLaunching<br>kTCGBObserverHeartbeat<br>kTCGBObserverNetwork | [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data] | \- |
| Purchase - Promotion payment | kTCGBPurchaseUpdated | [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data] | \- |
| Push - Message received | kTCGBPushReceivedMessage | [TCGBPushMessage pushMessageFromJsonString:message.data] | \- |
| Push - Message clicked | kTCGBPushClickMessage | [TCGBPushMessage pushFromJsonString:message.data] | \- |
| Push - Action clicked | kTCGBPushClickAction | [TCGBPushMessage pushFromJsonString:message.data] | Operates when the RichMessage button is clicked. |

#### Server Push

* This is a message sent from the Gamebase server to the client's device.
* The Server Push Types supported from Gamebase are as follows:
	* kTCGBServerPushAppKickout
    	* If you register a kickout ServerPush message in **Operation > Kickout** of the NHN Cloud Gamebase Console, then all clients connected to Gamebase will receive the kickout message.
    * kTCGBServerPushTransferKickout
    	* If the guest account is successfully transferred to another device, the previous device receives a kickout message.

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

* It is a system used to handle many different status-changing events in Gamebase.
* The Observer Types supported by Gamebase are as follows:
    * kTCGBObserverLaunching
    	* It operates when the Launching status is changed, for instance when the server is under maintenance, or the maintenance is over, or a new version is deployed and update is required.
    	* TCGBGamebaseEventObserverData.code: Indicates the TCGBLaunchingStatus value.
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
    	* Operates when the status of a user account changes, for instance when the user account is deleted or banned.
    	* TCGBGamebaseEventObserverData.code: Indicates the TCGBError value.
            * TCGB_ERROR_INVALID_MEMBER: 6
            * TCGB_ERROR_BANNED_MEMBER: 7
    * kTCGBObserverNetwork
    	* Can receive the information about the changes in the network.
    	* Operates when the network is disconnected or connected, or switched from Wi-Fi to a cellular network.
    	* TCGBGamebaseEventObserverData.code: Indicates the NetworkManager value.
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

* This event is triggered when a product is acquired by redeeming a promotion code.
* Can acquire payment receipt information.

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

#### Purchase Updated

* This event is triggered when a product is acquired by redeeming a promotion code.
* Can acquire payment receipt information.

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

* This event is triggered when a received message is clicked.

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

* This event is triggered when the button created by the Rich Message feature is clicked.
* actionType provides the following:
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

**Example**

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

The game index can be transferred to the Gamebase server.

> <font color="red">[Caution]</font><br/>
>
> All APIs supported by the Gamebase Analytics can be called after login.

> [TIP]
>
> The index is transmitted automatically when payment is made by calling requestPurchaseWithItemSeq:viewController:completion API of TCGBPurchase or when promotion payment is made by calling setPromotionIAPHandler.

Please see the following guide for how to use Analytics console.

- [Analytics console](./oper-analytics)

#### Game User Data Settings

The game user level information can be transmitted as an index after logging in to the game.

> <font color="red">[Caution]</font><br/>
>
> If the SetGameUserData API is not called after login to the game, the level information may be missed from other indexes.
>

Parameters required for calling the API are as follows:

**GameUserData**

| Name | Mandatory(M) / Optional(O) | Type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | This field represents game user's level. |
| channelId | O | String | This field represents channel. |
| characterId | O | String | This field represents character name. |
| classId | O | String | This field represents class. |


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

The game user level information can be transmitted as an index after leveling up.

Parameters required for calling the API are as follows:

**LevelUpData**

| Name | Mandatory (M) / Optional (O) | Type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | This field represents game user's level. |
| levelUpTime | M | long | Enter in Epoch time.</br>The unit is milliseconds. |
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

Gamebase provides features to respond to customer inquiries. 

> [TIP]
>
> Associate it with the NHN Cloud Contact service to easily respond to inquiries from customers.
> See the guide below if you want to know how to use the NHN Cloud Contact service in detail.
> [NHN Cloud Online Contact Guide] (/Contact%20Center/ko/online-contact-overview/)

#### Open Contact WebView

This feature is used to represent the **Customer Center URL** WebView entered in the Gamebase Console.
Uses the value entered from **Gamebase Console > App > InApp URL > Service Center**.

**API**

```objectivec
+ (void)openContactWithViewController:(UIViewController *)viewController completion:(void(^)(TCGBError *error))completion;
```

**Example**

```objectivec
[TCGBContact openContactWithViewController:parentViewController completion:^(TCGBError *error) {
    if (error != NULL && error.code == TCGB_ERROR_WEBVIEW_INVALID_URL) { // 7001
        // TODO: Gamebase Console Service Center URL is invalid.
        //  Please check the url field in the NHN Cloud Gamebase Console.
    } else if (error != NULL) {
        // TODO: Error occur when opening the contact web view.
    } else {
        // A user close the contact web view.
    }
}];
```
