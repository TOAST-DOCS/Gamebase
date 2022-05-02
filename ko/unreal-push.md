## Game > Gamebase > Unreal SDK 사용 가이드 > 푸시

여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

### Settings

Android나 iOS에서 푸시를 설정하는 방법은 다음 문서를 참고하시기 바랍니다.

* Android
    * [Android Push Settings](aos-push/#settings)
    * [Firebase Notification Settings](aos-started/#firebase-notification)
        * Android 빌드 시 res/values/google-services-json.xml 파일이 포함되어야 하므로 가이드를 참고하여 작성합니다.
        * Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml 경로에 파일이 있습니다.
* iOS
    * [iOS Push Settings](ios-push#settings)

> <font color="red">[주의]</font><br/>
>
> 외부 플러그인에서 푸시 관련 처리가 있는 경우, Gamebase 푸시 기능이 정상적으로 동작하지 않을 수 있습니다.

### Register Push

다음 API를 호출하여, NHN Cloud Push에 해당 사용자를 등록합니다.
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush)값을 사용자로부터 받아, 다음 API를 호출해 등록을 완료합니다.

> <font color="red">[주의]</font><br/>
>
> UserID 마다 푸시 설정이 다를 수 있고, 푸시 토큰이 만료되는 경우도 발생할 수 있으므로 로그인 이후에는 매번 RegisterPush API를 호출할 것을 권장합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseErrorDelegate& onCallback);
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseNotificationOptions& notificationOptions, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight };
    
    IGamebase::Get().GetPush().RegisterPush(configuration, FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

### Notification Options

* 단말기에 표시하는 알림을 어떤 형태로 표시할 것인지 Notification Options 를 통해 변경할 수 있습니다.
* 런타임에 RegisterPush API 를 호출하여 변경할 수 있습니다.

#### Set Notification Options with RegisterPush in Runtime

RegisterPush API 호출시 FGamebaseNotificationOptions 인자를 추가하여 알림 옵션을 설정할 수 있습니다.
FGamebaseNotificationOptions 의 생성자에 IGamebase::Get().GetPush().GetNotificationOptions() 호출 결과를 전달하면, 현재의 알림 옵션으로 초기화 된 오브젝트가 생성되므로, 필요한 값만 변경할 수 있습니다.<br/>
설정 가능한 값은 아래와 같습니다.

| API                    | Parameter       | Description        |
| ---------------------  | ------------ | ------------------ |
| foregroundEnabled      | bool         | 앱이 포그라운드 상태일 때의 알림 노출 여부<br/>**default**: false |
| badgeEnabled           | bool         | 배지 아이콘 사용 여부<br/>**default**: true |
| soundEnabled           | bool         | 알림음 사용 여부<br/>**default**: true<br/>**iOS Only** |
| priority               | int32        | 알림 우선 순위. 아래 5가지 값을 설정할 수 있습니다.<br/>GamebaseNotificationPriority::Min : -2<br/> GamebaseNotificationPriority::Low : -1<br/>GamebaseNotificationPriority::Default : 0<br/>GamebaseNotificationPriority::High : 1<br/>GamebaseNotificationPriority::Max : 2<br/>**default**: GamebaseNotificationPriority::High<br/>**Android Only** |
| smallIconName          | FString       | 알림용 작은 아이콘 파일 이름.<br/>설정하지 않을 경우 앱 아이콘이 사용됩니다.<br/>**default**: null<br/>**Android Only** |
| soundFileName          | FString       | 알림음 파일 이름. 안드로이드 8.0 미만 OS 에서만 동작합니다.<br/>'res/raw' 폴더의 mp3, wav 파일명을 지정하면 알림음이 변경됩니다.<br/>**default**: null<br/>**Android Only** |

**Example**

```cpp
void Sample::RegisterPushWithOption(bool pushEnabled, bool adAgreement, bool adAgreementNight, const FString& displayLanguage, bool foregroundEnabled, bool badgeEnabled, bool soundEnabled, int32 priority, const FString& smallIconName, const FString& soundFileName)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight, displayLanguage };
    FGamebaseNotificationOptions notificationOptions{ foregroundEnabled, badgeEnabled, soundEnabled, priority, smallIconName, soundFileName };

    IGamebase::Get().GetPush().RegisterPush(configuration, notificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush succeeded"));
        }
        else
        {
            // Check the error code and handle the error appropriately.
            UE_LOG(GamebaseTestResults, Display, TEXT("RegisterPush failed. (error: %d)"), error->code);
        }
    }));
}
```

#### Get NotificationOptions

푸시를 등록할 때 기존에 설정했던 알림 옵션값을 가져옵니다.

**API**

Supported Platforms

<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
FGamebaseNotificationOptionsPtr GetNotificationOptions();
```

**Example**

```cpp
void Sample::GetNotificationOptions()
{
    auto notificationOptions = IGamebase::Get().GetPush().GetNotificationOptions();
    if (result.IsValid())
    {
        notificationOptions->foregroundEnabled = true;
        notificationOptions->smallIconName = TEXT("notification_icon_name");
        
        FGamebasePushConfiguration configuration;
        
        IGamebase::Get().GetPush().RegisterPush(configuration, notificationOptions, FGamebaseErrorDelegate::CreateLambda([=](const FGamebaseError* error) { }));
    }
    else
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("No GetNotificationOptions"));
    }
}
```


### Request Push Settings

사용자의 푸시 설정을 조회하려면 다음 API를 이용합니다.
콜백으로 받은 FGamebasePushTokenInfo 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void QueryTokenInfo(const FGamebasePushTokenInfoDelegate& onCallback);

// Legacy API
void QueryPush(const FGamebasePushConfigurationDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryTokenInfo()
{
    IGamebase::Get().GetPush().QueryTokenInfo(FGamebasePushTokenInfoDelegate::CreateLambda([=](const FGamebasePushTokenInfo* tokenInfo, const FGamebaseError* error)
    {
        if (Gamebase::IsSuccess(error))
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, tokenInfo= %s)"),
                tokenInfo->pushEnabled ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreement ? TEXT("true") : TEXT("false"),
                tokenInfo->adAgreementNight ? TEXT("true") : TEXT("false"),
                *tokenInfo->token);
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryTokenInfo failed. (error: %d)"), error->code);
        }
    }));
}
```


#### FGamebasePushTokenInfo

| Parameter           | Values                 | Description         |
| --------------------| -----------------------| ------------------- |
| pushType            | FString                | Push 토큰 타입       |
| token               | FString                | 토큰                 |
| userId              | FString                | 사용자 아이디         |
| deviceCountryCode   | FString                | 국가 코드           |
| timezone            | FString                | 표준시간대           |
| registeredDateTime  | FString                | 토큰 업데이트 시간    |
| languageCode        | FString                | 언어 설정            |
| agreement           | FGamebasePushAgreement | 수신 동의 여부        |
| sandbox             | bool                   | sandbox 여부(iOS Only)        |

#### FGamebasePushAgreement

| Parameter        | Values  | Description               |
| -----------------| --------| ------------------------- |
| pushEnabled      | bool | 알림 표시 동의 여부           |
| adAgreement      | bool | 광고성 알림 표시 동의 여부      |
| adAgreementNight | bool | 야간 광고성 알림 표시 동의 여부  |


### Event Handling

* 푸시 메시지가 도착했거나 푸시 메시지를 클릭했을 때 이벤트를 처리할 수 있습니다.
* 이벤트 등록 방법은 GamebaseEventHandler 가이드를 참고하시기 바랍니다.
    * [ Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./unreal-etc/#push-received-message)
    * [ Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./unreal-etc/#push-click-message)
    * [ Game > Gamebase > Unreal SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./unreal-etc/#push-click-action)


#### Setting for APNS Sandbox

* SandboxMode를 켜면, APNS Sandbox로 Push를 발송하도록 등록할 수 있습니다.
* 콘솔 발송 방법
    * Push 메뉴의 **대상**에서 **iOS Sandbox**를 선택한 후 발송합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS

```cpp
void SetSandboxMode(bool isSandbox);
```

**Example**

```cpp
void Sample::SetSandboxMode(bool isSandbox)
{
    IGamebase::Get().GetPush().SetSandboxMode(isSandbox);
}
```


### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud  Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR             | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud  Push 라이브러리에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (Gamebase::IsSuccess(error))
{
    // succeeded
}
else
{
    UE_LOG(GamebaseTestResults, Display, TEXT("code: %d, message: %s"), error->code, *error->message);

    GamebaseInnerError* moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (moduleError.code != GamebaseErrorCode::SUCCESS)
    {
        UE_LOG(GamebaseTestResults, Display, TEXT("moduleErrorCode: %d, moduleErrorMessage: %s"), moduleError->code, *moduleError->message);
    }
}
```

* NHN Cloud  Push 오류 코드를 확인하시기 바랍니다.
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
