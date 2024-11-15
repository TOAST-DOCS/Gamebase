## Game > Gamebase > Unity SDK 사용 가이드 > UI

## ImageNotice

콘솔에 이미지를 등록한 후 사용자에게 공지를 띄울 수 있습니다.

![ImageNotice Example](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/DevelopersGuide/imageNotice-guide-landscape-ko_v3.png)

### Show ImageNotices

이미지 공지를 화면에 띄워 줍니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowImageNotices(GamebaseRequest.ImageNotice.Configuration configuration, GamebaseCallback.ErrorDelegate closeCallback, GamebaseCallback.GamebaseDelegate<string> eventCallback = null)
```

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 이미지 공지 팝업 창 표시 중 시간이 초과되어 모든 팝업 창을 강제 종료합니다. |
| UI\_IMAGE\_NOTICE\_NOT\_SUPPORTED\_OS | 6902 | 롤링 타입의 경우 API 19 이하의 단말기에서는 이미지 공지를 지원하지 않습니다. |
| WEBVIEW\_HTTP\_ERROR | 7003 | 롤링 타입 이미지 공지 웹뷰 오픈 중 HTTP 오류가 발생했습니다. |
| SERVER\_INVALID\_RESPONSE | 8003 | 서버가 유효하지 않은 응답을 반환했습니다. |

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

사용자 설정 이미지 공지를 화면에 띄워 줍니다.
GamebaseRequest.ImageNotice.Configuration으로 사용자 설정 이미지 공지를 만들 수 있습니다.

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
| colorR                   | 0~255                                    | 백그라운드 배경 색상 R            |
| colorG                   | 0~255                                    | 백그라운드 배경 색상 G                |
| colorB                   | 0~255                                    | 백그라운드 배경 색상 B                |
| colorA                   | 0~255                                    | 백그라운드 배경 색상 Alpha                |
| timeoutMS                | long        | 이미지 공지 최대 로딩 시간(단위: millisecond)<br/>**default**: 5000                     |


### Close ImageNotices

closeImageNotices API를 호출하여 현재 표시 중인 이미지 공지를 모두 종료할 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void CloseImageNotices()
```

## Terms

Gamebase 콘솔에 설정한 약관을 표시합니다.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

ShowTermsView API 는 웹뷰로 약관 창을 표시해줍니다.
Game 의 UI 에 맞는 약관 창을 직접 제작하고자 하는 경우에는 QueryTerms API를 호출하여, Gamebase 콘솔에 설정한 약관 항목을 불러올 수 있습니다.
유저가 약관에 동의했다면 각 항목별 동의 여부를 UpdateTerms API를 통해 Gamebase 서버로 전송하시기 바랍니다.

### ShowTermsView

약관 창을 화면에 띄워 줍니다.
유저가 약관에 동의를 했을 경우, 동의 여부를 서버에 등록합니다.
약관에 동의했다면 ShowTermsView API를 다시 호출해도 약관 창이 표시되지 않고 바로 성공 콜백이 반환됩니다.
단, Gamebase 콘솔에서 약관 재동의 항목을 **필요** 로 변경했다면 유저가 다시 약관에 동의할 때까지는 약관 창이 표시됩니다.

#### Optional 파라미터

* GamebaseTermsConfiguration : GamebaseTermsConfiguration 객체를 통해 강제 약관 동의창 표시여부와 같은 설정을 변경할 수 있습니다. 
* Callback: 약관 동의 후 약관 창이 종료될 때 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseResponse.DataContainer 객체는 GamebaseResponse.Push.PushConfiguration 변환해서 로그인 후 Gamebase.Push.RegisterPush API에 사용할 수 있습니다.


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
| forceShow | O | 약관에 동의했다면 ShowTermsView API를 다시 호출해도 약관 창이 표시되지 않지만, 이를 무시하고 강제로 약관 창을 표시합니다.<br>**default**: false | 
| enableFixedFontSize | O | 약관 창의 글자 크기 고정 여부를 결정합니다.<br>**default**: false<br/>**Android에 한함** |
 

**GamebaseResponse.Terms.ShowTermsViewResult**

| Parameter              | Values                          | Description         |
| ---------------------- | --------------------------------| ------------------- |
| isTermsUIOpened        | bool                            | **true** : 유저가 약관에 동의하여 약관 창이 종료되었습니다.<br>**false** : 이미 약관에 동의하여 약관 창이 표시되지 않고 종료되었습니다.        |
| pushConfiguration      | GamebaseResponse.Push.PushConfiguration           | isTermsUIOpened가 **true**이고, 약관에 푸시 수신 동의 여부를 추가했다면 pushConfiguration은 항상 유효한 객체를 가집니다.<br>그렇지 않을 경우에는 **null**입니다.<br>pushConfiguration이 유효할 때 pushConfiguration.pushEnabled 값은 항상 **true**입니다. |

**ErrorCode**

| Error | Error Code | Description |
| --- | --- | --- |
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| LAUNCHING\_SERVER\_ERROR | 2001 | 론칭 서버에 전달받은 항목에 약관 관련 내용이 없는 경우에 발생하는 에러입니다.<br/>정상적인 상황이 아니므로 Gamebase 담당자에게 문의해주시기 바랍니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | Terms API 호출이 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925 | 약관 웹뷰가 아직 종료되지 않았는데 다시 호출되었습니다. |
| WEBVIEW\_TIMEOUT | 7002 | 약관 웹뷰 표시 중 타임아웃이 발생했습니다. |
| WEBVIEW\_HTTP\_ERROR | 7003 | 약관 웹뷰 오픈 중 HTTP 오류가 발생했습니다. |

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

### QueryTerms

Gamebase는 단순한 형태의 웹뷰로 약관을 표시합니다.
게임UI에 맞는 약관을 직접 제작하고자 하신다면, QueryTerms API를 호출하여 Gamebase 콘솔에 설정한 약관 정보를 내려받아 활용하실 수 있습니다.

로그인 후에 호출하신다면 게임유저가 약관에 동의했는지 여부도 함께 확인할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * GamebaseResponse.Terms.ContentDetail.required 가 true인 필수 항목은 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false로 반환됩니다.
>     * 필수 항목은 항상 true로만 저장되기 때문입니다.
> * 푸시 수신 동의 여부도 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false로 반환됩니다.
>     * 유저의 푸시 수신 동의 여부는 Gamebase.Push.QueryPush API를 통해 확인하시기 바랍니다.
> * 콘솔에서 '기본 약관 설정'을 하지 않는 경우 약관 언어와 다른 국가 코드로 설정된 단말기에서 queryTerms API를 호출하면 `UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)` 오류가 발생합니다.
>     * 콘솔에서 '기본 약관 설정'을 하거나, `UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)` 오류가 발생했을 때는 약관을 표시하지 않도록 처리하시기 바랍니다.
> * Standalone 플랫폼에서는 푸시와 관련된 기능을 지원하지 않으므로, 게임 UI에 해당 약관이 노출되지 않도록 주의합니다.

#### Required 파라미터
* Callback: API 호출 결과를 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseResponse.Terms.QueryTermsResult로 콘솔에 설정된 약관 정보를 얻을 수 있습니다.
 

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
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921 | 약관 정보가 콘솔에 등록되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922 | 단말기 국가코드에 맞는 약관 정보가 콘솔에 등록되어 있지 않습니다. |

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
| termsSeq             | int                             | 약관 전체 KEY.<br/>updateTerms API 호출 시 필요한 값입니다.          |
| termsVersion         | string                          | 약관 버전.<br/>updateTerms API 호출 시 필요한 값입니다.              |
| termsCountryType     | string                          | 약관 타입.<br/> - KOREAN: 한국 약관 <br/> - GDPR: 유럽 약관 <br/> - ETC: 기타 약관         |
| contents             | List< ContentDetail > | 약관 항목 정보          |


#### GamebaseResponse.Terms.ContentDetail

| Parameter            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| termsContentSeq      | int                   | 약관 항목 KEY         | 
| name                 | string                | 약관 항목 이름         |
| required             | bool                  | 필수 동의 여부         |
| agreePush            | string                | 광고성 푸시 동의 여부<br/> - NONE: 동의 안 함 <br/> - ALL: 전체 동의 <br/> - DAY: 주간 푸시 동의<br/> - NIGHT: 야간 푸시 동의          |
| agreed               | bool                  | 해당 약관 항목에 대한 유저 동의 여부           |
| node1DepthPosition   | int                   | 1단계 항목 노출 순서           |
| node2DepthPosition   | int                   | 2단계 항목 노출 순서<br/> 없을 경우 -1           |
| detailPageUrl        | string                | 약관 자세히 보기 URL<br/> 없을 경우 null. |


### UpdateTerms

QueryTerms API 로 내려받은 약관 정보로 UI 를 직접 제작했다면,
게임유저가 약관에 동의한 내역을 UpdateTerms API를 통해 Gamebase 서버로 전송하시기 바랍니다.

선택 약관 동의를 취소하는 것과 같이, 약관에 동의했던 내역을 변경하는 목적으로도 활용하실 수 있습니다.


> <font color="red">[주의]</font><br/>
>
> 푸시 수신 동의 여부는 Gamebase 서버에 저장되지 않습니다.
> 푸시 수신 동의 여부는 **로그인 후에** Gamebase.Push.RegisterPush API를 호출해서 저장하세요.
> Standalone 플랫폼에서는 로그인 후, 해당 API를 호출해야 합니다. 로그인을 하지 않고 호출할 경우 NOT_LOGGED_IN 오류가 전달됩니다.
>

#### Required 파라미터
* configuration : 서버에 등록할 유저의 선택 약관 정보입니다.
 
#### Optional 파라미터

* callback : 선택 약관 정보를 서버에 등록 후 사용자에게 콜백으로 알려줍니다.


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
| NOT\_INITIALIZED | 1 | Gamebase가 초기화되어 있지 않습니다. |
| NOT\_LOGGED_IN | 2 | 로그인이 필요합니다. (Standalone에 한함) |
| UI\_TERMS\_UNREGISTERED\_SEQ | 6923 | 등록되지 않은 약관 Seq 값을 설정하였습니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924 | Terms API 호출이 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |


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
| termsVersion         | **M**                      | string                    | 약관 버전.<br/>queryTerms API를 호출해 내려받은 값을 전달해야 합니다.   |
| termsSeq             | **M**                      | int                       | 약관 전체 KEY.<br/>queryTerms API를 호출해 내려받은 값을 전달해야 합니다.             |
| contents             | **M**                      | List< Content > | 선택 약관 유저 동의 정보  |

#### GamebaseRequest.Terms.Content

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 선택 약관 항목 KEY      |
| agreed               | **M**                      | bool               | 선택 약관 항목 동의 여부  |


### IsShowingTermsView

현재 약관 창이 화면에 표시되고 있는지를 알 수 있습니다.

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

웹뷰를 표시합니다.<br/>

##### Required 파라미터
* url : 파라미터로 전송되는 url은 유효한 값이어야 합니다.

##### Optional 파라미터
* configuration : GamebaseWebViewConfiguration으로 웹뷰의 레이아웃을 변경 할 수 있습니다.
* closeCallback : 웹뷰가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList : 사용자가 받고 싶은 커스텀 스킴 목록을 지정합니다.
* schemeEvent : schemeList로 지정한 커스텀 스킴을 포함하는 url을 콜백으로 알려 줍니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE

```cs
static void ShowWebView(string url, GamebaseRequest.Webview.GamebaseWebViewConfiguration configuration = null, GamebaseCallback.ErrorDelegate closeCallback = null, List<string> schemeList = null, GamebaseCallback.GamebaseDelegate<string> schemeEvent = null)
```

> Standalone에서는 WebViewAdapter를 통해서 웹뷰를 지원하며 웹뷰가 열려 있을 때 UI로 입력되는 Event를 Blocking하지 않습니다.

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
     configuration.isBackButtonVisible = true;
    
    var schemeList = new List<string>() { "customScheme://openBrowser" };
    
    Gamebase.Webview.ShowWebView("https://www.toast.com/", configuration, CloseCallback, schemeList, SchemeEvent);
}
```

#### GamebaseWebViewConfiguration

| Parameter | Values | Description |
| ------------------------ | ---------------------------------------- | --------------------------- |
| title                    | string                                   | 웹뷰의 제목               |
| orientation              | GamebaseScreenOrientation.UNSPECIFIED    | 미지정(**default**)            |
|                          | GamebaseScreenOrientation.PORTRAIT       | 세로 모드                    |
|                          | GamebaseScreenOrientation.LANDSCAPE      | 가로 모드                    |
|                          | GamebaseScreenOrientation.LANDSCAPE_REVERSE | 가로 모드를 180도 회전     |
| contentMode<br>(iOS 전용) | GamebaseWebViewContentMode.RECOMMENDED      | 현재 플랫폼 추천 브라우저(**default**)  |
|                          | GamebaseWebViewContentMode.MOBILE           | 모바일 브라우저            |
|                          | GamebaseWebViewContentMode.DESKTOP          | 데스크톱 브라우저          |
| colorR                   | 0~255                                    | 내비게이션 바 색상 R<br>**default**: 18               |
| colorG                   | 0~255                                    | 내비게이션 바 색상 G<br>**default**: 93               |
| colorB                   | 0~255                                    | 내비게이션 바 색상 B<br>**default**: 230              |
| colorA                   | 0~255                                    | 내비게이션 바 색상 Alpha<br>**default**: 255          |
| barHeight                | height                                   | 내비게이션 바 높이<br>**Android에 한함**                 |
| isNavigationBarVisible   | true or false                            | 내비게이션 바 활성 또는 비활성<br>**default**: true    |
| isBackButtonVisible      | true or false                            | 뒤로 가기 버튼 활성 또는 비활성<br>**default**: true   |
| backButtonImageResource  | ID of resource                           | 뒤로 가기 버튼 이미지         |
| closeButtonImageResource | ID of resource                           | 닫기 버튼 이미지             |
| enableFixedFontSize<br>(Android 전용)   | true or false              | 약관 창의 글자 크기 고정 여부를 결정합니다.<br>**default**: false |
| renderOutSideSafeArea<br>(Android 전용) | true or false              | Safe Area 영역 밖 렌더링 여부를 결정합니다.<br>**default**: false |

> [TIP]
>
> iPadOS 13 이상에서 웹뷰는 기본적으로 데스크톱 모드입니다.
> contentMode =`GamebaseWebViewContentMode.MOBILE` 설정으로 모바일 모드로 변경할 수 있습니다.

#### Predefined Custom Scheme

Gamebase에서 지정해 놓은 스킴입니다.

| scheme | 용도 |
| ----------------------------- | ------------------------------ |
| gamebase://dismiss | 웹뷰 닫기 |
| gamebase://goBack | 웹뷰 뒤로 가기 |
| gamebase://getUserId          | 현재 로그인중인 있는 게임 유저의 사용자 ID를 표시 |
| gamebase://getMaintenanceInfo | 점검 내용을 WebPage에 표시 |
| gamebase://showwebview?link={URLEncodedURL} | link 파라메터의 URL 을 웹뷰로 열기.<br>URLEncodedURL : 웹뷰로 열 URL.<br>URL 디코딩 필요. |
| gamebase://openbrowser?link={URLEncodedURL} | link 파라메터의 URL을 외부 브라우저로 열기<br/>URLEncodedURL : 외부 브라우저로 열 URL<br/>URL 디코딩 필요 |

### Close WebView

다음 API를 이용하여 보여지고 있는 웹뷰를 닫을 수 있습니다.

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

다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 url은 유효한 값이어야 합니다.

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

시스템 알림을 표시할 수 있습니다.
시스템 알림에 콜백을 등록할 수도 있습니다.

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

다음 API를 사용하여 쉽게 메시지를 표시할 수 있습니다.

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
| UI\_IMAGE\_NOTICE\_TIMEOUT | 6901 | 이미지 공지 표시 중 타임아웃이 발생했습니다. |
| UI\_UNKNOWN\_ERROR | 6999       | 알수 없는 오류입니다(정의되지 않은 오류입니다). |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
