## Game > Gamebase > Android SDK使用指南 > 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Initialization

> <font color="red">[注意]</font><br/>
>
> ONE Store仅支持v17和v19。
> 对ONE Store v21的支持正在审核中，目前不支持。
> ONE Store按每个版本只能使用v17、v19或external中的一个，并且不能同时使用。

* 初始化Gamebase时，需要指定**STORE_CODE**。
* 请您在以下值中选择**STORE_CODE**。 
    * GG : Google Play
    * ONESTORE : ONE Store
    * GALAXY : Galaxy Store
    * AMAZON : Amazon Appstore
    * HUAWEI : Huawei AppGallery
    * MYCARD: MyCard

    
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

1. 未能正常结束上一次支付时，若不进行“支付再处理”则将导致支付失败。因此支付前应调用**requestItemListOfNotConsumed**进行“支付再处理”，
   如果存在未提供的道具则进行Consume Flow。
2. 游戏客户端通过从Gamebase SDK调用**requestPurchase**进行付款。 
3. 如果付款成功，请调用requestItemListOfNotConsumed查看未消费结算明细。若存在未提供的道具，则进行Consume Flow。

### Consume Flow

如果返还的未消费结算明细列表中存在值，请按以下顺序进行**Consume Flow**。

> <font color="red">[注意]</font><br/>
>
> 为了防止重复提供道具，必须通过游戏服务器确认是否重复提供道具。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. 游戏客户向游戏服务器请求consume（消费）。
    * 传送UserID、gamebaseProductId、paymentSeq、purchaseToken。
2. 游戏服务器查看在游戏DB中是否存在以同样的paymentSeq提供道具的历史记录。
    * 2-1. 如果未提供道具则调用Gamebase服务器的Payment Transaction API，验证paymentSeq和purchaseToken值​​是否有效。
        * [Game > Gamebase > API指南 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. purchaseToken为正常的值时，向UserID提供使用gamebaseProductId购买的道具。 
    * 2-3. 提供道具后在游戏DB中保存UserID、gamebaseProductId、paymentSeq和purchaseToken，防止重复提供或允许重新提供道具。
3. 游戏服务器通过调用Gamebase服务器的consume（消费）API提供道具。这时不考虑是否已提供道具。
    * [Game > Gamebase > API指南 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 商店支付已成功，但因出现错误无法正常终止时，
* 请调用**requestItemListOfNotConsumed**进行“支付再处理”。若存在尚未提供的道具，则进行[Consume Flow](./aos-purchase/#consume-flow)。
* 建议在以下情况下调用“支付再处理”。
    * 完成登录后
    * 支付之前
    * 进入游戏内商店（或 Lobby）时
    * 查询用户简介或邮箱时

### Purchase Item

使用想要购买商品的gamebaseProductId调用以下API请求购买。<br/>
gamebaseProductId与在商店注册的道具id相同，但可在Gamebase Console中更改。

用户取消购买时，返还**GamebaseError.PURCHASE_USER_CANCELED**错误代码。
请进行取消处理。

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(@NonNull final Activity activity,
                                          @NonNull final String gamebaseProductId,
                                          @NonNull final GamebaseDataCallback<PurchasableReceipt> callback);
```

**示例**

```java
String userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
Gamebase.Purchase.requestPurchase(activity, gamebaseProductId, userPayload, new GamebaseDataCallback<PurchasableReceipt>() {
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
    // 是购买的道具的商品ID。
    @Nullable
    String gamebaseProductId;
            
    // 是调用Gamebase.Purchase.requestPurchase API时作为payload传送的值。
    // 我们不建议使用，因为根据商店服务器的状态，信息可能会丢失。    
    @Nullable
    String payload;
    
    // 是购买的商品的价格。
    float price;
    
    // 是货币代码。
    @NonNull
    String currency;
    
    // 是结算标识符。
    // 是与purchaseToken调用“Consume”服务器API时使用的重要信息。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意 : 请在游戏服务器调用Consume API!
    @NonNull
    String paymentSeq;
    
    // 是结算标识符。   
    // 是与paymentSeq调用“Consume”服务器API时使用的重要信息。在Consume API应将名称作为“accessToken”传送。
    // 在Consume API应将名称作为“accessToken”传送。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意 : 请在游戏服务器调用Consume API!
    @Nullable
    String purchaseToken;
    
    // 是在商店控制台中注册的产品ID，如Google和Apple 。
    @NonNull
    String marketItemId;
    
    // 作为产品类型，存在以下值。
    // * UNKNOWN : 无法识别类型/请更新Gamebase SDK或联系Gamebase客户服务。
    // * CONSUMABLE : 消费型商品   
    // * AUTO_RENEWABLE : 订阅型商品
    // * CONSUMABLE_AUTO_RENEWABLE : 当您想向购买订阅型产品的用户定期提供可消费商品时，使用“可消费订阅商品”。
    @NonNull
    String productType;
    
    // 购买商品的User ID
    // 如果您使用没有购买商品的User ID登录，则无法获取购买的道具。
    @NonNull     
    String userId;
    
    // 为商店的结算标识符。
    @Nullable
    String paymentId;
    
    // 为购买商品的时间。(epoch time)
    long purchaseTime;
    
    // 为订阅结束的时间。(epoch time)
    long expiryTime;
    
    // 是用于Google支付的值。以下值​​可以使用。
    // 但在Google服务器出现故障而在Gamebase结算服务器暂时关闭验证逻辑时，
    // 仅返还为null，因此要注意不始终保证有效的值。 
    // * null : 一般付款
    // * Test : 测试付款
    // * Promo : Promotion付款
    @Nullable
    String purchaseType;
    
    // 每当订阅商品被更新时paymentId也将被更新。 
    // 通过此字段可以确认首次支付订阅产品时使用的paymentId。
    // 根据商店类型或结算服务器状态可能不存在值，
    // 因此不始终保证有效的值。
    @Nullable
    String originalPaymentId;
    
    // 是通过itemSeq购买商品的Legacy API专用标识符。
    long itemSeq;
    
    // 购买过商品的Store Code
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

**VO**

```java
class PurchasableItem {
    // 是在Gamebase控制台中注册的商品ID。
    // 当调用Gamebase.Purchase.requestPurchase API购买商品时使用它。
    @Nullable
    String gamebaseProductId;
    
    // 是商品的价格。
    float price;
    
    // 是货币代码。
    @Nullable
    String currency;
    
    // 是在Gamebase控制台中注册的商品名称。
    @Nullable
    String itemName;
    
    // 是在商店控制台中注册的产品ID，如Google或Apple。
    @NonNull
    String marketItemId;
    
    // 为商品类型，存在以下值。
    // * UNKNOWN : 无法识别类型/请更新Gamebase SDK或联系Gamebase客户服务。
    // * CONSUMABLE : 消费型商品
    // * AUTORENEWABLE : 订阅型商品
    // * CONSUMABLE_AUTO_RENEWABLE : 当您想向购买订阅型产品的用户定期提供可消费商品时使用“可消费订阅商品”。
    @NonNull
    String productType;
    
    // 是包含货币符号的当地价格信息。 
    @Nullable
    String localizedPrice;
    
    // 是在商店控制台中注册的当地商品名称。
    @Nullable
    String localizedTitle;
    
    // 是在商店控制台中注册的当地商品说明。
    @Nullable
    String localizedDescription;
    
    // 在Gamebase控制台中显示此商品的“使用与否”。
    boolean isActive;
    
    // 是通过itemSeq购买商品的Legacy API专用标识符。 
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

* 查询尚未消费的一次性商品(CONSUMABLE)与消费性订阅商品(CONSUMABLE_AUTO_RENEWABLE) 信息。
* 如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。
* 未能完成支付时，也起到“支付再处理”的作用。可在下列情况下调用该函数。
    * 查看是否存在未提供的道具。
    	* 完成登录后 
    	* 进入游戏内商店（或 Lobby） 时
    	* 查询用户简介或邮箱时
    * 查看需要进行“支付再处理”的道具。
    	* 支付之前
    	* 支付失败后

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| newBuilder()                    | **M**                      | 创建Builder以创建Configuration对象。                                |
| build()                         | **M**                      | 将已设置的Builder转换为Configuration对象。                                |
| setAllStores(boolean allStores) | O                          | 还返还使用相同的UserID在其他商店购买的未消费明细。<br/>默认值为**false**。 |

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(@NonNull final Activity activity,
                                                       @NonNull final PurchasableConfiguration configuration,
                                                       @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**示例**

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

以当前用户ID为准查看激活的订阅列表。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。
关于“订阅寿命周期处理”，请参考以下文件。
[NHN Cloud > SDK使用指南 > IAP > Android > Google Play Store订阅(定期支付)功能 > 订阅寿命周期处理](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#subscription-lifecycle-handling) 

> <font color="red">[注意]</font><br/>
>
> 当前订阅商品为Android商品时，仅支持Google Play商店。
>

**PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                               |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------- |
| newBuilder()                    | **M**                      | 创建Builder以创建Configuration对象。                            |
| build()                         | **M**                      | 将已设置的Builder转换为Configuration对象。                           |
| setAllStores(boolean allStores) | O                          | 还返还使用相同的UserID在其他商店购买的订阅。<br/>默认值为**false**。  |

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(@NonNull final Activity activity,
                                                    @NonNull final PurchasableConfiguration configuration,
                                                    @NonNull final GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**示例**

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

以当前用户ID为准查看购买的订阅商品的状态。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。
通过调用**PurchasableConfiguration.setIncludeExpiredSubscriptions(true)** API可查询已到期的订阅商品的状态。 
关于订阅状态代码，请参考以下文件。
[NHN Cloud > SDK使用指南 > IAP > Android > NHN Cloud IAP Class Reference > IapSubscriptionStatus.StatusCode](https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode)

> <font color="red">[注意]</font><br/>
>  
> * 订阅状态代码通常仅在您根据以下指南设置订阅事件时返回。
>     * [Game > Gamebase > 商店控制台指南 > Google控制台指南 > Google系统内实时订阅信息事件传播 设置](./console-google-guide/#google_1)
>     * 对于未进行事件设置的状态下购买的订阅商品，始终返回状态代码0（PURCHASED）。
> * 当前的订阅商品仅支持Google Play商店。

**PurchasableConfiguration**

| API                                             | Mandatory(M) / Optional(O) | Description                              |
|-------------------------------------------------|----------------------------|------------------------------------------|
| newBuilder()                                    | **M**                      | 创建Builder以创建Configuration对象。  |
| build()                                         | **M**                      | 将已设置的Builder转换为Configuration对象。 |
| setIncludeExpiredSubscriptions(boolean include) | O                          | 包含已到期的订阅商品。<br/>默认值为**false**。 |

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
    // 是购买的道具的商品ID。
    @Nullable
    String gamebaseProductId;
    
    // 是订阅状态代码。
    //
    // IapSubscriptionStatus.StatusCode : https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-android/#iapsubscriptionstatusstatuscode
    public int statusCode;
    
    // 是有关订阅状态代码的说明。
    @NonNull
    public String statusDescription;
    
    // 是购买的商品的价格。 
    float price;
    
    // 是货币代码。
    @NonNull
    String currency;
    
    // 是结算标识符。
    // 是与purchaseToken调用“Consume”服务器API时使用的重要信息。
    // 
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意 : 请在游戏服务器调用Consume API!
    @NonNull
    String paymentSeq;
    
    // 是结算标识符。
    // 是与paymentSeq调用“Consume”服务器API时使用的重要信息。
    // 在Consume API应将名称作为“accessToken”传送。
    //
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 注意 : 请在游戏服务器调用Consume API!
    @NonNull
    String purchaseToken;
    
    // 是在商店控制台中注册的产品ID，如Google和Apple 。
    @NonNull
    String marketItemId;
    
    // 作为商品类型，都有以下值。
    // * UNKNOWN : 无法识别类型/请更新Gamebase SDK或联系Gamebase客户服务。
    // * CONSUMABLE : 消费型商品
    // * AUTO_RENEWABLE : 订阅型商品
    // * CONSUMABLE_AUTO_RENEWABLE : 当您想向购买订阅型产品的用户定期提供可消费商品时使用“可消费订阅商品”。
    @NonNull  
    String productType;
    
    // 购买商品的User ID 
    // 如果您使用没有购买商品的User ID登录，则无法获取购买的道具。
    @NonNull  
    String userId;
    
    // 是购买过商品的Store Code。
    @NonNull
    public String storeCode;
    
    // 是商店的结算标识符。
    @Nullable
    String paymentId;
    
    // 是购买商品的时间。(epoch time)
    long purchaseTime;
    
    // 是订阅结束的时间。(epoch time)
    long expiryTime;
    
    // Google支付时使用的值如下。
    // 但要注意由于Google服务器出现故障，而要占时关闭Gamebase结算服务器时，
    // 将始终返还为null，不始终保障有效的值。 
    // * null : 一般支付
    // * Test : 测试支付
    // * Promo : Promotion支付
    @Nullable
    String purchaseType;
    
    // 每当更新订阅产品时，paymentId也会更改。
    // 通过此字段可以确认首次支付订阅商品时使用的paymentId。 
    // 根据商店类型和结算服务器状态可能不存在值，
    // 因此不始终保障有效的值。
    @Nullable
    String originalPaymentId;
    
    // 是通过itemSeq购买商品的Legacy API专用标识符。 
    long itemSeq;
    
    // 是调用Gamebase.Purchase.requestPurchase API时传送至payload的值。 
    // 根据商店服务器状态，信息可能会被流失，因此不建议使用。
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
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | 不支持的商店<br>可选择的商店是GG(Google)、ONESTORE、GALAXY、AMAZON及HUAWEI。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | 是NHN Cloud IAP库错误。<br/>请确认详细错误。 |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 未知的购买错误<br>请将完整的Log上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 当NHN Cloud IAP库中发生错误时，将返回此错误。
* 在NHN Cloud IAP库中发生的错误信息包含在详细错误中，而详细错误代码和消息如下。

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

* NHN Cloud IAP SDK 错误代码，请参考以下文档。
    * [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud IAP > Android > 错误代码](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-android/#error-codes)

