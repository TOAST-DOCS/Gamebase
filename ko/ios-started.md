## Game > Gamebase > Developer's Guide (iOS) > Getting Started

## Getting Started

### Environments


> [INFO]
>
> 최소사양 : iOS8 이상 <br/>
> - arm7, arm7s, arm64, i386, x86_64 지원 <br/>
> - Xcode7 이상
>


### Setting Xcode Project to use Gamebase

Gamebase는 아래와 같은 방법으로 설정이 가능합니다.

#### 1. Configuration

##### 1. Download

Gamebase는 [LINK \[http://docs.cloud.toast.com/ko/Download/\]](http://docs.cloud.toast.com/ko/Download/)에서 다운로드 받습니다.<br/>
Gamebase.framework.zip 및 필요한 adapter 들을 다운로드 받습니다.<br/>
또한 각 IDP의 인증을 하기위한 SDK파일들을 다운로드 받아야합니다. 해당 IDP의 로그인을 사용할 때만 포함하면 됩니다.<br/>
다운로드 받은 뒤, 해당 SDK파일을 프로젝트의 target에 포함시켜야 합니다.

[3rd Party SDK Download]

| Gamebase SDK | Gamebase Auth Adapter | External(iOS) SDK & Compatible Version | External SDK Download Link |
| --- | --- | --- | --- |
| Gamebase | Gamebase.framework, Gamebase.bundle |  |  |
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v4.17.0 | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.1.6 | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework |  |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework | Gamebase내에 포함 |
| Gamebase Push | GamebasePushAdapter.framework |  | Gamebase내에 포함 |



> [WARNING]
>
> Gamebase Framework 파일 중 이름에 **Adapter**가 포함되어 있는 파일들은 선택적으로 프로젝트 내에서 사용여부를 결정할 수 있으며, 해당 Adapter Framework를 사용하기 위해서는 위의 표에 명시된 외부 SDK들이 필요할 수 있습니다.
>

<br/>


> [INFO]
> 
>각 IDP에서 제공하는 외부 SDK에 대한 설정은 각 IDP의 가이드 문서를 참고하시길 바랍니다.
>

##### 2. Decompression 

압축을 풀면, 다음과 같이 Gamebase.framework 등의 SDK를 볼 수 있습니다.

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)

##### 3. Project Configuration

1. Framework 파일을 Project의 Project Navigator로 끌어와서 import 합니다. 이 때 추가된 Framework 파일들은 프로젝트 target에 추가되어야 합니다. 
2. **Gamebase.bundle** 파일도 **Copy Bundle Resources** 에 추가하도록 합니다.
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
3. Gamebase를 사용하기 위해서는 Gamebase의 framework외에, Gamebase에서 사용하고 있는 외부 SDK들의 기능을 포함하기 위하여, 여러 framework와 library 파일을 linker에서 참조할 수 있도록 추가해야합니다. 아래 항목들을 추가해야합니다.
    * libicucore.tbd (Gamebase SDK v1.1.5 이상에서 추가)
    * libz.tbd
    * libsqlite3.tbd
    * libstdc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework

![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)

4. **Target > Build Settings > Linking > Other Linker Flags**에 **-ObjC**를 추가해야 합니다.
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)

5. **Target > Build Settings > Enable Bitcode**를 **No**로 설정합니다.
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)


> [INFO]
>
> Linker에 **-ObjC**옵션 설정은 Static Library에 있는 모든 Objective-C class와 category를 로드하도록 합니다. <br/>
> 따라서 이 옵션을 설정하지 않았을 때에 **selector not recognized**와 같은 오류가 발생할 수 있습니다.
>




## API Reference
SDK 내에 포함되어 있습니다.
