## Game > Gamebase > Unreal SDK使用ガイド > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

イメージ告知を画面に表示します。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowImageNotices(FGamebaseImageNoticeConfiguration& configuration, const FGamebaseErrorDelegate& onCloseCallback);
void ShowImageNotices(FGamebaseImageNoticeConfiguration& configuration, const FGamebaseErrorDelegate& onCloseCallback, const FGamebaseImageNoticeEventDelegate& onEventCallback);
```

**Example**

```cpp
void Sample::ShowImageNotices(int32 colorR, int32 colorG, int32 colorB, int32 colorA, int64 timeOut)
{
    FGamebaseImageNoticeConfiguration configuration{ colorR, colorG, colorB, colorA, timeOut };

    IGamebase::Get().GetImageNotice().ShowImageNotices(configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            // Called when the entire imageNotice is closed.
            ...
        }),
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& scheme, const FGamebaseError* error) {
            // Called when custom event occurred.
            ...
        })
    );
}
```

#### FGamebaseImageNoticeConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| colorR                   | 0～255                                    | ナビゲーションバーの色R            |
| colorG                   | 0～255                                    | ナビゲーションバーの色G                |
| colorB                   | 0～255                                    | ナビゲーションバーの色B                |
| colorA                   | 0～255                                    | ナビゲーションバーの色Alpha                |
| timeOut                  | int64        | イメージ告知最大ローディング時間(単位: millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

closeImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


## Terms

Gamebaseコンソールに設定した約款を表示します。

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

ShowTermsView APIは、Webビューで約款ウィンドウを表示します。
GameのUIに合った約款ウィンドウを直接製作したい場合には、QueryTerms APIを呼び出して、 Gamebaseコンソールに設定した約款項目を呼び出すことができます。
ユーザーが約款に同意した場合、各項目の同意有無をUpdateTerms APIを介してGamebaseサーバーに転送してください。

### ShowTermsView

約款ウィンドウを画面に表示します。
ユーザーが約款に同意した場合、同意の有無をサーバーに登録します。
約款に同意した場合、ShowTermsView APIを再度呼び出しても約款ウィンドウが表示されず、すぐに成功コールバックが返されます。
ただし、Gamebaseコンソールで約款再同意項目を**必要**に変更すると、ユーザーが再度約款に同意するまでは約款ウィンドウが表示されます。

> <font color="red">[注意]</font><br/>
>
> * FGamebasePushConfigurationは、約款ウィンドウが表示されていない場合にはnullです。(約款ウィンドウが表示された場合、常に有効なオブジェクトが返されます。)
> * FGamebasePushConfiguration.pushEnabled値は常にtrueです。
> * FGamebasePushConfigurationがnullではない場合、**ログイン後に** IGamebase::Get().GetPush().RegisterPush()を呼び出してください。
#### Optionalパラメータ

* callback：約款同意後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで来るGamebaseResponse.DataContainerオブジェクトはGamebaseResponse.Push.PushConfiguration変換してログイン後、Gamebase.Push.RegisterPush APIに使用できます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cpp
void ShowTermsView(const FGamebaseDataContainerDelegate& onCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebaseが初期化されていません。 |
| LAUNCHING\_SERVER\_ERROR(2001) | ローンチサーバーから取得した項目に約款関連内容がない場合に発生するエラーです。<br/>正常な状況ではないため、Gamebase担当者にお問い合わせください。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてからもう一度お試しください。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW(6925) | 約款Webビューがまだ終了していないのに再度呼び出されました。 |
| WEBVIEW\_TIMEOUT(7002) | 約款Webビューの表示中にタイムアウトが発生しました。 |
| WEBVIEW\_HTTP\_ERROR(7003) | 約款Webビューのオープン中にHTTPエラーが発生しました。 |

**Example**

```cpp
void Sample::ShowTermsView()
{
    IGamebase::Get().GetTerms().ShowTermsView(
        FGamebaseDataContainerDelegate::CreateLambda([=](const FGamebaseDataContainer* dataContainer, const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView succeeded."));
                
                // If the 'FGamebasePushConfiguration' is not null,
                // save the 'FGamebasePushConfiguration' and use it for IGamebase::Get().GetPush().RegisterPush() after IGamebase::Get().Login().
                const auto pushConfiguration = FGamebasePushConfiguration::From(dataContainer);
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView failed. (error: %d)"), error->code);
            }
        })
    );
}
```


### QueryTerms

Gamebaseは、単純な形式のWebビューで約款を表示します。
ゲームのUIに合った約款を直接製作したい場合、QueryTerms APIを呼び出してGamebaseコンソールに設定した約款情報を取得して活用できます。

ログイン後に呼び出す場合は、ゲームユーザーが約款に同意したかどうかも一緒に確認できます。

> <font color="red">[注意]</font><br/>
>
> * GamebaseResponse.Terms.ContentDetail.requiredがtrueの必須項目はGamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * 必須項目は常にtrueで保存されるので、保存する意味がないためです。
> * プッシュ受信同意状況もGamebaseサーバーに保存されないため、agreed値は常にfalseが返されます。
>     * ユーザーのプッシュ受信同意状況は、Gamebase.Push.QueryPush APIを通して確認してください。
> * コンソールで「基本約款設定」を行わない場合、約款言語と異なる国コードに設定された端末でqueryTerms APIを呼び出すと、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生します。
>     * コンソールで「基本約款設定」を行ったり、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生した時は、約款を表示しないように処理してください。
#### Requiredパラメータ
* callback：API呼び出し結果をユーザーにコールバックで伝えます。コールバックで来るGamebaseResponse.Terms.QueryTermsResultでコンソールに設定された約款情報を取得できます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
void QueryTerms(const FGamebaseQueryTermsResultDelegate& onCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebaseが初期化されていません。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | 約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | 端末の国コードに合った約款情報がコンソールに登録されていません。 |

**Example**

```cpp
void Sample::QueryTerms()
{
    IGamebase::Get().GetTerms().QueryTerms(
        FGamebaseQueryTermsResultDelegate::CreateLambda([=](const FGamebaseQueryTermsResult* data, const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms failed. (error: %d)"), error->code);
            }
        })
    );
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| termsSeq             | int32                           | 約款全体KEY.<br/>updateTerms APIの呼び出し時に必要な値です。          |
| termsVersion         | FString                         | 約款バージョン。<br/>updateTerms APIの呼び出し時に必要な値です。              |
| termsCountryType     | FString                         | 約款タイプ。<br/> - KOREAN：韓国約款 <br/> - GDPR：EU約款 <br/> - ETC：その他約款       |
| contents             | TArray<FGamebaseTermsContent>   | 約款項目情報        |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int32                 | 約款項目KEY         | 
| name                 | FString               | 約款項目名       |
| required             | bool                  | 同意必須かどうか        |
| agreePush            | FString               | 広告性プッシュ同意状況<br/> - NONE：同意しない<br/> - ALL：全て同意 <br/> - DAY：1週間プッシュ同意<br/> - NIGHT：夜間プッシュ同意        |
| agreed               | bool                  | 該当約款項目に対するユーザーの同意有無          |
| node1DepthPosition   | int32                 | 1段階項目表示順序。           |
| node2DepthPosition   | int32                 | 2段階項目表示順序。<br/> ない場合 -1           |
| detailPageUrl        | FString               | 約款詳細表示URL。<br/> ない場合null |


### UpdateTerms

QueryTerms APIで取得した約款情報でUIを直接製作した場合、
ゲームユーザーが約款に同意した内容をUpdateTerms APIを通してGamebaseサーバーに転送してください。

選択約款同意をキャンセルするように、約款に同意した内容を変更する目的でも活用できます。


> <font color="red">[注意]</font><br/>
>
> プッシュ受信同意状況はGamebaseサーバーに保存されません。
> プッシュ受信同意状況は**ログイン後に** Gamebase.Push.RegisterPush APIを呼び出して保存してください。
>
#### Requiredパラメータ
* configuration：サーバーに登録するユーザーの選択約款情報です。

#### Optionalパラメータ

* callback：選択約款情報をサーバーに登録後、ユーザーにコールバックで伝えます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cpp
void UpdateTerms(const FGamebaseUpdateTermsConfiguration& configuration, const FGamebaseErrorDelegate onCallback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebaseが初期化されていません。 |
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 登録されていない約款Seq値を設定しました。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてからもう一度お試しください。 |


**Example**

```cpp
void Sample::UpdateTerms(int32 termsSeq, const FString& termsVersion, int32 termsContentSeq, bool agreed)
{
    TArray<FGamebaseTermsContent> contents;
    contents.Add(FGamebaseTermsContent { termsContentSeq, agreed });
    
    IGamebase::Get().GetTerms().UpdateTerms(
        FGamebaseUpdateTermsConfiguration { termsSeq, termsVersion, contents },
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            if (Gamebase::IsSuccess(error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms failed. (error: %d)"), error->code);
            }
        })
    );
}
```


#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsVersion         | **M**                      | FString                    | 約款バージョン。<br/>queryTerms APIを呼び出して取得した値を伝達する必要があります。   |
| termsSeq             | **M**                      | int32                       | 約款全体KEY.<br/>queryTerms APIを呼び出して取得した値を伝達する必要があります。             |
| contents             | **M**                      | List< Content > | 選択約款ユーザー同意情報 |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int32                | 選択約款項目KEY      |
| agreed               | **M**                      | bool               | 選択約款項目同意状況 |


## Webview

### Show WebView

WebViewを表示します。<br/>

##### Requiredパラメータ
* url：パラメータに転送されるurlは、有効な値である必要があります。

##### Optionalパラメータ(現在はRequireパラメータですが、今後のバージョンでOptionalに変更予定)
* configuration：GamebaseWebViewConfigurationでWebViewのレイアウトを変更できます。
* closeCallback：WebViewが終了する時、ユーザーにコールバックで伝えます。
* schemeList：ユーザーが受け取りたいカスタムSchemeリストを指定します。
* schemeEvent：schemeListに指定したカスタムSchemeを含むurlをコールバックで伝えます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowWebView(const FString& url, const FGamebaseWebViewConfiguration& configuration, FGamebaseErrorDelegate& onCloseCallback, const TArray<FString>& schemeList, const FGamebaseSchemeEventDelegate& onSchemeEvent);
```

**Example**
```cpp
void Sample::ShowWebView(const FString& url)
{
    FGamebaseWebViewConfiguration configuration{ TEXT("Title"), GamebaseScreenOrientation::Unspecified, 128, 128, 128, 255, 40, true, "", "" };

    TArray<FString> schemeList{ TEXT("customScheme://openBrowser") };

    IGamebase::Get().GetWebView().ShowWebView(url, configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) {
            Result(ANSI_TO_TCHAR(__FUNCTION__), TEXT("Close webview"));
        }),
        schemeList,
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& scheme, const FGamebaseError* error) {
        if (Gamebase::IsSuccess(error))
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), true, *FString::Printf(TEXT("scheme= %s"), *scheme));
        }
        else
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), false, GamebaseJsonUtil::UStructToJsonObjectString(*error));
        }
    }));
}
```


#### FGamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | FString                                   | WebViewのタイトル               |
| orientation              | GamebaseScreenOrientation::Unspecified   | 未指定 |
|                          | GamebaseScreenOrientation::Portrait      | 縦モード                     |
|                          | GamebaseScreenOrientation::Landscape     | 横モード                     |
|                          | GamebaseScreenOrientation::LandscapeReverse | 横モードを180度回転              |
| contentMode              | GamebaseWebViewContentMode::Recommended        | 現在のプラットフォームの推薦ブラウザ |
|                          | GamebaseWebViewContentMode::Mobile             | モバイルブラウザ         |
|                          | GamebaseWebViewContentMode::Desktop            | デスクトップブラウザ       |
| colorR                   | 0～255                                    | ナビゲーションバーの色相R            |
| colorG                   | 0～255                                    | ナビゲーションバーの色相G                |
| colorB                   | 0～255                                    | ナビゲーションバーの色相B                |
| colorA                   | 0～255                                    | ナビゲーションバーの色相Alpha                |
| buttonVisible            | true or false                            | 戻るボタン有効/無効          |
| barHeight                | height                                   | ナビゲーションバーの高さ                  |
| backButtonImageResource  | ID of resource                           | 戻るボタンのイメージ              |
| closeButtonImageResource | ID of resource | 閉じるボタンのイメージ |
| url | "http://" or "https://" or "file://" | Web URL |


> [TIP]
>
> iPadOS 13以上でWebViewは基本的にデスクトップモードです。
> contentMode =`GamebaseWebViewContentMode.MOBILE`設定でモバイルモードに変更できます。

#### Predefined Custom Scheme

Gamebaseで指定しておいたSchemeです。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebViewを閉じる |
| gamebase://getMaintenanceInfo | メンテナンス内容をWebPageに表示 |
| gamebase://getUserId          | 現在ログインしているゲームユーザーのユーザーIDを表示 |
| gamebase://goBack | WebView戻る |


### Close WebView

次のAPIを利用して、表示されているWebViewを閉じることができます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseWebView();
```

**Example**CloseWebview
```cpp
void Sample::CloseWebView()
{
    IGamebase::Get().GetWebView().CloseWebView();
}
```


## Open External Browser

次のAPIを通して外部ブラウザーを開くことができます。パラメータに転送されるurlは有効な値である必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void OpenWebBrowser(const FString& url);
```

**Example**
```cpp
void Sample::OpenWebBrowser(const FString& url)
{
    IGamebase::Get().GetWebView().OpenWebBrowser(url);
}
```


## Alert

システム通知を表示できます。
システム通知にコールバックを登録することもできます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowAlert(const FString& title, const FString& message);
void ShowAlert(const FString& title, const FString& message, const FGamebaseAlertCloseDelegate& onCloseCallback);
```

**Example**
```cpp
void Sample::ShowAlert(const FString& title, const FString& message)
{
    IGamebase::Get().GetUtil().ShowAlert(title, message);
}

void Sample::ShowAlertEvent(const FString& title, const FString& message)
{
    IGamebase::Get().GetUtil().ShowAlert(title, message, FGamebaseAlertCloseDelegate::CreateLambda([=]()
    {
            UE_LOG(GamebaseTestResults, Display, TEXT("ShowAlert ButtonClick."));
    }));
}
```

## Toast

次のAPIを使用して、簡単にメッセージを表示できます。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void ShowToast(const FString& message, EGamebaseToastExposureTime exposureTimeType);
```

**Example**
```cpp
void Sample::ShowToast(const FString& message, EGamebaseToastExposureTime exposureTimeType)
{
    IGamebase::Get().GetUtil().ShowToast(message, exposureTimeType);
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知の表示中タイムアウトが発生しました。 |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./error-code/#client-sdk)
