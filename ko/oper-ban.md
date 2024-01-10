## Game > Gamebase > 콘솔 사용 가이드 > 이용 정지

앱을 부당하게 사용하거나 어뷰징하는 게임 유저에 대해 앱 이용을 제한할 수 있는 이용 정지 기능을 제공합니다.
이용이 정지된 게임 유저가 다시 로그인하거나 세션을 복구하는 경우에 이용 정지 팝업이 표시되어 게임 이용이 제한됩니다.

이용 정지 등록은 Gamebase Console에서 수동으로 등록하거나 NHN Cloud AppGuard를 사용하는 경우 패턴등록을 이용해 자동으로 등록할 수 있습니다.

AppGuard를 연동하는 방법은 [AppGuard](./oper-ban/#appguard)를 참고하시기 바랍니다.


## Ban

이용 정지 이력을 조회하거나 이용 정지 등록, 이미 이용 정지 중인 게임 유저의 이용 정지 해제가 가능합니다.

### Search Banned User

검색 조건에 맞는 이용 정지/이용 정지 해제 게임 유저 목록을 조회합니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_01_ko_240105.jpg)

**검색 조건**

- **상태**: (필수) 원하는 이용 정지 상태 선택(이용 정지/이용 정지해제). 두 가지 모두 선택은 불가능하며 한 가지만 필수로 선택해야 합니다.
- **등록일**: (필수) 이용 정지 등록일 구간 선택(from-to)
- **유저 ID**: 이용 정지/이용 정지 해제된 Gamebase 사용자 아이디
- **템플릿**: 이용 정지 등록에 사용한 특정 템플릿을 선택하여 조회
- **등록 시스템**: 이용 정지 등록한 시스템을 선택하여 조회. 다중 선택 가능
    - **콘솔**: Gamebase Console을 통하여 등록
    - **앱가드**: AppGuard 연동으로 자동 등록
    - **외부 서버**: 앱을 운영하는 서버 또는 기타 다른 외부 서버에서 등록
    - **기타**: 앞의 경우를 제외한 나머지 이용 정지 등록(API 직접 호출 등)

> [참고]
>
> 유저에게 표시할 메시지를 다국어로 입력하여 손쉽게 재사용할 수 있도록 템플릿을 제공합니다.
> 등록된 템플릿이 하나 이상이여야 이용 정지 등록을 할 수 있습니다.
> 템플릿을 등록하는 방법은 [Template](./oper-ban/#template)을 참고하시기 바랍니다.

**검색 결과**

- **유저 ID**: 이용 정지 게임 이용자 아이디
- **기간**: 이용 정지 기간. 영구 정지인 경우 '영구정지'로 노출
- **템플릿**: 이용 정지 등록에 사용한 템플릿
- **사유**: 이용 정지 등록 시에 운영자가 입력한 사유. 해당 사유는 유저에게 표시되지 않고 운영 이력으로만 확인할 수 있습니다.
- **등록자/등록일**: 이용 정지 등록한 운영자 계정/이용 정지 등록일
- **해제 사유**: 이용 정지 해제 시에 운영자가 등록한 사유. 해당 사유는 유저에게 노출되지 않고 운영 이력으로만 확인할 수 있습니다.
- **해제 등록자/해제 등록일**: 이용 정지 해제한 운영자 계정/이용 정지 해제일
- **해제**: 이용 정지 상태인 유저는 검색 목록에서 이용 정지 해제가 가능하도록 **이용해제** 버튼이 표시됩니다. 버튼을 클릭하면 이용 정지 해제 사유를 입력하는 팝업이 표시되고, 이용 정지 해제 사유를 입력하고 **저장** 버튼을 클릭하면 이용 정지를 해제할 수 있습니다.
- **상태**
    - <font color="white" style="background-color:#FB8F37">이용 정지</font>: 게임 유저가 현재 앱에 접속할 수 없는 상태
    - <font color="white" style="background-color:#A1A1A1">이용 정지(기간 만료)</font>: 게임 유저의 이용 정지 기간이 끝났으나 현재 로그인을 하지 않은 상태. 게임 유저가 재접속 시 해제(기간 만료)로 상태가 변경됨
    - <font color="white" style="background-color:#88C637">해제</font>: 운영자에 의해서 게임 유저의 이용 정지가 해제된 상태
    - <font color="white" style="background-color:#2AB1A6">해제(기간 만료)</font>: 이용 정지 기간이 끝난 이후 자연스럽게 게임 유저가 게임에 재접속하여 이용 정지가 해제된 상태


> [참고]
>
> **파일 다운로드** 버튼을 클릭하면 검색 결과를 CSV 파일로 저장할 수 있습니다.



### Register Ban

이용 정지 조회 화면에서 **등록** 버튼을 클릭하면 이용 정지 등록이 가능합니다.

![gamebase_ban_02_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_02_ko_240105.jpg)
#### (1) 유저 ID
이용 정지를 등록할 Gamebase 유저 아이디를 입력합니다. 한 번에 다수의 유저를 등록할 수 있으며, 등록 방법은 아래 두가지입니다.

- **사용자 입력**: 등록할 유저 아이디를 입력 창에 직접 입력한 후 **Enter** 키를 누르거나 **추가** 버튼을 클릭합니다. 유저 아이디 유효성을 검사하므로 유효하지 않은 유저 아이디는 입력이 불가능합니다.
- **일괄 등록**: CSV 파일만 업로드할 수 있으며 예시 파일은 Console 화면에서 다운로드할 수 있습니다. 일괄 등록은 1회 최대 10,000명까지 가능합니다.
  ![gamebase_ban_03_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_03_ko_240105.jpg)

> [참고]</br>
>
> 일괄 등록을 진행하다가 실패하면 팝업이 표시됩니다. 해당 팝업에서 **Download** 버튼을 클릭하면 등록에 실패한 유저 목록을 파일로 다운로드할 수 있습니다.
> ![gamebase_ban_04_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_04_ko_240105.jpg)

#### (2) 기간
게임 유저의 이용 정지 기간을 설정합니다. 이용 정지가 등록되는 시점부터 게임 유저는 로그인이 불가능합니다.

- **영구 이용 정지**: 영구 이용 정지를 하려면 선택합니다.
- **기간 지정**: 이용 정지 기간을 입력합니다. 일(day)과 시간(hour) **예상 만료 시간** 정보로 유저의 이용 정지 기간을 미리 확인하실 수 있습니다.

#### (3) 사유
유저의 이용 정지 사유를 입력합니다.
해당 사유는 유저에게 노출되지 않고 운영 이력으로만 확인할 수 있습니다.

#### (4) 노출 메시지
유저에게 표시할 이용 정지 메시지를 입력합니다.
유저에게 표시할 메시지를 다국어로 입력하여 손쉽게 재사용할 수 있도록 템플릿을 제공합니다. 미리 등록한 템플릿을 선택하여 등록합니다.

> <font color="red">[중요]</font>
> 표시된 메시지의 템플릿이 등록된 경우에만 이용 정지를 등록할 수 있습니다.
> 템플릿을 등록하지 않은 경우 **BAN** 메뉴의 **템플릿** 탭에서 템플릿을 먼저 등록해 주세요.
> 템플릿을 등록하는 방법은 [Template](./oper-ban/#template)을 참고하시기 바랍니다.

#### (5) 리더보드 삭제
이용 정지를 등록할 때 해당 게임 유저의 Leaderboard 데이터도 함께 삭제할지 여부를 설정합니다.
선택 후 등록하면 리더보드에서 게임 유저의 데이터가 삭제되며 <font color="red">해당 데이터는 복구되지 않으므로</font> 주의해야 합니다.

### Release Ban

이용 정지 조회 화면에서 **해제** 버튼을 클릭하면 이용 정지를 해제할 수 있습니다.

![gamebase_ban_05_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_05_ko_240105.jpg)

#### 해제 사유
유저의 이용 정지를 해제하는 사유를 입력합니다.
해당 사유는 유저에게 표시되지 않고 운영 이력으로만 확인할 수 있습니다.

#### 유저 ID
이용 정지를 해제할 Gamebase 유저 아이디를 입력합니다. 한 번에 다수의 유저를 등록할 수 있으며, 등록 방법은 아래 두가지입니다.

- **사용자 입력**: 등록할 유저 아이디를 입력 창에 직접 입력한 후 **Enter** 키를 누르거나 **추가** 버튼을 클릭합니다. 유저 아이디 유효성을 검사하므로 유효하지 않은 유저 아이디는 입력이 불가능합니다.
- **일괄 등록**: CSV 파일만 업로드할 수 있으며 예시 파일은 Console 화면에서 다운로드할 수 있습니다. 일괄 등록은 1회 최대 10,000명까지 가능합니다.

![gamebase_ban_06_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_06_ko_240105.jpg)


> [참고]
>
> 일괄 등록을 진행하다가 실패하면 팝업이 표시됩니다. 해당 팝업에서 **Download** 버튼을 클릭하면 등록에 실패한 시용자 목록을 파일로 다운로드할 수 있습니다.
> ![gamebase_ban_04_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_04_ko_240105.jpg)

## Template
이용 정지 대상 유저에게 표시할 메시지를 다국어로 입력하여 손쉽게 재사용할 수 있도록 템플릿을 제공합니다. 미리 등록한 템플릿을 선택합니다.
언어별로 등록할 수 있으며 이용 정지 대상 유저에게는 디바이스에 설정된 언어를 기준으로 이용 정지 메시지가 표시됩니다.

### Search

등록된 템플릿 목록을 검색할 수 있습니다.
새 템플릿을 등록하거나 등록된 템플릿을 수정할 수 있으며, 등록된 템플릿을 삭제할 수는 없습니다.

![gamebase_ban_07_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_07_ko_240105.jpg)

- 템플릿 목록화면에서 노출메시지 항목에는 템플릿 등록시 '기본 언어'로 입력한 노출 메시지가 표시됩니다.

### Register Template
![gamebase_app_01_202004](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_08_ko_240105.jpg)

#### (1) 이름
이용 정지 등록 시 목록에 표시할 템플릿의 이름을 입력합니다.

#### (2) 노출 메시지
이용 정지 대상 유저에게 표시할 메시지를 입력합니다.
여러 개의 언어로 등록할 수 있으며, 입력한 언어 이외의 언어를 사용하는 유저에게는 '기본 언어'로 선택된 언어가 표시됩니다. 오른쪽의 **+** 버튼을 클릭하면 언어를 추가할 수 있으며 원하는 언어가 없는 경우 [고객 센터](https://toast.com/support/inquiry)로 연락 주시면 새로운 언어를 추가할 수 있습니다.
'기본 언어로 자동 번역'버튼을 선택할 경우 기본언어로 입력된 내용을 기반으로 내용을 번역하여 각 항목에 설정된 언어에 맞게 내용이 입력됩니다.

## AppGuard

> <font color="red">[중요]</font>
> AppGuard 연동 기능은 NHN Cloud에서 해당 기능을 적용하려는 서비스와 동일한 프로젝트에 NHN AppGuard 서비스를 활성화한 경우에만 이용할 수 있습니다.

![gamebase_ban_09_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/Ban/ko/gamebase_ban_09_ko_240105.jpg)

- **연동 여부**: AppGuard에서 탐지 또는 제재된 유저를 자동으로 Gamebase 이용 정지 유저로 등록하고자 하는 경우에 활성화합니다.
- **자동 이용 정지**는 **즉시 차단**, **조건 차단** 두 가지가 있습니다.
    - **즉시 차단**에 의한 이용 정지는 매시간 15분, 45분에 수행됩니다. **즉시 차단**은 탐지 기간을 설정할 수 없으며, 탐지 횟수는 1회로 고정됩니다.
    - **조건 차단**에 의한 이용 정지는 하루에 한 번 자정에 수행됩니다.
        - 예: 변조에 대해 탐지 기간: 5, 탐지 횟수: 3 을 설정한 경우
            - 1월 14일 ~ 1월 18일 동안 특정 유저가 변조에 의한 탐지가 3회 이상 검출된 경우, 1월 19일 자정에 이용 정지가 등록됩니다.
- **이용 정지 타입**은 **영구 이용 정지**, **조건 이용 정지** 두 가지가 있습니다.
    - **영구 이용 정지**는 자동 이용 정지 조건에 맞는 유저를 영구적으로 이용 정지합니다.
    - **조건 이용 정지**는 자동 이용 정지 조건에 맞는 유저를 설정한 기간 동안 이용 정지합니다.
- **리더보드 삭제** 항목을 선택하면 자동 이용 정지된 유저에 대해서, 이용 정지 시 리더보드 데이터도 함께 삭제합니다.

> [참고]
>
> AppGuard 연동으로 이용 정지가 자동으로 등록된 경우 등록 시스템에 '앱가드'로 등록됩니다.