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

#### Search Conditions

- **Store**: Information of a store where payment has been made
- **Date**: Time when a user tried to purchase
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Item Sequence**: Number of an item a user purchased in an app (can be found under the 'Item' tab.)
- **User ID**: ID of a user who made payments
- **Sort Order**: Ascending or descending order, by registration date
- **Payment Status**: Filter by payment status: Reserved, Failure, Success, and Cancel.
- **Store Reference Key**: Original payment number issued at a store

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