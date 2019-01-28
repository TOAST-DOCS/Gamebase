## Game > Gamebase > iOS SDK 사용 가이드 > 초기화

Gamebase iOS SDK를 사용하려면 먼저 초기화를 진행해야 합니다. 

### Import Header File

먼저 Gamebase 헤더 파일을 앱으로 가져와야 합니다.<br/>
AppDelegate.h 등 Gamebase 기능을 초기화할 곳에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```


### Configuration Settings

Gamebase 초기화 시 TCGBConfiguration 객체로 Gamebase 설정을 변경할 수 있습니다.

| API                                | Mandatory(M) / Optional(O) | Description                              |
| ---------------------------------- | -------------------------- | ---------------------------------------- |
| configurationWithAppID:appVersion: | M                          | TCGBConfiguration의 앱 ID와 앱 버전을 설정합니다.<br/>업데이트, 점검에 해당하는지 여부는 게임 버전으로 판단합니다.<br/>게임 버전을 지정해 주세요. |
| enablePopup:                       | O                          | **[UI]**<br/>시스템 점검, 이용 제재(ban) 등 게임 이용자가 게임을 플레이할 수 없는 상황에서 팝업 등으로 사유를 표시해야 할 때가 있습니다.<br/>**YES**로 설정하면 Gamebase가 해당 상황에서 정보 팝업을 자동으로 표시합니다.<br/>기본값은 **NO**입니다.<br/>**NO** 상태에서는 론칭 결과를 통해 정보를 획득한 후 자체 UI를 구현해 게임을 플레이할 수 없는 이유를 표시해 주시기 바랍니다. |
| enableLaunchingStatusPopup:        | O                          | **[UI]**<br/>론칭 결과에 따라 로그인할 수 없는 상태에서(주로 점검 상태) Gamebase가 자동으로 팝업을 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:YES** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |
| enableBanPopup:                    | O                          | **[UI]**<br/>게임 이용자가 이용 제재를 당한 상태일 때 Gamebase가 자동으로 제재 사유를 팝업으로 표시할지 여부를 변경할 수 있습니다.<br/>**enablePopup:** 상태에서만 동작합니다.<br/>기본값은 **YES**입니다. |


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



### Launching Status

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

**API**

```objectivec
#import <Gamebase/Gamebase.h>

+ NSDictionary* launchingInfo = [TCGBLaunching laucnhingInformations];
```

### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 정상 서비스 중                                 |
| RECOMMEND_UPDATE            | 201  | 업데이트 권장                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트할 수 있습니다. |
| REQUIRE_UPDATE              | 300  | 업데이트 필수                                  |
| BLOCKED_USER                | 301  | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302  | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303  | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304  | 전체 서비스 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500  | 내부 서버 오류                                 |


## Lifecycle Event

iOS의 앱 이벤트를 관리하려면 다음 **UIApplicationDelegate** 프로토콜을 구현합니다.

### OpenURL Event
**application:openURL:sourceApplication:annotation:** 메서드를 호출하여, 어플리케이션의 외부 URL Open 시도를 Gamebase에 알려주어야 합니다. Gamebase에서는 각 Idp의 인증용 SDK에 해당 값을 전달하여, 필요한 동작을 하도록 알려줍니다.

> <font color="red">[주의]</font><br/>
>
> UIApplicationDelegate의 **application:openURL:options:**를 이미 재정의(overriding)했다면, **application:openURL:sourceApplication:annotation:**이 호출되지 않을 수 있습니다.
>

```objectivec
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [TCGBGamebase application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
}
```

### DidBecomeActive Event
**applicationDidBecomeActive:** 메서드를 호출하여, 앱의 활성화 여부를 Gamebase에 알려주어야 합니다. Gamebase에서는 각 Idp의 인증용 SDK에 해당 값을 전달하여, 필요한 동작을 하도록 알려줍니다.



```objectivec
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [TCGBGamebase applicationDidBecomeActive:application];
}
```

### DidEnterBackground Event
**applicationDidEnterBackground** 메서드를 호출하여, Gamebase에 앱이 백그라운드(background)로 전환된다는 것을 알려 주어야 합니다.


```objectivec
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [TCGBGamebase applicationDidEnterBackground:application];
}
```

### WillEnterForeground Event
**applicationWillEnterForeground** 메서드를 호출하여, Gamebase에 앱이 포그라운드(foreground)로 전환된다는 것을 알려 주어야 합니다.

```objectivec
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [TCGBGamebase applicationWillEnterForeground:application];
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
