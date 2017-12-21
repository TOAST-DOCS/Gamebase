## Game > Gamebase > Overview

TOAST Cloud Gamebase supports developing games with ease and efficiency by providing commonly-required game functions via integrated SDK. <br>
Game developers can focus on developing game content only, as Gamebase provides all the other services.   

Gamebase provides basic game functions like authentication, payment, and push, as well as data management, operational maintenance, and notifications which are required for game app operations.  Basic indicators of interest are also supported in real time for the whole game industry.

Key functions of Gamebase and description are as follows:

<br/>
## Key Features

### Authentication

Gamebase supports OAuth login based on ID, using accounts of many identity providers (IdP), and passwords; guest login by using UUID of a device.  Its authentication service is based on member information provided by external IdP, without having its own member system. In other words, user's ID or passwords are not saved in Gamebase.

* Provides many authentication methods via single interface <br/>
  Development costs can be saved by enabling easier and faster external IdP development. Developers can easily implement authentication with no regards to complicated procedure, legal or policy issues. <br/>
* Provides various external IdP authentication methods <br/>
  Provided external authentication is to be continuously updated, and if there is any other authentication you'd like to include, contact [Customer Center](https://cloud.toast.com/support/faq/).    <br/>

Following is the list of external authentication supported by Gamebase.

| External Authentication | Provided Platform |
| ----------------------- | ----------------- |
| Facebook                | iOS, Android      |
| Apple Game Center       | iOS               |
| Google                  | Android           |
| Payco                   | iOS, Android      |

* Provides guest logins
  With guest login, users can log into and start a game without any entry required. As Gamebase user ID is issued even to guest users , game data of all users, regardless of OAuth or guest login, can be  managed.  <br/>
* Provides independent member identification  <br/>
  On a first-time login, Gamebase user ID is automatically created, which can be used as user identifier in a game. User ID is issued to all users, regardless of authentication method, and is not inclusive to a particular IdP, so user processing is available in the same method throughout any IdP.    <br/>
* Provides log-out and withdrawal <br/>
  You can choose to log out and log in again from different authentication, and if you withdraw from a game, user ID will be deleted from Gamebase with all related information.
* Provides mapping so that a game user can use multiple external IdPs at the same time <br/>For example, a game user with Facebook authentication can use the same ID with Google authentication. If Facebook authentication is mapped with Google authentication to a user ID, the user can play games with Facebook authentication and Google authentication, each on two different devices.   <br/>

#### Reference

* [Android Developer Guide > Auth](./aos-authentication)
* [iOS Developer Guide > Auth](./ios-authentication)
* [Unity Developer Guide > Auth](./unity-authentication)

### Launching

A game app in service requires lots of information when it first launches. Gamebase provides data required for operation to a game app during initial execution, and it is called Launching.  <br/>
Launching data can be set on the Gamebase Console in real time, and the changes can be checked when initializing SDK or at game launching status. <br/><br/>

Following information is provided by Gamebase for Launching.

* App Status
  * Whether to update game client, and download URL
  * Maintenance information
* Urgent Notice
* Authentication
* URL list of in-app games  

#### Reference

* [Android Developer Guide > Launching Info](./aos-initialization/#launching-status)
* [iOS Developer Guide > Launching Info](./ios-initialization/#launching-status)
* [Unity Developer Guide > Launching Info](./unity-initialization/#launching-informations)
* [Operator Guide > App Info(App, Client, Installed URL)](./oper-app): Set status of app and client, and installation URL
* [Operator Guide > Operator(Maintenance, Notice)](./oper-operation): Register maintenance and notice


### For Global

Gamebase basically supports global games, and provides following functions to that purpose:  

* Provides user messages in multiple languages
  * User messages can be entered in multiple languages via console, but, language set follows user device's setting. If Korean, English, and Japanese are entered via console, users of Korean device will be displayed with Korean messages.
* Allows filtering by country
  * In case urgent notice or push messages are directed at users of a particular country during game operations, the messages can be sent to a specified country only.  
* Select operator's local standard time to enter time at ease
  * If a game is run in Vietnam, a standard time of Vietnam can be selected and entered, with no need to convert to Korean time.

### Standard Indicators (BI)

* Gamebase Console provides basic operating indicators in real time.
  * Concurrent Connected User (CCU): Number of concurrently connected users in real time
  * Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date)
  * Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date)
  * New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date)
  * Market Share Chart: Pie chart of market share by user's operating system, country, and game client's version
  * Graph of Concurrent Access Change: Provides change of concurrent access of a day on a graph. Can check graph changes due to maintenance and push message delivery at a glance.  
* Group indicator is provided to find indicators of all projects at authority.
* By providing installation URL statistics, URL call counts and market share data are provided per date, browser (Internet Explorer, Chrome, or others), and platform (Windows, Android, and etc.).
* Screen of sales status is provided to check sales statistics of a game.
  * Sum of sales of each month, date, and store
  * In currency of choice (such as KRW or USD)

#### Reference

* [Operator Guide > Operating indicator](./oper-operating-indicator)

### Other TOAST Cloud Services

* Supports easier interfaces to TOAST Cloud products that a game requires
  * Gamebase provides wrapped APIs on the basis of Gamebase User IDs. Therefore, users don't need to make separate calls to each product's API.
  * [Notification > PUSH](http://cloud.toast.com/service/notification) : Integrated push service to send push messages
  * [Common > IAP](http://cloud.toast.com/service/iap) : Integrated in-app payment service
  * [Game > Leaderboard](http://cloud.toast.com/service/leaderboard) : Real-time large-capacity ranking service
  * [Security > AppGuard](https://cloud.toast.com/service/security) : Prevents code manipulation of applications in real time


<br/>
## Glossary
Gamebase service terms are as follows:

| Term      | Description                              |
| --------- | ---------------------------------------- |
| UserID    | User identifier inside of Gamebase       |
| DeviceKey | Device identifier (iOS:IDFV, Android:Android ID) |
| UUID      | Device identifier used to create a guest, which is retained before app is deleted. |
| IdP       | Identity Providers who provide authentication:  Facebook, Google, Apple Game Center, Payco, and etc. |
| IdP Token | Access token received from IdP SDK after authentication |
| IdP Login | Login with an external IdP (such as Facebook or Google) |

<br/>

## Service Architecture
Below are the service structure of Gamebase with simple description.
![논리 구성도](http://static.toastoven.net/prod_gamebase/Overview/img_logical_1.0.png)
<br>

| Component       | Description                              |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - Client development SDK                 |
| Gateway         | - Provides mashup API between internal and external modules <br>- Delivers to backend services at the request of client and server |
| Gamebase Server | - Processes internal logic of Gamebase <br>- Provides data for client's initial execution <br>- Issues/manages user identifier keys and manages mapping <br>- Collects and manages concurrent access indicators per game |
| Console         | - Web Console                            |
