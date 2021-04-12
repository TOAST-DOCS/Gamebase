## Game > Gamebase > iOS SDK 使用指南 > 初始化

在使用Gamebase iOS SDK之前，必须先执行初始化。

### Import Header File

首先，需要将Gamebase头文件导入您的App。<br/>
获取以下头文件以初始化Gamebase功能，例如AppDelegate.h。

```objectivec
#import <Gamebase/Gamebase.h>
```

### Initialization Flow

当游戏开始时设置Debug Mode， 初始化Gamebase之后，根据Launching Status Code判断是否进入游戏。具体的Flow如下。 

![initialization flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

初始化Gamebase时，可以使用TCGBConfiguration对象修改Gamebase设置。

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M                          | 设置TCGBConfiguration的App ID和版本。<br/>无论是更新还是维护都取决于游戏版本。<br/>请指定游戏版本。 |
| enablePopup:                       | O                          | **[UI]**<br/>因系统维护或设置禁用（ban）等，游戏用户无法玩游戏的状态下，有时需要通过弹出窗口显示原因。<br/>如果设置为** YES **，Gamebase将在该情况下自动弹出窗口公告信息。<br/>默认为**NO**。<br/>**NO**的情况下，请通过Launching结果获取信息，并使用自定义UI，显示用户无法玩游戏的原因。 |
| enableLaunchingStatusPopup:        | O                          | **[UI]**根据Launching结果，可以更改Gamebase是否在无法登录时，自动显示弹出窗口（维护状态为主）。<br/>仅适用于**enablePopup:YES** 状态下。<br/>默认值为 **YES**。 | 
| enableBanPopup:                    | O                          | **[UI]**<br/>当游戏用户被禁用时，Gamebase可以设定是否将制裁原因以弹出窗口的形式显示给用户。<br/>仅适用于**enablePopup:** 状态下。<br/>默认值为 **YES**。 |


### Debug Mode
Gamebase仅显示警告(warning)和错误日志。
要打开系统日志以进行开发，请调用 **[TCGBGamebase setDebugMode:YES]**。

> <font color="red">[注意]</font><br/>
>
> 当**发布**游戏时，请务必从源代码中删除setDebugMode调用，或者将参数更改为false之后再打包。

调试设置也可在控制台进行，优先考虑在控制台中设置的值。
控制台设置方法请参考如下指南。

* [控制台测试终端机设置](./oper-app/#test-device)
* [控制台客户设置](./oper-app/#client)



### Initialize
**application:didFinishLaunchingWithOptions:**方法中按以下方式进行初始化。


> <font color="red">[注意]</font><br/>
>
> 为了初始化Gamebase，调用的**initializeWithConfiguration:launchOptions:completion:** 方法，也可以在**application:didFinishLaunchingWithOptions:** 之外进行调用。


<br/>


> <font color="red">[注意]</font><br/>
>
> 如果不调用**initializeWithConfiguration：launchOptions：completion：**方法，调用了另一个Gamebase API，它可能无法正常工作。

1. 创建一个**TCGBConfiguration**对象并设置每个属性。
2. 使用 **TCGBConfiguration** 对象调用**initializeWithConfiguration:launchOptions:completion:**。
3. 通过确认**completion**块传递的**TCGBError**对象，判断是否成功，如初始化失败，可重试。



```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration enablePopup:YES];
    [configuration enableLaunchingStatusPopup:YES];
    [configuration enableBanPopup:YES];

    [TCGBGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```



### Launching Information

可以通过调用Gamebase #initialize来确认Launching状态。<br/>
应在Gamebase初始化后调用Launching状态。

```objectivec
- (void)myMethodAfterGamebaseInitialized {
    TCGBLaunchingStatus launchingStatus = [TCGBLaunching launchingStatus];

    // You can check whether if Gamebase was initialized or not using this launchingStatus
    if (launchingStatus == 0) {
        NSLog(@"Service is not initialized.");
    }

    // After Initialize Complete
    if (launchingStatus == INSPECTING_SERVICE) {
        NSLog(@"Service in Maintenance");
    } else if (launchingStatus == IN_SERVICE) {
        NSLog(@"Service in Service");
    } else {
        ...
    }
}

```

使用getLaunchingInformations API，允许在初始化后获取LaunchingInfo对象。

**API**

```objectivec
#import <Gamebase/Gamebase.h>

+ NSDictionary* launchingInfo = [TCGBLaunching laucnhingInformations];
```


#### 1. Launching

是Gamebase启动信息。

**1.1 Status**

是Gamebase iOS SDK初始化设置中输入的应用程序版本的游戏状态信息。

* code: 游戏状态代码（正在检查、必须升级、结束服务等）
* message: 游戏状态信息

状态代码请参考下表。

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 正常服务中                                 |
| RECOMMEND_UPDATE            | 201  | 推荐更新                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 维护期间该服务不可用，但如果登记为测试设备，则无论维护如何，都可以连接和测试该服务。|
| IN_TEST                     | 203  | 正在测试 |
| IN_REVIEW                   | 204  | 正在审查 |
| IN_BETA                     | 205  | Beta服务器环境 |
| REQUIRE_UPDATE              | 300  | 强制更新                                  |
| BLOCKED_USER                | 301  | 访问权限已被禁用的用户 |
| TERMINATED_SERVICE          | 302  | 终止服务                                   |
| INSPECTING_SERVICE          | 303  | 服务正在维护中                                 |
| INSPECTING_ALL_SERVICES     | 304  | 所有服务正在维护中                             |
| INTERNAL_SERVER_ERROR       | 500  | 内部服务器错误                                 |


[Console Guide](/Game/Gamebase/ko/oper-app/#app)

**1.2 App**

是在Gamebase控制台中注册的App信息。

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

[控制台指南](/Game/Gamebase/ko/oper-app/#client)

**1.3 Maintenance**

是Gamebase Console中创建的维护信息。

* url: 检查页面URL
* timezone: 标准时间段(timezone)
* beginDate: 开始时间
* endDate: 结束时间
* message: 检查原因

[控制台指南](/Game/Gamebase/ko/oper-operation/#maintenance)

**1.4 Notice**

是Gamebase Console中创建的公告信息。

* message: 信息
* title: 标题
* url: 检查URL

[控制台指南](/Game/Gamebase/ko/oper-operation/#notice)

#### 2. tcProduct

是与Gamebase相关的TOAST服务的appKey。

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

是TOAST中创建的IAP商店信息。

* id: App ID
* name: App Name
* storeCode: Store Code

[控制台指南](/Game/Gamebase/ko/oper-purchase/)

#### 4. tcLaunching

是TOAST Launching Console中用户输入的信息。

* 以JSON string格式传送用户输入的值。
* 有关TOAST Launching的具体设置方法，请参考如下指南。

[控制台指南](/Game/Gamebase/ko/oper-management/#config)

### Handling Unregistered Version

对未在Gamebase控制台中注册的GameClientVersion进行初始化时，出现**LAUNCHING_UNREGISTERED_CLIENT(2004)**错误。
如果是enablePopup(true), enableLaunchingStatusPopup(true)状态，则会弹出强制更新窗口，并可跳转到商店。
如果不使用Gamebase弹窗，您可通过TCGBError对象获取UpdateInfo，在游戏中直接创建UI, 使得用户直接跳转到商店。


**VO**

```objectivec
@interface TCGBUpdateInfo : NSObject

// 可以下载最新版本的商店安装URL。
@property (nonatomic, strong, nullable) NSString* installUrl;

// 按照终端机设置的语言向用户显示消息。
// 语言为”en”时，提示以下消息。 
// 'The version is not supported. Please get the latest update version.'
@property (nonatomic, strong, nullable) NSString* message;

@end
```


**API**

```objectivec
+ (nullable TCGBUpdateInfo *)updateInfoFromError:(nonnull TCGBError *)error;
```


**Example**

```objectivec
- (void)initializeGamebase {
    TCGBConfiguration* config = [TCGBConfiguration configurationWithAppID:@"YOUR_APP_ID" appVersion:@"YOUR_APP_VERSION" zoneType:@"YOUR_ZONE_TYPE"];
    [TCGBGamebase initializeWithConfiguration:config completion:^(id launchingData, TCGBError *result) {

        if (result == nil) {
            // Gamebase initialization succeeded.
        } else {
            // Gamebase initialization failed.
            TCGBUpdateInfo* updateInfo = [TCGBUpdateInfo updateInfoFromError:result];
            if (updateInfo) {
                // Unregistered game client version.
                // Open market url to update application.
                NSLog(@"UpdateInfo after initialize => \n%@", [updateInfo prettyJsonString]);
            }
        }
    }];
}
```

## Lifecycle Event

要在iOS上管理App事件，请实现**UIApplicationDelegate**协议。

> <font color="red">[注意]</font><br/>
>
> 如果要使用SceneDelegate(iOS 13以上)， 要实现**UISceneDelegate**协议。
>

### OpenURL Event
需要通过调用**application:openURL:sourceApplication:annotation:**方法，将App的外部URL Open尝试告知Gamebase，Gamebase将相应的值传递给各Idp的认证用SDK，告知需要的操作。

> <font color="red">[注意]</font><br/>
>
> 如果已经覆盖(overriding)了UIApplicationDelegate的 **application:openURL:options:**，则可能无法调用**application:openURL:sourceApplication:annotation:**。
>

> <font color="red">[注意]</font><br/>
>
> 如果使用WeiboAuthAdapter，必须要实现**application:openURL:sourceApplication:annotation:**。
>

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [TCGBGamebase application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

如果使用SceneDelegate(iOS 13以上)，需要调用**scene:openURLContexts:**方法。

```objectivec
// SceneDelegate.m
- (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts {
    [TCGBGamebase scene:scene openURLContexts:URLContexts];
}
```


### DidBecomeActive Event
需要通过调用**applicationDidBecomeActive:**方法，将App是否处于活动状态告知Gamebase，Gamebase将相应的值传递给各Idp的认证用SDK，告知需要的操作。



```objectivec
// AppDelegate.m
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

如果使用SceneDelegate(iOS 13以上)，需要调用**sceneDidBecomeActive:**方法。

```objectivec
// SceneDelegate.m
- (void)sceneDidBecomeActive:(UIScene *)scene {
    [TCGBGamebase sceneDidBecomeActive:scene];
}
```

### DidEnterBackground Event
需要通过调用**applicationDidEnterBackground** 方法，将App切换到后台告知Gamebase。


```objectivec
// AppDelegate.m
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

如果使用SceneDelegate(iOS 13以上)，需要调用**sceneDidEnterBackground:**方法。

```objectivec
// SceneDelegate.m
- (void)sceneDidEnterBackground:(UIScene *)scene {
    [TCGBGamebase sceneDidEnterBackground:scene];
}
```

### WillEnterForeground Event
需要通过调用**applicationWillEnterForeground** 方法，将App切换到前台(foreground)告知Gamebase。

```objectivec
// AppDelegate.m
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```

如果使用SceneDelegate(iOS 13以上)，需要调用**sceneWillEnterForeground:**方法。

```objectivec
// SceneDelegate.m
- (void)sceneWillEnterForeground:(UIScene *)scene {
    [TCGBGamebase sceneWillEnterForeground:scene];
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED      | 1          | Gamebase未初始化。 |
| TCGB\_ERROR\_NOT\_LOGGED\_IN       | 2          | 需要登录。           |
| TCGB\_ERROR\_INVALID\_PARAMETER    | 3          | 无效的参数。          |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON格式错误。       |
| TCGB\_ERROR\_USER\_PERMISSION      | 5          | 无权限。              |
| TCGB\_ERROR\_NOT\_SUPPORTED        | 10         | 不支持此功能。        |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS   | 12         | iOS不支持此功能。   |



* 所有错误代码，请参考以下文档。
   * [错误代码](./error-code/#client-sdk)
