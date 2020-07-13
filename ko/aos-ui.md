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

![Webview Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

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
| gamebase://goBack    | WebView 뒤로 가기                     |
| gamebase://getUserId | 현재 로그인돼 있는 사용자의 아이디 표시 |

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

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


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
