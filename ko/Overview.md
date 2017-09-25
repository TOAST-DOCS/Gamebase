                                                                              ## Game > Gamebase > Overview

게임에서 공통적으로 필요한 기능들을 통합SDK로 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.<br>
게임개발자는 게임컨텐츠만 만드시면 나머지는 Gamebase가 해결해드립니다.

## Key Features

Gamebase는 다음과 같은 기능을 제공합니다.

### Authentication

Gamebase는 여러 IDP(Identity provider)의 계정을 이용한 ID, PASSWORD 기반의 OAuth로그인과 단말기의 UUID를 이용한 게스트 로그인을 지원합니다. Gamebase의 인증은 자체적인 회원체계를 구축하지 않고 외부 IDP에서 제공하는 회원 정보를 이용하여 인증 서비스를 제공하는 서비스입니다. 자체적인 회원체계가 없다라는 것은 사용자의 ID, PASSWORD를 Gamebase 내부에서 저장하지 않는 것을 의미합니다.

* 다양한 인증방식을 단일 인터페이스로 제공합니다. <br/>
단일 인터페이스로 API를 제공하여 보다 쉽고 빠르게 외부 IDP 추가개발이 가능하기 때문에 개발비용이 절갑됩니다. 개발자는 복잡한 인증 절차나 법적 문제, 정책적 문제 등을 고려하지 않고 쉽게 인증 기능을 구현할 수 있습니다.

* 다양한 외부 IDP 인증을 제공합니다. <br/> 
제공하는 외부 인증은 지속적으로 업데이트 예정되어 있으며, 게임에서 추가를 원하는 인증이 있는 경우에는 고객센터로 연락주시기 바랍니다.<br/>
| 외부 인증 | 제공되는 플랫폼 |
|--------|--------|
| facebook       | iOS, Android        |
| iosgamecenter | iOS        |
| google      | Android        |
| payco       | iOS, Android        |
[표1] Gamebase에서 지원하는 외부 인증 리스트<br/>

* 게스트로그인을 제공합니다. 
게스트 로그인을 이용하면 사용자는 아무런 입력 없이 바로 게임에 로그인하여 간편하게 게임을 시작 할 수 있습니다. 게스트로그인만으로도 Gamebase User ID가 발급되므로 게임은 OAuth 로그인 사용자와 게스트 로그인 사용자의 구분 없이 동일하게 사용자의 게임데이터를 관리 할 수 있습니다.<br/>

* 독립적인 회원식별자를 제공합니다. <br/>
최초로그인을 하면 Gamebase User ID가 자동으로 생성되며, 게임에서는 사용자를 구별하는 식별자로 사용하실 수 있습니다. User ID는 인증방식과 관계없이 모든 사용자에게 발급되며 IDP에 종속적이지 않으므로 어떤한 IDP를 통해 로그인하더라도 게임내에서 동일한 방식으로 사용자 처리가 가능합니다.<br/>

* 로그아웃 및 회원탈퇴 기능을 제공합니다.<br/>

* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공합니다.<br/>
facebook 인증을 사용하여 게임을 이용하고 있는 유저가 google인증으로도 동일한 User ID를 사용할 수 있도록 mapping기능을 제공합니다. 하나의 User ID에 facebook과 google 인증을 mapping하면 유저는 모바일폰에서는 facebook, pad에서는 google로 인증하여 게임플레이가 가능합니다.

#### Reference

* [LINK [Android Developer Guide-Auth] ](./aos-authentication)
* [LINK [iOS Developer Guide-Auth] ](./ios-authentication)
* [LINK [Unity Developer Guide-Auth] ](./unity-authentication)

### Launching

서비스되고 있는 게임 앱은 기동시 여러 정보가 필요합니다. Gamebase는 게임 앱 실행 초기에 게임 앱에게 운영에 필요한 데이터를 제공하고 이를 Launching이라고 부릅니다. <br/>
Launching 정보는 Gamebase Console에서 실시간으로 정보 설정이 가능하며 SDK 초기화나 Launching Status변경시에 게임에서 확인할 수 있습니다.<br/>

Gamebase에서 제공되는 Launching 정보는 다음과 같습니다.

* 앱 상태정보
	* 게임 클라이언트 업데이트 필요여부, 다운로드 URL
	* 점검 정보
* 긴급 공지 정보
* 인증 정보
* 게임내 in app URL 목록

#### Reference

* [LINK [Android Developer Guide-Launching] ](./aos-initialization/#launching-status)
* [LINK [iOS Developer Guide-Launching] ](./ios-initialization/#launching-status)
* [LINK [Unity Developer Guide-Launching info] ](./unity-initialization/#launching-informations)
* [LINK [Operator Guide-App Info(App, Client, Installed URL)] ](./app) : 앱, 클라이언트 상태 및 설치 URL설정
* [LINK [Operator Guide-Operator(Maintenance,Notice)] ](./operation) : 점검, 공지 등록


### For Global 

Gamebase는 기본적으로 게임의 글로벌 오픈을 지원하고 있으며 글로벌 환경에서의 게임 운영을 지원하기 위하여 다음과 같은 기능들을 제공합니다.

* User에게 노출되는 메시지는 모두 다국어 처리가 가능합니다.
	* User에서 노출되는 메시지를 Console에서 입력할 때 다국어로 입력받아 User의 단말기 언어 설정에 맞게 언어를 표시합니다. Console에서 한국어, 영어, 일본어를 입력하면 한국어 단말기를 사용하는 User에게는 한국어 메시지가 표시됩니다.
* 국가 필터링 기능을 제공합니다.
	* 운영 중에 특정 국가의 User에게만 긴급공지 메시지나 푸시메시지를 보내고 싶은 경우 국가를 지정하여 메시지 노출이 가능합니다. 
* 운영자의 Local Timezone을 선택하여 손쉽게 시간입력이 가능합니다.
	* 베트남에서 게임을 운영하는 경우 베트남 Timezone을 선택하여 베트남 시간 기준으로 입력이 가능하므로 한국 시간으로 변경하는 수고를 줄일 수 있습니다.

### Standard Index(BI)

* Gamebase Console에서 기본적인 운영지표를 실시간으로 제공합니다.
	* CCU : 실시간 동시접속자수
	* MCU : 하루동안의 최대 동접수 (실시간&일자별 조회제공)
	* DAU : 하루동안 게임을 사용한 순수 이용자수 (실시간&일자별 조회제공)
	* NRU : 하루동안의 신규사용자 수 (실시간&일자별 조회제공)
	* 점유율 차트 : 게임 User의 OS별, 국가별, 게임 클라이언트버전별 점유율을 Pie chart로 제공
	* 동접 변화 그래프 : 하루동안의 동접변화량을 그래프로 제공. 점검과 푸시메시지 전송에 따른 그래프 변경도 한눈에 확인이 가능.
* 권한 있는 여러 프로젝트의 지표를 한 눈에 확인 할 수 있도록 그룹지표를 제공합니다.
* 설치 URL 통계를 제공하여 일자별, 브라우저(ie, chrome등)별, 플랫폼(windows, android등)별 설치 URL 호출수 및 점유율을 제공합니다.
* 게임의 매출통계를 확인할 수 있는 판매현황 화면을 제공합니다.
	* 월별, 일자별, 스토어별 매출합계를 제공
	* 원하는 통계로 변경하여 확인 가능

#### Reference

* [LINK [Operator Guide-Operating indicator] ](./operating-indicator) 

### Using the other TOAST Cloud Service

* 게임에서 필요한 TOAST Cloud상품을 보다 쉽게 연동할 수 있도록 돕습니다.
	* 연동이 완료된 상품 :  [Notification > PUSH](http://cloud.toast.com/service/notification), [Common > IAP](http://cloud.toast.com/service/iap), [Game > Leaderboard](http://cloud.toast.com/service/leaderboard), [Security > Leaderboard](https://cloud.toast.com/service/security)



## Terms

| 용어 | 설명 |
|--------|--------|
| UserID      | Gamebase 내부의 유저식별자       |
| DeviceKey      | 단말 식별자.(iOS:IDFV/Android:Android ID)       |
| UUID      | Guest 생성시 사용되는 단말 식별자로 앱삭제전까지 유지      |
| IDP       | Identify Provider로 인증제공자. 외부 페이스북, 구글, 애플게임센터, payco 등       |
| IDP Token      | IDP SDK로부터 인증 후 받은 access token       |
| IDP Login      | 외부 IDP 로그인 (facebook, google 등)       |

[표2] Gamebase 서비스 용어

## Service Architecture

![논리 구성도](http://static.toastoven.net/prod_gamebase/Overview/img_logical_1.0.png)
<center>[그림1] Gamebase 서비스 구조</center>
<br>

| 컴포넌트명 | 설명 |
| --- | --- |
| Gamebase SDK | - 클라이언트 개발을 위한 SDK |
| Gateway | - 내부/외부 모듈간의 mashup API 제공 <br>- 클라이언트 및 서버로부터 요청을 받아 backend 단 서비스로 전달 |
| Gamebase server | - Gamebase 내부 로직을 처리하는 서버들 <br>- 클라이언트 기동 초기 데이터 제공 <br>- 사용자 구분키 발급과 관리, 매핑관리 <br>- 게임별 동접 지표 수집 및 관리 |
| console | - Web Console |

[표3] Gamebase Component




























