## Game > Gamebase > Android SDK 사용 가이드 > 시작하기

## Environments

Android에서 Gamebase를 사용하기 위한 시스템 환경은 다음과 같습니다.

> [최소 사양]
>
> * 사용자 실행 환경 : Android API 19 (KitKat, OS 4.4) 이상
> * 빌드 환경 : Android Gradle Plugin 3.2.0 이상
> * 개발 환경 : Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | 용도 | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk | nhncloud-core-1.9.1<br>nhncloud-common<br>nhncloud-crash-reporter-ndk<br>nhncloud-logger<br>gson-2.8.9<br>okhttp-3.12.13<br>kotlin-stdlib-1.8.0<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.6.4<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | Gamebase의 인터페이스 및 핵심 로직을 포함 | API 19(Kitkat, OS 4.4) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | Sign In With Apple 로그인을 지원 | - |
|  | gamebase-adapter-auth-facebook | facebook-login-16.1.2 | Facebook 로그인을 지원 | - |
|  | gamebase-adapter-auth-google | play-services-auth-20.3.0 | Google 로그인을 지원 | - |
|  | gamebase-adapter-auth-gpgs-v2 | play-services-games-v2-20.1.2 | GPGS(Google Play Games Services) V2 로그인을 지원 | API 21(Lollipop, OS 5.0) |
|  | gamebase-adapter-auth-hangame | hangame-id-1.13.0 | Hangame 로그인을 지원 | - |
|  | gamebase-adapter-auth-line | linesdk-5.8.1 | LINE 로그인을 지원 | - |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-5.8.0 | NAVER 로그인을 지원 | API 21(Lollipop, OS 5.0) |
|  | gamebase-adapter-auth-payco | payco-login-1.5.15 | PAYCO 로그인을 지원 | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | Twitter 로그인을 지원 | API 21(Lollipop, OS 5.0) |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-13.5.0 | Weibo 로그인을 지원 | - |
|  | gamebase-adapter-auth-weibo-v4 | openDefault-4.4.4 | Weibo 로그인을 지원 | - |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.19.3<br>kakaogame.gamesdk-3.19.3<br>kakaogame.common-3.19.3<br>kakao.sdk.v2-auth-2.17.0<br>kakao.sdk.v2-partner-auth-2.17.0<br>kakao.sdk.v2-common-2.17.0<br>play-services-ads-identifier-17.0.0 | Kakao 로그인을 지원 | API 23(Marshmallow, OS 6.0) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | nhncloud-iap-core | 게임 내 결제 지원 | - |
|  | gamebase-adapter-purchase-amazon | nhncloud-iap-amazon | Amazon Appstore를 지원 | - |
|  | gamebase-adapter-purchase-galaxy | nhncloud-iap-galaxy | Samsung Galaxy Store를 지원 | API 21(Lollipop, OS 5.0)<br>Galaxy IAP SDK의 minSdkVersion은 18이지만, 실제 결제를 위해 설치해야 하는 Checkout 서비스 앱의 minSdkVersion은 21입니다. |
|  | gamebase-adapter-purchase-google | billingclient.billing-5.0.0<br>nhncloud-iap-google | Google Play를 지원 | - |
|  | gamebase-adapter-purchase-huawei | nhncloud-iap-huawei | Huawei AppGallery를 지원 | - |
|  | gamebase-adapter-purchase-onestore | nhncloud-iap-onestore | ONE store v17을 지원 | - |
|  | gamebase-adapter-purchase-onestore-v19 | nhncloud-iap-onestore-v19 | ONE store v19를 지원 | - |
|  | gamebase-adapter-purchase-onestore-v21 | nhncloud-iap-onestore-v21 | ONE store v21을 지원 | API 23(Marshmallow, OS 6.0) |
|  | gamebase-adapter-purchase-onestore-external | nhncloud-iap-onestore-external | ONE store 외부 결제 기능을 지원 | - |
|  | gamebase-adapter-purchase-mycard | nhncloud-iap-mycard | MyCard 결제 기능을 지원 | API 21(Lollipop, OS 5.0) |
| Gamebase Push Adapters | gamebase-adapter-toastpush | nhncloud-push-analytics<br>nhncloud-push-core<br>nhncloud-push-notification | Push를 지원 | - |
|  | gamebase-adapter-push-adm | nhncloud-push-adm | Amazon Device Messaging을 지원 | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>nhncloud-push-fcm | Firebase Cloud Messaging을 지원 | - |


## Setting

### Console

> <font color="red">[주의]</font><br/>
>
> * NHN Cloud Console에서 새 프로젝트를 생성하여 Gamebase 서비스를 활성화했는지 반드시 확인하세요.
> * 각 IdP 콘솔에서 Client ID를 발급받아 Gamebase 콘솔에 입력했는지 꼭 확인하세요.

* Gamebase Android SDK를 사용하기 전에 NHN Cloud Console에서 앱 아이디를 발급받아야 합니다. 앱 아이디를 발급받으려면 NHN Cloud Console에서 **(+)서비스 선택**을 클릭한 다음 **Game > Gamebase**를 클릭하여 서비스를 활성화 합니다.
* 인증을 위해 IdP 콘솔에서 Client ID를 발급받아 Gamebase 콘솔에 입력합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Authentication Information](./oper-app/#authentication-information)
* 아이템 구매를 위해 Store 콘솔에서 앱 정보를 등록하여 Gamebase > 구매(IAP) 콘솔에 입력합니다.
    * [Game > Gamebase > 스토어 콘솔 가이드 > Google 콘솔 가이드](./console-google-guide)
    * [Game > Gamebase > 스토어 콘솔 가이드 > ONE Store 콘솔 가이드](./console-onestore-guide)
    * [Game > Gamebase > 스토어 콘솔 가이드 > GALAXY 콘솔 가이드](./console-galaxy-guide)
    * [Game > Gamebase > 스토어 콘솔 가이드 > Amazon 콘솔 가이드](./console-amazon-guide)
    * [Game > Gamebase > 스토어 콘솔 가이드 > Huawei 콘솔 가이드](./console-huawei-guide)
    * [Game > Gamebase > 스토어 콘솔 가이드 > MyCard 콘솔 가이드](./console-mycard-guide)
    * 아래 가이드를 참고하여 아이템을 등록합니다.
        * [Game > Gamebase > 콘솔 사용 가이드 > 결제 > Register](./oper-purchase/#register_1)
* 푸시 알림을 위해 푸시 알림 서비스 인증서를 Gamebase > 푸시 > 인증서 콘솔에 입력합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 푸시 > Authentication > Authentication register](./oper-push/#authentication)
* 새로운 Gamebase 프로젝트가 생성되었으니 AppVersion과 StoreCode를 등록해야 합니다.
    * 다음 가이드를 따라 새로운 클라이언트 버전을 등록하시기 바랍니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Client > Client List](./oper-app/#client-list)

### Register as Tester

#### Gamebase Test Device

* 점검중에도 정상적으로 게임에 접근하고자 한다면 Gamebase 콘솔에 테스트 단말기를 등록합니다.
    * [Game > Gamebase > 콘솔 사용 가이드 > 앱 > Test Device](./oper-app/#test-device)

#### Store's Tester

* 결제 테스트를 위하여 스토어별로 다음과 같이 테스터로 등록합니다.(Gamebase Tester 등록이 아닌, 스토어의 테스트 결제를 위한 설정입니다.)
    * Google Play Store
        * [Android > 테스트 구매 설정](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > SUPPORT > 개발도구 > (OLD Version) 인앱결제 가이드 > 원스토어 인앱결제 API V5 (SDK V17) 안내 및 다운로드 > 인앱결제 테스트 및 보안 > 테스트 ID 등록/관리](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v17/undefined-5#id-id)
        * [ONE store > SUPPORT > 개발도구 > (OLD Version) 인앱결제 가이드 > 원스토어 인앱결제 API V6 (SDK V19) 안내 및 다운로드 > 인앱결제 테스트 및 보안 > 테스트 ID 등록/관리](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v19/undefined-4#id-id)
        * [ONE store > SUPPORT > 개발도구 > 원스토어 인앱결제 API V7 (SDK V21) 안내 및 다운로드 > 03. 결제 테스트 및 보안 > 테스트 ID 등록/관리](https://onestore-dev.gitbook.io/dev/tools/tools/v21/03.#id-03.-id)
    * GALAXY Store
        * [Samsung Developers > Samsung IAP > Technical Documents > Test Guide > 3. IAP Testing > 3.2 Test Type > (3) Production Closed Beta Test](https://developer.samsung.com/iap/iap-test-guide.html)
        * [GALAXY store > 앱 > 등록한 앱 > 바이너리 > Beta Test > Tester 설정](https://seller.samsungapps.com/application)
        * 삼성 단말기에서만 결제 테스트가 가능합니다.
    * Amazon Appstore
        * [Amazon Appstore > In-App Purchasing > Test IAP Apps > IAP Testing Overview](https://developer.amazon.com/docs/in-app-purchasing/iap-testing-overview.html)
    * Huawei App Gallery
        * [Huawei Developers > HMS Core > App Services > In-App Purchases > Guides > Sandbox Testing](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/sandbox-testing-0000001050035039)

### Gradle

#### Using AndroidX

* AndroidX 사용 선언을 빌드 설정에 추가하세요.
    * Android Studio
        
            # gradle.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
        
    * Unity 2019.2 이하
            
            // mainTemplate.gradle
            ([rootProject] + (rootProject.subprojects as List)).each {
                ext {
                    // >>> [AndroidX]
                    it.setProperty("android.useAndroidX", true)
                    it.setProperty("android.enableJetifier", true)
                }
            }
            
    * Unity 2019.3 이상
            
            # gradleTemplate.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
            
    * Unreal
            
            <gradleProperties>
              <insert>
                android.useAndroidX=true
                android.enableJetifier=true
              </insert>
            </gradleProperties>
            
        
#### Under AGP 3.4.0

* Android Gradle Plugin 버전이 3.4.0 미만인 경우 빌드가 실패하므로 다음 선언이 필요합니다.
    
        # gradle.properties
        # >>> Fix for AGP under 3.4.0
        android.enableD8.desugaring=true
        android.enableIncrementalDesugaring=false
    
* Unity의 경우 Editor 버전이 2018.4.3 이하이거나, 2019.1.6 이하인 경우 이에 해당됩니다.(AGP 버전이 3.2.0)
        
        // mainTemplate.gradle
        ([rootProject] + (rootProject.subprojects as List)).each {
            ext {
                // >>> Fix for AGP under 3.4.0
                it.setProperty("android.enableD8.desugaring", true)
                it.setProperty("android.enableIncrementalDesugaring", false)
            }
        }
        
#### Root level build.gradle

* Google Play Billing Library(PBL) 6.x를 R8과 함께 사용하는 경우, Android 4.4(API 레벨 19)에서 동작하지 않는 문제가 발생할 수 있습니다.
    * Gamebase Android SDK 2.65.0부터 PBL 6.2.1을 사용합니다.
    * 이 문제를 해결하고 Android 4.4(API 레벨 19)를 지원하려면 프로젝트 수준(root level)의 build.gradle 또는 settings.gradle(AGP 7.1 이상)에 다음 선언을 추가하세요.

            buildscript {
                repositories {
                    // Raw R8 releases.
                    maven {
                        url("https://storage.googleapis.com/r8-releases/raw")
                    }
                }

                dependencies {
                    classpath("com.android.tools:r8:8.1.46")
                }
            }

* Huawei IAP를 사용하기 위해서 프로젝트 수준(root level)의 build.gradle 또는 settings.gradle(AGP 7.1 이상)에 다음 선언을 추가하세요.

        buildscript {
            repositories {
                ...
                // [Huawei App Gallery] Maven repository address for the HMS Core SDK.
                maven { url 'https://developer.huawei.com/repo/' }
            }

            dependencies {
                ...
                // [Huawei App Gallery] AppGallery Connect plugin configuration. please use the latest plugin version.
                classpath 'com.huawei.agconnect:agcp:1.6.0.300'
            }
        }

#### Define Adapters

* 사용할 Gamebase 버전, 사용할 인증, 결제, 푸시 모듈을 build.gradle 파일에 선언하세요.
    * Gamebase 최신 버전은 [Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/) 에서 확인할 수 있습니다.
    * **mavenCentral()** 저장소를 추가하세요.

```groovy
// >>> [Huawei App Gallery] agconnect plugin for huawei - when Native Android build
apply plugin: 'com.huawei.agconnect'

repositories {
    // >>> For Gamebase SDK
    mavenCentral()
    ...
    
    // >>> [Huawei App Gallery]
    maven { url 'https://developer.huawei.com/repo/' }

    // >>> [ONE store v21]
    maven { url 'https://repo.onestore.co.kr/repository/onestore-sdk-public' }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    // >>> Gamebase Version
    def GAMEBASE_SDK_VERSION = 'x.x.x'

    // >>> Gamebase - Add Auth Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-gpgs-v2:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-appleid:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v21:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-mycard:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"
    
    // >>> 다음 모듈의 사용 방법은 고객 센터로 문의 하시기 바랍니다.
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejp:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejpemail:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
    // >>> [Weibo v4]
    // https://github.com/nhn/toast.gamebase.android.sample/tree/main/weibo_sdk
    implementation files('libs/openDefault-4.4.4.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo-v4:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v16]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v16:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v17]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v19]
    // https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs
    implementation files('libs/iap_sdk-v19.01.00.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
    // >>> [Push Custom Receiver]
    implementation "com.toast.android.gamebase:gamebase-adapter-push-notification:$GAMEBASE_SDK_VERSION"
}

android {
    compileOptions {
        // >>> [AndroidX]
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        // >>> [Weibo IdP]
        ndk {
            abiFilters 'armeabi' // , 'armeabi-v7a', 'arm64-v8a'
        }
    }
}
```

### Resources

#### Weibo IdP

* 빌드 타깃에 따라 다음 URL의 so 파일들을 다운로드하여 프로젝트로 복사하세요.
    * https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so
* Android Studio 빌드인 경우
    * 프로젝트의 src/main/java/jniLibs 폴더 하위로 복사합니다.
    * ![Add so file to Android Studio project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-android-studio-2.53.0.png)
* Unity 빌드인 경우
    * so 파일 및 폴더를 Assets/Plugins/Android/libs 폴더 하위로 복사합니다.
    * ![Add so file to Unity project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-unity-2.53.0.png)

#### Huawei Store

* AppGallery Connection 구성 파일(agconnect-services.json)을 assets 폴더에 추가해야 합니다.
    * [AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)에 로그인 한 다음 **내 프로젝트**를 클릭합니다.
    * 프로젝트에서 앱을 선택합니다.
    * **Project settings** > **General information**으로 이동합니다.
    * **App information**에서 **agconnect-services.json** 파일을 다운로드합니다.
    * Android Studio 빌드인 경우
        * **agconnect-services.json** 파일을 프로젝트의 **assets** 폴더에 복사합니다.
    * Unity 빌드인 경우
        * **agconnect-services.json** 파일을 프로젝트의 **Assets/StreamingAssets** 폴더에 복사합니다.

#### Firebase Notification

* Android Studio 빌드인 경우
    * Firebase 푸시를 사용하려면 아래 가이드에 따라 Firebase 설정을 완료한 후 google-services.json 파일을 프로젝트에 포함시켜야 합니다.
        * [NHN Cloud > SDK 사용 가이드 > Push > Android > Firebase Cloud Messaging 설정](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* Unity 빌드인 경우
    * 'Firebase Console > 프로젝트 설정'에서 google-services.json 파일을 다운로드합니다. 그런 다음 json 파일을 xml 파일로 변환하기 위한 **[generate_xml_from_google_services_json.exe](https://github.com/firebase/firebase-cpp-sdk/blob/main/generate_xml_from_google_services_json.exe)** 파일을 다운로드하고, 아래 명령어를 실행하여 파일을 변환할 수 있습니다.
            
            "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
            
    * 변환한 xml 파일은 'Android 라이브러리 프로젝트'에 리소스로 추가해야 합니다.
        * 'Android 라이브러리 프로젝트'는 폴더명에 '.androidlib'를 포함해야 하고, AndroidManifest.xml 파일을 가지고 있어야 합니다.
            * [https://docs.unity3d.com/kr/2023.2/Manual/android-library-project-import.html](https://docs.unity3d.com/kr/2023.2/Manual/android-library-project-import.html)
        * 다음 예시 경로는 'Android 라이브러리 프로젝트'에 추가한 String 리소스 경로를 보여줍니다.
            * 'Assets/Plugins/Android/MyAndroidProject.androidlib/res/values/google-services.xml'

* Unreal 빌드인 경우
    * Unreal의 프로젝트의 [Gamebase Android 설정](./unreal-started/#android-settings)에서 `GoogleServicesFilePath`의 값을 Firebase 콘솔에서 다운로드한 `google-services.json`의 경로로 지정합니다.
    * Firebase와 관련하여 다른 플러그인을 사용할 때 google-services.json의 데이터를 Android 리소스로 만드는 과정이 있다면 Gamebase 처리하는 리소스 처리와 중복되어 빌드 중 오류가 발생할 수 있습니다. 이 경우에는 Gamebase Android 설정에서 `GoogleServicesFilePath`의 값을 비워 두면 Gamebase에서는 해당 JSON을 Android 리소스로 변환하는 작업을 진행하지 않습니다.
    
### AndroidManifest.xml

#### Contact

* 고객 센터 페이지([Game > Gamebase > Android SDK 사용 가이드 > ETC > Additional Features > Contact](./aos-etc/#contact))에서 문의글 작성 시 사진 및 미디어를 첨부하기 위해 Android API Level 21(OS 5.0) 이하 단말기에서는 저장소 읽기 권한 선언이 필요합니다.
        
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="21"/>
        
* Android API Level 22 이상의 단말기에서도 READ_EXTERNAL_STORAGE 권한이 필요하다면 **android:maxSdkVersion="21"** 구문을 제거하고 런타임 권한 요청을 구현해야 합니다.

#### Facebook IdP

* Facebook SDK 초기화를 위해 App ID와 Client Token을 선언합니다.
    * 해당 값을 직접 선언하지 말고 아래 예시와 같이 resources를 참조하도록 설정하세요.
    * App ID는 필수값이 아니지만 Client Token은 Facebook SDK v13.0부터 필수로 입력해야 로그인할 수 있습니다.
        * Client Token은 Facebook 개발자 사이트 > 설정 > 고급 설정 > 보안 항목에서 찾을 수 있습니다.

**AndroidManifest.xml**

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [Facebook] Configurations begin -->
        <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id" />
        <meta-data android:name="com.facebook.sdk.ClientToken" android:value="@string/facebook_client_token"/>
        <!-- [Facebook] Configurations end -->
        ...
    </application>
</manifest>
```

**res/values/strings.xml**

```xml
<resources>
    <!-- [Facebook] Facebook APP ID & Client Token -->
    <string name="facebook_app_id">123456789012345</string>
    <string name="facebook_client_token">a01234bc56de7fg89012hi3j45k67890</string>
</resources>
```

#### GPGS v2 IdP

* GPGS v2 SDK 초기화를 위해 App ID를 선언합니다.
    * 해당 값을 직접 선언하지 말고 아래 예시와 같이 resources를 참조하도록 설정하세요.

**AndroidManifest.xml**

```xml
<manifest ...>
    <application ...>
        ...
        <!-- [GPGS v2] Configurations begin -->
        <meta-data android:name="com.google.android.gms.games.APP_ID" android:value="@string/game_services_project_id" />
        <!-- [GPGS v2] Configurations end -->
        ...
    </application>
</manifest>
```

**res/values/strings.xml**

```xml
<resources>
    <!-- [GPGS v2] GPGS v2 APP ID -->
    <string name="game_services_project_id">1234567890</string>
</resources>
```

#### Weibo IdP

* Weibo IdP가 정상 동작 하기 위해서는 **application** 태그에 **android:networkSecurityConfig** attribute 를 추가하고, weibo, sina 관련 URL 을 선언한 xml 파일 이름을 설정해야 합니다.

```xml
<application android:networkSecurityConfig="@xml/my_network_security_config"
    ...>
    ...
</application>
```

* res/xml/my_network_security_config.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <!-- Weibo SDK start -->
        <domain includeSubdomains="true">sina.cn</domain>
        <domain includeSubdomains="true">weibo.cn</domain>
        <domain includeSubdomains="true">weibo.com</domain>
        <domain includeSubdomains="true">sinaimg.cn</domain>
        <domain includeSubdomains="true">sinajs.cn</domain>
        <domain includeSubdomains="true">sina.com.cn</domain>
        <!-- Weibo SDK end -->
    </domain-config>
</network-security-config>
```

#### ONE Store

* ONE store 는 전체 결제 화면과 팝업 결제 화면을 지원합니다.
    * AndroidManifest.xml에 meta-data를 추가하여 전체 결제 화면("full") 또는 팝업 결제 화면("popup")을 선택할 수 있습니다.
    * meta-data를 설정하지 않으면 기본값("full")이 적용됩니다.

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup:팝업 결제 화면 / full:전체 결제 화면 -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| 결제 화면 | 설정 값 |
| --- | --- |
| 전체 결제 화면 | "full" |
| 팝업 결제 화면 | "popup" |

#### Huawei Store

* Unity와 같은 multi platform 빌드 시 apply plugin 대신 아래의 내용을 추가하면 정상적인 결제가 가능합니다.
* agconnect-services.json의 cp_id, app_id 필드의 값을 AndroidManifest.xml의 meta-data에 입력하세요.

```xml
<meta-data  
    android:name="com.huawei.hms.client.appid"  
    android:value="appid=123456789">  
</meta-data>
<meta-data
    android:name="com.huawei.hms.client.cpid"
    android:value="cpid=1234567891234">
</meta-data>
```
주의: 사용자 단말에 Huawei App Gallery가 설치되어 있어야 정상적으로 결제가 가능합니다.

#### MyCard

* MyCard 결제를 연동하려면 GamebaseMyCardApplication을 사용해야 합니다. AndroidManifest.xml에 다음 내용을 추가하세요

```xml
<application
    android:name="com.toast.android.gamebase.purchase.mycard.GamebaseMyCardApplication"
  ...>
  ...
</application>

```

* 직접 Application을 정의하여 사용하는 경우 GamebaseMyCardApplication을 상속 받아 사용하세요.

```kotlin
class MyApplication: GamebaseMyCardApplication() {
    ...
}
```

* 결제 테스트를 하려면 AndroidManifest.xml에 'test_mode'를 추가하세요. 'test_mode'를 설정하지 않으면 기본값은 false입니다.
```xml
<application>
  <meta-data android:name="iap:test_mode" android:value="true | false"/>
</application>
```

#### Notification Options

* 다음과 같은 방법으로 알림 옵션을 설정할 수 있습니다.

**Example**

```xml
<!-- When you have multiple applications sharing an Gamebase project, use this field to identify each application. -->
<meta-data android:name="com.nhncloud.sdk.push.deviceId.salt"
           android:value="ApplicationForGoogleStore" />

<!-- Notification priority -->
<meta-data android:name="com.toast.sdk.push.notification.default_priority"
           android:value="1"/>
<!-- Notification background color -->
<meta-data android:name="com.toast.sdk.push.notification.default_background_color"
           android:resource="@color/defaultNotificationColor"/>
<!-- LED light -->
<meta-data android:name="com.toast.sdk.push.notification.default_light_color"
           android:value="#0000ff"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_on_ms"
           android:value="0"/>
<meta-data android:name="com.toast.sdk.push.notification.default_light_off_ms"
           android:value="500"/>
<!-- Small icon -->
<meta-data android:name="com.toast.sdk.push.notification.default_small_icon"
           android:resource="@drawable/ic_notification"/>
<!-- Sound -->
<meta-data android:name="com.toast.sdk.push.notification.default_sound"
           android:value="notification_sound"/>
<!-- Vibrate pattern -->
<meta-data android:name="com.toast.sdk.push.notification.default_vibrate_pattern"
           android:resource="@array/default_vibrate_pattern"/>
<!-- Use badge icon or not -->
<meta-data android:name="com.toast.sdk.push.notification.badge_enabled"
           android:value="true"/>
<!-- Enable notification when application is foreground. -->
<meta-data android:name="com.toast.sdk.push.notification.foreground_enabled"
           android:value="false"/>
```

| meta-data key | value type | description |
| ------------- | ---------- | ----------- |
| com.nhncloud.sdk.push.deviceId.salt | String | 서로 다른 애플리케이션이 하나의 Gamebase 프로젝트를 공유하는 경우, 푸시가 정상적으로 동작하지 않습니다.<br/>각각의 앱마다 서로 다른 임의의 'salt' 값을 지정해야 합니다. |
| com.toast.sdk.push.notification.default_priority | int | 우선 순위.<br/>아래 5가지 값을 설정할 수 있습니다.<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 배경색. |
| com.toast.sdk.push.notification.default_light_color | int | LED 색. |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED 불이 들어올 때의 시간. |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED 불이 나갈 때의 시간. |
| com.toast.sdk.push.notification.default_small_icon | resource id | 작은 아이콘의 리소스 식별자. |
| com.toast.sdk.push.notification.default_sound | String | 알림음 파일 이름.<br/>안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다. |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 진동의 패턴. |
| com.toast.sdk.push.notification.badge_enabled | boolean | 배지 아이콘 사용 여부. |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 포그라운드 알림 사용 여부. |

### Android 11

* Android 11 은 빌드시 미리 선언된 애플리케이션이 아니면 다른 애플리케이션이 실행되지 않습니다.
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* 이를 위해 targetSdkVersion 을 30 이상으로 설정하는 경우에는 반드시 AndroidManifest.xml 에 **queries** 태그를 통해 허용할 애플리케이션을 미리 선언해두어야 합니다.

> <font color="red">[주의]</font><br/>
>
> * 'queries' 태그는 기존 Android Gradle Plugin(AGP)에서는 인식하지 못하여 빌드가 실패합니다.
> * 아래 가이드 및 표를 참고해 'queries' 태그 빌드가 가능한 AGP 버전으로 업그레이드하시기 바랍니다.
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * AGP 3.2.* 이하의 버전을 사용한다면 3.3.3 이상으로 업그레이드해야 합니다.
>     * AGP 4.1.0 이상의 버전을 사용한다면 AGP 업그레이드는 하지 않아도 무방합니다.

| If you are using<br>the Android Gradle<br>plugin version... | ...upgrade to: | Unity Editor |
| --- | --- | --- |
| 4.1.* | N/A (no upgrade needed)| \- |
| 4.0.* | 4.0.1 | \- |
| 3.6.* | 3.6.4 | 2020.1 ~ |
| 3.5.* | 3.5.4 | \- |
| 3.4.* | 3.4.3 | 2018.4.4 ~<br>2019.1.7 ~ |
| 3.3.* | 3.3.3 | \- |
| 3.2.* | Not supported | 2017.4.17 ~<br>2018.3 ~ 2018.4.3<br>2019.1.0 ~ 2019.1.6 |
| 3.0.* | Not supported | 2018.2 |
| 2.3.* | Not supported | 2017.3 ~ 2017.4.16<br>2018.1 |
| 2.1.* | Not supported | Unity 5<br>2017.1 ~ 2017.2 |

```xml
<manifest>
    <!-- [Android11] settings start -->
    <queries>
        <!-- [All SDK] AppToWeb Authenthcation support start -->
        <package android:name="com.android.chrome" />
        <package android:name="com.chrome.beta" />
        <package android:name="com.chrome.dev" />
        <package android:name="com.sec.android.app.sbrowser" />
        <!-- [All SDK] AppToWeb Authenthcation support end -->
        
        <!-- [Facebook] Configurations begin -->
        <package android:name="com.facebook.katana" />
        <!-- [Facebook] Configurations end -->

        <!-- [PAYCO/Hangame] Configurations begin -->
        <package android:name="com.nhnent.payapp" />
        <!-- [PAYCO/Hangame] Configurations end -->

        <!-- [Hangame] Configurations begin -->
        <package android:name="com.nhn.hangameotp" />
        <package android:name="com.sci.siren24.ipin" />
        <package android:name="kr.co.samsungcard.mpocket" />
        <package android:name="com.lcacApp" />
        <package android:name="com.shcard.smartpay" />
        <package android:name="com.hyundaicard.appcard" />
        <package android:name="com.kbcard.cxh.appcard" />
        <package android:name="com.hanaskcard.paycla" />
        <package android:name="kvp.jjy.MispAndroid320" />
        <package android:name="nh.smart.nhallonepay" />
        <!-- [Hangame] Configurations end -->
        
        <!-- [NAVER] Configurations begin -->
        <package android:name="com.nhn.android.search" />
        <!-- [NAVER] Configurations end -->
        
        <!-- [Weibo] Configurations begin -->
        <package android:name="com.weico.international" />
        <package android:name="com.sina.weibo" />
        <!-- [Weibo] Configurations end -->

        <!-- [ONE store] Configurations begin -->
        <!-- Android 2.60.0 이상부터는 ONE store queries 선언이 필요하지 않습니다. -->
        <intent>
            <action android:name="com.onestore.ipc.iap.IapService.ACTION" />
        </intent>
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <data android:scheme="onestore" />
        </intent>
        <!-- [ONE store] Configurations end -->

        <!-- [Galaxy store] Configurations begin -->
        <package android:name="com.sec.android.app.samsungapps" />
        <!-- [Galaxy store] Configurations end -->
    </queries>
    
    <!-- [Amazon Appstore] Configuration begin -->
    <uses-permission
        android:name="android.permission.QUERY_ALL_PACKAGES"
        tools:ignore="QueryAllPackagesPermission" />
    <!-- [Amazon Appstore] Configuration end -->
    
    <!-- [Android11] settings end -->
</manifest>
```

* Amazon Appstore에서는 'queries' 요소 대신 **QUERY_ALL_PACKAGES** 권한을 추가합니다.

> <font color="red">[주의]</font><br/>
>
> * **QUERY_ALL_PACKAGES** 권한은 Amazon Appstore 전용 선언이므로 Google Play 빌드시에는 적용하지 않도록 주의하시기 바랍니다.

### Proguard

* Amazon Device Messaging
    * Amazon Device Messaging(ADM)에서 Proguard를 사용하는 경우, 다음 가이드를 확인하여 적용하셔야 합니다.
        * [NHN Cloud > SDK 사용 가이드 > Push > Android > Amazon Device Messaging 설정 > ADM SDK 다운로드](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-android/#adm-sdk)
        * [NHN Cloud > SDK 사용 가이드 > Push > Android > Amazon Device Messaging 설정 > Proguard 설정](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-android/#proguard)
* Gamebase 2.21.0 미만 버전은 Proguard 적용 시 Proguard Rule 에 다음 선언을 추가하지 않으면 결제 API 호출 시 크래시가 발생합니다.
    * Gamebase 2.21.0 버전에서 수정되었습니다.

            # ---------------------- [Gamebase TOAST IAP] defines start ----------------------
            # For using reflection
            -keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
            # ---------------------- [Gamebase TOAST IAP] defines end ----------------------


## Recommended Flow

* Gamebase에서 권장하는 flow는 Sample Project에도 동일하게 구현되어 있습니다.
    * Android Sample Project
        * [https://github.com/nhn/toast.gamebase.android.sample](https://github.com/nhn/toast.gamebase.android.sample)
            * app/src/main/java/com/toast/android/gamebase/sample/gamebase_manager 폴더의 kt 파일들을 참고하시면 됩니다.
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* 게임이 시작되었을 때 Gamebase 클라이언트 SDK를 초기화하고 로그인이 성공하면, 결제 재처리를 시작하고 푸시 토큰을 등록하세요.

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.30.1.png)

* 상세 flow 는 다음 링크에서 확인할 수 있습니다.
    * [Game > Gamebase > Android SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler](./aos-etc/#gamebase-event-handler)
    * [Game > Gamebase > Android SDK 사용 가이드 > 초기화 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK 사용 가이드 > 인증 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK 사용 가이드 > 결제 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)
    * [Game > Gamebase > Android SDK 사용 가이드 > 푸시 > Register Push](./aos-push/#register-push)

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/android)
* [Google APIs for Android](https://developers.google.com/android/guides/overview)
* [NAVER for developers](https://developers.naver.com/docs/login/android/)
* [Twitter Android Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Android Developer's guide - Authentication](https://developer.twitter.com/en/docs/authentication/overview)
* [LINE for developers](https://developers.line.biz/en/docs/android-sdk/integrate-line-login/)
* [PAYCO Login SDK for developers](https://developers.payco.com/guide/development/apply/android)
* [Sign in with Apple JS guide](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js)
* [Weibo for developers](https://github.com/sinaweibosdk/weibo_android_sdk/blob/master/2019SDK/文档)
* [Kakaogame SDK 3.0 Guide for Channeling](https://kakaogames.atlassian.net/wiki/spaces/KS3GFC/overview)

## API Reference

* API Reference는 SDK 내에 포함되어 있습니다.

## API Deprecate Governance

Gamebase에서 더 이상 지원하지 않는 API는 Deprecate 처리합니다.
Deprecated 된 API는 다음 조건 충족 시 사전 공지 없이 삭제될 수 있습니다.

* 5회 이상의 마이너 버전 업데이트
    * Gamebase Version Format - XX.YY.ZZ
        * XX : Major
        * YY : Minor
        * ZZ : Hotfix
* 최소 5개월 경과

