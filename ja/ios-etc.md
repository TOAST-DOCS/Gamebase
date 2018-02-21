## Game > Gamebase > iOS SDK 사용 가이드 > ETC

## Additional Features
Gamebase에서 지원하는 부가적인 기능을 설명합니다.

### Display Language
* Gamebase에서 표시하는 언어를 기기에 설정된 언어가 아닌 다른 언어로 변경할 수 있습니다.
* Gamebase는 클라이언트에 포함되어 있는 메시지를 표시하거나 서버에서 받은 메시지를 표시합니다.
* DisplayLanguage를 설정하게 되면 사용자가 설정한 언어코드(ISO-639)에 적합한 언어로 메시지를 표시합니다.
* 필요하다면 사용자가 지원하고 싶은 언어셋을 추가할 수 있습니다. (하단 지원 언어코드를 참고)

> [참고]
>
> Gamebase의 클라이언트 메시지는 영어(en), 한글(ko)만 포함합니다.

#### Gamebase에서 지원하고 있는 언어코드의 종류
| Code | Name |
| --- | --- |
| de | German |
| en |English  |
| es | Spanish |
| fi | Finish |
| fr | French |
| id | Indonesian |
| it | Italian |
| ja | Japanese |
| ko | Korean |
| pt | Portuguese |
| ru | Russian |
| th | Thai |
| vi | Vietnamese |
| ms | Malay | 
| zh-CN | Chinese-Simplified |
| zh-TW | Chinese-Traditional |

해당 언어코드는 `TCGBConstants.h`에 정의되어 있습니다.

> `[주의]`
>
> Gamebase에서 지원하고 있는 언어코드는 대소문자를 구분합니다.
> "EN" 이나 "zh-cn"과 같이 설정할 경우 문제가 발생할 수 있습니다.

```objectivec
#pragma mark - DisplayLanguageCode
extern NSString* const kTCGBDisplayLanguageCodeGerman;
extern NSString* const kTCGBDisplayLanguageCodeEnglish;
extern NSString* const kTCGBDisplayLanguageCodeSpanish;
extern NSString* const kTCGBDisplayLanguageCodeFinish;
extern NSString* const kTCGBDisplayLanguageCodeFrench;
extern NSString* const kTCGBDisplayLanguageCodeIndonesian;
extern NSString* const kTCGBDisplayLanguageCodeItalian;
extern NSString* const kTCGBDisplayLanguageCodeJapanese;
extern NSString* const kTCGBDisplayLanguageCodeKorean;
extern NSString* const kTCGBDisplayLanguageCodePortuguese;
extern NSString* const kTCGBDisplayLanguageCodeRussian;
extern NSString* const kTCGBDisplayLanguageCodeThai;
extern NSString* const kTCGBDisplayLanguageCodeVietnamese;
extern NSString* const kTCGBDisplayLanguageCodeMalay;
extern NSString* const kTCGBDisplayLanguageCodeChineseSimplified;
extern NSString* const kTCGBDisplayLanguageCodeChineseTraditional;
```


#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

**API**

```objectivec
+ (void)initializeWithConfiguration:(TCGBConfiguration *)configuration completion:(InitializeCompletion)completion;
```

**Example**

```objectivec
- (void)initializeWithDisplayLanguageConfiguration {
        TCGBConfiguration* config = [TCGBConfiguration configurationWithAppID:@"your app(project) ID" appVersion:@"your app version"];
        [config setDisplayLanguageCode:kTCGBDisplayLanguageCodeEnglish];
        [TCGBGamebase initializeWithConfiguration:config completion:^(LaunchingInfo launchingData, TCGBError *error) {
            if ([TCGBGamebase isSuccessWithError:error] == YES) {
                NSLog(@"TCGBGamebase initialization is succeeded");
                // Check status of you app.
                // If status of app is maintenance or terminated service or etc, you must blocking UI and you should make user cannot log in your service.
                // You can use [TCGBLaunching launchingStatus] method to check status of your app.
            }
            else {
                NSLog(@"TCGBGamebase initialization is failed with error:[%@]", [error description]);
            }
        }];
    }
```

#### Set Display Language

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

**API**

```objectivec
+ (void)setDisplayLanguageCode:(NSString *)languageCode;
```

**Example**

```objectivec
- (void)setDisplayLanguageCode {
    [TCGBGamebase setDisplayLanguageCode:kTCGBDisplayLanguageCodeEnglish];
}
```

#### Get Display Language

현재 적용된 Display Language를 조회할 수 있습니다.

**API**

```objectivec
+ (NSString *)displayLanguageCode;
```

**Example**

``` cs
- (void)getDisplayLanguageCode()
{
    NSString* displayLanguage = [TCGBGamebase displayLanguageCode];
}
```

#### 신규 언어셋 추가

Gamebase에서 제공하는 기본 언어(ko, en) 외 다른 언어를 사용하는 경우에는 Gamebase.bundle 파일의 Resource폴더에 있는 **localizedString.json** 파일에 값을 추가합니다.

localizedString.json에 정의되어 있는 형식은 아래와 같습니다.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  }
}
```

일본어를 추가해야 할 경우에는 localizedString.json 파일에 `"ja":{"key":"value"}` 형태로 값을 추가하시면 됩니다.

```json
{
  "en": {
    "common_ok_button": "OK",
    "common_cancel_button": "Cancel",
    ...
    "launching_service_closed_title": "Service Closed"
  },
  "ko": {
    "common_ok_button": "확인",
    "common_cancel_button": "취소",
    ...
    "launching_service_closed_title": "서비스 종료"
  },
  "ja": {
    "common_ok_button": "確認",
    "common_cancel_button": "キャンセル",
    ...
    "launching_service_closed_title": "サービス終了"
  }
}
```

위 json 형식에서 "ja":{ } 내부에 key가 누락될 경우에는 `기기에 설정된 언어` 또는 `en`으로 자동 입력됩니다.

#### Display Language 우선 순위

초기화 및 setDisplayLanguageCode: API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedString.json 파일에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 기기에 설정된 언어코드가 localizedString.json 파일에 정의되어 있는지 확인합니다. (이 값은 초기화 이후, 기기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동 설정됩니다.
