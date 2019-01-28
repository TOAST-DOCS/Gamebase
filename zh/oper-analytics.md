## Game > Gamebase > 콘솔 사용 가이드 > Analytics

앱 이용자의 현황, 매출과 관련된 지표들을 표와 그래프로 확인할 수 있습니다.
Analytics는 다음의 메뉴로 구성되어 있습니다.

* 실시간 모니터링: 앱 이용자들의 실시간 동접 및 실시간 결제 지표
* 이용자 지표: 앱 이용자들 중심의 기본 지표(DAU, MCU, NRU) 및 환경, 유입/유출, Retention과 같은 지표
* 매출 지표: 앱의 매출 관련 지표
* 그룹 동접: Gamebase 서비스 이용자가 속한 프로젝트의 그룹 동접과 그룹별 기본 지표
* 이용 환경: 설치 URL 호출에 대한 통계 지표

## Real-time Monitoring
### Concurrent User
![gamebase_analytics_01_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_01_201901_2.png)

현재 앱 이용자의 실시간 동접 지표 및 점검, 푸시 정보를 확인할 수 있습니다.

#### 1. 실시간 동접(CCU) 변화 그래프
1분마다 데이터를 갱신하여, 실시간으로 변경된 지표를 확인할 수 있습니다.

* 동시 접속자(CCU): 1분 단위로 측정된 실시간 동시 접속자 수 (로그인 이용자 수)
* 결제금액: 당일 0시~24시까지 이용자가 Gamebase에서 결제한 금액의 합으로 환불, 결제 취소 등의 정보가 포함되지 않은 순수 결제 금액을 말합니다.
* 신규 가입자(NRU): 신규 가입자. 당일 0시~24시까지 로그인 로그가 최초 수집된 유저 (memberno 기준)

#### 2. 점검 정보
당일 0시~24시까지 Gamebase에 등록된 점검 정보

#### 3. 푸시 정보
당일 0시~24시까지 Gamebase에 발송된 PUSH 정보

### Dashboard
![gamebase_analytics_02_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_02_201901_2.png)

실시간 이용자 현황에 대한 여러 지표를 한눈에 확인할 수 있습니다.

#### 1. 실시간 이용자 현황
앱 이용자 및 결제 관련 지표를 확인할 수 있습니다.
선택된 일자가 오늘이면 10분마다 갱신되고, 이전 날이면 해당 일의 지표를 보여줍니다.

* 동시 접속자(CCU): 10분 단위로 측정된 실시간 동시 접속자 수 (로그인 이용자 수)
* 최대 동접자(MCU): 0시~24시까지 최대 동접자 수 (1분 단위 CCU 중 가장 큰 값)
* 신규 가입자(NRU): 당일 0시 ~ 24시까지, 로그인 로그가 최초 수집된 신규 유저 (memberno 기준)
* 일 사용자(DAU): 일간 memberno 기준 로그인 1회 이상 액티브 이용자 수 (Daily Active Users)
* avg.Playtime: 일자별 전체 Playtime의 평균 (DAU의 Playtime의 합 / DAU)
* 누적 이용자: Gamebase 설치 후 가입된 전체 누적 유저 수 (memberno 기준)
* 결제금액: 0시~24시까지 이용자가 결제한 총 결제금액
* 결제 건수: 0시~24시까지 이용자의 총 결제 건수
* PU: 유료상품을 결제한 이용자 (Paying User). (=재구매 PU + 신규 PU)
* 신규 PU (NPU): 유료 상품을 처음 결제한 이용자 (New Paying Users)
* ARPPU: 결제 이용자 (PU) 1인이 결제한 평균 결제금액 (=결제금액/PU)
* ARPU: 게임 이용자 (DAU) 1인이 결제한 평균 결제금액 (=결제금액/DAU)

※ MCU, ACU, ARPU의 경우 필터가 전체일 경우만 확인할 수 있습니다.

#### 2. 주요 지표 실시간 변화 그래프
동시 접속자(CCU), 신규 가입자(NRU), 결제금액, PU 지표에 대해 10분 간격의 변화를 그래프로 확인할 수 있습니다.

#### 3. 실시간 점유율 현황
이용자의 OS, APP 버전, Store, 국가에 대한 점유율을 그래프로 확인할 수 있습니다.
일자가 오늘이면 CCU를 기준으로, 이전 날이면 DAU를 기준으로 보여줍니다.

* OS 점유율: DAU 중 OS 별 점유 비율 (당일은 CCU 기준)
* APP 버전 점유율: DAU 중 APP 버전 점유 비율 (당일은 CCU 기준)
* Store 점유율: DAU 중 Store 점유 비율 (당일은 CCU 기준)
* 국가별 점유율: DAU 중 국가별 점유 비율 (당일은 CCU 기준)

## User Statistics
### User
![gamebase_analytics_03_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_03_201901_2.png)

이용자의 기본 지표들을 확인할 수 있습니다.

#### 1. 이용자 현황
선택된 기간 동안의 이용자 기본 지표들을 보여줍니다.

* 일간 총 이용자(DAU+): 일간 memberno 기준 로그인 1회 이상 액티브 이용자 수의 합 (Daily Active Users)
* 주간 총 이용자(WAU+): 주간 DAU의 합 (Weekly Active Users). 주간 지표 선택 시, DAU 항목이 WAU로 대체
* 월간 총 이용자(MAU+): 월간 DAU의 합 (Monthly Active Users) 월간 지표 선택 시, DAU 항목이 MAU로 대체
* 최대 동접자 수(MCU): 0시~24시까지 최대 동접자 수. 1분 단위 CCU 값에서 가장 큰 값을 1일 단위로 집계함.
* 신규 가입자(NRU): 신규 가입자. 당일 0시~24시까지 로그인 로그가 최초 수집된 유저 (memberno 기준)
* 탈퇴 유저: 탈퇴 유저. 당일 0시~24시까지 memberno 가 삭제된 유저 수
* Avg.CCU: 선택된 기간 동안의 CCU의 평균
* Avg.Playtime: Gamebase 설치 후 가입된 전체 누적 유저 수 (memberno 기준)

#### 2. 일별 지표
선택된 기간 동안의 일별 이용자 기본 지표를 그래프와 표로 보여줍니다.

※ MCU, 누적 이용자(ACU)의 경우 필터가 전체일 경우만 확인할 수 있습니다.

### User Environment
![gamebase_analytics_04_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_04_201901_2.png)

이용 환경에 따른 이용자의 지표를 확인할 수 있습니다.

* 조회 조건
    * OS: 모바일 운영 소프트웨어 Android, IOS 등
    * Country: 이용자 모바일기기에 설정된 국가
    * Store: IdP: 페이스북, 구글 등 이용자의 IdP 로그인/인증 정보
    * App Version: 실행된 앱의 버전 정보
    * Device: 이용자의 디바이스 기기 종류. Device 선택 시 PU, 결제금액은 제공하지 않음.(주간, 월간 데이터도 제공하지 않음)
* 조회 값
    * 일 사용자(DAU): 일간 memberno 기준 로그인 1회 이상 액티브 이용자 수 (Daily Active Users)
    * 신규 가입자(NRU): 신규 가입자. 당일 0시~24시까지 로그인 로그가 최초 수집된 유저 (memberno 기준)
    * PU: 유료상품을 결제한 이용자 (Paying User). (=재구매 PU + 신규 PU)
    * 결제금액: 이용자가 결제한 총 결제금액

### User Inflow and Outflow
![gamebase_analytics_05_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_05_201901_2.png)

앱 이용자의 유입, 유출에 대한 일자별 추이를 확인할 수 있습니다.

* 유입 이용자(신규+복귀): 유입 이용자는 신규 가입자와 복귀 이용자의 합. (=신규 가입자 + 복귀 이용자)
* 신규 가입자: 신규 가입자. 당일 0시~24시까지 로그인 로그가 최초 수집된 유저 (memberno 기준)
* 복귀 이용자: 기준일에 이전 8일 동안 로그가 수집되지 않은 이용자
* 유출 이용자(탈퇴+이탈): 유출 이용자는 탈퇴 이용자와 이탈 이용자의 합. (=탈퇴 이용자 + 이탈 이용자)
* 탈퇴 이용자: 탈퇴 유저. 당일 0시~24시까지 memberno 가 삭제된 유저 수
* 이탈 이용자: 기준일 이전 7일 동안 로그가 수집되지 않은 이용자

### Retention
![gamebase_analytics_06_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_201901_2.png)

Retention은 특정 일에 가입한 이용자가 D+1일부터 D+90일까지 얼마나 잔존해 있는지를 보여주는 지표입니다.

현재는 당일 탈퇴자를 포함하여 Retention 값을 보여주고 있습니다. 추후에 당일 탈퇴자를 제외한 기준의 Retention 도 제공할 예정입니다.

* 당일 탈퇴자 제외: 당일에 가입하고, 당일에 탈퇴한 이용자를 신규 이용자에서 제외하고 Retention 값을 계산합니다. 
    * 신규 가입자(New User) = 당일 가입자 - 당일가입 후 탈퇴자
    * 예) 1월 1일 100명 신규 가입, 이 중 20명이 1월 1일 탈퇴자라면 실제 신규 가입자를 80명(100명-20명)으로 계산

## Sales Statistics
### Payment Amount
![gamebase_analytics_07_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_07_201901_2.png)

결제 금액에 대한 지표를 확인할 수 있습니다.

#### 1. 결제 금액 현황 표
선택된 기간 동안의 결제 금액을 보여줍니다.
총 결제금액과 주요 스토어의 국가 별 결제금액을 확인할 수 있습니다.

#### 2. 매출 추이
일자별로 신규 매출, 재구매 매출, PU(결제 이용자)의 추이를 그래프로 확인할 수 있습니다.
아래의 표에서는 국가별 매출을 확인할 수 있습니다.

### Paying User
![gamebase_analytics_08_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_08_201901_2.png)

유료 이용자(PU)에 관한 지표를 확인할 수 있습니다.
아래는 그래프와 표에 나온 용어 설명입니다.

* 결제금액: 이용자가 결제한 결제금액
* 신규 결제금액: 신규 결제 이용자(NPU)가 결제한 결제금액
* DAU: 일간 memberno 기준 로그인 1회 이상 액티브 이용자수 (Daily Active Users)
* PU: 유료상품을 결제한 이용자 (Paying User). PU=재구매PU + 신규PU
* 신규 PU(NPU): 유료 상품을 처음 결제한 이용자 (New Paying Users)
* 재구매 PU: 누적 PU - 신규 PU (재구매 PU 는 일간 data 로 전일자 기준으로 계산)
* PUR: 유료 이용자의 비율 (PU/DAU * 100)
* ARPU: 하루 동안 게임 이용자 수의 평균 결제 금액 (결제 금액/DAU)
* ARPPU: 결제 이용자 수의 평균 결제 금액 (결제 금액/PU)
* ARPNPU: 신규 유료 이용자의 평균 결제 금액 (결제 금액/NPU)

### Item Sales
![gamebase_analytics_09_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_09_201901_2.png)

등록된 아이템의 판매 지표를 확인할 수 있습니다.

* 아이템: Gamebase에 등록한 아이템 목록
* Best Item Top 10: 판매금액별, 판매건수별 판매량이 높은 아이템 top 10의 List 출력
* 스토어: 앱스토어, 구글플레이스토어 등과 같은 스토어
* 결제금액: 이용자가 결제한 아이템별 결제금액
* 결제 건수: 아이템별 결제 건수
* 결제 비중: 아이템별 결제 비중

### First Purchase
![gamebase_analytics_10_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_10_201901_2.png)

신규 유료 이용자의 첫 구매에 관한 정보를 확인할 수 있습니다.
신규 유료 이용자가 구입한 모든 아이템을 결제 금액 순으로 보여줍니다.

* 아이템: 신규 PU가 구매한 아이템 목록
* 스토어: 앱스토어, 구글플레이스토어 등과 같은 스토어
* 신규 PU(NPU): 유료 상품을 처음 결제한 이용자 (New Paying Users)
* 신규 결제금액: 신규 PU가 발생시킨 결제금액

## Concurrent Group User
### Concurrent Group User
![gamebase_analytics_11_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_11_201901_2.png)

Gamebase 서비스 이용자가 속한 모든 프로젝트의 동접 지표를 확인할 수 있습니다.

* 실시간 그룹 동접: Gamebase 서비스 이용자가 속한 프로젝트의 실시간 동접자(CCU)를 나타냅니다.
* 프로젝트 그룹 동접: 선택된 기간, 필터를 기준으로 앱 이용자 수가 나타납니다.

### Group Comparison
![gamebase_analytics_12_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_12_201901_2.png)

Gamebase 서비스 이용자가 속한 프로젝트들을 필터와 조합하여 그룹으로 비교할 수 있습니다.

* DAU: 일간 memberno 기준 로그인 1회 이상 액티브 이용자 수 (Daily Active Users)
* NRU: 당일 신규 가입자
* PU: 유료상품을 결제한 이용자 (Paying User). (=재구매 PU + 신규 PU)
* 결제금액: 이용자가 결제한 결제금액

※ 그래프에 표시되는 그룹명은 **{appId} _ {OS} _ {국가}** 형태입니다.

## Environment
### Install URL
![gamebase_analytics_13_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_13_201901_2.png)

설치 URL 호출에 대한 통계 지표를 확인할 수 있습니다.

* 최근 일주일간 다운로드 URL 호출 수: 앱 > 설치 URL의 설치 경로로 게임 설치 API를 호출한 고객수. 앱 설치 마케팅용으로 Short URL 호출한 고객수를 측정함으로써, 고객 반응도를 확인할 수 있습니다.
* 브라우저별 점유율(전체 누적): 브라우저별로 설치 URL 호출 수 비율을 확인할 수 있습니다. (전체 누적)
* 플랫폼별 점유율(전체 누적): 플랫폼별로 설치 URL 호출 수 비율을 확인할 수 있습니다. (전체 누적)