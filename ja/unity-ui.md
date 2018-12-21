## Game > Gamebase > Unity SDK ご利用ガイド > UI

## Webview

### Show WebView

WebViewを表示します。<br/>

##### Requiredパラメーター
* url:パラメーターで送信されるurlは、有効な値でなければなりません。

##### Optionalパラメーター
* configuration:GamebaseWebViewConfigurationでWebViewのレイアウトを変更することができます。
* closeCallback:WebViewが終了する際に、ユーザーにコールバックで知らせます。
* schemeList:ユーザーの求めるカスタムSchemeのリストを指定します。
* schemeEvent:schemeListに指定したカスタムSchemeを含むurlをコールバックで知らせます。

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
| title                    | string                                   | WebViewのタイトル                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 未指定 |
|                          | GamebaseScreenOrientation.PORTRAIT       | 縦モード                       |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 横モード                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 横モードを180度回転              |
| colorR                   | 0~255                                    | ナビゲーションバーの色Alpha            |
| colorG                   | 0~255                                    | ナビゲーションバーの色R                |
| colorB                   | 0~255                                    | ナビゲーションバーの色G                |
| colorA                   | 0~255                                    | ナビゲーションバーの色B                |
| buttonVisible            | true or false                            | 戻るボタンの有効化または無効化          |
| barHeight                | height                                   | ナビゲーションバーの高さ                  |
| backButtonImageResource  | ID of resource                           | 戻るボタンのイメージ                |
| closeButtonImageResource | ID of resource | 閉じるボタンの画像 |
| url | "http://" or "https://" or "file://" | ウェブURL |

#### Predefined Custom Scheme

Gamebaseで指定しておいたSchemeです。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebViewを閉じる |
| gamebase://goBack | WebViewに戻る |
| gamebase://getUserId          | 現在ログイン中のゲームユーザーのユーザーIDを表示 |
| gamebase://getMaintenanceInfo | メンテナンス内容をWebPageに表示 |


### Close WebView

次のAPIを利用して表示されているWebViewを閉じることができます。

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

次のAPIを通じて、外部ブラウザを開くことができます。パラメーターで送信されるurlは、有効な値でなければなりません。

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

システム通知を表示することができます。
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

次のAPIを使用して簡単にメッセージを表示することができます。

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
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

