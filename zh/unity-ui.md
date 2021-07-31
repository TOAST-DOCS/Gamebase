## Game > Gamebase > Unity SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，可以向用户显示公告。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

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

## Terms

在Gamebase控制台中显示注册的条款。

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

调用ShowTermsView API，可通过Webview显示条款窗口。
如果要生成符合Game UI的条款窗口，则可通过调用QueryTerms API显示在Gamebase控制台中注册的条款项目。 
如果用户已经同意条款，通过UpdateTerms API，将各项目的”同意与否”传送到Gamebase服务器。

### ShowTermsView

将条款窗口显示在页面上。
用户已经同意条款时，在服务器注册”同意与否”。
如果已经同意条款，即使再调用ShowTermsView API，也不再显示条款窗口，而直接返还”成功回调”。 
但如果在Gamebase控制台中将条款的“重新同意"项目更改为**必须**，用户再次同意之前一直显示条款窗口。  

> <font color="red">[注意]</font><br/>
>
> 如果已在条款中添加”是否同意接收推送”，则可从GamebaseResponse.DataContainer生成GamebaseResponse.Push.PushConfiguration。
> 如果GamebaseResponse.Push.PushConfiguration不为null，**登录后**，请调用Gamebase.Push.RegisterPush API。
>

#### Optional参数

* callback : 同意条款后，关闭条款窗口时，通过回调通知用户。如果将通过回调获取的GamebaseDataContainer对象转换为PushConfiguration，登录后可用于调用Gamebase.Push.registerPush API。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowTermsView(GamebaseCallback.GamebaseDelegate<GamebaseResponse.DataContainer> callback)
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | 未初始化Gamebase。|
| LAUNCHING\_SERVER\_ERROR(2001) | 是从启动服务器返还的项目不包含相关条款内容时出现的错误。<br/>出现该问题时，请联系Gamebase负责人员。|
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 上一次调用的Terms API未完成。<br/>请稍后再试。|
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW(6925) | Webview尚未关闭，但已重新调用。|
| WEBVIEW\_TIMEOUT(7002) | 显示条款Webview时出现超时错误。|
| WEBVIEW\_HTTP\_ERROR(7003) | 打开条款Webview时出现HTTP错误。|  

**Example**

```cs
public void SampleShowTermsView()
{
    Gamebase.Terms.ShowTermsView((data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("ShowTermsView succeeded.");
            GamebaseResponse.Push.PushConfiguration pushConfiguration = GamebaseResponse.Push.PushConfiguration.From(data)
        }
         else
        {
            Debug.Log(string.Format("ShowTermsView failed. error:{0}", error));
        }
    });
}
```


### QueryTerms 

Gamebase通过Webview，以简单形式显示条款。 
如果要直接制作符合游戏UI的条款，通过调用queryTerms API，则可使用从Gamebase控制台中被返还的条款信息。

如果登录后调用，则可确认游戏用户是否同意条款。

> <font color="red">[注意]</font><br/>
>
> * 因Gamebase服务器不保存GamebaseTermsContentDetail.getRequired()为true的必要项目，agreed值将始终以false返回。 
>     * 这是因为必要项目将始终保存为true，不需要保存。
> * 因”是否接收推送”没被存储在gamebase服务器中，agreed值将始终以false返回。
>     * 如需查看”是否接收推送”，请调用Gamebase.Push.queryTokenInfo API。
> * 如果在控制台中未设置”基本条款”的状态下，使用与条款语言不同的国家代码在设置的终端机上调用queryTerms API，则将出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误。
>     * 在控制台中设置”基本条款”或出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误时，请不要显示条款。

#### Required参数
* callback : 通过回调通知用户API的调用结果。通过作为回调被返回的GamebaseQueryTermsResult，可获取在控制台中设置的条款信息。 


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void QueryTerms(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Terms.QueryTermsResult> callback)
```
 
**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | 未初始化Gamebase。|
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | 在控制台中未注册条款信息。|
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | 在控制台中未注册符合终端机国家代码的条款信息。|

**Example**

```cs
public void SampleQueryTerms()
{
    Gamebase.Terms.QueryTerms((data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("QueryTerms succeeded.");
        }
        else
        {
            Debug.Log(string.Format("QueryTerms failed. error:{0}", error));
        }
    });
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- | 
| termsSeq             | int                             | 所有条款的KEY<br/>是调用updateTerms API时的必须值。|
| termsVersion         | string                          | 条款版本<br/>是调用updateTerms API时的必须值。             |
| termsCountryType     | string                          | 条款类型<br/> - KOREAN : 韩国条款 <br/> - GDPR : 欧洲条款 <br/> - ETC : 其他条款         |
| contents             | List< ContentDetail > | 条款项目信息          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 条款项目KEY         | 
| name                 | string                | 条款项目名称         |
| required             | bool                  | 是否必须同意        |
| agreePush            | string                | 是否同意接收广告性推送通知<br/> - NONE : 不同意。<br/> - ALL : 全部同意。<br/> - DAY : 同意白天接收推送通知。<br/> - NIGHT : 同意夜间接收推送通知。          |
| agreed               | bool                  | 用户是否同意相关条款项目           
| node1DepthPosition   | int                   | 显示第1阶段项目的顺序           |
| node2DepthPosition   | int                   | 显示第2阶段项目的顺序<b r/> 没有时 -1           |
| detailPageUrl        | string                | 详细查看条款URL<br/> 没有时 -null |


### UpdateTerms

如果使用调用QueryTerms API后被返回的条款信息创建了UI，
请将游戏用户同意条款的记录通过updateTerms API传送到Gamebase服务器。

不仅可用于取消”可选择条款同意”，也可用于修改同意条款的记录。 

> <font color="red">[注意]</font><br/>
>
> Gamebase服务器基本上不保存”是否同意接收推送”。
> 当保存”是否同意接收推送时，**登录后**通过调用Gamebase.Push.registerPush API来保存。
>

#### Required参数
* configuration : 是将要在服务器中注册的用户的可选条款信息。
 
#### Optional参数

* callback : 在服务器中注册可选条款信息后，通过回调通知用户。 


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void UpdateTerms(GamebaseRequest.Terms.UpdateTermsConfiguration configuration, GamebaseCallback.ErrorDelegate callback)
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | 未初始化Gamebase。|
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 您设置了未注册的条款Seq值。|
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 上一次调用的Terms API未完成。<br/>请稍后再试。|


**Example**

```cs
public void SampleUpdateTerms()
{            
    List<GamebaseRequest.Terms.Content> list = new List<GamebaseRequest.Terms.Content>();
    list.Add(new GamebaseRequest.Terms.Content()
    {
        termsContentSeq = 0,
        agreed = true
    });
    
    Gamebase.Terms.UpdateTerms(
        new GamebaseRequest.Terms.UpdateTermsConfiguration()
        {
            termsSeq = 0,
            termsVersion = "1.0.0",
            contents = list
        }
        ,
        (error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("UpdateTerms succeeded.");
            }
            else
            {
                Debug.Log(string.Format("UpdateTerms failed. error:{0}", error));
            }
        });
}
```


#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | string                    | 条款版本<br/>需要传送调用queryTerms API时获取的值。  |
| termsSeq             | **M**                      | int                       | 所有条款的KEY<br/>通过调用queryTerms API，需要传送获取的值。             |
| contents             | **M**                      | List< Content > | 用户同意可选条款的信息 |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 可选条款项目KEY      |
| agreed               | **M**                      | bool               | 是否同意可选条款项目 |


## Webview

### Show WebView

显示WebView。<br/>

##### Required参数
* url：作为参数发送的url必须是有效值。

##### 可选参数
* configuration：可以使用GamebaseWebViewConfiguration更改WebView的布局。
* closeCallback：WebView关闭时通过回调通知用户。
* schemeList：指定用户想要接收的自定义SchemeList。
* schemeEvent：将用schemeList指定的包含自定义Scheme的url，作为回调通知。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> 在Standalone中通过WebViewAdapter支持WebView。当WebView被打开时，不需要对输入为UI的Event进行Blocking。

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

| scheme | 描述 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss |关闭WebView。|
| gamebase://goBack | 返回上一页。|
| gamebase://getUserId          | 显示当前登录的用户ID。|
| gamebase://getMaintenanceInfo | 在WebPage上显示维护内容。|
| gamebase://showwebview?link={URLEncodedURL} | 通过WebView打开link参数的URL。<br>URLEncodedURL : 通过WebView将要打开的URL。<br>URL需要解码。|
| gamebase://openbrowser?link={URLEncodedURL} | 使用外部浏览器打开link参数的URL。<br/>URLEncodedURL : 通过使用外部浏览器将要打开的URL。<br/>URL需要解码。|

### Close WebView

通过以下API，可以关闭当前显示的WebView。

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

可使用以下API打开外部浏览器。作为参数传递的URL必须是有效值。

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
