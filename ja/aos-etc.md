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

メンテナンスポップアップなどでGamebaseが表示する言語は、端末に設定された言語と同じです。

しかしゲームで表示する言語を、端末に設定した言語ではなく、別のオプションで変更できるゲームがあります。
例えば、端末に設定された言語は英語ですが、ゲーム表示言語を日本語に変更した場合、 Gamebaseで表示する言語も日本語に変更したいですが、Gamebaseが表示する言語は端末に設定された言語の英語が表示されます。

このように`端末に設定された言語ではなく、他の言語でGamebaseのメッセージを表示したい`アプリケーションのためにGamebaseは`Display Language`という機能を提供します。

GamebaseはDisplay Languageで設定した言語でGamebaseのメッセージを表示します。
Display Languageに入力する言語コードは、以下の表(**Gamebaseでサポートする言語コードの種類**)で指定したコードだけを使用できます。

> <font color="red">[注意]</font><br/>
>
> * Display Languageは、端末設定言語と関係なくGamebaseの表示言語を変更したい場合にのみ使用してください。
> * Display Language CodeはISO-639形式の値で、大文字/小文字を区別します。
> 'EN'または'zh-cn'と設定すると問題が発生する場合があります。
> * もしDisplay Language Codeに入力した値が以下の表(**Gamebaseでサポートする言語コードの種類**)に存在しない場合、Display Langauge CodeはGamebaseコンソールで設定したデフォルト言語に指定されます。
>     * もしGamebaseコンソールで言語設定を行っていなければ英語(en)がデフォルト言語に設定されます。

> [参考]
>
> * Gamebaseのクライアントに含まれていない言語セットは直接追加できます。
> **新規言語セット追加**項目を参照してください。

#### Gamebaseでサポートする言語コードの種類

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

該当する言語コードは、「DisplayLanguage」クラスに定義されています。

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

Gamebaseで提供するデフォルト言語(ko、en、ja、zh-CN、zh-TW、th)以外の言語を追加したい場合は、プロジェクトのres > rawフォルダにlocalizedstring.jsonファイルを追加してください。

![localizedstring.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-etc_001_1.11.0.png)

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

他の言語の追加が必要な場合は、localizedstring.jsonファイルの該当言語コードに`"key":"value"`形式で値を追加してください。

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

#### Display Languageの優先順位

初期化及びSetDisplayLanguageCodeAPIを通してDisplay Languageを設定する場合、最終的に適用されるDisplay Languageは、入力した値と違う値が適用されることがあります。

1. 入力されたlanguageCodeがlocalizedstrin.jsonファイルに定義されているかどうかを確認します。
2. 1番が失敗した場合は、Gamebase初期化時、端末に設定された言語コードがlocalizedstring.jsonファイルに定義されているかを確認します。(この値は初期化後、端末に設定された言語を変更しても維持されます。)
3. 2番が失敗した場合は、Gamebaseコンソールに設定されたデフォルト言語が設定されます。
4. Gamebaseコンソールに言語設定がなければ、`en`がデフォルト値に設定されます。

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

![observer](https://static.toastoven.net/prod_gamebase/DevelopersGuide/get_country_code_001_1.14.0.png)

**API**

```java
+ (String)Gamebase.getCountryCode()
```

### Gamebase Event Handler

* Gamebaseは各種イベントを**GamebaseEventHandler**という1つのイベントシステムで全て処理できます。
* GamebaseEventHandlerは下記のAPIを利用して簡単にListenerを追加/削除できます。

**API**

```java
+ (void)Gamebase.addEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeEventHandler(GamebaseEventHandler handler);
+ (void)Gamebase.removeAllEventHandler();
```

**VO**

```java
class GamebaseEventMessage {
	// Eventの種類を表します。
    // GamebaseEventCategoryクラスの値が割り当てられます。
    @NonNull
    final public String category;

    // 各categoryに合ったVOに変換できるJSON Stringデータです。
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

* CategoryはGamebaseEventCategoryクラスに定義されています。
* イベントは大きくServerPush、Observer、Purchase、Pushに分けられ、各Categoryに基づいて、GamebaseEventMessage.dataを次の表のような方法でVOに変換できます。

| Event種類 | GamebaseEventCategory | VO変換方法 | 備考 |
| --------- | --------------------- | ----------- | --- |
| LoggedOut | GamebaseEventCategory.LOGGED_OUT | GamebaseEventLoggedOutData.from(message.data) | \- |
| ServerPush | GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED<br>GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT<br>GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT | GamebaseEventServerPushData.from(message.data) | \- |
| Observer | GamebaseEventCategory.OBSERVER_LAUNCHING<br>GamebaseEventCategory.OBSERVER_NETWORK<br>GamebaseEventCategory.OBSERVER_HEARTBEAT | GamebaseEventObserverData.from(message.data) | \- |
| Purchase<br>- プロモーション決済<br>- 지연 결제 | GamebaseEventCategory.PURCHASE_UPDATED | PurchasableReceipt.from(message.data) | \- |
| Push<br>- メッセージ受信 | GamebaseEventCategory.PUSH_RECEIVED_MESSAGE | PushMessage.from(message.data) | **isForeground**値を使用してForegroundでメッセージを受信したかどうかを確認できます。 |
| Push<br>- メッセージクリック | GamebaseEventCategory.PUSH_CLICK_MESSAGE | PushMessage.from(message.data) | **isForeground**値がありません。 |
| Push<br>- アクションクリック | GamebaseEventCategory.PUSH_CLICK_ACTION | PushAction.from(message.data) | RichMessageボタンを押すと動作します。 |

#### How to handle events when the application is not running

* カスタムApplicationクラスでGamebaseEventHandlerを登録すると、アプリケーションが実行されなかった時もにもイベント処理をできます。

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

* Gamebase Access Tokenの有効期限が切れてネットワークセッションを復元するためにログイン関数の呼び出しが必要な場合に発生するイベントです。


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

* Gamebaseサーバーからクライアント端末へ送信するメッセージです。
* GamebaseでサポートするServer Push Typeは次の通りです。
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseと接続したすべてのクライアントでキックアウトメッセージを受け取ります。
        * クライアント端末でサーバーメッセージを受信した直後に発生するイベントです。
        * 「オートプレイ」のようにゲームが動作中の場合、ゲームを一時停止させる目的で活用できます。
	* GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT
    	* NHN Cloud Gamebaseコンソールの**Operation > Kickout**でキックアウトServerPushメッセージを登録すると、Gamebaseに接続されたすべてのクライアントでキックアウトメッセージを受信します。
        * クライアント端末でサーバーメッセージを受信した時、ポップアップを表示しますが、ユーザーがポップアップを閉じたときに発生するイベントです。
    * GamebaseEventCategory.SERVER_PUSH_TRANSFER_KICKOUT
    	* Guestアカウントを他の端末へ移行すると、以前の端末でキックアウトメッセージを受信します。

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

* Gamebase Gamebaseの各種状態変動イベントを処理するシステムです。
* GamebaseでサポートするObserver Typeは次の通りです。
    * GamebaseEventCategory.OBSERVER_LAUNCHING
    	* メンテナンスが開始または終了した場合、新しいバージョンが配布されてアップデートが必要な場合など、Launching状態が変更された時に動作します。
    	* GamebaseEventObserverData.code : LaunchingStatusの値を意味します。
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
    	* 退会処理や利用停止により、ユーザーアカウントの状態が変わった時に動作します。
    	* GamebaseEventObserverData.code : GamebaseError値を意味します。
            * GamebaseError.INVALID_MEMBER: 6
            * GamebaseError.BANNED_MEMBER: 7
    * GamebaseEventCategory.OBSERVER_NETWORK
    	* ネットワーク変動事項情報を受け取れます。
    	* ネットワークが切断されたり、接続された時、またはWi-FiからCellularネットワークに変更された時に動作します。
    	* GamebaseEventObserverData.code : NetworkManager値を意味します。
            * NetworkManager.TYPE_NOT: -1
            * NetworkManager.TYPE_MOBILE: 0
            * NetworkManager.TYPE_WIFI: 1
            * NetworkManager.TYPE_ANY: 2

**VO**

```java
class GamebaseEventObserverData {
	// 状態値を表す情報です。
    public int code;

    // 状態に関連するメッセージ情報です。
    @Nullable
    public String message;

    // 追加情報用の予備フィールドです。
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

* Promotion 코드 입력을 통해 상품을 획득한 경우 또는 Pending 결제(느린 결제, 부모 동의 등)가 완료되었을 때 발생하는 이벤트입니다.
* 決済領収書情報を取得できます。

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
                        // If the user got item by 'Promotion Code' or
                        // 'Lazy purchase', or 'Parents permission',...,
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

* Pushメッセージが到着した時に発生するイベントです。
* **isForeground**フィールドを利用してフォアグラウンドでメッセージを受信したのか、バックグラウンドでメッセージを受信したのかを区別できます。
* extrasフィールドをJSONに変換して、Push送信時に送信されたカスタム情報を取得することもできます。

**VO**

```java
class PushMessage {
	// メッセージ固有のidです。
    @NonNull
    public String id;

    // Pushメッセージのタイトルです。
    @Nullable
    public String title;

    // Pushメッセージの本文です。
    @Nullable
    public String body;

    // JSONObjectに変換してすべての情報を確認できます。
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

* 受信したPushメッセージをクリックした時に発生するイベントです。
* 「GamebaseEventCategory.PUSH_RECEIVED_MESSAGE」とは異なり、**isForeground**フィールドが存在しません。

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

* Rich Message機能を利用して作成したボタンをクリックした時に発生するイベントです。
* actionTypeは、次の項目が提供されます。
	* "OPEN_APP"
	* "OPEN_URL"
	* "REPLY"
	* "DISMISS"

**VO**

```java
class PushAction {
	// ボタンアクションの種類です。
    @NonNull
    public String actionType;

	// PushMessageデータです。
    @NonNull
    public PushMessage message;

	// Pushコンソールで入力したユーザーテキストです。
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
> NHN Cloud Contactサービスと連携して使用すると、簡単に顧客からの問い合わせに対応できます。
> 詳細はNHN Cloud Contactサービスの利用ガイドを参照してください。
> [NHN Cloud Online Contact Guide](https://docs.nhncloud.com/ja/Contact%20Center/ja/online-contact-overview/)

> <font color="red">[注意]</font><br/>
>
> * Gamebase Android SDK 2.53.0以降のバージョンは、下記のガイドに従ってAndroidManifest.xmlに権限宣言を追加するだけで、サポートにファイルを添付する際、Gamebase Android SDKが自動的に権限を要求します。
>     * [Game > Gamebase > Android SDK使用ガイド > はじめる > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)
> * Gamebase Android SDK 2.52.0以下のバージョンは、プラットフォーム別ガイドを参考にして直接権限取得処理を実装する必要があります。
>     * [Android Developer's Guide :Request App Permissions](https://developer.android.com/training/permissions/requesting)
>     * [Unity Guide : Requesting Permissions](https://docs.unity3d.com/2018.4/Documentation/Manual/android-RequestingPermissions.html)

#### Customer Service Type

**Gamebase コンソール > App > Customer service**では、次のように3つのタイプのサポートを選択できます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_001_2.16.0.png)

| Customer Service Type     | Required Login |
| ------------------------- | -------------- |
| Developer customer center | X              |
| Gamebase customer center  | △             |
| NHN Cloud Online Contact      | △              |

タイプに応じてGamebase SDKのサポートAPIは次のURLを使用します。

* 開発会社独自のサポート(Developer customer center)
    * **サポートURL**に入力したURL.
* Gamebase提供サポート(Gamebase customer center)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。
* NHN Cloud組織商品(Online Contact)
    * ログイン前：ユーザー情報が**ない**サポートURL。
    * ログイン後：ユーザー情報が含まれたサポートURL。

#### Open Contact WebView

サポートWebビューを表示します。
URLはサポートタイプに基づいて決定されます。
ContactConfigurationでURLに追加情報を伝達できます。

**ContactConfiguration**

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | ContactConfigurationオブジェクトはnewBuilder()関数で作成できます。 |
| build() | **M** | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setUserName(String userName) | O | ユーザー名(ニックネーム)を伝達したい時に使用します。<br>NHN Cloud組織商品(Online Contact)タイプで使用するフィールドです。<br>**default** : null |
| setAdditionalURL(String additionalURL) | O | 開発会社の独自サポートURLの後ろにつく追加のURLです。<br>サポートタイプが`CUSTOM`の場合にのみ使用してください。<br>**default** : null |
| setAdditionalParameters(Map&lt;String, String&gt; additionalParameters) | O | サポートセンターURLの後ろにつく追加のパラメータです。<br>**default** : null |
| setExtraData(Map&lt;String, Object&gt; extraData) | O | 任意のextra dataをサポートのオープン時に伝達します。<br>**default** : EmptyMap |

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
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |
| UI\_CONTACT\_FAIL\_ANDROID\_DUPLICATED\_VIEW(6913)  | サポートWebビューがすでに表示中です。 |

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

サポートのWebビューを表示するのに使用されるURLを返します。

**API**

```java
+ (void)Gamebase.Contact.requestContactURL(@NonNull final GamebaseDataCallback<String> callback);

+ (void)Gamebase.Contact.requestContactURL(@NonNull final ContactConfiguration configuration,
                                           @NonNull final GamebaseDataCallback<String> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1)                                 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_CONTACT\_FAIL\_INVALID\_URL(6911)               | サポートURLが存在しません。<br>Gamebaseコンソールの**サポートURL**を確認してください。 |
| UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET(6912) | ユーザーを識別するための臨時チケットの発行に失敗しました。 |

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

サポートのタイプが「NHN Cloud組織商品」の場合、「追加パラメータ」項目のKeyに**from**、Valueに**app**を入力すると、ファイル添付時のタイプ選択ポップアップが表示されます。
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_002_2.53.0.png)
![](https://static.toastoven.net/prod_gamebase/DevelopersGuide/etc_customer_center_003_2.53.0.png)

### Age Signals Support

Texas SB 2420及び類似する州の法律は、未成年者の保護のためにアプリでユーザーの年齢確認を求めています。
Gamebaseは、Google Play Age Signals APIをラッピングし、このような要件を満たすAPIを提供します。

#### Dependencies

SDK内部的に以下の依存関係を持っています。
`implementation 'com.google.android.play:age-signals'`

#### Requirements

* 最小Android API Level: API 23 (Android 6.0)以上
* Gamebase SDKバージョン: 2.78.0以上

#### Google guide

詳細は[https://developer.android.com/google/play/age-signals](https://developer.android.com/google/play/age-signals)を参照してください。

> <font color="red">[注意]</font><br/>
>
> Play Age Signals API (ベータ)は、2026年1月1日まで例外を発生させます。1月1日からAPIはリアルタイムレスポンスを返します。
>

#### Check age signal

**Gamebase.AgeSignals.checkAgeSignals(Context, GamebaseAgeSignalsRequest)**を呼び出し、年齢情報を確認します。

**GamebaseAgeSignalsRequest**

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | GamebaseAgeSignalsRequestオブジェクトは、newBuilder()関数を通じて生成できます。 |
| build() | **M** | 設定を終えたBuilderをRequestオブジェクトに変換します。 |

**API**

```java
+ (void)Gamebase.AgeSignals.checkAgeSignals(@NonNull Context context,
                                            @NonNull GamebaseAgeSignalsRequest request,
                                            @NonNull GamebaseDataCallback<GamebaseAgeSignalsResult> callback) 
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_SUPPORTED(10)                     | Android API 23未満のデバイスで呼び出されました。 |
| AUTH\_EXTERNAL\_LIBRARY\_ERROR(3009) | Google Play Age Signals APIでエラーを返しました。 |

**Example**

```kotlin
// Age Signalsリクエスト生成
val request = GamebaseAgeSignalsRequest.newbuilder().build()

// Age Signals確認リクエスト
Gamebase.AgeSignals.checkAgeSignals(context, request) { result, exception ->
    if (Gamebase.isSuccess(exception)) {
        // 成功: 年齢確認情報処理
        handleAgeSignalsResult(result)
    } else {
        // 失敗: エラー処理
        val errorCode = exception?.code
        val errorMessage = exception?.message
        when (errorCode) {
            GamebaseError.NOT_SUPPORTED -> {
                // Android API 23未満のデバイスではサポートされません。
                Log.e(TAG, "Age Signals API is not supported on this device")
            }
            GamebaseError.AUTH_EXTERNAL_LIBRARY_ERROR -> {
                // Google Playサービスでエラーが発生しました。
                Log.e(TAG, "Google Play Age Signals error: $errorMessage")
            }
        }
    }
}
```

#### Handle results

**GamebaseAgeSignalsResult.userStatus()**でユーザーの状態を確認できます。
Status値に従ってユーザー規制の有無を判断してください。

**GamebaseAgeSignalsVerificationStatus**

ユーザー検証状態定数です。

| Status                        | Code | Description          |
| ----------------------------- | ---- | -------------------- |
| VERIFIED                      | 0    | 18歳以上の成人          |
| SUPERVISED                    | 1    | 保護者の同意がある未成年者 |
| SUPERVISED\_APPROVAL\_PENDING | 2    | 保護者承認待機中        |
| SUPERVISED\_APPROVAL\_DENIED  | 3    | 保護者承認拒否済み         |
| UNKNOWN                       | 4    | 検証されていないユーザー        |


**Example**

```kotlin
private fun handleAgeSignalsResult(result: GamebaseAgeSignalsResult) {
    val userStatus = result.userStatus()

    if (userStatus == null) {
        // ユーザーが規制地域(テキサス、ユタ、ルイジアナ)にいないことを意味します。
        // 規制対象ではないユーザーに対するアプリのロジックを進行できます。
        return
    }

    when (userStatus) {
        GamebaseAgeSignalsVerificationStatus.VERIFIED -> {
            // 18歳以上の成人ユーザー
            // 全ての機能に対するアクセス許可
            // ageLowerとageUpperはnullです
            handleAdultUser(result)
        }
        GamebaseAgeSignalsVerificationStatus.SUPERVISED -> {
            // 保護者の同意がある未成年者
            // Texas SB 2420に従い未成年者のための制限された機能を提供

            // 年齢帯を確認できます。
            val ageLower = result.ageLower() // 例: 13
            val ageUpper = result.ageUpper() // 例: 17
            val installId = result.installId()
            handleSupervisedMinor(result)
        }
        GamebaseAgeSignalsVerificationStatus.SUPERVISED_APPROVAL_PENDING -> {
            // 保護者の承認を待つ間、制限された機能のみ提供
            // ユーザーに承認待機中であることを通知
            handleApprovalPending(result)
        }
        GamebaseAgeSignalsVerificationStatus.SUPERVISED_APPROVAL_DENIED -> {
            // 保護者が承認を拒否した場合
            // 制限された機能のみ提供するか、サービス利用不可を案内
            handleApprovalDenied(result)
        }
        GamebaseAgeSignalsVerificationStatus.UNKNOWN -> {
            // 該当管轄地域で検証されていないユーザーまたは年齢確認情報を使用できない場合
            // ユーザーにPlayストアを訪問して状態を解決するようにリクエストしてください。
            handleUnknownUser(result)
        }
        else -> {
          // このケースをロギングして潜在的な未来の状態に対しても柔軟に処理できるようにします。
        }
    }
}
```

**GamebaseAgeSignalsResult**

詳細は[https://developer.android.com/google/play/age-signals/use-age-signals-api#age-signals-responses](https://developer.android.com/google/play/age-signals/use-age-signals-api#age-signals-responses)を参照してください。
