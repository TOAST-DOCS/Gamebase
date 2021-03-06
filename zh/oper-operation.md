## Game > Gamebase > 控制台使用指南 > 运营

在运营App时提供必要功能的菜单。

* 维护(Maintenance)：APP维护管理
* 公告(Notice)：以弹出窗口的形式提示游戏用户的紧急公告的管理
* 图片通知(Image notice) : 以图像形式提示游戏用户的图片通知的管理

## Maintenance

![gamebase_op_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_01_201812.png)

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
![gamebase_op_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_02_201812.png)
Gamebase提供的默认维护网页(显示维护原因和维护时间）如下：
![gamebase_op_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_03_201812.png)

### Register Maintenance
点击维护选项卡上的**注册**按钮，进入设置维护的界面。

![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_op_04_202004.png)

>  <font color="red">[注意] </font><br/>
>  
>  **同时设定强制更新与维护**时，服务状态为“强制更新”。
>  如果您不希望用户在维护过程中看到“强制更新”的弹出窗口，则应在维护结束后将服务状态更改为“强制更新”。

#### (1) 目标
选择需要维护的对象。

- 所有游戏 : 所有客户端版本都需要维护时选择。
- 一些客户 : 仅当特定客户端版本需要维护时选择。点击“选择版本”，可以查看在客户端菜单中登记的客户端版本列表。
  **[选择部分客户端的页面如下]**
  可以按客户端状态和商店类别进行全部选择。选择要维护的客户端版本后，点击“确定”按钮。
![gamebase_op_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_05_201812.png)

#### (2) 原因
输入维护原因。
此信息不会向用户公开，只需简单输入设置维护的原因即可。

#### (3) 时间
设置维护进行时间。
默认情况下，时区为‘UTC + 09：00’，可以通过选择服务国家的时区来设置维护。
 
#### (4) 维护页面
设置要提供给用户的维护页面的类型。
可以在**Gamebase提供的页面（WebView）**、**自定义HTML（WebView）**和**外部页面**中选择，每个项目的输入框不同。
每个页面的其他项如下所示。输入的内容可以通过点击预览按钮确认。

##### 4-1) Gamebase提供的页面（WebView）
这是默认格式维护页面，显示管理员在Gamebase提供的WebView页面上输入的信息。
如果您没有自己的维护页面，这非常有用。
在**显示的消息**中输入在维护过程中向用户显示的消息。
消息可以用英语、日语及中文等外语输入，选择的语言将设置为“默认语言”。
如果在已登记信息中没有匹配的语言，将显示选择的“默认语言”。点击右侧的+按钮可以添加语言。如果没有所需语言，通过[客户服务](https://alpha.toast.com/support/inquiry)与我们联系，即可添加新语言。 
点击**预览**，以“默认语言”查看预览页面。

##### 4-2) 自定义HTML（WebView）
管理员以HTML格式手动输入维护页面，并将其提供给用户。
它还支持基于输入的HTML标签的预览页面。
当您想要创建所需的维护页面格式时，这非常有用。

##### 4-3) 外部页面
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Maintenance2_4.1.png)
如果您有自己的维护页面或维护模板，则可以将维护页面链接到该URL。
支持所链接URL的外部页面预览。
如果要通过单独输入维护信息，让外部URL来接收，请选择**提供维护信息**项，并在**显示消息**中输入信息。 您可以在维护页面上接收Gamebase上设置的维护信息（维护时间、消息等）。
传递参数如下。所有维护信息，以URL编码传递。

- message：根据设备信息中设置的语言的维护信息。对于预览，将提供默认选定的消息。
- timezone：设置维护时选择的时区信息。例)‘UTC+9’，则传递‘- +09：00’
- beginDate：设置维护时输入的开始时间
- endDate：设置维护时输入的结束时间

#### (5)推送消息
设置维护时推送的消息。
如果点击”翻译”按键，则将默认语言翻译成各项目的设置语言。

### Modify Maintenance

可以查看、修改和删除已设置维护的详细信息。
修改维护的输入项与设置维护页面的基本相同。当维护信息不正确时，可以通过点击删除按钮来删除维护。
如果要使用类似信息重新设置新维护，可以通过复制功能钮，轻松地设置新维护。

## Notice

![gamebase_op_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_06_201812.png)

提供App启动时以弹出窗口方式的公告。因为是在登录前弹出的公告，所以在发生外部身份验证失败或游戏服务器故障时，可设置使用。
可以一目了然地查看已设置的公告列表和进行情况，并可以按公告信息进行搜索。

公告状态分为以下三种类型：

(1) 已预约：预约计划发布的公告
(2) 发布中：正在发布中的公告
(3) 结束：公告发布结束

### Register Notice

点击主页上的“添加”按钮转到设置公告页面。

![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_op_07_202011.png)

#### (1) 对象

选择要发布公告的对象。

- 所有游戏：所有客户端版本都需要维护时选择。
- 部分客户端：仅当特定客户端版本需要维护时选择。点击“选择版本”，可以查看在客户端菜单中，登记的的客户端版本列表。
  **选择部分客户端的如下**
  可以按客户端状态和商店类别进行全部选择。选择了要维护的客户端版本后，点击“确定”按钮。
  ![gamebase_op_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_05_201812.png)

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
如果点击”翻译”按键，则将默认语言翻译成各项目的设置语言。

#### (6) 底部按钮类型
指定要在公告弹屏底部显示的按钮类型。
- 关闭：仅显示关闭按钮。
  点击“关闭”按钮则关闭弹出窗口并继续游戏。

- 关闭+详细信息：显示“关闭”和“详细信息”按钮。
  - 直接输入 : 当用户点击“详细信息”按钮时，在WebView中打开在Console中输入的链接。 
  - 联系客户服务 : 设置Gamebase提供的客户服务时, 用户可通过点击”详细信息”按键在WebView中打开客户服务。
  
#### 紧急公告弹出窗口示例
关闭按钮(左), 关闭+详细信息(右)
![gamebase_op_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_08_201812.png)

### Modify Notice
可以查看、修改并删除已设置维护的详细信息。
输入项与注册页面基本相同。当公告信息不正确时，可以通过点击删除按钮来删除公告。
如果要使用类似信息重新设置新维护，可以通过复制功能钮，轻松地设置公告。

## Image notice

![gamebase_image_notice_01_202007](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_image_notice_01_202007.png)

通过在控制台中注册图片，在游戏中可推送图片通知。
显示时期未到期的公告列表在顶级列表中显示，而已到期的公告列表在下端的单独的列表上显示。
可通过在未到期的公告列表中直接移动行设置显示顺序，并在游戏中通过调用显示API，在上端部分先显示注册的图片。在同一时间内最多只能显示5个图片通知。

#### properties
每个项目的内容如下。

- **图片通知** : 以缩略图形式显示需要提供的图片。注册后过14天图片将会被删除，删除之后缩略图显示默认图像。
- **原因** : 显示注册通知的原因。但此信息不会向用户公开。 
- **显示时间** : 设置显示通知的时间（显示注册人选择的时间和时区信息）。
- **显示时间(+09:00)** : 以韩国标准时间(+09:00)显示通知的显示时间信息。
- **更改日期** : 上一次修改图片通知的时间
- **点击率(%)** : 在游戏内显示通知的次数和用户点击通知的次数的比率。显示对整个比率的值，点击确认按钮，可以以图形形式查看显示周期的“日显示次数”和“点击次数”。 
- **状态** : 设置显示状态。每个状态如下。
![gamebase_image_notice_02_202007](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_image_notice_02_202007.png)

```
(1) 已预约 : 预约计划发布的图片通知
(2) 发布中 : 正在发布中的图片通知
(3) 结束 : 图片通知发布结束
```

### Register Image notice

您可以通过在**图片通知**列表中点击**注册**按钮来注册图片通知。
![gamebase_image_notice_03_202007](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_image_notice_03_202007.png)

#### (1) 目标

选择要显示图片通知的对象。 

- 所有游戏 : 需要显示在所有客户端版本时选择。
- 一些客户 : 只需显示在特定客户端版本时选择。点击”选择版本”按键，可以查看在客户端菜单中登记的客户端版本列表。
  **选择部分客户端的页面如下**
  可以按客户端状态和商店类别进行全部选择。选择要显示的客户端版本后，点击“确定”按钮。
![gamebase_op_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_op_05_201812.png)


#### (2) 目标国家
选择要发布通知的国家。

- 所有国家：向所有用户发布通知。
- 一些国家：仅向所选国家的用户发布通知。  
  输入要添加的国家代码，它将自动被输入。如果不存在您想要选择的国家代码，请通过[客户服务](https://toast.com/support/inquiry)与我们联系。

> [参考]
>
> 国家判断基准
> 以用户的**USIM**国家代码作为判断基准。若没有USIM，则以**终端机**设置的国家为基准显示通知。


#### (3) 原因
输入注册图片通知的原因。
不需要向游戏用户公开信息，因此只输入运营时所需的识别信息即可。 
 
#### (4) 显示时间
设置显示图片通知的时间。
时区的默认选项已设置为“UTC + 09：00”，也可以通过选择提供服务的国家的时区来设置维护。

#### (5) 点击后启动
设置用户点击图片通知时要处理的事件。
要设置的项目如下。

- **开启URL** : 输入要开启的URL。点击后在新的Webview中打开输入的URL，图片通知窗不自动关闭。
- **Payload** : 将输入的数据传送到客户端。如果使用接收的值点击图片，则可移动至游戏内页面或可处理特定事件。将会全部关闭您点击后打开的所有图片通知。
- **没有** : 即使点击图片通知，也不启动。

> [参考]
>
> 如果要在**开启URL**项目中使用**http**URL，需要在Android build中声明”域名除外”。
> 要不然因OS默认约束，Android 9.0以上终端机的页面将显示不正常。 

#### (6) 图片
注册要在游戏内显示的图片。
您可以设置每个语言类别的显示图片，并以终端机设置的语言显示图片。
支持JEPG, JPG, PNG格式（文件大小不得超过10MB）。
推荐尺寸为1200x900(Landscape), 900x1200(Portrait)。显示整个图片时要保持图片纵横比，如果横向或纵向长度过长，图片会显示不全。
点击预览时，可以再看到原有的图片。 

> [参考]
>
> 图片通知的显示时期到期后过14天自动删除上传的图片。

#### (7) 弹窗颜色主题
可以设置显示图片通知时的Webview颜色主题。
可以从**深色模式**和 **浅色模式**中选择颜色主题后保存。默认选项已设置为**深色模式**。

- 深色模式 : 弹屏底部颜色-灰色
- 浅色模式 : 弹屏底部颜色-白色

### Modify Image notice

可以查看、修改或删除已注册的图片通知的详细信息。
当您要换图片时，可以在修改页面重新注册图片，并可修改图片通知的显示时间和显示对象。
如果要重新注册类似于以前的图片通知内容的图片通知，利用复制功能钮可以重新注册图片。
