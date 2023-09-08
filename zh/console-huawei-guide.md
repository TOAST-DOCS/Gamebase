## Game > Gamebase > Store Console Guide > Huawei AppGallery Console Guide

## Huawei Developer Console
1. After registering an account in [Huawei Developer Console](https://developer.huawei.com/consumer/en/console) <br/>
   go to Ecosystem Service / App Service -> [AppGallery Connect Console](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/).
   ![Select AppGallery Connect in Huawei Developer Console](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_eng.png)
2. If you enter the My Apps menu in the [AppGallery Connect console](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/), you can register new apps or manage existing apps.
   ![AppGallery console main screen](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_main_eng.png)
   ![Register new app in MyApps menu](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_01_eng.png)
3. For integration with Huawei store, NHN Cloud Gamebase only supports Android APK package type and mobile devices.<br/>
   In addition, since the app must be integrated with the project in the end, you must create a project in advance when creating the app, or select an existing project and link it with the app.
   ![Huawei new app registration screen](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_02_eng.png)
4. After creating the app, enter additional information and enter the information that can be found on the Huawei AppGallery Connect console in the store settings of the NHN Cloud Gamebase console.
5. This guide covers only how to link the app information registered in the Huawei AppGallery Connect and the app information of the NHN Cloud Gamebase. For more detailed steps for Huawei AppGallery app registration, refer to [Huawei documentation](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/introduction-0000001050033062).

## Setting Values ​​Required for Linking
![NHN Cloud Gamebase store settings popup](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_iap_console_en.png)
### Store App ID
- You can check the information of the project linked with the app on the AppGallery Connect > MyProject > Project Settings screen.
- You must enter the value of `App Information - Package Name` on this screen into 'Store App ID' in the NHN Cloud Gamebase console store settings.
   - This value can also be found on the AppGallery Connect > MyApp > App screen.

![AppGallery Connect project basic information screen](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

### Huawei App ID & App Secret
- You can check the information of the project linked with the app on the AppGallery Connect > MyProject > Project Settings screen.
- You must enter the `App Information - OAuth 2.0 Client ID` value on this screen as the 'Huawei App ID' value in the NHN Cloud Gamebase console store settings.
- You must enter the `App Information - OAuth 2.0 Client Secret` value on this screen as the 'Huawei App Secret' value in the NHN Cloud Gamebase console store settings.

![AppGallery Connect project basic information screen](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

### Huawei Developer Public Key
- You can check IAP-related information on the AppGallery Connect > MyProject > Earn > In-App Purchases screen.
- When you enter this screen for the first time, if you enable IAP by clicking the button that enables IAP, you can check the generated `Public Key` value.<br/>
  You must enter this value as the 'Huawei Developer Public Key' value in the NHN Cloud Gamebase console store settings.

![AppGallery Connect IAP screen](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_05_eng.png)

