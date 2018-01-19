## Game > Gamebase > Unity SDK 사용 가이드 > UI

## Webview

### Show WebView

WebView를 표시합니다.<br/>

##### Required 파라미터
* url : 파라미터로 전송되는 url은 유효한 값이어야 합니다.

##### Optional 파라미터
* configuration : GamebaseWebViewConfiguration으로 WebView의 레이아웃을 변경 할 수 있습니다.
* closeCallback : WebView가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList : 사용자가 받고 싶은 커스텀 Scheme 목록을 지정합니다.
* schemeEvent : schemeList로 지정한 커스텀 Scheme을 포함하는 url을 콜백으로 알려 줍니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

**Example**
```cs
public void ShowWebView(GamebaseCallback.ErrorDelegate closeCallback, List<string> schemeList, GamebaseCallback.GamebaseDelegate<string> schemeEvent)
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


     Gamebase.Webview.ShowWebView("https://www.toast.com/", configuration, closeCallback, schemeList, schemeEvent);
}
```


#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | WebView의 제목                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 미지정 |
|                          | GamebaseScreenOrientation.PORTRAIT       | 세로 모드                       |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 가로 모드                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 가로 모드를 180도 회전              |
| colorR                   | 0~255                                    | 내비게이션 바 색상 Alpha            |
| colorG                   | 0~255                                    | 내비게이션 바 색상 R                |
| colorB                   | 0~255                                    | 내비게이션 바 색상 G                |
| colorA                   | 0~255                                    | 내비게이션 바 색상 B                |
| buttonVisible            | true or false                            | 뒤로 가기 버튼 활성 또는 비활성          |
| barHeight                | height                                   | 내비게이션 바 높이                  |
| backButtonImageResource  | ID of resource                           | 뒤로 가기 버튼 이미지                |
| closeButtonImageResource | ID of resource | 닫기 버튼 이미지 |
| url | "http://" or "https://" or "file://" | 웹 URL |

#### Predefined Custom Scheme

Gamebase에서 지정해 놓은 Scheme 입니다.

| scheme | 용도 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebView 닫기 |
| gamebase://goBack | WebView 뒤로가기 |
| gamebase://getUserId          | 현재 로그인되어 있는 게임 이용자의 사용자 ID를 표시 |
| gamebase://getMaintenanceInfo | 점검 내용을 WebPage에 표시 |


### Close WebView

다음 API를 이용하여 보여지고 있는 WebView를 닫을 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)


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

다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 url은 유효한 값이어야 합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)
![STANDALONE](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone_1.2.0.png)
![WEBGL](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-webgl_1.2.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor_1.2.0.png)


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

시스템 알림을 표시할 수 있습니다.<br/>
시스템 알림에 버튼이나 콜백을 등록할 수도 있습니다. 

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios_1.2.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android_1.2.0.png)

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

다음 API를 사용하여 쉽게 메시지를 표시할 수 있습니다.

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

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 알수 없는 오류입니다(정의되지 않은 오류입니다). |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
  - [Entire Error Codes](./error-codes#client-sdk)