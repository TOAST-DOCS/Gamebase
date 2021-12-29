## Game > Gamebase > iOS SDK 사용 가이드 > 푸시

### Settings

#### Apple Developer Certificates

여기에서는 푸시 알림 전송에 필요한 Apple 개발자 인증서를 생성하는 과정을 설명합니다.

* [Apple Developer 사이트](https://developer.apple.com)의 **Add iOS Certificate**에서 **Apple Push Notification service SSL**로 인증서를 생성합니다.
* Keychain을 등록한 후 생성된 인증서를 Personal Information Exchange (.p12) 형식으로 내보냅니다(export).
* 인증서를 내보낼(export) 때 비밀번호를 설정합니다.

#### NHN Cloud Console 등록
* **Notification > Push > Certificate**에서 **APNS Certificate**와 **APNS (Sandbox) Certificate**에 위에서 생성한 인증서를 등록합니다.
* 위 인증서를 만들 때 설정한 비밀번호를 사용해서 등록합니다.

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

다음 API를 호출하여, NHN Cloud Push에 해당 사용자를 등록합니다.<br/>
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.

> <font color="red">[주의]</font><br/>
>
> 푸시 토큰이 언제 만료될지 모르기 때문에, 로그인 이후에는 항상 registerPush API를 호출하는 것을 권장합니다.
>

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    // You should receive the above values to the logged-in user.

    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];

    [TCGBPush registerPushWithPushConfiguration:pushConfig completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
}
```

NHN Cloud Push에 사용자를 등록할 때 TCGBNotificationOptions 객체로 알림 옵션 설정이 가능합니다.<Mb>
포그라운드 푸시 여부(foregroundEnabled), 배지 사용 여부(badgeEnabled), 알림음 사용 여부(soundEnabled) 값을 사용자로부터 받아, 다음의 API 호출을 통해 알림 옵션 설정이 가능합니다.

```objectivec
- (void)didLoginSucceeded {
    BOOL enablePush;
    BOOL enableAdPush;
    BOOL enableAdNightPush;

    BOOL foregroundEnabled;
    BOOL badgeEnabled;
    BOOL soundEnabled;

    // You should receive the above values to the logged-in user.
    
    TCGBPushConfiguration* pushConfig = [TCGBPushConfiguration pushConfigurationWithPushEnable:enablePush ADAgreement:enableAdPush ADAgreementNight:enableAdNightPush];
    
    TCGBNotificationOptions* options = [TCGBNotificationOptions notificationOptionsWithForegroundEnabled:foregroundEnabled badgeEnabled:badgeEnabled soundEnabled:soundEnabled];

    [TCGBPush registerPushWithPushConfiguration:pushConfig notificationOptions:options completion:^(TCGBError* error) {
        if (error != nil) {
            // To Register Push Failed.
        }
    }];
    // You should receive the above values to the logged-in user.
}
```

#### Setting for APNS Sandbox

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

#### Get NotificationOptions

푸시를 등록할 때 설정한 알림 옵션값을 가져옵니다.

```objectivec
- (void)didLoginSucceeded {
    TCGBNotificationOptions *options = [TCGBPush notificationOptions];

    if (options == nil) {
        // You need to login and call the registerPush API first.
    }
}
```

#### TCGBNotificationOptions

| Parameter             | Values       | Description        |
| --------------------  | ------------ | ------------------ |
| foregroundEnabled     | YES or NO    | 앱이 포그라운드 상태일때의 알림 노출 여부<br/>**default**: NO           |
| badgeEnabled          | YES or NO    | 배지 아이콘 사용 여부<br/>**default**: YES           |
| soundEnabled          | YES or NO    | 알림음 사용 여부<br/>**default**: YES           |


> [참고]
>
> foregroundEnabled 옵션은 런타임 때 변경이 가능합니다.
> badgeEnabled, soundEnabled 옵션은 registerPush API 최초 호출 시에만 반영이 되고 런타임 때의 변경은 보장되지 않습니다.
>


### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다.<br/>
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

* 푸시 메세지가 도착했거나 푸시 메세지를 클릭했을 때 이벤트를 받을 수 있습니다.
* 이벤트 등록 방법은 GamebaseEventHandler 가이드를 참고하시기 바랍니다.
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Received Message](./ios-etc/#push-received-message)
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Message](./ios-etc/#push-click-message)
    * [ Game > Gamebase > iOS SDK 사용 가이드 > ETC > Additional Features > Gamebase Event Handler > Push Click Action](./ios-etc/#push-click-action)

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | NHN Cloud Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 NHN Cloud Push 라이브러리에서 발생한 오류입니다.
* 오류 코드 확인은 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // Callback 으로 넘어온 TCGBError 인스턴스
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 외부 라이브러리에서 발생한 오류 객체
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 아래 [tcgbError description] 호출을 통해서 json format의 전체 오류 정보를 얻을 수 있습니다.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* NHN Cloud Push 오류 코드는 다음과 같습니다.
    
| 오류 코드 |  설명 |
| --- | --- |
| TCPushErrorNotInitialized | 초기화되지 않음 |
| TCPushErrorInvalidParameters | 파라미터 오류 |
| TCPushErrorPermissionDenined | 권한 미획득 |
| TCPushErrorSystemFail | 시스템 알림 등록 실패 |
| TCPushErrorNetworkFail | 네트워크 송수신 실패 |
| TCPushErrorServerFail | 서버 응답 실패 |
| TCPushErrorInvalidUrl | 잘못된 URL 요청 |
| TCPushErrorNetworkNotReachable | 네트워크 미연결 |

