## Game > Gamebase > Android SDK 사용 가이드 > 결제

여기에서는 앱에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Initialization

* Gamebase 초기화 시 스토어 코드를 지정해야 합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
    * GG: Google Store
    * ONESTORE: ONE Store
    * GALAXY: Galaxy Store

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

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
    * UserID, gamebaseProductId, paymentSeq, purchaseToken 을 전달합니다.
2. 게임 서버는 게임 DB 에 이미 동일한 paymentSeq 로 아이템을 지급한 이력이 있는지 확인합니다.
    * 2-1. 아직 아이템을 지급하지 않았다면 UserID 에 gamebaseProductId 에 해당하는 아이템을 지급합니다.
    * 2-2. 아이템 지급 후 게임 DB 에 UserID, gamebaseProductId, paymentSeq, purchaseToken 을 저장하여 중복 지급 방지 또는 재지급을 할 수 있도록 합니다.
3. 아이템 지급 여부와 무관하게 게임 서버는 Gamebase 서버의 consume(소비) API를 호출하여 아이템 지급을 완료합니다.
    * [Game > Gamebase > API 가이드 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 스토어 결제에는 성공했으나 오류가 발생해 정상 종료되지 못하는 경우가 있습니다.
* **requestItemListOfNotConsumed**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 [Consume Flow](./aos-purchase/#consume-flow) 를 진행하세요.
* 재처리는 다음과 같은 시점에 호출할 것을 권장합니다.
    * 로그인 완료 후.
    * 결제 전.
    * 게임 내 상점(또는 로비) 진입시.
    * 유저 프로필 또는 우편함 확인시.

### Purchase Item

구매하고자 하는 아이템의 gamebaseProductId 를 이용해 다음의 API를 호출해 구매를 요청합니다.<br/>
gamebaseProductId 는 일반적으로는 스토어에 등록한 아이템의 id와 동일하지만, Gamebase 콘솔에서 변경할 수도 있습니다.
payload 필드에 입력한 추가 정보는 결제 성공 후 **PurchasableReceipt.payload** 필드에 유지되므로 여러가지 용도로 활용할 수 있습니다.<br/>
게임 유저가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 오류가 반환됩니다.
취소 처리를 해 주시기 바랍니다.

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

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
사용자 ID가 같다면 Android와 iOS에서 구매한 구독 상품이 모두 조회됩니다.

> <font color="red">[주의]</font><br/>
>
> 현재 구독 상품은 Android의 경우 Google Play 스토어만 지원합니다.
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

프로모션 결제가 완료되었을때 GamebaseEventHandler 를 통해 이벤트를 받아 처리할 수 있습니다.
GamebaseEventHandler 로 프로모션 결제 이벤트를 처리하는 방법은 아래 가이드를 확인하세요.
[Game > Gamebase > Android SDK 사용 가이드 > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해주세요. |
| PURCHASE_USER_CANCELED                   | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다.     |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | 해당 상품이 활성화 상태가 아닙니다.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | 존재하지 않는 GamebaseProductID 로 결제를 요청하였습니다. |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), ONESTORE 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP 라이브러리 오류입니다.<br>DetailCode를 확인하세요.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 TOAST IAP SDK 에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

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

* TOAST IAP SDK 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [TOAST > TOAST SDK 사용 가이드 > TOAST IAP > Android > 오류 코드](/TOAST/ko/toast-sdk/iap-android/#_24)

