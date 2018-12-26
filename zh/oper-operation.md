## Game > Gamebase > 控制台使用指南 > 运营

在运营App时提供必要功能的菜单。

* 维护(Maintenance)：APP维护管理
* 公告(Notice)：以弹出窗口的形式提示游戏用户的紧急公告的管理

## Maintenance


![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance1_1.1.png)

如果您需要维护游戏，可以轻松的在Console中设置。
可以一目了然地查询已添加App的维护历史记录，和维护设置内容以及进度状态。
维护状态分为以下五种类型。

(1) 已预约：预约计划进行的维护
(2) 维护中：正在进行的维护
(3) 结束：维护正常结束
(4) 解除维护：指管理员在维护中的状态下强制 **解除维护**
(5) 解除维护(到期)：指已**解除维护**，并预约的维护也已到达结束时间

Gamebase提供维护弹出窗口和详细信息的页面，以便在维护期间向游戏内的用户公告。
Gamebase的默认维护弹出窗口如下：
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_popup_1.0.png)
Gamebase提供的默认维护网页(显示维护原因和维护时间）如下：
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance_webview_1.1.png)


### Register Maintenance

点击**维护** 选项卡上的**添加** 按钮，进入设置维护界面。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_1.4.png)

>  <font color="red">[注意] </font>**同时设定强制更新与维护**时，服务状态为 “强制更新”。
> 如果您不希望用户在维护过程中看到“强制更新”的弹出窗口，则应在维修完成后将服务状态更改为“强制更新”。

#### (1) 对象
选择需要维护的对象。

- 所有游戏：所有客户端版本都需要维护时选择。
- 部分客户端：仅当特定客户端版本需要维护时选择。点击“选择版本”，可以查看在客户端菜单中，登记的客户端版本列表。
  **[选择部分客户端的页面如下]**
  可以按客户端状态和商店类别进行全部选择。选择要维护的客户端版本后，点击“确定”按钮。
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)

#### (2) 原因
输入维护原因。
此信息不会向用户公开，只需简单输入设置维护的原因即可。

#### (3) 时间
设置维护进行时间。
默认情况下，时区为‘UTC + 09：00’，可以通过选择服务国家的时区来设置维护。

#### (4) 维护页面
设置要提供给用户的维护页面的类型。
可以在**Gamebase提供的页面（WebView）**，**自定义HTML（WebView）**和**外部页面** 中选择。
每个页面的其他项如下所示。输入的内容可以通过点击**预览**按钮确认。

##### 1)Gamebase提供的页面（WebView）
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_2.0.png)
这是默认格式维护页面，显示管理员在Gamebase提供的webview页面上输入的信息。
在**公告**中，输入在维护过程中向用户推送的消息。
信息也可以用英语，日语和中文等外语输入，选择的语言将设置为“默认语言”。
如果在已登记信息中没有匹配的语言，将显示选择的“默认语言”。点击右侧的 ** + ** 按钮可以添加语言。如果没有所需语言，通过 [客服中心](https://alpha.toast.com/support/inquiry)与我们联系，即可添加新语言。点击**预览**，以“默认语言”查看预览页面。

##### 2) 自定义HTML(WebView)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_3.0.png)
管理员以HTML格式手动输入维护页面，并将其提供给用户。
它还支持基于输入的HTML标签的预览页面。
当您想要创建所需的维护页面格式时，这非常有用。
> [参考]
> HTML页面服务预计于2018年2月之后提供。

##### 3) 外部页面
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_4.1.png)
如果您有自己的维护页面或维护模板，则可以将维护页面链接到该URL。
支持所链接URL的外部页面预览。
如果要通过单独输入维护信息，让外部URL来接收，请选择**提供维护信息**项，并在**显示消息**中输入信息。 您可以在维护页面上接收Gamebase上设置的维护信息（维护时间、消息等）。
传递参数如下。所有维护信息，以URL编码传递。

- message：根据设备信息中设置的语言的维护信息。对于预览，将提供默认选定的消息。
- timezone：设置维护时选择的时区信息。例)‘UTC+9’，则传递‘- +09：00’ 
- beginDate：设置维护时输入的开始时间
- endDate：设置维护时输入的结束时间

### Modify Maintenance
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance3_1.3.png)

可以查看、修改和删除已设置维护的详细信息。
修改维护的输入项与设置维护页面的基本相同。当维护信息不正确时，可以通过点击删除按钮来删除维护。
如果要使用类似信息重新设置新维护，可以通过复制功能钮，轻松地设置新维护。

## Notice

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice1_1.2.png)

提供App启动时以弹出窗口方式的公告。因为是在登录前弹出的公告，所以在发生外部身份验证失败或游戏服务器故障时，可设置使用。
可以一目了然地查看已设置的公告列表和进行情况，并可以按公告信息进行搜索。

公告状态分为以下三种类型：

(1) 已预约：预约计划发布的公告
(2) 发布中：正在发布中的公告
(3) 结束：公告发布结束

### Register Notice

点击主页上的“添加”按钮转到设置公告页面。

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice2_1.0.png)

#### (1) 对象

选择要发布公告的对象。

- 所有游戏：所有客户端版本都需要维护时选择。
- 部分客户端：仅当特定客户端版本需要维护时选择。点击“选择版本”，可以查看在客户端菜单中，登记的的客户端版本列表。
  **选择部分客户端的如下**
  可以按客户端状态和商店类别进行全部选择。选择了要维护的客户端版本后，点击“确定”按钮。
  ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance4_1.3.png)


#### (2) 选择国家
选择要发布公告的国家。

- 所有国家：向所有用户发布公告
- 部分国家：仅向所选国家的用户发布公告。
输入要添加的国家代码，它将自动完成。如果想选择的国家代码不存在，请您可以通过 [客服中心](https://toast.com/support/inquiry)与我们联系。

> [注意]
> 国家标准
> 以用户的 **USIM国家代码** 来判断，如果没有USIM，公告将根据 **Device**中设置的国家发布。

#### (3) 显示次数
选择公告对用户显示的次数。

- 期间内显示一次：公告期间内共显示一次。
- 启动App时始终显示：在公告期间内，每次用户启动App时，始终显示公告。

#### (4) 显示时间
设置公告的显示时间。
时区默认选择“UTC + 09：00”，可根据该国家时区来设置维护。

#### (5) 公告信息
输入向用户显示的公告信息。
可以使用多种语言输入信息，选中的语言将被设置为“默认语言”。
在语言列表中如果没有用户匹配语言，将显示选择为“默认语言”的语言。点击右侧的 ** + ** 按钮可以添加语言。如果没有所需语言，通过[客服中心](https://toast.com/support/inquiry)与我们联系，添加新语言。


#### (6)底部按钮类型
指定要在公告弹屏底部显示的按钮类型。

- 关闭：仅显示关闭按钮。
点击“关闭”按钮则关闭弹出窗口并继续游戏。

- 关闭+详细信息：显示“关闭”和“详细信息”按钮。
当用户点击“详细信息”按钮时，在WebView中打开在 Console中输入的链接。

#### 紧急公告弹出窗口示例
(1) 关闭按钮
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_1.1.png)
(2) 关闭+详细信息
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice_popup_close_detail_1.0.png)

### Modify Notice
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Notice3_1.1.png)

可以查看，修改和删除已设置公告的详细信息。
修改公告的输入项与设置公告页面的基本相同。当公告信息不正确时，可以通过点击删除按钮来删除公告。
如果要使用类似信息重新设置新公告，可以通过复制功能，轻松地设置新公告。