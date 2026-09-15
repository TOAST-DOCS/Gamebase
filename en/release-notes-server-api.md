<!-- pre-align:aligned sig=33a84d66e7f0 -->

<a id="game-gamebase-release-notes-server-api"></a>
## Game > Gamebase > Release Notes > Server API { #game-gamebase-release-notes-server-api }

<a id="2026-08-25"></a>
### 2026. 08. 25. { #2026-08-25 }

<a id="2026-08-25-added-features"></a>
#### Added Features
* Added APIs related to Google Chargeback

<a id="2026-03-24"></a>
### 2026. 03. 24. { #2026-03-24 }

<a id="2026-03-24-added-features"></a>
#### Added Features
* Added an API to retrieve push tokens by user ID

<a id="february-24-2026"></a>
### February 24, 2026 { #february-24-2026 }

<!-- TODO: translate body -->

<a id="february-24-2026-added-features"></a>
#### Added Features

<!-- TODO: translate body -->

<a id="december-23-2025"></a>
### December 23, 2025 { #december-23-2025 }

<!-- TODO: translate body -->

<a id="december-23-2025-added-features"></a>
#### Added Features

<!-- TODO: translate body -->

<a id="august-27-2024"></a>
### August 27, 2024 { #august-27-2024 }

<!-- TODO: translate body -->

<a id="august-27-2024-added-features"></a>
#### Added Features

<!-- TODO: translate body -->

<a id="october-31-2023"></a>
### October 31, 2023 { #october-31-2023 }

<a id="october-31-2023-added-features"></a>
#### Added Features
* Added the `Get Subscription Status` API to retrieve the current status of subscriptions

<a id="august-17-2023"></a>
### August 17, 2023 { #august-17-2023 }

<a id="august-17-2023-added-features"></a>
#### Added Features
* Added the `Get Ban Members` API to retrieve users who are banned from using the service

<a id="july-25-2023"></a>
### July 25, 2023 { #july-25-2023 }

<a id="july-25-2023-added-features"></a>
#### Added Features
* Add 'includeInactiveGoogleStatuses' field to the List Active Subscriptions API request body
* Added 'renewTime' field to the List Active Subscriptions API response
* Added 'marketIds' field to the List Active Subscriptions API to allow querying against N stores at once.

<a id="december-27-2022"></a>
### December 27, 2022 { #december-27-2022 }

<a id="december-27-2022-added-features"></a>
#### Added Features

* Add an API to cancel products in subscription for Google Play Store
* Added the 'linkedPaymentId' field to response results of the "List Active Subscriptions" API

<a id="december-27-2022-feature-updates"></a>
#### Feature Updates
* Fixed an issue where, when purchasing items through a specific payment scenario, null occurs in gamebaseProductId for response results of the list non-consumed payment API

<a id="august-23-2022"></a>
### August 23, 2022 { #august-23-2022 }

<a id="august-23-2022-added-features"></a>
#### Added Features

* Added the server URL
	* https://api-gamebase.nhncloudservice.com

<a id="july-26-2022"></a>
### July 26, 2022 { #july-26-2022 }

<a id="july-26-2022-added-features"></a>
#### Added Features
* Added 'marketIds' to the "List Consumables" API that queries the unconsumed payment history so that multiple stores can be viewed at a time. 

<a id="june-30-2022"></a>
### June 30, 2022 { #june-30-2022 }

<a id="june-30-2022-added-features"></a>
#### Added Features
* Added Apple ID AccessToken expiry API call in case the withdrawn user is using Apple ID authentication
* Added the 'paymentId' field to the response for unconsumed payment history query API

<a id="june-14-2022"></a>
### June 14, 2022 { #june-14-2022 }

<a id="june-14-2022-added-features"></a>
#### Added Features
* Added the payment transaction query API
* Added the 'isTestPurchase' field to the response for the unconsumed payment history query API

<a id="may-24-2022"></a>
### May 24, 2022 { #may-24-2022 }

<a id="may-24-2022-added-features"></a>
#### Added Features
* Added the Ban and Ban Release APIs

<a id="may-10-2022"></a>
### May 10, 2022 { #may-10-2022 }

<a id="may-10-2022-added-features"></a>
#### Added Features
* Added an API to query users who have withdrawn during a specific period

<a id="september-14-2021"></a>
### September 14, 2021 { #september-14-2021 }

<a id="september-14-2021-bug-fixes"></a>
#### Bug Fixes
* Modified Leaderboard Wrapping API
	* Fixed an error where mapping of Register Scores/ExtraData of Multiple Users API is wrong.

<a id="march-09-2021"></a>
### March 09, 2021 { #march-09-2021 }

<a id="march-09-2021-added-features"></a>
#### Added Features
* Added an API that can be used to acquire Gamebase user ID with IdP ID

<a id="august-11-2020"></a>
### August 11, 2020 { #august-11-2020 }

<a id="august-11-2020-feature-updates"></a>
#### Feature Updates
* Added error code for Coupon Expired API: When a coupon code includes a value other than English or numbers (Error Code:-4000205)

<a id="february-11-2020"></a>
### February 11, 2020 { #february-11-2020 }

<a id="february-11-2020-feature-updates"></a>
#### Feature Updates
* Added validation for the regUser length when Withdraw API is called

<a id="january-14-2020"></a>
### January 14, 2020 { #january-14-2020 }

<a id="january-14-2020-added-features"></a>
#### Added Features
* Added Withdraw Users API

<a id="november-12-2019"></a>
### November 12, 2019 { #november-12-2019 }

<a id="november-12-2019-added-features"></a>
#### Added Features
* Coupon Service Newly Open: Create and manage coupons in large quantity
	* Find coupons and add Consume API

<a id="may-28-2019"></a>
### May 28, 2019 { #may-28-2019 }

<a id="may-28-2019-feature-updates"></a>
#### Feature Updates
* Modified LTV queries and the failover logic

<a id="2019-03-26"></a>
### 2019. 03. 26. { #2019-03-26 }

<a id="2019-03-26-1"></a>
#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
	- (Server API)
		* 발급된 TransferAccount의 ID/PW 검증하는 서버 API (validateTransferAccount)

<a id="2018-06-26"></a>
### 2018. 06. 26. { #2018-06-26 }

<a id="2018-06-26-1"></a>
#### 기능 추가
* getSimpleLaunching : 클라이언트 앱 기동시 제공되는 Launching 정보 확인용 API

<a id="2017-11-30"></a>
### 2017. 11. 30. { #2017-11-30 }

<a id="2017-11-30-1"></a>
#### 기능 개선/변경
* [점검 조회 API](./api-guide/#check-maintenance-set) 결과를 List 에서 단일 객체로 변경

<a id="2017-04-04"></a>
### 2017. 04. 04. { #2017-04-04 }

<a id="2017-04-04-1"></a>
#### 기능 개선/변경
* [IAP](./api-guide/#purchase-iap) API 연동 : 아이템 조회, 미소비내역 조회
* checkAccessToken API 응답 결과에, 로그인 시 사용된 IdP 관련 정보 포함하는 스펙 추가


<a id="2017-03-21"></a>
### 2017. 03. 21. { #2017-03-21 }

<a id="2017-03-21-1"></a>
#### 기능 개선/변경
* [Leaderboard](./api-guide/#leaderboard), [IAP](./api-guide/#purchase-iap) API 연동

<a id="2017-03-09"></a>
### 2017. 03. 09. { #2017-03-09 }

<a id="2017-03-09-1"></a>
#### 신규 상품 출시
* 게임에서 공통적으로 필요한 기능들을 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.
	* 다양한 인증 지원 : Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증
	* 로그아웃 및 회원탈퇴 기능을 제공
	* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공
	* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공
	* 실시간 운영지표 확인 가능한 웹콘솔 화면 제공
	* TOAST Cloud상품 연동 : PUSH, IAP
