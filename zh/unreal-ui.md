## Game > Gamebase > Unreal SDK使用指南 > UI

## ImageNotice

通过在控制台中注册图片，向用户推送图片通知。

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

在页面上显示图片通知。

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
| colorR                   | 0~255                                    | Navigation Bar颜色R            |
| colorG                   | 0~255                                    | Navigation Bar颜色G                |
| colorB                   | 0~255                                    | Navigation Bar颜色B                |
| colorA                   | 0~255                                    | Navigation Bar颜色Alpha                |
| timeOut                  | int64        | 图片通知最大加载时间(单位 : millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

通过调用closeImageNotices API，可关闭当前显示的所有图片通知。 

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void CloseImageNotices();
```


## Terms

在Gamebase控制台中显示注册的条款。 
 
![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

调用showTermsView API显示条款窗时通过Webview显示。  
如果要创建符合Game UI的条款窗，可通过调用QueryTerms API显示Gamebase控制台中注册的条款项目。
若用户已同意条款，请将各项目的”同意与否"通过调用UpdateTerms API传送到Gamebase服务器。 

### ShowTermsView

在页面上显示条款窗。
若用户已同意条款，在服务器中注册”同意与否“。
如果已同意条款，即使重新调用ShowTermsView API也不显示条款窗，而直接返还“成功回调”。
但，如果在Gamebase控制台中将”重新同意条款”项目更改为**必须**，用户再次同意条款之前一直显示条款窗。

> <font color="red">[注意]</font><br/> 
>
> * 未显示条款窗时，FGamebasePushConfiguration为null。(显示条款窗时，始终返还有效的对象。)
> * FGamebasePushConfiguration.pushEnabled值始终为true。
> * 如果FGamebasePushConfiguration不是null，**登录后**调用IGamebase::Get().GetPush().RegisterPush()。

#### Optional参数

* callback : 同意条款后关闭条款窗时，通过回调通知用户。可将作为回调返回的GamebaseResponse.DataContainer对象变换为GamebaseResponse.Push.PushConfiguration，登录后用于调用Gamebase.Push.RegisterPush API。  
                

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
| NOT\_INITIALIZED(1) | 未初始化Gamebase。  |
| LAUNCHING\_SERVER\_ERROR(2001) | 是从启动服务器返还的项目不包含相关条款内容时出现的错误。<br/>出现此问题时，请联系Gamebase负责人员。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 上一次调用的Terms API未完成。<br/>请稍后再试。 |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW(6925) | 条款Webview尚未结束，但已再次被调用。 | 	
| WEBVIEW\_TIMEOUT(7002) | 显示条款Webview时出现超时错误。 |
| WEBVIEW\_HTTP\_ERROR(7003) | 打开条款Webview时出现HTTP错误。 | 

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

Gamebase通过Webview以简单形式显示条款。 
如果要创建符合游戏UI的条款窗，可通过调用QueryTerms API ，在Gamebase控制台中获取条款信息后使用。

如果登录后调用，则可确认游戏用户是否同意条款。 

> <font color="red">[注意]</font><br/>
>
> * 因Gamebase服务器不保存GamebaseResponse.Terms.ContentDetail.required为true的必要项目，agreed值将始终返回为false。 
>     * 这是因为必要项目将始终保存为true，不需要保存。
> * 因gamebase服务器不保存”是否接收推送”，agreed值将始终返回为false。
>     * 如需查看用户”是否接收推送”，请调用Gamebase.Push.QueryPush API 。
> * 使用与条款语言不同的国家代码在设置的终端机上调用queryTerms API，则将出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)**错误。
>     * 在控制台中设置”基本条款”或出现**UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 错误时，请不要显示条款。 
            
#### Required参数
* callback : 通过回调通知用户调用API的结果。可使用通过回调被返回的GamebaseResponse.Terms.QueryTermsResult来获取控制台中设置的条款信息。
 

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
| NOT\_INITIALIZED(1) | 未初始化Gamebase。 |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | 在控制台中未注册条款信息。 |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | 在控制台中未注册符合终端机国家代码的条款信息。 |

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
| termsSeq             | int32                           | 所有条款KEY<br/>是调用updateTerms API时所需的值。          |
| termsVersion         | FString                         | 条款版本<br/>是调用updateTerms API时所需的值。              |
| termsCountryType     | FString                         | 条款类型<br/> - KOREAN : 韩国条款 <br/> - GDPR : 欧洲条款 <br/> - ETC : 其他条款         |
| contents             | TArray<FGamebaseTermsContent>   | 条款项目信息          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int32                 | 条款项目KEY         | 
| name                 | FString               | 条款项目名称         |
| required             | bool                  | 是否需要必须同意         |
| agreePush            | FString               | 是否同意接收 广告性推送<br/> - NONE : 不同意。 <br/> - ALL : 全部同意。 <br/> - DAY : 同意在白天接收推送。<br/> - NIGHT : 同意在夜间接收推送。          |
| agreed               | bool                  | 用户是否同意相关条款项目           |
| node1DepthPosition   | int32                 | 显示第1阶段项目的顺序           |
| node2DepthPosition   | int32                 | 显示第2阶段项目的顺序 <br/> 没有 -1           |
| detailPageUrl        | FString               | 查看条款详情URL<br/> 没有 -null |


### UpdateTerms

如果使用调用QueryTerms API后被返回的条款信息创建了UI，
请调用UpdateTerms API将游戏用户同意条款的记录传送到Gamebase服务器。 

不仅 可用于取消”可选条款同意”，也可用于修改同意条款的记录。


> <font color="red">[注意]</font><br/>
>
> Gamebase服务器不保存”是否同意接收推送”。
> 当保存”是否同意接收推送时，**登录后**通过调用Gamebase.Push.RegisterPush API后保存。
>

#### Required参数
* configuration : 需要在服务器中注册的用户的可选条款信息。
 
#### Optional参数

* callback : 在服务器中注册可选条款信息，并通过回调通知用户。


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
| NOT\_INITIALIZED(1) | 未初始化Gamebase。 |
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 设置了未注册的条款Seq值。 |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 上一次调用的Terms API未完成。<br/>请稍后再试。 |


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
| termsVersion         | **M**                      | FString                    | 条款版本<br/>需要传送调用queryTerms API获取的值。   |
| termsSeq             | **M**                      | int32                       | 所有条款KEY<br/>需要传送调用queryTerms API获取的值。             |
| contents             | **M**                      | List< Content > | 用户同意可选条款的信息  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int32                | 可选条款项目KEY      |
| agreed               | **M**                      | bool               | 是否同意可选条款项目  |

      
## Webview

### Show WebView

显示WebView。<br/>

##### Required参数
* url : 作为参数发送的url必须是有效值。

##### Optional参数(是Require参数，但在以后版本需要更改为Optional。)
* configuration : 通过使用GamebaseWebViewConfiguration可更改WebView的布局。
* closeCallback : 关闭WebView时通过回调通知用户。
* schemeList : 指定用户需要接收的自定义Scheme列表。
* schemeEvent : 通过回调通知包含指定为schemeList的自定义Scheme url。

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
| title                    | string                                   | WebView的标题                 |
| orientation              | GamebaseScreenOrientation::Unspecified    | 未指定 |
|                          | GamebaseScreenOrientation::Portrait       | 纵向模式                       |
|                          | GamebaseScreenOrientation::Landscape      | 横向模式                       |
|                          | GamebaseScreenOrientation::LandscapeReverse | 将横向模式旋转180度              |
| contentMode              | GamebaseWebViewContentMode::Recommended        | 当前平台推荐的浏览器    |
|                          | GamebaseWebViewContentMode::Mobile             | 移动浏览器            |
|                          | GamebaseWebViewContentMode::Desktop            | 桌面浏览器          |
| colorR                   | 0~255                                    | Navigation Bar颜色R            |
| colorG                   | 0~255                                    | Navigation Bar颜色G                |
| colorB                   | 0~255                                    | Navigation Bar颜色B                |
| colorA                   | 0~255                                    | Navigation Bar颜色Alpha                |
| buttonVisible            | true or false                            | 激活或不激活返回按钮          |
| barHeight                | height                                   | Navigation Bar高度                  |
| backButtonImageResource  | ID of resource                           | 返回按钮图片                |
| closeButtonImageResource | ID of resource | 关闭按钮图片 |
| url | "http://" or "https://" or "file://" | 网络URL |

> [TIP]
>
> iPadOS 13以上中的WebView基本上是桌面模式。
> 可通过contentMode = ”GamebaseWebViewContentMode.MOBILE”设置来更改移动模式。

#### Predefined Custom Scheme

是Gamebase指定的Scheme。

| scheme | 用途 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | 关闭WebView。|
| gamebase://getMaintenanceInfo | 在WebPage显示维护结果。 |
| gamebase://getUserId          | 显示当前登录的游戏用户ID。 |
| gamebase://goBack | 返回到WebView的上一页。|


### Close WebView

可以通过调用以下API来关闭WebView。

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

可以通过调用以下API打开外部浏览器。作为参数传送的url必须是有效值。

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

可以显示系统通知。
可以在系统通知注册回调。

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

通过调用以下API可轻松地显示消息。

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 显示图片通知时出现了超时错误。|
| UI\_UNKNOWN\_ERROR | 6999       | 未知错误(是未定义的错误)。|

* 关于所有错误代码，请参考以下文档。 
    * [错误代码](./error-code/#client-sdk)
