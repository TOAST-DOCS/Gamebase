## Game > Gamebase > Android Developer's Guide  > Purchase

## Purchase

### Settings

#### 1. Store Console

* 다음 IAP 가이드를 참고하여 각 스토어에 앱을 등록 하고 어플리케이션 키를 발급받도록 합니다.
* [LINK \[IAP > Store interlocking information\]](http://docs.cloud.toast.com/ko/Common/IAP/ko/Store%20interlocking%20information/)

#### 2. Register as Store's Tester

* 결제 테스트를 위하여 스토어별로 다음과 같이 테스터로 등록합니다.
	* Google
		* [LINK \[Android > 테스트 구매 설정\]](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
	* ONE store
		* [LINK \[ONE store > 인앱결제 테스트\]](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
		* 반드시 In-App 정보 - 테스트 버튼으로 샌드박스를 원하는 단말기 전화번호를 등록해서 테스트 해야 합니다.
		* 테스트용 단말기는 USIM이 있어야 하고, 전화번호를 등록해야 합니다.(MDN)
		* **ONE store** 어플리케이션이 설치되어 있어야 합니다.

#### 3. TOAST Cloud Console

* IAP 가이드를 참고하여 IAP 설정 및 상품등록을 합니다.
	* [LINK \[IAP > Getting Started\]](http://docs.cloud.toast.com/ko/Common/IAP/ko/Web%20Console/)

#### 4. Download

* 다운로드 받은 SDK의 **gamebase-adapter-purchase-iap** 폴더를 프로젝트에 추가합니다.
	* ONE Store 결제가 필요 없다면 **iap-tstore-x.x.x.jar**, **iap_tstore_plugin_vxx.xx.xx.jar** 파일은 삭제하셔도 됩니다.
	* 반대로 ONE Store 결제를 하신다면 위의 jar파일은 반드시 프로젝트에 포함되어 빌드되어야 합니다.

#### 5. AndroidManifest.xml(ONE store only)

* ONE store 사용을 위해서는 다음 설정을 추가하여야 합니다.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:api_version" android:value="4" /> <!-- 버전 16.XX.XX의 경우, 4를 입력합니다. https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#iapapi_version-%EC%84%A4%EC%A0%95 -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development:개발모드 / release:운영 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Gamebase 초기화시 configuration의 **setStoreCode()**를 호출합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
	* GG : Google
	* TS : ONE store
	* TEST : IAP 테스트용

```java
String STORE_CODE = "GG";	// Google

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
        .setStoreCode(STORE_CODE)	// Store code를 반드시 선언합니다.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Purchase Flow

아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.<br/>

1. requestPurchase 를 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 requestItemListOfNotConsumed 를 호출하여 미소비 결제내역을 확인합니다.
3. 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume 을 요청합니다.
4. 게임 서버는 IAP서버에 서버 API를 통해 Consume 을 요청합니다.
5. IAP서버에서 Consume이 성공하였다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.
	
스토어 결제는 성공하였으나 에러가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 다음 두 API를 각각 호출하여 재처리 로직을 구현하시기 바랍니다. <br/>

1. 미처리 아이템 배송 요청
	* 로그인이 성공하면 requestItemListOfNotConsumed 를 호출하여 미소비 결제내역을 확인합니다.
	* 리턴된 미소비 결제내역 리스트에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
	
2. 결제 오류 재처리 시도
	* 로그인이 성공하면 requestRetryTransaction 을 호출하여 미처리 내역에 대한 자동 재처리를 시도합니다.
	* 리턴된 successList 에 값이 존재한다면 게임 클라이언트가 게임 서버에 Consume을 요청하여 아이템 지급 처리를 합니다.
	* 리턴된 failList 에 값이 존재한다면 해당 값을 게임 서버나 Log&Crash 등을 통해 전송하여 데이터를 확보하고, 빌링 개발팀에 재처리 실패 원인을 문의합니다.

### Purchase Item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다. <br/>
유저가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 에러가 리턴됩니다. 취소 처리를 해주시기 바랍니다.

```java
long itemSeq; // The itemSeq value can be got through the requestItemListPurchasable API.

Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else if(exception.getCode() == GamebaseError.PURCHASE_USER_CANCELED) {
            // User canceled.
        } else {
            // To Purchase Item Failed cause of the error
        }
    }
});
```

### Get a List of Purchasable Items

아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

```java
Gamebase.Purchase.requestItemListPurchasable(activity, new GamebaseDataCallback<List<PurchasableItem>>() {
    @Override
    public void onCallback(List<PurchasableItem> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Get a List of Non-Consumed Items

아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 **미소비 결제내역**을 요청합니다.<br/>
해당 내역을 받은 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

* 다음 두가지 상황에서 호출해 주세요.
    1. 결제 성공 후 아이템 Consume 처리 전 최종 확인을 위하여 호출.
    2. 로그인 성공 후 Consume하지 못한 아이템이 남아있지는 않은지 확인 하기 위하여 호출.

```java
Gamebase.Purchase.requestItemListOfNotConsumed(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request item list failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Reprocess Failed Purchase Transaction

스토어 결제는 정상적으로 이루어졌지만, TOAST Cloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에, 해당 API를 이용하여 재처리를 시도합니다. <br/>
최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.

```java
Gamebase.Purchase.requestRetryTransaction(activity, new GamebaseDataCallback<PurchasableRetryTransactionResult>() {
    @Override
    public void onCallback(PurchasableRetryTransactionResult data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request retry transaction failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
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

* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 에러는 IAP 모듈에서 발생한 에러입니다.
* exception.getDetailCode() 를 통해 IAP 에러 코드를 확인하여야 합니다.
* IAP 에러코드는 다음 문서를 참고하시기 바랍니다.
* [LINK \[IAP > Error Code Guide > Client API 에러 타입\]](http://docs.cloud.toast.com/ko/Common/IAP/ko/Error%20Code/#client-api)

