## Game > Gamebase > Android SDK ご利用ガイド > ETC

## Additional Features

Gamebaseで対応している付加機能について説明します。

### Device Language

* 단말기에 설정된 언어 코드를 리턴합니다.
* 여러개의 언어가 등록된 경우, 우선권이 가장 높은 언어만을 리턴합니다.

**API**

```java
+ (String)Gamebase.getDeviceLanguageCode();
```

### Display Language

* Gamebaseに表示される言語をデバイスに設定されている言語ではない他の言語に変更することができます。
* Gamebaseは、クライアントに含まれているメッセージを表示したり、サーバーから取得したメッセージを表示します。
* DisplayLanguageを設定すると、ユーザーが設定した言語コード(ISO-639)に対応する言語でメッセージを表示します。
* 必要な場合、ユーザーがサポートしたい言語セットを追加することができます。(下の対応言語コードを参考)

> [参考]
>
> Gamebaseのクライアントメッセージには、英語(en)、韓国語(ko)、일본어(ja)のみ含まれます。

#### Gamebaseでサポートする言語コードの種類

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

該当する言語コードは、「DisplayLanguage」クラスに定義されています。

> `[注意]`
>
> Gamebaseでサポートしている言語コードは、大文字・小文字を区別します。
> “EN”や"zh-cn"のように設定する場合、問題が発生することがあります。

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

#### Gamebaseを初期化する際のDisplay Languageの設定

Gamebaseを初期化する際のDisplay Languageを設定することができます。

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

Gamebaseを初期化する際に入力されたDisplay Languageを変更することができます。

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

現在適用されているDisplay Languageを照会することができます。

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

#### 言語セットの新規追加

Gamebaseで提供するデフォルト言語(ko、en、ja)以外に他の言語を使用しなければならない場合、gamebase-sdk-base.aar > res > rawにあるlocalizedstring.jsonファイルに値を追加しなければなりません。

![localizedstring.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

localizedstring.jsonに定義されている形式は、次の通りです。

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

他の言語の追加が必要な場合、localizedstring.jsonファイルに`"${言語コード}":{"key":"value"}`の形で値を追加してください。

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

上記のjson形式で"${言語コード}":{ }内部にkeyが抜けている場合、「デバイスに設定されている言語」または`en`で自動入力されます。

#### Display Languageの優先順位

初期化及びSetDisplayLanguageCodeAPIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。

1. 入力されたlanguageCodeがlocalizedstrin.jsonファイルに定義されているかどうかを確認します。
2. Gamebaseを初期化する際に、デバイスに設定されている言語コードがlocalizedstrin.jsonファイルに定義されているかどうかを確認します。(この値は、初期化後にデバイスに設定されている言語を変更した場合でも維持されます。)
3. Display Languageのデフォルト値である`en`が自動で設定されます。

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

* USIM, 기기 언어 설정의 순서로 국가 코드를 확인하여 리턴합니다.
* getCountryCode API는 다음 순서로 동작합니다.
	1. USIM에 기록된 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	2. USIM 국가 코드가 빈 값이라면 단말기 국가 코드를 확인해 보고 값이 존재한다면 추가적인 체크 없이 그대로 리턴합니다.
	3. USIM, 단말기 국가 코드가 모두 빈 값이라면 'ZZ' 를 리턴합니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Server Push
* Gamebase 서버에서 클라이언트 기기로 보내는 Server Push Message를 처리할 수 있습니다.
* Gamebase 클라이언트에서 ServerPushEvent Listener를 추가하면 해당 메시지를 사용자가 받아서 처리할 수 있으며, 추가된 ServerPushEvent Listener를 삭제할 수 있습니다.


#### Server Push Type
현재 Gamebase에서 지원하는 Server Push Type은 다음과 같습니다.

* ServerPushEventMessage.Type.APP_KICKOUT (= "appKickout")
    * TOAST Gamebase 콘솔의 **Operation > Kickout** 에서 킥아웃 ServerPush 메시지를 등록하면 Gamebase와 연결된 모든 클라이언트에서 **APP_KICKOUT** 메시지를 받게 됩니다.

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Gamebase Client에 ServerPushEvent를 등록하여 Gamebase Console 및 Gamebase 서버에서 발급된 Push 이벤트를 처리할 수 있습니다.

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
Gamebase에 등록된 ServerPushEvent를 삭제할 수 있습니다.

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
* Gamebase Observer로 Gamebase의 각종 상태 변동 이벤트를 전달받아 처리할 수 있습니다.
* 상태 변동 이벤트 : 네트워크 타입 변동, Launching 상태 변동(점검 등에 의한 상태 변동), Heartbeat 정보 변동(사용자 이용 정지 등에 의한 Heartbeat 정보 변동) 등


#### Observer Type
현재 Gamebase에서 지원하는 Observer Type은 다음과 같습니다.

* Network 타입 변동
    * 네트워크 변동 사항 정보를 받을 수 있습니다. 예를 들어 ObserverMessage.data.get("code") 의 값으로 Network Type을 알 수 있습니다.
    * Type: ObserverMessage.Type.NETWORK (= "network")
    * Code: NetworkManager에 선언된 상수를 참고합니다. 
        * NetworkManager.TYPE_NOT: -1
        * NetworkManager.TYPE_MOBILE: 0
        * NetworkManager.TYPE_WIFI: 1        
        * NetworkManager.TYPE_ANY: 2
* Launching 상태 변동
    * 주기적으로 애플리케이션 상태를 확인하는 Launching Status response에 변동이 있을 때 발생합니다. 예를 들어 점검, 업데이트 권장 등에 의한 이벤트가 있습니다.
    * Type: ObserverMessage.Type.LAUNCHING (= "launching")
    * Code: LaunchingStatus에 선언된 상수를 참고합니다.
        * LaunchingStatus.IN_SERVICE: 200
        * LaunchingStatus.RECOMMEND_UPDATE: 201
        * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST: 202
        * LaunchingStatus.REQUIRE_UPDATE: 300
        * LaunchingStatus.BLOCKED_USER: 301
        * LaunchingStatus.TERMINATED_SERVICE: 302
        * LaunchingStatus.INSPECTING_SERVICE: 303
        * LaunchingStatus.INSPECTING_ALL_SERVICES: 304
        * LaunchingStatus.INTERNAL_SERVER_ERROR: 500
* Heartbeat 정보 변동
    * 주기적으로 Gamebase 서버와 연결을 유지하는 Heartbeat response에 변동이 있을 때 발생합니다. 예를 들어 사용자 이용 정지에 의한 이벤트가 있습니다.
    * Type: ObserverMessage.Type.HEARTBEAT (= "heartbeat")
    * Code: GamebaseError에 선언된 상수를 참조합니다.
        * GamebaseError.INVALID_MEMBER: 6
        * GamebaseError.BANNED_MEMBER: 7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase Client에 Observer를 등록하여 각종 상태 변동 이벤트를 처리할 수 있습니다.

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
Gamebase에 등록된 Observer를 삭제할 수 있습니다.

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
| userLevel | M | int |  |
| channelId | O | String |  |
| characterId | O | String |  |

**API**

```java
static void setGameUserData(GameUserData gameUserData)
```

**Example**

``` java
public void onLoginSuccess(, String channelId, String characterId) {
    int userLevel = 10;
    String channelId, characterId;

    GameUserData gameUserData = new GameUserData(userLevel);
    gameUserData.channelId = channelId; // Optional
    gameUserData.characterId = characterId; // Optional

    Gamebase.Analytics.setGameUserData(gameUserData);
}
```

#### Level Up Trace

레벨업이 되었을 경우 유저 레벨 정보를 변경할 수 있습니다.

API 호출에 필요한 파라미터는 아래와 같습니다.

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int |  |
| levelUpTime | O | long | Epoch Time으로 입력합니다.</br>Millisecond 단위로 입력 합니다. |
| channelId | O | String |  |
| characterId | O | String |  |



**API**

```java
static void TraceLevelUp(GamebaseRequest.Analytics.LevelUpData levelUpData)
```

**Example**

``` java
public void onLevelUp(int userLevel, long levelUpTime, String channelId, String characterId) {
    LevelUpData levelUpData = new LevelUpData(userLevel);
    levelUpData.levelUpTime = levelUpTime; // Optional
    levelUpData.channelId = channelId; // Optional
    levelUpData.characterId = characterId; // Optional

    Gamebase.Analytics.traceLevelUp(levelUpData);
}
```
