## Game > Gamebase > Android SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

### Device Language

* 단말기에 설정된 언어 코드를 반환합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 반환합니다.

**API**

```java
+ (String)Gamebase.getDeviceLanguageCode();
```

### Display Language

점검 팝업 창과 같이 Gamebase가 표시하는 언어는 단말기에 설정된 언어로 표시됩니다.

그런데 게임에서 표시하는 언어를 단말기에 설정된 언어가 아닌, 별도의 옵션으로 언어를 변경할 수 있는 게임이 있습니다.
예를 들어, 단말기에 설정된 언어는 영어 이지만 게임 표시 언어를 일본어로 변경한 경우, Gamebase에서 표시하는 언어도 일본어로 변경하고 싶지만 Gamebase가 표시하는 언어는 단말기에 설정된 언어인 영어로 표시됩니다.

이와 같이 `단말기에 설정된 언어가 아닌, 다른 언어로 Gamebase 메시지를 표시하고 싶은` 애플리케이션을 위해 Gamebase는 `Display Language` 라는 기능을 제공합니다.

Gamebase는 Display Language로 설정한 언어로 Gamebase 메시지를 표시합니다.
Display Language에 입력하는 언어 코드는 반드시 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 지정된 코드만을 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * Display Language는 단말기 설정 언어와 무관하게 Gamebase의 표시 언어를 변경하고 싶은 경우에만 사용하시기 바랍니다.
> * Display Language Code는 ISO-639 형태의 값으로, 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
> * 만일 Display Language Code로 입력한 값이 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 존재하지 않는다면, Display Langauge Code는 Gamebase 콘솔에서 설정한 기본 언어로 지정됩니다.
>     * 만일 Gamebase 콘솔에서 언어 설정을 하지 않았다면 영어(en)가 기본 언어로 설정됩니다.

> [참고]
>
> * Gamebase의 클라이언트에 포함되어 있지 않은 언어셋은 직접 추가할 수 있습니다.
> **신규 언어셋 추가** 항목을 참조하시기 바랍니다.

#### Gamebase에서 지원하는 언어코드의 종류

| Code | Name |
| --- | --- |
| de | German |
| en |English  |
| es | Spanish |
| fi | Finnish |
| fr | French |
| id | Indonesian |
| it | Italian |
| ja | Japanese |
| ko | Korean |
| pt | Portuguese |
| ru | Russian |
| th | Thai |
| vi | Vietnamese |
| ms | Malay |
| zh-CN | Chinese-Simplified |
| zh-TW | Chinese-Traditional |

해당 언어 코드는 `DisplayLanguage` 클래스에 정의되어 있습니다.

```cs
package com.toast.android.gamebase.base.ui;

public class DisplayLanguage {
    public static class Code {
        public static final String German              = "de";
        public static final String English             = "en";
        public static final String Spanish             = "es";
        public static final String Finnish             = "fi";
        public static final String French              = "fr";
        public static final String Indonesian          = "id";
        public static final String Italian             = "it";
        public static final String Japanese            = "ja";
        public static final String Korean              = "ko";
        public static final String Portuguese          = "pt";
        public static final String Russian             = "ru";
        public static final String Thai                = "th";
        public static final String Vietnamese          = "vi";
        public static final String Malay               = "ms";
        public static final String Chinese_Simplified  = "zh-CN";
        public static final String Chinese_Traditional = "zh-TW";
    }
    ...
}
```

#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

**API**

```java
+ (GamebaseConfiguration)GamebaseConfiguration.Builder.setDisplayLanguageCode(String displayLanguageCode);
```

**Example**

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        String appId = "T0aStC1d";
        String appVersion = "1.0.0";
        GamebaseConfiguration configuration = new GamebaseConfiguration.Builder(appId, appVersion)
                                .enableLaunchingStatusPopup(true)
                                .setDisplayLanguageCode(DisplayLanguage.Code.English)
                                .build();
        
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                ...
            }
        });
        ...
    }
    ...
}
```

#### Set Display Language

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

**API**

```java
+ (void)Gamebase.setDisplayLanguageCode(String displayLanguageCode);
```

**Example**

```java
public void setDisplayLanguageCodeToEnglishInRuntime() {
    Gamebase.setDisplayLanguageCode(DisplayLanguage.Code.English);
}
```

#### Get Display Language

현재 적용된 Display Language를 조회할 수 있습니다.

**API**

```java
+ (String)Gamebase.getDisplayLanguageCode();
```

**Example**

```java
public void getDisplayLanguageCodeInRuntime() {
    String currentDisplayLanguageCode = Gamebase.getDisplayLanguageCode();
}
```

#### 신규 언어셋 추가

Gamebase에서 제공하는 기본 언어(ko, en, ja, zh-CN, zh-TW, th) 외 다른 언어를 추가하려면 프로젝트의 res > raw 폴더에 localizedstring.json 파일을 추가하면 됩니다.

![localizedstring.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

localizedstring.json에 정의되어 있는 형식은 아래와 같습니다.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  },
  "zh-CN": {
    "common_ok_button": "确定",
    "common_cancel_button": "取消",
    ...
    "launching_service_closed_title": "关闭服务"
  },
  "zh-TW": {
    "common_ok_button": "好",
    "common_cancel_button": "取消",
    ...
    "launching_service_closed_title": "服務關閉"
  },
  "th": {
    "common_ok_button": "ยืนยัน",
    "common_cancel_button": "ยกเลิก",
    ...
    "launching_service_closed_title": "ปิดให้บริการ"
  },
  "de": {},
  "es": {},
  ...
  "ms": {}
}
```

다른 언어셋을 추가해야 할 경우에는 localizedstring.json 파일의 해당 언어 코드에 `"key":"value"` 형태로 값을 추가하면 됩니다.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  ...
  "vi": {
    "common_ok_button": "value",
    "common_cancel_button": "value",
    ...
    "launching_service_closed_title": "value"
  },
  ...
  "ms": {}
}
```

#### Display Language 우선순위

초기화 및 SetDisplayLanguageCode API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedstring.json 파일에 정의되어 있는지 확인합니다.
2. 1번이 실패했다면 Gamebase 초기화 시, 단말기에 설정된 언어코드가 localizedstring.json 파일에 정의되어 있는지 확인합니다.(이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. 2번이 실패했다면 Gamebase 콘솔에 설정된 기본 언어가 설정됩니다.
4. Gamebase 콘솔에 언어 설정이 없다면 `en`이 기본값으로 설정됩니다.

### Country Code

* Gamebase는 System의 Country Code를 다음과 같은 API로 제공하고 있습니다.
* 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가코드를 반환합니다.
* USIM에 잘못된 국가코드가 기록되어 있다 하더라도 추가적인 체크 없이 그대로 반환합니다.
* 값이 비어있는 경우 'ZZ'를 반환합니다.

**API**

```java
+ (String)Gamebase.getCountryCodeOfUSIM()
```

#### Device Country Code

* OS로부터 전달받은 단말기 국가코드를 추가적인 체크 없이 그대로 반환합니다.
* 단말기 국가코드는 '언어' 설정에 따라 OS가 자동으로 결정합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어로 국가코드를 결정합니다.
* 값이 비어있는 경우 'ZZ'를 반환합니다.

**API**

```java
+ (String)Gamebase.getCountryCodeOfDevice()
```

#### Intergrated Country Code

* USIM, 단말기 언어 설정의 순서로 국가 코드를 확인하여 반환합니다.
* getCountryCode API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 반환합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 반환합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ'를 반환합니다.

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Gamebase Event Handler

* Gamebase는 각종 이벤트를 **GamebaseEventHandler**라는 하나의 이벤트 시스템에서 모두 처리할 수 있습니다.
* GamebaseEventHandler는 아래 API를 통해 간단하게 Listener를 추가/제거할 수 있습니다.

**API**

```java
+ (void)Gamebase.addEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeAllEventHandler();
```

**VO**

```java
class GamebaseEventMessage {
	// Event 종류를 나타냅니다.
    // GamebaseEventCategory 클래스의 값이 할당됩니다.
    @NonNull
    final public String category;

    // 각 category 에 맞는 VO 로 변환할 수 있는 JSON String 데이터입니다.
    @Nullable
    final public String data;
}
```

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.LOGGED_OUT:
                    GamebaseEventLoggedOutData loggedOutData = GamebaseEventLoggedOutData.from(message.data);
                    if (loggedOutData != null) {
                        processLoggedOut(activity, message.category, loggedOutData);
                    }
                    break;
                case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED:
                case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
                case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
                    GamebaseEventServerPushData serverPushData = GamebaseEventServerPushData.from(message.data);
                    if (serverPushData != null) {
                        processServerPush(activity, message.category, serverPushData);
                    }
                    break;
                case GamebaseEventCategory.OBSERVER_LAUNCHING:
                case GamebaseEventCategory.OBSERVER_HEARTBEAT:
                case GamebaseEventCategory.OBSERVER_NETWORK:
                    GamebaseEventObserverData observerData = GamebaseEventObserverData.from(message.data);
                    if (observerData != null) {
                        processObserver(activity, message.category, observerData);
                    }
                    break;
                case GamebaseEventCategory.PURCHASE_UPDATED:
                    ...
                case GamebaseEventCategory.PUSH_RECEIVED_MESSAGE:
                    ...
                case GamebaseEventCategory.PUSH_CLICK_MESSAGE:
                    ...
                case GamebaseEventCategory.PUSH_CLICK_ACTION:
                    ...
                default:
                    ...
            }
        }
    });
}
```

* Category는 GamebaseEventCategory 클래스에 정의되어 있습니다.
* 이벤트는 크게 LoggedOut, ServerPush, Observer, Purchase, Push로 나뉘며, 각 Category에 따라, 아래 표와 같은 방법으로 GamebaseEventMessage.data를 VO로 변환할 수 있습니다.

| Event 종류 | GamebaseEventCategory | VO 변환 방법 | 비고 |
| --------- | --------------------- | ----------- | --- |
| LoggedOut | GamebaseEventCategory.LOGGED_OUT | GamebaseEventLoggedOutData.from(message.data) | \- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED<br>GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseEventObserverData.from(message.data) | \- |
| Purchase - 프로모션 결제 | GamebaseEventCategory.PURCHASE_UPDATED | PurchasableReceipt.from(message.data) | \- |
| Push - 메시지 수신 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | PushMessage.from(message.data) | **isForeground** 값을 통해 Foreground에서 메시지를 수신했는지 여부를 확인할 수 있습니다. |
| Push - 메시지 클릭 | GamebaseEventCategory.PUSH_CLICK_MESSAGE | PushMessage.from(message.data) | **isForeground** 값이 없습니다. |
| Push - 액션 클릭 | GamebaseEventCategory.PUSH_CLICK_ACTION | PushAction.from(message.data) | RichMessage 버튼 클릭 시 동작합니다. |

#### How to handle events when the application is not running

* 커스텀 Application 클래스에서 GamebaseEventHandler를 등록하면 애플리케이션이 실행되지 않았을 때에도 이벤트를 처리할 수 있습니다.

```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        Gamebase.addEventHandler(new GamebaseEventHandler() {
            @Override
            public void onReceive(@NonNull GamebaseEventMessage message) {
                // ...
            }
        });

        // ...
    }
}
```

#### Logged Out

* Gamebase Access Token이 만료되어 네트워크 세션을 복구하기 위해 로그인 함수 호출이 필요한 경우 발생하는 이벤트입니다.

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.LOGGED_OUT:
                    GamebaseEventLoggedOutData loggedOutData = GamebaseEventLoggedOutData.from(message.data);
                    if (loggedOutData != null) {
                        processLoggedOut(activity, message.category, loggedOutData);
                    }
                    break;
                default:
                    ...
            }
        }
    });
}

void processLoggedOut(String category, GamebaseEventLoggedOutData data) {
    if (category.equals(GamebaseEventCategory.LOGGED_OUT)) {
        // There was a problem with the access token.
        // Call login again.
        Gamebase.login(activity, Gamebase.getLastLoggedInProvider(), (authToken, exception) -> {});
    }
}
```

#### Server Push

* Gamebase 서버에서 클라이언트 단말기로 보내는 메시지입니다.
* Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED
    	* NHN Cloud Gamebase 콘솔의 **Operation > Kickout**에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신한 직후에 발생하는 이벤트입니다.
        * '오토 플레이'와 같이 게임이 동작 중인 경우, 게임을 일시 정지시키는 목적으로 활용할 수 있습니다.
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* NHN Cloud Gamebase 콘솔의 **Operation > Kickout**에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
        * 클라이언트 단말기에서 서버 메시지를 수신했을 때 팝업 창을 표시하는데, 유저가 이 팝업 창을 닫았을 때 발생하는 이벤트입니다.
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guest 계정을 다른 단말기로 이전을 성공하게 되면 이전 단말기에서 킥아웃 메시지를 받게 됩니다.

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED:
                case GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT:
                case GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT:
                    GamebaseEventServerPushData serverPushData = GamebaseEventServerPushData.from(message.data);
                    if (serverPushData != null) {
                        processServerPush(activity, message.category, serverPushData);
                    }
                    break;
                default:
                    ...
            }
        }
    });
}

void processServerPush(String category, GamebaseEventServerPushData data) {
    if (category.equals(GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED)) {
        // Currently, the kickout pop-up is displayed.
        // If your game is running, stop it.
    } else if (category.equals(GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT)) {
        // Kicked out from Gamebase server.(Maintenance, banned or etc..)
        // And the game user closes the kickout pop-up.
        // Return to title and initialize Gamebase again.
    } else if (category.equals(GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT)) {
        // If the user wants to move the guest account to another device,
        // if the account transfer is successful,
        // the login of the previous device is released,
        // so go back to the title and try to log in again.
    }
}
```

#### Observer

* Gamebase의 각종 상태 변동 이벤트를 처리하는 시스템입니다.
* Gamebase에서 지원하는 Observer Type은 다음과 같습니다.
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 점검이 걸리거나 풀린 경우, 새로운 버전이 배포되어 업데이트가 필요한 경우와 같이, Launching 상태가 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code: LaunchingStatus 값을 의미합니다.
            * LaunchingStatus.IN_SERVICE: 200
            * LaunchingStatus.RECOMMEND_UPDATE: 201
            * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
            * LaunchingStatus.IN_TEST: 203
            * LaunchingStatus.IN_REVIEW: 204
            * LaunchingStatus.IN_BETA: 205
            * LaunchingStatus.REQUIRE_UPDATE: 300
            * LaunchingStatus.BLOCKED_USER: 301
            * LaunchingStatus.TERMINATED_SERVICE: 302
            * LaunchingStatus.INSPECTING_SERVICE: 303
            * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
            * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
    * GamebaseEventCategory.OBSERVER_HEARTBEAT
    	* 탈퇴 처리 되거나 이용 정지로 인하여 사용자 계정 상태가 변했을 때 동작합니다.
    	* GamebaseEventObserverData.code: GamebaseError 값을 의미합니다.
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 네트워크 변동 사항 정보를 받을 수 있습니다.
    	* 네트워크가 끊기거나 연결되었을 때, 혹은 Wifi에서 셀룰러 네트워크로 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code: NetworkManager 값을 의미합니다.
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2

**VO**

```java
class GamebaseEventObserverData {
	// 상태값을 나타내는 정보입니다.
    public int code;

    // 상태에 관련된 메시지 정보입니다.
    @Nullable
    public String message;

    // 추가 정보용 예비 필드입니다.
    @Nullable
    public String extras;
}
```

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.OBSERVER_LAUNCHING:
                case GamebaseEventCategory.OBSERVER_HEARTBEAT:
                case GamebaseEventCategory.OBSERVER_NETWORK:
                    GamebaseEventObserverData observerData = GamebaseEventObserverData.from(message.data);
                    if (observerData != null) {
                        processObserver(activity, message.category, observerData);
                    }
                    break;
                default:
                    ...
            }
        }
    });
}

void processObserver(String category, GamebaseEventObserverData data) {
    if (category.equals(GamebaseEventCategory.OBSERVER_LAUNCHING)) {
        int launchingStatusCode = data.code;
        String launchingMessage = data.message;
        switch (launchingStatusCode) {
            case LaunchingStatus.IN_SERVICE:
                // Finished maintenance.
                break;
            case LaunchingStatus.INSPECTING_SERVICE:
            case LaunchingStatus.INSPECTING_ALL_SERVICES:
                // Under maintenance.
                break;
            ...
        }
    } else if (category.equals(GamebaseEventCategory.OBSERVER_HEARTBEAT)) {
        int errorCode = data.code;
        switch (errorCode) {
            case GamebaseError.INVALID_MEMBER:
                // You can check the invalid user session in here.
                // ex) After transferred account to another device.
                break;
            case GamebaseError.BANNED_MEMBER:
                // You can check the banned user session in here.
                break;
            case GamebaseError.AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO:
                // There was a problem with the access token.
                // Call login again.
                Gamebase.login(activity, Gamebase.getLastLoggedInProvider(), (authToken, exception) -> {});
                break;
        }
    } else if (category.equals(GamebaseEventCategory.OBSERVER_NETWORK)) {
        int networkTypeCode = data.code;
        // You can check the changed network status in here.
        if (networkTypeCode == NetworkManager.TYPE_NOT) {
            // Network disconnected.
        } else {
            // Network connected.
        }
    }
}
```

#### Purchase Updated

* Promotion 코드 입력을 통해 상품을 획득한 경우 발생하는 이벤트입니다.
* 결제 영수증 정보를 획득할 수 있습니다.

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.PURCHASE_UPDATED:
                    PurchasableReceipt receipt = PurchasableReceipt.from(message.data);
                    if (receipt != null) {
                        // If the user got item by 'Promotion Code',
                        // this event will be occurred.
                    }
                    break;
                default:
                    ...
            }
        }
    });
}
```

#### Push Received Message

* Push 메시지가 도착했을때 발생하는 이벤트입니다.
* **isForeground** 필드를 통해 포그라운드에서 메시지를 수신했는지, 백그라운드에서 메시지를 수신했는지 구분할 수 있습니다.
* extras 필드를 JSON으로 변환하여, Push 발송 시 전송했던 커스텀 정보를 얻을 수도 있습니다.

**VO**

```java
class PushMessage {
	// 메시지 고유의 id입니다.
    @NonNull
    public String id;

    // Push 메시지 제목입니다.
    @Nullable
    public String title;

    // Push 메시지 본문 내용입니다.
    @Nullable
    public String body;

    // JSONObject 로 변환하여 모든 정보를 확인할 수 있습니다.
    @NonNull
    public String extras;
}
```

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.PUSH_RECEIVED_MESSAGE:
                    PushMessage pushMessage = PushMessage.from(message.data);
                    if (pushMessage != null) {
                        // When you received push message.
                        try {
                            JSONObject json = new JSONObject(pushMessage.extras);
                            // There is 'isForeground' information.
                            boolean isForeground = json.getBoolean("isForeground");
                            Object customValue = json.get("YourCustomKey");
                        } catch (Exception e) {}
                    }
                    break;
                default:
                    ...
            }
        }
    });
}
```

#### Push Click Message

* 수신한 Push 메시지를 클릭했을 때 발생하는 이벤트입니다.
* 'GamebaseEventCategory.PUSH_RECEIVED_MESSAGE'와는 다르게 **isForeground** 필드가 존재하지 않습니다.

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.PUSH_CLICK_MESSAGE:
                    PushMessage clickedMessage = PushMessage.from(message.data);
                    if (clickedMessage != null) {
                        // When you clicked push message.
                    }
                    break;
                default:
                    ...
            }
        }
    });
}
```

#### Push Click Action

* Rich Message 기능을 통해 생성한 버튼을 클릭했을 때 발생하는 이벤트입니다.
* actionType은 다음 항목이 제공됩니다.
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```java
class PushAction {
	// 버튼 액션 종류입니다.
    @NonNull
    public String actionType;

	// PushMessage 데이터입니다.
    @NonNull
    public PushMessage message;

	// Push 콘솔에서 입력한 사용자 텍스트입니다.
    @Nullable
    public String userText;
}
```

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
                case GamebaseEventCategory.PUSH_CLICK_ACTION:
                    PushAction pushAction = PushAction.from(message.data);
                    if (pushAction != null) {
                        // When you clicked action button by 'Rich Message'.
                    }
                    break;
                default:
                    ...
            }
        }
    });
}
```

### Analytics

Game지표를 Gamebase Server로 전송할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> Gamebase Analytics에서 지원하는 모든 API는 로그인 후에 호출이 가능합니다.
>

> [TIP]
>
> Gamebase.Purchase.requestPurchase() API를 호출하여 결제를 진행한 경우, 결제가 완료되면 자동으로 서버로 지표가 전송됩니다.
>

#### Game User Data Settings

로그인 이후 유저 레벨 정보를 설정할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 게임 로그인 이후 setGameUserData API를 호출하지 않으면 다른 지표에서 Level 정보가 누락될 수 있습니다.
>

API 호출에 필요한 파라미터는 아래와 같습니다.

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 유저 레벨을 나타내는 필드입니다. |
| channelId | O | String | 채널을 나타내는 필드입니다. |
| characterId | O | String | 캐릭터 이름을 나타내는 필드입니다. |
| classId | O | String | 직업을 나타내는 필드입니다. |

**API**

```java
Gamebase.Analytics.setGameUserData(GameUserData gameUserData);
```

**Example**

``` java
public void onLoginSuccess() {
    int userLevel = 10;
    String channelId, characterId, classId;

    GameUserData gameUserData = new GameUserData(userLevel);
    gameUserData.channelId = channelId; // Optional
    gameUserData.characterId = characterId; // Optional
    gameUserData.classId = classId; // Optional

    Gamebase.Analytics.setGameUserData(gameUserData);
}
```

#### Level Up Trace

레벨업이 되었을 경우 유저 레벨 정보를 변경할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 유저 레벨을 나타내는 필드입니다. |
| levelUpTime | M | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |



**API**

```java
Gamebase.Analytics.traceLevelUp(LevelUpData levelUpData);
```

**Example**

``` java
public void onLevelUp(int userLevel, long levelUpTime) {
    LevelUpData levelUpData = new LevelUpData(userLevel, levelUpTime);

    Gamebase.Analytics.traceLevelUp(levelUpData);
}
```

### Contact

Gamebase에서는 고객 문의 대응을 위한 기능을 제공합니다.

> [TIP]
>
> NHN Cloud Contact 서비스와 연동해서 사용하면 보다 쉽고 편리하게 고객 문의에 대응할 수 있습니다.
> 자세한 NHN Cloud Contact 서비스 이용법은 아래 가이드를 참고하시기 바랍니다.
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ko/Contact%20Center/ko/online-contact-overview/) 

> <font color="red">[주의]</font><br/>
>
> * Gamebase Android SDK 2.53.0 이상 버전은 아래 가이드에 따라 AndroidManifest.xml에 권한 선언만 추가하면 고객 센터에 파일 첨부 시 Gamebase Android SDK가 자동으로 권한을 요청합니다.
>     * [Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)
> * Gamebase Android SDK 2.52.0 이하 버전은 플랫폼별 가이드를 참고하여 직접 권한 획득 처리를 구현해야 합니다.
>     * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>     * [Unity Guide : Requesting Permissions](https://docs.unity3d.com/2018.4/Documentation/Manual/android-RequestingPermissions.html)

#### Customer Service Type

**Gamebase 콘솔 > App > Customer service** 에서는 아래와 같이 3가지 유형의 고객 센터를 선택할 수 있습니다.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud Online Contact      | △              |

각 유형에 따라 Gamebase SDK의 고객 센터 API는 다음 URL을 사용합니다.

* 개발사 자체 고객 센터(Developer customer center)
    * **고객 센터 URL**에 입력한 URL.
* Gamebase 제공 고객 센터(Gamebase customer center)
    * 로그인 전 : 유저 정보가 **없는** 고객 센터 URL.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.
* NHN Cloud 조직 상품(Online Contact)
    * 로그인 전 : 유저 정보가 **없는** 고객 센터 URL.
    * 로그인 후 : 유저 정보가 포함된 고객 센터 URL.

#### Open Contact WebView

고객 센터 웹뷰를 표시합니다.
URL은 고객 센터 유형에 따라 결정됩니다.
ContactConfiguration으로 URL에 추가 정보를 전달할 수 있습니다.

**ContactConfiguration**

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | ContactConfiguration 객체는 newBuilder() 함수를 통해 생성할 수 있습니다. |
| build() | **M** | 설정을 마친 Builder를 Configuration 객체로 변환합니다. |
| setUserName(String userName) | O | 사용자 이름(닉네임)을 전달하고자 할 때 사용합니다.<br>NHN Cloud 조직 상품(Online Contact) 유형에서 사용하는 필드입니다.<br>**default**: null |
| setAdditionalURL(String additionalURL) | O | 개발사 자체 고객 센터 URL 뒤에 붙는 추가적인 URL입니다.<br>고객 센터 타입이 `CUSTOM` 인 경우에만 사용하시기 바랍니다.<br>**default**: null |
| setAdditionalParameters(Map&lt;String, String&gt; additionalParameters) | O | 고객 센터 URL 뒤에 붙는 추가적인 파라미터입니다.<br>**default**: null |
| setExtraData(Map&lt;String, Object&gt; extraData) | O | 개발사가 원하는 extra data를 고객 센터 오픈 시에 전달합니다.<br>**default**: EmptyMap |

**API**

```java
+ (void)Gamebase.Contact.openContact(@NonNull  final Activity activity,
                                     @Nullable final GamebaseCallback onCloseCallback);

+ (void)Gamebase.Contact.openContact(@NonNull  final Activity activity,
                                     @NonNull  final ContactConfiguration configuration,
                                     @Nullable final GamebaseCallback onCloseCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize가 호출되지 않았습니다. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 고객 센터 URL이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL**을 확인하세요. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 사용자 식별을 위한 임시 티켓 발급에 실패하였습니다. |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | 고객 센터 웹뷰가 이미 표시중입니다. |

**Example**

``` java
Gamebase.Contact.openContact(activity, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // A user close the contact web view.
        } else if (exception.code == UI_CONTACT_FAIL_INVALID_URL) { // 6911
            // TODO: Gamebase Console Service Center URL is invalid.
            // Please check the url field in the TOAST Gamebase Console.
        } else if (exception.code == UI_CONTACT_FAIL_ANDROID_DUPLICATED_VIEW) { // 6913
            // The customer center web view is already opened.
        } else {
            // An error occur when opening the contact web view.
        }
    }
});
```

#### Request Contact URL

고객 센터 웹뷰를 표시하는데 사용되는 URL을 반환합니다.

**API**

```java
+ (void)Gamebase.Contact.requestContactURL(@NonNull final GamebaseDataCallback<String> callback);

+ (void)Gamebase.Contact.requestContactURL(@NonNull final ContactConfiguration configuration,
                                           @NonNull final GamebaseDataCallback<String> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initialize가 호출되지 않았습니다. |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 고객 센터 URL이 존재하지 않습니다.<br>Gamebase 콘솔의 **고객 센터 URL**을 확인하세요. |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 사용자를 식별을 위한 임시 티켓 발급에 실패하였습니다. |

**Example**

``` java
ContactConfiguration configuration = ContactConfiguration.newBuilder()
        .setUserName(userName)
        .build();
Gamebase.Contact.requestContactURL(configuration, new GamebaseDataCallback<String>() {
    @Override
    public void onCallback(String contactUrl, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Open webview with 'contactUrl'
        } else if (exception.code == UI_CONTACT_FAIL_INVALID_URL) { // 6911
            // TODO: Gamebase Console Service Center URL is invalid.
            // Please check the url field in the TOAST Gamebase Console.
        } else {
            // An error occur when requesting the contact web view url.
        }
    }
});
```

#### File Attach Type Popup

고객 센터 유형이 'NHN Cloud 조직 상품'인 경우 '추가 파라미터' 항목의 Key에 **from**, Value에 **app**을 입력하면 파일 첨부 시 타입 선택 팝업이 표시됩니다.
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_002_2.53.0.png)
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_003_2.53.0.png)