## Game > Gamebase > Operator Guide > Ban
Provides ban of app use for those game users who use apps inappropriately or abusively. <br/>
When a banned user tries to log in again or restores session, a pop-up on ban will be displayed to restrict game use. <br/>

Ban can be registered, either manually in Gamebase Console or automatically with TOAST Cloud AppGuard by registering patterns. 

Refer to [AppGuard](./ban/#appguard) on how to integrate AppGuard. 


## Ban

Can retrieve ban history, register ban, or release ban from banned game users. <br/>

### Search Banned User
Retrieve the list of game users who are banned/released from ban, as search condition allows. 

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban1_1.0.png)

**Search Conditions**

- **Status**: (Required) Select a status of ban (ban/release ban). Cannot choose both but one only.  
- **Registration Date**: (Required) Select the range of registration date of ban (from-to)
- **User ID**: Gamebase user IDs of those who are banned/released from ban 
- **Template**: Select and retrieve a particular template applied when registering a ban 
- **Registration System**: Select and retrieve a system registering ban. Can make multiple choices. 
  - **Console**: Register via Gamebase Console
  - **AppGuard**: Automatically register with AppGuard integration 
  - **External Server**: Register at an app operating server or other external servers
  - **Others**: Register other cases of ban, except the above (e.g. direct API calls) 

> [Note] <br/>
> A template of multi-language input is provided to display message to users and allow easy reuse. <br/>
> Can register ban, only when a template of displayed messages is registered. <br/>
> Refer to [Template](./ban/#template) to register a template. <br/>

**Search Results**

- **User ID**: User ID of a banned game user 
- **Range**: Ban period. Show as ''Permanent  Ban' for a permanent banned user 
- **Template**: Template applied to register a ban 
- **Reason**: Reasons that an operator enter when a ban is registered. Serves only as an operation history, without exposed to users.
- **Registration User/Registration Date**: Account of an operator who registered a ban/date of ban registration 
- **Release Reason**: Reasons an operator registered to release ban. Serves only as an operation history, without exposed to users. 
- **Release User/Release Date**: Account of an operator who released a ban/date of ban release 
- **Release**: A banned user is marked with the **Release** button on the search list to allow release. Click the button, and a pop-up will show to enter release reason; fill up the release reason, click **Save**, and a ban is released.   
- **Status**
  - <font color="white" style="background-color:#FB8F37">Ban</font>: Game user cannot access the app at the moment  
  - <font color="white" style="background-color:#A1A1A1">Ban (expired)</font>: Game user's ban has expired but user has not logged in. When the user logs in, the status will change to release (expired). 
  - <font color="white" style="background-color:#88C637">Release</font>: Game user's ban has been released by operator. 
  - <font color="white" style="background-color:#2AB1A6">Release (expired)</font>: Game user's ban has been released due to expiration. 


> <font color="blue">[Note]</font><br/>
> Click **Download Files** and save search results in CSV files.  <br/>



### Register Ban

Can register a ban by clicking **Register** on the Retrieve Ban page.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban2_1.0.png)
#### (1) User ID
Enter a Gamebase user ID to register ban. Multiple users can be registered at once, following the two methods as below. 

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.  
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered at once by batch.  <br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban4_1.0.png)

> <font color="blue">[Note]</font><br/>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban5_1.0.png)

#### (2) Range
Set a ban period for a game user. Ban will be applied from the moment of registration.  <br />

- **Permanent Ban**: Select to ban permanently.
- **Specify Period**: Enter how long to ban by day and hour. Can expect banned period with **anticipated expiration time**.  <br />

#### (3) Reason
Enter reasons why a user is to be banned. <br />
Serves only as an operation history, without exposed to users.<br />

#### (4) Message
Enter a ban message to be displayed to user. <br/>
A template of multi-language input is provided to display message to users and allow easy reuse. Select a pre-registered template to register.<br />

> <font color="red">[Note]</font><br/>
> Can register ban, only when a template of displayed message is registered.  <br/>
> If a template has not been registered, first go to **Template** of **BAN** to register a template. <br/>
> Refer to [Template](./ban/#template) on how to register a template. <br/>


### Release Ban

Can release a ban by clicking **Release** on the Retrieve Ban page. Register Ban by clicking **Register** on the Retrieve Ban page.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban3_1.2.png)

#### Reason 
Enter reasons why a user's ban is to be released. <br />
Serves only as an operation history, without exposed to users. <br />

#### User ID
Enter a Gamebase use ID to release ban. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.  
- **Batch Registration 등록**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered by batch at once.   <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban6_1.0.png)


> <font color="blue">[Note]</font><br/>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban7_1.0.png)

## Template
Provide a template of multi-language input to display message to users and allow easy reuse. Select a pre-registered template. 
Can register by language, and ban messages will be displayed for banned users based on the language set on each of their devices.  

### Search

Enable the search of registered list of templates. <br/>
Can register a new template or modify registered templates, but cannot delete templates.  <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template1_1.1.png)

- Messages on the list are displayed with a 'default language'.

### Register Template
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template2_1.1.png)

#### (1) Name
Enter a name of template to show on the list to register ban. <br/>

#### (2) Message 	
Enter messages for banned users. <br />
Can be registered in many languages, and for other language users, default language will be displayed. To add a language, click **+** on the right. If there's any other languages you want on the list, contact [Customer Center](https://cloud.toast.com/support/faq).  <br />

## AppGuard

> <font color="red">[Note]</font><br/>
> Service is available only for TOAST Cloud AppGuard product users. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_AppGuard1_1.0.png)

- **Integration Status**: To be enabled only when users who are detected or sanctioned by AppGuard need to be automatically registered as Gamebase banned users. 
- To apply **automatic ban** by detection/sanction type, select **ON**, enter 'Message for User' and **Ban Period**, and click **Save**. 

> [Note] <br/>
> In case a ban is automatically registered by AppGuard integration, 'AppGuard' will be registered as the Registration System. 

