## Game > Gamebase > iOS SDK 使用指南 > 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Settings

#### Apple iTunes-Connect
1. 上传测试版APP build进行测试
2. In-App Purchases 商品登记及批准
3. 注册您的Sandbox Tester帐户
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### TOAST Console 登记
应在TOAST Console中设置以下内容。

1. **Gamebase > Purchase(IAP) > APP**中添加使用的商店。
    * 商店:选择**App Store**。
2. **Gamebase > Purchase(IAP) > item**中添加商品。
    * 商店: 选择**App Store**。
    * 商店项目ID: 输入在iTunes-Connect中登记的Product ID。
3. 设置完商品后，请点击**保存**。

#### Xcode Project 设置
1. **Targets > Capabilities > In-App Purchase**设置为**ON**。
2. 根据需要设置**Targets > General > Identity** 的Bundle Identifier, Version和Build的值。

#### 导入Header文件

将以下头文件导入要实现Purchase API的ViewController。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Purchase Flow

请按以下顺序实现购买商品。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. 游戏客户端通过从Gamebase SDK 调用**requestPurchaseWithItemSeq:viewController:completion:**进行付款。
2. 如果付款成功，请调用 **requestItemListOfNotConsumedWithCompletion:**查看未消费结算明细。
3. 如果返还的未消费结算明细列表中存在值，游戏客户端向游戏服务器请求对游戏付款商品的consume（消费）。
4. 游戏服务器通过Gamebase server的API请求 consume(消费)API 。
    [API 指南](./api-guide/#wrapping-api)
5. 如果在IAP服务器上consume(消费)API调用成功，则游戏服务器向游戏客户端支付item。

商店付款成功，但发生错误无法正常结束的情况下，请登录后调用以下两个API执行重试逻辑。 <br/>

1. 未处理的商品配送请求
* 如果登录成功，请调用 **requestItemListOfNotConsumedWithCompletion:** 以检查您的未消费结算明细。
    *如果返还的未消费结算明细列表中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。
2. 尝试重新处理付款错误
    * 如果登录成功，请调用 **requestRetryTransactionWithCompletion:** 以自动尝试重新处理未处理的明细。
    * 如果被返回的 successList 中存在值，则游戏客户端向游戏服务器请求consume(消费)并支付item。
    *  如果被返回的failList中存在值，请通过游戏服务器或 Log & Crash 传输来获取数据, 可以通过, [客服中心](https://toast.com/support/inquiry)咨询重新处理失败原因。

* 商店支付成功，但存在发生错误而未能正常结束的情况。完成登录后请确认未消费支付明细。<br/>
	* 若登录成功，调用**requestItemListOfNotConsumedWithCompletion:**确认未消费支付明细。
	* 若返回的未消费支付明细列表中存在值，游戏客户向游戏服务器申请consume（消费），提供道具。

### Purchase Item

使用想要购买商品的itemSeq调用以下API并请求购买。

```objectivec
- (void)purchasingItem:(long)itemSeq {
    [TCGBPurchase requestPurchaseWithItemSeq:itemSeq viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Purchase Item Succeeded
        } else if (error.code == TCGB_ERROR_PURCHASE_USER_CANCELED) {
            // User Canceled Purchasing Item
        } else if (error) {
            // To Purchase Item Failed cause of the error
        }
    }];
}
```



### Get a List of Purchasable Items

要查询商品列表，请调用以下API。回调返还的数组(array)包含各item的信息。

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListPurchasableWithCompletion:^(NSArray *purchasableItemArray, TCGBError *error) {
        if (error != nil) {
            // To Request Item List Failed cause of the error
            return;
        }

        NSMutableArray *itemArrayMutable = [[NSMutableArray alloc] init];
        [purchasableItemArray enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            TCGBPurchasableItem *item = (TCGBPurchasableItem *)obj;

            [itemArrayMutable addObject:item];
        }];
    }];
}
```


### Get a List of Non-Consumed Items

请求已购买了商品，却没有正常消费（发送，提供）item的未消费结算明细。<br/>
如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。

* 请在以下两种情况下调用。
    1. 成功付款后，为了在处理item消费(consume)前进行最终确认而调用。
    2. 登录成功后，为了确认是否还存在未消费(consume)的item而调用。


```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestItemListOfNotConsumedWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Non-consumed Item List Failed cause of the error
            return;
        }

        // Should Deal With This Non-consumed Items.
        // Send this item list to the game(item) server for consuming item
    }];
}
```

### Get a List of Activated Subscriptions

以当前用户ID为准查询激活的订阅列表。
完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）到期前可一直查询。
若用户ID相同，同时查询在Android和iOS中购买的订阅商品。

### Reprocess Failed Purchase Transaction

如果在商店付款成功，但因TOAST IAP服务器认证失败等原因未能正常付款的情况下，我们将尝试使用API重新处理。<br/>
最后，根据付款成功的历史记录，需要通过调用item配送(支付) 等的API 来进行处理。

```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestActivatedPurchasesWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Activated Item List Failed cause of the error
            return;
        }

        // Should Deal With This Activated Items.
    }];
}
```

### Restore Purchase

以利用用户的AppStore账号购买的明细为准，恢复购买明细，反映在控制台中。
购买的订阅商品查询不到或者未激活时使用。
包括完成的支付项，恢复的支付项作为结果返回。
对于自动更新型消费性订阅商品，存在未反映的购买明细时，恢复后可在未消费购买明细中查询。


```objectivec
- (void)viewDidLoad {
    [TCGBPurchase requestRestoreWithCompletion:^(NSArray<TCGBPurchasableReceipt *> *purchasableReceiptArray, TCGBError *error) {
        if (error != nil) {
            // To Requesting Restore Failed cause of the error
            return;
        }

        // Should Deal With This Restored Items.
    }];
}
```

### App Store Promotion IAP

> `注意`
> 仅适用于iOS 11或更高版本。
> 需要在Xcode 9.0以上版本build。
> Gamebase 1.13.0及更高版本支持。 (TOAST IAP SDK 1.6.0 以上适用)


> `注意`
> 只能在成功登录后调用。
> 成功登录后，必须在任何其他支付API之前执行。


#### 概述
* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f


提供从AppStore应用程序内购买商品的功能。
购买商品成功后，通过以下登记的处理程序进行item支付。

促销 IAP需在AppStore Connect中另行设置才能显示。


```objectivec
- (void)didSuccessLogin {
	[TCGBPurchase setPromotionIAPHandler:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
    	if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // To Purchase Item Succeeded
        } else if (error.code == TCGB_ERROR_PURCHASE_USER_CANCELED) {
            // User Canceled Purchasing Item
        } else if (error) {
            // To Purchase Item Failed cause of the error
        }
    }];
}
```


#### 如何测试AppStore Promotion IAP

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
| | bundleId | APP的 bundeld identifier |
| | productIdentifier | 购买商品的 product identifier |

示例) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | 不包含GamebaseAdapter。<br/>如果Error对象的域是 "TCGB.Gamebase.TCGBPurchase"，请确认PurchaseAdapter是否存在。 |
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | Gamebase PurchaseAdapter未初始化。   |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | 游戏用户已取消购买商品。                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 之前的购买未完成。                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 该商店的余额不足，无法结算。            |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 不支持的商店。 iOS支持的商店是 "AS" 。 |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | IAP库错误。<br>请确认error.message。 |
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | 未知的购买错误。<br>请将完整的Log上传到 [客服中心](https://toast.com/support/inquiry)，我们会尽快回复。|

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)



**TCGB_ERROR_PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 这是在IAP模块中的错误.
* 检查错误代码的方法如下.

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // NSError object from external module
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* IAP错误代码，请参考以下文档。
    * [Mobile Service > IAP > 错误代码 > Client API 错误类型](/Mobile%20Service/IAP/ko/error-code/#client-api)


