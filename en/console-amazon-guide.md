<!-- pre-align:aligned sig=83150409a851 -->

<a id="game-gamebase-store-console-guide-amazon-appstore-console-guide"></a>
## Game > Gamebase > Store Console Guide > Amazon Appstore Console Guide { #game-gamebase-store-console-guide-amazon-appstore-console-guide }

<a id="amazon-developer-console"></a>
## Amazon Developer Console { #amazon-developer-console }
1. After registering an account in [Amazon Developer Console](https://developer.amazon.com/), create an app in the Amazon Appstore Management menu.
   ![Amazon Developer Console](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_developer_console_eng.png)
2. NHN Cloud IAP officially supports only the Amazon Appstore app for the Android platform. Select the Android platform, enter the app name, and click the `Create app` button to create an app.
   ![Select app platform](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_0_eng.png)
3. After creating an app, you can check the list of created apps as shown below.
   ![App list in Appstore Developer Console](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_1_eng.png)
4. After creating the app, enter additional information and enter the information provided by the Developer Console in the app settings of the NHN Cloud IAP console.
5. This guide covers only how to link the app information registered in the Amazon Appstore and the app information of the NHN Cloud IAP. <br/> For more detailed steps for Amazon Appstore app registration, refer to [Amazon documentation](https://developer.amazon.com/apps-and-games/documentation).

<a id="setting-values-required-for-linking"></a>
## Setting Values ​​Required for Linking { #setting-values-required-for-linking }
![NHN Cloud IAP app settings popup](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_iap_console_en.png)
<a id="store-app-id"></a>
### Store App ID { #store-app-id }
- Click the name of the created app on the [app list](https://developer.amazon.com/apps-and-games/console/apps/list.html) screen to enter the detailed settings menu.
- In the detailed settings menu, click Edit and enter 'App SKU', and enter the value of this App SKU in 'Store App ID' in the NHN Cloud Gamebase console store settings.
  ![App detailed settings screen in Amazon Developer Console](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_2_eng.png)


<a id="amazon-shared-key"></a>
### Amazon Shared Key { #amazon-shared-key }
- Go to [Settings -> Identity menu](https://developer.amazon.com/settings/console/sdk/shared-key) in the Amazon Developer Console, and you can check the shared key on the following screen.
  ![Identity screen in Amazon Developer Console](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_3_eng.png)
- You must enter this value in the 'Amazon Shared Key' item in the NHN Cloud Gamebase console store settings.