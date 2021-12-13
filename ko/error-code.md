## Game > Gamebase > 오류 코드

## Client SDK

| Category        | Platform           | Error                                    | Error Code | Description                                    |
| --------------- | ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Common          | Android, UNITY<br/>IOS | NOT_INITIALIZED<br/>TCGB\_ERROR\_NOT\_INITIALIZED | 1          | Gamebase가 초기화되어 있지 않습니다.                 |
|                 | Android, UNITY<br/>IOS | NOT\_LOGGED\_IN<br/>TCGB\_ERROR\_NOT\_LOGGED\_IN | 2          | 로그인이 필요합니다.                              |
|                 | Android, UNITY<br/>IOS | INVALID_PARAMETER<br/>TCGB\_ERROR\_INVALID\_PARAMETER | 3          | 잘못된 파라미터입니다.                             |
|                 | Android, UNITY<br/>IOS | INVALID\_JSON\_FORMAT<br/>TCGB\_ERROR\_INVALID\_JSON\_FORMAT | 4          | JSON 형식 오류입니다.                           |
|                 | Android, UNITY<br/>IOS | USER_PERMISSION<br/>TCGB\_ERROR\_USER\_PERMISSION | 5          | 권한이 없습니다.                                |
|                 | Android, UNITY<br/>IOS | INVALID\_MEMBER<br/>TCGB\_ERROR\_INVALID\_MEMBER  | 6          | 잘못된 회원에 대한 요청입니다.                              |
|                 | Android, UNITY<br/>IOS | BANNED\_MEMBER<br/>TCGB\_ERROR\_BANNED\_MEMBER   | 7         | 제재된 회원입니다.                                |
|                 | Android, UNITY<br/>IOS | NOT_SUPPORTED<br/>TCGB\_ERROR\_NOT\_SUPPORTED | 10         | 지원하지 않는 기능입니다.                           |
|                 | UNITY<br/>IOS          | NOT\_SUPPORTED\_ANDROID<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_ANDROID | 11         | Android에서 지원하지 않는 기능입니다.                 |
|                 | UNITY<br/>IOS          | NOT\_SUPPORTED\_IOS<br/>TCGB\_ERROR\_NOT\_SUPPORTED\_IOS | 12         | iOS에서 지원하지 않는 기능입니다.                     |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_EDITOR            | 13         | Editor에서 지원하지 않는 기능입니다.                  |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_STANDALONE        | 14         | Standalone에서 지원하지 않는 기능입니다.              |
|                 | UNITY                  | NOT\_SUPPORTED\_UNITY\_WEBGL             | 15         | WebGL에서 지원하지 않는 기능입니다.                   |
|                 | Android                | ANDROID\_ACTIVITY\_DESTROYED             | 31         | Activity가 강제종료되었습니다.                       |
|                 | Android                | ANDROID\_ACTIVEAPP\_NOT\_CALLED          | 32         | activeApp API가 호출되지 않았습니다.                 |
|                 | IOS                    | IOS_GAMECENTER_DENIED                    | 51         | Gamecenter 로그인이 거부되었습니다.                 |
| Network(Socket) | Android, UNITY<br/>IOS | SOCKET\_RESPONSE\_TIMEOUT<br/>TCGB\_ERROR\_SOCKET\_RESPONSE\_TIMEOUT | 101        | 네트워크 상태가 불안정하여 응답이 없습니다.                 |
|                 | Android, UNITY<br/>IOS | SOCKET_ERROR<br/>TCGB\_ERROR\_SOCKET\_ERROR | 110        | 소켓 오류입니다.                                    |
|                 | Android, UNITY<br/>IOS | UNKNOWN_ERROR<br/>TCGB\_ERROR\_UNKNOWN\_ERROR | 999        | 소켓 알 수 없는 오류입니다.                             |
| Launching       | Android, UNITY<br/>IOS | LAUNCHING\_SERVER\_ERROR<br/>TCGB\_ERROR\_LAUNCHING\_SERVER\_ERROR | 2001       | 론칭 서버 오류입니다.                             |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_NOT\_EXIST\_CLIENT\_ID<br/>TCGB\_ERROR\_LAUNCHING\_NOT\_EXIST\_CLIENT\_ID | 2002       | 클라이언트 ID가 없습니다.                    |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_APP<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_APP | 2003       | 등록되지 않은 앱입니다.                         |
|                 | Android, UNITY<br/>IOS | LAUNCHING\_UNREGISTERED\_CLIENT<br/>TCGB\_ERROR\_LAUNCHING\_UNREGISTERED\_CLIENT | 2004       | 등록되지 않은 클라이언트(버전)입니다.            |
| Auth            | Android, UNITY<br/>IOS | AUTH\_USER\_CANCELED<br/>TCGB\_ERROR\_AUTH\_USER\_CANCELED | 3001       | 로그인이 취소되었습니다.                            |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_SUPPORTED\_PROVIDER<br/>TCGB\_ERROR\_AUTH\_NOT\_SUPPORTED\_PROVIDER | 3002       | 지원하지 않는 인증 방식입니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_NOT\_EXIST\_MEMBER<br/>TCGB\_ERROR\_AUTH\_NOT\_EXIST\_MEMBER | 3003       | 존재하지 않거나 탈퇴한 회원입니다.                      |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_INITIALIZATION\_ERROR | 3006       | 외부 인증 라이브러리 초기화에 실패하였습니다.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_AUTH\_EXTERNAL\_LIBRARY\_ERROR | 3009       | 외부 인증 라이브러리 오류입니다.                       |
|                 | Android, UNITY<br/>IOS | AUTH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_AUTH\_ALREADY\_IN\_PROGRESS\_ERROR | 3010       | 이전 인증 프로세스가 완료되지 않았습니다.                 |
| TransferAccount | Android, UNITY<br/>IOS | SAME\_REQUESTOR<br/>TCGB\_ERROR\_SAME\_REQUESTOR | 8 | 발급한 TransferAccount를 동일한 단말기에서 사용했습니다. |
|                 | Android, UNITY<br/>IOS | NOT\_GUEST\_OR\_HAS\_OTHERS<br/>TCGB\_ERROR\_NOT\_GUEST\_OR\_HAS\_OTHERS | 9 | 게스트가 아닌 계정에서 이전을 시도했거나, 계정에 게스트 이외의 IdP가 연동되어 있습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_EXPIRED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_EXPIRED | 3041 | TransferAccount의 유효기간이 만료됐습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_BLOCK<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_BLOCK | 3042 | 잘못된 TransferAccount를 여러번 입력하여 계정 이전 기능이 잠겼습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_ID | 3043 | TransferAccount의 Id가 유효하지 않습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_INVALID\_PASSWORD | 3044 | TransferAccount의 Password가 유효하지 않습니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_CONSOLE\_NO\_CONDITION | 3045 | TransferAccount 설정이 되어있지 않습니다. <br/> NHN Cloud Gamebase Console에서 먼저 설정해주세요. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_NOT\_EXIST<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_NOT\_EXIST | 3046 | TransferAccount가 존재하지 않습니다. TransferAccount를 먼저 발급받아주세요. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_EXIST\_ID | 3047 | TransferAccount가 이미 존재합니다. |
|                 | Android, UNITY<br/>IOS | AUTH\_TRANSFERACCOUNT\_ALREADY\_USED<br/>TCGB\_ERROR\_AUTH\_TRANSFERACCOUNT\_ALREADY\_USED | 3048 | TransferAccount가 이미 사용되었습니다. |
| Auth (Login)    | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_FAILED | 3101       | 토큰 로그인에 실패했습니다.                         |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_TOKEN\_INFO | 3102       | 토큰 정보가 유효하지 않습니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_TOKEN\_LOGIN\_INVALID\_LAST\_LOGGED\_IN\_IDP | 3103       | 최근에 로그인한 IdP 정보가 없습니다.                   |
| IDP Login       | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_FAILED<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_FAILED | 3201       | IdP 로그인에 실패했습니다.                        |
|                 | Android, UNITY<br/>IOS | AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_IDP\_LOGIN\_INVALID\_IDP\_INFO | 3202       | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
| Add Mapping     | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FAILED | 3301       | 매핑 추가에 실패했습니다.                          |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_MAPPED\_TO\_OTHER\_MEMBER | 3302       | 이미 다른 멤버에 매핑되어 있습니다.                      |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_ALREADY\_HAS\_SAME\_IDP | 3303       | 이미 같은 IdP에 매핑되어 있습니다.                     |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_INVALID\_IDP\_INFO | 3304       | IdP 정보가 유효하지 않습니다(Console에 해당 IdP 정보가 없습니다). |
|                 | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_CANNOT\_ADD\_GUEST\_IDP | 3305       | 게스트 IDP로는 AddMapping이 불가능합니다. |
| Add Mapping Forcibly | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_NOT\_EXIST\_KEY | 3311       | 강제 매핑 키(ForcingMappingKey)가 존재하지 않습니다. <br/>ForcingMappingTicket을 다시 한번 확인해주세요. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_ALREADY\_USED\_KEY | 3312       | 강제 매핑 키(ForcingMappingKey)가 이미 사용되었습니다. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_EXPIRED\_KEY | 3313       | 강제 매핑 키(ForcingMappingKey)의 유효기간이 만료되었습니다. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_IDP | 3314       | 강제 매핑 키(ForcingMappingKey)가 다른 IdP에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP에 강제 매핑을 시도 하는데 사용됩니다. |
|                      | Android, UNITY<br/>IOS | AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY<br/>TCGB\_ERROR\_AUTH\_ADD\_MAPPING\_FORCIBLY\_DIFFERENT\_AUTHKEY | 3315       | 강제 매핑 키(ForcingMappingKey)가 다른 계정에 사용되었습니다. <br/>발급받은 ForcingMappingKey는 같은 IdP 및 계정에 강제 매핑을 시도 하는데 사용됩니다. |
| Remove Mapping  | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_FAILED<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_FAILED | 3401       | 매핑 삭제에 실패했습니다.                          |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LAST\_MAPPED\_IDP | 3402       | 마지막에 매핑된 IdP는 삭제할 수 없습니다.                |
|                 | Android, UNITY<br/>IOS | AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP<br/>TCGB\_ERROR\_AUTH\_REMOVE\_MAPPING\_LOGGED\_IN\_IDP | 3403       | 현재 로그인되어 있는 IdP입니다.                      |
| Logout          | Android, UNITY<br/>IOS | AUTH\_LOGOUT\_FAILED<br/>TCGB\_ERROR\_AUTH\_LOGOUT\_FAILED | 3501       | 로그아웃에 실패했습니다.                           |
| Withdrawal      | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_FAILED<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_FAILED | 3601       | 탈퇴에 실패했습니다.                             |
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_ALREADY\_TEMPORARY\_WITHDRAW | 3602   | 이미 임시 탈퇴를 요청한 유저 입니다.                    |
|                 | Android, UNITY<br/>IOS | AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW<br/>TCGB\_ERROR\_AUTH\_WITHDRAW\_NOT\_TEMPORARY\_WITHDRAW | 3603       | 임시 탈퇴를 요청한 유저가 아닙니다.                     |
| Not Playable    | Android, UNITY<br/>IOS | AUTH\_NOT\_PLAYABLE<br/>TCGB\_ERROR\_AUTH\_NOT\_PLAYABLE | 3701       | 플레이할 수 없는 상태입니다(점검 또는 서비스 종료 등).        |
| Auth(Unknown)   | Android, UNITY<br/>IOS | AUTH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_AUTH\_UNKNOWN\_ERROR | 3999       | 알 수 없는 오류입니다(정의되지 않은 오류).           |
| Purchase        | Android, UNITY<br/>IOS | PURCHASE\_NOT\_INITIALIZED<br/>TCGB\_ERROR\_PURCHASE\_NOT\_INITIALIZED | 4001       | Gamebase PurchaseAdapter가 초기화되지 않았습니다.   |
|                 | Android, UNITY<br/>IOS | PURCHASE\_USER\_CANCELED<br/>TCGB\_ERROR\_PURCHASE\_USER\_CANCELED | 4002       | 구매가 취소되었습니다.                             |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING<br/>TCGB\_ERROR\_PURCHASE\_NOT\_FINISHED\_PREVIOUS\_PURCHASING | 4003       | 이전 구매가 완료되지 않았습니다.                       |
|                 | UNITY                  | PURCHASE\_NOT\_ENOUGH\_CASH                                        | 4004       | 해당 스토어의 캐시가 부족하여 결제할 수 없습니다.(Unity Only) |
|                 | Android, UNITY<br/>IOS | PURCHASE\_INACTIVE\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_INACTIVE\_PRODUCT\_ID | 4005       | 해당 상품이 활성화 상태가 아닙니다.  |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_EXIST\_PRODUCT\_ID<br/>TCGB\_ERROR\_PURCHASE\_NOT\_EXIST\_PRODUCT\_ID | 4006       | 존재하지 않는 GamebaseProductID 로 결제를 요청하였습니다. |
|                 | Android, UNITY<br/>IOS | PURCHASE\_LIMIT\_EXCEEDED<br/>TCGB\_ERROR\_PURCHASE\_LIMIT\_EXCEEDED | 4007       | 월 구매 한도를 초과했습니다. |
|                 | Android, UNITY<br/>IOS | PURCHASE\_NOT\_SUPPORTED\_MARKET<br/>TCGB\_ERROR\_PURCHASE\_NOT\_SUPPORTED\_MARKET | 4010       | 지원하지 않는 스토어입니다.                          |
|                 | Android, UNITY<br/>IOS | PURCHASE\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_EXTERNAL\_LIBRARY\_ERROR | 4201       | 외부 IAP 라이브러리 오류입니다.                      |
|                 | Android, UNITY<br/>IOS | PURCHASE\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PURCHASE\_UNKNOWN\_ERROR | 4999       | 알 수 없는 구매 오류입니다.                           |
| Push            | Android, UNITY<br/>IOS | PUSH\_EXTERNAL\_LIBRARY\_ERROR<br/>TCGB\_ERROR\_PUSH\_EXTERNAL\_LIBRARY\_ERROR | 5101       | 외부 라이브러리 오류입니다.                          |
|                 | Android, UNITY<br/>IOS | PUSH\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_PUSH\_ALREADY\_IN\_PROGRESS\_ERROR | 5102       | 이전 푸시 API 호출이 완료되지 않았습니다.              |
|                 | Android, UNITY<br/>IOS | PUSH\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_PUSH\_UNKNOWN\_ERROR | 5999       | 알 수 없는 푸시 오류입니다(정의되지 않은 푸시 오류).      |
| UI              | Android, UNITY<br/>IOS | UI\_IMAGE\_NOTICE\_TIMEOUT<br/>TCGB\_ERROR\_UI\_IMAGE\_NOTICE\_TIMEOUT | 6901       | 이미지 공지 표시 중 타임아웃이 발생했습니다.            |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_INVALID\_URL<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_INVALID\_URL | 6911       | 고객센터 웹뷰 URL 생성에 실패했습니다.            |
|                 | Android, UNITY<br/>IOS | UI\_CONTACT\_FAIL\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET<br/>TCGB\_ERROR\_UI\_CONTACT\_FAIL\_ISSUE\_SHORT\_TERM\_TICKET | 6912       | 사용자 식별을 위한 임시 티켓 발급에 실패했습니다.            |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_IN\_CONSOLE | 6921       | 약관 정보가 콘솔에 등록되어 있지 않습니다. |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY<br/>TCGB\_ERROR\_UI\_TERMS\_NOT\_EXIST\_FOR\_DEVICE\_COUNTRY | 6922       | 단말기 국가코드에 맞는 약관 정보가 콘솔에 등록되어 있지 않습니다. |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_UNREGISTERED\_SEQ<br/>TCGB\_ERROR\_UI\_TERMS\_UNREGISTERED\_SEQ | 6923       | 등록되지 않은 약관 Seq 값입니다.            |
|                 | Android, UNITY<br/>IOS | UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR<br/>TCGB\_ERROR\_UI\_TERMS\_ALREADY\_IN\_PROGRESS\_ERROR | 6924       | 이전에 호출된 Terms API 가 아직 완료되지 않았습니다.<br/>잠시 후 다시 시도하세요. |
|                 | Android, UNITY         | UI\_TERMS\_ANDROID\_DUPLICATED\_VIEW | 6925       | 약관 웹뷰가 아직 종료되지 않았는데 다시 호출되었습니다. |
|                 | Android, UNITY<br/>IOS | UI\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_UI\_UNKNOWN\_ERROR | 6999       | 알 수 없는 오류입니다(정의되지 않은 오류).            |
| WebView         | Android, UNITY<br/>IOS | WEBVIEW\_INVALID\_URL<br/>TCGB\_ERROR\_WEBVIEW\_INVALID\_URL           | 7001       | 잘못된 URL 입니다.            |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_TIMEOUT<br/>TCGB\_ERROR\_WEBVIEW\_TIMEOUT                     | 7002       | 웹뷰 표시중 타임아웃이 발생했습니다.            |
|                 | Android, UNITY<br/>IOS | WEBVIEW\_HTTP\_ERROR<br/>TCGB\_ERROR\_WEBVIEW\_HTTP\_ERROR 	        | 7003       | HTTP 에러로 웹뷰 표시가 실패하였습니다.            |
|                 | Android, UNITY         | WEBVIEW\_OPENED\_NEW\_BROWSER\_BEFORE\_CLOSE                           | 7004       | Browser 형태의 웹뷰를 종료하기 전에 새로운 웹뷰를 표시하였습니다. |
|                 | UNITY                  | WEBVIEW\_UNKNOWN\_ERROR 								| 7999       | 웹뷰 호출시 알 수 없는 오류가 발생했습니다.(정의되지 않은 오류).            |
| Server          | Android, UNITY<br/>IOS | SERVER\_INTERNAL\_ERROR<br/>TCGB\_ERROR\_SERVER\_INTERNAL\_ERROR | 8001       | 서버 내부 오류                                 |
|                 | Android, UNITY<br/>IOS | SERVER\_REMOTE\_SYSTEM\_ERROR<br/>TCGB\_ERROR\_SERVER\_REMOTE\_SYSTEM\_ERROR | 8002       | 서버에서 외부 연동 중 오류가 발생했습니다.                        |
|                 | Android, UNITY<br/>IOS | SERVER\_UNKNOWN\_ERROR<br/>TCGB\_ERROR\_SERVER\_UNKNOWN\_ERROR | 8999       | 서버에서 알 수 없는 오류가 발생했습니다.                           |

<br/>
<br/>

## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | 잘못된 파라미터 유형으로 API 호출 <br/>- 예) 파라미터는 int형으로 선언돼 있는데, string형 데이터로 API가 호출됨 |
|         | -4000002<br/>-4000004<br>-4000005 | 필수 파라미터가 생략되었거나 값이 없을 때, 부적절한 값으로 호출될 때 <br> |
|         | -4000003              | Request body에 정의되지 않은 값이 전달된 경우 |
|         | -4000007              | 더 이상 지원되지 않는 API 버전을 호출 |
|         | -4000008              | 파라미터 길이가 초과됨 |
|         | -4010001              | 잘못된 앱 ID가 호출됨 |
|         | -4010002              | 잘못된 앱 키가 호출됨 |
|         | -4010003              | 인증되지 않은 클라이언트에서 인증이 필요한 API를 호출한 경우 |
|         | -4010004              | 잘못된 비밀 키(secret key)가 호출됨 |
|         | -4060001              | HTTP 헤더에 Content-Type을 잘못 설정 |
|         | -4060002              | Depreated API 버전을 호출 |
|         | -4060003              | 잘못된 API 버전을 호출 했거나, 잘못된 URI로 호출한 경우 |
|         | -4090001 ~ 4          | 내부 DB 관련 오류 |
|         | -4150001              | 잘못된 형식의 JSON 데이터 전달 |
|         | -5000001 ~ 15         | 내부 시스템 오류 |
| Gateway | -4010202              | 잘못된 앱 ID가 호출됨 |
|         | -4010203              | 유효하지 않은 액세스 토큰 |
|         | -4010204              | 이용 정지/탈퇴/계정 유실 등 유효하지 않은 사용자 |
|         | -4010208              | Gamebase Access Token 만료 혹은 IdP Access Token 만료 |
|         | -4040201              | 호출한 API에 대한 NHN Cloud 서비스가 활성화되어 있지 않을 때 <br/>- 예) Leaderboard 서비스를 사용하지 않는 상태에서 Gamebase를 통해 Leaderboard API를 호출할 때 <br/>혹은 Gamebase 자체가 활성화되어 있지 않을 때 |
|         | -4040202              | 정의되어 있지 않은 API를 호출한 경우 |
|         | -4120201              | 일부 제공되는 SANDBOX 시스템에서 실제 환경 서버 주소로 API 호출 (혹은 반대로 호출) <br/>- 해당 오류 발생시 서버 호출 주소 확인이 필요함 |
|         | -5000201 ~ 8          | Gateway 내부 시스템 오류 |
| Launching | -4040303            | 등록되지 않은 클라이언트 버전으로 초기화 요청일 때<br>콘솔에서 현재 등록된 클라이언트 버전 및 스토어 코드 확인이 필요함 |
| Member  | -4000402              | 사용자 ID를 잘못 입력했을 때 |
|         | -4000403              | 잘못된 회원을 요청했을 때 |
|         | -4000404              | 잘못된 Auth를 요청했을 때 |
|         | -4000409              | 단말기 이전을 위해 발급받은 TransferAccount 정보를 동일한 단말기에서 사용 |
|         | -4040401              | 존재하지 않거나 탈퇴한 회원을 요청할 때 |
|         | -4040403              | 존재하지 않는 TransferAccount에 대한 요청일 때 |
|         | -4100402              | 이미 사용된 TransferAccount 요청일 때 |
|         | -4100403              | 유효 기간이 만료된 TransferAccount 의 요청일 때 |
|         | -4100401              | 이미 탈퇴한 회원을 요청할 때 |
|         | -4220401              | 사용자 Auth 데이터가 정상적이지 않을 때 |
| IdP     | -4000901              | Block 된 게임 유저가 TransferAccount 검증 요청한 경우 |
|         | -4000920              | 내부에서 PASSWORD 암호화시 오류 발생. 계속 발생시 고객센터를 통해 문의 필요 |
|         | -4000921 ~ 2          | TransferAccount 발급 시 내부 오류, 계속 발생 시 고객 센터로 문의 |
|         | -4000923              | 매뉴얼(MANUAL) 방식으로 비밀번호를 변경하려고 할 때, 현재 비밀번호와 동일한 문자열 입력 |
|         | -4000924              | 내부 오류. 계속 발생시 고객센터를 통해 문의 필요 |
|         | -4000925              | 매뉴얼(MANUAL) 방식으로 ID를 변경하려고 할 때, 현재 사용 중인 ID를 입력 |
|         | -4000927              | TransferAccount 유효성 검증 시, 비밀번호가 잘못되었을 때 |
|         | -4040920              | TransferAccount 유효성 검증 시, ID가 존재하지 않을 때 |
|         | -5110920              | TransferAccount 발급 시스템 오류, 계속 발생 시 고객 센터로 문의 |
| Coupon  | -100002<br>-4000205   | 유효하지 않은 쿠폰 코드 혹은 요청<br>- 유저가 잘못된 쿠폰 코드로 쿠폰 소비를 요청 함<br>- 콘솔에서 등록된 쿠폰의 상태가 현재 비활성<br>- 콘솔에서 스토어 지정 후, 스토어 코드 생략 혹은 잘못된 스토어 코드로 호출 |
|         | -100004               | 발급된 쿠폰 코드에 대해, 운영자 요청에 의해 수동으로 DB에서 직접 만료 처리한 쿠폰 코드 |
|         | -100005               | 이미 사용된 쿠폰 |
|         | -100007 ~ 8           | 쿠폰 사용 기간이 아닌 경우 |
|         | -100011               | 유저별 사용 가능한 쿠폰 제한 수량을 초과한 경우 |
|         | -999999               | 쿠폰 시스템 내부 오류, 계속 발생 시 고객 센터로 문의 |
| IAP     | 5000                  | 소비 실패 |
|         | 5018                  | 이미 소비를 수행한 결제 건 |
|         | 1100                  | 필수 파라미터 생략 및 잘못된 파라미터 전달 |
|         | 5032                  | 잘못된 형식의 JSON 데이터 전달 |
|         | 9999                  | 알 수 없는 오류 |
|         | -4001002              | 유효하지 않은 Gamebase 상품 |
