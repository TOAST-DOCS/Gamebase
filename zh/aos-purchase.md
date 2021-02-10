## Game > Gamebase > Android SDK使用指南> 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Settings

#### 1. Store Console

* 请参考以下IAP指南，在每个商店添加您的应用并获取应用密钥。
* [Mobile Service > IAP > 控制台使用指南 > Store interlocking information](/Mobil e%20Service/IAP/zh/console-guide/#store-interlocking-information)

#### 2. 注册Store的测试账户

* 为了进行付费测试，请按以下方式添加每个商店的测试人员。
    * Google
        * [Android > 测试购买设置](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > INAPP应用内结算测试](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * 必须使用应用内信息 - 通过测试按钮注册想要沙盒的终端电话号码进行测试。
        * 测试终端必须有USIM并需要注册电话号码(MDN)。
        * 需要安装**ONE store**。

#### 3. 使用NHN Cloud IAP服务

* 请参考IAP指南以设置IAP并注册item。
    * [Mobile Service > IAP > 控制台使用指南](/Mobile%20Service/IAP/zh/console-guide/)

#### 4. 下载

* 将下载的SDK的 **gamebase-adapter-purchase-iap** 文件夹添加到项目中。
    * 如果您不需要ONE store 付款，则可以删除 **iap-onestore-x.x.x.aar** 文件。
    * 相反，如果您需要ONE store付款，上述jar文件必须内置到项目中。

#### 5. AndroidManifest.xml(仅ONE store)

* 要使用ONE商店，您需要添加以下设置：

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!-- 对于版本16.XX.XX，输入4. https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development:开发模式 / release:运营 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. 初始化

* 初始化Gamebase时应指定Store Code。
* **STORE_CODE**从以下值中选择。
    * GG: Google
    * ONESTORE: ONE store

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Purchase Flow

请按以下顺序实现商品购买。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. 游戏客户端通过从Gamebase SDK调用 **requestPurchase**进行付款。
2. 如果付款成功，请调用 **requestItemListOfNotConsumed**查看未消费结算明细。
3. 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求对游戏付款商品的consume（消费）。
4. 游戏服务器通过Gamebase server的API请求 consume(消费)API。
   [API 指南](./api-guide/#wrapping-api)
5. 如果在IAP服务器上consume(消费)API调用成功，则游戏服务器向游戏客户端支付item。

* 商店支付成功，但存在发生错误而未能正常结束的情况。完成登录后请确认未消费支付明细。 <br/>
    * 如果登录成功，请调用 **requestItemListOfNotConsumed**以检查您的未消费结算明细。
    * 如果返还的未消费结算明细列表中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。

### Purchase Item

使用想要购买商品的itemSeq调用以下API并请求购买。<br/>
如果游戏用户取消购买，将返还**GamebaseError.PURCHASE_USER_CANCELED** 错误。请取消处理。

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(Activity activity, long itemSeq, GamebaseDataCallback<PurchasableReceipt> callback);
```

**示例**

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

* 查询尚未消费的一次性商品(CONSUMABLE)与消费性订阅商品(CONSUMABLE_AUTO_RENEWABLE) 信息。<br/>
如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。

* 请在以下两种情况下调用。
    1. 成功付款后，为了在处理item消费(consume)前进行最终确认而调用。
    2. 登录成功后，为了确认是否还存在未消费(consume)的item而调用。

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
若用户ID相同，同时查询在Android和iOS中购买的订阅商品。

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

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 模块未初始化。请确认是否将<br>gamebase-adapter-purchase-IAP 模块添加到项目中。 |
| PURCHASE_USER_CANCELED                   | 4002       | 游戏用户已取消购买商品。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 尚未完成购买逻辑的情况下已调用API。    |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 该商店的余额不足，无法结算。           |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 不支持的商店.<br>可选择的商店是 GG(Google), ONESTORE。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP库错误。<br>请确认DetailCode。   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 未知的购买错误。<br>请将完整的Log上传到 [客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 这是在IAP模块中发生的错误。
* 检查错误代码的方法如下。

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

* IAP错误代码，请参考以下文档。
    * [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud IAP > Android > 错误代码](/TOAST/zh/toast-sdk/iap-android/#_24)

