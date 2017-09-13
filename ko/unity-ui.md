## Game > Gamebase > Developer's Guide (Unity) > UI & Util


## Webview

### 1. Show WebBrowser

Fullscreen 웹뷰를 지원합니다.
Fullscreen 스타일은 네비게이션바를 가지며, Close/GoBack 버튼을 가집니다. 네비게이션바에 타이틀을 지정할 수 있습니다.


**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
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

### 2. Show WebPopup

Popup 웹뷰를 지원합니다.
Popup 스타일은 기존화면 위에 모달뷰 형식으로 나타나게 되며, 뒷 배경은 투명한 mask view로 덮어씌워집니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
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

### 3. Show WebView

Gamebase에서는 기본적인 웹뷰를 지원하고 Customizing이 가능합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
```cs
static void ShowWebView(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuratio)
```

**Example**
```cs
public void ShowWebView()
{
    GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = new GamebaseRequest.Webview.GamebaseWebViewConfiguration();
     configuration.title = "Title";
     configuration.orientation = -1;
     configuration.colorR = 128;
     configuration.colorG = 128;
     configuration.colorB = 128;
     configuration.colorA = 255;
     configuration.barHeight = 40;
     configuration.buttonVisible = true;
     configuration.url = "http://www.naver.com";

     Gamebase.Webview.ShowWebView(configuration);
 }
```

### 4. Show WebView File

Gamebase에서는 기본적인 웹뷰를 지원하고 Customizing이 가능합니다.
Local HTML 파일을 웹뷰에서 로딩이 가능합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
```cs
static void ShowWebViewFile(GamebaseRequest.Webview.GamebaseWebViewConfiguration configuratio)
```

**Example**
```cs
public void ShowWebViewFile()
{
    GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = new GamebaseRequest.Webview.GamebaseWebViewConfiguration();
     configuration.title = "Title";
     configuration.orientation = -1;
     configuration.colorR = 128;
     configuration.colorG = 128;
     configuration.colorB = 128;
     configuration.colorA = 255;
     configuration.barHeight = 40;
     configuration.buttonVisible = true;
     configuration.url = "http://www.naver.com";

     Gamebase.Webview.ShowWebViewFile(configuration);
 }
```


## Util

### 1. Show AlertDialog

System Alert 를 위한 API를 제공합니다.
다음의 API를 통해서, 사용자는 Alert에 버튼 및 콜백을 등록할 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
```cs
static void ShowAlertDialog(string title, string message, string okTitle, GamebaseCallback.VoidDelegate okCallback, string cancelTitle, GamebaseCallback.VoidDelegate cancelCallback)
```

**Example**
```cs
public void ShowWebViewFile()
{
    Gamebase.Util.ShowAlertDialog
    (
        "Title",
        "Message",
        "OK Button",
        () => {
                Debug.Log("OKButtonClick");
            },
        "Cancel Button",
        () => {
                Debug.Log("CancelButtonClick");
            });
}
```
