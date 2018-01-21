## Game > Gamebase > Android SDK 사용 가이드 > 결제

여기에서는 앱에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Settings

#### 1. Store Console

* 다음 IAP 가이드를 참고하여 각 스토어에 앱을 등록하고 앱 키를 발급받습니다.
* [Mobile Service > IAP > 콘솔 사용 가이드 > Store interlocking information](http://alpha-docs.cloud.toast.com/ko/Mobile%20Service/IAP/ko/console-guide/#Store%20interlocking%20information/)

#### 2. Register as Store's Tester

* 결제 테스트를 위하여 스토어별로 다음과 같이 테스터로 등록합니다.
  * Google
    * [Android > 테스트 구매 설정](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
  * ONE store
    * [ONE store > 인앱결제 테스트](https://github.com/ONE-store/inapp-sdk/wiki/IAP-Developer-Guide#%EC%9D%B8%EC%95%B1%EA%B2%B0%EC%A0%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8)
    * 반드시 인앱 정보 - 테스트 버튼으로 샌드박스를 원하는 단말기 전화번호를 등록해서 테스트해야 합니다.
    * 테스트용 단말기는 USIM이 있어야 하고, 전화번호를 등록해야 합니다(MDN).
    * **ONE store** 어플리케이션이 설치되어 있어야 합니다.

#### 3. TOAST IAP 서비스 이용

* IAP 가이드를 참고하여 IAP를 설정하고 아이템을 등록합니다.
  * [Mobile Service > IAP > 콘솔 사용 가이드](http://alpha-docs.cloud.toast.com/ko/Mobile%20Service/IAP/ko/console-guide/)

#### 4. Download

* 다운로드한 SDK의 **gamebase-adapter-purchase-iap** 폴더를 프로젝트에 추가합니다.
  * ONE store 결제가 필요 없다면 **iap-tstore-x.x.x.jar**, **iap_tstore_plugin_vxx.xx.xx.jar** 파일은 삭제해도 됩니다.
  * 반대로 ONE store 결제를 한다면 위의 jar 파일은 반드시 프로젝트에 포함해 빌드해야 합니다.

#### 5. AndroidManifest.xml(ONE store only)

* ONE store을 사용하려면 다음 설정을 추가해야 합니다.

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

* Gamebase 초기화 시 configuration의 **setStoreCode()**를 호출합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
  * GG: Google
  * TS: ONE store
  * TEST: IAP 테스트용

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

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. 게임 클라이언트에서는 Gamebase SDK의 **requestPurchase**를 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 **requestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인합니다.
3. 반환된 미소비 결제 내역 목록에 값이 있으면 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
4. 게임 서버는 Gamebase 서버에 API를 통해 consume(소비) API를 요청합니다.
   [API 가이드](http://alpha-docs.cloud.toast.com/ko/Game/Gamebase/ko/api-guide/#wrapping-api)
5. IAP 서버에서 consume(소비) API 호출에 성공했다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.

스토어 결제는 성공했으나 오류가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 다음 두 API를 각각 호출하여 재처리 로직을 구현하시기 바랍니다. <br/>

1. 미처리 아이템 배송 요청
  * 로그인에 성공하면 **requestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인합니다.
  * 반환된 미소비 결제 내역 목록에 값이 존재한다면 게임 클라이언트가 게임 서버에 consume(소비)를 요청하여 아이템을 지급합니다.

2. 결제 오류 재처리 시도
  * 로그인에 성공하면 **requestRetryTransaction**을 호출하여 미처리 내역에 대해 자동으로 재처리를 시도합니다.
  * 반환된 successList에 값이 존재한다면 게임 클라이언트가 게임 서버에 consume(소비)를 요청하여 아이템을 지급합니다.
  * 반환된 failList에 값이 존재한다면 해당 값을 게임 서버나 Log & Crash 등을 통해 전송하여 데이터를 확보하고, **[고객센터](https://alpha.toast.com/support/inquiry)**에 재처리 실패 원인을 문의합니다.

### Purchase Item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출해 구매를 요청합니다. <br/>
게임 이용자가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 오류가 반환됩니다. 취소 처리를 해 주시기 바랍니다.

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

아이템 목록을 조회하려면 다음 API를 호출합니다. 콜백으로 반환되는 배열(array) 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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

아이템을 구매했지만, 정상적으로 아이템이 소비(배송, 지급)되지 않은 미소비 결제 내역을 요청합니다.<br/>
미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다.

* 다음 두 가지 상황에서 호출해 주세요.
    1. 결제 성공 후 아이템 소비(consume) 처리 전 최종 확인을 위하여 호출
    2. 로그인 성공 후 소비(consume)하지 못한 아이템이 남아 있지는 않은지 확인하기 위하여 호출

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

스토어에서는 결제가 정상적으로 되었으나, TOAST IAP 서버 검증 실패 등으로 정상적으로 결제되지 않은 경우에는,  API를 이용해 재처리를 시도합니다. <br/>
마지막으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급) 등의 API를 호출해 처리해야 합니다.

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

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해주세요. |
| PURCHASE_USER_CANCELED                   | 4002       | 게임 이용자가 아이템 구매를 취소하였습니다.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다.     |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 해당 스토어의 캐시가 부족하여 결제할 수 없습니다.             |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), TS(ONE store), TEST 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP 라이브러리 오류입니다.<br>DetailCode를 확인하세요.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://alpha.toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
  - [오류 코드](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 IAP 모듈에서 발생한 오류입니다.
* exception.getDetailCode()를 통해 IAP 오류 코드를 확인해야 합니다.
* IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
  - [Mobile Service > IAP > 오류 코드 > Client API 에러 타입](http://alpha-docs.cloud.toast.com/ko/Mobile%20Service/IAP/ko/error-code/#client-api)

