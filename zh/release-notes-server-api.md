## Game > Gamebase > Release Notes > Server API

### 2021. 09. 14.

#### 修改程序错误
* 修改了Leaderboard Wrapping API。
	* 修改了”多数用户分数/ExtraData注册”API的映射错误。

### 2021. 03. 09.

#### 添加功能
* 添加了使用IdP ID获取Gamebase user ID的API。

### 2020. 08. 11.

#### 改善/修复功能
* 添加了消耗优惠券的API错误代码 : 在优惠券代码输入英文或数字之外的值时(Error Code:-4000205)

### 2020. 02. 11.

#### 改善/修复功能
* 添加了调用退出API时的regUser长度有效性验证(validation)。

### 2020. 01. 14.

#### 添加功能
* 添加了用户退出API。

### 2019. 11. 12.

#### 添加功能
* 已开始优惠券服务 : 生成和管理大量优惠券的功能
	* 添加了确认或消费优惠券的API。

### 2019. 05. 28.

#### 改善/修复功能
* 更改了LTV查询修改和failover逻辑。

### 2019. 03. 26.

#### 添加功能
* 添加了TransferAccount功能 : 是guest用户在未映射的状态下，最多使用2个密钥转移至新设备的功能。
	* 验证发布的TransferAccount的ID/PW服务器API(validateTransferAccount)。

### 2018. 06. 26.

#### 添加功能
* getSimpleLaunching : 是确认启动客户端应用程序时提供的Launching信息的API。

### 2017. 11. 30.

#### 改善/修复功能
* 在List中将[查询维护API](./api-guide/#check-under-maintenance)结果更改为单一对象。

### 2017. 04. 04.

#### 改善/修复功能
* [IAP](./api-guide/#purchaseiap) API联动 : 查询道具、查询未消费明细
* 在checkAccessToken API响应结果中添加了用于登录的包含IdP信息的Spec。

### 2017. 03. 21.

#### 改善/修复功能
* [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API联动

### 2017. 03. 09.

#### 推出新商品
* 是一项通过提供游戏中通常需要的功能来帮助轻松高效地开发游戏的服务。
	* 支持多种认证 : Guest、3rd Party(Google、Facebook、GameCenter等)认证
	* 提供注销和退出会员功能。
	* 为了使一名User同时使用多个外部IDP，提供mapping功能。
	* 为了运营游戏，通过网络控制台提供游戏应用程序状态管理、维护、紧急公告等功能。
	* 提供可实时确认运营指标的网络控制台。
	* TOAST Cloud商品联动 : PUSH、IAP
