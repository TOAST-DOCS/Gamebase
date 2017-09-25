## Game > Gamebase > Operator Guide >  Management
Gamebase를 사용하는 게임에 대한 조회권한 관리/알람 발송 설정/알람에 대한 내역조회 등의 기능을 제공합니다.

### Authorization(권한)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Authorization1_1.1.png)
- Gamebase Console을 사용할 수 있는 권한을 관리하는 메뉴입니다.<br />
- **판매현황 접근 권한**은 **유료**메뉴에 대한 접근권한을 부여하며, **관리메뉴 접근 권한**은 그 외 나머지 메뉴에 대한 접근권한을 부여합니다.<br />
- 새로운 멤버의 등록은 Toast Cloud의 프로젝트 멤버에서 추가해주셔야 합니다.<br />
- 자기 자신에 대한 권한은 수정할 수 없습니다.<br />

### Alarm(알람)
#### 1) 알람
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm1_1.4.png)
Gamebase는 게임 유저의 증가/감소율, 최소 동접자 수 도달에 따른 알람기능을 제공합니다.<br />
알람 메뉴를 통해 증가/감소율, 최소 동접자 수를 설정할 수 있으며 점검시 해당 기능을 끌 수 있는 기능도 제공합니다.<br />

##### (1) 알람 활성
동접 변화에 따른 알람을 받을지에 대한 여부를 설정합니다.
On으로 되어 있을 시 설정된 수치만큼 변화가 있을 경우 Email/SMS를 통하여 알람메시지를 발송하게 됩니다.

##### (2) 점검에 의한 동접 감소 알람 무시
앱에 점검이 필요할 경우 필연적으로 발생하는 동접감소 및 증가 알람을 On/Off하실 수 있습니다.
해당 기능이 켜져 있을 경우 앱이 점검중일 때 동접 변화에 따른 알람을 발송하지 않습니다.

##### (3) 감소율
동시 접속자수가 감소했을 때 알람을 받고 싶은 수치를 설정할 수 있습니다.

##### (4) 증가율
동시 접속자수가 증가했을 때 알람을 받고 싶은 수치를 설정할 수 있습니다.

##### (5) 알림 메시지
알람 발송 메시지의 언어를 선택할 수 있습니다.
현재는 한글/영어 메시지만 지원하며 추후 다른언어들도 추가 될 예정입니다.

##### (6) 최소 동시 접속자 수
앱 사용자의 기대 최소 접속자 수를 설정하여 설정 수치 이하로 동시접속자가 떨어졌을 때 알람을 받을 수 있습니다.
최소 동시 접속자 수 설정 값은 현재 500명으로 제한되어 있습니다.


#### 2) 알람 로그
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm2_1.0.png)
알람 메뉴 아래에 위치해 있으며 알람이 발생한 이력을 조회할 수 있습니다.<br />
조회이력은 최대 30일까지 가능하며, 조회 후 Search칸을 통해 실시간 필터링을 해보실 수도 있습니다.<br />

- 발생 시간 : 알람이 발송된 시간 정보
- 이전 동접자 수 : 알람이 발송되기 이전 수집되었던 동접자 정보
- 신규 동접자 수 : 알람이 발송되는 순간 수집된 동접자 정보
- 변화율(설정값) : 앞의 변화율 값은 이전 동접자 수 대비 신규 동접자 수에 대한 정보. 설정값은 알람 발생 시의 발송을 위해 설정해둔 값.

#### 3)수신자 목록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm3_1.0.png)
알람을 수신할 유저에 대한 설정을 진행할 수 있으며, 새로운 멤버의 등록은 Toast Cloud의 프로젝트 멤버에서 추가해주셔야 합니다.<br />
Gamebase 알람은 Email / SMS 전송을 지원합니다.<br />
Email의 경우는 Toast Cloud 상품 가입시 입력한 메일주소로 발송되며 SMS의 경우 가입 시 번호를 잘못 등록하였을 경우 수신받지 못할 수도 있습니다.<br />
휴대폰 정보는 Toast Cloud 내 정보 관리 페이지에서 확인하실 수 있습니다.


### Config(기타 설정)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Config1_1.0.png)
Gamebase와 ToastCloud와의 연동 관련 설정을 할 수 있습니다.<br />
Toast Cloud Launching과의 연동 설정을 할 수 있으며 연동 설정을 하기위해서는 Toast Cloud Launching이 활성화 되어 있어야 합니다.
추후 다른 상품과의 연동 설정도 추가 될 예정입니다.<br />