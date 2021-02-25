## Game > Gamebase > Android SDK使用指南 > UI

## WebView

Gamebase支持基本的WebView。


### Show WebView

显示WebView。<br/>

##### Required 参数
* activity：显示WebView的活动。
* url：作为参数发送的url必须是有效值。

##### 可选参数
* configuration：可以使用GamebaseWebViewConfiguration更改WebView的布局。
* GamebaseCallback：WebView关闭时通过回调通知用户。
* schemeList：指定用户想要接收的自定义SchemeList。
* GamebaseDataCallback：用schemeList指定的包含自定义Scheme的url，作为回调通知。

**API**

```java
+ (void)Gamebase.WebView.showWebView(Activity activity, 
                String urlString, 
                GamebaseWebViewConfiguration configuration,
                GamebaseCallback onCloseCallback,
                List<String> schemeList,
                GamebaseDataCallback<String> onEvent);
```

**示例**

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


#### 自定义WebView

显示自定义WebView。<br/>
可以用GamebaseWebViewConfiguration设置自定义WebView。

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setStyle(GamebaseWebViewStyle.BROWSER)
            .setTitleText("title")                              // 设置WebView标题
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // 设置WebView页面方向
            .setNavigationBarColor(Color.RED)                   // 设置导航栏的颜色
            .setNavigationBarHeight(40)                         // 设置导航栏的高度
            .setBackButtonVisible(true)                         // 返回按钮有效或无效
            .setBackButtonImageResource(R.id.back_button)       // 设置返回按钮图标
            .setCloseButtonImageResource(R.id.close_button)     // 设置关闭按钮图标
            .build();
GamebaseWebView.showWebView(MainActivity.this, "http://www.toast.com", configuration);
```
| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setStyle(int style)                      | GamebaseWebViewStyle.BROWSER        | 浏览器风格的WebView   |
|                                          | GamebaseWebViewStyle.POPUP          | 弹出窗口风格的WebView     |
| setTitleText(String title)               | title                               | WebView标题        |
| setScreenOrientation(int orientation)    | ScreenOrientation.PORTRAIT          | 纵向模式         |
|                                          | ScreenOrientation.LANDSCAPE         | 横向模式         |
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | 将横向模式旋转180度 |
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | 导航栏颜色  |
| setBackButtonVisible(boolean visible)    | true or false                       | 返回按钮有效或无效 |
| setNavigationBarHeight(int height)       | height                              | 导航栏高度    |
| setBackButtonImageResource(int resourceId) | ID of resource                      | 返回按钮的图标      |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | 关闭按钮的图标      |


### Close WebView
通过以下API，可以关闭正在显示的WebView。

**API**

```java
+ (void)Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

可以使用以下API打开外部浏览器。作为参数传递的URL必须是有效值。

**API**

```java
+ (void)Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

可以显示系统提醒。<br/>

### Simple Alert Dialog

只需要输入标题和消息即可显示提醒对话框。

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

如果要在显示通知对话框后接收回调，请使用以下API。

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity,
                            String title,
                            String messsage,
                            String okButtonText,
                            DialogInterface.OnClickListener clickListener);
```

## Toast

可以使用以下API轻松显示 [Android 토스트(toast)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) 消息。<br/>
用于显示信息的时间类型参数是int型，并根据Android SDK NotificationManagerService类的定义，可显示的时间如下表。

| 时间类型(int)         | 显示时间                     |
| ------------------ | ------------------------- |
| Toast.LENGTH_SHORT | 2秒                        |
| Toast.LENGTH_LONG  | 3.5秒                      |
| 0                  | Toast.LENGTH_SHORT => 2秒  |
| 1                  | Toast.LENGTH_LONG => 3.5秒 |
| 全部剩余值           | Toast.LENGTH_SHORT => 2秒  |

**API**

```java
+ (void)Gamebase.Util.showToast(Activity activity,
                        String message,
                        int duration);    // 显示信息的时间类型 (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

## Custom Maintenance Page

点击维护状态中的“详细信息”，可以更改维护页面。

* 将自定义的页面设置为维护页面。
    * 在AndroidManifest.xml中， 以com.gamebase.maintenance.detail.url为键值，设置元数据。
    * 可以输入.html文件或URL作为android：value值。

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | 描述                  |
| ------------------ | ---------- | ---------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误)。 |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
