## Game > Gamebase > User Guide for Unreal SDK > Push

This document describes setting requirements to enable push notifications on each platform. 

### Settings

Regarding push settings on Android or iOS, see the following documents. <br/>

* [Android Push Settings](aos-push#settings)
    * For a Firebase push, read the guide as below.
        * In the guide, Unity build setting must be applied the same for Unreal.  
            * For Android buildup, see the guide as reference so as to include the res/values/google-services-json.xml file. 
            * File exists on the Plugins/Gamebase/Source/Gamebase/ThirdParty/Android/res/values/google-services-json.xml path. 
* [iOS Push Settings](ios-push#settings)

### Register Push

Call the following API to register users for TOAST Push. 
Get a consent from user for EnablePush, EnableAdPush, and EnableAdNightPush, and call the following API to complete with registration. 

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

### Request Push Settings

Use the following API to query user's push setting. 
You can get user configuration value from the PushConfiguration value of the incoming callback. 

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
        if (Gamebase::IsSuccess(error))
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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error of TOAST Push library.<br> Check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | Previous call for push API is yet to be completed. <br> Wait until the previous push API callback is executed and call again. |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error. <br> Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry) and we'll reply at the earliest possible moment. |

* Read the following document for the entire error codes: 
    * [Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* This error occurs from TOAST Push library. 
* Check your error codes like below: 

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

* Please check TOAST Push error codes.  
    * [Android](aos-push#error-handling)
    * [iOS](ios-push#error-handling)
