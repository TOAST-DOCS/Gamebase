## Game > Gamebase > Operator Guide > App
Basic app information can be set in the **App** menu. Go to the TOAST Cloud Console and click **Game > Gamebase > App**.

* **App**: Manage app information
* **Client**: Manage client version and status information
* **Install URL**: Manage installation URL of each app store


## App

When a Gamebaes product is activated, app is automatically created, and only registered information can be modified in each menu. <br/>
Cannot register a new app or delete an app, as each TOAST Cloud project can manage one Gamebase app. Deactivate a Gamebase product to delete an app. <br />
For details of each item, refer to **Properties** as below. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)

### Properties

Describes information of apps managed by Gamebase Console. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.1.png)

####(1) Install URL
Refers to short URL information to install and promote an app. <br/>
Can manage apps that are distributed to many stores, with a short URL. <br/>
For detailed operation and management methods, refer to the following link. [Installed URL](./app/#installed-url) <br />

> [Note] <br/>
> Once Gamebase is activated, modification is unavailable as app is automatically created.<br/>

####(2) Server URL
Applied when a game requires server address (such as IP, URL) in real time.<br/>
When it is set, information can be checked on 'Launching Information' which is entered after client initialization. <br/>
Enter only when a game requires; otherwise, leave it empty. <br />

####(3) Customer Center Information
Enter information such as email and phone number, other than Customer Center website.<br />
When a website is available, enter information in **Customer Center** of **In-app URL**.<br />
> [Note] <br/>
> Information input will be displayed on the detailed maintenance page provided by Gamebase.<br />

####(4) Authentication Information
Can register, modify, or delete authentication information required for a login. <br />

Can set callback URL and additional information, as well as client ID of external authentication and a secret key. <br/>
Click the **+** button beside authentication information, to modify information.
For more details of each IdP, refer to [Authentication Information](#authentication-information).<br />
> [Note] <br/>
> What is **Token Re-validation**?<br/>
> Set whether to revalidate tokens for an external IdP when a client calls Latest Login API. <br/>
> Select **No validation**, and only internal tokens will be validated, without token revalidation of external IdPs. <br/>
> Select **Always**, and validity will be verified at all time, both for internal IdP tokens issued by Gamebase and external IdP tokens.


####(5) InApp URL
Can modify URLs that are frequently used in an app via console in real time, without having to redistribute the client. <br/>

- Terms of Use <br/>
- Personal Information Consent <br/>
- Punishment Provision <br/> 
- Service Center URL <br/>

Enter only when a game requires; otherwise, leave it empty. <br />


####(6) Test Device
A device registered for testing can access a game, even when an app using Gamebase is under maintenance. <br />
To register a test device, enter **Device Key** or **User ID**. <br/>
**User ID** is provided for user convenience; **Device Key** of the latest login history will be automatically registered, along with the input of **User ID**. If a test device has been replaced after registration, a new device need to be registered.
<br />

### Authentication Information

#### Facebook
Enter {App ID} and {App Secret Code} of an app registered in the Facebook developer's site in the TOAST Cloud Gamebase Console.<br />
Note that {Facebook Permission} which is required for a login should also be entered to Additional Info in the Json string format.<br />

**Entry Fields** <br />

- ClientID: {AppID} <br />
- Secret Key: {App Secret Code} <br />
- Additional Info: Facebook Permission (json format) <br />


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

**[Example] facebook_permission format **<br />
'''json
{ "facebook_permission": [ "public_profile", "email", "user_friends"] }
'''

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

**Reference URL **<br />

- [Facebook Developer's Site](https://developers.facebook.com/)<br />
- [Facebook Permission](https://developers.facebook.com/docs/facebook-login/permissions/)<br />




#### Google
Enter {Client ID}, {Secret Key}, and {Redirection URI} of an app registered on the Google developer's console in the TOAST Cloud Gamebase Console. <br />

**Entry Fields**<br />

- ClientID: {Client ID}<br />
- Secret Key: {Secret Key}<br />
- Callback URL: {Redirection URI}
  <br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

#### Apple Game Center
Enter BundLeID registered on Apple Developer's Site in the TOAST Cloud Gamebase Console. <br />

**Entry Fields**<br />

- ClientID: {Bundle ID}<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

**Reference URL**<br />

- [Apple Developer's Site](https://developer.apple.com/)<br />
- [Apple iTunes Connect](https://itunesconnect.apple.com/)<br />

#### PAYCO
Enter {client_id} and {client_secret} issued from PAYCO ID application in the TOAST Cloud Gamebase Console. <br />

**Entry Fields**<br />

- ClientID: {Payco client_id}<br />
- Secret Key: {Payco client_secret}<br />





## Client

Can manage client information by each operating system (iOS, Android, Unity WebGL, or Unity Standalone), or version.<br />

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
Can check the list of clients currently registered.<br />
To be displayed for an operating system, and the number of icon indicates the version entered when registering a client. <br />
The icon list shows service status, such as 'Test', 'Evaluate', 'In Service', 'In Service (Update Recommended)' only. Click the arrow key at the bottom right to check client list in status such as, 'Update Required' or 'Service Closed'.<br />
Icons in different colors help easily identify service status. <br />

### Properties

Describes client registration information managed by the Gamebase Console.<br/>
In the **Client** tab, click **Register AOS**, **Register iOS** and a screen for client registration will show. To modify or delete client's entry value, click an icon from the icon list or select a client from the entire list. <br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) Store
(<font color="red">Required</font>) Select a store to distribute a client. <br/>
Each operating system has a different store. <br />
#### (2) Client Version
(<font color="red">Required</font>) Enter a client version. <br/>
Enter character strings in accordance with rules of each game. <br />
#### (3) Service Status
(<font color="red">Required</font>) Select a service status of a client.<br />
The status is classified into six, such as <font color="white" style="background-color:#F8BB28">Test</font>, <font color="white" style="background-color:#FB8F37">Evaluate</font>, <font color="white" style="background-color:#88C637">In Service</font>, <font color="white" style="background-color:#2AB1A6">Update Recommended (In Service)</font>, <font color="white" style="background-color:#A1A1A1">Update Required</font>, <font color="white" style="background-color:#CCCCCC">Service Closed</font>.

- <font color="white" style="background-color:#F8BB28">Test</font>: Internal test underway <br />
- <font color="white" style="background-color:#FB8F37">Evaluate</font>: Store under evaluation. <br/> Can add setting of stabilization indicator.<br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
> [Note]<br/>
> What is 'Stabilization Indicator'?<br/>
> It is an indicator log which is left in Gamebase when a Gamebase API is called. <br/>
> When activated during 'Evaluate', the indicator helps detect problems more at ease than Gamebase internal issues. <br />
> Use of stabilization indicator can be decided in the Gamebase Console in real time, along with the log level. <br />

- <font color="white" style="background-color:#88C637">In Service</font>: Service is normally provided <br />
- <font color="white" style="background-color:#2AB1A6">Update Recommended (In Service)</font>: Service is normally provided. <br/> Pop-up will show to lead into more stable versions. Although downloading a newer version is recommended, user can opt to stay with the current version.<br />Gamebase provides the default pop-up as below.<br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">Update Required</font>: Service is unavailable. <br/> As the current version is not supported by game, pop-up will show to update app. <br /> Gamebase provides the default pop-up as below. <br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[주의] </font>  <br/>
>  When **both Update Required and Under Maintenance is set at the same time**, the service status becomes 'Update Required'.<br/>
>  If you don't want the Update Required pop-up to be displayed to user during maintenance, the service status should be changed to 'Update Required' after maintenance is completed.<br/>

- <font color="white" style="background-color:#CCCCCC">Service Closed</font>: Service unavailable. <br/> To be selected for a version of closed service.<br />Gamebase provides the default pop-up as below.<br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [Note] <br/>
> Setting of Message for Service Status <br/>
> For **Update Recommended (In Service)**, **Update Required**, and **Service Closed**, messages can be set in multiple languages.<br />
> When a service status is selected, default messages are provided in five languages (Korean, English, Japanese, Chinese Simplified, and Chinese Traditional), and you can add a language or change messages. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)

#### (4) Server URL
Enter a server URL (IP, URL) that a client will use.<br />
A server URL entered in the **App** tab will be applied for all clients. So, enter each server URL for different clients. <br />


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.1.png)

Manage store URL information to install a game.<br/>
A user's click on a short URL via PC or mobile will be redirected to a site entered on a user device (device, operating system, store, etc.). <br/>
If there is no store information or redirection is failed, user will go to the URL set in 'COMMON'.<br/><br/><br/>

_[Example 1] A user clicks Install URL in a text message on an Android device_<br/>
**(Device:mobile,OS:Android,Store:N/A)** Move to a mobile URL of a representing Android store. In the case of 'Google Play', move to the URL set on the 'Google Play' mobile. <br/>
_[Example 2] A user playing a game downloaded from 'One Store' clicks 'Update Now'in the 'Update Required' pop-up._<br/>
**(Device:mobile,OS:Android,Store:One Store)** Move to URL set on the 'One Store' mobile (Installation page of One Store mobile)<br/>
_[Example 3] A user enters Install URL on a pc_<br/>
**(Device:PC,OS:Windows,Store:N/A)** Move to URL set in COMMON PC <br/>


### Properties

To modify information of entered Install URL, click **Modify**.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.1.png)

- Allows separate settings of item for PC and mobile. If there is no need to separate, enter the same value for each. <br />
#### (1) Common
When store information is unavailable or store relocation has failed, set an URL to connect.<br />
#### (2) Android
At the request of Android users, set an URL to connect. <br />
When a store you want is not on the list, contact a person in charge to add as required. <br />
#### (3) iOS
At the request of App Store users, set an URL to connect. <br />
#### (4) WebGL
Set an URL to connect from WebGL internally.<br />
#### (5) Standalone
Set an URL to connect from an app of standalone service. <br />
