## Upcoming Products > Gamebase > Getting Started

## Getting Started
Gamebase 서비스 사용을 위한 아주 간단하지만 꼭 필요한 기본적인 절차에 대해 설명합니다.

1. 서비스 활성화 [Console]
2. 프로젝트 ID 및 Secret Key 확인[Console]
3. 게임 및 클라이언트 정보 등록[Console]
4. Gamebase SDK 다운로드

### Enable Gamebase [Console]

[TOAST Cloud Console](http://console.cloud.toast.com)에서 **[Game] > [Gamebase]** 상품을 선택한 후 **[상품 이용]** 버튼을 클릭하여 서비스를 활성화 합니다.

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[그림1] Gamebase 상품 활성화</center>

### Check Project ID and {Secret Key} [Console]

#### appId
appId는 TOAST Cloud의 프로젝트ID로 Console의 Project list 화면에서 확인 가능합니다.<br>
해당 값은 Server API 호출시나 SDK 설정시에 꼭 필요한 값입니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.0.png)


#### secretKey
secretKey는 API에 대한 접근 제어 방안으로 Gamebase 콘솔에서 확인 가능합니다. <br>
해당 값은 Server API 호출시 HTTP Header에 설정되어야 합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.0.png)


### Register App and Client [Console]

Gamebase의 Console의 [앱] 메뉴를 이용하여 게임 및 클라이언트의 기본 정보를 등록합니다.<br>
각 항목의 자세한 설명은 [[Operator Guide > App](./app/#app)] 과 [[Operator Guide > Client](./app/#client)] 부분을 참고하시기 바랍니다. <br>
SDK 설정시에 Client 버전 정보가 필요하므로 해당 화면에서 꼭 확인하세요!



![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_app_1.0.png)
<center>[그림2] 게임 정보 등록 화면</center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_client_1.0.png)
<center>[그림3] 클라이언트 정보 등록 화면</center>



### Download Gamebase SDK

Gamebase SDK는 [SDK 다운로드 페이지](http://docs.cloud.toast.com/ko/Download/)에서 내려받을 수 있습니다.<br>SDK 다운로드 후 플랫폼별 자세한 설정 방법은 각 플랫폼별 [Developers Guide]를 참고하시기 바랍니다.

* [LINK [iOS 개발프로젝트 설정하기] ](./ios-started/)
* [LINK [Android 개발프로젝트 설정하기] ](./aos-started/)
* [LINK [Unity Plug-in 개발프로젝트 설정하기] ](./unity-started)

> 드디어 Gamebase 서비스를 사용할 준비가 끝났습니다. :-) <br> 보다 자세한 가이드는 아래를 참고해 주세요.


## 플랫폼별 가이드 링크
### Client Developer's Guide
* [LINK [iOS Developer's Guide] ](./ios-started/)
* [LINK [Android Developer's Guide] ](./aos-started/)
* [LINK [Unity Developer's Guide] ](./unity-started/)

### Server Developer's Guide
* [LINK [Server Developer's Guide] ](./Server%20Developer%60s%20Guide/)

### Operator's Guide
* [LINK [Operator's Guide] ](./operating-indicator/)


## 기능별 가이드 링크

| Feature | Description | client | server  | console |
|--------|--------|--------|--------|--------|
| Login        | Guest , 3rd Party 인증지원  <br> - 지원되는 IDP : facebook, google+, iosgamecenter, payco      | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)]  | [토큰검증](./Server%20Developer%60s%20Guide/#token-authentication) <br> [회원조회](./Server%20Developer%60s%20Guide/#get-member) |  [[App]메뉴의 인증정보설정](./app/#authentication-information) <br> [[Member]메뉴](./member/#member) <br> - 회원조회(기본정보, 로그인이력, 플레이타임 등) |
| Logout       |  Logout      | [[iOS](./ios-authentication/#logout)] [[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)]| | |
| Withdraw       | 게임 탈퇴 <br> - User의 매핑정보 등 모든 정보 삭제     | [[iOS](./ios-authentication/#withdraw)] [[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)]| | |
| Mapping       | User ID하나의 여러개의 IDP를 연동하는 기능      | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)]| | |
| Purchase(IAP)       |  (TOAST Cloud 상품연동) <br> 인앱결제 <br> - 지원되는 스토어 : google, app store      | [[iOS](./ios-authentication/#purchase)] [[Android](./aos-authentication/#purchase)] [[Unity](./unity-authentication/#purchase)]| | [[IAP]메뉴](./purchase/#app)<br> [- 아이템 등록](./purchase/#item) <br> [- 결제정보 조회](./purchase/#transactions) |
| Push       | (TOAST Cloud 상품연동) <br> Push 메시지 전송 및 결과 확인      | [[iOS](./ios-authentication/#push)] [[Android](./aos-authentication/#push)] [[Unity](./unity-authentication/#push)]| |[[Push]메뉴](./push/#push) |
| Webview      | 추후 지원       |  | | |
| [Operator]Maintenance      | (운영) 점검기능       |  | [점검여부확인](./Server%20Developer%60s%20Guide/#maintenance) |  [[Maintenance]메뉴](./operation/#maintenance) |
| [Operator]Notice      | (운영) 긴급 공지 기능 <br> - 게임 유저가 앱 실행시 사용자는 팝업형태로 공지 확인이 가능      | | | [[Notice]메뉴](./operation/#notice) |

