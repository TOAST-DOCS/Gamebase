## Game > Gamebase > Unreal SDK 사용 가이드 > UI

## ImageNotice

콘솔에 이미지를 등록한 후 사용자에게 공지를 띄울 수 있습니다.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

이미지 공지를 화면에 띄워 줍니다.

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
| colorR                   | 0~255                                    | 내비게이션 바 색상 R            |
| colorG                   | 0~255                                    | 내비게이션 바 색상 G                |
| colorB                   | 0~255                                    | 내비게이션 바 색상 B                |
| colorA                   | 0~255                                    | 내비게이션 바 색상 Alpha                |
| timeOut                  | int64        | 이미지 공지 최대 로딩 시간 (단위 : millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

closeImageNotices API를 호출하여 현재 표시 중인 이미지 공지를 모두 종료할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


## Webview

### Show WebView

WebView를 표시합니다.<br/>

##### Required 파라미터
* url: 파라미터로 전송되는 url은 유효한 값이어야 합니다.

##### Optional 파라미터 (현재는 Require 파라미터지만, 이후 버전에서 Optional로 변경 예정)
* configuration: GamebaseWebViewConfiguration으로 WebView의 레이아웃을 변경할 수 있습니다.
* closeCallback: WebView가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList: 사용자가 받고 싶은 커스텀 Scheme 목록을 지정합니다.
* schemeEvent: schemeList로 지정한 커스텀 Scheme을 포함하는 url을 콜백으로 알려 줍니다.

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
| title                    | string                                   | WebView의 제목                 |
| orientation              | GamebaseScreenOrientation::Unspecified    | 미지정 |
|                          | GamebaseScreenOrientation::Portrait       | 세로 모드                       |
|                          | GamebaseScreenOrientation::Landscape      | 가로 모드                       |
|                          | GamebaseScreenOrientation::LandscapeReverse | 가로 모드를 180도 회전              |
| contentMode              | GamebaseWebViewContentMode::Recommended        | 현재 플랫폼 추천 브라우저    |
|                          | GamebaseWebViewContentMode::Mobile             | 모바일 브라우저            |
|                          | GamebaseWebViewContentMode::Desktop            | 데스크탑 브라우저          |
| colorR                   | 0~255                                    | 내비게이션 바 색상 R            |
| colorG                   | 0~255                                    | 내비게이션 바 색상 G                |
| colorB                   | 0~255                                    | 내비게이션 바 색상 B                |
| colorA                   | 0~255                                    | 내비게이션 바 색상 Alpha                |
| buttonVisible            | true or false                            | 뒤로 가기 버튼 활성 또는 비활성          |
| barHeight                | height                                   | 내비게이션 바 높이                  |
| backButtonImageResource  | ID of resource                           | 뒤로 가기 버튼 이미지                |
| closeButtonImageResource | ID of resource | 닫기 버튼 이미지 |
| url | "http://" or "https://" or "file://" | 웹 URL |

> [TIP]
>
> iPadOS 13 이상에서 WebView는 기본적으로 데스크탑 모드입니다.
> contentMode =`GamebaseWebViewContentMode.MOBILE` 설정으로 모바일 모드로 변경할 수 있습니다.

#### Predefined Custom Scheme

Gamebase에서 지정해 놓은 Scheme입니다.

| scheme | 용도 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebView 닫기 |
| gamebase://getMaintenanceInfo | 점검 내용을 WebPage에 표시 |
| gamebase://getUserId          | 현재 로그인된 게임 유저의 사용자 ID를 표시 |
| gamebase://goBack | WebView 뒤로 가기 |


### Close WebView

다음 API를 이용해 보이는 WebView를 닫을 수 있습니다.

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

다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 url은 유효한 값이어야 합니다.

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

시스템 알림을 표시할 수 있습니다.
시스템 알림에 콜백을 등록할 수도 있습니다.

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

다음 API를 사용하여 쉽게 메시지를 표시할 수 있습니다.

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 이미지 공지 표시 중 타임아웃이 발생했습니다. |
| UI\_UNKNOWN\_ERROR | 6999       | 알수 없는 오류입니다(정의되지 않은 오류입니다). |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
