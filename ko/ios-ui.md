## Game > Gamebase > iOS Developer's Guide > UI

## UI

### WebView

Gamebase에서는 기본적인 웹뷰를 지원합니다. 웹뷰의 스타일은 Fullscreen과 Popup 스타일을 지원하며, Customizing이 가능합니다.<br/>

Fullscreen 스타일(Browser)은 네비게이션바를 가지며, Close/GoBack 버튼을 가집니다. 네비게이션바에 타이틀을 지정할 수 있습니다.<br/>

Popup 스타일은 기존화면 위에 모달뷰 형식으로 나타나게 되며, 뒷 배경은 투명한 mask view로 덮어씌워집니다.

<br/><br/>

웹뷰와 관련된 리소스(이미지 및 html, 기타 리소스)는 Gamebase.bundle 에 포함되어있습니다.

#### Show WebView
##### WebView Styles
WebView는 아래와 같이 2가지의 형태를 제공합니다.

1. Full Screen
2. Popup

```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    [TCGBWebView showWebBrowserWithURL:@"http://cloud.toast.com" viewController:self];
}

// Show Popup Style WebView
- (void)showPopupWebView:(id)sender {
    [TCGBWebView showPopupWithURL:@"http://cloud.toast.com" viewController:self];
}
```

##### WebView Customizing
WebView를 Customizing 하는 기능을 제공합니다.

```objectivec
- (void)showFixedOrientationWebView:(id)sender {
	NSString* urlString = @"https://www.toast.com/";
	TCGBWebViewConfiguration* config = [[TCGBWebViewConfiguration alloc] init];
    // Webview is fixed to Landscape mode
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

#### Custom Scheme in WebView

Gamebase WebView에서 로딩한 WebPages내에 scheme을 사용하여, 특정 기능을 사용하거나, WebPage 내용을 변경할 수 있습니다.

##### Predefined Custom Scheme

Gamebase에서 지정해 놓은 Scheme 입니다.<br/>

| scheme | 용도 |
| --- | --- | --- |
| gamebase://dismiss | WebView 닫기 |
| gamebase://goBack | WebView 뒤로가기 |
| gamebase://getUserId | 현재 로그인되어 있는 유저의 UserId를 표시 |
| gamebase://getMaintenanceInfo | 점검 내용을 WebPage에 표시 |



##### User Custom Scheme

Gamebase에 유저가 scheme명과 block을 지정하여 원하는 기능을 추가할 수 있습니다.


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

### Alert

System Alert 를 위한 API를 제공합니다.<br/>
iOS8 이상에서 동작하는 UIAlertController와, iOS8 미만에서의 UIAlertView 처리를 내부적으로 해줍니다.<br/>

#### Types of Alert
1. '확인'버튼 1개만 제공하며, 확인버튼 클릭 시, completion이 호출됩니다.
2. '확인'버튼 1개만 제공하며, completion을 제공하지 않습니다.

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

### Toast

안드로이드와 같은 Toast를 제공합니다. <br/>
간단한 메시지와 노출되는 시간을 설정할 수 있습니다.

```objectivec
- (void)showToastMessage:(id)sender {
	// 3초 동안 메시지 나타내기
	[TCGBUtil showToastMessage:@"TOAST MESSAGE" duration:3];
}
```


### Error Handling


| Error | Error Code | Notes |
| --- | --- | --- |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999 | 알수 없는 에러입니다. (정의되지 않은 에러입니다.) |



* 전체 에러코드 참조 : [LINK \[Entire Error Codes\]](./error-codes#client-sdk)