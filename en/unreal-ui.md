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


## Terms

Shows the Terms and Conditions specified in the Gamebase Console.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

ShowTermsView API displays the terms and conditions window in WebView.
If you want to create your own terms and conditions window appropriate for the Game UI, call the QueryTerms API to load the terms and conditions set in the Gamebase console.
If users agree to the terms and conditions, please use the UpdateTerms API to send the user consent of each item to the Gamebase server.

### ShowTermsView

Shows the terms and conditions window on the screen.
If users agree to the terms and conditions, register the user consent data in the server.
If users agree to the terms and conditions, calling the ShowTermsView API again will immediately return the success callback without displaying the terms and conditions window.
However, if the Terms and Conditions reconsent requirement has been changed to **Required**, the terms and conditions window is displayed until users agree again to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * FGamebasePushConfiguration is null if the terms and conditions window is not displayed. (If the terms and conditions window is displayed, a valid object is always returned.)
> * FGamebasePushConfiguration.pushEnabled value is always true.
> * If FGamebasePushConfiguration is not null, call IGamebase::Get().GetPush().RegisterPush() **after login**.

#### Optional parameter

* GamebaseTermsConfiguration: Using the GamebaseTermsConfiguration object, you can change settings such as whether to forcibly display the terms and conditions agreement window.
* callback: Uses a callback to inform the user when the terms and conditions window closes after agreeing to it. The GamebaseResponse.DataContainer object which comes as a callback can be converted to GamebaseResponse.Push.PushConfiguration. The converted object can be used in the Gamebase.Push.RegisterPush API after login.

**FGamebaseTermsConfiguration** 
 
| API | Mandatory(M) / Optional(O) | Description | 
| --- | --- | --- | 
| forceShow | O | If the user agreed to the terms, calling the showTermsView API again will not display the terms and conditions window, but ignore it and force the display of the terms and conditions window.<br>**default** : false | 
 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowTermsView(const FGamebaseDataContainerDelegate& onCallback);
void ShowTermsView(const FGamebaseDataContainerDelegate& onCallback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| LAUNCHING\_SERVER\_ERROR | 2001 | This error occurs when the items downloaded from the launching server do not have any information about the terms and conditions.<br/>This is not a usual case, and you should contact the Gamebase personnel. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | Unfinished terms & conditions WebView has been called again. |
| WEBVIEW\_TIMEOUT | 7002 | Timed out while displaying the terms and conditions WebView. |
| WEBVIEW\_HTTP\_ERROR | 7003 | An HTTP error has occurred while opening the terms and conditions WebView. |

**Example**

```cpp
void Sample::ShowTermsView()
{
    FGamebaseTermsConfiguration configuration { true };

    IGamebase::Get().GetTerms().ShowTermsView(configuration,
        FGamebaseDataContainerDelegate::CreateLambda([=](const FGamebaseDataContainer* dataContainer, const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView succeeded."));
                
                // Save the 'PushConfiguration' and use it for RegisterPush() after Login().
                savedPushConfiguration = FGamebasePushConfiguration::From(dataContainer);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView failed. (error: %d)"), error->code);
            }
        })
    );
}

void Sample::AfterLogin()
{
    // Call RegisterPush with saved PushConfiguration.
    if (savedPushConfiguration != null)
    {
        Gamebase.Push.RegisterPush(savedPushConfiguration, (error) =>
        {
            ...
        });
    }
}
```


### QueryTerms

Gamebase displays the terms and conditions with a simple WebView.
If you want to create the terms and conditions appropriate for the game UI, call the QueryTerms API to download the terms and conditions information set in the Gamebase Console for later use.

Calling it after login also lets you see if the game user has agreed to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * The required items with GamebaseResponse.Terms.ContentDetail.required set to true are not stored in the Gamebase server; therefore, false is always returned for the agreed value.
>     * It is because there is no point in storing the required items since they are always stored as true.
> * The user consent for receiving the push notification is not stored in the Gamebase server either; therefore, the agreed value is always returned as false.
>     * To see if the user has agreed to receive push, use the Gamebase.Push.QueryPush API.
> * If you do not touch the 'Terms and Conditions settings' in the console, **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs when you call the queryTerms API from the device with the country code different from the terms and conditions language.
>     * If you complete the 'Terms and Conditions settings' in the console or if **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs, please make sure the terms and conditions are not displayed.

#### Required parameter
* callback: Uses a callback to inform the user about the API call result. With the GamebaseResponse.Terms.QueryTermsResult that comes as callback, you can acquire the terms and conditions information set in the console.
 

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
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | Terms & conditions information is not registered with the console. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | Terms & conditions information appropriate for the device's country code is not registered with the console. |

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
| termsSeq             | int32                           | KEY for the entire terms and conditions.<br/>This value is required when calling updateTerms API.          |
| termsVersion         | FString                         | T&C version.<br/>This value is required when calling updateTerms API.              |
| termsCountryType     | FString                         | Terms and conditions type.<br/> - KOREAN: Korean terms and conditions <br/> - GDPR: European terms and conditions <br/> - ETC: Other countries' terms and conditions         |
| contents             | TArray<FGamebaseTermsContent>   | Terms and conditions info          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int32                 | T&C KEY         | 
| name                 | FString               | T&C Name         |
| required             | bool                  | Whether agreement is required         |
| agreePush            | FString               | Whether to accept advertisement push.<br/> - NONE: Do not accept <br/> - ALL: Accept all <br/> - DAY: Accept push notification during daytime<br/> - NIGHT: Accept push notification during night time          |
| agreed               | bool                  | User's consent to the terms and conditions           |
| node1DepthPosition   | int32                 | Primary item exposure sequence.           |
| node2DepthPosition   | int32                 | Secondary item exposure sequence.<br/> If none, -1           |
| detailPageUrl        | FString               | URL for the full terms and conditions.<br/> If none, null. |


### UpdateTerms

If the UI has been created manually with the terms and conditions info downloaded from the QueryTerms API,
please use the UpdateTerms API to send the game user's agreement history to the Gamebase server.

It can be used to terminate the agreement to optional terms and conditions as well as to revise the agreed T&C clauses.


> <font color="red">[Caution]</font><br/>
>
> Push accept status is not stored in the Gamebase server.
> Push accept status should be stored by calling the Gamebase.Push.RegisterPush API **after login**.
>

#### Required parameter
* configuration: Information of optional T&C of users who will be registered on the server.
 
#### Optional parameter

* callback: Registers the information of optional terms and conditions on the server, and uses the callback to inform the user.


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
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | Unregistered terms and conditions Seq value has been set. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |


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
| termsVersion         | **M**                      | FString                    | T&C version.<br/>The queryTerms API must be called to pass the downloaded value.   |
| termsSeq             | **M**                      | int32                       | KEY for the entire terms and conditions.<br/>The queryTerms API must be called to pass the downloaded value.             |
| contents             | **M**                      | List< Content > | Info on whether user agrees to the optional terms and conditions  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int32                | KEY for optional terms and conditions      |
| agreed               | **M**                      | bool               | Info on whether user agrees to optional terms and conditions  |


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
