## Game > Gamebase > iOS SDK 사용 가이드 > ETC

## Additional Features
Gamebase에서 지원하는 부가적인 기능을 설명합니다.

### IDFA

* 단말기의 광고식별자 값을 리턴합니다.

**API**

```objectivec
+ (NSString *)idfa;
```

> <font color="red">[주의]</font><br/>
>
> iOS 14 이상부터 IDFA 값 요청 시, 사용자 권한을 받아야합니다.
> 사용자 권한 요청할 때 노출시킬 문구를 info.plist에 설정을 해야합니다.
> info.plist에 'Privacy - Tracking Usage Description' 설정을 해주시기 바랍니다.




### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language

점검 팝업 창과 같이 Gamebase가 표시하는 언어는 단말기에 설정된 언어로 표시됩니다.

그런데 게임에서 표시하는 언어를 단말기에 설정된 언어가 아닌, 별도의 옵션으로 언어를 변경할 수 있는 게임이 있습니다.
예를 들어, 단말기에 설정된 언어는 영어 이지만 게임 표시 언어를 일본어로 변경한 경우, Gamebase에서 표시하는 언어도 일본어로 변경하고 싶지만 Gamebase가 표시하는 언어는 단말기에 설정된 언어인 영어로 표시됩니다.

이와 같이 `단말기에 설정된 언어가 아닌, 다른 언어로 Gamebase 메시지를 표시하고 싶은` 애플리케이션을 위해 Gamebase 는 `Display Language` 라는 기능을 제공합니다.

Gamebase는 Display Language로 설정한 언어로 Gamebase 메시지를 표시합니다.
Display Language에 입력하는 언어 코드는 반드시 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 지정된 코드만을 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * Display Language는 단말기 설정 언어와 무관하게 Gamebase의 표시 언어를 변경하고 싶은 경우에만 사용하시기 바랍니다.
> * Display Language Code는 ISO-639 형태의 값으로, 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
> * 만일 Display Language Code로 입력한 값이 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 존재하지 않는다면, Display Langauge Code는 Gamebase 콘솔에서 설정한 기본 언어로 지정됩니다.
>   * 만일 Gamebase 콘솔에서 언어 설정을 하지 않았다면 영어(en)가 기본 언어로 설정됩니다.

> [참고]
>
> * Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함하고 있으므로 아래의 표에 존재하는 언어 코드라 할지라도 영어(en), 한글(ko), 일본어(ja) 이외의 언어를 지정하면 기본값인 영어(en)로 자동 설정됩니다.
> * Gamebase의 클라이언트에 포함되어 있지 않은 언어셋은 직접 추가할 수 있습니다.
> **신규 언어셋 추가** 항목을 참조하시기 바랍니다.


#### Gamebase에서 지원하고 있는 언어코드의 종류
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

해당 언어코드는 `TCGBConstants.h`에 정의되어 있습니다.


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

Gamebase에서 제공하는 기본 언어(ko, en, ja, zh-CN, zh-TW, th) 외 다른 언어를 사용하는 경우에는 Xcode 프로젝트의 `Copy Bundle Resources`에 **localizedstring.json** 파일을 추가하면 됩니다.

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

다른 언어셋을 추가해야 할 경우에는 localizedstring.json 파일의 해당 언어코드에 `"key":"value"` 형태로 값을 추가하면 됩니다.

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

#### Display Language 우선 순위

초기화 및 setDisplayLanguageCode: API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedstring.json 파일에 정의되어 있는지 확인합니다.
2. 1번이 실패했다면 Gamebase 초기화 시, 단말기에 설정된 언어코드가 localizedstring.json 파일에 정의되어 있는지 확인합니다.(이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. 2번이 실패했다면 Gamebase 콘솔에 설정된 기본 언어가 설정됩니다.
4. Gamebase 콘솔에 언어 설정이 없다면 `en`이 기본값으로 설정됩니다.

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

* OS로부터 전달받은 단말기 지역 설정을 추가적인 체크 없이 그대로 리턴합니다.
* 단말기 국가코드는 '설정 > 일반 > 언어 및 지역 > 지역' 설정에 따라 OS가 결정합니다.
* iOS에서 제공하는 NSLocaleCountryCode를 사용하여 획득한 값을 리턴합니다.

**API**

```objectivec
+ (NSString *)deviceCountryCode;
```

#### Intergrated Country Code

* USIM, 단말기 지역 설정의 순서로 국가 코드를 확인하여 리턴합니다.
* country API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ'를 리턴합니다.

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```objectivec
+ (NSString *)countryCode;
```


### Gamebase Event Handler

* Gamebase는 각종 이벤트를 **GamebaseEventHandler**라는 하나의 이벤트 시스템에서 모두 처리할 수 있습니다.
* GamebaseEventHandler는 아래 API를 통해 간단하게 Handler를 추가/제거할 수 있습니다.

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

* Category는 GamebaseEventCategory 클래스에 정의되어 있습니다.
* 이벤트는 크게 IdPRevoked, LoggedOut, ServerPush, Observer, Purchase, Push로 나눌 수 있고, 각 Category에 따라, TCGBGamebaseEventMessage.data를 아래 표와 같은 방법으로 VO로 변환할 수 있습니다.

| Event 종류 | GamebaseEventCategory | VO 변환 방법 | 비고 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | kTCGBIdPRevoked | [TCGBGamebaseEventIdPRevokedData gamebaseEventIdPRevokedDataFromJsonString:message.data] | \- |
| LoggedOut | kTCGBLoggedOut | [TCGBGamebaseEventLoggedOutData gamebaseEventLoggedOutDataFromJsonString:message.data] | \- |
| ServerPush | kTCGBServerPushAppKickoutMessageReceived<br>kTCGBServerPushAppKickout<br>kTCGBServerPushTransferKickout | [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data] | \- |
| Observer | kTCGBObserverLaunching<br>kTCGBObserverHeartbeat<br>kTCGBObserverNetwork | [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data] | \- |
| Purchase - 프로모션 결제 | kTCGBPurchaseUpdated | [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data] | \- |
| Push - 메시지 수신 | kTCGBPushReceivedMessage | [TCGBPushMessage pushMessageFromJsonString:message.data] | \- |
| Push - 메시지 클릭 | kTCGBPushClickMessage | [TCGBPushMessage pushFromJsonString:message.data] | \- |
| Push - 액션 클릭 | kTCGBPushClickAction | [TCGBPushMessage pushFromJsonString:message.data] | RichMessage 버튼 클릭 시 동작합니다. |

#### IdP Revoked

* IdP에서 해당 서비스를 삭제하였을 때 발생하는 이벤트입니다.
* 유저에게 IdP가 사용 중지되었음을 알려주고, 동일 IdP로 로그인할 때 userID를 새로 발급받을 수 있도록 구현해야 합니다.
* TCGBGamebaseEventIdPRevokedData.code: TCGBIdPRevokedCode 값을 의미합니다.
    * IDP_REVOKED_WITHDRAW: 600
        * 현재 사용 중지된 IdP로 로그인되어 있고, 매핑된 IdP 목록이 없을 때를 의미합니다.
        * withdraw API를 호출해서 현재 계정을 탈퇴시켜줘야 합니다.
    * IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING: 601
        * 현재 사용 중지된 IdP로 로그인되어 있고, 사용 중지된 IdP 외에 다른 IdP가 매핑되어 있는 경우를 의미합니다.
        * 매핑된 IdP 목록 중 하나의 IdP로 로그인을 하고 removeMapping API를 호출해서 사용 중지된 IdP에 대한 연동을 해제해야 합니다.
    * IDP_REVOKED_REMOVE_MAPPING: 602
        * 현재 계정에 매핑된 IdP 중 사용 중지된 IdP가 있을 경우를 의미합니다.
        * removeMapping API를 호출해서 사용 중지된 IdP에 대한 연동을 해제해야 합니다.
* TCGBGamebaseEventIdPRevokedData.idpType: 사용 중지된 IdP 타입을 의미합니다.
* TCGBGamebaseEventIdPRevokedData.authMappingList: 현재 계정에 매핑되어 있는 IdP 목록을 의미합니다.

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
                    // 현재 사용 중지된 IdP로 로그인되어 있고, 매핑된 IdP 목록이 없을 때를 의미합니다.
                    // 유저에게 현재 계정이 탈퇴됨을 알려주세요.
                    [TCGBGamebase withdrawWithViewController:nil completion:^(TCGBError *error) {
                        ...
                    }];
                    break;
                }   
                case IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING:
                {
                    // 현재 사용 중지된 IdP로 로그인되어 있고, 사용 중지된 IdP 외에 다른 IdP가 매핑되어 있는 경우를 의미합니다.
                    // 유저가 authMappingList 중 어떤 IdP로 다시 로그인할 지 선택하고, 선택된 IdP로 로그인한 후에 사용 중지된 IdP에 대해서는 연동 해제 시켜주세요.
                    NSString *selectedIdPType = "유저가 선택한 IdP";
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
                    // 현재 계정에 매핑된 IdP 중 사용 중지된 IdP가 있을 경우를 의미합니다.
                    // 유저에게 현재 계정에서 사용 중지된 IdP가 연동 해제됨을 알려주세요.
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

* Gamebase Access Token이 만료되어 네트워크 세션을 복구하기 위해 로그인 함수 호출이 필요한 경우 발생하는 이벤트입니다.

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

* Gamebase 서버에서 클라이언트 단말기로 보내는 메시지입니다.
* Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.
	* kTCGBServerPushAppKickoutMessageReceived
    	* NHN Cloud Gamebase 콘솔의 **Operation > Kickout**에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신한 직후에 발생하는 이벤트입니다.
        * '오토 플레이'와 같이 게임이 동작 중인 경우, 게임을 일시 정지시키는 목적으로 활용할 수 있습니다.
	* kTCGBServerPushAppKickout
    	* NHN Cloud Gamebase 콘솔의 **Operation > Kickout**에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신했을 때 팝업 창을 표시하는데, 유저가 이 팝업 창을 닫았을 때 발생하는 이벤트입니다.
    * kTCGBServerPushTransferKickout
    	* Guest 계정을 다른 단말기로 이전을 성공하게 되면 이전 단말기에서 킥아웃 메시지를 받게 됩니다.

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

* Gamebase의 각종 상태 변동 이벤트를 처리하는 시스템입니다.
* Gamebase에서 지원하는 Observer Type은 다음과 같습니다.
    * kTCGBObserverLaunching
    	* 점검이 걸리거나 풀린 경우, 새로운 버전이 배포되어 업데이트가 필요한 경우와 같이, Launching 상태가 변경되었을 때 동작합니다.
    	* TCGBGamebaseEventObserverData.code : TCGBLaunchingStatus 값을 의미합니다.
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
    	* 탈퇴 처리 되거나 이용 정지로 인하여 사용자 계정 상태가 변했을 때 동작합니다.
    	* TCGBGamebaseEventObserverData.code : TCGBError 값을 의미합니다.
            * TCGB_ERROR_INVALID_MEMBER: 6
            * TCGB_ERROR_BANNED_MEMBER: 7
    * kTCGBObserverNetwork
    	* 네트워크 변동 사항 정보를 받을 수 있습니다.
    	* 네트워크가 끊기거나 연결되었을 때, 혹은 Wifi 에서 셀룰러 네트워크로 변경되었을 때 동작합니다.
    	* TCGBGamebaseEventObserverData.code : NetworkManager 값을 의미합니다.
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

* Promotion 코드 입력을 통해 상품을 획득한 경우 발생하는 이벤트입니다.
* 결제 영수증 정보를 획득할 수 있습니다.

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


* Push 메시지가 도착했을때 발생하는 이벤트입니다.
* extras 필드를 JSON으로 변환하여, Push 발송 시 전송했던 커스텀 정보를 얻을 수도 있습니다.

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

* 수신한 Push 메시지를 클릭했을때 발생하는 이벤트입니다.

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

* Rich Message 기능을 통해 생성한 버튼을 클릭했을 때 발생하는 이벤트입니다.
* actionType은 다음 항목이 제공됩니다.
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

게임 로그인 이후 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 SetGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**GameUserData**

| Name | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 게임 유저 레벨을 나타내는 필드입니다. |
| channelId | O | String | 채널을 나타내는 필드입니다. |
| characterId | O | String | 캐릭터 이름을 나타내는 필드입니다. |
| classId | O | String | 직업을 나타내는 필드입니다. |


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

레벨업이 되었을 경우 게임 유저 레벨 정보를 지표로 전송할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name | Mandatory (M) / Optional (O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 게임 유저 레벨을 나타내는 필드입니다. |
| levelUpTime | M | long | Epoch time으로 입력합니다.</br>밀리초(ms) 단위로 입력합니다. |
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

Gamebase에서는 고객 문의 대응을 위한 기능을 제공합니다.

> [TIP]
>
> NHN Cloud Contact 서비스와 연동해서 사용하면 보다 쉽고 편리하게 고객 문의에 대응할 수 있습니다.
> 자세한 NHN Cloud Contact 서비스 이용법은 아래 가이드를 참고하시기 바랍니다.
> [NHN Cloud Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)
>

#### Customer Service Type

**Gamebase 콘솔 > App > InApp URL > Service center** 에서는 아래와 같이 3가지 유형의 고객 센터를 선택할 수 있습니다.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △              |
| NHN Cloud Online Contact      | △              |

각 유형에 따라 Gamebase SDK의 고객 센터 API는 다음 URL을 사용합니다.

* 개발사 자체 고객 센터(Developer customer center)
    * **고객 센터 URL**에 입력한 URL.
* Gamebase 제공 고객 센터(Gamebase customer center)
    * 로그인 전 : 유저 정보가 **없는** 고객 센터 URL.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.
* NHN Cloud 조직 상품(Online Contact)
    * 로그인 전 : 유저 정보가 **없는** 고객 센터 URL.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.

#### Open Contact WebView

Gamebase 콘솔에 입력한 **고객 센터 URL** 웹뷰를 나타낼 수 있는 기능입니다.
TCGBContactConfiguration으로 URL에 추가 정보를 전달할 수 있습니다.


**TCGBContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | 사용자 이름(닉네임)<br>**default**: nil    |
| additionalURL | O             | string                             | 개발사 자체 고객 센터 URL 뒤에 붙는 추가적인 URL<br>고객 센터 타입이 `CUSTOM` 인 경우에만 사용<br>**default**: nil    |
| additionalParameters | O      | dictionary&lt;string, string&gt;         | 고객 센터 URL 뒤에 붙는 추가적인 파라미터<br>**default**: nil    |
| extraData     | O             | dictionary&lt;string, string&gt;         | 개발사가 원하는 extra data를 고객 센터 오픈 시에 전달<br>**default**: nil    |


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
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 고객 센터 URL이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL**을 확인하세요. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 사용자 식별을 위한 임시 티켓 발급에 실패하였습니다. |

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

> <font color="red">[주의]</font><br/>
>
> 고객 센터 문의 시 파일 첨부를 위해 카메라 또는 앨범 접근이 필요할 수 있습니다.
> info.plist에 'Privacy - Camera Usage Description', 'Privacy - Photo Library Usage Description' 설정을 해주시기 바랍니다.

#### Request Contact URL

고객 센터 웹뷰를 표시하는데 사용되는 URL을 얻을 수 있습니다.

**API**

```objectivec
+ (void)requestContactURLWithCompletion:(void(^)(NSString *contactUrl, TCGBError *error))completion;

+ (void)requestContactURLWithConfiguration:(TCGBContactConfiguration *)configuration
                                completion:(void(^)(NSString *contactUrl, TCGBError *error))completion;
```

**Error Code**

| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 고객 센터 URL이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL**을 확인하세요. |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 사용자 식별을 위한 임시 티켓 발급에 실패하였습니다. |

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

