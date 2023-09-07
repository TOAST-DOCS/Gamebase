## Game > Gamebase > iOS SDK 사용 가이드 > 푸시

> <font color="red">[주의]</font><br/>
>
> Unreal, Unity 등 3rd party 푸시 플러그인 또는 모듈을 사용할 경우, Gamebase 푸시 기능에 영향을 줄 수 있습니다.
>

### Settings

#### APNS JWT 인증 정보 얻기

여기에서는 푸시 알림 전송에 필요한 APNS JWT 인증 정보를 얻는 과정을 설명합니다.

* [Notification > Push > Console Guide > APNS JWT 인증 정보 얻기](https://docs.toast.com/en/Notification/Push/en/console-guide/#get-authentication-information-for-apns-jwt) 가이드를 참고하여 ANPS JWT 등록에 필요한 필수 인증 정보를 얻습니다.

#### Gamebase Console 등록
* **Gamebase > Push > Certificate**에서 **APNS JWT**에 위에서 얻은 정보를 입력합니다.

#### Notification Service Extension 구현
* 수신 지표 수집, 알림음 설정 등을 위해서는 [NHN Cloud Push 가이드](https://docs.toast.com/ko/TOAST/ko/toast-sdk/push-ios/#notification-service-extension)를 참고하여 애플리케이션에 **Notification Service Extension**을 구현해야 합니다.


#### Xcode Project 설정
* **Targets > Capabilities > Push Notifications **항목을 **ON**으로 설정합니다.
* 자동으로 생성된 .entitlements 파일을 열어서, **APS Environment** 키의 값을 알맞은 값으로 설정합니다.
    * **development**: Sandbox APNS
    * **production**:  APNS

#### Import Header File
푸시 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

다음 API를 호출하여, NHN Cloud Push에 해당 사용자를 등록합니다.

푸시 수신 동의 여부(TCGBPushConfiguration)를 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

> <font color="red">[주의]</font><br/>
>
> 푸시 토큰이 언제 만료될지 모르기 때문에, 로그인 이후에는 항상 registerPush API를 호출하는 것을 권장합니다.
>

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```

#### TCGBPushConfiguration

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| pushEnabled                   | M             | BOOL         | 푸시 동의 여부 |
| ADAgreement                   | M             | BOOL         | 광고성 푸시 동의 여부 |
| ADAgreementNight              | M             | BOOL         | 야간 광고성 푸시 동의 여부 |
| alwaysAllowTokenRegistration  | O             | BOOL         | 사용자가 푸시 권한을 거부해도 토큰을 등록할지 여부<br>YES로 설정할 경우 푸시 권한을 획득하지 못하더라도 토큰을 등록합니다.<br>**default**: NO    |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;
    BOOL alwaysAllowTokenRegistration;
    
    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                            ADAgreement:enableAdPush
                                                                        ADAgreementNight:enableAdNightPush
                                                            alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

NHN Cloud Push에 사용자를 등록할 때 TCGBNotificationOptions 객체로 알림 옵션 설정이 가능합니다.

포그라운드 푸시 여부(foregroundEnabled), 배지 사용 여부(badgeEnabled), 알림음 사용 여부(soundEnabled) 값을 사용자로부터 받아, 다음의 API 호출을 통해 알림 옵션 설정이 가능합니다.

#### API

```objectivec
+ (void)registerPushWithPushConfiguration:(TCGBPushConfiguration *)configuration
                      notificationOptions:(nullable TCGBNotificationOptions *)notificationOptions
                               completion:(nullable void(^)(TCGBError * _Nullable error))completion;
```

#### TCGBNotificationOptions

| Parameter     | Mandatory(M) /<br/>Optional(O) | Values            | Description        |
| ------------- | ------------- | ---------------------------------- | ------------------ |
| foregroundEnabled   | M     | BOOL         | 앱이 포그라운드 상태일때의 알림 노출 여부<br/>**default**: NO           |
| badgeEnabled        | M     | BOOL         | 배지 아이콘 사용 여부<br/>**default**: YES           |
| soundEnabled        | M     | BOOL         | 알림음 사용 여부<br/>**default**: YES           |

#### Example

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;
    BOOL alwaysAllowTokenRegistration;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration *pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush
                                                                                   ADAgreement:enableAdPush
                                                                        ADAgreementNight:enableAdNightPush
                                                            alwaysAllowTokenRegistration:alwaysAllowTokenRegistration];
    
    TCGBNotificationOptions *options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled 
                                                                                            badgeEnabled:badgeEnabled 
                                                                                            soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

### Setting for APNS Sandbox

SandboxMode를 켜면, APNS Sandbox로 Push를 발송하도록 등록할 수 있습니다.

* 클라이언트 설정 방법

```objectivec
- (void)didLoginSucceeded {
	[TCGBPush setSandboxMode:YES];
    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError *error) {
    	...
    }];
}
```

* 콘솔 발송 방법

Push 메뉴의 **대상**에서 **iOS Sandbox**를 선택한 후 발송합니다.

### Get NotificationOptions

푸시를 등록할 때 설정한 알림 옵션값을 가져옵니다.

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

> [참고]
>
> foregroundEnabled 옵션은 런타임 때 변경이 가능합니다.
> badgeEnabled, soundEnabled 옵션은 registerPush API 최초 호출 시에만 반영이 되고 런타임 때의 변경은 보장되지 않습니다.
>


### Query Token Info

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다.
콜백으로 오는 TCGBPushTokenInfo 값으로 등록한 푸시 정보를 얻을 수 있습니다.

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryTokenInfoWithCompletion:^(TCGBPushTokenInfo *tokenInfo, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Token Info Failed.
        }

        NSString *pushType = tokenInfo.pushType;
        NSString *token = tokenInfo.token;
        ...
        // You can handle these variables.
    }];
}
```

#### TCGBPushTokenInfo

| Parameter                              | Values                           | Description                        |
| -------------------------------------- | -------------------------------- | ---------------------------------- |
| pushType                               | string                           | Push 토큰 타입                       |
| token                                  | string                           | 토큰                                |
| userId                                 | string                           | 사용자 아이디                         |
| deviceCountryCode                      | string                           | 국가 코드                            |
| timezone                               | string                           | 표준시간대                            |
| registeredDateTime                     | string                           | 토큰 업데이트 시간                      |
| languageCode                           | string                           | 언어 설정                             |
| sandbox                                | YES or NO                        | 샌드박스 환경에서 등록된 토큰인지 확인       |
| agreement                              | TCGBPushAgreement                | 수신 동의 여부                         |

#### TCGBPushAgreement

| Parameter                              | Values                            | Description               |
| -------------------------------------- | --------------------------------- | ------------------------- |
| pushEnabled                            | YES or NO                         | 알림 표시 동의 여부           |
| ADAgreement                            | YES or NO                         | 광고성 알림 표시 동의 여부      |
| ADAgreementNight                       | YES or NO                         | 야간 광고성 알림 표시 동의 여부  |

### Event Handling

* 푸시 메시지가 도착했거나 푸시 메시지를 클릭했을 때 이벤트를 받을 수 있습니다.
* 이벤트 등록 방법은 GamebaseEventHandler 가이드를 참고하시기 바랍니다.
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./ios-etc/#push-received-message)
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./ios-etc/#push-click-message)
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./ios-etc/#push-click-action)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | NHN Cloud Push 라이브러리 오류입니다.<br/>상세 오류를 확인하십시오. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud Push 라이브러리에서 오류가 발생했을 때 반환됩니다.
* NHN Cloud Push 라이브러리에서 발생한 오류 정보는 상세 오류에 포함되어 있으며 상세한 오류 코드 및 메시지는 다음과 같이 확인할 수 있습니다. 

```objectivec
TCGBError *tcgbError = error; // TCGBError object via callback

NSInteger detailErrorCode = [error detailErrorCode];
NSString *detailErrorMessage = [error detailErrorMessage];

// If you use **description** method, you can get entire information of this object by JSON Format
NSLog(@"TCGBError: %@", [tcgbError description]);
```


* NHN Cloud Push 오류 코드는 다음과 같습니다.
    
| 오류 코드 |  설명 |
| --- | --- |
| NHNCloudPushErrorUnknown |           알수 없음 |
| NHNCloudPushErrorNotInitialized |    초기화하지 않음 |
| NHNCloudPushErrorUserInvalid |       사용자 아이디 미설정 |
| NHNCloudPushErrorPermissionDenied |  권한 획득 실패 |
| NHNCloudPushErrorSystemFailed |      시스템에 의한 실패 |
| NHNCloudPushErrorTokenInvalid |      토큰 값이 없거나 유효하지 않음 |
| NHNCloudPushErrorAlreadyInProgress | 이미 진행중 |
| NHNCloudPushErrorParameterInvalid |  매계변수 오류 |
| NHNCloudPushErrorNotSupported |      지원하지 않는 기능 |
| NHNCloudPushErrorClientFailed |      서버 오류 |