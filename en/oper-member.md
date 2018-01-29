## Game &gt; Gamebase &gt; Operator Guide &gt; Member

## Member

Retrieve information of members who are logged in a game.


### Search Member

Enter User ID to search for member information.<br/>
User ID is a user identifier which is automatically issued at a first-time login by Gamebase. To lessen confusion while delivered, &quot;ABCDFGHJKLMNPQRSTWXYZ1346789&quot; are applied only, in order to prevent confusion due to similar pronunciation.<br/><br/>

Detailed information of searched users are displayed in the above, while history data such as login, mapping, purchase, ban, and playtime are shown under, as taps. <br/>




### Detail Information
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_1.4.png)

**User ** <br/>

- **User ID**: Gamebase User ID
- **Country (USIM)**: USIM country code of a user device. When it fails to collect, it shows &#39; **ZZ**&#39;. Go to **Login History** below to check the country code of your device.<br/>
- **Latest Login Date**: The last time a user logged in.<br/>
- **Registration Date**: The first time a user logged in.<br/>
- **Account Status**<br/>
  - **OK**: Normal users. Click **Ban** to manually change status.<br/>
  - **Banned**: Users who are banned from abusive acts. Click **Release Ban** to manually change status.<br/>
  - **Withdraw**: Users who have withdrawn.<br/>

**Identity Provider ** <br/>

In Gamebase, many external IdPs can be integrated. In short, a user can log in by registering two IdPs, such as Facebook and Google in one user ID. IdP can be registered when **Login with Specific IDP** or &#39; **Add Mapping&#39;** API is called from SDK.<br/>

- **IdP**: External IdPs (Guest, Facebook, PAYCO, or Google)
- **Idp ID**: ID provided by an external IdP (Facebook no, or PAYCO ID)
- **Registration Date**: The first time when a user registered an IdP.

### Login History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_LoginHistory1_1.2.png)

Retrieve login history of a searched user. <br />
For an initial search, the duration is a recent one day or a date of choice. However, history data is provided for recent three months (or 90 days) only.<br />
Can add history when SDK calls login-related APIs.<br/>

- **Login Date**: User&#39;s login time to an app.
- **Login Type**: Type of authentication applied for a user login (such as IdPLogin/Guest). In-parentheses refers to IdProvider information which was actually used.
- **OS / Ver**: OS (iOS/Android/WebGL) and OS version information used for user login.
- **Device model**: Name of a device model applied for user login.
- **Device Key**: Original identifier (Android:Android id, iOS:IDFV) of a device applied for user login.
- **Device Country Code**: Country code configured on a device applied for user login.
- **USIM Country Code**: Country code configured at USIM card for user login.
- **Telecom**: Information of telecommunication provider used for login.
- **Network**: Network type applied for user login (Wifi/3G/LTE).
- **Language Code**: Language code information configured on a device for user login.
- **Store Code**: Store information where a user download apps.
- **Client Version**: The client version information when an app is downloaded.
- **Gamebase SDK Version**: Version information of Gamebase SDK applied to an app
- **etc**: Any other information than above items applied for a login.

### Mapping History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_1.2.png)

Retrieve mapping and remove mapping history of a searched user. Shows all history data of the recent 3 months (90 days).<br />

- **Based on User ID**: Retrieve by retrieved user ID.
- **Based on IdP ID**: Retrieve by the currently mapped IdP ID. <br/>
  When a user ID is mapped to Facebook and Google IdP, the list will display the two IdP IDs.<br/>

* **IdP ID**: ID information used for Idp login.
* **IdP**: Mapped IdP information.
* **Date**: Time when mapping between IdP ID and Gamebase ID is done.
* **Type**: Details of mapping
  - AAM: Add Mapping
  - ARM: Remove Mapping
  - AFR: Remove Mapping by force
  - GMG: Create a guest account
  - OMG: Create an IdP account

### Purchase History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_PurchaseHistory1_1.0.png)
Retrieve purchase history of user products.<br />
Can search by date, and history data can be retrieved up to 1 month (30 days).<br />

- **Payment Sequence**: Original number to identify payments within Gamebase
- **Store**: Information of a store where payment has been made
- **Item Name**: Name of an item a user purchased in an app
- **Price**: Price of an item a user purchased
- **Currency**: Type of currency used to purchase
- **Consume**: Whether a paid item has been provided or not
- **Payment Status**: Current status of payment
- **Store Reference Key**: Original payment number issued by store
- **Registration Date**: Time when a user tried or completed purchasing
- **Refund Date**: Time when a user item was refunded

### Ban History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_BanHistory1_1.0.png)

Can retrieve ban history of a searched user.<br />
Can search by date, and history data can be retrieved up to 1 month (30 days).<br />

- **Start Date**: Start time to apply banning
- **End Date**: End time to apply banning
- **Template**: Name of template applied to register banning
- **Reason**: Actual reasons of an operator to ban a user
- **Registered User/Registration Date**: Operator who registered banning/System information and date
- **Release Reason**: Actual reasons of an operator to release banning
- **Released User/Release Date**: Operator who released banning/system information and date

### Playtime
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Playtime1_1.2.png)
Retrieve playtime of a searched user by date.<br />
Can search by date, and history data can be retrieved up to 1 month (30 days).<br />