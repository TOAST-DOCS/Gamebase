## Game > Gamebase > Console Guide > Member

Retrieve information of members who are logged in a game.


### Search Member

Enter User ID to search for member information.
User ID is a user identifier which is automatically issued at a first-time login by Gamebase. To lessen confusion while delivered, "ABCDFGHJKLMNPQRSTWXYZ1346789" are applied only, in order to prevent confusion due to similar pronunciation.
IdP is not used for signing in or out, but serves as a unique identifier provided from Id Provider. So you need to be cautious when you want to search by IdP.

Detailed information of searched users are displayed in the above, while history data such as login, mapping, purchase, ban, and playtime are shown under, as taps.



### Detail Information
![gamebase_member_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_01_201812_en.png)

**User **

- **User ID**: Gamebase User ID
- **Country (USIM)**: USIM country code of a user device. When it fails to collect, it shows '**ZZ**'. Go to **Login History** below to check the country code of your device.
- **Latest Login Date**: The last time a user logged in.
- **Registration Date**: The first time a user logged in.
- **Account Status**
  - **OK**: Normal users. Click **Ban** to manually change status.
  - **Banned**: Users who are banned from abusive acts. Click **Release Ban** to manually change status.
  - **Withdraw**: Users who have withdrawn.
- **Push Token**: Shows push token details.

#### Changing Account Status
![gamebase_member_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812_en.png)

You can change user account status to these below:

- **OK**: You can change to Banned or Withdrawn status. When the account is withdrawn, all the information is lost and can't be recovered. 
- **Banned**: You can lift a ban on the account.
- **Withdrawn**: 'Modify Status' button won't be shown.

**Identity Provider **

In Gamebase, many external IdPs can be integrated. In short, a user can log in by registering two IdPs, such as Facebook and Google in one user ID. IdP can be registered when **Login with Specific IDP** or '**Add Mapping'** API is called from SDK.

- **IdP**: External IdPs (Guest, Facebook, PAYCO, or Google)
- **Idp ID**: ID provided by an external IdP (Facebook no, or PAYCO ID)
- **Registration Date**: The first time when a user registered an IdP.

#### Mapping
You can map the user's IdP to other account.
This only works if the target account status is OK. 
 *Press the button pointing to the right to map the IdP*.
 You can revoke the change - as long as you didn't press the 'Add Mapping' button - by pressing the subtract button.
 *When a guest account is mapped to an IdP, the guest information will be lost*.
 When the account has only one IdP bound but it's mapped to other account, the account status will change to **Lost** and it can't be used anymore.

 ##### Mapping Example

![gamebase_member_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_03_201812_en.png)

#### Unmapping
 If the account has been mapped multiple times, it can be unmapped.
The button only works when the account has been mapped more than twice.
 *Clicking on the button will make the modal appear*

![gamebase_member_04_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_04_201812_en.png)

 Clicking on the 'Remove' button will show final confirmation message along with IdP information.

### Login History
![gamebase_member_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_05_201812_en.png)

Retrieve login history of a searched user.
For an initial search, the duration is a recent one day or a date of choice. However, history data is provided for recent three months (or 90 days) only.
Can add history when SDK calls login-related APIs.

- **Login Date**: User's login time to an app.
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
![gamebase_member_06_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_06_201812_en.png)

Retrieve mapping and remove mapping history of a searched user. Shows all history data of the recent 3 months (90 days).

* **IdP ID**: ID information used for Idp login.
* **IdP**: Mapped IdP information.
* **Date**: Time when mapping between IdP ID and Gamebase ID is done.
* **Type**: Details of mapping
  - AAM: Add Mapping
  - ARM: Remove Mapping
  - AFR: Remove Mapping by force
  - GMG: Create a guest account
  - OMG: Create an IdP account

Clicking on the mapped IdP will show the mapping history modal based that IdP.
![gamebase_member_07_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_07_201812_en.png)

### Purchase History
![gamebase_member_08_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_08_201812_en.png)

Retrieve purchase history of user products.
Can search by date, and history data can be retrieved up to 1 month (30 days).

- **Transaction ID**: Original number to identify payments within Gamebase
- **Store**: Information of a store where payment has been made
- **Item Name**: Name of an item a user purchased in an app
- **Price**: Price of an item a user purchased
- **Currency**: Type of currency used to purchase
- **Consume**: Whether a paid item has been provided or not
- **Payment Status**: Current status of payment
- **Store Reference Key**: Original payment number issued by store
- **Reservation date**: Time when a user tried or completed purchasing
- **Refund Date**: Time when a user item was refunded

조회한 사용자의 상품 구입 내역을 조회합니다.
원하는 날짜를 입력하여 조회할 수 있으며 조회 가능한 최대 날짜는 1개월(30일)입니다.

### Ban History
![gamebase_member_09_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_09_201812_en.png)

Can retrieve ban history of a searched user.
Can search by date, and history data can be retrieved up to 1 month (30 days).

- **Start Date**: Start time to apply banning
- **End Date**: End time to apply banning
- **Template**: Name of template applied to register banning
- **Reason**: Actual reasons of an operator to ban a user
- **Registered User/Registration Date**: Operator who registered banning/System information and date
- **Release Reason**: Actual reasons of an operator to release banning
- **Released User/Release Date**: Operator who released banning/system information and date

### Playtime
![gamebase_member_10_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_member_10_201812_en.png)

Retrieve playtime of a searched user by date.
Can search by date, and history data can be retrieved up to 1 month (30 days).

### Withdraw History
![image alt](./image/Operators_Guide/Console_Member_WithdrawHistory1_1.1.png)
조회한 사용자가 탈퇴한 사용자라면 탈퇴 이력을 보여줍니다.
이 메뉴는 탈퇴 유저를 조회할 경우에만 나타나며 유저의 탈퇴경로를 조회할 수 있습니다.
