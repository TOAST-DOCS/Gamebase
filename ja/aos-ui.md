## Game > Gamebase > Android SDK ご利用ガイド > UI

## WebView

Gamebaseでは、基本的なWebViewに対応しています。


### Show WebView

WebViewを表示します。<br/>

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
Gamebase.WebView.showWebView(activity, "http://www.toast.com",
    new GamebaseWebViewConfiguration.Builder().build(),
    new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            Logger.d(TAG, "WebView is closed.");
        }
    }, schemeList,
    new GamebaseDataCallback<String>() {
        @Override
        public void onCallback(String fullUrl, GamebaseException exception) {
            Logger.d(TAG, "WebView Event occured. Event Url :" + fullUrl);
        }
    }
 );
```

![Webview Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)


#### Custom WebView

ユーザーが指定したWebViewを表示します。<br/>
GamebaseWebViewConfigurationでユーザーが指定したWebViewを作成することができます。

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setStyle(GamebaseWebViewStyle.BROWSER)
            .setTitleText("title")                              // WebViewタイトルを設定
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // WebViewのスクリーン方向を設定
            .setNavigationBarColor(Color.RED)                   // ナビゲーションバーの色を設定
            .setNavigationBarHeight(40)                         // ナビゲーションバーの高さを設定
            .setBackButtonVisible(true)                         // 戻るボタンを有効にするかどうかを設定
            .setBackButtonImageResource(R.id.back_button)       // 戻るボタンの画像を設定
            .setCloseButtonImageResource(R.id.close_button)     // 閉じるボタンの画像を設定
            .build();
GamebaseWebView.showWebView(MainActivity.this, "http://www.toast.com", configuration);
```
| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setStyle(int style)                      | GamebaseWebViewStyle.BROWSER        | ブラウザスタイルのWebView   |
|                                          | GamebaseWebViewStyle.POPUP          | ポップアップスタイルのWebView     |
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
Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

次のAPIを通じて、外部ブラウザを開くことができます。パラメーターで送信されるURLは、有効な値でなければなりません。

**API**

```java
Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

システム通知を表示することができます。<br/>

### Simple Alert Dialog

タイトルとメッセージだけを入力して簡単に通知のダイアログボックスを表示することができます。

**API**

```java
Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

通知のダイアログボックスを表示した後、処理結果をコールバックで受け取りたい場合は次のAPIを使用します。

**API**

```java
Gamebase.Util.showAlertDialog(Activity activity,
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
Gamebase.Util.showToast(Activity activity,
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
