## Game > Gamebase > iOS SDK 사용 가이드 > ETC

## Additional Features
Gamebase에서 지원하는 부가적인 기능을 설명합니다.


### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language
* Gamebase에서 표시하는 언어를 기기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.
* Gamebase는 클라이언트에 포함되어 있는 메시지를 표시하거나 서버에서 받은 메시지를 표시합니다.
* DisplayLanguage를 설정하게 되면 사용자가 설정한 언어코드(ISO-639)에 적합한 언어로 메시지를 표시합니다.
* 필요하다면 사용자가 지원하고 싶은 언어셋을 추가할 수 있습니다. (하단 지원 언어코드를 참고)

> [참고]
>
> Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함합니다.

#### Gamebase에서 지원하고 있는 언어코드의 종류
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

해당 언어코드는 `TCGBConstants.h`에 정의되어 있습니다.

> `[주의]`
>
> Gamebase에서 지원하고 있는 언어코드는 대소문자를 구분합니다.
> "EN" 이나 "zh-cn"과 같이 설정할 경우 문제가 발생할 수 있습니다.

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


#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

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

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

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

현재 적용된 Display Language를 조회할 수 있습니다.

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

#### 신규 언어셋 추가

Gamebase에서 제공하는 기본 언어(ko, en) 외 다른 언어를 사용하는 경우에는 Gamebase.bundle 파일의 Resource폴더에 있는 **localizedstring.json** 파일에 값을 추가합니다.

localizedstring.json에 정의되어 있는 형식은 아래와 같습니다.

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

다른 언어셋을 추가해야 할 경우에는 localizedstring.json 파일에 `"${언어 코드}":{"key":"value"}` 형태로 값을 추가하면 됩니다.

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

위 JSON 형식에서 "${언어코드}":{ } 내부에 key가 누락될 경우에는 `기기에 설정된 언어` 또는 `en`이 자동으로 입력됩니다.

#### Display Language 우선 순위

초기화 및 setDisplayLanguageCode: API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedstring.json 파일에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 기기에 설정된 언어코드가 localizedstring.json 파일에 정의되어 있는지 확인합니다. (이 값은 초기화 이후, 기기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동 설정됩니다.


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
* Gamebase 서버에서 클라이언트 기기로 보내는 Server Push Message를 처리할 수 있습니다.
* Gamebase 클라이언트에서 ServerPushEvent를 추가 하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent를 삭제 할 수 있습니다.


#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.

* kTCGBServerPushNotificationTypeAppKickout (= "appKickout")
    * TOAST Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 **APP_KICKOUT** 메시지를 받게 됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Gamebase Client에 ServerPushEvent를 등록하여 Gamebase Console 및 Gamebase 서버에서 발급된 Push 이벤트를 처리할 수 있습니다.

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
아래의 API들을 사용하여 Gamebase에 등록된 ServerPushEvent를 삭제할 수 있습니다.

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
* Gamebase Observer를 통하여 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* 상태 변동 이벤트 : 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등



#### Observer Type
현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.

* Network 타입 변동
    * 네트워크 변동사항에 대한 정보를 받을 수 있습니다. 예를 들어서, message.data[@"code"] 의 값으로 Network Type을 알 수 있습니다.
    * Type : kTCGBObserverMessageTypeNetwork (= @"network")
    * Code : NetworkStatus에 선언된 상수를 참고합니다. 
        * NotReachable : -1
        * ReachableViaWWAN : 0
        * ReachableViaWifi : 1        
        * ReachabilityIsNotDefined : -100
* Launching 상태 변동
    * 주기적으로 어플리케이션의 상태를 체크하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어서, 점검, 업데이트 권장 등에 의한 이벤트가 있습니다.
    * Type : kTCGBObserverMessageTypeLaunching (= @"launching")
    * Code : TCGBLaunchingStatus 선언된 상수를 참고합니다.
        * IN_SERVICE : 200
        * RECOMMEND_UPDATE : 201
        * IN_SERVICE_BY_QA_WHITE_LIST : 202
        * REQUIRE_UPDATE : 300
        * BLOCKED_USER : 301
        * TERMINATED_SERVICE : 302
        * INSPECTING_SERVICE : 303
        * INSPECTING_ALL_SERVICES : 304
        * INTERNAL_SERVER_ERROR : 500
* Heartbeat 정보 변동
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어서, 사용자 이용 정지에 의한 이벤트가 있습니다.
    * Type : ObserverkTCGBObserverMessageTypeHeartbeat (= @"heartbeat")
    * Code : TCGBErrorCode 선언된 상수를 참조합니다.
        * TCGB_ERROR_INVALID_MEMBER : 6
        * TCGB_ERROR_BANNED_MEMBER : 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase Client에 Observer를 등록하여 각종 상태 변동 이벤트를 처리할 수 있습니다.

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
아래의 API들을 사용하여 Gamebase에 등록된 Observer를 삭제할 수 있습니다.

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

Game지표를 Gamebase Server로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase Analytics에서 지원하는 모든 API는 로그인 후에 호출할 수 있습니다.

> [TIP]
>
> TCGBPurchase의 requestPurchaseWithItemSeq:viewController:completion API의 호출을 통한 결제 또는 setPromotionIAPHandler를 통한 프로모션 결제를 완료하면 자동으로 지표를 전송합니다.

Analytics Console 사용법은 아래 가이드를 참고하십시오.

- [Analytics Console](./oper-analytics)

#### Game User Data Settings

게임 로그인 이후 유저 레벨 정보를 지표로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 SetGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**GameUserData**

  | Name | Mandatory(M) / Optional(O) | type | Desc |
  | -------------------------- | -------------------------- | ---- | ---- |
  | userLevel | M | int | |
  | channelId | O | string | |
  | characterId | O | string | |

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

레벨업이 되었을 경우 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

  | Name | Mandatory(M) / Optional(O) | type | Desc |
  | -------------------------- | -------------------------- | ---- | ---- |
  | userLevel | M | int | |
  | levelUpTime | O | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |
  | channelId | O | string | |
  | characterId | O | string | |

**API**

```objectivec
+ (void)traceLevelUpWithLevelUpData:(nonnull TCGBAnalyticsLevelUpData *)levelUpData;
```

**Example**

```objectivec
- (void)traceLevelUpWith:(int)level levelUpTime:(long long)levelUpTime channelId:(NSString *)channelId characterId:(NSString *)characterId {
  TCGBAnalyticsLevelUpData* levelUpData = [TCGBAnalyticsLevelUpData levelUpDataWithUserLevel:level];
  [levelUpData setLevelUpTime:levelUpTime];
  [levelUpData setChannelId:channelId];
  [levelUpData setCharacterId:characterId];
  [TCGBAnalytics traceLevelUpWithLevelUpData:levelUpData];
}
```
