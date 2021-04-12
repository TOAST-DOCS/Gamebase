## Game > Gamebase > iOS SDK 使用指南 > 结算

以下是设置使用应用内结算的方法。

Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的应用内结算。

### Settings

#### Apple iTunes-Connect
1. 上传测试版APP build进行测试
2. In-App Purchases 商品登记及批准
3. 注册您的Sandbox Tester帐户
* Detail Guide for iTunes-Connect: [Apple Guide](https://help.apple.com/itunes-connect/developer/#/devb57be10e7)

#### Gamebase Console设置

您需要在Gamebase Console中设置的内容如下。

1. 在**Gamebase > Purchase(IAP) > 商店**中注册使用的商店。  
    * 商店 : 选择**App Store**。
2. 在**Gamebase > Purchase(IAP) > 商品**中注册商品。
    * 商品ID : 输入请求支付时使用的商品ID。
    * 商品名称 : 输入付款时显示的商品名称。
    * 使用与否 : 选择是否使用道具。
    * 商店 : 选择**App Store**。
    * 商店道具ID : 输入在iTunes-Connect中注册的Product ID。
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

购买商品的程序大体分为结算Flow、Consume Flow及”支付再处理”Flow。
请按以下顺序实现结算Flow。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 未能正常结束上一次支付时，若不进行‘’支付再处理”则将导致支付失败。因此支付前应调用**requestItemListOfNotConsumedWithCompletion:**启动‘’支付再处理”，若存在未提供的道具则进行Consume Flow。
2. 游戏客户端通过从Gamebase SDK调用**requestPurchaseWithGamebaseProductId:viewController:completion:**进行付款。 
3. 如果付款成功，请调用**requestItemListOfNotConsumedWithCompletion:**查看未消费结算明细。若存在未提供的道具，则进行Consume Flow。

### Consume Flow

如果未消费结算明细列表中存在值，请按以下顺序进行**Consume Flow**。

> <font color="red">[注意]</font><br/>
>
> 为了防止重复提供道具，必须要求游戏服务器确认是否重复提供道具。
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. 游戏客户向游戏服务器请求consume（消费）。
    * 传送UserID、gamebaseProductId、paymentSeq、purchaseToken。
2. 游戏服务器在游戏DB中查询是否存在以同样的paymentSeq提供道具的历史记录。
    * 2-1. 若存在未提供道具，则向UserID提供使用gamebaseProductId购买的商品。
    * 2-2. 提供后在游戏DB保存UserID、gamebaseProductId、paymentSeq、purchaseToken，必要时进行‘’支付再处理”或防止重复提供。
3. 游戏服务器通过调用Gamebase服务器的consume（消费）API提供道具。此时无需考虑是否已经提供道具。
    * [Game > Gamebase > API指南 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 商店支付已成功，但因出现错误无法正常终止时，
* 请调用**requestItemListOfNotConsumedWithCompletion:**进行‘’支付再处理”。若存在尚未提供的道具，则进行[Consume Flow](./aos-purchase/#consume-flow)。
* 请在下列情况下进行‘’支付再处理”。
    * 完成登录后
    * 支付之前
    * 进入游戏内商店（或 Lobby） 时
    * 查询用户简介或邮箱时

### Purchase Item

使用想要购买商品的gamebaseProductId，调用以下API申请购买。<br/>
gamebaseProductId基本上与在商店中注册的道具ID相同，并可以在Gamebase控制台中进行修改。完成支付后，在payload字段中输入的附加信息将会一直留在**TCGBPurchasableReceipt.payload**字段，因此可用于多种用途。<br/>
游戏用户取消购买时，返还**TCGB_ERROR_PURCHASE_USER_CANCELED**错误。请进行取消处理。

**API**

```objectivec
+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                              viewController:(UIViewController *)viewController
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

+ (void)requestPurchaseWithGamebaseProductId:(NSString *)gamebaseProductId 
                                     payload:(NSString *)payload 
                              viewController:(UIViewController *)viewController 
                                  completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;

// Legacy API
+ (void)requestPurchaseWithItemSeq:(long)itemSeq 
                    viewController:(UIViewController *)viewController 
                        completion:(void(^)(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error))completion;
```

**Example**

```objectivec
- (void)purchasingItem:(NSString *)gamebaseProductId {
    NSString *userPayload = @"USER_PAYLOAD";

    [TCGBPurchase requestPurchaseWithGamebaseProductId:gamebaseProductId payload:userPayload viewController:self completion:^(TCGBPurchasableReceipt *purchasableReceipt, TCGBError *error) {
        NSString *receivedPayload = purchasableReceipt.payload;
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



### List Purchasable Items

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


### List Non-Consumed Items

请求已购买了商品，却没有正常消费（发送，提供）item的未消费结算明细。<br/>
如果有未完成的商品，您必须要求游戏服务器（item服务器）处理配送item（支付）。

* 未完成支付时，可进行‘’支付再处理”。请在下列情况下调用。
    * 查询是否存在未提供的道具
        * 完成登录后
        * 进入游戏内商店（或 Lobby） 时
        * 查询用户简介或邮箱时
    * 查询需要进行‘’支付再处理”的道具
        * 支付之前
        * 支付失败后

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

### List Activated Subscriptions

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

利用用户的AppStore账号，以购买明细为准恢复购买明细后，将其注册在控制台中。
查询不到购买的订阅商品或未激活时使用。
完成的支付件、恢复的支付件都返还为结果。
如果存在未注册的自动更新型消费性订阅商品购买明细时，恢复之后可以在未消费购买明细中进行查询。

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

### Event by Promotion

> `注意`
> 仅适用于iOS 11或更高版本。
> 需要在Xcode 9.0以上版本build。
> Gamebase 1.13.0及更高版本支持。 (TOAST IAP SDK 1.6.0 以上适用)


> `注意`
> 只能在成功登录后调用。
> 成功登录后，必须在任何其他支付API之前执行。

#### 使用时的注意事项
正像Facebook SDK和Google AdMob SDK，在SDK内有In App Purchase (AppStore支付)功能时，如果要在进行Gamebase Login之前提前尝试支付， 则可能不显示付款弹窗。

* 解决方法
  * Facebook
    * Facebook Console > 设置 > 默认设置 > 禁用‘’将应用程序内事件自动Logging（推荐）"功能
    * 如果不使用Facebook认证功能 : 排除”GamebaseAuthFacebookAdapter.framework文件‘’后创建。

#### Overview
* Apple Developer Overview : [https://developer.apple.com/app-store/promoting-in-app-purchases/](https://developer.apple.com/app-store/promoting-in-app-purchases/)
* Apple Developer Reference : [https://help.apple.com/app-store-connect/#/deve3105860f](https://help.apple.com/app-store-connect/#/deve3105860f)


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

#### Process Promotion Event with GamebaseEventHandler

通过GamebaseEventHandler也可处理Promotion支付事件。
有关通过GamebaseEventHandler处理Promotion支付事件的方法，请参考如下指南。
[Game > Gamebase > iOS SDK使用指南 > ETC > Gamebase Event Handler](./ios-etc/#purchase-updated)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_NOT_SUPPORTED                 | 10         | 未包含GamebaseAdapter<br/>Error对象的域名为"TCGB.Gamebase.TCGBPurchase"时，请确认是否存在PurchaseAdapter。|
| TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED  | 4001       | 未初始化Gamebase PurchaseAdapter |
| TCGB\_ERROR\_PURCHASE\_USER\_CANCELED    | 4002       | 取消了购买。                             |
| TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 您未完成上一次的购买。                       |
| TCGB\_ERROR\_PURCHASE\_NOT\_ENOUGH\_CASH | 4004       | 因该商店的现金不足，无法进行结算。             |
| TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID | 4005       | 是未激活商品。             |
| TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID | 4006       | 您使用不存在的GamebaseProductID请求了支付。             |
| TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 是不支持的商店。iOS支持的商店是"AS"。 |
| TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 是IAP库错误。<br>请确认error.message。|
| TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR    | 4999       | 是未定义的购买错误。<br>将所有日志上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。|

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

* 关于IAP错误代码，请参考以下文件。
    * [TOAST > TOAST SDK使用指南 > TOAST IAP > iOS > 错误代码](/TOAST/ko/toast-sdk/iap-ios/#_15)

