## Game > Gamebase > User Guide for Unreal SDK > UI

## GameNotice

This feature displays registered notices with images on the console.

![GameNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/gameNotice_guide_001.png)

### Open GameNotice

Show the game notice on the screen.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS


```cpp
void OpenGameNotice(const FGamebaseErrorDelegate& Callback);
```

**ErrorCode**

| Error                                | Error Code | Description |
|--------------------------------------| --- | --- |
| NOT\_INITIALIZED                     | 1 | Gamebase is not initialized. |
| UI\_GAME\_NOTICE\_FAIL\_INVALID\_URL            | 6941 | Failed to generate the game notice URL. |
| UI\_GAME\_NOTICE\_FAIL\_ANDROID\_DUPLICATED\_VIEW | 6942 | The game notice was called again before the previous popup was closed.  |
| WEBVIEW\_TIMEOUT                | 7002 | Webview display timed out (10 seconds). |
| WEBVIEW\_HTTP\_ERROR                 | 7003 |  An HTTP error occurred in the WebView. |
| WEBVIEW\_UNKNOWN\_ERROR           | 7999 | Unknown WebView error occurred. |

**Example**

```cpp
void USample::OpenGameNotice()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetGameNotice()->OpenGameNotice(
        FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error) {
            // Called when the entire imageNotice is closed.
            ...
        })
    );
}
```

## ImageNotice

You can pop up a notice to users after registering an image to the console.

![ImageNotice Example](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/DevelopersGuide/imageNotice-guide-landscape-en_v3.png)

### Show ImageNotices

Show the image notice on the screen.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void ShowImageNotices(FGamebaseImageNoticeConfiguration& Configuration, const FGamebaseErrorDelegate& CloseCallback);
void ShowImageNotices(FGamebaseImageNoticeConfiguration& Configuration, const FGamebaseErrorDelegate& CloseCallback, const FGamebaseImageNoticeEventDelegate& EventCallback);
```

**Example**

```cpp
void USample::ShowImageNotices(int32 ColorR, int32 ColorG, int32 ColorB, int32 ColorA, int64 TimeOut)
{
    FGamebaseImageNoticeConfiguration Configuration;
    Configuration.BackgroundColor = FColor(ColorR, ColorG, colorB, colorA);
    Configuration.TimeOut = TimeOut;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetImageNotice()->ShowImageNotices(Configuration,
        FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error) {
            // Called when the entire imageNotice is closed.
            ...
        }),
        FGamebaseSchemeEventDelegate::CreateLambda([](const FString& Scheme, const FGamebaseError* Error) {
            // Called when custom event occurred.
            ...
        })
    );
}
```

#### FGamebaseImageNoticeConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| BackgroundColor          | Color                                    | Background color           |
| TimeOut                  | int64        | Image notice max loading time (in millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

You can call the closeImageNotices API to terminate all image notices currently being displayed.

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

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
| bForceShow | O | If the user agreed to the terms, calling the showTermsView API again will not display the terms and conditions window, but ignore it and force the display of the terms and conditions window.<br>**default** : false | 
| bEnableFixedFontSize | O | Determines whether to fix the font size of the terms and conditions window.<br>**default** : false<br/>**Android Only** |
 
**FGamebaseShowTermsViewResult**

| Parameter              | Values                          | Description         |
| ---------------------- | --------------------------------| ------------------- |
| bIsTermsUIOpened        | bool                            | **true** : The terms window was displayed and the user agreed, and the terms window has been closed.<br>**false** : The terms window was not displayed and the terms window has been closed because the user has already agreed to the terms.        |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowTermsView(const FGamebaseDataContainerDelegate& Callback);
void ShowTermsView(const FGamebaseTermsConfiguration& Configuration, const FGamebaseDataContainerDelegate& Callback);
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
void USample::ShowTermsView()
{
    FGamebaseTermsConfiguration Configuration { true };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->ShowTermsView(Configuration,
        FGamebaseDataContainerDelegate::CreateLambda([](const FGamebaseDataContainer* DataContainer, const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(LogTemp, Display, TEXT("ShowTermsView succeeded."));
                
                const auto result = FGamebaseShowTermsResult::From(DataContainer);
                if (result.IsValid())
                {
                    // Save the 'PushConfiguration' and use it for RegisterPush() after Login().
                    SavedPushConfiguration = FGamebasePushConfiguration::From(DataContainer);
                }
            }
            else
            {
                UE_LOG(LogTemp, Display, TEXT("ShowTermsView failed. (Error: %d)"), Error->Code);
            }
        })
    );
}

void USample::AfterLogin()
{
    // Call RegisterPush with saved PushConfiguration.
    if (SavedPushConfiguration != null)
    {
        Gamebase.Push.RegisterPush(SavedPushConfiguration, (Error) =>
        {
            ...
        });
    }
}
```


### QueryTerms

Gamebase displays the terms and conditions with a simple WebView.
If you want to create the terms and conditions appropriate for the game UI, call the QueryTerms API to download the terms and conditions information set in the Gamebase Console for later use.

The "optional" terms items will return the user's consent status when queried after login. However, the consent status for "required" items will always be returned as false.

> <font color="red">[Caution]</font><br/>
>
> * If a required item has GamebaseResponse.Terms.ContentDetail.required set to true, the consent status is not stored on the Gamebase server, so the agreed value will always be returned as false.
>     * Since users cannot proceed with the game or log in without agreeing to the required terms, if the terms popup is closed and the user is logged in, it is considered that they have already agreed to the required items. Therefore, there is no need to store the consent status for required items for logged-in users, as they are assumed to have already provided consent.
> * The user consent for receiving the push notification is not stored in the Gamebase server either; therefore, the agreed value is always returned as false.
>     * To see if the user has agreed to receive push, use the Gamebase.Push.QueryPush API.
> * If you do not touch the 'Terms and Conditions settings' in the console, **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs when you call the queryTerms API from the device with the country code different from the terms and conditions language.
>     * If you complete the 'Terms and Conditions settings' in the console or if **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs, please make sure the terms and conditions are not displayed.

#### Required parameter
* callback: Uses a callback to inform the user about the API call result. With the GamebaseResponse.Terms.QueryTermsResult that comes as callback, you can acquire the terms and conditions information set in the console.
 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void QueryTerms(const FGamebaseQueryTermsResultDelegate& Callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | Terms & conditions information is not registered with the console. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | Terms & conditions information appropriate for the device's country code is not registered with the console. |

**Example**

```cpp
void USample::QueryTerms()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->QueryTerms(
        FGamebaseQueryTermsResultDelegate::CreateLambda([](const FGamebaseQueryTermsResult* Data, const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(LogTemp, Display, TEXT("QueryTerms succeeded."));
            }
            else
            {
                UE_LOG(LogTemp, Display, TEXT("QueryTerms failed. (Error: %d)"), Error->Code);
            }
        })
    );
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| TermsSeq             | int32                           | KEY for the entire terms and conditions.<br/>This value is required when calling updateTerms API.          |
| TermsVersion         | FString                         | T&C version.<br/>This value is required when calling updateTerms API.              |
| TermsCountryType     | FString                         | Terms and conditions type.<br/> - KOREAN: Korean terms and conditions <br/> - GDPR: European terms and conditions <br/> - ETC: Other countries' terms and conditions         |
| Contents             | TArray<FGamebaseTermsContent>   | Terms and conditions info          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| TermsContentSeq      | int32                 | T&C KEY         | 
| Name                 | FString               | T&C Name         |
| bRequired             | bool                  | Whether agreement is required         |
| AgreePush            | FString               | Whether to accept advertisement push.<br/> - NONE: Do not accept <br/> - ALL: Accept all <br/> - DAY: Accept push notification during daytime<br/> - NIGHT: Accept push notification during night time          |
| bAgreed               | bool                  | User's consent to the terms and conditions           |
| Node1DepthPosition   | int32                 | Primary item exposure sequence.           |
| Node2DepthPosition   | int32                 | Secondary item exposure sequence.<br/> If none, -1           |
| DetailPageUrl        | FString               | URL for the full terms and conditions.<br/> If none, null. |


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

* Configuration: Information of optional T&C of users who will be registered on the server.
 
#### Optional parameter

* Callback: Registers the information of optional terms and conditions on the server, and uses the callback to inform the user.


**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void UpdateTerms(const FGamebaseUpdateTermsConfiguration& Configuration, const FGamebaseErrorDelegate Callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | Unregistered terms and conditions Seq value has been set. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |


**Example**

```cpp
void USample::UpdateTerms(int32 TermsSeq, const FString& TermsVersion, int32 TermsContentSeq, bool bAgreed)
{
    TArray<FGamebaseTermsContent> Contents;
    Contents.Add(FGamebaseTermsContent { TermsContentSeq, bAgreed });
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->UpdateTerms(
        FGamebaseUpdateTermsConfiguration { TermsSeq, TermsVersion, Contents },
        FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(LogTemp, Display, TEXT("UpdateTerms succeeded."));
            }
            else
            {
                UE_LOG(LogTemp, Display, TEXT("UpdateTerms failed. (Error: %d)"), Error->Code);
            }
        })
    );
}
```

#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| TermsVersion         | **M**                      | FString                    | T&C version.<br/>The queryTerms API must be called to pass the downloaded value.   |
| TermsSeq             | **M**                      | int32                       | KEY for the entire terms and conditions.<br/>The queryTerms API must be called to pass the downloaded value.             |
| Contents             | **M**                      | List< Content > | Info on whether user agrees to the optional terms and conditions  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| TermsContentSeq      | **M**                      | int32                | KEY for optional terms and conditions      |
| bAgreed               | **M**                      | bool               | Info on whether user agrees to optional terms and conditions  |

### IsShowingTermsView

Determines whether the current terms and conditions window is being displayed on the screen.

**API**

```cpp
bool IsShowingTermsView();
```

**Example**

```cpp
void USample::IsShowingTermsView()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    bool isShowingTermsView = Subsystem->GetTerms()->IsShowingTermsView();
    UE_LOG(LogTemp, Display, TEXT("IsShowingTermsView : %s"), isShowingTermsView ? TEXT("true") : TEXT("false"));
}
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
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void ShowWebView(const FString& Url, const FGamebaseWebViewConfiguration& Configuration, FGamebaseErrorDelegate& CloseCallback, const TArray<FString>& SchemeList, const FGamebaseSchemeEventDelegate& onSchemeEvent);
```

**Example**
```cpp
void USample::ShowWebView(const FString& Url)
{
    FGamebaseWebViewConfiguration Configuration;
    Configuration.Title = TEXT("Title");

    TArray<FString> SchemeList{ TEXT("customScheme://openBrowser") };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->ShowWebView(Url, Configuration,
        FGamebaseErrorDelegate::CreateLambda([](const FGamebaseError* Error) {
            Result(ANSI_TO_TCHAR(__FUNCTION__), TEXT("Close webview"));
        }),
        SchemeList,
        FGamebaseSchemeEventDelegate::CreateLambda([](const FString& Scheme, const FGamebaseError* Error) {
        if (Gamebase::IsSuccess(Error))
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), true, *FString::Printf(TEXT("Scheme= %s"), *Scheme));
        }
        else
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), false, GamebaseJsonUtil::UStructToJsonObjectString(*Error));
        }
    }));
}
```


#### FGamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| Title                    | FString                                   | Title of WebView                  |
| Orientation              | GamebaseScreenOrientation::Unspecified    | Unspecified (**default**)            |
|                          | GamebaseScreenOrientation::Portrait       | Portrait mode                       |
|                          | GamebaseScreenOrientation::Landscape      | Landscape mode                       |
|                          | GamebaseScreenOrientation::LandscapeReverse | Rotate portrait mode 180 degrees            |
| ContentMode              | GamebaseWebViewContentMode::Recommended      | Browser recommended by the current platform (**default**)   |
|                          | GamebaseWebViewContentMode::Mobile           | Mobile browser            |
|                          | GamebaseWebViewContentMode::Desktop          | Desktop browser          |
| NavigationColor          | FColor                                   | Color of Navigation Bar<br>**default**: FColor(18, 93, 230, 255)        |
| NavigationTitleColor     | FColor                                   | Color of Navigation Bar Title<br>**default**: FColor::White          |
| NavigationIconTintColor  | TOptional&lt;FColor&gt;                  | Color of Navigation Bar Title Icon Tint |
| NavigationBarHeight      | height                                   | Height of Navigation Bar<br>**Android Only**                 |
| bIsNavigationBarVisible  | true or false                            | Activate or deactivate Navigation Bar<br>**default**: true    |
| bIsBackButtonVisible     | true or false                            | Activate or deactivate Go Back button<br>**default**: true   |
| BackButtonImageResource  | ID of resource                           | Image of Go Back button         |
| CloseButtonImageResource | ID of resource                           | Image of Close button            |
| bEnableFixedFontSize      | true or false                            | Fix the font size for the terms and condtion window .<br>**default**: false<br>**Only for Android**     |
| bRenderOutSideSafeArea    | true or false                            | Rendering to Cutout Area Ignoring SafeArea.<br>**default**: false<br>**Only for Android**   |
| CutoutColor              | TOptional<FColor>                        | Cutout area background color outside of SafeArea<br>**Only for Android**                            |

> [TIP]
>
> In iPadOS 13 or later, WebView is the default desktop mode.
> You can use the contentMode =`GamebaseWebViewContentMode::MOBILE` setting to switch to the mobile mode.

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
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void CloseWebView();
```

**Example**CloseWebview
```cpp
void USample::CloseWebView()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->CloseWebView();
}
```


## Open External Browser

With the following API, you can open an external browser. The URL sent to parameters must be valid. 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void OpenWebBrowser(const FString& url);
```

**Example**
```cpp
void USample::OpenWebBrowser(const FString& Url)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->OpenWebBrowser(Url);
}
```


## Alert

Shows notifications of the system. 
Callback registration is also available on the system notification. 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowAlert(const FString& Title, const FString& Message);
void ShowAlert(const FString& Title, const FString& Message, const FGamebaseAlertCloseDelegate& CloseCallback);
```

**Example**
```cpp
void USample::ShowAlert(const FString& Title, const FString& Message)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowAlert(Title, Message);
}

void USample::ShowAlertEvent(const FString& Title, const FString& Message)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowAlert(Title, Message, FGamebaseAlertCloseDelegate::CreateLambda([]()
    {
        UE_LOG(LogTemp, Display, TEXT("ShowAlert ButtonClick."));
    }));
}
```

## Toast

With the following API, it gets easy to display messages. 

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowToast(const FString& Message, EGamebaseToastExposureTime ExposureTimeType);
```

**Example**
```cpp
void USample::ShowToast(const FString& Message, EGamebaseToastExposureTime ExposureTimeType)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowToast(Message, ExposureTimeType);
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | Timed out while displaying image notice. |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown (undefined) error.  |

* See the following document for the entire error codes. 
    * [Error Codes](./error-code/#client-sdk)
