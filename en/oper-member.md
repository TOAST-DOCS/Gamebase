## Game > Gamebase > Console Guide > Member

Retrieve information of members who are logged in a game.


## Search Member

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
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_WithdrawHistory1_1.1.png)
조회한 사용자가 탈퇴한 사용자라면 탈퇴 이력을 보여줍니다.
이 메뉴는 탈퇴 유저를 조회할 경우에만 나타나며 유저의 탈퇴경로를 조회할 수 있습니다.

## Transfer account
기기 이전 기능을 사용할 경우에만 사용하실 수 있습니다. [기기이전 기능 활성화하기](./oper-app/#transfer-account)
유저의 기기 이전키의 발급 및 검증 이력을 확인할 수 있습니다. 차단된 키의 차단해제나 만료된 키의 재발급이 가능합니다.

![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount1_1.0.png)
**기기 이전 발급 Key**
- **ID** : 유저에게 발급된 기기 이전 ID
- **발급 일자** : 기기 이전 ID가 발급된 일자
- **만료 일자** : 발급된 기기 이전 ID의 만료 일자
- **상태** : 발급된 기기 이전 ID의 현재 상태
  - <font color="white" style="background-color:#88C637">정상</font>: 발급된 키가 정상 상태입니다. 해당 키를 이용하여 기기 이전이 가능합니다.
  - <font color="white" style="background-color:#FB8F37">차단</font>: 발급된 키가 차단된 상태입니다. 발급된 키를 이용하여 기기 이전이 불가능합니다.
  - <font color="white" style="background-color:#A1A1A1">만료</font>: 발급된 키의 사용 기간이 만료된 상태입니다. 발급된 키를 이용하여 기기 이전이 불가능합니다.

**기기 이전 이력**
해당 유저에게 발급된 키의 이력을 조회할 수 있습니다.
기본적으로 가장 최근에 발급된 키가 선택되어 있으며 다른 키를 선택할 경우 선택한 키의 이력을 조회할 수 있습니다.

### 기기 이전 재발급
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount_Renewal1_1.0.png)
**재발급** 버튼을 클릭하여 새로운 기기 이전 키를 다시 발급 할 수 있습니다. 재발급하면 이전에 발급된 키는 더이상 사용할 수 없습니다.
1) **ID,비밀번호 재발급** : ID, 비밀번호를 모두 새롭게 발급합니다. 
2) **비밀번호 재발급** : ID는 이전에 발급된 ID를 그대로 사용하고 비밀번호만을 재발급합니다.

#### 재발급시 주의사항
- 비밀번호는 재발급 시 한번만 노출되므로 재발급 진행 이후 꼭 해당 정보를 별도로 보관하셔야 합니다.
- 보관하지 못하셨을 경우 따로 비밀번호를 찾을 수 있는 방법이 없으므로 재발급을 다시 진행해 주셔야 합니다.
- 만료된 키는 만료 일자가 갱신되나 계정 상태가 정상/차단일 경우에는 만료 일자가 갱신되지 않습니다.

