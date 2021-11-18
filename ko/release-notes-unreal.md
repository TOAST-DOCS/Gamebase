## Game > Gamebase > 릴리스 노트 > Unreal

### 2.26.1 (2021.11.23)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* GamebaseDisplayLanguageCode 핀란드어 오타 수정
    * Finish → Finnish

### 2.26.0 (2021.09.28)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.26.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* 공통약관 기능 추가
    * 약관 WebView를 여는 API 추가
    * 약관 리스트 및 유저별 동의 여부를 조회하는 API 추가
    * 유저별 약관 동의 여부를 Gamebase 서버에 저장하는 API 추가

#### 기능 개선/변경
* 고객센터 타입이 TOAST 조직 상품(Online Contact)인 경우 로그인을 하지 않아도 고객센터가 표시되도록 변경
* 내부 론칭 URL 변경
* Gamebase에서 Android multidex 적용 제거

### 2.19.2 (2021.06.29)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.2/GamebaseSDK-Unreal.zip)

#### 버그 수정
* 이미지 공지 ShowImageNotices API 호출 시 onEventCallback을 등록하지 않는 경우 닫기 버튼을 눌렀을 때 크래시가 발생하는 문제 수정
* Android 설정 툴 - Enable Hangame, Enable Weibo가 정상 동작하지 않는 문제 수정

### 2.19.1 (2021.02.09)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.1/GamebaseSDK-Unreal.zip)

#### 버그 수정
* Unity 빌드 중 제외되는 파일이 생길 때 발생하는 컴파일 오류 수정

### 2.19.0 (2021.01.26)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.19.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* SDK 배포: 2.16.0 ~ 2.19.0 누적된 내역 반영
    * [Android 설정 툴](https://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#android-settings) 제공: Gamebase_Android_UPL.xml 파일을 수정하는 대신 설정 툴을 사용바랍니다.
    * 고객센터 기능 추가    
    * 인증 추가: Hangame, Weibo
    * Galaxy 스토어 추가
    * 결제 아이템 정보에 지역화된 상품 정보 추가: localizedTitle, localizedDescription
    * Android 설정 툴 제공
    * Unreal 4.26 지원
    
### 2.15.0 (2020.10.27)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.15.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* Unreal SDK 기능 추가: SDK 2.15.0
    * 기존의 모든 이벤트 시스템을 통합하는 GamebaseEventHandler 추가
        * ServerPush, Observer 기능을 포함하고 있고, 프로모션 결제 이벤트 및 푸시 이벤트 확인 가능
    * API 추가
        * 상품 ID로 결제 요청하고 추가 정보(UserPayload)를 입력해 결제 완료 시 확인 가능한 결제 API 추가
        * 이미지 공지 표시: showImageNotices
        * Push 토큰 정보 확인: queryTokenInfo
    * 푸시 토큰 등록 시 NotificationOption 설정으로 앱이 포그라운드(foreground) 상태에서도 푸시 알림을 받을 수 있도록 기능 추가
    * WebViewConfiguration contentMode 설정 추가
    
#### 기능 개선/변경
* [SDK] 2.15.0
    * (Unreal) TOAST SDK 업데이트: Android(0.23.0), iOS(0.26.0), Unity(0.21.0)    

#### 버그 수정
* [SDK] 2.15.0    
    * (Unreal) 결제 모듈에 ProGuard 선언이 누락된 오류 수정

### 2.9.1 (2020.08.25)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.1/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [SDK] 2.9.1
    * (Unreal) Unreal 4.22 ~ 4.25 지원
    * (Unreal) PLCrashReporter 이슈 지원: [가이드](http://docs.toast.com/ko/Game/Gamebase/ko/unreal-started/#ios-settings)

#### 기능 개선/변경
* [SDK] 2.9.1
    * (Unreal) iOS Plugin 내부 Gamebase SDK for iOS 버전 업데이트(2.9.1)
    * (Unreal) UObject 레퍼런싱 처리가 누락된 부분을 수정  
   
### 2.9.0 (2020.05.12)
[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gamebase/v2.9.0/GamebaseSDK-Unreal.zip)

#### 기능 추가
* [SDK] 2.9.0
    * (Unreal) SDK 신규 배포
