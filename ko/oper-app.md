## Game > Gamebase > 콘솔 사용 가이드 > 앱

TOAST Console에서 **Game > Gamebase > App**을 클릭하여 앱의 기본 정보를 설정할 수 있습니다.

* **앱**: 앱 정보 관리
* **클라이언트**: 클라이언트 버전과 상태 정보 관리
* **설치 URL**: 앱의 스토어별 설치 URL 관리


## App

Gamebaes 서비스를 활성화하면 자동으로 앱이 생성되며 해당 메뉴에서는 등록된 정보의 수정만 가능합니다.
TOAST 프로젝트 하나당 하나의 Gamebase 앱을 관리할 수 있으므로 앱을 추가로 등록하거나 삭제할 수는 없습니다. Gamebase 서비스를 비활성화 하시면 앱에 등록된 정보가 삭제됩니다.
각 항목별 상세 설명은 아래 **Properties** 항목을 참고합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.2.png)

### Properties

Gamebase Console에서 관리하는 앱 정보를 설명합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.2.png)

#### (1) 설치 URL
앱 설치와 홍보에 이용할 수 있는 단축 URL 정보입니다.
앱이 배포된 스토어가 여러 개인 경우에도 하나의 단축 URL로 관리할 수 있습니다.
자세한 동작 및 관리 방법은 다음 링크를 참고합니다. [설치 URL 관리](./oper-app/#installed-url)

> [참고] 
> Gamebase를 활성화하면 자동으로 생성되므로 변경은 불가능합니다.

####(2) 서버 주소
게임에서 게임 서버 주소(IP, URL 등)를 실시간으로 전달받아야 할 때 사용합니다.
서버 주소를 설정하면 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.
게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.

####(3) 고객 센터 정보
고객 센터 페이지 이외 이메일, 전화번호 등의 정보를 입력합니다.
고객 센터 페이지가 있는 경우에는 아래 **인앱 URL**의 **고객센터**에 정보를 입력합니다.
> [참고] <br/>
> 입력된 정보는 Gamebase가 제공하는 점검 상세 페이지에 표시됩니다.

####(4) 인증 정보
앱에서 로그인할 때 사용할 IdP의 인증 정보를 등록, 수정, 삭제할 수 있습니다.

외부 인증의 클라이언트 아이디, 비밀 키(secret key)뿐만 아니라 콜백 URL과 추가 정보를 설정할 수 있습니다. 
인증 정보 옆에 **+** 버튼을 클릭하면 정보를 추가할 수 있으며 **-** 버튼을 클릭하면 정보를 삭제할 수 있습니다.
Idp별 자세한 설정 방법은 [Authentication Information](#authentication-information)을 참고하시기 바랍니다.
> [참고] 
> **토큰 재검증**이란?
> 클라이언트에서 Latest Login API 호출 시에 외부 IdP의 토큰 재검증 여부를 설정합니다. 
> **검증 안함**을 선택하면 외부 IdP의 토큰 재검증 없이 내부 토큰 검증만을 수행합니다. 
> **항상 검증**을 선택하면 Gamebase에서 발급한 내부 토큰뿐만 아니라 외부 IdP 토큰에 대해서도 항상 유효성 검증을 진행합니다.


####(5) 인앱 URL
클라이언트를 다시 배포하지 않고 앱 내에서 자주 사용하는 URL을 Console을 통해 실시간으로 수정할 수 있습니다.

- 이용약관 
- 개인 정보동의 
- 이용 정지 규정 
- 고객센터 URL 

게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.
설정한 정보는 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.

### Test Device

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_1.0.png)
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검 중이어도 정상적으로 게임에 접근할 수 있습니다.
테스트 단말기를 등록하려면 **Device Key**를 입력해야 합니다. 직접 입력하거나 **게임유저 ID**를 조회하여 등록할 수 있습니다.
더 이상 사용하지 않는 테스트 단말기를 삭제할 수도 있습니다.

> [참고] 
> 테스트 단말기는 최대 100개까지만 등록할 수 있습니다.

#### (1) 조회

앱에 등록된 전체 테스트 단말기를 확인 할 수 있습니다. 검색어를 **Search**에 입력하여 검색 조건에 맞는 테스트 단말기를 찾아 볼 수 있습니다.

#### (2) 등록

조회 화면에서 **등록** 버튼을 클릭하면 테스트 단말기를 등록할 수 있는 화면이 나타납니다. **Device Key**를 직접 입력하거나 **게임유저 ID**를 검색해 테스트 단말기를 등록할 수 있습니다.

**(1) 게임유저 ID를 통한 등록**

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_2.1.png)

타입을 유저 ID로 선택하고 게임유저 ID를 입력하여 **검색** 버튼을 클릭하면 화면 하단에 사용자의 로그인 로그 내역이 조회됩니다. 조회된 내역에서 테스트 단말기로 등록하고자 하는 Device Key를 선택하여 **추가 정보**를 입력하여 **등록** 버튼을 클릭하면 해당 Device key가 테스트 단말기 정보로 등록됩니다.

> [참고] 
> 추가 정보에는 사용자가 알아보기 편한 별칭을 입력하시면 됩니다. 예시) iPhone 6 테스트, 토스트님 아이패드

**(2)Device Key를 통한 등록**
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_3.0.png)

등록하고자 하는 Device key를 알고 있을 경우 타입을 **Device Key**로 선택하여 직접 테스트 단말기를 등록할 수 있습니다.
Device Key와 등록 단말기의 **추가 정보**를 입력 후 등록 버튼을 누르면 테스트 단말기로 등록됩니다.

#### (3) 삭제

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_4.0.png)

테스트 단말기 조회 화면에서 삭제하고자 하는 테스트 단말기를 체크한 후 왼쪽 위의 삭제 버튼을 클릭하면 테스트 단말기 정보가 삭제됩니다. 삭제된 정보는 복구할 수 없으므로 삭제 전에 다시 한번 확인한 후 삭제하시기 바랍니다.

### Authentication Information

#### Facebook
Facebook 개발자 사이트에 등록한 앱의 {앱 아이디}와 {앱 시크릿 코드}를 Gamebase Console에 입력합니다.
이때, 로그인 시 필요한 {Facebook Permission} 또한 JSON String 형태로 추가 정보란에 입력해야 합니다.

**입력 필드 ** 

- ClientID: {AppID} 
- Secret Key: {App Secret Code} 
- 추가정보: Facebook Permission (json format) 


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

**[예시] facebook_permission format **

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"] }
```

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_permission_1.0.png)

**Reference URL **<br />

- [Facebook 개발자 사이트](https://developers.facebook.com/)
- [Facebook 권한](https://developers.facebook.com/docs/facebook-login/permissions/)




#### Google
Google 개발자 콘솔에 등록된 앱의 {클라이언트 ID} 및 {클라이언트 보안 비밀}, {리다이렉션 URI}를 Gamebase Console에 입력합니다.

**입력 필드**

- ClientID: {클라이언트 ID}
- Secret Key: {클라이언트 보안 비밀}
- Callback URL: {리다이렉션 URI}
  <br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

#### Apple Game Center
Apple 개발자 사이트에 등록된 BundleID를 Gamebase Console에 입력합니다.

**입력 필드**<br />

- ClientID: {Bundle ID}<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

**Reference URL**<br />

- [Apple Developer 사이트](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)

#### PAYCO
PAYCO Client ID를 신청해서 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.

**입력 필드**<br />

- ClientID: {Payco client_id}
- Secret Key: {Payco client_secret}

#### NAVER
Naver Developer 사이트에서 신청하여 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.
이때, 로그인 동의창에서의 노출될 애플리케이션이름 및 scheme등 iOS 애플리케이션에서 필요한 정보 또한 JSON String 형태로 추가 정보란에 입력해야 합니다.

**입력 필드**

- Client ID: {Naver client_id}
- Secret Key: {Naver client_secret}
- 추가정보: Naver Application Name & iOS url scheme (json format) 

**[예시] Naver Additional input format **
```json
{ "url_scheme_ios_only": "Your Url Scheme", "service_name": "Your Service Name" }
```

**Reference URL**<br />
- [Naver Developers - 애플리케이션 등록](https://developers.naver.com/apps/#/register)
- [Naver Developers - 클라이언트 아이디와 클라이언트 시크릿 확인](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)



## Client

클라이언트 정보를 운영체제(iOS, Android, Unity WebGL, Unity Standalone), 버전별로 관리할 수 있습니다.

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
현재 등록된 클라이언트 목록을 확인할 수 있습니다.
운영체제별로 구분되어 보여지며 아이콘 내 숫자는 클라이언트 등록 시 입력한 버전을 의미합니다.
아이콘 목록은 서비스 상태가 <font color="white" style="background-color:#F8BB28">테스트</font>, <font color="white" style="background-color:#FB8F37">심사중</font>, <font color="white" style="background-color:#88C637">서비스</font>, <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스중)</font>인 목록만 표시됩니다. 운영체제별 하단 오른쪽의 화살표를 클릭하면 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>, <font color="white" style="background-color:#CCCCCC">종료</font> 상태의 클라이언트 목록을 확인할 수 있습니다.
아이콘 색깔을 서비스 상태별로 구분하여 한눈에 서비스 상태를 파악할 수 있습니다.

### Properties

Gamebase Console에서 관리하는 클라이언트 등록 정보를 설명합니다.
**클라이언트** 탭에서 **AOS 등록**, **iOS 등록** 버튼 등을 클릭하면 클라이언트 등록 화면이 나타납니다. 등록된 클라이언트의 입력값을 수정하거나 삭제하고 싶다면 아이콘 목록에서 아이콘을 클릭하거나 클라이언트 전체 목록에서 원하는 클라이언트를 선택하시면 됩니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) 스토어
(<font color="red">필수</font>) 클라이언트를 배포할 스토어를 선택합니다. 
운영체제별로 선택 가능한 스토어가 다릅니다.
#### (2) 게임 버전
(<font color="red">필수</font>) 클라이언트 버전을 입력합니다. 
게임에서 정한 규칙에 따라 문자열로 입력하면 됩니다.
#### (3) 서비스 상태
(<font color="red">필수</font>) 클라이언트의 서비스 상태를 선택합니다.
상태는 <font color="white" style="background-color:#F8BB28">테스트</font>, <font color="white" style="background-color:#FB8F37">심사중</font>, <font color="white" style="background-color:#88C637">서비스</font>, <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스중)</font>, <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>, <font color="white" style="background-color:#CCCCCC">종료</font> 이렇게 6가지입니다.

- <font color="white" style="background-color:#F8BB28">테스트</font>: 내부 테스트
- <font color="white" style="background-color:#FB8F37">심사중</font>: 스토어 심사 중. 안정화 지표를 추가로 설정할 수 있습니다.
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
  
> [참고]
> '안정화 지표'란?<br/>
> Gamebase 내부에서 Gamebase API 호출 시에 남기는 지표 로그입니다. 
> 서비스 상태가 '심사중'일 때에 안정화 지표를 활성화하면 심사 중에 Gamebase 내부에서 문제가 발생한 경우 보다 쉽게 문제를 확인할 수 있습니다. 
> Gamebase Console에서 실시간으로 안정화 지표를 사용할지의 여부와 로그 레벨을 설정할 수 있습니다.

- <font color="white" style="background-color:#88C637">서비스중</font>: 정상 서비스
- <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font>: 정상 서비스. <br/>보다 안정적인 버전을 사용하도록 유도하기 위해 팝업을 표시합니다. 새로운 버전을 다운로드해서 이용하도록 유도하지만 사용자가 원하는 경우 현재 버전으로도 계속 서비스를 이용할 수 있습니다.<br />아래는 '업데이트 권장(서비스 중)' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>: 서비스 불가능. <br/>현재 게임에서 서비스를 지원하지 않는 버전으로, 최신 버전 설치 안내 팝업을 표시합니다.<br />아래는 '업데이트 필수' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[주의] </font> 
>  **업데이트 필수와 점검이 동시에 설정**되어 있을 경우 서비스 상태는 '업데이트 필수'가 됩니다.
>  점검 진행 도중 사용자에게 업데이트 필수 팝업을 표시하고 싶지 않다면 점검 완료 이후에 서비스 상태를 '업데이트 필수'로 변경해야 합니다.

- <font color="white" style="background-color:#CCCCCC">종료</font>: 서비스 불가능. <br/> 서비스가 종료된 버전인 경우 선택합니다.<br />아래는 '종료' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [참고] 
> 서비스 상태별 표시할 메시지 설정
> **업데이트 권장(서비스중)**, **업데이트 필수**, **종료** 상태인 경우 사용자에게 표시할 안내 메시지를 다국어로 설정할 수 있습니다. 
> 서비스 상태를 선택하면 각 상태에 맞는 기본 메시지가 5개(한국어, 영어, 일본어, 중국어 간체, 중국어 번체)의 언어로 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)

#### (4) 서버 주소
클라이언트에서 이용할 서버 주소(IP, URL)를 입력합니다.
**앱** 탭에서 서버 주소를 입력하면 모든 클라이언트에 적용되므로, 클라이언트마다 다른 서버 주소를 사용하고 싶을 때만 서버 주소를 입력합니다. 


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.2.png)

게임을 설치하기 위한 스토어 URL 정보를 관리합니다.
사용자가 PC나 모바일에서 단축 URL을 클릭하면, 사용자 단말기 정보(디바이스, 운영체제, 스토어 등)를 이용하여 입력된 사이트로 리디렉션합니다.
스토어 정보가 없거나 스토어 이동에 실패하면 'COMMON'에 설정된 URL로 이동합니다.

_[예시1] Android 휴대폰에서 문자로 받은 설치 URL을 클릭하는 경우
**(Device:mobile,OS:Android,Store:없음)** Android 중에 대표 스토어로 지정된 모바일 URL로 이동. 대표 스토어가 'Google Play'인 경우 'Google Play' 모바일에 설정된 URL로 이동.
_[예시2] 'One Store'에서 다운로드한 앱으로 게임 중이던 사용자가 '업데이트 필수' 팝업 창에서 '지금 업데이트' 버튼을 클릭한 경우_
**(Device:mobile,OS:Android,Store:One Store)** 'One Store' 모바일에 설정된 URL로 이동(One Store 모바일 설치 페이지)<br/>
_[예시3] PC에서 설치 URL을 입력한 경우_
**(Device:PC,OS:Windows,Store:없음)** COMMON PC에 설정된 URL로 이동


### Properties

입력된 설치 URL 정보를 변경하려면 **수정** 버튼을 클릭합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.2.png)

- 각 항목은 PC, 모바일별로 따로 설정할 수 있습니다. PC와 모바일을 구분할 필요가 없다면 동일한 값을 각각 입력하면 됩니다.
- 원하는 스토어가 목록에 표시되지 않을 경우, [고객 센터](https://toast.com/support/inquiry)로 연락 주시면 해당 스토어에 대한 추가가 가능합니다.

#### (1) Common
스토어 정보가 없거나 스토어 이동에 실패했을 때 연결될 주소를 설정합니다.
#### (2) Android
Android 사용자가 설치 URL을 실행할 때 연결될 주소를 설정합니다.

#### (3) iOS
iOS 사용자가 설치 URL을 실행할 때 연결될 주소를 설정합니다.
#### (4) Standalone
Standalone으로 서비스 되는 앱에서 연결될 주소를 설정합니다. Standalone은 PC에서만 동작하므로 PC설정만 하면 됩니다.


