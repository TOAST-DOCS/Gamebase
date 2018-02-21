## Game > Gamebase > Operator Guide > Operation
Provides functions that are required for an app operation. <br/>

* Maintenance: Manage app maintenance
* Notice: Manage urgent pop-up notices for game users

## Maintenance


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance1_1.1.png)

Can easily register maintenance, when required, on the console.<br/>
Retrieve maintenance history of registered apps and check progress at a glance, and search maintenance by registered causes of maintenance.<br />
Status of maintenance is classified into five as below. <br />

(1) In Reservation: Maintenance has been reserved <br />
(2) In Progress: Maintenance is underway <br />Default maintenance pop-up of Gamebase
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_popup_1.0.png)
Default maintenance page of Gamebase (displays reasons and time of maintenance)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_webview_1.1.png)
(3) Completed: Maintenance time is closed <br />
(4) Off: Operator has called for **Off** while maintenance is underway <br />
(5) Off (Expired): When maintenance time is closed when it is **Off** <br />


### Register Maintenance

Click **Register** on the **Maintenance** tab, to register maintenance.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_1.3.png)

>  <font color="red">[Caution] </font> When **Update Required and Maintenance is set at the same time**, the service status becomes 'Update Required'.<br/>
>  If you don't want the Update Required pop-up to be displayed to user during maintenance, the service status should be changed to 'Update Required' after maintenance is completed.<br/>

#### (1) Target
Select a target of maintenance.<br />

- All Games: When maintenance is required for all client versions.
- Some Clients: When only a particular client version requires maintenance. Click 'Select a Version' to show the list of client versions registered in the client menu. <br/>
  **[Example of selecting particular clients]**<br/>
  Can select a client status and all for each store, and select a client version for maintenance and press OK.<br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)


#### (2) Time
Set time of maintenance.<br />
For a timezone, 'UTC+09:00' is provided as default, and maintenance can be registered by selecting a timezone of a country at service. <br />

#### (3) Message
Set messages to show to user during maintenance.<br />
Can register many languages, for those who speak other languages than registered, a default language will show. To add a language, click **+** on the right, and if a language you want is not on the list, contact [Customer Center](https://cloud.toast.com/support/faq) to add as required.<br />

#### (4) URL Maintenance Page URL
Set a maintenance page to be impressed to game users.<br />

- Not Use: Default maintenance page of Gamebase will be applied.<br />
- Use: Enter a maintenance page URL made by game.<br />


### Modify Maintenance
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance3_1.1.png)

Can check, modify, and delete details of registered maintenance. <br />
Input items are same as registration, and Delete button is provided to delete maintenance. <br />
To register maintenance again with similar content, you may copy and paste for an easy registration.<br />

## Notice

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice1_1.2.png)

Provides pop-up notifications during app execution. The pop-ups will show before logins; in case of errors in external authentication or game server, pop-ups need to be registered.<br/>
Can easily check the list of registered notifications with status and search messages.<br />
Status of notice is classified into three as below. <br />

(1) Expected: Notice is expected to show <br />
(2) In Progress: Notice is now showing <br />
(3) Completed: Notice time has been completed <br />

### Register Notice

Click 'Register' on the main screen to register notices.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice2_1.0.png)

#### (1) Target

Select a target to show notification.<br />

- All Games: When maintenance is required for all client version.
- Some Clients: When only a particular client version requires maintenance. Click 'Select a Version' to show the list of client versions registered in the client menu. <br />
  **Example of selecting particular clients** <br/>
  Can select a client status and all for each store, and select a client version for maintenance and press OK.<br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)


#### (2) Target Country
Select a country to show notification.<br />

- All Countries: Show to all users
- Some Countries: Show users of a selected country only. <br/>
 Enter a country code to add and it will be automatically completed for input. If a country code you want does not exist, contact administrator.

> [Note]<br/>
> Criteria of Country Selection <br/>
> Notice shows to countries, on the basis of user's **USIM Country Code**; when USIM is not available, countries set on **Device** are the target.<br />

#### (3) Number of Impressions
Select a number of times a notice shows to users.<br />

- One impression during the impression period: Shows one time during impression period
- Always show when you start an app: Shows every time user starts an app during impression period

#### (4) Time
Set time to show notices. <br />
For a timezone, 'UTC+09:00' is provided as default, and time can be registered by selecting a timezone of a country at service.<br />

#### (5) Message
Enter notice messages to show to users.<br />
Can register many languages, for those who speak other languages than registered, a default language will show. To add a language, click **+** on the right, and if a language you want is not on the list, contact administrator to add as required.<br />


#### (6) Bottom Button Type
Specify a type of button to show at the bottom of a notice pop-up.<br />

- Close: Show Close button only. <br/>
  Click 'Close' to close a pop-up and play games. <br />

- Close + More: Show 'Close' and 'More'.<br/>
  Click 'More' and the link entered in the console will open on WebView. <br />


#### Example of a Notice Pop-up
(1) Close
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_1.1.png)
(2) Close + More
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_detail_1.0.png)

### Modify Notice
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice3_1.1.png)

Can check, modify, and delete details of notification.<br />
Input items are same as registration, and Delete button is provided to delete a notice.<br />
To register a notice again with similar content, you may copy and paste for an easy registration..<br />
