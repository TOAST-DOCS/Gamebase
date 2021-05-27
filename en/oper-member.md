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

- **유저 ID**: Gamebase 사용자 아이디
- **국가코드(USIM)**: 사용자 단말기의 USIM 국가 코드로 수집에 실패하면 'ZZ'로 표기됩니다. 단말기에 설정된 국가 코드를 확인하고 싶다면 하단의 **로그인 이력**에서 확인하세요.
- **마지막 로그인 시간**: 사용자가 가장 마지막에 로그인한 시간
- **가입일**: 사용자가 최초로 로그인한 시간
- **계정 상태**
  - **정상**: 정상 사용자.
  - **이용정지**: 어뷰징 등으로 이용 정지(ban)된 사용자. 우측 상단의 계정 상태 변경 메뉴를 통해 이용정지를 해제할 수 있습니다.
  - **탈퇴**: 탈퇴한 사용자.
- **푸시 부가정보 조회**: 게임 유저의 푸시 토큰 및 태그 정보 조회.
![gamebase_member_12_202104](https://static.toastoven.net/prod_gamebase/gamebase_member_12_202104.png)


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

Retrieve withdraw log of a searched user by date.

## Transfer account
This is available only when the **device transfer** function is used. [Enable the device transfer function](./oper-app/#transfer-account)
You can check the history of issue and verification of the device transfer key for game users. You can unblock blocked keys or reissue expired keys.

![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount1_1.0.png)
**Device transfer issue key**

- **ID**: A device transfer ID issued to a game user
- **Issued on**: A date when the device transfer ID has been issued
- **Expired on**: An expiry date of the device transfer ID
- **Status**: The current status of the issued device transfer ID
  - <font color="white" style="background-color:#88C637">OK</font>: The issued key status is normal. Your device can be transferred using the key.
  - <font color="white" style="background-color:#FB8F37">Blocked</font>: The issued key status is blocked. Your device cannot be transferred using the key.
  - <font color="white" style="background-color:#A1A1A1">Expired</font>: The issued key has expired. Your device cannot be transferred using the key.

**Device transfer history**:
You can retrieve the history of keys issued to game users.
By default, the last issued key is selected. If you select another key, you can check the history of that key.

### Reissuance Transfer account

Click the **Reissue** button to reissue the device transfer key. The previous key cannot be used when a new one is reissued.

![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount_Renewal1_1.0.png)

- **Reissue ID/Password**: Reissue both the ID and password.
- **Reissue Password**: Use the ID issued as it is and reissue the password only.

#### Cautions for reissuance
- The password is displayed only once when it is reissued. Please write down the information somewhere else once it is reissued.
- If you lost it again, there is no way to find the password and in that case, you need to reissue it again.
- The expiration date of the expired key will be updated; however, it is not updated when the account status is either OK or Blocked.
