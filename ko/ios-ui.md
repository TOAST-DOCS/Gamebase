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

**ErrorCode**

| Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT(6901) | 이미지 공지 팝업 표시중 타임아웃이 발생하여 모든 팝업을 강제 종료합니다. |



## Terms

Gamebase 콘솔에 설정한 약관을 표시합니다.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

showTermsView API 는 웹뷰로 약관창을 표시해줍니다.
Game 의 UI 에 맞는 약관창을 직접 제작하고자 하는 경우에는 queryTerms API 를 호출하여, Gamebase 콘솔에 설정한 약관 항목을 불러올 수 있습니다.
유저가 약관에 동의했다면 각 항목별 동의 여부를 updateTerms API 를 통해 Gamebase 서버로 전송하시기 바랍니다.

### showTermsView

약관창을 화면에 띄워 줍니다.
유저가 약관에 동의를 했을 경우, 동의 여부를 서버에 등록합니다.
약관에 동의했다면 showTermsView API 를 다시 호출해도 약관창이 표시되지 않고 바로 성공 콜백이 리턴됩니다.
단, Gamebase 콘솔에서 약관 재동의 항목을 **필요** 로 변경했다면 유저가 다시 약관에 동의할 때까지는 약관창이 표시됩니다.

> <font color="red">[주의]</font><br/>
>
> 약관에 푸시 수신 동의 여부를 추가했다면, TCGBDataContainer 로부터 TCGBPushConfiguration 을 생성할 수 있습니다.
> TCGBPushConfiguration 이 nil 이 아니라면 **로그인 후에** [TCGBPush registerPushWithConfiguration:completion:] API 를 호출하세요.
>

#### Required 파라미터
* viewController : 약관창이 노출되는 ViewController 입니다.
 
#### Optional 파라미터

* completion : 약관 동의 후 약관창이 종료될 때 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 TCGBDataContainer 객체는 TCGBPushConfiguration으로 변환해서 로그인 후 registerPush API 에 사용할 수 있습니다.


**API**

```objectivec
+ (void)showTermsViewWithViewController:(UIViewController *)viewController
                             completion:(nullable void (^)(TCGBDataContainer * _Nullable dataContainer, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR(2001) | 런칭서버가 내려준 항목에 약관 관련 내용이 없는 경우에 발생하는 에러입니다.<br/>정상적인 상황이 아니므로 Gamebase 담당자에게 문의해주시기 바랍니다. |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |
| TCGB\_ERROR\_WEBVIEW\_TIMEOUT(7002) | 약관 웹뷰 표시 중 타임아웃이 발생했습니다. |
| TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR(7003) | 약관 웹뷰 오픈 중 HTTP 에러가 발생하였습니다. |

**Example**

```objectivec
- (void)showTermsView {
    void(^completion)(TCGBDataContainer *, TCGBError *) = ^(TCGBDataContainer *dataContainer, TCGBError *error) {
        // Called when the entire termsView is closed.
        NSLog(@"TermsView closed");

        // Save Push Configuration and use it for registerPush after login.
        TCGBPushConfiguration *configuraiton = [TCGBPushConfiguration fromDataContainer:dataContainer];
    };

    [TCGBTerms showTermsViewWithViewController:self completion:completion];
}
```


### queryTerms

Gamebase는 단순한 형태의 웹뷰로 약관을 표시합니다.
게임UI에 맞는 약관을 직접 제작하고자 하신다면, queryTerms API 를 호출하여 Gamebase 콘솔에 설정한 약관 정보를 내려받아 활용하실 수 있습니다.

로그인 후에 호출하신다면 게임유저가 약관에 동의했는지 여부도 함께 확인할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * TCGBTermsContentDetail.required 가 true 인 필수 항목은 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 필수 항목은 항상 true 로 저장될 수 밖에 없어서 저장하는 의미가 없기 때문입니다.
> * 푸시 수신 동의 여부도 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 유저의 푸시 수신 동의 여부는 [TCGBPush queryTokenInfoWithCompletion:] API 를 통해 확인하시기 바랍니다.
> * 콘솔에서 '기본 약관 설정' 을 하지 않는 경우, 약관 언어와 다른 국가코드로 설정된 단말기에서 queryTerms API 를 호출하면 **TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생합니다.
>     * 콘솔에서 '기본 약관 설정' 을 하거나, **TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생했을때는 약관을 표시하지 않도록 처리하시기 바랍니다.

#### Required 파라미터
* viewController : 최상위 ViewController 입니다.
* completion : API 호출 결과를 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 TCGBQueryTermsResult으로 콘솔에 설정된 약관 정보를 얻을 수 있습니다.
 

**API**

```objectivec
+ (void)queryTermsWithViewController:(UIViewController *)viewController
                          completion:(void (^)(TCGBQueryTermsResult * _Nullable queryTermsResult, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | 약관 정보가 콘솔에 등록되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | 단말기 국가코드에 맞는 약관 정보가 콘솔에 등록되어 있지 않습니다. |

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
| termsSeq             | int                             | 약관 전체 KEY.<br/>updateTerms API 호출 시 필요한 값입니다.          |
| termsVersion         | String                          | 약관 버전.<br/>updateTerms API 호출 시 필요한 값입니다.              |
| termsCountryType     | String                          | 약관 타입.<br/> - KOREAN : 한국 약관 <br/> - GDPR : 유럽 약관 <br/> - ETC : 기타 약관         |
| contents             | Array< TCGBTermsContentDetail > | 약관 항목 정보          |


#### TCGBTermsContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 약관 항목 KEY         | 
| name                 | String                | 약관 항목 이름         |
| required             | BOOL                  | 필수 동의 여부         |
| agreePush            | String                | 광고성 푸시 동의 여부.<br/> - NONE : 동의 안함 <br/> - ALL : 전체 동의 <br/> - DAY : 주간 푸시 동의<br/> - NIGHT : 야간 푸시 동의          |
| agreed               | BOOL                  | 해당 약관 항목에 대한 유저 동의 여부           |
| node1DepthPosition   | int                   | 1단계 항목 노출 순서.           |
| node2DepthPosition   | int                   | 2단계 항목 노출 순서.<br/> 없을 경우 -1           |
| detailPageUrl        | String                | 약관 자세히 보기 URL.<br/> 설정되어 있지 않으면 필드 없음           |


### updateTerms

queryTerms API 로 내려받은 약관 정보로 UI 를 직접 제작했다면,
게임유저가 약관에 동의한 내역을 updateTerms API 를 통해 Gamebase 서버로 전송하시기 바랍니다.

선택 약관 동의를 취소하는 것과 같이, 약관에 동의했던 내역을 변경하는 목적으로도 활용하실 수 있습니다.


> <font color="red">[주의]</font><br/>
>
> 푸시 수신 동의 여부는 Gamebase 서버에 저장되지 않습니다.
> 푸시 수신 동의 여부는 **로그인 후에** [TCGBPush registerPushWithConfiguration:completion:] API 를 호출해서 저장하세요.
>

#### Required 파라미터
* viewController : 최상위 ViewController 입니다.
* configuration : 서버에 등록할 유저의 선택 약관 정보입니다.
 
#### Optional 파라미터

* completion : 선택 약관 정보를 서버에 등록 후 사용자에게 콜백으로 알려줍니다.


**API**

```objectivec
+ (void)updateTermsWithViewController:(UIViewController *)viewController
                        configuration:(TCGBUpdateTermsConfiguration *)configuration
                           completion:(nullable void (^)(TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 등록되지 않은 약관 Seq 값을 설정하였습니다. |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |


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
| termsVersion         | **M**                      | String                    | 약관 버전.<br/>queryTerms API 를 호출해서 내려받았던 값을 전달해야 합니다.   |
| termsSeq             | **M**                      | int                       | 약관 전체 KEY.<br/>queryTerms API 를 호출해서 내려받았던 값을 전달해야 합니다.             |
| contents             | **M**                      | Array< TCGBTermsContent > | 선택 약관 유저 동의 정보  |

#### TCGBTermsContent

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 선택 약관 항목 KEY      |
| agreed               | **M**                      | BOOL               | 선택 약관 항목 동의 여부  |

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
    void(^closeCompletion)(TCGBError *) = ^(TCGBError *error) {
        NSLog(@"WebView Close Event occured");
    };

    [TCGBWebView showWebViewWithURL:urlString viewController:self configuration:nil closeCompletion:closeCompletion schemeList:nil schemeEvent:nil];

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

Gamebase WebView에서 로딩한 웹 페이지 내에 스키마(scheme)로 특정 기능을 사용하거나 웹 페이지 내용을 변경할 수 있습니다.

##### Predefined Custom Scheme

Gamebase에서 지정해 놓은 스키마입니다.<br/>

| scheme               | 용도                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | WebView 닫기             |
| gamebase://goback    | WebView 뒤로 가기          |
| gamebase://getuserid | 현재 로그인중인 있는 사용자의 아이디 표시 |
| gamebase://showwebview?link={URLEncodedURL} | link 파라메터의 URL 을 WebView로 열기.<br>URLEncodedURL : WebView로 열 URL.<br>URL 디코딩 필요. |
| gamebase://openbrowser?link={URLEncodedURL} | link 파라메터의 URL을 외부 브라우저로 열기<br/>URLEncodedURL : 외부 브라우저로 열 URL<br/>URL 디코딩 필요 |



#### User Custom Scheme

Gamebase에 스키마 이름과 블록을 지정해 원하는 기능을 추가할 수 있습니다.


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
