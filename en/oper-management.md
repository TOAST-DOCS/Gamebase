## Game > Gamebase > Console Guide > Management

You can manage search authority on Gamebase games, set alarm delivery, and retrieve alarm history.

## Authorization

Authority of Gamebase Console can be managed.

![gamebase_manage_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_01_201812_en.png)

* Manage authority of Gamebase Console
  * **Access to sales status** : Access to **IAP** menu
  * **Access to management menu** : Access authority to other menu
  * **Role of Downloading All Members**: Role to download information of **All Project Members** 
* To register a new member, go to **NHN Cloud Console Page**.
* Cannot modify your own authority.

## Alarm

Gamebase Alarm notifies increase/decrease rate of game users, or change of initial number of concurrent access.

### Alarm

![gamebase_manage_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_02_201812_en.png)

#### (1) Activate Decrease Alarms
Set whether to receive alarms for decreased concurrent access. To receive alarms, set **On** for Activate Decrease Alarms.

- **Decrease Rate**: Specify the percentage of decrease in concurrent access to send alarms.
- **Ignore Alarms for Decrease due to maintenance**: The number of concurrent access cannot help but decrease while app is under maintenance.
  In this case, set **On** for **Ignore Alarms for Decrease due to Maintenance** so as not to receive alarms.

#### (2) Activate Increase Alarms
You may set to receive alarms when concurrent access increases.
The operator can set criteria to receive alarms when the function is activated.

#### (3) Message Language
Select a language for alarm messages. Now only Korean and English are supported, and other languages will be added at the request.

#### (4) Minimum Number of Concurrent Access
You can receive alarms when there is lower number of app access than specified **minimum number of concurrent access**. For example, if the **minimum number of concurrent access** is '500', you will receive an alarm when the number is lower than 500. You cannot set below 100 as it is the minimum number.

### Alarm Log

In Alarm Log, which is under the Alarm Menu, you can retrieve history of alarm occurrences.
Logs can be retrieved up to 30 days, and real-time filtering by using **Search** Textbox is also available.

![gamebase_manage_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_03_201812_en.png)

- Time of Occurrence: Time when an alarm is sent
- Number of Previous Concurrent Access: Information of concurrent access collected before alarm is sent
- Number of New Concurrent Access: Information of concurrent access collected at the moment when alarm is sent
- Rate of Change (Value Set): Rate of change refers to the number of new concurrent access as compared to previous concurrent access. Value set is the value which has been set to deliver when alarm occurs.

### Webhook
Webhook configuration is provided to receive alarms, other than SMS/Email as default Gamebase functions.
When there is a request for alarm delivery via Webhook URL of an external system, alarm is sent altogether.

#### (1) Retrieve List
![gamebase_manage_04_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_04_201812_en.png)
Retrieve the list of registered webhooks that can receive alarms.
When a registered webhook URL is required, click **Copy URL** on the right.

#### (2) Register
![gamebase_manage_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_05_201812_en.png)
Click **Register** to register webhook information from external systems.
Currently, only Dooray and Slack can be registered, and the list is to be added at the request.

#### (3) Retrieve/Modify/Delete Details
![gamebase_manage_06_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_06_201812_en.png)
Click each item to retrieve details.
To change registered information, click **Modify**. You may click to **Delete** a webhook when it is not required.

### Recipient List

Alarm receivers can be configured. To register a new member, go to **NHN Cloud Console > Project > Member Management**.
![gamebase_manage_07_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_07_201812_en.png)
Gamebase allows to send alarms via email or SMS.
In both cases, subscription information to NHN Cloud are used for delivery; alarms may not be sent properly if there is any wrong information in email address or number. To check mobile phone information, go to **My Account** of NHN Cloud Cloud.

<br/>
## Config

Integration between Gamebase and NHN Cloud can be configured.

![gamebase_manage_08_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_manage_08_201812_en.png)

You may set whether to receive information set in NHN Cloud Launching all at once, when Gamebase Launching API is called. The function can be turned On or Off, only when you use the NHN Cloud Launching service.
