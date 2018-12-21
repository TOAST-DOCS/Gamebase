## Game > Gamebase > Console Guide > App

Go to the TOAST Cloud Console and click **Game > Gamebase > App**.

* **App** : Manage app information
* **Client** : Manage client version and status information
* **Install URL** : Manage installation URL of each app store


## App

When Gamebase is activated, app is automatically created, and only registered information can be modified in each menu.
Cannot register a new app or delete one, as each TOAST Cloud project can manage one Gamebase app. Deactivate Gamebase to delete the app.
For details of each item, refer to **Properties** as below.

![gamebase_app_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_01_201812_en.png)

### Properties

Describes app information managed by Gamebase Console.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.2.png)

#### (1) Install URL
Refers to short URL information to install and patch an app.
Can manage apps that are distributed in many stores, with a short URL.
For detailed operations and management, refer to the following link: [Manage Installation URL](./oper-app/#installed-url)

> [Note] 
> Cannot modify URL, as it will be automatically created, once Gamebase is activated.

####(2) Server URL
Applied when a game requires server address (such as IP, URL) in real time.
When it is set, information can be checked on 'Launching Information' which is entered after client initialization.
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

Click **Register** to find a screen for registration of test devices: enter Device Key directly or search a **game user ID** to register.

![gamebase_app_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_03_201812_en.png)

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

- [Apple Developer 사이트](https://developer.apple.com/)
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
* **TOAST Console > Gamebase > App > 인증 정보 > 추가 정보 & Callback URL**의 **추가 정보** 항목에 JSON String 형태의 정보를 설정해야합니다.
	* NAVER의 경우, 로그인 동의 창에 표시할 앱 이름인 **service_name**을 설정해야 합니다.

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

##### iOS

 > <font color="red">[주의]</font><br/>
 >
 > Gamebase iOS SDK 1.14.0 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
 >

 * 1.13.0 이하
	* 별도의 URL Scheme 설정이 필요하지 않습니다.

* 1.14.0 이상
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.

* Twitter 추가 인증 정보 입력 예제

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
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
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
>  
>  <font color="orange">[참고] </font>
>  업데이트 버튼을 누르면 설치 URL 메뉴에서 설정한 각각의 스토어 주소로 연결됩니다.
>  예를 들면 클라이언트가 App store로 설정되어 있고 설치 URL 메뉴에서 App store 관련 설정이 존재한다면 설정한 주소로 이동되며 만약 설치 URL 메뉴에 설정이 되어 있지 않을 경우 공통(Common) URL로 연결됩니다.


- <font color="white" style="background-color:#CCCCCC">Close</font>: Service unavailable. <br/> To be selected for a version of closed service.<br />Gamebase provides the default pop-up as below.


> [Note] 
> Setting Message for Service Status.
> For **Update Recommended (In Service)**, **Update Required** , and **Service Closed** , messages can be set in multiple languages. 
> When a service status is selected, default messages are provided in five languages (Korean, English, Japanese, Chinese Simplified, and Chinese Traditional), and you can add a language or change messages. 
> ![gamebase_app_18_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_18_201812_en.png)

#### (4) Server URL
Enter a server URL (IP, URL) that a client will use.
A server URL entered in the **App** tab will be applied for all clients; enter only when each client has a different server URL.

## Installed URL

![gamebase_app_19_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812_en.png)

* Manage store URL information to install a game.
* 클라이언트 상태 중 <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font> 또는 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font> 일 때 각각의 스토어 별로 제공할 주소들에 대한 값을 설정합니다.
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


