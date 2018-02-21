## Game &gt; Gamebase &gt; Operator Guide &gt; Purchase

You can register information related to In-App Purchase (IAP) and retrieve details.<br/>
In Gamebase, TOAST IAP service is provided.<br/>
<br/>
## Store
Register stores to sell products in games.<br/>
Register a new store on the **Store Information List** of the **Store** tab, or manage registered stores.<br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)


### Register

Click **Register** on the **Store Information List** to register a new store.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)

* **Store**<br/> Select an external store to register.<br />If it is not on the list, contact [Customer Center](https://toast.com/support/inquiry).<br />
* **App Name** <br/> Enter the name of a game to register.<br />
* **Store App ID** <br/> Enter information issued by store.<br />
* **Use or Not**<br/> Select whether to use the store or not.<br />

### Modify

Retrieve or modify detail information of registered stores on the list.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- Select a registered store on the list to retrieve detail information.<br />
- Click **Modify** to modify information such as app name, store app, and use or not, but not store App ID.<br />
- Click **Delete** to delete information: only for the stores that are Not in Use.<br />
  <br/>

## Item

Register items to sell at each store. <br>
Register a new item on the **Store** tab, or manage registered stores. Items of all stores will show, and filtering is also available for each store.<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)

### Register

Click **Register** on the **Store Information List** to register a new item.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)

* **Store**<br />  Select an external store to register.<br />  If it is not on the list, need to register the store first in the **Store** menu.<br />
* **Item Name**<br /> Enter the item information issued after store is registered.<br />Can show the item name registered in the game.<br>
* **Store Item ID**<br />Enter the name of an item to register.<br />
* **Use or Not**<br />  Select whether to sell the item or not<br />

### Modify

Retrieve or modify detail information of registered items on the list.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)
- Select each item on the Item List to retrieve detail information.<br />
- Click **Modify** to change information except store information and item sequence.<br />
- Click **Delete** to delete item information.<br />
  <br/>

## Transactions
Retrieve payment information.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.1.png)
<br />

Retrieve payment information of choice, by using the search conditions as below. <br>

### Search Conditions
- **Store**: Information of a store where payment has been made
- **Date**: Time when a user tried to purchase
- **Payment Sequence**: Original number to identify payments within Gamebase
- **Item Sequence**: Number of an item a user purchased in an app (can be found under the &#39;Item&#39; tab.)
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

A manual payment processing function will be provided in the Console, when TOAST Cloud IAP fails to complete payment after a successful payment at external store (App Store, Google Play, and etc.) and hence items are not provided: will be updated in close future.<br />
