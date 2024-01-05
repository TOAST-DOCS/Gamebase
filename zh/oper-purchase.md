## Before Using Purchase Menu
![purchase_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_01_en_240103.png)
To use the purchase menu, currency must be selected for purchase metrics. 
It can be set only once initially, and the Analytics sales metrics show in the currency code as configured. 
Please be cautious with your choice, since the currency code cannot be modified, once selected. 

## Game > Gamebase > Console User Guide > Purchase

You can register in-app purchase information and view the history.
Gamebase uses NHN Cloud IAP (In-App Purchase) service.

## Store

Register stores to sell products in games.
Register a new store on the **Store Information List** of the **Store** tab, or manage registered stores.
![purchase_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_02_en_240103.png)

### Register

Click **Register** on the **Store Information List** to register a new store.

![purchase_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_03_en_240103.png)

* **Store**  Select an external store to register.  If it is not on the list, contact [Customer Center](https://toast.com/support/inquiry).
* **App Name**   Enter the name of a game to register.
* **Store App ID**   Enter information issued by store.
* **Use or Not**  Select whether to use the store or not.

> [Note] Validation of receipts from Google Play Store
>
> When Google's receipt validation system experiences failure, you can use the Gamebase's internal signature validation to properly process the purchase by setting the **Receipt validation settings for onetime products** to 1-step validation.
> 2-step validation is always performed for subscription products regardless of the setting.

### Modify

Retrieve or modify detail information of registered stores on the list.

![purchase_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_04_en_240103.png)

- Select a registered store on the list to retrieve detail information.
- Click **Modify** to modify information such as app name, store app, and use or not, but not store App ID.
- Click **Delete** to delete information: only for the stores that are Not in Use.

## Product

You can register products to sell at the store.
In the **Product** tab, you can register a new product or manage the registered products.

- (1) **Filter** : Provides filters for usage, store/store item ID/product ID, product name to allow easy search. When there is no search value, the list of products from all stores is displayed.
- (2) **Register** : You can use a single Store Item ID to register multiple products.
- (3) **Change store item status** : You can use a single Store Item ID to change whether to use the registered products all at once.

![purchase_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_05_en_240103.png)

### Register

To register a new product, click **Register** on the **Product List** page. 
#### 1. User-input Registration
![purchase_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_06_en_240103.png)

* (1) **Product ID**: Enter product ID to request for purchase. Purchase API must be called from SDK with a product ID so as to purchase the corresponding product. 
* (2) **Product Name**: Enter name to show for purchase. Purchase history as well as metrics can be listed by the name.   
* (3) **Use**: Select whether to use a product. At the request of SDK for the list of products, those SDK that are selected as 'USE' only are listed. 
* (4) **More Products**: To register more products, click **+** to add input lines.   
* (5) **Store**: Select an external store to register. If there's none, first register a store at **Store**. 
* (6) **Store Item ID**: Enter ID issued after store is registered. The item registered in the Gamebase product list is purchased using this information at the request of purchase to the selected store. 
* (7) **Store Item Type**: Select the type of product to register. For Google Play and App Store, items can be registered for subscription, and for other stores, only one-time item registration is available.     

#### 2. File-uploading Registration 
![purchase_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_07_en_240103.png)

* Products can be registered by uploading files. 
* Up to 1,000 products can be registered at once. 
* Be cautious with the input type, since a product may not be properly registered with invalid input type. 
* Korean may not be saved properly if the file is not encoded in 'UTF-8'. 
* When it fails to register files, you may download failed list from **Download** at the result popup. 


### Modify

Query details or change information of a registered product from the list. 
![purchase_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_08_en_240103.png)
- Select each item from the list to query details. 
- Click **Modify** to change information, except Store, Item Number, and Product Type. 
- Only **Product Name, Use, and Store Item ID** are modifiable, and the others cannot be changed. 
- **Store Item ID** is only modifiable with other already registered **Store Item ID**, and a new **Store Item ID** requires product registration.

## Transactions

Purchase information can be queried. 
Select a search type in need and query purchase information. 
To query the list of purchases, click **Download** on top right of the page. 

### Transaction Status Code
Transaction status code indicates what occurs while the user is making a payment.  

> [Note]
> Transaction process is divided into four stages.
> 
> [Reservation] - [Payment] - [Verification] - [Completion]
> 
> - Reservation: Prepares for user’s purchase and sends the required information to Gamebase
> - Payment: Processes purchases in the market
> - Verification: Verifies the results sent from the market
> - Completion: Finishes remaining processes required by each market after verification

- Reservation completed: [Reservation] is completed
- Verification failed: Verification is failed in [Verification]
- Verification completed: [Verification] is completed
- Purchase completed: All purchase processes are done
- Refund completed: Purchase is refunded by user or operator
- Canceled during purchase: Purchase is canceled by user during purchase

> [Note]
> - The **Reservation completed** status can occur in the following cases.
>   - When the user cancels purchase from the payment screen
>   - When the user exits the app while the purchase is in progress
>   - When the user doesn't receive a response from the market during the purchase process (temporary market error, user network disconnection, etc.)
>   - When the user attempts to purchase the same item for which the payment hasn't been processed by the marketplace
> 
> - If there is no change in the **Verification completed** status, please contact the Customer Center.

### View Transaction List
![purchase_09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_09_en_240103.png)

#### Category

You can view transaction list with two categories.

- **All**: View all transaction lists
- **Product Information Unregistered Payment**: View transactions where payment is made but product information is missing so the item cannot be delivered.


#### Search conditions
Each search type shows different search items.  

##### (1) General Search 
![purchase_10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_10_en_240103.png)

Search is available by the following conditions: 
- **Search Period**: A period when user attempted to purchase, in the ascending or descending order 
- **Store**: Information of store to search 
- **Store Item ID**: ID issued after store is registered 
- **Item**: Select an item to search 
- **User ID**: User ID that made the purchase 
- **Purchase Status**: Status of purchase to search 


##### (2) Search by Transaction ID 
![purchase_11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_11_en_240103.png)

Search is available by Transaction ID which is created upon purchase. 

##### (3) Search by Receipt 
![purchase_12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_12_en_240103.png)
You can view the results using the receipt issued during the purchase.


#### [All] Search Results
The search results are as follows:

![purchase_13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_13_en_240103.png)

- **Transaction ID**: Unique number for identifying purchases within Gamebase
- **Store**: Information about the store where the purchase has been made
- **User ID**: User ID that made the purchase
- **Product name**: The name of the actual product that user purchased in app
- **Product ID (Store item ID)**: The actual ID of the product purchased by the user in app, and the ID of the store to which the actual purchase has been made
- **Item type**: Type of the actual product that was purchased by the user in app
- **Price/currency unit**: The price and monetary unit of the item purchased by the user
- **Consume status**: Whether the purchased item has been provided
- **Purchase status**: The current status of the purchase
- **Store Reference Key**: Unique purchase number issued by the store
- **Scheduled purchase date**: Time when a user attempted purchase
- **Purchase date**: Time when a user completed purchase
- **Refund date**: Time when a user's item was refunded
- **Additional information**: Additional information delivered by SDK when the purchase was requested (Developer payload)

##### Change purchase status
The status of the searched purchase information is as follows:
- **Purchase successful (Success)**
    - Purchase has been made successfully.
    - Can be changed to the Refund status.
- **Reservation completed (Reserved)**
	- No longer proceeding with purchase through the store, or did not proceed with purchase validation.
	- Can be changed to Success or Refund status.
    - But, cannot be changed to Success status in Google Play Store.
- **Failed to validate purchase (Failure)**
	- The store proceeded with the purchase, but an error occurred during purchase validation.
	- Can be changed to Success or Refund status.
- **Refund successful (Refund)**
	- The administrator manually updated the refund status in the store.
	- Cannot be changed to any other purchase status.
- **Canceling during the purchase process (UserClose)** 
	- User has withdrawn the purchase


##### Changing to Success
![purchase_14](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_14_en_240103.png)
In order to proceed, you need to provide **Receipt Number, Value, and Currency**.

#### Changing to Refund
![purchase_15](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_15_en_240103.png)
No additional information is required. 
Once it has been changed to Refund status, it's set for good.

##### Validate Receipt
![purchase_16](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_16_en_240103.png)

* Validates transaction of the retrieved receipt.
* You can see the results of comparing each field. The response value is provided in JSON format, so you can directly check the data if needed.
* For now, only App Store transactions can be validated.



##### View Transaction List
You can view transaction lists by clicking the Transaction ID of the payment information you found.
![purchase_17](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_17_en_240103.png)

###### (1) View Additional Info and Receipt
For each payment status, you can click the right arrow to view additional information and receipt information.


#### Search Result of [Product Information Unregistered Payment]
Search result items are as follows.

![purchase_18](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_18_en_240103.png)

- **Transaction ID**: Unique number to identify transactions in Gamebase
- **Store**: Information of the store where payment is made
- **User ID**: User ID that made payment
- **Product name**: Product name users actually made in the app
- **Product ID(Store Item ID)**: The actual product ID that users purchased in the app and the store item ID that was actually paid for in the store.
- **Product type**: The actual product type users purchased in the app
- **Price/currency type**: Price and currency type of the item users purchased
- **Consume Status**: The purchased item is provided or not
- **Payment Status**: THe current status of payment
- **Store Reference Key**: Unique payment number issued by the store
- **Additional Info**: Additional information passed by the SDK in the payment request (Developer payload)
- **Register Product ID**: Product information manually registered

##### Register Product ID
![purchase_19](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_19_en_240103.png)
* You can manually add missing item information. 

## Monitor Purchase Abuse

You can view purchase abuse information and set automatic lockdown/release.

### Query Refund History

![purchase_20](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_20_en_240103.png)

Purchase and refund information can be queried by search conditions as below.
Click **Download** on top right to download purchase and refund list at any time.

#### Search Conditions
- **Refund Date**: Time when a user item was refunded
- **User ID**: User ID that made the purchase
- **Refund Count**: Refund count of the user. Only the items with count higher than input value are queried.
- **Refund Amount**: Refund amount of the user. Only the items with amount higher than input amount are queried.
- **Store**: The store where refund was made

#### Search Results
- **User ID**: User ID that made the purchase
- **Store**: Information of a store where purchase has been made
- **Refund Count**: Refund count of the user
- **Refund Amount**: Refund amount of the user
- **Purchase Count (Query Period)**: Purchase count of the user during query period
- **Purchase Amount (Query Period)**: Purchase amount of the user during query period
- **Purchase Count (Accumulated)**: Total accumulated purchase count of the user
- **Purchase Amount (Accumulated)**: Total purchase amount of the user
- **Purchase Count (Recent 3 months)**: Purchase count of the user during the last three months
- **Purchase Amount (Recent 3 months)**: Purchase amount of the user during the last three months
- **Status**:  Current status of the user
- **Change Status**: Ban or release depending on the user status

#### Change Status
![purchase_21](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_21_en240103.png)

The feature allows to change status of the game user's account.
Each status is available for change like below:

- **Normal**: Can be changed to ban or withdrawal status. Please note that, once withdrawn, user's account information cannot be reverted.  
- **Ban**: Ban can be released.
- **Ban Suspended**: Can be changed to ban status.
- **Withdrawal**: The button is not displayed, because the status cannot be changed.

#### Check Purchase History

You can query detailed purchase history during the search period by clicking a user ID on the list.

![purchase_22](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_22_en240103.png)

#### Purchase History
- **Scheduled purchase date**: Time when a user attempted purchase
- **Purchase date**: Time when a user completed purchase
- **Refund date**: Time when a user's item was refunded
- **Transaction ID**: Unique number to identify purchases within Gamebase
- **Store**: Information of a store where purchase has been made
- **Item Name**: Name of actual item which the user purchased from the app
- **Price**: Price of item purchased by the user
- **Currency**: Type of currency used by the user for purchase
- **Purchase Status**: Current status of purchase

### Query the History of Automatic Release for Purchase Abuse

![purchase_23](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_23_en240103.png)

You can search for user information on automatic release for purchase abuse by using the following search conditions.

#### Search Conditions
- **Search Period**: Query is performed based on the time when ban suspension started.
- **User ID**: User ID for ban suspension
- **Purchase Count**: The number of purchases made by the user during the ban suspension period. Only the items with count higher than input value are queried.
- **Purchase Amount**: The purchase amount of the user during the ban suspension period. Only the items with amount higher than input amount are queried.

#### Search Results
- **User ID**: User ID for ban suspension
- **Ban Suspension Period**: Ban suspension start and end time
- **Purchase Count (Release Condition)**: Total number of purchases required to release ban
- **Purchase Amount (Release Condition)**: The total amount to purchase to release ban
- **Release Conditions Satisfying**: Conditions (AND, OR) to satisfy the release condition
- **Purchase Count (Suspension Period)**: Total accumulated number of purchases made by the user during the ban suspension period
- **Purchase Amount (Suspension Period)**: Total amount purchased by the user during the ban suspension period

#### Check Purchase History

You can query detailed purchase history during the search period by clicking a user ID on the list.
(Note that users without purchase history shows up as inactivated.)

![purchase_24](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_24_en240103.png)

#### Purchase History
- **Purchase Date**: Time when a user completed purchase
- **Transaction ID**: Unique number to identify purchases within Gamebase
- **Store**: Information of a store where purchase has been made
- **User ID**: User ID that made the purchase
- **Store Item ID**: Actual store item ID that the user purchased in the app
- **Purchase Amount**: Amount of purchase

#### Set Automatic Lockdown for Purchase Abuse

Click **Enable** to enable the auto lockdown setting, and enter value.

![purchase_25](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_25_en240103.png)

#### Setting Information

![purchase_26](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_26_en240103.png)

* **Ban Period**  Enter the ban period to be applied for auto lockdown.
    * **Permanent Ban**: To be selected for a permanent ban.
    * **Temporary Ban**: Enter period for a temporary ban (days).  
* **Setting Conditions for Automatic Lockdown** Users that meet set conditions are automatically locked down. At least one setting is required.
    * **Refund Count**: Enter refund count due to abusive acts.
    * **Refund Amount**: Enter refund amount due to abusive acts.
* **Setting Messages for Automatic Lockdown**  
    * Enter a ban message to show to users.
    * Enter messages in multiple languages on a template to allow easy re-use. Select and register a pre-registered template.
* **Deleting Leaderboard**
    * Set whether to delete Leaderboard of the game user as well, along with auto-lockdown.
    * With registration, game user's data is to be deleted from the leaderboard, and note that <font color="red">such data cannot be recovered</font>.

#### Set Automatic Release for Purchase Abuse

To use the automatic release setting, click the **Use** button and enter the setting value.
To enable the automatic release setting, the automatic lockdown setting must be <font color="red">enabled</font>.

![purchase_27](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_27_en240103.png)

#### Setting Information

![purchase_28](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Purchase/en/purchase_28_en240103.png)

* **Temporary ban release period**: Enter the ban suspension period when automatic released is applied.
* **Set the conditions for releasing the ban**: Set the conditions required for automatic release. At least one field must be set.
     * **Purchase Count**: Enter the number of purchases for automatic release.
     * **Purchase Amount**: Enter the amount to purchase for automatic release.
* **Ban release message**
     * Enter a ban release message to be displayed to the user.
     * Templates with messages to be displayed to users entered in multiple languages are provided so that users can easily reuse the message. Select a pre-registered template and register the template.