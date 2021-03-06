## Game > Gamebase > Unity SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，可以向用户显示公告。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

将图片通知显示在页面上。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowImageNotices(GamebaseRequest.ImageNotice.Configuration configuration, GamebaseCallback.ErrorDelegate closeCallback, GamebaseCallback.GamebaseDelegate<string> eventCallback = null)
```

**Example**

```cs
public void ShowImageNotices()
{
    Gamebase.ImageNotice.ShowImageNotices(
        null,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

### Custom ImageNotices

将自定义图片通知显示在页面上。
通过使用GamebaseRequest.ImageNotice.Configuration可创建自定义图片通知。

**Example**

```cs
public void ShowImageNotices(int colorR = 0 , int colorG = 0, int colorB = 0, int colorA = 128, long timeOut = 5000)
{
    GamebaseRequest.ImageNotice.Configuration configuration = new GamebaseRequest.ImageNotice.Configuration();
    configuration.colorR = colorR;
    configuration.colorG = colorG;
    configuration.colorB = colorB;
    configuration.colorA = colorA;
    configuration.timeOut = timeOut;

    Gamebase.ImageNotice.ShowImageNotices(
        configuration,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

#### GamebaseRequest.ImageNotice.Configuration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| colorR                   | 0~255                                    | Navigation Bar 颜色R            |
| colorG                   | 0~255                                    | Navigation Bar 颜色G                |
| colorB                   | 0~255                                    | Navigation Bar 颜色B                |
| colorA                   | 0~255                                    | Navigation Bar 颜色Alpha                |
| timeoutMS                | long        | 图片通知的最大加载时间(单位 : millisecond)<br/>**default** : 5000                     |

### Close ImageNotices

通过调用closeImageNotices API，可以关闭当前显示的所有图片通知。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseImageNotices()
```

## Webview

### Show WebView

显示WebView。<br/>

##### Required参数
* url：作为参数发送的url必须是有效值。

##### 可选参数
* configuration：可以使用GamebaseWebViewConfiguration更改WebView的布局。
* closeCallback：WebView关闭时通过回调通知用户。
* schemeList：指定用户想要接收的自定义SchemeList。
* schemeEvent：用schemeList指定的包含自定义Scheme的url，作为回调通知。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> 在Standalone中通过WebViewAdapter支持WebView。当WebView被打开时，不对输入为UI的Event进行Blocking。

**示例**
```cs
private void SchemeEvent(string scheme, GamebaseError error)
{
    Debug.Log(string.Format("[SchemeEvent] scheme:{0}", scheme));
}

private void CloseCallback(GamebaseError error)
{
    Debug.Log("CloseCallback");
}
    
public void ShowWebView()
{
    GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = new GamebaseRequest.Webview.GamebaseWebViewConfiguration();
     configuration.title = "Title";
     configuration.orientation = GamebaseScreenOrientation.Portrait;
     configuration.colorR = 128;
     configuration.colorG = 128;
     configuration.colorB = 128;
     configuration.colorA = 255;
     configuration.barHeight = 40;
     configuration.buttonVisible = true;
    
    var schemeList = new List<string>() { "customScheme://openBrowser" };
    
    Gamebase.Webview.ShowWebView("https://www.toast.com/", configuration, CloseCallback, schemeList, SchemeEvent);
}
```


#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | WebView标题                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 未指定 |
|                          | GamebaseScreenOrientation.PORTRAIT       | 纵向模式                      |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 横向模式                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 将横向模式旋转180度             |
| contentMode              | GamebaseWebViewContentMode.RECOMMENDED        | 当前平台推荐的浏览器 |
|                          | GamebaseWebViewContentMode.MOBILE             | 移动浏览器            |
|                          | GamebaseWebViewContentMode.DESKTOP            | 桌面浏览器          |
| colorR                   | 0~255                                    | Navigation Bar颜色R            |
| colorG                   | 0~255                                    | Navigation Bar颜色G                |
| colorB                   | 0~255                                    | Navigation Bar颜色B                |
| colorA                   | 0~255                                    | Navigation Bar颜色Alpha                |
| buttonVisible            | true or false                            | 返回按钮有效或无效          |
| barHeight                | height                                   | 导航栏高度                 |
| backButtonImageResource  | ID of resource                           | 返回按钮的图标              |
| closeButtonImageResource | ID of resource | 关闭按钮的图标 |
| url | "http://" or "https://" or "file://" | Web URL |

> [TIP]
>
> iPadOS 13以上WebView基本上是桌面模式。
> 通过contentMode =”GamebaseWebViewContentMode.MOBILE”，可以转换为移动模式。

#### Predefined Custom Scheme

在Gamebase中指定的架构。

| scheme | 作用 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss |关闭WebView  |
| gamebase://goBack | 返回WebView  |
| gamebase://getUserId          | 显示当前登录用户的ID  |
| gamebase://getMaintenanceInfo | 在WebPage上显示维护内容 |


### Close WebView

通过以下API，可以关闭正在显示的WebView。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseWebview()
```

**示例**

```cs
public void CloseWebview()
{
    Gamebase.Webview.CloseWebview();
}
```


## Open External Browser

可以使用以下API打开外部浏览器。作为参数传递的URL必须是有效值。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void OpenWebBrowser(string url)
```

**示例**
```cs
public void OpenWebBrowser(string url)
{
    Gamebase.Webview.OpenWebBrowser(url);
}
```


## Alert

可以显示系统提醒。
可以在系统通知中登记按钮或回调。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowAlert(string title, string message)
static void ShowAlert(string title, string message, GamebaseCallback.VoidDelegate buttonCallback)
```

**示例**
```cs
public void ShowAlertD()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message"
    );
}

public void ShowAlertDialog()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message",
        () => {
                    Debug.Log("ButtonClick");
              }
    );
}
```

## Toast

可以使用以下API轻松显示消息。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowToast(string message, int duration)
static void ShowToast(string message, GamebaseUIToastType type)
```

**示例**
```cs
public void ShowToast(string message, int duration)
{
    Gamebase.Util.ShowToast(
        message,
        duration
        );
}
public void ShowToast(string message, GamebaseUIToastType type)
{
    Gamebase.Util.ShowToast(
        message,
        type
        );
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 显示图片通知时出现超时错误。|
| UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误) |

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
