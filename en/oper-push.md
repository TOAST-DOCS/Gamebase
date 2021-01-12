## Game > Gamebase > Console Guide > Push

You can send push notification to app users.
In Gamebase, push notifications are provided by applying TOAST Cloud Push service.

## Push

> <font color="red">[Important]</font><br/>
>
> The service menu is enabled for **Gamebase SDK 2.6.0** or higher game versions.   

Push delivery history and registered push reservation list can be found.  

![gamebase_push_01_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_01_201910.png)

### Registered List

Select a push from the list to find its scheduled delivery time and registration information; click **Cancel Delivery** to cancel scheduled delivery or **Copy** to newly register a push by using registered push information.

### Send History

Select a push from the list of delivery history to find details of the sent push. Click **Copy** to easily register push by using registration information of the sent push.  

![gamebase_push_02_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_02_201910.png)
![gamebase_push_03_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_03_201910.png)

### Register Push

To register a new push, click **Register**.
See Preview UI on the right to find values that are actually registered on console.

![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_push_04_202004.png)
![gamebase_push_05_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_05_201910.png)

#### (1) Delivery Type

Select a delivery cycle.
- **Immediate Delivery**: Send push messages immediately after registered.
- **Scheduled Delivery**: Send push messages on scheduled time. To send push messages as of time of user country on device, select **Send by Current Time**. Set '09:00, 01-01-2017', and Korean device users receive push messages at 09:00 on January 1st of 2017 in Korean time, whereas UK device users receive their messages at 09:00 on January 1st of 2017 in UK time.
- **Repetitive Delivery**: Send same pushes periodically.  To send push messages as of country time of user's device, select **Send by Current Time**. Select period to send messages, and select time to send. Then, enter information for a selected cycle during selected period, such as **Daily, Weekly, or Monthly** basis.   
	- Daily: Send push messages every day.
	- Weekly: Select a particular day to send push messages. Days can be redundantly selected.
	- Monthly: Send push messages on dates of a month. Only dates are available (1~31), one or more. For instance, to send push messages on the 1st, 5th, and 10th, enter 1, 5, and 10.  

#### (2) Delivery Target
Select a target to send push messages to.
- **All**: To be selected by each operating system. All users on a selected operating system shall receive push messages.
- **Particular Members Only**: To be selected to send push messages to particular members only. Send push messages onto a device in which push token is registered with user ID. If a target user plays games on five devices, messages are sent to the five devices.
- **Group**: Register a list of users who need messages in a file: available up to 10,000 persons at a time.  
- **Tags**: Use tags to send push messages. Deliver with tags that are registered on the **Push-Tag** menu, and many tags can be registered. Tag filters support the following features:
	- AND: Send push messages only to the users who satisfy all selected tags.
	- OR: Send push messages to the users who satisfy at least one of the selected tags.

#### (3) Target Country

Select countries to send push messages.
- **All Countries**: Send push messages to all users.
- **Selected Countries**: Send push messages to users of selected countries.
  Enter a country name to add for an auto-complete. When the country you need is missing from the list, contact [Customer Center](https://toast.com/support/inquiry).

> [Note]
>
> Criteria as Country
> A country is based on the user's **USIM Country Code**; if USIM is not available, push messages show based on the country set on user's device.   

#### (4) Notification Sound (Optional)

You may set a notification sound when receiving push messages on device.
Click **Add Notification Sound** to set sounds; otherwise, default sound shall be played.
Enter external URL link or a file path for notification sound deployed within app.  


#### (5) Message Type

> [Note]
>
> This feature is provided to comply with the 'Act on Promotion of Information and Communications Network Utilization and Information Protection' of Kora.
> Unless a push message is informative, it must start with '(Ad)' and include contact and method of unsubscription.

- **Promotional**: 'Ad' phrase is added to a message input. Further contact information and unsubscription method are delivered as well. Particulary in Korea,  when it comes to ad push messages sent to all subscribers, 'Promotional' must be indicated to respect the 'Act on Promotion of Information and Communications Network Utilization and Information Protection'.
- **Informative**: Only input messages are sent as push messages. For non-Korean device users (of which the USIM country code is not Korea), send messages as informative.   


> <font color="red">[Important]</font><br/>
>
> After selecting **Promotional**, do not further add '(Ad)' to your message to prevent redundancy of the '(Ad)' phrase.   

#### (6) Messages

Enter push messages to show to user.

A message can be registered in many languages, and for any other language users, default language is selected. To add a language, click **Add Messages** at the bottom. To add any other languages out of the list, contact [Customer Center](https://toast.com/support/inquiry).
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

> [Note]
>
> When your device does not receive push messages, it is mostly because a push token is not registered. Check if user's push token has been registered.
> Regarding how to register push tokens on a platform, see the following document.   
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)

#### (7) Message Color (Android Only)

On Android devices, you can set a color for push messages.  
Different colors can be applied for title and body, by selecting a color from a color swatch or entering RGB Hex directly.
You can check a selected color from preview on the right.

#### (8) Large Icons (Android Only)

Clik **Add Large Icons** to set an image of the icon which will show on the right when a push message is received.
Set the image and send a message, and the image shall show only on an Android device, but not on iOS.  
Select External and enter external URL for the icon image to show, or select Internal and enter the file path of the icon image deployed within app.
For an external image, check the icon from preview on the right.

#### (9) Buttons

You may send a message along with up to three buttons, to be displayed at the bottom of the message.
Buttons are available in four types, and each type provides different method of operations and setting, like the guides as below:

- Open App: Click button from a push message to execute the app. Only the name can be specified.
- Open URL: Click button from a push message to go to URL entered on console. Any particular app can be executed via schema app or connected to a webpage.
- Respond: Click button from a push message to execute a window for response. For iOS, name for a delivery button can be set as well.
- Close: Click buttom from a push message to close the push. Only the name can be specified.

#### (10) Media (iOS)

Dynamically executed media can be added to receive an iOS push message.
You may set **IMAGE**, **GIF**, **VIDEO**, or **AUDIO**, by entering external URL or internal file path to be connected.  

#### (11) Media (Android)

Media can be added to receive an Android push message.
Currently, Android allows the setting of **IMAGE** only by entering external URL or internal file path to be connected.

## Push(Old)
Check push history delivered via Gamebase and registered list of push schedule.
![gamebase_push_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_push_01_201812_en.png)
Scheduled push delivery on the list can be cancelled from Push Details.

### Registered List

Select a push on the list of scheduled delivery to check expected delivery time and registration information.
Currently, delivery schedule can only be cancelled; the modification function will be provided in close future.


### Send History

Select a push on the list of delivery history to retrieve details of delivered push messages.
You can easily register by clicking the **Copy** button to use the registration information of the sent push.
![gamebase_push_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_push_02_201812_en.png)


### Register Push

Click **Register** to register a new push.

![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_push_old_03_202004.png)

#### (1) Message Type
> [Note]
> This function is provided in compliance with the Act on Promotion of Information and Communications Network set by the Government of Korea.
> To deliver non-informative push messages, the messages should start with '(Ad)' and include contact information and how to cancel subscription.

- **Ad Messages**: Input messages are added with '(Ad)' to the header. In addition, secondary contact information and how to cancel subscription are delivered altogether. For ad push messages for all in Korea, be sure to deliver as 'ad' messages to abide by the act on promotion of information and communications network.
- **Informative Messages**: Only input messages are delivered as push messages. Send as informative messages for non-Korean device users (when the USIM country code is not Korea).

> <font color="red">[Caution]</font>
> Please note if you enter '(Ad)' to messages after selecting Ad Messages, '(Ad)' will become duplicate.

#### (2) Push Target
Select a target to send push messages to.

- **All**: Select by operating system. Any user using a selected operating system shall receive push messages.
- **Specific user**: Can be applied to send push messages to particular members only. Push messages are sent to a device in which push token is registered with an input user ID.
- **Group**: Register the list of users who want message delivery in a file. The group delivery can be sent up to 10,000 persons at a time.

#### (3) Type
Select a cycle of delivery.

- **Immediate Delivery**: Send push messages immediately after registration
- **Reservation Delivery**: Send push messages on reserved time. Further services such as repeated delivery (every day, every week, or every month) and local time-based delivery are to be provided. For instance, if you check 'Deliver by Local Time' and set time in '2017- 01-01 09:00', a Korean device user shallÍ receive a push message in '2017-01-01 09:00' in Korean time, while a British device user shall receive his own in '2017-01-01 09:00' in British time.

Repeated delivery services (every day, week, or month) are to be added for Reservation Delivery.

#### (4) Target Country
Select countries to send push messages to.

- **All Countries** : Deliver to all users
- **Selected Countries**: Deliver to users of selected countries only.
  Enter a country code to add, then it will be automatically completed and entered. If there's no country code to enter, contact  [Customer Center](https://toast.com/support/inquiry).

> [Note]
> Criteria of Country Selection
> Countries are selected on the basis of user's **USIM Country Code** ; when USIM is not available, countries set on **Device** are the target.

#### (5) Message
Enter push messages to show to users.
Messages can be registered in many languages, and for users who speak other languages than registered, default language is displayed. To add a language, click **'+'** on the right. For other languages that are not on the list, contact  [Customer Center](https://toast.com/support/inquiry).
Click **Auto Translate to Default Language** and messages in default language are translated into a language set for each item. 

> [Note]
> If push messages do not arrive on a device: in most cases,
> it is because users have not registered push tokens. Please check if user's push token has been registered.
> Refer to documents as below to setup push messages in other platforms.
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)


## Tag

With tags, users can be bound by particular criteria for delivery.   

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_06_201910.png)

Tag names can be registered for push delivery at TOAST PUSH.



### Register

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_09_201910.png)



### Details

Registered tags, as well as the list of registered users of each tag, can be managed.  

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_push_07_201911.png)

Use Delete/Modify on top to delete or modify tag information, and Manage User ID at the bottom to register or delete users to/from tags.

#### User Registration

##### By Case

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_08_201910.png)

##### By File

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_10_201910.png)

Pree Register, and a pop-up shows like the above; enter ID or register a file.

To register with files, up to 1000 persons can be registered at once.



#### User Deletion

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_11_201910.png)

To delete a user, check the box on the left of the list and press Delete.
