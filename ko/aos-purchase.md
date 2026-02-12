## Game > Gamebase > Android SDK 사용 가이드 > 결제

여기에서는 앱에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Initialization

> <font color="red">[주의]</font><br/>
>
> ONE Store는 v17, v19, v21을 지원합니다.
> ONE Store는 버전별로 v17, v19, v21, external 중 하나만 사용이 가능하며, 동시에 사용이 불가능합니다.

* Gamebase 초기화 시 스토어 코드를 지정해야 합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
    * GG: Google Play
    * ONESTORE: ONE Store
    * GALAXY: Galaxy Store
    * HUAWEI: Huawei AppGallery
    * MYCARD: MyCard

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, callback);
```

### Purchase Flow

아이템 구매는 크게 **결제 Flow** 와 **[Consume Flow](./aos-purchase/#consume-flow)**, **[재처리 Flow](./aos-purchase/#retry-transaction-flow)** 로 나누어 볼 수 있습니다.
**결제 Flow**는 다음과 같은 순서로 구현하시기 바랍니다.

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 이전 결제가 정상적으로 종료되지 못한 경우 재처리가 동작하지 않으면 결제가 실패합니다. 그러므로 결제 전에 **requestItemListOfNotConsumed**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 Consume Flow 를 진행합니다.
2. 게임 클라이언트에서는 Gamebase SDK의 **requestPurchase**를 호출하여 결제를 시도합니다.
3. 결제가 성공하였다면 **requestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인한 후 지급할 아이템이 존재한다면 Consume Flow 를 진행합니다.

### Consume Flow

미소비 결제 내역 목록에 값이 있으면 다음과 같은 순서로 **Consume Flow** 를 진행하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 아이템이 중복 지급되는 일이 발생하지 않도록, 게임 서버에서 반드시 중복 지급 여부를 체크하시기 바랍니다.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
    * UserID, paymentSeq, purchaseToken을 전달합니다.
2. 게임 서버는 게임 DB에 이미 동일한 paymentSeq로 아이템을 지급한 이력이 있는지 확인합니다.
    * 2-1. 아직 아이템을 지급하지 않았다면 Gamebase 서버의 Payment Transaction API를 호출하여 purchaseToken이 유효한지, 응답 필드의 paymentSeq와 일치하는지 검증합니다.
        * [Game > Gamebase > API 가이드 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseToken이 서버 API 가이드 문서의 **accessToken**에 해당합니다.
    * 2-2. gamebaseProductId는 서버의 Payment Transaction API의 응답 필드에서 확인할 수 있습니다.
        * 클라이언트의 미소비 결제 내역 목록에도 gamebaseProductId가 존재하지만, 재처리 시에는 해당 값이 없을 수도 있으므로 서버의 Payment Transaction API로부터 획득한 gamebaseProductId 값을 사용하시기 바랍니다.
    * 2-3. Payment Transaction API 호출이 성공하여 purchaseToken이 정상이라고 확인되면 UserID에 gamebaseProductId에 해당하는 아이템을 지급합니다.
    * 2-4. 아이템 지급 후 게임 DB에 UserID, gamebaseProductId, paymentSeq, purchaseToken을 저장하여 중복 지급 방지 또는 재지급을 할 수 있도록 합니다.
3. 아이템 지급 여부와 무관하게 게임 서버는 더 이상 미소비 내역이 리턴되지 않도록 Gamebase 서버의 consume(소비) API를 호출하여 아이템 지급을 완료합니다.
    * [Game > Gamebase > API 가이드 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 스토어 결제에는 성공했으나 오류가 발생해 정상 종료되지 못하는 경우가 있습니다.
* **requestItemListOfNotConsumed**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 [Consume Flow](./aos-purchase/#consume-flow) 를 진행하세요.
* 재처리는 다음과 같은 시점에 호출할 것을 권장합니다.
    * 로그인 완료 후.
    * 결제 전.
    * 게임 내 상점(또는 로비) 진입시.
    * 유저 프로필 또는 우편함 확인시.

### Purchase Items

구매하고자 하는 아이템의 gamebaseProductId 를 이용해 다음의 API를 호출해 구매를 요청합니다.<br/>
gamebaseProductId 는 일반적으로는 스토어에 등록한 아이템의 id와 동일하지만, Gamebase 콘솔에서 변경할 수도 있습니다.

게임 유저가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 오류가 반환됩니다.
취소 처리를 해 주시기 바랍니다.

느린 결제나 부모 동의와 같이 결제 완료를 기다려야 하는 상황이 발생하는 경우에는 **GamebaseError.PURCHASE_PENDING** 오류가 반환됩니다.
이후에 결제가 정상적으로 완료되는 경우, GamebaseEventHandler에서 결제 완료 이벤트를 수신할 수 있습니다.
[Game > Gamebase > Android SDK 사용 가이드 > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

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
    // 구매한 아이템의 상품 ID입니다.
    @Nullable
    String gamebaseProductId;
    
    // Gamebase.Purchase.requestPurchase API 호출 시 payload로 전달했던 값입니다.
    // 스토어 서버 상태에 따라 정보가 유실되는 경우가 있으므로 사용을 권장하지 않습니다.
    @Nullable
    String payload;
    
    // 구매한 상품의 가격입니다.
    float price;
    
    // 통화 코드입니다.
    @NonNull
    String currency;
    
    // 결제 식별자입니다.
    // purchaseToken과 함께 'Consume' 서버 API를 호출하는 데 사용하는 중요한 정보입니다.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요!
    @NonNull
    String paymentSeq;
    
    // 결제 식별자입니다.
    // paymentSeq 와 함께 'Consume' 서버 API를 호출하는데 사용하는 중요한 정보입니다.
    // Consume API 에서는 'accessToken' 라는 이름의 파라메터로 전달해야 합니다.
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요!
    @Nullable
    String purchaseToken;
    
    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID입니다.
    @NonNull
    String marketItemId;
    
    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객 센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTO_RENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    @NonNull
    String productType;
    
    // 상품을 구매했던 User ID.
    // 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
    @NonNull
    String userId;
    
    // 스토어의 결제 식별자입니다.
    @Nullable
    String paymentId;
    
    // 상품을 구매했던 시각입니다.(epoch time)
    long purchaseTime;
    
    // 구독이 종료되는 시각입니다.(epoch time)
    long expiryTime;
    
    // Google 결제시 사용되는 값으로, 다음 값들이 올 수 있습니다.
    // 하지만 Google 서버에서 장애가 발생하여 Gamebase 결제 서버에서 일시적으로 검증 로직을 끄는 경우에는
    // null 로만 반환되므로 항상 유효한 값을 보장하지는 않음에 주의하시기 바랍니다.
    // * null : 일반 결제
    // * Test : 테스트 결제
    // * Promo : Promotion 결제
    @Nullable
    String purchaseType;
    
    // 구독 상품은 갱신될 때마다 paymentId가 변경됩니다.
    // 이 필드는 맨 처음 구독 상품을 결제 했을때의 paymentId 를 알려줍니다.
    // 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    // 항상 유효한 값을 보장하지는 않습니다.
    @Nullable
    String originalPaymentId;
    
    // itemSeq 로 상품을 구매하는 Legacy API용 식별자입니다.
    long itemSeq;
    
    // 상품을 구매했던 Store Code.
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

아이템 목록을 조회하려면 다음 API를 호출합니다. 콜백으로 반환되는 배열(array) 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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
    // Gamebase 콘솔에 등록된 상품 ID입니다.
    // Gamebase.Purchase.requestPurchase API로 상품을 구매할 때 사용됩니다.
    @Nullable
    String gamebaseProductId;
    
    // 상품의 가격입니다.
    float price;
    
    // 통화 코드입니다.
    @Nullable
    String currency;
    
    // Gamebase 콘솔에 등록된 상품 이름입니다.
    @Nullable
    String itemName;
    
    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID입니다.
    @NonNull
    String marketItemId;
    
    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객 센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTORENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    @NonNull
    String productType;
    
    // 통화 기호가 포함된 현지화 된 가격 정보입니다.
    @Nullable
    String localizedPrice;
    
    // 스토어 콘솔에 등록된 현지화된 상품 이름입니다.
    @Nullable
    String localizedTitle;
    
    // 스토어 콘솔에 등록된 현지화된 상품 설명입니다.
    @Nullable
    String localizedDescription;
    
    // Gamebase 콘솔에서 해당 상품의 '사용 여부'를 나타냅니다.
    boolean isActive;
    
    // itemSeq 로 상품을 구매하는 Legacy API용 식별자입니다.
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

* 아직 소비되지 않은 일회성 상품(CONSUMABLE)과 소비성 구독 상품(CONSUMABLE_AUTO_RENEWABLE) 정보를 조회합니다.
* 미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다.
* 정상적으로 결제가 완료되지 못한 경우 재처리의 역할도 하므로 다음 상황에서 호출해 주세요.
    * 게임 유저에게 지급되지 못한 아이템이 남아 있는지 확인
    	* 로그인 완료 후
    	* 게임 내 상점(또는 로비) 진입시
    	* 유저 프로필 또는 우편함 확인시
    * 재처리가 필요한 아이템이 있는지 확인
    	* 결제 전
    	* 결제 실패 후

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| newBuilder()                    | **M**                      | Configuration 객체 생성을 위한 Builder를 생성합니다.                                |
| build()                         | **M**                      | 설정을 마친 Builder를 Configuration 객체로 변환합니다.                                |
| setAllStores(boolean allStores) | O                          | 동일한 UserID로 다른 스토어에서 구매한 미소비 내역도 반환합니다.<br/>기본값은 **false**입니다. |

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

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
구독 수명 주기 처리는 다음 문서를 참조하시기 바랍니다.
[NHN Cloud > SDK 사용 가이드 > IAP > Android > Google Play Store 구독(정기 결제) 기능 > 구독 수명 주기 처리](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#subscription-lifecycle-handling)

> <font color="red">[주의]</font><br/>
>
> 현재 구독 상품은 Android의 경우 Google Play 스토어만 지원합니다.
>

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                               |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------- |
| newBuilder()                    | **M**                      | Configuration 객체 생성을 위한 Builder를 생성합니다.                            |
| build()                         | **M**                      | 설정을 마친 Builder를 Configuration 객체로 변환합니다.                           |
| setAllStores(boolean allStores) | O                          | 동일한 UserID로 다른 스토어에서 구매한 구독도 반환합니다.<br/>기본값은 **false**입니다.  |

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

현재 사용자 ID 기준으로 구입한 구독 상품의 상태를 조회할 수 있습니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
**PurchasableConfiguration.setIncludeExpiredSubscriptions(true)** API로 만료된 구독 상품의 상태도 조회할 수 있습니다.
구독 상태 코드는 다음 문서를 참조하시기 바랍니다.
[NHN Cloud > SDK 사용 가이드 > IAP > Android > NHN Cloud IAP Class Reference > IapSubscriptionStatus.StatusCode](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode)

> <font color="red">[주의]</font><br/>
>
> * 구독 상태 코드는 아래 가이드를 따라 구독 이벤트 설정을 해야 정상적으로 반환됩니다.
>     * [Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드 > Google 시스템 내 실시간 구독 정보 이벤트 전파 설정](./console-google-guide/#google_1)
>     * 이벤트 설정을 하지 않은 상태에서 구매한 구독 상품의 상태 코드는 항상 0(PURCHASED)이 반환됩니다.
> * 현재 구독 상품은 Google Play 스토어만 지원합니다.

**PurchasableConfiguration**

| API                                             | Mandatory(M) / Optional(O) | Description                              |
|-------------------------------------------------|----------------------------|------------------------------------------|
| newBuilder()                                    | **M**                      | Configuration 객체 생성을 위한 Builder를 생성합니다.  |
| build()                                         | **M**                      | 설정을 마친 Builder를 Configuration 객체로 변환합니다. |
| setIncludeExpiredSubscriptions(boolean include) | O                          | 만료된 구독 상품을 포함합니다.<br/>기본값은 **false**입니다. |

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
    // 구매한 아이템의 상품 ID입니다.
    @Nullable
    String gamebaseProductId;
    
    // 구독 상태 코드입니다.
    //
    // IapSubscriptionStatus.StatusCode : https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode
    public int statusCode;
    
    // 구독 상태 코드에 대한 설명입니다.
    @NonNull
    public String statusDescription;
    
    // 구매한 상품의 가격입니다.
    float price;
    
    // 통화 코드입니다.
    @NonNull
    String currency;
    
    // 결제 식별자입니다.
    // purchaseToken과 함께 'Consume' 서버 API를 호출하는 데 사용하는 중요한 정보입니다.
    //
    // Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의: Consume API는 게임 서버에서 호출하세요!
    @NonNull
    String paymentSeq;
    
    // 결제 식별자입니다.
    // paymentSeq와 함께 'Consume' 서버 API를 호출하는 데 사용하는 중요한 정보입니다.
    // Consume API에서는 'accessToken' 이라는 이름의 파라미터로 전달해야 합니다.
    //
    // Consume API: https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의: Consume API는 게임 서버에서 호출하세요!
    @NonNull
    String purchaseToken;
    
    // Google, Apple과 같이 스토어 콘솔에 등록된 상품 ID입니다.
    @NonNull
    String marketItemId;
    
    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN: 인식 불가능한 타입. Gamebase SDK를 업데이트하거나 Gamebase 고객 센터로 문의하세요.
    // * CONSUMABLE: 소비성 상품.
    // * AUTO_RENEWABLE: 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE: 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    @NonNull
    String productType;
    
    // 상품을 구매했던 User ID.
    // 상품을 구매하지 않은 User ID로 로그인하면 구매한 아이템을 획득할 수 없습니다.
    @NonNull
    String userId;
    
    // 상품을 구매했던 Store Code.
    @NonNull
    public String storeCode;
    
    // 스토어의 결제 식별자입니다.
    @Nullable
    String paymentId;
    
    // 상품을 구매했던 시각입니다.(epoch time)
    long purchaseTime;
    
    // 구독이 종료되는 시각입니다.(epoch time)
    long expiryTime;
    
    // Google 결제 시 사용되는 값으로, 다음 값들이 올 수 있습니다.
    // Google 서버에서 장애가 발생하여 Gamebase 결제 서버에서 일시적으로 검증 로직을 종료하는 경우
    // null로만 반환되므로 항상 유효한 값을 보장하지 않을 수 있습니다.
    // * null: 일반 결제
    // * Test: 테스트 결제
    // * Promo: Promotion 결제
    @Nullable
    String purchaseType;
    
    // 구독 상품은 갱신될 때마다 paymentId가 변경됩니다.
    // 이 필드는 구독 상품을 최초 결제했을 때의 paymentId를 알려줍니다.
    // 스토어 및 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    // 항상 유효한 값을 보장하지는 않습니다.
    @Nullable
    String originalPaymentId;
    
    // itemSeq로 상품을 구매하는 Legacy API용 식별자입니다.
    long itemSeq;
    
    // Gamebase.Purchase.requestPurchase API 호출 시 payload로 전달했던 값입니다.
    // 스토어 서버 상태에 따라 정보가 유실되는 경우가 있으므로 사용을 권장하지 않습니다.
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

### Promotional Events

Promotion 코드 입력을 통해 상품을 획득한 경우 또는 Pending 결제(느린 결제, 부모 동의 등)가 완료되었을 때 GamebaseEventHandler 를 통해 이벤트를 받아 처리할 수 있습니다.
GamebaseEventHandler 로 프로모션 결제 및 지연 결제 이벤트를 처리하는 방법은 아래 가이드를 확인하세요.
[Game > Gamebase > Android SDK 사용 가이드 > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해주세요. |
| PURCHASE_USER_CANCELED                    | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다.     |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 해당 상품이 활성화 상태가 아닙니다.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 존재하지 않는 GamebaseProductID 로 결제를 요청하였습니다. |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 월 구매 한도를 초과했습니다.             |
| PURCHASE_PENDING                          | 4008       | 결제를 완료하려면 추가 확인이 필요합니다. |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), ONESTORE, GALAXY, HUAWEI, MYCARD입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAP 라이브러리 오류입니다.<br/>상세 오류를 확인하십시오. |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud IAP 라이브러리에서 오류가 발생했을 때 반환됩니다.
* NHN Cloud IAP 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 

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

* NHN Cloud IAP SDK 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [NHN Cloud > NHN Cloud SDK 사용 가이드 > NHN Cloud IAP > Android > 오류 코드](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-android/#error-codes)

