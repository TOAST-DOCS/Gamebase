## Game > Gamebase > 控制台使用指南 > IAP

您可以注册应用程序内结算有关的信息并查看其历史记录。
在Gamebase使用 TOAST IAP(In-App Purchase，应用程序内支付)服务。

## Store

为了在游戏内销售商品，添加应用商店。
您可以在**Store** 选项卡中的**商店信息列表**中添加新商店或管理已添加的商店。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App1_1.0.png)

### Register

添加新商店，请点击**商店信息列表**页面上的**添加**按钮。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App2_1.0.png)

* **商店**  选择要添加的外部商店，如果没有需要添加的商店，请联系 [客服中心](https://toast.com/support/inquiry)。
* **App 名称** 输入要添加的游戏名称。
* **Store App ID** 输入从商店获取的信息。
* **使用与否**  选择是否使用商店。

### Modify

您可以在查询列表中查询已添加商店的详细信息或更改信息。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_App3_1.0.png)
- 在查询目录中选择已添加的商店来查询详细信息。
- 点击**编辑** 按钮可以编辑应用程序名称、商店应用程序和可用性信息，但Store App ID除外。
- 点击**删除** 按钮可以删除商店信息。但，只能删除未使用的商店。

## Item

您可以添加在商店中出售的item。
您可以在** Item ** 标签中添加新item或管理已添加的item。默认情况下，将显示所有商店的item，并且还可以使用各商店的筛选功能。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item1_1.0.png)

### Register

要添加新item，请点击**商店信息列表**页面上的** 添加 **按钮。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item2_1.0.png)

* **商店** 选择要添加的外部商店。如果没有您要添加的商店，请先在**商店**菜单上添加商店。
* **item名称** 输入您在添加商店后收到的商品信息。使用游戏中添加的item名称在app中显示。
* **商店商品ID** 输入您要添加的item名称。
* **使用与否** 选择是否出售该item。

### Modify

您可以在查询列表中查询已添加item的详细信息或更改信息。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Item3_1.0.png)
- 在查询列表中选择各item，则可以查询已添加item的详细信息。
- 点击**编辑** 按钮可以更改除商店和 item以外的信息。
- 点击**删除** 按钮可以删除item信息。

## Transactions

可以查询结算信息。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction1_1.4.png)

您可以使用以下搜索条件查询所需的付款信息。
您可以随时点击右上角的“下载”按钮下载付款详细信息。
#### 搜索条件

- **商店**: 已付款的商店信息
- **日期**: 用户尝试购买的时间
- **付款编号**: 用于区分Gamebase内支付的唯一编号
- **item编号**: 用户在APP中购买的实际item编号（item编号可在“item”标签上确认）
- **用户ID**: 付款的用户ID
- **排列顺序**: 以记录时间为基准，进行升降排序
- **付款状态**: 根据付款状态查看信息

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

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_1.0.png)
您可以通过输入付款时收到的**发票编号**, **价格**, **货币**信息来更改状态。

##### Refund 变更
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction2_2.0.png)
不必输入其他追加信息，确认状态后选择变更即可。
您更改的付款信息无法再次更改，因此需要仔细检查信息。
#### 영수증 검증
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_IAP_Transaction3.1.png)
* 조회된 영수증을 기반으로 해당 결제건이 유효한 지 검증할 수 있습니다.
* 각 필드의 비교결과를 알려주며 스토어로부터 받은 응답값을 Json형식으로 제공하므로 필요한 경우 데이터를 직접 확인하실 수 있습니다.
* 현재는 App Store 결제건에 대해서만 검증을 제공합니다.
