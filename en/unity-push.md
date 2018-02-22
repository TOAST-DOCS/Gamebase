## Game > Gamebase > Unity Developer's Guide > Push

This document describes how to set push notifications for each platform.

### Settings

For Android or iOS users, refer to the following documents:

* [Android Push Settings](aos-push#settings)<br/>
* [iOS Push Settings](ios-push#settings)


### Register Push

Register a user to TOAST Push by calling API as below.
With user&#39;s agreement to enablePush, enableAdPush, and enableAdNightPush, call following API to complete registration.

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

To retrieve user's push setting, apply API as below.
From PushConfiguration callback values, you can get user's value set.

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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | Error in TOAST Push library.Please check DetailCode. |
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | Previous Push API call is not completed.Please call again after the previous push API callback is executed.  |
| PUSH_UNKNOWN_ERROR             | 5999       | Undefined push error. Please upload the entire logs to [Customer Center](https://toast.com/support/inquiry), and we'll respond ASAP. |

* Refer to the following document for the entire error codes.
	* [Entire Error Codes](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* Occurs in the TOAST Push library.
* Refer to the following document for TOAST Push error codes.
	* [Notification > Push > SDK v1.4 Guide > Error Handling](/en/Notification/Push/en/Client%20SDK%20Guide/#_5)


