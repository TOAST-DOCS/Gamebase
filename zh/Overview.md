## Game > Gamebase > 概述

充满信心地为您推荐拥有游戏平台领先企业NHN的10年经验的Gamebase。
只要应用Gamebase SDK，即可轻松地使用游戏中所需的常规服务。

![Gamebase_summary](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_01_20210426_en.png)

## Gamebase Sample App

通过参考示例应用程序可掌握Gamebase的各种功能。
使用示例应用程序可尝试Gamebase在游戏应用程序中提供的功能，并提前掌握应用程序的运行方式。
通过使用以下下载页面上的示例应用程序代码，可轻松掌握适用Gamebase的方法。

* [下载页面](https://github.com/nhn/toast.gamebase.unity.sample/releases)
![Gamebase_sample_app](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_Sample_App1.png)
* 可通过使用QR代码下载Sample App APK。(支持平台 : Android OS)

## 主要功能

### Gamebase Analytics

只要应用Gamebase SDK，即免费提供销售、用户及游戏平衡指标。 
提供游戏中产生的销售、并发用户、用户、级别、道具销售等游戏事业与运营中必不可少的指标服务。 
请快速应用，并积极地运用于服务中！
![Gamebase_analytics](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_02_201903_en.png)

#### Reference

* [控制台使用指南 > Analytics](./oper-analytics) 

### 认证

Gamebase可以使用多种IDP(identity provider)账户ID，支持基于ID和密码的OAuth登录，游客使用其设备的UUID登录。
Gamebase认证服务不构建独立会员体系，而是利用外部IDP提供的会员信息提供认证服务。没有独立会员体系意味着不会将用户的ID，密码保存在Gamebase中。

* **通过单一接口提供多种认证方式。**
   通过提供具有单一接口的API可以降低开发成本，从而更容易，更快地开发其他外部IdP。 开发人员可以轻松实现身份验证，而无需考虑复杂的身份验证过程、法律问题或策略问题。


* **提供多种外部IDP认证**
提供的外部认证将会持续更新，如果您有想在游戏中使用的认证请联系【客户服务】。(https://toast.com/support/inquiry)

以下是Gamebase支持的外部认证列表。

| 外部认证             | Android | iOS | Windows(based Unity) |
| ----------------- | ------------ | ------------ | ------------ |
| Facebook          | O | O | O |
| Sign In with Apple | O  | O | |
| Apple Game Center |  | O | |
| Google            | O | O | O |
| PAYCO             | O | O | O |
| NAVER             | O | O | O |
| Twitter			| O | O | |
| LINE				| O | O | O  |
| Hangame			| O | O | O  |
| Weibo | O  | O  | |

* **提供游客登录。**
游客登录，玩家无需输入任何信息，即可直接登录并启动游戏。由于游客登录，是使用了Gamebase发放的ID，所以客户可以管理用户的游戏数据，游戏不论OAuth登录用户和游客登录用户，均可统一管理不用进行区分。


* **提供独立的会员识别符。**
    首次登录会自动生成Gamebase用户ID，在游戏中可识别用户，无论身份认证方式如何，都会向所有用户发放用户ID，并且不依赖IdP，因此，无论通过何种IdP登录，在游戏内均可使用相同的方式处理。


* **提供退出登录及退出游戏的功能。**
   退出登录后，您可以选择其他身份认证方式重新登录，如果退出游戏，则用户在Gamebase中的ID及其他所有相关信息都将会被删除。

* **提供映射（Mapping）功能，使一名游戏用户可以同时使用多个外部IDP。**
    例如，使用Facebook认证的游戏用户，也可以通过Google认证来使用相同用户ID。如果将Facebook和Gogle认证映射到一个游戏用户ID上，游戏用户就可在某个机器上使用Facebook认证，在其它机器上使用Google认证进行游戏。

#### 参考  

* [Android SDK 使用指南 > 认证](./aos-authentication)
* [iOS SDK 使用指南 > 认证](./ios-authentication)
* [Unity SDK 使用指南 > 认证](./unity-authentication)
* [Unreal SDK 使用指南 > 认证](./unreal-authentication)

### Payment

若游戏公司在多个商店推出已经打造的游戏，则可以通过较少的努力将利润最大化。Gamebase支持轻松地关联多个商店，因此不完全学会游戏中各主要商店的支付绑定配置也无妨。

如下为Gamebase支持的商店列表。

* Google Play Store
* App Store
* Galaxy Store
* ONE Store
* Facebook
* Amazon

* **通过单一接口提供多个商店的应用程序内支付。**
  通过单一接口提供API，可更轻松快速地进一步开发商店，因此节省了开发费用。开发者可以不学习复杂的支付绑定方法，轻松地实现支付功能。  
* **通过另外运用的支付验证服务器可确保支付安全及稳定性。**
  Gamebase中另外构筑用于验证与外部商店进行支付的服务器，更加稳定地提供从移动设备特性上来说可能不稳定的支付交易处理。考虑不稳定的网络状态，重新尝试支付并另外进行道具提供处理管理。
* **除购买单一道具，还提供订阅、Promotion功能。**
  提供谷歌商店和应用程序商店提供的订阅功能，可向用户销售月商品。游戏中无需另行实现即可轻松使用谷歌的促销功能。外部商店添加的功能今后也将在游戏库中作为添加功能提供。
* **利用网页控制台的各种功能（支付明细查询功能等）可顺利应对顾客咨询。**
  在网页控制台可确认用户的支付明细及道具提供状态，还可应对取消支付及滥用。

#### 参考

* [Android SDK 使用指南 > 结算](./aos-purchase)
* [iOS SDK 使用指南 > 结算](./ios-purchase)
* [Unity SDK 使用指南 > 结算](./unity-purchase)
* [Unreal SDK 使用指南 > 结算](./unreal-purchase)

### Launching

已上线的游戏APP，在移动终端上启动游戏时需要的各种信息，是由Gamebase提供，将之称为Launching。
Launching信息可以在Gamebase Console实时设定，SDK的初始化或Launching状态变更时可以在游戏中确认。

在Gamebase上提供的Launching信息如下。

* APP状态信息
	* 游戏客户端是否需要更新、下载URL
	* 维护信息
* 紧急公告信息
* 认证信息
* 游戏内URL清单

#### 参考

* [Android SDK 使用指南 > 初始化 > Launching Status](./aos-initialization/#launching-status)
* [iOS SDK 使用指南 > 初始化 > Launching Status](./ios-initialization/#launching-status)
* [Unity SDK 使用指南 > 初始化 > Launching Information](./unity-initialization/#launching-information)
* [Unreal SDK 使用指南 > 初始化 > Launching Status](./unreal-initialization/#launching-status)
* [控制台使用指南> APP](./oper-app)：设置APP、客户端状态和安装URL
* [控制台使用指南> 运营](./oper-operation)：维护、登记公告

### For Global

Gamebase基本上支持全球游戏，并提供以下功能：

* **为游戏用户提供多国语言服务。**
	* 在Console中输入游戏用户所显示的信息时,以多语种输入，会根据用户设备的语言设置显示。如果在Console中输入韩语，英语，日语，则使用韩语设备的用户将显示韩语信息。
* **提供国家过滤功能。**
	* 运营中，如果想给特定国家的游戏用户发送公告或推送消息，可以指定国家并显示信息。
* **运营者可以选择当地标时区(local timezone)，轻松输入时间。**
	* 在越南运营游戏时，可以选择越南时区(timezone)，以越南的时间为准输入，可以省去变更为韩国时间的步骤。

### 其他NHN Cloud服务

* 可以轻松的联动游戏中所需的NHN Cloud服务。
  * Gamebase根据Gamebase用户ID提供封装(wrapping)的API。 因此，用户无需对每个服务的API进行单独调用。
  * [通知 > 推送](https://toast.com/service/notification/push) ：发送推送消息的综合通知服务
  * [游戏 > 排行榜](https://toast.com/service/game/leaderboard) ：大容量数据的实时排名服务
  * [安全 > AppGuard](https://toast.com/service/security/appguard) ：实时防止应用程序代码被篡改的服务

## 术语表
以下是Gamebase服务术语列表。

| 术语      | 说明                                              |
| --------- |    ----------------------------------------       |
| 游戏用户名| Gamebase中的用户标识符                     |
| 设备秘钥  | 设备标识符(iOS:IDFV, Android:Android ID)            |
| UUID      | Guest用户创建时生成（使用）的终端识别符，应用删除前一直有效。   |
| IdP       | 身份认证的提供者（Identify Provder），例如：Facebook, Gogle, APPLE Game Center, PAYCO等 |
| IdP 令牌 | 身份验证后从IdP SDK收到的访问令牌（access token）         |
| IdP 登录 | 外部IdP登录(Facebook, Google 等)                  |

## 服务架构

以下是Gamebase的服务架构图和简介。
![逻辑架构图](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_03_202203_en.png)

| 组件名称        | 说明                                       |
| --------------- | ---------------------------------------- |
| Gamebase SDK    | - 用于客户端开发的SDK                       |
| Gamebase Server | - 在内部/外部模块之间提供mashup API，并处理内部逻辑<br>- 在客户端启动时提供数据 <br>- 管理发放用户标识符，并管理映射<br>-收集与管理各游戏的同时在线指标|
| Console         | - Web控制台                              |

## 平台指南

### 客户端开发者指南

* [Android SDK 使用指南](./aos-started/)
* [iOS SDK 使用指南](./ios-started/)
* [Unity SDK 使用指南](./unity-started/)
* [Unreal SDK 使用指南](./unreal-started/)
 
### 服务器端开发者指南

* [API 指南](./api-guide/)

### 管理员指南

* [控制台使用指南](./oper-analytics/)

## 功能指南

| 功能               | 描述                              | 客户端                                   | 服务器端                                   | 控制台                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Analytics                  | 实时指标, 销售指标, 用户指标, 平衡指标 | [[Android](./aos-etc/#analytics)] [[iOS](./ios-etc/#analytics)] [[Unity](./unity-etc/#analytics)] |                                          | [[Analytics]](./oper-analytics)  ||
| Login                 | 游客，支持第三方认证  <br> - [支持的 IdP](./Overview/#authentication) |  [[Android](./aos-authentication/#login)] [[iOS](./ios-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[令牌验证](./api-guide/#token-authentication)] <br> [[查询用户](./api-guide/#get-member)] | [[App] > 设置认证信息](./oper-app/#authentication-information) <br> [[Member] >查询用户 ](./oper-member/#member) <br> - 基本信息，登录历史，在线时间，付款记录等|
| Logout                | 退出登录                                 | [[Android](./aos-authentication/#logout)]  [[iOS](./ios-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | 退出游戏 <br> -  删除游戏用户的用户ID，浏览记录等所有信息 | [[Android](./aos-authentication/#withdraw)] [[iOS](./ios-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | 一个游戏用户ID映射多个IdP的功能        | [[Android](./aos-authentication/#mapping)] [[iOS](./ios-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Purchase(IAP)         | 应用内结算| [[Android](./aos-purchase/#purchase)] [[iOS](./ios-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./api-guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- 注册商品](./oper-purchase/#item) <br> [- 查询结算信息](./oper-purchase/#transactions) |
| 推送                  | (NHN Cloud 连动服务) <br> 推送消息及确认结果 | [[Android](./aos-push/#push)] [[iOS](./ios-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- 实时、预约推送消息 |
| 排行榜           | 查询及注册大容量数据的实时排名 |                                          | [[Wrapping API](./api-guide/#leaderboard)] |                                          |
| Webview               |SDK提供默认的WebView UI<br/>提供系统弹出窗口和TOAST UI | [[Android](./aos-ui/#webview)] [[iOS](./ios-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [管理员] Maintenance | (操作)维护功能                               |                                          | [[确认维护](./api-guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- 登记或取消维护 |
| [管理员] Notice      | (操作) 紧急公告功能 <br> -  游戏用户运行应用时，可以以弹出窗口的形式查看公告的功能 |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-登记公告 |
| [管理员] Image Notice         | (运营)图片通知功能 <br> -  在游戏内显示弹窗形式的图片通知 | [[Android](./aos-ui/#imagenotice)] [[iOS](./ios-ui/#imagenotice)] [[Unity](./unity-ui/#imagenotice)] <br/> - 显示图片通知 |                             | [[Image Notice]](./oper-operation/#image-notice) <br/>- 管理图片通知 |
| [管理员] Ban         | (操作) 游戏用户的登记和解除禁用状态<br> - 游戏用户的登记和解除禁用状态 | [[Android](./aos-authentication/#get-banned-user-information)] [[iOS](./ios-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -确认禁用用户信息 |   [[查询用户禁用历史记录](./api-guide/#_6)]                                       | [[Ban]](./oper-ban/#ban) <br/>-登记和解除禁用状态 |
| [管理员] Coupon         | (运营)管理优惠券<br>- 查看发布历史 |  |                                      [[验证优惠券有效性/更改优惠券状态](./api-guide/#coupon)  | [[Coupon]](./oper-coupon) <br/>- 发布优惠券 |
| [管理员] Customer Service         | (运营)接收并处理1:1咨询 <br> -  管理FAQ/公告 | [[Android](./aos-etc/#contact)] [[iOS](./ios-etc/#contact)] [[Unity](./unity-etc/#contact)] <br/> - 通过Webview显示客户服务网页 |                                        | [[Customer Service]](./oper-customer-service) <br/>- 处理客户服务咨询<br>- 管理FAQ/公告 |

## Console Role

关于TOAST的基本成员政策和权限，请参考以下指南。
* [TOAST > 控制台使用指南 > 管理成员](https://docs.toast.com/zh/TOAST/zh/console-guide/#_14)

### Manage Role

**Console > 设置项目 > 管理成员**  
可以在”设置项目”页面上添加TOAST成员或给注册的成员授予权限。可以给一个成员授予多个权限。    
![项目权限](http://static.toastoven.net/prod_gamebase/Overview/overview_project_role_01_20201123.png)

**Console > 设置项目 > 管理权限组**
为了运营上的方便，可以将经常使用的权限注册为*权限组*，以权限组为单位授予TOAST成员权限 。
![项目权限组](http://static.toastoven.net/prod_gamebase/Overview/overview_project_role_02_20201123.png)

**Console > 设置组织 > 设置项目共同权限组**
在”组织管理页面”上可对组织内的项目共同使用的权限组进行管理。 
![项目权限组](http://static.toastoven.net/prod_gamebase/Overview/overview_company_role_01_20201123.png)

### Gamebase提供的权限列表

| 服务 | 权限 | 描述 |
| --- | --- | --- |
| Gamebase | ADMIN | **访问并控制全屏**<br>Gamebase服务 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | ANALYTICS VIEWER - ALL | 所有指标 Read(读取)<br>可以下载指标结果的Excel文件。|
| Gamebase | ANALYTICS VIEWER - EXCLUDING SALES | 除销售以外的所有指标 Read(读取) |
| Gamebase | ANALYTICS VIEWER - ONLY REAL-TIME | 实时指标 Read(读取) |
| Gamebase | APP ADMIN | APP菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | APP VIEWER | APP菜单 Read(读取) |
| Gamebase | BAN ADMIN | 禁止使用菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | BAN VIEWER | 禁止使用菜单 Read(读取) |
| Gamebase | COUPON ADMIN | 优惠券菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | COUPON VIEWER | 优惠券菜单 Read(读取) |
| Gamebase | CS ADMIN | 客户服务菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | CS INQUIRY SUPPORT | 客户服务咨询菜单 Read(读取)、Update(修改)及成员菜单 Read(读取) |
| Gamebase | IAP ADMIN | 购买菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | IAP VIEWER | 购买菜单 Read(读取) |
| Gamebase | LEADERBOARD ADMIN | Leaderboard菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | LEADERBOARD VIEWER | Leaderboard菜单 Read(读取) |
| Gamebase | MANAGEMENT ADMIN | 管理菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | MEMBER ADMIN | 成员菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | MEMBER VIEWER | 成员菜单 Read(读取) |
| Gamebase | MEMBER FILE DOWNLOAD | 成员菜单 Read(读取)及成员文件下载 |
| Gamebase | OPERATION ADMIN | 运营菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | OPERATION VIEWER | 运营菜单 Read(读取) |
| Gamebase | PUSH ADMIN | 推送菜单 Create(创建)、Read(读取)、Update(修改)、Delete(删除) |
| Gamebase | PUSH VIEWER | 推送菜单 Read(读取) |


* 是通过创建项目中经常使用的权限组来管理权限的示例。如果在游戏中需要，可通过创建适当的权限组来进行管理。 

| 服务 | 权限 | 管理员/事业 | 开发 | CS |
| --- | --- | --- | --- | --- | 
| Gamebase | ADMIN | ● | | |
| Gamebase | ANALYTICS VIEWER - ALL |  |  | |
| Gamebase | ANALYTICS VIEWER - EXCLUDING SALES |   |  | |
| Gamebase | ANALYTICS VIEWER - ONLY REAL-TIME |  | ● | |
| Gamebase | APP ADMIN | |  ● | |
| Gamebase | APP VIEWER | |  | |
| Gamebase | BAN ADMIN | |  ● | ●|
| Gamebase | BAN VIEWER | |  | |
| Gamebase | COUPON ADMIN | | ● | |
| Gamebase | COUPON VIEWER | |  |● |
| Gamebase | CS ADMIN | |  | |
| Gamebase | CS INQUIRY SUPPORT | |  | ● |
| Gamebase | IAP ADMIN | | ● | |
| Gamebase | IAP VIEWER | |  |● |
| Gamebase | LEADERBOARD ADMIN || ● | | 
| Gamebase | LEADERBOARD VIEWER | |  | |
| Gamebase | MANAGEMENT ADMIN | | ● | |
| Gamebase | MEMBER ADMIN | | ● | ● |
| Gamebase | MEMBER VIEWER | |  |  |
| Gamebase | MEMBER FILE DOWNLOAD | |  | |
| Gamebase | OPERATION ADMIN | | ● | |
| Gamebase | OPERATION VIEWER | |  |● |
| Gamebase | PUSH ADMIN | | ● | |
| Gamebase | PUSH VIEWER | |  | ● |