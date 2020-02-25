## Game > Gamebase > Unity SDK使用指南 > IAP

以下是如何在Unity中使用应用内结算而进行必要设置的方法。
Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Settings

在Android或iOS中设置应用内结算的方法请参考以下文档。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

为在Unity Standalone中进行支付，必须添加IapAdapter与WebViewAdapter。
![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)


###  Purchase Flow

请按以下顺序实现商品购买。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. 游戏客户端通过从Gamebase SDK调用**RequestPurchase**进行付款。
2. 如果付款成功，请调用**RequestItemListOfNotConsumed**查看未消费结算明细。
3. 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求对游戏付款商品的consume（消费）。
4. 游戏服务器通过Gamebase server的API请求 consume(消费)API。 [API 指南](/Game/Gamebase/zh/api-guide/#wrapping-api)
5. 如果在IAP服务器上consume(消费)API调用成功，则游戏服务器向游戏客户端支付item。

* 商店支付成功，但存在发生错误而未能正常结束的情况。完成登录后请确认未消费支付明细。<br/>
	* 若登录成功，调用**RequestItemListOfNotConsumed**确认未消费支付明细。
	* 若返回的未消费支付明细列表中存在值，游戏客户向游戏服务器申请consume（消费），提供道具。

### Purchase Item

使用想要购买商品的itemSeq调用以下API并请求购。
如果游戏用户取消购买，将返还**PURCHASE_USER_CANCELED**错误。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**示例**
```cs
public void RequestPurchase(long itemSeq)
{
    Gamebase.Purchase.RequestPurchase(itemSeq, (purchasableReceipt, error) =>
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
```

### Get a List of Purchasable Items

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



### Get a List of Non-Consumed Items

请求已购买了商品，却没有正常消费（发送，提供）item的未消费结算明细。
如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**示例**
```cs
public void RequestItemListOfNotConsumed()
{
    Gamebase.Purchase.RequestItemListOfNotConsumed((purchasableReceiptList, error) =>
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

### Get the List of Actived Subscriptions

以当前用户ID为准查询激活的订阅列表。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。
若用户ID相同，同时查询在Android和iOS中购买的订阅商品。

> <font color="red">[注意]</font><br/>
>
> 当前订阅商品为Android的商品时，仅支持Google Play商店。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**示例**
```cs
public void RequestActivatedPurchasesSample()
{
    Gamebase.Purchase.RequestActivatedPurchases((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RequestItemListPurchasable succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableReceipt purchasableReceipt in purchasableReceiptList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("itemSeq:{0}", purchasableReceipt.itemSeq));
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

### App Store Promotion IAP

提供从AppStore应用程序内购买商品的功能。
购买商品成功后，通过以下登记的处理程序进行item支付。

促销 IAP需在AppStore Connect中另行设置才能显示。

> <font color="red">[注意]</font><br/>
>
> 仅适用于iOS 11或更高版本。
> 需要在Xcode 9.0以上版本build。
> Gamebase 1.13.0及更高版本支持。 (TOAST IAP SDK 1.6.0 以上适用)


> <font color="red">[注意]</font><br/>
>
> 只能在成功登录后调用。
> 成功登录后，必须在任何其他支付API之前执行。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetPromotionIAPHandler(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**示例**
```cs
public void SetPromotionIAPHandler()
{
    Gamebase.Purchase.SetPromotionIAPHandler((purchasableReceipt, error) => 
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
```
**Overview**

* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f

**How to Test AppStore Promotion IAP**

> `注意`
> 将应用程序上传到App Store Connect后，可以通过TestFlight安装应用程序后对其进行测试。
> 

1. 用TestFlight安装App。
2. 调用以下URL Scheme进行测试。

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 固定值 |
| host &amp; path | 无 | 无 |
| queries | action | purchaseIntent |
|		  | bundleId | APP的 bundeld identifier |
|		  | productIdentifier | 购买商品的 product identifier  |

示例) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | 模块未初始化。<br>请确认是否将gamebase-adapter-purchase-IAP模块添加到项目中。 |
| PURCHASE_USER_CANCELED                   | 4002       | 游戏用户已取消购买商品。                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 之前的购买逻辑未完成的情况下调用了API。 |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 该商店的余额不足，无法结算。              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 不支持的商店。<br>可选择的商店为GG(Google), TS(ONE store), TEST。 |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP库错误。<br>请确认DetailCode。   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 未知购买错误。<br>请将完整的Log上传到 [客服中心](https://toast.com/support/inquiry)，我们会尽快回复。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 这是在IAP模块中的错误。
* 检查错误代码的方法如下。

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

* IAP错误代码，请参考以下文档。
    * [Mobile Service > IAP > 错误代码 > Client API错误类型](/Mobile%20Service/IAP/zh/error-code/#client-api)





