## Mobile Service > IAP > ONEStore 콘솔 가이드

> [공지]
> 구독 결제를 지원하는 신규 IAP SDK가 [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/)로 출시됐습니다.
> 기존 IAP SDK는 더 이상 신규 기능을 개발하지 않을 예정입니다.
> 본 문서는 [TOAST SDK](http://docs.toast.com/ko/TOAST/ko/toast-sdk/overview/) 가이드입니다.


원스토어에서 라이선스 키 및 OAuth 인증 정보를 생성하여 IAP 앱 정보에 등록합니다.

### 원스토어 키 생성
```
Apps > 앱 선택 >In-App정보 > 인증 및 라이선스
```
![원스토어 인증 및 라이선스 확인](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap_52.PNG)

<br>

![[]](http://static.toastoven.net/prod_gamebase/StoreConsoleGuide/iap-console-onestore-edit.png)

<br>

[표] 원스토어 v17 연동을 위한 앱 등록 필드

| 필드         | 설명                             |
| ------------- | ------------------------------ |
| Store ID     | 스토어 리스트에서 ONE Store v17 선택|
| App Name      | IAP Console에서 사용할 이름|
| ONE Store Client ID | 스토어에 등록한 ClientID |
| ONE Store Client Secret | 스토어 Oauth 인증 정보 중 Client Secret |
| ONE Store License Key | 스토어 Oauth 인증 정보 중 License Key|

