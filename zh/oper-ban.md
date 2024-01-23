## Game > Gamebase > Console Guide > Ban

Gamebase bans those game users who use apps inappropriately or abusively.
When a banned user tries to log in again, or session is restored, a pop-up will be displayed to restrict game use.

You can register banning either manually in the Gamebase Console or automatically with NHN Cloud Cloud AppGuard by using registered patterns.

Refer to [AppGuard](./oper-ban/#appguard) on how to integrate AppGuard.


## Ban

Retrieve the list of game users who are banned/released from banning, as search conditions allow.

### Search Banned User

Retrieve the list of game users who are banned/released from banning, as search conditions allow.

![gamebase_ban_01_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_01_en_240105.jpg)

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
>
> By using a multi-language template, messages can be easily displayed to users in a language of choice, while the template can be reused.
> Requires more than one registered template to register banning.
> Refer to [Template](./oper-ban/#template) to register a template.

**Search Results**

- **User ID**: User ID of a banned game user.
- **Period**: Banning period. Shown as 'Permanent Banning' for a permanently banned user.
- **Template**: Template used for registering banning.
- **Reason**: Reasons that an operator entered when registering banning. Serves only as an operation history, without being displayed to users.
- **Registered User/Registration Date**: Account of an operator who registered banning/date of ban registration.
- **Release Reason**: Reasons an operator registered to release banning. Serves only as an operation history, without being displayed to users.
- **Released User/Release Date**: Account of an operator who released banning/date of release.
- **Release**: A banned user is marked with the **Release** button on the search list to allow release. Click the button, and a pop-up to enter release reason will show up. Enter the release reason, click **Save**, and banning is released.
- **Status**
    - <font color="white" style="background-color:#FB8F37">Ban</font>: Game user cannot access the app at the moment.
    - <font color="white" style="background-color:#A1A1A1">Ban (expired)</font>: Game user's ban has expired but user has not logged in. When the user logs in, the status will change to release (expired).
    - <font color="white" style="background-color:#88C637">Release</font>: Game user's ban has been released by operator.
    - <font color="white" style="background-color:#2AB1A6">Release (expired)</font>: Game user's ban has been released due to expiration.


> [Note]
>
> Click the **File Download** button, and you can save search results in CSV files.



### Register Ban

You can register banning by clicking **Register** on the Retrieve Ban page.

![gamebase_ban_02_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_02_en_240105.jpg)
#### (1) User ID
Enter a Gamebase user ID to register banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered at once by batch.
  ![gamebase_ban_03_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_03_en_240105.jpg)

> [Note]</br>
>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file.
> ![gamebase_ban_04_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_04_en_240105.jpg)

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

> <font color="red">[Important Note]</font>
> You can register banning only when a template of displayed message is registered.
> If a template has not been registered, first go to **Template** of **BAN** to register a template.
> Refer to [Template](./oper-ban/#template) on how to register a template.

#### (5) Delete Leaderboard
When registering banning, you can select whether to delete the game user's Leaderboard data or not.
If you select this option and register banning, data of the game user is deleted. Caution is required because the deleted Leaderboard data <font color="red">cannot be recovered</font>.

### Release Ban

You can release banning by clicking **Release** on the Retrieve Ban page.

![gamebase_ban_05_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_05_en_240105.jpg)

#### Release Reason
Enter reasons why a user is to be released from banning.
Serves only as an operation history, without exposed to users.

#### User ID
Enter a Gamebase user ID to release banning. Multiple users can be registered at once, following the two methods as below.

- **User Input**: Directly enter a user ID to register and press **Enter** or click **Add.** As validity is checked for user IDs, invalid user IDs cannot be entered.
- **Batch Registration**: Can upload CSV files only, and an example file can be downloaded from the Console page. Up to 10,000 persons can be registered by batch at once.

![gamebase_ban_06_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_06_en_240105.jpg)


> [Note]
>
> If batch registration fails during progress, a pop-up will be displayed. Click **Download** from the pop-up to download the list of users who are failed to be registered in a file.
> ![gamebase_ban_04_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_04_en_240105.jpg)

## Template
Provides a multi-language template to display messages for banning target users and allows easy re-use. Select a pre-registered template.
You can register templates by language, and a message of banning will be displayed for banning target users based on the language set on each of their devices.

### Search

Allows you to search the list of registered templates.
You can register a new template, or modify registered templates, but cannot delete them.

![gamebase_ban_07_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_07_en_240105.jpg)

- In the template list, the Message item will show messages in a language entered as 'Default Language' while registering a template.

### Register Template
![gamebase_app_01_202004](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_08_en_240105.jpg)

#### (1) Name
Enter a name of template to show on the list to register banning.

#### (2) Message
Enter messages for banning target users.
The messages can be registered in multiple languages, and for users using other languages, the language selected as 'Default Language' will be displayed. To add a language, click **+** on the right, and if there's any other language you want on the list, contact [Customer Center](https://toast.com/support/inquiry).
Click **Auto Translate to Default Language** and messages in the default language are translated into a language set for each item.

## AppGuard

> <font color="red">[Important Note]</font>
> The AppGuard integration function is available only when the NHN AppGuard service is activated in the same project as the service to which the function is to be applied in NHN Cloud.

![gamebase_ban_09_201812_en](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/en/gamebase_ban_09_en_240105.jpg)

- **Linking**: Enable this if you want to automatically register the users detected or restricted by AppGuard as Gamebase-banned users.
- There are two types of **Auto Ban**: **Immediate Block** and **Conditional Block**.
    - Banning by **Immediate Block** is performed at 15 and 45 minutes of every hour. For **Immediate Block**, the detection period cannot be set, and the number of detection is fixed to one.
    - Banning by **Conditional Block** is performed once a day at midnight.
        - Example: If "Detection Period: 5" and "Detection Count: 3" are set for tampering
            - If a user's tampering activity is detected 3 or more times during the period of January 14-18, user ban will be registered on January 19 at midnight.
- There are two types of **Ban Type**: **Permanent Ban** and **Conditional Ban**.
    - **Permanent Ban** permanently bans the users who meet the automatic ban conditions.
    - **Conditional Ban** bans the users who meet the automatic ban conditions for a set period.
- If you select **Delete Leaderboard**, Leaderboard data will also be deleted for users who have been automatically banned.

> [Note]
>
> In case banning is automatically registered due to AppGuard integration, it will be registered as 'AppGuard' in the registration system.
