## Game > Gamebase > Unreal SDK 사용 가이드 > 결제

여기에서는 Unreal에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.
Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Settings

Android나 iOS에서 인앱 결제 기능을 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)
* [Windows Purchase Settings](unreal-started/#windows-settings)

#### Unreal Plugin 설정

> <font color="red">[주의]</font><br/>
>
> 외부 플러그인에서 결제 관련 처리가 있는 경우, Gamebase 결제 기능이 정상적으로 동작하지 않을 수 있습니다.

* Unreal에서 기본으로 활성화 되어있는 Online SubSystem 플러그인을 비활성화 혹은 스토어 기능을 이용하지 못하도록 변경해야 합니다.
    * Online SubSystem GooglePlay 플러그인 사용 시 /Config/Android/AndroidEngine.ini 파일을 편집합니다.
            
            [OnlineSubsystemGooglePlay.Store]
            bSupportsInAppPurchasing=False
            bUseGooglePlayBillingApiV2=False
            
    * Online SubSystem iOS 플러그인 사용 시 /Config/IOS/IOSEngine.ini 파일을 편집합니다.
            
            [OnlineSubsystemIOS.Store]
            bSupportsInAppPurchasing=False

### Purchase Flow

아이템 구매는 크게 결제 Flow 와 Consume Flow, 재처리 Flow 로 나누어 볼 수 있습니다.
결제 Flow는 다음과 같은 순서로 구현하시기 바랍니다.

![purchase flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.10.0.png)

1. 이전 결제가 정상적으로 종료되지 못한 경우 재처리가 동작하지 않으면 결제가 실패합니다. 그러므로 결제 전에 **RequestItemListOfNotConsumed**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 Consume Flow 를 진행합니다.
2. 게임 클라이언트에서는 Gamebase SDK의 **RequestPurchase**를 호출하여 결제를 시도합니다.
3. 결제가 성공했다면 **RequestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인한 후 지급할 아이템이 존재한다면 Consume Flow 를 진행합니다.

### Consume Flow

미소비 결제 내역 목록에 값이 있으면 다음과 같은 순서로 Consume Flow 를 진행하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 아이템이 중복 지급되는 일이 발생하지 않도록, 게임 서버에서 반드시 중복 지급 여부를 체크하시기 바랍니다.
>

![consume flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_002_2.64.0.png)

1. 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
    * UserID, paymentSeq, purchaseToken을 전달합니다.
2. 게임 서버는 게임 DB에 이미 동일한 paymentSeq로 아이템을 지급한 이력이 있는지 확인합니다.
    * 2-1. 아직 아이템을 지급하지 않았다면 Gamebase 서버의 Payment Transaction API를 호출하여 purchaseToken이 유효한지, 응답 필드의 paymentSeq와 일치하는지 검증합니다.
        * [Game > Gamebase > API 가이드 > Purchase(IAP) > Get Payment Transaction](./api-guide/#get-payment-transaction)
        * purchaseToken이 서버 API 가이드 문서의 **accessToken**에 해당합니다.
    * 2-2. gamebaseProductId는 서버의 Payment Transaction API의 응답 필드에서 확인할 수 있습니다.
        * 클라이언트의 미소비 결제 내역 목록에도 gamebaseProductId가 존재하지만, 재처리 시에는 해당 값이 없을 수도 있으므로 서버의 Payment Transaction API로부터 획득한 GamebaseProductId 값을 사용하시기 바랍니다.
    * 2-3. Payment Transaction API 호출이 성공하여 purchaseToken이 정상이라고 확인되면 UserID에 gamebaseProductId에 해당하는 아이템을 지급합니다.
    * 2-4. 아이템 지급 후 게임 DB에 UserID, GamebaseProductId, paymentSeq, purchaseToken을 저장하여 중복 지급 방지 또는 재지급을 할 수 있도록 합니다.
3. 아이템 지급 여부와 무관하게 게임 서버는 더 이상 미소비 내역이 리턴되지 않도록 Gamebase 서버의 consume(소비) API를 호출하여 아이템 지급을 완료합니다.
    * [Game > Gamebase > API 가이드 > Purchase(IAP) > Consume](./api-guide/#consume)

### Retry Transaction Flow

![retry transaction flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_retry_transaction_flow_2.19.0.png)

* 스토어 결제에는 성공했으나 오류가 발생해 정상 종료되지 못하는 경우가 있습니다.
* **RequestItemListOfNotConsumed**를 호출하여 재처리를 동작시켜 미지급된 아이템이 있으면 Consume Flow 를 진행하세요.
* 재처리는 다음과 같은 시점에 호출할 것을 권장합니다.
    * 로그인 완료 후.
    * 결제 전.
    * 게임 내 상점(또는 로비) 진입시.
    * 유저 프로필 또는 우편함 확인시.

### Purchase Item

구매하고자 하는 아이템의 gamebaseProductId를 이용해 다음의 API를 호출하여 구매를 요청합니다.
게임 유저가 구매를 취소하는 경우 **PURCHASE_USER_CANCELED** 오류가 반환됩니다.


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
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->Price, *PurchasableReceipt->Currency,
                *PurchasableReceipt->PaymentSeq, *PurchasableReceipt->PurchaseToken);
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
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
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                *PurchasableReceipt->GamebaseProductId, PurchasableReceipt->price, *PurchasableReceipt->currency,
                *PurchasableReceipt->paymentSeq, *PurchasableReceipt->purchaseToken);

            FString payload = PurchasableReceipt->payload;
        }
        else
        {
            if (Error->Code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                // Check the Error code and handle the Error appropriately.
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (Error: %d)"), Error->Code);
            }
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableReceipt
{   
    // 구매한 아이템의 상품 ID입니다.
    FString GamebaseProductId;

    // itemSeq 로 상품을 구매하는 Legacy API용 식별자입니다.
    int64 ItemSeq;

    // 구매한 상품의 가격입니다.
    float Price;

    // 통화 코드입니다.
    FString Currency;

    // 결제 식별자입니다.
    // purchaseToken과 함께 'Consume' 서버 API를 호출하는 데 사용하는 중요한 정보입니다.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요! 
    FString PaymentSeq;

    // 결제 식별자입니다.
    // paymentSeq 와 함께 'Consume' 서버 API를 호출하는데 사용하는 중요한 정보입니다.
    // Consume API 에서는 'accessToken' 라는 이름의 파라메터로 전달해야 합니다.
    // Consume API : https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap
    // 주의 : Consume API 는 게임 서버에서 호출하세요! 
    FString PurchaseToken;

    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID입니다.
    FString MarketItemId;

    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객 센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTO_RENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    FString ProductType;

    // 상품을 구매했던 User ID.
    // 상품을 구매하지 않은 User ID 로 로그인 한다면 구매한 아이템을 획득할 수 없습니다.
    FString UserId;

    // 스토어의 결제 식별자입니다.
    FString PaymentId;

    // 구독 상품은 갱신될 때마다 paymentId가 변경됩니다.
    // 이 필드는 맨 처음 구독 상품을 결제 했을때의 paymentId 를 알려줍니다.
    // 스토어에 따라, 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    // 항상 유효한 값을 보장하지는 않습니다.
    FString OriginalPaymentId;
    
    // 상품을 구매했던 시각입니다.(epoch time)
    int64 PurchaseTime;
    
    // 구독이 종료되는 시각입니다.(epoch time)
    int64 ExpiryTime;
    
    // 결제한 스토어 코드입니다.
    // GamebaseStoreCode 클래스에서 스토어 코드 목록을 확인할 수 있습니다.
    FString StoreCode;
    
    // RequestPurchase API 호출 시 payload로 전달했던 값입니다.
    // 스토어 서버 상태에 따라 정보가 유실되는 경우가 있으므로 사용을 권장하지 않습니다.
    FString Payload;
    
    // 프로모션 결제 여부
    // - (Android) Gamebase 결제 서버에서 일시적으로 검증 로직을 끄는 경우에는 false로만 출력되므로 유효한 값이 보장되지 않습니다.
    bool bIsPromotion;

    // 테스트 결제 여부
    // - (Android) Gamebase 결제 서버에서 일시적으로 검증 로직을 끄는 경우에는 false로만 출력되므로 유효한 값이 보장되지 않습니다.
    bool bIsTestPurchase;
};
```


### List Purchasable Items

아이템 목록을 조회하려면 다음 API를 호출합니다. 
콜백으로 반환되는 목록 안에는 각 아이템들에 대한 정보가 있습니다.

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
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& PurchasableItem : *PurchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - GamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *PurchasableItem.GamebaseProductId, PurchasableItem.Price, *PurchasableItem.Currency, *PurchasableItem.ItemName, *PurchasableItem.MarketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

**VO**

```cpp
struct FGamebasePurchasableItem
{
    // Gamebase 콘솔에 등록된 상품 ID입니다.
    // Gamebase.Purchase.requestPurchase API로 상품을 구매할 때 사용됩니다.
    FString GamebaseProductId;

    // itemSeq 로 상품을 구매하는 Legacy API용 식별자입니다.
    int64 ItemSeq;

    // 상품의 가격입니다.
    float Price;

    // 통화 코드입니다.
    FString Currency;

    // Gamebase 콘솔에 등록된 상품 이름입니다.
    FString ItemName;

    // Google, Apple 과 같이 스토어 콘솔에 등록된 상품 ID입니다.
    FString MarketItemId;

    // 상품 타입으로, 다음 값들이 올 수 있습니다.
    // * UNKNOWN : 인식 불가능한 타입. Gamebase SDK 를 업데이트 하거나 Gamebase 고객 센터로 문의하세요.
    // * CONSUMABLE : 소비성 상품.
    // * AUTORENEWABLE : 구독형 상품.
    // * CONSUMABLE_AUTO_RENEWABLE : 구독형 상품을 구매한 유저에게 정기적으로 소비가 가능한 상품을 지급하고자 하는 경우 사용되는 '소비가 가능한 구독 상품'.
    FString ProductType;
    
    // 통화 기호가 포함된 현지화 된 가격 정보입니다.
    FString LocalizedPrice;
    
    // 스토어 콘솔에 등록된 현지화된 상품 이름입니다.
    FString LocalizedTitle;

    // 스토어 콘솔에 등록된 현지화된 상품 설명입니다.
    FString LocalizedDescription;

    // Gamebase 콘솔에서 해당 상품의 '사용 여부'를 나타냅니다.
    bool bIsActive;
};
```


### List Non-Consumed Items

아이템을 구매했지만, 정상적으로 아이템이 소비(배송, 지급)되지 않은 미소비 결제 내역을 요청합니다.
미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다.


**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| bAllStores                       | O                          | 동일한 UserID로 다른 스토어에서 구매한 미소비 내역도 반환합니다.<br/>기본값은 **false**입니다. |

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
    Subsystem->GetPurchase()->RequestItemListOfNotConsumed(Configuration, FGamebasePurchasableItemListDelegate::CreateLambda(
        [](const TArray<FGamebasePurchasableItem>* PurchasableItemList, const FGamebaseError* Error)
    {
        if (Gamebase::IsSuccess(Error))
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableItem& PurchasableItem : *PurchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - GamebaseProductId= %s, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableItem.Price, *PurchasableItem.Currency, *PurchasableItem.ItemName, *PurchasableItem.MarketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

### List Actived Subscriptions

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
사용자 ID가 같다면 Android와 iOS에서 구매한 구독 상품이 모두 조회됩니다.

> <font color="red">[주의]</font><br/>
>
> Android에서는 Google Play 스토어에서만 현재 구독 상품을 지원합니다.

**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                                    |
| ------------------------------- | -------------------------- | ------------------------------------------------------------------------------ |
| bAllStores                       | O                          | 동일한 UserID로 다른 스토어에서 구매한 미소비 내역도 반환합니다.<br/>기본값은 **false**입니다. |

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

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
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

### List Subscriptions Status

현재 사용자 ID 기준으로 구독 상품들의 상태를 조회합니다.
콜백으로 반환되는 목록 안에는 구독 상품들의 정보가 담겨 있습니다.

> <font color="red">[주의]</font><br/>
>
> * 아래 가이드에 따라 구독 이벤트를 설정해야 구독 상태 코드가 정상적으로 반환됩니다.
>     * [Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드 > Google 시스템 내 실시간 구독 정보 이벤트 전파 설정](./console-google-guide/#google_1)
>     * 이벤트 설정을 하지 않은 상태에서 구매한 구독 상품의 상태 코드는 항상 0(PURCHASED)이 반환됩니다.
> * 현재 구독 상품은 Google Play 스토어만 지원합니다.


**FGamebasePurchasableConfiguration**

| API                             | Mandatory(M) / Optional(O) | Description                                                 |
| ------------------------------- | -------------------------- | ----------------------------------------------------------- |
| bIncludeExpiredSubscriptions    | O                          | 만료된 구독 상품까지 포함하여 조회합니다.<br/>기본값은 **false**입니다.   |

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
        if (Gamebase::IsSuccess(Error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestSubscriptionsStatus succeeded."));

            for (const FGamebasePurchasableSubscriptionStatus& PurchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - GamebaseProductId= %s, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    *PurchasableReceipt.GamebaseProductId, PurchasableReceipt.Price, *PurchasableReceipt.Currency, *PurchasableReceipt.PaymentSeq, *PurchasableReceipt.PurchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestSubscriptionsStatus failed. (Error: %d)"), Error->Code);
        }
    }));
}
```

**VO**
```cpp
struct FGamebasePurchasableSubscriptionStatus
{
    // 앱이 설치된 스토어에 대해 Gamebase에서 내부적으로 정의한 코드입니다.
    FString StoreCode;
    
    // 스토어의 결제 식별자입니다.
    FString PaymentId;

    // 구독 상품은 갱신될 때마다 paymentId가 변경됩니다.
    // 이 필드는 구독 상품을 최초 결제했을 때의 paymentId를 알려줍니다.
    // 스토어 및 결제 서버 상태에 따라 값이 존재하지 않을 수 있으므로
    // 항상 유효한 값을 보장하지는 않습니다.
    FString OriginalPaymentId;

    // 결제 식별자입니다.
    // purchaseToken과 함께 'Consume' 서버 API를 호출하는 데 사용하는 중요한 정보입니다.
    //    
    // 주의: Consume API는 게임 서버에서 호출하세요! (https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchase-iap)
    FString PaymentSeq;

    // 구매한 상품의 상품 ID입니다.
    FString MarketItemId;
    
    // IAP 웹 콘솔의 항목 고유 식별자
    int64 ItemSeq;

    // 다음 값 중 하나를 가집니다.
    // * UNKNOWN: 알 수 없는 유형입니다. Gamebase SDK를 업데이트하거나 Gamebase 고객 센터에 문의하세요.
    // * CONSUMABLE: 소모품입니다.
    // * AUTO_RENEWABLE: 구독 상품입니다.
    FString ProductType;

    // 상품을 구매한 사용자 아이디입니다.
    // 상품 구매에 사용하지 않은 사용자 아이디로 로그인하면 구매한 상품을 받을 수 없습니다.
    FString UserId;
    
    // 상품의 가격입니다.
    float Price;

    // 통화 정보입니다.
    FString Currency;

    // Payment 식별자.
    // paymentSeq로 'Consume' 서버 API를 호출하는 데 사용되는 중요한 정보입니다.
    // Consume API에서 매개변수 이름을 'accessToken'으로 지정해야 전달됩니다.
    // 참고: https://docs.toast.com/ko/Game/Gamebase/ko/api-guide/#purchase-iap
    FString PurchaseToken;

    // 상품을 구매한 시간.(epoch time)
    int64 PurchaseTime;
    
    // 구독이 만료되는 시간.(epoch time)
    int64 ExpiryTime;
    
    // RequestPurchase API 호출 시 페이로드에 전달되는 값입니다.
    // 스토어 서버 상태에 따라 정보가 유실되는 경우가 있으므로 사용을 권장하지 않습니다.
    FString Payload;
    
    // 구독 상태
    // 전체 상태 코드는 다음 문서를 참조하세요.
    // - https://docs.nhncloud.com/en/TOAST/en/toast-sdk/iap-unity/#iapsubscriptionstatus
    int32 StatusCode;
    
    // 구독 상태에 대한 설명입니다.
    FString StatusDescription;
    
    // Gamebase 콘솔에 등록된 상품 ID입니다.
    // RequestPurchase API로 상품을 구매할 때 사용됩니다.
    FString GamebaseProductId;

    // 이 값은 Google에서 구매할 때 사용되며 다음 값을 가질 수 있습니다.
    // 단, Google 서버의 오류로 인해 Gamebase 결제 서버에서 일시적으로 인증 로직이 비활성화된 경우
    // null만 반환하므로 항상 유효한 값을 보장하지 않을 수 있습니다.
    // * null: 정상 결제
    // * 테스트: 테스트 결제
    // * 프로모션: 프로모션 결제
    FString PurchaseType;
};
```


### Error Handling

| Error                                     | Error Code | Description                              |
| ----------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                  | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해 주세요. |
| PURCHASE_USER_CANCELED                    | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다. |
| PURCHASE_NOT_ENOUGH_CASH                  | 4004       | 해당 스토어의 캐시가 부족해 결제할 수 없습니다.              |
| PURCHASE_INACTIVE_PRODUCT_ID              | 4005       | 해당 상품이 활성화 상태가 아닙니다.  |
| PURCHASE_NOT_EXIST_PRODUCT_ID             | 4006       | 존재하지 않는 GamebaseProductID 로 결제를 요청하였습니다. |
| PURCHASE_LIMIT_EXCEEDED                   | 4007       | 월 구매 한도를 초과했습니다.             |
| PURCHASE_NOT_SUPPORTED_MARKET             | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 AS(App Store), GG(Google), ONESTORE, GALAXY입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR           | 4201       | NHN Cloud IAP 라이브러리 오류입니다.<br/>상세 오류를 확인하십시오. |
| PURCHASE_UNKNOWN_ERROR                    | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./Error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud IAP 라이브러리에서 오류가 발생했을 때 반환됩니다.
* NHN Cloud IAP 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 

```cpp
GamebaseError* gamebaseError = Error; // GamebaseError object via callback

if (Gamebase::IsSuccess(Error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), Error->Code, *Error->Messsage);

    GamebaseInnerError* moduleError = gamebaseError.Error; // GamebaseError.Error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleerror->Code, *moduleerror->Messsage);
    }
}
```

* NHN Cloud IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [NHN Cloud > SDK 사용 가이드 > IAP > Android > 오류 코드](https://docs.nhncloud.com/ko/nhncloud/ko/nhncloud-sdk/iap-android/#_32)
    * [NHN Cloud > SDK 사용 가이드 > IAP > iOS > 에러 코드](https://docs.nhncloud.com/ko/nhncloud/ko/nhncloud-sdk/iap-ios/#_17)




