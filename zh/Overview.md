## Game > Gamebase > 概述

充满信心地为您推荐拥有游戏平台领先企业NHN的10年经验的Gamebase。 
只要应用Gamebase SDK，即可轻松地使用游戏中所需的常规服务。 

![Gamebase_summary](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_01_201903_en.png)

## 主要功能

### 认证

Gamebase可以使用多种IDP(identity provider)账户ID，支持基于ID和密码的OAuth登录，游客使用其设备的UUID登录。
Gamebase认证服务不构建独立会员体系，而是利用外部IDP提供的会员信息提供认证服务。没有独立会员体系意味着不会将用户的ID，密码保存在Gamebase中。<br/>

* **通过单一接口提供多种认证方式。**
   通过提供具有单一接口的API可以降低开发成本，从而更容易，更快地开发其他外部IdP。 开发人员可以轻松实现身份验证，而无需考虑复杂的身份验证过程、法律问题或策略问题。

* **提供多种外部IDP认证**
提供的外部认证将会持续更新，如果您有想在游戏中使用的认证请联系【客服中心】。(https://toast.com/support/inquiry)

以下是Gamebase支持的外部认证列表。

| 外部认证          | Android | iOS | Windows(based Unity) | Web(based JavaScript)    |
| ----------------- | ------------ | ------------ | ------------ | ------------ |
| Facebook          | O | O | O | O |
| Apple Game Center | O | | | |
| Google            | O | O | O | O |
| PAYCO             | O | O | O | O |
| NAVER             | O | O | O | O |
| Twitter			| O | O | |  |
| LINE				| O | O |  |  |

* **提供游客登录。**
游客登录，玩家无需输入任何信息，即可直接登录并启动游戏。由于游客登录，是使用了Gamebase发放的ID，所以客户可以管理用户的游戏数据，游戏不论OAuth登录用户和游客登录用户，均可统一管理不用进行区分。

* **提供独立的会员识别符。**
    首次登录会自动生成Gamebase用户ID，在游戏中可识别用户，无论身份认证方式如何，都会向所有用户发放用户ID，并且不依赖IdP，因此，无论通过何种IdP登录，在游戏内均可使用相同的方式处理。

* **提供退出登录及退出游戏的功能。**
   退出登录后，您可以选择其他身份认证方式重新登录，如果退出游戏，则用户在Gamebase中的ID及其他所有相关信息都将会被删除。

* **提供映射（Mapping）功能，使一名游戏用户可以同时使用多个外部IDP。**
    例如，使用Facebook认证的游戏用户，也可以通过Google认证来使用相同用户ID。如果将Facebook和Gogle认证映射到一个游戏用户ID上，游戏用户就可在某个机器上使用Facebook认证,在其它机器上使用Google认证进行游戏。

#### 参考

* [Android SDK 使用指南 > 认证](./aos-authentication)
* [iOS SDK 使用指南 > 认证](./ios-authentication)
* [Unity SDK 使用指南 > 认证](./unity-authentication)

### Gamebase Analytics

只要应用Gamebase SDK，即免费提供销售、用户、游戏平衡指标。 
提供游戏中产生的销售、并发用户、用户、级别、道具销售等游戏事业与运营中必不可少的指标服务。 
请快速应用，并积极地运用于服务中！
![Gamebase_analytics](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_02_201903_en.png)

#### Reference

* [控制台使用指南 > Analytics](./oper-analytics) 

### Launching

已上线的游戏APP，在移动终端上启动游戏时需要的各种信息，是由Gamebase提供，将之称为Launching。
Launching信息可以在 Gamebase Console实时设定，SDK的初始化或 Launching状态变更时可以在游戏中确认。

在Gamebase上提供的Launching信息如下

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

### Standard Index(BI)

* Gamebase Console实时提供基本运营指标。
	* CCU(concurrent connected users)：同时在线用户数
	* MCU(maximum concurrent users)：最大同时在线用户数(可按实时，日期查询)
	* DAU(daily active users)：日活跃用户数(可按实时，日期查询)
	* NRU(newly registered uesrs)：新注册用户数(可按实时，日期查询)
* 占有率图表：通过游戏用户的操作系统，国家，游戏客户端版本提供游戏用户占有率的图表
	* 同时在线趋势图表：一天的变化量以图表形式显示，维护和推送消息导致的变化，在图表上也可一目了然
* 提供组指标，可一目了然地查看有权限的多种指标。
* 提供安装URL统计，按日期，浏览器(Internet Exploer, Chroome等)，平台(Windows, Android等)分类的数量和百分率。
* 提供销售现状画面，可查看游戏的销售统计信息。
	* 提供月，日，各商店总销售额
	* 可变更为所需货币(KRW, USD 等)

#### 参考

* [控制台使用指南 > 运营指标](./oper-operating-indicator) 

### 其他TOAST服务

* 可以轻松的联动游戏中所需的TOAST服务。
  * Gamebase根据Gamebase用户ID提供封装(wrapping)的API。 因此，用户无需对每个服务的API进行单独调用。
  * [通知 > 推送](https://toast.com/service/notification/push) ：发送推送消息的综合通知服务
  * [移动服务 > IAP](https://toast.com/service/mobile_service/iap) ：综合移动应用程序内结算服务
  * [游戏 > 排行榜](https://toast.com/service/game/leaderboard) ：大容量数据的实时排名服务
  * [安全 > AppGuard](https://toast.com/service/security/appguard) ：实时防止应用程序代码被篡改的服务

## 术语表
以下是Gamebase服务术语列表。

| 术语      | 说明                                              |
| --------- |    ----------------------------------------       |
| 游戏用户名| Gamebase中的用户标识符                     |
| 设备秘钥  | 设备标识符(iOS:IDFV, Android:Android ID)            |
| UUID      | Guest用户创建时生成（使用）的终端识别符，应用删除前一直有效   |
| IdP       | 身份认证的提供者（Identify Provder），例如：Facebook, Gogle, APPLE Game Center, PAYCO等 |
| IdP 令牌 | 身份验证后从IdP SDK收到的访问令牌（access token）         |
| IdP 登录 | 外部IdP登录(Facebook, Google 等)                  |

<br/>

## 服务架构
以下是Gamebase的服务架构图和简介
![逻辑架构图](http://static.toastoven.net/prod_gamebase/Overview/Gamebase_overview_03_201903_en.png)
<br>

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

### 服务器端开发者指南

* [API 指南](./api-guide/)

### 管理员指南

* [控制台使用指南](./oper-operating-indicator/)

<br/>
## 功能指南

| 功能               | 描述                              | 客户端                                   | 服务器端                                   | 控制台                                  |
| --------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Login                 | 游客，支持第三方认证  <br> - 支持的 IdP：Facebook, Google, Apple Game Center, PAYCO |  [[Android](./aos-authentication/#login)] [[iOS](./ios-authentication/#login)] [[Unity](./unity-authentication/#login)] | [[令牌验证](./api-guide/#token-authentication)] <br> [[查询用户](./api-guide/#get-member)] | [[App] > 设置认证信息](./oper-app/#authentication-information) <br> [[Member] >查询用户 ](./oper-member/#member) <br> - 基本信息，登录历史，在线时间，付款记录等|
| Logout                | 退出登录                                 | [[Android](./aos-authentication/#logout)]  [[iOS](./ios-authentication/#logout)] [[Unity](./unity-authentication/#logout)] |                                          |                                          |
| Withdraw              | 退出游戏 <br> -  删除游戏用户的用户ID，浏览记录等所有信息 | [[Android](./aos-authentication/#withdraw)] [[iOS](./ios-authentication/#withdraw)] [[Unity](./unity-authentication/#withdraw)] |                                          |                                          |
| Mapping               | 一个游戏用户ID映射多个IdP的功能        | [[Android](./aos-authentication/#mapping)] [[iOS](./ios-authentication/#mapping)] [[Unity](./unity-authentication/#mapping)] |                                          |                                          |
| Analytics                  | 实时指标, 销售指标, 用户指标, 平衡指标 | [[Android](./aos-etc/#analytics)] [[iOS](./ios-etc/#analytics)] [[Unity](./unity-etc/#analytics)] |                                          | [[Analytics]](./oper-analytics)  ||
| Purchase(IAP)         | (TOAST联动服务) <br> 应用内结算 <br> - 支持商店：Google, App Store | [[Android](./aos-purchase/#purchase)] [[iOS](./ios-purchase/#purchase)] [[Unity](./unity-purchase/#purchase)] | [[Wrapping API](./api-guide/#purchaseiap)] | [[Purchase]](./oper-purchase/#app)<br> [- 注册商品](./oper-purchase/#item) <br> [- 查询结算信息](./oper-purchase/#transactions) |
| 推送                  | (TOAST 连动服务) <br> 推送消息及确认结果 | [[Android](./aos-push/#push)] [[iOS](./ios-push/#push)] [[Unity](./unity-push/#push)] |                                          | [[Push]](./oper-push/#push) <br/>- 实时、预约推送消息 |
| 排行榜           | (TOAST联动服务) <br> 查询及注册大容量数据的实时排名 |                                          | [[Wrapping API](./api-guide/#leaderboard)] |                                          |
| Webview               |SDK提供默认的WebView UI<br/>提供系统弹出窗口和TOAST UI | [[Android](./aos-ui/#webview)] [[iOS](./ios-ui/#webview)] [[Unity](./unity-ui/#webview)] |                                          |                                          |
| [管理员] Maintenance | (操作)维护功能                               |                                          | [[确认维护](./api-guide/#maintenance)] | [[Maintenance]](./oper-operation/#maintenance)<br>- 登记或取消维护 |
| [管理员] Notice      | (操作) 紧急公告功能 <br> -  游戏用户运行应用时，可以以弹出窗口的形式查看公告的功能 |                                          |                                          | [[Notice]](./oper-operation/#notice) <br/>-登记公告 |
| [管理员] Ban         | (操作) 游戏用户的登记和解除禁用状态<br> - 游戏用户的登记和解除禁用状态 | [[Android](./aos-authentication/#get-banned-user-information)] [[iOS](./ios-authentication/#get-banned-user-information)] [[Unity](./unity-authentication/#get-banned-user-infomation)] <br/> -确认禁用用户信息 |   [[게임 이용자의 이용정지 이력조회](./api-guide/#ban-histories)                                       | [[Ban]](./oper-ban/#ban) <br/>-登记和解除禁用状态 |


