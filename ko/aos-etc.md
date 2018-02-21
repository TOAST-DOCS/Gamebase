## Game > Gamebase > Unity SDK 사용 가이드 > ETC

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

해당 언어코드는 `DisplayLanguage` 클래스에 정의되어 있습니다.

> `[주의]`
>
> Gamebase에서 지원하고 있는 언어코드는 대소문자를 구분합니다.
> "EN" 이나 "zh-cn"과 같이 설정할 경우 문제가 발생할 수 있습니다.

```cs
package com.toast.android.gamebase.base.ui;

public class DisplayLanguage {
    public static class Code {
        public static final String German             = "de";
        public static final String English            = "en";
        public static final String Spanish            = "es";
        public static final String Finish             = "fi";
        public static final String French             = "fr";
        public static final String Indonesian         = "id";
        public static final String Italian            = "it";
        public static final String Japanese           = "ja";
        public static final String Korean             = "ko";
        public static final String Portuguese         = "pt";
        public static final String Russian            = "ru";
        public static final String Thai               = "th";
        public static final String Vietnamese         = "vi";
        public static final String Malay              = "ms";
        public static final String Chinese_Simplified = "zh-CN";
    }
    ...
}
```

#### Gamebase 초기화 시 Display Language 설정

Gamebase 초기화 시 Display Language를 설정할 수 있습니다.

**API**

```java
+ (GamebaseConfiguration)GamebaseConfiguration.Builder.setDisplayLanguageCode(String displayLanguageCode);
```

**Example**

```java
public class MainActivity extends AppCompatActivity {
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        GamebaseConfiguration configuration = new GamebaseConfiguration.Builder()
                                .setAppId("T0aStC1d")
                                .setAppVersion("1.0.0")
                                .enableLaunchingStatusPopup(true)
                                .setDisplayLanguageCode("en")
                                .build();
        
        Gamebase.initialize(activity, configuration, new GamebaseDataCallback<LaunchingInfo>() {
            @Override
            public void onCallback(final LaunchingInfo data, GamebaseException exception) {
                ...
            }
        });
        ...
    }
    ...
}
```

#### Set Display Language

Gamebase 초기화 시 입력된 Display Language를 변경할 수 있습니다.

**API**

```java
+ (void)Gamebase.setDisplayLanguageCode(String displayLanguageCode);
```

**Example**

```java
public void setDisplayLanguageCodeToEnglishInRuntime() {
    Gamebase.setDisplayLanguageCode(DisplayLanguage.Code.English);
}
```

#### Get Display Language

현재 적용된 Display Language를 조회할 수 있습니다.

**API**

```java
+ (String)Gamebase.getDisplayLanguageCode();
```

**Example**

```java
public void getDisplayLanguageCodeInRuntime() {
    String currentDisplayLanguageCode = Gamebase.getDisplayLanguageCode();
}
```

#### 신규 언어셋 추가

Gamebase에서 제공하는 기본 언어(ko, en) 외 다른 언어를 사용해야 할 경우에는 gamebase-sdk.aar > res > raw 에 있는 gamebasedisplay 파일에 값을 추가하여야 합니다.

![gamebasedisplay](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/aos-developers-guide-etc_001_1.7.0.png)

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

초기화 및 SetDisplayLanguageCode API를 통해 Display Language를 설정할 경우, 최종 적용되는 Display Language는 입력한 값과 다르게 적용될 수 있습니다.

1. 입력된 languageCode가 localizedString.json 파일에 정의되어 있는지 확인합니다.
2. Gamebase 초기화 시, 기기에 설정된 언어코드가 localizedString.json 파일에 정의되어 있는지 확인합니다. (이 값은 초기화 이후, 기기에 설정된 언어를 변경하더라도 유지됩니다.)
3. Display Language의 기본값인 `en`이 자동 설정됩니다.