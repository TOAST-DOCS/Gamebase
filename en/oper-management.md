## Game &gt; Gamebase &gt; Operator Guide &gt; Management

You can manage search authority on Gamebase games, set alarm delivery, and retrieve alarm history.

<br/>

## Authorization

Authority of Gamebase Console can be managed. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Authorization1_1.2.png)

* Manage authority of Gamebase Console<br />
  * **Access to sales status** : Access to **IAP** menu
  * **Access to management menu** : Access authority to other menu
* To register a new member, go to **TOAST Console Page**.<br />
* Cannot modify your own authority.<br />
  <br/>

## Alarm

Gamebase Alarm notifies increase/decrease rate of game users, or change of initial number of concurrent access.<br />

### Alarm

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm1_1.7.png)

Please note that the translation will be completed as soon as possible.
#### (1) 감소 알람 활성
동시 접속이 감소했을 때 알람을 받을지에 대한 여부를 설정합니다. On으로 되어 있을 때는 설정된 수치만큼 변화가 있을 경우 이메일이나 SMS로 알람 메시지를 발송합니다.<br />
기능이 활성화가 되었을 경우 원하는 감소율 및 점검에 의한 동접 감소 알람 무시 기능을 추가 설정할 수 있습니다.<br />
점검에 의한 동접 감소 알람 무시 설정을 통해 앱에 점검이 필요할 경우 필연적으로 발생하는 동시 접속 감소 알람을 On/Off할 수 있습니다. 해당 설정이 켜져 있을 경우 앱이 점검 중일 때 동시 접속 감소에 따른 알람을 발송하지 않습니다.<br />

#### (2) 증가 알람 활성
동시 접속자 수가 증가했을 때 알람을 받도록 설정할 수 있습니다. <br />
기능이 활성화 되었을 경우 운영자가 알람을 받고자 하는 수치를 설정할 수 있습니다. <br />

#### (3) 메시지 언어
알람 발송 메시지의 언어를 선택할 수 있습니다. 현재는 한글과 영어 메시지만 지원하며 추가 요구 사항이 있을 때 다른 언어들을 추가할 예정입니다.

#### (4) 최소 동시 접속자 수
앱 사용자의 기대 최소 접속자 수를 설정하여 설정 수치 이하로 동시 접속자 수가 떨어졌을 때 알람을 받을 수 있습니다. 최소 동시 접속자 수 설정 값은 100명으로 제한되어 있습니다.


### Alarm Log

In Alarm Log, which is under the Alarm Menu, you can retrieve history of alarm occurrences.<br />
Logs can be retrieved up to 30 days, and real-time filtering by using **Search** Textbox is also available. <br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm2_1.0.png)

- Time of Occurrence: Time when an alarm is sent
- Number of Previous Concurrent Access: Information of concurrent access collected before alarm is sent
- Number of New Concurrent Access: Information of concurrent access collected at the moment when alarm is sent
- Rate of Change (Value Set): Rate of change refers to the number of new concurrent access as compared to previous concurrent access. Value set is the value which has been set to deliver when alarm occurs.

Please note that the translation will be completed as soon as possible.
### Webhook
Gamebase에서 기본으로 제공되는 SMS/Email외에 별도로 알람을 수신할 수 있는 Webhook 설정기능을 제공합니다.
외부 시스템의 Webhook URL을 통해 알람발송 요청이 있을 경우 함께 알람을 전송합니다.

#### (1) 목록 조회
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm4_1.1.png)
현재 알람을 수신할 수 있는 Webhook들에 대한 등록 내역을 조회하여 보여줍니다.<br />
등록된 Webhook URL이 필요한 경우 우측의 URL 복사 기능을 통해 손쉽게 복사가 가능합니다.

#### (2) 등록
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm4_2.0.png)
상단의 등록 버튼을 통해 Webhook정보를 등록할 수 있습니다.<br />
외부 시스템에서 발급받은 Webhook URL을 해당 기능을 통해 등록하실 수 있습니다.<br />
현재는 Dooray/Slack 등록만 제공하고 있으며 추후 요청이 있을 시 새로운 목록을 추가할 예정입니다.

#### (2) 상세조회/수정/삭제
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm4_3.1.png)
목록에서 상세정보를 조회하고자 하는 항목을 클릭하면 위와 같이 상세정보를 조회할 수 있습니다..<br />
더불어 수정버튼을 통해 등록된 정보를 변경할 수 있으며 해당 Webhook이 필요하지 않을 경우 삭제버튼을 통해 언제든지 삭제가 가능합니다.


### Recipient List

Alarm receivers can be configured.<br />
To register a new member, go to **TOAST Console Page**. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm3_1.1.png)
In both cases, subscription information to TOAST are used for delivery; alarms may not be sent properly if there is any wrong information in email address or number.<br />
To check mobile phone information, go to **My Account** of TOAST Cloud.

<br/>
## Config

Integration between Gamebase and TOAST can be configured.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Config1_1.0.png)

Please note that the translation will be completed as soon as possible.
TOAST Launching에 설정한 정보를 Gamebase 런칭 API 호출 시에 함께 전달받을지를 설정할 수 있습니다. TOAST Launching 서비스를 사용하는 경우에만 기능을 On, Off할 수 있습니다.<br />