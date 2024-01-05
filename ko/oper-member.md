## Game > Gamebase > 콘솔 사용 가이드 > 회원

게임에 로그인한 회원 정보를 조회합니다.

## Search Member

User ID/IdP ID를 입력하면 회원정보를 검색할 수 있습니다.
사용자 아이디(User ID)는 최초로 로그인할 때 Gamebase에서 자동으로 발급하는 사용자 식별자입니다. 전달 시 혼란을 줄이고자 같은 발음의 문자를 배제하여 "ABCDFGHJKLMNPQRSTWXYZ1346789" 문자만을 사용하고 있습니다.
IdP ID는 IdP에서 제공하는 아이디 정보로써 로그인 시 입력하는 정보가 아닌 IdP내의 고유 식별자를 의미합니다. 따라서 IdP ID 항목으로 검색하고자 할 경우 검색정보 입력에 주의가 필요합니다.

검색된 사용자의 상세 정보를 위쪽에 표시하고 로그인, 매핑, 결제, 이용 정지, 플레이 시간 등의 이력은 아래쪽에 탭 형태로 표시됩니다.

### Detail Information
![member_01](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_01_kr_240103.png)

**User **

- **유저 ID**: Gamebase 사용자 아이디
- **국가코드(USIM)**: 사용자 단말기의 USIM 국가 코드로 수집에 실패하면 'ZZ'로 표기됩니다. 단말기에 설정된 국가 코드를 확인하고 싶다면 하단의 **로그인 이력**에서 확인하세요.
- **마지막 로그인 시간**: 사용자가 가장 마지막에 로그인한 시간
- **가입일**: 사용자가 최초로 로그인한 시간
- **계정 상태**
  - **정상**: 정상 사용자.
  - **이용정지**: 어뷰징 등으로 이용 정지(ban)된 사용자. 우측 상단의 계정 상태 변경 메뉴를 통해 이용정지를 해제할 수 있습니다.
  - **탈퇴**: 탈퇴한 사용자.
- **푸시 부가정보 조회**: 게임 유저의 푸시 토큰 및 태그 정보 조회.
![member_02](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_02_kr_240103.png)

**Identity Provider **

Gamebase에서는 여러 개의 외부 IdP를 연동할 수 있습니다. 즉, 사용자가 하나의 사용자 아이디에 Facebook, Google 두 개의 IdP를 등록하여 로그인할 수 있습니다. SDK에서 **Login using a specific IdP**나 '**Add Mapping** API를 호출하는 경우에 IdP가 등록됩니다.

- **IdP**: 외부 IdP(게스트, Facebook, PAYCO, Google 등)
- **Idp ID**: 외부 IdP에서 제공하는 아이디(Facebook no, PAYCO 아이디 등)
- **등록일**: 사용자가 최초로 해당 IdP를 등록한 시간

#### 계정 상태 변경
![member_03](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_03_kr_240103.png)

조회한 게임 유저의 계정 상태를 변경할 수 있는 기능입니다.
상태별 변경할 수 있는 경우는 아래와 같습니다.
- **정상**: 이용정지, 탈퇴상태로 변경이 가능합니다. 탈퇴시에는 해당 계정정보를 되돌릴 수 없으므로 처리 전 확인 및 주의가 필요합니다.
- **이용 정지**: 이용정지 해제를 진행할 수 있습니다.
- **탈퇴**: 해당 버튼이 표시되지 않습니다.

#### 매핑 추가

조회한 게임 유저의 IdP 정보를 추가할 수 있는 기능입니다.
연결할 IdP를 가지고 있는 게임 유저의 정보가 **정상**일 때만 매핑 추가 작업을 할 수 있습니다.
* 제공 유저의 IdP 설정란에서 연결할 IdP 정보를 1번 버튼을 클릭해 오른쪽 화면에 추가한 후, 아래 **매핑 추가** 버튼을 클릭합니다.
* 잘못 추가했다면 **매핑 추가** 버튼을 클릭하기 전에는 2번 버튼을 클릭해 언제든지 다른 IdP로 바꿀 수 있습니다.
* 제공받는 게임 유저가 게스트 정보만 가지고 있을 때는 새로운 IdP 정보가 추가되면서 기존의 게스트 정보는 유실되므로 주의해야 합니다.
* 제공 유저의 IdP 정보가 한 개일 때는 작업을 진행하면 제공 유저 정보는 **유실** 상태로 변경되어 더 이상 사용할 수 없으므로 미리 확인해야 합니다.
##### 제공 예시
![member_04](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_04_kr_240103.png)
![member_05](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_05_kr_240103.png)

#### 매핑 해제
다중 매핑이 된 계정은 요청에 따라 IdP 정보 연동을 해제할 수 있습니다.
각각의 계정은 최소 1개의 연결 정보가 있어야 하므로 2개 이상의 연결 정보가 있을 때만 버튼이 활성화됩니다.
* 버튼을 클릭하면 아래와 같이 연결된 IdP 정보와 함께 **매핑 해제** 버튼이 나타납니다.

![member_06](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_06_kr_240103.png)

* **해제** 버튼을 클릭하면 아래와 같이 최종 확인 창과 함께 IdP 정보를 확인합니다. **확인** 버튼을 클릭하면 매핑이 해제됩니다.

![member_07](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_07_kr_240103.png)

### Login History
![member_08](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_08_kr_240103.png)

조회한 사용자의 로그인 내역을 조회합니다.
최초 조회 시에는 최근 1일로 조회하며 조회를 원하는 날짜를 다시 입력하여 조회할 수도 있습니다. 단, 최근 3개월(90일) 동안의 이력만 제공합니다.
SDK에서 로그인 관련 API를 호출할 때 이력이 추가됩니다.

- **Login Date**: 사용자가 앱에 로그인 한 시간
- **Login Type**: 사용자 로그인 시 사용한 인증 유형(IdP Login/Guest 등). 괄호안의 정보는 실제 사용 된 IdProvider 정보.
- **OS / Ver**: 사용자 로그인 시 사용한 OS(IOS/Android/WebGL 등) 및 해당 OS의 버전 정보
- **Device model**: 사용자가 앱 로그인 시 사용한 단말기 모델명
- **Device Key**: 사용자 로그인 시 사용한 단말기가 가지고 있는 고유 식별값(Android:Android id, iOS:IDFV)
- **Device Country Code**: 사용자 로그인 시 단말기에 설정된 국가 코드
- **USIM Country Code**: 사용자 로그인 시 USIM카드에 설정된 국가 코드
- **Telecom**: 사용자 로그인 시 이용한 통신사 정보
- **Network**: 사용자 로그인 시 사용한 네트워크 유형(Wi-Fi/3G/LTE 등)
- **Language Code**: 사용자 로그인 시 단말기에 설정 된 언어 코드 정보
- **Store Code**: 사용자가 앱을 다운로드한 스토어 정보
- **Client Version**: 앱 다운로드 당시의 클라이언트 버전 정보
- **Gamebase SDK Version**: 앱에 사용된 Gamebase SDK의 버전 정보
- **etc**: 기타 로그인 시 사용된 위 항목 외 정보

### Mapping History
![member_09](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_09_kr_240103.png)

조회한 사용자의 매핑, 매핑 해제된 이력을 조회합니다. 조회 가능한 최대 날짜는 3개월(90일)입니다.

* **IdP ID**: IdP 로그인 시 사용되는 ID 정보
* **IdP**: 매핑된 IdP 정보
* **일시**: IdP 아이디와 Gamebase 아이디 매핑 관련 작업이 된 시간
* **Type**: 매핑 작업 상세 내역
  - AAM: 매핑 추가
  - ARM: 매핑 제거
  - AFR: 매핑 강제 제거
  - GMG: 게스트 계정 생성
  - OMG: IdP 계정 생성

매핑된 IdP 이력을 클릭할 경우 해당 IdP를 기준으로 Gamebase ID에 매핑된 이력을 보여줍니다.

![member_10](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_10_kr_240103.png)

### Purchase History
![member_11](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_11_kr_240103.png)
조회한 사용자의 상품 구입 내역을 조회합니다.
원하는 날짜를 입력하여 조회할 수 있으며 조회 가능한 최대 날짜는 1개월(30일)입니다.

- **Transaction ID**: Gamebase 내에서 결제를 구별할 수 있는 고유 번호
- **스토어**: 결제한 스토어 정보
- **아이템 이름**: 사용자가 앱에서 구입한 실제 아이템 이름
- **가격**: 사용자가 구입한 아이템의 가격
- **통화**: 사용자가 구입 시 사용한 통화 종류
- **소비 상태**: 결제한 아이템의 지급 여부
- **결제 상태**: 결제의 현재 진행 상태
- **Store Reference Key**: 스토어에서 발급하는 결제 고유 번호
- **결제예약일시**: 사용자가 구입을 시도한 시간
- **결제일시**: 사용자가 구입을 완료한 시간
- **환불일시**: 사용자가 아이템을 환불한 시간

### Ban History
![member_12](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_12_kr_240103.png)

조회한 사용자의 이용 정지 내역을 조회할 수 있습니다.
원하는 날짜를 입력하여 조회할 수 있으며 조회할 수 있는 최대 날짜는 1개월(30일)입니다.

- **시작일**: 사용자의 이용 정지 적용 시작 시간
- **종료일**: 사용자의 이용 정지가 끝나는 시간
- **템플릿**: 사용자 이용 정지 등록 시 사용한 템플릿 이름
- **사유**: 운영자가 사용자를 이용 정지한 실제 사유 정보
- **등록자/등록일**: 이용 정지를 등록한 운영자/시스템 정보 및 일시
- **해제 사유**: 운영자가 이용 정지 해제를 진행할 때 입력한 실제 해제 사유
- **해제 등록자/해제 등록일**: 이용 정지를 해제한 운영자/시스템 정보 및 일시

### Playtime
![member_13](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_13_kr_240103.png)
조회한 사용자가 게임을 플레이한 시간을 일자별로 조회합니다.
원하는 날짜를 입력하여 조회할 수 있으며 조회가 가능한 최대 날짜는 1개월(30일)입니다.

### Coupon using history
![member_14](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_14_kr_240103.png)

### Inquiry history
![member_15](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_15_kr_240103.png)

### Withdraw History
![member_16](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_16_kr_240103.png)
조회한 사용자가 탈퇴한 사용자라면 탈퇴 이력을 보여줍니다.
이 메뉴는 탈퇴이거나 탈퇴 유예 상태의 유저를 조회할 경우에만 나타나며 유저의 탈퇴내역을 상세히 조회할 수 있습니다.

## Transfer account
**단말기 이전** 기능을 사용할 경우에만 사용하실 수 있습니다. [단말기 이전 기능 활성화](./oper-app/#transfer-account)
게임 유저의 단말기 이전 키의 발급 및 검증 이력을 확인할 수 있습니다. 차단된 키를 차단 해제하거나 만료된 키를 재발급할 수 있습니다.

![member_17](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_17_kr_240103.png)
**단말기 이전 발급 키**

- **ID**: 게임 유저에게 발급된 단말기 이전 ID
- **발급 일자**: 단말기 이전 ID가 발급된 일자
- **만료 일자**: 발급된 단말기 이전 ID의 만료 일자
- **상태**: 발급된 단말기 이전 ID의 현재 상태
  - <font color="white" style="background-color:#88C637">정상</font>: 발급된 키가 정상 상태입니다. 해당 키를 이용하여 단말기 이전이 가능합니다.
  - <font color="white" style="background-color:#FB8F37">차단</font>: 발급된 키가 차단된 상태입니다. 발급된 키를 이용하여 단말기 이전이 불가능합니다.
  - <font color="white" style="background-color:#A1A1A1">만료</font>: 발급된 키의 사용 기간이 만료된 상태입니다. 발급된 키를 이용하여 단말기 이전이 불가능합니다.

**단말기 이전 이력**
해당 게임 유저에게 발급된 키의 이력을 조회할 수 있습니다.
기본적으로 가장 최근에 발급된 키가 선택되어 있으며 다른 키를 선택할 경우 선택한 키의 이력을 확인할 수 있습니다.

### Reissuance Transfer account

**재발급** 버튼을 클릭하면 새로운 단말기 이전 키를 다시 발급할 수 있습니다. 재발급하면 이전에 발급된 키는 더는 사용할 수 없습니다.

![member_18](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Member/ko/member_18_kr_240103.png)

- **ID,비밀번호 재발급**: ID, 비밀번호를 모두 새로 발급합니다.
- **비밀번호 재발급**: ID는 이전에 발급된 ID를 그대로 사용하고 비밀번호만 재발급합니다.

#### 재발급 시 주의 사항
- 비밀번호는 재발급 시 한 번만 표시되므로 재발급 진행 이후 꼭 해당 정보를 별도로 보관하셔야 합니다.
- 보관하지 못하셨을 경우 따로 비밀번호를 찾을 수 있는 방법이 없으므로 재발급을 다시 진행해 주셔야 합니다.
- 만료된 키는 만료 일자가 갱신되나 계정 상태가 정상/차단일 경우에는 만료 일자가 갱신되지 않습니다.
