## Game > Gamebase > 控制台使用指南 > 禁用

提供禁用功能，限制不当使用应用或玩游戏的用户。
当被禁用的用户再次登录或恢复会话时，将显示禁止使用弹窗以限制游戏的使用。

可以在Gamebase Console中手动添加禁用，如果您使用的是TOAST AppGuard，也可以使用添加模板来自动添加禁用。

AppGuard的联动方法 ，请参考[AppGuard](./oper-ban/#appguard)。

## Ban

您可以查询禁用历史记录，添加禁用，解除已禁用的游戏用户。

### Search Banned User

根据搜索条件查询禁用/解除禁用的游戏用户列表。

![gamebase_ban_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_01_201812.png)

**搜索条件**

- **状态**:（必选）选择所需的状态（禁用/解除禁用）。不可两者都选，只能单选。
- **注册日期**: （必选）选择禁用添加的日期区间(from-to)
- **用户ID**: 禁用/解除禁用的 Gamebase 用户ID
- **模板**: 选择用于添加的特定模板查询
- **注册系统**: 选择添加禁用系统进行搜索，可以多选
- **Console**：通过Gamebase Console添加
- **AppGuard**: 联动AppGuard即可自动添加
- **外部服务器**: 添加运行应用程序的服务器或其他外部服务器
- **其他**: 除上述情况外，其他禁用添加（API直接调用等）

> [参考]
> 提供模板以轻松重用以多种语言向用户发布的消息。
> 您需要创建一个以上的模板，才可以添加禁用。
> 关于创建模板的方法，请参考[Template](./oper-ban/#template)。

**搜索结果**

- **用户 ID**: 禁用的游戏用户ID
- **期间**: 禁用期间。 如果是永久禁用，则显示为“永久禁用”
- **模板**: 用于添加禁用的模板
- **原因**: 管理员在添加禁用时输入的原因。 其原因不会向用户显示，只能通过操作记录确认。
- **添加人/添加日期**: 添加禁用的管理员/添加禁用的日期
- **解除原因**: 解除禁用时管理员登记的原因。其原因不会向用户显示，只能通过操作记录确认。
- **解除注册人/解除注册日期**: 解除禁用的管理员账户/解除禁用日期
- **解除**: 处于禁用状态的用户将显示**解除禁用**键，当点击该按钮，会弹出一个用于输入解除禁用原因的页面。您可以输入解除原因并点击**保存**按钮解除禁用。
- **状态**
  - <font color="white" style="background-color:#FB8F37">禁用</font>：游戏用户目前无法使用该应用
  - <font color="white" style="background-color:#A1A1A1">禁用（到期）</font>：游戏用户禁用时间已到期，但尚未登录。 用户重新登录时，游戏用户的状态被更改为解除禁用（到期）
  - <font color="white" style="background-color:#88C637">解除</font>：游戏用户已被管理员解除禁用
  - <font color="white" style="background-color:#2AB1A6">解除(到期)</font>: 在结束禁用后，游戏用户重新连接到游戏并且禁用自然解除的状态

> [参考]
> 点击**下载文件**按钮将搜索结果另存为CSV文件。

### Register Ban

点击禁用页面上的**添加**按钮，则可以添加禁用。

![gamebase_ban_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_02_201812.png)
#### (1) 用户 ID
输入Gamebase用户ID以添加禁用。 您可以一次添加多个用户，有两种添加方式。

- **输入用户**: 在输入窗口中直接输入要禁用的用户ID，然后按**Enter** 键或点击**添加**按钮。
- **批量添加**: 只能上传CSV文件。可以从Console 画面下载示例文件。批量添加一次最多可支持10,000人。
  ![gamebase_ban_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_03_201812.png)

> [参考]</br>
> 如果批量添加失败，将出现提示。您可以通过点击提示页面中的**Download**按钮下载添加失败的用户列表文件。

> ![gamebase_ban_04_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_04_201812.png)

#### (2) 期限
设置游戏用户的禁用期限。 添加禁用后此游戏用户无法登录。

- **永久禁用**: 选择此项以永久禁用。
- **指定期限**: 输入禁用期限。通过日期(day)和小时(hour)**预计到期时间**信息，可以提前确认用户的禁用期限。

#### (3) 原因
输入用户被禁用的原因。
其原因不会向用户显示，只能通过操作记录确认。

#### (4) 公布消息
输入向用户显示的禁用消息。
提供模板方便以多种语言输入消息显示给用户并重复使用。 选择预先添加的模板进行添加。

> <font color="red">[重要]</font>
> 您只能在添加了显示消息模板的前提下，才能完成添加禁用。
> 如果您没有添加模板，请先在**BAN**菜单的**Template**标签中添加模板。
> 添加Template方法请参考 [Template](./oper-ban/#template)。

#### (5)删除Leaderboard
设置是否在添加禁用时删除用户的 Leader board 。
选择后，添加时将从Leader board 删除用户的数据<font color="red">请注意，删除后该数据将无法恢复。</font>

### Release Ban

您可以通过点击禁用页面上的 **解除** 按钮解除禁用。

![gamebase_ban_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_05_201812.png)

#### 解除原因
输入解除用户禁用原因。
其原因不会向用户显示，只能通过操作记录确认。

#### 用户 ID
输入要解除禁用的Gamebase用户ID。 您可以一次添加多个用户，有两种添加方式。

- **用户输入**：在输入窗口中直接输入要添加禁用的用户ID，然后按**Enter** 按钮或点击**添加**按钮。由于检查用户ID的有效性，因此无法输入无效的用户ID。
- **批量注册**：只能上传CSV文件。可以从Console 画面下载示例文件。批量添加一次最多可支持10,000人。
![gamebase_ban_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_06_201812.png)

> [参考]
> 如果批量添加失败，将出现提示。您可以通过点击提示页面中的**Download**按钮下载添加失败的用户列表文件。
> ![gamebase_ban_04_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_04_201812.png)

## Template
提供模板轻松以多种语言输入消息显示给用户。 选择预先创建的模板。
您可以按语种添加，如果您禁用该用户，将根据设置的语言显示禁用消息。

### Search

您可以搜索已创建模板列表。
您可以创建新模板，修改已创建的模板，不能删除已创建的模板。


![gamebase_ban_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_07_201812.png)

- 在模板列表页面中，公布消息显示创建模板时作为“默认语言”输入的消息。

### Register Template
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_ban_08_202004.png)

#### (1) 名称
输入禁用在列表中显示的模板的名称。

#### (2) 公布消息
输入向用户显示的禁用消息。
您可以使用多种语言，对于使用您输入语言之外的其他语言的用户，将显示为“默认语言”选择的语言。
点击右侧的** + **按钮可以添加语言，如果没有所需的语言，可以通过联系[客户中心](https://toast.com/support/inquiry)将其添加。
选择“翻译”按键，可将默认语言翻译成设置的语言。

## AppGuard

> <font color="red">[重要]</font>
> 仅在使用TOAST AppGuard服务时可用。

![gamebase_ban_09_201812](https://static.toastoven.net/prod_gamebase/gamebase_ban_09_201812.png)

- **是否联动**: 当AppGuard需要自动将已检测或被制裁用户添加为Gamebase禁用用户时启用。
- **自动禁用** 是针对检测/制裁等种类，通过将自动禁用设置为**ON**，输入 '显示给用户消息'和 **禁用期限**，点击**保存** 按钮即可。
- 您可以通过选择**与Leader board联动 **项，选择是否删除检测到或受制裁用户的Leader board数据。
> [参考]
> 因用户触发APPGuard规则被自动禁用时，结果将会添加到注册系统的”APPGuard”列表。
