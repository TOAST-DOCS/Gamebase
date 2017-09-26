## Game > Gamebase > Getting Started

## Getting Started
Gamebase 서비스 사용을 위한 아주 간단하지만 꼭 필요한 기본적인 절차에 대해 설명합니다.

1. 서비스 활성화 [Console]
2. 프로젝트 ID 및 Secret Key 확인[Console]
3. 게임 및 클라이언트 정보 등록[Console]
4. Gamebase SDK 다운로드

### Enable Gamebase [Console]

[TOAST Cloud Console]에서 **[Game] > [Gamebase]** 상품을 선택한 후 **[상품 이용]** 버튼을 클릭하여 서비스를 활성화 합니다.

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[그림1] Gamebase 상품 활성화</center>

### Check {Project ID} and {Secret Key} [Console]

#### AppId
appId는 TOAST Cloud의 프로젝트ID로 Console의 Project list 화면에서 확인 가능합니다.<br>
해당 값은 Server API 호출시나 SDK 설정시에 꼭 필요한 값입니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.1.png)
<center>[그림2] Gamebase AppId</center>

#### SecretKey
secretKey는 API에 대한 접근 제어 방안으로 Gamebase Console에서 확인 가능합니다. <br>
해당 값은 Server API 호출시 HTTP Header에 필수적으로 설정되어야 합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.1.png)
<center>[그림3] Gamebase SecretKey</center>

### Register App and Client [Console]

Gamebase Console의 [앱] 메뉴를 이용하여 게임 및 클라이언트의 기본 정보를 등록합니다.<br>
각 항목의 자세한 설명은 아래 링크를 참고하시기 바랍니다.

* [LINK [Operator Guide > App]](./app/#app) : App 설정정보
* [LINK [Operator Guide > Client]](./app/#client) : Client 설정정보


![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)
<center>[그림4] 게임 정보 등록 화면</center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
<center>[그림5] 클라이언트 정보 등록 화면</center>
> [INFO]<br>
> SDK 설정시에 Client 버전 정보가 필요하므로 해당 화면에서 꼭 확인하세요!<br>


### Download Gamebase SDK

Gamebase SDK는 아래 다운로드 페이지에서 내려받을 수 있습니다.<br/>
[LINK [Download Gamebase SDK]](http://docs.cloud.toast.com/ko/Download/)

SDK 다운로드 후 플랫폼별 자세한 설정 방법은 각 플랫폼별 [Developers Guide]를 참고하시기 바랍니다.<br/>

* [LINK [iOS 개발프로젝트 설정하기] ](./ios-started/)
* [LINK [Android 개발프로젝트 설정하기] ](./aos-started/)
* [LINK [Unity Plug-in 개발프로젝트 설정하기] ](./unity-started)
<br/>
> 드디어 Gamebase 서비스를 사용할 준비가 끝났습니다. :-) <br> 보다 자세한 가이드는 아래를 참고해 주세요.

<br/>
## Platform Guide

### Client Developer's Guide

* [LINK [iOS Developer's Guide] ](./ios-started/)
* [LINK [Android Developer's Guide] ](./aos-started/)
* [LINK [Unity Developer's Guide] ](./unity-started/)

### Server Developer's Guide

* [LINK [Server Developer's Guide] ](./Server%20Developer%60s%20Guide/)

### Operator's Guide

* [LINK [Operator's Guide] ](./operating-indicator/)

<br/>
## Funtional Guide

| Feature | Description | client | server  | console |
|--------|--------|--------|--------|--------|
| Login        | Guest , 3rd Party 인증지원  <br> - 지원되는 IDP : facebook, google+, iosgamecenter, payco      | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)]  | [[토큰검증](./Server%20Developer%60s%20Guide/#token-authentication)] <br> [[회원조회](./Server%20Developer%60s%20Guide/#get-member)] |  [[App] > 인증정보설정](./app/#authentication-information) <br> [[Member] > 회원조회](./member/#member) <br> - 기본정보, 로그인이력, 플레이타임, 결제이력 등 |
| Logout       |  Logout      | [[iOS](./ios-authentication/#logout)] [[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)]| | |
| Withdraw       | 게임 탈퇴 <br> - User의 User Id, 매핑정보 등 모든 정보 삭제     | [[iOS](./ios-authentication/#withdraw)] [[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)]| | |
| Mapping       | 하나의 User ID에 여러개의 IDP를 연동하는 기능      | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)]| | |
| Purchase(IAP)       |  (TOAST Cloud 상품연동) <br> 인앱결제 <br> - 지원되는 스토어 : google, app store      | [[iOS](./ios-purchase/#purchase)] [[Android](./aos-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)]| [[Wrappping API](./Server%20Developer%60s%20Guide/#purchaseiap)]  | [[Purchase]](./purchase/#app)<br> [- 아이템 등록](./purchase/#item) <br> [- 결제정보 조회](./purchase/#transactions) |
| Push       | (TOAST Cloud 상품연동) <br> Push 메시지 전송 및 결과 확인      | [[iOS](./ios-push/#push)] [[Android](./aos-push/#push)] [[Unity](./unity-push/#push)]| |[[Push]](./push/#push) <br/>- 실시간, 예약 Push 발송 |
| Leaderboard       | (TOAST Cloud 상품연동) <br> 실시간 대용량 랭킹 조회 및 등록    | | [[Wrappping API](./Server%20Developer%60s%20Guide/#leaderboard)] | |
| Webview      |  SDK에서 기본적인 Webview UI를 제공<br/>alert, toast ui 제공      | [[iOS](./ios-ui/#webview)] [[Android](./aos-ui/#webview)] [[Unity](./unity-ui/#webview)]| | |
| [Operator]Maintenance      | (운영) 점검기능       |  | [[점검여부확인](./Server%20Developer%60s%20Guide/#maintenance)] |  [[Maintenance]](./operation/#maintenance)<br>- 점검등록, 점검해제 |
| [Operator]Notice      | (운영) 긴급 공지 기능 <br> - 게임 유저가 앱 실행시 팝업형태로 공지 확인이 가능      | | | [[Notice]](./operation/#notice) <br/>-공지 등록 |

