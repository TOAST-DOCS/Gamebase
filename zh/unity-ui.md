## Game > Gamebase > Unity Developer's Guide > UI

## ImageNotice

You can pop up a notice to users after registering an image to the console.

![ImageNotice Example](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/DevelopersGuide/imageNotice-guide-landscape-en_v3.png)

### Show ImageNotices

Show the image notice on the screen.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowImageNotices(GamebaseRequest.ImageNotice.Configuration configuration, GamebaseCallback.ErrorDelegate closeCallback, GamebaseCallback.GamebaseDelegate<string> eventCallback = null)
```

**Example**

```cs
public void ShowImageNotices()
{
    Gamebase.ImageNotice.ShowImageNotices(
        null,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

### Custom ImageNotices

Pops up a customized image notice on the screen.
You can use GamebaseRequest.ImageNotice.Configuration to create the customized image notice.

**Example**

```cs
public void ShowImageNotices(int colorR = 0 , int colorG = 0, int colorB = 0, int colorA = 128, long timeOut = 5000)
{
    GamebaseRequest.ImageNotice.Configuration configuration = new GamebaseRequest.ImageNotice.Configuration();
    configuration.colorR = colorR;
    configuration.colorG = colorG;
    configuration.colorB = colorB;
    configuration.colorA = colorA;
    configuration.timeOut = timeOut;

    Gamebase.ImageNotice.ShowImageNotices(
        configuration,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

#### GamebaseRequest.ImageNotice.Configuration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| colorR                   | 0~255                                    | Background color R            |
| colorG                   | 0~255                                    | Background color G                |
| colorB                   | 0~255                                    | Background color B                |
| colorA                   | 0~255                                    | Background color Alpha                |
| timeoutMS                | long        | Image notice max loading time (in millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

You can call the closeImageNotices API to terminate all image notices currently being displayed.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseImageNotices()
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

#### Optional parameter

* GamebaseTermsConfiguration : Using the GamebaseTermsConfiguration object, you can change settings such as whether to display the forced terms and conditions agreement window.
* callback: Uses a callback to inform the user when the terms and conditions window closes after agreeing to it. The GamebaseResponse.DataContainer object which comes as a callback can be converted to GamebaseResponse.Push.PushConfiguration. The converted object can be used in the Gamebase.Push.RegisterPush API after login.


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowTermsView(GamebaseCallback.GamebaseDelegate<GamebaseResponse.DataContainer> callback)
static void ShowTermsView(GamebaseRequest.Terms.GamebaseTermsConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.DataContainer> callback)
```

**GamebaseRequest.Terms.GamebaseTermsConfiguration** 
 
| API | Mandatory(M) / Optional(O) | Description | 
| --- | --- | --- | 
| forceShow | O | If the user agreed to the terms, calling the showTermsView API again will not display the terms and conditions window, but ignore it and force the display of the terms and conditions window.<br>**default** : false | 
| enableFixedFontSize | O | Sets whether to fix the font size of the terms and conditions window.<br>**default** : false<br/>**Android Only** |
 

**GamebaseResponse.Terms.ShowTermsViewResult**

| Parameter              | Values                          | Description         |
| ---------------------- | --------------------------------| ------------------- |
| isTermsUIOpened        | bool                            | **true**: The terms and conditions window was displayed, and it was closed after the user agreed to the terms and conditions.<br>**false**: The user has already agreed to the terms and conditions, so the terms and conditions window was closed without being displayed.        |
| pushConfiguration      | GamebaseResponse.Push.PushConfiguration           | If isTermsUIOpened is **true** and you have added consent to receive push notifications to the terms and conditions, then pushConfiguration will always have a valid object.<br>Otherwise it will be **null**.<br>When pushConfiguration is valid, the value of pushConfiguration.pushEnabled is always **true**. |

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

```cs
static GamebaseResponse.Push.PushConfiguration savedPushConfiguration = null;

public void SampleShowTermsView()
{
    var configuration = new GamebaseRequest.Terms.GamebaseTermsConfiguration
    {
        forceShow = true
    };

    Gamebase.Terms.ShowTermsView(configuration, (data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("ShowTermsView succeeded.");

            GamebaseResponse.Terms.ShowTermsViewResult result = GamebaseResponse.Terms.ShowTermsViewResult.From(data);
            
            // Save the 'PushConfiguration' and use it for Gamebase.Push.RegisterPush() after Gamebase.Login().
            savedPushConfiguration = result.pushConfiguration;
            
            // Wheter the TermsUI was displayed.
            bool isTermsUIOpened = result.isTermsUIOpened;
        }
        else
        {
            Debug.Log(string.Format("ShowTermsView failed. error:{0}", error));
        }
    });
}

public void AfterLogin()
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
> * The standalone platform does not support push-related functions, so be careful not to expose these terms in the game UI.

#### Required parameter
* callback: Uses a callback to inform the user about the API call result. With the GamebaseResponse.Terms.QueryTermsResult that comes as callback, you can acquire the terms and conditions information set in the console.
 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void QueryTerms(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Terms.QueryTermsResult> callback)
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | Terms & conditions information is not registered with the console. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | Terms & conditions information appropriate for the device's country code is not registered with the console. |

**Example**

```cs
public void SampleQueryTerms()
{
    Gamebase.Terms.QueryTerms((data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("QueryTerms succeeded.");
        }
        else
        {
            Debug.Log(string.Format("QueryTerms failed. error:{0}", error));
        }
    });
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| termsSeq             | int                             | KEY for the entire terms and conditions.<br/>This value is required when calling updateTerms API.          |
| termsVersion         | string                          | T&C version.<br/>This value is required when calling updateTerms API.              |
| termsCountryType     | string                          | Terms and conditions type.<br/> - KOREAN: Korean terms and conditions <br/> - GDPR: European terms and conditions <br/> - ETC: Other countries' terms and conditions         |
| contents             | List< ContentDetail > | Terms and conditions info          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | T&C KEY         | 
| name                 | string                | T&C Name         |
| required             | bool                  | Whether agreement is required         |
| agreePush            | string                | Whether to accept advertisement push.<br/> - NONE: Do not accept <br/> - ALL: Accept all <br/> - DAY: Accept push notification during daytime<br/> - NIGHT: Accept push notification during night time          |
| agreed               | bool                  | User's consent to the terms and conditions           |
| node1DepthPosition   | int                   | Primary item exposure sequence.           |
| node2DepthPosition   | int                   | Secondary item exposure sequence.<br/> If none, -1           |
| detailPageUrl        | string                | URL for the full terms and conditions.<br/> If none, null. |


### UpdateTerms

If the UI has been created manually with the terms and conditions info downloaded from the QueryTerms API,
please use the UpdateTerms API to send the game user's agreement history to the Gamebase server.

It can be used to terminate the agreement to optional terms and conditions as well as to revise the agreed T&C clauses.


> <font color="red">[Caution]</font><br/>
>
> Push accept status is not stored in the Gamebase server.
> Push accept status should be stored by calling the Gamebase.Push.RegisterPush API **after login**.
> In the standalone platform, you must call the API after logging in. If the API is called without logging in, a NOT_LOGGED_IN error is returned.
>

#### Required parameter
* configuration: Information of optional T&C of users who will be registered on the server.
 
#### Optional parameter

* callback: Registers the information of optional terms and conditions on the server, and uses the callback to inform the user.


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void UpdateTerms(GamebaseRequest.Terms.UpdateTermsConfiguration configuration, GamebaseCallback.ErrorDelegate callback)
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| NOT\_LOGGED_IN | 2 | Login is required. (Only Standalone) |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | Unregistered terms and conditions Seq value has been set. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |


**Example**

```cs
public void SampleUpdateTerms()
{            
    List<GamebaseRequest.Terms.Content> list = new List<GamebaseRequest.Terms.Content>();
    list.Add(new GamebaseRequest.Terms.Content()
    {
        termsContentSeq = 0,
        agreed = true
    });
    
    Gamebase.Terms.UpdateTerms(
        new GamebaseRequest.Terms.UpdateTermsConfiguration()
        {
            termsSeq = 0,
            termsVersion = "1.0.0",
            contents = list
        }
        ,
        (error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("UpdateTerms succeeded.");
            }
            else
            {
                Debug.Log(string.Format("UpdateTerms failed. error:{0}", error));
            }
        });
}
```


#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | string                    | T&C version.<br/>The queryTerms API must be called to pass the downloaded value.   |
| termsSeq             | **M**                      | int                       | KEY for the entire terms and conditions.<br/>The queryTerms API must be called to pass the downloaded value.             |
| contents             | **M**                      | List< Content > | Info on whether user agrees to the optional terms and conditions  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | KEY for optional terms and conditions      |
| agreed               | **M**                      | bool               | Info on whether user agrees to optional terms and conditions  |


### IsShowingTermsView

Determines whether the terms and conditions window is currently displayed or not.

**API**

```cs
static bool IsShowingTermsView()
```

**Example**

```cs
public void SampleIsShowingTermsView()
{
    bool isShowingTermsView = Gamebase.Terms.IsShowingTermsView();
    Debug.Log(string.Format("isShowingTermsView: {0}", isShowingTermsView));
}
```

## Webview

### Show WebView

Shows a WebView.<br/>

##### Required Parameters
* URL: The url delivered as a parameter should be valid.

##### Optional Parameters
* configuration: Changes WebView layout by using GamebaseWebViewConfiguration.
* closeCallback: Notifies users when a WebView is closed.
* schemeList: Specifies the list of customized schemes a user wants.
* schemeEvent: Notifies url including customized scheme specified by the schemeList with a callback.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> (Standalone) supports login with WebViewAdapter and does not block events entered through UI when WebView is open.

**Example**
```cs
private void SchemeEvent(string scheme, GamebaseError error)
{
    Debug.Log(string.Format("[SchemeEvent] scheme:{0}", scheme));
}

private void CloseCallback(GamebaseError error)
{
    Debug.Log("CloseCallback");
}
    
public void ShowWebView()
{
    GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = new GamebaseRequest.Webview.GamebaseWebViewConfiguration();
     configuration.title = "Title";
     configuration.orientation = GamebaseScreenOrientation.Portrait;
     configuration.colorR = 128;
     configuration.colorG = 128;
     configuration.colorB = 128;
     configuration.colorA = 255;
     configuration.barHeight = 40;
     configuration.isBackButtonVisible = true;
    
    var schemeList = new List<string>() { "customScheme://openBrowser" };
    
    Gamebase.Webview.ShowWebView("https://www.toast.com/", configuration, CloseCallback, schemeList, SchemeEvent);
}
```

#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | Title of WebView               |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | Unspecified (**default**)            |
|                          | GamebaseScreenOrientation.PORTRAIT       | Portait mode                    |
|                          | GamebaseScreenOrientation.LANDSCAPE      | Landscape mode                    |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | Reverse landscape     |
| contentMode<br>(Only for iOS )           | GamebaseWebViewContentMode.RECOMMENDED      | Browser recommended by the current platform (**default**)  |
|                          | GamebaseWebViewContentMode.MOBILE           | Mobile browser            |
|                          | GamebaseWebViewContentMode.DESKTOP          | Desktop browser          |
| colorR                   | 0~255                                    | Color of Navigation Bar: R<br>**default**: 18               |
| colorG                   | 0~255                                    | Color of Navigation Bar: G<br>**default**: 93               |
| colorB                   | 0~255                                    | Color of Navigation Bar: B<br>**default**: 230              |
| colorA                   | 0~255                                    | Color of Navigation Bar: Alpha<br>**default**: 255          |
| barHeight                | height                                   | Height of Navigation Bar<br>**Android Only**                 |
| isNavigationBarVisible   | true or false                            | Activate or deactivate Navigation Bar<br>**default**: true    |
| isBackButtonVisible      | true or false                            | Activate or deactivate Go Back Button<br>**default**: true   |
| backButtonImageResource  | ID of resource                           | Image of Go Back Button         |
| closeButtonImageResource | ID of resource                           | Image of Close Button             |
| enableFixedFontSize<br>(Only for Android)   | true or false              | Decide if the font size of the Terms window should be fixed.<br>**default**: false |
| renderOutSideSafeArea<br>(Only for Android) | true or false              | Decide whether to render outside Safe Area.<br>**default**: false |

> [TIP]
>
> In iPadOS 13 or later, WebView is the default desktop mode.
> You can use the contentMode =`GamebaseWebViewContentMode.MOBILE` setting to switch to the mobile mode.

#### Predefined Custom Scheme

This is the scheme that has been set by Gamebase.

| scheme | Usage |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | Close WebView |
| gamebase://goBack | Go back on WebView |
| gamebase://getUserId | Display the user ID of the currently logged in game user |
| gamebase://getMaintenanceInfo | Display maintenance information on WebPage |
| gamebase://showwebview?link={URLEncodedURL} | Open URL of the link parameter with WebView.<br>URLEncodedURL: URL to open with WebView.<br>Requires URL decoding. |
| gamebase://openbrowser?link={URLEncodedURL} | Open URL of the link parameter with an external browser<br/>URLEncodedURL: URL to open with an external browser<br/>URL decoding required |

### Close WebView

Close currently displayed WebView by using the following API.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseWebview()
```

**Example**CloseWebview
```cs
public void CloseWebview()
{
    Gamebase.Webview.CloseWebview();
}
```


## Open External Browser

Open an external browser by using the following API. The URL delivered as a parameter should be valid.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void OpenWebBrowser(string url)
```

**Example**
```cs
public void OpenWebBrowser(string url)
{
    Gamebase.Webview.OpenWebBrowser(url);
}
```


## Alert

Displays a system alert API.
You can also register a callback to the system alert.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowAlert(string title, string message)
static void ShowAlert(string title, string message, GamebaseCallback.VoidDelegate buttonCallback)
```

**Example**
```cs
public void ShowAlertD()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message"
    );
}

public void ShowAlertDialog()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message",
        () => {
                    Debug.Log("ButtonClick");
              }
    );
}
```

## Toast

Displays message easily, by using the following API.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowToast(string message, int duration)
static void ShowToast(string message, GamebaseUIToastType type)
```

**Example**
```cs
public void ShowToast(string message, int duration)
{
    Gamebase.Util.ShowToast(
        message,
        duration
        );
}
public void ShowToast(string message, GamebaseUIToastType type)
{
    Gamebase.Util.ShowToast(
        message,
        type
        );
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | Timed out while displaying image notice. |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)
