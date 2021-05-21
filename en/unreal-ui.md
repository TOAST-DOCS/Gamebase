## Game > Gamebase > User Guide for Unreal SDK > UI

## ImageNotice

You can pop up a notice to users after registering an image to the console.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

Show the image notice on the screen.

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
| colorR                   | 0 - 255                                    | Navigation bar color R            |
| colorG                   | 0 - 255                                    | Navigation bar color G                |
| colorB                   | 0 - 255                                    | Navigation bar color B                |
| colorA                   | 0 - 255                                    | Navigation bar color Alpha                |
| timeOut                  | int64        | Image notice max loading time (in millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

You can call the closeImageNotices API to terminate all image notices currently being displayed.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


## Webview

### Show WebView

Shows WebView.<br/>

##### Required Parameters 
* url: An URL sent to parameters must be valid. 

##### Optional Parameters (Currently Required, which is to be changed to Optional in a later version)
* configuration: WebView layout could be changed with GamebaseWebViewConfiguration.
* closeCallback: Notify the user of the closure of WebView with a callback. 
* schemeList: Specify the list of customized scheme as the user needs.  
* schemeEvent: Notify the URL including customized scheme which is specified by schemeList.

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
| title                    | FString                                   | Title of WebView                  |
| orientation              | GamebaseScreenOrientation::Unspecified    | Unspecified |
|                          | GamebaseScreenOrientation::Portrait       | Portrait Mode                       |
|                          | GamebaseScreenOrientation::Landscape      | Landscape Mode                       |
|                          | GamebaseScreenOrientation::LandscapeReverse | Rotate portrait mode 180 degrees            |
| contentMode              | GamebaseWebViewContentMode::Recommended        | Browser recommended by the current platform    |
|                          | GamebaseWebViewContentMode::Mobile             | Mobile browser            |
|                          | GamebaseWebViewContentMode::Desktop            | Desktop browser          |
| colorR                   | 0~255                                    | Color R of navigation bar            |
| colorG                   | 0~255                                    | Color G of navigation bar                |
| colorB                   | 0~255                                    | Color B of navigation bar                |
| colorA                   | 0~255                                    | Color alpha of navigation bar                |
| buttonVisible            | true or false                            | Activate or deactivate the back button          |
| barHeight                | height                                   | Height of navigation bar                  |
| backButtonImageResource  | ID of resource                           | The back button image                |
| closeButtonImageResource | ID of resource | The close button image |
| url | "http://" or "https://" or "file://" | Web URL |

> [TIP]
>
> In iPadOS 13 or later, WebView is the default desktop mode.
> You can use the contentMode =`GamebaseWebViewContentMode.MOBILE` setting to switch to the mobile mode.

#### Predefined Custom Scheme

Refers to the scheme specified by Gamebase.

| Scheme | Usage |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | Close WebView  |
| gamebase://getMaintenanceInfo | Show maintenance on WebPage  |
| gamebase://getUserId          | Show user ID of the currently logged-in game user  |
| gamebase://goBack | Go Back of WebView |


### Close WebView

With the following API, you can close the Webview of the current display.

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

With the following API, you can open an external browser. The URL sent to parameters must be valid. 

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

Shows notifications of the system. 
Callback registration is also available on the system notification. 

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

With the following API, it gets easy to display messages. 

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | Timed out while displaying image notice. |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown (undefined) error.  |

* See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)
