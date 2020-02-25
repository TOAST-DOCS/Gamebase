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

* 可以将Gamebase显示的语言更改为设备上设置的语言以外的语言。
* Gamebase显示客户端中包含的信息或从服务器接收的信息。
* 如果设置DisplayLanguage，将以用户设置的语言代码（ISO-639）的语言显示信息。
* 可以添加所需的语言。 以下是可以添加的语言代码：

> [参考]
>
> Gamebase的客户信息仅包含英文(en)、韩文(ko)、日文(ja)。

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

> <font color="red">[注意]</font><br/>
>
> Gamebase支持的语言代码区分大小写。
> 将其设置为“EN”或“zh-cn”可能会出现问题。

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

### Server Push
* 可以处理从Gamebase服务器发送到客户端设备的Server Push Message。
* 在Gamebase客户端上添加ServerPushEvent Listener允许用户接收和处理信息，可以删除添加的ServerPushEvent Listener。


#### Server Push类型
目前，Gamebase支持的Server Push Type如下。

* ServerPushEventMessage.Type.APP_KICKOUT (= "appKickout")
    * 如果在TOAST Gamebase控制台的 **Operation > Kickout** 中注册kickout ServerPush消息，与Gamebase连接的所有客户端的将收到 **APP_KICKOUT** 消息。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### 注册 ServerPushEvent
可以在Gamebase Client中注册ServerPushEvent来处理Gamebase Console和Gamebase服务器发出的Push事件。

**API**

```java
+ (void)Gamebase.addServerPushEvent(ServerPushEvent serverPushEvent);
```

**示例**

```java
public class MyServerPushEventManager {
    private ServerPushEvent mServerPushEvent = new ServerPushEvent() {
        @Override
        public void onReceive(ServerPushEventMessage message) {
            if (message.type.equals(ServerPushEventMessage.Type.APP_KICKOUT)) {
                // Logout
                // Go to Main
            } else if (message.type.equals(ServerPushEventMessage.Type.TRANSFER_KICKOUT)) {
                // Logout
                // Go to Main
            } else {
                ...
            }
        }
    };

    private void addServerPushEvent() {
        Gamebase.addServerPushEvent(mServerPushEvent);
    }
    ...
}
```


#### 删除 ServerPushEvent
可以删除在Gamebase注册的 ServerPushEvent。

**API**

```java
+ (void)Gamebase.removeServerPusEvent(ServerPushEvent serverPushEvent);
+ (void)Gamebase.removeAllServerPusEvent();
```

**示例**

```java
public class MyServerPushEventManager {
    ...
    private void removeServerPushEvent() {
        Gamebase.removeServerPusEvent(mServerPushEvent);
    }

    private void removeAllServerPushEvent() {
        Gamebase.removeAllServerPushEvent();
    }
    ...
}
```





### Observer
* Gamebase Observer允许接收并处理Gamebase的各种状态变动事件。
* 状态变动事件：网络类型变动，Launching状态变动(由于维护等引起的状态变动)，Heartbeat信息变动(用户禁用而导致Heartbeat信息变动）等。


#### Observer类型
Gamebase目前支持的 Observer类型如下。

* Network 类型变动
    * 可以接收网络变动信息。例如，可以用 ObserverMessage.data.get("code") 值获取 Network Type。
    * Type: ObserverMessage.Type.NETWORK (= "network")
    * Code: 请参考NetworkManager中声明的常量。
        * NetworkManager.TYPE_NOT: -1
        * NetworkManager.TYPE_MOBILE: 0
        * NetworkManager.TYPE_WIFI: 1
        * NetworkManager.TYPE_ANY: 2
* Launching 状态变动
    * 当定期检查应用程序状态的Launching Status response有变动时发生触发。例如，有维护或推荐更新等产生的事件。
    * Type: ObserverMessage.Type.LAUNCHING (= "launching")
    * Code: 请参考在LaunchingStatus中声明的常量。
        * LaunchingStatus.IN_SERVICE: 200
        * LaunchingStatus.RECOMMEND_UPDATE: 201
        * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * LaunchingStatus.REQUIRE_UPDATE: 300
        * LaunchingStatus.BLOCKED_USER: 301
        * LaunchingStatus.TERMINATED_SERVICE: 302
        * LaunchingStatus.INSPECTING_SERVICE: 303
        * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat信息变动
	* 定期与Gamebase服务器保持连接的Heartbeat response有变动时发生触发。例如，由用户禁用引起的事件。
    * Type: ObserverMessage.Type.HEARTBEAT (= "heartbeat")
    * Code: 请参考GamebaseError中声明的常量。
        * GamebaseError.INVALID_MEMBER: 6
        * GamebaseError.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### 注册Observer
可以在Gamebase Client上注册Observer来处理各种状态变动事件。

**API**

```java
+ (void)Gamebase.addObserver(Observer observer);
```

**示例**

```java
public class MyObserverManager {
    private Observer mGamebaseObserver = new Observer() {
        @Override
        public void onUpdate(ObserverMessage message) {
            String typeOfMessage = message.type;
            Map<String, Object> dataMap = message.data;

            if (typeOfMessage.equalsIgnoreCase(ObserverMessage.Type.LAUNCHING)) {
                int code = Integer.parseInt(dataMap.get("code"));
                String messageString = (String) dataMap.get("message");
                Log.d(TAG, "Update launching status to " + code + ", " + messageString);

                // You can check the changed launching status in here.
                switch (code) {
                    case LaunchingStatus.IN_SERVICE:
                        ...
                        break;
                    case LaunchingStatus.RECOMMEND_UPDATE:
                        ...
                        break;
                    case ...
                        break;
                    ...
                }
            } else if (typeOfMessage.equalsIgnoreCase(ObserverMessage.Type.HEARTBEAT)) {
                int code = Integer.parseInt(dataMap.get("code"));
                Log.d(TAG, "Heartbeat changing : " + dataMap);

                switch (code) {
                    case GamebaseError.INVALID_MEMBER:
                        // You can check the invalid user session in here.
                        ...
                        break;
                    case GamebaseError.BANNED_MEMBER:
                        // You can check the banned user session in here.
                        ...
                        break;
                }
            } else if (typeOfMessage.equalsIgnoreCase(ObserverMessage.Type.NETWORK)) {
                int code = Integer.parseInt(dataMap.get("code"));
                Log.d(TAG, "Network changing : " + dataMap);

                // You can check the changed network status in here.
                if (code == NetworkManager.TYPE_NOT) {
                    ...
                } else {
                    ...
                }
            } else {
                ...
            }
        }
    };

    private void addObserver() {
        Gamebase.addObserver(mGamebaseObserver);
    }
    ...
}
```


#### 删除 Observer
可以删除在Gamebase中注册的Observer。

**API**

```java
+ (void)Gamebase.removeObserver(Observer observer);
+ (void)Gamebase.removeAllObserver();
```

**示例**

```java
public class MyObserverManager {
    ...
    private void removeObserver() {
        Gamebase.removeObserver(mGamebaseObserver);
    }

    private void removeAllObserver() {
        Gamebase.removeAllObserver();
    }
    ...
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

#### Open Contact WebView

可弹出Gamebase Console中输入的**客服中心URL**页面的功能。
使用**Gamebase Console > App > InApp URL > Service center**中输入的值。

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
