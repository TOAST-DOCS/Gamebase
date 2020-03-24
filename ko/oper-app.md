## Game > Gamebase > 콘솔 사용 가이드 > 앱

TOAST Console에서 **Game > Gamebase > App**을 클릭하여 앱의 기본 정보를 설정할 수 있습니다.

* **앱**: 앱 정보 관리
* **클라이언트**: 클라이언트 버전과 상태 정보 관리
* **설치 URL**: 앱의 스토어별 설치 URL 관리


## App


Gamebase 서비스를 활성화하면 자동으로 앱이 생성되며 해당 메뉴에서는 등록된 정보의 수정만 가능합니다.
TOAST 프로젝트 하나당 하나의 Gamebase 앱을 관리할 수 있으므로 앱을 추가로 등록하거나 삭제할 수는 없습니다. Gamebase 서비스를 비활성화 하시면 앱에 등록된 정보가 삭제됩니다.
각 항목별 상세 설명은 아래 **Properties** 항목을 참고합니다.

### Properties

![gamebase_ban_01_201812](./image/Operators_Guide/gamebase_app_01_202003_1.png)
![gamebase_ban_01_201812](./image/Operators_Guide/gamebase_app_01_202003_2.png)

#### (1) 설치 URL
앱 설치와 홍보에 이용할 수 있는 단축 URL 정보입니다.
앱이 배포된 스토어가 여러 개인 경우에도 하나의 단축 URL로 관리할 수 있습니다.
자세한 동작 및 관리 방법은 다음 링크를 참고합니다. [설치 URL 관리](./oper-app/#installed-url)

> [참고]
> Gamebase를 활성화하면 자동으로 생성되므로 변경은 불가능합니다.

####(2) 서버 주소
게임에서 게임 서버 주소(IP, URL 등)를 실시간으로 전달받아야 할 때 사용합니다.
서버 주소를 설정하면 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.
클라이언트의 상태에 따라 전달할 서버주소를 설정할 수 있습니다. 예를 들어 클라이언트의 상태가 테스트중이거나 심사중 일 경우 각 항목에 설정된 서버주소값이 최초 런칭정보에 내려가게 됩니다.
게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.

####(3) 고객 센터 정보
고객 센터 페이지 이외 이메일, 전화번호 등의 정보를 입력합니다.
고객 센터 페이지가 있는 경우에는 아래 **인앱 URL**의 **고객센터**에 정보를 입력합니다.
> [참고] <br/>
> 입력된 정보는 Gamebase가 제공하는 점검 상세 페이지에 표시됩니다.

####(4) 테스트 결제 포함 여부
기본 설정은 '테스트 결제 포함'으로 설정되어 있으며 '테스트 결제 제외'로 설정하면 Analytics 매출 지표에서 테스트 결제건은 모두 제외하여 보여줍니다.

####(5) 인증 정보
앱에서 로그인할 때 사용할 IdP의 인증 정보를 등록, 수정, 삭제할 수 있습니다.

외부 인증의 클라이언트 아이디, 비밀 키(secret key)뿐만 아니라 콜백 URL과 추가 정보를 설정할 수 있습니다.
인증 정보 옆에 **+** 버튼을 클릭하면 정보를 추가할 수 있으며 **-** 버튼을 클릭하면 정보를 삭제할 수 있습니다.
Idp별 자세한 설정 방법은 [Authentication Information](#authentication-information)을 참고하시기 바랍니다.
> [참고]
> **토큰 재검증**이란?
> 클라이언트에서 Latest Login API 호출 시에 외부 IdP의 토큰 재검증 여부를 설정합니다.
> **검증 안함**을 선택하면 외부 IdP의 토큰 재검증 없이 내부 토큰 검증만을 수행합니다.
> **항상 검증**을 선택하면 Gamebase에서 발급한 내부 토큰뿐만 아니라 외부 IdP 토큰에 대해서도 항상 유효성 검증을 진행합니다.


####(6) 인앱 URL
클라이언트를 다시 배포하지 않고 앱 내에서 자주 사용하는 URL을 Console을 통해 실시간으로 수정할 수 있습니다.

- 이용약관
- 개인 정보동의
- 이용 정지 규정
- 고객센터 URL

게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워 두시면 됩니다.
설정한 정보는 클라이언트 초기화 이후에 '런칭정보'에서 입력된 정보를 확인할 수 있습니다.

### Test Device

![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_02_201912.png)
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검 중이어도 정상적으로 게임에 접근할 수 있습니다.
테스트 단말기를 등록하려면 **Device Key** 또는 **IP** 정보를 등록해야 합니다. 직접 입력하거나 **게임유저 ID**를 조회하여 등록할 수 있습니다.
점검시 게임플레이가 가능할 수 있도록 하거나 단말기별 Debug Log 출력 여부를 설정하여 테스트 단말기를 관리할 수 있습니다.
더 이상 사용하지 않는 테스트 단말기를 삭제할 수도 있습니다.
접속 이력 확인버튼을 누르면 해당 기기를 통한 **점검이 진행되는 동안의 접속 시간 및 상세 접속 로그**를 확인할 수 있습니다.
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_09_201912.png)

> [참고]
> 테스트 단말기는 최대 100개까지만 등록할 수 있습니다.

#### (1) 조회

앱에 등록된 전체 테스트 단말기를 확인 할 수 있습니다. 검색어를 **Search**에 입력하여 검색 조건에 맞는 테스트 단말기를 찾아 볼 수 있습니다.

#### (2) 등록

조회 화면에서 **등록** 버튼을 클릭하면 테스트 단말기를 등록할 수 있는 화면이 나타납니다. **Device Key**를 직접 입력하거나 **게임유저 ID**를 검색해 테스트 단말기를 등록할 수 있습니다.

![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_10_201912.png)
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_11_201912.png)

**(1) 게임유저 ID를 통한 등록**

타입을 유저 ID로 선택하고 게임유저 ID를 입력하여 **검색** 버튼을 클릭하면 화면 하단에 사용자의 로그인 로그 내역이 조회됩니다. 조회된 내역에서 테스트 단말기로 등록하고자 하는 Device Key를 선택하여 **추가 정보**를 입력하여 **등록** 버튼을 클릭하면 해당 Device key가 테스트 단말기 정보로 등록됩니다.



**(2)Device Key 또는 IP를 통한 등록**

등록하고자 하는 Device key 또는 IP 정보를 알고 있을 경우 타입을 원하는 입력방식으로 선택하여 직접 테스트 단말기를 등록할 수 있습니다.
등록하고자 하는 단말기의 **단말기 이름** 및 디버그 로그, 점검 무시 여부를 입력 후 등록 버튼을 누르면 테스트 단말기로 등록됩니다.

> [참고]
> 단말기 이름에는 사용자가 알아보기 편한 별칭을 입력하시면 됩니다. 예시) iPhone 6 테스트, 토스트님 아이패드

#### (3) 삭제

![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_12_201912.png)

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
TOAST Console에서의 설정 외에 추가 설정은 없습니다.



#### 2. Google

##### Google Cloud Console

![gamebase_app_06_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_06_201812.png)

1. Google 인증을 위해서는 Google Cloud Console에서 **Web Application Client ID**를 발급받아 Gamebase Console에 입력해야 합니다.
2. 승인된 리디렉션 URI 란에 다음 값을 입력합니다.
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback

##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* GOOGLE의 경우, iOS 앱에서 필요한 정보 **url_scheme_ios_only**의 설정이 필요합니다.
		* **url_scheme_ios_only**의 값은 Xcode의 URL Scheme에 등록된 값들 중 한개와 일치해야 합니다.

	* URL Schemes를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**

* 1.12.2 이상
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.google`를 추가해야 합니다.

* GOOGLE 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Schemes" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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

##### Android & Unity
- AdditionalInfo를 설정해야 합니다.
    * **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
    * PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

* PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.

* 1.12.2 이상
	* AdditionalInfo를 설정해야 합니다.
		* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보** 항목에 JSON string 형태의 정보를 설정해야 합니다.
		* PAYCO의 경우, PaycoSDK에서 요구하는 **service_code**와 **service_name**을 설정해야 합니다.
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.payco`를 추가해야 합니다.

* PAYCO 추가 인증 정보 입력 예제

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 5.NAVER
Naver Developers 사이트에서 신청하여 발급받은 {client_id} 및 {client_secret}을 Gamebase Console에 입력합니다.
이때, 로그인 동의 창에서 표시할 애플리케이션 이름인 **service_name** 을 설정해야 하고, iOS 의 경우에는 추가로 **url_scheme_ios_only** 값을 JSON String 형태로 추가 정보란에 입력해야 합니다.

**입력 필드**

- Client ID: {NAVER client_id}
- Secret Key: {NAVER client_secret}
- 추가정보: NAVER Application Name & iOS url scheme (json format)

**Reference URL**<br />
- [NAVER Developers - 애플리케이션 등록](https://developers.naver.com/apps/#/register)
- [NAVER Developers - 클라이언트 아이디와 클라이언트 시크릿 확인](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Android & Unity
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야 합니다.
	* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

```json
{"service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[주의]</font><br/>
>
> Gamebase iOS SDK 1.12.2 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
>

* 1.12.1 이하
	* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.
		* iOS 앱에서 필요한 정보인 **url_scheme_ios_only**를 추가로 설정해야 합니다.

	* URL Schemes를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**

* 1.12.2 이상
	* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
		* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 `tcgb.{Bundle ID}.naver`를 추가해야 합니다.

* NAVER 추가 인증 정보 입력 예제

```json
{ "url_scheme_ios_only": "Your URL Schemes", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

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
 > 2019년 7월 25일부로 Twitter에서는 TLS 1.0, TLS 1.1 지원을 중단하고, TLS1.2만 지원하고 있습니다.
 > 이에, Android 4.3 (Jellybean, API Level 18) 이하의 단말기에서는 Android WebView를 통한 Twitter 로그인이 불가능합니다.
 >
 > 즉, Android 4.4 이상(KitKat, API Level 19)인 기기에서만 Twitter 로그인을 사용하실 수 있습니다.

##### iOS

 > <font color="red">[주의]</font><br/>
 >
 > Gamebase iOS SDK 1.14.0 버전에서 URL 스킴의 설정 방법이 변경되었습니다. 사용 SDK 버전에 맞는 가이드를 참고하시기 바랍니다.
 >

 * 1.13.0 이하
	* 별도로 URL 스킴을 설정하지 않아도 됩니다.

* 1.14.0 이상
	* URL 스킴을 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.

* Twitter 추가 인증 정보 입력 예제

![Twitter URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 7. LINE

**입력 필드**
- Client ID: {LINE Channel ID}
- Secret Key: {LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
LINE Login 기능을 사용하기 위하여, Xcode에 추가 설정이 필요합니다.
- URL Schemes를 설정해야 합니다.
	* **XCode > Target > Info > URL Types**에 **line3rdp.{App Bundle ID}**를 추가해야 합니다.

- Info.plist파일을 설정해야합니다.
	* LINE에서 발급받은 ChannelID를 설정합니다.
	```
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* ATS 설정을 위하여 scheme을 등록합니다.
	```
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
- LINE Login을 사용하기 위한 프로젝트 설정은 다음 링크를 참고합니다. (인증 필요)
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)


#### 8. Sign In with Apple
Sign In with Apple 기능을 사용하기 위하여, AppStore Connect, Gamebase Console 그리고 Xcode 에 설정이 필요합니다.

##### AppStore Connect Settings
* [Certificates, Identifiers & Profiles \> Keys 로 바로가기](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > 추가(+)
1. `Sign In with Apple` 체크박스를 선택하고 설정을 진행합니다.
![Check SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. `Sign in with Apple` 을 사용할 Bundle ID 를 선택합니다.
![ChooseAPrimaryAppID](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. <span style="color:#e11d21">Privatekey</span>를 다운로드 후 보관, 생성된 <span style="color:#e11d21">Key ID를 </span> 확인합니다.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Certificates, Identifiers & Profiles > Identifiers > 대상 앱을 선택 > `Sign In with Apple`을 활성화합니다.
    * `Enable as a primary App ID` 로 설정합니다.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[TOAST Console 바로가기](https://console.toast.com/)

* Gamebase
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid4_1.0.png)


###### Client ID Settings
> 앱의 Bundle ID를 설정합니다.

###### Secret Key Settings
> Apple Developer Account 설정에서 획득한 값(**TeamID**, **KeyID**, **PrivateKey**)으로 JSON 문자열을 생성해서 설정합니다.

* `teamId` : 개발자 계정 우측상단의 값을 설정합니다.
* `keyId` : Certificates, Identifiers & Profiles > Keys > Sign In with Apple 을 체크하여 생성된 값을 설정합니다.
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid5_1.0.png)
* `privateKey` : 위의 Keys에서 키를 생성하면서 같이 생성된 PrivateKey 파일의 내용을 설정합니다. (다운로드한 파일을 열어서 아래 스크린샷과 같이 빨간 사각형 부분의 값을 사용합니다.)
![SecretKey설정](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

위의 값을 아래의 예제와 같이 JSON 으로 만들어서 설정합니다.


```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. 중략"
}
```

###### Additional Info Settings
[Sign In with Apple 의 AuthorizationScope 에 대해서 알아보기](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

Gamebase Console > App 에서 Apple 을 추가하게 되면 기본 값으로 아래의 JSON 값이 설정됩니다.
현재 (2019.11) 기준으로는 Scope의 종류가 `full_name`, `email` 만 존재하며, Gamebase 에서는 이 두 가지 값을 기본값으로 설정합니다.

```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[주의]</font><br/>
> Xcode 11 이상에서만 'Sign In with Apple` 기능을 사용하는 프로젝트를 빌드할 수 있습니다.


1. Target 선택 > Signing & Capabilities > Sign In with Apple 항목을 추가합니다.
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. Target 선택 > Build Phases > Link Binary With Libraries > Authentication.framework 를 **Optional** 로 추가합니다.
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)
    - ```주의```: Optional이 아닌 Required 로 설정되어 있을 경우에는 iOS 11 이하의 단말기에서는 앱 실행 시, runtime crash 가 발생합니다.



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
![gamebase_app_13_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_201901.png)
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
> 서비스 상태를 선택하면 각 상태에 맞는 기본 메시지가 5개(한국어, 영어, 일본어, 중국어 간체, 중국어 번체)의 언어로 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.
> ![gamebase_app_18_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_18_201812.png)

#### (4) 서버 주소
클라이언트에서 이용할 서버 주소(IP, URL)를 입력합니다.
**앱** 탭에서 서버 주소를 입력하면 모든 클라이언트에 적용되므로, 클라이언트마다 다른 서버 주소를 사용하고 싶을 때만 서버 주소를 입력합니다.

#### (5) Debug log
Gamebae SDK의 Debug Log 출력 여부를 콘솔에서 실시간으로 변경할 수 있습니다.
설정되어 있지 않으면 기본적으로 Gamebase SDK 내부에 설정된 값을 우선으로 동작하고 Gamebase 콘솔에서 Debug Log 출력 여부를 설정할 수 있습니다.
Gamebase SDK에 Debug Log가 'OFF' 상태이더라도 콘솔에서 'ON'으로 설정하면 단말기에 Gamebase Debug Log가 출력됩니다.

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
