## Game > Gamebase > 콘솔 사용 가이드 > 앱

NHN Cloud Console에서 **Game > Gamebase > App**을 클릭하여 앱의 기본 정보를 설정할 수 있습니다.

* **앱**: 앱 정보 관리
* **클라이언트**: 클라이언트 버전과 상태 정보 관리
* **설치 URL**: 앱의 스토어별 설치 URL 관리


## App

Gamebase 서비스를 활성화하면 자동으로 앱이 생성되며 해당 메뉴에서는 등록된 정보의 수정만 가능합니다.
NHN Cloud 프로젝트 하나당 하나의 Gamebase 앱을 관리할 수 있으므로 앱을 추가로 등록하거나 삭제할 수는 없습니다. Gamebase 서비스를 비활성화 하시면 앱에 등록된 정보가 삭제됩니다.
각 항목별 상세 설명은 아래 세부 항목을 참고합니다.

### 기본 정보
![gamebase_app_01_202009](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_01_202009.png)

#### (1) 설치 URL
앱 설치와 홍보에 이용할 수 있는 단축 URL 정보입니다.
앱이 배포된 스토어가 여러 개인 경우에도 하나의 단축 URL로 관리할 수 있습니다.
자세한 동작 및 관리 방법은 다음 링크를 참고합니다. [설치 URL 관리](./oper-app/#installed-url)

> [참고]
> Gamebase를 활성화하면 자동으로 생성되므로 변경은 불가능합니다.

#### (2) 테스트 결제 포함 여부
앱의 지표를 볼 때 테스트 결제도 함께 지표에 포함할 지에 대한 여부를 선택합니다.
기본 설정은 '테스트 결제 포함'으로 설정되어 있으며 '테스트 결제 제외'로 설정하면 Analytics 매출 지표에서 테스트 결제건은 모두 제외하여 보여줍니다.
> [참고1]
> 지표 표시 설정여부에 관계 없이 데이터는 테스트 결제 건과 실제 결제 건을 항상 쌓고 있으므로 테스트 결제에 대한 표시여부는 언제든지 변경해도 실제 데이터 수집에는 영향이 없습니다.

> [참고2]
> 테스트 데이터는 Google 및 AppStore만 지원하고 있으며 다른 스토어는 지원하지 않습니다.
> 각 스토어 별 테스트 지표 기준은 아래와 같습니다.
> * Google : Google 콘솔에 등록한 테스트 계정을 이용하여 결제를 진행한 내역
> * AppStore : Sandbox 환경에서 테스트 결제를 진행한 내역

#### (3) 탈퇴 유예 기간
앱의 탈퇴 유예 기능을 사용하고자 할 경우 탈퇴 유예시 부여할 기간을 설정합니다.
기본 설정은 '7일'로 설정되어 있으며 1일 ~ 30일까지 설정이 가능합니다.
> [참고]
> 탈퇴 유예 기간동안에는 정상적으로 서비스 이용이 가능합니다.

### 서버 주소
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_02_202012.png)

- 게임에서 게임 서버 주소(IP, URL 등)를 실시간으로 전달받아야 할 때 사용합니다.
- 서버 주소를 설정하면 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.
- 클라이언트의 상태별로 서버 주소를 설정할 수 있고 런칭 정보에서 서버 주소를 확인할 수 있습니다.
- 게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.

### 언어 설정
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_03_202004.png)
- 각 메뉴의 다국어 설정에서 기본적으로 노출할 언어를 미리 지정할 수 있습니다.
- 다국어 항목을 표시할 때 선택한 언어들이 표시되며 기본 언어도 설정한 항목으로 선택되어 있습니다.
- 사용하고자 하지 않을 경우에는 해당 란을 비워 두시면 됩니다.

###  인증 정보
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_04_202004.png)

앱에서 로그인할 때 사용할 IdP의 인증 정보를 등록, 수정, 삭제할 수 있습니다.

외부 인증의 클라이언트 아이디, 비밀 키(secret key)뿐만 아니라 콜백 URL과 추가 정보를 설정할 수 있습니다.
인증 정보 옆에 **+** 버튼을 클릭하면 정보를 추가할 수 있으며 **-** 버튼을 클릭하면 정보를 삭제할 수 있습니다.
Idp별 자세한 설정 방법은 [Authentication Information](#authentication-information)을 참고하시기 바랍니다.
> [참고]
> **토큰 재검증**이란?
> 클라이언트에서 Latest Login API 호출 시에 외부 IdP의 토큰 재검증 여부를 설정합니다.
> **검증 안함**을 선택하면 외부 IdP의 토큰 재검증 없이 내부 토큰 검증만을 수행합니다.
> **항상 검증**을 선택하면 Gamebase에서 발급한 내부 토큰뿐만 아니라 외부 IdP 토큰에 대해서도 항상 유효성 검증을 진행합니다.

### 인앱 URL
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_05_202009.png)
클라이언트를 다시 배포하지 않고 앱 내에서 자주 사용하는 URL을 Console을 통해 실시간으로 수정할 수 있습니다.

- 이용약관
- 개인 정보동의
- 이용 정지 규정

게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.
설정한 정보는 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.

### 고객센터
고객센터 관련 설정을 진행할 수 있습니다.
현재 Gamebase에서는 3가지의 고객센터 형식을 제공하고 있으며 선택되는 고객센터 유형별로 설정할 수 있는 항목이 다릅니다.
각 고객센터 유형별 설정은 아래와 같습니다.

#### 1. 개발사 자체 고객센터
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_19_202009.png)
개발사에서 자체적으로 고객센터를 사용하고 있을 경우 설정합니다.
설정 항목은 아래와 같습니다.
* **고객센터 URL** : 현재 제공하거나 사용하고 있는 개발사의 자체 고객센터 주소를 입력합니다.
* **연락처** : 고객센터 연락처를 입력합니다. 이 정보는 Gamebase SDK를 통해 전달받을 수 있습니다.

#### 2. Gamebase 제공 고객센터
![gamebase_app_20_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_20_202009.png)
Gamebase에서 제공하는 고객센터 기능을 사용하고자 할 때 설정합니다.
설정 항목은 아래와 같습니다.
* **고객센터 URL** : 고객에게 문의를 인입받을 수 있는 페이지 정보를 제공합니다. 해당 URL은 Gamebase 제공 고객센터를 선택할 경우 자동으로 생성되며 이 URL을 통해 고객의 문의를 별도의 웹페이지를 통해 전달받을 수 있습니다.
* **연락처** : 고객센터 연락처를 입력합니다. 이 정보는 Gamebase SDK를 통해 전달받을 수 있습니다.
* **지원 언어** : 고객센터 사용자에게 지원할 언어를 선택합니다. 프로젝트 자체 언어설정과는 별도로 설정되는 정보이며 현재는 한국어, 영어, 일본어, 중국어를 지원하고 있습니다. 이 설정에서 선택된 언어를 기반으로 Gamebase 고객센터 기능을 사용할 수 있습니다.
* **기본 언어** : 지원 언어에서 선택한 항목 중 고객센터 내에서 기본으로 제공할 언어를 선택합니다.

#### 3. NHN Cloud 조직 상품(Online Contact)
![gamebase_app_21_202107.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_21_202107.png)
NHN Cloud에서 조직별로 제공되는 Online contact 상품을 사용하는 경우 설정합니다.
설정 항목은 아래와 같습니다.
* **고객센터 URL** : NHN Cloud Online Contact에서 제공되는 주소를 입력합니다. 해당정보는 NHN Cloud Online Contact에 접속하여 확인할 수 있습니다.
* **연락처** : 고객센터 연락처를 입력합니다. 이 정보는 Gamebase SDK를 통해 추가로 정보를 전달받을 수 있습니다.
* **OC 조직 Key** : NHN Cloud Online Contact 고객센터 문의를 확인하기 위한 Key를 입력합니다. 해당정보를 입력하지 않을 경우 고객센터 페이지 내에서 인입된 문의를 확인할 수 없으므로 확인 후 입력해야 합니다. 자세한 연동 방법은 아래 참고내용을 보시고 연동해주시면 됩니다.
> [참고] NHN Cloud Online Contact와 Gamebase간의 연동
> Gamebase내에서 NHN Cloud Online Contact 연동하고자 할 경우 아래 과정에 따라 SSO 로그인 API Key를 발급받아 Gamebase내에 설정해주셔야 고객센터 서비스를 정상적으로 이용할 수 있습니다.
> 고객센터의 안정적인 서비스 제공을 위해 아래 순서대로 참고하시어 진행 해주시기 바랍니다.
>
> 1) NHN Cloud Online Contact에 회원 연동 방식 설정
> 서비스 관리 -> 헬프센터 -> 회원 연동
> ![gamebase_app_22_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_22_202102.png)
> 회원 연동 활성화: 활성화
> 로그인 타입: GET 방식
> Token 검증 URL: https://gamebase-web.cloud.toast.com/tcgb-web/v1.0/apps/{appId}/online-contact/login-status
> **{appId}** 부분은 설정하고자 하는 Gamebase의 프로젝트 ID를 확인하신 후 해당 위치에 입력해주시면 됩니다.
> 
> 2) OC 조직 Key를 획득하여 OC 조직 Key항목에 입력
> 전체 관리 -> 계약 서비스 현황 -> 조직 정보로 이동한 후 OC 조직 정보의 OC 조직 Key를 복사하여 Gamebase OC 조직 Key 항목에 입력 
> ![gamebase_app_25_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_25_202102.png)
>
> 3) NHN Cloud Online contact 고객센터 페이지 주소를 획득하여 고객센터 URL에 입력
> 헬프센터 -> 하위메뉴 선택 -> 우측 위 헬프센터 바로가기 클릭
> ![gamebase_app_26_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_26_202009.png)
> 브라우저 상단에 표시된 주소를 Gamebase 고객센터 URL 항목에 입력
> ![gamebase_app_27_202102.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_27_202102.png)
>

### Test Device

![gamebase_app_02_201912.png](http://static.toastoven.net/prod_gamebase/gamebase_app_02_201912.png)
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검 중이어도 정상적으로 게임에 접근할 수 있습니다.
테스트 단말기를 등록하려면 **Device Key** 또는 **IP** 정보를 등록해야 합니다. 직접 입력하거나 **게임유저 ID**를 조회하여 등록할 수 있습니다.
점검시 게임플레이가 가능할 수 있도록 하거나 단말기별 Debug Log 출력 여부를 설정하여 테스트 단말기를 관리할 수 있습니다.
더 이상 사용하지 않는 테스트 단말기를 삭제할 수도 있습니다.
접속 이력 확인버튼을 누르면 해당 기기를 통한 **점검이 진행되는 동안의 접속 시간 및 상세 접속 로그**를 확인할 수 있습니다.
![gamebase_app_09_201912.png](http://static.toastoven.net/prod_gamebase/gamebase_app_09_201912.png)

> [참고]
> 테스트 단말기는 최대 100개까지만 등록할 수 있습니다.

#### (1) 조회

앱에 등록된 전체 테스트 단말기를 확인 할 수 있습니다. 검색어를 **Search**에 입력하여 검색 조건에 맞는 테스트 단말기를 찾아 볼 수 있습니다.

#### (2) 등록

조회 화면에서 **등록** 버튼을 클릭하면 테스트 단말기를 등록할 수 있는 화면이 나타납니다. **Device Key**를 직접 입력하거나 **게임유저 ID**를 검색해 테스트 단말기를 등록할 수 있습니다.

![gamebase_app_10_201912.png](http://static.toastoven.net/prod_gamebase/gamebase_app_10_201912.png)
![gamebase_app_11_202107.png](http://static.toastoven.net/prod_gamebase/gamebase_app_11_202107.png)

**(1) 게임유저 ID를 통한 등록**

타입을 유저 ID로 선택하고 게임유저 ID를 입력하여 **검색** 버튼을 클릭하면 화면 하단에 사용자의 로그인 로그 내역이 조회됩니다. 조회된 내역에서 테스트 단말기로 등록하고자 하는 Device Key를 선택하여 **추가 정보**를 입력하여 **등록** 버튼을 클릭하면 해당 Device key가 테스트 단말기 정보로 등록됩니다.



**(2)Device Key 또는 IP를 통한 등록**

등록하고자 하는 Device key 또는 IP 정보를 알고 있을 경우 타입을 원하는 입력방식으로 선택하여 직접 테스트 단말기를 등록할 수 있습니다.
등록하고자 하는 단말기의 **단말기 이름** 및 디버그 로그, 점검 무시 여부를 입력 후 등록 버튼을 누르면 테스트 단말기로 등록됩니다.

> [참고]
> 단말기 이름에는 사용자가 알아보기 편한 별칭을 입력하시면 됩니다. 예시) iPhone 6 테스트, 토스트님 아이패드

#### (3) 삭제

![gamebase_app_12_201912.png](http://static.toastoven.net/prod_gamebase/gamebase_app_12_201912.png)

테스트 단말기 조회 화면에서 삭제하고자 하는 테스트 단말기를 체크한 후 왼쪽 위의 삭제 버튼을 클릭하면 테스트 단말기 정보가 삭제됩니다. 삭제된 정보는 복구할 수 없으므로 삭제 전에 다시 한번 확인한 후 삭제하시기 바랍니다.

### Authentication Information

#### 1. Facebook
Facebook 개발자 사이트에 등록한 앱의 {앱 아이디}와 {앱 시크릿 코드}를 Gamebase Console에 입력합니다.
이때, 로그인 시 필요한 {Facebook Permission} 또한 JSON String 형태로 추가 정보란에 입력해야 합니다.

**입력 필드**

- ClientID: {AppID}
- Secret Key: {App Secret Code}
- 추가정보: Facebook Permission (json format)


![gamebase_app_04_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_04_201812.png)

**[예시] facebook_permission format**
* Facebook의 경우, OAuth 인증 시도 시, Facebook에 요청할 정보의 종류를 설정해야 합니다.

```json
{ "facebook_permission": [ "public_profile", "email"] }
```

![gamebase_app_05_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_05_201812.png)

**Reference URL**<br />

- [Facebook 개발자 사이트](https://developers.facebook.com/)
- [Facebook 권한](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
NHN Cloud Console에서의 설정 외에 추가 설정은 없습니다.



#### 2. Google

##### Google Cloud Console

![gamebase_app_06_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_06_201812.png)

1. Google 인증을 위해서는 Google Cloud Console에서 **Web Application Client ID**를 발급받아 Gamebase Console에 입력해야 합니다.
2. 승인된 리디렉션 URI 란에 다음 값을 입력합니다.
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback

##### iOS
* [Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP Settings > Google](./ios-started/#google)


#### 3. Apple Game Center
Apple 개발자 사이트에 등록된 BundleID를 Gamebase Console에 입력합니다.

**입력 필드**<br />

- ClientID: {Bundle ID}<br />

![gamebase_app_08_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_08_201812.png)

**Reference URL**<br />

- [Apple Developer 사이트](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)


#### 4. PAYCO
PAYCO Client ID를 신청해서 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.

**입력 필드**<br />

- ClientID: {Payco client_id}
- Secret Key: {Payco client_secret}
- 추가정보: Payco Service & Service Name (JSON format)

##### Additional Info Settings

* **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
* PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

* PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "Your Service Code", "service_name": "Your Service Name" }
```

##### iOS
* [Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings > Payco](./ios-started/#payco)

#### 5.NAVER
Naver Developers 사이트에서 신청하여 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.
이때, 로그인 동의 창에서 표시할 애플리케이션 이름인 **service_name** 을 설정해야 합니다.

**입력 필드**

- Client ID: {NAVER client_id}
- Secret Key: {NAVER client_secret}
- 추가정보: NAVER Application Name (json format)

**Reference URL**

- [NAVER Developers - 애플리케이션 등록](https://developers.naver.com/apps/#/register)
- [NAVER Developers - 클라이언트 아이디와 클라이언트 시크릿 확인](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Additional Info Settings

* **NHN Cloud Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야 합니다.
* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

```json
{"service_name": "Your Service Name" }
```

##### iOS
* [Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings > Naver](./ios-started/#naver)

#### 6. Twitter
Twitter Application Management 사이트에서 앱을 등록하고 발급받은 {Consumer Key} 및 {Consumer Secret}을 Gamebase Console에 입력합니다.

**입력 필드**

- Client ID: {Twitter Consumer Key}
- Secret Key: {Twitter Consumer Secret}

**Reference URL**
- [Twitter Application Management](https://apps.twitter.com/)

##### Android
 > <font color="red">[주의]</font><br/>
 >
 > 2019년 7월 25일부로 Twitter에서는 TLS 1.0, TLS 1.1 지원을 중단하고, TLS1.2만 지원합니다.
 > 이에, Android 4.3(Jelly Bean, API Level 18) 이하의 단말기에서는 Android WebView로 Twitter에 로그인할 수 없습니다.
 >
 > 즉, Android 4.4 이상(KitKat, API Level 19)인 기기에서만 Twitter 로그인을 사용할 수 있습니다.

##### iOS
* [Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings > Twitter](./ios-started/#twitter)


#### 7. LINE

**입력 필드**
- Client ID: {LINE Channel ID}
- Secret Key: {LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
* [Gamebase > iOS SDK 사용 가이드 > 시작하기 > IdP settings > Line](./ios-started/#line)

#### 8. Sign In with Apple
Sign In with Apple 기능을 사용하려면 App Store Connect, Gamebase 콘솔, Xcode에 설정이 필요합니다.

##### AppStore Connect Settings
* [Certificates, Identifiers & Profiles \> Keys 로 바로가기](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > 추가(+)
1. **Sign In with Apple** 체크 박스를 선택하고 설정을 진행합니다.
![Check SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. **Sign in with Apple**을 사용할 Bundle ID를 선택합니다.
![ChooseAPrimaryAppID](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. <span style="color:#e11d21">Privatekey</span>를 다운로드 후 보관, 생성된 <span style="color:#e11d21">Key ID를 </span>확인합니다.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Certificates, Identifiers & Profiles > Identifiers > 대상 앱을 선택하고 **Sign In with Apple**을 활성화합니다.
    * `Enable as a primary App ID` 로 설정합니다.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[NHN Cloud Console 바로가기](https://console.toast.com/)

* Gamebase
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid4_1.0.png)


###### Client ID Settings
> 앱의 Bundle ID를 설정합니다.

###### Secret Key Settings
> Apple Developer Account 설정에서 획득한 값(**TeamID**, **KeyID**, **PrivateKey**)으로 JSON 문자열을 생성해 설정합니다.

* `teamId`: 개발자 계정 오른쪽 상단의 값을 설정합니다.
* `keyId`: Certificates, Identifiers & Profiles > Keys > Sign In with Apple을 선택하여 생성된 값을 설정합니다.
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid5_1.0.png)
* `privateKey`: 위의 Keys에서 키를 생성하면서 같이 생성된 PrivateKey 파일의 내용을 설정합니다. 다운로드한 파일을 열어서 아래 그림과 같이 빨간 사각형 부분의 값을 사용합니다.
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

위의 값을 아래의 예제와 같이 JSON으로 만들어서 설정합니다.


```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. 중략"
}
```

###### Additional Info Settings
[Sign In with Apple 의 AuthorizationScope 알아보기](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

Gamebase 콘솔 **App**에서 Apple을 추가하면 기본으로 아래의 JSON값이 설정됩니다.
2019년 11월 기준으로 Scope의 종류는 `full_name`, `email`만 있으며, Gamebase에서는 이 두 가지 값을 기본값으로 설정합니다.

```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[주의]</font><br/>
>
> Xcode 11 이상에서만 **Sign In with Apple** 기능을 사용하는 프로젝트를 빌드할 수 있습니다.


1. **Target**을 선택하고 **Signing & Capabilities**에서 **Sign In with Apple** 항목을 추가합니다.
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. **Target**을 선택하고 **Build Phases > Link Binary With Libraries**에서 **Authentication.framework**를 **Optional**로 추가합니다.
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)

> <font color="red">[주의]</font><br/>
> Optional이 아닌 Required로 설정되어 있으면 iOS 12 이하의 단말기에서는 앱 실행 시 런타임 크래시가 발생합니다.





##### iOS 12 버전 이하를 지원하기 위한 설정 (Sign In with Apple JS)

> <font color="red">[주의]</font><br/>
>
> Gamebase SDK iOS 2.13.0 이상 버전에서는 iOS 12 이하 버전에서의 WebView를 이용한 Sign In with Apple 기능 사용이 가능합니다.
>
> 기존의 2.13.0 이전버전을 사용했던 게임의 경우에도 하단 **iOS 12 버전 이하를 지원하기 위한 설정** 을 참고하여 기존 프로젝트를 설정하고,
>
> Gambase SDK iOS 2.13.0 이상을 적용하면, iOS 12 이하버전에서 Sign In with Apple 기능을 사용할 수 있습니다.


* iOS 12 이하 버전에서 Sign In with Apple 을 사용하기 위해서는 Sign In with Apple JS 를 사용해서, 웹페이지를 통해 로그인을 해야합니다.
* Apple ID 로그인 웹페이지에서는 Apple 계정과 비밀번호를 입력해서 로그인을 할 수 있습니다.

**아래의 절차를 따라서 Apple 개발자 사이트에서 새로운 Service ID 를 등록해야 합니다.**

1. Service ID 를 추가<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_01.png)
2. Service ID 로 사용할 식별자를 설정 (일반적으로는 bundle ID + **.구분할 문자열**)<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_02.png)
3. 등록된 Service ID 를 확인 후 수정<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_03.png)
4. 하단의 Sign In with Apple 항목의 Configure 를 클릭<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_04.png)
5. Primary App ID 를 설정 (기존에 Sign In with Apple 을 사용하고 있었다면, 해당앱의 Bundle ID 를 설정)<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_05.png)
6. Apple ID 로 인증한 이후 인증 정보를 받을 Callback URL 설정<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_06.png)
7. 설정 후 저장<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_07.png)


**위에서 설정한 Service ID 를 NHN Cloud Gamebase Console > Gamebase > 앱 > 인증 정보 > Apple > Service ID 에 입력합니다.***

> <font color="red">[주의]</font><br/>
>
> 기존에 Sign In with Apple 설정이 되어 있지 않다면, 나머지 항목도 설정이 필요합니다.

1. Apple 개발자 사이트에서 설정한 Service ID 를 아래와 같이 Service ID 항목에 추가합니다. (기존에 Sign In with Apple 설정값이 있다면, 다른 값들은 변경이 필요없습니다.)
![Set Service ID for Sign In with Apple JS](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_TOAST_01.png)


#### 9. WEIBO

##### Weibo Console

1. Weibo Developers 사이트에서 신청하여 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.
이때, 로그인 시 필요한 {scope} 또한 JSON String 형태로 추가 정보란에 입력해야 합니다.


![gamebase_app_29_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_29_202012.png)

2. callback URL 란에 다음 값을 입력합니다.
	* Authorization callback page : https://api.weibo.com/oauth2/default.html
	* Cancel authorization callback page : https://api.weibo.com/oauth2/default.html


**입력 필드**

- ClientID: {App Key}
- Secret Key: {App Secret}
- 추가정보: scope (json format)


**Additional Info Settings**

* Scope

Application 에서 필요로 하는 권한을 나타냅니다.
Weibo 가이드 문서에 따라 기본값으로 모든 권한이 선언되어 있습니다.
필요에 따라 추가/제거/변경하실 수 있습니다.

* oauthApiUrl

내부적으로 Weibo Open API 를 호출하기 위한 도메인 입니다.
변경해서는 안됩니다.


![gamebase_app_28_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_28_202012.png)


**Reference URL**
- [Weibo Developer](https://open.weibo.com/)



## Client

클라이언트 정보를 운영체제(iOS, Android, Unity WebGL, Unity Standalone), 버전별로 관리할 수 있습니다.

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
현재 등록된 클라이언트 목록을 확인할 수 있습니다.
운영체제별로 구분되어 보여지며 아이콘 내 숫자는 클라이언트 등록 시 입력한 버전을 의미합니다.
아이콘 목록은 서비스 상태가 <font color="white" style="background-color:#eed14c">테스트</font>, <font color="white" style="background-color:#eba34b">베타 서비스</font>, <font color="white" style="background-color:#eb7e4b">심사중</font>, <font color="white" style="background-color:#88C637">서비스</font>, <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스중)</font>인 목록만 표시됩니다. 운영체제별 하단 오른쪽의 화살표를 클릭하면 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>, <font color="white" style="background-color:#CCCCCC">종료</font> 상태의 클라이언트 목록을 확인할 수 있습니다.
아이콘 색깔을 서비스 상태별로 구분하여 한눈에 서비스 상태를 파악할 수 있습니다.

### Properties

Gamebase Console에서 관리하는 클라이언트 등록 정보를 설명합니다.
**클라이언트** 탭에서 **AOS 등록**, **iOS 등록** 버튼 등을 클릭하면 클라이언트 등록 화면이 나타납니다. 등록된 클라이언트의 입력값을 수정하거나 삭제하고 싶다면 아이콘 목록에서 아이콘을 클릭하거나 클라이언트 전체 목록에서 원하는 클라이언트를 선택하시면 됩니다.
![gamebase_app_13_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_202012.png)
#### (1) 스토어
(<font color="red">필수</font>) 클라이언트를 배포할 스토어를 선택합니다.
운영체제별로 선택 가능한 스토어가 다릅니다.
#### (2) 게임 버전
(<font color="red">필수</font>) 클라이언트 버전을 입력합니다.
게임에서 정한 규칙에 따라 문자열로 입력하면 됩니다.
#### (3) 서비스 상태
(<font color="red">필수</font>) 클라이언트의 서비스 상태를 선택합니다.
상태는 <font color="white" style="background-color:#eed14c">테스트</font>, <font color="white" style="background-color:#eba34b">베타 서비스</font>, <font color="white" style="background-color:#eb7e4b">심사중</font>, <font color="white" style="background-color:#88C637">서비스</font>, <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스중)</font>, <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>, <font color="white" style="background-color:#CCCCCC">종료</font> 이렇게 6가지입니다.

- <font color="white" style="background-color:#F8BB28">테스트</font>: 내부 테스트
- <font color="white" style="background-color:#eba34b">베타 서비스</font>: 서비스 서버가 아닌 별도의 베타 서버에 연결이 필요한 경우 선택합니다.
- <font color="white" style="background-color:#FB8F37">심사중</font>: 스토어 심사 중
![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

- <font color="white" style="background-color:#88C637">서비스중</font>: 정상 서비스
- <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font>: 정상 서비스. <br/>보다 안정적인 버전을 사용하도록 유도하기 위해 팝업을 표시합니다. <br/>새로운 버전을 다운로드해서 이용하도록 유도하지만 사용자가 원하는 경우 현재 버전으로도 계속 서비스를 이용할 수 있습니다.<br />아래는 '업데이트 권장(서비스 중)' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.

- <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>: 서비스 불가능. <br/>현재 게임에서 서비스를 지원하지 않는 버전으로, 최신 버전 설치 안내 팝업을 표시합니다.<br />아래는 '업데이트 필수' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.

>  <font color="red">[주의] </font>
>  **업데이트 필수와 점검이 동시에 설정**되어 있을 경우 서비스 상태는 '업데이트 필수'가 됩니다.
>  점검 진행 도중 사용자에게 업데이트 필수 팝업을 표시하고 싶지 않다면 점검 완료 이후에 서비스 상태를 '업데이트 필수'로 변경해야 합니다.
>  <font color="orange">[참고] </font>
>  업데이트 버튼을 누르면 설치 URL 메뉴에서 설정한 각각의 스토어 주소로 연결됩니다.
>  예를 들면 클라이언트가 App store로 설정되어 있고 설치 URL 메뉴에서 App store 관련 설정이 존재한다면 설정한 주소로 이동되며 만약 설치 URL 메뉴에 설정이 되어 있지 않을 경우 공통(Common) URL로 연결됩니다.

- <font color="white" style="background-color:#CCCCCC">종료</font>: 서비스 불가능. <br/> 서비스가 종료된 버전인 경우 선택합니다.<br />아래는 '종료' 상태일 때 Gamebase SDK에서 기본적으로 제공하는 팝업입니다.

> [참고]
> 서비스 상태별 표시할 메시지 설정
> **업데이트 권장(서비스중)**, **업데이트 필수**, **종료** 상태인 경우 사용자에게 표시할 안내 메시지를 다국어로 설정할 수 있습니다.
> 서비스 상태를 선택하면 앱에 설정되어 있는 언어 설정 정보에 따라 각 상태에 맞는 기본 메시지가 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.
> 만약 이전에 각 상태로 설정되어 있던 각 언어별 설정들이 있다면 앱의 언어 설정 정보에 관계 없이 이전에 등록했던 내용들을 불러와 보여지게 됩니다.
> 앱의 언어 설정에 설정된 정보가 없을 경우 5개(한국어, 영어, 일본어, 중국어 간체, 중국어 번체)의 언어로 기본 메시지가 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.
> ![gamebase_app_18_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_18_202004.png)

#### (4) 서버 주소
클라이언트에서 이용할 서버 주소(IP, URL)를 입력합니다.
**앱** 탭에서 서버 주소를 입력하면 모든 클라이언트에 적용되므로, 클라이언트마다 다른 서버 주소를 사용하고 싶을 때만 서버 주소를 입력합니다.

#### (5) Debug log
Gamebae SDK의 Debug Log 출력 여부를 콘솔에서 실시간으로 변경할 수 있습니다.
설정되어 있지 않으면 기본적으로 Gamebase SDK 내부에 설정된 값을 우선으로 동작하고 Gamebase 콘솔에서 Debug Log 출력 여부를 설정할 수 있습니다.
Gamebase SDK에 Debug Log가 'OFF' 상태이더라도 콘솔에서 'ON'으로 설정하면 단말기에 Gamebase Debug Log가 출력됩니다.

#### (6) 메모
해당 클라이언트에 대한 간단한 메모를 30자 내로 입력할 수 있습니다.

## Terms Of Service
게임에 보여줄 약관을 생성 및 구성을 설정합니다.
![gamebase_app_30_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_30_202102.png)
### (1) 생성된 약관 목록
- **+** 버튼을 클릭하여 약관을 추가로 생성할 수 있습니다.
![gamebase_app_31_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_31_202102.png)

### (2) 약관의 국가 타입

### (3) 약관의 대상 국가
- 국가 타입이 기타 국가인 경우 대상 국가를 추가로 선택할 수 있습니다.

### (4) 약관의 구성
- 드래그 앤 드롭 방식으로 약관 항목의 순서를 지정할 수 있습니다.
- 약관 항목은 상위 약관 5개, 하위 약관 5개 총 25개를 생성할 수 있습니다.

### (5) 약관 항목 생성
- 약관 항목 목록에서 약관 구성을 선택한 후, **추가** 버튼을 클릭하면 해당 약관의 하위 약관이 생성됩니다.
- 하위 약관을 선택한 경우 약관을 생성할 수 없습니다.

### (6) 선택한 약관의 상세 정보
- 약관 명
	- 약관을 관리하기 위한 약관 이름입니다.
- 약관 동의
	- 약관 동의 필수 여부입니다.
- 상세 페이지
	- 없음: 상세 페이지가 존재하지 않는 경우입니다.
	- URL 입력: 상세 페이지의 URL을 설정할 수 있습니다.
	- 직접 입력: 상세 페이지를 생성할 수 있습니다.
![gamebase_app_32_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_32_202102.png)
- 표시할 텍스트
	- 게임에 표시할 텍스트입니다.
	- **+** 버튼을 클릭하여 언어를 추가할 수 있습니다.
	- 지원하지 않는 언어를 요청할 경우, 버튼에 체크된 언어를 기본으로 보여줍니다.
- 푸시 동의
	- 없음: 푸시 관련 동의가 아닌 경우입니다.
	- 광고성 수신 동의: 광고성 푸시 수신 동의가 필요한 약관입니다.
	- 광고성 야간 수신 동의: 광고성 야간 푸시 수신 동의가 필요한 약관입니다.

> <font color="red">[주의]</font><br/>
>
> 저장 버튼을 클릭하지 않으면 작성한 약관 내용이 적용되지 않습니다.
> 저장시 현재 선택된 약관 상세 정보만 저장 됩니다.
>


## Terms Of Service Deploy

게임에 표시할 약관 배포 및 배포 이력입니다.
![gamebase_app_33_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_33_202102.png)

### (1) 기본 약관 설정
![gamebase_app_35_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_35_202102.png)

- 생성한 약관 중 설정된 배포 국가 이외의 국가에서 접속할 경우 기본으로 노출될 약관을 선택합니다.

> <font color="red">[주의]</font><br/>
>
> 기본 약관은 설정하지 않을 수도 있습니다. 기본 약관이 설정되지 않았다면  배포된 국가 이외의 국가에서 접속시 약관이 노출 되지 않습니다.
>

### (2) 약관 목록

- 현재 생성된 약관 목록입니다.

### (3) 미리보기
![gamebase_app_36_202102](https://static.toastoven.net/prod_gamebase//gamebase_app_36_202102.png)

- 약관 목록에서 선택된 약관을 미리볼 수 있습니다.

### (4) 약관 배포 및 배포 이력
#### 배포
- 약관 목록에서 선택된 약관을 배포할 수 있습니다.
- 약관 재동의 체크 후 배포를 진행하면, 기존에 약관 동의를 받았던 유저에게도 새롭게 약관창이 노출 됩니다. 문구 등의 단순 수정시에는 약관 재동의를 체크 할 필요가 없습니다.

#### 배포 이력
![gamebase_app_34_202102](https://static.toastoven.net/prod_gamebase/gamebase_app_34_202102.png)
- 약관 목록에서 선택된 약관의 배포 이력입니다.

## Installed URL

게임을 설치하기 위한 스토어 URL 정보를 관리합니다.

![gamebase_app_19_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812_en.png)

클라이언트 상태가  <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font> 또는 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font>일 때 스토어별로 제공할 주소의 값을 설정합니다.
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

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_20_201812.png)

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

## Transfer account
게스트로 로그인한 게임 유저가 다른 아이디 제공자를 연동하지 않고 다른 단말기에서 이어서 게임을 할 수 있는 기능을 제공합니다.
사용자는 현재 게임 중인 단말기에서 이전을 위한 키를 발급받아 이전하려는 단말기에 키를 입력하는 것만으로 쉽게 게임 단말기를 변경할 수 있습니다.
**단말기 이전** 기능은 기본적으로 비활성화되어 있습니다. 사용하려면 **단말기 이전**에서 **사용하기**를 클릭합니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_1.0.png)

**사용하기** 버튼을 클릭한 후 단말기 이전에 필요한 정보를 입력합니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_2.0.png)
각 항목에 대한 설명은 아래와 같습니다.

### Properties

#### 발급
단말기 이전 발급 키의 형식을 설정합니다.
단말기 이전 키는 ID만 사용하거나 ID, 비밀번호 두 개의 키를 이용할 수 있습니다. ID, 비밀번호의 형식은 게임에서 원하는 소문자, 대문자, 숫자 조합으로 구성할 수 있습니다.

1. **ID 자동 발급 형식**: 단말기 이전 ID 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
- **숫자(최소길이:12)** : 숫자로만 된 아이디를 발급합니다. 발급되는 ID의 최소 길이는 12자입니다.
- **숫자+소문자(최소길이:10)** : 숫자와 소문자의 조합으로만 구성된 ID를 발급합니다. 발급되는 ID의 최소 길이는 10자입니다.
- **숫자+대문자(최소길이:10)** : 숫자와 대문자의 조합으로만 구성된 ID를 발급합니다. 발급되는 ID의 최소 길이는 10자입니다.
- **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 구성된 ID를 발급합니다. 발급되는 ID의 최소 길이는 9자입니다.
- **소문자+대문자(최소길이:9)** : 소문자와 대문자의 조합으로만 구성된 ID를 발급합니다. 발급되는 ID의 최소 길이는 9자입니다.

2. **비밀번호 자동 발급 형식** : 단말기 이전 ID를 이용하여 로그인할 때 사용할 비밀번호 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
- **비밀번호 사용 안함** : 비밀번호를 사용하지 않을 때 선택합니다. 이 항목을 선택하면 아래 검증 항목에서 아이디의 유효 시간만 설정할 수 있습니다.
- **숫자(최소길이:12)** : 숫자로만 구성된 비밀번호를 발급합니다. 발급되는 비밀번호의 최소 길이는 12자입니다.
- **숫자+소문자(최소길이:10)** : 숫자와 소문자의 조합으로만 구성된 비밀번호를 발급합니다. 발급되는 비밀번호의 최소 길이는 10자입니다.
- **숫자+대문자(최소길이:10)** : 숫자와 대문자의 조합으로만 구성된 ID를 발급합니다. 발급되는 비밀번호의 최소 길이는 10자입니다.
- **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 구성된 ID를 발급합니다. 발급되는 비밀번호의 최소 길이는 9자입니다.
- **소문자+대문자(최소길이:9)** : 소문자와 대문자의 조합으로만 구성된 ID를 발급합니다. 발급되는 비밀번호의 최소 길이는 9자입니다.

#### 검증
발급된 단말기 이전 키의 검증 조건을 설정합니다.
단말기 이전 키를 검증할 때 이전 횟수나 유효 기간, 실패 시 차단 등을 설정할 수 있습니다.
3. **단말기 이전 횟수**: 발급된 아이디의 단말기 이전이 가능한 횟수를 설정합니다. 무제한, 일회성 중 하나를 선택해야 합니다.
4. **유효 기간**: 발급된 계정의 유효 시간을 설정합니다. 발급된 단말기 이전 ID는 이 설정값의 영향을 받습니다. 무제한, 기간 설정 중 하나를 선택해야 합니다.
5. **실패 시 재검증 차단 여부**: 로그인을 시도하다 실패하면 특정 시간 동안 계정을 차단합니다. 선택하면 추가 설정 항목이 나타납니다.
6. **차단 기준 횟수**: **실패 시 재검증 차단 여부**를 선택하면 나타납니다. 입력한 횟수만큼 검증에 실패하면 계정이 차단됩니다. 1회 이상 설정해야 합니다.
7. **차단 기간**: 계정이 차단되고 얼마 후 다시 검증을 시도할 수 있는지 설정합니다. **영구 차단**, **기간 지정** 중 하나를 선택합니다. **기간 지정**을 선택하면 원하는 차단 시간과 분을 지정할 수 있습니다.

#### 초기 설정 완료 이후
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
최초 설정이 완료되면 게임 유저는 단말기 이전 기능의 비활성화만 가능하며 설정 변경이 필요할 경우 고객 센터에 문의하시기 바랍니다.
**사용 안함** 버튼을 클릭하여 기능을 비활성화할 수 있고 기존에 발급된 단말기 이전 키는 모두 삭제되기 때문에 활성화 이후에는 비활성화 여부를 신중하게 선택해야 합니다.

## Analytics indicator
Analytics에 지표를 쌓기위한 전송 지표를 확인 및 설정할 수 있습니다.
유저 레벨(INT)별, 월드/서버/채널별, 클래스/직업별 항목이 나누어져 있으며 유저 레벨의 경우는 실제 Analytics에 전송된 레벨 항목만 표시되고 월드/서버/채널별, 클래스/직업별 항목에서는 이 메뉴에서 등록된 항목들만 Analytics에 지표로 쌓이게 됩니다.
### 유저 레벨(INT)별
Analytics 시스템에 전송된 레벨 지표 항목을 확인할 수 있습니다.
이 항목에서는 별도의 수정항목이 없이 조회만 가능합니다.
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_01_202003.png)

### 월드/서버/채널별, 클래스/직업별 조회
현재 각 항목별로 설정되어 있는 전송 지표 항목을 확인할 수 있습니다.
조회화면에서는 설정된 항목들에 대한 지표를 쌓지 않고자 할 경우 삭제 버튼을 통하여기존에 등록된 항목에 대한 삭제가 가능합니다.
항목이 삭제되면 이후 **Analytics 메뉴에서 지표에 표시가 되지 않으며** 이후에는 삭제한 항목에 대한 지표가 쌓이지 않으므로 삭제 시 주의가 필요합니다.
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_02_202003.png)

### 월드/서버/채널별, 클래스/직업별 등록
Analytics 지표로 쌓고자 하는 정보를 새롭게 등록할 수 있습니다.
하단에 있는 추가 버튼을 이용해 등록할 수 있으며 **전체 항목 최대 100개**까지 신규로 등록이 가능합니다.
등록화면에서는 기존에 등록된 데이터들에 대하여 **지표 화면 표시 항목을 수정만을 제공**하며 삭제를 하고자 할 경우 다시 조회화면으로 이동하여 삭제를 진행해주셔야 합니다.
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_03_202003.png)

#### (1) ChannelId / ClassId : Analytics내에 쌓을 구분자 정보를 입력합니다. 지표를 쌓고자 할 떄 설정하실 ID정보를 입력하시면 됩니다.
#### (2) 지표 화면 표시 : 1번항목으로 입력한 ID로 전송된 지표를 화면에서 표시할 떄 보여주고자 하는 내용을 입력합니다. 해당정보는 기존에 등록된 지표항목들도 수정할 수 있습니다.
#### (3) 삭제 : 등록 화면에서는 새롭게 추가된 항목들만 삭제가 가능합니다.
