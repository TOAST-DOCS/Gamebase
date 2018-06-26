## Game > Gamebase > Android SDK User Guide > ETC

## Additional Features

Additional functions provided by Gamebase are described as below:

### Display Language

* The display language can be changed into another language which is not set on a device.
* Gamebase displays messages which are included in a client or as received by a server.
* With DisplayLanguage, messages are displayed in an appropriate language for the language code (ISO-639) set by the user.
* If necessary, language sets can be added as the user wants. The list of available language codes is as follows:

> [Note]
>
> Client messages of Gamebase include English(en) and Korean(ko), only. 

#### Types of Language Codes Supported by Gamebase

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

Each language code is defined in the `DisplayLanguage` class.

> `[Warning]`
>
> Gamebase distinguishes the language code between the upper and lower case. 
> For example, settings like 'EN' or 'zh-ch' may cause a problem. 

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

#### Set Display Language with Gamebase Initialization

Display Language can be set when Gamebase is initialized.

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

You can change the initial setting of Display Language.

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

You can retrieve the current application of Display Language.

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

#### Add New Language Sets 

To use another language in addition to default Gamebase languages (en, ko, ja), go to gamebase-sdk-base.aar > res > raw and add a value to the localizedstring.json file.

![localizedstring.json](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

The localizedstring.json has a format defined as below: 

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

To add another language, add `"${language code}":{"key":"value"}` to the localizedstring.json file. 

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

If key is missing from inside of "${language code}":{ } of the json format above, `Languages Set on Device` or `en` will be automatically entered. 

#### Priority in Display Language

If Display Language is set via initialization and SetDisplayLanguageCode API, the final application may be different from what has been entered. 

1. Check if the languageCode you enter is defined in the localizedstring.json file. 
2. See if, during Gamebase initialization, the language code set on the device is defined in the localizedstring.json file. (This value shall maintain even if the language set on device changes after initialization.)
3.  `en`, which is the default value of Display Language, is automatically set.





### Server Push
* Handles Server Push Messages from Gamebase server to a client device. 
* Add ServerPushEvent Listener to Gamebase Client, and the user can handle messages; the added ServerPushEvent Listener can be deleted. 


#### Server Push Type
Server Push Types currently supported by Gamebase are as follows: 

* ServerPushEventMessage.Type.APP_KICKOUT (= "appKickout")
    * Go to `Operation > Kickout`  in the TOAST Gamebase console and register Kickout ServerPush messages, and `APP_KICKOUT` messages are sent to all clients connected to Gamebase.
* ServerPushEventMessage.Type.TRANSFER_KICKOUT (= "transferKickout")
	* TransferKey 를 통해 게스트 계정 이전이 성공한 경우, TransferKey를 발급받았던 단말기로 `TRANSFER_KICKOUT` 메세지가 전송됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Use the API below, register ServerPushEvent to handle the push event triggered from the Gamebase Console and Gamebase server. 

**API**

```java
+ (void)Gamebase.addServerPushEvent(ServerPushEvent serverPushEvent);
```

**Example**

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


#### Remove ServerPushEvent
Use the APIs below to delete ServerPushEvent registered in Gamebase. 

**API**

```java
+ (void)Gamebase.removeServerPusEvent(ServerPushEvent serverPushEvent);
+ (void)Gamebase.removeAllServerPusEvent();
```

**Example**

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
* With Gamebase Observer, receive and process status change events of Gamebase. 
* Status change events : change of network type, change of launching status (change of status due to maintenance, and etc.), and change of heartbeat information (change of heartbeat information due to service suspension), and etc.


#### Observer Type
The Observer Types currently supported by Gamebase are as follows:

* Change of Network Type
    * Receive information on changes of a network. For instance, find a network type with the ObserverMessage.data.get("code") value. 
    * Type: ObserverMessage.Type.NETWORK (= "network")
    * Code: Refer to the constant numbers declared in NetworkManager. 
        * NetworkManager.TYPE_NOT : -1
        * NetworkManager.TYPE_MOBILE : 0
        * NetworkManager.TYPE_WIFI : 1        
        * NetworkManager.TYPE_ANY : 2
* Change of Launching Status 
    * Occurs when there is a change in the launching status response which periodically checks application status. For example, events occur for maintenance, or update recommendations. 
    * Type: ObserverMessage.Type.LAUNCHING (= "launching")
    * Code: Refer to the constant numbers declared in LaunchingStatus. 
        * LaunchingStatus.IN_SERVICE : 200
        * LaunchingStatus.RECOMMEND_UPDATE : 201
        * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST : 202
        * LaunchingStatus.REQUIRE_UPDATE : 300
        * LaunchingStatus.BLOCKED_USER : 301
        * LaunchingStatus.TERMINATED_SERVICE : 302
        * LaunchingStatus.INSPECTING_SERVICE : 303
        * LaunchingStatus.INSPECTING_ALL_SERVICES : 304
        * LaunchingStatus.INTERNAL_SERVER_ERROR : 500
* Change of Heartbeat Information 
    * Occurs when there is a change in the heartbeat response which periodically maintains connection with the Gamebase server. For example, an event occurs for service suspension. 
    * Type: ObserverMessage.Type.HEARTBEAT (= "heartbeat")
    * Code: Refer to the constant numbers declared in GamebaseError.
        * GamebaseError.INVALID_MEMBER: 6
        * GamebaseError.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Use the API below, register Observer to handle the status change events of Gamebase.

**API**

```java
+ (void)Gamebase.addObserver(Observer observer);
```

**Example**

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


#### Remove Observer
Use the APIs below to delete Observer registered in Gamebase. 

**API**

```java
+ (void)Gamebase.removeObserver(Observer observer);
+ (void)Gamebase.removeAllObserver();
```

**Example**

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