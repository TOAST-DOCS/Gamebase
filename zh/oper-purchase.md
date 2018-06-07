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

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.1.png)

Retrieve payment information of choice, by using the search conditions as below. 
### Search Conditions

- **Store**: Information of a store where payment has been made
- **Date**: Time when a user tried to purchase
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Item Sequence**: Number of an item a user purchased in an app (can be found under the 'Item' tab.)
- **User ID**: ID of a user who made payments
- **Sort Order**: Ascending or descending order, by registration date

### Search Results
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Store**: Information of a store where payment has been made
- **User ID**: ID of a user who made payments
- **Item Name**: Name of an item a user purchased in an app
- **Price**: Price of an item a user purchased
- **Currency**: Type of currency used to purchase
- **Consume**: Whether a paid item has been provided or not
- **Payment Status**: Current status of payment
- **Store Reference Key**: Original payment number issued at a store
- **Registration Date**: Time when a user tried or completed purchasing
- **Refund Date**: Time when a user item was refunded

A manual payment processing function will be provided in the Console, when TOAST Cloud IAP fails to complete payment after a successful payment at external store (App Store, Google Play, and etc.) and hence items are not provided: will be updated in close future.