## Game > Gamebase > iOS SDK 使用指南 > UI

## WebView

Gamebase支持基本WebView。<br/>
<br/>
Gamebase.bundle包含与WebView相关的资源（图像和html，其他资源）。

### Show WebView

显示WebView。<br/>

##### Required 参数
* url : 作为参数发送的url必须是有效值。
* viewController : View Controller用于显示WebView。

##### 可选参数
* configuration : 可以使用GamebaseWebViewConfiguration更改WebView的布局。
* closeCompletion : WebView关闭时通过回调通知用户。
* schemeList : 指定用户想要接收的自定义SchemeList。
* schemeEvent : 用schemeList指定的包含自定义Scheme的url，作为回调通知。


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


#### 自定义WebView
显示自定义WebView。<br/>可以用GamebaseWebViewConfiguration设置自定义WebView。

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

您可以用amebase WebView加载的网页中利用scheme使用某些功能或更改网页内容。

##### Predefined Custom Scheme

在Gamebase中指定的架构。<br/>

| scheme               | 作用                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | 关闭WebView             |
| gamebase://goBack    | 返回WebView         |
| gamebase://getUserId | 显示当前登录用户的ID |



#### User Custom Scheme

您可以通过在Gamebase中指定模式名称和块来添加所需的功能。


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
| navigationBarTitle                     | string                                   | WebView的标题        |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | 不明               |
|                                        | TCGBWebViewOrientationPortrait           | 纵向模式              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | 纵向旋转180度     |
|                                        | TCGBWebViewOrientationLandscapeRight     | 横向模式              |
|                                        | TCGBWebViewOrientationLandscapeLeft      |横向旋转180度     |
| navigationBarColor                     | UIColor                                  | 导航栏颜色         |
| isBackButtonVisible                    | YES or NO                                | 返回按钮有效或无效 |
| navigationBarHeight                    | CGFloat                                  | 导航栏高度     |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 返回按钮图标      |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 关闭按钮图标          |


### Close WebView
通过以下API，可以关闭正在显示的WebView。

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

可以使用以下API打开外部浏览器。作为参数传递的URL必须是有效值。

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

可以显示系统提醒。<br/>
iOS 8或更高版本上可以运行的UIAlertControlle，和iOS 8以下的版本可以运行的UIAlertView可进行内部处理。<br/>

#### Types of Alert
1. 只提供一个“确定”按钮，单击确定按钮将调用completion。
2. 只提供一个“确定”按钮，并且不提供completion。

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

可以使用以下API轻松显示 [Android toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html)消息。<br/>
可以设置简单的消息和显示的时间。

```objectivec
- (void)showToastMessage:(id)sender {
    // 显示消息3秒钟 (deprecated API)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE" duration:3];
    
    // 显示较长时间消息（3.5秒）
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum long" length:GamebaseToastLengthLong]; 
    
    // 显示较短时间消息（2秒）
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum short" length:GamebaseToastLengthShort];
}
```


## Error Handling


| Error                           | Error Code | 描述                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误)。|

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
