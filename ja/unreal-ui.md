## Game > Gamebase > Unreal SDKご利用ガイド > UI

## ImageNotice

コンソールにイメージを登録した後、ユーザーに告知を表示できます。

![ImageNotice Example](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/DevelopersGuide/imageNotice-guide-landscape-ja_v3.png)

### Show ImageNotices

イメージ告知を画面に表示します。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void ShowImageNotices(FGamebaseImageNoticeConfiguration& Configuration, const FGamebaseErrorDelegate& CloseCallback, const FGamebaseImageNoticeEventDelegate& EventCallback = {});
```

**Example**

```cpp
void USample::ShowImageNotices(int32 ColorR, int32 ColorG, int32 ColorB, int32 ColorA, int64 TimeOut)
{
    FGamebaseImageNoticeConfiguration Configuration;
    Configuration.BackgroundColor = FColor(ColorR, ColorG, colorB, colorA);
    Configuration.TimeOut = TimeOut;

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetImageNotice()->ShowImageNotices(Configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error) {
            // Called when the entire imageNotice is closed.
            ...
        }),
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& Scheme, const FGamebaseError* Error) {
            // Called when custom event occurred.
            ...
        })
    );
}
```

#### FGamebaseImageNoticeConfiguration

| Parameter                              | Values                                   | Description        |
| -------------------------------------- | ---------------------------------------- | ------------------ |
| BackgroundColor          | FColor       | バックグラウンド背景色           |
| timeOut                  | int64        | イメージ告知最大ローディング時間(単位: millisecond)<br/>**default**: 5000 |


### Close ImageNotices

CloseImageNotices APIを呼び出して現在表示中のイメージ告知を全て終了できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

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
> * FGamebasePushConfigurationは、約款ウィンドウが表示されていない場合にはnullです(約款ウィンドウが表示された場合、常に有効なオブジェクトが返されます。)。
> * FGamebasePushConfiguration.bPushEnabled 値は常にtrueです。
> * FGamebasePushConfigurationがnullではない場合、**ログイン後に** Subsystem->GetPush()->RegisterPush()を呼び出してください。

#### Optionalパラメータ

* GamebaseTermsConfiguration : GamebaseTermsConfigurationオブジェクトを介して強制的に約款同意ウィンドウを表示するかどうかなどの設定を変更できます。
* Callback ：約款同意後、約款ウィンドウが終了する時、ユーザーにコールバックで伝えます。コールバックで来るGamebaseResponse.DataContainerオブジェクトはGamebaseResponse.Push.PushConfiguration変換してログイン後、Gamebase.Push.RegisterPush APIに使用できます。

**FGamebaseTermsConfiguration** 

| API | Mandatory(M) / Optional(O) | Description | 
| --- | --- | --- | 
| bForceShow | O | 約款に同意した場合、showTermsView APIを再度呼び出しても約款ウィンドウが表示されませんが、これを無視して強制的に約款ウィンドウを表示します。<br>**default** : false |
| bEnableFixedFontSize | O | 約款ウィンドウのフォントサイズを固定するかどうかを決定します。<br>**default**：false<br/>**Android Only** |

**FGamebaseShowTermsViewResult**

| Parameter              | Values                          | Description         |
| ---------------------- | --------------------------------| ------------------- |
| bIsTermsUIOpened        | bool                            | **true**：約款ウィンドウが表示され、ユーザーが同意して約款ウィンドウが終了しました。<br>**false**：すでに約款に同意していて、約款ウィンドウが表示されずに約款ウィンドウが終了しました。        |

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowTermsView(const FGamebaseDataContainerDelegate& Callback);
void ShowTermsView(const FGamebaseTermsConfiguration& configuration, const FGamebaseDataContainerDelegate& Callback);
```

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

```cpp
void USample::ShowTermsView()
{
    FGamebaseTermsConfiguration Configuration { true };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->ShowTermsView(Configuration,
        FGamebaseDataContainerDelegate::CreateLambda([=](const FGamebaseDataContainer* DataContainer, const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView succeeded."));
                
                const auto result = FGamebaseShowTermsResult::From(DataContainer);
                if (result.IsValid())
                {
                    // Save the 'PushConfiguration' and use it for RegisterPush() after Login().
                    SavedPushConfiguration = FGamebasePushConfiguration::From(DataContainer);
                }
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("ShowTermsView failed. (Error: %d)"), Error->Code);
            }
        })
    );
}

void USample::AfterLogin()
{
    // Call RegisterPush with saved PushConfiguration.
    if (SavedPushConfiguration != null)
    {
        Gamebase.Push.RegisterPush(SavedPushConfiguration, (Error) =>
        {
            ...
        });
    }
}
```


### QueryTerms

Gamebaseは、単純な形式のWebビューで約款を表示します。
ゲームのUIに合った約款を直接製作したい場合、QueryTerms APIを呼び出してGamebaseコンソールに設定した約款情報を取得して活用できます。

'「選択」約款項目はログイン後に呼び出すと、同意の有無も一緒に知ることができます。ただし、「必須」項目の同意有無は常にfalseで返されます。

> <font color="red">[注意]</font><br/>
>
> * GamebaseResponse.Terms.ContentDetail.requiredがtrueの必須項目は同意有無をGamebaseサーバーに保存しないため、bAgreed値は常にfalseが返されます。
>     * 約款必須項目に同意していない場合、ゲーム進行またはゲームログインをさせてはいけないので、約款ポップアップが閉じていてログインしている状態であれば、当然約款必須項目に同意したのと同じです。そのため、ログインしたユーザーはすでに必須項目にすべて同意した状態なので、あえて同意の有無を保存する必要がないためです。
> * プッシュ受信同意状況もGamebaseサーバーに保存されないため、bAgreed 値は常にfalseが返されます。
>     * ユーザーのプッシュ受信同意状況は、Gamebase.Push.QueryPush APIを通して確認してください。
> * コンソールで「基本約款設定」を行わない場合、約款言語と異なる国コードに設定された端末でqueryTerms APIを呼び出すと、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生します。
>     * コンソールで「基本約款設定」を行ったり、**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**エラーが発生した時は、約款を表示しないように処理してください。
#### Requiredパラメータ
* Callback：API呼び出し結果をユーザーにコールバックで伝えます。コールバックで来るGamebaseResponse.Terms.QueryTermsResultでコンソールに設定された約款情報を取得できます。


**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cs
void QueryTerms(const FGamebaseQueryTermsResultDelegate& Callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 約款情報がコンソールに登録されていません。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 端末国コードに合った約款情報がコンソールに登録されていません。 |

**Example**

```cpp
void USample::QueryTerms()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->QueryTerms(
        FGamebaseQueryTermsResultDelegate::CreateLambda([=](const FGamebaseQueryTermsResult* Data, const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("QueryTerms failed. (Error: %d)"), Error->Code);
            }
        })
    );
}
```

#### GamebaseResponse.Terms.QueryTermsResult

| Parameter            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| TermsSeq             | int32                           | 約款全体KEY.<br/>updateTerms APIの呼び出し時に必要な値です。          |
| TermsVersion         | FString                         | 約款バージョン。<br/>updateTerms APIの呼び出し時に必要な値です。              |
| termsCountryType     | FString                         | 約款タイプ。<br/> - KOREAN：韓国約款 <br/> - GDPR：EU約款 <br/> - ETC：その他約款       |
| Contents             | TArray<FGamebaseTermsContent>   | 約款項目情報        |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| TermsContentSeq      | int32                 | 約款項目KEY         | 
| Name                 | FString               | 約款項目名       |
| Required             | bool                  | 同意必須かどうか        |
| AgreePush            | FString               | 広告性プッシュ同意状況<br/> - NONE：同意しない<br/> - ALL：全て同意 <br/> - DAY：1週間プッシュ同意<br/> - NIGHT：夜間プッシュ同意        |
| bAgreed               | bool                  | 該当約款項目に対するユーザーの同意有無          |
| Node1DepthPosition   | int32                 | 1段階項目表示順序。           |
| Node2DepthPosition   | int32                 | 2段階項目表示順序。<br/> ない場合 -1           |
| DetailPageUrl        | FString               | 約款詳細表示URL。<br/> ない場合null |


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
* Configuration サーバーに登録するユーザーの選択約款情報です。

#### Optionalパラメータ

* callback：選択約款情報をサーバーに登録後、ユーザーにコールバックで伝えます。


**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void UpdateTerms(const FGamebaseUpdateTermsConfiguration& Configuration, const FGamebaseErrorDelegate Callback);
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebaseが初期化されていません。 |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 登録されていない約款Seq値を設定しました。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | 以前に呼び出されたTerms APIがまだ完了していません。<br/>しばらくしてから再度試行してください。 |


**Example**

```cpp
void USample::UpdateTerms(int32 TermsSeq, const FString& TermsVersion, int32 TermsContentSeq, bool bAgreed)
{
    TArray<FGamebaseTermsContent> Contents;
    Contents.Add(FGamebaseTermsContent { TermsContentSeq, bAgreed });
    
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetTerms()->UpdateTerms(
        FGamebaseUpdateTermsConfiguration { TermsSeq, TermsVersion, Contents },
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error) {
            if (Gamebase::IsSuccess(Error))
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms succeeded."));
            }
            else
            {
                UE_LOG(GamebaseTestResults, Display, TEXT("UpdateTerms failed. (Error: %d)"), Error->Code);
            }
        })
    );
}
```

#### GamebaseRequest.Terms.UpdateTermsConfiguration

| Parameter            | Mandatory(M) / Optional(O) | Values                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| TermsVersion         | **M**                      | FString                    | 約款バージョン。<br/>queryTerms APIを呼び出して取得した値を伝達する必要があります。   |
| TermsSeq             | **M**                      | int32                       | 約款全体KEY.<br/>queryTerms APIを呼び出して取得した値を伝達する必要があります。             |
| Contents             | **M**                      | List< Content > | 選択約款ユーザー同意情報 |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| TermsContentSeq      | **M**                      | int32                | 選択約款項目KEY      |
| bAgreed               | **M**                      | bool               | 選択約款項目同意状況 |

### IsShowingTermsView

現在約款ウィンドウが画面に表示されているかどうかを知ることができます。

**API**

```cpp
bool IsShowingTermsView();
```

**Example**

```cpp
void USample::IsShowingTermsView()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    bool isShowingTermsView = Subsystem->GetTerms()->IsShowingTermsView();
    UE_LOG(GamebaseTestResults, Display, TEXT("IsShowingTermsView : %s"), isShowingTermsView ? TEXT("true") : TEXT("false"));
}
```

## Webview

### Show WebView

WebViewを表示します。<br/>

##### Requiredパラメータ
* Url：パラメータに転送されるurlは、有効な値である必要があります。

##### Optionalパラメータ(現在はRequireパラメータですが、今後のバージョンでOptionalに変更予定)
* Configuration：GamebaseWebViewConfigurationでWebViewのレイアウトを変更できます。
* closeCallback：WebViewが終了する時、ユーザーにコールバックで伝えます。
* SchemeList：ユーザーが受け取りたいカスタムSchemeリストを指定します。
* SchemeEvent：schemeListに指定したカスタムスキームを含むurlをコールバックで伝えます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void ShowWebView(const FString& Url, const FGamebaseWebViewConfiguration& Configuration, FGamebaseErrorDelegate& CloseCallback, const TArray<FString>& SchemeList, const FGamebaseSchemeEventDelegate& SchemeEvent);
```

**Example**
```cpp
void USample::ShowWebView(const FString& Url)
{
    FGamebaseWebViewConfiguration Configuration;
    Configuration.Title = TEXT("Title");

    TArray<FString> SchemeList{ TEXT("customScheme://openBrowser") };

    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->ShowWebView(Url, Configuration,
        FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* Error) {
            Result(ANSI_TO_TCHAR(__FUNCTION__), TEXT("Close webview"));
        }),
        SchemeList,
        FGamebaseSchemeEventDelegate::CreateLambda([=](const FString& Scheme, const FGamebaseError* Error) {
        if (Gamebase::IsSuccess(Error))
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), true, *FString::Printf(TEXT("Scheme= %s"), *Scheme));
        }
        else
        {
            Result(ANSI_TO_TCHAR(__FUNCTION__), false, GamebaseJsonUtil::UStructToJsonObjectString(*Error));
        }
    }));
}
```


#### FGamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| Title                    | FString                                   | WebViewのタイトル               |
| Orientation              | GamebaseScreenOrientation::Unspecified   | 未指定(**default**)            |
|                          | GamebaseScreenOrientation::Portrait      | 縦モード                     |
|                          | GamebaseScreenOrientation::Landscape     | 横モード                     |
|                          | GamebaseScreenOrientation::LandscapeReverse | 横モードを180度回転              |
| ContentMode              | GamebaseWebViewContentMode::Recommended     | 現在のプラットフォームの推薦ブラウザ(**default**)   |
|                          | GamebaseWebViewContentMode::Mobile          | モバイルブラウザ         |
|                          | GamebaseWebViewContentMode::Desktop         | デスクトップブラウザ       |
| NavigationColor          | FColor                                   | ナビゲーションバーの色<br>**default**: FColor(18, 93, 230, 255)   |
| NavigationTitleColor     | FColor                                   | ナビゲーションバータイトルの色<br>**default**: FColor::White          |
| NavigationIconTintColor  | TOptional&lt;FColor&gt;                  | ナビゲーションバーアイコンティントカラー |
| NavigationBarHeight      | height                                   | ナビゲーションバーの高さ<br>**Androidのみ**                 |
| bIsNavigationBarVisible  | true or false                            | ナビゲーションバー有効または無効<br>**default**: true    |
| bIsBackButtonVisible     | true or false                            | 戻るボタンの有効または無効<br>**default**: true   |
| BackButtonImageResource  | ID of resource                           | 戻るボタンの画像        |
| CloseButtonImageResource | ID of resource                           | 閉じるボタンの画像            |
| bEnableFixedFontSize     | true or false                            | 約款ウィンドウの文字サイズを固定するかどうか決定<br>**default**: false<br>**Android限定**     |
| bRenderOutSideSafeArea   | true or false                            | SafeAreaを無視し、Cutout領域にもレンダリング<br>**default**: false<br>**Android限定**   |
| CutoutColor              | TOptional<FColor>                        | SafeArea以外のCutout領域背景色<br>**Android限定**                            |

> [TIP]
>
> iPadOS 13以上でWebViewは基本的にデスクトップモードです。
> contentMode =`GamebaseWebViewContentMode::MOBILE`設定でモバイルモードに変更できます。

#### Predefined Custom Scheme

Gamebaseで指定しておいたスキームです。

| Scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | WebViewを閉じる |
| gamebase://getMaintenanceInfo | メンテナンス内容をWebPageに表示 |
| gamebase://getUserId          | 現在ログインしているゲームユーザーのユーザーIDを表示 |
| gamebase://goBack | WebView戻る |


### Close WebView

次のAPIを利用して、表示されているWebViewを閉じることができます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNREAL_WINDOWS

```cpp
void CloseWebView();
```

**Example**CloseWebview
```cpp
void USample::CloseWebView()
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->CloseWebView();
}
```


## Open External Browser

次のAPIを通して外部ブラウザーを開くことができます。パラメータに転送されるurlは有効な値である必要があります。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void OpenWebBrowser(const FString& Url);
```

**Example**
```cpp
void USample::OpenWebBrowser(const FString& Url)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetWebView()->OpenWebBrowser(Url);
}
```


## Alert

システム通知を表示できます。
システム通知にコールバックを登録することもできます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowAlert(const FString& Title, const FString& Message);
void ShowAlert(const FString& Title, const FString& Message, const FGamebaseAlertCloseDelegate& CloseCallback);
```

**Example**
```cpp
void USample::ShowAlert(const FString& Title, const FString& Message)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowAlert(Title, Message);
}

void USample::ShowAlertEvent(const FString& Title, const FString& Message)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowAlert(Title, Message, FGamebaseAlertCloseDelegate::CreateLambda([=]()
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("ShowAlert ButtonClick."));
    }));
}
```

## Toast

次のAPIを使用して、簡単にメッセージを表示できます。

**API**

Supported Platforms
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void ShowToast(const FString& Message, EGamebaseToastExposureTime ExposureTimeType);
```

**Example**
```cpp
void USample::ShowToast(const FString& Message, EGamebaseToastExposureTime ExposureTimeType)
{
    UGamebaseSubsystem* Subsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance());
    Subsystem->GetUtil()->ShowToast(Message, ExposureTimeType);
}
```

## Error Handling

| Error              | Error Code | Description                 |
| ------------------ | ---------- | --------------------------- |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | イメージ告知の表示中タイムアウトが発生しました。 |
| UI\_UNKNOWN\_ERROR | 6999       | 不明なエラーです(定義されていないエラーです)。 |

* エラーコードの一覧は、次の文書を参照してください。
    * [エラーコード](./Error-code/#client-sdk)
