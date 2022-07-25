## Game > Gamebase > Unity Developer's Guide > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

イメージ告知を画面に表示します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowImageNotices(GamebaseRequest.ImageNotice.Configuration configuration, GamebaseCallback.ErrorDelegate closeCallback, GamebaseCallback.GamebaseDelegate<string> eventCallback = null)
```

**Example**

```cs
public void ShowImageNotices()
{
    Gamebase.ImageNotice.ShowImageNotices(
        null,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

### Custom ImageNotices

ユーザー設定イメージ告知を画面に表示します。
GamebaseRequest.ImageNotice.Configurationでユーザー設定イメージ告知を作成できます。

**Example**

```cs
public void ShowImageNotices(int colorR = 0 , int colorG = 0, int colorB = 0, int colorA = 128, long timeOut = 5000)
{
    GamebaseRequest.ImageNotice.Configuration configuration = new GamebaseRequest.ImageNotice.Configuration();
    configuration.colorR = colorR;
    configuration.colorG = colorG;
    configuration.colorB = colorB;
    configuration.colorA = colorA;
    configuration.timeOut = timeOut;

    Gamebase.ImageNotice.ShowImageNotices(
        configuration,
        (error) =>
        {
            // Called when the entire imageNotice is closed.
            ...
            
        },
        (scheme, error) =>
        {
            // Called when custom event occurred.
            ...
        });        
}
```

#### GamebaseRequest.ImageNotice.Configuration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| colorR                   | 0～255                                    | ナビゲーションバーの色R            |
| colorG                   | 0～255                                    | ナビゲーションバーの色G                |
| colorB                   | 0～255                                    | ナビゲーションバーの色B                |
| colorA                   | 0～255                                    | ナビゲーションバーの色Alpha                |
| timeoutMS                | long        | イメージ告知最大ローディング時間(単位：millisecond)<br/>**default**：5000                     |


### Close ImageNotices

closeImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseImageNotices()
```

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

#### Optionalパラメータ

* GamebaseTermsConfiguration : GamebaseTermsConfigurationオブジェクトを介して強制的に約款同意ウィンドウを表示するかどうかなどの設定を変更できます。
* callback：約款に同意した後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで返されたGamebaseResponse.DataContainerオブジェクトはGamebaseResponse.Push.PushConfigurationに変換してログイン後、Gamebase.Push.registerPush APIに使用できます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowTermsView(GamebaseCallback.GamebaseDelegate<GamebaseResponse.DataContainer> callback)
static void ShowTermsView(GamebaseRequest.Terms.GamebaseTermsConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.DataContainer> callback)
```

**GamebaseRequest.Terms.GamebaseTermsConfiguration** 
 
| API | Mandatory(M) / Optional(O) | Description | 
| --- | --- | --- | 
| forceShow | O | 約款に同意した場合、showTermsView APIを再度呼び出しても約款ウィンドウが表示されませんが、これを無視して強制的に約款ウィンドウを表示します。<br>**default** : false |
| enableFixedFontSize | O | 約款ウィンドウの文字サイズを固定するかどうかを決定します。<br>**default** : false<br/>**Android Only** |


**GamebaseResponse.Terms.ShowTermsViewResult**
| Parameter              | Values                          | Description         |
| ---------------------- | --------------------------------| ------------------- |
| isTermsUIOpened        | bool                            | **true**：約款ウィンドウが表示され、ユーザーが同意して約款ウィンドウが終了しました。<br>**false**：すでに約款に同意していて約款ウィンドウが表示されずに約款ウィンドウが終了しました。 |
| pushConfiguration      | GamebaseResponse.Push.PushConfiguration           | isTermsUIOpenedが **true**で、約款にプッシュ受信同意有無を追加した場合、pushConfigurationは常に有効なオブジェクトを持ちます。<br>そうでない場合は**null**です。<br>pushConfigurationが有効なとき、pushConfiguration.pushEnabled値は常に**true**です。 |

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| LAUNCHING\_SERVER\_ERROR | 2001 | ローンチサーバーからダウンロードした項目に約款関連内容がない場合に発生するエラーです。<br/>正常な状況ではないため、Gamebase担当者にお問い合わせください。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | 約款Webビューが終了していないのに再び呼び出されました。 |
| WEBVIEW\_TIMEOUT | 7002 | 約款Webビューの表示中にタイムアウトが発生しました。 |
| WEBVIEW\_HTTP\_ERROR | 7003 | 約款Webビューのオープン中にHTTPエラーが発生しました。 |

**Example**

```cs
static GamebaseResponse.Push.PushConfiguration savedPushConfiguration = null;

public void SampleShowTermsView()
{
    var configuration = new GamebaseRequest.Terms.GamebaseTermsConfiguration
    {
        forceShow = true
    };
    Gamebase.Terms.ShowTermsView(configuration, (data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("ShowTermsView succeeded.");

            GamebaseResponse.Terms.ShowTermsViewResult result = GamebaseResponse.Terms.ShowTermsViewResult.From(data);

            // Save the 'PushConfiguration' and use it for Gamebase.Push.RegisterPush() after Gamebase.Login().
            savedPushConfiguration = result.pushConfiguration;
            
            // Wheter the TermsUI was displayed.
            bool isTermsUIOpened = result.isTermsUIOpened;
        }
        else
        {
            Debug.Log(string.Format("ShowTermsView failed. error:{0}", error));
        }
    });
}

public void AfterLogin()
{
    // Call RegisterPush with saved PushConfiguration.
    if (savedPushConfiguration != null)
    {
        Gamebase.Push.RegisterPush(savedPushConfiguration, (error) =>
        {
            ...
        });
    }
}
```    

### queryTerms

Gamebaseは単純な形式のWebビューで約款を表示します。
ゲームUIに合った約款を直接作成したい場合は、queryTerms APIを呼び出してGamebaseコンソールに設定した約款情報をダウンロードして活用できます。

ログイン後に呼び出す場合は、ゲームユーザーが約款に同意したかどうかも一緒に確認できます。

> <font color="red">[注意]</font><br/>
>
> * GamebaseResponse.Terms.ContentDetail.requiredがtrueの必須項目は、Gamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * 必須項目は、常にtrueで保存されるしかないので、保存する意味がないためです。
> * プッシュ受信同意有無もGamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * ユーザーのプッシュ受信同意有無は、Gamebase.Push.QueryPush APIで確認してください。
> * コンソールで「基本約款設定」をしない場合、約款言語と異なる国コードで設定された端末からqueryTerms APIを呼び出した場合、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生します。
>     * コンソールで「基本約款設定」を行ったり、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生した時は、約款を表示しないように処理してください。
> * Standaloneプラットフォームでは、プッシュに関連する機能をサポートしないため、ゲームUIにその約款が表示されないように注意します。

#### Requiredパラメータ
* callback：API呼び出し結果をユーザーにコールバックで伝えます。コールバックで返されたGamebaseResponse.Terms.QueryTermsResultでコンソールに設定された約款情報を取得できます。
 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void QueryTerms(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Terms.QueryTermsResult> callback)
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 端末国コードに合った約款情報がコンソールに登録されていません。 |

**Example**

```cs
public void SampleQueryTerms()
{
    Gamebase.Terms.QueryTerms((data, error) => 
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("QueryTerms succeeded.");
        }
        else
        {
            Debug.Log(string.Format("QueryTerms failed. error:{0}", error));
        }
    });
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| termsSeq             | int                             | 約款全体KEY。<br/>updateTerms APIを呼び出す時に必要な値です。          |
| termsVersion         | string                          | 約款のバージョン。<br/>updateTerms APIを呼び出す時に必要な値です。              |
| termsCountryType     | string                          | 約款タイプ。<br/> - KOREAN：韓国の約款 <br/> - GDPR ：ヨーロッパの約款 <br/> - ETC：その他の約款 |
| contents             | List< ContentDetail > | 約款項目情報       |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 約款項目KEY         | 
| name                 | string                | 約款項目名      |
| required             | bool                  | 必須同意有無        |
| agreePush            | string                | 広告性プッシュ同意有無。<br/> - NONE ：同意しない <br/> - ALL ：全て同意 <br/> - DAY ：1週間プッシュ同意<br/> - NIGHT ：夜間プッシュ同意       |
| agreed               | bool                  | 該当約款項目のユーザー同意有無。           |
| node1DepthPosition   | int                   | 1段階項目表示順序。           |
| node2DepthPosition   | int                   | 2段階項目表示順序。<br/> ない場合は-1           |
| detailPageUrl        | string                | 約款詳細表示URL。<br/> ない場合はnull。 |


### updateTerms

queryTerms APIでダウンロードした約款情報でUIを直接作った場合は、
ゲームユーザーが約款に同意した内容をupdateTerms APIを介してGamebaseサーバーへ送信してください。

任意約款同意をキャンセルするように、約款に同意した内容を変更する目的で活用できます。


> <font color="red">[注意]</font><br/>
>
> プッシュ受信同意有無は、Gamebaseサーバーに保存されません。
> プッシュ受信同意有無は、**ログイン後に**Gamebase.Push.registerPush APIを呼び出して保存してください。
> Standaloneプラットフォームではログイン後に該当APIを呼び出す必要があります。ログインを行わずに呼び出す場合はNOT_LOGGED_INエラーが渡されます。
>

#### Requiredパラメータ
* configuration :サーバーに登録するユーザーの任意約款情報です。
 
#### Optionalパラメータ

* callback：任意約款情報をサーバーに登録した後、ユーザーにコールバックで伝えます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void UpdateTerms(GamebaseRequest.Terms.UpdateTermsConfiguration configuration, GamebaseCallback.ErrorDelegate callback)
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| NOT\_LOGGED_IN | 2 | ログインが必要です。 (Only Standalone) |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 登録されていない約款Seq値を設定しました。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |


**Example**

```cs
public void SampleUpdateTerms()
{            
    List<GamebaseRequest.Terms.Content> list = new List<GamebaseRequest.Terms.Content>();
    list.Add(new GamebaseRequest.Terms.Content()
    {
        termsContentSeq = 0,
        agreed = true
    });
    
    Gamebase.Terms.UpdateTerms(
        new GamebaseRequest.Terms.UpdateTermsConfiguration()
        {
            termsSeq = 0,
            termsVersion = "1.0.0",
            contents = list
        }
        ,
        (error) =>
        {
            if (Gamebase.IsSuccess(error) == true)
            {
                Debug.Log("UpdateTerms succeeded.");
            }
            else
            {
                Debug.Log(string.Format("UpdateTerms failed. error:{0}", error));
            }
        });
}
```


#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | String                    | 約款のバージョン。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。   |
| termsSeq             | **M**                      | int                       | 約款全体KEY。<br/>queryTerms APIを呼び出してダウンロードした値を伝達する必要があります。             |
| contents             | **M**                      | List< Content > | 任意約款ユーザー同意情報 |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 任意約款項目KEY      |
| agreed               | **M**                      | bool               | 任意約款項目同意有無 |


### IsShowingTermsView

現在約款ウィンドウが画面に表示されているかどうかを知ることができます。

**API**

```cs
static bool IsShowingTermsView()
```
**Example**
```cs
public void SampleIsShowingTermsView()
{
    bool isShowingTermsView = Gamebase.Terms.IsShowingTermsView();
    Debug.Log(string.Format("isShowingTermsView: {0}", isShowingTermsView));
}
```

## Webview

### Show WebView

WebViewを表示します。<br/>

##### Requiredパラメーター
* url:パラメーターで送信されるurlは、有効な値でなければなりません。

##### Optionalパラメーター
* configuration:GamebaseWebViewConfigurationでWebViewのレイアウトを変更することができます。
* closeCallback:WebViewが終了する際に、ユーザーにコールバックで知らせます。
* schemeList:ユーザーの求めるカスタムSchemeのリストを指定します。
* schemeEvent:schemeListに指定したカスタムスキームを含むurlをコールバックで知らせます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> スタンドアローン(standalone)では、WebViewAdapterでログインをサポートし、WebViewが開かれている時は、UIで入力されるイベントをブロッキング(blocking)しません。


**Example**
```cs
private void SchemeEvent(string scheme, GamebaseError error)
{
    Debug.Log(string.Format("[SchemeEvent] scheme:{0}", scheme));
}

private void CloseCallback(GamebaseError error)
{
    Debug.Log("CloseCallback");
}
    
public void ShowWebView()
{
    GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = new GamebaseRequest.Webview.GamebaseWebViewConfiguration();
     configuration.title = "Title";
     configuration.orientation = GamebaseScreenOrientation.Portrait;
     configuration.colorR = 128;
     configuration.colorG = 128;
     configuration.colorB = 128;
     configuration.colorA = 255;
     configuration.barHeight = 40;
     configuration.buttonVisible = true;
    
    var schemeList = new List<string>() { "customScheme://openBrowser" };
    
    Gamebase.Webview.ShowWebView("https://www.toast.com/", configuration, CloseCallback, schemeList, SchemeEvent);
}
```


#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | WebViewのタイトル                 |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 未指定 |
|                          | GamebaseScreenOrientation.PORTRAIT       | 縦モード                       |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 横モード                       |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 横モードを180度回転              |
| contentMode              | GamebaseWebViewContentMode.RECOMMENDED        | 現在プラットフォーム推薦ブラウザ |
|                          | GamebaseWebViewContentMode.MOBILE             | モバイルブラウザ         |
|                          | GamebaseWebViewContentMode.DESKTOP            | デスクトップブラウザ       |
| colorR                   | 0~255                                    | ナビゲーションバーの色Alpha            |
| colorG                   | 0~255                                    | ナビゲーションバーの色R                |
| colorB                   | 0~255                                    | ナビゲーションバーの色G                |
| colorA                   | 0~255                                    | ナビゲーションバーの色B                |
| isNavigationBarVisible   | true or false                            | ナビゲーションバーの有効または無効         |
| isBackButtonVisible      | true or false                            | 戻るボタンの有効または無効         |
| barHeight                | height                                   | ナビゲーションバーの高さ                  |
| backButtonImageResource  | ID of resource                           | 戻るボタンのイメージ                |
| closeButtonImageResource | ID of resource                           | 閉じるボタンの画像 |
| url                      | "http://" or "https://" or "file://"     | Web URL |
| enableFixedFontSize      | true or false                            | 文字サイズ固定の有効または無効<br/>**Android Only** |

> [TIP]
>
> iPadOS 13以上でWebViewは基本的にデスクトップモードです。
> contentMode =`GamebaseWebViewContentMode.MOBILE`設定でモバイルモードに変更できます。


### Close WebView

次のAPIを利用して表示されているWebViewを閉じることができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseWebview()
```

**Example**CloseWebview
```cs
public void CloseWebview()
{
    Gamebase.Webview.CloseWebview();
}
```


## Open External Browser

次のAPIを通じて、外部ブラウザを開くことができます。パラメーターで送信されるurlは、有効な値でなければなりません。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

```cs
static void OpenWebBrowser(string url)
```

**Example**
```cs
public void OpenWebBrowser(string url)
{
    Gamebase.Webview.OpenWebBrowser(url);
}
```


## Alert

システム通知を表示することができます。
システム通知にコールバックを登録することもできます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowAlert(string title, string message)
static void ShowAlert(string title, string message, GamebaseCallback.VoidDelegate buttonCallback)
```

**Example**
```cs
public void ShowAlertD()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message"
    );
}

public void ShowAlertDialog()
{
    Gamebase.Util.ShowAlert
    (
        "Title",
        "Message",
        () => {
                    Debug.Log("ButtonClick");
              }
    );
}
```

## Toast

次のAPIを使用して簡単にメッセージを表示することができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void ShowToast(string message, int duration)
static void ShowToast(string message, GamebaseUIToastType type)
```

**Example**
```cs
public void ShowToast(string message, int duration)
{
    Gamebase.Util.ShowToast(
        message,
        duration
        );
}
public void ShowToast(string message, GamebaseUIToastType type)
{
    Gamebase.Util.ShowToast(
        message,
        type
        );
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知の表示中タイムアウトが発生しました。 |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)
