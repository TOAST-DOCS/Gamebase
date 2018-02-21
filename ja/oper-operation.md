## Game > Gamebase > 콘솔 사용 가이드 > 운영

앱 운영시 필요한 기능들을 제공하는 메뉴입니다. <br/>

* 점검(Maintenance): 앱 점검 관리
* 공지(Notice): 게임 이용자에게 팝업 형태로 제공하는 긴급 공지 관리

## Maintenance


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance1_1.1.png)

게임 점검이 필요한 경우 Console에서 손쉽게 등록할 수 있습니다.<br/>
등록한 앱 점검 내역 조회와 점검 등록 내용 및 진행 상태 등을 한눈에 확인할 수 있으며 등록된 점검 사유로 점검 검색이 가능합니다.<br />
점검 상태는 아래와 같이 다섯 가지로 구분됩니다.<br />

(1) 예약중: 점검이 진행될 예정<br />
(2) 점검중: 현재 점검 진행 중<br />
(3) 종료: 점검 시간 종료<br />
(4) 점검해제: 점검이 진행 중인 상태에서 운영자가 **점검해제**를 한 경우<br />
(5) 점검 해제(기한만료): **점검해제** 상태에서 점검 시간이 종료되는 경우<br />

Gamebase에서는 점검진행 중 게임내에서 사용자에게 보여줄 점검팝업과 상세페이지를 제공하고 있습니다.
Gamebase에서 기본으로 제공하는 점검 팝업
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_popup_1.0.png)
Gamebase에서 기본으로 제공하는 점검 페이지(점검 사유와 점검 시간 표시)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_webview_1.1.png)


### Register Maintenance

**점검** 탭에서 **등록** 버튼을 클릭하면 점검을 등록하는 화면으로 이동합니다.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_1.4.png)

>  <font color="red">[주의] </font>**업데이트 필수와 점검이 동시에 설정**돼 있을 경우 서비스 상태는 '업데이트 필수'가 됩니다.<br/>
>  점검 진행 도중 사용자에게 업데이트 필수 팝업을 표시하고 싶지 않다면 점검 완료 이후에 서비스 상태를 '업데이트 필수'로 변경해야 합니다.<br/>

#### (1) 대상
점검을 진행할 대상을 선택합니다.<br />

- 전체 게임 : 모든 클라이언트 버전에 점검이 필요한 경우 선택합니다.
- 일부 클라이언트 : 특정 클라이언트 버전에만 점검이 필요한 경우 선택합니다. '버전 선택'버튼을 클릭하면 클라이언트 메뉴에서 등록한 클라이언트 버전리스트가 출력됩니다.<br/>
  **[일부 클라이언트 선택 화면 예시]**<br/>
  클라이언트 상태 및 스토어별 전체 선택이 가능하며, 점검을 원하는 클라이언트 버전을 선택 후 확인 버튼을 누르면 됩니다.<br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)

#### (2) 사유
점검이 진행되는 사유를 입력합니다.<br />
이 입력정보는 유저에게 노출되지 않으며 해당 점검을 등록하는 간단한 사유에 대하여 입력하시면 됩니다.<br />

#### (3) 시간
점검이 진행될 시간을 설정합니다.<br />
타임존의 경우 기본적으로 'UTC+09:00'가 선택돼 있으며, 서비스를 하는 국가의 시간대를 선택해 점검을 등록하는 것도 가능합니다.<br />

#### (4) 점검 페이지
사용자에게 제공할 점검 페이지 유형을 설정합니다.<br />
**Gamebase 제공 페이지(웹뷰)**, **사용자 정의 HTML(웹뷰)**, **외부 페이지** 중 선택할 수 있으며 각 항목별로 입력 창이 달라집니다.<br />
각 항목에 대한 추가 입력 항목은 아래와 같습니다. 입력한 내용은 **미리보기**를 클릭해 확인할 수 있습니다.<br />

##### 1) Gamebase 제공 페이지(웹뷰)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_2.0.png)
기본으로 제공되는 형식의 점검 페이지로, Gamebase에서 제공하는 웹뷰 페이지에 운영자가 입력한 정보를 표시합니다.<br />
별도의 점검 페이지가 없는 경우 유용하게 사용할 수 있습니다.<br />
**노출 메시지**에는 점검 진행 중에 사용자에게 표시할 메시지를 입력합니다.<br />
메시지는 영어, 일어, 중국어 등 외국어로도 입력할 수 있으며, 등록된 언어 중에 선택된 언어는 '기본 언어'로 설정됩니다.<br />
등록된 메시지 중에 매칭되는 언어가 없는 사용자에게는 '기본 언어'로 선택된 언어가 표시됩니다. 오른쪽의 **+** 버튼을 클릭하면 언어를 추가할 수 있으며 원하는 언어가 없는 경우 [고객 센터](https://alpha.toast.com/support/inquiry)로 연락 주시면 새로운 언어를 추가할 수 있습니다.<br />
**미리보기**를 클릭하면 '기본 언어'로 된 미리보기 화면을 확인할 수 있습니다.<br />

##### 2) 사용자 제공 HTML(웹뷰)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_3.0.png)
운영자가 직접 점검 페이지를 HTML 형식으로 입력하여 사용자에게 제공합니다.<br />
입력한 HTML 태그를 기반으로 미리보기 페이지도 함께 지원합니다.<br />
원하는 점검 페이지 형식을 만들고자 할 때 유용하게 사용할 수 있습니다.<br />
> [참고]<br/>
> HTML 페이지 제공 기능은 2018년 2월 이후에 제공될 예정입니다.<br />

##### 3) 외부 페이지
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_4.1.png)
자체 점검 페이지 또는 점검 템플릿을 가지고 있을 경우 점검 페이지를 해당 URL로 연결할 수 있습니다.<br />
연결하는 URL의 미리보기 페이지도 함께 지원합니다.<br />
점검 정보를 별도로 입력하여 점검 정보를 전달받고 싶은 경우 **점검 정보 제공** 항목을 선택하고 **노출 메시지**에 메시지를 입력합니다. 점검 페이지에 Gamebase 점검 내용에 등록한 점검 정보(점검 시간 정보, 메시지 등)를 전달받을 수 있습니다.<br />
점검 전달 파라미터는 다음과 같습니다. 모두 URL 인코딩되어 전달됩니다.

- message: 디바이스 정보에서 설정한 언어에 따른 점검 메시지. 미리보기의 경우 기본으로 선택된 메시지가 전달됨
- timezone: 점검 등록 시 선택한 표준 시간대 정보 예) UTC+9의 경우 전달 값 - +09:00
- beginDate: 점검 등록 시 입력한 시작 시간
- endDate: 점검 등록 시 입력한 종료 시간

### Modify Maintenance
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance3_1.3.png)

등록한 점검의 상세내용을 확인하고 수정, 삭제가 가능합니다.<br />
기본적으로 입력 항목은 등록 화면과 동일하며, 점검을 잘못 등록하였을 때 삭제버튼을 통하여 점검 삭제도 가능합니다.<br />
유사한 내용으로 점검을 다시 등록하고자 하는 경우 복사기능을 통하여 점검을 쉽게 등록 하실 수 있습니다.<br />

## Notice

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice1_1.2.png)

앱 실행시 팝업 형태로 노출되는 공지를 제공합니다. 로그인 이전에 노출되는 팝업이므로 외부 인증 장애나 게임 서버 장애가 발생한 경우 등록하여 사용하면 됩니다.<br/>
등록된 공지리스트와 진행상태 등을 한눈에 확인 가능하며 공지메시지로 검색도 가능합니다.<br />
공지 상태는 아래와 같이 세 가지로 구분되어 관리됩니다.<br />

(1) 예정 : 공지가 노출될 예정<br />
(2) 노출중 : 공지 노출중<br />
(3) 완료 : 공지 노출시간 종료<br />

### Register Notice

공지 메인화면에서 '등록'버튼을 클릭하면 공지를 등록하는 화면으로 이동합니다.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice2_1.0.png)

#### (1) 대상

공지를 노출할 대상을 선택합니다.<br />

- 전체 게임 : 모든 클라이언트 버전에 점검이 필요한 경우 선택합니다.
- 일부 클라이언트 : 특정 클라이언트 버전에만 점검이 필요한 경우 선택합니다. '버전 선택'버튼을 클릭하면 클라이언트 메뉴에서 등록한 클라이언트 버전리스트가 출력됩니다.<br />
  **일부 클라이언트 선택 화면 예시** <br/>
  클라이언트 상태 및 스토어별 전체 선택이 가능하며, 점검을 원하는 클라이언트 버전을 선택 후 확인 버튼을 누르면 됩니다.<br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)


#### (2) 대상 국가
공지를 노출할 국가를 선택합니다.<br />

- 전체 국가 : 모든 사용자에게 노출
- 일부 국가 : 선택한 국가의 사용자에게만 공지 노출. <br/>
  추가하고자 하는 국가코드를 입력하면 자동으로 완성되어 입력됩니다. 입력하고자 하는 국가코드가 없는 경우 [고객 센터](https://toast.com/support/inquiry)로 연락 주시기 바랍니다.

> [참고]<br/>
> 국가 판단 기준<br/>
> 사용자의 **USIM 국가코드** 기준으로 판단하며 USIM이 없을 경우 **Device**에 설정되어 있는 국가를 기준으로 공지가 노출됩니다.<br />

#### (3) 노출 횟수
공지가 사용자에게 노출되는 회수를 선택합니다.<br />

- 기간 중 1회 노출 : 노출기간 중 1회 노출
- 앱 시작할때마다 항상 노출 : 노출기간 중 사용자가 앱을 실행할 때마다 공지를 노출

#### (4) 노출 시간
공지가 표시될 시간을 설정합니다.<br />
Timezone의 경우 기본적으로 'UTC+09:00'이 선택되어 있으며, 서비스하는 국가의 시간대를 선택하여 점검을 등록하는 것도 가능합니다.<br />

#### (5) 노출 메시지
사용자에게 노출할 공지메시지를 입력합니다.<br />
메시지는 다국어로 입력이 가능하며, 등록된 언어 중에 선택된 언어는 '기본언어'로 설정됩니다.
등록된 메시지 중에 매칭되는 언어가 없는 사용자에게는 '기본 언어'로 선택된 언어가 표시됩니다. 오른쪽의 **'+'**버튼을 클릭하면 언어 추가가 가능하며 원하는 언어가 없는 경우 [고객 센터](https://toast.com/support/inquiry)로 연락 주시면 새로운 언어 추가가 가능합니다.<br />


#### (6) 하단 버튼 타입
공지 팝업 하단에 노출될 버튼의 타입을 지정합니다.<br />

- 닫기 : 닫기 버튼만 노출. <br/>
  '닫기'버튼 클릭하면 팝업을 닫고 게임을 진행합니다.<br />

- 닫기+자세히 보기 : '닫기'와 '자세히보기' 버튼을 노출.<br/>
  사용자가 '자세히보기' 버튼을 클릭하면 Console에서 입력한 링크를 WebView로 오픈합니다.<br />


#### 긴급공지 팝업 예시
(1) 닫기버튼
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_1.1.png)
(2) 닫기+자세히보기
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_detail_1.0.png)

### Modify Notice
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice3_1.1.png)

등록한 공지의 상세내용을 확인하고 수정, 삭제가 가능합니다.<br />
기본적으로 입력 항목은 등록 화면과 동일하며, 공지를 잘못 등록하였을 때 삭제버튼을 통하여 공지 삭제도 가능합니다.<br />
유사한 내용으로 점검을 다시 등록하고자 하는 경우 복사기능을 통하여 공지를 쉽게 등록 하실 수 있습니다.<br />

