## Game > Gamebase > Android Developer's Guide > UI

## ImageNotice

You can pop up a notice to users after registering an image to the console.

![ImageNotice Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/imageNotice-guide-002.png)

### Show ImageNotices

Show the image notice on the screen.

#### Required parameter
* Activity: An activity where the image notice is exposed.

#### Optional parameter
* ImageNoticeConfiguration: Can change the image notice settings.
* GamebaseCallback: Informs the user with callback when the entire image notice is terminated.
* GamebaseDataCallback: Informs the payload which is registered in the console as callback.

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
| NOT\_INITIALIZED(1) | Gamebase.initialize has not been called. |
| UI\_IMAGE\_NOTICE\_TIMEOUT(6901) | Performs a force shutdown of all popups because timeout has occurred while displaying the image notice popup. |

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

Pops up a customized image notice on the screen.
You can use ImageNoticeConfiguration to create a customized image notice.

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
| newBuilder() | **M** | ImageNoticeConfiguration object can be created using the newBuilder() function. |
| build() | **M** | Converts the configured builder into a Configuration object. |
| setBackgroundColor(int backgroundColor)<br>setBackgroundColor(String backgroundColor) | O | Image notice background color.<br>This string uses a value converted into android.graphics.Color.parseColor(String) API.<br>**default**: #80000000 |
| setTimeout(long timeoutMs) | O | Max time to load an image notice (in millisec)<br>**default**: 5000L (5s) |
| enableAutoCloseByCustomScheme(boolean enable) | O | Determine whether to force shutdown the image notice when a custom scheme event occurs.<br>**default**: true |


### Close ImageNotices

You can call the closeImageNotices API to terminate all image notices currently being displayed.

**API**

```java
+ (void)Gamebase.ImageNotice.closeImageNotices(@NonNull Activity activity);
```

## Terms

Shows the Terms and Conditions specified in the Gamebase Console.

![TermsView Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/termsView-guide-ui-001_2.20.0.png)

showTermsView API displays the terms and conditions window in WebView.
If you want to create your own terms and conditions window appropriate for the Game UI, call the queryTerms API to load the terms and conditions set in the Gamebase console.
If users agree to the terms and conditions, please use the updateTerms API to send the user consent of each item to the Gamebase server.

### showTermsView

Shows the terms and conditions window on the screen.
If users agree to the terms and conditions, register the user consent data in the server.
If users agree to the terms and conditions, calling the showTermsView API again will immediately return the success callback without displaying the terms and conditions window.
However, if the "Agree again to Terms and Conditions" item has been switched to **Required**, the terms and conditions window is displayed until users agree again to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * If you have added user consent for receiving push notification in the terms and conditions, you can create a PushConfiguration from GamebaseDataContainer.
* PushConfiguration is null if the terms and conditions window is not displayed. (If the terms and conditions window is displayed, a valid object is always returned.)
* PushConfiguration.pushEnabled value is always true.
* If PushConfiguration is not null, call Gamebase.Push.registerPush API **after login**.

#### Required parameter

* Activity: An activity that prompts the terms and conditions window.
 
#### Optional parameter

* GamebaseDataCallback: Uses a callback to inform the user when the terms and conditions window closes after agreeing to it. The GamebaseDataContainer object which comes as a callback can be converted to PushConfiguration. The converted object can be used in the Gamebase.Push.registerPush API after login.

**API**

```java
+ (void)Gamebase.Terms.showTermsView(@NonNull Activity activity,
                                     @Nullable GamebaseDataCallback<GamebaseDataContainer> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase not initialized. |
| LAUNCHING\_SERVER\_ERROR(2001) | This error occurs when the items downloaded from the launching server does not have any information about the terms and conditions.<br/>This is not a usual case, and you should contact the Gamebase personnel. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | The Terms API called previously has not been completed yet.<br/>Please try again later. |
| UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW(6925) | Unfinished terms & conditions WebView has been called again. |
| WEBVIEW\_TIMEOUT(7002) | Timed out while displaying the terms and conditions WebView. |
| WEBVIEW\_HTTP\_ERROR(7003) | An HTTP error has occurred while opening the terms and conditions WebView. |

**Example**

```java
public void showTermsView(final Activity activity,
                          final GamebaseDataCallback<GamebaseDataContainer> callback) {
    Gamebase.Terms.showTermsView(activity, new GamebaseDataCallback<GamebaseDataContainer>() {
        @Override
        public void onCallback(GamebaseDataContainer container, GamebaseException exception) {
            if (Gamebase.isSuccess(exception)) {
                // If the 'PushConfiguration' is not null,
                // save the 'PushConfiguration' and use it for Gamebase.Push.registerPush()
                // after Gamebase.login().
                @Nullable PushConfiguration savedPushConfiguration = PushConfiguration.from(container);
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
    if (savedPushConfiguration != null) {
        Gamebase.Push.registerPush(activity, savedPushConfiguration, new GamebaseCallback() {...});
    }
}
```

### queryTerms

Gamebase displays the terms and conditions with a simple WebView.
If you want to create the terms and conditions appropriate for the game UI, call the queryTerms API to download the terms and conditions information set in the Gamebase Console for later use.

Calling it after login also lets you see if the game user has agreed to the terms and conditions.

> <font color="red">[Caution]</font><br/>
>
> * The required items with GamebaseTermsContentDetail.getRequired() set to true are not stored in the Gamebase server; therefore, false is always returned for the agreed value.
>     * It is because there is no point in storing the required items since they are always stored as true.
> * The user consent for receiving the push notification is not stored in the Gamebase server either; therefore, the agreed value is always returned as false.
>     * To see if the user has agreed to receive push, please use the Gamebase.Push.queryTokenInfo API.
> * If you do not touch the 'Terms and Conditions settings' in the console, **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs when you call the queryTerms API from the device with the country code different from the terms and conditions language.
>     * If you complete the 'Terms and Conditions settings' in the console or if **UI_TERMS_NOT_EXIST_FOR_DEVICE_COUNTRY(6922)** error occurs, please make sure the terms and conditions are not displayed.

#### Required parameter

* Activity: A top level activity at the time of API call.
* GamebaseDataCallback: Uses a callback to inform users about the API call result. With the GamebaseQueryTermsResult that comes as callback, you can acquire the terms and conditions information set in the console.

**API**

```java
+ (void)Gamebase.Terms.queryTerms(@NonNull Activity activity,
                                  @NonNull GamebaseDataCallback<GamebaseQueryTermsResult> callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase not initialized. |
| UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE(6921) | Terms & conditions information is not registered with the console. |
| UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY(6922) | Terms & conditions appropriate for the device's country code is not registered with the console. |

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
| getTermsSeq             | int                             | KEY for the entire terms and conditions.<br/>This value is required when calling updateTerms API.          |
| getTermsVersion         | String                          | T&C version.<br/>This value is required when calling updateTerms API.              |
| getTermsCountryType     | String                          | Terms and conditions type.<br/> - KOREAN: Korean T&C <br/> - GDPR: European T&C <br/> - ETC: Other countries |
| getContents             | List<GamebaseTermsContentDetail> | Details of each T&C |

#### GamebaseTermsContentDetail

| API            | Values                | Description         |
| -------------------- | ----------------------| ------------------- |
| getTermsContentSeq      | int                   | T&C KEY         | 
| getName                 | String                | T&C Name         |
| getRequired             | boolean                  | Whether agreement is required         |
| getAgreePush            | String                | Whether to accept advertisement push.<br/> - NONE: Do not accept <br/> - ALL: Accept all <br/> - DAY: Accept push notification during daytime<br/> - NIGHT: Accept push notification during night time          |
| getAgreed               | boolean                  | Whether users agree to the T&C.<br/> - Always false before login.<br/> - Always false for push items. |
| getNode1DepthPosition   | int                   | Primary item exposure sequence.           |
| getNode2DepthPosition   | int                   | Secondary item exposure sequence.<br/> If none, -1           |
| getDetailPageUrl        | String                | URL for the full terms and conditions.<br/> If none, null. |

### updateTerms

If the UI has been created manually with the terms and conditions info downloaded from the queryTerms API,
please use the updateTerms API to send the game user's agreement history to the Gamebase server.

It can be used to terminate the agreement to optional terms and conditions as well as to revise the agreed T&C clauses.

> <font color="red">[Caution]</font><br/>
>
> Push accept status is not stored in the Gamebase server.
> Push accept status should be stored by calling the Gamebase.Push.registerPush API **after login**.

#### Required parameter

* Activity: A top level activity at the time of API call.
* GamebaseUpdateTermsConfiguration: Information of optional T&C of users who will be registered on the server.

#### Optional parameter

* GamebaseCallback: Registers the information of optional terms and conditions on the server, and notifies user with a callback.

**API**

```java
+ (void)Gamebase.Terms.updateTerms(@NonNull Activity activity,
                                   @NonNull GamebaseUpdateTermsConfiguration configuration,
                                   @Nullable GamebaseCallback callback);
```

**ErrorCode**

| Error Code | Description |
| --- | --- |
| NOT\_INITIALIZED(1) | Gamebase not initialized. |
| UI\_TERMS\_UNREGISTERED\_SEQ(6923) | Unregistered terms and conditions Seq value has been set. |
| UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR(6924) | The Terms API called previously has not been completed yet.<br/>Please try again later. |

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
| newBuilder(String termsVersion, int termsSeq, List<GamebaseTermsContent> contents) | Creates a Builder to create Configuration objects. |
| build() | Converts the configured builder into a Configuration object. |

| Parameter            | Mandatory(M) / Optional(O) | Type                    | Description         |
| -------------------- | -------------------------- | ------------------------- | ------------------- |
| termsSeq             | **M**                      | int                       | KEY for the entire terms and conditions.<br/>The queryTerms API must be called to pass the downloaded value.             |
| termsVersion         | **M**                      | String                    | T&C version.<br/>The queryTerms API must be called to pass the downloaded value.   |
| contents             | **M**                      | List<GamebaseTermsContent> | Info on whether user agrees to the optional terms and conditions  |

#### GamebaseTermsContent

**Constructor**

| API                  | Description         |
| -------------------- | ------------------- |
| GamebaseTermsContent(int termsContentSeq, boolean agreed) | GamebaseTermsContent creator. |
| from(GamebaseTermsContentDetail) | Factory method which creates the GamebaseTermsContent object from the GamebaseTermsContentDetail object. |
| setAgreed(boolean) | Changes the agreed value of the object. |

| Parameter            | Mandatory(M) / Optional(O) | Values             | Description         |
| -------------------- | -------------------------- | ------------------ | ------------------- |
| termsContentSeq      | **M**                      | int                | KEY for optional terms and conditions      |
| agreed               | **M**                      | boolean            | Info on whether user agrees to optional terms and conditions  |

## WebView

Gamebase supports a default WebView.


### Show WebView

Shows a WebView.

##### Required Parameters
* activity: Shows WebView.
* url: The url delivered as a parameter should be valid.

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

![Webview Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-001_1.0.0.png)

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
| gamebase://showwebview?link={URLEncodedURL} | Open the URL of the link parameter with the WebView.<br>URLEncodedURL: Column URL with the WebView.<br>Requires URL decoding. |
| gamebase://openbrowser?link={URLEncodedURL} | Open the URL of the link parameter with an external browser.<br>URLEncodedURL: Column URL with the WebView.<br>Requires URL decoding. |

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

![Alert Dialog Example](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-ui-002_1.0.0.png)


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

## Toast

Displays [Android Toast](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) messages, by using the following API.<br/>
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
