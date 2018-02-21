## Game > Gamebase > iOS Developer's Guide > UI


## WebView

Gamebase supports basic WebView and the user can configure the style: full screen or popup. <br/>

<br/>

WebView-related resources (images, html, and others) are included to Gamebase.bundle. 

### Browser Style WebView

Supports Full-screen WebView. <br/>
The full-screen style (browser) displays a navigation bar, as well as Close and Go Back buttons. You can set a title on the navigation bar. 


```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    [TCGBWebView showWebBrowserWithURL:@"http://cloud.toast.com" viewController:self];
}
```



### Popup Style WebView

Supports pop-up WebView. <br/>
The pop-up style displays a modal view on an existing screen, with the background covered by a transparent mask view. 


```objectivec
// Show Popup Style WebView
- (void)showPopupWebView:(id)sender {
    [TCGBWebView showPopupWithURL:@"http://cloud.toast.com" viewController:self];
}
```

### Custom WebView
Displays customized WebView. <br/>Customized WebView can be created by using TCGBWebViewConfiguration. 

```objectivec
- (void)showFixedOrientationWebView:(id)sender {
	NSString* urlString = @"https://www.toast.com/";
	TCGBWebViewConfiguration* config = [[TCGBWebViewConfiguration alloc] init];
    // WebView is fixed to Landscape mode
    config.orientationMask = TCGBWebViewOrientationLandscapeLeft | TCGBWebViewOrientationLandscapeRight;
    // Change color of Navigation Bar to blue
    [configuration setNavigationBarColor:[UIColor blueColor]];
    // Change height of Navigation Bar to 50.0
    [configuration setNavigationBarHeight:50.0];
    
    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:config];
}
```

```objectivec
// Configure Custom Style Configuration to All TCGBWebView Objects
- (void)configureWebViewStyle {
    // After this method is called, every webview(TCGBWebView) is shown with popup style.

    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    [configuration setStyle:TCGBWebViewLaunchPopUp];    //or TCGBWebViewLaunchFullScreen

    [TCGBWebView sharedTCGBWebView].defaultWebConfiguration = configuration;
}
```

### Custom Scheme 

Can apply scheme to use specific functions on a webpage of Gamebase WebView or change content. 

#### Predefined Custom Scheme

Gamebase has specified following schemes: <br/>

| Scheme               | Usage                                    |
| -------------------- | ---------------------------------------- |
| gamebase://dismiss   | Close WebView                            |
| gamebase://goBack    | Go back from WebView                     |
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

### Custom WebView with Local URL
Displays HTML files at a local directory on a customized WebView.<br/>Customized WebView can be created by using TCGBWebViewConfiguration.

```objectivec
- (IBAction)clickGoButton:(id)sender {
    NSString *urlString = @"file://here.html"
    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    configuration.style = _style;
    configuration.orientationMask = _orientationMask;
    configuration.navigationBarColor = [UIColor redColor];
    configuration.navigationBarTitle = @"Loading from Local WebPage";
    
    [TCGBWebView showWebViewWithLocalURL:urlString bundle:nil viewController:self configuration:configuration];

}
```



### TCGBWebViewConfiguration

| Parameter                              | Values                                   | Description                        |
| -------------------------------------- | :--------------------------------------- | :--------------------------------- |
| navigationBarTitle                     | string                                   | Title of WebView                   |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | Unspecified                        |
|                                        | TCGBWebViewOrientationPortrait           | Vertical Mode                      |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | Turn Upside Down                   |
|                                        | TCGBWebViewOrientationLandscapeRight     | Horizontal Mode                    |
|                                        | TCGBWebViewOrientationLandscapeLeft      | Turn Left to Right                 |
| navigationBarColor                     | UIColor                                  | Color of Navigation Bar            |
| isBackButtonVisible                    | YES or NO                                | Activate/Deactivate Go Back Button |
| navigationBarHeight                    | CGFloat                                  | Height of Navigation Bar           |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | Image of Go Back Button            |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | Image of Close Button              |



## Alert

Displays system alerts. <br/>
Internally process UIAlertController for iOS 8 or higher and UIAlertView for below iOS 8 versions. <br/>

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
	// Show message for 3 seconds (deprecated API)
	[TCGBUtil showToastWithMessage:@"TOAST MESSAGE" duration:3];
    
    // Show message for long time (3.5 seconds)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum long" length:GamebaseToastLengthLong]; 
    
    // Show message for short time (2 seconds)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum short" length:GamebaseToastLengthShort];
}
```


## Error Handling


| Error                           | Error Code | Description                      |
| ------------------------------- | ---------- | -------------------------------- |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes: 
  - [Entire Error Codes](./error-codes#client-sdk)