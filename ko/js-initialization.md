## Game > Gamebase > JavaScript SDK 사용 가이드 > 초기화

Gamebase JavaScript SDK를 사용하려면 먼저 초기화를 진행해야 합니다.

### Configuration Settings

Gamebase를 초기화할 때, GamebaseConfiguration 객체로 Gamebase 설정을 변경할 수 있습니다.

| KEY                                                 | Mandatory(M) / Optional(O) | Description                              |
| --------------------------------------------------- | -------------------------- | ---------------------------------------- |
| appId: string                                       | **M**                      | NHN Cloud  Project ID입니다. 필수적으로 입력해야합니다. |
| clientVersion: string                               | **M**                      | 서비스 중, 점검, 공지사항 등 Playable한 상태인지를 게임 버전을 통해 판단합니다. <br/> `NHN Cloud  Console > Gamebase > App > Client Version > WEB`의 버전을 입력해주세요. |
| enableDebugMode: boolean                            | O                          | DebugMode를 활성화 할 수 있습니다. Debug log는 개발자 콘솔에 출력됩니다. <br/> 기본값은 **false**입니다. |
| uiConfiguration.enablePopup: boolean                | O                          | **[UI]**<br/>시스템 점검, 이용 제재(ban) 등 게임 이용자가 게임을 플레이할 수 없는 상황에서 팝업 등으로 사유를 표시해야 할 때가 있습니다. <br/> **true**로 설정하면 Gamebase가 해당 상황에서 정보 팝업을 자동으로 표시합니다. <br/> 기본값은 **false**입니다.<br/>**false** 상태에서는 론칭 결과를 통해 정보를 획득한 후 자체 UI를 구현해 게임을 플레이할 수 없는 이유를 표시해 주시기 바랍니다. |
| uiConfiguration.enableLaunchingStatusPopup: boolean | O                          | **[UI]**<br/>론칭 결과에 따라 로그인할 수 없는 상태에서(주로 점검 상태) Gamebase가 자동으로 팝업을 표시할지 여부를 변경할 수 있습니다. <br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| uiConfiguration.enableBanPopup: boolean             | O                          | **[UI]**<br/>게임 이용자가 이용 제재를 당한 상태일 때 Gamebase가 자동으로 이용정지 사유를 팝업으로 표시할지 여부를 변경할 수 있습니다. <br/>**enablePopup(true)** 상태에서만 동작합니다.<br/>기본값은 **true**입니다. |
| displayLanguageCode: string                         | O                          | Gamebase UI 에서 사용하는 언어코드입니다. <br/> 기본값은 **브라우져의 language**(navigator.language)입니다. |
| displayLanguageTable: json                          | O                          | Gamebase UI 노출시 사용하는 Text Resource입니다. <br/> 해당 값은 내장되어있는 언어셋 테이블과 자동으로 merging이 되어 사용됩니다. <br/> `toast.Gamebase.getDisplayLanguageTable()` 을 통해서 내장되어있는 언어셋 테이블 및 포멧을 확인할 수 있습니다.|


#### GamebaseConfiguration Example
```javascript
var gamebaseConfiguration = {
    appId: 'T0asTC1oud',    // TOAST Console Project ID
    clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
    enableDebugMode: true,  // Note: Do not turn it on in production mode.
    uiConfiguration: {
        enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
        enableLaunchingStatusPopup: true,
        enableBanPopup: true,
    },
};
```


### Debug Mode
Gamebase 디버그를 위한 설정입니다.
* true: Gamebase의 모든 로그가 출력됩니다.
* false: warning, error 로그가 출력됩니다.
* 기본값: false

디버그 설정은 Console에서도 가능하며 Console에서 설정된 값을 우선시합니다.
Console 설정 방법은 아래 가이드를 참고하십시오.

* [Console 테스트 단말기 설정](./oper-app/#test-device)
* [Console Client 설정](./oper-app/#client)


개발에 참고할 수 있는 시스템 로그를 켜려면 **toast.Gamebase.setDebugMode(true)**를 호출하시기 바랍니다.
> <font color="red">[주의]</font><br/>
>
> 게임을 **릴리스**할 때는 반드시 소스 코드에서 setDebugMode 호출을 제거하거나 파라미터를 false로 바꿔야합니다.
>



### Initialize

**toast.Gamebase.initialize(gamebaseConfiguration, (launchingInfo, error) => { ... })**를 호출하여 Gamebase SDK를 초기화합니다.<br/>

```js
function gamebaseInitialize() {
    var gamebaseConfiguration = {
        appId: 'T0asTC1oud',    // TOAST Console Project ID
        clientVersion: '1.0.0', // TOAST Console Gamebase App Client Version
        enableDebugMode: true,  // Note: Do not turn it on in production mode.
        uiConfiguration: {
            enablePopup: true,  // Default is false. If you want to use the Gamebase UI, please turn it on.
            enableLaunchingStatusPopup: true,
            enableBanPopup: true,
        },
    };  


    toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
        if (error) {
            // 초기화에 실패하면 Gamebase SDK를 이용할 수 없습니다.
            // appId, clientVersion 및 TOAST Console의 설정이 정상적으로 입력되었는지 확인하세요.
            console.log('Gamebase initialization failed');
            console.log(error);
            return;
        }

        const statusCode = launchingInfo.launching.status.code;
        if (isPlayable(statusCode)) { // Status 값은 하단의 Launching Status Code 표를 참조하시길 바랍니다.
            // 게임 플레이 가능상태입니다.
            console.log('Playable!');
        } else {
            // 게임 플레이 불가능상태입니다. (점검, 서비스 종료 등)
            console.log('Not Playable!');
        }
    });

    var isPlayable = function(statusCode) {
        if (statusCode >= 200 && statusCode < 300)
            return true;

        return false;
    };
}
```

### Launching Information

toast.Gamebase.initialize 호출 결과로 론칭 상태를 확인할 수 있습니다.

```js
toast.Gamebase.initialize(gamebaseConfiguration, function (launchingInfo, error) {
    if (error) {
        console.log(error);
        return;
    }

	var statusCode = launchingInfo.launching.status.code;
});
```

`toast.Gamebase.Launching.getLaunchingInformations()` API를 이용하면 초기화 이후에도 LaunchingInfo 객체를 획득할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> **toast.Gamebase.Launching.getLaunchingInformations()** API 는 실시간으로 서버에서 정보를 가져오는 비동기 API 가 아닙니다.
> 2분 주기로 업데이트 되는 캐시 정보를 리턴하므로, 실시간으로 현재의 점검 여부를 판단하는 용도로는 적합하지 않습니다.
> 이런 경우에는 Launching Status Code 가 변경되었을때 이벤트가 동작하는 GamebaseEventHandler 를 활용하시기 바랍니다.
> [Game > Gamebase > Javascript SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Observer](./js-etc/#observer)

**API**

```java
+ (LaunchingInfo)Gamebase.Launching.getLaunchingInformations();
```
LaunchingInfo 객체에는 Gamebase Console에 설정한 값들과 게임 상태 등이 포함돼 있습니다.


#### 1. Launching

Gamebase 론칭 정보입니다.

**1.1 Status**

Gamebase JavaScript SDK 초기화 설정에 입력한 앱 버전의 게임 상태 정보입니다.

* code: 게임 상태 코드(점검 중, 업데이트 필수, 서비스 종료 등)
* message: 게임 상태 메시지

상태 코드는 아래 표를 참고하십시오.

#### Launching Status Code

| Status                      | Code | Description                              |
| --------------------------- | ---- | ---------------------------------------- |
| IN_SERVICE                  | 200  | 정상 서비스 중                                 |
| RECOMMEND_UPDATE            | 201  | 업데이트 권장                                  |
| IN_SERVICE_BY_QA_WHITE_LIST | 202  | 점검 중에는 서비스를 이용할 수 없지만 QA 단말기로 등록된 경우에는 점검과 상관없이 서비스에 접속해 테스트할 수 있습니다. |
| IN_TEST                     | 203  | 테스트 중 |
| IN_REVIEW                   | 204  | 심사 중 |
| REQUIRE_UPDATE              | 300  | 업데이트 필수                                  |
| BLOCKED_USER                | 301  | 접속 차단으로 등록된 단말기(디바이스 키)로 서비스에 접속한 경우입니다. |
| TERMINATED_SERVICE          | 302  | 서비스 종료                                   |
| INSPECTING_SERVICE          | 303  | 서비스 점검 중                                 |
| INSPECTING_ALL_SERVICES     | 304  | 전체 서비스 점검 중                              |
| INTERNAL_SERVER_ERROR       | 500  | 내부 서버 오류                                 |

[Console Guide](./oper-app/#app)

**1.2 App**

Gamebase Console에 등록된 앱 정보입니다.

* accessInfo
    * serverAddress: 서버 주소
    * csInfo: 고객 센터 정보
* relatedUrls
    * termsUrl: 이용약관
    * personalInfoCollectionUrl: 개인 정보 동의
    * punishRuleUrl: 이용 정지 규정
    * csUrl : 고객 센터
* install: 설치 URL
* idP: 인증 정보

[Console Guide](./oper-app/#client)

**1.3 Maintenance**

Gamebase Console에 등록된 점검 정보입니다.

* url: 점검 페이지 URL
* timezone: 표준 시간대(timezone)
* beginDate: 시작 시간
* endDate: 종료 시간
* message: 점검 사유

[Console Guide](./oper-operation/#maintenance)

**1.4 Notice**

Gamebase Console에 등록된 공지 정보입니다.

* message: 메시지
* title: 타이틀
* url: 점검 URL

[Console Guide](./oper-operation/#notice)

#### 2. tcProduct

Gamebase와 연계된 NHN Cloud  서비스의 appKey입니다.

* gamebase
* tcLaunching
* iap
* push

#### 3. tcIap

NHN Cloud  Console에 등록된 IAP 스토어 정보입니다.

* id: App ID
* name: App Name
* storeCode: Store Code
 
[Console Guide](./oper-purchase/)

#### 4. tcLaunching

NHN Cloud  Console에 등록된 Launching 정보입니다

* 사용자가 Console에 입력한 값을 JSON string으로 전달합니다.
 
[Console Guide](./oper-management/#config)


### Error Handling

| Error                        | Error Code | Description                |
| ---------------------------- | ---------- | -------------------------- |
| NOT_INITIALIZED              | 1          | Gamebase 초기화돼 있지 않습니다. |
| NOT_LOGGED_IN                | 2          | 로그인이 필요합니다.            |
| INVALID_PARAMETER            | 3          | 잘못된 파라미터입니다.           |
| INVALID_JSON_FORMAT          | 4          | JSON 포맷 오류입니다.          |
| USER_PERMISSION              | 5          | 권한이 없습니다.               |
| NOT_SUPPORTED                | 10         | 지원하지 않는 기능입니다.        |


* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
