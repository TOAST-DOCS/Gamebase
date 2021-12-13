## Game > Gamebase > 控制台使用指南 > 管理

可在使用Gamebase的游戏中使用查询权限管理、报警发送设置、查询报警记录等功能。

## Authorization

可以管理Gamebase Console的使用权限。 
![gamebase_manage_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_01_202106.png)

* Gamebase Console使用权限管理
  * **接收周报的权限** : 是否接收**周报**的权限
* 如需注册新成员，要在NHN Cloud项目成员管理中进行添加。 
* 不能修改自己的权限。


## Alarm

您可以使用Gamebase的报警功能接收有关游戏用户增减率，最低同时在线用户数变化的报警。

### Alarm

![gamebase_manage_02_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_02_202106.png)

#### (1) 启用减少报警
设置同时在线用户数减少时是否接受报警。如需接收报警将**启用减少报警**设置为**On**。

- **减少率** : 指定接收报警的同时在线用户数减少百分比。
- **维护时忽略同时在线用户数减少报警** : 如果您在维护应用程序，同时在线用户数会减少。
  在这种情况下，您可以将**忽略维护引起的减少报警**设置为**On**，拒绝接受报警。

#### (2) 启用增加报警
您可以设置同时在线用户数增加时接受报警。
启用该功能后，您可以设置管理员需要接收报警的值。

#### (3) 消息语言
您可以选择报警发送的语种。目前仅支持韩语和英语消息，以后根据需求将会添加其他语言。

#### (4)最低同时在线用户数
当同时在线用户数少于指定的**最低同时在线用户数**时您可以收到报警。例如，**最低同时在线用户数**设置为500名，如果同时在线用户数低于500，您将收到报警。最低设定值为100，不能设定小于100的值。

### Alarm Log

报警记录位于报警菜单下，您可以查询报警的历史记录。
最多可以搜索30天以内的报警记录。查询后单击**Search**按钮，可以实时筛选。

![gamebase_manage_03_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_03_202106.png)

- 发生时间：发送报警的时间信息
- 历史同时在线用户数 : 在发送报警之前收集的同时在线用户数
- 新的同时在线用户数 : 发送报警的瞬间收集的同时在线用户数
- 变化率（设定值）：前面提到的变化率是历史同时在线用户数与新的同时在线用户数对比信息。设定值是为了发生报警时发送而设定的值。

### Webhook
除了Gamebase提供的默认SMS/Email外，还提供可以单独接收报警的Webhook设置功能。
如果有通过外部系统的Webhook URL发出报警的请求，则会同时发送报警。

#### (1) 查询列表
![gamebase_manage_04_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_04_202106.png)
可以查看可接收报警的Webhook注册历史记录。
当需要已注册的Webhook URL，通过点击右边的**复制URL**进行复制。 

#### (2) 注册
![gamebase_manage_05_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_05_202106.png)
通过点击**注册**按钮，可注册从外部系统接收的Webhook信息。 
目前只能注册Dooray和Slack，而请求时将计划添加新列表。

#### (2) 查询详细/修改/删除
![gamebase_manage_06_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_06_202106.png)
通过点击项目，可查看详细信息。
若要修改注册的信息，则点击**修改**按钮。若不需要相关Webhook，则可通过点击**删除**按钮来删除。 
 
### Recipient List

可以设置要发送报警的对象。如需注册新成员，则添加在NHN Cloud项目的成员管理中。
![gamebase_manage_07_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_07_202106.png)
可通过Gamebase发送报警邮件和SMS。 
将根据加入NHN Cloud时输入的信息发送电子邮件和SMS。如果邮件地址或号码不正确，无法接收警报。可以在NHN Cloud的**我的信息管理**页面上确认手机号码信息。


## Config

设置与Gamebase和NHN Cloud服务联动有关的设置。 

![gamebase_manage_08_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_manage_08_202106.png)

调用Gamebase Launching API时可以设置是否要一起接收在NHN Cloud Launching中设置的信息。只能在使用NHN Cloud Launching服务时开启或关闭功能。 
