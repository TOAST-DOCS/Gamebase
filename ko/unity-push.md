## Game > Gamebase > Unity SDK 사용 가이드 > 푸시

여기에서는 플랫폼별로 푸시 알림을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.

### Settings


Android나 iOS에서 푸시를 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>

* [Android Push Settings](aos-push#settings)<br/>
* [iOS Push Settings](ios-push#settings)


### Register Push

다음 API를 호출하여, TOAST Push에 해당 사용자를 등록합니다.
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void RegisterPush(GamebaseRequest.Push.PushConfiguration pushConfiguration, GamebaseCallback.ErrorDelegate callback)
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

### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다.
콜백으로 오는 PushConfiguration 값으로 사용자 설정값을 얻을 수 있습니다.

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
public static void QueryPush(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Push.PushConfiguration> callback)
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
* TOAST Push 오류 코드를 확인하시기 바랍니다.
    * [Notification > Push > SDK v1.4 사용 가이드 > 오류 처리](/ko/Notification/Push/ko/sdk-guide/#_5)


