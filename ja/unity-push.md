## Game > Gamebase > Unity SDK ご利用ガイド > Push

ここではプラットフォームごとにPush通知を使用するために必要な設定方法についてご案内いたします。

### Settings


AndroidやiOSでPushを設定する方法は、次のドキュメントをご参考ください。<br/>

* [Android Push Settings](aos-push#settings)<br/>
* [iOS Push Settings](ios-push#settings)


### Register Push

次のAPIを呼び出してNHN Cloud Pushに該当するユーザーを登録します。
Pushの同意状態(enablePush)、Push型広告の同意状態(enableAdPush)、夜間のPush型広告の同意状態(enableAdNightPush)の値をユーザーから取得し、次のAPIを呼び出して登録を完了させます。


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

ユーザーのPush設定を照会するために、次のAPIを利用します。
コールバックによるPushConfigurationの値からユーザー設定値を取得することができます。

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
| PUSH_EXTERNAL_LIBRARY_ERROR    | 5101       | NHN Cloud Pushライブラリーエラーです。<br>DetailCodeを確認してください。|
| PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 前回のPush APIの呼び出しが完了していません。<br> 前回のPush APIのコールバックが実行された後、もう一度呼び出してください。|
| PUSH_UNKNOWN_ERROR             | 5999       | 定義されていないPushエラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PUSH_EXTERNAL_LIBRARY_ERROR**

* このエラーは、NHN Cloud Pushライブラリーで発生したエラーです。
* エラーコードは次のように確認できます。

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

* NHN Cloud Pushのエラーコードを確認してください。
    * [Android](aos-push#error-handling)<br/>
    * [iOS](ios-push#error-handling)



