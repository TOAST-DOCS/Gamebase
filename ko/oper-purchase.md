## Game > Gamebase > 콘솔 사용 가이드 > 결제

인앱 결제와 관련된 정보를 등록하고 내역을 조회할 수 있습니다.
Gamebase에서는 TOAST IAP(In-App Purchase, 인앱 결제) 서비스를 사용합니다.

## Store

게임 내에서 아이템을 판매하기 위해 스토어를 등록합니다. 
**Store** 탭의 **스토어 정보 리스트**에서 새 스토어를 등록하거나 이미 등록한 스토어를 관리할 수 있습니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)

### Register

새로운 스토어를 등록하려면 **스토어 정보 리스트** 화면의 **등록** 버튼을 클릭합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)

* **스토어**  등록하고자 하는 외부 스토어를 선택합니다.  등록하고자 하는 스토어가 없다면, [고객센터](https://toast.com/support/inquiry)로 연락주시기 바랍니다.
* **앱 이름**   등록하고자 하는 게임의 이름을 입력합니다.
* **스토어 앱 ID**   스토어에서 발급받은 정보를 입력합니다.
* **사용 여부**  스토어 사용 여부를 선택합니다.

### Modify

조회 목록에서 등록된 스토어의 상세 정보를 조회하거나 정보를 변경할 수 있습니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- 조회 목록에서 등록된 스토어를 선택하면 상세 정보를 조회할 수 있습니다.
- **수정** 버튼을 클릭하면 스토어 앱 ID를 제외한 앱 이름, 스토어 앱, 사용 여부 정보를 수정할 수 있습니다.
- **삭제** 버튼을 클릭하면 스토어의 정보를 삭제할 수 있습니다. 단, 사용여부가 미사용인 스토어 삭제만 가능합니다.

## Item

스토어에서 판매할 아이템을 등록할 수 있습니다. 
**아이템** 탭에서 새 아이템을 등록하거나 이미 등록한 아이템을 관리할 수 있습니다. 기본적으로 모든 스토어에 대한 아이템이 표시되며, 각 스토어별 필터링 기능도 사용할 수 있습니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)

### Register

새로운 아이템을 등록하려면 **스토어 정보 리스트** 화면의 **등록** 버튼을 클릭합니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)

* **스토어**  등록하고자 하는 외부 스토어를 선택합니다.  등록하려는 스토어가 없다면 **스토어** 메뉴에서 먼저 스토어를 등록해야 합니다.
* **아이템 이름** 스토어 등록 후 발급받은 아이템의 정보를 입력합니다.게임에서 등록된 아이템 이름을 이용하여 앱 내에 노출이 가능합니다.
* **스토어 아이템 ID**등록하고자 하는 아이템의 이름을 입력합니다.
* **사용 여부**  해당 아이템의 판매 여부를 선택합니다.

### Modify

조회 목록에서 등록된 아이템의 상세 정보를 조회하거나 정보를 변경할 수 있습니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)
- 조회 목록에서 각 아이템을 선택하면 등록된 아이템의 상세 정보를 조회할 수 있습니다.
- **수정** 버튼을 클릭하면 스토어와 아이템 Seq를 제외한 나머지 정보를 변경할 수 있습니다.
- **삭제** 버튼을 클릭하면 아이템 정보를 삭제할 수 있습니다.

## Transactions

결제 정보를 조회할 수 있습니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.4.png)

아래 검색조건을 이용하여 원하는 결제 정보를 조회할 수 있습니다.
결제 내역은 우측 상단의 다운로드 버튼을 통해 언제든지 다운로드 받으실 수 있습니다.
### 검색 조건

- **스토어**: 결제된 스토어 정보
- **날짜**: 사용자가 구입을 시도한 시간
- **결제 번호**: Gamebase 내에서 결제를 구별할 수 있는 고유 번호
- **아이템 번호**: 사용자가 앱에서 구입한 실제 아이템 번호(아이템번호는 '아이템'탭에서 확인 가능합니다.)
- **유저 ID**: 결제한 사용자 아이디
- **정렬순서**: 등록일시 기준으로 오름차순, 내림차순 정렬
- **결제상태**: 결제 상태를 기준으로 정보를 조회

### 검색 결과
- **Transaction ID**: Gamebase 내에서 결제를 구별할 수 있는 고유 번호
- **스토어**: 결제된 스토어 정보
- **유저 ID**: 결제한 사용자 아이디
- **아이템 이름**: 사용자가 앱에서 구입한 실제 아이템 이름
- **가격**: 사용자가 구입한 아이템의 가격
- **통화**: 사용자가 구입 시 사용한 통화 종류
- **소비 상태**: 결제한 아이템의 지급 여부
- **결제 상태**: 결제의 현재 진행 상태
- **Store Reference Key**: 스토어에서 발급해주는 결제 고유 번호
- **결제예약일시**: 사용자가 구입을 시도 또는 완료한 시간
- **환불일시**: 사용자 아이템이 환불된 시간

### 결제 상태 변경
조회된 결제정보의 상태는 아래와 같으며 각각의 상태는 아래와 같습니다.
- **Success**
	- 결제 완료
    - 결제 처리가 정상적으로 완료됬을 경우를 의미합니다.
    - Refund상태로 변경 가능합니다.
- **Reserved**
	- 결제 진행중
	- 스토어를 통한 결제가 더이상의 진행이 되지 않거나 결제검증까지 진행되지 않은 경우를 의미합니다.
	- Success, Refund상태로 변경 가능합니다.
- **Failure**
	- 결제 검증 실패
	- 스토어에서 결제를 진행했으나 결제검증에서 오류가 난 경우를 의미합니다.
	- Success, Refund상태로 변경 가능합니다.
- **Refund**
	- 환불 완료
	- 관리자가 수동으로 마켓에서 환불처리에 대한 여부를 업데이트한 경우입니다.
	- 다른 결제상태로 변경이 불가능합니다.

#### Success 변경
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_1.0.png)
결제 진행시 발급받은 **영수증 번호**, **가격**, **통화**정보를 입력해야 상태 변경이 가능합니다.

#### Refund 변경
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_2.0.png)
별다른 추가정보의 입력 없이 상태를 선택한 후 변경을 선택합니다.
변경된 결제 정보는 이후 변경이 불가능하므로 신중하게 확인 후 진행해야 합니다.

### 영수증 검증
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* 조회된 영수증을 기반으로 해당 결제건이 유효한 지 검증할 수 있습니다.
* 각 필드의 비교결과를 알려주며 스토어로부터 받은 응답값을 Json형식으로 제공하므로 필요한 경우 데이터를 직접 확인하실 수 있습니다.
* 현재는 App Store 결제건에 대해서만 검증을 제공합니다.
