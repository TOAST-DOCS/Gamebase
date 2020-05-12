## Game > Gamebase > Console User Guide > Coupons

Coupons to be deployed to game users can be created in bulk and managed for game operations.  

## Publish Coupons
You can publish or query coupons for the app.
Currently, only serial coupons are available, but keyword coupons are to be provided in the near future.

### Search
Query history of published coupons according to search conditions.

![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_01_201911.png)

**Search Conditions**

- **Status**: (Required) Current status of the coupon: choose from All/Activate/Deactivate.
- **Valid Period**: (Required) List available coupons during a selected period.
- **Coupon Name**: Search by registered coupon name.

**Search Results**

- **Coupon Name**: Name of a published coupon
- **Supplied Item**: Name of registered item when published: for a multiple number of items, show one representative and n counts.
- **Store**: Stores where coupons are available. Only general coupons are supported now, and coupon registration is to be provided at each store.
- **Use/Publish**: Total amount of coupons used/published
- **Valid Period**: Valid period of a coupon
- **Published Date**: Date and time information of a published coupon
- **Coupon Usage History**: Go to page to query usage history of a published coupon
- **Download **: Download the list of detail coupon codes of published coupons
- **Supplied Item**: Show if a published coupon can be registered or not

### Publish
To publish, click Register on the query page of Publish Coupons.

![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_02_201911.png)

#### (1) Coupon Type
Set a coupon type to publish.
Each type is described as below.
Currently, only serial coupons are provided, and more types are to be provided.

- **Serial Coupons** : Publish coupon codes by creating a random coupon number.

#### (2) Coupon Name
Enter name of a coupon along with the purpose.

#### (3) Store
Select a store to use published coupons.
Currently, you can get **General** coupons only, and more times are to be added to get coupons from each store.

#### (4) Valid Period
Set active period of using published coupons.  

#### (5) Published Count
Set the number of coupon codes to be created.
UP to 50 thousand coupon codes can be created with a single request.

#### (6) Available Count per User
Set the maximum number of published coupons a user can use.
The maximum number is 100, and with 0, unlimited usage is available.

#### (7) Item
Enter information of items to be provided for coupon code registration.
Select an item to provide and enter count of it on the right.
To register an item, it must be registered first on the coupon item menu.


> [Note]
>
> Regarding coupon item registration, see [Coupon Item](./oper-coupon/#Coupon_Item).


### Update Coupons

To update information of already published coupon, press Update on Detail information.
It is impossible to edit already published coupon code type; to get a new type of coupon, register new coupon information.

![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_03_201911.png)

#### (1) Coupon Name
Enter name of a coupon along with the purpose.

#### (2) Store
Select a store to use published coupons.
Currently, you can get **General** coupons only, and more times are to be added to get coupons from each store.  

#### (3) Valid Period
Set the period to use published coupons.

#### (4) Available Count per User
Set the maximum number of coupons a user can use.
The maximum count is 100; if it is '0', coupons are available indefinitely.

#### (5) Item
Enter information of item to be provided with coupon code registration.
Select an item to be provided and enter the supply count on the right.
For item registration, register an item from the menu first.


> [Note]
>
> Regarding coupon item registration, see [Coupon Item](./oper-coupon/#Coupon Item).

## Query Coupon History
Usage history of a published coupon can be queried.
Search screen comes as below depending on the search conditions.

### By Coupon Code
Enter coupon code to query the usage.
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_04_201911.png)

### By User ID
Query history of coupon usage by each user ID.
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_05_201911.png)

### By Coupon Name
Query history of coupon usage by coupon name and other search conditions.
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_06_201911.png)
(1) **Coupon Name**: Select a coupon published from Publish Coupons.
(2) **Availability**: Query is available by selecting availability. Currently, only used coupons can be queried, and further search conditions are to be added for other statuses.  
(3) **Coupon Usage Date**: It is available to query coupons that are used during a set period only.

## Coupon Items
Query/Manage coupon items to be provided to use coupon codes.  

### List
List history of registered coupon items.
Search by Item ID/Item Name is available through filtering.
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_07_201911.png)

### Register
Register items to be provided to use coupon codes.
Items can be registered by case or file.

#### By Case
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_08_201911.png)

##### (1) Item ID
Enter original item ID registered within app. Since data input on this page are to be delivered as result data for actual server, an original item ID is required so as to be identified within the server.
Note that item ID cannot be changed after initial registration.

##### (2) Item Name 
Enter item name to identify a registered item.

#### By File
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_09_201911.png)
You may use files to register mass data all at once: up to 10,000 cases can be registered with a file. To enable file registration, first download a template file and fill in information for each format, before uploading it.  

### Update
Information of registered item can be updated, and you may disable the item so as it does not show when a coupon is published.

> [Note]
>
> Item ID cannot be modified: instead of correction, a new item ID must be registered.

![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_coupon_10_201911.png)

#### (1) Item Name
Enter item name to be identified.

#### (2) Availability
Items can be added for coupon registration, only when it is **Enabled**. In order not to show items on the coupon registration page, change the status to **Disabled**. Items are always **Enabled** when they are registered initially.
