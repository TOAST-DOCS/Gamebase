# Upcoming Products > Gamebase > Development for Ongame

## Summary

> **주의**
>
> 본 문서는 일반적인 경우 사용하지 않는 기능을 설명하고 있습니다.
> 프로젝트에 따라 특별한 요구사항이 있는 경우에만 Gamebase팀의 가이드에 따라 해당 기능을 사용하여야 합니다.


## Initialization
### Android
<내용없음>
***

### iOS
#### Ongame API 사용을 위한 ATS 설정

> 주의!
>
> iOS의 각 프로젝트에서 사용하는 'info.plist' 파일 내에 아래의 가이드 내용을 추가해야,정상적으로 Ongame REST API를 호출할 수 있습니다.
> iOS 10 이상에서는 TLS버전이 v1.3이상이어야만 별도의 White list 등록없이 TLS v1.3 미만의 서버 API를 호출할 수 있습니다.

```xml
		<key>NSExceptionDomains</key>
		<dict>
			<key>id.ongame.vn</key>
			<dict>
				<key>NSExceptionMinimumTLSVersion</key>
				<string>TLSv1.0</string>
			</dict>
		</dict>
```
***

### Unity
<내용없음>
***

## Login
### Android
#### 1. Login with WebView
OnGame Login WebView를 띄우고 인증 성공시 온게임 토큰을 가져와서 Gamebase에 로그인을 합니다.
providerName은 **@"ongame"**으로 설정합니다.
```java
Gamebase.login(activity, "ongame", new GamebaseDataCallback<AuthToken>() {
    @Override
    public void onCallback(AuthToken data, GamebaseException exception) {
        if (Gamebase.isSuccess(exception)) {
            Log.d(TAG, "Login successful");
            // 로그인 성공
        } else {
            Log.e(TAG, "Login failed- "
                    + "errorCode: " + exception.getCode()
                    + "errorMessage: " + exception.getMessage());
            // 로그인 실패
        }
    }
});
```

#### 2. Login with Access Token of Facebook
페이스북의 Access Token을 이용하여 Gamebase에 로그인 합니다.

```java
LoginManager.getInstance().registerCallback(mCallbackManager, new FacebookCallback<LoginResult>() {
    @Override
    public void onSuccess(LoginResult loginResult) {
        Log.d(TAG, "Facebook login successful.");

        final String facebookAccessToken = loginResult.getAccessToken().getToken();
        Map<String, Object> additionalInfo = new HashMap<>();
        additionalInfo.put("facebook_access_token", facebookAccessToken);

        Gamebase.login(activity, "ongame", additionalInfo, new GamebaseDataCallback<AuthToken>() {
            @Override
            public void onCallback(AuthToken data, GamebaseException exception) {
                if (Gamebase.isSuccess(exception)) {
                    Log.d(TAG, "Gamebase Login successful");
                    // 로그인 성공
                } else {
                    Log.e(TAG, "Gamebase Login failed- "
                            + "errorCode: " + exception.getCode()
                            + "errorMessage: " + exception.getMessage());
                    // 로그인 실패
                }
            }
        });
    }

    @Override
    public void onCancel() {
        Log.d(TAG, "Facebook login canceled.");
    }

    @Override
    public void onError(FacebookException error) {
        Log.d(TAG, "Facebook login error.);
    }
    });
```

***

### iOS
Gamebase SDK를 이용한 ongame 로그인 방법은 다음과 같이 2가지 방법으로 시도할 수 있습니다.
1. Ongame 계정을 이용한 로그인
2. Facebook 계정을 이용한 로그인

각각의 방법은 모두 UserID라는 Ongame의 고유 식별자를 획득할 수 있습니다.
또한 Gamebase의 로그인과 같이 연계되어 있기 때문에 Gamebase의 UserID도 획득할 수 있습니다.

#### 1. Ongame 계정을 이용한 로그인
Ongame 계정을 이용한 로그인은 WebView를 사용하여 지원되며, WebView에 로딩된 WebPage 내에서 ID/PW를 입력하여 로그인을 하는 방법입니다.
Type은 **@"ongame"**으로 설정합니다.

```objectivec
[TCGBGamebase loginWithType:@"ongame" viewController:nil completion:^(TCGBAuthToken *authToken, TCGBError *error) {
    [self printLogAndShowAlertWithData:[authToken description] error:error alertTitle:@"login ongame"];
}];
```

#### 2. Facebook 계정을 이용한 로그인
Facebook계정을 이용하여 로그인을 하기 위해서는 FacebookSDK를 통해 인증을 완료한 뒤, FacebookSDK에서 발급한 **AccessToken**을 사용하면 Ongame과 Gamebase를 연계한 계정 인증을 받을 수 있습니다.
Type은 **@"ongame"**으로 설정하며, 추가적으로 **additionalInfo** 파라미터에 facebookSDK를 통해 발급받은 **accessToken**을 설정해야합니다. 설정방법은 다음 예제코드를 확인합니다.

AdditionalInfo파라미터에는 **kTCGBAuthAdditionalInfoFacebookAccessToken** 키를 사용하거나, **@"facebook_access_token"** 문자열을 사용하면 됩니다.

```objectivec
#import "TCGBConstants.h"

[TCGBGamebase loginWithType:@"ongame" additionalInfo:@{ kTCGBAuthAdditionalInfoFacebookAccessToken: @"여기에 Facebook Access Token을 입력하시오." } viewController:parentViewController  completion:^(TCGBAuthToken *authToken, TCGBError *error) {
        [self printLogAndShowAlertWithData:[authToken description] error:error alertTitle:@"login ongame with facebook accessToken"];
    }];
```
> **information**
> 다음 페이지에서 권한이 있는 App에 대한 테스트용 AccessToken을 확인할 수 있습니다.
> https://developers.facebook.com/tools/accesstoken/
> (위의 예제의 facebook token 부분에 입력하면 로그인 테스트가 가능함)

***
### Unity

UnityEditor에서도 iOS와 동일한 방식으로 ongame 로그인을 지원합니다.

#### 1. Ongame 계정을 이용한 로그인

Gamebase의 Login with credential API를 사용한 방식으로 자세한 Flow는 아래와 같습니다.

1. 아래 경로에서 ongame 로그인 후 token_id 획득
	* [Register](https://id.ongame.vn/server/account/register?client_id=nhnent&redirect_uri=http://localhost)
	* [Login](https://id.ongame.vn/server/account/login?client_id=nhnent&redirect_uri=http://localhost)
2. 인증 후 발급되는 token_id를 복사
3. Gamebase.Login API 호출

> !주의
> Ongame Login API를 사용하려면 Register API를 통해 Ongame ID를 먼저 등록해야 합니다.

**Example**
```cs
#region OngameLogin with token_id
public void OngameLoginWithTokenID(string tokenID)
{
    var credential = new Dictionary<string, object>();
    credential.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, "ongame");
    credential.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, tokenID);

    Gamebase.Login(credential, (authToken, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            CurrentTC.IsSuccess = true;
            var sb = new StringBuilder();
            sb.AppendLine("success");
            sb.Append("UserID : " + authToken.member.userId);
            CurrentTC.PrintResult(sb);
        }
        else
        {
            CurrentTC.IsSuccess = false;
            CurrentTC.PrintResult(error);
        }
    });
}
#endregion
```

#### 2. Facebook 계정을 이용한 로그인

Gamebase의 Login with credential API를 사용한 방식으로 자세한 Flow는 아래와 같습니다.

1. 아래 경로에서 Facebook access token 획득
	* [developers.facebook.com](https://developers.facebook.com/tools/accesstoken/)
2. Net2E API를 사용하여 Facebook access token을 token_id로 변경
3. 획득한 token_id를 사용하여 Gamebase.Login API 호출

**Example**
```cs
#region OngameLogin with FacebookAccessToken
public class OngameVO
{
    public string token_id;
}

private IEnumerator OngameLoginWithFacebookAccessToken(string facebookAccessToken)
{
#if UNITY_EDITOR
    if (string.IsNullOrEmpty(facebookAccessToken))
    {
        GamebaseLog.Error("Facebook AccessToken is null.");
        yield break;
    }

    var form = new WWWForm();
    form.AddField(GamebaseAuthProviderCredential.ACCESS_TOKEN, facebookAccessToken);
    var headers = form.headers;
    if (headers.ContainsKey("Content-Type"))
        headers["Content-Type"] = "application/x-www-form-urlencoded";
    else
        headers.Add("Content-Type", "application/x-www-form-urlencoded");

    var www = new WWW("https://id.ongame.vn/server/api/v1/user/VerifyFacebookAccessToken", form);
    yield return www;

    if (string.IsNullOrEmpty(www.text))
    {
        Debug.Log(string.Format("error:{0}", www.error));
        yield break;
    }

    var vo = JsonUtility.FromJson<OngameVO>(www.text);
    if (string.IsNullOrEmpty(vo.token_id))
        yield break;

    OngameLoginWithTokenID(vo.token_id);

#elif !UNITY_EDITOR && (UNITY_ANDROID || UNITY_IOS)
    if (string.IsNullOrEmpty(facebookAccessToken))
    {
        GamebaseLog.Error("Facebook AccessToken is null.");
        yield break;
    }

    var additional = new Dictionary<string, object>();
    additional.Add("facebook_access_token", facebookAccessToken);
    Gamebase.Login("ongame", additional, (authToken, error) =>
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
#endif
}

#region OngameLogin with token_id
public void OngameLoginWithTokenID(string tokenID)
{
    var credential = new Dictionary<string, object>();
    credential.Add(GamebaseAuthProviderCredential.PROVIDER_NAME, "ongame");
    credential.Add(GamebaseAuthProviderCredential.ACCESS_TOKEN, tokenID);

    Gamebase.Login(credential, (authToken, error) =>
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
#endregion
#endregion
```

#### Get authentication information for external IDP

외부 IDP SDK 에서 아래와 같은 정보를 가져올 수 있습니다.

* AccessToken
* UserID
* Profile

Ongame에 대한  Profile key 값은 다음과 같습니다.

| Key | Value | Description |
| --- | --- | --- |
| "UserID" | "shhong" | |
| "NickName" | null | |
| "UserName" | "shhong" | |
| "SEX" | 1 | |
| "OnCash" | 0 | |
| "PhotoPath" | "http://id-test.vn/Content/images/avatar/default.png" | |
| "PhotoAdminAuth" | "" | |
| "VIPType" | "" | |
| "HackUser" | "" | |
| "OpenID" | null | |
| "AuthType" | "ongame" or "ongame-facebook" | "ongame" : Ongame web login<br>"ongame-facebook" : Facebook token login |

**API**<br>
![IOS](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-ios-plugin_1.0.0.png)
![ANDROID](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-icon-android-plugin_1.0.0.png)

```cs
static string GetAuthProviderUserID(string providerName)
static string GetAuthProviderAccessToken(string providerName)
static GamebaseRequest.AuthProviderProfile GetAuthProviderProfile(string providerName)
```

**Example**
```cs
public void GetAuthProviderUserID(string providerName)
{
	string userID = Gamebase.GetAuthProviderUserID(providerName);
    Debug.Log(string.Format("{0} UserID is {1}", providerName, userID));
}

public void GetAuthProviderAccessToken(string providerName)
{
	string accessToken = Gamebase.GetAuthProviderAccessToken(providerName);
    Debug.Log(string.Format("{0} AccessToken is {1}", providerName, accessToken));
}

public void GetAuthProviderProfile(string providerName)
{
	GamebaseRequest.AuthProviderProfile profile = Gamebase.GetAuthProviderProfile(providerName);

    if (profile == null || profile.information == null)
    {
        Debug.Log("Failed to get authentication information for external IDP..");
        return;
    }

    if (profile.information.ContainsKey("name"))
    {
        string name = profile.information["name"];
        Debug.Log(string.Format("{0} name is {1}", providerName, name));
    }

    if (profile.information.ContainsKey("email"))
    {
        Debug.Log(string.Format("{0} email is {1}", providerName, name));
    }
}
```

## Purchase
### Android

#### 1. Store Settings

Gamebase 초기화 시에 Store Code를 "**ONGATE**"로 설정해야 합니다.
하지만 다음 절에서 설명 드릴 **3. Change store runtime** API를 통해 동적으로 Store를 변경할 수도 있습니다.

```java
String STORE_CODE = PurchaseProvider.StoreCode.ONGATE;

TAPConfiguration configuration = new TAPConfiguration.Builder()
        .setAppId(APP_ID)
        .setAppVersion(APP_VERSION)
        .setStoreCode("ONGATE")	// Store code를 반드시 선언합니다.
        .build();

Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
    @Override
    public void onCallback(final LaunchingInfo data, GamebaseException exception) {
        ...
    }
});
```

#### 2. Purchase API

다음 링크를 통해서, Android 결제 API를 확인하시길 바랍니다.
* [Android 결제 API](http://docs.cloud.toast.com/ko/Upcoming%20Products/Gamebase/Android%20Developer%60s%20Guide/#purchase)

#### 3. Change store runtime

런타임 중, 다음의 API를 사용하여 동적으로 Store를 변경할 수 있습니다.
```java
// Google로 Store 변경
Gamebase.Purchase.setStoreCode(PurchaseProvider.StoreCode.GOOGLE);

// Ongate로 Store 변경
Gamebase.Purchase.setStoreCode(PurchaseProvider.StoreCode.ONGATE);
```

***

### iOS

현재 Gamebase에서는 결제 스토어으로 **AppStore(AS)**와 **OnGate(ONGATE)**를 지원하고 있습니다.

> **[ 지원 스토어 정보 ]**
> AppStore : https://itunes.apple.com/pw/genre/ios
> OnGate : https://ongate.vn

#### 1. 스토어 설정
어플리케이션 초기화시에, 사용할 스토어를 설정해줍니다. 설정을 하지 않았을 경우, 디폴트인 **"AS(AppStore)"** 로 설정이 됩니다.
**ONGATE** 스토어 설정을 위해서는 다음 예시처럼 앱 초기화 과정에서 반드시 **[configuration setStoreCode:@"ONGATE"];**로 설정해주어야합니다.

앱 구동 이후, Gamebase Initialization 이후에 Store를 변경하는 API는 **3. Runtime Store 변경**에서 설명하겠습니다.


```objectivec
NSString *projectID = @"T0aStC1d";
NSString *gameAppVersion = @"1.2";

TCGBConfiguration *configuration = [TCGBConfiguration configurationWithAppID:projectID appVersion:gameAppVersion];
[configuration setStoreCode:@"ONGATE"];

[TCGamebase initializeWithConfiguration:configuration launchOptions:launchOptions completion:^(id launchingData, TCGBError *error) {
    if ([TCGamebase isSuccessWithError:error] == YES) {
        // Gamebase Initialization is Succeeded
    }
}];
```

#### 2. 아이템 결제 및 구매 가능 아이템 리스트 조회
다음 링크를 통해서, iOS 결제 API를 확인하시길 바랍니다.
* [iOS 결제 API](http://docs.cloud.toast.com/ko/Upcoming%20Products/Gamebase/iOS%20Developer%60s%20Guide/#purchase)



#### 3. Runtime Store 변경
런타임 중, 다음의 API를 사용하여 Store를 변경할 수 있습니다.
```
//Store 변경
[TCGBGamebase setStoreCode:@"ONGATE"];
[TCGBPurchase requestItemListPurchasableWithCompletion:^(NSArray *purchasableItemArray, TCGBError *error) {
    // ...
}];


//Store 변경
[TCGBGamebase setStoreCode:@"AS"];
[TCGBPurchase requestItemListPurchasableWithCompletion:^(NSArray *purchasableItemArray, TCGBError *error) {
    // ...
}];
```

***


### Unity

#### 1. Store settings

Gamebase SDK를 초기화 할때 스토어 설정을 해야합니다.
스토어 설정을 하지 않을 경우 iOS는 AppStore, Android는 Google로 자동 설정됩니다.

Ongame의 경우 Gamebase Unity SDK 초기화 시 StoreCode를 `ONGATE`로 설정해야 하며, 초기화 관련 가이드는 아래를 참고하십시오.

* [Unity Developer's Guide in TOAST Cloud](http://docs.cloud.toast.com/ko/Upcoming%20Products/Gamebase/Unity%20Developer%60s%20Guide/)

##### 1. Initialize using the Unity Inspector.
Unity inspector를 사용하여 초기화 할 경우에는 아래 그림과 같이 StoreCode를 "ONGATE"로 설정하십시오.
![unity inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-Initialization-ongame_1.1.0.png)

##### 2. Initialize without using the Unity Inspector.
Unity inspector를 사용하지 않고 초기화 할 경우에는 아래 Example을 참고하십시오.

**Example**

```cs
public void Initialize()
{
    var configuration = new GamebaseRequest.GamebaseConfiguration();

    configuration.appID = "APP_ID";
    configuration.appVersion = "APP_VERSION";
    configuration.zoneType = "REAL";
    configuration.enableLaunchingStatusPopup = true;
    configuration.storeCode = "ONGATE";
    configuration.fcmSenderId = "FCM_SENDER_ID"; // Only AOS

    Gamebase.Initialize(configuration, (launchingInfo, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
        	Debug.Log("Gamebase initialization succeeded.");
        }
        else
        {
        	Debug.Log(string.Format("Gamebase initialization failed. error is {0}", error));
        }
    });
}
```
##### 3. Change store code on run time
초기화 이후 StoreCode를 변경 할 경우에는 아래 Example을 참고하십시오.

**Example**

```cs
public void SetStoreCode(string storeCode)
{
    Gamebase.Purchase.SetStoreCode(storeCode);
}

public void GetStoreCode()
{
    string storeCode = Gamebase.Purchase.GetStoreCode();
    if (string.IsNullOrEmpty(storeCode))
    {
        Debug.Log("fail");
    }
    else
    {
        Debug.Log(string.Format("success storeCode : {0}", storeCode));
    }
}
```

##### Store supported on mobile platforms.

현재 Gamebase에서 지원하는 스토어는 아래와 같습니다.

**Android store code**

| Store | Code | Infomation |
| --- | --- | --- |
| Google | GG | https://play.google.com/store |
| Ongame | ONGATE | https://ongate.vn |

**iOS store code**

| Store | Code | Infomation |
| --- | --- | --- |
| AppStore | AS | https://itunes.apple.com/pw/genre/ios |
| Ongame | ONGATE | https://ongate.vn |


#### 2. Check Sandbox Mode
Project가 ToastCloud에서 Sandbox로 설정이 되어있는지 확인할 수 있습니다.
초기화 이후에 호출 해야하며, 초기화 이전에 호출시, 크래시 및 예외가 발생합니다.

프로젝트가 Sandbox 모드일 때에는 Ongame 및 Payco 인증이 alpha/demo로 인증이 됩니다.
또한, ONGATE Store의 결제관련 기능도 **테스터 계정**에 한하여 Alpha ONGATE 로 연동됩니다.<br/>
(**테스터 계정**은 빌링개발팀 **양은주 책임**님께 등록 요청을 하셔야합니다.)

**Example**

```cs
public void IsSandbox()
{
    bool isSandbox = Gamebase.IsSandbox();
    if (isSandbox == true)
    {
        Debug.Log("Sandbox environment...");
    }
    else
    {
        Debug.Log("Not Sandbox environment...");
    }
}
```
