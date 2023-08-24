## Game > Gamebase > 스토어 콘솔 가이드 > Apple 콘솔 가이드

Apple 구독 상품 결제를 사용하려면 App Store Connect에서 secret key 생성 및 Notification url 설정이 필요합니다.
Secret Key는 IAP 앱 정보에 등록합니다.
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
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-1.png)

<br>

### 앱 별 shared secret key
```
1. App Store Connect
2. [My Apps] 클릭 > 생성 하려는 [앱] 클릭 > toolbar에서 [Features] 클릭
3. [App-Specific Shared Secret] 클릭
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-shared-key-2.png)


### IAP 앱 정보에 shared secret key 입력하기
```
1. Gamebase > 구매(IAP) > 스토어 메뉴에서 등록된 App store 정보 선택
2. Apple Shared Secret 입력
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-apple-edit-gamebase.png)


## Notification URL 등록하기
**Notification V2** 는 아직 지원하지 않습니다. **V1** 을 사용해 주시기 바랍니다.
```
1. App Store Connect > 나의 앱 > 앱 정보 > 일반 정보 
2. 프로덕션 서버 URL에 IAP URL을 등록합니다.
- URL : https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/AS
- {YOUR_PACKAGE_NAME} : app bundle id
```
- Notification URL 정보를 등록해야, 구독, 프로모션 교환 및 환불 정보가 Gamebase에 갱신됩니다.

