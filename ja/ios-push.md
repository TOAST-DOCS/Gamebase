## Game > Gamebase > iOS SDK ご利用ガイド > Push

### Settings

#### Apple Developer Certificates

ここではPush通知の送信に必要なApple開発者認証書を作成する過程について説明します。

* [Apple Developerサイト](https://developer.apple.com)の**Add iOS Certificate**から**Apple Push Notification service SSL**で認証書を作成します。
* Keychainを登録した後、作成された認証書をPersonal Information Exchange(.p12)形式でエクスポートします(export)。
* 認証書をエクスポート(export)するときに、パスワードを設定します。

#### NHN Cloud Console登録
* **Notification > Push > Certificate**で**APNS Certificate**と**APNS (Sandbox)Certificate**に上で作成した証明書を登録します。
* 上の証明書を作成する時に設定したパスワードを使用して登録します。

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

次のAPIを呼び出して、 NHN Cloud Pushに該当ユーザーを登録します。<br/>
プッシュ同意有無(enablePush)、広告性プッシュ同意有無(enableAdPush)、夜間広告性プッシュ同意有無(enableAdNightPush)値をユーザーから取得し、次のAPIを呼び出して登録を完了します。

> <font color="red">[注意]</font><br/>
>
> プッシュトークンがいつ有効期限切れになるかわからないため、ログイン後は常にregisterPush APIを呼び出すことを推奨します。
>

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

NHN Cloud Pushにユーザーを登録する時、TCGBNotificationOptionsオブジェクトで通知オプションの設定が可能です。<Mb>
フォアグラウンドプッシュ有無(foregroundEnabled)、バッジ使用有無(badgeEnabled)、通知音使用有無(soundEnabled)値をユーザーから取得し、次のAPIを呼び出して通知オプションの設定が可能です。

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.
    
    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];
    
    TCGBNotificationOptions* options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled badgeEnabled:badgeEnabled soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

#### Setting for APNS Sandbox

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

#### Get NotificationOptions

プッシュを登録する時に設定した通知オプション値を取得します。

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

#### TCGBNotificationOptions

| Parameter             | Values       | Description        |
| --------------------  | ------------ | ------------------ |
| foregroundEnabled     | YES or NO    | アプリがフォアグラウンド状態の時の通知表示有無<br/>**default**: NO           |
| badgeEnabled          | YES or NO    | バッジアイコン使用有無<br/>**default**: YES           |
| soundEnabled          | YES or NO    | 通知音使用有無<br/>**default**: YES           |


> [参考]
>
> foregroundEnabledオプションはランタイムの時に変更が可能です。
> badgeEnabled、soundEnabledオプションは、registerPush APIを初めて呼び出した時にのみ反映され、ランタイムの時の変更は保障されません。
>


### Request Push Settings

ユーザーのプッシュ設定を照会するために、次のAPIを利用します。<br/>
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
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | NHN Cloud Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud Pushライブラリーで発生したエラーです。
* エラーコードの確認は、次の通りです。

```objectivec
TCGBError *tcgbError = error; // Callbackで返ってきたTCGBErrorのインスタンス
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 外部ライブラリーで発生したエラーの客体
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 次の[tcgbError description]を呼び出すことで、json formatの全体のエラー情報を取得できます。
NSLog(@"TCGBError:%@", [tcgbError description]);
```

* NHN Cloud Pushのエラーコードは次の通りです。

| エラーコード | 説明 |
| --- | --- |
| TCPushErrorNotInitialized | 初期化されていない |
| TCPushErrorInvalidParameters | パラメータエラー |
| TCPushErrorPermissionDenined | 権限未取得 |
| TCPushErrorSystemFail | システム通知登録失敗 |
| TCPushErrorNetworkFail | ネットワーク送受信失敗 |
| TCPushErrorServerFail | サーバーレスポンス失敗 |
| TCPushErrorInvalidUrl | 無効なURLリクエスト |
| TCPushErrorNetworkNotReachable | ネットワーク未接続 |
