## Game > Gamebase > iOS SDK 사용 가이드 > 초기화

Gamebase iOS SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 

### Import Header File

먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.<br/>
AppDelegate.h 등 Gamebase 기능을 초기화할 곳에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Initialization Flow

게임이 시작되면 Debug Mode 를 설정하고, Gamebase 를 초기화하여 Launching Status Code 에 따라 게임 진입여부를 결정하도록 아래 Flow 와 같이 구현하시면 됩니다.

![initialization flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/initialization_flow_2.19.0.png)

### Configuration Settings

Gamebase 초기화 시 TCGBConfiguration 객체로 Gamebase 설정을 변경할 수 있습니다.

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M                          | TCGBConfiguration의 앱 ID와 앱 버전을 설정합니다.<br/>업데이트, 점검에 해당하는지 여부는 게임 버전으로 판단합니다.<br/>게임 버전을 지정해 주세요. |
| enablePopup:                       | O                          | **[UI]**<br/>시스템 점검, 이용 제재(ban) 등 게임 유저가 게임을 플레이할 수 없는 상황에서 팝업 창 등으로 사유를 표시해야 할 때가 있습니다.<br/>**YES**로 설정하면 Gamebase가 해당 상황에서 정보 팝업 창을 자동으로 표시합니다.<br/>기본값은 **NO**입니다.<br/>**NO** 상태에서는 론칭 결과를 통해 정보를 획득한 후 자체 UI를 구현해 게임을 플레이할 수 없는 이유를 표시해 주시기 바랍니다. |
| enableLaunchingStatusPopup:        | O                          | **[UI]**<br/>론칭 결과에 따라 로그인할 수 없는 상태에서(주로 점검 상태) Gamebase가 자동으로 팝업 창을 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:YES** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |
| enableBanPopup:                    | O                          | **[UI]**<br/>게임 유저가 이용 제재를 당한 상태일 때 Gamebase가 자동으로 제재 사유를 팝업 창으로 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:YES** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |


### Debug Mode
Gamebase는 경고(warning)와 오류 로그만 표시합니다.
개발에 참고할 수 있는 시스템 로그를 켜려면 **[TCGBGamebase setDebugMode:YES]**를 호출하시기 바랍니다.

> <font color="red">[주의]</font><br/>
>
> 게임을 **릴리스**할 때는 반드시 소스 코드에서 setDebugMode: 호출을 제거하거나 파라미터를 NO로 바꿔 빌드하세요.

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)



### Initialize
**application:didFinishLaunchingWithOptions:** 메서드에서 다음과 같이 초기화를 진행합니다.


> <font color="red">[주의]</font><br/>
>
> Gamebase를 초기화하기 위한 **initializeWithConfiguration:launchOptions:completion:** 메서드의 호출은 **application:didFinishLaunchingWithOptions:** 외에서도 호출할 수 있습니다.
>

<br/>


> <font color="red">[주의]</font><br/>
>
> **initializeWithConfiguration:launchOptions:completion:** 메서드를 호출하지 않고 다른 Gamebase API를 호출하면 정상적으로 작동하지 않을 수 있습니다.

1. **TCGBConfiguration** 객체를 생성하여, 각 속성을 설정합니다.
2. 설정된 **TCGBConfiguration** 객체를 사용하여 **initializeWithConfiguration:launchOptions:completion:**을 호출합니다.
3. **completion** 블록으로 전달된 **TCGBError** 객체를 확인하여 성공 여부를 판단하며, 초기화가 실패했을 때에는 다시 시도할 수 있게 합니다.



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

Gamebase 초기화 호출 결과로 론칭 상태를 확인할 수 있습니다.<br/>
론칭 상태는 Gamebase 초기화 이후에 호출해야 합니다.

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

launchingInformations API를 이용하면 초기화 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> launchingInformations API 는 실시간으로 서버에서 정보를 가져오는 비동기 API가 아닙니다.
> 2분 주기로 업데이트 되는 캐시 정보를 리턴하므로, 실시간으로 현재의 점검 여부를 판단하는 용도로는 적합하지 않습니다.
> 이런 경우에는 Launching Status Code가 변경되었을때 이벤트가 동작하는 GamebaseEventHandler 를 활용하시기 바랍니다.
> [Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Observer](./ios-etc/#observer)

**API**

```objectivec
#import <Gamebase/Gamebase.h>

NSDictionary* launchingInfo = [TCGBLaunching launchingInformations];
```


#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase iOS SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

상태 코드는 아래 표를 참고하십시오.

##### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 정상 서비스 중                                 |
| RECOMMEND_UPDATE            | 201  | 업데이트 권장                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트할 수 있습니다. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| IN_BETA                     | 205  | 베타 서버 환경 |
| REQUIRE_UPDATE              | 300  | 업데이트 필수                                  |
| BLOCKED_USER                | 301  | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302  | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303  | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304  | 전체 서비스 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500  | 내부 서버 오류                                 |

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > App](./oper-app/#app)

**1.2 App**

Gamebase 콘솔에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: 서버 주소
* customerService
    * accessInfo : 고객센터 연락처
    * type : 고객센터 유형
    * url : 고객센터 URL
* relatedUrls
    * termsUrl: 이용 약관
    * personalInfoCollectionUrl: 개인 정보 동의
    * punishRuleUrl: 이용 정지 규정
* install: 설치 URL
* idP: 인증 정보

[Game > Gamebase > 콘솔 사용 가이드 > 앱 > Client](./oper-app/#client)

**1.3 Maintenance**

Gamebase 콘솔에 등록된 점검 정보입니다.

* url: 점검 페이지 URL
* timezone: 표준 시간대(timezone)
* beginDate: 시작 시간
* endDate: 종료 시간
* message: 점검 사유
* hideDate: 점검 시작, 종료 시간을 표시할 것인지 여부

[Game > Gamebase > 콘솔 사용 가이드 > 운영 > Maintenance](./oper-operation/#maintenance)

##### Change Default Maintenance HTML

`enablePopup`과 `enableLaunchingStatusPopup` 값이 모두 `true`인 경우, 게임이 점검 상태라면 자동으로 점검 팝업 창이 표시됩니다.
여기서 **자세히 보기** 버튼을 클릭하면 점검 정보가 자동으로 웹뷰로 표시됩니다.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/maintenance_webview_android_2.30.0.png)

이때 표시되는 html 파일을 수정하고 싶다면 다음 링크의 html 파일을 다운로드하여 원하는 대로 수정한 후 Xcode 프로젝트의 `Copy Bundle Resources`에 **gamebase-maintenance.html** 파일을 추가하면 됩니다.
[html 파일 다운로드 LINK](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gamebase-maintenance.html)

**1.4 Notice**

Gamebase 콘솔에 등록된 공지 정보입니다.

* message: 메시지
* title: 타이틀
* url: 점검 URL

[Game > Gamebase > 콘솔 사용 가이드 > 운영 > Notice](./oper-operation/#notice)

#### 2. tcProduct

Gamebase와 연계된 NHN Cloud 서비스의 앱키입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud 콘솔에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code

[Game > Gamebase > 콘솔 사용 가이드 > 결제](./oper-purchase/)

#### 4. tcLaunching

NHN Cloud Launching Console에서 사용자가 입력한 정보입니다.

* 사용자가 입력한 값을 JSON string으로 전달합니다.
* NHN Cloud Launching 상세 설정은 아래 가이드를 참고하시기 바랍니다.

[Game > Gamebase > 콘솔 사용 가이드 > 관리 > Config](./oper-management/#config)


### Handling Unregistered Version
      
Gamebase 콘솔에 등록되지 않은 GameClientVersion 을 초기화를 하면 **LAUNCHING_UNREGISTERED_CLIENT(2004)** 에러가 발생합니다.
enablePopup(true), enableLaunchingStatusPopup(true) 상태라면 강제 업데이트 팝업 창이 표시되고, 마켓으로 이동할 수 있습니다.
Gamebase 팝업 창을 사용하지 않을 경우에는 UpdateInfo를 TCGBError 객체로부터 얻어 사용자가 마켓으로 이동할 수 있도록 게임에서 직접 UI를 구현할 수 있습니다.

**VO**

```objectivec
@interface TCGBUpdateInfo : NSObject

// 최신 버전을 다운로드 할 수 있는 스토어 설치 URL.
@property (nonatomic, strong, nullable) NSString* installUrl;

// 사용자에게 노출할 수 있는 메시지로 사용자의 단말기 언어에 맞게 전달됩니다.
// 만일 언어가 'en'인 경우 메시지는 아래와 같습니다.
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

iOS의 앱 이벤트를 관리하려면 다음 **UIApplicationDelegate** 프로토콜을 구현합니다.

> <font color="red">[주의]</font><br/>
>
> SceneDelegate(iOS 13 이상)을 사용한다면, **UISceneDelegate** 프로토콜을 구현해야 합니다.
>

### DidFinishLaunching Event
**application:didFinishLaunchingWithOptions:** 메서드를 호출하여, Gamebase에 앱이 시작되었음을 알려줘야 합니다.

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### OpenURL Event
**application:openURL:sourceApplication:annotation:** 메서드를 호출하여, 애플리케이션의 외부 URL Open 시도를 Gamebase에 알려주어야 합니다. Gamebase에서는 각 Idp의 인증용 SDK에 해당 값을 전달하여, 필요한 동작을 하도록 알려줍니다.

> <font color="red">[주의]</font><br/>
>
> UIApplicationDelegate의 **application:openURL:options:**를 이미 재정의(overriding)했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.
>


> <font color="red">[주의]</font><br/>
>
> WeiboAuthAdapter를 사용할 경우, **application:openURL:sourceApplication:annotation:**를 필수로 구현해야 합니다.
>

```objectivec
// AppDelegate.m
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [TCGBGamebase application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

만약 SceneDelegate(iOS 13 이상)을 사용한다면, **scene:openURLContexts:** 메서드를 호출해야 합니다.

```objectivec
// SceneDelegate.m
- (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts {
    [TCGBGamebase scene:scene openURLContexts:URLContexts];
}
```


### DidBecomeActive Event
**applicationDidBecomeActive:** 메서드를 호출하여, 앱의 활성화 여부를 Gamebase에 알려주어야 합니다. Gamebase에서는 각 Idp의 인증용 SDK에 해당 값을 전달하여, 필요한 동작을 하도록 알려줍니다.



```objectivec
// AppDelegate.m
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

만약 SceneDelegate(iOS 13 이상)을 사용한다면, **sceneDidBecomeActive:** 메서드를 호출해야 합니다.

```objectivec
// SceneDelegate.m
- (void)sceneDidBecomeActive:(UIScene *)scene {
    [TCGBGamebase sceneDidBecomeActive:scene];
}
```

### DidEnterBackground Event
**applicationDidEnterBackground** 메서드를 호출하여, Gamebase에 앱이 백그라운드(background)로 전환된다는 것을 알려 주어야 합니다.


```objectivec
// AppDelegate.m
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

만약 SceneDelegate(iOS 13 이상)을 사용한다면, **sceneDidEnterBackground:** 메서드를 호출해야 합니다.

```objectivec
// SceneDelegate.m
- (void)sceneDidEnterBackground:(UIScene *)scene {
    [TCGBGamebase sceneDidEnterBackground:scene];
}
```

### WillEnterForeground Event
**applicationWillEnterForeground** 메서드를 호출하여, Gamebase에 앱이 포그라운드(foreground)로 전환된다는 것을 알려 주어야 합니다.

```objectivec
// AppDelegate.m
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```

만약 SceneDelegate(iOS 13 이상)을 사용한다면, **sceneWillEnterForeground:** 메서드를 호출해야 합니다.

```objectivec
// SceneDelegate.m
- (void)sceneWillEnterForeground:(UIScene *)scene {
    [TCGBGamebase sceneWillEnterForeground:scene];
}
```

### Error Handling

| Error                              | Error Code | Description            |
| ---------------------------------- | ---------- | ---------------------- |
| TCGB\_ERROR\_NOT\_INITIALIZED      | 1          | Gamebase 초기화돼 있지 않습니다. |
| TCGB\_ERROR\_NOT\_LOGGED\_IN       | 2          | 로그인이 필요합니다.            |
| TCGB\_ERROR\_INVALID\_PARAMETER    | 3          | 잘못된 파라미터입니다.           |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON 포맷 오류입니다.         |
| TCGB\_ERROR\_USER\_PERMISSION      | 5          | 권한이 없습니다.              |
| TCGB\_ERROR\_NOT\_SUPPORTED        | 10         | 지원하지 않는 기능입니다.         |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS   | 12         | iOS에서 지원하지 않는 기능입니다.   |



* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
