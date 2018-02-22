## Game > Gamebase > Android Developer's Guide > Push

## Push

### Settings

This document describes how to set push notifications for each platform.

#### TOAST Cloud Console 등록

To set your Console, refer to [TOAST Cloud Push Guide](/Notification/Push/en/Getting%20Started/#console_1).

#### Download

- For Firebase Push
    - Add the **gamebase-adapter-push-fcm** folder of downloaded SDK to your project.
- For Tencent Push
    - Add the **gamebase-adapter-push-tencent** folder of downloaded SDK to your project.


> <font color="red">[Note]</font><br/>
>
> Requires only one push module.<br/>
> Do not add both Firebase and Tencent pushes in one project.


#### AndroidManifest.xml

- Add required setting for Gamebase Push.

> <font color="red">[Note]</font><br/>
>
> Must set a **package name** for **${applicationId}.**
>

*Firebase*

```xml
<manifest>
    ...
    <permission
        android:name="${applicationId}.permission.C2D_MESSAGE"
        android:protectionLevel="signature" />
    <uses-permission android:name="${applicationId}.permission.C2D_MESSAGE" />
    ...
    <application>
    ...
        <provider
            android:name="com.google.firebase.provider.FirebaseInitProvider"
            android:authorities="${applicationId}.firebaseinitprovider"
            android:exported="false"
            android:initOrder="100" />
        <receiver
            android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
            android:exported="true"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />

                <category android:name="${applicationId}" />
            </intent-filter>
        </receiver>
    ...
    </application>
</manifest>
```

*Tencent*

```xml
<manifest>
    ...
    <application>
    ...
        <provider
            android:name="com.tencent.android.tpush.XGPushProvider"
            android:authorities="${applicationId}.AUTH_XGPUSH"
            android:exported="true" />
        <provider
            android:name="com.tencent.android.tpush.SettingsContentProvider"
            android:authorities="${applicationId}.TPUSH_PROVIDER"
            android:exported="false" />
        <provider
            android:name="com.tencent.mid.api.MidProvider"
            android:authorities="${applicationId}.TENCENT.MID.V3"
            android:exported="true" />
    ...
    </application>
</manifest>
```

#### Google Services Settings (Firebase only)


- For Gradle Builds
    - To use Firebase push, google-services.json file is required to setup. Refer to [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/#add_firebase_to_your_app) on how to add the configuration file to your project.
    - Add **apply plugin: ';com.google.gms.google-services'** to the Gradle setting.
    - With the setting above, Google Services Gradle Plugin will be applied and the google-services.json file will be changed to a string resource named res/google-services/{build_type}/values/values.xml.
- For Unity Builds
    - Cannot use Google Services Gradle Plugin. To add a string resource xml file of your own creation to the project, refer to [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file).
        - Below is an example of a string resource file.

```xml
<!-- res/values/google-services-json.xml -->
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

#### Initialization

- To initialize Gamebase, call **setPushType()** of configuration.
- For Firebase Push
    - Make an additional call to **setFCMSenderId()**.
- For Tencent Push
    - Make an additional call to **setTencentAccessId()**.
    - Make an additional call to **setTencentAccessKey()**.

```java
private static final String PUSH_FCM_SENDER_ID = "...";
private static final String PUSH_TENCENT_ACCESS_ID = "...";
private static final String PUSH_TENCENT_ACCESS_KEY = "...";

GamebaseConfiguration configuration = new GamebaseConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
        .setFCMSenderId(PUSH_FCM_SENDER_ID)				// Firebase requires SenderId.
        .setTencentAccessId(PUSH_TENCENT_ACCESS_ID)		// Requires Tencent AccessId.
        .setTencentAccessKey(PUSH_TENCENT_ACCESS_KEY)   // Requires Tencent AccessKey.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

### Register Push

By calling API as below, a user can be registered to TOAST Cloud Push.<br/>
With user's agreement to enablePush, enableAdPush, and enableAdNightPush, call following API to complete registration.

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
| PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | Error in TOAST Cloud Push library.<br/>Please check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | Previous push API call has not been completed.<br/>Please call again after the previous push API callback is executed. |
| PUSH_UNKNOWN_ERROR | 5999 | Unknown push error.<br/>Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

- Refer to the following document for the entire error codes:
    - [Entire Error Codes](./error-codes#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

- Occurs in the TOAST Cloud Push library.
- Need to check TOAST Cloud Push error codes with exception.getDetailCode().
- Refer to the following document for TOAST Cloud Push error codes:
  - [Push > Client SDK Developer's Guide > Error Code Guide > Error Handling](/Notification/Push/en/Client%20SDK%20Guide/#_5)





