## Mobile Service > IAP > Google 콘솔 가이드

Google 일반상품 및 구독상품 인앱 결제를 위해 Google Play Billing을 연동해야 합니다.
Google Play Billing은 Google Play Console 과 Google API Console에서 생성된 값들을 사용합니다.
더불어 구글 구독상품 결제를 위해 Notification을 설정해야 합니다.
Notification 설정이 올바르지 않으면 구독 결제가 진행되지 않습니다.

## Google Application Key

아래 정보를 IAP 앱 정보에 등록합니다.

| 필드 | 설명                                             |
| ---------------------------------- | ---------------------------------------------- |
| Google In App Purchase License Key | Google Play에 등록된 애플리케이션의 Public KEY(RSA)       |
| Google API Client ID               | Google API Project의 OAuth Client ID            |
| Google API Client Secret           | Google API Project의 OAuth Client Secret        |
| Refresh Token For Google OAuth     | Google Play Developer 계정을 통해 획득한 Refresh Token |
<center>[표 1] Google 인앱 결제를 위한 앱 등록값</center>


## Google Console

| Console        | 위치                              |
| -------------- | ------------------------------- |
| Google Play Console | https://developer.android.com/distribute/console |
| Google API Console | https://console.developers.google.com/apis/dashboard |

## Google Play Console

### Google In App Purchase License Key 확인하기
```
Google Play Console > App 선택 > (좌측) 개발 도구 > 서비스 및 API > 라이선스 및 인앱 결제
```
![](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_license_ko.jpg)

## Google API Console

* Android Developer Guide
	* [Android Developers - 인앱 결제 관리](http://developer.android.com/google/play/billing/billing_admin.html)
	* [Android Developers - Authorization](https://developers.google.com/identity/protocols/OAuth2WebServer)

### OAuth 클라이언트 생성

```
Google Play Consle과 동일한 계정으로 Google API Console에 프로젝트를 생성합니다. 
아래의 그림을 참고하여 OAuth 인증에 필요한 정보 3가지를 생성합니다.
1) Client ID  
2) Client Secret  
3) Refresh Token  
```

##### 1. OAuth 클라이언트 생성 (웹 어플리케이션)

* https://console.developers.google.com/apis/credentials
![[그림 1] Client ID 및 Client Secret 생성 1](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_01.png)


##### 2. 승인된 redirection url 입력: `https://developers.google.com/oauthplayground`
![[그림 2] Client ID 및 Client Secret 생성 2](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_ko.png)


##### 3. 생성 후 팝업 창에서 클라이언트 ID / 클라이언트 seceret 복사
![[그림 3] Client ID 및 Client Secret 생성 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_clientSecret_ko.png)

##### 4. 클라이언트 ID, 클라이언트 Secret 입력: [OAuth Playground](https://developers.google.com/oauthplayground/) > 오른쪽 상단 oauthplayground 설정 > Use your own OAuth credentials 사용 체크 후 복사한 클라이언트 ID, 클라이언트 Secret 입력
![[그림 4] Client ID 및 Client Secret 생성 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_03.png)


##### 5. Authorization code 코드 발급: Step 1에서 https://www.googleapis.com/auth/androidpublisher 입력하여 발급
![[그림 5] Client ID 및 Client Secret 생성 4](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_04.png)


##### 6. 토큰 발급: Step 2에서 Exchange authorization code for tokens 버튼을 눌러 발급
![[그림 6] Client ID 및 Client Secret 생성 5](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_05.png)


## Google Play 연동 주의사항

OAuth 인증 정보 생성 후, 아래 가이드를 참고하여 프로젝트 설정을 진행합니다.

> [참고]
> 구글 가이드 : https://developers.google.com/android-publisher/getting_started

#### 1. Google Play Android Developer API의 enable 상태를 확인합니다.

```
  - https://console.developers.google.com > APIs & Services > Dashboard
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-1.png)

#### 2. Google Play Developer Console에서 Linked Project를 확인합니다.

```
  - https://play.google.com/apps/publish > Settings > Developer account > API access
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-2.png)

#### 3. Google Play Developer Console의 Linked Project와 GoogleAPIs의 OAuth 클라언트 생성 프로젝트가 같은지 확인합니다.

![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_new_06.png)


## Google real-time developer notification 설정하기

> [참고]
> Google Cloud (https://cloud.google.com) Platform을 사용해야 합니다. 
> Google Cloud Platform 에 결제 프로필을 등록하고 (신용카드 필요함) 사용 상태로 변경해야 합니다.

### Google Cloud > 빅데이터 > 게시/구독

게시/구독 콘솔(https://console.cloud.google.com/cloudpubsub)에서 아래 작업을 진행합니다.

#### Topic 만들기

```
1. Topic이 생성되면, 옵션의 권한을 클릭하거나, Topic 이름을 클릭하여 Topic 세부정보 페이지로 이동합니다.
2. 주제 세부정보 페이지에서 역할 선택을 클릭하여 게시/구독 게시자를 선택합니다.
3. 구성원 추가에 google-play-developer-notifications@system.gserviceaccount.com 를 입력합니다.
4. 추가 버튼을 클릭합니다.
```
![[] Topic 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-new-topic.png)
![[] Topic 수정하기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_addMember_ko.png)


#### Subscription 만들기
```
1. Topic 마우스 클릭 > 새 구독 
2. 전송유형
- [엔드포인트 URL로 푸시] 선택
- URL :  https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG
- {YOUR_PACKAGE_NAME} : google package name
```
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_new_subscirption_ko.png)
![[] Subscription 만들기](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_create_subscription_ko.png)

#### IAP 도메인 검증
```

1. https://console.cloud.google.com/apis/credentials/domainverification 에 접근합니다.
2. [도메인 확인] 화면에서 [도메인 추가] 를 클릭합니다.
3. https://api-iap.cloud.toast.com를 입력합니다.
4. [지금 이동하기] 버튼을 눌러 웹마스터 센터로 이동합니다.
5. 웹마스터 센터에서 속성추가하기를 클릭합니다.
6. [속성 추가]에 https://api-iap.cloud.toast.com 를 입력합니다.
7. 도메인 인증 URL의 html 파일명을 TOAST IAP CONSOLE App 화면에서 구글 앱 등록 시 Domain authentication File Names 항목에 입력한다.
    -> ex) https://api-iap.cloud.toast.com/googleabc.html 인 경우 googleabc.html 입력
8. [권장 방법] 하단의 [로봇이 아닙니다.] 클릭 후 [확인]을 클릭합니다.
9. 인증에 성공하면 마지막 이미지와 같은 화면이 노출됩니다. 이 화면이 노출되지 않으면 구독결제를 정상적으로 사용 할 수 없습니다.
```
#### 1. https://console.cloud.google.com/apis/credentials/domainverification 에 접근합니다.
#### 2. [도메인 확인] 화면에서 [도메인 추가] 를 클릭합니다.
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-1.png)
#### 3. https://api-iap.cloud.toast.com를 입력합니다.
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-2.png)

![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-3.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/google_domain_auth.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-4.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-5.png)



