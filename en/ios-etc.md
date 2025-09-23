## Game > Gamebase > iOS SDK User Guide > ETC

## Additional Features
Additional functions provided by Gamebase are described as below:

### IDFA

* Returns the ad identifier value of the device.

**API**

```objectivec
+ (NSString *)idfa;
```

> <font color="red">[Caution]</font><br/>
>
> For iOS 14 or later, user permission must be required when requesting the IDFA value.
> When asking for user permission, the text to prompt must be set in info.plist.
> Please set 'Privacy - Tracking Usage Description' in info.plist.




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
> * If the value entered for Display Language Code does not exist in the table below (**Types of Language Codes Supported by Gamebase**), Display Language Code is set to the default language set in the Gamebase console.
>   * If the language is not set in the Gamebase console, English (en) is set as the default language.

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
| fi | Finnish |
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
extern NSString* const kTCGBDisplayLanguageCodeFinnish;
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

If you are using a language other than the default language provided by Gamebase (ko, en, ja, zh-CN, zh-TW, th), add a **localizedstring.json** file to `Copy Bundle Resources` in the Xcode project.

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
  "zh-CN": {
    "common_ok_button": "确定",
    "common_cancel_button": "取消",
    ...
    "launching_service_closed_title": "关闭服务"
  },
  "zh-TW": {
    "common_ok_button": "好",
    "common_cancel_button": "取消",
    ...
    "launching_service_closed_title": "服務關閉"
  },
  "th": {
    "common_ok_button": "ยืนยัน",
    "common_cancel_button": "ยกเลิก",
    ...
    "launching_service_closed_title": "ปิดให้บริการ"
  },
  "de": {},
  "es": {},
  ...
  "ms": {}
}
```

If you need to add another language set, you can add a value in the form of `"key":"value"` to the corresponding language code in the localizedstring.json file.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  ...
  "vi": {
    "common_ok_button": "value",
    "common_cancel_button": "value",
    ...
    "launching_service_closed_title": "value"
  },
  "ms": {}
}
```

#### Priority in Display Language

If Display Language is set via initialization and SetDisplayLanguageCode API, the final application may be different from what has been entered.

1. Check if the languageCode you enter is defined in the localizedstring.json file. 
2. If step 1 fails, check if the language code set in the device is defined in the localizedstring.json file during the initialization of Gamebase. (This value is maintained even if the language set in the device is changed after initialization.)
3. If step 2 fails, the default language set in the Gamebase console is set as the Display Language.
4. If there is no language set in the Gamebase console, `en` is set as the default value.

### Country Code

* Gamebase provides (country codes) for the system in the following APIs.

#### Device Country Code

* Returns the device region setting received from the OS as it is without any further verification.
* The country code of the device is automatically determined by the OS according to the **Setting> General > Language and Region > Region** settings.
* Returns the value obtained using NSLocaleCountryCode provided by iOS.

**API**

```objectivec
+ (NSString *)deviceCountryCode;
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
        if ([message.category isEqualToString:kTCGBIdPRevoked] == YES) {
            TCGBGamebaseEventIdPRevokedData* idPRevokedData = [TCGBGamebaseEventIdPRevokedData gamebaseEventIdPRevokedDataFromJsonString:message.data];
            if (idPRevokedData != nil) {
                //TODO: process idp revoked
            }
        } else if ([message.category isEqualToString:kTCGBLoggedOut] == YES) {
            TCGBGamebaseEventLoggedOutData* loggedOutData = [TCGBGamebaseEventLoggedOutData gamebaseEventLoggedOutDataFromJsonString:message.data];
            if (loggedOutData != nil) {
                //TODO: process loggedOut
            }
        } else if ([message.category isEqualToString:kTCGBServerPushAppKickoutMessageReceived] == YES
            || [message.category isEqualToString:kTCGBServerPushAppKickout] == YES
            || [message.category isEqualToString:kTCGBServerPushTransferKickout] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push
            }
        } else if ([message.category isEqualToString:kTCGBObserverLaunching] == YES
            || [message.category isEqualToString:kTCGBObserverHeartbeat] == YES
            || [message.category isEqualToString:kTCGBObserverNetwork] == YES) {
            TCGBGamebaseEventObserverData* observerData = [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data];
            if (observerData != nil) {
                //TODO: process observer
            }
        } else if ([message.category isEqualToString:kTCGBPurchaseUpdated] == YES) {
            
        } else if ([message.category isEqualToString:kTCGBPushReceivedMessage] == YES) {
            
        } else if ([message.category isEqualToString:kTCGBPushClickMessage] == YES) {
            
        } else if ([message.category isEqualToString:kTCGBPushClickAction] == YES) {
            
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

* Category is defined in the GamebaseEventCategory class.
* In general, events can be categorized into IdPRevoked, LoggedOut, ServerPush, Observer, Purchase, or Push. TCGBGamebaseEventMessage.data can be converted into a VO in the ways shown in the following table for each Category.

| Event type | GamebaseEventCategory | VO conversion method | Remarks |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | kTCGBIdPRevoked | [TCGBGamebaseEventIdPRevokedData gamebaseEventIdPRevokedDataFromJsonString:message.data] | \- |
| LoggedOut | kTCGBLoggedOut | [TCGBGamebaseEventLoggedOutData gamebaseEventLoggedOutDataFromJsonString:message.data] | \- |
| ServerPush | kTCGBServerPushAppKickoutMessageReceived<br>kTCGBServerPushAppKickout<br>kTCGBServerPushTransferKickout | [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data] | \- |
| Observer | kTCGBObserverLaunching<br>kTCGBObserverHeartbeat<br>kTCGBObserverNetwork | [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data] | \- |
| Purchase - Promotion payment | kTCGBPurchaseUpdated | [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data] | \- |
| Push - Message received | kTCGBPushReceivedMessage | [TCGBPushMessage pushMessageFromJsonString:message.data] | \- |
| Push - Message clicked | kTCGBPushClickMessage | [TCGBPushMessage pushFromJsonString:message.data] | \- |
| Push - Action clicked | kTCGBPushClickAction | [TCGBPushMessage pushFromJsonString:message.data] | Operates when the RichMessage button is clicked. |

#### IdP Revoked

> [Note]
>
> This is an event that only occur when using the iOS Appleid login.

* This event occurs when the service is deleted from the IdP.
* Notifies the user that the IdP has been revoked, and issues a new userID when the user logs in with the same IdP.
* TCGBGamebaseEventIdPRevokedData.code: Indicates the TCGBIdPRevokedCode value.
    * IDP_REVOKED_WITHDRAW: 600
        * Indicates that the user is logged in with a revoked IdP, and there is no list of mapped IdPs.
        * You need to call the Withdraw API to remove the current account.
    * IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING: 601
        * Indicates that the user is logged in with a revoked IdP and IdPs other than the revoked IdP are mapped.
        * You need to log in with one of the mapped IdPs and call the removeMapping API to remove mapping with the revoked IdP.
    * IDP_REVOKED_REMOVE_MAPPING: 602
        * Indicates that there is a revoked IdP among IdPs mapped to the current account.
        * You need to call the removeMapping API to remove mapping with the revoked IdP.
* TCGBGamebaseEventIdPRevokedData.idpType: Indicates the revoked IdP type.
* TCGBGamebaseEventIdPRevokedData.authMappingList: Indicates the list of IdPs mapped to the current account.

```objectivec
@interface TCGBGamebaseEventIdPRevokedData : NSObject <TCGBValueObject>

@property (nonatomic, assign) int64_t                 code;
@property (nonatomic, strong) NSString*               idPType;
@property (nonatomic, strong) NSArray<NSString *>*    authMappingList;
@property (nonatomic, strong) NSString*               extras;

@end
```

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        if ([message.category isEqualToString:kTCGBIdPRevoked] == YES) {
            TCGBGamebaseEventIdPRevokedData *idPRevokedData = [TCGBGamebaseEventIdPRevokedData gamebaseEventIdPRevokedDataFromJsonString:message.data];
            if (idPRevokedData == nil) { return; }   

            NSString *revokedIdP = idPRevokedData.idPType;
            switch (idPRevokedData.code) {
                case IDP_REVOKED_WITHDRAW:
                {
                    // The user is logged in with a revoked IdP, and there is no list of mapped IdPs.
                    // Notifies the user that the current account has been removed.
                    [TCGBGamebase withdrawWithViewController:nil completion:^(TCGBError *error) {
                        ...
                    }];
                    break;
                }   
                case IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING:
                {
                    // The user is logged in with a revoked IdP and IdPs other than the revoked IdP are mapped.
                    // Allows the user to select a IdP to log in to among the authMappingList, and remove mapping with the revoked IdP after login with the selected IdP.
                    NSString *selectedIdPType = "The IdP selected by the user";
                    NSMutableDictionary *additionalInfo = [NSMutableDictionary dictionary];
                    additionalInfo[kTCGBAuthLoginWithCredentialIgnoreAlreadyLoggedInKeyname] = @(YES);
                    [TCGBGamebase loginWithType:selectedIdPType additionalInfo:additionalInfo viewController:viewController completion:^(TCGBAuthToken *authToken, TCGBError *loginError) {
                        if ([TCGBGamebase isSuccessWithError:loginError]) {
                            [TCGBGamebase removeMappingWithType:revokedIdP viewController:nil completion:^(TCGBError * _Nullable removeMappingError) {
                                ...
                            }];
                        }
                    }];
                    break;
                }
                case IDP_REVOKED_REMOVE_MAPPING:
                {
                    // There is a revoked IdP among IdPs mapped to the current account.
                    // Notifies the user that mapping with the revoked IdP is removed from the current account.
                    [TCGBGamebase removeMappingWithType:revokedIdP viewController:nil completion:^(TCGBError *error) {
                        ...
                    }];   
                    break;
                }
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

#### Logged Out

* This event occurs when the Gamebase Access Token has expired and a login function call is required to recover the network session.

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        [self printLogAndShowAlertWithData:[message prettyJsonString] error:nil alertTitle:@"addEventHandler Result"];
        if ([message.category isEqualToString:kTCGBLoggedOut] == YES) {
            TCGBGamebaseEventLoggedOutData* loggedOutData = [TCGBGamebaseEventLoggedOutData gamebaseEventLoggedOutDataFromJsonString:message.data];
            if (loggedOutData != nil) {
                //TODO: process loggedOut
            }
        }
    };
    
    [TCGBGamebase addEventHandler:eventHandler];
}
```

#### Server Push

* This is a message sent from the Gamebase server to the client's device.
* The Server Push Types supported from Gamebase are as follows:
	* kTCGBServerPushAppKickoutMessageReceived
    	* If you register a kickout ServerPush message in **Operation > Kickout** in the NHN Cloud Gamebase console, all clients connected to Gamebase will receive a kickout message.
        * This event occurs immediately after receiving a server message from the client device.
        * It can be used to pause the game when the game is running, as in the case of 'Auto Play'.
	* kTCGBServerPushAppKickout
    	* If you register a kickout ServerPush message in **Operation > Kickout** of the NHN Cloud Gamebase Console, then all clients connected to Gamebase will receive the kickout message.
        * A pop-up is displayed when the client device receives a server message. This event occurs when the user closes this pop-up.
    * kTCGBServerPushTransferKickout
    	* If the guest account is successfully transferred to another device, the previous device receives a kickout message.

**Example**

```objectivec
- (void)eventHandler_addEventHandler {
    void(^eventHandler)(TCGBGamebaseEventMessage *) = ^(TCGBGamebaseEventMessage * _Nonnull message) {
        [self printLogAndShowAlertWithData:[message prettyJsonString] error:nil alertTitle:@"addEventHandler Result"];
        if ([message.category isEqualToString:kTCGBServerPushAppKickoutMessageReceived] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push 
            }
        } else if ([message.category isEqualToString:kTCGBServerPushAppKickout] == YES) {
            TCGBGamebaseEventServerPushData* serverPushData = [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data];
            if (serverPushData != nil) {
                //TODO: process server push
            }
        } else if ([message.category isEqualToString:kTCGBServerPushTransferKickout] == YES) {
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

@end
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

#### Push Received Message


* This event occurs when a push message is received.
* By converting the extras field to JSON, you can also get custom information sent along with the push message.

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

* This event is triggered when a received push message is clicked.

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
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/en/Contact%20Center/en/online-contact-overview/)
>

#### Customer Service Type

In the **Gamebase Console > App > Customer service**, you can choose from three different types of Customer Centers.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △              |
| NHN Cloud Online Contact      | △              |

Gamebase SDK's Customer Center API uses the following URLs based on the type:

* Developer's Customer Center
    * URL specified in the **Customer Center URL** field.
* Gamebase's Customer Center
    * Before login: Customer Center URL **without** user information.
    * After login: Customer Center URL with user information.
* NHN Cloud organization product (Online Contact)
    * Before login: Customer Center URL **without** user information.
    * After login: Customer Center URL with user information.

#### Open Contact WebView

This feature is used to represent the **Customer Center URL** WebView entered in the Gamebase Console.
You can pass the additional information to the URL using TCGBContactConfiguration.


**TCGBContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | User name (nickname)<br>**default**: nil    |
| additionalURL | O             | string                             | Additional URL appended to the developer's own customer center URL<br>Use it only if the customer center type is `CUSTOM`<br>**default**: nil    |
| additionalParameters | O      | dictionary&lt;string, string&gt;         | Additional parameters appended to the contact center URL<br>**default**: nil    |
| extraData     | O             | dictionary&lt;string, string&gt;        | Passes the extra data wanted by the developer when opening the customer center<br>**default**: nil    |


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
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebase not initialized. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | Failed to issue a temporary ticket for user identification. |

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

> <font color="red">[Caution]</font><br/>
>
> When contacting the Customer Center, access to camera or album may be required for file attachment.
> Please set 'Privacy - Camera Usage Description', 'Privacy - Microphone Library Usage Description' in info.plist.

#### Request Contact URL

Can get the URL used for displaying the Customer Center WebView.

**API**

```objectivec
+ (void)requestContactURLWithCompletion:(void(^)(NSString *contactUrl, TCGBError *error))completion;

+ (void)requestContactURLWithConfiguration:(TCGBContactConfiguration *)configuration
                                completion:(void(^)(NSString *contactUrl, TCGBError *error))completion;
```

**Error Code**

| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebase not initialized. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | The Customer Center URL does not exist.<br>Check the **Customer Center URL** of the Gamebase Console. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | Failed to issue a temporary ticket for user identification. |

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

