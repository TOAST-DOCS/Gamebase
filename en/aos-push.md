## Game > Gamebase > Android Developer's Guide > Push

### Settings

This document describes how to set push notifications for each platform.

#### Register TOAST Cloud Console

Set console in reference of [Notification > Push > Console Guide](/Notification/Push/en/console-guide/)

#### Gradle

* Add a module for build.gradle.

```groovy
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    
    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'
    
    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-tencent:$GAMEBASE_SDK_VERSION"
}
```

* For a push module, either Firebase Cloud Messaging (FCM) or Tencent must be added. However, if a multiple number of modules are added for testing purpose, you may choose a PushType to be used for Gamebase.initialize.
    * PushProvider.Type.FCM : "FCM"
    * PushProvider.Type.TENCENT : "TENCENT"

```java
String PUSH_TYPE = PushProvider.Type.FCM;	// Firebase Cloud Messaging

GamebaseConfiguration configuration = GamebaseConfiguration.newBuilder(APP_ID, APP_VERSION, STORE_CODE)
		.setPushType(PUSH_TYPE)
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### Firebase

* For Gradle Builds
    * To enable Firebase push, follow the guide to complete the Firebase setting.
    	* [TOAST > TOAST SDK User Guide > TOAST Push > Android > Set Firebase Cloud Messaging](/TOAST/en/toast-sdk/push-android/#firebase-cloud-messaging)
* For a Unity build
    * You must create a string resource (xml) file by yourself and place it in the Assets/Plugins/Android/res/values/ folder.
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * Below is an example of a string resource file.
            ```
            <!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
            <?xml version="1.0" encoding="utf-8"?>
            <resources>
            <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
            <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
            <string name="firebase_database_url" translatable="false">https://tap-development-00000000.firebaseio.com</string>
            <string name="google_app_id" translatable="false">1:000000000000:android:749cbe01c8ada279</string>
            <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
            <string name="google_storage_bucket" translatable="false">tap-development-00000000.appspot.com</string>
            </resources>
            ```
        * You can view each of values set in the string resource (xml) file by clicking **Firebase Console > Project Setting** and download the **google-services.json** file.
            Depending on the Firebase service link, the contents of the google-services.json file may vary.
            ![Download google-services.json](http://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)

#### Tencent

* To enable Tencent push, follow the guide and complete the Tencent setting. 
	* [TOAST > TOAST SDK User Guide > TOAST Push > Android > Set Tencent Push Notification](/TOAST/en/toast-sdk/push-android/#tencent-push-notification)

### Register Push

By calling API as below, a user can be registered to TOAST Cloud Push.<br/>
With user's agreement to enablePush, enableAdPush, and enableAdNightPush, call following API to complete registration.

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, PushConfiguration configuration, GamebaseCallback callback);
```

**Example**

```java
boolean enablePush;
boolean enableAdPush;
boolean enableAdNightPush;

PushConfiguration configuration = new PushConfiguration(enablePush, enableAdPush, enableAdNightPush);

Gamebase.Push.registerPush(activity, configuration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
        } else {
            // Failed.
            Log.e(TAG, "Register push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Request Push Settings

To retrieve user's push setting, apply API as below. <br/>
From PushConfiguration callback values, you can get user's value set.

**API**

```java
+ (void)Gamebase.Push.registerPush(Activity activity, GamebaseDataCallback<PushConfiguration> callback);
```

**Example**

```java
Gamebase.Push.queryPush(activity, new GamebaseDataCallback<PushConfiguration>() {
    @Override
    public void onCallback(PushConfiguration data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            // Succeeded.
            boolean enablePush = data.pushEnabled;
            boolean enableAdPush = data.adAgreement;
            boolean enableAdNightPush = data.adAgreementNight;
        } else {
            // Failed.
            Log.e(TAG, "Query push failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
        }
    }
});
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error in TOAST Cloud Push library.<br>Please check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous push API call has not been completed.<br>Please call again after the previous push API callback is executed. |
| PUSH_UNKNOWN_ERROR             | 5999       | Unknown push error.<br>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

* Refer to the following document for the entire error codes:
    * [Entire Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the TOAST Cloud Push library.
* Check the error code as below.

```java
Gamebase.Push.registerPush(activity, pushConfiguration, new GamebaseCallback() {
    @Override
    public void onCallback(GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Register push successful");
            ...
        } else {
            Log.e(TAG, "Register push failed");

            // Gamebase Error Info
            int errorCode = exception.getCode();
            String errorMessage = exception.getMessage();
            
            if (errorCode == GamebaseError.PUSH_EXTERNAL_LIBRARY_ERROR) {
                // TOAST Push Error Info
                int moduleErrorCode = exception.getDetailCode();
                String moduleErrorMessage = exception.getDetailMessage();
                
                ...
            }
        }
    }
});
```

* The TOAST Push SDK error codes are as follows:

| Error | Error Code | Description |
| --- | --- | --- |
| OK | 0 | API call is successful |
| NOT_INITIALIZE | 100 | TOAST SDK or TOAST Push SDK is not initialized |
| PROVIDER_SDK_ERROR | 101 | Error occurs from external SDK (Firebase or Tencent) |
| USER_ID_NOT_REGISTERED | 102 | Not logged in |
| UNSUPPORTED_PUSH_TYPE | 103 | PushType is invalid or push library is not included to a project |
| API_SERVER_ERROR | 104 | TOAST Push server API call fails |
| TOKEN_NOT_REGISTERED | 105 | Internally-cached push token does not exist |
