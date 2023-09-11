## Game > Gamebase > iOS SDK ご利用ガイド > ETC

## Additional Features
Gamebaseで対応している付加機能について説明します。

### IDFA

* 端末の広告識別子値をリターンします。

**API**

```objectivec
+ (NSString *)idfa;
```

> <font color="red">[注意]</font><br/>
>
> iOS 14から、IDFA値をリクエストする時、ユーザー権限を取得する必要があります。
> ユーザー権限をリクエストする時に表示させる文言をinfo.plistに設定する必要があります。
> info.plistに「Privacy - Tracking Usage Description」の設定を行ってください。




### Device Language

* 端末に設定されている言語コードを返します。
* 複数の言語が登録されている場合、優先権が最も高い言語だけを返します。

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language

メンテナンスポップアップなどでGamebaseが表示する言語は、端末に設定された言語と同じです。

しかしゲームで表示する言語を、端末に設定した言語ではなく、別のオプションで変更できるゲームがあります。
例えば、端末に設定された言語は英語ですが、ゲーム表示言語を日本語に変更した場合、 Gamebaseで表示する言語も日本語に変更したいですが、Gamebaseが表示する言語は端末に設定された言語の英語が表示されます。

このように`端末に設定された言語ではなく、他の言語でGamebaseのメッセージを表示したい`アプリケーションのためにGamebaseは`Display Language`という機能を提供します。

GamebaseはDisplay Languageで設定した言語でGamebaseのメッセージを表示します。
Display Languageに入力する言語コードは、以下の表(**Gamebaseでサポートする言語コードの種類**)で指定したコードだけを使用できます。

> <font color="red">[注意]</font><br/>
>
> * Display Languageは、端末設定言語と関係なくGamebaseの表示言語を変更したい場合にのみ使用してください。
> * Display Language CodeはISO-639形式の値で、大文字/小文字を区別します。
> 'EN'または'zh-cn'と設定すると問題が発生する場合があります。
> * もしDisplay Language Codeに入力した値が以下の表(**Gamebaseでサポートする言語コードの種類**)に存在しない場合、Display Langauge CodeはGamebaseコンソールで設定したデフォルト言語に指定されます。
>   * もしGamebaseコンソールで言語設定を行っていなければ英語(en)がデフォルト言語に設定されます。

> [Note]
>
> * Gamebaseのクライアントメッセージは英語(en)、韓国語(ko)、日本語(ja)のみ含んでいるため、下記の表に存在する言語コードであっても、英語(en)、韓国語(ko)、日本語(ja)以外の言語を指定するとデフォルト値の英語(en)に自動設定されます。
> * Gamebaseのクライアントに含まれていない言語セットは直接追加できます。
> **新規言語セット追加**項目を参照してください。


#### Gamebaseでサポートしている言語コードの種類
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

該当する言語コードは、「TCGBConstants.h」に定義されています。


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


#### Gamebaseを初期化する際のDisplay Languageの設定

Gamebaseを初期化する際にDisplay Languageを設定することができます。

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

Gamebaseを初期化する際に入力されたDisplay Languageを変更することができます。

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

現在適用されているDisplay Languageを照会することができます。

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

#### 言語セットの新規追加

Gamebaseで提供するデフォルト言語(ko、en、ja、zh-CN、zh-TW、th)以外の言語を使用したい場合は、 Xcodeプロジェクトの`Copy Bundle Resources`に**localizedstring.json**ファイルを追加してください。

localizedstring.jsonに定義されている形式は、次の通りです。

```json
{
  "en":{
    "common_ok_button":"OK",
    "common_cancel_button":"Cancel",
    ...
    "launching_service_closed_title":"Service Closed"
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

他の言語の追加が必要な場合は、localizedstring.jsonファイルの該当言語コードに`"key":"value"`形式で値を追加してください。

```json
{
  "en":{
    "common_ok_button":"OK",
    "common_cancel_button":"Cancel",
    ...
    "launching_service_closed_title":"Service Closed"
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

#### Display Languageの優先順位

初期化及びsetDisplayLanguageCode:APIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。

1. 入力されたlanguageCodeがlocalizedstring.jsonファイルに定義されているかどうかを確認します。
2. 1番が失敗した場合は、Gamebase初期化時、端末に設定された言語コードがlocalizedstring.jsonファイルに定義されているかを確認します。(この値は初期化後、端末に設定された言語を変更しても維持されます。)
3. 2番が失敗した場合は、Gamebaseコンソールに設定されたデフォルト言語が設定されます。
4. Gamebaseコンソールに言語設定がなければ、`en`がデフォルト値に設定されます。

### Country Code

* GamebaseはSystemのCountry Codeを次のAPIで提供しています。

#### Device Country Code

* OSから伝達された端末地域設定を、追加確認を行わずにそのまま返します。
* 端末国コードは**設定 > 一般 > 言語および地域 > 地域**設定に応じてOSが自動的に決定します。
* iOSで提供するNSLocaleCountryCodeを使用して取得した値を返します。

**API**


```objectivec
+ (NSString *)deviceCountryCode;
```

### Gamebase Event Handler

* Gamebaseは各種イベントを**GamebaseEventHandler**という1つのイベントシステムで全て処理できます。
* GamebaseEventHandlerは下記のAPIを利用して簡単にHandlerを追加/削除できます。

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
        if ([message.category isEqualToString:kTCGBLoggedOut] == YES) {
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

* CategoryはGamebaseEventCategoryクラスに定義されています。
* イベントは大きくIdPRevoked、LoggedOut、ServerPush、Observer、Purchase、Pushに分けられ、各Categoryに基づいて、TCGBGamebaseEventMessage.dataを次の表のような方法でVOに変換できます。

| Event種類 | GamebaseEventCategory | VO変換方法 | 備考 |
| --------- | --------------------- | ----------- | --- |
| IdPRevoked | kTCGBIdPRevoked | [TCGBGamebaseEventIdPRevokedData gamebaseEventIdPRevokedDataFromJsonString:message.data] | \- |
| LoggedOut | kTCGBLoggedOut | [TCGBGamebaseEventLoggedOutData gamebaseEventLoggedOutDataFromJsonString:message.data] | \- |
| ServerPush | kTCGBServerPushAppKickoutMessageReceived<br>kTCGBServerPushAppKickout<br>kTCGBServerPushTransferKickout | [TCGBGamebaseEventServerPushData gamebaseEventServerPushDataFromJsonString:message.data] | \- |
| Observer | kTCGBObserverLaunching<br>kTCGBObserverHeartbeat<br>kTCGBObserverNetwork | [TCGBGamebaseEventObserverData gamebaseEventObserverDataFromJsonString:message.data] | \- |
| Purchase - プロモーション決済 | kTCGBPurchaseUpdated | [TCGBPurchasableReceipt purchasableReceiptFromJsonString:message.data] | \- |
| Push - メッセージ受信 | kTCGBPushReceivedMessage | [TCGBPushMessage pushMessageFromJsonString:message.data] | \- |
| Push - メッセージクリック | kTCGBPushClickMessage | [TCGBPushMessage pushFromJsonString:message.data] | \- |
| Push - アクションクリック | kTCGBPushClickAction | [TCGBPushMessage pushFromJsonString:message.data] | RichMessageボタンを押すと動作します。 |

#### IdP Revoked

* IdPで該当サービスを削除したときに発生するイベントです。
* ユーザーにIdPが使用停止したことを知らせ、同じIdPでログインするとき、userIDを新たに発行できるように実装する必要があります。
* TCGBGamebaseEventIdPRevokedData.code: TCGBIdPRevokedCode値を意味します。
    * IDP_REVOKED_WITHDRAW: 600
        * 現在使用停止しているIdPでログインしていて、マッピングされたIdPリストがないことを意味します。
        * withdraw APIを呼び出して現在のアカウントを退会させる必要があります。
    * IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING: 601
        * 現在使用停止しているIdPでログインしていて、使用停止しているIdP以外の他のIdPがマッピングされている場合を意味します。
        * マッピングされたIdPリストのうちの1つのIdPにログインし、removeMapping APIを呼び出して使用停止しているIdPの連動を解除する必要があります。
    * IDP_REVOKED_REMOVE_MAPPING: 602
        * 現在アカウントにマッピングされているIdPのうち、使用停止しているIdPがある場合を意味します。
        * removeMapping APIを呼び出して使用停止しているIdPの連動を解除する必要があります。
* TCGBGamebaseEventIdPRevokedData.idpType：使用停止したIdPタイプを意味します。
* TCGBGamebaseEventIdPRevokedData.authMappingList：現在アカウントにマッピングされているIdPリストを意味します。

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
                    // 現在使用停止しているIdPでログインしていて、マッピングされたIdPリストがないことを意味します。
                    // ユーザーに現在のアカウントが退会していることを伝えてください。
                    [TCGBGamebase withdrawWithViewController:nil completion:^(TCGBError *error) {
                        ...
                    }];
                    break;
                }   
                case IDP_REVOKED_OVERWRITE_LOGIN_AND_REMOVE_MAPPING:
                {
                    // 現在使用停止しているIdPでログインしていて、使用停止したIdP以外のIdPがマッピングされている場合を意味します。
                    // ユーザーがauthMappingListのうちどのIdPで再度ログインするか選択し、選択したIdPでログインした後、使用停止したIdPについては連動を解除してください。
                    NSString *selectedIdPType = "ユーザーが選択したIdP";
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
                    // 現在のアカウントにマッピングされているIdPのうち使用停止しているIdPがある場合を意味します。
                    // ユーザーに現在のアカウントで使用停止しているIdPが連動解除されたことを伝えてください。
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

* Gamebase Access Tokenの有効期限が切れてネットワークセッションを復元するためにログイン関数の呼び出しが必要な場合に発生するイベントです。

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

* Gamebaseサーバーからクライアント端末へ送信するメッセージです。
* GamebaseでサポートするServer Push Typeは次の通りです。
	* kTCGBServerPushAppKickoutMessageReceived
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseと接続したすべてのクライアントでキックアウトメッセージを受け取ります。
        * クライアント端末でサーバーメッセージを受信した直後に発生するイベントです。
        * 「オートプレイ」のようにゲームが動作中の場合、ゲームを一時停止させる目的で活用できます。
	* kTCGBServerPushAppKickout
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受信します。
        * クライアント端末でサーバーメッセージを受信した時、ポップアップを表示しますが、ユーザーがポップアップを閉じたときに発生するイベントです。
    * kTCGBServerPushTransferKickout
    	* Guestアカウントを他の端末へ移行すると、以前の端末でキックアウトメッセージを受信します。

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

* Gamebase Gamebaseの各種状態変動イベントを処理するシステムです。
* GamebaseでサポートするObserver Typeは次の通りです。
    * kTCGBObserverLaunching
    	* メンテナンスが開始または終了した場合、新しいバージョンが配布されてアップデートが必要な場合など、Launching状態が変更された時に動作します。
    	* TCGBGamebaseEventObserverData.code : TCGBLaunchingStatus値を意味します。
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
    	* 退会処理や利用停止により、ユーザーアカウントの状態が変わった時に動作します。
    	* TCGBGamebaseEventObserverData.code : TCGBError値を意味します。
            * TCGB_ERROR_INVALID_MEMBER: 6
            * TCGB_ERROR_BANNED_MEMBER: 7
    * kTCGBObserverNetwork
    	* ネットワーク変動事項情報を受け取れます。
    	* ネットワークが切断されたり、接続された時、またはWi-FiからCellularネットワークに変更された時に動作します。
    	* TCGBGamebaseEventObserverData.code : NetworkManager値を意味します。
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

'@end
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

* Promotionコードを入力して商品を獲得した場合に発生するイベントです。
* 決済領収書情報を取得できます。

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


* Pushメッセージが到着した時に発生するイベントです。
* extrasフィールドをJSONに変換して、Push送信時に送信されたカスタム情報を取得することもできます。

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

* 受信したPushメッセージをクリックした時に発生するイベントです。

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

* Rich Message機能を利用して作成したボタンをクリックした時に発生するイベントです。
* actionTypeは、次の項目が提供されます。
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

ゲーム指標をGamebaseサーバーに伝送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIは、ログイン後に呼び出すことができます。

> [TIP]
>
> TCGBPurchaseのrequestPurchaseWithItemSeq:viewController:completion APIを呼び出して決済するか、setPromotionIAPHandlerを呼び出してプロモーション決済を完了すると、自動的に指標を伝送します。

Analyticsコンソールの使用方法は、下記のガイドを参照してください。

- [Analyticsコンソール](./oper-analytics)

#### Game User Data Settings

ゲームログイン後、ゲームユーザーレベル情報を指標として伝送できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後にsetGameUserData APIを呼び出さない場合、他の指標でレベル情報が抜ける場合があります。
>

APIの呼び出しに必要なパラメータは下記の通りです。

> ゲームログイン後にsetGameUserData APIを呼び出さない場合、他の指標でレベル情報が抜ける場合があります。

**GameUserData**

| Name | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| channelId | O | String | チャンネルを表すフィールドです。 |
| characterId | O | String | キャラクター名を表すフィールドです。 |
| classId | O | String | 職業を表すフィールドです。 |


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

レベルアップすると、ゲームユーザーレベル情報を指標として伝送できます。

APIの呼び出しに必要なパラメータは下記の通りです。

**LevelUpData**

| Name | Mandatory (M) / Optional (O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
| levelUpTime | M | long | Epoch timeで入力します。</br>ミリ秒(ms)単位で入力します。 |

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

Gamebaseでは顧客からの問い合わせに対応するための機能を提供します。

> [TIP]
>
> NHN Cloud Contactサービスと連動して使用すると、より簡単に顧客からのお問い合わせに対応できます。
> 詳細なNHN Cloud Contactサービスの利用方法は以下のガイドを参照してください。
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ja/Contact%20Center/ja/online-contact-overview/)

#### Customer Service Type

**Gamebaseコンソール > App > InApp URL > Service center** では、以下のように3つのタイプのサポートを選択できます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △              |
| NHN Cloud Online Contact      | △              |

各タイプに応じて、Gamebase SDKのサポートAPIは次のURLを使用します。

* 開発会社独自のサポート(Developer customer center)
    * **サポートURL**に入力したURL.
* Gamebase提供のサポート(Gamebase customer center)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。
* NHN Cloud組織商品(Online Contact)
    * ログイン前：ユーザー情報が**ない**サポートURL.
    * ログイン後：ユーザー情報が含まれたサポートURL。

#### Open Contact WebView

Gamebaseコンソールに入力した**サポートURL** Webビューを表示できる機能です。
TCGBContactConfigurationでURLに追加情報を伝達できます。


**TCGBContactConfiguration**

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| userName      | O             | string                             | ユーザー名前(ニックネーム)<br>**default** : nil    |
| additionalURL | O             | string                             | 開発会社独自のサポートURLの後ろにつく追加のURL<br>サポートタイプが`CUSTOM`の場合にのみ使用<br>**default** : nil    |
| additionalParameters | O      | dictionary&lt;string, string&gt;         | サポートURLの後ろにつく追加のパラメータ<br>**default** : nil    |
| extraData     | O             | dictionary&lt;string, string&gt;         | 開発会社が任意のextra dataをサポートオープン時に伝達<br>**default** : nil    |


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
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | ユーザーを識別するための臨時チケットの発行に失敗しました。 |

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
> サポートへのお問い合わせの際、ファイルを添付するために、カメラまたはアルバムへのアクセスが必要な場合があります。
> info.plistに'Privacy - Camera Usage Description'、'Privacy - Photo Library Usage Description'、'Privacy - Microphone Usage Description' の設定を行ってください。

#### Request Contact URL

サポートWebビューを表示するのに使用されるURLを取得できます。

**API**

```objectivec
+ (void)requestContactURLWithCompletion:(void(^)(NSString *contactUrl, TCGBError *error))completion;

+ (void)requestContactURLWithConfiguration:(TCGBContactConfiguration *)configuration
                                completion:(void(^)(NSString *contactUrl, TCGBError *error))completion;
```

**Error Code**

| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1       | Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | ユーザーを識別するための臨時チケットの発行に失敗しました。 |

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
