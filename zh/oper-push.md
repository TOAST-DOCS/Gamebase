## Game > Gamebase > 操控台使用指南> Push



可向应用程序用户发送推送通知。

在Gamebase中通过使用NHN Cloud Push服务来发送推送通知。 

## Push
> <font color="red">[注意]</font><br/>
>
> 正在服务的游戏应用**Gamebase SDK 2.6.0以上**版本时使用的菜单。

可确认发送推送的历史和创建的推送预约列表。

![gamebase_push_01_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_01_201910.png)

### Registered List
在预约列表中选择推送，可确认推送的预计发送时间及创建信息，单击**取消传输**按钮，可取消预约发送件，或单击**复制**按钮，可利用创建的推送的创建信息新建推送。

### Send History

在发送历史列表中选择推送，可查询传输的推送的详细明细。
单击**复制**按钮，可利用发送的推送创建信息轻松创建推送。

![gamebase_push_03_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_03_202101.png)

### Register Push 

若欲创建新推送，单击**注册**按钮。
可以通过右侧的预览UI确认控制台中注册的值在实际终端机中如何显示。

![gamebase_push_05_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_05_202101.png)

#### (1) 发送类型

选择发送类型。

- **立即发送** : 注册时立即发送推送消息。
- **预约发送** : 在预约的时间发送推送消息。如果要以用户终端机设置的国家时间为准发送推送消息，要选择**以当地时间为准发送**。当选择**以当地时间为准发送**，将时间设置为”2017-01-01 09:00”时，使用韩国终端的用户在韩国时间”2017-01-01 09:00”，使用英国终端的用户在英国时间”2017-01-01 09:00”接收推送的消息。
- **重复** : 定期发送同样的推送消息。如果要以用户终端机设置的国家时间为准发送推送消息，要选择**以当地时间为准发送**。选择发送时期和时间。以**每天**、**每周**、**每月**为准输入发送周期。 
	- 每天 : 每天发送推送消息。
	- 每周 : 选择要发送推送的星期发送推送消息。可以重复选择星期。
	- 每月 : 以一个月为准，在愿意发送的日期推送消息。您可输入的日期为1~31，而要输入一个或一个以上的日期。例如，如果要在1号、5号和10号发送，则要输入1、5、10。

#### (2) 目标
选择要发送推送消息的对象。 
- **全部发送** : 按OS类别选择。正在使用您选择的OS的用户都会收到消息。 
- **特定会员发送** : 要向指定会员发送推送消息时选择。向使用输入的用户ID注册推送令牌的终端机发送推送消息。指定的用户在5个终端玩游戏时，向5个终端发送推送消息。 
- **组发送** : 以文件形式注册将要发送推送的用户列表。向小组发送时一次最多只能发给10,000名用户。
- **Tag** : 使用标签发送推送消息。使用注册在**推送 - 标签**菜单中的标签发送，而可同时注册多个标签。标签过滤功能如下。
	- AND : 只向符合所选Tag的用户发送推送消息。 
	- OR : 向至少符合所选Tag的一个条件的用户发送推送消息。

#### (3) 事件键
选择用于发送推送统计的事件键。 
点击**选择**按钮，将出现选择事件键的弹窗，而可以选择**正在搜集**的事件键。
![gamebase_push_22_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_22_202101.png)

#### (4) 目标国家

选择要发送推送消息的国家。 
- **所有国家** : 向所有的用户发送推送消息。
- **部分国家** : 向所选的国家用户发送推送消息。
  如果输入要添加的国家名，则自动输入。如果没有您要输入的国家，请联系[客户服务](https://toast.com/support/inquiry)。

> [参考]
>
> 国家判断标准
> 以用户的**USIM国家代码**为准进行判断，无USIM时，以为终端机设置的国家为准显示推送信息。

#### (5) 消息类型

> [参考]
>
> 为遵守韩国的“信息通信网法”而提供的功能。
> 发送推送时，若不是信息性信息，应以“（广告）”开始信息，信息中应含有联系方式及取消接收方法。

- **宣传性**：在输入的信息中添加“（广告）”序言。并且，添加输入的联系方式及取消接收方法一同发送。对于全部发送等广告性推送，在韩国国内，为遵守信息通信网法，必须作为“宣传性”信息发送。
- **信息性**：仅输入的信息通过推送发送。对于非韩国终端机（USIM国家代码不是韩国）的用户，作为信息性信息发送即可。


> <font color="red">[重要]</font><br/>
>
> 选择**宣传性**后在输入信息中输入“（广告）”语句时，“（广告）”语句会重复，因此请注意。

#### (6) 消息信息

信息可利用多种语言输入，对于使用输入的语种以外语种的用户，以选择为默认语种的语种显示。若欲添加语种，单击下端的**添加信息**按钮。若欲添加列表中没有的语种，请联系[客服中心](https://toast.com/support/inquiry)。

> [参考]
>
> 发送了推送信息，但终端机未收到信息时
> 大部分情况下是未注册用户的推送令牌。请确认是否已注册用户的推送令牌。
> 在平台上注册推送令牌的方法，请参考以下文件。
选择“翻译”按键，可将默认语言翻译成设置的语言。
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)

#### (7) 留言颜色(Only Android)

对于Android，可指定终端机中显示的推送信息的文字颜色。
可分别指定标题和内容的颜色，可在文字颜色选择窗口中选择颜色或直接输入RGB Hex值指定颜色。
选择的文字颜色可通过右侧的预览界面确认。

#### (8) 提示音(可选项目)

可设置终端机接收推送时播放的提示音。
单击**添加提示音**按钮，可设置提示音，若不设置，则播放默认提示音。
输入外部URL链接或应用程序中内置的提示音文件路径。

#### (9) 大图标(Only Android)

单击**添加大图标**按钮，可设置接收推送时右侧显示的图标的图像。
设置大图标图像并发送推送时，仅Android终端机显示图像，iOS中不显示。
选择外部，输入要显示的图标图像的外部URL，或选择内部，输入应用程序中内置的图标图像文件路径。
为外部图像时，可在右侧预览中提前确认相应图标。

#### (10) 按键

接收推送时的信息下方显示的按钮最多可设置3个，一同发送。
有4种形态的按钮，各类型运行方式及设置的值不同，因此请参考如下指南设置。
- 打开应用程序：在推送信息中单击按钮，即可运行相应应用程序。仅可指定按钮名。
- 打开URL：在推送信息中单击按钮，即可移动至在控制台中输入的URL。可通过应用程序架构运行特定应用程序或连接网页。
- 响应：在推送信息中单击按钮，即可运行能够响应的窗口。对于iOS，可一同设置传送按钮名。
- 关闭，在推送信息中单击按钮，即可关闭推送。仅可指定按钮名。


#### (11) 媒体(iOS)

可添加接收iOS推送时动态执行的媒体。
可设置**IMAGE**、**GIF**、**VIDEO**、**AUDIO**项目，可输入欲连接的外部URL或内部文件路径。


#### (12) 媒体(Android)

可添加接收Android推送时执行的媒体。
目前Android中仅可设置**IMAGE**项目，可输入欲连接的外部URL或内部文件路径。


## Statistics

可通过表和图表确认与推送消息、令牌、”接收设定”有关的指标。 
统计菜单项目如下。

- 发送/接收 : 与发送或接收推送消息相关的指标 
- 注册令牌 :  与注册或删除推送令牌相关的指标 
- 接收设定 : 与接收推送的设置相关的指标

### 发送/接收

![gamebase_push_12_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_12_202101.png)


1. 发送/接收统计 

显示在所选期间推送消息的”发送/接收”统计。

* 发送比率 : 在相关时期成功发送的次数 / 发送的总次数
* 接收比率 : 在相关时期成功接收的次数 / 成功发送的总次数
* 接收确认率 : 在相关时期已确认接收的次数 / 成功接收的总次数

2. Push发送列表
在所选的时期发送推送消息的列表。

3. 按时间划分的数据
显示在所选的时期按时间划分的”发送推送、”发送失败”、接收并确认接收的指标。 

* 只有在所选的时期不到24小时，才能选择**分**单位。 

### 注册令牌

![gamebase_push_13_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_13_202101.png)

1. 注册令牌统计

显示在所选时期注册或删除令牌的统计。 

* 类型 : 按令牌类型划分的注册占有率
* 国家 : 按用户国家划分的令牌注册占有率
* 语言 : 按用户语言划分的令牌注册占有率

2. 国家 
显示在所选时期按用户国家划分的令牌的注册和统计的删除。

3. 语言
显示在所选时期按用户语言划分的令牌的注册和统计的删除。

### 接收设定
显示与在所选时期接收设定有关的统计。
![gamebase_push_14_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_14_202101.png)
 
## Event Key
可管理推送发送统计时使用的Event Key。
![gamebase_push_15_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_15_202101.png)
可注册通过Push发送消息时使用的Event Key。

### Event Key register
![gamebase_push_16_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_16_202101.png)

### Event Key detail
可以管理已注册的Event Key。
![gamebase_push_17_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_17_202101.png)

通过点击上端的**删除**、**更改**按钮，可修改或删除Event Key信息。 

## Authentication
可以管理推送发送时使用的认证证书。  
![gamebase_push_18_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_18_202101.png)

通过点击各认证证书的**注册**、**更改**、**删除**按钮，可注册、更改或删除认证证书。

### Authentication register
![gamebase_push_19_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_19_202101.png)

## Tag

提供可按照特定标准绑定用户进行传输的标签功能。

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_06_201910.png)

可以注册使用NHN Cloud Push发送推送消息时使用的标签名称。


### Tag register

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_09_201910.png)


### Tag detail

可管理注册的标签及注册于相应标签的用户列表。

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_07_201911.png)

通过上方的删除/修改按钮，可修改并删除标签相关信息，通过下方的用户ID管理功能，可在标签注册或删除用户。

#### 注册用户

##### 单件注册

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_08_201910.png)

##### 文件注册

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_10_201910.png)

按注册按钮，弹出如上注册弹出窗口，可直接输入ID或通过文件注册输入。

欲利用文件注册进行注册时，一次最多可注册1000人。

#### 删除用户

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_11_201910.png)

如果要删除在标签中注册的用户，则在用户列表中勾选左边的复选框后点击**删除**按钮。 


## Setting
可进行推送设置。
![gamebase_push_20_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_20_202101.png)

### 设置接收和确认消息
是收集已发送的消息的接收和确认信息。

### 邮件保护设置重复
即使要求多次发送同一条消息，也不会在设置的时间内发送。

* 可以设置为1分钟到60分钟，增量为1分钟。

### 设置广告展示位置
可以设置发送广告性消息时显示的广告展示位置。

* 点击右边的**预览**按钮时，您可以看到根据设置的广告展示位置推送消息的示例。

![gamebase_push_21_202101](https://static.toastoven.net/prod_gamebase/gamebase_push_21_202101.png)
 
### 代币设定

可以设置令牌的过期时间和应用程序类型。 

* 代币的有效期 : 可以设置为1天到730天。
* 应用程序类型
	* 多个令牌 : UID(用户ID)可以具备多个令牌。是一个用户可同时在多台设备上使用的应用程序。
	* 单令牌 : UID只可以具备一个令牌。是一个用户可只在一台设备上使用的应用程序。

### 保存运输记录

是将发送消息的记录保存在NHN Cloud Log & Crash Search中的功能。

* AppKey : 是Log & Crash Search的应用程序key。
* Log Source : 使用Gamebase提供的PUSH API，可在Log & Crash Search查看发送PUSH的记录。
* Log Level : 是将传输到Log & Crash Search的日志级别。
	* ALL : 将所有的日志传送到Log & Crash Search。
	* INFO : 过期的令牌和运送失败的日志将会被传送到Log & Crash Search。
	* ERROR : 运送失败的日志将会被传送到Log & Crash Search。
