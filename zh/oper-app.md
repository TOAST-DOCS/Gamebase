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
启用Gamebase服务时，将自动创建应用程序。在应用程序菜单中，您只能修改注册的信息。
由于每个TOAST项目仅能管理一个Gamebase应用程序，因此无法添加或删除其他应用程序。如果禁用Gamebase服务，将删除应用程序中登记的信息。  
关于各个项目的详细说明，请参考以下属性项目。

### 基本信息
![gamebase_app_01_202009](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_01_202009.png)

#### (1) 安装URL
简捷URL可用于APP安装和宣传。
如果APP已发布到多个应用商店，可以使用一个简捷URL进行管理。
详细的操作及管理方法，请参考以下链接 [安装URL 管理]。(./oper-app/#installed-url)

> [参考]
> 如果启动Gamebase，它将自动创建，无法更改。

#### (2) 是否包含测试付款                                 
查看App指标时，请选择是否包含测试付款。
默认选项已设为"包含测试付款"。如果选为"测试付款不包括"，当显示指标时，将从Analytics销售指标中筛选所有测试付款。
> [参考1]
> 无论指标的设置状态如何，因数据始终累积测试支付件和实际的支付件，即使更改测试付款的显示与否，也不影响到实际的数据搜集。

> [参考2]
> 测试数据只支持Google和AppStore。
> 各商店的测试指标的标准如下。
> * Google : 使用在Google控制台中注册的测试账户进行付款的历史记录
> * AppStore : 在Sandbox环境下进行测试付款的历史记录

#### (3) 预约退出时期
若欲使用App的”预约退出‘’功能，请设置预约退出的时间。
默认选项已设为”7天”，而可以选择1到30天之间的任何时间。
> [参考]
> 在预约退出时期也可使用服务。

### 服务器地址
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_02_202012.png)

- 当需要在游戏中不断接收游戏服务器地址（IP, URL等）时使用。
- 设置服务器地址，初始化客户端后，可在“推出信息”中确认输入的信息。
- 可以按客户端的各状态设置服务器地址，并在推出信息中查看服务器地址。
- 仅在游戏需要时输入，否则留空。

### 语言配置
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_03_202004.png)

- 可以在每个菜单的多国语言设置中提前指定要显示的默认语言。
- 显示多国语言时显示所选的语言，而也将所选的默认语言也显示为设置项目。
- 如果不需要使用，则留空。

### 认证信息
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_04_202004.png)

登录gamebase时，可以注册、编辑或删除IdP的认证信息。

可以设置外部认证的客户端ID、密钥（secret key）、回调URL及附加信息。
可以通过单击认证信息旁边的 + 按钮来添加信息，也可单击 - 按钮删除信息。
关于Idp详细的设置方法，请参考[Authentication Information](#authentication-information)。
> [参考]
> 什么是令牌再验证?
> 在客户端调用Latest Login API时，设置是否要重新验证外部IdP的令牌。
> 如果选择**不验证**，则仅验证内部令牌，不重新验证外部IdP的令牌。
> 如果选择**始终验证**，将始终验证Gamebase发布的内部令牌以及外部IdP令牌。

### In-App URL
![gamebase_app_01_202004](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_05_202009.png)
可以通过控制台实时修改应用程序内最常用的URL，而不需要重新发布客户端。

- 利用条款
- 同意隐私协议
- 禁用规定

仅在游戏需要时输入，否则留空。
初始化客户端后在“推出信息”中可以确认设置的信息。

### 客户服务
可以设置与客户服务有关的项目。
Gamebase提供3种客户服务类型，根据类型可设置不同的项目。
设置方法如下。

#### 1. 开发公司自建客户服务
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_19_202009.png)
若欲使用开发公司提供的自建客户服务时进行设置。
需要设置的项目如下。
* **客户服务URL** : 输入您使用的开发公司自建客户服务地址。
* **电话号码** : 输入客户服务的电话号码。可以通过Gamebase SDK接收值。

#### 2. Gamebase提供的客户服务
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_20_202009.png)
若欲使用Gamebase提供的客户服务时使用。
需要设置的项目如下。
* **客户服务URL** : 提供可接收来自用户的1:1咨询的页面信息。选择‘’Gamebase提供的客户服务‘’时将自动生成URL，并通过该URL在另一个网页上可接收用户的1:1咨询。
* **电话号码** : 输入客户服务的电话号码。可以通过Gamebase SDK接收值。
* **支持语言** : 选择要支持的语言（这与项目本身的语言设置不同）。支持语言为韩语、英语、日语、汉语。可以从设置中选择使用Gamebase客户服务时要使用的语言。
* **默认语言** : 在支持语言选项中选择客户服务内提供的默认语言。 

#### 3. TOAST组织服务(Online Contact)
![gamebase_app_19_202009.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_21_202009.png)
若欲使用TOAST组织提供的Online contact服务时进行设置。 
需要设置的项目如下。
* **客户服务URL** : 输入TOAST Online Contact提供的地址。此信息可通过TOAST Online Contact确认。
* **电话号码** : 输入客户服务电话号码。可以通过Gamebase SDK接收值。 
* **SSO登录API Key** : 输入需要确认TOAST Online Contact客户服务1:1咨询时使用的Key。可以通过Gamebase SDK接收值。若不输入此信息，则不能确认客户的咨询内容，因此必须要输入。关于具体的联动方法，请参考以下内容。
> [参考] TOAST Online Contact与Gamebase间的联动
> 如果要使Gamebase与TOAST Online Contact互相联动，则需按以下程序获取SSO登录API Key，并在Gamebase内进行设置才能使用服务。
> 为了提供更可靠的客户服务，请参考以下程序。
>
> 1) 在TOAST Online Contact中注册SSO
> 全局管理 -> SSO登录 -> 点击注册按钮
> ![gamebase_app_02_201812.png](https://   static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_22_202009.png)
>
> 2) 输入‘’SSO登录”的备注信息 
> SSO登录名 : Gamebase SSO认证(或输入相关标识符信息)
> 远程登录URL : https://gamebase-web.cloud.toast.com/tcgb-web/v1.0/apps/{appId}/online-contact/login
> 登录状态URL : https://gamebase-web.cloud.toast.com/tcgb-web/v1.0/apps/{appId}/online-contact/login
> 确认Gamebase项目ID后，在**{appId}**部分中输入。
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_23_202009.png)
>
> 3) 启用SSO登录认证并选择注册的SSO 
> 服务管理 -> 认证 -> 移动至SSO登录后，将”启用SSO登录"选为激活，并将”指定SSO登录”选为”Gamebase SSO认证”。
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_24_202009.png)
>
> 4) 获取SSO登录API Key之后，在SSO登录API Key输入框中输入。
> 全局管理 -> 移动至SSO登录页面，复制页面上的API Key，在Gamebase SSO登录API Key输入框中输入。
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_25_202009.png)
>
> 5) 获取TOAST Online contact客户服务页面地址后，在客户服务URL中输入该地址。
> Help Center -> 选择子菜单 -> 点击右上方的”返回Help Center‘’。 
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_26_202009.png)
> 在Gamebase客户服务URL输入框中输入在浏览器上端显示的地址。
> ![gamebase_app_02_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_27_202009.png)
>

### Test Device
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_02_201912.png)
一旦注册为测试终端，即使使用Gamebase的应用程序正在进行维护，也可进入游戏。
若要设置测试终端机，则应注册**Device Key**或**IP**信息。进行注册时，可以直接输入或查看**游戏用户ID**。
可以通过允许用户玩游戏或设置每个终端机的‘’Debug Log输出与否”来管理测试终端机。
可以删除不需要的测试终端。
若点击”连接记录”按钮，则可通过相关机器确认**进行维护时的连接时间和详细的连接日志**。
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_09_201912.png)

> [参考]
> 最多只能注册100个测试终端机。 

#### (1) 查询

可以查看在APP中注册的所有的测试设备。在**Search**输入关键词，便可查看符合搜索条件的测试设备。

#### (2) 注册

![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_10_201912.png)
![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_11_201912.png)

在查询页面单击**注册**按钮，将出现注册测试设备的页面。直接输入**Device Key**或者搜索**游戏用户ID**后注册测试设备。
**(A) 通过游戏用户ID注册**
选择类型作为用户的ID，输入游戏用户ID，然后单击**搜索**按钮，用户的登录日志显示在页面底部。从显示的列表中选择要注册为测试设备的设备密钥，然后输入**备注信息**后单击**注册**按钮将相应的设备密钥注册为测试设备。
**(B) 使用Device Key或IP进行注册**
如果已确认要注册的Device key或IP信息，通过在类型选项中选择您要使用的方式， 直接注册测试终端机。
输入您要注册的终端机的 **终端机名称**、“Debug Log及‘’维护与否”后点击注册按键，则可注册测试终端机。

> [参考]
> 请在备注信息中输入用户易于查看的自定义名称。 例) iPhone 6 测试, Toast的iPad

#### (3) 删除

![gamebase_app_02_201812.png](http://static.toastoven.net/prod_gamebase/gamebase_app_12_201912.png)

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
{ "service_code"："FRIENDS", "service_name"："Your Service Name" }
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

##### Android
> <font color="red">[注意]</font><br/>
>
> 2019年7月25日起，Twitter终止支持TLS 1.0、TLS 1.1，仅支持TLS1.2。
> 因此，Android 4.3 (Jellybean, API Level 18)以下的终端机无法通过Android WebView登录Twitter。
>
> 即，仅Android 4.4以上(KitKat, API Level 19)的设备可使用Twitter登录。

##### iOS

> <font color="red">[注意]</font><br/>
>
> Gamebase iOS SDK 1.14.0版本中URL方案的设置方法已更改。请参考符合所用SDK版本的指南。
>

* 1.13.0以下
	* 可以不另行设置URL方案。

* 1.14.0以上
	* 需要设置URL Schema。
		* 需要在**XCode > Target > Info > URL Types**中添加**tcgb.{Bundle ID}.twitter**。
    * 需要设置Twitter的Developer网页的Apps > 项目 > App Details > Callback URL。
      *  请添加**tcgb.{Bundle ID}.twitter://**。

* Twitter附加验证信息输入示例

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

#### 8. Sign In with Apple
为使用Sign In with Apple功能，需要对AppStore Connect、Gamebase Console及Xcode进行设置。

##### AppStore Connect Settings
* [Certificates, Identifiers & Profiles \> 跳转至Keys](https://developer.apple.com/account/resources/authkeys/list)

###### Certificates, Identifiers & Profiles > Keys > 添加(+)
1. 选择“Sign In with Apple”复选框并进行设置。
![Check SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid0_1.0.png)
2. 选择要使用“Sign in with Apple”的Bundle ID。
![ChooseAPrimaryAppID](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid1_1.0.png)
3. 下载<span style="color:#e11d21">Privatekey</span>后保管，确认生成的<span style="color:#e11d21">Key ID</span>。
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid2_1.0.png)
4. Certificates, Identifiers & Profiles > Identifiers > 选择对象应用程序 > 激活“Sign In with Apple”。
    * 设置为“Enable as a primary App ID”。
![DownloadPrivateKey](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid3_1.0.png)

##### Gamebase Console > App Settings
[跳转至TOAST Console](https://console.toast.com/)

* Gamebase
![SecretKey设置](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid4_1.0.png)

###### Client ID Settings
> 设置应用程序的Bundle ID。

###### Secret Key Settings
> 利用在Apple Developer Account设置中获得的值(**TeamID**, **KeyID**, **PrivateKey**)创建JSON字符串并进行设置。

* “teamId”：设置开发者账号右上方的值。
* “keyId”：Certificates, Identifiers & Profiles > Keys > 勾选Sign In with Apple，设置创建的值。
![SecretKey设置](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid5_1.0.png)
* “privateKey”：设置在上面的Keys中创建密钥同时创建的PrivateKey文件的内容。（打开下载的文件，如下方截屏所示，使用红色矩形部分的值。）
![SecretKey设置](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid7_1.0.png)

如下方示例所示，将上面的值创建为JSON，进行设置。

```json
{
    "teamId":"2UH5Cxxxx",
    "keyId":"3C3FXYxxxx",
    "privateKey":"MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBA.. 中间省略“
}
```

###### Additional Info Settings
[了解Sign In with Apple的AuthorizationScope](https://developer.apple.com/documentation/authenticationservices/asauthorizationscope?language=occ)

在Gamebase Console > App中添加Apple后，将下方的JSON值设置为默认值。
以当前(2019.11)为准，Scope的类型仅有“full_name”、“email”，在Gamebase中将这两种值设置为默认值。

```json
{ "authorization_scope":["full_name", "email"] }
```

##### Xcode Project Settings
> <font color="red">[注意]</font><br/>
> 只能在Xcode 11以上创建使用**Sign In with Apple**功能的项目。

1. 选择Target > Signing & Capabilities > 添加Sign In with Apple项目。
![Capability_SignInWithApple](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid8_1.0.png)
2. 选择Target > Build Phases > Link Binary With Libraries > 将Authentication.framework添加为**Optional**。
![AuthenticationServices.framework](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Auth_appleid9_1.0.png)
> <font color="red">[注意]</font><br/>
> 如果设置为Required，而不是Optional，在iOS 12以下终端机上启动App时将出现运行时冲击。


##### 支持iOS 12以下版本的设置(Sign In with Apple JS)

> <font color="red">[注意]</font><br/>
> 
在Gamebase iOS SDK 2.13.0以上版本上，可通过iOS 12以下的WebView使用Sign In with Apple功能。 
> 
> 即使是使用2.13.0以前版本的游戏，也要在设置以前项目时参考以下**支持iOS 12以下版本的设置**。
>
> 若适用Gambase SDK iOS 2.13.0以上版本, 则可以在iOS 12以下版本上使用Sign In with Apple功能。


* 为了在iOS 12以下版本使用Sign In with Apple功能，需要利用Sign In with Apple JS功能进行登录。
* 登录时应在Apple ID登录网页上输入Apple帐户和密码。

**按照以下程序，可以在Apple开发人员网页上注册新的Service ID。**

1. 添加Service ID。 <br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_01.png)
2. 设置用作Service ID的标识符(一般为bundle ID + **.要区分的字符串**)。<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_02.png)
3. 确认注册的Service ID后进行修改。<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_03.png)
4. 点击下端Sign In with Apple项目中的Configure。<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_04.png)
5. 设置Primary App ID。(若正在使用Sign In with Apple，设置相关应用程序的Bundle ID。)<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_05.png)
6. 使用Apple ID进行认证后， 请设置要接收认证信息的Callback URL。<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_06.png)
7. 设置后保存。<br/>
![Create new Service ID](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_AppStore_07.png)

**在TOAST Gamebase Console > Gamebase > APP > 认证信息 > Apple > Service ID中输入在述程序中使用的Service ID。***

> <font color="red">[注意]</font><br/>
>
> 如果还尚未设置Sign In with Apple，则还要设置其他的项目。

1. 按以下方式，将在Apple开发人员网页上设置的Service ID添加在Service ID项目中。(若已有Sign In with Apple设定值，则不需要更改其他值。)
![Set Service ID for Sign In with Apple JS](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_SignInWithAppleJS_TOAST_01.png)

#### 9. WEIBO

##### Weibo Console

1. 在Weibo Developers网页上申请并获取{client_id}、{client_secret}后，将获取的信息在Gamebase控制台中输入。 
必须要以进行登录时所需的{scope}或JSON String格式输入。               


![gamebase_app_29_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_29_202012.png)

2. 请在callback URL栏里输入以下值。 
	* Authorization callback page : https://api.weibo.com/oauth2/default.html
	* Cancel authorization callback page : https://api.weibo.com/oauth2/default.html

**输入字段**

- ClientID : {App Key}
- Secret Key : {App Secret}
- 备注信息 : scope (json format)

**Additional Info Settings**

* Scope

显示Application所需的权限。 
根据Weibo指南所有权限已声明为默认值。
如果需要时可以添加、删除或修改。

* oauthApiUrl

是通过Gambase SDK调用Weibo Open API时使用的域名。
因此不应更改。 

![gamebase_app_28_202012.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_28_202012.png)


**Reference URL**
- [Weibo Developer](https://open.weibo.com/)


## Client

客户端信息可以通过系统(iOS, Android, Unity WebGL, Unity Standalone)和版本进行管理。

### Client List
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client1_1.2.png)
您可以看到当前注册的客户端列表。
按系统区分表示，而图标中的数字表示注册客户端时输入的版本。 
图标列表中仅显示服务状态为<font  color="white" style="background-color:#eed14c">测试</font>、<font color="white" style="background-color:#eba34b">Beta Service</font>、<font color="white" style="background-color:#eb7e4b">审核中</font>、<font color="white" style="background-color:#88C637">服务</font>、<font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>。通过点击每个运营体系右下端的箭头符号，可确认<font color="white" style="background-color:#A1A1A1">必须更新</font>、<font color="white" style="background-color:#CCCCCC">终止</font>的客户端列表。
图标颜色可以根据服务状态进行分类，因此可以一目了然地掌握服务状态。

### Properties

以下描述在Gamebase Console管理的客户端注册信息。 
在客户端选项卡中，单击 AOS注册、iOS注册按钮时，将显示注册客户端页面。如果要修改或删除已注册客户端的输入值，请单击图标列表中的图标，或从客户端的整个列表中选择所需的客户端即可。
![gamebase_app_13_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/gamebase_app_13_202012.png)
#### (1) 商店
(<font color="red">必须</font>) 选择要发布客户端的商店。
每个系统都有不同的商店。
#### (2) 游戏版本
(<font color="red">必须</font>) 输入客户端版本。
可以根据游戏上设置的规则输入字符串。
#### (3) 服务状态
(<font color="red">必须</font>) 选择客户端的服务状态。
各状态分别为<font color="white" style="background-color:#eed14c">测试</font>、<font color="white" style="background-color:#eba34b">Beta Service</font>、<font color="white" style="background-color:#eb7e4b">审核中</font>、<font color="white" style="background-color:#88C637">服务</font>、<font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>、<font color="white" style="background-color:#A1A1A1">必须更新</font>、<font color="white" style="background-color:#CCCCCC">终止</font>等共6种。

- <font color="white" style="background-color:#F8BB28">测试</font> : 内部测试
- <font color="white" style="background-color:#eba34b">Beta Service</font> : 若需要连接到服务服务器以外的其他beta服务器时选择。
- <font color="white" style="background-color:#FB8F37">正在审核</font> : 正在进行商店审核
![gamebase_app_15_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_app_15_201812.png)

- <font color="white" style="background-color:#88C637">服务中</font>：正常服务。
- <font color="white" style="background-color:#2AB1A6">推荐更新(服务中)</font>：正常服务。<br/>显示弹出窗口以建议您使用更新的版本。虽然建议下载新版本，但用户仍然可以使用当前版本。<br />下面是“推荐更新（服务中心）”状态时，Gamebase SDK中默认弹出的窗口。

- <font color="white" style="background-color:#A1A1A1">强制更新</font>：服务不可用。<br/>当前版本已经不能继续使用，提示必须安装最新版本。<br />下面是“强制更新”状态时，Gamebase SDK中默认弹出的窗口。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_updateRequired_1.1.png)
>  <font color="red">[注意] </font>
>  如果**同时设置强制更新和维护**，则服务状态变为“强制更新”。
>  如果不希望在维护过程中向用户弹出“强制更新”的窗口，则应在维护结束后将服务状态改为“强制更新”。

>  <font color="orange">[参考] </font>
>  点击更新按键时，将移动到”设置URL菜单”上设置的各商店地址。
>  例如，如果客户端已被设置为App store，而在‘’设置URL菜单”中存在与App store相关的设置，将会移动到注册的地址。若在”设置URL菜单”中不存在该设置时，将移动到共同(Common) URL。

- <font color="white" style="background-color:#CCCCCC">已下线</font>：服务不可用。<br/>如果版本已下线不再对外服务时，请选择此项。<br />下面是“已下线”状态时，Gamebase SDK中默认弹出的窗口。
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_Client_ended_1.0.png)

> [参考]
> 按照服务状态设置要显示的消息。
> 如果各状态为**推荐更新(服务中)**、**必须更新**、**终止**，则可通过多国语言设置注册消息。
> 选择服务状态时，按照在App中设置的语言信息提供符合各状态的默认消息。如果需要时，可以添加语言或修改默认消息的内容。
> 如果以前已按照各状态注册‘’语言类别设置"时，无论应用程序的语言设置信息状态如何，将显示以前注册的内容。   
> 在应用程序的语言设置中不存在设置信息时，可以以5种(韩语、英语、日语、汉语简体及繁体)语言提供默认消息。如果需要时，可以添加语言或修改默认消息的内容。
> ![gamebase_app_18_202004](https://static.toastoven.net/prod_gamebase/gamebase_app_18_202004.png)

#### (4) 服务器地址
输入客户端要使用的服务器地址（IP，URL）。
在** App **选项卡中输入的服务器地址适用于所有客户端，因此仅当您要为每个客户端使用不同的服务器地址时才输入服务器地址。

#### (5) Debug log
可在控制台中实时更改是否输出Gamebae SDK的Debug Log。
若未设置，默认优先运行Gamebase SDK内部设置的值，可在Gamebase控制台设置是否输出Debug Log。
即使Gamebase SDK中Debug Log为’OFF’状态，若在控制台中设置为’ON’，也将向终端机输出Gamebase Debug Log。

#### (6) Memo
可以在30个字符内输入有关客户端的简短的Memo。

## Installed URL

管理用于安装游戏的商店URL信息。

![gamebase_app_19_201812_en](https://static.toastoven.net/prod_gamebase/gamebase_app_19_201812_en.png)

客户状态为<font color="white" style="background-color:#2AB1A6">建议升级（正在服务）</font>或<font color="white" style="background-color:#A1A1A1">必须升级</font>时，设置各商店提供的地址的值。
用户在PC或移动设备中单击快捷URL，利用用户终端机信息（设备、操作系统、商店等）重定向至输入的网站。
若无商店信息或移动至商店失败，移动至‘COMMON’中设置的URL。

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
作为访客登录的游戏用户不与其他ID提供者关联，提供可在其他终端机上继续玩游戏的功能。
用户收到可从当前游戏中的终端机转移的密钥，在欲转移至的终端机输入密钥，即可轻松更改游戏终端机。
**转移终端机**功能默认为禁用。若欲使用，在**转移终端机**中单击**使用**。

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_1.0.png)
单击**使用**按钮后，输入转移终端机所需的信息。

![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_2.0.png)

各项目相关说明如下。

#### 发放
设置终端机转移发放密钥的形式。
终端机转移密钥可仅使用ID或使用ID、密码两个密钥。ID、密码的形式由游戏中需要的小写字母、大写字母、数字组合构成。

1.**ID自动发放形式**：设置终端机转移ID发放形式。设置项目如下。

- **数字（最小长度：12）**：发放仅由数字构成的ID。发放的ID的最小长度为12个字符。
- **数字+小写字母（最小长度：10）**：发放仅由数字和小写字母的组合构成的ID。发放的ID的最小长度为10个字符。
- **数字+大写字母（最小长度：10）**：发放仅由数字和大写字母的组合构成的ID。发放的ID的最小长度为10个字符。
- **数字+小写字母+大写字母（最小长度：9）**：发放仅由数字、小写字母、大写字母的组合构成的ID。发放的ID的最小长度为9个字符。
- **小写字母+大写字母（最小长度：9）**：发放仅由小写字母和大写字母的组合构成的ID。发放的ID的最小长度为9个字符。
2.**密码自动发放形式**：设置用终端机转移ID登录时使用的密码发放形式。设置项目如下。
- **不使用密码**：不使用密码时选择。若选择该项目，在如下验证项目中仅可设置ID的有效时间。
- **数字（最小长度：12）**：发放仅由数字构成的密码。发放的密码的最小长度为12个字符。
- **数字+小写字母（最小长度：10）**：发放仅由数字和小写字母的组合构成的密码。发放的密码的最小长度为10个字符。
- **数字+大写字母（最小长度：10）**：发放仅由数字和大写字母的组合构成的ID。发放的密码的最小长度为10个字符。
- **数字+小写字母+大写字母（最小长度：9）**：发放仅由数字、小写字母、大写字母的组合构成的ID。发放的密码的最小长度为9个字符。
- **小写字母+大写字母（最小长度：9）**：发放仅由小写字母和大写字母的组合构成的ID。发放的密码的最小长度为9个字符。

#### 验证
设置发放的终端机转移密钥的验证条件。
可设置验证终端机转移密钥时转移次数或有效期、失败时阻止等。
3.**终端机转移次数**：设置发放的ID的终端机可转移次数。应从无限制、一次性中选择一项。
4.**有效期**：设置发放的账户的有效时间。发放的终端机转移ID受此设定值影响。应从无限制、设置时间中选择一项。
5.**失败时是否阻止再次验证**：若尝试登录时失败，在特定时间内阻止账户。若选择，显示附加设置项目。
6.**阻止基准次数**：选择**失败时是否阻止再次验证**时显示。验证失败达到输入的次数时阻止账户。应设置为1次以上。
7.**阻止时间**：设置账户阻止多久后可重新尝试验证。从**永远阻止**、**指定时间**中选择一项。若选择**指定时间**，可指定希望阻止的小时和分钟。


#### 初始设置完成后
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/Operators_Guide/Console_App_TransferAccount1_3.0.png)
初始设置完成后，游戏用户仅可进行终端机转移功能的禁用，若需要更改设置，请咨询客服中心。
单击**不使用**按钮可禁用功能，由于原本发放的终端机转移密钥会全部删除，因此启用后应慎重选择是否禁用。

## Analytics indicator
可以确认或设置用于在Analytics中累积指标的”传送指标"。
按照用户级别(INT)、世界/服务器/渠道、类/职业进行分类。当显示用户级别时，只显示传送到实际Analytics的级别项目。在世界/服务器/渠道、类/职业类别项目中，只将在该菜单中注册的项目累积成Analytics的指标。
### 用户级别(INT)类别
可以确认传送到Analytics系统的级别指标项目。
只能对相关项目进行查询，而无法修改。
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_01_202003.png)

### 查询世界/服务器/渠道、类/职业类别 
可以确认按项目类别设置的传送指标项目。
可通过点击查询页面上的删除按钮，按项目类别进行删除。
一旦删除，**Analytics菜单不显示此项目**，并不累积被删除的项目，因此删除时要注意。
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_02_202003.png)

### 注册世界/服务器/渠道、类/职业类别  
可以注册想要累积成Analytics指标的信息。
可通过点击下端的添加按钮进行注册。可以注册**全项目最多100个**新的项目。
在注册页面上，对以前注册的数据**只提供修改功能**。如果想要删除，需要返回查询页面进行删除。 
![gamebase_app_20_201812.png](https://static.toastoven.net/prod_gamebase/gamebase_analytics_indicator_03_202003.png)

#### (1) ChannelId / ClassId : 输入要在Analytics中累积的标识符信息。请输入累积指标时要设置的ID信息。
#### (2) 显示指标页面 : 将要显示由输入为项目1的ID传送的指标时，输入要显示的内容。通过此信息可以修改以前的指标项目。
#### (3) 删除 : 在注册页面上只能删除添加的新项目。