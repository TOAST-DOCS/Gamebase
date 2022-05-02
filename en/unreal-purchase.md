## Game > Gamebase > User Guide for Unreal SDK > Purchase 

This document describes setting requirements to use Unreal in-app purchase.  
The unified purchase API of Gamebase supports for easy integration of in-app purchases of many stores. 

### Settings

Regarding how to set in-app purchases on Android or iOS, read the following documents: <br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### Unreal Plugin Settings

> <font color="red">[Caution]</font><br/>
>
> If there is payment-related processing in an external plugin, the Gamebase purchase function may not work properly.

* You must disable the Online SubSystem plugin, which is enabled by default in Unreal, or change it to not use the Store function.
    * If you're using Online SubSystem GooglePlay plugin, edit the /Config/Android/AndroidEngine.ini file.
            
            [OnlineSubsystemGooglePlay.Store]
            bSupportsInAppPurchasing=False
            
    * If you're using Online SubSystem Online SubSystem iOS plugin, edit the /Config/IOS/IOSEngine.ini file.
            
            [OnlineSubsystemIOS.Store]
            bSupportsInAppPurchasing=False
            

#### Setting for Purchases on Android (for 4.24 or lower engine version)

* When the version 4.24 or lower is installed via Epic Games Launcher, 
    delete **Engine\Build\Android\Java\src\com\android\vending\billing\IInAppBillingService.aidl** to build it properly.  
    * [IInAppBillingService.aidl](https://developer.android.com/google/play/billing/api), provided by Gamebase, must be removed to prevent conflicts. 
    * There's no need to remove it, though, if you're using 4.25 or higher engine versions, or if the engine is provided by github. 

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
You may execute an item purchase in the following order: 

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. When a previous purchase has not been closed, purchase fails unless reprocessing runs. Therefore, call **RequestItemListOfNotConsumed** before payment so as to run reprocessing, and execute Consume Flow if there's any unsupplied item. 
2. For the game client, call **RequestPurchase** of Gamebase SDK to make a purchase. 
3. When a purchase has been successful, call **RequestItemListOfNotConsumed** and check history of non-consumable purchases; and if there's any item to supply, execute Consume Flow. 


### Consume Flow

If there's a value on the list of non-consumable purchases, execute Consume Flow in the following order:

> <font color="red">[Caution]</font><br/>
>
> To prevent the multiple issuance of the same purchased item, always check the game server for issuance history of items.
>

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.18.1.png)

1. The game client requests for Consume of a purchase item on the game server. 
    * Deliver UserID, itemSeq, paymentSeq, purchaseToken.
2. The game server tracks down the history of item supplies with same paymentSeq, or purchaseToken within game database.  
    * 2-1. If not supplied yet, supply the item for itemSeq to UserID.  
    * 2-2. After item is provided, save UserID, itemSeq, paymentSeq, and purchaseToken to game database so as to check redundancy.  
3. The game server calls Consume API to the Gamebase server to complete with item supply.
    * [API Guide > Purchase (IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

* Sometimes, a successful purchase at store ends up with failed closure, due to error. 
* Call **RequestItemListOfNotConsumed** to run reprocessing and execute Consume Flow for any non-supplied items. 
* It is recommended to call reprocessing in time for the following:
    * After login is completed
    * Before payment
    * Entering store (or lobby) of a game
    * Checking user profile or mailbox

### Purchase Item

By using itemSeq of an item to purchase, call the following API and request a purchase.  
If a game user cancels purchase, the **PURCHASE_USER_CANCELED** error is returned.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestPurchase(const FString& gamebaseProductId, const FGamebasePurchasableReceiptDelegate& onCallback);
void RequestPurchase(const FString& gamebaseProductId, const FString& payload, const FGamebasePurchasableReceiptDelegate& onCallback);
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

**VO**

```cpp
USTRUCT()
struct FGamebasePurchasableReceipt
{
    GENERATED_USTRUCT_BODY()
    
    // The product ID of a purchased item.
    UPROPERTY()
    FString gamebaseProductId;

    // An identifier for Legacy API that purchases products with itemSeq.
    UPROPERTY()
    int64 itemSeq;

    // The price of purchased product.
    UPROPERTY()
    float price;

    // Currency code.
    UPROPERTY()
    FString currency;

    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with purchaseToken.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    UPROPERTY()
    FString paymentSeq;

    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    UPROPERTY()
    FString purchaseToken;

    // The product ID that is registered to store console such as Google or Apple.
    UPROPERTY()
    FString marketItemId;

    // The product type which can have the following values:
    // * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE: A consumable product.
    // * AUTO_RENEWABLE: A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    UPROPERTY()
    FString productType;

    // This is a user ID with which a product is purchased.
    // If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
    UPROPERTY()
    FString userId;

    // The payment identifier of a store.
    UPROPERTY()
    FString paymentId;

    // paymentId is changed whenever product subscription is renewed.
    // This field shows the paymentId that was used when a subscription product was first purchased.
    // This value does not guarantee to be always valid, as it can have no value depending on the store
    // the user made a purchase and the status of the payment server.
    UPROPERTY()
    FString originalPaymentId;
    
    // The time when the product was purchased.(epoch time)
    UPROPERTY()
    int64 purchaseTime;
    
    // The time when the subscription expires.(epoch time)
    UPROPERTY()
    int64 expiryTime;

    // It is the value passed to payload when calling Gamebase.Purchase.requestPurchase API.
    //
    // This field can be used to hold a variety of additional information.
    // For example, this field can be used to separately handle purchase
    // and provision of the products purchased using the same user ID and sort them by game channel or character.
    UPROPERTY()
    FString payload;
};
```


### List Purchasable Items

To query the list of items, call the following API: 
The list of callback returns includes information of each item. 

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

**VO**

```cpp
USTRUCT()
struct FGamebasePurchasableItem
{
    GENERATED_USTRUCT_BODY()
    
    // The product ID that is registered with the Gamebase console.
    // Used when a product is purchased using Gamebase.Purchase.requestPurchase API.
    UPROPERTY()
    FString gamebaseProductId;

    // An identifier for Legacy API that purchases products with itemSeq.
    UPROPERTY()
    int64 itemSeq;

    // The price of a product.
    UPROPERTY()
    float price;

    // Currency code.
    UPROPERTY()
    FString currency;

    // The name of a product registered in the Gamebase console.
    UPROPERTY()
    FString itemName;

    // The product ID that is registered to store console such as Google or Apple.
    UPROPERTY()
    FString marketItemId;

    // The product type which can have the following values:
    // * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE: A consumable product.
    // * AUTO_RENEWABLE: A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    UPROPERTY()
    FString productType;
    
    // Localized price information with currency symbol.
    UPROPERTY()
    FString localizedPrice;
    
    // The name of a localized product registered with the store console.
    UPROPERTY()
    FString localizedTitle;

    // The description of a localized product registered with the store console.
    UPROPERTY()
    FString localizedDescription;

    // Shows whether the product is 'used or not' in the Gamebase console.
    UPROPERTY()
    bool isActive;
};
```


### List Non-Consumed Items

Send a request for non-consumable purchases of which items have not been normally consumed (delivered or paid) even after purchased. 
When there's a non-consumable purchase, send a request to the game server (item server) so as to deliver (pay) items. 

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

### List Activated Subscriptions

Query the list of activated subscriptions of a current user ID. 
Paid subscriptions (auto-renewal subscription, or auto-renewal consumable subscription products) can be queried until they are expired. 
Under same user ID, you can query all subscriptions purchased both on Android and iOS.  

> <font color="red">[Caution]</font><br/>
>
> For Android, subscriptions are currently supported only on the Google Play Store.

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

| Error                                       | Error Code | Description                              |
| ------------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                    | 4001       | The purchase module has not been initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to project. |
| PURCHASE_USER_CANCELED                      | 4002       | Purchase has been cancelled. |
| PURCHASE_NOT_FINISHED\_PREVIOUS\_PURCHASING | 4003       | API has been called when a purchase logic is not completed. |
| PURCHASE_NOT_ENOUGH_CASH                    | 4004       | Cannot purchase due to shortage of cash of the store. |
| PURCHASE_INACTIVE_PRODUCT_ID                | 4005       | Product is not activated.   |
| PURCHASE_NOT_EXIST_PRODUCT_ID               | 4006       | Requested for purchase with invalid GamebaseProductID. |
| PURCHASE_LIMIT_EXCEEDED                     | 4007       | You have exceeded your monthly purchase limit.              |
| PURCHASE_NOT_SUPPORTED_MARKET               | 4010       | The store is not supported.<br>The stores you can select are AS (App Store), GG (Google), ONESTORE, and GALAXY. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR             | 4201       | Error in IAP library.<br>Check DetailCode. |
| PURCHASE_UNKNOWN_ERROR                      | 4999       | Unknown error in purchase.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error occurs from IAP module. 
* Here's how to check error codes: 

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

* For IAP error codes, refer to the document below.
    * [NHN Cloud > User Guide for NHN Cloud SDK > NHN Cloud IAP > Unity > Error Codes](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)
