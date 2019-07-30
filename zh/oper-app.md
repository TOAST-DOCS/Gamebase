## Game > Gamebase > 控制台使用指南> APP

在TOAST Console依次点击**Game > Gamebase > App**来设定APP的基本信息。
* **APP**：APP信息管理
* **客户端**：管理状态信息及客户端版本
* **安装URL**：管理每个APP商店的安装URL


## App

如果启用Gamebaes服务，将自动创建应用程序，您只能修改该菜单中的注册信息。
由于每个TOAST项目可以管理一个Gamebase应用程序，因此您无法添加或删除其他应用程序。如果禁用Gamebase服务，将删除应用程序中登记的信息。
有关各个项目的详细说明，请参考以下 **属性** 项目。

### Properties

以下描述在Gamebase Console管理的应用信息。

![gamebase_app_01_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_01_201812.png)

#### (1) 安装URL
简捷URL可用于APP安装和宣传。
如果APP已发布到多个应用商店，可以使用一个简捷URL进行管理。
详细的操作及管理方法，请参考以下链接 [安装URL 管理]。(./oper-app/#installed-url)

> [参考] 
> 如果启动Gamebase，它将自动创建，无法更改。

####(2) 服务器地址
当游戏需要实时接收游戏服务器地址（IP，URL等）时使用。
如果设置了服务器地址，则可以在客户端初始化后的“启动信息”中确认信息。
仅在游戏需要时输入，否则请留空。

####(3) 客服中心信息
如果没有客服中心页面，请输入邮件、电话号码等信息。
如果有客服中心页面，请在下面的**应用程序内URL**的**客服中心**输入信息。

> [参考] <br/>
>输入的信息显示在Gamebase提供的维护页面上。
 
####(4) 认证信息
在登录APP时，可以注册，编辑和删除IdP的认证信息。

可以设置外部认证的客户端ID和密钥(secret key)，以及回调URL和备注信息。
可以通过单击认证信息旁边的** + **按钮来添加信息，也可单击** - **按钮删除信息。
关于Idp详细的设置方法请参考 [Authentication Information](#authentication-information
> [参考] 
> 什么是**令牌再验证**?
>在客户端调用最新登录API时，是否重新验证外部IdP的令牌。
> 如果选择**不验证**，则仅验证内部令牌，不重新验证外部IdP的令牌。
> 如果选择**始终验证**，将始终验证Gamebase发布的内部令牌以及外部IdP令牌。


####(5) 应用程序内URL
可以通过控制台实时修改应用程序内的最常用的URL，而不需要重新部发布客户端。

- 服务条款 
- 用户隐私协议
- 禁用条款
- 客服中心URL 

仅在游戏需要时输入，否则留空。
客户端初始化后的“启动信息”中确认信息。

### Test Device

![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_02_201812.png)
如果注册为测试设备，即使正在维护使用Gamebase的APP，您也可以正常登录该游戏。
要注册为测试设备，必须输入**Device Key**。直接输入或查询“游戏用户ID” 后注册。
可以删除不再使用的测试设备。

> [参考] 
> 最多可以注册100个测试设备。

#### (1) 查询

可以查看在APP中注册的所有的测试设备。在**Search**输入关键词，便可查看符合搜索条件的测试设备。

#### (2) 注册

![gamebase_app_03_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_03_201812.png)


在查询页面单击**注册**按钮，将出现注册测试设备的页面。直接输入**Device Key**或者搜索**游戏用户ID**后注册测试设备。
**(A) 通过游戏用户ID注册**
选择类型作为用户的ID，输入游戏用户ID，然后单击**搜索**按钮，用户的登录日志显示在页面底部。从显示的列表中选择要注册为测试设备的设备密钥，然后输入**备注信息**后单击**注册**按钮将相应的设备密钥注册为测试设备。


**(B) 通过设备密匙注册**
如果知道要注册的设备密钥，可以通过选择类型**设备密钥**直接注册测试设备。
输入注册设备和设备密钥的**备注信息**后单击注册按钮则注册为测试设备。

> [参考] 
> 请在备注信息中输入用户易于查看的自定义名称。 例) iPhone 6 测试, Toast的iPad

#### (3) 删除

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_App3_4.0.png)

在测试设备查询页面上确认要删除的测试设备后，单击左上角的删除按钮，则删除测试设备信息。信息删除后无法恢复，请删除前仔细确认。

### Authentication Information

#### 1. Facebook
在Gamebase Console输入，您在Facebook开发者网站上注册的{App ID}和{App Secret Code}
此时，还需要以JSON String的形式在备注信息栏中输入登录时所需的{Facebook Permission}。

**输入字段** 

- ClientID：{AppID} 
- Secret Key：{App Secret Code} 
- 备注信息：Facebook Permission (json format) 


![gamebase_app_04_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_04_201812.png)

**[例] facebook_permission format**
*在Facebook上，当您尝试使用OAuth进行身份验证时，您需要在Facebook上设置要求提供的信息类型。

```json
{ "facebook_permission"：[ "public_profile", "email"] }
```

![gamebase_app_05_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_05_201812.png)

**Reference URL**<br />

- [Facebook 开发者网站](https://developers.facebook.com/)
- [Facebook 权限](https://developers.facebook.com/docs/facebook-login/permissions/)

##### Android & iOS & Unity
除了TOAST Console中的设置以外，没有其他设置。


#### 2. Google

##### Google Cloud Console

![gamebase_app_06_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_06_201812.png)
1. 为了进行Google认证，必须从Google Cloud Console获取 **Web Application Client ID**，并将其输入到Gamebase Console。
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-001_1.11.0.png)
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-002_1.11.0.png)
2. 在“授权重定向URI”字段中，输入以下值。
	* https://alpha-id-gamebase.toast.com/oauth/callback
	* https://beta-id-gamebase.toast.com/oauth/callback
	* https://id-gamebase.toast.com/oauth/callback
	* ![google console](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-google-console-003_1.11.0.png)

##### iOS

> <font color="red">[注意]</font><br/>
>
> Gamebase iOS SDK 1.12.2版本中 URL Scheme的设置方法已变更，请确认相应SDK版本指南后，进行设置。
>

* 1.12.1或更低版本
	* 需要设置AdditionalInfo
		* **TOAST Console > Gamebase > App > 认证信息 > 备注信息 & Callback URL**的 **备注信息**中，您必须以JSON string的形式设置信息。
		*对于GOOGLE，您需要在iOS App中设定所需的**url_scheme_ios_only**信息。
		* **url_scheme_ios_only**的值必须与Xcode的URL Scheme中注册的值的其中一个匹配。

    *需要设置URL Scheme
        * **XCode > Target > Info > URL Types**

```json
{ "url_scheme_ios_only"："Your URL Schemes" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 3. Apple游戏中心
在Gamebase Console上，输入Apple 开发者网站上注册的BundleID。

**输入字段**<br />

- ClientID：{Bundle ID}<br />

![gamebase_app_08_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_08_201812.png)

**Reference URL**<br />

- [Apple开发者网站](https://developer.apple.com/)
- [Apple iTunes Connect](https://itunesconnect.apple.com/)


#### 4. PAYCO
在Gamebase Console上输入，申请PAYCO Client ID发放的 {client_id} 和 {client_secret}。

**输入字段**<br />

- ClientID：{Payco client_id}
- Secret Key：{Payco client_secret}
- 备注信息：Payco Service & Service Name (JSON format)

##### Android & Unity
- 需要设置AdditionalInfo
    * **TOAST Console > Gamebase > App > 认证信息 > 备注信息**中，您必须以JSON string的形式设置信息。
    *对于PAYCO，需要设置PaycoSDK所需的**service_code**和**service_name**。

* PAYCO添加认证信息输入示例

```json
{ "service_code"："HANGAME", "service_name"："Your Service Name" }
```

##### iOS

> <font color="red">[注意]</font><br/>
>
> 在Gamebase iOS SDK 1.12.2版本中 URL Scheme的设置方法已变更，请确认相应SDK版本指南后，进行设置。
>
* 1.12.1或更低版本
	* 需要设置AdditionalInfo.
		* **TOAST Console > Gamebase > App > 认证信息 >备注信息**中， 您必须以JSON string的形式设置信息。
		*对于PAYCO，您需要设置PaycoSDK所需的**service_code**和**service_name**

* 1.12.2或更高版本
	* 需要设定AdditionalInfo
		* **TOAST Console > Gamebase > App > 认证信息 >备注信息**中， 您必须以JSON string的形式设置信息。
		* 对于PAYCO，您需要设置PaycoSDK所需的**service_code**和**service_name**。
	* 需要设定URL Scheme
		***需要在XCode > Target > Info > URL Types**中添加 `tcgb.{Bundle ID}.payco`。

* PAYCO添加认证信息示例

```json
{ "service_code": "HANGAME", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

#### 5.NAVER
 在Gamebase Console上，输入在Naver开发者网站上申请发放的{client_id} 及 {client_secret}。
此时，您需要要置** service_name **，它会在登录窗口上显示。在iOS中，** url_scheme_ios_only **的值，应以JSON string的形式，添加到备注信息字段中。

**输入字段**

- Client ID：{NAVER client_id}
- Secret Key：{NAVER client_secret}
- 附加信息：NAVER Application Name & iOS url scheme (json format)

**Reference URL**<br />
- [NAVER开发者 –申请注册](https://developers.naver.com/apps/#/register)
- [NAVER开发者 - 检查客户端ID和客户端密钥](https://developers.naver.com/docs/common/openapiguide/#/appregister.md)

##### Android & Unity
* **TOAST Console > Gamebase > App > 认证信息 > 备注信息 & Callback URL**的 **备注信息**中，您必须以JSON string的形式设置信息。
	* 对于NAVER，您需要设置**service_name**，它会在登录窗口上显示。
```json
{"service_name"："Your Service Name" }
```

##### iOS

> <font color="red">[注意]</font><br/>
>
> 在Gamebase iOS SDK 1.12.版本中URL Scheme的设置方法已变更，请确认相应SDK版本指南后，进行设置。
>

* 1.12.1或更低版本
	* **TOAST Console > Gamebase > App > 认证信息 > 备注信息 & Callback URL**的 **备注信息**中，您必须以JSON string的形式设置信息。
		* 对于NAVER，您需要设置**service_name**，它会在登录窗口上显示。
		* 您需要在iOS App中设定所需的**url_scheme_ios_only**信息。

	*需要设置URL Schemes
		* **XCode > Target > Info > URL Types**

* NAVER添加认证信息例示

```json
{ "url_scheme_ios_only": "Your URL Schemes", "service_name": "Your Service Name" }
```

![gamebase_app_07_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_07_201812.png)

* 1.12.2 或更高版本
	* ** TOAST Console > Gamebase > App > 认证信息 > 备注信息 & Callback URL**的 **备注信息**中，您必须以JSON string的形式设置信息。

		*对于NAVER，您需要设置**service_name**，它会在登录窗口上显示。

	* 需要设置URL Scheme
		* **需要在XCode > Target > Info > URL Types**添加 `tcgb.{Bundle ID}.naver`


#### 6. Twitter
 在Gamebase Console上，输入在Twitter Application Management网站申请发放的 {Consumer Key}及 {Consumer Secret}。

**输入字段**

- Client ID：{Twitter Consumer Key}
- Secret Key：{Twitter Consumer Secret}

**Reference URL**
- [Twitter Application Management](https://apps.twitter.com/)

##### iOS

 > <font color="red">[주의]</font><br/>
 >
 > Gamebase iOS SDK 1.14.0 버전에서 URL Scheme의 설정 방법이 변경 되었습니다. 사용 SDK 버전에 맞는 가이드를 확인하여 설정하시기 바랍니다.
 >

 * 1.13.0 이하
	* 별도의 URL Scheme 설정이 필요하지 않습니다.

* 1.14.0 이상
	* URL Scheme를 설정해야 합니다.
		* **XCode > Target > Info > URL Types**에 **tcgb.{Bundle ID}.twitter**를 추가해야 합니다.

* Twitter 추가 인증 정보 입력 예제

![Twitter URL Types](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-auth-001_1.7.0.png)

#### 7. LINE

**输入字段**
- Client ID：{LINE Channel ID}
- Secret Key：{LINE Channel Secret}

**Reference URL**

- [LINE Developer Console](https://developers.line.me/console/)

##### iOS
要使用LINE登录功能，Xcode中需要其他设置。
- 需要设置URL Schemes.
	* **需要在XCode > Target > Info > URL Types**添加‘line3rdp.{App Bundle ID}’。

- 您需要设置Info.plist文件。
	* 设置LINE发放的ChannelID。
	```xml
	<key>LineSDKConfig</key>
	<dict>
    	<key>ChannelID</key>
    	<string>{Issued LINE ChannleID}</string>
	</dict>
	```
	* 为了设置ATS，登记scheme。
	```xml
	<key>LSApplicationQueriesSchemes</key>
	<array>
    	<string>lineauth</string>
    	<string>line3rdp.{App Bundle ID}</string>
	</array>
	```
- 有关项目设置，请参考以下链接以使用LINE登录。 （需要验证）
* [LINK \[LINE Developer Guide\]](https://developers.line.biz/en/docs/ios-sdk/objective-c/overview/)

## Client

客户端信息可以通过系统(iOS, Android, Unity WebGL, Unity Standalone)和版本进行管理。

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
您可以看到当前已注册的客户端列表。
按系统区分表示，图标中的数字表示注册客户端时输入的版本。
图标列表中仅显示服务状态为<font color="white" style="background-color:#F8BB28">测试</font>, <font color="white" style="background-color:#FB8F37">审核中</font>, <font color="white" style="background-color:#88C637">服务</font>, <font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>。单击各系统右下角的箭头，可以查看<font color="white" style="background-color:#A1A1A1">强制更新</font>, <font color="white" style="background-color:#CCCCCC">已下线</font> 状态的客户端列表。
图标颜色可以根据服务状态进行分类，因此可以一目了然地掌握服务状态。

### Properties

下面描述Gamebase控制台管理的客户端属性。
在**客户端**选项卡中，单击** AOS注册**，** iOS注册**按钮等，将弹出注册客户端页面。如果要修改或删除已注册客户端的信息，请单击图标列表中的图标，或从客户端的整个列表中选择所需的客户端即可。
![gamebase_app_13_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_201901.png)
#### (1) 商店
(<font color="red">必须</font>)选择要发布客户端的商店。每个系统都有不同的商店。
#### (2) 客户端版本
(<font color="red">必须</font>) 输入客户端版本。
可以根据游戏中设置的规则输入字符串。
#### (3) 服务状态
(<font color="red">必须</font>)选择客户端的服务状态。
状态是 <font color="white" style="background-color:#F8BB28">测试</font>, <font color="white" style="background-color:#FB8F37">审核中</font>，<font color="white" style="background-color:#88C637">服务中</font>，<font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>，<font color="white" style="background-color:#A1A1A1">强制更新</font>，<font color="white" style="background-color:#CCCCCC">已下线</font> 共六种。

- <font color="white" style="background-color:#F8BB28">测试</font>：内部测试。
- <font color="white" style="background-color:#FB8F37">审核中</font>：商店正在审核中时，可以设置其他稳定指标。
	![gamebase_app_14_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_14_201812.png)

> [参考]
>什么是“稳定指标”？<br/>
>在Gamebase内部调用Gamebase API时，产生的指标日志。
>如果服务状态为“审核中”时，启用稳定性指标，可以更容易地确认审核期间Gamebase内发生的问题。
>您可以在Gamebase控制台中设置是否实时使用稳定指标及日志保存级别。

![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

- <font color="white" style="background-color:#88C637">服务中</font>：正常服务。
- <font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>：正常服务。<br/>显示弹出窗口以建议您使用更新的版本。虽然建议下载新版本，但用户仍然可以使用当前版本。<br />下面是“推荐更新（服务中心）”状态时，Gamebase SDK中默认弹出的窗口。

- <font color="white" style="background-color:#A1A1A1">强制更新</font>：服务不可用。<br/>当前版本已经不能继续使用，提示必须安装最新版本。<br />下面是“强制更新”状态时，Gamebase SDK中默认弹出的窗口。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[注意] </font>
>  如果**同时设置强制更新和维护**，则服务状态变为“强制更新”。
>  如果不希望在维护过程中向用户弹出“强制更新”的窗口，则应在维护结束后将服务状态改为“强制更新”。

- <font color="white" style="background-color:#CCCCCC">已下线</font>：服务不可用。<br/>如果版本已下线不再对外服务时，请选择此项。<br />下面是“已下线”状态时，Gamebase SDK中默认弹出的窗口。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [参考]
>设置各服务状态显示的消息。
>您可以设置多语言消息，在状态为**推荐更新(服务中)**, **强制更新**, **已下线**时提示用户。
>选择服务状态时，每种状态的默认消息有五种语言（韩语，英语，日语，简体中文和繁体中文），您可以根据需要添加语言或更改默认消息的文本。
> ![gamebase_app_18_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_18_201812.png)

#### (4) 服务器地址
输入客户端要使用的服务器地址（IP，URL）。
在** App **选项卡中输入的服务器地址适用于所有客户端，因此仅当您要为每个客户端使用不同的服务器地址时才输入服务器地址。

#### (5) Debug log
Gamebae SDK의 Debug Log 출력 여부를 Console을 통하여 실시간으로 변경할 수 있습니다.
설정되어 있지 않으면 기본적으로 Gamebase SDK 내부에 설정된 값을 우선으로 동작하고 Gamebase Console에서 Debug Log 출력 여부를 설정하실 수 있습니다.
Gamebase SDK에 Debug Log가 'OFF"상태이더라도 Console에서 'ON'으로 설정하시면 단말기에 Gamebase Debug Log가 출력됩니다.

## Installed URL

![gamebase_app_19_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812.png)


게임을 설치하기 위한 스토어 URL 정보를 관리합니다.
클라이언트 상태 중 <font color="white" style="background-color:#2AB1A6">업데이트 권장(서비스 중)</font> 또는 <font color="white" style="background-color:#A1A1A1">업데이트 필수</font> 일 때 각각의 스토어 별로 제공할 주소들에 대한 값을 설정합니다.
사용자가 PC나 모바일에서 단축 URL을 클릭하면, 사용자 단말기 정보(디바이스, 운영체제, 스토어 등)를 이용하여 입력된 사이트로 리디렉션합니다.
스토어 정보가 없거나 스토어 이동에 실패하면 'COMMON'에 설정된 URL로 이동합니다.


_[示例1] 单击在Android手机上收到的安装URL
**(Device:mobile,OS:Android,Store:无)** 时，会转到Android上默认商店的移动网址。如果默认商店是“Google Play”，会转到“Google Play”移动设备中设置的网址。
_[例示2] 如果从“One Store”下载应用程序的用户，在进行游戏中弹出“强制更新”窗口，点击了“立即更新”按钮
**(Device:mobile,OS:Android,Store:One Store)** 会转到“One Store”移动设备设置的URL(One Store移动安装页面)<br/>
_[例示3] 如果在PC上输入安装URL
**(Device:PC,OS:Windows,Store:无)** 时，会转到COMMON PC上设置的URL

### Properties

要更改输入的安装URL信息，请单击**修改**按钮。

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_20_201812.png)

-  可以为PC和移动设备单独设置每个项。如果您不需要区分PC和移动设备，则可以输入相同的值。
- 如果在列表中没有所需的商店，可以通过联系[客服中心](https://toast.com/support/inquiry) 将其添加。

#### (1) Common
设置没有商店信息或商店重定向失败时要连接的地址。

#### (2) Android
设置Android用户在运行安装URL时要连接的地址。

#### (3) iOS
设置iOS用户在运行安装URL时要连接的地址。
#### (4) Standalone
在Standalone提供的应用中设置要连接的地址。 Standalone仅适用于PC，因此只需设置PC即可。

## Transfer account
Guest로 로그인한 유저가 다른 아이디 제공자를 연동하지 않고 다른 단말기에서 이어서 게임을 할 수 있는 기능을 제공합니다.
사용자는 현재 게임중인 단말에서 기기 이전을 위한 Key를 발급 받아 이전하려는 단말기에 Key를 입력하는 것만으로 쉽게 게임 기기를 변경할 수 있습니다.
기기이전 기능은 기본적으로 비활성화 되어 있으며 사용을 원하는 경우에는 기기이전 메뉴에서 **사용하기** 클릭하여 기능을 활성화 해야 합니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_1.0.png)
1) 기기 이전 기능을 사용하기 위해서는 **사용하기** 클릭하여 기능을 활성화 해야 합니다. 사용하기 버튼을 누르면 기기 이전 기능 사용을 위해 필요한 값을 설정하는 화면으로 전환됩니다.

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_2.0.png)
기기 이전 기능에 필요한 값들을 설정 할 수 있는 화면입니다.
각 항목에 대한 설명은 아래와 같습니다.

#### 발급
기기 이전 발급키의 형식을 설정합니다.
사용자의 기기 이전키는 ID만 사용하거나 ID, 비밀번호 두 개의 키를 이용할 수 있습니다. ID, 비밀번호의 형식은 게임에서 원하는 소문자, 대문자, 숫자 조합으로 구성할 수 있습니다.
1) **ID 자동 발급 형식** : 기기 이전 ID 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
  - **숫자(최소길이12)** : 숫자로만 이루어진 아이디를 발급합니다. 발급되는 ID의 최소길이는 12자입니다.
  - **숫자+소문자(최소길이10)** : 숫자와 소문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 10자입니다.
  - **숫자+대문자(최소길이10)** : 숫자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 10자입니다.
  - **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 9자입니다.
  - **소문자+대문자(최소길이9)** : 소문자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 ID의 최소길이는 9자입니다.

2) **비밀번호 자동 발급 형식** : 기기 이전 ID를 이용하여 로그인할 때 사용할 비밀번호 발급 형식을 설정합니다. 설정 항목은 아래와 같습니다.
  - **비밀번호 사용 안함** : 비밀번호를 사용하지 않을 때 선택합니다. 해당항목을 선택하면 아래 검증항목에서 아이디의 유효시간만 설정할 수 있습니다.
  - **숫자(최소길이12)** : 숫자로만 이루어진 비밀전호를 발급합니다. 발급되는 비밀번호의 최소길이는 12자입니다.
  - **숫자+소문자(최소길이10)** : 숫자와 소문자의 조합으로만 이루어진 비밀번호를 발급합니다. 발급되는 비밀번호의 최소길이는 10자입니다.
  - **숫자+대문자(최소길이10)** : 숫자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 10자입니다.
  - **숫자+소문자+대문자(최소길이:9)** : 숫자, 소문자, 대문자의 조합으로 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 9자입니다.
  - **소문자+대문자(최소길이9)** : 소문자와 대문자의 조합으로만 이루어진 ID를 발급합니다. 발급되는 비밀번호의 최소길이는 9자입니다.

#### 검증
발급된 기기이전키의 검증 조건을 설정합니다.
기기이전키의 검증시 이전횟수나 유효기간, 실패시 차단 등의 설정을 하실 수 있습니다.
3) **기기 이전 횟수** : 발급된 아이디의 기기 이전이 가능한 횟수를 설정합니다. 무제한/일회성 중 한가지를 선택해야 합니다.
4) **유효 기간** : 발급된 계정의 유효 시간을 설정합니다. 발급된 기기 이전 ID는 이 설정값의 영향을 받습니다. 무제한/기간 설정 중 한가지를 선택해야 합니다.
5) **실패 시 재검증 차단 여부** : 유저가 아이디를 이용하여 로그인을 시도했을 때 실패했을 경우 재시도에 대한 설정을 추가로 설정하실 수 있습니다. 해당 항목을 체크하면 차단 관련 설정을 추가로 진행해야 합니다.
6) **차단 기준 횟수** : 실패 시 재검증 차단 여부가 선택되었을 때 설정할 수 있는 항목입니다. 유저가 최대로 실패할 수 있는 횟수를 설정할 수 있습니다. 최소 1회 이상은 설정되어야 합니다.
7) **차단 기간** : 유저가 검증에 실패하여 계정이 차단되었을 경우 다시 재시도를 할 수 있는 시간에 대한 설정을 할 수 있습니다. 영구 차단/기간 지정 중 한가지를 선택해야 합니다.


#### 초기 설정 완료 이후
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
최초 설정이 완료되면 유저는 기기 이전 기능의 비활성화만 가능하며 설정 변경이 필요할 경우 고객센터에 문의하시기 바랍니다.
**사용 안함** 버튼을 클릭하여 기능을 비활성화 할 수 있고 기존에 발급된 기기이전키는 모두 삭제되기 때문에 활성화 이후에는 비활성화 여부를 신중하게 선택해야 합니다.
