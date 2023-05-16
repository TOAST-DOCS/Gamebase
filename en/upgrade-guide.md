## Game > Gamebase > Upgrade Guide

## 2.49.0

### Unreal

* Raised the minimum supported version from 4.22 to 4.26.
* Please update to a new API due to changes to the Query Unconsumed Purchases API.

        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* Make sure to update to a new API due to changes to the Query Activated Subscription API.
    * To get the same results as the existing API, set **FGamebasePurchasableConfiguration.allStores** to **true**.

            // Unity: Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // Unity: New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

### Android

```
Raised the minimum supported verision to Android 4.4.(minSdk 16 -> 19)
```

## 2.47.0

### Android

* When applying Proguard in Unity, API calls related to Purchase fail.
    * The issue has been fixed in the version of 2.48.0.

## 2.45.0

### Android, iOS, Unity

* Please update to a new API due to changes to the Query Unconsumed Purchases API.

        // Unity: Deprecated API
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
        // Unity: New API
        Gamebase.Purchase.RequestItemListOfNotConsumed(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                       GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
        // Android: Deprecated API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity,
                                                       GamebaseDataCallback<List<PurchasableReceipt>> callback);
        // Android: New API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity activity,
                                                       PurchasableConfiguration configuration,
                                                       GamebaseDataCallback<List<PurchasableReceipt>> callback);
        // iOS: Deprecated API
        + (void)requestItemListOfNotConsumedWithCompletion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
        // iOS: New API
        + (void)requestItemListOfNotConsumedWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                                   completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
* Make sure to update to a new API due to changes to the Query Activated Subscription API.
    * To get the same results as the existing API, set **PurchasableConfiguration.setAllStores(true)**.

            // Unity: Deprecated API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
            // Unity: New API
            Gamebase.Purchase.RequestActivatedPurchases(GamebaseRequest.Purchase.PurchasableConfiguration configuration,
                                                        GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback);
            // Android: Deprecated API
            Gamebase.Purchase.requestActivatedPurchases(Activity activity,
                                                        GamebaseDataCallback<List<PurchasableReceipt>> callback);
            // Android: New API
            Gamebase.Purchase.requestActivatedPurchases(Activity activity,
                                                        PurchasableConfiguration configuration,
                                                        GamebaseDataCallback<List<PurchasableReceipt>> callback);
            // iOS: Deprecated API
            + (void)requestActivatedPurchasesWithCompletion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;
            // iOS: New API
            + (void)requestActivatedPurchasesWithConfiguration:(TCGBPurchasableConfiguration *)configuration
                                                    completion:(void(^)(NSArray<TCGBPurchasableReceipt *> * _Nullable purchasableReceiptArray, TCGBError * _Nullable error))completion;

## 2.42.2

### Unity

* Added the ONE store v19 Purchase Adapter to Gamebase SettingTool(v2.5.0).
    * When enabling ONE store v19 Adapter in **SettingTool > Android**, you are connected to the download page of iap_sdk-v19.xx.xx.aar, and must copy the file in **Assets > Plugins > Android**.

## 2.44.0

### Android

* Confirmed that a crash occurred on Android 6.0 (M, API Level 23) when calling registerPush in Gamebase Android SDK 2.44.0.
    * Please use Gamebase Android SDK 2.44.1 where the issue was fixed.  

## 2.43.3

### Unreal

* Changed to Google Billing Client 5.0.0. When using the Online SubSystem GooglePlay plugin provided by Unreal, you must add the value to the /Config/Android/AndroidEngine.ini file so that a build error would not occur.

            [OnlineSubsystemGooglePlay.Store]
            bUseGooglePlayBillingApiV2=False

## 2.42.1

### Unity
    
#### Changed/Deprecated APIs
* The enableFixedFontSize field in FGamebaseWebViewConfiguration is no longer supported.
* Default values have been added to some fields of GamebaseWebViewConfiguration, which may behave differently if no values are set.
    * The default values of navigation bar color R, colorG, colorB, and colorA are set to 18, 93, 230, 255.
    * The default value of the isNavigationBarVisible field to enable the navigation bar is set to true.
    * The default value of the isBackButtonVisible field to enable Go Back button in the webview is set to true.

### Unreal

* (iOS) Added the**Xcode Path** setting to change the path of Xcode in [the iOS Settings tool](./unreal-started/#ios-settings).
    * If you don’t change the path, it is set to default (default: /Applications/Xcode.app).

#### Changed/Deprecated APIs
* The enableKickoutPopup property of FGamebaseConfiguration is no longer supported.
* Default values have been added to some fields in FGamebaseConfiguration, which may behave differently if no values are set.
    * The default value of enableLaunchingStatusPopup is set to true.
    * The default value of enableBanPopup is set to true.
* The enableFixedFontSize field in FGamebaseWebViewConfiguration is no longer supported.
* Default values have been added to some fields in FgamebaseWebViewConfiguration, which may behave differently if no values are set.
    * The default values of navigation bar color R, colorG, colorB, and colorA are set to 18, 93, 230, 255.
    * The default value of the isNavigationBarVisible field to enable the navigation bar is set to true.
    * The default value of the isBackButtonVisible field to enable the Go Back button in WebVeiw is set to true.

## 2.41.0

### Android

* When the custom scheme event registered in WebView works, the WebView is automatically closed.
    * If you want to maintain WebView when the custom scheme works as before, call **GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)** API.
* There is a bug where the 'View' button in the Terms and Conditions screen does not work in Gamebase Android SDK 2.41.0.
    * To use the Gamebase's Terms and Conditions screen, use the Gamebase Android SDK 2.41.1 where the bug is fixed.

### Unity

* Added the required updates to Gamebase SettingTool. (v2.4.0)
    * You need to intall the latest version of SettingTool after completely deleting the previous version of SettingTool from Unity projects.
    * SettingTool v1 is no longer supported.

## 2.40.0

### Unreal

* The enableKickoutPopup property of FGamebaseConfiguration is no longer supported.
* Changed the name of the following APIs.
    * FGamebaseAnalyticsLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* (iOS) Made modifications so that only the necessary frameworks are included when building, with the iOS settings tool.
* (iOS) PLCrashReporter has been updated, so PLCrashReporter inside the engine also needs to be updated.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Installation > iOS Settings > PLCrashReporter](./unreal-started/#ios-settings)
* (iOS) Facebook iOS SDK has been updated to version 9.2.0, so engine code needs to be modified in order to use swift.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Installation > iOS Settings > Facebook SDK](./unreal-started/#ios-settings)

## 2.36.0

### Android

#### Hangame SDK
* Made improvements so that sms_hash is generated internally in Hangame Android SDK v1.4.5.
    * sms_hash does not need to be set anymore.

## 2.35.0

### Android

#### NAVER IdP

* From this version, a token is not deleted when performing NAVER logout.
    * When the user logs in again, the information provision consent window does not appear.
    * The account is not changed when performing web login.
    * To maintain the previous behavior, set AdditionalInfo in the Gamebase Console as follows.

```
{"logout_and_delete_token":true}
```

## 2.34.0

### Android

#### Changed/Deprecated APIs

* The following field has been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **UIPopupConfiguration.enableKickoutPopup**

### iOS

#### Changed/Deprecated APIs

* The following APIs have been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**

### Unity

* The enableKickoutPopup property of GamebaseConfiguration is no longer supported.

## 2.33.0

### iOS

* The error code mapped to TCGB_ERROR_UNKNOWN_ERROR has been changed.
    * Changed the error code mapped to the TCGB_ERROR_UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the TCGB_ERROR_SOCKET_UNKNOWN_ERROR error mapped to the error code 999.

### Unity

* The error code mapped to GamebaseErrorCode.UNKNOWN_ERROR has been changed.
    * Changed the error code mapped to the GamebaseErrorCode.UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the GamebaseErrorCode.SOCKET_UNKNOWN_ERROR error mapped to the error code 999.

### Unreal

* The error code mapped to GamebaseErrorCode.UNKNOWN_ERROR has been changed.
    * Changed the error code mapped to the GamebaseErrorCode::UNKNOWN_ERROR error from 999 to 9999.
    * Newly added the GamebaseErrorCode::SOCKET_UNKNOWN_ERROR error mapped to the error code 999.

## 2.32.0

### Android

* The category for a GamebaseEventHandler event that occurs when the Gamebase Access Token expires and cannot be recovered has been changed from **GamebaseEventCategory.OBSERVER_HEARTBEAT** to **GamebaseEventCategory.LOGGED_OUT**.
    * If you implemented the code to perform login when the GamebaseEventObserverData.code value is **GamebaseError.AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO(3102)** in the **GamebaseEventCategory.OBSERVER_HEARTBEAT** event, change it to perform login in the **GamebaseEventCategory.LOGGED_OUT** event.

## 2.29.0 
 
### iOS

* The minimum supported version of Xcode has been changed from 12 to 13.
    * An error occurs if you run archive build in Xcode 12. Please update to Xcode 13.

### Unity 
 
* Setting Tool 2.0.0 has been released.
    * The folder structure has been changed, so you must delete the previous version of the Setting Tool completely and reinstall the tool.
    * If you are using Setting Tool 1.5.0 or lower, you must remove all Gamebase-related libraries in the following directories.
        * **Assets/Plugins/Android**
        * **Assets/Plugins/iOS**
    * Check the following guide for changed features and how to use them.
        * [Game > Gamebase > Unity Developer's Guide > Getting Started > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)
 
## 2.26.0

### Unity

* If you're using this version, you need to manually delete **Assets/Gamebase/Toast/IAP/Plugins** before use.
    * If Gamebase Unity SDK 2.27.0 or higher is applied, you do not need to delete it.

### Unreal

* The multidex setting has been removed from Gamebase. To enable multidex, see the following guide.
    * [Game > Gamebase > Unreal SDK User Guide > Getting Started > Installation > Android Settings > Enable multidex](./unreal-started/#android-settings)


## 2.25.0

### Android

#### Changed Minimum Support Version

* The minimum Android Gradle Plugin(AGP) version has changed from 2.3.0 to 3.2.0 .
    * AGP 3.3.3 or higher is required to support Android 11 terminals when setting the targetSdkVersion to 30 or higher.
        * Refer to the following document.
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Android 11](./aos-started/#android-11)
* Please contact [Customer Service]( https://toast.com/support/inquiry) if you need support for an earlier version of AGP.

#### AndroidX

* Please apply the following changes to Gradle since the dependency of the Android Support Library has been changed to AndroidX.

* Add migration declarations for libraries that do not support AndroidX in the gradle.properties file.

```groovy
# >>> [AndroidX]
android.useAndroidX=true
android.enableJetifier=true
```

* Add Java 8 build settings for the latest AndroidX to the build.gradle file.

```groovy
android {
    compileOptions {
        // >>> [AndroidX]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

#### Under AGP 3.4.0

* If the Android Gradle Plugin version is lower than 3.4.0, the build will fail, so the following declaration is required in the gradle.properties file:

```groovy
# gradle.properties
# >>> Fix for AGP under 3.4.0
android.enableD8.desugaring=true
android.enableIncrementalDesugaring=false
```

#### LINE IdP

* When using the LINE IdP, the build may fail depending on the AGP version as there is a **&lt;queries&gt;** tag inside the LINE SDK.
    * Please refer to the following guide to upgrade to the AGP version that can build the 'queries' tag.
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Android 11](./aos-started/#android-11)
* If you are using the LINE IdP, an error may occur in Manifest merger when building an application because **android:allowBackup="false"** is declared in LINE SDK. If a build fails for this reason, declare **tools:replace="android:allowBackup"** in the application tag as follows.

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

### iOS

* Changed to return TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR (3009) error when an ASAuthorizationErrorUnknown error occurs in Sign In with an Apple OS.

### Unity

* If you're using this version, you need to manually delete **Assets/Gamebase/Toast/IAP/Plugins** before use.
    * If Gamebase Unity SDK 2.27.0 or higher is applied, you do not need to delete it.

#### Changed Minimum Support Version

* The minimum Unity version changed from April 16, 2017 to April 0, 2018.
* Please contact [Customer Service]( https://toast.com/support/inquiry) if you need support for an older version of Unity.

#### AndroidX Build

* Please add the following declaration when building Android due to AndroidX migration of the Gamebase Android SDK.
* 2019.3 or earlier

```groovy
// mainTemplate.gradle
([rootProject] + (rootProject.subprojects as List)).each {
    ext {
        it.setProperty("android.useAndroidX", true)
        it.setProperty("android.enableJetifier", true)
    }
}
```

* 2019.3 or later

```groovy
// gradleTemplate.properties
android.useAndroidX=true
android.enableJetifier=true
```

#### Under AGP 3.4.0

* If the Unity Editor version is 2018.4.3 or lower or 2019.1.6 or lower, the build will fail because the AGP version is low (3.2.0), so add the following declaration.

```groovy
// mainTemplate.gradle
([rootProject] + (rootProject.subprojects as List)).each {
    ext {
        // >>> Fix for AGP under 3.4.0
        it.setProperty("android.enableD8.desugaring", true)
        it.setProperty("android.enableIncrementalDesugaring", false)
    }
}
```

### Unreal

#### AndroidX Build

* Please add the following declaration to UPL when building Android due to AndroidX migration of the Gamebase Android SDK.

```xml
<gradleProperties>    
  <insert>      
    android.useAndroidX=true      
    android.enableJetifier=true    
  </insert>  
</gradleProperties>
```

## 2.21.2

### iOS

* If you enable bitcode and **archive build** in Gamebase iOS SDK 2.21.1, error occurs.
    * If you want to use bitcode, use Gamebase iOS SDK 2.21.2 to avoid any associated error.

## 2.21.0

### Android

* If a wrong jCenter build is deployed so that **jcenter()** was declared before **mavenCentral()** was declared in Gamebase Android SDK 2.21.0, all Gamebase APIs might crash.
    * In this case, use a properly deployed Gamebase Android SDK 2.21.1 or declare **mavenCentral()** ahead of **jcenter()**.
* Maven Repository
    * As jCenter stopped providing services to individual users, ( [https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) ) new builds can no longer be uploaded.(Users will not be able to access jCenter starting from February 1st, 2022.)
    * For this reason, since Gamebase Android SDK 2.21.0 and later versions will be deployed only to **mavenCentral()** and not to **jcenter()**, please add mavenCentral to gradle repository.

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
}
```

#### LINE IdP

* If you are using LINE IdP, due to the LINE SDK update, you must configure **JavaVersion.VERSION_1_8** in Gradle to make the build succeed.

```groovy
android {
    compileOptions {
        // >>> [LINE IdP]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

### iOS

* Error occurs when using bitcode in Gamebase iOS SDK 2.21.0.
    * If you want to use bitcode, use Gamebase iOS SDK 2.21.1 instead.

## 2.20.2

### iOS

#### Facebook IdP

* In Gamebase iOS SDK 2.20.2, Facebook SDK has been updated to 9.1.0. 
    * Now the Facebook SDK needs additional settings, so please add the following value to the info.plist. If not, the system might crash.
        * FacebookAutoLogAppEventsEnabled
        * FacebookAdvertiserIDCollectionEnabled
* To find out more, see the [Facebook iOS SDK Guide](https://developers.facebook.com/docs/app-events/getting-started-app-events-ios).

## 2.19.0

### Android

#### Weibo IdP

* Alternating the call between Weibo IdP login and another IdP login in Gamebase Android SDK 2.19.0 will lead to a crash.
    * If the Weibo IdP is being used, please use Gamebase Android SDK 2.19.1 where the issue has been fixed.

## 2.18.2

### Android

#### Removed APIs

* In Gamebase Android SDK 2.6.0, the following features were removed after being deprecated.
    * **GamebaseConfiguration.Builder.setFCMSenderId()**
    * **GamebaseConfiguration.Builder.setTencentAccessKey()**
    * **GamebaseConfiguration.Builder.setTencentAccessId()**

## 2.18.0

### Android

#### Purchase Google

* Calling the Google item payment in Gamebase Android SDK 2.18.0 causes a crash.
    * Please use the Gamebase Android SDK 2.18.1 where the issue has been fixed.

## 2.17.0

### Android

* Calling Gamebase.ImageNotice.showImageNotices API in Gamebase Android SDK 2.17.0 causes a crash.
    * Please use Gamebase Android SDK 2.17.4 where the crash issues from 2.17.0 and custom scheme event failure from OS 5.0 - 6.0 have been fixed.

## 2.15.1

### iOS

* If the type **GamebaseEventCategory** defined by the SDK is used in place of the NSString, it has to be updated to **TCGBGamebaseEventCategory**.

## 2.15.0

### Android

#### Purchase Google

* If **gamebase-adapter-purchase-google** is being used, the previous Game Client Version must be set to **Update to the latest version required** if the Gamebase SDK version earlier than 2.15.0 is to be upgraded to 2.15.0 or later.
	* The Google Billing Client module has been updated. Because of this, when purchasing an item while different billing client versions have been applied to multiple devices, any resultant error could lead to a reprocessing problem.

## 2.6.0

### Unity

#### Android Limitation

* With Android Support Library upgraded to 28.0.0, an Android build fails in Unity 5, Unity 2017.1, and Unity 2017.2.  
	* If your Editor is below Unity 2017.3, install a higher version than Unity 2017.3, copy the 'Editor/Data/PlaybackEngines/AndroidPlayer/Tools/gradle/lib' folder and paste it to the same path of your current Unity Editor; then, modify the mainTemplate.gradle file like below.

```groovy
// GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
buildscript {
	repositories {
		jcenter()
        // >>> For download Gradle Android Plugin
        maven {
            url 'https://maven.google.com'
        }
	}

	dependencies {
		//classpath 'com.android.tools.build:gradle:2.1.0'
        // >>> Update Gradle Android Plugin version
        classpath 'com.android.tools.build:gradle:2.3.0'
	}
}
```

#### Firebase Push

* In case of using Firebase Cloud Messaging, download a google-services.json file from Firebase Console and convert it into XML. Include XML resource in your project.
    * XML resource is required in Gamebase version 2.6.0 or higher. Under 2.6.0, Firebase Push works without XML resource.
* Refer to the below guide for implementation.
    * [\[Game > Gamebase > Android Developer's Guide > Push > Settings > Firebase\]](./aos-push/#firebase)

#### Standalone

* Removed Japan Purchase
	* Faded out Japan Purchase.
	* If you're using GamebaseUnitySDK_IAPAdapter, please delete the folders like below:
        * Asset/Toast/Common
        * Asset/Toast/Core
        * Asset/Toast/IAP
        * Asset/Toast/Standalone

### Android

#### Limitation

* Changed the minSdkVersion from 15 (IceCreamSandwichMR1, 4.0.3) to 16 (JellyBean, 4.1).
	* When your minSdkVersion of the project is 15, since normal operation is not ensured for any device below OS 4.1, change it to 16.

#### Removed APIs

* Following functions have been removed: replace them with alternatives.
    * Removed **Gamebase.getAuthBanInfo()**: replace it with **Gamebase.getBanInfo()**.
    * Removed **Gamebase.getLanguageCode()**: replace it with **Gamebase.getDeviceLanguageCode()**.
    * Removed **new GamebaseConfiguration.Builder(void)**: replace it with  **GamebaseConfiguration.newBuilder()**.
    * Removed **new GamebaseConfiguration.Builder.setAppId()**: replace it with  **GamebaseConfiguration.newBuilder()**.
    * Removed **new GamebaseConfiguration.Builder.setAppVersion()**: replace it with **GamebaseConfiguration.newBuilder()**.

#### Changed/Deprecated APIs

* No need to call Gamebase.activeApp() any more since it is automatically called.
* Change the method of creating GamebaseConfiguration, which is required as parameter of Gamebase.initialize().
    * Call **GamebaseConfiguration.newBuilder()**, instead of **new GamebaseConfiguration.Builder(String, String)**.
* Do not call LaunchingStatus.isPlayable() any more.
	* See [\[Game > Gamebase > Android SDK User Guide > Initialization > Launching Information\]](./aos-initialization/#launching-information) and decide if game play is available according to the Launching Status Code.
* Purchase
    * Since store code cannot be changed, it must be delivered at  **GamebaseConfiguration.newBuilder()**.
        * Gamebase.Purchase.getStoreCode() / Gamebase.Purchase.setStoreCode() is scheduled to be removed. Do not use it again.
    * No need to call Gamebase.Purchase.requestRetryTransaction() any more.
* Push
    * Since Gamebase Android SDK 2.6.0, push messages must be sent out via menu of the **Push** tab on the Gamebase console.
        * For Gamebase Android SDK 2.6.0 or lower versions, push must be sent from the **Push(Old)** tab of the Gamebase console.
    * No need to call GamebaseConfiguration.Builder.setFCMSenderId() any more.
    * When GamebaseConfiguration.Builder.setTencentAccessKey(),  GamebaseConfiguration.Builder.setTencentAccessId() is being called, remove the API call and declare for build.gradle as follows:

```groovy
android {
    defaultConfig {
        ...
        // >>> For Tencent Push Notification
        manifestPlaceholders = [
            XG_ACCESS_ID : "1234567890",
            XG_ACCESS_KEY : "ABCDEFGHIJKL",
        ]
    }
}
```

## 2.4.4

### Unity

* Updated the setting tool.
    * Due to changes in the folder structure, the previous SettingTool must be completely removed before re-installed. 

## 2.2.2

### Unity

* Changed the variable name from **storeCodeAOS** to **storeCodeAndroid** for the GamebaseUnitySDKSettings class.
    * If there is any code or prefab which defines store code in reference of **storeCodeAOS**, change the variable to **storeCodeAndroid**, so as variable reference does not fail.

## 2.2.0

### Unity

* Changed the Package Name of GamebaseMainActivity.
    * Unless the MainActivity declaration of AndroidManifest.xml is changed like below, crash may occur:
    * **com.toast.gamebase.activity.GamebaseMainActivity** -> **com.toast.android.gamebase.activity.GamebaseMainActivity**

```xml
<manifest>
    ...
    <application>
    ...
        <activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
            android:launchMode="singleTask"
            android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    ...
    </application>
    ...
</manifest>
```

## 2.1.0

### Common

#### Removed APIs

* Removed TransferKey which is not used.
	* If you need guest account transfer, go to [\[Game > Gamebase > Unity SDK User Guide > Authentication > TransferAccount\]](./unity-authentication/#transferaccount), updated as of Gamebase SDK 2.2.1.
