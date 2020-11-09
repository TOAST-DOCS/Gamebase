## Game > Gamebase > iOS SDK 사용 가이드 > UI

## ImageNotice

콘솔에 이미지를 등록한 후 사용자에게 공지를 띄울 수 있습니다.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

이미지 공지를 화면에 띄워 줍니다.

#### Required 파라미터
* viewController : 이미지 공지가 노출되는 ViewController 입니다.
 
#### Optional 파라미터
* configuration : TCGBImageNoticeConfiguration으로 배경색 등 이미지 공지 설정을 변경할 수 있습니다.
* closeCompletion : 이미지 공지가 전체 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeEvent : 이미지를 클릭했을 때, 콘솔에 등록한 payload를 콜백으로 알려 줍니다.


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

사용자 설정 이미지 공지를 화면에 띄워 줍니다.
TCGBImageNoticeConfiguration으로 사용자 설정 이미지 공지를 만들 수 있습니다.


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
| backgroundColor                  | UIColor     | 이미지 공지 뒷 배경색<br/>**default**: [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5]         |
| timeoutMS                  | long        | 이미지 공지 최대 로딩 시간 (단위 : millisecond)<br/>**default**: 5000                     |
| enableAutoCloseByCustomScheme    | YES or NO   | custom scheme 이벤트 발생 시 공지 전체 닫기 또는 다음 공지 표시<br/>**default**: YES         |


### Close ImageNotices

closeImageNotices API를 호출하여 현재 표시 중인 이미지 공지를 모두 종료할 수 있습니다.

```objectivec
- (void)closeImageNotices {
    [TCGBImageNotice closeImageNoticesWithViewController:self];
}
```

## WebView

Gamebase에서는 기본적인 WebView를 지원합니다.<br/>
WebView와 관련된 리소스(이미지 및 html, 기타 리소스)는 Gamebase.bundle에 포함돼 있습니다.

### Show WebView

WebView를 표시합니다.<br/>

##### Required 파라미터
* url : 파라미터로 전송되는 url은 유효한 값이어야 합니다.
* viewController : WebView가 노출되는 View Controller입니다.

##### Optional 파라미터
* configuration : TCGBWebViewConfiguration으로 WebView의 레이아웃을 변경 할 수 있습니다.
* closeCompletion : WebView가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList : 사용자가 받고 싶은 커스텀 Scheme 목록을 지정합니다.
* schemeEvent : schemeList로 지정한 커스텀 Scheme을 포함하는 url을 콜백으로 알려 줍니다.


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
사용자 지정 WebView를 표시합니다.<br/>TCGBWebViewConfiguration으로 사용자 지정 WebView를 만들 수 있습니다.

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

Gamebase WebView에서 로딩한 웹 페이지 내에 스키마(scheme)로 특정 기능을 사용하거나 웹 페이지 내용을 변경할 수 있습니다.

##### Predefined Custom Scheme

Gamebase에서 지정해 놓은 스키마입니다.<br/>

| scheme               | 용도                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | WebView 닫기             |
| gamebase://goBack    | WebView 뒤로 가기          |
| gamebase://getUserId | 현재 로그인돼 있는 사용자의 아이디 표시 |
| gamebase://openbrowser?link={URLEncodedURL} | link 파라메터의 URL을 외부 브라우저로 열기<br/>URLEncodedURL : 외부 브라우저로 열 URL<br/>URL 디코딩 필요 |



#### User Custom Scheme

Gamebase에 스키마 이름과 블록을 지정해 원하는 기능을 추가할 수 있습니다.


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
| navigationBarTitle                     | string                                   | WebView의 제목        |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | 미지정                |
|                                        | TCGBWebViewOrientationPortrait           | 세로 모드              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | 세로 모드 180도 회전      |
|                                        | TCGBWebViewOrientationLandscapeRight     | 가로 모드              |
|                                        | TCGBWebViewOrientationLandscapeLeft      | 가로 모드를 180도 회전     |
| contentMode                            | TCGBWebViewContentModeRecommended        | 현재 플랫폼 추천 브라우저    |
|                                        | TCGBWebViewContentModeMobile             | 모바일 브라우저            |
|                                        | TCGBWebViewContentModeDesktop            | 데스크탑 브라우저          |
| navigationBarColor                     | UIColor                                  | 내비게이션 바 색상         |
| isBackButtonVisible                    | YES or NO                                | 뒤로 가기 버튼 활성 또는 비활성 |
| navigationBarHeight                    | CGFloat                                  | 내비게이션 바 높이         |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 뒤로 가기 버튼 이미지       |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 닫기 버튼 이미지          |

> [TIP]
>
> iPadOS 13 이상에서 WebView는 기본적으로 데스크탑 모드입니다.
> contentMode=`TCGBWebViewContentModeMobile` 설정으로 모바일 모드로 변경할 수 있습니다.



### Close WebView
다음 API를 통하여, 보여지고 있는 WebView를 닫을 수 있습니다.

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

다음 API를 사용해 외부 브라우저를 열 수 있습니다. 파라미터로 전송되는 URL은 유효한 값이어야 합니다.

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

시스템 알림을 표시할 수 있습니다.<br/>

#### Types of Alert
1. '확인' 버튼을 1개만 제공하며, 확인 버튼을 클릭하면 completion이 호출됩니다.
2. '확인' 버튼을 1개만 제공하며, completion을 제공하지 않습니다.

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
1. 기본적으로 'Cancel' 버튼이 있는 ActionSheet을 제공합니다.
2. 'blocks'에 사용자의 AlertAction을 등록할 수 있습니다.


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

다음 API를 사용하여 쉽게 [Android 토스트(toast)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) 메시지를 표시할 수 있습니다.<br/>
간단한 메시지와 표시되는 시간을 설정할 수 있습니다.

```objectivec
- (void)showToastMessage:(id)sender {
    // 3초 동안 메시지 나타내기 (deprecated API)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE" duration:3];
    
    // 길게(3.5초) 메시지 나타내기
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum long" length:GamebaseToastLengthLong]; 
    
    // 짧게(2초) 메시지 나타내기
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum short" length:GamebaseToastLengthShort];
}
```


## Error Handling


| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | 이미지 공지 표시 중 타임아웃이 발생했습니다. |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 알 수 없는 오류입니다(정의되지 않은 오류입니다). |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
