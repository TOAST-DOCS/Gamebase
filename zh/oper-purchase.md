## 使用IAP菜单之前
![gamebase_purchase_01_201812](http://static.toastoven.net/prod_gamebase/Console_IAP_Select_currency_1.0.png)
如需使用IAP菜单，必须先从销售指标选项卡中选择一种货币。
将设置的货币代码显示在Analytics销售指标中（但只能设置一次）。
一旦选择，货币代码将无法更改，请慎重选择。

## Game > Gamebase > 控制台使用指南 > IAP

可以注册应用程序内结算信息并查看其记录。 
Gamebase使用NHN Cloud IAP(In-App Purchase、应用程序内结算)服务。

## Store
 
为了在游戏内销售商品，需要添加应用商店。
您可以在**Store**选项卡中的**商店信息列表**中添加新商店或管理已添加的商店。  
![gamebase_purchase_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_01_202210_zh.png)

### Register

如果要添加新商店，请点击**商店信息列表**页面上的**添加**按钮。
![gamebase_purchase_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_02_202210_zh.png)

* **商店** 选择要添加的外部商店，如果没有需要添加的商店，请联系[客户服务](https://toast.com/support/inquiry)。
* **App名称** 输入要添加的游戏名称。
* **Store App ID** 输入从商店获取的信息。
* **使用与否** 选择是否使用商店。

> [参考] Google Play Store的发票验证
>
> 谷歌发票验证系统出现故障时，如果将**一次性商品的发票验证设定**设为第一阶段，则可通过Gamebase内标签验证进行结算。
> 不管设置值为多少，始终对订购产品执行两个阶段的验证。
 
### Modify

您可以在查询列表中查看已添加商店的详细信息并更改信息。

![gamebase_purchase_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_03_202210_zh.png)
- 在查询目录中选择已添加的商店来查看详细信息。
- 点击**编辑**按钮可以编辑应用程序名称、商店应用程序和可用性信息，但Store App ID除外。
- 点击**删除**按钮可以删除商店信息。但只能删除未使用的商店。

## Product
可以注册商店的销售产品。
可通过使用**产品**选项卡，注册新的商品或管理以前注册的商品。
- (1) **注册** : 使用一个商店道具ID可以注册多个产品。 
- (2) **更改商店商品状态** : 通过修改一次，可更改使用一个商店商品ID注册的产品的“使用与否”。
- (3) **过滤** : 通过提供“是否使用”、“商店/商店道具ID/产品ID”、“产品名称过滤”功能，可使搜索变得更方便。若不存在搜索的值，则显示所有商店的产品列表。

![gamebase_purchase_04_202006](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_04_202006.png)

### Register
若需注册新商品，点击**道具列表**页面上的**注册**按钮。
#### 1. 通过直接输入注册的方法  
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_05_202006.png) 

* (1) **产品ID** : 输入请求支付时要使用的产品ID。使用该ID通过SDK调用购买API，则可购买输入的产品。
* (2) **产品名称** : 输入进行支付的产品名称。可以以输入的内容为准查询结算明细，并在指标中显示相关产品名称。
* (3) **是否使用** : 选择是否使用产品。通过SDK请求产品列表时，只将“是否使用”设为“USE”的商品传送到产品列表中。 
* (4) **添加产品** : 通过添加产品时选择**+**按键，可添加商品输入框。
* (5) **商店** : 选择需要注册的外部商店。若不存在要注册的商店，则需在**商店**菜单中先注册商店。 
* (6) **商店道具ID** : 注册商店后，输入获取的ID信息。当在所选的商店对在Gamebase商品中注册的列表请求支付时，基于在该部分输入的内容进行结算。
* (7) **商店道具类别** : 选择想要注册的产品类别。可以在Google play、App store注册订阅商品。如果选择Google play、App store之外的其他商店，将视为为一次性道具。

#### 2. 通过上载文件注册的方法
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_06_202006.png)

* 可通过上载文件注册商品。 
* 上载文件注册商品时，一次最多只能注册1,000个商品。 
* 若输入形式不正确，则无法注册商品，因此需要注意输入形式。
* 如果文件的编码形式不是“UTF-8”，则无法使用韩语键盘，因此需要注意。
* 若文件注册失败，则可通过结果窗口的**下载**按钮下载失败的列表后进行确认。


### Modify

可以查看或修改在查询列表中注册的商品的详细信息。 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_07_202006.png)
- 在查询列表中选择道具时，可以查看道具的详细信息。
- 通过点击**修改**按键，可修改商店、产品编号及产品类别之外的其他信息。 
- 可以对**产品名称**、**是否使用**、**商店道具ID**等项目进行修改。但其他项目无法更改，因此注册时要注意。
- **商店道具ID**只能修改为其他已注册的**商店道具ID**，新的**商店道具ID**必须要注册为商品。

## Transactions

可以查询付款信息。
通过选择搜索条件，可简单查看所需的付款信息。
通过点击右上端的**下载**按键，可下载结算明细的查询结果。

### Transaction Status Code
“支付状态代码”是显示用户进行支付时出现的状态的代码。

> [参考]
> 支付处理过程主要分为4个步骤。
> 
> [预约] - [支付] - [验证] - [完成]
> 
> - 预约 : 准备用户付款并将付款所需信息传递给Gamebase的过程
> - 支付 : 进行市场付款的过程
> - 验证 : 验证从市场接收的结果的过程
> - 完成 : 验证后，对每个市场执行必要的完成流程
- 完成支付准备 : 完成[预约] 阶段的状态
- 支付验证失败 : 在[验证] 阶段验证失败的状态
- 完成支付验证 : 完成[验证] 阶段的状态
- 完成支付 : 所有的支付过程已被结束的状态
- 完成退款 : 用户或运营者已退款的状态
- 进行之中取消 : 用户在进行支付之中取消的状态

> [参考]
> 如果完成验证状态没有变化，请联系客户服务。
 
### Properties

#### Search conditions
按照选择的搜索类型显示不同的搜索项目。

##### (1) 一般搜索
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_10_202210_zh.png)

可以搜索符合下列搜索条件的结果。
- **搜索日期** : 用户尝试购买的时间（通过使用右边的降序/升序项目选择排列）
- **商店** : 要搜索的商店信息
- **商店道具ID** : 注册商店后获取的ID信息
- **产品** : 选择要搜索的商品。
- **用户ID** : 支付的用户ID
- **支付状态** : 将要搜索的支付状态基准


##### (2) 搜索Transaction ID
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_11_202210_zh.png)

可以利用付款时生成的Transaction ID进行查询。 

##### (3) 搜索发票
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_12_202210_zh.png)
可利用支付时提供的发票信息进行查询。 

#### 搜索结果
以下为查询结果项目。
- **Transaction ID** : 可在Gamebase内区分的固有编号
- **商店** : 支付的商店信息
- **用户ID** : 支付的用户ID
- **产品名称** : 用户在应用程序内购买的实际商品名称
- **产品ID(商店道具ID)** : 用户在应用程序内购买的实际商品ID和在商店进行支付的商店道具ID
- **商店道具类别** : 用户在应用程序内购买的实际道具类别
- **价格/货币单位** : 用户购买的道具价格及货币单位
- **消费状态** : 是否提供支付完的道具
- **支付状态** : 结算程序的当前进行状态
- **Store Reference Key** : 商店发布的结算固有编号
- **预约支付日期** : 用户尝试购买的时间
- **支付日期** :  用户完成购买的时间
- **退款日期** : 用户道具退款的时间
- **追加信息** : 通过SDK请求支付时传送的追加信息(Developer payload)

#### 变更付款状态
搜索的付款信息的各状态如下。
- **完成付款(Success)**
    - 表示已正常完成付款流程。
    - 可以更改为Refund状态。
- **完成支付预约(Reserved)**
	- 表示不再进行商店内付款或未进行结算验证
	- 可以更改为Success、Refund状态。
- **支付验证失败(Failure)**
	- 表示在商店付款过程中验证失败。
	- 可以更改为Success、Refund状态。 
- **完成退款(Refund)**
	- 表示管理员手动处理“在商店是否准许退还请求”。
	- 无法更改为其他付款状态。
- **取消支付(UserClose)**
	- 用户取消支付的状态

##### Success变更
![gamebase_purchase_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_08_202210_zh.png)
您可以通过输入付款时收到的**发票编号**、**价格**、**货币**信息来更改状态。

##### Refund变更
![gamebase_purchase_09_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_09_202210_zh.png)
不必输入其他追加信息，确认状态后选择变更即可。
您更改的付款信息无法再次更改，因此需要仔细检查信息。

#### 验证发票
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* 可验证查询的发票的付款是否有效。
* 可确认各字段的比较结果。从商店获得的响应值以JSON形式提供，需要时可直接确认数据。
* 当前仅可验证App Store付款项目。

## 结算Abusing监测

可查看结算Abusing信息，设置自动制裁/解除。 

### 退款历史查询

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing1_1.3_ko.png)

使用以下搜索条件可查看所需的结算和退款信息。
通过单机右上方的**下载**按钮来下载结算和退款明细。

#### 搜索条件
- **退款时间** : 处理退款的时间
- **用户ID** : 进行支付的用户ID
- **退款数** : 是用户退款的次数，可以查询输入的次数以上的次数。
- **退款金额** : 是用户退款的金额，可以查询输入的金额以上的金额。
- **商店** : 进行退款的商店

#### 搜索结果
- **用户ID** : 支付的用户ID
- **商店** : 进行退款的商店信息
- **退款数** : 用户退款的次数
- **退款金额** : 用户退款的金额
- **结算次数(查询时期)** : 用户在查询时期支付的次数
- **结算金额(查询时期)** : 用户在查询时期支付的金额
- **结算次数(积累)** : 用户支付的总次数
- **结算金额(积累)** : 用户支付的总金额
- **结算次数(最近3个月)** : 用户在最近3个月之内支付的次数 
- **结算金额(最近3个月)** : 用户在最近3个月之内支付的金额
- **状态** : 用户的现状态
- **状态变化** : 根据用户的状态，禁止使用或解除禁用。

#### 状态变化
![gamebase_member_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812.png)

是可更改查询游戏用户的账户状态的功能。 
可进行更改的各状态如下。

- **正常** : 可更改为禁用或退出状态。退出时不可返回相关账户信息，因此处理前必须注意。
- **禁用** : 可解除禁用。
- **禁用预约** : 可更改为禁用状态。
- **退出** : 无法更改状态，因此不显示相关按键。

#### 确认支付明细

在搜索的列表中单击用户ID时，可 查询搜索期间的支付详细明细。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing2_1.3_en.png)

#### 支付明细
- **预约支付日期**：用户尝试或完成购买的时间
- **退款日期**：用户对道具进行退款的时间
- **预约支付日期** : 用户尝试购买的时间
- **支付日期** :  用户完成购买的时间
- **退款日期** : 用户道具退款的时间
- **Transaction ID**：Gamebase中可区别支付的固有编号
- **商店**：支付的商店信息
- **道具名**：用户在应用程序中购买的实际道具名
- **价格**：用户购买的道具价格
- **货币单位**：用户购买时使用的货币类型
- **支付状态**：支付的当前进行状态

### 结算Abusing自动解除历史查询

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing5_1.3_en.png)

通过以下搜索条件，可查看所需的结算Abusing自动解除用户信息。

#### 搜索条件
- **查看期** : 以开始预约禁用的时间为准进行查询。
- **用户ID** : 预约禁用的用户ID
- **结算次数** : 是在预约禁用的时期内用户支付的次数。可以查看输入的次数以上的次数。
- **结算金额** : 是在预约禁用的时期内用户支付的金额。可以查看输入的金额以上的金额。

#### 搜索结果
- **用户ID** : 预约禁用的用户ID
- **禁用预约时期** : 开始禁用预约或结束禁用的时间。
- **结算次数(解除条件)** : 为了解除禁用要进行支付的总次数
- **结算金额(解除条件)** : 为了解除要支付的总金额
- **满足解除条件** : 为满足解除的条件(AND, OR)
- **结算次数(预约时期)** : 在禁用预约时期内用户支付的总次数
- **结算金额(预约时期)** : 在禁用预约时期内用户支付的总金额

#### 结算明细查询

通过在搜索的列表中点击用户ID，可查看查询期间的支付详细明细。
(但没有支付明细的用户将更改为非激活状态。)

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing6_1.3_en.png)

#### 支付明细
- **支付日期** : 用户完成购买的时间
- **Transaction ID** : 可区分在Gamebase内支付的固有编号
- **商店** : 结算的商店信息
- **用户ID** : 结算的用户ID
- **商店道具ID** : 用户在应用程序内购买的实际商店道具ID
- **结算金额** : 用户支付的金额

#### 结算Abusing自动制裁设置

若欲使用设置自动制裁，单击**使用**按钮，输入设定值。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing3_1.3_en.png)

#### 设置信息

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing4_1.3_en.png)

* **禁用时期** ：适用自动制裁时需要输入禁用时期。
    * **永久禁用** : 当要进行永久禁用处理时选择。
    * **指定时期** : 以日(day)单位指定禁用时期。 
* **自动制裁条件设置** ：将符合设置条件的用户处理为自动制裁。至少要设置1个以上。 
    * **退款数** : 输入Abusing的退款次数。
    * **退款金额** : 输入Abusing的退款金额。
* **自动制裁消息设置**  
    * 输入要向用户显示的禁用消息。
    * 通过用多国语言输入向用户显示的消息，提供便于重复使用的模板。选择已被注册的模板进行注册。 
* **删除Leaderboard**
    * 自动制裁时，设置是否将相关游戏用户的Leaderboard数据也要一起删除。
    * 进行选择和注册后，适用自动制裁时，Leaderboard中的游戏用户数据将被删除。<font color="red">相关数据将无法恢复，</font>因此必须注意。
 
#### 结算Abusing自动解除设置 

如需使用自动解除设置，则单机**使用**按钮设置设定值。 
为了激活自动解除设置，必须要激活自动制裁设置<font color="red"></font>。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing7_1.3_en.png)

#### 设置信息

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing8_1.3_en.png)

* **禁用的解除时期** : 适用自动解除时输入禁用预约时期。
* **禁用解除条件设置** : 设置自动解除时所需的条件。至少要设置1个以上。 
    * **结算次数** : 为了自动解除，要输入结算次数。 
    * **结算金额** : 为了自动解除，要输入结算金额。
* **禁用解除消息**
    * 输入向用户显示的禁用解除消息。
    * 通过用多国语言输入向用户显示的消息，提供便于重复使用的模板。选择以前已被注册的模板进行注册。   
