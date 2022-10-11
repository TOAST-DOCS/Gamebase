## Game > Gamebase > 商店控制台指南 > Huawei AppGallery控制台指南

## Huawei Developer Console
1. 在[Huawei开发人员控制台](https://developer.huawei.com/consumer/en/console) 注册账户后 <br/>
   进入Eco System服务 / 应用程序服务 -> [AppGallery Connect控制台](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/) 。
   ![在Huawei开发人员控制台中选择AppGallery Connect。](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_kor.png)
2. 在[AppGallery Connect控制台](https://developer.huawei.com/consumer/en/service/josp/agc/index.html#/) 中进入My Apps菜单注册新应用程序或管理现有应用程序。
   ![AppGallery控制台主页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_main_eng.png)
   ![在MyApps菜单注册新应用程序。](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_01_eng.png)
3. 当NHN Cloud IAP与Huawei商店连接时仅支持安卓APK Package类型和移动设备。<br/>
   另外，由于应用程序最终必须链接到项目，第一次创建应用程序时，您必须事先创建项目，或者选择现有项目并将其连接于应用程序。
   ![Huawei新应用程序注册页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_02_eng.png)
4. 创建应用程序后应输入附加信息，并将可在华为AppGallery Connect控制台中查看的信息在NHN Cloud Gamebase控制台的应用程序设置中输入。
5. 本指南只包含在华为AppGallery Connect中注册的应用程序信息和与NHN Cloud Gamebase间连接应用程序信息时所需的信息。  <br/> 关于更加详细的华为AppGallery应用程序注册方法，请参考[Huawei指南文件](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/introduction-0000001050033062) 。

## 连接时所需的设定值
![NHN Cloud Gamebase Store Settings](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_iap_console_ko.png)
### Store App ID
- 在AppGallery Connect > MyProject > Project Settings页面上可查看与应用程序相连接的项目信息。
- 在NHN Cloud Gamebase控制台商店设置的“商店应用程序ID”中输入此页面的`App Information - Package Name`值。
    - 您还可以在AppGallery Connect>MyApp>App页面上查看此值。

![AppGallery Connect项目基本信息页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

### Huawei App ID & App Secret
- 在AppGallery Connect > MyProject > Project Settings页面上可查看与应用程序相连接的项目信息。 
- 在NHN Cloud Gamebase控制台商店设置的“Huawei App ID”中输入此页面的`App Information - OAuth 2.0 Client ID`值。
- 在NHN Cloud Gamebase控制台商店设置的“Huawei App Secret”中输入此页面的`App Information - OAuth 2.0 Client Secret`值。

![AppGallery Connect项目基本信息页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_06_eng.png)

### Huawei Developer Public Key
- 在AppGallery Connect > MyProject > Earn > In-App Purchases页面上可查看与IAP有关的信息。
- 当第一次进入此页面按启用IAP的按钮启用IAP时，可查看被创建的`Public Key`值，<br/>
  在NHN Cloud Gamebase控制台商店设置的“Huawei Developer Public Key”中输入此值。

![AppGallery Connect IAP页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/huawei_console_app_05_eng.png)