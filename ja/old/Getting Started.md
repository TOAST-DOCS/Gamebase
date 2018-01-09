## Game > Gamebase > Getting Started

## Getting Started
Gamebase 서비스를 사용하기 위해 아주 간단하지만 꼭 필요한 기본적인 절차에 대해 설명합니다.

1. 서비스 활성화 [Console]
2. 프로젝트 ID 및 비밀 키(secret key) 확인 [Console]
3. 게임 및 클라이언트 정보 등록 [Console]
4. Gamebase SDK 다운로드

### Enable Gamebase [Console]

먼저 Gamebase를 활성화해야 합니다. 서비스를 활성화하려면 TOAST Cloud Console에서 **Game > Gamebase** 상품을 선택한 후 **상품 이용** 버튼을 클릭합니다.

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[그림 1] Gamebase 상품 활성화</center>

### Check Project ID and Secret Key [Console]

#### AppId
앱 ID는 TOAST Cloud의 프로젝트 ID로 Console의 **Project list** 화면에서 확인할 수 있습니다.<br>
해당 값은 Server API를 호출하거나 SDK를 설정할 때 꼭 필요한 값입니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.1.png)
<center>[그림 2] Gamebase 앱 ID</center>

#### SecretKey
비밀 키(secret key)는 API에 대한 접근 제어 방안으로, Gamebase Console에서 확인할 수 있습니다. <br>
해당 값은 Server API를 호출할 때 HTTP 헤더에 필수로 설정해야 합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.1.png)
<center>[그림 3] Gamebase SecretKey</center>

### Register App and Client [Console]

Gamebase Console의 **앱** 메뉴를 이용하여 게임 및 클라이언트의 기본 정보를 등록합니다.<br>
각 항목의 자세한 설명은 아래 링크를 참고하시기 바랍니다.

* [Operator Guide > App](./oper-app/#app): App 설정 정보(인증 정보, 게임 서버 주소 등)
* [Operator Guide > Client](./oper-app/#client): Client 설정 정보(버전별 서비스 상태, 스토어 정보 등)


![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)
<center>[그림 4] 게임 정보 등록 화면</center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
<center>[그림 5] 클라이언트 정보 등록 화면</center>
> [참고]<br>
> SDK를 설정할 때 클라이언트 버전 정보가 필요하므로 해당 화면에서 꼭 확인하시기 바랍니다.<br>


### Download Gamebase SDK

Gamebase SDK는 아래 다운로드 페이지에서 다운로드할 수 있습니다.<br/>
[Download Gamebase SDK](http://docs.cloud.toast.com/ko/Download/)

SDK를 다운로드한 후 플랫폼별 자세한 설정 방법은 각 플랫폼별 **Developers Guide**를 참고하시기 바랍니다.<br/>

* [iOS 개발 프로젝트 설정](./ios-started/)
* [Android 개발 프로젝트 설정](./aos-started/)
* [Unity Plug-in 개발 프로젝트 설정](./unity-started)
  <br/>
> 드디어 Gamebase 서비스를 사용할 준비가 끝났습니다. <br> 보다 자세한 가이드는 아래를 참고해 주세요.

<br/>
## Platform Guide

### Client Developer's Guide

* [iOS Developer's Guide](./ios-started/)
* [Android Developer's Guide](./aos-started/)
* [Unity Developer's Guide](./unity-started/)

### Server Developer's Guide

* [Server Developer's Guide](./Server%20Developer%60s%20Guide/)

### Operator's Guide

* [Operator's Guide](./oper-operating-indicator/)

<br/>
## Funtional Guide

| Feature               | Description                              | client                                   | server                                   | Console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | Guest , 3rd Party 인증 지원  <br> - 지원되는 IdP : Facebook, Google, Apple Game Center, PAYCO | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[토큰검증](./Server%20Developer%60s%20Guide/#token-authentication)] <br> [[회원조회](./Server%20Developer%60s%20Guide/#get-member)] | [[App] > 인증정보설정](./oper-app/#authentication-information) <br> [[Member] > 회원조회](./oper-member/#member) <br> - 기본정보, 로그인이력, 플레이타임, 결제이력 등 |
| Logout                | 로그아웃                                     | [[iOS](./ios-authentication/#logout)] [[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | 게임 탈퇴 <br> -  게임 이용자의 사용자 ID, 매핑 정보 등 모든 정보 삭제 | [[iOS](./ios-authentication/#withdraw)] [[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | 하나의 사용자 ID에 여러 개의 IdP를 연동하는 기능           | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Purchase(IAP)         | (TOAST Cloud 상품연동) <br> 인앱 결제 <br> - 지원되는 스토어: Google, App Store | [[iOS](./ios-purchase/#purchase)] [[Android](./aos-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrappping API](./Server%20Developer%60s%20Guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- 아이템 등록](./oper-purchase/#item) <br> [- 결제정보 조회](./oper-purchase/#transactions) |
| Push                  | (TOAST Cloud 상품 연동) <br> 푸시 메시지 전송 및 결과 확인 | [[iOS](./ios-push/#push)] [[Android](./aos-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- 실시간, 예약 Push 발송 |
| Leaderboard           | (TOAST Cloud 상품 연동) <br> 실시간 대용량 랭킹 조회 및 등록 |                                          | [[Wrappping API](./Server%20Developer%60s%20Guide/#leaderboard)] |                                          |
| Webview               | SDK에서 기본적인 WebView UI를 제공<br/>시스템 팝업, 토스트(toast) UI 제공 | [[iOS](./ios-ui/#webview)] [[Android](./aos-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator] Maintenance | (운영) 점검 기능                               |                                          | [[점검여부확인](./Server%20Developer%60s%20Guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- 점검등록, 점검해제 |
| [Operator] Notice      | (운영) 긴급 공지 기능 <br> -  게임 이용자가 앱을 실행할 때 팝업 형태로 공지 확인 가능 |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-공지 등록 |
| [Operator] Ban         | (운영) 게임 이용자의 이용 정지 등록 및 해제 <br> -  게임 이용자의 이용 정지 등록 및 해제 | [[iOS](./ios-authentication/#get-banned-user-information)][[Android](./aos-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -이용정지 게임 이용자 정보 확인 |                                          | [[Ban]](./oper-ban/#ban) <br/>-이용정지 등록 및 해제 |


