## Game > Gamebase > iOS SDK ご利用ガイド > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

イメージ告知を画面に表示します。

#### Requiredパラメータ
* viewController ：イメージ告知が表示されるViewControllerです。
 
#### Optionalパラメータ
* configuration ： TCGBImageNoticeConfigurationで背景色などのイメージ告知設定を変更できます。
* closeCompletion ：イメージ告知が全て終了する時、ユーザーにコールバックで知らせます。
* schemeEvent ：イメージを押した時、コンソールに登録したpayloadをコールバックで知らせます。


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

ユーザー設定イメージ告知を画面に表示します。
TCGBImageNoticeConfigurationでユーザー設定イメージ告知を作成できます。


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
| backgroundColor                  | UIColor     | イメージ告知の背景色<br/>**default**: [UIColor colorWithRed:0 green:0 blue:0 alpha:0.5]         |
| timeoutMS                  | long        | イメージ告知の最大ローディング時間(単位: millisecond)<br/>**default**: 5000                     |
| enableAutoCloseByCustomScheme    | YES or NO   | custom schemeイベント発生時、告知全体を閉じる、または次の告知を表示<br/>**default**: YES         |


### Close ImageNotices

closeImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

```objectivec
- (void)closeImageNotices {
    [TCGBImageNotice closeImageNoticesWithViewController:self];
}
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知ポップアップ表示中タイムアウトが発生してすべてのポップアップを強制終了します。 |



## Terms

Gamebaseコンソールに設定した約款を表示します。

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

showTermsView APIは、Webビューで約款ウィンドウを表示します。
GameのUIに合った約款ウィンドウを直接作成したい場合は、queryTerms APIを呼び出して、 Gamebaseコンソールに設定した約款項目を呼び出すことができます。
ユーザーが約款に同意した場合、項目別の同意有無をupdateTerms APIを介してGamebaseサーバーへ送信してください。

### showTermsView

約款ウィンドウを画面に表示します。
ユーザーが約款に同意した場合、同意の有無をサーバーに登録します。
約款に同意した場合、showTermsView APIを再度呼び出しても約款ウィンドウが表示されず、すぐに成功コールバックがリターンされます。
ただし、Gamebaseコンソールで「約款の再同意」項目を**必要**に変更した場合は、ユーザーが再度約款に同意するまでは約款ウィンドウが表示されます。

> <font color="red">[注意]</font><br/>
>
> * 約款にプッシュ受信同意有無を追加した場合は、TCGBDataContainerからTCGBPushConfigurationを作成できます。
> * PushConfigurationは、約款ウィンドウが表示されていない場合はnilです。(約款ウィンドウが表示されていれば常に有効なオブジェクトが返されます。)
> * PushConfiguration.pushEnabled値は常にtrueです。
> * TCGBPushConfigurationがnilではない場合、**ログイン後に**[TCGBPush registerPushWithConfiguration:completion:] APIを呼び出してください。
>

#### Requiredパラメータ
* viewController：約款ウィンドウが表示されるViewControllerです。
 
#### Optionalパラメータ
* configuration : TCGBTermsConfigurationで約款ウィンドウを強制的に表示するかどうかなどの設定を変更できます。
* completion：約款同意後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで来るTCGBDataContainerオブジェクトは、TCGBPushConfigurationに変換してログイン後、registerPush APIに使用できます。


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
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001 | ローンチサーバーからダウンロードした項目に約款関連内容がない場合に発生するエラーです。<br/>正常な状況ではないため、Gamebase担当者にお問い合わせください。 |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |
| TCGB\_ERROR\_WEBVIEW\_TIMEOUT | 7002 | 約款Webビューの表示中にタイムアウトが発生しました。 |
| TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR | 7003 | 約款Webビューのオープン中にHTTPエラーが発生しました。 |

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
| forceShow            | BOOL                            | 約款に同意した後も約款ウィンドウを強制的に表示します。<br/>**default**: NO          |

**TCGBShowTermsViewResult**

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| isTermsUIOpened            | BOOL                            | 約款ウィンドウが画面に表示されているかどうかを表します。          |
| TCGBPushConfiguration      | TCGBPushConfiguration           | 約款にプッシュ受信同意有無を追加した場合、プッシュ受信同意有無についての情報を持っています。    |

### queryTerms

Gamebaseは単純な形式のWebビューで約款を表示します。
ゲームUIに合った約款を直接作成したい場合は、queryTerms APIを呼び出してGamebaseコンソールに設定した約款情報をダウンロードして活用できます。

ログイン後に呼び出す場合は、ゲームユーザーが約款に同意したかどうかも一緒に確認できます。

> <font color="red">[注意]</font><br/>
>
> * TCGBTermsContentDetail.requiredがtrueの必須項目は、Gamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * 必須項目は、常にtrueで保存されるしかないので、保存する意味がないためです。
> * プッシュ受信同意有無もGamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * ユーザーのプッシュ受信同意有無は、[TCGBPush queryTokenInfoWithCompletion:] APIを介して確認してください。
> * コンソールで「基本約款設定」をしない場合、約款言語と異なる国コードで設定された端末からqueryTerms APIを呼び出した場合、**TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生します。
>     * コンソールで「基本約款設定」を行ったり、**TCGB_ERROR_UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生した時は、約款を表示しないように処理してください。

#### Requiredパラメータ
* viewController :最上位ViewControllerです。
* completion :API呼び出し結果をユーザーにコールバックで伝えます。コールバックで返されたTCGBQueryTermsResultでコンソールに設定された約款情報を取得できます。
 

**API**

```objectivec
+ (void)queryTermsWithViewController:(UIViewController *)viewController
                          completion:(void (^)(TCGBQueryTermsResult * _Nullable queryTermsResult, TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 約款情報がコンソールに登録されていません。 |
| TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 端末国コードに合った約款情報がコンソールに登録されていません。 |

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
| termsSeq             | int                             | 約款全体KEY。<br/>updateTerms APIを呼び出す時に必要な値です。          |
| termsVersion         | String                          | 約款バージョン。<br/>updateTerms APIを呼び出す時に必要な値です。              |
| termsCountryType     | String                          | 約款タイプ。<br/> -KOREAN：韓国の約款 <br/> - GDPR ：ヨーロッパの約款 <br/> - ETC：その他の約款 |
| contents             | Array< TCGBTermsContentDetail > | 約款項目情報       |


#### TCGBTermsContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 約款項目KEY         | 
| name                 | String                | 約款項目名      |
| required             | BOOL                  | 必須同意有無        |
| agreePush            | String                | 広告性プッシュ同意有無。<br/> - NONE ：同意しない <br/> - ALL ：全て同意 <br/> - DAY ：1週間プッシュ同意<br/> - NIGHT ：夜間プッシュ同意       |
| agreed               | BOOL                  | 該当約款項目のユーザー同意有無          |
| node1DepthPosition   | int                   | 1段階項目表示順序。           |
| node2DepthPosition   | int                   | 2段階項目表示順序。<br/> ない場合は-1           |
| detailPageUrl        | String                | 約款詳細表示URL。<br/> 設定されていない場合はフィールドなし          |


### updateTerms

queryTerms APIでダウンロードした約款情報でUIを直接作った場合は、
ゲームユーザーが約款に同意した内容をupdateTerms APIを介してGamebaseサーバーへ送信してください。

任意約款同意をキャンセルするように、約款に同意した内容を変更する目的で活用できます。


> <font color="red">[注意]</font><br/>
>
> プッシュ受信同意有無は、Gamebaseサーバーに保存されません。
> プッシュ受信同意有無は、**ログイン後に**[TCGBPush registerPushWithConfiguration:completion:] APIを呼び出して保存してください。
>

#### Requiredパラメータ
* viewController :最上位ViewControllerです。
* configuration :サーバーに登録するユーザーの任意約款情報です。
 
#### Optionalパラメータ

* completion ：任意約款情報をサーバーに登録した後、ユーザーにコールバックで伝えます。


**API**

```objectivec
+ (void)updateTermsWithViewController:(UIViewController *)viewController
                        configuration:(TCGBUpdateTermsConfiguration *)configuration
                           completion:(nullable void (^)(TCGBError * _Nullable error))completion;
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| TCGB\_ERROR\_NOT\_INITIALIZED | 1 |Gamebaseが初期化されていません。 |
| TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 登録されていない約款Seq値を設定しました。 |
| TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |


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
| termsVersion         | **M**                      | String                    | 約款のバージョン。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。   |
| termsSeq             | **M**                      | int                       | 約款全体KEY。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。             |
| contents             | **M**                      | Array< TCGBTermsContent > | 任意約款ユーザー同意情報 |

#### TCGBTermsContent

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 任意約款項目KEY      |
| agreed               | **M**                      | BOOL               | 任意約款項目同意有無 |

## WebView

Gamebaseでは、基本的なWebViewに対応しています。<br/>
WebViewの関連リソース(画像及びhtml、その他のリソース)はGamebase.bundleに含まれています。

### Show WebView

WebViewを表示します。<br/>

##### Required パラメーター
* url:パラメーターで送信されるurlは、有効な値でなければなりません。
* viewController:WebViewが表示されるView Controllerです。

##### Optional パラメーター
* configuration : TCGBWebViewConfigurationでWebViewのレイアウトを変更することができます。
* closeCompletion : WebViewが終了する際に、ユーザーにコールバックで知らせます。
* schemeList : ユーザーの求めるカスタムSchemeのリストを指定します。
* schemeEvent : schemeListに指定したカスタムSchemeを含むurlをコールバックで知らせます。


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
ユーザーが指定したWebViewを表示します。<br/>TCGBWebViewConfigurationでユーザーが指定したWebViewを作成することができます。

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

Gamebase WebViewで読み込んだウェブページ内にスキーム(scheme)で特定の機能を使用したり、ウェブページの内容を変更することができます。

##### Predefined Custom Scheme

Gamebaseで指定したスキームです。<br/>

| scheme               | 用途                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | WebViewを閉じる |
| gamebase://goback    | WebViewに戻る |
| gamebase://getuserid | 現在ログインしているユーザーのIDを表示 |
| gamebase://showwebview?link={URLEncodedURL} | linkパラメータのURLをWebViewで開く。<br>URLEncodedURL ：WebViewで開くURL。<br>URLのデコード必要。 |
| gamebase://openbrowser?link={URLEncodedURL} | linkパラメータのURLを外部ブラウザで開く<br>URLEncodedURL：外部ブラウザで開くURL<br>URLデコード必要 |



#### User Custom Scheme

Gamebaseにスキーム名とブロックを指定し、任意の機能を追加することができます。

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
| navigationBarTitle                     | string                                   | WebViewのタイトル |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | 指定なし                |
|                                        | TCGBWebViewOrientationPortrait           | 縦モード              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | 縦モードを180度回転      |
|                                        | TCGBWebViewOrientationLandscapeRight     | 横モード              |
|                                        | TCGBWebViewOrientationLandscapeLeft      | 横モードを180度回転    |
| contentMode                            | TCGBWebViewContentModeRecommended        | 現在プラットフォーム推薦ブラウザ |
|                                        | TCGBWebViewContentModeMobile             | モバイルブラウザ         |
|                                        | TCGBWebViewContentModeDesktop            | デスクトップブラウザ       |
| navigationBarColor                     | UIColor                                  | ナビゲーションバーの色         |
| isBackButtonVisible                    | YES or NO                                | 戻るボタンの有効化または無効化 |
| navigationBarHeight                    | CGFloat                                  | ナビゲーションバーの高さ          |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 戻るボタンの画像        |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 閉じるボタンの画像          |

> [TIP]
>
> iPadOS 13以上でWebViewは基本的にデスクトップモードです。
> contentMode=`TCGBWebViewContentModeMobile`設定でモバイルモードに変更できます。



### Close WebView
次のAPIを通じて、表示されているWebViewを閉じることができます。

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

次のAPIを使用して外部ブラウザを開くことができます。パラメータに渡されるURLは有効な値である必要があります。

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

システム通知を表示できます。<br/>

#### Types of Alert
1. 「確認」ボタンを1つだけ提供し、確認ボタンをクリックするとcompletionを呼び出します。
2. 「確認」ボタンを1つだけ提供し、completionは提供しません。

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

1. 基本的に**Cancel**ボタンがあるActionSheetを提供します。
2. **blocks**にユーザーのAlertActionを登録できます。


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

次のAPIを使用して簡単に[Android トースト(toast)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html)のメッセージを表示することができます。<br/>
簡単なメッセージと表示される時間を設定することができます。

```objectivec
- (void)showToastMessage:(id)sender {
    // 3秒間メッセージを表示する (deprecated API)
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE" duration:3];

    // メッセージを長く(3.5秒間)表示する
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum long" length:GamebaseToastLengthLong];

    // メッセージを短く(2秒間)表示する
    [TCGBUtil showToastWithMessage:@"TOAST MESSAGE with enum short" length:GamebaseToastLengthShort];
}
```


## Error Handling


| Error                           | Error Code | Description                 |
| ------------------------------- | ---------- | --------------------------- |
| TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | イメージ告知の表示中にタイムアウトが発生しました。 |
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
