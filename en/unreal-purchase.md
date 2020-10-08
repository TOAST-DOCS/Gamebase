## Game > Gamebase > User Guide for Unreal SDK > Purchase 결제

This document describes setting requirements to use Unreal in-app purchase.  
The unified purchase API of Gamebase supports for easy integration of in-app purchases of many stores. 

### Settings

Regarding how to set in-app purchases on Android or iOS, read the following documents: <br/>

* [Android Purchase Settings](aos-purchase#settings)
* [iOS Purchase Settings](ios-purchase#settings)

#### Setting for Purchases on Android (for 4.24 or lower engine version)

* When 4.24 is installed via Epic Games Launcher, 를 통해 4.24 버전을 설치한 경우,
    delete **Engine\Build\Android\Java\src\com\android\vending\billing\IInAppBillingService.aidl** to build it properly.  을 삭제해야 정상적으로 빌드할 수 있습니다.
    * [IInAppBillingService.aidl](https://developer.android.com/google/play/billing/api), provided by Gamebase, must be removed to prevent conflicts. 에서 제공하고 있어 충돌이 발생하여 제거가 필요합니다.
    * There's no need to remove it, though, if you're using 4.25 or higher engine versions, or if the engine is provided by github. 4.25 이상 버전이나 github에서 엔진을 받은 경우에는 별도로 제거하실 필요 없습니다.

###  Purchase Flow

You may execute item purchaes in the following order: <br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.6.2.png)


1. For the game client, call **RequestPurchase** of Gamebase SDK to make a purchase. 
2. When purchase has been successful, call **RequestItemListOfNotConsumed** and check history of non-consumable purchases. 
3. When a value exists on the list of returned non-consumable purchases, the game client sends a request to the game server to consume purchased items. 
	* Deliver UserID, itemSeq, paymentSeq, and purchaseToken.
4. The game server checks if history exists in the game database for items paid with same paymentSeq or puchasToken. 
	* 4-1. If items are still unpaid, pay UserID with items for itemSeq.  
    * 4-2. After item is paid, save UserID, itemSeq, paymentSeq, and purchaseToken in the game database to check redundant payment afterwards. 
5. The game server requests Consume API via API to the Gamebase server. 
	* [API Guide](./api-guide/#consume)

<br/>

* In some cases, it is not normally closed due to error, even after payment at store has been successful. Please check the list of non-consumable purchases after login. 
	* When you're successful with login, call **RequestItemListOfNotConsumed** to check non-consumable purchases. 
	* If a value exists on the list of returned non-consumable purchases, the game client requests Consume to the game server and pay items. 

### Purchase Item

By using itemSeq of an item to purchase, call the following API and request a purchase.  
If a game user cancels purchase, the **PURCHASE_USER_CANCELED** error is returned.  


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RequestPurchase(int64 itemSeq, const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::RequestPurchaseSample(int64 itemSeq)
{
    IGamebase::Get().GetPurchase().RequestPurchase(itemSeq, FGamebasePurchasableReceiptDelegate::CreateLambda(
        [](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase succeeded. (itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                purchasableReceipt->itemSeq, purchasableReceipt->price, *purchasableReceipt->currency, *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("User canceled purchase."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("RequestPurchase failed. (error: %d)"), error->code);
            }
            
        }
    }));
}
```

### List Purchasable Items

아이템 목록을 조회하려면 다음 API를 호출합니다. To query the list of items, call the following API: 
콜백으로 반환되는 목록 안에는 각 아이템들에 대한 정보가 담겨 있습니다. The list of callback returns includes information of each item. 

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    purchasableItem.itemSeq, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListPurchasable failed. (error: %d)"), error->code);
        }
    }));
}
```



### List Non-Consumables

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.

            UE_LOG(GamebaseTestResults, Display, TEXT("RequestItemListOfNotConsumed succeeded."));

            for (const FGamebasePurchasableItem& purchasableItem : *purchasableItemList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, itemName= %s, itemName= %s, marketItemId= %s"),
                    purchasableItem.itemSeq, purchasableItem.price, *purchasableItem.currency, *purchasableItem.itemName, *purchasableItem.marketItemId);
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

Query the list of activated subscriptions of a current user ID. 
Paid subscriptions (auto-renewal subscription, or auto-renewal consumable subscription products) can be queried until they are expired. 
Under same user ID, you can query all subscriptions purchased both on Android and iOS.  

> <font color="red">[Caution]</font><br/>
>
> Currently, subscriptions of Android are supported only at Google Playstore. 

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases succeeded."));

            for (const FGamebasePurchasableReceipt& purchasableReceipt : *purchasableReceiptList)
            {
                UE_LOG(GamebaseTestResults, Display, TEXT(" - itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s"),
                    purchasableReceipt.itemSeq, purchasableReceipt.price, *purchasableReceipt.currency, *purchasableReceipt.paymentSeq, *purchasableReceipt.purchaseToken);
            }
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RequestActivatedPurchases failed. (error: %d)"), error->code);
        }
    }));
}
```

### App Store Promotion IAP

Features for purchasing items are available on the App Store app. 
After a successful item purchase, items can be paid through handler registered like below.  

The promotion IAP can be made public by separate configuration at App Store Connect. 

> <font color="red">[Caution]</font><br/>
>
> Available only on iOS 11 or higher. 
> Build is required only for Xcode 9.0 or higher. 
> Supported on Gamebase 1.13.0 or higher. (with TOAST IAP SDK 1.6.0 or higher)


> <font color="red">[Caution]</font><br/>
>
> Call is available only afet a successful login. 
> Must be executed before any other purchase API, after a successful login. 

> <font color="red">[Caution]</font><br/>
>
> Facebook SDK 사용 시, App Dashboard > Settings > Basic 페이지에서 `Log In-App Events Automatically (Recommended)` 설정을 활성화하면 해당 기능이 정상 작동하지 않으니 주의하십시오.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void SetPromotionIAPHandler(const FGamebasePurchasableReceiptDelegate& onCallback);
```

**Example**
```cpp
void Sample::SetPromotionIAPHandler()
{
    IGamebase::Get().GetPurchase().SetPromotionIAPHandler(FGamebasePurchasableReceiptDelegate::CreateLambda([](const FGamebasePurchasableReceipt* purchasableReceipt, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("SetPromotionIAPHandler succeeded. (itemSeq= %ld, price= %f, currency= %s, paymentSeq= %s, purchaseToken= %s)"),
                purchasableReceipt->itemSeq, purchasableReceipt->price, *purchasableReceipt->currency, *purchasableReceipt->paymentSeq, *purchasableReceipt->purchaseToken);
        }
        else
        {
            if (error->code == GamebaseErrorCode::PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("SetPromotionIAPHandler failed. (error: %d)"), error->code);
            }
        }
    }));
}
```
**Overview**

* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f

**How to Test AppStore Promotion IAP**

> `Caution`
> App Store Connect에 앱을 업로드한 다음 TestFlight를 통하여 앱을 설치 후, 테스트를 진행할 수 있습니다.
> 

1. TestFlight로 App을 설치합니다.
2. 아래와 같은 URL Scheme을 호출하여, 테스트를 진행합니다.

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 고정값 |
| host &amp; path | 없음 | 없음 |
| queries | action | purchaseIntent |
|		  | bundleId | 앱의 bundeld identifier |
|		  | productIdentifier | 구매 아이템의 product identifier |

예제) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해 주세요. |
| PURCHASE_USER_CANCELED                   | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003      | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다. |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 해당 스토어의 캐시가 부족해 결제할 수 없습니다.              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), ONESTORE 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP 라이브러리 오류입니다.<br>DetailCode를 확인하세요.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다. See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* The error occurs from IAP module. 
* Here's how to check error codes: 

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

* For IAP error codes, see the following document as reference.
    * [TOAST > User Guide for TOAST SDK > TOAST IAP > Unity > Error Codes](/TOAST/ko/toast-sdk/iap-unity/#_17)





