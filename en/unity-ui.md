## Game > Gamebase > Unity Developer's Guide > UI

## Webview

### Show WebView

Shows a WebView.<br/>

##### Required Parameters
* URL: The url delivered as a parameter should be valid.

##### Optional Parameters
* configuration: Changes WebView layout by using GamebaseWebViewConfiguration.
* closeCallback: Notifies users when a WebView is closed.
* schemeList: Specifies the list of customized schemes a user wants.
* schemeEvent: Notifies url including customized scheme specified by the schemeList with a callback.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> Stansalone에서는 WebViewAdapter를 통해서 WebView를 지원하며 WebView가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.

**Example**
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
| title                    | string                                   | Title of WebView                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | Unspecified |
|                          | GamebaseScreenOrientation.PORTRAIT       | Portrait Mode                      |
|                          | GamebaseScreenOrientation.LANDSCAPE      | Landscape Mode                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | Reverse Landscape              |
| colorR                   | 0~255                                    | Color of Navigation Bar: Alpha            |
| colorG                   | 0~255                                    | Color of Navigation Bar: R                 |
| colorB                   | 0~255                                    | Color of Navigation Bar: G               |
| colorA                   | 0~255                                    | Color of Navigation Bar: B                |
| buttonVisible            | true or false                            | Activate/Deactivate Go Back Button           |
| barHeight                | height                                   | Height of Navigation Bar                  |
| backButtonImageResource  | ID of resource                           | Image of Go Back Button                |
| closeButtonImageResource | ID of resource | Image of Close Button |
| url | "http://" or "https://" or "file://" | Web URL |

#### Predefined Custom Scheme

Gamebase has specified following schemes.

| scheme | Usage |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | Close WebView |
| gamebase://goBack | Go back from WebView |
| gamebase://getUserId          | Show ID of a user who is currently logged-in |
| gamebase://getMaintenanceInfo | Display maintenance information on WebPage |


### Close WebView

Close currently displayed WebView by using the following API.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseWebview()
```

**Example**CloseWebview
```cs
public void CloseWebview()
{
    Gamebase.Webview.CloseWebview();
}
```


## Open External Browser

Open an external browser by using the following API. The URL delivered as a parameter should be valid.

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

**Example**
```cs
public void OpenWebBrowser(string url)
{
    Gamebase.Webview.OpenWebBrowser(url);
}
```


## Alert

Displays a system alert API.
시스템 알림에 콜백을 등록할 수도 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowAlert(string title, string message)
static void ShowAlert(string title, string message, GamebaseCallback.VoidDelegate buttonCallback)
```

**Example**
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

Displays message easily, by using the following API.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowToast(string message, int duration)
static void ShowToast(string message, GamebaseUIToastType type)
```

**Example**
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
| UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)
