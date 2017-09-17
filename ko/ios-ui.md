## Game > Gamebase > Developer's Guide (iOS) > UI

## UI

### 1. WebView

Gamebase에서는 기본적인 웹뷰를 지원합니다. 웹뷰의 스타일은 Fullscreen과 Popup 스타일을 지원하며, Customizing이 가능합니다.<br/>

Fullscreen 스타일(Browser)은 네비게이션바를 가지며, Close/GoBack 버튼을 가집니다. 네비게이션바에 타이틀을 지정할 수 있습니다.<br/>

Popup 스타일은 기존화면 위에 모달뷰 형식으로 나타나게 되며, 뒷 배경은 투명한 mask view로 덮어씌워집니다.

<br/><br/>

웹뷰와 관련된 리소스(이미지 및 html, 기타 리소스)는 Gamebase.bundle 에 포함되어있습니다.

```objectivec
// Show Fullscreen Style WebView
- (void)showFullScreenWebView:(id)sender {
    [TCGBWebView showWebBrowserWithURL:@"http://cloud.toast.com" viewController:self];
}

// Show Popup Style WebView
- (void)showPopupWebView:(id)sender {
    [TCGBWebView showPopupWithURL:@"http://cloud.toast.com" viewController:self];
}

// Show Customized WebView
- (void)showCustomizedWebView:(id)sender {
    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    [configuration setStyle:TCGBWebViewLaunchFullScreen];    //or TCGBWebViewLaunchPopUp
    [configuration setNavigationBarColor:[UIColor blueColor]];
    [configuration setNavigationBarHeight:50.0];

    [TCGBWebView showWebViewWithURL:@"http://cloud.toast.com" viewController:self configuration:configuration];
}

// Configure Custom Style Configuration to All TCGBWebView Objects
- (void)configureWebViewStyle {
    // After this method is called, every webview(TCGBWebView) is shown with popup style.

    TCGBWebViewConfiguration *configuration = [[TCGBWebViewConfiguration alloc] init];
    [configuration setStyle:TCGBWebViewLaunchPopUp];    //or TCGBWebViewLaunchFullScreen

    [TCGBWebView sharedTCGBWebView].defaultWebConfiguration = configuration;
}
```


### 2. Alert

System Alert 를 위한 API를 제공합니다.<br/>
iOS8 이상에서 동작하는 UIAlertController와, iOS8 미만에서의 UIAlertView 처리를 내부적으로 해줍니다.<br/>
다음의 API를 통해서, 사용자는 Alert에 버튼 및 콜백을 등록할 수 있습니다.

```objectivec
- (void)showAlert:(id)sender {
    void (^positiveBlock)(id) = ^(id title) {
        NSLog(@"Positive Block Clicked");
    };

    void (^negativeBlock)(id) = ^(id title) {
        NSLog(@"Negative Block Clicked");
    };

    [TCGBUtil showAlertWithTitle:@"alert title" message:@"alert message"
            positiveTitle:@"positive" positiveBlock:positiveBlock
            negativeTitle:@"negative" negativeBlock:negativeBlock];
}
```

