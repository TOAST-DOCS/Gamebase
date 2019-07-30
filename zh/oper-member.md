## Game > Gamebase > 控制台使用指南 > 会员

查询登录游戏的会员信息。


## Search Member

您可以通过输入用户ID / IdP ID来搜索会员信息。
用户ID(User ID)是Gamebase首次登录时自动发放的用户标识。 为了减少同音字符造成的混淆，仅使用“ABCDFGHJKLMNPQRSTWXYZ1346789”字符。
IdP ID是Id Provider提供的ID信息，这意味着Id Provider中的唯一标识符，而不是登录时输入的信息。 因此，如果要按IDP ID项搜索，请在输入搜索信息时注意。

搜索到的用户的详细信息显示在顶部，登录，映射，付款，禁用和游戏时间等历史记录显示在底部的选项卡中。


### Detail Information
![gamebase_member_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_01_201812.png)

**User **

- **用户ID**: Gamebase用户ID
- **国家代码(USIM)**: 如果用户设备的USIM国家代码采集失败，则表示为“ZZ”。 如果您想查看设备上设置的国家代码，请在**登录记录**确认。
- **上次登录时间**: 用户上次登录的时间
- **注册日期**: 用户首次登录的时间
- **帐户状态**
  - **普通**: 普通用户。
  - **禁用**：因使用外挂而被禁用（ban）的用户。 您可以通过右上角的更改帐户状态菜单解除禁用。
  - **退出**: 已退出的用户。
- **推送令牌**: 查看用户的推送令牌信息。

####更改账户状态
![gamebase_member_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_02_201812.png)
这是一个可以更改被查询用户的帐户状态的功能。
以下情况可以按状态更改。
- **正常**: 您可以将用户状态更改为禁用或退出。 退出后无法恢复数据，因此请在处理前注意确认。
- **禁用**: 您可以解除禁用账户。
- **退出**: 未显示相关按钮。

**Identity Provider **

Gamebase允许多个外部IdP协同工作。 换句话说，用户可以通过使用一个用户ID注册Facebook和Google 两个IDP来登录。 SDK在调用 **Login using a specific IdP**或'**Add Mapping** API时注册IdP。

- **IdP**: 外部 IdP(guest, Facebook, PAYCO, Google 等)
- **Idp ID**: 外部IdP提供的ID(Facebook no, PAYCO ID等)
- **注册日期**: 用户首次注册IdP的时间

#### 매핑 추가

조회한 유저의 Id Provider 정보를 추가할 수 있는 기능입니다.
연결하고자하는 IdP를 가지고 있는 유저의 정보가 **정상**일 경우에만 매핑 추가 작업이 가능합니다.
* 제공 유저의 IdP 설정란에서 연결하고자 하는 Id Provider 정보를 1번 버튼을 통해 오른쪽화면에 추가한 후, 아래 매핑 추가 버튼을 통해 매핑 추가 작업을 진행합니다.
* 잘못 추가했다면 매핑 추가 버튼을 누르기 전에는 2번 버튼을 통해 언제든지 다른 Id provider로 교체 가능합니다.
* 제공 받는 유저가 GUEST정보만 가지고 있을 경우에는 새로운 Id Provider정보가 추가되면서 기존의 Guest정보는 유실되므로 작업 진행 시 주의가 필요합니다.
* 제공 유저의 Id Provider 정보가 한개일 경우 해당작업을 진행하면 제공 유저 정보는 **유실**상태로 변경되어 더이상 사용할 수 없으므로 작업 진행 전 확인이 필요합니다.
##### 제공 예시
![gamebase_member_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_03_201812.png)

#### 매핑 해제
다중 매핑이 이루어진 계정의 경우 요청에 따라 Id Provider 정보 연동을 해제할 수 있습니다.
각각의 계정은 최소 1개의 연결정보가 존재해야 하므로 2개 이상의 연결정보가 존재할 때만 버튼이 활성화 됩니다.
* 버튼을 누르면 아래와 같이 연결된 Id Provider 정보와 함께 매핑 해제 버튼이 노출됩니다.

![gamebase_member_04_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_04_201812.png)

* * 해제 버튼을 누를 경우 아래와 같이 최종 확인창과 함께 Id Provider 정보를 확인합니다. 확인버튼을 누르시면 매핑 해제가 진행됩니다.
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_RemoveMapping_2.0.png)

### Login History
![gamebase_member_05_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_05_201812.png)

查询被查询用户的登录历史记录。
查询时最初可以查询最近1天，您还可以输入想查询的日期来进行查询。但，仅提供过去3个月（90天）的历史记录。
SDK调用与登录相关的API时会添加历史记录。

- **Login Date**: 用户登录应用程序的时间
- **Login Type**:用户登录的身份验证类型（IdP Login/Guest等）。 括号内的信息是实际使用的IdProvider信息。
- **OS / Ver**:用户登录的OS（IOS / Android / WebGL等）和OS的版本信息
- **Device model**:登录应用程序的设备型号名称
- **Device Key**:登录用户的设备的唯一标识值（Android：Android ID，iOS：IDFV）。
- **Device Country Code**: 用户登录时在设备上设置的国家代码
- **USIM Country Code**: 用户登录时在USIM卡上设置的国家代码
- **Telecom**: 用户登录时使用的运营商信息
- **Network**:用户登录的网络类型(Wi-Fi/3G/LTE 等)
- **Language Code**:用户登录时在终端中设置的语言代码信息
- **Store Code**: 用户下载应用的商店信息
- **Client Version**: 应用下载时的客户端版本信息
- **Gamebase SDK Version**: APP使用的Gamebase SDK版本信息
- **etc**: 登录时使用的除上述项之外的其他信息

### Mapping History
![gamebase_member_06_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_06_201812.png)

查询被查询用户的Mapping，解除Mapping的历史记录。 最长可查看日期为三个月（90天）。

* **IdP ID**: 登录IdP时使用的ID信息
* **IdP**: Mapping IdP信息
* **日期**: IdP ID和 Gamebase ID Mapping 映射操作的时间
* **Type**: Mapping 操作的详细信息
  - AAM: 添加 Mapping 
  - ARM: 删除 Mapping 
  - AFR: 强制删除 Mapping 
  - GMG: 创建guest帐户
  - OMG: 创建IdP 账户

点击Mapping 的IDP历史记录可显示基于IdP 映射到Gamebase ID的历史记录。
![gamebase_member_07_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_07_201812.png)

### Purchase History
![gamebase_member_08_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_08_201812.png)
查询被查询用户的购买记录。
您可以输入要查看的日期，可以查看的最长日期为一个月（30天）。

- **Transaction ID**: 用于区分Gamebase内支付的唯一编号
- **商店**: 已付款的商店信息
- **道具名称**: 用户在APP内购买的实际item名称
- **价格**: 用户购买的item价格
- **货币**: 用户购买时使用的货币类型
- **消费状态**: 付款项目是否已付款
- **付款状态**: 目前的付款进度
- **Store Reference Key**: 商店发行的付款唯一编号
- **付款日期**: 用户尝试购买或完成购买的时间
- **退还日期**: 用户退还item的时间

### Ban History
![gamebase_member_09_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_09_201812.png)

查询被查询用户的禁用记录。

您可以输入要查看的日期，可以查看的最长日期为一个月（30天）。

- **开始日期**: 用户禁用应用程序开始时间
- **结束日期**:解除禁用的时间
- **模板**: 添加用户禁用时使用的模板名称
- **原因**: 管理员禁用用户的实际原因信息
- **添加人/添加日期**: 管理员/系统信息以及日期和时间
- **解除原因**: 管理员解除禁用时输入的解除原因
- **解除禁用人/解除日期**: 管理员/系统信息以及日期和时间

### Playtime

![gamebase_member_10_201812](https://static.toastoven.net/prod_gamebase/gamebase_member_10_201812.png)
按日期查询被查询用户的游戏时间。
您可以输入要查看的日期，可以查看的最长日期为一个月（30天）。

### Withdraw History

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_WithdrawHistory1_1.1.png)
如果查询的用户是退出的用户，则会显示退出记录。
只有在您查询退出用户时才会显示此菜单，可以查询用户退出的方式。


## Transfer account

기기 이전 기능을 사용할 경우에만 사용하실 수 있습니다. [기기이전 기능 활성화하기](./oper-app/#transfer-account)
유저의 기기 이전키의 발급 및 검증 이력을 확인할 수 있습니다. 차단된 키의 차단해제나 만료된 키의 재발급이 가능합니다.

![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount1_1.0.png)
**기기 이전 발급 Key**
- **ID** : 유저에게 발급된 기기 이전 ID
- **발급 일자** : 기기 이전 ID가 발급된 일자
- **만료 일자** : 발급된 기기 이전 ID의 만료 일자
- **상태** : 발급된 기기 이전 ID의 현재 상태
  - <font color="white" style="background-color:#88C637">정상</font>: 발급된 키가 정상 상태입니다. 해당 키를 이용하여 기기 이전이 가능합니다.
  - <font color="white" style="background-color:#FB8F37">차단</font>: 발급된 키가 차단된 상태입니다. 발급된 키를 이용하여 기기 이전이 불가능합니다.
  - <font color="white" style="background-color:#A1A1A1">만료</font>: 발급된 키의 사용 기간이 만료된 상태입니다. 발급된 키를 이용하여 기기 이전이 불가능합니다.

**기기 이전 이력**
해당 유저에게 발급된 키의 이력을 조회할 수 있습니다.
기본적으로 가장 최근에 발급된 키가 선택되어 있으며 다른 키를 선택할 경우 선택한 키의 이력을 조회할 수 있습니다.

### 기기 이전 재발급
![image alt](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_TransferAccount_Renewal1_1.0.png)
**재발급** 버튼을 클릭하여 새로운 기기 이전 키를 다시 발급 할 수 있습니다. 재발급하면 이전에 발급된 키는 더이상 사용할 수 없습니다.
1) **ID,비밀번호 재발급** : ID, 비밀번호를 모두 새롭게 발급합니다. 
2) **비밀번호 재발급** : ID는 이전에 발급된 ID를 그대로 사용하고 비밀번호만을 재발급합니다.

#### 재발급시 주의사항
- 비밀번호는 재발급 시 한번만 노출되므로 재발급 진행 이후 꼭 해당 정보를 별도로 보관하셔야 합니다.
- 보관하지 못하셨을 경우 따로 비밀번호를 찾을 수 있는 방법이 없으므로 재발급을 다시 진행해 주셔야 합니다.
- 만료된 키는 만료 일자가 갱신되나 계정 상태가 정상/차단일 경우에는 만료 일자가 갱신되지 않습니다.
