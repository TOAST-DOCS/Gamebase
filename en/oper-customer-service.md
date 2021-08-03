## Game > Gamebase > Console User Guide > Customer Center

You can process user inquiries received during game operation and manage settings for notices, FAQ, etc. provided through the Customer Center page.
When processing the user inquiries, you can also register the settings for the email to be sent to the user and frequently used answers as a template for later use.
> [Note]
> To use this menu, go to App -  Customer Center settings, and select Customer Center Provided by Gamebase.
>

## Help Center Web Page 

Describes the customer center web page exposed to the user.
Through this screen, the user registers 1:1 inquiry and views the history of inquiries. The user can also check FAQ and Notice pages.

### Main 

When you open the customer center website using the Gamebase SDK in the game, the following screen is exposed to the user.
![main](http://static.toastoven.net/prod_gamebase/gamebase_help_center_00_20201125.png)

#### (1) 1:1 Inquiry

When the user clicks the **1:1 inquiry** button, they are redirected to the screen where 1:1 inquiries can be registered.

![Inquiry](http://static.toastoven.net/prod_gamebase/gamebase_help_center_01_20201125.png)

The following are the items to be entered when registering inquiries.
Registered inquiries can be viewed and answered in the **[Customer Center > Customer Inquiry](./oper-customer-service/#inquiry)** console.

1. Inquiry type: Select the inquiry type for submission. A submission inquiry type can be registered, edited, and deleted in the [Customer Center > Customer Inquiry](./oper-customer-service/#inquiry). 
2. Reply email: Enter the email address to receive an answer to user inquiry. If the inquiry answer is completely processed in the console, the answer email is automatically sent to the entered email address.
3. Name (nickname): Users can enter a nickname to be used in the game. The maximum character length is 10. 
When you open the customer center page after setting the game nickname as additional info, the nickname is automatically entered even if the user did not enter it.
4. Subject: Enter the inquiry subject. The maximum length is 100 characters
5. Inquiry: Write the contents of the inquiry. The maximum character length is 1,000 characters.
6. Attachment: Register the attachment if you have any. Up to 5 files (under 10 MB) can be attached.

#### (2) My Inquiries

Logging in and accessing the customer center web page is required to activate **My Inquiries** button. Click the button to go to the screen where users can view the history of their previous inquiries.
![MyInquiries_login](http://static.toastoven.net/prod_gamebase/gamebase_help_center_02_20201125.png)

In My Inquiries, you can see 10 listings by default. If there are more than 10, you can click **View more** to expose 10 additional listings.

> [Note] Login is required to be able to view the details in My Inquiries.
> If the user posts the inquiry without logging in, they can check the inquiries only through emails and cannot see them in My Inquiries.
> ![MyInquiries_no-login](http://static.toastoven.net/prod_gamebase/gamebase_help_center_03_20201125.png)

#### (3) Frequently Asked Questions

In FAQ, the user can see categorized questions and frequently asked questions. In the list, up to 12 items are exposed.
The user can search for topics or click the Category button to see the FAQ registered by the [Customer Center > FAQ](./oper-customer-service/#faq).
![FAQ](http://static.toastoven.net/prod_gamebase/gamebase_help_center_04_20201125.png)

1) You can enter the keyword you want to check to see the FAQs containing that keyword.
2) You can see the questions registered as FAQ.
3) You can see FAQs by grouping them using **Manage FAQ Type** that was set when registering the FAQs.
4) FAQ categories can be added or deleted through [Gamebase Console > Customer Center >  Manage FAQ Type](./oper-customer-service/#search-faq).

#### (4) Notices
Registered posts can be viewed in the **Customer Center > Notices**.

On the main screen, the three most recent posts are displayed, and the posts pinned at the top are displayed as boldfaced. You can click **more** to see all registered notices.
Created date is sorted in descending order to expose the notice posts, and the notices pinned at the top are shown in the boldface format. Expired posts are no longer shown in the list. You can click the post to see the details.
![Notices](http://static.toastoven.net/prod_gamebase/gamebase_help_center_05_20201126.png)

## Inquiry
Inquiries sent by customers can be viewed or processed.
You can also set the submission types necessary to register a user inquiry, and set the push notifications sent to the user when the inquiry is completely processed.

### Search Inquiry

Searches for the customer inquiry that matches the search conditions.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_01_202107.png)

**Search conditions**

- **Status**: (required) Selects the current status of the customer inquiry.
- **Submission type**: (required) Selects the submission type selected by the user upon user inquiry. There are Submit/Completed items.
- **Submission period**: (required) Shows the inquiries received during the selected period.
- **User ID**: Enter the user ID if you want to search for an inquiry made by a particular user.
- **Inquiry Title**: Enter the inquiry title to search for an inquiry with a specific title.

**Search results**

- **Submission type**: Submission type selected by the user when registering the inquiry
- **Inquiry subject**: Inquiry subject entered by the user when registering the inquiry
- **Submit date**: Date and time user registered the inquiry
- **Processed date**: Date and time when the customer representative processed the received inquiry
- **Status**: Current status of the received inquiry
    - Received: The user has registered an inquiry. Status changed to Received even if leaving an additional inquiry to the previous inquiry
    - Pending: The person in charge has left a reply as Pending. Used when additional confirmation is required.
    - Resolved: The person in charge has left a reply as Resolved. The inquiry has been resolved.
    - Completed: The status becomes Completed automatically when completed or two weeks have passed after being resolved by the person in charge.

#### 1. Manage submission type
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_02_202106.png)

You can manage the submission type which can be selected by users when registering inquiries.
These can be registered in any of the supported languages, and the maximum length is 20 characters for each type.
The list is shown to the user in the order of appearance, and this order can be changed within the list by dragging & dropping.
> [Note]
> The currently selected supported language can be checked in App - Customer Center settings.

#### 2. Send reply settings
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_05_202106.png)

You can enable this function when you want to send the Push message to the user when the inquiry has been processed.
If you decide to use it, check Send at the top to also send the completion push notification to the user when the inquiry has been processed.
As for the global service, you can additionally register the language you want and sent the mail in that language. Then, the push notification is sent to the user in the language appropriate to the device language setting of the user.
> [Note]
> 1. To use this function, NHN Cloud Push product must be enabled first.
> 2. As for selection of the Send Reply settings, all languages supported by Gamebase can be registered regardless of the languages supported by the customer center.

### Inquiry details

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/Operators_Guide/gamebase_customer_inquiry_03_202107.png)

You can check details and process inquiries regarding the inquiries sent by users.
After processing the inquiry, the user can make additional inquiries.
After processing the inquiry, click the **Complete** button to change the inquiry status to Processed. User can no longer leave an inquiry when they have been Processed.

If you decided to use a template, you can use a template content configured in the Customer Center - Reply Template menu right for the reply. You can also use the Text Editor in addition to the template to customize your answer to the user.
If you need to attach files when answering the user inquiry, you can attach up to 5 files (under 10 MB).
And when the inquiry has been processed, the answer written by the customer representative is sent to the user's email address which was entered by the user to submit the inquiry.
At this point, you can check if the push notification is being sent to the user when the inquiry has been processed by checking the items with the reply sent.
> [Note]
> ![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/Operators_Guide/gamebase_customer_inquiry_06_202107.png)
> If a logged-in user registered the inquiry, the information about the user is displayed in a single view.
> You can close the window by clicking the X button on the right. The window will reopen when you click the user ID.
> The user information is viewed as similar to the functions from the previous member menu, you can easily check the necessary information when having to respond to the user inquiry.

#### 1. Send Reply settings
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_05_202106.png)

You can enable this function when you want to send the Push message to the user when the inquiry has been processed.
If you decide to use it, check Send at the top to also send the completion push notification to the user when the inquiry has been processed.
As for the global service, you can additionally register the language you want and sent the mail in that language. Then, the push notification is sent to the user in the language appropriate to the device language setting of the user.
> [Note1]
> 1. To use this function, NHN Cloud Push product must be enabled first.
> 2. As for selection of the Send Reply settings, all languages supported by Gamebase can be registered regardless of the languages supported by the customer center.

> [Note2]
> When viewing a closed user inquiry, you can view the inquiry and the answer, and if there is a file attached to the inquiry while processing it, you can click the item to download the attachment.


## FAQ

You can manage the FAQ provided by the customer center page.

### Search FAQ

You can search the registered FAQs.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_07_202106.png)

**Search conditions**

- **Status**: (required) Select the exposure type of FAQ. You can select either Exposed or Not exposed.
- **Type**: (required) You can select and search an FAQ type. The selected ones are displayed based on what was registered in Manage FAQ Type.
- **Question+Answer**: This function is used when trying to search for the FAQ containing a certain keyword in the question or answer. When you want to register the inquiry registered in another language, perform search after specifying the language to search with.

**Search results**

- **Frequently asked questions**: Shows whether FAQ is included in the Frequently Asked Questions section.
- **Type**: Displays the categorical type of the registered FAQ.
- **Subject**: The question part of the FAQ.
- **Modified by**: Shows the information of the last user who registered or modified the FAQ.
- **Modified date**: Shows the last date when the FAQ was registered or modified.
- **Status**: Shows whether FAQ is currently being displayed. It is either Exposed or Not exposed.

#### Manage FAQ type
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_09_202106.png)

You can manage the type available for selection when registering or modifying the FAQ.
These can be registered in any of the supported languages, and the maximum length is 20 characters for each type.
The list is shown in the order of appearance, and this order can be changed within the list by dragging & dropping.
> [Note]
> The currently selected supported language can be checked in App - Customer Center settings.

### Register or Update FAQ
You can register an FAQ or modify the info of an FAQ that is already registered.
What can be changed during registration or modification is the same.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_08_202106.png)

#### 1. Status
Selects the exposure status of the FAQ you want to register or modify.
There are Exposed / Not exposed, and you just have to select whether it will be exposed to actual users on the Customer Center page.

#### 2. Type
Select the FAQ type that you want to register or modify based on the type registered under Manage FAQ Type.

#### 3. Frequently asked questions
Check whether you will display the question in the Frequently Asked Questions section of the customer center page.

#### 4. Question
Enter the text of the FAQ.
> [Note]
Supported languages that were configured in the > App - Customer Center can be registered only when all of them were entered.

####. 5. Answer
Enter the answer to the FAQ.
You can customize the answer by using the Text Editor, and the answer is exposed in the web page as is.
> [Note]
Supported languages that were configured in the > App - Customer Center can be registered only when all of them were entered.

## Notice

You can manage the notices to be provided by the customer center page.

### Search Notice

You can search the registered notices list.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_10_202106.png)

**Search conditions**

- **Search type**: (required) You can select the search type for notices. Status is selected by default. But if you want to search by date of exposure, perform the same search procedure after selecting it for the search condition.
- **Status**: (required) Search by the exposure status of the current notices using the items selected by default from the above search type. You can select one from To Be Exposed / Currently Exposed / Finished.
- **Date of exposure**: Can be enabled if the exposure date has been selected from the search type. And you can search and view the notices list which is shown based on the selected exposure date.
- **Subject+Content**: This function is used when trying to search for the notice containing a certain keyword in the subject or body text. When you want to register the notice registered in another language, perform search after specifying the language to search with.

**Search results**
- **Pinned at the top**: Shows whether the notice is included in the pinned field at the top.
- **Type**: Displays the categorical type of the registered notice.
- **Subject**: Subject of the notice.
- **Exposure date (UTC+9)**: Shows the registration date (display date) to carry out the actual exposure when the notice is being exposed.
- **Exposure period**: Shows the exposure period of the notice.
- **Status**: Shows the current progress of the notice. You can select one from To Be Exposed / Currently Exposed / Finished.

#### Manage Header
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_11_202106.png)

You can manage the header which can be selected when registering or modifying the notice.
These can be registered in any of the supported languages, and the maximum length is 20 characters for each type.
The list is shown in the order of appearance, and this order can be changed within the list by dragging & dropping.
> [Note]
> The currently selected supported language can be checked in App - Customer Center settings.

### Register or Update Notice
You can register a new notice or modify the notice information which has already been registered.
What can be changed during registration or modification is the same.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_15_202106.png)

#### 1. Date of exposure
Set the period to expose the notice.

#### 2. Display time
Select the date to be shown to the user when posting the notice.

#### 3. Header
Select the header of the notice.

#### 4. Pin to top
Pin the notice at the top, so that it will always be exposed.

#### 5. Subject
Enter the subject of the notice.

#### 6. Body
Enter the text for the body of the notice.
You can customize the answer by using the Text Editor, and the answer is exposed in the web page as is.
> [Note]
Supported languages that were configured in the > App - Customer Center can be registered only when all of them were entered.

#### 7. Attach file
You can attach and upload a file to be shown with the notice.
Up to 5 files (under 10 MB) can be attached.
The attachments are exposed with the notice, and can be downloaded by clicking on them.

## Answer template

To reduce repetitive typing of the same text while processing customer inquiries, this function provides a template to process the inquiry.

### Search Template
It shows the list of the currently registered templates, and you can enter the search term in the upper-right corner to search for the currently registered template.
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_12_202106.png)

**Results**
- **Template name**: Name of the template which is exposed in the template list for selection when processing user inquiries.
- **Modified by**: Shows the information of the last user who registered or modified the reply template.
- **Modified date**: Shows the date when the reply template was most recently registered or modified.

### Register or Update Template
You can register a new reply template or modify the information of the previously registered reply template.
What can be changed during registration or modification is the same.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_13_202106.png)

#### 1. Template name
Enter the name of the template which will be exposed in the template list when processing user inquiries.

#### 2. Body
Enter the body text to fill the template upon the selection of the template while processing the inquiry.
You can freely use the text editor to fill out the template, and this text will be applied as is when selecting the template while processing the inquiry.

## Email Config
You can set the format of the email which will be sent out to the user after the inquiry is processed.
A default template is provided when initially activated, and you can edit it as freely as you want by using the text editor.

Test sending function is provided, which can be utilized for using the currently entered template to preview how it is being sent to the actual user.

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_14_202106.png)

> [Note]
> If the email in the sender address does not have any SPF record setup, the email can be considered spam. 
> Thus, the following value must be registered in the TXT record of the DNS before setting it to the sender's address.
> Additional value : v=spf1 include:_spfblocka.toast.com ~all
