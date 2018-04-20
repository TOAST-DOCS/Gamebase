## Game > Gamebase > iOS SDK ご利用ガイド > UI

## WebView

Gamebaseでは、基本的なWebViewに対応しています。<br/>
<br/>
WebViewの関連リソース(画像及びhtml、その他のリソース)はGamebase.bundleに含まれています。

### Show WebView

WebViewを表示します。<br/>

##### Required パラメーター
* url:パラメーターで送信されるurlは、有効な値でなければなりません。
* viewController:WebViewが表示されるView Controllerです。

##### Optional パラメーター
* configuration:GamebaseWebViewConfigurationでWebViewのレイアウトを変更することができます。
* closeCompletion : WebViewが終了する際に、ユーザーにコールバックで知らせます。
* schemeList : ユーザーの求めるカスタムSchemeのリストを指定します。
* schemeEvent : schemeListに指定したカスタムSchemeを含むurlをコールバックで知らせます。


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
ユーザーが指定したWebViewを表示します。<br/>TCGBWebViewConfigurationでユーザーが指定したWebViewを作成することができます。

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

Gamebase WebViewで読み込んだウェブページ内にスキーム(scheme)で特定の機能を使用したり、ウェブページの内容を変更することができます。

##### Predefined Custom Scheme

Gamebaseで指定したスキームです。<br/>

| scheme               | 用途                     |
| -------------------- | ---------------------- |
| gamebase://dismiss   | WebViewを閉じる |
| gamebase://goBack    | WebViewに戻る |
| gamebase://getUserId | 現在ログインしているユーザーのIDを表示 |



#### User Custom Scheme

Gamebaseにスキーム名とブロックを指定し、任意の機能を追加することができます。

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
| navigationBarTitle                     | string                                   | WebViewのタイトル |
| orientationMask                        | TCGBWebViewOrientationUnspecified        | 指定なし                |
|                                        | TCGBWebViewOrientationPortrait           | 縦モード              |
|                                        | TCGBWebViewOrientationPortraitUpsideDown | 縦モードを180度回転      |
|                                        | TCGBWebViewOrientationLandscapeRight     | 横モード              |
|                                        | TCGBWebViewOrientationLandscapeLeft      | 横モードを180度回転    |
| navigationBarColor                     | UIColor                                  | ナビゲーションバーの色         |
| isBackButtonVisible                    | YES or NO                                | 戻るボタンの有効化または無効化 |
| navigationBarHeight                    | CGFloat                                  | ナビゲーションバーの高さ          |
| goBackImagePathForFullScreenNavigation | file name in Gamebase.bundle             | 戻るボタンの画像        |
| closeImagePathForFullScreenNavigation  | file name in Gamebase.bundle             | 閉じるボタンの画像          |


### Close WebView
次のAPIを通じて、表示されているWebViewを閉じることができます。

```objectivec
// Close the gamebase web view
- (void)closeWebView:(id)sender {
    [TCGBWebView closeWebView];
}
```


## Open External Browser

次のAPIを通じて、外部ブラウザを開くことができます。パラメーターで送信されるURLは、有効な値でなければなりません。

```objectivec
// Open the url with Browser
- (void)openWebBrowser:(id)sender {
    NSString* urlString = @"https://www.toast.com/";
    [TCGBWebView openWebBrowserWithURL:urlString];
}
```


## Alert

システム通知を表示することができます。<br/>
iOS 8以上で動作するUIAlertControllerと、iOS 8未満で動作するUIAlertViewを内部的に処理してくれます。 <br/>

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
| TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
