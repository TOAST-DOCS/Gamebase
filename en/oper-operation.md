## Game > Gamebase > Console Guide > Operation

This menu provides functions that are required for an app operation.

* (Maintenance): Manage app maintenance
* (Notice): Manage urgent notices by showing a popup window to game users
* (Image notice): Manage image notices which is provided as an image to game users
* (Kick out): Disconnect the app user

## Maintenance

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_01_en_240625.png)

Can easily register maintenance in the Console, when required.
Retrieve maintenance history of registered apps and check progress at a glance, and search maintenance by registered causes of maintenance.
Status of maintenance is classified into five as below.

(1) In Reservation: Maintenance has been reserved
(2) In Progress: Maintenance is underway
(3) Completed: Maintenance time is over.
(4) Off: Operator has called for **Off** while maintenance is underway
(5) Off (Expired): When maintenance time is over during **Off**

Gamebase provides maintenance pop-ups and detail pages to show to game users while maintenance is underway.
Default maintenance pop-up of Gamebase.
![gamebase_op_img](https://static.toastoven.net/prod_gamebase/gamebase_op_02_201812.png)
Default maintenance page of Gamebase (with cause and time of maintenance)
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_02_en_240105.jpg)

### Register Maintenance

Click **Register** under the **Maintenance** tab, to register maintenance.

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_03_en_240625.png)

>  <font color="red">[Caution] </font><br/>
>  
> If **Update Required and Maintenance are both enabled**, the service status becomes 'Update Required'.
>  If you do not want to show a popup about required updates to users during the maintenance, the service status must be changed to 'Update Required' after the maintenance.

#### (1) Target
Select the maintenance target.

- Entire game : Select it when maintenance is required for all client versions.
- Some clients : Select it when maintenance is required only for certain client versions. Click the 'Select version' button to display the list of the client versions registered from the client menu.
  **[Example of selecting Some clients]**
  Select All is possible by client status and by store. Just select the client version to perform maintenance, and click the Confirm button.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_04_en_240625.png)

#### (2) Reason
Enter the reason why the maintenance is needed.
This information is not exposed to game users. You just need to enter a brief explanation as to why you registered the maintenance.

#### (3) Time
Set when to conduct the maintenance.
As for timezone, 'UTC+09:00' is selected by default, and you can also register maintenance by selecting the timezone of the country you are providing service to.

#### (4) Maintenance page
Set the type of the maintenance page which will be shown to users.
You can choose **Page provided by Gamebase (WebView)**, **Custom HTML (WebView)**, or **External page**, and the input window is different for each item.
The following are the extra input fields for each item. You can click **Preview** to see what was entered.

##### 4-1) Page provided by Gamebase(WebView)
A default type maintenance page which displays the information that was entered by the operator in the WebView page provided by Gamebase.
It can be useful when there is no separate maintenance page.
In the **Exposed Message** field, enter the message to be shown to the users during maintenance.
The message can also be entered in other languages such as Korean, Japanese, and Chinese, and the chosen language becomes the 'default language'.
For users who do not have the matching language among the registered messages, the one selected as the 'Default Language' is displayed. You can click the **+** button on the right to add a language. If the language you are looking for is not available, please contact our [Customer Center](https://toast.com/support/inquiry) to request a new language.
Click **Preview** to see the Preview screen in the 'Default Language'.

##### 4-2) HTML provided by users (WebView)
The operator directly fills out the maintenance page in HTML format and provides it to users.
The preview page is also supported based on the entered HTML tags.
This is useful when creating the maintenance page format you want.

##### 4-3) External page

![image alt](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_05_en_240625.png)
If you own a maintenance page or template, the maintenance page can be linked to URL.
Preview of the URL to connect is also supported.
To be provided with maintenance information, choose **Provide Information** and enter messages in **Message**: maintenance information registered in Gamebase (maintenance time, messages, and etc.) can be provided on the maintenance page.
Maintenance parameters are as follows: all delivered with URL encoded.

- message: Maintenance messages in the language as set in device information. For preview, deliver default messages.
- timezone: Standard time zone selected for maintenance registration. e.g) Delivery value for UTC+0: +09:00
- beginDate:  Start time entered for maintenance registration
- endDate:  End time entered for maintenance registration.

#### (5) Popup Messages 
Set a message to show for maintenance. 
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

### Modify Maintenance

Can check, modify, and delete details of registered maintenance.
Input items are same as Registration page, and Delete button is also available.
To register maintenance again with similar content, you may copy and paste for an easy registration.

## Notice

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_06_en_240625.png)

Provides pop-up notifications during app execution. The pop-ups will show before logins; in case of errors in external authentication or game server, pop-ups need to be registered.
Can easily check the list of registered notifications with status and search messages.
Status of notice is classified into three as below.

(1) Expected: Notice is expected to show
(2) In Progress: Notice is now showing
(3) Completed: Notice time has been completed

### Register Notice

Clicking the 'Register' button on the main screen of Notice redirects you to the screen where you can register a notice.

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_07_en_240625.png)

#### (1) Target

Select a target to show notification.
- All Games: When maintenance is required for all client version.
- Some Clients: When only a particular client version requires maintenance. Click 'Select a Version' to show the list of client versions registered in the client menu.
  **Example of selecting particular clients**
  Can select a client status and all for each store, and select a client version for maintenance and press OK.
  ![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_04_en_240625.png)


#### (2) Target Country
Select a country to show notification.

- All Countries : Show to all users
- Some Countries: Show to users of a selected country only.
  Enter a country code to add and it will be automatically completed and entered. If a country code you want des not exist, contact [Customer Center](https://toast.com/support/inquiry).

> [Note]
> Criteria of Country Selection
> Notice shows on the basis of user's **USIM Country Code** ; when USIM is not available, it shows based on countries set on **Device**.

#### (3) Number of Impressions
Select a number of times a notice shows to users.

- One impression during the impression period: Shows one time during impression period
- Always show when you start an app: Shows every time user starts an app during impression period

#### (4) Time
Set time to show notices.
For a timezone, 'UTC+09:00' is provided as default, and maintenance can be registered by selecting a timezone of a country at service.

#### (5) Message
Enter notice messages to show to users.
Can register many languages, for those who speak other languages than registered, a default language will show.
To add a language, click **+** on the right, and if a language you want is not on the list, contact [Customer Center](https://toast.com/support/inquiry)to add as required.
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

#### (6) bottom button type
Specify the type of the buttons which will be exposed at the bottom of the notice popup.
- Close: Expose the Close button only.
  Click the 'Close' button to close the popup and continue the game.

- Close+Read more: Expose the 'Close' and 'Read more' buttons.
  - Enter Manually: When the user clicks the 'Read More' button, the link that was entered in the console opens in WebView.
  - Connect to Customer Center: When set to Customer Center Provided by Gamebase, the customer center opens in WebView when the user clicks the 'Read more' button.


#### Example of a Notice Pop-up
Close (left), Close+More (right)
![gamebase_op_img](https://static.toastoven.net/prod_gamebase/gamebase_op_08_201812.png)

You can read, edit, or delete the details of the registered notice.
By default, the input items are identical to those of the registration screen. You can also delete the notice by clicking the 'Delete' button if the notice has been entered incorrectly.
If you want reregister the maintenance with similar details, you can use the copy function to easily register the notice.

## Image Notice

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_08_en_240625.png)

You can easily provide the notice as an image within the game by registering the image into the console.
The list of the current notices is exposed at the top of the list, and the list of the completed notices is displayed separately at the bottom.
Up to 10 active listings within the user's search timeframe are displayed at the top, with completed listings displayed as a separate list at the bottom.
The exposure sequence can be set by directly moving the row in the list of the current notices, and when you call the Exposure API in the game, the image registered at the top is exposed first.

> [Note]
>
> Basic settings to bulk apply image notice popups can be found in **Gamebase > App > Image Notice**.

#### properties
What's displayed on each item is as follows:

- **Image Notice**: Shows the thumbnail of the image which is to be actually exposed. The registered image is deleted 14 days after completion. Once deleted, the thumbnails will show a default image.
- **Reason**: Shows a brief description about the registered image notice. What is entered here is not exposed in the actual image notice.
- **Display Time**: Displays the time when the notice becomes exposed. When enabled, it shows the time and timezone information selected during registration.
- **Display Time (+09:00)**: Changes the time (when the notice is exposed) into Korea Standard Time (+09:00) before displaying it.
- **Modified Date**: Shows the time when the notice was most recently modified.
- **Click-Through Rate (%)**: Shows simple statistics about how many times the image notice has been displayed within the game and how many times it has actually been clicked. It shows the value against the total percentage, and you can see the graph on daily impressions and clicks during the display period by clicking the 'Confirm' button.
    * You can download and check the click-through rate data by date that users searched for and viewed within the display period. 
- **Status** : Shows the display status as follows.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_09_en_240625.png)

```
(1) To be exposed: The image notice is expected to be exposed
(2) Currently exposed: The image notice is currently being exposed
(3) Finished: The exposure has ended
```

### Register Image notice

You can register the image notice by selecting the **Register** button from the **Image Notice** list.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_10_en_240625.png)

#### (1) Target

Select the target to expose the image notice to.

- Entire game : Select it when exposure is required for all client versions.
- Some clients : Select it when exposure is required only for certain client versions. Click the 'Select version' button to display the list of the client versions registered from the client menu.
  **Example of selecting Some clients**
  Select All is possible by client status and by store. Just select the client version to expose, and click the Confirm button.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_04_en_240625.png)


#### (2) Target country
Select the country to expose the notice to.

- All countries : Exposed to all game users
- Some countries : Exposed to users from the selected countries only
  Entering the country code that you want to add will autocomplete the spelling. If you cannot find the particular country code, please contact our [Customer Center](https://toast.com/support/inquiry).

> [Note]
>
> Criteria for determining the country
> This is determined based on the criteria of **USIM** country code. If there is no USIM, the notice is exposed based on the country setting of the **device**.


#### (3) Reason
Enter the reason why the image notice has been registered.
The input information is not exposed to the game user, and you just need to enter brief information for administrative use.

#### (4) Time of exposure
Set the time when the registered image will be exposed within the game.
As for timezone, 'UTC+09:00' is selected by default, and you can also register maintenance by selecting the timezone of the country you are providing service to.

#### (5) Image
Register the image to expose within the game.
You can set the image that you want to expose per language, and the image is exposed according to the language of the device.
Registrable file formats are JPEG, JPG, and PNG, and the maximum size is 512 KB.
Recommended image size is 1200x900 (Landscape) and 900x1200 (Portrait). It maintains the aspect ratio of the image to display the entire image, but if the width or height is too long, the image can be cropped when displayed.
You can see the original image by clicking the preview image.

> [Note]
>
> The uploaded image is automatically deleted 14 days after the exposure date of the image notice ends.

#### (6) Image Click Action
Set the click action to handle when a game user clicks on an image notice. The click action can be set for each image you want to display per language, and the following click actions can be set.

- **Open URL**: Enter the URL to go to. After clicking, the entered URL will open in a new webview and the image notice window will not close automatically.
- **Payload**: Pass the entered data to the client. You can use the value passed to navigate to an in-game screen or handle certain events when the image is clicked. Any image notices opened after the click will be closed.
- **No Action**: Clicking on the image notice does not perform any other actions.

> [Note]
>
> If you want to use a specific **http** URL for the **Open URL** field, you must add a domain exception declaration to your Android build.
> Otherwise, the page will display abnormally on Android 9.0+ devices due to the OS's native limitations.
  
### Modify Image notice

You can read, edit, or delete the details of the registered images notice.
You can also register the image from the Edit screen again to replace the previous image, and edit the time and target to expose the image notice.
If you want to register the notice image again with the similar details to the already registered image notice, you can use the copy function to register by uploading a new image only.


### Modify Image notice setting
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_10_en_240709.png)
You can make basic settings for the image notice popup, and the information you set will be batch applied to all image notice.

- Image Notice Type: Specify the image notice posting type. You can choose from the following types:
  - Rolling Type: Display multiple notices in a single pop-up, so that when the image is displayed one by one, you can swipe left or right to see the next image notice.
  - Individual Pop-up: Display the registered notices as individual pop-ups, so that when you register multiple notices, the pop-ups are displayed on top of each other.
- Apply Show No More Today: Specify whether or not to show the **Show No More Today** area that appears in the bottom area in addition to the image notice.
  - Pop-up Theme: Specify the webview theme when displaying image notices. The following themes are available to choose from:
    - Dark Mode: Set the webview bottom bar colour to grey
    - Light Mode: Set the webview bottom bar colour to white
  - Popup re-display settings: Specify when to re-display the image notice popup when the **Show No More Today** setting is applied.


## Kick Out
If you need to disconnect users for reasons such as game maintenance, you can easily do so in the console.
You can see the kickout history and kickout registrations at a glance.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_11_en_240625.png)

### Register Kick Out

Clicking the **Register** button on the **Kickout** tab redirects you to the screen where you can register a kickout.

![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_12_en_240625.png)

### (1) Target to process
Select the target client to kick out.
- Entire client : Select it when kickout is required for all client versions.
- Some clients : Select it when kickout is required only for certain client versions. Click the 'Select Version' button to display the list of the client versions registered from the client menu.
  **[Example of selecting Some clients]**
  Select All is possible by client status and by store. Just select the client version to perform maintenance, and click the Confirm button.
![gamebase_op_img](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Operation/en/gamebase_maintenance_04_en_240625.png)

### (2) Reason
You can write the reason for the kickout, up to 100 characters.
This input information is not exposed to the game user, and you can enter a brief reason for registering the kickout for operational purposes.

### (3) Whether to Expose Popup
- Expose Popup: You can enter a message in the popup that is exposed to the user upon kickout.
- Do Not Expose Popup: Popup is not exposed when kicking out.

### (4) Message
A kickout message to expose to users. You can write the message only when **Whether to Expose Popup** is **Expose Popup**.
If you select 'Auto-translate to default language', the message is translated based on what was entered in the default language and the message is filled out using the appropriate language set for each item.