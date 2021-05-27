## Game > Gamebase > Android SDK ご利用ガイド > 決済

ここではアプリでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。

Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連動することができるようサポートします。

### Settings

#### 1. Store Console

* 次のIAPガイドをご参考の上、各ストアにアプリを登録してアプリケーションキーを発行してもらいます。
	* [Game > Gamebase > ストアコンソールガイド > Googleコンソールガイド](./console-google-guide)
	* [Game > Gamebase > ストアコンソールガイド > ONEStoreコンソールガイド](./console-onestore-guide)

#### 2. Register as Store's Tester

* 決済テストのため、次のようにストアごとにテスター登録をします。
    * Google
        * [Android > テスト購入設定](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > アプリ内決済テスト](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * 必ずアプリ内情報 - テストボタンでサンドボックスを希望する端末の電話番号を登録してテストを行う必要があります。
        * テスト用の端末にはUSIMが必要であり、電話番号を登録しなければなりません(MDN)。
        * **ONE store**のアプリケーションがインストールされている必要があります。

#### 3. Register Item

* 下記のガイドを参考にしてアイテムを登録します。
    * [Game > Gamebase > コンソール使用ガイド > 決済 > Register](./oper-purchase/#register_1)

#### 4. Setting SDK

* 使用するマーケットのgamebase-adapter-purchaseモジュールをgradle依存性に追加します。

```groovy
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'
    
    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
}
```

#### 5. AndroidManifest.xml(ONE store only)

* ONE storeを使用するためには、次の設定を追加する必要があります。

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!—development:開発モード / release:運営 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

### Initialization

* Gamebaseの初期化時、ストアコードを指定する必要があります。
* **STORE_CODE**は、次の値の中から選択します。
    * GG：Google
    * ONESTORE：ONEstore
    * GALAXY: Galaxy Store

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

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. ゲームクライアントがゲームサーバーに決済アイテムのconsume(消費)をリクエストします。
    * UserID、gamebaseProductId、paymentSeq、purchaseTokenを伝達します。
2. ゲームサーバーは、ゲームDBにすでに同じpaymentSeqでアイテムを支給した履歴があるかを確認します。
    * 2-1まだアイテムを支給していない場合、UserIDにgamebaseProductIdに該当するアイテムを支給します。
    * 2-2アイテム支給後、ゲームDBにUserID、gamebaseProductId、paymentSeq、purchaseTokenを保存し、後で重複支給の有無を確認できるようにします。
3. ゲームサーバーはGamebaseサーバーのconsume(消費) APIを呼び出してアイテムの支給を完了します。
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

### Purchase Item

購入するアイテムのgamebaseProductIdを利用して次のAPIを呼び出し、購入をリクエストします。<br/>
gamebaseProductIdは一般的にはストアに登録したアイテムのIDと同じですが、Gamebaseコンソールで変更することもできます。
payloadフィールドに入力した追加情報は決済成功後、**PurchasableReceipt.payload**フィールドに維持されるため、複数の用途で活用できます。<br/>
ゲームユーザーが購入をキャンセルすると、**GamebaseError.PURCHASE_USER_CANCELED**エラーが返ります。
キャンセル処理をしてください。

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final String payload,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
// Legacy API
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          final long itemSeq,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

```java
String userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, userPayload, new GamebaseDataCallback<PurchasableReceipt>() {
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
    // 구매한 아이템의 상품 ID 입니다.
    @Nullable
    String gamebaseProductId;
    
    // Gamebase.Purchase.requestPurchase API 호출시 payload 로 전달했던 값입니다.
    //
    // 이 필드는 예를 들어 동일한 User ID 로 구매 했음에도 게임 채널, 캐릭터 등에 따라
    // 상품 구매 및 지급을 구분하고자 하는 경우 등
    // 게임에서 필요로 하는 다양한 추가 정보를 담기 위한 목적으로 활용할 수 있습니다.
    @Nullable
    String payload;
    
    // 구매한 상품의 가격 입니다.
    float price;
    
    // 통화 코드 입니다.
    @NonNull
    String currency;
    
    // 결제 식별자입니다.
    // purchaseToken 과 함께 'Consume' 서버 API 를 호출하는데 사용하는 중요한 정보입니다.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요!
    @NonNull
    String paymentSeq;
    
    // 결제 식별자입니다.
    // paymentSeq 와 함께 'Consume' 서버 API 를 호출하는데 사용하는 중요한 정보입니다.
    // Consume API 에서는 'accessToken' 라는 이름의 파라메터로 전달해야 합니다.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요!
    @Nullable
    String purchaseToken;
    
    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID 입니다.
    @NonNull
    String marketItemId;
    
    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTO_RENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    @NonNull
    String productType;
    
    // 상품을 구매했던 User ID.
    // 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
    @NonNull
    String userId;
    
    // 스토어의 결제 식별자 입니다.
    @Nullable
    String paymentId;
    
    // 상품을 구매했던 시각입니다.(epoch time)
    long purchaseTime;
    
    // 구독이 종료되는 시각입니다.(epoch time)
    long expiryTime;
    
    // Google 결제시 사용되는 값으로, 다음 값들이 올 수 있습니다.
    // 하지만 Google 서버에서 장애가 발생하여 Gamebase 결제 서버에서 일시적으로 검증 로직을 끄는 경우에는
    // null 로만 리턴되므로 항상 유효한 값을 보장하지는 않음에 주의하시기 바랍니다.
    // * null : 일반 결제
    // * Test : 테스트 결제
    // * Promo : Promotion 결제
    @Nullable
    String purchaseType;
    
    // 구독 상품은 갱신 될때마다 paymentId 가 변경됩니다.
    // 이 필드는 맨 처음 구독 상품을 결제 했을때의 paymentId 를 알려줍니다.
    // 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    // 항상 유효한 값을 보장하지는 않습니다.
    @Nullable
    String originalPaymentId;
    
    // itemSeq 로 상품을 구매하는 Legacy API용 식별자 입니다.
    long itemSeq;
}
```

**Response Example**

```json
{
    "gamebaseProductId": "my_product_001",
    "payload": "UserPayload:!@#...",
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
    "payload": "MyData:{\"1234\":\"5678\"}",
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
    // Gamebase 콘솔에 등록된 상품 ID 입니다.
    // Gamebase.Purchase.requestPurchase API 로 상품을 구매할때 사용됩니다.
    @Nullable
    String gamebaseProductId;
    
    // 상품의 가격 입니다.
    float price;
    
    // 통화 코드 입니다.
    @Nullable
    String currency;
    
    // Gamebase 콘솔에 등록된 상품 이름입니다.
    @Nullable
    String itemName;
    
    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID 입니다.
    @NonNull
    String marketItemId;
    
    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTORENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    @NonNull
    String productType;
    
    // 통화 기호가 포함된 현지화 된 가격 정보입니다.
    @Nullable
    String localizedPrice;
    
    // 스토어 콘솔에 등록된 현지화된 상품 이름 입니다.
    @Nullable
    String localizedTitle;
    
    // 스토어 콘솔에 등록된 현지화된 상품 설명 입니다.
    @Nullable
    String localizedDescription;
    
    // Gamebase 콘솔에서 해당 상품의 '사용 여부'를 나타냅니다.
    boolean isActive;
    
    // itemSeq 로 상품을 구매하는 Legacy API용 식별자 입니다.
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

### List Activated Subscriptions

現在のユーザーID基準で有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、有効期限が切れる前まで照会できます。
ユーザーIDが同じ場合、AndroidとiOSで購入した定期購入商品が全て照会されます。

> <font color="red">[注意]</font><br/>
>
> 現在定期購入商品は、Androidの場合はGoogle Playストアのみサポートします。
>

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
Gamebase.Purchase.requestActivatedPurchases(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
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

### Event by Promotion

プロモーション決済が完了すると、GamebaseEventHandlerを通してイベントを取得して処理できます。
GamebaseEventHandlerでプロモーション決済イベントを処理する方法は、下記のガイドを参照してください。
[Game > Gamebase > Android SDK使用ガイド > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 購入ロジックが完了していない状態でAPIが呼び出されました。    |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | 該当商品が有効になっていません。  |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | 存在しないGamebaseProductIDで決済をリクエストしました。 |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | このストアには対応していません。<br>選択可能なストアはGG(Google)、ONESTORE、GALAXYです。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリーエラーです。<br>DetailCodeを確認してください。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud IAP SDKで発生したエラーです。
* IAPーエラーの詳細は次のように確認できます。

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
    * [NHN Cloud > NHN Cloud SDK使用ガイド > NHN Cloud IAP > Android > エラーコード](/TOAST/ja/toast-sdk/iap-android/#_24)

