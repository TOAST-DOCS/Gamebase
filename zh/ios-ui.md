## Game > Gamebase > iOS Developer's Guide > UI

## WebView

Gamebase supports a default WebView.<br/>
<br/>
WebView-related resources (images, html, and others) are included to Gamebase.bundle.

### Show WebView

Shows a WebView.<br/>

##### Required Parameters
* url : The url delivered as a parameter should be valid.
* viewController : WebView is displayed on the View Controller.

##### Optional Parameters
* configuration : Changes WebView layout by using GamebaseWebViewConfiguration.
* closeCompletion : Notifies users with a callback when a WebView is closed.
* schemeList : Specifies the list of customized schemes a user wants.
* schemeEvent : Notifies url including customized scheme specified by the schemeList with a callback.


```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView showWebViewWithURL:urlString 
                     viewController:self 
                      configuration:nil
                    closeCompletion:^(TCGBError *error) {
                        NSLog(@"WebView Close Event occured");
                    }
                         schemeList:@[@"gamebase://"]
                        schemeEvent:^(NSString *fullUrl, TCGBError *error) {
                            NSLog(@"WebView Event occured. Event Url : %@", fullUrl);
                        }
    ];

}
```


#### Custom WebView
Shows a customized WebView.<br/>
Can configure a customzed WebView by using TCGBWebViewConfiguration.

```objectivec
- (void)showFixedOrientationWebView:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    TCGBWebViewConfiguration* config = [[TCGBWebViewConfiguration alloc] init];
    // Webview is fixed to Landscape mode
    config.orientationMask = TCGBWebViewOrientationLandscapeLeft | TCGBWebViewOrientationLandscapeRight;
    
    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:config
                    closeCompletion:^(TCGBError *error){
                        NSLog(@"WebView Close Event occured");
                    }
                         schemeList:@[@"gamebase://"]
                        schemeEvent:^(NSString *fullUrl, TCGBError *error) {
                            NSLog(@"WebView Event occured. Event Url : %@", fullUrl);
                        }];
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
| gamebase://goBack    | Go back from WebView          |
| gamebase://getUserId | Show ID of a user who is currently logged-in |



#### User Custom Scheme

Can add customized functions by specifying scheme names and blocks in Gamebase.


```objectivec

- (void)setCustomSchemes {
    // reigster an scheme called 'gamebase://openSafari' to load an page has url
    [TCGBWebView addCustomScheme:@"gamebase://openSafari" block:^(UIViewController<TCGBWebViewDelegate> *viewController, TCGBWebURL *webURL) {
        NSLog(@"%@ called!", webURL.host);
        __block NSMutableString *url = [[NSMutableString alloc] init];
        // Parsing parameters
        [webURL.query enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
            if ([key caseInsensitiveCompare:@"url"] == NSOrderedSame) {
                url = obj;
            }
        }];
        
        // Open Safari Browser
        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:url] options:@{} completionHandler:^(BOOL success) {
            NSLog(@"Safari URL : %@", url);
        }];
    }];
}
```


#### TCGBWebViewConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| navigationBarTitle                     | string                                   | Title of WebView        |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | Unspecified                |
|                                        | TCGBWebViewOrientationPortrait           | Portrait Mode              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | Reverse Portrait      |
|                                        | TCGBWebViewOrientationLandscapeRight     | Landscape Mode              |
|                                        | TCGBWebViewOrientationLandscapeLeft      | Reverse Landscape     |
| navigationBarColor                     | UIColor                                  | Color of Navigation Bar         |
| isBackButtonVisible                    | YES or NO                                | Activate/Deactivate Go Back Button |
| navigationBarHeight                    | CGFloat                                  | Height of Navigation Bar         |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | Image of Go Back Button       |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | Image of Close Button          |


### Close WebView
Close currently displayed WebView by using the following API.

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

다음 API를 통하여 외부 브라우저를 열 수 있습니다. 파라미터로 전송되는 URL은 유효한 값이어야 합니다.

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

Displays a system alert.<br/>
Internally process UIAlertController for iOS 8 or higher and UIAlertView for below iOS 8 versions.<br/>

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
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes.
    * [Entire Error Codes](./error-code/#client-sdk)
