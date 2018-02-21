## Game &gt; Gamebase &gt; Operator Guide &gt; Ban

Gamebase bans those game users who use apps inappropriately or abusively.<br/>
When a banned user tries to log in again, or session is restored, a pop-up will be displayed to restrict game use.<br/>

Can register banning either manually in the Gamebase Console or automatically with TOAST Cloud AppGuard by using registered patterns.

Refer to [AppGuard](./ban/#appguard)on how to integrate AppGuard.


## Ban

Retrieve the list of game users who are banned/released from banning, as search conditions allow.<br/>

### Search Banned User

Retrieve the list of game users who are banned/released from banning, as search conditions allow.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban1_1.1.png)

**Search Conditions**

- **Status**: (Required) Select a status of banning (ban/release ban). Must choose only one, not both.
- **Registration Date**: (Required) Select the range of registration date of banning (from-to).
- **User ID**: Gamebase user IDs of those who are banned/released from ban.
- **Template**: Select and retrieve a particular template which is applied to register banning.
- **Registration System**: Select and retrieve a system which registers banning. Can make multiple choices.
  - **Console**: Register via Gamebase Console.
  - **AppGuard**: Automatically register with AppGuard integration.
  - **External Server**: Register at an app operating server or other external servers.
  - **ETC**: Register other banning cases, except the above (e.g. direct API calls).

> [Note] <br/>
> By using a multi-language template, messages can be easily displayed to users in a language of choice, while the template can be reused.<br/>
> Requires more than one registered template to register banning.<br/>
> Refer to [Template](./ban/#template) to register a template.<br/>

**Search Results**

- **User ID**: User ID of a banned game user.
- **Period**: Banning period. Show as &#39;&#39;Permanent Banning&#39; for a permanently banned user.
- **Template**: Template applied to register banning.
- **Reason**: Reasons that an operator enter when registering Serves only as an operation history, without displayed to users.
- **Registered User/Registration Date**: Account of an operator who registered banning/date of ban registration.
- **Release Reason**: Reasons an operator registered to release banning. Serves only as an operation history, without displayed to users.
- **Released User/Release Date**: Account of an operator who released banning/date of release.
- **Release**: A banned user is marked with the **Release** button on the search list to allow release. Click the button, and a pop-up will show to enter release reason; fill up the release reason, click **Save** , and banning is released.
- **Status**
  - <font color="white" style="background-color:#FB8F37">Ban</font>: Game user cannot access the app at the moment.
  - <font color="white" style="background-color:#A1A1A1">Ban (expired)</font>: Game user&#39;s ban has expired but user has not logged in. When the user logs in, the status will change to release (expired).
  - <font color="white" style="background-color:#88C637">Release</font>: Game user&#39;s ban has been released by operator.
  - <font color="white" style="background-color:#2AB1A6">Release (expired)</font>: Game user&#39;s ban has been released due to expiration.


> [Note]<br/>
> Click **Download Files** and save search results in CSV files.<br/>



### Register Ban

Can register banning by clicking **Register** on the Retrieve Ban page.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban2_1.1.png)
#### (1) User ID
Enter a Gamebase user ID to register banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered at once by batch. <br/>
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban4_1.1.png)

> [Note]</br>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban5_1.0.png)

#### (2) Period
Set a banning period for a game user. Banning will be applied from the moment of registration.<br />

- **Permanent**: Select to ban permanently.
- **Specify Period**: Enter how long to ban by day and hour. Can expect banned period with **anticipated expiration time**.<br />

#### (3) Reason
Enter reasons why a user is to be banned.<br />
Serves only as an operation history, without exposed to users.<br />

#### (4) Message
Enter messages to be displayed to banned user. <br/>
A multi-language template to display messages is supported for users and allow easy re-use. Select a pre-registered template to register.<br />

> <font color="red">[Note]</font><br/>
> Can register banning, only when a template of displayed message is registered. <br/>
> If a template has not been registered, first go to **Template** of **BAN** to register a template.<br/>
> Refer to [Template](./ban/#template)on how to register a template.<br/>


### Release Ban

Can release banning by clicking **Release** on the Retrieve Ban page.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban3_1.2.png)

#### Release Reason
Enter reasons why a user is to be released from banning.<br />
Serves only as an operation history, without exposed to users.<br />

#### User ID
Enter a Gamebase user ID to release banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered by batch at once. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban6_1.0.png)


> [Note]<br/>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file. <br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban7_1.0.png)

## Template
Provides a multi-language template to display messages for users and allows easy re-use. <br />
Select a pre-registered template. Can register by language, and a message of banning will be displayed for banned users based on the language set on each of their devices.

### Search

Search the list of registered templates.<br/>
Can register a new template, or modify registered templates, but cannot delete them.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template1_1.1.png)

- The Message tab will show messages in &#39;Default Language&#39; registered in Template.

### Register Template
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template2_1.1.png)

#### (1) Name
Enter a name of template to show on the list to register banning. <br/>

#### (2) Message
Enter messages for banned users. <br />
Can be registered in many languages, and for other language users, &#39;Default Language&#39; will show. To add a language, click **+** on the right, and if there&#39;s any other languages you want on the list, contact [Customer Center](https://toast.com/support/inquiry).<br />

## AppGuard

> <font color="red">[Note]</font><br/>
> This service is available only for TOAST Cloud AppGuard users. <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_AppGuard1_1.0.png)

- **Integration Status**: To be enabled only when users who are detected or sanctioned by AppGuard need to be automatically registered as Gamebase banned users.
- To **Ban Automatically** by detection/sanction type, select **ON** , enter &#39;User Messages&#39; and **Range** , and click **Save**.

> [Note] <br/>
> In case banning is automatically registered due to AppGuard integration, &#39;AppGuard&#39; will be registered at a result page.

