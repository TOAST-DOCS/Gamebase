## Game > Gamebase > Unreal SDK使用指南 > 初始化

若需使用Gamebase Unreal SDK，必须先进行初始化。此外，必须在TOAST控制台中注册应用程序ID和版本信息。

### Include Header File

为了使用Gamebase API，返还以下头文件。 

```cpp
#include "Gamebase.h"
```

### FGamebaseConfiguration 

初始化时所需的设置如下。

| Setting value              | Supported Platform | Mandatory(M) / Optional(O) |
| -------------------------- | ------------------ | -------------------------- |
| appID | ALL | M |
| appVersion | ALL | M |
| storeCode | ALL | M |  
| displayLanguageCode | ALL | O |
| enablePopup | ALL | O |
| enableLaunchingStatusPopup | ALL | O |
| enableBanPopup | ALL | O |
| enableKickoutPopup | ALL | O |

#### 1. App ID
 
是在Gamebase Console中注册的项目ID。

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

#### 2. appVersion

是在Gamebase Console中注册的客户端版本。

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

#### 3. storeCode

是初始化TOAST统合In-App结算服务IAP(In-App Purchase)时所需的商店信息。

| Store       | Code | Description  |
| ----------- | ---- | ------------ |
| App Store | AS | only iOS |
| Google Play | GG | only Android |
| One Store | ONESTORE | only Android |

#### 4. displayLanguageCode

可以将Gamebase提供的UI和SystemDialog显示的语言更改为终端机设置的语言以外的其他语言。 

[Display Language](./unreal-etc/#display-language)

#### 5. enablePopup

因系统检查、禁用(ban)等原因游戏用户无法玩游戏时，需要通过弹窗显示其理由。 
通过此设置可以选择是否使用Gamebase提供的弹窗。
 
* true : 弹窗的显示与否取决于enableLaunchingStatusPopup、enableBanPopup设置状态。 
* false : 不显示Gamebase提供的所有弹窗。
* 默认值 : false  
 
#### 6. enableLaunchingStatusPopup

LaunchingStatus为不能玩游戏的状态时，通过此设置可以选择是否使用Gamebase提供的弹窗。 
关于LaunchingStatus，请参考以下Launching项目的State和Code内容。

* 默认值 : true

#### 7. enableBanPopup

游戏用户已被禁止登陆游戏时，通过此设置可以选择是否使用Gamebase提供的弹窗。

* 默认值 : true

#### 8. enableKickoutPopup

从Gamebase Server接收Kickout事件时，通过此设置可以选择是否使用Gamebase提供的弹窗。 

* 默认值 : true


### Debug Mode

* Gamebase仅显示警告(warning)和错误日志。 
* 如需打开开发时参考的系统日志，请调用**IGamebase::Get().SetDebugMode(true)**。

> <font color="red">[注意]</font><br/>
>
> 当**发布**游戏时，请务必从源代码中删除SetDebugMode调用，或将参数更改为false之后再打包。

调试设置也可在控制台进行，优先考虑在控制台中设置的值。
关于Console设置方法，请参考以下指南。

* [Console测试终端机的设置](./oper-app/#test-device)
* [Console Client的设置](./oper-app/#client)

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void SetDebugMode(bool isDebugMode);
```

**Example**

```cpp
void Sample::SetDebugMode(bool isDebugMode)
{
    IGamebase::Get().SetDebugMode(isDebugMode);
}
```


### Initialize

对SDK进行初始化。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
void Initialize(const FGamebaseConfiguration& configuration, const FGamebaseLaunchingInfoDelegate& onCallback);
```

**Example**

```cpp
void Sample::Initialize(const FString& appID, const FString& appVersion)
{
    FGamebaseConfiguration configuration{ "AppID", "AppVersion", "real", GamebaseDisplayLanguageCode.Korean, true, true, true, true, GamebaseStoreCode.Google, true };

    IGamebase::Get().Initialize(configuration, FGamebaseLaunchingInfoDelegate::CreateLambda([=](const FGamebaseLaunchingInfo* launchingInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize succeeded."));
        
            // Following notices are registered in the Gamebase Console
            auto notice = launchingInfo->launching.notice;
            if (notice != null)
            {
                if (string.IsNullOrEmpty(notice.message) == false)
                {
                    UE_LOG(GamebaseTestResults, Display, TEXT("title: %s"), notice.title);
                    UE_LOG(GamebaseTestResults, Display, TEXT("message: %s"), notice.message);
                    UE_LOG(GamebaseTestResults, Display, TEXT("url: %s"), notice.url);
                }
            }
            
            // Status information of game app version set in the Gamebase Unreal SDK initialization.
            auto status = launchingInfo->launching.status;
    
            // Game status code (e.g. Under maintenance, Update is required, Service has been terminated)
            // refer to GamebaseLaunchingStatus
            if (status.code == GamebaseLaunchingStatus::IN_SERVICE)
            {
                // Service is now normally provided.
            }
            else
            {
                switch (status.code)
                {
                    case GamebaseLaunchingStatus::RECOMMEND_UPDATE:
                    {
                        // Update is recommended.
                        break;
                    }
                    // ... 
                    case GamebaseLaunchingStatus::INTERNAL_SERVER_ERROR:
                    {
                        // Error in internal server.
                        break;
                    }
                }
            }
        }
        else
        {
                // Check the error code and handle the error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("Initialize failed."));
        }
    }));
}
```

### Launching Information

通过调用Initialize API初始化Gamebase Unreal SDK时，可将获取LaunchingInfo对象结果值。
此LaunchingInfo对象包含Gamebase Console中注册的多个值和游戏状态等信息。

#### 1. Launching

是Gamebase推出信息。

**1.1 Status**

是在Gamebase Unreal SDK初始化设置中输入的应用程序版本的游戏状态信息。 

* code : 游戏状态代码(维护中、必须更新、终止服务等)
* message : 游戏状态消息

有关状态代码，请参考如下。

| Status                      | Status Code | Description                                    |
| --------------------------- | ----------- | ---------------------------------------- |
| IN_SERVICE | 200 | 在服务中 |
| RECOMMEND_UPDATE | 201 | 推荐更新 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202         | 进行维护时不能使用服务，但注册为QA终端机时，即使在进行维护，也可连接服务进行测试。|
| IN_TEST                     | 203  | 正在测试 |
| IN_REVIEW                   | 204  | 正在审核 |
| REQUIRE_UPDATE | 300 | 必须更新 |
| BLOCKED_USER                | 301         | 使用注册为禁止访问的终端机(device key)访问了服务。|  
| TERMINATED_SERVICE          | 302         | 终止服务                                   |
| INSPECTING_SERVICE          | 303         | 服务正在维护中                                 |
| INSPECTING_ALL_SERVICES     | 304         | 正在进行系统的全面检查                             |
| INTERNAL_SERVER_ERROR       | 500         | 内部服务器错误                                 |

[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

是注册在Gamebase Console中的应用程序信息。

* accessInfo
    * serverAddress : 服务器地址 
* customerService
    * accessInfo : 客户服务电话号码
    * type : 客户服务类型
    * url : 客户服务URL
* relatedUrls
    * termsUrl : 利用条款
    * personalInfoCollectionUrl : 同意隐私协议
    * punishRuleUrl : 禁用规定
* install : 安装URL
* idP : 认证信息

[Console Guide](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

是在Gamebase Console中注册的维护信息。 

* url : 维护页面URL
* timezone : 标准时区(timezone)
* beginDate : 开始时间 
* endDate : 终止时间
* message : 维护原因

[Console Guide](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

是在Gamebase Console中注册的公告信息。

* message : 消息
* title : 标题
* url : 维护URL

[Console Guide](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

是与Gamebase相关的TOAST服务的Appkey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

是在TOAST Console中注册的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

是用户在TOAST Launching控制台中输入的信息。

* 以JSON string格式传送用户输入的值。
* 关于TOAST Launching的详细设置，请参考以下指南。 
 
[Console Guide](/Game/Gamebase/ko/oper-management/#config)

### Get Launching Information

调用GetLaunchingInformations API时，Initialize之后也可以获取LaunchingInfo对象。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#B60205; font-size: 10pt">■</span> UNREAL_EDITOR

```cpp
const FGamebaseLaunchingInfoPtr GetLaunchingInformations() const;
```

**Example**

```cpp
void Sample::GetLaunchingInformations()
{
    auto launchingInformation = IGamebase::Get().GetLaunching().GetLaunchingInformations();
    if (launchingInformation.IsValid() == false)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("Not found launching info."));
        return;
    }
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| NOT\_INITIALIZED      | 1          | 未初始化Gamebase。|
| NOT\_LOGGED\_IN       | 2          | 需要登录。            |
| INVALID\_PARAMETER    | 3          | 是错误的参数。           |
| INVALID\_JSON\_FORMAT | 4          | 是JSON格式错误。         |
| USER\_PERMISSION      | 5          | 没有权限。              |
| NOT\_SUPPORTED        | 10         | 不支持此功能。         |

* 关于所有错误代码，请参考以下文档。 
    * [错误代码](./error-code/#client-sdk)