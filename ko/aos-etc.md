## Game > Gamebase > Android SDK 사용 가이드 > ETC

## Additional Features

Gamebase에서 지원하는 부가 기능을 설명합니다.

### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

```java
+ (String)Gamebase.getDeviceLanguageCode();
```

### Display Language

점검 팝업과 같이 Gamebase 가 표시하는 언어는 단말기에 설정된 언어로 표시됩니다.

그런데 게임에서 표시하는 언어를 단말기에 설정된 언어가 아닌, 별도의 옵션으로 언어를 변경할 수 있는 게임이 있습니다.
예를 들어, 단말기에 설정된 언어는 영어 이지만 게임 표시 언어를 일본어로 변경한 경우, Gamebase 에서 표시하는 언어도 일본어로 변경하고 싶지만 Gamebase 가 표시하는 언어는 단말기에 설정된 언어인 영어로 표시됩니다.

이와 같이 `단말기에 설정된 언어가 아닌, 다른 언어로 Gamebase 메세지를 표시하고 싶은` 어플리케이션을 위해 Gamebase 는 `Display Language` 라는 기능을 제공합니다.

Gamebase 는 Display Language 로 설정한 언어로 Gamebase 메세지를 표시합니다.
Display Language 에 입력하는 언어 코드는 반드시 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 지정된 코드만을 사용할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * Display Language 는 단말기 설정 언어와 무관하게 Gamebase 의 표시 언어를 변경하고 싶은 경우에만 사용하시기 바랍니다.
> * Display Language Code 는 ISO-639 형태의 값으로, 대소문자를 구분합니다.
> 'EN'이나 'zh-cn'과 같이 설정하면 문제가 발생할 수 있습니다.
> * 만일 Display Language Code 로 입력한 값이 아래의 표(**Gamebase에서 지원하는 언어코드의 종류**)에 존재하지 않는다면, Display Langauge Code 는 자동으로 기본값인 영어(en)로 지정됩니다.

> [참고]
>
> * Gamebase의 클라이언트 메시지는 영어(en), 한글(ko), 일본어(ja)만 포함하고 있으므로 아래의 표에 존재하는 언어 코드라 할지라도 영어(en), 한글(ko), 일본어(ja) 이외의 언어를 지정하면 기본값인 영어(en)로 자동 설정됩니다.
> * Gamebase의 클라이언트에 포함되어 있지 않은 언어셋은 직접 추가할 수 있습니다.
> **신규 언어셋 추가** 항목을 참조하시기 바랍니다.

#### Gamebase에서 지원하는 언어코드의 종류

| Code | Name |
| --- | --- |
| de | German |
| en |English  |
| es | Spanish |
| fi | Finish |
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

해당 언어코드는 `DisplayLanguage` 클래스에 정의되어 있습니다.

```cs
package com.toast.android.gamebase.base.ui;

public class DisplayLanguage {
    public static class Code {
        public static final String German             = "de";
        public static final String English            = "en";
        public static final String Spanish            = "es";
        public static final String Finish             = "fi";
        public static final String French             = "fr";
        public static final String Indonesian         = "id";
        public static final String Italian            = "it";
        public static final String Japanese           = "ja";
        public static final String Korean             = "ko";
        public static final String Portuguese         = "pt";
        public static final String Russian            = "ru";
        public static final String Thai               = "th";
        public static final String Vietnamese         = "vi";
        public static final String Malay              = "ms";
        public static final String Chinese_Simplified = "zh-CN";
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
                                .setDisplayLanguageCode("en")
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

Gamebase에서 제공하는 기본 언어(ko, en, ja) 외 다른 언어를 사용하려면 gamebase-sdk-base.aar > res > raw에 있는 localizedstring.json 파일에 값을 추가해야 합니다.

![localizedstring.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

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
}
```

다른 언어셋을 추가해야 할 경우에는 localizedstring.json 파일에 `"${언어 코드}":{"key":"value"}` 형태로 값을 추가하면 됩니다.

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
  "${언어코드}": {
      "common_ok_button": "...",
      ...
  }
}
```

위 JSON 형식에서 "${언어코드}":{ } 내부에 key가 누락될 경우에는 `단말기에 설정된 언어` 또는 `en`이 자동으로 입력됩니다.

#### Display Language 우선순위

초기화 및 SetDisplayLanguageCode API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedstring.json 파일에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 단말기에 설정된 언어코드가 localizedstring.json 파일에 정의되어 있는지 확인합니다.(이 값은 초기화 이후, 단말기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동으로 설정됩니다.

### Country Code

* Gamebase는 System의 Country Code를 다음과 같은 API로 제공하고 있습니다.
* 각 API 마다 특징이 있으니 쓰임새에 맞는 API를 선택하시기 바랍니다.

#### USIM Country Code

* USIM에 기록된 국가코드를 리턴합니다.
* USIM에 잘못된 국가코드가 기록되어 있다 하더라도 추가적인 체크 없이 그대로 리턴합니다.
* 값이 비어있는 경우 'ZZ'를 리턴합니다.

**API**

```java
+ (String)Gamebase.getCountryCodeOfUSIM()
```

#### Device Country Code

* OS 로부터 전달받은 단말기 국가코드를 추가적인 체크 없이 그대로 리턴합니다.
* 단말기 국가코드는 '언어' 설정에 따라 OS가 자동으로 결정합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어로 국가코드를 결정합니다.
* 값이 비어있는 경우 'ZZ'를 리턴합니다.

**API**

```java
+ (String)Gamebase.getCountryCodeOfDevice()
```

#### Intergrated Country Code

* USIM, 단말기 언어 설정의 순서로 국가 코드를 확인하여 리턴합니다.
* getCountryCode API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 리턴합니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Gamebase Event Handler

* Gamebase 는 각종 이벤트를 `GamebaseEventHandler` 라는 하나의 이벤트 시스템에서 모두 처리할 수 있습니다.
* GamebaseEventHandler 는 아래 API 를 통해 간단하게 Listener 를 추가/제거 할 수 있습니다.

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

    // 각 category 에 맞는 VO 로 변환할 수 있는 JSON String 데이터 입니다.
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

* Category 는 GamebaseEventCategory 클래스에 정의되어 있습니다.
* 이벤트는 크게 ServerPush, Observer, Purchase, Push 로 나눌 수 있고, 각 Category 에 따라, GamebaseEventMessage.data 를 아래 표와 같은 방법으로 VO 로 변환할 수 있습니다.

| Event 종류 | GamebaseEventCategory | VO 변환 방법 | 비고 |
| --------- | --------------------- | ----------- | --- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseEventObserverData.from(message.data) | \- |
| Purchase - 프로모션 결제 | GamebaseEventCategory.PURCHASE_UPDATED | PurchasableReceipt.from(message.data) | \- |
| Push - 메세지 수신 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | PushMessage.from(message.data) | **isForeground** 값을 통해 Foreground 에서 메세지를 수신했는지 여부를 확인할 수 있습니다. |
| Push - 메세지 클릭 | GamebaseEventCategory.PUSH_CLICK_MESSAGE | PushMessage.from(message.data) | **isForeground** 값이 없습니다. |
| Push - 액션 클릭 | GamebaseEventCategory.PUSH_CLICK_ACTION | PushAction.from(message.data) | RichMessage 버튼 클릭시 동작합니다. |

#### Server Push

* Gamebase 서버에서 클라이언트 단말기로 보내는 메세지 입니다.
* Gamebase 에서 지원하는 Server Push Type 은 다음과 같습니다.
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* TOAST Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 킥아웃 메시지를 받게 됩니다.
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guest 계정을 다른 단말기로 이전을 성공하게 되면 이전 단말기에서 킥아웃 메세지를 받게 됩니다.

**Example**

```java
void eventHandlerSample(Activity activity) {
    Gamebase.addEventHandler(new GamebaseEventHandler() {
        @Override
        public void onReceive(@NonNull GamebaseEventMessage message) {
            switch (message.category) {
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
    if (category.equals(GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT)) {
        // Kicked out from Gamebase server.(Maintenance, banned or etc..)
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

* Gamebase Gamebase의 각종 상태 변동 이벤트를 처리하는 시스템 입니다.
* Gamebase 에서 지원하는 Observer Type 은 다음과 같습니다.
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 점검이 걸리거나 풀린 경우, 새로운 버전이 배포되어 업데이트가 필요한 경우와 같이, Launching 상태가 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code : LaunchingStatus 값을 의미합니다.
            * LaunchingStatus.IN_SERVICE: 200
            * LaunchingStatus.RECOMMEND_UPDATE: 201
            * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
            * LaunchingStatus.REQUIRE_UPDATE: 300
            * LaunchingStatus.BLOCKED_USER: 301
            * LaunchingStatus.TERMINATED_SERVICE: 302
            * LaunchingStatus.INSPECTING_SERVICE: 303
            * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
            * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
    * GamebaseEventCategory.OBSERVER_HEARTBEAT
    	* 탈퇴 처리 되거나 이용 정지로 인하여 사용자 계정 상태가 변했을 때 동작합니다.
    	* GamebaseEventObserverData.code : GamebaseError 값을 의미합니다.
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 네트워크 변동 사항 정보를 받을 수 있습니다.
    	* 네트워크가 끊기거나 연결되었을 때, 혹은 Wifi 에서 셀룰러 네트워크로 변경되었을 때 동작합니다.
    	* GamebaseEventObserverData.code : NetworkManager 값을 의미합니다.
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2

**VO**

```java
class GamebaseEventObserverData {
	// 상태값을 나타내는 정보입니다.
    public int code;

    // 상태에 관련된 메세지 정보 입니다.
    @Nullable
    public String message;

    // 추가 정보용 예비 필드 입니다.
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

* Promotion 코드 입력을 통해 상품을 획득한 경우 발생하는 이벤트 입니다.
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

* Push 메세지가 도착했을때 발생하는 이벤트 입니다.
* **isForeground** 필드를 통해 포그라운드에서 메세지를 수신했는지, 백그라운드에서 메세지를 수신했는지 구분할 수 있습니다.
* extras 필드를 JSON 으로 변환하여, Push 발송시 전송했던 커스텀 정보를 얻을 수도 있습니다.

**VO**

```java
class PushMessage {
	// 메세지 고유의 id 입니다.
    @NonNull
    public String id;

    // Push 메세지 제목 입니다.
    @Nullable
    public String title;

    // Push 메세지 본문 내용 입니다.
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

* 수신한 Push 메세지를 클릭했을때 발생하는 이벤트 입니다.
* 'GamebaseEventCategory.PUSH_RECEIVED_MESSAGE' 와는 다르게 **isForeground** 필드가 존재하지 않습니다.

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

* Rich Message 기능을 통해 생성한 버튼을 클릭했을때 발생하는 이벤트 입니다.
* actionType 은 다음 항목이 제공됩니다.
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```java
class PushAction {
	// 버튼 액션 종류 입니다.
    @NonNull
    public String actionType;

	// PushMessage 데이터 입니다.
    @NonNull
    public PushMessage message;

	// Push 콘솔에서 입력한 사용자 텍스트 입니다.
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
| characterId | O | String | 케릭터 이름을 나타내는 필드입니다. |
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
> TOAST Contact 서비스와 연동해서 사용하면 보다 쉽고 편리하게 고객 문의에 대응할 수 있습니다.
> 자세한 TOAST Contact 서비스 이용법은 아래 가이드를 참고하시기 바랍니다.
> [TOAST Online Contact Guide](/Contact%20Center/ko/online-contact-overview/)
>

#### Open Contact WebView

Gamebase 콘솔에 입력한 **고객 센터 URL** 웹뷰를 나타낼 수 있는 기능입니다.
**Gamebase 콘솔 > App > InApp URL > Service center**에 입력한 값이 사용됩니다.

**API**

```java
Gamebase.Contact.openContact(Activity activity, GamebaseCallback callback);
```

**Example**

``` java
public void openContact(Activity activity) {
    Gamebase.Contact.openContact(activity, new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            if (exception != null && exception.code == WEBVIEW_INVALID_URL) { // 7001
                // TODO: Gamebase Console Service Center URL is invalid.
                //  Please check the url field in the TOAST Gamebase Console.
            } else if (exception != null) {
                // TODO: Error occur when opening the contact web view.
            } else {
                // A user close the contact web view.
            }
        }
    });
}
```
