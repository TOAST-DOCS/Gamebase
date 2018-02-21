## Game > Gamebase > Operator Guide > Push

You can send push notification to app users. <br/>
In Gamebase, push notifications are delivered by applying TOAST Cloud Push service.<br/>

## Push
Push history delivered via Gamebase as well as list of scheduled delivery can be retrieved. <br />
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push1_1.1.png)
Reservation delivery on the list may be canceled in Retrieve Details. <br />

### Push Details 

By selecting a push on the list of all, details of delivered push messages can be retrieved .<br />

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push2_1.1.png)

On detailed reservation message delivery screen, you can see estimated delivery time . Currently, delivery on reservation messages can be canceled only; modification function is to be provided later.<br />

### Register Push
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Push3_1.1.png)

#### (1) Message Types
> Note <br/>
> This function is provided in compliance with the Act on Promotion of Information and Communications Network set by the Government of Korea. <br/>To deliver non-informative push messages, the messages should start with '(Ad)' and include contact information and how to cancel subscription .<br/>

- Ad Messages**: Input messages are added with '(Ad)' to the header. In addition, secondary contact information and how to cancel subscription are delivered altogether. For ad push messages for all in Korea, be sure to deliver as 'ad' messages to abide by the act on promotion of information and communications network. 
- **Informative Messages**: Only input messages are delivered as push messages. For users outside of Korea, you can deliver as informative messages.

> <font color="red">Caution</font><br/>
> Please note if you enter '(Ad') to messages after selecting Ad Messages, '(Ad)' will be duplicate. <br/>

#### (2) Delivery Targets  
Select a target to send push messages to. <br/>

- **All**: Can select by operating system. All users of a selected operating system will receive push messages. 
- **Particular Members**: Can be applied to send push messages to particular members only.  Push messages are sent to a device in which push token is registered with an input user ID.  
- **Group**: Register the list of users who want message delivery as a file. The group delivery can be sent up to 10,000 persons at a time. 

#### (3) Delivery Types
Select cycle of delivery.<br/>

- **Immediate  Delivery**: Send push messages immediately after registration 
- **Reservation Delivery**: Send push messages on reserved time

Further services such as repeated delivery (every day, every week, or every month) and local time-based delivery are to be provided. 

#### (4) Target Countries 
Select countries to send push messages to.<br/>

- **All Countries**: Deliver to all users 
- **Selected Countries**: Deliver to users of selected countries only  <br/>
  Enter a country code to add, then it will be automatically completed and entered. If there's no country code to enter, contact administrator.

> Note <br/>
> Meeting Criteria for Country <br/>
> When there is no USIM based on user's **USIM Country Code**, push messages are displayed on the basis of countries set on a **device**.<br />

#### (5) Delivery Messages 
Enter push messages to show to users .<br />
Messages can be registered in many languages, and for users who speak other languages than registered, default language is displayed. To add a language, click **'+'**  on the right. For other languages that are not on the list, contact [Customer Center](https://cloud.toast.com/support/faq).<br />

> <font color="blue">Note </font><br/>
> When push messages do not arrive on a device<br/>
> In most cases, users have not registered push tokens. Please check if user's push token has been registered. <br/>
>
> Refer to documents as below to setup push messages in other platforms.
>
> - [Android OS > Register PUSH](./aos-push/#2-register-push) <br />
> - [iOS > Register PUSH](./ios-push/#2-register-push) <br />
>  - [Unity > Register PUSH](./unity-push/#2-register-push) <br />
