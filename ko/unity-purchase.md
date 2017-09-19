## Game > Gamebase > Unity Developer's Guide > Purchase

## Purchase

### Settings

> 각 플랫폼별 셋팅 부분을 참고하시기 바랍니다.
> * [Android Purchase Settings](aos-purchase#settings)
> * [iOS Purchase Settings](ios-purchase#project-settings)

###  Purchase Flow

아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.
1. **RequestPurchase** 를 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 **RequestItemListOfNotConsumed** 를 호출하여 미소비 결제내역을 확인합니다.
3. 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume 을 요청합니다.
4. 게임 서버는 IAP서버에 서버 API를 통해 Consume 을 요청합니다.
5. IAP서버에서 Consume이 성공하였다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.<br/>


스토어 결제는 성공하였으나 에러가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 다음 두 API를 각각 호출하여 재처리 로직을 구현하시기 바랍니다.

1. 미처리 아이템 배송 요청
	* 로그인이 성공하면 **RequestItemListOfNotConsumed** 를 호출하여 미소비 결제내역을 확인합니다.
	* 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
2. 결제 오류 재처리 시도
    * 로그인이 성공하면 **RequestRetryTransaction** 을 호출하여 미처리 내역에 대한 자동 재처리를 시도합니다.
    * 리턴된 successList 에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
    * 리턴된 failList 에 값이 존재한다면 해당 값을 게임 서버나 Log&Crash 등을 통해 전송하여 데이터를 확보하고, 빌링 개발팀에 재처리 실패 원인을 문의합니다.<br/>


### Purchase item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.
유저가 구매를 취소하는 경우 **PURCHASE_USER_CANCELED** 에러가 리턴됩니다.

**API**
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

```cs
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**

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

### Get a list of items purchasable

아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 List 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

**API**
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**Example**

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

### Get a list of items non-consumed

아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 미소비 결제내역을 요청합니다. 
미결제 내역이 있는 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

**API**
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>
```cs
static void RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**

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

### Reprocess purchase transaction

스토어 결제는 정상적으로 이루어졌지만, ToastCloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에, 해당 API를 이용하여 재처리를 시도합니다. 최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.

**API**
<div align="left" width = "100">
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
</div>
```cs
static void RequestRetryTransaction(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableRetryTransactionResult> callback)
```

**Example**

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

### Error Handling

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| PURCHASE_NOT_INITIALIZED | 4001 | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-iap 모듈을 프로젝트에 추가 했는지 확인해주세요. |
| PURCHASE_USER_CANCELED | 4002 | 유저가 아이템 구매를 취소하였습니다. |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다. |
| PURCHASE_NOT_ENOUGH_CASH | 4004 | 해당 스토어의 캐쉬가 부족하여 결제할 수 없습니다. |
| PURCHASE_NOT_SUPPORTED_MARKET | 4010 | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), TS(ONE Store), TEST 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | IAP 라이브러리 에러입니다.<br>DetailCode를 확인하세요. |
| PURCHASE_UNKNOWN_ERROR | 4999 | 정의되지 않은 구매 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |

#### PURCHASE_EXTERNAL_LIBRARY_ERROR

* 이 에러는 IAP 모듈에서 발생한 에러입니다.
* IAP 에러 코드를 확인하시기 바랍니다.
	* [LINK \[IAP > Error Code Guide > Client API 에러 타입\]](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)

