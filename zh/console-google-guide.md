## Game > Gamebase > 商店控制台指南 > Google控制台指南

> [公告]
> 本文档介绍如何将Google Play上发布的应用程序信息注册并连接于[Gamebase IAP](https://docs.toast.com/ko/Game/Gamebase/ko/oper-purchase/)控制台。
> 关于在Google Play上推出应用程序的控制台设置方法，请参考Google提供的Google Play Console指南。 

## 链接Google Cloud项目  
- 为了将Google Play上注册的应用程序和相关信息连接于Gamebase IAP，你需要一个将连接到Google play的Google Cloud项目。
- 移动至Google Play Console的**设置** > **API访问**页面。
    - 通过选择“创建新Google Cloud项目”或“链接现有的项目”来连接Google Play和Google Cloud。
    - 如果已有被链接的Google Cloud项目，将显示被连接的Google Cloud项目状态。
    - 还可以在[Google Cloud Console主页面](https://console.cloud.google.com/home/dashboard)中提前创建项目后链接现有项目。
    
![链接Google Cloud项目](https://static.toastoven.net/prod_iap/console_google/google_common_step_01.png)   
   
- 当Google Play和Google Cloud之间的项目连接成功建立时，将显示连接到Google Play Console**设置** > **API访问**页面的Google Cloud项目和API列表。
- 如果按照以下指南完成了设置，但已连接项目的状态与以下页面上显示的状态不同，您先要确认是否链接到Google Cloud。
                                      
![链接Google Cloud项目](https://static.toastoven.net/prod_iap/console_google/google_common_step_03.png)

- 创建新项目后，需要配置OAuth同意屏幕以设置集成验证。
    - 关于**配置OAuth同意屏幕**的方法，请参考页面中的指南和[Google提供的OAuth2指南](https://developers.google.com/identity/protocols/oauth2/)。

![链接Google Cloud项目](https://static.toastoven.net/prod_iap/console_google/google_common_step_02.png)


## 两种集成验证方式
                                                           
![设置NHN Cloud IAP应用程序](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_zh_202210.png)

- 为了连接到Google，需要进行访问Google Android开发者API验证。 
- NHN Cloud IAP提供两种验证模式，为了验证每个模式需要不同的特定信息。 
- 特定信息之外，为了确认与Google的连接和支付，需要输入共同信息，因此还需要**确认共同信息**。
    - “Store App ID” : 请参考共同信息指南Package Name。
    - “Google In App Purchase License Key” : 请参考共同信息指南InAppPurchase License Public Key。
    - “省略商店连接验证” : 请参考共同信息指南/省略商店连接。
- 按照谷歌提供的指南中的以下步骤，可通过[Google Cloud Platform Console](https://console.cloud.google.com/apis/dashboard)、[Google Play Console](https://play.google.com/console/developers)及[Google Developer Console](https://developers.google.com/oauthplayground/)获取特定信息和共同信息。
- 用于确认Google Play应用程序付款信息的[Google Android Publisher API](https://developers.google.com/android-publisher)是OAuth2所需的验证目标API。


## 高级管理员(Supervisor)验证模式  
- 是管理要在Google控制台中注册的应用程序的，取代Google账户验证的模式，也是NHN Cloud IAP作为默认值提供的验证模式。
- 在Google Console中可使用一个Google账户来注册并管理多个应用程序，而同一账号下注册的应用程序的NHN Cloud IA设置信息均相同。
- 按照以下步骤确认3种信息后，并将其作为NHN Cloud IAP应用程序信息输入。它将用于相应Google帐户的OAuth验证。

1. 输入特定于NHN Cloud IAP Google高级管理员验证模式的信息。
    - “Google API Client ID” : 请参考高级管理员验证模式指南中的步骤4。
    - “Google API Client Secret” : 请参考高级管理员验证模式指南中的步骤4。
    - “Refresh Token For Google Oauth” : 请参考高级管理员验证模式指南中的步骤6。
      ![设置高级管理员模式](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_supervisor_zh_202210.png)


2. 移动至[Google Cloud Console API和服务](https://console.cloud.google.com/apis/dashboard)仪表盘。
    - 选择项目以链接到Google Play管理员帐户。
    - 移动至**用户验证信息**菜单。
    - **创建用户验证信息** > 选择**OAuth客户端ID**

   ![设置高级管理员模式](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_01.png)

3. 输入新的OAuth Client基本信息。
    - 应用程序类型 : 指定网络应用程序。
    - 名称 : 为管理员设置名称，以将其与NHN Cloud IAP的名称区分开来。
    - 授权的JavaScript来源：跳过
    - 授权的Redirection URI : 添加后输入“https://developers.google.com/oauthplayground”。

   ![设置高级管理员模式](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_02.png)

4. 创建新的OAuth Client时，将显示一个弹窗，其中包含有关所创建的OAuth客户端的两种信息。
    - 客户端ID : 在NHN Cloud IAP应用程序信息设置页面上输入作为“Google API Client ID”的值。
    - 客户端密钥 : 在NHN Cloud IAP应用程序信息设置页面上输入作为“Google API Client Secret”的值 。
    - 可以在OAuth客户端信息页面上再次查看上述两种信息，也可以在Google提供的OAuth Client信息JSON下载文件中查看信息。

   ![设置高级管理员模式](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_03.png)

5. 移动至[Google OAuth 2 Playground](https://developers.google.com/oauthplayground)页面。
    - 在按下屏幕右上角的齿轮状按钮时出现的**OAuth 2.0 Configuration**弹窗设置中，选中“Use your own OAuth credentials”复选框。
    - 在OAuth 2.0 Configuration弹窗设置中，分别在Client ID和Client Secret中输入步骤4中获得的信息。
    - 在左侧“选择API授权范围”页面上选择“Google Play Android Developer API”。
        - 您也可以在左侧选择屏幕底部的范围输入框中输入“https://www.googleapis.com/auth/androidpublisher”。
    - 输入并选择所有设置值后，单击左下角的Authorize API按钮。 

   ![设置高级管理员模式](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_04.png)

6. 单机Authorize API按钮时Google OAuth 2 Playground页面将转换为Step 2并显示附加信息。
    - Refresh Token : 在NHN Cloud IAP应用程序信息设置页面上输入“Refresh Token For Google Oauth”。

   ![设置高级管理员模式](https://static.toastoven.net/prod_iap/console_google/google_supervisor_step_05.png)

## 服务账号验证模式      
- 此模式是由高级管理员帐户委派权限的，是取代Google服务帐户验证的模式。该模式于2022年4月被添加到NHN Cloud IAP。  
- 高级管理员(Google的实际管理账户)在Google控制台中创建并管理被授予权限范围的服务账户。
- 高级管理员可以向服务帐户授予与高级管理员相同的权限范围，也可以授予仅限于高级管理员具有权限的特定应用程序的权限范围。
    - 委托权限范围的配置策略取决于客户所需的特性。
- 如果您觉得高级管理员验证模式设置的权限范围过大，可以在Google控制台中创建服务帐户后使用此验证模式。([Google指南](https://developers.google.com/identity/protocols/oauth2/service-account))

1. 输入NHN Cloud IAP服务账户验证模式的特定信息。
    - “服务账户集成信息” : 请参考服务账户验证模式指南中的步骤5。   
      ![设置服务账户模式](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/gamebase_google_iap_console_service_account_zh_202210.png)

2. 移动至[Google Cloud Console](https://console.cloud.google.com/apis/dashboard)API和服务页面。
    - 选择将与Google Play管理账户连接的项目。 
    - 移动至**用户验证信息**菜单。
    - **创建用户验证信息** > 选择**服务账户**

   ![设置服务账户模式](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_02.png)

3. 输入新服务账户基本信息。
    - 以易于管理的格式输入基本信息，然后单击“创建并继续”。
    - 在“给服务帐户授予访问权限”中，将角色选为“所有者”。   
    - 在附加选项中输入要创建的服务帐户的管理员电子邮件，该管理员将被授予设置服务帐户的权限。
        - 如果是Google Cloud Console控制台中不存在的管理员邮件地址，将向您输入的邮件地址发送邀请为管理员的电子邮件。
          
   ![设置服务账户模式](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_03.png)

4. 将权限委派给服务帐户。                   
    - 移动至**Google Play Console** > **API访问**菜单。   
    - 在Google Cloud Console中创建的服务帐户将在屏幕底部的服务帐户列表中显示。单击创建的服务帐户右侧的**授予权限**链接。
    - 可按高级管理员帐户拥有的所有应用程序或按各别应用程序单位多次指定权限范围，并可在该范围内指定权限类型。
    - 按客户所需的特性设置范围，但在权限类型中，必须将“查看应用程序信息（只读）”、“查看财务数据”以及“管理订单和订阅”的权限设置为必须。
    - 为了反映委派的权限需要一段时间。在某些情况下，可能需要7天才能反映委派的权限。

   ![设置服务账户模式](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_05.png)

5. 转到API和服务页面上新创建的服务帐户列表中创建的服务帐户的详细信息页面。
    - 移动至密钥键后添加密钥。 > 创建新的密钥。
    - 在新弹窗中选择“JSON”格式后点击创建。
    - 密钥创建完成后，会自动从Google下载一个JSON格式的文件。          
    - 您需要在纯文本编辑器（如Windows记事本）上加载文件，复制整个内容，然后在NHN Cloud IAP应用程序信息设置页面上的“服务账户集成信息”中输入它。
    - 第一次下载后，无法再次下载文件，因此要保存文件输入信息。
    - 如步骤4所述，如果Google尚未反映该权限，则可能会出现“未经授权的注册”错误，因此要注意。


   ![设置服务账户模式](https://static.toastoven.net/prod_iap/console_google/google_service_account_step_04.png)


## 共同信息
### Package Name   
- 在构建通过Google Play Console注册的应用程序时指定了packageName。此值在Google中被用作应用程序的唯一标识符。
- 在NHN Cloud IAP App Settings中输入此值作为“Store App ID”。

### InAppPurchase License Public Key
- 进入**Google Play Console** > 相关应用程序仪表盘 > **创出收入** 菜单。
- 在屏幕底部，您将看到Base64编码的许可密钥。在NHN Cloud IAP应用程序设置中输入此值作为“Google In-App Purchase”。

![IAP许可密钥](https://static.toastoven.net/prod_iap/console_google/google_iap_license_key.png)

### 省略商店验证                     
- NHN Cloud IAP提供的Google支付验证分为两种类型。其中**设置是否省略步骤2**。
- 不必要必须省略，为了应对偶尔发生的Google服务器故障，可能会维持临时付款许可状态。（默认值为“NO”。）
    - 省略此步骤并不表示传到的付款验证请求始终被处理为正常付款。
    - 省略选项不适用于订阅付款的验证。

#### Google验证步骤
| 步骤 | 描述           |
| --------------- | ----------------------------- |                
| 步骤1 | 确认请求验证的付款信息是否是伪造的。         |   
| 步骤2 | 确认并重新验证请求验证的付款信息的Google服务器的最新状态。   |


## 在Google系统中设置实时订阅信息事件传播。 
- 可以将NHN Cloud IAP服务器设置为接收和处理Google提供的订阅购买的实时状态传播事件的状态。
- 在Google Cloud Platform中注册付款账号(需要信用卡)后将状态更改为使用状态。
- 如果不在Google中设置此传播事件，订阅付款的续订信息将根据用户应用程序客户端的LaunchAction反映出来，因此建议您在使用订阅付款时进行设置。
- 以前，将接收的地址的域名验证必须通过Google网站管理员工具来完成，以接收“订阅实时传播事件”的推送通知，但目前不需要域名验证。

1. 移动到[Google Cloud Pub/Sub Console](https://console.cloud.google.com/cloudpubsub)。
    - 您需要正确选择与Google Play应用程序连接的项目。
    - 在**主题(Topic)**菜单中创建新主题。
    - 主题ID可以是易于管理的任何名称。

   ![设置订阅信息传播](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_01.png)

2. 在创建的主题**添加主要成员**。
    - 输入成员信息“google-play-developer-notifications@system.gserviceaccount.com”。
    - 选择角色"发布/订阅" > "发布/订阅发布者"。

   ![设置订阅信息传播](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_02.png)
   ![设置订阅信息传播](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_03.png)

3. 移动至**订阅**菜单，创建将通知IAP服务器的订阅。
    - 在订阅ID中输入易于管理的值。
    - 选择Cloud Pub/Sub主题 : 选择以前创建的主题。 
    - 传送类型 : 选择推送
    - EndPoint URL : 输入“https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG” 
    - 在“{YOUR_PACKAGE}”中输入与NHN Cloud App设置的StoreAppID相同的值（构建应用程序时使用的PACKAGE名称）。

   ![设置订阅信息传播](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_04.png)  

4. 如果使用用于测试的Alpha环境/Gamebase Sandbox环境，则需为Alpha/Sandbox分别创建步骤3的订阅。
    - 输入Alpha EndPoint URL : “https://alpha-api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG”。
    - 输入Gamebase Sandbox EndPoint URL : “https://sandbox-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG”。
5. 移动到[Google Play Console](https://play.google.com/console)的应用程序仪表盘。
    - 在**设置创出收益** > **Google Play支付**页面的“实时开发人员通知主题名称”中输入您在步骤1创建的主题的全称。 

   ![设置订阅信息传播](https://static.toastoven.net/prod_iap/console_google/google_subscription_event_05.png)
