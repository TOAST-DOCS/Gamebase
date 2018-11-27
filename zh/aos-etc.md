## Game > Gamebase > Android SDK使用指南 > ETC

## 附加功能

以下描述Gamebase支持的附加功能。

### 显示语言

* 可以将Gamebase显示的语言更改为设备上设置的语言以外的语言。
* Gamebase显示客户端中包含的信息或从服务器接收的信息。
* 如果设置DisplayLanguage，将以用户设置的语言代码（ISO-639）的语言显示信息。
* 可以添加所需的语言。 以下是可以添加的语言代码：

> [参考]
>
>Gamebase中的客户端信息仅包含英语（en）和韩语（ko）。

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

> `[注意]`
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





### Server Push
* 可以处理从Gamebase服务器发送到客户端设备的Server Push Message。
* 在Gamebase客户端上添加ServerPushEvent Listener允许用户接收和处理信息，可以删除添加的ServerPushEvent Listener。


#### Server Push类型
目前，Gamebase支持的Server Push Type如下。

* ServerPushEventMessage.Type.APP_KICKOUT (= "appKickout")
	* 如果在TOAST Gamebase控制台的`Operation> Kickout`中注册kickout ServerPush消息，与Gamebase连接的所有客户端的将收到`APP_KICKOUT`消息。
* ServerPushEventMessage.Type.TRANSFER_KICKOUT (= "transferKickout")
	* 如果通过TransferKey成功转移Guest帐户，则会向接收TransferKey的终端发送`TRANSFER_KICKOUT`信息。

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
