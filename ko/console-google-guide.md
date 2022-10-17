## Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드

> [공지]
> 본 문서는 Google Play로 출시된 앱의 정보를 [Gamebase IAP](https://docs.toast.com/ko/Game/Gamebase/ko/oper-purchase/) 콘솔에 등록 및 연동시키는 방법을 다룹니다.
> Google Play로 앱을 출시하기 위한 보다 자세한 콘솔 설정 관련 사항들은 Google이 제공하는 Google Play Console 가이드를 참조하시길 바랍니다.

## Google Cloud 프로젝트 연결
- Google Play에 등록된 앱과 관련된 정보를 연동하기 위해 Google Play에 연결할 Google Cloud 프로젝트가 필요합니다.
- Google Play Console의 **설정** > **API 액세스** 페이지로 이동
    - 신규 Google Cloud 프로젝트 생성 또는 기존 프로젝트 연결 중 선택하여 Google Play와 Google Cloud 프로젝트를 연결하세요.
    - 이미 연결된 Google Cloud 프로젝트가 있다면 연결된 Google Cloud 프로젝트의 상태가 표시됩니다.
    - [Google Cloud Console 메인 페이지](https://console.cloud.google.com/home/dashboard)에서 미리 프로젝트를 생성 후 기존 프로젝트 연결도 가능합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_iap/console_google/google_common_step_01.png)

- Google Play와 Google Cloud 간의 프로젝트 연결이 정상적으로 이뤄지면 Google Play Console의 **설정** > **API 액세스** 페이지에 연결된 Google Cloud 프로젝트와 API 목록이 표시됩니다.
- 아래 가이드에 따라 설정 후 화면과 같이 연결된 프로젝트의 상태가 나타나지 않는다면, Google Cloud와의 연결 부분을 우선 확인하세요.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_iap/console_google/google_common_step_03.png)

- 신규 프로젝트 생성 후 연동 인증 설정을 위해서는 OAuth 동의 화면 설정 등이 필요합니다.
    - **OAuth 동의 화면 설정** 관련 자세한 내용은 화면 내 가이드 및 [Google이 제공하는 OAuth2 가이드](https://developers.google.com/identity/protocols/oauth2/)를 참조 바랍니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_iap/console_google/google_common_step_02.png)


## 두 가지 연동 인증 방식 제공

![NHN Cloud IAP 앱 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_202210.png)

- Google 연동을 위해서는 Google의 안드로이드 개발자 API 접근을 위한 인증이 필요합니다.
- NHN Cloud IAP는 두가지 인증 모델을 제공하며, 각각은 인증을 위해 각기 다른 특화 정보들이 필요합니다.
- 모델별 특화 정보 외 공통적으로 필요한 정보도 Google 연동 및 결제 확인을 위해 필요하므로, **공통 입력 정보도 함께 확인**해야 합니다.
    - `Store App ID`: 공통 입력 정보 가이드 Package Name 참조
    - `Google In App Purchase License Key`: 공통 입력 정보 가이드 InAppPurchase License Public Key 참조
    - `마켓 연동 검증 생략`: 공통 입력 정보 가이드 마켓 검증 생략 참조
- 모델 특화/공통 필요 정보들은 Google이 제공하는 아래 가이드 단계들을 따라가며 [Google Cloud Platform Console](https://console.cloud.google.com/apis/dashboard), [Google Play Console](https://play.google.com/console/developers), [Google Developer Console](https://developers.google.com/oauthplayground/) 등을 통해 얻을 수 있습니다.
- Google Play 앱의 결제 정보를 확인하기 위한 [Google Android Publisher API](https://developers.google.com/android-publisher)는 OAuth2 필수 인증 대상 API입니다.


## 최고 관리자(Supervisor) 인증 모델
- 등록하려는 앱을 Google 콘솔에서 관리하는 Google 계정의 인증을 대행하는 모델이며, NHN Cloud IAP가 기본값으로 제공해온 인증 모델입니다.
- Google Console에서는 하나의 Google 계정에 여러 앱들을 등록 관리할 수 있으며 동일 계정 하위에 등록된 앱들의 NHN Cloud IAP 설정 정보는 모두 동일합니다.
- 다음 안내에 따라 총 3가지의 정보를 확인하여 NHN Cloud IAP의 앱 정보로 입력해야 하며, 이 정보들은 해당 Google 계정의 OAuth 인증에 사용됩니다.

1. NHN Cloud IAP Google 최고 관리자 인증 모델 특화 입력 정보
    - `Google API Client ID`: 최고 관리자 인증모델 가이드 4단계 참조
    - `Google API Client Secret`: 최고 관리자 인증모델 가이드 4단계 참조
    - `Refresh Token For Google Oauth`: 최고 관리자 인증모델 가이드 6단계 참조
      ![최고 관리자 모델 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_supervisor_202210.png)


2. [Google Cloud Console API 및 서비스](https://console.cloud.google.com/apis/dashboard) 대시보드로 이동
    - Google Play 관리 계정과 연동할 프로젝트 선택
    - **사용자 인증 정보** 메뉴로 이동
    - **사용자 인증 정보 만들기** > **OAuth 클라이언트 ID** 선택

   ![최고 관리자 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_01.png)

3. 신규 OAuth Client 기본 정보 입력
    - 애플리케이션 유형: 웹 애플리케이션 지정
    - 이름: 관리자가 NHN Cloud IAP 용도임을 구분할 수 있는 적절한 명칭 지정
    - 승인된 자바스크립트 원본: 무시
    - 승인된 리다이렉션 URI: 추가 후 `https://developers.google.com/oauthplayground` 입력

   ![최고 관리자 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_02.png)

4. 신규 OAuth Client가 생성되면 생성된 OAuth Client의 두 가지 정보에 대한 안내 팝업이 노출됩니다.
    - 클라이언트 ID: NHN Cloud IAP 앱 정보 설정 화면의 `Google API Client ID`의 값으로 입력합니다.
    - 클라이언트 보안 비밀번호: NHN Cloud IAP 앱 정보 설정 화면의 `Google API Client Secret`의 값으로 입력합니다.
    - 위 두 정보는 OAuth Client 정보 화면에서 재확인할 수 있으며, Google이 제공하는 OAuth Client 정보 JSON 다운로드 파일에서도 확인할 수 있습니다.

   ![최고 관리자 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_03.png)

5. [Google OAuth 2 Playground](https://developers.google.com/oauthplayground) 페이지로 이동
    - 화면 우측 상단의 톱니바퀴 버튼을 누르면 나타나는 **OAuth 2.0 Configuration** 팝업 설정에서 Use your own OAuth credentials 체크박스를 체크합니다.
    - OAuth 2.0 Configuration 팝업 설정의 Client ID, Client Secret에 각각 4번 단계에서 얻은 정보를 입력합니다.
    - 좌측의 API 권한 범위 선택 화면에서 `Google Play Android Developer API`를 선택합니다.
        - 좌측 선택화면 하단의 범위 입력 부분에 `https://www.googleapis.com/auth/androidpublisher`를 입력해도 무방합니다.
    - 모든 설정값 입력 및 선택이 완료되면 좌측 하단의 Authorize API 버튼을 클릭합니다.

   ![최고 관리자 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_04.png)

6. Authorize API 버튼을 클릭하면 Google OAuth 2 Playground 페이지가 Step 2로 변경되며 추가 정보가 표시됩니다.
    - Refresh Token: NHN Cloud IAP 앱 정보 설정 화면의 `Refresh Token For Google Oauth` 값으로 입력합니다.

   ![최고 관리자 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_05.png)

## 서비스 계정 인증 모델
- 최고 관리자 계정이 위임한 권한을 가지는 Google 내 서비스 계정의 인증을 대행하는 모델이며, 2022년 4월 NHN Cloud IAP에 신규 추가된 인증 지원 모델입니다.
- 권한 범위를 위임받는 서비스 계정의 생성 및 관리는 최고 관리자(Google의 실제 관리 계정)가 Google 콘솔에서 수행해야 합니다.
- 서비스 계정은 최고 관리자에 의해 최고 관리자에 준하는 범위의 권한을 위임받을 수도 있고, 최고 관리자가 권한을 가진 특정 앱만으로 한정된 범위의 권한을 위임받을 수도 있습니다.
    - 위임 권한 범위에 대한 설정 전략은 고객이 의도한 특성에 맞게 설정하시면 됩니다.
- 최고 관리자 인증 모델이 가지는 권한의 범위에 대한 부담을 느끼신다면, Google 콘솔 내에서 서비스 계정 생성 후 이 인증 모델을 사용하시면 됩니다. ([Google 가이드](https://developers.google.com/identity/protocols/oauth2/service-account))

1. NHN Cloud IAP 서비스 계정 인증 모델 특화 입력 정보
    - `서비스 계정 연동 정보`: 서비스 계정 인증 모델 가이드 5단계 참조   
      ![서비스 계정 모델 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_service_account_202210.png)

2. [Google Cloud Console](https://console.cloud.google.com/apis/dashboard) API 및 서비스 페이지로 이동
    - Google Play 관리 계정과 연동할 프로젝트 선택
    - **사용자 인증 정보** 메뉴로 이동
    - **사용자 인증 정보 만들기** > **서비스 계정** 선택

   ![서비스 계정 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_02.png)

3. 신규 서비스 계정 기본 정보 입력
    - 관리하기 용이한 형태의 기본 정보들을 입력 후 `만들고 계속 하기`를 클릭합니다.
    - 서비스 계정에 대한 액세스 권한 부여에서 역할을 `소유자`로 선택합니다.
    - 추가 선택 사항에서 생성하려는 서비스 계정의 관리자 이메일을 입력하면, 해당 관리자에게 서비스 계정 설정에 대한 권한이 부여됩니다.
        - Google Cloud Console 내 존재하지 않는 관리자 이메일 주소일 경우엔 입력한 메일 주소로 관리자 초대 메일이 발송됩니다.

   ![서비스 계정 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_03.png)

4. 서비스 계정으로의 권한 위임
    - **Google Play Console** > **API 액세스** 메뉴로 이동
    - Google Cloud Console에서 생성한 서비스 계정이 화면 하단의 서비스 계정 목록에 노출됩니다. 생성한 서비스 계정 우측의 **권한 부여** 링크를 클릭합니다.
    - 권한의 범위는 최고 관리자 계정이 가진 앱 전체 혹은 앱 개별 단위로 복수 지정이 가능하며, 범위 내에서 권한의 종류를 지정할 수 있습니다.
    - 범위는 고객의 의도에 맞게 설정하되, 권한의 종류 중 `앱 정보 보기 (읽기 전용)` , `재무 데이터 보기`, `주문 및 구독 관리`의 권한은 필수로 위임 설정해야 합니다.
    - 위임 설정된 권한은 반영이 되기까지 시일이 소요될 수 있습니다. 위임 권한이 반영될 때까지 길게는 7일 정도의 시일이 소요된 사례가 있으니 참고하시기 바랍니다.

   ![서비스 계정 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_05.png)

5. API 및 서비스 페이지의 새로 생성된 서비스 계정 목록 중 생성한 **서비스 계정**의 세부 정보 화면으로 이동
    - 키 탭으로 이동 후 키 추가 > 새 키 만들기
    - 신규 팝업에서 `JSON` 형식 선택 후 만들기 클릭
    - 키 만들기가 완료되면 자동으로 JSON 형식의 파일이 Google로부터 다운로드됩니다.
    - 해당 파일을 윈도우 메모장과 같은 순수 텍스트 편집기로 불러내어 전체 내용을 복사 후 NHN Cloud IAP 앱 정보 설정 화면의 `서비스 계정 연동 정보`에 입력해야 합니다.
    - 다운로드한 파일은 최초 다운로드 후 다시 다운로드할 수 없으므로, 잘 보관하여 입력하시기 바랍니다.
    - 4번 단계에서 언급되어 있듯이, Google 내부적으로 미처 권한 반영이 되지 않은 경우 권한 없음 등록 오류가 발생할 수 있으니, 이럴 경우 시간을 두고 재시도를 해봅니다.

   ![서비스 계정 모델 설정](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_04.png)


## 공통 입력 정보
### Package Name
- Google Play Console을 통해 등록한 앱을 빌드 시 지정한 packageName이 있습니다. 이 값은 Google 내에서 앱의 고유 지정자로 활용됩니다.
- 이 값을 NHN Cloud IAP 앱 설정의 `Store App ID`로 입력합니다.

### InAppPurchase License Public Key
- **Google Play Console** > 해당 앱 대시보드 > **수입 창출** 메뉴 진입
- 화면 하단에 Base64 인코딩된 라이선스 공개 키가 표기됩니다. 이 값을 NHN Cloud IAP 앱 설정의 `Google In App Purchase License Key`로 입력합니다.

![IAP 라이선스 키](https://static.toastoven.net/prod_iap/console_google/google_iap_license_key.png)

### 마켓 검증 생략
- NHN Cloud IAP가 제공하는 Google 결제의 검증은 크게 두 단계로 나뉩니다. 두 단계 중 **2단계의 생략 여부를 설정**합니다.
- 생략은 권장 사양이 아니며, 간혹 발생하는 Google 서버 장애 상황에 대응하는 일시적인 결제 승인 상태를 유지할 수 있습니다. (기본값은 `NO`)
    - 생략을 하더라도 유입되는 결제 검증 요청이 무조건 정상 결제로 처리되는 것은 아닙니다.
    - 구독 결제의 검증은 생략 옵션이 적용되는 범위가 아닙니다.

#### Google 검증 단계
| 단계 | 설명           |
| --------------- | ----------------------------- |
| 1 단계 | 검증이 요청된 결제 정보의 변조 여부 확인         |
| 2 단계 | 검증이 요청된 결제 정보의 Google 서버 측 최신 상태 확인 및 재검증   |


## Google 시스템 내 실시간 구독 정보 이벤트 전파 설정
- Google이 제공하는 구독 구매의 실시간 상태 전파 이벤트를 NHN Cloud IAP 서버가 받아 처리하도록 설정할 수 있습니다.
- Google Cloud Platform에 결제 프로필을 등록하고(신용카드 필요) 사용 상태로 변경해야 합니다.
- 이 전파 이벤트 설정을 Google 내에서 하지 않으면, 구독 결제에 대한 갱신 정보가 사용자의 앱 클라이언트 실행 액션에 기반하여 반영되므로, 구독 결제를 사용한다면 설정하는 것을 권장합니다.
- 과거에는 구독 실시간 전파 이벤트의 푸시 수신을 위해 수신 받을 주소에 대한 도메인 검증을 Google 웹마스터 도구를 통해 진행해야 했으나, 현재는 도메인 검증을 요구하지 않습니다.

1. [Google Cloud Pub/Sub Console](https://console.cloud.google.com/cloudpubsub) 이동
    - Google Play 앱과 연결된 프로젝트를 제대로 선택해야 합니다.
    - **주제(Topic)** 메뉴에서 새로운 주제를 생성합니다.
    - 주제 ID는 관리하기 쉬운 임의의 명칭을 사용하시면 됩니다.

   ![구독 정보 전파 설정](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_01.png)

2. 생성한 주제에 **주 구성원을 추가**합니다.
    - 세부 구성원 정보 `google-play-developer-notifications@system.gserviceaccount.com` 입력
    - 역할 `게시/구독` > `게시/구독 게시자` 선택

   ![구독 정보 전파 설정](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_02.png)
   ![구독 정보 전파 설정](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_03.png)

3. **구독** 메뉴로 이동해 IAP 서버에게 알려줄 구독을 생성합니다.
    - 구독 ID는 관리용으로 편한 값을 입력
    - Cloud Pub/Sub 주제 선택: 앞서 생성한 주제를 선택
    - 전송 유형: 푸시 선택
    - 엔드포인트 URL: `https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG` 입력
    - `{YOUR_PACKAGE}` 부분엔 NHN Cloud App 설정의 StoreAppID로 입력한 값과 동일한 앱 빌드 시 사용한 패키지명을 입력합니다.

   ![구독 정보 전파 설정](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_04.png)

4. 테스트를 위한 알파 환경/Gamebase 샌드박스 환경을 사용할 경우 3번 단계의 구독을 알파/샌드박스 용으로 각각 생성해야 합니다.
    - 알파 엔드포인트 URL 입력: `https://alpha-api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG`
    - Gamebase 샌드박스 엔드포인트 URL 입력: `https://sandbox-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG`
5. [Google Play Console](https://play.google.com/console)의 앱 대시보드로 이동
    - **수익창출 설정** > **Google Play 결제** 화면의 실시간 개발자 알림 주제 이름에 1번 단계에서 생성한 주제의 전체 명칭을 입력

   ![구독 정보 전파 설정](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_05.png)
