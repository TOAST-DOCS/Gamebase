## Game &gt; Gamebase &gt; Operator Guide &gt; Operation

This menu provides functions that are required for an app operation. <br/>

* Maintenance: Manage app maintenance
* Notice: Manage urgent pop-up notices for game users

## Maintenance


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance1_1.1.png)

Can easily register maintenance in the Console, when required.<br/>
Retrieve maintenance history of registered apps and check progress at a glance, and search maintenance by registered causes of maintenance.<br />
Status of maintenance is classified into five as below.<br />

(1) In Reservation: Maintenance has been reserved<br />
(2) In Progress: Maintenance is underway<br />
(3) Completed: Maintenance time is over.<br />
(4) Off: Operator has called for **Off** while maintenance is underway<br />
(5) Off (Expired): When maintenance time is over during **Off**<br />

Gamebase provides maintenance pop-ups and detail pages to show to game users while maintenance is underway.<br />
Default maintenance pop-up of Gamebase.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_popup_1.0.png)
Default maintenance page of Gamebase (with cause and time of maintenance)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_webview_1.1.png)


### Register Maintenance

Click **Register** under the **Maintenance** tab, to register maintenance.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_1.4.png)

>  <font color="red">[Caution] </font>When **Update Required and Maintenance are set at the same time** , the service status becomes &#39;Update Required&#39;.<br/>
>  If you don&#39;t want to show the Update Required pop-up to user during maintenance, the service status should be changed to &#39;Update Required&#39; after maintenance is completed.<br/>

#### (1) Target
Select a target of maintenance.<br />

- All Games : When maintenance is required for all client versions.
- Some Clients: When only a particular client version requires maintenance. Click &#39;Select a Version&#39; to show the list of client versions registered in the client menu.<br/>
  **[Example of selecting particular clients]**<br/>
  Can select All for client&#39;s status and each store; select a client version for maintenance and press OK. <br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)

Please note that the translation will be completed as soon as possible.
#### (2) 사유
점검이 진행되는 사유를 입력합니다.<br />
이 입력정보는 유저에게 노출되지 않으며 해당 점검을 등록하는 간단한 사유에 대하여 입력하시면 됩니다.<br />

#### (3) Time
Set time of maintenance.<br />
For a timezone, &#39;UTC+09:00&#39; is selected as default, and maintenance can be registered by selecting a timezone of a serviced country.<br />

Please note that the translation will be completed as soon as possible.
#### (4) 점검 페이지
유저에게 제공 될 점검 페이지 타입을 설정합니다.<br />
Gamebase 제공 페이지(웹뷰)/사용자 정의 HTML(웹뷰)/외부 페이지 항목이 있으며 각 항목별로 입력창이 변경됩니다.<br />
각 항목에 대한 추가 입력항목은 아래와 같으며 입력한 내용에 대한 미리보기도 함께 지원합니다.<br />

##### 1) Gamebase 제공 페이지(웹뷰)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_2.0.png)
기본적으로 제공되는 형식의 점검 페이지로써 Gamebase에서 제공하는 웹뷰 페이지에 운영자가 입력한 정보를 노출합니다.<br />
별도의 점검 페이지가 없는 경우 유용하게 사용하실 수 있습니다.<br />
노출 메시지 항목은 점검 진행 중에 사용자에게 표시할 메시지를 설정합니다.<br />
메시지는 다국어로 입력이 가능하며, 등록된 언어 중에 선택된 언어는 '기본언어'로 설정됩니다.<br />
등록된 메시지 중에 매칭되는 언어가 없는 사용자에게는 '기본 언어'로 선택된 언어가 표시됩니다. 오른쪽의 **+** 버튼을 클릭하면 언어를 추가할 수 있으며 원하는 언어가 없는 경우 [고객 센터](https://alpha.toast.com/support/inquiry)로 연락 주시면 새로운 언어를 추가할 수 있습니다.<br />
미리보기 선택 시 '기본 언어'에 대하여 미리보기 화면을 제공합니다.<br />

##### 2) 사용자 제공 HTML(웹뷰)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_3.0.png)
운영자가 직접 점검 페이지를 HTML형식으로 입력하여 유저에게 제공합니다.<br />
입력한 HTML 태그를 기반으로 미리보기 페이지도 함께 지원합니다.<br />
자신이 원하는 점검페이지 형식을 만들고자 할 때 유용하게 사용하실 수 있습니다.<br />
> [참고]<br/>
> HTML 페이지 제공 기능은 2018년 2월 배포 이후 제공될 예정입니다.<br />

##### 3) 외부 페이지
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_4.1.png)
자체 점검 페이지 또는 점검 템플릿을 가지고 있을 경우 점검 페이지를 해당 URL로 연결할 수 있습니다.<br />
연결하는 URL을 기반으로 미리보기 페이지도 함께 지원합니다.<br />
점검 정보를 별도로 입력하여 전달받고 싶은 경우 점검 정보 제공 항목을 클릭 후 점검 메시지 정보를 입력하면 점검 페이지에 Gamebase 점검 내용에 등록한 점검 정보(점검시간정보, 메시지 등)를 전달받으실 수 있습니다.<br />
점검 전달 파라미터는 아래와 같으며 모두 URL Encoding이 적용되어 전달됩니다.

- message : 디바이스 정보 언어설정에 따른 점검 메시지. 미리보기의 경우 기본으로 선택된 메시지가 전달됨.
- timezone : 점검 등록 시 선택한 timezone 정보 ex)UTC+9의 경우 전달 값 - +09:00
- beginDate : 점검 등록 시 입력한 시작시간
- endDate : 점검 등록 시 입력한 종료시간

### Modify Maintenance
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance3_1.3.png)

Can check, modify, and delete details of registered maintenance.<br />
Input items are same as Registration page, and Delete button is also available.<br />
To register maintenance again with similar content, you may copy and paste for an easy registration.<br />

## Notice

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice1_1.2.png)

Provides pop-up notifications during app execution. The pop-ups will show before logins; in case of errors in external authentication or game server, pop-ups need to be registered.<br/>
Can easily check the list of registered notifications with status and search messages.<br />
Status of notice is classified into three as below.<br />

(1) Expected: Notice is expected to show<br />
(2) In Progress: Notice is now showing<br />
(3) Completed: Notice time has been completed<br />

### Register Notice

Click &#39;Register&#39; on the main screen to register notices.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice2_1.0.png)

#### (1) Target

Select a target to show notification.<br />

- All Games: When maintenance is required for all client version.
- Some Clients: When only a particular client version requires maintenance. Click &#39;Select a Version&#39; to show the list of client versions registered in the client menu.<br />
  **Example of selecting particular clients** <br/>
  Can select a client status and all for each store, and select a client version for maintenance and press OK.<br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)


#### (2) Target Country
Select a country to show notification.<br />

- All Countries : Show to all users
- Some Countries: Show to users of a selected country only. <br/>
  Enter a country code to add and it will be automatically completed and entered. If a country code you want des not exist, contact [Customer Center](https://toast.com/support/inquiry).

> [Note]<br/>
> Criteria of Country Selection<br/>
> Notice shows on the basis of user&#39;s **USIM Country Code** ; when USIM is not available, it shows based on countries set on **Device**.<br />

#### (3) Number of Impressions
Select a number of times a notice shows to users.<br />

- One impression during the impression period: Shows one time during impression period
- Always show when you start an app: Shows every time user starts an app during impression period

#### (4) Time
Set time to show notices.<br />
For a timezone, &#39;UTC+09:00&#39; is provided as default, and maintenance can be registered by selecting a timezone of a country at service.<br />

#### (5) Message
Enter notice messages to show to users.<br />
Can register many languages, for those who speak other languages than registered, a default language will show.<br />
To add a language, click **+** on the right, and if a language you want is not on the list, contact [Customer Center](https://toast.com/support/inquiry)to add as required.


#### (6) Bottom Button Type
Specify a type of button to show at the bottom of a notice pop-up.<br />

- Close : Show Close button only. <br/>
  Click &#39;Close&#39; to close a pop-up and play games.<br />

- Close + More: Show &#39;Close&#39; and &#39;More&#39;.<br/>
  Click &#39;More&#39; and the link entered in the console opens on WebView.<br />


#### **Example of a Notice Pop-up**
(1) Close
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_1.1.png)
(2) Close + More
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_detail_1.0.png)

### Modify Notice
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice3_1.1.png)

Can check, modify, and delete details of notification.<br />
Input items are same as registration, and Delete button is provided to delete a notice.<br />
To register a notice again with similar content, you may copy and paste for an easy registration.<br />

