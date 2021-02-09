## Game > Gamebase > Android SDK 사용 가이드 > UI

## ImageNotice

콘솔에 이미지를 등록한 후 사용자에게 공지를 띄울 수 있습니다.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-001.png)

### Show ImageNotices

이미지 공지를 화면에 띄워 줍니다.

#### Required 파라미터
* Activity : 이미지 공지가 노출되는 Activity입니다.

#### Optional 파라미터
* ImageNoticeConfiguration : 이미지 공지 설정을 변경할 수 있습니다.
* GamebaseCallback : 이미지 공지가 전체 종료될 때 사용자에게 콜백으로 알려 줍니다.
* GamebaseDataCallback : 이미지를 클릭했을 때, 콘솔에 등록한 payload 를 콜백으로 알려 줍니다.

**API**

```java
+ (void)Gamebase.ImageNotice.showImageNotices(@NonNull Activity activity,
                                              @Nullable GamebaseCallback onCloseCallback);
+ (void)Gamebase.ImageNotice.showImageNotices(@NonNull Activity activity,
                                              @Nullable ImageNoticeConfiguration configuration,
                                              @Nullable GamebaseCallback onCloseCallback,
                                              @Nullable GamebaseDataCallback<String> onEvent);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase.initialize 가 호출되지 않았습니다. |
| UI\_IMAGE\_NOTICE\_TIMEOUT(6901) | 이미지 공지 팝업 표시중 타임아웃이 발생하여 모든 팝업을 강제 종료합니다. |

**Example**

```java
Gamebase.ImageNotice.showImageNotices(getActivity(), null,
    new GamebaseCallback(){
        @Override
        public void onCallback(GamebaseException exception) {
        	// Called when the entire imageNotice is closed.
            ...
        }
    },
    new GamebaseDataCallback<String>() {
        @Override
        public void onCallback(String payload, GamebaseException exception) {
        	// Called when custom event occurred.
            ...
        }
    });
```

### Custom ImageNotices

사용자 설정 이미지 공지를 화면에 띄워 줍니다.
ImageNoticeConfiguration으로 사용자 설정 이미지 공지를 만들 수 있습니다.

**Example**

```java
ImageNoticeConfiguration configuration = ImageNoticeConfiguration.newBuilder()
		.setBackgroundColor("#FFFF0000")		// Red
        .setTimeout(10000L)						// 10000ms == 10s
        .enableAutoCloseByCustomScheme(false)
        .build();
Gamebase.ImageNotice.showImageNotices(getActivity(), configuration, null, null);
```

#### ImageNoticeConfiguration

| API | Mandatory(M) / Optional(O) | Description |
| --- | --- | --- |
| newBuilder() | **M** | ImageNoticeConfiguration 객체는 newBuilder() 함수를 통해 생성할 수 있습니다. |
| build() | **M** | 설정을 마친 Builder 를 Configuration 객체로 변환합니다. |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | 이미지 공지 뒷 배경색.<br>String 은 android.graphics.Color.parseColor(String) API 로 변환한 값을 사용합니다.<br>**default** : #80000000 |
| setTimeout(long timeoutMs) | O | 이미지 공지 최대 로딩 시간 (단위 : millisecond)<br>**default** : 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | custom scheme 이벤트가 발생하면 이미지 공지를 강제종료 할지 여부를 결정합니다.<br>**default** : true |


### Close ImageNotices

closeImageNotices API를 호출하여 현재 표시 중인 이미지 공지를 모두 종료할 수 있습니다.

**API**

```java
+ (void)Gamebase.ImageNotice.closeImageNotices(@NonNull Activity activity);
```

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
단, Gamebase 콘솔에서 '약관 재동의' 항목을 **필요** 로 변경했다면 유저가 다시 약관에 동의할 때까지는 약관창이 표시됩니다.

> <font color="red">[주의]</font><br/>
>
> 약관에 푸시 수신 동의 여부를 추가했다면, GamebaseDataContainer 로부터 PushConfiguration 를 생성할 수 있습니다.
> PushConfiguration 이 null 이 아니라면 **로그인 후에** Gamebase.Push.registerPush API 를 호출하세요.

#### Required 파라미터

* Activity : 약관창이 노출되는 Activity 입니다.
 
#### Optional 파라미터

* GamebaseDataCallback : 약관 동의 후 약관창이 종료될 때 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseDataContainer 객체는 PushConfiguration 으로 변환해서 로그인 후 Gamebase.Push.registerPush API 에 사용할 수 있습니다.

**API**

```java
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| LAUNCHING\_SERVER\_ERROR(2001) | 런칭서버가 내려준 항목에 약관 관련 내용이 없는 경우에 발생하는 에러입니다.<br/>정상적인 상황이 아니므로 Gamebase 담당자에게 문의해주시기 바랍니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW(6925) | 약관 웹뷰가 아직 종료되지 않았는데 다시 호출되었습니다. |
| WEBVIEW\_TIMEOUT(7002) | 약관 웹뷰 표시 중 타임아웃이 발생했습니다. |
| WEBVIEW\_HTTP\_ERROR(7003) | 약관 웹뷰 오픈 중 HTTP 에러가 발생하였습니다. |

**Example**

```java
public void showTermsView(final Activity activity,
                          final GamebaseDataCallback<GamebaseDataContainer> callback) {
    Gamebase.Terms.showTermsView(activity, new GamebaseDataCallback<GamebaseDataContainer>() {
        @Override
        public void onCallback(GamebaseDataContainer container, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // Save the PushConfiguration and use it for Gamebase.Push.registerPush()
                // after Gamebase.login().
                PushConfiguration savedPushConfiguration = PushConfiguration.from(container);
            } else {
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        // Wait for a while and try again.
                        try { Thread.sleep(2000); }
                        catch (Exception ignored) {}
                        showTermsView(activity, callback);
                    }
                }).start();
            }
        }
    });
}

public void afterLogin(Activity activity) {
    // Call registerPush with saved PushConfiguration.
    Gamebase.Push.registerPush(activity, savedPushConfiguration, new GamebaseCallback() {...});
}
```

### queryTerms

Gamebase는 단순한 형태의 웹뷰로 약관을 표시합니다.
게임UI에 맞는 약관을 직접 제작하고자 하신다면, queryTerms API 를 호출하여 Gamebase 콘솔에 설정한 약관 정보를 내려받아 활용하실 수 있습니다.

로그인 후에 호출하신다면 게임유저가 약관에 동의했는지 여부도 함께 확인할 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> * GamebaseTermsContentDetail.getRequired() 가 true 인 필수 항목은 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 필수 항목을 체크하지 않으면 약관 동의를 하지 않은 상태이므로 다음으로 진행할 수 없어 항상 해당 항목은 true 로 저장되기 때문입니다.
> * 푸시 수신 동의 여부도 Gamebase 서버에 저장되지 않으므로 agreed 값은 항상 false 로 리턴됩니다.
>     * 푸시 수신 동의 여부는 Gamebase.Push.queryTokenInfo API 를 통해 조회하시기 바랍니다.
> * 콘솔에서 '기본 약관 설정' 을 하지 않는 경우, 약관 언어와 다른 국가코드로 설정된 단말기에서 queryTerms API 를 호출하면 **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생합니다.
>     * 콘솔에서 '기본 약관 설정' 을 하거나, **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** 에러가 발생했을때는 약관을 표시하지 않도록 처리하시기 바랍니다.

#### Required 파라미터

* Activity : API 호출 시점의 최상위 Activity 입니다.
* GamebaseDataCallback : API 호출 결과를 사용자에게 콜백으로 알려줍니다. 콜백으로 오는 GamebaseQueryTermsResult 로 콘솔에 설정된 약관 정보를 얻을 수 있습니다.

**API**

```java
+ (void)Gamebase.Terms.queryTerms(@NonNull Activity activity,
                                  @NonNull GamebaseDataCallback<GamebaseQueryTermsResult> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | 약관 정보가 콘솔에 등록되어 있지 않습니다. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | 단말기 국가코드에 맞는 약관 정보가 콘솔에 등록되어 있지 않습니다. |

**Example**

```java
Gamebase.Terms.queryTerms(activity, new GamebaseDataCallback<GamebaseQueryTermsResult>() {
    @Override
    public void onCallback(GamebaseQueryTermsResult result, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            final int termsSeq = result.getTermsSeq();
            final String termsVersion = result.getTermsVersion();
            final String termsCountryType = result.getTermsCountryType();
            final List<GamebaseTermsContentDetail> contents = result.getContents();
        } else if (exception.getCode() == GamebaseError.UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY) {
            // Another country device.
            // Pass the 'terms and conditions' step.
        } else {
            // Failed.
        }
    }
});
```

#### GamebaseQueryTermsResult

| API            | Values                          | Description         |
| -------------------- | --------------------------------| ------------------- |
| getTermsSeq             | int                             | 약관 전체 KEY.<br/>updateTerms API 호출 시 필요한 값입니다.          |
| getTermsVersion         | String                          | 약관 버전.<br/>updateTerms API 호출 시 필요한 값입니다.              |
| getTermsCountryType     | String                          | 약관 타입.<br/> - KOREAN : 한국 약관 <br/> - GDPR : 유럽 약관 <br/> - ETC : 기타 국가 |
| getContents             | List<GamebaseTermsContentDetail> | 약관 상세 정보 |

#### GamebaseTermsContentDetail

| API            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| getTermsContentSeq      | int                   | 약관 항목 KEY         | 
| getName                 | String                | 약관 항목 이름         |
| getRequired             | boolean                  | 필수 동의 여부         |
| getAgreePush            | String                | 광고성 푸시 동의 여부 <br/> - NONE : 동의 안함 <br/> - ALL : 전체 동의 <br/> - DAY : 주간 푸시 동의<br/> - NIGHT : 야간 푸시 동의          |
| getAgreed               | boolean                  | 해당 약관 항목에 대한 유저 동의 여부<br/> - 로그인 전에는 항상 false.<br/> - 푸시 항목은 항상 false. |
| getNode1DepthPosition   | int                   | 1단계 항목 노출 순서           |
| getNode2DepthPosition   | int                   | 2단계 항목 노출 순서 <br/> 없을 경우 -1           |
| getDetailPageUrl        | String                | 약관 자세히 보기 URL <br/> 설정되어 있지 않으면 필드 없음           |

### updateTerms

queryTerms API 로 내려받은 약관 정보로 UI 를 직접 제작했다면,
게임유저가 약관에 동의한 내역을 updateTerms API 를 통해 Gamebase 서버로 전송하시기 바랍니다.

선택 약관 동의를 취소하는 것과 같이, 약관에 동의했던 내역을 변경하는 목적으로도 활용하실 수 있습니다.

> <font color="red">[주의]</font><br/>
>
> 푸시 수신 동의 여부는 Gamebase 서버에 저장되지 않습니다.
> 푸시 수신 동의 여부는 **로그인 후에** Gamebase.Push.registerPush API 를 호출해서 저장하세요.

#### Required 파라미터

* Activity : API 호출 시점의 최상위 Activity 입니다.
* GamebaseUpdateTermsConfiguration : 서버에 등록할 유저의 선택 약관 정보입니다.

#### Optional 파라미터

* GamebaseCallback : 선택 약관 정보를 서버에 등록 후 사용자에게 콜백으로 알려줍니다.

**API**

```java
+ (void)Gamebase.Terms.updateTerms(@NonNull Activity activity,
                                   @NonNull GamebaseUpdateTermsConfiguration configuration,
                                   @Nullable GamebaseCallback callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase가 초기화되어 있지 않습니다. |
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | 등록되지 않은 약관 Seq 값을 설정하였습니다. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |

**Example**

```java
Gamebase.Terms.queryTerms(activity, new GamebaseDataCallback<GamebaseQueryTermsResult>() {
    @Override
    public void onCallback(GamebaseQueryTermsResult result, GamebaseException queryTermsException) {
        if (Gamebase.isSuccess(queryTermsException)) {
            // Succeeded to query terms.
            final int termsSeq = result.getTermsSeq();
            final String termsVersion = result.getTermsVersion();
            final List<GamebaseTermsContent> contents = new ArrayList<>();
            for (GamebaseTermsContentDetail detail : result.getContents()) {
                GamebaseTermsContent content = GamebaseTermsContent.from(detail);
                // TODO: Change agree value what you want.
                content.setAgreed(agreeOrNot);
                contents.add(content);
            }

            final GamebaseUpdateTermsConfiguration configuration =
                    GamebaseUpdateTermsConfiguration.newBuilder(termsSeq, termsVersion, contents)
                            .build();
            Gamebase.Terms.updateTerms(activity, configuration, new GamebaseCallback() {
                @Override
                public void onCallback(GamebaseException updateTermsException) {
                    if (Gamebase.isSuccess(updateTermsException)) {
                        // Succeeded to update terms.
                    } else {
                        // Failed to update terms.
                    }
                }
            });
        } else {
            // Failed to query terms.
        }
    }
});
```

#### GamebaseUpdateTermsConfiguration

**Builder**

| API                  | Description         |
| -------------------- | ------------------- |
| newBuilder(String termsVersion, int termsSeq, List<GamebaseTermsContent> contents) | Configuration 객체 생성을 위한 Builder 를 생성합니다. |
| build() | 설정을 마친 Builder 를 Configuration 객체로 변환합니다. |

| Parameter            | Mandatory(M) / Optional(O) | Type                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsSeq             | **M**                      | int                       | 약관 전체 KEY.<br/>queryTerms API를 호출해서 내려받았던 값을 전달해야 합니다.             |
| termsVersion         | **M**                      | String                    | 약관 버전.<br/>queryTerms API를 호출해서 내려받았던 값을 전달해야 합니다.   |
| contents             | **M**                      | List<GamebaseTermsContent> | 선택 약관 유저 동의 정보  |

#### GamebaseTermsContent

**Constructor**

| API                  | Description         |
| -------------------- | ------------------- |
| GamebaseTermsContent(int termsContentSeq, boolean agreed) | GamebaseTermsContent 생성자 입니다. |
| from(GamebaseTermsContentDetail) | GamebaseTermsContentDetail 객체로부터 GamebaseTermsContent 객체를 생성하는 팩토리 메서드 입니다. |
| setAgreed(boolean) | 해당 객체의 agreed 값을 변경합니다. |

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | 선택 약관 항목 KEY      |
| agreed               | **M**                      | boolean            | 선택 약관 항목 동의 여부  |

## WebView

Gamebase에서는 기본적인 WebView를 지원합니다.


### Show WebView

WebView를 표시합니다.

##### Required 파라미터
* activity : WebView가 노출되는 Activity입니다.
* url : 파라미터로 전송되는 url은 유효한 값이어야 합니다.

##### Optional 파라미터
* configuration : GamebaseWebViewConfiguration으로 WebView의 레이아웃을 변경 할 수 있습니다.
* GamebaseCallback : WebView가 종료될 때 사용자에게 콜백으로 알려 줍니다.
* schemeList : 사용자가 받고 싶은 커스텀 Scheme 목록을 지정합니다.
* GamebaseDataCallback : schemeList로 지정한 커스텀 Scheme을 포함하는 url을 콜백으로 알려 줍니다.

**API**

```java
+ (void)Gamebase.WebView.showWebView(Activity activity,
                String urlString,
                GamebaseWebViewConfiguration configuration,
                GamebaseCallback onCloseCallback,
                List<String> schemeList,
                GamebaseDataCallback<String> onEvent);
```

**Example**

```java
Gamebase.WebView.showWebView(activity, "http://www.toast.com");
```

![Webview Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

#### Custom WebView

사용자 지정 WebView를 표시합니다. <br/>
GamebaseWebViewConfiguration으로 사용자 지정 WebView를 만들 수 있습니다.

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setTitleText("title")                              // WebView 제목을 설정
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // WebView 스크린 방향 설정
            .setNavigationBarColor(Color.RED)                   // 내비게이션바 색상 설정
            .setNavigationBarHeight(40)                         // 내비게이션바 높이 설정
            .setBackButtonVisible(true)                         // 백 버튼 활성화 여부 설정
            .setBackButtonImageResource(R.id.back_button)       // 백 버튼 이미지 설정
            .setCloseButtonImageResource(R.id.close_button)     // 닫기 버튼 이미지 설정
            .build();
GamebaseWebView.showWebView(activity, "http://www.toast.com", configuration);
```

#### Custom Scheme

Gamebase WebView에서 로딩한 웹 페이지 내에 스키마(scheme)로 특정 기능을 사용하거나 웹 페이지 내용을 변경할 수 있습니다.

##### Predefined Custom Scheme

Gamebase에서 지정해 놓은 스키마입니다.

| scheme               | 용도                                  |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | WebView 닫기                          |
| gamebase://goback    | WebView 뒤로 가기                     |
| gamebase://getuserid | 현재 로그인중인 있는 사용자의 아이디 표시 |
| gamebase://showwebview?link={URLEncodedURL} | link 파라메터의 URL 을 WebView로 열기.<br>URLEncodedURL : WebView로 열 URL.<br>URL 디코딩 필요. |
| gamebase://openbrowser?link={URLEncodedURL} | link 파라메터의 URL 을 외부 브라우저로 열기.<br>URLEncodedURL : 외부 브라우저로 열 URL.<br>URL 디코딩 필요. |

#### User Custom Scheme

Gamebase에 스키마 이름과 블록을 지정해 원하는 기능을 추가할 수 있습니다.

```java
GamebaseWebViewConfiguration configuration = new GamebaseWebViewConfiguration.Builder()
        .setTitleText(title)
        .build();
List<String> schemeList = new ArrayList<>();
schemeList.add("mygame://test");
schemeList.add("mygame://opensomebrowser");
schemeList.add("closemywebview://");
showWebView(activity, urlString, configuration,
        new GamebaseCallback() {
            @Override
            public void onCallback(GamebaseException exception) {
                // When closed WebView, this callback will be called.
            }
        },
        schemeList,
        new GamebaseDataCallback<String>() {
            @Override
            public void onCallback(String fullUrl, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    if (fullUrl.contains("mygame://test")) {
                        // Do something.
                    } else if (fullUrl.contains("mygame://opensomebrowser")) {
                        Gamebase.WebView.openWebBrowser(someUrl);
                    } else if (fullUrl.contains("closemywebview://")) {
                        // We will close webview.
                        Gamebase.WebView.closeWebView(activity);
                    }
                } else {
                    // Something went wrong.
                }
            }
        });
```

#### GamebaseWebViewConfiguration

| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setTitleText(String title)               | title                               | WebView의 제목         |
| setScreenOrientation(int orientation)    | ScreenOrientation.PORTRAIT          | 세로 모드          |
|                                          | ScreenOrientation.LANDSCAPE         | 가로 모드          |
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | 가로 모드를 180도 회전 |
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | 내비게이션 바 색상     |
| setBackButtonVisible(boolean visible)    | true or false                       | 백 버튼 활성 또는 비활성 |
| setNavigationBarHeight(int height)       | height                              | 내비게이션 바 높이     |
| setBackButtonImageResource(int resourceId) | ID of resource                      | 백 버튼 이미지       |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | 닫기 버튼 이미지      |


### Close WebView
다음 API를 통하여, 보여지고 있는 WebView를 닫을 수 있습니다.

**API**

```java
+ (void)Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 URL은 유효한 값이어야 합니다.

**API**

```java
+ (void)Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

시스템 알림을 표시할 수 있습니다.<br/>

### Simple Alert Dialog

제목과 메시지만 입력하여 간단하게 알림 대화 상자를 표시할 수 있습니다.

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

알림 대화 상자를 표시한 후 처리 결과를 콜백받고 싶다면 다음 API를 사용합니다.

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity,
                            String title,
                            String messsage,
                            String okButtonText,
                            DialogInterface.OnClickListener clickListener);
```

## Toast

다음 API를 사용하여 쉽게 [Android 토스트(toast)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) 메시지를 표시할 수 있습니다.<br/>
메시지를 표시하는 시간 종류 파라미터는 int 형식이며, Android SDK NotificationManagerService 클래스의 정의에 따라 아래 표에 정리한 시간 동안 표시됩니다.

| 시간 종류(int)         | 노출 시간                     |
| ------------------ | ------------------------- |
| Toast.LENGTH_SHORT | 2초                        |
| Toast.LENGTH_LONG  | 3.5초                      |
| 0                  | Toast.LENGTH_SHORT => 2초  |
| 1                  | Toast.LENGTH_LONG => 3.5초 |
| 나머지 모든 값           | Toast.LENGTH_SHORT => 2초  |

**API**

```java
+ (void)Gamebase.Util.showToast(Activity activity,
                        String message,
                        int duration);    // 메시지를 표시하는 시간 종류 (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

## Custom Maintenance Page

점검 상태에서 '자세히 보기'를 클릭하면 표시되는 점검 페이지를 변경할 수 있습니다.

* 사용자 지정 웹 페이지를 점검 페이지로 등록
    * AndroidManifest.xml에 com.gamebase.maintenance.detail.url를 키 값으로 하는 meta-data를 설정합니다.
    * android:value 값으로 .html 파일 또는 URL을 입력할 수 있습니다.

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | Description                  |
| ------------------ | ---------- | ---------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | 알 수 없는 오류입니다(정의되지 않은 오류입니다). |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)
