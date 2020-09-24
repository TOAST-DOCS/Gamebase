## Game > Gamebase > User Guide for Unreal SDK 사용 가이드 > Push

This document describes setting requirements to enable push notifications on each platform. 여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

### Settings

Regarding push settings on Android or iOS, see the following documents. 에서 푸시를 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>

* [Android Push Settings](aos-push#settings)
    * For a Firebase push, read the guide as below. 푸시를 사용한다면 아래 가이드를 참고 바랍니다.
        * In the guide, Unity build setting must be applied the same for Unreal.  가이드에서 Unity 빌드인 경우는 Unreal의 경우도 동일하게 설정이 필요합니다.
            * For Android buildup, see the guide as reference so as to include the res/values/google-services-json.xml file. 파일이 포함되어야 하므로 가이드를 참고하여 작성합니다.
            * File exists on the Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml path. 경로에 파일이 있습니다.
* [iOS Push Settings](ios-push#settings)

### Register Push

Call the following API to register users for TOAST Push. 다음 API를 호출하여, TOAST Push에 해당 사용자를 등록합니다.
Get a consent from user for EnablePush, EnableAdPush, and EnableAdNightPush, and call the following API to complete with registration. 푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void RegisterPush(const FGamebasePushConfiguration& configuration, const FGamebaseErrorDelegate& onCallback);
```

**Example**

```cpp
void Sample::RegisterPush(bool pushEnabled, bool adAgreement, bool adAgreementNight)
{
    FGamebasePushConfiguration configuration{ pushEnabled, adAgreement, adAgreementNight };
    
    IGamebase::Get().GetPush().RegisterPush(FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

### Request Push Settings

Use the following API to query user's push setting. 사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다.
User configuration 콜백으로 오는 From the PushConfiguration value of the incoming callback, you can get user configuration value. 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNREAL_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNREAL_ANDROID

```cpp
void QueryPush(const FGamebasePushConfigurationDelegate& onCallback);
```

**Example**

```cpp
void Sample::QueryPush()
{
    IGamebase::Get().GetPush().QueryPush(
        FGamebasePushConfigurationDelegate::CreateLambda([](const FGamebasePushConfiguration* pushAdvertisements, const FGamebaseError* error)
    {
        if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryPush succeeded. (pushEnabled= %s, adAgreement= %s, adAgreementNight= %s, displayLanguageCode= %s)"),
                pushAdvertisements->pushEnabled ? TEXT("true") : TEXT("fasle"),
                pushAdvertisements->adAgreement ? TEXT("true") : TEXT("fasle"),
                pushAdvertisements->adAgreementNight ? TEXT("true") : TEXT("fasle"),
                *pushAdvertisements->displayLanguageCode.ToString());
        }
        else
        {
            UE_LOG(GamebaseTestResults, Display, TEXT("QueryPush failed. (error: %d)"), error->code);
        }
    }));
}
```

### Error Handling

| Error                          | Error Code | Description                              |
| ------------------------------ | ---------- | ---------------------------------------- |
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error of TOAST Push library.라이브러리 오류입니다.<br> Check DetailCode를 확인하세요. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous call for push API is yet to be completed. 이전 푸쉬 API 호출이 완료되지 않았습니다.<br> Wait until the previous push API callback is executed and call again. 이전 푸쉬 API의 콜백이 실행된 이후에 다시 호출하세요. |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error. 정의되지 않은 푸쉬 오류입니다.<br> Please upload the entire logs to 전체 로그를 [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. 에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* Read the following document for the entire error codes: 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* This error occurrs from TOAST Push library. 이 오류는 TOAST Push 라이브러리에서 발생한 오류입니다.
* Check your error codes like below: 오류 코드를 확인하는 방법은 다음과 같습니다.

```cpp
GamebaseError* gamebaseError = error; // GamebaseError object via callback

if (error == nullptr || error->code == GamebaseErrorCode::SUCCESS)
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

* Please check TOAST Push error codes.  오류 코드를 확인하시기 바랍니다.
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
