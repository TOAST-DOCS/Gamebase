## Game > Gamebase > Unity SDK使用指南 > IAP

以下是如何在Unity中使用应用内结算而进行必要设置的方法。
Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Settings

在Android或iOS中设置应用内结算的方法请参考以下文档。

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

### Purchase Flow

购买道具的程序大体分为Flow、Consume Flow及“支付再处理”Flow。
请按以下顺序实现结算Flow。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 未正常结束上一次支付时，若不进行“支付再处理”则将导致支付失败。因此支付前应调用**RequestItemListOfNotConsumed**进行“支付再处理”， 若存在未提供的道具则进行Consume Flow。
2. 游戏客户端通过从Gamebase SDK调用**RequestPurchase**尝试支付。 
3. 如果付款成功，请调用**RequestItemListOfNotConsumed**查看未消费结算明细。若存在未提供的道具，则进行Consume Flow。

### Consume Flow

如果返还的未消费结算明细列表中存在值，请按以下顺序进行Consume Flow。

> <font color="red">[注意]</font><br/>
>
> 为了防止重复提供道具，必须通过游戏服务器确认是否重复提供道具。 
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.40.1.png)

1. 游戏客户向游戏服务器请求consume（消费）。
    * 传送UserID、gamebaseProductId、paymentSeq、purchaseToken。
2. 游戏服务器查看在游戏DB中是否存在以同样的paymentSeq提供道具的历史记录。
    * 2-1.
        * [Game > Gamebase > API指南 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-2. 若存在未提供的道具，通过调用Gamebase服务器的Payment Transaction API验证paymentSeq、purchaseToken值​​是否有效。
    * [Game > Gamebase > API指南 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
    * 2-3. 提供道具后在游戏DB保存UserID、gamebaseProductId、paymentSeq、purchaseToken，必要时进行“支付再处理”或防止重复提供。
3. 游戏服务器通过调用Gamebase服务器的consume（消费）API提供道具。这时不考虑是否已提供道具。
    * [Game > Gamebase > API指南 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 商店支付已成功，但因出现错误无法正常终止时，
* 请调用**RequestItemListOfNotConsumed**进行“支付再处理”。若存在尚未提供的道具，则进行Consume Flow。
* 请在下列情况下进行“支付再处理”。
    * 完成登录后
    * 支付之前
    * 进入游戏内商店（或 Lobby）时
    * 查询用户简介或邮箱时

### Purchase Item

使用您想要购买的物品的gamebaseProductId请求购买。<br/>
gamebaseProductId通常与商店中注册的商品的ID相同，但也可以在Gamebase控制台中进行更改。
在payload字段中输入的附加信息成功付款后将保留在**PurchasableReceipt.payload**中，允许您将其用于多种目的。<br/>

> <font color="red">[注意]</font><br/>
>
> AMAZON商店不支持**payload**字段。
>

游戏用户取消购买时返还**PURCHASE_USER_CANCELED**错误。
请进行取消处理。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(string gamebaseProductId, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
static void RequestPurchase(string gamebaseProductId, string payload, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)

```

**示例**
```cs
public void RequestPurchase(string gamebaseProductId)
{
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}


public void RequestPurchase(string gamebaseProductId)
{
    string userPayload = "{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}";
    Gamebase.Purchase.RequestPurchase(gamebaseProductId, userPayload, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
            // userPayload value entered when calling API
            string payload = purchasableReceipt.payload
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}  
```

**VO**
```cs
public class PurchasableReceipt
{
    /// <summary>
    /// 是购买的道具的商品ID。
    /// </summary>
    public string gamebaseProductId;

    /// <summary>
    /// 是通过itemSeq购买商品的Legacy API专用标识符。
    /// </summary>
    public long itemSeq;

    /// <summary>
    /// 是购买的商品的价格。 
    /// </summary>
    public float price;

    /// <summary>
    /// 是货币代码。
    /// </summary>
    public string currency;

    /// <summary>
    /// 是结算标识符。
    /// 是与purchaseToken调用“Consume”服务器API的重要信息。
    ///    
    /// 注意 : 请在游戏服务器调用Consume API!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string paymentSeq;

    /// <summary>
    /// 是结算标识符。  
    /// 是与paymentSeq调用“Consume”服务器API的重要信息。
    /// 在Consume API应将名称作为“accessToken”传送。
    ///    
    /// 注意 : 请在游戏服务器调用Consume API!
    /// <para/><see href="https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap">Consume API</see>
    /// </summary>
    public string purchaseToken;

    /// <summary>
    /// 是在商店控制台中注册的产品ID，如Google和Apple 。
    /// </summary>
    public string marketItemId;

    /// <summary>
    /// 作为产品类型，存在以下值。
    /// * UNKNOWN : 无法识别类型/请更新Gamebase SDK或联系Gamebase客户服务。
    /// * CONSUMABLE : 消费型商品
    /// * AUTO_RENEWABLE : 订购型商品
    /// * CONSUMABLE_AUTO_RENEWABLE : 当您想向购买订阅型产品的用户定期提供可消费商品时使用“可消费订阅商品”。
    /// <para/><see cref="GamebasePurchase.ProductType"/>
    /// </summary>
    public string productType;

    /// <summary>
    /// 购买商品的User ID  
    /// 如果您使用没有购买商品的User ID登录，则无法获取购买的道具。
    /// </summary>
    public string userId;

    /// <summary>
    /// 为商店的结算标识符。
    /// </summary>
    public string paymentId;

    /// <summary>
    /// 当订购商品被更新时，paymentId也将被更改。
    /// 通过此字段可以确认第一次进行订阅商品结算时的paymentId。
    /// 根据商店类型、结算服务器状态，可能不存在值，
    /// 因此不能保证始终是有效值。
    /// </summary>
    public string originalPaymentId;

    /// <summary>
    /// 为购买商品的时间。(epoch time)
    /// </summary>
    public long purchaseTime;

    /// <summary>
    /// 为订阅结束的时间。(epoch time)
    /// </summary>
    public long expiryTime;


    /// <summary>
    /// 是已支付的商店代码。
    /// 您可以在GamebaseStoreCode类中查看商店代码列表。
    /// </summary>
    public string storeCode;

    /// <summary>
    /// 是调用Gamebase.Purchase.requestPurchase时作为payload传送的值。
    ///  
    /// 当使用相同的User ID进行了购买，但仍然需要根据游戏频道或角色
    /// 区分商品的购买和提供等，
    /// 即，当您需要添加游戏中所需的各种附加信息时，可使用此字段。
    /// </summary>
    public string payload;

    /// <summary>
    /// 是否进行Promotion付款。
    /// 在- (Android) Gamebase结算服务器占时关闭验证逻辑时，它只输出false，因此并不始终保证有效值。
    /// </summary>
    public bool isPromotion;
    
    /// <summary>
    /// 是否进行测试付款。
    /// 在- (Android) Gamebase结算服务器占时关闭验证逻辑时，它只输出false，因此并不始终保证有效值。
    /// </summary>
    public bool isTestPurchase;
}
```

### List Purchasable Items

要查询商品列表，请调用以下API。 
回调返还的数组(array)包含各item的信息。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**示例**
```cs
public void RequestItemListPurchasable()
{
    Gamebase.Purchase.RequestItemListPurchasable((purchasableItemList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

### List Non-Consumed Items

请求已购买了道具, 但未能消费(配送、支付)道具未消费结算明细。
如果有未完成的商品，必须要求游戏服务器（item 服务器）处理配送item（支付）。
未能完成支付时，也起到“支付再处理”的作用。可在下列情况下调用该函数。
* 查看是否存在未向用户提供的道具
    * 完成登录后
    * 进入游戏内商店（或 Lobby）时
    * 查看用户简介或邮箱时
* 查看需要进行“支付再处理”的道具
    * 支付之前   
    * 支付失败后

**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ | 
| allStores                       | O                          | 还返还使用相同UserID的在其他商店购买的未消费明细。<br/>默认值为**false**。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**示例**
```cs
public void RequestItemListOfNotConsumedSample(bool allStores)
{
    var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
    {
        allStores = allStores
    };
    Gamebase.Purchase.RequestItemListOfNotConsumed(configuration, (purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");

            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

### List Actived Subscriptions

以当前用户ID为准查询激活的订阅列表。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。

> <font color="red">[注意]</font><br/>
>
> 目前Android只在Google Play商店支持订购商品。


**GamebaseRequest.Purchase.PurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | 还返还使用相同UserID的在其他商店购买的未消费明细<br/>默认值为**false**。 |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration, GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**示例**
```cs
public void RequestActivatedPurchasesSample(bool allStores)
{
        var configuration = new GamebaseRequest.Purchase.PurchasableConfiguration
        {
               allStores = allStores
        };
    
       Gamebase.Purchase.RequestActivatedPurchases(configuration, (purchasableReceiptList, error) =>
       {
                  if (Gamebase.IsSuccess(error) == true)
               {
            Debug.Log("RequestItemListPurchasable succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableReceipt purchasableReceipt in purchasableReceiptList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("gamebaseProductId:{0}", purchasableReceipt.gamebaseProductId));
                message.AppendLine(string.Format("price:{0}", purchasableReceipt.price));
                message.AppendLine(string.Format("currency:{0}", purchasableReceipt.currency));
                
                // You will need paymentSeq and purchaseToken when calling the Consume API.
                // Refer to the following document for the Consume API.
                // https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchaseiap
                message.AppendLine(string.Format("paymentSeq:{0}", purchasableReceipt.paymentSeq));
                message.AppendLine(string.Format("purchaseToken:{0}", purchasableReceipt.purchaseToken));
                message.AppendLine(string.Format("marketItemId:{0}", purchasableReceipt.marketItemId));
                Debug.Log(message);
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RequestItemListPurchasable failed. error is {0}", error));
        }
    });
}
```

### Event by Promotion

完成Promotion支付后，可通过GamebaseEventHandler接收Event并进行处理。 
关于使用GamebaseEventHandler处理Promotion支付Event的方法，请参考以下指南。
[Game > Gamebase > Unity SDK使用指南 > ETC > Gamebase Event Handler](./unity-etc/#purchase-updated)

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

> <font color="red">[注意]</font><br/>
>
> 关于iOS Promotion付款方式，请参考如下指南。 
> [Game > Gamebase > iOS SDK使用指南 > IAP > Event by Promotion](./ios-purchase/#event-by-promotion)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase模块未初始化。请确认是否将<br>gamebase-adapter-purchase-IAP模块添加到项目中。 |
| PURCHASE_USER_CANCELED                   | 4002       | 游戏用户已取消购买商品。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 尚未完成购买逻辑的情况下调用了API。    |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 该商店的余额不足，无法结算。           |
| PURCHASE_INACTIVE_PRODUCT_ID             | 4005       | 此商品为非激活状态。 |
| PURCHASE_NOT_EXIST_PRODUCT_ID            | 4006       | 请求支付的GamebaseProductID不存在。 |
| PURCHASE_LIMIT_EXCEEDED                  | 4007       | 超过了一个月购买限额。             |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 不支持的商店<br>可选择的商店是GG(Google)、ONESTORE、GALAXY、AMAZON及HUAWEI。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | 是NHN Cloud IAP库错误。<br/>请确认详细错误。 |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 未知的购买错误<br>请将完整的Log上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 当在NHN Cloud IAP库中发生错误时，将返还此错误。 
* 在NHN Cloud IAP库发生的错误信息包含在详细错误中，而详细的错误代码和消息如下。

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* 关于NHN Cloud IAP错误代码，请参考以下文件。
    * [NHN Cloud > NHN Cloud SDK使用指南 > NHN Cloud IAP > Unity > 错误代码](https://docs.toast.com/zh/TOAST/zh/toast-sdk/iap-unity/#error-code)




