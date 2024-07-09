## Game > Gamebase > Upgrade Guide

## 2.65.0

### Common

* Gamebase SDK 2.65.0에서 이미지 공지 기능 사용 시 발생하는 문제를 수정하였습니다.
    * 표시할 이미지 공지가 없는 경우 오류 대신 성공 콜백이 호출되도록 변경하였습니다.
    * 등록된 이미지 공지가 없을 경우 빈 공지 화면이 노출되고 이 때 Android에서는 오늘은 그만보기를 체크한 뒤 닫으면 crash가 발생합니다.
    * 이슈가 해결된 Gamebase SDK 2.65.1 이상을 사용하세요.

### Android

* Google billing client version 6.2.1이 적용되어 Android OS 4.4(API Level 19) 단말기에서 결제하려면 추가 설정이 필요합니다.
    * 자세한 내용은 [Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > Gradle > Root level build.gradle](./aos-started/#root-level-buildgradle) 가이드를 참고하시기 바랍니다.

### iOS

* Facebook SDK가 17.0.1로 업데이트되면서 Dynamic Framework로 변경되었습니다.
    * Gamebase SDK를 다운로드하여 Xcode에 직접 설정하는 경우, Facebook SDK를 Embeded Frameworks에 추가해야 합니다.
    * 자세한 내용은 [Game > Gamebase > iOS SDK 사용 가이드 > 시작하기 > Setting > Xcode Settings](./ios-started/#xcode-settings) 가이드를 참고하시기 바랍니다.

## 2.64.0

### iOS

* Kakaogame 인증 최소 지원 버전이 12.0에서 13.0으로 변경되었습니다.

## 2.63.0

### iOS

* Facebook SDK가 17.0.0으로 업데이트되면서 Info.plist에 FacebookClientToken과 FacebookDisplayName을 추가해야 합니다.
```
<key>FacebookClientToken</key>
<string>{FACEBOOK_CLIENT_TOKEN}</string>
<key>FacebookDisplayName</key>
<string>{FACEBOOK_DISPLAY_NAME}</string>
```

### Unreal

* Android Firebase Notification 설정 방법이 변경되어 플러그인 내부에 google-services-json.xml 파일이 아닌 설정 툴에서 직접 지정하도록 변경되었습니다.
    * 기존에 제공되었던 Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml 파일이 제거되었습니다.
    * Firebase 콘솔에서 다운로드한 google-services.json 파일을 [Android 설정 툴](./unreal-started/#android-settings)의 Push 항목 내 FCM 하위에 있는 `GoogleServicesFilePath` 값을 설정합니다.

## 2.62.0

### Android

* Gamebase Android SDK 2.62.0은 Android 7.0(API Level 24) 미만 단말기에서 다음 이슈가 발생합니다. 
    * Gamebase.loginForLastLoggedInProvider 호출이 항상 실패합니다.
    * Guest 계정이 유실됩니다.
    * 이슈가 해결된 Gamebase Android SDK 2.62.1을 사용하세요.

### iOS
* Xcode 최소 지원 버전이 14.1에서 15로 변경되었습니다.
* Gamebase iOS 최소 지원 버전이 11.0에서 12.0으로 변경되었습니다.
* Gamebase와 Gamebase Adapter에 Privacy Manifest와 서명을 적용했습니다.
    * 2024년 5월 1일 이후 신규 출시 또는 업데이트를 하는 경우, Apple 정책에 따라 Gamebase iOS SDK 2.62.0 이상을 적용해야 합니다.
* LINE 인증 최소 지원 버전이 11.0에서 13.0으로 변경되었습니다.

### Unity

* Apple 개인정보 보호 정책을 준수하기 위한 대응 조치가 완료되었습니다.
    * Privacy Manifest 파일이 추가되었습니다.
    * Framework에 서명이 적용되었습니다.
    * 2024년 5월 1일 이후에는 새로운 출시나 업데이트를 위해 Apple 정책에 따라 Gamebase SDK for Unity 2.62.0 이상을 적용해야 합니다.

## 2.59.0

### iOS

* GamebaseAuthNaverAdapter에서 사용하는 NAVER iOS SDK가 xcframework로 변경되었습니다.

## 2.58.0

### Android

#### Twitter IdP
* Twitter API 서버의 인증서 업데이트로 minSdkVersion이 19에서 21로 상향되었습니다.

### iOS

* GamebaseAuthPaycoAdapter에서 사용하는 PAYCO iOS SDK가 xcframework로 변경되었습니다.

## 2.57.0

### iOS

* Privacy manifest 파일을 추가했습니다.
    * Privacy manifest 파일에서 Gamebase iOS SDK가 수집하는 데이터와 허용된 사유를 명시해야 하는 API 목록들을 볼 수 있습니다.
    * Apple 정책에 따라 2024년 봄까지 Gamebase iOS SDK 2.57.0 이상으로 업데이트해 주시기 바랍니다.

### Unreal
 
* Gamebase 모듈이 분리되었습니다. Gamebase 코드를 사용하려면 모듈의 Build.cs 파일 내 **GamebaseInterface** 모듈을 의존 모듈로 추가해야 합니다.

        PrivateDependencyModuleNames.AddRange(
            new[]
            {
                "Gamebase",
                "GamebaseInterface"
            }
        );

## 2.56.0

### Unreal
 
* 제공되는 타입이 USTRUCT에서 일반 구조체로 변경되었습니다.
    * 결과로 받는 타입은 기본적으로 제공되지 않는 값인 경우 TOptional 형태로 제공됩니다. 기존에 사용하던 값인 경우 Value.IsSet()를 이용해 설정된 값인지 확인한 뒤 Value.GetValue()를 통해 값을 사용할 수 있습니다.

## 2.55.0

### Android

#### Naver IdP
* Naver Login SDK의 업데이트로 minSDK가 19에서 21로 상향되었습니다.

#### MyCard Adapter
* NHN Cloud SDK의 업데이트로 minSDK가 19에서 21로 상향되었습니다.

## 2.54.0

### iOS

* Gamebase SDK가 xcframework로 변경되었습니다.
* Facebook iOS SDK가 14.1.0으로 업데이트되었습니다. Gamebase Console의 AdditionalInfo에 Facebook Client Token을 설정해 주시기 바랍니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > App > Authentication Information > 1. Facebook](./oper-app/#1-facebook)    

## 2.53.0

### Android

#### Contact

* '고객 센터' 기능을 사용하는 경우 첨부 파일 선택 시 권한 요청이 정상적으로 작동하도록 아래 가이드에 따라 AndroidManifest.xml에 권한 설정을 추가해야 합니다.
    * [Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > AndroidManifest.xml > Contact](./aos-started/#contact)

#### Line IdP

* 기존 ['시작하기' 문서](./aos-started)에서 Line IdP 사용 시 AndroidManifest.xml에 선언하도록 안내한 아래 내용은 Line SDK 업데이트로 인해 불필요해졌으므로 삭제하시기 바랍니다.

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

* Android 4.4(OS 19 Kitkat) 단말기에서 크래시가 발생합니다.
    * 이슈가 수정된 Gamebase Android SDK 2.52.1을 사용하시기 바랍니다.

### Unity

* '**IapOnestore**'로 표시되던 **ONE Store v17** 결제 어댑터가 Gamebase Setting Tool (v2.7.0)부터 '**IapOnestoreV17**'로 표시됩니다.

### iOS

#### Weibo IdP

* WeiboSDK가 3.3.3로 업데이트되면서 info.plist에 weibosdk3.3을 추가해야 합니다.
```
<key>LSApplicationQueriesSchemes</key>
<array>
    ...
    <string>weibosdk</string>
    <string>weibosdk2.5</string>
    <string>weibosdk3.3</string>
    ...
</array>
```

#### Changed/Deprecated APIs

* iOS 16.4부터 Apple이 CTCarrier class가 deprecated됨에 따라 아래 API들이 deprecated되었습니다.
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

* Android 4.4(OS 19 Kitkat) 단말기에서 크래시가 발생합니다.
    * 이슈가 수정된 Gamebase Android SDK 2.50.1을 사용하시기 바랍니다.

## 2.49.0

### Unreal

* 최소 지원 버전이 4.22에서 4.26으로 변경되었습니다.
* 미소비 내역 조회 API가 변경되어 신규 API로 변경해야 합니다.

        // Deprecated API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableReceiptListDelegate& onCallback);
        // New API
        void RequestItemListOfNotConsumed(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

* 활성화 구독 조회 API가 변경되어 신규 API로 변경해야 합니다.
    * 기존 API와 동일한 결과를 받으려면 **FGamebasePurchasableConfiguration.allStores**를 **true**로 설정해야 합니다.

            // Deprecated API
            void RequestActivatedPurchases(const FGamebasePurchasableReceiptListDelegate& onCallback);
            // New API
            void RequestActivatedPurchases(const FGamebasePurchasableConfiguration& Configuration, const FGamebasePurchasableReceiptListDelegate& onCallback);

### Android

```
최소 지원 버전이 Android 4.4 이상으로 상향되었습니다.(minSdk 16 -> 19)
```

## 2.47.0

### Android

* Unity에서 Proguard 적용 시 Purchase 관련 API 호출에 실패합니다.
    * 해당 이슈는 2.48.0에서 수정되었습니다.

## 2.45.0

### Android, iOS, Unity

* 미소비 내역 조회 API가 변경되어 신규 API로 변경해야 합니다.

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

* 활성화 구독 조회 API가 변경되어 신규 API로 변경해야 합니다.
    * 기존 API와 동일한 결과를 받으려면 **PurchasableConfiguration.allStores**를 **true**로 설정해야 합니다.

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

* Gamebase Setting Tool (v2.5.0)에 ONE Store v19 결제 어댑터가 추가되었습니다.
    * **SettingTool > Android** 설정에서 ONE Store v19 어댑터 활성화 시 iap_sdk-v19.xx.xx.aar 다운로드 페이지로 연결되며, 해당 파일을 **Assets > Plugins > Android** 폴더에 복사해야 합니다.

## 2.44.0

### Android

* Gamebase Android SDK 2.44.0에서 registerPush를 호출하면 Android 6.0(M, API Level 23) 단말기에서 크래시가 발생합니다.
    * 문제가 해결된 Gamebase Android SDK 2.44.1을 사용하세요.

## 2.43.3

### Unreal

* Google Billing Client 5.0.0 버전으로 변경되었습니다. Unreal에서 제공하는 Online SubSystem GooglePlay 플러그인 사용 시 /Config/Android/AndroidEngine.ini 파일에 해당 값을 추가해야 빌드 시 오류가 발생하지 않습니다.

            [OnlineSubsystemGooglePlay.Store]
            bUseGooglePlayBillingApiV2=False

## 2.42.1

### Unity
    
#### Changed/Deprecated APIs
* FGamebaseWebViewConfiguration에서 enableFixedFontSize 필드는 더 이상 지원하지 않습니다.
* GamebaseWebViewConfiguration 일부 필드에 기본값이 추가되어 값을 설정하지 않은 경우 기존과 다르게 동작할 수 있습니다.
    * 내비게이션 바의 색상 필드인 colorR, colorG, colorB, colorA의 기본값이 18, 93, 230, 255로 설정되었습니다.
    * 내비게이션 바 활성 여부를 지정하는 필드인 isNavigationBarVisible의 기본값이 true로 설정되었습니다.
    * 웹뷰 내 뒤로 가기 버튼 활성 여부를 지정하는 필드인 isBackButtonVisible의 기본값이 true로 설정되었습니다

### Unreal

* (iOS) [iOS 설정 툴](./unreal-started/#ios-settings)에서 Xcode의 경로를 변경할 수 있도록 **Xcode Path** 설정이 추가되었습니다.
    * 변경하지 않는 경우 기본값으로 설정됩니다(기본값: /Applications/Xcode.app).

#### Changed/Deprecated APIs
* FGamebaseConfiguration의 enableKickoutPopup 속성을 더 이상 지원하지 않습니다.
* FGamebaseConfiguration 일부 필드에 기본값이 추가되어 값을 설정하지 않은 경우 기존과 다르게 동작할 수 있습니다.
    * enableLaunchingStatusPopup의 기본값이 true로 설정되었습니다.
    * enableBanPopup의 기본값이 true로 설정되었습니다.
* FGamebaseWebViewConfiguration에서 enableFixedFontSize 필드는 더 이상 지원하지 않습니다.
* FGamebaseWebViewConfiguration 일부 필드에 기본값이 추가되어 값을 설정하지 않은 경우 기존과 다르게 동작할 수 있습니다.
    * 내비게이션 바의 색상 필드인 colorR, colorG, colorB, colorA의 기본값이 18, 93, 230, 255으로 설정되었습니다.
    * 내비게이션 바 활성 여부를 지정하는 필드인 isNavigationBarVisible의 기본값이 true로 설정되었습니다.
    * 웹뷰 내 뒤로 가기 버튼 활성 여부를 지정하는 필드인 isBackButtonVisible의 기본값이 true로 설정되었습니다.

## 2.41.0

### Android

* 이제 웹뷰에 등록한 커스텀 스킴 이벤트가 동작할 때 자동으로 웹뷰가 종료됩니다.
    * 이전과 같이 커스텀 스킴 이벤트가 동작하더라도 웹뷰를 유지하려면 **GamebaseWebViewConfiguration.Builder.enableAutoCloseByCustomScheme(false)** API를 호출하세요.
* Gamebase Android SDK 2.41.0에는 약관 창의 '보기' 버튼이 동작하지 않는 버그가 존재합니다.
    * Gamebase 약관 창을 사용하려면 문제가 해결된 Gamebase Android SDK 2.41.1을 사용하세요.

### Unity

* Gamebase SettingTool 필수 업데이트가 추가되었습니다. (v2.4.0)
    * 기존 SettingTool은 Unity 프로젝트에서 완전히 제거한 뒤 최신 버전으로 다시 설치해야 합니다.
    * SettingTool v1은 더 이상 지원하지 않습니다.

## 2.40.0

### Unreal

* 일부 API의 이름이 변경되었습니다.
    * FGamebaseAnalyticesLevelUpData → FGamebaseAnalyticsLevelUpData
    * FGambaseBanInfoPtr → FGamebaseBanInfoPtr
* (iOS) iOS 설정 툴을 제공하면서 빌드 시 필요한 프레임워크만 포함되도록 수정되었습니다.
* (iOS) PLCrashReporter가 업데이트되어 엔진 내부에 PLCrashReporter도 업데이트가 필요합니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Installation > iOS Settings > PLCrashReporter](./unreal-started/#ios-settings)
* (iOS) Facebook iOS SDK가 9.2.0버전으로 업데이트되어 swift 사용을 위해 엔진 코드 수정이 필요합니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Installation > iOS Settings > Facebook SDK](./unreal-started/#ios-settings)

## 2.36.0

### Android

#### Hangame SDK
* Hangame Android SDK v1.4.5에서 sms_hash가 내부에서 생성되도록 개선되었습니다.
    * 더 이상 sms_hash를 설정하지 않아도 됩니다. 

## 2.35.0

### Android

#### NAVER IdP

* 이제 NAVER 로그아웃 시 토큰을 삭제하지 않습니다.
    * 재로그인할 때 정보 제공 동의 창이 나타나지 않습니다.
    * 웹 로그인 시에는 계정이 변경되지 않습니다.
    * 이전 동작을 유지하려면 Gamebase Console의 AdditionalInfo에 다음과 같이 설정하세요.

```
{"logout_and_delete_token":true}
```

## 2.34.0

### Android

#### Changed/Deprecated APIs

* 킥아웃 팝업 표시 여부는 Gamebase 콘솔에서 킥아웃 등록시 설정할 수 있으므로 다음 필드가 deprecated 되었습니다.
    * **UIPopupConfiguration.enableKickoutPopup**

### iOS

#### Changed/Deprecated APIs

* 킥아웃 팝업 표시 여부는 Gamebase 콘솔에서 킥아웃 등록시 설정할 수 있으므로 아래 API들이 deprecated 되었습니다.
    * **[TCGBConfiguration enableKickoutPopup:]**
    * **[TCGBConfiguration isEnableKickoutPopup]**

### Unity

* GamebaseConfiguration의 enableKickoutPopup 속성을 더 이상 지원하지 않습니다.

## 2.33.0

### iOS

* TCGB_ERROR_UNKNOWN_ERROR 에러에 매핑된 오류 코드가 변경되었습니다.
    * TCGB_ERROR_UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 TCGB_ERROR_SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.

### Unity

* GamebaseErrorCode.UNKNOWN_ERROR 에러에 매핑된 오류 코드가 변경되었습니다.
    * GamebaseErrorCode.UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 GamebaseErrorCode.SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.

### Unreal

* GamebaseErrorCode.UNKNOWN_ERROR 에러에 매핑된 오류 코드가 변경되었습니다.
    * GamebaseErrorCode::UNKNOWN_ERROR 에러에 매핑된 오류 코드를 999에서 9999로 변경하였습니다.
    * 오류 코드 999에 매핑한 GamebaseErrorCode::SOCKET_UNKNOWN_ERROR 에러를 새로 추가하였습니다.

## 2.32.0

### Android

* Gamebase Access Token이 만료되어 복구되지 않을 때 발생하는 GamebaseEventHandler 이벤트 category가 **GamebaseEventCategory.OBSERVER_HEARTBEAT**에서 **GamebaseEventCategory.LOGGED_OUT**으로 변경되었습니다.
    * **GamebaseEventCategory.OBSERVER_HEARTBEAT** 이벤트에서 GamebaseEventObserverData.code 값이 **GamebaseError.AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO(3102)**일 때 로그인 하도록 구현했다면 **GamebaseEventCategory.LOGGED_OUT** 이벤트에서 로그인을 하도록 변경하시기 바랍니다.

## 2.29.0

### iOS

* Xcode 최소 지원 버전이 12에서 13으로 변경되었습니다.
    * Xcode 12에서 아카이브 빌드를 하면 에러가 발생합니다. Xcode 13으로 업데이트하시기 바랍니다.

### Unity
 
* Setting Tool 2.0.0이 배포되었습니다.
    * 폴더 구조가 변경되어, 이전 버전의 Setting Tool을 완전히 삭제한 후 재설치해야 합니다. 
    * Setting Tool 1.5.0 이하 사용자는 아래 디렉터리 있는 Gamebase 관련 라이브러리들을 모두 제거해야 합니다. 
        * **Assets/Plugins/Android**  
        * **Assets/Plugins/iOS** 
    * 변경된 내용 및 사용 방법은 아래 가이드를 확인하십시오. 
        * [Game > Gamebase > Unity SDK 사용 가이드 > 시작하기 > Specification of Setting Tool](./unity-started/#specification-of-setting-tool)
 
## 2.26.0

### Unity

* 해당 버전을 사용 시에는 **Assets/Gamebase/Toast/IAP/Plugins**를 직접 삭제한 후 사용하시기 바랍니다.
    * Gamebase Unity SDK 2.27.0 이상 버전이 적용된 경우에는 삭제할 필요가 없습니다.

### Unreal

* Gamebase에서 multidex 설정이 제거되었습니다. 설정 하시려면 아래 가이드를 참고 바랍니다.
    * [Game > Gamebase > Unreal SDK 사용 가이드 > 시작하기 > Installation > Android Settings > multidex 적용](./unreal-started/#android-settings)

## 2.25.0

### Android

#### Changed Minimum Support Version

* 최소 지원 Android Gradle Plugin(AGP) 버전이 2.3.0 에서 3.2.0 으로 변경되었습니다.
    * 하지만 targetSdkVersion 을 30 이상으로 설정하는 경우, Android 11 단말기 대응을 위해서는 AGP 3.3.3 이상이 필요합니다.
        * 다음 문서를 참고하시기 바랍니다.
        * [Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > Android 11](./aos-started/#android-11)
* 하위 버전의 AGP 지원이 필요하다면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### AndroidX

* Android Support Library 의존성이 AndroidX 로 변경되었으므로 Gradle 에 다음 변경사항을 적용하시기 바랍니다.

* gradle.properties 파일에 AndroidX 대응이 되어 있지 않은 라이브러리들을 위한 마이그레이션 선언을 추가하세요.

```groovy
# >>> [AndroidX]
android.useAndroidX=true
android.enableJetifier=true
```

* build.gradle 파일에 최신 AndroidX 를 위한 Java 8 빌드 설정을 추가하세요.

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

* Android Gradle Plugin 버전이 3.4.0 미만인 경우 빌드가 실패하므로 gradle.properties 파일에 다음 선언이 필요합니다.

```groovy
# gradle.properties
# >>> Fix for AGP under 3.4.0
android.enableD8.desugaring=true
android.enableIncrementalDesugaring=false
```

#### LINE IdP

* LINE IdP 를 사용하는 경우, LINE SDK 내부에 **&lt;queries&gt;** 태그가 존재하여 AGP 버전에 따라서는 빌드가 실패할 수 있습니다.
    * 다음 가이드를 참고하여 'queries' 태그 빌드가 가능한 AGP 버전으로 업그레이드 하시기 바랍니다.
    * [Game > Gamebase > Android SDK 사용 가이드 > 시작하기 > Setting > Android 11](./aos-started/#android-11)
* LINE IdP 를 사용하는 경우, LINE SDK 내부에 **android:allowBackup="false"** 로 선언되어 있어 애플리케이션 빌드시 Manifest merger 에서 fail 이 발생할 수 있습니다. 이렇게 빌드가 실패한다면 다음과 같이 application 태그에 **tools:replace="android:allowBackup"** 선언을 추가하시기 바랍니다.

```xml
<application
      tools:replace="android:allowBackup"
      ... >
```

### iOS

* Sign In with Apple 의 ASAuthorizationErrorUnknown 에러가 발생했을 경우, TCGB_ERROR_AUTH_EXTERNAL_LIBRARY_ERROR (3009) 에러를 반환하도록 변경되었습니다.

### Unity

* 해당 버전을 사용 시에는 **Assets/Gamebase/Toast/IAP/Plugins**를 직접 삭제한 후 사용하시기 바랍니다.
    * Gamebase Unity SDK 2.27.0 이상 버전이 적용된 경우에는 삭제할 필요가 없습니다.

#### Changed Minimum Support Version

* 최소 지원 Unity 버전이 2017.4.16 에서 2018.4.0 으로 변경되었습니다.
* 하위 버전의 Unity 지원이 필요하다면 [고객 센터](https://toast.com/support/inquiry)로 문의해 주시기 바랍니다.

#### AndroidX Build

* Gamebase Android SDK 의 AndroidX 이전으로 인해 Android 빌드시 다음 선언을 추가하시기 바랍니다.
* 2019.3 미만

```groovy
// mainTemplate.gradle
([rootProject] + (rootProject.subprojects as List)).each {
    ext {
        it.setProperty("android.useAndroidX", true)
        it.setProperty("android.enableJetifier", true)
    }
}
```

* 2019.3 이상

```groovy
// gradleTemplate.properties
android.useAndroidX=true
android.enableJetifier=true
```

#### Under AGP 3.4.0

* Unity Editor 버전이 2018.4.3 이하이거나 2019.1.6 이하인 경우, AGP 버전이 낮아서(3.2.0) 빌드가 실패하므로 다음 선언을 추가하세요.

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

* Gamebase Android SDK 의 AndroidX 이전으로 인해 Android 빌드시 UPL 에 다음 선언을 추가하시기 바랍니다.

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

* Gamebase iOS SDK 2.21.1 에서 bitcode 를 활성화 한 후에 **아카이브 빌드**를 하면 에러가 발생합니다.
    * bitcode 사용을 원하시는 경우엔 위 이슈가 해결된 Gamebase iOS SDK 2.21.2 를 사용하시기 바랍니다.

## 2.21.0

### Android

* Gamebase Android SDK 2.21.0 은 jCenter 에는 잘못된 빌드가 배포되어, **jcenter()** 를 **mavenCentral()** 보다 먼저 선언 했다면 모든 Gamebase API 에서 크래시가 발생할 수 있습니다.
    * 정상적으로 배포된 Gamebase Android SDK 2.21.1 을 사용하시거나 **mavenCentral()** 을 **jcenter()** 보다 먼저 선언하시기 바랍니다.
* Maven Repository
    * jCenter가 일반 사용자를 위한 서비스를 종료하여 ( [https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/](https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/) ) 더 이상 새로운 빌드는 업로드는 할 수 없게 되었습니다.(jCenter 의 접근 또한 2022 년 2월 1일에 종료됩니다.)
    * 그래서 Gamebase Android SDK 2.21.0 부터는 **jcenter()**가 아닌 **mavenCentral()** 에서만 배포가 되므로 gradle repository 에 mavenCentral 을 추가하세요.

```groovy
repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
}
```

#### LINE IdP

* LINE IdP 를 사용하는 경우, LINE SDK 업데이트로 인해 아래와 같이 Gradle 에 **JavaVersion.VERSION_1_8** 설정을 하지 않으면 빌드가 실패합니다.

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

* Gamebase iOS SDK 2.21.0 에서 bitcode 를 사용할 경우 에러가 발생합니다.
    * bitcode 사용을 원하시는 경우엔 Gamebase iOS SDK 2.21.1 을 사용하시기 바랍니다.

## 2.20.2

### iOS

#### Facebook IdP

* Gamebase iOS SDK 2.20.2 에서 Facebook SDK가 9.1.0으로 업데이트 되었습니다. 
    * Facebook SDK에 추가 설정이 필요하게 되어 info.plist에 아래의 값을 추가해 주시기 바랍니다. 설정하지 않을 경우 크래시가 발생할 수 있습니다.
        * FacebookAutoLogAppEventsEnabled
        * FacebookAdvertiserIDCollectionEnabled
* 자세한 내용은 [Facebook iOS SDK 가이드](https://developers.facebook.com/docs/app-events/getting-started-app-events-ios) 를 참고하시기 바랍니다.

## 2.19.0

### Android

#### Weibo IdP

* Gamebase Android SDK 2.19.0 에서 Weibo IdP 로그인과 다른 IdP 로그인을 번갈아가며 호출하는 경우 크래시가 발생합니다.
    * Weibo IdP 를 사용한다면 이슈가 수정된 Gamebase Android SDK 2.19.1 을 사용하시기 바랍니다.

## 2.18.2

### Android

#### Removed APIs

* Gamebase Android SDK 2.6.0 에서 deprecated 되었던 아래 함수들이 제거되었습니다.
    * **GamebaseConfiguration.Builder.setFCMSenderId()**
    * **GamebaseConfiguration.Builder.setTencentAccessKey()**
    * **GamebaseConfiguration.Builder.setTencentAccessId()**

## 2.18.0

### Android

#### Purchase Google

* Gamebase Android SDK 2.18.0 에서 Google 아이템 결제를 호출하면 크래시가 발생합니다.
    * 이슈가 수정된 Gamebase Android SDK 2.18.1 을 사용하시기 바랍니다.

## 2.17.0

### Android

* Gamebase Android SDK 2.17.0 에서 Gamebase.ImageNotice.showImageNotices API 를 호출하면 크래시가 발생합니다.
    * 2.17.0 의 크래시 및 OS 5.0~6.0 에서 커스텀 스킴 이벤트가 동작하지 않는 이슈가 수정된 Gamebase Android SDK 2.17.4 를 사용하시기 바랍니다.

## 2.15.1

### iOS

* SDK에서 정의한 타입 **GamebaseEventCategory**를 NSString 대신에 사용할 경우, 해당 타입을 **TCGBGamebaseEventCategory**로 수정해야 합니다.

## 2.15.0

### Android

#### Purchase Google

* **gamebase-adapter-purchase-google** 을 사용한다면 Gamebase SDK 2.15.0 미만 버전에서 2.15.0 이상으로 업그레이드 하는 경우 반드시 **이전 버전의 Game Client Version 을 업데이트 필수** 로 설정해야 합니다.
    * Google Billing Client 모듈이 업데이트되어, 여러개의 단말기에서 서로 다른 Billing Client 버전이 적용된 상태에서 아이템을 구매하는 경우 오류가 발생했을때 재처리에 문제가 생길 수 있기 때문입니다.

## 2.6.0

### Unity

#### Android Limitation

* Android Support Library 버전이 28.0.0으로 올라, Unity 5, Unity 2017.1, Unity 2017.2에서는 Android 빌드에 실패합니다.
	* Unity 2017.3 미만 버전의 에디터를 사용한다면 Unity 2017.3 이상 버전을 설치하고 'Editor/Data/PlaybackEngines/AndroidPlayer/Tools/gradle/lib' 폴더를 복사해 사용 중인 Unity Editor의 동일한 경로에 덮어씁니다. 그리고 mainTemplate.gradle 파일을 아래와 같이 수정하시기 바랍니다.

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

* Firebase Cloud Messaging 을 사용하는 경우, Firebase Console 에서 다운로드 받은 google-services.json 파일을 xml 리소스로 변환하여 프로젝트에 포함하여야 Push가 정상동작 합니다.
    * Gamebase 2.5.0 이전 버전에서는 xml 리소스가 없어도 Push가 동작했지만, Gamebase 2.6.0 이상부터는 xml 리소스가 반드시 필요합니다.
* 아래 가이드를 참고하여 구현하시기 바랍니다.
    * [\[Game > Gamebase > Android SDK 사용 가이드 > 푸시 > Settings > Firebase\]](./aos-push/#firebase)

#### Standalone

* Removed Japan Purchase
	* 일본 결제 서비스가 종료되었습니다.
	* GamebaseUnitySDK_IAPAdapter를 사용 중이면 아래 폴더를 직접 삭제해 주시기 바랍니다.
        * Asset/Toast/Common
        * Asset/Toast/Core
        * Asset/Toast/IAP
        * Asset/Toast/Standalone

### Android

#### Limitation

* minSdkVersion이 15(IceCreamSandwichMR1, 4.0.3)에서 16(Jelly Bean, 4.1)으로 변경되었습니다.
	* OS 4.1 미만의 단말기에서는 제대로 동작하지 않을 수 있으니 프로젝트의 minSdkVersion이 15인 경우, 16으로 변경해 주시기 바랍니다.

#### Removed APIs

* 제거된 함수는 다음과 같습니다. 대체 함수로 변경하시기 바랍니다.
    * **Gamebase.getAuthBanInfo()**가 제거되었습니다. **Gamebase.getBanInfo()**로 변경하세요.
    * **Gamebase.getLanguageCode()**가 제거되었습니다. **Gamebase.getDeviceLanguageCode()**로 변경하세요.
    * **new GamebaseConfiguration.Builder(void)**가 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.
    * **new GamebaseConfiguration.Builder.setAppId()**가 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.
    * **new GamebaseConfiguration.Builder.setAppVersion()**이 제거되었습니다. **GamebaseConfiguration.newBuilder()**로 변경하세요.

#### Changed/Deprecated APIs

* Gamebase.activeApp()은 자동으로 호출되므로 더 이상 호출하지 않아도 됩니다.
* Gamebase.initialize()의 인자로 필요한 GamebaseConfiguration의 생성 방법이 변경되었습니다.
    * **new GamebaseConfiguration.Builder(String, String)** 대신 **GamebaseConfiguration.newBuilder()**를 호출하세요.
* LaunchingStatus.isPlayable()은 더 이상 호출하지 마세요.
	* [\[Game > Gamebase > Android SDK 사용 가이드 > 초기화 > Launching Information\]](./aos-initialization/#launching-information) 문서를 참고하여 Launching Status Code에 따라 게임 플레이가 가능한지 여부를 직접 결정하시기 바랍니다.
* Purchase
    * Store Code는 변경할 수 없으므로 **GamebaseConfiguration.newBuilder()**에서 Store Code를 전달해야 합니다.
        * Gamebase.Purchase.getStoreCode()/Gamebase.Purchase.setStoreCode()는 제거될 예정입니다. 더 이상 사용하지 마세요.
    * Gamebase.Purchase.requestRetryTransaction()은 이제 호출하지 않아도 됩니다.
* Push
    * Gamebase Android SDK 2.6.0 이상부터는 푸시 메시지를 발송할 때, Gamebase 콘솔의 **푸시** 탭의 메뉴를 이용해 발송해야 합니다.
        * Gamebase Android SDK 2.6.0 미만이라면 Gamebase 콘솔의 **푸시(구)** 탭에서 푸시를 발송해야 합니다.
    * GamebaseConfiguration.Builder.setFCMSenderId()는 이제 호출하지 않아도 됩니다.
    * GamebaseConfiguration.Builder.setTencentAccessKey(), GamebaseConfiguration.Builder.setTencentAccessId()를 호출하는 경우 API 호출을 제거하고 build.gradle에 다음과 같이 선언해야 합니다.

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

* Setting Tool이 업데이트되었습니다.
    * 폴더 구조가 변경되어, 이전 버전의 Setting Tool을 완전히 삭제한 후 재설치해야 합니다.

## 2.2.2

### Unity

* GamebaseUnitySDKSettings 클래스의 **storeCodeAOS** 변수명이 **storeCodeAndroid**로 변경되었습니다.
    * **storeCodeAOS**를 참조하여 Store Code를 정의하는 코드나 Prefab이 있다면 변수 참조에 실패하므로 **storeCodeAndroid** 변수로 변경하시기 바랍니다.

## 2.2.0

### Unity

* GamebaseMainActivity의 Package Name이 변경되었습니다.
    * AndroidManifest.xml의 MainActivity 선언을 아래와 같이 변경하지 않으면 크래시가 발생합니다.
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

* 사용되지 않는 TransferKey 기능이 제거되었습니다.
	* Guest 계정 이전 기능이 필요하다면 Gamebase SDK 2.2.1부터 추가된 [\[Game > Gamebase > Unity SDK 사용 가이드 > 인증 > TransferAccount\]](./unity-authentication/#transferaccount) 기능을 이용하세요.