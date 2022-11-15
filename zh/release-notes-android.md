## Game > Gamebase > Release Notes > Android

### 2.44.1 (2022. 10. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.1/GamebaseSDK-Android.zip)

#### 添加功能
* 添加了**PushConfiguration.Builder.enableRequestNotificationPermission(boolean)** API，以防止在Android 13以上版本的OS上调用registerPush API时自动弹出请求Push权限的弹窗。

#### 改善/修改功能
* Facebook Android SDK 13.2.0以上版本需要Facebook Client Token设置。
    * 从Gamebase Android SDK 2.44.1以上版本开始，如果在Gamebase Console的additionalInfo中添加以下**facebook_client_token**字段，Facebook Client Token则将自动应用到客户端SDK。

            { "facebook_permission": [...], "facebook_client_token": "a01234bc56de7fg89012hi3j45k67890" }

#### 修改程序错误
* 修改了在Android 6.0(M, API Level 23)终端机调用**Gamebase.Push.registerPush** API时出现**IllegalArgumentException**例外的错误。

### 2.44.0 (2022. 10. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : NHN Cloud Android SDK(1.2.0), TOAST Gamebase IAP Android SDK(0.21.0), Google Play Services Auth(20.0.3)
* 在Android 13 OS上调用registerPush时自动显示一个请求接收通知的弹窗。
* 改善了内部逻辑以Google登录时使用silentSignIn API。

#### 修改程序错误
* 当通过Hangame Idp登录时，若通过有效的Third Idp试图进行登录后，再通过不支持的Third Idp试图Hangame登录时则不出现错误，而如果通过以前的Idp登录则出现崩溃，而目前此错误被修改。

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加了ONE store v19 Purchase Adapter。
    * 在Build的依赖性添加**gamebase-adapter-purchase-onestore-v19**模块儿和[ONE store v19 IAP SDK后](https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs)可以使用。
            
            dependencies {
                ...
                implementation files('libs/iap_sdk-v19.00.02.aar')
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
            }
            
#### 改善/修改功能
* 外部SDK升级 : Google Billing Client(5.0.0), NHN Cloud Android SDK(1.1.0), TOAST Gamebase IAP Android SDK(0.20.0), Kakaogame Android SDK(3.14.4)
* 添加了可输入Line Login时提供服务的Region的参数。
    * [Game > Gamebase > Android SDK使用指南 > 认证 > Login with IdP](./aos-authentication/#login-with-idp)
* 添加了防御逻辑，以防止在使用Line IdP时，在Line IdP不支持的低于API 19的终端机上出现崩溃。

#### 修改程序错误
* 修改了为了使用Naver PLUG SDK或Naver Cafe SDK而将Naver Login SDK版本强制降为4.1.4时出现崩溃的问题。

### 2.42.1 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : Facebook Android SDK(11.3.0)

### 2.42.0 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : Hangame Android SDK(1.5.2)
* 在ForcingMappingTicket VO类中添加了显示映射用户状态的mappedUserValid字段。
* 如果Gamebase Adapter版本与Gamebase版本不一致，则可能出现运行时例外，因此更改它，使其无法初始化。

#### 修改程序错误
* 修复了在LDPlayer中Naver网路登录失败的问题。
* 修复了由于OS版本较低导致Twitter登录失败时发生的崩溃。

### 2.41.2 (2022. 07. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.2/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 将默认Webview设置更改为“允许Cookie”。

### 2.41.1 (2022. 07. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-Android.zip)

#### 修改程序错误
* 修复了条款窗中“查看”按钮不正常启动的错误。

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.31.1), Hangame Android SDK(1.4.6)
* 在Webview中注册的CustomScheme事件启动时Webview将自动关闭。
    * 即使CustomScheme事件正常启动，也要维持Webview时， 请调用**GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)** API。

#### 修改程序错误
* 修复了在注销Hangame IdP后立即尝试登录时导致间歇性崩溃或登录失败的问题。

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Android.zip)

#### 添加功能
* 为了ONE store外部支付，添加了Purchase Adapter。
    * 如需使用，则在打包依赖性添加**gamebase-adapter-purchase-onestore-external**模块儿。 
            
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
            }
            
#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.31.0)、TOAST Gamebase IAP Android SDK(0.18.5)、LINE Android SDK(5.8.0)
* 更改了不同的应用程序使用同一个Gamebase项目时，无法正常启动推送功能的问题。
    * 请在AndroidManifest.xml中声明各应用程序的**com.nhncloud.sdk.push.deviceId.salt**值。

            <!-- When you have multiple applications sharing an Gamebase project, use this field to identify each application. -->
            <meta-data android:name="com.nhncloud.sdk.push.deviceId.salt"
                       android:value="ApplicationForGoogleStore" />

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.30.1)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加了Amazon(ADM) Push Adapter。
    * 如需使用，则在打包依赖性添加**gamebase-adapter-push-adm**模块儿。
            
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"
            }
            
    * 使用Proguard时，请先参考以下指南。
        * [NHN Cloud > SDK使用指南 > TOAST Push > Android > 设置Amazon Device Messaging > 升级ADM SDK](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#download-the-adm-sdk)
        * [NHN Cloud > SDK使用指南 > TOAST Push > Android > 设置Amazon Device Messaging > 设置Proguard](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard-settings)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.30.0)
* 更改了Display Language中汉语繁体(zh-TW)语言集中的错误句子。

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Android.zip)

#### 添加功能
* 为了在客户服务URL后边添加参数，添加了以下字段。
    * **ContactConfiguration.Builder.setAdditionalParameters(Map&lt;String, String&gt;)**

#### 改善/修改功能
* 外部SDK升级 : TOAST Gamebase IAP Android SDK(0.18.3)
* 更改后，当在Amazon Appstore结算数据中未输入userid和gamebaseproductid时，自动添加userid和gamebaseproductid。

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.29.2), TOAST Gamebase IAP Android SDK(0.18.2), Hangame Android SDK(1.4.5)
* 更改后，在Hangame Android SDK v1.4.5内部创建sms_hash。
    * 不需要设置sms_hash。

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Android.zip)

```
目前只能使用Maven Central发布Gamebase Android SDK。
发布专用ZIP文件不再包含AAR文件。  
```

#### 添加功能
* 添加了用于确认是否显示条款窗的API。
    * **Gamebase.Terms.isShowingTermsView()**
* 添加了在WebView中可固定文字大小的选项。 
    * **GamebaseWebViewConfiguration.Builder.enableFixedFontSize(boolean)**
* 添加了在条款窗中可固定文字大小的选项。 
    * **GamebaseTermsConfiguration.Builder.enableFixedFontSize(boolean)**
* 添加了登录Facebook、NAVER时，即使Facebook、NAVER等应用程序已被设置，也可强制登录网页的功能。
    * 如需使用此功能，请在Gamebase Console的AdditionalInfo中进行以下设置。 

```
{"enforce_app2web":true}
```

* 注销NAVER账户时不删除令牌。
    * 重新登录时不显示个人隐私协议窗。 
    * 登录网页时，保持原有账号。
    * 如需保持更改之前的状态，请在Gamebase Console中的AdditionalInfo输入如下信息。

```
{"logout_and_delete_token":true}
```

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.29.1)、Hangame Android SDK(1.4.4)
* 更改后，在显示条款窗时不显示长白色背景。

#### 修改程序错误
* 更改了未能正常启动用于隐藏WebView导航栏的**GamebaseWebViewConfiguration.Builder.setNavigationBarVisible()** API的问题。

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Android.zip)

#### 添加功能
* 在Gamebase控制台的“必须更新”设定中选择**添加弹窗按钮**时，客户端的必须更新弹窗中将添加一个**查看更多**按钮。
* 添加了可确认终端机是否允许提示通知的API。
    * **Gamebase.Push.queryNotificationAllowed()**
* 添加了当调用共同条款API后可确认条款UI是否被显示的VO类。 
    * **GamebaseShowTermsViewResult**

#### 改善/修改功能
* 由于在Gamebase控制台中注册Kickout时可以设置是否显示kickout弹窗，因此以下字段已被deprecated。
    * **UIPopupConfiguration.enableKickoutPopup**

#### 修改错误    
* 即使在图片通知中已点击**24小时内不显示**，但过24小时也不显示图片通知的错误被修改。 

### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加了可通过更改共同条款显示选项的新API。
    * 关于可以更改设置的项目，请参考以下指南。
    * [Game > Gamebase > Android SDK使用指南 > UI > Terms > showTermsView](./aos-ui/#showtermsview)

#### 改善/修改功能
* 外部SDK升级 : PAYCO Android SDK(1.5.7), Hangame Android SDK(1.4.3.1), TOAST Gamebase IAP Andoid SDK(0.18.1)
* 添加了登录成功后可检查Launching信息是否被更改的逻辑。   

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Android.zip)

#### 添加功能 
* 在GamebaseEventHandler的GamebaseEventCategory中添加了**GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED**类型。
    * 关于此事件的适用方法，请参考如下指南。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Server Push](./aos-etc/#server-push)
* 添加了当Gamebase Access Token过期，登录时需要启动的**GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Gamebase Event Handler > Logged Out](./aos-etc/#logged-out)
 
#### 改善/修改功能
* 改善Webview后，现在可启动Webview URL地址开始为**onestore://**的ONE store Deeplink。

#### 修改程序错误
* 修改了已在Gamebase Android SDK 2.31.0中调用退出函数，但因未调用IdP登录函数，无法更改IdP账户的程序错误。 

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.29.0)
* 解决了被禁用的用户（使用禁用信息）无法通过“禁用Webview”内客户服务链接注册查询的问题。
* 修复了打开应用程序后立即调用Gamebase初始化函数时，Launching弹窗显示英语的问题。
* 改善Scheduler之后，现在您可以始终确认当应用程序从后台转为前台时Launching信息是否已被更改。
    
### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Android.zip)

#### 添加功能   
* 为了改善强制映射时需要再次尝试IdP登录的不便，添加了一个新的强制映射API。
    * [Game > Gamebase > Android SDK使用指南 > 认证 > Mapping > Add Mapping Forcibly](./aos-authentication/#add-mapping-forcibly) 
* 为了解决调用Gamebase.addMapping()后出现AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302)错误的问题，添加了将帐户转换为已有的帐户后可进行登录的API。
    * [Game > Gamebase > Android SDK使用指南 > 认证 > Mapping > Change Login with ForcingMappingTicket](./aos-authentication/#change-login-with-forcingmappingticket)

#### 改善/修改功能
* 外部SDK升级 : Hangame Android SDK(1.4.2)
* 改善后，用户可直接修改或使用Gamebase提供的”查看维护详情Webview”html。 
    * [Game > Gamebase > Android SDK使用指南 > 初始化 > Launching Information > 1. Launching > 1.3 Maintenance > Change Default Maintenance HTML](./aos-initialization/#change-default-maintenance-html)
* 修改了即使设置DisplayLanguageCode也仍以终端机设置的语言显示维护Webview时间的错误。 
* 改善了因以断开的连接尝试通信而反复出现网络通信错误的问题。

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Android.zip)

#### 添加功能
* 已添加Google登录时声明Scope的功能。
    * [https://developers.google.com/identity/protocols/oauth2/scopes](https://developers.google.com/identity/protocols/oauth2/scopes)
    * 通过Scope添加**email**时，可在简介中获取Email信息。 
    * 如下所示，若在Gamebase Console的AdditionalInfo中进行设置，Gamebase登录谷歌时将自动设置Scope。

```
{"scope":["email","myscope1","myscope2",...]}
```

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.27.4)
* 只在DisplayLanguage指南上描述，实际上未包含在SDK的DisplayLanguage.Code类已被添加。
    * [Game > Gamebase > Android SDK使用指南 > ETC > Display Language > Gamebase支持的语言代码种类](./aos-etc/#gamebase)

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Android.zip)

#### 添加功能
* 添加Kakaogame认证
* ”结算Abusing自动解除”功能已被添加。 
    * [Game > Gamebase > Android SDK使用指南 > 认证 > GraceBan](./aos-authentication/#graceban)
    * 结算Abusing自动解除功能是当存在需通过”结算Abusing自动制裁”来禁止使用的用户时，禁止这些用户的使用之前先提供预约时间的功能。
    * 如果为”预约禁用”状态，在设定的时期内满足解除条件，则可正常玩游戏。
    * 若在所定的时期内未能满足条件，则会被禁用。
* 登录使用结算Abusing自动解除功能的游戏后，始终要确认AuthToken.getGraceBanInfo() API值，如果返还GraceBanInfo对象，而不返还null，要向相关用户通知禁用解除条件、时期等。
    * 当需要控制处于预约禁用状态的用户进入游戏时，要在游戏中进行处理。
* 等待登录响应时，显示等待图标。  

#### 改善/修改功能 
* 外部SDK升级 : PAYCO Android SDK(1.5.6)  
  
### 2.27.1 (2021.09.14) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Android.zip)   

#### 改善/修改功能
* 外部SDK升级 : PAYCO Android SDK(1.5.5), Hangame Android SDK(1.4.1), Weibo Android SDK(11.8.1)
* 通过反复重试，改善了模拟器和Rooting terminal不正常显示Webview的问题。
    * 对象包括通过Webview启动的图片通知，客户服务和共同条款。
* 通过改善Weibo IdP认证，提高了稳定性。
    * Gamebase对实际上是同步API，但启动为异步API而导致错误的API添加了意外处理、等待、重试等的辅助处理。  

#### 修改错误
* 已修改只用英语显示”未注册的游戏版本”错误弹窗的问题。 
* 已修改维护弹窗不显示汉语的错误。
* 已修改当进行[Credential Login](./aos-authentication/#login-with-credential)时，[Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp)调用失败的错误。 

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* 外部SDK升级 : TOAST Android SDK(0.27.1)
* 添加ONE Store V16商店

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Android.zip)

#### 改善/修改功能
* Display Language功能已被改善。
    * 以前为了添加语言集，必须直接修改gamebase-sdk-base-version.aar文件。
        * 但经过功能改善，将localizedstring.json文件放入res/raw文件夹里就可添加语言了。
    * 以前未能将Unity指南的Display Language语言集添加方法适用于Android。
        * 但经过功能改善，按照Unity指南添加localizedstring.json文件时，可将其方法适用于Android打包了。
        * [Game > Gamebase > Unity SDK使用指南 > ETC > Additional Features > Display Language > 添加新语言集](./unity-etc/#add-new-language-sets)
    * 在Display Language语言集中添加了汉语简体(zh-CN)、汉语繁体(zh-TW)及泰国语(th)。
    * 默认语言代码为**en**，但经过改善已可适用Gamebase控制台中设置的默认语言。
        * [Game > Gamebase > 控制台使用指南 > 应用程序 > App > 语言设置](./oper-app/#language-settings)
* 调用showTermsView API后，可创建PushConfiguration对象的基准被更改为；
    * 修改前
        * 仅当在条款项目中存在**接收Push**项目时，才返还有效的PushConfiguration，而不返还null。
        * 用户拒绝在白天和夜间接收广告性Push时，PushConfiguration.pushEnabled将会创建为false。
    * 修改后
        * 如果显示了条款UI，将返还有效的PushConfiguration，而不始终返还null。
        * showTermsView返还的PushConfiguration对象的pushEnabled值始终为true。
    * 未修改项目
        * 若因已同意条款，条款UI未被显示时，PushConfiguration将会返回为null。

#### 修改错误
* 已修改因终端机的语言代码和Push控制台中的语言代码不一致，而导致未能正常设置Push语言的问题。

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Android.zip)

#### More Features
* Add monthly payment limit feature
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

#### Feature Updates
* Change the dependency of Android Support Library to AndroidX
* Guarantee the PushConfiguration object in the terms and conditions with Push notification items
    * The PushConfiguration to be created as the result of calling Gamebase.Terms.showTermsView API was null if user did not agree to receive push notifications in the terms of UI. It has now changed so that the PushConfiguration object is always returned if there is a Push notification item in the terms and conditions.
    * When user rejects push notifications, the PushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * The PushConfiguration is null when there is no Push notification item in the terms and conditions.
* External SDK Update
    * TOAST Android SDK(0.26.0)
    * Kotlin(1.5.21)
    * Google Play Services Auth(19.0.0)
    * Facebook Android SDK(11.1.0)
    * Naver Android SDK(4.4.1)
    * Line Android SDK(5.6.2)
    * Weibo Android SDK(11.6.0)
* Fixed a crash that occurred when logging in to Weibo.

### 2.24.0 (2021.06.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Change the internal launch URL
* Fixed incorrect wording in SDK attachments

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed the issue of the title of the suspended view details web view not being displayed

### 2.22.0 (2021.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated the external SDK: TOAST Android SDK(0.25.0), Hangame Android SDK(1.4.0)

#### Bug Fixes
* The following error has been fixed: When a user logs out and logs in again with another user ID, a payment at Google Play Store is successful but the return value is sometimes "Failed."
* The following error has been fixed: When the name of an app package contains a capital letter, the "Sign In with Apple" log-in fails.

### 2.21.1 (2021.04.19) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed an issue where the system crashes when canceling Hangame login via PAYCO

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

#### More Features
* Japanese authentication for Hangame added.	 	

#### Feature Updates
* External SDK update: Facebook Android SDK (6.5.1), Line Android SDK (5.4.0)
	
#### Bug Fixes
* Fixed a crashing error caused when calling payment API on build with Proguard applied.

### 2.20.2 (2021.03.30) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated to Billing Client Version 3.0.3 where payment errors caused by Android 11 devices in Google Play Store are fixed

### 2.20.1 (2021.02.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed a logic that could cause the push-fcm module to crash during initialization

### 2.20.0 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Android.zip)

#### More Features
* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

### 2.19.1 (December 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added
	
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) Fixed an issue that caused OutOfMemoryException on retry in WebSocket
* [SDK] 2.19.1
	* (Android) Fixed a crash when logging in with a different IdP after trying to log in to Weibo IdP

### 2.18.2 (December 15, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Android.zip)

#### More Features
* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [SDK] 2.18.2
    * (Common) TOAST SDK update: Android(0.24.2), iOS(0.27.1), Unity(0.21.3)
	* (Android) External SDK update to resolve encryption logic security warnings: Payco Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
#### Bug Fixes
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### 2.18.1 (November 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.1/GamebaseSDK-Android.zip)

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: Android(0.24.1) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### 2.17.1 (October 13, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Android.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
	* (Common) Supports the download feature when a Customer Center attachment image is clicked
	* (Common) TOAST SDK update: Android(0.23.2), Unity(0.21.2)

#### Bug Fixes  
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
### 2.16.0 (September 22, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Android.zip)

#### More Features
* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
### 2.15.0 (August 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Android.zip)
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

#### Feature Updates
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)

### 2.13.0 (July 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 

#### Bug Fixes
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

### 2.12.0 (July 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Android.zip)

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order 
    * [SDK] 2.12.0: Added Show Image Notice API 
  
### 2.11.0 (June 23, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

### 2.10.0 (May 26, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

### 2.9.1 (May 12, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.9.1
	* (Andoird) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error 

### 2.9.0 (April 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Android.zip)

#### More Features
* Suspension of Membership Withdrawal 
	* [SDK] 2.9.0
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

#### Feature Updates
* [SDK] 2.9.0
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated Payco Login SDK: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (April 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	
#### Bug Fixes
* [SDK] 2.8.1 
	* (Android) Modified codes that may cause crashes after process restarts

### 2.8.0 (March 24, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 

#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 
	
### 2.7.2 (March 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.7.2 
    * Fix code that may cause a crash in the ToastLogger initialization part during Gamebase initialization
    * The server version has been updated to v1.2.1.

### 2.7.1 (February 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called

### 2.7.0 (January 21, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 

### 2.6.2 (December 24, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)

### 2.6.1 (December 10, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize() 
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address 
  * (Android) Fixed crash occurrence when IAP is not enabled  

### 2.6.0 (November 12, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Android.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features 
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (Android) Added the payment feature for Google subscription  
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase. 

### 2.5.0 (August 27, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
### 2.4.4 (July 23, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)

### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Android.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 
	
### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Android.zip)

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Chanage of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]

    * (Android) Naver SDK Version Updated (v4.2.5): Bug of Naver SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while login to Naver was underway)  
