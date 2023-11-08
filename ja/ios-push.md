## Game > Gamebase > iOS SDK ご利用ガイド > Push

> <font color="red">[注意]</font><br/>
>
> Unreal、Unityなど3rd partyプッシュプラグインまたはモジュールを使用する場合は、 Gamebaseプッシュ機能に影響を与える可能性があります。
>

### Settings

#### APNS JWT認証情報を取得する

ここではPush通知の送信に必要なAPNS JWT認証情報を取得するプロセスを説明します。

* [Notification > Push > Console Guide > APNS JWT認証情報を取得する](https://docs.toast.com/en/Notification/Push/en/console-guide/#get-authentication-information-for-apns-jwt)ガイドを参考にしてANPS JWTの登録に必要な必須認証情報を取得します。

#### Gamebase Console登録
* **Gamebase > Push > Certificate**で**APNS JWT**に上で取得した情報を入力します。

#### Notification Service Extension実装
* 受信指標収集、通知音設定などを行うには[NHN Cloud Pushガイド](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-ios/#notification-service-extension)を参考にしてアプリケーションに**Notification Service Extension**を実装する必要があります。


#### Xcode Project設定
* **Targets > Capabilities > Push Notifications**項目を **ON**に設定します。
* 自動的に作成された.entitlementsファイルを開いて、**APS Environment**キーの値を適切な値に設定します。
    * **development**: Sandbox APNS
    * **production**:  APNS

#### Import Header File
Push APIを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

次のAPIを呼び出して、NHN Cloud Pushに該当ユーザーを登録します。

プッシュ受信同意有無(TCGBPushConfiguration)をユーザーから取得し、次のAPIを呼び出して登録を完了します。

> <font color="red">[注意]</font><br/>
>
> プッシュトークンがいつ有効期限切れになるかわからないため、ログイン後は常にregisterPush APIを呼び出すことを推奨します。
>

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```

#### TCGBPushConfiguration

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| pushEnabled                   | M             | BOOL         | プッシュ同意有無 |
| ADAgreement                   | M             | BOOL         | 広告性プッシュ同意有無 |
| ADAgreementNight              | M             | BOOL         | 夜間広告性プッシュ同意有無 |
| alwaysAllowTokenRegistration  | O             | BOOL         | ユーザーがプッシュ権限を拒否してもトークンを登録するかどうか<br>YESに設定した場合は、プッシュ権限を取得できなくてもトークンを登録します。<br>**default**: NO    |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;
    BOOL alwaysAllowTokenRegistration;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                                   ADAgreement:enableAdPush
                                                                               ADAgreementNight:enableAdNightPush
                                                                   alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

<br/>

NHN Cloud Pushにユーザーを登録する時、TCGBNotificationOptionsオブジェクトで通知オプションの設定が可能です。

フォアグラウンドプッシュ有無(foregroundEnabled)、バッジ使用有無(badgeEnabled)、通知音使用有無(soundEnabled)値をユーザーから取得し、次のAPIを呼び出して通知オプションの設定が可能です。

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                      notificationOptions:(nullable TCGBNotificationOptions *)notificationOptions
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```

#### TCGBNotificationOptions

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| foregroundEnabled   | M     | BOOL         | アプリがフォアグラウンド状態の時に通知を表示するかどうか<br/>**default**: NO           |
| badgeEnabled        | M     | BOOL         | バッジアイコンの使用有無<br/>**default**: YES           |
| soundEnabled        | M     | BOOL         | 通知音の使用有無<br/>**default**: YES           |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;
    BOOL alwaysAllowTokenRegistration;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration *pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                                   ADAgreement:enableAdPush
                                                                              ADAgreementNight:enableAdNightPush
                                                                  alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];

    TCGBNotificationOptions *options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled 
                                                                                            badgeEnabled:badgeEnabled 
                                                                                            soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

### Setting for APNS Sandbox

SandboxModeをオンにすると、APNS SandboxでPushを送信するように登録できます。

* クライアント設定方法

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* コンソール送信方法

Pushメニューの**対象**から**iOS Sandbox**を選択した後に送信します。

### Get NotificationOptions

プッシュを登録する時に設定した通知オプション値を取得します。

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

> [参考]
>
> foregroundEnabledオプションはランタイムの時に変更が可能です。
> badgeEnabled、soundEnabledオプションは、registerPush APIを初めて呼び出した時にのみ反映され、ランタイムの時の変更は保障されません。
>


### Query Token Info

ユーザーのプッシュ設定を照会するために、次のAPIを利用します。
コールバックで来るTCGBPushTokenInfo値で登録したプッシュ情報を取得できます。

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryTokenInfoWithCompletion:^(TCGBPushTokenInfo *tokenInfo, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Token Info Failed.
        }

        NSString *pushType = tokenInfo.pushType;
        NSString *token = tokenInfo.token;
        ...
        // You can handle these variables.
    }];
}
```

#### TCGBPushTokenInfo

| Parameter                              | Values                           | Description                        |
| -------------------------------------- | -------------------------------- | ---------------------------------- |
| pushType                               | string                           | Pushトークンタイプ                    |
| token                                  | string                           | トークン                             |
| userId                                 | string                           | ユーザーID                         |
| deviceCountryCode                      | string                           | 国コード                         |
| timezone                               | string                           | 標準時間帯                         |
| registeredDateTime                     | string                           | トークンアップデート時間                   |
| languageCode                           | string                           | 言語設定                          |
| sandbox                                | YES or NO                        | サンドボックス環境で登録されたトークンなのかを確認    |
| agreement                              | TCGBPushAgreement                | 受信同意有無                        |

#### TCGBPushAgreement

| Parameter                              | Values                            | Description               |
| -------------------------------------- | --------------------------------- | ------------------------- |
| pushEnabled                            | YES or NO                         | 通知表示同意有無          |
| ADAgreement                            | YES or NO                         | 広告性通知表示同意有無     |
| ADAgreementNight                       | YES or NO                         | 夜間広告性通知表示同意有無 |

### Event Handling

* プッシュメッセージが到着した場合、またはプッシュメッセージをクリックしたときにイベントを受け取ることができます。
* イベントの登録方法はGamebaseEventHandlerガイドを参照してください。
    * [ Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./ios-etc/#push-received-message)
    * [ Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./ios-etc/#push-click-message)
    * [ Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./ios-etc/#push-click-action)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | NHN Cloud Pushライブラリエラーです。<br/>詳細エラーをご確認ください。 |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud Pushライブラリでエラーが発生した時に返されます。
* NHN Cloud Pushライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError:%@", [tcgbError description]);
```


* NHN Cloud Pushのエラーコードは次の通りです。

| エラーコード | 説明 |
| --- | --- |
| NHNCloudPushErrorUnknown |           不明 |
| NHNCloudPushErrorNotInitialized |   初期化されていない |
| NHNCloudPushErrorUserInvalid |      ユーザーIDが未設定 |
| NHNCloudPushErrorPermissionDenied | 権限の取得に失敗 |
| NHNCloudPushErrorSystemFailed |     システムによる失敗 |
| NHNCloudPushErrorTokenInvalid |     トークン値がないか無効 |
| NHNCloudPushErrorAlreadyInProgress | すでに進行中 |
| NHNCloudPushErrorParameterInvalid | 引数エラー |
| NHNCloudPushErrorNotSupported |     サポートしていない機能 |
| NHNCloudPushErrorClientFailed |     サーバーエラー |
