## Game > Gamebase > iOS SDK 사용 가이드 > Push

## Push

### Settings

#### Apple Developer Certificates

여기에서는 푸시 알림 전송에 필요한 Apple 개발자 인증서를 생성하는 과정을 설명합니다.

* [Apple Developer 사이트](https://developer.apple.com)의 **Add iOS Certificate**에서 **Apple Push Notification service SSL**로 인증서를 생성합니다.
* Keychain을 등록한 후 생성된 인증서를 Personal Information Exchange (.p12) 형식으로 내보냅니다(export).
* 인증서를 내보낼(export) 때 비밀번호를 설정합니다.

#### TOAST Console 등록
* **Notification > Push > Certificate**에서 **APNS Certificate**와 **APNS (Sandbox) Certificate**에 위에서 생성한 인증서를 등록합니다.
* 위 인증서를 만들 때 설정한 비밀번호를 사용해서 등록합니다.

#### XCode Project 설정
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

다음 API를 호출하여, TOAST Push에 해당 사용자를 등록합니다.<br/>
푸시 동의 여부(enablePush), 광고성 푸시 동의 여부(enableAdPush), 야간 광고성 푸시 동의 여부(enableAdNightPush) 값을 사용자로부터 받아, 다음의 API 호출을 통해 등록을 완료합니다.


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

### Request Push Settings

사용자의 푸시 설정을 조회하기 위해, 다음 API를 이용합니다.<br/>
콜백으로 오는 TCGBPushConfiguration 값으로 사용자 설정값을 얻을 수 있습니다.

```objectivec
- (void)didLoginSucceeded {
    [TCGBPush queryPushWithCompletion:^(TCGBPushConfiguration *configuration, TCGBError *error) {
        if ([TCGBGamebase isSuccessWithError:error] == NO) {
            // To Request Push Configuration Failed.
        }

        BOOL enablePush = configuration.pushEnabled;
        BOOL enableAdPush = configuration.ADAgreement;
        BOOL enableAdNightPush = configuration.ADAgreementNight;

        // You can handle these variables.
    }];
}
```

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR   | 5101       | TOAST Push 라이브러리 오류입니다.<br>DetailCode를 확인하세요. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.<br>이전 푸시 API의 콜백이 실행된 이후에 다시 호출하세요. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR            | 5999       | 정의되지 않은 푸시 오류입니다.<br>전체 로그를 [고객 센터](https://alpha.toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

**TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 TOAST Push 라이브러리에서 발생한 오류입니다.
* 오류 코드 확인은 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // Callback 으로 넘어온 TCGBError 인스턴스
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 외부 라이브러리에서 발생한 오류 객체
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 아래 [tcgbError description] 호출을 통해서 json format의 전체 오류 정보를 얻을 수 있습니다.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* TOAST Push 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [Notification > Push > SDK v1.4 사용 가이드 > 오류 처리](http://alpha-docs.cloud.toast.com/ko/Notification/Push/ko/sdk-guide/#_5)


