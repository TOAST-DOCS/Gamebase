## Game > Gamebase > iOS SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

在页面上显示图片通知。

#### Required参数
* viewController : 为显示图片通知的ViewController。
 
#### Optional参数 
* configuration : 通过使用TCGBImageNoticeConfiguration可更改背景色等的图片通知设置。
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

在页面上显示自定义图片通知。
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

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 由于显示图片通知弹窗时出现超时错误，所有弹窗都将关闭。 |



## Terms

在Gamebase控制台中显示注册的条款。 

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

调用showTermsView API显示条款窗时通过Webview显示。 
如果要创建符合Game UI的条款窗，可通过调用queryTerms API显示Gamebase控制台中注册的条款项目。
若用户已同意条款，请将各项目的“同意与否”通过调用updateTerms API传送到Gamebase服务器。 

### showTermsView

在页面上显示条款窗。
用户已同意条款时，在服务器中注册“同意与否”。  
如果已同意条款，即使重新调用showTermsView API，也不显示条款窗，而直接返还“成功回调”。
但，如果在Gamebase控制台中将“重新同意条款”项目更改为**必须**，用户再次同意条款之前一直显示条款窗。

> <font color="red">[注意]</font><br/>
>
> * 如果在条款中已添加“是否同意接收推送”，可从TCGBDataContainer创建TCGBPushConfiguration。
> * 未显示条款窗时，PushConfiguration为nil。(显示条款窗时，始终返还有效的对象。) 
> * PushConfiguration.pushEnabled值始终为true。 
> * 如果TCGBPushConfiguration不是nil，请**登录后**调用[TCGBPush registerPushWithConfiguration:completion:] API。
>

#### Required参数
* viewController : 是显示条款窗的ViewController。 
 
#### Optional参数
* configuration : 可通过TCGBTermsConfiguration更改是否强制显示条款窗的设置。
* completion : 同意条款后关闭条款窗时，通过回调通知用户。可将作为回调返回的TCGBDataContainer对象变换为TCGBPushConfiguration，登录后用于调用registerPush API。

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
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001 | 是启动服务器返还的项目中不包含相关条款内容时出现的错误。<br/>出现此问题时，请联系Gamebase负责人员。 |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 上一次调用的Terms API未完成。<br/>请稍后再试。 |
| TCGB\_ERROR\_WEBVIEW\_TIMEOUT | 7002 | 显示条款Webview时出现超时错误。 |
| TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR | 7003 | 打开条款Webview时出现HTTP错误。 |

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
| forceShow            | BOOL                            | 即使同意条款也将强制显示条款窗。<br/>**default**: NO          |

**TCGBShowTermsViewResult**

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| isTermsUIOpened            | BOOL                            | 显示是否在页面上已显示条款窗。          |
| TCGBPushConfiguration      | TCGBPushConfiguration           | 如果已在条款和条件中添加了“是否同意接受推送”，则具有关于是否同意接受推送的信息。    |

### queryTerms

Gamebase通过Webview以简单形式显示条款。 
如果要创建符合游戏UI的条款窗，可通过调用queryTerms API，获取在Gamebase控制台中注册的条款信息后使用。

如果登录后调用，则可确认游戏用户是否同意条款。 

> <font color="red">[注意]</font><br/>
>
> * 因Gamebase服务器不保存TCGBTermsContentDetail.required为true的必要项目，agreed值将始终返回为false。 
>     * 这是因为必要项目将始终保存为true，不需要保存。
> * 因“是否接收推送”没有被存储在gamebase服务器中，agreed值将始终返回为false。
>     * 如需查看用户“是否接收推送”，请调用[TCGBPush queryTokenInfoWithCompletion:] API 。 
> * 如果在控制台中未设置“基本条款”的状态下，使用与条款语言不同的国家代码在设置的终端机上调用queryTerms API，则将出现**TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误。
>     * 在控制台中设置“基本条款”或出现**TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误时，请不要显示条款。

#### Required参数
* viewController : 是最上级的ViewController。
* completion : 通过回调通知用户API的调用结果。通过作为回调被返回的TCGBQueryTermsResult，可获取在控制台中设置的条款信息。
  

**API**

```objectivec
+ (void)queryTermsWithViewController:(UIViewController *)viewController
                          completion:(void (^)(TCGBQueryTermsResult * _Nullable queryTermsResult, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 |  在控制台中未注册条款信息。 |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 在控制台中未注册符合终端机国家代码的条款信息。 |

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
| termsSeq             | int                             | 所有条款KEY<br/>是调用updateTerms API时的必须值。         |
| termsVersion         | String                          | 条款版本<br/>是调用updateTerms API时的必须值。              |
| termsCountryType     | String                          | 条款类型<br/> - KOREAN : 韩国条款 <br/> - GDPR : 欧洲条款 <br/> - ETC : 其他条款         |
| contents             | Array< TCGBTermsContentDetail > | 条款项目信息          |


#### TCGBTermsContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 条款项目KEY         | 
| name                 | String                | 条款项目名称         |
| required             | BOOL                  | 是否必须同意         |
| agreePush            | String                | 是否同意接收广告性推送通知<br/> - NONE : 不同意。<br/> - ALL : 全部同意。<br/> - DAY : 同意白天接收推送通知。<br/> - NIGHT : 同意夜间接收推送通知。          |
| agreed               | BOOL                  | 用户是否同意相关条款项目            |
| node1DepthPosition   | int                   | 显示第1阶段项目的顺序             |
| node2DepthPosition   | int                   | 显示第2阶段项目的顺序<br/> 没有 -1           |
| detailPageUrl        | String                | 查看条款URL详情<br/> 未设置时没有字段。           |

   
### updateTerms

如果使用调用queryTerms API后被返回的条款信息创建了UI，
请将游戏用户同意条款的记录通过updateTerms API传送到Gamebase服务器。

不仅可用于取消“可选条款同意”，也可用于修改同意条款的记录。
 

> <font color="red">[注意]</font><br/>
>
> Gamebase服务器基本上不保存“是否同意接收推送”。
> 当保存是否同意接收推送时，**登录后**通过调用[TCGBPush registerPushWithConfiguration:completion:] API来保存。
>

#### Required参数
* viewController : 是最上级ViewController。
* configuration : 要向服务器注册的用户的可选条款信息。
 
#### Optional参数

* completion : 在服务器中注册可选条款信息后，通过回调通知用户。


**API**

```objectivec
+ (void)updateTermsWithViewController:(UIViewController *)viewController
                        configuration:(TCGBUpdateTermsConfiguration *)configuration
                           completion:(nullable void (^)(TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | 未初始化Gamebase。 |
| TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 设置了未注册的条款Seq值。 |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 一次调用的Terms API未完成。<br/>请稍后再试。 |


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
| termsVersion         | **M**                      | String                    | 条款版本<br/>需要传送调用queryTerms API时获取的值。   |
| termsSeq             | **M**                      | int                       | 所有条款KEY<br/>通过调用queryTerms API，需要传送获取的值。              |
| contents             | **M**                      | Array< TCGBTermsContent > | 用户同意可选条款的信息。 |

#### TCGBTermsContent

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 可选条款项目KEY      |
| agreed               | **M**                      | BOOL               | 是否同意可选条款项目 |

### isShowingTermsView

可以看到当前条款窗正在页面上显示。

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

Gamebase支持基本WebView。<br/>
与WebView有关的资源(图片、html及其他资源)已被包括在Gamebase.bundle中。

### Show WebView

显示WebView。<br/>

##### Required参数
* url : 作为参数发送的url必须是有效值。
* viewController : ViewController用于显示WebView。

##### Optional参数
* configuration : 使用TCGBWebViewConfiguration可更改WebView的布局。
* closeCompletion : 关闭WebView时通过回调通知用户。 
* schemeList : 指定用户需要接收的自定义Scheme列表。
* schemeEvent : 将包含“指定为schemeList的自定义Scheme”的url作为回调通知。

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

可在通过Gamebase WebView加载的网页上通过scheme使用某些功能或更改网页内容。

##### Predefined Custom Scheme

在Gamebase中指定的架构。<br/>

| scheme               | 描述                   |
| -------------------- | ---------------------- |
| gamebase://dismiss   | 关闭WebView。            |
| gamebase://goback    | 返回上一页。         |
| gamebase://getuserid | 查看当前登录的用户ID。|
| gamebase://showwebview?link={URLEncodedURL} | 用link参数的URL打开WebView。<br>URLEncodedURL : 通过WebView要打开的URL。<br>URL需要解码。|
| gamebase://openbrowser?link={URLEncodedURL} | 使用外部浏览器打开link参数的URL。<br/>URLEncodedURL : 使用外部浏览器要打开的URL。<br/>URL需要解码。|




#### User Custom Scheme

可在Gamebase中指定scheme名称和block来添加所需的功能。


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
| isNavigationBarVisible                 | YES or NO                                | 导航栏有效或无效<br/>**default**: YES    |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 返回按钮图标      |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 关闭按钮图标          |

> [TIP]
>
> 在iPadOS 13以上，WebView基本上是Desktop模式。
> contentMode=可以通过“TCGBWebViewContentModeMobile”设置，转换Mobile模式。



### Close WebView
通过以下API，可以关闭当前显示的WebView。

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

可通过调用以下API打开外部浏览器。传送到参数的URL应该为有效值。 

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

无法显示系统通知。

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
2. 可以在“blocks”中注册用户的AlertAction。

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

可以使用以下API轻松显示[Android toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html)消息。<br/>
可以设置简单的消息和显示时间。

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
