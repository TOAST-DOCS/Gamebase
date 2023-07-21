## Game > Gamebase > Release Notes > Console

### 2023. 07. 25.

#### 改善/修改功能
* 应用程序 > 应用程序  
    * 改善后，操作员可以直接为NHN Cloud Online Contact客户服务设置附加参数。
* 客户服务 > 查询
    * 修改为从查询页面列表中进入查询的详细信息页面后，按下取消/保存按钮时移动到上一页列表。
* 客户服务 > 查询    
    * 将语言项目添加到客户查询搜索项目中。(但所有先前创建的查询都设置为默认语言，并且从新创建的查询正确设置了语言，因此从新创建的查询可进行搜索。)

#### 修改程序错误
* 推送 > 推送
    * 更改了在UTC+9以外的时区设置“重复出现”时无法正确设置发送时间的错误。

### 2023. 07. 11.

#### 改善/修改功能
* 运营 > KickOut
    * 改善后，当处理目标是一部分客户端时，甚至可以选择处于“必须更新”状态的客户端。

#### 修改程序错误
* 应用程序 > 分发条款
    * 修改了能够分发与现有分发国家相同的分发国家的错误。

### 2023. 06. 27.

#### 添加功能
* 购买(IAP) > 商店
	* 添加了One Store API v7(SDK v21)。
* 购买(IAP) > 付款信息
	* 添加了查看付款详细信息的功能。

#### 改善/修改功能
* 应用程序 > 应用程序
    * 在NHN Cloud Online Contact页面中输入的Token验证URL域名从`https://gamebase-web.cloud.toast.com` 更改为`https://web-gamebase.nhncloud.com` 。

### 2023. 06. 13.

#### 改善/修改功能
* 应用程序 > 应用程序
    * 在Weibo登录认证的附加信息中添加了universalLink项目。

### 2023. 05. 16.

#### 添加功能
* 购买(IAP) > 商店
    * 添加了MyCard商店。
* 优惠券 > 发放优惠券
    * 添加了Galaxy商店。

#### 2023. 04. 25.

### 添加功能
* 推送 > 推送
    * 将分页添加到推送预约列表。
* Analytics > 用户指标 > 流入/流出
    * 添加了每月流入/流出指标

### 2023. 04. 11.

#### 改善/修改功能
* Analytics > 销售指标 > 付费用户
	* 改善后，可以同时显示用户指标和筛选器。

### 2023. 03. 28.

#### 添加功能
* 购买(IAP) > 付款信息
    * 添加了可查看未注册商品的支付明细并注册未注册商品信息的功能。

#### 改善/修改功能
* Analytics > 用户指标 > 用户指标
	* 添加了下载每日Excel时被遗漏的累积DAU指标。

### 2023. 03. 14.

#### 改善/修改功能
* 购买(IAP) > 付款信息
    * 改善了功能，允许将状态从“已完成付款预约”更改为“已完成付款”。(Google Play商店除外。)

### 2023. 02. 28.

#### 改善/修改功能
* 购买(IAP) > 商品
    * 改善了功能，使每个商店商品的状态更改也可以应用于Gamebase商品。

#### 添加功能
* 成员 > 会员
    * 添加了使用Device Key搜索会员的功能。 

### 2023. 02. 14.

#### 改善/修改功能
* 管理 > 报警 > 收件人
	* 还支持IAM成员的SMS报警接收功能。

### 2023. 01. 10.

#### 添加功能
* 应用程序 > 应用程序
    * 添加了设置“支持商品类型(单一商品/多重商品)”的功能。
* 推送 > 统计
    * 添加了可以查看消息发送失败的原因的功能。
* Analytics
	* 在多重选择筛选器的国家项目中添加了滚动功能。

#### 改善/修改功能
* 购买(IAP) > 商品
    * 设定了“单一产品”的游戏已更改为每个外部商店道具只注册一个Gamebase产品。

### 2022. 12. 27.

#### 改善/修改功能
* Analytics > 用户指标 > Retention
    * 将最多提供时期从90天已修改为180天。
   
### 2022. 11. 29.

#### 添加功能
* Analytics > 销售指标 > 道具销售指标
	*  添加了进行日查询时各道具PU项目。
* Analytics > 销售指标 > 首购
	* 添加了首次购买时所需时间项目

### 2022. 11. 15.

#### 添加功能
* Analytics > 用户指标 > 用户指标
	* 进行一天查询时添加每月累计用户项目。

### 2022. 10. 25.

#### 添加功能
* Analytics > 销售指标 > 付款金额
	* 进行一天查询时添加每月累计付款金额项目。

#### 改善/修改功能
* Analytics > 用户指标 > 使用环境
	* 修改后当搜索条件设置为Device，下载数据文件时显示所有设备。

### 2022. 10. 04.

#### 添加功能
* Analytics > 销售指标 > 付款金额
	* 在商店列表中添加“查看更多”功能。 
* Analytics > 用户指标 > 用户指标
	* 添加平均登录次数项目
* Analytics > 销售指标 > 付费用户
	* 进行一天查询时添加每月累计PU项目。

#### 改善/修改功能
* Analytics  
	* 改善后包括传送指标在内的的所有菜单上可使用多个可选过滤器。
	* 改善后下载Excel时可按国家类别创建Sheet。
* Analytics > 销售指标 > 付款金额
	* 改善后可固定输出商店的顺序。

### 2022. 09. 14.

#### 改善/修改功能
* 应用程序 > 应用程序
	* 对Line IdP添加了Multi Channel(Japan/Thailand/Taiwan/Indonesia)功能。
* 购买(IAP) > 商店
	* 添加了Onestore API v6(SDK v19)。
* 购买(IAP) > 付款信息
	* 在支付明细文件中添加了国家代码和附加信息项目。

#### 修改程序错误
* 应用程序 > 应用程序
	* 修复了首次激活“Gamebase提供的客户服务”时出现间歇性失败的问题。

### 2022. 07. 26.

#### 改善/修改功能
* 购买(IAP) > 付款信息
	* 在付款明细中添加了结算日期和时间项目。 

#### 修改程序错误
* 购买(IAP) > 付款信息
	* 修复了由于有很多付款明细而保存文件时出现的错误。
* 移动客户服务
	* 修复了附件名称中存在空格时的下载错误。

### 2022. 06. 14.

#### 改善/修改功能
* 优惠券 > 发放优惠券
	* 添加了设置优惠券有效期内小时和分钟的功能。

### 2022. 05. 24.

#### 改善/修改功能
* 购买(IAP) > 商店
	* 添加了OneStore外部支付。

#### 修改程序错误
* 应用程序 > 条款
	* 修复了在Android WebView中显示手动输入的条款详细页面时出现不必要的横向滚动条的问题。
               
### 2022. 04. 26.

#### 改善/修改功能
* 应用程序 > 应用程序 
	* 在NHN Cloud服务商品客户服务中添加了输入内容提供者ID(CPID)的功能。
* 应用程序 > 客户端
	* 修复后，在显示服务状态的消息中输入换行符（“\n”）时，由客户端SDK执行换行处理。 
* 成员 > 会员    
	* 添加了处理用户退出时，如果推出的用户当前已登录则终止连接的功能。 
* 购买(IAP) > 商店 
	* 对Google Play商店添加了服务账户关联。 

#### 修改程序错误
* 客户服务 > FAQ
	* 修复了在FAQ类型管理中输入特殊文字时错误消息输出为“重复类别错误”的问题。

### 2022. 04. 12.

#### 改善/修改功能
* 购买(IAP) > 商品
	* 修复后在Amazon商店中对“商店项目ID”只能注册1个“Gamebase商品”。

### 2022. 03. 29.

#### 改善/修改功能
* 应用程序 > 应用程序
	* 修复后在iOS上通过谷歌SDK登录。 

#### 修改程序错误
* 应用程序 > 设置URL
	* 修复了第一次保存数据时由于同时在多个窗口中保存数据而导致的错误。

### 2022. 03. 15.

#### 修改程序错误
* 购买(IAP) > 付款信息
	* 修复了搜索收票时项目名称显示不正确的问题。 
	* 修复了将大量付款历史查询记录保存为Excel文件时出现的错误。
* 运营 > 维护
	* 修复了维护页面为外部页面时未能修改维护信息的错误。
* 推送 > 设置
	* 修复了更改设置后未能进行保存的错误。

### 2022. 02. 22.

#### 改善/修改功能
* 客户服务
	* 在Gamebase提供的客户服务支持语言中添加了汉语(繁体)和俄语。
* 应用程序 > 客户端
	* 在“必须更新”弹窗中添加了**查看更多**按钮。    
* 购买(IAP) > 商店
	* 添加了Amazon Appstore和Huawei AppGallery商店。
* 推送 > 推送
	* 添加了“点击按钮执行功能”和自定义字段。
* 推送 > 设定
	* 添加了预约发送“是否同意接收推送广告”消息的功能。  

#### 修改程序错误
* 购买(IAP) > 商品
	* 修改了当无法以CSV文件形式注册商品时，在特定环境下不显示错误消息的程序错误。  

### 2022. 01. 25.

#### 改善/修改功能
* 运营 > Kickout
	* 添加了选择是否显示Kickout弹窗的功能。 

### 2022. 01. 11.

#### 改善/修改功能
* 客户服务
	* 添加了用户可从客户服务支持的语言当中选择显示语言的功能。

### 2021. 12. 28.

#### 修改程序错误  
* Analytics > 用户指标 > 流入/流出
	* 修改了显示新/退出用户时，被显示的用户数量增加2倍的错误。

### 2021. 12. 14.

#### 改善/修改功能
* 应用程序 > 应用程序 > 客户服务
	* 添加了用户可通过网络浏览器直接跳转到客户服务网页注册1:1查询的功能。
* 运营 > 维护
	* 添加了在维护网页上可选择“是否显示维护时间”的功能。
* 客户服务 > 模板
	* 在以前注册的模板中，删除或不显示用客户服务不再支持的语言输入的模板内容。
	
#### 修改程序错误
* 引用程序 > 引用程序 > 客户服务	
	* 修复了即使将设置为支持语言，客户服务也不显示汉语的错误。

### 2021. 11. 23.

#### 改善/修改功能
* 应用程序 > 客户端
	* 添加了服务器地址和与URL设置有关的说明。  
* 禁止使用 > 禁用
	* 改善了注册禁用的页面  

#### 修改程序错误
* 禁止使用 > 禁用
	* 修改了缩小屏幕宽度时无法选择时期选项中的“日/时间”的错误。

### 2021. 11. 09.

#### 改善/修改功能
* 应用程序 > 设置URL
	* 为新创建的Gamebase提供的ShortURL域名已更改为https://nh.nu。

#### 修改程序错误
* 运营 > 维护
	* 修改了注册自定义HTML(Webview)维护时, 未能正常看到“预览”的错误。

### 2021. 10. 26.

#### 改善/修改功能
* Analytics
	* 将下载文件的形式更改为.csv和.xlsx。
* 运营 > Kickout
	* 添加了复制Kickout详细明细的功能。

### 2021. 10. 12.

#### 改善/修改功能
* 推送 > 统计 > 发送/接收、接收设置 
	* 将下载文件的形式更改为.csv。     
* 优惠券 > 优惠券使用履历、优惠券发布列表
	* 将下载文件的形式更改为.csv。

#### 修改程序错误
* Analytics > 用户指标 > Life Cycle
	* 修改了加入的当天将退出用户指标显示为0的程序错误。   

### 2021. 09. 28.

#### 改善/修改功能
* 购买(IAP) > 结算Abusing监测
	* 添加了结算Abusing自动解除功能。
* 购买(IAP) > 商店
	* 支持ONE store SDK v16。

#### 修改程序错误
* 购买(IAP) > 商品
	* 修改了通过文件注册道具时，因存在多个商店信息而出现不正确的“注册错误”的问题。
* 推送 > 推送
	* 修改了为没有标题但只有文本的消息设置文本颜色时出现“发送错误”的问题。

### 2021. 09. 14.

#### 改善/修改功能
* 推送 > 统计 > 接收设置
	* 更改了与接收设置统计有关的语句。
* 客户服务
	* 添加了用户可根据查询类型输入模板的功能。
	* 添加了通过模板回复客户提问时使用多国语的功能。
* Analytics
    * 选择过滤时提供MCU/ACU数据。
        * 从8月11号开始提供MCU数据。 
* Analytics > 实时监测 > 实时同时在线
    * 开始提供推送预约明细。

#### 修改程序错误
* 客户服务 > 客户查询
	* 修改了选择接收时期的日期时，日历显示不正确的错误。

### 2021. 08. 24.

#### 改善/修改功能  
* 客户服务 > 客户提问
	* 在下端客户信息中添加了查询明细。
* 购买(IAP)
	* 初次设置货币时，根据在NHN Cloud中选择的语言显示货币基准。

### 2021. 08. 10.

#### 改善/修改功能
* Analytics > 实时监测 > 实时同时在线
    * 将同时在线人数(CCU)变化图标的时间固定为GMT+9。
* 推送 > 注册推送
	* 添加了可删除输入的所有消息的功能。
* 客户服务 > 客户提问 > 推送响应设置
	* 改善之后，即使未设置推送响应设置，也将在应用程序 > 语言设置 > 支持语言中选择的语言全部显示出来。 
* 优惠券 > 发放优惠券
	* 修改了优惠券发布个数的提示语句。 

#### 修改程序错误   
* 应用程序 > 条款
	* 修改了在“预览条款”中未能看到部分国家名的错误。
* 购买(IAP) > 商店
	* 修改了未输入必须值时，在下端无法显示提示消息的问题。
* 成员 > 会员
	* 修改了查看不存在的成员时的通知弹窗提示语句。 

### July 27, 2021

#### Feature Updates
* Customer Center > Customer Inquiry
	* Improved to leave multiple answers when handling customer inquiries.
	* Improved to allow users to leave additional inquiries after leaving an initial inquiry.
	*  Add Status: To be used when needing additional confirmation after pending-replied.
* Purchase(IAP) > Payment Abusing Monitoring: Improved to display the user status in multiple languages in the payment history by user section.

#### Bug Fixes
* Coupon > Issue Coupon: Fixed an error where statistical data was omitted when sending coupon through SMS.

### July 13, 2021

#### More Features
* New mobile menu open: Retention    

### June 29, 2021

#### Bug Fixes
* Analytics > Dashboard: Fixed a bug where the PU accumulation value of the downloaded Excel file included duplicate PUs

### May 25, 2021

#### Feature Updates
* Purchase(IAP) > Item: Added a feature of allowing users to confirm low-level ID information when changing status of store Item

### May 11, 2021

#### Feature Updates
* App > App: Improved the view so that users can group and edit apps by feature

### April 13, 2021

#### Feature Updates
* Member > Member: Improved system to display the opt in date if opt in for advertising and opt in for night-time advertising are both true when viewing the push token
* Purchase (IAP) > Payment Information: Updated strings on the pop-up window for additional information to be displayed with line breaks
* Purchase (IAP) > Payment Abusing Monitoring
	* Updated automatic restriction detection period which was fixed to one hour is now customizable by users (1 hour to 48 hours)
	* Updated automatic restriction condition input of numbers and prices that only allowed AND condition to also allow OR condition

### March 23, 2021

#### Feature Updates
* Member > Download: Increased the size of data stored in a file (from 50,000 to > 500,000)

### March 09, 2021

#### More Features
* App > Terms and Conditions: Added the GDPR terms

### February 23, 2021

#### More Features
* Operation > Kickout: Added a feature to enable kickout for each client version
* Purchase (IAP) > Store: Added a feature to set one-time receipt verification phase for Google Play store

### February 15, 2021

#### Bug Fixes
* Purchase (IAP) > Payment History: Fixed an error displaying wrong item name on file download

### February 9, 2021

#### More Features
* Common Terms and Conditions added
	* New menus available: App > Terms and Conditions, App > Deploy Terms and Conditions

#### Feature Updates
* App > Client: Improved the screen to classify and display the client version per store

### January 26, 2021

```
Push > Push (Old) Console menu feature has been removed.
```

#### More Features
* User Ban > AppGuard: Conditional Ban added
* Purchase (IAP) > Payment Abusing Monitoring: Apple App Store added

#### Feature Updates
* Manage > Permissions: Sales Access Permissions removed [See the notice](https://www.toast.com/kr/support/notice/detail/2101)

### January 12, 2021

#### More Features
* Added a new push menu
	* Statistics: Checks push statistics such as push sent, received, token registration, etc.
	* Event key: Manages the event key used in push send statistics
	* Certificate: Manages the certificate used to push send
	* Setting: Manages the values related to push

### December 29, 2020

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added

#### Feature Updates
* App > App: beta service server added to Manage Server Address
* App > Client: beta service added to Client Status, memo function added so that additional client information can be registered
* Purchase (IAP) > Product: search condition added - whether currently being used or not
* Purchase (IAP) > Payment Information: now store test payment is displayed in the payment history
* Purchase (IAP) > Terminate Sales Menu: Analytics > Sales Metrics and functions are integrated now.
* Analytics > Preferences > Terminate Installation URL Menu

### December 15, 2020

#### More Features
* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [Console]
	* Analytics: Improved to maintain the selected search condition even when the date is changed after filtered search
	* Push > Push: Tencent Push removed
	* Purchase (IAP) > Transaction information: Changed not to show the Check Receipt button in the refund state
* [SDK] 2.18.2
    * (Common) TOAST SDK update: [Android(0.24.2)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0242-20201124), [iOS(0.27.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0271-20201124), [Unity(0.21.3)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-unity/#0213-20201124)
	* (Android) External SDK update to resolve encryption logic security warnings: Payco Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
	* (iOS) showWebView: Returns error when invalid URL is delivered, delivered URL is used as is without encoding
	* (iOS) Changed to run the custom scheme regardless of letter case
	* (Unity) The following fields of GamebaseRequest.GamebaseConfiguration class deprecated: zoneType, fcmSenderId

#### Bug Fixes
* [Console]
	* Purchase (IAP) > item: Fixed the issue where duplicate items are registered when a batch of items is added using a file
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### December 2, 2020

#### More Features
* [Console]
	* Newly added subdivided Gamebase permission functions: 24
	* Analytics > Group Metrics: new subscribers per project and payment amount comparison graph added
	* Members > Members: View Customer Center Inquiry History tab added at the bottom
	* Coupon > Issue Coupon: Issue Additional Coupons function added so that more coupons can be issued to an already issued coupon (100,000 at one time)

#### Feature Updates
* [Console]
	* Analytics > Live Monitoring: display color changes when there is a gain/loss in day-over-day metric data
		* Increase: blue->red, decrease: red->blue
	* Analytics > Sales Metrics > Payment Amount: Now sales data comparison can be viewed "per store" and "per IdP" in addition to the previous "per country"
	* Operation > announcement: setting to move to Customer Center added in the Read More link
	* Customer Center > Customer Inquiry: Auto Translation function added in the Send Reply settings
	* Coupon > Issue Coupon: increased the initial coupon issuance quantity from 50,000 to 1,000,000

#### Bug Fixes
* [Console]
    * Purchase (IAP) > Payment Information: fixed the issue where files cannot be downloaded when too much data is viewed

### November 10, 2020

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: [Android(0.24.1)](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-android/#0240-20201027) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings
    * (iOS) Added API that supports the SceneDelegate of iOS 13 or higher

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### October 27, 2020

#### More Features
- Added the Unreal SDK feature: SDK 2.15.0
    - Added GamebaseEventHandler that integrates all existing event systems
        * It includes the ServerPush and Observer features and checks promotion transaction event and push event.
    * Added API
    	* Added an API that can be used to send a transaction request with product ID, enter additional information (UserPayload), and check them after the transaction is complete
    	* Display image notification: showImageNotices
    	* Check the Push token information: queryTokenInfo
    * Added a feature that allows foreground apps to receive push notifications using the NotificationOption setting if push token is registered.
    * Added the WebViewConfiguration contentMode setting

#### Feature Updates
* [SDK] 2.17.1
    * (iOS) When sending a specific index, an error message is added to it: When push registration fails or when sending a game index
    * (Unity) Supports Unity 2017.2.5
* [SDK] 2.15.0
    * (Unreal) TOAST SDK update: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)  

#### Bug Fixes  
* [Console]
    * Analytics > User Indexes: Fixed an issue where the weekly and monthly average CCU were displayed abnormally with their logic modified.
    * Push > Push: Fixed an issue where null was displayed on the title when a non-black color was used without entering any text for the title.
	* Coupon > Issue Coupon: Fixed an issue where the coupon file could not be downloaded when there were more than 50,000 coupons issued.
* [SDK] 2.17.1
    * (Unity) Fixed an issue where the latter API would not work when the image notification API and web view API were called in turn.
* [SDK] 2.15.0    
    * (Unreal) Fixed an issue where the ProGuard declaration was missing in the transaction module

### October 13, 2020

```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
	* (Common) Supports the download feature when a Customer Center attachment image is clicked
	* (Common) TOAST SDK update: Android(0.23.2), Unity(0.21.2)
	* (iOS) Changed the type of TCGBMember.regDate and TCGBMember.lastLoginDate to long long.
	* (iOS) Changed the logic so that the title can be displayed again after changing URL and title in a web view

#### Bug Fixes  
* [SDK] 2.17.0
	* (iOS) PAYCO authentication: Fixed an issue where the logout callback would not return when logout was called after lastLoggedInProvider login
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0

### September 22, 2020

#### More Features
* Added a feature to Customer Center
	* [Console] The Customer Center menu is now available: To handle customer queries and manage FAQ/notifications
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API

#### Feature Updates
* [Console]
	* Analytics menu (common): Changed the way filters are sorted for each country (index descending -> name ascending)    
    * Analytics > Sale index: Displays the total transaction amount as well as the transaction amount for each country for the country store in store-specific dashboards

### September 16, 2020

#### Feature Updates
* [SDK] 2.15.1
    * (iOS) TOAST SDK update: iOS(0.27.0)
	* A new version of IAP SDK is applied to support the changes made to iOS 14 beta. [TOAST SDK Release Notes](https://docs.toast.com/ko/TOAST/ko/toast-sdk/release-notes-ios/#0270-20200911)

### September 15, 2020

#### More Features
* [SDK] 2.15.0
    * (JavaScript) Added GamebaseProductId to Purchase API of Hangame points

#### Bug Fixes    
* [Console]
    * Fixed Purchase (IAP) > Payment Information: Fixed an issue in which authentication by receipt did not properly show

### August 25, 2020

```
Updated Google Billing Client in the Gamebase SDK 2.15.0 version.

For 'gamebase-adapter-purchase-google', to upgrade a version below Gamebase SDK 2.15.0 to more than 2.15.0,  
set 'Requires Update' for 'Game Client Version' of the previous version.

This is because, in order to execute reprocessing when an error occurs during purchasing an item,  
you may encounter an issue during reprocessing if a different billing client version is applied to each of many devices.   
```

#### More Features
* [SDK] 2.15.0
    * (Common) Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting  
    * (Common) Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)
* [SDK] 2.9.1
    * (Unreal) Supports Unreal 4.22 ~ 4.25
    * (Unreal) Supports PLCrashReporter Issue: [Guide](http://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#ios-settings)

#### Feature Updates
* [Console]
    * Push > Push: Modified to allow sending without sender's contact information or method of unsubscription when notification is sent for promotional push
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)
    * (iOS) Added the null check logic for the payload of payment
* [SDK] 2.9.1
    * (Unreal) Updated Gamebase SDK version for iOS within iOS Plugin (2.9.1)
    * (Unreal) Fixed the missing part of UObject referencing

#### Bug Fixes
* [Console]
    * Push > Push: Fixed an issue in which time was identically applied with UTC+9, for delivering repetitive push notification, regardless of timezone

### August 19, 2020

#### Bug Fixes
* [Console]
    * The Entire Menus of Analytics: Fixed the unavailability of downloading excel files

### August 11, 2020

#### Feature Updates
* [Console]
    * Analytics > User Indicators > Retention: Show numbers, as well as %
* [SDK] 2.14.0
    * (iOS) Removed Constant Value of PAYCO IdP: Due to rejections made on Apple inspections thanks to PAYCO character strings
    * (iOS, Unity) Adde the contentMode setting for TCGBWebViewConfiguration
* [Server]
    * Added error code for Coupon Expired API: When a coupon code includes a value other than English or numbers (Error Code:-4000205)

### July 28, 2020

#### More Features
* [Console]
    * Analytics: Added the WAU (Weekly Active User) and MAU (Monthly Active User) indicators
* [SDK] 2.13.0
    * (Unity) Standalone: Added Show Notice on Image API     

#### Feature Updates
* [Console]
    * App > App: Modified to enter further information to authenticate Sign In With Apple on iOS 12 or lower versions  
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image
    * (iOS) Authenticate Sign In With Apple: Supported for iOS 12 or lower

#### Bug Fixes
* [Console]
    * Operations > Notice on Image: Fixed the feature of copying, as well as error in which selected countries are not properly changed to all countries
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module

### July 14, 2020

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order
    * [Console] Operations > Image Notices: Menu added  
    * [SDK] 2.12.0: Added Show Image Notice API

#### Feature Updates
* [Console]
    * Purchase (IAP) > Products: Products can be queried by item number
    * Membership > Member: Updated to change the status of users who are suspended from withdrawal to normal
    * Membership > Download: Added deviceKey and IdP code to the history of login logs
* [SDK] 2.12.0
    * (iOS) Updated Facebook SDK (7.1.1)
    * (iOS) Attempts Gamebase initialization with storeCode(default=AS) set for configuration
    * (iOS) Fixed failed closing due to lack of the close button while printing webview which cannot load content
    * (Unity) Updated TOAST Unity SDK (0.20.1.1)

### June 23, 2020

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed

#### Feature Updates
* [Console]
	* Purchase (IAP) > Products: Updated to register and manage many Gamebase products for a store item ID  

### June 9, 2020

#### Feature Updates
* [Console]
	* Membership > Member: Additionally shows the status of withdrwal suspension (withdrawal suspended, cancelled, or immediately withdrawn) on the **Query Withdrawal History**
* [SDK] 2.10.1
	* (iOS) Updated to set device language if language code is not configured when user push setting is initialized

#### Bug Fixes
* [Console]
	* Coupons > Issue Coupons: Fixed the inavailability of downloading history of coupon statistics sent via SMS

* [SDK] 2.10.1
	* (Unity) Fixed failed login calls since ViewController is not configured at iOS Plugin
	* (JavaScript) Fixed errors that occur if StoreCode is not entered during initialization


### May 26, 2020

#### More Features
* [Console]
	* Coupons > Issue Coupons: Added features of delivery statistics and downloading history of coupon deliveries  
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems
		* Includes ServerPush and Observer, and checks promotional purchase or push events

#### Feature Updates
* [Console]
	* All: Updated button/tag UIs to suit for common design guides  
* [SDK] 2.10.0
	* (Unity) Updated CefWebview version with StandaloneWebviewAdapter: v2.0.4
		* Updated the logic of WebviewIndex validation  
		* Fixed infrequent error of NullReferenceException while Webview is created

### May 12, 2020

#### More Features
* [SDK] 2.9.0
	* (Unreal) Newly released SDK

#### Feature Updates
* [Console]
	* App > App: Updated to save TOAST accounts of the users who changed the grace period of withdrawal  
	* Member > Member: Fixed an issue in which data does not properly show when mapping history is queried
	* Purchase (IAP) > Store: Test, Old) OneStore is updated not to allow new registration  

#### Bug Fixes
* [SDK] 2.9.1
	* (Andoird) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error

### April 29, 2020

#### Bug Fixes
* [SDK] 2.9.1
	* (Unity) Fixed a error which occurs when client service status is changed after initialized on console
		* Versions at Issue: v2.8.0 or higher
		* Platforms at Issue: Standalone, WebGL, and Editor

### April 28, 2020

#### More Features
* Suspension of Membership Withdrawal
	* [SDK 2.9.0]
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  
	* [Console]
		* Apps > Apps: Added features allowing to set suspension period for withdrawal
#### Feature Updates
* [SDK 2.9.0]
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated Payco Login SDK: Android(v1.5.0), iOS(v1.4.0)
* [Console]
	* All Menus: Changed the design of buttons and tags on console
	* Operations > Maintenance, Operations > Notice, Push: Supports auto-translation in multiple languages
	* Members > Membership: Further shows suspension period expired, when querying members who are suspended for withdrawal

### April 14, 2020

#### Feature Updates
* [Console]
	* Analytics Common: Updated the TUI chart version, and applied to Frequency7 indicators
* [SDK] 2.8.1
	* (Common) Added internal indicators to check Analytics delivery results

#### Bug Fixes
* [Console]
	* Analytics Common: Fixed an issue in which the scroll is deviated from area for a long country name
	* Analytics > Real-time Monitoring: Fixed an issue in which indicator shows 0 when query is requested while saving data
* [SDK] 2.8.1
	* (Android) Modified codes that may cause crashes after process restarts
	* (JavaScript) Modified an issue in which credentialInfo login is unavailable with Hangame IdP

### March 24, 2020

#### More Features
* [Console]
	* Release of New Menus: Analytics > User Indicators > Frequency 7
		* Provides weekly visits and rate of DAU. You can find the level of immersion and loyalty for a game.
	* Coupon > Publish: Texting coupons is now available  
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices
	* (Unity) Updated CefWebview to v2.0.1 within StandaloneWebviewAdapter
		* When the PopupType is PASS_INFO, popup data can be delivered without a popup
 	* (Javascript) Supports Hangame channeling: Hangame IdP authentication and purchase with Hancoin


#### Feature Updates
* [Console]
	* App > Transfer Indicator Setting: Allows pre-registered meta filters only for transfer indicators
		* Meta filter indicators that go beyond limit are not displayed.: Level (5,000), World/Server/Channel (100), and Occupation/Class (100)
* [SDK] 2.8.0
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login

#### Bug Fixes
* [Console]
	* Sales Indicators > Purchase Amount
		* Modified the display of currency, which is currently fixed at Won (KRW) for chart tooltips, to show as configured on the app  
		* Fixed the issue of not showing February indicators for monthly search

### March 10, 2020

#### More Features

- [Console]
	- App  >  App: Set whether to include test payment or not, when analytics sales indicators are displayed   
    		- With 'Exclude Test Payment', the analytics sales indicators show all but test payment.
		- Purchase (IAP): Set currency code for payment indicators on the initial access to purchase (IAP) menu
	- Only the initial setting is available, and the Analytics sales indicators show in the configured currency code.
  	- Added the 'View on Desktop' feature on the mobile console (including TOAST app)

#### Feature Updates

- [Console]
  	- App  >  Installation URL: Additionally apply available scheme for input URL  
    		- Previously: Common ('http://', 'https://'), Android('market://')
    		- Now: iOS('itms://', 'itmss://', 'itms-apps://'), Android('intent://')
- [SDK] 2.7.2
  	- (Unity) Updated FacebookAdapter  
    		- Compatibility testig from v7.9.4 to v7.18.1
    		- Null exception handling   
  	- (Unity) Updated StandaloneWebviewAdapter
    		- Added the feature of exporting webpages in texture
    		- Support multiple webviews  
    		- Added the option of deleting cookies  
    		- Supports sizing of texture  
		- Supports showing/hiding the scrollbar
    		- Notifies the completion of page loading  
    		- Supports transparent background
  	- (Unity) Fixed an error which occurs when Android/iOS is selected from Editor and Initialize API is called

#### Bug Fixes

- [Console]
  	- Analytics: Fixed an issue in which the sales indicator is displayed as '0' when the currency code is in coins

### February 25, 2020

#### More Features
* [Console]
	* Coupon > Publish: Added the feature of allowing published coupons to be used only at certain stores.   

#### Feature Updates
* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called
* [Console]
	* App > App: Added the notification logic when a same client is re-registered after deleted
	* Purchase (IAP) > Item: Added the registration field to register subscription product (App Store - Shared secret,Google store - Domain authentication File Names)

#### Bug Fixes
* [Console]
	* Analytics > Real-time Monitoring > Real-time Indicators: Fixed empty or infinity issues that infrequently occur for ccu after push is sent
	* Analytics > Transfer Indicators
		* Fixed bugs that are not updated to No Data when data is gone from grid
		* Fixed the vertical display of buttons when the filter name is short

### February 11, 2020

#### More Features
* [Console]
	* Allows to see the flow of user indicators at a glance, from creating a new menu release project at Analytics > User Indicators > Life Cycle
	* Management > Authority: Added the role of receiving Weekly Report
		* The 'Weekly Report' is to be mailed from March.

#### Feature Updates
* [Server API] Added validation for the regUser length when Withdraw API is called
* [Console]
	* Analytics: Apply Japanese fonts for Grid, Chart
	* Purchase: Updated to let users intuitively view popup message display when error occurs

#### Bug Fixes
* [Console]
	* Analytics: Modified the currency display from 'Yen (JPY)' to 'Won (KRW)' when the language is changed to Japanese

### January 21, 2020

#### More Features
* [SDK] 2.7.0
	* (Unity) Supports NaverCafePLUG

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response
	* (Android) Modified not to occur exceptions when Firebase setting is missing
	* (Unity) Added the gamebase://dismiss scheme handling for a web login
	* (Unity) Modified infrequent failure in the display of webview for a release build 	
* [Console]
	* Analytics: Fixed failed re-direction to a login page when the user session is expired

### January 14, 2020

#### More Features
* [Server API] Added Withdraw Users API

#### Feature Updates
* [SDK] 2.6.3
	* (Unity) Updated Standalone Webview: Updated CefWebview 	
	* (Unity) Added .dll file missing from an error occured after login
		* ToastCommon.dll, vcruntime140.dll

#### Bug Fixes
* [SDK] 2.6.3
	* (Unity) Fixed error that occur when Login(CredentialInfo) API is called

### December 24, 2019

#### More Features
* Conpon > Publish: Added the feature of keyword coupons

#### Feature Updates
* [Console]
	* Purchase > Query Payment Information: Added the column for additional information
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)
	* (iOS) Naver SDK Updates (4.1.0)

### December 10, 2019

#### More Features
* App > App: Allows to register devices for QA testing via IP as well  

#### Bug Fixes
* [Console]
  * Modified incorrect Japanese
* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize()
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address
  * (Android) Fixed crash occurrence when IAP is not enabled  
  * (iOS) Fixed the issue in which mapping is not available when AddMapping (Forcibly) is applied
  * (iOS) Fixed crash occurrence by NSNUll object, when displayLanguageCode of PushConfiguration is not set by Unity Plugin

### November 26, 2019

#### Bug Fixes
* [Console]
  * Coupons >Issuance: Fixed the abnormal downloading when downloading a coupon after session is expired  
  * Analytics > Real-time Monitoring > Dashboard: Fixed data of the previous date wrongly displayed as 0  
     Fixed the issue in which the page is not properly displayed for a disabled product, when accessing relevant menu of TOAST Product (e.g. IAP, Push, or AppGuard)

### November 20, 2019

#### Bug Fixes
* [SDK] 2.6.1
  * (Unity) Added the iOS Plugin file (toast_sdk_wrap.m) to Package in order to prevent errors for an iOS build  
  * (Unity) Fixed the error of UnityEditor in which the store code comes as empty on a platform other than Standalone, resulting in failed initialization  
  * (Unity) Fixed the error of NullReferenceException due to errors from processing zone type within Initialize API

### November 13, 2019

#### Bug Fixes
* GamebaseSettingTool
  * Fixed the error in which files are not properly updated, with the version updated to Gamebase v2.6.0


### November 12, 2019

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features
* Coupon Service Newly Open: Create and manage coupons in large quantity
  * [Console] Newly opened the Coupon Menu             
  * [Server API] Find coupons and add Consume API
* Auto anti-abusive payment practices  
  * [Console] Purchase (IAP) > Newly opened monitoring menu for abusive payment practices
    * Auto banning against abusive payment
    * Manual service suspension after searching conditions for abusive payment
* Added the payment feature for Google subscription  
  * [SDK] 2.6.0 Android
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis
  * (iOS) Added authentication for Sign In with Apple
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase.

#### Feature Updates
* [SDK] 2.6.0
  * (Unity) Fixed the error in the logic of updating LaunchingStatus for a login
  * (Unity) Fixed the error of endless repetition of log delivery from the client, if delivery of debug logs is set on the Gamebase console
* [Console]
  * App >App: Allowed to enter each service status (e.g. testing, under inspection, or in service) of a server address  
  * Purchase (IAP) > Payment Information: Changed UI to search by selecting search conditions

### October 29, 2019

#### Feature Updates
* [Console]
  * Analytics: Changed tooltips in the pie chart
  * Analytics > Real-time Monitoring: More targets for push delivery


### October 15, 2019

#### Feature Updates
* [SDK] 2.5.2
	* (iOS) Changed UIWebView into WKWebView
* Sample App
	* Updated Gamebase SDK (v2.4.0)
	* Applied Smart Downloader (v1.5.8) and Leaderboard
	* More Features: Downloading game resources, integrating Leaderboard and TAA indicators, transferring devices, and forced mapping
	* Updates: Added ServerPush listeners and detection of observer maintenance  
	* Renewed games

#### Bug Fixes
* [Console]
	* Management > Authority: Fixed failed modification of authority
	* For Mobile
		* Fixed the issue of keyword enabled by selecting Datepicker
		* Analytics: Fixed the issue of NRU value exposed for ARPPU

### September 24, 2019

#### Feature Updates
* [Console]
	* App >Client: Modified UI to select stores for the registration of web client  
#### Bug Fixes
* [Console]
	* Management > Authority: Fixed the failed modification of authority
	* For Mobile
		* Fixed the issue of keyword enabled by selecting Datepicker
		* Analytics: Fixed the issue of NRU value exposed for ARPPU

### September 10, 2019

#### More Features
* [Console]
	* Analytics: Further shows the level indicator for channel/world/server and occupation/class transfer indicators

#### Feature Updates
* [Console]
	* Analytics: Performance updated for grid rendering (with tui-grid 4.4x)
* [SDK] 2.5.1
	* (iOS) Updated to TCPushSDK 1.7.0 which is for GamebasePushAdapter
		* Since file has changed from static library to framework for TCPushSDK, TCPushSDK.framework must be added to project.

### August 27, 2019

#### More Features
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview

#### Feature Updates/Changes
* [Console]
	* Analytics: Updated chart performances

### August 2, 2019

#### Bug Fixes
* [SDK] Setting Tool 1.4.3
	* Fixed the build error by moving down the script file below the editor folder  
	* Fixed failed operations when Multilanguage is provided with the entire path of the language file on MAC OS.

### July 23, 2019

#### Added
* [Console]
	* New Menu Released on Member > Download: As of the last login time on the date of service joining, list of game users can be queried and downloaded in files   

#### Feature Updates
* [Console] Mobile
	* Can register and modify maintenance and push  
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)
* Setting Tool
	* Change of Folder Structure: Must reinstall after previous SettingTool is completely deleted  
	* More languages are supported

#### Bug Fixes
* [Console]
	* Analytics > User Indicators: Fixed the issue of date overlaps on the axis X

### July 11, 2019

#### Feature Updates/Changes
* [Console] Analytics
	* Improved in level-up query performance
	* Shows Min./Max. data within a chart
	* Supported in multiple languages (Chinese)

#### Bug Fixes
* [SDK] 2.4.3
	* (iOS) Fixed crash occurrence due to parsing attempts of error messages with conflicting formats, regarding authentication
	* (Unity) Fixed failed operations of AddMappingForcibly API, for the builds in iOS or Android
	* (Unity) Fixed the json parsing error at iOSPlugin when RequestRetryTransaction API is called

### July 1, 2019

#### Bug Fixes
* [Console]
	* Management > Alarms: Fixed the failure in modifying set alarm after Webhook is set up  

### June 27,2019

#### Bug Fixes
* [Console]
	* Service Suspension: Fixed failure in uploading files to register suspension of service in mass
* [SDK] Setting Tool 1.4.1
	* Fixed the error in uploading existing setting data when GamebaseSettingTool was executed

### June 25, 2019

#### Feature Added
* More Transfer Indicators
    * [Console]Analytics>Transfer Indicators: Menu available to check indicators per Level, Channel, or Class
		* Status in real time
		* Status by level
		* Status of the world, or each server or channel
		* Status of each class or profession
		* Level-ups
		* Sales status of items
		* Top 50 best-selling items

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo
	* (iOS) LINE SDK Updated (v5.0.1)
		* The minimum support OS version for LINE Adapter has changed to iOS 10
		* Login also available via LINE app

#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer.
	* (iOS) Fixed infrequent crashes occurred out of network connection issues

### June 13, 2019

#### Bug Fixes
* [SDK] 2.4.1
	* (iOS) Fixed the error in output of indicators due to missing of partial parameters during transfer of Analyticis indicators

### May 28, 2019

#### Feature Updates
* Purchase for HANGAME mix Available for Japan
    * [SDK] 2.4.0
      * (Unity) Added external purchase for Standalone Japan  
      * (Unity)  Added HANGAME authentication for Standalone Japan
    * [Console]
      * Purchase>Store: Added 'HANGAME mix (JAPAN)' Store
      * App>Client: Added store setting for client registration on Windows
      * App> Installation URL: Added store setting to add installation URL on Windows

#### Feature Updates/Changes
* [SDK] 2.4.0

  * (Common) Chanage of Classes Relevant to Indicators
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#game-user-data-settings) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#game-user-data-settings) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#game-user-data-settings) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#game-user-data-settings)]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](http://docs.toast.com/en/Game/Gamebase/en/aos-etc/#level-up-trace) / [iOS](http://docs.toast.com/en/Game/Gamebase/en/ios-etc/#level-up-trace) / [Unity](http://docs.toast.com/en/Game/Gamebase/en/unity-etc/#level-up-trace) / [JavaScript](http://docs.toast.com/en/Game/Gamebase/en/js-etc/#level-up-trace)]

    * (Android) Naver SDK Version Updated (v4.2.5): Bug of Naver SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while login to Naver was underway)  
    * (Unity) StandaloneWebview supports 32bit Build (SDK volume upgraded from 53.6MB to 99.2MB)
* [Server]

    * Modified LTV queries and the failover logic
* [Console]

    * Support available for LTV Grid ComplexColumns and excel downloading
