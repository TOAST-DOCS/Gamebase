## Game > Gamebase > Overview

Gamebase is a strongly recommended service as it embraces a decade's operational knowhow of NHN, a leading game platform provider. 
All you need to do is apply the Gamebase SDK and all services are at your fingertips.

![Gamebase_summary](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/Overview/en/Gamebase_overview_00_en_202501.png)

## Gamebase Sample App

We are providing sample apps to help you test out the various features of Gamebase.
With the sample apps, you can test the Gamebase features in your game app and predict how they work.
Developers can check the sample app code to easily find out how to apply Gamebase.

* [Download page](https://github.com/nhn/toast.gamebase.unity.sample/releases)
![Gamebase_sample_app](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_Sample_App1.png)
* You can use a QR code to download the Sample App APK.(Supported platform: Android OS)

## Key Features

### Gamebase Analytics

With Gamebase SDK,indicators on sales, users, and game balancing are provided for free. 
Necessary indicator services for game business and operations, including sales, concurrent access, users, level, and item sales, are provided. 
Apply them fast to the needs of your service!
![Gamebase_analytics](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_02_201903_en.png)

#### Reference

* [Operator Guide > Analytics](./oper-analytics) 

### Authentication

Gamebase supports OAuth login based on ID and passwords, using accounts of many identity providers (IdPs); guest login by using UUID of a device. Its authentication service is based on member information provided by external IdP, without having its own member system. In other words, user's ID or passwords are not saved in Gamebase.

* **Provides many authentication methods via single interface. **
  Development costs can be saved by enabling external IdP development easier and faster.  Developers can easily implement authentication without concerning complicated procedure and legal or policy issues.

* **Provides various external IdP authentication methods.**
  Provided external authentication is to be continuously updated, and if there is any other authentication you'd like to include, contact [Customer Center](https://toast.com/support/inquiry).

Following is the list of external authentication supported by Gamebase.

| External Authentication       | Android | iOS | Unity(Windows, macOS) | Unreal(Windows) |
| ----------------- | ------------ | ------------ | ------------ | ------------ |
| Facebook          | O | O | O | O |
| Sign In with Apple | O  | O | | O |
| Apple Game Center |  | O | | |
| Google            | O | O | O | O |
| PAYCO             | O | O | O | |
| NAVER             | O | O | O | |
| Twitter			| O | O | | O |
 | LINE				| O | O | | O |
| Hangame			| O | O | O  | |
| Weibo | O  | O  | | |
| Steam | O  | O  | | O |

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
* [Unreal Developer Guide > Auth](./unreal-authentication)

### Payment

Game companies can enjoy maximum profits with less efforts by releasing already available games to many stores. Since Gamebase supports easy integration with many stores, you're not required a full knowledge of payment integration conditions of each major store.  

Gamebase supports the following stores: 
* Google Play Store
* App Store
* Galaxy Store
* ONE Store
* Steam
* Epic Games Store
* Amazon Appstore
* MyCard

* **In-app purchase of many stores on a single interface** 
  Since further store development gets easy and fast via single-interface API, you can save development costs. Developers can easily implement purchase without having to learn the complexity of integration.  
* **Standalone payment verification server for secure and stable purchases **
  Gamebase helps to stabilize purchase transactions by setting up a seperate server to verify payment with external stores. Given the network status could be unstable, payment retries and item credits are managed separately.   
* **Buying as well as subscription and promotion **
  The subscription feature of Google PlayStore and Appstore is enabled to sell users monthly products. Google's promotion is also available in each game without further implementation. More features of external stores are to be added to Gamebase.  
* **Flawless response to customer inquiries supported by web console features (e.g. query purchase list**
  On the web console, user can check his purchase list and item credit status; can even cancel purchase and respond to abusive acts. 

#### Reference

* [Android Developer Guide > Payment](./aos-purchase/)
* [iOS Developer Guide > Payment](./ios-purchase)
* [Unity Developer Guide > Payment](./unity-purchase)
* [Unreal Developer Guide > Payment](./unreal-purchase)

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
* [Unreal Developer Guide > Launching Info](./unreal-initialization/#launching-information)
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

### Using the other NHN Cloud Service

* Supports easier interfaces to TOAST service that a game requires.
  * Gamebase provides wrapped APIs on the basis of Gamebase User IDs. Therefore, users don't need to make separate calls to each service's API.
  * [Notification > PUSH](http://www.toast.com/service/notification) : Integrated push service to send push messages  
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

<br/>

## Service Architecture
The following shows the service structure of Gamebase with simple description
![logical architecture](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_03_202203_en.png)
<br>

| Component           | Description                                       |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - Client development SDK                      |
| Gateway         | - Provides mashup API between internal and external modules.<br/>- Delivers to backend services at the request of client and server.|
| Gamebase Server | - Processes internal logic of Gamebase. <br>- Provides data for client's initial execution  <br>- Issues/manages user identifier keys and manages mapping <br>- Collects and manages concurrent access indicators per game. |
| Console         | - Web Console                              |


## Platform Guide

### Client Developer's Guide

* [Android Developer's Guide](./aos-started/)
* [iOS Developer's Guide](./ios-started/)
* [Unity Developer's Guide](./unity-started/)
* [Unreal Developer's Guide](./unreal-started/)

### Server Developer's Guide

* [Server Developer's Guide](./api-guide/)

### Operator's Guide

* [Console Guide](./oper-operating-indicator/)

<br/>
## Functional Guide

| Feature               | Description                              | Client                                   | Server                                   | Console                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Analytics                  | - Real-time Indicators<br>- Sales Indicators<br>- User Indicators<br>- Balancing Indicators | [[Android](./aos-etc/#analytics)] [[iOS](./ios-etc/#analytics)] [[Unity](./unity-etc/#analytics)] |                                          | [[Analytics]](./oper-analytics)   ||
| Login                 | Support Guest and 3rd Party authentication<br/>- [Supported IdPs](./Overview/#authentication) | [[iOS](./ios-authentication/#login)] [[Android](./aos-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[Token Authentication](./api-guide/#token-authentication)] <br> [[Retrieve Member](./api-guide/#get-member)] | [[App] > Setting Authentication Information](./oper-app/#authentication-information) <br> [[Member] > Retrieve](./oper-member/#member) <br> - Basic information, login history, playtime, purchase history, and etc. |
| Logout                | Logout                                     | [[iOS](./ios-authentication/#logout)] [[Android](./aos-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | Withdraw from a game<br/> - Delete all information of a game user, including user ID and mapping information | [[iOS](./ios-authentication/#withdraw)] [[Android](./aos-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | Integrate one user ID to many IdPs           | [[iOS](./ios-authentication/#mapping)] [[Android](./aos-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Purchase(IAP)         | (TOAST Integration) <br/> InApp Purchase <br/>- Supported stores: Google and App Store | [[iOS](./ios-purchase/#purchase)] [[Android](./aos-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./api-guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- Register Items](./oper-purchase/#item) <br> [- Retrieve Transaction](./oper-purchase/#transactions) |
| Push                  | (TOAST Integration) <br>Send push messages and check results | [[iOS](./ios-push/#push)] [[Android](./aos-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- Real-time delivery |
| Leaderboard           | (TOAST Integration)<br> Retrieve and register real-time ranking of large-capacity data |                                          | [[Wrapping API](./api-guide/#leaderboard)] |                                          |
| Webview               | SDK provides default WebView UI<br/>Provides both system pop-up and TOAST UI | [[iOS](./ios-ui/#webview)] [[Android](./aos-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [Operator] Maintenance | (Operation) Maintenance                               |                                          | [[Check for Maintenance](./api-guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- Register or cancel maintenance |
| [Operator] Notice      | (Operation) Urgent Notification <br> -  In pop-ups while user is executing an app |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-Register Notice |
| [Operator] Image Notice         | (Operation) Image Notice function <br> -  Exposes the image notice in in-game popup format | [[Android](./aos-ui/#imagenotice)] [[iOS](./ios-ui/#imagenotice)] [[Unity](./unity-ui/#imagenotice)] <br/> - Expose image notice |                             | [[Image Notice]](./oper-operation/#image-notice) <br/>- Manage image notice |
| [Operator] Ban         | (Operation) Register/Release banned game users <br> -  Register/Release banned game users | [[iOS](./ios-authentication/#get-banned-user-information)][[Android](./aos-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -Check information of banned users |   [[Retrieving the ban history of game users](./api-guide/#ban-histories)                                       | [[Ban]](./oper-ban/#ban) <br/>-Register and Release Ban |
| [Operator] Coupon         | (Operation) Manage coupon<br>- issue, view history |  |                                      [[Validate coupon and change coupon status](./api-guide/#coupon)  | [[Coupon]](./oper-coupon) <br/>- Issue coupon | | [Operator] Customer Service         | (Operation) Receive and process 1:1 inquiry <br> -  Manage FAQ and notices | [[Android](./aos-etc/#contact)] [[iOS](./ios-etc/#contact)] [[Unity](./unity-etc/#contact)] <br/> - Display the Customer Center web page in WebView |                                        | [[Customer Service]](./oper-customer-service) <br/>- Process inquiries sent to customer center<br>- Manage FAQ/notices |
| [Operator] Customer Service         | (Operation) 1:1 inquiry reception and handling <br> - FAQ, notice management | [[Android](./aos-etc/#contact)] [[iOS](./ios-etc/#contact)] [[Unity](./unity-etc/#contact)] <br/> - Display customer center webpages in WebView | | [[Customer Service]](./oper-customer-service) <br/>- Handling Customer Center Inquiries<br>- FAQ/Notice Management


## Console Role

As for the standard member policy and permission for NHN Cloud, see the following guide.
* [NHN Cloud > Console User Guide > Manage Members](https://docs.nhncloud.com/en/nhncloud/en/console-guide/#manage-members)

### Manage Role

**Console > Project Settings > Manage Members**
On the Project Settings screen, you can add Toast members, or grant permissions to registered members individually. Multiple permissions can be granted to a single member.
![Project Permission](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/Overview/en/gamebase_overview_01_en_240105.png)

**Console > Project Settings > Manage Group Permission**
For the ease of operation, group permission can be granted to a Toast member by registering frequently used permissions as *group permission*.
![Project Permission Group](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/Overview/en/gamebase_overview_02_en_240105.png)

**Console > Organization Settings > Project Common Permission Group Settings**
On the Organization Admin screen, you can manage the permission group commonly used in the project within the organization.
![Organization Permission Group](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/Overview/en/gamebase_overview_03_en_240105.png)

### Permissions list provided by Gamebase

| Services | Permission | Description |
| --- | --- | --- |
| Gamebase | ADMIN | **Full screen access and control**<br>Create/Read/Update/Delete Gamebase service |
| Gamebase | ANALYTICS VIEWER - ALL | Read all indexes()<br>Can download the index results in Excel file |
| Gamebase | ANALYTICS VIEWER - EXCLUDING SALES | Read all indexes except for sales |
| Gamebase | ANALYTICS VIEWER - ONLY REAL-TIME | Read real-time indexes |
| Gamebase | APP ADMIN | Create/Read/Update/Delete APP menu |
| Gamebase | APP VIEWER | Read APP menu |
| Gamebase | BAN ADMIN | Create/Read/Update/Delete User Ban menu |
| Gamebase | BAN VIEWER | Read User Ban menu |
| Gamebase | COUPON ADMIN | Create/Read/Update/Delete Coupon menu |
| Gamebase | COUPON VIEWER | Read Coupon menu |
| Gamebase | CS ADMIN | Create/Read/Update/Delete Customer Center menu |
| Gamebase | CS INQUIRY SUPPORT | Read/Update Customer Center inquiry menu and Read Member menu |
| Gamebase | IAP ADMIN | Create/Read/Update/Delete Purchase menu |
| Gamebase | IAP VIEWER | Read Purchase menu |
| Gamebase | LEADERBOARD ADMIN | Create/Read/Update/Delete Leaderboard menu |
| Gamebase | LEADERBOARD VIEWER | Read Leaderboard menu |
| Gamebase | MANAGEMENT ADMIN | Create/Read/Update/Delete Management menu |
| Gamebase | MEMBER ADMIN | Create/Read/Update/Delete Member menu |
| Gamebase | MEMBER VIEWER | Read Member menu |
| Gamebase | MEMBER FILE DOWNLOAD | Read Member menu and download Member files |
| Gamebase | OPERATION ADMIN | Create/Read/Update/Delete Operation menu |
| Gamebase | OPERATION VIEWER | Read Operation menu |
| Gamebase | PUSH ADMIN | Create/Read/Update/Delete Push menu |
| Gamebase | PUSH VIEWER | Read Push menu |

* This is an example of managing the permissions by creating a frequently used permission group in the project. You can create and manage an appropriate permission group as necessary in the game.

| Services | Permission | Administrator/Business | Development | CS |
| --- | --- | --- | --- | --- | 
| Gamebase | ADMIN | ● | | |
| Gamebase | ANALYTICS VIEWER - ALL |  |  | |
| Gamebase | ANALYTICS VIEWER - EXCLUDING SALES |   |  | |
| Gamebase | ANALYTICS VIEWER - ONLY REAL-TIME |  | ● | |
| Gamebase | APP ADMIN | |  ● | |
| Gamebase | APP VIEWER | |  | |
| Gamebase | BAN ADMIN | |  ● | ●|
| Gamebase | BAN VIEWER | |  | |
| Gamebase | COUPON ADMIN | | ● | |
| Gamebase | COUPON VIEWER | |  |● |
| Gamebase | CS ADMIN | |  | |
| Gamebase | CS INQUIRY SUPPORT | |  | ● |
| Gamebase | IAP ADMIN | | ● | |
| Gamebase | IAP VIEWER | |  |● |
| Gamebase | LEADERBOARD ADMIN || ● | | 
| Gamebase | LEADERBOARD VIEWER | |  | |
| Gamebase | MANAGEMENT ADMIN | | ● | |
| Gamebase | MEMBER ADMIN | | ● | ● |
| Gamebase | MEMBER VIEWER | |  |  |
| Gamebase | MEMBER FILE DOWNLOAD | |  | |
| Gamebase | OPERATION ADMIN | | ● | |
| Gamebase | OPERATION VIEWER | |  |● |
| Gamebase | PUSH ADMIN | | ● | |
| Gamebase | PUSH VIEWER | |  | ● |

## Privacy Policy
### Information on Privacy and Compliance
In the process of using the Gamebase service, the Customer shall be aware of and comply with its obligations under relevant laws and regulations (Information and Communications Network Act, Personal Information Protection Act, E-commerce Act, etc.)
As a service provider, NHN Cloud is a partner that actively supports customers to use the Gamebase service safely, and NHN Cloud is not responsible for posts managed by customers, personal information of users, etc.
In addition, in the process of collecting users' personal information, a delegation relationship may arise between the customer and NHN Cloud regarding the processing of personal information.
Customers who are in the position of a trustor may enter into a separate written consignment agreement with NHN Cloud as a trustee, and may notify the following in the privacy policy operated by the customer.

* Trustee: NHN Cloud Corp.
* Duties of Trustee: Provision of Gamebase