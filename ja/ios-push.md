## Game > Gamebase > iOS SDK ご利用ガイド > Push

### Settings

#### Apple Developer Certificates

ここではPush通知の送信に必要なApple開発者認証書を作成する過程について説明します。

* [Apple Developerサイト](https://developer.apple.com)の**Add iOS Certificate**から**Apple Push Notification service SSL**で認証書を作成します。
* Keychainを登録した後、作成された認証書をPersonal Information Exchange(.p12)形式でエクスポートします(export)。
* 認証書をエクスポート(export)するときに、パスワードを設定します。

#### TOAST Consoleの登録
* **Notification > Push > Certificate**で**APNS Certificate**と**APNS (Sandbox) Certificate**に上で作成した認証書を登録します。
* 上の認証書を作成する際に設定したパスワードを使用して登録します。

#### XCode Projectの設定
* **Targets > Capabilities > Push Notifications **項目を**ON**に設定します。
* 自動で作成された.entitlementsファイルを開いて、**APS Environment**のキーの値を正しく設定します。
    * **development**:Sandbox APNS
    * **production**: APNS

#### Import Header File
Push APIを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

次のAPIを呼び出してTOAST Pushに該当するユーザーを登録します。<br/>
Pushの同意状態(enablePush)、Push型広告の同意状態(enableAdPush)、夜間のPush型広告の同意状態(enableAdNightPush)の値をユーザーから取得し、次のAPIを呼び出して登録を完了させます。


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


#### Setting for APNS Sandbox

SandboxMode를 켜면, APNS Sandbox로 Push를 발송하도록 등록할 수 있습니다.
* 클라이언트 설정 방법

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* 콘솔 발송 방법
PUSH 메뉴의 **대상**에서  **iOS Sandbox** 체크박스를 선택 후 발송합니다.

### Request Push Settings

ユーザーのPush設定を照会するために、次のAPIを利用します。<br/>
コールバックで返ってくるTCGBPushConfigurationの値からユーザー設定値を取得することができます。

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryPushWithCompletion:^(TCGBPushConfiguration *configuration, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Configuration Failed.
        }

        BOOL enablePush = configuration.pushEnabled;
        BOOL enableAdPush = configuration.ADAgreement;
        BOOL enableAdNightPush = configuration.ADAgreementNight;

        // You can handle these variables.
    }];
}
```

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | TOAST Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、TOAST Pushライブラリーで発生したエラーです。
* エラーコードの確認は、次の通りです。

```objectivec
TCGBError *tcgbError = error; // Callbackで返ってきたTCGBErrorのインスタンス
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 外部ライブラリーで発生したエラーの客体
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 次の[tcgbError description]を呼び出すことで、json formatの全体のエラー情報を取得できます。
NSLog(@"TCGBError:%@", [tcgbError description]);
```

* TOAST Pushのエラーコードは、次のドキュメントをご参考ください。
    * [Notification > Push > SDK v1.4 ご利用ガイド > エラー処理](/Notification/Push/ja/sdk-guide/#_5)

