## Game > Gamebase > Release Notes > Server API

### August 23, 2022

#### Added Features

* Added the server URL
	* https://api-gamebase.nhncloudservice.com

### July 26, 2022

#### Added Features
* Added 'marketIds' to the "List Consumables" API that queries the unconsumed payment history so that multiple stores can be viewed at a time. 

### June 30, 2022

#### Added Features
* Added Apple ID AccessToken expiry API call in case the withdrawn user is using Apple ID authentication
* Added the 'paymentId' field to the response for unconsumed payment history query API

### June 14, 2022

#### Added Features
* Added the payment transaction query API
* Added the 'isTestPurchase' field to the response for the unconsumed payment history query API

### May 24, 2022

#### Added Features
* Added the Ban and Ban Release APIs

### May 10, 2022

#### Added Features
* Added an API to query users who have withdrawn during a specific period

### September 14, 2021

#### Bug Fixes
* Modified Leaderboard Wrapping API
	* Fixed an error where mapping of Register Scores/ExtraData of Multiple Users API is wrong.

### March 09, 2021

#### Added Features
* Added an API that can be used to acquire Gamebase user ID with IdP ID

### August 11, 2020

#### Feature Updates
* Added error code for Coupon Expired API: When a coupon code includes a value other than English or numbers (Error Code:-4000205)

### February 11, 2020

#### Feature Updates
* Added validation for the regUser length when Withdraw API is called

### January 14, 2020

#### Added Features
* Added Withdraw Users API

### November 12, 2019

#### Added Features
* Coupon Service Newly Open: Create and manage coupons in large quantity
	* Find coupons and add Consume API

### May 28, 2019

#### Feature Updates
* Modified LTV queries and the failover logic

### 2019. 03. 26.

#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
	- (Server API)
		* 발급된 TransferAccount의 ID/PW 검증하는 서버 API (validateTransferAccount)

### 2018. 06. 26.

#### 기능 추가
* getSimpleLaunching : 클라이언트 앱 기동시 제공되는 Launching 정보 확인용 API

### 2017. 11. 30.

#### 기능 개선/변경
* [점검 조회 API](./api-guide/#check-under-maintenance) 결과를 List 에서 단일 객체로 변경

### 2017. 04. 04.

#### 기능 개선/변경
* [IAP](./api-guide/#purchaseiap) API 연동 : 아이템 조회, 미소비내역 조회
* checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가


### 2017. 03. 21.

#### 기능 개선/변경
* [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchaseiap) API 연동

### 2017. 03. 09.

#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
