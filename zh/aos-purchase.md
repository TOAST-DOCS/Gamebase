## Game > Gamebase > Android SDK使用指南> 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Initialization

> <font color="red">[注意]</font><br/>
>
> ONE Store仅支持v17。
> 目前不支持ONE Store v19，但仍在审查是否支持它。

* 初始化Gamebase时，需要指定**STORE_CODE**。
* 请您在以下值中选择**STORE_CODE**。 
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

购买道具的程序大体分为**结算Flow**、**[Consume Flow](./aos-purchase/#consume-flow)**及**[Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)**。
请按以下顺序实现**结算Flow**。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 未能正常结束上一次支付时，若不进行‘’支付再处理”则将导致支付失败。因此支付前应调用**requestItemListOfNotConsumed**进行‘’支付再处理”，
   如果存在未提供的道具则进行Consume Flow。
2. 游戏客户端通过从Gamebase SDK调用**requestPurchase**进行付款。 
3. 如果付款成功，请调用requestItemListOfNotConsumed查看未消费结算明细。若存在未提供的道具，则进行Consume Flow。

### Consume Flow

如果返还的未消费结算明细列表中存在值，请按以下顺序进行**Consume Flow**。

> <font color="red">[注意]</font><br/>
>
> 为了防止重复提供道具，必须通过游戏服务器确认是否重复提供道具。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. 游戏客户端向游戏服务器请求consume（消费）。
    * 传送UserID、gamebaseProductId、paymentSeq、purchaseToken。
2. 游戏服务器查看在游戏DB中是否存在以同样的paymentSeq提供道具的历史记录。
    * 2-1. 若存在未提供道具，则需向UserID提供以gamebaseProductId购买的道具。
    * 2-2. 提供道具后在游戏DB保存UserID、gamebaseProductId、paymentSeq、purchaseToken，必要时进行‘’支付再处理”或防止重复提供。
3. 游戏服务器通过调用Gamebase服务器的consume（消费）API提供道具。这时不考虑是否已提供道具。
    * [Game > Gamebase > API指南 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 商店支付已成功，但因出现错误无法正常终止时，
* 请调用**requestItemListOfNotConsumed**进行‘’支付再处理”。若存在尚未提供的道具，则进行[Consume Flow](./aos-purchase/#consume-flow)。
* 建议在以下情况下调用‘’支付再处理”。
    * 完成登录后
    * 支付之前
    * 进入游戏内商店（或 Lobby）时
    * 查询用户简介或邮箱时

### Purchase Item

使用想要购买商品的gamebaseProductId调用以下API请求购买。<br/>
gamebaseProductId与在商店注册的道具id相同，但可在Gamebase Console中更改。
支付后在payload field中输入的附加信息（将一直留在**PurchasableReceipt.payload**field）可用于多种用途。<br/>
用户取消购买时，返还**GamebaseError.PURCHASE_USER_CANCELED**错误代码。
请进行取消处理。

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

**示例**

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

要查询商品列表，请调用以下API。回调返还的数组(array)包含各item的信息。

**API**

```java
+ (void)Gamebase.Purchase.requestItemListPurchasable(Activity activity, GamebaseDataCallback<List<PurchasableItem>> callback);
```

**示例**

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

* 查询尚未消费的一次性商品(CONSUMABLE)与消费性订阅商品(CONSUMABLE_AUTO_RENEWABLE) 信息。
* 如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。
* 未能完成支付时，也起到‘’支付再处理”的作用。可在下列情况下调用该函数。
    * 查看是否存在未提供的道具。
    	* 完成登录后 
    	* 进入游戏内商店（或 Lobby） 时
    	* 查询用户简介或邮箱时
    * 查看需要进行‘’支付再处理”的道具。
    	* 支付之前
    	* 支付失败后

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**示例**

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

以当前用户ID为准查询激活的订阅列表。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。 
若用户ID相同，可同时查询在Android和iOS中购买的订阅商品。

> <font color="red">[注意]</font><br/>
>
> 当前订阅商品为Android的商品时，仅支持Google Play商店。
>

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**示例**

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

完成Promotion支付后，可通过GamebaseEventHandler接收Event并进行处理。 
关于使用GamebaseEventHandler处理Promotion支付Event的方法，请参考以下指南。
[Game > Gamebase > Android SDK 使用指南 > ETC > Gamebase Event Handler](./aos-etc/#purchase-updated) 

### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | 未初始化Purchase模块。请确认是否将<br>gamebase-adapter-purchase-IAP模块添加到项目中。 |
| PURCHASE_USER_CANCELED                    | 4002       | 游戏用户已取消购买商品。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 尚未完成购买逻辑的情况下已调用API。    |
| PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 此商店的余额不足，无法结算。           |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 此商品为非激活状态。 |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 请求支付的GamebaseProductID不存在。 |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 超过了一个月购买限额。             |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | 不支持的商店<br>可选择的商店是GG(Google)、ONESTORE及GALAXY。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | IAP库错误<br>请确认DetailCode。   |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 未知的购买错误<br>请将完整的Log上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 这是在TOAST IAP SDK模块中发生的错误。
* 检查错误代码的方法如下。

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

* TOAST IAP SDK错误代码，请参考以下文档。
    * [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud IAP > Android > 错误代码](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-android/#error-codes)

