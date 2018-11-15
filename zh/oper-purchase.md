## Game > Gamebase > Console Guide > Purchase

You can register information related to In-App Purchase (IAP) and retrieve details.
In Gamebase, TOAST IAP service is provided.

## Store

Register stores to sell products in games. 
Register a new store on the **Store Information List** of the **Store** tab, or manage registered stores.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)

### Register

Click **Register** on the **Store Information List** to register a new store.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)

* **Store**  Select an external store to register.  If it is not on the list, contact [Customer Center](https://toast.com/support/inquiry).
* **App Name**   Enter the name of a game to register.
* **Store App ID**   Enter information issued by store.
* **Use or Not**  Select whether to use the store or not.

### Modify

Retrieve or modify detail information of registered stores on the list.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- Select a registered store on the list to retrieve detail information.
- Click **Modify** to modify information such as app name, store app, and use or not, but not store App ID.
- Click **Delete** to delete information: only for the stores that are Not in Use.

## Item

Register items to sell at each store. 
Register a new item on the **Store** tab, or manage registered stores. Items of all stores will show, and filtering is also available for each store.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)

### Register

Click **Register** on the **Store Information List** to register a new item.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)

* **Store**  Select an external store to register.  If it is not on the list, need to register the store first in the **Store** menu.
* **Item Name** Enter the item information issued after store is registered.Can show the item name registered in the game.
* **Store Item ID**Enter the name of an item to register.
* **Use or Not**  Select whether to sell the item or not.

### Modify

Retrieve or modify detail information of registered items on the list.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)
- Select each item on the Item List to retrieve detail information.
- Click **Modify** to change information except store information and item sequence.
- Click **Delete** to delete item information.

## Transactions

Retrieve payment information.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.4.png)

Retrieve payment information of choice, by using the search conditions as below.
결제 내역은 우측 상단의 다운로드 버튼을 통해 언제든지 다운로드 받으실 수 있습니다.

### Search Conditions

- **Store**: Information of a store where payment has been made
- **Date**: Time when a user tried to purchase
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Item Sequence**: Number of an item a user purchased in an app (can be found under the 'Item' tab.)
- **User ID**: ID of a user who made payments
- **Sort Order**: Ascending or descending order, by registration date
- **결제상태**: 결제 상태를 기준으로 정보를 조회

### Search Results
- **Transaction ID**: Original number to identify payments within Gamebase
- **Store**: Information of a store where payment has been made
- **User ID**: ID of a user who made payments
- **Item Name**: Name of an item a user purchased in an app
- **Price**: Price of an item a user purchased
- **Currency**: Type of currency used to purchase
- **Consume**: Whether a paid item has been provided or not
- **Payment Status**: Current status of payment
- **Store Reference Key**: Original payment number issued at a store
- **Reservation Date**: Time when a user tried or completed purchasing
- **Refund Date**: Time when a user item was refunded

### 결제 상태 변경
조회된 결제정보의 상태는 아래와 같으며 각각의 상태는 아래와 같습니다.
- **Success**
	- 결제 완료
    - 결제 처리가 정상적으로 완료됬을 경우를 의미합니다.
    - Refund상태로 변경 가능합니다.
- **Reserved**
	- 결제 진행중
	- 스토어를 통한 결제가 더이상의 진행이 되지 않거나 결제검증까지 진행되지 않은 경우를 의미합니다.
	- Success, Refund상태로 변경 가능합니다.
- **Failure**
	- 결제 검증 실패
	- 스토어에서 결제를 진행했으나 결제검증에서 오류가 난 경우를 의미합니다.
	- Success, Refund상태로 변경 가능합니다.
- **Refund**
	- 환불 완료
	- 관리자가 수동으로 마켓에서 환불처리에 대한 여부를 업데이트한 경우입니다.
	- 다른 결제상태로 변경이 불가능합니다.

#### Success 변경
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_1.0.png)
결제 진행시 발급받은 **영수증 번호**, **가격**, **통화**정보를 입력해야 상태 변경이 가능합니다.

#### Refund 변경
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_2.0.png)
별다른 추가정보의 입력 없이 상태를 선택한 후 변경을 선택합니다.
변경된 결제 정보는 이후 변경이 불가능하므로 신중하게 확인 후 진행해야 합니다.

### 영수증 검증
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* 조회된 영수증을 기반으로 해당 결제건이 유효한 지 검증할 수 있습니다.
* 각 필드의 비교결과를 알려주며 스토어로부터 받은 응답값을 Json형식으로 제공하므로 필요한 경우 데이터를 직접 확인하실 수 있습니다.
* 현재는 App Store 결제건에 대해서만 검증을 제공합니다.