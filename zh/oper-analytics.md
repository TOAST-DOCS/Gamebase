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