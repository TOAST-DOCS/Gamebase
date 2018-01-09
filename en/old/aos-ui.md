## Game > Gamebase > Android Developer's Guide > UI

## WebView

### Browser Style WebView

Displays a default browser-style WebView.
```java
Gamebase.WebView.showWebBrowser(activity, "http://cloud.toast.com");
```

![Webview Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)


### Popup Style WebView (To be Supported)

Displays a default pop-up-style WebView. 
```java
Gamebase.WebView.showWebPopup(activity, "http://cloud.toast.com");
```

### Custom WebView

Displays customized WebView. <br/>
Customzed WebView can be created by using GamebaseWebViewConfiguration. 
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
GamebaseWebView.showWebView(MainActivity.this, "http://cloud.toast.com", configuration);
```
| Method                                   | Values                              | Description                        |
| ---------------------------------------- | ----------------------------------- | ---------------------------------- |
| setStyle(int style)                      | GamebaseWebViewStyle.BROWSER        | Browser-style WebView              |
|                                          | GamebaseWebViewStyle.POPUP          | Pop-up-style WebView               |
| setTitleText(String title)               | title                               | Title of WebView                   |
| setScreenOrientation(int orientation)    | ScreenOrientation.PORTRAIT          | Vertical Mode                      |
|                                          | ScreenOrientation.LANDSCAPE         | Horizontal Mode                    |
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | Turn Left to Right                 |
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | Color of Navigation Bar            |
| setBackButtonVisible(boolean visible)    | true or false                       | Activate/Deactivate Go Back Button |
| setNavigationBarHeight(int height)       | height                              | Height of Navigation Bar           |
| setBackButtonImageResource(int resourceId) | ID of resource                      | Image of Go Back Button            |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | Image of Close Button              |

## Alert

Displays systems alerts.<br/>

### Simple Alert Dialog

Shows alert dialogue by entering title and simple message. 

```java
Gamebase.Util.showAlertDialog(activity, "title", "message");
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

Use the following API to receive callbacks on processing results after alert dialog is displayed.

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

Displays [Android Toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API. <br/>
The type of time parameter to display message is provided in int format, which will be displayed during the time as below, as the  Android SDK NotificationManagerService class is defined. 

| Type of Time (int)  | Exposure Time                    |
| ------------------- | -------------------------------- |
| Toast.LENGTH_SHORT  | 2 seconds                        |
| Toast.LENGTH_LONG   | 3.5 seconds                      |
| 0                   | Toast.LENGTH_SHORT => 2 seconds  |
| 1                   | Toast.LENGTH_LONG => 3.5 seconds |
| All the rest values | Toast.LENGTH_SHORT => 2 seconds  |

```java
Gamebase.Util.showToast(activity,
                        "message",              // Message text to expose
                        Toast.LENGTH_SHORT);    // Type of time to display message (Toast.LENGTH_SHORT or Toast.LENGTH_LONG) Type of time to display message 
```

## Custom Maintenance Page

Click 'Detail' in maintenance status to change maintenance page.

* Register customized web page as a maintenance page
  * Set meta-data with com.gamebase.maintenance.detail.url as key value to AndroidManifest.xml.
  * Enter .html file or URL with android:value.

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | Description                      |
| ------------------ | ---------- | -------------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes:
  - [Entire Error Codes](./error-codes#client-sdk)