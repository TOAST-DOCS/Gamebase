## Game > Gamebase > Android SDK ご利用ガイド > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

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
| newBuilder() | **M** | ImageNoticeConfigurationオブジェクトはnewBuilder()関数で作成できます。 |
| build() | **M** | 設定を終えたBuilderをConfigurationオブジェクトに変換します。 |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | イメージ告知の背景色。<br>Stringはandroid.graphics.Color.parseColor(String) APIで変換した値を使用します。<br>**default** : #80000000 |
| setTimeout(long timeoutMs) | O | イメージ告知の最大ローディング時間(単位：millisecond)<br>**default** ： 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | custom schemeイベントが発生した時、メージ告知を強制終了するかどうかを決定します。<br>**default** : true |


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

> <font color="red">[注意]</font><br/>
>
> * 約款にプッシュ受信同意有無を追加した場合は、GamebaseDataContainerからPushConfigurationを作成できます。
> * PushConfigurationは、約款ウィンドウが表示されていない場合はnullです。(約款ウィンドウが表示されていれば常に有効なオブジェクトが返されます。)
> * PushConfiguration.pushEnabled値は常にtrueです。
> * PushConfigurationがnullではない場合、**ログイン後に**Gamebase.Push.registerPush APIを呼び出してください。

#### Requiredパラメータ

* Activity：約款ウィンドウが表示されるActivityです。
 
#### Optionalパラメータ

* GamebaseDataCallback：約款に同意した後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで返されたGamebaseDataContainerオブジェクトはPushConfigurationに変換してログイン後、Gamebase.Push.registerPush APIに使用できます。

**API**

```java
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseTermsConfiguration configuration,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
```

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
        savedPushConfiguration = PushConfiguration.from(container);
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

ログイン後に呼び出す場合は、ゲームユーザーが約款に同意したかどうかも一緒に確認できます。

> <font color="red">[注意]</font><br/>
>
> * GamebaseTermsContentDetail.getRequired()がtrueの必須項目は、Gamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * 必須項目は、常にtrueで保存されるしかないので、保存する意味がないためです。
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
* GamebaseDataCallback : schemeListに指定したカスタムSchemeを含むurlをコールバックで知らせます。

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
Gamebase.WebView.showWebView(activity, "http://www.toast.com");
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
            .setNavigationBarColor(Color.RED)                   // ナビゲーションバーの色を設定
            .setNavigationBarHeight(40)                         // ナビゲーションバーの高さを設定
            .setBackButtonVisible(true)                         // 戻るボタンを有効にするかどうかを設定
            .setBackButtonImageResource(R.id.back_button)       // 戻るボタンの画像を設定
            .setCloseButtonImageResource(R.id.close_button)     // 閉じるボタンの画像を設定
            .build();
GamebaseWebView.showWebView(activity, "http://www.toast.com", configuration);
```

#### Custom Scheme

Gamebase WebViewでローディングしたWebページ内に、スキーマ(scheme)で特定機能を使用したり、Webページの内容を変更できます。

##### Predefined Custom Scheme

Gamebaseで指定しておいたスキーマです。

| スキーマ         | 用途                                |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | WebViewを閉じる                       |
| gamebase://goBack    | WebView前に戻る                   |
| gamebase://getUserId | 現在ログインしているユーザーのIDを表示 |
| gamebase://showwebview?link={URLEncodedURL} | linkパラメータのURLをWebViewで開く。<br>URLEncodedURL ：WebViewで開くURL。<br>URLのデコード必要。 |
| gamebase://openbrowser?link={URLEncodedURL} | linkパラメータのURLを外部ブラウザで開く。<br>URLEncodedURL：外部ブラウザで開くURL。<br>URLデコード必要。 |

#### User Custom Scheme

Gamebaseにスキーマ名とブロックを指定し、自由に機能を追加できます。

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
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | ナビゲーションバーの色     |
| setBackButtonVisible(boolean visible)    | true or false                       | 戻るボタンの有効化または無効化 |
| setNavigationBarHeight(int height)       | height                              | ナビゲーションバーの高さ     |
| setBackButtonImageResource(int resourceId) | ID of resource                      | 戻るボタンの画像       |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | 閉じるボタンの画像      |


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

| Error              | Error Code | Description                  |
| ------------------ | ---------- | ---------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
