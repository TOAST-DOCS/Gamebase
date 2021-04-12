## Game > Gamebase > 控制台使用指南 > Analytics

可以通过表及图表确认应用程序用户的现状、销售相关指标。
Analytics由以下菜单构成。

* 实时监控：应用程序用户的实时同时在线及实时付款指标
* 用户指标：以应用程序用户为中心的基本指标(DAU, MCU, NRU)及环境、流入/流出、保留等指标
* 销售指标：应用程序的销售管理指标
* 组同时在线：Gamebase服务用户所在项目的组同时在线及各组基本指标
* 使用环境：调用安装URL的相关统计指标

## 实时监控
### 实时同时在线

可确认当前应用程序用户的实时同时在线指标及检查、推送信息。

![gamebase_analytics_01_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_01_201901_2.png)

#### 1. 实时同时在线人数(CCU)变化图表
每隔1分钟更新数据，可确认实时更改的指标。

* 同时在线人数(CCU)：以1分钟为单位测定的实时同时在线人数（登录用户数）
* 付款金额：当天0点~24点用户在Gamebase中支付的金额之和，且不含退款、取消付款等信息的纯付款金额。
* 新注册者(NRU)：新注册者。当天0点~24点首次被收集登录日志的用户（以memberno为准）

#### 2. 维护信息
根据当天0点~24点登记到Gamebase的维护信息，可以确认维护后同时在线人数的增减趋势。

#### 3. 推送信息
根据当天0点~24点向Gamebase发送的推送信息，可以确认发送后同时在线人数的增减趋势。

### 仪表板

可以一目了然地实时确认游戏用户的各种指标。

![gamebase_analytics_02_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_02_201901_2.png)

#### 1. 实时用户现状仪表板
可以确认应用程序用户及付款指标。
如果日期为今天，每隔10分钟更新；根据选择的日期，显示当天的指标。

* 同时在线(CCU)：以10分钟为单位测定的实时同时在线人数（登录用户数）
* 最多同时在线人数(MCU)：0点~24点最多同时在线人数（以1分钟为单位CCU中最大的值）
* 新注册者(NRU)：当天0点~24点首次被收集登录日志的新注册者（以memberno为准）
* 日用户(DAU)：以每日memberno为准登录1 次以上的活跃用户数(Daily Active Users)
* 平均游戏时间(Avg.Playtime)：按日期别的整体平均游戏时间（DAU的游戏时间之和/DAU）
* 累计用户：安装Gamebase后注册的全部累计用户数（以memberno为准）
* 付款金额：0点~24点用户付款的总金额
* 付款件数：0点~24点用户的总付款件数
* PU: 支付收费商品的用户（游戏用户）（=再次购买PU + 新增PU）
* 新增PU(NPU)：第一次支付收费商品的用户(New Paying Users)
* ARPPU: 1名付款用户(PU)支付的平均付款金额（=付款金额/PU）
* ARPU: 1名游戏用户(DAU)支付的平均付款金额（=付款金额/DAU）

※ MCU、ACU、ARPU仅在过滤器为全部时可确认。

#### 2. 主要指标实时变化图表
可以以10分钟为间隔确认同时在线人数(CCU)、新注册者(NRU)、付款金额、PU指标的变化。

#### 3. 实时占有率现状
可以通过图表确认用户的OS、应用程序版本、商店、国家的占有率。
如果日期为今天，以CCU为准，如果为前一天，以DAU为准显示。

* OS占有率：DAU中各OS占有比例（当天以CCU为准）
* 应用程序版本占有率：DAU中应用程序版本占有比例（当天以CCU为准）
* 商店占有率：DAU中商店占有比例（当天以CCU为准）
* 各国家占有率：DAU中各国家占有比例（当天以CCU为准）

## 用户指标
### 用户

可以确认用户的基本指标。

![gamebase_analytics_03_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_03_201901_2.png)

#### 1. 用户现状
显示所选期间内用户的基本指标。

* 日总用户(DAU+)：以每日memberno为准登录1次以上的活跃用户数之和(Daily Active Users)
* 周总用户(WAU+)：一周DAU之和(Weekly Active Users)。选择周指标时，WAU替代DAU项目
* 月总用户(MAU+)：一个月DAU之和(Monthly Active Users)。选择月指标时，WAU替代DAU项目
* 最多同时在线人数(MCU)：0点~24点最多同时在线人数以1天为单位来合计以1分钟为单位CCU中最大的值
* 新注册者(NRU)：新注册者。当天0点~24点首次被收集登录日志的用户（以memberno为准）
* 退出用户：退出用户。当天0点~24点删除memberno的用户数
* 平均CCU：所选时间段内CCU的平均值
* 平均游戏时间(Avg.Playtime)：安装Gamebase后注册的全部累计用户数（以memberno为准）

#### 2. 日别指标
以图表和表显示所选期间内日别用户的基本指标。

※ MCU、累计用户(ACU)仅在过滤器为全部时可确认。

### 用户环境
![gamebase_analytics_04_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_04_201901_2.png)

可确认各环境下的用户指标。

* 查询条件
    * OS: 移动设备运营软件Android、IOS等
    * 国家：用户移动设备上设置的国家
    * 商店： IdP: Facebook、Google等用户的IdP登录/验证信息
    * 应用程序版本：运行的应用程序的版本信息
    * 设备：用户的设备种类。选择设备时不提供PU、付款金额（也不提供周、月数据）
* 查询值
    * 日用户(DAU)：以每日memberno为准登录1 次以上的活跃用户数(Daily Active Users)
    * 新注册者(NRU)：新注册者。当天0点~24点首次被收集登录日志的用户（以memberno为准）
    * PU: 支付收费商品的用户（游戏用户）（=再次购买PU + 新增PU）
    * 付款金额：用户付款的总金额

### 用户流入与流出


可以确认与应用程序用户的流入、流出相关的不同日期的趋势。

![gamebase_analytics_05_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_05_201901_2.png)

* 流入用户（新增+回归）：流入用户为新注册者与回归用户之和（=新注册者 + 回归用户）
* 新注册者：新注册者。当天0点~24点首次被收集登录日志的用户（以memberno为准）
* 回归用户：基准日期的前8天内未被收集日志的用户
* 流出用户（退出+离开）：流出用户为退出用户与离开用户之和（=退出用户 + 离开用户）
* 退出用户：退出用户。当天0点~24点删除memberno的用户数
* 离开用户：基准日期的前7天内未被收集日志的用户

### Retention

Retention为可以确认在指定日注册的用户从第二天起的90天内还剩余多少的指标。

![gamebase_analytics_06_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_201901_2.png)

若选择当天退出者除外选项，可将当天注册后退出的用户排除在外进行确认。

* 当天退出者除外：将当天注册并当天退出用户排除在新增用户之外后计算值。
    * 新注册者(New User) = 当天注册者 - 当天注册后退出者
      例）1月1日新注册100名，若其中20名在1月1日退出，则实际新注册者计算为80名（100名-20名）

### LTV

LTV是显示用户组中一个选定用户每年的预期销售的估算指标。

根据国家、OS及日期提供LTV图标。可以在下端表中确认LTV、累积NRU、累积PU、累积付款金额等详细信息。

#### 估算方法
Gamebase以注册后365天的累积ARPU作为估算LTV的方法。
  
#### 用户组条件
用户组的条件如下。 

* 注册日期 
* 国家
* OS

#### 限制条件
为了正确估算LTV适用的条件如下。

* 用户组的用户数需要达到1000名以上。 
* 用户组的PU(结算用户数)需要达到30以上。
* 只估算新注册后已过7天的用户组用户。

### Life Cycle

Life Cycle是可确认从用户首次注册到至今的用户的“日在线趋势”的指标。将数据提供3年。

* 日用户(DAU) : 以日memberno为基准， 登录次数为1次以上的活跃用户数（Daily Active Users）
* 最多同时在线用户(MCU) : 0点~24点最多同时在线人数/以1天为单位来合计以1分钟为单位CCU中最大的值
* 新注册者(NRU) : 当天0点~24点首次被收集登录日志的用户（以memberno为准）
* 退出用户：退出用户/当天0点~24点删除memberno的用户数
* 当天注册退出用户 : 当天注册后，当天退出的用户
* 平均CCU：所选时间段内CCU的平均值
* 玩游戏的平均时间 - Avg.Playtime(/DAU) : 查询期间的平均Playtime（DAU的Playtime之合 / DAU）

### Frequency7


Frequency7指标提供DAU的一周内的访问人数和比率信息。可一目了然地确认游戏沉迷程度及忠诚度。

Frequency7的标准可分为以下3个项目。

* 访问次数 : 7天中访问的次数
* 连续访问次数 : 7天中包括特定日期的连续访问次数
* 最多的连续访问次数 : 7天中连续访问的最多次数

上述标准的计算方法如下。
如果以3/7为标准，而用户总共在3/1, 3/2, 3/3, 3/6, 3/7访问了，等于访问的次数如下。

* 总访问次数 : 5天(3/1, 3/2, 3/3, 3/6, 3/7)
* 连续访问次数 : 2天(3/6, 3/7)
* 最多的连续访问次数 : 3天(3/1, 3/2, 3/3)


> [参考] 
>
> 只处理预先注册的世界/服务器/渠道、类别/职业信息。
> 请参考如下文件进行注册。
>
> - [应用程序 > Analytics Indicator](./oper-app/#analytics-indicator)

## 销售指标
### 付款金额

可以确认付款金额指标。

![gamebase_analytics_07_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_07_201901_2.png)

#### 1. 付款金额现状表
可以确认所选时间段内的付款金额。
可以确认总付款金额与主要商店的各国付款金额。

#### 2. 销售趋势
可以通过图表确认日期别新增销售、再次购买销售、PU（付款用户）的趋势。
可以在下表中确认各国销售。

### 收费用户

可以确认收费用户(PU)的指标。

![gamebase_analytics_08_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_08_201901_2.png)

以下为图表及表中出现的用语说明。

* 付款金额：用户付款的付款金额
* 新增付款金额：新增付款用户(NPU)支付的付款金额
* DAU: 以每日memberno为准登录1次以上的活跃用户数(Daily Active Users)
* PU: 支付收费商品的用户(Paying User)PU=再次购买PU + 新增PU
* 新增PU(NPU)：第一次支付收费商品的用户(New Paying Users)
* 再次购买PU：累计PU - 新增PU（再次购买PU为日数据，以前一日为准计算）
* PUR: 收费用户的比例(PU/DAU * 100)
* ARPU: 一天之内游戏用户数的平均付款金额（付款金额/DAU）
* ARPPU: 付款用户数的平均付款金额（付款金额/PU）
* ARPNPU: 新增收费用户的平均付款金额（付款金额/NPU）

### 道具销售指标

可以确认Gamebase中登记的道具的销售指标。

![gamebase_analytics_09_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_09_201901_2.png)

* 道具：Gamebase中登记的道具列表
* 最佳道具Top 10：按照销售金额、销售数量排行的销量最高的Top 10道具列表
* 商店：应用程序商店、Google Play等商店
* 付款金额：用户付款的各道具付款金额
* 付款件数：各道具付款件数
* 付款比例：各道具付款比例

### 首次购买
![gamebase_analytics_10_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_10_201901_2.png)

可以确认新增收费用户首次购买的相关信息。
按照付款金额顺序显示新增收费用户购买的所有道具。

* 道具：新增PU购买的道具列表
* 商店：应用程序商店、Google Play等商店
* 新增PU(NPU)：第一次支付收费商品的用户(New Paying Users)
* 新增付款金额：新增PU的付款金额

## 组同时在线
### 组同时在线人数

可以确认Gamebase服务用户所在所有项目的同时在线人数指标。

![gamebase_analytics_11_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_11_201901_2.png)

* 实时组同时在线：显示Gamebase服务用户所在项目的实时同时在线人数(CCU)。
* 项目组同时在线：以选择的时间段、过滤器为准显示应用程序用户数。

### 组比较指标

可以将Gamebase服务用户所在项目与过滤器组合，作为组比较。

![gamebase_analytics_12_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_12_201901_2.png)

* DAU: 以每日memberno为准登录1次以上活跃用户数(Daily Active Users)
* NRU: 当天新加入者
* PU: 支付收费商品的用户(Paying User)（=再次购买PU + 新增PU）
* 付款金额：用户付款的付款金额

※ 图表中显示的组名为**{appId} _ {OS} _ {国家}**形式。

## 环境
### 安装URL

可以确认调用安装URL的相关统计指标。

![gamebase_analytics_13_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_13_201901_2.png)

* 最近一周下载URL调用数：通过[应用程序 > 安装URL]的安装路径调用游戏安装API的顾客数.通过应用程序安装的市场营销，可测定调用Short URL的客户数，以确认客户反应。
* 各浏览器占有率（全部累计）：可确认各浏览器安装URL调用数的比例。
* 各平台占有率（全部累计）：可确认各平台安装URL调用数的比例。

## Transmission

**传送指标**标签可在游戏向API传送指标时确认。
传送指标分为如下3种。

* 用户级别：可按用户级别确认访问及销售信息。
* 世界/服务器/渠道：可按世界/服务器/渠道确认访问及销售信息User IdUser Id。
* 类别/职业：可按类别/职业确认访问及销售信息。

> [참고] 
>
> 월드/서버/채널, 클래스/직업은 사전에 등록된 정보만 처리 합니다.
> 등록 방법은 다음 문서를 참고하시기 바랍니다
>
> - [앱 > Analytics Indicator](./oper-app/#analytics-indicator)

### Concurrent Status

可确认所选传送指标类型及日期的访问、销售信息。
同时在线人数按当天提供CCU，按日期提供DAU信息。当天以10分钟为单位更新信息。

![gamebase_analytics_14_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_14_201906_1.png)

* CCU (Concurrent User)：以10分钟为单位测定的实时同时在线人数（登录用户数）
* DAU (Daily Active User)：以一日用户ID为准，登录1次以上的活跃用户数
* NRU (New Registered User)：当天新增用户
* 付款件数：收费商品付款件数
* 付款金额：收费商品付款金额

### Status By Level

可按级别确认访问、销售现况。

![gamebase_analytics_15_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_15_201906_1.png)

* DAU (Daily Active User)：以日间用户ID为准，登录1次以上活动用户数
* Avg.Playtime:该级别的各日期所有Playtime的平均值（DAU的Playtime之和 / DAU）
* 付款金额：收费商品付款金额
* 新增付款：新增付款用户(NPU)支付的金额
* 再次购买付款：再次购买付款用户支付的金额
* PU (Paying User)：支付收费商品的用户。（=再次购买PU + 新增PU）
* 新增PU：首次支付收费商品的用户
* 再次购买PU：所有PU - 新增PU（再次购买PU为一日数据，以前一日为基准计算）

### Status By Channel

可按世界/服务器/渠道确认访问、销售现况。

![gamebase_analytics_16_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_16_201906_1.png)

* DAU (Daily Active User)：以一日用户ID为准，登录1次以上的活跃用户数
* Avg.Playtime:相应级别的各日期所有Playtime的平均值（DAU的Playtime之和 / DAU）
* 付款金额：收费商品付款金额
* 新增付款：新增付款用户(NPU)支付的金额
* 再次购买付款：再次购买付款用户支付的金额
* PU (Paying User)：支付收费商品的用户（=再次购买PU + 新增PU）
* 新增PU：首次支付收费商品的用户
* 再次购买PU：所有PU - 新增PU（再次购买PU为一日数据，以前一日为基准计算）

### Status By Class

可按类别/职业确认访问、销售现况。

![gamebase_analytics_17_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_17_201906_1.png)

* DAU (Daily Active Users)：以一日用户ID为准，登录1次以上的活跃用户数
* Avg.Playtime:相应级别的各日期所有Playtime的平均值（DAU的Playtime之和 / DAU）
* 付款金额：收费商品付款金额
* 新增付款：新增付款用户(NPU)支付的金额
* 再次购买付款：再次购买付款用户支付的金额
* PU (Paying User)：支付收费商品的用户。（=再次购买PU + 新增PU）
* 新增PU：首次支付收费商品的用户
* 再次购买PU：所有PU - 新增PU（再次购买PU为一日数据，以前一日为基准计算）

### Level Up

可确认用户的升级信息。

* 达成级别：已达成的级别
* 升级达成用户：已达成相应级别的用户数
* 升级平均达成时间（分钟）：达成相应级别的用户的平均达成时间（分钟）

![gamebase_analytics_18_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_18_201906_1.png)

### Item Sales Status

可确认所选传送指标类型的道具销售现况。
单击**条件**按钮，可选择如下查询值。

* 付款金额
* 付款件数
* PU (Paying User)
* 新增PU

![gamebase_analytics_19_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_19_201906_1.png)

### Item Sales TOP 50

可确认所选传送指标类型及值的道具销售前50名项目。

![gamebase_analytics_20_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_20_201906_1.png)