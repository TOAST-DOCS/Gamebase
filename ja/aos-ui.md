## Game > Gamebase > Android SDK ご利用ガイド > UI

## GameNotice

コンソールに画像と一緒に登録した告知事項を表示する機能です。

![GameNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gameNotice_guide_001.png)
![GameNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gameNotice_guide_002.png)

### Open GameNotice

ゲーム告知を画面に表示します。

#### Requiredパラメータ
* Activity:ゲーム告知が表示されるActivityです。


#### Optionalパラメータ
* GameNoticeConfiguration:ゲーム告知設定を変更できます。

* GamebaseCallback:ゲーム告知が正常に終了したり、エラーで表示できなかったときにユーザーにコールバックでお知らせします。


**API**

```java
+ (void)Gamebase.GameNotice.openGameNotices(@NonNull Activity activity,
                                            @Nullable GamebaseCallback onCloseCallback);
+ (void)Gamebase.GameNotice.openGameNotices(@NonNull Activity activity,
                                            @Nullable GameNoticeConfiguration configuration,
                                            @Nullable GamebaseCallback onCloseCallback);
```

**ErrorCode**

| Error | Error Code | Description |
| ---- | ------- | ----------- |
| - | 0 | 成功 |
| NOT\_INITIALIZED | 1 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_GAME\_NOTICE\_FAIL\_INVALID\_URL | 6941 | ゲーム告知URLの作成に失敗しました。 |
| UI\_GAME\_NOTICE\_FAIL\_ANDROID\_DUPLICATED\_VIEW | 6942 | ゲーム告知ポップアップを終了する前にゲーム告知を再度呼び出しました。 |
| WEBVIEW\_TIMEOUT | 7002 | Webビュー表示時間が超過しました。(10秒) |
| WEBVIEW\_HTTP\_ERROR | 7003 | Webビュー内部でHTTPエラーが発生しました。 |
| WEBVIEW\_UNKNOWN\_ERROR | 7999 | 不明なWebビューエラーが発生しました。 |

**Example**

```java
Gamebase.GameNotice.openGameNotice(activity, (GamebaseCallback) exception -> {
    if (Gamebase.isSuccess(exception)) {
        // Game Notice was opened and closed successfully.
    } else {
        // Game Notice did not opened with error.
    }
});
```

### Custom GameNotice

ユーザー設定ゲーム告知を表示します。
GameNoticeConfigurationで表示設定を変更できます。

**Example**

```java
GameNoticeConfiguration configuration = GameNoticeConfiguration.newBuilder()
        .setBackgroundColor("#80FFFF00")
        .build();
Gamebase.GameNotice.openGameNotice(
        activity,
        configuration,
        (GamebaseCallback) exception -> {
            if (Gamebase.isSuccess(exception)) {
                // Game Notice was opened and closed successfully.
            } else {
                // Game Notice did not opened with error.
            }
        });
```

#### GameNoticeConfiguration

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | GameNoticeConfiguration.BuilderオブジェクトはnewBuilder()関数を使用して作成できます。 |
| build() | **M** | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | ゲーム告知の背景色です。<br>色はARGB順序です。<br>Stringはandroid.graphics.Color.parseColor(String) APIで変換した値を使用します。<br>**default**: #CC000000 |

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/DevelopersGuide/imageNotice-guide-landscape-ja_v3.png)

### Show ImageNotices

イメージ告知を画面に表示します。

#### Requiredパラメータ
* Activity ：イメージ告知が表示されるActivityです。

#### Optionalパラメータ
* ImageNoticeConfiguration：イメージ告知設定を変更できます。
* GamebaseCallback ：イメージ告知が全て終了する時、ユーザーにコールバックで伝えます。
* GamebaseDataCallback ：イメージを押した時、コンソールに登録したpayloadコールバックで伝えます。

**API**

```java
+ (void)Gamebase.ImageNotice.showImageNotices(@NonNull Activity activity,
                                              @Nullable GamebaseCallback onCloseCallback);
+ (void)Gamebase.ImageNotice.showImageNotices(@NonNull Activity activity,
                                              @Nullable ImageNoticeConfiguration configuration,
                                              @Nullable GamebaseCallback onCloseCallback,
                                              @Nullable GamebaseDataCallback<String> onEvent);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase.initializeが呼び出されませんでした。 |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知ポップアップウィンドウの表示中にタイムアウトが発生してすべてのポップアップを強制終了します。 |
| UI\_IMAGE\_NOTICE\_NOT\_SUPPORTED\_OS | 6902 | ローリングタイプの場合、 API 19以下の端末では画像イメージ告知をサポートしません。 |
| SERVER\_INVALID\_RESPONSE | 8003 | サーバーから無効なレスポンスが返されました。|
| WEBVIEW\_HTTP\_ERROR | 7003 | ローリングタイプ画像告知Webビューを開く最中にHTTPエラーが発生しました。 |

**Example**

```java
Gamebase.ImageNotice.showImageNotices(getActivity(), null,
    new GamebaseCallback(){
        @Override
        public void onCallback(GamebaseException exception) {
        	// Called when the entire imageNotice is closed.
            ...
        }
    },
    new GamebaseDataCallback<String>() {
        @Override
        public void onCallback(String payload, GamebaseException exception) {
        	// Called when custom event occurred.
            ...
        }
    });
```

### Custom ImageNotices

ユーザー設定イメージ告知を画面に表示します。
ImageNoticeConfigurationでユーザー設定イメージ告知を作成できます。

**Example**

```java
ImageNoticeConfiguration configuration = ImageNoticeConfiguration.newBuilder()
		.setBackgroundColor("#FFFF0000")		// Red
        .setTimeout(10000L)						// 10000ms == 10s
        .enableAutoCloseByCustomScheme(false)
        .build();
Gamebase.ImageNotice.showImageNotices(getActivity(), configuration, null, null);
```

#### ImageNoticeConfiguration

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | ImageNoticeConfiguration.BuilderオブジェクトはnewBuilder()関数で作成できます。 |
| build() | **M** | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | イメージ告知の背景色。<br>Stringはandroid.graphics.Color.parseColor(String) APIで変換した値を使用します。<br>**default** : #80000000 |
| setTimeout(long timeoutMs) | O | イメージ告知の最大ローディング時間(単位：millisecond)<br>**default** ： 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | カスタムスキームイベントが発生した時、メージ告知を強制終了するかどうかを決定します。<br>**default** : true |


### Close ImageNotices

closeImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

**API**

```java
+ (void)Gamebase.ImageNotice.closeImageNotices(@NonNull Activity activity);
```

## Terms

Gamebaseコンソールに設定した約款を表示します。

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

showTermsView APIは、Webビューで約款ウィンドウを表示します。
GameのUIに合った約款ウィンドウを直接作成したい場合は、queryTerms APIを呼び出して、 Gamebaseコンソールに設定した約款項目を呼び出すことができます。
ユーザーが約款に同意した場合、項目別の同意有無をupdateTerms APIを介してGamebaseサーバーへ送信してください。

### showTermsView

約款ウィンドウを画面に表示します。
ユーザーが約款に同意した場合、同意の有無をサーバーに登録します。
約款に同意した場合、showTermsView APIを再度呼び出しても約款ウィンドウが表示されず、すぐに成功コールバックがリターンされます。
ただし、Gamebaseコンソールで「約款の再同意」項目を**必要**に変更した場合は、ユーザーが再度約款に同意するまでは約款ウィンドウが表示されます。

#### Requiredパラメータ

* Activity：約款ウィンドウが表示されるActivityです。
 
#### Optionalパラメータ

* GamebaseTermsConfiguration : GamebaseTermsConfigurationオブジェクトを介して強制的に約款同意ウィンドウを表示するかどうかなどの設定を変更できます。
* GamebaseDataCallback：約款に同意した後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで返されたGamebaseDataContainerオブジェクトはGamebaseShowTermsViewResultに変換して追加情報を確認できます。

**API**

```java
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseTermsConfiguration configuration,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
```

**GamebaseTermsConfiguration**

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | GamebaseTermsConfiguration.BuilderオブジェクトはnewBuilder()関数で作成できます。 |
| build() | **M** | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setForceShow(boolean forceShow) | O | 約款に同意した場合、showTermsView APIを再度呼び出しても約款ウィンドウが表示されませんが、これを無視して強制的に約款ウィンドウを表示します。<br>**default** : false |
| enableFixedFontSize(boolean enable) | O | システム文字サイズを無視し、固定されたサイズで約款を表示します。<br>**default** : false |

**GamebaseShowTermsViewResult**

| Field | Type | Nullable / NonNull | Description |
| --- | --- | --- | --- |
| isTermsUIOpened | boolean | NonNull | **true**：約款ウィンドウが表示され、ユーザーが同意して約款ウィンドウが終了しました。<br>**false**：すでに約款に同意していて約款ウィンドウが表示されずに約款ウィンドウが終了しました。 |
| pushConfiguration | PushConfiguration | Nullable | isTermsUIOpenedが**true**で、約款にプッシュ受信同意有無を追加した場合、pushConfigurationは常に有効なオブジェクトを持ちます。<br>そうでない場合は**null**です。<br>pushConfigurationが有効なとき、pushConfiguration.pushEnabled値は常に**true**です。 |

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| LAUNCHING\_SERVER\_ERROR | 2001 | ローンチサーバーからダウンロードした項目に約款関連内容がない場合に発生するエラーです。<br/>正常な状況ではないため、Gamebase担当者にお問い合わせください。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | 約款Webビューが終了していないのに再び呼び出されました。 |
| WEBVIEW\_TIMEOUT | 7002 | 約款Webビューの表示中にタイムアウトが発生しました。 |
| WEBVIEW\_HTTP\_ERROR | 7003 | 約款Webビューのオープン中にHTTPエラーが発生しました。 |

**Example**

```java
static PushConfiguration savedPushConfiguration = null;
final GamebaseTermsConfiguration configuration = GamebaseTermsConfiguration.newBuilder()
        .setForceShow(true)
        .build();
Gamebase.Terms.showTermsView(activity, configuration, (container, exception) -> {
    if (Gamebase.isSuccess(exception)) {
        // Save the PushConfiguration and use it for Gamebase.Push.registerPush()
        // after Gamebase.login().
        GamebaseShowTermsViewResult termsViewResult = GamebaseShowTermsViewResult.from(container);
        if (termsViewResult != null) {
            savedPushConfiguration = termsViewResult.pushConfiguration;
        }
    } else {
        new Thread(() -> {
            // Wait for a while and try again.
            try { Thread.sleep(2000); }
            catch (Exception ignored) {}
            showTermsView(activity, callback);
        }).start();
    }
});
public void afterLogin(Activity activity) {
    // Call registerPush with saved PushConfiguration.
    if (savedPushConfiguration != null) {
        Gamebase.Push.registerPush(activity, savedPushConfiguration, exception -> {...});
    }
}
```

### queryTerms

Gamebaseは単純な形式のWebビューで約款を表示します。
ゲームUIに合った約款を直接作成したい場合は、queryTerms APIを呼び出してGamebaseコンソールに設定した約款情報をダウンロードして活用できます。

'「選択」約款項目はログイン後に呼び出すと、同意の有無も一緒に知ることができます。ただし、「必須」項目の同意有無は常にfalseで返されます。

> <font color="red">[注意]</font><br/>
>
> * GamebaseTermsContentDetail.getRequired()がtrueの必須項目は同意有無をGamebaseサーバーに保存しないため、agreed値は常にfalseが返されます。
>     * 約款必須項目に同意していない場合、ゲーム進行やゲームログインができないため、約款ポップアップが閉じていてログインしている状態であれば、当然、約款必須項目に同意したのと同じです。そのため、ログインしたユーザーはすでに必須項目に全て同意した状態なので、同意の有無を保存する必要はありません。
> * プッシュ受信同意有無もGamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * プッシュ受信同意有無は、Gamebase.Push.queryTokenInfo APIを介して照会してください。
> * コンソールで「基本約款設定」をしない場合、約款言語と異なる国コードで設定された端末からqueryTerms APIを呼び出した場合、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生します。
>     * コンソールで「基本約款設定」を行ったり、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生した時は、約款を表示しないように処理してください。

#### Requiredパラメータ

* Activity ： API呼び出し時点の最上位Activityです。
* GamebaseDataCallback ：API呼び出し結果をユーザーにコールバックで伝えます。コールバックで返されたGamebaseQueryTermsResultでコンソールに設定された約款情報を取得できます。

**API**

```java
+ (void)Gamebase.Terms.queryTerms(@NonNull Activity activity,
                                  @NonNull GamebaseDataCallback<GamebaseQueryTermsResult> callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 端末国コードに合った約款情報がコンソールに登録されていません。 |

**Example**

```java
Gamebase.Terms.queryTerms(activity, new GamebaseDataCallback<GamebaseQueryTermsResult>() {
    @Override
    public void onCallback(GamebaseQueryTermsResult result, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            final int termsSeq = result.getTermsSeq();
            final String termsVersion = result.getTermsVersion();
            final String termsCountryType = result.getTermsCountryType();
            final List<GamebaseTermsContentDetail> contents = result.getContents();
        } else if (exception.getCode() == GamebaseError.UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY) {
            // Another country device.
            // Pass the 'terms and conditions' step.
        } else {
            // Failed.
        }
    }
});
```

#### GamebaseQueryTermsResult

| API            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| getTermsSeq             | int                             | 約款全体KEY。<br/>updateTerms APIを呼び出す時に必要な値です。          |
| getTermsVersion         | String                          | 約款バージョン。<br/>updateTerms APIを呼び出す時に必要な値です。              |
| getTermsCountryType     | String                          | 約款タイプ。<br/> - KOREAN：韓国の約款 <br/> - GDPR ：ヨーロッパの約款 <br/> - ETC：その他の国 |
| getContents             | List<GamebaseTermsContentDetail> | 約款項目別詳細情報 |

#### GamebaseTermsContentDetail

| API            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| getTermsContentSeq      | int                   | 約款項目KEY         | 
| getName                 | String                | 約款項目名      |
| getRequired             | boolean                  | 必須同意有無       |
| getAgreePush            | String                | 広告性プッシュ同意有無。<br/> - NONE ：同意しない <br/> - ALL ：全て同意 <br/> - DAY ：1週間プッシュ同意<br/> - NIGHT ：夜間プッシュ同意       |
| getAgreed               | boolean                  | 該当約款項目のユーザー同意有無。<br/> - ログイン前は常にfalse。<br/> - プッシュ項目は常にfalse。 |
| getNode1DepthPosition   | int                   | 1段階項目表示順序。           |
| getNode2DepthPosition   | int                   | 2段階項目表示順序。<br/> ない場合は-1           |
| getDetailPageUrl        | String                | 約款詳細表示URL。<br/> ない場合はnull。 |

### updateTerms

queryTerms APIでダウンロードした約款情報でUIを直接作った場合は、
ゲームユーザーが約款に同意した内容をupdateTerms APIを介してGamebaseサーバーへ送信してください。

任意約款同意をキャンセルするように、約款に同意した内容を変更する目的で活用できます。

> <font color="red">[注意]</font><br/>
>
> プッシュ受信同意有無は、Gamebaseサーバーに保存されません。
> プッシュ受信同意有無は、**ログイン後に**Gamebase.Push.registerPush APIを呼び出して保存してください。

#### Requiredパラメータ

* Activity ：API呼び出し時点の最上位Activityです。
* GamebaseUpdateTermsConfiguration：サーバーに登録するユーザーの任意約款情報です。

#### Optionalパラメータ

* GamebaseCallback：任意約款情報をサーバーに登録した後、ユーザーにコールバックで伝えます。

**API**

```java
+ (void)Gamebase.Terms.updateTerms(@NonNull Activity activity,
                                   @NonNull GamebaseUpdateTermsConfiguration configuration,
                                   @Nullable GamebaseCallback callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 登録されていない約款Seq値を設定しました。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |

**Example**

```java
Gamebase.Terms.queryTerms(activity, new GamebaseDataCallback<GamebaseQueryTermsResult>() {
    @Override
    public void onCallback(GamebaseQueryTermsResult result, GamebaseException queryTermsException) {
        if (Gamebase.isSuccess(queryTermsException)) {
            // Succeeded to query terms.
            final int termsSeq = result.getTermsSeq();
            final String termsVersion = result.getTermsVersion();
            final List<GamebaseTermsContent> contents = new ArrayList<>();
            for (GamebaseTermsContentDetail detail : result.getContents()) {
                GamebaseTermsContent content = GamebaseTermsContent.from(detail);
                // TODO: Change agree value what you want.
                content.setAgreed(agreeOrNot);
                contents.add(content);
            }

            final GamebaseUpdateTermsConfiguration configuration =
                    GamebaseUpdateTermsConfiguration.newBuilder(termsSeq, termsVersion, contents)
                            .build();
            Gamebase.Terms.updateTerms(activity, configuration, new GamebaseCallback() {
                @Override
                public void onCallback(GamebaseException updateTermsException) {
                    if (Gamebase.isSuccess(updateTermsException)) {
                        // Succeeded to update terms.
                    } else {
                        // Failed to update terms.
                    }
                }
            });
        } else {
            // Failed to query terms.
        }
    }
});
```

#### GamebaseUpdateTermsConfiguration

**Builder**

| API                  | Description         |
| -------------------- | ------------------- |
| newBuilder(String termsVersion, int termsSeq, List<GamebaseTermsContent> contents) | Configurationオブジェクトを作成するためのBuilderを作成します。 |
| build() | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |

| Parameter            | Mandatory(M) / Optional(O) | Type                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsSeq             | **M**                      | int                       | 約款全体KEY。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。             |
| termsVersion         | **M**                      | String                    | 約款のバージョン。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。   |
| contents             | **M**                      | List<GamebaseTermsContent> | 任意約款ユーザー同意情報 |

#### GamebaseTermsContent

**Constructor**

| API                  | Description         |
| -------------------- | ------------------- |
| GamebaseTermsContent(int termsContentSeq, boolean agreed) | GamebaseTermsContent作成者です。 |
| from(GamebaseTermsContentDetail) | GamebaseTermsContentDetailオブジェクトからGamebaseTermsContentオブジェクトを作成するFactory Methodです。 |
| setAgreed(boolean) | 該当オブジェクトのagreed値を変更します。 |

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 任意約款項目KEY      |
| agreed               | **M**                      | boolean            | 任意約款項目同意有無 |

### isShowingTermsView

現在約款ウィンドウが表示されている状態かどうかを伝えます。

**API**

```java
+ (boolean)Gamebase.Terms.isShowingTermsView();
```

## WebView

Gamebaseでは、基本的なWebViewに対応しています。


### Show WebView

WebViewを表示します。

##### Required パラメーター
* activity : WebViewが表示されるActivityです。
* url : パラメーターで送信されるurlは、有効な値でなければなりません。

##### Optionalパラメーター
* configuration : GamebaseWebViewConfigurationでWebViewのレイアウトを変更することができます。
* GamebaseCallback : WebViewが終了する際に、ユーザーにコールバックで知らせます。
* schemeList : ユーザーの求めるカスタムSchemeのリストを指定します。
* GamebaseDataCallback : schemeListに指定したカスタムスキームを含むurlをコールバックで知らせます。

**API**

```java
+ (void)Gamebase.WebView.showWebView(Activity activity,
                String urlString,
                GamebaseWebViewConfiguration configuration,
                GamebaseCallback onCloseCallback,
                List<String> schemeList,
                GamebaseDataCallback<String> onEvent);
```

**Example**

```java
Gamebase.WebView.showWebView(activity, "https://www.toast.com");
```

![Webview Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

#### Custom WebView

ユーザーが指定したWebViewを表示します。<br/>
GamebaseWebViewConfigurationでユーザーが指定したWebViewを作成することができます。

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setTitleText("title")                              // WebViewタイトルを設定
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // WebViewのスクリーン方向を設定            
            .setNavigationBarColor(Color.RED)                   // ナビゲーションバーの色設定

            .setNavigationBarTitleColor(Color.BLACK)            // ナビゲーションバータイトル色設定

            .setNavigationBarIconTintColor(Color.BLACK)         // ナビゲーションバーアイコンティントカラー設定

            .setNavigationBarHeight(40)                         // ナビゲーションバーの高さ設定
            
            .setBackButtonVisible(true)                         // 戻るボタンを有効にするかどうかを設定
            .setBackButtonImageResource(R.id.back_button)       // 戻るボタンの画像を設定
            .setCloseButtonImageResource(R.id.close_button)     // 閉じるボタンの画像を設定
            .build();
GamebaseWebView.showWebView(activity, "https://www.toast.com", configuration);
```

#### Custom Scheme

Gamebase WebViewでローディングしたWebページ内に、スキームで特定機能を使用したり、Webページの内容を変更できます。

##### Predefined Custom Scheme

Gamebaseで指定しておいたスキームです。

| スキーマ         | 用途                                |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | WebViewを閉じる                       |
| gamebase://goBack    | WebView前に戻る                   |
| gamebase://getUserId | 現在ログインしているユーザーのIDを表示 |
| gamebase://showwebview?link={URLEncodedURL} | linkパラメータのURLをWebViewで開く。<br>URLEncodedURL ：WebViewで開くURL。<br>URLのデコード必要。 |
| gamebase://openbrowser?link={URLEncodedURL} | linkパラメータのURLを外部ブラウザで開く。<br>URLEncodedURL：外部ブラウザで開くURL。<br>URLデコード必要。 |

#### User Custom Scheme

Gamebaseにスキーム名とブロックを指定し、自由に機能を追加できます。

```java
GamebaseWebViewConfiguration configuration = new GamebaseWebViewConfiguration.Builder()
        .setTitleText(title)
        .build();
List<String> schemeList = new ArrayList<>();
schemeList.add("mygame://test");
schemeList.add("mygame://opensomebrowser");
schemeList.add("closemywebview://");
showWebView(activity, urlString, configuration,
        new GamebaseCallback() {
            @Override
            public void onCallback(GamebaseException exception) {
                // When closed WebView, this callback will be called.
            }
        },
        schemeList,
        new GamebaseDataCallback<String>() {
            @Override
            public void onCallback(String fullUrl, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    if (fullUrl.contains("mygame://test")) {
                        // Do something.
                    } else if (fullUrl.contains("mygame://opensomebrowser")) {
                        Gamebase.WebView.openWebBrowser(someUrl);
                    } else if (fullUrl.contains("closemywebview://")) {
                        // We will close webview.
                        Gamebase.WebView.closeWebView(activity);
                    }
                } else {
                    // Something went wrong.
                }
            }
        });
```

#### GamebaseWebViewConfiguration

| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setTitleText(String title)               | title                               | WebViewのタイトル         |
| setScreenOrientation(int orientation)    | ScreenOrientation.PORTRAIT          | 縦モード          |
|                                          | ScreenOrientation.LANDSCAPE         | 横モード          |
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | 横モードを180度回転 |
| setNavigationBarVisible(boolean enable)  | true or false                       | ナビゲーションバーの有効または無効<br>**default**: true |
| setNavigationBarColor(int color)         | Color.argb(a, r, g, b)              | ナビゲーションバーの色<br>**default**:#125DE6  |
| setNavigationBarTitleColor(int color)    | Color.argb(a, r, g, b)              | ナビゲーションバータイトル色<br>**default**: Color.WHITE  |
| setNavigationBarIconTintColor(int color) | Color.argb(a, r, g, b)              | ナビゲーションバーアイコンティントカラー<br>**default**:ティントを設定しない  |
| setNavigationBarHeight(int height)       | height                              | ナビゲーションバーの高さ     |
| setBackButtonVisible(boolean visible)    | true or false                       | 戻るボタンの有効または無効<br>**default**: true |
| setBackButtonImageResource(int resourceId) | ID of resource                      | 戻るボタンの画像       |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | 閉じるボタンの画像      |
| enableAutoCloseByCustomScheme(boolean enable) | true or false | カスタムScheme動作時、自動的にWebビュー終了。<br>**default**: true |
| enableFixedFontSize(boolean enable)      | true or false | システム文字サイズを無視し、固定されたサイズでWebビューを表示。<br>**default**: false |
| setRenderOutsideSafeArea(boolean render) | true or false | safe areaを無視し、cutout領域にもrender。<br>**default**: false |
| setCutoutAreaColor(int color) | Color.argb(a, r, b, b) | SafeArea外のCutout領域の背景色 |

### Close WebView
次のAPIを通じて、表示されているWebViewを閉じることができます。

**API**

```java
+ (void)Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

次のAPIを通じて、外部ブラウザを開くことができます。パラメーターで送信されるURLは、有効な値でなければなりません。

**API**

```java
+ (void)Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

システム通知を表示することができます。<br/>

### Simple Alert Dialog

タイトルとメッセージだけを入力して簡単に通知のダイアログボックスを表示することができます。

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

通知のダイアログボックスを表示した後、処理結果をコールバックで受け取りたい場合は次のAPIを使用します。

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity,
                            String title,                                   // タイトルテキスト。
                            String messsage,                                // メッセージテキスト。
                            String okButtonText,                            // ポジティブボタンテキスト。
                            DialogInterface.OnClickListener clickListener); // ポジティブボタンを押したときに呼び出されるListener。
```

## Toast

次のAPIを使用して簡単に[Android トースト(toast)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html)のメッセージを表示することができます。<br/>
メッセージを表示する時間タイプのパラメーターはint形式であり、Android SDK NotificationManagerServiceクラスの定義に基づき、下の図の時間の間、表示されます。

| 時間タイプ(int)         | 表示時間                     |
| ------------------ | ------------------------- |
| Toast.LENGTH_SHORT | 2秒                        |
| Toast.LENGTH_LONG  | 3.5秒                      |
| 0                  | Toast.LENGTH_SHORT => 2秒  |
| 1                  | Toast.LENGTH_LONG => 3.5秒 |
| 残りのすべての値           | Toast.LENGTH_SHORT => 2秒  |

**API**

```java
+ (void)Gamebase.Util.showToast(Activity activity,
                        String message,     // 表示するメッセージテキスト
                        int duration);      // メッセージを表示する時間タイプ (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

## Custom Maintenance Page

メンテナンス状態で｢詳細確認」をクリックすると表示されるメンテナンスページを変更することができます。

*ユーザーの指定するウェブページをメンテナンスページに登録
    * AndroidManifest.xmlにcom.gamebase.maintenance.detail.urlをキー値とするmeta-dataを設定します。
    * android:valueの値に.htmlファイルまたはURLを入力することができます。

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error                                             | Error Code | Description                                                                                 |
|---------------------------------------------------|------------|---------------------------------------------------------------------------------------------|
| NOT\_INITIALIZED                                  | 1          | Gamebase.initializeが呼び出されていません。                                                            |
| LAUNCHING\_SERVER\_ERROR                          | 2001       | ローンチサーバーから渡された項目に約款関連内容がない場合に発生するエラーです。<br/>正常な状況ではないので、 Gamebase担当者にお問い合わせください。 |
| UI\_IMAGE\_NOTICE\_TIMEOUT                        | 6901       | 画像告知ポップアップウィンドウ表示中にタイムアウトし、全てのポップアップウィンドウを強制終了します。 |
| UI\_IMAGE\_NOTICE\_NOT\_SUPPORTED\_OS             | 6902       | ローリングタイプの場合、API 19以下の端末では画像告知をサポートしません。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE                | 6921       | 約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY       | 6922       | 端末国家コードに合った約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_UNREGISTERED\_SEQ                      | 6923       | 登録されていない約款Seq値を設定しました。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR           | 6924       | Terms API呼び出しがまだ完了していません。<br/>しばらくしてからもう一度お試しください。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW              | 6925       | 約款Webビューがまだ終了していない状態で再度呼び出されました。 |
| UI\_GAME\_NOTICE\_FAIL\_INVALID\_URL              | 6941       | ゲーム告知URLの作成に失敗しました。 |
| UI\_GAME\_NOTICE\_FAIL\_ANDROID\_DUPLICATED\_VIEW | 6942       | ゲーム告知ポップアップを終了する前に再度ゲーム告知を呼び出しました。 |
| UI\_UNKNOWN\_ERROR                                | 6999       | 不明なエラーです(定義されていないエラーです)。 |
| WEBVIEW\_TIMEOUT                                  | 7002       | Webビュー表示時間が超過しました。(10秒) |
| WEBVIEW\_HTTP\_ERROR                              | 7003       | Webビュー内部でHTTPエラーが発生しました。 |
| WEBVIEW\_UNKNOWN\_ERROR                           | 7999       | 不明なWebビューエラーが発生しました。 |
| SERVER\_INVALID\_RESPONSE                         | 8003       | サーバーが有効ではないレスポンスを返しました。 |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
