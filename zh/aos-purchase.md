## Game > Gamebase > Android SDK使用指南> 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### 设置

#### 1. Store Console

* 请参考以下IAP指南，在每个商店添加您的应用并获取应用密钥。
* [Mobile Service > IAP > 控制台使用指南 > Store interlocking information](/Mobil e%20Service/IAP/ko/console-guide/#store-interlocking-information)

#### 2. 注册Store的测试账户

* 为了进行付费测试，请按以下方式添加每个商店的测试人员。
    * Google
        * [Android > 测试购买设置](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > INAPP应用内结算测试](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
        * 必须使用应用内信息 - 通过测试按钮注册想要沙盒的终端电话号码进行测试。
        * 测试终端必须有USIM并需要注册电话号码(MDN)。
        * 需要安装**ONE store**。

#### 3. 使用TOAST IAP服务

* 请参考IAP指南以设置IAP并注册item。
    * [Mobile Service > IAP > 控制台使用指南](/Mobile%20Service/IAP/ko/console-guide/)

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

* 在Gamebase 初始化期间调用configuration的**setStoreCode()**。
* **STORE_CODE**从以下值中选择。
    * GG: Google
    * ONESTORE: ONE store
    * TEST: 用于IAP测试


```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(APP_ID, APP_VERSION)
        .setStoreCode(STORE_CODE)	// 必须声明Store code.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### 购买流程

请按以下顺序实现商品购买。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. 游戏客户端通过从Gamebase SDK调用 **requestPurchase**进行付款。
2. 如果付款成功，请调用 **requestItemListOfNotConsumed**查看未消费结算明细。
3. 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求对游戏付款商品的consume（消费）。
4. 游戏服务器通过Gamebase server的API请求 consume(消费)API。
   [API 指南](/Game/Gamebase/ko/api-guide/#wrapping-api)
5. 如果在IAP服务器上consume(消费)API调用成功，则游戏服务器向游戏客户端支付item。

商店付款成功，但发生错误无法正常结束的情况下，请登录后调用以下两个API执行重试逻辑。 <br/>

1. 未处理的商品配送请求
    * 如果登录成功，请调用 **requestItemListOfNotConsumed**以检查您的未消费结算明细。
    * 如果返还的未消费结算明细列表中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。

2. 尝试重新处理付款错误
	* 如果登录成功，请调用 **requestRetryTransaction**以自动尝试重新处理未处理的明细。
    * 如果被返还的successList中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。
    * 如果被返还的failList中存在值，请通过游戏服务器或 Log & Crash 传输来获取数据, 可以通过**[客服中心](https://toast.com/support/inquiry)**咨询重新处理失败原因。

### 购买商品

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

### 获取购买商品列表

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

### 获取未支付商品列表

请求已购买了商品，却没有正常消费（发送，提供）item的未消费结算明细。<br/>
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

### 重新处理失败的购买交易

如果在商店付款成功，但因TOAST IAP服务器认证失败等原因未能正常付款的情况下，我们将尝试使用API重新处理。 <br/>
最后，根据付款成功的历史记录，需要通过调用item配送(支付) 等的API 来进行处理。

**API**

```java
+ (void)Gamebase.Purchase.requestRetryTransaction(Activity activity, GamebaseDataCallback<PurchasableRetryTransactionResult> callback);
```

**示例**

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request retry transaction failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Error处理

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 模块未初始化。请确认是否将<br>gamebase-adapter-purchase-IAP 模块添加到项目中。 |
| PURCHASE_USER_CANCELED                   | 4002       | 游戏用户已取消购买商品。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 尚未完成购买逻辑的情况下已调用API。    |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 该商店的余额不足，无法结算。           |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 不支持的商店.<br>可选择的商店是 GG(Google), TS(ONE store), TEST . |
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
    * [Mobile Service > IAP > 错误代码 > Client API 错误类型](/Mobile%20Service/IAP/ko/error-code/#client-api)

