## Game > Gamebase > Console for AWS

AWS marketplace를 통해 회원가입을 한 사용자가 로그인하는 콘솔의 기본적인 설정과 사용 방법을 안내합니다.

Gamebase for AWS 의 콘솔은 아래와 같은 기능을 제공합니다.
* 프로젝트, 서비스 관리
* 서비스를 이용하는 회원 관리

## 퀵가이드

콘솔에서 제공하는 기본 기능에 대한 퀵 가이드입니다.

![Gamebase-for-aws_quick-guide](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-01-202108)
![Gamebase-for-aws_quick-guide](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-02-202108)
![Gamebase-for-aws_quick-guide](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-03-202108)
![Gamebase-for-aws_quick-guide](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_quick-guide-04-202108)

## 프로젝트 관리

Gamebase는 프로젝트 단위로 이용하여, 이에 따라 과금합니다.

### 프로젝트 생성

* aws marketplace페이지를 통해 가입한 관리자 회원만 프로젝트를 생성, 수정, 삭제 할 수 있습니다.
* 프로젝트 이름과 설명을 입력하면 프로젝트의 ID는 자동으로 생성됩니다.
* 프로젝트를 생성하면 Gamebase 서비스는 자동으로 활성화 됩니다.
* 프로젝트 생성 후 협업이 필요한 경우 프로젝트 멤버로 일반 회원을 추가할 수 있습니다.

![Gamebase-for-aws_project-create](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-01-202108)
![Gamebase-for-aws_project-create](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_project-02-202108)

1. **+새프로젝트** 버튼을 클릭하여 프로젝트를 생성합니다.
2. **프로젝트 이름**과 **프로젝트 설명**을 입력합니다.
3. **확인** 버튼을 클릭하여 프로젝트를 생성합니다.
4. 프로젝트가 생성되면 메뉴에 프로젝트 이름이 표시됩니다.
5. **대시보드** 화면에서 프로젝트 정보를 확인합니다.


### 프로젝트 편집

* 프로젝트 상세 화면의 우측 상단의 **편집** 버튼을 클릭하면 프로젝트 편집 화면이 표시됩니다.
* 편집 화면에서는 프로젝트 정보의 수정 및 삭제, 이용중인 서비스의 비활성화가 가능합니다.
* 프로젝트 이름과 설명은 수정 가능하나 프로젝트 ID는 수정이 불가능합니다.
* 프로젝트에서 이용 중인 서비스가 없을 경우에 프로젝트 삭제가 가능합니다.
* 프로젝트 삭제 시, 프로젝트의 모든 리소스는 삭제되며 복원이 불가능합니다.
	
### 프로젝트 변경

* 상단의 프로젝트 이름을 클릭하면 등록된 프로젝트 목록을 확인할 수 있습니다.


## 회원 

| 구분     | 관리자 | 일반 | 
| ------ | ------------ | ------------ | 
| 정의     | 프로젝트 관리 | 서비스 이용만 가능 | 
| 등록 방법 | aws marketplace 구독 페이지를 통해 가입  | 관리자가 콘솔에서 생성한 회원 | 
| 권한 | - 프로젝트 관리: 생성/수정/삭제<br>- 일반 회원 관리 | 권한이 부여된 서비스 콘솔에만 접근 | 

### 프로젝트에 일반 회원 추가하기

아래의 순서로 콘솔에 접근할 수 있는 일반 회원을 추가할 수 있습니다.
#### 1. 일반 회원 추가하기

![Gamebase-for-aws_create-ID](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-01-202108)

1. **이름 > 일반/그룹 관리** 클릭하면 일반 회원 관리 페이지가 표시됩니다.
2. **+회원 추가** 버튼을 클릭합니다.
3. ID, 이름, 메일을 입력하면 입력한 메일로 **비밀번호 설정 확인 메일**이 발송됩니다.
4. 전달받은 메일을 이용하여 비밀번호를 변경하면 추가된 ID로 로그인이 가능합니다.

#### 2. 프로젝트 멤버로 추가

![Gamebase-for-aws_project-member](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-02-202108)
1. 프로젝트 대시보드 화면에서 **+프로젝트 멤버 추가** 버튼을 클릭합니다.
2. 1에서 추가한 일반 회원 ID를 입력하고 그룹 권한을 부여하며 현재 프로젝트에 접근이 가능합니다.

### 그룹 권한 관리

일반 회원의 서비스 콘솔 접근 권한을 관리할 수 있습니다.
**이름 > 일반/그룹 관리** 클릭하여 권한 그룹 관리 페이지에서 권한 그룹의 생성, 수정, 삭제가 가능합니다.

![Gamebase-for-aws_project-member](http://static.toastoven.net/prod_gamebase/console-for-aws/console_Gamebase-for-aws_member-03-202108)
1. 추가하고자 하는 그룹명을 입력하고 **추가** 버튼을 클릭합니다.
2. 추가된 그룹명을 선택하면 화면의 오른쪽에 그룹의 권한을 수정할 수 있는 화면이 표시됩니다.
3. 그룹 수정 화면에서 서비스 메뉴별로 Read, ALL 권한 부여가 가능합니다. 부여하고자 하는 권한 체크후 저장 버튼을 클릭합니다.
