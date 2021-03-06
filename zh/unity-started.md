## Game > Gamebase > Unity SDK使用指南 > 开始

以下说明Gamebase Unity SDK使用环境和初始设置。

### Environments 

> [参考] 
>
> Gamebase支持的Unity版本
>
> * 2017.4.16 ~ 2020.1.8
> * 如果需要Gamebase支持的低版本Unity时，请联系[客户服务](https://toast.com/support/inquiry)。

#### Android
> <font color="red">[注意]</font>
>
> 2019年8月1日起Google Play公告的新应用程序应支持64位架构。
> [确认Google Play政策及支持64bit Unity版本](https://developer.android.com/distribute/best-practices/develop/64-bit#unity-developers)

#### Supported Platforms

* iOS
* Android
* Standalone
    * Windows7 以上
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
    * 支持最新版本的下载。
2. 安装SDK
    * 支持下载的SDK安装。
3. 删除SDK
    * 支持删除已安装的SDK。
4. 更新SDK
    * 不支持更新功能。
    * 如需更新请删除后重新安装。

### Using the Setting Tool

#### 安装SDK
1. 打开Unity项目。
2. 输入GamebaseUnitySettingTool_{version}.unitypackage。
3. 运行Menu > Tools > Gamebase > SDKSettings > Setting Tool。
	* v1.0.1 以下：Menu > Gamebase > SDKSettings > Setting Tool
4. 通过点击[Download SDK]按钮来下载SDK。
5. 选择所需的平台。
    * Android
    * iOS
6. 选择在每个平台上使用的模块儿。
    * Authentication支持与类似Google的ID Provider(所谓IDP)的联动。
    * Push支持FCM(Firebase)、APNS Push等服务。
    * Pruchase通过使用TOAST支付服务的IAP(In-App Purchase)来支持支付。
7. 通过点击[Settings]按钮来设置SDK。

#### 删除SDK
1. 运行Menu > Tools > Gamebase > SDKSettings > Setting Tool。
	* v1.0.1 以下：Menu > Gamebase > SDKSettings > Setting Tool
2. 点击[Remove] 按钮删除已安装的SDK。

<br/>
> [参考]
> 
> 如果Setting Tool发生意外错误，请关闭窗口后重新运行。 <br/>
> 如果您使用Unity Facebook认证，则需要单独下载Facebook Unity SDK。 [Go to Download](https://developers.facebook.com/docs/unity/)<br/>
> 有关Unity Facebook Authentication支持的Facebook Unity SDK版本，请参考随附的README文件。 <br/>

### Video of Setting Tool Usage

<iframe src="https://www.youtube.com/embed/kZ3Z1Kfr7Zw" frameborder="0" allowfullscreen="" wmode="Opaque" allow="encrypted-media" style="
    margin: auto;
    position: relative;
    width: 560px;
    height: 315px;
"></iframe>


### Update of Setting Tool

如果需要更新Setting Tool，Setting Tool将通知更新。
根据更新类型，Setting Tool提供的部分功能会受限制。

#### 强制更新

* 需要更新
* SDK下载限制
	* 可以使用已下载的SDK进行安装和删除

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-1_1.13.0.png)

#### 选择更新

* 选择更新
* 可下载SDK

![Select Build System](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-started-settingtool-update-2_1.13.0.png)

### Android Lifecycle

为了管理生命周期，需要把“com.toast.gamebase.activity.GamebaseMainActivity”设置为为MainActivity。
"com.toast.gamebase.activity.GamebaseMainActivity"是继承"com.unity3d.player.UnityPlayerActivity"实现的。


> <font color="red">[注意]</font>
>
> AndroidPlugin开发也必须继承GamebaseMainActivity。 <br/>
> GamebaseMainActivity包含在GamebasePlugin.jar。 <br/>
> launchMode应该是singleTask。(Unity默认Activity也将固定为singleTask。) 否则，第一次启动应用程序时可能会崩溃。

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

