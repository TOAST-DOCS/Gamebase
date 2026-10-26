<!-- machine_translated: true -->

<!-- pre-align:aligned sig=bacf30302c7a -->

<a id="game-gamebase-android-developers-guide-purchase"></a>
## Game > Gamebase > Android SDK ご利用ガイド > 決済 { #game-gamebase-android-developers-guide-purchase }

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連動することができるようサポートします。

<a id="initialization"></a>
### Initialization { #initialization }

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

<a id="purchase-flow"></a>
### Purchase Flow { #purchase-flow }

アイテムの購入は大きく分けて**決済フロー**、**[消費フロー](#consume-flow)**、**[再処理フロー](#retry-transaction-flow)**の3つがあります。
**決済フロー**は、次のような順序で実装してください。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 以前の決済が正常に終了せず、再処理が動作しない場合、決済に失敗します。そのため決済前に**requestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合はConsume Flowを進行します。
2. ゲームクライアントではGamebase SDKの**requestPurchase**を呼び出して決済を試行します。
3. 決済に成功すると**requestItemListOfNotConsumed**を呼び出して未消費決済履歴を確認した後、支給するアイテムが存在する場合、Consume Flowを進行します。

<a id="consume-flow"></a>
### Consume Flow { #consume-flow }

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

<a id="retry-transaction-flow"></a>
### Retry Transaction Flow { #retry-transaction-flow }

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* ストア決済には成功したがエラーが発生して正常に終了しなかった場合があります。
* **requestItemListOfNotConsumed**を呼び出して再処理を行い、未支給のアイテムがある場合、[Consume Flow](#consume-flow)を進行してください。
* 再処理は次の時点で呼び出すことを推奨します。
    * ログイン完了後
    * 決済前
    * ゲーム内ショップ(またはロビー)に移動した時
    * ユーザープロフィールまたはメールボックスを確認した時

<a id="purchase-items"></a>
### Purchase Items { #purchase-items }

購入するアイテムのgamebaseProductIdを利用して次のAPIを呼び出し、購入をリクエストします。<br/>
gamebaseProductIdは一般的にはストアに登録したアイテムのIDと同じですが、Gamebaseコンソールで変更することもできます。

ゲームユーザーが購入をキャンセルすると、**GamebaseError.PURCHASE_USER_CANCELED**エラーが返ります。
キャンセル処理をしてください。

遅延決済や保護者の同意のように、決済完了を待つ必要がある状況が発生した場合は、**GamebaseError.PURCHASE_PENDING**エラーが返却されます。
その後、決済が正常に完了した場合、GamebaseEventHandlerで決済完了イベントを受信できます。
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
    // Consume API : https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意：Consume APIは、ゲームサーバーで呼び出してください！
    @NonNull
    String paymentSeq;
    
    // 決済識別子です。
    // paymentSeqと一緒にConsumeサーバーAPIを呼び出すのに使用する重要な情報です。
    // Consume APIでは「accessToken」という名前のパラメータで渡す必要があります。
    //
    // Consume API : https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
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

<a id="list-purchasable-items"></a>
### List Purchasable Items { #list-purchasable-items }

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

<a id="list-non-consumed-items"></a>
### List Non-Consumed Items { #list-non-consumed-items }

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

<a id="list-activated-subscriptions"></a>
### List Activated Subscriptions { #list-activated-subscriptions }

現在のユーザーID基準で有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、有効期限が切れる前まで引き続き照会できます。
購読ライフサイクル処理は、次の項目を参照してください。
[Google Play Store購読(定期決済)機能 > 購読ライフサイクル処理](#subscription-lifecycle-handling)

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

<a id="list-status-of-subscriptions"></a>
### List Status of Subscriptions { #list-status-of-subscriptions }

現在のユーザーID基準で購入した購読商品の状態を照会できます。
決済が完了した購読商品(自動更新型購読、自動更新型消費性購読商品)は、期限が切れる前まで継続して照会できます。
**PurchasableConfiguration.setIncludeExpiredSubscriptions(true)** APIで、期限切れの購読商品の状態も照会できます。
購読ステータスコードは、次の項目を参照してください。
[購読ステータスコード](#subscription-status-code)

> <font color="red">[注意]</font><br/>
>
> * 購読ステータスコードは、以下のガイドに従って購読イベント設定を行うと正常に返されます。
>     * [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド > Googleシステム内リアルタイム購読情報イベント配信設定](./console-google-guide/#set-up-google-notifications-to-receive-real-time-subscription-status)
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
    // 購読ステータスコード : https://docs.nhncloud.com/en/Game/Gamebase/en/aos-purchase/#subscription-status-code
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
    // purchaseTokenと一緒に'Consume'サーバーAPIを呼び出すために使用する重要な情報です。
    //
    // Consume API: https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意: Consume APIはゲームサーバーで呼び出してください！
    @NonNull
    String paymentSeq;
    
    // 決済識別子です。
    // paymentSeqと一緒に'Consume'サーバーAPIを呼び出すために使用する重要な情報です。
    // Consume APIでは'accessToken'という名前のパラメータで渡す必要があります。
    //
    // Consume API: https://docs.nhncloud.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意: Consume APIは、ゲームサーバーで呼び出してください！
    @NonNull
    String purchaseToken;
    
    // Google、Appleなどのストアコンソールに登録された商品IDです。
    @NonNull
    String marketItemId;
    
    // 商品タイプとして、次の値が来ることができます。
    // * UNKNOWN：認識できないタイプ。 Gamebase SDKをアップデートするか、Gamebaseサポートにお問い合わせください。
    // * CONSUMABLE：消費性商品。
    // * AUTO_RENEWABLE：購読商品。
    // * CONSUMABLE_AUTO_RENEWABLE：購読型商品を購入したユーザーに定期的に消費が可能な商品を支給したい場合に使用される'消費が可能な購読商品'。
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
    // Googleサーバーで障害が発生し、Gamebase決済サーバーで一時的に検証ロジックを終了する場合、
    // nullのみ返されるため、常に有効な値を保証しない場合があります。
    // * null: 一般決済
    // * Test: テスト決済
    // * Promo: Promotion決済
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

<a id="subscription-status-code"></a>
#### 購読ステータスコード

* **PurchasableSubscriptionStatus.statusCode**で返される購読ステータスを表すコードです。

| Name | Code | Status | Description |
| ---- | ---- | ------ | ----------- |
| ACTIVE | 0 | 活性 | 購読がアクティブな状態です。 |
| CANCELED | 3 | キャンセル | 購読がキャンセルされました。 |
| ON_HOLD | 5 | アカウント保留 | 定期決済がアカウント保留状態になりました(有効化されている場合)。 |
| IN_GRACE_PERIOD | 6 | 猶予期間 | 定期決済が猶予期間状態に移行しました(有効化されている場合)。 |
| PAUSED | 10 | 一時停止 | 購読が一時停止されました。 |
| REVOKED | 12 | 解約 | ユーザーが定期決済を期限切れ前にキャンセルしました。 |
| EXPIRED | 13 | 期限切れ | 定期決済が期限切れになりました。 |
| UNKNOWN | 9999 | 未定義 | 定義されていない状態です。 |

<a id="google-play-store-subscription"></a>
### Google Play Storeサブスクリプション(定期決済)機能 { #google-play-store-subscription }

Google Playストアのサブスクリプション決済の更新や期限切れなどのライフサイクルに応じたイベントを処理する方法を説明します。
詳細については、[定期決済別機能追加](https://developer.android.com/google/play/billing/billing_subscriptions)を参照してください。

<a id="subscription-lifecycle-handling"></a>
#### サブスクリプションのライフサイクル処理

Google Play Storeのサブスクリプションは、ライフサイクル中にさまざまな状態変化を経るため、アプリは各状態に応じて対応する必要があります。

* **有効化状態(Active)**: 定期決済コンテンツにアクセスでき、自動更新が進行中の状態
* **キャンセル(Cancelled)**: 定期決済コンテンツにアクセスできるが、ユーザーがサブスクリプション商品をこれ以上使用しないとしてキャンセルし、自動更新が停止された状態
* **猶予期間(In grace period)**: 支払い方法の問題により定期決済が失敗したが、定期決済コンテンツにはアクセスできる状態(ユーザーが支払い方法を変更するのを待つ状態)
* **アカウント保留(On hold)**: 支払い方法の問題により定期決済が失敗して保留中の状態(猶予期間が有効になっている場合、猶予期間中に支払い方法を変更しなかったため決済が保留された状態)
* **一時停止(Pause)**: 定期決済商品を一時的に停止した状態
* **期限切れ(Expired)**: 定期決済商品が期限切れになった状態

| 状態 | 未消費決済照会<br>(NhnCloudIap.queryConsumablePurchases) | 有効化されたサブスクリプション照会<br>(NhnCloudIap.queryActivatedPurchases) | 期限切れ時間 | 自動更新可否 |
| --- | ------------------------------------------------- | ------------------------------------------------- | ----- | -------- |
| 有効化状態(Active) | Yes | Yes | 未来時間 | Yes |
| キャンセル(Cancelled) | Yes | Yes | 未来時間 | No |
| 猶予期間(In grace period) | No | Yes | 未来時間 | Yes |
| アカウント保留(On hold) | No | No | 過去時間 | Yes |
| 一時停止(Pause) | No | No | 過去時間 | Yes |
| 期限切れ(Expired) | No | No | 過去時間 | No |

<a id="subscription-grace-period"></a>
#### 猶予期間

猶予期間が有効になっている場合、決済サイクルの終了時に決済手段に問題があると、定期決済は猶予期間に移行します。<br/>
<span style="color:#e11d21">猶予期間中、ユーザーは定期決済コンテンツにアクセスできる必要があります。</span>
詳細については、[猶予期間](https://developer.android.com/google/play/billing/subs#grace)を参照してください。

> <font color="red">[注意]</font><br/>
>
> * 猶予期間中に決済手段の変更などで復元された場合、自動更新を再開します。
> * NHN IAPは、更新された決済の結果を決済アップデートリスナー(IapService.PurchaseUpdatedListener)で通知します。
> * ゲームやアプリは、重要な動作中に決済アップデートリスナーが不要なポップアップをユーザーに表示しないように注意する必要があります。

##### 一般サブスクリプション商品(AUTO_RENEWABLE)

* 猶予期間中、一般サブスクリプション商品は定期決済コンテンツにアクセスできる必要があります。
* 猶予期間中は`NhnCloudIap.queryActivatedPurchases()`で照会できます。

##### 消費性サブスクリプション商品(CONSUMABLE_AUTO_RENEWABLE)

* 猶予期間が始まると、Googleは新しいレシートを発行しますが、決済手段を変更しない場合はアカウント保留状態になるか、キャンセルされます。
* 消費性サブスクリプション商品は、猶予期間中に商品を消費できないよう`NhnCloudIap.queryConsumablePurchases()`では照会されません。

<a id="subscription-account-hold"></a>
#### アカウント保留

アカウント保留とは、支払い方法の問題により更新が失敗したときのユーザー状態を指します。
決済に失敗すると猶予期間中に再試行し、猶予期間中も決済が失敗すると、定期決済の状態は保留状態になります。
アカウント保留状態のユーザーは、定期決済コンテンツにアクセスすることはできません。
アカウント保留期間は最大30日です。
アカウント保留期間が終了する前に支払い方法を修正しない場合、キャンセル処理されます。
詳細については、[アカウント保留](https://developer.android.com/google/play/billing/subs#account-hold)を参照してください。

> <font color="red">[注意]</font><br/>
>
> * アカウント保留期間中に支払い方法の修正などにより復元された場合、自動更新を再開します。
> * NHN IAPは、更新された決済の決済結果を決済更新リスナー(IapService.PurchaseUpdatedListener)で通知します。
> * ゲームやアプリは、重要な動作中に決済更新リスナーが不要なポップアップをユーザーに表示しないよう注意する必要があります。

##### 一般サブスクリプション商品(AUTO_RENEWABLE)

* アカウント保留期間中、一般サブスクリプション商品は定期決済コンテンツにアクセスすることはできません。
* アカウント保留期間中、`NhnCloudIap.queryActivatedPurchases()`で照会されません。

##### 消費性サブスクリプション商品(CONSUMABLE_AUTO_RENEWABLE)

* アカウント保留期間中、消費性サブスクリプション商品は新しい購入を作成しません。
* アカウント保留期間中、`NhnCloudIap.queryConsumablePurchases()`で新しい購入が照会されません。

<a id="subscription-pause"></a>
#### 一時停止

一時停止機能を設定すると、ユーザーが定期決済を1週間から3か月の間、一時停止できます。
定期決済の一時停止は、現在のサブスクリプション期間が終了した後に適用されます。
一時停止期間が終了すると、定期決済が自動的に再開されます。
詳細については、[一時停止](https://developer.android.com/google/play/billing/subs#pause)を参照してください。

> <font color="red">[注意]</font><br/>
>
> * 一時停止期間が終了すると、自動更新を再開します。
> * NHN IAPは、更新された決済の決済結果を決済アップデートリスナー(IapService.PurchaseUpdatedListener)で通知します。
> * ゲームやアプリは、重要な動作中に決済アップデートリスナーが不要なポップアップをユーザーに表示しないよう注意する必要があります。

##### 一般サブスクリプション商品(AUTO_RENEWABLE)

* 一時停止期間中、一般サブスクリプション商品は定期決済コンテンツにアクセスできません。
* 一時停止期間中、`NhnCloudIap.queryActivatedPurchases()`で照会されません。

##### 消費性サブスクリプション商品(CONSUMABLE_AUTO_RENEWABLE)

* 一時停止期間中、消費性サブスクリプション商品は新しい購入を生成しません。
* 一時停止期間中、`NhnCloudIap.queryConsumablePurchases()`で新しい購入が照会されません。

<a id="subscription-resubscribe"></a>
#### 定期決済の再申請

定期決済の再申請機能を設定すると、ユーザーが定期決済の期限切れ日から12か月以内にキャンセルした定期決済を再申請できます。
定期決済の再申請では、新しい定期決済および購入トークンが作成されます。
定期決済が期限切れになった後、ユーザーはGoogle Play定期決済センターで期限切れから1年まで同じ商品を再購入できます。
詳細については、[定期決済の再申請](https://developer.android.com/google/play/billing/subs#resubscribe)を参照してください。

> <font color="red">[注意]</font><br/>
>
> アプリやゲーム内の画面で購入が行われないため、ユーザーデータ(`IapPurchase.getDeveloperPayload()`)を使用できません。
> <font color="red">[注意]</font><br/>
>
> * Google Playストアで定期決済の再申請によりサブスクリプション商品を購入する場合、NHN IAPは購入した決済の決済結果を決済アップデートリスナー(IapService.PurchaseUpdatedListener)で通知します。
> * ゲームやアプリは、重要な動作中に決済アップデートリスナーが不要なポップアップをユーザーに表示しないよう注意する必要があります。

<a id="event-by-purchase"></a>
### Event by Purchase { #event-by-purchase }

Promotionコード入力により商品を取得した場合、またはPending決済(遅延決済、保護者の同意など)が完了した際、GamebaseEventHandlerを通じてイベントを受け取り、処理できます。
GamebaseEventHandlerでプロモーション決済及び遅延決済イベントを処理する方法は、以下のガイドをご確認ください。
[Game > Gamebase > Android SDK使用ガイド > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

<a id="error-handling"></a>
### Error Handling { #error-handling }

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                    | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックが完了していない状態でAPIが呼び出されました。    |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 該当商品が有効になっていません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 月の購入限度を超過しました。             |
| PURCHASE_PENDING                          | 4008       | 決済を完了するには追加の確認が必要です。 |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | このストアには対応していません。<br>選択可能なストアはGG(Google)、ONESTORE、GALAXY、HUAWEI, MYCARDです。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN IAPライブラリエラーです。<br/>詳細エラーを確認してください。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーはNHN IAPライブラリでエラーが発生した時に返されます。
* NHN IAPライブラリで発生したエラー情報は詳細エラーに含まれており、詳細なエラーコードおよびメッセージは次のように確認できます。

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

* NHN IAP SDKのエラーコードについては、次の項目を参照してください。
    * [NHN IAP SDKエラーコード](#nhn-iap-sdk-error-codes)

<a id="nhn-iap-sdk-error-codes"></a>
#### NHN IAP SDKエラーコード

* **PURCHASE_EXTERNAL_LIBRARY_ERROR**の詳細エラー(`exception.getDetailCode()`)として返されるNHN IAP SDKのエラーコードです。

##### 共通エラーコード

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| FEATURE_NOT_SUPPORTED | -2 | リクエストした機能はサポートされていません。<br>Requested feature is not supported. |
| SERVICE_DISCONNECTED | -1 | ストアサービスが接続されていません。<br>Store service is not connected. |
| OK | 0 | 成功。<br>Success. |
| USER_CANCELED | 1 | ユーザーキャンセル。<br>User canceled. |
| SERVICE_UNAVAILABLE | 2 | ネットワークが接続されていません。<br>Network connection is down. |
| BILLING_UNAVAILABLE | 3 | リクエストされたタイプのAPI Versionはサポートされていません。<br>API version is not supported for the type requested. |
| PRODUCT_UNAVAILABLE | 4 | リクエストした商品は使用できません。<br>Requested product is not available. |
| DEVELOPER_ERROR | 5 | 無効な引数がAPIに提供されました。開発段階で発生するエラーです。<br>Developer error. |
| ERROR | 6 | API操作中に重大なエラーが発生しました。<br>Fatal error during the API action. |
| PRODUCT_ALREADY_OWNED | 7 | すでに所持している商品のため、購入できませんでした。<br>Failure to purchase since item is already owned. |
| PRODUCT_NOT_OWNED | 8 | 所持していない商品のため、消費できません。<br>Failure to consume since item is not owned. |
| USER_ID_NOT_REGISTERED | 9 | ユーザーIDが登録されていません。<br>User ID Is not registered. |
| NETWORK_ERROR | 12 | ネットワークエラーが発生しました。<br>A network error occurred during the operation. |
| UNDEFINED_ERROR | 9999 | 定義されていないエラー<br>Undefined error. |

##### サーバーエラーコード

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| INACTIVATED_APP | 101 | 有効化されていないアプリです。<br>App is not active. |
| VERIFY_PURCHASE_FAILED | 103 | 決済検証に失敗しました。<br>Failure to verify purchase. |
| PURCHASE_ALREADY_CONSUMED | 104 | すでに消費済みの購入です。<br>Purchase already consumed. |
| PURCHASE_ALREADY_REFUNDED | 105 | 返金済みの購入です。<br>Purchase already refunded. |
| PURCHASE_LIMIT_EXCEEDED | 106 | 購入上限を超えました。<br>Purchase limit exceeded. |

##### ONE storeエラーコード

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| ONESTORE_NEED_LOGIN | 301 | ONE storeサービスにログインしていません。<br>ONE store service is not logged in. |
| ONESTORE_NEED_UPDATE | 302 | ONE storeサービスがアップデートまたはインストールされていません。<br>ONE store service is not updated or installed. |
| ONESTORE_SECURITY_ERROR | 303 | 異常なアプリから決済をリクエストしました。<br>Abnormal purchase request. |
| ONESTORE_PURCHASE_FAILED | 304 | 決済リクエストに失敗しました。<br>Purchase request failed. |

##### Galaxy storeエラーコード

| RESULT | CODE | DESC |
| ------ | ---- | ---- |
| GALAXY_NOT_LOGGED_IN | 501 | Galaxy storeサービスにログインしていません。<br>Galaxy service is not logged in. |
| GALAXY_NOT_UPDATED | 502 | Galaxy storeサービスがアップデートまたはインストールされていません。<br>Galaxy service is not updated or installed. |
| GALAXY_PURCHASE_FAILED | 503 | 異常なアプリから決済をリクエストしました。<br>Galaxy purchase failed. |
| GALAXY_SERVICE_DENIED | 504 | 決済リクエストに失敗しました。<br>PurGalaxy service denied. |
