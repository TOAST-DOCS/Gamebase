## Game > Gamebase > Operator Guide > Ban
앱을 부당하게 사용하거나 어뷰징을 하는 유저에 대하여 앱의 진입을 제한할 수 있는 이용 정지 기능을 제공합니다.<br/>
이용정지된 유저가 다시 로그인하거나 세션복구를 하는 경우에 이용정지 팝업이 노출되어 게임이용이 제한됩니다.<br/>

이용정지 등록은 Gamebase Console을 통해 수동으로 등록하거나 AppGuard를 사용하는 경우 패턴등록을 이용하여 자동으로 등록할 수 있습니다.
LINK [[AppGuard 연동하기](./ban/#appguard)] <br/>


## Ban

이용정지 이력을 조회하거나 이용 정지 등록, 이미 이용정지 중인 사용자의 이용정지해제가 가능합니다.<br/>

### Search Banned User
검색조건에 맞는 이용정지/이용정지해제 사용자 목록을 조회합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban1_1.0.png)

** 검색조건 **

- 등록일 : (필수) 이용정지 등록일을 구간을 선택. (From~TO)
- 유저 ID : 이용정지/이용정지해제 된 Gamebase User ID 입력
- 상태 : (필수) 원하는 이용정지상태 선택(이용정지/이용정지해제). 두가지 모두 선택은 불가능하며 한가지만 필수로 선택해야 합니다.
- 템플릿 : 이용정지 등록에 사용한 특정 템플릿을 선택하여 조회.
- 등록시스템 : 이용정지 등록한 시스템을 선택하여 조회. 다중선택 가능.
-- 콘솔 : Gamebase Console을 통하여 등록<br />
-- 앱가드 : AppGuard 연동으로 자동등록<br />
-- 외부서버 : 앱을 운영하는 서버 또는 기타 다른 외부 서버에 의해서 등록<br />
-- 기타 : 앞의 경우를 제외한 나머지 이용 정지 등록(API 직접호출 등) (<br />

> [Note] <br/>
> 이용정지 등록을 보다 쉽게 하기 위해서 사용자에게 노출할 메시지를 다국어로 입력하여 재사용이 가능하도록 템플릿을 제공합니다.<br/>
> 템플릿이 등록된 경우에만 이용정지 등록이 가능합니다.<br/>
> LINK [[템플릿 등록하기](./ban/#template)] <br/>

** 검색결과 **

- 유저ID : 이용정지 USer ID
- 기간 : 이용정지 기간. 영구정지인 경우 '영구정지'로 노출.
- 템플릿 : 이용정지 등록에 사용한 템플릿.
- 사유 : 이용정지 등록시에 운영자가 입력한 사유. 해당 사유는 사용자에게 노출되지 않고 운영 이력으로만 확인 가능합니다.
- 등록자/등록일 : 이용정지 등록한 운영자 계정 / 이용정지 등록일
- 해제사유 : 이용정지 해제시에 운영자가 등록한 사유. 해당 사유는 사용자에게 노출되지 않고 운영 이력으로만 확인 가능합니다.
- 해제등록자/해제등록일 : 이용정지 해제한 운영자 계정 / 이용정지 해제일
- 해제 : 이용정지 상태인 사용자는 검색목록에서 이용정지해제가 가능하도록 '이용해제'버튼이 노출된다. 버튼을 클릭하면 이용정지해제사유를 입력하는 팝업이 노출되고, 이용정지 해제 사유 입력하고 저장버튼을 클릭하면 이용정지해제가 가능하다.
- 상태
-- <font color="white" style="background-color:#FB8F37">이용정지</font> : 유저가 현재 앱에 접속할 수 없는 상태<br />
-- <font color="white" style="background-color:#A1A1A1">이용정지(기간 만료)</font> : 유저의 이용 정지 기간이 끝났으나 현재 로그인을 하지 않은 상태. 유저 재접속 시 해제(기간 만료)로 상태가 변경됨.<br />
-- <font color="white" style="background-color:#88C637">해제</font> : 운영자에 의해서 유저의 이용 정지가 해제된 상태.<br />
-- <font color="white" style="background-color:#2AB1A6">해제(기간 만료)</font> : 이용 정지 기간이 끝난 이후 자연스럽게 이용 정지가 해제된 상태.<br />

 
> <font color="blue">[TIPs]</font><br/>
> 검색결과 파일로 저장하기
> '파일 다운로드'버튼을 클릭하면 검색결과를 '*.csv' 파일로 저장할 수 있습니다. <br/>



### Register Ban

이용정지 조회 화면에서 '등록'버튼을 클릭하면 이용정지 등록이 가능합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban2_1.0.png)
#### (1) 유저 ID
이용 정지를 등록할 Gamebase User ID를 입력합니다. 한번에 다수의 유저 등록이 가능하며, 등록방법은 아래 두가지 방법을 제공합니다.

- 사용자 입력 : 등록할 유저ID를 입력창에 직접 입력 후 'Enter'키를 누르거나 '추가'버튼을 클릭합니다. User ID 유효성을 검사하므로 유효하지 않은 User ID는 입력이 불가능합니다.
- 일괄 등록 : csv파일만 업로드 가능하며 예시파일은 console화면에서 다운로드 받을 수 있습니다. 일괄 등록은 1회 최대 10,000명까지 등록 가능합니다. <br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban4_1.0.png)

> <font color="blue">[TIPs]</font><br/>
> 일괄등록을 이용하여 이용정지 등록 진행 중 실패하는 경우 팝업이 노출되고, 해당 팝업에서 'Download'버튼을 클릭하면 실패 유저 리스트를 Local PC에 파일로 다운로드 받을 수 있습니다.<br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban5_1.0.png)

#### (2) 기간
유저의 이용 정지 기간을 설정합니다. 이용정지는 등록되는 시점부터 이용정지가 됩니다.<br />

- 영구 이용 정지 : 영구 이용정지를 위해 선택합니다.
- 기간 지정 : 얼마동안 이용정지를 진행할지 일(day)과 시간(hour)을 입력합니다. '예상 만료 시간'정보로 유저의 이용 정지 기간을 미리 확인하실 수 있습니다.<br />

#### (3) 사유
유저의 이용 정지를 등록하게 된 사유를 입력합니다.<br />
해당 사유는 사용자에게 노출되지 않고 운영 이력으로만 확인 가능합니다.<br />

#### (4) 노출 메시지
유저에게 노출할 이용 정지 메시지를 입력합니다. <br/>
이용정지 등록을 보다 쉽게 하기 위해서 사용자에게 노출할 메시지를 다국어로 입력하여 재사용이 가능하도록 템플릿을 제공합니다. 미리 등록한 템플릿을 선택하여 등록합니다.<br />

> <font color="red">[WARNNING]</font><br/>
> 노출메시지의 템플릿이 등록된 경우에만 이용정지 등록이 가능합니다. <br/>
> 템플릿을 등록하지 않은 경우 [BAN]-[템플릿]메뉴에서 템플릿 등록을 먼저 진행하세요.<br/>
> LINK [[템플릿 등록하기](./ban/#template)] <br/>


### Release Ban

이용정지 조회 화면에서 '해제'버튼을 클릭하면 이용정지 해제가 가능합니다.

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban3_1.2.png)

#### (1) 해제 사유
유저의 이용 정지 해제를 진행하게 되는 사유를 입력합니다.<br />
해당 사유는 사용자에게 노출되지 않고 운영 이력으로만 확인 가능합니다.<br />

#### (2) 유저 ID
이용 정지를 해제할 Gamebase User ID를 입력합니다. 한번에 다수의 유저 등록이 가능하며, 등록방법은 아래 두가지 방법을 제공합니다.

- 사용자 입력 : 등록할 유저ID를 입력창에 직접 입력 후 'Enter'키를 누르거나 '추가'버튼을 클릭합니다. User ID 유효성을 검사하므로 유효하지 않은 User ID나 이용정지상태가 아닌 USer ID는 입력이 불가능합니다.
- 일괄 등록 : csv파일만 업로드 가능하며 예시파일은 console화면에서 다운로드 받을 수 있습니다. 일괄 등록은 1회 최대 10,000명까지 등록 가능합니다. <br/>
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban6_1.0.png)


> <font color="blue">[TIPs]</font><br/>
> 일괄등록을 이용하여 이용정지 해제 진행 중 실패하는 경우 팝업이 노출되고, 해당 팝업에서 'Download'버튼을 클릭하면 실패 유저 리스트를 Local PC에 파일로 다운로드 받을 수 있습니다.<br />
> ![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Ban7_1.0.png)

## Template
이용정지 유저에게 노출할 메시지를 다국어로 입력하여 재사용이 가능하도록 템플릿을 제공합니다.
언어별로 등록 가능하며 이용 정지 유저에게는 디바이스에 설정 된 언어를 기준으로 이용 정지 메시지가 노출됩니다.

### Search

등록된 템플릿 목록 조회 및 검색이 가능합니다.<br/>
새로운 템플릿 등록과 등록된 템플릿의 항목 수정만 가능하며, 등록된 템플릿의 삭제는 불가능합니다.<br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template1_1.1.png)

- 목록에서 노출메시지는 '기본언어'로 설정된 메시지가 노출됩니다.

### Register Template
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_Template2_1.1.png)

#### (1) 이름
이용정지 등록시 리스트에 노출할 템플릿의 이름을 입력합니다. <br/>

#### (2) 노출 메시지	
이용 정지 유저에게 보여줄 메시지를 입력합니다. <br />
다국어 지원을 위해 여러개의 언어로 등록 가능하며, 입력한 언어 이외의 언어를 사용하는 사용자에게는 기본 언어로 선택된 언어가 노출됩니다. 오른쪽의 **'+'**버튼을 클릭하면 언어 추가가 가능하며 원하는 언어가 없는 경우 담당자에게 연락을 주시면 새로운 언어 추가가 가능합니다.<br />

## AppGuard(앱가드)

> <font color="red">[WARNNING]</font><br/>
> TOAST Cloud Appguard 상품을 사용하는 경우에만 이용 가능합니다.  <br/>

![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Ban_AppGuard1_1.0.png)

- 연동여부 : 앱가드에 의해 탐지 및 제재된 사용자를 자동으로 Gamebase 이용정지 유저로 등록하고자 하는 경우에 활성화합니다.
- 탐지/제재 종류별로 '자동이용정지'를 원하는 경우에 'ON'을 선택하여 '사용자 노출메시지'와 '이용정지기간'을 입력하여 '저장'버튼을 클릭하면 적용됩니다.

> [INFO] <br/>
> 앱가드 연동으로 이용정지가 자동으로 등록된 경우 등록시스템에 '앱가드'로 등록됩니다.
