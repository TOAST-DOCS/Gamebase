## Game > Gamebase > Operator Guide > Purchase

You can register information related to in-app purchase and retrieve details. <br/>
In Gamebase, TOAST Cloud In-App Purchase (IAP) service is provided. <br/>
<br/>
## Register Stores
Register stores to sell products in games. <br/> 

### Register

1. Click a project on a console to use the service. 
2. Click **Game > Gamebase > Purchase(IAP)** on the left.  
3. Register a new store on the **Store Information List** of the **App** tab, or manage registered stores. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)

- **Store**: Name of a store registered to provide in-app purchase 
- **App ID**: App's original ID issued at a store 
- **App Name**: Name of an app registered at a store 
- **Use or Not**: Whether a store is in use or not
- **Registration Date**: Date and time when store information is registered  

4. To register a new store, click **Registration** at the bottom of the **Store Information List**. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)

* **Store**<br/>  Select an external store to register. <br />  If a store to register is not on the list, contact administrator. <br />
* **App Name** <br/>  Enter the name of a game to register. <br />
* **Store App ID** <br/>  Enter information issued at a store. <br />
* **Use or Not**<br/>  Select whether a store is now in use or not. <br />

### Modify

Retrieve or modify detail information of registered stores on the list. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- Select each store on the list to retrieve detail information. <br />
- Click **Modify** to change information other than input store information. <br />
- Click **Delete** to delete information of unused stores. <br />
  <br/>
## Register Items 
Register items to sell at each store. Register new items or manage registered items on the **Item** List tab. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)
Items of all stores are displayed, and filtering is also available for each store.<br />

- **Store**: Store where an item is registered
- **Item Sequence**: Original number of an item issued at IAP
- **Item Name**: Name of an item to display in an app 
- **Store Item ID**: ID of an item registered at a store 
- **Use or Not**: Whether an item is now in use or not 
- **Registration Date**: Date and time when an item is registered  

### Register

1. To register an item, click **Registration** on **Item List**. 
2. Enter information on **Item Registration**.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)

* **Store**<br />  Select an external store to store. <br />  If a store to register is not on the list, register store on the **App** menu. <br />
* **Item Name**<br /> Enter item information issued after store registration.<br />
* **Store Item ID**<br /> Enter the name of an item to register.<br />
* **Use or Not**<br />  Select whether the item is on sales or not. <br />
  <br />


### Modify

Retrieve or modify detail information of registered items on the list.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)

- Select each item on the Item List to retrieve detail information.<br />
- Click **Modify** to change information except store information and item sequence.  .<br />
- Click **Delete** to delete items that will not be on sales. <br />
  <br/>
## Transactions
Retrieve payment information of registered items. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.1.png)
<br />
Enter and retrieve information you want for each **Store ID/Date/Payment Sequence/Item No./User ID and Sort Order**.<br />
For items that have not been provided, a function of forceful execution is to be added later.<br />

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