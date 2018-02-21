## Game > Gamebase > Android Developer's Guide > UI

## WebView

Gamebase supports a default WebView.


### Show WebView

Shows a WebView.

##### Required Parameters

- activity: Shows WebView.
- url: The url delivered as a parameter should be valid.

##### Optional Parameters
- configuration: Changes WebView layout by using GamebaseWebViewConfiguration.
- GamebaseCallback: Notifies users when a WebView is closed.
- schemeList: Specifies the list of customized schemes a user wants.
- gamebaseDataCallback: Notifies url including customized scheme specified by the schemeList with a callback.


```java
Gamebase.WebView.showWebView(activity, "http://www.toast.com",
    new GamebaseWebViewConfiguration.Builder().build(),
    new GamebaseCallback() {
        @Override
        public void onCallback(GamebaseException exception) {
            Logger.d(TAG, "WebView is closed.");
        }
    }, schmeList,
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

Shows a customized WebView.<br/>
Can configure a customzed WebView by using GamebaseWebViewConfiguration.

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setStyle(GamebaseWebViewStyle.BROWSER)
            .setTitleText("title")                              // Set Title
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // Set Screen Orientation
            .setNavigationBarColor(Color.RED)                   // Set Navigation Bar Color
            .setNavigationBarHeight(40)                         // Set Navigation Bar Height
            .setBackButtonVisible(true)                         // Set Back Button Visible
            .setBackButtonImageResource(R.id.back_button)       // Set Back Button Image
            .setCloseButtonImageResource(R.id.close_button)     // Set Close Button Image
            .build();
GamebaseWebView.showWebView(MainActivity.this, "http://www.toast.com", configuration);
```

| Method                                   | Values                              | Description    |
| ---------------------------------------- | ----------------------------------- | -------------- |
| setStyle(int style)                          | GamebaseWebViewStyle.BROWSER | Browser-style WebView |
|                                              | GamebaseWebViewStyle.POPUP   | Pop-up-style WebView  |
| setTitleText(String title)                   | title                        | Title of WebView      |
| setScreenOrientation(int orientation)        | ScreenOrientation.PORTRAIT   | Portrait Mode         |
|                                              | ScreenOrientation.LANDSCAPE  | Landscape Mode        |
|                                              | ScreenOrientation.LANDSCAPE_REVERSE | Reverse Landscape |
| setNavigationBarColor(int color)             | Color.argb(a, r, b, b)       | Color of Navigation Bar  |
| setBackButtonVisible(boolean visible)        | true or false                | Activate/Deactivate Go Back Button |
| setNavigationBarHeight(int height)           | height                       | Height of Navigation Bar |
| setBackButtonImageResource(int resourceId)   | ID of resource               | Image of Go Back Button  |
| setCloseButtonImageResource(int resourceId)  | ID of resource               | Image of Close Button    |




### Close WebView
Close currently displayed WebView by using the following API.

```java
Gamebase.WebView.closeWebView(activity);
```


## Open External Browser

Open an external browser by using the following API. The URL delivered as a parameter should be valid.

```java
Gamebase.WebView.openWebBrowser(activity, "http://www.toast.com");
```


## Alert

Displays a system alert API.

### Simple Alert Dialog

Shows a simple alert dialogue by entering title and message only.

```java
Gamebase.Util.showAlertDialog(activity, "title", "message");
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

Use the following API to receive callbacks on processing results after an alert dialog is displayed.

```java
Gamebase.Util.showAlertDialog(activity,
                            "title",                        // Title text.
                            "messsage",                     // Message text.
                            "OK",                           // Positive button text
                            positiveButtonEventListener,    // Listener called when pressing a positive button
                            "Cancel",                       // Negative button text.
                            negativeButtonEventListener,    // Listener called when pressing a negative button.
                            backKeyEventListener,           // Listener called when an alert dialog is cancelled.
                            true);                          // Set whether alert dialog can be cancelled.
```

## Toast

Displays [Android Toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API.<br/>
The type of time parameter to display message is provided in int format and will be displayed during time as below, as the Android SDK NotificationManagerService class is defined.


|  Type of Time (int) |  Display Time  |
| ------------------- | -------------- |
| Toast.LENGTH_SHORT  | 2 seconds      |
| Toast.LENGTH_LONG   | 3.5 seconds    |
| 0                   | Toast.LENGTH_SHORT => 2 seconds  |
| 1                   | Toast.LENGTH_LONG => 3.5 seconds |
| All the rest values | Toast.LENGTH_SHORT => 2 seconds  |



```java
Gamebase.Util.showToast(activity,
                        "message",              // Message text to display
                        Toast.LENGTH_SHORT);    // Type of time to display message (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

## Custom Maintenance Page

Click 'Detail' in maintenance status to change maintenance page.

- Register a customized web page as a maintenance page
    - Set meta-data with "com.gamebase.maintenance.detail.url" as key value to AndroidManifest.xml.
    - Enter .html file or URL with android:value.


```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | Description                  |
| ------------------ | ---------- | ---------------------------- |
| UI_UNKNOWN_ERROR   | 6999       | Unknown error (Undefined error). |

- Refer to the following document for the entire error codes:
    - [Entire Error Codes](./error-codes#client-sdk)
