## Upcoming Products > Gamebase > Getting Started

## 사용 준비
Gamebase 서비스 사용을 위한 아주 간단하지만 꼭 필요한 기본적인 절차에 대해 설명합니다.

1. 서비스 활성화 [Console]
2. 프로젝트 ID 및 Secret Key 확인[Console]
3. 게임 및 클라이언트 정보 등록[Console]
4. Gamebase SDK 다운로드

### 서비스 활성화 [Console]

[TOAST Cloud Console](http://console.cloud.toast.com)에서 **[Game] > [Gamebase]** 상품을 선택한 후 **[상품 이용]** 버튼을 클릭하여 서비스를 활성화 합니다.

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[그림1] Gamebase 상품 활성화</center>

### 프로젝트 ID 및 Secret Key 확인[Console]

#### appId
appId는 TOAST Cloud의 프로젝트ID로 Console의 Project list 화면에서 확인 가능합니다.<br>
해당 값은 Server API 호출시나 SDK 설정시에 꼭 필요한 값입니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.0.png)


#### secretKey
secretKey는 API에 대한 접근 제어 방안으로 Gamebase 콘솔에서 확인 가능합니다. <br>
해당 값은 Server API 호출시 HTTP Header에 설정되어야 합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.0.png)


### 게임 및 클라이언트 정보 등록 [Console]

Gamebase의 Console의 [앱] 메뉴를 이용하여 게임 및 클라이언트의 기본 정보를 등록합니다.<br>
각 항목의 자세한 설명은 [[Operators Guide > App](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_3)] 과 [[Operators Guide > Client](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_4)] 부분을 참고하시기 바랍니다. <br>
SDK 설정시에 Client 버전 정보가 필요하므로 해당 화면에서 꼭 확인하세요!



![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_app_1.0.png)
<center>[그림2] 게임 정보 등록 화면</center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_client_1.0.png)
<center>[그림3] 클라이언트 정보 등록 화면</center>



### Gamebase SDK 다운로드

Gamebase SDK는 [SDK 다운로드 페이지](http://docs.cloud.toast.com/ko/Download/)에서 내려받을 수 있습니다.<br>SDK 다운로드 후 플랫폼별 자세한 설정 방법은 각 플랫폼별 [Developers Guide]를 참고하시기 바랍니다.

* [iOS 개발프로젝트 설정하기](./iOS Developer`s Guide/#getting-started)
* [Android 개발프로젝트 설정하기](./Android Developer`s Guide/#getting-started)
* [Unity Plug-in 개발프로젝트 설정하기](./Unity Developer`s Guide/#getting-started)

> 드디어 Gamebase 서비스를 사용할 준비가 끝났습니다. :-) <br> 보다 자세한 가이드는 아래를 참고해 주세요.


## 플랫폼별 가이드 링크
### Client Developer's Guide
* [iOS Developer's Guide](./iOS Developer`s Guide)
* [Android Developer's Guide](./Android Developer`s Guide)
* [Unity Developer's Guide](./Unity Developer`s Guide)

### Server Developer's Guide
* [Server Developer's Guide](./Server Developer`s Guide)

### Operator's Guide
* [Operator's Guide](./Operator`s Guide)


## 기능별 가이드 링크

| Feature | Description | client | server  | console |
|--------|--------|--------|--------|--------|
| Login        | Guest , 3rd Party 인증지원  <br> - 지원되는 IDP : facebook, google+, iosgamecenter, payco      | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#login)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#login)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#login)]  | [토큰검증](/Upcoming%20Products/Gamebase/Server%20Developer%60s%20Guide/#_5) <br> [회원조회](/Upcoming%20Products/Gamebase/Server%20Developer%60s%20Guide/#_7) |  [[App]메뉴의 인증정보설정](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_3) <br> [[Member]메뉴](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_11) <br> - 회원조회(기본정보, 로그인이력, 플레이타임 등) |
| Logout       |  Logout      | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#logout)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#logout)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#logout)]| | |
| Withdraw       | 게임 탈퇴 <br> - User의 매핑정보 등 모든 정보 삭제     | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#withdraw)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#withdraw)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#withdraw)]| | |
| Mapping       | User ID하나의 여러개의 IDP를 연동하는 기능      | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#mapping)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#mapping)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#mapping)]| | |
| Purchase(IAP)       |  (TOAST Cloud 상품연동) <br> 인앱결제 <br> - 지원되는 스토어 : google, app store      | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#purchase)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#purchase)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#purchase)]| | [[IAP]메뉴](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_13)<br> [- 아이템 등록](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_15) <br> [- 결제정보 조회](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_16) |
| Push       | (TOAST Cloud 상품연동) <br> Push 메시지 전송 및 결과 확인      | [[iOS](/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#push)] [[Android](/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#push)] [[Unity](/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/#push)]| |[[Push]메뉴](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_9) |
| Webview      | 추후 지원       |  | | |
| [Operator]Maintenance      | (운영) 점검기능       |  | [점검여부확인](/Upcoming%20Products/Gamebase/Server%20Developer%60s%20Guide/#_10) |  [[Maintenance]메뉴](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_5) |
| [Operator]Notice      | (운영) 긴급 공지 기능 <br> - 게임 유저가 앱 실행시 사용자는 팝업형태로 공지 확인이 가능      | | | [[Notice]메뉴](/Upcoming%20Products/Gamebase/Operator%60s%20Guide/#_7) |
