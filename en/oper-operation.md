## Game > Gamebase > Console Guide > Operation

This menu provides functions that are required for an app operation.

* Maintenance: Manage app maintenance
* Notice: Manage urgent pop-up notices for game users

## Maintenance

![gamebase_op_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_op_01_201812_en.png)

Can easily register maintenance in the Console, when required.
Retrieve maintenance history of registered apps and check progress at a glance, and search maintenance by registered causes of maintenance.
Status of maintenance is classified into five as below.

(1) In Reservation: Maintenance has been reserved
(2) In Progress: Maintenance is underway
(3) Completed: Maintenance time is over.
(4) Off: Operator has called for **Off** while maintenance is underway
(5) Off (Expired): When maintenance time is over during **Off**

Gamebase provides maintenance pop-ups and detail pages to show to game users while maintenance is underway.
Default maintenance pop-up of Gamebase.
![gamebase_op_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_02_201812.png)
Default maintenance page of Gamebase (with cause and time of maintenance)
![gamebase_op_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_03_201812.png)

### Register Maintenance

Click **Register** under the **Maintenance** tab, to register maintenance.

![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_op_04_202004.png)

>  <font color="red">[Caution] </font>When **Update Required and Maintenance are set at the same time** , the service status becomes 'Update Required'.
>  If you don't want to show the Update Required pop-up to user during maintenance, the service status should be changed to 'Update Required' after maintenance is completed.

#### (1) Target
Select a target of maintenance.

- All Games : When maintenance is required for all client versions.
- Some Clients: When only a particular client version requires maintenance. Click 'Select a Version' to show the list of client versions registered in the client menu.
  **[Example of selecting particular clients]**
  Can select All for client's status and each store; select a client version for maintenance and press OK.
  ![gamebase_op_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_op_05_201812_en.png)

#### (2) Reason
Provide reasons for maintenance simply.
This message is not shown to game users.

#### (3) Time
Set time of maintenance.
For a timezone, 'UTC+09:00' is selected as default, and maintenance can be registered by selecting a timezone of a serviced country.

#### (4) Maintenance Page
Choose one, out of **Gamebase Page (Webview)**, **Custom Page HTML (Webview)**, and **External Link**, with different input windows displayed for each item.
Below describe additional input items for each page: click **Preview** to check your input messages.

##### 4-1) Gamebase Page (Webview)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_2.0.png)
This default maintenance page displays information entered by operator on the Webview page of Gamebase.
It is Useful when there is no additional maintenance page.
Enter **messages to show** to users when maintenance is underway.
Enter in many languages, including English, Japanese, and Chinese, and one of the registered languages is set as the 'default language'.
For users speaking other languages than registered, the 'default language' shall be applied. Click **+**on the right to add more languages, and if your option is not available, contact [Customer Center](https://alpha.toast.com/support/inquiry).
Click **Preview** to check the screen in 'default language'.

##### 4-2) Custom Page HTML (Webview)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_3.0.png)
The maintenance page is entered in HTML type by operator and provided to users.
Preview page is supported as well, based on HTML tag inputs.
Useful in making a maintenance page type as wanted.

##### 4-3) External Link
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_4.1.png)
If you own a maintenance page or template, the maintenance page can be linked to URL.
Preview of the URL to connect is also supported.
To be provided with maintenance information, choose **Provide Information** and enter messages in **Message**: maintenance information registered in Gamebase (maintenance time, messages, and etc.) can be provided on the maintenance page.
Maintenance parameters are as follows: all delivered with URL encoded.

- message: Maintenance messages in the language as set in device information. For preview, deliver default messages.
- timezone: Standard time zone selected for maintenance registration. e.g) Delivery value for UTC+0: +09:00
- beginDate:  Start time entered for maintenance registration
- endDate:  End time entered for maintenance registration.

#### (5) Popup Messages 
Set a message to show for maintenance. 
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

### Modify Maintenance

Can check, modify, and delete details of registered maintenance.
Input items are same as Registration page, and Delete button is also available.
To register maintenance again with similar content, you may copy and paste for an easy registration.

## Notice

![gamebase_op_06_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_op_06_201812_en.png)

Provides pop-up notifications during app execution. The pop-ups will show before logins; in case of errors in external authentication or game server, pop-ups need to be registered.
Can easily check the list of registered notifications with status and search messages.
Status of notice is classified into three as below.

(1) Expected: Notice is expected to show
(2) In Progress: Notice is now showing
(3) Completed: Notice time has been completed

### Register Notice

Click 'Register' on the main screen to register notices.
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_op_07_202004.png)

#### (1) Target

Select a target to show notification.
- All Games: When maintenance is required for all client version.
- Some Clients: When only a particular client version requires maintenance. Click 'Select a Version' to show the list of client versions registered in the client menu.
  **Example of selecting particular clients**
  Can select a client status and all for each store, and select a client version for maintenance and press OK.
  ![gamebase_op_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_op_05_201812_en.png)


#### (2) Target Country
Select a country to show notification.

- All Countries : Show to all users
- Some Countries: Show to users of a selected country only.
  Enter a country code to add and it will be automatically completed and entered. If a country code you want des not exist, contact [Customer Center](https://toast.com/support/inquiry).

> [Note]
> Criteria of Country Selection
> Notice shows on the basis of user's **USIM Country Code** ; when USIM is not available, it shows based on countries set on **Device**.

#### (3) Number of Impressions
Select a number of times a notice shows to users.

- One impression during the impression period: Shows one time during impression period
- Always show when you start an app: Shows every time user starts an app during impression period

#### (4) Time
Set time to show notices.
For a timezone, 'UTC+09:00' is provided as default, and maintenance can be registered by selecting a timezone of a country at service.

#### (5) Message
Enter notice messages to show to users.
Can register many languages, for those who speak other languages than registered, a default language will show.
To add a language, click **+** on the right, and if a language you want is not on the list, contact [Customer Center](https://toast.com/support/inquiry)to add as required.
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

#### (6) Bottom Button Type
Specify a type of button to show at the bottom of a notice pop-up.

- Close : Show Close button only.
  Click 'Close' to close a pop-up and play games.
- Close + More: Show 'Close' and 'More'.
  Click 'More' and the link entered in the console opens on WebView.

#### Example of a Notice Pop-up
Close (left), Close+More (right)
![gamebase_op_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_08_201812.png)

### Modify Notice

Can check, modify, and delete details of notification.
Input items are same as registration, and Delete button is provided to delete a notice.
To register a notice again with similar content, you may copy and paste for an easy registration.
