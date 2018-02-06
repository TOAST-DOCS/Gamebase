## Game > Gamebase > iOS Developer's Guide > Getting Started
### Environments


> [INFO]
>
> Minimum Requirements: iOS8 or higher <br/>
> Supports arm7, arm7s, arm64, i386, x86_64<br/>
> Xcode7 or higher
>


### Installation

Gamebase can be setup as below.

#### Download

Download Gamebase from [{@수정}사용설명서 > Download](/en/Download/#game-gamebase).
Download Gamebase.framework.zip and required adapters.
Also download SDK files to authenticate each IdP, which are required only for a login.
Then, include corresponding SDK files to a target of your project.

**3rd Party SDK Download**

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | Usage | External SDK Download Link |
| --- | --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle |   | Includes Gamebase interface and key logics |   |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | Supports Facebook logins | [LINK [Go to Download]]( [https://developers.facebook.com/docs/ios/downloads](https://developers.facebook.com/docs/ios/downloads)) |
|   | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | Supports Payco logins | [LINK [Go to Download]]( [https://developers.payco.com/guide/sdk/download](https://developers.payco.com/guide/sdk/download)) |
|   | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Supports Gamecenter logins |   |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | Supports in-game purchase | Gamebase Included in IAP |
| Gamebase Push | GamebasePushAdapter.framework |   | Supports Push | Included in Gamebase |

> <font color="red">[Caution]</font><br/>
>
> Among Gamebase Framework files, **Adapter** files can be selectively used in a project to that end, external SDKs may be required as specified in the above table.
>

<br/>

> [INFO]
> 
> For setting of external SDKs which each IdP provides, refer to each IdP guide.
>

#### Xcode Settings

By decompression, following SDKs will show, including Gamebase.framework.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) Drag framework files to Project Navigator of the project and import. Note that added framework files should be added to a project target.
* 2) Also add the **Gamebase.bundle** file to **Copy Bundle Resources**.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) To include functions of external SDKs used in Gamebase, other than Gamebase framework, many frameworks and library files should be added to the linker for reference, as below.
    * libicucore.tbd (for Gamebase SDK v1.1.5 or higher)
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


> [INFO]
>
> The **-ObjC option** to the Linker allows all Objective-C classes and categories of Static Library to be loaded. <br/>
> Therefore, if this option is not set, an error like **selector not recognized** may occur during runtime.
>

## API Reference

Included in SDK.
