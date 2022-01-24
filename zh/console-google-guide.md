## Game > Gamebase > 商店控制台指南 > Google控制台指南

为了Google一般商品和订阅商品应用程序内支付，需要连接Google Play Billing。
当进行Google Play Billing时使用在Google Play Console和Google API Console中生成的值。 
为了Google订阅商品支付，还需要设置Notification。
若Notification设置不正确，则无法进行订阅支付。 

## Google Application Key

在IAP应用程序信息中注册以下信息。

| 字段 | 描述                                             |
| ---------------------------------- | ---------------------------------------------- |
| Google In App Purchase License Key | 在Google Play中注册的应用程序Public KEY(RSA)       |
| Google API Client ID               | Google API Project的OAuth Client ID            |    
| Google API Client Secret           | Google API Project的OAuth Client Secret        |
| Refresh Token For Google OAuth     | 通过Google Play Developer账户获取的Refresh Token | 
<center>[表1] 用于Google应用程序内支付的应用程序注册值</center>

 
## Google Console

| Console        | 位置                             |
| -------------- | ------------------------------- |
| Google Play Console | https://play.google.com/console/developers |
| Google API Console | https://console.developers.google.com/apis/dashboard |

## Google Play Console

### 查看Google In App Purchase License Key
```
Google Play Console > 选择应用程序 > 创出收益 > 设置创出收益 > 许可
```
![](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-google_license_kr.png)) 

## Google API Console

* Android Developer Guide
	* [Android Developers - 应用程序内支付管理](http://developer.android.com/google/play/billing/billing_admin.html)
	* [Android Developers - Authorization](https://developers.google.com/identity/protocols/OAuth2WebServer)

### 创建OAuth客户端
                                         
```
使用与Google Play Consle相同的账户，在Google API Console中创建项目。
参考以下图片，创建用于OAuth认证的3个信息。
1) Client ID  
2) Client Secret  
3) Refresh Token  
```   

##### 1. 创建OAuth客户端 (网络应用程序)

* https://console.developers.google.com/apis/credentials
![[图片1] 创建Client ID和Client Secret 1](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-oAuth_kr.png)


##### 2. 输入已认可的redirection url : “https://developers.google.com/oauthplayground”。
![[图片2] 创建Client ID和Client Secret 2](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-oAuth_2_kr.png)


##### 3. 创建之后从弹窗中复制客户端ID / 客户端seceret。
![[图片3] 创建Client ID和Client Secret 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_Oauth_clientSecret_ko.png)

##### 4. 输入客户端 ID和客户端Secret : [OAuth Playground](https://developers.google.com/oauthplayground/) > 设置右上方oauthplayground > 打钩选择“Use your own OAuth credentials使用”，输入复制的客户端ID和客户端Secret。
![[图片4] 创建Client ID和Client Secret 3](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_03.png)


##### 5. 发布Authorization code代码 : 在Step 1中输入https://www.googleapis.com/auth/androidpublisher后发布。
![[图片5] 创建Client ID和Client Secret 4](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_04.png)


##### 6. 发布令牌 : 在Step 2中点击Exchange authorization code for tokens按钮后发布。
![[图片6] 创建Client ID和Client Secret 5](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_g_05.png)


## 连接Google Play时的注意事项

* 您必须使用销售者账户注册商品。
* 创建OAuth认证信息后，请参考以下指南设置项目。

> [参考]
> Google指南 : https://developers.google.com/android-publisher/getting_started

#### 1. 查看Google Play Android Developer API的enable状态。

```
  - https://console.developers.google.com > APIs & Services > Dashboard
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-google-console-1.png)

#### 2. 在Google Play Developer Console中查看被连接的项目。

```
  - https://play.google.com/console/developers > Settings > Developer account > API access
```
![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-API_access_kr.png)

#### 3. 确认Google Play Developer Console中的Linked Project和GoogleAPIs的OAuth客户端创建的项目是否相同。 

![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-API_access_2_kr.png)


## 设置Google real-time developer notification

> [参考]
> 使用Google Cloud (https://cloud.google.com) Platform。 
> 在Google Cloud Platform中注册支付简介后(需要信用卡)更改为使用状态。 

### Google Cloud > 大数据 > 公布/订阅

在公布/订阅控制台(https://console.cloud.google.com/cloudpubsub)中进行以下程序。 

#### 创建Topic

```
1. 创建Topic后通过点击选项的权限或点击Topic名称来跳转到“Topic详细信息”网页。
2. 在Topic详细信息网页上点击“选择角色”选择公布/订阅发帖人。
3. 在“添加成员”中输入google-play-developer-notifications@system.gserviceaccount.com。
4. 点击添加按钮。
```
![[] 创建Topic](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-new-topic.png)
![[] 修改Topic](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_addMember_ko.png)


#### 创建Subscription
```
1. 点击Topic鼠标 > 新的订阅 
2. 传送类型
- 将传送类型选为Push的Endpoint URL。 
- Endpoint URL :  https://api-iap.cloud.toast.com/callback/subscription/{YOUR_PACKAGE_NAME}/GG
- {YOUR_PACKAGE_NAME} : google package name
```
![[] 创建Subscription](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_new_subscirption_ko.png)
![[] 创建Subscription](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_google_create_subscription_ko.png)

#### 验证IAP域名
```

1. 访问https://console.cloud.google.com/apis/credentials/domainverification。
2. 在[查看域名]页面上点击[添加域名]。
3. 输入https://api-iap.cloud.toast.com。
4. 点击[立即跳转]按钮跳转到网站管理员中心。
5. 点击网站管理员中心中的“添加属性”。
6. 在[添加属性]中输入https://api-iap.cloud.toast.com。  
7. 在NHN Cloud IAP CONSOLE App页面上注册Google应用程序时，在Domain authentication File Names中输入域名认证URL的html文件名。
    -> ex) 如果为https://api-iap.cloud.toast.com/googleabc.html，则输入googleabc.html。
8. 点击[推荐方法]下端的[不是机器人]后再点击[确认]。
9. 如果认证成功，将显示以下最后一页，而若不显示，则无法进行订阅支付。 
```
#### 1. 访问https://console.cloud.google.com/apis/credentials/domainverification。
#### 2. 在[查看域名]页面上点击[添加域名]。
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-1.png)
#### 3. 输入https://api-iap.cloud.toast.com。
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-2.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-3.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/google_domain_auth_gamebase.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-4.png)
![[] domain verification](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-domain-verification-5.png)

### 实时开发人员通知

* 设置完网站管理员后，回到应用程序设置网页，在Pub/Sub设置中输入“标题名称”。
````
https://play.google.com/console/developers > [选择应用程序] > 创出收益 > 设置创出收益 > 实时开发人员通知
````
![[]实时开发人员通知设置](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/2020-google_realtime_notification_kr.png)

