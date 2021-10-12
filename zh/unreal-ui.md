## Game > Gamebase > Unreal SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

将图片通知显示在页面上。

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
| colorR                   | 0~255                                    | Navigation Bar颜色R            |
| colorG                   | 0~255                                    | Navigation Bar颜色G                |
| colorB                   | 0~255                                    | Navigation Bar颜色B                |
| colorA                   | 0~255                                    | Navigation Bar颜色Alpha                |
| timeOut                  | int64        | 图片通知最大加载时间(单位 : millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

通过调用closeImageNotices API，可关闭当前显示的所有图片通知。 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


## Webview

### Show WebView

显示WebView。<br/>

##### Required参数
* url : 作为参数发送的url必须是有效值。

##### Optional参数(是Require参数，但在以后的版本要更改为Optional)
* configuration : 通过使用GamebaseWebViewConfiguration可以更改WebView的布局。
* closeCallback : 关闭WebView时通过回调通知用户。
* schemeList : 指定用户需要接收的自定义Scheme列表。
* schemeEvent : 通过回调通知包含指定为schemeList的自定义Scheme的url。

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
| title                    | string                                   | WebView的标题                 |
| orientation              | GamebaseScreenOrientation::Unspecified    | 未指定 |
|                          | GamebaseScreenOrientation::Portrait       | 纵向模式                       |
|                          | GamebaseScreenOrientation::Landscape      | 横向模式                       |
|                          | GamebaseScreenOrientation::LandscapeReverse | 将横向模式旋转180度              |
| contentMode              | GamebaseWebViewContentMode::Recommended        | 当前平台推荐的浏览器    |
|                          | GamebaseWebViewContentMode::Mobile             | 移动浏览器            |
|                          | GamebaseWebViewContentMode::Desktop            | 桌面浏览器          |
| colorR                   | 0~255                                    | Navigation Bar颜色R            |
| colorG                   | 0~255                                    | Navigation Bar颜色G                |
| colorB                   | 0~255                                    | Navigation Bar颜色B                |
| colorA                   | 0~255                                    | Navigation Bar颜色Alpha                |
| buttonVisible            | true or false                            | 激活或不激活返回按钮          |
| barHeight                | height                                   | Navigation Bar高度                  |
| backButtonImageResource  | ID of resource                           | 返回按钮图片                |
| closeButtonImageResource | ID of resource | 关闭按钮图片 |
| url | "http://" or "https://" or "file://" | 网络URL |

> [TIP]
>
> iPadOS 13以上中的WebView基本上是桌面模式。
> 通过contentMode = ”GamebaseWebViewContentMode.MOBILE”设置可以更改移动模式。

#### Predefined Custom Scheme

是Gamebase指定的Scheme。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | 关闭WebView。|
| gamebase://getMaintenanceInfo | 在WebPage显示维护结果。 |
| gamebase://getUserId          | 显示当前登录的游戏用户的ID。 |
| gamebase://goBack | 返回到WebView的上一页。|


### Close WebView

可以通过调用以下API来关闭当前显示的WebView。

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

可以通过调用以下API打开外部浏览器。作为参数传送的url必须是有效值。

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

可以显示系统通知。
可以在系统通知注册回调。

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

通过调用以下API可轻松地显示消息。

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 显示图片通知时出现了超时错误。|
| UI\_UNKNOWN\_ERROR | 6999       | 是未知错误(是未定义的错误)。|

* 关于所有错误代码，请参考以下文档。 
    * [错误代码](./error-code/#client-sdk)
