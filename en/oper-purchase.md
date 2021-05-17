## Before Using Purchase Menu
![gamebase_purchase_01_201812](http://static.toastoven.net/prod_gamebase/Console_IAP_Select_currency_1.0.png)
To use the purchase menu, currency must be selected for purchase indicators. 
Only the initial setting is available, and the Analytics sales indicators show in the currenty code as configured. 
Please be cautious with your choice, since the currency code cannot be modified, once selected. 

## Game > Gamebase > Console User Guide > Payment

You can register in-app purchase information and view the history.
Gamebase uses NHN Cloud IAP (In-App Purchase) service.


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

> [Note] Validation of receipts from Google Play Store
>
> When Google's receipt validation system experiences failure, you can use the Gamebase's internal signature validation to properly process the payment by setting the **Receipt validation settings for onetime products** to 1-step validation.
> 2-step validation is always performed for subscription products regardless of the setting.

### Modify

Retrieve or modify detail information of registered stores on the list.

![gamebase_purchase_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_purchase_03_201812_en.png)

- Select a registered store on the list to retrieve detail information.
- Click **Modify** to modify information such as app name, store app, and use or not, but not store App ID.
- Click **Delete** to delete information: only for the stores that are Not in Use.

## Product

You can register products to sell at the store.
In the **Product** tab, you can register a new product or manage the registered products.
- (1) **Register** : You can use a single Store Item ID to register multiple products.
- (2) **Change store item status** : You can use a single Store Item ID to change whether to use the registered products all at once.
- (3) **Filter** : Provides filters for usage, store/store item ID/product ID, product name to allow easy search. When there is no search value, the list of products from all stores is displayed.

![gamebase_purchase_04_202006](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_04_202012.png)

### Register

To register a new product, click **Register** on the **Product List** page. 
#### 1. User-input Registration
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_05_202006.png)

* (1) **Product ID**: Enter product ID to request for purchase. Purchase API must be called from SDK with a product ID so as to purchase the corresponding product. 
* (2) **Product Name**: Enter name to show for purchase. Purchase history as well as indicators can be listed by the name.   
* (3) **Use**: Select whether to use a product. At the request of SDK for the list of products, those SDK that are selected as 'USE' only are listed. 
* (4) **More Products**: To register more products, click **+** to add input lines.   
* (5) **Store**: Select an external store to register. If there's none, first register a store at **Store**. 
* (6) **Store Item ID**: Enter ID issued after store is registered. Registered product list for Gamebase shall be paid at the request of payment to each selected store. 
* (7) **Store Item Type**: Select the type of product to register. For Google Play and App Store, items can be registered for subscription, and for other stores, only one-time item registratio is available.     

#### 2. File-uploading Registration 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_06_202006.png)

* Products can be registered by uploading files. 
* Up to 1,000 products can be registered at once. 
* Be cautious with the input type, since a product may not be properly registered with invalid input type. 
* Korean may not be saved properly if the file is not encoded in 'UTF-8'. 
* When it fails to register files, you may download failed list from **Download** at the result popup. 


### Modify

Query details or change information of a registered product from the list. 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_07_202006.png)

- Select each item from the list to query details. 
- Click **Modify** to change information, except Store, Item Number, and Product Type. 
- Only **Product Name, Use, and Store Item ID** are modifiable, and the others cannot be changed. 

## Transactions

Payment information can be queried. 
Select a search type in need and query payment information. 
To query the list of payments, click **Download** on top right of the page. 

### Properties

#### Search conditions
Each search type shows different search items.  

##### (1) General Search 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_10_202006.png)

Search is available by the following conditions: 
- **Search Period**: A period when user attempted to purchase, in the ascending or descending order 
- **Store**: Information of store to search 
- **Store Item ID**: ID issued after store is registered 
- **Item**: Select an item to search 
- **User ID**: ID of paying user 
- **Payment Status**: Status of payment to search 

##### (2) Search by Transaction ID 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_11_202006.png)

Search is available by Transaction ID which is created upon payment. 

##### (3) Search by Receipt 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_12_202006.png)

You can view the results using the receipt issued during the payment.


#### Search results
The search results are as follows:

- **Transaction ID**: Unique number for identifying payments within Gamebase
- **Store**: Information about the store where the payment has been made
- **User ID**: User ID that made the payment
- **Product name**: The name of the actual product that user purchased in app
- **Product ID (Store item ID)**: The actual ID of the product purchased by the user in app, and the ID of the store to which the actual payment has been made
- **Store item type**: Type of the actual product that was purchased by the user in app
- **Price/monetary unit**: The price and monetary unit of the item purchased by the user
- **Consume status**: Whether the paid item has been provided
- **Payment status**: The current progress of the payment
- **Store Reference Key**: Unique payment number issued by the store
- **Scheduled payment date**: The time of purchase attempt or completion
- **Refund date**: The time when the user's item was refunded
- **Additional information**: Additional information delivered by SDK when the payment was requested (Developer payload)

#### Change payment status
The status of the searched payment information is as follows:
- **Purchase successful (Success)**
    - Payment has successfully been made.
    - Can be changed to the Refund status.
- **Ready to make purchase (Reserved)**
	- No longer proceeding with payment through the store, or did not proceed with purchase validation.
	- Can be changed to Success or Refund status.
- **Failed to validate payment (Failure)**
	- Payment has been proceeded through the store, but an error occurred during payment validation.
	- Can be changed to Success or Refund status.
- **Refund successful (Refund)**
	- The administrator manually updated the refund status in the store.
	- Cannot be changed to any other payment status.
- **Canceling during the payment process (UserClose)** 
	- User has withdrawn the payment

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
