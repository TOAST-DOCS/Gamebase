## Game > Gamebase > Upgrade Guide

## 2.71.2

### Android

* Gamebase Android SDK 2.71.2는 다음 이슈가 발생합니다.
    * 네트워크 연결이 끊어진 후 복구되거나 앱을 백그라운드로 내렸다가 포그라운드로 활성화 시킨 경우, 간헐적으로 웹소켓 모듈에서 ArrayIndexOutOfBoundsException으로 인한 크래시가 발생할 수 있습니다.
    * 이슈가 해결된 Gamebase Android SDK 2.72.0을 사용하세요.

## 2.70.0

### Android

* Gamebase Android SDK 2.70.0에서 사용하는 NHN Cloud Android SDK 1.9.5에서는 Android 7.0(API Level 24) 미만 단말기에서 결제를 시도하는 경우 크래시가 발생합니다.
    * 이 문제를 해결하기 위해서는 Gradle에 하위 OS를 위한 [Java 8+ API 디슈가링 지원](https://developer.android.com/studio/write/java8-support#library-desugaring) 선언을 추가해야 합니다.
    * 앱 모듈의 Gradle, Unity의 경우 launcherTemplate.gradle에 다음 선언을 추가하세요.
    
            android {
                compileOptions {
                    // Flag to enable support for the new language APIs
                    coreLibraryDesugaringEnabled true
                }
            }

            dependencies {
                // If AGP 4.0 to 7.2
                coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:1.1.9")
                // If AGP 7.3
                // coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:1.2.3")
                // If AGP 7.4+
                // coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.0.3")
            }
    
    * Unity Editor 버전에 따라 AGP 버전이 다르므로 올바른 버전을 확인하세요.

## 2.69.0

### Unity

* GPGS AutoLogin를 사용하는 경우, **GetLastLoggedInProvider()** 동기 API 대신 신규 추가된 **RequestLastLoggedInProvider(GamebaseCallback.GamebaseDelegate\<string> callback)** 비동기 API를 사용하세요.

### Unreal

* 약관 조회 결과 API인 FGamebaseQueryTermsResult가 수정되었습니다.
    * TermsCountryType의 값이 설정되지 않는 문제를 수정했습니다.
    * bPushEnabled, bAdAgreement, bAdAgreementNight가 제거되었습니다.
* GPGS AutoLogin을 사용하는 경우, **GetLastLoggedInProvider()** 동기 API 대신 신규 추가된 **RequestLastLoggedInProvider(GamebaseCallback.GamebaseDelegate\<string> callback)** 비동기 API를 사용하세요.

### Android

* **gamebase-adapter-auth-gpgs-autologin** 모듈을 빌드에 포함하는 경우, **getLastLoggedInProvider()** 동기 API 대신 신규 추가된 **requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;)** 비동기 API를 사용하세요.

## 2.68.1

### Unreal

* (Windows) WebView 플러그인을 옵션으로 선택할 수 있도록 변경되었습니다.
    * [WebView 플러그인 가이드](./unreal-started/#windows-settings)를 확인하여 업데이트가 필요합니다.
* (Windows) 크래시 로그 전송 시 프로젝트 바이너리 경로에 심벌 파일을 압축한 파일이 생성되도록 추가되었습니다.
    * [크래시 로그 전송 가이드](./unreal-logger/#crash-reporter)

## 2.68.0

### Android

#### Changed Minimum Support Version

* Raised the minimum supported version to Android 5.0 or later. (minSdk 19 -> 21)

## 2.67.1

### Unreal

* (Windows) Purchase 설정 시 스토어를 하나만 선택할 수 있도록 변경되었습니다.
    * 스토어 재설정이 필요합니다.
* (Windows) Epic Games Store 사용 시 EOS SDK의 핸들을 등록하는 과정이 변경되었습니다.
    * Online Subsystem EOS를 사용하는 경우 Gamebase 초기화 시 StoreCode가 Epic Games Store의 해당하는 값이면 자동으로 핸들을 등록합니다.
    * Online Subsystem EOS를 사용하지 않는 경우 [Windows Settings](./unreal-started/#windows-settings) 가이드를 참고하여 EOS의 핸들을 등록하는 과정이 필요합니다.
* (Windows) Steamworks SDK 지원 버전이 1.59로 변경되었습니다.
    * [Steamworks 업그레이드 가이드](./unreal-started/#windows-settings)를 확인하여 업데이트가 필요합니다.

## 2.67.0

### Unity

#### Changed Minimum Support Version

* Changed the minimum supported Unity version from 2020.3.0 to 2020.3.16.
* If you need support for a lower version of Unity, contact [Contact Center](https://toast.com/support/inquiry).

### Unreal

#### Changed Minimum Support Version

* Changed the minimum supported version from UE 4.26 to UE 4.27.

### Android, iOS

* Change the Twitter authentication method to OAuth 2.0 so that login will not work without changing the settings below.
    * Issue OAuth 2.0 Client ID and Client Secret
        * Create an OAuth 2.0 Client ID and Client Secret in the Twitter Developer Portal, then register them in the Gamebase console.
    * Callback URL Settings
        * Set the Callback URL (https://id-gamebase.toast.com/oauth/callback) in the Gamebase console. 
        * Set the same Callback URL in the Twitter Developer Portal.
    * For more information, see the following link.
        * [Game > Gamebase > Console User Guide > App > Authentication Information](./oper-app/#authentication-information)

## 2.66.3

### Unity

#### Changed Minimum Support Version

* Changed the minimum supported Unity version from 2018.4.0 to 2020.3.0.
* If you need support for a lower version of Unity, contact [Customer Center](https://toast.com/support/inquiry).

## 2.66.2

### iOS

#### Changed/Deprecated APIs

* Deprecated the following field.
    * **TCGBWebViewConfiguration.orientationMask**
    
## 2.66.0

### Unreal

* Made changes to how to use APIs.
    * Changed the API from being provided by **IGamebase**, which inherits from `IModuleInterface`, to being provided by **UGamebaseSubsystem**, which inherits from `UGameInstanceSubsystem`.
    * As a subsystem of GameInstance, **UGamebaseSubsystem** follows the GameInstance lifecycle and you must find the subsystem through the GameInstance you use when calling the SDK APIs for use.
    * Changed to conform to naming conventions in the Unreal Coding Standard.

```cpp
if (UGamebaseSubsystem* GamebaseSubsystem = UGameInstance::GetSubsystem<UGamebaseSubsystem>(GetGameInstance()))
{
    GamebaseSubsystem->Initialize(...);
}
```

* As the **GamebaseInterface** module has been removed, you need to remove it from your module dependencies when using Gamebase.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase"
            }
        );

## 2.65.0

### Common

* Fixed an issue when using the image notice feature in Gamebase SDK 2.65.0.
    * Changed the success callback to be called instead of an error if there are no image notices to display.
    * If there are no registered image notices, an empty notice screen appears, at which point Android will crash if you check Stop viewing today and close it.
    * Please use Gamebase SDK 2.65.1 or later, where the issue has been resolved.

### Android

* With the application of Google billing client version 6.2.1, additional settings are required to make payments on Android OS 4.4 (API Level 19) devices.
    * For more information, see [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle > Root level build.gradle](./aos-started/#root-level-buildgradle).

### iOS

* With Facebook SDK updated to 17.0.1, changed to Dynamic Framework.
    * If you download the Gamebase SDK and set it up directly in Xcode, you must add the Facebook SDK to Embedded Frameworks.
    * For more information, see [Game > Gamebase > iOS SDK User Guide > Getting Started > Setting > Xcode Settings](./ios-started/#xcode-settings).

## 2.64.0

### iOS

* Raised the minimum supported version of Kakaogame from 12.0 to 13.0.

## 2.63.0

### iOS

* With the Facebook SDK updated to 17.0.0, you must add FacebookClientToken and FacebookDisplayName to your Info.plist.
```
<key>FacebookClientToken</key>
<string>{FACEBOOK_CLIENT_TOKEN}</string>
<key>FacebookDisplayName</key>
<string>{FACEBOOK_DISPLAY_NAME}</string>
```

### Unreal

* Changed the way Android Firebase Notification is set up so that you need to specify it directly in the settings tool instead of the google-services-json.xml file inside the plugin
    * Removed the previously provided Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml file
     * Set the `GoogleServicesFilePath` value in the [Android Settings Tool](./unreal-started/#android-settings) under FCM in the Push entry in the [Android Settings Tool] (./unreal-started/#android-settings) to the google-services.json file downloaded from the Firebase console.

## 2.62.0

### Android
     
* Gamebase Android SDK 2.62.0 causes the following issues on devices below Android 7.0 (API Level 24). 
    * The Gamebase.loginForLastLoggedInProvider call always fails.
    * Guest accounts are lost.
    * Please use Gamebase Android SDK 2.62.1 where the issue was fixed.

### iOS
* Raised the minimum supported version of Xcode from 14.1 to 15.
* Raised the minimum supported version of Gamebase iOS from 11.0 to 12.0.
* Applied Privacy Manifest and signature to Gamebase and Gamebase Adapter.
    * For new releases or updates after May 1, 2024, you must apply Gamebase iOS SDK 2.62.0 or later according to the Apple policy.
* Raised the minimum supported version of LINE from 11.0 to 13.0.

### Unity
   
* Completed corresponding actions to comply with the Apple Privacy Policy.
    * Added the Privacy Manifest file.
    * Applied signatures to Framework.
    * For new releases or updates after May 1, 2024, you must apply Gamebase SDK for Unity 2.62.0 or later according to the Apple policy.

## 2.59.0

### iOS

* Changed the NAVER iOS SDK used by GamebaseAuthNaverAdapter to xcframework.

## 2.58.0

### Android

#### Twitter IdP
* Updated minSDKVersion to 21 from 19 after Twitter API server certificate update.

### iOS

* Changed the PAYCO iOS SDK used by GamebaseAuthPaycoAdapter to xcframework.

## 2.57.0

### iOS

* Added the Privacy manifest fils.
    * In the Privacy manifest file, you can see a list of APIs that need to specify what data the Gamebase iOS SDK collects and why it is allowed.
    * Please update to Gamebase iOS SDK 2.57.0 or later by Spring 2024 according to the Apple policies. 

### Unreal
 
* The Gamebase module has been separated. To use the Gamebase code, add the **GamebaseInterface** module as a dependency module in the module's Build.cs file.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase",
                "GamebaseInterface"
            }
        );

## 2.56.0

### Unreal

* The provided type has changed from USTRUCT to a general structure.
    * If the type received as a result is not a value provided by default, it is provided in TOptional form. If you used the value previously, you can use the value through Value.GetValue() after checking whether it is a value set through the Value.IsSet() API.

## 2.55.0

### Android

#### Naver IdP
* Raised the minimum supported version from 19 to 21 due to Naver Login SDK's updates.

#### MyCard Adapter
* Raised the minimum supported version from 19 to 21 due to NHN Cloud SDK's updates.

## 2.54.0

### iOS

* Changed Gamebase SDK to xcframework.
* Updated Facebook iOS SDK to 14.1.0. Set the Facebook Client Token in AdditionalInfo in the Gamebase Console.
    * [Game > Gamebase > Console User Guide > App > App > Authentication Information > 1. Facebook](./oper-app/#1-facebook) 

## 2.53.0

### Android

#### Contact

* If you are using the 'Customer Center' feature, you will need to add permission settings to the AndroidManifest.xml following the guide below to ensure that permission requests work properly when selecting attachments.
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)

#### Line IdP

* For the following content of declaration to the AndroidManifest.xml when using Line IdP as shown in [Getting Started](./aos-started), the content is now unnecessary due to the Line SDK update so please delete the following.

```xml
<manifest>
    <!-- [Android11] settings start -->
    <queries>
        <!-- [LINE] Configurations begin -->
        <package android:name="jp.naver.line.android" />
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="https" />
        </intent>
        <!-- [LINE] Configurations end -->
    </queries>
    <!-- [Android11] settings end -->
        
    <application
          tools:replace="android:allowBackup"
          ... >
</manifest>
```

## 2.52.0

### Android

* Confirmed that a crash occurred on Android 4.4 (OS 19 Kitkat) devices.
    * Please use the Gamebase Android SDK 2.52.1 where the issue was fixed.

### Unity

* The **ONE Store v17** purchase adapter, which was displayed as '**IapOnestore**', is now displayed as '**IapOnestoreV17**' starting with the Gamebase Setting Tool (v2.7.0).

### iOS

#### Weibo IdP

* With the WeiboSDK updated to 3.3.3, you must add weibosdk3.3 to info.plist.
```
<key>LSApplicationQueriesSchemes</key>
	<array>
		...
		<string>weibosdk</string>
		<string>weibosdk2.5</string>
		<string>weibosdk3.3</string>
        ...
    </array>
</key>
```

#### Changed/Deprecated APIs
* Starting from iOS 16.4, the following APIs have been deprecated as Apple deprecated the CTCarrier class.
    * **[TCGBGamebase countryCode];**
    * **[TCGBGamebase countryCodeOfUSIM];**
    * **[TCGBGamebase carrierCode];**
    * **[TCGBGamebase carrierName];**
    * **[TCGBUtil countryCode];**
    * **[TCGBUtil usimCountryCode];**
    * **[TCGBUtil carrierCode];**
    * **[TCGBUtil carrierName];**

## 2.50.0

### Android

* Confirmed that a crash occurred on Android 4.4 (OS 19 Kitkat) devices.
    * Please use the Gamebase Android SDK 2.50.1 where the issue was fixed.

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
