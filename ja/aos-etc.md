## Game > Gamebase > Android SDK ご利用ガイド > ETC

## Additional Features

Gamebaseで対応している付加機能について説明します。

### Device Language

* 端末に設定されている言語コードを返します。
* 複数の言語が登録されている場合、優先権が最も高い言語だけを返します。

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
> Gamebaseのクライアントメッセージには、英語(en)、韓国語(ko)、日本語(ja)のみ含まれます。

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

> <font color="red">[注意]</font><br/>
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
  "${言語コード}": {
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

* Gamebaseは、システムの国コード(country code)を次のようなAPIで提供しています。
* APIごとに特徴があるため、使用用途に合ったAPIを選択してください。

#### USIM Country Code

* USIMに記録された国コードを返します。
* USIMに無効な国コードが記録されていても、確認せずにそのまま返します。
* 値が空の場合、'ZZ'を返します。

**API**

```java
+ (String)Gamebase.getCountryCodeOfUSIM()
```

#### Device Country Code

* OSから伝達された端末国コードを、確認しないでそのまま返します。
* 端末国コードは、'言語'設定に応じてOSが自動的に決定します。
* 複数の言語が登録されている場合、優先権が最も高い言語で国コードを決定します。
* 値が空の場合、'ZZ'を返します。

**API**

```java
+ (String)Gamebase.getCountryCodeOfDevice()
```

#### Intergrated Country Code

* USIM、端末言語設定の順序で国コードを確認して返します。
* getCountryCode APIは次の順序で動作します。
	1. USIMに記録されている国コードを確認し、値があれば別途確認しないでそのまま返します。
	2. USIM国コードが空の値の場合は端末国コードを確認し、値があれば別途確認しないでそのまま返します。
	3. USIM、端末国コードがどちらも空の値の場合は、'ZZ'を返します。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Server Push
* Gamebaseサーバーからクライアント端末に送るServer Push Messageを処理できます。
* GamebaseクライアントでServerPushEvent Listenerを追加すると、該当メッセージをユーザーが受信して処理でき、追加されたServerPushEvent Listenerを削除できます。


#### Server Push Type
現在GamebaseでサポートするServer Push Typeは次の通りです。

* ServerPushEventMessage.Type.APP_KICKOUT (= "appKickout")
    * TOAST Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseと接続されたすべてのクライアントで**APP_KICKOUT**メッセージを受け取ることになります。

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/serverpush_flow_001_1.11.0.png)

#### Add ServerPushEvent
Gamebase ClientにServerPushEventを登録し、GamebaseコンソールおよびGamebaseサーバーで発行されたPushイベントを処理できます。

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
Gamebaseに登録されたServerPushEventを削除できます。

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
* Gamebase ObserverでGamebaseの各種ステータス変動イベントを受け取って処理できます。
* ステータス変動イベント：ネットワークタイプ変動、Launchingステータス変動(メンテナンスなどによるステータス変動)、Heartbeat情報変動(ユーザー利用停止などによるHeartbeat情報変動)など


#### Observer Type
現在GamebaseでサポートするObserver Typeは次の通りです。

* Networkタイプ変動
    * ネットワーク変動事項情報を受け取れます。例えばObserverMessage.data.get("code")の値でNetwork Typeを知ることができます。
    * Type：ObserverMessage.Type.NETWORK (= "network")
    * Code：NetworkManagerに宣言された定数は次の通りです。
        * NetworkManager.TYPE_NOT：-1
        * NetworkManager.TYPE_MOBILE：0
        * NetworkManager.TYPE_WIFI：1        
        * NetworkManager.TYPE_ANY：2
* Launchingステータス変動
    * 周期的にアプリケーションのステータスを確認するLaunching Status responseに変動がある時に発生します。例えば、メンテナンス、アップデート推奨などによるイベントがあります。
    * Type：ObserverMessage.Type.LAUNCHING (= "launching")
    * Code：LaunchingStatusに宣言された定数は次の通りです。
        * LaunchingStatus.IN_SERVICE：200
        * LaunchingStatus.RECOMMEND_UPDATE：201
        * LaunchingStatus.IN_SERVICE_BY_QA_WHITE_LIST：202
        * LaunchingStatus.REQUIRE_UPDATE：300
        * LaunchingStatus.BLOCKED_USER：301
        * LaunchingStatus.TERMINATED_SERVICE：302
        * LaunchingStatus.INSPECTING_SERVICE：303
        * LaunchingStatus.INSPECTING_ALL_SERVICES：304
        * LaunchingStatus.INTERNAL_SERVER_ERROR：500
* Heartbeat情報の変動
    * 周期的にGamebaseサーバーと接続を維持するHeartbeat responseに変動がある時に発生します。例えばユーザー利用停止によるイベントがあります。
    * Type：ObserverMessage.Type.HEARTBEAT (= "heartbeat")
    * Code：GamebaseErrorに宣言された定数は次の通りです。
        * GamebaseError.INVALID_MEMBER：6
        * GamebaseError.BANNED_MEMBER：7

![observer](http://static.toastoven.net/prod_gamebase/DevelopersGuide/observer_flow_001_1.11.0.png)

#### Add Observer
Gamebase ClientにObserverを登録して、各種ステータス変動イベントを処理できます。

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
Gamebaseに登録されたObserverを削除できます。

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

ゲーム指標をGamebase Serverに伝送できます。

> <font color="red">[注意]</font><br/>
>
> Gamebase AnalyticsでサポートするすべてのAPIは、ログイン後に呼び出すことができます。
>

> [TIP]
>
> Gamebase.Purchase.requestPurchase() APIを呼び出し、決済が完了すると、自動的に指標を伝送します。
>

#### Game User Data Settings

ゲームログイン後、ユーザーレベル情報を設定できます。

> <font color="red">[注意]</font><br/>
>
> ゲームログイン後にsetGameUserData APIを呼び出さない場合、他の指標でレベル情報が抜ける場合があります。
>

APIの呼び出しに必要なパラメータは下記の通りです。

**GameUserData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc |
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ユーザーレベルを表すフィールドです。 |
| channelId | O | String | チャンネルを表すフィールドです。 |
| characterId | O | String | キャラクター名を表すフィールドです。 |
| classId | O | String | 職業を表すフィールドです。 |

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

レベルアップすると、ユーザーレベル情報を変更できます。

APIの呼び出しに必要なパラメータは、下記の通りです。

**LevelUpData**

| Name                       | Mandatory(M) / Optional(O) | type | Desc	|
| -------------------------- | -------------------------- | ---- | ---- |
| userLevel | M | int | ユーザーレベルを表すフィールドです。 |
| levelUpTime | M | long | Epoch Timeで入力します。</br>Millisecond単位で入力します。 |



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

Gamebaseでは顧客からの問い合わせに対応するための機能を提供します。

> [TIP]
>
> TOAST Contactサービスと連携して使用すると、簡単に顧客からの問い合わせに対応できます。
> 詳細はTOAST Contactサービスの利用ガイドを参照してください。
> [TOAST Online Contact Guide](/Contact%20Center/ja/online-contact-overview/)
>

#### Open Contact WebView

Gamebase Consoleに入力した**サポートURL**をWebビューで表示する機能です。
**Gamebase Console > App > InApp URL > Service center**に入力した値が使用されます。

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
