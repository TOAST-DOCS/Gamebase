## Game > Gamebase > Overview

Gamebase is a strongly recommended service as it embraces a decade's operational knowhow of NHN Entertainment, which is the leading game platform provider. 
It only takes Gamebase SDK to make easy use of common game services.  

![Gamebase_summary](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_01_201903_en.png)

## Key Features

### Authentication

Gamebase supports OAuth login based on ID and passwords, using accounts of many identity providers (IdPs); guest login by using UUID of a device. Its authentication service is based on member information provided by external IdP, without having its own member system. In other words, user's ID or passwords are not saved in Gamebase.

* **Provides many authentication methods via single interface. **
  Development costs can be saved by enabling external IdP development easier and faster.  Developers can easily implement authentication without concerning complicated procedure and legal or policy issues.

* **Provides various external IdP authentication methods.**
  Provided external authentication is to be continuously updated, and if there is any other authentication you'd like to include, contact [Customer Center](https://toast.com/support/inquiry).

Following is the list of external authentication supported by Gamebase.

| External Authentication             | Android | iOS | Windows(based Unity) | Web(based JavaScript)    |
| ----------------- | ------------ | ------------ | ------------ | ------------ |
| Facebook          | O | O | O | O |
| Apple Game Center |  | O | | |
| Google            | O | O | O | O |
| PAYCO             | O | O | O | O |
| NAVER             | O | O | O | O |
| Twitter			| O | O | |  |
| LINE				| O | O |  |  |

* **Provides guest logins.**
  With guest login, users can log in and start a game without any authentication required. As Gamebase user ID is issued even to guest users, game data can be managed for all users, regardless of OAuth or guest login
  
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


### Gamebase Analytics

With Gamebase SDK,indicators on sales, users, and game balancing are provided for free. 
Necessary indicator services for game business and operations, including sales, concurrent access, users, level, and item sales, are provided. 
Apply them fast to the needs of your service!
![Gamebase_analytics](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_02_201903_en.png)

#### Reference

* [Operator Guide > Analytics](./oper-analytics) 


### Launching

A game app in service requires lots of information when it first launches. Gamebase provides data required to operate the game app during initial execution, which is called Launching. 
Launching data can be set in the Gamebase Console in real time, and the changes can be checked when initializing SDK or at game launching status.

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
* [Operator Guide > App Info(App, Client, Installed URL)](./oper-app): Set status of app and client, and installation URL
* [Operator Guide > Operator(Maintenance,Notice)](./oper-operation): Register maintenance and notice


### For Global

Gamebase basically supports global games, and provides following functions to that purpose:

* Provides user messages in multiple languages.
  * User messages can be entered in multiple languages via console, but, language set follows user device's setting. If Korean, English, and Japanese are entered via console, users of Korean device will be displayed with Korean messages.
* Allows filtering by country.
  * In case urgent notice or push messages are directed at users of a particular country during game operations, the messages can be sent to a specified country only. 
* Selects operator's local time zone to enter time at ease.
  * If a game is run in Vietnam, Vietnam’s time zone can be selected and entered, with no need to convert to Korean time.

### Other TOAST Services

* Supports easier interfaces to TOAST service that a game requires.
  * Gamebase provides wrapped APIs on the basis of Gamebase User IDs. Therefore, users don't need to make separate calls to each service's API.
  * [Notification > PUSH](http://www.toast.com/service/notification) : Integrated push service to send push messages
  * [Common > IAP](http://www.toast.com/service/iap) : Integrated In-App Purchase service
  * [Game > Leaderboard](http://www.toast.com/service/leaderboard) : Real-time large-capacity ranking service
  * [Security > AppGuard](https://cloud.toast.com/service/security) : Prevents code manipulation of applications in real time

## Glossary
Gamebase service terms are as follows:

| Term      | Description                                       |
| ------- | ---------------------------------------- |
| User ID  | User identifier inside of Gamebase                     |
| Device Key  | Device identifier (iOS:IDFV, Android:Android ID)   |
| UUID    | Device identifier used to create a guest, which is retained before app is deleted.     |
| IdP     | Identity Providers who provide authentication: Facebook, Google, Apple Game Center, PAYCO, and etc. |
| IdP Token  | Access token received from IdP SDK after authentication  |
| IdP Login | Login with an external IdP (such as Facebook or Google)           |

## Service Architecture
Below shows the service structure of Gamebase with simple description
![logical architecture](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_03_201903_en.png)

| Component           | Description                                       |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - Client development SDK                      |
| Gateway         | - Provides mashup API between internal and external modules.<br/>- Delivers to backend services at the request of client and server.|
| Gamebase Server | - Processes internal logic of Gamebase. <br>- Provides data for client's initial execution  <br>- Issues/manages user identifier keys and manages mapping <br>- Collects and manages concurrent access indicators per game. |
| Console         | - Web Console                              |


## Platform Guide

### Client Developer's Guide

* [iOS Developer's Guide](./ios-started/)
* [Android Developer's Guide](./aos-started/)
* [Unity Developer's Guide](./unity-started/)

### Server Developer's Guide

* [Server Developer's Guide](./api-guide/)

### Operator's Guide

* [Console Guide](./oper-operating-indicator/)

## Funtional Guide

| Feature               | Description                              | Client                                   | Server                                   | Console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | Support Guest and 3rd Party authentication<br/>- [Supported IdPs](./Overview/#authentication) | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[Token Authentication](./api-guide/#token-authentication)] <br> [[Retrieve Member](./api-guide/#get-member)] | [[App] > Setting Authentication Information](./oper-app/#authentication-information) <br> [[Member] > Retrieve](./oper-member/#member) <br> - Basic information, login history, playtime, purchase history, and etc. |
| Logout                | Logout                                     | [[iOS](./ios-authentication/#logout)] [[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | Withdraw from a game<br/> - Delete all information of a game user, including user ID and mapping information | [[iOS](./ios-authentication/#withdraw)] [[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | Integrate one user ID to many IdPs           | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Analytics                  | - Real-time Indicators<br>- Sales Indicators<br>- User Indicators<br>- Balancing Indicators | [[Android](./aos-etc/#analytics)] [[iOS](./ios-etc/#analytics)] [[Unity](./unity-etc/#analytics)] |                                          | [[Analytics]](./oper-analytics)   ||
| Purchase(IAP)         | (TOAST Integration) <br/> InApp Purchase <br/>- Supported stores: Google and App Store | [[iOS](./ios-purchase/#purchase)] [[Android](./aos-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./api-guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- Register Items](./oper-purchase/#item) <br> [- Retrieve Transaction](./oper-purchase/#transactions) |
| Push                  | (TOAST Integration) <br>Send push messages and check results | [[iOS](./ios-push/#push)] [[Android](./aos-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- Real-time delivery |
| Leaderboard           | (TOAST Integration)<br> Retrieve and register real-time ranking of large-capacity data |                                          | [[Wrapping API](./api-guide/#leaderboard)] |                                          |
| Webview               | SDK provides default WebView UI<br/>Provides both system pop-up and TOAST UI | [[iOS](./ios-ui/#webview)] [[Android](./aos-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator] Maintenance | (Operational)  Maintenance                               |                                          | [[Check for Maintenance](./api-guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- Register or cancel maintenance |
| [Operator] Notice      | (Operational) Urgent Notification <br> -  In pop-ups while user is executing an app |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-Register Notice |
| [Operator] Ban         | (Operational) Register/Release banned game users <br> -  Register/Release banned game users | [[iOS](./ios-authentication/#get-banned-user-information)][[Android](./aos-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -Check information of banned users |   [[Retrieving the ban history of game users](./api-guide/#ban-histories)                                       | [[Ban]](./oper-ban/#ban) <br/>-Register and Release Ban |

