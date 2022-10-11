## Game > Gamebase > Android SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

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

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | 未调用Gamebase.initialize。|
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 显示图片通知弹窗时，因出现超时错误强制关闭所有弹窗。|

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
| newBuilder() | **M** | 使用newBuilder()函数，可以生成ImageNoticeConfiguration.Builder对象。 |
| build() | **M** | 将设置的Builder转换为Configuration对象。 |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | 图片通知背景颜色<br>使用String参数时，调用转换为android.graphics.Color.parseColor(String) API的值。<br>**default**: #80000000 |
| setTimeout(long timeoutMs) | O | 最大加载时间 (单位 : millisecond)<br>**default**: 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | 出现custom scheme事件时，判断是否应强制关闭图片通知。<br>**default**: true |


### Close ImageNotices

通过调用closeImageNotices API，可以关闭所有的图片通知。

**API**

```java
+ (void)Gamebase.ImageNotice.closeImageNotices(@NonNull Activity activity);
```

## Terms

显示在Gamebase控制台中注册的条款。

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

调用showTermsView API，通过Webview显示条款窗口。
如果需要直接创建符合Game UI的条款窗口，可通过调用queryTerms API来显示Gamebase控制台中注册的条款项目。
如果用户已同意，则将各项目的“同意与否”通过updateTerms API传送到Gamebase服务器。

### showTermsView

在页面中显示条款窗口。
用户同意条款后，将“同意与否”注册在服务器中。   
如果已同意条款，即使再调用showTermsView API，也不显示条款窗口，而立即返还“成功回调”。
但如果将Gamebase控制台中的“重新同意条款”项目修改为**必须**，用户再次同意条款之前会一直显示条款窗口。

#### Required参数

* Activity : 是显示条款窗口的Activity。
 
#### Optional参数

* GamebaseTermsConfiguration : 通过GamebaseTermsConfiguration对象可以更改“是否强制显示同意条款窗”等设置。 
* GamebaseDataCallback : 同意条款后，条款窗将被关闭时通过回调通知用户。将通过回调返还的GamebaseDataContainer对象 转换为GamebaseShowTermsViewResult确认附加信息。 

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
| newBuilder() | **M** | 通过newBuilder()函数创建GamebaseTermsConfiguration.Builder对象。 |
| build() | **M** | 将设置完的Builder转换为Configuration对象。 |
| setForceShow(boolean forceShow) | O | 如果已同意条款，即使再调用showTermsView API也不显示条款窗口。但忽略此项并强制显示条款窗。<br>**default**: false |
| enableFixedFontSize(boolean enable) | O | 忽略系统字体大小并以固定大小显示条款。<br>**default**: false |

**GamebaseShowTermsViewResult**

| Field | Type | Nullable / NonNull | Description |
| --- | --- | --- | --- |
| isTermsUIOpened | boolean | NonNull | **true**: 显示条款窗后，由于用户已同意条款，条款窗被关闭。<br>**false**: 由于已同意条款，不显示条款窗，条款窗已关闭。 |  
| pushConfiguration | PushConfiguration | Nullable | 如果isTermsUIOpened为**true**时，在条款中添加“是否同意接收推送”， pushConfiguration则始终具有有效的对象。<br>否则为**null**。<br>pushConfiguration有效时，pushConfiguration.pushEnabled值始终为 **true**。 |

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| LAUNCHING\_SERVER\_ERROR | 2001 | 当从Launching服务器接收的项目中没有与条款相关的信息时，会发生此错误。<br/>发生此问题时，请联系Gamebase负责人。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | Terms API的调用未完成 。<br/>请稍后再试。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | 条款Webview尚未终止，但已再次调用。 |
| WEBVIEW\_TIMEOUT | 7002 | 显示条款Webview时出现了Timeout。 |
| WEBVIEW\_HTTP\_ERROR | 7003 | 打开条款Webview时出现了HTTP错误。 |

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

Gamebase通过Webview，以简单形式显示条款。
如果您要直接制作符合游戏UI的条款，通过调用queryTerms API可使用Gamebase控制台返还的条款信息。

如果登录后调用，则可确认游戏用户是否同意条款。

> <font color="red">[注意]</font><br/>
>
> * 因Gamebase服务器不保存GamebaseTermsContentDetail.getRequired()为true的必要项目，agreed值将始终以false返回。
>     * 这是因为必要项目将始终保存为true，不需要保存。
> * 因“是否接收推送”没有被存储在gamebase服务器中，agreed值将始终以false返回。  
>     * 如需查看“是否接收推送”，请调用Gamebase.Push.queryTokenInfo API。
> * 如果控制台中未设置“基本条款”，使用与条款语言不同的国家代码在设置的终端上调用queryterms API，则将出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误。
>     * 在控制台中设置“基本条款”或出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误时，请不要显示条款。

#### Required参数

* Activity : 是调用API时的最上级Activity。
* GamebaseDataCallback : 通过回调通知用户API的调用结果。通过作为回调返回的GamebaseQueryTermsResult，可获取在控制台中设置的条款信息。 

**API**

```java
+ (void)Gamebase.Terms.queryTerms(@NonNull Activity activity,
                                  @NonNull GamebaseDataCallback<GamebaseQueryTermsResult> callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 未在控制台中注册条款信息。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 未在控制台中注册与终端机国家代码匹配的条款信息。 |

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
| getTermsSeq             | int                             | 所有条款KEY<br/>是调用updateTerms API时的必须值。          |
| getTermsVersion         | String                          | 条款版本<br/>是调用updateTerms API时的必须值。             |
| getTermsCountryType     | String                          | 条款类型<br/> - KOREAN : 韩国条款<br/> - GDPR : 欧洲条款<br/> - ETC : 其他国家 |
| getContents             | List<GamebaseTermsContentDetail> | 各条款的详细信息 | 

#### GamebaseTermsContentDetail

| API            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| getTermsContentSeq      | int                   | 条款项目KEY         | 
| getName                 | String                | 条款项目名称         |
| getRequired             | boolean                  | 是否要必须同意         |
| getAgreePush            | String                | 是否同意接收广告性推送<br/> - NONE : 不同意。<br/> - ALL : 全部同意。<br/> - DAY : 同意白天接收推送。<br/> - NIGHT : 同意夜间接收推送。         |
| getAgreed               | boolean                  | 用户是否同意相关条款项目<br/> - 登录前始终会是false。<br/> - 推送项目始终会是false。|
| getNode1DepthPosition   | int                   | 第1阶段项目的显示顺序           |
| getNode2DepthPosition   | int                   | 第2阶段项目的显示顺序<br/> 没有时 -1           |
| getDetailPageUrl        | String                | 详细查看条款的URL<br/> 没有时 -null |

### updateTerms

如果使用通过queryTerms API获取的条款信息直接创建了UI，
请将游戏用户同意条款的记录通过updateTerms API传送到Gamebase服务器。

不仅可用于取消“同意可选择条款”，也可用于修改同意条款的记录。 

> <font color="red">[注意]</font><br/>
>
> Gamebase服务器基本上不保存“是否同意接收推送”。
> **登录后**，请通过调用Gamebase.Push.registerPush API保存“是否同意接收推送”。

#### Required参数

* Activity : 是调用API时的最上级Activity。
* GamebaseUpdateTermsConfiguration : 是将要在服务器中注册的用户的可选择条款信息。

#### Optional参数

* GamebaseCallback : 在服务器中注册可选择条款信息后，通过回调通知用户。

**API**

```java
+ (void)Gamebase.Terms.updateTerms(@NonNull Activity activity,
                                   @NonNull GamebaseUpdateTermsConfiguration configuration,
                                   @Nullable GamebaseCallback callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | 未初始化Gamebase。|
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 设置了未注册的条款Seq值。|
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 上一次调用的Terms API未完成。<br/>请稍后再试。|

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
| newBuilder(String termsVersion, int termsSeq, List<GamebaseTermsContent> contents) | 为了生成Configuration对象，创建Builder。|
| build() | 将创建完的Builder，变换为Configuration对象。|

| Parameter            | Mandatory(M) / Optional(O) | Type                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsSeq             | **M**                      | int                       | 所有条款KEY<br/>需要传送调用queryTerms API时获取的值。             |
| termsVersion         | **M**                      | String                    | 条款版本<br/>通过调用queryTerms API，需要传送获取的值。 |
| contents             | **M**                      | List<GamebaseTermsContent> | 用户同意可选择条款的信息 |

#### GamebaseTermsContent

**Constructor**

| API                  | Description         |
| -------------------- | ------------------- |
| GamebaseTermsContent(int termsContentSeq, boolean agreed) | 是GamebaseTermsContent构造器。|
| from(GamebaseTermsContentDetail) | 是从GamebaseTermsContentDetail对象构造GamebaseTermsContent对象的工厂方法模式。|
| setAgreed(boolean) | 相关对象的agreed值将被修改。|

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 可选择条款项目KEY      |
| agreed               | **M**                      | boolean            | 可选择条款项目的同意与否 |

### isShowingTermsView

指示当前是否显示了条款窗。

**API**

```java
+ (boolean)Gamebase.Terms.isShowingTermsView();
```

## WebView

Gamebase支持基本的WebView。


### Show WebView

显示WebView。

##### Required参数
* activity ：显示WebView活动。
* url ：作为参数发送的url必须是有效值。

##### 可选参数
* configuration ：使用GamebaseWebViewConfiguration更改WebView的布局。
* GamebaseCallback ：关闭时WebView通过回调通知用户。
* schemeList ：指定用户想要接收的自定义SchemeList。
* GamebaseDataCallback ：用schemeList指定的包含自定义Scheme的url，作为回调通知。

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
Gamebase.WebView.showWebView(activity, "https://www.toast.com");
```

![Webview Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

#### Custom WebView

显示自定义WebView。<br/>
可以用GamebaseWebViewConfiguration设置自定义WebView。

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setTitleText("title")                              // 设置WebView标题
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // 设置WebView Screen方向
            .setNavigationBarColor(Color.RED)                   // 设置导航栏的颜色
            .setNavigationBarHeight(40)                         // 设置导航栏的高度
            .setBackButtonVisible(true)                         // 返回按钮有效或无效
            .setBackButtonImageResource(R.id.back_button)       // 设置返回按钮图标
            .setCloseButtonImageResource(R.id.close_button)     // 设置关闭按钮图标
            .build();
GamebaseWebView.showWebView(activity, "https://www.toast.com", configuration);
```

#### Custom Scheme

使用在Gamebase WebView加载的网页内scheme，可使用某个特定功能或更改网页内容。

##### Predefined Custom Scheme

为Gamebase指定的scheme。

| scheme               | 描述                                  |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | 关闭WebView。                         |
| gamebase://goback    | 返回上一页。                |
| gamebase://getuserid | 查看当前登录的用户ID。|
| gamebase://showwebview?link={URLEncodedURL} | 通过WebView打开link参数的URL。<br>URLEncodedURL : 将通过WebView要打开的URL。<br>URL需要解码。 |
| gamebase://openbrowser?link={URLEncodedURL} | 用外部浏览器打开link参数的URL。<br>URLEncodedURL : 使用外部浏览器要打开的URL。<br>URL需要解码。 |

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
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | 将横向模式旋转180度。 |
| setNavigationBarVisible(boolean enable)  | true or false                       | 导航栏有效或无效<br>**default**: true |
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | 导航栏颜色  |
| setNavigationBarHeight(int height)       | height                              | 导航栏高度    |
| setBackButtonVisible(boolean visible)    | true or false                       | 返回按钮有效或无效<br>**default**: true |
| setBackButtonImageResource(int resourceId) | ID of resource                      | 返回按钮图像       |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | 关闭按钮的图标      |
| enableAutoCloseByCustomScheme(boolean enable) | true or false | 当Custom Scheme启动时WebView将自动关闭。<br>**default**: true |


### Close WebView
通过以下API，可以关闭当前显示的WebView。

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

![Alert Dialog Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


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

可以使用以下API轻松显示[Android toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html)消息。<br/>
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
    * 可以输入.html文件或URL作为android：value值

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | 描述                  |
| ------------------ | ---------- | ---------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误) |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
