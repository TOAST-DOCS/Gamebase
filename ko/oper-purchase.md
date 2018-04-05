## Game > Gamebase > 콘솔 사용 가이드 > 결제



인앱 결제와 관련된 정보를 등록하고 내역을 조회할 수 있습니다.<br/>

Gamebase에서는 TOAST IAP(In-App Purchase, 인앱 결제) 서비스를 사용합니다.<br/>

<br/>

## Store

게임 내에서 아이템을 판매하기 위해 스토어를 등록합니다.<br/> 

**Store** 탭의 **스토어 정보 리스트**에서 새 스토어를 등록하거나 이미 등록한 스토어를 관리할 수 있습니다.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)





### Register



새로운 스토어를 등록하려면 **스토어 정보 리스트** 화면의 **등록** 버튼을 클릭합니다.



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)



* **스토어**<br/>  등록하고자 하는 외부 스토어를 선택합니다.<br />  등록하고자 하는 스토어가 없다면, [고객센터](https://toast.com/support/inquiry)로 연락주시기 바랍니다.<br />

* **앱 이름** <br/>  등록하고자 하는 게임의 이름을 입력합니다.<br />

* **스토어 앱 ID** <br/>  스토어에서 발급받은 정보를 입력합니다.<br />

* **사용 여부**<br/>  스토어 사용 여부를 선택합니다.<br />



### Modify



조회 목록에서 등록된 스토어의 상세 정보를 조회하거나 정보를 변경할 수 있습니다.



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)

- 조회 목록에서 등록된 스토어를 선택하면 상세 정보를 조회할 수 있습니다.<br />

- **수정** 버튼을 클릭하면 스토어 앱 ID를 제외한 앱 이름, 스토어 앱, 사용 여부 정보를 수정할 수 있습니다.<br />

- **삭제** 버튼을 클릭하면 스토어의 정보를 삭제할 수 있습니다. 단, 사용여부가 미사용인 스토어 삭제만 가능합니다.<br />

  <br/>



## Item



스토어에서 판매할 아이템을 등록할 수 있습니다. <br>

**아이템** 탭에서 새 아이템을 등록하거나 이미 등록한 아이템을 관리할 수 있습니다. 기본적으로 모든 스토어에 대한 아이템이 표시되며, 각 스토어별 필터링 기능도 사용할 수 있습니다.<br />



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)



### Register



새로운 아이템을 등록하려면 **스토어 정보 리스트** 화면의 **등록** 버튼을 클릭합니다.



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)



* **스토어**<br />  등록하고자 하는 외부 스토어를 선택합니다.<br />  등록하려는 스토어가 없다면 **스토어** 메뉴에서 먼저 스토어를 등록해야 합니다.<br />

* **아이템 이름**<br /> 스토어 등록 후 발급받은 아이템의 정보를 입력합니다.<br />게임에서 등록된 아이템 이름을 이용하여 앱 내에 노출이 가능합니다.<br>

* **스토어 아이템 ID**<br />등록하고자 하는 아이템의 이름을 입력합니다.<br />

* **사용 여부**<br />  해당 아이템의 판매 여부를 선택합니다.<br />



### Modify



조회 목록에서 등록된 아이템의 상세 정보를 조회하거나 정보를 변경할 수 있습니다.



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)

- 조회 목록에서 각 아이템을 선택하면 등록된 아이템의 상세 정보를 조회할 수 있습니다.<br />

- **수정** 버튼을 클릭하면 스토어와 아이템 Seq를 제외한 나머지 정보를 변경할 수 있습니다.<br />

- **삭제** 버튼을 클릭하면 아이템 정보를 삭제할 수 있습니다.<br />

  <br/>



## Transactions

결제 정보를 조회할 수 있습니다.



![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.1.png)

<br />



아래 검색조건을 이용하여 원하는 결제 정보를 조회할 수 있습니다. <br>



### 검색 조건

- **스토어**: 결제된 스토어 정보

- **날짜**: 사용자가 구입을 시도한 시간

- **결제 번호**: Gamebase 내에서 결제를 구별할 수 있는 고유 번호

- **아이템 번호**: 사용자가 앱에서 구입한 실제 아이템 번호(아이템번호는 '아이템'탭에서 확인 가능합니다.)

- **유저 ID**: 결제한 사용자 아이디

- **정렬순서**: 등록일시 기준으로 오름차순, 내림차순 정렬







### 검색 결과

- **결제 번호**: Gamebase 내에서 결제를 구별할 수 있는 고유 번호

- **스토어**: 결제된 스토어 정보

- **사용자 아이디**: 결제한 사용자 아이디

- **아이템 이름**: 사용자가 앱에서 구입한 실제 아이템 이름

- **가격**: 사용자가 구입한 아이템의 가격

- **통화**: 사용자가 구입 시 사용한 통화 종류

- **Consume**: 결제한 아이템의 지급 여부

- **결제 상태**: 결제의 현재 진행 상태

- **Store Reference Key**: 스토어에서 발급해주는 결제 고유 번호

- **등록일시**: 사용자가 구입을 시도 또는 완료한 시간

- **환불일시**: 사용자 아이템이 환불된 시간



외부 스토어(App Store, Google Play 등)에서는 결제가 완료되었으나, TOAST IAP에서는 결제 완료 처리가 되지 않아 아이템이 지급되지 않을 때 Console에서 수동으로 결제 완료 처리할 수 있는 기능을 제공할 예정입니다. 일정은 미정입니다.<br />