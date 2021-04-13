## Game > Gamebase > Android SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户发送推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

将图片通知显示在页面上。

#### Required参数
* Activity : 为显示图片通知的活动。

#### Optional参数
* ImageNoticeConfiguration : 使用ImageNoticeConfiguration可更改图片通知的布局。
* GamebaseCallback : 关闭图片通知时通过回调通知用户。
* GamebaseDataCallback : 点击图片时, 将设置在控制台的payload作为回调通知。

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

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | 未调用Gamebase.initialize |
| UI\_IMAGE\_NOTICE\_TIMEOUT(6901) | 显示图片通知弹窗时，因出现超时错误强制关闭所有弹窗。|

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

将自定义图片通知显示在页面上。
使用ImageNoticeConfiguration可创建自定义图片通知。

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
| newBuilder() | **M** | 使用newBuilder()函数可以生成ImageNoticeConfiguration对象。 |
| build() | **M** | 将设置完的Builder转换为Configuration对象。 |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | ImageNotice背景颜色<br>使用String参数时，调用转换为android.graphics.Color.parseColor(String) API的值。<br>**default** : #80000000 |
| setTimeout(long timeoutMs) | O | ImageNotice最大加载时间(单位 : millisecond)<br>**default** : 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | 出现custom scheme event时，判断是否应强制关闭图片通知。<br>**default** : true |


### Close ImageNotices

通过调用closeImageNotices API，可以关闭所有的图片通知。

**API**

```java
+ (void)Gamebase.ImageNotice.closeImageNotices(@NonNull Activity activity);
```

## WebView

Gamebase支持基本的WebView。


### Show WebView

显示WebView。

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
Gamebase.WebView.showWebView(activity, "http://www.toast.com");
```

![Webview Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

#### 自定义WebView

显示自定义WebView。<br/>
可以用GamebaseWebViewConfiguration设置自定义WebView。

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setTitleText("title")                              // 设置WebView标题
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // 设置WebView页面方向
            .setNavigationBarColor(Color.RED)                   // 设置导航栏的颜色
            .setNavigationBarHeight(40)                         // 设置导航栏的高度
            .setBackButtonVisible(true)                         // 返回按钮有效或无效
            .setBackButtonImageResource(R.id.back_button)       // 设置返回按钮图标
            .setCloseButtonImageResource(R.id.close_button)     // 设置关闭按钮图标
            .build();
GamebaseWebView.showWebView(activity, "http://www.toast.com", configuration);
```

#### Custom Scheme

使用在Gamebase WebView加载的网页内scheme，可使用某个特定功能或更改网页内容。。

##### Predefined Custom Scheme

为Gamebase指定的scheme。

| scheme               | 作用                                  |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | 关闭WebView                          |
| gamebase://goback    | 返回上一页                 |
| gamebase://getuserid | 查看当前登录的用户ID |
| gamebase://openbrowser?link={URLEncodedURL} | 用外部浏览器打开link参数的URL。<br>URLEncodedURL : 使用外部浏览器将要打开的URL。<br>需要对URL进行解码。 |

#### User Custom Scheme

直接设置SchemeList，当URL匹配时处理Event。

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

可以使用以下API轻松显示 [Android toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) 消息。<br/>
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
