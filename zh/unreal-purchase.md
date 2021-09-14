## Game > Gamebase > Unreal SDK使用指南 > 结算

下面将了解使用In-App结算的时的设置方法。
Gamebase提供集成支付API，帮助您在游戏中轻松联动多家商店的In-App结算。

### Settings

如果要在Android或iOS上设置In-App结算功能，请参考以下文档。<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### 有关Android结算的设置(引擎版本为4.24以下)

* 通过Epic Games Launcher设置4.24版本时,
    删除**Engine\Build\Android\Java\src\com\android\vending\billing\IInAppBillingService.aidl**才能打包。
    * 因Gamebase提供[IInAppBillingService.aidl](https://developer.android.com/google/play/billing/api)文件，将发生冲突，因此需要删除。
    * 如果使用4.25以上版本或由github提供引擎，则不需删除。

### Purchase Flow

道具的购买大体分为Flow、Consume Flow及‘’支付再处理”Flow。
请按以下顺序实现结算Flow。

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 尚未正常结束上一次支付时，若不进行‘’支付再处理”则将导致支付失败。因此支付前应调用**RequestI temListOfNotConsumed**进行‘’支付再处理”， 若存在未提供的道具则进行Consume Flow。
2. 游戏客户端通过从Gamebase SDK调用**RequestPurchase**尝试支付。 
3. 如果付款成功，调用**RequestItemListOfNotConsumed**查看未消费结算明细。若存在未提供的道具，则进行Consume Flow。

### Consume Flow

如果返还的未消费结算明细列表中存在值，请按以下顺序进行**Consume Flow**。

> <font color="red">[注意]</font><br/>
>
> 为了防止重复提供道具，必须通过游戏服务器确认是否重复提供道具。
>

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. 游戏客户端向游戏服务器请求consume（消费）。
    * 传送UserID、gamebaseProductId、paymentSeq、purchaseToken。
2. 游戏服务器查看在游戏DB中是否存在以同样的paymentSeq提供道具的历史记录。
    * 2-1. 若存在未提供道具，则需向UserID提供使用gamebaseProductId购买的道具。
    * 2-2. 提供道具后在游戏DB保存UserID、gamebaseProductId、paymentSeq、purchaseToken，必要时进行‘’支付再处理”或防止重复提供。
3. 游戏服务器通过调用Gamebase服务器的consume（消费）API提供道具。这时不考虑是否已经提供道具。
    * [API指南 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

* 商店支付已成功，但因出现错误无法正常终止时，
* 请调用**RequestItemListOfNotConsumed**进行‘’支付再处理”。若存在尚未提供的道具，则进行Consume Flow。
* 请在下列情况下进行‘’支付再处理”。
    * 完成登录后
    * 支付之前
    * 进入游戏内商店（或 Lobby）时
    * 查看用户简介或邮箱时

### Purchase Item

使用想要购买商品的gamebaseProductId调用以下API请求购买。
游戏用户取消购买时返还**PURCHASE_USER_CANCELED**错误。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestPurchase(const FString& gamebaseProductId, const FGamebasePurchasableReceiptDelegate& onCallback);
void RequestPurchase(const FString& gamebaseProductId, const FString& payload, const FGamebasePurchasableReceiptDelegate& onCallback);

// Legacy API
void RequestPurchase(int64 itemSeq, const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestPurchase(const FString& gamebaseProductId)
{
    IGamebase::Get().GetPurchase().RequestPurchase(gamebaseProductId, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *purchasableReceipt->gamebaseProductId, purchasableReceipt->price, *purchasableReceipt->currency,
                *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the error code and handle the error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }

        }
    }));
}

void Sample::RequestPurchaseWithPayload(const FString& gamebaseProductId)
{
    FString userPayload = TEXT("{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}");
    
    IGamebase::Get().GetPurchase().RequestPurchase(gamebaseProductId, userPayload, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *purchasableReceipt->gamebaseProductId, purchasableReceipt->price, *purchasableReceipt->currency,
                *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);

            FString payload = purchasableReceipt->payload;
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the error code and handle the error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }
        }
    }));
}
```

### List Purchasable Items

若需查看道具列表，请调用以下API。
通过回调返还的列表显示每个道具的信息。 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestItemListPurchasable(const FGamebasePurchasableItemListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestItemListPurchasable()
{
    IGamebase::Get().GetPurchase().RequestItemListPurchasable(FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* purchasableItemList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *purchasableItem.gamebaseProductId, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable failed. (error: %d)"), error->code);
        }
    }));
}
```



### List Non-Consumed Items

对已购买，但未消费(配送、支付)道具的未消费结算明细进行请求。
如果有未完成的商品，必须要求游戏服务器（item 服务器）处理配送item（支付）。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestItemListOfNotConsumed()
{
    IGamebase::Get().GetPurchase().RequestItemListOfNotConsumed(FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* purchasableItemList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed failed. (error: %d)"), error->code);
        }
    }));
}
```

### List Actived Subscriptions

以当前用户ID为准查看激活的订阅列表。
到期前可一直查看完成支付的订阅商品（自动更新型订阅、自动更新型消费性订阅商品）。 
若用户ID相同，可同时查看在Android和iOS中购买的订阅商品。

> <font color="red">[注意]</font><br/>
>
> Android对当前的订阅商品仅支持Google Play商店。 
 
**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestActivatedPurchases()
{
    IGamebase::Get().GetPurchase().RequestActivatedPurchases(FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableReceipt>* purchasableReceiptList, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases failed. (error: %d)"), error->code);
        }
    }));
}
```


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | 未初始化Purchase模块。<br>请确认是否将gamebase-adapter-purchase-IAP模块添加在项目中。|
| PURCHASE_USER_CANCELED                   | 4002       | 游戏用户取消购买道具。                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003      | 未完成购买逻辑的状态下调用了API。|
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 因该商店的现金不足，无法进行结算。              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 是不支持的商店。<br>可以选择的商店为GG(Google)和ONESTORE。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | 是IAP库错误。<br>请确认DetailCode。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 是未定义的购买错误。<br>请将所有日志上传到[客户服务](https://toast.com/support/inquiry)，我们会尽快回复。|

* 关于所有错误代码，请参考以下文档。 
    * [错误代码](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 是在IAP模块出现的错误。 
* 确认错误代码的方法如下。 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* 关于IAP错误代码，请参考以下文档。 
    * [TOAST > TOAST SDK使用指南 > TOAST IAP > Unity > 错误代码](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)





