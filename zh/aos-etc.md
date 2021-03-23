## Game > Gamebase > Android SDK使用指南 > ETC

## Additional Features

以下描述Gamebase支持的附加功能。

### Device Language

* 返回终端机设置的语言代码。
* 注册多种语言时，仅返回优先权最高的语言。

**API**

```java
+ (String)Gamebase.getDeviceLanguageCode();
```

### Display Language

Gamebase显示终端机设置的语言。

但有些游戏允许通过额外选项更改终端机设置的语言。
若终端机设置的默认语言是英语，则需将游戏的显示语言转换为日语时，即使要将Gamebase的显示语言也转换为日语，Gamebase仍显示终端机设置的默认语言（en）。

因此Gamebase向需以终端机设置语言之外的其他语言显示Gamebase消息的应用程序，提供”Display Language“功能。

Gamebase显示消息时，以设置为Display Language的语言显示。
在Display Language输入语言代码时，只能使用以下列表中（**Gamebase支持的语言代码种类**）指定的代码。

> <font color="red">[注意]</font><br/>
>
> * 无论终端机设置的语言如何，需要更改Gamebase显示的语言时使用Display Language Gamebase功能。
> * Display Language Code是区分大小写的ISO-639形态的值。
> 若按”EN"或"zh-cn"进行设置，可能出现问题。
> * 若输入的Display Language Code值不在以下列表时（**Gamebase支持的语言代码种类**）, 则将Display Langauge Code自动设置为默认语言(en)。

> [参考]
>
> * 因Gamebase客户端消息中仅包含英语（en）、韩语（ko）、日语（ja），即使是下列表指定的语言代码，指定英语（en）、韩语（ko）、日语（ja）之外的语言时，也将设置为默认语言(en)。
> * 可以直接添加未注册在Gamebase客户端的语言集合。
> 请参考**添加新语言集合**项目。

#### Gamebase支持的语言代码种类。

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

相应的语言代码在 `DisplayLanguage` 类中定义。

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

#### 在Gamebase初始化时设置显示语言。

在Gamebase初始化时可以设置显示语言。

**API**

```java
+ (GamebaseConfiguration)GamebaseConfiguration.Builder.setDisplayLanguageCode(String displayLanguageCode);
```

**示例**

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

#### 设置显示语言

Gamebase初始化时可更改输入的 Display Language。

**API**

```java
+ (void)Gamebase.setDisplayLanguageCode(String displayLanguageCode);
```

**示例**

```java
public void setDisplayLanguageCodeToEnglishInRuntime() {
    Gamebase.setDisplayLanguageCode(DisplayLanguage.Code.English);
}
```

#### 查询显示语言

可以查询当前使用的显示语言。

**API**

```java
+ (String)Gamebase.getDisplayLanguageCode();
```

**示例**

```java
public void getDisplayLanguageCodeInRuntime() {
    String currentDisplayLanguageCode = Gamebase.getDisplayLanguageCode();
}
```

#### 添加新语言集

如果要使用Gamebase提供的默认语言(ko, en, ja)外的其他语言， 需要在gamebase-sdk-base.aar > res > raw的 localizedstring.json 文件中添加值。

![localizedstring.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

localizedstring.json中定义的格式如下。

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

如果需要添加另一种语言集，可在localizedstring.json文件中添加 `"${语言代码}"：{"key"："value"}` 形式的值。

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
  "${语言代码}": {
      "common_ok_button": "...",
      ...
  }
}
```

如果在上述JSON文件的格式 "${语言代码}":{ } 中缺少 key，则会自动输入`在设备上设置的语言`或`en`。

#### 显示语言优先级

通过初始化或SetDisplayLanguageCode API设置Display Language时，最终应用的Display Language可以与输入的值不同。

1. 确认输入的languageCode是否在localizedstring.json文件中定义。
2. 初始化Gamebase时，确认是否在localizedstring.json文件中定义了设备上设置的语言代码（即使在初始化后更改了设备上设置的语言，此值也将保留）。
3. 自动设置Display Language的默认值为`en`。

### Country Code

* Gamebase以如下API提供系统的国家代码(country code)。
* 各API具有不同特征，因此请选择与用途相符的API。

#### USIM Country Code

* 返回USIM中记录的国家代码。
* 即使USIM中记录的是错误的国家代码也将不进行确认就直接返回，。
* 若值为空，则返回’ZZ’。

**API**

```java
+ (String)Gamebase.getCountryCodeOfUSIM()
```

#### Device Country Code

* 从OS接收的终端机国家代码直接返回，不另行确认。
* 终端机国家代码根据’语言’设置，由OS自动决定。
* 注册多种语言时，以优先权最高的语言决定国家代码。
* 若值为空，则返回’ZZ’。

**API**

```java
+ (String)Gamebase.getCountryCodeOfDevice()
```

#### Intergrated Country Code

* 按照USIM、终端机语言设置的顺序确认国家代码并返回。
* getCountryCode API按照如下顺序运行。
	1.确认USIM中记录的国家代码，若存在值，则直接返回，不另行确认。
	2.若USIM国家代码为空值，确认终端机国家代码，若存在值，则直接返回，不另行确认。
	3.若USIM、终端机国家代码均为空值，则返回’ZZ’。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Gamebase Event Handler

### Gamebase Event Handler

* Gamebase通过**GamebaseEventHandler**事件系统处理所有的事件。  
* GamebaseEventHandler通过以下API简单添加或删除Listener。 


**API**

```java
+ (void)Gamebase.addEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeAllEventHandler();
```

**VO**

```java
class GamebaseEventMessage {
	// 显示Event种类。
    // 分配GamebaseEventCategory类中定义的值。 
    @NonNull
    final public String category;

    // 是可转换为符合category的VO的JSON String数据。
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

* Category在GamebaseEventCategory类中定义。
* 事件大体分为ServerPush, Observer, Purchase, Push, 并按照各Category, 将GamebaseEventMessage.data以如下列表的方式，转换为VO。 

| Event种类 | GamebaseEventCategory | VO转换方法 | 备注 |
| --------- | --------------------- | ----------- | --- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseEventObserverData.from(message.data) | \- |
| Purchase - Promotion支付 | GamebaseEventCategory.PURCHASE_UPDATED | PurchasableReceipt.from(message.data) | \- |
| Push - 接收消息 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | PushMessage.from(message.data) | 通过**isForeground**值，可以确认是否是在Foreground状态接收的消息。 |
| Push - 点击消息 | GamebaseEventCategory.PUSH_CLICK_MESSAGE | PushMessage.from(message.data) | 不存在**isForeground**值。 |
| Push - 动态点击 | GamebaseEventCategory.PUSH_CLICK_ACTION | PushAction.from(message.data) | 如果点击RichMessage按键则运行。 |

#### Server Push

* 为从Gamebase服务器向客户端终端机传送的消息。 
* Gamebase支持的Server Push Type如下。 
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* 如果在TOAST Gamebase控制台**Operation > Kickout**中注册Kickout ServerPush消息，则从与Gamebase连接的所有客户端接收Kickout消息。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* 将Guest账号成功转移到其他终端机时，从转移之前的终端机接收Kickout消息。

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

* 为处理Gamebase各状态的变动事件的系统。 
* Gamebase支持的Observer Type如下。 
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* 当维护开始、结束时或发布新版本必须进行更新等Launching状态出现变动时运行。
    	* GamebaseEventObserverData.code : 为LaunchingStatus值。 
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
    	* 当因已被退出或禁用，用户账号状态出现变化时运行。
    	* GamebaseEventObserverData.code : 为GamebaseError值。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* 可以接收网络变动信息。
    	* 当网络断开或被连接时、从Wifi转为Cellular网络时运行。
        * GamebaseEventObserverData.code : 为NetworkManager值。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2

**VO**

```java 
class GamebaseEventObserverData {
	// 为显示状态值的信息。
    public int code;

    // 为描述状态的message信息。 
    @Nullable
    public String message;

    // 为用于附加信息的保留字段。
    @Nullable
    public String extras;
}
```

**示例**

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

* 为输入Promotion代码获取商品时出现的事件。
* 可以获取结算票据信息。

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

* 为接收Push消息时出现的事件。
* 通过**isForeground**字段可区分是在Foreground状态还是在Backgroud状态接收的消息。 
* 通过将extras字段转换为JSON，可获取发送Push时传送的自定义信息。

**VO**

```java
class PushMessage {
	// 为消息的固有id。
    @NonNull
    public String id;

    // 为Push消息的标题。 
    @Nullable
    public String title;

    // 为Push消息的身体。
    @Nullable
    public String body;

    // 通过转换为JSONObject，可确认所有的信息。
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

* 为点击”已接收的Push消息”时出现的事件。
* 与”GamebaseEventCategory.PUSH_RECEIVED_MESSAGE”不同，不存在**isForeground** field。

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

* 为通过Rich Message功能，点击生成按钮时出现的事件。
* actionType中存在以下值。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```java
class PushAction {
	// 为ButtonAction种类。 
    @NonNull
    public String actionType;

	// 为PushMessage数据。
    @NonNull
    public PushMessage message;

	// 为在Push控制台中输入的用户文本。
    @Nullable
    public String userText;
}
```

**示例**

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

可将游戏指标传送至Gamebase服务器。

> <font color="red">[注意]</font><br/>
>
> Gamebase Analytics支持的所有API登录后可调用。
>

> [TIP]
>
> 调用Gamebase.Purchase.requestPurchase() API并完成付款后，自动传送指标。
>

#### Game User Data Settings

登录游戏后可设置用户级别信息。

> <font color="red">[注意]</font><br/>
>
> 若登录游戏后不调用setGameUserData API，则其他指标中可能遗漏级别信息。
>

调用API所需的参数如下。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示用户级别的字段。 |
| channelId | O | String | 是显示通道的字段。 |
| characterId | O | String | 是显示角色名的字段。 |
| classId | O | String | 是显示职业的字段。 |

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

升级后可更改用户级别信息。

调用API所需的参数如下。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | 是显示用户级别的字段。 |
| levelUpTime | M | long | 按Epoch Time输入。</br>按Millisecond单位输入。|



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

Gamebase提供用于应对客户咨询的功能。

> [TIP]
>
> 若与TOAST Contact商品关联使用，则可更加轻松方便地应对顾客咨询。
> 详细的TOAST Contact商品使用，请参考如下指南。
> [TOAST Online Contact Guide](/Contact%20Center/en/online-contact-overview/)
>

#### Customer Service Type

从**Gamebase控制台 > App > InApp URL > Service center**当中选择如下3个客服中心类型中的一个。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| TOAST Online Contact      | O              |

Gamebase SDK客服中心API根据类型使用如下URL。

* 开发公司自建客服中心(Developer customer center)
    * 在**客服中心URL**输入的URL。
* Gamebase提供的客服中心(Gamebase customer center)
    * 登录前 : **不包含**用户信息的客服中心URL。
    * 登录后 : 包含用户信息的客服中心URL。
* TOAST组织服务(Online Contact)
    * 登录前 : 出现NOT_LOGGED_IN(2)错误。
    * 登录后 : 包含用户信息的客服中心URL。

#### Open Contact WebView

显示客服中心WebView。
根据客服中心类型选择URL。 
可通过ContactConfiguration向URL传送附加信息。 

**ContactConfiguration**

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | 可通过newBuilder()函数生成ContactConfiguration对象。 |
| build() | **M** | 将设置完的Builder转换为Configuration对象。 |
| setUserName(String userName) | O | 需传送用户名(nickname)时使用。<br>是在TOAST组织服务(Online Contact)类型使用的字段。<br>**default** : null |
| setAdditionalURL(String additionalURL) | O | 是添加在开发公司客服中心URL后面的附加URL。<br>只能在客服中心类型为”CUSTOM”时使用。<br>**default** : null |
| setExtraData(Map<String, Object> extraData) | O | 客服中心服务开始后传送开发公司需要的extra data。<br>**default** : EmptyMap |

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
| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize |
| NOT\_LOGGED\_IN(2)                                  | 客服中心类型为“TOAST OC“时，登录前已调用ContactConfiguration函数。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客服中心URL不存在<br>  |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | 识别用户的临时ticket发放失败 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | 已显示客服中心WebView |

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

> <font color="red">[注意]</font><br/>
>
> 询问客服中心时可能需要添附文件。
> 为此，需要在运行时从用户获得有关相机拍照或Storage存储的权限。 
> [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>
> Unity用户请通过参考如下指南执行上述程序 。
> [Unity Guide : Requesting Permissions](https://docs.unity3d.com/2018.4/Documentation/Manual/android-RequestingPermissions.html)

#### Request Contact URL

返还显示客服中心WebView时使用的URL。

**API**

```java
+ (void)Gamebase.Contact.requestContactURL(@NonNull final GamebaseDataCallback<String> callback);

+ (void)Gamebase.Contact.requestContactURL(@NonNull final ContactConfiguration configuration,
                                           @NonNull final GamebaseDataCallback<String> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | 未调用Gamebase.initialize。 |
| NOT\_LOGGED\_IN(2)                                  | 客服中心的类型为'TOAST OC'时，登录前已调用ContactConfiguration函数。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | 客服中心URL不存在。<br>请确认Gamebase控制台的**客服中心URL**。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) |  识别用户的临时ticket发放失败。 |

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