## Game > Gamebase > Android SDK 사용 가이드 > UI

## WebView

Gamebase에서는 기본적인 WebView를 지원합니다.


### Show WebView

WebView를 표시합니다.<br/>

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
Gamebase.WebView.showWebView(activity, "http://www.toast.com",
    new GamebaseWebViewConfiguration.Builder().build(),
    new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            Logger.d(TAG, "WebView is closed.");
        }
    }, schemeList,
    new GamebaseDataCallback<String>() {
        @Override
        public void onCallback(String fullUrl, GamebaseException exception) {
            Logger.d(TAG, "WebView Event occured. Event Url :" + fullUrl);
        }
    }
 );
```

![Webview Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)


#### Custom WebView

사용자 지정 WebView를 표시합니다. <br/>
GamebaseWebViewConfiguration으로 사용자 지정 WebView를 만들 수 있습니다.

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setStyle(GamebaseWebViewStyle.BROWSER)
            .setTitleText("title")                              // WebView 제목을 설정
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // WebView 스크린 방향 설정
            .setNavigationBarColor(Color.RED)                   // 내비게이션바 색상 설정
            .setNavigationBarHeight(40)                         // 내비게이션바 높이 설정
            .setBackButtonVisible(true)                         // 백 버튼 활성화 여부 설정
            .setBackButtonImageResource(R.id.back_button)       // 백 버튼 이미지 설정
            .setCloseButtonImageResource(R.id.close_button)     // 닫기 버튼 이미지 설정
            .build();
GamebaseWebView.showWebView(MainActivity.this, "http://www.toast.com", configuration);
```
| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setStyle(int style)                      | GamebaseWebViewStyle.BROWSER        | 브라우저 스타일의 WebView   |
|                                          | GamebaseWebViewStyle.POPUP          | 팝업 스타일의 WebView     |
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
Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

다음 API를 통하여 외부 브라우져를 열 수 있습니다. 파라미터로 전송되는 URL은 유효한 값이어야 합니다.

**API**

```java
Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

시스템 알림을 표시할 수 있습니다.<br/>

### Simple Alert Dialog

제목과 메시지만 입력하여 간단하게 알림 대화 상자를 표시할 수 있습니다.

**API**

```java
Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

알림 대화 상자를 표시한 후 처리 결과를 콜백받고 싶다면 다음 API를 사용합니다.

**API**

```java
Gamebase.Util.showAlertDialog(Activity activity,
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
Gamebase.Util.showToast(Activity activity,
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
