## Game > Gamebase > 콘솔 사용 가이드 > 운영 지표

앱을 이용하는 사용자의 현황을 지표 및 그래프로 확인할 수 있습니다.
모니터링, 그룹 동시 접속, 설치 URL 통계, 판매 현황 메뉴로 구성되어 있습니다.


## Monitoring
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Monitoring1_1.1.png)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Monitoring2_1.1.png)
현재 앱을 이용하는 사용자의 전체 통계 및 현재 예약된 푸시 현황, 예약된 점검 내역을 확인할 수 있습니다. 
5분이 지나면 자동으로 화면이 '새로 고침'이 되고, 실시간으로 변경된 지표를 확인할 수 있습니다.

* 기본 지표
	* CCU(concurrent connected users): 실시간 동시 접속자 수
	* MCU(maximum concurrent users): 하루 동안의 최대 동시 접속자 수(실시간, 일자별 조회 가능)
	* DAU(daily active users): 하루 동안 게임을 사용한 순 이용자 수(실시간, 일자별 조회 가능)
	* NRU(new registered users): 하루 동안의 신규 사용자 수(실시간, 일자별 조회 가능)
* 점유율 차트: 게임 사용자의 점유율 파이 차트
	* 운영체제별: Android, iOS, WebGL 등
	* 국가별: SDK에서 수집된 USIM 국가 기준
	* 버전별: Console에 등록된 클라이언트 버전별 점유율
* 동시 접속 변화 그래프: 금일 00:00부터 현재 시간까지의 동시 접속 변화 그래프
	* 점검 및 푸시에 따른 동시 접속 변화를 확인하기 쉽도록 점검, 푸시 내역을 그래프에 별도로 표시하고 있습니다.
    


## User Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_UserStatistics1_1.0.png)
DAU, MCU, NRU, CCU AVG 현황을 그래프로 확인할 수 있습니다.
현재 앱을 사용하는 게임유저의 추이 변화를 한눈에 확인할 수 있습니다. 
오른쪽 위의 기간 선택 바를 이용해 원하는 날짜를 선택하여 확인할 수도 있습니다.
각 항목별 설명은 아래와 같습니다.

* 항목 설명
	* DAU(daily active users): 하루 동안 게임을 사용한 순 이용자 수(실시간, 일자별 조회 가능)
	* MCU(maximum concurrent users): 하루 동안의 최대 동시 접속자 수(실시간, 일자별 조회 가능)
	* NRU(new registered users): 하루 동안의 신규 사용자 수(실시간, 일자별 조회 가능)
	* CCU AVG(concurrent connected users average): 실시간 동시 접속자 수의 평균값

## Concurrent Group User
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_ConcurrentUser1_1.1.png)
자신이 속한 프로젝트의 그룹 동시 접속 통계를 확인할 수 있습니다. 권한이 있는 여러 프로젝트의 운영체제별 실시간 동시 접속자 수를 한눈에 볼 수 있습니다.


## Installed URL Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_InstallUrl1_1.0.png)
설치 URL 호출에 대한 통계 데이터를 확인할 수 있습니다.

* 일자별 설치 URL 호출수 변화 그래프
* 브라우저별 점유율: Internet Explorer, Chrome 등
* 플랫폼별 점유율: Android, iOS 등
  

## Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Statistics1_1.2.png)
판매 현황 화면을 통하여 앱의 매출현황을 손쉽게 확인할 수 있습니다.
오른쪽 위 **통화**에서 원하는 통화를 선택해 통화별 매출액을 확인할 수도 있습니다.

### (1) 판매 현황 일별 통계 그래프
일자별 판매 현황과 추이를 꺾은선 그래프로 손쉽게 파악할 수 있습니다.

### (2) 월별 매출 현황
한 달 기준 또는 현재 진행중인 이달의 매출 합계를 스토어별 및 총합 데이터로 통계를 내어 보여줍니다.

### (3) 일자별 매출 현황
앱에서 등록한 스토어별 매출현황을 일자별로 조회할 수 있습니다.
이달의 오늘의 데이터까지 모두 조회할 수 있습니다.
월별로 보는 것보다 더 자세한 매출 현황을 확인할 수 있습니다. 이벤트 진행 기간 또는 이슈가 발생했을 때의 매출 현황을 비교하고 분석할 수 있습니다.