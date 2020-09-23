## Game > Gamebase > User Guide for Unreal SDK 사용 가이드 > UI

## Webview

### Show WebView

Shows WebView를 표시합니다.<br/>

##### Required Parameters 파라미터
* url : URLs sent to parameters must be valid. 파라미터로 전송되는 url은 유효한 값이어야 합니다.

##### Optional Parameters 파라미터 (현재는 Require 파라미터지만, 이후 버전에서 Optional로 변경 예정)
* configuration: WebView layout could be changed with GamebaseWebViewConfiguration으로 WebView의 레이아웃을 변경 할 수 있습니다.
* closeCallback: Notifies the closure of WebView to user by callback. 가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList: Specify the list of custom scheme as user needs.  사용자가 받고 싶은 커스텀 Scheme 목록을 지정합니다.
* schemeEvent: Notifies the URL including custom scheme which is specified by schemeList로 지정한 커스텀 Scheme을 포함하는 url을 콜백으로 알려 줍니다.

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
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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
| title                    | string                                   | Title of WebView의 제목                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | Unspecified 미지정 |
|                          | GamebaseScreenOrientation.PORTRAIT       | Portrait Mode 세로 모드                       |
|                          | GamebaseScreenOrientation.LANDSCAPE      | Landscape Mode 가로 모드                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | Rotate portrait mode 180 degrees가로 모드를 180도 회전              |
| colorR                   | 0~255                                    | Color alpha of navigation bar내비게이션 바 색상 Alpha            |
| colorG                   | 0~255                                    | Color R of navigation bar 내비게이션 바 색상 R                |
| colorB                   | 0~255                                    | Color G of navigation bar 내비게이션 바 색상 G                |
| colorA                   | 0~255                                    | Color B of navigation bar 내비게이션 바 색상 B                |
| buttonVisible            | true or false                            | Activate or deactivate the back button 뒤로 가기 버튼 활성 또는 비활성          |
| barHeight                | height                                   | Height of navigation bar 내비게이션 바 높이                  |
| backButtonImageResource  | ID of resource                           | The back button image 뒤로 가기 버튼 이미지                |
| closeButtonImageResource | ID of resource | The close button image 닫기 버튼 이미지 |
| url | "http://" or "https://" or "file://" | Web URL |

#### Predefined Custom Scheme

Refers to the scheme specified by Gamebase에서 지정해 놓은 Scheme 입니다.

| Scheme | Usage |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | Close WebView 닫기 |
| gamebase://goBack | Go Back of WebView 뒤로가기 |
| gamebase://getUserId          | Show user ID of the currently logged-in game user 현재 로그인되어 있는 게임 유저의 사용자 ID를 표시 |
| gamebase://getMaintenanceInfo | Show maintenance on WebPage 점검 내용을 WebPage에 표시 |


### Close WebView

With the following API, you can close the Webview in the current display다음 API를 이용하여 보여지고 있는 WebView를 닫을 수 있습니다.

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

With the following API, you can open an external browser. The URL sent to parameters must be valid. 다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 url은 유효한 값이어야 합니다.

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

Shows notifications of the system. 시스템 알림을 표시할 수 있습니다.
Callback registration is also available on the system notification. 시스템 알림에 콜백을 등록할 수도 있습니다.

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

다음 API를 사용하여 쉽게 메시지를 표시할 수 있습니다. With the following API, it gets easy to display messages. 

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
| UI\_UNKNOWN\_ERROR | 6999       | Unknown (undefined) error. 알수 없는 오류입니다(정의되지 않은 오류입니다). |

* See the following document for the entire error codes. 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Error Codes](./error-code/#client-sdk)
