## Game > Gamebase > iOS SDK ご利用ガイド > 初期化

Gamebase iOS SDKを使用するためには、まず初期化を行う必要があります。

### Import Header File

まず、Gamebaseのヘッダーファイルをアプリに持ってくる必要があります。<br/>
AppDelegate.hなどGamebase機能を初期化する場所に次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Initialization Flow

ゲームが始まったらDebug Modeを設定し、Gamebaseを初期化して、Launching Status Codeに従ってゲームに進入可否を決定するように、以下のフローのように実装してください。

![initialization flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

Gamebaseを初期化する際に、TCGBConfigurationの客体でGamebaseの設定を変更することができます。

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion:| M                          | TCGBConfigurationのアプリIDとアプリバージョンを設定します。<br/>アップデート、メンテナンスに該当するかどうかはゲームバージョンで判断します。<br/>ゲームバージョンを指定してください。|
| enablePopup:                      | O                          | **[UI]**<br/>システムメンテナンス、利用制限(ban)などゲームユーザーがゲームをプレイすることができない状況のとき、ポップアップなどで理由を表示しなければならない場合があります。<br/>**YES**に設定すれば、Gamebaseが該当する状況で案内ポップアップを自動で表示します。<br/>デフォルトは**NO**です。<br/>**NO**の状態のときは起動結果を通して情報を取得した後に直接UIを設計し、ゲームをプレイすることができない理由を表示してください。|
| enableLaunchingStatusPopup:       | O                          | **[UI]**<br/>起動結果によりログインできない状態のとき(主にメンテナンス状態)、 Gamebaseが自動でポップアップを表示するかどうかを変更することができます。<br/>**enablePopup:YES**の状態でのみ動作します。<br/>デフォルトは**YES**です。|
| enableBanPopup:                   | O                          | **[UI]**<br/>ゲームユーザーが利用を制限された状態のとき、Gamebaseが自動でbanされた理由をポップアップで表示するかどうかを変更することができます。<br/>**enablePopup:** の状態でのみ動作します。<br/>デフォルトは**YES**です。|


### Debug Mode
Gamebaseは、警告(warning)とエラーログだけを表示します。
開発の参考になるシステムログをオンにしたいときは、**[TCGBGamebase setDebugMode:YES]**を呼び出してください。

> <font color="red">[注意]</font><br/>
>
> ゲームを**リリース**するときは、必ずソースコードからsetDebugMode:の呼び出しを除去したりパラメーターをNOに変えてからビルドしてください。

デバッグ設定は、コンソールでも行うことができ、コンソールで設定された値を優先視します。
コンソールの設定方法は、下記のガイドを参照してください。

* [コンソールテスト端末設定](./oper-app/#test-device)
* [コンソールクライアント設定](./oper-app/#client)



### Initialize
**application:didFinishLaunchingWithOptions:**メソッドで次のように初期化を進めます。


> <font color="red">[注意]</font><br/>
>
> Gamebaseを初期化するための**initializeWithConfiguration:launchOptions:completion:**メソッドの呼び出しは**application:didFinishLaunchingWithOptions:**の他にも呼び出すことができます。
>

<br/>


> <font color="red">[注意]</font><br/>
>
> **initializeWithConfiguration:launchOptions:completion:**メソッドを呼び出さずに他のGamebaseAPIを呼び出すと、正常に動作しないことがあります。

1. **TCGBConfiguration**の客体を作成して各属性を設定します。
2. 設定された**TCGBConfiguration**の客体を使用して**initializeWithConfiguration:launchOptions:completion:**を呼び出します。
3. **completion**ブロックに転送された**TCGBError**の客体を確認して成功したかどうかを判断し、初期化に失敗したときは、もう一度試せるようにします。



```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration enablePopup:YES];
    [configuration enableLaunchingStatusPopup:YES];
    [configuration enableBanPopup:YES];

    [TCGBGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```



### Launching Information

Gamebase初期化の呼び出し結果により起動状態を確認することができます。<br/>
起動状態は、Gamebase初期化後に呼び出さなければなりません。

```objectivec
- (void)myMethodAfterGamebaseInitialized {
    TCGBLaunchingStatus launchingStatus = [TCGBLaunching launchingStatus];

    // You can check whether if Gamebase was initialized or not using this launchingStatus
    if (launchingStatus == 0) {
        NSLog(@"Service is not initialized.");
    }

    // After Initialize Complete
    if (launchingStatus == INSPECTING_SERVICE) {
        NSLog(@"Service in Maintenance");
    } else if (launchingStatus == IN_SERVICE) {
        NSLog(@"Service in Service");
    } else {
        ...
    }
}

```

launchingInformations APIを利用すると、初期化後もLaunchingInfoオブジェクトを取得できます。

> <font color="red">[注意]</font><br/>
>
> launchingInformations APIは、リアルタイムでサーバーから情報を取得する非同期APIではありません。
> 2分毎にアップデートされるキャッシュ情報を返すため、リアルタイムで現在のメンテナンス状況を判断する用途には適していません。
> このような場合にはLaunching Status Codeが変更されれ時にイベントが動作するGamebaseEventHandlerを活用してください。
> [Game > Gamebase > iOS SDK使用ガイド > ETC > Additional Features > Gamebase Event Handler > Observer](./ios-etc/#observer)

**API**

```objectivec
#import <Gamebase/Gamebase.h>

NSDictionary* launchingInfo = [TCGBLaunching launchingInformations];
```


#### 1. Launching

Gamebaseローンチ情報です。

**1.1 Status**

Gamebase iOS SDKの初期化設定に入力したアプリバージョンのゲーム状態情報です。

* code：ゲームステータスコード(メンテナンス中、アップデート必須、サービス終了など)
* message：ゲーム状態メッセージ

ステータスコードは下記の表を参照してください。

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | サービスが正常に動作しています。                                 |
| RECOMMEND_UPDATE            | 201  | アップデートを推奨します。                                 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | メンテナンス中にはサービスを利用することができませんが、QA端末として登録されている場合はメンテナンスに関係なくサービスに接続してテストすることができます。|
| IN_TEST                     | 203  | テスト中 |
| IN_REVIEW                   | 204  | 審査中 |
| IN_BETA                     | 205  | ベータサーバー環境 |
| REQUIRE_UPDATE              | 300  | アップデートが必ず必要です。                                 |
| BLOCKED_USER                | 301  | 接続ブロックに登録された端末(デバイスキー)でサービスに接続したケースです。|
| TERMINATED_SERVICE          | 302  | サービスが終了しました。                                  |
| INSPECTING_SERVICE          | 303  | サービスをメンテナンス中です。                                |
| INSPECTING_ALL_SERVICES     | 304  | 全体サービスをメンテナンス中です。                             |
| INTERNAL_SERVER_ERROR       | 500  | 内部サーバーエラーです。                                |

[Game > Gamebase > コンソール使用ガイド > アプリ > App](./oper-app/#app)

**1.2 App**

Gamebaseコンソールに登録されたアプリ情報です。

* accessInfo
    * serverAddress：サーバーアドレス
* customerService
    * accessInfo ：サポートの連絡先
    * type：サポートタイプ
    * url ：サポートURL
* relatedUrls
    * termsUrl：利用約款
    * personalInfoCollectionUrl：個人情報同意
    * punishRuleUrl：利用停止規定
* install：インストールURL
* idP：認証情報

[Game > Gamebase > コンソール使用ガイド > アプリ > Client](./oper-app/#client)

**1.3 Maintenance**

Gamebaseコンソールに登録されたメンテナンス情報です。

* url：メンテナンスページURL
* timezone：標準時間帯(timezone)
* beginDate：開始時間
* endDate：終了時間
* message：メンテナンス理由

[Game > Gamebase > コンソール使用ガイド > 運営 > Maintenance](./oper-operation/#maintenance)

##### Change Default Maintenance HTML

enablePopupとenableLaunchingStatusPopupの値がいずれもtrueの場合、ゲームがメンテナンス状態の時、自動的にメンテナンスポップアップが表示されます。
ここで**詳細表示**ボタンをクリックするとメンテナンス情報が自動的にWebビューで表示されます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_webview_android_2.30.0.png)

この時に表示されるhtmlファイルを修正したい場合は、次のリンクのhtmlファイルをダウンロードして自由に修正した後、Xcodeプロジェクトの`Copy Bundle Resources`に**gamebase-maintenance.html**ファイルを追加してください。
[htmlファイルのダウンロードLINK](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gamebase-maintenance.html)

**1.4 Notice**

Gamebaseコンソールに登録された告知情報です。

* message: Message
* title: Title
* url: Maintenance URL

[Game > Gamebase > コンソール使用ガイド > 運営 > Notice](./oper-operation/#notice)

#### 2. tcProduct

Gamebaseと連携したNHN Cloudサービスのアプリケーションキーです。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloudコンソールに登録されたIAPストア情報です。

* id: App ID
* name: App Name
* storeCode: Store Code

[Game > Gamebase > コンソール使用ガイド > 決済](./oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launching Consoleでユーザーが入力した情報です。

* ユーザーが入力した値をJSON stringで渡します。
* NHN Cloud Launching詳細設定は、次のガイドを参照してください。

[Game > Gamebase > コンソール使用ガイド > 管理 > Config](./oper-management/#config)


### Handling Unregistered Version
      
Gamebaseコンソールに登録されていないGameClientVersionを初期化すると、**LAUNCHING_UNREGISTERED_CLIENT(2004)**エラーが発生します。
enablePopup(true)、enableLaunchingStatusPopup(true)状態の場合、強制アップデートポップアップが表示され、マーケットへ移動する場合があります。
Gamebaseポップアップを使用していない場合はUpdateInfoをTCGBErrorオブジェクトから取得し、ユーザーがマーケットへ移動できるようにゲームで直接UIを実装できます。

**VO**

```objectivec
@interface TCGBUpdateInfo : NSObject

// 最新バージョンをダウンロードできるストアインストールURL.
@property (nonatomic, strong, nullable) NSString* installUrl;

// ユーザーに表示できるメッセージで、ユーザーの端末言語に合わせて伝達されます。
// 言語が「en」の場合、メッセージは次の通りです。
// 'The version is not supported. Please get the latest update version.'
@property (nonatomic, strong, nullable) NSString* message;

@end
```


**API**

```objectivec
+ (nullable TCGBUpdateInfo *)updateInfoFromError:(nonnull TCGBError *)error;
```


**Example**

```objectivec
- (void)initializeGamebase {
    TCGBConfiguration* config = [TCGBConfiguration configurationWithAppID:@"YOUR_APP_ID" appVersion:@"YOUR_APP_VERSION" zoneType:@"YOUR_ZONE_TYPE"];
    [TCGBGamebase initializeWithConfiguration:config completion:^(id launchingData, TCGBError *result) {

        if (result == nil) {
            // Gamebase initialization succeeded.
        } else {
            // Gamebase initialization failed.
            TCGBUpdateInfo* updateInfo = [TCGBUpdateInfo updateInfoFromError:result];
            if (updateInfo) {
                // Unregistered game client version.
                // Open market url to update application.
                NSLog(@"UpdateInfo after initialize => \n%@", [updateInfo prettyJsonString]);
            }
        
        }
    }];
}
```

## Lifecycle Event

iOSのアプリイベントを管理したい場合、次の**UIApplicationDelegate**プロトコルを設計します。

> <font color="red">[注意]</font><br/>
>
> SceneDelegate(iOS 13以上)を使用している場合は、**UISceneDelegate**プロトコルを実装する必要があります。
>

### DidFinishLaunching Event
**application:didFinishLaunchingWithOptions:**メソッドを呼び出しして、Gamebaseにアプリが起動されたことを伝える必要があります。

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### OpenURL Event
**application:openURL:sourceApplication:annotation:**メソッドを呼び出してアプリケーションの外部URL Openの試みをGamebaseに知らせなければなりません。Gamebaseでは、各Idpの認証用SDKに該当する値を送り、必要な動作をするように知らせます。

> <font color="red">[注意]</font><br/>
>
> UIApplicationDelegateの**application:openURL:options:**を既に再定義した場合、**application:openURL:sourceApplication:annotation:**が呼び出されなことがあります。
>


> <font color="red">[注意]</font><br/>
>
> WeiboAuthAdapterを使用する場合、 **application:openURL:sourceApplication:annotation:**を必ず実装する必要があります。
>

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [TCGBGamebase application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

SceneDelegate(iOS 13以上)を使用する場合は、**scene:openURLContexts:**メソッドを呼び出す必要があります。

```objectivec
// SceneDelegate.m
- (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts {
    [TCGBGamebase scene:scene openURLContexts:URLContexts];
}
```


### DidBecomeActive Event
**applicationDidBecomeActive:**メソッドを呼び出してアプリを有効にするかどうかをGamebaseに知らせなければなりません。Gamebaseでは、各Idpの認証用SDKに該当する値を送り、必要な動作をするように知らせます。



```objectivec
// AppDelegate.m
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

SceneDelegate(iOS 13以上)を使用する場合は、**sceneDidBecomeActive:**メソッドを呼び出す必要があります。

```objectivec
// SceneDelegate.m
- (void)sceneDidBecomeActive:(UIScene *)scene {
    [TCGBGamebase sceneDidBecomeActive:scene];
}
```

### DidEnterBackground Event
**applicationDidEnterBackground**メソッドを呼び出してGamebaseにアプリがバックグランド(background)に切り替わるということを知らせる必要があります。


```objectivec
// AppDelegate.m
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

SceneDelegate(iOS 13以上)を使用する場合は、**sceneDidEnterBackground:**メソッドを呼び出す必要があります。

```objectivec
// SceneDelegate.m
- (void)sceneDidEnterBackground:(UIScene *)scene {
    [TCGBGamebase sceneDidEnterBackground:scene];
}
```

### WillEnterForeground Event
**applicationWillEnterForeground**メソッドを呼び出してGamebaseにアプリがフォアグラウンド(foreground)に切り替わるということを知らせる必要があります。

```objectivec
// AppDelegate.m
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```

SceneDelegate(iOS 13以上)を使用する場合は、**sceneWillEnterForeground:**メソッドを呼び出す必要があります。

```objectivec
// SceneDelegate.m
- (void)sceneWillEnterForeground:(UIScene *)scene {
    [TCGBGamebase sceneWillEnterForeground:scene];
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED      | 1          | Gamebaseが初期化されていません。|
| TCGB\_ERROR\_NOT\_LOGGED\_IN       | 2          | ログインが必要です。          |
| TCGB\_ERROR\_INVALID\_PARAMETER    | 3          | 正しくないパラメーターです。         |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSONフォーマットエラーです。       |
| TCGB\_ERROR\_USER\_PERMISSION      | 5          | 権限がありません。             |
| TCGB\_ERROR\_NOT\_SUPPORTED        | 10         | この機能には対応しておりません。       |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS   | 12         | この機能はiOSには対応しておりません。 |



* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
