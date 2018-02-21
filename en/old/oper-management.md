## Game > Gamebase > Operator Guide >  Management
You can manage search authority on Gamebase games, set alarm delivery, and retrieve alarm history. <br/>

## Authorization
Authority of Gamebase Console can be managed. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Authorization1_1.1.png)

* Manage Authority of Gamebase Console <br />
  * Access to sales status : Access to **paid** menu
  * Access to control menu : Access authority to other menu
* To register a new member, go to **TOAST Cloud Console>Setting>Project Member** <br />
* Cannot modify your own authority. <br />
  <br/>
## Alarm
Gamebase Alarm notifies increase/decrease rate of game users, or change of initial number of concurrent access . <br />
### Alarm
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm1_1.4.png)

#### (1) **Active Alarm**
Decide whether to receive alarms on change of concurrent access. If it is set On, alarm messages are sent via email or SMS when there is a change as much as configured. <br/>

#### (2) Ignore Alarm by Maintenance  
If an app requires maintenance, alarms on decrease/increase of concurrent access, which will inevitably occur, can be turned on or off.  When it is Off, alarms on change of concurrent access during maintenance will not be delivered.

#### (3) Decrease Rate
Can adjust measures to send alarms when the number of concurrent access decreases.  

#### (4) Increase Rate
Can adjust measures to send alarms when the number of concurrent access increases.

#### (5) Alarm Message
Select a language for alarm messages. At the moment, only Korean and English are supported and other languages are to be added later.   

#### (6) Minimum Concurrent Users
By setting up expected number of initial access, alarms can be delivered when the number decreases below set number. The number setting is confined to 500.  


### Alarm Log
In Alarm Logs, which is under the Alarm Menu, you can retrieve history of alarm occurrences. <br />
Logs can be retrieved up to 30 days, and real-time filtering by using **{Search} Textbox** is also available.  <br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm2_1.0.png)

- Time of Occurrence: Time when an alarm is sent
- Number of Previous Concurrent Access : Information of concurrent access collected before alarm is sent  
- Number of New Concurrent Access: Information of concurrent access collected at the moment when alarm is sent
- Rate of Change (Value Set): Rate of change refers to the number of new concurrent access as compared to previous concurrent access. Value set is the value which has been set to deliver when alarm occurs.

### Recipient List
**Alarm receivers can be configured**. To register a new member, go to TOAST Cloud Project Member.  <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Alarm3_1.0.png)
Alarms can be sent via email and SMS in Gamebase. <br />
In both cases, subscription information to TOAST Cloud are used for delivery; alarms may not be sent properly if there is any wrong information in email address or number. <br />

For mobile phone information, go to **My Account** of TOAST Cloud. <br/>

## Config
Integration between Gamebase and TOAST Cloud can be configured. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Management_Config1_1.0.png)
Integration with TOAST Cloud Launching is available, and it requires TOAST Cloud Launching to be enabled. Further configuration for integration with other products is to be added later.   <br />
