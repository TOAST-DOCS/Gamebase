## Game > Gamebase > iOS SDK ご利用ガイド > ETC

## Additional Features
Gamebaseで対応している付加機能について説明します。


### Device Language

* 端末に設定されている言語コードを返します。
* 複数の言語が登録されている場合、優先権が最も高い言語だけを返します。

**API**

```objectivec
+ (NSString *)deviceLanguageCode;
```


### Display Language
* Gamebaseに表示される言語をデバイスに設定されている言語ではなく、他の言語に変更することができます。
* Gamebaseは、クライアントに含まれているメッセージを表示したり、サーバーから取得したメッセージを表示します。
* DisplayLanguageを設定すると、ユーザーが設定した言語コード(ISO-639)に対応する言語でメッセージを表示します。
* 必要な場合、ユーザーがサポートしたい言語セットを追加することができます。(下の対応言語コードを参考)

> [参考]
>
> Gamebaseのクライアントメッセージには、英語(en)、韓国語(ko)、日本語(ja)のみ含まれます。

#### Gamebaseでサポートしている言語コードの種類
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

該当する言語コードは、「TCGBConstants.h」に定義されています。

> `[注意]`
>
> Gamebaseでサポートしている言語コードは、大文字・小文字を区別します。
> “EN”や"zh-cn"のように設定する場合、問題が発生することがあります。

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

Gamebaseで提供するデフォルト言語(ko、en、ja)以外に他の言語を使用する場合は、Gamebase.bundle ファイルのResourceフォルダにある**localizedstring.json**ファイルに値を追加します。

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
}
```

他の言語の追加が必要な場合、localizedstring.jsonファイルに`"${言語コード}":{"key":"value"}`の形で値を追加してください。

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
  "ja":{
    "common_ok_button":"確認",
    "common_cancel_button":"キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
  "${言語コード}": {
      "common_ok_button": "...",
      ...
  }
}
```

上記のjson形式で"${言語コード}":{ }内部にkeyが抜けている場合、「デバイスに設定されている言語」または`en`で自動入力されます。

#### Display Languageの優先順位

初期化及びsetDisplayLanguageCode:APIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。

1. 入力されたlanguageCodeがlocalizedstring.jsonファイルに定義されているかどうかを確認します。
2. Gamebaseを初期化する際に、デバイスに設定されている言語コードがlocalizedstring.jsonファイルに定義されているかどうかを確認します。(この値は、初期化後にデバイスに設定されている言語を変更した場合でも維持されます。)
3. Display Languageのデフォルト値である`en`が自動で設定されます。


### Country Code

* Gamebaseは、システムの国コード(country code)を次のようなAPIで提供しています。
* APIごとに特徴があるため、使用用途に合ったAPIを選択してください。

#### USIM Country Code

* USIMに記録された国コードを返します。
* USIMに無効な国コードが記録されていても、追加確認を行わずにそのまま返します。
* 値が空の場合、'ZZ'を返します。

**API**

```objectivec
+ (NSString *)usimCountryCode;
```

#### Device Country Code

* OSから伝達された端末地域設定を、追加確認を行わずにそのまま返します。
* 端末国コードは**設定 > 一般 > 言語および地域 > 地域**設定に応じてOSが自動的に決定します。
* iOSで提供するNSLocaleCountryCodeを使用して取得した値を返します。

**API**


```objectivec
+ (NSString *)deviceCountryCode;
```

#### Intergrated Country Code

* USIM、端末地域設定の順序で国コードを確認して返します。
* country APIは次の順序で動作します。
	1. USIMに記録されている国コードを確認し、値があれば別途確認しないでそのまま返します。
 2. USIM国コードが空の値の場合は端末国コードを確認し、値があれば追加確認を行わずにそのまま返します。
	3. USIM、端末国コードがどちらも空の値の場合は、'ZZ'を返します。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```objectivec
+ (NSString *)countryCode;
```



### Server Push
* Gamebaseサーバーからクライアント端末に送るServer Push Messageを処理できます。
* GamebaseクライアントでServerPushEventを追加すると、該当メッセージをユーザーが受信して処理でき、追加されたServerPushEventを削除できます。


#### Server Push Type
現在GamebaseでサポートするServer Push Typeは次の通りです。

* kTCGBServerPushNotificationTypeAppKickout (= "appKickout")
    * NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseと接続されたすべてのクライアントで**APP_KICKOUT**メッセージを受け取ることになります。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
GamebaseクライアントにServerPushEventを登録し、GamebaseコンソールおよびGamebaseサーバーで発行されたPushイベントを処理できます。

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
下記のAPIを使用し、Gamebaseに登録されたServerPushEventを削除できます。

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
* Gamebase Observerを通して、Gamebaseの各種状態変動イベントを受け取って処理できます。
* 状態変動イベント：ネットワークタイプ変動、Launching状態変動(メンテナンスなどによる状態変動)、Heartbeat情報変動(ユーザー利用停止などによるHeartbeat情報変動)など



#### Observer Type
現在GamebaseでサポートするObserver Typeは次の通りです。

* Networkタイプ変動
    * ネットワーク変動事項情報を受け取れます。例えば、message.data[@"code"]の値でNetwork Typeを知ることができます。
    * Type: kTCGBObserverMessageTypeNetwork (= @"network")
    * Code: NetworkStatusに宣言された定数は次の通りです。
        * NotReachable: -1
        * ReachableViaWWAN: 0
        * ReachableViaWifi: 1        
        * ReachabilityIsNotDefined: -100
* Launching状態変動
    * 周期的にアプリケーションの状態を確認するLaunching Status responseに変動がある時に発生します。例えば、メンテナンス、アップデート推奨などによるイベントがあります。
    * Type: kTCGBObserverMessageTypeLaunching (= @"launching")
    * Code: TCGBLaunchingStatusに宣言された定数は次の通りです。
        * IN_SERVICE: 200
        * RECOMMEND_UPDATE: 201
        * IN_SERVICE_BY_QA_WHITE_LIST: 202
        * REQUIRE_UPDATE: 300
        * BLOCKED_USER: 301
        * TERMINATED_SERVICE: 302
        * INSPECTING_SERVICE: 303
        * INSPECTING_ALL_SERVICES: 304
        * INTERNAL_SERVER_ERROR: 500
* Heartbeat情報の変動
    * 周期的にGamebaseサーバーと接続を維持するHeartbeat responseに変動がある時に発生します。例えばユーザー利用停止によるイベントがあります。
    * Type: ObserverkTCGBObserverMessageTypeHeartbeat (= @"heartbeat")
    * Code: TCGBErrorCodeに宣言された定数は次の通りです。
        * TCGB_ERROR_INVALID_MEMBER: 6
        * TCGB_ERROR_BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase ClientにObserverを登録して各種状態変動イベントを処理できます。

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
下記のAPIを使用して、Gamebaseに登録されたObserverを削除できます。

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
  |  userLevel | M | int | ゲームユーザーレベルを表すフィールドです。 |
  | levelUpTime | M | long | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |
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

Gamebaseでは顧客からの問い合わせに対応するための機能を提供します。

> [TIP]
>
> NHN Cloud Contactサービスと連携して使用すると、簡単に顧客からの問い合わせに対応できます。
> 詳細はNHN Cloud Contactサービスの利用ガイドを参照してください。
> [NHN Cloud Online Contact Guide](/Contact%20Center/ja/online-contact-overview/)
>

#### Open Contact WebView

Gamebase Consoleに入力した**サポートURL**をWebビューで表示する機能です。
**Gamebase Console > App > InApp URL > Service center**に入力した値が使用されます。

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
