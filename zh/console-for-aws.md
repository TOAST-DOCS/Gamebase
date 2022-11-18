## Game > Gamebase > Console for AWS

下面描述了通过AWS marketplace注册会员的用户登录的控制台的基本设置和使用方法。

Gamebase for AWS控制台提供以下功能。
* 管理项目和服务。
* 管理使用服务的会员。

## Quick-Guide

以下是关于在控制台中提供的基本功能的Quick-Guide。

![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-01-202108.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-02-202108.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-03-202108.png)
![Gamebase-for-aws_quick-guide](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-04-202108.png)

## 管理项目

Gamebase以项目单位使用，因此按单位收费。

### 创建项目

* 只有通过aws订阅页面注册的管理者会员才能创建、修改或删除项目。
* 输入项目名称和说明后，系统将自动创建项目的ID。
* 创建项目时，Gamebase服务将自动激活。
* 创建项目后，如需合作，则可以将常规成员添加为项目会员。

![Gamebase-for-aws_project-create](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-01-202108.png)
![Gamebase-for-aws_project-create](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-02-202108.png)

1. 点机**+新项目**按钮创建项目。
2. 输入**项目名称**和**项目说明**。
3. 点击**确认**按钮创建项目。
4. 当项目被创建时，菜单中将显示项目名称。
5. 在**仪表盘**页面中查看项目信息。


### 编辑项目

* 点击项目详细页面右上端的**编辑**按钮时，将显示项目编辑页面。
* 在编辑页面上进行项目信息的修改和删除，并可停止服务的启用。
* 可以修改项目名称和说明，但无法修改项目ID。
* 如果在项目中没有正在使用的服务，则可以删除项目。 
* 删除项目时项目中的所有资源都将被删除，而且无法恢复。

### 修改项目

* 可以通过单击顶部的项目名称来查看已注册的项目列表。


## 会员

| 区分     | 管理员 | 一般 | 
| ------ | ------------ | ------------ | 
| 定义     | 项目管理 | 只能使用服务。 | 
| 注册方法 | 通过aws marketplace订阅页面注册。  | 管理员在控制台中创建的会员 | 
| 权限 | - 项目管理 : 创建/修改/删除<br>- 一般会员管理 | 只访问已有权限的服务控制台。 | 

### 在项目中添加一般会员

可以按以下顺序添加允许访问控制台的一般会员。
#### 1. 添加一般会员

![Gamebase-for-aws_create-ID](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-01-202108.png)

1. 点击 **姓名 > 一般/小组管理**后，将显示一般会员管理页面。 
2. 点击**+添加会员**按钮。
3. 当输入ID、姓名及电子邮件时，将通过电子邮件发送**确认密码设置的邮件**。
4. 如果通过收到的电子邮件更改密码，则可以使用添加的ID登录。

#### 2. 添加为项目成员

![Gamebase-for-aws_project-member](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-02-202108.png)
1. 在项目仪表盘页面上点击**+添加项目成员**按钮。
2. 输入在1中添加的一般会员ID，并赋予小组权限，访问当前项目。

### 管理小组权限

管理一般会员的服务控制台访问权限。 
通过点击**姓名 > 一般/小组管理**，您可在权限小组管理页面上创建、修改或删除权限小组。 

![Gamebase-for-aws_project-member](https://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-03-202108.png)
1. 输入您要添加的小组名称后点击**添加**按钮。 
2. 当您选择已被添加的小组名称，在页面右方将显示可修改小组权限的页面。
3. 在小组修改页面上可按各服务菜单赋予Read、ALL权限。选中要授予的权限后，单击“保存”按钮。
