## Game > Gamebase > 스토어 콘솔 가이드 > Galaxy 콘솔 가이드

IAP에서 Galaxy Store를 연동하려면 앱 등록 시, PackageName과 IAP Public Key를 입력해야 합니다.

## Package Name 확인하기
* [Galaxy Store Seller Portal](https://seller.samsungapps.com/main/sellerMain.as) 에서 바이너리 파일 등록 후, 패키지명을 확인합니다. 
* Galaxy Store Seller Portal > 앱 > 앱 선택 > 바이너리
 ![galaxy_app](http://static.toastoven.net/prod_iap/2020/galaxy_app_kr.png)
 

## IAP Public Key 생성하기
> [참고]
> https://developer.samsung.com/iap/isn/requirements.html#Create-an-IAP-key-in-Seller-Portal

* [Galaxy Store Seller Portal](https://seller.samsungapps.com/main/sellerMain.as) > 셀러지원 > IAP 서비스 > IAP Key > IAP Key 만들기

## 콘솔에서 정보 입력하기
[NHN Cloud 콘솔](https://console.nhncloud.com/)에서 조직 및 프로젝트를 선택하고 <strong>Game > Gamebase > 구매(IAP) > 스토어 > 등록</strong> 또는 <strong>Galaxy Store를 선택하고 [수정]</strong>을 클릭합니다.

* 스토어 : Galaxy Store 앱 Package Name 입력
* IAP Key : IAP Key에서 생성한 Public Key 입력

![console_img](https://static.toastoven.net/prod_gamebase/StoreConsoleGuide/GalaxyStore/2026_gamebase_galaxy_store_kr.png)


## 실시간 서버 알림 (ISN) 등록
* 앱 > 앱 선택 > <strong>In App Purchase</strong> > 더보기 > <strong>실시간 서버 알림 (ISN)</strong>
![galaxy_isn](https://static.toastoven.net/prod_iap/console_galaxy/galaxy_isn.png)

* ISN url: ```https://api-iap.nhncloudservice.com/markets/GALAXY/notification/{Galaxy Store Package Name}/receive```
* Gamebase 샌드박스를 사용하고 있다면 ISN url은 ```https://sandbox-api-iap.nhncloudservice.com/markets/GALAXY/notification/{Galaxy Store Package Name}/receive``` 입력