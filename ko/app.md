## Game > Gamebase > Operator Guide > App
앱의 기본적인 정보를 설정할 수 있는 메뉴입니다. 운영을 위한 클라이언트 버전별 상태 관리 및 설치 URL 설정이 가능합니다.

## App

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)
- Gamebaes 상품을 활성화하면 자동으로 앱이 생성되며 해당 메뉴에서는 등록된 정보의 수정만 가능합니다.
- TOAST Cloud 프로젝트 하나당 하나의 Gamebase 앱을 관리할 수 있으므로 앱의 추가 등록 및 삭제는 불가능 합니다. 앱을 삭제하고 싶은 경우에는 Gamebase 상품을 비활성화 하시면 됩니다.<br />
- 각 항목별 상세 설명은 다음 링크를 참고합니다. LINK [[App's Properties](./app/#app/#Properties)]<br />

### Properties

Gamebase Console에서 관리하는 App 정보를 설명합니다.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.1.png)

####(1) 설치 URL
앱 설치 및 홍보에 이용할 수 있는 단축URL 정보입니다. <br/>
앱이 배포된 스토어가 여러개인 경우에도 하나의 단축URL로 관리가 가능합니다. <br/>
자세한 동작 및 관리 방법은 다음 링크를 참고합니다. LINK [[Installed URL](./app/#installed-url)]<br />

> [INFO]
>
> Gamebase 활성화 시 자동으로 생성되므로 변경은 불가능합니다.<br />

####(2) 서버 주소
게임서버 주소(ip, url 등)를 실시간으로 게임에서 전달받아야 하는 경우 사용합니다.<br/>
서버주소를 설정하면 <span style="color:red;">`Client 초기화 이후에 런칭정보`</span>에서 입력된 정보를 확인 할 수 있습니다.<br/>
게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워두시면 됩니다.<br />
  
####(3) 고객센터 정보
고객센터페이지 이외의 고객센터 정보(e-mail, 전화번호 등)를 입력합니다.<br />
고객센터페이지가 있는 경우에는 아래 인앱URL의 고객센터URL에 정보를 입력합니다.<br />
> [INFO]
>
> 입력된 정보는 Gamebase가 제공하는 점검상세페이지에 노출됩니다.<br />

####(4) 인증 정보
앱에서 로그인 시 사용할 IdP의 인증정보를 등록/수정/삭제할 수 있습니다.<br />
외부 인증의 Client ID, Secret Key 뿐만 아니라 Callback URL과 추가 정보를 설정할 수 있습니다. <br/>
Idp별 자세한 설정방법은 다음 링크를 참고합니다. LINK [[App's Authentication Information](./app/#Authentication-Information)]<br />
> [INFO]
>
> <span style="color:red;">`토큰재검증`</span> 이란?
> <span style="color:red;">`Latest Login API`</span> 호출시에 외부 IDP의 토큰 재검증여부를 설정합니다. <br/>'검증안함'으로 설정하면 외부 IDP의 토큰 재검증 없이 내부 토큰 검증만을 수행합니다. <br/>'항상검증'으로 설정하면 Gamebase에서 발급한 내부토큰 뿐만 아니라 외부 IDP 토큰에 대해서도 항상 유효성 검증을 진행합니다.


####(5) 인앱URL
앱 내에서 자주 사용하는 URL을 client 재배포없이 console을 통해 실시간으로 수정이 가능합니다.<br/>
- 이용약관
- 개인 정보동의
- 처벌규정
- 고객센터의 URL
 
게임에서 필요한 경우에만 입력하고, 그렇지 않은 경우에는 비워두시면 됩니다.<br />


####(6) 테스트 단말기
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검중이여도 정상적으로 게임 접근이 가능합니다.<br />
테스트 단말기 등록을 위하여 `Device Key`를 입력하거나 `User ID`를 입력합니다. <br/>
`User ID` 입력은 편의성을 위하여 제공하며 `User ID`를 입력하면 가장 최근 로그인한 이력의 Device Key를 찾아 자동으로 등록합니다. 만약, 등록 이후 테스트 단말기를 변경하였다면 다시 단말기를 등록해 주어야 합니다.

### Authentication Information

#### Facebook
페이스북 개발자 사이트에 등록한 어플리케이션의 `{앱 아이디}`와 `{앱 시크릿 코드}`를 TOAST Cloud Gamebase 콘솔에 입력합니다.
이 때, 로그인 시 필요한 `{Facebook Permission}` 또한 JSON String 형태로 추가정보란에 입력해주어야합니다.

##### 입력 필드
- ClientID : {AppID}
- Secret Key : {App Secret Code}
- 추가정보 : Facebook Permission (json format)


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

##### Reference URL
- LINK [[페이스북 개발자 사이트](https://developers.facebook.com/)]
- LINK [[페이스북 권한](https://developers.facebook.com/docs/facebook-login/permissions/)]


##### [예시] facebook_permission format
```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"] }
```

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

#### Google
구글 개발자 콘솔에 등록된 어플리케이션의 `{클라이언트 ID}` 및 `{클라이언트 보안 비밀}`, `{리다이렉션 URI}`를 TOAST Cloud Gamebase 콘솔에 입력합니다.

##### 입력 필드
- ClientID : {클라이언트 ID}
- Secret Key : {클라이언트 보안 비밀}
- Callback URL : {리다이렉션 URI}

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_permission_1.0.png)

#### Apple GameCenter
Apple 개발자사이트에 등록된 BundleID를 TOAST Cloud Gamebase 콘솔에 입력합니다.

##### 입력 필드
- ClientID : {Bundle ID}

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

##### Reference URL
- LINK [[Apple 개발자 사이트](https://developer.apple.com/)]
- LINK [[Apple Itunesconnect](https://itunesconnect.apple.com/)]

#### PAYCO
페이코 Client ID 신청을 통해 발급받은 `{client_id}` 및 `{client_secret}`을 TOAST Cloud Gamebase 콘솔에 입력합니다.

##### 입력 필드
- ClientID : {Payco client_id}
- Secret Key : {Payco client_secret}





## Client

클라이언트의 정보를 `OS`(iOS, Android, Unity WebGL, Unity Standalone), `Versiton`별로 관리 할 수 있습니다.<br />

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
- 현재 등록된 클라이언트 리스트를 확인할 수 있습니다.<br />
- OS별로 그룹핑되어 보여집니다.<br />
- 서비스 상태별로 다른 색깔로 표시됩니다.<br />
	- 테스트
	- 심사중
	- 서비스중
	- 서비스중(업데이트권장)
	- 업데이트 필수
	- 종료
- icon 내의 숫자는 클라이언트 등록시 입력한 버전을 의미합니다.<br />
- icon list는 서비스 상태가 `테스트`,`심사중`,`서비스중`,`서비스중(업데이트권장)`인 목록만 보여집니다. OS별 하단 오른쪽의 화살표를 클릭하면`업데이트필수`,`종료`상태의 클라이언트 목록을 확인할 수 있습니다.<br />

### Properties

Gamebase Console에서 관리하는 Client 등록 정보를 설명합니다.<br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) 스토어
<span style="color:red;">`필수`</span>Client를 배포할 스토어를 선택합니다. <br/>
OS별로 선택 가능한 스토어가 다릅니다.<br />
#### (2) 게임 버전
<span style="color:red;">`필수`</span>Client의 버전을 입력합니다. <br/>
각 게임별로 정해진 규칙에 따라 문자열로 입력해주시면 됩니다.<br />
#### (3) 서비스 상태
<span style="color:red;">`필수`</span>Client의 서비스 상태를 선택합니다.<br />
- 테스트 : 내부 테스트<br />
- 심사중 : 스토어 심사 중. <br/> 안정화 지표를 추가로 설정할 수 있습니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
> [INFO]
>
> `안정화 지표`란?
> Gamebase 내부에서 Gamebase API 호출시에 남기는 지표로그입니다. <br/>
> 서비스상태가 `심사중`일 때에 안정화 지표를 활성화하면 심사중에 Gamebase내부에서 문제가 발생한 경우 문제 확인이 보다 쉬워집니다. <br />
> Gamebase Console에서 실시간으로 안정화 지표 사용 여부와 Log Level을 설정 할 수 있습니다.<br />

- 서비스중 : 정상 서비스<br />
- 서비스중(업데이트 권장) : 정상 서비스. <br/>보다 안정적인 버전으로 유도하기 위한 팝업 노출 후 서비스 진입하도록 합니다. 새로운 버전을 다운로드 받아 이용하도록 유도하지만 사용자가 원하는 경우 현재 버전으로도 계속 서비스를 이용할 수 있습니다.<br />
- 업데이트 필수 : 서비스 불가능. <br/>현재 게임에서 서비스 지원하지 않는 버전으로 최신버전 설치 팝업을 노출합니다.<br />
>  [WARNING] 
>  
>  업데이트 필수 상태 변경 시 주의사항<br/>
> **업데이트 필수와 점검이 동시에 설정**되어 있을 경우 서비스 상태는 `업데이트 필수`로 Client에 내려가게 됩니다.<br/>
> 점검 진행 도중 사용자에게 업데이트 필수 팝업 노출을 원하지 않는 경우에는 점검 완료 이후에 서비스 상태를 `업데이트 필수`로 변경해야 합니다.<br/>

- 종료 : 서비스 불가능. <br/> 서비스가 종료된 버전인 경우 선택합니다.<br />

> [INFO]
>
> 서비스 상태별 노출 메시지 설정
> `서비스중(업데이트 권장)`,`업데이트 필수`,'종료` 상태인 경우 사용자에게 노출할 안내메시지를 다국어로 설정할 수 있습니다. <br />
>  서비스 상태를 선택하면 각 상태에 맞는 기본 메시지가 5개(한국어, 영어, 일본어, 중국어(간체), 중국어(번체))의 언어로 제공되며 원하는 경우 언어를 추가하거나 기본 메시지의 문구를 변경할 수 있습니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)  

#### (4) 서버 주소
Client에서 이용할 서버 주소(ip, url)를 입력합니다.<br />
앱 메뉴에서 서버 주소를 입력하면 모든 Client에 적용이 되므로 만약 Client마다 다른 서버 주소를 사용하고자 하는 경우에만 서버주소를 입력합니다. <br />


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.1.png)

게임 설치를 위한 스토어 URL정보를 관리합니다.<br/>
사용자가 PC나 모바일에서 단축URL을 클릭하면, 사용자 단말기 정보(device, OS, store등)를 이용하여 입력된 사이트로 Redirection 합니다.<br/>
스토어 정보가 없거나 스토어 이동 실패시 `COMMON`에 설정된 URL로 이동합니다.<br/>

[예시1] Android 전화기에서 문자로 받은 설치 URL을 클릭하는 경우<br/>
(Device:mobile,OS:Android,Store:없음) Android중에 대표스토어로 지정된 Mobile URL로 이동. 대표스토어가 'Google Play'인 경우 'Google Play' Mobile에 설정된 URL로 이동.<br/>
[예시2] 'One Store'에서 다운받은 앱으로 게임 중이던 사용자가 '업데이트 필수' 팝업창에서 '지금 업데이트' 버튼을 클릭한 경우<br/>
(Device:mobile,OS:Android,Store:One Store) 'One Store' Mobile에 설정된 URL로 이동(One Store 모바일 설치 페이지)<br/>
[예시3] PC에서 설치 URL을 입력한 경우<br/>
(Device:PC,OS:Windows,Store:없음) COMMON PC에 설정된 URL로 이동<br/>


### Properties
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.1.png)
- 각 항목은 PC, Mobile별로 따로 설정이 가능합니다. PC와 모바일 구분이 필요 없을 경우 동일한 값을 각각 입력하면 됩니다.<br />
#### (1) Common
스토어 정보가 없거나 스토어 이동 실패시 연결될 주소를 설정합니다.<br />
#### (2) Android
Android를 사용하는 유저들의 요청일 때 연결될 주소를 설정합니다.<br />
원하는 스토어가 목록에 노출되지 않을 경우, 담당자에게 요청 주시면 해당 스토어에 대한 추가가 가능합니다.<br />
#### (3) iOS
App Store를 사용하는 유저들의 요청일 때 연결될 주소를 설정합니다.<br />
#### (4) WebGL
WebGL을 내부에서 연결될 주소를 설정합니다.<br />
#### (5) Standalone
Standalone으로 서비스 되는 앱에서 연결될 주소를 설정합니다.<br />
