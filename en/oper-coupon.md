<!-- pre-align:aligned sig=4800987c46f8 -->

<a id="game-gamebase-console-user-guide-coupons"></a>
## Game > Gamebase > Console User Guide > Coupons { #game-gamebase-console-user-guide-coupons }

Coupons to be deployed to game users can be created in bulk and managed for game operations.  

<a id="publish-coupons"></a>
## Publish Coupons { #publish-coupons }

You can issue or search for coupons which can be used within the app.

<a id="search-coupon-publish"></a>
### Search Coupon publish { #search-coupon-publish }

Searches for coupon issuance history that matches the search conditions.

![gamebase_coupon_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_01_240813.png)

**Search conditions**

- **Status**: (required) You can select the current progress for the coupon (all, activated, inactivated).
- **Expiration date:**: (required) Searches for the list of coupons available for use during the selected period.
- **Coupon name**: Searches by registered coupon name.

**Search results**

- **Coupon name**: Name of the issued coupon
- **Coupon type**: Type of the issued coupon
- **Issued item**: Name of the item registered upon issuance; if there are multiple items, they are displayed as 'Main item and N other(s)'
- **Store**: Information of the store where you can use the coupon
- **Use/Issue**: A total number of the issued coupons that were used/a total number of issuance
- **Expiration date:**: Expiration date of the coupon
- **Date and time of issuance**: Information about when the coupon was issued
- **Coupon use history**: Switches to the screen where you can search for the use history of the issued coupon
- **Download**: Downloads the detailed codes list for the issued coupon
- **Provided item**: Shows whether the issued coupon can be registered

<a id="publish-coupon"></a>
### Publish coupon { #publish-coupon }

You can click the **Register** button on the coupon Issuance search screen to proceed with the coupon issuance

![gamebase_coupon_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_02_240813.png)

<a id="publish-coupon-coupon-type"></a>
#### 1. Coupon Type

Set a coupon type to publish.
Each type is described as below.

- **Serial Coupons** : Publish coupon codes by creating a random coupon number.
- **Keyword**: Issues the name of a specified keyword as a coupon code that can be used by users.

<a id="publish-coupon-coupon-name"></a>
#### 2. Coupon Name

Enter name of a coupon along with the purpose.

<a id="publish-coupon-coupon-code"></a>
#### 3. Coupon Code

Enter a unique coupon code that users can redeem when **keyword coupons are issued*.

<a id="publish-coupon-store"></a>
#### 4. Store

Select a store to use published coupons.

Currently, you can get **General** coupons only, and more times are to be added to get coupons from each store.

<a id="publish-coupon-valid-period"></a>
#### 5. Valid Period

Sets the period during which you can use the issued coupon

<a id="publish-coupon-number-of-issuance"></a>
#### 6. Number of issuance

Sets the number of coupon codes to generate during issuance.
Up to one million coupon codes can be created per request.

<a id="publish-coupon-number-of-coupons-allowed-per-user"></a>
#### 7. Number of coupons allowed per user

Sets the max number of coupons allowed per user.
Can be set to max value of 99. To make it unlimited, set it to  0.

<a id="publish-coupon-item"></a>
#### 8. Item

Enter the item information to provide when the coupon code is redeemed.
Select the item to issue, and enter the quantity on the right.
You have to register the item in the Coupon Item menu first to be able to select it.


> [Note]
>
> Regarding coupon item registration, see [Coupon Item](#Coupon_Item).


<a id="update-coupons"></a>
### Update Coupons { #update-coupons }

To change the information about the issued coupon, click the **Modify** button in the Details.
Since a coupon code type already issued cannot be edited, you must register new issuance information if you want to issue a new coupon type.

![gamebase_coupon_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_03_240813.png)

<a id="update-coupons-coupon-name"></a>
#### 1. Coupon Name

Enter name of a coupon along with the purpose.

<a id="update-coupons-store"></a>
#### 2. Store

Select a store to use published coupons.
Currently, you can get **General** coupons only, and more times are to be added to get coupons from each store.  

<a id="update-coupons-valid-period"></a>
#### 3. Valid Period

Set the period to use published coupons.

<a id="update-coupons-available-count-per-user"></a>
#### 4. Available Count per User

Set the maximum number of coupons a user can use.
The maximum count is 100; if it is '0', coupons are available indefinitely.

<a id="update-coupons-item"></a>
#### 5. Item

Enter information of item to be provided with coupon code registration.
Select an item to be provided and enter the supply count on the right.
For item registration, register an item from the menu first.

<a id="update-coupons-send-coupon"></a>
#### 6. Send Coupon
Provides a feature that directly sends coupons to users using issued coupon information.

![gamebase_coupon_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_04_240813.png)
##### (1) Send type: Select the send type. Supports MMS/SMS. For MMS, you also need to enter the title to send it.

##### (2) Select template: If you registered a template from the TOAST Cloud SMS service, you can select and send that template. If you do not have any template registered, you do not have to select any.

##### (3) Advertisement: Select whether the SMS will be tagged as an advertisement. Remember that an advertisement must include an '(Ad) [Opt Out for Free]' string to be sent as an SMS.

##### (4) Send number: Select the send number registered for TOAST Cloud SMS. This number is presented as a sender number to the recipient.

##### (5) Send type: Can select between Instant and Scheduled. If it is Instant, SMS is sent immediately after the button is clicked. If it is Scheduled, SMS is sent at the time you specified.

##### (6) Title: Specify the title of the MMS when sending one. Title can be entered only for sending an MMS.

##### (7) Body: Enter the full body text to send. The ##code## character string is the one replaced with a coupon. If this character string is not included, the coupon won't be sent.

##### (8) Call block number: Select the call block number registered for TOAST Cloud SMS. If you want to send an advertisement, you need to specify that it is an advertisement. Since options on the Send screen are references visible to users, they need to be carefully written.

##### (9) Receiver number: Register recipients using the target of the SMS. It can be either **number** or **number/coupon**. If only numbers are entered, they are sent in the order of their number.

##### (10) Test receive number: You can test sending an SMS based on the entered form. Enter the recipient's number and click the Send button to send the test SMS to that number. We recommend you to test send an SMS in advance to see if everything looks all right.

> [Note1]
>
> Refer to [Coupon Item](#coupon-items) to see how to register coupon items.
>
> [Note2]
>
> SMS products must be enabled to be able to issue coupons.

<a id="update-coupons-issue-additional-coupons"></a>
#### 7. Issue additional coupons
If the coupon type is serial, you can receive up to 100 (100,000 at a time) additional coupons (including the initially issued quantity).

![gamebase_coupon_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_05_240813.png)

<a id="update-coupons-coupon-statistics"></a>
#### 8. Coupon statistics
SMS send history can be viewed at the bottom of the coupon issue details screen. The statistics related to issuing coupons can be viewed and the file can be downloaded.
![gamebase_coupon_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_06_240813.png)

The send statistics of SMS that actually sent to users can be checked via requested statistics. You can also download the detailed results by clicking the Request Download button on the right.
The status after requesting download is as follows:
- **Preparing to create**: The download request has been completed.
- **Creating**: The requested download file is being created.
- **Download**: When the file is ready to be downloaded, Download button is activated. Click the button to download the send details.

<a id="query-coupon-history"></a>
## Query Coupon History { #query-coupon-history }

Usage history of a published coupon can be queried.
Search screen comes as below depending on the search conditions.

<a id="properties"></a>
### Properties { #properties }

<a id="properties-query-by-coupon-code"></a>
#### Query By Coupon Code

Enter coupon code to query the usage.
![gamebase_coupon_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_07_240813.png)

<a id="properties-query-by-user-id"></a>
#### Query By User ID

Query history of coupon usage by each user ID.
![gamebase_coupon_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_08_240813.png)

<a id="properties-query-by-coupon-name"></a>
#### Query By Coupon Name

Query history of coupon usage by coupon name and other search conditions.
![gamebase_coupon_09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_09_240813.png)

1. **Coupon Name**: Select a coupon published from Publish Coupons.
2. **Availability**: Query is available by selecting availability. Currently, only used coupons can be queried, and further search conditions are to be added for other statuses.  
3. **Coupon Usage Date**: It is available to query coupons that are used during a set period only.

<a id="coupon-items"></a>
## Coupon Items { #coupon-items }

Query/Manage coupon items to be provided to use coupon codes.  

<a id="search-coupon-item"></a>
### Search Coupon item { #search-coupon-item }

List history of registered coupon items.
Search by Item ID/Item Name is available through filtering.
![gamebase_coupon_10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_10_240813.png)

<a id="register-coupon-item"></a>
### Register Coupon item { #register-coupon-item }

Register items to be provided to use coupon codes.
Items can be registered by case or file.

<a id="register-coupon-item-register-by-case"></a>
#### Register By Case

![gamebase_coupon_11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_11_240813.png)

##### 1. Item ID

Enter original item ID registered within app. Since data input on this page are to be delivered as result data for actual server, an original item ID is required so as to be identified within the server.
Note that item ID cannot be changed after initial registration.

##### 2. Item Name

Enter item name to identify a registered item.

<a id="register-coupon-item-register-by-file-upload"></a>
#### Register By File Upload

![gamebase_coupon_12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_12_240813.png)
You may use files to register mass data all at once.
Up to 10,000 cases can be registered with a file. To enable file registration, first download a template file and fill in information for each format, before uploading it.  

<a id="update-coupon-item"></a>
### Update Coupon item { #update-coupon-item }

Information of registered item can be updated, and you may disable the item so as it does not show when a coupon is published.

> [Note]
>
> Item ID cannot be modified: instead of correction, a new item ID must be registered.

![gamebase_coupon_13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Coupon/en/gamebase_coupon_13_240813.png)

<a id="update-coupon-item-item-name"></a>
#### 1. Item Name

Enter item name to be identified.

<a id="update-coupon-item-availability"></a>
#### 2. Availability

Items can be added for coupon registration, only when it is **Enabled**. In order not to show items on the coupon registration page, change the status to **Disabled**. Items are always **Enabled** when they are registered initially.
