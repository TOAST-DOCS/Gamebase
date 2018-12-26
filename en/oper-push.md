## Game > Gamebase > Console Guide > Push

You can send push notification to app users.
In Gamebase, push notifications are provided by applying TOAST Cloud Push service.

## Push
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

![gamebase_push_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_push_03_201812_en.png)

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

> [Note]
> If push messages do not arrive on a device: in most cases,
> it is because users have not registered push tokens. Please check if user's push token has been registered.
> Refer to documents as below to setup push messages in other platforms.
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)

