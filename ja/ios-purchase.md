## Game > Gamebase > iOS SDK ご利用ガイド > 決済

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携させることができるようサポートしています。

### Settings

#### Apple iTunes-Connect
1. テスト用アプリのビルドをアップロード
2. In-App Purchasesアイテムの登録及び承認
3. Sandbox Testerアカウントの登録
* Detail Guide for iTunes-Connect:[Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### TOAST Consoleの登録
次は、TOAST Consoleで設定する必要のある内容です。

1. **Gamebase > Purchase(IAP) > アプリ**で利用するストアを登録します。
    * ストア:**App Store**を選択します。
2. **Gamebase > Purchase(IAP) > アイテム**でアイテムを登録します。
    * ストア:**App Store**を選択します。
    * ストアアイテムID:iTunes-Connectに登録したProduct IDを入力します。
3. アイテムを設定したら、**保存**をクリックします。

#### Xcode Projectの設定
1. **Targets > Capabilities > In-App Purchase**を**ON**に設定します。
2. **Targets > General > Identity**のBundle Identifier、Version、Buildの値を正しく設定します。

#### Import Header File

購入APIを設計するViewControllerに次のヘッダーファイルを持ってきます。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

アイテムの購入は次のような手順で設計してください。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. ゲームクライアントでは、Gamebase SDKの**requestPurchaseWithItemSeq:viewController:completion:**を呼び出して決済を試みます。
2. 決済が成功した場合、**requestItemListOfNotConsumedWithCompletion:**を呼び出して未消費決済の内訳を確認します。
3. 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーに決済アイテムに対するconsume(消費)をリクエストします。
4. ゲームサーバーは、Gamebase サーバーAPIを通してconsume(消費)APIをリクエストします。
    [APIガイド](./api-guide/#wrapping-api)
5. IAPサーバーからconsume(消費)APIの呼び出しに成功すると、ゲームサーバーがゲームクライアントにアイテムを配布します。

ストア決済には成功したものの、エラーが発生し、正常に終了することができない場合があります。ログイン完了後に次の二つのAPIをそれぞれ呼び出し、再処理ロジックを設計してください。<br/>

1. 未処理アイテムの送信リクエスト
    * ログインに成功した後、**requestItemListOfNotConsumedWithCompletion:**を呼び出して未消費決済内訳を確認します。
    * 返された未消費決済内訳のリストに値が存在する場合、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを配布します。
2. 決済エラー再処理リクエスト
    * ログインに成功した後、**requestRetryTransactionWithCompletion:**を呼び出して未処理内訳に対し自動で再処理を試みます。
    * 返されたsuccessList に値が存在する場合、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを配布します。
    * 返されたfailListに値が存在する場合、該当する値をゲームサーバーやLog & Crashなどを通した送信でデータを確保し、[カスタマーセンター](https://toast.com/support/inquiry)に再処理失敗の原因をお問い合わせください。


### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。

```objectivec
- (void)purchasingItem:(long)itemSeq {
    [TCGBPurchase requestPurchaseWithItemSeq:itemSeq viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
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



### Get a List of Purchasable Items

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


### Get a List of Non-Consumed Items

アイテムを購入したものの、正常にアイテムが消費(送信、配布)されていない未消費決済の内訳をリクエストします。<br/>
未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。.

* 次の二つの状況で呼び出してください。
    1. 決済成功後、アイテム消費(consume)処理前に最終確認のために呼び出し
    2. ログイン成功後、消費(consume)できなかったアイテムが残っていないか確認するために呼び出し


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



### Reprocess Failed Purchase Transaction

ストアでは決済が正常に行われたものの、TOAST IAPサーバーの検証エラーなどにより正常に決済されていない場合は、APIを利用して再処理を試みます。<br/>
最後に、決済が成功した内訳を基にアイテム送信(配布)などのAPIを呼び出して処理する必要があります。

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestRetryTransactionWithCompletion:^(TCGBPurchasableRetryTransactionResult *transactionResult, TCGBError *error) {
        if (error != nil) {
            // To Retry Failed Purchasing Transaction Failed cause of the error
            return;
        }

        // Should Deal With This Retry Transaction Result.
        // You may send result to your gameserver and add item to user.
    }];
}
```



### AppStore Promotion IAP

> `注意`
> iOS 11以上でのみ使用できます。
> Xcode 9.0以上でビルドする必要があります。
> Gamebase 1.13.0以上でサポートします(TOAST IAP SDK 1.6.0以上適用)。


> `注意`
> ログイン成功後にのみ呼び出すことができます。
> ログイン成功後、他の決済APIより先に実行する必要があります。


#### Overview
* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f


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


#### How to Test AppStore Promotion IAP

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

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | GamebaseAdapterが含まれていません。<br/>Error 客体のドメインが"TCGB.Gamebase.TCGBPurchase" の場合、PurchaseAdapterが存在しているかどうかを確認してください。|
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | Gamebase PurchaseAdapterが初期化されていませんでした。  |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | 購入がキャンセルされました。                            |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 前回の購入が完了していません。                      |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 該当するストアのCashが足りないため、決済することができません。           |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | このストアには対応しておりません。iOSで対応できるストアは、「AS」です。|
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | IAPライブラリーエラーです。<br>error.messageを確認してください。|
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

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

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [Mobile Service > IAP > エラーコード> Client APIエラータイプ](/Mobile%20Service/IAP/ja/error-code/#client-api)

