## Game > Gamebase > iOS Developer's Guide > Initialization

## Initialization

### Import Header File
먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.<br/>
AppDelegate.h등 Gamebase기능을 초기화할 곳에서 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```


### Configuration Settings

Gamebase 초기화시 TCGBConfiguration 객체를 통해 Gamebase 설정을 변경할 수 있습니다.

| API | Mandatory(M) / Optional(O) | 기능 설명 |
| --- | --- | --- |
| configurationWithAppID:appVersion: | M | TCGBConfiguration의 appID와 appVersion을 설정합니다.<br/>업데이트, 점검에 해당하는지 여부는 게임 버전으로 판단합니다.<br/>게임 버전을 지정해 주세요. |
| enablePopup: | O | **[UI]**<br/>시스템 점검, 유저 밴 등 유저가 게임을 플레이 할 수 없는 상황에서 팝업 등을 통해 사유를 표시해야 할 필요가 있습니다.<br/>**YES**로 설정하는 경우 Gamebase가 해당 상황에서 정보 팝업을 자동으로 표시합니다.<br/>기본값은 **NO** 입니다.<br/>**NO** 상태에서는 Launching 결과를 통해 정보를 획득하여 자체 UI를 구현하여 게임을 플레이 할 수 없는 이유를 표시해주시기 바랍니다. |
| enableLaunchingStatusPopup: | O | **[UI]**<br/>Launching결과에 따라 로그인 할 수 없는 상태에서(주로 점검 상태가 해당됩니다.) Gamebase가 자동으로 팝업을 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:YES** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |
| enableBanPopup: | O | **[UI]**<br/>유저가 이용 제재를 당한 상태일때 Gamebase가 자동으로 제재 사유를 팝업으로 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |


### Debug Mode
* Gamebase는 warning 및 error 로그만을 표시합니다.
* 개발에 참고하기 위해 시스템 로그를 켜기 위해서는 **[TCGBGamebase setDebugMode:YES]**를 호출해 주시기 바랍니다.

> <font color="red">[WARNING]</font><br/>
>
> 게임을 **RELEASE** 할 때는 반드시 소스코드에서 setDebugMode: 호출을 제거하거나 파라메터를 NO로 바꿔 빌드하세요.


### Initialize
**application:didFinishLaunchingWithOptions:** 메소드에서, 다음과 같이 초기화를 진행합니다.


> <font color="red">[WARNING]</font><br/>
>
> Gamebase초기화를 위한 **initializeWithConfiguration:launchOptions:completion:** 메서드의 호출은  **application:didFinishLaunchingWithOptions:** 외에서도 호출이 가능합니다.
>

<br/>


> <font color="red">[WARNING]</font><br/>
>
> **initializeWithConfiguration:launchOptions:completion:** 메서드는  호출되지 않은 상태에서의 다른 Gamebase API 호출에 대해서는 정상작동을 보장하지 않습니다.

1. **TCGBConfiguration** 객체를 생성하여, 각 property를 설정합니다.
2. 설정된 **TCGBConfiguration**객체를 사용하여, **initializeWithConfiguration:launchOptions:completion:**을 호출합니다.
3. **completion** block으로 받은 **TCGBError** 객체를 확인하여 성공여부를 판단하며, 초기화가 실패하였을 경우에는 재시도를 할 수 있도록 합니다.



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



### Launching Status

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

### Launching Status Code

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

### OpenURL Event

**application:openURL:sourceApplication:annotation:** 메소드를 호출하여 Switching App을 사용한 인증 시, 각 IDP들의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.


> <font color="red">[WARNING]</font><br/>
>
> UIApplicationDelegate의 **application:openURL:options:**를 이미 Overriding 했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.
>

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[UIApplication sharedApplication] setApplicationIconBadgeNumber:0];
    return [TCGBGamebase application:application didFinishLaunchingWithOptions:launchOptions];
}
```

### DidBecomeActive Event

**applicationDidBecomeActive:** 메소드를 호출하여, App이 활성화 되었는지 여부를 각 IDP의 인증용 SDK에서 필요한 동작을 하도록 알려줍니다.

```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### DidEnterBackground Event

**applicationDidEnterBackground** 메소드를 호출하여, App이 Background로 전환되었는지 알려주어야 합니다.

```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### WillEnterForeground Event

**applicationWillEnterForeground** 메소드를 호출하여, App이 Foreground로 전환된다는 것을 알려주어야 합니다.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
}
```


### Error Handling

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase 초기화가 되어있지 않습니다. |
| TCGB\_ERROR\_NOT\_LOGGED\_IN | 2 | 로그인이 필요합니다. |
| TCGB\_ERROR\_INVALID\_PARAMETER | 3 | 잘못된 파라미터입니다. |
| TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4 | JSON 포맷 에러입니다. |
| TCGB\_ERROR\_USER\_PERMISSION | 5 | 권한이 없습니다. |
| TCGB\_ERROR\_NOT\_SUPPORTED | 10 | 지원하지 않는 기능입니다. |
| TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12 | iOS에서 지원하지 않는 기능입니다. |



* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)
