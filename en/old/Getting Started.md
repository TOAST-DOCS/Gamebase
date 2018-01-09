## Game > Gamebase > Getting Started

## Getting Started
There is a simple but necessary procedure to follow to use the Gamebase service.

1. Enable Service [Console]
2. Check Project ID and Secret Key [Console]
3. Register Game and Client Information [Console]
4. Download Gamebase SDK 

### Enable Gamebase [Console]

First, Gamebase must be enabled. Go to TOAST Cloud Console and select **Game > Gamebase** and click **Enable**. 

![상품활성화](http://static.toastoven.net/prod_gamebase/GettingStarted/img_console_active_1.0.png)
<center>[Figure 1] Enable Gamebase </center>

### Check Project ID and Secret Key [Console]

#### AppId
App ID is a project ID of TOAST Cloud, and you can find it on **Project List** of the Console. <br>
It is a prerequisite to call server API or set SDK.

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_appId_v1.1.png)
<center>[그림 2] Gamebase AppId</center>

#### SecretKey
Secret Key is a means for access control of API, and you can find it in the Gamebase Console.  <br>
It is a prerequisite for HTTP header to call server API. 

![image alt](http://static.toastoven.net/prod_gamebase/Server_Developers_Guide/pre_secret_key_v1.1.png)
<center>[Figure 3] Gamebase SecretKey</center>

### Register App and Client [Console]

Register basic information of game and client on [App] of the Gamebase Console.<br>
For details of each item, refer to the links below.

* [Operator Guide > App](./oper-app/#app): App Configuration (Authentication, game server address, and etc.)
* [Operator Guide > Client](./oper-app/#client): Client Configuration (Service status of each version, store information, and etc. )


![게임 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App1_1.1.png)
<center>[Figure 4] Game Information Registration </center>

![클라이언트 정보 등록 화면](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client4_1.1.png)
<center>[Figure 5] Client Information Registration </center>
> [Note]<br>
> Keep note to check your client version information which is required to set SDK. 


### Download Gamebase SDK

You can download Gamebase SDK in the link below.<br/>
[Download Gamebase SDK]](http://docs.cloud.toast.com/ko/Download/)

For details of each platform, refer to **Developer's Guide** of each platform. <br/>

* [Getting Started for iOS Development Project](./ios-started/)
* [Getting Started for Android Development Project](./aos-started/)
* [Getting Started for Unity Plug-in Development Project](./unity-started)
  <br/>
> Now you're ready to use Gamebase service. <br>Read below for more guides. 

<br/>
## Platform Guide

### Client Developer's Guide

* [iOS Developer's Guide](./ios-started/)
* [Android Developer's Guide](./aos-started/)
* [Unity Developer's Guide](./unity-started/)

### Server Developer's Guide

* [Server Developer's Guide](./Server%20Developer%60s%20Guide/)

### Operator's Guide

* [Operator's Guide](./oper-operating-indicator/)

<br/>
## Functional Guide

| Feature               | Description                              | client                                   | server                                   | console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | Support Guest , 3rd Party authentication <br> - Supported IdPs: Facebook, Google+, Apple Game Center, PAYCO | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[Token Authentication](./Server%20Developer%60s%20Guide/#token-authentication)] <br> [[Retrieve Member](./Server%20Developer%60s%20Guide/#get-member)] | [[App] > Setting Authentication Information ](./oper-app/#authentication-information) <br> [[Member] > Retrieve](./oper-member/#member) <br> - Basic information, login history, playtime, purchase history, and etc. |
| Logout                | Logout                                   | [[iOS](./ios-authentication/#logout)][[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | Withdraw from a game <br> -  Delete all information of a game user, including user ID and mapping information | [[iOS](./ios-authentication/#withdraw)][[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | Integrating one user ID to many IdPs     | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Purchase(IAP)         | (TOAST Cloud Integration) <br> InApp Purchase <br> - Supported stores: Google, App Store | [[iOS](./ios-purchase/#purchase)][[Android](./aos-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrappping API](./Server%20Developer%60s%20Guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- Register Items](./oper-purchase/#item) <br> [- Retrieve Transaction](./oper-purchase/#transactions) |
| Push                  | (TOAST Cloud Integration) <br> Send push messages and check results | [[iOS](./ios-push/#push)] [[Android](./aos-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- Real-time delivery |
| Leaderboard           | (TOAST Cloud Integration) <br> Retrieve and register real-time ranking of large-capacity data |                                          | [[Wrappping API](./Server%20Developer%60s%20Guide/#leaderboard)] |                                          |
| WebView               | SDK provides default WebView UI <br/>Provides alerts, TOAST UI | [[iOS](./ios-ui/#webview)] [[Android](./aos-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator]Maintenance | Maintenance (operational)                |                                          | [Check for Maintenance](./Server%20Developer%60s%20Guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- Register or cancel maintenance |
| [Operator]Notice      | (Operational) Urgent Notification <br> -  In pop-ups while user is executing an app |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>- Register Notice |
| [Operator]Ban         | (Operational) Register/Release ban on game users <br> -  Register or release ban on users | [[iOS](./ios-authentication/#get-banned-user-information)][[Android](./aos-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -Check information of banned users |                                          | [[Ban]](./oper-ban/#ban) <br/>-Register and Release Ban |


