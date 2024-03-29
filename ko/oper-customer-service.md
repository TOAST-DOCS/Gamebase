## Game > Gamebase > 콘솔 사용 가이드 > 고객센터

게임 운영 중 유저에게 인입된 문의를 처리할 수 있으며 그 외 고객센터 페이지를 통해 제공할 수 있는 공지사항, FAQ 등의 설정을 관리할 수 있습니다.
문의 처리시에 유저에게 발송될 이메일 설정 및 자주 쓰이는 답변을 템플릿으로 등록하여 활용하실 수도 있습니다.
> [참고]
> 이 메뉴를 이용하시려면 앱 - 고객센터 설정에서 Gamebase 제공 고객센터 항목을 선택하셔야 합니다.
>

## Help Center Web Page 

유저에게 노출되는 고객센터 웹페이지에 대해서 설명합니다.
해당 화면을 통해 유저는 1:1 문의를 등록하고 내 문의 내역 조회가 가능하며, 자주하는 질문 및 공지사항을 확인할 수 있습니다.

### Main 

게임에서 Gamebase SDK를 이용하여 고객센터 페이지를 오픈하면 다음과 같은 화면이 유저에게 노출됩니다.
![main](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_01_ko_240105.jpg)

#### (1) 1:1 문의

**1:1 문의** 버튼을 클릭하면 1:1 문의를 등록하는 화면으로 이동합니다.

![문의하기](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_02_ko_240105.jpg)

다음은 문의 등록 시에 입력하는 항목입니다.
등록된 문의는 **[고객센터 > 고객문의](./oper-customer-service/#inquiry)** 콘솔에서 확인하고 답변 처리가 가능합니다.

1. 문의 유형: 접수 문의 유형을 선택합니다. 접수 문의 유형은 [고객센터 > 고객문의](./oper-customer-service/#inquiry)에서 등록, 수정, 삭제가 가능합니다. 
2. 답변 이메일: 문의에 관한 답변을 받을 이메일 주소를 입력합니다. 콘솔에서 문의 처리를 완료하면 입력된 이메일 주소로 자동으로 메일을 발송합니다.
3. 이름(닉네임): 게임에서 사용하는 닉네임을 입력합니다. 최대 10자까지 입력 가능합니다. 
게임 닉네임을 추가 정보로 설정하여 고객센터 페이지를 열면 유저가 입력하지 않아도 자동으로 닉네임이 입력됩니다.
4. 제목: 문의 제목을 입력합니다. 최대 100자까지 입력 가능합니다.
5. 문의 내용: 문의 내용을 작성하여 입력합니다. 최대 1,000자까지 입력 가능합니다.
6. 첨부: 첨부파일이 있는 경우 등록합니다. 10MB 미만의 파일 5개까지 첨부 가능합니다.

> [참고] 
> 접수 문의 유형을 선택 시에 해당 문의 유형에 템플릿이 설정되어 있으면, 문의 내용에 자동으로 템플릿 내용이 작성됩니다.
> 템플릿 설정은 [고객센터 > 고객 문의 > 문의 유형 관리](./oper-customer-service/#inquiry)에서 할 수 있습니다.

#### (2) 내 문의 내역

로그인을 하고 고객센터 웹페이지에 접근하여야 **내 문의 내역** 버튼이 표시됩니다. 클릭하면 이전에 고객이 문의한 내역을 확인하는 화면으로 이동합니다.
![내문의내역_로그인](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_03_ko_240105.jpg)

내 문의 내역은 기본 10개의 목록을 확인할 수 있고 10개 이상인 경우 **더 보기** 클릭 시 10개가 추가로 노출됩니다.

> [참고] 로그인을 하지 않으면 내 문의 내역을 확인할 수 없습니다.
> 로그인 하지 않고 문의를 남기면 이메일로만 문의 내역 확인이 가능하며 내 문의 내역에서의 확인 또한 불가능합니다.
> ![내문의내역_비로그인](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_04_ko_240105.jpg)

#### (3) 자주하는 질문

FAQ에서는 카테고리 질문 및 자주하는 질문 등을 확인할 수 있습니다. 리스트에서는 최대 12개가 노출됩니다.
원하는 내용을 검색하거나 카테고리 버튼을 클릭하여 [고객센터 > FAQ](./oper-customer-service/#faq)에서 등록한 FAQ내용을 확인할 수 있습니다.
![FAQ](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_05_ko_240105.jpg)

1) 확인하고 싶은 검색어를 입력하여 검색어가 포함된 FAQ를 확인할 수 있습니다.
2) 자주 하는 질문으로 등록된 질문을 확인할 수 있습니다.
3) FAQ 등록시 설정한 **FAQ 유형 관리**별로 묶어서 FAQ를 확인할 수 있습니다.
4) FAQ 카테고리는 [Gamebase Console > 고객센터 >  FAQ 유형관리](./oper-customer-service/#search-faq)를 통해 추가하거나 삭제할 수 있습니다.

#### (4) 공지사항
**고객센터 > 공지사항**에 등록된 게시물을 확인할 수 있습니다.

메인 화면에서는 최근에 작성한 3개의 게시물이 표시되고 상단 고정 게시물은 굵은 폰트로 표시됩니다. **more**를 클릭하여 등록된 전체 공지사항을 확인할 수 있습니다.
작성일 내림차순으로 정렬되어 공지사항 게시물이 노출되고 상단 고정으로 지정한 공지는 굵은 폰트로 상단에 우선 노출됩니다. 노출 기간이 지난 게시물은 목록에서 더 이상 표시되지 않습니다. 게시물을 클릭하면 상세 내용을 확인할 수 있습니다.
![공지사항](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_help_center_06_ko_240105.jpg)

## Inquiry
고객에게 인입된 문의를 처리하거나 조회할 수 있습니다.
그 외에도 고객이 문의를 등록하고자 할 때 필요한 접수 유형 항목을 설정할 수 있으며 문의가 처리되었을 때 유저에게 발송되는 Push 알람에 대한 설정도 가능합니다.

### Search Inquiry

검색 조건에 맞는 고객 문의 내역을 검색합니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_01_ko_240105.jpg)

**검색 조건**

- **상태**: (필수) 고객 문의의 현재 처리 상태를 선택합니다.
- **접수 유형**: (필수) 고객 문의시 유저가 선택한 접수 유형 항목을 선택합니다. 접수 / 처리 완료 항목이 존재합니다.
- **접수 기간**: (필수)선택한 기간동안 인입된 문의를 조회합니다.
- **유저 ID**: 특정 유저의 문의를 검색하려면 유저ID를 입력합니다.
- **문의 제목**: 특정 제목의 문의를 검색하려면 문의 제목을 입력합니다.

**검색 결과**

- **접수 유형**: 유저가 문의 등록시 선택한 접수 유형 항목
- **문의 제목**: 유저가 문의 등록시 입력한 문의 제목
- **접수일**: 유저의 문의 등록 일시
- **처리일**: 담당자가 인입된 문의를 처리한 일시
- **상태**: 인입된 문의의 현재 처리 상태
    - 접수: 유저가 문의를 등록한 상태입니다. 기존 문의 글에 추가 문의를 남겨도 접수 상태로 변경됩니다.
    - 보류: 담당자가 답변을 남길 때, 보류로 작성한 상태입니다. 추가로 확인이 필요한 경우 사용됩니다.
    - 해결: 담당자가 답변을 남길 때, 해결로 작성한 상태입니다. 문의가 해결된 상태입니다.
    - 완료: 담당자가 완료 처리 혹은 해결된 문의가 2주 지나면 자동으로 완료 상태가 됩니다.

#### 1. 문의 유형 관리
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_02_ko_240105.jpg)

유저가 문의 등록시 선택할 수 있는 접수 유형 항목을 관리할 수 있습니다.
지원하는 언어별로 등록이 가능하며 항목별 최대 글자수는 20자입니다.
표시되는 순서대로 유저에게 목록이 표시되며 해당순서는 드래그앤 드랍을 이용하여 목록 내에서 변경이 가능합니다.
**고객센터 > 템플릿**에서 등록한 템플릿을 선택하여 고객이 문의 작성 시 문의 대응에 필요한 정보들을 작성할 수 있도록 할 수 있습니다.
> [참고]
> 지원 언어 선택 현황은 앱 - 고객센터 설정에서 확인할 수 있습니다.

#### 2. 답변 발송 설정
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_03_ko_240105.jpg)

문의에 대한 처리가 완료되었을 경우 유저에게 Push 메시지를 통해 알림을 발송하고자 할 경우에 해당 기능을 설정할 수 있습니다.
사용하고자 할 경우에 상단에 발송여부를 체크하면 유저에게 처리완료 시 완료 Push알람이 함께 전송됩니다.
글로벌 서비스의 경우 원하는 언어를 추가로 등록하여 발송할 수 있으며 유저의 기기설정에 따라 맞는 언어설정으로 유저에게 푸시 알람이 전송되게 됩니다.
> [참고]
> 1. 이 기능을 사용하려면 NHN Cloud Push 상품이 먼저 활성화되어 있어야 합니다.
> 2. 답변발송 설정의 언어선택의 경우 고객센터에서 지원되는 언어와 무관하게 Gamebase에서 지원되는 모든 언어를 등록할 수 있습니다.

### Inquiry details

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_04_ko_240105.jpg)

유저에게 인입된 문의에 대하여 상세내용 확인 및 해당 문의에 대한 처리를 진행할 수 있습니다.
문의를 처리한 후 유저가 추가로 문의할 수 있습니다.
문의를 처리한 후에 **처리 완료** 버튼을 클릭하여 문의 상태를 처리 완료 상태로 변경할 수 있으며, 처리 완료 상태의 문의에는 유저가 더는 문의를 남길 수 없습니다.

템플릿을 선택할 경우 고객센터 - 답변 템플릿 메뉴에서 설정한 템플릿 내용을 답변에서 바로 사용할 수 있으며 템플릿 내용 외에 Text editor를 이용하여 원하시는 모양대로 답변을 만들어 유저에게 전달할 수 있습니다.
유저 문의 답변시 파일첨부가 필요할 경우 최대 10MB 이내의 파일을 5개까지 첨부하여 유저에게 전달가능합니다.
이후 처리하기가 완료될 경우 문의 인입시 등록된 email을 통해 담당자가 작성한 내용이 유저에게 전달됩니다.
이 때 답변 발송 항목을 통해 문의 처리 완료 시 해당 유저에게 Push알람이 전송되는지에 대한 여부를 확인할 수 있습니다.
> [참고]
> ![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_05_ko_240105.jpg)
> 로그인된 유저가 문의를 등록했을 경우 해당 유저에 대한 정보가 한 화면에 조회되어 확인할 수 있습니다.
> 우측 X 버튼을 클릭하여 창을 닫을 수 있으며, 유저 ID를 클릭 시 다시 노출됩니다.
> 기존 회원 메뉴에서 사용했던 기능과 동일하게 유저정보가 조회되므로 유저 문의대응시 필요한 정보를 한화면에서 편리하게 확인하실 수 있습니다.

#### 1. 답변 발송 설정
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_inquiry_03_ko_240105.jpg)

문의에 대한 처리가 완료되었을 경우 유저에게 Push 메시지를 통해 알림을 발송하고자 할 경우에 해당 기능을 설정할 수 있습니다.
사용하고자 할 경우에 상단에 발송여부를 체크하면 유저에게 처리완료 시 완료 Push알람이 함께 전송됩니다.
글로벌 서비스의 경우 원하는 언어를 추가로 등록하여 발송할 수 있으며 유저의 기기설정에 따라 맞는 언어설정으로 유저에게 푸시 알람이 전송되게 됩니다.
> [참고1]
> 1. 이 기능을 사용하려면 NHN Cloud Push 상품이 먼저 활성화되어 있어야 합니다.
> 2. 답변발송 설정의 언어선택의 경우 고객센터에서 지원되는 언어와 무관하게 Gamebase에서 지원되는 모든 언어를 등록할 수 있습니다.

> [참고2]
> 문의 처리가 완료된 고객 문의를 조회할 경우에는 문의 내역 및 처리 내역을 함께 조회할 수 있으며 처리 당시에 등록한 첨부파일이 있을 경우 해당항목을 클릭하여 다운이 가능합니다.


## FAQ

고객센터 페이지에서 제공되는 FAQ 항목에 대한 관리를 진행할 수 있습니다.

### Search FAQ

등록되어 있는 FAQ 항목에 대하여 검색할 수 있습니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_faq_01_ko_240105.jpg)

**검색 조건**

- **상태**: (필수) FAQ의 노출 형태를 선택합니다. 노출 / 비노출 항목을 선택할 수 있습니다.
- **유형**: (필수) FAQ의 유형을 선택하여 검색할 수 있습니다. 선택항목은 FAQ 유형 관리에서 등록된 내용을 기준으로 표시됩니다.
- **질문+답변**: 질문 또는 답변에 특정한 키워드를 포함한 FAQ를 검색하고자 할 때 사용합니다. 다른 언어로 등록된 내용을 등록하고자 할 경우에는 검색할 언어를 지정한 후에 검색합니다.

**검색 결과**

- **자주하는 질문**: FAQ가 자주하는 질문란에 들어가있는지에 대한 여부를 표시합니다.
- **유형**: 등록된 FAQ의 분류 유형을 표시합니다.
- **제목**: FAQ의 질문 제목입니다.
- **수정자**: FAQ를 마지막으로 등록 또는 수정한 유저의 정보를 보여줍니다.
- **수정일**: FAQ가 마지막으로 등록 또는 수정된 날짜 정보를 보여줍니다.
- **상태**: FAQ가 현재 표시되고 있는지 여부를 보여줍니다. 노출 중 / 비노출 상태가 있습니다.

#### FAQ 유형 관리
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_faq_02_ko_240105.jpg)

FAQ 등록 또는 수정시 선택할 수 있는 유형을 관리할 수 있습니다.
지원하는 언어별로 등록이 가능하며 항목별 최대 글자수는 20자입니다.
표시되는 순서대로 표시되며 해당순서는 드래그앤 드랍을 이용하여 목록 내에서 변경이 가능합니다.
> [참고]
> 지원 언어 선택 현황은 앱 - 고객센터 설정에서 확인할 수 있습니다.

### Register or Update FAQ
FAQ를 등록하거나 또는 기존에 등록된 FAQ정보를 수정할 수 있습니다.
등록 또는 수정 시 변경할 수 있는 항목은 모두 동일합니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_faq_03_ko_240105.jpg)

#### 1. 상태
등록 또는 수정하고자 하는 FAQ의 노출 상태를 선택합니다.
노출 / 비노출 항목이 있으며 고객센터 페이지에서 실제 유저에게 노출할지에 대한 여부를 선택하면 됩니다.

#### 2. 유형
FAQ 유형 관리에서 등록된 유형을 기반으로 등록 또는 수정하고자 하는 FAQ의 유형을 선택합니다.

#### 3. 자주하는 질문
고객센터 페이지에서 자주하는 질문란에 해당 질문을 표시할 지에 대한 여부를 체크합니다.

#### 4. 질문
FAQ 질문 내용을 입력합니다.
> [참고]
> 앱 - 고객센터에서 설정한 지원 언어들은 모두 입력해야 등록이 가능합니다.

####. 5. 답변
FAQ 질문에 대한 답변 내용을 입력합니다.
Text Editor를 통해 원하는 형태로 답변을 입력할 수 있으며 해당형태 그대로 웹페이지에 노출됩니다.
> [참고]
> 앱 - 고객센터에서 설정한 지원 언어들은 모두 입력해야 등록이 가능합니다.

## Notice

고객센터 페이지에서 제공할 공지사항에 대한 관리를 진행할 수 있습니다.

### Search Notice

등록되어 있는 공지사항 목록에 대하여 검색할 수 있습니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_notice_01_ko_240105.jpg)

**검색 조건**

- **검색 유형**: (필수) 공지사항의 검색 유형을 선택할 수 있습니다. 기본으로 상태가 선택되어 있으며 노출일을 기준으로 검색하고자 할 경우 해당항목을 선택한 후에 동일하게 검색할 수 있습니다.
- **상태**: (필수) 위 검색 유형에서 기본으로 선택되어 있는 항목으로 현재 공지의 노출 상태를 기준으로 검색합니다. 예정 / 노출 중 / 종료 항목을 선택할 수 있습니다.
- **노출일**: 검색 유형에서 노출일 항목을 선택헀을 경우 설정할 수 있으며 선택된 노출일을 기준으로 보여지는 공지사항 목록을 검색하여 볼 수 있습니다.
- **제목+내용**: 제목 또는 내용에 특정한 키워드를 포함한 공지사항을 검색하고자 할 때 사용합니다. 다른 언어로 등록된 내용을 등록하고자 할 경우에는 검색할 언어를 지정한 후에 검색합니다.

**검색 결과**
- **상단 고정**: 해당 공지사항이 상단 고정란에 들어가있는지에 대한 여부를 표시합니다.
- **유형**: 등록된 공지사항의 분류 유형을 표시합니다.
- **제목**: 공지사항의 제목입니다.
- **노출일(UTC+9)**: 공지사항이 노출될 때 실제 노출할 등록일(표시일)을 보여줍니다.
- **노출 기간**: 해당 공지사항의 노출 기간을 표시합니다.
- **상태**: 공지사항의 현재 진행여부 보여줍니다. 예정 / 노출 중 / 종료 상태가 있습니다.

#### 말머리 관리
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_notice_02_ko_240105.jpg)

공지사항 등록 또는 수정시 선택할 수 있는 말머리를 관리할 수 있습니다.
지원하는 언어별로 등록이 가능하며 항목별 최대 글자수는 20자입니다.
표시되는 순서대로 표시되며 해당순서는 드래그앤 드랍을 이용하여 목록 내에서 변경이 가능합니다.
> [참고]
> 지원 언어 선택 현황은 앱 - 고객센터 설정에서 확인할 수 있습니다.

### Register or Update Notice
새로운 공지사항을 등록하거나 기존에 등록된 공지사항 정보를 수정할 수 있습니다.
등록 또는 수정 시 변경할 수 있는 항목은 모두 동일합니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_notice_03_ko_240105.jpg)

#### 1. 노출일
공지사항을 노출하고자 하는 기간을 설정합니다.

#### 2. 표시 시각
공지사항 내용을 표시할 때 실제 유저에게 보여질 날짜를 선택합니다.

#### 3. 말머리
공지사항의 말머리를 선택합니다.

#### 4. 상단 고정
공지사항을 상단에 고정하여 항상 노출될 수 있도록 합니다.

#### 5. 제목
공지사항의 제목을 입력합니다.

#### 6. 내용
공지사항에 대한 내용을 입력합니다.
Text Editor를 통해 원하는 형태로 답변을 입력할 수 있으며 해당형태 그대로 웹페이지에 노출됩니다.
> [참고]
> 앱 - 고객센터에서 설정한 지원 언어들은 모두 입력해야 등록이 가능합니다.

#### 7. 파일 첨부
해당 공지사항에 함께 노출할 파일을 첨부하여 올릴 수 있습니다.
10MB 이내의 파일을 최대 5개까지 첨부가능합니다.
첨부파일들은 공지사항에 함께 노출되며 클릭 시 다운로드가 가능합니다.

## Answer template

고객 문의 처리 시 반복입력을 자주 할 경우 템플릿을 사용하여 처리할 수 있도록 기능을 지원합니다.
또한, 고객이 문의 작성 시 필요한 정보들을 작성할 수 있도록 문의 유형별 템플릿을 사용할 수 있도록 기능을 지원합니다.


### Search Template
현재 등록된 템플릿 리스트를 표시하며, 우측 상단에 검색어를 입력하여 현재 등록된 템플릿을 검색할 수 있습니다.
![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_template_01_ko_240105.jpg)

**결과**
- **템플릿명**: 문의 처리시에 템플릿 항목에 노출되어 선택할 수 있는 템플릿명입니다.
- **수정자**: 답변 템플릿을 마지막으로 등록 또는 수정한 유저의 정보를 보여줍니다.
- **수정일**: 답변 템플릿이 마지막으로 등록 또는 수정된 날짜 정보를 보여줍니다.

### Register or Update Template
답변 템플릿을 새롭게 등록하거나 기존에 등록된 답변 템플릿 정보를 수정할 수 있습니다.
등록 또는 수정 시 변경할 수 있는 항목은 모두 동일합니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_template_02_ko_240105.jpg)

#### 1. 구분
- **문의 처리**: 고객 문의에 대한 기본 답변 메시지입니다.
- **문의 유형**: 유저 문의 입력에 기본으로 노출되는 메시지입니다.

#### 2. 템플릿명
문의 처리시에 템플릿 선택항목에 노출될 템플릿명을 입력합니다.
구분이 문의 유형인 경우는 문의 유형 관리에서 노출될 템플릿 명입니다.

#### 3. 내용
문의 처리시 템플릿이 선택되었을 때 채워질 내용을 입력합니다.
Text editor를 이용하여 자유롭게 입력이 가능하며 입력된 내용이 그대로 문의 처리시 템플릿을 선택하면 동일하게 적용됩니다.

## Email Config
문의 처리가 완료되었을 경우 유저에게 발송할 이메일 형식을 설정할 수 있습니다.
최초에 활성화 시켰을 경우 기본 템플릿이 제공되며 이후 Text editor를 통해 원하는 형태로 얼마든지 수정하실 수 있습니다.

테스트 발송 기능이 제공되며 해당 기능을 통해 현재 입력한 템플릿을 이용하여 실제 유저에게 어떤 형태로 전송되는지 미리 확인할 수 있습니다.

![gamebase_ban_01_201812](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_gamebase/ConsoleGuide/CustomerService/ko/gamebase_template_03_ko_240105.jpg)

> [참고]
> 발신 주소에 설정된 이메일이 SPF 레코드가 설정되지 않았을 경우에는 해당 메일이 스팸 처리 될 수 있습니다. 
> 그러므로 반드시 아래의 값을 DNS의 TXT레코드에 먼저 등록 후 발신주소에 설정해 주셔야 합니다.
> 추가 값 : v=spf1 include:_spfblocka.toast.com ~all
