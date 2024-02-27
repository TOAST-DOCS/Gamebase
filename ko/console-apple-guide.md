## Game > Gamebase > 스토어 콘솔 가이드 > Apple 콘솔 가이드

> 본 문서는 App Store로 출시된 앱의 정보를 [Gamebase](https://docs.nhncloud.com/ko/Game/Gamebase/ko/Overview/) 콘솔에 등록 및 연동시키는 방법을 다룹니다.
> 연동 방법은 **(신)영수증 검증+Notification V2**, **(구)영수증 검증+Notification V1** 두 가지 방식으로 나뉩니다.

## NHN Cloud SDK iOS 버전별 연동 방식
| 버전        | 연동 방식                                                 |
|-----------|----------------------------------------------------------|
| v1.8.0 이상 | (신)영수증 검증+Notification V2, (구)영수증 검증+Notification V1 |
| v1.7.*    | (신)영수증 검증+Notification V2                              |
| v1.6.2 이하 | (구)영수증 검증+Notification V1                              |

## (신)영수증 검증+Notification V2
### 앱 내 구입 키 생성
> **참고** 
> [https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-keys-for-in-app-purchases](https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-keys-for-in-app-purchases)

1. **App Store Connect** > **사용자 및 액세스** > **통합** 탭 클릭
2. **키** > **앱 내 구입** 클릭
3. **앱 내 구입 키 생성** 클릭
4. 키 이름 입력 후 **생성** 클릭
5. **앱 내 구입 키 다운로드** 클릭
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/ko/app_store_connect_01_ko_240226.png)

### Gamebase 정보에 앱 내 구입 키 입력
1. [콘솔](https://console.nhncloud.com)에서 조직 및 프로젝트를 선택하고 **Game** > **Gamebase** > **구매(IAP)** > **스토어** > **등록** 또는 App을 선택하고 **수정**을 클릭
2. Store APP ID: **App Bundle ID** 입력
3. 영수증 검증 및 노티 방식: **(신)영수증 검증+Notification V2** 선택
4. 다운로드한 앱 내 구입 키, Key ID, Issuer ID 입력
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/ko/store_info_01_ko_240226.png)

### Notification V2 URL 등록
1. **App Store Connect** > **앱** > **앱 선택** > **일반 정보** > **앱 정보** > **App Store 서버 알림**
2. **프로덕션 서버 URL** 또는 **Sandbox 서버 URL** 편집 클릭
3. 알림 버전: **버전 2 알림** 선택
4. 서버 URL: `https://api-iap.cloud.toast.com/callback/subscription/{APP_BUNDLE_ID}/AS/v2` 입력

## (구)영수증 검증+Notification V1 (Deprecated 예정)
- Apple 구독 상품 결제를 사용하려면 App Store Connect에서 **공유 암호** 생성 및 **Notification V1 URL** 설정이 필요합니다.
- 공유 암호는 스토어 정보에 등록합니다.
- Apple 일반 상품 결제는 별도 설정이 필요하지 않습니다.

### 공유 암호 생성
> **참고**
> 모든 앱에 대한 단일 암호인 **기본 공유 암호** 또는 개별 앱에 대한 **앱 공유 암호**를 생성할 수 있습니다.
> [https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-a-shared-secret-to-verify-receipts](https://developer.apple.com/help/app-store-connect/configure-in-app-purchase-settings/generate-a-shared-secret-to-verify-receipts)

#### 기본 공유 암호
1. **App Store Connect** > **사용자 및 액세스** > **통합** 탭 클릭
2. **생성** 클릭
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/ko/app_store_connect_02_ko_240226.png)

#### 앱 공유 암호
1. **App Store Connect** > **앱** > **앱 선택** > **배포** > **일반 정보** > **앱 정보** > **앱 공유 암호** > **관리** 클릭
2. **생성** 클릭
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/ko/app_store_connect_03_ko_240226.png)

### 스토어 정보 등록에 Shared Secret 입력
1. [콘솔](https://console.nhncloud.com)에서 조직 및 프로젝트를 선택하고 
**Game** > **Gamebase** > **구매(IAP)** > **스토어** > **등록** 또는 App을 선택하고 **수정**을 클릭
2. Store APP ID: **App Bundle ID** 입력
3. 영수증 검증 및 노티 방식: **(구)영수증 검증+Notification V1** 선택
4. Shared Secret 입력
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/AppStore/ko/store_info_02_ko_240226.png)

### Notification V1 URL 등록
1. **App Store Connect** > **앱** > **앱 선택** > **일반 정보** > **앱 정보** > **App Store 서버 알림**
2. **프로덕션 서버 URL** 또는 **Sandbox 서버 URL** 편집 클릭
3. 알림 버전: **버전 1 알림** 선택
4. 서버 URL: `https://api-iap.cloud.toast.com/callback/subscription/{APP_BUNDLE_ID}/AS` 입력

## (구)영수증 검증+Notification V1 → (신)영수증 검증+Notification V2 변경 시 주의 사항
- App 운영 중 변경할 경우 장애가 발생할 수 있으므로 반드시 점검 중에 변경을 진행하십시오.
- **(신)영수증 검증+Notification V2** 가이드를 참고하여 App 점검 중에 진행하십시오.
    - Product App 변경 전 Sandbox App에서 충분히 테스트한 뒤 작업하십시오.
- 점검을 마친 뒤 사용자가 App의 최신 버전을 사용하도록 강제 업데이트가 필요합니다.
    - 최신 버전이 아닐 경우 사용자가 App을 이용할 때 오류가 발생할 수 있습니다.
