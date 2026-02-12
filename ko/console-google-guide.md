## Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드

> [공지]
> 본 문서는 Google Play로 출시된 앱의 정보를 [Gamebase IAP](https://docs.toast.com/ko/Game/Gamebase/ko/oper-purchase/) 콘솔에 등록 및 연동시키는 방법을 다룹니다.
> Google Play로 앱을 출시하기 위한 보다 자세한 콘솔 설정 관련 사항들은 Google이 제공하는 Google Play Console 가이드를 참조하시길 바랍니다.

## Google 사이트
- 연동에 필요한 정보를 얻기 위해 아래 google 사이트를 이용합니다.
    - [Google Play Console](https://play.google.com/console/developers)  
    - [Google Cloud Console](https://console.cloud.google.com/)
    - [Google Developers - OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)

## 기본 정보 입력
![최고 관리자 모델 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_01.png)

### 1. Store App ID
- Google Play 등록을 위해 빌드한 앱의 Package Name으로 Google Play 내에서 앱을 식별할 수 있는 고유값입니다.
- 앱을 등록했다면 Google Play Console의 앱 목록 또는 대시보드 등에서 확인이 가능합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_02.png)

### 2. Google InApp Purchase License Key
- 라이선스 확인을 위해 Google Play Console에 접속합니다.
- **홈** 화면에서 설정할 앱을 선택 후 **수익 창출 설정**으로 들어갑니다.
- 항목 중 **라이선스**에 있는 Base64로 인코딩된 내용을 복사하여 붙여넣습니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_03.png)

### 3. 마켓 연동 검증 생략
- Google 장애 상황을 대비한 옵션으로 일반적인 경우 기본값인 **NO**로 설정하십시오.
- **YES**로 설정 시 전송된 결제 정보의 변조 여부만 확인하고, Google의 검증을 생략합니다.
- 모든 결제에 유효한 것은 아니며, 구독이나 재검증 등에는 적용되지 않습니다.


## 연동을 위한 두 가지 인증 방식 제공
- Google 연동을 위해서는 Google Cloud API를 사용해야 하며, Google Cloud API는 Google에서 제공하는 OAuth2.0 인증이 필요합니다.
- Gamebase IAP는 Google의 OAuth2.0 인증 중 **클라이언트 ID** 방식과 **서비스 계정** 방식을 지원합니다.
- Gamebase IAP 연동 방식에서 클라이언트 ID는 **SUPERVISOR**로, 서비스 계정은 **SERVICE_ACCOUNT**로 매핑됩니다.
- 두 방식의 차이는 간략히 아래와 같으며, 자세한 내용은 [Google의 OAuth 2.0 가이드](https://developers.google.com/identity/protocols/oauth2?hl=ko)를 참고하십시오.

**[클라이언트 ID 방식]**
- 사용자를 대신하여 인증에 사용할 클라이언트 ID와 인증 정보를 생성합니다.
- 인증 정보를 생성하는 과정에 실제 Google 사용자(개발자)의 승인이 필요합니다. 
- 승인은 웹에서 1회만 이루어지고, 생성된 인증 정보와 승인 결과가 웹사이트로 반환됩니다.
- 승인된 클라이언트 ID로 Google Cloud API를 사용하면 승인한 사용자와 동일한 권한을 갖습니다.
- Google Play Console을 생성(소유)한 사용자가 인증 정보를 생성했다면 해당 클라이언트 ID는 Google Play Console 내 모든 앱에 접근이 가능합니다.

**[서비스 계정 방식]**
- Google Cloud Console에서는 프로젝트를 위한 서비스 계정을 생성할 수 있습니다. 서비스 계정은 Google 이메일을 갖는 일반 사용자 계정이 아닙니다. 
- Google Play Console에서는 Google Cloud Console에서 생성한 서비스 계정을 추가하여 사용합니다.
- 서비스 계정이 앱에 접근하기 위해서는 적절한 권한을 부여해야 합니다.

![NHN Cloud IAP 앱 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_04.png)


## Google Cloud 프로젝트 설정
- Google Play에 등록된 앱과 연동하기 위해 Google Cloud 프로젝트가 필요합니다.
- 이미 만들어진 프로젝트가 있다면 기존 프로젝트 사용도 가능하나 여기서는 Google Cloud 프로젝트 생성부터 가이드합니다.

### 1.  프로젝트 생성
- 프로젝트 생성을 위해 [Google Cloud Console](https://console.cloud.google.com/)에 접속합니다.
- Google Play Console 개발자 계정을 소유한 사용자로 로그인합니다.
- **IAM 및 관리자 > 프로젝트 만들기**를 선택합니다.
- **프로젝트 이름**과 **위치**를 입력 후 프로젝트를 생성합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_05.png)

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_06.png)


### 2. 프로젝트에서 사용할 API 추가
- 생성한 프로젝트를 선택하고, **API 및 서비스 > 라이브러리** 메뉴로 이동합니다.
- **API 라이브러리**에서 사용할 API를 선택합니다. Google Play에 등록한 앱과 연동하기 위해 다음 API가 필요합니다.
    - **Play Android Developer API**
    - **Play Games Services Publishing API**
- 해당 API 선택 후 **제품 세부정보**에서 사용으로 설정합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_07.png)


### 3. Google Cloud Console 메뉴 노출
- 설정 과정 중 Google Cloud Pub/Sub와 같이 보이지 않는 메뉴가 있을 경우 **제품 및 솔루션 > 모든 제품**에 들어가면 메뉴(고정된 제품)에 추가할 수 있습니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_08.png)


## SUPERVISOR 연동 방식 설정

Gamebase IAP에서 Google Cloud 클라이언트 ID 인증을 사용하기 위해 클라이언트 ID로 생성한 Refresh token이 필요합니다. Refresh token 생성 중에는 사용자의 승인 과정이 있으며, 이를 위해 Google Cloud 프로젝트에서 **OAuth 동의 화면**을 구성해야 합니다. Google Play Console에 등록한 앱의 접근 권한은 Refresh token 생성을 승인한 사용자의 권한을 따릅니다.

### 1. OAuth 동의 화면 구성
- 클라이언트 ID 생성 전 **OAuth 동의 화면**을 구성한 적이 없다면 먼저 **OAuth 동의 화면**을 구성해야 합니다.
- **API 및 서비스 > OAuth 동의 화면**에서 사용자가 인증 정보 생성을 승인할 때 보게 될 화면을 구성합니다.
- Google Workspace를 사용하지 않았다면, **User Type**은 **외부**만 선택이 가능합니다.
- 나머지 구성 관련 설정은 화면 내 **알아보기**를 따라 진행합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_09.png)


### 2. Google Cloud 클라이언트 ID 생성
- **API 및 서비스 > 사용자 인증 정보**에서 상단의 **사용자 인증 정보 만들기 > OAuth 클라이언트 ID**를 선택하여 **OAuth 클라이언트 ID 만들기** 페이지로 들어갑니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_10.png)

- **애플리케이션 유형**은 **웹 애플리케이션**을 선택합니다.
- 클라이언트 ID를 식별할 **이름**을 입력합니다.
- **승인된 리디렉션 URI**는 앞서 설정한 **OAuth 동의 화면**에서 사용자 승인 후 결과를 반환 받을 주소입니다. **애플리케이션 유형**을 **웹 애플리케이션**으로 설정하면 승인된 인증 정보(Authorization code)를 웹으로 받습니다.
- 별도의 사용자 웹 애플리케이션으로 인증 결과를 받고 싶다면 사용자의 웹 주소 설정도 가능합니다. 여기서는 Google Developers 사이트를 이용하여 인증 정보를 확인합니다.
- 승인된 리디렉션 URI에 ```https://developers.google.com/oauthplayground```를 입력합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_11.png)

- **만들기**를 클릭해 OAuth 클라이언트를 생성하면 **클라이언트 ID**와 **클라이언트 보안 비밀번호**를 확인할 수 있습니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_12.png)


### 3. OAuth 클라이언트로 Refresh token 생성
- Refresh token 생성을 위해 [Google Developers - OAuth 2.0 Playground](https://developers.google.com/oauthplayground)에 접속합니다.
- **Step 1**에서 인증에 사용할 API인 **Google Play Android Developer API v3**의 ```https://www.googleapis.com/auth/androidpublisher```를 선택합니다.
- 우측 상단의 톱니바퀴 모양 버튼을 눌러 **OAuth 2.0 configuration**을 열고, **Use your own OAuth credentials**를 체크하여 추가 입력란이 나오게 합니다.
- 앞에서 생성한 **클라이언트 ID**와 **클라이언트 보안 비밀번호**를 **OAuth Client ID**와 **OAuth Client secret**에 입력합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_13.png)

- **Authorize APIs** 버튼을 누르고, 다음 로그인 페이지에서 Google Play Console 개발자 계정을 가진 사용자로 로그인합니다.
- 로그인하면 **OAuth 동의 화면** 메뉴에서 구성한 페이지로 이동합니다.
- **계속**을 누르면 **Google Developers - Oauth 2.0 Playground**의 **Step 2**로 리디렉션합니다.

- **OAuth 동의 화면**에서 **게시 상태**가 테스트 상태인 경우 **Google에서 확인하지 않은 앱** 화면을 볼 수도 있습니다.


- 클라이언트 ID를 생성할 때 **승인된 리디렉션 URI**에 ```https://developers.google.com/oauthplayground ```를 입력하지 않았다면 결과를 수신할 수 없습니다.
- 정상적으로 리디렉션되면 **Step 2**에서 **Authorization code**를 확인할 수 있습니다.
- 여기서 **Exchange authorization code for tokens**를 눌러 **Refresh token**과 **Access token**을 발급 받습니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_14.png)

- **Step 3**는 진행하지 않아도 무방합니다.


### 4. Gamebase IAP 앱에서 클라이언트 정보 설정
- **IAP > 스토어**의 **추가**에서 Google Cloud Console과 Google Developers에서 확인한 정보를 입력합니다.
- **Google API Client ID: 클라이언트 ID**를 입력
- **Google API Client Secret: 클라이언트 보안 비밀번호**를 입력
- **Refresh Token For Google Oauth**: Google Developsers OAuth Playground에서 수신한 **Refresh token**을 입력

![최고 관리자 모델 설정](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_15.png)



> [주의]
> 발급 받은 Refresh token은 인증한 사용자 계정의 비밀번호를 변경하면 즉시 만료됩니다. 만약 앱이 운영 중이라면 이는 장애를 초래할 수 있습니다.
> 그 밖에도 만료되는 경우가 있으니 [Google의 OAuth 2.0 가이드 - 갱신 토큰 만료](https://developers.google.com/identity/protocols/oauth2?hl=ko#expiration)를 반드시 확인해 주시기 바랍니다.



## SERVICE_ACCOUNT 연동 방식 설정

사람이 아닌 사용자가 Google Cloud 리소스에 접근 가능하도록 Google Cloud IAM에서 서비스 계정을 발급할 수 있습니다. 사용자 계정과의 차이점 및 운영 전략은 [Google Cloud IAM](https://cloud.google.com/iam/docs/service-account-overview?hl=ko) 문서 또는 [Google Cloud 인증 문서](https://cloud.google.com/docs/authentication?hl=ko#credentials)를 참고하십시오.


### 1. Google Cloud 서비스 계정 생성
- **IAM 및 관리자 > 서비스 계정**에서 **서비스 계정 만들기**를 누르거나 **API 및 서비스 > 사용자 인증 정보**에서 **사용자 인증 정보 만들기 > 서비스 계정**을 선택합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_16.png)

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_17.png)

- **서비스 계정 이름**과 **서비스 계정 ID**에 알맞은 정보를 입력 후 **만들고 계속하기**를 눌러 다음으로 진행합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_18.png)

- 액세스 권한 부여는 **소유자**를 선택합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_19.png)

- 이후 완료하거나 추가로 서비스 계정에 대한 관리자 이메일을 등록할 수 있습니다. 관리자 이메일을 등록하면 생성 중인 서비스 계정의 관리 권한을 얻습니다. 등록한 이메일이 현재 프로젝트에 참여 중이 아닐 경우 초대 메일이 발송됩니다.


### 2. Google Cloud 서비스 계정의 키 생성

- 생성된 서비스 계정을 클릭하여 세부 정보를 확인합니다.
- **키** 탭으로 이동하여 **키 추가 > 새 키 만들기**를 선택합니다.
- **키 유형**은 **JSON**을 선택하고 **만들기**를 누르면 키 파일을 다운로드합니다.
- 다운로드된 파일의 내용은 Gamebase IAP 앱을 설정할 때 사용합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_20.png)



> [주의]
> 다운로드한 서비스 계정의 키 파일은 다시 다운로드할 수 없습니다. 분실한다면 키를 폐기하고, 신규로 생성해야 합니다.
> 또한 키는 서비스 계정에 부여한 모든 권한을 사용할 수 있으므로 키 보안에 각별히 유의하십시오.


### 3. Google Play Console에 서비스 계정 등록

- Google Play Console에 접속합니다.
- **사용자 및 권한**에서 **신규 사용자 초대** 버튼을 클릭합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_21.png)

- 사용자 초대 화면에서 생성한 서비스 계정 이메일을 입력합니다. **액세스 권한 만료일 설정**은 체크하지 않습니다.
- 권한은 서비스 계정 하위에 애플리케이션을 추가해 앱별로 부여할 수도 있고, 등록하는 서비스 계정에 권한을 부여할 수도 있습니다. 여기서는 **계정 권한**으로 등록합니다.
- 범위는 고객의 의도에 맞게 설정하되, **앱 정보 보기 및 보고서 일괄 다운로드(읽기 전용), 재무 데이터, 주문, 취소 설문조사 응답 보기, 주문 및 구독 관리**는 반드시 선택해야 합니다.
- 권한 설정이 반영되기까지 일정 시일이 소요됩니다. 경우에 따라 7일 정도의 시일이 소요될 수 있습니다.
- 서비스 계정은 초대 후 사용자의 이메일 승인 과정 없이 활성화됩니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_22.png)

> Google의 일반 사용자 계정도 Google Cloud의 프로젝트에 주 구성원으로 등록되어 있고, Google Play Console에서 사용자 초대와 권한을 부여한다면 SUPERVISOR 방식과 같이 클라이언트 ID를 통한 Google Cloud API 접근이 가능합니다.


### 4. NHN Cloud IAP 앱에서 서비스 계정 설정
- **IAP > App**의 **추가** 또는 **편집**에서 **서비스 계정 연동 정보** 항목에 다운로드한 서비스 계정의 키 파일 내용을 입력합니다.
- 복사할 때는 메모장과 같은 텍스트 편집기를 사용해 내용 전체를 복사하십시오.

![연동방식_sevice_account](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_23.png)


## 실시간 구독 상태 수신을 위한 Google 알림 설정
Google Play에서 구독 상품을 판매하는 경우 NHN Cloud IAP에서 Google로부터 알림을 받아 구독의 최신 상태를 관리할 수 있습니다. 구독 상품은 갱신 시점에 Google 내에서 자동으로 갱신됩니다. 이와 같은 Google 내에서 발생하는 구독 이벤트를 추적하기 위해 Google Cloud의 주제(Topic) 를 사용합니다. 주제에 대한 자세한 내용은 [Android Developers - 주제 만들기](https://developer.android.com/google/play/billing/getting-ready#create-topic)에서 확인할 수 있습니다.

### 1. Google Cloud 알림 주제 생성
- [Google Cloud Console](https://console.cloud.google.com/)에 접속합니다.
- **Pub/Sub**에서 **주제 만들기**를 클릭합니다.
- **주제 ID**를 입력하고, **기본 구독 추가**와 **Google 관리 암호화 키**를 선택하여 주제를 만듭니다.
- **Pub/Sub** 메뉴가 보이지 않는다면 **제품 및 솔루션 > 모든 제품**에서 접근할 수 있습니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_24.png)
![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_25.png)

- 주제가 생성되면 구독 이벤트가 발생했을 때 주제에 게시할 게시자를 추가해야 합니다. 생성된 주제를 선택 후 **권한** 탭에서 **주 구성원 추가**를 클릭합니다.
- **새 주 구성원**은 ```google-play-developer-notifications@system.gserviceaccount.com```를, **역할**은 **게시/구독 게시자**를 선택하고 저장합니다.

![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_26.png)


### 2. 주제에 게시할 구독 설정
- 주제를 생성하면 **구독** 메뉴에서 해당 주제의 구독이 함께 생성된 것을 볼 수 있습니다.
- 구독 수정으로 들어가 **전송 유형**은 **푸시**를 선택하고, **엔드포인트 URL**은 Gamebase IAP의 알림 수신 주소인 ```https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG ```을 입력합니다. 입력할 때 ```{YOUR_PACKAGE_NAME}```은 위의 Gamebase IAP 앱 기본 정보 입력 중 **Store App ID**와 동일한 값으로 교체해야 합니다.
- Gamebase 샌드박스를 사용하고 있다면 **엔드포인트 URL**은 ```https://sandbox-api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG ```로 입력합니다.
- 이미 만들어진 주제에 구독을 추가하고 싶다면 **구독 만들기**로 구독을 추가할 수도 있습니다.
![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_27.png)

### 3. Google Play Console에 구독 주제 등록
- **홈** 화면에서 알림을 받을 앱을 선택 후 **수익 창출 설정**으로 들어갑니다.
- **Google Play 결제** 항목 중 **주제 이름**에 앞서 만든 주제의 이름을 입력합니다.
![Google Cloud 프로젝트 연결](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GooglePlay/ko/260202_ko_28.png)