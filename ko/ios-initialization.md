## Upcoming Products > Gamebase > Developer's Guide (iOS) > Initialization

## Initialization

### 1. Import Header file into AppDelegate
먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.<br/>
AppDelegate.h 에서 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```



### 2. Initializing Method
**application:didFinishLaunchingWithOptions:** 메소드에서, 다음과 같이 초기화를 진행합니다.


> [WARNING]
> 
> **initializeWithConfiguration:launchOptions:completion:** 메서드는
> 초기화가 **application:didFinishLaunchingWithOptions:** 외에서도 호출이 가능합니다.
>

<br/>


> [WARNING]
>
> **initializeWithConfiguration:launchOptions:completion:** 메서드는 
> 호출되지 않은 상태에서의 다른 Gamebase API 호출에 대해서는 정상작동을 보장하지 않습니다.
>

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *projectID = @"T0aStC1d";
    NSString *gameAppVersion = @"1.2";

    TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
    [configuration enableLaunchingStatusPopup:YES];

    [TCGBGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            // Gamebase Initialization is Succeeded
        }
    }];
}
```

### 3. Launching Status
Gamebase initialize 호출 결과로 런칭 상태를 확인 할 수 있습니다.<br/>
런칭 상태는 Gamebase 초기화 이후에 호출되어야합니다.

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

#### Launching Status Code
| Status | Code | Description |
| --- | --- | --- |
| IN_SERVICE | 200 | 정상 서비스 중 |
| RECOMMEND_UPDATE | 201 | 업데이트 권장 |
| IN_SERVICE_BY_QA_WHITE_LIST | 202 | 점검 중이지만 QA 유저 서비스 가능 |
| REQUIRE_UPDATE | 300 | 업데이트 필수 |
| BLOCKED_USER | 301 | 접속 차단 유저 |
| TERMINATED_SERVICE | 302 | 서비스 종료 |
| INSPECTING_SERVICE | 303 | 서비스 점검 중 |
| INSPECTING_ALL_SERVICES | 304 | 전체 서비스 점검 중 |
| INTERNAL_SERVER_ERROR | 500 | 내부 서버 에러 |


## Lifecycle Event
iOS의 App Event를 관리하기 위하여 아래에 명기된 **UIApplicationDelegate** protocol을 구현해야합니다.

### 1. OpenURL Event
**application:openURL:sourceApplication:annotation:** 메소드를 호출하여 Switching App을 사용한 인증 시, 각 IDP들의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.


> [WARNING]
>
> UIApplicationDelegate의 **application:openURL:options:**를 이미 Overriding 했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.
>

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[UIApplication sharedApplication] setApplicationIconBadgeNumber:0];
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### 2. DidBecomeActive Event
**applicationDidBecomeActive:** 메소드를 호출하여, App이 활성화 되었는지 여부를 각 IDP의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### 3. DidEnterBackground Event
**applicationDidEnterBackground** 메소드를 호출하여, App이 Background로 전환되었는지 알려주어야 합니다.

```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### 4. WillEnterForeground Event
**applicationWillEnterForeground** 메소드를 호출하여, App이 Foreground로 전환된다는 것을 알려주어야 합니다.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```
