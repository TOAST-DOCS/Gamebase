## Game > Gamebase > 控制台使用指南 > 客户服务

运营游戏时，可处理来自客户的1:1咨询，并在客户服务网页上管理公告和FAQ设置。
处理客户咨询时，您可将发给客户的”电子邮件设定"和"常问问题答案”注册为模板使用。 
> [参考]
> 如果要使用此菜单，请在”应用程序 - 客户服务”设置中的选项中选择Gamebase提供的客户服务。
>

## Help Center Web Page 

描述客户服务网页的内容。 
客户可在网页上注册1:1咨询，查看自己的咨询记录、常见问题及公告。

### Main 

在游戏中通过Gamebase SDK向用户显示以下客户服务网页。
![main](http://static.toastoven.net/prod_gamebase/gamebase_help_center_00_20201125.png)

#### (1) 1:1咨询

点击**1:1咨询**按钮，将跳转到注册1:1咨询的页面。 
![咨询](http://static.toastoven.net/prod_gamebase/gamebase_help_center_01_20201125.png)

如果要注册咨询，需要输入以下项目。 
您可在**[客户服务 > 客户咨询](./oper-customer-service/#inquiry)**控制台中确认来自客户的咨询，并给客户提供答案。 

1. 咨询类别 : 选择要接待的咨询类别。咨询类别可在[客户服务 > 客户咨询](./oper-customer-service/#inquiry)中进行注册、修改或删除。
2. 回复邮件 : 输入要提供答案的电子邮件地址。当您在控制台中处理客户咨询或回复邮件时，将答案自动发送到指定客户的邮件地址。
3. 名字(网名) : 输入游戏网名（最多只能输入10个字符）。
如果将游戏网名设置为追加信息，打开客户服务网页时不用直接输入网名，网名将自动被输入。
4. 标题 : 输入咨询标题（最多只能输入100个字符）。
5. 咨询内容 : 输入咨询内容（最多只能输入1,000个字符）。
6. 附加文件 : 如有请上传附件。最多可以附加5个小于10MB的文件。 

#### (2) 我的咨询记录

访问客户服务网页时先要登录，才能看到**我的咨询记录**按钮。点击按钮，可确认客户咨询。
![我的咨询记录_登录](http://static.toastoven.net/prod_gamebase/gamebase_help_center_02_20201125.png)

最多可查看10个历史记录。若想要看更多，点击**更多**按钮则再显示10个。

> [参考] 若未登录，则不能确认我的咨询记录。
> 如果在未登录的状态下注册咨询，只能通过电子邮件接收答案，无法在咨询记录列表中确认。
> ![我的咨询记录_未登录](http://static.toastoven.net/prod_gamebase/gamebase_help_center_03_20201125.png)
 
#### (3) 常问问题

可在FAQ中确认咨询类别及常见问题。列表最多展示12个。
通过查看信息或点击”类别”按钮，可在[客户服务 > FAQ](./oper-customer-service/#faq)中确认注册的FAQ内容。 
![FAQ](http://static.toastoven.net/prod_gamebase/gamebase_help_center_04_20201125.png)

1) 通过输入搜索关键词来确认包含该关键词的FAQ。
2) 可以查看常问问题。
3) 注册FAQ时按照注册的**FAQ类别管理**确认FAQ。
4) 可在[Gamebase Console > 客户服务 >  FAQ类别管理](./oper-customer-service/#search-faq)中添加或删除FAQ类别。 

#### (4) 公告
可以在**客户服务 > 公告**中查看公告。
   
在主页上显示3个帖子，并以粗体显示固定在顶部的帖子。通过点击**more**按钮，可将公告的内容全部显示出来。
以注册日期为准，按降序排序显示帖子，并以粗体先显示固定在顶部的帖子。若帖子的显示时期到期，则不显示。通过点击帖子，可以查看详细信息。
![公告](http://static.toastoven.net/prod_gamebase/gamebase_help_center_05_20201126.png)

## Inquiry 
可以查询或处理来自客户的1:1咨询。
除此之外，还可以设置注册咨询时所需的查询类别，并通过使用发送Push的功能，通知客户回复咨询。

### Search Inquiry
 
可以查询符合搜索条件的咨询记录。
 
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_01_202009.png)

**搜索条件**

- **状态** : （必选）选择处理客户咨询的状态。
- **咨询类别** : （必选）选择客户咨询类别。从接收 / 已处理选项当中选择。
- **接待日期** : （必选）搜索在所选期间接收的客户咨询。
- **用户ID** : 需要查看指定客户咨询时输入。

**搜索结果**

- **咨询类别** : 客户注册咨询时选择的类别项目
- **咨询标题** : 客户注册咨询时输入的标题
- **接待日期** : 客户注册咨询的日期
- **解决日期** : 负责人接收咨询并提供答案的日期
- **状态** : 处理客户咨询的状态

#### 1. 管理咨询类别
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_02_202009.png)

可以管理客户注册咨询时选择的类别选项。
可以按支持语言类别进行注册（每个项目最多只能输入20个字符）。
按照显示顺序向用户显示列表。利用鼠标拖放功能可修改列表顺序。
> [参考]
> 若要查看支持语言的选择现状，请在“应用程序 - 客户服务‘’中确认。 

#### 2. 答案发送设定
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_05_202011.png)


处理客户咨询后通过Push消息向用户发送通知时，可通过使用此功能来发送通知。
如需使用，则中选上端"发送与否”，向用户发送Push通知，通知客户客户咨询已被回复。
如果是全球服务，可以再添加语言后发送，并可根据符合用户机器设置的语言向用户发送推送通知。
> [参考]
> 1. 为了使用此功能，先要激活NHN Cloud Push商品。      
> 2. 选择”答案发送设定”语言时，如果是Gamebase支持的语言，即使客户服务不支持也可进行注册。


### Inquiry details
   
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_03_202009.png)
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquir   ；‘’y_04_202009.png)

可以确认来自客户的咨询并对此进行处理。

选择模板时 - 可将答案模板菜单中设置的模板内容直接提供为答案，并通过使用Text editor，按所需的方式编辑模板上的内容之外的答案后提供给客户。
给客户提供答案时，若需上载文件，最多只能附加5个小于10MB的文件。
处理咨询后，将负责人提供的答案发送到客户注册的电子邮件地址。
发送邮件时通过使用“答案发送”选项，可以确认客户是否接收Push通知。
> [参考]
> ![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_06_202011.png)
> 当客户登录后注册咨询时，可在一个页面上查看该用户的信息。
> 因可通过以前成员菜单中使用过的功能查看客户信息，可以在同一个页面上处理客户咨询。

#### 1. 答案发送设定
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_05_202009.png)

处理客户咨询后通过Push向用户发送通知时，可通过使用此功能来发送通知。
如需使用，则中选上端"发送与否”，向用户发送Push通知，通知客户咨询已被回复。
如果是全球服务，可以再添加语言后发送，并可根据符合用户机器设置的语言向用户发送推送通知。
> [参考1]
> 1. 为了使用此功能，先要激活NHN Cloud Push商品。 
> 2. 选择”答案发送设定”语言时，如果是Gamebase支持的语言，即使客户服务不支持也可注册。

> [参考2]
> 查看已处理完的客户咨询时，可同时查看咨询内容和处理记录。如有要注册的附件，请点击下载。  

> [参考2]
> 查看处理完的咨询时，可同时查看客户咨询记录和处理完的咨询记录。如有要注册的附件，请点击下载。      

## FAQ

可以管理客户服务的FAQ项目。 
### Search FAQ

可以查看注册的FAQ项目。 

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_07_202009.png)

**搜索条件**

- **状态** : 选择FAQ的显示形式。从显示 / 不显示当中选择。
- **类别** : （必选）进行查询时，请选择FAQ类别。将以FAQ类别管理中注册的内容为准显示选择项。        
- **提问+答案** : 查询客户提问或在答案中包括相关关键词的FAQ时使用该选项。若要设置用其他语言注册的内容，请先指定语言后再进行搜索。 

**搜索结果**

- **常问问题** : 显示常问问题列表是否包含FAQ。 
- **类别** : 显示注册的FAQ类别。 
- **标题** : 是FAQ提问的标题。
- **修改用户** : 显示上一次注册或修改FAQ的用户信息。 
- **修改日期** : 显示上一次注册或修改FAQ的日期。
- **状态** : 是否在页面上显示FAQ。可以从显示 / 不显示当中选择。

#### 管理FAQ类别
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_09_202009.png)

可以管理注册或修改FAQ时选择的类别。
可按支持语言类别进行注册（每个项目最多只能输入20个字符）。
按显示的顺序显示，并可用鼠标拖放功能修改列表顺序。 
> [参考]
> 可在”应用程序 - 客户服务”设置中确认支持语言现状。

### Register or Update FAQ
可以注册FAQ或修改以前注册的FAQ信息。
注册或修改时，只能修改同样的项目。

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_08_202009.png)

#### 1. 状态
选择是否显示要注册或修改的FAQ。
从显示 / 不显示选项中选择是否在客户服务页面上显示FAQ。 

#### 2. 类别
以在FAQ类别管理中注册的类别为准选择需要注册或修改的FAQ类别。

#### 3. 常问问题
选择是否将特定客户提问添加在客户服务网页上的常问问题栏中。

#### 4. 提问
输入FAQ提问内容。 
> [参考]
> 注册时必须输入”应用程序 - 客户服务”中的所有支持语言。

####. 5. 答案
输入FAQ答案。 
通过使用Text Editor可以按自由形式编辑答案后显示。 
> [参考]
> 注册时必须输入”应用程序 - 客户服务”中的所有支持语言。

## Notice

可以管理客户服务网页显示的公告。

### Search Notice

可以查看注册的公告列表。

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_10_202009.png)

**搜索条件**                         

- **查询类别**:（必选）选择公告的查询类别。状态基本为默认项，若要以显示日期为准进行搜索，则选择相关项目后以相同的方式进行搜索。
- **状态** :（必选）是上述查询类别选项的默认项，要以显示当前的公告的状态为准进行搜索。从预定中 / 开了 / 关闭当中选择。
- **显示日期** : 在查询类别中选择显示日期后进行设置。并可查看按所选的显示日期被显示的公告列表。
- **标题+内容** : 查看标题或内容中包含特定关键词的公告时使用。如果要用其他语言注册，请先指定语言后再进行搜索。

**搜索结果**
- **最高固定** : 显示是否将公告固定在上端的固定框上。
- **类别** : 显示公告类别。
- **标题** : 公告标题
- **显示日期(UTC+9)** : 显示公告的注册日期（显示日期）。
- **显示时期** : 显示公告的显示时期。
- **状态** : 是否要显示公告。从预定中 / 开了 / 关闭中选择。

#### 管理开头词语
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_11_202009.png)

可以管理注册或修改公告时选择的开头词语。 
可按支持语言类别进行注册（最多只能输入20个字符）。
以显示的顺序显示，并可使用鼠标拖放功能修改列表中的顺序。
> [参考]
> 可以在‘’应用程序 - 客户服务‘’设置中确认支持语言的选择现状。

### Register or Update Notice
可以注册新的公告或修改以前的公告信息。
注册或修改时只能修改同样的项目。 

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_15_202009.png)
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_16_202009.png)

#### 1. 显示时期
设置要显示公告的时期。

#### 2. 显示时间
设置向用户显示公告的日期。

#### 3. 开头词语
选择公告的开头词语。

#### 4. 最高固定
通过将公告固定在上端位置，可一直显示。

#### 5. 标题
输入公告的标题。

#### 6. 内容
输入公告的内容。 
通过使用Text Editor可以按所需的方式编辑答案后显示。
> [参考]
> 注册时必须输入”应用程序 - 客户服务”中的所有的支持语言。

#### 7. 附加文件
注册公告时，可以上传附加文件。
最多可以附加5个小于10MB的文件。 
如果需要，请点击下载。

## Answer template

处理客户咨询时，若需要反复输入相同的答案，使用答案模板。

### Search Template
显示当前的模板列表。搜索模板时，请在右上端输入关键词。
![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_12_202009.png)

**结果**
- **模板名称** : 处理咨询时从模板选项中选择模板名称。
- **修改用户** : 显示上一次注册或修改答案模板的用户信息。
- **修改日期** : 显示上一次注册或修改答案模板的日期。

### Register or Update Template
可以注册答案模板或修改已注册的答案模板信息。
注册或修改时只能修改同样的项目。

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_13_202009.png)

#### 1. 模板名称
输入处理咨询时要在模板选项中显示的模板名称。

#### 2. 内容
处理咨询时，输入要在模板上显示的内容。 
通过使用Text editor可随意输入，如果处理时可适用同样的内容，通过选择模板反复适用即可。

## Email Config
可设置处理咨询后发送答案的电子邮件设定。
第一次激活时提供默认模板。 一旦被激活，每当修改时通过使用Text editor可随意修改。 

提供测试发送功能。使用此功能，可通过当前输入的模板内容提前确认客户接收的电子邮件形式如何。

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_customer_inquiry_14_202011.png)


> [参考]
> 若未在输入的电子邮件地址中设置SPF记录，该电子邮件可能会被视为垃圾邮件，
> 因此先要在DNS的TXT记录中注册以下值后设置发送地址。
> 附加值: v=spf1 include:_spfblocka.toast.com ~all
