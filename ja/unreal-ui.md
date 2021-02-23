## Game > Gamebase > Unreal SDK使用ガイド > UI

## Webview

### Show WebView

WebViewを表示します。<br/>

##### Requiredパラメータ
* url：パラメータに転送されるurlは、有効な値である必要があります。

##### Optionalパラメータ(現在はRequireパラメータですが、今後のバージョンでOptionalに変更予定)
* configuration：GamebaseWebViewConfigurationでWebViewのレイアウトを変更できます。
* closeCallback：WebViewが終了する時、ユーザーにコールバックで伝えます。
* schemeList：ユーザーが受け取りたいカスタムSchemeリストを指定します。
* schemeEvent：schemeListに指定したカスタムSchemeを含むurlをコールバックで伝えます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowWebView(const FString& url, const FGamebaseWebViewConfiguration& configuration, FGamebaseErrorDelegate& onCloseCallback, const TArray<FString>& schemeList, const FGamebaseSchemeEventDelegate& onSchemeEvent);
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

**Example**
```cpp
void Sample::ShowWebView(const FString& url)
{
    FGamebaseWebViewConfiguration configuration{ TEXT("Title"), GamebaseScreenOrientation::Unspecified, 128, 128, 128, 255, 40, true, "", "" };

    TArray<FString> schemeList{ TEXT("customScheme://openBrowser") };

    IGamebase::Get().GetWebView().ShowWebView(url, configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            Result(ANSI_TO_TCHAR(__FUNCTION__), TEXT("Close webview"));
        }),
        schemeList,
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& scheme, const FGamebaseError* error) {
        if (Gamebase::IsSuccess(error))
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), true, *FString::Printf(TEXT("scheme= %s"), *scheme));
        }
        else
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), false, GamebaseJsonUtil::UStructToJsonObjectString(*error));
        }
    }));
}
```


#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | WebViewのタイトル               |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 未指定 |
|                          | GamebaseScreenOrientation.PORTRAIT       | 縦モード                     |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 横モード                     |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 横モードを180度回転              |
| colorR                   | 0～255                                    | ナビゲーションバーの色相Alpha            |
| colorG                   | 0～255                                    | ナビゲーションバーの色相R                |
| colorB                   | 0～255                                    | ナビゲーションバーの色相G                |
| colorA                   | 0～255                                    | ナビゲーションバーの色相B                |
| buttonVisible            | true or false                            | 戻るボタン有効/無効          |
| barHeight                | height                                   | ナビゲーションバーの高さ                  |
| backButtonImageResource  | ID of resource                           | 戻るボタンのイメージ              |
| closeButtonImageResource | ID of resource | 閉じるボタンのイメージ |
| url | "http://" or "https://" or "file://" | Web URL |

#### Predefined Custom Scheme

Gamebaseで指定しておいたSchemeです。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebViewを閉じる |
| gamebase://goBack | WebView戻る |
| gamebase://getUserId          | 現在ログインしているゲームユーザーのユーザーIDを表示 |
| gamebase://getMaintenanceInfo | メンテナンス内容をWebPageに表示 |


### Close WebView

次のAPIを利用して、表示されているWebViewを閉じることができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseWebView();
```

**Example**CloseWebview
```cpp
void Sample::CloseWebView()
{
    IGamebase::Get().GetWebView().CloseWebView();
}
```


## Open External Browser

次のAPIを通して外部ブラウザーを開くことができます。パラメータに転送されるurlは有効な値である必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void OpenWebBrowser(const FString& url);
```

**Example**
```cpp
void Sample::OpenWebBrowser(const FString& url)
{
    IGamebase::Get().GetWebView().OpenWebBrowser(url);
}
```


## Alert

システム通知を表示できます。
システム通知にコールバックを登録することもできます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowAlert(const FString& title, const FString& message);
void ShowAlert(const FString& title, const FString& message, const FGamebaseAlertCloseDelegate& onCloseCallback);
```

**Example**
```cpp
void Sample::ShowAlert(const FString& title, const FString& message)
{
    IGamebase::Get().GetUtil().ShowAlert(title, message);
}

void Sample::ShowAlertEvent(const FString& title, const FString& message)
{
    IGamebase::Get().GetUtil().ShowAlert(title, message, FGamebaseAlertCloseDelegate::CreateLambda([=]()
    {
            UE_LOG(GamebaseTestResults, Display, TEXT("ShowAlert ButtonClick."));
    }));
}
```

## NHN Cloud

次のAPIを使用して、簡単にメッセージを表示できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowToast(const FString& message, EGamebaseToastExposureTime exposureTimeType);
```

**Example**
```cpp
void Sample::ShowToast(const FString& message, EGamebaseToastExposureTime exposureTimeType)
{
    IGamebase::Get().GetUtil().ShowToast(message, exposureTimeType);
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
