## Game > Gamebase > iOS Developer's Guide > Getting Started

## Getting Started

### Environments


> [Note]
>
> Minimum Requirements: iOS8 or higher <br/>
> Supports arm7, arm7s, arm64, i386, x86_64 <br/>
> Xcode7 or higher
>


### Installation

Gamebase can be setup as below.

#### Download

Download Gamebase from [http://docs.cloud.toast.com/ko/Download/](http://docs.cloud.toast.com/ko/Download/).<br/>
Download Gamebase.framework.zip with required adapters. <br/>
Also download SDK files to authenticate each IdP: they are in need only when to log in each IdP.<br/>
Then, include corresponding SDK files to a target of project.

**3rd Party SDK Download**

| Gamebase SDK           | Gamebase Auth Adapter                   | External(iOS) SDK & Compatible Version | Use                               | External SDK Download Link               |
| ---------------------- | --------------------------------------- | -------------------------------------- | ------------------------------- | ---------------------------------------- |
| Gamebase               | Gamebase.framework, Gamebase.bundle     |                                        | Includes Gamebase interface and key logics |                                          |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework   | FacebookSDK v4.17.0                    | Supports Facebook logins                | [Go to Download](https://developers.facebook.com/docs/ios/downloads) |
|                        | GamebaseAuthPaycoAdapter.framework      | PaycoID Login 3rd SDK v1.1.6           | Supports Payco logins                   | [Go to Download](https://developers.payco.com/guide/sdk/download) |
|                        | GamebaseAuthGamecenterAdapter.framework | GameKit.framework                      | Supports Gamecenter logins              |                                          |
| Gamebase IAP           | GamebasePurchaseIAPAdapter.framework    | StoreKit.framework                     | Supports in-game purchase                     | Gamebase Included in IAP                       |
| Gamebase Push          | GamebasePushAdapter.framework           |                                        | Supports Push                        | Included in Gamebase                            |



> <font color="red">[Caution]</font><br/>
>
> Among Gamebase Framework files, those with **Adapter**included in the name can be selected for use in a project; to use a corresponding Adapter Framework, external SDKs in the above table may be required.
>

<br/>


> [Note]
>
> For setting of external SDKs which each IdP provides, refer to each IdP guide.
>

#### Xcode Settings

By decompression, following SDKs, including Gamebase.framework, will show.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) Drag framework files to Project Navigator of the project and import. Note that added framework files should be added to a project target.
* 2) Also add the **Gamebase.bundle** file to **Copy Bundle Resources**.
  ![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) To use Gamebase, many frameworks and library files should be added to the linker for reference, so as to include functions of external SDKs to Gamebase, like the following:
    * libicucore.tbd (to add for Gamebase SDK v1.1.5 or higher)
    * libz.tbd
    * libsqlite3.tbd
    * libstdc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
      ![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) Add **-ObjC** to **Target > Build Settings > Linking > Other Linker Flags**.
  ![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 5) Set **No** for **Target > Build Settings > Enable Bitcode**.
  ![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)


> [Note]
>
> The **-ObjC option**to the Linker allows all Objective-C classes and categories of Static Library to be loaded. <br/>
> Therefore, if this option is not set, an error like **selector not recognized** may occur during runtime.
>




## API Reference

Included in SDK.
