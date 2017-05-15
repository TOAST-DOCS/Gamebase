## Upcoming Products > Gamebase > Operator's Guide

Toast Cloud Gamebase Console Guide입니다.
## 모니터링(Monitoring)
게임을 이용하는 사용자의 현황을 지표 및 그래프로 확인할 수 있습니다.<br />
모니터링 / 그룹동접 / 설치 URL 통계 메뉴로 구성되어 있습니다.<br />
각각의 메뉴는 다음과 같은 지표 및 그래프를 제공합니다.<br />
### 모니터링
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Monitoring1_1.0.png)
현재 게임을 이용하는 유저의 전체 통계를 제공합니다.

### 그룹동접
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_ConcurrentUser1_1.0.png)
자신이 속한 프로젝트의 그룹동접 통계를 제공합니다.

### 설치 URL 통계
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_InstallUrl1_1.0.png)
설치 URL을 통해 게임을 설치한 유저의 통계를 제공합니다.

## 앱(App)
게임 앱에 관련된 기본적인 설정 및 클라이언트, 설치URL에 관련된 설정을 조회/등록/수정할 수 있습니다.
### 앱
#### 1) 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.0.png)
- 게임에 대한 전반적인 설정을 조회할 수 있습니다.<br />
- 앱 메뉴는 최초 Gamebase 활성화시 생성되는 정보로써, 수정만 가능하며 등록/삭제는 불가합니다.<br />
- 각 항목별 상세 설명은 아래 수정화면을 참고 바랍니다.<br />

#### 2) 수정
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.0.png)
**(1) 서버 주소**<br />
  게임서버의 URL을 설정할 수 있습니다.<br />
**(2) 설치URL**<br />
  게임 설치 및 홍보에 이용할 수 있는 단축URL 정보입니다. Gamebase 활성화 시 자동으로 생성되므로 변경은 불가능합니다.<br />
**(3) 인앱URL**<br />
  각 게임 내에서 필요한 이용약관/개인 정보동의/처벌규정/고객센터의 URL을 설정할 수 있습니다.<br />
**(4) 인증 정보**<br />
  게임에서 로그인 시 사용할 IdP의 인증정보를 등록/수정/삭제할 수 있습니다.<br />
  각 인증정보에 대한 추가 정보, Callback URL을 설정할 수 있으며, 클라이언트에서 로그인 시 토큰에 대한 재검증 여부도 함께 설정할 수 있습니다.<br />
**(5) 테스트 단말기**<br />
  게임 QA를 위한 단말기 정보를 등록합니다.<br />
  해당 정보가 등록된 단말기는 Gamebase를 사용하는 앱이 점검중이여도 정상적으로 게임에 대한 접근이 가능합니다.<br />
  User ID를 통해 등록할 경우 가장 최근 로그인한 이력의 Device Key를 찾아 등록하며, 직접 Device Key를 등록할 수도 있습니다.<br />

### 클라이언트<br />
게임에 대한 설정을 사용하는 클라이언트에 대한 등록/수정/삭제를 진행할 수 있습니다.<br />
OS별로 등록이 가능하며, 각 스토어에 맞는 클라이언트를 선택하여 등록할 수 있습니다.<br />
#### 1) 리스트 조회<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.0.png)
- 등록한 클라이언트들에 대한 리스트를 조회할 수 있습니다.<br />
- 숫자들은 클라이언트의 버전을 의미하며, 아이콘 리스트의 경우 현재 클라이언트의 서비스 상태가 **테스트/심사중/서비스중/서비스중(업데이트 권장)**인 목록만 조회됩니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client2_1.0.png)
- **업데이트 권장/종료**상태의 목록을 조회하려면 오른쪽의 화살표를 클릭하시면 위와 같이 조회가능합니다.<br />

#### 2) 상세 조회<br />
**(1) 테스트/서비스**<br />
  클라이언트 등록시 선택한 스토어/게임버전/상태/서버주소에 대한 정보를 보여줍니다.<br />
  가장 기본적인 클라이언트 조회 화면입니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client3_1.0.png)

**(2) 심사중**<br />
  안정화 지표에 대한 설정을 추가로 할 수 있습니다.<br />
  안정화 지표를 추가하게 되면, 해당 게임에서 Gamebase를 사용하여 처리되는 부분에 대한 로그를 모두 확인하실 수 있습니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.0.png)

**(3) 업데이트 권장(서비스중)/업데이트 필수/종료**<br />
  각 클라이언트 상태별 국가별 메시지를 설정하여 유저가 사용하는 클라이언트에 대한 안내문을 제공할 수 있습니다.<br />
  서비스 상태를 설정하면, 각 상태에 맞는 기본 메시지를 지원합니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.0.png)

#### 3) 등록/수정<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
**(1) 스토어**<br />
  각 OS별 제공하는 스토어를 선택합니다.<br />
**(2) 게임 버전**<br />
  해당 클라이언트의 버전을 입력합니다. 버저닝은 각 게임별로 정해진 규칙에 따라 입력해주시면 됩니다.<br />
**(3) 서비스 상태**<br />
  등록할 클라이언트의 최초 상태 또는 등록된 클라이언트의 변경할 상태를 선택합니다.<br />
  **심사중** 상태를 선택할 경우 안정화 지표를 추가로 설정할 수 있습니다.<br />
**(4) 서버 주소**<br />
  클라이언트에서 이용할 서버 주소를 입력합니다.<br />
  앱 메뉴에서 설정해놓은 기본 서버주소를 참고사항으로 보여주고 있으며, 각 클라이언트별로 상이한 서버 주소를 사용할 경우 해당 주소를 입력하면 됩니다.<br />

### 설치 URL<br />
#### 1) 조회<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.0.png)
- Gamebase 최초 활성화시 발급된 설치URL에 연결될 각 스토어 별 설치 주소를 설정할 수 있습니다.<br />
- COMMON, iOS, Android 카테고리가 존재하며, Common으로 설정한 값의 경우 iOS, Android에도 속하지 않은 OS의 요청일 경우 연결되는 주소입니다.<br />
<br />
#### 2) 수정<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.0.png)
- 각 항목은 PC, Mobile별로 따로 설정이 가능합니다.<br />
**(1) Common**<br />
  iOS, Android에도 속하지 않을 경우의 요청일 때 연결될 주소를 설정합니다.<br />
**(2) iOS**<br />
  App Store를 사용하는 유저들의 요청일 때 연결될 주소를 설정합니다.<br />
**(3) Android**<br />
  Android 스토어를 사용하는 유저들의 요청일 때 연결될 주소를 설정합니다.<br />
  원하는 스토어가 목록에 노출되지 않을 경우, 담당자에게 요청 주시면 해당 스토어에 대한 추가가 가능합니다.<br />

## 점검
게임이 업데이트를 진행하거나 긴급 수정사항이 필요할 경우, 점검을 걸어 유저들의 진입을 제한할 수 있습니다.
### 점검
#### 1) 점검 리스트 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance1_1.0.png)
- 게임에서 등록한 점검 내역을 조회할 수 있습니다.<br />
- 수행한 점검내역에 대한 검색 및 현재 점검의 진행상태 등을 확인할 수 있습니다.<br />
- 진행상태는 **예약 중/점검 중/점검 해제/종료/점검 해제(기한 만료)**로 구분되어 있습니다.<br />
- 현재 진행중인 점검을 해제하게 되면 상태는 **점검 해제**상태로 변경되며, 다시 점검을 활성화하지 않은 상태로 기한이 만료되면 **점검 해제(기한 만료)**상태로 변경됩니다.<br />

#### 2) 점검 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_1.1.png)
- 점검 등록을 수행하는 화면입니다.<br />
**(1) 대상**<br />
  점검 대상에 대한 선택을 하는 항목입니다.<br />
  **전체 게임/일부 클라이언트**가 선택이 가능하며, 클라이언트의 경우 앱-클라이언트 메뉴에서 등록한 클라이언트 버전리스트가 출력됩니다.<br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.0.png)
  클라이언트 버전을 선택 시, 클라이언트 상태별 및 스토어별 전체 선택이 가능하며, 개별로 점검을 거실 경우 원하시는 클라이언트 버전을 선택 후 확인 버튼을 누르시면 됩니다.<br />
**(2) 점검 시간**<br />
  점검이 수행될 시간을 설정할 수 있습니다.<br />
  Timezone의 경우 기본적으로 UTC+09:09이 선택되어 있으며, 서비스하시는 국가에 따라 해당 국가시간대를 선택하여 점검을 등록하는 것도 가능합니다.<br />
**(3) 점검 사유**<br />
  점검이 활성화 되었을 때 유저들이 볼 수 있는 메시지를 설정합니다.<br />
  각 언어별로 오른쪽의 **+**버튼을 통해 추가가 가능하며, 원하시는 언어가 없으실 경우 담당자에게 연락을 주시면 새로운 언어도 추가가 가능합니다.<br />
**(4) 점검 페이지 URL**<br />
  유저에게 노출할 점검 페이지를 설정합니다.<br />
  - 사용하지 않음 : Gamebase에서 제공하는 기본 점검페이지를 사용합니다.<br />
  - 사용 : 게임에서 점검에서 사용하기 위해 만든 점검페이지 URL 입력을 하면 점검시에 유저에게 노출합니다.<br />


#### 3) 점검 상세내용 조회/수정/삭제
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance3_1.0.png)
- 등록한 점검에 대한 상세내용을 확인 및 수정/삭제를 진행할 수 있습니다.<br />
- 기본적으로 입력이 가능한 항목은 등록 화면과 동일하며, 점검을 잘못 등록했을 시 삭제버튼을 통하여 점검 삭제도 가능합니다.<br />

## 공지
유저가 접속했을 때 게임의 중요한 업데이트 및 이벤트 내용을 알릴 수 있도록 공지사항 기능을 제공합니다.
### 공지

#### 1) 공지 리스트 조회

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice1_1.0.png)
- 게임에서 등록한 공지사항에 대한 내역을 조회할 수 있습니다.<br />
- 수행한 공지사항에 대하여 검색 및 현재 공지사항의 진행상태 등을 확인할 수 있습니다.<br />
- 진행상태는 **예정/노출 중/완료**로 구분되어 있습니다.<br />

#### 2) 공지 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice2_1.0.png)
- 공지 등록을 수행하는 화면입니다.<br />
**(1) 대상**<br />
  공지를 노출할 대상에 대한 선택을 하는 항목입니다.<br />
  **전체 게임/일부 클라이언트**가 선택이 가능하며, 클라이언트의 경우 앱-클라이언트 메뉴에서 등록한 클라이언트 버전리스트가 출력됩니다.<br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.0.png)
  클라이언트 버전을 선택 시, 클라이언트 상태별 및 스토어별 전체 선택이 가능하며, 개별로 점검을 거실 경우 원하시는 클라이언트 버전을 선택 후 확인 버튼을 누르시면 됩니다.<br />
**(2) 대상 국가**<br />
  공지를 노출할 국가를 선택할 수 있습니다.<br />
  전체의 경우 게임에 등록된 모든 유저들에게 공지가 노출되며, 국가를 선택한 경우 최우선적으로 **USIM 코드**의 국가기준으로 공지를 노출하며, USIM이 없을 경우 **Device**에 설정되어 있는 국가를 기준으로 공지가 노출됩니다.<br />
**(3) 노출 횟수**<br />
  공지가 노출될 횟수를 선택할 수 있습니다.<br />
  **노출 기간 중 1회 노출**을 선택할 경우 주어진 기간 내에 1번만 해당 공지를 노출하며, **앱 시작할 때마다 항상 노출**을 선택하게 되면 주어진 기간 동안 유저가 로그인이 완료된 후 항상 공지사항을 노출하게 됩니다.<br />
**(4) 노출 시간**<br />
  공지가 노출될 시간을 설정할 수 있습니다.<br />
  Timezone의 경우 기본적으로 UTC+09:09이 선택되어 있으며, 서비스하시는 국가에 따라 해당 국가시간대를 선택하여 점검을 등록하는 것도 가능합니다.<br />
**(5) 노출 메시지**<br />
  공지가 보여질 때의 내용을 입력하는 부분입니다.<br />
  각 언어별로 오른쪽의 **+**버튼을 통해 추가가 가능하며, 원하시는 언어가 없으실 경우 담당자에게 연락을 주시면 새로운 언어도 추가가 가능합니다.<br />
**(6) 하단 버튼 타입**<br />
  공지사항 하단에 노출될 버튼의 타입을 지정할 수 있습니다.<br />
  **닫기**를 선택하게 되면 공지사항의 닫기버튼을 누를 경우 해당 팝업이 사라지게 되고, **닫기+자세히 보기**를 선택하게 되면 해당 공지사항에 연결된 링크를 Gamebase 클라이언트에 제공하는 웹뷰 또는 자체적으로 구현하신 웹뷰에 띄울 수 있도록 링크를 전달해 드립니다.<br />

#### 3) 공지 상세내용 조회/수정/삭제
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice3_1.0.png)
- 등록한 공지에 대한 상세내용을 확인 및 수정/삭제를 진행할 수 있습니다.<br />
- 기본적으로 입력이 가능한 항목은 등록 화면과 동일하며, 공지를 잘못 등록했을 시 삭제버튼을 통하여 삭제도 가능합니다.<br />

## 푸쉬
유저의 인입/게임의 새로운 정보 알림을 돕기 위한 푸쉬 기능을 제공합니다.
### 푸쉬
#### 1) 푸쉬 리스트 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push1_1.0.png)
- 게임에서 발송한 푸쉬 내역 및 발송 예정 내역을 조회할 수 있습니다.<br />
- 발송 예정 내역에 있는 리스트들은 상세조회를 통해 예약 전송을 취소할 수 있습니다.<br />

#### 2) 푸쉬 상세 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push2_1.0.png)
- 푸쉬 리스트에서 선택한 푸쉬의 상세 발송 내역을 조회할 수 있습니다.<br />
- 예약발송을 잘못 발송했을 경우, 현재는 발송 취소만 가능하며 수정 기능은 추후 제공될 예정입니다.<br />

#### 3) 푸쉬 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push3_1.0.png)
**(1) 메시지 타입**<br />
  메시지 타입은 **홍보성/정보성**을 지원하며, **홍보성**의 경우 연락처와 수신철회방법을 함께 입력해주어야 전송이 가능합니다.<br />
  **정보성**의 경우 연락처와 수신철회방법 입력 없이 전송이 가능합니다.<br />
  정보통신망보호법상 기준으로 국내에 발송할 푸쉬는 **홍보성**을 이용하여 발송하여야 하며, 외국 유저에게 푸쉬를 보낼 경우 **정보성**을 이용하여 발송이 가능합니다.<br />
**(2) 발송 대상**<br />
  발송 대상은 **전체 발송/특정회원 발송/그룹 발송**을 지원합니다.<br />
  전체발송의 경우 OS별로 선택이 가능하며, 선택한 OS를 사용하는 유저에게 모두 푸쉬를 발송하게 됩니다.<br />
  특정회원의 경우 발송하고자하는 유저의 ID를 입력하여 특정 유저에게만 푸쉬를 발송할 수 있도록 제공하는 기능입니다.<br />
  그룹발송의 경우 파일을 통해 발송 대상자를 선택할 수 있으며, 그룹발송 예시 파일을 통해 해당 형식으로 파일을 업로드하면 해당 유저들에게 푸쉬를 전송합니다. <br />
  그룹발송은 최대 10000명까지 가능합니다.<br />
**(3) 발송 타입**<br />
  발송 타입은 **즉시 발송/예약 발송**을 지원합니다.<br />
  즉시발송의 경우 등록 즉시 푸쉬가 발송되며, 예약 발송의 경우 한국 시간 기준으로 정해진 시간에 푸쉬를 발송하게 됩니다.<br />
  Timezone의 경우 아직 지원하지 않으며, 추후 지원될 예정입니다.<br />
**(4) 대상 국가**<br />
  대상 국가의 경우 **전체/일부 국가**를 지원하며 일부 국가의 발송대상은 1순위로 USIM코드의 국가정보를 따르며 2순위로 Device에 설정된 국가정보를 따르게 됩니다.<br />
**(5) 발송 메시지**<br />
  언어별로 푸쉬 메시지에 대한 설정이 가능합니다. 국가와 마찬가지로 1순위로 USIN코드의 언어정보를 따르며, 2순위로 Device에 설정된 언어정보를 따르게 됩니다.<br />
  
## 회원
### 회원
게임을 이용하는 회원에 대한 조회 기능을 제공합니다.

#### 1) 회원 기본정보 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_1.1.png)
- 회원에 대한 기본정보를 조회할 수 있습니다.<br />
- 유저 ID를 통해 회원정보를 조회할 수 있으며, 최초 조회시 기본정보 및 Login History를 함께 조회합니다.<br />
- 하위 탭으로 **Login History/Mapping History/Widthdraw History/Playtime**이 존재하며, 각각의 탭은 기본 회원정보를 조회한 뒤에 조회가 가능합니다.<br />

#### 2) Login History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_LoginHistory1_1.0.png)
- 유저의 로그인 내역을 조회할 수 있습니다.<br />
- 원하시는 날짜를 입력하여 조회가 가능합니다.<br />
- 조회가 가능한 최대 날짜는 3개월(90일)입니다.<br />

#### 3) Mapping History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_1.0.png)
- 유저 계정의 매핑 이력을 조회할 수 있습니다.<br />
- **유저ID/IDP**기준으로 조회가 가능합니다.<br />
- 조회 결과는 최근 3개월(90일)이내의 이력만 조회됩니다.<br />

#### 4) Widthdraw History
- 준비중입니다.

#### 5) Playtime
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Playtime1_1.1.png)
- 유저가 게임을 플레이한 시간을 조회할 수 있습니다.<br />
- 원하시는 날짜를 입력하여 조회가 가능합니다.<br />
- 조회가 가능한 최대 날짜는 1개월(30일)입니다.<br />

## 유료
### 앱
게임 내에서 상품을 팔기 위한 스토어에 대한 등록을 제공합니다.

#### 1) 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)
- 게임에서 사용하기 위해 등록한 스토어들에 대한 정보를 조회할 수 있습니다.

#### 2) 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)
- 상품을 등록하기 위한 스토어를 등록할 수 있습니다.<br />
**(1) 스토어**<br />
  등록하고자 하는 외부 스토어를 선택합니다.<br />
  등록하시고자 하는 스토어가 없으실 경우, 담당자에게 연락주시면 새로운 스토어에 대한 추가가 가능합니다.<br />
**(2) APP 이름**<br />
  스토어에서 발급 받으신 정보를 입력합니다.<br />
**(3) 스토어 App ID**<br />
  등록하고자 하는 게임의 이름을 입력합니다.<br />
**(4) 사용 여부**<br />
  해당 스토어를 사용할 지 말지에 대한 여부를 선택합니다.<br />

#### 3) 조회/수정/삭제
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- 조회 리스트에서 각 스토어를 선택하면 등록된 상세정보를 조회하실 수 있습니다.<br />
- 수정버튼을 누르면 스토어 입력정보를 제외한 나머지 정보를 변경하실 수 있습니다.<br />
- 삭제 버튼을 통해 미사용 스토어의 정보는 삭제하실 수 있습니다.<br />

### 아이템
각 스토어에서 판매할 아이템을 등록할 수 있습니다.

#### 1) 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)
- 현재 스토어에 등록된 아이템 리스트를 조회할 수 있습니다.<br />
- 기본적으로는 모든 스토어에 대한 아이템을 노출하며, 각 스토어별 필터링 기능도 제공합니다.<br />

#### 2) 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)
- 상품을 등록하기 위한 스토어를 등록할 수 있습니다.<br />
**(1) 스토어**<br />
  등록하고자 하는 외부 스토어를 선택합니다.<br />
  등록하시고자 하는 스토어가 없으실 경우 앱 메뉴에서 스토어 등록을 먼저 진행해 주셔야 합니다.<br />
**(2) 아이템 이름**<br />
  스토어 등록 후 발급받으신 아이템의 정보를 입력합니다.<br />
**(3) 스토어 아이템 ID**<br />
  등록하고자 하는 아이템의 이름을 입력합니다.<br />
**(4) 사용 여부**<br />
  해당 아이템의 판매 여부를 선택합니다.<br />
<br />
#### 3) 조회/수정/삭제
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)
- 조회 리스트에서 각 아이템을 선택하면 등록된 아이템의 상세정보를 조회하실 수 있습니다.<br />
- 수정버튼을 누르면 스토어 정보 및 아이템 Seq를 제외한 나머지 정보를 변경하실 수 있습니다.<br />
- 삭제 버튼을 통해 판매하지 않을 아이템의 정보는 삭제하실 수 있습니다.<br />

### 결제 정보
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.0.png)
- 유료에서 등록한 아이템들의 판매 정보를 조회할 수 있습니다.<br />
- **Store ID/날짜/Payment seq/Item No/User ID**별로 원하시는 정보를 입력하여 조회가 가능합니다.<br />
- 아이템 미지급으로 인한 강제진행 기능은 추후 제공될 예정입니다.<br />

## 관리
Gamebase를 사용하는 게임에 대한 조회권한 관리/알람 발송 설정/알람에 대한 내역조회 등의 기능을 제공합니다.

### 권한
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Authorization1_1.0.png)
- Gamebase Console을 사용할 수 있는 권한을 관리하는 메뉴입니다.<br />
- **판매현황 접근 권한**은 **유료**메뉴에 대한 접근권한을 부여하며, **관리메뉴 접근 권한**은 그 외 나머지 메뉴에 대한 접근권한을 부여합니다.<br />
- 새로운 멤버의 등록은 Toast Cloud의 프로젝트 멤버에서 추가해주셔야 합니다.<br />
- 자기 자신에 대한 권한은 수정할 수 없습니다.<br />

### 알람
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm1_1.0.png)
- Gamebase는 게임 유저의 증가/감소율, 최소 동접자 수 도달에 따른 알람기능을 제공합니다.<br />
- 알람 메뉴를 통해 증가/감소율, 최소 동접자 수를 설정할 수 있으며 점검시 해당 기능을 끌 수 있는 기능도 제공합니다.<br />
- 알람을 수신할 유저에 대한 설정을 진행할 수 있으며, 새로운 멤버의 등록은 Toast Cloud의 프로젝트 멤버에서 추가해주셔야 합니다.<br />

### 알람 로그
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_AlarmLog1_1.0.png)
- 알람이 발생한 이력을 조회할 수 있습니다.<br />
- 조회이력은 최대 30일까지 가능하며, 조회 후 Search칸을 통해 실시간 필터링을 해보실 수도 있습니다.