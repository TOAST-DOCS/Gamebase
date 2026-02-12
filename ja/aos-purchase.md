## Game > Gamebase > Android SDK ご利用ガイド > 決済

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連動することができるようサポートします。

### Initialization

> <font color="red">[注意]</font><br/>
>
> ONE Storeはv17, v19, v21をサポートします。
> ONE Storeはバージョンごとにv17、v19、v21、externalのいずれか1つのみ使用することができ、同時に使用できません。

* Gamebaseの初期化時、ストアコードを指定する必要があります。
* **STORE_CODE**は、次の値の中から選択します。
    * GG：Google
    * ONESTORE：ONEstore
    * GALAXY：Galaxy Store
    * HUAWEI：Huawei AppGallery
    * MYCARD: MyCard

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, callback);
```

### Purchase Flow

アイテムの購入は大きく分けて**決済フロー**、**[消費フロー](./aos-purchase/#consume-flow)**、**[再処理フロー](./aos-purchase/#retry-transaction-flow)**の3つがあります。
**決済フロー**は、次のような順序で実装してください。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 以前の決済が正常に終了せず、再処理が動作しない場合、決済に失敗します。そのため決済前に**requestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合はConsume Flowを進行します。
2. ゲームクライアントではGamebase SDKの**requestPurchase**を呼び出して決済を試行します。
3. 決済に成功すると**requestItemListOfNotConsumed**を呼び出して未消費決済履歴を確認した後、支給するアイテムが存在する場合、Consume Flowを進行します。

### Consume Flow

未消費決済履歴リストに値がある場合、次のような順序で**Consume Flow**を進行してください。

> <font color="red">[注意]</font><br/>
>
> アイテムの重複支給が発生しないように、ゲームサーバーで必ず重複支給の有無をチェックしてください。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID、paymentSeq、purchaseTokenを渡します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1. まだアイテムを支給していなければGamebaseサーバーのPayment Transaction APIを呼び出してpurchaseTokenが有効か、レスポンスフィールドのpaymentSeqと一致するか検証します。
        * [Game > Gamebase > APIガイド > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseTokenがサーバーAPIガイド文書の**accessToken**に該当します。
    * 2-2. gamebaseProductIdはサーバーのPayment Transaction APIのレスポンスフィールドで確認できます。
        * クライアントの未消費決済履歴リストにもgamebaseProductIdが存在しますが、再処理時にはその値がない場合もありますので、サーバーのPayment Transaction APIから取得したgamebaseProductIdの値を使用してください。
    * 2-3. Payment Transaction APIの呼び出しが成功し、purchaseTokenが正常であることが確認されると、UserIDにgamebaseProductIdに該当するアイテムを支給します。
    * 2-4. アイテム支給後、ゲームDBにUserID、gamebaseProductId、paymentSeq、purchaseTokenを保存して重複支給防止または再支給ができるようにします。
3. アイテム支給有無に関係なく、ゲームサーバーは未消費履歴が返されないようにGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
    * [Game > Gamebase > APIガイド > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* ストア決済には成功したがエラーが発生して正常に終了しなかった場合があります。
* **requestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合、[Consume Flow](./aos-purchase/#consume-flow)を進行してください。
* 再処理は次の時点で呼び出すことを推奨します。
    * ログイン完了後
    * 決済前
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時

### Purchase Items

購入するアイテムのgamebaseProductIdを利用して次のAPIを呼び出し、購入をリクエストします。<br/>
gamebaseProductIdは一般的にはストアに登録したアイテムのIDと同じですが、Gamebaseコンソールで変更することもできます。

ゲームユーザーが購入をキャンセルすると、**GamebaseError.PURCHASE_USER_CANCELED**エラーが返ります。
キャンセル処理をしてください。

느린 결제나 부모 동의와 같이 결제 완료를 기다려야 하는 상황이 발생하는 경우에는 **GamebaseError.PURCHASE_PENDING** 오류가 반환됩니다.
이후에 결제가 정상적으로 완료되는 경우, GamebaseEventHandler에서 결제 완료 이벤트를 수신할 수 있습니다.
[Game > Gamebase > Android SDK使用ガイド > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt receipt, GamebaseException exception) {
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

**VO**

```java
class PurchasableReceipt {
    // 購入したアイテムの商品IDです。
    @Nullable
    String gamebaseProductId;
    
    // Gamebase.Purchase.requestPurchase API呼び出し時にpayloadに渡された値です。
    // ストアサーバーの状態によっては情報が失われる場合があるため、使用を推奨しません。
    @Nullable
    String payload;
    
    // 購入した商品の価格です。
    float price;
    
    // 通貨コードです。
    @NonNull
    String currency;
    
    // 決済識別子です。
    // purchaseTokenと一緒にConsumeサーバーAPIを呼び出すのに使用する重要な情報です。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIは、ゲームサーバーで呼び出してください！
    @NonNull
    String paymentSeq;
    
    // 決済識別子です。
    // paymentSeqと一緒にConsumeサーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume APIでは「accessToken」という名前のパラメータで渡す必要があります。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIは、ゲームサーバーで呼び出してください！
    @Nullable
    String purchaseToken;
    
    // Google、Appleのようにストアコンソールに登録された商品IDです。
    @NonNull
    String marketItemId;
    
    // 商品タイプです。次の値を使用できます。
    // * UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTO_RENEWABLE：購読型商品。
    // * CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに、定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」。
    @NonNull
    String productType;
    
    // 商品を購入したUser ID。
    // 商品を購入していないUser IDでログインした場合、購入したアイテムを獲得できません。
    @NonNull
    String userId;
    
    // ストアの決済識別子です。
    @Nullable
    String paymentId;
    
    // 商品を購入した時刻です。(epoch time)
    long purchaseTime;
    
    // 購読が終了する時刻です。(epoch time)
    long expiryTime;
    
    // Google決済時に使用される値。次の値を使用できます。
    // しかしGoogleサーバーで障害が発生してGamebase決済サーバーで一時的に検証ロジックをオフにする場合は
    // nullを返すため、常に有効な値を保障しないことに注意してください。
    // * null：一般決済
    // * Test：テスト決済
    // * Promo：Promotion決済
    @Nullable
    String purchaseType;
    
    // 購読商品は更新されるごとにpaymentIdが変更されます。
    // このフィールドは最初に購読商品を決済した時のpaymentIdを伝えます。
    // ストアによっては、決済サーバーの状態に応じた値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    @Nullable
    String originalPaymentId;
    
    // itemSeqで商品を購入するLegacy API用の識別子です。
    long itemSeq;

    // 商品を購入したStore Code。
    @NonNull
    public String storeCode;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "paymentSeq": "2021032510000001",
    "purchaseToken": "5U_NVCLKSDFKLJJ...",
    "marketItemId": "my_product_001",
    "productType": "CONSUMABLE",
    "userId": "AS@123456ABCDEFGHIJ",
    "paymentId": "GPA.1111-2222-3333-44444",
    "purchaseTime": 1616649225531,
    "expiryTime": 0,
    "itemSeq": 1000001
}
```
```json
{
    "gamebaseProductId": "my_subcription_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "paymentSeq": "2021032510000001",
    "purchaseToken": "5U_NVCLKKLJLSDG...",
    "marketItemId": "my_subcription_product_001",
    "productType": "CONSUMABLE_AUTO_RENEWABLE",
    "userId": "AS@123456ABCDEFGHIJ",
    "paymentId": "GPA.1111-2222-3333-56789",
    "purchaseTime": 1617069916128,
    "expiryTime": 1617070323784,
    "purchaseType": "Test",
    "originalPaymentId": "GPA.1111-2222-3333-56789",
    "itemSeq": 1000002
}
```

### List Purchasable Items

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

**VO**

```java
class PurchasableItem {
    // Gamebaseコンソールに登録された商品IDです。
    // Gamebase.Purchase.requestPurchase APIで商品を購入する時に使用されます。
    @Nullable
    String gamebaseProductId;
    
    // 商品の価格です。
    float price;
    
    // 通貨コードです。
    @Nullable
    String currency;
    
    // Gamebaseコンソールに登録されている商品名です。
    @Nullable
    String itemName;
    
    // Google、Appleのようにストアコンソールに登録された商品IDです。
    @NonNull
    String marketItemId;
    
    // 商品タイプです。次の値を使用できます。
    // * UNKNOWN：認識できないタイプ。Gamebase SDKをアップデートするか、Gamebaseサポートへお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTORENEWABLE：購読型商品。
    // * CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに定期的に消費が可能な商品を支給したい場合に使われる「消費が可能な購読商品」。
    @NonNull
    String productType;
    
    // 通貨記号が含まれるローカライズされた価格情報です。
    @Nullable
    String localizedPrice;
    
    // ストアコンソールに登録されているローカライズされた商品名です。
    @Nullable
    String localizedTitle;
    
    // ストアコンソールに登録されているローカライズされた商品説明です。
    @Nullable
    String localizedDescription;
    
    // Gamebaseコンソールで該当商品の「使用有無」を表します。
    boolean isActive;
    
    // itemSeqで商品を購入するLegacy API用の識別子です。
    long itemSeq;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_product_001",
    "price": 1000.0,
    "currency": "KRW",
    "itemName": "Consumable product for test",
    "marketItemId": "my_product_001",
    "productType": "CONSUMABLE",
    "localizedPrice": "₩1,000",
    "localizedTitle": "TEST PRODUCT 001",
    "localizedDescription": "Product for test 001",
    "isActive": true,
    "itemSeq": 1000001
}
```

### List Non-Consumed Items

* まだ消費していない一回性商品(CONSUMABLE)と消費性定期購入商品(CONSUMABLE_AUTO_RENEWABLE)情報を照会します。
* 未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。
* 正常に決済が完了しなかった場合、再処理の役割もするため、次の状況で呼び出してください。
    * ゲームユーザーに支給されなかったアイテムがあるかを確認
    	* ログイン完了後
    	* ゲーム内ショップ(またはロビー)に移動した時
    	* ユーザープロフィールまたはメールボックスを確認した時
    * 再処理が必要なアイテムがあるかを確認
    	* 決済前
    	* 決済失敗後

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| newBuilder()                    | **M**                      | Configurationオブジェクト作成のためのBuilderを作成します。                                |
| build()                         | **M**                      | 設定を終えたBuilderをConfigurationオブジェクトに変換します。                                |
| setAllStores(boolean allStores) | O                          | 同じUserIDで他のストアにて購入した未消費履歴も変換します。<br/>デフォルト値は**false**です。 |

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(@NonNull final Activity activity,
                                                       @NonNull final PurchasableConfiguration configuration,
                                                       @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder().build();
Gamebase.Purchase.requestItemListOfNotConsumed(activity, configuration, new GamebaseDataCallback<List<PurchasableReceipt>>() {
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

### List Activated Subscriptions

現在のユーザーID基準で有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、有効期限が切れる前まで照会できます。
購読ライフサイクル処理は、次の文書をご覧ください。
[NHN Cloud > SDK使用ガイド > IAP > Android > Google Play Store購読(定期的決済)機能 > 購読ライフサイクル処理](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#subscription-lifecycle-handling)

> <font color="red">[注意]</font><br/>
>
> 現在定期購入商品は、Androidの場合はGoogle Playストアのみサポートします。
>
**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                               |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------- |
| newBuilder()                    | **M**                      | Configurationオブジェクト作成のためのBuilderを作成します。                            |
| build()                         | **M**                      | 設定を終えたBuilderをConfigurationオブジェクトに変換します。                           |
| setAllStores(boolean allStores) | O                          | 同じUserIDで他のストアにて購入した購読も変換します。<br/>デフォルト値は**false**です。  |

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(@NonNull final Activity activity,
                                                    @NonNull final PurchasableConfiguration configuration,
                                                    @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder()
        .setAllStores(true)
        .build();
Gamebase.Purchase.requestActivatedPurchases(activity, configuration, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request subscription list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### List Status of Subscriptions

現在のユーザーID基準で購入した購読商品の状態を照会できます。
決済が完了した購読商品(自動更新型購読、自動更新型消費性購読商品)は、期限が切れる前まで継続して照会できます。
**PurchasableConfiguration.setIncludeExpiredSubscriptions(true)**APIで、有効期限が切れた購読商品の状態も照会できます。
購読ステータスコードは、次の文書を参照してください。
[NHN Cloud > SDK使用ガイド > IAP > Android > NHN Cloud IAP Class Reference > IapSubscriptionStatus.StatusCode](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode)

> <font color="red">[注意]</font><br/>
>
> * 購読ステータスコードは、以下のガイドに従って購読イベント設定を行うと正常に返されます。
>     * [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド > Googleシステム内リアルタイム購読情報イベント配信設定](./console-google-guide/#google_1)
>     * イベント設定を行っていない状態で購入した購読商品のステータスコードは常に0(PURCHASED)が返されます。
> * 現在、購読商品はGoogle Playストアのみサポートします。

**PurchasableConfiguration**

| API                                             | Mandatory(M) / Optional(O) | Description                              |
|-------------------------------------------------|----------------------------|------------------------------------------|
| newBuilder()                                    | **M**                      | Configurationオブジェクトを作成するためのBuilderを作成します。  |
| build()                                         | **M**                      | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setIncludeExpiredSubscriptions(boolean include) | O                          | 有効期限が切れた購読商品を含めます。<br/>デフォルト値は**false**です。 |

**API**

```java
+ (void)Gamebase.Purchase.requestSubscriptionsStatus(@NonNull final Activity activity,
                                                     @NonNull final PurchasableConfiguration configuration,
                                                     @NonNull final GamebaseDataCallback<List<PurchasableSubscriptionStatus>> callback);
```

**Example**

```java
final PurchasableConfiguration configuration = PurchasableConfiguration.newBuilder()
        .setIncludeExpiredSubscriptions(true)
        .build();
Gamebase.Purchase.requestSubscriptionsStatus(activity, configuration, new GamebaseDataCallback<List<PurchasableSubscriptionStatus>>() {
    @Override
    public void onCallback(List<PurchasableSubscriptionStatus> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request status of subscription list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage()
                    + "errorDetail: " + exception.toString());
        }
    }
});
```

**VO**

```java
class PurchasableSubscriptionStatus {
    // 購入したアイテムの商品IDです。
    @Nullable
    String gamebaseProductId;
    
    // 購読ステータスコードです。
    //
    // IapSubscriptionStatus.StatusCode : https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode
    public int statusCode;
    
    // 購読ステータスコードの説明です。
    @NonNull
    public String statusDescription;
    
    // 購入した商品の価格です。
    float price;
    
    // 通貨コードです。
    @NonNull
    String currency;
    
    // 決済識別子です。
    // purchaseTokenと一緒に「Consume」サーバーAPIを呼び出すために使用する重要な情報です。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意: Consume APIはゲームサーバーで呼び出してください！
    @NonNull
    String paymentSeq;
    
    // 決済識別子です。
    // paymentSeqと一緒に「Consume」サーバーAPIを呼び出すために使用する重要な情報です。
    // Consume APIでは'accessToken'という名前のパラメータで渡す必要があります。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意: Consume APIは、ゲームサーバーで呼び出してください！
    @NonNull
    String purchaseToken;
    
    // Google、Appleなどのストアコンソールに登録された商品IDです。
    @NonNull
    String marketItemId;
    
//商品タイプとして、次の値が来ることができます。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートにお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTO_RENEWABLE：購読商品。
    // * CONSUMABLE_AUTO_RENEWABLE：購読商品購を購入したユーザーに定期的に消費が可能な商品を支給したい場合に使用される「消費が可能な購読商品」。
    @NonNull
    String productType;
    
    // 商品を購入したUser ID.
    // 商品を購入していないUser IDでログインした場合は購入したアイテムを獲得できません。
    @NonNull
    String userId;
    
    // 商品を購入したStore Code。
    @NonNull
    public String storeCode;
    
    // ストアの決済識別子です。
    @Nullable
    String paymentId;
    
    // 商品を購入した時刻です。(epoch time)
    long purchaseTime;
    
    // 購読が終了する時刻です。(epoch time)
    long expiryTime;
    
    // Google決済時に使用される値で、次の値が来ることができます。
    // しかしGoogleサーバーで障害が発生してGamebase決済サーバーで一時的に検証ロジックをオフにした場合には
    // nullのみ返されるため常に有効な値を保障するわけではないことに注意してください。
    // * null :一般決済
    // * Test :テスト決済
    // * Promo : Promotion決済
    @Nullable
    String purchaseType;
    
    // 購読商品は更新されるたびにpaymentIdが変更されます。
    // このフィールドは、最初の購読商品を決済したときのpaymentIdを示します。
    // ストアや決済サーバーの状態によっては値が存在しない場合があるため
    // 常に有効な値を保障するわけではありません。
    @Nullable
    String originalPaymentId;
    
    // itemSeqで商品を購入するLegacy API用の識別子です。
    long itemSeq;
    
    // Gamebase.Purchase.requestPurchase API呼び出し時にpayloadに渡した値です。
    // ストアサーバーの状態によっては情報が失われる場合があるため、使用を推奨しません。
    @Nullable
    String payload;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_subcription_product_002",
    "statusCode": 13,
    "statusDescription": "EXPIRED",
    "userId": "AS@123456ABCDEFGHIJ",
    "storeCode": "GG",
    "currency": "KRW",
    "expiryTime": 1675012345678,
    "itemSeq": 1000003,
    "marketItemId": "my_subcription_product_002",
    "originalPaymentId": "GPA.1111-2222-3333-56789",
    "paymentId": "GPA.1111-2222-3333-56789",
    "paymentSeq": "2021032510000002",
    "price": 1000.0,
    "productType": "CONSUMABLE_AUTO_RENEWABLE",
    "purchaseTime": 1675001234567,
    "purchaseToken": "kfetTfGk4...",
    "purchaseType": "Test"
}
```

### Event by Purchase

Promotion 코드 입력을 통해 상품을 획득한 경우 또는 Pending 결제(느린 결제, 부모 동의 등)가 완료되었을 때 GamebaseEventHandler 를 통해 이벤트를 받아 처리할 수 있습니다.
GamebaseEventHandler 로 프로모션 결제 및 지연 결제 이벤트를 처리하는 방법은 아래 가이드를 확인하세요.
[Game > Gamebase > Android SDK使用ガイド > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                    | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックが完了していない状態でAPIが呼び出されました。    |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 該当商品が有効になっていません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 月の購入限度を超過しました。             |
| PURCHASE_PENDING                          | 4008       | 결제를 완료하려면 추가 확인이 필요합니다. |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | このストアには対応していません。<br>選択可能なストアはGG(Google)、ONESTORE、GALAXY、HUAWEI, MYCARDです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAPライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN Cloud IAPライブラリでエラーが発生した時に返されます。
* NHN Cloud IAPライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。 

```java
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, new GamebaseDataCallback<PurchasableReceipt>() {
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

* NHN Cloud IAP SDKのエラーコードは、次のドキュメントをご参考ください。
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Android > エラーコード](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-android/#error-codes)
