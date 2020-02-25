## Game > Gamebase > 控制台使用指南 > IAP

您可以注册应用程序内结算有关的信息并查看其历史记录。
在Gamebase使用 TOAST IAP(In-App Purchase，应用程序内支付)服务。

## Store

为了在游戏内销售商品，添加应用商店。
您可以在**Store** 选项卡中的**商店信息列表**中添加新商店或管理已添加的商店。
![gamebase_purchase_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_01_201812.png)

### Register

添加新商店，请点击**商店信息列表**页面上的**添加**按钮。
![gamebase_purchase_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_02_201812.png)

* **商店**  选择要添加的外部商店，如果没有需要添加的商店，请联系 [客服中心](https://toast.com/support/inquiry)。
* **App 名称** 输入要添加的游戏名称。
* **Store App ID** 输入从商店获取的信息。
* **使用与否**  选择是否使用商店。

### Modify

您可以在查询列表中查询已添加商店的详细信息或更改信息。

![gamebase_purchase_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_03_201812.png)
- 在查询目录中选择已添加的商店来查询详细信息。
- 点击**编辑** 按钮可以编辑应用程序名称、商店应用程序和可用性信息，但Store App ID除外。
- 点击**删除** 按钮可以删除商店信息。但，只能删除未使用的商店。

## Item

您可以添加在商店中出售的item。
您可以在** Item ** 标签中添加新item或管理已添加的item。默认情况下，将显示所有商店的item，并且还可以使用各商店的筛选功能。

![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_04_201911.png)

### Register

要添加新item，请点击**商店信息列表**页面上的** 添加 **按钮。
![gamebase_purchase_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_05_201812.png)

* **商店** 选择要添加的外部商店。如果没有您要添加的商店，请先在**商店**菜单上添加商店。
* **item名称** 输入您在添加商店后收到的商品信息。使用游戏中添加的item名称在app中显示。
* **商店商品ID** 输入您要添加的item名称。
* **使用与否** 选择是否出售该item。

### Modify

您可以在查询列表中查询已添加item的详细信息或更改信息。
![gamebase_purchase_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_06_201812.png)
- 在查询列表中选择各item，则可以查询已添加item的详细信息。
- 点击**编辑** 按钮可以更改除商店和 item以外的信息。
- 点击**删除** 按钮可以删除item信息。

## Transactions

可以查询结算信息。
원하는 검색 유형을 선택하여 원하는 결제 정보를 편리하게 조회할 수 있습니다.
결제 내역 조회 결과는 오른쪽 상단의 **다운로드** 버튼을 클릭해 언제든지 다운로드할 수 있습니다.

#### Search conditions
按照选择的搜索类型显示不同的搜索项目。

##### (1) 一般搜索
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_10_201911.png)

一般搜索的情况下，可查询满足如下搜索条件项目的结果。
- **商店**：支付的商店信息
- **日期**：用户尝试购买的时间
- **支付编号**：Gamebase中可区别支付的固有编号
- **道具编号**：用户在应用程序中购买的实际道具编号（道具编号可在“道具”标签确认。）
- **用户ID**：支付的用户ID
- **排列顺序**：以注册日期为准升序、降序排列
- **支付状态**：以支付状态为准搜索

##### (2) 搜索Trnasaction ID
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_11_201911.png)

可通过支付后生成的Transaction ID查询。

##### (3) 搜索发票
![gamebase_ban_01_201812](http://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_purchase_12_201911.png)
可利用支付时提供的发票信息查询。

#### 搜索结果
- **Transaction ID**: 用于区分Gamebase内支付的唯一编号
- **商店**: 已付款的商店信息
- **用户 ID**: 付款的用户ID
- **item名称**: 用户在APP内购买的实际item名称
- **价格**: 用户购买的item价格
- **货币**: 用户购买时使用的货币类型
- **消费状态**: 付款项目是否已付款
- **付款状态**: 目前的付款进度
- **Store Reference Key**: 商店发行的付款唯一编号
- **付款日期**: 用户尝试购买或完成购买的时间
- **退还日期**: 用户退还item的时间

#### 变更付款状态
查询付款信息的状态如下所示。

- **Success**
	- 完成付款
    - 这意味着付款流程已正常完成。
    - 可以变更为Refund状态。
- **Reserved**
	- 正在进行付款
	- 这意味着通过商店付款不再进行或尚未进行验证
	- 可以变更为Success, Refund状态
- **Failure**
	- 付款验证失败
	- 这意味着在商店付款过程中验证失败。
	- 可以变更为Success, Refund状态。
- **Refund**
	- 完成退款
	- 管理员已在商店中手动处理,是否准许退还请求。
	- 无法更改为其他付款状态。

##### Success 变更
![gamebase_purchase_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_08_201812.png)
您可以通过输入付款时收到的**发票编号**, **价格**, **货币**信息来更改状态。

##### Refund 变更
![gamebase_purchase_09_201812](https://static.toastoven.net/prod_gamebase/gamebase_purchase_09_201812.png)
不必输入其他追加信息，确认状态后选择变更即可。
您更改的付款信息无法再次更改，因此需要仔细检查信息。

#### 验证发票
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* 可验证查询的发票的付款是否有效。
* 可确认各字段的比较结果。从商店获得的响应值以JSON形式提供，因此需要时可直接确认数据。
* 当前仅可验证App Store付款项目。

## Payment abusing monitoring

可查询滥用支付信息并制裁。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing1_1.0.png)

可利用如下搜索条件查询所需支付及退款信息。
支付及退款明细可单击右上方的**下载**按钮随时下载。
#### 搜索条件

- **退款日期**：退款处理的时间
- **用户ID**：支付的用户ID
- **退款次数**：用户获得退款的次数。查询到超出输入的次数。
- **退款金额**：用户获得退款的金额。查询到超出输入的金额。

#### 搜索结果
- **用户ID**：支付的用户ID
- **商店**：支付的商店信息
- **退款次数**：用户获得退款的次数
- **退款金额**：用户获得退款的金额
- **支付次数（查询期间）**：用户在查询期间内支付的次数
- **支付金额（查询期间）**：用户在查询期间内支付的金额
- **支付次数（累计）**：用户支付的全部累计次数
- **支付金额（累计）**：用户支付的全部累计金额
- **支付次数（最近3个月）**：用户最近3个月内支付的次数
- **支付金额（最近3个月）**：用户最近3个月内支付的金额
- **状态**：用户的当前状态
- **手动制裁**：根据用户状态，停止使用或解除停止

#### 手动制裁
![gamebase_member_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812.png)

可更改查询的用户账号状态的功能。
可按状态更改的情况如下。
- **正常**：可更改为停止使用、退出状态。退出时无法恢复相应账号信息，因此处理前需要确认并注意。
- **停止使用**：可取消停止使用。
- **退出**：不显示相应按钮。

#### 确认支付明细

在搜索的列表中单击用户ID时，可查询搜索期间的支付详细明细。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing2_1.0.png)

#### 支付明细
- **预约支付日期**：用户尝试或完成购买的时间
- **退款日期**：用户对道具进行退款的时间
- **Transaction ID**：Gamebase中可区别支付的固有编号
- **商店**：支付的商店信息
- **道具名**：用户在应用程序中购买的实际道具名
- **价格**：用户购买的道具的价格
- **货币单位**：用户购买时使用的货币类型
- **支付状态**：支付的当前进行状态

#### 设置自动制裁滥用支付

若欲使用设置自动制裁，单击**使用**按钮，输入设定值。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing3_1.0.png)

#### 设置信息

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_PaymentAbusing4_1.0.png)

* **停止使用期间**  应用自动制裁时，输入停止使用期间。
    * **永久停止**：欲永久停止使用时选择。
    * **指定期间**：输入停止使用期间。日 (day)
* **设置自动制裁条件**  符合设置条件的用户自动进行制裁处理。最少应设置为1个以上。
    * **退款次数**：输入属于滥用的退款次数。
    * **退款金额**：输入属于滥用的退款金额。
* **设置自动制裁信息**  
    * 输入向用户显示的停止使用信息。 
    * 以多国语言输入向用户显示的信息，提供模板，以便再次使用。选择提前注册的模板注册。
* **删除排行榜** 
    * 设置自动制裁时相应游戏用户的Leaderboard数据是否也一同删除。
    * 选择后注册，则应用自动制裁时，排行榜中游戏用户的数据被删除，<font color="red">相应数据无法恢复，</font>因此应注意。
