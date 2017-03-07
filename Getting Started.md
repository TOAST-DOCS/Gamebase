## Upcoming Products > Gamebase > Getting Started

## 사용 준비
Gamebase 서비스 사용을 위한 아주 간단하지만 꼭 필요한 기본적인 절차에 대해 설명합니다.

1. 서비스 활성화 [Console]]
2. 게임 및 클라이언트 정보 등록[Console]]
3. Gamebase SDK 다운로드

### 서비스 활성화 [Console]

[TOAST Cloud Console](http://console.cloud.toast.com)에서 **[Game] > [Gamebase]** 상품을 선택한 후 **[상품 이용]** 버튼을 클릭하여 서비스를 활성화 합니다.

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[그림1] Gamebase 상품 활성화</center>

### 게임 및 클라이언트 정보 등록 [Console]

Gamebase의 Console의 [앱] 메뉴를 이용하여 게임 및 클라이언트의 기본 정보를 등록합니다.
각 항목의 자세한 설명은 [Operator's Guide > App](./Operator`s Guide/) 과 [Operator's Guide > Client] 부분을 참고하시기 바랍니다.


![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_app_1.0.png)
<center>[그림2] 게임 정보 등록 화면</center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_client_1.0.png)
<center>[그림3] 클라이언트 정보 등록 화면</center>



### Gamebase SDK 다운로드

Gamebase SDK는 [SDK 다운로드 페이지](http://docs.cloud.toast.com/ko/Download/)에서 내려받을 수 있습니다.
SDK 다운로드 후 플랫폼별 자세한 설정 방법은 각 플랫폼별 [Developers Guide]를 참고하시기 바랍니다.

[iOS 개발프로젝트 설정하기](./Developer`s Guide/iOS Developer`s Guide)
[Android 개발프로젝트 설정하기](./Developer`s Guide/Android Developer`s Guide)
[Unity Plug-in 개발프로젝트 설정하기](./Developer`s Guide/Unity Developer`s Guide)

> 드디어 Gamebase 서비스를 사용할 준비가 끝났습니다. :-)
> 보다 자세한 가이드는 아래를 참고해 주세요.


## 플랫폼별 가이드 링크
### Client Developer's Guide
* [iOS Developer's Guide](./iOS Developer`s Guide)
* [Android Developer's Guide](./Android Developer's Guide)
* [Unity Developer's Guide](./Unity Developer's Guide)

### Server Developer's Guide
* [Server Developer's Guide](./Server Developer's Guide)

### Operator's Guide
* [Operator's Guide](./Operator's Guide)


## 기능별 가이드 링크

| Feature | Description | client | server  | console |
|--------|--------|--------|--------|--------|
| Login        | Guest , 3rd Party 인증지원  <br> - 지원되는 IDP : facebook, google+, iosgamecenter, payco      | [[iOS]()] [[Android]()] [[Unity]()] | | [[App]메뉴의 인증정보설정]() |
| Logout       |  Logout      | [[iOS]()] [[Android]()] [[Unity]()]| | |
| Withdraw       | 게임 탈퇴 <br> - User의 매핑정보 등 모든 정보 삭제     | [[iOS]()] [[Android]()] [[Unity]()]| | |
| Mapping       | User ID하나의 여러개의 IDP를 연동하는 기능      | [[iOS]()] [[Android]()] [[Unity]()]| | |
| Purchase(IAP)       |  (TOAST Cloud 상품연동) <br> 인앱결제 <br> - 지원되는 스토어 : google, app store      | [[iOS]()] [[Android]()] [[Unity]()]| | |
| Push       | (TOAST Cloud 상품연동) <br> Push 메시지 전송 및 결과 확인      | [[iOS]()] [[Android]()] [[Unity]()]| | |
| Webview      |        | [[iOS]()] [[Android]()] [[Unity]()] | | |
| [Operator]Maintenance      | (운영) 점검기능       | [[iOS]()] [[Android]()] [[Unity]()] | |  |
| [Operator]Notice      | (운영) 긴급 공지 기능 <br> - 게임 유저가 앱 실행시 사용자는 팝업형태로 공지 확인이 가능      | [[iOS]()] [[Android]()] [[Unity]()] | |  |
