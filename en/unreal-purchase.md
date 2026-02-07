## Game > Gamebase > User Guide for Unreal SDK > Purchase 

This document describes setting requirements to use Unreal in-app purchase.  
The unified purchase API of Gamebase supports for easy integration of in-app purchases of many stores. 

### Settings

Regarding how to set in-app purchases on Android or iOS, read the following documents: <br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)
* [Windows Purchase Settings](unreal-started/#windows-settings)

#### Unreal Plugin Settings

> <font color="red">[Caution]</font><br/>
>
> If there is payment-related processing in an external plugin, the Gamebase purchase function may not work properly.

* You must disable the Online SubSystem plugin, which is enabled by default in Unreal, or change it to not use the Store function.
    * If you're using Online SubSystem GooglePlay plugin, edit the /Config/Android/AndroidEngine.ini file.
            
            [OnlineSubsystemGooglePlay.Store]
            bSupportsInAppPurchasing=False
            bUseGooglePlayBillingApiV2=False
            
    * If you're using Online SubSystem Online SubSystem iOS plugin, edit the /Config/IOS/IOSEngine.ini file.
            
            [OnlineSubsystemIOS.Store]
            bSupportsInAppPurchasing=False           

### Purchase Flow

Purchase of an item can be divided into Purchase Flow, Consume Flow, and Reprocess Flow.
It is recommended to implement the Purchase Flow in the following order:

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. If the previous purchase was not completed normally, the purchase will fail if the reprocessing is not performed. Therefore, call **RequestItemListOfNotConsumed** before the purchase to run reprocessing, so that the Consume Flow is executed if there are any unprovided items.
2. The game client attempts to make a purchase by calling **RequestPurchase** of the Gamebase SDK.
3. If the purchase is successful, call **RequestItemListOfNotConsumed** to check the unconsumed purchase details, and if there is an item to provide, proceed with the Consume Flow.

### Consume Flow

If there's a value on the list of unconsumed purchases, proceed with the Consume Flow in the following order:

> <font color="red">[Caution]</font><br/>
>
> To prevent duplicate provision of an item, always check whether the item is being provided in duplicate in the game server.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. The game client makes a request to the game server to consume the purchase item.
    * Passes UserID, paymentSeq, and purchaseToken.
2. The game server checks the game DB to see if there is a history of already providing an item with the same paymentSeq.
    * 2-1. If the item has not been provided yet, call the Gamebase server's Payment Transaction API to verify that the paymentSeq value is matched with the response field and purchaseToken value is valid.
        * [Game > Gamebase > API Guide > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseToken corresponds to **accessToken** in the server API guide document 
    * 2-2. gamebaseProductId can be found in the response field of the server's Payment Transaction API 
        * gamebaseProductId also exists in the client's used payment history list, but the value may not be available at the time of reprocessing, so please use the gamebaseProductId value obtained from the server's Payment Transaction API 
    * 2-3. If the Payment Transaction API call succeeds and the purchaseToken is confirmed to be valid, issues the item corresponding to the gamebaseProductId to the UserID 
    * 2-4. After issuing the item, saves the UserID, gamebaseProductId, paymentSeq, and purchaseToken in the game DB to prevent duplicate payments or re-payments 
3. Regardless of whether the item has been provided, the game server completes the item provision by calling the Gamebase server's consume API.
    * [Game > Gamebase > API Guide > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* There are cases where the store purchase has been made successfully but the process was not properly completed due to errors.
* Call **RequestItemListOfNotConsumed** to run reprocessing and proceed with the Consume Flow for any unprovided items.
* It is recommended to call reprocessing at the following times:
    * After login is completed
    * Before making a purchase
    * When entering the store (or lobby) in a game
    * When checking the user profile or mailbox

### Purchase Item

By using itemSeq of an item to purchase, call the following API and request a purchase.  
If a game user cancels purchase, the **PURCHASE_USER_CANCELED** error is returned.


**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestPurchase(const FString& GamebaseProductId, const FGamebasePurchasableReceiptDelegate& Callback);
void RequestPurchase(const FString& GamebaseProductId, const FString& payload, const FGamebasePurchasableReceiptDelegate& Callback);
```

**Example**
```cpp
void USample::RequestPurchase(const FString& GamebaseProductId)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestPurchase(GamebaseProductId, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* PurchasableReceipt, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->Price, *PurchasableReceipt->Currency,
                *PurchasableReceipt->PaymentSeq, *PurchasableReceipt->PurchaseToken);
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(LogTemp, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(LogTemp, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
            }

        }
    }));
}

void USample::RequestPurchaseWithPayload(const FString& GamebaseProductId)
{
    FString UserPayload = TEXT("{\"description\":\"This is example\",\"channelId\":\"delta\",\"characterId\":\"abc\"}");
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestPurchase(GamebaseProductId, UserPayload, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* PurchasableReceipt, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->price, *PurchasableReceipt->Currency,
                *PurchasableReceipt->PaymentSeq, *PurchasableReceipt->PurchaseToken);

            FString payload = PurchasableReceipt->payload;
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(LogTemp, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(LogTemp, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
            }
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableReceipt
{   
    // The product ID of a purchased item.
    FString GamebaseProductId;

    // An identifier for Legacy API that purchases products with itemSeq.
    int64 ItemSeq;

    // The price of purchased product.
    float Price;

    // Currency code.
    FString Currency;

    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with purchaseToken.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    FString PaymentSeq;

    // Payment identifier.
    // This is an important piece of information used to call 'Consume' server API with paymentSeq.
    // In Consume API, the parameter must be named 'accessToken' to be passed.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // Caution: Call Consume API from game server!
    FString PurchaseToken;

    // The product ID that is registered to store console such as Google or Apple.
    FString MarketItemId;

    // The product type which can have the following values:
    // * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE: A consumable product.
    // * AUTO_RENEWABLE: A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    FString ProductType;

    // This is a user ID with which a product is purchased.
    // If a user logs in with a user ID that is not used to purchase a product, the user cannot obtain the product they purchased.
    FString UserId;

    // The payment identifier of a store.
    FString PaymentId;

    // paymentId is changed whenever product subscription is renewed.
    // This field shows the paymentId that was used when a subscription product was first purchased.
    // This value does not guarantee to be always valid, as it can have no value depending on the store
    // the user made a purchase and the status of the payment server.
    FString OriginalPaymentId;
    
    // The time when the product was purchased.(epoch time)
    int64 PurchaseTime;
    
    // The time when the subscription expires.(epoch time)
    int64 ExpiryTime;
    
    // This is a code for store where purchase is made.
    // The store code list can be found in the GamebaseStoreCode class.  
    FString StoreCode;
    
    // The value sent to payload when callign the RequestPurchase API.
    // Not guarantee to use due to possible loss of information depending on the store server status.
    FString Payload;
    
    // Whether it is promotion or not
    // - (Android) If the validation logic is temporarily turned off in the Gamebase payment server, a valid value is not guaranteed because this value will be output as false only.
    bool bIsPromotion;

    // Whether it is test purchase or not
    // - (Android) If the validation logic is temporarily turned off in the Gamebase payment server, a valid value is not guaranteed because this value will be output as false only.
    bool bIsTestPurchase;
};
```


### List Purchasable Items

To query the list of items, call the following API: 
The list of callback returns includes information of each item. 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestItemListPurchasable(const FGamebasePurchasableItemListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestItemListPurchasable()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestItemListPurchasable(FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* PurchasableItemList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& PurchasableItem : *PurchasableItemList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *PurchasableItem.GamebaseProductId, PurchasableItem.Price, *PurchasableItem.Currency, *PurchasableItem.ItemName, *PurchasableItem.MarketItemId);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListPurchasable failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableItem
{
    // The product ID that is registered with the Gamebase console.
    // Used when a product is purchased using Gamebase.Purchase.requestPurchase API.
    FString GamebaseProductId;

    // An identifier for Legacy API that purchases products with itemSeq.
    int64 ItemSeq;

    // The price of a product.
    float Price;

    // Currency code.
    FString Currency;

    // The name of a product registered in the Gamebase console.
    FString ItemName;

    // The product ID that is registered to store console such as Google or Apple.
    FString MarketItemId;

    // The product type which can have the following values:
    // * UNKNOWN: An unknown type. Either update Gamebase SDK or contact Gamebase Customer Center.
    // * CONSUMABLE: A consumable product.
    // * AUTO_RENEWABLE: A subscription product.
    // * CONSUMABLE_AUTO_RENEWABLE: This 'consumable subscription product' is used when providing a subscribed user a subscription product that can be consumed periodically.
    FString ProductType;
    
    // Localized price information with currency symbol.
    FString LocalizedPrice;
    
    // The name of a localized product registered with the store console.
    FString LocalizedTitle;

    // The description of a localized product registered with the store console.
    FString LocalizedDescription;

    // Shows whether the product is 'used or not' in the Gamebase console.
    bool bIsActive;
};
```


### List Non-Consumed Items

Send a request for unconsumed purchases of which items have not been normally consumed (delivered or provided) even after purchased. 
When there's an unconsumed purchase, send a request to the game server (item server) so as to deliver (provide) items. 


**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| bAllStores                       | O                          | Return unconsumed lists purchased with the same UserID from a different store.<br/>Default is **false**. |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestItemListOfNotConsumed(bool bAllStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestItemListOfNotConsumed(Configuration, FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* PurchasableItemList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(LogTemp, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableReceipt& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestItemListOfNotConsumed failed. (Error: %d)"), Error->Code);
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

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| allStores                       | O                          | Return unconsumed lists purchased with the same UserID from a different store.<br/>Default is **false**. |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& Callback);
```

**Example**
```cpp
void USample::RequestActivatedPurchases(bool bAllStores)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestActivatedPurchases(Configuration, FGamebasePurchasableReceiptListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableReceipt>* purchasableReceiptList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestActivatedPurchases failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

### List Subscriptions Status

Retrieve the status of subscription products based on the current user ID.
The list returned in the callback contains information about the subscription products.

> <font color="red">[Caution]</font><br/>
>
> * The subscription status code is only returned correctly if you follow the guide below to set up the subscription event.
>     * Go to [Game > Gamebase > Store Console Guide > Google Console Guide and set up event propagation of real-time subscription information within Google's system
>     * The status code for a subscription product purchased without setting up events always returns 0 (PURCHASED).
> * Subscription products currently only supports Google Play Store.


**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                 |
| ------------------------------- | -------------------------- | ----------------------------------------------------------- |
|  bIncludeExpiredSubscriptions    | O                          | Retrieves including expired subscription products.<br/>Default value is **false**.   |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestSubscriptionsStatus(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableSubscriptionStatusDelegate& Callback);
```

**Example**
```cpp
void USample::RequestSubscriptionsStatus(bool bIncludeExpiredSubscriptions)
{
    FGamebasePurchasableConfiguration Configuration;
    Configuration.bAllStores = bAllStores;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetPurchase()->RequestSubscriptionsStatus(Configuration, FGamebasePurchasableSubscriptionStatusDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableSubscriptionStatus>* purchasableReceiptList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(LogTemp, Display, TEXT("RequestSubscriptionsStatus succeeded."));

            for (const FGamebasePurchasableSubscriptionStatus& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(LogTemp, Display, TEXT(" - gamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *purchasableReceipt.gamebaseProductId, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(LogTemp, Display, TEXT("RequestSubscriptionsStatus failed. (error: %d)"), error->code);
        }
    }));
}
```

**VO**
```cpp
struct FGamebasePurchasableSubscriptionStatus
{
    // This is the code defined internally by Gamebase for the store where your app is installed.
    FString StoreCode;
    
    // The payment identifier of a store
    FString PaymentId;

    // The paymentId is changed every time subscription product is renewed.
    // This field provides the paymentId of the first time the subscription is paid for. 
    // This value does not guarantee to be always valid, 
    // because the value may not exist depending on the status of the store or payment server.
    FString OriginalPaymentId;

    // Payment identifier
    // This is important information used to call the ‘Consume’ server API along with purchaseToken.
    //    
    // Caution: Call the Consume API from the game server! (https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap)
    FString PaymentSeq;

    // Product ID for the purchased product.
    FString MarketItemId;
    
    // Item unique identifier in the IAP web console
    int64 ItemSeq;

    // Contains one of the following values.
    // * UNKNOWN: Unknown type. Update Gamebase SDK or contact Gamebase customer center.
    // * CONSUMABLE: a consumable.
    // * AUTO_RENEWABLE: a subscription product.
    FString ProductType;

    // User ID that purchased the product.
    // If you log in not with a user ID that purchased the product, you cannot get the purchased product.
    FString UserId;
    
    // Product price.
    float Price;

    // Currency information.
    FString Currency;

    // Payment identifier.
    // As paymentSeq, important information to call the 'Consume' server API.
    // It is sent only when the parameter name is set to ‘access_Token’ from Consume API.
    // Note: https://docs.toast.com/ko/Game/Gamebase/ko/api-guide/#purchase-iap
    FString PurchaseToken;

    // Time of product purchase .(epoch time)
    int64 PurchaseTime;
    
    // Time the subscription expires.(epoch time)
    int64 ExpiryTime;
    
    // A value sent to payload when calling the RequestPurchase API.
    // Not guarantee to use due to possible loss of information depending on the store server status.
    FString Payload;
    
    // Subscription status
    // For all status codes, see the following documentation.
    // - https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatus
    int32 StatusCode;
    
    // Description for subscription status.
    FString StatusDescription;
    
    // Product ID registered in Gamebase console.
    // It is used to purchase products with the RequestPurchase API.
    FString GamebaseProductId;
    
    // This value is used when purchasing from Google and can have the following values.
     // However, if the authentication logic is temporarily disabled on the Gamebase payment server due to an error in the Google server,
     // only returns null, so may not always guarantee a valid value.
     // * null: normal payment
     // * test: test payment
     // * Promotion: promotion payment
    FString PurchaseType;
};
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
| PURCHASE_EXTERNAL_LIBRARY_ERROR             | 4201       | Error in NHN Cloud IAP library.<br>Check the error details. |
| PURCHASE_UNKNOWN_ERROR                      | 4999       | Unknown error in purchase.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error is returned when an error occurs in NHN Cloud IAP library.
* The information on the error in NHN Cloud IAP library is included in the error details, and you can find detailed error code and message as follows.

```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(LogTemp, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Message);

    GamebaseInnerError* moduleError = gamebaseError.Error; // GamebaseError.Error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(LogTemp, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleerror->Code, *moduleerror->Message);
    }
}
```

* For NHN Cloud IAP error codes, refer to the document below.
    * [NHN Cloud > User Guide for NHN Cloud SDK > NHN Cloud IAP > Unity > Error Codes](https://docs.toast.com/en/TOAST/en/toast-sdk/iap-unity/#error-code)





