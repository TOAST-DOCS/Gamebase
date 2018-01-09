## Game > Gamebase > Unity Developer's Guide > UI


## Webview

Gamebase supports basic WebView and the user can configure the style: full screen or popup. <br/>

<br/>

WebView-related resources (images, html, and others) are included to Gamebase.bundle. 

### Browser Style WebView

Supports Full-screen WebView. <br/>
The full-screen style (browser) displays a navigation bar, as well as Close and Go Back buttons. You can set a title on the navigation bar. 


**API**<br> 
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)


```cs
static void ShowWebBrowser(string url)
```

**Example**
```cs
public void ShowWebBrowser(string url)
{
    Gamebase.Webview.ShowWebBrowser(url);
}
```

### Popup Style WebView

Supports pop-up WebView. <br/>
The pop-up style displays a modal view on an existing screen, with the background covered by a transparent mask view. 

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)

```cs
static void ShowWebPopup(string url)
```

**Example**
```cs
public void ShowWebPopup(string url)
{
    Gamebase.Webview.ShowWebPopup(url);
}
```

### Custom WebView

Displays customized WebView. <br/>Customized WebView can be created by using GamebaseWebViewConfiguration. 

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration)
```

**Example**
```cs
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
     configuration.url = "https://www.toast.com/";

     Gamebase.Webview.ShowWebView(configuration);
}
```

### Custom WebView with Local URL

Displays HTML files at a local directory on a customized WebView.<br/>Customized WebView can be created by using GamebaseWebViewConfiguration.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void ShowWebViewFile(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuratio)
```

**Example**
```cs
public void ShowWebViewFile()
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
     configuration.url = "file://test.html";

     Gamebase.Webview.ShowWebViewFile(configuration);
}
```

### GamebaseWebViewConfiguration

| Parameter                | Values                                   | Description                        |
| ------------------------ | ---------------------------------------- | ---------------------------------- |
| title                    | string                                   | Title of WebView                    |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    |        |
|                          | GamebaseScreenOrientation.PORTRAIT       | Vertical Mode                      |
|                          | GamebaseScreenOrientation.LANDSCAPE      | Horizontal Mode                    |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | Turn Left to Right                 |
| colorR                   | 0~255                                    | Color of Navigation Bar: Alpha     |
| colorG                   | 0~255                                    | Color of Navigation Bar: R         |
| colorB                   | 0~255                                    | Color of Navigation Bar: G         |
| colorA                   | 0~255                                    | Color of Navigation Bar: B         |
| buttonVisible            | true or false                            | Activate/Deactivate Go Back Button |
| barHeight                | height                                   | Height of Navigation Bar           |
| backButtonImageResource  | ID of resource                           | Image of Go Back Button            |
| closeButtonImageResource | ID of resource                           | Image of Close Button              |
| url                      | "http://" or "https://" or "file://"     | Web URL                            |

### Predefined Custom Scheme

Gamebase has specified following schemes: <br/>

| scheme                        | Usage                                    |
| ----------------------------- | ---------------------------------------- |
| gamebase://dismiss            | Close WebView                            |
| gamebase://goBack             | Go back from WebView                     |
| gamebase://getUserId          | Show ID of a user show is currently logged-in |
| gamebase://getMaintenanceInfo | Display maintenance information on WebPage |

## Alert

Displays system alerts. <br/>
Can register buttons or callback on system alerts. 

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void ShowAlert(string title, string message)
static void ShowAlert(string title, string message, GamebaseCallback.VoidDelegate buttonCallback)
```

**Example**
```cs
public void ShowAlertDialog()
{
    Gamebase.Util.ShowAlertDialog
    (
        "Title",
        "Message"
    );
}

public void ShowAlertDialog()
{
    Gamebase.Util.ShowAlertDialog
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

Displays [Android Toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API. 

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

| Error              | Error Code | Description                      |
| ------------------ | ---------- | -------------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes: 
  - [Entire Error Codes](./error-codes#client-sdk)