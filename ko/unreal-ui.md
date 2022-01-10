## Game > Gamebase > Unreal SDK 사용 가이드 > UI

## ImageNotice

콘솔에 이미지를 등록한 후 사용자에게 공지를 띄울 수 있습니다.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

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


## Terms

Gamebase 콘솔에 설정한 약관을 표시합니다.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

ShowTermsView API 는 웹뷰로 약관창을 표시해줍니다.
Game 의 UI 에 맞는 약관창을 직접 제작하고자 하는 경우에는 QueryTerms API 를 호출하여, Gamebase 콘솔에 설정한 약관 항목을 불러올 수 있습니다.
유저가 약관에 동의했다면 각 항목별 동의 여부를 UpdateTerms API 를 통해 Gamebase 서버로 전송하시기 바랍니다.

### ShowTermsView

약관창을 화면에 띄워 줍니다.
유저가 약관에 동의를 했을 경우, 동의 여부를 서버에 등록합니다.
약관에 동의했다면 ShowTermsView API 를 다시 호출해도 약관창이 표시되지 않고 바로 성공 콜백이 리턴됩니다.
단, Gamebase 콘솔에서 약관 재동의 항목을 **필요** 로 변경했다면 유저가 다시 약관에 동의할 때까지는 약관창이 표시됩니다.

> <font color="red">[주의]</font><br/>
>
> * FGamebasePushConfiguration 은 약관창이 표시되지 않은 경우에는 null입니다.(약관창이 표시되었다면 항상 유효한 객체가 리턴됩니다.)
> * FGamebasePushConfiguration.pushEnabled 값은 항상 true입니다.
> * FGamebasePushConfiguration 이 null 이 아니라면 **로그인 후에** IGamebase::Get().GetPush().RegisterPush() 를 호출하세요.

#### Optional 파라미터

* callback : 약관 동의 후 약관창이 종료될 때 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseResponse.DataContainer 객체는 GamebaseResponse.Push.PushConfiguration 변환해서 로그인 후 Gamebase.Push.RegisterPush API 에 사용할 수 있습니다.


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowTermsView(const FGamebaseDataContainerDelegate& onCallback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| LAUNCHING\_SERVER\_ERROR | 2001 | 런칭서버가 내려준 항목에 약관 관련 내용이 없는 경우에 발생하는 에러입니다.<br/>정상적인 상황이 아니므로 Gamebase 담당자에게 문의해주시기 바랍니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | 약관 웹뷰가 아직 종료되지 않았는데 다시 호출되었습니다. |
| WEBVIEW\_TIMEOUT | 7002 | 약관 웹뷰 표시 중 타임아웃이 발생했습니다. |
| WEBVIEW\_HTTP\_ERROR | 7003 | 약관 웹뷰 오픈 중 HTTP 에러가 발생하였습니다. |

**Example**

```cpp
void Sample::ShowTermsView()
{
    IGamebase::Get().GetTerms().ShowTermsView(
        FGamebaseDataContainerDelegate::CreateLambda([=](const FGamebaseDataContainer* dataContainer, const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView succeeded."));

                // If the 'FGamebasePushConfiguration' is not null,
                // save the 'FGamebasePushConfiguration' and use it for IGamebase::Get().GetPush().RegisterPush() after IGamebase::Get().Login().
                const auto pushConfiguration = FGamebasePushConfiguration::From(dataContainer);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView failed. (error: %d)"), error->code);
            }
        })
    );
}
```


### QueryTerms

Gamebase는 단순한 형태의 웹뷰로 약관을 표시합니다.
게임UI에 맞는 약관을 직접 제작하고자 하신다면, QueryTerms API 를 호출하여 Gamebase 콘솔에 설정한 약관 정보를 내려받아 활용하실 수 있습니다.

로그인 후에 호출하신다면 게임유저가 약관에 동의했는지 여부도 함께 확인할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * GamebaseResponse.Terms.ContentDetail.required 가 true 인 필수 항목은 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 필수 항목은 항상 true 로 저장될 수 밖에 없어서 저장하는 의미가 없기 때문입니다.
> * 푸시 수신 동의 여부도 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 유저의 푸시 수신 동의 여부는 Gamebase.Push.QueryPush API 를 통해 확인하시기 바랍니다.
> * 콘솔에서 '기본 약관 설정' 을 하지 않는 경우, 약관 언어와 다른 국가코드로 설정된 단말기에서 queryTerms API 를 호출하면 **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생합니다.
>     * 콘솔에서 '기본 약관 설정' 을 하거나, **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생했을때는 약관을 표시하지 않도록 처리하시기 바랍니다.

#### Required 파라미터
* callback : API 호출 결과를 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseResponse.Terms.QueryTermsResult 로 콘솔에 설정된 약관 정보를 얻을 수 있습니다.
 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cs
void QueryTerms(const FGamebaseQueryTermsResultDelegate& onCallback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 약관 정보가 콘솔에 등록되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 단말기 국가코드에 맞는 약관 정보가 콘솔에 등록되어 있지 않습니다. |

**Example**

```cpp
void Sample::QueryTerms()
{
    IGamebase::Get().GetTerms().QueryTerms(
        FGamebaseQueryTermsResultDelegate::CreateLambda([=](const FGamebaseQueryTermsResult* data, const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms failed. (error: %d)"), error->code);
            }
        })
    );
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| termsSeq             | int32                           | 약관 전체 KEY.<br/>updateTerms API 호출 시 필요한 값입니다.          |
| termsVersion         | FString                         | 약관 버전.<br/>updateTerms API 호출 시 필요한 값입니다.              |
| termsCountryType     | FString                         | 약관 타입.<br/> - KOREAN : 한국 약관 <br/> - GDPR : 유럽 약관 <br/> - ETC : 기타 약관         |
| contents             | TArray<FGamebaseTermsContent>   | 약관 항목 정보          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int32                 | 약관 항목 KEY         | 
| name                 | FString               | 약관 항목 이름         |
| required             | bool                  | 필수 동의 여부         |
| agreePush            | FString               | 광고성 푸시 동의 여부.<br/> - NONE : 동의 안함 <br/> - ALL : 전체 동의 <br/> - DAY : 주간 푸시 동의<br/> - NIGHT : 야간 푸시 동의          |
| agreed               | bool                  | 해당 약관 항목에 대한 유저 동의 여부           |
| node1DepthPosition   | int32                 | 1단계 항목 노출 순서.           |
| node2DepthPosition   | int32                 | 2단계 항목 노출 순서.<br/> 없을 경우 -1           |
| detailPageUrl        | FString               | 약관 자세히 보기 URL.<br/> 없을 경우 null. |


### UpdateTerms

QueryTerms API 로 내려받은 약관 정보로 UI 를 직접 제작했다면,
게임유저가 약관에 동의한 내역을 UpdateTerms API 를 통해 Gamebase 서버로 전송하시기 바랍니다.

선택 약관 동의를 취소하는 것과 같이, 약관에 동의했던 내역을 변경하는 목적으로도 활용하실 수 있습니다.


> <font color="red">[주의]</font><br/>
>
> 푸시 수신 동의 여부는 Gamebase 서버에 저장되지 않습니다.
> 푸시 수신 동의 여부는 **로그인 후에** Gamebase.Push.RegisterPush API 를 호출해서 저장하세요.
>

#### Required 파라미터
* configuration : 서버에 등록할 유저의 선택 약관 정보입니다.
 
#### Optional 파라미터

* callback : 선택 약관 정보를 서버에 등록 후 사용자에게 콜백으로 알려줍니다.


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void UpdateTerms(const FGamebaseUpdateTermsConfiguration& configuration, const FGamebaseErrorDelegate onCallback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 등록되지 않은 약관 Seq 값을 설정하였습니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |


**Example**

```cpp
void Sample::UpdateTerms(int32 termsSeq, const FString& termsVersion, int32 termsContentSeq, bool agreed)
{
    TArray<FGamebaseTermsContent> contents;
    contents.Add(FGamebaseTermsContent { termsContentSeq, agreed });
    
    IGamebase::Get().GetTerms().UpdateTerms(
        FGamebaseUpdateTermsConfiguration { termsSeq, termsVersion, contents },
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms failed. (error: %d)"), error->code);
            }
        })
    );
}
```


#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | FString                    | 약관 버전.<br/>queryTerms API를 호출해서 내려받았던 값을 전달해야 합니다.   |
| termsSeq             | **M**                      | int32                       | 약관 전체 KEY.<br/>queryTerms API를 호출해서 내려받았던 값을 전달해야 합니다.             |
| contents             | **M**                      | List< Content > | 선택 약관 유저 동의 정보  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int32                | 선택 약관 항목 KEY      |
| agreed               | **M**                      | bool               | 선택 약관 항목 동의 여부  |


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
| title                    | FString                                   | WebView의 제목                 |
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

## Toast

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
