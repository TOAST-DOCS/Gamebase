## Game &gt; Gamebase &gt; Operator Guide &gt; Push

You can send push notification to app users.<br/>
In Gamebase, push notifications are provided by applying TOAST Cloud Push service.<br/>

## Push
Check push history delivered via Gamebase and registered list of push schedule. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push1_1.3.png)
Scheduled push delivery on the list can be cancelled from Push Details.<br />

### Registered List

Select a push on the list of scheduled delivery to check expected delivery time and registration information. <br>
Currently, delivery schedule can only be cancelled; the modification function will be provided in close future.<br />

### Send History

Select a push on the list of delivery history to retrieve details of delivered push messages.<br />
You can easily register by clicking the 'Copy' button to use the registration information of the sent push.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push2_1.2.png)


### Register Push

Click **Register** to register a new push.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push3_1.1.png)

#### (1) Message Type
> [Note]<br/>
> This function is provided in compliance with the Act on Promotion of Information and Communications Network set by the Government of Korea. <br/>
> To deliver non-informative push messages, the messages should start with &#39;(Ad)&#39; and include contact information and how to cancel subscription.<br/>

- **Ad Messages**: Input messages are added with &#39;(Ad)&#39; to the header. In addition, secondary contact information and how to cancel subscription are delivered altogether. For ad push messages for all in Korea, be sure to deliver as &#39;ad&#39; messages to abide by the act on promotion of information and communications network.
- **Informative Messages**: Only input messages are delivered as push messages. Send as informative messages for non-Korean device users (when the USIM country code is not Korea).

> <font color="red">[Caution]</font><br/>
> Please note if you enter &#39;(Ad)&#39; to messages after selecting Ad Messages, &#39;(Ad)&#39; will become duplicate. <br/>

#### (2) Push Target
Select a target to send push messages to. <br/>

- **All**: 운영체제별로 선택할 수 있습니다. 선택한 운영체제를 사용하는 사용자는 모두 푸시를 수신하게 됩니다.
- **Specific user**: Can be applied to send push messages to particular members only. Push messages are sent to a device in which push token is registered with an input user ID.
- **Group**: Register the list of users who want message delivery in a file. The group delivery can be sent up to 10,000 persons at a time.

#### (3) Type
Select a cycle of delivery.<br/>

- **Immediate Delivery**: Send push messages immediately after registration
- **Reservation Delivery**: Send push messages on reserved time. Further services such as repeated delivery (every day, every week, or every month) and local time-based delivery are to be provided. 

#### (4) Target Country
Select countries to send push messages to.<br/>

- **All Countries** : Deliver to all users
- **Selected Countries**: Deliver to users of selected countries only. <br/>
  Enter a country code to add, then it will be automatically completed and entered. If there&#39;s no country code to enter, contact  [Customer Center](https://toast.com/support/inquiry).

> [Note]<br/>
> Criteria of Country Selection<br/>
> Countries are selected on the basis of user&#39;s **USIM Country Code** ; when USIM is not available, countries set on **Device** are the target.<br />

#### (5) Message
Enter push messages to show to users.<br />
Messages can be registered in many languages, and for users who speak other languages than registered, default language is displayed. To add a language, click **&#39;+&#39;** on the right. For other languages that are not on the list, contact  [Customer Center](https://toast.com/support/inquiry).<br />

> [Note]<br/>
> If push messages do not arrive on a device: in most cases, it is because users have not registered push tokens. Please check if user&#39;s push token has been registered.<br/>
> Refer to documents as below to setup push messages in other platforms.
>
> - [Android > Register Push](./aos-push/#2-register-push) <br />
> - [iOS > Register Push](./ios-push/#2-register-push) <br />
> - [Unity > Register Push](./unity-push/#2-register-push) <br />

