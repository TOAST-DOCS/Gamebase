## Game > Gamebase > Operator Guide > App
앱의 기본 정보는 **App** 메뉴에서 설정할 수 있습니다. TOAST Cloud Console 메뉴에서 **Game > Gamebase > App**을 클릭합니다. 

* **앱**: 앱 정보 관리
* **클라이언트**: 클라이언트 버전과 상태 정보 관리
* **설치 URL**: 앱의 스토어별 설치 URL 관리


## App

Gamebaes 상품을 활성화하면 자동으로 앱이 생성되며 해당 메뉴에서는 등록된 정보의 수정만 가능합니다.<br/>
TOAST Cloud 프로젝트 하나당 하나의 Gamebase 앱을 관리할 수 있으므로 앱을 추가로 등록하거나 삭제할 수는 없습니다. 앱을 삭제하려면 Gamebase 상품을 비활성화하시면 됩니다.<br />
각 항목별 상세 설명은 아래 **Properties** 항목을 참고합니다. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)

### Properties

Gamebase Console에서 관리하는 앱 정보를 설명합니다.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.1.png)

####(1) 설치 URL
앱 설치와 홍보에 이용할 수 있는 단축 URL 정보입니다. <br/>
앱이 배포된 스토어가 여러 개인 경우에도 하나의 단축 URL로 관리할 수 있습니다. <br/>
자세한 동작 및 관리 방법은 다음 링크를 참고합니다. [설치 URL 관리](./oper-app/#installed-url) <br />

> [참고] <br/>
> Gamebase를 활성화하면 자동으로 생성되므로 변경은 불가능합니다.<br/>

####(2) 서버 주소
게임에서 게임 서버 주소(IP, URL 등)를 실시간으로 전달받아야 할 때 사용합니다.<br/>
서버 주소를 설정하면 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.<br/>
게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.<br />

####(3) 고객 센터 정보
고객 센터 페이지 이외 이메일, 전화번호 등의 정보를 입력합니다.<br />
고객 센터 페이지가 있는 경우에는 아래 **인앱 URL**의 **고객센터**에 정보를 입력합니다.<br />
> [참고] <br/>
> 입력된 정보는 Gamebase가 제공하는 점검 상세 페이지에 표시됩니다.<br />

####(4) 인증 정보
앱에서 로그인할 때 사용할 IdP의 인증 정보를 등록, 수정, 삭제할 수 있습니다.<br />

외부 인증의 클라이언트 아이디, 비밀 키(secret key)뿐만 아니라 콜백 URL과 추가 정보를 설정할 수 있습니다. <br/>
인증 정보 옆에 **+** 버튼을 클릭하면 정보를 수정할 수 있습니다.
Idp별 자세한 설정 방법은 [Authentication Information](#authentication-information)을 참고하시기 바랍니다.<br />
> [참고] <br/>
> **토큰 재검증**이란?<br/>
> 클라이언트에서 Latest Login API 호출 시에 외부 IdP의 토큰 재검증 여부를 설정합니다. <br/>
> **검증 안함**을 선택하면 외부 IdP의 토큰 재검증 없이 내부 토큰 검증만을 수행합니다. <br/>
> **항상 검증**을 선택하면 Gamebase에서 발급한 내부 토큰뿐만 아니라 외부 IdP 토큰에 대해서도 항상 유효성 검증을 진행합니다.


####(5) 인앱 URL
클라이언트를 다시 배포하지 않고 앱 내에서 자주 사용하는 URL을 Console을 통해 실시간으로 수정할 수 있습니다.<br/>

- 이용약관 <br/>
- 개인 정보동의 <br/>
- 이용 정지 규정 <br/> 
- 고객센터 URL <br/>

게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.<br />


####(6) 테스트 단말기
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검 중이어도 정상적으로 게임에 접근할 수 있습니다.<br />
테스트 단말기를 등록하려면 **Device Key**를 입력하거나 **유저 ID**를 입력합니다. <br/>
**User ID** 입력은 편의를 위해 제공하는 것으로, **User ID**를 입력하면 가장 최근에 로그인한 이력의 **Device Key**를 찾아 자동으로 등록합니다. 만약, 등록 이후 테스트 단말기를 변경했다면 다시 단말기를 등록해야 합니다.
<br />

### Authentication Information

#### Facebook
Facebook 개발자 사이트에 등록한 앱의 {앱 아이디}와 {앱 시크릿 코드}를 TOAST Cloud Gamebase Console에 입력합니다.<br />
이때, 로그인 시 필요한 {Facebook Permission} 또한 JSON String 형태로 추가 정보란에 입력해야 합니다.<br />

**입력 필드 ** <br />

- ClientID: {AppID} <br />
- Secret Key: {App Secret Code} <br />
- 추가정보: Facebook Permission (json format) <br />


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

**[예시] facebook_permission format **<br />

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"] }
```

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_permission_1.0.png)

**Reference URL **<br />

- [Facebook 개발자 사이트](https://developers.facebook.com/)<br />
- [Facebook 권한](https://developers.facebook.com/docs/facebook-login/permissions/)<br />




#### Google
Google 개발자 콘솔에 등록된 앱의 {클라이언트 ID} 및 {클라이언트 보안 비밀}, {리다이렉션 URI}를 TOAST Cloud Gamebase Console에 입력합니다.<br />

**입력 필드**<br />

- ClientID: {클라이언트 ID}<br />
- Secret Key: {클라이언트 보안 비밀}<br />
- Callback URL: {리다이렉션 URI}
  <br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

#### Apple Game Center
Apple 개발자 사이트에 등록된 BundleID를 TOAST Cloud Gamebase Console에 입력합니다.<br />

**입력 필드**<br />

- ClientID: {Bundle ID}<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

**Reference URL**<br />

- [Apple Developer 사이트](https://developer.apple.com/)<br />
- [Apple iTunes Connect](https://itunesconnect.apple.com/)<br />

#### PAYCO
PAYCO Client ID를 신청해서 발급받은 {client_id} 및 {client_secret}을 TOAST Cloud Gamebase Console에 입력합니다.<br />

**입력 필드**<br />

- ClientID: {Payco client_id}<br />
- Secret Key: {Payco client_secret}<br />





## Client

클라이언트 정보를 운영체제(iOS, Android, Unity WebGL, Unity Standalone), 버전별로 관리할 수 있습니다.<br />

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
현재 등록된 클라이언트 목록을 확인할 수 있습니다.<br />
운영체제별로 표시지며 아이콘 내 숫자는 클라이언트 등록 시 입력한 버전을 의미합니다.<br />
아이콘 목록은 서비스 상태가 '테스트','심사중','서비스중','서비스중(업데이트권장)'인 목록만 표시됩니다. 운영체제별 하단 오른쪽의 화살표를 클릭하면 '업데이트필수','종료' 상태의 클라이언트 목록을 확인할 수 있습니다.<br />
아이콘 색깔을 서비스 상태별로 구분하여 한눈에 서비스 상태를 파악할 수 있습니다.<br />

### Properties

Gamebase Console에서 관리하는 클라이언트 등록 정보를 설명합니다.<br/>
**클라이언트** 탭에서 **AOS 등록**, **iOS 등록** 버튼 등을 클릭하면 클라이언트 등록 화면이 나타납니다. 등록된 클라이언트의 입력값을 수정하거나 삭제하고 싶다면 아이콘 목록에서 아이콘을 클릭하거나 클라이언트 전체 목록에서 원하는 클라이언트를 선택하시면 됩니다.<br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) 스토어
(<font color="red">필수</font>) 클라이언트를 배포할 스토어를 선택합니다. <br/>
운영체제별로 선택 가능한 스토어가 다릅니다.<br />
#### (2) 게임 버전
(<font color="red">필수</font>) 클라이언트 버전을 입력합니다. <br/>
각 게임별로 정해진 규칙에 따라 문자열로 입력하면 됩니다.<br />
#### (3) 서비스 상태
(<font color="red">필수</font>) 클라이언트의 서비스 상태를 선택합니다.<br />
상태는 <font color="white" style="background-color:#F8BB28">테스트</font>, <font color="white" style="background-color:#FB8F37">심사중</font>, <font color="white" style="background-color:#88C637">서비스</font>, <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스중)</font>, <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>, <font color="white" style="background-color:#CCCCCC">종료</font> 이렇게 6가지입니다.

- <font color="white" style="background-color:#F8BB28">테스트</font>: 내부 테스트<br />
- <font color="white" style="background-color:#FB8F37">심사중</font>: 스토어 심사 중. <br/> 안정화 지표를 추가로 설정할 수 있습니다.<br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
> [참고]<br/>
> '안정화 지표'란?<br/>
> Gamebase 내부에서 Gamebase API 호출 시에 남기는 지표 로그입니다. <br/>
> 서비스 상태가 '심사중'일 때에 안정화 지표를 활성화하면 심사 중에 Gamebase 내부에서 문제가 발생한 경우 보다 쉽게 문제를 확인할 수 있습니다. <br />
> Gamebase Console에서 실시간으로 안정화 지표를 사용할지의 여부와 로그 레벨을 설정할 수 있습니다.<br />

- <font color="white" style="background-color:#88C637">서비스중</font>: 정상 서비스<br />
- <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font>: 정상 서비스. <br/>보다 안정적인 버전을 사용하도록 유도하기 위해 팝업을 표시합니다. 새로운 버전을 다운로드해서 이용하도록 유도하지만 사용자가 원하는 경우 현재 버전으로도 계속 서비스를 이용할 수 있습니다.<br />아래는 '업데이트 권장(서비스 중)' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.<br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>: 서비스 불가능. <br/>현재 게임에서 서비스를 지원하지 않는 버전으로, 최신 버전 설치 안내 팝업을 표시합니다.<br />아래는 '업데이트 필수' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.<br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[주의] </font>  <br/>
>  **업데이트 필수와 점검이 동시에 설정**돼 있을 경우 서비스 상태는 '업데이트 필수'가 됩니다.<br/>
>  점검 진행 도중 사용자에게 업데이트 필수 팝업을 표시하고 싶지 않다면 점검 완료 이후에 서비스 상태를 '업데이트 필수'로 변경해야 합니다.<br/>

- <font color="white" style="background-color:#CCCCCC">종료</font>: 서비스 불가능. <br/> 서비스가 종료된 버전인 경우 선택합니다.<br />아래는 '종료' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.<br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [참고] <br/>
> 서비스 상태별 표시할 메시지 설정<br/>
> **업데이트 권장(서비스중)**, **업데이트 필수**, **종료** 상태인 경우 사용자에게 표시할 안내 메시지를 다국어로 설정할 수 있습니다. <br />
> 서비스 상태를 선택하면 각 상태에 맞는 기본 메시지가 5개(한국어, 영어, 일본어, 중국어 간체, 중국어 번체)의 언어로 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.<br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)

#### (4) 서버 주소
클라이언트에서 이용할 서버 주소(IP, URL)를 입력합니다.<br />
**앱** 탭에서 서버 주소를 입력하면 모든 클라이언트에 적용되므로, 클라이언트마다 다른 서버 주소를 사용하고 싶을 때만 서버 주소를 입력합니다. <br />


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.1.png)

게임을 설치하기 위한 스토어 URL 정보를 관리합니다.<br/>
사용자가 PC나 모바일에서 단축 URL을 클릭하면, 사용자 단말기 정보(디바이스, 운영체제, 스토어 등)를 이용하여 입력된 사이트로 리디렉션합니다.<br/>
스토어 정보가 없거나 스토어 이동에 실패하면 'COMMON'에 설정된 URL로 이동합니다.<br/><br/><br/>

_[예시1] Android 휴대폰에서 문자로 받은 설치 URL을 클릭하는 경우_<br/>
**(Device:mobile,OS:Android,Store:없음)** Android 중에 대표 스토어로 지정된 모바일 URL로 이동. 대표 스토어가 'Google Play'인 경우 'Google Play' 모바일에 설정된 URL로 이동.<br/>
_[예시2] 'One Store'에서 다운로드한 앱으로 게임 중이던 사용자가 '업데이트 필수' 팝업 창에서 '지금 업데이트' 버튼을 클릭한 경우_<br/>
**(Device:mobile,OS:Android,Store:One Store)** 'One Store' 모바일에 설정된 URL로 이동(One Store 모바일 설치 페이지)<br/>
_[예시3] PC에서 설치 URL을 입력한 경우_<br/>
**(Device:PC,OS:Windows,Store:없음)** COMMON PC에 설정된 URL로 이동<br/>


### Properties

입력된 설치 URL 정보를 변경하려면 **수정** 버튼을 클릭합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.1.png)

- 각 항목은 PC, 모바일별로 따로 설정할 수 있습니다. PC와 모바일을 구분할 필요가 없다면 동일한 값을 각각 입력하면 됩니다.<br />
#### (1) Common
스토어 정보가 없거나 스토어 이동에 실패했을 때 연결될 주소를 설정합니다.<br />
#### (2) Android
Android 사용자가 설치 URL을 실행할 때 연결될 주소를 설정합니다.<br />
원하는 스토어가 목록에 표시되지 않을 경우, [고객 센터](https://cloud.toast.com/support/faq/)로 연락 주시면 해당 스토어에 대한 추가가 가능합니다.<br />
#### (3) iOS
iOS 사용자가 설치 URL을 실행할 때 연결될 주소를 설정합니다.<br />
#### (4) WebGL
WebGL 클라이언트 내부에서 설치 URL을 실행하는 경우 연결할 주소를 설정합니다.  <br />
#### (5) Standalone
Standalone으로 서비스 되는 앱에서 연결될 주소를 설정합니다.<br />
