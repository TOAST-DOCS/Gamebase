## Game > Gamebase > iOS Developer's Guide > UI

## ImageNotice

You can pop up a notice to users after registering an image to the console.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

Show the image notice on the screen.

#### Required parameter
* viewController: ViewController where the image notice is displayed.
 
#### Optional parameter
* configuration: Can change the image notice settings (e.g. background color) with TCGBImageNoticeConfiguration.
* closeCompletion: Informs the user with callback when the entire image notice ends.
* schemeEvent: Informs the payload which is registered in the console as callback when clicking the image.


```objectivec
- (void)showImageNotices {
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        // Called when the entire imageNotice is closed.
        NSLog(@"ImageNotices closed");
    };

    void(^schemeEvent)(NSString *, TCGBError *) = ^(NSString *payload , TCGBError *error) {
        // Called when image click event occurred.
        NSLog(@"Image click event occurred : %@", payload);
    };

    [TCGBImageNotice showImageNoticesWithViewController:self configuration:nil closeCompletion:closeCompletion schemeEvent:schemeEvent];
}
```


### Custom ImageNotices

Pops up a customized image notice on the screen.
You can use TCGBImageNoticeConfiguration to create a customized image notice.


```objectivec
- (void)showImageNotices {
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        // Called when the entire imageNotice is closed.
        NSLog(@"ImageNotices closed");
    };

    void(^schemeEvent)(NSString *, TCGBError *) = ^(NSString *payload , TCGBError *error) {
        // Called when image click event occurred.
        NSLog(@"Image click event occurred : %@", payload);
    };

    TCGBImageNoticeConfiguration *configuration = [[TCGBImageNoticeConfiguration] alloc] init];
    configuartion.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5];
    configuartion.timeoutInterval = 5000;
    configuartion.enableAutoCloseByCustomScheme = YES;

    [TCGBImageNotice showImageNoticesWithViewController:self configuration:configuration closeCompletion:closeCompletion schemeEvent:schemeEvent];
}
```


#### TCGBImageNoticeConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| backgroundColor                  | UIColor     | Image notice background color<br/>**default**: [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5]         |
| timeoutMS                  | long        | Image notice max loading time (in millisecond)<br/>**default**: 5000                     |
| enableAutoCloseByCustomScheme    | YES or NO   | Close all notifications when the custom scheme event occurs, or display next notification<br/>**default**: YES         |


### Close ImageNotices

You can call the closeImageNotices API to terminate all image notices currently being displayed.

```objectivec
- (void)closeImageNotices {
    [TCGBImageNotice closeImageNoticesWithViewController:self];
}
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | Performs a force shutdown of all popup windows because timeout has occurred while displaying the image notice popup. |



## Terms

Shows the Terms and Conditions specified in the Gamebase Console.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

showTermsView API displays the terms and conditions window in WebView.
If you want to create your own terms and conditions window appropriate for the Game UI, call the queryTerms API to load the terms and conditions set in the Gamebase console.
If users agree to the terms and conditions, please use the updateTerms API to send the user consent of each item to the Gamebase server.

### showTermsView

Shows the terms and conditions window on the screen.
If users agree to the terms and conditions, register the user consent data in the server.
If users agree to the terms and conditions, calling the showTermsView API again will immediately return the success callback without displaying the terms and conditions window.
However, if the Terms and Conditions reconsent requirement has been changed to **Required**, the terms and conditions window is displayed until users agree again to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * If you have added user consent for receiving push notification in the terms and conditions, you can create a TCGBPushConfiguration from TCGBDataContainer.
> * PushConfiguration is nil if the terms and conditions window is not displayed. (If the terms and conditions window is displayed, a valid object is always returned.)
> * PushConfiguration.pushEnabled value is always true.
> * If TCGBPushConfiguration is not nil, call [TCGBPush registerPushWithConfiguration:completion:] **after login**.
>

#### Required parameter
* viewController: The terms and conditions window is exposed in ViewController.
 
#### Optional parameter
* configuration: Using TCGBTermsConfiguration, you can change settings such as whether to forcibly display the terms and conditions agreement window.
* completion: Uses a callback to inform the user when the terms and conditions window closes after agreeing to it. The TCGBDataContainer object which comes as callback can be converted to TCGBPushConfiguration. The converted object can be used in the registerPush API after login.


**API**

```objectivec
+ (void)showTermsViewWithViewController:(UIViewController *)viewController
                             completion:(nullable void (^)(TCGBDataContainer * _Nullable dataContainer, TCGBError * _Nullable error))completion;
                             
+ (void)showTermsViewWithConfiguration:(TCGBTermsConfiguration *)configuration
                        viewController:(nullable UIViewController *)viewController
                            completion:(nullable void (^)(TCGBDataContainer * _Nullable dataContainer, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001 | This error occurs when the items downloaded from the launching server do not have any information about the terms and conditions.<br/>This is not a usual case, and you should contact the Gamebase personnel. |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |
| TCGB\_ERROR\_WEBVIEW\_TIMEOUT | 7002 | Timed out while displaying the terms and conditions WebView. |
| TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR | 7003 | An HTTP error has occurred while opening the terms and conditions WebView. |

**Example**

```objectivec
- (void)showTermsView {
    void(^completion)(TCGBDataContainer *, TCGBError *) = ^(TCGBDataContainer *dataContainer, TCGBError *error) {
        // Called when the entire termsView is closed.
        NSLog(@"TermsView closed");

        TCGBShowTermsViewResult *showTermsViewResult = [TCGBShowTermsViewResult fromDataContainer:dataContainer];

        // If the TCGBPushConfiguration is not null, 
        // save TCGBPushConfiguration and use it for registerPush after login.
        TCGBPushConfiguration *savedPushConfiguraiton = showTermsViewResult.pushConfiguration;

        // Wheter the TermsUI was displayed.
        BOOL isTermsUIOpened = showTermsViewResult.isTermsUIOpened;
    };

    [TCGBTerms showTermsViewWithViewController:self completion:completion];
}

- (void)afterLogin {
    // Call registerPush with saved TCGBPushConfiguration.
    if (savedPushConfiguration != nil) {
        [TCGBPush registerPushWithPushConfiguration:savedPushConfiguration completion:^(TCGBError* error) {
            ...
        }];
    }
}
```

**TCGBTermsConfiguration**

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| forceShow            | BOOL                            | Even after the user agreed to the terms and conditions, forcibly display the terms and conditions window.<br/>**default**: NO          |

**TCGBShowTermsViewResult**

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| isTermsUIOpened            | BOOL                            | Indicates whether the terms and conditions window was displayed on the screen.          |
| TCGBPushConfiguration      | TCGBPushConfiguration           | If you added consent to receive push notifications to the terms and conditions, this parameter has information on whether the user agrees to receive push notifications.    |

### queryTerms

Gamebase displays the terms and conditions with a simple WebView.
If you want to create the terms and conditions appropriate for the game UI, call the queryTerms API to download the terms and conditions information set in the Gamebase Console for later use.

Calling it after login also lets you see if the game user has agreed to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * The required items with TCGBTermsContentDetail.required set to true are not stored in the Gamebase server; therefore, false is always returned for the agreed value.
>     * It is because there is no point in storing the required items since they are always stored as true.
> * The user consent for receiving the push notification is not stored in the Gamebase server either; therefore, the agreed value is always returned as false.
>     * To see if the user has agreed to receive push, please check the [TCGBPush queryTokenInfoWithCompletion:] API.
> * If you do not touch the 'Terms and Conditions settings' in the console, **TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs when you call the queryTerms API from the device with the country code that is different from the terms and conditions language.
>     * If you complete the 'Terms and Conditions settings' in the console or if **TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs, please make sure the terms and conditions are not displayed.

#### Required parameter
* viewController: The top-level ViewController.
* completion: Uses a callback to inform users about the API call result. With the TCGBQueryTermsResult that comes as callback, you can acquire the terms and conditions information set in the console.
 

**API**

```objectivec
+ (void)queryTermsWithViewController:(UIViewController *)viewController
                          completion:(void (^)(TCGBQueryTermsResult * _Nullable queryTermsResult, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | Terms & conditions information is not registered with the console. |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | Terms & conditions information appropriate for the device's country code is not registered with the console. |

**Example**

```objectivec
- (void)queryTerms {
    void(^completion)(TCGBQueryTermsResult *, TCGBError *) = ^(TCGBQueryTermsResult *queryTermsResult, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            int termsSeq = queryTermsResult.termsSeq;
            NSString *termsVersion = queryTermsResult.termsVersion;
            ...    
        } else if (error.code == TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY) {
            // Another country device.
            // Pass the 'terms and contidions' step
        } else {
            // QueryTerms API Failed.
        }
    };

    [TCGBTerms queryTermsWithViewController:self completion:completion];
}
```

#### TCGBQueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| termsSeq             | int                             | KEY for the entire terms and conditions.<br/>This value is required when calling updateTerms API.          |
| termsVersion         | String                          | T&C version.<br/>This value is required when calling updateTerms API.              |
| termsCountryType     | String                          | Terms and conditions type.<br/> - KOREAN: Korean terms and conditions <br/> - GDPR: European terms and conditions <br/> - ETC: Other countries' terms and conditions         |
| contents             | Array< TCGBTermsContentDetail > | Terms and conditions info          |


#### TCGBTermsContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | T&C KEY         | 
| name                 | String                | T&C Name         |
| required             | BOOL                  | Whether agreement is required         |
| agreePush            | String                | Whether to accept advertisement push.<br/> - NONE: Do not accept <br/> - ALL: Accept all <br/> - DAY: Accept push notification during daytime<br/> - NIGHT: Accept push notification during night time          |
| agreed               | BOOL                  | User's consent to the terms and conditions           |
| node1DepthPosition   | int                   | Primary item exposure sequence.           |
| node2DepthPosition   | int                   | Secondary item exposure sequence.<br/> If none, -1           |
| detailPageUrl        | String                | Full terms and conditions text URL.<br/> Does not require filed if not enabled           |


### updateTerms

If the UI has been created manually with the terms and conditions info downloaded from the queryTerms API,
please use the updateTerms API to send the game user's agreement history to the Gamebase server.

It can be used to terminate the agreement to optional terms and conditions as well as to revise the agreed T&C clauses.


> <font color="red">[Caution]</font><br/>
>
> Push accept status is not stored in the Gamebase server.
> Push accept status should be stored by calling the [TCGBPush registerPushWithConfiguration:completion:] API **after login**.
>

#### Required parameter
* viewController: The top-level ViewController.
* configuration: Information of optional T&C of users who will be registered on the server.

#### Optional parameter

* completion: Registers the information of optional terms and conditions on the server, and notifies user with a callback.


**API**

```objectivec
+ (void)updateTermsWithViewController:(UIViewController *)viewController
                        configuration:(TCGBUpdateTermsConfiguration *)configuration
                           completion:(nullable void (^)(TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebase not initialized. |
| TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | Unregistered terms and conditions Seq value has been set. |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | The Terms API call has not been completed yet.<br/>Please try again later. |


**Example**

```objectivec
- (void)updateTerms {
    void(^completion)(TCGBError *) = ^(TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // UpdateTerms API Failed.
        }
    };

    TCGBTermsContent *termsContent = [TCGBTermsContent termsContentWithTermsContentSeq:12 agreed:YES];

    NSMutableArray *contents = [NSMutableArray array];
    [contents addObject:termsContent];

    TCGBUpdateTermsConfiguration *configuration = [TCGBUpdateTermsConfiguration configurationWithTermsVersion:@"1.2.3" termsSeq:1 contents:contents];

    [TCGBTerms updateTermsWithViewController:self configuration:configuration completion:completion];
}
```


#### TCGBUpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | String                    | T&C version.<br/>The queryTerms API must be called to pass the downloaded value.   |
| termsSeq             | **M**                      | int                       | KEY for the entire terms and conditions.<br/>The queryTerms API must be called to pass the downloaded value.             |
| contents             | **M**                      | Array< TCGBTermsContent > | Info on whether user agrees to the optional terms and conditions  |

#### TCGBTermsContent

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | KEY for optional terms and conditions      |
| agreed               | **M**                      | BOOL               | Info on whether user agrees to optional terms and conditions  |

### isShowingTermsView

Determines whether the terms and conditions window is currently displayed or not.

**API**

```objectivec
+ (void)isShowingTermsView;
```
**Example**

```objectivec
- (void)isShowingTermsView {
    BOOL isShowingTermsView = [TCGBTerms isShowingTermsView];   // YES or NO
}
```

## WebView

Gamebase supports a default WebView.<br/>
WebView-related resources (images, html, and others) are included to Gamebase.bundle.

### Show WebView

Shows a WebView.<br/>

##### Required Parameters
* url: The url delivered as a parameter should be valid.
* viewController: WebView is displayed on the View Controller.

##### Optional Parameters
* configuration: Changes WebView layout by using TCGBWebViewConfiguration.
* closeCompletion: Notifies users with a callback when a WebView is closed.
* schemeList: Specifies the list of customized schemes a user wants.
* schemeEvent: Notifies url including customized scheme specified by the schemeList with a callback.


```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        NSLog(@"WebView Close Event occured");
    };

    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:nil closeCompletion:closeCompletion schemeList:nil schemeEvent:nil];

}
```

![Webview Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

#### Custom WebView
Shows a customized WebView.<br/>Can configure a customzed WebView by using TCGBWebViewConfiguration.

```objectivec
- (void)showFixedOrientationWebView:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    TCGBWebViewConfiguration* config = [[TCGBWebViewConfiguration alloc] init];
    
    // Webview is fixed to Landscape mode
    config.orientationMask = TCGBWebViewOrientationLandscapeLeft | TCGBWebViewOrientationLandscapeRight;
    
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        NSLog(@"WebView Close Event occured");
    };

    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:config closeCompletion:closeCompletion schemeList:nil schemeEvent:nil];
}
```

```objectivec
// Configure Custom Style Configuration to All TCGBWebView Objects
- (void)configureWebViewStyle {
    // After this method is called, every webview(TCGBWebView) is shown with Landscape mode

    TCGBWebViewConfiguration *config = [[TCGBWebViewConfiguration alloc] init];
    config.orientationMask = TCGBWebViewOrientationLandscapeLeft | TCGBWebViewOrientationLandscapeRight;

    [TCGBWebView sharedTCGBWebView].defaultWebConfiguration = config;
}
```


#### Custom Scheme 

Can apply scheme to use specific functions on a webpage of Gamebase Webview or change content.

##### Predefined Custom Scheme

Gamebase has specified following schemes.<br/>

| scheme               | Usage                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | Close WebView             |
| gamebase://goback    | Go back from WebView          |
| gamebase://getuserid | Show ID of a user who is currently logged-in |
| gamebase://showwebview?link={URLEncodedURL} | Open the URL of the link parameter with the WebView.<br>URLEncodedURL: Column URL with the WebView.<br>Requires URL decoding. |
| gamebase://openbrowser?link={URLEncodedURL} | Open the URL of the link parameter with an external browser.<br/>URLEncodedURL: Column URL with the WebView.<br/>Requires URL decoding. |



#### User Custom Scheme

Can add customized functions by specifying scheme names and blocks in Gamebase.


```objectivec

- (void)setCustomSchemes {
    NSString* urlString = @"https://www.toast.com/";
    
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        NSLog(@"WebView Close Event occured");
    };

    NSArray *schemeList = @[@"mygame://test", @"mygame://opensomebrowser"];

    void(^schemeEvent)(NSString *, TCGBError *error) = ^(NSString *fullUrl, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == YES) {
            if ([@"mygame://test" isEqualToString:fullUrl]) {
                NSLog(@"mygame://test scheme event occurred");
            } else if ([@"mygame://opensomebrowser" isEqualToString:fullUrl]) {
                NSLog(@"mygame://opensomebrowser scheme event occurred");
            }
        }
    };

    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:config closeCompletion:closeCompletion schemeList:schemeList schemeEvent:schemeEvent];
}
```


#### TCGBWebViewConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| navigationBarTitle                     | string                                   | Title of WebView        |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | Unspecified                |
|                                        | TCGBWebViewOrientationPortrait           | Portrait mode              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | Reverse portrait      |
|                                        | TCGBWebViewOrientationLandscapeRight     | Landscape mode              |
|                                        | TCGBWebViewOrientationLandscapeLeft      | Reverse landscape     |
| contentMode                            | TCGBWebViewContentModeRecommended        | Browser recommended by the current platform (**default**)    |
|                                        | TCGBWebViewContentModeMobile             | Mobile browser            |
|                                        | TCGBWebViewContentModeDesktop            | Desktop browser          |
| navigationBarColor                     | UIColor                                  | Color of Navigation Bar<br/>**default**: [UIColor colorWithRed: 0.07 green: 0.36 blue: 0.90 alpha: 1.00]   |
| isBackButtonVisible                    | YES or NO                                | Activate or deactivate Go Back Button<br/>**default**: YES |
| isNavigationBarVisible                 | YES or NO                                | Show or hide Navigation Bar<br/>**default**: YES    |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | Image of Go Back Button       |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | Image of Close Button          |

> [TIP]
>
> In iPadOS 13 or later, WebView is the default desktop mode.
> You can use the contentMode=`TCGBWebViewContentModeMobile` setting to switch to the mobile mode.



### Close WebView
Close currently displayed WebView by using the following API.

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

Use the following API to open external browsers. The URL sent by parameters must be valid. 

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

Shows alerts on the system. <br/>

#### Types of Alert
1. Provides only one 'OK' button, and its click brings Completion.
2. Provides only one 'OK' button, which does not provide Completion.

```objectivec
// 1. Alert has completion
- (void)showAlertWithCompletion:(id)sender {
    [TCGBUtil showAlertWithTitle:@"TITLE" message:@"MESSAGE" completion:^{
        NSLog(@"Tapped OK Button.");
    }];
}

// 2. Alert without completion
- (void)showAlertWitoutCompletion:(id)sender {
    [TCGBUtil showAlertWithTitle:@"TITLE" message:@"MESSAGE"];
}
```

#### Types of ActionSheet
1. ActionSheet is provided along with the **Cancel** button. 
2. On **blocks**, user's AlerAction can be registered. 


```objectivec
// Create ActionSheet [OK, Detail, Cancel]
- (void)showActionSheet {
    NSMutableDictionary<NSString *, void(^)(UIAlertAction *)> *blocks = [NSMutableDictionary dictionary];
    
    void(^okActionHandler)(UIAlertAction *) = ^(UIAlertAction *action){
        NSLog(@"OK");
    };
    void(^detailActionHandler)(UIAlertAction *) = ^(UIAlertAction *action){
        NSLog(@"Detail");
    };

    // Add AlertAction(Title: "OK", Handler: okActionHandler)
    [blocks setValue:okActionHandler forKey:@"OK"];

    // Add AlertAction(Title: "Detail", Handler: detailActionHandler)
    [blocks setValue:detailActionHandler forKey:@"Detail"];
    
    [TCGBUtil showActionSheetWithTitle:@"TITLE" message:@"MESSAGE" blocks:blocks];
}
```


## Toast

Displays [Android Toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API.<br/>
Simple messages with display time can be set.

```objectivec
- (void)showToastMessage:(id)sender {
    // Show message for 3 seconds  (deprecated API)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE" duration:3];
    
    // Show message for long time (3.5 seconds)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum long" length:GamebaseToastLengthLong]; 
    
    // Show message for short time (2 seconds)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum short" length:GamebaseToastLengthShort];
}
```


## Error Handling


| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | Timed out while displaying image notice. |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)
