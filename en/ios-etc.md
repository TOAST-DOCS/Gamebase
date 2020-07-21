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
* The display language can be changed into another language which is not set on a device. 
* Gamebase displays messages which are included in a client or as received by a server.
* With DisplayLanguage, messages are displayed in an appropriate language for the language code (ISO-639) set by the user. 
* If necessary, language sets can be added as the user wants. The list of available language codes is as follows:

> [Note]
>
> Client messages of Gamebase include English(en), Korean(ko) and Japanese(ja) only.

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

> `[Warning]`
>
> Gamebase distinguishes the language code between the upper and lower case. 
> For example, settings like 'EN' or 'zh-ch' may cause a problem. 

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



### Server Push
* Handles Server Push Messages from Gamebase server to a client device. 
* Add ServerPushEvent Listener to Gamebase Client, and the user can handle messages; the added ServerPushEvent Listener can be deleted.


#### Server Push Type
Server Push Types currently supported by Gamebase are as follows: 

* kTCGBServerPushNotificationTypeAppKickout (= "appKickout")
    * Go to **Operation > Kickout**  in the TOAST Gamebase console and register Kickout ServerPush messages, and **APP_KICKOUT** messages are sent to all clients connected to Gamebase.


![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Use the API below, register ServerPushEvent to handle the push event triggered from the Gamebase Console and Gamebase server.

**API**

```objectivec
+ (void)addServerPushEvent:(void(^)(TCGBServerPushMessage *))handler;
```


**Example**
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


#### Remove ServerPushEvent
Use the APIs below to delete ServerPushEvent registered in Gamebase. 

**API**
```objectivec
+ (void)removeServerPushEvent:(void(^)(TCGBServerPushMessage *))handler;
+ (void)removeAllServerPushEvent;
```

**Example**
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
* With Gamebase Observer, receive and process status change events of Gamebase.
* Status change events : change of network type, change of launching status (change of status due to maintenance, and etc.), and change of heartbeat information (change of heartbeat information due to service suspension), and etc.



#### Observer Type
The Observer Types currently supported by Gamebase are as follows:

* Change of Network Type
    * Receive information on changes of a network. For instance, find a network type with the message.data[@"code"] value.
    * Type: kTCGBObserverMessageTypeNetwork (= @"network")
    * Code: Refer to the constant numbers declared in NetworkStatus. 
        * NotReachable : -1
        * ReachableViaWWAN : 0
        * ReachableViaWifi : 1        
        * ReachabilityIsNotDefined : -100
* Change of Launching Status
    * Occurs when there is a change in the launching status response which periodically checks application status. For example, events occur for maintenance, or update recommendations.
    * Type : kTCGBObserverMessageTypeLaunching (= @"launching")
    * Code : Refer to the constant numbers declared in TCGBLaunchingStatus.
        * IN_SERVICE : 200
        * RECOMMEND_UPDATE : 201
        * IN_SERVICE_BY_QA_WHITE_LIST : 202
        * REQUIRE_UPDATE : 300
        * BLOCKED_USER : 301
        * TERMINATED_SERVICE : 302
        * INSPECTING_SERVICE : 303
        * INSPECTING_ALL_SERVICES : 304
        * INTERNAL_SERVER_ERROR : 500
* Change of Heartbeat Information
    * Occurs when there is a change in the heartbeat response which periodically maintains connection with the Gamebase server. For example, an event occurs for service suspension.
    * Type : ObserverkTCGBObserverMessageTypeHeartbeat (= @"heartbeat")
    * Code : Refer to the constant numbers declared in TCGBErrorCode.
        * TCGB_ERROR_INVALID_MEMBER : 6
        * TCGB_ERROR_BANNED_MEMBER : 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Use the API below, register Observer to handle the status change events of Gamebase.

**API**
```objectivec
+ (void)addObserver:(void(^)(TCGBObserverMessage *))handler;
```

**Example**
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


#### Remove Observer
Use the APIs below to delete Observer registered in Gamebase.

**API**
```objectivec
+ (void)removeObserver:(void(^)(TCGBObserverMessage *))handler;
+ (void)removeAllObserver;
```

**Example**

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

| Name | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | Describes the level of game user. |
| channelId | O | String | Describes the channel. |
| characterId | O | String | Describes the name of character. |
| classId | O | String | Describes the occupation. |


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

| Name | Mandatory (M) / Optional (O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | Describes the level of game user. |
| levelUpTime | M | long | Enter in Epoch Time</br>by the millisecond. |
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
> By integrating with TOAST Contact, customer inquiries can be handled more at ease and convenience. 
> For more details on TOAST Contact, see the guide as below: 
> [TOAST Online Contact Guide](/Contact%20Center/en/online-contact-overview/)

#### Open Contact WebView

The webview for **Customer Center URL** can be displayed as on Gamebase Console. 
Apply the same input values for **Gamebase Console > App > InApp URL > Service center**.

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
