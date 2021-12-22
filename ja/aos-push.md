## Game > Gamebase > Android SDK ご利用ガイド > Push

### Settings

#### Android Notification Icon

プッシュ通知が届いた時に表示されるアイコンは、基本状態ではアプリアイコンが使用されます。

しかしAndroidのプッシュアイコンは、アルファ領域を適用した単色の画像を使用した時のみ正常に表示されます。
[マテリアルデザインガイド - Android Notification \[LINK\]](https://material.io/design/platform-guidance/android-notifications.html#anatomy-of-a-notification)

端末解像度ごとにアイコンサイズも決められています。次を参考にしてプッシュアイコンを準備してください。

* MDPI - 24 x 24  (drawable-mdpi)
* HDPI - 36 x 36  (drawable-hdpi)
* XHDPI - 48 x 48  (drawable-xhdpi)
* XXHDPI - 72 x 72  (drawable-xxhdpi)
* XXXHDPI - 96 x 96  (drawable-xxxhdpi)

プッシュアイコンのファイル名にスペース、英字大文字、特殊文字は使用しないでください。

そして次のNotification Optionsガイドを参考にして**default_small_icon**(AndroidManifest.xml)または**setSmallIconName**(API)を設定する必要があります。
[Game > Gamebase > Android SDK使用ガイド > 始める > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)
[Game > Gamebase > Android SDK使用ガイド > プッシュ > Notification Options > Set Notification Options with RegisterPush in Runtime](./aos-push/#set-notification-options-with-registerpush-in-runtime)

### Register Push

次のAPIを呼び出して、 NHN Cloud Pushに該当ユーザーを登録します。<br/>
プッシュ同意有無(enablePush)、広告性プッシュ同意有無(enableAdPush)、夜間広告性プッシュ同意有無(enableAdNightPush)値をユーザーから取得し、次のAPIを呼び出して登録を完了します。

> <font color="red">[注意]</font><br/>
>
> UserIDごとにプッシュ設定が異なる場合があり、プッシュトークンの有効期限切れも発生することがあるため、ログイン後は毎回registerPush APIを呼び出すことを推奨します。

**API**

```java
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseCallback callback);
+ (void)Gamebase.Push.registerPush(@NonNull Activity activity,
                                   @NonNull PushConfiguration configuration,
                                   @NonNull GamebaseNotificationOptions notificationOptions,
                                   @NonNull GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = PushConfiguration.newBuilder()
        .enablePush(enablePush)
        .enableAdAgreement(enableAdPush)
        .enableAdAgreementNight(enableAdNightPush)
        .build();

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

### Notification Options

* 端末に表示する通知をどのような形式で表示するかをNotification Optionsで変更できます。
* Notification Optionsは、AndroidManifest.xmlに設定したり、ランタイムにregisterPush APIを呼び出して変更できます。

#### Set Notification Options with AndroidManifest.xml

通知オプションは、AndroidManifest.xmlに定義して設定できます。<br/>
設定方法は、以下のガイドを確認してください。

[Game > Gamebase > Android SDK使用ガイド > 始める > Setting > AndroidManifest.xml > Notification Options](./aos-started/#notification-options)

#### Set Notification Options with RegisterPush in Runtime

通知オプションをAndroidManifest.xmlに定義しないで、ランタイムに設定することもできます。またはAndroidManifest.xmlに定義した値をランタイムに変更することもできます。
registerPush APIを呼び出す時、GamebaseNotificationOptionsの引数を追加して通知オプションを設定します。
GamebaseNotificationOptions.newBuilder()の引数にGamebase.Push.getNotificationOptions()の呼び出し結果を渡すと、現在の通知オプションで初期化されたBuilderが作成されるため、必要な値のみ変更できます。<br/>
設定可能な値は次のとおりです。

| API                   | Parameter       | Description        |
| --------------------  | ------------ | ------------------ |
| enableForeground      | boolean      | アプリがフォアグラウンド状態の時の通知表示有無<br/>**default**: false |
| enableBadge           | boolean      | バッジアイコン使用有無<br/>**default**: true |
| setPriority           | int          | 通知の優先順位。以下の5つの値を設定できます。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2<br/>**default**: NoticationComapt.HIGH |
| setSmallIconName         | String       | 通知用の小さなアイコンファイル名。<br/>設定しない場合、アプリアイコンが使用されます。<br/>**default**: null |
| setSoundFileName      | String       | 通知音ファイル名。Android 8.0未満のOSでのみ動作します。<br/>「res/raw」フォルダのmp3、wavファイル名を指定すると、通知音が変更されます。<br/>**default**: null |

**Example**

```java
boolean enableForeground;
boolean enableBadge;
// NotificationCompat.PRIORITY_MIN     : -2
// NotificationCompat.PRIORITY_LOW     : -1
// NotificationCompat.PRIORITY_DEFAULT :  0
// NotificationCompat.PRIORITY_HIGH    :  1
// NotificationCompat.PRIORITY_MAX     :  2
int priority;
String smallIconName;
String soundFileName;

GamebaseNotificationOptions currentOptions = Gamebase.Push.getNotificationOptions(activity);
GamebaseNotificationOptions notificationOptions = GamebaseNotificationOptions.newBuilder(currentOptions)
        .enableForeground(enableForeground)
        .enableBadge(enableBadge)
        .setPriority(priority)
        .setSmallIconName(smallIconName)
        .setSoundFileName(soundFileName)
        .build();

Gamebase.Push.registerPush(activity, pushConfiguration, notificationOptions, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
        }
    }
});
```

#### Get NotificationOptions

プッシュを登録する時、既に設定している通知オプション値を取得します。
AndroidManifest.xmlに設定した値とランタイムに変更された値を含め、最も新しい設定を取得します。

**API**

```java
+ (GamebaseNotificationOptions)Gamebase.Push.getNotificationOptions(@NonNull Context context);
```

### Request Push Settings

ユーザーのPush設定を照会するために、次のAPIを利用します。<br/>
コールバックによるGamebasePushTokenInfoの値からユーザー設定値を取得することができます。

**API**

```java
+ (void)Gamebase.Push.queryTokenInfo(@NonNull Activity activity,
                                     @NonNull GamebaseDataCallback<GamebasePushTokenInfo> callback);

// Legacy API
+ (void)Gamebase.Push.queryPush(@NonNull Activity activity,
                                @NonNull GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

```java
Gamebase.Push.queryTokenInfo(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(GamebasePushTokenInfo data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.agreement.pushEnabled;
            boolean enableAdPush = data.agreement.adAgreement;
            boolean enableAdNightPush = data.agreement.adAgreementNight;
            String token = data.token;
        } else {
            // Failed.
        }
    }
});
```

#### GamebasePushTokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | String                | Pushトークンタイプ    |
| token               | String                | トークン              |
| userId              | String                | ユーザーID         |
| deviceCountryCode   | String                | 国コード        |
| timezone            | String                | 標準時間帯        |
| registeredDateTime  | String                | トークンアップデート時間 |
| languageCode        | String                | 言語設定         |
| agreement           | GamebasePushAgreement | 受信同意有無      |

#### GamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | boolean | 通知表示同意有無         |
| adAgreement      | boolean | 広告性通知表示同意有無    |
| adAgreementNight | boolean | 夜間広告性通知表示同意有無 |

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 前回のPush APIの呼び出しが完了していません。<br>前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次ドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud Pushライブラリーで発生したエラーです。
* エラーの詳細は次のように確認できます。

```java
Gamebase.Push.registerPush(activity, pushConfiguration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Register push successful");
            ...
        } else {
            Log.e(TAG, "Register push failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PUSH_EXTERNAL_LIBRARY_ERROR) {
                // TOAST Push Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* NHN Cloud Push SDKのエラーコードは次の通りです。

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API呼び出し成功 |
| NOT_INITIALIZE | 100 | NHN Cloud SDKまたはNHN Cloud Push SDKが初期化されていない場合。 |
| PROVIDER_SDK_ERROR | 101 | 外部SDK(Firebase, Tencent)でエラーが発生した場合。 |
| USER_ID_NOT_REGISTERED | 102 | ログインしていない場合。 |
| UNSUPPORTED_PUSH_TYPE | 103 | PushTypeの入力に誤りがあるか、プッシュライブラリがプロジェクトに含まれていない場合。 |
| API_SERVER_ERROR | 104 | NHN Cloud PushサーバーAPIの呼び出しに失敗した場合。 |
| TOKEN_NOT_REGISTERED | 105 | 内部にキャッシュされたPush Tokenが存在しない場合。 |
