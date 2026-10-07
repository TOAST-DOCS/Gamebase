<!-- machine_translated: true -->

<!-- pre-align:aligned sig=28cf8b9f9a55 -->

<a id="game-gamebase-release-notes-android"></a>
## Game > Gamebase > Release Notes > Android { #game-gamebase-release-notes-android }

<a id="2-83-0-2026-09-17"></a>
### 2.83.0 (September 17, 2026) { #2-83-0-2026-09-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.83.0/GamebaseSDK-Android.zip)

<a id="830-2026-09-17-added-features"></a>
#### Added Features

* When a Google OOAP (Out-Of-App Purchases) purchase succeeds, the Purchase Updated event in the Gamebase Event Handler is triggered.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Purchase Updated](./aos-etc/#gamebase-event-handler-purchase-updated)

<a id="830-2026-09-17-feature-updates"></a>
#### Feature Updates

* When an automatic retry transaction succeeds after login or when the app returns from the background to the foreground, the Purchase Updated event in the Gamebase Event Handler is triggered.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Purchase Updated](./aos-etc/#gamebase-event-handler-purchase-updated)

<a id="2-82-0-2026-07-28"></a>
### 2.82.0 (2026. 07. 28.) { #2-82-0-2026-07-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.82.0/GamebaseSDK-Android.zip)

<a id="820-2026-07-28-feature-updates"></a>
#### Feature Updates

* Improved internal logic

<a id="820-2026-07-28-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the game build failed when applying 2.81.0 in an environment below AGP 8.0 without upgrading the R8 version.

<a id="2-81-0-2026-06-23"></a>
### 2.81.0 (2026. 06. 23.) { #2-81-0-2026-06-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.81.0/GamebaseSDK-Android.zip)

<a id="810-2026-06-23-feature-updates"></a>
#### Feature Updates

* The payment module dependency has been changed from NHN Cloud IAP SDK (1.12.0) to NHN IAP SDK (2.1.0).
    * Applied Google Play Billing Library 8.3.0.
    * Updated to reflect the OneStore V21 server domain change.

<a id="2-80-2-2026-04-28"></a>
### 2.80.2 (2026. 04. 28.) { #2-80-2-2026-04-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.2/GamebaseSDK-Android.zip)

<a id="802-2026-04-28-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.17.4)

<a id="2-80-1-2026-03-30"></a>
### 2.80.1 (2026. 03. 30.) { #2-80-1-2026-03-30 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.1/GamebaseSDK-Android.zip)

<a id="801-2026-03-30-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the Pending event-related logic added in version 2.80.0 was causing excessive load on the IAP server.

<a id="2-80-0-2026-02-13"></a>
### 2.80.0 (2026. 02. 13.) { #2-80-0-2026-02-13 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.80.0/GamebaseSDK-Android.zip)

<a id="800-2026-02-13-feature-updates"></a>
#### Feature Updates

* Added a new error code, **PURCHASE_PENDING (4008)**, which is returned when a transaction awaits completion (e.g., slow-process payments or parental consent).
* Expanded the functionality of the GamebaseEventCategory.PURCHASE_UPDATED event within the GamebaseEventHandler.
    * The GamebaseEventHandler now supports receiving completion events for pending payments (e.g., slow payments, parental consent) while the app is running.

<a id="800-2026-02-13-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the terms window occasionally appeared oversized.
* Fixed an issue where the automatic notification permission popup was not displayed when code obfuscation was applied.

<a id="2-79-0-2026-01-27"></a>
### 2.79.0 (2026. 01. 27.) { #2-79-0-2026-01-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.79.0/GamebaseSDK-Android.zip)

<a id="790-2026-01-27-feature-updates"></a>
#### Feature Updates

* Added support for targetSdk 36. Fixed an issue where the back navigation in WebView did not function correctly when running a targetSdk 36 build on an Android 16 device.
* Improved internal logic

<a id="2-78-0-2025-12-23"></a>
### 2.78.0 (2025. 12. 23.) { #2-78-0-2025-12-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.78.0/GamebaseSDK-Android.zip)

<a id="780-2025-12-23-feature-updates"></a>
#### Feature Updates

* External SDK update: Play Age Signals library (0.0.2)
    * The Play Age Signals library has been updated.

<a id="2-77-0-2025-12-09"></a>
### 2.77.0 (2025. 12. 09.) { #2-77-0-2025-12-09 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.77.0/GamebaseSDK-Android.zip)

<a id="770-2025-12-09-feature-updates"></a>
#### Feature Updates

* Improved internal payment logic

<a id="2-76-0-2025-11-28"></a>
### 2.76.0 (2025. 11. 28.) { #2-76-0-2025-11-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.76.0/GamebaseSDK-Android.zip)

<a id="760-2025-11-28-added-features"></a>
#### Added Features

* Added the API to verify the age based on Google Play Age Signals to assist with compliance with age verification laws in certain jurisdictions, including Texas, Utah, and Louisiana, USA.
    * [Game > Gamebase > Android SDK User Guide > ETC > Age Signals Support](./aos-etc/#age-signals-support)
    * The Play Age Signals library is currently in beta (0.0.1-beta02), so its APIs will always throw exceptions.
        * For the library to function properly, please use Gamebase Android SDK 2.78.0 or later, which includes Play Age Signals version 0.0.2.

<a id="760-2025-11-28-feature-updates"></a>
#### Feature Updates

* **Gamebase.Purchase.requestItemListAtIAPConsole()** API has been deprecated.
    * Use **Gamebase.Purchase.requestItemListPurchasable()** API.

<a id="2-75-1-2025-10-17"></a>
### 2.75.1 (2025. 10. 17.) { #2-75-1-2025-10-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.1/GamebaseSDK-Android.zip)

<a id="751-2025-10-17-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.17.3)
* Improved internal logic

<a id="2-75-0-2025-09-23"></a>
### 2.75.0 (2025. 09. 23.) { #2-75-0-2025-09-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.75.0/GamebaseSDK-Android.zip)

<a id="750-2025-09-23-feature-updates"></a>
#### Feature Updates
 
* External SDK update: NHN Cloud Android SDK(1.12.0), PAYCO Android SDK(1.5.17), Weibo Android SDK(13.10.5)
* Updated external SDK  to address Google Play's 16KB page constraint
    * Updated NHN Cloud SDK / Weibo SDK
    * Removed 16KB unresponsive gamebase-adapter-auth-weibo-v4 module
* Removed unused module
    * gamebase-adapter-purchase-amazon, gamebase-adapter-push-adm
* Improved internal logic

<a id="2-73-1-2025-08-12"></a>
### 2.73.1 (2025. 08. 12.) { #2-73-1-2025-08-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.1/GamebaseSDK-Android.zip)

<a id="731-2025-08-12-feature-updates"></a>
#### Feature Updates

* External SDK update: Facebook Android SDK(18.0.0)
* Improved internal logic

<a id="731-2025-08-12-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where Naver login would fail when building with AGP 8.5.
* Fixed an issue where the size of the dialog exceeds the screen size on devices with punch holes when selecting Terms and Conditions ->  Learn More.


<a id="2-73-0-2025-07-15"></a>
### 2.73.0 (2025. 07. 15.) { #2-73-0-2025-07-15 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Android.zip)

```
The minimum supported version has been increased to Android 5.1 or later. (minSdk 21 → 22)
The minimum Android Gradle Plugin version has been increased to 7.4.2 or later. (4.0.1 -> 7.4.2)
```

<a id="730-2025-07-15-feature-updates"></a>
#### Feature Updates

* Improved internal logic

<a id="730-2025-07-15-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the login webview would incorrectly calculate margin sizes when rotating the screen.

<a id="2-72-0-2025-06-24"></a>
### 2.72.0 (2025. 06. 24.) { #2-72-0-2025-06-24 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Android.zip)

<a id="720-2025-06-24-feature-updates"></a>
#### Feature Updates

* Fixed logic that could cause ArrayIndexOutOfBoundsException when the websocket module was called multiple times.
    * This issue only occurs in Gamebase Android SDK 2.71.2.

<a id="720-2025-06-24-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where additional region information from LINE IdP was not applied, causing problems in mapping operations.
    * Issues that fail to call Gamebase.login("guest") -&gt; Gamebase.addMapping("line") -&gt; Gamebase.loginForLastLoggedInProvider()
    * Issues that fail to call Gamebase.login(idp) -&gt; Gamebase.addMapping("line") -&gt; AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER(3302) -&gt; Gamebase.changeLogin(ForcingMappingTicket)
    * Issues that fail to call Gamebase.login("line") -&gt; Gamebase.addMapping(idP) -&gt; AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER(3302) -&gt; Gamebase.changeLogin(ForcingMappingTicket)

<a id="2-71-2-2025-05-20"></a>
### 2.71.2 (2025. 05. 20.) { #2-71-2-2025-05-20 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.2/GamebaseSDK-Android.zip)

<a id="712-2025-05-20-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.17.2)
* Added Sign-in with Google login on devices with older versions of Google Play Services
* Improved internal logic

<a id="2-71-1-2025-04-29"></a>
### 2.71.1 (2025. 04. 29.) { #2-71-1-2025-04-29 }

[SDK Download](https://static.toaㄴㄴstoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Android.zip)

<a id="711-2025-04-29-bug-fixes"></a>
#### Bug Fixes

* Fixed an error related to webview size calculation

<a id="2-71-0-2025-04-15"></a>
### 2.71.0 (2025. 04. 15.) { #2-71-0-2025-04-15 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Android.zip)

<a id="710-2025-04-15-added-features"></a>
#### Added Features

* Added a new feature: Game Notice
    * Gamebase.GameNotice.openGameNotice(Activity activity, GamebaseCallback onCloseCallback);
    * To learn how to call the API, see the following link.
        * [Game > Gamebase > Android SDK User Guide > UI > GameNotice](./aos-ui/#gamenotice)

<a id="710-2025-04-15-feature-updates"></a>
#### Feature Updates

* Changed behavior to return an **INVALID_PARAMETER(3)** error instead of throwing an exception when calling Gamebase initialization with storeCode set to null.

<a id="2-70-1-2025-03-13"></a>
### 2.70.1 (2025. 03. 13.) { #2-70-1-2025-03-13 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.1/GamebaseSDK-Android.zip)

<a id="701-2025-03-13-bug-fixes"></a>
#### Bug Fixes

* Resized the X-buttons in the Apple ID, Steam, and Twitter login navigation bars.
* Fixed an issue that prevented Kotlin files from referencing AuthProvider's IdP constant (e.g. AuthProvider.GUEST).

<a id="2-70-0-2025-03-11"></a>
### 2.70.0 (2025. 03. 11.) { #2-70-0-2025-03-11 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Android.zip)

<a id="700-2025-03-11-added-features"></a>
#### Added Features

* External SDK update: NHN Cloud SDK(1.9.5)
    * Applied Google billing client version 7.1.1.
    * In NHN Cloud Android SDK 1.9.5, a crash occurs when attempting to make a payment on a device running Android 7.0 (API Level 24) or lower.
        * To work around this issue, Gradle needs to add a [Java 8+ API desugaring support](https://developer.android.com/studio/write/java8-support#library-desugaring) declaration for lower OSes.
        * For the app module's Gradle or in the case of Unity, add the following declaration to launcherTemplate.gradle.
        
                android {
                    compileOptions {
                        // Flag to enable support for the new language APIs
                        coreLibraryDesugaringEnabled true
                    }
                }

                dependencies {
                    // desugar_jdk_libs 2.+ needs AGP 7.4+
                    coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.5")
                }
        
        * Using desugar_jdk_libs version 1.x may cause a crash during Kakaogame login. We recommend using version 2.x instead.
            * The required AGP (Android Gradle Plugin) and Gradle versions may vary depending on the Unity Editor version. You may need to update them accordingly.  
* Added an initialization option for the GPGS Auto Login feature that prompts the user to log in to GPGS only once after installing the app.
    * **GamebaseConfiguration.Builder.enableGPGSSignInCheck(boolean)**
    * By default, this is set to true, which means the GPGS login prompt will appear again during Gamebase initialization even if the user previously declined.
    * If set to false, the GPGS login prompt is shown only once when the app is launched for the first time.
* Added a new error code indicating that an error occurred on the IdP server during login.
    * AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* Added options to configure the navigation bar title color and icon tint color in GamebaseWebView.
    * **GamebaseWebViewConfiguration.Builder.setNavigationBarTitleColor(int)**
    * **GamebaseWebViewConfiguration.Builder.setNavigationBarIconTintColor(int)**

<a id="700-2025-03-11-feature-updates"></a>
#### Feature Updates

* When integrating the 'GPGS Auto Login' feature, if the user does not log in to GPGS, the behavior where Gamebase kept attempting GPGS login during initialization, login, and logout has been changed to attempt it only during Gamebase initialization.
* Updated the navigation bar for Apple ID, Steam, and Twitter login screens to display the close (X) button in the same color as the title.

<a id="700-2025-03-11-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where LaunchingInfo data was not updated in the user event handler.
* Fixed an issue where the aspect ratio of image notices was displayed differently from the original image ratio in Unity builds.

<a id="2-69-0-2025-01-21"></a>
### 2.69.0 (2025. 01. 21.) { #2-69-0-2025-01-21 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Android.zip)

<a id="690-2025-01-21-added-features"></a>
#### Added Features

* Added the **Gamebase.requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) asynchronous API**.
    * **Gamebase.getLastLoggedInProvider() synchronous API** may sometimes fail to return the correct value due to timing issues.
    * When including the **gamebase-adapter-auth-gpgs-autologin** module in the build, it takes some time to fetch data from the GPGS server. Therefore, calling the synchronous getLastLoggedInProvider() API immediately after Gamebase initialization may not return the correct value.
    * In this case, the asynchronous API requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) guarantees the correct value.
    * If the gamebase-adapter-auth-gpgs-autologin module is not included in the build, it is safe to continue using the synchronous getLastLoggedInProvider() API.
* Added the **GamebaseWebViewConfiguration.Builder.setCutoutAreaColor() API**.
    * When **GamebaseWebViewConfiguration.Builder.renderOutsideSafeArea() API** is set to **false**, padding is automatically added to the cutout area.
    * The setCutoutAreaColor() API allows you to set the color of this added padding area.
    * If renderOutsideSafeArea() is set to false but setCutoutAreaColor() is not specified, the padding area's color will be automatically determined by the web page body's background-color value.

<a id="690-2025-01-21-feature-updates"></a>
#### Feature Updates

* When the **gamebase-adapter-auth-gpgs-autologin** module is included in the build, calling the **Gamebase.getLastLoggedInProvider() synchronous API** immediately after Gamebase initialization previously returned null because the internal data had not yet been initialized. This logic has been changed to return the string **"NOT_INITIALIZED_YET"** instead.
* Updated the internal logic so that when **GamebaseWebViewConfiguration.Builder.renderOutsideSafeArea() API** is to **false**, the webview is now displayed **edge-to-edge**, including the cutout area.
    * Padding is automatically added to prevent content from being obscured.

<a id="690-2025-01-21-bug-fixes"></a>
#### Bug Fixes
 
* Fixed an issue where calling the Gamebase.Push.getNotificationOptions() API before login could cause a crash.
* Added defensive code to address an issue where the loading progress bar occasionally did not disappear or caused a crash.
* Added defensive code to prevent crashes caused by duplicate callback invocations in WebSocket under certain conditions.

<a id="2-68-0-2024-11-26"></a>
### 2.68.0 (2024. 11. 26.) { #2-68-0-2024-11-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Android.zip)

```
Raised the minimum supported version to Android 5.0 or later. (minSdk 19 -> 21)
```

<a id="680-2024-11-26-added-features"></a>
#### Added Features

* Added auto sign-in integration with Google Play Games Services accounts.
    * To enable this feature, you must add the **gamebase-adapter-auth-gpgs-autologin** module declaration to your build dependencies.

            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-auth-gpgs-autologin:$GAMEBASE_SDK_VERSION"
            }
            
    * You can also refer to the following guides to set up additional information
        * [Game > Gamebase > Android SDK User Guide > Get Started > Setting > AndroidManifest.xml > GPGS IdP](./aos-started/#gpgs-idp)

<a id="680-2024-11-26-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.17.0)
* Updated Google authentication libraries.
    * Google Sign-In for Android has been deprecated and switched to Google Credential Manager.
    * Authentication method changed from AuthCode to OIDC token.
* Fixed webview not redirecting URLs when a registered custom scheme is matched.

<a id="2-67-0-2024-10-29"></a>
### 2.67.0 (2024. 10. 29.) { #2-67-0-2024-10-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Android.zip)

<a id="670-2024-10-29-added-features"></a>
#### Added Features

* Added Steam authentication adapter.

<a id="670-2024-10-29-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK(1.9.3)
* Twitter has changed its authentication method to OAuth 2.0, so login will not work without changing the settings below.
    * Issue OAuth 2.0 Client ID and Client Secret
        * Create an OAuth 2.0 Client ID and Client Secret in the Twitter Developer Portal, then register in the Gamebase console.
    * Callback URL Settings
        * Set the Callback URL (https://id-gamebase.toast.com/oauth/callback) in the Gamebase console.
        * Add the same Callback URL to the Twitter Developer Portal.
    * For more information, see the following link.
        * [Game > Gamebase > Consolue User Guide > App > Authentication Information > 6. Twitter](./oper-app/#6-twitter)

<a id="670-2024-10-29-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where touching Detail after disconnecting from the network while the terms screen was exposed would cause the terms popup to exit.

<a id="2-66-3-2024-09-10"></a>
### 2.66.3 (2024. 09. 10.) { #2-66-3-2024-09-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Android.zip)

<a id="663-2024-09-10-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK(1.9.2)
    * Fixed an issue where Native Crash logs are intermittently not reported on Android 13 and above devices.
    * Improved Amazon payment reprocessing.

<a id="2-66-2-2024-08-27"></a>
### 2.66.2 (2024. 08. 27.) { #2-66-2-2024-08-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-Android.zip)

<a id="662-2024-08-27-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK(1.9.1), Kakaogame SDK(3.19.3), PAYCO SDK(1.5.15)
* Added supplemental logic to ensure that when a problem occurs with an Amazon store checkout and reprocessing is triggered, the item is awarded to the User ID that first attempted the payment.
* Changed the color and name of Twitter login title bar.
* Fixed a failure callback to be called instead of the previous success callback when an error occurs inside the webview of a rolling image announcement.

<a id="662-2024-08-27-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where, when an Activity is destroyed, the WebView floating on top of the destroyed Activity is closed and the close event callback is missing.
* Added defensive logic to prevent the Hangame Login Adapter from causing an already resumed error if a duplicate callback is received when logging into an external idP.

<a id="2-66-1-2024-07-23"></a>
### 2.66.1 (2024. 07. 23.) { #2-66-1-2024-07-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Android.zip)

<a id="661-2024-07-23-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the `gamebase://dismiss` scheme did not work on Android 14 devices when built with targetSdk 34, preventing custom schemes from exiting the webview.

<a id="2-66-0-2024-07-10"></a>
### 2.66.0 (2024. 07. 10.) { #2-66-0-2024-07-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Android.zip)

<a id="660-2024-07-10-added-features"></a>
#### Added Features

* Added GPGS v2 authentication
    * For more details on how to set, see the following document.
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > GPGS IdP](./aos-started/#gpgs-idp)

<a id="2-65-1-2024-06-25"></a>
### 2.65.1 (2024. 06. 25.) { #2-65-1-2024-06-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-Android.zip)

<a id="651-2024-06-25-feature-updates"></a>
#### Feature Updates

* Fixed so that if there are no images to show on a particular client, a success callback is called instead of an error.

<a id="651-2024-06-25-bug-fixes"></a>
#### Bug Fixes  

* Fixed an error where, when an empty image notice is exposed if there were no registered image notices, a crash occurs on closing after checking the Show less for today.

<a id="2-65-0-2024-06-11"></a>
### 2.65.0 (2024. 06. 11.) { #2-65-0-2024-06-11 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-Android.zip)

<a id="650-2024-06-11-added-features"></a>
#### Added Features

* Added a new type to the image notice feature.
    * Added the `Rolling Popup` type.
    * Displays the existing image notice as the `Individual Popup` type.

<a id="650-2024-06-11-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK(1.9.0), Hangame Android SDK(1.13.0)
    * Applied Google billing client version 6.2.1.
    * Additional settings are required to make payments on Android OS 4.4 (API Level 19) devices.
        * For more information, see [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle > Root level build.gradle](./aos-started/#root-level-buildgradle).
* Improved internal logic

<a id="2-64-0-2024-05-28"></a>
### 2.64.0 (2024. 05. 28.) { #2-64-0-2024-05-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Android.zip)

<a id="640-2024-05-28-feature-updates"></a>
#### Feature Updates

* External SDK update: Kakaogame SDK (3.19.0), PAYCO SDK (1.5.14)
* Changed so that the back key does not run when the Terms and Conditions popup appears.

<a id="640-2024-05-28-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where Gamebase internal messages do not appear correctly due to string resource reference failures on devices below API Level 23 (OS 6.0, M).

<a id="2-63-0-2024-04-23"></a>
### 2.63.0 (2024. 04. 23.) { #2-63-0-2024-04-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Android.zip)

<a id="630-2024-04-23-feature-updates"></a>
#### Feature Updates

* Improved internal logic

<a id="2-62-1-2024-03-29"></a>
### 2.62.1 (2024. 03. 29.) { #2-62-1-2024-03-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.1/GamebaseSDK-Android.zip)

<a id="621-2024-03-29-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where the Gamebase.loginForLastLoggedInProvider call would always fail on devices below Android 7.0 (API Level 24) and the Guest account would be lost. 
    * This bug only occurs in Gamebase Android SDK 2.62.0.

<a id="2-62-0-2024-03-26"></a>
### 2.62.0 (2024. 03. 26.) { #2-62-0-2024-03-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Android.zip)

<a id="620-2024-03-26-feature-updates"></a>
#### Feature Updates

*  Added a testDevice field to the LaunchingInfo VO returned after Gamebase initialization to indicate that it is a test device.

<a id="620-2024-03-26-620-2024-03-26-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.9.0)
* Improved internal logic so that Preference cannot be copied for use.
* Incorporated the gamebase-sdk-base module into a single gamebase-sdk module.

<a id="2-61-0-2024-02-27"></a>
### 2.61.0 (2024. 02. 27.) { #2-61-0-2024-02-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-Android.zip)

<a id="610-2024-02-27-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK(1.8.4)
* Added a login method with Twitter callback URL.
* Added a declaration to the AndroidManifest to enable the use of Photo Picker, which does not require permission, when uploading photos to the Customer Center. Accordingly, the runtime permission request for READ_EXTERNAL_STORAGE has been removed.
* Improved internal logic

<a id="2-60-0-2024-01-23"></a>
### 2.60.0 (2024. 01. 23.) { #2-60-0-2024-01-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Android.zip)

<a id="600-2024-01-23-feature-updates"></a>
#### Feature Updates

* External SDK update: PAYCO Android SDK (1.5.13)
* Moved the queries declaration required when using the ONE store adapter inside the SDK.
* Improved internal logic

<a id="600-2024-01-23-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where ConcurrentModificationException exception occurs intermittenly when running the app.

<a id="2-59-0-2023-12-19"></a>
### 2.59.0 (2023. 12. 19.) { #2-59-0-2023-12-19 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-Android.zip)

<a id="590-2023-12-19-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK (1.7.2)
* Improved internal logic

<a id="590-2023-12-19-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where .wav format files could not be uploaded in the Customer Center.

<a id="2-58-0-2023-11-28"></a>
### 2.58.0 (2023. 11. 28.) { #2-58-0-2023-11-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-Android.zip)

<a id="580-2023-11-28-feature-updates"></a>
#### Feature Updates

* External SDK update: Kakaogame version update (3.17.5)
* Updated Twitter Adapter minSDK to 21 due to Twitter API server certificate renewal
* Improved internal logic

<a id="580-2023-11-28-bug-fixes"></a>
#### Bug Fixes

* Added a defense code to prevent a crash when an empty string is entered in the message of the Gamebase.Logger.report(String message, ...) API.

<a id="2-57-0-2023-10-31"></a>
### 2.57.0 (2023. 10. 31.) { #2-57-0-2023-10-31 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Android.zip)

<a id="570-2023-10-31-feature-updates"></a>
#### Feature Updates

* External SDK update: Naver Login Android SDK(5.8.0)

<a id="570-2023-10-31-added-featrues"></a>
#### Added Featrues

* Added a new API to send exceptions to Log & Crash.

        Gamebase.Logger.report(String message, Throwable throwable);
        Gamebase.Logger.report(String message, Throwable throwable, Map<String, String> userFields);

<a id="570-2023-10-31-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where an EmptyStackException would rarely occur when Gamebase WebView close().

<a id="2-56-1-2023-10-17"></a>
### 2.56.1 (2023. 10. 17.) { #2-56-1-2023-10-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.1/GamebaseSDK-Android.zip)

<a id="561-2023-10-17-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud Android SDK (1.8.0)
    * Google billing client version 5.2.1 has been applied.
    * When new or app updates are made to the Google Play Store after 2023/11/01, it is necessary to apply the corresponding version. For more information, please refer to the following link.
    * [Google Play Billing Library version deprecation](https://developer.android.com/google/play/billing/deprecation-faq)

<a id="2-56-0-2023-09-26"></a>
### 2.56.0 (2023. 09. 26.) { #2-56-0-2023-09-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Android.zip)

<a id="560-2023-09-26-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK (1.7.1)

<a id="2-55-0-2023-09-12"></a>
### 2.55.0 (2023. 09. 12.) { #2-55-0-2023-09-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-Android.zip)

<a id="550-2023-09-12-feature-updates"></a>
#### Feature Updates

* External SDK update: Naver Login Android SDK(5.7.0), NHN Cloud Android SDK(1.7.1)
* Fixed a cross-app scripting vulnerability in the OAuthLoginInAppBrowserActivity in older versions of the Naver Login SDK.
* Added a defense logic to prevent crashes when using Naver IdP on devices below API 21, which are not supported by Naver IdP.

<a id="550-2023-09-12-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the loading animation off is not applied when idP Login.
* Fixed an issue where the navigation bar reappears when the windowFocus is changed in API Level 28, 29 fullscreen webview.
* Added a defensive logic to prevent crashing if Weibo login is successful but access token is returned as null from Weibo SDK intermittently.

<a id="2-53-0-2023-08-17"></a>
### 2.53.0 (2023. 08. 17.) { #2-53-0-2023-08-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.53.0/GamebaseSDK-Android.zip)

<a id="530-2023-08-17-added-features"></a>
#### Added Features

* Added a new API to specify an option that hides the loading animation when calling loginForLastLoggedInProvider.
    * Gamebase.loginForLastLoggedInProvider(Activity activity, Map&lt;String, Object&gt; additionalInfo, GamebaseDataCallback&lt;AuthToken&gt; callback);
    * For more details on how to call API, see the following documents.
        * [Game > Gamebase > Android SDK User Guide > Authentication > Login > Login Flow > Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp)

<a id="530-2023-08-17-feature-updates"></a>
#### Feature Updates

* External SDK update: Facebook Android SDK(16.1.2), Line Android SDK(5.8.1), Weibo Android SDK(13.5.0)
* Improved so that, when attaching files in the Customer Center Webview, permissions are automatically acquired according to albums, cameras, storage types, and run the right feature for the type.
    * To use the enhanced file attachment feature in the Customer Center, you need to add permission settings to the AndroidManifest.xml by following the guide below.
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)

<a id="2-52-1-2023-07-17"></a>
### 2.52.1 (2023. 07. 17.) { #2-52-1-2023-07-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Android.zip)

<a id="521-2023-07-17-feature-updates"></a>
#### Feature Updates

* External SDK version changed: OkHttp 3.12.13 (downgraded from 4.10.0)

<a id="521-2023-07-17-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where a crash occurs on Android 4.4 (OS 19 Kitkat) devices due to the mimum supported OS version raised to 21 starting from OkHttp 3.13.

<a id="2-52-0-2023-06-27"></a>
### 2.52.0 (2023. 06. 27.) { #2-52-0-2023-06-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-Android.zip)

<a id="520-2023-06-27-added-features"></a>
#### Added Features

* Added ONE store v21 Adapter.
* Added custom push receiver with the feature to suppress notifications with certain messages.
    * To enable this feature, add the **gamebase-adapter-push-notification** module declaration to your build dependencies.
    
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-push-notification:$GAMEBASE_SDK_VERSION"
            }

<a id="520-2023-06-27-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud SDK 1.6.0

<a id="520-2023-06-27-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where the navigation bar and X button overlapps in horisontal mode of Render outside safe area.
* Fixed the Terms and Conditions details page that appears when you click "Detail" in the Terms and Conditions window to not be clickable in the background until it finishes loading.

<a id="2-50-1-2023-07-17"></a>
### 2.50.1 (2023. 07. 17.) { #2-50-1-2023-07-17 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Android.zip)

<a id="501-2023-07-17-feature-updates"></a>
#### Feature Updates

* External SDK version changed: OkHttp 3.12.13 (downgraded from 4.10.0)

<a id="501-2023-07-17-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where a crash occurs on Android 4.4 (OS 19 Kitkat) devices due to the mimum supported OS version raised to 21 starting from OkHttp 3.13.

<a id="2-50-0-2023-05-16"></a>
### 2.50.0 (2023. 05. 16.) { #2-50-0-2023-05-16 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.50.0/GamebaseSDK-Android.zip)

<a id="500-2023-05-16-added-features"></a>
#### Added Features

* Added MyCard Adapter.

<a id="500-2023-05-16-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud Android SDK 1.5.0, Gson 2.8.9, OkHttp 4.10.0, PAYCO Android SDK 1.5.12

<a id="500-2023-05-16-bug-fixes"></a>
#### Bug Fixes

* Fixed an error where, when calling the Terms and Conditions API, Activity size is reduced within a safe area.

<a id="2-49-0-2023-04-25"></a>
### 2.49.0 (2023. 04. 25.) { #2-49-0-2023-04-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Android.zip)
```
Raised the minimum supported version to Android 4.4.(minSdk 16 -> 19)
```

<a id="490-2023-04-25-feature-updates"></a>
#### Feature Updates

* Improved the internal metrics

<a id="490-2023-04-25-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where, when including the following adapters in the build, unnecessary READ_PHONE_STATE permission is added.
    * gamebase-adapter-auth-facebook
    * gamebase-adapter-auth-hangame
    * gamebase-adapter-auth-line
    * gamebase-adapter-purchase-google
    * gamebase-adapter-purchase-onestore
    * gamebase-adapter-purchase-onestore-external
    * gamebase-adapter-purchase-onestore-v16
    * gamebase-adapter-purchase-onestore-v19
    * gamebase-adapter-push-adm
    * gamebase-adapter-push-fcm

<a id="2-48-0-2023-03-28"></a>
### 2.48.0 (2023. 03. 28.) { #2-48-0-2023-03-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-Android.zip)

<a id="480-2023-03-28-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud Android SDK(1.4.2), PAYCO Android SDK(1.5.11)
* Applied the standby domain for Gamebase server in preparation for DNS failure
* Improved the internal logic

<a id="480-2023-03-28-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where, when proguard is applied in Unity, API calls related to Purchase fails.

<a id="2-47-0-2023-02-14"></a>
### 2.47.0 (2023. 02. 14.) { #2-47-0-2023-02-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.47.0/GamebaseSDK-Android.zip)

<a id="470-2023-02-14-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK (1.6.3)
* Improved the internal logic

<a id="2-46-0-2023-01-31"></a>
### 2.46.0 (2023. 01. 31.) { #2-46-0-2023-01-31 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-Android.zip)

<a id="460-2023-01-31-added-features"></a>
#### Added Features

* Added an API to retrieve subscription statuses.
    * Gamebase.Purchase.requestSubscriptionsStatus(Activity, PurchasableConfiguration, GamebaseDataCallback&lt;List&lt;PurchasableSubscriptionStatus&gt;&gt;)
    * You can view expired subscription statuses with the PurchasableConfiguration.Builder.setIncludeExpiredSubscriptions(boolean) API.
* Added an option to perform rendering on Cutout area and ignore SafeArea from the WebView.
    * GamebaseWebViewConfiguration.Builder.setRenderOutsideSafeArea(boolean)

<a id="460-2023-01-31-feature-updates"></a>
#### Feature Updates

* External SDK update: Kakaogame SDK (3.14.14)

<a id="2-45-0-2022-12-27"></a>
### 2.45.0 (2022. 12. 27.) { #2-45-0-2022-12-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-Android.zip)

<a id="450-2022-12-27-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud Android SDK (1.4.0), Payco Android SDK (1.5.9), Hangame Android SDK (1.6.2)
* Make sure to update to a new API due to changes to the Query Unconsumed Purcahses API.

        // Deprecated API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity,
                                                       GamebaseDataCallback<List<PurchasableReceipt>>);
        
        // New API
        Gamebase.Purchase.requestItemListOfNotConsumed(Activity,
                                                       PurchasableConfiguration,
                                                       GamebaseDataCallback<List<PurchasableReceipt>>);

* Make sure to update to a new API due to changes to the Query Activated Subscription API.

    * To get the same results as the existing API, set **PurchasableConfiguration.setAllStores(true)**.

            // Deprecated API
            Gamebase.Purchase.requestActivatedPurchases(Activity,
                                                        GamebaseDataCallback<List<PurchasableReceipt>>);

            // New API
            Gamebase.Purchase.requestActivatedPurchases(Activity,
                                                        PurchasableConfiguration,
                                                        GamebaseDataCallback<List<PurchasableReceipt>>);

<a id="450-2022-12-27-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where ConcurrentModification exceptions occur intermittenly while running the app.
* Fixed an issue where, when calling Gamebase.getAuthProviderUserID() after logging in with Hangame thirdIdP, NullPointerException occurs.

<a id="2-44-2-2022-11-29"></a>
### 2.44.2 (2022. 11. 29.) { #2-44-2-2022-11-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.2/GamebaseSDK-Android.zip)

<a id="442-2022-11-29-added-features"></a>
#### Added Features

* Added the 'storeCode' field to the PurchasableReceipt VO class.

<a id="442-2022-11-29-feature-updates"></a>
#### Feature Updates

* External SDK update: Kotlin(1.7.20), Hangame Android SDK(1.6.1)
* Modified the Gamebase WebView by reflecting the recommendations in 'Google Play Pre-Launch Report'.
    * Expanded the title bar size
    * Modified the image description text

<a id="442-2022-11-29-bug-fixes"></a>
#### Bug Fixes

* Removed the 'deprecated' annotaion incorrectly declared on the 'itemName' field of the PurchasableItem V0 class.

<a id="2-44-1-2022-10-25"></a>
### 2.44.1 (2022. 10. 25.) { #2-44-1-2022-10-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.1/GamebaseSDK-Android.zip)

<a id="441-2022-10-25-added-features"></a>
#### Added Features

* Added the **PushConfiguration.Builder.enableRequestNotificationPermission(boolean)** API so that a popup to request Push permission does not show up automatically when calling the registerPush API from Android 13 OS or higher.

<a id="441-2022-10-25-feature-updates"></a>
#### Feature Updates

* For Facebook Android SDK 13.2.0 or higher, Facebook Client Token must be set.
    * When adding the **facebook_client_token** field to additionalInfo in the Gamebase Console for Gamebase Android SDK 2.44.1 or higher as follows, Facebook Client Token is automatically applied to the client SDK.

            { "facebook_permission": [...], "facebook_client_token": "a01234bc56de7fg89012hi3j45k67890" }

<a id="441-2022-10-25-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where, when calling the **Gamebase.Push.registerPush** API from a device running Android 6.0(M, API Level 23), **IllegalArgumentException** exception occurs.

<a id="2-44-0-2022-10-11"></a>
### 2.44.0 (2022. 10. 11.) { #2-44-0-2022-10-11 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Android.zip)

<a id="440-2022-10-11-feature-updates"></a>
#### Feature Updates

* External SDK update: NHN Cloud Android SDK(1.2.0), TOAST Gamebase IAP Android SDK(0.21.0), Google Play Services Auth(20.0.3)
* Modified to show a popup that automatically requests permission to allow notification when calling registerPush from Android 13 OS.
* Improved the internal logic so that silentSignIn API can be sued when logging into Google.

<a id="440-2022-10-11-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where a crash occurs when logging with the previous version of IdP while logging with Hangame IdP, when no error occurs if you log in with an invalid third-party IdP after using a valid third-party IdP.

<a id="2-43-0-2022-09-07"></a>
### 2.43.0 (2022. 09. 07.) { #2-43-0-2022-09-07 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Android.zip)

<a id="430-2022-09-07-added-features"></a>
#### Added Features

* Added ONE store v19 Purchase Adapter.
    * You can use it by adding the **gamebase-adapter-purchase-onestore-v19** module and [ONE store v19 IAP SDK] to your build dependentices (https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs).
            
            dependencies {
                ...
                implementation files('libs/iap_sdk-v19.00.02.aar')
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
            }
            
<a id="430-2022-09-07-feature-updates"></a>
#### Feature Updates

* External SDK update: Google Billing Client(5.0.0), NHN Cloud Android SDK(1.1.0), TOAST Gamebase IAP Android SDK(0.20.0), Kakaogame Android SDK(3.14.4)
* Added a parameter to enter a service region when logging in to LINE.
    * [Game > Gamebase > Android SDK User Guide > Authentication > Login with IdP](./aos-authentication/#login-with-idp)
* Added a defense logic so that a crash does not occur When using LINE IdP even in devices with API 19 or lower.

<a id="430-2022-09-07-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where a crash occurs when forcibly lowering the Naver Login SDK version to 4.1.4 to use the Naver PLUG SDK or Naver Cafe SDK.

<a id="2-42-1-2022-07-26"></a>
### 2.42.1 (2022. 07. 26.) { #2-42-1-2022-07-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Android.zip)

<a id="421-2022-07-26-feature-updates"></a>
#### Feature Updates

* External SDK update: Facebook Android SDK(11.3.0)

<a id="2-42-0-2022-07-26"></a>
### 2.42.0 (2022. 07. 26.) { #2-42-0-2022-07-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-Android.zip)

<a id="420-2022-07-26-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.5.2)
* Added the mappedUserValid field that represents the mapped user status to the ForcingMappingTicket VO class.
* Modified to fail initialization when the version of Gamebase Adapter does not match the version of Gamebase, as it can cause runtime exception.

<a id="420-2022-07-26-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where Naver web login fails from LDPlayer.
* Fixed an issue where a crash occurs when Twiter login fails due to a low OS verison.

<a id="2-41-2-2022-07-22"></a>
### 2.41.2 (2022. 07. 22.) { #2-41-2-2022-07-22 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.2/GamebaseSDK-Android.zip)

<a id="412-2022-07-22-feature-updates"></a>
#### Feature Updates 

* Changed the default WebView settings to 'Allow cookies'.

<a id="2-41-1-2022-07-12"></a>
### 2.41.1 (2022. 07. 12.) { #2-41-1-2022-07-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-Android.zip)

<a id="411-2022-07-12-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the 'View' button in the Terms and Conditions screen does not work.

<a id="2-41-0-2022-07-05"></a>
### 2.41.0 (2022. 07. 05.) { #2-41-0-2022-07-05 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Android.zip)

<a id="410-2022-07-05-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.31.1), Hangame Android SDK(1.4.6)
* When the custom scheme event registered in WebView works, the WebView is automatically closed.
    * To maintain WebView when the custom scheme event works, call **GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)** API.

<a id="410-2022-07-05-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the system crashes intermittently or login fails when trying login right after Hangame IdP logout

<a id="2-40-0-2022-05-24"></a>
### 2.40.0 (2022. 05. 24.) { #2-40-0-2022-05-24 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Android.zip)

<a id="400-2022-05-24-added-features"></a>
#### Added Features

* Added Purchase Adapter for external payment of ONE store.
    * You can use it by adding the **gamebase-adapter-purchase-onestore-external** module to your build dependencies.
            
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
            }
            
<a id="400-2022-05-24-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.31.0), TOAST Gamebase IAP Android SDK(0.18.5), LINE Android SDK(5.8.0)
* Fixed an issue where push did not work properly when different apps share a single Gamebase project.
    * Declare a different **com.nhncloud.sdk.push.deviceId.salt** value for each app in AndroidManifest.xml.

            <!-- When you have multiple applications sharing an Gamebase project, use this field to identify each application. -->
            <meta-data android:name="com.nhncloud.sdk.push.deviceId.salt"
                       android:value="ApplicationForGoogleStore" />

<a id="2-39-0-2022-05-10"></a>
### 2.39.0 (2022. 05. 10.) { #2-39-0-2022-05-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Android.zip)

<a id="390-2022-05-10-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.30.1)

<a id="2-38-0-2022-05-03"></a>
### 2.38.0 (2022. 05. 03.) { #2-38-0-2022-05-03 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Android.zip)

<a id="380-2022-05-03-added-features"></a>
#### Added Features

* Added the Amazon(ADM) Push Adapter.
    * You can use it by adding the **gamebase-adapter-push-adm** module to your build dependencies.
            
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"
            }
            
    * To apply Proguard, you must apply it by referring to the following guide.
        * [NHN Cloud > SDK User Guide > TOAST Push > Android > Amazon Device Messaging Settings > Download the ADM SDK](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#download-the-adm-sdk)
        * [NHN Cloud > SDK User Guide > TOAST Push > Android > Amazon Device Messaging Settings > Proguard settings](https://docs.toast.com/en/TOAST/en/toast-sdk/push-android/#proguard-settings)

<a id="380-2022-05-03-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.30.0)
* Fixed unnatural sentences in the Traditional Chinese (zh-TW) language set of Display Language.

<a id="2-37-0-2022-04-26"></a>
### 2.37.0 (2022. 04. 26.) { #2-37-0-2022-04-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Android.zip)

<a id="370-2022-04-26-added-features"></a>
#### Added Features

* Added the following field so that you can add parameters after the contact center URL.
    * **ContactConfiguration.Builder.setAdditionalParameters(Map&lt;String, String&gt;)**

<a id="370-2022-04-26-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Gamebase IAP Android SDK(0.18.3)
* Made improvements so that, when userId and gamebaseProductId are missing from the Amazon Appstore payment data, userId and gamebaseProductId are automatically filled in.

<a id="2-36-0-2022-04-12"></a>
### 2.36.0 (2022. 04. 12.) { #2-36-0-2022-04-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Android.zip)

<a id="360-2022-04-12-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.29.2), TOAST Gamebase IAP Android SDK(0.18.2), Hangame Android SDK(1.4.5)
* Made improvements so that sms_hash is generated internally in Hangame Android SDK v1.4.5.
    * sms_hash does not need to be set anymore.

<a id="2-35-0-2022-03-29"></a>
### 2.35.0 (2022. 03. 29.) { #2-35-0-2022-03-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Android.zip)

```
Gamebase Android SDK is now only distributed through Maven Central.
The ZIP file for distribution no longer includes AAR files.
```

<a id="350-2022-03-29-added-features"></a>
#### Added Features

* Added an API to determine whether the terms and conditions window is displayed or not.
    * **Gamebase.Terms.isShowingTermsView()**
* Added an option to fix the font size in the WebView.
    * **GamebaseWebViewConfiguration.Builder.enableFixedFontSize(boolean)**
* Added an option to fix the font size in the terms and conditions window.
    * **GamebaseTermsConfiguration.Builder.enableFixedFontSize(boolean)**
* Added a function to forcibly perform web login even if the Facebook or NAVER app is installed, when logging in with the Facebook or NAVER account.
    * To use this function, set AdditionalInfo in the Gamebase Console as follows.

```
{"enforce_app2web":true}
```

* From this version, a token is not deleted when performing NAVER logout.
    * When the user logs in again, the information provision consent window does not appear.
    * The account is not changed when performing web login.
    * To maintain the previous behavior, set AdditionalInfo in the Gamebase Console as follows.

```
{"logout_and_delete_token":true}
```

<a id="350-2022-03-29-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.29.1), Hangame Android SDK(1.4.4)
* Improvements have been made so that the long white background is not displayed when the terms and conditions window is displayed.

<a id="350-2022-03-29-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the **GamebaseWebViewConfiguration.Builder.setNavigationBarVisible()** API, which hides the WebView's navigation bar, did not work properly.

<a id="2-34-0-2022-02-22"></a>
### 2.34.0 (2022. 02. 22.) { #2-34-0-2022-02-22 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Android.zip)

<a id="340-2022-02-22-added-features"></a>
#### Added Features

* If you select **Add Popup Button** in the Update Required settings of the Gamebase console, a **Details** button will be added to the client's Update Required popup window.
* Added an API to find out whether the device has allowed notifications or not.
    * **Gamebase.Push.queryNotificationAllowed()**
* Added a VO class that can be used to find out whether the terms and conditions UI was displayed after calling the common terms and conditions API.
    * **GamebaseShowTermsViewResult**

<a id="340-2022-02-22-feature-updates"></a>
#### Feature Updates

* The following field has been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **UIPopupConfiguration.enableKickoutPopup**

<a id="340-2022-02-22-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where, when a user selected **Do not show again today** on an image notice, the image notice is not displayed even after 24 hours have passed.

<a id="2-33-0-2022-01-25"></a>
### 2.33.0 (2022. 01. 25.) { #2-33-0-2022-01-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Android.zip)

<a id="330-20220125-added-features"></a>
#### Added Features

* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > Android SDK User Guide > UI > Terms > showTermsView](./aos-ui/#showtermsview)

<a id="330-20220125-feature-updates"></a>
#### Feature Updates

* External SDK update: PAYCO Android SDK(1.5.7), Hangame Android SDK(1.4.3.1), TOAST Gamebase IAP Android SDK(0.18.1)
* Added logic to check whether the launching information has not changed immediately after successful login.

<a id="2-32-0-2021-12-28"></a>
### 2.32.0 (2021. 12. 28.) { #2-32-0-2021-12-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Android.zip)

<a id="320-20211228-added-features"></a>
#### Added Features

* Added the **GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED** type to GamebaseEventCategory of GamebaseEventHandler.
    * Please refer to the following document for how to use this event.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./aos-etc/#server-push)
* Added the **GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category, which works when Gamebase Access Token expires and login is required.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./aos-etc/#logged-out)

<a id="320-20211228-feature-updates"></a>
#### Feature Updates

* Improved the webview so that the ONE store deep link whose webview URL starts with **onestore://** works.

<a id="320-20211228-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug in Gamebase Android SDK 2.31.0 where an IdP account cannot be changed because IdP logout is not called even when logout is called.

<a id="2-31-0-2021-12-14"></a>
### 2.31.0 (2021. 12. 14.) { #2-31-0-2021-12-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Android.zip)

<a id="310-20211214-added-features"></a>
#### Added Features

* Added Amazon Appstore.
    * **STORE_CODE** is **AMAZON**.
    * For how to set up the store, check the following guide.
        * [Game > Gamebase > Store Console Guide > Amazon Appstore Console Guide](./console-amazon-guide)
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle > Define Adapters](./aos-started/#define-adapters)
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Android 11](./aos-started/#android-11)
* Added Huawei AppGallery.
    * **STORE_CODE** is **HUAWEI**.
    * For how to set up the store, check the following guide.
        * [Game > Gamebase > Store Console Guide > Huawei AppGallery Console Guide](./console-huawei-guide)
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle > Define Adapters](./aos-started/#define-adapters)
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Resources > Huawei Store](./aos-started/#resources)

<a id="310-20211214-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK(0.29.0)
* Fixed an issue where it was not possible to register inquiries with banned user information from the Customer Center link in the ban webview.
* Fixed an issue where the launch pop-up was intermittently displayed in English when calling Gamebase initialization as soon as the app was executed.
* Improved the scheduler so that it always checks whether the launch information has changed when the app is switched from the background to the foreground status.

<a id="2-30-0-2021-11-23"></a>
### 2.30.0 (2021. 11. 23.) { #2-30-0-2021-11-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Android.zip)

<a id="300-20211123-added-features"></a>
#### Added Features

* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > Android SDK User Guide > Authentication > Mapping > Add Mapping Forcibly](./aos-authentication/#add-mapping-forcibly)
* Added an API that allows you to log in to the corresponding account when an AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) error occurs after calling Gamebase.addMapping().
    * [Game > Gamebase > Android SDK User Guide > Authentication > Mapping > Change Login with ForcingMappingTicket](./aos-authentication/#change-login-with-forcingmappingticket)

<a id="300-20211123-feature-updates"></a>
#### Feature Updates

* External SDK update: Hangame Android SDK(1.4.2)
* Improved so that the user can modify and use the maintenance details webview HTML provided by Gamebase by default.
    * [Game > Gamebase > Android SDK User Guide > Initialization > Launching Information > 1. Launching > 1.3 Maintenance > Change Default Maintenance HTML](./aos-initialization/#change-default-maintenance-html)
* Fixed an error where the time on the default maintenance webview was displayed in the device language even though DisplayLanguageCode was set.
* Improved the network error that occurred repeatedly by trying to communicate with a disconnected connection when a communication error occurs.

<a id="2-29-0-2021-11-09"></a>
### 2.29.0 (2021. 11. 09.) { #2-29-0-2021-11-09 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Android.zip)

<a id="290-20211109-added-features"></a>
#### Added Features

* Added a feature to declare scope when logging in to Google.
    * [https://developers.google.com/identity/protocols/oauth2/scopes](https://developers.google.com/identity/protocols/oauth2/scopes)
    * If you add **email** as scope, you can obtain email information from the profile.
    * Scope is automatically set when the user logs in if you set the AdditionalInfo in the Gamebase Console as follows.

```
{"scope":["email","myscope1","myscope2",...]}
```

<a id="290-20211109-feature-updates"></a>
#### Feature Updates

* External SDK update: TOAST Android SDK (0.27.4)
* Added DisplayLanguage.Code class, which was described only in the DisplayLanguage guide document and was not actually included in the SDK.
    * [Game > Gamebase > Android SDK User Guide > ETC > Display Language > Types of language codes supported by Gamebase](./aos-etc/#types-of-language-codes-supported-by-gamebase)

<a id="2-28-0-2021-09-28"></a>
### 2.28.0 (2021. 09. 28.) { #2-28-0-2021-09-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Android.zip)

<a id="280-20210928-added-features"></a>
#### Added Features

* Added Kakaogame authentication
* Added a 'purchase abuse automatic release' function.
    * [Game > Gamebase > Android SDK User Guide > Authentication > GraceBan](./aos-authentication/#graceban)
    * The purchase abuse automatic release function allows users who should be banned due to purchase abuse automatic lockdown to be banned after ban suspension status.
    * When a user is in ban suspension status, if the user satisfies all of the release conditions within the set period of time, the user will be able to play normally.
    * If the user does not satisfy the conditions within the period, the user is banned.
* Games that use the purchase abuse automatic release function must always call the AuthToken.getGraceBanInfo() API after login. If a valid GraceBanInfo object that is not null is returned, the user must be informed of the ban release conditions, period, etc.
    * In-game access control for users who are in ban suspension status must be handled by the game.
* Added a feature to display a wait icon while waiting for a login response.

<a id="280-20210928-feature-updates"></a>
#### Feature Updates

* External SDK update: PAYCO Android SDK(1.5.6)

<a id="2-27-1-2021-09-14"></a>
### 2.27.1 (2021. 09. 14.) { #2-27-1-2021-09-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Android.zip)

<a id="271-20210914-feature-updates"></a>
#### Feature Updates

* External SDK update: PAYCO Android SDK (1.5.5), Hangame Android SDK (1.4.1), Weibo Android SDK (11.8.1)
* Added a retry logic when the webview is not displayed normally in the emulator or rooted terminal, so that the webview is displayed normally.
    * This applies to image notification, customer center, and common terms and conditions that run as a webview.
* Improved stability by improving Weibo IdP authentication.
    * Added exception handling, waiting, and retry logic to an API that is a synchronous API but actually operates asynchronously and generates an error.

<a id="271-20210914-bug-fixes"></a>
#### Bug Fixes

* Fixed a bug where the 'Unregistered Game Version' error pop-up was displayed only in English.
* Fixed a bug where the Chinese text was not displayed in the maintenance pop-up.
* Fixed a bug where, if [Credential Login](./aos-authentication/#login-with-credential) is performed, [Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp ) call always fails.

<a id="2-27-0-2021-08-24"></a>
### 2.27.0 (2021. 08. 24.) { #2-27-0-2021-08-24 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Android.zip)

<a id="270-20210824-feature-updates"></a>
#### Feature Updates

* Updated the external SDK: TOAST Android SDK (0.27.1)
* Added ONE Store V16 store

<a id="2-26-0-2021-08-10"></a>
### 2.26.0 (2021. 08. 10.) { #2-26-0-2021-08-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Android.zip)

<a id="260-20210810-feature-updates"></a>
#### Feature Updates

* Improved the Display Language feature.
    * Until now, you had to manually edit the gamebase-sdk-base-version.aar file to add the language set.
        * It has been improved so that you can add the localizedstring.json file to the res/raw folder of the project.
    * Until now, the method of adding the Display Language language set in the Unity guide could not be applied to Android.
        * It has been improved so that the information is reflected in the Android build even if a localizedstring.json file is added, according to the Unity guide.
        * [Game > Gamebase > Unity SDK User Guide > Notes > Additional Features > Display Language > Add New Language Sets](./unity-etc/#display-language-add-new-language-sets)
    * Simplified Chinese (zh-CN), Traditional Chinese (zh-TW), and Thai (th) have been added to the Display Language language set.
    * The default language code was **en**, but it has been improved to reflect the default language set in the Gamebase console.
        * [Game > Gamebase > Console User Guide > App > App > Language settings](./oper-app/#language-settings)
* Changed the creation criteria of the PushConfiguration object that can be created after calling the showTermsView API as follows.
    * Before change
        * A valid non-null PushConfiguration was returned only when **Receive Push Notification** item exists in the terms and conditions.
        * PushConfiguration.pushEnabled was created as false when the user declines to receive both daytime and nighttime promotional push notifications.
    * After change
        * A valid non-null PushConfiguration is always returned if the terms and conditions UI was displayed.
        * The pushEnabled value of the PushConfiguration object returned by showTermsView is always true.
    * Same point without change
        * PushConfiguration is returned as null if the user has already agreed to the terms and conditions and the terms and conditions UI was not displayed.

<a id="260-20210810-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the language code of the message sent from the Push console does not match because the language code of the device is applied to the Push notification language setting without any extra processing.

<a id="2-25-0-2021-07-27"></a>
### 2.25.0 (2021. 07. 27.) { #2-25-0-2021-07-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Android.zip)

<a id="250-20210727-more-features"></a>
#### More Features

* Add monthly payment limit feature
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

<a id="250-20210727-feature-updates"></a>
#### Feature Updates

* Change the dependency of Android Support Library to AndroidX
* Guarantee the PushConfiguration object in the terms and conditions with Push notification items
    * The PushConfiguration to be created as the result of calling Gamebase.Terms.showTermsView API was null if user did not agree to receive push notifications in the terms of UI. It has now changed so that the PushConfiguration object is always returned if there is a Push notification item in the terms and conditions.
    * When user rejects push notifications, the PushConfiguration object is created as (consent to push notifications = false, consent to advertisement push notifications = false, consent to push notifications for advertisements at night = false).
    * The PushConfiguration is null when there is no Push notification item in the terms and conditions.
* External SDK Update
    * TOAST Android SDK(0.26.0)
    * Kotlin(1.5.21)
    * Google Play Services Auth(19.0.0)
    * Facebook Android SDK(11.1.0)
    * NAVER Android SDK(4.4.1)
    * LINE Android SDK(5.6.2)
    * Weibo Android SDK(11.6.0)
* Fixed a crash that occurred when logging in to Weibo.

<a id="2-24-0-2021-06-29"></a>
### 2.24.0 (2021. 06. 29.) { #2-24-0-2021-06-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Android.zip)

<a id="240-20210629-feature-updates"></a>
#### Feature Updates

* Change the internal launch URL
* Fixed incorrect wording in SDK attachments

<a id="2-23-0-2021-06-14"></a>
### 2.23.0 (2021. 06. 14.) { #2-23-0-2021-06-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Android.zip)

<a id="230-20210614-bug-fixes"></a>
#### Bug Fixes

* Fixed the issue of the title of the suspended view details web view not being displayed

<a id="2-22-0-2021-05-25"></a>
### 2.22.0 (2021. 05. 25.) { #2-22-0-2021-05-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Android.zip)

<a id="220-20210525-feature-updates"></a>
#### Feature Updates

* Updated the external SDK: TOAST Android SDK(0.25.0), Hangame Android SDK(1.4.0)

<a id="220-20210525-bug-fixes"></a>
#### Bug Fixes

* The following error has been fixed: When a user logs out and logs in again with another user ID, a payment at Google Play Store is successful but the return value is sometimes "Failed."
* The following error has been fixed: When the name of an app package contains a capital letter, the "Sign In with Apple" log-in fails.

<a id="2-21-1-2021-04-19"></a>
### 2.21.1 (2021. 04. 19.) { #2-21-1-2021-04-19 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-Android.zip)

<a id="211-20210419-bug-fixes"></a>
#### Bug Fixes

* Fixed an issue where the system crashes when canceling Hangame login via PAYCO

<a id="2-21-0-2021-04-13"></a>
### 2.21.0 (2021. 04. 13.) { #2-21-0-2021-04-13 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

<a id="210-20210413-more-features"></a>
#### More Features

* Japanese authentication for Hangame added.	 	

<a id="210-20210413-feature-updates"></a>
#### Feature Updates

* External SDK update: Facebook Android SDK (6.5.1), LINE Android SDK (5.4.0)
	
<a id="210-20210413-bug-fixes"></a>
#### Bug Fixes

* Fixed a crashing error caused when calling payment API on build with Proguard applied.

<a id="2-20-2-2021-03-30"></a>
### 2.20.2 (2021. 03. 30.) { #2-20-2-2021-03-30 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-Android.zip)

<a id="202-20210330-feature-updates"></a>
#### Feature Updates

* Updated to Billing Client Version 3.0.3 where payment errors caused by Android 11 devices in Google Play Store are fixed

<a id="2-20-1-2021-02-23"></a>
### 2.20.1 (2021. 02. 23.) { #2-20-1-2021-02-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-Android.zip)

<a id="201-20210223-bug-fixes"></a>
#### Bug Fixes

* Fixed a logic that could cause the push-fcm module to crash during initialization

<a id="2-20-0-2021-02-09"></a>
### 2.20.0 (2021. 02. 09.) { #2-20-0-2021-02-09 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Android.zip)

<a id="200-20210209-more-features"></a>
#### More Features

* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

<a id="200-20210209-feature-updates"></a>
#### Feature Updates

* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

<a id="2-19-1-2020-12-29"></a>
### 2.19.1 (December 29, 2020) { #2-19-1-2020-12-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Android.zip)

<a id="191-december-29-2020-more-features"></a>
#### More Features

* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added
	
<a id="191-december-29-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

<a id="191-december-29-2020-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.19.0
    * (Unity) Fixed an issue where OutOfMemoryException occurs when retrying in WebSocket
* [SDK] 2.19.1
	* (Android) Fixed an issue where a crash occurred when logging in with another IdP after attempting Weibo login

<a id="2-18-2-2020-12-15"></a>
### 2.18.2 (December 15, 2020) { #2-18-2-2020-12-15 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Android.zip)

<a id="182-december-15-2020-more-features"></a>
#### More Features

* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

<a id="182-december-15-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.18.2
    * (Common) TOAST SDK update: Android(0.24.2), iOS(0.27.1), Unity(0.21.3)
	* (Android) External SDK update to resolve encryption logic security warnings: PAYCO Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
<a id="182-december-15-2020-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

<a id="2-18-1-2020-11-10"></a>
### 2.18.1 (November 10, 2020) { #2-18-1-2020-11-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.1/GamebaseSDK-Android.zip)

<a id="181-november-10-2020-more-features"></a>
#### More Features

* Added Galaxy Store: SDK 2.18.0

<a id="181-november-10-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.18.0
    * (Android) TOAST SDK update: Android(0.24.1) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings

<a id="181-november-10-2020-bug-fixes"></a>
#### Bug Fixes  

* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

<a id="2-17-1-2020-10-13"></a>
### 2.17.1 (October 13, 2020) { #2-17-1-2020-10-13 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Android.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

<a id="171-october-13-2020-more-features"></a>
#### More Features

* Added Hangame IdP authentication: SDK 2.17.0

<a id="171-october-13-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.17.0
	* (Common) Supports the download feature when a Customer Center attachment image is clicked
	* (Common) Updated TOAST SDK: Android(0.23.2), Unity(0.21.2)

<a id="171-october-13-2020-bug-fixes"></a>
#### Bug Fixes  

* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
<a id="2-16-0-2020-09-22"></a>
### 2.16.0 (September 22, 2020) { #2-16-0-2020-09-22 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Android.zip)

<a id="160-september-22-2020-more-features"></a>
#### More Features

* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
<a id="2-15-0-2020-08-25"></a>
### 2.15.0 (August 25, 2020) { #2-15-0-2020-08-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Android.zip)
```
Updated Google Billing Client in the Gamebase SDK 2.15.0 version. 

For 'gamebase-adapter-purchase-google', to upgrade a version below Gamebase SDK 2.15.0 to more than 2.15.0,  
set 'Requires Update' for 'Game Client Version' of the previous version.

This is because, in order to execute reprocessing when an error occurs during purchasing an item,  
you may encounter an issue during reprocessing if a different billing client version is applied to each of many devices.   
```

<a id="150-august-25-2020-more-features"></a>
#### More Features

* [SDK] 2.15.0
    * (Common) Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting  
    * (Common) Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)

<a id="150-august-25-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)

<a id="2-13-0-2020-07-28"></a>
### 2.13.0 (July 28, 2020) { #2-13-0-2020-07-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Android.zip)

<a id="130-july-28-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 

<a id="130-july-28-2020-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

<a id="2-12-0-2020-07-14"></a>
### 2.12.0 (July 14, 2020) { #2-12-0-2020-07-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Android.zip)

<a id="120-july-14-2020-more-features"></a>
#### More Features

* Image Notices: Shows image popups within a game according to exposed period and priority order 
    * [SDK] 2.12.0: Added Show Image Notice API 
  
<a id="2-11-0-2020-06-23"></a>
### 2.11.0 (June 23, 2020) { #2-11-0-2020-06-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Android.zip)

<a id="110-june-23-2020-more-features"></a>
#### More Features

* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

<a id="2-10-0-2020-05-26"></a>
### 2.10.0 (May 26, 2020) { #2-10-0-2020-05-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Android.zip)

<a id="100-may-26-2020-more-features"></a>
#### More Features

* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

<a id="2-9-1-2020-05-12"></a>
### 2.9.1 (May 12, 2020) { #2-9-1-2020-05-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Android.zip)

<a id="91-may-12-2020-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.9.1
	* (Android) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error 

<a id="2-9-0-2020-04-28"></a>
### 2.9.0 (April 28, 2020) { #2-9-0-2020-04-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Android.zip)

<a id="90-april-28-2020-more-features"></a>
#### More Features

* Suspension of Membership Withdrawal 
	* [SDK] 2.9.0
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

<a id="90-april-28-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.9.0
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated PAYCO Login SDK: Android(v1.5.0), iOS(v1.4.0)

<a id="2-8-1-2020-04-14"></a>
### 2.8.1 (April 14, 2020) { #2-8-1-2020-04-14 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Android.zip)

<a id="81-april-14-2020-feature-updates"></a>
#### Feature Updates 

* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	* (Android) Modified codes that may cause crashes after process restarts
	
<a id="2-8-0-2020-03-24"></a>
### 2.8.0 (March 24, 2020) { #2-8-0-2020-03-24 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Android.zip)

<a id="80-march-24-2020-more-features"></a>
#### More Features 

* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 

<a id="80-march-24-2020-feature-updates"></a>
#### Feature Updates 

* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 
	
<a id="2-7-2-2020-03-10"></a>
### 2.7.2 (March 10, 2020) { #2-7-2-2020-03-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Android.zip)

<a id="72-march-10-2020-feature-updates"></a>
#### Feature Updates

* [SDK] 2.7.2 
      * Fixed codes where a crash could occur during ToastLogger initialization while Gamebase initializes
      * Updated server version to v1.2.1.

<a id="2-7-1-2020-02-25"></a>
### 2.7.1 (February 25, 2020) { #2-7-1-2020-02-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-Android.zip)

<a id="71-february-25-2020-feature-updates"></a>
#### Feature Updates 

* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called

<a id="2-7-0-2020-01-21"></a>
### 2.7.0 (January 21, 2020) { #2-7-0-2020-01-21 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Android.zip)

<a id="70-january-21-2020-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 

<a id="2-6-2-2019-12-24"></a>
### 2.6.2 (December 24, 2019) { #2-6-2-2019-12-24 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Android.zip)

<a id="62-december-24-2019-feature-updates"></a>
#### Feature Updates

* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)

<a id="2-6-1-2019-12-10"></a>
### 2.6.1 (December 10, 2019) { #2-6-1-2019-12-10 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Android.zip)

<a id="61-december-10-2019-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize() 
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address 
  * (Android) Fixed crash occurrence when IAP is not enabled  

<a id="2-6-0-2019-11-12"></a>
### 2.6.0 (November 12, 2019) { #2-6-0-2019-11-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Android.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

<a id="60-november-12-2019-more-features"></a>
#### More Features 

* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (Android) Added the payment feature for Google subscription  
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase. 

<a id="2-5-0-2019-08-27"></a>
### 2.5.0 (August 27, 2019) { #2-5-0-2019-08-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Android.zip)

<a id="50-august-27-2019-more-features"></a>
#### More Features 

* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
<a id="2-4-4-2019-07-23"></a>
### 2.4.4 (July 23, 2019) { #2-4-4-2019-07-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Android.zip)

<a id="44-july-23-2019-feature-updates"></a>
#### Feature Updates

* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)

<a id="2-4-2-2019-06-25"></a>
### 2.4.2 (June 25, 2019) { #2-4-2-2019-06-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Android.zip)

<a id="42-june-25-2019-features-updateschanges"></a>
#### Features Updates/Changes

* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

<a id="42-june-25-2019-bug-fixes"></a>
#### Bug Fixes

* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 
	
<a id="2-4-0-2019-05-28"></a>
### 2.4.0 (May 28, 2019) { #2-4-0-2019-05-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Android.zip)

<a id="40-may-28-2019-feature-updateschanges"></a>
#### Feature Updates/Changes

* [SDK] 2.4.0
  * (Common) Change of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]

    * (Android) NAVER SDK Version Updated (v4.2.5): Bug of NAVER SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while NAVER login was underway)  

<a id="2-3-1-2019-05-16"></a>
### 2.3.1 (2019. 05. 16.) { #2-3-1-2019-05-16 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.1/GamebaseSDK-Android.zip)

<a id="31-20190516-1"></a>
#### 버그수정

* [SDK] 2.3.1
    * (Android) Fixed an issue where Twitter login did not work in version 2.3.0

<a id="2-3-0-2019-04-23"></a>
### 2.3.0 (2019. 04. 23.) { #2-3-0-2019-04-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Android.zip)
    
```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

<a id="30-20190423-1"></a>
#### 기능 추가

* [SDK] 2.3.0
    * (Android/Unity) Added Chinese store authentication/payment

<a id="30-20190423-2"></a>
#### 기능 개선/변경

* [SDK] 2.3.0
    * (Common) Added Launching Status Codes: "Under Review (204)", "Under Testing (203)"
    * (Android) Fixed an issue where AuthToken was being deleted upon receiving login failures via the most recently logged-in provider or WebSocket response failures (timeout, network disabled, etc.)
    * (Android) Fixed a memory leak that occurred inside AuthAdapter during IdP login

<a id="2-2-2-2019-04-11"></a>
### 2.2.2 (2019. 04. 11.) { #2-2-2-2019-04-11 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Android.zip)

<a id="22-20190411-1"></a>
#### 버그수정

* [SDK] 2.2.2
    * (Android) Fixed an issue where the callback was not received when calling the TransferAccount API before Gamebase initialization

<a id="2-2-0-2019-03-26"></a>
### 2.2.0 (2019. 03. 26.) { #2-2-0-2019-03-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Android.zip)

<a id="20-20190326-1"></a>
#### 기능 추가

* Added the TransferAccount feature: a feature that allows guest users to transfer to a new device using up to 2 keys without Mapping
    * (Common SDK) Added APIs
        * TransferAccountInfo issue API (issueTransferAccount)
        * An API that requests account transfer using the issued TransferAccountInfo (transferAccountWithIdPLogin)
        * API to check the issued TransferAccountInfo (queryTransferAccount)
        * Added an API for renewing already-issued TransferAccountInfo (renewTransferAccount)
* Added a feature for force Mapping: a feature to map an IdP account that is already linked to another account
    * (SDK Common) Added APIs
        * API for force mapping (addMappingForcibly)

<a id="20-20190326-2"></a>
#### 기능 개선/변경

* [SDK] 2.2.0
    * (Android) Updated IAP SDK to the latest version, v1.5.3

<a id="2-1-0-2019-02-26"></a>
### 2.1.0 (2019. 02. 26.) { #2-1-0-2019-02-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Android.zip)

<a id="10-20190226-1"></a>
#### 기능 개선/변경

* [SDK] 2.1.0
    * (Common) Removed TransferKey API
        * issueTransferKey : Issue TransferKey
        * requestTransfer : TransferKey Verification
        
<a id="10-20190226-2"></a>
#### 버그수정

* [SDK] 2.1.0
    * (Android) Fixed a bug where onActivityResult() was called before Gamebase initialization, causing abnormal behavior

<a id="2-0-0-2019-01-29"></a>
### 2.0.0 (2019. 01. 29.) { #2-0-0-2019-01-29 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Android.zip)

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

<a id="00-20190129-1"></a>
#### 기능 추가

* [SDK] 2.0.0
    * (Common) Added API for custom indicators (automatically sent from within the SDK when a purchase is successful)
        * setGameUserData : Sends user level information after game login
        * traceLevelUpData : Call when the game user levels up to track level-up


<a id="00-20190129-2"></a>
#### 기능 개선/변경

* [SDK] 2.0.0
    * (Android) Push SDK update (android:1.7.0)
    * (Android)Changed the Adapter API
        * Pass launching information
        * Added Callback to logout and withdraw APIs

<a id="1-14-5-2018-12-27"></a>
### 1.14.5 (2018. 12. 27.) { #1-14-5-2018-12-27 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.5/GamebaseSDK-Android.zip)

<a id="145-20181227-1"></a>
#### 기능 개선/변경

* [SDK] 1.14.5
    * The following APIs that were deprecated have been removed.
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
    * Payment module (gamebase-adapter-purchase-iap) Modified.
        * Updated to IAP SDK 1.5.2
        * Removed IAP TEST Store not in use from Client
        * Fixed an issue where the call fails in the payment retry transaction logic (requestRetryTransaction) when data is incomplete
        * Added exception handling to all IAP SDK call sites to prevent crashes

<a id="1-14-2-2018-11-15"></a>
### 1.14.2 (2018. 11. 15.) { #1-14-2-2018-11-15 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Android.zip)

<a id="142-20181115-1"></a>
#### 기능 개선/변경

* [SDK] 1.14.2
    * (Android) Changed the type of epoch time, which indicates the maintenance start/end time in the data structure, from String to long: Fixed an issue where the callback was not returned due to a type mismatch when calling maintenance after integrating with the existing Gamebase Unity.

<a id="142-20181115-2"></a>
#### 버그수정

* [SDK] 1.14.2
    * (Android) Fixed a crash bug caused by the store not being checked when "installing/updating an app" in an emulator environment without store apps (PlayStore, OneStore, etc.)
    
<a id="1-14-1-2018-10-23"></a>
### 1.14.1 (2018. 10. 23.) { #1-14-1-2018-10-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.1/GamebaseSDK-Android.zip)

<a id="141-20181023-1"></a>
#### 기능 추가

* [SDK] 1.14.0
    * (Common) Added a file attachment feature in Gamebase WebView : Does not work properly on Android API 19, KitKat.
    
<a id="141-20181023-2"></a>
#### 기능 개선/변경

* [SDK] 1.14.0
    * (Common) Updated to URL-encode messages written by users in the Console for ban/maintenance and decode them on the client side for processing
    * Remove API : Webview, Network, Launching
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)        
    * Deprecated  API 
        * (void)Gamebase.WebView.showWebView(Activity, String)
        * (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
    
<a id="141-20181023-3"></a>
#### 버그수정

* [SDK] 1.14.1
    * (Android) Fixed a bug where calling the Auth API again in a Callback after calling the Auth API did not work properly
    
<a id="1-13-0-2018-09-13"></a>
### 1.13.0 (2018. 09. 13.) { #1-13-0-2018-09-13 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Android.zip)

<a id="130-20180913-1"></a>
#### 기능 개선/변경

* [SDK] 1.13.0
    * (Common) Applied the latest version of IAP SDK (android:1.5.1, iOS:1.6.0)
    * (Android)Improved error messages for Push API call failures to be clearer based on the Gamebase initialization/login status
        * Call before initialization: NOT_INITIALIZED(1)
        * When called after initialization, the Push module is not available: NOT_SUPPORTED(10)
        * Call before initialization succeeds and before login : NOT_LOGGED_IN(2)
    
<a id="130-20180913-2"></a>
#### 버그수정

* [SDK] 1.13.0
    * (Android) Fixed an error that occurred during NAVER login due to a conflict with the NaverCafe SDK
        
<a id="1-12-2-2018-08-28"></a>
### 1.12.2 (2018. 08. 28.) { #1-12-2-2018-08-28 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-Android.zip)

<a id="122-20180828-1"></a>
#### 기능 개선/변경

* [SDK] 1.12.2
    * (Android) Added a defensive logic to handle a bug that could cause a crash when a WebSocket timeout occurs (API call time elapsed)
    
<a id="122-20180828-2"></a>
#### 버그수정

* [SDK] 1.12.2
    * (Android) Fixed an issue where an initialization error occurs when building with TargetSdk 28 while including auth-twitter-adapter

<a id="1-12-1-2018-08-09"></a>
### 1.12.1 (2018. 08. 09.) { #1-12-1-2018-08-09 }

<a id="121-20180809-1"></a>
#### 기능 개선/변경

* [SDK] 1.12.1
    * (Common) Applied the latest version of IAP SDK (1.5.0)
    * (Common) Improved the Gamebase maintenance page to display the maintenance time according to the country time set on the device
    * (Common) Added a feature to use the maintenance information entered in the Console when using an external page as the maintenance page
    * (Common) An error now occurs when a user with an IdP mapping attempts to add a Guest mapping (TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (Common) Error occurs when calling authentication API in duplicate (AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (Android) Updated TencentPush SDK (3.2.3)
    * (Android)Supports Onestore v17 (API v5): Gamebase does not support v16 (store code = TS).

<a id="1-11-1-2018-07-05"></a>
### 1.11.1 (2018. 07. 05.) { #1-11-1-2018-07-05 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-Android.zip)

<a id="111-20180705-1"></a>
#### 기능 개선/변경

* [SDK] 1.11.1
    * (Common) Changed so that when AddMapping succeeds after a Guest login, calling loginForLastLoggedInProvider will log in using the IdP account for which AddMapping succeeded
    
<a id="111-20180705-2"></a>
#### 버그수정

* [SDK] 1.11.1
    * (Common) Fixed a bug where subsequent API calls (login/push/purchase, etc.) did not proceed after maintenance was lifted
    * (Android) Fixed a bug where the type of ObserverMessage.data.code is String instead of int when ObserverMessage is received through Gamebase.addObserver()


<a id="1-11-0-2018-06-26"></a>
### 1.11.0 (2018. 06. 26.) { #1-11-0-2018-06-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Android.zip)

<a id="110-20180626-1"></a>
#### 기능 추가

* Added Twitter IdP: Android, iOS
* Added LINE IdP: Android only. iOS support is scheduled for July 2018.
    
<a id="110-20180626-2"></a>
#### 기능 개선/변경

* [SDK] 1.11.0
    * (Common) Added Japanese translation for LocalizedString
    * (Common) Improved internal logic to clearly distinguish error codes when Initialization or login has not been performed before calling the Authenticate API
    * (Android) Removed the `android.permission.READ_PHONE_STATE` permission
    * (Android) Changed to allow the required configuration values setAppId and setAppVersion in GamebaseConfiguration.Builder to be entered in the constructor
    * (Android) Removed the setServerApiVersion API from GamebaseConfiguration.Builder
    * (Android) Changed the name of the getAuthBanInfo() API and class AuthBanInfo to getBanInfo() and class BanInfo

<a id="1-9-0-2018-05-03"></a>
### 1.9.0 (2018. 05. 03.) { #1-9-0-2018-05-03 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Android.zip)

<a id="90-20180503-1"></a>
#### 기능 추가

* Added a Transfer feature
    * Added a feature to allow guest users to transfer to a new device without mapping
    * (Common) Added APIs
        * Added an API to issue a transfer key (IssueTransferKey)
        * Use the issued TransferKey to request account transfer via the API (RequestTransfer)

<a id="90-20180503-2"></a>
#### 버그 수정

* [SDK] 1.9.0
    * (Android) Fixed an issue where the ban popup window did not appear when a user was determined to be invalid in Heartbeat (fixed with the same logic as iOS)

<a id="1-8-1-2018-04-12"></a>
### 1.8.1 (2018. 04. 12.) { #1-8-1-2018-04-12 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Android.zip)

<a id="81-20180412-1"></a>
#### 버그 수정

* [SDK] 1.8.1
    * (Android. iOS) Fixed a bug where registerPush fails when displayLanguageCode is passed as null

<a id="1-8-0-2018-04-05"></a>
### 1.8.0 (2018. 04. 05.) { #1-8-0-2018-04-05 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Android.zip)

<a id="80-20180405-1"></a>
#### 기능 추가

* Added a Kick out feature
    * Added a feature to disconnect all users currently in the game (can be used when you want to disconnect all users from the game during maintenance)
    * (SDK Common) Added an API to receive kick out events
* Added Observer feature development and APIs
    * (SDK common) Added an API to handle all changes to app status/network status/user status (ban) — such as maintenance — through Observer registration in a batch

<a id="80-20180405-2"></a>
#### 기능 개선/변경

* [SDK] 1.8.0
    * (Common) The following APIs have been deprecated due to the addition of the Observer feature: LaunchingStatus Listener, Network Listener (existing users can continue to use them)

<a id="1-7-0-2018-02-22"></a>
### 1.7.0 (2018. 02. 22.) { #1-7-0-2018-02-22 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Android.zip)

<a id="70-20180222-1"></a>
#### 기능 추가

* [SDK] 1.7.0
    * Added NAVER IdP authentication
    * Added Display Language settings: Added Display Language to allow you to set the language displayed to game users in the game separately from the device language.

<a id="1-5-0-2017-12-21"></a>
### 1.5.0 (2017. 12. 21.) { #1-5-0-2017-12-21 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Android.zip)
<a id="50-20171221-1"></a>
#### 기능 추가

* [SDK] 1.5.0
    * Added a Close Callback that occurs when the WebView is closed
    * Added a feature to receive events from Custom Schemes used in WebView

<a id="1-4-0-2017-11-23"></a>
### 1.4.0 (2017. 11. 23.) { #1-4-0-2017-11-23 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Android.zip)

<a id="40-20171123-1"></a>
#### 버그 수정

* [SDK] 1.4.0 update
    * (Android) Fixed an error where ban information was returned as null when not using the Gamebase-provided popup window

<a id="1-3-0-2017-10-26"></a>
### 1.3.0 (2017. 10. 26.) { #1-3-0-2017-10-26 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Android.zip)

<a id="30-20171026-1"></a>
#### 기능 추가

* [SDK] 1.3.0 update
    * Added the AddMapping API using Credential

<a id="1-2-0-2017-09-21"></a>
### 1.2.0 (2017. 09. 21.) { #1-2-0-2017-09-21 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Android.zip)

<a id="20-20170921-1"></a>
#### 기능 추가

* Added a ban (user penalty) feature
* [SDK] 1.2.0 update
    * Display a popup for suspended users


<a id="1-1-5-2017-07-20"></a>
### 1.1.5 (2017. 07. 20.) { #1-1-5-2017-07-20 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Android.zip)

<a id="15-20170720-1"></a>
#### 기능 개선/변경

* [SDK] 1.1.5 update
    * Added system popup API (showAlertWithTitle)
    * Changed to return the country code in uppercase letters (Android)
    * Updated to TCPush SDK 1.4.1
    * Updated to IAP SDK 1.3.3.20170627

<a id="1-1-4-2017-05-25"></a>
### 1.1.4 (2017. 05. 25.) { #1-1-4-2017-05-25 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Android.zip)
<a id="14-20170525-1"></a>
#### 기능 개선/변경

* [SDK] 1.1.4 update
    * Provided APIs to change the payment store at runtime
    * (Android) Applied TCPushSdk v1.4, Tencent Push feature provided

<a id="1-1-3-2017-04-20"></a>
### 1.1.3 (2017. 04. 20.) { #1-1-3-2017-04-20 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.3/GamebaseSDK-Android.zip)
<a id="13-20170420-1"></a>
#### 기능 개선/변경

* [SDK] 1.1.3 update
    * (Android) Improved launch structure and popup/maintenance page: Added the feature to configure custom maintenance pages
    * (Android) Improved authentication structure and added logging: outputs authentication adapter and SDK version logs

<a id="13-20170420-2"></a>
#### 버그 수정

* [SDK] 1.1.3 update
    * (Android) Fixed a crash issue that occurred during initialization with Facebook SDK v4.19.0 or later


<a id="1-1-2-2017-04-04"></a>
### 1.1.2 (2017. 04. 04.) { #1-1-2-2017-04-04 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Android.zip)

<a id="12-20170404-1"></a>
#### 기능 개선/변경

* [SDK] 1.1.2 update
    * Improved the maintenance check and emergency notice popup window displayed at game launch

<a id="1-1-0-2017-03-21"></a>
### 1.1.0 (2017. 03. 21.) { #1-1-0-2017-03-21 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Android.zip)

<a id="10-20170321-1"></a>
#### 기능 개선/변경

* [SDK] 1.1.0 update
    * Added an interface that receives an external AccessToken and performs idPLogin
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

<a id="1-0-0-2017-03-09"></a>
### 1.0.0 (2017. 03. 09.) { #1-0-0-2017-03-09 }

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Android.zip)

<a id="00-20170309-1"></a>
#### 신규 상품 출시

* It is a service that provides commonly required features for games, helping developers build games easily and efficiently.
    * Supports various authentications: Guest, 3rd party (Google, Facebook, Game Center, etc.)
    * Provided logout and membership withdrawal features
    * Provides a mapping feature that allows a single user to use multiple external IDPs simultaneously
    * Provides game app status management, maintenance, and emergency notice features for game operations via the web console
    * Provides a web Console screen for checking real-time operational metrics
    * Integrated with TOAST Cloud products: PUSH, IAP
