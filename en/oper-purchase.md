## Game > Gamebase > Console Guide > Purchase

You can register information related to In-App Purchase (IAP) and retrieve details.
In Gamebase, TOAST IAP service is provided.

## Store

Register stores to sell products in games.
Register a new store on the **Store Information List** of the **Store** tab, or manage registered stores.

![gamebase_purchase_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_01_201812_en.png)

### Register

Click **Register** on the **Store Information List** to register a new store.

![gamebase_purchase_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_02_201812_en.png)

* **Store**  Select an external store to register.  If it is not on the list, contact [Customer Center](https://toast.com/support/inquiry).
* **App Name**   Enter the name of a game to register.
* **Store App ID**   Enter information issued by store.
* **Use or Not**  Select whether to use the store or not.

### Modify

Retrieve or modify detail information of registered stores on the list.

![gamebase_purchase_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_03_201812_en.png)

- Select a registered store on the list to retrieve detail information.
- Click **Modify** to modify information such as app name, store app, and use or not, but not store App ID.
- Click **Delete** to delete information: only for the stores that are Not in Use.

## Item

Register items to sell at each store.
Register a new item on the **Store** tab, or manage registered stores. Items of all stores will show, and filtering is also available for each store.

![gamebase_purchase_04_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_04_201812_en.png)

### Register

Click **Register** on the **Store Information List** to register a new item.

![gamebase_purchase_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_05_201812_en.png)

* **Store**  Select an external store to register.  If it is not on the list, need to register the store first in the **Store** menu.
* **Item Name** Enter the item information issued after store is registered.Can show the item name registered in the game.
* **Store Item ID**Enter the name of an item to register.
* **Use or Not**  Select whether to sell the item or not.

### Modify

Retrieve or modify detail information of registered items on the list.

![gamebase_purchase_06_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_06_201812_en.png)

- Select each item on the Item List to retrieve detail information.
- Click **Modify** to change information except store information and item sequence.
- Click **Delete** to delete item information.

## Transactions

Retrieve payment information.

![gamebase_purchase_07_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_07_201812_en.png)

Retrieve payment information of choice, by using the search conditions as below.
You can download the purchase history by clicking on the button in the top-right corner.

#### Search conditions
Different search items show depending on the selected search type.

##### (1) General Search
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_10_201911.png)

With general search, you can query results that meet the following search conditions:
- **Store**: Information of a store where payment has been made
- **Date**: Time when user attempted to purchase
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Item Sequence**: Number of an item a user purchased in an app (available on the 'Item' tab.)
- **User ID**: ID of the paying user
- **Sort Order**: Ascending or descending order by registration date
- **Payment Status**: Status of payment

##### (2) Transaction ID Search
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_11_201911.png)

Query is available with Transaction ID which is created with payment.

##### (3) Receipt Search
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_12_201911.png)
Query is available with receipt information paid on payment.

#### Search Results
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

### Changing Payment Status

These are the types of payment status:

- **Success**
  - Transaction Complete
    - This means that transaction was done successfully.
    - The status can be changed to Refund
- **Reserved**
  - Transaction in Progress
  - This means that the transaction through the store hasn't been done or verified yet.
  - The status can be changed to Success or Refund.
- **Failure**
  - Transaction Verification Failed
  - The verification process after the transaction resulted in an error from the store.
  - The status can be changed to Success or Refund.
- **Refund**
  - Refund Complete
  - The admin has manually changed the payment status after the refund process in the store.
  - The status can't be changed to other status.

#### Changing to Success

![gamebase_purchase_08_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_08_201812_en.png)

In order to proceed, you need to provide **Receipt Number, Value, and Currency**.

#### Changing to Refund

![gamebase_purchase_09_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_09_201812_en.png)

No additional information is required. Once it has been changed to Refund status, it's set for good.

#### Validate Receipt
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* Validates transaction of the retrieved receipt.
* You can see the results of comparing each field. The response value is provided in JSON format, so you can directly check the data if needed.
* For now, only App Store transactions can be validated.

## Monitor Abusive Payments

Abusive payment information can be queried and banned.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing1_1.0.png)

Payment and refund information can be queried by search conditions as below.
Click **Download** on top right to download payment and refund list at any time.

#### Search Conditions

- **Refund Date**: Time when a user item was refunded
- **User ID**: ID of the paying user
- **Refund Count**: Refund count of the user: queried more than input value.
- **Refund Amount**: Refund amount of the user: queried more than input amount.

#### Search Results
- **User ID**: ID of the paying user
- **Store**: Information of a store where payment has been made
- **Refund Count**: Refund count of the user
- **Refund Amount**: Refund amount of the user
- **Purchase Count (Query Period)**: Purchase count of the user during query period
- **Purchase Amount (Query Period)**: Paid amount of the user during query period
- **Purchase Count (Accumulated)**: Total accumulated purchase count of the user
- **Purchase Amount (Accumulated)**: Total purchase amount of the user
- **Purchase Count (Recent 3 months)**: Purchase count of the user during the last three months
- **Purchase Amount (Recent 3 months)**: Purchase amount of the user during the last three months
- **Status**:  Current status of the user
- **Manual Lockdown**: Ban or service lockdown depending on the user status

#### Manual Lockdown
![gamebase_member_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812.png)

The feature allows to change status of the game user's account.
Each status is available for change like below:

- **Normal**: Can be changed to ban or withdrawal. Please note that, once withdrawn, user's account information cannot be reverted.  
- **Ban**: Ban can be released.
- **Withdrawal**: The button does not show.

#### Check Purchase List

With the click of a user ID on the list, purchase details during search period can be queried.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing2_1.0.png)

#### Purchase History
- **Reserved Purchase Date**: Time when user attempted or completed with a purchase
- **Refund Date**: Time when a user item was refunded
- **Transaction ID**: Original number to identify payments within Gamebase
- **Store**:  Information of a store where payment has been made
- **Item Name**: Name of actual item which the user purchased from the app
- **Price**: Price of item purchased by the user
- **Currency**: Type of currency used by the user for purchase
- **Payment Status**: Current status of payment

#### Auto Lockdown for Abusive Payment

Click **Enable** to enable the auto lockdown setting, and enter value.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing3_1.0.png)

#### Setting Information

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing4_1.0.png)

* **Ban Period**  Enter the ban period to be applied for auto lockdown.
    * **Permanent Ban**: To be selected for a permanent ban.
    * **Temporary Ban**: Enter period for a temporary ban (days).  
* **Setting Conditions for Auto Lockdown** Users that meet set conditions are automatically locked down. At least one setting is required.
    * **Refund Count**: Enter refund count due to abusive acts.
    * **Refund Amount**: Enter refund amount due to abusive acts.
* **Setting Messages for Auto Lockdown**  
    * Enter a ban message to show to users.
    * Enter messages in multiple languages on a template to allow easy re-use. Select and register a pre-registered template.
* **Deleting Leaderboard**
    * Set whether to delete Leaderboard of the game user as well, along with auto-lockdown.
    * With registration, game user's data is to be deleted from the leaderboard, and note that <font color="red">such data cannot be recovered</font>.
