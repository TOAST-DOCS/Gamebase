## Game > Gamebase > Console Guide > Ban

Gamebase bans those game users who use apps inappropriately or abusively.
When a banned user tries to log in again, or session is restored, a pop-up will be displayed to restrict game use.

Can register banning either manually in the Gamebase Console or automatically with TOAST Cloud AppGuard by using registered patterns.

Refer to [AppGuard](./ban/#appguard)on how to integrate AppGuard.


## Ban

Retrieve the list of game users who are banned/released from banning, as search conditions allow.

### Search Banned User

Retrieve the list of game users who are banned/released from banning, as search conditions allow.

![gamebase_ban_01_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_01_201812_en.png)

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

> [Note]
> By using a multi-language template, messages can be easily displayed to users in a language of choice, while the template can be reused.
> Requires more than one registered template to register banning.
> Refer to [Template](./ban/#template) to register a template.

**Search Results**

- **User ID**: User ID of a banned game user.
- **Period**: Banning period. Show as 'Permanent Banning' for a permanently banned user.
- **Template**: Template applied to register banning.
- **Reason**: Reasons that an operator enter when registering Serves only as an operation history, without displayed to users.
- **Registered User/Registration Date**: Account of an operator who registered banning/date of ban registration.
- **Release Reason**: Reasons an operator registered to release banning. Serves only as an operation history, without displayed to users.
- **Released User/Release Date**: Account of an operator who released banning/date of release.
- **Release**: A banned user is marked with the **Release** button on the search list to allow release. Click the button, and a pop-up will show to enter release reason; fill up the release reason, click **Save** , and banning is released.
- **Status**
  - <font color="white" style="background-color:#FB8F37">Ban</font>: Game user cannot access the app at the moment.
  - <font color="white" style="background-color:#A1A1A1">Ban (expired)</font>: Game user's ban has expired but user has not logged in. When the user logs in, the status will change to release (expired).
  - <font color="white" style="background-color:#88C637">Release</font>: Game user's ban has been released by operator.
  - <font color="white" style="background-color:#2AB1A6">Release (expired)</font>: Game user's ban has been released due to expiration.


> [Note]
> Click **Download Files** and save search results in CSV files.



### Register Ban

Can register banning by clicking **Register** on the Retrieve Ban page.

![gamebase_ban_02_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_02_201812_en.png)

#### (1) User ID
Enter a Gamebase user ID to register banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered at once by batch.
  ![gamebase_ban_03_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_03_201812_en.png)

> [Note]</br>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file.
> ![gamebase_ban_04_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_04_201812_en.png)

#### (2) Period
Set a banning period for a game user. Banning will be applied from the moment of registration.

- **Permanent**: Select to ban permanently.
- **Specify Period**: Enter how long to ban by day and hour. Can expect banned period with **anticipated expiration time**.

#### (3) Reason
Enter reasons why a user is to be banned.
Serves only as an operation history, without exposed to users.

#### (4) Message
Enter messages to be displayed to banned user.
A multi-language template to display messages is supported for users and allow easy re-use. Select a pre-registered template to register.

> <font color="red">[Note]</font>
> Can register banning, only when a template of displayed message is registered.
> If a template has not been registered, first go to **Template** of **BAN** to register a template.
> Refer to [Template](./ban/#template)on how to register a template.

#### (5) Delete Leaderboard records

When the account is to be banned, you can configure whether to delete the leaderboard records or not. The deleted leaderboard records <font color="red">cannot be recovered</font>.

### Release Ban

Can release banning by clicking **Release** on the Retrieve Ban page.

![gamebase_ban_05_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_05_201812_en.png)

#### Release Reason
Enter reasons why a user is to be released from banning.
Serves only as an operation history, without exposed to users.

#### User ID
Enter a Gamebase user ID to release banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered by batch at once.

![gamebase_ban_06_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_06_201812_en.png)


> [Note]
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file.
> ![gamebase_ban_04_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_04_201812_en.png)

## Template
Provides a multi-language template to display messages for users and allows easy re-use.
Select a pre-registered template. Can register by language, and a message of banning will be displayed for banned users based on the language set on each of their devices.

### Search

Search the list of registered templates.
Can register a new template, or modify registered templates, but cannot delete them.

![gamebase_ban_07_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_07_201812_en.png)

- The Message tab will show messages in 'Default Language' registered in Template.

### Register Template

![gamebase_ban_08_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_08_201812_en.png)

#### (1) Name
Enter a name of template to show on the list to register banning.

#### (2) Message
Enter messages for banned users.
Can be registered in many languages, and for other language users, 'Default Language' will show. To add a language, click **+** on the right, and if there's any other languages you want on the list, contact [Customer Center](https://toast.com/support/inquiry).

## AppGuard

> <font color="red">[Note]</font>
> This service is available only for TOAST Cloud AppGuard users.

![gamebase_ban_09_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_ban_09_201812_en.png)

- **Integration Status**: To be enabled only when users who are detected or sanctioned by AppGuard need to be automatically registered as Gamebase banned users.
- To **Ban Automatically** by detection/sanction type, select **ON** , enter 'User Messages' and **Range** , and click **Save**.
- You can choose whether to delete the leaderboard records automatically or not.

> [Note]
> In case banning is automatically registered due to AppGuard integration, 'AppGuard' will be registered at a result page.

