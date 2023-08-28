## Game > Gamebase > Android SDK使用指南 > 开始

## Environments

在Android上应用Gamebase时，需要以下系统环境。
 
> [最低版本]
>
> * 用户运行环境 : Android API 19 (KitKat, OS 4.4)以上
> * Build环境 : Android Gradle Plugin 3.2.0以上
> * 开发环境 : Android Studio

### Dependencies

| Gamebase SDK | Gamebase Adapter | External SDK | 用途 | minSdkVersion |
| --- | --- | --- | --- | --- |
| Gamebase | gamebase-sdk-base<br>gamebase-sdk | nhncloud-core-1.6.0<br>nhncloud-common<br>nhncloud-crash-reporter-ndk<br>nhncloud-logger<br>gson-2.8.9<br>okhttp-3.12.13<br>kotlin-stdlib-1.8.0<br>kotlin-stdlib-common<br>kotlin-stdlib-jdk7<br>kotlin-stdlib-jdk8<br>kotlin-android-extensions-runtime<br>kotlinx-coroutines-core-1.6.4<br>kotlinx-coroutines-android<br>kotlinx-coroutines-core-jvm | 包含Gamebase界面和核心逻辑。 | API 19(Kitkat, OS 4.4) |
| Gamebase Auth Adapters | gamebase-adapter-auth-appleid | - | 支持Sign In With Apple。 | - |
|  | gamebase-adapter-auth-facebook | facebook-login-16.1.2 | 支持Facebook登录。 | - |
|  | gamebase-adapter-auth-google | play-services-auth-20.3.0 | 支持Google登录。 | - |
|  | gamebase-adapter-auth-hangame | hangame-id-1.6.3 | 支持Hangame登录。 | - |
|  | gamebase-adapter-auth-line | linesdk-5.8.1 | 支持LINE 登录。 | - |
|  | gamebase-adapter-auth-naver | naveridlogin-android-sdk-4.4.1 | 支持Naver登录。 | - |
|  | gamebase-adapter-auth-payco | payco-login-1.5.12 | 支持PAYCO登录。 | - |
|  | gamebase-adapter-auth-twitter | signpost-core-1.2.1.2 | 支持Twitter登录。 | - |
|  | gamebase-adapter-auth-weibo | sinaweibosdk.core-13.5.0 | 支持Weibo登录。 | - |
|  | gamebase-adapter-auth-weibo-v4 | openDefault-4.4.4 | 支持Weibo登录。 | - |
|  | gamebase-adapter-auth-kakaogame | kakaogame.idp_kakao-3.14.14<br>kakaogame.gamesdk<br>kakaogame.common<br>kakao.sdk.v2-auth-2.11.0<br>kakao.sdk.v2-partner-auth<br>kakao.sdk.v2-common<br>play-services-ads-identifier-17.0.0 | 支持Kakao登录。 | API 21(Lollipop, OS 5.0) |
| Gamebase IAP Adapters | gamebase-adapter-toastiap | nhncloud-iap-core | 支持游戏内支付。 | - |
|  | gamebase-adapter-purchase-amazon | nhncloud-iap-amazon | 支持Amazon Appstore。 | - |
|  | gamebase-adapter-purchase-galaxy | nhncloud-iap-galaxy | 支持Samsung Galaxy Store。 | API 21(Lollipop, OS 5.0)<br>Galaxy IAP SDK的minSdkVersion是18, 而为了实际结算要安装的Checkout服务应用程序等的minSdkVersion是21。 |
|  | gamebase-adapter-purchase-google | billingclient.billing-5.0.0<br>nhncloud-iap-google | 支持Google Play。 | - |
|  | gamebase-adapter-purchase-huawei | nhncloud-iap-huawei | 支持Huawei AppGallery。 | - |
|  | gamebase-adapter-purchase-onestore | nhncloud-iap-onestore | 支持ONE store v17。 | - |
|  | gamebase-adapter-purchase-onestore-v19 | nhncloud-iap-onestore-v19 | 支持ONE store v19。 | - |
|  | gamebase-adapter-purchase-onestore-v21 | nhncloud-iap-onestore-v21 | 支持ONE store v21。 | API 23(Marshmallow, OS 6.0) |
|  | gamebase-adapter-purchase-onestore-external | nhncloud-iap-onestore-external | 支持ONE store外部支付功能。 | - |
|  | gamebase-adapter-purchase-mycard | nhncloud-iap-mycard | 支持MyCard支付功能。 | - |
| Gamebase Push Adapters | gamebase-adapter-toastpush | nhncloud-push-analytics<br>nhncloud-push-core<br>nhncloud-push-notification | 支持Push。 | - |
|  | gamebase-adapter-push-adm | nhncloud-push-adm | 支持Amazon Device Messaging。 | - |
|  | gamebase-adapter-push-fcm | firebase-messaging-17.6.0<br>nhncloud-push-fcm | 支持Firebase Cloud Messaging。 | - |


## Setting

### Console

> <font color="red">[注意]</font><br/>
>
> * 在NHN Cloud Console中创建新项目后，请确认是否启用了Gamebase服务。
> * 通过从各IdP控制台中获取Client ID确认是否在Gamebase控制台中输入了ID。 
 
* 在应用Gamebase Android SDK之前，您需要从NHN Cloud Console获取App ID。获取应用App ID，请在NHN Cloud Console上在单击**(+)服务**，然后点击**Game > Gamebase**以启用该服务。
* 验证时从IdP控制台获取client id，在Gamebase控制台中输入。 
    * [Game > Gamebase > 控制台使用指南 > App > Authentication Information](./oper-app/#authentication-information)
* 如果想购买道具，在Store控制台中注册App信息，在Gamebase > 购买(IAP)控制台中输入相关信息。
	* [Game > Gamebase > Store控制台指南 > Google控制台指南](./console-google-guide) 
	* [Game > Gamebase > Store控制台指南 > ONE store控制台指南](./console-onestore-guide) 
	* [Game > Gamebase > Store控制台指南 > GALAXY控制台指南](./console-galaxy-guide)
	* [Game > Gamebase > Store控制台指南 > Amazon控制台指南](./console-amazon-guide)
	* [Game > Gamebase > Store控制台指南 > Huawei控制台指南](./console-huawei-guide)
	* [Game > Gamebase > Store控制台指南 > MyCard控制台指南](./console-mycard-guide)
    * 注册道具时，请参考以下指南。
        * [Game > Gamebase > 控制台使用指南 > 结算 > Register](./oper-purchase/#register_1)
* 为了推送通知，要将推送通知服务认证书在Gamebase > 推送 > 认证书控制台中输入。
    * [Game > Gamebase > 控制台使用指南 > 推送 > Authentication > Authentication register](./oper-push/#authentication)
* 若新Gamebase项目已生成，需要注册AppVersion和StoreCode。
    * 注册新客户端版本时，请参考如下指南。 
    * [Game > Gamebase > 控制台使用指南 > App > Client > Client List](./oper-app/#client-list)

### Register as Tester

#### Gamebase Test Device

* 需要在维护中进入游戏时，在Gamebase控制台中注册测试终端机。
    * [Game > Gamebase > 控制台使用指南 > App > Test Device](./oper-app/#test-device)

#### Store's Tester

* 进行付费测试时，请按以下方式添加每个商店的测试人员。（不要在Gamebase设置Tester，请在Store控制台中设置。)
    * Google Play Store
        * [Android > 测试购买设置](https://developer.android.com/google/play/billing/billing_testing.html?hl=ko#billing-testing-test)
    * ONE store
        * [ONE store > SUPPORT > 开发道具 > (OLD Version) 应用程序内支付指南 > ONE store应用程序内支付API V5 (SDK V17) 指南和下载 > 应用程序内支付测试和保安> 测试ID 注册/管理](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v17/undefined-5#id-id)
        * [ONE store > SUPPORT > 开发道具 > (OLD Version) 应用程序内支付指南 > ONE store应用程序内支付API V6 (SDK V19) 指南和下载 > 应用程序内支付测试和保安 > 测试ID 注册/管理](https://onestore-dev.gitbook.io/dev/tools/tools/old-version/v19/undefined-4#id-id)
        * [ONE store > SUPPORT > 开发道具 > ONE store应用程序内支付API V7 (SDK V21) 指南和下载 > 03. 支付测试和保安 > 测试ID 注册/管理](https://onestore-dev.gitbook.io/dev/tools/tools/v21/03.#id-03.-id)
    * GALAXY Store
        * [Samsung Developers > Samsung IAP > Technical Documents > Test Guide > 3. IAP Testing > 3.2 Test Type > (3) Production Closed Beta Test](https://developer.samsung.com/iap/iap-test-guide.html)
        * [GALAXY store > App > 已注册的App > Binary > Beta Test > 设置Tester](https://seller.samsungapps.com/application)
        * 只能在三星终端机上进行付费测试。
    * Amazon Appstore
        * [Amazon Appstore > In-App Purchasing > Test IAP Apps > IAP Testing Overview](https://developer.amazon.com/docs/in-app-purchasing/iap-testing-overview.html)
    * Huawei App Gallery
        * [Huawei Developers > HMS Core > App Services > In-App Purchases > Guides > Sandbox Testing](https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides/sandbox-testing-0000001050035039)

### Gradle

#### Using AndroidX

* 请将AndroidX使用声明添加到打包设置中。
    * Android Studio
        
            # gradle.properties
            # >>> [AndroidX]
            android.useAndroidX=true
            android.enableJetifier=true
        
    * Unity 2019.2以下
            
            // mainTemplate.gradle
            ([rootProject] + (rootProject.subprojects as List)).each {
                ext {
                    // >>> [AndroidX]
                    it.setProperty("android.useAndroidX", true)
                    it.setProperty("android.enableJetifier", true)
                }
            }
            
    * Unity 2019.3以上
            
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

* Android Gradle Plugin版本低于3.4.0时将出现Build失败，因此需要添加以下声明。 
    
        # gradle.properties
        # >>> Fix for AGP under 3.4.0
        android.enableD8.desugaring=true
        android.enableIncrementalDesugaring=false
    
* 使用Unity时，如果Editor版本为2018.4.3或更低版本、2019.1.6或更低版本，则会出现Build失败，因此需要添加以下声明。(AGP版本为3.2.0)
        
        // mainTemplate.gradle
        ([rootProject] + (rootProject.subprojects as List)).each {
            ext {
                // >>> Fix for AGP under 3.4.0
                it.setProperty("android.enableD8.desugaring", true)
                it.setProperty("android.enableIncrementalDesugaring", false)
            }
        }

#### Root level build.gradle

* 为了使用Huawei IAP，请在项目水平(root level)的build.gradle或settings.gradle(AGP 7.1以上)中添加以下声明。

```groovy
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
```

#### Define Adapters

* 请在build.gradle文件中声明要使用的Gamebase版本和需要使用的验证、支付及推送模块。
	* Gamebase的最新版本可在[Maven Central(LINK)](https://repo1.maven.org/maven2/com/toast/android/gamebase/gamebase-sdk/)上确认。
	* 请添加**mavenCentral()**仓库。

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
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-facebook:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-appleid:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-twitter:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-naver:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-line:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-payco:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Purchase Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-google:$GAMEBASE_SDK_VERSION"
   // >>> [ONE store v17]
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-external:$GAMEBASE_SDK_VERSION"
    // >>> [ONE store v19]
    // https://github.com/ONE-store/onestore_iap_release/tree/iap19-release/android_app_sample/app/libs
    implementation files('libs/iap_sdk-v19.00.02.aar')
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v19:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v21:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-galaxy:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-amazon:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-huawei:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-mycard:$GAMEBASE_SDK_VERSION"

    // >>> Gamebase - Select Push Adapter
    implementation "com.toast.android.gamebase:gamebase-adapter-push-fcm:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-push-adm:$GAMEBASE_SDK_VERSION"

    // >>> 关于下一个模块儿的使用方法，请参考客户服务。
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejp:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-hangamejpemail:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-kakaogame:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-auth-weibo-v4:$GAMEBASE_SDK_VERSION"
    implementation "com.toast.android.gamebase:gamebase-adapter-purchase-onestore-v16:$GAMEBASE_SDK_VERSION"
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

* 根据Build Targrt，从以下URL下载so文件并将其复制到项目中。
    * https://github.com/sinaweibosdk/weibo_android_sdk/tree/master/so
* 如果为Android Studio Build
    * 将其复制到项目的src/main/java/jniLibs文件夹的子文件夹。
    * ![Add so file to Android Studio project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-android-studio-2.53.0.png)
* 如果为Unity 
    * 将so文件和文件夹复制到Assets/Plugins/Android/libs文件夹的子文件夹。
    * ![Add so file to Unity project](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-started-resources-weibo-so-unity-2.53.0.png)

#### Huawei Store

* AppGallery Connection配置文件（agconnect-services.json）必须添加到assets文件夹中。
    * 登录[AppGallery Connect](https://developer.huawei.com/consumer/en/service/josp/agc/index.html)后单机**我的项目**。
    * 在项目中选择应用程序。
    * 移动至**Project settings** > **General information**。
    * 在**App information**中下载**agconnect-services.json**文件。
    * 如果为Android Studio Build时
        * 将**agconnect-services.json**文件复制到项目的**assets**文件夹中。 
    * 如果为Unity Build时
        * 将**agconnect-services.json**文件复制到项目的**Assets/StreamingAssets**文件夹中。

#### Firebase Notification

* 使用Android Studio Build时
    * 使用Firebase推送功能时，请参考以下指南设置Firebase之后，将google-services.json文件添加在项目中。 
        * [NHN Cloud > SDK使用指南 > Push > Android > 设置Firebase Cloud Messaging](/TOAST/ko/toast-sdk/push-android/#firebase-cloud-messaging)
* 如果使用Unity Build
     * **注意** : 不必要安装Firebase Unity SDK Package , 即使不安装，推送也将正常启动。
    * 如果已设置Firebase Unity SDK Package，则使用以下命令执行**generate_xml_from_google_services_json.exe**文件，将json文件转换为xml文件。
            
            "{UnityProject}\Firebase\Editor\generate_xml_from_google_services_json.exe" -i "{JsonFilePath}\google-services.json" -o "{UnityProject}\Assets\Plugins\Android\res\values\google-services.xml" -p "{PackageName}"
            
    * 如果未设置Firebase Unity SDK Package，则需在“Firebase Console > 项目设置”中下载google-services.json文件，按照如下指南制作string resource(xml)文件后添加在“Assets/Plugins/Android/res/values/”文件夹。
        根据Firebase服务联动，google-services.json文件的内容可能会有所不同。
        ![Download google-services.json](https://static.toastoven.net/prod_gamebase/DevelopersGuide/aos-developers-guide-push_001_1.13.0.png)
        * [Google Service Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin#processing_the_json_file)
        * 以下为string resource(xml)文件的示例。

```xml
<!-- Assets/Plugins/Android/res/values/google-services-json.xml -->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="firebase_database_url" translatable="false">https://gamebase-sample-00000000.firebaseio.com</string>
    <string name="gcm_defaultSenderId" translatable="false">000000000000</string>
    <string name="google_storage_bucket" translatable="false">gamebase-sample-00000000.appspot.com</string>
    <string name="project_id" translatable="false">gamebase-sample-00000000</string>
    <string name="google_api_key" translatable="false">AbCd_AbCd_AbCd_AbCd_AbCd_AbCd_AbCd</string>
    <string name="google_app_id" translatable="false">1:000000000000:android:abcd0123abcd0123</string>
    <string name="default_web_client_id" translatable="false">000000000000-abcdabcdabcdabcdabcdabcdabcd.apps.googleusercontent.com</string>
</resources>
```

* 如果是Unreal Build，当前在发布包含空google-service-json.xml文件的Gamebase Unreal SDK，因此需要修改为符合相关游戏信息的值。
    * 如果有自动生成类似于EasyFirebase格式的XML的Content，因资源的重复，可能出现Build错误。这时需要删除google-service-json.xml文件。

### AndroidManifest.xml

#### Contact

* 客户服务页面([Game > Gamebase > Android SDK使用指南 > ETC > Additional Features > Contact]当在(./aos-etc/#contact))中输入您要咨询的问题时，需要声明存储读取权限才能添加照片和多媒体内容。

        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

* 如果您的应用程序目标为Android 13（API Level 33）或更高版本，除了读取存储权限外，您还需要以下细粒度媒体权限声明。

        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />
        <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
        <uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />

* 如果声明了权限，Gamebase SDK会在文件上传时自动请求运行时权限。

#### Facebook IdP

* 初始化Facebook SDK时，需要声明App ID和Client Token。
    * 如下列所示，若设置方式是可以参考resources的，则不必将其值直接声明。
    * App ID不是需要值，但从Facebook SDK v13.0开始，您必须输入Client Token才能成功登录。
        * 您可以在Facebook开发人员网站 > 设置 > 高级设置 > 安全项目中查找Client Token。

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

#### Weibo IdP

* 为了使Weibo IdP正常启动，需要在**application**标签中添加**android:networkSecurityConfig** attribute，并需要设置声明为与weibo和sina相关的URL的xml文件名。 

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

* ONE store支持支付页面和支付页面弹窗。
    * 通过在AndroidManifest.xml添加meta-data，可选择支付页面("full")或支付页面弹窗("popup")。
    * 如果不设置meta-data，则适用默认值("full")。

```xml
<manifest>
    ...
    <application>
        ...
        <!-- [ONE store] Configurations begin -->
        <!-- popup : 支付页面弹窗 / full : 支付页面 -->
        <meta-data
            android:name="iap:view_option"
            android:value="popup | full" />
        <!-- [ONE store] Configurations end -->
        ...
    </application>
</manifest>
```

| 支付页面 | 设定值 |
| --- | --- |
| 支付页面 | "full" |
| 支付页面弹窗 | "popup" |

#### Huawei Store    

* 如果您在构建像Unity的multi platform时添加以下信息而不是apply plugin，则可正常付款。
* 请在AndroidManifest.xml的meta-data中输入agconnect-services.json的cp_id, app_id字段的值。

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
注意 ：只有在用户终端上安装了Huawei App Gallery，才能正常付款。

#### MyCard

* 对于MyCard支付集成，必须使用GamebaseMyCardApplication。
请在AndroidManifest.xml中添加以下信息。 

```xml
<application
    android:name="com.toast.android.gamebase.purchase.mycard.GamebaseMyCardApplication"
  ...>
  ...
</application>

```

* 如果您已经直接定义并正在使用应用程序，请继承并使用GamebaseMyCardApplication。

```kotlin
class MyApplication: GamebaseMyCardApplication() {
    ...
}
```

* 如果您要进行付费测试，请在AndroidManifest.xml中添加“test_mode”。如果不设置“test_mode”，基本值将false。
```xml
<application>
  <meta-data android:name="iap:test_mode" android:value="true | false"/>
</application>
```

#### Notification Options

* 请按以下方式设置Notification Options。

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
| com.nhncloud.sdk.push.deviceId.salt | String | 如果不同的应用程序连接于一个Gamebase项目时，有可能推送不正常启动。<br/>要对每个应用程序分别指定任意的“salt”值。 |
| com.toast.sdk.push.notification.default_priority | int | 优先顺序<br/>可设置以下5个值。<br/>NoticationComapt.PRIORITY_MIN : -2<br/> NoticationComapt.PRIORITY_LOW : -1<br/>NoticationComapt.PRIORITY_DEFAULT : 0<br/>NoticationComapt.PRIORITY_HIGH : 1<br/>NoticationComapt.PRIORITY_MAX : 2 |
| com.toast.sdk.push.notification.default_background_color | int | 背景色 |
| com.toast.sdk.push.notification.default_light_color | int | LED色 |
| com.toast.sdk.push.notification.default_light_on_ms | int | LED灯亮的时间 |
| com.toast.sdk.push.notification.default_light_off_ms | int | LED灯灭的时间 |
| com.toast.sdk.push.notification.default_small_icon | resource id | 小图标的资源标识符 |
| com.toast.sdk.push.notification.default_sound | String | 提示音文件名<br/>只在安卓8.0以下OS上运行。<br/>如果指定“res/raw”文件夹的mp3、wav文件名，则可更改提示音。 |
| com.toast.sdk.push.notification.default_vibrate_pattern | long[] | 震动模式 |
| com.toast.sdk.push.notification.badge_enabled | boolean | 是否使用badge图标 |
| com.toast.sdk.push.notification.foreground_enabled | boolean | 是否使用foreground通知 |

### Android 11

* Android 11 Build时，只能运行在AndroidManifest.xml中提前声明的应用程序。
    * [https://developer.android.com/about/versions/11/privacy/package-visibility](https://developer.android.com/about/versions/11/privacy/package-visibility)
* 为此，如果要将targetSdkVersion设置为30以上，通过**queries**标签必须在AndroidManifest.xml中声明允许运行的应用程序。

> <font color="red">[注意]</font><br/>
>
> * 如果以前的Android Gradle Plugin(AGP)认知不到“queries”标签，将出现Build失败。
> * 通过参考以下指南和列表，请升级到可进行“queries”标签Build的AGP版本。 
>     * [https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html](https://android-developers.googleblog.com/2020/07/preparing-your-build-for-package-visibility-in-android-11.html)
>     * 如果使用AGP 3.2.*以下版本，请升级到3.3.3以上。
>     * 如果使用AGP 4.1.0以上版本，则不用进行AGP升级。

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

        <!-- [Naver] Configurations begin -->
        <package android:name="com.nhn.android.search" />
        <!-- [Naver] Configurations end -->
        
        <!-- [Weibo] Configurations begin -->
        <package android:name="com.weico.international" />
        <package android:name="com.sina.weibo" />
        <!-- [Weibo] Configurations end -->

        <!-- [ONE store] Configurations begin -->
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

* 在Amazon Appstore添加**QUERY_ALL_PACKAGES**权限，而不是 “queries”因素。 

> <font color="red">[注意]</font><br/>
>
> * **QUERY_ALL_PACKAGES**权限是专用于Amazon Appstore的声明，因此注意进行Google Play  Build时不应用。  

### Proguard

* Amazon Device Messaging
    * 在Amazon Device Messaging(ADM)使用Proguard时，请参考以下指南后应用。
        * [NHN Cloud > SDK使用指南> Push > Android > 设置Amazon Device Messaging > 下载ADM SDK ](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-android/#adm-sdk)
        * [NHN Cloud > SDK使用指南 > Push > Android > 设置Amazon Device Messaging > 设置Proguard ](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-android/#proguard)   
* 将Proguard应用于Gamebase 2.21.0或低于2.21.0的版本时，若调用结算API而不添加Proguard Rule，则将发生崩溃。
    * 在Gamebase 2.21.0版本上已被修改。

            # ---------------------- [Gamebase TOAST IAP] defines start ----------------------
            # For using reflection
            -keep class com.toast.android.toastgb.iap.ToastGbStoreCode { *; }
            # ---------------------- [Gamebase TOAST IAP] defines end ----------------------


## Recommended Flow

* 可在Sample Project上确认Gamebase推荐的flow。
    * Android Sample Project
        * [https://github.com/nhn/toast.gamebase.android.sample](https://github.com/nhn/toast.gamebase.android.sample)
            * 请参考app/src/main/java/com/toast/android/gamebase/sample/gamebase_manager文件夹的kt文件。 
    * Unity Sample Project
        * [https://github.com/nhn/toast.gamebase.unity.sample](https://github.com/nhn/toast.gamebase.unity.sample)
* 当您启动游戏、初始化Gamebase客户端SDK并成功登录时，请进行“支付再处理”并注册推送令牌。

![overview flow](https://static.toastoven.net/prod_gamebase/DevelopersGuide/overview_flow_2.30.1.png)

* 通过以下链接查看具体的Flow。
    * [Game > Gamebase > Android SDK 使用指南 > ETC > Additional Features > Gamebase Event Handler](./aos-etc/#gamebase-event-handler)
    * [Game > Gamebase > Android SDK 使用指南 > 初始化 > Initialization Flow](./aos-initialization/#initialization-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 认证 > Login Flow](./aos-authentication/#login-flow)
    * [Game > Gamebase > Android SDK 使用指南 > 结算 > Retry Transaction Flow](./aos-purchase/#retry-transaction-flow)
    * [Game > Gamebase > Android SDK 使用指南 > Push > Register Push](./aos-push/#register-push)

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

* API Reference已被包含在SDK中。

## API Deprecate Governance

对Gamebase已不再支持的API，进行Deprecate处理。
满足以下条件时，可以删除Deprecated的API，不另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix
* 至少5个月
