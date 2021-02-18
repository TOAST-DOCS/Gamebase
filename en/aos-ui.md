## Game > Gamebase > Android Developer's Guide > UI

## WebView

Gamebase supports a default WebView.


### Show WebView

Shows a WebView.<br/>

##### Required Parameters
* activity : Shows WebView.
* url : The url delivered as a parameter should be valid.

##### Optional Parameters
* configuration: Changes WebView layout by using GamebaseWebViewConfiguration.
* GamebaseCallback: Notifies users when a WebView is closed.
* schemeList: Specifies the list of customized schemes a user wants.
* gamebaseDataCallback: Notifies url including customized scheme specified by the schemeList with a callback.

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

Shows a customized WebView. <br/>
Can configure a customzed WebView by using GamebaseWebViewConfiguration.

```java
GamebaseWebViewConfiguration configuration
        = new GamebaseWebViewConfiguration.Builder()
            .setTitleText("title")                              // Set Title
            .setScreenOrientation(ScreenOrientation.PORTRAIT)   // Set Screen Orientation
            .setNavigationBarColor(Color.RED)                   // Set Navigation Bar Color
            .setNavigationBarHeight(40)                         // Set Navigation Bar Height
            .setBackButtonVisible(true)                         // Set Back Button Visible
            .setBackButtonImageResource(R.id.back_button)       // Set Back Button Image
            .setCloseButtonImageResource(R.id.close_button)     // Set Close Button Image
            .build();
GamebaseWebView.showWebView(activity, "http://www.toast.com", configuration);
```

#### Custom Schema

With schema on the webpage loaded by Gamebase WebView, specific features become available or webpage can be changed. 

##### Predefined Custom Schema

Gamebase has the following schemas 

| Schema     | Usage                            |
| -------------------- | ------------------------------------- |
| gamebase://dismiss   | Close WebView                    |
| gamebase://goBack    | Go back to previous page on WebView |
| gamebase://getUserId | Show ID of current logged-in user |

#### User Custom Schema

By specifying the name and block of a schema for Gamebase, features may be added in need.   

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
| setTitleText(String title)               | title                               | Title of WebView         |
| setScreenOrientation(int orientation)    | ScreenOrientation.PORTRAIT          | Portrait Mode          |
|                                          | ScreenOrientation.LANDSCAPE         | Landscape Mode          |
|                                          | ScreenOrientation.LANDSCAPE_REVERSE | Reverse Landscape |
| setNavigationBarColor(int color)         | Color.argb(a, r, b, b)              | Color of Navigation Bar     |
| setBackButtonVisible(boolean visible)    | true or false                       | Activate/Deactivate Go Back Button |
| setNavigationBarHeight(int height)       | height                              | Height of Navigation Bar     |
| setBackButtonImageResource(int resourceId) | ID of resource                      | Image of Go Back Button       |
| setCloseButtonImageResource(int resourceId) | ID of resource                      | Image of Close Button      |


### Close WebView
Close currently displayed WebView by using the following API.

**API**

```java
+ (void)Gamebase.WebView.closeWebView(Activity activity);
```


## Open External Browser

Open an external browser by using the following API. The URL delivered as a parameter should be valid.

**API**

```java
+ (void)Gamebase.WebView.openWebBrowser(Activity activity, String urlString);
```


## Alert

Displays a system alert API.<br/>

### Simple Alert Dialog

Shows a simple alert dialogue by entering title and message only.

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity, String title, String message);
```

![Alert Dialog Example](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


### Alert Dialog with Listener

Use the following API to receive callbacks on processing results after an alert dialog is displayed.

**API**

```java
+ (void)Gamebase.Util.showAlertDialog(Activity activity,
                            String title,                                   // Title text.
                            String messsage,                                // Message text.
                            String okButtonText,                            // Positive button text.
                            DialogInterface.OnClickListener clickListener); // Listener called when pressing a positive button.
```

## NHN Cloud

Displays [Android NHN Cloud](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API.<br/>
The type of time parameter to display message is provided in int format and will be displayed during time as below, as the Android SDK NotificationManagerService class is defined.

| Type of Time (int)         | Display Time                     |
| ------------------ | ------------------------- |
| Toast.LENGTH_SHORT | 2 seconds                        |
| Toast.LENGTH_LONG  | 3.5 seconds                      |
| 0                  | Toast.LENGTH_SHORT => 2 seconds  |
| 1                  | Toast.LENGTH_LONG => 3.5 seconds |
| All the rest values           | Toast.LENGTH_SHORT => 2 seconds  |

**API**

```java
+ (void)Gamebase.Util.showToast(Activity activity,
                        String message,     // Message text to display
                        int duration);      // Type of time to display message (Toast.LENGTH_SHORT or Toast.LENGTH_LONG)
```

## Custom Maintenance Page

Click 'Detail' in maintenance status to change maintenance page.

* Register a customized web page as a maintenance page
    * Set meta-data with "com.gamebase.maintenance.detail.url" as key value to AndroidManifest.xml.
    * Enter .html file or URL with android:value.

```xml
<meta-data
	android:name="com.gamebase.maintenance.detail.url"
	android:value="file:///android_asset/html/gamebase-maintenance.html"/>
```

## Error Handling

| Error              | Error Code | Description                  |
| ------------------ | ---------- | ---------------------------- |
| UI\_UNKNOWN\_ERROR | 6999       | Unknown error (Undefined error). |

* Refer to the following document for the entire error codes:
    * [Entire Error Codes](./error-codes#client-sdk)
