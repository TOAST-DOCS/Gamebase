## Upcoming Products > Gamebase > Unity Developer's Guide



## Configuration

### Getting Started
Gamebase Unity SDK 사용에 대해 설명합니다.

#### Environments

> [ 최소사양 ]
> Unity5

#### Supported Unity Platforms

Gamebase Unity SDK는 현재 아래 플랫폼에 대해서만 지원하고 있습니다.

* UNITY_ANDROID
* UNITY_IOS
* UNITY_EDITOR (일부 기능)

추후 아래 플랫폼을 지원할 예정입니다.

* UNITY_STANDALONE
* UNITY_WEBGL

Gamebase Unity SDK 에서 지원하는 플랫폼 선택 후, 선택한 플랫폼에서 지원하지 않는 API 호출 시에는 아래와 같은 에러가 콜백으로 리턴되며 콜백이 없는 API의 경우에는 Data type의 초기값이 전달됩니다.

* GamebaseErrorCode.NOT_SUPPORTED
* GamebaseErrorCode.NOT_SUPPORTED_ANDROID
* GamebaseErrorCode.NOT_SUPPORTED_IOS
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_EDITOR
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_STANDALONE
* GamebaseErrorCode.NOT_SUPPORTED_UNITY_WEBGL

API 별 지원하는 플랫폼은 아래와 같은 icon 으로 구분합니다.<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-standalone-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-webgl_1.0.0.png)

#### Installation

Gamebase Unity SDK는 유니티 패키지 형태(.unitypackage)로 배포되며 아래 링크에서 다운로드 가능합니다.

* [Download Gamebase Unity SDK](http://docs.cloud.toast.com/ko/Download/)


Gamebase Unity SDK를 게임 프로젝트에 추가하는 방법은 다음과 같습니다.

1. Unity에서 이미 생성된 게임 프로젝트나 새로 생성한 프로젝트 열기.
2. Unity Menu에서 Assets > Import Package > Custome Package를 선택하여 배포된 GamebaseUnitySDK.unitypackage Import.

##### Add Android SDK

Unity Android 빌드 시 필요한 설정입니다.

다운로드 받은 Android SDK에서 아래 폴더 및 aar 파일을 프로젝트의 Assets/Plugins/Android 폴더에 추가 합니다

* gamebase-sdk/
* gamebase-sdk-VERSION-release.aar
* gamebase-sdk-base-VERSION-release.aar

인증 모듈 추가

1. 다운로드 받은 SDK의 gamebase-adapter-auth-IDP_NAME 폴더를 프로젝트의 Assets/Plugins/Android폴더에 추가합니다.
2. google, facebook, payco 중에서 사용할 인증 모듈을 모두 프로젝트의 Assets/Plugins/Android폴더에 추가합니다.

##### AndroidManifest.xml
Lifecycle 관리를 위해 "com.toast.gamebase.activity.GamebaseMainActivity"를 MainActivity로 해야 합니다.
> **!주의**
> AndroidPlugin 개발에도 GamebaseMainActivity를 상속받아 만들어야 합니다.
> GamebaseMainActivity는 GamebaseAndroidPlugin.jar에 포함되어 있습니다.

```xml
<activity android:name="com.toast.gamebase.activity.GamebaseMainActivity"
          android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
          android:label="@string/app_name">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

Android SDK 추가 설정은 아래 링크를 참조 하시기 바랍니다

* [Android SDK 추가 설정 링크](./Android Developer`s Guide#initialization)

![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-AndroidSetting_1.0.0.png)<br>
**그림. Android SDK 추가하기**

##### Add IOS SDK

Unity IOS 빌드 시 필요한 설정입니다.

다운로드 받은 IOS SDK에서 다음 폴더 및 framework 파일을 프로젝트의 Assets/Plugins/IOS 폴더에 추가 합니다

* externals/
* Gamebase.bundle
* Gamebase.framework

인증 모듈 추가(아래 폴더 및 framework 파일 중에서 사용할 기능을 모두 프로젝트의 Assets/Plugins/IOS 폴더에 추가 합니다)

* FacebookSDK/
* GamebaseAuthFacebookAdapter.framework
* GamebaseAuthGamecenterAdapter.framework
* GamebaseAuthPaycoAdapter.framework
* PIDThirdPartyAuth.framework

Purchase 모듈과 추가(아래 framework을 프로젝트의 Assets/Plugins/IOS 폴더에 추가 합니다)

* GamebasePurchaseIAPAdapter.framework

Push 모듈과 추가(아래 framework 을 프로젝트의 Assets/Plugins/IOS 폴더에 추가 합니다)

* GamebasePushAdapter.framework

IOS SDK 추가 설정은 아래 링크를 참조 하시기 바랍니다

* [IOS SDK 추가 설정 링크](./iOS Developer`s Guide#setting-xcode-project-to-use-gamebase)

![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-iOSSetting_1.0.0.png)<br>
**그림. IOS SDK 추가하기**

### Initialization

Gamebase Unity SDK 를 사용하기 전에 초기화를 수행해야 하며, App ID, App Version 정보는 사전에 TOAST Cloud Console에 반드시 등록되어 있어야 합니다.

초기화 시 필요한 값들은 아래와 같습니다.

* appID : TOAST Cloud에 등록한 프로젝트 ID
* appVersion : TOAST Cloud에 등록한 클라이언트 버전
* zoneType : "REAL" 또는 " "

> **!주의**
> zoneType은 "REAL" 또는 " " 으로 설정하십시오. (" " 설정 시 자동으로 "REAL"로 설정됩니다.)
> 그외 다른 값을 입력할 경우 정상 작동하지 않습니다.

#### Initialize UnitySDK using Inspector settings

1. 빈 게임 오브젝트를 생성합니다. (아래 그림의 GamebaseUnitySDK)
2. 생성한 게임 오브젝트에 GamebaseUnitySDKSettings.cs 파일을 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.
3. 추가 된 Component에서 appID, appVersion, zoneType을 입력합니다.
4. Gamebase.Initialize(callback) API를 호출합니다.

> **!주의**
> 생성한 게임 오브젝트를 삭제하면 Android, iOS API 호출 후 콜백을 받을 수 없으므로 주의하시기 바랍니다.
> 실수로 삭제된 경우 "Do not destroy this gameObject in order to receive callback." 에러 메시지가 노출됩니다.

![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-Initialization_1.0.0.png)<br>
**그림. Inspector를 이용한 초기화**

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)

```cs
static void Initialize(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

``` cs
public void Initialize()
{
    Gamebase.Initialize((launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Gamebase initialization is succeeded.");
        }
        else
        {
        	Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
        }
    }
}
```

#### Initialize UnitySDK with parameters

인자를 이용한 초기화를 하기 위해서는 GamebaseRequest.GamebaseConfiguration 인스턴스에 값을 설정하여 Initialize API를 호출해야 합니다.
Android, iOS 플랫폼을 사용하는 경우에는 Native에서 Callback을 받기 위해 PluginCallbackManager를 GameObject에 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.

1. 빈 게임 오브젝트를 생성합니다.
2. 생성한 게임 오브젝트에 PluginCallbackManager.cs 파일을 Add Component 하여 게임 오브젝트의 컴포넌트로 추가합니다.
3. GamebaseRequest.GamebaseConfiguration 인스턴스에 appID, appVersion, zoneType을 설정합니다.
4. Gamebase.Initialize(configuration, callback) API 호출를 호출합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)

```cs
static void Initialize(GamebaseRequest.GamebaseConfiguration configuration, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Launching.LaunchingInfo> callback)
```

**Example**

```cs
public void Initialize()
{
    var configuration = new GamebaseRequest.GamebaseConfiguration();
    configuration.appID = GamebaseUnitySDK.AppID;
    configuration.appVersion = GamebaseUnitySDK.AppVersion;
    configuration.zoneType = "REAL";

    Gamebase.Initialize(configuration, (launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Gamebase initialization is succeeded");
        }
        else
        {
        	Debug.Log(string.Format("Gamebase initialization is failed. error is {0}", error));
        }
    }
}
```

### Login

Gamebase 에서는 guest 로그인을 기본으로 지원합니다. guest 이외의 Provider에 로그인을 하기 위해서는 해당 Provider AuthAdapter가 필요합니다. AuthAdapter 대한 설정은 다음의 링크를 참고하시길 바랍니다.

* Android : [설정 링크](./Android Developer`s Guide#dependency)
* iOS : [설정 링크](./iOS Developer`s Guide#setting-xcode-project-to-use-gamebase)

#### 1. Login using a specific IDP

특정 IDP에 대한 로그인 버튼을 클릭하였을 때, 다음 로그인 API를 구현합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)

UnityEditor에서는 Guest로그인만 지원합니다.

```cs
static void Login(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) // Only IOS
```

> 몇몇 IDP의 로그인시에는 필수적으로 들어가야하는 정보가 있습니다. 예를 들어, facebook 로그인을 구현하기 위해서는 scope 등을 설정해주어야합니다. 이러한 필수 정보들을 설정해주기 위해서, static void Login(string providerName, Dictionary<string, object> additionalInfo, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback) API를 제공합니다. 파라미터 additionalInfo에 필수 정보들을 Dictionary 형태로 입력하시면 됩니다. (파라미터 값이 null일 때는, TOAST Cloud Console에 등록한 additionalInfo 값으로 채워집니다. 파라미터 값이 있을 때는 Console에 등록해놓은 값보다 우선시하여 값을 덮어쓰게 됩니다.)

**Example**

``` cs
public void Login(string providerName)
{
	Gamebase.Login(providerName, (authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	string userId = authToken.member.userId;
        	Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
        	Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

#### 2.  Login as the latest login IDP

가장 최근에 로그인한 IDP로의 로그인을 시도합니다.
해당 로그인에 대한 토큰이 만료되었거나, 토큰에 대한 검증 등이 실패하였을 때, 실패를 리턴합니다. 이 때는 [해당 IDP에 대한 로그인](#1-log-in-using-a-specific-idp)을 구현해야합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void LoginForLastLoggedInProvider(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**

``` cs
public void LoginForLastLoggedInProvider()
{
	Gamebase.LoginForLastLoggedInProvider((authToken, error) =>
    {
    	if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Login succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.SOCKET_ERROR || error.code == (int)GamebaseErrorCode.SOCKET_RESPONSE_TIMEOUT)
            {
            	Debug.Log(string.Format("Retry LoginForLastLoggedInProvider or notify an error message to the user. : {0}", error.message));
            }
            else
            {
                Debug.Log("Try to login using a specifec IDP");
                Login("LastLoggedInProvider");
            }
        }
    });
}

public void Login(string providerName)
{
    Gamebase.Login(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            string userId = authToken.member.userId;
            Debug.Log(string.Format("Login succeeded. Gamebase userId is {0}", userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error is {0}", error));
        }
    });
}
```

### Logout

로그아웃 버튼을 클릭했을 때, 다음과 같이 로그아웃 API를 구현합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)
![EDITOR](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-editor-plugin_1.0.0.png)

```cs
static void Logout(GamebaseCallback.ErrorDelegate callback)
```

**Example**
```cs
public void Logout()
{
    Gamebase.Logout((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Logout succeeded.");
        }
        else
        {
        	Debug.Log(string.Format("Logout failed. error is {0}", error));
        }
    });
}
```

### Withdraw

탈퇴 버튼을 클릭했을 때, 다음과 같이 탈퇴 API를 구현합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void Withdraw(GamebaseCallback.ErrorDelegate callback)
```

**Example**
```cs
public void Withdraw()
{
    Gamebase.Withdraw((error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Withdraw succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Withdraw failed. error is {0}", error));
        }

    });
}
```

### Mapping

Mapping은 기존에 로그인된 계정에 다른 IDP의 계정을 연동/해제시키는 기능입니다. 특정 IDP에 연동된(guest 포함) 계정에 다른 IDP의 계정을 연동하였을 때, 각각의 계정들에 대해서 UserID는 동일하게 주어집니다.

#### 1. Add Mapping

특정 IDP에 로그인 된 상태에서 다른 IDP로 Mapping을 시도합니다. Mapping을 하려는 IDP의 계정이 이미 다른 계정이 연동이 되어있다면, AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER(3302) 에러를 리턴합니다.

Mapping이 성공이 되었어도, 현재 로그인된 IDP는 Mapping된 IDP가 아니라, 기존에 로그인했던 IDP가 됩니다. 즉, Mapping은 단순히 IDP를 연동만 해줍니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void AddMapping(string providerName, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Auth.AuthToken> callback)
```

**Example**
```cs
public void AddMapping(string providerName)
{
    Gamebase.AddMapping(providerName, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("AddMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("AddMapping failed. error is {0}", error));
        }
    });
}
```

#### 2. Remove mapping

특정 IDP에 대한 연동을 해제합니다. 만약, 해제하고자 하는 IDP가 유일한 IDP라면, 실패를 리턴하게 됩니다. 연동 해제후에는 Gamebase 내부에서, 해당 IDP에 대한 로그아웃처리를 해줍니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RemoveMapping(string providerName, GamebaseCallback.ErrorDelegate callback)
```

**Example**
```cs
public void RemoveMapping(string providerName)
{
    Gamebase.RemoveMapping(providerName, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RemoveMapping succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RemoveMapping failed. error is {0}", error));
        }
    });
}
```

### Purchase

#### 1. Purchase item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매요청을 합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**

```cs
public void RequestPurchase(long itemSeq)
{
    Gamebase.Purchase.RequestPurchase(itemSeq, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}
```

#### 2. Get a list of items purchasable

아이템 목록을 조회하기 위하여 다음의 API를 호출합니다. 콜백으로 리턴되는 Array 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**Example**

```cs
public void RequestItemListPurchasable()
{
    Gamebase.Purchase.RequestItemListPurchasable((purchasableItemList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

#### 3. Get a list of items non-consumed

아이템을 구매는 하였지만, 정상적으로 아이템이 소비(배송, 지급)되었지 않은 미소비 결제내역을 요청합니다. 해당 내역을 받은 경우에는 게임서버(아이템 서버)에 요청을 하여, 아이템을 배송(지급)하도록 처리하여야합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**

```cs
public void RequestItemListOfNotConsumed()
{
    Gamebase.Purchase.RequestItemListOfNotConsumed((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
            
            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

#### 4. Reprocess purchase transaction

스토어 결제는 정상적으로 이루어졌지만, ToastCloud IAP 서버 검증 실패 등으로 인해 정상적으로 결제가 이뤄지지 않은 경우에, 해당 API를 이용하여 재처리를 시도합니다. 최종적으로 결제가 성공한 내역을 바탕으로, 아이템 배송(지급)등의 API를 호출하여 처리를 해주어야합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RequestRetryTransaction(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableRetryTransactionResult> callback)
```

**Example**

```cs
public void RequestRetryTransaction()
{
    Gamebase.Purchase.RequestRetryTransaction((purchasableRetryTransactionResult, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RequestRetryTransaction succeeded.");
            
            // Should Deal With This Retry Transaction Result.
            // You may send result to your gameserver and add item to user.
        }
        else
        {
            Debug.Log(string.Format("RequestRetryTransaction failed. error is {0}", error));
        }
    });
}
```

### Push

#### 1. Register push

다음의 API를 호출하여, ToastCloud Push에 해당 사용자를 등록합니다. Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을 사용자로부터 받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void RegisterPush(NativeRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback)
```

**Example**

```cs
public void RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    var pushConfiguration = new NativeRequest.Push.PushConfiguration();
    pushConfiguration.pushEnabled = pushEnabled;
    pushConfiguration.adAgreement = adAgreement;
    pushConfiguration.adAgreementNight = adAgreementNight;
    
    // You should receive the above values to the logged-in user.
    
    Gamebase.Push.RegisterPush(pushConfiguration, (error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

#### 2. Get push settings

사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다. 콜백으로 오는 PushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback)
```

**Example**

```cs
public void QueryPush()
{
    Gamebase.Push.QueryPush((pushAdvertisements, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("QueryPush succeeded.");
            
            bool pushEnabled = pushAdvertisements.pushEnabled;
            bool adAgreement = pushAdvertisements.adAgreement;
            bool adAgreementNight = pushAdvertisements.adAgreementNight;
            
            // You can handle these variables.
        }
        else
        {
            Debug.Log(string.Format("QueryPush failed. error is {0}", error));
        }
    });
}
```

### UI

## Error code

| Category | Sub Category | Error | Error Code | Notes |
| --- | --- | --- | --- | --- |
|Success| | SUCCESS | 0 | |
|Common| | NOT_INITIALIZED | 1 | |
|      | | NOT_LOGGED_IN | 2 | |
|      | | INVALID_PARAMETER | 3 | |
|      | | INVALID_JSON_FORMAT | 4 | |
|      | | USER_PERMISSION | 5 | |
|      | | NOT_SUPPORTED | 10 | |
|      | | NOT_SUPPORTED_ANDROID | 11 | |
|      | | NOT_SUPPORTED_IOS | 12 | |
|      | | NOT_SUPPORTED_UNITY_EDITOR | 13 | |
|      | | NOT_SUPPORTED_UNITY_STANDALONE | 14 | |
|      | | NOT_SUPPORTED_UNITY_WEBGL | 15 | |
| Network | Socket | SOCKET_RESPONSE_TIMEOUT | 101 | |
|         | | SOCKET_ERROR | 110 | |
|         | | UNKNOWN_ERROR | 999 | |
| Launching | | LAUNCHING_SERVER_ERROR | 2001 | |
| Auth | Common | AUTH_USER_CANCELED | 3001 | |
|      |        | AUTH_NOT_SUPPORTED_PROVIDER | 3002 | |
|      |        | AUTH_NOT_EXIST_MEMBER | 3003 | |
|      |        | AUTH_INVALID_MEMBER | 3004 | |
|      |        | AUTH_EXTERNAL_LIBRARY_ERROR | 3009 | |
|      | Gamebase Login | AUTH_TOKEN_LOGIN_FAILED | 3101 | |
|      |          | AUTH_TOKEN_LOGIN_INVALID_TOKEN_INFO | 3102 | |
|      |          | AUTH_TOKEN_LOGIN_INVALID_LAST_LOGGED_IN_IDP | 3103 | |
|      | IDP Login | AUTH_IDP_LOGIN_FAILED | 3201 | |
|      |           | AUTH_IDP_LOGIN_INVALID_IDP_INFO | 3201 | |
|      | Add Mapping | AUTH_ADD_MAPPING_FAILED | 3301 | |
|      |            | AUTH_ADD_MAPPING_ALREADY_MAPPED_TO_OTHER_MEMBER | 3302 | |
|      |            | AUTH_ADD_MAPPING_ALREADY_HAS_SAME_IDP | 3303 | |
|      |            | AUTH_ADD_MAPPING_INVALID_IDP_INFO | 3304 | |
|      | Remove Mapping | AUTH_REMOVE_MAPPING_FAILED | 3401 | |
|      |               | AUTH_REMOVE_MAPPING_LAST_MAPPED_IDP | 3402 | |
|      |               | AUTH_REMOVE_MAPPING_LOGGED_IN_IDP | 3403 | |
|      | Logout | AUTH_LOGOUT_FAILED | 3501 | |
|      | Withdrawal | AUTH_WITHDRAW_FAILED | 3601 | |
|      | Not Playable | AUTH_NOT_PLAYABLE | 3701 | |
|      | Unknown | AUTH_UNKNOWN_ERROR | 3999 | |
| Purchase | | PURCHASE_NOT_INITIALIZED | 4001 | |
|          | | PURCHASE_USER_CANCELED | 4002 | |
|          | | PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | |
|          | | PURCHASE_NOT_SUPPORTED_MARKET | 4010 | |
|          | | PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | |
|          | | PURCHASE_UNKNOWN_ERROR | 4999 | |
| Push | | PUSH_NOT_REGISTERED | 5001 | |
|      | | PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | |
|      | | PUSH_UNKNOWN_ERROR | 5999 | |
| UI | | UI_UNKNOWN_ERROR | 6999 | |
| Server | | SERVER_INTERNAL_ERROR | 8001 | |
|        | | SERVER_REMOTE_SYSTEM_ERROR | 8002 | |
|        | | SERVER_UNKNOWN_ERROR | 8999 | |
| Platform Reserved | | INVALID_INTERNAL_STATE | 11001 | |
|        | | NOT_CALLABLE_STATE | 11002 | |



## API Reference
SDK 내에 포함되어 있습니다.