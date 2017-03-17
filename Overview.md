## Upcomming Products > Gamebase > Overview

게임에서 공통적으로 필요한 기능들을 통합SDK로 제공하여 손쉽고 효율적으로 게임 개발이 가능하도록 돕는 서비스입니다.<br>게임개발자는 게임컨텐츠만 만드시면 나머지는 Gamebase가 해결해드립니다.

## 주요 기능

Gamebase는 다음과 같은 기능을 제공합니다.

* 다양한 인증방식을 단일 인터페이스로 제공합니다.
	* Guest , 3rd Party(Google , Facebook, GameCenter 등) 인증 지원 
	
| 외부 인증 | 제공되는 플랫폼 |
|--------|--------|
| facebook       | iOS, Android        |
| iosgamecenter | iOS        |
| google      | Android        |
| payco       | iOS, Android        |

[표1] Gamebase에서 지원하는 외부 인증 리스트

* 로그아웃 및 회원탈퇴 기능을 제공합니다.
* 하나의 User가 여러 개의 외부 IDP를 동시에 사용할 수 있도록 mapping기능을 제공합니다.
* 게임운영을 위한 게임 앱 상태관리, 점검, 긴급공지 등의 기능을 웹콘솔로 제공합니다.
* 글로벌 게임 운영을 보다 쉽게 할 수 있도록 돕습니다.
	* User에게 노출되는 메시지는 모두 다국어 처리가 가능
* 웹콘솔에서 운영지표를 실시간으로 손쉽게 확인할 수 있습니다.
* 게임에서 필요한 TOAST Cloud상품을 보다 쉽게 연동할 수 있도록 돕습니다.
	* 연동이 완료된 상품 :  [Notification > PUSH](http://cloud.toast.com/service/notification), [Common > IAP](http://cloud.toast.com/service/iap), [Game > Leaderboard](http://cloud.toast.com/service/leaderboard)



## 서비스 용어

| 용어 | 설명 |
|--------|--------|
| UserID      | Gamebase 내부의 유저식별자       |
| DeviceKey      | 단말 식별자.(iOS:IDFA/Android:Android ID)       |
| UUID      | Guest 생성시 사용되는 단말 식별자로 앱삭제전까지 유지      |
| IDP       | Identify Provider로 인증제공자. 외부 페이스북, 구글, 애플게임센터, payco 등       |
| IDP Token      | IDP SDK로부터 인증 후 받은 access token       |
| IDP Login      | 외부 IDP 로그인       |

[표2] Gamebase 서비스 용어

## 서비스 구조

![논리 구성도](http://static.toastoven.net/prod_gamebase/Overview/img_logical_1.0.png)
<center>[그림1] Gamebase 서비스 구조</center>
<br>

| 컴포넌트명 | 설명 |
| --- | --- |
| Gamebase SDK | - 클라이언트 개발을 위한 SDK |
| gateway | - 내부/외부 모듈간의 mashup API 제공 <br>- 클라이언트 및 서버로부터 요청을 받아 backend 단 서비스로 전달 |
| Gamebase server | - Gamebase 내부 로직을 처리하는 서버들 <br>- 클라이언트 기동 초기 데이터 제공 <br>- 사용자 구분키 발급과 관리, 매핑관리 <br>- 게임별 동접 지표 수집 및 관리 |
| console | - Web Console |

[표3] Gamebase Component




























