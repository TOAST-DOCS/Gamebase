## Game > Gamebase > 商店控制台指南 > Amazon Appstore控制台指南

## Amazon Developer Console
1. 在[Amazon开发人员控制台](https://developer.amazon.com/) 中注册账户后在亚马孙App Store管理菜单中创建应用程序。
   ![Amazon开发人员控制台](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_developer_console_eng.png)
2. NHN Cloud IAP只支持安卓平台的亚马孙App Store应用程序。选择安卓平台输入应用程序名称后，通过单机 `Create app` 按钮创建应用程序。
   ![选择应用程序平台](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_0_eng.png)
3. 创建应用程序后，如下所示，您可以查看创建的应用程序列表。
   ![应用程序开发人员控制台的应用程序列表](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_1_eng.png)
4. 创建应用程序后将开发人员控制台提供的信息在NHN Cloud IAP控制台的应用程序设置中输入。
5. 本指南仅涵盖在亚马逊应用程序商店中注册的应用程序信息和与NHN Cloud IAP链接应用程序信息指南 。 <br/> 有关在亚马孙商店注册应用程序的更多详细信息，请参考[Amazon指南文件](https://developer.amazon.com/apps-and-games/documentation)。

## 连接时所需的设定值
![NHN Cloud Gamebase商店的设置页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_iap_console_ko.png)
### Store App ID
- 单机在[应用程序列表](https://developer.amazon.com/apps-and-games/console/apps/list.html) 页面上创建的应用程序的名称，进入详细设置菜单。
- 在详细设置菜单中通过点击编辑输入“App SKU”后，将其App SKU的值在 [NHN Cloud Gamebase控制台 > 商店]中商店信息注册页面的“商店应用程序ID”中输入。
  ![Amazon开发人员控制台的应用程序详细设置页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_2_eng.png)


### Amazon Shared Key
- 进入Amazon开发人员控制台的 [Settings -> Identity菜单](https://developer.amazon.com/settings/console/sdk/shared-key) 时可以在以下页面查看共享密钥。
  ![Amazon开发人员控制台 Identity页面](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/amazon_appmenu_3_eng.png)
- 您要在NHN Cloud Gamebase控制台应用程序设置中的“亚马逊共享密钥”项中输入此值。
 