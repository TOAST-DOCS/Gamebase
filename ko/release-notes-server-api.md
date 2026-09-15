<!-- pre-align:aligned sig=33a84d66e7f0 -->

<a id="game-gamebase-release-notes-server-api"></a>
## Game > Gamebase > 릴리스 노트 > Server API { #game-gamebase-release-notes-server-api }

<a id="2026-08-25"></a>
### 2026. 08. 25. { #2026-08-25 }

<a id="2026-08-25-added-features"></a>
#### 기능 추가
* Google Chargeback 관련 API 추가

<a id="2026-03-24"></a>
### 2026. 03. 24. { #2026-03-24 }

<a id="2026-03-24-added-features"></a>
#### 기능 추가
* 유저 ID로 Push 토큰을 조회하는 API 추가

<a id="february-24-2026"></a>
### 2026. 02. 24. { #february-24-2026 }

<a id="february-24-2026-added-features"></a>
#### 기능 추가
* 쿠폰 코드로 쿠폰 정보를 조회하는 "Get Coupon Information by Coupon Code" API가 추가

<a id="december-23-2025"></a>
### 2025. 12. 23. { #december-23-2025 }

<a id="december-23-2025-added-features"></a>
#### 기능 추가
* "Withdraw Histories" API의 요청 파라미터에 eventLogType 추가
* Sign in with Apple 계정 관련 처리를 위한 "SIWA Account Webhook" API가 추가

<a id="august-27-2024"></a>
### 2024. 08. 27. { #august-27-2024 }

<a id="august-27-2024-added-features"></a>
#### 기능 추가
* 결제 내역 조회 API request body에 'paymentToken' 필드 추가

<a id="october-31-2023"></a>
### 2023. 10. 31. { #october-31-2023 }

<a id="october-31-2023-added-features"></a>
#### 기능 추가
* 구독의 현재 상태를 조회하는 "Get subscriptions Status" API가 추가

<a id="august-17-2023"></a>
### 2023. 08. 17. { #august-17-2023 }

<a id="august-17-2023-added-features"></a>
#### 기능 추가
* 현재 이용정지 유저들을 조회하는 "Get Ban Members" API가 추가

<a id="july-25-2023"></a>
### 2023. 07. 25. { #july-25-2023 }

<a id="july-25-2023-added-features"></a>
#### 기능 추가
* 구독 조회 API request body에 'includeInactiveGoogleStatuses' 필드 추가
* 구독 조회 API 응답에 'renewTime' 필드 추가
* 구독 조회 API에 한 번에 N개의 스토어를 대상으로 조회할 수 있도록 'marketIds' 필드 추가

<a id="december-27-2022"></a>
### 2022. 12. 27. { #december-27-2022 }

<a id="december-27-2022-added-features"></a>
#### 기능 추가
* Google Play 스토어의 구독 중인 상품 취소 API 추가
* 구독 조회 API 응답에 'linkedPaymentId' 필드 추가

<a id="december-27-2022-feature-updates"></a>
#### 기능 개선/변경
* 특정한 결제 시나리오를 통해 아이템 구매 시 미소비 결제 내역 조회 API 응답 결과에 gamebaseProductId가 null 발생하는 이슈 대응

<a id="august-23-2022"></a>
### 2022. 08. 23. { #august-23-2022 }

<a id="august-23-2022-added-features"></a>
#### 기능 추가
* 서버 URL 추가
	* https://api-gamebase.nhncloudservice.com

<a id="july-26-2022"></a>
### 2022. 07. 26. { #july-26-2022 }

<a id="july-26-2022-added-features"></a>
#### 기능 추가
* 미소비 결제 내역 조회 API에 한 번에 N개의 스토어를 대상으로 조회할 수 있도록 'marketIds'를 추가

<a id="june-30-2022"></a>
### 2022. 06. 30. { #june-30-2022 }

<a id="june-30-2022-added-features"></a>
#### 기능 추가
* 탈퇴 유저가 Apple ID 인증을 사용하고 있다면, Apple ID AccessToken 만료 API 호출 추가
* 미소비 결제 내역 조회 API 응답에 'paymentId' 필드 추가

<a id="june-14-2022"></a>
### 2022. 06. 14. { #june-14-2022 }

<a id="june-14-2022-added-features"></a>
#### 기능 추가
* 결제 트랜잭션 조회 API 추가
* 미소비 결제 내역 조회 API 응답에 'isTestPurchase' 필드 추가

<a id="may-24-2022"></a>
### 2022. 05. 24. { #may-24-2022 }

<a id="may-24-2022-added-features"></a>
#### 기능 추가
* 이용 정지 및 이용 정지 해제 API 추가

<a id="may-10-2022"></a>
### 2022. 05. 10. { #may-10-2022 }

<a id="may-10-2022-added-features"></a>
#### 기능 추가
* 특정 기간 동안 탈퇴한 유저들을 조회하는 API 추가

<a id="september-14-2021"></a>
### 2021. 09. 14. { #september-14-2021 }

<a id="september-14-2021-bug-fixes"></a>
#### 버그 수정
* Leaderboard Wrapping API 수정
	* '다수 사용자 점수/ExtraData 등록' API의 매핑이 잘못된 오류 수정

<a id="march-09-2021"></a>
### 2021. 03. 09. { #march-09-2021 }

<a id="march-09-2021-added-features"></a>
#### 기능 추가
* IdP ID로 Gamebase user ID를 획득하는 API 추가

<a id="august-11-2020"></a>
### 2020. 08. 11. { #august-11-2020 }

<a id="august-11-2020-feature-updates"></a>
#### 기능 개선/변경
* 쿠폰 소진 API의 오류 코드 추가: 쿠폰 코드에 영문, 숫자 이외의 값을 입력한 경우(Error Code:-4000205)

<a id="february-11-2020"></a>
### 2020. 02. 11. { #february-11-2020 }

<a id="february-11-2020-feature-updates"></a>
#### 기능 개선/변경
* 탈퇴 API 호출 시 regUser 길이에 대한 유효성 검사(validation) 추가

<a id="january-14-2020"></a>
### 2020. 01. 14. { #january-14-2020 }

<a id="january-14-2020-added-features"></a>
#### 기능 추가
* 사용자 탈퇴 API 추가

<a id="november-12-2019"></a>
### 2019. 11. 12. { #november-12-2019 }

<a id="november-12-2019-added-features"></a>
#### 기능 추가
* 쿠폰 서비스 신규 오픈: 쿠폰을 대량으로 생성하고 관리하는 기능
	* 쿠폰 확인 및 소비 API 추가

<a id="may-28-2019"></a>
### 2019. 05. 28. { #may-28-2019 }

<a id="may-28-2019-feature-updates"></a>
#### 기능 개선/변경
* LTV 쿼리 수정 및 failover 로직 수정

<a id="2019-03-26"></a>
### 2019. 03. 26. { #2019-03-26 }

<a id="2019-03-26-1"></a>
#### 기능 추가
* TransferAccount 기능 추가: guest 사용자가 매핑없이 최대 2개의 키를 이용하여 새로운 기기로 이전할 수 있는 기능
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
