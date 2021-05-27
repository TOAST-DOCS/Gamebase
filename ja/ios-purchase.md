## Game > Gamebase > iOS SDK ご利用ガイド > 決済

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

// Legacy API
+ (void)requestPurchaseWithItemSeq:(long)itemSeq 
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

// 구매한 아이템의 상품 ID
@property (nonatomic, strong) NSString *gamebaseProductId;

// 구매한 상품의 가격
@property (assign)            float price;

// 통화 코드
@property (nonatomic, strong) NSString *currency;

// 결제 식별자
// purchaseToken 과 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의 : Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *paymentSeq;

// 결제 식별자
// paymentSeq 와 함께 'Consume' 서버 API 를 호출하는데 사용
// Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
// 주의 : Consume API 는 게임 서버에서 호출하세요!
@property (nonatomic, strong) NSString *purchaseToken;

// Apple 스토어 콘솔에 등록된 상품 ID
@property (nonatomic, strong) NSString *marketItemId;

// 상품 타입
// UNKNOWN : 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객센터로 문의하세요.
// CONSUMABLE : 소비성 상품
// AUTO_RENEWABLE : 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 상품을 구매한 User ID
// 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
@property (nonatomic, strong) NSString *userId;

// 스토어의 결제 식별자
@property (nonatomic, strong) NSString *paymentId;

// 구독이 종료되는 시각 (epoch time)
@property (nonatomic, assign) long expiryTime;

// 상품 구매 시간 (epoch time)
@property (nonatomic, assign) long purchaseTime;

// requestPurchase API 호출 시 payload 로 전달했던 값
// 이 필드는 예를 들어 동일한 User ID 로 구매 했음에도 게임 채널, 캐릭터 등에 따라 상품 구매 및 지급을 구분하고자 하는 경우 등
// 게임에서 필요로 하는 다양한 추가 정보를 담기 위한 목적으로 활용할 수 있습니다.
@property (nonatomic, strong) NSString *payload;

// 구독 상품은 갱실 될때마다 paymentId 가 변경됩니다.
// 이 필드는 맨 처음 구독 상품을 결제 했을 때의 paymentId 를 알려줍니다.
// 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로 항상 유요한 값을 보장하지는 않습니다.
@property (nonatomic, strong) NSString *originalPaymentId;

// itemSeq 로 상품을 구매하는 Lecacy API 용 식별자
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

// Gamebase 콘솔에 등록된 상품 ID
// requestPurchase API 로 상품을 구매할 때 사용
@property (nonatomic, strong) NSString *gamebaseProductId;

// 상품 가격
@property (assign) float price;

// 통화 코드
@property (nonatomic, strong) NSString *currency;

// Gamebase 콘솔에 등록된 상품 이름
@property (nonatomic, strong) NSString *itemName;

// 스토어 코드 ("AS")
@property (nonatomic, strong) NSString *marketId;

// Apple 스토어 콘솔에 등록된 상품 ID
@property (nonatomic, strong) NSString *marketItemId;

// 상품 타입
// UNKNOWN : 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객센터로 문의하세요.
// CONSUMABLE : 소비성 상품
// AUTO_RENEWABLE : 구독성 상품
// CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'
@property (nonatomic, strong) NSString *productType;

// 통화 기호가 포함된 현지화 된 가격 정보
@property (nonatomic, strong) NSString *localizedPrice;

// 스토어 콘솔에 등록된 현지화된 상품 이름
@property (nonatomic, strong) NSString *localizedTitle;

// 스토어 콘솔에 등록된 현지화된 상품 설명
@property (nonatomic, strong) NSString *localizedDescription;

// Gamebase 콘솔에서 해당 상품의 '사용 여부'
@property (nonatomic, assign, getter=isActive) BOOL active;

// itemSeq로 상품을 구매하는 Lecacy API 용 아이템 식별자
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


> `注意`
> ログイン成功後にのみ呼び出すことができます。
> ログイン成功後、他の決済APIより先に実行する必要があります。

#### 使用時の注意事項
Facebook SDK、Google AdMob SDKなどのように、SDK内にIn App Purchase(App Store決済)機能がある場合、Gamebaseにログインする前に事前決済を始めると、決済ウィンドウが表示されないことがあります。

* 解決方法
  * Facebook
    * Facebook Console > 설정 > 기본 설정 > **앱 내 이벤트를 자동으로 로깅(권장)** 기능을 비활성화
    * Facebook 인증 기능을 사용하지 않을 경우 : **GamebaseAuthFacebookAdapter.framework 파일을 제외** 시킨 후 빌드


#### Overview
* Apple Developer Overview : [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference : [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


App Storeアプリ内でアイテムを購入できる機能を提供します。
アイテム購入成功後、登録しておいた下記のハンドラでアイテムを支給できます。

プロモーションIAPは、App Store Connectで別途設定すると表示されます。


```objectivec
- (void)didSuccessLogin {
	[TCGBPurchase setPromotionIAPHandler:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
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

#### Process Promotion Event with GamebaseEventHandler

プロモーション決済イベントはGamebaseEventHandlerでも処理できます。
GamebaseEventHandlerでプロモーション決済イベントを処理する方法は、下記のガイドを参照してください。
[Game > Gamebase > iOS SDK使用ガイド > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)

### Error Handling

| Error                                                      | Error Code | Description                                                  |
| ---------------------------------------------------------- | ---------- | ------------------------------------------------------------ |
| TCGB_ERROR_NOT_SUPPORTED                                   | 10         | GamebaseAdapterが含まれていません。<br/>Errorオブジェクトのドメインが"TCGB.Gamebase.TCGBPurchase"の場合、 PurchaseAdapterの存在有無を確認してください。 |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED                    | 4001       | Gamebase PurchaseAdapterが初期化されていません。            |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED                      | 4002       | 購入がキャンセルされました。                                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 以前の購入が完了していません。                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH                   | 4004       | 該当ストアのキャッシュが足りないため決済できません。              |
| TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID               | 4005       | 該当商品が有効になっていません。                          |
| TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。       |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET              | 4010       | サポートしないストアです。 iOSのサポート可能なストアは"AS"です。 |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR            | 4201       | IAPライブラリエラーです。<br>error.messageを確認してください。    |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR                      | 4999       | 定義されていない購入エラーです。<br>全てのログを[サポート](https://toast.com/support/inquiry)へお伝えください。内容を確認後、早急にご返信させて頂きます。 |

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
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > iOS > エラーコード](/TOAST/ko/toast-sdk/iap-ios/#_15)

