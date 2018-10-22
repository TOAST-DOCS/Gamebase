## Game > Gamebase > Console Guide > Member

Retrieve information of members who are logged in a game.


### Search Member

Enter User ID to search for member information.
User ID is a user identifier which is automatically issued at a first-time login by Gamebase. To lessen confusion while delivered, "ABCDFGHJKLMNPQRSTWXYZ1346789" are applied only, in order to prevent confusion due to similar pronunciation.
IdP ID는 Id Provider에서 제공하는 아이디 정보로써 로그인 시 입력하는 정보가 아닌 Id Provider내의 고유 식별자를 의미합니다. 따라서 IdP ID 항목으로 검색하고자 할 경우 검색정보 입력에 주의가 필요합니다.

Detailed information of searched users are displayed in the above, while history data such as login, mapping, purchase, ban, and playtime are shown under, as taps.



### Detail Information
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_1.6.png)

**User **

- **User ID**: Gamebase User ID
- **Country (USIM)**: USIM country code of a user device. When it fails to collect, it shows '**ZZ**'. Go to **Login History** below to check the country code of your device.
- **Latest Login Date**: The last time a user logged in.
- **Registration Date**: The first time a user logged in.
- **Account Status**
  - **OK**: Normal users. Click **Ban** to manually change status.
  - **Banned**: Users who are banned from abusive acts. Click **Release Ban** to manually change status.
  - **Withdraw**: Users who have withdrawn.
- **푸시 토큰**: 유저의 푸시 토큰 정보 조회.

####계정 상태 변경
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_2.2.png)
조회한 유저의 계정 상태를 변경할 수 있는 기능입니다.
상태별 변경할 수 있는 경우는 아래와 같습니다.
- **정상**: 이용정지, 탈퇴상태로 변경이 가능합니다. 탈퇴시에는 해당 계정정보를 되돌릴 수 없으므로 처리 전 확인 및 주의가 필요합니다.
- **이용 정지**: 이용정지 해제를 진행할 수 있습니다.
- **탈퇴**: 해당 버튼이 노출되지 않습니다.

**Identity Provider **

In Gamebase, many external IdPs can be integrated. In short, a user can log in by registering two IdPs, such as Facebook and Google in one user ID. IdP can be registered when **Login with Specific IDP** or '**Add Mapping'** API is called from SDK.

- **IdP**: External IdPs (Guest, Facebook, PAYCO, or Google)
- **Idp ID**: ID provided by an external IdP (Facebook no, or PAYCO ID)
- **Registration Date**: The first time when a user registered an IdP.

#### 매핑 추가

조회한 유저의 Id Provider 정보를 추가할 수 있는 기능입니다.
연결하고자하는 IdP를 가지고 있는 유저의 정보가 **정상**일 경우에만 매핑 추가 작업이 가능합니다.
* 제공 유저의 IdP 설정란에서 연결하고자 하는 Id Provider 정보를 1번 버튼을 통해 오른쪽화면에 추가한 후, 아래 매핑 추가 버튼을 통해 매핑 추가 작업을 진행합니다.
* 잘못 추가했다면 매핑 추가 버튼을 누르기 전에는 2번 버튼을 통해 언제든지 다른 Id provider로 교체 가능합니다.
* 제공 받는 유저가 GUEST정보만 가지고 있을 경우에는 새로운 Id Provider정보가 추가되면서 기존의 Guest정보는 유실되므로 작업 진행 시 주의가 필요합니다.
* 제공 유저의 Id Provider 정보가 한개일 경우 해당작업을 진행하면 제공 유저 정보는 **유실**상태로 변경되어 더이상 사용할 수 없으므로 작업 진행 전 확인이 필요합니다.
##### 제공 전 예시
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_AddMapping_1.3.png)
##### 제공 후 예시
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_AddMapping_2.1.png)

#### 매핑 해제
다중 매핑이 이루어진 계정의 경우 요청에 따라 Id Provider 정보 연동을 해제할 수 있습니다.
각각의 계정은 최소 1개의 연결정보가 존재해야 하므로 2개 이상의 연결정보가 존재할 때만 버튼이 활성화 됩니다.
* 버튼을 누르면 아래와 같이 연결된 Id Provider 정보와 함께 매핑 해제 버튼이 노출됩니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_RemoveMapping_1.0.png)
* 해제 버튼을 누를 경우 아래와 같이 최종 확인창과 함께 Id Provider 정보를 확인합니다. 확인버튼을 누르시면 매핑 해제가 진행됩니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_RemoveMapping_2.0.png)

### Login History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_LoginHistory1_1.2.png)

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
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_1.4.png)

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

매핑된 IDP 이력을 클릭할 경우 해당 IdP를 기준으로 Gamebase ID에 매핑된 이력을 보여줍니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_2.1.png)

### Purchase History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_PurchaseHistory1_1.0.png)

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
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_BanHistory1_1.0.png)

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
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Playtime1_1.2.png)

Retrieve playtime of a searched user by date.
Can search by date, and history data can be retrieved up to 1 month (30 days).

### Withdraw History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_WithdrawHistory1_1.1.png)
조회한 사용자가 탈퇴한 사용자라면 탈퇴 이력을 보여줍니다.
이 메뉴는 탈퇴 유저를 조회할 경우에만 나타나며 유저의 탈퇴경로를 조회할 수 있습니다.
