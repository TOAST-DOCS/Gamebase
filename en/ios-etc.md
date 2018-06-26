## Game > Gamebase > iOS SDK User Guide > ETC

## Additional Features
Additional functions provided by Gamebase are described as below:

### Display Language
* The display language can be changed into another language which is not set on a device. 
* Gamebase displays messages which are included in a client or as received by a server.
* With DisplayLanguage, messages are displayed in an appropriate language for the language code (ISO-639) set by the user. 
* If necessary, language sets can be added as the user wants. The list of available language codes is as follows:

> [Note]
>
> Client messages of Gamebase include English(en) and Korean(ko), only.

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

``` cs
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



### Server Push
* Handles Server Push Messages from Gamebase server to a client device. 
* Add ServerPushEvent Listener to Gamebase Client, and the user can handle messages; the added ServerPushEvent Listener can be deleted.


#### Server Push Type
Server Push Types currently supported by Gamebase are as follows: 

* kTCGBServerPushNotificationTypeAppKickout (= "appKickout")
    * Go to `Operation > Kickout`  in the TOAST Gamebase console and register Kickout ServerPush messages, and `APP_KICKOUT` messages are sent to all clients connected to Gamebase.
* kTCGBServerPushNotificationTypeTransferKickout (= "transferKickout")
	* TransferKey 를 통해 게스트 계정 이전이 성공한 경우, TransferKey를 발급받았던 단말기로 `TRANSFER_KICKOUT` 메세지가 전송됩니다.

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