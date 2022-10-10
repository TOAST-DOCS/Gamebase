  ## Game > Gamebase > Unity SDK使用指南 > 开始

以下说明Gamebase Unity SDK使用环境和初始设置。

## Environments

> [参考] 
>
> Gamebase支持的Unity版本
>
> * 2018.4.0 ~ 2021.3.3
> * 如果需要Gamebase支持的低版本Unity，请联系[客户服务](https://toast.com/support/inquiry)。

#### Android
> <font color="red">[注意]</font>
>
> 2019年8月1日起Google Play公告的新应用程序应支持64位架构。
> [确认Google Play政策及支持64bit Unity版本](https://developer.android.com/games/optimize/64-bit?#unity-developers)

#### Dependencies

| Gamebase SDK | External SDK |
| --- | --- |
| Gamebase | NHN Cloud Unity SDK 0.26.2 |

* [Gamebase Android SDK - Dependencies](./aos-started/#dependencies)
* [Gamebase iOS SDK - Dependencies](./ios-started/#setting)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows7以上
	* 不支持MAC OS。
* WebGL
    * [WebGL Browser Compatibility](https://docs.unity3d.com/Manual/webgl-browsercompatibility.html)
* Editor
    * 只支持部分功能。

调用所选平台不支持的Gamebase API时，将返回以下错误作为回调。如果没有回调，则输出Warning日志。

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR

各API支持的平台由以下图标标识。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#F9D0C4; font-size: 10pt">■</span> UNITY_STANDALONE
<span style="color:#5319E7; font-size: 10pt">■</span> UNITY_WEBGL
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR

## Installation

为了轻松安装Gamebase SDK，提供Setting Tool。

* [Download Gamebase Unity SDK](/Download/#game-gamebase)

### Specification of Setting Tool

1. 下载SDK
    * 支持下载最新版本SDK。 
2. 安装SDK
    * 支持安装下载的SDK。 
        * Unity : Unitypackage
        * Android : Gradle
        * iOS : CocoaPods
3. 删除SDK
    * 支持删除SDK。 
4. 更新SDK
    * 不支持更新功能。
    * 删除SDK后，通过重新设置来取代更新功能。

### Using the Setting Tool

* 发布了Gamebase SettingTool **v2.0.0**。 
    * 与v1.5.0不兼容，请删除后使用v2.0.0以上版本。 

**AS-IS**

1. 在Unity项目中进行打包时包括适用于Android和iOS的Gamebase SDK。
2. 不支持Gradle和CocoaPods。

**TO-BE**

1. 支持Gradle和CocoaPods。
2. 将EDM4U(External Dependency Manager for Unity)使用为必要的库。
    * 在[EDM4U Github](https://github.com/googlesamples/unity-jar-resolver)中下载EDM4U后进行设置。 
    * 如果没有EDM4U则无法设置Gamebase SDK for Android和iOS。
    * 当使用已经包含EDM4U的SDK，如Facebook、GPGS SDK和Firebase，不需要下载EDM4U。
3. 在为Android平台提供服务时，请选择上方菜单 > **Assets > External Dependency Manager > Android Resolver > Settings**，打开Android Resolver Settings窗口，按如下方式进行设置。 
    * Enable Auto-Resolution : 不启用
    * Explode AARs : 不启用
    * Patch mainTemplate.gradle : 启用
    * Use Jetifier : 启用
    * ![Android Resolver Settings](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-edm4u-settings-1_2.0.0.png)
4. 在为iOS平台提供服务时，选择上方菜单 > **Assets > External Dependency Manager > iOS Resolver > Settings**，打开iOS Resolver Settings窗口，按如下方式进行设置。 
    * Use Shell to Execute Cocoapod Tool : 不启用
        * 如果此功能已被激活，在Unity打包iOS时将出现无法生成xcworkspace的错误。(CocoaPods 1.11.x程序错误)
        * 需要激活此功能的用户，请用以下2种方法之一解决错误。
            * 设置CocoaPods 1.10.x版本。
            * 在Unity生成的Xcode项目中直接调用**pod install**。

> <font color="red">[注意]</font>
>
> 在为iOS平台提供服务时，需要安装CocoaPods。关于安装CocoaPods和具体的说明，请参考[cocoapods.org](https://cocoapods.org/)。

#### 安装SDK

1. 打开Unity项目。  
2. 导入GamebaseUnitySettingTool_{version}.unitypackage。 
3. 选择上方菜单 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**。
4. 在SDK Download项目中点击[Gamebase SDK]按钮下载最新SDK。
5. 选择要使用的平台。
    * Android
    * iOS
6. 选择各平台将使用的模块儿。
    * authentication支持与Google等ID Provider(以下简称IDP)进行交互。
    * push支持FCM(Firebase)和APNS Push服务。
    * pruchase使用NHN Cloud结算服务IAP(In-App Purchase)支持付款。
7. 点击[Settings]按钮安装SDK。
8. 如果选择了Android和iOS模块儿，则要执行EDM4U的resolve。
    * Android : 选择上方菜单 > **Assets > External Dependency Manager > Android Resolver > Force Resolve**。
    * iOS : 选择上方菜单 > **Assets > External Dependency Manager > iOS Resolver > install Cocoapods**。

> <font color="red">[注意]</font>
>
> 如果没有EDM4U，则不设置Gamebase SDK for Android和iOS。<br/>
> 在执行EDM4U的resolve之前，要在**Build Settings**窗口中点击Switch Platform按钮转为将要打包的平台。如果Android平台已被选择，则在**Player Settings > Publishing Settings**中启用Custom Gradle Template创建mainTemplate.gradle文件。<br/>
> 如果使用“Unity 2019.3”以上，则需在**Player Settings > Publishing Settings**中启用Custom Gradle Properties Template创建gradleTemplate.properties文件。


#### 更新SDK

1. 选择上方菜单 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**。
2. 在**SDK Download**项目中点击[Gamebase SDK]按钮下载最新SDK。
    * 最新SDK已被下载时相关按钮将被激活。 
3. 点击[Settings]按钮安装SDK。
    * 可以更改之前选择的平台模块。


#### 删除SDK

1. 选择上方菜单 > **Tools > NhnCloud > Gamebase > SettingTool > Settings**。
2. 请点击[Remove]按钮删除安装的SDK。

> [参考]
> 
> 如果在Setting Tool出现意外错误，请关闭窗口并重试。 <br/> 
> 如果错误没能通过重新运行解决，在**Assets/NhnCloud/GamebaseTools/SettingTool/Editor/Scripts**中打开SettingToolWindow.cs文件，在ShowWindow方法中解除SettingTool.SetDebugMode(true);代码注释后，请传送日志。<br/><br/>
> 使用Unity Facebook Authentication时需要另行下载Facebook Unity SDK。 [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> 关于在Unity Facebook Authentication中支持的Facebook Unity SDK版本，请参考随附的README文件。 <br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Setting Tool Update

如果需要更新Setting Tool，Setting Tool将通知更新。
根据更新类型，Setting Tool提供的部分功能会受限制。

#### 强制更新

* 需要更新
* SDK下载限制
	* 可以使用已下载的SDK进行安装和删除。

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 选择更新

* 选择更新
* 可以下载SDK

![Select Build System](https://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

为了管理生命周期，需要把“com.toast.gamebase.activity.GamebaseMainActivity”设置为MainActivity。
"com.toast.gamebase.activity.GamebaseMainActivity"是继承"com.unity3d.player.UnityPlayerActivity"实现的。


> <font color="red">[注意]</font>
>
> AndroidPlugin开发也必须继承GamebaseMainActivity。 
> GamebaseMainActivity包含在GamebasePlugin.jar。
> launchMode应该是singleTask。(Unity默认Activity也将固定为singleTask。) 否则，第一次启动应用程序时可能会崩溃。
>
> 修改相关Lifecycle时，要通过启用Project Settings > Settings for Android > Publish Settings > Build > Custom Main Manifest来在相关AndroidManifest.xml上进行修改。 

> <font color="red">[注意]</font>
> 
> 将Android的targetSdkVersion设置为31以上时，在存在intent-filter的标签中必须声明android:exported属性。
> 当为了在Gamebase中管理Lifecycle而提供的**GamebaseMainActivity**设置为MainActivity时也需在属性中添加**android:exported="true"**。

```xml
<manifest>
	...
    <application>
    ...
    	<activity android:name="com.toast.android.gamebase.activity.GamebaseMainActivity"
        	android:launchMode="singleTask"
        	android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name"
            android:exported="true">
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

### iOS Settings

> <font color="red">[注意]</font>
>
> * Unity 2019.3以上注意事项
>     * 将PROJECT > Unity-iPhone > Enable Bitcode设置为No。
>     * 在TARGETS > UnityFramework添加iOS SDK设置。
>

1. 在Unity项目中进行iOS打包。
2. 对创建的XCode项目添加设置。       
    * [iOS SDK设置指南](./ios-started)

## API Reference

API Reference包含在GamebaseUnitySDK中。

## API Deprecate Governance

Gamebase不再支持的API，进行Deprecate处理。
在满足以下条件时，可以删除Deprecated的API，不再另行通知。

* 超过5次小更新
	* Gamebase Version Format - XX.YY.ZZ
		* XX : Major
		* YY : Minor
		* ZZ : Hotfix

* 至少5个月

