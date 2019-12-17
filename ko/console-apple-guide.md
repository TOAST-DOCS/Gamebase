## Mobile Service > IAP > Apple 콘솔 가이드

> [공지]
> 구독 결제를 지원하는 신규 IAP SDK가 [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/)로 출시됐습니다.
> 기존 IAP SDK는 더 이상 신규 기능을 개발하지 않을 예정입니다.
> 본 문서는 [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/) 가이드입니다.


Apple 구독 상품 결제를 사용하려면 App Store Connect에서 secret key 생성 및 Notification url 설정이 필요합니다.<br>
Secret Key는 IAP 앱 정보에 등록합니다.<br>
Apple 일반 상품 결제는 특별한 설정이 필요하지 않습니다.

> 참고
> https://help.apple.com/app-store-connect/#/devf341c0f01

## shared secret key 생성하기
```
shared secret key은 모든 앱에 공통인 마스터키로 생성할 수 있고 앱 별로 생성할 수도 있습니다.
secret key를 IAP 앱 정보에 등록합니다.
```

### master shared secret key
```
1. App Store Connect
2. [My Apps] 클릭
3. [Master Shared Secret] 클릭
```
![[]](http://static.toastoven.net/prod_iap/iap-console-apple-shared-key-1.png)

<br>

### 앱 별 shared secret key
```
1. App Store Connect
2. [My Apps] 클릭 > 생성 하려는 [앱] 클릭 > toolbar에서 [Features] 클릭
3. [App-Specific Shared Secret] 클릭
```
![[]](http://static.toastoven.net/prod_iap/iap-console-apple-shared-key-2.png)


### IAP 앱 정보에 shared secret key 입력하기
```
1. IAP Console > App > ios 앱 선택
2. Apple Shared Secret 입력
```
![[]](http://static.toastoven.net/prod_iap/iap-console-apple-edit.png)



## Notification url 등록하기
```
1. App Store Connect > 나의 앱 > 앱 정보 > 일반 정보 
2. 구독 상태 URL 에 IAP url을 등록합니다.
- URL : https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/AS
- {YOUR_PACKAGE_NAME} : app bundle id
```

