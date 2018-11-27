## Game > Gamebase > Unity SDK使用指南 > IAP

以下是如何在Unity中使用应用内结算而进行必要设置的方法。
Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### 设置

在Android或iOS中设置应用内结算的方法请参考以下文档。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

###  购买流程

请按以下顺序实现商品购买。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)


1. 游戏客户端通过从Gamebase SDK调用**RequestPurchase**进行付款。
2. 如果付款成功，请调用**RequestItemListOfNotConsumed**查看未消费结算明细。
3. 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求对游戏付款商品的consume（消费）。
4. 游戏服务器通过Gamebase server的API请求 consume(消费)API。 [API 가이드](/Game/Gamebase/ko/api-guide/#wrapping-api)
5. 如果在IAP服务器上consume(消费)API调用成功，则游戏服务器向游戏客户端支付item。

商店付款成功，但出现错误无法正常结束的情况下，请登录后调用以下两个API执行重试逻辑。 <br/>

1. 未处理的商品配送请求
    * 如果登录成功，请调用**RequestItemListOfNotConsumed**以检查您的未消费结算明细。
    * 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求consume(消费)后支付item。
2. 尝试重新处理付款错误
    * 如果登录成功，请调用**RequestRetryTransaction**以自动尝试重新处理未处理的明细。
    * 如果被返还的successList中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。
    * 如果被返还的failList中存在值，请通过游戏服务器或 Log & Crash 传输来获取数据, 可以通过[客服中心](https://toast.com/support/inquiry) 咨询重新处理失败原因。

### 购买商品

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

### 获取购买商品列表

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



### 获取未支付商品列表

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

### 重新处理失败的购买交易

如果在商店付款成功，但因TOAST IAP服务器认证失败等原因未能正常付款的情况下，我们将尝试使用API重新处理。
最后，根据付款成功的历史记录，需要通过调用item配送(支付) 等的API 来进行处理。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestRetryTransaction(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableRetryTransactionResult> callback)
```

**示例**
```cs
public void RequestRetryTransaction()
{
    Gamebase.Purchase.RequestRetryTransaction((purchasableRetryTransactionResult, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RequestRetryTransaction succeeded.");

            // Should Deal With This Retry Transaction Result.
            // You may send result to your gameserver and add item to user.
        }
        else
        {
            Debug.Log(string.Format("RequestRetryTransaction failed. error is {0}", error));
        }
    });
}
```

### AppStore Promotion IAP

提供从AppStore应用程序内购买商品的功能。
购买商品成功后，通过以下登记的处理程序进行item支付。

促销 IAP需在AppStore Connect中另行设置才能显示。

> <font color="red">[注意]</font><br/>
> 仅适用于iOS 11或更高版本。
> 需要在Xcode 9.0以上版本build。
> Gamebase 1.13.0及更高版本支持。 (TOAST IAP SDK 1.6.0 以上适用)


> <font color="red">[注意]</font><br/>
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


### Error处理

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
    * [Mobile Service > IAP > 错误代码 > Client API错误类型](/Mobile%20Service/IAP/ko/error-code/#client-api)





