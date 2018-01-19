## Game > Gamebase > 콘솔 사용 가이드 >  Management

Gamebase를 사용하는 게임에 대한 조회 권한 관리, 알람 발송 설정, 알람에 대한 내역 조회 등의 기능을 사용할 수 있습니다.

<br/>
## Authorization

Gamebase Console 사용 권한을 관리할 수 있습니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Authorization1_1.1.png)

* Gamebase Console 사용 권한 관리<br />
  * **판매현황 접근 권한** : **유료** 메뉴에 대한 접근 권한
  * **관리메뉴 접근 권한** : 그 외 메뉴에 대한 접근 권한
* 새 멤버를 등록하려면 TOAST 프로젝트 멤버관리에서 추가해야 합니다.<br />
* 자기 자신의 권한은 수정할 수 없습니다.<br />
  <br/>

## Alarm

Gamebase의 알람 기능을 사용하여 게임 이용자의 증가율이나 감소율, 최소 동시 접속자 수 변화 등에 대한 알람을 받을 수 있습니다.<br />

### Alarm 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm1_1.4.png)

#### (1) 알람 활성

동시 접속 변화가 있을 때 알람을 받을지에 대한 여부를 설정합니다. On으로 되어 있을 때는 설정된 수치만큼 변화가 있을 경우 이메일이나 SMS로 알람 메시지를 발송합니다.<br/>

#### (2) 점검에 의한 동시 접속 감소 알람 무시

앱에 점검이 필요할 경우 필연적으로 발생하는 동시 접속 감소 및 증가 알람을 On/Off할 수 있습니다. 해당 기능이 켜져 있을 경우 앱이 점검 중일 때 동시 접속 변화에 따른 알람을 발송하지 않습니다.

#### (3) 감소율

동시 접속자 수가 감소했을 때 알람을 받고 싶은 수치를 설정할 수 있습니다.

#### (4) 증가율

동시 접속자 수가 증가했을 때 알람을 받고 싶은 수치를 설정할 수 있습니다.

#### (5) 알림 메시지

알람 발송 메시지의 언어를 선택할 수 있습니다. 현재는 한글과 영어 메시지만 지원하며 추가 요구 사항이 있을 때 다른 언어들을 추가할 예정입니다.

#### (6) 최소 동시 접속자 수

앱 사용자의 기대 최소 접속자 수를 설정하여 설정 수치 이하로 동시 접속자 수가 떨어졌을 때 알람을 받을 수 있습니다. 최소 동시 접속자 수 설정 값은 500명으로 제한되어 있습니다.


### Alarm Log

알람 로그는 알람 메뉴 아래에 있으며 알람이 발생한 이력을 조회할 수 있습니다.<br />
조회는 최대 30일까지 가능하며, 조회 후 **Search** 버튼을 클릭하면 실시간으로 필터링도 할 수 있습니다.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm2_1.0.png)

- 발생 시간: 알람이 발송된 시간 정보
- 이전 동시 접속자 수: 알람이 발송되기 이전 수집되었던 동시 접속자 정보
- 신규 동시 접속자 수: 알람이 발송되는 순간 수집된 동시 접속자 정보
- 변화율(설정값): 앞의 변화율 값은 이전 동시 접속자 수 대비 신규 동시 접속자 수에 대한 정보. 설정값은 알람 발생 시의 발송을 위해 설정해둔 값

### Recipient List

알람을 수신할 사용자를 설정할 수 있습니다. 새 맴버를 등록하려면 TOAST 프로젝트 멤버관리에서 추가해야  합니다.<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm3_1.0.png)
Gamebase에서는 이메일과 SMS로 알람을 전송할 수 있습니다.<br />
이메일과 SMS 모두 TOAST 가입할 때 입력한 정보를 이용하여 발송되며, 이메일 주소나 번호를 잘못 등록한 경우에는 알람을 받지 못할 수도 있습니다.<br />휴대폰 번호 정보는 TOAST의 **내 정보 관리** 페이지에서 확인할 수 있습니다.

<br/>
## Config

Gamebase와 TOAST 서비스의 연동 관련 설정을 할 수 있습니다.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Config1_1.0.png)

TOAST Launching에 설정한 정보를 Gamebase 런칭 API 호출 시에 함께 전달받을지를 설정할 수 있습니다. TOAST Launching 서비스를 사용하는 경우에만 기능을 On, Off할 수 있습니다.<br />