## Game > Gamebase > Android SDK 사용 가이드 > 결제

여기에서는 앱에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Settings

#### 1. Store Console

* 다음 IAP 가이드를 참고하여 각 스토어에 앱을 등록하고 앱 키를 발급받습니다.
* [Mobile Service > IAP > 콘솔 사용 가이드 > Store interlocking information](/Mobile%20Service/IAP/ko/console-guide/#store-interlocking-information)

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
    * [Mobile Service > IAP > 콘솔 사용 가이드](/Mobile%20Service/IAP/ko/console-guide/)

#### 4. Download

* 다운로드한 SDK의 **gamebase-adapter-purchase-iap** 폴더를 프로젝트에 추가합니다.
    * ONE store 결제가 필요 없다면 **iap-onestore-x.x.x.aar** 파일은 삭제해도 됩니다.
    * 반대로 ONE store 결제를 한다면 위의 jar 파일은 반드시 프로젝트에 포함해 빌드해야 합니다.

#### 5. AndroidManifest.xml(ONE store only)

* ONE store을 사용하려면 다음 설정을 추가해야 합니다.

```xml
<manifest>
    ...
    <application>
    ...
        <!-- [ONE store] Configurations begin -->
        <meta-data android:name="iap:plugin_mode" android:value="development" /> <!-- development:개발모드 / release:운영 -->
        <!-- [ONE store] Configurations end -->
    ...
    </application>
</manifest>
```

#### 6. Initialization

* Gamebase 초기화 시 Store Code 를 지정해야 합니다.
* **STORE_CODE**는 다음 값 중에서 선택합니다.
    * GG: Google
    * ONESTORE: ONE store

```java
String STORE_CODE = "GG";	// Google

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
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
   [API 가이드](./api-guide/#wrapping-api)
5. IAP 서버에서 consume(소비) API 호출에 성공했다면 게임 서버가 게임 클라이언트에 아이템을 지급합니다.

* 스토어 결제는 성공했으나 오류가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 미소비 결제 내역을 확인하시기 바랍니다. <br/>
	* 로그인에 성공하면 **requestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인합니다.
	* 반환된 미소비 결제 내역 목록에 값이 존재한다면 게임 클라이언트가 게임 서버에 consume(소비)를 요청하여 아이템을 지급합니다.

### Purchase Item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출해 구매를 요청합니다. <br/>
게임 유저가 구매를 취소하는 경우 **GamebaseError.PURCHASE_USER_CANCELED** 오류가 반환됩니다. 취소 처리를 해 주시기 바랍니다.

**API**

```java
+ (void)Gamebase.Purchase.requestPurchase(Activity activity, long itemSeq, GamebaseDataCallback<PurchasableReceipt> callback);
```

**Example**

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

**API**

```java
+ (void)Gamebase.Purchase.requestItemListPurchasable(Activity activity, GamebaseDataCallback<List<PurchasableItem>> callback);
```

**Example**

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

* 아직 소비되지 않은 일회성 상품(CONSUMABLE)과 소비성 구독 상품(CONSUMABLE_AUTO_RENEWABLE) 정보를 조회합니다.<br/>
* 미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다.

* 다음 두 가지 상황에서 호출해 주세요.
    1. 결제 성공 후 아이템 소비(consume) 처리 전 최종 확인을 위하여 호출
    2. 로그인 성공 후 소비(consume)하지 못한 아이템이 남아 있지는 않은지 확인하기 위하여 호출

**API**

```java
+ (void)Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

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

### Get a List of Activated Subscriptions

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다. 
사용자 ID가 같다면 Android와 iOS에서 구매한 구독 상품이 모두 조회됩니다.

> <font color="red">[주의]</font><br/>
>
> 현재 구독 상품은 Android의 경우 Google Play 스토어만 지원합니다.
>

**API**

```java
+ (void)Gamebase.Purchase.requestActivatedPurchases(Activity activity, GamebaseDataCallback<List<PurchasableReceipt>> callback);
```

**Example**

```java
Gamebase.Purchase.requestActivatedPurchases(activity, new GamebaseDataCallback<List<PurchasableReceipt>>() {
    @Override
    public void onCallback(List<PurchasableReceipt> data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Request subscription list failed- "
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
| PURCHASE_USER_CANCELED                   | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003       | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다.     |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 해당 스토어의 캐시가 부족하여 결제할 수 없습니다.             |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), ONESTORE 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP 라이브러리 오류입니다.<br>DetailCode를 확인하세요.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 IAP 모듈에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

```java
Gamebase.Purchase.requestPurchase(activity, itemSeq, new GamebaseDataCallback<PurchasableReceipt>() {
    @Override
    public void onCallback(PurchasableReceipt data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Purchase successful");
            ...
        } else {
            Log.e(TAG, "Purchase failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PURCHASE_EXTERNAL_LIBRARY_ERROR) {
                // IAP Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [TOAST > TOAST SDK 사용 가이드 > TOAST IAP > Android > 오류 코드](/TOAST/ko/toast-sdk/iap-android/#_24)

