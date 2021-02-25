## Game > Gamebase > Unity SDK 사용 가이드 > 푸시

여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

### Settings


Android나 iOS에서 푸시를 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>

* [Android Push Settings](aos-push#settings)<br/>
* [iOS Push Settings](ios-push#settings)


### Register Push

다음 API를 호출하여, NHN Cloud Push에 해당 사용자를 등록합니다.
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.


**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback);
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseRequest.Push.NotificationOptions options, GamebaseCallback.ErrorDelegate callback);
```

**Example**

```cs
public void RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    GamebaseRequest.Push.PushConfiguration pushConfiguration = new GamebaseRequest.Push.PushConfiguration();
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

### Notification Options

* 단말기에 표시하는 알림을 어떤 형태로 표시할 것인지 Notification Options 를 통해 변경할 수 있습니다.
* 런타임에 registerPush API 를 호출하여 변경할 수 있습니다.

#### Set Notification Options with RegisterPush in Runtime

RegisterPush API 호출시 GamebaseRequest.Push.NotificationOptions 인자를 추가하여 알림 옵션을 설정할 수 있습니다.
GamebaseRequest.Push.NotificationOptions 의 생성자에 Gamebase.Push.GetNotificationOptions() 호출 결과를 전달하면, 현재의 알림 옵션으로 초기화 된 오브젝트가 생성되므로, 필요한 값만 변경할 수 있습니다.<br/>
설정 가능한 값은 아래와 같습니다.

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | 앱이 포그라운드 상태일때의 알림 노출 여부<br/>**default**: false |
| badgeEnabled           | bool         | 배지 아이콘 사용 여부<br/>**default**: true |
| soundEnabled           | bool         | 알림음 사용 여부<br/>**default**: true<br/>**iOS Only** |
| priority               | int          | 알림 우선 순위. 아래 5가지 값을 설정할 수 있습니다.<br/>GamebaseNotificationPriority.MIN : -2<br/> GamebaseNotificationPriority.LOW : -1<br/>GamebaseNotificationPriority.DEFAULT : 0<br/>GamebaseNotificationPriority.HIGH : 1<br/>GamebaseNotificationPriority.MAX : 2<br/>**default**: GamebaseNotificationPriority.HIGH<br/>**Android Only** |
| smallIconName          | string       | 알림용 작은 아이콘 파일 이름.<br/>설정하지 않을 경우 앱 아이콘이 사용됩니다.<br/>**default**: null<br/>**Android Only** |
| soundFileName          | string       | 알림음 파일 이름. 안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다.<br/>**default**: null<br/>**Android Only** |

**Example**

```cs
public void RegisterPushSample(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    var configuration = new GamebaseRequest.Push.PushConfiguration
    {
        pushEnabled = pushEnabled,
        adAgreement = adAgreement,
        adAgreementNight = adAgreementNight,
        displayLanguageCode = GamebaseDisplayLanguageCode.Korean
    };

    var notificationOptions = new GamebaseRequest.Push.NotificationOptions
    {
        foregroundEnabled = true,
        priority = GamebaseNotificationPriority.HIGH
    };

    Gamebase.Push.RegisterPush(configuration, (error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RegisterPush succeeded.");
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RegisterPush failed. error is {0}", error));
        }
    });
}
```

#### Get NotificationOptions

푸시를 등록할 때 기존에 설정했던 알림 옵션값을 가져옵니다.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static GamebaseResponse.Push.NotificationOptions GetNotificationOptions();
```

**Example**

```cs
public void GetNotificationOptionsSample()
{
    GamebaseResponse.Push.NotificationOptions notificationOptions = Gamebase.Push.GetNotificationOptions();

    GamebaseRequest.Push.NotificationOptions options = new GamebaseRequest.Push.NotificationOptions(notificationOptions);
    options.foregroundEnabled = true;
    options.smallIconName = "notification_icon_name";

    GamebaseRequest.Push.PushConfiguration configuration = new GamebaseRequest.Push.PushConfiguration();
    Gamebase.Push.RegisterPush(configuration, options, (error)=> { });
}
```

### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다. <br/>
콜백으로 오는 GamebaseResponse.Push.TokenInfo 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS<br/>
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void QueryTokenInfo(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.TokenInfo> callback);

// Legacy API
public static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback);
```

**Example**

```cs
public void QueryTokenInfoSample(bool isSandbox)
{
    Gamebase.Push.QueryTokenInfo((data, error)=> 
    {
        if (Gamebase.IsSuccess(error)) 
        {
            // Succeeded.
            bool enablePush = data.agreement.pushEnabled;
            bool enableAdPush = data.agreement.adAgreement;
            bool enableAdNightPush = data.agreement.adAgreementNight;
            string token = data.token;
        } 
        else 
        {
        // Failed.
        }
    });
}
```

#### GamebaseResponse.Push.TokenInfo

| Parameter           | Values                | Description         |
| --------------------| ----------------------| ------------------- |
| pushType            | string                | Push 토큰 타입       |
| token               | string                | 토큰                 |
| userId              | string                | 사용자 아이디         |
| deviceCountryCode   | string                | 국가 코드           |
| timezone            | string                | 표준시간대           |
| registeredDateTime  | string                | 토큰 업데이트 시간    |
| languageCode        | string                | 언어 설정            |
| agreement           | GamebaseResponse.Push.Agreement | 수신 동의 여부        |
| sandbox             | bool                  | sandbox 여부(iOS Only)        |

#### GamebaseResponse.Push.Agreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 알림 표시 동의 여부           |
| adAgreement      | bool | 광고성 알림 표시 동의 여부      |
| adAgreementNight | bool | 야간 광고성 알림 표시 동의 여부  |


#### Setting for APNS Sandbox
* SandboxMode를 켜면, APNS Sandbox로 Push를 발송하도록 등록할 수 있습니다.
* 콘솔 발송 방법
    * Push 메뉴의 **대상**에서 **iOS Sandbox**를 선택한 후 발송합니다.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
public static void SetSandboxMode(bool isSandbox)
```

**Example**

```cs
public void SetSandboxModeSample()
{
    Gamebase.Push.SetSandboxMode(true);
}
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | TOAST Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 푸쉬 API 호출이 완료되지 않았습니다.<br>이전 푸쉬 API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR             | 5999       | 정의되지 않은 푸쉬 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 TOAST Push 라이브러리에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* NHN Cloud Push 오류 코드를 확인하시기 바랍니다.
    * [Android](aos-push#error-handling)<br/>
    * [iOS](ios-push#error-handling)
