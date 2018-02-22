## Game &gt; Gamebase &gt; Operator Guide &gt; App

Basic app information can be set in the **App** menu. Go to the TOAST Cloud Console and click **Game &gt; Gamebase &gt; App**.

- **App** : Manage app information
- **Client** : Manage client version and status information
- **Install URL** : Manage installation URL of each app store


## App

When Gamebase is activated, app is automatically created, and only registered information can be modified in each menu.<br/>
Cannot register a new app or delete one, as each TOAST Cloud project can manage one Gamebase app. Deactivate Gamebase to delete the app.<br />
For details of each item, refer to **Properties** as below <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.2.png)

### Properties

Describes app information managed by Gamebase Console.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App2_1.2.png)

#### (1) Install URL
Refers to short URL information to install and patch an app. <br/>
Can manage apps that are distributed in many stores, with a short URL. <br/>
For detailed operations and management, refer to the following link: [Manage Installation URL](./oper-app/#installed-url) <br />

> [Note] <br/>
> Cannot modify URL, as it will be automatically created, once Gamebase is activated.<br/>

####(2) Server URL
Applied when a game requires server address (such as IP, URL) in real time.<br/>
When it is set, information can be checked on &#39;Launching Information&#39; which is entered after client initialization.<br/>
Enter only when a game requires; otherwise, leave it empty.<br />

####(3) Customer Center Info
Enter information such as email and phone number, other than Customer Center website.<br />
When a website is available, enter information in **Service Center** of **In-app URL**.<br />
> [Note] <br/>
> Information will be displayed on the detailed maintenance page provided by Gamebase.<br />

####(4) Authentication Information
Can register, modify, or delete authentication information of IdP required for a login.<br />

Can set callback URL and additional information, as well as client ID of external authentication and a secret key. <br/>
Click **+** button beside authentication information, to modify information. For more configuration details of each IdP, refer to [Authentication Information](#authentication-information).

> [Note] <br/>
> What is **Token Re-validation**?<br/>
> Set whether to revalidate tokens for an external IdP when a client calls Latest Login API. <br/>
> Select **No Validation** , and only internal tokens will be validated, without token revalidation of external IdPs.<br/>
> Select **Always Validate** , and validity will be checked at all time, both for internal IdP tokens issued by Gamebase and external IdP tokens.


####(5) In-App URL
Can modify URLs that are frequently used in an app via console in real time, without having to redistribute the client.<br/>

- Terms of Use <br/>
- Personal Information Consent <br/>
- Punishment Provision <br/>
- Service Center <br/>

Enter only when a game requires; otherwise, leave it empty.<br />

Please note that the translation will be completed as soon as possible.
### Test Device

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_1.0.png)
테스트 단말기로 등록되면 Gamebase를 사용하는 앱이 점검 중이어도 정상적으로 게임에 접근할 수 있습니다.<br />
테스트 단말기를 등록하려면 **Device Key**를 입력해야 합니다. 직접 입력하거나 **유저 ID**를 조회하여 등록할 수 있습니다.<br/> 
더 이상 사용하지 않는 테스트 단말기 삭제도 가능합니다.<br />

> [참고] <br/>
> 테스트 단말기는 최대 100개까지만 등록 가능합니다.<br/>

#### (1) 조회

앱에 등록된 전체 테스트 단말기를 확인 할 수 있습니다. 검색어를 **Search**에 입력하여 검색 조건에 맞는 테스트 단말기를 찾아 볼 수 있습니다.

#### (2) 등록

조회 화면에서 **등록**버튼을 클릭하면 테스트 단말기 등록화면이 노출됩니다. 테스트 단말기는 **Device Key**를 직접 입력하거나 **유저 ID**검색으로 등록할 수 있습니다.

**(1) 유저 ID를 통한 등록**<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_2.1.png)

타입을 유저 ID로 선택하고 유저ID를 입력하여 **검색** 버튼을 클릭하면 화면 하단에 사용자의 로그인 로그 내역이 조회됩니다. 조회된 내역에서 테스트 단말기로 등록하고자 하는 Device Key를 선택하여 **추가정보**를 입력하여 **등록**버튼을 클릭하면 해당 Device key가 테스트 단말기 정보로 등록됩니다.<br />

> [참고] <br/>
> 추가 정보에는 사용자가 알아보기 편한 별칭을 입력하시면 됩니다. 예시) iphone6 테스트, 토스트님 아이패드<br/>

**(2)Device Key를 통한 등록**<br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_3.0.png)

등록하고자 하는 Device key를 알고 있을 경우 타입을 **Device Key**로 선택하여 직접 테스트 단말기를 등록할 수 있습니다.<br />
Device Key와 등록 단말기의 **추가정보**를 입력 후 등록 버튼을 누르면 테스트 단말기로 Device key가 등록됩니다.<br />

#### (3) 삭제

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_4.0.png)

테스트 단말기 조회 화면에서 삭제하고자 하는 테스트 단말기를 체크한 후 상단의 삭제 버튼을 클릭하면 테스트 단말기 정보가 삭제됩니다. 삭제된 정보는 복구가 불가능하므로 확인 후 삭제하시기 바랍니다.<br />

### Authentication Information

#### Facebook
Enter {App ID} and {App Secret Code} of an app registered in the Facebook developer&#39;s site in the TOAST Cloud Gamebase Console.<br />
Note that {Facebook Permission} which is required for a login should also be entered to Additional Info in the json string format.<br />

**Entry Fields** <br />

- Client ID: {AppID} <br />
- Secret Key: {App Secret Code} <br />
- Additional Info: Facebook Permission (json format) <br />


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_1.0.png)

**[Example] facebook_permission format **<br />

```json
{ "facebook_permission": [ "public_profile", "email", "user_friends"] }
```

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_facebook_permission_1.0.png)

**Reference URL **<br />

- [Facebook Developer&#39;s Site](https://developers.facebook.com/)<br />
- [Facebook Permission](https://developers.facebook.com/docs/facebook-login/permissions/)<br />




#### Google
Enter {Client ID}, {Secret Key}, and {Redirection URI} of an app registered on the Google developer&#39;s console in the TOAST Cloud Gamebase Console.<br />

**Entry Fields**<br />

- Client ID: {Client ID}<br />
- Secret Key: {Secret Key}<br />
- Callback URL: {Redirection URI}
  <br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_google_1.0.png)

#### Apple Game Center
Enter Bundle ID registered on Apple Developer&#39;s Site in the TOAST Cloud Gamebase Console.<br />

**Entry Fields**<br />

- ClientID: {Bundle ID}<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_apple_1.0.png)

**Reference URL**<br />

- [Apple Developer&#39;s Site](https://developer.apple.com/)<br />
- [Apple iTunes Connect](https://itunesconnect.apple.com/)<br />

#### PAYCO
Enter {client\_id} and {client\_secret} issued from PAYCO ID application in the TOAST Cloud Gamebase Console.<br />

**Entry Fields**<br />

- ClientID: {Payco client_id}<br />
- Secret Key: {Payco client_secret}<br />

#### NAVER
Enter {client\_id} and {client\_secret} issued from NAVER ID application in the TOAST Cloud Gamebase Console.
Set json string-type information to **Additional Information**. : **service_name** and **url_scheme_ios_only** should be set as NaverSDK requires. 
	
**Entry Fields**<br />

- ClientID: {Payco client_id}<br />
- Secret Key: {Payco client_secret}<br />
- Additional Info: Naver Application Name & iOS url scheme (json format) <br />

**[Example] Naver Additional input format **<br />
```json
{ "url_scheme_ios_only": "Your Url Scheme", "service_name": "Your Service Name" }
```
**Reference URL**<br />
- [Naver Developers - Register Application](https://developers.naver.com/apps/#/register)<br />
- [Naver Developers - client id, client secret](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)<br />

## Client

Can manage client information by operating system (iOS, Android, Unity WebGL, or Unity Standalone), or version.<br />

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
Can check the list of currently-registered clients by operating system.<br />
The number of icon indicates the version entered when registering a client.<br />
The icon list shows service status, such as <font color="white" style="background-color:#F8BB28">Test</font>, <font color="white" style="background-color:#FB8F37">Review</font>, <font color="white" style="background-color:#88C637">Service</font>, <font color="white" style="background-color:#2AB1A6">Update Recommended(in service)</font>only. Click the arrow key at the bottom right to check the list of clients, in such status as, <font color="white" style="background-color:#A1A1A1">Update required</font>, <font color="white" style="background-color:#CCCCCC">Close</font>.<br />
The color of an icon helps identify service status at a glance.<br />

### Properties

Describes client registration information managed by the Gamebase Console.<br/>
In the **Client** tab, click **Register AOS** , **Register iOS** and a screen for client registration will show. To modify or delete client&#39;s entry value, click an icon from the icon list or select a client from the entire list.<br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client6_1.0.png)
#### (1) Store
(<font color="red">Required</font>) Select a store to distribute a client. <br/>
Each operating system has a different store.<br />
#### (2) Client Version
(<font color="red">Required</font>) Enter a client version. <br/>
Enter character strings in accordance with rules of each game.<br />
#### (3) Service Status
(<font color="red">Required</font>) Select a service status of a client, out of 6:<br />
<font color="white" style="background-color:#F8BB28">Test</font>, <font color="white" style="background-color:#FB8F37">Review</font>, <font color="white" style="background-color:#88C637">Service</font>, <font color="white" style="background-color:#2AB1A6">Update Recommended(in service)</font>, <font color="white" style="background-color:#A1A1A1">Update required</font>, <font color="white" style="background-color:#CCCCCC">Close</font>.

- <font color="white" style="background-color:#F8BB28">Test</font>: Internal test is underway<br />
- <font color="white" style="background-color:#FB8F37">Review</font>: Store is under evaluation. <br/> Can add setting of Stabilization Index.<br />
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
> [Note]<br/>
> What is &#39;Stabilization Index&#39;?<br/>
> It is an indicator log to call a Gamebase API within Gamebase. <br/>
> When it is activated during &#39;Test&#39;, the indicator helps detect problems more at ease when an issue arises in Gamebase. <br />
> Its use can be set in the Gamebase Console in real time, along with the log level.<br />

- <font color="white" style="background-color:#88C637">Service</font>: Service is normally provided<br />
- <font color="white" style="background-color:#2AB1A6">Update Recommended (In Service)</font>: Service is normally provided. <br/>Pop-up will show to lead into more stable versions. Although downloading a newer version is recommended, users can continue playing games with a current version.<br />Gamebase provides the default pop-up as below. <br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRecommended_1.0.png)

- <font color="white" style="background-color:#A1A1A1">Update Required</font>: Service is unavailable. <br/>As the current version is not supported by game, a pop-up will show to update app.<br />Gamebase provides the default pop-up as below. <br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[Caution] </font>  <br/>
>  When **both Update Required and Under Maintenance are set at the same time** , the service status becomes &#39;Update Required&#39;.<br/>
>  If you don&#39;t want to show the Update Required pop-up to user during maintenance, the service status should be changed to &#39;Update Required&#39; after maintenance is completed.<br/>

- <font color="white" style="background-color:#CCCCCC">Close</font>: Service unavailable. <br/> To be selected for a version of closed service.<br />Gamebase provides the default pop-up as below. <br />![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [Note] <br/>
> Setting Message for Service Status.<br/>
> For **Update Recommended (In Service)**, **Update Required** , and **Service Closed** , messages can be set in multiple languages. <br />
> When a service status is selected, default messages are provided in five languages (Korean, English, Japanese, Chinese Simplified, and Chinese Traditional), and you can add a language or change messages. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client5_1.1.png)

#### (4) Server URL
Enter a server URL (IP, URL) that a client will use.<br />
A server URL entered in the **App** tab will be applied for all clients; enter only when each client has a different server URL.<br />


## Installed URL

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl1_1.2.png)

Manage store URL information to install a game.<br/>
A user&#39;s click on a short URL via PC or mobile will be redirected to a site entered on a user device (device, operating system, store, etc.).<br/>
If there is no store information, or redirection is failed, the URL will be linked as set in &#39;COMMON&#39;.<br/><br/><br/>

_[Example 1] A user clicks Install URL in a text message on an Android device._<br/>
**(Device:mobile,OS:Android,Store:N/A)** Move to a mobile URL of a representing Android store. In the case of &#39;Google Play&#39;, move to the URL set on the &#39;Google Play&#39; mobile.<br/>
_[Example 2] A user playing a game downloaded from &#39;One Store&#39; clicks &#39;Update Now&#39; in the &#39;Update Required&#39; pop-up._<br/>
**(Device:mobile,OS:Android,Store:One Store)** 'One Store' 모바일에 설정된 URL로 이동(One Store 모바일 설치 페이지)<br/>
_[Example 3] A user enters Install URL on a PC._<br/>
**(Device:PC,OS:Windows,Store:N/A)** Move to URL set in COMMON PC.


### Properties

To modify Install URL, click **Modify**.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_InstallUrl2_1.2.png)

- Item setting can be different for PC and mobile. Enter the same value for each device, if there is no need to separate.<br />
- When a store you want is not on the list, contact [CustomerCenter](https://toast.com/support/inquiry) so as to add as required.<br />

#### (1) Common
Set an URL to connect when store information is unavailable or store relocation has failed.<br />
#### (2) Android
Set an URL to connect at the request of Android users.<br />

#### (3) iOS
Set an URL to connect at the request of App Store users.<br />
#### (4) Standalone
Set an URL to connect from an app of standalone service. Standalone needed PC URL only.<br />


