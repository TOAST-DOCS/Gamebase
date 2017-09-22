## Upcoming Products > Gamebase > iOS Developer's Guide > Push

## Push

### Settings

#### Apple Developer Certificates
* **'Developer Site > Add iOS Certificate'**에서 **Apple Push Notification service SSL** 로 인증서를 생성합니다.
* 생성된 인증서를 Keychain이 등록 후, `Personal Information Exchange (.p12)` 포맷으로 export합니다.
* export 시에 비밀번호를 설정합니다.

#### Toast Cloud Console 등록
* **'Notification > Push > Certificate**에서 **APNS Certificate**와 **APNS (Sandbox) Certificate**에 위에서 생성한 인증서를 등록해줍니다.
* 위 인증서를 만들때 설정해놓은 비밀번호를 사용해서 등록합니다.

#### XCode Project 설정
* **'Targets > Capabilities > Push Notifications'**항목을 **ON**으로 설정합니다.
* 자동으로 생성된 .entitlements 파일을 열어서, **'APS Environment'**키의 값을 알맞는 값으로 설정합니다.
	* **development**: Sandbox APNS
	* **production**:  APNS

#### Import Header File

구매 API를 구현하고자 하는 ViewController에 다음의 헤더 파일을 가져옵니다.

```objectivec
#import <Gamebase/Gamebase.h>
```

### Register Push

다음 API를 호출하여, ToastCloud Push에 해당 사용자를 등록합니다.<br/>
Push 동의 여부(enablePush), 광고성 Push 동의 여부(enableAdPush), 야간 광고성 Push 동의 여부(enableAdNightPush)값을 사용자로부터 받아온 후, 다음의 API 호출을 통해 등록을 완료합니다.


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

사용자의 Push 설정을 조회하기 위해서, 다음의 API를 이용합니다.<br/>
콜백으로 오는 TCGBPushConfiguration 값을 바탕으로, 사용자 설정값을 얻을 수 있습니다.

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

| Error | Error Code | Notes |
| ----- | ---------- | ----- |
| TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR | 5101 | TCPush 라이브러리 에러입니다.<br>DetailCode를 확인하세요. |
| TCGB_ERROR_PUSH_ALREADY_IN_PROGRESS_ERROR | 5102 | 이전 PUSH API 호출이 완료되지 않았습니다.<br>이전 PUSH API의 콜백이 실행된 이후에 다시 호출하세요. |
| TCGB_ERROR_PUSH_UNKNOWN_ERROR | 5999 | 정의되지 않은 푸시 에러입니다.<br>전체 로그를 Gamebase 개발팀에 전달하여 에러상황을 문의해 주세요. |

#### TCGB_ERROR_PUSH_EXTERNAL_LIBRARY_ERROR

* 이 에러는 TOAST Cloud Push 라이브러리에서 발생한 에러입니다.
* 에러 코드 확인은 다음과 같이 확인하실 수 있습니다.

```objectivec
TCGBError *tcgbError = error; // Callback 으로 넘어온 TCGBError 인스턴스
NSError *moduleError = [tcgbError.userInfo objectForKey:NSUnderlyingErrorKey]; // 외부 라이브러리에서 발생한 에러객체
NSInteger moduleErrorCode = moduleError.code;
NSString *moduleErrorMessage = moduleError.message;

// 아래 [tcgbError description] 호출을 통해서 json format의 전체 에러 정보를 얻을 수 있습니다.
NSLog(@"TCGBError: %@", [tcgbError description]);
```

* TCPush 에러코드는 다음 문서를 참고하시기 바랍니다.
* [LINK \[Push > Client SDK Developer's Guide > Error Code Guide > 오류 처리\]](http://docs.cloud.toast.com/ko/Notification/Push/ko/Client%20SDK%20Guide/#_5)


