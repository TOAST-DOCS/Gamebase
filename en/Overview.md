## Game > Gamebase > Overview

TOAST Gamebase supports developing games with ease and efficiency by providing commonly-required game functions via integrated SDK.
Game developers can focus on developing game content only, as Gamebase provides all the other services.

Gamebase provides basic game functions like authentication, payment, and push; operational functions for a game app, like data management, maintenance, and notifications. Basic indicators of interest are also supported in real time for the whole game industry.

Key functions of Gamebase and description are as follows: 

## Key Features

### Authentication

Gamebase supports OAuth login based on ID and passwords, using accounts of many identity providers (IdPs); guest login by using UUID of a device. Its authentication service is based on member information provided by external IdP, without having its own member system. In other words, user's ID or passwords are not saved in Gamebase.<br/>


* **Provides many authentication methods via single interface. ** 
  Development costs can be saved by enabling external IdP development easier and faster.  Developers can easily implement authentication without concerning complicated procedure and legal or policy issues.

* **Provides various external IdP authentication methods.** 
  Provided external authentication is to be continuously updated, and if there is any other authentication you'd like to include, contact [Customer Center](https://cloud.toast.com/support/faq/).


Following is the list of external authentication supported by Gamebase.

| External Authentication            | Provided Platform     |
| ----------------- | ------------ |
| Facebook          | iOS, Android |
| Apple Game Center | iOS          |
| Google            | Android      |
| PAYCO             | iOS, Android |
| Naver          | iOS, Android |

* **Provides guest logins.** 
  With guest login, users can log in and start a game without any authentication required. As Gamebase user ID is issued even to guest users, game data can be managed for all users, regardless of OAuth or guest login<br/>

* **Provides independent member identification.** 
  On a first-time login, Gamebase user ID is automatically created, which can be used as user identifier in a game. User ID is issued to all users, regardless of authentication method, and is not inclusive to a particular IdP, so user processing is available in the same method throughout any IdP.
  
* **Provides log-out and withdrawal.**
  You can choose to log out and log in again from different authentication, and if you withdraw from a game, user ID will be deleted from Gamebase with all related information.
  
* **Provides mapping so that a game user can use multiple external IdPs at the same time.**
  For example, a game user with Facebook authentication can use the same ID with Google authentication. If Facebook authentication is mapped with Google authentication to a user ID, the user can play games with Facebook authentication and Google authentication, each on two different devices. 

#### Reference

* [Android Developer Guide > Auth](./aos-authentication)
* [iOS Developer Guide > Auth](./ios-authentication)
* [Unity Developer Guide > Auth](./unity-authentication)

### Launching

A game app in service requires lots of information when it first launches. Gamebase provides data required to operate the game app during initial execution, which is called Launching.  <br/>
Launching data can be set in the Gamebase Console in real time, and the changes can be checked when initializing SDK or at game launching status.<br/><br/>

Following information is provided by Gamebase for launching.

* App status
	* Whether to update game client, and download URL
	* Maintenance information
* Urgent notice
* Authentication
* URL list of in-app games 

#### Reference

* [Android Developer Guide > Launching Info](./aos-initialization/#launching-status)
* [iOS Developer Guide > Launching Info](./ios-initialization/#launching-status)
* [Unity Developer Guide > Launching Info](./unity-initialization/#launching-informations)
* [Operator Guide > App Info(App, Client, Installed URL)](./oper-app) : Set status of app and client, and installation URL
* [Operator Guide > Operator(Maintenance,Notice)](./oper-operation) : Register maintenance and notice


### For Global

Gamebase basically supports global games, and provides following functions to that purpose: 

* **Provides user messages in multiple languages.**
  User messages can be entered in multiple languages via console, but, language set follows user device's setting. If Korean, English, and Japanese are entered via console, users of Korean device will be displayed with Korean messages.
* **Allows filtering by country.**
  In case urgent notice or push messages are directed at users of a particular country during game operations, the messages can be sent to a specified country only.
* **Selects operator's local time zone to enter time at ease.**
  If a game is run in Vietnam, Vietnam’s time zone can be selected and entered, with no need to convert to Korean time.

### Standard Indicators (BI)

* The Gamebase Console provides basic operating indicators in real time: 
	* Concurrent Connected User (CCU): Number of concurrently connected users in real time.
	* Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date).
	* Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date).
	* New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date).
	* Market Share Chart: Pie chart of market share by user's operating system, country, and game client's version.
	* Graph of Concurrent Access Change: Change of concurrent access of a day on a graph. Can check graph changes due to maintenance and push message delivery at a glance.
* Group indicator to find indicators of all projects at authority.
* URL call counts and market share data per date, browser (Internet Explorer, Chrome, or others), and platform (Windows, Android, and etc.), by providing installation URL statistics.
* Sales status page to check sales statistics of the game.
	* Sum of sales by month, date, and store.
	* Can check by the currency of choice: KRW or USD.

#### Reference

* [Operator Guide > Operating indicator](./oper-operating-indicator) 

### Other TOAST Services

Supports easier interfaces to TOAST service that a game requires.<br/>
Gamebase provides wrapped APIs on the basis of Gamebase User IDs. Therefore, users don't need to make separate calls to each service's API.
* [Notification > PUSH](https://toast.com/service/notification/push) : Integrated push service to send push messages
* [Common > IAP](https://toast.com/service/mobile_service/iap) : Integrated In-App Purchase service
* [Game > Leaderboard](https://toast.com/service/game/leaderboard) : Real-time large-capacity ranking service
* [Security > AppGuard](https://toast.com/service/security/appguard) : Prevents code manipulation of applications in real time


## Glossary

Gamebase service terms are as follows:

| Term      | Description                                       |
| ------- | ---------------------------------------- |
| User ID | User identifier inside of Gamebase                    |
| Device Key  | Device identifier (iOS:IDFV, Android:Android ID)  |
| UUID    | Device identifier used to create a guest, which is retained before app is deleted.    |
| IdP     | Identity Providers who provide authentication: Facebook, Google, Apple Game Center, PAYCO, and etc. |
| IdP Token  | Access token received from IdP SDK after authentication |
| IdP Login | Login with an external IdP (such as Facebook or Google)          |

<br/>

## Service Architecture

Below shows the service structure of Gamebase with simple description
![논리 구성도](http://static.toastoven.net/prod_gamebase/Overview/img_logical_1.2.png)
<br>

| Component           | Description                                       |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - Client development SDK                      |
| Gamebase Server | - Provides mashup API between internal and external modules.<br/>- Processes internal logic of Gamebase. <br>- Provides data for client's initial execution  <br>- Issues/manages user identifier keys and manages mapping <br>- Collects and manages concurrent access indicators per game. |
| Console         | - Web Console                              |


## Platform Guide

### Client Developer's Guide

* [Android Developer's Guide](./aos-started/)
* [iOS Developer's Guide](./ios-started/)
* [Unity Developer's Guide](./unity-started/)

### Server Developer's Guide

* [Server Developer's Guide](./Server%20Developer%60s%20Guide/)

### Operator's Guide

* [Operator's Guide](./oper-operating-indicator/)

<br/>
## Funtional Guide

| Feature               | Description                              | Client                                   | Server                                   | Console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | Support Guest and 3rd Party authentication<br/>- Supported IdPs: Facebook, Google, Apple Game Center, and PAYCO | [[Android](./aos-authentication/#login)] [[iOS](./ios-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[Token Authentication](./Server%20Developer%60s%20Guide/#token-authentication)] <br> [[Retrieve Member](./Server%20Developer%60s%20Guide/#get-member)] | [[App] > Setting Authentication Information](./oper-app/#authentication-information) <br> [[Member] > Retrieve](./oper-member/#member) <br> - Basic information, login history, playtime, purchase history, and etc. |
| Logout                | Logout                                     | [[Android](./aos-authentication/#logout)] [[iOS](./ios-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | Withdraw from a game<br/> - Delete all information of a game user, including user ID and mapping information | [[Android](./aos-authentication/#withdraw)] [[iOS](./ios-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | Integrate one user ID to many IdPs         | [[Android](./aos-authentication/#mapping)] [[iOS](./ios-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Purchase(IAP)         | (TOAST Integration) <br/> InApp Purchase <br/>- Supported stores: Google and App Store  | [[Android](./aos-purchase/#purchase)] [[iOS](./ios-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./Server%20Developer%60s%20Guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- Register Items](./oper-purchase/#item) <br> [- Retrieve Transaction](./oper-purchase/#transactions) |
| Push                  | (TOAST Integration) <br>Send push messages and check results  | [[Android](./aos-push/#push)] [[iOS](./ios-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- Real-time delivery |
| Leaderboard           | (TOAST Integration)<br> Retrieve and register real-time ranking of large-capacity data |                                          | [[Wrapping API](./Server%20Developer%60s%20Guide/#leaderboard)] |                                          |
| Webview               | SDK provides default WebView UI<br/>Provides both system pop-up and TOAST UI | [[Android](./aos-ui/#webview)] [[iOS](./ios-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator] Maintenance | (Operational)  Maintenance                               |                                          | [[Check for Maintenance](./Server%20Developer%60s%20Guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- Register or cancel maintenance |
| [Operator] Notice      | (Operational) Urgent Notification <br/>- In pop-ups while user is executing an app |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-Register Notice|
| [Operator] Ban         | (Operational) Register/Release banned game users  | [[Android](./aos-authentication/#get-banned-user-information)] [[iOS](./ios-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> - Check information of banned users |                                    | [[Ban]](./oper-ban/#ban) <br/>- Register and Release Ban |

