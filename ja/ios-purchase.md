## Game > Gamebase > iOS SDK ご利用ガイド > 決済

> <font color="red">[注意]</font><br/>
>
> Unreal、Unityなど3rd party決済プラグインまたはモジュールを使用する場合、 Gamebase決済機能に影響を与える場合があります。
>

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携させることができるようサポートしています。

### Settings

#### Apple iTunes-Connect
1. テスト用アプリのビルドをアップロード
2. In-App Purchasesアイテムの登録及び承認
3. Sandbox Testerアカウントの登録
* Detail Guide for iTunes-Connect:[Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Gamebase Console登録
次はGamebase Consoleで設定する必要がある内容です。

1. **Gamebase > Purchase(IAP) > ストア**より、利用するストアを登録します。
    * ストア：**App Store**を選択します。
2. **Gamebase > Purchase(IAP) > 商品**より、アイテムを登録します。
    * 商品ID：決済リクエスト時に使用する商品IDを入力します。
    * 商品名：決済時に表示する商品名を入力します。
    * 使用有無：アイテムの使用有無を決定します。
    * ストア：**App Store**を選択します。
    * ストアアイテムID：iTunes-Connectに登録したProduct IDを入力します。
3. **保存**を押します。

#### Xcode Projectの設定
1. **Targets > Capabilities > In-App Purchase**を**ON**に設定します。
2. **Targets > General > Identity**のBundle Identifier、Version、Buildの値を正しく設定します。

#### Import Header File

購入APIを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

アイテムの購入は大きく分けて決済フロー、消費フロー、再処理フローの3つがあります。
決済フローは、次のような順序で実装してください。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 以前の決済が正常に終了していない場合、再処理が動作しなければ決済が失敗します。そのため決済前に**requestItemListOfNotConsumedWithCompletion:**を呼び出して再処理を動作させ、未支給のアイテムがあればConsume Flowを進行します。
2. ゲームクライアントではGamebase SDKの**requestPurchaseWithGamebaseProductId:viewController:completion:**を呼び出して決済を試行します。
3. 決済が成功したら**requestItemListOfNotConsumedWithCompletion:**を呼び出して未消費決済履歴を確認した後、支給するアイテムが存在すればConsume Flowを進行します。

### Consume Flow

未消費決済履歴リストに値がある場合、次のような順序でConsume Flowを進行してください。

> <font color="red">[注意]</font><br/>
>
> アイテムが重複支給されることがないように、ゲームサーバーで必ず重複支給有無をチェックしてください。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID, gamebaseProductId, paymentSeq, purchaseTokenを伝達します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1. まだアイテムを支給していなければUserIDにgamebaseProductIdに該当するアイテムを支給します。
    * 2-2. アイテム支給後、ゲームDBにUserID、gamebaseProductId、paymentSeq、purchaseTokenを保存して重複支給防止または再支給ができるようにします。
3. アイテム支給有無に関係なく、ゲームサーバーはGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
    * [Game > Gamebase > APIガイド > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* ストア決済には成功したがエラーが発生して正常終了できない場合があります。
* **requestItemListOfNotConsumedWithCompletion:**を呼び出して再処理を動作させ、未支給アイテムがあれば[Consume Flow](./ios-purchase/#consume-flow)を進行してください。
* 再処理は、次のようなタイミングで呼び出すことを推奨します。
    * ログイン完了後。
    * 決済前。
    * ゲーム内商店(またはロビー)進入時。
    * ユーザープロフィールまたはメールボックスの確認時。

### Purchase Item

購入するアイテムのgamebaseProductIdを利用して次のAPIを呼び出し、購入をリクエストします。<br/>
gamebaseProductIdは一般的にはストアに登録したアイテムのIDと同じですが、Gamebaseコンソールでも変更できます。payloadフィールドに入力した追加情報は決済成功後、**TCGBPurchasableReceipt.payload**フィールドに維持されるため、複数の用途で活用できます。 <br/>
ゲームユーザーが購入をキャンセルした場合、**TCGB_ERROR_PURCHASE_USER_CANCELED**エラーが返ります。キャンセル処理を行ってください。

**API**

```objectivec
+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                              viewController:(UIViewController *)viewController
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                                     payload:(NSString *)payload 
                              viewController:(UIViewController *)viewController 
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;
```

**Example**

```objectivec
- (void)purchasingItem:(NSString *)gamebaseProductId {
    NSString *userPayload = @"USER_PAYLOAD";

    [TCGBPurchase requestPurchaseWithGamebaseProductId:gamebaseProductId payload:userPayload viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
        NSString *receivedPayload = purchasableReceipt.payload;
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Purchase Item Succeeded
        } else if (error.code == TCGB_ERROR_PURCHASE_USER_CANCELED) {
            // User Canceled Purchasing Item
        } else if (error) {
            // To Purchase Item Failed cause of the error
        }
    }];
}
```

**VO**

```objectivec
@interface TCGBPurchasableReceipt : NSObject

// 購入したアイテムの商品ID
@property (nonatomic, strong) NSString *gamebaseProductId;

// 購入した商品の価格
@property (assign)            float price;

// 通貨コード
@property (nonatomic, strong) NSString *currency;

// 決済識別子
// purchaseTokenと一緒にConsumeサーバーAPIを呼び出すために使用
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 注意：Consume APIは、ゲームサーバーで呼び出してください！
@property (nonatomic, strong) NSString *paymentSeq;

// 決済識別子
// paymentSeqと一緒にConsumeサーバーAPIを呼び出すために使用
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 注意：Consume APIは、ゲームサーバーで呼び出してください！
@property (nonatomic, strong) NSString *purchaseToken;

// Appleストアコンソールに登録された商品ID
@property (nonatomic, strong) NSString *marketItemId;

// 商品タイプ
// UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
// CONSUMABLE：消費性商品
// AUTO_RENEWABLE：購読性商品
// CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」
@property (nonatomic, strong) NSString *productType;

// 商品を購入したUser ID
// 商品を購入していないUser IDでログインした場合、購入したアイテムを獲得できません。
@property (nonatomic, strong, nullable) NSString *paymentId;

// ストアの決済識別子
@property (nonatomic, strong, nullable) NSString *paymentId;

// 購読が終了する時刻(epoch time)
@property (nonatomic, strong, nullable) NSString *payload;

// 商品購入時間(epoch time)
@property (nonatomic, strong, nullable) NSString *originalPaymentId;

// requestPurchase API呼び出し時にpayloadに渡された値
// このフィールドは例えば同じUser IDで購入したがゲームチャンネル、キャラクターなどに応じて商品の購入および支給を区分したい場合など
// ゲームで必要とするさまざまな追加情報を入れる目的で活用できます。
@property (nonatomic, strong) NSString *payload;

// 購読商品は、更新されるごとにpaymentIdが変更されます。
// このフィールドは最初に購読商品を決済した時のpaymentIdを伝えます。
// ストアによっては、決済サーバーの状態に応じた値が存在しない場合があるため常に有効な値を保障するわけではありません。
@property (nonatomic, strong) NSString *originalPaymentId;

// itemSeqで商品を購入するLegacy API用の識別子
@property (assign)            long itemSeq;

@end
```



### List Purchasable Items

アイテムリストを照会したい場合、次のAPIを呼び出します。コールバックで返される配列(array)の中にはそれぞれ各アイテムの情報が含まれています。

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListPurchasableWithCompletion:^(NSArray *purchasableItemArray, TCGBError *error) {
        if (error != nil) {
            // To Request Item List Failed cause of the error
            return;
        }

        NSMutableArray *itemArrayMutable = [[NSMutableArray alloc] init];
        [purchasableItemArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            TCGBPurchasableItem *item = (TCGBPurchasableItem *)obj;

            [itemArrayMutable addObject:item];
        }];
    }];
}
```


**VO**

```objectivec
@interface TCGBPurchasableItem : NSObject

// Gamebaseコンソールに登録された商品ID
// requestPurchase APIで商品を購入する時に使用
@property (nonatomic, strong) NSString *gamebaseProductId;

// 商品価格
@property (assign) float price;

// 通貨コード
@property (nonatomic, strong) NSString *currency;

// Gamebaseコンソールに登録された商品名
@property (nonatomic, strong) NSString *itemName;

// ストアコード("AS")
@property (nonatomic, strong) NSString *marketId;

// Appleストアコンソールに登録された商品ID
@property (nonatomic, strong) NSString *marketItemId;

// 商品タイプ
// UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
// CONSUMABLE：消費性商品
// AUTO_RENEWABLE：購読型商品
// CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」
@property (nonatomic, strong) NSString *productType;

// 通貨記号が含まれるローカライズされた価格情報
@property (nonatomic, strong) NSString *localizedPrice;

// ストアコンソールに登録されているローカライズされた商品名
@property (nonatomic, strong) NSString *localizedTitle;

// ストアコンソールに登録されているローカライズされた商品説明
@property (nonatomic, strong) NSString *localizedDescription;

// Gamebaseコンソールで該当商品の「使用有無」
@property (nonatomic, assign, getter=isActive) BOOL active;

// itemSeqで商品を購入するLecacy API用のアイテム識別子
@property (assign) long itemSeq;

@end
```

### List Non-Consumed Items

アイテムを購入したものの、正常にアイテムが消費(送信、配布)されていない未消費決済の内訳をリクエストします。<br/>
未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。.

* 正常に決済が完了しなかった場合、再処理の役割もするため、次の状況で呼び出してください。
    * ゲームユーザーに支給されなかったアイテムがあるかを確認
        * ログイン完了後
        * ゲーム内ショップ(またはロビー)に移動した時
        * ユーザープロフィールまたはメールボックスを確認した時
    * 再処理が必要なアイテムがあるかを確認
        * 決済前
        * 決済失敗後

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListOfNotConsumedWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Non-consumed Item List Failed cause of the error
            return;
        }

        // Should Deal With This Non-consumed Items.
        // Send this item list to the game(item) server for consuming item
    }];
}
```

### List Activated Subscriptions

現在のユーザーIDで有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、期間が終了するまで照会できます。 
ユーザーIDが同じならAndroidとiOSで購入した定期購入商品が全て照会されます。

### Reprocess Failed Purchase Transaction

ストアでは決済が正常に行われたものの、TOAST IAPサーバーの検証エラーなどにより正常に決済されていない場合は、APIを利用して再処理を試みます。<br/>
最後に、決済が成功した内訳を基にアイテム送信(配布)などのAPIを呼び出して処理する必要があります。

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestActivatedPurchasesWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Activated Item List Failed cause of the error
            return;
        }

        // Should Deal With This Activated Items.
    }];
}
```

### Restore Purchase

ユーザーのApp Storeアカウントで購入した履歴を基準に購入履歴を復元してコンソールに反映します。
購入した定期購入商品が照会できない時や、有効になっていない場合に使用します。
有効期限が切れた決済を含めて復元された決済が結果に返ります。
自動更新型消費性定期購入商品は反映されていない購入履歴がある場合、復元後に未消費購入履歴で照会できます。


```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestRestoreWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Restore Failed cause of the error
            return;
        }

        // Should Deal With This Restored Items.
    }];
}
```

### Event by Promotion

> `注意`
> iOS 11以上でのみ使用できます。
> Gamebase 1.13.0以上でサポートします。(NHN Cloud IAP SDK 1.6.0以上適用)


プロモーション決済イベントはGamebaseEventHandlerによって処理できます。
GamebaseEventHandlerでプロモーション決済イベントを処理する方法は、以下のガイドを確認してください。
[Game > Gamebase > iOS SDK使用ガイド > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)

#### 使用時の注意事項
Facebook SDK、Google AdMob SDKなどのように、SDK内にIn App Purchase(App Store決済)機能がある場合、Gamebaseにログインする前に事前決済を始めると、決済ウィンドウが表示されないことがあります。

* 解決方法
  * Facebook
    * Facebook Console > 設定 > 基本設定 > **アプリ内イベントを自動的にロギング(推奨)**機能を無効化
    * Facebook認証機能を使用しない場合：**GamebaseAuthFacebookAdapter.frameworkファイルを除外**した後ビルド


#### Overview
* Apple Developer Overview : [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference : [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


App Storeアプリ内でアイテムを購入できる機能を提供します。
アイテム購入成功後、登録しておいた下記のハンドラでアイテムを支給できます。

プロモーションIAPは、App Store Connectで別途設定すると表示されます。


#### How to Test App Store Promotion IAP

> `注意`
> App Store Connectにアプリをアップロードし、TestFlightでアプリをインストールした後、テストできます。
>

1. TestFlightでアプリをインストールします。
2. 下記のようなURLスキーム(scheme)を呼び出し、テストを進行します。

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 固定値 |
| host &amp; path | なし | なし |
| queries | action | purchaseIntent |
| | bundleId | アプリのbundeld identifier |
| | productIdentifier | 購入アイテムのproduct identifier |

例) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                                | Error Code | Description                              |
| ---------------------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                             | 10         | GamebaseAdapterが含まれていません。<br/>Errorオブジェクトのドメインが"TCGB.Gamebase.TCGBPurchase"の場合、 PurchaseAdapterの存在有無を確認してください。 |
| TCGB_ERROR_PURCHASE_NOT_INITIALIZED                  | 4001       | Gamebase PurchaseAdapterが初期化されていません。            |
| TCGB_ERROR_PURCHASE_USER_CANCELED                    | 4002       | 購入がキャンセルされました。                                       |
| TCGB_ERROR_PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 以前の購入が完了していません。                             |
| TCGB_ERROR_PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 該当ストアのキャッシュが足りないため決済できません。              |
| TCGB_ERROR_PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 該当商品が有効になっていません。                          |
| TCGB_ERROR_PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。       |
| TCGB_ERROR_PURCHASE_LIMIT_EXCEEDED                   | 4007       | 月の購入限度を超過しました。             |
| TCGB_ERROR_PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | サポートしないストアです。 iOSのサポート可能なストアは"AS"です。 |
| TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | IAPライブラリエラーです。<br>error.messageを確認してください。    |
| TCGB_ERROR_PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へお伝えください。内容を確認後、早急にご返信させて頂きます。 |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* エラーコードの確認は、次の通りです。

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError:%@", [tcgbError description]);
```

* IAPエラーコードは、次の文書を参照してください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > iOS > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-ios/#error-codes)
