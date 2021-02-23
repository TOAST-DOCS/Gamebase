## Game > Gamebase > User Guide for Unreal SDK > UI

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
| title                    | string                                   | Title of WebView                 |
| orientation              | GamebaseScreenOrientation::UNSPECIFIED    | Unspecified |
|                          | GamebaseScreenOrientation::PORTRAIT       | Portrait Mode                       |
|                          | GamebaseScreenOrientation::LANDSCAPE      | Landscape Mode                      |
|                          | GamebaseScreenOrientation::LANDSCAPE_REVERSE | Rotate portrait mode 180 degrees              |
| colorR                   | 0~255                                    | Color alpha of navigation bar            |
| colorG                   | 0~255                                    | Color R of navigation bar                |
| colorB                   | 0~255                                    | Color G of navigation bar                |
| colorA                   | 0~255                                    | Color B of navigation bar                |
| buttonVisible            | true or false                            | Activate or deactivate the back button           |
| barHeight                | height                                   | Height of navigation bar                  |
| backButtonImageResource  | ID of resource                           | The back button image                     |
| closeButtonImageResource | ID of resource | The close button image  |
| url | "http://" or "https://" or "file://" | Web URL |

#### Predefined Custom Scheme

Refers to the scheme specified by Gamebase.

| Scheme | Usage |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | Close WebView  |
| gamebase://goBack | Go Back of WebView |
| gamebase://getUserId          | Show user ID of the currently logged-in game user  |
| gamebase://getMaintenanceInfo | Show maintenance on WebPage  |


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
| UI\_UNKNOWN\_ERROR | 6999       | Unknown (undefined) error.  |

* See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)
