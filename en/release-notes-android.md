## Game > Gamebase > Release Notes > Android

### 2.73.0 (2025. 07. 15.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.73.0/GamebaseSDK-Android.zip)

```
최소 지원 버전이 Android 5.1 이상으로 상향되었습니다.(minSdk 21 → 22)
Android Gradle Plugin 최소 버전이 7.4.2 이상으로 상향되었습니다.(4.0.1 -> 7.4.2)
```

#### 기능 개선/변경

* 내부 로직 개선

#### 버그 수정

* 로그인 웹뷰에서 화면 회전시 여백 크기를 잘못 계산하는 오류를 수정했습니다.

### 2.72.0 (2025. 06. 24.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.72.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경

* 웹소켓 모듈이 중복 호출되는 경우 ArrayIndexOutOfBoundsException이 발생할 수 있는 로직을 수정했습니다.
    * 이 문제는 Gamebase Android SDK 2.71.2에서만 발생합니다.

#### 버그 수정

* LINE IdP의 Region 추가 정보 대응이 되지 않아 매핑 관련 동작에서 발생하는 문제를 수정했습니다.
    * Gamebase.login("guest") -&gt; Gamebase.addMapping("line") -&gt; Gamebase.loginForLastLoggedInProvider() 호출 실패 이슈
    * Gamebase.login(idp) -&gt; Gamebase.addMapping("line") -&gt; AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER(3302) -&gt; Gamebase.changeLogin(ForcingMappingTicket) 호출 실패 이슈
    * Gamebase.login("line") -&gt; Gamebase.addMapping(idP) -&gt; AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER(3302) -&gt; Gamebase.changeLogin(ForcingMappingTicket) 호출 실패 이슈

### 2.71.2 (2025. 05. 20.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경

* 외부 SDK 업데이트: Hangame Android SDK(1.17.2)
* 구버전 Google Play Service가 설치된 단말기에서 Sign-in with Google 로그인 지원
* 내부 로직 개선

### 2.71.1 (2025. 04. 29.)

[SDK Download](https://static.toaㄴㄴstoven.net/toastcloud/sdk_download/gamebase/v2.71.1/GamebaseSDK-Android.zip)

#### 버그 수정

* 웹뷰 크기 계산 관련 오류를 수정하였습니다.

### 2.71.0 (2025. 04. 15.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.71.0/GamebaseSDK-Android.zip)

#### 기능 추가

* '게임 공지' 신규 기능이 추가되었습니다.
    * Gamebase.GameNotice.openGameNotice(Activity activity, GamebaseCallback onCloseCallback);
    * API 호출 방법은 다음 가이드 문서를 참고하시기 바랍니다.
        * [Game > Gamebase > Android SDK 사용 가이드 > UI > GameNotice](./aos-ui/#gamenotice)

#### 기능 개선/변경

* storeCode를 null로 설정하여 Gamebase 초기화를 호출했을 때 예외가 발생하는 대신 **INVALID_PARAMETER(3)** 에러를 리턴하도록 동작을 변경했습니다.

### 2.70.1 (2025. 03. 13.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.1/GamebaseSDK-Android.zip)

#### 버그 수정

* Apple ID, Steam, Twitter로그인 네비게이션 바의 X버튼 사이즈를 재조정하였습니다.
* Kotlin 파일에서 AuthProvider의 IdP constant(예. AuthProvider.GUEST 등)를 참조할 수 없는 이슈를 수정하였습니다.

### 2.70.0 (2025. 03. 11.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.70.0/GamebaseSDK-Android.zip)

#### 기능 추가

* 외부 SDK 업데이트: NHN Cloud SDK(1.9.5)
    * Google billing client version 7.1.1이 적용되었습니다.
    * NHN Cloud Android SDK 1.9.5에서는 Android 7.0(API Level 24) 미만 단말기에서 결제를 시도하는 경우 크래시가 발생합니다.
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
* 'GPGS 자동 로그인' 기능 연동시 유저에게 GPGS 로그인을 앱 설치 후 한번만 물어보는 초기화 옵션을 추가했습니다.
    * **GamebaseConfiguration.Builder.enableGPGSSignInCheck(boolean)**
    * 기본 설정은 true로, 유저가 GPGS 로그인을 거부하더라도 Gamebase 초기화 때 GPGS 로그인 창을 다시 표시합니다.
    * false로 설정하면 앱 최초 실행시에만 GPGS 로그인 창이 한번 표시됩니다.
* 로그인 시 IdP 서버로부터 에러가 발생했음을 나타내는 신규 에러 코드가 추가되었습니다.
    * AUTH_AUTHENTICATION_SERVER_ERROR(3012)
* GamebaseWebView에 네비게이션 바 title 컬러와 icon tint 컬러 설정 옵션을 추가했습니다.
    * **GamebaseWebViewConfiguration.Builder.setNavigationBarTitleColor(int)**
    * **GamebaseWebViewConfiguration.Builder.setNavigationBarIconTintColor(int)**

#### 기능 개선/변경

* 'GPGS 자동 로그인' 기능 연동시 유저가 GPGS 로그인을 하지 않으면 Gamebase 초기화, 로그인, 로그아웃 시 GPGS 로그인을 계속 시도하던 동작을 Gamebase 초기화 때만 시도하도록 변경했습니다.
* Apple ID, Steam, Twitter로그인 네비게이션 바에 title과 같은 색으로 X버튼을 표시하도록 변경했습니다.

#### 버그 수정

* LaunchingInfo data가 유저 Event Handler에서 업데이트 되지 않는 이슈를 수정했습니다.
* Unity 빌드에서 이미지 공지 비율이 원본 이미지 비율과 다르게 표시되는 문제를 수정했습니다.

### 2.69.0 (2025. 01. 21.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.69.0/GamebaseSDK-Android.zip)

#### 기능 추가

* **Gamebase.requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) 비동기 API**를 추가했습니다.
    * **Gamebase.getLastLoggedInProvider() 동기 API**가 타이밍 상 정상적인 값을 반환하지 못할 때가 있습니다.
    * **gamebase-adapter-auth-gpgs-autologin** 모듈을 빌드에 포함하는 경우 GPGS 서버에서 데이터를 획득하는 시간이 필요하므로 Gamebase 초기화 직후 getLastLoggedInProvider() 동기 API를 호출하면 정상적인 값을 획득할 수 없습니다.
    * 이때 requestLastLoggedInProvider(GamebaseDataCallback&lt;String&gt;) 비동기 API는 정확한 값을 보장합니다.
    * gamebase-adapter-auth-gpgs-autologin 모듈이 빌드에 포함되지 않은 경우에는 계속해서 getLastLoggedInProvider() 동기 API를 사용해도 무방합니다.
* **GamebaseWebViewConfiguration.Builder.setCutoutAreaColor() API**를 추가했습니다.
    * GamebaseWebView의 **GamebaseWebViewConfiguration.Builder.renderOutsideSafeArea() API**를 **false**로 설정한 경우, cutout 영역에 자동으로 padding 여백을 추가합니다.
    * setCutoutAreaColor()는 이렇게 추가된 padding 영역의 색을 설정할 수 있습니다.
    * renderOutsideSafeArea()를 false로 설정했지만 setCutoutAreaColor()는 설정하지 않는 경우에는 웹 페이지 'body'의 'background-color' 값으로 자동으로 padding 영역의 색상을 결정합니다.

#### 기능 개선/변경

* **gamebase-adapter-auth-gpgs-autologin** 모듈을 빌드에 포함하는 경우 Gamebase 초기화와 동시에 **Gamebase.getLastLoggedInProvider() 동기 API**를 호출하면 내부 데이터가 초기화가 완료되지 않아 null이 반환되었으나, 이 경우 **'NOT\_INITIALIZED\_YET'** 이라는 문자열을 반환하도록 내부 로직을 변경했습니다.
* **GamebaseWebViewConfiguration.Builder.renderOutsideSafeArea() API**를 **false**로 설정한 경우에도 cutout 영역까지 웹뷰를 모두 표시하도록(**edge-to-edge**) 내부 로직을 변경했습니다.
    * 그 대신 자동으로 padding 여백을 추가하여 컨텐츠가 가려지지 않도록 했습니다.

#### 버그 수정
 
* 로그인 전에 Gamebase.Push.getNotificationOptions() API를 호출하는 경우 크래시가 발생하지 않도록 수정했습니다.
* Loading Progress가 간헐적으로 사라지지 않거나 크래시가 발생하는 이슈에 대한 방어코드를 추가했습니다.
* WebSocket에서 간헐적으로 내부 콜백 함수가 중복으로 호출되어 발생하는 크래시에 대한 방어코드를 추가했습니다.

### 2.68.0 (2024. 11. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.68.0/GamebaseSDK-Android.zip)

```
Raised the minimum supported version to Android 5.0 or later. (minSdk 19 -> 21)
```

#### Added Features
* Added auto sign-in integration with Google Play Games Services accounts.
    * To enable this feature, you must add the **gamebase-adapter-auth-gpgs-autologin** module declaration to your build dependencies.

            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-auth-gpgs-autologin:$GAMEBASE_SDK_VERSION"
            }
            
    * You can also refer to the following guides to set up additional information
        * [Game > Gamebase > Android SDK User Guide > Get Started > Setting > AndroidManifest.xml > GPGS IdP](./aos-started/#gpgs-idp)

#### Feature Updates
* External SDK update: Hangame Android SDK(1.17.0)
* Updated Google authentication libraries.
    * Google Sign-In for Android has been deprecated and switched to Google Credential Manager.
    * Authentication method changed from AuthCode to OIDC token.
* Fixed webview not redirecting URLs when a registered custom scheme is matched.

### 2.67.0 (2024. 10. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.67.0/GamebaseSDK-Android.zip)

#### Added Features
* Added Steam authentication adapter.

#### Feature Updates
* External SDK update: NHN Cloud SDK(1.9.3)
* Twitter has changed its authentication method to OAuth 2.0, so login will not work without changing the settings below.
    * Issue OAuth 2.0 Client ID and Client Secret
        * Create an OAuth 2.0 Client ID and Client Secret in the Twitter Developer Portal, then register in the Gamebase console.
    * Callback URL Settings
        * Set the Callback URL (https://id-gamebase.toast.com/oauth/callback) in the Gamebase console.
        * Add the same Callback URL to the Twitter Developer Portal.
    * For more information, see the following link.
        * [Game > Gamebase > Consolue User Guide > App > Authentication Information](./oper-app/#authentication-information)

#### Bug Fixes
* Fixed an issue where touching Detail after disconnecting from the network while the terms screen was exposed would cause the terms popup to exit.

### 2.66.3 (2024. 09. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.3/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud SDK(1.9.2)
    * Fixed an issue where Native Crash logs are intermittently not reported on Android 13 and above devices.
    * Improved Amazon payment reprocessing.

### 2.66.2 (2024. 08. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.2/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud SDK(1.9.1), Kakaogame SDK(3.19.3), PAYCO SDK(1.5.15)
* Added supplemental logic to ensure that when a problem occurs with an Amazon store checkout and reprocessing is triggered, the item is awarded to the User ID that first attempted the payment.
* Changed the color and name of Twitter login title bar.
* Fixed a failure callback to be called instead of the previous success callback when an error occurs inside the webview of a rolling image announcement.

#### Bug Fixes
* Fixed an issue where, when an Activity is destroyed, the WebView floating on top of the destroyed Activity is closed and the close event callback is missing.
* Added defensive logic to prevent the Hangame Login Adapter from causing an already resumed error if a duplicate callback is received when logging into an external idP.

### 2.66.1 (2024. 07. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed an issue where the `gamebase://dismiss` scheme did not work on Android 14 devices when built with targetSdk 34, preventing custom schemes from exiting the webview.

### 2.66.0 (2024. 07. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.66.0/GamebaseSDK-Android.zip)

#### Added Features
* Added GPGS v2 authentication
    * For more details on how to set, see the following document.
        * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > GPGS IdP](./aos-started/#gpgs-idp)

### 2.65.1 (2024. 06. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.1/GamebaseSDK-Android.zip)

#### Feature Updates
* Fixed so that if there are no images to show on a particular client, a success callback is called instead of an error.

#### Bug Fixes  
* Fixed an error where, when an empty image notice is exposed if there were no registered image notices, a crash occurs on closing after checking the Show less for today.

### 2.65.0 (2024. 06. 11.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.65.0/GamebaseSDK-Android.zip)

#### Added Features
* Added a new type to the image notice feature.
    * Added the `Rolling Popup` type.
    * Displays the existing image notice as the `Individual Popup` type.

#### Feature Updates
* External SDK update: NHN Cloud SDK(1.9.0), Hangame Android SDK(1.13.0)
    * Applied Google billing client version 6.2.1.
    * Additional settings are required to make payments on Android OS 4.4 (API Level 19) devices.
        * For more information, see [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > Gradle > Root level build.gradle](./aos-started/#root-level-buildgradle).
* Improved internal logic

### 2.64.0 (2024. 05. 28.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.64.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Kakaogame SDK (3.19.0), PAYCO SDK (1.5.14)
* Changed so that the back key does not run when the Terms and Conditions popup appears.

#### Bug Fixes
* Fixed a bug where Gamebase internal messages do not appear correctly due to string resource reference failures on devices below API Level 23 (OS 6.0, M).

### 2.63.0 (2024. 04. 23.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.63.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Improved internal logic

### 2.62.1 (2024. 03. 29.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed a bug where the Gamebase.loginForLastLoggedInProvider call would always fail on devices below Android 7.0 (API Level 24) and the Guest account would be lost. 
    * This bug only occurs in Gamebase Android SDK 2.62.0.

### 2.62.0 (2024. 03. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.62.0/GamebaseSDK-Android.zip)

#### Feature Updates
*  Added a testDevice field to the LaunchingInfo VO returned after Gamebase initialization to indicate that it is a test device.

#### Feature Updates
* External SDK update: Hangame Android SDK(1.9.0)
* Improved internal logic so that Preference cannot be copied for use.
* Incorporated the gamebase-sdk-base module into a single gamebase-sdk module.

### 2.61.0 (2024. 02. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.61.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud SDK(1.8.4)
* Added a login method with Twitter callback URL.
* Added a declaration to the AndroidManifest to enable the use of Photo Picker, which does not require permission, when uploading photos to the Customer Center. Accordingly, the runtime permission request for READ_EXTERNAL_STORAGE has been removed.
* Improved internal logic

### 2.60.0 (2024. 01. 23.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.60.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: PAYCO Android SDK (1.5.13)
* Moved the queries declaration required when using the ONE store adapter inside the SDK.
* Improved internal logic

#### Bug Fixes
* Fixed an issue where ConcurrentModificationException exception occurs intermittenly when running the app.

### 2.59.0 (2023. 12. 19.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.59.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Hangame Android SDK (1.7.2)
* Improved internal logic

#### Bug Fixes
* Fixed an issue where .wav format files could not be uploaded in the Customer Center.

### 2.58.0 (2023. 11. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.58.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Kakaogame version update (3.17.5)
* Updated Twitter Adapter minSDK to 21 due to Twitter API server certificate renewal
* Improved internal logic

#### Bug Fixes
* Added a defense code to prevent a crash when an empty string is entered in the message of the Gamebase.Logger.report(String message, ...) API.

### 2.57.0 (2023. 10. 31.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.57.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Naver Login Android SDK(5.8.0)

#### Added Featrues
* Added a new API to send exceptions to Log & Crash.

        Gamebase.Logger.report(String message, Throwable throwable);
        Gamebase.Logger.report(String message, Throwable throwable, Map<String, String> userFields);

#### Bug Fixes
* Fixed a bug where an EmptyStackException would rarely occur when Gamebase WebView close().

### 2.56.1 (2023. 10. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.1/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud Android SDK (1.8.0)
    * Google billing client version 5.2.1 has been applied.
    * When new or app updates are made to the Google Play Store after 2023/11/01, it is necessary to apply the corresponding version. For more information, please refer to the following link.
    * [Google Play Billing Library version deprecation](https://developer.android.com/google/play/billing/deprecation-faq)

### 2.56.0 (2023. 09. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.56.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Hangame Android SDK (1.7.1)

### 2.55.0 (2023. 09. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.55.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Naver Login Android SDK(5.7.0), NHN Cloud Android SDK(1.7.1)
* Fixed a cross-app scripting vulnerability in the OAuthLoginInAppBrowserActivity in older versions of the Naver Login SDK.
* Added a defense logic to prevent crashes when using Naver IdP on devices below API 21, which are not supported by Naver IdP.

#### Bug Fixes
* Fixed an issue where the loading animation off is not applied when idP Login.
* Fixed an issue where the navigation bar reappears when the windowFocus is changed in API Level 28, 29 fullscreen webview.
* Added a defensive logic to prevent crashing if Weibo login is successful but access token is returned as null from Weibo SDK intermittently.

### 2.53.0 (2023. 08. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.53.0/GamebaseSDK-Android.zip)

#### Added Features
* Added a new API to specify an option that hides the loading animation when calling loginForLastLoggedInProvider.
    * Gamebase.loginForLastLoggedInProvider(Activity activity, Map&lt;String, Object&gt; additionalInfo, GamebaseDataCallback&lt;AuthToken&gt; callback);
    * For more details on how to call API, see the following documents.
        * [Game > Gamebase > Android SDK User Guide > Authentication > Login > Login Flow > Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp)

#### Feature Updates
* External SDK update: Facebook Android SDK(16.1.2), Line Android SDK(5.8.1), Weibo Android SDK(13.5.0)
* Improved so that, when attaching files in the Customer Center Webview, permissions are automatically acquired according to albums, cameras, storage types, and run the right feature for the type.
    * To use the enhanced file attachment feature in the Customer Center, you need to add permission settings to the AndroidManifest.xml by following the guide below.
    * [Game > Gamebase > Android SDK User Guide > Getting Started > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)

### 2.52.1 (2023. 07. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK version changed: OkHttp 3.12.13 (downgraded from 4.10.0)

#### Bug Fixes
* Fixed an issue where a crash occurs on Android 4.4 (OS 19 Kitkat) devices due to the mimum supported OS version raised to 21 starting from OkHttp 3.13.

### 2.52.0 (2023. 06. 27.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.0/GamebaseSDK-Android.zip)

#### Added Features
* Added ONE store v21 Adapter.
* Added custom push receiver with the feature to suppress notifications with certain messages.
    * To enable this feature, add the **gamebase-adapter-push-notification** module declaration to your build dependencies.
    
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-push-notification:$GAMEBASE_SDK_VERSION"
            }

#### Feature Updates
* External SDK update: NHN Cloud SDK 1.6.0

#### Bug Fixes
* Fixed a bug where the navigation bar and X button overlapps in horisontal mode of Render outside safe area.
* Fixed the Terms and Conditions details page that appears when you click "Detail" in the Terms and Conditions window to not be clickable in the background until it finishes loading.

### 2.50.1 (2023. 07. 17.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.52.1/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK version changed: OkHttp 3.12.13 (downgraded from 4.10.0)

#### Bug Fixes
* Fixed an issue where a crash occurs on Android 4.4 (OS 19 Kitkat) devices due to the mimum supported OS version raised to 21 starting from OkHttp 3.13.

### 2.50.0 (2023. 05. 16.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.50.0/GamebaseSDK-Android.zip)

#### Added Features
* Added MyCard Adapter.

#### Feature Updates
* External SDK update: NHN Cloud Android SDK 1.5.0, Gson 2.8.9, OkHttp 4.10.0, PAYCO Android SDK 1.5.12

#### Bug Fixes
* Fixed an error where, when calling the Terms and Conditions API, Activity size is reduced within a safe area.

### 2.49.0 (2023. 04. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.49.0/GamebaseSDK-Android.zip)
```
Raised the minimum supported version to Android 4.4.(minSdk 16 -> 19)
```

#### Feature Updates
* Improved the internal metrics

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

### 2.48.0 (2023. 03. 28.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.48.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud Android SDK(1.4.2), PAYCO Android SDK(1.5.11)
* Applied the standby domain for Gamebase server in preparation for DNS failure
* Improved the internal logic

#### Bug Fixes
* Fixed a bug where, when proguard is applied in Unity, API calls related to Purchase fails.

### 2.47.0 (2023. 02. 14.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.47.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Hangame Android SDK (1.6.3)
* Improved the internal logic

### 2.46.0 (2023. 01. 31.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.46.0/GamebaseSDK-Android.zip)

#### Added Features
* Added an API to retrieve subscription statuses.
    * Gamebase.Purchase.requestSubscriptionsStatus(Activity, PurchasableConfiguration, GamebaseDataCallback&lt;List&lt;PurchasableSubscriptionStatus&gt;&gt;)
    * You can view expired subscription statuses with the PurchasableConfiguration.Builder.setIncludeExpiredSubscriptions(boolean) API.
* Added an option to perform rendering on Cutout area and ignore SafeArea from the WebView.
    * GamebaseWebViewConfiguration.Builder.setRenderOutsideSafeArea(boolean)

#### Feature Updates
* External SDK update: Kakaogame SDK (3.14.14)

### 2.45.0 (2022. 12. 27.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.45.0/GamebaseSDK-Android.zip)

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

#### Bug Fixes

* Fixed an issue where ConcurrentModification exceptions occur intermittenly while running the app.
* Fixed an issue where, when calling Gamebase.getAuthProviderUserID() after logging in with Hangame thirdIdP, NullPointerException occurs.

### 2.44.2 (2022. 11. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.2/GamebaseSDK-Android.zip)

#### Added Features
* Added the 'storeCode' field to the PurchasableReceipt VO class.

#### Feature Updates
* External SDK update: Kotlin(1.7.20), Hangame Android SDK(1.6.1)
* Modified the Gamebase WebView by reflecting the recommendations in 'Google Play Pre-Launch Report'.
    * Expanded the title bar size
    * Modified the image description text

#### Bug Fixes
* Removed the 'deprecated' annotaion incorrectly declared on the 'itemName' field of the PurchasableItem V0 class.

### 2.44.1 (2022. 10. 25.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.1/GamebaseSDK-Android.zip)

#### Added Features
* Added the **PushConfiguration.Builder.enableRequestNotificationPermission(boolean)** API so that a popup to request Push permission does not show up automatically when calling the registerPush API from Android 13 OS or higher.

#### Feature Updates
* For Facebook Android SDK 13.2.0 or higher, Facebook Client Token must be set.
    * When adding the **facebook_client_token** field to additionalInfo in the Gamebase Console for Gamebase Android SDK 2.44.1 or higher as follows, Facebook Client Token is automatically applied to the client SDK.

            { "facebook_permission": [...], "facebook_client_token": "a01234bc56de7fg89012hi3j45k67890" }

#### Bug Fixes
* Fixed a bug where, when calling the **Gamebase.Push.registerPush** API from a device running Android 6.0(M, API Level 23), **IllegalArgumentException** exception occurs.

### 2.44.0 (2022. 10. 11.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.44.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: NHN Cloud Android SDK(1.2.0), TOAST Gamebase IAP Android SDK(0.21.0), Google Play Services Auth(20.0.3)
* Modified to show a popup that automatically requests permission to allow notification when calling registerPush from Android 13 OS.
* Improved the internal logic so that silentSignIn API can be sued when logging into Google.

#### Bug Fixes
* Fixed an issue where a crash occurs when logging with the previous version of IdP while logging with Hangame IdP, when no error occurs if you log in with an invalid third-party IdP after using a valid third-party IdP.

### 2.43.0 (2022. 09. 07.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.43.0/GamebaseSDK-Android.zip)

#### Added Features
* Added ONE store v19 Purchase Adapter.
    * You can use it by adding the **gamebase-adapter-purchase-onestore-v19** module and [ONE store v19 IAP SDK] to your build dependentices (https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs).
            
            dependencies {
                ...
                implementation files('libs/iap_sdk-v19.00.02.aar')
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
            }
            
#### Feature Updates
* External SDK update: Google Billing Client(5.0.0), NHN Cloud Android SDK(1.1.0), TOAST Gamebase IAP Android SDK(0.20.0), Kakaogame Android SDK(3.14.4)
* Added a parameter to enter a service region when logging in to LINE.
    * [Game > Gamebase > Android SDK User Guide > Authentication > Login with IdP](./aos-authentication/#login-with-idp)
* Added a defense logic so that a crash does not occur When using LINE IdP even in devices with API 19 or lower.

#### Bug Fixes
* Fixed an issue where a crash occurs when forcibly lowering the Naver Login SDK version to 4.1.4 to use the Naver PLUG SDK or Naver Cafe SDK.

### 2.42.1 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.1/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Facebook Android SDK(11.3.0)

### 2.42.0 (2022. 07. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.42.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: Hangame Android SDK(1.5.2)
* Added the mappedUserValid field that represents the mapped user status to the ForcingMappingTicket VO class.
* Modified to fail initialization when the version of Gamebase Adapter does not match the version of Gamebase, as it can cause runtime exception.

#### Bug Fixes
* Fixed an issue where Naver web login fails from LDPlayer.
* Fixed an issue where a crash occurs when Twiter login fails due to a low OS verison.

### 2.41.2 (2022. 07. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.2/GamebaseSDK-Android.zip)

#### Feature Updates 
* Changed the default WebView settings to 'Allow cookies'.

### 2.41.1 (2022. 07. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed an issue where the 'View' button in the Terms and Conditions screen does not work.

### 2.41.0 (2022. 07. 05.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.41.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: TOAST Android SDK(0.31.1), Hangame Android SDK(1.4.6)
* When the custom scheme event registered in WebView works, the WebView is automatically closed.
    * To maintain WebView when the custom scheme event works, call **GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)** API.

#### Bug Fixes
* Fixed an issue where the system crashes intermittently or login fails when trying login right after Hangame IdP logout

### 2.40.0 (2022. 05. 24.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.40.0/GamebaseSDK-Android.zip)

#### Added Features
* Added Purchase Adapter for external payment of ONE store.
    * You can use it by adding the **gamebase-adapter-purchase-onestore-external** module to your build dependencies.
            
            dependencies {
                ...
                implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
            }
            
#### Feature Updates
* External SDK update: TOAST Android SDK(0.31.0), TOAST Gamebase IAP Android SDK(0.18.5), LINE Android SDK(5.8.0)
* Fixed an issue where push did not work properly when different apps share a single Gamebase project.
    * Declare a different **com.nhncloud.sdk.push.deviceId.salt** value for each app in AndroidManifest.xml.

            <!-- When you have multiple applications sharing an Gamebase project, use this field to identify each application. -->
            <meta-data android:name="com.nhncloud.sdk.push.deviceId.salt"
                       android:value="ApplicationForGoogleStore" />

### 2.39.0 (2022. 05. 10.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.39.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: TOAST Android SDK(0.30.1)

### 2.38.0 (2022. 05. 03.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.38.0/GamebaseSDK-Android.zip)

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

#### Feature Updates
* External SDK update: TOAST Android SDK(0.30.0)
* Fixed unnatural sentences in the Traditional Chinese (zh-TW) language set of Display Language.

### 2.37.0 (2022. 04. 26.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.37.0/GamebaseSDK-Android.zip)

#### Added Features
* Added the following field so that you can add parameters after the contact center URL.
    * **ContactConfiguration.Builder.setAdditionalParameters(Map&lt;String, String&gt;)**

#### Feature Updates
* External SDK update: TOAST Gamebase IAP Android SDK(0.18.3)
* Made improvements so that, when userId and gamebaseProductId are missing from the Amazon Appstore payment data, userId and gamebaseProductId are automatically filled in.

### 2.36.0 (2022. 04. 12.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.36.0/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: TOAST Android SDK(0.29.2), TOAST Gamebase IAP Android SDK(0.18.2), Hangame Android SDK(1.4.5)
* Made improvements so that sms_hash is generated internally in Hangame Android SDK v1.4.5.
    * sms_hash does not need to be set anymore.

### 2.35.0 (2022. 03. 29.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.35.0/GamebaseSDK-Android.zip)

```
Gamebase Android SDK is now only distributed through Maven Central.
The ZIP file for distribution no longer includes AAR files.
```

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

#### Feature Updates
* External SDK update: TOAST Android SDK(0.29.1), Hangame Android SDK(1.4.4)
* Improvements have been made so that the long white background is not displayed when the terms and conditions window is displayed.

#### Bug Fixes
* Fixed an issue where the **GamebaseWebViewConfiguration.Builder.setNavigationBarVisible()** API, which hides the WebView's navigation bar, did not work properly.

### 2.34.0 (2022. 02. 22.)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.34.0/GamebaseSDK-Android.zip)

#### Added Features
* If you select **Add Popup Button** in the Update Required settings of the Gamebase console, a **Details** button will be added to the client's Update Required popup window.
* Added an API to find out whether the device has allowed notifications or not.
    * **Gamebase.Push.queryNotificationAllowed()**
* Added a VO class that can be used to find out whether the terms and conditions UI was displayed after calling the common terms and conditions API.
    * **GamebaseShowTermsViewResult**

#### Feature Updates
* The following field has been deprecated because whether to display the kickout popup window can be set during kickout registration in the Gamebase console.
    * **UIPopupConfiguration.enableKickoutPopup**

#### Bug Fixes
* Fixed a bug where, when a user selected **Do not show again today** on an image notice, the image notice is not displayed even after 24 hours have passed.

### 2.33.0 (2022.01.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.33.0/GamebaseSDK-Android.zip)

#### Added Features
* Added a new API that allows you to change settings of the common terms and conditions window.
    * [Game > Gamebase > Android SDK User Guide > UI > Terms > showTermsView](./aos-ui/#showtermsview)

#### Feature Updates
* External SDK update: PAYCO Android SDK(1.5.7), Hangame Android SDK(1.4.3.1), TOAST Gamebase IAP Android SDK(0.18.1)
* Added logic to check whether the launching information has not changed immediately after successful login.

### 2.32.0 (2021.12.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.32.0/GamebaseSDK-Android.zip)

#### Added Features
* Added the **GamebaseEventCategory.SERVER_PUSH_APP_KICKOUT_MESSAGE_RECEIVED** type to GamebaseEventCategory of GamebaseEventHandler.
    * Please refer to the following document for how to use this event.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Server Push](./aos-etc/#server-push)
* Added the **GamebaseEventCategory.LOGGED_OUT** GamebaseEventHandler category, which works when Gamebase Access Token expires and login is required.
    * [Game > Gamebase > Android SDK User Guide > ETC > Additional Features > Gamebase Event Handler > Logged Out](./aos-etc/#logged-out)

#### Feature Updates
* Improved the webview so that the ONE store deep link whose webview URL starts with **onestore://** works.

#### Bug Fixes
* Fixed a bug in Gamebase Android SDK 2.31.0 where an IdP account cannot be changed because IdP logout is not called even when logout is called.

### 2.31.0 (2021.12.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.31.0/GamebaseSDK-Android.zip)

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

#### Feature Updates
* External SDK update: TOAST Android SDK(0.29.0)
* Fixed an issue where it was not possible to register inquiries with banned user information from the Customer Center link in the ban webview.
* Fixed an issue where the launch pop-up was intermittently displayed in English when calling Gamebase initialization as soon as the app was executed.
* Improved the scheduler so that it always checks whether the launch information has changed when the app is switched from the background to the foreground status.

### 2.30.0 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.30.0/GamebaseSDK-Android.zip)

#### Added Features
* Added a new forced mapping API, which removes the inconvenience of having to try IdP login once more when performing forced mapping.
    * [Game > Gamebase > Android SDK User Guide > Authentication > Mapping > Add Mapping Forcibly](./aos-authentication/#add-mapping-forcibly)
* Added an API that allows you to log in to the corresponding account when an AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) error occurs after calling Gamebase.addMapping().
    * [Game > Gamebase > Android SDK User Guide > Authentication > Mapping > Change Login with ForcingMappingTicket](./aos-authentication/#change-login-with-forcingmappingticket)

#### Feature Updates
* External SDK update: Hangame Android SDK(1.4.2)
* Improved so that the user can modify and use the maintenance details webview HTML provided by Gamebase by default.
    * [Game > Gamebase > Android SDK User Guide > Initialization > Launching Information > 1. Launching > 1.3 Maintenance > Change Default Maintenance HTML](./aos-initialization/#change-default-maintenance-html)
* Fixed an error where the time on the default maintenance webview was displayed in the device language even though DisplayLanguageCode was set.
* Improved the network error that occurred repeatedly by trying to communicate with a disconnected connection when a communication error occurs.

### 2.29.0 (2021.11.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.29.0/GamebaseSDK-Android.zip)

#### Added Features
* Added a feature to declare scope when logging in to Google.
    * [https://developers.google.com/identity/protocols/oauth2/scopes](https://developers.google.com/identity/protocols/oauth2/scopes)
    * If you add **email** as scope, you can obtain email information from the profile.
    * Scope is automatically set when the user logs in if you set the AdditionalInfo in the Gamebase Console as follows.

```
{"scope":["email","myscope1","myscope2",...]}
```

#### Feature Updates
* External SDK update: TOAST Android SDK (0.27.4)
* Added DisplayLanguage.Code class, which was described only in the DisplayLanguage guide document and was not actually included in the SDK.
    * [Game > Gamebase > Android SDK User Guide > ETC > Display Language > Types of language codes supported by Gamebase](./aos-etc/#types-of-language-codes-supported-by-gamebase)

### 2.28.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.28.0/GamebaseSDK-Android.zip)

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

#### Feature Updates
* External SDK update: PAYCO Android SDK(1.5.6)

### 2.27.1 (2021.09.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.1/GamebaseSDK-Android.zip)

#### Feature Updates
* External SDK update: PAYCO Android SDK (1.5.5), Hangame Android SDK (1.4.1), Weibo Android SDK (11.8.1)
* Added a retry logic when the webview is not displayed normally in the emulator or rooted terminal, so that the webview is displayed normally.
    * This applies to image notification, customer center, and common terms and conditions that run as a webview.
* Improved stability by improving Weibo IdP authentication.
    * Added exception handling, waiting, and retry logic to an API that is a synchronous API but actually operates asynchronously and generates an error.

#### Bug Fixes
* Fixed a bug where the 'Unregistered Game Version' error pop-up was displayed only in English.
* Fixed a bug where the Chinese text was not displayed in the maintenance pop-up.
* Fixed a bug where, if [Credential Login](./aos-authentication/#login-with-credential) is performed, [Login as the Latest Login IdP](./aos-authentication/#login-as-the-latest-login-idp ) call always fails.

### 2.27.0 (2021.08.24)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.27.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated the external SDK: TOAST Android SDK (0.27.1)
* Added ONE Store V16 store

### 2.26.0 (2021.08.10)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Improved the Display Language feature.
    * Until now, you had to manually edit the gamebase-sdk-base-version.aar file to add the language set.
        * It has been improved so that you can add the localizedstring.json file to the res/raw folder of the project.
    * Until now, the method of adding the Display Language language set in the Unity guide could not be applied to Android.
        * It has been improved so that the information is reflected in the Android build even if a localizedstring.json file is added, according to the Unity guide.
        * [Game > Gamebase > Unity SDK User Guide > Notes > Additional Features > Display Language > Add New Language Sets](./unity-etc/#add-new-language-sets)
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

#### Bug Fixes
* Fixed an issue where the language code of the message sent from the Push console does not match because the language code of the device is applied to the Push notification language setting without any extra processing.

### 2.25.0 (2021.07.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.25.0/GamebaseSDK-Android.zip)

#### More Features
* Add monthly payment limit feature
    * If the monthly payment limit is exceeded, **a PURCHASE_LIMIT_EXCEEDED(4007)** error occurs.

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

### 2.24.0 (2021.06.29) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.24.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Change the internal launch URL
* Fixed incorrect wording in SDK attachments

### 2.23.0 (2021.06.14)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.23.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed the issue of the title of the suspended view details web view not being displayed

### 2.22.0 (2021.05.25) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.22.0/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated the external SDK: TOAST Android SDK(0.25.0), Hangame Android SDK(1.4.0)

#### Bug Fixes
* The following error has been fixed: When a user logs out and logs in again with another user ID, a payment at Google Play Store is successful but the return value is sometimes "Failed."
* The following error has been fixed: When the name of an app package contains a capital letter, the "Sign In with Apple" log-in fails.

### 2.21.1 (2021.04.19) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed an issue where the system crashes when canceling Hangame login via PAYCO

### 2.21.0 (2021.04.13) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.21.0/GamebaseSDK-Android.zip)

#### More Features
* Japanese authentication for Hangame added.	 	

#### Feature Updates
* External SDK update: Facebook Android SDK (6.5.1), LINE Android SDK (5.4.0)
	
#### Bug Fixes
* Fixed a crashing error caused when calling payment API on build with Proguard applied.

### 2.20.2 (2021.03.30) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.2/GamebaseSDK-Android.zip)

#### Feature Updates
* Updated to Billing Client Version 3.0.3 where payment errors caused by Android 11 devices in Google Play Store are fixed

### 2.20.1 (2021.02.23) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* Fixed a logic that could cause the push-fcm module to crash during initialization

### 2.20.0 (2021.02.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.20.0/GamebaseSDK-Android.zip)

#### More Features
* Common Terms and Conditions added
	* Added an API that opens the Terms and Conditions webview
	* Added an API that views the Terms and Conditions list and agreement status per user
	* Added an API that saves the user agreement data on the Gamebase server

#### Feature Updates
* Changed to display the Customer Center without login if the Customer Center type is TOAST organization product (Online Contact).

### 2.19.1 (December 29, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.19.0
	* (Common) Weibo authentication added
	* (Android) Sign-in with Apple authentication added
	
#### Feature Updates
* [SDK] 2.19.0
	* (Common) Launching status code added: beta service (205)

#### Bug Fixes
* [SDK] 2.19.0
    * (Unity) WebSocket에서 재시도 시 OutOfMemoryException이 발생하는 문제 수정
* [SDK] 2.19.1
	* (Android) Weibo 로그인 시도 후 다른 IdP로 로그인 시 크래시가 발생하는 문제 수정

### 2.18.2 (December 15, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.2/GamebaseSDK-Android.zip)

#### More Features
* When the Gamebase Customer Center page opens, game-defined extra data is delivered: SDK 2.18.2
	* [Console] Extra data added can be checked in Customer Center > Customer Inquiry: Customer Inquiry Details
* [SDK] 2.18.2
	* (Common) additionalURL field added for the case of a developer's own Customer Center being opened
	* (Common) Localized product information added in the transaction item information: localizedTitle, localizedDescription

#### Feature Updates
* [SDK] 2.18.2
    * (Common) TOAST SDK update: Android(0.24.2), iOS(0.27.1), Unity(0.21.3)
	* (Android) External SDK update to resolve encryption logic security warnings: PAYCO Login SDK (1.5.3), Hangame ID SDK (1.3.2)
	* (Android) Tencent Push module removed
	* (Android) The deprecated function in Gamebase Android SDK 2.6.0 removed
		* GamebaseConfiguration.Builder.setFCMSenderId()
		* GamebaseConfiguration.Builder.setTencentAccessKey()
		* GamebaseConfiguration.Builder.setTencentAccessId()
#### Bug Fixes
* [SDK] 2.18.2
    * (Android) Fixed the issue where WebView custom scheme does not run on a 5.0 - 6.0 OS device

### 2.18.1 (November 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.18.1/GamebaseSDK-Android.zip)

#### More Features
* Added Galaxy Store: SDK 2.18.0

#### Feature Updates
* [SDK] 2.18.0
    * (Android) TOAST SDK update: Android(0.24.1) - Apply GooglePlay Billing Library v.3.0.1
    * (Android) Added the response for WebView SSL security warnings

#### Bug Fixes  
* [SDK] 2.18.1
    * (Android) Fixed an issue where a crash would occur after a Google transaction is approved in 2.18.0

### 2.17.1 (October 13, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.17.1/GamebaseSDK-Android.zip)

```
Contact our Customer Center if you want to use the Hangame authentication.
```

#### More Features
* Added Hangame IdP authentication: SDK 2.17.0

#### Feature Updates
* [SDK] 2.17.0
	* (공통) Supports the download feature when a Customer Center attachment image is clicked
	* (공통) TOAST SDK update: Android(0.23.2), Unity(0.21.2)

#### Bug Fixes  
* [SDK] 2.17.1
	* (Android) Fixed an issue where a crash would occur in the kotlinx-coroutine module when ImageNotice API is called in 2.17.0
	
### 2.16.0 (September 22, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.16.0/GamebaseSDK-Android.zip)

#### More Features
* Added a feature to Customer Center
	* [SDK] 2.16.0
		* (Common) Added API (Gamebase.Contact.requestContactURL): Returns Customer Center URL
		* (Common) Added the ContactConfiguration parameter so userName can be configured for Customer Center API
		
### 2.15.0 (August 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Android.zip)
```
Updated Google Billing Client in the Gamebase SDK 2.15.0 version. 

For 'gamebase-adapter-purchase-google', to upgrade a version below Gamebase SDK 2.15.0 to more than 2.15.0,  
set 'Requires Update' for 'Game Client Version' of the previous version.

This is because, in order to execute reprocessing when an error occurs during purchasing an item,  
you may encounter an issue during reprocessing if a different billing client version is applied to each of many devices.   
```

#### More Features
* [SDK] 2.15.0
    * (Common) Added feature, for push token registration, to allow the app to receive push alarms even under Foreground with the NotificationOption setting  
    * (Common) Added Push API: Check token information of a push (Gamebase.Push.queryTokenInfo API)

#### Feature Updates
* [SDK] 2.15.0
    * (Common) TOAST SDK Updates: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)

### 2.13.0 (July 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.13.0/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.13.0
    * (Android) Modified the logic of calculating the percentage of popup image for notice on image 

#### Bug Fixes
* [SDK] 2.13.0
    * (Android) Fixed an issue in which the ANDROID_ACTIVITY_DESTROYED(31) error is returned for the close callback when an webview is closed 
    * (Android) Fixed error in which the ProGuard declaraction is missing from the payment module 

### 2.12.0 (July 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.12.0/GamebaseSDK-Android.zip)

#### More Features
* Image Notices: Shows image popups within a game according to exposed period and priority order 
    * [SDK] 2.12.0: Added Show Image Notice API 
  
### 2.11.0 (June 23, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.11.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.11.0
	* Added Purchase API: Request for payment with Product ID, and enter additional information (UserPayload) to be confirmed when payment is completed 

### 2.10.0 (May 26, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.10.0/GamebaseSDK-Android.zip)

#### More Features
* [SDK] 2.10.0
	* (Common) Added GamebaseEventHandler which has all previous event systems 
		* Includes ServerPush and Observer, and checks promotional purchase or push events 

### 2.9.1 (May 12, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.9.1
	* (Android) Fixed an error in which an indicator level becomes null after mapped and does not show properly on the purchase indicator  
	* (iOS) Fixed the inavailability of a build on an unreal engine since warning is considered as a build error 

### 2.9.0 (April 28, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Android.zip)

#### More Features
* Suspension of Membership Withdrawal 
	* [SDK] 2.9.0
		* (Common) Added API: Apply for suspension of withdrawal, Cancel application for suspension of withdrawal, Immediately withdraw while on suspension, and Check if user's withdrawal is suspended  

#### Feature Updates
* [SDK] 2.9.0
	* (Common) Updated TOAST SDK: Android(v0.21.0), iOS(v0.23.0), Unity(0.20.1)
	* (Common) Updated PAYCO Login SDK: Android(v1.5.0), iOS(v1.4.0)

### 2.8.1 (April 14, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.8.1 
	* (Common) Added internal indicators to check Analytics delivery results
	
#### Bug Fixes
* [SDK] 2.8.1 
	* (Android) Modified codes that may cause crashes after process restarts

### 2.8.0 (March 24, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.8.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.8.0
	* (Common) Added more purchase and product information, such as product type and regional prices 

#### Feature Updates 
* [SDK] 2.8.0 
	* (Common) Updated to further show a popup to move to stores when it fails to initialize on an app version not registered on console 
	* (Android) Fixed codes that may fail due to initialization timing when payment-related API is called immediately after login 
	
### 2.7.2 (March 10, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.7.2 
      * Gamebase 초기화중 ToastLogger 초기화 부분에서 크래쉬가 발생할 수 있는 코드를 수정
      * 서버 버전을 v1.2.1 로 업데이트 하였습니다.

### 2.7.1 (February 25, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.1/GamebaseSDK-Android.zip)

#### Feature Updates 
* [SDK] 2.7.1
	* (Common) Updated to return value, after guest login, when GetAuthProviderUserID is called

### 2.7.0 (January 21, 2020)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.7.0/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.7.0
	* (Android) Modified not to occur crash when the traceError, which is a required parameter, is missing at the server response 
	* (Android) Modified not to occur exceptions when Firebase setting is missing 

### 2.6.2 (December 24, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.2/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.6.2
	* (Common) TOAST SDK Updates: Android(0.19.4), iOS(0.20.1), Unity(0.18.0)

### 2.6.1 (December 10, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.1/GamebaseSDK-Android.zip)

#### Bug Fixes
* [SDK] 2.6.1
  * (Android) Fixed crash occurrence when Gamebase.login() is called before Gamebase.initialize() 
  * (Android) Fixed the wrong delivery of TOAST Analytics User Data to java address 
  * (Android) Fixed crash occurrence when IAP is not enabled  

### 2.6.0 (November 12, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.6.0/GamebaseSDK-Android.zip)

```
To upgrade to Gamebase SDK 2.6.0 from a lower-than-2.6.0 version,  
make sure to apply changes as described in the Upgrade Guide.  
Find Upgrade Guide at: Game > Gamebase > Upgrade Guide
```

#### More Features 
* [SDK] 2.6.0
  * (Common) Added TOAST Logger to send data to Log & Crash for analysis 
  * (Android) Added the payment feature for Google subscription  
  * (Android) Since Gamebase Android SDK is deployed by Bintray, it only takes a gradle setting to enable Gamebase. 

### 2.5.0 (August 27, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.5.0/GamebaseSDK-Android.zip)

#### More Features 
* [SDK] 2.5.0
	* Provides API which opens CS URL entered on a console via webview 
	
### 2.4.4 (July 23, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.4/GamebaseSDK-Android.zip)

#### Feature Updates
* [SDK] 2.4.4
	* (Common) Format changed for member error code
	* (Unity) Key added for GamebaseServerPushType (TRANSFER_KICKOUT)

### 2.4.2 (June 25, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.2/GamebaseSDK-Android.zip)

#### Features Updates/Changes
* [SDK] 2.4.2
	* (Common) Add TOAST Launching information in the JSON string format to LaunchingInfo

#### Bug Fixes
* [SDK] 2.4.2
	* (Common) Fixed Bugs in Analytics: Modified to initialize indicators data that are saved before logout, withdrawal, or account transfer. 
	
### 2.4.0 (May 28, 2019)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.4.0/GamebaseSDK-Android.zip)

#### Feature Updates/Changes
* [SDK] 2.4.0
  * (Common) Change of Classes Relevant to Indicators 
        * LevelUpData Class: Changed userLevel and levelUpTime as required parameters; the other fields are deleted [See Details: [Android](./aos-etc/#game-user-data-settings) / [iOS](./ios-etc/#game-user-data-settings) / [Unity](./unity-etc/#game-user-data-settings) / JavaScript]
            * GameUserData Class: Added the classId (game user's profession) field [See Details: [Android](./aos-etc/#level-up-trace) / [iOS](./en/ios-etc/#level-up-trace) / [Unity](./unity-etc/#level-up-trace) / JavaScript]

    * (Android) NAVER SDK Version Updated (v4.2.5): Bug of NAVER SDK fixed (fixed the issue, in which authentication process was stopped due to forced closure of activities when the app was restarted via app icon while NAVER login was underway)  

### 2.3.1 (2019.05.16)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.1/GamebaseSDK-Android.zip)

#### 버그수정
* [SDK] 2.3.1
    * (Android)2.3.0버전에서 Twitter 로그인 되지 않던 문제 수정

### 2.3.0 (2019.04.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.3.0/GamebaseSDK-Android.zip)
    
```
Gamebase를 사용하면 50여개의 중국스토어 연동이 가능합니다.
중국출시에 관심 있으신 경우에는 고객센터로 연락주세요.
```

#### 기능 추가
* [SDK] 2.3.0
    * (Android/Unity)중국스토어 인증/결제 추가

#### 기능 개선/변경
* [SDK] 2.3.0
    * (공통)Launching Status Code 추가: "심사중(204)", "테스트중(203)"
    * (Android)최근 로그인한 Provider로 로그인 및 웹소켓 응답 실패를 받았을 경우(Timeout, network disable 등) AuthToken을 삭제 처리하지 않도록 수정
    * (Android)IdP로그인 시 AuthAdapter 내부에서 발생하는 MemoryLeak을 수정

### 2.2.2 (2019.04.11)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.2/GamebaseSDK-Android.zip)

#### 버그수정
* [SDK] 2.2.2
    * (Android)Gamebase 초기화 이전 TransferAccount API 호출시, 콜백이 오지 않는 이슈를 수정

### 2.2.0 (2019.03.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.2.0/GamebaseSDK-Android.zip)

#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * TransferAccountInfo 발급 API (issueTransferAccount)
        * 발급된 TransferAccountInfo를 사용하여 계정 이전을 요청하는 API (transferAccountWithIdPLogin)
        * 발급된 TransferAccountInfo를 확인하는 API (queryTransferAccount)
        * 이미 발급된 TransferAccountInfo 갱신하는 API (renewTransferAccount)        
* 강제매핑 기능 추가: 이미 다른 계정에 연동 되어있는 IdP계정을 매핑할 수 있는 기능
    * (SDK공통)추가된 API 
        * 강제매핑하는 API (addMappingForcibly)

#### 기능 개선/변경
* [SDK] 2.2.0
    * (Android)IAP SDK 버전을 최신버전인 v1.5.3 버전으로 업데이트

### 2.1.0 (2019.02.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.1.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 2.1.0
    * (공통)TransferKey API 삭제
        * issueTransferKey : TransferKey 발급
        * requestTransfer : TransferKey 검증
        
#### 버그수정
* [SDK] 2.1.0
    * (Android)Gamebase 초기화 이전, onActivityResult()가 호출되면서 이상 동작하던 버그 수정

### 2.0.0 (2019.01.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.0.0/GamebaseSDK-Android.zip)

```
Gamebase 2.0의 개선된 전체 지표를 활용하기 위해서는 SDK 업데이트가 필요합니다.
```

#### 기능 추가
* [SDK] 2.0.0
    * (공통)Custom 지표를 위한 API 추가 (구매 성공의 경우 SDK내부에서 자동 전송)
        * setGameUserData : 게임 로그인 이후 유저 레벨 정보 전송
        * traceLevelUpData : 레벨업 추적을 위하여 게임 유저의 레벨업이 되었을 때 호출


#### 기능 개선/변경
* [SDK] 2.0.0
    * (Android)Push SDK 업데이트(android:1.7.0)
    * (Android)Adapter API 변경
        * Launching 정보 전달
        * logout, withdraw API에 Callback 추가

### 1.14.5 (2018.12.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.5/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.14.5
    * deprecated 되었던 다음 API가 제거되었습니다.
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)
    * 결제 모듈(gamebase-adapter-purchase-iap) 수정되었습니다.
        * IAP SDK를 1.5.2로 업데이트
        * Client에서는 사용되지 않는 IAP TEST Store 제거
        * 결제 재처리 로직(requestRetryTransaction)에서 데이터가 불완전할 때 호출이 실패하는 문제를 수정
        * 크래시를 방지하기 위해 모든 IAP SDK 호출부에 예외 처리

### 1.14.2 (2018.11.15)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.14.2
    * (Android)점검시, 데이터구조에서 점검 시작/종료 시간을 의미하는 epoch time의 타입을 기존 String에서 long으로 타입 변경 : 기존 Gamebase Unity와 연동 후 점검 호출 시 타입불일치로 콜백이 내려오지 않는 현상으로 인한 수정

#### 버그수정
* [SDK] 1.14.2
    * (Android)에뮬레이터 환경에서 스토어앱(PlayStore, OneStore 등)이 없는 상태에서 "앱 설치/업데이트"시 스토어 미체크로 인한 crash 버그를 수정
    
### 1.14.1 (2018.10.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.14.1/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.14.0
    * (공통)Gamebase Webview에서 파일첨부 기능 추가 : Android의 API 19, Kitcat 에서는 정상 동작하지 않습니다.
    
#### 기능 개선/변경
* [SDK] 1.14.0
    * (공통)이용정지/점검에 대해 사용자가 콘솔에 작성한 메시지들을 URL 인코딩하여 전송하고 클라이언트에서 디코딩하여 처리하도록 수정
    * Remove API : Webview, Network, Launching
        * (void)Gamebase.WebView.showWebBrowser(Activity, String)
        * (void)Gamebase.Network.addOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Network.removeOnChangedListener(NetworkManager.OnChangedListener)
        * (void)Gamebase.Launching.addOnUpdatedListener(LaunchingOnUpdateListener)
        * (void)Gamebase.Launching.removeOnUpdatedListener(LaunchingOnUpdateListener)        
    * Deprecated  API 
        * (void)Gamebase.WebView.showWebView(Activity, String)
        * (void)Gamebase.WebView.showWebView(Activity, String, GamebaseWebViewConfiguration)
    
#### 버그수정
* [SDK] 1.14.1
    * (Android)Auth API 호출 후 콜백에서 다시 Auth API 중복 호출시 정상 호출이 되지 않는 버그 수정
    
### 1.13.0 (2018.09.13)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.13.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.13.0
    * (공통)IAP SDK 최신버전 적용 (android:1.5.1, iOS:1.6.0)
    * (Android)Push API 호출 시, Gamebase 초기화/로그인 상태에 따라 호출 실패에 대한 에러 메시지를 보다 명확하게 개선
        * 초기화 전 호출 : NOT_INITIALIZED(1)
        * 초기화 이후 호출시 Push 모듈이 없음 : NOT_SUPPORTED(10)
        * 초기화 성공 및 로그인 이전 호출 : NOT_LOGGED_IN(2)        
    
#### 버그수정
* [SDK] 1.13.0
    * (Android)NaverCafe SDK와의 충돌로 NAVER 로그인 시 발생하던 오류 해결
        
### 1.12.2 (2018.08.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.12.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.12.2
    * (Android)WebSocket 타입아웃시 (API 호출 시간 경과), 크래시가 날 수 있는 버그에 대해 방어로직 처리
    
#### 버그수정
* [SDK] 1.12.2
    * (Android)auth-twitter-adapter 를 포함한 상태에서 TargetSdk 28로 빌드시 초기화 에러가 발생하는 문제 수정

### 1.12.1 (2018.08.09)

#### 기능 개선/변경
* [SDK] 1.12.1
    * (공통)IAP SDK 최신버전 적용 (1.5.0)
    * (공통)Gamebase 점검페이지에서 점검시간을 단말기 설정 국가시간에 맞추어 노출하도록 개선
    * (공통)점검페이지를 외부 페이지로 사용할 때 Console에 입력한 점검 정보를 사용할 수 있도록 기능 추가
    * (공통)IdP 매핑된 사용자의 Guest 매핑시도시 에러 발생(TCGB_ERROR_AUTH_ADD_MAPPING_CANNOT_ADD_GUEST_IDP)
    * (공통)인증 API 중복 호출시 에러 발생(AUTH_ALREADY_IN_PROGRESS_ERROR)
    * (Android)TencentPush SDK 업데이트 (3.2.3)
    * (Android)Onestore v17(API v5) 지원 : Gamebase에서는 v16(스토어코드=TS)은 제공하지 않습니다.

### 1.11.1 (2018.07.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.1/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.11.1
    * (공통)Guest로그인 후 AddMapping 성공 시, loginForLastLoggedInPrivder를 하게되면, AddMapping 성공한 IdP계정을 사용하여 로그인하도록 변경
    
#### 버그수정
* [SDK] 1.11.1
    * (공통)점검 해제 후 후속 API 진행(login/push/purchase 등)이 되지 않던 버그 수정
    * (Android)Gamebase.addObserver()를 통해 ObserverMessage를 수신하였을 경우, ObserverMessage.data.code의 타입이 int가 아니라 String인 버그를 수정


### 1.11.0 (2018.06.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.11.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Twitter IdP 추가 : Android, iOS
* LINE IdP 추가 : Android만 제공. iOS는 2018년 7월 제공 예정입니다.
    
#### 기능 개선/변경
* [SDK] 1.11.0
    * (공통)LocalizedString 일본어 번역 추가
    * (공통)인증 API 호출시 초기화, 로그인을 하지 않은 경우 명확히 에러 코드를 구분하도록 내부 로직을 개선
    * (Android)'android.permission.READ_PHONE_STATE' 권한 제거
    * (Android)GamebaseConfiguration.Builder의 필수 설정값인 setAppId, setAppVersion을 생성자에서 입력할 수 있도록 변경
    * (Android)GamebaseConfiguration.Builder 의 setServerApiVerseion API를 제거
    * (Android)getAuthBanInfo() API, class AuthBanInfo 이름을 변경 : getBanInfo(), class BanInfo

### 1.9.0 (2018.05.03)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.9.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Transfer 기능 추가
    * guest 사용자가 매핑없이 새로운 기기로 이전할 수 있는 기능
    * (SDK공통)추가된 API 
        * Transfer Key 발급 API (IssueTransferKey)
        * 발급된 TransferKey를 사용하여 계정 이전을 요청하는 API (RequestTransfer)

#### 버그 수정
* [SDK] 1.9.0
    * (Android) Heartbeat 에서 잘못된 사용자로 판정되는 경우 이용정지 팝업 창이 뜨지 않도록 수정(iOS 와 동일한 로직으로 수정)

### 1.8.1 (2018.04.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.1/GamebaseSDK-Android.zip)

#### 버그 수정
* [SDK] 1.8.1
    * (Android. iOS)registerPush를 호출시 displayLanguageCode를 null로 전달하면 registerPush가 실패하는 버그 수정

### 1.8.0 (2018.04.05)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.8.0/GamebaseSDK-Android.zip)

#### 기능 추가
* Kick out 기능 추가
    * 현재 게임 중인 전체 사용자의 연결을 끊는 기능(점검시 게임에서 전체 사용자의 연결을 끊고 싶을 때 사용할 수 있음)
    * (SDK 공통)kick out 이벤트를 받을 수 있는 API 추가
* Observer 기능 개발 및 API 추가
    * (SDK 공통) 점검 등 앱 상태/네트워크 상태/유저 상태(이용정지) 변경사항에 대한 Listener를 Observer 등록을 통하여 일괄 처리할 수 있도록 API 추가

#### 기능 개선/변경
* [SDK] 1.8.0
    * (공통)Observer 기능 추가에 따라 다음 API Deprecated : LaunchingStatus Listener, Network Listener(기존 사용자는 계속 사용 가능)

### 1.7.0 (2018.02.22)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.7.0/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.7.0
    * NAVER IdP 인증 추가
    * Display Language 설정 추가: 단말기 언어와 별도로 게임내에서 게임유저의 노출 언어를 설정할 수 있도록 Display 언어를 추가하였습니다.

### 1.5.0 (2017.12.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.5.0/GamebaseSDK-Android.zip)
#### 기능 추가
* [SDK] 1.5.0
    * WebView가 닫힐 때 발생하는 Close Callback 추가
    * WebView에서 사용하는 Custom Scheme의 Event를 받을 수 있는 기능 추가

### 1.4.0 (2017.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.4.0/GamebaseSDK-Android.zip)

#### 버그 수정
* [SDK] 1.4.0 업데이트
    * (Android)Gamebase 제공 팝업 창을 사용하지 않는 경우 이용정지 정보가 null로 리턴되는 오류 수정

### 1.3.0 (2017.10.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.3.0/GamebaseSDK-Android.zip)

#### 기능 추가
* [SDK] 1.3.0 업데이트
    * Credential을 이용한 AddMapping API추가

### 1.2.0 (2017.09.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.2.0/GamebaseSDK-Android.zip)

#### 기능 추가
* 이용정지(사용자처벌) 기능 추가
* [SDK] 1.2.0 업데이트
    * 이용정지 사용자 팝업 창 노출


### 1.1.5 (2017.07.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.5/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.5 업데이트
    * 시스템 팝업 창 API 추가 (showAlertWithTitle)
    * 국가코드를 대문자로 반환하도록 변경 (Android)
    * TCPush SDK 1.4.1 로 업데이트
    * IAP SDK 1.3.3.20170627 로 업데이트

### 1.1.4 (2017.05.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.4/GamebaseSDK-Android.zip)
#### 기능 개선/변경
* [SDK] 1.1.4 업데이트
    * 런타임 중 결제 Store를 변경할 수 있는 API 제공
    * (Android)TCPushSdk v1.4 적용, Tencent Push 기능 제공

### 1.1.3 (2017.04.20)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.3/GamebaseSDK-Android.zip)
#### 기능 개선/변경
* [SDK] 1.1.3 업데이트
    * (Android)론칭 구조 및 팝업 창/점검 페이지 개선 :커스텀 점검 페이지 설정 기능 추가
    * (Android)인증 구조 개선 및 로그 추가 : 인증 Adapter 및 SDK 버전 로그 출력

#### 버그 수정
* [SDK] 1.1.3 업데이트
    * (Android)Facebook SDK v4.19.0 이상에서 초기화시 크래시 오류 수정


### 1.1.2 (2017.04.04)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.2/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.2 업데이트
    * 게임론칭시 점검, 긴급공지 팝업 창 개선

### 1.1.0 (2017.03.21)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.1.0/GamebaseSDK-Android.zip)

#### 기능 개선/변경
* [SDK] 1.1.0 업데이트
    * 외부 AccessToken을 받아서 idPLogin을 해주는 인터페이스를 추가
    * [UI 기능 추가](./aos-ui) : Custom Webview, AlertDialog

### 1.0.0 (2017.03.09) 
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v1.0.0/GamebaseSDK-Android.zip)

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
    * 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
    * 로그아웃 및 회원탈퇴 기능을 제공
    * 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
    * 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
    * 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
    * TOAST Cloud상품 연동 : PUSH, IAP
