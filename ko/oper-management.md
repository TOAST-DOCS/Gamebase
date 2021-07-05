## Game > Gamebase > 콘솔 사용 가이드 >  관리

Gamebase를 사용하는 게임에 대한 조회 권한 관리, 알람 발송 설정, 알람에 대한 내역 조회 등의 기능을 사용할 수 있습니다.



## Authorization

Gamebase Console 사용 권한을 관리할 수 있습니다.
![gamebase_manage_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_01_202106.png)

* Gamebase Console 사용 권한 관리
  * **위클리 리포트 수신 권한** : **위클리 리포트** 수신에 대한 권한
* 새 멤버를 등록하려면 NHN Cloud 프로젝트 멤버관리에서 추가해야 합니다.
* 자기 자신의 권한은 수정할 수 없습니다.


## Alarm

Gamebase의 알람 기능을 사용하여 게임 유저의 증가율이나 감소율, 최소 동시 접속자 수 변화 등에 대한 알람을 받을 수 있습니다.

### Alarm

![gamebase_manage_02_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_02_202106.png)

#### (1) 감소 알람 활성
동시 접속이 감소했을 때 알람을 받을지 여부를 설정합니다. 알람을 받으려면 **감소 알람 활성**을 **On**으로 설정합니다.

- **감소율**: 동시 접속이 몇 % 감소했을 때 알람을 받을 것인지를 지정합니다.
- **점검에 의한 동접 감소 알람 무시**: 앱을 점검하는 경우에는 동시 접속 수가 감소할 수밖에 없습니다.
  이런 경우에는 **점검에 의한 동접 감소 알람 무시**를 **On**으로 설정하여 알람을 받지 않게 할 수 있습니다.

#### (2) 증가 알람 활성
동시 접속자 수가 증가했을 때 알람을 받도록 설정할 수 있습니다.
기능이 활성화되었을 경우 운영자가 알람을 받고자 하는 수치를 설정할 수 있습니다.

#### (3) 메시지 언어
알람 발송 메시지의 언어를 선택할 수 있습니다. 현재는 한글과 영어 메시지만 지원하며 추가 요구 사항이 있을 때 다른 언어들을 추가할 예정입니다.

#### (4) 최소 동시 접속자 수
**최소 동시 접속자 수**에 지정한 인원보다 적은 수가 앱에 접속한 경우에 알람을 받게 할 수 있습니다. 예를 들어 **최소 동시 접속자 수**를 '500명'으로 설정한 경우, 동시 접속자 수가 500명 아래로 떨어지면 알람을 받게 됩니다. 최소 설정값은 100명이며, 100명 미만의 값은 설정할 수 없습니다.

### Alarm Log

알람 로그는 알람 메뉴 아래에 있으며 알람이 발생한 이력을 조회할 수 있습니다.
조회는 최대 30일까지 가능하며, 조회 후 **Search** 버튼을 클릭하면 실시간으로 필터링도 할 수 있습니다.

![gamebase_manage_03_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_03_202106.png)

- 발생 시간: 알람이 발송된 시간 정보
- 이전 동시 접속자 수: 알람이 발송되기 이전 수집되었던 동시 접속자 정보
- 신규 동시 접속자 수: 알람이 발송되는 순간 수집된 동시 접속자 정보
- 변화율(설정값): 앞의 변화율 값은 이전 동시 접속자 수 대비 신규 동시 접속자 수에 대한 정보. 설정값은 알람 발생 시의 발송을 위해 설정해둔 값

### Webhook
Gamebase에서 기본으로 제공되는 SMS/Email 외에 별도로 알람을 수신할 수 있는 Webhook 설정 기능을 제공합니다.
외부 시스템의 Webhook URL을 통해 알람 발송 요청이 있을 경우 함께 알람을 전송합니다.

#### (1) 목록 조회
![gamebase_manage_04_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_04_202106.png)
현재 알람을 수신할 수 있는 Webhook들에 대한 등록 내역을 볼 수 있습니다.
등록된 Webhook URL이 필요한 경우 오른쪽의 **URL 복사**를 클릭해 손쉽게 복사할 수 있습니다.

#### (2) 등록
![gamebase_manage_05_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_05_202106.png)
**등록** 버튼을 클릭해 외부 시스템에서 발급받은 Webhook 정보를 등록할 수 있습니다.
현재는 Dooray와 Slack만 등록할 수 있으며 추후 요청이 있을 때 새로운 목록을 추가할 예정입니다.

#### (2) 상세조회/수정/삭제
![gamebase_manage_06_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_06_202106.png)
각 항목을 클릭하면 상세 정보를 조회할 수 있습니다.
등록된 정보를 변경하려면 **수정** 버튼을 클릭합니다. 만약 해당 Webhook이 필요하지 않을 경우에는 **삭제** 버튼을 클릭해 항목을 삭제할 수도 있습니다.

### Recipient List

알람을 수신할 사용자를 설정할 수 있습니다. 새 맴버를 등록하려면 NHN Cloud 프로젝트 멤버관리에서 추가해야  합니다.
![gamebase_manage_07_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_07_202106.png)
Gamebase에서는 이메일과 SMS로 알람을 전송할 수 있습니다.
이메일과 SMS 모두 NHN Cloud 가입할 때 입력한 정보를 이용하여 발송되며, 이메일 주소나 번호를 잘못 등록한 경우에는 알람을 받지 못할 수도 있습니다.휴대폰 번호 정보는 NHN Cloud의 **내 정보 관리** 페이지에서 확인할 수 있습니다.


## Config

Gamebase와 NHN Cloud 서비스의 연동 관련 설정을 할 수 있습니다.

![gamebase_manage_08_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_08_202106.png)

NHN Cloud Launching에 설정한 정보를 Gamebase Launching API 호출 시에 함께 전달받을지를 설정할 수 있습니다. NHN Cloud Launching 서비스를 사용하는 경우에만 기능을 On, Off할 수 있습니다.
