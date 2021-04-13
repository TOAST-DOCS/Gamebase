## Game > Gamebase > iOS SDK 使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户发送推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

将图片通知显示在页面上。

#### Required参数
* viewController : 为显示图片通知的ViewController。
 
#### Optional参数 
* configuration : 使用TCGBImageNoticeConfiguration可更改背景色等的图片通知设置。
* closeCompletion : 关闭图片通知时通过回调通知用户。
* schemeEvent : 点击图片时, 将设置在控制台的payload作为回调通知。 

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

将自定义图片通知显示在页面上。
通过TCGBImageNoticeConfiguration可创建自定义图片通知。

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
| backgroundColor                  | UIColor     | 图片通知背景色<br/>**default**: [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5]         |
| timeoutMS                  | long        | 图片通知最大加载时间(单位 : millisecond)<br/>**default**: 5000                     |
| enableAutoCloseByCustomScheme    | YES or NO   | 发生custom scheme事件时关闭所有通知或显示下一个通知。<br/>**default**: YES         |


### Close ImageNotices

通过调用closeImageNotices API，可以关闭所有当前显示的图片通知。

```objectivec
- (void)closeImageNotices {
    [TCGBImageNotice closeImageNoticesWithViewController:self];
}
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED(1) | 未初始化Gamebase |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT(6901) | 提示图片通知弹窗时，因发生超时错误，要强制关闭所有的弹窗。|

## WebView

Gamebase支持基本WebView。<br/>
与WebView有关的资源(图片、html及其他资源)已被包括在Gamebase.bundle中。

### Show WebView

显示WebView。<br/>

##### Required 参数
* url : 作为参数发送的url必须是有效值。
* viewController : View Controller用于显示WebView。

##### Optional参数
* configuration : 使用TCGBWebViewConfiguration可更改WebView的布局。
* closeCompletion : 关闭WebView时通过回调通知用户。 
* schemeList : 指定用户需要接收的自定义Scheme列表。
* schemeEvent : 将包含”指定为schemeList的自定义Scheme"的url作为回调通知。

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

#### 自定义WebView
显示自定义WebView。<br/>可以用GamebaseWebViewConfiguration设置自定义WebView。

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

您可以用amebase WebView加载的网页中利用scheme使用某些功能或更改网页内容。

##### Predefined Custom Scheme

在Gamebase中指定的架构。<br/>

| scheme               | 作用                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | 关闭WebView             |
| gamebase://goback    | 返回上一页          |
| gamebase://getuserid | 查看当前登录的用户ID |
| gamebase://openbrowser?link={URLEncodedURL} | 使用外部浏览器打开link参数的URL。<br/>URLEncodedURL : 使用外部浏览器要打开的URL。<br/>要对URL进行解码。|




#### User Custom Scheme

您可以通过在Gamebase中指定模式名称和块来添加所需的功能。


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
| navigationBarTitle                     | string                                   | WebView的标题        |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | 不明               |
|                                        | TCGBWebViewOrientationPortrait           | 纵向模式              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | 纵向旋转180度     |
|                                        | TCGBWebViewOrientationLandscapeRight     | 横向模式              |
|                                        | TCGBWebViewOrientationLandscapeLeft      |横向旋转180度     |
| contentMode                            | TCGBWebViewContentModeRecommended        | 当前平台推荐的浏览器             |
|                                        | TCGBWebViewContentModeMobile             | Mobile浏览器            |
|                                        | TCGBWebViewContentModeDesktop            | Desktop浏览器          |
| navigationBarColor                     | UIColor                                  | 导航栏颜色         |
| isBackButtonVisible                    | YES or NO                                | 返回按钮有效或无效 |
| navigationBarHeight                    | CGFloat                                  | 导航栏高度     |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 返回按钮图标      |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 关闭按钮图标          |

> [TIP]
>
> 在iPadOS 13以上，WebView基本上是Desktop模式。
> contentMode=可以通过‘’TCGBWebViewContentModeMobile‘’设置，转换Mobile模式。



### Close WebView
通过以下API，可以关闭正在显示的WebView。

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

可通过调用以下API打开外部浏览器。传送到参数的URL应该为有效的值。 

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

无法显示系统通知

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

#### Types of ActionSheet
1. 基本上提供有“Cancel”按键的ActionSheet。
2. 可以在‘’blocks”中注册用户的AlertAction。

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
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901      | 显示图片通知时发生了超时错误。|
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 未知错误(未定义的错误)。|

* 所有错误代码，请参考以下文档。
    * [错误代码](./error-code/#client-sdk)
