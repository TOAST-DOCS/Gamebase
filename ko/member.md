## Game > Gamebase > Operator Guide > Member

## Member

게임에 로그인한 회원 정보를 조회합니다.


### Search Member

ser ID를 입력하면 회원정보를 검색할 수 있습니다.<br/>
User ID는 최초 로그인시에 Gamebase에서 자동으로 발급하는 유저 식별자입니다. 전달시 혼란을 줄이고자 같은 발음의 문자를 배제하여 "ABCDFGHJKLMNPQRSTWXYZ1346789" 문자만을 사용하고 있습니다.<br/><br/>

검색된 유저의 상세정보를 상단에 노출하고 로그인, 매핑, 결제, 이용정지, 플레이타임 등의 이력은 하단에 탭형태로 노출합니다. <br/>




### Detail Information
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Member1_1.4.png)

** USER ** <br/>

- User ID : Gamebase User ID.
- 국가코드(USIM) : 사용자 단말기의 USIM 국가코드. <br/>
수집에 실패하는 경우에는 'ZZ'로 표기됩니다. 단말기의 국가코드를 확인하고 싶은 경우에는 하단의 'Login History'를 확인하세요.<br/>
- 마지막 로그인 시간 : 사용자가 가장 마지막에 로그인한 시간.
- 등록일 : 사용자가 최초 로그인한 시간.
- 계정 상태 
-- 정상 : 정상 사용자. '이용정지' 버튼을 클릭하여 수동으로 이용정지가 가능합니다.
-- 이용정지 : 어뷰징 등으로 이용정지(Ban)된 사용자. '이용정지해제' 버튼을 클릭하여 수동으로 이용정지해제가 가능합니다.
-- 탈퇴 : 탈퇴한 사용자.

** Identity Provider ** <br/>

Gamebase에서는 여러개의 외부 IdP 연동이 가능합니다. 즉, 사용자가 하나의 User ID에 facebook, google 두 개의 IdP를 등록하여 로그인이 가능합니다. SDK에서 'Login using a specific IDP' 나 'Add Mapping' API를 호출하는 경우에 IdP 등록이 됩니다.<br/>

- IdP : 외부 Identity Provider. (GUEST, facebook, payco, google 등)
- Idp ID : 외부 IdP에서 제공하는 ID. (facebook no, payco id 등)
- 등록일 : 사용자가 최초로 해당 IdP를 등록한 시간.

### Login History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_LoginHistory1_1.2.png)

조회한 사용자의 로그인 내역을 조회합니다. <br />
최초 조회시에는 최근 1일로 조회하며 조회를 원하는 날짜를 다시 입력하여 조회도 가능합니다. 단, 최근 3개월(90일)동안의 이력만 제공합니다.<br />
SDK에서 로그인 관련 API 호출시에 이력이 추가됩니다.<br/>

- Login Date : 
- Login Type : 
- OS / Ver : 
- Device model : 
- Device Key : 
- Device Country Code : 
- USIN Country Code : 
- Telecom : 
- Network : 
- Language Code : 
- Store Code : 
- Client Version :
- Gamebase SDK Version : 
- etc : 

### Mapping History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_MappingHistory1_1.2.png)

조회한 사용자의 매핑, 매핑해제된 이력을 조회합니다. 최근 3개월(90일)동안의 이력이 모두 노출됩니다.<br />

- User ID 기준 : 조회된 User ID를 기준으로 조회합니다. 
- IdP ID 기준 : 조회된 User ID에 현재 매핑된 IdP ID 기준으로 조회합니다. <br/>
조회된 User ID에 facebook, gooogle IdP가 매핑된 경우 두개의 IdP ID가 리스트에 노출됩니다.<br/>

- IdP ID :
- IdP : 
- 매핑일시 : 
- Type : 


### Purchase History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_PurchaseHistory1_1.0.png)
조회한 사용자의 상품 구입 내역을 조회합니다.<br />
원하는 날짜를 입력하여 조회가 가능하며 조회가 가능한 최대 날짜는 1개월(30일)입니다.<br />

- 결제번호 : 
- 스토어 : 
- 아이템 이름 :
- 가격 : 
- 통화
- Consume : 
- 결제상태 : 
- Store Reference Key : 
- 등록일시 : 
- 환불일시 : 

### Ban History
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_BanHistory1_1.0.png)

조회한 사용자의 이용 정지 내역을 조회할 수 있습니다.<br />
원하는 날짜를 입력하여 조회가 가능하며 조회가 가능한 최대 날짜는 1개월(30일)입니다.<br />

- 시작일 : 
- 종료일 :
- 템플릿 : 
- 사유 : 
- 등록자/등록일 : 
- 해제사유 : 
- 해제등록자/해제등록일 : 

### Playtime
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Member_Playtime1_1.2.png)
조회한 사용자의 게임을 플레이한 시간을 일자별로 조회합니다.<br />
원하는 날짜를 입력하여 조회가 가능하며 조회가 가능한 최대 날짜는 1개월(30일)입니다.<br />

- 날짜 : 
- 플레이시간 : 
