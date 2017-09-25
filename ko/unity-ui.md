## Upcoming Products > Gamebase > Unity Developer's Guide > UI


## Webview

### Browser Style WebView

Fullscreen 웹뷰를 지원합니다.<br/>
Fullscreen 스타일은 네비게이션바를 가지며, Close/GoBack 버튼을 가집니다. 네비게이션바에 타이틀을 지정할 수 있습니다.


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

Popup 웹뷰를 지원합니다.<br/>
Popup 스타일은 기존화면 위에 모달뷰 형식으로 나타나게 되며, 뒷 배경은 투명한 mask view로 덮어씌워집니다.

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

Custom WebView를 노출합니다.<br/>
GamebaseWebViewConfiguration 설정으로 WebView를 Customizing 할 수 있습니다.

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

### Custom WebView with Local Url

로컬에 가지고 있는 html 파일을 Custom WebView에 노출합니다. <br/>
GamebaseWebViewConfiguration 설정으로 WebView를 Customizing 할 수 있습니다.

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

| Parameter | Values | Description |
| --- | --- | --- |
| title | string | 웹뷰의 타이틀 |
| orientation | GamebaseScreenOrientation.UNSPECIFIED | |
| | GamebaseScreenOrientation.PORTRAIT | 세로모드 |
| | GamebaseScreenOrientation.LANDSCAPE | 가로모드 |
| | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 가로모드를 180도 회전 |
| colorR | 0~255 | 네비게이션바 색상 Alpha |
| colorG | 0~255 | 네비게이션바 색상 R |
| colorB | 0~255 | 네비게이션바 색상 G |
| colorA | 0~255 | 네비게이션바 색상 B |
| buttonVisible | true or false | 백 버튼 활성 or 비활성 |
| barHeight | height | 네비게이션바 높이 |
| backButtonImageResource | ID of resource | 백 버튼 이미지 |
| closeButtonImageResource | ID of resource | 닫기 버튼 이미지 |
| url | "http://" or "https://" or "file://" | 웹 URL |

### Predefined Custom Scheme

Gamebase에서 지정해 놓은 Scheme 입니다.

| scheme | 용도 |
| --- | --- | 
| gamebase://dismiss | WebView 닫기 |
| gamebase://goBack | WebView 뒤로가기 |
| gamebase://getUserId | 현재 로그인되어 있는 유저의 UserId를 표시 |
| gamebase://getMaintenanceInfo | 점검 내용을 WebPage에 표시 |

## Alert

System Alert 를 위한 API를 제공합니다.<br/>
다음의 API를 통해서, 사용자는 Alert에 버튼 및 콜백을 등록할 수 있습니다.

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

Toast를 간단하게 노출 할 수 있는 API를 제공합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void ShowToast(string message, int duration)
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
```

## Error Handling

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| UI\_UNKNOWN\_ERROR | 6999 | 알 수 없는 에러입니다. (정의되지 않은 에러입니다.) |

* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)