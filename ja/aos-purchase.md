## Game > Gamebase > Android SDK ご利用ガイド > 決済

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連動することができるようサポートします。

### Settings

#### 1. Store Console

* 次のIAPガイドをご参考の上、各ストアにアプリを登録してアプリケーションキーを発行してもらいます。
* [Mobile Service > IAP > Console ご利用ガイド > Store interlocking information](/Mobile%20Service/IAP/ja/console-guide/#store-interlocking-information)

#### 2. Register as Store's Tester

* 決済テストのため、次のようにストアごとにテスター登録をします。
    * Google
        * [Android > テスト購入設定](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > アプリ内決済テスト](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * 必ずアプリ内情報 - テストボタンでサンドボックスを希望する端末の電話番号を登録してテストを行う必要があります。
        * テスト用の端末にはUSIMが必要であり、電話番号を登録しなければなりません(MDN)。
        * **ONE store**のアプリケーションがインストールされている必要があります。

#### 3. TOAST IAPサービスの利用

* IAPガイドをご参考の上、IAPを設定し、アイテムを登録します。
    * [Mobile Service > IAP > Console ご利用ガイド](/Mobile%20Service/IAP/ja/console-guide/)

#### 4. Download

* ダウンロードしたSDKの**gamebase-adapter-purchase-iap**フォルダをプロジェクトに追加します。
    * ONE store 決済が不要な場合、**iap-tstore-x.x.x.aar**, **iap_tstore_plugin_vxx.xx.xx.jar**ファイルは削除してもかまいません。
    * 逆に、ONE store決済を利用する場合は、上のjarファイルを必ずプロジェクトに含めてビルドする必要があります。

#### 5. AndroidManifest.xml(ONE store only)

* ONE storeを使用するためには、次の設定を追加する必要があります。

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!--バージョン 16.XX.XXの場合、4を入力します。https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!—development:開発モード / release:運営 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Gamebaseを初期化する際にconfigurationの**setStoreCode()**を呼び出します。
* **STORE_CODE**は、次の値の中から選択します。
    * GG:Google
    * TS:ONE store
    * TEST:IAPテスト用


```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setStoreCode(STORE_CODE)	// Store codeを必ず宣言します。
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Purchase Flow

アイテムの購入は次のような手順で設計してください。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. ゲームクライアントでは、Gamebase SDKの**requestPurchase**を呼び出して決済を試みます。
2. 決済に成功した場合、**requestItemListOfNotConsumed**を呼び出して未消費決済の内訳を確認します。
3. 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーに決済アイテムに対するconsume(消費)をリクエストします。
4. ゲームサーバーは、GamebaseのサーバーにAPI経由でconsume(消費)APIをリクエストします。
   [APIガイド](/Game/Gamebase/ja/api-guide/#wrapping-api)
5. IAPサーバーからconsume(消費)APIの呼び出しに成功すると、ゲームサーバーがゲームクライアントにアイテムを配布します。

ストア決済には成功したものの、エラーが発生して正常に終了することができない場合があります。ログイン完了後に次の二つのAPIをそれぞれ呼び出し、再処理ロジックを設計してください。<br/>

1. 未処理アイテムの送信リクエスト
    * ログインに成功した後、**requestItemListOfNotConsumed**を呼び出して未消費決済の内訳を確認します。
    * 返された未消費決済内訳のリストに値が存在する場合、ゲームクライアントがゲームサーバーのconsume(消費)をリクエストしてアイテムを配布します。

2. 決済エラー再処理リクエスト
    * ログインに成功した後、**requestRetryTransaction**を呼び出して未処理内訳に対し自動で再処理を試みます。
    * 返されたsuccessListに値が存在する場合、ゲームクライアントがゲームサーバーのconsume(消費)をリクエストしてアイテムを配布します。
    * 返されたfailListに値が存在する場合、該当する値をゲームサーバーやLog & Crashなどで送信してデータを確保し、**[カスタマーセンター](https://toast.com/support/inquiry)**に再処理失敗の原因についてお問い合わせください。
	
### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。<br/>
ゲームユーザーが購入をキャンセルする場合、**GamebaseError.PURCHASE_USER_CANCELED**エラーが返されます。キャンセル処理を行ってください。

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(Activity activity, long itemSeq, GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
long itemSeq; // The itemSeq value can be got through the requestItemListPurchasable API.

Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else if(exception.getCode() == GamebaseError.PURCHASE_USER_CANCELED) {
            // User canceled.
        } else {
            // To Purchase Item Failed cause of the error
        }
    }
});
```

### Get a List of Purchasable Items

アイテムリストを照会したい場合、次のAPIを呼び出します。コールバックで返される配列(array)の中にはそれぞれ各アイテムの情報が含まれています。

**API**

```java
+ (void)Gamebase.Purchase.requestItemListPurchasable(Activity activity, GamebaseDataCallback<List<PurchasableItem>> callback);
```

**Example**

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Get a List of Non-Consumed Items

アイテムを購入したものの、アイテムが正常に消費(送信、配布)されていない未消費決済の内訳をリクエストします。<br/>
未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。

* 次の二つの状況で呼び出してください。
    1. 決済成功後、アイテム消費(consume)処理前に最終確認のために呼び出し
    2. ログイン成功後、消費(consume)できなかったアイテムが残っていないか確認するために呼び出し

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
Gamebase.Purchase.requestItemListOfNotConsumed(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Reprocess Failed Purchase Transaction

ストアでは決済が正常に行われたものの、TOAST IAPサーバーの検証失敗などにより決済が正常に行われなかった場合、APIを利用して再処理を試みます。<br/>
最後に、決済が成功した内訳を基にアイテム送信(配布)などのAPIを呼び出して処理する必要があります。

**API**

```java
+ (void)Gamebase.Purchase.requestRetryTransaction(Activity activity, GamebaseDataCallback<PurchasableRetryTransactionResult> callback);
```

**Example**

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request retry transaction failed- "
                    + "errorCode:" + exception.getCode()
                    + "errorMessage:" + exception.getMessage());
        }
    }
});
```

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックが完了していない状態でAPIが呼び出されました。    |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 該当するストアのcashが足りないため決済することができません。           |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | このストアには対応していません。<br>選択可能なストアはGG(Google)、TS(ONE store)、TESTです。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリーエラーです。<br>DetailCodeを確認してください。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* IAPーエラーの詳細は次のように確認できます。

```java
Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Purchase successful");
            ...
        } else {
            Log.e(TAG, "Purchase failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PURCHASE_EXTERNAL_LIBRARY_ERROR) {
                // IAP Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [Mobile Service > IAP > エラーコード > Client APIエラータイプ](/Mobile%20Service/IAP/ja/error-code/#client-api)

