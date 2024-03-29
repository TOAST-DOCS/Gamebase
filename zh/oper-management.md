## Game > Gamebase > Console Guide > Management

You can manage search authority on Gamebase games, set alarm delivery, and retrieve alarm history.

## Authorization

You can manage the Gamebase Console access permission.
![management_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_01_en_240103.png)
* Manage Gamebase Console access permission
  * **Permission to receive weekly reports** : Permission for receiving **weekly reports**
* To register a new member, they must be added in Manage Members menu of the NHN Cloud project.
* One's own permission cannot be changed.

## Alarm

Gamebase Alarm notifies increase/decrease rate of game users, or change of initial number of concurrent access.

### Alarm

![management_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_02_en_240103.png)

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

![management_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_03_en_240103.png)

- Time of Occurrence: Time when an alarm is sent
- Number of Previous Concurrent Access: Information of concurrent access collected before alarm is sent
- Number of New Concurrent Access: Information of concurrent access collected at the moment when alarm is sent
- Rate of Change (Value Set): Rate of change refers to the number of new concurrent access as compared to previous concurrent access. Value set is the value which has been set to deliver when alarm occurs.

### Webhook
Webhook configuration is provided to receive alarms, other than SMS/Email as default Gamebase functions.
When there is a request for alarm delivery via Webhook URL of an external system, alarm is sent altogether.

#### (1) Retrieve List
![management_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_04_en_240103.png)
Retrieve the list of registered webhooks that can receive alarms.
When a registered webhook URL is required, click **Copy URL** on the right.

#### (2) Register
![management_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_05_en_240103.png)
Click **Register** to register webhook information from external systems.
Currently, only Dooray and Slack can be registered, and the list is to be added at the request.

#### (3) Retrieve/Modify/Delete Details
![management_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_06_en_240103.png)
Click each item to retrieve details.
To change registered information, click **Modify**. You may click to **Delete** a webhook when it is not required.

### Recipient List

You can set the user to receive notifications. To register a new member, they must be added in Manage Members menu of the NHN Cloud  project.
![management_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_07_en_240103.png)
In Gamebase, you can use the email or SMS to send the notification.
Both emails and SMS are sent out using the information that was entered when joining NHN Cloud, and member who registered invalid email address or phone number may not receive the notification. The mobile phone number is found in the **Manage My Information** page of the NHN Cloud.


## Config

You can set the integration between Gamebase and NHN Cloud service.

![management_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Management/en/management_08_en_240103.png)

You can choose to receive the information that was set in the NHN Cloud Launching when calling the Gamebase Launching API. Only the users of the NHN Cloud Launching service can toggle the function to On or Off.
