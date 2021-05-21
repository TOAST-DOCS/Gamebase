## Game > Gamebase > Unreal SDK使用ガイド > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

イメージ告知を画面に表示します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowImageNotices(FGamebaseImageNoticeConfiguration& configuration, const FGamebaseErrorDelegate& onCloseCallback);
void ShowImageNotices(FGamebaseImageNoticeConfiguration& configuration, const FGamebaseErrorDelegate& onCloseCallback, const FGamebaseImageNoticeEventDelegate& onEventCallback);
```

**Example**

```cpp
void Sample::ShowImageNotices(int32 colorR, int32 colorG, int32 colorB, int32 colorA, int64 timeOut)
{
    FGamebaseImageNoticeConfiguration configuration{ colorR, colorG, colorB, colorA, timeOut };

    IGamebase::Get().GetImageNotice().ShowImageNotices(configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            // Called when the entire imageNotice is closed.
            ...
        }),
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& scheme, const FGamebaseError* error) {
            // Called when custom event occurred.
            ...
        })
    );
}
```

#### FGamebaseImageNoticeConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| colorR                   | 0～255                                    | ナビゲーションバーの色R            |
| colorG                   | 0～255                                    | ナビゲーションバーの色G                |
| colorB                   | 0～255                                    | ナビゲーションバーの色B                |
| colorA                   | 0～255                                    | ナビゲーションバーの色Alpha                |
| timeOut                  | int64        | イメージ告知最大ローディング時間(単位: millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

closeImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


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


#### FGamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | FString                                   | WebViewのタイトル               |
| orientation              | GamebaseScreenOrientation::Unspecified   | 未指定 |
|                          | GamebaseScreenOrientation::Portrait      | 縦モード                     |
|                          | GamebaseScreenOrientation::Landscape     | 横モード                     |
|                          | GamebaseScreenOrientation::LandscapeReverse | 横モードを180度回転              |
| contentMode              | GamebaseWebViewContentMode::Recommended        | 現在のプラットフォームの推薦ブラウザ |
|                          | GamebaseWebViewContentMode::Mobile             | モバイルブラウザ         |
|                          | GamebaseWebViewContentMode::Desktop            | デスクトップブラウザ       |
| colorR                   | 0～255                                    | ナビゲーションバーの色相R            |
| colorG                   | 0～255                                    | ナビゲーションバーの色相G                |
| colorB                   | 0～255                                    | ナビゲーションバーの色相B                |
| colorA                   | 0～255                                    | ナビゲーションバーの色相Alpha                |
| buttonVisible            | true or false                            | 戻るボタン有効/無効          |
| barHeight                | height                                   | ナビゲーションバーの高さ                  |
| backButtonImageResource  | ID of resource                           | 戻るボタンのイメージ              |
| closeButtonImageResource | ID of resource | 閉じるボタンのイメージ |
| url | "http://" or "https://" or "file://" | Web URL |


> [TIP]
>
> iPadOS 13以上でWebViewは基本的にデスクトップモードです。
> contentMode =`GamebaseWebViewContentMode.MOBILE`設定でモバイルモードに変更できます。

#### Predefined Custom Scheme

Gamebaseで指定しておいたSchemeです。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebViewを閉じる |
| gamebase://getMaintenanceInfo | メンテナンス内容をWebPageに表示 |
| gamebase://getUserId          | 現在ログインしているゲームユーザーのユーザーIDを表示 |
| gamebase://goBack | WebView戻る |


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

## Toast

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知の表示中タイムアウトが発生しました。 |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
