## Game > Gamebase > Console Guide > App

Go to the TOAST Cloud Console and click **Game > Gamebase > App**.

* **App** : Manage app information
* **Client** : Manage client version and status information
* **Install URL** : Manage installation URL of each app store


## App

When Gamebase is activated, app is automatically created, and only registered information can be modified in each menu.
Cannot register a new app or delete one, as each TOAST Cloud project can manage one Gamebase app. Deactivate Gamebase to delete the app.
For details of each item, refer to **Properties** as below.

### Properties

Describes app information managed by Gamebase Console.

![gamebase_app_01_201812.png](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_01_201911.png)

#### (1) Install URL
Refers to short URL information to install and patch an app.
Can manage apps that are distributed in many stores, with a short URL.
For detailed operations and management, refer to the following link: [Manage Installation URL](./oper-app/#installed-url)

> [Note]
> Cannot modify URL, as it will be automatically created, once Gamebase is activated.

####(2) Server URL
Applied when a game requires server address (such as IP, URL) in real time.
When it is set, information can be checked on 'Launching Information' which is entered after client initialization.
Server addresses for delivery can be set depending on the client status. For instance, if the client is under testing or inspection, server address set for each item shall go down on the initial launching information.
Enter only when a game requires; otherwise, leave it empty.

####(3) Customer Center Info
Enter information such as email and phone number, other than Customer Center website.
When a website is available, enter information in **Service Center** of **In-app URL**.
> [Note] <br/>
> Information will be displayed on the detailed maintenance page provided by Gamebase.

####(4) Authentication Information
Can register, modify, or delete authentication information of IdP required for a login.

Can set callback URL and additional information, as well as client ID of external authentication and a secret key.
Click **+** button beside authentication information, to modify information.
For more configuration details of each IdP, refer to [Authentication Information](#authentication-information).
> [Note]
> What is **Token Re-validation**?
> Set whether to revalidate tokens for an external IdP when a client calls Latest Login API.
> Select **No Validation** , and only internal tokens will be validated, without token revalidation of external IdPs.
> Select **Always Validate** , and validity will be checked at all time, both for internal IdP tokens issued by Gamebase and external IdP tokens.


####(5) In-App URL
Can modify URLs that are frequently used in an app via console in real time, without having to redistribute the client.

- Terms of Use
- Personal Information Consent
- Punishment Provision
- Service Center

Enter only when a game requires; otherwise, leave it empty.
You can find your information setting in' Launching Information' after client initialization.

### Test Device

![gamebase_app_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_02_201812_en.png)
You can access games with a registered test device even when an app using Gamebase is under maintenance.
To register a test device, **Device Key** is required: enter directly or search a **Game User ID**.
Can even delete test devices that are no longer in use.

> [Note]
> You can register test devices up to the maximum of 100.

#### (1) Retrieve

Check the list of test devices registered in an app. Enter search words in **Search** to find the devices that fit your search conditions.

#### (2) Register

![gamebase_app_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_03_201812_en.png)

Click **Register** to find a screen for registration of test devices: enter Device Key directly or search a **game user ID** to register.

**(A) ** **Register by Game User ID**

Choose User ID  in Type, and enter User ID  and click Search, and the user login history will show. Select a Device Key to register as a test device,  enter **additional information**, and click Registration to complete registering the device key as test device.

> [Note]
> For additional information, enter user-defined names. e.g.) iPhone 6 Test, iPad of TOAST

**(B)Register by Device Key**

When you know the device key to register, select **Device Key** in Type to register the device.
Enter Device Key and **additional information**, and click **Register** to complete registration.

#### (3) Delete

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_4.0.png)

To delete test devices, go to the test device screen, check the devices to delete, and click **Delete** at the top left. Keep note that information cannot be restored, once deleted.

### Authentication Information

#### 1. Facebook
Enter {App ID} and {App Secret Code} of an app registered in the Facebook developer's site in the TOAST Cloud Gamebase Console.
Note that {Facebook Permission} which is required for a login should also be entered to Additional Info in the json string format.

**Entry Fields**

- Client ID: {AppID}
- Secret Key: {App Secret Code}
- Additional Info: Facebook Permission (json format)


![gamebase_app_04_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_04_201812.png)

**[Example] facebook_permission format **
- In case of Facebook OAuth, You should specify the information to request.

```json
{ "facebook_permission": [ "public_profile", "email"] }
```

![gamebase_app_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_05_201812_en.png)

**Reference URL**<br />

- [Facebook Developer's Site](https://developers.facebook.com/)
- [Facebook Permission](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
No further configuration needs to be done apart from TOAST console.

#### 2. Google

##### Google Cloud Console

![gamebase_app_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_06_201812.png)

1. You need to enter **Web Application Client ID** from Google Cloud Console into Gamebase Console for configuration.
2. Enter these URIs in the **Authorized redirect URIs** field.
   - https://alpha-id-gamebase.toast.com/oauth/callback
   - https://beta-id-gamebase.toast.com/oauth/callback
   - https://id-gamebase.toast.com/oauth/callback

##### iOS

> <font color="red">[Caution]</font><br/>
>
> URL Scheme configuration method has changed in Gamebase iOS SDK Version 1.12.2. Please make sure to check the version and follow the instruction accordingly.
>

- 1.12.1 or under
  - Provide **AdditionalInfo**.
    - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
    - You need to provide **url_scheme_ios_only** from iOS application for Google.
    - **url_scheme_ios_only** should match with at least one of the URL Schemes in Xcode.
  - Add URL schemes.
    - **XCode > Target > Info > URL Types**
- 1.12.2 or above
  - Add URL schemes.
    - Add `tcgb.{Bundle ID}.google` in **XCode > Target > Info > URL Types**.

- - Google authentication configuration example

```json
{ "url_scheme_ios_only": "Your URL Schemes" }
```

![gamebase_app_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 3. Apple Game Center
Enter Bundle ID registered on Apple Developer's Site in the TOAST Cloud Gamebase Console.

**Entry Fields**<br />

- ClientID: {Bundle ID}<br />

![gamebase_app_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_08_201812.png)


**Reference URL**<br />

- [Apple Developer Site](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)

#### 4. PAYCO
Enter {client_id} and {client_secret} issued from PAYCO ID application in the TOAST Cloud Gamebase Console.

**Entry Fields**<br />

- ClientID: {Payco client_id}
- Secret Key: {Payco client_secret}
- Additional Information: Payco Service & Service Name (JSON format)

##### Android & Unity

- Provide **AdditionalInfo**.
  - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information**.
  - You need to configure **service_code** and **service_name** that Payco SDK asks for.

- PAYCO authentication configuration example

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[Caution]</font><br/>
>
> URL Scheme configuration method has changed in Gamebase iOS SDK Version 1.12.2. Please make sure to check the version and follow the instruction accordingly.

- 1.12.1 or under
  - Provide **AdditionalInfo**.
    - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information**.
    - You need to configure **service_code** and **service_name** that Payco SDK asks for.
- 1.12.2 or above
  - Provide **AdditionalInfo**.
    - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
    - You need to configure **service_code** and **service_name** that Payco SDK asks for.
  - Add URL schemes.
    - Add `tcgb.{Bundle ID}.payco` in **XCode > Target > Info > URL Types**.

- - PAYCO authentication configuration example

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)


#### 5.NAVER

Enter {client_id} and {client_secret} you requested and acquired from NAVER Developers website into the Gamebase console.
You need to set **service_name** which will be displayed in sign in agreement window. In case of iOS, you need to provide additional **url_scheme_ios_only** field in JSON String format.

**Entry Fields**

- Client ID: {NAVER client_id}
- Secret Key: {NAVER client_secret}
- Additional Info: NAVER Application Name & iOS url scheme (json format)

**Reference URL**<br />
- [NAVER Developers - Register Applications](https://developers.naver.com/apps/#/register)
- [NAVER Developers - Check Client IDs and Client Secrets](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Android & Unity
* You need to provide JSON string data in **Additional Info** at **TOAST Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
	* For NAVER, you need to set the app name **service_name** which will be displayed in login agreement window.

```json
{"service_name": "Your Service Name" }
```

##### iOS

> <font color="red">[Caution]</font><br/>
>
> URL Scheme configuration method has changed in Gamebase iOS SDK Version 1.12.2. Please make sure to check the version and follow the instruction accordingly.

- 1.12.1 or under
  - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
    - In case of NAVER, you need to provide **service_name** to be displayed in sign-in agreement window.
    - You need to provide **url_scheme_ios_only** for iOS.
  - Add URL Schemes.
    - **XCode > Target > Info > URL Types**
- 1.12.2 or above
  - You need to provide JSON string data in **TOAST Console > Gamebase > App > Authentication Information > Additional Info & Callback URL**.
    - In case of NAVER, you need to provide **service_name** to be displayed in sign-in agreement window.
  - Add URL Schemes.
    - Add `tcgb.{Bundle ID}.naver` in **XCode > Target > Info > URL Types**.

- NAVER authentication configuration example

```json
{ "url_scheme_ios_only": "Your URL Schemes", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 6. Twitter

You need to provide {Consumer Key} and {consumer Secret} acquired from Twitter Applicaiton Management Website.

**Input Fields**

- Client ID: {Twitter Consumer Key}
- Secret Key: {Twitter Consumer Secret}

**Reference URL**
- [Twitter Application Management](https://apps.twitter.com/)

##### Android
> <font color="red">[Caution]</font><br/>
>
> As of July 25 of 2019, Twitter has suspended its support for TLS 1.0 and TLS 1.1, to support TLS 1.2 only.  Hence, Android 4.3 (Jellybean, API Level 18) or lower devices do not support logins to Twitter via Android WebView.
>
> In short, only Android 4.4 and higher devices (KitKat, API Level 19) allow logins to Twitter.

##### iOS

 > <font color="red">[Caution]</font><br/>
 > In the Gamebase iOS SDK Version 1.14.0, URL scheme setting has been changed. See the guide appropriate for your SDK version.
 >

 * 1.13.0 or earlier
	* You don't need to set the URL scheme separately.

* 1.14.0 or later
	* Set the URL scheme.
		* **tcgb.{Bundle ID}.twitter** must be added to **XCode > Target > Info > URL Types**.

* Example of entering the additional authentication information for Twitter

![Twitter URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 7. LINE

**Input Fields**

- Client ID: {LINE Channel ID}
- Secret Key: {LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS

You need additional configuration in order to user LINE Login.

- Add URL Schemes.

  - Add `line3rdp.{App Bundle ID}` into **XCode > Target > Info > URL Types**.

- Configure **Info.plist**
  - Set **ChannelID** acquired from LINE.

  ```xml
  <key>LineSDKConfig</key>
  <dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
  </dict>
  ```

  - Add scheme for ATS setting.

  ```xml
  <key>LSApplicationQueriesSchemes</key>
  <array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
  </array>
  ```

- For proejct settings in LINE Login, please refer to the link below.

- [LINK \[LINE Developer Guide\]](https://developers.line.me/en/docs/ios-sdk/try-line-login/)

#### 8. Sign In with Apple
To enable Sign In with Apple, setting is required for AppStore Connect, Gamebase Console and Xcode.

##### AppStore Connect Settings
* [Direct Link to Certificates, Identifiers & Profiles \> Keys](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > Add (+)
1. Check `Sign In with Apple` and process the setting.
![Check SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. Select Bundle ID to enable`Sign in with Apple`.
![ChooseAPrimaryAppID](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. Download <span style="color:#e11d21">Privatekey</span> and check <span style="color:#e11d21">Key ID</span> which is created and saved.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Go to Certificates, Identifiers & Profiles > Identifiers > Select Target Apps and enable  `Sign In with Apple`.
    * Set `Enable as a primary App ID`.
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[Direct link to TOAST Console](https://console.toast.com/)

* Gamebase
![Set SecretKey Setting](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid4_1.0.png)


###### Setting Client ID
> Set Bundle ID for the app.

###### Setting Secret Key
> Use **TeamID**, **KeyID**, or **PrivateKey** that are acquired from Apple Developer Account setting to create JSON character strings.  

* `teamId`: Set value on top right of the developer account.
* `keyId`: Go to Certificates, Identifiers & Profiles > Keys, and check Sign In with Apple to set created value.
![SecretKey Setting](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid5_1.0.png)
* `privateKey`: Set PrivateKey file which is created along with the Keys. (Open the downloaded file and apply the value in red rectangle like the screen shot as below.)
![SecretKey Setting](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

Create the above value into JSON like the example as below.  


```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. omits"
}
```

###### Setting Additional Info
[Getting to know AuthorizationScope of Sign In With Apple](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

By adding Apple on Gamebase Console > App, JSON is set as default.
As of today (November of 2019), there are only two types of scope, such as `full_name` and `email`, and Gamebase has the two as default.

```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[Caution]</font><br/>Only Xcode 11 or higher versions allow 'Sign In with Apple' for the  buildup of projects.   


1. Select Target > Signing & Capabilities,  and add Sign In with Apple.
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. Select Target > Build Phases > Link Binary With Libraries, and add Authentication.framework as **Optional**.
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)
    - ```Caution```: If the setting is not Optional but Required, you may encounter runtime crash on iOS 11 or lower-version devices.


## Client

Can manage client information by operating system (iOS, Android, Unity WebGL, or Unity Standalone), or version.

### Client List

![gamebase_app_12_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_12_201812_en.png)

Can check the list of currently-registered clients by operating system.
The number of icon indicates the version entered when registering a client.
The icon list shows service status, such as <font color="white" style="background-color:#F8BB28">Test</font>, <font color="white" style="background-color:#FB8F37">Review</font>, <font color="white" style="background-color:#88C637">Service</font>, <font color="white" style="background-color:#2AB1A6">Update Recommended(in service)</font>only. Click the arrow key at the bottom right to check the list of clients, in such status as, <font color="white" style="background-color:#A1A1A1">Update required</font>, <font color="white" style="background-color:#CCCCCC">Close</font>.
The color of an icon helps identify service status at a glance.

### Properties

Describes client registration information managed by the Gamebase Console.
In the **Client** tab, click **Register AOS** , **Register iOS** and a screen for client registration will show. To modify or delete client's entry value, click an icon from the icon list or select a client from the entire list.

![gamebase_app_13_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_13_201812_en.png)

#### (1) Store

(<font color="red">Required</font>) Select a store to distribute a client.
Each operating system has a different store.

#### (2) Client Version

(<font color="red">Required</font>) Enter a client version.
Enter character strings in accordance with rules of each game.

#### (3) Service Status

(<font color="red">Required</font>) Select a service status of a client, out of 6:
<font color="white" style="background-color:#F8BB28">Test</font>, <font color="white" style="background-color:#FB8F37">Review</font>, <font color="white" style="background-color:#88C637">Service</font>, <font color="white" style="background-color:#2AB1A6">Update Recommended(in service)</font>, <font color="white" style="background-color:#A1A1A1">Update required</font>, <font color="white" style="background-color:#CCCCCC">Close</font>.

- <font color="white" style="background-color:#F8BB28">Test</font>: Internal test is underway
- <font color="white" style="background-color:#FB8F37">Review</font>: Store is under evaluation. Can add setting of Stabilization Index.
  ![gamebase_app_14_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_14_201812_en.png)

> [Note]
> What is 'Stabilization Index'?<br/>
> It is an indicator log to call a Gamebase API within Gamebase.
> When it is activated during 'Test', the indicator helps detect problems more at ease when an issue arises in Gamebase.
> Its use can be set in the Gamebase Console in real time, along with the log level.

![gamebase_app_15_201812](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

- <font color="white" style="background-color:#88C637">Service</font>: Service is normally provided
- <font color="white" style="background-color:#2AB1A6">Update Recommended (In Service)</font>: Service is normally provided. <br/>Pop-up will show to lead into more stable versions. Although downloading a newer version is recommended, users can continue playing games with a current version.<br />Gamebase provides the default pop-up as below.

- <font color="white" style="background-color:#A1A1A1">Update Required</font>: Service is unavailable. <br/>As the current version is not supported by game, a pop-up will show to update app.<br />Gamebase provides the default pop-up as below.

>  <font color="red">[Caution] </font>
>  When **both Update Required and Under Maintenance are set at the same time** , the service status becomes 'Update Required'.
>  If you don't want to show the Update Required pop-up to user during maintenance, the service status should be changed to 'Update Required' after maintenance is completed.

- <font color="white" style="background-color:#CCCCCC">Close</font>: Service unavailable. <br/> To be selected for a version of closed service.<br />Gamebase provides the default pop-up as below.

> [Note]
> Setting Message for Service Status.
> For **Update Recommended (In Service)**, **Update Required** , and **Service Closed** , messages can be set in multiple languages.
> When a service status is selected, default messages are provided in five languages (Korean, English, Japanese, Chinese Simplified, and Chinese Traditional), and you can add a language or change messages.
> ![gamebase_app_18_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_18_201812_en.png)

#### (4) Server URL
Enter a server URL (IP, URL) that a client will use.
A server URL entered in the **App** tab will be applied for all clients; enter only when each client has a different server URL.

#### (5) Debug log
You can change whether to display debug logs of Gamebase SDK in the console in real time.
If it is not set, the default value set in the Gamebase SDK will be used. You can set whether to display debug logs in the console or not.
Even if the debug log is 'OFF' in the Gamebase SDK, the Gamebase debug logs are displayed on the device if you set debug log to 'ON' for the console.

## Installed URL

Manage store URL information to install a game.

![gamebase_app_19_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812_en.png)

Set the value of address to be provided by store when the client status is   <font color="white" style="background-color:#2AB1A6">Update is recommended(in service )</font> or <font color="white" style="background-color:#A1A1A1">update is required</font>.

* A user's click on a short URL via PC or mobile will be redirected to a site entered on a user device (device, operating system, store, etc.).
* If there is no store information, or redirection is failed, the URL will be linked as set in 'COMMON'.

_[Example 1] A user clicks Install URL in a text message on an Android device._
**(Device:mobile,OS:Android,Store:N/A)** Move to a mobile URL of a representing Android store. In the case of 'Google Play', move to the URL set on the 'Google Play' mobile.
_[Example 2] A user playing a game downloaded from 'One Store' clicks 'Update Now' in the 'Update Required' pop-up._
**(Device:mobile,OS:Android,Store:One Store)** Move to the URL set in 'One Store' mobile (installation page for One Store mobile)<br/>
_[Example 3] A user enters Install URL on a PC._
**(Device:PC,OS:Windows,Store:N/A)** Move to URL set in COMMON PC.

### Properties

To modify Install URL, click **Modify**.

![gamebase_app_20_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_20_201812_en.png)

- Item setting can be different for PC and mobile. Enter the same value for each device, if there is no need to separate.
- When a store you want is not on the list, contact [CustomerCenter](https://toast.com/support/inquiry) so as to add as required.

#### (1) Common
Set an URL to connect when store information is unavailable or store relocation has failed.

#### (2) Android
Set an URL to connect at the request of Android users.

#### (3) iOS
Set an URL to connect at the request of App Store users.

#### (4) Standalone
Set an URL to connect from an app of standalone service. Standalone needed PC URL only.

## Transfer account
Transfer account allows a game user logged in as Guest to continue the game on another device without using another ID provider.
You can change the game device just by getting the transfer key from the current device where the game is being played and entering the key on another device.
**Transfer Device** function is disabled by default. To use this, click **Enable** on **Transfer Device**.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_1.0.png)

Click the **Enable** button and then enter the information required for transferring to the device.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_2.0.png)
Description on each item is as follows:

#### Issue
Sets the format of a key issued for device transfer.
The key for device transfer can be either ID only or both ID and password. Format of ID and password can be a combination of the desired lowercase letters, uppercase letters, and numbers.

1. **Auto ID Issue Format**: Sets the format of issuing a device transfer ID. The options to be set are as follows:

- **Number(Minimum length: 12)**: Issues an ID with numbers only. The minimum length of the issued ID is 12 characters.
- **Number+Lowercase(Minimum length: 10)**: Issues an ID with numbers and lowercase letters. The minimum length of the issued ID is 10 characters.
- **Number+Uppercase(Minimum length: 10)**: Issues an ID with numbers and uppercase letters. The minimum length of the issued ID is 10 characters.
- **Number+Lowercase+Uppercase(Minimum length: 9)**: Issues an ID with numbers, lowercase letters, and uppercase letters. The minimum length of the issued ID is 9 characters.
- **Lowercase+Uppercase(Minimum length: 9)**: Issues an ID with lowercase letters and uppercase letters. The minimum length of the issued ID is 9 characters.
2. **Password auto issue format**: Sets the format for issuing the password to be used for login with the ID transferred to the device. The options to be set are as follows:
- **Disable Password**: Select this not to use a password. If you select this option, you can set the valid time of the ID only from the verification below.
- **Number(Minimum length: 12)**: Issues a password with numbers only. The minimum length of the issued password is 12 characters.
- **Number+Lowercase(Minimum length: 10)**: Issues a password with numbers and lowercase letters. The minimum length of the issued password is 10 characters.
- **Number+Uppercase(Minimum length: 10)**: Issues an ID with numbers and uppercase letters. The minimum length of the issued password is 10 characters.
- **Number+Lowercase+Uppercase(Minimum length: 9)**: Issues an ID with numbers, lowercase letters, and uppercase letters. The minimum length of the issued password is 9 characters.
- **Lowercase+Uppercase(Minimum length: 9)**: Issues an ID with lowercase letters and uppercase letters. The minimum length of the issued password is 9 characters.

#### Verification
Sets the verification conditions of the issued device transfer key.
For verification of the issued device transfer, you can set the transfer count, expiration date, block upon failure, etc.
3. **Device transfer count**: Sets the count of allowed number of device transfers for the issued ID. You can select either Unlimited or One-time.
4. **Expiration date**: Sets the expiration date of the issued account. The issued device transfer ID is affected by this value. You have to select either Unlimited or Set Period.
5. **Block reverification upon failure**: When a user tried logging in but failed, the account is blocked for a certain period. If you select this option, additional settings will be displayed.
6. **Login attempt count until blocking**: Displayed when **Block reverification upon failure** is selected. The account is blocked when login verification is failed for the specific count entered here. The value must be 1 or above.
7. **Block period**: Sets the period of blocking time that must be passed to unblock the account for retry. Select **PERMANENT** or **Specify Period**.If you select **Specify Period**, you can set the desired block hours and minutes.

#### After initial setting completed
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
Once the initial settings are done, game users can disable the device transfer function only if they want. If they need to change the settings, they need to contact Customer Center.
Click **Disable** to disable the function. In this case, all device transfer keys issued will be deleted. So be careful when you should determine whether to disable the function or not.
