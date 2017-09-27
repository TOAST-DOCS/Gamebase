## Game > Gamebase > Operator Guide > Monitoring

앱을 이용하는 사용자의 현황을 지표 및 그래프로 확인할 수 있습니다.<br />
모니터링 / 그룹동접 / 설치 URL 통계 / 판매 현황 메뉴로 구성되어 있습니다.<br />

<br/>
## Monitoring
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Monitoring1_1.1.png)
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Monitoring2_1.1.png)
현재 앱을 이용하는 유저의 전체 통계 및 현재 예약된 푸시 현황, 예약된 점검 내역을 제공합니다. <br/>
5분이 지나면 자동으로 화면이 리프레시 되고, 실시간으로 변경된 지표를 확인할 수 있습니다.<br/>

* 기본 지표
	* CCU : 실시간 동시접속자수
	* MCU : 하루동안의 최대 동접수 (실시간&일자별 조회제공)
	* DAU : 하루동안 게임을 사용한 순수 이용자수 (실시간&일자별 조회제공)
	* NRU : 하루동안의 신규사용자 수 (실시간&일자별 조회제공)
* 점유율 차트 : 게임 유저의 점유율 Pie chart
	* OS별 : Android, iOS, WebGL 등
	* 국가별 : SDK에서 수집된 USIM Country 기준
	* 버전별 : Console에 등록된 Client 버전별 점유율
* 동접 변화 그래프 : 금일 00:00 부터 현재 시간까지의 동접변화 그래프
	* 점검 및 푸시에 따른 동접변화를 확인하기 쉽도록 점검, 푸시 내역을 그래프에 별도 표시하고 있습니다.
<br/>
## Concurrent Group User
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_ConcurrentUser1_1.1.png)
자신이 속한 프로젝트의 그룹동접 통계를 제공합니다. 권한 있는 여러 프로젝트의 OS별 실시간 동접자수를 한 눈에 확인할 수 있습니다.

<br/>
## Installed URL Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_InstallUrl1_1.0.png)
설치 URL 호출에 대한 통계데이터를 제공합니다. 

* 일자별 설치 URL 호출 변화 그래프
* 브라우저별 점유율 : ie, chrome 등
* 플랫폼별 점유율 : android, ios 등
<br/>
## Statistics
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Statistics1_1.0.png)
게임의 매출통계를 실시간으로 확인할 수 있는 판매현황 화면을 제공합니다.

* 월별, 일자별, 스토어별 매출합계를 제공
* 원하는 통화(KRW,USD등)로 변경하여 확인 가능